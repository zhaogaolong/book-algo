# DP

DP 的核心是不断的复用前面的数据, 动态的去寻找结果。想解 dp 问题，其核心是寻找 dp 的*递推公式*.

| 问题名称         | 链接                                              |
| ---------------- | ------------------------------------------------- |
| 198. 打家劫舍    | https://leetcode-cn.com/problems/house-robber/    |
| 213. 打家劫舍 II | https://leetcode-cn.com/problems/house-robber-ii/ |

## 198. 打家劫舍

> https://leetcode-cn.com/problems/house-robber-ii/

动态规划的推到公式

当前房间的价值 = max(前 2 个房子的价值 + 当前房子的价值, 对比 前一个房子的价值）
`dp[i] = max(dp[i-2]+nums[i], dp[i-1])`

```golang
func rob(nums []int) int {
    // 初识数据判断
    n := len(nums)
    if n == 0{
        return 0
    }else if n == 1{
        return nums[0]
    }

    // dp 推到
    dp := make([]int, n)
    dp[0] = nums[0]
    dp[1] = max(nums[0], nums[1])
    for i:=2; i<n; i++{
        dp[i] = max(dp[i-2]+nums[i], dp[i-1])
    }
    return dp[n-1]
}

func max(a, b int) int{
    if a > b{
        return a
    }
    return b
}
```

复杂度分析：空间复杂度：O(n), 时间复杂度：O(n)

### 优化 1

发现 dp 里的数据直接可以抽成两个变量存储即可

```go
func rob(nums []int) int {
    // 初识数据判断
    n := len(nums)
    if n == 0{
        return 0
    }else if n == 1{
        return nums[0]
    }

    // dp 推到
    first, second := nums[0], max(nums[0], nums[1])
    for i:=2; i<n; i++{
        first, second = second, max(first + nums[i], second)
    }
    return second
}
func max(a, b int) int{
    if a > b{
        return a
    }
    return b
```

复杂度分析：空间复杂度：O(1), 时间复杂度：O(n)

## 213. 打家劫舍 II

> https://leetcode-cn.com/problems/house-robber-ii/

题目要求： 房子连接成环

dp 推到：
从第二个房子开始偷: `dp1[0], dp1[1] = 0, nums[1]`
从第一个房子开始偷: `dp2[0], dp2[1] = nums[0], nums[1]`
最终结果：`max(dp[len(nums)-1], dp2[len(nums)-1]`

```go
func rob(nums []int) int {
    n := len(nums)
    if n == 0{
        return 0
    }else if n == 1{
        return nums[0]
    }

    dp1 := make([]int, n)
    dp1[0] = 0
    dp1[1] = nums[1]
    for i:=2; i<n; i++{
        dp1[i] = max(dp1[i-2]+nums[i], dp1[i-1])
    }

    dp2 := make([]int, n)
    dp2[0] = nums[0]
    dp2[1] = max(nums[0], nums[1])
    for i:=2; i<n; i++{
        dp2[i] = max(dp2[i-2]+nums[i], dp2[i-1])
    }

    /*
    dp1[n-1]
    dp2[n-2] 为啥，因为 dp2 是从第一个开始偷得， dp1 是从第二个开始偷得(环的要求)
    */
    return max(dp1[n-1], dp2[n-2])
}

func max(a, b int) int{
    if a > b{
        return a
    }
    return b
}
```
