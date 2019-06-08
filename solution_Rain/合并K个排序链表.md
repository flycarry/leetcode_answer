### 题目：

合并 k 个排序链表，返回合并后的排序链表。请分析和描述算法的复杂度。

#### 示例:

**输入**:
[
  1->4->5,
  1->3->4,
  2->6
]
**输出**: 1->1->2->3->4->4->5->6

### 解析：

借助合并两个有序链表的题目，我们只要尽力减少合并次数就可以了，使用分治法，最后的时间复杂度为：O(logk*len)

### 解答：

```go
func mergeKLists(lists []*ListNode) *ListNode {
    if len(lists) == 0{
        return nil
    }
	if len(lists) == 1 {
		return lists[0]
	}
	left, right := lists[0], lists[1]
	if len(lists) > 2 {
		left = mergeKLists(lists[:len(lists)/2])
		right = mergeKLists(lists[len(lists)/2:])
	}
	return mergeTwoLists(left, right)
}

func mergeTwoLists(l1 *ListNode, l2 *ListNode) *ListNode {
	head := &ListNode{0, nil}
	cur := head
	for l1 != nil && l2 != nil {
		if l1.Val < l2.Val {
			cur.Next = l1
			l1 = l1.Next
		} else {
			cur.Next = l2
			l2 = l2.Next
		}
		cur = cur.Next
	}
	last := l1
	if l2 != nil {
		last = l2
	}
	cur.Next = last
	return head.Next
}
```

