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

这个题要借助快排的思想和二分查找的思想，注意在快排的时候大的要往前放。这样才能找到第K个大的元素，如果给的例子前面没有重复的话

#### 解答：

```go
func findKthLargest(nums []int, k int) int {
	return quick(nums, 0, len(nums)-1, k)
}

func quick(nums []int, left, right, k int) int {
	index := partition(nums, left, right)
	switch {
	case index+1 == k:
		return nums[index]
	case index+1 < k:
		return quick(nums, index+1, right, k)
	default:
		return quick(nums, left, index-1, k)
	}
}

func partition(nums []int, left, right int) int {
    if left == right {
		return left
	}
	index, left := left, left+1
	for left < right {
		switch {
		case nums[left] > nums[index]:
			left++
		case nums[left] <= nums[index]:
			nums[left], nums[right] = nums[right], nums[left]
			right--
		}
	}
	if nums[left] <= nums[index] {
		left--
	}
	nums[index], nums[left] = nums[left], nums[index]
	return left
}
```