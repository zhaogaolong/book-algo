# hash table

| 问题名称              | 链接                                               |
| --------------------- | -------------------------------------------------- |
| 1. 两数之和           | https://leetcode-cn.com/problems/two-sum/          |
| 242. 有效的字母异位词 | https://leetcode-cn.com/problems/valid-anagram/    |
| 49. 字母异位词分组    | https://leetcode-cn.com/problems/group-anagrams/   |
| 169. 多数元素         | https://leetcode-cn.com/problems/majority-element/ |

## 1 两之和

```golang
func twoSum(nums []int, target int) []int {
    data := map[int]int{}
    for i, v := range nums{
        n := target - v
        j, ok := data[n]
        if ok{
            return []int{j, i}
        }
        data[v] = i
    }
    return nil
}
```

## 242. 有效的字母异位词

异位词： 两个单词的字母数量是否相同

```golang
func isAnagram(s string, t string) bool {
    if len(s) != len(t){
        return false
    }
    data := map[byte]int{}
    for i := range s{
        data[s[i]]++
        data[t[i]]--
    }

    for _, v := range data{
        if v != 0{
            return false
        }
    }
    return true
}
```

## 49. 字母异位词分组

相同的移位词分到一个组里

原理：
首先把每个单词使用 sort 排序作为 key 放到 map 里， 然后 append 进去

```golang
func groupAnagrams(strs []string) [][]string {
    kind := map[string][]string{}
    for _, s := range strs{
        v := []byte(s)
        sort.Slice(v, func(i, j int) bool{ return v[i] < v[j] })  // sort key
        kind[string(v)] = append(kind[string(v)], s)
    }
    res := [][]string{}
    for _, v := range kind{
        res = append(res, v)
    }
    return res
}
```

## 169. 多数元素

是指该元素在这个数组中出现的次数超过数组的 50%

```golang
func majorityElement(nums []int) int {
    data := map[int]int{}
    for _, v := range nums{
        data[v]++
    }
    half := len(nums)/2
    for num,count := range data{
        if count > half{
            return num
        }
    }
    return -1
}
```
