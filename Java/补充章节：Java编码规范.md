## 补充章节：Java编码规范

### ① 核心概念

**一句话总结：**
> 编码规范是代码的"书写规则"——让代码易读、易维护、风格统一，就像书法有楷书规范，大家都能看懂。

**生活类比：**
- **无规范** = 每个人写字的笔顺、大小、间距都不同（别人看不懂）
- **有规范** = 印刷体，清晰统一（所有人都能轻松阅读）

**为什么重要？**
> 代码是给人看的，顺便给机器执行。规范让团队协作顺畅，减少沟通成本。

---

## 一、标识符命名规范

### ① 核心原则

| 类型 | 命名规则 | 示例 |
|------|----------|------|
| **包名** | 全小写，点分隔，域名倒序 | `com.network.chat.client` |
| **类/接口** | 大驼峰（PascalCase），首字母大写 | `ChatClient`, `MessageHandler` |
| **方法/变量** | 小驼峰（camelCase），首字母小写 | `sendMessage()`, `userName` |
| **常量** | 全大写，下划线分隔 | `MAX_RETRY_COUNT`, `DEFAULT_PORT` |
| **枚举** | 类名大驼峰，常量全大写 | `enum Color { RED, GREEN, BLUE }` |
| **泛型** | 单个大写字母 | `T`, `E`, `K`, `V` |

---

### ② 精简示例

```java
// 包名：全小写
package com.network.chat.client;

// 类名：大驼峰
public class ChatClient {
    
    // 常量：全大写 + 下划线
    public static final int DEFAULT_PORT = 8080;
    public static final String SERVER_ADDRESS = "127.0.0.1";
    
    // 变量：小驼峰
    private String userName;
    private int retryCount;
    private boolean isConnected;
    
    // 方法：小驼峰，动词开头
    public void sendMessage(String message) { }
    public String getCurrentUser() { }
    private boolean isValidPort(int port) { }
    
    // 枚举
    public enum ConnectionStatus {
        CONNECTED, DISCONNECTED, CONNECTING
    }
}
```

---

### ③ 命名禁忌

| ❌ 错误示例 | ✅ 正确示例 | 原因 |
|------------|------------|------|
| `String a;` | `String userName;` | 有意义的名称 |
| `int 1stNumber;` | `int firstNumber;` | 数字不能开头 |
| `String user-name;` | `String userName;` | 不能用横线 |
| `class chat_client` | `class ChatClient` | 类名用大驼峰 |
| `final int max = 100;` | `final int MAX_SIZE = 100;` | 常量全大写 |
| `String _temp;` | `String temp;` | 避免下划线开头 |

---

## 二、注释规范

### ① 核心原则

| 注释类型 | 格式 | 使用场景 |
|----------|------|----------|
| **单行注释** | `// 注释内容` | 解释复杂逻辑、TODO标记 |
| **多行注释** | `/* 注释内容 */` | 临时注释代码块 |
| **文档注释** | `/** 注释内容 */` | 类、方法、字段的说明 |

---

### ② 精简示例

```java
/**
 * 聊天室客户端
 * 
 * <p>负责与服务器建立连接、发送消息、接收消息</p>
 * 
 * @author 你的名字
 * @version 1.0
 * @since 2024-01-15
 */
public class ChatClient {
    
    /**
     * 服务器端口号
     * 默认使用8080，可通过构造函数修改
     */
    private int port;
    
    /**
     * 发送消息到服务器
     * 
     * @param message 要发送的消息内容，不能为null或空字符串
     * @return true 发送成功，false 发送失败
     * @throws IllegalStateException 连接未建立时调用
     */
    public boolean sendMessage(String message) {
        // 边界检查：消息不能为空
        if (message == null || message.trim().isEmpty()) {
            return false;
        }
        
        // TODO: 后续需要添加消息加密功能
        writer.println(message);
        
        /* 
         * 这段代码暂时注释，等新版本稳定后删除
         * oldMethod(message);
         */
        
        return true;
    }
}
```

---

### ③ 文档注释常用标签

| 标签 | 作用 | 示例 |
|------|------|------|
| `@param` | 描述方法参数 | `@param name 用户名称` |
| `@return` | 描述返回值 | `@return 是否成功` |
| `@throws` | 描述可能抛出的异常 | `@throws IOException 网络错误` |
| `@author` | 作者 | `@author 张三` |
| `@version` | 版本 | `@version 1.0` |
| `@since` | 开始版本 | `@since 1.0` |
| `@deprecated` | 标记已过时 | `@deprecated 请用新方法` |

---

### ④ 注释规范要点

```java
// ✅ 好注释：解释"为什么"
// 使用指数退避算法，避免服务器压力过大
Thread.sleep(backoffTime * 2);

// ❌ 坏注释：重复代码内容
// 让线程睡眠1秒
Thread.sleep(1000);

// ✅ 特殊情况：复杂正则或算法
// 正则说明：匹配邮箱格式，如 user@example.com
Pattern EMAIL_PATTERN = Pattern.compile("^[A-Za-z0-9+_.-]+@(.+)$");
```

---

## 三、日志规范

### ① 为什么需要日志？

**解决什么痛点？**

```java
// 坏习惯：用System.out.println
System.out.println("用户登录：" + userName);
System.out.println("连接失败");

// 问题：
// 1. 不能控制输出级别（调试/生产无法区分）
// 2. 不能输出到文件
// 3. 没有时间戳、线程信息
// 4. 正式环境无法关闭
```

---

### ② 日志框架

```java
import org.slf4j.Logger;
import org.slf4j.LoggerFactory;

public class ChatClient {
    // 创建日志对象（每个类一个）
    private static final Logger log = LoggerFactory.getLogger(ChatClient.class);
    
    public void connect(String host, int port) {
        // 不同级别的日志
        log.debug("尝试连接 {}:{}", host, port);  // 调试
        log.info("连接成功");                       // 信息
        log.warn("连接超时，重试中");               // 警告
        log.error("连接失败", exception);          // 错误（带异常堆栈）
    }
}
```

---

### ③ 日志级别（从低到高）

| 级别 | 用途 | 生产环境是否开启 |
|------|------|------------------|
| `TRACE` | 最细粒度跟踪 | ❌ 关闭 |
| `DEBUG` | 调试信息 | ❌ 关闭 |
| `INFO` | 关键流程 | ✅ 开启 |
| `WARN` | 警告，不影响运行 | ✅ 开启 |
| `ERROR` | 错误，需要关注 | ✅ 开启 |

**配置示例（logback.xml）：**
```xml
<configuration>
    <!-- 生产环境只输出INFO及以上 -->
    <root level="INFO">
        <appender-ref ref="FILE"/>
    </root>
    
    <!-- 开发环境可以输出DEBUG -->
    <root level="DEBUG">
        <appender-ref ref="CONSOLE"/>
    </root>
</configuration>
```

---

### ④ 日志规范要点

```java
// ✅ 使用参数化，避免字符串拼接
log.debug("用户{}登录成功", userName);

// ❌ 字符串拼接（性能差，且级别关闭时也会拼接）
log.debug("用户" + userName + "登录成功");

// ✅ 记录异常时要传异常对象
try {
    connect();
} catch (IOException e) {
    log.error("连接失败", e);  // 打印堆栈
}

// ❌ 只打印消息，丢失堆栈信息
log.error("连接失败：" + e.getMessage());
```

---

## 四、缩进与格式规范

### ① 核心规范

| 规范项 | 要求 | 示例 |
|--------|------|------|
| **缩进** | 4个空格（不用Tab） | 每级缩进4空格 |
| **行宽** | 不超过80-120字符 | 超长自动换行 |
| **大括号** | 左大括号不换行 | `if (x > 0) {` |
| **空行** | 逻辑块之间空行 | 方法之间空行 |
| **空格** | 关键字后加空格 | `if (true)` `for (int i)` |

---

### ② 精简示例

```java
// ✅ 正确格式
public class FormatExample {
    private String name;
    
    public void method() {
        if (name != null) {              // 左大括号不换行
            for (int i = 0; i < 10; i++) {
                System.out.println(i);
            }
        } else {
            System.out.println("空");
        }
    }
    
    public void longMethodWithManyParams(
            String param1,
            String param2,
            int param3) {
        // 参数过多时换行，缩进8空格
    }
}

// ❌ 错误格式
public class BadFormat{
private String name;
public void method(){
if(name!=null){
System.out.println("拥挤");
}}
}
```

---

### ③ 常见IDE格式化快捷键

| IDE | 快捷键 | 说明 |
|-----|--------|------|
| IDEA | `Ctrl + Alt + L` | 格式化代码 |
| Eclipse | `Ctrl + Shift + F` | 格式化代码 |
| VS Code | `Shift + Alt + F` | 格式化代码 |

---

## 五、声明顺序规范

### ① 类成员声明顺序

```java
public class OrderExample {
    // 1. 静态常量（static final）
    public static final int MAX_SIZE = 100;
    
    // 2. 静态变量
    private static int instanceCount = 0;
    
    // 3. 实例常量（final）
    private final String id;
    
    // 4. 实例变量
    private String name;
    private int age;
    
    // 5. 静态初始化块
    static {
        instanceCount = 0;
    }
    
    // 6. 实例初始化块
    {
        id = UUID.randomUUID().toString();
    }
    
    // 7. 构造方法
    public OrderExample(String name) {
        this.name = name;
    }
    
    // 8. 静态方法
    public static int getInstanceCount() {
        return instanceCount;
    }
    
    // 9. 实例方法
    public String getName() {
        return name;
    }
    
    // 10. 重写方法（Object类方法）
    @Override
    public String toString() {
        return name;
    }
}
```

---

## 六、魔法数字与常量

### ① 核心概念

**一句话总结：**
> 魔法数字是代码中出现的没有解释的字面量，应该用有意义的常量代替。

**生活类比：**
- **魔法数字** = 说"等5分钟"（5什么意思？）
- **常量** = 说"等下课时间5分钟"（含义明确）

---

### ② 示例对比

```java
// ❌ 坏代码：魔法数字
public class BadCode {
    public boolean isValidAge(int age) {
        return age >= 0 && age <= 150;  // 0和150是什么意思？
    }
    
    public void delay() {
        Thread.sleep(3000);  // 为什么是3000？
    }
    
    public String getErrorMessage(int code) {
        if (code == 404) return "Not Found";  // 404代表什么？
        if (code == 500) return "Server Error";
        return "Unknown";
    }
}

// ✅ 好代码：使用常量
public class GoodCode {
    private static final int MIN_AGE = 0;
    private static final int MAX_AGE = 150;
    private static final int CONNECTION_TIMEOUT_MS = 3000;
    
    // 使用枚举替代魔法数字
    public enum HttpStatus {
        NOT_FOUND(404, "Not Found"),
        SERVER_ERROR(500, "Server Error");
        
        private final int code;
        private final String message;
        
        HttpStatus(int code, String message) {
            this.code = code;
            this.message = message;
        }
    }
    
    public boolean isValidAge(int age) {
        return age >= MIN_AGE && age <= MAX_AGE;
    }
    
    public void connect() throws InterruptedException {
        Thread.sleep(CONNECTION_TIMEOUT_MS);
    }
}
```

---

### ③ 枚举替代魔法数字

```java
// 最佳实践：用枚举代替整数常量
public enum ConnectionState {
    DISCONNECTED(0, "未连接"),
    CONNECTING(1, "连接中"),
    CONNECTED(2, "已连接");
    
    private final int code;
    private final String description;
    
    ConnectionState(int code, String description) {
        this.code = code;
        this.description = description;
    }
    
    public int getCode() { return code; }
    public String getDescription() { return description; }
}

// 使用
ConnectionState state = ConnectionState.CONNECTED;
if (state == ConnectionState.CONNECTED) {
    // 不需要记忆数字2代表什么
}
```

---

## 七、小思考题

```java
// 指出下面代码的规范问题
public class test {
    final static int max = 100;
    
    public void calc(){
        int a=10,b=20;
        if(a>b){
        System.out.println(a);
        }
        // 计算价格
        double p = a * 1.08;
        System.out.println(404);
    }
}
```

**思考题2：**
为什么说"好代码是自解释的"？注释越多越好吗？
