## 第一章：Java概述与环境搭建

**① 核心概念（一句话总结）**
> Java程序通过JVM（Java虚拟机）运行，实现“一次编写，到处运行”。

**② 为什么需要它？**
C语言编译后的程序依赖操作系统，换平台要重新编译。Java编译成字节码（.class），由JVM解释执行，屏蔽了底层差异。

**③ 精简代码示例**
```java
// HelloWorld.java
public class HelloWorld {
    public static void main(String[] args) {
        System.out.println("Hello Java");  // 输出语句
    }
}
// 编译：javac HelloWorld.java → 生成 HelloWorld.class
// 运行：java HelloWorld
```

**④ 小思考题**
`JDK`、`JRE`、`JVM`三者是什么关系？（提示：包含关系）

---

## 第二章：变量与数据类型

**① 核心概念（一句话总结）**
> Java是强类型语言，变量必须先声明类型再使用。

**② 与C的区别**
- Java的**基本类型**大小固定（int永远是4字节），不随平台变化
- 多了**boolean**类型（只有true/false，不能用0/1代替）
- 引用类型（类、数组、接口）相当于C的指针，但更安全

**③ 精简代码示例**
```java
int age = 20;           // 整型
double score = 89.5;    // 浮点型
boolean isPass = true;  // 布尔型，不能用1/0
String name = "张三";    // 引用类型，String是类
```

**④ 小思考题**
`int a = 10;` 和 `String s = "hello";` 中，a和s在内存存储上有什么不同？

---

## 第三章：运算符与表达式

**① 核心概念（一句话总结）**
> 运算符用法与C几乎完全相同，重点掌握逻辑运算符的**短路特性**。

**② 与C的区别**
- 逻辑运算符 `&&` 和 `||` 同样有短路求值
- 位运算符完全一致
- 三元运算符 `条件 ? 值1 : 值2` 用法相同

**③ 精简代码示例**
```java
int a = 10, b = 20;
int max = (a > b) ? a : b;   // 三元运算符
boolean result = (a > 5) && (b < 30);  // 短路与，左边false则右边不执行
System.out.println(max);      // 20
```

**④ 小思考题**
`int x = 5; boolean flag = (x > 3) || (x++ > 3);` 执行后x的值是多少？为什么？

---

## 第四章：流程控制

**① 核心概念（一句话总结）**
> 流程控制语法与C完全一致，switch支持String类型是Java的特色。

**② 与C的区别**
- `if-else`、`for`、`while`、`do-while`、`break`、`continue` 用法完全相同
- Java的switch**可以判断String类型**（C语言不行）

**③ 精简代码示例**
```java
// switch支持String（Java 7+）
String day = "周一";
switch (day) {
    case "周一":
        System.out.println("开始上课");
        break;
    default:
        System.out.println("其他");
}
```

**④ 小思考题**
用for循环打印1到10，但跳过数字5，你会怎么实现？

---

## 第五章：数组

**① 核心概念（一句话总结）**
> 数组在Java中是**对象**，有length属性，不像C那样用sizeof计算长度。

**② 与C的区别**
- 数组有 `length` 属性，直接获取长度
- 声明方式：`int[] arr = new int[5];`（推荐写法）
- 多维数组不一定是规则的（可以是不规则数组）

**③ 精简代码示例**
```java
int[] numbers = {1, 2, 3, 4, 5};   // 声明并初始化
System.out.println(numbers.length); // 5，直接获取长度

// 遍历数组
for (int i = 0; i < numbers.length; i++) {
    System.out.print(numbers[i] + " ");
}
```

**④ 小思考题**
Java的 `int[] arr = new int[5];` 和C的 `int arr[5];` 有什么区别？（提示：默认值）

---

## 快速总结

| 对比项 | C语言 | Java |
|--------|-------|------|
| 编译运行 | 编译成机器码 | 编译成字节码，由JVM运行 |
| 基本类型 | 大小依赖平台 | 大小固定 |
| 布尔类型 | 用int代替（0/1） | 独立的boolean类型 |
| switch | 只支持整型 | 支持整型、String、枚举 |
| 数组 | 指针操作，无长度属性 | 对象，有length属性 |
