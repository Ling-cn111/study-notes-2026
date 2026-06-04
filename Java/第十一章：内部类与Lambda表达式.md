## 第十一章：内部类与Lambda表达式

这一章我们分两部分讲，先讲内部类（了解即可），再讲Lambda（重点掌握）。

---

## 第一部分：内部类

### ① 核心概念

**一句话总结：**
> 内部类就是定义在另一个类内部的类，可以访问外部类的私有成员，用于逻辑上的"从属关系"。

**生活类比：**
- **外部类** = 汽车
- **内部类** = 发动机（发动机离不开汽车，汽车可以访问发动机的内部状态）

---

### ② 为什么需要它？

**解决什么痛点？**

当某个类只服务于另一个类，不希望被外部直接访问时，可以把它定义成内部类，实现更好的封装。

---

### ③ 精简代码示例

```java
// 外部类
class Outer {
    private String secret = "秘密";
    
    // 成员内部类
    class Inner {
        void showSecret() {
            System.out.println(secret);  // 可以访问外部类的私有成员
        }
    }
    
    // 静态内部类
    static class StaticInner {
        void show() {
            System.out.println("静态内部类");
            // 不能访问非静态的 secret
        }
    }
}

// 使用
public class Main {
    public static void main(String[] args) {
        // 创建成员内部类需要先有外部类对象
        Outer outer = new Outer();
        Outer.Inner inner = outer.new Inner();
        inner.showSecret();   // 输出：秘密
        
        // 创建静态内部类不需要外部类对象
        Outer.StaticInner staticInner = new Outer.StaticInner();
        staticInner.show();   // 输出：静态内部类
    }
}
```

---

### ④ 匿名内部类（重点）

**一句话总结：**
> 匿名内部类是没有名字的内部类，用于**即用即创建**一个接口或抽象类的实现，通常用于事件监听、回调等场景。

```java
// 先定义一个接口
interface Greeting {
    void sayHello();
}

// 传统方式：单独写一个类实现接口
class ChineseGreeting implements Greeting {
    public void sayHello() {
        System.out.println("你好");
    }
}

// 匿名内部类方式：不需要单独写类
public class Main {
    public static void main(String[] args) {
        // 匿名内部类：直接创建接口的实现
        Greeting g = new Greeting() {
            @Override
            public void sayHello() {
                System.out.println("你好");
            }
        };
        g.sayHello();   // 输出：你好
    }
}
```

---

## 第二部分：Lambda表达式（重点掌握）

### ① 核心概念

**一句话总结：**
> Lambda表达式是**匿名内部类的简化写法**，用于实现只有一个抽象方法的接口（函数式接口），让代码更简洁。

**对比理解：**
- 匿名内部类：啰嗦，像写作文
- Lambda表达式：简洁，像写便条

```java
// 匿名内部类写法（啰嗦）
Greeting g1 = new Greeting() {
    @Override
    public void sayHello() {
        System.out.println("你好");
    }
};

// Lambda表达式写法（简洁）
Greeting g2 = () -> System.out.println("你好");
```

---

### ② 为什么需要它？

**解决什么痛点？**

在集合排序、多线程、事件处理等场景，经常需要传递"行为"。传统匿名内部类语法太冗余，Lambda让代码更清晰、更函数式。

---

### ③ Lambda语法格式

**语法：**
```
(参数列表) -> { 方法体 }
```

**精简规则：**
| 场景 | 规则 | 示例 |
|------|------|------|
| 参数类型可省略 | 编译器自动推断 | `(a, b) -> a + b` |
| 单参数可省略括号 | 只有一个参数时 | `a -> a * a` |
| 单条语句可省略大括号和return | 方法体只有一条语句 | `a -> a * a` |

---

### ④ 精简代码示例

```java
import java.util.Arrays;

// 定义函数式接口（只有一个抽象方法）
interface Calculator {
    int calc(int a, int b);
}

public class Main {
    public static void main(String[] args) {
        // 1. 基本Lambda
        Calculator add = (a, b) -> { return a + b; };
        System.out.println(add.calc(3, 5));  // 8
        
        // 2. 简化版（省略大括号和return）
        Calculator subtract = (a, b) -> a - b;
        System.out.println(subtract.calc(10, 3));  // 7
        
        // 3. 集合排序中的Lambda（实用场景）
        String[] names = {"张三", "李四", "王五"};
        Arrays.sort(names, (s1, s2) -> s1.length() - s2.length());
        System.out.println(Arrays.toString(names));  // [张三, 李四, 王五]
        
        // 4. 线程中的Lambda
        new Thread(() -> System.out.println("线程运行")).start();
    }
}
```

---

### ⑤ @FunctionalInterface 注解

```java
// 加上这个注解，编译器会检查：接口只能有一个抽象方法
@FunctionalInterface
interface MyFunction {
    void doSomething();
    // void doAnother();  // 加上这行会编译错误
}
```

**常见的内置函数式接口（java.util.function包）：**
| 接口 | 抽象方法 | 用途 |
|------|----------|------|
| `Predicate<T>` | `boolean test(T t)` | 判断条件 |
| `Consumer<T>` | `void accept(T t)` | 消费数据 |
| `Function<T,R>` | `R apply(T t)` | 转换数据 |
| `Supplier<T>` | `T get()` | 提供数据 |

```java
// 使用示例
Predicate<Integer> isPositive = x -> x > 0;
System.out.println(isPositive.test(5));  // true

Consumer<String> printer = s -> System.out.println(s);
printer.accept("Hello");  // Hello

Function<String, Integer> length = s -> s.length();
System.out.println(length.apply("Java"));  // 4
```

---

### ⑥ 小思考题

```java
// 代码1：匿名内部类
Runnable r1 = new Runnable() {
    @Override
    public void run() {
        System.out.println("运行");
    }
};

// 代码2：Lambda表达式
Runnable r2 = () -> System.out.println("运行");
```

**思考题：**
两种写法完全等价吗？如果不完全等价，区别在哪里？

提示：思考 `this` 关键字在两种写法中分别指向谁。
