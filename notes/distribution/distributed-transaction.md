# 前言
不知道你是否遇到过这样的情况，去小卖铺买东西，付了钱，但是店主因为处理了一些其他事，居然忘记你付了钱，又叫你重新付。又或者在网上购物明明已经扣款，但是却告诉我没有发生交易。这一系列情况都是因为没有事务导致的。这说明了事务在生活中的一些重要性。有了事务，你去小卖铺买东西，那就是一手交钱一手交货。有了事务，你去网上购物，扣款即产生订单交易。
## 事务的具体定义
事务提供一种机制将一个活动涉及的所有操作纳入到一个不可分割的执行单元，组成事务的所有操作只有在所有操作均能正常执行的情况下方能提交，只要其中任一操作执行失败，都将导致整个事务的回滚。简单地说，事务提供一种“要么什么都不做，要么做全套（All or Nothing）”机制。

# 数据库本地事务
## ACID
说到数据库事务就不得不说，数据库事务中的四大特性，ACID:
- A:原子性(Atomicity)

一个事务(transaction)中的所有操作，要么全部完成，要么全部不完成，不会结束在中间某个环节。事务在执行过程中发生错误，会被回滚（Rollback）到事务开始前的状态，就像这个事务从来没有执行过一样。

就像你买东西要么交钱收货一起都执行，要么要是发不出货，就退钱。

- C:一致性(Consistency)

事务的一致性指的是在一个事务执行之前和执行之后数据库都必须处于一致性状态。如果事务成功地完成，那么系统中所有变化将正确地应用，系统处于有效状态。如果在事务中出现错误，那么系统中的所有变化将自动地回滚，系统返回到原始状态。

- I:隔离性(Isolation)

指的是在并发环境中，当不同的事务同时操纵相同的数据时，每个事务都有各自的完整数据空间。由并发事务所做的修改必须与任何其他并发事务所做的修改隔离。事务查看数据更新时，数据所处的状态要么是另一事务修改它之前的状态，要么是另一事务修改它之后的状态，事务不会查看到中间状态的数据。

打个比方，你买东西这个事情，是不影响其他人的。

- D:持久性(Durability)

指的是只要事务成功结束，它对数据库所做的更新就必须永久保存下来。即使发生系统崩溃，重新启动数据库系统后，数据库还能恢复到事务成功结束时的状态。

打个比方，你买东西的时候需要记录在账本上，即使老板忘记了那也有据可查。
## InnoDB实现原理
InnoDB是mysql的一个存储引擎，大部分人对mysql都比较熟悉，这里简单介绍一下数据库事务实现的一些基本原理，在本地事务中，服务和资源在事务的包裹下可以看做是一体的:

![](https://raw.githubusercontent.com/lindage1994/images/master/typora202010/17/200020-128755.png)
我们的本地事务由资源管理器进行管理:
![](https://raw.githubusercontent.com/lindage1994/images/master/typora202010/17/200004-98885.png)
而事务的ACID是通过InnoDB日志和锁来保证。事务的隔离性是通过数据库锁的机制实现的，持久性通过redo log（重做日志）来实现，原子性和一致性通过Undo log来实现。UndoLog的原理很简单，为了满足事务的原子性，在操作任何数据之前，首先将数据备份到一个地方（这个存储数据备份的地方称为UndoLog）。然后进行数据的修改。如果出现了错误或者用户执行了ROLLBACK语句，系统可以利用Undo Log中的备份将数据恢复到事务开始之前的状态。
和Undo Log相反，RedoLog记录的是新数据的备份。在事务提交前，只要将RedoLog持久化即可，不需要将数据持久化。当系统崩溃时，虽然数据没有持久化，但是RedoLog已经持久化。系统可以根据RedoLog的内容，将所有数据恢复到最新的状态。
对具体实现过程有兴趣的同学可以去自行搜索扩展。

# 分布式事务
## 什么是分布式事务
分布式事务就是指事务的参与者、支持事务的服务器、资源服务器以及事务管理器分别位于不同的分布式系统的不同节点之上。简单的说，就是一次大的操作由不同的小操作组成，这些小的操作分布在不同的服务器上，且属于不同的应用，分布式事务需要保证这些小操作要么全部成功，要么全部失败。本质上来说，分布式事务就是为了保证不同数据库的数据一致性。
## 分布式事务产生的原因
从上面本地事务来看，我们可以看为两块，一个是service产生多个节点，另一个是resource产生多个节点。

### service多个节点
随着互联网快速发展，微服务，SOA等服务架构模式正在被大规模的使用，举个简单的例子，一个公司之内，用户的资产可能分为好多个部分，比如余额，积分，优惠券等等。在公司内部有可能积分功能由一个微服务团队维护，优惠券又是另外的团队维护
![](https://raw.githubusercontent.com/lindage1994/images/master/typora202010/17/195942-380177.png)
这样的话就无法保证积分扣减了之后，优惠券能否扣减成功。

### resource多个节点
同样的，互联网发展得太快了，我们的Mysql一般来说装千万级的数据就得进行分库分表，对于一个支付宝的转账业务来说，你给的朋友转钱，有可能你的数据库是在北京，而你的朋友的钱是存在上海，所以我们依然无法保证他们能同时成功。
![](https://raw.githubusercontent.com/lindage1994/images/master/typora202010/17/200037-365352.png)

## 分布式事务的基础
从上面来看分布式事务是随着互联网高速发展应运而生的，这是一个必然的我们之前说过数据库的ACID四大特性，已经无法满足我们分布式事务，这个时候又有一些新的大佬提出一些新的理论:
### CAP
CAP定理，又被叫作布鲁尔定理。对于设计分布式系统来说(不仅仅是分布式事务)的架构师来说，CAP就是你的入门理论。
- C (一致性):对某个指定的客户端来说，读操作能返回最新的写操作。对于数据分布在不同节点上的数据上来说，如果在某个节点更新了数据，那么在其他节点如果都能读取到这个最新的数据，那么就称为强一致，如果有某个节点没有读取到，那就是分布式不一致。
- A (可用性)：非故障的节点在合理的时间内返回合理的响应(不是错误和超时的响应)。可用性的两个关键一个是合理的时间，一个是合理的响应。合理的时间指的是请求不能无限被阻塞，应该在合理的时间给出返回。合理的响应指的是系统应该明确返回结果并且结果是正确的，这里的正确指的是比如应该返回50，而不是返回40。
- P (分区容错性):当出现网络分区后，系统能够继续工作。打个比方，这里个集群有多台机器，有台机器网络出现了问题，但是这个集群仍然可以正常工作。

熟悉CAP的人都知道，三者不能共有，如果感兴趣可以搜索CAP的证明，在分布式系统中，网络无法100%可靠，分区其实是一个必然现象，如果我们选择了CA而放弃了P，那么当发生分区现象时，为了保证一致性，这个时候必须拒绝请求，但是A又不允许，所以分布式系统理论上不可能选择CA架构，只能选择CP或者AP架构。

对于CP来说，放弃可用性，追求一致性和分区容错性，我们的zookeeper其实就是追求的强一致。

对于AP来说，放弃一致性(这里说的一致性是强一致性)，追求分区容错性和可用性，这是很多分布式系统设计时的选择，后面的BASE也是根据AP来扩展。

顺便一提，CAP理论中是忽略网络延迟，也就是当事务提交时，从节点A复制到节点B，但是在现实中这个是明显不可能的，所以总会有一定的时间是不一致。同时CAP中选择两个，比如你选择了CP，并不是叫你放弃A。因为P出现的概率实在是太小了，大部分的时间你仍然需要保证CA。就算分区出现了你也要为后来的A做准备，比如通过一些日志的手段，是其他机器回复至可用。

### BASE
 BASE 是 Basically Available(基本可用)、Soft state(软状态)和 Eventually consistent (最终一致性)三个短语的缩写。是对CAP中AP的一个扩展
1.   基本可用:分布式系统在出现故障时，允许损失部分可用功能，保证核心功能可用。
2.   软状态:允许系统中存在中间状态，这个状态不影响系统可用性，这里指的是CAP中的不一致。
3.   最终一致:最终一致是指经过一段时间后，所有节点数据都将会达到一致。

BASE解决了CAP中理论没有网络延迟，在BASE中用软状态和最终一致，保证了延迟后的一致性。BASE和 ACID 是相反的，它完全不同于ACID的强一致性模型，而是通过牺牲强一致性来获得可用性，并允许数据在一段时间内是不一致的，但最终达到一致状态。

# 分布式事务解决方案
有了上面的理论基础后，这里介绍开始介绍几种常见的分布式事务的解决方案。
## 是否真的要分布式事务
在说方案之前，首先你一定要明确你是否真的需要分布式事务？

上面说过出现分布式事务的两个原因，其中有个原因是因为微服务过多。我见过太多团队一个人维护几个微服务，太多团队过度设计，搞得所有人疲劳不堪，而微服务过多就会引出分布式事务，这个时候我不会建议你去采用下面任何一种方案，而是请把需要事务的微服务聚合成一个单机服务，使用数据库的本地事务。因为不论任何一种方案都会增加你系统的复杂度，这样的成本实在是太高了，千万不要因为追求某些设计，而引入不必要的成本和复杂度。

如果你确定需要引入分布式事务可以看看下面几种常见的方案。

## 2PC
说到2PC就不得不聊数据库分布式事务中的 XA Transactions。
![](https://raw.githubusercontent.com/lindage1994/images/master/typora202010/17/200134-775525.jpeg)
在XA协议中分为两阶段:

第一阶段：事务管理器要求每个涉及到事务的数据库预提交(precommit)此操作，并反映是否可以提交.

第二阶段：事务协调器要求每个数据库提交数据，或者回滚数据。

优点： 尽量保证了数据的强一致，实现成本较低，在各大主流数据库都有自己实现，对于MySQL是从5.5开始支持。

缺点: 
- 单点问题:事务管理器在整个流程中扮演的角色很关键，如果其宕机，比如在第一阶段已经完成，在第二阶段正准备提交的时候事务管理器宕机，资源管理器就会一直阻塞，导致数据库无法使用。
- 同步阻塞:在准备就绪之后，资源管理器中的资源一直处于阻塞，直到提交完成，释放资源。
- 数据不一致:两阶段提交协议虽然为分布式数据强一致性所设计，但仍然存在数据不一致性的可能，比如在第二阶段中，假设协调者发出了事务commit的通知，但是因为网络问题该通知仅被一部分参与者所收到并执行了commit操作，其余的参与者则因为没有收到通知一直处于阻塞状态，这时候就产生了数据的不一致性。

总的来说，XA协议比较简单，成本较低，但是其单点问题，以及不能支持高并发(由于同步阻塞)依然是其最大的弱点。
## TCC
关于TCC（Try-Confirm-Cancel）的概念，最早是由Pat Helland于2007年发表的一篇名为《Life beyond Distributed Transactions:an Apostate’s Opinion》的论文提出。
TCC事务机制相比于上面介绍的XA，解决了其几个缺点:
1.解决了协调者单点，由主业务方发起并完成这个业务活动。业务活动管理器也变成多点，引入集群。
2.同步阻塞:引入超时，超时后进行补偿，并且不会锁定整个资源，将资源转换为业务逻辑形式，粒度变小。
3.数据一致性，有了补偿机制之后，由业务活动管理器控制一致性
![](https://raw.githubusercontent.com/lindage1994/images/master/typora202010/17/200151-172712.png)
对于TCC的解释:

- Try阶段：尝试执行,完成所有业务检查（一致性）,预留必须业务资源（准隔离性）

- Confirm阶段：确认执行真正执行业务，不作任何业务检查，只使用Try阶段预留的业务资源，Confirm操作满足幂等性。要求具备幂等设计，Confirm失败后需要进行重试。

- Cancel阶段：取消执行，释放Try阶段预留的业务资源
Cancel操作满足幂等性Cancel阶段的异常和Confirm阶段异常处理方案基本上一致。

举个简单的例子如果你用100元买了一瓶水，
Try阶段:你需要向你的钱包检查是否够100元并锁住这100元，水也是一样的。

如果有一个失败，则进行cancel(释放这100元和这一瓶水)，如果cancel失败不论什么失败都进行重试cancel，所以需要保持幂等。

如果都成功，则进行confirm,确认这100元扣，和这一瓶水被卖，如果confirm失败无论什么失败则重试(会依靠活动日志进行重试)

对于TCC来说适合一些:
- 强隔离性，严格一致性要求的活动业务。
- 执行时间较短的业务

实现参考:ByteTCC:https://github.com/liuyangming/ByteTCC/
## 本地消息表
本地消息表这个方案最初是ebay提出的 ebay的完整方案https://queue.acm.org/detail.cfm?id=1394128。

此方案的核心是将需要分布式处理的任务通过消息日志的方式来异步执行。消息日志可以存储到本地文本、数据库或消息队列，再通过业务规则自动或人工发起重试。人工重试更多的是应用于支付场景，通过对账系统对事后问题的处理。
![](https://raw.githubusercontent.com/lindage1994/images/master/typora202010/17/200224-299523.png)

对于本地消息队列来说核心是把大事务转变为小事务。还是举上面用100元去买一瓶水的例子。

1.当你扣钱的时候，你需要在你扣钱的服务器上新增加一个本地消息表，你需要把你扣钱和写入减去水的库存到本地消息表放入同一个事务(依靠数据库本地事务保证一致性。

2.这个时候有个定时任务去轮询这个本地事务表，把没有发送的消息，扔给商品库存服务器，叫他减去水的库存，到达商品服务器之后这个时候得先写入这个服务器的事务表，然后进行扣减，扣减成功后，更新事务表中的状态。

3.商品服务器通过定时任务扫描消息表或者直接通知扣钱服务器，扣钱服务器本地消息表进行状态更新。

4.针对一些异常情况，定时扫描未成功处理的消息，进行重新发送，在商品服务器接到消息之后，首先判断是否是重复的，如果已经接收，在判断是否执行，如果执行在马上又进行通知事务，如果未执行，需要重新执行需要由业务保证幂等，也就是不会多扣一瓶水。

本地消息队列是BASE理论，是最终一致模型，适用于对一致性要求不高的。实现这个模型时需要注意重试的幂等。
## MQ事务
在RocketMQ中实现了分布式事务，实际上其实是对本地消息表的一个封装，将本地消息表移动到了MQ内部，下面简单介绍一下MQ事务，如果想对其详细了解可以参考:
https://www.jianshu.com/p/453c6e7ff81c。
![](https://raw.githubusercontent.com/lindage1994/images/master/typora202010/17/200240-502990.png)
基本流程如下:
第一阶段Prepared消息，会拿到消息的地址。

第二阶段执行本地事务。

第三阶段通过第一阶段拿到的地址去访问消息，并修改状态。消息接受者就能使用这个消息。

如果确认消息失败，在RocketMq Broker中提供了定时扫描没有更新状态的消息，如果有消息没有得到确认，会向消息发送者发送消息，来判断是否提交，在rocketmq中是以listener的形式给发送者，用来处理。
![](https://raw.githubusercontent.com/lindage1994/images/master/typora202010/17/200300-36917.png)
如果消费超时，则需要一直重试，消息接收端需要保证幂等。如果消息消费失败，这个就需要人工进行处理，因为这个概率较低，如果为了这种小概率时间而设计这个复杂的流程反而得不偿失

## Saga事务
Saga是30年前一篇数据库伦理提到的一个概念。其核心思想是将长事务拆分为多个本地短事务，由Saga事务协调器协调，如果正常结束那就正常完成，如果某个步骤失败，则根据相反顺序一次调用补偿操作。
Saga的组成：

每个Saga由一系列sub-transaction Ti 组成
每个Ti 都有对应的补偿动作Ci，补偿动作用于撤销Ti造成的结果,这里的每个T，都是一个本地事务。
可以看到，和TCC相比，Saga没有“预留 try”动作，它的Ti就是直接提交到库。

Saga的执行顺序有两种：

T1, T2, T3, ..., Tn

T1, T2, ..., Tj, Cj,..., C2, C1，其中0 < j < n
Saga定义了两种恢复策略：

向后恢复，即上面提到的第二种执行顺序，其中j是发生错误的sub-transaction，这种做法的效果是撤销掉之前所有成功的sub-transation，使得整个Saga的执行结果撤销。
向前恢复，适用于必须要成功的场景，执行顺序是类似于这样的：T1, T2, ..., Tj(失败), Tj(重试),..., Tn，其中j是发生错误的sub-transaction。该情况下不需要Ci。

这里要注意的是，在saga模式中不能保证隔离性，因为没有锁住资源，其他事务依然可以覆盖或者影响当前事务。

还是拿100元买一瓶水的例子来说，这里定义

T1=扣100元 T2=给用户加一瓶水 T3=减库存一瓶水 

C1=加100元 C2=给用户减一瓶水 C3=给库存加一瓶水 

我们一次进行T1,T2，T3如果发生问题，就执行发生问题的C操作的反向。
上面说到的隔离性的问题会出现在，如果执行到T3这个时候需要执行回滚，但是这个用户已经把水喝了(另外一个事务)，回滚的时候就会发现，无法给用户减一瓶水了。这就是事务之间没有隔离性的问题

可以看见saga模式没有隔离性的影响还是较大，可以参照华为的解决方案:从业务层面入手加入一 Session 以及锁的机制来保证能够串行化操作资源。也可以在业务层面通过预先冻结资金的方式隔离这部分资源， 最后在业务操作的过程中可以通过及时读取当前状态的方式获取到最新的更新。

具体实例:可以参考华为的servicecomb

# 最后
还是那句话，能不用分布式事务就不用，如果非得使用的话，结合自己的业务分析，看看自己的业务比较适合哪一种，是在乎强一致，还是最终一致即可。上面对解决方案只是一些简单介绍，如果真正的想要落地，其实每种方案需要思考的地方都非常多，复杂度都比较大，所以最后再次提醒一定要判断好是否使用分布式事务。最后在总结一些问题,大家可以下来自己从文章找寻答案:

1. ACID和CAP的 CA是一样的吗？
2. 分布式事务常用的解决方案的优缺点是什么？适用于什么场景？
3. 分布式事务出现的原因？用来解决什么痛点？

如果上面问题有什么疑问的话可以关注公众号，来和我一起讨论吧.

# 1.分布式事务

在去年的时候我写过一篇关于分布式事务的文章[再有人问你分布式事务，把这篇扔给他](https://github.com/javagrowing/JGrowing/blob/master/%E5%88%86%E5%B8%83%E5%BC%8F/%E5%88%86%E5%B8%83%E5%BC%8F%E4%BA%8B%E5%8A%A1/%E5%86%8D%E6%9C%89%E4%BA%BA%E9%97%AE%E4%BD%A0%E5%88%86%E5%B8%83%E5%BC%8F%E4%BA%8B%E5%8A%A1%EF%BC%8C%E8%BF%99%E7%AF%87%E6%96%87%E7%AB%A0%E6%89%94%E7%BB%99%E4%BB%96.md)。再这篇文章中我叫大家能不用分布式事务就别用分布式事务，因为会引入很多的复杂度。当时说这个的时候其实还有一个原因，没有大厂的成熟开源解决方案，虽然再网上有很多开源的分布式事务框架，但是都不是太成熟，没有大量的业务验证。它不像其他的分布式中间件有大量的成熟的解决方案，比如分布式消息队列中间件:Apache Kafka,Apache RocketMQ,Apache Pulsar这三个均是Apache顶级项目；又比如分布式任务调度，也有很多的开源比如XXL-JOB,Elastic-Job都是有很多的公司在使用。

而我们的分布式事务框架，却一直没有一个经过大量业务验证的框架。通过我的一些了解，其实各大公司都是有自己的分布式事务的解决方案，但很多时候都和业务上强耦合了，不适于做一些通用的框架。但是阿里在今年年初的时候给了大家一个惊喜，Fescar开源！从而让大家再以后选择分布式事务的时候多了一个选择方案，而且是经过成熟业务验证的方案。

这篇文章我会带大家认识Fescar，以及他的一些设计。后续的文章还会陆续推出Fescar的核心源码解析篇。

# 2.初识Fescar

说Fescar之前这里先简单的介绍一下著名的2PC:XA Transactions。
在XA协议中分为两阶段:
![](https://raw.githubusercontent.com/lindage1994/images/master/typora202010/17/200324-405559.jpeg)
第一阶段：事务管理器要求每个涉及到事务的数据库预提交(precommit)此操作，并反映是否可以提交.

第二阶段：事务协调器要求每个数据库提交数据，或者回滚数据。

优点： 尽量保证了数据的强一致，实现成本较低，在各大主流数据库都有自己实现，对于MySQL是从5.5开始支持。

缺点: 

- 单点问题:事务管理器在整个流程中扮演的角色很关键，如果其宕机，比如在第一阶段已经完成，在第二阶段正准备提交的时候事务管理器宕机，资源管理器就会一直阻塞，导致数据库无法使用。
- 同步阻塞:在准备就绪之后，资源管理器中的资源一直处于阻塞，直到提交完成，释放资源。
- 数据不一致:两阶段提交协议虽然为分布式数据强一致性所设计，但仍然存在数据不一致性的可能，比如在第二阶段中，假设协调者发出了事务commit的通知，但是因为网络问题该通知仅被一部分参与者所收到并执行了commit操作，其余的参与者则因为没有收到通知一直处于阻塞状态，这时候就产生了数据的不一致性。
- 只能用于单一类型数据库，跨数据库种类，其他缓存或者文件等资源不支持。

总的来说，XA协议比较简单，成本较低，但是其单点问题，以及不能支持高并发(由于同步阻塞)依然是其最大的弱点。

可以看见XA协议问题较多所以其作为分布式事务方案讨论的时候基本都会被无情的干掉，但是XA有个很好优点是对业务没有侵入(数据库层面)，再业务上不需要编写额外的代码，更加关注业务。

在我的那篇文章中我又介绍了几种在应用层面上的分布式事务，但是或多或少的对业务都有一定的入侵。比如我们的TCC,常用的一些TCC框架都需要编写try,confirm,cancel等接口对于现成的业务需要进行转换。如果采用本地消息表模式那么又需要增加额外的表。如果采用事务性消息比如RocketMQ,会让一些没有使用该消息队列的业务需要更换消息队列。如果采用Saga模式，同样我们需要编写正向和反向接口。可以看见不论采用哪种分布式事务方案，都会有一定的业务改造，业务入侵成本。

而Fescar结合了XA的无侵入的优点和其他应用层事务协议高性能的优点，在应用层实现了二阶段协议的事务，同时对业务代码基本无侵入。

# 2.1 Fescar和XA

Fescar虽然是二阶段提交协议的分布式事务，但是其解决了上面XA的一些缺点:

- 单点问题:虽然目前Fescar(0.4.1)还是单server的，但是Fescar官方预计将会在0.5.x中推出HA-Cluster，到时候就可以解决单点问题。
- 同步阻塞:Fescar的二阶段，其再第一阶段的时候本地事务就已经提交释放资源了，不会像XA会再两个prepare和commit阶段资源都锁住，并且Fescar,commit是异步操作，也是提升性能的一大关键。
- 数据不一致:如果出现部分commit失败，那么fescar-server会根据当前的事务模式和分支事务的返回状态的结果来进行不同的重试策略。并且fescar的本地事务会在一阶段的时候进行提交，其实单看数据库来说在commit的时候数据库已经是一致的了。
- 只能用于单一数据库: Fescar提供了两种模式，AT和MT。在AT模式下事务资源可以是任何支持ACID的数据库，在MT模式下事务资源没有限制，可以是缓存，可以是文件，可以是其他的等等。当然这两个模式也可以混用。

同时Fescar也保留了接近0业务入侵的优点，只需要简单的配置Fescar的数据代理和加个注解，加一个Undolog表，就可以达到我们想要的目的。

# 3.Fescar总体设计

Fescar的设计核心就是他的角色分类。不论是数据库上的XA还是Fescar都有两个角色TM(事务管理器)和RM(资源管理器)，同时Fescar还有一个TC(事务协调器)。我们先来看看如果没有TC，只有TM和RM会发生什么呢？这里我举个简单的例子，小明去网站上面购买了一个商品，如下图所示：

![](https://raw.githubusercontent.com/lindage1994/images/master/typora202010/17/200341-42525.png)

这里小明其实就是TM(事务管理器)，而商品和账户其实就是我们的RM(资源管理器)，正常情况下可能没问什么问题，账户和库存都能扣减成功。如果小明再扣减库存的时候成功但是在扣减账户的时候失败，这个时候就需要对我们的库存资源进行回滚。小明这个时候就会通知库存把上个阶段扣减的货物补回来。但是回滚库存的时候库存服务不稳定，这次回滚就失败了。一般来说小明会不断的去重试，直到成功。这样就有个问题小明就一直被阻塞，不能做任何事。这个也可以看做二阶段commit/rollback的时候一直会阻塞TM，网易DDB的XA协议针对这种情况会做一个异步线程的操作。但是在Fescar中一切都是由TC去做的，当然TC其实不仅仅会做二阶段失败的重试，他会做二阶段的所有RM的commit和rollback，让我们的TM做更少的事。

再Fescar中TM，RM，TC的关系如下面官方提供的图:


![](https://raw.githubusercontent.com/lindage1994/images/master/typora202010/17/200359-959116.png)

- TM:事务的发起者。用来告诉TC，全局事务的开始，提交，回滚。
- RM:具体的事务资源，每一个RM都会作为一个分支事务注册在TC。
- TC:事务的协调者。也可以看做是Fescar-servr，用于接收我们的事务的注册，提交和回滚。

这三个角色的分工明确，正是我们Fescar真正的核心所在，下面我会通过如何使用Fescar带大家更加深刻的理解这三个角色。

# 4.快速开始Fescar

在Fescar的github上已经提供了一个简单的例子https://github.com/fescar-group/fescar-samples，这里我们需要将这个例子下载下来。

## 4.1搭建TC Fescar-server

首先我们搭建事务协调器，也可以叫做搭建Fescar-Server.官网的例子是使用Nacos加上fescar-server已经打好的Jar包运行的。这里为了方便我们直接下载fescar的代码https://github.com/alibaba/fescar。

找到我们的Server：

![](https://raw.githubusercontent.com/lindage1994/images/master/typora202010/17/200441-564680.png)

直接运行main方法，该方法会帮助我们在本地启动一个端口号为8091的fescar-server服务。如果我们想要进行服务注册，我们可以修改registry.conf下面的type

![](https://raw.githubusercontent.com/lindage1994/images/master/typora202010/17/200455-938788.png)

可以看见在0.4.1版本的时候支持四种服务注册，nacos,eureka,redis,zk。目前使用redis进行服务注册是有问题的，我也提了一个PR给官方进行修正。当然为了方便其实选择file，后续我们直连是最为便捷的。

再运行main方法之后，如果出现Server started日志，就代表我们的TC(事务协调器)成功启动。

![](https://raw.githubusercontent.com/lindage1994/images/master/typora202010/17/200509-801998.png)

## 4.2 认识TM

上面事务协调器已经搭建完成，我们接下来需要做的就是将TM和RM运行起来，将对应的操作交给我们的事务协调器去做。这个时候我们需要打开fescar-samples这个项目:由于这个项目使用的RPC是Dubbo,他默认配的服务注册中心是Nacos,需要我们再本地安装一个Nacos，具体安装可以自行搜索，这里不展开讲了。

这里官方例子中，业务关系如下图:
![](https://raw.githubusercontent.com/lindage1994/images/master/typora202010/17/200522-431222.png)

可以看见Business也就是我们的TM，找到对应的代码:

![](https://raw.githubusercontent.com/lindage1994/images/master/typora202010/17/200537-628602.png)

从代码中我们知道启动一个分布式事务是需要添加@GlobalTransactional注解的，当然Fescar也提供了API的方式让我们达到同样的效果。我们同时也需要修改registry.conf中的Type为file。

如果不是在例子中，而是在我们真正的业务上完全不需要修改业务代码，直接在我们分布式事务发起方添加上这个注解即可。

这个GloablTranscational注解到底做了什么呢？其实加了这个注解的都会走一个叫GlobalTransactionalInterceptor的切面，再这个切面中又会进入TrascationTemplate这个类中的excute方法，这个也是TM的核心方法:


![](https://raw.githubusercontent.com/lindage1994/images/master/typora202010/17/200552-383791.png)

上面的代码有部分删减，只选取了核心的流程。TrascationTemplate其实也是Fescar提供给我们的API，如果不使用注解那么我们也可以模仿他的方式去做。可以看见主要分为五步:

1. 获取当前上下文中是否已经有事务，这一步是通过ThreadLocal去实现的，如果有则获取当前的，如果没有则获取默认的。
2. 开启事务，这一步是向TC(事务协调器)发出一个请求，注册一个GloabTranscation，这里的timeout是指超过这段时间没有rollback或者commit,TC会帮助我们做rollback。
3. 做业务。
4. 如果该方法抛出了异常，则回滚。这里要注意的时候抛出异常，很多时候我们会把异常给捕获，导致我们这里根本没有异常抛出，所以就不会出现回滚。这里的回滚也是向TC发起一个回滚请求，由他帮助我们对RM进行回滚。
5. 如果没有异常则向TC发起commit,TC会帮助我们向RM异步发起提交请求。

TM的核心过程主要是这5步，其他详细的讲解会在后续的代码中体现。

## 4.3 认识RM

当我们上面的Business发起业务请求之后，就来到了我们RM的流程，我们的Storage和Order服务是怎么知道现在已经是处于分布式事务当中了呢？这个就需要借助RPC框架来完成了，这里我们使用的是Dubbo,fescar为dubbo提供了一个filter,如下图所示：

![](https://raw.githubusercontent.com/lindage1994/images/master/typora202010/17/200702-726654.png)

这里会从rpcContext中获取我们的xid，也就是我们的分布式事务ID,如果有的话就证明本次请求处于分布式事务中，那么就会把XID种入我们的RootContext(fescar的本地上下文)。如果你不是Dubbo，那么也可以根据此方法适配你的RPC。

在RM中我们应该做什么呢？只需要做下面两步:

1. 将数据源换成Fescar代理
   ![](https://raw.githubusercontent.com/lindage1994/images/master/typora202010/17/200717-699714.png)
2. 在当前数据库中添加一个Undolog的表，用于记录日志回滚。

再Fescar中不仅仅是对dataSource进行代理，也会对connection和statement进行代理，如下图:
![](https://raw.githubusercontent.com/lindage1994/images/master/typora202010/17/200734-512327.png)

大家都知道我们的SQL的具体执行需要依赖Statement,在Fescar的StatementProxy中有如下代码:

![](https://raw.githubusercontent.com/lindage1994/images/master/typora202010/17/200758-597882.png)

可以看见运行的方法是ExecuteTemplate.execute,在execute方法中会根据我们执行语句的类型记录我们的Undolog，具体的执行流程参考下面官方的一张图片:

![](https://raw.githubusercontent.com/lindage1994/images/master/typora202010/17/200818-821299.png)

总的来说我们的RM核心流程主要有两个：一个是如何识别分布式事务，另外一个是通过我们数据源代理让我们原本简单的执行SQL流程做了更多的事。

# 5.总结

写这篇文章的目的，不仅仅是让大家知道Fescar,也是让更多的人知道一个优秀分布式事务框架到底应该怎样去做。目前Fescar的版本号是0.4.1，还有很多功能比如HA-Cluster,SpringCloud集成还没有发布。所以目前再线上使用的话可能会遇到Fescar单点的问题，所以目前还不是太推荐线上使用。Fescar的目前规划会在0.5.x版本推出HA-Cluster，到时候其单点问题就会被解决。

这篇文章的原理目前介绍的比较粗浅，后面会陆续推出三篇文章详细介绍分析:TC,TM,RM，敬请期待。



# 参考文章

阿里开源分布式事务解决方案Fescar: https://mp.weixin.qq.com/s/TFGRcHV6EgeLB45OEJPRXw

# 1.背景

在之前的文章中已经介绍过Seata的总体介绍，如何使用以及Seata-Server的原理分析，有兴趣的可以阅读下面的文章：

- [深度剖析一站式分布式事务方案Seata-Server](https://mp.weixin.qq.com/s/2GwhFt-Q6Q0LmcmXBNJYww)
- [解密分布式事务框架-Fescar](https://mp.weixin.qq.com/s/g4OUkie972EqrW0gbCyEAQ)

这篇文章会介绍Seata中另外两个重要的角色`TM`(事务管理器)和`RM`(资源管理器)，首先还是来看看下面这张图:

![](https://raw.githubusercontent.com/lindage1994/images/master/typora202010/17/200921-747450.png)
上一个文章对于`TC`的原理已经做了详细介绍，对于TM和RM我们看见在图中都是属于`client`的角色,他们分别的功能如下:

- `TM`(事务管理器):用来控制整个分布式事务的管理，发起全局事务的`Begin/Commit/Rollback`。
- `RM(资源管理器)`:用来注册自己的分支事务，接受`TC`的`Commit`或者`Rollback`请求.

# 2.Seata-Spring

首先我们来介绍一些`Seata-client`中`Spring`模块，`Seata`通过这个模块对自己的`TM`和`RM`进行初始化以及扫描AT模式和TCC模式的注解并初始化这些模式需要的资源。
在`Seata`的项目中有一个`spring`模块,里面包含了我们和`spring`相关的逻辑,`GlobalTransactionScanner`是其中的核心类:

```java
public class GlobalTransactionScanner extends AbstractAutoProxyCreator implements InitializingBean,ApplicationContextAware,
        DisposableBean
```

上面代码是类的定义，首先它继承了`AbstractAutoProxyCreator`实现了`wrapIfNecessary`方法实现我们的方法的切面代理，实现了`InitializingBean`接口用于初始化我们的客户端，实现了`ApplicationContextAware`用于保存我们的`spring`容器，实现了`DisposableBean`用于优雅关闭。

首先来看继承AbstractAutoProxyCreator实现的wrapIfNecessary

```java
    protected Object wrapIfNecessary(Object bean, String beanName, Object cacheKey) {
        if (PROXYED_SET.contains(beanName)) {
            return bean;
        }
        interceptor = null;
        //check TCC proxy
        if (TCCBeanParserUtils.isTccAutoProxy(bean, beanName, applicationContext)) {
            //TCC interceptor， proxy bean of sofa:reference/dubbo:reference, and LocalTCC
            interceptor = new TccActionInterceptor(TCCBeanParserUtils.getRemotingDesc(beanName));
        } else {
            Class<?> serviceInterface = SpringProxyUtils.findTargetClass(bean);
            Class<?>[] interfacesIfJdk = SpringProxyUtils.findInterfaces(bean);
            if (!existsAnnotation(new Class[]{serviceInterface})
                    && !existsAnnotation(interfacesIfJdk)) {
                return bean;
            }
            if (interceptor == null) {
                interceptor = new GlobalTransactionalInterceptor(failureHandlerHook);
            }
        }
        if (!AopUtils.isAopProxy(bean)) {
            bean = super.wrapIfNecessary(bean, beanName, cacheKey);
        } else {
            AdvisedSupport advised = SpringProxyUtils.getAdvisedSupport(bean);
            Advisor[] advisor = buildAdvisors(beanName, getAdvicesAndAdvisorsForBean(null, null, null));
            for (Advisor avr : advisor) {
                advised.addAdvisor(0, avr);
            }
        }
        PROXYED_SET.add(beanName);   
        return bean;
    }
```

- Step1：检查当前`beanName`是否已经处理过 如果处理过本次就不处理。
- Step2：根据注解，找到对应模式的`Inteceptor`，这里有三种情况第一个`TCC`，第二个是全局事务管理TM的拦截器，第三个是没有注解，如果没有那么直接返回即可。
- Step3：将对应的`interceptor`添加进入当前`Bean`。

然后再看从`InitializingBean`中实现的`afterPropertiesSet`，也就是对`Seata`的初始化：

```java
    public void afterPropertiesSet() {
        initClient();

    }
    private void initClient() {
     
        //init TM
        TMClient.init(applicationId, txServiceGroup);
        //init RM
        RMClient.init(applicationId, txServiceGroup);
        registerSpringShutdownHook();
    }
    private void registerSpringShutdownHook() {
        if (applicationContext instanceof ConfigurableApplicationContext) {
            ((ConfigurableApplicationContext) applicationContext).registerShutdownHook();
            ShutdownHook.removeRuntimeShutdownHook();
        }
        ShutdownHook.getInstance().addDisposable(TmRpcClient.getInstance(applicationId, txServiceGroup));
        ShutdownHook.getInstance().addDisposable(RmRpcClient.getInstance(applicationId, txServiceGroup));
    }    
```

上面的代码逻辑比较清楚:

- Step1：初始化`TM`客户端，这里会向`Server`注册该`TM`。
- Step2：初始化`RM`客户端，这里会向Server注册该`RM`。
- Step3：注册`ShutdownHook`，后续将`TM`和`RM`优雅关闭。

注意这里初始化的时候会初始化两个客户端，分别是`TM`客户端和`RM`客户端，很多人认为`TM`和`RM`是用的同一个客户端，这里需要注意一下。

## 2.1 Interceptor

再上面的第一部分逻辑中我们看到我们有两个业务核心`Interceptor`,一个是`GlobalTransactionalInterceptor`用来处理全局事务的管理（开启，提交，回滚），另外一个是`TccActionInterceptor`用来处理TCC模式。熟悉Seata的朋友会问AT模式呢，为什么只有TCC模式，这里AT模式代表着就是自动处理事务，我们不需要有切面

#### 2.1.1 GlobalTransactionalInterceptor

首先来看看GlobalTransactionalInterceptor#invoke：

```java
    public Object invoke(final MethodInvocation methodInvocation) throws Throwable {
        Class<?> targetClass = (methodInvocation.getThis() != null ? AopUtils.getTargetClass(methodInvocation.getThis()) : null);
        Method specificMethod = ClassUtils.getMostSpecificMethod(methodInvocation.getMethod(), targetClass);
        final Method method = BridgeMethodResolver.findBridgedMethod(specificMethod);

        final GlobalTransactional globalTransactionalAnnotation = getAnnotation(method, GlobalTransactional.class);
        final GlobalLock globalLockAnnotation = getAnnotation(method, GlobalLock.class);
        if (globalTransactionalAnnotation != null) {
            return handleGlobalTransaction(methodInvocation, globalTransactionalAnnotation);
        } else if (globalLockAnnotation != null) {
            return handleGlobalLock(methodInvocation);
        } else {
            return methodInvocation.proceed();
        }
    }
```

- Step1：从代理类中获取到原始的`Method`
- Step2: 获取`Method`中的注解
- Step3: 如果有`@GlobalTransactional`注解执行handleGlobalTransaction切面逻辑，这个也是我们全局事务的逻辑。
- Step4: 如果有`@GlobalLock`注解，则执行handleGlobalLock切面逻辑，这个注解是用于一些非AT模式的数据库加锁，加上这个注解之后再执行Sql语句之前会查询对应的数据是否加锁，但是他不会加入全局事务。

`handleGlobalTransaction`逻辑如下：

```java
    private Object handleGlobalTransaction(final MethodInvocation methodInvocation,
                                           final GlobalTransactional globalTrxAnno) throws Throwable {

        return transactionalTemplate.execute(new TransactionalExecutor() {

            @Override
            public Object execute() throws Throwable {
                return methodInvocation.proceed();
            }

        });
    }
    TransactionalTemplate#execute
        public Object execute(TransactionalExecutor business) throws Throwable {
        // 1. get or create a transaction
        GlobalTransaction tx = GlobalTransactionContext.getCurrentOrCreate();
        // 1.1 get transactionInfo
        TransactionInfo txInfo = business.getTransactionInfo();
        if (txInfo == null) {
            throw new ShouldNeverHappenException("transactionInfo does not exist");
        }
        try {
            // 2. begin transaction
            beginTransaction(txInfo, tx);
            Object rs = null;
            try {
                // Do Your Business
                rs = business.execute();

            } catch (Throwable ex) {
                // 3.the needed business exception to rollback.
                completeTransactionAfterThrowing(txInfo,tx,ex);
                throw ex;
            }
            // 4. everything is fine, commit.
            commitTransaction(tx);
            return rs;
        } finally {
            //5. clear
            triggerAfterCompletion();
            cleanUp();
        }
    }

```

在`handleGlobalTransaction`中将具体的实现交给了`TransactionalTemplate#execute`去做了，其中具体的步骤如下:

- Step1：获取当前的全局事务，如果没有则创建。
- Step2：获取业务中的事务信息包含超时时间等。
- Step3：开启全局事务
- Step4：如果有异常抛出处理异常，rollback。
- Step5：如果没有异常那么commit全局事务。
- Step6：清除当前事务上下文信息。

#### 2.1.2 `TccActionInterceptor`

我们先看看TccActionInterceptor是如何使用:

```java
    @TwoPhaseBusinessAction(name = "TccActionOne" , commitMethod = "commit", rollbackMethod = "rollback")
    public boolean prepare(BusinessActionContext actionContext, int a);

    public boolean commit(BusinessActionContext actionContext);
    
    public boolean rollback(BusinessActionContext actionContext);
```

一般来说会定义三个方法一个是阶段的try方法，另外一个是二阶段的commit和rollback，每个方法的第一个参数是我们事务上下文，这里我们不需要关心他在我们切面中会自行填充处理。

接下来我们再看看TCC相关的拦截器是如何处理的：

```java
public Object invoke(final MethodInvocation invocation) throws Throwable {
		Method method = getActionInterfaceMethod(invocation);
		TwoPhaseBusinessAction businessAction = method.getAnnotation(TwoPhaseBusinessAction.class);	
		//try method
	    if(businessAction != null) {
			if(StringUtils.isBlank(RootContext.getXID())){
				//not in distribute transaction
				return invocation.proceed();
			}
	    	Object[] methodArgs = invocation.getArguments();
	    	//Handler the TCC Aspect
			Map<String, Object> ret = actionInterceptorHandler.proceed(method, methodArgs, businessAction, new Callback<Object>(){
				@Override
				public Object execute() throws Throwable {
					return invocation.proceed();
				}
	    	});
	    	//return the final result
	    	return ret.get(Constants.TCC_METHOD_RESULT);
	    }
		return invocation.proceed();
	}
```

- Step1：获取原始`Method`。
- Step2：判断是否再全局事务中，也就是整个逻辑服务最外层是否执行了`GlobalTransactionalInterceptor`。如果不再直接执行即可。
- Step3：执行`TCC`切面，核心逻辑在`actionInterceptorHandler#proceed`中。

再来看看`actionInterceptorHandler#proceed`这个方法:

```java
 public Map<String, Object> proceed(Method method, Object[] arguments, TwoPhaseBusinessAction businessAction, Callback<Object> targetCallback) throws Throwable {
		Map<String, Object> ret = new HashMap<String, Object>(16);
		
		//TCC name
        String actionName = businessAction.name();
        String xid = RootContext.getXID();
        BusinessActionContext actionContext = new BusinessActionContext();
        actionContext.setXid(xid);
        //set action anme
        actionContext.setActionName(actionName)

        //Creating Branch Record
        String branchId = doTccActionLogStore(method, arguments, businessAction, actionContext);
        actionContext.setBranchId(branchId);
        
        //set the parameter whose type is BusinessActionContext
        Class<?>[] types = method.getParameterTypes();
        int argIndex = 0;
        for (Class<?> cls : types) {
            if (cls.getName().equals(BusinessActionContext.class.getName())) {
            	arguments[argIndex] = actionContext;
                break;
            }
            argIndex++;
        }
        //the final parameters of the try method
        ret.put(Constants.TCC_METHOD_ARGUMENTS, arguments);
        //the final result
        ret.put(Constants.TCC_METHOD_RESULT, targetCallback.execute());
        return ret;
	}

```

- Step1：获取一些事务信息，比如`TCC`名字，本次事务`XID`等。
- Step2：创建`Branch`事务，一个是在本地的`context`上下文中将它的`commit`和`rollback`信息保存起来，另一个是向我们的`Seata-Server`注册分支事务，用于后续的管理。
- Step3：填充方法参数，也就是我们的`BusinessActionContext`。

## 2.2 小结

Spring的几个总要的内容已经剖析完毕，核心类主要是三个，一个`Scanner`，两个`Interceptor`。整体来说比较简单，Spring做的基本上也是我们客户端一些初始化的事，接下来我们深入了解一下TM这个角色。

# 3. TM 事务管理器

在上面章节中我们讲了`GlobalTransactionalInterceptor`这个切面拦截器，我们知道了这个拦截器中做了我们TM应该做的事，事务的开启，事务的提交，事务的回滚。这里只是我们整体逻辑的发起点，其中具体的客户端逻辑在我们的DefaultTransactionManager中，这个类中的代码如下所示：

```java
public class DefaultTransactionManager implements TransactionManager {

    @Override
    public String begin(String applicationId, String transactionServiceGroup, String name, int timeout)
        throws TransactionException {
        GlobalBeginRequest request = new GlobalBeginRequest();
        request.setTransactionName(name);
        request.setTimeout(timeout);
        GlobalBeginResponse response = (GlobalBeginResponse)syncCall(request);
        return response.getXid();
    }

    @Override
    public GlobalStatus commit(String xid) throws TransactionException {
        GlobalCommitRequest globalCommit = new GlobalCommitRequest();
        globalCommit.setXid(xid);
        GlobalCommitResponse response = (GlobalCommitResponse)syncCall(globalCommit);
        return response.getGlobalStatus();
    }

    @Override
    public GlobalStatus rollback(String xid) throws TransactionException {
        GlobalRollbackRequest globalRollback = new GlobalRollbackRequest();
        globalRollback.setXid(xid);
        GlobalRollbackResponse response = (GlobalRollbackResponse)syncCall(globalRollback);
        return response.getGlobalStatus();
    }

    @Override
    public GlobalStatus getStatus(String xid) throws TransactionException {
        GlobalStatusRequest queryGlobalStatus = new GlobalStatusRequest();
        queryGlobalStatus.setXid(xid);
        GlobalStatusResponse response = (GlobalStatusResponse)syncCall(queryGlobalStatus);
        return response.getGlobalStatus();
    }

    private AbstractTransactionResponse syncCall(AbstractTransactionRequest request) throws TransactionException {
        try {
            return (AbstractTransactionResponse)TmRpcClient.getInstance().sendMsgWithResponse(request);
        } catch (TimeoutException toe) {
            throw new TransactionException(TransactionExceptionCode.IO, toe);
        }
    }
}
```

在`DefaultTransactionManager`中整体逻辑比较简单有四个方法：

- `begin`：向`Server`发起`GlobalBeginRequest`请求，用于开启全局事务。
- `commit`：向`Server`发起`GlobalCommitRequest`请求，用于提交全局事务。
- `rollback`：向`Server`发起`GlobalRollbackRequest`请求，用于回滚全局事务。
- `getStatus`：向`Server`发起`GlobalStatusRequest`请求，用于查询全局事务状态信息。

# 4. RM 资源管理器

在`Seata`中目前管理`RM`有两种模式：一种是`AT`模式，需要事务性数据库支持，会自动记录修改前快照和修改后的快照，用于提交和回滚；还有一种是`TCC`模式，也可以看作是`MT`模式，用于AT模式不支持的情况，手动进行提交和回滚。接下来将会深入剖析一下这两种模式的实现原理。

## 4.1 AT 资源管理

`AT`模式下需要使用`Seata`提供的数据源代理，其整体实现逻辑如下图所示：

![](https://raw.githubusercontent.com/lindage1994/images/master/typora202010/17/201132-790199.png)

在我们的程序中执行一个`sql`语句，无论你是使用`mybatis`，还是直接使用`jdbcTemplate`,都会遵循下面的步骤：

- Step 1：从数据源中获取数据库连接。
- Step 2: 从连接中获取`Statement`。
- Step 3: 通过Statement执行我们的`sql`语句

所以我们可以将`DataSource`，`Connection`，`Statement`代理起来然后执行我们的一些特殊的逻辑，完成我们的AT模式。

#### 4.1.1 DataSourceProxy

在DataSourceProxy中没有太多的业务逻辑，只是简单的将获取`Connection`用我们的`ConnectionProxy`代理类进行了封装,代码如下：

```java
    public ConnectionProxy getConnection() throws SQLException {
        Connection targetConnection = targetDataSource.getConnection();
        return new ConnectionProxy(this, targetConnection);
    }
```

首先通过我们代理之前的`DataSource`获取连接，然后用`ConnectionProxy`将其代理起来。

#### 4.1.2 ConnectionProxy

`ConnectionProxy`主要做三件事，第一个是生成代理的`Statement`,第二个是保存我们的连接上下文：加锁的Key,undoLog等，第三个是代理执行我们的本地事务的`commit`和`rollback`。

首先来看看代理生成的`Statement`：

```java
    @Override
    public Statement createStatement() throws SQLException {
        Statement targetStatement = getTargetConnection().createStatement();
        return new StatementProxy(this, targetStatement);
    }

    @Override
    public PreparedStatement prepareStatement(String sql) throws SQLException {
        PreparedStatement targetPreparedStatement = getTargetConnection().prepareStatement(sql);
        return new PreparedStatementProxy(this, targetPreparedStatement, sql);
    }
```

这里也是通过我们原来的连接直接生成`Statement`，然后将其进行代理。


接下来看看对我们上下文的管理，大家都知道我们的一个事务其实对应的是一个数据库连接，在这个事务中的所有`sql`的`undolog`和`lockKey`都会在连接的上下文中记录。如下面代码所示：

```java
    /**
     * append sqlUndoLog
     *
     * @param sqlUndoLog the sql undo log
     */
    public void appendUndoLog(SQLUndoLog sqlUndoLog) {
        context.appendUndoItem(sqlUndoLog);
    }

    /**
     * append lockKey
     *
     * @param lockKey the lock key
     */
    public void appendLockKey(String lockKey) {
        context.appendLockKey(lockKey);
    }
```

这里的代码很简单，`lockKey`和`undolog`都是用`list`保存，直接`add`即可。

当我们的本地事务完成的时候，需要调用`Connection`的`commit`或`rollback`来进行事务的提交或回滚。这里我们也需要代理这两个方法来完成我们对分支事务的处理，先来看看`commit`方法。

```java
    public void commit() throws SQLException {
        if (context.inGlobalTransaction()) {
            processGlobalTransactionCommit();
        } else if (context.isGlobalLockRequire()) {
            processLocalCommitWithGlobalLocks();
        } else {
            targetConnection.commit();
        }
    }
    private void processGlobalTransactionCommit() throws SQLException {
        try {
            register();
        } catch (TransactionException e) {
            recognizeLockKeyConflictException(e);
        }
        try {
            if (context.hasUndoLog()) {
                UndoLogManager.flushUndoLogs(this);
            }
            targetConnection.commit();
        } catch (Throwable ex) {
            report(false);
            if (ex instanceof SQLException) {
                throw new SQLException(ex);
            }
        }
        report(true);
        context.reset();
    }
    
```

- Step 1：判断`context`是否再全局事务中，如果在则进行提交,到Step2。
- Step 2: 注册分支事务并加上全局锁，如果全局锁加锁失败则抛出异常。
- Step 3: 如果`context`中有`undolog`，那么将`Unlog`刷至数据库。
- Step 4: 提交本地事务。
- Step 5：报告本地事务状态，如果出现异常则报告失败，如果没有问题则报告正常。

上面介绍了提交事务的流程，当`context`在全局锁的流程中，会进行全局锁的查询，这里比较简单就不做赘述，如果`context`都没有在上述的情况中那么会直接进行事务提交。

对于我们`rollback`来说代码比较简单：

```java
    public void rollback() throws SQLException {
        targetConnection.rollback();
        if (context.inGlobalTransaction()) {
            if (context.isBranchRegistered()) {
                report(false);
            }
        }
        context.reset();
    }

```

- Step 1：首先提交本地事务。
- Step 2：判断是否在全局事务中。
- Step 3：如果在则判断分支事务是否已经注册。
- Step 4: 如果已经注册那么直接向客户端报告该事务失败异常。

> 细心的小伙伴可能发现如果我们的本地事务提交或者回滚之后失败，那我们的分布式事务运行结果还能正确吗？这里完全不用担心，再我们的服务端有完善的超时检测，重试等机制，来帮助我们应对这些特殊情况。

#### 4.1.3 StatementProxy

我们一般用`statement`会调用`executeXXX`方法来执行我们的`sql`语句，所以在我们的`Proxy`中可以利用这个方法，再执行`sql`的时候做一些我们需要做的逻辑，下面看看`execute`方法的代码：

```java
    public boolean execute(String sql) throws SQLException {
        this.targetSQL = sql;
        return ExecuteTemplate.execute(this, new StatementCallback<Boolean, T>() {
            @Override
            public Boolean execute(T statement, Object... args) throws SQLException {
                return statement.execute((String) args[0]);
            }
        }, sql);
    }
```

这里直接将逻辑交给我们的`ExecuteTemplate`去执行，有如下代码：

```java
    public static <T, S extends Statement> T execute(SQLRecognizer sqlRecognizer,
                                                     StatementProxy<S> statementProxy,
                                                     StatementCallback<T, S> statementCallback,
                                                     Object... args) throws SQLException {

        if (!RootContext.inGlobalTransaction() && !RootContext.requireGlobalLock()) {
            // Just work as original statement
            return statementCallback.execute(statementProxy.getTargetStatement(), args);
        }

        if (sqlRecognizer == null) {
            sqlRecognizer = SQLVisitorFactory.get(
                    statementProxy.getTargetSQL(),
                    statementProxy.getConnectionProxy().getDbType());
        }
        Executor<T> executor = null;
        if (sqlRecognizer == null) {
            executor = new PlainExecutor<T, S>(statementProxy, statementCallback);
        } else {
            switch (sqlRecognizer.getSQLType()) {
                case INSERT:
                    executor = new InsertExecutor<T, S>(statementProxy, statementCallback, sqlRecognizer);
                    break;
                case UPDATE:
                    executor = new UpdateExecutor<T, S>(statementProxy, statementCallback, sqlRecognizer);
                    break;
                case DELETE:
                    executor = new DeleteExecutor<T, S>(statementProxy, statementCallback, sqlRecognizer);
                    break;
                case SELECT_FOR_UPDATE:
                    executor = new SelectForUpdateExecutor<T, S>(statementProxy, statementCallback, sqlRecognizer);
                    break;
                default:
                    executor = new PlainExecutor<T, S>(statementProxy, statementCallback);
                    break;
            }
        }
        T rs = null;
        try {
            rs = executor.execute(args);
        } catch (Throwable ex) {
            if (!(ex instanceof SQLException)) {
                // Turn other exception into SQLException
                ex = new SQLException(ex);
            }
            throw (SQLException)ex;
        }
        return rs;
    }
}
```

这里是我们代理执行`sql`的核心逻辑，步骤如下：

- Step 1：如果不在全局事务且不需要查询全局锁，那么就直接执行原始的`Statement`。
- Step 2: 如果没有传入`sql`识别器，那么我们需要生成`sql`识别器，这里我们会借用Druid中对`sql`的解析，我们获取`sql`的识别器，我们通过这个识别器可以获取到不同类型的`sql`语句的一些条件，比如说`SQLUpdateRecognizer`是用于`update`的`sql`识别器，我们可以直接获取到表名，条件语句，更新的字段，更新字段的值等。
- Step 3：根据`sql`识别器的类型，来生成我们不同类型的执行器。
- Step 4：通过第三步中的执行器来执行我们的sql语句。

这里有五种`Executor`:`INSERT,UPDATE,DELETE`的执行器会进行undolog记录并且记录全局锁，`SELECT_FOR_UPDATE`只会进行查询全局锁，有一个默认的代表我们现在还不支持，什么都不会做直接执行我们的`sql`语句。

对于INSERT,UPDATE,DELETE的执行器会继承我们的`AbstractDMLBaseExecutor`：

```java
    protected T executeAutoCommitFalse(Object[] args) throws Throwable {
        TableRecords beforeImage = beforeImage();
        T result = statementCallback.execute(statementProxy.getTargetStatement(), args);
        TableRecords afterImage = afterImage(beforeImage);
        prepareUndoLog(beforeImage, afterImage);
        return result;
    }

    protected abstract TableRecords beforeImage() throws SQLException;


    protected abstract TableRecords afterImage(TableRecords beforeImage) throws SQLException;
    
```

在`AbstractDMLBaseExecutor`中执行逻辑在`executeAutoCommitFalse`这个方法，步骤如下：

- Step 1：获取执行当前`sql`之前所受影响行的快照，这里`beforeImage`会被不同类型的sql语句重新实现。
- Step 2：执行当前`sql`语句，并获取结果。
- Step 3：获取执行`sql`之后的快照，这里的`afterIamge`也会被不同类型的sql语句重新实现。
- Step 4：将`undolog`准备好，这里会保存到我们的`ConnectionContext`中。


```java
    protected void prepareUndoLog(TableRecords beforeImage, TableRecords afterImage) throws SQLException {
        if (beforeImage.getRows().size() == 0 && afterImage.getRows().size() == 0) {
            return;
        }

        ConnectionProxy connectionProxy = statementProxy.getConnectionProxy();

        TableRecords lockKeyRecords = sqlRecognizer.getSQLType() == SQLType.DELETE ? beforeImage : afterImage;
        String lockKeys = buildLockKey(lockKeyRecords);
        connectionProxy.appendLockKey(lockKeys);

        SQLUndoLog sqlUndoLog = buildUndoItem(beforeImage, afterImage);
        connectionProxy.appendUndoLog(sqlUndoLog);
    }
```

准备`UndoLog`的时候会获取我们的`ConnectionProxy`,将我们的`Undolog`和`LockKey`保存起来,给后面的本地事务`commit`和`rollback`使用，上面已经讲过。


#### 4.1.4 分支事务的提交和回滚

上面的4.1.1-4.1.3都是说的是我们分布式事务的第一阶段，也就是将我们的分支事务注册到`Server`,而第二阶段分支提交和分支回滚都在我们的`DataSourceManager`中，对于分支事务提交有如下代码：

```java
public BranchStatus branchCommit(BranchType branchType, String xid, long branchId, String resourceId, String applicationData) throws TransactionException {
        return asyncWorker.branchCommit(branchType, xid, branchId, resourceId, applicationData);
    }
public BranchStatus branchCommit(BranchType branchType, String xid, long branchId, String resourceId, String applicationData) throws TransactionException {
        if (!ASYNC_COMMIT_BUFFER.offer(new Phase2Context(branchType, xid, branchId, resourceId, applicationData))) {
            LOGGER.warn("Async commit buffer is FULL. Rejected branch [" + branchId + "/" + xid + "] will be handled by housekeeping later.");
        }
        return BranchStatus.PhaseTwo_Committed;
    }
```

这里将我们的分支事务提交的信息，放到一个队列中，异步去处理，也就是异步删除我们的`undolog`数据，因为提交之后`undolog`数据没用了。

这里有人可能会问如果当你将这个信息异步提交到队列中的时候，机器宕机，那么就不会执行异步删除`undolog`的逻辑，那么这条`undolog`是不是就会成为永久的脏数据呢？这里`Seata`为了防止这种事出现，会定时扫描某些较老的undolog数据然后进行删除，不会污染我们的数据。

对于我们的分支事务回滚有如下代码：

```java
    public BranchStatus branchRollback(BranchType branchType, String xid, long branchId, String resourceId, String applicationData) throws TransactionException {
        DataSourceProxy dataSourceProxy = get(resourceId);
        if (dataSourceProxy == null) {
            throw new ShouldNeverHappenException();
        }
        try {
            UndoLogManager.undo(dataSourceProxy, xid, branchId);
        } catch (TransactionException te) {
            if (te.getCode() == TransactionExceptionCode.BranchRollbackFailed_Unretriable) {
                return BranchStatus.PhaseTwo_RollbackFailed_Unretryable;
            } else {
                return BranchStatus.PhaseTwo_RollbackFailed_Retryable;
            }
        }
        return BranchStatus.PhaseTwo_Rollbacked;

    }
```

这里会先获取到我们的数据源，接下来调用我们的重做日志管理器的`undo`方法进行日志重做，`undo`方法较长这里就不贴上来了，其核心逻辑是查找到我们的`undolog`然后将里面的快照在我们数据库进行重做。

## 4.2 TCC 资源管理

`TCC`没有`AT`模式资源管理这么复杂，部分核心逻辑在之前的`Interceptor`中已经讲解过了，比如二阶段方法的保存等。这里主要看看`TCC`的分支事务提交和分支事务回滚，在`TCCResourceManager`中有：

```java
	public BranchStatus branchCommit(BranchType branchType, String xid, long branchId, String resourceId,
									 String applicationData) throws TransactionException {
		TCCResource tccResource = (TCCResource) tccResourceCache.get(resourceId);
		if (tccResource == null) {
			throw new ShouldNeverHappenException("TCC resource is not exist, resourceId:" + resourceId);
		}
		Object targetTCCBean = tccResource.getTargetBean();
		Method commitMethod = tccResource.getCommitMethod();
		if (targetTCCBean == null || commitMethod == null) {
			throw new ShouldNeverHappenException("TCC resource is not available, resourceId:" + resourceId);
		}
		boolean result = false;
		//BusinessActionContext
		BusinessActionContext businessActionContext =
				getBusinessActionContext(xid, branchId, resourceId, applicationData);
		Object ret = commitMethod.invoke(targetTCCBean, businessActionContext);
		LOGGER.info("TCC resource commit result :" + ret + ", xid:" + xid + ", branchId:" + branchId + ", resourceId:" +
				resourceId);
		if (ret != null && ret instanceof TwoPhaseResult) {
			result = ((TwoPhaseResult) ret).isSuccess();
		} else {
			result = (boolean) ret;
		}
		return result ? BranchStatus.PhaseTwo_Committed : BranchStatus.PhaseTwo_CommitFailed_Retryable;
	}
```

步骤如下：

- Step 1：首先查找当前服务是否有该`TCC`资源，如果没有抛出异常。
- Step 2：然后找到我们的TCC对象和对应的`commit`方法。
- Step 3：然后执行我们的`commit`方法。
- Step 4：最后将结果返回给我们的`Server`,由`Server`决定是否重试。

这里的`branchRollback`方法也比较简单，这里就不做过多分析了。

# 总结

通过上面分析我们知道，`Seata`的初始化是依赖`Spring`去进行，我们的全局事务的开启/提交/回滚都是依赖我们的TM事务管理器，而我们的分支事务的管理是依靠我们的`RM`，其中提供了两个模式`AT`和`TCC`，`AT`模式必须使用数据库，其核心实现是实现数据源的代理，将我们自己的逻辑注入进去。而我们的TCC能弥补我们没有使用数据库的情况，将提交和回滚都交由我们自己实现，其核心实现逻辑是依赖将一个资源的二阶段的方法和我们的目标对象在我们的资源上下文中保存下来，方便我们后续使用。

最后如果大家对分布式事务感兴趣，欢迎大家使用并阅读`Seata`的代码，并给我们提出建议。

> 如果大家觉得这篇文章对你有帮助，你的关注和转发是对我最大的支持，O(∩_∩)O:

# 1.关于Seata

再前不久，我写了一篇关于分布式事务中间件Fescar的解析，没过几天Fescar团队对其进行了品牌升级，取名为Seata(Simpe Extensible Autonomous Transcaction Architecture)，而以前的Fescar的英文全称为Fast & EaSy Commit And Rollback。可以看见Fescar从名字上来看更加局限于Commit和Rollback，而新的品牌名字Seata旨在打造一套一站式分布式事务解决方案。更换名字之后，我对其未来的发展更有信心。

这里先大概回忆一下Seata的整个过程模型:

![](https://raw.githubusercontent.com/lindage1994/images/master/typora202010/17/201428-709711.png)

- TM:事务的发起者。用来告诉TC，全局事务的开始，提交，回滚。
- RM:具体的事务资源，每一个RM都会作为一个分支事务注册在TC。
- TC:事务的协调者。也可以看做是Fescar-servr，用于接收我们的事务的注册，提交和回滚。

在之前的文章中对整个角色有个大体的介绍，在这篇文章中我将重点介绍其中的核心角色TC，也就是事务协调器。

# 2.Transcation Coordinator

为什么之前一直强调TC是核心呢？那因为TC这个角色就好像上帝一样，管控着云云众生的RM和TM。如果TC一旦不好使，那么RM和TM一旦出现小问题，那必定会乱的一塌糊涂。所以要想了解Seata，那么必须要了解他的TC。

那么一个优秀的事务协调者应该具备哪些能力呢？我觉得应该有以下几个:

- 正确的协调：能正确的协调RM和TM接下来应该做什么，做错了应该怎么办，做对了应该怎么办。
- 高可用: 事务协调器在分布式事务中很重要，如果不能保证高可用，那么他也没有存在的必要了。
- 高性能：事务协调器的性能一定要高，如果事务协调器性能有瓶颈那么他所管理的RM和TM那么会经常遇到超时，从而引起回滚频繁。
- 高扩展性：这个特点是属于代码层面的，如果是一个优秀的框架，那么需要给使用方很多自定义扩展，比如服务注册/发现，读取配置等等。

下面我也将逐步阐述Seata是如何做到上面四点。

## 2.1 Seata-Server的设计


![](https://raw.githubusercontent.com/lindage1994/images/master/typora202010/17/201444-604693.png)

Seata-Server整体的模块图如上所示:

- Coordinator Core: 在最下面的模块是事务协调器核心代码，主要用来处理事务协调的逻辑，如是否commit,rollback等协调活动。
- Store:存储模块，用来将我们的数据持久化，防止重启或者宕机数据丢失。
- Discover: 服务注册/发现模块，用于将Server地址暴露给我们Client。
- Config: 用来存储和查找我们服务端的配置。
- Lock: 锁模块，用于给Seata提供全局锁的功能。
- Rpc:用于和其他端通信。
- HA-Cluster:高可用集群，目前还没开源。为Seata提供可靠的高可用功能。

## 2.2 Discover

首先来讲讲比较基础的Discover模块，又称服务注册/发现模块。我们将Seata-Sever启动之后，需要将自己的地址暴露给其他使用者，那么就需要我们这个模块帮忙。

![](https://raw.githubusercontent.com/lindage1994/images/master/typora202010/17/201458-886622.png)
这个模块有个核心接口RegistryService，如上图所示:

- register：服务端使用，进行服务注册。
- unregister：服务端使用，一般在JVM关闭钩子，ShutdownHook中调用。
- subscribe：客户端使用，注册监听事件，用来监听地址的变化。
- unsubscribe：客户端使用，取消注册监听事件。
- looup：客户端使用，根据key查找服务地址列表。
- close：都可以使用，用于关闭Register资源。

如果需要添加自己定义的服务注册/发现，那么实现这个接口即可。截止目前在社区的不断开发推动下，已经有四种服务注册/发现，分别是redis,zk,nacos,eruka。下面简单介绍下Nacos的实现：

#### 2.2.1 register接口：

![](https://raw.githubusercontent.com/lindage1994/images/master/typora202010/17/201517-754496.png)

step1:校验地址是否合法

step2:获取Nacos的Name实例，然后将地址注册到当前Cluster名称上面。

unregister接口类似，这里不做详解。

#### 2.2.2 lookup接口：

![](https://raw.githubusercontent.com/lindage1994/images/master/typora202010/17/201528-964378.png)

step1：获取当前clusterName名字

step2：判断当前cluster是否已经获取过了，如果获取过就从map中取。

step3：从Nacos拿到地址数据，将其转换成我们所需要的。

step4：将我们事件变动的Listener注册到Nacos

#### 2.2.3 subscribe接口

![](https://raw.githubusercontent.com/lindage1994/images/master/typora202010/17/201542-159777.png)
这个接口比较简单，具体分两步:

step1：将clstuer和listener添加进map中。

step2：向Nacos注册。

## 2.3 Config

配置模块也是一个比较基础，比较简单的模块。我们需要配置一些常用的参数比如:Netty的select线程数量，work线程数量，session允许最大为多少等等，当然这些参数再Seata中都有自己的默认设置。

同样的在Seata中也提供了一个接口Configuration，用来自定义我们需要的获取配置的地方:

![](https://raw.githubusercontent.com/lindage1994/images/master/typora202010/17/201558-427382.png)

- getInt/Long/Boolean/Config()：通过dataId来获取对应的值。
- putConfig：用于添加配置。
- removeConfig：删除一个配置。
- add/remove/get ConfigListener：添加/删除/获取 配置监听器，一般用来监听配置的变更。

目前为止有四种方式获取Config：File(文件获取),Nacos,Apollo,ZK。再Seata中首先需要配置registry.conf，来配置conf的类型。实现conf比较简单这里就不深入分析。

## 2.4 Store

存储层的实现对于Seata是否高性能，是否可靠非常关键。
如果存储层没有实现好，那么如果发生宕机，在TC中正在进行分布式事务处理的数据将会被丢失，既然使用了分布式事务，那么其肯定不能容忍丢失。如果存储层实现好了，但是其性能有很大问题，RM可能会发生频繁回滚那么其完全无法应对高并发的场景。

在Seata中默认提供了文件方式的存储，下面我们定义我们存储的数据为Session，而我们的TM创造的全局事务数据叫GloabSession，RM创造的分支事务叫BranchSession，一个GloabSession可以拥有多个BranchSession。我们的目的就是要将这么多Session存储下来。

在FileTransactionStoreManager#writeSession代码中:

![](https://raw.githubusercontent.com/lindage1994/images/master/typora202010/17/201612-839515.png)

上面的代码主要分为下面几步：

- step1：生成一个TransactionWriteFuture。
- step2：将这个futureRequest丢进一个LinkedBlockingQueue中。为什么需要将所有数据都丢进队列中呢？当然这里其实也可以用锁来实现，再另外一个阿里开源的RocketMQ中，使用的锁。不论是队列还是锁他们的目的是为了保证单线程写，这又是为什么呢？有人会解释说，需要保证顺序写，这样速度就很快，这个理解是错误的，我们的FileChannel其实是线程安全的，已经能保证顺序写了。保证单线程写其实是为了让我们这个写逻辑都是单线程的，因为可能有些文件写满或者记录写数据位置等等逻辑，当然这些逻辑都可以主动加锁去做，但是为了实现简单方便，直接再整个写逻辑加锁是最为合适的。
- step3：调用future.get，等待我们该条数据写逻辑完成通知。

我们将数据提交到队列之后，我们接下来需要对其进行消费，代码如下：

![](https://raw.githubusercontent.com/lindage1994/images/master/typora202010/17/201651-413238.png)

这里将一个WriteDataFileRunnable()提交进我们的线程池，这个Runnable的run()方法如下:

![](https://raw.githubusercontent.com/lindage1994/images/master/typora202010/17/201705-692180.png)
分为下面几步:

step1： 判断是否停止，如果stopping为true则返回null。

step2：从我们的队列中获取数据。

step3：判断future是否已经超时了，如果超时，则设置结果为false，此时我们生产者get()方法会接触阻塞。

step4：将我们的数据写进文件，此时数据还在pageCahce层并没有刷新到磁盘，如果写成功然后根据条件判断是否进行刷盘操作。

step5：当写入数量到达一定的时候，或者写入时间到达一定的时候，需要将我们当前的文件保存为历史文件，删除以前的历史文件，然后创建新的文件。这一步是为了防止我们文件无限增长，大量无效数据浪费磁盘资源。

在我们的writeDataFile中有如下代码:

![](https://raw.githubusercontent.com/lindage1994/images/master/typora202010/17/201719-596582.png)

step1：首先获取我们的ByteBuffer，如果超出最大循环BufferSize就直接创建一个新的，否则就使用我们缓存的Buffer。这一步可以很大的减少GC。

step2：然后将数据添加进入ByteBuffer。

step3：最后将ByteBuffer写入我们的fileChannel,这里会重试三次。此时的数据还在pageCache层，受两方面的影响，OS有自己的刷新策略，但是这个业务程序不能控制，为了防止宕机等事件出现造成大量数据丢失，所以就需要业务自己控制flush。下面是flush的代码:
![](https://raw.githubusercontent.com/lindage1994/images/master/typora202010/17/201807-107558.png)

这里flush的条件写入一定数量或者写的时间超过一定时间，这样也会有个小问题如果是停电，那么pageCache中有可能还有数据并没有被刷盘，会导致少量的数据丢失。目前还不支持同步模式，也就是每条数据都需要做刷盘操作，这样可以保证每条消息都落盘，但是性能也会受到极大的影响，当然后续会不断的演进支持。

我们的store核心流程主要是上面几个方法，当然还有一些比如，session重建等，这些比较简单，读者可以自行阅读。

## 2.5 Lock

大家知道数据库实现隔离级别主要是通过锁来实现的，同样的再分布式事务框架Seata中要实现隔离级别也需要通过锁。一般在数据库中数据库的隔离级别一共有四种:读未提交，读已提交，可重复读，串行化。在Seata中可以保证写的隔离级别是已提交，而读的隔离级别一般是未提交，但是提供了达到读已提交隔离的手段。

Lock模块也就是Seata实现隔离级别的核心模块。在Lock模块中提供了一个接口用于管理我们的锁:
![](https://raw.githubusercontent.com/lindage1994/images/master/typora202010/17/201821-468154.png)

其中有三个方法:

- acquireLock：用于对我们的BranchSession加锁，这里虽然是传的分支事务Session，实际上是对分支事务的资源加锁，成功返回true。
- isLockable：根据事务ID，资源Id，锁住的Key来查询是否已经加锁。
- cleanAllLocks：清除所有的锁。
  对于锁我们可以在本地实现，也可以通过redis或者mysql来帮助我们实现。官方默认提供了本地全局锁的实现：
  ![](https://raw.githubusercontent.com/lindage1994/images/master/typora202010/17/201847-200997.png)
  在本地锁的实现中有两个常量需要关注:
- BUCKET_PER_TABLE：用来定义每个table有多少个bucket，目的是为了后续对同一个表加锁的时候减少竞争。
- LOCK_MAP：这个map从定义上来看非常复杂，里里外外套了很多层Map，这里用个表格具体说明一下：

| 层数             | key                                            | value         |
| ---------------- | ---------------------------------------------- | ------------- |
| 1-LOCK_MAP       | resourceId（jdbcUrl）                          | dbLockMap     |
| 2- dbLockMap     | tableName （表名）                             | tableLockMap  |
| 3- tableLockMap  | PK.hashcode%Bucket （主键值的hashcode%bucket） | bucketLockMap |
| 4- bucketLockMap | PK                                             | trascationId  |

可以看见实际上的加锁在bucketLockMap这个map中，这里具体的加锁方法比较简单就不作详细阐述，主要是逐步的找到bucketLockMap,然后将当前trascationId塞进去，如果这个主键当前有TranscationId，那么比较是否是自己，如果不是则加锁失败。

## 2.6 Rpc

保证Seata高性能的关键之一也是使用了Netty作为RPC框架，采用默认配置的线程模型如下图所示：

![](https://raw.githubusercontent.com/lindage1994/images/master/typora202010/17/202144-700067.png)

如果采用默认的基本配置那么会有一个Acceptor线程用于处理客户端的链接，会有cpu*2数量的NIO-Thread，再这个线程中不会做业务太重的事情，只会做一些速度比较快的事情，比如编解码，心跳事件，和TM注册。一些比较费时间的业务操作将会交给业务线程池，默认情况下业务线程池配置为最小线程为100，最大为500。

这里需要提一下的是Seata的心跳机制，这里是使用Netty的IdleStateHandler完成的，如下:

![](https://raw.githubusercontent.com/lindage1994/images/master/typora202010/17/202158-512958.png)

在Sever端对于写没有设置最大空闲时间，对于读设置了最大空闲时间，默认为15s，如果超过15s则会将链接断开，关闭资源。


![](https://raw.githubusercontent.com/lindage1994/images/master/typora202010/17/202209-280391.png)

step1：判断是否是读空闲的检测事件。

step2：如果是则断开链接，关闭资源。

## 2.7 HA-Cluster

目前官方没有公布HA-Cluster,但是通过一些其他中间件和官方的一些透露，可以将HA-Cluster用如下方式设计:
![](https://raw.githubusercontent.com/lindage1994/images/master/typora202010/17/202226-538759.png)

具体的流程如下:

step1：客户端发布信息的时候根据transcationId保证同一个transcation是在同一个master上，通过多个Master水平扩展，提供并发处理性能。

step2：在server端中一个master有多个slave，master中的数据近实时同步到slave上，保证当master宕机的时候，还能有其他slave顶上来可以用。

当然上述一切都是猜测，具体的设计实现还得等0.5版本之后。目前有一个Go版本的Seata-Server也捐赠给了Seata(还在流程中)，其通过raft实现副本一致性，其他细节不是太清楚。

## 2.8 Metrics  

这个模块也是一个没有具体公布实现的模块，当然有可能会提供插件口，让其他第三方metric接入进来，最近Apache skywalking 正在和Seata小组商讨如何接入进来。

# 3.Coordinator Core

上面我们讲了很多Server基础模块，想必大家对Seata的实现已经有个大概，接下来我会讲解事务协调器具体逻辑是如何实现的，让大家更加了解Seata的实现内幕。

## 3.1 启动流程

启动方法在Server类有个main方法，定义了我们启动流程：

![](https://raw.githubusercontent.com/lindage1994/images/master/typora202010/17/202245-350526.png)

step1：创建一个RpcServer，再这个里面包含了我们网络的操作，用Netty实现了服务端。

step2：解析端口号和文件地址。

step3：初始化SessionHoler,其中最重要的重要就是重我们dataDir这个文件夹中恢复我们的数据，重建我们的Session。

step4：创建一个CoorDinator,这个也是我们事务协调器的逻辑核心代码，然后将其初始化，其内部初始化的逻辑会创建四个定时任务：

- retryRollbacking：重试rollback定时任务，用于将那些失败的rollback进行重试的，每隔5ms执行一次。
- retryCommitting：重试commit定时任务，用于将那些失败的commit进行重试的，每隔5ms执行一次。
- asyncCommitting：异步commit定时任务，用于执行异步的commit，每隔10ms一次。
- timeoutCheck：超时定时任务检测，用于检测超时的任务，然后执行超时的逻辑，每隔2ms执行一次。

step5： 初始化UUIDGenerator这个也是我们生成各种ID(transcationId,branchId)的基本类。

step6：将本地IP和监听端口设置到XID中，初始化rpcServer等待客户端的连接。

启动流程比较简单，下面我会介绍分布式事务框架中的常见的一些业务逻辑Seata是如何处理的。

## 3.2 Begin-开启全局事务

一次分布式事务的起始点一定是开启全局事务，首先我们看看全局事务Seata是如何实现的：

![](https://raw.githubusercontent.com/lindage1994/images/master/typora202010/17/202300-334887.png)

step1： 根据应用ID，事务分组，名字，超时时间创建一个GloabSession，这个再前面也提到过他和branchSession分别是什么。

step2：对其添加一个RootSessionManager用于监听一些事件，这里要说一下目前在Seata里面有四种类型的Listener(这里要说明的是所有的sessionManager都实现了SessionLifecycleListener)：

- ROOT_SESSION_MANAGER：最全，最大的，拥有所有的Session。
- ASYNC_COMMITTING_SESSION_MANAGER：用于管理需要做异步commit的Session。
- RETRY_COMMITTING_SESSION_MANAGER：用于管理重试commit的Session。
- RETRY_ROLLBACKING_SESSION_MANAGER：用于管理重试回滚的Session。
  由于这里是开启事务，其他SessionManager不需要关注，我们只添加RootSessionManager即可。

step3：开启Globalsession

![](https://raw.githubusercontent.com/lindage1994/images/master/typora202010/17/202326-440589.png)

这一步会把状态变为Begin,记录开始时间,并且调用RootSessionManager的onBegin监听方法，将Session保存到map并写入到我们的文件。

step4：最后返回XID，这个XID是由ip+port+transactionId组成的，非常重要，当TM申请到之后需要将这个ID传到RM中，RM通过XID来决定到底应该访问哪一台Server。

## 3.3 BranchRegister-分支事务注册

当我们全局事务在TM开启之后，我们RM的分支事务也需要注册到我们的全局事务之上，这里看看是如何处理的：

![](https://raw.githubusercontent.com/lindage1994/images/master/typora202010/17/202343-68937.png)

step1：通过transactionId获取并校验全局事务是否是开启状态。

step2：创建一个新的分支事务，也就是我们的BranchSession。

step3：对分支事务进行加全局锁，这里的逻辑就是使用的我们锁模块的逻辑。

step4：添加branchSession，主要是将其添加到globalSession对象中，并写入到我们的文件中。

step5：返回branchId,这个ID也很重要，我们后续需要用它来回滚我们的事务，或者对我们分支事务状态更新。

分支事务注册之后，还需要汇报分支事务的后续状态到底是成功还是失败，在Server目前只是简单的做一下保存记录，汇报的目的是，就算这个分支事务失败，如果TM还是执意要提交全局事务，那么再遍历提交分支事务的时候，这个失败的分支事务就不需要提交。

## 3.4 GlobalCommit - 全局提交

当我们分支事务执行完成之后，就轮到我们的TM-事务管理器来决定是提交还是回滚，如果是提交，那么就会走到下面的逻辑:


![](https://raw.githubusercontent.com/lindage1994/images/master/typora202010/17/202438-821600.png)

step1：首先找到我们的globalSession。如果他为Null证明已经被commit过了，那么直接幂等操作，返回成功。

step2：关闭我们的GloabSession防止再次有新的branch进来。

step3：如果status是等于Begin，那么久证明还没有提交过，改变其状态为Committing也就是正在提交。

step4：判断是否是可以异步提交，目前只有AT模式可以异步提交，因为是通过Undolog的方式去做的。MT和TCC都需要走同步提交的代码。

step5：如果是异步提交，直接将其放进我们ASYNC_COMMITTING_SESSION_MANAGER，让其再后台线程异步去做我们的step6，如果是同步的那么直接执行我们的step6。

step6：遍历我们的BranchSession进行提交，如果某个分支事务失败，根据不同的条件来判断是否进行重试，异步不需要重试，因为其本身都在manager中，只要没有成功就不会被删除会一直重试，如果是同步提交的会放进异步重试队列进行重试。

## 3.5 GlobalRollback - 全局回滚

如果我们的TM决定全局回滚，那么会走到下面的逻辑：

![](https://raw.githubusercontent.com/lindage1994/images/master/typora202010/17/202456-621965.png)

这个逻辑和提交流程基本一致，可以看作是他的反向，这里就不展开讲了。

# 4.总结

最后在总结一下开始我们提出了分布式事务的关键4点，Seata到底是怎么解决的：

- 正确的协调：通过后台定时任务各种正确的重试，并且未来会推出监控平台有可能可以手动回滚。
- 高可用: 通过HA-Cluster保证高可用。
- 高性能：文件顺序写，RPC通过netty实现，Seata未来可以水平扩展，提高处理性能。
- 高扩展性：提供给用户可以自由实现的地方，比如配置，服务发现和注册，全局锁等等。

最后希望大家能从这篇文章能了解Seata-Server的核心设计原理，当然你也可以想象如果你自己去实现一个分布式事务的Server应该怎样去设计？

seata github地址：https://github.com/seata/seata。

最后这篇文章被我收录于JGrowing-分布式事务篇，一个全面，优秀，由社区一起共建的Java学习路线，如果您想参与开源项目的维护，可以一起共建，github地址为:https://github.com/javagrowing/JGrowing 

