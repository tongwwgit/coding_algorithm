#### 给定一个链表，两两交换其中相邻的节点，并返回交换后的链表。你不能只是单纯的改变节点内部的值，而是需要实际的进行节点交换。Refer to [leetcode 24](https://leetcode-cn.com/problems/swap-nodes-in-pairs/)
* 给定 1->2->3->4, 你应该返回 2->1->4->3.

1. 递归法
```python
class Solution:
    def swapPairs(self, head: ListNode) -> ListNode:
        if not head or not head.next:
            return head
        second=head.next
        head.next=self.swapPairs(second.next)
        second.next=head
        return second
```
2. 循环法
```python
class Solution:
    def swapPairs(self, head: ListNode) -> ListNode:
        res=p=ListNode(0)
        p.next=head
        while head and head.next:
            first=head
            second=first.next
            #swap
            first.next=second.next
            second.next=first
            p.next=second

            p=first
            head=p.next
        return res.next
``
