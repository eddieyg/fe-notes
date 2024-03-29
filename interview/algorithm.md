# 08 算法


时间复杂度  

O(1)  常数复杂度  
O(n)  线性复杂度  
O(n^2) 平方复杂度  
O(2^n) 指数复杂度  
O(log n) 对数复杂度（二分查找）  
O(n log n) 线性对数复杂度  

 
数据结构   

逻辑结构：数据之间的关系，分为线性结构（有序排列）和非线性结构  
存储结构：顺序存储、链式存储、索引存储、散列存储  


树  
非线性结构，具有树状结构的数据集合  
二叉树：每个节点最多有左子树、右子树两个子节点（树的遍历、对称性）  


堆  
堆的底层实际上是一棵完全二叉树，可以用数组实现    
* 每个的节点元素值不小于其子节点 - 最大堆    
* 每个的节点元素值不大于其子节点 - 最小堆    


链表   
线性结构，一个对象存储着本身的值和其他对象的地址，需要遍历查询  
单向链表：当前对象存储下一个对象的地址  
双向链表：当前对象存储上一个和下一个对象的地址  


数组  
按照顺序存储的数据集合，可通过索引随机访问，插入或删除需移动后续的元素  


栈  
按照 先进后出 的顺序访问


队列  
按照 先进先出 的顺序访问


哈希表  
通过唯一的 键 存储、快速查询对应的地址值


图  
顶点之间通过边的连接关系的结构，根据边的有无方向分为有向图、无向图；边如果有权值 为带权图

【数据结构】数据结构-图的基本概念  https://www.cnblogs.com/songgj/p/9107797.html


￼


算法


思想

分治：将问题分为子问题去解，再把子问题的解合起来得出最终的结果

回溯：尝试问题每一步的所以选项

贪心：计算局部最优

动态规划：


DFS 深度优先搜索：先搜索完子节点的子节点

BFS 广度优先搜索：先搜索离根节点更近的子节点

斐波那契数列：（黄金分割数列） 每个数为前两个数的和


排序算法

快速排序：取第一个元素做目标对比其他元素，小于目标的放左数组，大于目标的放右数组，分别对左右数组递归调用，获得值后左右拼接返回。（时间O(n log n)，空间O(log n)）

插入算法：从第二个元素开始遍历，每个元素再遍历左侧元素比较大小去移动。（时间O(n^2)，空间O(1)）

冒泡算法：遍历比较当前和下一个比较大小交换位置，遍历完后最后一个就是最大的，然后再继续从头遍历比较，不重复遍历最后一个。（时间O(n^2)，空间O(1)）





动态规划

双指针



前端该如何准备数据结构和算法？ https://juejin.cn/post/6844903919722692621
如何系统地学习数据结构与算法？https://zhuanlan.zhihu.com/p/137041568
