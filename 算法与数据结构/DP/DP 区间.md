## DP区间——扩展到在中间做DP，而不只是限于前缀/后缀
从数组的左右两端不断缩短，求解关于某段下标区间的最优值。

一般定义 f[i][j] 表示下标区间 [i,j] 的最优值。  
[【区间 DP：最长回文子序列【基础算法精讲 22】】](https://www.bilibili.com/video/BV1Gs4y1E7EU?vd_source=1e683c3cb93400956a910790b98ffccb)  
![](DP-Pictures/DP%20区间-1.png)  
所以你会发现，如果你的DP两个参数是左右端点，研究的是这个区间里的性质，那么这就可以用区间DP来求解
### ①选或不选  
![](DP-Pictures/DP%20区间-2.png)  
类似最长公共子序列  
![](DP-Pictures/DP%20区间-3.png)
**到底怎么枚举?**   
看怎么来的：从前转移来的还是后面转移来的
![](DP-Pictures/DP%20区间-4.png)
[516.最长回文子序列](https://leetcode.cn/problems/longest-palindromic-subsequence/description/)  

```python
class Solution:
    def longestPalindromeSubseq(self, s: str) -> int:
        n = len(s)
        # @cache
        # def dfs(i, j):
        #     if i == j:
        #         return 1
        #     if i > j:
        #         return 0
        #     if s[i] == s[j]:
        #         return dfs(i+1, j-1) + 2
        #     return max(dfs(i+1, j), dfs(i, j-1))
        # return dfs(0, n-1)
        f = [[0] * n for _ in range(n)]
        for i in range(n-1, -1, -1): # 发现i要通过i+1求，所以倒序
            f[i][i] = 1 # 直接在这里填入1就行
            for j in range(i+1, n): # 发现j由j-1来，所以正序
                if s[i] == s[j]:
                    f[i][j] = f[i+1][j-1] + 2
                else:
                    f[i][j] = max(f[i+1][j], f[i][j-1])
        return f[0][n-1]
```
### ②枚举选哪个
![](DP-Pictures/DP%20区间-5.png)
**到底怎么枚举?**   
看怎么来的：从前转移来的还是后面转移来的
![](DP-Pictures/DP%20区间-6.png)
[1039.多边形三角剖分的最低得分](https://leetcode.cn/problems/minimum-score-triangulation-of-polygon/)  
```python
class Solution:
    def minScoreTriangulation(self, v: List[int]) -> int:
        n = len(v)
        # @cache
        # def dfs(i, j):
        #     if i+1 == j:
        #         return 0
        #     res = inf
        #     for k in range(i+1, j):
        #         res = min(res, dfs(i, k) + dfs(k, j) + v[i] * v[j] * v[k])
        #     return res
        # return dfs(0, n-1)
        f = [[0] * n for _ in range(n)]
        for i in range(n-3, -1, -1):
            for j in range(i+2, n):
                res = inf
                for k in range(i+1, j):
                    res = min(res, f[i][k] + f[k][j] + v[i] * v[j] * v[k])
                f[i][j] = res
        return f[0][n-1]
```

**第451场周赛T4** [3563.移除相邻字符后字典序最小的字符串](https://leetcode.cn/problems/lexicographically-smallest-string-after-adjacent-removals/description/) ~ 2700  
区间DP + 线性DP  
1. 区间DP求是否可以变成空串
![](DP-Pictures/DP%20区间-7.png)
2. 线性DP求出最小字典序
![](DP-Pictures/DP%20区间-8.png)

```python
def is_consecutive(x: str, y: str) -> bool:
    d = abs(ord(x) - ord(y))
    return d == 1 or d == 25

class Solution:
    def lexicographicallySmallestString(self, s: str) -> str:
        n = len(s)

        @cache
        def can_be_empty(i: int, j: int) -> bool:
            if i > j:  # 空串
                return True
            # 性质 2
            if is_consecutive(s[i], s[j]) and can_be_empty(i + 1, j - 1):
                return True
            # 性质 3
            for k in range(i + 1, j - 1, 2):
                if can_be_empty(i, k) and can_be_empty(k + 1, j):
                    return True
            return False

        @cache
        def dfs(i: int) -> str:
            if i == n:
                return ""
            # 包含 s[i]
            res = s[i] + dfs(i + 1)
            # 不包含 s[i]，注意 s[i] 不能单独消除，必须和其他字符一起消除
            for j in range(i + 1, n, 2):
                if can_be_empty(i, j):  # 消除 s[i] 到 s[j]
                    res = min(res, dfs(j + 1))
            return res

        return dfs(0)
```
