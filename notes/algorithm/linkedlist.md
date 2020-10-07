链表，别名链式存储结构或单链表，用于存储逻辑关系为 "一对一" 的数据。与顺序表不同，链表不限制数据的物理存储状态，换句话说，使用链表存储的数据元素，其物理存储位置是随机的。

例如，使用链表存储 `{1,2,3}`，数据的物理存储状态如图 1 所示：

![链表随机存储数据](https://raw.githubusercontent.com/lindage1994/images/master/typora202009/28/191209-420219.gif)
图 1 链表随机存储数据


我们看到，图 1 根本无法体现出各数据之间的逻辑关系。对此，链表的解决方案是，每个数据元素在存储时都配备一个指针，用于指向自己的直接后继元素。如图 2 所示：

![各数据元素配备指针](https://raw.githubusercontent.com/lindage1994/images/master/typora202009/28/191218-786435.gif)
图 2 各数据元素配备指针


像图 2 这样，数据元素随机存储，并通过指针表示数据之间逻辑关系的存储结构就是链式存储结构。

## 链表的节点

从图 2 可以看到，链表中每个数据的存储都由以下两部分组成：

1. 数据元素本身，其所在的区域称为数据域；
2. 指向直接后继元素的指针，所在的区域称为指针域；


即链表中存储各数据元素的结构如图 3 所示：

![img](https://raw.githubusercontent.com/lindage1994/images/master/typora202009/28/191229-384621.gif)
图 3 节点结构


图 3 所示的结构在链表中称为节点。也就是说，链表实际存储的是一个一个的节点，真正的数据元素包含在这些节点中，如图 4 所示：

![链表中的节点](https://raw.githubusercontent.com/lindage1994/images/master/typora202009/28/191238-736805.gif)
图 4 链表中的节点


因此，链表中每个节点的具体实现，需要使用 C 语言中的结构体，具体实现代码为：

```c
typedef struct Link{  
    char elem; //代表数据域  
    struct Link * next; //代表指针域，指向直接后继元素
}link; //link为节点名，每个节点都是一个 link 结构体
```

提示，由于指针域中的指针要指向的也是一个节点，因此要声明为 Link 类型（这里要写成 `struct Link*` 的形式）。

## 头节点，头指针和首元节点

其实，图 4 所示的链表结构并不完整。一个完整的链表需要由以下几部分构成：

1. 头指针：一个普通的指针，它的特点是永远指向链表第一个节点的位置。很明显，头指针用于指明链表的位置，便于后期找到链表并使用表中的数据；

2. 节点

   ：链表中的节点又细分为头节点、首元节点和其他节点：

   - 头节点：其实就是一个不存任何数据的空节点，通常作为链表的第一个节点。对于链表来说，头节点不是必须的，它的作用只是为了方便解决某些实际问题；
- 首元节点：由于头节点（也就是空节点）的缘故，链表中称第一个存有数据的节点为首元节点。首元节点只是对链表中第一个存有数据节点的一个称谓，没有实际意义；
   - 其他节点：链表中其他的节点；


因此，一个存储 `{1,2,3}` 的完整链表结构如图 5 所示：

![完整的链表示意图](https://raw.githubusercontent.com/lindage1994/images/master/typora202009/28/191331-78994.gif)
图 5 完整的链表示意图

注意：链表中有头节点时，头指针指向头节点；反之，若链表中没有头节点，则头指针指向首元节点。

明白了链表的基本结构，下面我们来学习如何创建一个链表。

## 链表的创建（初始化）

创建一个链表需要做如下工作：

1. 声明一个头指针（如果有必要，可以声明一个头节点）；
2. 创建多个存储数据的节点，在创建的过程中，要随时与其前驱节点建立逻辑关系；


例如，创建一个存储 `{1,2,3,4}` 且无头节点的链表，C 语言实现代码如下：

```c
link * initLink() {    
    int i;    link * p = NULL;//创建头指针   
    link * temp = (link*)malloc(sizeof(link));//创建首元节点 
    //首元节点先初始化 
    temp->elem = 1; 
    temp->next = NULL;
    p = temp;//头指针指向首元节点  
    //从第二个节点开始创建 
    for (i = 2; i < 5; i++) {  
        //创建一个新节点并初始化   
        link *a = (link*)malloc(sizeof(link));    
        a->elem = i;   
        a->next = NULL;   
        //将temp节点与新建立的a节点建立逻辑关系    
        temp->next = a;     
        //指针temp每次都指向新链表的最后一个节点，其实就是 a节点，这里写temp=a也对   
        temp = temp->next;   
    }    //返回建立的节点，只返回头指针 p即可，通过头指针即可找到整个链表   
    return p;
}
```

如果想创建一个存储 `{1,2,3,4}` 且含头节点的链表，则 C 语言实现代码为：

```c
link * initLink(){  
    int i;  
    link * p=(link*)malloc(sizeof(link));//创建一个头结点 
    link * temp=p;//声明一个指针指向头结点，
    //生成链表  
    for (i=1; i<5; i++) {   
        link *a=(link*)malloc(sizeof(link));    
        a->elem=i;     
        a->next=NULL;    
        temp->next=a;    
        temp=temp->next; 
    }   
    return p;
}
```


我们只需在主函数中调用 initLink 函数，即可轻松创建一个存储 `{1,2,3,4}` 的链表，C 语言完整代码如下：

```c
#include <stdio.h>
#include <stdlib.h>
//链表中节点的结构
typedef struct Link {  
    int  elem;  
    struct Link *next;
}link;
//初始化链表的函数
link * initLink();
//用于输出链表的函数
void display(link *p);
int main() {  
    link*p = NULL;  
    //初始化链表（1，2，3，4） 
    printf("初始化链表为：\n"); 
    p = initLink();  
    display(p);  
    return 0;
}
link * initLink() {  
    int i; 
    link * p = NULL;//创建头指针  
    link * temp = (link*)malloc(sizeof(link));//创建首元节点  
    //首元节点先初始化   
    temp->elem = 1; 
    temp->next = NULL;  
    p = temp;//头指针指向首元节点  
    for (i = 2; i < 5; i++) {   
        link *a = (link*)malloc(sizeof(link));    
        a->elem = i;    
        a->next = NULL;    
        temp->next = a;    
        temp = temp->next;   
    }   
    return p;
}
void display(link *p) {  
    link* temp = p;//将temp指针重新指向头结点   
    //只要temp指针指向的结点的next不是Null，就执行输出语句。  
    while (temp) {   
        printf("%d ", temp->elem);   
        temp = temp->next;  
    }    
    printf("\n");
}
```

程序运行结果为：

初始化链表为：
1 2 3 4


注意，如果使用带有头节点创建链表的方式，则输出链表的 display 函数需要做适当地修改：

```c
void display(link *p){   
    link* temp=p;//将temp指针重新指向头结点   
    //只要temp指针指向的结点的next不是Null，就执行输出语句。  
    while (temp->next) {     
        temp=temp->next;   
        printf("%d",temp->elem); 
    }  
    printf("\n");
}
```

链表存储数据元素，以及如何使用 C 语言创建链表。本节将详细介绍对链表的一些基本操作，包括对链表中数据的添加、删除、查找（遍历）和更改。

注意，以下对链表的操作实现均建立在已创建好链表的基础上，创建链表的代码如下所示：

```c
//声明节点结构
typedef struct Link {   
    int  elem;//存储整形元素   
    struct Link *next;//指向直接后继元素的指针
}link;

//创建链表的函数
link * initLink() {  
    link * p = (link*)malloc(sizeof(link));//创建一个头结点  
    link * temp = p;//声明一个指针指向头结点，用于遍历链表  
    int i = 0;    //生成链表   
    for (i = 1; i < 5; i++) {    
        //创建节点并初始化      
        link *a = (link*)malloc(sizeof(link));  
        a->elem = i;     
        a->next = NULL;    
        //建立新节点与直接前驱节点的逻辑关系     
        temp->next = a;    
        temp = temp->next;  
    }  
    return p;
}
```

从实现代码中可以看到，该链表是一个具有头节点的链表。由于头节点本身不用于存储数据，因此在实现对链表中数据的"增删查改"时要引起注意。

## 链表插入元素

同顺序表一样，向链表中增添元素，根据添加位置不同，可分为以下 3 种情况：

- 插入到链表的头部（头节点之后），作为首元节点；
- 插入到链表中间的某个位置；
- 插入到链表的最末端，作为链表中最后一个数据元素；


虽然新元素的插入位置不固定，但是链表插入元素的思想是固定的，只需做以下两步操作，即可将新元素插入到指定的位置：

1. 将新结点的 next 指针指向插入位置后的结点；
2. 将插入位置前结点的 next 指针指向插入结点；


例如，我们在链表 `{1,2,3,4}` 的基础上分别实现在头部、中间部位、尾部插入新元素 5，其实现过程如图

 1 所示：

![链表中插入元素的 3 种情况示意图](https://raw.githubusercontent.com/lindage1994/images/master/typora202009/28/191926-387588.gif)
图 1 链表中插入元素的 3 种情况示意图


从图中可以看出，虽然新元素的插入位置不同，但实现插入操作的方法是一致的，都是先执行步骤 1 ，再执行步骤 2。

注意：链表插入元素的操作必须是先步骤 1，再步骤 2；反之，若先执行步骤 2，会导致插入位置后续的部分链表丢失，无法再实现步骤 1。

通过以上的讲解，我们可以尝试编写 C 语言代码来实现链表插入元素的操作：

```c
//p为原链表，elem表示新数据元素，add表示新元素要插入的位置
link * insertElem(link * p, int elem, int add) {  
    link * temp = p;//创建临时结点temp  
    link * c = NULL;   
    int i = 0;   
    //首先找到要插入位置的上一个结点  
    for (i = 1; i < add; i++) {   
        if (temp == NULL) {       
            printf("插入位置无效\n");      
            return p;     
        }     
        temp = temp->next;
    }  
    //创建插入结点c 
    c = (link*)malloc(sizeof(link)); 
    c->elem = elem;  
    //向链表中插入结点  
    c->next = temp->next;  
    temp->next = c;   
    return  p;
}
```

提示，insertElem 函数中加入一个 if 语句，用于判断用户输入的插入位置是否有效。例如，在已存储 `{1,2,3}` 的链表中，用户要求在链表中第 100 个数据元素所在的位置插入新元素，显然用户操作无效，此时就会触发 if 语句。

## 链表删除元素

从链表中删除指定数据元素时，实则就是将存有该数据元素的节点从链表中摘除，但作为一名合格的程序员，要对存储空间负责，对不再利用的存储空间要及时释放。因此，从链表中删除数据元素需要进行以下 2 步操作：

1. 将结点从链表中摘下来;
2. 手动释放掉结点，回收被结点占用的存储空间;


其中，从链表上摘除某节点的实现非常简单，只需找到该节点的直接前驱节点 temp，执行一行程序：

temp->next=temp->next->next;

例如，从存有 `{1,2,3,4}` 的链表中删除元素 3，则此代码的执行效果如图 2 所示：

![链表删除元素示意图](https://raw.githubusercontent.com/lindage1994/images/master/typora202009/28/192201-315665.gif)
图 2 链表删除元素示意图


因此，链表删除元素的 C 语言实现如下所示：

```c
//p为原链表，add为要删除元素的值
link * delElem(link * p, int add) {  
    link * temp = p;  
    link * del = NULL;  
    int i = 0;   
    //temp指向被删除结点的上一个结点  
    for (i = 1; i < add; i++) {  
        temp = temp->next; 
    } 
    del = temp->next;//单独设置一个指针指向被删除结点，以防丢失   
    temp->next = temp->next->next;//删除某个结点的方法就是更改前一个结点的指针域  
    free(del);//手动释放该结点，防止内存泄漏 
    return p;
}
```

我们可以看到，从链表上摘下的节点 del 最终通过 free 函数进行了手动释放。

## 链表查找元素

在链表中查找指定数据元素，最常用的方法是：从表头依次遍历表中节点，用被查找元素与各节点数据域中存储的数据元素进行比对，直至比对成功或遍历至链表最末端的 `NULL`（比对失败的标志）。

因此，链表中查找特定数据元素的 C 语言实现代码为：

```c
//p为原链表，elem表示被查找元素、
int selectElem(link * p, int elem) {   
    //新建一个指针t，初始化为头指针 p  
    link * t = p;  
    int i = 1;  
    //由于头节点的存在，因此while中的判断为t->next  
    while (t->next) {   
        t = t->next;  
        if (t->elem == elem) {    
            return i;  
        }    
        i++;   
    }  
    //程序执行至此处，表示查找失败  
    return -1;
}
```

注意，遍历有头节点的链表时，需避免头节点对测试数据的影响，因此在遍历链表时，建立使用上面代码中的遍历方法，直接越过头节点对链表进行有效遍历。

## 链表更新元素

更新链表中的元素，只需通过遍历找到存储此元素的节点，对节点中的数据域做更改操作即可。

直接给出链表中更新数据元素的 C 语言实现代码：

```c
//更新函数，其中，add 表示更改结点在链表中的位置，newElem 为新的数据域的值
link *amendElem(link * p, int add, int newElem) {  
    int i = 0;  
    link * temp = p;   
    temp = temp->next;//在遍历之前，temp指向首元结点   
    //遍历到被删除结点  
    for (i = 1; i < add; i++) {    
        temp = temp->next;   
    }  
    temp->elem = newElem; 
    return p;
}
```

## 总结

以上内容详细介绍了对链表中数据元素做"增删查改"的实现过程及 C 语言代码，在此给出本节的完整可运行代码：

```c
#include <stdio.h>
#include <stdlib.h>

typedef struct Link {
    int  elem;
    struct Link *next;
}link;
link * initLink();
//链表插入的函数，p是链表，elem是插入的结点的数据域，add是插入的位置
link * insertElem(link * p, int elem, int add);
//删除结点的函数，p代表操作链表，add代表删除节点的位置
link * delElem(link * p, int add);
//查找结点的函数，elem为目标结点的数据域的值
int selectElem(link * p, int elem);
//更新结点的函数，newElem为新的数据域的值
link *amendElem(link * p, int add, int newElem);
void display(link *p);

int main() {
    link *p = NULL;
    int address;
    //初始化链表（1，2，3，4）
    printf("初始化链表为：\n");
    p = initLink();
    display(p);

    printf("在第4的位置插入元素5：\n");
    p = insertElem(p, 5, 4);
    display(p);

    printf("删除元素3:\n");
    p = delElem(p, 3);
    display(p);

    printf("查找元素2的位置为：\n");
    address = selectElem(p, 2);
    if (address == -1) {
        printf("没有该元素");
    }
    else {
        printf("元素2的位置为：%d\n", address);
    }
    printf("更改第3的位置上的数据为7:\n");
    p = amendElem(p, 3, 7);
    display(p);

    return 0;
}

link * initLink() {
    link * p = (link*)malloc(sizeof(link));//创建一个头结点
    link * temp = p;//声明一个指针指向头结点，用于遍历链表
    int i = 0;
    //生成链表
    for (i = 1; i < 5; i++) {
        link *a = (link*)malloc(sizeof(link));
        a->elem = i;
        a->next = NULL;
        temp->next = a;
        temp = temp->next;
    }
    return p;
}
link * insertElem(link * p, int elem, int add) {
    link * temp = p;//创建临时结点temp
    link * c = NULL;
    int i = 0;
    //首先找到要插入位置的上一个结点
    for (i = 1; i < add; i++) {
        if (temp == NULL) {
            printf("插入位置无效\n");
            return p;
        }
        temp = temp->next;
    }
    //创建插入结点c
    c = (link*)malloc(sizeof(link));
    c->elem = elem;
    //向链表中插入结点
    c->next = temp->next;
    temp->next = c;
    return  p;
}

link * delElem(link * p, int add) {
    link * temp = p;
    link * del = NULL;
    int i = 0;
    //遍历到被删除结点的上一个结点
    for (i = 1; i < add; i++) {
        temp = temp->next;
    }
    del = temp->next;//单独设置一个指针指向被删除结点，以防丢失
    temp->next = temp->next->next;//删除某个结点的方法就是更改前一个结点的指针域
    free(del);//手动释放该结点，防止内存泄漏
    return p;
}
int selectElem(link * p, int elem) {
    link * t = p;
    int i = 1;
    while (t->next) {
        t = t->next;
        if (t->elem == elem) {
            return i;
        }
        i++;
    }
    return -1;
}
link *amendElem(link * p, int add, int newElem) {
    int i = 0;
    link * temp = p;
    temp = temp->next;//tamp指向首元结点
    //temp指向被删除结点
    for (i = 1; i < add; i++) {
        temp = temp->next;
    }
    temp->elem = newElem;
    return p;
}
void display(link *p) {
    link* temp = p;//将temp指针重新指向头结点
    //只要temp指针指向的结点的next不是Null，就执行输出语句。
    while (temp->next) {
        temp = temp->next;
        printf("%d ", temp->elem);
    }
    printf("\n");
}
```

代码运行结果：

初始化链表为：
1 2 3 4
在第4的位置插入元素5：
1 2 3 5 4
删除元素3:
1 2 5 4
查找元素2的位置为：
元素2的位置为：2
更改第3的位置上的数据为7:
1 2 7 4

是否存在一种存储结构，可以融合顺序表和链表

各自的优点，从而既能快速访问元素，又能快速增加或删除数据元素。

静态链表，也是线性存储结构的一种，它兼顾了顺序表和链表的优点于一身，可以看做是顺序表和链表的升级版。

使用静态链表存储数据，数据全部存储在数组中（和顺序表一样），但存储位置是随机的，数据之间"一对一"的逻辑关系通过一个整形变量（称为"游标"，和指针功能类似）维持（和链表类似）。

例如，使用静态链表存储 `{1,2,3}` 的过程如下：

创建一个足够大的数组，假设大小为 6，如图 1 所示：

![空数组](https://raw.githubusercontent.com/lindage1994/images/master/typora202009/28/192629-719908.gif)
图 1 空数组


接着，在将数据存放到数组中时，给各个数据元素配备一个整形变量，此变量用于指明各个元素的直接后继元素所在数组中的位置下标，如图 2 所示：

![静态链表存储数据](https://raw.githubusercontent.com/lindage1994/images/master/typora202009/28/192703-23627.gif)
图 2 静态链表存储数据

通常，静态链表会将第一个数据元素放到数组下标为 1 的位置（a[1]）中。

图 2 中，从 a[1] 存储的数据元素 1 开始，通过存储的游标变量 3，就可以在 a[3] 中找到元素 1 的直接后继元素 2；同样，通过元素 a[3] 存储的游标变量 5，可以在 a[5] 中找到元素 2 的直接后继元素 3，这样的循环过程直到某元素的游标变量为 0 截止（因为 a[0] 默认不存储数据元素）。

类似图 2 这样，通过 "数组+游标" 的方式存储具有线性关系数据的存储结构就是静态链表。

## 静态链表中的节点

通过上面的学习我们知道，静态链表存储数据元素也需要自定义数据类型，至少需要包含以下 2 部分信息：

- 数据域：用于存储数据元素的值；
- 游标：其实就是数组下标，表示直接后继元素所在数组中的位置；


因此，静态链表中节点的构成用 C 语言实现为：

```c
typedef struct {
    int data;//数据域   
    int cur;//游标
}component;
```

## 备用链表

图 2 显示的静态链表还不够完整，静态链表中，除了数据本身通过游标组成的链表外，还需要有一条连接各个空闲位置的链表，称为备用链表。

备用链表的作用是回收数组中未使用或之前使用过（目前未使用）的存储空间，留待后期使用。也就是说，静态链表使用数组申请的物理空间中，存有两个链表，一条连接数据，另一条连接数组中未使用的空间。

通常，备用链表的表头位于数组下标为 0（a[0]） 的位置，而数据链表的表头位于数组下标为 1（a[1]）的位置。

静态链表中设置备用链表的好处是，可以清楚地知道数组中是否有空闲位置，以便数据链表添加新数据时使用。比如，若静态链表中数组下标为 0 的位置上存有数据，则证明数组已满。

例如，使用静态链表存储 `{1,2,3}`，假设使用长度为 6 的数组 a，则存储状态可能如图 3 所示：

![备用链表和数据链表](https://raw.githubusercontent.com/lindage1994/images/master/typora202009/28/192742-221210.gif)
图 3 备用链表和数据链表


图 3 中，备用链表上连接的依次是 a[0]、a[2] 和 a[4]，而数据链表上连接的依次是 a[1]、a[3] 和 a[5]。

## 静态链表的实现

假设使用静态链表（数组长度为 6）存储 `{1,2,3}`，则需经历以下几个阶段。

在数据链表未初始化之前，数组中所有位置都处于空闲状态，因此都应被链接在备用链表上，如图 4 所示：

![未存储数据之前静态链表的状态](https://raw.githubusercontent.com/lindage1994/images/master/typora202009/28/192751-270327.gif)
图 4 未存储数据之前静态链表的状态


当向静态链表中添加数据时，需提前从备用链表中摘除节点，以供新数据使用。

备用链表摘除节点最简单的方法是摘除 a[0] 的直接后继节点；同样，向备用链表中添加空闲节点也是添加作为 a[0] 新的直接后继节点。因为 a[0] 是备用链表的第一个节点，我们知道它的位置，操作它的直接后继节点相对容易，无需遍历备用链表，耗费的时间复杂度为 `O(1)`。

因此，在图 4 的基础上，向静态链表中添加元素 1 的过程如图 5 所示：

![静态链表中添加元素 1](https://raw.githubusercontent.com/lindage1994/images/master/typora202009/28/192812-984973.gif)
图 5 静态链表中添加元素 1


在图 5 的基础上，添加元素 2 的过程如图 6 所示：

![静态链表中继续添加元素 2](https://raw.githubusercontent.com/lindage1994/images/master/typora202009/28/192903-115611.gif)
图 6 静态链表中继续添加元素 2


在图 6 的基础上，继续添加元素 3 ，过程如图 7 所示：

![静态链表中继续添加元素 3](https://raw.githubusercontent.com/lindage1994/images/master/typora202009/28/192914-958379.gif)
图 7 静态链表中继续添加元素 3


由此，静态链表就创建完成了。

下面给出了创建静态链表的 C 语言实现代码：

```c
#include <stdio.h>
#define maxSize 6
typedef struct {   
    int data; 
    int cur;
}component;
//将结构体数组中所有分量链接到备用链表中
void reserveArr(component *array);
//初始化静态链表
int initArr(component *array);
//输出函数
void displayArr(component * array, int body);
//从备用链表上摘下空闲节点的函数
int mallocArr(component * array);
int main() {  
    component array[maxSize];  
    int body = initArr(array); 
    printf("静态链表为：\n");  
    displayArr(array, body); 
    return 0;
}
//创建备用链表
void reserveArr(component *array) { 
    int i = 0; 
    for (i = 0; i < maxSize; i++) {    
        array[i].cur = i + 1;//将每个数组分量链接到一起  
        array[i].data = 0;   
    }   
    array[maxSize - 1].cur = 0;//链表最后一个结点的游标值为0
}
//提取分配空间
int mallocArr(component * array) {   
    //若备用链表非空，则返回分配的结点下标，否则返回 0（当分配最后一个结点时，该结点的游标值为 0） 
    int i = array[0].cur;  
    if (array[0].cur) {  
        array[0].cur = array[i].cur;   
    }   
    return i;
}
//初始化静态链表
int initArr(component *array) { 
    int tempBody = 0, body = 0;   
    int i = 0;  
    reserveArr(array); 
    body = mallocArr(array); 
    //建立首元结点   
    array[body].data = 1;   
    array[body].cur = 0;  
    //声明一个变量，把它当指针使，指向链表的最后的一个结点，当前和首元结点重合 
    tempBody = body;  
    for (i = 2; i < 4; i++) {  
        int j = mallocArr(array); //从备用链表中拿出空闲的分量    
        array[j].data = i;  
        //初始化新得到的空间结点     
        array[tempBody].cur = j; //将新得到的结点链接到数据链表的尾部    
        tempBody = j;        
        //将指向链表最后一个结点的指针后移    
    }  
    array[tempBody].cur = 0;//新的链表最后一个结点的指针设置为0  
    return body;
}
void displayArr(component * array, int body) {  
    int tempBody = body;//tempBody准备做遍历使用  
    while (array[tempBody].cur) {     
        printf("%d,%d\n", array[tempBody].data, array[tempBody].cur);  
        tempBody = array[tempBody].cur;  
    }  
    printf("%d,%d\n", array[tempBody].data, array[tempBody].cur);
}
```

代码输出结果为：

静态链表为：
1,2
2,3
3,0

由此，我们就成功创建了一个不带头结点的静态链表（如图 7 所示），感兴趣的读者可自行尝试创建一个带有头结点的静态链表。

上节，我们初步创建了一个静态链表，本节学习有关静态链表的一些基本操作，包括对表中数据元素的添加、删除、查找和更改。

本节是建立在已能成功创建静态链表的基础上，因此我们继续使用上节中已建立好的静态链表学习本节内容，建立好的静态链表如图1 所示：

![建立好的静态链表](https://raw.githubusercontent.com/lindage1994/images/master/typora202010/07/201412-645937.gif)
图 1 建立好的静态链表

## 静态链表添加元素

例如，在图 1 的基础，将元素 4 添加到静态链表中的第 3 个位置上，实现过程如下：

1. 从备用链表中摘除一个节点，用于存储元素 4；
2. 找到表中第 2 个节点（添加位置的前一个节点，这里是数据元素 2），将元素 2 的游标赋值给新元素 4；
3. 将元素 4 所在数组中的下标赋值给元素 2 的游标；


经过以上几步操作，数据元素 4 就成功地添加到了静态链表中，此时新的静态链表如图 2 所示：

![img](https://raw.githubusercontent.com/lindage1994/images/master/typora202010/07/201720-940255.gif)
图 2 添加元素 4 的静态链表


由此，我们通过尝试编写 C 语言程序实现以上操作。读者可参考如下程序：

```c
//向链表中插入数据，body表示链表的头结点在数组中的位置，add表示插入元素的位置，num表示要插入的数据
void insertArr(component * array, int body, int add, int num) { 
    int tempBody = body;//tempBody做遍历结构体数组使用   
    int i = 0, insert = 0;  
    //找到要插入位置的上一个结点在数组中的位置  
    for (i = 1; i < add; i++) {     
        tempBody = array[tempBody].cur;  
    }   
    insert = mallocArr(array);//申请空间，准备插入  
    array[insert].data = num;  
    array[insert].cur = array[tempBody].cur;//新插入结点的游标等于其直接前驱结点的游标  
    array[tempBody].cur = insert;//直接前驱结点的游标等于新插入结点所在数组中的下标
}
```

## 静态链表删除元素

静态链表中删除指定元素，只需实现以下 2 步操作：

1. 将存有目标元素的节点从数据链表中摘除；
2. 将摘除节点添加到备用链表，以便下次再用；


比较特殊的是，对于无头结点的数据链表来说，如果需要删除头结点，则势必会导致数据链表的表头不再位于数组下标为 1 的位置，换句话说，删除头结点之后，原数据链表中第二个结点将作为整个链表新的首元结点。

若问题中涉及大量删除元素的操作，建议读者在建立静态链表之初创建一个带有头节点的静态链表，方便实现删除链表中第一个数据元素的操作。

如下是针对无头结点的数据链表，实现删除操作的 C 语言代码：

```c
//删除结点函数，num表示被删除结点中数据域存放的数据，函数返回新数据链表的表头位置
int deletArr(component * array, int body, int num) { 
    int tempBody = body;  
    int del = 0;  
    int newbody = 0;  
    //找到被删除结点的位置  
    while (array[tempBody].data != num) {  
        tempBody = array[tempBody].cur;   
        //当tempBody为0时，表示链表遍历结束，说明链表中没有存储该数据的结点    
        if (tempBody == 0) {    
            printf("链表中没有此数据");    
            return;    
        }   
    }  
    //运行到此，证明有该结点  
    del = tempBody;    
    tempBody = body;  
    //删除首元结点，需要特殊考虑  
    if (del == body) {     
        newbody = array[del].cur;   
        freeArr(array, del);      
        return newbody;   
    }    else    {     
        //找到该结点的上一个结点，做删除操作    
        while (array[tempBody].cur != del) {    
            tempBody = array[tempBody].cur;  
        }     
        //将被删除结点的游标直接给被删除结点的上一个结点   
        array[tempBody].cur = array[del].cur; 
        //回收被摘除节点的空间      
        freeArr(array, del);    
        return body;    
    }  
}
```

## 静态链表查找元素

静态链表查找指定元素，由于我们只知道静态链表第一个元素所在数组中的位置，因此只能通过逐个遍历静态链表的方式，查找存有指定数据元素的节点。

静态链表查找指定数据元素的 C 语言实现代码如下：

```c
//在以body作为头结点的链表中查找数据域为elem的结点在数组中的位置
int selectNum(component * array, int body, int num) {   
    //当游标值为0时，表示链表结束   
    while (array[body].cur != 0) {   
        if (array[body].data == num) { 
            return body;   
        }     
        body = array[body].cur;  
    }    //判断最后一个结点是否符合要求  
    if (array[body].data == num) {   
        return body;   
    }   
    return -1;//返回-1，表示在链表中没有找到该元素
}
```

## 静态链表中更改数据

更改静态链表中的数据，只需找到目标元素所在的节点，直接更改节点中的数据域即可。

实现此操作的 C 语言代码如下：

```c
//在以body作为头结点的链表中将数据域为oldElem的结点，数据域改为
newElemvoid amendElem(component * array, int body, int oldElem, int newElem) {  
    int add = selectNum(array, body, oldElem);  
    if (add == -1) {   
        printf("无更改元素");   
        return;  
    }   
    array[add].data = newElem;
}
```

## 总结

这里给出以上对静态链表做 "增删查改" 操作的完整实现代码：

```c
#include <stdio.h>
#define maxSize 7
typedef struct {
    int data;
    int cur;
}component;
//将结构体数组中所有分量链接到备用链表中
void reserveArr(component *array);
//初始化静态链表
int initArr(component *array);
//向链表中插入数据，body表示链表的头结点在数组中的位置，add表示插入元素的位置，num表示要插入的数据
void insertArr(component * array, int body, int add, int num);
//删除链表中存有num的结点,返回新数据链表中第一个节点所在的位置
int deletArr(component * array, int body, int num);
//查找存储有num的结点在数组的位置
int selectNum(component * array, int body, int num);
//将链表中的字符oldElem改为newElem
void amendElem(component * array, int body, int oldElem, int newElem);
//输出函数
void displayArr(component * array, int body);
//从备用链表中摘除空闲节点的实现函数
int mallocArr(component * array);
//将摘除下来的节点链接到备用链表上
void freeArr(component * array, int k);

int main() {
    component array[maxSize];
    int body = initArr(array);
    int selectAdd;
    printf("静态链表为：\n");
    displayArr(array, body);

    printf("在第3的位置上插入元素4:\n");
    insertArr(array, body, 3, 4);
    displayArr(array, body);

    printf("删除数据域为1的结点:\n");
    body = deletArr(array, body, 1);
    displayArr(array, body);

    printf("查找数据域为4的结点的位置:\n");
    selectAdd = selectNum(array, body, 4);
    printf("%d\n", selectAdd);
    printf("将结点数据域为4改为5:\n");
    amendElem(array, body, 4, 5);
    displayArr(array, body);
    return 0;
}
//创建备用链表
void reserveArr(component *array) {
    int i = 0;
    for (i = 0; i < maxSize; i++) {
        array[i].cur = i + 1;//将每个数组分量链接到一起
    }
    array[maxSize - 1].cur = 0;//链表最后一个结点的游标值为0
}

//初始化静态链表
int initArr(component *array) {
    int tempBody = 0, body = 0;
    int i = 0;
    reserveArr(array);
    body = mallocArr(array);
    //建立首元结点
    array[body].data = 1;
    array[body].cur = 0;
    //声明一个变量，把它当指针使，指向链表的最后的一个结点，当前和首元结点重合
    tempBody = body;
    for (i = 2; i < 4; i++) {
        int j = mallocArr(array); //从备用链表中拿出空闲的分量
        array[j].data = i;      //初始化新得到的空间结点
        array[tempBody].cur = j; //将新得到的结点链接到数据链表的尾部
        tempBody = j;             //将指向链表最后一个结点的指针后移
    }
    array[tempBody].cur = 0;//新的链表最后一个结点的指针设置为0
    return body;
}

//向链表中插入数据，body表示链表的头结点在数组中的位置，add表示插入元素的位置，num表示要插入的数据
void insertArr(component * array, int body, int add, int num) {
    int tempBody = body;//tempBody做遍历结构体数组使用
    int i = 0, insert = 0;
    //找到要插入位置的上一个结点在数组中的位置
    for (i = 1; i < add; i++) {
        tempBody = array[tempBody].cur;
    }
    insert = mallocArr(array);//申请空间，准备插入
    array[insert].data = num;

    array[insert].cur = array[tempBody].cur;//新插入结点的游标等于其直接前驱结点的游标
    array[tempBody].cur = insert;//直接前驱结点的游标等于新插入结点所在数组中的下标
}

//删除结点函数，num表示被删除结点中数据域存放的数据
int deletArr(component * array, int body, int num) {
    int tempBody = body;
    int del = 0;
    int newbody = 0;
    //找到被删除结点的位置
    while (array[tempBody].data != num) {
        tempBody = array[tempBody].cur;
        //当tempBody为0时，表示链表遍历结束，说明链表中没有存储该数据的结点
        if (tempBody == 0) {
            printf("链表中没有此数据");
            return;
        }
    }
    //运行到此，证明有该结点
    del = tempBody;
    tempBody = body;
    //删除首元结点，需要特殊考虑
    if (del == body) {
        newbody = array[del].cur;
        freeArr(array, del);
        return newbody;
    }
    else
    {
        //找到该结点的上一个结点，做删除操作
        while (array[tempBody].cur != del) {
            tempBody = array[tempBody].cur;
        }
        //将被删除结点的游标直接给被删除结点的上一个结点
        array[tempBody].cur = array[del].cur;
        //回收被摘除节点的空间
        freeArr(array, del);
        return body;
    }  
}

//在以body作为头结点的链表中查找数据域为elem的结点在数组中的位置
int selectNum(component * array, int body, int num) {
    //当游标值为0时，表示链表结束
    while (array[body].cur != 0) {
        if (array[body].data == num) {
            return body;
        }
        body = array[body].cur;
    }
    //判断最后一个结点是否符合要求
    if (array[body].data == num) {
        return body;
    }
    return -1;//返回-1，表示在链表中没有找到该元素
}

//在以body作为头结点的链表中将数据域为oldElem的结点，数据域改为newElem
void amendElem(component * array, int body, int oldElem, int newElem) {
    int add = selectNum(array, body, oldElem);
    if (add == -1) {
        printf("无更改元素");
        return;
    }
    array[add].data = newElem;
}

void displayArr(component * array, int body) {
    int tempBody = body;//tempBody准备做遍历使用
    while (array[tempBody].cur) {
        printf("%d,%d ", array[tempBody].data, array[tempBody].cur);
        tempBody = array[tempBody].cur;
    }
    printf("%d,%d\n", array[tempBody].data, array[tempBody].cur);

}

//提取分配空间
int mallocArr(component * array) {
    //若备用链表非空，则返回分配的结点下标，否则返回0（当分配最后一个结点时，该结点的游标值为0）
    int i = array[0].cur;
    if (array[0].cur) {
        array[0].cur = array[i].cur;
    }
    return i;
}

//备用链表回收空间的函数，其中array为存储数据的数组，k表示未使用节点所在数组的下标
void freeArr(component * array, int k) {
    array[k].cur = array[0].cur;
    array[0].cur = k;
}
```

程序运行结果为：

静态链表为：
1,2 2,3 3,0
在第3的位置上插入元素4:
1,2 2,3 3,4 4,0
删除数据域为1的结点:
2,3 3,4 4,0
查找数据域为4的结点的位置:
4
将结点数据域为4改为5:
2,3 3,4 5,0

无论是静态链表还是动态链表，有时在解决具体问题时，需要我们对其结构进行稍微地调整。比如，可以把链表的两头连接，使其成为了一个环状链表，通常称为循环链表。和它名字的表意一样，只需要将表中最后一个结点的指针指向头结点，链表就能成环儿，如图1 所示。


![img](https://raw.githubusercontent.com/lindage1994/images/master/typora202010/07/201930-335273.png)

图1 循环链表

 

需要注意的是，虽然循环链表成环状，但本质上还是链表，因此在循环链表中，依然能够找到头指针和首元节点等。循环链表和普通链表相比，唯一的不同就是循环链表首尾相连，其他都完全一样。

## 循环链表实现约瑟夫环

约瑟夫环问题，是一个经典的循环链表问题，题意是：已知 n 个人（分别用编号 1，2，3，…，n 表示）围坐在一张圆桌周围，从编号为 k 的人开始顺时针报数，数到 m 的那个人出列；他的下一个人又从 1 开始，还是顺时针开始报数，数到 m 的那个人又出列；依次重复下去，直到圆桌上剩余一个人。如图 2 所示，假设此时圆周周围有 5 个人，要求从编号为 3 的人开始顺时针数数，数到 2 的那个人出列：

![循环链表实现约瑟夫环](https://raw.githubusercontent.com/lindage1994/images/master/typora202009/28/193124-314936.png)
图 2 循环链表实现约瑟夫环


出列顺序依次为：

- 编号为 3 的人开始数 1，然后 4 数 2，所以 4 先出列；
- 4 出列后，从 5 开始数 1，1 数 2，所以 1 出列；
- 1 出列后，从 2 开始数 1，3 数 2，所以 3 出列；
- 3 出列后，从 5 开始数 1，2 数 2，所以 2 出列；
- 最后只剩下 5 自己，所以 5 胜出。

约瑟夫环问题有多种变形，比如顺时针转改为逆时针等，虽然问题的细节有多种变数，但解决问题的中心思想是一样的，即使用循环链表。

通过以上的分析，我们可以尝试编写 C 语言代码，完整代码如下所示：

```c

#include <stdio.h>
#include <stdlib.h>
typedef struct node {  
    int number;  
struct node * next;
}person;
person * initLink(int n) { 
    int i = 0;  
    person * head = NULL, *cyclic = NULL;  
    head = (person*)malloc(sizeof(person));  
    head->number = 1;   
    head->next = NULL; 
    cyclic = head;  
    for (i = 2; i <= n; i++) {    
        person * body = (person*)malloc(sizeof(person));  
        body->number = i;      
        body->next = NULL;     
        cyclic->next = body;    
        cyclic = cyclic->next; 
    }   
    cyclic->next = head;//首尾相连  
    return head;
}
void findAndKillK(person * head, int k, int m) {   
    person * p = NULL; 
    person * tail = head;  
    //找到链表第一个结点的上一个结点，为删除操作做准备  
    while (tail->next != head) {  
        tail = tail->next;  
    }   
    p = head;   
    //找到编号为k的人  
    while (p->number != k) {    
        tail = p;     
        p = p->next;  
    }   
    //从编号为k的人开始，只有符合p->next==p时，说明链表中除了p结点，所有编号都出列了， 
    while (p->next != p) {    
        int i = 0;   
        //找到从p报数1开始，报m的人，并且还要知道数m-1de人的位置tail，方便做删除操作。   
        for (i = 1; i < m; i++) {      
            tail = p;   
            p = p->next;   
        }    
        tail->next = p->next;//从链表上将p结点摘下来      
        printf("出列人的编号为:%d\n", p->number);  
        free(p);    
        p = tail->next;//继续使用p指针指向出列编号的下一个编号，游戏继续  
    }   
    printf("出列人的编号为:%d\n", p->number);  
    free(p);
}
int main() {    
    int n = 0, k = 0, m = 0;  
    person * head = NULL; 
    printf("输入圆桌上的人数:");   
    scanf("%d", &n);   
    head = initLink(n); 
    printf("从第几个人开始报数(k>1且k<%d)：", n); 
    scanf("%d", &k); 
    printf("数到几的人出列：");  
    scanf("%d", &m); 
    findAndKillK(head, k, m);  
    return 0;
}
```

输出结果：

输入圆桌上的人数:5
从第几个人开始报数(k>1且k<5)：3
数到几的人出列：2
出列人的编号为:4
出列人的编号为:1
出列人的编号为:3
出列人的编号为:2
出列人的编号为:5

最后出列的人，即为胜利者。当然，你也可以改进程序，令查找出最后一个人时，输出此人胜利的信息。

## 总结

循环链表和动态链表唯一不同在于它的首尾连接，这也注定了在使用循环链表时，附带最多的操作就是遍历链表。

在遍历的过程中，尤其要注意循环链表虽然首尾相连，但并不表示该链表没有第一个节点和最后一个结点。所以，不要随意改变头指针的指向。



目前我们所学到的链表，无论是动态链表还是静态链表，表中各节点中都只包含一个指针（游标），且都统一指向直接后继节点，通常称这类链表为单向链表（或单链表）。虽然使用单链表能 100% 解决逻辑关系为 "一对一" 数据的存储问题，但在解决某些特殊问题时，单链表并不是效率最优的存储结构。比如说，某场景中需要大量地查找某结点的前趋结点，这种情况下使用单链表无疑是灾难性的，因为单链表更适合 "从前往后" 找，"从后往前" 找并不是它的强项。对于逆向查找（从后往前）相关的问题，使用本节讲解的双向链表，会更加事半功倍。双向链表，简称双链表。从名字上理解双向链表，即链表是 "双向" 的，如图 1 所示：

![双向链表结构示意图](https://raw.githubusercontent.com/lindage1994/images/master/typora202009/28/193148-177263.gif)
图 1 双向链表结构示意图

所谓双向，指的是各节点之间的逻辑关系是双向的，但通常头指针只设置一个，除非实际情况需要，可以为最后一个节点再设置一个“头指针”。

根据图 1 不难看出，双向链表中各节点包含以下 3 部分信息（如图 2 所示）：

1. 指针域：用于指向当前节点的直接前驱节点；
2. 数据域：用于存储数据元素；
3. 指针域：用于指向当前节点的直接后继节点。

![双向链表的节点构成](https://raw.githubusercontent.com/lindage1994/images/master/typora202009/28/193158-753996.gif)
图 2 双向链表的节点构成


因此，双链表的节点结构用 C 语言实现为：

```c
typedef struct line{ 
    struct line * prior; //指向直接前趋   
    int data; 
    struct line * next; //指向直接后继
}line;
```

> 读者可根据实际场景的需要，调整数据域 data 的数据类型。

## 双向链表的创建

同单链表相比，双链表仅是各节点多了一个用于指向直接前驱的指针域。因此，我们可以在单链表的基础轻松实现对双链表的创建。

和创建单链表不同的是，创建双向链表的过程中，每一个新节点都要和前驱节点之间建立两次链接，分别是：

- 将新节点的 prior 指针指向直接前驱节点；
- 将直接前驱节点的 next 指针指向新节点；


这里给出创建双向链表的 C 语言实现代码：

```c
line* initLine(line * head) {  
    int i = 0;   
    line * list = NULL;  
    //创建一个首元节点，链表的头指针为head  
    head = (line*)malloc(sizeof(line));  
    //对节点进行初始化  
    head->prior = NULL; 
    head->next = NULL;  
    head->data = 1; 
    //声明一个指向首元节点的指针，方便后期向链表中添加新创建的节点  
    list = head;   
    for (i = 2; i <= 5; i++) {   
        //创建新的节点并初始化    
        line * body = (line*)malloc(sizeof(line));   
        body->prior = NULL;    
        body->next = NULL;   
        body->data = i;  
        //新节点与链表最后一个节点建立关系    
        list->next = body;     
        body->prior = list;      
        //list永远指向链表中最后一个节点   
        list = list->next;  
    }    
    //返回新创建的链表  
    return head;
}
```


我们可以尝试着在 main 函数中输出创建的双链表，C 语言代码如下：

```c

#include <stdio.h>
#include <stdlib.h>
//节点结构
typedef struct line {  
    struct line * prior; 
    int data;  
    struct line * next;}line;
//双链表的创建函数
line* initLine(line * head);
//输出双链表的函数void display(line * head);
int main() {   
    //创建一个头指针  
    line * head = NULL;   
    //调用链表创建函数  
    head = initLine(head);   
    //输出创建好的链表  
    display(head);   
    //显示双链表的优点  
    printf("链表中第 4 个节点的直接前驱是：%d", head->next->next->next->prior->data);  
    return 0;
}
line* initLine(line * head) { 
    int i = 0; 
    line * list = NULL;  
    //创建一个首元节点，链表的头指针为head  
    head = (line*)malloc(sizeof(line));   
    //对节点进行初始化  
    head->prior = NULL;  
    head->next = NULL;  
    head->data = 1;   
    //声明一个指向首元节点的指针，方便后期向链表中添加新创建的节点 
    list = head; 
    for (i = 2; i <= 5; i++) {    
        //创建新的节点并初始化    
        line * body = (line*)malloc(sizeof(line));    
        body->prior = NULL;    
        body->next = NULL;    
        body->data = i;      
        //新节点与链表最后一个节点建立关系     
        list->next = body;  
        body->prior = list;    
        //list永远指向链表中最后一个节点    
        list = list->next;  
    }    
    //返回新创建的链表   
    return head;
}
void display(line * head) {  
    line * temp = head;  
    while (temp) {    
        //如果该节点无后继节点，说明此节点是链表的最后一个节点   
        if (temp->next == NULL) {    
            printf("%d\n", temp->data);  
        }     
        else {       
            printf("%d <-> ", temp->data);   
        }    
        temp = temp->next;  
    }
}
```

程序运行结果：

1 <-> 2 <-> 3 <-> 4 <-> 5
链表中第 4 个节点的直接前驱是：3

前面学习了如何创建一个双向链表，本节学习有关双向链表的一些基本操作，即如何在双向链表中添加、删除、查找或更改数据元素。

本节知识基于已熟练掌握双向链表创建过程的基础上，我们继续上节所创建的双向链表来学习本节内容，创建好的双向链表如图 1 所示：

![双向链表示意图](https://raw.githubusercontent.com/lindage1994/images/master/typora202010/07/202021-395314.gif)
图 1 双向链表示意图

## 双向链表添加节点

根据数据添加到双向链表中的位置不同，可细分为以下 3 种情况：

#### 1) 添加至表头

将新数据元素添加到表头，只需要将该元素与表头元素建立双层逻辑关系即可。

换句话说，假设新元素节点为 temp，表头节点为 head，则需要做以下 2 步操作即可：

1. temp->next=head; head->prior=temp;
2. 将 head 移至 temp，重新指向新的表头；


例如，将新元素 7 添加至双链表的表头，则实现过程如图 2 所示：

![添加元素至双向链表的表头](https://raw.githubusercontent.com/lindage1994/images/master/typora202010/07/202031-828853.gif)
图 2 添加元素至双向链表的表头

#### 2) 添加至表的中间位置

同单链表添加数据类似，双向链表中间位置添加数据需要经过以下 2 个步骤，如图 3 所示：

1. 新节点先与其直接后继节点建立双层逻辑关系；
2. 新节点的直接前驱节点与之建立双层逻辑关系；

![双向链表中间位置添加数据元素](https://raw.githubusercontent.com/lindage1994/images/master/typora202010/07/202045-934800.gif)
图 3 双向链表中间位置添加数据元素

#### 3) 添加至表尾

与添加到表头是一个道理，实现过程如下（如图 4 所示）：

1. 找到双链表中最后一个节点；
2. 让新节点与最后一个节点进行双层逻辑关系；

![双向链表尾部添加数据元素](https://raw.githubusercontent.com/lindage1994/images/master/typora202010/07/202055-39627.gif)
图 4 双向链表尾部添加数据元素


因此，我们可以试着编写双向链表添加数据的 C 语言代码，参考代码如下：

```c
//data 为要添加的新数据，add 为添加到链表中的位置
line * insertLine(line * head, int data, int add) {  
    //新建数据域为data的结点  
    line * temp = (line*)malloc(sizeof(line));  
    temp->data = data;  
    temp->prior = NULL; 
    temp->next = NULL;   
    //插入到链表头，要特殊考虑 
    if (add == 1) {   
        temp->next = head;   
        head->prior = temp;    
        head = temp;  
    }   
    else {    
        int i = 0;   
        line * body = head;    
        //找到要插入位置的前一个结点    
        for (i = 1; i < add - 1; i++) {    
            body = body->next;      
            if (body == NULL) {        
                printf("插入位置有误\n");      
                break;      
            }     
        }     
        if (body) {     
            //判断条件为真，说明插入位置为链表尾        
            if (body->next == NULL) {   
                body->next = temp;          
                temp->prior = body;     
            }         
            else {          
                body->next->prior = temp;        
                temp->next = body->next;    
                body->next = temp;           
                temp->prior = body;    
            }    
        }   
    }   
    return head;
}
```

## 双向链表删除节点

双链表删除结点时，只需遍历链表找到要删除的结点，然后将该节点从表中摘除即可。

例如，从图 1 基础上删除元素 2 的操作过程如图 5 所示：



![双链表删除元素操作示意图](https://raw.githubusercontent.com/lindage1994/images/master/typora202010/07/202105-308974.gif)
图 5 双链表删除元素操作示意图


双向链表删除节点的 C 语言实现代码如下：

```c
//删除结点的函数，data为要删除结点的数据域的值
line * delLine(line * head, int data) {  
    line * temp = head;   
    //遍历链表  
    while (temp) {     
        //判断当前结点中数据域和data是否相等，若相等，摘除该结点     
        if (temp->data == data) {  
            temp->prior->next = temp->next;      
            temp->next->prior = temp->prior;     
            free(temp);        
            return head;    
        }      
        temp = temp->next;  
    }   
    printf("链表中无该数据元素\n");  
    return head;
}
```

## 双向链表查找节点

通常，双向链表同单链表一样，都仅有一个头指针。因此，双链表查找指定元素的实现同单链表类似，都是从表头依次遍历表中元素。

C 语言实现代码为：

```c
//head为原双链表，elem表示被查找元素
int selectElem(line * head, int elem) {   
    //新建一个指针t，初始化为头指针 head    
    line * t = head;  
    int i = 1;  
    while (t) {  
        if (t->data == elem) {    
            return i;   
        }     
        i++;     
        t = t->next;   
    }   
    //程序执行至此处，表示查找失败  
    return -1;
}
```

## 双向链表更改节点

更改双链表中指定结点数据域的操作是在查找的基础上完成的。实现过程是：通过遍历找到存储有该数据元素的结点，直接更改其数据域即可。

实现此操作的 C 语言实现代码如下：

```c
//更新函数，其中，add 表示更改结点在双链表中的位置，newElem 为新数据的值
line *amendElem(line * p, int add, int newElem) {   
    int i = 0;   
    line * temp = p;  
    //遍历到被删除结点  
    for (i = 1; i < add; i++) {  
        temp = temp->next;     
        if (temp == NULL) {       
            printf("更改位置有误！\n");  
            break;       
        }   
    }   
    if (temp) {   
        temp->data = newElem; 
    }   
    return p;
}
```

## 总结

这里给出双链表中对数据进行 "增删查改" 操作的完整实现代码：

```c

#include <stdio.h>
#include <stdlib.h>
typedef struct line {    
    struct line * prior;    
    int data;    
    struct line * next;
}line;
//双链表的创建
line* initLine(line * head);
//双链表插入元素，add表示插入位置
line * insertLine(line * head, int data, int add);
//双链表删除指定元素
line * delLine(line * head, int data);
//双链表中查找指定元素
int selectElem(line * head, int elem);
//双链表中更改指定位置节点中存储的数据，add表示更改位置
line *amendElem(line * p, int add, int newElem);
//输出双链表的实现函数
void display(line * head);
int main() {    
    line * head = NULL;    
    //创建双链表    
    printf("初始链表为：\n");    
    head = initLine(head);    
    display(head);    
    //在表中第 3 的位置插入元素 7    
    printf("在表中第 3 的位置插入新元素 7：\n");    
    head = insertLine(head, 7, 3);    
    display(head);    
    //表中删除元素 2    
    printf("删除元素 2：\n");    
    head = delLine(head, 2);    
    display(head);    
    printf("元素 3 的位置是：%d\n", selectElem(head, 3));    
    //表中第 3 个节点中的数据改为存储 6    
    printf("将第 3 个节点存储的数据改为 6：\n");    
    head = amendElem(head, 3, 6);    
    display(head);    
    return 0;
}
line* initLine(line * head) {    
    int i = 0;    
    line * list = NULL;    
    head = (line*)malloc(sizeof(line));    
    head->prior = NULL;    
    head->next = NULL;    
    head->data = 1;    
    list = head;    
    for (i = 2; i <= 3; i++) {        
        line * body = (line*)malloc(sizeof(line));        
        body->prior = NULL;        
        body->next = NULL;        
        body->data = i;        
        list->next = body;        
        body->prior = list;        
        list = list->next;    
    }    
    return head;
}
line * insertLine(line * head, int data, int add) {    
    //新建数据域为data的结点    
    line * temp = (line*)malloc(sizeof(line));    
    temp->data = data;    
    temp->prior = NULL;    
    temp->next = NULL;    
    //插入到链表头，要特殊考虑    
    if (add == 1) {        
        temp->next = head;        
        head->prior = temp;       
        head = temp;    
    }    
    else {        
        int i = 0;        
        line * body = head;        
        //找到要插入位置的前一个结点        
        for (i = 1; i < add - 1; i++) {            
            body = body->next;            
            if (body == NULL) {                
                printf("插入位置有误\n");                
                break;            
            }        
        }        
        if (body) {            
            //判断条件为真，说明插入位置为链表尾            
            if (body->next == NULL) {                
                body->next = temp;                
                temp->prior = body;            
            }            
            else {                
                body->next->prior = temp;                
                temp->next = body->next;                
                body->next = temp;                
                temp->prior = body;            
            }        
        }    
    }    
    return head;
}
line * delLine(line * head, int data) {    
    line * temp = head;    
    //遍历链表    
    while (temp) {        
        //判断当前结点中数据域和data是否相等，若相等，摘除该结点        
        if (temp->data == data) {            
            temp->prior->next = temp->next;            
            temp->next->prior = temp->prior;            
            free(temp);            
            return head;        
        }        
        temp = temp->next;    
    }    
    printf("链表中无该数据元素\n");    
    return head;
}
//head为原双链表，elem表示被查找元素
int selectElem(line * head, int elem) {    
    //新建一个指针t，初始化为头指针 head    
    line * t = head;    
    int i = 1;    
    while (t) {        
        if (t->data == elem) {            
            return i;        
        }       
        i++;        
        t = t->next;    
    }    
    //程序执行至此处，表示查找失败    
    return -1;
}
//更新函数，其中，add 表示更改结点在双链表中的位置，newElem 为新数据的值
line *amendElem(line * p, int add, int newElem) {    
    int i = 0;    
    line * temp = p;    
    //遍历到被删除结点    
    for (i = 1; i < add; i++) {        
        temp = temp->next;        
        if (temp == NULL) {           
            printf("更改位置有误！\n");           
            break;       
        }   
    }   
    if (temp) {   
        temp->data = newElem; 
    }    
    return p;
}//输出链表的功能函数
void display(line * head) {  
    line * temp = head;  
    while (temp) {      
        if (temp->next == NULL) {   
            printf("%d\n", temp->data);    
        }        else {       
            printf("%d->", temp->data);  
        }      
        temp = temp->next;   
    }
}
```

程序执行结果为：

初始链表为：
1->2->3
在表中第 3 的位置插入新元素 7：
1->2->7->3
删除元素 2：
1->7->3
元素 3 的位置是：3
将第 3 个节点存储的数据改为 6：
1->7->6