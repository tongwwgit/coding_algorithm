### 链表
Definition for singly-linked list.
```python
class ListNode:
    def __init__(self, x):
        self.val = x
        self.next = None
```
递归操作链表并不高效。和迭代解法相比，虽然时间复杂度都是 O(N)，但是迭代解法的空间复杂度是 O(1)，而递归解法需要堆栈，空间复杂度是 O(N)。
