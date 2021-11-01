# Java面试--Java语言

## 基础与常识

### Java语言特点

1. 面对对象的语言（封装，继承，多态）
2. Java具有平台无关性（通过JVM实现）
3. 编译与解释并存

### JVM、JDK、JRE

#### JVM

JVM是Java虚拟机，它是运行Java字节码的虚拟机。JVM针对不同系统给出了不同实现。目的是使用相同字节码在不同系统上运行能够得到相同结果

**问题扩展1：**什么是字节码，采用字节码的好处是什么

> Java字节码是一种JVM能够理解的代码，文件以扩展名.class结尾。Java字节码不面向任何处理器，只面向Java虚拟机。
>
> ‘Java源代码首先编译成字节码在解释运行，在一定程度上解决了传统解释型语言执行效率低的问题，同时又保持了解释行语言可移植的特点。通过Java字节码，Java程序能够实现编译一次就能够在多种操作系统上比较高效的运行
>
> 

**问题扩展2：**Java程序从源代码到运行需要经历什么

![image-20211012100134804](Java%E9%9D%A2%E8%AF%95--Java%E8%AF%AD%E8%A8%80.assets/image-20211012100134804.png)

```
JIT？？
```

#### JDK、JRE

JDK（Java Development Kit），它是功能齐全的Java SDK（Software Development Kit）。它不仅拥有JRE所拥有的一切，还包括了编译器（javac）和javac开发的相关工具（javadoc，jdb）

JRE（Java Runtime Environment）是Java运行时环境。它是运行已编译的Java程序所需要的所有内容的集合，包括JVM，Java类库等。但是仅有JRE是不能够构建新的Java程序的。

### 为什么说Java语言是“编译和解释并存”

因为从Java源代码到程序运行要经过两个步骤。首先Java源代码需要通过javac编译成字节码文件，其次字节码文件通过类加载器被加载到JVM中逐行解释运行，或者被JIT编译成机器指令后再运行。

因此我们可以认为Java语言编译和解释并存

### Java与C++之间有共同点和区别

- Java和C++都是面向对象的语言，都支持封装，继承和多态
- Java不提供指针来直接访问内存，内存的管理通过GC自动管理，程序内存更加的安全
- Java的类是单继承的而C++支撑多继承机制。但是Java也同样可以继承多个接口

## 基本语法

### 字符型常量和字符串常量的区别

1. 形式：字符常量是一对单引号引起来的字符，而字符串常量是一对双引号引起来的0个或若干个字符
2. 含义：字符常量相当于对应于其ASCII码的int型值，而字符串常量实际上一个引用值，指向字符串常量池中的字符串数据。
3. 占用内存大小：字符常量占用2字节，字符串占若干个字节

> 字符封装类 `Character` 有一个成员常量 `Character.SIZE` 值为 16,单位是`bits`,该值除以 8(`1byte=8bits`)后就可以得到 2 个字节

### 标识符和关键字的区别

Java语言中，对于变量，常量，函数，语句块取名字，都称之为Java标识符。关键字是被赋予特殊含义的标识符，Java语言已经赋予其特殊的含义，只能用在特殊的地方。

|                      |          |            |          |              |            |           |        |
| -------------------- | -------- | ---------- | -------- | ------------ | ---------- | --------- | ------ |
| 分类                 | 关键字   |            |          |              |            |           |        |
| 访问控制             | private  | protected  | public   |              |            |           |        |
| 类，方法和变量修饰符 | abstract | class      | extends  | final        | implements | interface | native |
|                      | new      | static     | strictfp | synchronized | transient  | volatile  |        |
| 程序控制             | break    | continue   | return   | do           | while      | if        | else   |
|                      | for      | instanceof | switch   | case         | default    |           |        |
| 错误处理             | try      | catch      | throw    | throws       | finally    |           |        |
| 包相关               | import   | package    |          |              |            |           |        |
| 基本类型             | boolean  | byte       | char     | double       | float      | int       | long   |
|                      | short    | null       | true     | false        |            |           |        |
| 变量引用             | super    | this       | void     |              |            |           |        |
| 保留字               | goto     | const      |          |              |            |           |        |

### Java泛型，类型擦除，通配符

Java泛型（Generics）提供了编译时类型安全检查机制，这个机制允许程序员在编译时检查到非法的类型。泛型最众所周知的应用就是容器类

```
？？？不是太理解
Java中的泛型是使用擦除来实现的，所以在使用泛型的时候，任何具体的类型信息都被擦除了，只知道当前使用的是一个对象

泛型的本质是参数化类型，也就是说所操作的数据类型被指定为一个参数。
```

**常用的通配符为： T，E，K，V，？**

- ？ 表示不确定的 java 类型
- T (type) 表示具体的一个 java 类型
- K V (key value) 分别代表 java 键值中的 Key Value
- E (element) 代表 Element

### ==和equals的区别

- 对于基本数据类型来说，==比较的是值。对于引用数据类型来说==比较的是对象地址。本质上来说，比较的都是值
- equals()不能够用于基本数据类型的比较，只能够用于判断两个对象是否相等。equals方法定义在Objet类中。

`Object` 类 `equals()` 方法：

```java
public boolean equals(Object obj) {
     return (this == obj);
}
```

- 如果没有重写equals()方法，在Object类中默认使用==作为比较规则，也就是比较对象地址
- 如果重写了equals()方法，则使用重写的规则进行比较。通常比较规则是属性是否相等

> - String中的equals()是已经被重写过的，比较的是对象的值
>
> ```java
> //`String`类`equals()`方法：
> public boolean equals(Object anObject) {
>     if (this == anObject) {
>         return true;
>     }
>     if (anObject instanceof String) {
>         String anotherString = (String)anObject;
>         int n = value.length;
>         if (n == anotherString.value.length) {
>             char v1[] = value;
>             char v2[] = anotherString.value;
>             int i = 0;
>             while (n-- != 0) {
>                 if (v1[i] != v2[i])
>                     return false;
>                 i++;
>             }
>             return true;
>         }
>     }
>     return false;
> }
> ```
>
> 

### hashCode()与equals()

#### 什么是hashCode()

- hashCode()定义在Object函数中，这就意味着任何Java的类都包含hashCode()。
- hashCode()的作用是获取哈希码。哈希码的作用是确定当前对象在哈希表中的索引位置。
- 散列表存储的是键值对(key-value)，它的特点是：能根据“键”快速的检索出对应的“值”。这其中就利用到了散列码！

#### 为什么要有hashCode

当我们把一个对象加入HashSet的时候，会首先计算对象的hashCode从而得到对象加入的位置

- 如果当前位置为空则直接将对象加入HashSet
- 如果当前位置已经添加了元素，这调用对象的equals()方法对这个位置上的所有对象依次进行判断。如果返回true则放弃添加，如果全都不相等，则添加。

#### 为什么重写equals的似乎必须重写hashCode()

`equals`和`hashCode`都是用来判断两个对象是否相等的。

- equals - 保证比较对象是否是绝对相等的
- hashCode - 保证在最快的时间内判断两个对象是否相等，可能有误差值

从而可以得出以下结论

- equals为 true，hashCode一定相等
- hashCode相等时 ， equals可以不用为 true （也就是hash碰撞的时候）

如果不同时重写，将导致该类无法结合所以与散列的集合一起正常运作，这里指的是HashMap、HashSet。

**如果只重写equals**

这会导致两个相等的对象拥有不同的散列码。这会导致在HashSet在进行添加的时候产生重复





## 基本数据类型

### Java中有几种数据类型，分别对应的包装类是什么，各自占用多少字节？

Java 中有 8 种基本数据类型，分别为：

1. 6 种数字类型 ：`byte`、`short`、`int`、`long`、`float`、`double`
2. 1 种字符类型：`char`
3. 1 种布尔型：`boolean`。

这 8 种基本数据类型的默认值以及所占空间的大小如下：

| 基本类型  | 位数 | 字节 | 默认值  |
| --------- | ---- | ---- | ------- |
| `int`     | 32   | 4    | 0       |
| `short`   | 16   | 2    | 0       |
| `long`    | 64   | 8    | 0L      |
| `byte`    | 8    | 1    | 0       |
| `char`    | 16   | 2    | 'u0000' |
| `float`   | 32   | 4    | 0f      |
| `double`  | 64   | 8    | 0d      |
| `boolean` | 1    |      | false   |

另外，对于 `boolean`，官方文档未明确定义，它依赖于 JVM 厂商的具体实现。逻辑上理解是占用 1 位，但是实际中会考虑计算机高效存储因素。

这八种基本类型都有对应的包装类分别为：`Byte`、`Short`、`Integer`、`Long`、`Float`、`Double`、`Character`、`Boolean` 。

> 基本数据类型直接存放在 Java 虚拟机栈中的局部变量表中，而包装类型属于对象类型，我们知道对象实例都存在于堆中。相比于对象类型， 基本数据类型占用的空间非常小。
