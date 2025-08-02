## 线性DP——在数组前缀或后缀做转移
### ①最长公共子序列
[最长公共子序列 编辑距离【基础算法精讲 19】](https://www.bilibili.com/video/BV1TM4y1o7ug?vd_source=ab8f6cb2a78f8a26a792e44477726c95&spm_id_from=333.788.videopod.sections)  
[1143.最长公共子序列](https://leetcode.cn/problems/longest-common-subsequence/description/)  
```python
class Solution:
    def longestCommonSubsequence(self, s: str, t: str) -> int:
        n = len(s)
        m = len(t)
        # @cache
        # def dfs(i, j):
        #     if i < 0 or j < 0:
        #         return 0
        #     if s[i] == t[j]:
        #         return dfs(i-1, j-1) + 1
        #     return max(dfs(i-1, j), dfs(i, j - 1))
        # return dfs(n-1, m-1)
        f = [[0] * (m + 1) for _ in range(n + 1)]
        for i, x in enumerate(s):
            for j, y in enumerate(t):
                if x == y:
                    f[i+1][j+1] = f[i][j] + 1
                else:
                    f[i+1][j+1] = max(f[i][j+1], f[i+1][j])
        return f[n][m]
```

### ②最长递增子序列
[【最长递增子序列【基础算法精讲 20】】](https://www.bilibili.com/video/BV1ub411Q7sB?vd_source=ab8f6cb2a78f8a26a792e44477726c95&spm_id_from=333.788.videopod.sections)  
[300.最长递增子序列](https://leetcode.cn/problems/longest-increasing-subsequence/submissions/627531172/)  
从回溯-> 记忆化搜素 + 递推 -> 优化状态
```python
class Solution:
    def lengthOfLIS(self, nums: List[int]) -> int:
        # n = len(nums)
        # @cache
        # def dfs(i):
        #     res = 0
        #     for j in range(i):
        #         if nums[j] < nums[i]:
        #             res = max(res, dfs(j))
        #     return res + 1
        # return max(dfs(i) for i in range(n))
        n = len(nums)
        f = [0] * n
        for i in range(n):
            for j in range(i):
                if nums[j] < nums[i]:
                    f[i] = max(f[i], f[j])
            f[i] += 1
        return max(f)
```
**状态定义**：f[i]表示以nums[i]结尾的最长递增子序列的长度。  

O(n^2)时间复杂度较高，有无可优化方法？  

**进阶技巧：** 交换状态和状态值  
![](DP-Pictures/DP%20线性-1.png)  
为什么要维护一个最小值呢？看下面的解释：
![](DP-Pictures/DP%20线性-2.png)  
由此看来，维护最小值才有可能让这个子序列变得更大  
并且每次都是去改变第一个大于等于nums[i]的第一个数字 -> **bisect_left** !
![](DP-Pictures/DP%20线性-3.png)

![](DP-Pictures/DP%20线性-4.png)

![](DP-Pictures/DP%20线性-5.png)
```python
class Solution:
    def lengthOfLIS(self, nums: List[int]) -> int:
        g = []
        for x in nums:
            j = bisect_left(g, x)
            if j == len(g):
                g.append(x)
            else:
                g[j] = x
        return len(g)
```

