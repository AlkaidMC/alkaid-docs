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
   new AlkaidCommand();
   ```

2. 配置文件与序列化 Config

   ```java
   new AlkaidJsonConfiguration();
   ```

3. 事件 Event

   ```java
   new AlkaidEvent();
   ```

4. 服务端、依赖与 NMS Server

   ```java
   new AlkaidServer();
   ```

5. 任务 Task

   ```java
   new AlkaidTask();
   ```

## 指令 Command



## 配置文件与序列化 Config

## 事件 Event

## 服务端、依赖与 NMS Server

## 任务 Task