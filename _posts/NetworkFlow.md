<center><h3>Assignment 5  Network Flow</h3></center>

<center><h5>李哲舟 2018E8018661088</h5></center>

### Problem one : Load Balance

You have some different computers and jobs. For each job, it can only be done on one of two specified computers. The load of a computer is the number of jobs which have been done on the computer. Give the number of jobs and two computer ID for each job. You task is to minimize the max load. 

* 模型描述

  对于负载均衡问题，首先对其进行建模处理。将m个任务和和n个电脑抽象为节点$N_{t1}...N_{tm}$和$N_{c1}...N_{cn}$，将所有的任务节点和与其相应电脑节点相连，并将其容量设为1。设立虚拟节点S、T作为起始点和终点，将起始点与每个任务节点相连，容量为1；终点T与所有电脑节点相连，其容量统一设置为变量Load。

* 算法概括

  模型建立后，已知任务数量为TASKNUM，通过二分法，不断检测是否尝试存在更小的负载。

  解负载均衡问题伪代码如下：

```
parameter: 
	MinLoad        最小可行负载
	maxFlow        模型的最大流
findMinimiumLoad()
Load  = TASKNUM
high = TASKNUM
low = 0
while(high-low >= 1)
	figure_out_MaxmiumFlow
	if  (maxFlow = TASKNUM)
		MinLoad = Load
		high = Load
	else
		low = Load
		break
	end if
    Load = (high + low) / 2
end while
```

* 正确性验证

  将该问题转化为一个网络流问题，存在可行解即代表着系统满足约束，即可以找到在该负载约定下的分配方案。通过二分查找，不断尝试是否存在更优解，直到检索域用尽。

* 复杂度分析：

  在这里取网络时间复杂度为$O(n*n*m)$,故算法整体时间复杂度为：

​    								$O(log(m)(m+n+2)^2*(3m+n)*)$

### Problem Two: Matrix

For a matrix filled with 0 and 1, you know the sum of every row and column.
You are asked to give such a matrix which satisfys the conditions.

* 模型描述

​	设立起始点和终点S、T，并将每一行、每一列都抽象为虚拟节点$r_i$$c_i$;。

​	首先，将起始点与所有$r_i$相连，其容量大小为该行数字“1”的数量。

​	随后，将每个节点$r_i$都与所有$c_j$相连，容量皆为1。

​	最后，将所有$c_i$都与终点T连接，容量为该列数字“1”的数量。

* 算法概括

  解该模型的最大流，解出来的最大流如果为矩阵中1的数量，则说题目有解，将边节点和列节点相连的边的流量即为矩阵该行与该列相交位置的参数。

* 正确性验证

  解出来的最大流如果为矩阵中1的数量，则说题目有解，且连接$r_i$与$c_j$的边如果为1则说明$m[i][j]$为1，反之为0。

* 时间复杂度

  （对m行n列的矩阵）

  ​									$O((m+n)^2*(m+n+m*n))$

### Problem Three: Network Cost

For a network, there is one source and one sink. Every edge is directed and has two value c and a. c means the maximum flow of the adge. a is a coefficient number which means that if the flow of the edge is x, the cost is ax2 . 

Design an algorithm to get the Minimum Cost Maximum Flow. 

* 模型描述

  * 最大流模型

    与题干一致，并无变化。

  * 费用流模型

    因费用与流量大小成二次关系，传统费用流模型无法应用，故在此基础上做出修改。因网络流为整数问题，故对费用边进行划分：以一条容量为3，费用系数为c的边为例，其可划分为三条容量为1的边，费用系数分别为1c、3c、4c，即$(2^i-\sum_{n=0}^{i-1}2^n)c$。

* 算法概括

  与传统最小费用最大流算法类似，先计算最大流模型的最大流，随后再计算最大流模型下的最小费用问题。

* 时间复杂度

  （n为点，m为边）

  ​										$O(n*n*m)$