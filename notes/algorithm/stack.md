# 栈（stack）及其存储结构和特点详解

[栈](http://data.biancheng.net/view/169.html)是一个有着特殊规则的数据结构。我们熟悉汉诺塔游戏（如图1 所示），这里有一个明确的规则，即每次只能移动顶端的一个圆盘。


![img](http://data.biancheng.net/uploads/allimg/180923/1-1P92309254a13.jpg)
图 1 汉诺塔游戏


栈也有这个特点。我们可以将栈视为汉诺塔中的一个柱子，我们往这个柱子上放置圆盘，先放下去的一定是最后才能拿出来的，而最后放下去的一定是最先拿出来的。这也是栈的最重要一个特点——后进先出（LIFO，Last In First Out），也可以说是先进后出（FILO，First In Last Out），我们无论如何只能从一端去操作元素。

栈又叫作堆栈（Stack），这里说明一下不要将它和堆混淆。实际上堆和栈是两个不同的概念，栈是一种只能在一端进行插入和删除的线性数据结构。

一般来说，栈主要有两个操作：一个是进栈（PUSH），又叫作入栈、压栈；另一个是出栈（POP），或者叫作退栈。

其实栈是一种比较简单的数据结构，但是由于其特性，又衍生了不少的相关算法。

## 栈的存储结构

栈一般使用一段连续的空间进行存储，通常预先分配一个长度，可以简单地使用

[数组](http://data.biancheng.net/view/181.html)

去实现，具体的存储结构如图 2 所示。



![img](http://data.biancheng.net/uploads/allimg/180923/1-1P923092AK95.jpg)
图 2 栈的存储结构


通过图 2 可以清晰地看到，只有一个方向可以对栈内的元素进行操作，而栈中最下面的一个元素成为栈底，一般是数组的第 0 个元素，而栈顶是栈内最后放入的元素。

一般而言，定义一个栈需要有一个初始的大小，这就是栈的初始容量。当需要放入的元素大于这个容量时，就需要进行扩容。

栈出入元素的操作如下。例如我们初始化一个长度为 10 的数组，并向其中放入元素，根据栈的定义，只能从数组的一端放入元素，我们设定这一端为数组中较大下标的方向。我们放入第 1 个元素，由于栈内没有元素，于是第 1 个元素就落到了数组的第 0 个下标的位置上；接着放入第 2 个元素，第 2 个元素该放入下标为 1 的位置上；以此类推，当放入了 5 个元素时，第 5 个入栈的元素应该在数组的第 4 个下标的位置上。

现在我们要进行出栈操作，出栈只能从一端操作，我们之前设定只能从数组下标较大的方向操作，因此需要确定数组中下标最大的一个方向中存在栈元素的位置下标是多少。我们一般会在栈中做个计数器来记录这个值。现在栈中有 5 个元素，所以将数组中的第 5 个位置也就是下标为 4 的元素出栈。此时数组中只剩下 4 个元素了。

下面是栈的实现代码，这里以整型元素为例，在 Java 类的高级语言中，数据类型可以换成对象。

```java
package me.irfen.algorithm.ch02;
import java.util.Arrays;
public class Stack {    
    private int size = 0;    
    private int[] array;    
    public Stack() {        
        this(10);    
    }    
    
    public Stack(int init) {        
        if (init <= 0) {            
            init = 10;        
        }        
        array = new int[init];    
    }   
    /**      * 入栈      * @param item 入栈元素的值    */    
    public void push(int item) {        
        if (size == array.length) {            
            array = Arrays.copyOf(array, size * 2);        
        }        
        array[size++] = item;    
    }   
    /**      * 获取栈顶元素，但是没有出栈      * @return      */    
    public int peek() {        
        if (size == 0) {            
            throw new IndexOutOfBoundsException("栈里已经空啦");        
        }       
        return array[size - 1];    
    }    
    /**      * 出栈，同时获取栈顶元素      * @return      */    
    public int pop() {        
        int item = peek();        
        size --; // 直接使元素个数减1，不需要真的清除元素，下次入栈会覆盖旧元素值        
        return item;    
    }    
    /**      * 栈是否满了      * @return      */    
    public boolean isFull() {        
        return size == array.length;    
    }    
    /**      * 栈是否为空栈      * @return      */    
    public boolean isEmpty() {        
        return size == 0;    
    }    
    public int size() {        
        return size;    
    }
}
```

下面是测试代码：

```java
package me.irfen.algorithm.ch02;
public class StackTest {
    public static void main(String[] args) {    
        Stack stack = new Stack(1); // 为了方便看出效果，设定初始数组长度为1    
        stack.push(1);    
        stack.push(2);    
        System.out.println(stack.size()); // 栈内元素个数为2，当前数组长度也为2    
        stack.push(3);    
        System.out.println(stack.size()); // 栈内元素个数为3，当前数组长度为4    
        System.out.println(stack.peek()); // 获取栈顶元素，为3，但是没有出栈    
        System.out.println(stack.size()); // 由于上面一行没有出栈，所以元素个数还是3    
        System.out.println(stack.pop()); // 栈顶元素出栈，返回3    
        System.out.println(stack.pop()); // 栈顶元素出站，返回2    
        System.out.println(stack.size()); // 出了两次栈，当前元素个数为1
    }
}
```

## 栈的特点

栈的特点是显而易见的，只能在一端进行操作，遵循先进后出或者后进先出的原则。

## 栈的适用场景

什么时使用栈？根据栈先进先出且只能在一端操作的特点，一般有下面几个应用。

#### 1) 逆序输出

由于栈具有先进后出的特点，所以逆序输出是其中一个非常简单的应用。首先把所有元素按顺序入栈，然后把所有元素出栈并输出，轻松实现逆序输出。

#### 2) 语法检查，符号成对出现

在编程语言中，一般括号都是成对出现的，比如“［”和“］”“｛”和“｝”“（”和“）”“＜”和“＞”（这里的“＜”和“＞”排除了作为大于小于号的情况）。

凡是遇到括号的前半部分，即为入栈符号（PUSH）；凡是遇到括号的后半部分，就比对是否与栈顶元素相匹配（PEEK），如果相匹配，则出栈（POP），否则就是匹配出错。

#### 3) 数制转换（将十进制的数转换为2～9的任意进制的数）

另一个应用就是用于实现十进制与其他进制的转换规则。

通过求余法，可以将十进制数转换为其他进制，比如要转为八进制，则将原十进制数除以 8，记录余数，然后继续将商除以 8，一直到商等于 0 为止，最后将余数倒着写出来就行了。

依照这个原理，当我们要将 100 转为八进制数时，先将 100 除以 8，商 12 余 4，第 1 个余数 4 入栈；之后继续用 12 除以 8，商 1 余 4，第 2 个余数 4 入栈；接着用 1 除以 8，商 0 余 1，第 3 个余数 1 入栈。最后将三个余数全部出栈，就得到了 100 的八进制数 144。当然，栈的应用不仅有这些，其他应用还有很多。比如常听到的编程语言调用中的“函数栈”，就是我们在调用方法时计算机会执行 PUSH 方法，记录调用，在 return 也就是方法结束之后，执行 POP 方法，完成前后对应。