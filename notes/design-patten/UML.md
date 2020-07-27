---
layout: post
title: "design_pattern_UML"
date: 2019-01-13
description: "设计模式之UML"
excerpt: "设计模式之UML"
tag: [design_pattern]
comments: true
---
### uml简介

> UML Unified Modeling Language 统一建模语言

#### uml核心

uml核心是图表，大致可以分为两类：结构图和行为图

> 结构型的图(Structure Diagram)

- 类图(Class Diagram)
- 对象图(Object Diagram)
- 构件图(Component Diagram)
- 部署图(Deployment Diagram)
- 包图(Package Diagram)

> 行为图(Behavior Diagram)

- 活动图(Activity Diagram)
- 状态机图(State Machine Diagram)
- 顺序图(Sequence Diagram)
- 通信图(Communication Diagram)
- 用例图(Use Case Diagram)
- 时序图(Timing Diagram)

#### 类图(class diagram)
>类图主要是用来显示系统中的类、接口以及它们之间的静态结构和关系的一种静态模型。


![类图示例](https://github.com/lindage1994/images/raw/master/blog/2019/design_pattern_1.png)


- 类图的三个基本组建，类名、属性、方法。

> 类、接口之间的六种关系：继承、实现、依赖、关联、聚合、组合

泛化(generalization)：表示is-a的关系，是对象之间耦合度最大的一种关系，子类继承父类的所有细节。直接使用语言中的继承表达。在类图中使用带三角箭头的实线表示，箭头从子类指向父类。


![泛化](https://github.com/lindage1994/images/raw/master/blog/2019/design_pattern2.jpg)


实现（Realization）:在类图中就是接口和实现的关系。这个没什么好讲的。在类图中使用带三角箭头的虚线表示，箭头从实现类指向接口。


![实现](https://github.com/lindage1994/images/raw/master/blog/2019/design_pattern3.jpg)


依赖(Dependency)：对象之间最弱的一种关联方式，是临时性的关联。代码中一般指由局部变量、函数参数、返回值建立的对于其他对象的调用关系。一个类调用被依赖类中的某些方法而得以完成这个类的一些职责。在类图使用带箭头的虚线表示，箭头从使用类指向被依赖的类。


![依赖](https://github.com/lindage1994/images/raw/master/blog/2019/design_pattern4.jpg)


关联(Association) : 对象之间一种引用关系，比如客户类与订单类之间的关系。这种关系通常使用类的属性表达。关联又分为一般关联、聚合关联与组合关联。后两种在后面分析。在类图使用带箭头的实线表示，箭头从使用类指向被关联的类。可以是单向和双向。


![关联](https://github.com/lindage1994/images/raw/master/blog/2019/design_pattern5.jpg)


聚合(Aggregation) : 表示has-a的关系，是一种不稳定的包含关系。较强于一般关联,有整体与局部的关系,并且没有了整体,局部也可单独存在。如公司和员工的关系，公司包含员工，但如果公司倒闭，员工依然可以换公司。在类图使用空心的菱形表示，菱形从局部指向整体。


![聚合](https://github.com/lindage1994/images/raw/master/blog/2019/design_pattern6.jpg)


组合(Composition) : 表示contains-a的关系，是一种强烈的包含关系。组合类负责被组合类的生命周期。是一种更强的聚合关系。部分不能脱离整体存在。如公司和部门的关系，没有了公司，部门也不能存在了；调查问卷中问题和选项的关系；订单和订单选项的关系。在类图使用实心的菱形表示，菱形从局部指向整体。


![组合](https://github.com/lindage1994/images/raw/master/blog/2019/design_pattern7.jpg)


多重性(Multiplicity) : 通常在关联、聚合、组合中使用。就是代表有多少个关联对象存在。使用数字..星号（数字）表示。如下图，一个割接通知可以关联0个到N个故障单。


![多重性](https://github.com/lindage1994/images/raw/master/blog/2019/design_pattern8.jpg)
