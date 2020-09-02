# 什么是队列，队列及其应用（超详细）

队列，和[栈](http://data.biancheng.net/view/169.html)一样，也是一种对数据的"存"和"取"有严格要求的线性存储结构。

与栈结构不同的是，队列的两端都"开口"，要求数据只能从一端进，从另一端出，如图1 所示：


![队列存储结构](http://data.biancheng.net/uploads/allimg/181203/2-1Q203200556309.gif)
图 1 队列存储结构

通常，称进数据的一端为 "队尾"，出数据的一端为 "队头"，数据元素进队列的过程称为 "入队"，出队列的过程称为 "出队"。

不仅如此，队列中数据的进出要遵循 "先进先出" 的原则，即最先进队列的数据元素，同样要最先出队列。拿图 1 中的队列来说，从数据在队列中的存储状态可以分析出，元素 1 最先进队，其次是元素 2，最后是元素 3。此时如果将元素 3 出队，根据队列 "先进先出" 的特点，元素 1 要先出队列，元素 2 再出队列，最后才轮到元素 3 出队列。

栈和队列不要混淆，栈结构是一端封口，特点是"先进后出"；而队列的两端全是开口，特点是"先进先出"。

因此，数据从表的一端进，从另一端出，且遵循 "先进先出" 原则的线性存储结构就是队列。

## 队列的实现

队列存储结构的实现有以下两种方式：

1. [顺序队列](http://data.biancheng.net/view/173.html)：在[顺序表](http://data.biancheng.net/view/158.html)的基础上实现的队列结构；
2. [链队列](http://data.biancheng.net/view/174.html)：在[链表](http://data.biancheng.net/view/160.html)的基础上实现的队列结构；


两者的区别仅是顺序表和链表的区别，即在实际的物理空间中，数据集中存储的队列是顺序队列，分散存储的队列是链队列。

## 队列的实际应用

实际生活中，队列的应用随处可见，比如排队买 XXX、医院的挂号系统等，采用的都是队列的结构。

拿排队买票来说，所有的人排成一队，先到者排的就靠前，后到者只能从队尾排队等待，队中的每个人都必须等到自己前面的所有人全部买票成功并从队头出队后，才轮到自己买票。这就不是典型的队列结构吗？

明白了什么是队列，接下来开始系统地学习顺序队列和链队列。



顺序[队列](http://data.biancheng.net/view/172.html)，即采用[顺序表](http://data.biancheng.net/view/158.html)模拟实现的队列结构。

我们知道，队列具有以下两个特点：

1. 数据从队列的一端进，另一端出；
2. 数据的入队和出队遵循"先进先出"的原则；


因此，只要使用顺序表按以上两个要求操作数据，即可实现顺序队列。首先来学习一种最简单的实现方法。

## 顺序队列简单实现

由于顺序队列的底层使用的是[数组](http://data.biancheng.net/view/181.html)，因此需预先申请一块足够大的内存空间初始化顺序队列。除此之外，为了满足顺序队列中数据从队尾进，队头出且先进先出的要求，我们还需要定义两个指针（top 和 rear）分别用于指向顺序队列中的队头元素和队尾元素，如 图 1 所示：


![顺序队列实现示意图](http://data.biancheng.net/uploads/allimg/181204/2-1Q204202R4539.gif)
图 1 顺序队列实现示意图


由于顺序队列初始状态没有存储任何元素，因此 top 指针和 rear 指针重合，且由于顺序队列底层实现靠的是数组，因此 top 和 rear 实际上是两个变量，它的值分别是队头元素和队尾元素所在数组位置的下标。

在图 1 的基础上，当有数据元素进队列时，对应的实现操作是将其存储在指针 rear 指向的数组位置，然后 rear+1；当需要队头元素出队时，仅需做 top+1 操作。

例如，在图 1 基础上将 `{1,2,3,4}` 用顺序队列存储的实现操作如图 2 所示：


![数据进顺序队列的过程实现示意图](http://data.biancheng.net/uploads/allimg/181204/2-1Q20420293O01.gif)
图 2 数据进顺序队列的过程实现示意图


在图 2 基础上，顺序队列中数据出队列的实现过程如图 3 所示：


![数据出顺序队列的过程示意图](http://data.biancheng.net/uploads/allimg/181204/2-1Q204202950120.gif)
图 3 数据出顺序队列的过程示意图


因此，使用顺序表实现顺序队列最简单方法的 C 语言实现代码为：

```
#include <stdio.h>int enQueue(int *a,int rear,int data){    a[rear]=data;    rear++;    return rear;}void deQueue(int *a,int front,int rear){    //如果 front==rear，表示队列为空    while (front!=rear) {        printf("出队元素：%d\n",a[front]);        front++;    }}int main() {    int a[100];    int front,rear;    //设置队头指针和队尾指针，当队列中没有元素时，队头和队尾指向同一块地址    front=rear=0;    //入队    rear=enQueue(a, rear, 1);    rear=enQueue(a, rear, 2);    rear=enQueue(a, rear, 3);    rear=enQueue(a, rear, 4);    //出队    deQueue(a, front, rear);    return 0;}
```

程序输出结果：

出队元素：1
出队元素：2
出队元素：3
出队元素：4

#### 此方法存在的问题

先来分析以下图 2b) 和图 3b)。图 2b) 是所有数据进队成功的示意图，而图 3b) 是所有数据全部出队后的示意图。通过对比两张图，你会发现，指针 top 和 rear 重合位置指向了 a[4] 而不再是 a[0]。也就是说，整个顺序队列在数据不断地进队出队过程中，在顺序表中的位置不断后移。

顺序队列整体后移造成的影响是：

- 顺序队列之前的数组存储空间将无法再被使用，造成了空间浪费；
- 如果顺序表申请的空间不足够大，则直接造成程序中数组 a 溢出，产生溢出错误；


为了避免以上两点，我建议初学者使用下面的方法实现顺序队列。

## 顺序队列另一种实现方法

既然明白了上面这种方法的弊端，那么我们可以试着在它的基础上对其改良。

为了解决以上两个问题，可以使用巧妙的方法将顺序表打造成一个环状表，如图 4 所示：


![环状顺序队列](http://data.biancheng.net/uploads/allimg/181204/2-1Q204203432215.gif)
图 4 环状顺序队列


图 4 只是一个想象图，在真正的实现时，没必要真创建这样一种结构，我们还是使用之前的顺序表，也还是使用之前的程序，只需要对其进行一点小小的改变：

```c
#include <stdio.h>
#define max 5//表示顺序表申请的空间大小
int enQueue(int *a,int front,int rear,int data){    
    //添加判断语句，如果rear超过max，则直接将其从a[0]重新开始存储，如果rear+1和front重合，则表示数组已满    
    if ((rear+1)%max==front) {        
        printf("空间已满");        
        return rear;    
    }    
    a[rear%max]=data;    
    rear++;    
    return rear;
}
int  deQueue(int *a,int front,int rear){    
    //如果front==rear，表示队列为空    
    if(front==rear%max) {        
        printf("队列为空");        
        return front;    
    }    
    printf("%d ",a[front]);    
    //front不再直接 +1，而是+1后同max进行比较，如果=max，则直接跳转到 a[0]    
    front=(front+1)%max;    
    return front;
}
int main() {    
    int a[max];    
    int front,rear;    
    //设置队头指针和队尾指针，当队列中没有元素时，队头和队尾指向同一块地址    
    front=rear=0;    
    //入队    
    rear=enQueue(a,front,rear, 1);    
    rear=enQueue(a,front,rear, 2);    
    rear=enQueue(a,front,rear, 3);    
    rear=enQueue(a,front,rear, 4);    
    //出队    
    front=deQueue(a, front, rear);    
    //再入队    
    rear=enQueue(a,front,rear, 5);    
    //再出队    
    front=deQueue(a, front, rear);    
    //再入队    
    rear=enQueue(a,front,rear, 6);    
    //再出队    
    front=deQueue(a, front, rear);    
    front=deQueue(a, front, rear);    
    front=deQueue(a, front, rear);    
    front=deQueue(a, front, rear);    
    return 0;
}
```

程序运行结果：

1 2 3 4 5 6

使用此方法需要注意的是，顺序队列在判断数组是否已满时，出现下面情况：

- 当队列为空时，队列的头指针等于队列的尾指针；
- 当数组满员时，队列的头指针等于队列的尾指针；


顺序队列的存储状态不同，但是判断条件相同。为了对其进行区分，最简单的解决办法是：牺牲掉数组中的一个存储空间，判断数组满员的条件是：尾指针的下一个位置和头指针相遇，就说明数组满了，即程序中第 5 行所示。

**链式队列**，简称"链队列"，即使用[链表](http://data.biancheng.net/view/160.html)实现的队列存储结构。

链式队列的实现思想同**顺序队列**

类似，只需创建两个指针（命名为 top 和 rear）分别指向链表中队列的队头元素和队尾元素，如 图1 所示:


![链式队列的初始状态](http://data.biancheng.net/uploads/allimg/181205/2-1Q205211052O7.gif)
图 1 链式队列的初始状态


图 1 所示为链式队列的初始状态，此时队列中没有存储任何数据元素，因此 top 和 rear 指针都同时指向头节点。

在创建链式队列时，强烈建议初学者创建一个带有头节点的链表，这样实现链式队列会更简单。

由此，我们可以编写出创建链式队列的 C 语言实现代码为:

```
//链表中的节点结构typedef struct QNode{    int data;    struct QNode * next;}QNode;//创建链式队列的函数QNode * initQueue(){    //创建一个头节点    QNode * queue=(QNode*)malloc(sizeof(QNode));    //对头节点进行初始化    queue->next=NULL;    return queue;}
```

## 链式队列数据入队

链队队列中，当有新的数据元素入队，只需进行以下 3 步操作：

1. 将该数据元素用节点包裹，例如新节点名称为 elem；
2. 与 rear 指针指向的节点建立逻辑关系，即执行 rear->next=elem；
3. 最后移动 rear 指针指向该新节点，即 rear=elem；


由此，新节点就入队成功了。

例如，在图 1 的基础上，我们依次将 `{1,2,3}` 依次入队，各个数据元素入队的过程如图 2 所示:


![{1,2,3} 入链式队列](http://data.biancheng.net/uploads/allimg/181205/2-1Q2052111543C.gif)
图 2 {1,2,3} 入链式队列


数据元素入链式队列的 C 语言实现代码为：

```
QNode* enQueue(QNode * rear,int data){    //1、用节点包裹入队元素    QNode * enElem=(QNode*)malloc(sizeof(QNode));    enElem->data=data;    enElem->next=NULL;    //2、新节点与rear节点建立逻辑关系    rear->next=enElem;    //3、rear指向新节点    rear=enElem;    //返回新的rear，为后续新元素入队做准备    return rear;}
```

## 链式队列数据出队

当链式队列中，有数据元素需要出队时，按照 "先进先出" 的原则，只需将存储该数据的节点以及它之前入队的元素节点按照原则依次出队即可。这里，我们先学习如何将队头元素出队。

链式队列中队头元素出队，需要做以下 3 步操作：

1. 通过 top 指针直接找到队头节点，创建一个新指针 p 指向此即将出队的节点；
2. 将 p 节点（即要出队的队头节点）从链表中摘除；
3. 释放节点 p，回收其所占的内存空间；


例如，在图 2b) 的基础上，我们将元素 1 和 2 出队，则操作过程如图 3 所示：


![链式队列中数据元素出队](http://data.biancheng.net/uploads/allimg/181205/2-1Q205211240637.gif)
图 3 链式队列中数据元素出队


链式队列中队头元素出队的 C 语言实现代码为：

```
void DeQueue(QNode * top,QNode * rear){    if (top->next==NULL) {        printf("队列为空");        return ;    }    // 1、    QNode * p=top->next;    printf("%d",p->data);    top->next=p->next;    if (rear==p) {        rear=top;    }    free(p);}
```

注意，将队头元素做出队操作时，需提前判断队列中是否还有元素，如果没有，要提示用户无法做出队操作，保证程序的健壮性。

## 总结

通过学习链式队列最基本的数据入队和出队操作，我们可以就实际问题，对以上代码做适当的修改。

前面在学习顺序队列时，由于

[顺序表](http://data.biancheng.net/view/158.html)

的局限性，我们在顺序队列中实现数据入队和出队的基础上，又对实现代码做了改进，令其能够充分利用

[数组](http://data.biancheng.net/view/181.html)

中的空间。链式队列就不需要考虑空间利用的问题，因为链式队列本身就是实时申请空间。因此，这可以算作是链式队列相比顺序队列的一个优势。

这里给出链式队列入队和出队的完整 C 语言代码为：

```c
#include <stdio.h>
#include <stdlib.h>
typedef struct QNode{    
    int data;    
    struct QNode * next;
}QNode;
QNode * initQueue(){    
    QNode * queue=(QNode*)malloc(sizeof(QNode));    
    queue->next=NULL;    
    return queue;
}
QNode* enQueue(QNode * rear,int data){    
    QNode * enElem=(QNode*)malloc(sizeof(QNode));    
    enElem->data=data;    
    enElem->next=NULL;    
    //使用尾插法向链队列中添加数据元素    
    rear->next=enElem;    
    rear=enElem;    
    return rear;
}
QNode* DeQueue(QNode * top,QNode * rear){    
    if (top->next==NULL) {        
        printf("\n队列为空");        
        return rear;    
    }    
    QNode * p=top->next;    
    printf("%d ",p->data);    
    top->next=p->next;    
    if (rear==p) {        
        rear=top;    
    }    
    free(p);    
    return rear;
}
int main() {    
    QNode * queue,*top,*rear;    
    queue=top=rear=initQueue();
    //创建头结点    
    //向链队列中添加结点，使用尾插法添加的同时，队尾指针需要指向链表的最后一个元素    
    rear=enQueue(rear, 1);    
    rear=enQueue(rear, 2);    
    rear=enQueue(rear, 3);    
    rear=enQueue(rear, 4);    
    //入队完成，所有数据元素开始出队列    
    rear=DeQueue(top, rear);    
    rear=DeQueue(top, rear);    
    rear=DeQueue(top, rear);    
    rear=DeQueue(top, rear);    
    rear=DeQueue(top, rear);    
    return 0;
}
```

程序运行结果为：

1 2 3 4
队列为空