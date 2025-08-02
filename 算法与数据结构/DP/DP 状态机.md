## DP 状态机
[买卖股票的最佳时机【基础算法精讲 21】](https://www.bilibili.com/video/BV1ho4y1W7QK?vd_source=ab8f6cb2a78f8a26a792e44477726c95&spm_id_from=333.788.videopod.sections)  
[122.买卖股票的最佳时机](https://leetcode.cn/problems/best-time-to-buy-and-sell-stock-ii/description/)
![](DP-Pictures/DP%20状态机-1.png)  
![](DP-Pictures/DP%20状态机-2.png)
这种表示状态之间转换的图叫**状态机**
![](DP-Pictures/DP%20状态机-3.png)  
**状态定义**：f[i][0]表示第i天不持有股票时的最大利润，f[i][1]表示第i天持有股票时的最大利润。  

- **参数i**表示前i天里的最大利润  
- **参数0/1**表示布尔值->是否持有股票  
![](DP-Pictures/DP%20状态机-4.png)  
**为啥最后是dfs(n-1, 0)?**  
- 因为最后要返回最大利润，所以最后返回f[n][0]  
- 如果最后还有股票，但是我卖不出去了，不是浪费钱么
- i 是从 n-1 递归到 -1 的，所以初始调用是 dfs(n-1, False)（从最后一天开始，未持有股票）
![](DP-Pictures/DP%20状态机-5.png)
```python
class Solution:
    def maxProfit(self, prices: List[int]) -> int:
        # n = len(prices)
        # @cache
        # def dfs(i, hold):
        #     if i < 0:
        #         return -inf if hold else 0
        #     if hold:
        #         return max(dfs(i-1, True), dfs(i-1, False) - prices[i])
        #     return max(dfs(i-1, False), dfs(i-1, True) + prices[i])
        # return dfs(n-1, False)
        n = len(prices)
        f = [[0] * 2 for _ in range(n+1)]
        f[0][1] = -inf
        for i, p in enumerate(prices):
            f[i+1][0] = max(f[i][0], f[i][1] + p)
            f[i+1][1] = max(f[i][1], f[i][0] - p)
        return f[n][0]
```