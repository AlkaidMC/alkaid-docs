<h1 align="center">Alkaid Redis 模块</h1>
<h6 align="center">We also provide documentation in English. <a href="../#/">Click here to go</a></h6>

## 使用模块

引入 Redis 模块（注意替换版本号）

**Maven**

```xml
<dependency>
  <groupId>com.alkaidmc.alkaid</groupId>
  <artifactId>alkaid-redis</artifactId>
  <version>{{project.version}}</version>
</dependency>
```

**Gradle**

```groovy
implementation "com.alkaidmc.alkaid:alkaid-redis:{{project.version}}"
```

**Gradle Kotlin**

```kotlin
implementation("com.alkaidmc.alkaid:alkaid-redis:{{project.version}}")
```

**创建模块引导类**

```java
new AlkaidRedis();
```

| 入口类方法 | 返回值类型                                                   | 功能                  |
| ---------- | ------------------------------------------------------------ | --------------------- |
| single()   | [SingleRedisConnector](https://github.com/AlkaidMC/alkaid/blob/main/alkaid-redis/src/main/java/com/alkaidmc/alkaid/redis/SingleRedisConnector.java) | 单服务端 Redis 连接器 |



## 单端 Redis 连接器

**使用模块引导类创建**

```java
AlkaidRedis redis = new AlkaidRedis();
SingleRedisConnector connector = redis.single();
```

**可配置变量**

| 变量名  | 参数类型        | 默认值                 | 传入方法 | 功能                                                     |
| ------- | --------------- | ---------------------- | -------- | -------------------------------------------------------- |
| host    | String          | "127.0.0.1"            | apply    | 目标主机 IP                                              |
| port    | int             | 6379                   | apply    | 目标主机端口                                             |
| auth    | String          | null                   | apply    | Redis 验证密钥 为 null 时不进行验证                      |
| connect | int             | 32                     | apply    | 连接池大小（允许的连接数量最大值）                       |
| timeout | int             | 1000                   | apply    | 每个连接的超时时间 单位毫秒（ms）                        |
| sleep   | long            | 1000                   | apply    | 消息订阅机制的检测时长 单位毫秒（ms）                    |
| config  | JedisPoolConfig | new JedisPoolConfig(); | apply    | 连接池配置文件 设置 connect 将覆盖该配置的最大连接数设置 |

**端点操作**

| 方法名     | 返回值类型                                                   | 功能                                        |
| ---------- | ------------------------------------------------------------ | ------------------------------------------- |
| connect    | [SingleRedisConnector](https://github.com/AlkaidMC/alkaid/blob/main/alkaid-redis/src/main/java/com/alkaidmc/alkaid/redis/SingleRedisConnector.java) | 使用配置进行 Redis Pool 创建 返回连接器自己 |
| disconnect | void                                                         | 断开 Redis Pool 连接                        |
| connection | [SingleRedisConnection](https://github.com/AlkaidMC/alkaid/blob/main/alkaid-redis/src/main/java/com/alkaidmc/alkaid/redis/SingleRedisConnection.java) | 获取一个单端连接                            |

**完整使用示例**

```java
SingleRedisConnector connector = new AlkaidRedis().single()
                .host("localhost")
                .port(6379)
                .auth(null)
                .connect(32)
                .timeout(1000)
                .sleep(1000)
                .config(new JedisPoolConfig())
    			// 立刻连接 Redis Pool
    			.connect();
```



## 使用单端连接器获取可用连接

使用端点操作 `connection` 方法将得到一个 [SingleRedisConnection](https://github.com/AlkaidMC/alkaid/blob/main/alkaid-redis/src/main/java/com/alkaidmc/alkaid/redis/SingleRedisConnection.java) 类型可用连接

**可用接口**

| 方法名      | 参数类型                   | 返回值类型 | 功能                                                   |
| ----------- | -------------------------- | ---------- | ------------------------------------------------------ |
| set         | String, String             | void       | 使用键与值参数设置一条数据                             |
| get         | String                     | String     | 使用键获取 Redis 中的数据                              |
| del         | String                     | void       | 使用键删除 Redis 中的设置                              |
| expire      | String, int                | void       | 设置已存在的键值数据的过期时间 单位为秒（s）           |
| exists      | String                     | boolean    | 使用键判断是否存在于 Redis                             |
| publish     | String, String             | void       | 提供通道、消息发布一条消息                             |
| subscribe   | String, Consumer\<String\> | void       | 提供通道和消息处理器处理消息，只能监听指定通道中的消息 |
| unsubscribe | void                       | void       | 取消订阅                                               |

**完整使用案例**

```java
public void single() {
        SingleRedisConnection connection = new AlkaidRedis().single()
                .host("127.0.0.1")
                .port(6379)
                .connect()
                .connection();

        // 写入数据
        IntStream.range(114514, 114517).forEach(count -> {
            connection.set(String.valueOf(count), "191810");
        });
        // 读取数据
        String data = connection.get("114514");
        assertEquals("191810", data);

        // 删除数据
        connection.del("114514");
        // 读取数据
        data = connection.get("114514");
        assertNull(data);

        // 定时删除数据
        connection.set("114514", "191810");
        connection.expire("114514", 3);
        // 读取数据
        data = connection.get("114514");
        assertEquals("191810", data);

        // 等待五秒
        try {
            Thread.sleep(5000);
        } catch (InterruptedException e) {
            e.printStackTrace();
        }

        // 读取数据
        data = connection.get("114514");
        assertNull(data);

        // 订阅消息
        connection.subscribe("alkaid", (message) -> {
            assertEquals("test", message);
        });
        // 发布消息
        connection.publish("alkaid", "test");

        // 等待五秒
        try {
            Thread.sleep(5000);
        } catch (InterruptedException e) {
            e.printStackTrace();
        }
    }
```

摘自 [Alkaid Redis 模块 GitHub Actions 集成测试](https://github.com/AlkaidMC/alkaid/blob/main/alkaid-redis/src/test/java/com/alkaidmc/alkaid/redis/AlkaidRedisTest.java) 

 `assertEquals(Object, Object)` 是比较两个值是否相等 `assertNull(Object)` 是判断值是否为 `null`