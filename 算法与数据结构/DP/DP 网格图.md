## DP 网格图搜索
题单：[DP题单](https://leetcode.cn/discuss/post/3581838/fen-xiang-gun-ti-dan-dong-tai-gui-hua-ru-007o/)  
主要分清楚**正序**还是**倒序**，然后在改善DP写法的时候来分析是正序遍历还是倒序遍历  

例题：[LeetCode 64. 最小路径和](https://leetcode.cn/problems/minimum-path-sum/)  
其实能掌握记忆化搜素和初步的DP转化就能够解决大部分问题了（
```python
class Solution:
    def minPathSum(self, grid: List[List[int]]) -> int:
        @cache  # 缓存装饰器，避免重复计算 dfs 的结果（记忆化）
        def dfs(i: int, j: int) -> int:
            if i < 0 or j < 0:  # 边界条件 
                return inf
            if i == 0 and j == 0:
                return grid[i][j]
            return min(dfs(i, j - 1), dfs(i - 1, j)) + grid[i][j]
        return dfs(len(grid) - 1, len(grid[0]) - 1) # 从右下角开始搜索，可以说是倒序递归
```
#### 这个时候就有人要问了：我怎么知道出界返回的是inf,-inf还是0呢？  
**Inf**：求的最小值，需要求min  
**-Inf**: 求的最大值，需要求max（当然是包括负数的，如果没有负数就可以return 0）  
**0**：求和，需要求sum   

    具体来说，f[i+1][j+1] 的定义和 dfs(i,j) 的定义是一样的，都表示从左上角到第 i 行第 j 列这个格子（记作 (i,j)）的最小价值和。这里 +1 是为了把 dfs(−1,j) 和 dfs(i,−1) 这些状态也翻译过来，这样我们可以把 f[0][j] 和 f[i][0] 作为初始值。  
相应的递推式（状态转移方程）也和 dfs 一样：  
f[i+1][j+1]=min(f[i+1][j],f[i][j+1])+grid[i][j]  
**问：为什么 grid[i][j] 的下标不用变?**  

    答：既然是在 f 的最左边和最上边插入一排状态，那么就只需要修改和 f 有关的下标，其余任何逻辑都无需修改。或者说，如果把 grid[i][j] 也改成 grid[i+1][j+1]，那么当 i=m−1 或者 j=n−1 时 grid[i+1][j+1] 会下标越界，这显然是错误的。
```python
class Solution:
    def minPathSum(self, grid: List[List[int]]) -> int:
        m, n = len(grid), len(grid[0])
        f = [[inf] * (n + 1) for _ in range(m + 1)]
        for i, row in enumerate(grid): # 正序遍历：why？因为原来是从后开始递归的，意思就是要先知道前面的值，所以从头开始遍历
            for j, x in enumerate(row):
                if i == j == 0:
                    f[1][1] = x
                else:
                    f[i + 1][j + 1] = min(f[i + 1][j], f[i][j + 1]) + x
        return f[m][n]
```
居然这道题做出来了）  
**技巧：在负数交替的时候，求最终的最值：要同时维护最大值和最小值**  
[1594 矩阵中的最大非负积](https://leetcode.cn/problems/maximum-non-negative-product-in-a-matrix/description/)