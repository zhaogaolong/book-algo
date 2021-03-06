## 二叉树

| 问题名称                  | 难度   | 链接                                                                      |
| ------------------------- | ------ | ------------------------------------------------------------------------- |
| 144. 二叉树的前序遍历     | medium | https://leetcode-cn.com/problems/binary-tree-preorder-traversal/          |
| 94. 二叉树的中序遍历      | medium | https://leetcode-cn.com/problems/binary-tree-inorder-traversal/           |
| 145. 二叉树的后序遍历     | medium | https://leetcode-cn.com/problems/binary-tree-postorder-traversal/         |
| 102. 二叉树的层序遍历     | medium | https://leetcode-cn.com/problems/binary-tree-level-order-traversal/       |
| 429. N 叉树的层序遍历     | Medium | https://leetcode-cn.com/problems/n-ary-tree-level-order-traversal/        |
| 589. N 叉树的前序遍历     | easy   | https://leetcode-cn.com/problems/n-ary-tree-preorder-traversal/           |
| 590. N 叉树的后序遍历     | easy   | https://leetcode-cn.com/problems/n-ary-tree-postorder-traversal/          |
| 226. 翻转二叉树           | easy   | https://leetcode-cn.com/problems/invert-binary-tree/                      |
| 98. 验证二叉搜索树        | medium | https://leetcode-cn.com/problems/validate-binary-search-tree/             |
| 111. 二叉树的最小深度     | easy   | https://leetcode-cn.com/problems/minimum-depth-of-binary-tree/            |
| 104. 二叉树的最大深度     | easy   | https://leetcode-cn.com/problems/maximum-depth-of-binary-tree/            |
| 236. 二叉树的最近公共祖先 | medium | https://leetcode-cn.com/problems/lowest-common-ancestor-of-a-binary-tree/ |

## 模板

```golang
// 递归
func tree(root *Tree){
	if root == nil{
		return nil
	}

	dfs(root)
}

func dfs(root *Tree){
	if root == nil{
		return
	}
	// todo
	dfs(root.Left)
	dfs(root.Right)
}

// 迭代
func tree(root *Tree){
	if root == nil{
		return nil
	}
	res := []int{}
	stack := []*Tree{root}
	var node *Tree
	for len(stack) > 0{
		// todo
	}
	return res
}

```

## 144. 二叉树的前序遍历

迭代实现

```golang
/**
 * Definition for a binary tree node.
 * type TreeNode struct {
 *     Val int
 *     Left *TreeNode
 *     Right *TreeNode
 * }
 */
func preorderTraversal(root *TreeNode) []int {
    if root == nil{
        return nil
    }
    res := []int{}
    stack := []*TreeNode{root}
    var node *TreeNode
    for len(stack) > 0{
        node, stack = pop(stack)
        if node == nil{
            continue
        }
        res = append(res, node.Val)
        stack = append(stack,node.Right, node.Left)
    }
    return res
}

func pop(stack []*TreeNode)(*TreeNode, []*TreeNode){
    return stack[len(stack)-1], stack[:len(stack)-1]
}
```

## 145. 二叉树的后序遍历

```golang
/**
 * Definition for a binary tree node.
 * type TreeNode struct {
 *     Val int
 *     Left *TreeNode
 *     Right *TreeNode
 * }
 */
func postorderTraversal(root *TreeNode) []int {
    if root == nil{
        return nil
    }
    res := []int{}
    stack := []*TreeNode{root}
    var node *TreeNode
    for len(stack) > 0{
        node, stack = pop(stack)
        if node == nil{
            continue
        }
        res = insert(res, node.Val)
        stack = append(stack, node.Left, node.Right)
    }
    return res
}
func pop(stack []*TreeNode)(*TreeNode, []*TreeNode){
    return stack[len(stack)-1], stack[:len(stack)-1]
}
func insert(nums []int, n int) []int{
    return append([]int{n}, nums...)
}
```

## 94. 二叉树的中序遍历

```golang
/**
 * Definition for a binary tree node.
 * type TreeNode struct {
 *     Val int
 *     Left *TreeNode
 *     Right *TreeNode
 * }
*/
func inorderTraversal(root *TreeNode) []int {
    if root == nil{
        return nil
    }
    res := []int{}
    stack := []*TreeNode{}
    node := root
    for node != nil || len(stack) > 0{
        for node != nil{
            stack = append(stack, node)
            node = node.Left
        }
        node, stack = pop(stack)
        res = append(res, node.Val)
        node = node.Right
    }
    return res
}

func pop(stack []*TreeNode)(*TreeNode, []*TreeNode){
    return stack[len(stack)-1], stack[:len(stack)-1]
}
```

## 102. 二叉树的层序遍历

```golang
/**
 * Definition for a binary tree node.
 * type TreeNode struct {
 *     Val int
 *     Left *TreeNode
 *     Right *TreeNode
 * }
 */
var res [][]int
func levelOrder(root *TreeNode) [][]int {
    if root == nil{
        return nil
    }
    res = make([][]int, 0)
    dfs(root, 0)
    return res
}

func dfs(root *TreeNode, level int){
    if root == nil{
        return
    }
    if len(res) == level{
        res = append(res, []int{})
    }
    res[level] = append(res[level], root.Val)
    dfs(root.Left, level+1)
    dfs(root.Right, level+1)
}
```

## 429. N 叉树的层序遍历

```golang
/**
 * Definition for a Node.
 * type Node struct {
 *     Val int
 *     Children []*Node
 * }
 */
var res [][]int
func levelOrder(root *Node) [][]int {
    if root == nil{
        return nil
    }
    res = [][]int{}
    dfs(root, 0)
    return res
}

func dfs(root *Node,level int){
    if root == nil{
        return
    }
    if len(res) == level{
        res = append(res, []int{})
    }
    res[level] = append(res[level], root.Val)
    for i := range root.Children{
        dfs(root.Children[i], level+1)
    }
}
```

## 589. N 叉树的前序遍历

```golang
/**
 * Definition for a Node.
 * type Node struct {
 *     Val int
 *     Children []*Node
 * }
 */

func preorder(root *Node) []int {
    if root == nil{
        return nil
    }
    res := []int{}
    stack := []*Node{root}
    var node *Node
    for len(stack) > 0{
        node, stack = pop(stack)
        if node == nil{
            continue
        }
        res = append(res, node.Val)
        for i := len(node.Children) -1; i >=0; i--{
            stack = append(stack, node.Children[i])
        }
    }
    return res
}

func pop(stack []*Node)(*Node, []*Node){
    return stack[len(stack)-1], stack[:len(stack)-1]
}
```

递归实现

```golang
/**
 * Definition for a Node.
 * type Node struct {
 *     Val int
 *     Children []*Node
 * }
 */

var res []int
func preorder(root *Node) []int {
    if root == nil{
        return nil
    }
    res = []int{}
    dfs(root)
    return res
}

func dfs(root *Node){
    if root == nil{
        return
    }
    res = append(res, root.Val)
    for i := range root.Children{
        dfs(root.Children[i])
    }
}
```

## 590. N 叉树的后序遍历

```golang
/**
 * Definition for a Node.
 * type Node struct {
 *     Val int
 *     Children []*Node
 * }
 */

func postorder(root *Node) []int {
    if root == nil{
        return nil
    }
    res := []int{}
    stack := []*Node{root}
    var node *Node
    for len(stack) > 0{
        node, stack = pop(stack)
        if node == nil{
            continue
        }
        res = insert(res, node.Val)
        stack = append(stack, node.Children...)
    }
    return res
}
func insert(nums []int, n int) []int{
    return append([]int{n}, nums...)
}
func pop(stack []*Node)(*Node, []*Node){
    return stack[len(stack)-1], stack[:len(stack)-1]
}
```

## 226. 翻转二叉树

```golang
/**
 * Definition for a binary tree node.
 * type TreeNode struct {
 *     Val int
 *     Left *TreeNode
 *     Right *TreeNode
 * }
 */
func invertTree(root *TreeNode) *TreeNode {
    if root == nil{
        return nil
    }
    invertTree(root.Left)
    invertTree(root.Right)
    root.Left, root.Right = root.Right, root.Left
    return root
}
```

## 98. 验证二叉搜索树

```golang
/**
 * Definition for a binary tree node.
 * type TreeNode struct {
 *     Val int
 *     Left *TreeNode
 *     Right *TreeNode
 * }
 */
var min *TreeNode
func isValidBST(root *TreeNode) bool {
    if root == nil{
        return true
    }
    min = &TreeNode{Val: -1 << 63}
    return dfs(root)
}

func dfs(root *TreeNode) bool{
    if root == nil{
        return true
    }
    if !dfs(root.Left) || root.Val <= min.Val{
        return false
    }
    min = root
    return dfs(root.Right)
}
```

## 111. 二叉树的最小深度

注意：如果树的一个子节点为空，另一个子节点不为空的话，前一个子节点就不能算。
例子：

```
1
 \
  3
   \
    5
     \
      8
```

```golang

/**
 * Definition for a binary tree node.
 * type TreeNode struct {
 *     Val int
 *     Left *TreeNode
 *     Right *TreeNode
 * }
 */
func minDepth(root *TreeNode) int {
    if root == nil{
        return 0
    }
    l := minDepth(root.Left)
    r := minDepth(root.Right)
    if root.Left == nil{
        return r +1
    }else if root.Right == nil{
        return l +1
    }else{
        return min(l, r ) +1
    }
}

func min(a, b int) int{
    if a < b{
        return a
    }
    return b
}

```

##

```golang
/**
 * Definition for a binary tree node.
 * type TreeNode struct {
 *     Val int
 *     Left *TreeNode
 *     Right *TreeNode
 * }
 */
func maxDepth(root *TreeNode) int {
    if root == nil{
        return 0
    }
    l := maxDepth(root.Left)
    r := maxDepth(root.Right)
    return max(l, r) +1
}
func max(a, b int) int{
    if a > b{
        return a
    }
    return b
}

```

##

```golang
/**
 * Definition for TreeNode.
 * type TreeNode struct {
 *     Val int
 *     Left *ListNode
 *     Right *ListNode
 * }
 */
func lowestCommonAncestor(root, p, q *TreeNode) *TreeNode {
    if root == nil{
       return nil
    }
    if p.Val == root.Val || q.Val == root.Val{
        return root
    }
    l := lowestCommonAncestor(root.Left, p,q)
    r := lowestCommonAncestor(root.Right, p,q)
    if l == nil{
        return r
    }else if r == nil{
        return l
    }
    return root
}
```
