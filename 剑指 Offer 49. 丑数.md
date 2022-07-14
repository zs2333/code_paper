#### [剑指 Offer 49. 丑数](https://leetcode.cn/problems/chou-shu-lcof/)

我们把只包含质因子 2、3 和 5 的数称作丑数（Ugly Number）。求按从小到大的顺序的第 n 个丑数。

示例:

输入: n = 10
输出: 12
解释: 1, 2, 3, 4, 5, 6, 8, 9, 10, 12 是前 10 个丑数。
说明:  

1 是丑数。
n 不超过1690。

思路：动态规划，三指针，p2,p3,p5,dp(n)一定是前面某个数乘以（2,3,5）中的某个数，按从大到小排列的话，就是乘起来较小的数排到前面，较大的数排后面。

```python
class Solution:
    def nthUglyNumber(self, n: int) -> int:
        dp=[0]*n
        dp[0]=1
        p2=p3=p5=0#三个指针都从0开始
        for i in range(1,n):
            dp[i]=min(dp[p2]*2,dp[p3]*3,dp[p5]*5)#最小的数排前面，然后指针向后推
            if dp[i]==dp[p2]*2:
                p2+=1
            if dp[i]==dp[p3]*3:#注意这里不能用elif,elif代表互斥，这里存在有共同因子的情况，如2*3和3*2，这时p2与p3都要向后推否则会出现重复。注意if与elif 的区别
                p3+=1
            if dp[i]==dp[p5]*5:
                p5+=1
        return dp[n-1]
```

