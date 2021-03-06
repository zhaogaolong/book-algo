# 递归问题

| 问题名称         | 链接                                                   |
| ---------------- | ------------------------------------------------------ |
| 22. 括号生成     | https://leetcode-cn.com/problems/generate-parentheses/ |
| 79. 单词搜索     | https://leetcode-cn.com/problems/word-search/          |
| 212. 单词搜索 II | https://leetcode-cn.com/problems/word-search-ii/1      |

## 22. 括号生成

问题： 生成合法的括号，长度为 n， 不可重复
example: n=3, `["((()))","(()())","(())()","()(())","()()()"]`

```go
var res []string
func generateParenthesis(n int) []string {
    if n < 1{
        return nil
    }

    res = []string{}
    dfs("", 0, 0, n)
    return res
}

func dfs(temp string, l, r, n int){
    if r == n && l == n{
        res = append(res, temp)
        return
    }

    if l < n{
        dfs(temp +"(", l+1, r, n)
    }
    if r < l{ // 注意，这里的 r < l
        dfs(temp +")", l, r+1, n)
    }
}
```

## 79. 单词搜索

> https://leetcode-cn.com/problems/word-search

单词是否在矩阵中

```go
func exist(board [][]byte, word string) bool {
    for i := range board{
        for j := range board[0]{
            if dfs(board, word, i, j){
                return true
            }
        }
    }
    return false
}

func dfs(board [][]byte, word string, i, j int) bool{
    // 1. 数据完了
    if len(word) == 0{
        return true
    }

    // 2. 是否越界
    if i<0 || j<0 || i >= len(board) || j >= len(board[i]){
        return false
    }

    // 3. 字母不匹配
    if word[0] != board[i][j]{
        return false
    }
    // 4. 匹配到了， 需要把 当前字母重置
    tmp := board[i][j]
    board[i][j] = ' '
    if dfs(board, word[1:], i-1, j){return true}
    if dfs(board, word[1:], i+1, j){return true}
    if dfs(board, word[1:], i, j-1){return true}
    if dfs(board, word[1:], i, j+1){return true}
    board[i][j] = tmp
    return false
}
```

## 212. 单词搜索 II

> https://leetcode-cn.com/problems/word-search-ii/

```go
func findWords(board [][]byte, words []string) []string {
    res := []string{}
    for i := range words{
        if exist(board, words[i]){
            res = append(res, words[i])
        }
    }
    return res
}

func exist(board [][]byte, word string)bool{
    for i := range board{
        for j := range board[0]{
            if dfs(board, word, i, j){
                return true
            }
        }
    }
    return false
}

func dfs(board [][]byte, word string, i, j int) bool{
    if len(word) == 0{
        return true
    }
    if i < 0 || j < 0 || i >= len(board) ||j >= len(board[i]){
        return false
    }
    if word[0] != board[i][j]{
        return false
    }
    temp := board[i][j]
    board[i][j] = byte(' ')
    defer func(){
        board[i][j] = temp
    }()
    if dfs(board, word[1:], i-1, j){return true}
    if dfs(board, word[1:], i+1, j){return true}
    if dfs(board, word[1:], i, j-1){return true}
    if dfs(board, word[1:], i, j+1){return true}
    return false
}
```

### 优化 1

使用 trie 树进行优化

```go
var set map[string]bool
func findWords(board [][]byte, words []string) []string {
    root := buildTrie(words)
    set = map[string]bool{}
    for i := range board{
        for j := range board[i]{
            dfs(board, root, i, j)
        }
    }
    res := []string{}
    for i := range set{
        res = append(res, i)
    }
    return res
}

func dfs(board [][]byte, root *Trie, i, j int){
    if i<0 || j<0 || i >= len(board) || j >= len(board[i]){
        return
    }
    v := board[i][j]
    sub, ok := root.sub[v]
    if !ok{
        return
    }
    if sub.isword{
        set[sub.word] = true
    }

    tmp := board[i][j]
    board[i][j] = ' '
    dfs(board, sub, i-1, j)
    dfs(board, sub, i+1, j)
    dfs(board, sub, i, j-1)
    dfs(board, sub, i, j+1)
    board[i][j] = tmp
}

type Trie struct{
    sub map[byte]*Trie
    isword bool
    word string
}
func newTrie() *Trie{
    return &Trie{ sub: make(map[byte]*Trie)}
}

func buildTrie(words []string) *Trie{
    root := newTrie()
    for _, str := range words{
        p := root
        for i := range str{
            sub, ok := p.sub[str[i]]
            if ok{
                p = sub
            }else{
                p.sub[str[i]] = newTrie()
                p = p.sub[str[i]]
            }
        }
        p.isword = true
        p.word = str
    }
    return root
}
```
