# 股票问题

| 问题名称 | 链接                                                                                   |
| -------- | -------------------------------------------------------------------------------------- |
| 股票买卖 | https://leetcode-cn.com/problems/best-time-to-buy-and-sell-stock/                      |
| 股票买卖 | https://leetcode-cn.com/problems/best-time-to-buy-and-sell-stock-ii/                   |
| 股票买卖 | https://leetcode-cn.com/problems/best-time-to-buy-and-sell-stock-with-cooldown/        |
| 股票买卖 | https://leetcode-cn.com/problems/best-time-to-buy-and-sell-stock-with-transaction-fee/ |
| 股票买卖 | https://leetcode-cn.com/problems/best-time-to-buy-and-sell-stock-iii/                  |
| 股票买卖 | https://leetcode-cn.com/problems/best-time-to-buy-and-sell-stock-iv/                   |

---

## 121. 买卖股票的最佳时机

> https://leetcode-cn.com/problems/best-time-to-buy-and-sell-stock/

要求：只能交易一次（买+卖）
思路：

- `dp0` 代表手里有钱，没股票
- `dp1` 代表手里有股票,(有可能欠钱)
- 因为只能是一次交易（买+买）,所以就用两个 dp 来表示即可

```go
func maxProfit(prices []int) int {
    // dp0 代表卖，剩的钱
    // dp1 代表买，买剩下的价值
    dp0, dp1 := 0, math.MinInt64

    for i := range prices{
        // 清不清仓，
        // dp1（手里有股票） + prices[i]
        dp0 = max(dp0, dp1 + prices[i])

        // 加不加仓
        dp1 = max(dp1, 0 - prices[i])
    }
    return dp0
}

func max(a, b int) int{
    if a > b{
        return a
    }
    return b
}
```

## 122. 买卖股票的最佳时机 II

> https://leetcode-cn.com/problems/best-time-to-buy-and-sell-stock-ii/

问题重点：

1. 交易次数尽可能的多
2. 买和卖不能在同一天

解题思路：

```go
func maxProfit(prices []int) int {
    dp0, dp1 := 0, math.MinInt64
    for i := range prices{
        // 昨天的 dp0
        pre := dp0
        dp0 = max(dp0, dp1 + prices[i])

        // 昨天的 dp0 减去当前的 price
        dp1 = max(dp1, pre - prices[i])
        pre = dp0 // 保存今天的值，在下次循环时候使用
    }
    return dp0
}

func max(a, b int) int{
    if  a > b{ return a }
    return b
}
```

## 309. 最佳买卖股票时机含冷冻期

> https://leetcode-cn.com/problems/best-time-to-buy-and-sell-stock-with-cooldown/

问题重点：

1. 不限制交易次数
2. 股票售出后，第二天不能买入股票（冷冻期一天）

```go
func maxProfit(prices []int) int {
    dp0 := 0
    dp1 := -1 << 32
    pre := 0
    for i := range prices{
        tmp := dp0 //  前一天的 dp0
        dp0 = max(dp0, dp1 + prices[i])
        dp1 = max(dp1, pre - prices[i])
        pre = tmp // 前一天的 dp0，下次 pre - 的时候就是下次循环了，也就是前两天的 dp0 了
    }
    return dp0
}

func max(a, b int) int {
    if a  > b{
        return a
    }
    return b
}
```

## 714. 买卖股票的最佳时机含手续费

> https://leetcode-cn.com/problems/best-time-to-buy-and-sell-stock-with-transaction-fee/

题目要点： 每次交易收取一定的手续费，不限次数交易（多挣交易费嘛 ^\_^）
题目分析：

- `dp0` 代表手里有钱，没股票
  - dp 公式：`dp1 = max(dp1, dp1 + prices[i])`
- `dp1` 代表手里有股票, 从前一天的 dp0 - 今天股价 - 手续费
  - dp 公式：`dp1 = max(dp1, dp0 - prices[i] - fee)`

```go
func maxProfit(prices []int, fee int) int {
    dp0, dp1 := 0, math.MinInt64
    for _, p := range prices{
        dp0 = max(dp0, dp1 + p)
        dp1 = max(dp1, dp0 - p - fee)
    }
    return dp0
}

func max(a, b int) int{
    if a > b{return a}
    return b
}
```

## 123. 买卖股票的最佳时机 III

> https://leetcode-cn.com/problems/best-time-to-buy-and-sell-stock-iii/

```go
func maxProfit(prices []int) int {
    dp01 := 0
    dp11 := -1 << 63
    dp02 := 0
    dp22 := -1 << 63
    for _, p := range prices{
        dp02 = max(dp02, dp22 + p)
        dp22 = max(dp22, dp01 - p)
        dp01 = max(dp01, dp11 + p)
        dp11 = max(dp11, 0 - p)
    }
    return dp02
}
func max(a, b int) int{
    if a > b {return a}
    return b
}
```

### 123. 买卖股票的最佳时机 III

> https://leetcode-cn.com/problems/best-time-to-buy-and-sell-stock-iii/

```go
func maxProfit(prices []int) int {
    dp0_0, dp0_1 := 0, math.MinInt64
    dp1_0, dp1_1 := 0, math.MinInt64
    for _, p := range prices{
        // 第二次交易
        dp1_0 = max(dp1_0, dp1_1 + p)
        // 这里使用 dp0_0(第一次交易的利润) - p 意思是第笔交易建立在第一笔交易完成之后
        dp1_1 = max(dp1_1, dp0_0 - p)

        // 第一次交易和 第一题一致
        dp0_0 = max(dp0_0, dp0_1 + p)
        dp0_1 = max(dp0_1, 0 - p)
    }

    return dp1_0
}

func max(a, b int) int{
    if a > b{return a}
    return b
}
```

## 买卖股票的最佳时机 IV

> https://leetcode-cn.com/problems/best-time-to-buy-and-sell-stock-iv/

要点：

- 完成 k 次交易
- 必须完成买完后再买

问题分析：

- `K` 的限制分析：一次完整交易需要两天，所以 `K <= n/2`
- 如果 `K` 超过了 `n/2` 就没有约束作用了，想当与直接退化为问题 `k = +infinity`《122. 买卖股票的最佳时机 II》

```go
func maxProfit(k int, prices []int) int {
    if k > len(prices)/2{
        return maxProfit_k_inf(prices)
    }

    dp0 := make([]int, k+1)
    dp1 := make([]int, k+1)
    for i := range dp1{
        dp1[i] = math.MinInt64
    }
    for _, p := range prices{

        /*
        为什么是  i=k i--?
        1. 首先计算出 每个 p 的交易结果， p++ 嘛
        2. 第 k 次交易依赖了 k-1 的交易, 也就是：当前是价格是 prices[i]，k-1 是使用 prices[i-1] 的交易结果，所以就形成了 k 交易依赖了 k-1 次交易的结果.
        */
        for i := k; i >= 1; i--{
            dp0[i] = max(dp0[i], dp1[i] + p)
            dp1[i] = max(dp1[i], dp0[i-1] - p)
        }
    }
    return dp0[k]
}

func max(a, b int) int{
    if a > b{return a}
    return b
}


func maxProfit_k_inf(prices []int) int{
    dp0, dp1 := 0, math.MinInt64
    for _, p := range prices{
        dp0 = max(dp0, dp1 + p)
        dp1 = max(dp1, dp0 - p)
    }
    return dp0
}
```

### 参考链接

- https://leetcode-cn.com/problems/best-time-to-buy-and-sell-stock-iv/solution/yi-ge-tong-yong-fang-fa-tuan-mie-6-dao-gu-piao-w-5/
