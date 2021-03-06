# 递归问题

| 问题名称        | 链接                                                      |
| --------------- | --------------------------------------------------------- |
| 78. 子集        | https://leetcode-cn.com/problems/subsets/                 |
| 90. 子集 II     | https://leetcode-cn.com/problems/subsets-ii/              |
| 46. 全排列      | https://leetcode-cn.com/problems/permutations/            |
| 47. 全排列 II   | https://leetcode-cn.com/problems/permutations-ii/         |
| 39. 组合总和    | https://leetcode-cn.com/problems/combination-sum/         |
| 40. 组合总和 II | https://leetcode-cn.com/problems/combination-sum-ii/      |
| 131. 分割回文串 | https://leetcode-cn.com/problems/palindrome-partitioning/ |

## 78. 子集

> https://leetcode-cn.com/problems/subsets/

```go
var res [][]int
func subsets(nums []int) [][]int {
    res = make([][]int, 0)
    dfs([]int{}, nums, 0)
    return res
}

func dfs(temp, nums []int, start int){
    res = append(res, append([]int{}, temp...))
    for i:=start;i<len(nums);i++{
        dfs(append(temp, nums[i]), nums, i+1)
    }
}
```

## 90. 子集 II

> https://leetcode-cn.com/problems/subsets-ii/

```go
var res [][]int
func subsets(nums []int) [][]int {
    sort.Ints(nums)
    res = make([][]int, 0)
    dfs([]int{}, nums, 0)
    return res
}

func dfs(temp, nums []int, start int){
    res = append(res, append([]int{}, temp...))
    for i:=start;i<len(nums);i++{
        if i > start && nums[i] == nums[i-1]{
            continue
        }
        dfs(append(temp, nums[i]), nums, i+1)
    }
}
```

## 46. 全排列

> https://leetcode-cn.com/problems/permutations/

```go
var res [][]int
func permute(nums []int) [][]int {
    if len(nums) == 0{
        return nil
    }
    res = make([][]int, 0)
    dfs([]int{}, nums)
    return res
}

func dfs(temp, nums []int){
    if len(temp) == len(nums) {
        res = append(res, append([]int{}, temp...))
        return
    }
    for i := range nums{
        if numExist(temp, nums[i]){
            continue
        }
        dfs(append(temp, nums[i]), nums)
    }
}

func numExist(nums []int, n int) bool{
    for _, v := range nums{
        if v == n{
            return true
        }
    }
    return false
}
```

## 47. 全排列 II

> https://leetcode-cn.com/problems/permutations-ii/

```go
var res [][]int
func permuteUnique(nums []int) [][]int {
    if len(nums) == 0{
        return nil
    }
    sort.Ints(nums)
    res = make([][]int, 0)
    used := make([]bool, len(nums))
    dfs([]int{}, nums, used)
    return res
}

func dfs(temp, nums []int, used []bool){
    if len(temp) == len(nums){
        res = append(res, append([]int{}, temp...))
        return
    }
    for i := range nums{
        if used[i]{
            continue
        }
        if i > 0 && nums[i] == nums[i-1] && used[i-1]{
            continue
        }
        used[i] = true
        dfs(append(temp, nums[i]), nums, used)
        used[i] = false
    }
}
```

## 39. 组合总和

> https://leetcode-cn.com/problems/combination-sum/

```go
var res [][]int
func combinationSum(candidates []int, target int) [][]int {
    res = make([][]int, 0)
    dfs([]int{}, candidates, target, 0)
    return res
}

func dfs(temp, nums []int, target, start int){
    if target < 0{
        return
    }else if target == 0{
        res = append(res, append([]int{}, temp...))
        return
    }
    for i:=start; i<len(nums);i++{
        v := target - nums[i]
        dfs(append(temp, nums[i]), nums, v, i)
    }
}
```

## 40. 组合总和 II

> https://leetcode-cn.com/problems/combination-sum-ii/

```go
var res [][]int
func combinationSum2(candidates []int, target int) [][]int {
    if len(candidates) == 0{
        return nil
    }
    sort.Ints(candidates)
    res = make([][]int, 0)
    dfs([]int{}, candidates, target, 0)
    return res
}

func dfs(temp, nums []int, target, start int){
    if  target < 0{
        return
    }else if target == 0{
        res = append(res, append([]int{}, temp...))
        return
    }
    for i:=start; i<len(nums);i++{
        if i > start && nums[i-1] == nums[i]{
            continue
        }
        v := target - nums[i]
        dfs(append(temp, nums[i]), nums, v, i+1)
    }
}
```

## 131. 分割回文串

> https://leetcode-cn.com/problems/palindrome-partitioning/

```go
var res [][]string
func partition(s string) [][]string {
    if len(s) == 0{
        return nil
    }
    res = make([][]string, 0)
    dfs([]string{}, s, 0)
    return res
}

func dfs(temp []string, s string, start int){
    if start == len(s) {
        res = append(res, append([]string{}, temp...))
        return
    }
    for i:=start; i<len(s);i++{
        v := s[start:i+1]
        if isok(v){
            dfs(append(temp, v), s, i+1)
        }
    }
}

// 子串是否是回文
func isok(s string) bool{
    for i, j := 0, len(s)-1; i<j; i,j = i+1,j-1{
        if s[i] != s[j]{
            return false
        }
    }
    return true
}
```
