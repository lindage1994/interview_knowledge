之前介绍的所有的数据结构都是线性存储结构。本章所介绍的树结构是一种非线性存储结构，存储的是具有“一对多”关系的数据元素的集合。

![img](https://raw.githubusercontent.com/lindage1994/images/master/typora202009/25/204947-990761.png)

​                                 

​    (A)                                     (B) 

[图](http://data.biancheng.net/view/200.html) 1 树的示例


图 1(A) 是使用树结构存储的集合 {A,B,C,D,E,F,G,H,I,J,K,L,M} 的示意图。对于数据 A 来说，和数据 B、C、D 有关系；对于数据 B 来说，和 E、F 有关系。这就是“一对多”的关系。

将具有“一对多”关系的集合中的数据元素按照图 1（A）的形式进行存储，整个存储形状在逻辑结构上看，类似于实际生活中倒着的树（图 1（B）倒过来），所以称这种存储结构为“树型”存储结构。

## 树的结点

结点：使用树结构存储的每一个数据元素都被称为“结点”。例如，图 1（A）中，数据元素 A 就是一个结点；

父结点（双亲结点）、子结点和兄弟结点：对于图 1（A）中的结点 A、B、C、D 来说，A 是 B、C、D 结点的父结点（也称为“双亲结点”），而 B、C、D 都是 A 结点的子结点（也称“孩子结点”）。对于 B、C、D 来说，它们都有相同的父结点，所以它们互为兄弟结点。

树根结点（简称“根结点”）：每一个非空树都有且只有一个被称为根的结点。图 1（A）中，结点A就是整棵树的根结点。

树根的判断依据为：如果一个结点没有父结点，那么这个结点就是整棵树的根结点。

叶子结点：如果结点没有任何子结点，那么此结点称为叶子结点（叶结点）。例如图 1（A）中，结点 K、L、F、G、M、I、J 都是这棵树的叶子结点。

## 子树和空树

子树：如图 1（A）中，整棵树的根结点为结点 A，而如果单看结点 B、E、F、K、L 组成的部分来说，也是棵树，而且节点 B 为这棵树的根结点。所以称 B、E、F、K、L 这几个结点组成的树为整棵树的子树；同样，结点 E、K、L 构成的也是一棵子树，根结点为 E。

> 注意：单个结点也是一棵树，只不过根结点就是它本身。图 1（A）中，结点 K、L、F 等都是树，且都是整棵树的子树。

知道了子树的概念后，树也可以这样定义：树是由根结点和若干棵子树构成的。

空树：如果集合本身为空，那么构成的树就被称为空树。空树中没有结点。

补充：在树结构中，对于具有同一个根结点的各个子树，相互之间不能有交集。例如，图 1（A）中，除了根结点 A，其余元素又各自构成了三个子树，根结点分别为 B、C、D，这三个子树相互之间没有相同的结点。如果有，就破坏了树的结构，不能算做是一棵树。

## 结点的度和层次

对于一个结点，拥有的子树数（结点有多少分支）称为结点的度（Degree）。例如，图 1（A）中，根结点 A 下分出了 3 个子树，所以，结点 A 的度为 3。

一棵树的度是树内各结点的度的最大值。图 1（A）表示的树中，各个结点的度的最大值为 3，所以，整棵树的度的值是 3。

结点的层次：从一棵树的树根开始，树根所在层为第一层，根的孩子结点所在的层为第二层，依次类推。对于图 1（A）来说，A 结点在第一层，B、C、D 为第二层，E、F、G、H、I、J 在第三层，K、L、M 在第四层。

一棵树的深度（高度）是树中结点所在的最大的层次。图 1（A）树的深度为 4。

如果两个结点的父结点虽不相同，但是它们的父结点处在同一层次上，那么这两个结点互为堂兄弟。例如，图 1（A）中，结点 G 和 E、F、H、I、J 的父结点都在第二层，所以之间为堂兄弟的关系。

## 有序树和无序树

如果树中结点的子树从左到右看，谁在左边，谁在右边，是有规定的，这棵树称为有序树；反之称为无序树。

在有序树中，一个结点最左边的子树称为"第一个孩子"，最右边的称为"最后一个孩子"。

拿图 1（A）来说，如果是其本身是一棵有序树，则以结点 B 为根结点的子树为整棵树的第一个孩子，以结点 D 为根结点的子树为整棵树的最后一个孩子。

## 森林

由 m（m >= 0）个互不相交的树组成的集合被称为森林。图 1（A）中，分别以 B、C、D 为根结点的三棵子树就可以称为森林。

前面讲到，树可以理解为是由根结点和若干子树构成的，而这若干子树本身是一个森林，所以，树还可以理解为是由根结点和森林组成的。用一个式子表示为：

Tree =（root,F）

其中，root 表示树的根结点，F 表示由 m（m >= 0）棵树组成的森林。

## 树的表示方法

除了图 1（A）表示树的方法外，还有其他表示方法：

![img](https://raw.githubusercontent.com/lindage1994/images/master/typora202009/25/205114-699422.png)

 


     （A）                     （B）

图2 树的表示形式


图 2（A）是以嵌套的集合的形式表示的（集合之间绝不能相交，即图中任意两个圈不能相交）。

图 2（B）使用的是凹入表示法（了解即可），表示方式是：最长条为根结点，相同长度的表示在同一层次。例如 B、C、D 长度相同，都为 A 的子结点，E 和 F 长度相同，为 B 的子结点，K 和 L 长度相同，为 E 的子结点，依此类推。

最常用的表示方法是使用

[广义表](http://data.biancheng.net/view/189.html)

的方式。图 1（A）用广义表表示为：

(A , ( B ( E ( K , L ) , F ) , C ( G ) , D ( H ( M ) , I , J ) ) )

## 总结

树型存储结构类似于家族的族谱，各个结点之间也同样可能具有父子、兄弟、表兄弟的关系。本节中，要重点理解树的根结点和子树的定义，同时要会计算树中各个结点的度和层次，以及树的深度。

通过《[树的存储结构](http://data.biancheng.net/view/23.html)》一节的学习，我们了解了一些

[树](http://data.biancheng.net/view/23.html)

存储结构的基本知识。本节将给大家介绍一类具体的树结构——二叉树。

简单地理解，满足以下两个条件的树就是二叉树：

1. 本身是有序树；
2. 树中包含的各个节点的度不能超过 2，即只能是 0、1 或者 2；


例如，

[图](http://data.biancheng.net/view/200.html)

 1a) 就是一棵二叉树，而图 1b) 则不是。

![二叉树示意图](https://raw.githubusercontent.com/lindage1994/images/master/typora202009/25/205205-158306.gif)
图 1 二叉树示意图

## 二叉树的性质

经过前人的总结，二叉树具有以下几个性质：

1. 二叉树中，第 i 层最多有 2i-1 个结点。
2. 如果二叉树的深度为 K，那么此二叉树最多有 2K-1 个结点。
3. 二叉树中，终端结点数（叶子结点数）为 n0，度为 2 的结点数为 n2，则 n0=n2+1。

性质 3 的计算方法为：对于一个二叉树来说，除了度为 0 的叶子结点和度为 2 的结点，剩下的就是度为 1 的结点（设为 n1），那么总结点 n=n0+n1+n2。
同时，对于每一个结点来说都是由其父结点分支表示的，假设树中分枝数为 B，那么总结点数 n=B+1。而分枝数是可以通过 n1 和 n2 表示的，即 B=n1+2*n2。所以，n 用另外一种方式表示为 n=n1+2*n2+1。
两种方式得到的 n 值组成一个方程组，就可以得出 n0=n2+1。


二叉树还可以继续分类，衍生出满二叉树和完全二叉树。

## 满二叉树

如果二叉树中除了叶子结点，每个结点的度都为 2，则此二叉树称为满二叉树。

![满二叉树示意图](https://raw.githubusercontent.com/lindage1994/images/master/typora202009/25/205211-550396.gif)
图 2 满二叉树示意图


如图 2 所示就是一棵满二叉树。

满二叉树除了满足普通二叉树的性质，还具有以下性质：

1. 满二叉树中第 i 层的节点数为 2n-1 个。
2. 深度为 k 的满二叉树必有 2k-1 个节点 ，叶子数为 2k-1。
3. 满二叉树中不存在度为 1 的节点，每一个分支点中都两棵深度相同的子树，且叶子节点都在最底层。
4. 具有 n 个节点的满二叉树的深度为 log2(n+1)。

## 完全二叉树

如果二叉树中除去最后一层节点为满二叉树，且最后一层的结点依次从左到右分布，则此二叉树被称为完全二叉树。


![完全二叉树示意图](http://data.biancheng.net/uploads/allimg/181226/2-1Q22620003J18.gif)
图 3 完全二叉树示意图


如图 3a) 所示是一棵完全二叉树，图 3b) 由于最后一层的节点没有按照从左向右分布，因此只能算作是普通的二叉树。

完全二叉树除了具有普通二叉树的性质，它自身也具有一些独特的性质，比如说，n 个结点的完全二叉树的深度为 ⌊log2n⌋+1。

⌊log2n⌋ 表示取小于 log2n 的最大整数。例如，⌊log24⌋ = 2，而 ⌊log25⌋ 结果也是 2。

对于任意一个完全二叉树来说，如果将含有的结点按照层次从左到右依次标号（如图 3a)），对于任意一个结点 i ，完全二叉树还有以下几个结论成立：

1. 当 i>1 时，父亲结点为结点 [i/2] 。（i=1 时，表示的是根结点，无父亲结点）
2. 如果 2*i>n（总结点的个数） ，则结点 i 肯定没有左孩子（为叶子结点）；否则其左孩子是结点 2*i 。
3. 如果 2*i+1>n ，则结点 i 肯定没有右孩子；否则右孩子是结点 2*i+1 。

## 总结

本节介绍了什么是二叉树，以及二叉树的性质，同时还介绍了满二叉树和完全二叉树以及各自所特有的性质，初学者需理解并牢记这些性质，才能更熟练地使用二叉树解决实际问题。





[二叉树](http://data.biancheng.net/view/192.html)

的存储结构有两种，分别为顺序存储和链式存储。本节先介绍二叉[树](http://data.biancheng.net/view/23.html)的顺序存储结构。

二叉树的顺序存储，指的是使用

[顺序表](http://data.biancheng.net/view/158.html)

（

[数组](http://data.biancheng.net/view/181.html)

）存储二叉树。需要注意的是，顺序存储只适用于完全二叉树。换句话说，只有完全二叉树才可以使用顺序表存储。因此，如果我们想顺序存储普通二叉树，需要提前将普通二叉树转化为完全二叉树。

有读者会说，满二叉树也可以使用顺序存储。要知道，满二叉树也是完全二叉树，因为它满足完全二叉树的所有特征。

普通二叉树转完全二叉树的方法很简单，只需给二叉树额外添加一些节点，将其"拼凑"成完全二叉树即可。如

[图](http://data.biancheng.net/view/200.html)

 1 所示：

![img](https://raw.githubusercontent.com/lindage1994/images/master/typora202009/25/205255-880150.png)
图 1 普通二叉树的转化


图 1 中，左侧是普通二叉树，右侧是转化后的完全（满）二叉树。

解决了二叉树的转化问题，接下来学习如何顺序存储完全（满）二叉树。

完全二叉树的顺序存储，仅需从根节点开始，按照层次依次将树中节点存储到数组即可。

![img](https://raw.githubusercontent.com/lindage1994/images/master/typora202009/25/205301-746992.gif)
图 2 完全二叉树示意图


例如，存储图 2 所示的完全二叉树，其存储状态如图 3 所示：


![img](http://data.biancheng.net/uploads/allimg/170830/2-1FS0105529235.png)
图 3 完全二叉树存储状态示意图


同样，存储由普通二叉树转化来的完全二叉树也是如此。例如，图 1 中普通二叉树的数组存储状态如图 4 所示：


![img](http://data.biancheng.net/uploads/allimg/170830/2-1FS0105A9146.png)
图 4 普通二叉树的存储状态


由此，我们就实现了完全二叉树的顺序存储。

不仅如此，从顺序表中还原完全二叉树也很简单。我们知道，完全二叉树具有这样的性质，将树中节点按照层次并从左到右依次标号（1,2,3,...），若节点 i 有左右孩子，则其左孩子节点为 2*i，右孩子节点为 2*i+1。此性质可用于还原数组中存储的完全二叉树，也就是实现由图 3 到图 2、由图 4 到图 1 的转变。

编写本节实现代码，需要对二叉树进行层次遍历，这个知识点本章有单独一节做详细介绍，这里不再给出具体的代码实现。



上一节讲了

[二叉树](http://data.biancheng.net/view/192.html)

的顺序存储，通过学习你会发现，其实二叉

[树](http://data.biancheng.net/view/23.html)

并不适合用

[数组](http://data.biancheng.net/view/181.html)

存储，因为并不是每个二叉树都是完全二叉树，普通二叉树使用

[顺序表](http://data.biancheng.net/view/158.html)

存储或多或多会存在空间浪费的现象。

本节我们学习二叉树的链式存储结构。

![普通二叉树示意图](https://raw.githubusercontent.com/lindage1994/images/master/typora202009/25/205342-25697.gif)
[图](http://data.biancheng.net/view/200.html) 1 普通二叉树示意图


如图 1 所示，此为一棵普通的二叉树，若将其采用链式存储，则只需从树的根节点开始，将各个节点及其左右孩子使用

[链表](http://data.biancheng.net/view/160.html)

存储即可。因此，图 1 对应的链式存储结构如图 2 所示：

![二叉树链式存储结构示意图](https://raw.githubusercontent.com/lindage1994/images/master/typora202009/25/205338-40317.gif)
图 2 二叉树链式存储结构示意图


由图 2 可知，采用链式存储二叉树时，其节点结构由 3 部分构成（如图 3 所示）：

- 指向左孩子节点的指针（Lchild）；
- 节点存储的数据（data）；
- 指向右孩子节点的指针（Rchild）；

![二叉树节点结构](http://data.biancheng.net/uploads/allimg/181228/2-1Q22R02Q6392.gif)
图 3 二叉树节点结构


表示该节点结构的 C 语言代码为：

```
typedef struct BiTNode{    TElemType data;//数据域    struct BiTNode *lchild,*rchild;//左右孩子指针    struct BiTNode *parent;}BiTNode,*BiTree;
```


图 2 中的链式存储结构对应的 C 语言代码为：

```
#include <stdio.h>#include <stdlib.h>#define TElemType inttypedef struct BiTNode{    TElemType data;//数据域    struct BiTNode *lchild,*rchild;//左右孩子指针}BiTNode,*BiTree;void CreateBiTree(BiTree *T){    *T=(BiTNode*)malloc(sizeof(BiTNode));    (*T)->data=1;    (*T)->lchild=(BiTNode*)malloc(sizeof(BiTNode));    (*T)->lchild->data=2;    (*T)->rchild=(BiTNode*)malloc(sizeof(BiTNode));    (*T)->rchild->data=3;    (*T)->rchild->lchild=NULL;    (*T)->rchild->rchild=NULL;    (*T)->lchild->lchild=(BiTNode*)malloc(sizeof(BiTNode));    (*T)->lchild->lchild->data=4;    (*T)->lchild->rchild=NULL;    (*T)->lchild->lchild->lchild=NULL;    (*T)->lchild->lchild->rchild=NULL;}int main() {    BiTree Tree;    CreateBiTree(&Tree);    printf("%d",Tree->lchild->lchild->data);    return 0;}
```

程序输出结果：

4

其实，二叉树的链式存储结构远不止图 2 所示的这一种。例如，在某些实际场景中，可能会做 "查找某节点的父节点" 的操作，这时可以在节点结构中再添加一个指针域，用于各个节点指向其父亲节点，如图 4 所示：

![自定义二叉树的链式存储结构](https://raw.githubusercontent.com/lindage1994/images/master/typora202009/25/205357-369669.gif)
图 4 自定义二叉树的链式存储结构

这样的链表结构，通常称为三叉链表。

利用图 4 所示的三叉链表，我们可以很轻松地找到各节点的父节点。因此，在解决实际问题时，用合适的链表结构存储二叉树，可以起到事半功倍的效果。

[二叉树](http://data.biancheng.net/view/192.html)

先序遍历的实现思想是：

1. 访问根节点；
2. 访问当前节点的左子[树](http://data.biancheng.net/view/23.html)；
3. 若当前节点无左子树，则访问当前节点的右子树；

![img](https://raw.githubusercontent.com/lindage1994/images/master/typora202009/25/205408-383418.png)
[图](http://data.biancheng.net/view/200.html) 1 二叉树


以图 1 为例，采用先序遍历的思想遍历该二叉树的过程为：

1. 访问该二叉树的根节点，找到 1；
2. 访问节点 1 的左子树，找到节点 2；
3. 访问节点 2 的左子树，找到节点 4；
4. 由于访问节点 4 左子树失败，且也没有右子树，因此以节点 4 为根节点的子树遍历完成。但节点 2 还没有遍历其右子树，因此现在开始遍历，即访问节点 5；
5. 由于节点 5 无左右子树，因此节点 5 遍历完成，并且由此以节点 2 为根节点的子树也遍历完成。现在回到节点 1 ，并开始遍历该节点的右子树，即访问节点 3；
6. 访问节点 3 左子树，找到节点 6；
7. 由于节点 6 无左右子树，因此节点 6 遍历完成，回到节点 3 并遍历其右子树，找到节点 7；
8. 节点 7 无左右子树，因此以节点 3 为根节点的子树遍历完成，同时回归节点 1。由于节点 1 的左右子树全部遍历完成，因此整个二叉树遍历完成；


因此，图 1 中二叉树采用先序遍历得到的序列为：

1 2 4 5 3 6 7

## 递归实现

二叉树的先序遍历采用的是递归的思想，因此可以递归实现，其 C 语言实现代码为：

```
#include <stdio.h>#include <string.h>#define TElemType int//构造结点的结构体typedef struct BiTNode{    TElemType data;//数据域    struct BiTNode *lchild,*rchild;//左右孩子指针}BiTNode,*BiTree;//初始化树的函数void CreateBiTree(BiTree *T){    *T=(BiTNode*)malloc(sizeof(BiTNode));    (*T)->data=1;    (*T)->lchild=(BiTNode*)malloc(sizeof(BiTNode));    (*T)->rchild=(BiTNode*)malloc(sizeof(BiTNode));      (*T)->lchild->data=2;    (*T)->lchild->lchild=(BiTNode*)malloc(sizeof(BiTNode));    (*T)->lchild->rchild=(BiTNode*)malloc(sizeof(BiTNode));    (*T)->lchild->rchild->data=5;    (*T)->lchild->rchild->lchild=NULL;    (*T)->lchild->rchild->rchild=NULL;    (*T)->rchild->data=3;    (*T)->rchild->lchild=(BiTNode*)malloc(sizeof(BiTNode));    (*T)->rchild->lchild->data=6;    (*T)->rchild->lchild->lchild=NULL;    (*T)->rchild->lchild->rchild=NULL;    (*T)->rchild->rchild=(BiTNode*)malloc(sizeof(BiTNode));    (*T)->rchild->rchild->data=7;    (*T)->rchild->rchild->lchild=NULL;    (*T)->rchild->rchild->rchild=NULL;    (*T)->lchild->lchild->data=4;    (*T)->lchild->lchild->lchild=NULL;    (*T)->lchild->lchild->rchild=NULL;}//模拟操作结点元素的函数，输出结点本身的数值void displayElem(BiTNode* elem){    printf("%d ",elem->data);}//先序遍历void PreOrderTraverse(BiTree T){    if (T) {        displayElem(T);//调用操作结点数据的函数方法        PreOrderTraverse(T->lchild);//访问该结点的左孩子        PreOrderTraverse(T->rchild);//访问该结点的右孩子    }    //如果结点为空，返回上一层    return;}int main() {    BiTree Tree;    CreateBiTree(&Tree);    printf("先序遍历: \n");}
```

运行结果：

先序遍历:
1 2 4 5 3 6 7

## 非递归实现

而递归的底层实现依靠的是

[栈](http://data.biancheng.net/view/169.html)

存储结构，因此，二叉树的先序遍历既可以直接采用递归思想实现，也可以使用栈的存储结构模拟递归的思想实现，其 C 语言实现代码为：

```
纯文本复制
#include <stdio.h>#include <string.h>#define TElemType intint top=-1;//top变量时刻表示栈顶元素所在位置//构造结点的结构体typedef struct BiTNode{    TElemType data;//数据域    struct BiTNode *lchild,*rchild;//左右孩子指针}BiTNode,*BiTree;//初始化树的函数void CreateBiTree(BiTree *T){    *T=(BiTNode*)malloc(sizeof(BiTNode));    (*T)->data=1;    (*T)->lchild=(BiTNode*)malloc(sizeof(BiTNode));    (*T)->rchild=(BiTNode*)malloc(sizeof(BiTNode));    (*T)->lchild->data=2;    (*T)->lchild->lchild=(BiTNode*)malloc(sizeof(BiTNode));    (*T)->lchild->rchild=(BiTNode*)malloc(sizeof(BiTNode));    (*T)->lchild->rchild->data=5;    (*T)->lchild->rchild->lchild=NULL;    (*T)->lchild->rchild->rchild=NULL;    (*T)->rchild->data=3;    (*T)->rchild->lchild=(BiTNode*)malloc(sizeof(BiTNode));    (*T)->rchild->lchild->data=6;    (*T)->rchild->lchild->lchild=NULL;    (*T)->rchild->lchild->rchild=NULL;    (*T)->rchild->rchild=(BiTNode*)malloc(sizeof(BiTNode));    (*T)->rchild->rchild->data=7;    (*T)->rchild->rchild->lchild=NULL;    (*T)->rchild->rchild->rchild=NULL;    (*T)->lchild->lchild->data=4;    (*T)->lchild->lchild->lchild=NULL;    (*T)->lchild->lchild->rchild=NULL;}//前序遍历使用的进栈函数void push(BiTNode** a,BiTNode* elem){    a[++top]=elem;}//弹栈函数void pop( ){    if (top==-1) {        return ;    }    top--;}//模拟操作结点元素的函数，输出结点本身的数值void displayElem(BiTNode* elem){    printf("%d ",elem->data);}//拿到栈顶元素BiTNode* getTop(BiTNode**a){    return a[top];}//先序遍历非递归算法void PreOrderTraverse(BiTree Tree){    BiTNode* a[20];//定义一个顺序栈    BiTNode * p;//临时指针    push(a, Tree);//根结点进栈    while (top!=-1) {        p=getTop(a);//取栈顶元素        pop();//弹栈        while (p) {            displayElem(p);//调用结点的操作函数            //如果该结点有右孩子，右孩子进栈            if (p->rchild) {                push(a,p->rchild);            }            p=p->lchild;//一直指向根结点最后一个左孩子        }    }}int main(){    BiTree Tree;    CreateBiTree(&Tree);    printf("先序遍历: \n");    PreOrderTraverse(Tree);}
```

运行结果

先序遍历:
1 2 4 5 3 6 7

[二叉树](http://data.biancheng.net/view/192.html)

中序遍历的实现思想是：

1. 访问当前节点的左子[树](http://data.biancheng.net/view/23.html)；
2. 访问根节点；
3. 访问当前节点的右子树；

![img](https://raw.githubusercontent.com/lindage1994/images/master/typora202009/25/205447-354874.png)
[图](http://data.biancheng.net/view/200.html) 1 二叉树


以图 1 为例，采用中序遍历的思想遍历该二叉树的过程为：

1. 访问该二叉树的根节点，找到 1；
2. 遍历节点 1 的左子树，找到节点 2；
3. 遍历节点 2 的左子树，找到节点 4；
4. 由于节点 4 无左孩子，因此找到节点 4，并遍历节点 4 的右子树；
5. 由于节点 4 无右子树，因此节点 2 的左子树遍历完成，访问节点 2；
6. 遍历节点 2 的右子树，找到节点 5；
7. 由于节点 5 无左子树，因此访问节点 5 ，又因为节点 5 没有右子树，因此节点 1 的左子树遍历完成，访问节点 1 ，并遍历节点 1 的右子树，找到节点 3；
8. 遍历节点 3 的左子树，找到节点 6；
9. 由于节点 6 无左子树，因此访问节点 6，又因为该节点无右子树，因此节点 3 的左子树遍历完成，开始访问节点 3 ，并遍历节点 3 的右子树，找到节点 7；
10. 由于节点 7 无左子树，因此访问节点 7，又因为该节点无右子树，因此节点 1 的右子树遍历完成，即整棵树遍历完成；


因此，图 1 中二叉树采用中序遍历得到的序列为：

4 2 5 1 6 3 7

## 递归实现

二叉树的中序遍历采用的是递归的思想，因此可以递归实现，其 C 语言实现代码为：

```
#include <stdio.h>#include <string.h>#define TElemType int//构造结点的结构体typedef struct BiTNode{    TElemType data;//数据域    struct BiTNode *lchild,*rchild;//左右孩子指针}BiTNode,*BiTree;//初始化树的函数void CreateBiTree(BiTree *T){    *T=(BiTNode*)malloc(sizeof(BiTNode));    (*T)->data=1;    (*T)->lchild=(BiTNode*)malloc(sizeof(BiTNode));    (*T)->rchild=(BiTNode*)malloc(sizeof(BiTNode));      (*T)->lchild->data=2;    (*T)->lchild->lchild=(BiTNode*)malloc(sizeof(BiTNode));    (*T)->lchild->rchild=(BiTNode*)malloc(sizeof(BiTNode));    (*T)->lchild->rchild->data=5;    (*T)->lchild->rchild->lchild=NULL;    (*T)->lchild->rchild->rchild=NULL;    (*T)->rchild->data=3;    (*T)->rchild->lchild=(BiTNode*)malloc(sizeof(BiTNode));    (*T)->rchild->lchild->data=6;    (*T)->rchild->lchild->lchild=NULL;    (*T)->rchild->lchild->rchild=NULL;    (*T)->rchild->rchild=(BiTNode*)malloc(sizeof(BiTNode));    (*T)->rchild->rchild->data=7;    (*T)->rchild->rchild->lchild=NULL;    (*T)->rchild->rchild->rchild=NULL;    (*T)->lchild->lchild->data=4;    (*T)->lchild->lchild->lchild=NULL;    (*T)->lchild->lchild->rchild=NULL;}//模拟操作结点元素的函数，输出结点本身的数值void displayElem(BiTNode* elem){    printf("%d ",elem->data);}//中序遍历void INOrderTraverse(BiTree T){    if (T) {        INOrderTraverse(T->lchild);//遍历左孩子        displayElem(T);//调用操作结点数据的函数方法        INOrderTraverse(T->rchild);//遍历右孩子    }    //如果结点为空，返回上一层    return;}int main() {    BiTree Tree;    CreateBiTree(&Tree);    printf("中序遍历算法: \n");    INOrderTraverse(Tree);}
```

运行结果：

中序遍历算法:
4 2 5 1 6 3 7

## 非递归实现

而递归的底层实现依靠的是

[栈](http://data.biancheng.net/view/169.html)

存储结构，因此，二叉树的先序遍历既可以直接采用递归思想实现，也可以使用栈的存储结构模拟递归的思想实现。

中序遍历的非递归方式实现思想是：从根结点开始，遍历左孩子同时压栈，当遍历结束，说明当前遍历的结点没有左孩子，从栈中取出来调用操作函数，然后访问该结点的右孩子，继续以上重复性的操作。

除此之外，还有另一种实现思想：中序遍历过程中，只需将每个结点的左子树压栈即可，右子树不需要压栈。当结点的左子树遍历完成后，只需要以栈顶结点的右孩子为根结点，继续循环遍历即可。

两种非递归方法实现二叉树中序遍历的代码实现为：

```
纯文本复制
#include <stdio.h>#include <string.h>#define TElemType intint top=-1;//top变量时刻表示栈顶元素所在位置//构造结点的结构体typedef struct BiTNode{    TElemType data;//数据域    struct BiTNode *lchild,*rchild;//左右孩子指针}BiTNode,*BiTree;//初始化树的函数void CreateBiTree(BiTree *T){    *T=(BiTNode*)malloc(sizeof(BiTNode));    (*T)->data=1;    (*T)->lchild=(BiTNode*)malloc(sizeof(BiTNode));    (*T)->rchild=(BiTNode*)malloc(sizeof(BiTNode));    (*T)->lchild->data=2;    (*T)->lchild->lchild=(BiTNode*)malloc(sizeof(BiTNode));    (*T)->lchild->rchild=(BiTNode*)malloc(sizeof(BiTNode));    (*T)->lchild->rchild->data=5;    (*T)->lchild->rchild->lchild=NULL;    (*T)->lchild->rchild->rchild=NULL;    (*T)->rchild->data=3;    (*T)->rchild->lchild=(BiTNode*)malloc(sizeof(BiTNode));    (*T)->rchild->lchild->data=6;    (*T)->rchild->lchild->lchild=NULL;    (*T)->rchild->lchild->rchild=NULL;    (*T)->rchild->rchild=(BiTNode*)malloc(sizeof(BiTNode));    (*T)->rchild->rchild->data=7;    (*T)->rchild->rchild->lchild=NULL;    (*T)->rchild->rchild->rchild=NULL;    (*T)->lchild->lchild->data=4;    (*T)->lchild->lchild->lchild=NULL;    (*T)->lchild->lchild->rchild=NULL;}//前序和中序遍历使用的进栈函数void push(BiTNode** a,BiTNode* elem){    a[++top]=elem;}//弹栈函数void pop( ){    if (top==-1) {        return ;    }    top--;}//模拟操作结点元素的函数，输出结点本身的数值void displayElem(BiTNode* elem){    printf("%d ",elem->data);}//拿到栈顶元素BiTNode* getTop(BiTNode**a){    return a[top];}//中序遍历非递归算法void InOrderTraverse1(BiTree Tree){    BiTNode* a[20];//定义一个顺序栈    BiTNode * p;//临时指针    push(a, Tree);//根结点进栈    while (top!=-1) {//top!=-1说明栈内不为空，程序继续运行        while ((p=getTop(a)) &&p){//取栈顶元素，且不能为NULL            push(a, p->lchild);//将该结点的左孩子进栈，如果没有左孩子，NULL进栈        }        pop();//跳出循环，栈顶元素肯定为NULL，将NULL弹栈        if (top!=-1) {            p=getTop(a);//取栈顶元素            pop();//栈顶元素弹栈            displayElem(p);            push(a, p->rchild);//将p指向的结点的右孩子进栈        }    }}//中序遍历实现的另一种方法void InOrderTraverse2(BiTree Tree){    BiTNode* a[20];//定义一个顺序栈    BiTNode * p;//临时指针    p=Tree;    //当p为NULL或者栈为空时，表明树遍历完成    while (p || top!=-1) {        //如果p不为NULL，将其压栈并遍历其左子树        if (p) {            push(a, p);            p=p->lchild;        }        //如果p==NULL，表明左子树遍历完成，需要遍历上一层结点的右子树        else{            p=getTop(a);            pop();            displayElem(p);            p=p->rchild;        }    }}int main(){    BiTree Tree;    CreateBiTree(&Tree);    printf("\n中序遍历算法1: \n");    InOrderTraverse1(Tree);    printf("\n中序遍历算法2: \n");    InOrderTraverse2(Tree);}
```

运行结果

中序遍历算法1:
4 2 5 1 6 3 7
中序遍历算法2:
4 2 5 1 6 3 7



[二叉树](http://data.biancheng.net/view/192.html)

后序遍历的实现思想是：从根节点出发，依次遍历各节点的左右子

[树](http://data.biancheng.net/view/23.html)

，直到当前节点左右子树遍历完成后，才访问该节点元素。

![img](https://raw.githubusercontent.com/lindage1994/images/master/typora202009/25/205506-924107.png)
[图](http://data.biancheng.net/view/200.html) 1 二叉树


如图 1 中，对此二叉树进行后序遍历的操作过程为：

- 从根节点 1 开始，遍历该节点的左子树（以节点 2 为根节点）；
- 遍历节点 2 的左子树（以节点 4 为根节点）；
- 由于节点 4 既没有左子树，也没有右子树，此时访问该节点中的元素 4，并回退到节点 2 ，遍历节点 2 的右子树（以 5 为根节点）；
- 由于节点 5 无左右子树，因此可以访问节点 5 ，并且此时节点 2 的左右子树也遍历完成，因此也可以访问节点 2；
- 此时回退到节点 1 ，开始遍历节点 1 的右子树（以节点 3 为根节点）；
- 遍历节点 3 的左子树（以节点 6 为根节点）；
- 由于节点 6 无左右子树，因此访问节点 6，并回退到节点 3，开始遍历节点 3 的右子树（以节点 7 为根节点）；
- 由于节点 7 无左右子树，因此访问节点 7，并且节点 3 的左右子树也遍历完成，可以访问节点 3；节点 1 的左右子树也遍历完成，可以访问节点 1；
- 到此，整棵树的遍历结束。

由此，对图 1 中二叉树进行后序遍历的结果为：

4 5 2 6 7 3 1

## 递归实现

后序遍历的递归实现代码为：

```
#include <stdio.h>#include <string.h>#define TElemType int//构造结点的结构体typedef struct BiTNode{    TElemType data;//数据域    struct BiTNode *lchild,*rchild;//左右孩子指针}BiTNode,*BiTree;//初始化树的函数void CreateBiTree(BiTree *T){    *T=(BiTNode*)malloc(sizeof(BiTNode));    (*T)->data=1;    (*T)->lchild=(BiTNode*)malloc(sizeof(BiTNode));    (*T)->rchild=(BiTNode*)malloc(sizeof(BiTNode));      (*T)->lchild->data=2;    (*T)->lchild->lchild=(BiTNode*)malloc(sizeof(BiTNode));    (*T)->lchild->rchild=(BiTNode*)malloc(sizeof(BiTNode));    (*T)->lchild->rchild->data=5;    (*T)->lchild->rchild->lchild=NULL;    (*T)->lchild->rchild->rchild=NULL;    (*T)->rchild->data=3;    (*T)->rchild->lchild=(BiTNode*)malloc(sizeof(BiTNode));    (*T)->rchild->lchild->data=6;    (*T)->rchild->lchild->lchild=NULL;    (*T)->rchild->lchild->rchild=NULL;    (*T)->rchild->rchild=(BiTNode*)malloc(sizeof(BiTNode));    (*T)->rchild->rchild->data=7;    (*T)->rchild->rchild->lchild=NULL;    (*T)->rchild->rchild->rchild=NULL;    (*T)->lchild->lchild->data=4;    (*T)->lchild->lchild->lchild=NULL;    (*T)->lchild->lchild->rchild=NULL;}//模拟操作结点元素的函数，输出结点本身的数值void displayElem(BiTNode* elem){    printf("%d ",elem->data);}//后序遍历void PostOrderTraverse(BiTree T){    if (T) {        PostOrderTraverse(T->lchild);//遍历左孩子        PostOrderTraverse(T->rchild);//遍历右孩子        displayElem(T);//调用操作结点数据的函数方法    }    //如果结点为空，返回上一层    return;}int main() {    BiTree Tree;    CreateBiTree(&Tree);    printf("后序遍历: \n");    PostOrderTraverse(Tree);}
```

运行结果：

后序遍历:
4 5 2 6 7 3 1

## 非递归实现

递归算法底层的实现使用的是

[栈](http://data.biancheng.net/view/169.html)

存储结构，所以可以直接使用栈写出相应的非递归算法。

后序遍历是在遍历完当前结点的左右孩子之后，才调用操作函数，所以需要在操作结点进栈时，为每个结点配备一个标志位。当遍历该结点的左孩子时，设置当前结点的标志位为 0，进栈；当要遍历该结点的右孩子时，设置当前结点的标志位为 1，进栈。

这样，当遍历完成，该结点弹栈时，查看该结点的标志位的值：如果是 0，表示该结点的右孩子还没有遍历；反之如果是 1，说明该结点的左右孩子都遍历完成，可以调用操作函数。

完整实现代码为：

```
纯文本复制
#include <stdio.h>#include <string.h>#define TElemType intint top=-1;//top变量时刻表示栈顶元素所在位置//构造结点的结构体typedef struct BiTNode{    TElemType data;//数据域    struct BiTNode *lchild,*rchild;//左右孩子指针}BiTNode,*BiTree;//初始化树的函数void CreateBiTree(BiTree *T){    *T=(BiTNode*)malloc(sizeof(BiTNode));    (*T)->data=1;    (*T)->lchild=(BiTNode*)malloc(sizeof(BiTNode));    (*T)->rchild=(BiTNode*)malloc(sizeof(BiTNode));    (*T)->lchild->data=2;    (*T)->lchild->lchild=(BiTNode*)malloc(sizeof(BiTNode));    (*T)->lchild->rchild=(BiTNode*)malloc(sizeof(BiTNode));    (*T)->lchild->rchild->data=5;    (*T)->lchild->rchild->lchild=NULL;    (*T)->lchild->rchild->rchild=NULL;    (*T)->rchild->data=3;    (*T)->rchild->lchild=(BiTNode*)malloc(sizeof(BiTNode));    (*T)->rchild->lchild->data=6;    (*T)->rchild->lchild->lchild=NULL;    (*T)->rchild->lchild->rchild=NULL;    (*T)->rchild->rchild=(BiTNode*)malloc(sizeof(BiTNode));    (*T)->rchild->rchild->data=7;    (*T)->rchild->rchild->lchild=NULL;    (*T)->rchild->rchild->rchild=NULL;    (*T)->lchild->lchild->data=4;    (*T)->lchild->lchild->lchild=NULL;    (*T)->lchild->lchild->rchild=NULL;}//弹栈函数void pop( ){    if (top==-1) {        return ;    }    top--;}//模拟操作结点元素的函数，输出结点本身的数值void displayElem(BiTNode* elem){    printf("%d ",elem->data);}//后序遍历非递归算法typedef struct SNode{    BiTree p;    int tag;}SNode;//后序遍历使用的进栈函数void postpush(SNode *a,SNode sdata){    a[++top]=sdata;}//后序遍历函数void PostOrderTraverse(BiTree Tree){    SNode a[20];//定义一个顺序栈    BiTNode * p;//临时指针    int tag;    SNode sdata;    p=Tree;    while (p||top!=-1) {        while (p) {            //为该结点入栈做准备            sdata.p=p;            sdata.tag=0;//由于遍历是左孩子，设置标志位为0            postpush(a, sdata);//压栈            p=p->lchild;//以该结点为根结点，遍历左孩子        }        sdata=a[top];//取栈顶元素        pop();//栈顶元素弹栈        p=sdata.p;        tag=sdata.tag;        //如果tag==0，说明该结点还没有遍历它的右孩子        if (tag==0) {            sdata.p=p;            sdata.tag=1;            postpush(a, sdata);//更改该结点的标志位，重新压栈            p=p->rchild;//以该结点的右孩子为根结点，重复循环        }        //如果取出来的栈顶元素的tag==1，说明此结点左右子树都遍历完了，可以调用操作函数了        else{            displayElem(p);            p=NULL;        }    }}int main(){    BiTree Tree;    CreateBiTree(&Tree);    printf("后序遍历: \n");    PostOrderTraverse(Tree);}
```

运行结果

后序遍历:
4 5 2 6 7 3 1

前边介绍了

[二叉树](http://data.biancheng.net/view/192.html)

的先序、中序和后序的遍历算法，运用了

[栈](http://data.biancheng.net/view/169.html)

的数据结构，主要思想就是按照先左子

[树](http://data.biancheng.net/view/23.html)

后右子树的顺序依次遍历树中各个结点。

本节介绍另外一种遍历方式：按照二叉树中的层次从左到右依次遍历每层中的结点。具体的实现思路是：通过使用

[队列](http://data.biancheng.net/view/172.html)

的数据结构，从树的根结点开始，依次将其左孩子和右孩子入队。而后每次队列中一个结点出队，都将其左孩子和右孩子入队，直到树中所有结点都出队，出队结点的先后顺序就是层次遍历的最终结果。

![img](https://raw.githubusercontent.com/lindage1994/images/master/typora202009/25/205519-968023.png)
[图](http://data.biancheng.net/view/200.html)1 二叉树

## 层次遍历的实现过程

例如，层次遍历图 1 中的二叉树：

- 首先，根结点 1 入队；
- 根结点 1 出队，出队的同时，将左孩子 2 和右孩子 3 分别入队；
- 队头结点 2 出队，出队的同时，将结点 2 的左孩子 4 和右孩子 5 依次入队；
- 队头结点 3 出队，出队的同时，将结点 3 的左孩子 6 和右孩子 7 依次入队；
- 不断地循环，直至队列内为空。

## 实现代码

```
纯文本复制
#include <stdio.h>#define TElemType int//初始化队头和队尾指针开始时都为0int front=0,rear=0;typedef struct BiTNode{    TElemType data;//数据域    struct BiTNode *lchild,*rchild;//左右孩子指针}BiTNode,*BiTree;void CreateBiTree(BiTree *T){    *T=(BiTNode*)malloc(sizeof(BiTNode));    (*T)->data=1;    (*T)->lchild=(BiTNode*)malloc(sizeof(BiTNode));    (*T)->rchild=(BiTNode*)malloc(sizeof(BiTNode));       (*T)->lchild->data=2;    (*T)->lchild->lchild=(BiTNode*)malloc(sizeof(BiTNode));    (*T)->lchild->rchild=(BiTNode*)malloc(sizeof(BiTNode));    (*T)->lchild->rchild->data=5;    (*T)->lchild->rchild->lchild=NULL;    (*T)->lchild->rchild->rchild=NULL;       (*T)->rchild->data=3;    (*T)->rchild->lchild=(BiTNode*)malloc(sizeof(BiTNode));    (*T)->rchild->lchild->data=6;    (*T)->rchild->lchild->lchild=NULL;    (*T)->rchild->lchild->rchild=NULL;       (*T)->rchild->rchild=(BiTNode*)malloc(sizeof(BiTNode));    (*T)->rchild->rchild->data=7;    (*T)->rchild->rchild->lchild=NULL;    (*T)->rchild->rchild->rchild=NULL;       (*T)->lchild->lchild->data=4;    (*T)->lchild->lchild->lchild=NULL;    (*T)->lchild->lchild->rchild=NULL;}//入队函数void EnQueue(BiTree *a,BiTree node){    a[rear++]=node;}//出队函数BiTNode* DeQueue(BiTNode** a){    return a[front++];}//输出函数void displayNode(BiTree node){    printf("%d ",node->data);}int main() {    BiTree tree;    //初始化二叉树    CreateBiTree(&tree);    BiTNode * p;    //采用顺序队列，初始化创建队列数组    BiTree a[20];    //根结点入队    EnQueue(a, tree);    //当队头和队尾相等时，表示队列为空    while(front<rear) {        //队头结点出队        p=DeQueue(a);        displayNode(p);        //将队头结点的左右孩子依次入队        if (p->lchild!=NULL) {            EnQueue(a, p->lchild);        }        if (p->rchild!=NULL) {            EnQueue(a, p->rchild);        }    }    return 0;}
```

运行结果：

1 2 3 4 5 6 7

前面讲了

[二叉树](http://data.biancheng.net/view/192.html)

的顺序存储和链式存储，本节来学习如何存储具有普通

[树](http://data.biancheng.net/view/23.html)

结构的数据。

![普通树存储结构](https://raw.githubusercontent.com/lindage1994/images/master/typora202009/25/205603-532051.gif)
[图](http://data.biancheng.net/view/200.html) 1 普通树存储结构


如图 1 所示，这是一棵普通的树，该如何存储呢？通常，存储具有普通树结构数据的方法有 3 种：

1. 双亲表示法；
2. [孩子表示法](http://data.biancheng.net/view/197.html)；
3. [孩子兄弟表示法](http://data.biancheng.net/view/198.html)；


本节先来学习双亲表示法。

双亲表示法采用[顺序表](http://data.biancheng.net/view/158.html)（也就是[数组](http://data.biancheng.net/view/181.html)）存储普通树，其实现的核心思想是：顺序存储各个节点的同时，给各节点附加一个记录其父节点位置的变量。

注意，根节点没有父节点（父节点又称为双亲节点），因此根节点记录父节点位置的变量通常置为 -1。

例如，采用双亲表示法存储图 1 中普通树，其存储状态如图 2 所示：

![双亲表示法存储普通树示意图](https://raw.githubusercontent.com/lindage1994/images/master/typora202009/25/205615-579335.gif)
图 2 双亲表示法存储普通树示意图


图 2 存储普通树的过程转化为 C 语言代码为：

```
#define MAX_SIZE 100//宏定义树中结点的最大数量typedef char ElemType;//宏定义树结构中数据类型typedef struct Snode{    TElemType data;//树中结点的数据类型    int parent;//结点的父结点在数组中的位置下标}PTNode;typedef struct {    PTNode tnode[MAX_SIZE];//存放树中所有结点    int n;//根的位置下标和结点数}PTree;
```


因此，存储图 1 中普通树的 C 语言实现代码为：

```
纯文本复制
#include<stdio.h>#include<stdlib.h>#define MAX_SIZE 20typedef char ElemType;//宏定义树结构中数据类型typedef struct Snode  //结点结构{    ElemType data;    int parent;}PNode;typedef struct  //树结构{    PNode tnode[MAX_SIZE];    int n;                 //结点个数}PTree;PTree InitPNode(PTree tree){    int i,j;    char ch;    printf("请输出节点个数:\n");    scanf("%d",&(tree.n));    printf("请输入结点的值其双亲位于数组中的位置下标:\n");    for(i=0; i<tree.n; i++)    {        fflush(stdin);        scanf("%c %d",&ch,&j);        tree.tnode[i].data = ch;        tree.tnode[i].parent = j;    }    return tree;}void FindParent(PTree tree){    char a;    int isfind = 0;    printf("请输入要查询的结点值:\n");    fflush(stdin);    scanf("%c",&a);    for(int i =0;i<tree.n;i++){        if(tree.tnode[i].data == a){            isfind=1;            int ad=tree.tnode[i].parent;            printf("%c的父节点为 %c,存储位置下标为 %d",a,tree.tnode[ad].data,ad);            break;        }    }    if(isfind == 0){        printf("树中无此节点");    }}int main(){    PTree tree;    tree = InitPNode(tree);    FindParent(tree);    return 0;}
```

程序运行示例：

请输出节点个数:
10
请输入结点的值其双亲位于数组中的位置下标:
R -1
A 0
B 0
C 0
D 1
E 1
F 3
G 6
H 6
K 6
请输入要查询的结点值:
C
C的父节点为 R,存储位置下标为 0

前面学习了如何用

[双亲表示法](http://data.biancheng.net/view/196.html)

存储普通

[树](http://data.biancheng.net/view/23.html)

，本节再学习一种存储普通树的方法——孩子表示法。

孩子表示法存储普通树采用的是 "

[顺序表](http://data.biancheng.net/view/158.html)

+

[链表](http://data.biancheng.net/view/160.html)

" 的组合结构，其存储过程是：从树的根节点开始，使用顺序表依次存储树中各个节点，需要注意的是，与双亲表示法不同，孩子表示法会给各个节点配备一个链表，用于存储各节点的孩子节点位于顺序表中的位置。

如果节点没有孩子节点（叶子节点），则该节点的链表为空链表。

例如，使用孩子表示法存储

[图](http://data.biancheng.net/view/200.html)

 1a) 中的普通树，则最终存储状态如图 1b) 所示：

![img](https://raw.githubusercontent.com/lindage1994/images/master/typora202009/25/205644-768408.gif)
图 1 孩子表示法存储普通树示意图


图 1 所示转化为 C 语言代码为：

```
#include<stdio.h>#include<stdlib.h>#define MAX_SIZE 20#define TElemType char//孩子表示法typedef struct CTNode{    int child;//链表中每个结点存储的不是数据本身，而是数据在数组中存储的位置下标    struct CTNode * next;}ChildPtr;typedef struct {    TElemType data;//结点的数据类型    ChildPtr* firstchild;//孩子链表的头指针}CTBox;typedef struct{    CTBox nodes[MAX_SIZE];//存储结点的数组    int n,r;//结点数量和树根的位置}CTree;//孩子表示法存储普通树CTree initTree(CTree tree){    printf("输入节点数量：\n");    scanf("%d",&(tree.n));    for(int i=0;i<tree.n;i++){        printf("输入第 %d 个节点的值：\n",i+1);        fflush(stdin);        scanf("%c",&(tree.nodes[i].data));        tree.nodes[i].firstchild=(ChildPtr*)malloc(sizeof(ChildPtr));        tree.nodes[i].firstchild->next=NULL;        printf("输入节点 %c 的孩子节点数量：\n",tree.nodes[i].data);        int Num;        scanf("%d",&Num);        if(Num!=0){            ChildPtr * p = tree.nodes[i].firstchild;            for(int j = 0 ;j<Num;j++){                ChildPtr * newEle=(ChildPtr*)malloc(sizeof(ChildPtr));                newEle->next=NULL;                printf("输入第 %d 个孩子节点在顺序表中的位置",j+1);                scanf("%d",&(newEle->child));                p->next= newEle;                p=p->next;            }        }    }    return tree;}void findKids(CTree tree,char a){    int hasKids=0;    for(int i=0;i<tree.n;i++){        if(tree.nodes[i].data==a){            ChildPtr * p=tree.nodes[i].firstchild->next;            while(p){                hasKids = 1;                printf("%c ",tree.nodes[p->child].data);                p=p->next;            }            break;        }    }    if(hasKids==0){        printf("此节点为叶子节点");    }}int main(){    CTree tree;    tree = initTree(tree);    //默认数根节点位于数组notes[0]处    tree.r=0;    printf("找出节点 F 的所有孩子节点：");    findKids(tree,'F');    return 0;}
```

程序运行结果为：

输入节点数量：
10
输入第 1 个节点的值：
R
输入节点 R 的孩子节点数量：
3
输入第 1 个孩子节点在顺序表中的位置1
输入第 2 个孩子节点在顺序表中的位置2
输入第 3 个孩子节点在顺序表中的位置3
输入第 2 个节点的值：
A
输入节点 A 的孩子节点数量：
2
输入第 1 个孩子节点在顺序表中的位置4
输入第 2 个孩子节点在顺序表中的位置5
输入第 3 个节点的值：
B
输入节点 B 的孩子节点数量：
0
输入第 4 个节点的值：
C
输入节点 C 的孩子节点数量：
1
输入第 1 个孩子节点在顺序表中的位置6
输入第 5 个节点的值：
D
输入节点 D 的孩子节点数量：
0
输入第 6 个节点的值：
E
输入节点 E 的孩子节点数量：
0
输入第 7 个节点的值：
F
输入节点 F 的孩子节点数量：
3
输入第 1 个孩子节点在顺序表中的位置7
输入第 2 个孩子节点在顺序表中的位置8
输入第 3 个孩子节点在顺序表中的位置9
输入第 8 个节点的值：
G
输入节点 G 的孩子节点数量：
0
输入第 9 个节点的值：
H
输入节点 H 的孩子节点数量：
0
输入第 10 个节点的值：
K
输入节点 K 的孩子节点数量：
0
找出节点 F 的所有孩子节点：G H K

使用孩子表示法存储的树结构，正好和双亲表示法相反，适用于查找某结点的孩子结点，不适用于查找其父结点。

其实，我们还可以将双亲表示法和孩子表示法合二为一，那么图 1a) 中普通树的存储效果如图 2所示：

![img](https://raw.githubusercontent.com/lindage1994/images/master/typora202009/25/205701-936807.gif)
图 2 双亲孩子表示法


使用图 2 结构存储普通树，既能快速找到指定节点的父节点，又能快速找到指定节点的孩子节点。该结构的实现方法很简单，只需整合这两节的代码即可，因此不再赘述。

前面讲解了存储普通

[树](http://data.biancheng.net/view/23.html)

的

[双亲表示法](http://data.biancheng.net/view/196.html)

和

[孩子表示法](http://data.biancheng.net/view/197.html)

，本节来讲解最后一种常用方法——孩子兄弟表示法。

![普通树示意图](https://raw.githubusercontent.com/lindage1994/images/master/typora202009/25/205954-945731.gif)
[图](http://data.biancheng.net/view/200.html) 1 普通树示意图


树结构中，位于同一层的节点之间互为兄弟节点。例如，图 1 的普通树中，节点 A、B 和 C 互为兄弟节点，而节点 D、E 和 F 也互为兄弟节点。

孩子兄弟表示法，采用的是链式存储结构，其存储树的实现思想是：从树的根节点开始，依次用[链表](http://data.biancheng.net/view/160.html)存储各个节点的孩子节点和兄弟节点。

因此，该链表中的节点应包含以下 3 部分内容（如图 2 所示）：

1. 节点的值；
2. 指向孩子节点的指针；
3. 指向兄弟节点的指针；

![节点结构示意图](https://raw.githubusercontent.com/lindage1994/images/master/typora202009/25/210007-296263.gif)
图 2 节点结构示意图


用 C 语言代码表示节点结构为：

```
#define ElemType chartypedef struct CSNode{    ElemType data;    struct CSNode * firstchild,*nextsibling;}CSNode,*CSTree;
```


以图 1 为例，使用孩子兄弟表示法进行存储的结果如图 3 所示:

![孩子兄弟表示法示意图](https://raw.githubusercontent.com/lindage1994/images/master/typora202009/25/210019-391738.gif)
图 3 孩子兄弟表示法示意图


由图 3 可以看到，节点 R 无兄弟节点，其孩子节点是 A；节点 A 的兄弟节点分别是 B 和 C，其孩子节点为 D，依次类推。

实现图 3 中的 C 语言实现代码也很简单，根据图中链表的结构即可轻松完成链表的创建和使用，因此不再给出具体代码。

接下来观察图 1 和图 3。图 1 为原普通树，图 3 是由图 1 经过孩子兄弟表示法转化而来的一棵树，确切地说，图 3 是一棵

[二叉树](http://data.biancheng.net/view/192.html)

。因此可以得出这样一个结论，即通过孩子兄弟表示法，任意一棵普通树都可以相应转化为一棵二叉树，换句话说，任意一棵普通树都有唯一的一棵二叉树于其对应。

因此，孩子兄弟表示法可以作为将普通树转化为二叉树的最有效方法，通常又被称为"二叉树表示法"或"二叉链表表示法"。



赫夫曼

[树](http://data.biancheng.net/view/23.html)

，别名“哈夫曼树”、“最优树”以及“最优

[二叉树](http://data.biancheng.net/view/192.html)

”。学习哈夫曼树之前，首先要了解几个名词。

## 哈夫曼树相关的几个名词

路径：在一棵树中，一个结点到另一个结点之间的通路，称为路径。

[图](http://data.biancheng.net/view/200.html)

 1 中，从根结点到结点 a 之间的通路就是一条路径。

路径长度：在一条路径中，每经过一个结点，路径长度都要加 1 。例如在一棵树中，规定根结点所在层数为1层，那么从根结点到第 i 层结点的路径长度为 i - 1 。图 1 中从根结点到结点 c 的路径长度为 3。

结点的权：给每一个结点赋予一个新的数值，被称为这个结点的权。例如，图 1 中结点 a 的权为 7，结点 b 的权为 5。

结点的带权路径长度：指的是从根结点到该结点之间的路径长度与该结点的权的乘积。例如，图 1 中结点 b 的带权路径长度为 2 * 5 = 10 。

树的带权路径长度为树中所有叶子结点的带权路径长度之和。通常记作 “WPL” 。例如图 1 中所示的这颗树的带权路径长度为：

> WPL = 7 * 1 + 5 * 2 + 2 * 3 + 4 * 3

![img](https://raw.githubusercontent.com/lindage1994/images/master/typora202009/25/210036-583075.png)
图1 哈夫曼树

## 什么是哈夫曼树

当用 n 个结点（都做叶子结点且都有各自的权值）试图构建一棵树时，如果构建的这棵树的带权路径长度最小，称这棵树为“最优二叉树”，有时也叫“赫夫曼树”或者“哈夫曼树”。

在构建哈弗曼树时，要使树的带权路径长度最小，只需要遵循一个原则，那就是：权重越大的结点离树根越近。在图 1 中，因为结点 a 的权值最大，所以理应直接作为根结点的孩子结点。

## 构建哈夫曼树

对于给定的有各自权值的 n 个结点，构建哈夫曼树有一个行之有效的办法：

1. 在 n 个权值中选出两个最小的权值，对应的两个结点组成一个新的二叉树，且新二叉树的根结点的权值为左右孩子权值的和；
2. 在原有的 n 个权值中删除那两个最小的权值，同时将新的权值加入到 n–2 个权值的行列中，以此类推；
3. 重复 1 和 2 ，直到所以的结点构建成了一棵二叉树为止，这棵树就是哈夫曼树。

![img](https://raw.githubusercontent.com/lindage1994/images/master/typora202009/25/210050-814637.png)

图 2 哈夫曼树的构建过程


图 2 中，（A）给定了四个结点a，b，c，d，权值分别为7，5，2，4；第一步如（B）所示，找出现有权值中最小的两个，2 和 4 ，相应的结点 c 和 d 构建一个新的二叉树，树根的权值为 2 + 4 = 6，同时将原有权值中的 2 和 4 删掉，将新的权值 6 加入；进入（C），重复之前的步骤。直到（D）中，所有的结点构建成了一个全新的二叉树，这就是哈夫曼树。

## 哈弗曼树中结点结构

构建哈夫曼树时，首先需要确定树中结点的构成。由于哈夫曼树的构建是从叶子结点开始，不断地构建新的父结点，直至树根，所以结点中应包含指向父结点的指针。但是在使用哈夫曼树时是从树根开始，根据需求遍历树中的结点，因此每个结点需要有指向其左孩子和右孩子的指针。

所以，哈夫曼树中结点构成用代码表示为：

```
//哈夫曼树结点结构typedef struct {    int weight;//结点权重    int parent, left, right;//父结点、左孩子、右孩子在数组中的位置下标}HTNode, *HuffmanTree;
```

## 哈弗曼树中的查找算法

构建哈夫曼树时，需要每次根据各个结点的权重值，筛选出其中值最小的两个结点，然后构建二叉树。

查找权重值最小的两个结点的思想是：从树组起始位置开始，首先找到两个无父结点的结点（说明还未使用其构建成树），然后和后续无父结点的结点依次做比较，有两种情况需要考虑：

- 如果比两个结点中较小的那个还小，就保留这个结点，删除原来较大的结点；
- 如果介于两个结点权重值之间，替换原来较大的结点；


实现代码：

```
//HT数组中存放的哈夫曼树，end表示HT数组中存放结点的最终位置，s1和s2传递的是HT数组中权重值最小的两个结点在数组中的位置void Select(HuffmanTree HT, int end, int *s1, int *s2){    int min1, min2;    //遍历数组初始下标为 1    int i = 1;    //找到还没构建树的结点    while(HT[i].parent != 0 && i <= end){        i++;    }    min1 = HT[i].weight;    *s1 = i;       i++;    while(HT[i].parent != 0 && i <= end){        i++;    }    //对找到的两个结点比较大小，min2为大的，min1为小的    if(HT[i].weight < min1){        min2 = min1;        *s2 = *s1;        min1 = HT[i].weight;        *s1 = i;    }else{        min2 = HT[i].weight;        *s2 = i;    }    //两个结点和后续的所有未构建成树的结点做比较    for(int j=i+1; j <= end; j++)    {        //如果有父结点，直接跳过，进行下一个        if(HT[j].parent != 0){            continue;        }        //如果比最小的还小，将min2=min1，min1赋值新的结点的下标        if(HT[j].weight < min1){            min2 = min1;            min1 = HT[j].weight;            *s2 = *s1;            *s1 = j;        }        //如果介于两者之间，min2赋值为新的结点的位置下标        else if(HT[j].weight >= min1 && HT[j].weight < min2){            min2 = HT[j].weight;            *s2 = j;        }    }}
```

> 注意：s1和s2传入的是实参的地址，所以函数运行完成后，实参中存放的自然就是哈夫曼树中权重值最小的两个结点在数组中的位置。

## 构建哈弗曼树的代码实现

```
//HT为地址传递的存储哈夫曼树的数组，w为存储结点权重值的数组，n为结点个数void CreateHuffmanTree(HuffmanTree *HT, int *w, int n){    if(n<=1) return; // 如果只有一个编码就相当于0    int m = 2*n-1; // 哈夫曼树总节点数，n就是叶子结点    *HT = (HuffmanTree) malloc((m+1) * sizeof(HTNode)); // 0号位置不用    HuffmanTree p = *HT;    // 初始化哈夫曼树中的所有结点    for(int i = 1; i <= n; i++)    {        (p+i)->weight = *(w+i-1);        (p+i)->parent = 0;        (p+i)->left = 0;        (p+i)->right = 0;    }    //从树组的下标 n+1 开始初始化哈夫曼树中除叶子结点外的结点    for(int i = n+1; i <= m; i++)    {        (p+i)->weight = 0;        (p+i)->parent = 0;        (p+i)->left = 0;        (p+i)->right = 0;    }    //构建哈夫曼树    for(int i = n+1; i <= m; i++)    {        int s1, s2;        Select(*HT, i-1, &s1, &s2);        (*HT)[s1].parent = (*HT)[s2].parent = i;        (*HT)[i].left = s1;        (*HT)[i].right = s2;        (*HT)[i].weight = (*HT)[s1].weight + (*HT)[s2].weight;    }}
```

## 哈夫曼编码

哈夫曼编码就是在哈夫曼树的基础上构建的，这种编码方式最大的优点就是用最少的字符包含最多的信息内容。

根据发送信息的内容，通过统计文本中相同字符的个数作为每个字符的权值，建立哈夫曼树。对于树中的每一个子树，统一规定其左孩子标记为 0 ，右孩子标记为 1 。这样，用到哪个字符时，从哈夫曼树的根结点开始，依次写出经过结点的标记，最终得到的就是该结点的哈夫曼编码。

> 文本中字符出现的次数越多，在哈夫曼树中的体现就是越接近树根。编码的长度越短。

![img](https://raw.githubusercontent.com/lindage1994/images/master/typora202009/25/210108-289000.png)
图 3 哈夫曼编码


如图 3 所示，字符 a 用到的次数最多，其次是字符 b 。字符 a 在哈夫曼编码是 `0` ，字符 b 编码为 `10` ，字符 c 的编码为 `110` ，字符 d 的编码为 `111` 。

使用程序求哈夫曼编码有两种方法：

1. 从叶子结点一直找到根结点，逆向记录途中经过的标记。例如，图 3 中字符 c 的哈夫曼编码从结点 c 开始一直找到根结点，结果为：0 1 1 ，所以字符 c 的哈夫曼编码为：1 1 0（逆序输出）。
2. 从根结点出发，一直到叶子结点，记录途中经过的标记。例如，求图 3 中字符 c 的哈夫曼编码，就从根结点开始，依次为：1 1 0。


采用方法 1 的实现代码为：

```
//HT为哈夫曼树，HC为存储结点哈夫曼编码的二维动态数组，n为结点的个数void HuffmanCoding(HuffmanTree HT, HuffmanCode *HC,int n){    *HC = (HuffmanCode) malloc((n+1) * sizeof(char *));    char *cd = (char *)malloc(n*sizeof(char)); //存放结点哈夫曼编码的字符串数组    cd[n-1] = '\0';//字符串结束符       for(int i=1; i<=n; i++){        //从叶子结点出发，得到的哈夫曼编码是逆序的，需要在字符串数组中逆序存放        int start = n-1;        //当前结点在数组中的位置        int c = i;        //当前结点的父结点在数组中的位置        int j = HT[i].parent;        // 一直寻找到根结点        while(j != 0){            // 如果该结点是父结点的左孩子则对应路径编码为0，否则为右孩子编码为1            if(HT[j].left == c)                cd[--start] = '0';            else                cd[--start] = '1';            //以父结点为孩子结点，继续朝树根的方向遍历            c = j;            j = HT[j].parent;        }        //跳出循环后，cd数组中从下标 start 开始，存放的就是该结点的哈夫曼编码        (*HC)[i] = (char *)malloc((n-start)*sizeof(char));        strcpy((*HC)[i], &cd[start]);    }    //使用malloc申请的cd动态数组需要手动释放    free(cd);}
```

采用第二种算法的实现代码为：

```
//HT为哈夫曼树，HC为存储结点哈夫曼编码的二维动态数组，n为结点的个数void HuffmanCoding(HuffmanTree HT, HuffmanCode *HC,int n){    *HC = (HuffmanCode) malloc((n+1) * sizeof(char *));    int m=2*n-1;    int p=m;    int cdlen=0;    char *cd = (char *)malloc(n*sizeof(char));    //将各个结点的权重用于记录访问结点的次数，首先初始化为0    for (int i=1; i<=m; i++) {        HT[i].weight=0;    }    //一开始 p 初始化为 m，也就是从树根开始。一直到p为0    while (p) {        //如果当前结点一次没有访问，进入这个if语句        if (HT[p].weight==0) {            HT[p].weight=1;//重置访问次数为1            //如果有左孩子，则访问左孩子，并且存储走过的标记为0            if (HT[p].left!=0) {                p=HT[p].left;                cd[cdlen++]='0';            }            //当前结点没有左孩子，也没有右孩子，说明为叶子结点，直接记录哈夫曼编码            else if(HT[p].right==0){                (*HC)[p]=(char*)malloc((cdlen+1)*sizeof(char));                cd[cdlen]='\0';                strcpy((*HC)[p], cd);            }        }        //如果weight为1，说明访问过一次，即是从其左孩子返回的        else if(HT[p].weight==1){            HT[p].weight=2;//设置访问次数为2            //如果有右孩子，遍历右孩子，记录标记值 1            if (HT[p].right!=0) {                p=HT[p].right;                cd[cdlen++]='1';            }        }        //如果访问次数为 2，说明左右孩子都遍历完了，返回父结点        else{            HT[p].weight=0;            p=HT[p].parent;            --cdlen;        }    }}
```

## 本节完整代码（可运行）

```
#include<stdlib.h>#include<stdio.h>#include<string.h>//哈夫曼树结点结构typedef struct {    int weight;//结点权重    int parent, left, right;//父结点、左孩子、右孩子在数组中的位置下标}HTNode, *HuffmanTree;//动态二维数组，存储哈夫曼编码typedef char ** HuffmanCode;//HT数组中存放的哈夫曼树，end表示HT数组中存放结点的最终位置，s1和s2传递的是HT数组中权重值最小的两个结点在数组中的位置void Select(HuffmanTree HT, int end, int *s1, int *s2){    int min1, min2;    //遍历数组初始下标为 1    int i = 1;    //找到还没构建树的结点    while(HT[i].parent != 0 && i <= end){        i++;    }    min1 = HT[i].weight;    *s1 = i;       i++;    while(HT[i].parent != 0 && i <= end){        i++;    }    //对找到的两个结点比较大小，min2为大的，min1为小的    if(HT[i].weight < min1){        min2 = min1;        *s2 = *s1;        min1 = HT[i].weight;        *s1 = i;    }else{        min2 = HT[i].weight;        *s2 = i;    }    //两个结点和后续的所有未构建成树的结点做比较    for(int j=i+1; j <= end; j++)    {        //如果有父结点，直接跳过，进行下一个        if(HT[j].parent != 0){            continue;        }        //如果比最小的还小，将min2=min1，min1赋值新的结点的下标        if(HT[j].weight < min1){            min2 = min1;            min1 = HT[j].weight;            *s2 = *s1;            *s1 = j;        }        //如果介于两者之间，min2赋值为新的结点的位置下标        else if(HT[j].weight >= min1 && HT[j].weight < min2){            min2 = HT[j].weight;            *s2 = j;        }    }}//HT为地址传递的存储哈夫曼树的数组，w为存储结点权重值的数组，n为结点个数void CreateHuffmanTree(HuffmanTree *HT, int *w, int n){    if(n<=1) return; // 如果只有一个编码就相当于0    int m = 2*n-1; // 哈夫曼树总节点数，n就是叶子结点    *HT = (HuffmanTree) malloc((m+1) * sizeof(HTNode)); // 0号位置不用    HuffmanTree p = *HT;    // 初始化哈夫曼树中的所有结点    for(int i = 1; i <= n; i++)    {        (p+i)->weight = *(w+i-1);        (p+i)->parent = 0;        (p+i)->left = 0;        (p+i)->right = 0;    }    //从树组的下标 n+1 开始初始化哈夫曼树中除叶子结点外的结点    for(int i = n+1; i <= m; i++)    {        (p+i)->weight = 0;        (p+i)->parent = 0;        (p+i)->left = 0;        (p+i)->right = 0;    }    //构建哈夫曼树    for(int i = n+1; i <= m; i++)    {        int s1, s2;        Select(*HT, i-1, &s1, &s2);        (*HT)[s1].parent = (*HT)[s2].parent = i;        (*HT)[i].left = s1;        (*HT)[i].right = s2;        (*HT)[i].weight = (*HT)[s1].weight + (*HT)[s2].weight;    }}//HT为哈夫曼树，HC为存储结点哈夫曼编码的二维动态数组，n为结点的个数void HuffmanCoding(HuffmanTree HT, HuffmanCode *HC,int n){    *HC = (HuffmanCode) malloc((n+1) * sizeof(char *));    char *cd = (char *)malloc(n*sizeof(char)); //存放结点哈夫曼编码的字符串数组    cd[n-1] = '\0';//字符串结束符       for(int i=1; i<=n; i++){        //从叶子结点出发，得到的哈夫曼编码是逆序的，需要在字符串数组中逆序存放        int start = n-1;        //当前结点在数组中的位置        int c = i;        //当前结点的父结点在数组中的位置        int j = HT[i].parent;        // 一直寻找到根结点        while(j != 0){            // 如果该结点是父结点的左孩子则对应路径编码为0，否则为右孩子编码为1            if(HT[j].left == c)                cd[--start] = '0';            else                cd[--start] = '1';            //以父结点为孩子结点，继续朝树根的方向遍历            c = j;            j = HT[j].parent;        }        //跳出循环后，cd数组中从下标 start 开始，存放的就是该结点的哈夫曼编码        (*HC)[i] = (char *)malloc((n-start)*sizeof(char));        strcpy((*HC)[i], &cd[start]);    }    //使用malloc申请的cd动态数组需要手动释放    free(cd);}//打印哈夫曼编码的函数void PrintHuffmanCode(HuffmanCode htable,int *w,int n){    printf("Huffman code : \n");    for(int i = 1; i <= n; i++)        printf("%d code = %s\n",w[i-1], htable[i]);}int main(void){    int w[5] = {2, 8, 7, 6, 5};    int n = 5;    HuffmanTree htree;    HuffmanCode htable;    CreateHuffmanTree(&htree, w, n);    HuffmanCoding(htree, &htable, n);    PrintHuffmanCode(htable,w, n);    return 0;}
```

运行结果

Huffman code :
2 code = 100
8 code = 11
7 code = 01
6 code = 00
5 code = 101

> 本节中介绍了两种遍历哈夫曼树获得哈夫曼编码的方法，同时也给出了各自完整的实现代码的函数，在完整代码中使用的是第一种逆序遍历哈夫曼树的方法。

## 总结

![img](https://raw.githubusercontent.com/lindage1994/images/master/typora202009/25/210123-191164.png)
图 4 程序运行效果图


本节的程序中对权重值分别为 2，8，7，6，5 的结点构建的哈夫曼树如图 4（A）所示。图 4（B）是另一个哈夫曼树，两棵树的带权路径长度相同。

程序运行效果图之所以是（A）而不是（B），原因是在构建哈夫曼树时，结点 2 和结点 5 构建的新的结点 7 存储在动态树组的最后面，所以，在程序继续选择两个权值最小的结点时，直接选择了的叶子结点 6 和 7 。

回溯法，又被称为“试探法”。解决问题时，每进行一步，都是抱着试试看的态度，如果发现当前选择并不是最好的，或者这么走下去肯定达不到目标，立刻做回退操作重新选择。这种走不通就回退再走的方法就是回溯法。

例如，在解决列举集合 {1,2,3} 中所有子集的问题中，就可以使用回溯法。从集合的开头元素开始，对每个元素都有两种选择：取还是舍。当确定了一个元素的取舍之后，再进行下一个元素，直到集合最后一个元素。其中的每个操作都可以看作是一次尝试，每次尝试都可以得出一个结果。将得到的结果综合起来，就是集合的所有子集。

实现代码为：

```
#include <stdio.h>//设置一个数组，数组的下标表示集合中的元素，所以数组只用下标为1，2，3的空间int set[5];//i代表数组下标，n表示集合中最大的元素值void PowerSet(int i,int n){    //当i>n时，说明集合中所有的元素都做了选择，开始判断    if (i>n) {        for (int j=1; j<=n; j++) {            //如果树组中存放的是 1，说明在当初尝试时，选择取该元素，即对应的数组下标，所以，可以输出            if (set[j]==1) {                printf("%d ",j);            }        }        printf("\n");    }else{        //如果选择要该元素，对应的数组单元中赋值为1；反之，赋值为0。然后继续向下探索        set[i]=1;PowerSet(i+1, n);        set[i]=0;PowerSet(i+1, n);    }}int main() {    int n=3;    for (int i=0; i<5; i++) {        set[i]=0;    }    PowerSet(1, n);    return 0;}
```

运行结果：

1 2 3
1 2
1 3
1
2 3
2
3

## 回溯VS递归

很多人认为回溯和递归是一样的，其实不然。在回溯法中可以看到有递归的身影，但是两者是有区别的。

回溯法从问题本身出发，寻找可能实现的所有情况。和穷举法的思想相近，不同在于穷举法是将所有的情况都列举出来以后再一一筛选，而回溯法在列举过程如果发现当前情况根本不可能存在，就停止后续的所有工作，返回上一步进行新的尝试。

递归是从问题的结果出发，例如求 n！，要想知道 n！的结果，就需要知道 n*(n-1)! 的结果，而要想知道 (n-1)! 结果，就需要提前知道 (n-1)*(n-2)!。这样不断地向自己提问，不断地调用自己的思想就是递归。

回溯和递归唯一的联系就是，回溯法可以用递归思想实现。

## 回溯法与树的遍历

使用回溯法解决问题的过程，实际上是建立一棵“状态树”的过程。例如，在解决列举集合{1,2,3}所有子集的问题中，对于每个元素，都有两种状态，取还是舍，所以构建的状态树为：

![img](https://raw.githubusercontent.com/lindage1994/images/master/typora202009/25/210141-843530.png)
[图](http://data.biancheng.net/view/200.html)1 状态树


回溯法的求解过程实质上是先序遍历“状态树”的过程。树中每一个叶子结点，都有可能是问题的答案。图 1 中的状态树是满

[二叉树](http://data.biancheng.net/view/192.html)

，得到的叶子结点全部都是问题的解。

在某些情况下，回溯法解决问题的过程中创建的状态树并不都是满二叉树，因为在试探的过程中，有时会发现此种情况下，再往下进行没有意义，所以会放弃这条死路，回溯到上一步。在树中的体现，就是在树的最后一层不是满的，即不是满二叉树，需要自己判断哪些叶子结点代表的是正确的结果。

## 回溯法解决八皇后问题

八皇后问题是以国际象棋为背景的问题：有八个皇后（可以当成八个棋子），如何在 8*8 的棋盘中放置八个皇后，使得任意两个皇后都不在同一条横线、纵线或者斜线上。

![img](https://raw.githubusercontent.com/lindage1994/images/master/typora202009/25/210202-683302.png)
图 2 八皇后问题示例（#代表皇后）


八皇后问题是使用回溯法解决的典型案例。算法的解决思路是：

1. 从棋盘的第一行开始，从第一个位置开始，依次判断当前位置是否能够放置皇后，判断的依据为：同该行之前的所有行中皇后的所在位置进行比较，如果在同一列，或者在同一条斜线上（斜线有两条，为正方形的两个对角线），都不符合要求，继续检验后序的位置。
2. 如果该行所有位置都不符合要求，则回溯到前一行，改变皇后的位置，继续试探。
3. 如果试探到最后一行，所有皇后摆放完毕，则直接打印出 8*8 的棋盘。最后一定要记得将棋盘恢复原样，避免影响下一次摆放。


实现代码：

```
纯文本复制
#include <stdio.h>int Queenes[8]={0},Counts=0;int Check(int line,int list){    //遍历该行之前的所有行    for (int index=0; index<line; index++) {        //挨个取出前面行中皇后所在位置的列坐标        int data=Queenes[index];        //如果在同一列，该位置不能放        if (list==data) {            return 0;        }        //如果当前位置的斜上方有皇后，在一条斜线上，也不行        if ((index+data)==(line+list)) {            return 0;        }        //如果当前位置的斜下方有皇后，在一条斜线上，也不行        if ((index-data)==(line-list)) {            return 0;        }    }    //如果以上情况都不是，当前位置就可以放皇后    return 1;}//输出语句void print(){    for (int line = 0; line < 8; line++)    {        int list;        for (list = 0; list < Queenes[line]; list++)            printf("0");        printf("#");        for (list = Queenes[line] + 1; list < 8; list++){            printf("0");        }        printf("\n");    }    printf("================\n");}void eight_queen(int line){    //在数组中为0-7列    for (int list=0; list<8; list++) {        //对于固定的行列，检查是否和之前的皇后位置冲突        if (Check(line, list)) {            //不冲突，以行为下标的数组位置记录列数            Queenes[line]=list;            //如果最后一样也不冲突，证明为一个正确的摆法            if (line==7) {                //统计摆法的Counts加1                Counts++;                //输出这个摆法                print();                //每次成功，都要将数组重归为0                Queenes[line]=0;                return;            }            //继续判断下一样皇后的摆法，递归            eight_queen(line+1);            //不管成功失败，该位置都要重新归0，以便重复使用。            Queenes[line]=0;        }    }}int main() {    //调用回溯函数，参数0表示从棋盘的第一行开始判断    eight_queen(0);    printf("摆放的方式有%d种",Counts);    return 0;}
```

大家可以自己运行一下程序，查看运行结果，由于八皇后问题有92种摆法，这里不一一列举。

本节要讨论的是当给定 n（n>=0）个结点时，可以构建多少种形态不同的

[树](http://data.biancheng.net/view/23.html)

。

> 如果两棵树中各个结点的位置都一一对应，可以说这两棵树相似。如果两棵树不仅相似，而且对应结点上的数据也相同，就可以说这两棵树等价。本节中，形态不同的树指的是互不相似的树。

前面介绍过，对于任意一棵普通树，通过

[孩子兄弟表示法](http://data.biancheng.net/view/198.html)

的转化，都可以找到唯一的一棵

[二叉树](http://data.biancheng.net/view/192.html)

与之对应。所以本节研究的题目也可以转化成：n 个结点可以构建多少种形态不同的二叉树。

每一棵普通树对应的都是一棵没有右子树的二叉树，所以对于 n 个结点的树来说，树的形态改变是因为除了根结点之外的其它结点改变形态得到的，所以，n 个结点构建的形态不同的树与之对应的是 n-1 个结点构建的形态不同的二叉树。

如果 tn 表示 n 个结点构建的形态不同的树的数量，bn 表示 n 个结点构建的形态不同的二叉树的数量，则两者之间有这样的关系：`tn=bn-1`。

## 方法一

最直接的一种方法就是推理。当 n=0 时，只能构建一棵空树；当 n=2 时，可以构建 2 棵形态不同的二叉树，如

[图](http://data.biancheng.net/view/200.html)

 1（A）；当 n=3 时，可以构建 5 棵形态互不相同的二叉树，如图 1（B）。

![img](https://raw.githubusercontent.com/lindage1994/images/master/typora202009/25/210212-227701.png)
图 1 不同形态的二叉树


对于具有 n（ n>1 ）个结点的二叉树来说，都可以看成是一个根结点、由 i 个结点组成的左子树和由 `n-i-1` 个结点组成的右子树。

> 当 n=1 时，也适用，只不过只有一个根结点，没有左右孩子（i=0）。

可以得出一个递推公式：
![img](http://data.biancheng.net/uploads/allimg/170905/2-1FZ5104141558.png)
通过对公式一步步的数学推算，最后得出，含有 n 个结点的不相似的二叉树的数量为：
![img](http://data.biancheng.net/uploads/allimg/170905/2-1FZ5104202Y5.png)

## 方法二

从遍历二叉树的角度进行分析，对于任意一棵二叉树来说，它的前序序列和中序序列以及后序序列都是唯一的。其实是这句话还可以倒过来说，只要确定了一棵二叉树的三种遍历序列中的两种，那么这棵二叉树也可以唯一确定。

例如，给定了一个二叉树的前序序列和中序序列分别为：

前序序列：A B C D E F G
中序序列：C B E D A F G

可以唯一得到的二叉树如图 2（4）：

![img](https://raw.githubusercontent.com/lindage1994/images/master/typora202009/25/210222-652579.png)
图 2 构造二叉树的过程示意图


分析：通过前序序列得知，结点A为二叉树的根结点，结合中序序列，在结点 A 左侧的肯定为其左孩子中的所有结点，右边为右孩子的所有结点，如图 2（1）所示。

再分析 A 结点的左孩子，在前序序列看到，结点 A 后紧跟的是结点 B，由此断定结点 A 的左孩子是 B，再看中序序列，结点 B 左侧只有一个结点 C ，为 B 的左孩子，结点 B 右侧的结点E 和 D 为右孩子，如图 2（2）。

再分析结点 B 的右孩子，前序序列看到，结点 D 在 E 的前边，所有 D 为 B 的右孩子。在中序序列中，结点 E 在 D 前边，说明 E 是 D 的左孩子，如图 2（3）。

最后分析结点 A 的右孩子，由前序序列看到， F 在 G 前边，说明F为根结点。在中序序列中也是如此，说明，G 是 F 的右孩子。如图 2（4）所示。

如果要唯一确定一棵二叉树，必须知道至少两种遍历序列。如果只确定一种序列，无法准确判定二叉树的具体构造。

![img](https://raw.githubusercontent.com/lindage1994/images/master/typora202009/25/210232-368191.png)
图 3 前序序列（1，2，3）的二叉树


如图 3 所示为前序序列（1，2，3）构建的不同形态的二叉树，他们的中序序列各不相同。所以不同形态二叉树的数目恰好就是前序序列一定的情况下，所能得到的不同的中序序列的个数。

中序序列是对二叉树进行中序遍历获得的，遍历的过程实质上就是结点数据进

[栈](http://data.biancheng.net/view/169.html)

出栈的过程。所以，中序序列的个数就是数列（1，2，3）按1-2-3的顺序进栈，
各元素选择在不同的时间点出栈，所获的的不同的出栈顺序即为中序序列，而中序序列的数目，也就是不同形态的二叉树的个数。

![img](https://raw.githubusercontent.com/lindage1994/images/master/typora202009/25/210240-701875.png)
图 4 中序遍历时进栈和出栈的过程



根据数列中数据的个数 n，所得到的排列顺序的数目为：
![img](https://raw.githubusercontent.com/lindage1994/images/master/typora202009/25/210324-409862.png)
通过以上两种方式，都可以知道n个结点能构建的不同形态的二叉树的数量，再结合 tn=bn-1，就可以计算出 n 个结点能构建的不同形态的树的个数。