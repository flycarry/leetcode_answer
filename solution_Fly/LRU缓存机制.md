#### 题目：

运用你所掌握的数据结构，设计和实现一个  [LRU (最近最少使用) 缓存机制](https://baike.baidu.com/item/LRU)。它应该支持以下操作： 获取数据 `get` 和 写入数据 `put` 。

获取数据 `get(key)` - 如果密钥 (key) 存在于缓存中，则获取密钥的值（总是正数），否则返回 -1。
写入数据 `put(key, value)` - 如果密钥不存在，则写入其数据值。当缓存容量达到上限时，它应该在写入新数据之前删除最近最少使用的数据值，从而为新的数据值留出空间。

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

主要考虑如何解决缓存过期问题和数据存储对应关系，在这里，我使用了map作为主要存储机制

#### 解答：

```go
type LRUCache struct {
	m   map[int]*Value
	l   []int
	cap int
	now int
}
type Value struct {
	life, index int
}

func Constructor(capacity int) LRUCache {
	return LRUCache{make(map[int]*Value), []int{}, capacity, 1}
}

func (this *LRUCache) Get(key int) int {
	if this.m[key]==nil{
		return -1
	} else {
		this.m[key].life = this.now
		this.now++
        
		return this.l[this.m[key].index]
	}
}

func (this *LRUCache) Put(key int, value int) {
	if this.m[key]==nil{
		if len(this.l) < this.cap {
			this.l = append(this.l, value)
			this.m[key] = &Value{this.now , len(this.l) - 1}
			this.now++
			return
		} else {
			mink,minv:=0,&Value{this.now,-1}
			for k, v := range this.m {
				if v!=nil&&v.life<=minv.life{
					minv=v
					mink=k
				}
			}
			this.l[minv.index]=value
			this.m[key]=&Value{this.now,minv.index}
			this.m[mink]=nil
			this.now++
		}
	}else {
		this.m[key].life=this.now
		this.l[this.m[key].index]=value
		this.now++
	}
}
```

