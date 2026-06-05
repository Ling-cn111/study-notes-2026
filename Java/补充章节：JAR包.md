## 补充章节：JAR包

### ① 核心概念

**一句话总结：**
> JAR（Java Archive）是把多个`.class`文件、配置文件和资源打包成一个压缩文件（类似`.zip`），用于分发、部署或作为库使用。

**生活类比：**
- **无JAR包** = 把100张零散的文件交给别人（容易丢，不好管理）
- **有JAR包** = 把所有文件装进一个信封寄出去（整洁、完整、易传输）

**用C的视角理解：**
相当于C语言的静态库（`.lib`/`.a`）或动态库（`.dll`/`.so`），但Java的JAR是跨平台的，且本质是ZIP压缩格式。

---

### ② 为什么需要JAR包？

**解决什么痛点？**

```java
// 问题1：项目有几十个.class文件，发给别人很麻烦
// com/
// └── network/
//     └── chat/
//         ├── client/
//         │   ├── ChatClient.class
//         │   └── LoginDialog.class
//         ├── server/
//         │   └── ChatServer.class
//         └── common/
//             └── Message.class
// 要发一堆文件，容易遗漏
```

```java
// 问题2：复用别人的代码
// 想用第三方库（如MySQL驱动），难道要把几十个.class文件手动复制到项目里？
```

**核心作用：**
1. **打包分发**：一个JAR文件包含整个程序
2. **依赖管理**：第三方库以JAR形式提供
3. **压缩节省空间**：ZIP压缩减小体积
4. **安全性**：可对JAR进行数字签名

---

### ③ JAR包的基本操作

#### 创建JAR包

```bash
# 1. 先编译Java文件
javac -d . ChatClient.java Message.java

# 2. 查看生成的.class文件
# com/network/chat/client/ChatClient.class
# com/network/chat/common/Message.class

# 3. 打包成JAR
jar cvf chat-client.jar com/

# 参数说明：
# c - create（创建）
# v - verbose（显示详细信息）
# f - file（指定文件名）
# 后面跟着：jar包名  要打包的内容
```

#### 查看JAR包内容

```bash
# 列出JAR包中的文件
jar tf chat-client.jar

# 输出示例：
# META-INF/MANIFEST.MF
# com/network/chat/client/ChatClient.class
# com/network/chat/common/Message.class
```

#### 解压JAR包

```bash
# 解压到当前目录
jar xf chat-client.jar

# x - extract（解压）
```

#### 运行JAR包

```bash
# 方式1：直接运行（需要指定主类）
java -jar chat-client.jar

# 方式2：手动指定主类（不依赖MANIFEST）
java -cp chat-client.jar com.network.chat.client.ChatClient

# -cp 或 -classpath：指定类路径
```

---

### ④ MANIFEST.MF（清单文件）

**核心概念：**
> MANIFEST.MF是JAR包中的特殊文件，记录JAR的元信息，如主类、版本、类路径等。

**为什么需要它？**
让JAR包可以**双击运行**或`java -jar`直接启动，不需要手动指定主类。

```bash
# MANIFEST.MF内容示例
Manifest-Version: 1.0
Main-Class: com.network.chat.client.ChatClient
Class-Path: lib/mysql-connector.jar lib/log4j.jar
Created-By: 1.8.0_301 (Oracle Corporation)
```

**创建带MANIFEST的JAR包：**

```bash
# 1. 创建MANIFEST.MF文件（注意：最后必须有一个空行）
echo "Main-Class: com.network.chat.client.ChatClient" > MANIFEST.MF
echo "" >> MANIFEST.MF

# 2. 打包时指定MANIFEST
jar cvfm chat-client.jar MANIFEST.MF com/

# m - manifest（指定清单文件）
```

---

### ⑤ 在项目中使用第三方JAR

```bash
# 项目目录结构
my-project/
├── lib/
│   └── mysql-connector-java-8.0.31.jar  # 第三方库
├── src/
│   └── com/
│       └── myapp/
│           └── DatabaseHelper.java
└── classes/                              # 编译输出

# 编译时指定classpath（告诉编译器去哪找第三方类）
javac -cp "lib/mysql-connector-java-8.0.31.jar" -d classes src/com/myapp/DatabaseHelper.java

# 运行时指定classpath
java -cp "classes;lib/mysql-connector-java-8.0.31.jar" com.myapp.DatabaseHelper
# Windows用分号，Linux/Mac用冒号：java -cp "classes:lib/mysql-connector.jar" ...
```

---

### ⑥ 在代码中使用JAR中的类

```java
// 只要JAR在classpath中，直接import即可使用
import com.mysql.cj.jdbc.Driver;
import org.apache.commons.codec.digest.DigestUtils;

public class DatabaseHelper {
    public static void main(String[] args) {
        // 使用第三方库的类
        String md5 = DigestUtils.md5Hex("hello");
        System.out.println(md5);
    }
}
```

---

### ⑦ 可执行JAR vs 普通JAR

| 类型 | 特点 | 如何运行 |
|------|------|----------|
| **普通JAR** | 只有.class文件，没有指定入口 | `java -cp jar包名 主类全名` |
| **可执行JAR** | MANIFEST中指定了Main-Class | `java -jar jar包名` 或双击 |

```bash
# 普通JAR运行
java -cp my-library.jar com.example.Main

# 可执行JAR运行
java -jar my-app.jar
```

---

### ⑧ IDEA/Eclipse中的JAR导出

**IDEA导出JAR步骤：**
1. File → Project Structure → Artifacts
2. 点击 + → JAR → From modules with dependencies
3. 选择Main Class
4. Build → Build Artifacts → Build

**Eclipse导出JAR步骤：**
1. 右键项目 → Export
2. Java → JAR file 或 Runnable JAR file
3. 选择导出位置和Main Class
4. Finish

---

### ⑨ 常见JAR命令速查

| 命令 | 说明 |
|------|------|
| `jar cvf my.jar com/` | 创建JAR包 |
| `jar cvfm my.jar MANIFEST.MF com/` | 创建带清单文件的JAR |
| `jar tf my.jar` | 查看JAR内容 |
| `jar xf my.jar` | 解压JAR |
| `jar uf my.jar newFile.class` | 更新JAR（添加新文件） |
| `jar xf my.jar META-INF/MANIFEST.MF` | 只解压清单文件 |

---

### ⑩ 依赖管理简介（了解）

**问题：** 大型项目可能有几十个JAR依赖，手动管理容易混乱。

**解决：** 使用构建工具（Maven/Gradle）自动管理依赖。

```xml
<!-- Maven的pom.xml中配置依赖 -->
<dependency>
    <groupId>mysql</groupId>
    <artifactId>mysql-connector-java</artifactId>
    <version>8.0.31</version>
</dependency>
```

**一句话：** 工具自动下载JAR、管理版本、处理传递依赖。

---

### ⑪ 小思考题

```bash
# 场景：你写了一个网络工具类 NetUtils，想让其他项目复用
# 问题1：应该把什么文件打包成JAR？.java还是.class？

# 问题2：别人的项目如何用你的NetUtils JAR？
```

**思考题2：**
如果JAR包中有一个配置文件`config.properties`，代码中如何读取？（提示：`ClassLoader.getResourceAsStream()`）
