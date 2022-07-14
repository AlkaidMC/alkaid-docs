<h1 align="center">Alkaid Bukkit 模块</h1>
<h6 align="center">We also provide documentation in English. <a href="../#/">Click here to go</a></h6>

## 使用模块

引入 Bukkit 模块（注意替换版本号）

**Maven**

```xml
<dependency>
  <groupId>com.alkaidmc.alkaid</groupId>
  <artifactId>alkaid-bukkit</artifactId>
  <version>{{alkaid.version}}</version>
</dependency>
```

**Gradle**

```groovy
implementation "com.alkaidmc.alkaid:alkaid-bukkit:{{alkaid.version}}"
```

**Gradle Kotlin**

```kotlin
implementation("com.alkaidmc.alkaid:alkaid-bukkit:{{alkaid.version}}")
```

**创建模块引导类**

Bukkit 模块包含五个分类，它们分别有各自的模块引导类

1. 指令 Command

   ```java
   new AlkaidCommand(Plugin plugin);
   ```

2. 配置文件与序列化 Config

   ```java
   new AlkaidJsonConfiguration();
   ```

3. 事件 Event

   ```java
   new AlkaidEvent(Plugin plugin);
   ```

4. 服务端、依赖与 NMS Server

   ```java
   new AlkaidServer(Plugin plugin);
   ```

5. 任务 Task

   ```java
   new AlkaidTask(Plugin plugin);
   ```

## 指令 Command

| 入口类方法 | 返回值类型                                                   | 功能               |
| ---------- | ------------------------------------------------------------ | ------------------ |
| simple()   | [SimpleCommandRegister](https://github.com/AlkaidMC/alkaid/blob/main/alkaid-bukkit/src/main/java/com/alkaidmc/alkaid/bukkit/command/SimpleCommandRegister.java) | 简单指令注册器     |
| regex()    | [RegexCommandRegister](https://github.com/AlkaidMC/alkaid/blob/main/alkaid-bukkit/src/main/java/com/alkaidmc/alkaid/bukkit/command/RegexCommandRegister.java) | 正则匹配指令注册器 |
| parse()    | [ParseCommandRegister](https://github.com/AlkaidMC/alkaid/blob/main/alkaid-bukkit/src/main/java/com/alkaidmc/alkaid/bukkit/command/ParseCommandRegister.java) | 解析树指令注册器   |

## 配置文件与序列化 Config

### AlkaidJsonConfiguration

通过 AlkaidJsonConfiguration 从插件文件中释放默认 json 配置文件：

```java
public class Test extends JavaPlugin {
    @Data
    static class TestConfig {
        String test;
    }
    @Override
    public void onEnable() {
        TestConfig config = new AlkaidJsonConfiguration().load(
                this,
                new Gson(),
                "config.json",
                "config.json",
                TestConfig.class
        );
    }
}
```

对应的 config.json 文件放置于 resources 根目录下：

```
{"test": "test"}
```

其中的 `<T> T load(Plugin plugin, Gson gson, String resource, String path, Class<T> type)` 方法参数分别为 ：

`plugin` 存放 json 资源的插件实例

`gson` 用于解析 json 的 Gson 实例，可以使用 AlkaidGsonBuilder 获取带有序列化适配器的 Gson 实例

`resource` 存放在 resources 目录下的位置

`path` 为保存文件的位置，相对于 plugin 实例的 `getDataFolder()` 目录

`type` 为需要将 json 映射到的数据实体类 （POJO）

**AlkaidJsonConfiguration 不会自动创建缺失的文件夹，请先确保数据文件的上级目录存在**

### AlkaidGsonBuilder

为了使 Gson 能解析 Bukkit 的对象，Alkaid 提供了三个适配器，分别是 [ItemStackGsonAdapter](https://github.com/AlkaidMC/alkaid/blob/main/alkaid-bukkit/src/main/java/com/alkaidmc/alkaid/bukkit/config/gson/ItemStackGsonAdapter.java)、[LocationGsonAdapter](https://github.com/AlkaidMC/alkaid/blob/main/alkaid-bukkit/src/main/java/com/alkaidmc/alkaid/bukkit/config/gson/LocationGsonAdapter.java) 和 [PlayerGsonAdapter](https://github.com/AlkaidMC/alkaid/blob/main/alkaid-bukkit/src/main/java/com/alkaidmc/alkaid/bukkit/config/gson/PlayerGsonAdapter.java) 它们分别对应 [ItemStack](https://hub.spigotmc.org/javadocs/bukkit/org/bukkit/inventory/ItemStack.html) [Location](https://hub.spigotmc.org/javadocs/bukkit/org/bukkit/Location.html) 和 [Player](https://hub.spigotmc.org/javadocs/bukkit/org/bukkit/entity/Player.html) 几个类型。

使用位于 `AlkaidGsonBuilder` 的静态方法 `gson()` 可以获得一个包含了以上三个适配器的 Gson 解析器。

这是它的实现方法：

```java
public static Gson gson() {
    return Optional.ofNullable(gson).orElseGet(() -> {
        gson = new GsonBuilder()
                .enableComplexMapKeySerialization()
                .serializeNulls()
                .setPrettyPrinting()
                .registerTypeAdapter(ItemStack.class, new ItemStackGsonAdapter())
                .registerTypeAdapter(Player.class, new PlayerGsonAdapter())
                .registerTypeAdapter(Location.class, new LocationGsonAdapter())
                .create();
        return gson;
    });
}
```

使用它，只需要一行：

```java
Gson gson = AlkaidGsonBuilder.gson();
```

## 事件 Event

| 入口类方法  | 返回值类型                                                   | 功能             |
| ----------- | ------------------------------------------------------------ | ---------------- |
| simple()    | [SimpleEventFactory](https://github.com/AlkaidMC/alkaid/blob/main/alkaid-bukkit/src/main/java/com/alkaidmc/alkaid/bukkit/event/AlkaidEvent.java) | 简单的事件注册器 |
| predicate() | [PredicateEventFactory](https://github.com/AlkaidMC/alkaid/blob/main/alkaid-bukkit/src/main/java/com/alkaidmc/alkaid/bukkit/event/AlkaidEvent.java) | 谓词事件注册器   |
| section()   | [SectionEventFactory](https://github.com/AlkaidMC/alkaid/blob/main/alkaid-bukkit/src/main/java/com/alkaidmc/alkaid/bukkit/event/AlkaidEvent.java) | 事件段落注册器   |
| count()     | [CountEventFactory](https://github.com/AlkaidMC/alkaid/blob/main/alkaid-bukkit/src/main/java/com/alkaidmc/alkaid/bukkit/event/AlkaidEvent.java) | 计数器事件注册器 |
| chain()     | [ChainEventRegister](https://github.com/AlkaidMC/alkaid/blob/main/alkaid-bukkit/src/main/java/com/alkaidmc/alkaid/bukkit/event/ChainEventRegister.java) | 事件链注册器     |

## 服务端、依赖与 NMS Server

| 入口类方法  | 返回值类型                                                   | 功能                       |
| ----------- | ------------------------------------------------------------ | -------------------------- |
| dependent() | [DependManagerFactory](https://github.com/AlkaidMC/alkaid/blob/main/alkaid-bukkit/src/main/java/com/alkaidmc/alkaid/bukkit/server/DependManager.java) | 用于获取插件的依赖插件实例 |
| depend()    | [DependOptional](https://github.com/AlkaidMC/alkaid/blob/main/alkaid-bukkit/src/main/java/com/alkaidmc/alkaid/bukkit/server/DependOptional.java) | 检测依赖是否存在           |

## 任务 Task

| 入口类方法 | 返回值类型                                                   | 功能           |
| ---------- | ------------------------------------------------------------ | -------------- |
| simple()   | [SimpleTaskRegister](https://github.com/AlkaidMC/alkaid/blob/main/alkaid-bukkit/src/main/java/com/alkaidmc/alkaid/bukkit/task/SimpleTaskRegister.java) | 简单任务注册器 |