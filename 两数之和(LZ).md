## 题目：

给定一个整数数组 nums 和一个目标值 target，请你在该数组中找出和为目标值的那 两个 整数，并返回他们的数组下标。

你可以假设每种输入只会对应一个答案。但是，你不能重复利用这个数组中同样的元素。

## 示例:

给定 nums = [2, 7, 11, 15], target = 9

因为 nums[0] + nums[1] = 2 + 7 = 9  

所以返回 [0, 1]


### 解答（L）：
```go
func twoSum(nums []int, target int) []int {
	m:=make(map[int]int)
	for i,j:=range nums{
		if m[j]!=0{
			return []int{m[j]-1,i}
		}else {
			m[target-j]=i+1
		}
	}
	return []int{0,0}
}
```

### 解答（Z）：
```go
func twoSum(nums []int, target int) []int {
	m:=make(map[int]int)
	for i,j:=range nums{
		if m[j]!=0{
			return []int{m[j]-1,i}
		}else {
			m[target-j]=i+1
		}
	}
	return []int{0,0}
}
```

### 解析（L）：

这个题目就是简单的使用map作为存储结构来讲遍历过的数据记录下来，使得数组在一次遍历的情况下就完成。


### 结论：

时间复杂度: o(n)
空间复杂度: o(n)