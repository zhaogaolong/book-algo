## Trie 树

| 问题名称                | 难度 | 链接                                                         |
| ----------------------- | ---- | ------------------------------------------------------------ |
| 208. 实现 Trie (前缀树) | m    | https://leetcode-cn.com/problems/implement-trie-prefix-tree/ |

## 208. 实现 Trie (前缀树)

> https://leetcode-cn.com/problems/implement-trie-prefix-tree/

思路： 构建一个前缀树

```go
type Trie struct {
    sub map[byte]*Trie
    isWrod bool
}


/** Initialize your data structure here. */
func Constructor() Trie {
    return Trie{ sub: make(map[byte]*Trie) }
}


/** Inserts a word into the trie. */
func (this *Trie) Insert(word string)  {
    root := this
    for i := range word{
        v := word[i]
        sub, ok := root.sub[v]
        if ok{
            root = sub
        }else{
            sub := Constructor()
            root.sub[v] = &sub
            root = root.sub[v]
        }
    }
    root.isWrod = true
}


/** Returns if the word is in the trie. */
func (this *Trie) Search(word string) bool {
    exist, ok := this.search(word)
    return exist && ok
}

/** Returns if there is any word in the trie that starts with the given prefix. */
func (this *Trie) StartsWith(prefix string) bool {
    _, ok := this.search(prefix)
    return ok
}

func (this *Trie) search(word string)(bool, bool){
    root := this
    for i := range word{
        v := word[i]
        sub, ok := root.sub[v]
        if !ok{
            return false, false
        }
        root = sub
    }
    return root.isWrod, true
}

/**
 * Your Trie object will be instantiated and called as such:
 * obj := Constructor();
 * obj.Insert(word);
 * param_2 := obj.Search(word);
 * param_3 := obj.StartsWith(prefix);
 */
```
