



我们知道，数据之间的关系有 3 种，分别是 "一对一"、"一对多" 和 "多对多"，前两种关系的数据可分别用**线性表**和**树**结构存储，本节学习存储具有"多对多"逻辑关系数据的结构——图存储结构。

![img](https://raw.githubusercontent.com/lindage1994/images/master/typora202009/26/194422-599202.gif)
图 1 图存储结构示意图


图 1 所示为存储 V1、V2、V3、V4 的图结构，从图中可以清楚的看出数据之间具有的"多对多"关系。例如，V1 与 V4 和 V2 建立着联系，V4 与 V1 和 V3 建立着联系，以此类推。

与**链表**不同，图中存储的各个数据元素被称为顶点（而不是节点）。拿图 1 来说，该图中含有 4 个顶点，分别为顶点 V1、V2、V3 和 V4。

图存储结构中，习惯上用 Vi 表示图中的顶点，且所有顶点构成的集合通常用 V 表示，如图 1 中顶点的集合为 V={V1,V2,V3,V4}。


注意，图 1 中的图仅是图存储结构的其中一种，数据之间 "多对多" 的关系还可能用如图 2 所示的图结构表示：

![img](https://raw.githubusercontent.com/lindage1994/images/master/typora202009/26/194449-605007.gif)
图 2 有向图示意图


可以看到，各个顶点之间的关系并不是"双向"的。比如，V4 只与 V1 存在联系（从 V4 可直接找到 V1），而与 V3 没有直接联系；同样，V3 只与 V4 存在联系（从 V3 可直接找到 V4），而与 V1 没有直接联系，以此类推。

因此，图存储结构可细分两种表现类型，分别为无向图（图 1）和有向图（图 2）。

## 图的基本常识

#### 弧头和弧尾

有向图中，无箭头一端的顶点通常被称为"初始点"或"弧尾"，箭头直线的顶点被称为"终端点"或"弧头"。

#### 入度和出度

对于有向图中的一个顶点 V 来说，箭头指向 V 的弧的数量为 V 的入度（InDegree，记为 ID(V)）；箭头远离 V 的弧的数量为 V 的出度（OutDegree，记为OD(V)）。拿图 2 中的顶点 V1来说，该顶点的入度为 1，出度为 2（该顶点的度为 3）。

#### (V1,V2) 和 <V1,V2> 的区别

无向图中描述两顶点（V1 和 V2）之间的关系可以用 (V1,V2) 来表示，而有向图中描述从 V1 到 V2 的"单向"关系用 <V1,V2> 来表示。

由于图存储结构中顶点之间的关系是用线来表示的，因此 (V1,V2) 还可以用来表示无向图中连接 V1 和 V2 的线，又称为边；同样，<V1,V2> 也可用来表示有向图中从 V1 到 V2 带方向的线，又称为弧。

#### 集合 VR 的含义

并且，图中习惯用 VR 表示图中所有顶点之间关系的集合。例如，图 1 中无向图的集合 VR={(v1,v2),(v1,v4),(v1,v3),(v3,v4)}，图 2 中有向图的集合 VR={<v1,v2>,<v1,v3>,<v3,v4>,<v4,v1>}。

#### 路径和回路

无论是无向图还是有向图，从一个顶点到另一顶点途径的所有顶点组成的序列（包含这两个顶点），称为一条路径。如果路径中第一个顶点和最后一个顶点相同，则此路径称为"回路"（或"环"）。

并且，若路径中各顶点都不重复，此路径又被称为"简单路径"；同样，若回路中的顶点互不重复，此回路被称为"简单回路"（或简单环）。

拿图 1 来说，从 V1 存在一条路径还可以回到 V1，此路径为 {V1,V3,V4,V1}，这是一个回路（环），而且还是一个简单回路（简单环）。

在有向图中，每条路径或回路都是有方向的。

#### 权和网的含义

在某些实际场景中，图中的每条边（或弧）会赋予一个实数来表示一定的含义，这种与边（或弧）相匹配的实数被称为"权"，而带权的图通常称为网。如图 3 所示，就是一个网结构：

![img](https://raw.githubusercontent.com/lindage1994/images/master/typora202009/26/194502-349315.gif)
图 3 带权的图存储结构


子图：指的是由图中一部分顶点和边构成的图，称为原图的子图。

## 图存储结构的分类

根据不同的特征，图又可分为完全图，连通图、稀疏图和稠密图：

- 完全图：若图中各个顶点都与除自身外的其他顶点有关系，这样的无向图称为完全图。同时，满足此条件的有向图则称为有向完全图。

  ![img](https://raw.githubusercontent.com/lindage1994/images/master/typora202009/26/194612-632761.gif)
图 4 完全图示意图
  
具有 n 个顶点的完全图，图中边的数量为 n(n-1)/2；而对于具有 n 个顶点的有向完全图，图中弧的数量为 n(n-1)。
  
- 稀疏图和稠密图：这两种图是相对存在的，即如果图中具有很少的边（或弧），此图就称为"稀疏图"；反之，则称此图为"稠密图"。

  稀疏和稠密的判断条件是：e<nlogn，其中 e 表示图中边（或弧）的数量，n 表示图中顶点的数量。如果式子成立，则为稀疏图；反之为稠密图。


有关连通图的相关知识，后续专门有一篇文章做详细介绍。

前面介绍了图存储结构，本节继续讲解什么是连通图。

前面讲过，图中从一个顶点到达另一顶点，若存在至少一条路径，则称这两个顶点是连通着的。例如图 1 中，虽然 V1 和 V3 没有直接关联，但从 V1 到 V3 存在两条路径，分别是 `V1-V2-V3` 和 `V1-V4-V3`，因此称 V1 和 V3 之间是连通的。

![顶点之间的连通状态示意图](https://raw.githubusercontent.com/lindage1994/images/master/typora202009/26/194649-752559.gif)
图 1 顶点之间的连通状态示意图


无向图中，如果任意两个顶点之间都能够连通，则称此无向图为连通图。例如，图 2 中的无向图就是一个连通图，因为此图中任意两顶点之间都是连通的。

![连通图示意图](https://raw.githubusercontent.com/lindage1994/images/master/typora202009/26/194710-970696.gif)
图 2 连通图示意图


若无向图不是连通图，但图中存储某个子图符合连通图的性质，则称该子图为连通分量。

前面讲过，由图中部分顶点和边构成的图为该图的一个子图，但这里的子图指的是图中"最大"的连通子图（也称"极大连通子图"）。

如图 3 所示，虽然图 3a) 中的无向图不是连通图，但可以将其分解为 3 个"最大子图"（图 3b)），它们都满足连通图的性质，因此都是连通分量。

![连通分量示意图](https://raw.githubusercontent.com/lindage1994/images/master/typora202009/26/194722-11251.gif)
图 3 连通分量示意图

提示，图 3a) 中的无向图只能分解为 3 部分各自连通的"最大子图"。

需要注意的是，连通分量的提出是以"整个无向图不是连通图"为前提的，因为如果无向图是连通图，则其无法分解出多个最大连通子图，因为图中所有的顶点之间都是连通的。

## 强连通图

有向图中，若任意两个顶点 Vi 和 Vj，满足从 Vi 到 Vj 以及从 Vj 到 Vi 都连通，也就是都含有至少一条通路，则称此有向图为强连通图。如图 4 所示就是一个强连通图。

![强连通图](https://raw.githubusercontent.com/lindage1994/images/master/typora202009/26/194735-615225.gif)
图 4 强连通图


与此同时，若有向图本身不是强连通图，但其包含的最大连通子图具有强连通图的性质，则称该子图为强连通分量。

![强连通分量](https://raw.githubusercontent.com/lindage1994/images/master/typora202009/26/194745-826872.gif)
图 5 强连通分量


如图 5 所示，整个有向图虽不是强连通图，但其含有两个强连通分量。

## 总结

可以这样说，连通图是在无向图的基础上对图中顶点之间的连通做了更高的要求，而强连通图是在有向图的基础上对图中顶点的连通做了更高的要求。

在学习连通图的基础上，本节学习什么是生成树，以及什么是生成森林。

对连通图进行遍历，过程中所经过的边和顶点的组合可看做是一棵普通树，通常称为生成树。

![连通图及其对应的生成树](https://raw.githubusercontent.com/lindage1994/images/master/typora202009/26/194825-402854.gif)
图 1 连通图及其对应的生成树


如图 1 所示，图 1a) 是一张连通图，图 1b) 是其对应的 2 种生成树。

连通图中，由于任意两顶点之间可能含有多条通路，遍历连通图的方式有多种，往往一张连通图可能有多种不同的生成树与之对应。

连通图中的生成树必须满足以下 2 个条件：

1. 包含连通图中所有的顶点；
2. 任意两顶点之间有且仅有一条通路；


因此，连通图的生成树具有这样的特征，即生成树中`边的数量 = 顶点数 - 1`。

## 生成森林

生成树是对应连通图来说，而生成森林是对应非连通图来说的。

我们知道，非连通图可分解为多个连通分量，而每个连通分量又各自对应多个生成树（至少是 1 棵），因此与整个非连通图相对应的，是由多棵生成树组成的生成森林。

![非连通图和连通分量](https://raw.githubusercontent.com/lindage1994/images/master/typora202009/26/194841-963170.gif)
图 2 非连通图和连通分量


如图 2 所示，这是一张非连通图，可分解为 3 个连通分量，其中各个连通分量对应的生成树如图 3 所示：

![生成森林](https://raw.githubusercontent.com/lindage1994/images/master/typora202009/26/194849-870653.gif)
图 3 生成森林

注意，图 3 中列出的仅是各个连通分量的其中一种生成树。

因此，多个连通分量对应的多棵生成树就构成了整个非连通图的生成森林。

使用图结构表示的数据元素之间虽然具有“多对多”的关系，但是同样可以采用顺序存储，也就是使用数组有效地存储图。

使用数组存储图时，需要使用两个数组，一个数组存放图中顶点本身的数据（一维数组），另外一个数组用于存储各顶点之间的关系（二维数组）。

存储图中各顶点本身数据，使用一维数组就足够了；存储顶点之间的关系时，要记录每个顶点和其它所有顶点之间的关系，所以需要使用二维数组。

不同类型的图，存储的方式略有不同，根据图有无权，可以将图划分为两大类：图和网 。

> 图，包括无向图和有向图；网，是指带权的图，包括无向网和有向网。存储方式的不同，指的是：在使用二维数组存储图中顶点之间的关系时，如果顶点之间存在边或弧，在相应位置用 1 表示，反之用 0 表示；如果使用二维数组存储网中顶点之间的关系，顶点之间如果有边或者弧的存在，在数组的相应位置存储其权值；反之用 0 表示。

结构代码表示：

```
#define MAX_VERtEX_NUM 20                   //顶点的最大个数#define VRType int                          //表示顶点之间的关系的变量类型#define InfoType char                       //存储弧或者边额外信息的指针变量类型#define VertexType int                      //图中顶点的数据类型typedef enum{DG,DN,UDG,UDN}GraphKind;       //枚举图的 4 种类型typedef struct {    VRType adj;                             //对于无权图，用 1 或 0 表示是否相邻；对于带权图，直接为权值。    InfoType * info;                        //弧或边额外含有的信息指针}ArcCell,AdjMatrix[MAX_VERtEX_NUM][MAX_VERtEX_NUM];typedef struct {    VertexType vexs[MAX_VERtEX_NUM];        //存储图中顶点数据    AdjMatrix arcs;                         //二维数组，记录顶点之间的关系    int vexnum,arcnum;                      //记录图的顶点数和弧（边）数    GraphKind kind;                         //记录图的种类}MGraph;
```

![img](https://raw.githubusercontent.com/lindage1994/images/master/typora202009/26/194918-20537.png)
图1 有向图和无向图


例如，存储图 1 中的无向图（B）时，除了存储图中各顶点本身具有的数据外，还需要使用二维数组存储任意两个顶点之间的关系。

由于 （B） 为无向图，各顶点没有权值，所以如果两顶点之间有关联，相应位置记为 1 ；反之记为 0 。构建的二维数组如图 2 所示。

![img](https://raw.githubusercontent.com/lindage1994/images/master/typora202009/26/194928-115795.png)
图2 无向图对应的二维数组arcs


在此二维数组中，每一行代表一个顶点，依次从 V1 到 V5 ，每一列也是如此。比如 arcs[0][1] = 1 ，表示 V1 和 V2 之间有边存在；而 arcs[0][2] = 0，说明 V1 和 V3 之间没有边。

对于无向图来说，二维数组构建的二阶矩阵，实际上是对称矩阵，在存储时就可以采用压缩存储的方式存储下三角或者上三角。

通过二阶矩阵，可以直观地判断出各个顶点的度，为该行（或该列）非 0 值的和。例如，第一行有两个 1，说明 V1 有两个边，所以度为 2。

存储图 1 中的有向图（A）时，对应的二维数组如图 3 所示：

![img](https://raw.githubusercontent.com/lindage1994/images/master/typora202009/26/194937-410954.png)
图 3 有向图对应的二维数组arcs


例如，arcs[0][1] = 1 ，证明从 V1 到 V2 有弧存在。且通过二阶矩阵，可以很轻松得知各顶点的出度和入度，出度为该行非 0 值的和，入度为该列非 0 值的和。例如，V1 的出度为第一行两个 1 的和，为 2 ； V1 的入度为第一列中 1 的和，为 1 。所以 V1 的出度为 2 ，入度为 1 ，度为两者的和 3 。

## 具体实现代码

```
#include <stdio.h>#define MAX_VERtEX_NUM 20                   //顶点的最大个数#define VRType int                          //表示顶点之间的关系的变量类型#define InfoType char                       //存储弧或者边额外信息的指针变量类型#define VertexType int                      //图中顶点的数据类型typedef enum{DG,DN,UDG,UDN}GraphKind;       //枚举图的 4 种类型typedef struct {    VRType adj;                             //对于无权图，用 1 或 0 表示是否相邻；对于带权图，直接为权值。    InfoType * info;                        //弧或边额外含有的信息指针}ArcCell,AdjMatrix[MAX_VERtEX_NUM][MAX_VERtEX_NUM];typedef struct {    VertexType vexs[MAX_VERtEX_NUM];        //存储图中顶点数据    AdjMatrix arcs;                         //二维数组，记录顶点之间的关系    int vexnum,arcnum;                      //记录图的顶点数和弧（边）数    GraphKind kind;                         //记录图的种类}MGraph;//根据顶点本身数据，判断出顶点在二维数组中的位置int LocateVex(MGraph * G,VertexType v){    int i=0;    //遍历一维数组，找到变量v    for (; i<G->vexnum; i++) {        if (G->vexs[i]==v) {            break;        }    }    //如果找不到，输出提示语句，返回-1    if (i>G->vexnum) {        printf("no such vertex.\n");        return -1;    }    return i;}//构造有向图void CreateDG(MGraph *G){    //输入图含有的顶点数和弧的个数    scanf("%d,%d",&(G->vexnum),&(G->arcnum));    //依次输入顶点本身的数据    for (int i=0; i<G->vexnum; i++) {        scanf("%d",&(G->vexs[i]));    }    //初始化二维矩阵，全部归0，指针指向NULL    for (int i=0; i<G->vexnum; i++) {        for (int j=0; j<G->vexnum; j++) {            G->arcs[i][j].adj=0;            G->arcs[i][j].info=NULL;        }    }    //在二维数组中添加弧的数据    for (int i=0; i<G->arcnum; i++) {        int v1,v2;        //输入弧头和弧尾        scanf("%d,%d",&v1,&v2);        //确定顶点位置        int n=LocateVex(G, v1);        int m=LocateVex(G, v2);        //排除错误数据        if (m==-1 ||n==-1) {            printf("no this vertex\n");            return;        }        //将正确的弧的数据加入二维数组        G->arcs[n][m].adj=1;    }}//构造无向图void CreateDN(MGraph *G){    scanf("%d,%d",&(G->vexnum),&(G->arcnum));    for (int i=0; i<G->vexnum; i++) {        scanf("%d",&(G->vexs[i]));    }    for (int i=0; i<G->vexnum; i++) {        for (int j=0; j<G->vexnum; j++) {            G->arcs[i][j].adj=0;            G->arcs[i][j].info=NULL;        }    }    for (int i=0; i<G->arcnum; i++) {        int v1,v2;        scanf("%d,%d",&v1,&v2);        int n=LocateVex(G, v1);        int m=LocateVex(G, v2);        if (m==-1 ||n==-1) {            printf("no this vertex\n");            return;        }        G->arcs[n][m].adj=1;        G->arcs[m][n].adj=1;//无向图的二阶矩阵沿主对角线对称    }}//构造有向网，和有向图不同的是二阶矩阵中存储的是权值。void CreateUDG(MGraph *G){    scanf("%d,%d",&(G->vexnum),&(G->arcnum));    for (int i=0; i<G->vexnum; i++) {        scanf("%d",&(G->vexs[i]));    }    for (int i=0; i<G->vexnum; i++) {        for (int j=0; j<G->vexnum; j++) {            G->arcs[i][j].adj=0;            G->arcs[i][j].info=NULL;        }    }    for (int i=0; i<G->arcnum; i++) {        int v1,v2,w;        scanf("%d,%d,%d",&v1,&v2,&w);        int n=LocateVex(G, v1);        int m=LocateVex(G, v2);        if (m==-1 ||n==-1) {            printf("no this vertex\n");            return;        }        G->arcs[n][m].adj=w;    }}//构造无向网。和无向图唯一的区别就是二阶矩阵中存储的是权值void CreateUDN(MGraph* G){    scanf("%d,%d",&(G->vexnum),&(G->arcnum));    for (int i=0; i<G->vexnum; i++) {        scanf("%d",&(G->vexs[i]));    }    for (int i=0; i<G->vexnum; i++) {        for (int j=0; j<G->vexnum; j++) {            G->arcs[i][j].adj=0;            G->arcs[i][j].info=NULL;        }    }    for (int i=0; i<G->arcnum; i++) {        int v1,v2,w;        scanf("%d,%d,%d",&v1,&v2,&w);        int m=LocateVex(G, v1);        int n=LocateVex(G, v2);        if (m==-1 ||n==-1) {            printf("no this vertex\n");            return;        }        G->arcs[n][m].adj=w;        G->arcs[m][n].adj=w;//矩阵对称    }}void CreateGraph(MGraph *G){    //选择图的类型    scanf("%d",&(G->kind));    //根据所选类型，调用不同的函数实现构造图的功能    switch (G->kind) {        case DG:            return CreateDG(G);            break;        case DN:            return CreateDN(G);            break;        case UDG:            return CreateUDG(G);            break;        case UDN:            return CreateUDN(G);            break;        default:            break;    }}//输出函数void PrintGrapth(MGraph G){    for (int i = 0; i < G.vexnum; i++)    {        for (int j = 0; j < G.vexnum; j++)        {            printf("%d ", G.arcs[i][j].adj);        }        printf("\n");    }}int main() {    MGraph G;//建立一个图的变量    CreateGraph(&G);//调用创建函数，传入地址参数    PrintGrapth(G);//输出图的二阶矩阵    return 0;}
```

> 注意：在此程序中，构建无向网和有向网时，对于之间没有边或弧的顶点，相应的二阶矩阵中存放的是 0。目的只是为了方便查看运行结果，而实际上如果顶点之间没有关联，它们之间的距离应该是无穷大（∞）。


例如，使用上述程序存储图 4（a）的有向网时，存储的两个数组如图 4（b）所示：

![img](https://raw.githubusercontent.com/lindage1994/images/master/typora202009/26/194946-867574.png)
图 4 有向网


相应地运行结果为：

2
6,10
1
2
3
4
5
6
1,2,5
2,3,4
3,1,8
1,4,7
4,3,5
3,6,9
6,1,3
4,6,6
6,5,1
5,4,5
0 5 0 7 0 0
0 0 4 0 0 0
8 0 0 0 0 9
0 0 5 0 0 6
0 0 0 5 0 0
3 0 0 0 1 0

## 总结

本节主要详细介绍了使用数组存储图的方法，在实际操作中使用更多的是链式存储结构，例如邻接表、十字链表和邻接多重表，这三种存储图的方式放在下一节重点去讲。通常，图更多的是采用链表

存储，具体的存储方法有 3 种，分别是邻接表、邻接多重表和十字链表。

本节先讲解图的邻接表存储法。邻接表既适用于存储无向图，也适用于存储有向图。

在具体讲解邻接表存储图的实现方法之前，先普及一个"邻接点"的概念。在图中，如果两个点相互连通，即通过其中一个顶点，可直接找到另一个顶点，则称它们互为邻接点。

邻接指的是图中顶点之间有边或者弧的存在。

邻接表存储图的实现方式是，给图中的各个顶点独自建立一个链表，用节点存储该顶点，用链表中其他节点存储各自的临界点。

与此同时，为了便于管理这些链表，通常会将所有链表的头节点存储到数组

中（也可以用链表存储）。也正因为各个链表的头节点存储的是各个顶点，因此各链表在存储临界点数据时，仅需存储该邻接顶点位于数组中的位置下标即可。

例如，存储图 1a) 所示的有向图，其对应的邻接表如图 1b) 所示：

![邻接表存储有向图](https://raw.githubusercontent.com/lindage1994/images/master/typora202009/26/195055-302007.gif)
图 1 邻接表存储有向图


拿顶点 V1 来说，与其相关的邻接点分别为 V2 和 V3，因此存储 V1 的链表中存储的是 V2 和 V3 在数组中的位置下标 1 和 2。

从图 1 中可以看出，存储各顶点的节点结构分为两部分，数据域和指针域。数据域用于存储顶点数据信息，指针域用于链接下一个节点，如图 2 所示：

![邻接表节点结构](https://raw.githubusercontent.com/lindage1994/images/master/typora202009/26/195104-506934.gif)
图 2 邻接表节点结构


在实际应用中，除了图 2 这种节点结构外，对于用链接表存储网（边或弧存在权）结构，还需要节点存储权的值，因此需使用图 3 中的节点结构：

![邻接表存储网结构使用的节点](https://raw.githubusercontent.com/lindage1994/images/master/typora202009/26/195112-7033.gif)
图 3 邻接表存储网结构使用的节点


图 1 中的链接表结构转化为对应 C 语言代码如下：

```
#define  MAX_VERTEX_NUM 20//最大顶点个数#define  VertexType int//顶点数据的类型#define  InfoType int//图中弧或者边包含的信息的类型typedef struct ArcNode{    int adjvex;//邻接点在数组中的位置下标    struct ArcNode * nextarc;//指向下一个邻接点的指针    InfoType * info;//信息域}ArcNode;typedef struct VNode{    VertexType data;//顶点的数据域    ArcNode * firstarc;//指向邻接点的指针}VNode,AdjList[MAX_VERTEX_NUM];//存储各链表头结点的数组typedef struct {    AdjList vertices;//图中顶点的数组    int vexnum,arcnum;//记录图中顶点数和边或弧数    int kind;//记录图的种类}ALGraph;
```

## 邻接表计算顶点的出度和入度

使用邻接表计算无向图中顶点的入度和出度会非常简单，只需从数组中找到该顶点然后统计此链表中节点的数量即可。

而使用邻接表存储有向图时，通常各个顶点的链表中存储的都是以该顶点为弧尾的邻接点，因此通过统计各顶点链表中的节点数量，只能计算出该顶点的出度，而无法计算该顶点的入度。

对于利用邻接表求某顶点的入度，有两种方式：

1. 遍历整个邻接表中的节点，统计数据域与该顶点所在数组位置下标相同的节点数量，即为该顶点的入度；
2. 建立一个逆邻接表，该表中的各顶点链表专门用于存储以此顶点为弧头的所有顶点在数组中的位置下标。比如说，建立一张图 1a) 对应的逆邻接表，如图 4 所示：

![逆邻接表示意图](https://raw.githubusercontent.com/lindage1994/images/master/typora202009/26/195121-832033.gif)
图 4 逆邻接表示意图


对于具有 n 个顶点和 e 条边的无向图，邻接表中需要存储 n 个头结点和 2e 个表结点。在图中边或者弧稀疏的时候，使用邻接表要比前一节介绍的邻接矩阵更加节省空间。

前面介绍了图的邻接表存储法，本节继续讲解图的另一种链式存储结构——十字链表法。

与邻接表不同，十字链表法仅适用于存储有向图和有向网。不仅如此，十字链表法还改善了邻接表计算图中顶点入度的问题。

十字链表存储有向图（网）的方式与邻接表有一些相同，都以图（网）中各顶点为首元节点建立多条链表，同时为了便于管理，还将所有链表的首元节点存储到同一数组（或链表）中。

其中，建立个各个链表中用于存储顶点的首元节点结构如图 1 所示：

![十字链表中首元节点结构示意图](https://raw.githubusercontent.com/lindage1994/images/master/typora202009/26/195233-754985.gif)
图 1 十字链表中首元节点结构示意图


从图 1 可以看出，首元节点中有一个数据域和两个指针域（分别用 firstin 和 firstout 表示）：

- firstin 指针用于连接以当前顶点为弧头的其他顶点构成的链表；
- firstout 指针用于连接以当前顶点为弧尾的其他顶点构成的链表；
- data 用于存储该顶点中的数据；

由此可以看出，十字链表实质上就是为每个顶点建立两个链表，分别存储以该顶点为弧头的所有顶点和以该顶点为弧尾的所有顶点。


注意，存储图的十字链表中，各链表中首元节点与其他节点的结构并不相同，图 1 所示仅是十字链表中首元节点的结构，链表中其他普通节点的结构如图 2 所示：

![十字链表中普通节点的结构示意图](https://raw.githubusercontent.com/lindage1994/images/master/typora202009/26/195246-839826.gif)
图 2 十字链表中普通节点的结构示意图


从图 2 中可以看出，十字链表中普通节点的存储分为 5 部分内容，它们各自的作用是：

- tailvex 用于存储以首元节点为弧尾的顶点位于数组中的位置下标；
- headvex 用于存储以首元节点为弧头的顶点位于数组中的位置下标；
- hlink 指针：用于链接下一个存储以首元节点为弧头的顶点的节点；
- tlink 指针：用于链接下一个存储以首元节点为弧尾的顶点的节点；
- info 指针：用于存储与该顶点相关的信息，例如量顶点之间的权值；


比如说，用十字链表存储图 3a) 中的有向图，存储状态如图 3b) 所示：

![十字链表存储有向图示意图](https://raw.githubusercontent.com/lindage1994/images/master/typora202009/26/195546-353421.gif)
图 3 十字链表存储有向图示意图


拿图 3 中的顶点 V1 来说，通过构建好的十字链表得知，以该顶点为弧头的顶点只有存储在数组中第 3 位置的 V4（因此该顶点的入度为 1），而以该顶点为弧尾的顶点有两个，分别为存储数组第 1 位置的 V2 和第 2 位置的 V3（因此该顶点的出度为 2）。

对于图 3 各个链表中节点来说，由于表示的都是该顶点的出度或者入度，因此没有先后次序之分。

图 3 中十字链表的构建过程转化为 C 语言代码为：

```
纯文本复制
#define  MAX_VERTEX_NUM 20#define  InfoType int//图中弧包含信息的数据类型#define  VertexType inttypedef struct ArcBox{    int tailvex,headvex;//弧尾、弧头对应顶点在数组中的位置下标    struct ArcBox *hlik,*tlink;//分别指向弧头相同和弧尾相同的下一个弧    InfoType *info;//存储弧相关信息的指针}ArcBox;typedef struct VexNode{    VertexType data;//顶点的数据域    ArcBox *firstin,*firstout;//指向以该顶点为弧头和弧尾的链表首个结点}VexNode;typedef struct {    VexNode xlist[MAX_VERTEX_NUM];//存储顶点的一维数组    int vexnum,arcnum;//记录图的顶点数和弧数}OLGraph;int LocateVex(OLGraph * G,VertexType v){    int i=0;    //遍历一维数组，找到变量v    for (; i<G->vexnum; i++) {        if (G->xlist[i].data==v) {            break;        }    }    //如果找不到，输出提示语句，返回 -1    if (i>G->vexnum) {        printf("no such vertex.\n");        return -1;    }    return i;}//构建十字链表函数void CreateDG(OLGraph *G){    //输入有向图的顶点数和弧数    scanf("%d,%d",&(G->vexnum),&(G->arcnum));    //使用一维数组存储顶点数据，初始化指针域为NULL    for (int i=0; i<G->vexnum; i++) {        scanf("%d",&(G->xlist[i].data));        G->xlist[i].firstin=NULL;        G->xlist[i].firstout=NULL;    }    //构建十字链表    for (int k=0;k<G->arcnum; k++) {        int v1,v2;        scanf("%d,%d",&v1,&v2);        //确定v1、v2在数组中的位置下标        int i=LocateVex(G, v1);        int j=LocateVex(G, v2);        //建立弧的结点        ArcBox * p=(ArcBox*)malloc(sizeof(ArcBox));        p->tailvex=i;        p->headvex=j;        //采用头插法插入新的p结点        p->hlik=G->xlist[j].firstin;        p->tlink=G->xlist[i].firstout;        G->xlist[j].firstin=G->xlist[i].firstout=p;    }}
```

提示，代码中新节点的插入采用的是头插法。前面讲过，无向图的存储可以使用邻接表，但在实际使用时，如果想对图中某顶点进行实操（修改或删除），由于邻接表中存储该顶点的节点有两个，因此需要操作两个节点。为了提高在无向图中操作顶点的效率，本节学习一种新的适用于存储无向图的方法——邻接多重表。注意，邻接多重表仅适用于存储无向图或无向网。邻接多重表存储无向图的方式，可看作是邻接表和十字链表的结合。同邻接表和十字链表存储图的方法相同，都是独自为图中各顶点建立一张链表，存储各顶点的节点作为各链表的首元节点，同时为了便于管理将各个首元节点存储到一个数组中。各首元节点结构如图 1 所示：

![邻接多重表各首元节点的结构示意图](https://raw.githubusercontent.com/lindage1994/images/master/typora202009/26/195701-499979.gif)
图 1 邻接多重表各首元节点的结构示意图


图 1 中各区域及其功能为：

- data：存储此顶点的数据；
- firstedge：指针域，用于指向同该顶点有直接关联的存储其他顶点的节点。


从图 1 可以看到，邻接多重表采用与邻接表相同的首元节点结构。但各链表中其他节点的结构与十字链表中相同，如图 2 所示：

![邻接多重表中其他节点结构](https://raw.githubusercontent.com/lindage1994/images/master/typora202009/26/195739-960081.gif)
图 2 邻接多重表中其他节点结构


图 2 节点中各区域及功能如下：

- mark：标志域，用于标记此节点是否被操作过，例如在对图中顶点做遍历操作时，为了防止多次操作同一节点，mark 域为 0 表示还未被遍历；mark 为 1 表示该节点已被遍历；
- ivex 和 jvex：数据域，分别存储图中各边两端的顶点所在数组中的位置下标；
- ilink：指针域，指向下一个存储与 ivex 有直接关联顶点的节点；
- jlink：指针域，指向下一个存储与 jvex 有直接关联顶点的节点；
- info：指针域，用于存储与该顶点有关的其他信息，比如无向网中各边的权；


综合以上信息，如果我们想使用邻接多重表存储图 3a) 中的无向图，则与之对应的邻接多重表如图 3b) 所示：

![无向图及其对应的邻接多重表](https://raw.githubusercontent.com/lindage1994/images/master/typora202009/26/195747-646978.gif)
图 3 无向图及其对应的邻接多重表


从图 3 中，可直接找到与各顶点有直接关联的其他顶点。比如说，与顶点 V1 有关联的顶点为存储在数组下标 1 处的 V2 和数组下标 3 处的 V4，而与顶点 V2 有关联的顶点有 3 个，分别是 V1、V3 和 V5。

图 3 中邻接多重表的整体结构转化为 C 语言代码如下所示：

```
纯文本复制
#define MAX_VERTEX_NUM 20                   //图中顶点的最大个数#define InfoType int                        //边含有的信息域的数据类型#define VertexType int                      //图顶点的数据类型typedef enum {unvisited,visited}VisitIf;    //边标志域typedef struct EBox{    VisitIf mark;                           //标志域    int ivex,jvex;                          //边两边顶点在数组中的位置下标    struct EBox * ilink,*jlink;             //分别指向与ivex、jvex相关的下一个边    InfoType *info;                         //边包含的其它的信息域的指针}EBox;typedef struct VexBox{    VertexType data;                        //顶点数据域    EBox * firstedge;                       //顶点相关的第一条边的指针域}VexBox;typedef struct {    VexBox adjmulist[MAX_VERTEX_NUM];//存储图中顶点的数组    int vexnum,degenum;//记录途中顶点个数和边个数的变量}AMLGraph;
```

前边介绍了有关图的 4 种存储方式，本节介绍如何对存储的图中的顶点进行遍历。常用的遍历方式有两种：深度优先搜索和广度优先搜索。

## 深度优先搜索（简称“深搜”或DFS）

![img](https://raw.githubusercontent.com/lindage1994/images/master/typora202009/26/195809-357694.png)
图 1 无向图


深度优先搜索的过程类似于树的先序遍历，首先从例子中体会深度优先搜索。例如图 1 是一个无向图，采用深度优先算法遍历这个图的过程为：

1. 首先任意找一个未被遍历过的顶点，例如从 V1 开始，由于 V1 率先访问过了，所以，需要标记 V1 的状态为访问过；
2. 然后遍历 V1 的邻接点，例如访问 V2 ，并做标记，然后访问 V2 的邻接点，例如 V4 （做标记），然后 V8 ，然后 V5 ；
3. 当继续遍历 V5 的邻接点时，根据之前做的标记显示，所有邻接点都被访问过了。此时，从 V5 回退到 V8 ，看 V8 是否有未被访问过的邻接点，如果没有，继续回退到 V4 ， V2 ， V1 ；
4. 通过查看 V1 ，找到一个未被访问过的顶点 V3 ，继续遍历，然后访问 V3 邻接点 V6 ，然后 V7 ；
5. 由于 V7 没有未被访问的邻接点，所有回退到 V6 ，继续回退至 V3 ，最后到达 V1 ，发现没有未被访问的；
6. 最后一步需要判断是否所有顶点都被访问，如果还有没被访问的，以未被访问的顶点为第一个顶点，继续依照上边的方式进行遍历。


根据上边的过程，可以得到图 1 通过深度优先搜索获得的顶点的遍历次序为：

V1 -> V2 -> V4 -> V8 -> V5 -> V3 -> V6 -> V7


所谓深度优先搜索，是从图中的一个顶点出发，每次遍历当前访问顶点的临界点，一直到访问的顶点没有未被访问过的临界点为止。然后采用依次回退的方式，查看来的路上每一个顶点是否有其它未被访问的临界点。访问完成后，判断图中的顶点是否已经全部遍历完成，如果没有，以未访问的顶点为起始点，重复上述过程。

> 深度优先搜索是一个不断回溯的过程。


采用深度优先搜索算法遍历图的实现代码为：

```
#include <stdio.h>#define MAX_VERtEX_NUM 20                   //顶点的最大个数#define VRType int                          //表示顶点之间的关系的变量类型#define InfoType char                       //存储弧或者边额外信息的指针变量类型#define VertexType int                      //图中顶点的数据类型typedef enum{false,true}bool;               //定义bool型常量bool visited[MAX_VERtEX_NUM];               //设置全局数组，记录标记顶点是否被访问过typedef struct {    VRType adj;                             //对于无权图，用 1 或 0 表示是否相邻；对于带权图，直接为权值。    InfoType * info;                        //弧或边额外含有的信息指针}ArcCell,AdjMatrix[MAX_VERtEX_NUM][MAX_VERtEX_NUM];typedef struct {    VertexType vexs[MAX_VERtEX_NUM];        //存储图中顶点数据    AdjMatrix arcs;                         //二维数组，记录顶点之间的关系    int vexnum,arcnum;                      //记录图的顶点数和弧（边）数}MGraph;//根据顶点本身数据，判断出顶点在二维数组中的位置int LocateVex(MGraph * G,VertexType v){    int i=0;    //遍历一维数组，找到变量v    for (; i<G->vexnum; i++) {        if (G->vexs[i]==v) {            break;        }    }    //如果找不到，输出提示语句，返回-1    if (i>G->vexnum) {        printf("no such vertex.\n");        return -1;    }    return i;}//构造无向图void CreateDN(MGraph *G){    scanf("%d,%d",&(G->vexnum),&(G->arcnum));    for (int i=0; i<G->vexnum; i++) {        scanf("%d",&(G->vexs[i]));    }    for (int i=0; i<G->vexnum; i++) {        for (int j=0; j<G->vexnum; j++) {            G->arcs[i][j].adj=0;            G->arcs[i][j].info=NULL;        }    }    for (int i=0; i<G->arcnum; i++) {        int v1,v2;        scanf("%d,%d",&v1,&v2);        int n=LocateVex(G, v1);        int m=LocateVex(G, v2);        if (m==-1 ||n==-1) {            printf("no this vertex\n");            return;        }        G->arcs[n][m].adj=1;        G->arcs[m][n].adj=1;//无向图的二阶矩阵沿主对角线对称    }}int FirstAdjVex(MGraph G,int v){    //查找与数组下标为v的顶点之间有边的顶点，返回它在数组中的下标    for(int i = 0; i<G.vexnum; i++){        if( G.arcs[v][i].adj ){            return i;        }    }    return -1;}int NextAdjVex(MGraph G,int v,int w){    //从前一个访问位置w的下一个位置开始，查找之间有边的顶点    for(int i = w+1; i<G.vexnum; i++){        if(G.arcs[v][i].adj){            return i;        }    }    return -1;}void visitVex(MGraph G, int v){    printf("%d ",G.vexs[v]);}void DFS(MGraph G,int v){    visited[v] = true;//标记为true    visitVex( G,  v); //访问第v 个顶点    //从该顶点的第一个边开始，一直到最后一个边，对处于边另一端的顶点调用DFS函数    for(int w = FirstAdjVex(G,v); w>=0; w = NextAdjVex(G,v,w)){        //如果该顶点的标记位false，证明未被访问，调用深度优先搜索函数        if(!visited[w]){            DFS(G,w);        }    }}//深度优先搜索void DFSTraverse(MGraph G){//    int v;    //将用做标记的visit数组初始化为false    for( v = 0; v < G.vexnum; ++v){        visited[v] = false;    }    //对于每个标记为false的顶点调用深度优先搜索函数    for( v = 0; v < G.vexnum; v++){        //如果该顶点的标记位为false，则调用深度优先搜索函数        if(!visited[v]){            DFS( G, v);        }    }}int main() {    MGraph G;//建立一个图的变量    CreateDN(&G);//初始化图    DFSTraverse(G);//深度优先搜索图    return 0;}
```

以图 1 为例，运行结果为：

8,9
1
2
3
4
5
6
7
8
1,2
2,4
2,5
4,8
5,8
1,3
3,6
6,7
7,3
1 2 4 8 5 3 6 7

## 广度优先搜索

广度优先搜索类似于树的层次遍历。从图中的某一顶点出发，遍历每一个顶点时，依次遍历其所有的邻接点，然后再从这些邻接点出发，同样依次访问它们的邻接点。按照此过程，直到图中所有被访问过的顶点的邻接点都被访问到。

最后还需要做的操作就是查看图中是否存在尚未被访问的顶点，若有，则以该顶点为起始点，重复上述遍历的过程。

还拿图 1 中的无向图为例，假设 V1 作为起始点，遍历其所有的邻接点 V2 和 V3 ，以 V2 为起始点，访问邻接点 V4 和 V5 ，以 V3 为起始点，访问邻接点 V6 、 V7 ，以 V4 为起始点访问 V8 ，以 V5 为起始点，由于 V5 所有的起始点已经全部被访问，所有直接略过， V6 和 V7 也是如此。
以 V1 为起始点的遍历过程结束后，判断图中是否还有未被访问的点，由于图 1 中没有了，所以整个图遍历结束。遍历顶点的顺序为：

V1 -> V2 -> v3 -> V4 -> V5 -> V6 -> V7 -> V8


广度优先搜索的实现需要借助**队列**这一特殊数据结构，实现代码为：

```c
#include <stdio.h>#include <stdlib.h>
#define MAX_VERtEX_NUM 20                   //顶点的最大个数
#define VRType int                          //表示顶点之间的关系的变量类型
#define InfoType char                       //存储弧或者边额外信息的指针变量类型
#define VertexType int                      //图中顶点的数据类型
typedef enum{false,true}bool;               //定义bool型常量
bool visited[MAX_VERtEX_NUM];               //设置全局数组，记录标记顶点是否被访问过
typedef struct Queue{   
    VertexType data;   
    struct Queue * next;
}Queue;
typedef struct {  
    VRType adj;                             //对于无权图，用 1 或 0 表示是否相邻；对于带权图，直接为权值。 
    InfoType * info;                        //弧或边额外含有的信息指针
}ArcCell,AdjMatrix[MAX_VERtEX_NUM][MAX_VERtEX_NUM];
typedef struct {   
    VertexType vexs[MAX_VERtEX_NUM];        //存储图中顶点数据   
    AdjMatrix arcs;                         //二维数组，记录顶点之间的关系   
    int vexnum,arcnum;                      //记录图的顶点数和弧（边）数
}MGraph;
//根据顶点本身数据，判断出顶点在二维数组中的位置
int LocateVex(MGraph * G,VertexType v){   
    int i=0;    
    //遍历一维数组，找到变量v  
    for (; i<G->vexnum; i++) {      
        if (G->vexs[i]==v) {  
            break;      
        }   
    }   
    //如果找不到，输出提示语句，返回-1  
    if (i>G->vexnum) {   
        printf("no such vertex.\n");    
        return -1;   
    }   
    return i;
}
//构造无向图
void CreateDN(MGraph *G){  
    scanf("%d,%d",&(G->vexnum),&(G->arcnum));   
    for (int i=0; i<G->vexnum; i++) {  
        scanf("%d",&(G->vexs[i]));  
    }  
    for (int i=0; i<G->vexnum; i++) {  
        for (int j=0; j<G->vexnum; j++) {   
            G->arcs[i][j].adj=0;    
            G->arcs[i][j].info=NULL;  
        }  
    }   
    for (int i=0; i<G->arcnum; i++) {     
        int v1,v2;  
        scanf("%d,%d",&v1,&v2);  
        int n=LocateVex(G, v1);  
        int m=LocateVex(G, v2);     
        if (m==-1 ||n==-1) {      
            printf("no this vertex\n"); 
            return;  
        }     
        G->arcs[n][m].adj=1;    
        G->arcs[m][n].adj=1;//无向图的二阶矩阵沿主对角线对称   
    }
}

int FirstAdjVex(MGraph G,int v){    //查找与数组下标为v的顶点之间有边的顶点，返回它在数组中的下标  
    for(int i = 0; i<G.vexnum; i++){    
        if( G.arcs[v][i].adj ){    
            return i;     
        }   
    }   
    return -1;
}

int NextAdjVex(MGraph G,int v,int w){  
    //从前一个访问位置w的下一个位置开始，查找之间有边的顶点  
    for(int i = w+1; i<G.vexnum; i++){      
        if(G.arcs[v][i].adj){    
            return i;    
        }    
    }   
    return -1;
}

//操作顶点的函数
void visitVex(MGraph G, int v){  
    printf("%d ",G.vexs[v]);
}

//初始化队列
void InitQueue(Queue ** Q){ 
    (*Q)=(Queue*)malloc(sizeof(Queue));   
    (*Q)->next=NULL;
}

//顶点元素v进队列
void EnQueue(Queue **Q,VertexType v){ 
    Queue * element=(Queue*)malloc(sizeof(Queue));  
    element->data=v; 
    Queue * temp=(*Q);  
    while (temp->next!=NULL) {   
        temp=temp->next;  
    }   
    temp->next=element;
}

//队头元素出队列
void DeQueue(Queue **Q,int *u){  
    (*u)=(*Q)->next->data;  
    (*Q)->next=(*Q)->next->next;
}

//判断队列是否为空
bool QueueEmpty(Queue *Q){  
    if (Q->next==NULL) {   
        return true;   
    }   
    return false;
}

//广度优先搜索
void BFSTraverse(MGraph G){//   
    int v;    //将用做标记的visit数组初始化为false  
    for( v = 0; v < G.vexnum; ++v){     
        visited[v] = false; 
    }   
    //对于每个标记为false的顶点调用深度优先搜索函数  
    Queue * Q;  
    InitQueue(&Q); 
    for( v = 0; v < G.vexnum; v++){  
        if(!visited[v]){      
            visited[v]=true;       
            visitVex(G, v);       
            EnQueue(&Q, G.vexs[v]);     
            while (!QueueEmpty(Q)) {      
                int u;        
                DeQueue(&Q, &u);   
                u=LocateVex(&G, u);  
                for (int w=FirstAdjVex(G, u); w>=0; w=NextAdjVex(G, u, w)) {  
                    if (!visited[w]) {     
                        visited[w]=true;       
                        visitVex(G, w);          
                        EnQueue(&Q, G.vexs[w]);    
                    }            
                }      
            }     
        }   
    }
}

int main() {  
    MGraph G;//建立一个图的变量 
    CreateDN(&G);//初始化图  
    BFSTraverse(G);//广度优先搜索图 
    return 0;
}
```


例如，使用上述程序代码遍历图 1 中的无向图，运行结果为：

8,9
1
2
3
4
5
6
7
8
1,2
2,4
2,5
4,8
5,8
1,3
3,6
6,7
7,3
1 2 3 4 5 6 7 8

## 总结

本节介绍了两种遍历图的方式：深度优先搜索算法和广度优先搜索算法。深度优先搜索算法的实现运用的主要是回溯法，类似于树的先序遍历算法。广度优先搜索算法借助队列的先进先出的特点，类似于树的层次遍历。

#### 推荐阅读

| [图的深度优先搜索和广度优先搜索](https://www.cnblogs.com/skywang12345/p/3711483.html) | 图文并茂的介绍了两种搜索算法的具体实现过程            |
| ------------------------------------------------------------ | ----------------------------------------------------- |
| [深度优先搜索（DNS）和广度优先搜索（BFS）](https://www.jianshu.com/p/bff70b786bb6) | 详细介绍了两种搜索算法的实现过程                      |
| [图的遍历算法（深度优先算法DFS和广度优先算法BFS）](https://www.cnblogs.com/kubixuesheng/p/4399705.html) | 详细介绍了两种算法的实现过程，并配备的 C++ 的实现代码 |
| [广度优先搜索（BFS）和深度优先搜索（DFS）的应用实例 ](https://www.cnblogs.com/idreamo/p/8742617.html) | 从实例出发介绍两种搜索算法                            |

本章的第一节中，介绍了有关生成树和生成森林的有关知识，本节来解决对于给定的无向图，如何构建它们相对应的生成树或者生成森林。

其实在对无向图进行遍历的时候，遍历过程中所经历过的图中的顶点和边的组合，就是图的生成树或者生成森林。

![img](https://raw.githubusercontent.com/lindage1994/images/master/typora202009/26/200620-629727.png)
图 1 无向图

例如，图 1 中的无向图是由 V1～V7 的顶点和编号分别为 a～i 的边组成。当使用深度优先搜索算法时，假设 V1 作为遍历的起始点，涉及到的顶点和边的遍历顺序为（不唯一）：
![img](https://raw.githubusercontent.com/lindage1994/images/master/typora202009/26/200644-1349.png)

此种遍历顺序构建的生成树为：

![img](https://raw.githubusercontent.com/lindage1994/images/master/typora202009/26/200643-309081.png)

图 2 深度优先生成树


由深度优先搜索得到的树为深度优先生成树。同理，广度优先搜索生成的树为广度优先生成树，图 1 无向图以顶点 V1 为起始点进行广度优先搜索遍历得到的树，如图 3 所示：

![img](https://raw.githubusercontent.com/lindage1994/images/master/typora202009/26/200700-538840.png)
图 3 广度优先生成树

## 非连通图的生成森林

非连通图在进行遍历时，实则是对非连通图中每个连通分量分别进行遍历，在遍历过程经过的每个顶点和边，就构成了每个连通分量的生成树。

非连通图中，多个连通分量构成的多个生成树为非连通图的生成森林。

## 深度优先生成森林

![img](https://raw.githubusercontent.com/lindage1994/images/master/typora202009/26/200713-513419.png)
图 4 深度优先生成森林


例如，对图 4 中的非连通图 （a） 采用深度优先搜索算法遍历时，得到的深度优先生成森林（由 3 个深度优先生成树构成）如 （b） 所示（不唯一）。

> 非连通图在遍历生成森林时，可以采用孩子兄弟表示法将森林转化为一整棵二叉树进行存储。


具体实现的代码：

```c
#include <stdio.h>
#include <stdlib.h>
#define MAX_VERtEX_NUM 20                   //顶点的最大个数
#define VRType int                          //表示顶点之间的关系的变量类型
#define VertexType int                     //图中顶点的数据类型
typedef enum{false,true}bool;               //定义bool型常量
bool visited[MAX_VERtEX_NUM];               //设置全局数组，记录标记顶点是否被访问过
typedef struct {   
    VRType adj;                             //对于无权图，用 1 或 0 表示是否相邻；对于带权图，直接为权值。
}ArcCell,AdjMatrix[MAX_VERtEX_NUM][MAX_VERtEX_NUM];
typedef struct {   
    VertexType vexs[MAX_VERtEX_NUM];        //存储图中顶点数据   
    AdjMatrix arcs;                         //二维数组，记录顶点之间的关系  
    int vexnum,arcnum;                      //记录图的顶点数和弧（边）数
}MGraph;

//孩子兄弟表示法的链表结点结构
typedef struct CSNode{  
    VertexType data; 
    struct CSNode * lchild;//孩子结点  
    struct CSNode * nextsibling;//兄弟结点
}*CSTree,CSNode;

//根据顶点本身数据，判断出顶点在二维数组中的位置
int LocateVex(MGraph G,VertexType v){  
    int i=0;    
    //遍历一维数组，找到变量v   
    for (; i<G.vexnum; i++) {  
        if (G.vexs[i]==v) {   
            break;  
        } 
    }    
    //如果找不到，输出提示语句，返回-1    
    if (i>G.vexnum) {    
        printf("no such vertex.\n");    
        return -1;   
    } 
    return i;
}

//构造无向图
void CreateDN(MGraph *G){  
    scanf("%d,%d",&(G->vexnum),&(G->arcnum));  
    getchar(); 
    for (int i=0; i<G->vexnum; i++) {   
        scanf("%d",&(G->vexs[i]));  
    }  
    for (int i=0; i<G->vexnum; i++) {   
        for (int j=0; j<G->vexnum; j++) {    
            G->arcs[i][j].adj=0;     
        }   
    }  
    for (int i=0; i<G->arcnum; i++) {   
        int v1,v2;    
        scanf("%d,%d",&v1,&v2);    
        int n=LocateVex(*G, v1);  
        int m=LocateVex(*G, v2);   
        if (m==-1 ||n==-1) {      
            printf("no this vertex\n");      
            return;    
        }      
        G->arcs[n][m].adj=1;     
        G->arcs[m][n].adj=1;//无向图的二阶矩阵沿主对角线对称   
    }
}

int FirstAdjVex(MGraph G,int v){  
    //查找与数组下标为v的顶点之间有边的顶点，返回它在数组中的下标  
    for(int i = 0; i<G.vexnum; i++){    
        if( G.arcs[v][i].adj ){     
            return i;      
        }  
    }  
    return -1;
}

int NextAdjVex(MGraph G,int v,int w){   
    //从前一个访问位置w的下一个位置开始，查找之间有边的顶点  
    for(int i = w+1; i<G.vexnum; i++){    
        if(G.arcs[v][i].adj){       
            return i;    
        }  
    }  
    return -1;
}

void DFSTree(MGraph G,int v,CSTree*T){  
    //将正在访问的该顶点的标志位设为true 
    visited[v]=true;   
    bool first=true;   
    CSTree q=NULL;    
    //依次遍历该顶点的所有邻接点   
    for (int w=FirstAdjVex(G, v); w>=0; w=NextAdjVex(G, v, w)) {
        //如果该临界点标志位为false，说明还未访问   
        if (!visited[w]) {     
            //为该邻接点初始化为结点       
            CSTree p=(CSTree)malloc(sizeof(CSNode));  
            p->data=G.vexs[w];       
            p->lchild=NULL;     
            p->nextsibling=NULL;     
            //该结点的第一个邻接点作为孩子结点，其它邻接点作为孩子结点的兄弟结点   
            if (first) {         
                (*T)->lchild=p;    
                first=false;       
            }         
            //否则，为兄弟结点      
            else{        
                q->nextsibling=p;      
            }          
            q=p;        
            //以当前访问的顶点为树根，继续访问其邻接点       
            DFSTree(G, w, &q);    
        }   
    }
}

//深度优先搜索生成森林并转化为二叉树
void DFSForest(MGraph G,CSTree *T){ 
    (*T)=NULL;    //每个顶点的标记为初始化为false 
    for (int v=0; v<G.vexnum; v++) {   
        visited[v]=false;  
    }  
    CSTree q=NULL;   
    //遍历每个顶点作为初始点，建立深度优先生成树   
    for (int v=0; v<G.vexnum; v++) { 
        //如果该顶点的标记位为false，证明未访问过   
        if (!(visited[v])) {      
            //新建一个结点，表示该顶点     
            CSTree p=(CSTree)malloc(sizeof(CSNode));     
            p->data=G.vexs[v];         
            p->lchild=NULL;        
            p->nextsibling=NULL;        
            //如果树未空，则该顶点作为树的树根    
            if (!(*T)) {           
                (*T)=p;        
            }        
            //该顶点作为树根的兄弟结点     
            else{           
                q->nextsibling=p;  
            }        
            //每次都要把q指针指向新的结点，为下次添加结点做铺垫    
            q=p;       
            //以该结点为起始点，构建深度优先生成树     
            DFSTree(G,v,&p);     
        }   
    }
}

//前序遍历二叉树
void PreOrderTraverse(CSTree T){  
    if (T) {  
        printf("%d ",T->data);   
        PreOrderTraverse(T->lchild);  
        PreOrderTraverse(T->nextsibling);  
    }   
    return;
}

int main() {   
    MGraph G;//建立一个图的变量   
    CreateDN(&G);//初始化图  
    CSTree T;  
    DFSForest(G, &T); 
    PreOrderTraverse(T); 
    return 0;
}
```

运行程序，拿图 4（a）中的非连通图为例，构建的深度优先生成森林，使用孩子兄弟表示法表示为：

![img](https://raw.githubusercontent.com/lindage1994/images/master/typora202009/26/200757-553735.png)
图5 孩子兄弟表示法表示深度优先生成森林

> 图中，3 种颜色的树各代表一棵深度优先生成树，使用孩子兄弟表示法表示，也就是将三棵树的树根相连，第一棵树的树根作为整棵树的树根。


运行结果

13,13
1
2
3
4
5
6
7
8
9
10
11
12
13
1,2
1,3
1,6
1,12
2,13
4,5
7,8
7,10
7,9
8,10
11,12
11,13
12,13
1 2 13 11 12 3 6 4 5 7 8 10 9

## 广度优先生成森林

非连通图采用广度优先搜索算法进行遍历时，经过的顶点以及边的集合为该图的广度优先生成森林。

拿图 4（a）中的非连通图为例，通过广度优先搜索得到的广度优先生成森林用孩子兄弟表示法为：

![img](https://raw.githubusercontent.com/lindage1994/images/master/typora202009/26/200808-902107.png)
图6 广度优先生成森林（孩子兄弟表示法）


实现代码为：

```
#include <stdio.h>#include <stdlib.h>#define MAX_VERtEX_NUM 20                   //顶点的最大个数#define VRType int                          //表示顶点之间的关系的变量类型#define InfoType char                       //存储弧或者边额外信息的指针变量类型#define VertexType int                      //图中顶点的数据类型typedef enum{false,true}bool;               //定义bool型常量bool visited[MAX_VERtEX_NUM];               //设置全局数组，记录标记顶点是否被访问过typedef struct {    VRType adj;                             //对于无权图，用 1 或 0 表示是否相邻；对于带权图，直接为权值。    InfoType * info;                        //弧或边额外含有的信息指针}ArcCell,AdjMatrix[MAX_VERtEX_NUM][MAX_VERtEX_NUM];typedef struct {    VertexType vexs[MAX_VERtEX_NUM];        //存储图中顶点数据    AdjMatrix arcs;                         //二维数组，记录顶点之间的关系    int vexnum,arcnum;                      //记录图的顶点数和弧（边）数}MGraph;typedef struct CSNode{    VertexType data;    struct CSNode * lchild;//孩子结点    struct CSNode * nextsibling;//兄弟结点}*CSTree,CSNode;typedef struct Queue{    CSTree data;//队列中存放的为树结点    struct Queue * next;}Queue;//根据顶点本身数据，判断出顶点在二维数组中的位置int LocateVex(MGraph * G,VertexType v){    int i=0;    //遍历一维数组，找到变量v    for (; i<G->vexnum; i++) {        if (G->vexs[i]==v) {            break;        }    }    //如果找不到，输出提示语句，返回-1    if (i>G->vexnum) {        printf("no such vertex.\n");        return -1;    }    return i;}//构造无向图void CreateDN(MGraph *G){    scanf("%d,%d",&(G->vexnum),&(G->arcnum));    for (int i=0; i<G->vexnum; i++) {        scanf("%d",&(G->vexs[i]));    }    for (int i=0; i<G->vexnum; i++) {        for (int j=0; j<G->vexnum; j++) {            G->arcs[i][j].adj=0;            G->arcs[i][j].info=NULL;        }    }    for (int i=0; i<G->arcnum; i++) {        int v1,v2;        scanf("%d,%d",&v1,&v2);        int n=LocateVex(G, v1);        int m=LocateVex(G, v2);        if (m==-1 ||n==-1) {            printf("no this vertex\n");            return;        }        G->arcs[n][m].adj=1;        G->arcs[m][n].adj=1;//无向图的二阶矩阵沿主对角线对称    }}int FirstAdjVex(MGraph G,int v){    //查找与数组下标为v的顶点之间有边的顶点，返回它在数组中的下标    for(int i = 0; i<G.vexnum; i++){        if( G.arcs[v][i].adj ){            return i;        }    }    return -1;}int NextAdjVex(MGraph G,int v,int w){    //从前一个访问位置w的下一个位置开始，查找之间有边的顶点    for(int i = w+1; i<G.vexnum; i++){        if(G.arcs[v][i].adj){            return i;        }    }    return -1;}//初始化队列void InitQueue(Queue ** Q){    (*Q)=(Queue*)malloc(sizeof(Queue));    (*Q)->next=NULL;}//结点v进队列void EnQueue(Queue **Q,CSTree T){    Queue * element=(Queue*)malloc(sizeof(Queue));    element->data=T;    element->next=NULL;       Queue * temp=(*Q);    while (temp->next!=NULL) {        temp=temp->next;    }    temp->next=element;}//队头元素出队列void DeQueue(Queue **Q,CSTree *u){    (*u)=(*Q)->next->data;    (*Q)->next=(*Q)->next->next;}//判断队列是否为空bool QueueEmpty(Queue *Q){    if (Q->next==NULL) {        return true;    }    return false;}void BFSTree(MGraph G,int v,CSTree*T){    CSTree q=NULL;    Queue * Q;    InitQueue(&Q);    //根结点入队    EnQueue(&Q, (*T));    //当队列为空时，证明遍历完成    while (!QueueEmpty(Q)) {        bool first=true;        //队列首个结点出队        DeQueue(&Q,&q);        //判断结点中的数据在数组中的具体位置        int v=LocateVex(&G,q->data);        //已经访问过的更改其标志位        visited[v]=true;        //遍历以出队结点为起始点的所有邻接点        for (int w=FirstAdjVex(G,v); w>=0; w=NextAdjVex(G,v, w)) {            //标志位为false，证明未遍历过            if (!visited[w]) {                //新建一个结点 p，存放当前遍历的顶点                CSTree p=(CSTree)malloc(sizeof(CSNode));                p->data=G.vexs[w];                p->lchild=NULL;                p->nextsibling=NULL;                //当前结点入队                EnQueue(&Q, p);                //更改标志位                visited[w]=true;                //如果是出队顶点的第一个邻接点，设置p结点为其左孩子                if (first) {                    q->lchild=p;                    first=false;                }                //否则设置其为兄弟结点                else{                    q->nextsibling=p;                }                q=p;            }        }    }}//广度优先搜索生成森林并转化为二叉树void BFSForest(MGraph G,CSTree *T){    (*T)=NULL;    //每个顶点的标记为初始化为false    for (int v=0; v<G.vexnum; v++) {        visited[v]=false;    }    CSTree q=NULL;    //遍历图中所有的顶点    for (int v=0; v<G.vexnum; v++) {        //如果该顶点的标记位为false，证明未访问过        if (!(visited[v])) {            //新建一个结点，表示该顶点            CSTree p=(CSTree)malloc(sizeof(CSNode));            p->data=G.vexs[v];            p->lchild=NULL;            p->nextsibling=NULL;            //如果树未空，则该顶点作为树的树根            if (!(*T)) {                (*T)=p;            }            //该顶点作为树根的兄弟结点            else{                q->nextsibling=p;            }            //每次都要把q指针指向新的结点，为下次添加结点做铺垫            q=p;            //以该结点为起始点，构建广度优先生成树            BFSTree(G,v,&p);        }    }}//前序遍历二叉树void PreOrderTraverse(CSTree T){    if (T) {        printf("%d ",T->data);        PreOrderTraverse(T->lchild);        PreOrderTraverse(T->nextsibling);    }    return;}int main() {    MGraph G;//建立一个图的变量    CreateDN(&G);//初始化图    CSTree T;    BFSForest(G, &T);    PreOrderTraverse(T);    return 0;}
```

运行结果为：

13,13
1
2
3
4
5
6
7
8
9
10
11
12
13
1,2
1,3
1,6
1,12
2,13
4,5
7,8
7,10
7,9
8,10
11,12
11,13
12,13
1 2 13 3 6 12 11 4 5 7 8 9 10

在无向图中，如果任意两个顶点之间含有不止一条通路，这个图就被称为重连通图。在重连通图中，在删除某个顶点及该顶点相关的边后，图中各顶点之间的连通性也不会被破坏。

在一个无向图中，如果删除某个顶点及其相关联的边后，原来的图被分割为两个及以上的连通分量，则称该顶点为无向图中的一个关节点（或者“割点”）。

![img](https://raw.githubusercontent.com/lindage1994/images/master/typora202009/26/201013-564296.png)
图 1 连通图


图 1 是连通图但不是重连通图，图中有4个关节点，分别是：A、B、D 和 G。比如删除顶点 B 及相关联的边后，原图就变为：

![img](https://raw.githubusercontent.com/lindage1994/images/master/typora202009/26/201006-785214.png)
图 2 连通分量


可以看到，图被分割为各自独立的 3 部分，顶点集合分别为：{A、C、F、L、M、J}、{G、H、I、K} 和 {D、E}。

了解了什么是关节点后，重连通图其实就是没有关节点的连通图。

在重连通图中，只删除一个顶点及其相关联的边，肯定不会破坏其连通性。如果一味地做删除顶点的操作，直到删除 K 个顶点及其关联的边后，图的连通性才遭到破坏，则称此重连通图的连通度为 K 。

## 重连通图的实际应用

如今的通信网络对人们的生活有着重要的影响，如果将通信网络比做一个巨大的连通图的话，它的连通度 K 值越高，证明其稳定性越好，即使某一个站点发生故障无法工作也不会影响整个系统的正常工作。

同样，小到城市之间，大到国家之间的航空网也可以看作是一个连通图，但如果此图建设成为重连通图，当某条航线因为天气等因素关闭时，飞机仍可以从别的航线到达目的地。

在战争中，有“兵马未动，粮草先行”的说法，可见后勤补给对军队的重要性。如果补给线是一个重连通图，就不用过于担心补给线被破坏的问题，因为即使破坏一条，还有其它的，只要连通度足够大。

## 判断重连通图的方法

了解了什么是重连通图之后，如何编写程序直接判断一个图是否是重连通图呢？

对于任意一个连通图来说，都可以通过深度优先搜索算法获得一棵深度优先生成树，例如，图 1 通过深度优先搜索获得的深度优先生成树为：

![img](https://raw.githubusercontent.com/lindage1994/images/master/typora202009/26/200932-893173.png)
图 3 深度优先生成树


虚线表示遍历生成树时未用到的边，简称“回边”。也就是图中有，但是遍历时没有用到，生成树中用虚线表示出来。

在深度优先生成树中，图中的关节点有两种特性：

1. 首先判断整棵树的树根结点，如果树根有两条或者两条以上的子树，则该顶点肯定是关节点。因为一旦树根丢失，生成树就会变成森林。
2. 然后判断生成树中的每个非叶子结点，以该结点为根结点的每棵子树中如果有结点的回边与此非叶子结点的祖宗结点相关联，那么此非叶子结点就不是关节点；反之，就是关节点。

> 注意：必须是和该非叶子结点的祖宗结点（不包括结点本身）相关联，才说明此结点不是关节点。


所以，判断一个图是否是重连通图，也可以转变为：判断图中是否有关节点，如果没有关节点，证明此图为重连通图；反之则不是。

拿图 3 的生成树来说，利用两个特性判断每个顶点是否为关节点：

- 首先，判断树根结点 A ，由于有两个孩子，也就是有两棵子树，所以 A 是关节点。然后判断树中所有的非叶子结点，也就是： L 、 M 、 B 、 D 、 H 、 K 、 G ；
- L 结点为根结点的子树中 B 结点有回边直接关联 A ，所以， L 不是关节点；
- 在以 M 结点为树根的子树中，J 结点和 B 结点都有回边关联 M 结点的祖宗结点，所以，M 不是关节点；
- 以 B 结点为根结点的 3 棵子树中，只有一棵子树（只包含结点 C ）与 B 结点的祖宗结点 A 有关联，其他两棵子树没有，所以结点 B 是关节点；
- 以 D 结点为根结点的子树中只有结点 E，且没有回边与祖宗结点关联，所以，D 是关节点；
- 以 H 结点为根结点的子树中， G 结点与 B 结点关联，所以， H 结点不是关节点；
- K 结点和 H 结点相同，由于 G 结点与祖宗结点 B 关联，所以 K 结点不是关节点；
- 以 G 结点为根结点的子树中只有一个结点 I，没有回边，所以结点 G 是关节点；


综上所述，图 3 中的关节点有 4 个，分别是： A 、 B 、 D 、 G 。

在学习拓扑排序一节时讲到拓扑排序只适用于 AOV 网，本节所介绍的求关键路径针对的是和 AOV 网相近的 AOE 网。

## AOE网

AOE 网是在 AOV 网的基础上，其中每一个边都具有各自的权值，是一个有向无环网。其中权值表示活动持续的时间。

![img](https://raw.githubusercontent.com/lindage1994/images/master/typora202009/26/200902-701130.png)
图] 1 AOE网


如图 1 所示就是一个 AOE 网，例如 a1=6 表示完成 a1 活动完成需要 6 天；AOE 网中每个顶点表示在它之前的活动已经完成，可以开始后边的活动，例如 V5 表示 a4 和 a5 活动已经完成，a7 和 a8 可以开始。

使用 AOE 网可以帮助解决这样的问题：如果将 AOE 网看做整个项目，那么完成整个项目至少需要多少时间？

解决这个问题的关键在于从 AOE 网中找到一条从起始点到结束点长度最长的路径，这样就能保证所有的活动在结束之前都能完成。

> 起始点是入度为 0 的点，称为“源点”；结束点是出度为 0 的点，称为“汇点”。这条最长的路径，被称为”关键路径“。

## 关键路径

为了求出一个给定 AOE 网的关键路径，需要知道以下 4 个统计数据：

- 对于 AOE 网中的顶点有两个时间：最早发生时间（用 Ve(j) 表示）和最晚发生时间（用 Vl(j) 表示）；
- 对于边来说，也有两个时间：最早开始时间（用 e(i) 表示）和最晚开始时间（ l(i) 表示）。


Ve(j)：对于 AOE 网中的任意一个顶点来说，从源点到该点的最长路径代表着该顶点的最早发生时间，通常用 Ve(j) 表示。

例如，图 1 中从 V1 到 V5 有两条路径，V1 作为源点开始后，a1 和 a2 同时开始活动，但由于 a1 和 a2 活动的时间长度不同，最终 V1-V3-V5 的这条路径率先完成。但是并不是说 V5 之后的活动就可以开始，而是需要等待 V1-V2-V5 这条路径也完成之后才能开始。所以对于 V5 来讲，Ve(5) = 7。


Vl(j)：表示在不推迟整个工期的前提下，事件 Vk 允许的最晚发生时间。

例如，图 1 中，在得知整个工期完成的时间是 18 天的前提下，V7 最晚要在第 16 天的时候开始，因为 a10 活动至少需要 2 天时间才能完成，如果在 V7 事件在推迟，就会拖延整个工期。所以，对于 V7 来说，它的 Vl(7)=16。


e(i)：表示活动 ai 的最早开始时间，如果活动 ai 是由弧 <Vk,Vj> 表示的，那么活动 ai 的最早开始的时间就等于时间 Vk 的最早发生时间，也就是说：e[i] = ve[k]。

e(i)很好理解，拿图 1 中 a4 来说，如果 a4 想要开始活动，那么首先前提就是 V2 事件开始。所以 e[4]=ve[2]。


l(i)：表示活动 ai 的最晚开始时间，如果活动 ai 是由弧 <Vk,Vj> 表示，ai 的最晚开始时间的设定要保证 Vj 的最晚发生时间不拖后。所以，l[i]=Vl[j]-len<Vk,Vj>。

在得知以上四种统计数据后，就可以直接求得 AOE 网中关键路径上的所有的关键活动，方法是：对于所有的边来说，如果它的最早开始时间等于最晚开始时间，称这条边所代表的活动为关键活动。由关键活动构成的路径为关键路径。

## 求关键路径的具体过程

对图 1 中的 AOE 图求关键路径，首先完成 Ve(j)、Vl(j)、e(i)、l(i) 4 种统计信息的准备工作。

Ve(j)，求出从源点到各顶点的最长路径长度为（长度最大的）：

![img](https://raw.githubusercontent.com/lindage1994/images/master/typora202009/26/200852-638509.png)

Vl(j)，求出各顶点的最晚发生时间（从后往前推，多种情况下选择最小的）：

![img](https://raw.githubusercontent.com/lindage1994/images/master/typora202009/26/200847-280541.png)

e(i)，求出各边中ai活动的最早开始时间：

![img](https://raw.githubusercontent.com/lindage1994/images/master/typora202009/26/200840-784218.png)

l(i),求各边中ai活动的最晚开始时间（多种情况下，选择最小的）：

![img](https://raw.githubusercontent.com/lindage1994/images/master/typora202009/26/200831-772585.png)

通过对比 l(i) 和 e(i) ，其中 a1 、 a4 、 a7 、 a8 、 a10 、 a11 的值都各自相同，所以，在图 1 中的 AOE 网中有两条关键路径：

![img](https://raw.githubusercontent.com/lindage1994/images/master/typora202009/26/200825-550885.png)
图 2 关键路径


关键路径的代码实现

```
#include <stdio.h>#include <stdlib.h>#define  MAX_VERTEX_NUM 20//最大顶点个数#define  VertexType int//顶点数据的类型typedef enum{false,true} bool;//建立全局变量，保存边的最早开始时间VertexType ve[MAX_VERTEX_NUM];//建立全局变量，保存边的最晚开始时间VertexType vl[MAX_VERTEX_NUM];typedef struct ArcNode{    int adjvex;//邻接点在数组中的位置下标    struct ArcNode * nextarc;//指向下一个邻接点的指针    VertexType dut;}ArcNode;typedef struct VNode{    VertexType data;//顶点的数据域    ArcNode * firstarc;//指向邻接点的指针}VNode,AdjList[MAX_VERTEX_NUM];//存储各链表头结点的数组typedef struct {    AdjList vertices;//图中顶点及各邻接点数组    int vexnum,arcnum;//记录图中顶点数和边或弧数}ALGraph;//找到顶点对应在邻接表数组中的位置下标int LocateVex(ALGraph G,VertexType u){    for (int i=0; i<G.vexnum; i++) {        if (G.vertices[i].data==u) {            return i;        }    }    return -1;}//创建AOE网，构建邻接表void CreateAOE(ALGraph **G){    *G=(ALGraph*)malloc(sizeof(ALGraph));       scanf("%d,%d",&((*G)->vexnum),&((*G)->arcnum));    for (int i=0; i<(*G)->vexnum; i++) {        scanf("%d",&((*G)->vertices[i].data));        (*G)->vertices[i].firstarc=NULL;    }    VertexType initial,end,dut;    for (int i=0; i<(*G)->arcnum; i++) {        scanf("%d,%d,%d",&initial,&end,&dut);               ArcNode *p=(ArcNode*)malloc(sizeof(ArcNode));        p->adjvex=LocateVex(*(*G), end);        p->nextarc=NULL;        p->dut=dut;               int locate=LocateVex(*(*G), initial);        p->nextarc=(*G)->vertices[locate].firstarc;        (*G)->vertices[locate].firstarc=p;    }}//结构体定义栈结构typedef struct stack{    VertexType data;    struct stack * next;}stack;stack *T;//初始化栈结构void initStack(stack* *S){    (*S)=(stack*)malloc(sizeof(stack));    (*S)->next=NULL;}//判断栈是否为空bool StackEmpty(stack S){    if (S.next==NULL) {        return true;    }    return false;}//进栈，以头插法将新结点插入到链表中void push(stack *S,VertexType u){    stack *p=(stack*)malloc(sizeof(stack));    p->data=u;    p->next=NULL;    p->next=S->next;    S->next=p;}//弹栈函数，删除链表首元结点的同时，释放该空间，并将该结点中的数据域通过地址传值给变量i;void pop(stack *S,VertexType *i){    stack *p=S->next;    *i=p->data;    S->next=S->next->next;    free(p);}//统计各顶点的入度void FindInDegree(ALGraph G,int indegree[]){    //初始化数组，默认初始值全部为0    for (int i=0; i<G.vexnum; i++) {        indegree[i]=0;    }    //遍历邻接表，根据各链表中结点的数据域存储的各顶点位置下标，在indegree数组相应位置+1    for (int i=0; i<G.vexnum; i++) {        ArcNode *p=G.vertices[i].firstarc;        while (p) {            indegree[p->adjvex]++;            p=p->nextarc;        }    }}bool TopologicalOrder(ALGraph G){    int indegree[G.vexnum];//创建记录各顶点入度的数组    FindInDegree(G,indegree);//统计各顶点的入度    //建立栈结构，程序中使用的是链表    stack *S;    //初始化栈    initStack(&S);    for (int i=0; i<G.vexnum; i++) {        ve[i]=0;    }    //查找度为0的顶点，作为起始点    for (int i=0; i<G.vexnum; i++) {        if (!indegree[i]) {            push(S, i);        }    }    int count=0;    //栈为空为结束标志    while (!StackEmpty(*S)) {        int index;        //弹栈，并记录栈中保存的顶点所在邻接表数组中的位置        pop(S,&index);        //压栈，为求各边的最晚开始时间做准备        push(T, index);        ++count;        //依次查找跟该顶点相链接的顶点，如果初始入度为1，当删除前一个顶点后，该顶点入度为0        for (ArcNode *p=G.vertices[index].firstarc; p ; p=p->nextarc) {                       VertexType k=p->adjvex;                       if (!(--indegree[k])) {                //顶点入度为0，入栈                push(S, k);            }            //如果边的源点的最长路径长度加上边的权值比汇点的最长路径长度还长，就覆盖ve数组中对应位置的值，最终结束时，ve数组中存储的就是各顶点的最长路径长度。            if (ve[index]+p->dut>ve[k]) {                ve[k]=ve[index]+p->dut;            }        }    }    //如果count值小于顶点数量，表明有向图有环    if (count<G.vexnum) {        printf("该图有回路");        return false;    }    return true;}//求各顶点的最晚发生时间并计算出各边的最早和最晚开始时间void CriticalPath(ALGraph G){    if (!TopologicalOrder(G)) {        return ;    }    for (int i=0 ; i<G.vexnum ; i++) {        vl[i]=ve[G.vexnum-1];    }    int j,k;    while (!StackEmpty(*T)) {        pop(T, &j);        for (ArcNode* p=G.vertices[j].firstarc ; p ; p=p->nextarc) {            k=p->adjvex;            //构建Vl数组，在初始化时，Vl数组中每个单元都是18，如果每个边的汇点-边的权值比源点值小，就保存更小的。            if (vl[k]-p->dut<vl[j]) {                vl[j] = vl[k]-p->dut;            }        }    }    for (j = 0; j < G.vexnum; j++) {        for (ArcNode*p = G.vertices[j].firstarc; p ;p = p->nextarc) {            k = p->adjvex;            //求各边的最早开始时间e[i],等于ve数组中相应源点存储的值            int ee = ve[j];            //求各边的最晚开始时间l[i]，等于汇点在vl数组中存储的值减改边的权值            int el = vl[k]-p->dut;            //判断e[i]和l[i]是否相等，如果相等，该边就是关键活动，相应的用*标记；反之，边后边没标记            char tag = (ee==el)?'*':' ';            printf("%3d%3d%3d%3d%3d%2c\n",j,k,p->dut,ee,el,tag);        }    }}int main(){    ALGraph *G;    CreateAOE(&G);//创建AOE网    initStack(&T);    TopologicalOrder(*G);    CriticalPath(*G);    return  0;}
```

拿图 1 中的 AOE 网为例，运行的结果为：

9,11
1
2
3
4
5
6
7
8
9
1,2,6
1,3,4
1,4,5
2,5,1
3,5,1
4,6,2
5,7,9
5,8,7
6,8,4
7,9,2
8,9,4
 0 3 5 0 3 
 0 2 4 0 2 
 0 1 6 0 0 *
 1 4 1 6 6 *
 2 4 1 4 6 
 3 5 2 5 8 
 4 7 7 7 7 *
 4 6 9 7 7 *
 5 7 4 7 10 
 6 8 2 16 16 *
 7 8 4 14 14 *


通过运行结果，可以看出关键活动有 6 个（后面带有 * 号的），而组成的关键路径就如图 2 中所示。