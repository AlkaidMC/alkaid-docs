<h1 align="center">Alkaid Bukkit 模块</h1>
<h6 align="center">We also provide documentation in English. <a href="../#/">Click here to go</a></h6>

## 使用模块

引入 Bukkit 模块（注意替换版本号）

**Maven**

```xml
<dependency>
  <groupId>com.alkaidmc.alkaid</groupId>
  <artifactId>alkaid-redis</artifactId>
  <version>{{alkaid.version}}</version>
</dependency>
```

**Gradle**

```groovy
implementation "com.alkaidmc.alkaid:alkaid-redis:{{alkaid.version}}"
```

**Gradle Kotlin**

```kotlin
implementation("com.alkaidmc.alkaid:alkaid-redis:{{alkaid.version}}")
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