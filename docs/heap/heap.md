# 堆 heap

| 问题名称                     | 链接                                                       |
| ---------------------------- | ---------------------------------------------------------- |
| 剑指 offer 40. 最小的 k 个数 | https://leetcode-cn.com/problems/zui-xiao-de-kge-shu-lcof/ |
| 239. 滑动窗口最大值          | https://leetcode-cn.com/problems/sliding-window-maximum/   |

## 剑指 offer 40. 最小的 k 个数

> https://leetcode-cn.com/problems/zui-xiao-de-kge-shu-lcof/

```go
type IntHeap struct{
    sort.IntSlice
}

func (h IntHeap) Less(i, j int) bool{
    return h.IntSlice[i] < h.IntSlice[j]
}

func (h *IntHeap)Push(v interface{}){
    h.IntSlice = append(h.IntSlice, v.(int))
}

func (h *IntHeap) Pop() interface{}{
    item := h.IntSlice[len(h.IntSlice)-1]
    h.IntSlice = h.IntSlice[:len(h.IntSlice)-1]
    return item
}

func getLeastNumbers(arr []int, k int) []int {
    h := IntHeap{}
    heap.Init(&h)
    for i := range arr{
        heap.Push(&h, arr[i])
    }
    res := []int{}
    for i:=0; i<k; i++{
        res = append(res, heap.Pop(&h).(int))
    }
    return res
}
```

## 239. 滑动窗口最大值

> https://leetcode-cn.com/problems/sliding-window-maximum/

使用双端队列

```go
func maxSlidingWindow(nums []int, k int) []int {
    q := []int{}
    push := func(i int){
        // for 循环能保证第 0 个位置的下标一直是最大的
        for len(q) > 0 && nums[i] >= nums[q[len(q)-1]]{
            q = q[:len(q)-1]
        }
        q = append(q, i)
    }
    for i:=0; i<k; i++{
        push(i)
    }
    n := len(nums)
    ans := []int{nums[q[0]]}
    for i:=k; i<n; i++{
        push(i)
        // 如果该下标已经不再活动窗口里了，就直接删除它
        for q[0] <= i-k{
            q = q[1:]
        }
        ans = append(ans, nums[q[0]])
    }
    return ans
}
```
