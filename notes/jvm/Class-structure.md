# 类文件结构

## JVM 的“无关性”

谈论 JVM 的无关性，主要有以下两个：  

* 平台无关性：任何操作系统都能运行 Java 代码
* 语言无关性： JVM 能运行除 Java 以外的其他代码

Java 源代码首先需要使用 Javac 编译器编译成 .class 文件，然后由 JVM 执行 .class 文件，从而程序开始运行。

JVM 只认识 .class 文件，它不关心是何种语言生成了 .class 文件，只要 .class 文件符合 JVM 的规范就能运行。 目前已经有 JRuby、Jython、Scala 等语言能够在 JVM 上运行。它们有各自的语法规则，不过它们的编译器 都能将各自的源码编译成符合 JVM 规范的 .class 文件，从而能够借助 JVM 运行它们。

> Java 语言中的各种变量、关键字和运算符号的语义最终都是由多条字节码命令组合而成的， 因此字节码命令所能提供的语义描述能力肯定会比 Java 语言本身更加强大。 因此，有一些 Java 语言本身无法有效支持的语言特性，不代表字节码本身无法有效支持。

## Class 文件结构


Class 文件是二进制文件，它的内容具有严格的规范，文件中没有任何空格，全都是连续的 0/1。Class 文件 中的所有内容被分为两种类型：无符号数、表。

* 无符号数  无符号数表示 Class 文件中的值，这些值没有任何类型，但有不同的长度。u1、u2、u4、u8 分别代表 1/2/4/8 字节的无符号数。
* 表  由多个无符号数或者其他表作为数据项构成的符合数据类型。
  ![img](https://raw.githubusercontent.com/lindage1994/images/master/typora202007/07/111923-219456.png)

Class 文件具体由以下几个构成:

* 魔数

* 版本信息

* 常量池

* 访问标志

* 类索引、父类索引、接口索引集合

* 字段表集合

* 方法表集合

* 属性表集合



### 魔数

```java
    u4             magic; //Class 文件的标志
```

Class 文件的头 4 个字节称为魔数，用来表示这个 Class 文件的类型。

Class 文件的魔数是用 16 进制表示的“CAFE BABE”，是不是很具有浪漫色彩？

> 魔数相当于文件后缀名，只不过后缀名容易被修改，不安全，因此在 Class 文件中标识文件类型比较合适。

### 版本信息

```
    u2             minor_version;//Class 的小版本号
    u2             major_version;//Class 的大版本号
```

紧接着魔数的 4 个字节是版本信息，5-6 字节表示次版本号，7-8 字节表示主版本号，它们表示当前 Class 文件中使用的是哪个版本的 JDK。

高版本的 JDK 能向下兼容以前版本的 Class 文件，但不能运行以后版本的 Class 文件，即使文件格式并未发生任何变化，虚拟机也必需拒绝执行超过其版本号的 Class 文件。

### 常量池

```
    u2             constant_pool_count;//常量池的数量
    cp_info        constant_pool[constant_pool_count-1];//常量池
```

紧接着主次版本号之后的是常量池，常量池的数量是 constant_pool_count-1（**常量池计数器是从1开始计数的，将第0项常量空出来是有特殊考虑的，索引值为0代表“不引用任何一个常量池项”**）。

常量池主要存放两大常量：字面量和符号引用。字面量比较接近于 Java 语言层面的的常量概念，如文本字符串、声明为 final 的常量值等。而符号引用则属于编译原理方面的概念。包括下面三类常量：

- 类和接口的全限定名
- 字段的名称和描述符
- 方法的名称和描述符

常量池中每一项常量都是一个表，这14种表有一个共同的特点：**开始的第一位是一个 u1 类型的标志位 -tag 来标识常量的类型，代表当前这个常量属于哪种常量类型．**

| 类型                             | 标志（tag） | 描述                   |
| -------------------------------- | ----------- | ---------------------- |
| CONSTANT_utf8_info               | 1           | UTF-8编码的字符串      |
| CONSTANT_Integer_info            | 3           | 整形字面量             |
| CONSTANT_Float_info              | 4           | 浮点型字面量           |
| CONSTANT_Long_info               | ５          | 长整型字面量           |
| CONSTANT_Double_info             | ６          | 双精度浮点型字面量     |
| CONSTANT_Class_info              | ７          | 类或接口的符号引用     |
| CONSTANT_String_info             | ８          | 字符串类型字面量       |
| CONSTANT_Fieldref_info           | ９          | 字段的符号引用         |
| CONSTANT_Methodref_info          | 10          | 类中方法的符号引用     |
| CONSTANT_InterfaceMethodref_info | 11          | 接口中方法的符号引用   |
| CONSTANT_NameAndType_info        | 12          | 字段或方法的符号引用   |
| CONSTANT_MothodType_info         | 16          | 标志方法类型           |
| CONSTANT_MethodHandle_info       | 15          | 表示方法句柄           |
| CONSTANT_InvokeDynamic_info      | 18          | 表示一个动态方法调用点 |

`.class` 文件可以通过`javap -v class类名` 指令来看一下其常量池中的信息(`javap -v class类名-> temp.txt` ：将结果输出到 temp.txt 文件)。

### 访问标志

在常量池结束之后，紧接着的两个字节代表访问标志，这个标志用于识别一些类或者接口层次的访问信息，包括：这个 Class 是类还是接口，是否为 public 或者 abstract 类型，如果是类的话是否声明为 final 等等。

类访问和属性修饰符:

![img](https://raw.githubusercontent.com/lindage1994/images/master/typora202007/07/114110-415971.png)

我们定义了一个 Employee 类

```
package top.snailclimb.bean;
public class Employee {
   ...
}
```

通过`javap -v class类名` 指令来看一下类的访问标志。

![img](https://raw.githubusercontent.com/lindage1994/images/master/typora202007/07/114158-565401.png)

### 类索引、父类索引、接口索引集合

```
    u2             this_class;//当前类
    u2             super_class;//父类
    u2             interfaces_count;//接口
    u2             interfaces[interfaces_count];//一个类可以实现多个接口
```

**类索引用于确定这个类的全限定名，父类索引用于确定这个类的父类的全限定名，由于 Java 语言的单继承，所以父类索引只有一个，除了 `java.lang.Object` 之外，所有的 java 类都有父类，因此除了 `java.lang.Object` 外，所有 Java 类的父类索引都不为 0。**

**接口索引集合用来描述这个类实现了那些接口，这些被实现的接口将按`implents`(如果这个类本身是接口的话则是`extends`) 后的接口顺序从左到右排列在接口索引集合中。**

### 字段表集合

```
    u2             fields_count;//Class 文件的字段的个数
    field_info     fields[fields_count];//一个类会可以有个字段
```

字段表（field info）用于描述接口或类中声明的变量。字段包括类级变量以及实例变量，但不包括在方法内部声明的局部变量。

**field info(字段表) 的结构:**

![img](https://raw.githubusercontent.com/lindage1994/images/master/typora202007/07/113324-461520.png)

- **access_flags:** 字段的作用域（`public` ,`private`,`protected`修饰符），是实例变量还是类变量（`static`修饰符）,可否被序列化（transient 修饰符）,可变性（final）,可见性（volatile 修饰符，是否强制从主内存读写）。
- **name_index:** 对常量池的引用，表示的字段的名称；
- **descriptor_index:** 对常量池的引用，表示字段和方法的描述符；
- **attributes_count:** 一个字段还会拥有一些额外的属性，attributes_count 存放属性的个数；
- **attributes[attributes_count]:** 存放具体属性具体内容。

上述这些信息中，各个修饰符都是布尔值，要么有某个修饰符，要么没有，很适合使用标志位来表示。而字段叫什么名字、字段被定义为什么数据类型这些都是无法固定的，只能引用常量池中常量来描述。

**字段的 access_flags 的取值:**

![img](https://raw.githubusercontent.com/lindage1994/images/master/typora202007/07/113230-849485.png)

> 字段表集合中不会出现从父类（或接口）中继承而来的字段，但有可能出现原本 Java 代码中不存在的字段，譬如在内部类中为了保持对外部类的访问性，会自动添加指向外部类实例的字段。

### 方法表集合

```
    u2             methods_count;//Class 文件的方法的数量
    method_info    methods[methods_count];//一个类可以有个多个方法
```

methods_count 表示方法的数量，而 method_info 表示的方法表。

Class 文件存储格式中对方法的描述与对字段的描述几乎采用了完全一致的方式。方法表的结构如同字段表一样，依次包括了访问标志、名称索引、描述符索引、属性表集合几项。

**method_info(方法表的) 结构:**

![img](https://raw.githubusercontent.com/lindage1994/images/master/typora202007/07/113132-983971.png)

**方法表的 access_flag 取值：**

![img](https://raw.githubusercontent.com/lindage1994/images/master/typora202007/07/113026-139344.png)



注意：因为`volatile`修饰符和`transient`修饰符不可以修饰方法，所以方法表的访问标志中没有这两个对应的标志，但是增加了`synchronized`、`native`、`abstract`等关键字修饰方法，所以也就多了这些关键字对应的标志。

### 属性表集合

```
   u2             attributes_count;//此类的属性表中的属性数
   attribute_info attributes[attributes_count];//属性表集合
```

在 Class 文件，字段表，方法表中都可以携带自己的属性表集合，以用于描述某些场景专有的信息。与 Class 文件中其它的数据项目要求的顺序、长度和内容不同，属性表集合的限制稍微宽松一些，不再要求各个属性表具有严格的顺序，并且只要不与已有的属性名重复，任何人实现的编译器都可以向属性表中写 入自己定义的属性信息，Java 虚拟机运行时会忽略掉它不认识的属性。



## 链接

- [https://blog.csdn.net/luanlouis/article/details/41039269](https://blog.csdn.net/luanlouis/article/details/41039269)