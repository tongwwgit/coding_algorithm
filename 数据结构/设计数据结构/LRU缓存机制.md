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
### 解法：核心数据结构就是哈希链表，双向链表和哈希表的结合体。哈希表可以保证查找，链表用来插入删除.
* 哈希表： key： node
* 双向链表操作对象是node： 
* cache操作对象是key和vlaue
#### 1. 构造ListNode和Doublelist对象，代码过于复杂
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

#### 2. python自带的OrderdedDict结构
```python
class LRUCache(collections.OrderedDict):

    def __init__(self, capacity: int):
        super().__init__()
        self.capacity = capacity


    def get(self, key: int) -> int:
        if key not in self:
            return -1
        self.move_to_end(key)
        return self[key]

    def put(self, key: int, value: int) -> None:
        if key in self:
            self.move_to_end(key)
        self[key] = value
        if len(self) > self.capacity:
            self.popitem(last=False)
```
#### 3. 更简洁的表示方法
```python
哈希表：key-> ListNode
需要记录访问的先后顺序, 将最近访问的移动到末尾，双向链表法
class ListNode:
    def __init__(self, key=None, value=None):
        self.key = key
        self.value = value
        self.prev = None
        self.next = None


class LRUCache:
    def __init__(self, capacity: int):
        self.capacity = capacity
        self.hashmap = {}
        # 新建两个节点 head 和 tail
        self.head = ListNode()
        self.tail = ListNode()
        # 初始化链表为 head <-> tail
        self.head.next = self.tail
        self.tail.prev = self.head

    # 因为get与put操作都可能需要将双向链表中的某个节点移到末尾，所以定义一个方法
    def move_node_to_tail(self, key):
            # 先将哈希表key指向的节点拎出来，为了简洁起名node
            #      hashmap[key]                               hashmap[key]
            #           |                                          |
            #           V              -->                         V
            # prev <-> node <-> next         pre <-> next   ...   node
            node = self.hashmap[key]
            node.prev.next = node.next
            node.next.prev = node.prev
            # 之后将node插入到尾节点前
            #                 hashmap[key]                 hashmap[key]
            #                      |                            |
            #                      V        -->                 V
            # prev <-> tail  ...  node                prev <-> node <-> tail
            node.prev = self.tail.prev
            node.next = self.tail
            self.tail.prev.next = node
            self.tail.prev = node

    def get(self, key: int) -> int:
        if key in self.hashmap:
            # 如果已经在链表中了就把它移到末尾（变成最新访问的）
            self.move_node_to_tail(key)
            node=self.hashmap[key]
            return node.value
        else:
            return -1


    def put(self, key: int, value: int) -> None:
        if key in self.hashmap:
            # 如果key本身已经在哈希表中了就不需要在链表中加入新的节点
            # 但是需要更新字典该值对应节点的value
            self.hashmap[key].value = value
            # 之后将该节点移到末尾
            self.move_node_to_tail(key)
        else:
            if len(self.hashmap) == self.capacity:
                # 去掉哈希表对应项
                self.hashmap.pop(self.head.next.key)
                # 去掉最久没有被访问过的节点，即头节点之后的节点
                self.head.next = self.head.next.next
                self.head.next.prev = self.head
            # 如果不在的话就插入到尾节点前
            new = ListNode(key, value)
            self.hashmap[key] = new
            new.prev = self.tail.prev
            new.next = self.tail
            self.tail.prev.next = new
            self.tail.prev = new
```
#### 4. 构造ListNode和Doublelist对象，简化了1的代码
```python
class ListNode:
    def __init__(self, key=None, value= None):
        self.key = key
        self.value = value
        self.pre = None
        self.next = None

class DoubleList: #双向链表
    def __init__(self):
        self.head = ListNode()
        self.tail = ListNode()
        self.head.next = self.tail
        self.tail.pre  = self.head
        self.size = 0
    
    def insert(self, node):
        node.next = self.tail
        node.pre = self.tail.pre
        node.pre.next = node
        node.next.pre = node
        self.size = self.size+1
    
    def remove(self, node=None): 
        self.size = self.size -1
        if node: #给定node参数，删除某个node
            node.pre.next = node.next
            node.next.pre = node.pre
        else:    #容量满了, 要求删除第一个node
            node = self.head.next
            self.head.next = node.next
            node.next.pre = self.head
        return node

                
class LRUCache():
    def __init__(self, capacity: int):
        self.capacity = capacity
        self.cache = DoubleList()
        self.hashmap = {}
       

    def get(self, key: int) -> int:
        if key in self.hashmap:
            node = self.hashmap[key]
            self.cache.remove(node)
            self.cache.insert(node)
            return node.value
        else:
            return -1

    def put(self, key: int, value: int) -> None:
        if key in self.hashmap:
            node = self.hashmap[key]
            node.value = value
            self.cache.remove(node)
            self.cache.insert(node)
        else:
            if len(self.hashmap)==self.capacity:
                node = self.cache.remove()
                self.hashmap.pop(node.key)
            node = ListNode(key, value)
            self.cache.insert(node)
            self.hashmap[key] = node
```
