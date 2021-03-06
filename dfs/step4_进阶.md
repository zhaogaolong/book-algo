# 递归问题

| 问题名称          | 链接                                                       |
| ----------------- | ---------------------------------------------------------- |
| 433. 最小基因变化 | https://leetcode-cn.com/problems/minimum-genetic-mutation/ |

## 433. 最小基因变化

> https://leetcode-cn.com/problems/minimum-genetic-mutation/

```go
var dna = map[byte][]string{
    'A': []string{"C", "G", "T"},
    'C': []string{"A", "G", "T"},
    'G': []string{"A", "C", "T"},
    'T': []string{"A", "C", "G"},
}
var (
    minCount int
    used []bool
)
func minMutation(start string, end string, bank []string) int {
    if idx := isIn(bank, end); idx == -1{
        return -1
    }

    minCount = len(bank)+1
    used = make([]bool, len(bank))
    dfs(start, end, bank, 0)
    if minCount <= len(bank){
        return minCount
    }
    return -1
}

func dfs(start, end string, bank []string, count int){
    if count > minCount{
        return
    }
    if start == end{
        if count < minCount{
            minCount = count
        }
        return
    }
    for i := range start{
        v := start[i]
        for _, n := range dna[v]{
            next := start[:i] + n + start[i+1:]
            idx := isIn(bank, next)
            if idx != -1 && !used[idx]{
                used[idx] = true
                dfs(next, end, bank, count+1)
                used[idx] = false
            }
        }
    }
}

func isIn(bank []string, s string) int{
    for i, v := range bank{
        if v == s{
            return i
        }
    }
    return -1
}
```

## 200. 岛屿数量

> https://leetcode-cn.com/problems/number-of-islands/

```go
func numIslands(grid [][]byte) int {
    count := 0
    for i := range grid{
        for j := range grid[i]{
            if grid[i][j] == '1'{
                count++
                dfs(grid, i, j)
            }
        }
    }
    return count
}

func dfs(grid [][]byte, i, j int){
    if i<0 || j<0 || i>= len(grid) || j>= len(grid[i]){
        return
    }
    if grid[i][j] != '1'{
        return
    }
    grid[i][j] = '0'
    dfs(grid, i+1, j)
    dfs(grid, i-1, j)
    dfs(grid, i, j+1)
    dfs(grid, i, j-1)
}
```
