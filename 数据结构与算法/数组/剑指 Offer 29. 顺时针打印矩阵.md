#### [剑指 Offer 29. 顺时针打印矩阵](https://leetcode.cn/problems/shun-shi-zhen-da-yin-ju-zhen-lcof/)

输入一个矩阵，按照从外向里以顺时针的顺序依次打印出每一个数字。

 

示例 1：

输入：matrix = [[1,2,3],[4,5,6],[7,8,9]]
输出：[1,2,3,6,9,8,7,4,5]
示例 2：

输入：matrix = [[1,2,3,4],[5,6,7,8],[9,10,11,12]]
输出：[1,2,3,4,8,12,11,10,9,5,6,7]


限制：

0 <= matrix.length <= 100
0 <= matrix[i].length <= 100

**适用于非方形矩阵**

思路：

解题思路：
根据题目示例 matrix = [[1,2,3],[4,5,6],[7,8,9]] 的对应输出 [1,2,3,6,9,8,7,4,5] 可以发现，顺时针打印矩阵的顺序是 “从左向右、从上向下、从右向左、从下向上” 循环。

因此，考虑设定矩阵的“左、上、右、下”四个边界，模拟以上矩阵遍历顺序。

![image-20220720192338692](../../assets/image-20220720192338692.png)

算法流程：
空值处理： 当 matrix 为空时，直接返回空列表 [] 即可。
初始化： 矩阵 左、右、上、下 四个边界 l , r , t , b ，用于打印的结果列表 res 。
循环打印： **“从左向右、从上向下、从右向左、从下向上”** 四个方向循环，每个方向打印中做以下三件事 （各方向的具体信息见下表） ；
根据边界打印，即将元素按顺序添加至列表 res 尾部；
边界向内收缩 11 （代表已被打印）；
判断是否打印完毕（边界是否相遇），若打印完毕则跳出。
返回值： 返回 res 即可。

![image-20220720192434213](../../assets/image-20220720192434213.png)

复杂度分析：
时间复杂度 O(MN) ： M, N分别为矩阵行数和列数。
空间复杂度 O(1) ： 四个边界 l , r , t , b 使用常数大小的 额外 空间（ res 为必须使用的空间）。

![image-20220720192530580](../../assets/image-20220720192530580.png)

代码：

```Python
class Solution:
    def spiralOrder(self, matrix: List[List[int]]) -> List[int]:
        if not matrix:
            return []
        l,t,r,b,res=0,0,len(matrix[0]),len(matrix),[]
        while True:#对应break
            for i in range(l,r):
                res.append(matrix[t][i])
            t+=1#上边界下移
            if t >b-1: break#如果左边界超过上边界，结束循环
            for i in range(t,b):
                res.append(matrix[i][r-1])
            r-=1#右边界左移
            if l >r-1: break
            for i in range(r-1,l-1,-1):
                res.append(matrix[b-1][i])
            b-=1#下边界上移
            if t >b-1: break
            for i in range(b-1,t-1,-1):
                res.append(matrix[i][l])
            l+=1#左边界右移
            if l >r-1: break
        return res
```

