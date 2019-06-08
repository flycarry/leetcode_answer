#### 题目：

合并 k 个排序链表，返回合并后的排序链表。请分析和描述算法的复杂度。

示例:

输入:
[
  1->4->5,
  1->3->4,
  2->6
]
输出: 1->1->2->3->4->4->5->6

#### 解析：

我准备使用优先队列方式解答，但是看起来递归分治算法更加好写，就暂时使用了递归。堆排序后面补上。

#### 解答：

```go
/**
 * Definition for singly-linked list.
 * type ListNode struct {
 *     Val int
 *     Next *ListNode
 * }
 */

func mergeKLists(lists []*ListNode) *ListNode {
	if len(lists)==0{
		return nil
	}else if len(lists)==1{
		return lists[0]
	}else {
		left, right := lists[0], lists[1]
		if len(lists) > 2 {
			left = mergeKLists(lists[:len(lists)/2])
			right = mergeKLists(lists[len(lists)/2:])
		}
		return mergeTwoList(left, right)
	}
}

func mergeTwoList(l1 *ListNode, l2 *ListNode) *ListNode {
	head:=&ListNode{}
	node:=&ListNode{}
	head.Next=node
	for l1!=nil&&l2!=nil{
		if l1.Val>l2.Val{
			node.Next=l2
			l2=l2.Next
		}else {
			node.Next=l1
			l1=l1.Next
		}
		node=node.Next
	}
	if l1==nil{
		node.Next=l2
	}else {
		node.Next=l1
	}
	return head.Next.Next
}
```

