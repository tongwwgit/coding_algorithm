### 实现LRU(Least Recently Used，最少最近使用)缓存机制：我们认为最近使用过的数据应该是是「有用的」，很久都没用过的数据应该是无用的，内存满了就优先删那些很久没用过的数据。[leetcode 146](https://leetcode-cn.com/problems/lru-cache/)
实现 LRUCache 类：时间复杂度O(1)

* LRUCache(int capacity) 以正整数作为容量 capacity 初始化 LRU 缓存
* int get(int key) 如果关键字 key 存在于缓存中，则返回关键字的值，否则返回 -1 。
* void put(int key, int value) 如果关键字已经存在，则变更其数据值；如果关键字不存在，则插入该组「关键字-值」。当缓存容量达到上限时，它应该在写入新数据之前删除最久未使用的数据值，从而为新的数据值留出空间。
```python
 class LRUCache:
     def __init__(self, capacity: int):
     
     def get(self, key: int) -> int:
     
     def put(self, key: int, value: int) -> None:
```
#### 解法：核心数据结构就是哈希链表，双向链表和哈希表的结合体。哈希表可以保证查找，链表用来插入删除
```python
class ListNode:  #需要pre和next节点，便于插入和删除
    def __init__(self, key=None, val=None):
        self.key = key #需要key的原因是删除node后需要删除hashmap的key
        self.val = val
        self.pre = None
        self.next = None

class DoubleList: #双向链表, 操作对象是节点
    def __init__(self, size):
        self.size = size
        self.head = ListNode()
        self.tail = ListNode()
        self.head.next = self.tail
        self.tail.pre = self.head
    
    #尾部添加节点，尾部是最新的
    def addlast(self, node):
        node.pre = self.tail.pre
        node.next = self.tail
        self.tail.pre.next = node
        self.tail.pre = node
        self.size = self.size+1

    #移除节点，节点肯定存在
    def remove(self, node):
        node.pre.next = node.next
        node.next.pre = node.pre
        self.size = self.size-1

    ##移除最久未使用的节点
    def removefist(self):
        if self.head.next == self.tail:
            return None
        first = self.head.next
        self.remove(first)
        return first

class LRUCache: # 操作对象是 key value

    def __init__(self, capacity: int):
        self.capacity = capacity
        self.hashmap = {} ## key => node
        self.cache = DoubleList(0)

    def makerecently(self, key): #将某个 key 提升为最近使用的
        node = self.hashmap.get(key)
        self.cache.remove(node)
        self.cache.addlast(node)
      
    def addrecently(self, key, val): #添加最近使用的元素
        node = ListNode(key, val)
        self.cache.addlast(node)
        self.hashmap[key]=node

    def deletekey(self, key): #删除某一个key
        node = self.hashmap.get(key)
        self.cache.remove(node)
        self.hashmap.pop(key)

    def removelongest(self): #删除最久未使用的
        node = self.cache.removefist()
        self.hashmap.pop(node.key)

        
    def get(self, key: int) -> int:
        if key not in self.hashmap:
            return -1
        self.makerecently(key)
        return self.hashmap[key].val


    def put(self, key: int, value: int) -> None:
        if key in self.hashmap:
            self.deletekey(key)
            self.addrecently(key,value)
            return

        if self.cache.size == self.capacity:
            self.removelongest()
        self.addrecently(key, value)



# Your LRUCache object will be instantiated and called as such:
# obj = LRUCache(capacity)
# param_1 = obj.get(key)
# obj.put(key,value)
```
