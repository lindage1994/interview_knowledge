顺序表，全名顺序存储结构，是

[线性表](http://data.biancheng.net/view/157.html)

的一种。通过《[线性表](http://data.biancheng.net/view/157.html)》一节的学习我们知道，线性表用于存储逻辑关系为“一对一”的数据，顺序表自然也不例外。

不仅如此，顺序表对数据的物理存储结构也有要求。顺序表存储数据时，会提前申请一整块足够大小的物理空间，然后将数据依次存储起来，存储时做到数据元素之间不留一丝缝隙。

例如，使用顺序表存储集合 `{1,2,3,4,5}`，数据最终的存储状态如

[图](http://data.biancheng.net/view/200.html)

 1 所示：


![img](http://data.biancheng.net/uploads/allimg/181121/2-1Q121202555F0.gif)
图 1 顺序存储结构示意图


由此我们可以得出，将“具有 '一对一' 逻辑关系的数据按照次序连续存储到一整块物理空间上”的存储结构就是顺序存储结构。

通过观察图 1 中数据的存储状态，我们可以发现，顺序表存储数据同

[数组](http://data.biancheng.net/view/181.html)

非常接近。其实，顺序表存储数据使用的就是数组。

## 顺序表的初始化

使用顺序表存储数据之前，除了要申请足够大小的物理空间之外，为了方便后期使用表中的数据，顺序表还需要实时记录以下 2 项数据：

1. 顺序表申请的存储容量；
2. 顺序表的长度，也就是表中存储数据元素的个数；

提示：正常状态下，顺序表申请的存储容量要大于顺序表的长度。

因此，我们需要自定义顺序表，C 语言实现代码如下：

```
typedef struct Table{    int * head;//声明了一个名为head的长度不确定的数组，也叫“动态数组”    int length;//记录当前顺序表的长度    int size;//记录顺序表分配的存储容量}table;
```

注意，head 是我们声明的一个未初始化的动态数组，不要只把它看做是普通的指针。

接下来开始学习顺序表的初始化，也就是初步建立一个顺序表。建立顺序表需要做如下工作：

- 给 head 动态数据申请足够大小的物理空间；
- 给 size 和 length 赋初值；


因此，C 语言实现代码如下：

```
#define Size 5 //对Size进行宏定义，表示顺序表申请空间的大小table initTable(){    table t;    t.head = (int*)malloc(Size * sizeof(int));//构造一个空的顺序表，动态申请存储空间    if (!t.head) //如果申请失败，作出提示并直接退出程序    {        printf("初始化失败");        exit(0);    }    t.length = 0;//空表的长度初始化为0    t.size = Size;//空表的初始存储空间为Size    return t;}
```

我们看到，整个顺序表初始化的过程被封装到了一个函数中，此函数返回值是一个已经初始化完成的顺序表。这样做的好处是增加了代码的可用性，也更加美观。与此同时，顺序表初始化过程中，要注意对物理空间的申请进行判断，对申请失败的情况进行处理，这里只进行了“输出提示信息和强制退出”的操作，可以根据你自己的需要对代码中的 if 语句进行改进。

通过在主函数中调用 initTable 语句，就可以成功创建一个空的顺序表，与此同时我们还可以试着向顺序表中添加一些元素，C 语言实现代码如下：

```C
#include <stdio.h>
#include <stdlib.h>  //malloc()、exit()
#define Size 5
typedef struct Table {    
    int * head;    
    int length;    
    int size;
}table;
table initTable() {    
    table t;    
    t.head = (int*)malloc(Size * sizeof(int));    
    if (!t.head)    {        
        printf("初始化失败");        
        exit(0);    
    }    
    t.length = 0;    
    t.size = Size;    
    return t;
}
//输出顺序表中元素的函数
void displayTable(table t) {    
    int i;    
    for (i = 0; i < t.length; i++) {        
        printf("%d ", t.head[i]);    
    }    
    printf("\n");}
int main() {    
    int i;    
    table t = initTable();    
    //向顺序表中添加元素    
    for (i = 1; i <= Size; i++) {        
        t.head[i - 1] = i;        
        t.length++;    
    }    
    printf("顺序表中存储的元素分别是：\n");    
    displayTable(t);    
    return 0;
}
```

程序运行结果如下：

顺序表中存储的元素分别是：
1 2 3 4 5

可以看到，顺序表初始化成功。



我们学习了

[顺序表](http://data.biancheng.net/view/158.html)

及初始化的过程，本节学习有关顺序表的一些基本操作，以及如何使用 C 语言实现它们。

## 顺序表插入元素

向已有顺序表中插入数据元素，根据插入位置的不同，可分为以下 3 种情况：

1. 插入到顺序表的表头；
2. 在表的中间位置插入元素；
3. 尾随顺序表中已有元素，作为顺序表中的最后一个元素；


虽然数据元素插入顺序表中的位置有所不同，但是都使用的是同一种方式去解决，即：通过遍历，找到数据元素要插入的位置，然后做如下两步工作：

- 将要插入位置元素以及后续的元素整体向后移动一个位置；
- 将元素放到腾出来的位置上；


例如，在 `{1,2,3,4,5}` 的第 3 个位置上插入元素 6，实现过程如下：

- 遍历至顺序表存储第 3 个数据元素的位置，如[图](http://data.biancheng.net/view/200.html) 1 所示：


![找到目标元素位置](http://data.biancheng.net/uploads/allimg/181122/2-1Q122201300X8.gif)
图 1 找到目标元素位置

- 将元素 3 以及后续元素 4 和 5 整体向后移动一个位置，如图 2 所示：


![将插入位置腾出](http://data.biancheng.net/uploads/allimg/181122/2-1Q122201355232.gif)
图 2 将插入位置腾出

- 将新元素 6 放入腾出的位置，如图 3 所示：


![插入目标元素](http://data.biancheng.net/uploads/allimg/181122/2-1Q12220142H50.gif)
图 3 插入目标元素


因此，顺序表插入数据元素的 C 语言实现代码如下：

```
//插入函数，其中，elem为插入的元素，add为插入到顺序表的位置table addTable(table t, int elem, int add){    int i;    //判断插入本身是否存在问题（如果插入元素位置比整张表的长度+1还大（如果相等，是尾随的情况），或者插入的位置本身不存在，程序作为提示并自动退出）    if (add > t.length + 1 || add < 1) {        printf("插入位置有问题");        return t;    }    //做插入操作时，首先需要看顺序表是否有多余的存储空间提供给插入的元素，如果没有，需要申请    if (t.length == t.size) {        t.head = (int *)realloc(t.head, (t.size + 1) * sizeof(int));        if (!t.head) {            printf("存储分配失败");            return t;        }        t.size += 1;    }    //插入操作，需要将从插入位置开始的后续元素，逐个后移    for (i = t.length - 1; i >= add - 1; i--) {        t.head[i + 1] = t.head[i];    }    //后移完成后，直接将所需插入元素，添加到顺序表的相应位置    t.head[add - 1] = elem;    //由于添加了元素，所以长度+1    t.length++;    return t;}
```

注意，动态

[数组](http://data.biancheng.net/view/181.html)

额外申请更多物理空间使用的是 realloc 函数。并且，在实现后续元素整体后移的过程，目标位置其实是有数据的，还是 3，只是下一步新插入元素时会把旧元素直接覆盖。

## 顺序表删除元素

从顺序表中删除指定元素，实现起来非常简单，只需找到目标元素，并将其后续所有元素整体前移 1 个位置即可。

后续元素整体前移一个位置，会直接将目标元素删除，可间接实现删除元素的目的。

例如，从 `{1,2,3,4,5}` 中删除元素 3 的过程如图 4 所示：


![img](http://data.biancheng.net/uploads/allimg/181122/2-1Q122201629521.gif)
图 4 顺序表删除元素的过程示意图


因此，顺序表删除元素的 C 语言实现代码为：

```
table delTable(table t, int add) {    int i;    if (add > t.length || add < 1) {        printf("被删除元素的位置有误");        exit(0);    }    //删除操作    for (i = add; i < t.length; i++) {        t.head[i - 1] = t.head[i];    }    t.length--;    return t;}
```

## 顺序表查找元素

顺序表中查找目标元素，可以使用多种查找算法实现，比如说

[二分查找](http://data.biancheng.net/view/55.html)

算法、插值查找算法等。

这里，我们选择

[顺序查找](http://data.biancheng.net/view/54.html)

算法，具体实现代码为：

```
//查找函数，其中，elem表示要查找的数据元素的值int selectTable(table t, int elem) {    int i;    for (i = 0; i < t.length; i++) {        if (t.head[i] == elem) {            return i + 1;        }    }    return -1;//如果查找失败，返回-1}
```

## 顺序表更改元素

顺序表更改元素的实现过程是：

1. 找到目标元素；
2. 直接修改该元素的值；


顺序表更改元素的 C 语言实现代码为：

```
//更改函数，其中，elem为要更改的元素，newElem为新的数据元素table amendTable(table t, int elem, int newElem) {    int add = selectTable(t, elem);    t.head[add - 1] = newElem;//由于返回的是元素在顺序表中的位置，所以-1就是该元素在数组中的下标    return t;}
```


以上是顺序表使用过程中最常用的基本操作，这里给出本节完整的实现代码：

```
#include <stdio.h>#include <stdlib.h>#define Size 5typedef struct Table {    int * head;    int length;    int size;}table;table initTable() {    table t;    t.head = (int*)malloc(Size * sizeof(int));    if (!t.head)    {        printf("初始化失败");        exit(0);    }    t.length = 0;    t.size = Size;    return t;}table addTable(table t, int elem, int add){    int i;    if (add > t.length + 1 || add < 1) {        printf("插入位置有问题");        return t;    }    if (t.length >= t.size) {        t.head = (int *)realloc(t.head, (t.size + 1) * sizeof(int));        if (!t.head) {            printf("存储分配失败");        }        t.size += 1;    }    for (i = t.length - 1; i >= add - 1; i--) {        t.head[i + 1] = t.head[i];    }    t.head[add - 1] = elem;    t.length++;    return t;}table delTable(table t, int add) {    int i;    if (add > t.length || add < 1) {        printf("被删除元素的位置有误");        exit(0);    }    for (i = add; i < t.length; i++) {        t.head[i - 1] = t.head[i];    }    t.length--;    return t;}int selectTable(table t, int elem) {    int i;    for (i = 0; i < t.length; i++) {        if (t.head[i] == elem) {            return i + 1;        }    }    return -1;}table amendTable(table t, int elem, int newElem) {    int add = selectTable(t, elem);    t.head[add - 1] = newElem;    return t;}void displayTable(table t) {    int i;    for (i = 0; i < t.length; i++) {        printf("%d ", t.head[i]);    }    printf("\n");}int main() {    int i, add;    table t1 = initTable();    for (i = 1; i <= Size; i++) {        t1.head[i - 1] = i;        t1.length++;    }    printf("原顺序表：\n");    displayTable(t1);    printf("删除元素1:\n");    t1 = delTable(t1, 1);    displayTable(t1);    printf("在第2的位置插入元素5:\n");    t1 = addTable(t1, 5, 2);    displayTable(t1);    printf("查找元素3的位置:\n");    add = selectTable(t1, 3);    printf("%d\n", add);    printf("将元素3改为6:\n");    t1 = amendTable(t1, 3, 6);    displayTable(t1);    return 0;}
```

程序运行结果为：

原顺序表：
1 2 3 4 5
删除元素1:
2 3 4 5
在第2的位置插入元素5:
2 5 3 4 5
查找元素3的位置:
3
将元素3改为6:
2 5 6 4 5