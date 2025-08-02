## 树形DP 二十三讲
类似DP的自底向上，所以归在一块  
**前置知识：**[树形 DP：树的直径【基础算法精讲 23](https://www.bilibili.com/video/BV17o4y187h1/?spm_id_from=333.337.search-card.all.click&vd_source=ab8f6cb2a78f8a26a792e44477726c95)  
其实和二叉树的最大直径那道题有很大联系：[104.二叉树的最大直径](https://leetcode.cn/problems/maximum-depth-of-binary-tree/description/)  
树形直径模板题：[2246](https://leetcode.cn/problems/longest-path-with-different-adjacent-characters/)
```python
# 先引入邻居的概念：相邻的节点，如二叉树和左孩子和右孩子
class Solution:
    def longestPath(self, parent: List[int], s: str) -> int:
        n = len(parent)
        g = [[] for _ in range(n)] # 建树 (无父结点)
        for i in range(1, n):
            g[parent[i]].append(i)
        
        ans = 0
        def dfs(x, fa):
            nonlocal ans
            x_len = 0
            for y in g[x]: # 枚举儿子们
                if y == fa: continue # 跳过父亲(如果有的话)
                y_len = dfs(y, x) + 1
                if s[y] != s[x]:
                    ans = max(ans, x_len + y_len)
                    x_len = max(x_len, y_len) # 这里是在维护最大和次大
            return x_len # 最大

        dfs(0, -1) # -1表示根节点无父节点
        return ans + 1
```
推广例题：感染二叉树 [2385](https://leetcode.cn/problems/amount-of-time-for-binary-tree-to-be-infected/description/)
```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
class Solution:
    def amountOfTime(self, root: Optional[TreeNode], start: int) -> int:
        # 多维护了一个判断是否找到了start节点的布尔值 就卡在这里了
        ans = 0 
        def dfs(node):
            if not node:
                return 0, False
            left, l_found = dfs(node.left)
            right, r_found = dfs(node.right)
            nonlocal ans
            if node.val == start:
                ans = max(left, right)
                return 1, True
            if l_found or r_found:
                ans = max(left + right, ans)
                return (left if l_found else right) + 1, True
            return max(left, right) + 1, False
        dfs(root)
        return ans
```
