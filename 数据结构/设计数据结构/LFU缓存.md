#### LFU(least frequently used, 最不经常使用)淘汰访问频次最低的数据，如果访问频次最低的数据有多条，需要淘汰最旧的数据。[leetcode 460](https://leetcode-cn.com/problems/lfu-cache/)
实现 LFUCache 类：

* LFUCache(int capacity) - 用数据结构的容量 capacity 初始化对象
* int get(int key) - 如果键存在于缓存中，则获取键的值，否则返回 -1。
* void put(int key, int value) - 如果键已存在，则变更其值；如果键不存在，请插入键值对。当缓存达到其容量时，则应该在插入新项之前，使最不经常使用的项无效。在此问题中，当存在平局（即两个或更多个键具有相同使用频率）时，应该去除 最久未使用 的键。
注意「项的使用次数」就是自插入该项以来对其调用 get 和 put 函数的次数之和。使用次数会在对应项被移除后置为 0 。

为了确定最不常使用的键，可以为缓存中的每个键维护一个 使用计数器 。使用计数最小的键是最久未使用的键。

当一个键首次插入到缓存中时，它的使用计数器被设置为 1 (由于 put 操作)。对缓存中的键执行 get 或 put 操作，使用计数器的值将会递增。

```python
class LFUCache:
    def __init__(self, capacity: int)
    
    def get(self, key: int) -> int:
    
     def put(self, key: int, value: int) -> None:
```

####解法：哈希表+双向链表
* 相对于LRU问题，需要定义fremap：保存频率到双向链表的映射
```python
class ListNode:
    def __init__(self, key=None, value= None):
        self.key = key
        self.value = value
        self.fre = 1  #初始频率=1
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

            
class LFUCache:
    def __init__(self, capacity: int):
        self.capacity = capacity
        self.keymap ={} # key: node
        self.fremap ={} # fre: Double list
        self.minfre = 0
    
    def increasefre(self, key):
        node = self.keymap[key]
        node_fre = node.fre
        self.fremap[node_fre].remove(node)

        if self.minfre == node_fre and self.fremap[node_fre].size==0:  #双向链表为空，需要更新minfre
            self.minfre = self.minfre+1
        node_fre = node_fre+1
        node.fre = node_fre
        if node_fre not in self.fremap:  #初始化新的双向链表
            self.fremap[node_fre] = DoubleList()
        self.fremap[node_fre].insert(node)

    def remove_min_fre(self):
        node = self.fremap[self.minfre].remove()
        self.keymap.pop(node.key)

    def get(self, key: int) -> int:
        if key in self.keymap: #两种情况
            self.increasefre(key)   #key 存在需要增加key的frequency
            node = self.keymap[key]
            return node.value
        else:
            return -1
        
    def put(self, key: int, value: int) -> None:
        if self.capacity == 0:
            return None
        if key in  self.keymap:
            node = self.keymap[key]
            node.value = value
            self.increasefre(key)
        
        else:
            if len(self.keymap)==self.capacity: #容量满了
                self.remove_min_fre() 
            node = ListNode(key, value)
            self.minfre = 1  #最小频率一定是1
            if 1 not in self.fremap: 
                self.fremap[1]= DoubleList()
            self.fremap[1].insert(node)
            self.keymap[key]= node  #更新keymap
            
# Your LFUCache object will be instantiated and called as such:
# obj = LFUCache(capacity)
# param_1 = obj.get(key)
# obj.put(key,value)
############################### 


       
```
