#### 题目：

运用你所掌握的数据结构，设计和实现一个 [LRU (最近最少使用) 缓存机制](https://baike.baidu.com/item/LRU)。它应该支持以下操作： 获取数据 `get` 和 写入数据 `put` 。

获取数据 `get(key)` - 如果密钥 (key) 存在于缓存中，则获取密钥的值（总是正数），否则返回 -1。 写入数据 `put(key, value)` - 如果密钥不存在，则写入其数据值。当缓存容量达到上限时，它应该在写入新数据之前删除最近最少使用的数据值，从而为新的数据值留出空间。

**进阶:**

你是否可以在 **O(1)** 时间复杂度内完成这两种操作？

**示例:**

```
LRUCache cache = new LRUCache( 2 /* 缓存容量 */ );

cache.put(1, 1);
cache.put(2, 2);
cache.get(1);       // 返回  1
cache.put(3, 3);    // 该操作会使得密钥 2 作废
cache.get(2);       // 返回 -1 (未找到)
cache.put(4, 4);    // 该操作会使得密钥 1 作废
cache.get(1);       // 返回 -1 (未找到)
cache.get(3);       // 返回  3
cache.get(4);       // 返回  4
```

#### 解析：

这题我想了很久，首先存储肯定用map，然后就是LRU的判定，需要一个rank，并且每次get put操作都要更新，如果使用slice作为rank的话，无法实现O(1)的时间复杂度，所以我决定使用list.List，然后map的value是list.Element，这样就可以直接通过map访问到ele继而move，删除，或者添加。然后还需要通过ele来访问key，这是为了确定LRU删除的key，所以我的list.Element的Value是[]int，分别是key和val

#### 解答：

```go
type LRUCache struct {
	storage map[int]*list.Element
	rank *list.List
	cap int
}

func Constructor(capacity int) LRUCache {
	return LRUCache{
		storage: make(map[int]*list.Element),
		rank: list.New(),
		cap: capacity,
	}
}

func (this *LRUCache) Get(key int) int {
	ele, ok := this.storage[key]
	if !ok {
		return -1
	}
	// 更改排名
	this.rank.MoveToFront(ele)
	return ele.Value.([]int)[1]
}

func (this *LRUCache) Put(key int, val int) {
	// 是否有这个key
	if ele, ok := this.storage[key]; !ok {
		if len(this.storage) >= this.cap {
			// 满了删除
			ele := this.rank.Back()
			this.rank.Remove(ele)
			delete(this.storage, ele.Value.([]int)[0])
		}
		// 放新的
		ele := this.rank.PushFront([]int{key, val})
		this.storage[key] = ele
	} else {
		ele.Value.([]int)[1] = val
		this.rank.MoveToFront(ele)
	}
}
```

