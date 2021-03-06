# DP

DP 的核心是不断的复用前面的数据, 动态的去寻找结果。想解 dp 问题，其核心是寻找 dp 的*递推公式*.

| 问题名称              | 链接                                                         |
| --------------------- | ------------------------------------------------------------ |
| 509. 斐波那契数       | https://leetcode-cn.com/problems/fibonacci-number/           |
| 62. 不同路径          | https://leetcode-cn.com/problems/unique-paths/               |
| 63. 不同路径 II       | https://leetcode-cn.com/problems/unique-paths-ii/            |
| 1143. 最长公共子序列  | https://leetcode-cn.com/problems/longest-common-subsequence/ |
| 70. 爬楼梯            | https://leetcode-cn.com/problems/climbing-stairs/            |
| 53. 最大子序和        | https://leetcode-cn.com/problems/maximum-subarray/           |
| 152. 乘积最大子数组   | https://leetcode-cn.com/problems/maximum-product-subarray/   |
| 120. 三角形最小路径和 | https://leetcode-cn.com/problems/triangle/                   |

## 509. 斐波那契数

> https://leetcode-cn.com/problems/fibonacci-number/

```go
func fib(n int) int {
    if n <=0{
        return 0
    }
    a, b := 0, 1
    for i:=0; i<n; i++{
        a, b = b, a+b
    }
    return a
```

## 62. 不同路径

> https://leetcode-cn.com/problems/unique-paths/

```go
func uniquePaths(m int, n int) int {
    dp := make([]int, n)
    for i := range dp{
        dp[i] = 1
    }
    for i:=1; i<m;i++{
        for j:=1; j<n;j++{
            dp[j] += dp[j-1]
        }
    }
    return dp[n-1]
}
```

## 63. 不同路径 II

> https://leetcode-cn.com/problems/unique-paths-ii/

```go
func uniquePathsWithObstacles(obstacleGrid [][]int) int {
    m := len(obstacleGrid)
    if m == 0 || obstacleGrid[0][0] == 1{
        return 0
    }
    obstacleGrid[0][0] = 1
    n := len(obstacleGrid[0])

    obs := false
    for i:=1; i<n;i++{
        if obstacleGrid[0][i] == 1{
            obs = true
        }
        obstacleGrid[0][i] = 1
        if obs{
            obstacleGrid[0][i] = 0
        }
    }

    obs = false
    for i:=1; i<m;i++{
        if obstacleGrid[i][0] == 1{
            obs = true
        }
        obstacleGrid[i][0] = 1
        if obs{
            obstacleGrid[i][0] = 0
        }
    }

    for i:=1; i<m; i++{
        for j:=1; j<n; j++{
            if obstacleGrid[i][j] == 1{
                obstacleGrid[i][j] = 0
            }else{
                obstacleGrid[i][j] =obstacleGrid[i][j-1] + obstacleGrid[i-1][j]
            }
        }
    }
    return obstacleGrid[m-1][n-1]
}
```

## 1143. 最长公共子序列

> https://leetcode-cn.com/problems/longest-common-subsequence/

DP 推到公式：

1. `if s1[i] != s2[j]`, 那么它就等于 `dp[i][j] = max(dp[i-1][j], dp[i][j-1])`
2. `if s1[i] == s2[j]`，也就是说两个字符串的最后一位是相同的，，
   - 那么我们可以直接把最后一个字符去掉进行剪枝, 就是 `dp[i][j] = dp[i-1][j-1] +1`
   - 这里不太好理解，需要自己画图理解

```go
func longestCommonSubsequence(text1 string, text2 string) int {
    m := len(text1)
    if m == 0{
        return 0
    }
    n := len(text2)

    dp := make([][]int, m+1)
    for i := range dp{
        dp[i] = make([]int, n+1)
    }

    for i:=1; i<=m; i++{
        for j:=1; j<=n; j++{
            if text1[i-1] == text2[j-1]{
                dp[i][j] = dp[i-1][j-1] +1
            }else{
                dp[i][j] = max(
                    dp[i-1][j], dp[i][j-1],
                )
            }
        }
    }
    return dp[m][n]
}

func max(a, b int) int{
    if a > b{
        return a
    }
    return b
}
```

## 53. 最大子序和

> https://leetcode-cn.com/problems/maximum-subarray/

要求连续的子数组的元素想加的最大值。
解决方案：

1. 递推出每个元素和之前元素想加的最大值，dp 的递推公式：`dp[i] = max(dp[i], dp[i]+dp[i-1])`
2. 这个值和结果进项 max，返回最大值

```go
func maxSubArray(nums []int) int {
    if len(nums) == 0{
        return -1
    }
    res := nums[0]
    for i:=1; i<len(nums);i++{
        nums[i] = max(nums[i], nums[i]+nums[i-1])
        res = max(res, nums[i])
    }
    return res
}
func max(a, b int) int{
    if a > b{
        return a
    }
    return b
}
```

## 152. 乘积最大子数组

> https://leetcode-cn.com/problems/maximum-product-subarray/

子数组相乘的最大`乘积` 分析

1. 正数相乘最大
2. 负数 x 负数也的正数
   就需要单独对两个乘积进行管理，`imin`, `imax`
   如果当前值 `nums[i] < 0`， imax, imin 互换，是因为想得到 负负得正的效果

```go
func maxProduct(nums []int) int {
    res := -1 << 63
    imax, imin := 1, 1
    for _, n := range nums{
        if n < 0{
            imin, imax = imax, imin
        }
        imax = max(n, imax * n)
        imin = min(n, imin * n)
        res = max(res, imax)
    }
    return res
}

func max(a, b int) int{
    if a > b{
        return a
    }
    return b
}
func min(a, b int) int{
    if a < b{
        return a
    }
    return b
}
```

## 120. 三角形最小路径和

> https://leetcode-cn.com/problems/triangle/

```go
func minimumTotal(triangle [][]int) int {
    m := len(triangle)
    for i:=m-2; i>=0; i--{
        for j := range triangle[i]{
            triangle[i][j] += min(
                triangle[i+1][j], triangle[i+1][j+1],
            )
        }
    }
    return triangle[0][0]
}

func min(a, b int) int{
    if  a < b{
        return a
    }
    return b
}
```
