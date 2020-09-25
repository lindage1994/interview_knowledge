# 贪心算法

贪心算法（又称贪婪算法）是指，在对问题求解时，总是做出在当前看来是最好的选择。

也就是说，不从整体最优上加以考虑，他所做出的是在某种意义上的局部最优解。

贪心算法不是对所有问题都能得到整体最优解，关键是贪心策略的选择，选择的**贪心策略必须具备无后效性，即某个状态以前的过程不会影响以后的状态，只与当前状态有关。**

# 基本要素

## 贪心选择

贪心选择是指所求问题的整体最优解可以通过一系列局部最优的选择，即贪心选择来达到。

这是贪心算法可行的第一个基本要素，也是贪心算法与动态规划算法的主要区别。

贪心选择是采用从顶向下、以迭代的方法做出相继选择，每做一次贪心选择就将所求问题简化为一个规模更小的子问题。

对于一个具体问题，要确定它是否具有贪心选择的性质，我们必须证明每一步所作的贪心选择最终能得到问题的最优解。

通常可以首先证明问题的一个整体最优解，是从贪心选择开始的，而且作了贪心选择后，原问题简化为一个规模更小的类似子问题。

然后，用数学归纳法证明，通过每一步贪心选择，最终可得到问题的一个整体最优解。

## 最优子结构

当一个问题的最优解包含其子问题的最优解时，称此问题具有最优子结构性质。

运用贪心策略在每一次转化时都取得了最优解。

问题的最优子结构性质是该问题可用贪心算法或动态规划算法求解的关键特征。

贪心算法的每一次操作都对结果产生直接影响，而动态规划则不是。

**贪心算法对每个子问题的解决方案都做出选择，不能回退；动态规划则会根据以前的选择结果对当前进行选择，有回退功能。**

动态规划主要运用于二维或三维问题，而贪心一般是一维问题。

# 区间调度问题

假设有如下课程，希望尽可能多的将课程安排在一间教室里：

| 课程   | 开始时间 | 结束时间 |
| :----- | :------- | :------- |
| 美术   | 9AM      | 10AM     |
| 英语   | 9:30AM   | 10:30AM  |
| 数学   | 10AM     | 11AM     |
| 计算机 | 10:30AM  | 11:30AM  |
| 音乐   | 11AM     | 12PM     |

这个问题看似要思考很多，实际上算法很简单:

1.选择结束最早的课，便是要在这教室上课的第一节课

2.接下来，选择第一堂课结束后才开始的课，并且结束最早的课，这将是第二节在教室上的课。

重复这样做就能找出答案，这边的选择策略便是结束最早且和上一节课不冲突的课进行排序，因为每次都选择结束最早的，所以留给后面的时间也就越多，自然就能排下越多的课了。

每一节课的选择都是策略内的局部最优解(留给后面的时间最多)，所以最终的结果也是近似最优解(这个案例上就是最优解)。

# 背包问题

有一个背包，容量为35磅， 现有如下物品

| 物品       | 重量 | 价格 |
| :--------- | :--- | :--- |
| 吉他       | 15   | 1500 |
| 音响       | 30   | 3000 |
| 笔记本电脑 | 20   | 2000 |
| 显示器     | 29   | 2999 |
| 笔         | 1    | 200  |

要求达到的目标为装入的背包的总价值最大，并且重量不超出。

方便计算所以只有3个物品，实际情况可能是成千上万。

同上使用贪婪算法，因为要总价值最大，所以每次每次都装入最贵的,然后在装入下一个最贵的，选择结果如下：

选择: 音响 + 笔，总价值 3000 + 200 = 3200

并不是最优解: 吉他 + 笔记本电脑, 总价值 1500 + 2000 = 3500

当然选择策略有时候并不是很固定，可能是如下：

(1)每次挑选价值最大的,并且最终重量不超出：

选择: 音响 + 笔，总价值 3000 + 200 = 3200

(2)每次挑选重量最大的,并且最终重量不超出(可能如果要求装入最大的重量才会优先考虑)：

选择: 音响 + 笔，总价值 3000 + 200 = 3200

(3)每次挑选单位价值最大的(价格/重量),并且最终重量不超出：

选择: 笔+ 显示器，总价值 200 + 2999 = 3199

如上最终的结果并不是最优解，在这个案例中贪婪算法并无法得出最优解，只能得到近似最优解,也算是该算法的局限性之一。

该类问题中需要得到**全局最优解的话可以采取动态规划算法。**

# 参考资料

[百度百科-贪心算法](https://baike.baidu.com/item/贪心算法/5411800?fr=aladdin)

[那些经典算法：贪心算法](https://www.cnblogs.com/seaspring/p/11562304.html)

[五大常用算法：分治、动态规划、贪心、回溯和分支界定详解](https://www.douban.com/note/689405675/)

[算法(六):图解贪婪算法](https://blog.csdn.net/weixin_34179762/article/details/88015684)