#### 题目：

在未排序的数组中找到第 **k** 个最大的元素。请注意，你需要找的是数组排序后的第 k 个最大的元素，而不是第 k 个不同的元素。

**示例 1:**

```
输入: [3,2,1,5,6,4] 和 k = 2
输出: 5
```

**示例 2:**

```
输入: [3,2,3,1,2,4,5,5,6] 和 k = 4
输出: 4
```

**说明:**

你可以假设 k 总是有效的，且 1 ≤ k ≤ 数组的长度。

#### 解析：

明显，这是一道快速排序的应用。

#### 解答：

```go
func findKthLargest(nums []int, k int) int {
	left, right := 0, len(nums)-1
	return find_quick2(nums, left, right, k)
}
func find_quick2(nums []int, l, r, k int) int {
	ll, lll, rr := l+1, l, r
	if l >= r {
		return nums[l]
	}
	t := nums[l]
	for ll <= rr {
		if nums[ll] < t {
			nums[ll],nums[rr] = nums[rr],nums[ll]
			rr--
		} else if nums[ll] > t {
			nums[lll] ,nums[ll]= nums[ll],nums[lll]
			lll++
			ll++
		} else {
			ll++
		}
	}
	if k > lll&&k<=ll {
		return nums[lll]
	}
	if k < lll+1 {
		return find_quick2(nums, l, lll-1, k)
	} else {
		return find_quick2(nums, rr+1, r, k)
	}
}
```

