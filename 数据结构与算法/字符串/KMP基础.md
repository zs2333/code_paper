# KMP算法

**核心思想：**当出现字符串不匹配时，可以记录一部分之前**已经匹配的文本内容**，利用这些信息**避免从头**再去做匹配。

KMP主要应用在字符串匹配上。

**如何记录已经匹配的文本内容**，是KMP的重点

### 前缀表

前缀表是用来回退的，它记录了模式串与主串（文本串）不匹配的时候，模式串应该从哪里开始重新匹配。

next数组就是一个前缀表（prefix table）

**例子：**要在文本串：aabaabaafa 中查找是否出现过一个模式串：aabaaf。

![KMP详解1](https://code-thinking.cdn.bcebos.com/gifs/KMP%E7%B2%BE%E8%AE%B21.gif)

可以看出，文本串中第六个字符b 和 模式串的第六个字符f，不匹配了。如果暴力匹配，会发现不匹配，此时就要从头匹配了。

但如果使用前缀表，就不会从头匹配，而是**从上次已经匹配的内容开始匹配**，找到了模式串中第三个字符b继续开始匹配。

前缀表的任务是当前位置匹配失败，找到之前已经匹配上的位置，再重新匹配，也就意味着在某个字符不匹配时，前缀表指示下一步匹配中，模式串应该跳到哪个位置。

**前缀表的记录方式：**记录下表i之前（包括i）的字符串中，有多大长度的相同前缀后缀。

前缀：不包含尾字母 从首字母开始：a aa aab …… aabaabaafa

后缀：不包含首字母 : a fa …… abaabaafa

## **next数组**

next数组对应字符串子串列表 的 相等前后缀长度

如 aababbbfc 对应的next数组的结果是[0,1,0,1,0,0,0,0,0] 

next数组的计算过程：

首先，因为是自己和自己比较，所以指针j设定为0，i设定为1

对于字符串s如果s[i]==s[j],j向前移动一位，如果相反，j向后回退，一直回退到相等或为0为止.

以**[28. 实现 strStr()](https://leetcode.cn/problems/implement-strstr/)**为例：

实现 strStr() 函数。

给你两个字符串 haystack 和 needle ，请你在 haystack 字符串中找出 needle 字符串出现的第一个位置（下标从 0 开始）。如果不存在，则返回  -1 。

说明：

当 needle 是空字符串时，我们应当返回什么值呢？这是一个在面试中很好的问题。

对于本题而言，当 needle 是空字符串时我们应当返回 0 。这与 C 语言的 strstr() 以及 Java 的 indexOf() 定义相符。

 

示例 1：

输入：haystack = "hello", needle = "ll"
输出：2
示例 2：

输入：haystack = "aaaaa", needle = "bba"
输出：-1


提示：

1 <= haystack.length, needle.length <= 10^4
haystack 和 needle 仅由小写英文字符组成

具体代码：

```python
    def get_next(self,a,needle):
        j=0
        next=[0]*a
        next[0]=0
        for i in range(1,a):
            while j>=1 and needle[i]!=needle[j]:
                j=next[j-1]#回退
            if needle[i]==needle[j]:
                j+=1
            next[i]=j
        return next
```

#### 使用next数组来做匹配

因为只是简单匹配，所以原理相似，完整代码如下：

```Python
class Solution:
    def strStr(self, haystack: str, needle: str) -> int:
        a=len(needle)
        b=len(haystack)
        c=0
        next_t=self.get_next(a,needle)
        for r in range(b):#这里注意，因为是两个字符串比较，不必从0开始
            while c>=1 and haystack[r]!=needle[c]:
                c=next_t[c-1]#回退到前一位对应的，相等前后缀值的位置，也就是，相等值的后一位，不断循环回退。
            if haystack[r]==needle[c]:
                c+=1
            if c==a:#此时haystack已经完全匹配needle
                return r-a+1
        return -1

    def get_next(self,a,needle):
        j=0
        next=[0]*a
        next[0]=0
        for i in range(1,a):
            while j>=1 and needle[i]!=needle[j]:
                j=next[j-1]
            if needle[i]==needle[j]:
                j+=1
            next[i]=j
        return next
```

