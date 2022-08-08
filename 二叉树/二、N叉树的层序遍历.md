# 二叉树的层序遍历

二叉树的层序遍历就是逐层地、从左到右访问二叉树所有节点。

<img src="../../assets/image-20220807160350868.png" alt="image-20220807160350868" style="zoom:33%;" />

返回的结果是[[3],[9,20],[15,7]]

需要借助辅助数据结构**队列**实现，队列先进先出，符合一层一层遍历的逻辑，递归先进后出，适合模拟深度优先遍历也就是递归的逻辑。

层序遍历方式，即广度优先遍历，应用在二叉树上。

<img src="https://tva1.sinaimg.cn/large/008eGmZEly1gnad5itmk8g30iw0cqe83.gif" alt="102二叉树的层序遍历" style="zoom:50%;" />

**思路：**要明确两个点：1.如何识别层的深度 。2. 如何识别每层的节点数

**层节点数：**下一层的节点数受上一层节点数影响，因此，可以根据在上层队列添加的节点通过for循环，每层所有节点数的遍历。

**深度**通过遍历队列识别，直到None说明每一层都已经得到遍历，



#### [102. 二叉树的层序遍历](https://leetcode.cn/problems/binary-tree-level-order-traversal/)

给你二叉树的根节点 `root` ，返回其节点值的 **层序遍历** 。 （即逐层地，从左到右访问所有节点）。

**示例 1：**

<img src="../../assets/image-20220807160111243.png" alt="image-20220807160111243" style="zoom:33%;" />

输入：root = [3,9,20,null,null,15,7]
输出：[[3],[9,20],[15,7]]
示例 2：

输入：root = [1]
输出：[[1]]
示例 3：

输入：root = []
输出：[]


提示：

树中节点数目在范围 [0, 2000] 内
-1000 <= Node.val <= 1000

```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
class Solution:
    def levelOrder(self, root: TreeNode) -> List[List[int]]:
        results=[]
        que=deque([root])#先把二叉树的头放到队列里

        if not root:
            return results

        while que :#这里每一轮是一层
            res=[]
            for _ in range(len(que)):#这里根据先前放入节点的长度，划分二叉树的层次
                node=que.popleft()#以先进先出的规则出队列
                res.append(node.val)
                if node.left:
                    que.append(node.left)
                if node.right:
                    que.append(node.right)
            results.append(res)
        return results
```

### 倒序层序遍历

因为只需要按层倒着输出就OK，因此只需要按照正序遍历的逻辑，将最终结果切片倒序输出

![image-20220807162843644](../../assets/image-20220807162843644.png)



### 二叉树保留最大深度（二叉树的左右视图）

#### [199. 二叉树的右视图](https://leetcode.cn/problems/binary-tree-right-side-view/)

给定一个二叉树的 根节点 root，想象自己站在它的右侧，按照从顶部到底部的顺序，返回从右侧所能看到的节点值。

 

示例 1:

输入: [1,2,3,null,5,null,4]
输出: [1,3,4]
示例 2:

输入: [1,null,3]
输出: [1,3]
示例 3:

输入: []
输出: []


提示:

二叉树的节点个数的范围是 [0,100]
-100 <= Node.val <= 100 

**思路**：

1.保留最大深度的方法，通过遍历，获得最大深度

2.优先保存右节点，没有右节点，则保存左节点的方法，使用队列保存，输出值的时候使用字典进行同一层的覆盖（用后append的right值覆盖掉前面的left。

```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
class Solution:
    def rightSideView(self, root: Optional[TreeNode]) -> List[int]:
        right_mdv=dict()#这里使用字典，能够进行覆盖和保留
        res=[]
        maxdepth=0
        que=deque([(root,0)])
        if not root:
            return res
        while que:
            node,depth=que.popleft()

            if node is not None:
                maxdepth=max(depth,maxdepth)
                right_mdv[depth]=node.val#使用字典方法覆盖
                que.append((node.left,depth+1))#使用队列添加
                que.append((node.right,depth+1))

        for i in range(maxdepth+1):
            res.append(right_mdv[i])
        return res
```



## N叉树的遍历

**核心思路就是注意node.children是个列表，包含所有节点的子节点**，注意需要使用for遍历，把子节点拆出来。

### [429. N 叉树的层序遍历](https://leetcode.cn/problems/n-ary-tree-level-order-traversal/)

给定一个 N 叉树，返回其节点值的层序遍历。（即从左到右，逐层遍历）。

树的序列化输入是用层序遍历，每组子节点都由 null 值分隔（参见示例）。

 

示例 1：

<img src="../../assets/image-20220808094204809.png" alt="image-20220808094204809" style="zoom: 67%;" />

输入：root = [1,null,3,2,4,null,5,6]
输出：[[1],[3,2,4],[5,6]]
示例 2：

<img src="../../assets/image-20220808094230480.png" alt="image-20220808094230480" style="zoom:67%;" />

输入：root = [1,null,2,3,4,5,null,null,6,7,null,8,null,9,10,null,null,11,null,12,null,13,null,null,14]
输出：[[1],[2,3,4,5],[6,7,8,9,10],[11,12,13],[14]]


提示：

树的高度不会超过 1000
树的节点总数在 [0, 10^4] 之间

**层序遍历方式与二叉树并无差别，除了.children的用法**

```python
"""
# Definition for a Node.
class Node:
    def __init__(self, val=None, children=None):
        self.val = val
        self.children = children
"""

class Solution:
    def levelOrder(self, root: 'Node') -> List[List[int]]:
        que=deque([root])
        result=[]
        if not root:
            return result
        while que:
            res=[]
            for _ in range(len(que)):
                node=que.popleft()
                res.append(node.val)
                #node.children是个列表，包含所有节点的子节点
                for child in node.children:
                    que.append(child)
            result.append(res)
        return result
```



### 二叉树节点值的比较

这部分在结构上其实没什么，主要要注意的就是值的比较，如果遇到与负数比较，可以设定初始值为**-inf(一个无限小的负数)**或者**inf(一个无限大的正数)**

#### [515. 在每个树行中找最大值](https://leetcode.cn/problems/find-largest-value-in-each-tree-row/)

给定一棵二叉树的根节点 root ，请找出该二叉树中每一层的最大值。

 

示例1：

<img src="../../assets/image-20220808095605652.png" alt="image-20220808095605652" style="zoom:67%;" />

输入: root = [1,3,2,5,3,null,9]
输出: [1,3,9]
示例2：

输入: root = [1,2,3]
输出: [1,3]


提示：

二叉树的节点个数的范围是 [0,10^4]
-2^31 <= Node.val <= 2^31 - 1

 代码：

```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
class Solution:
    def largestValues(self, root: Optional[TreeNode]) -> List[int]:
        que=deque([root])
        result=[]
        if not root:
            return result
        while que:    
            res=-inf
            for i in range(len(que)):
                node=que.popleft()
                res=max(res,node.val)
                if node.left:
                    que.append(node.left)
                if node.right:
                    que.append(node.right)
            result.append(res)
        return result
```

