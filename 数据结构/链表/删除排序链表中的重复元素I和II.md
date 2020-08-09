####  I. 给定一个排序链表，删除所有重复的元素，使得每个元素只出现一次。Refer to [leetcode 83](https://leetcode-cn.com/problems/remove-duplicates-from-sorted-list/)
* 输入: 1->1->2->3->3
* 输出: 1->2->3

1. 使用哑节点
```python
class Solution:
    def deleteDuplicates(self, head: ListNode) -> ListNode:
        if not head or not head.next:
            return head
        res=p=ListNode(0)
        cur=head
        while cur:
            val=cur.val
            while cur.next and cur.next.val==val:
                cur=cur.next
            p.next=cur
            p=p.next
            cur=cur.next
        return res.next
```
2. 直接删除法
```python
class Solution:
    def deleteDuplicates(self, head: ListNode) -> ListNode:
        cur=head
        while cur and cur.next:
            if cur.val==cur.next.val:
                cur.next=cur.next.next
            else:
                cur=cur.next
        return head
```

#### II. 给定一个排序链表，删除所有含有重复数字的节点，只保留原始链表中没有重复出现的数字。 Refer to [leetcode 82](https://leetcode-cn.com/problems/remove-duplicates-from-sorted-list-ii/)
* 输入: 1->2->3->3->4->4->5
* 输出: 1->2->5

1. 双节点法
```python
class Solution:
    def deleteDuplicates(self, head: ListNode) -> ListNode:
        if not head or not head.next:
            return head
        res=p=ListNode(0)
        p.next=head
        cur=head
        while cur and cur.next:
            if cur.val==cur.next.val:
                while cur.next and cur.next.val==cur.val:
                    cur=cur.next
                p.next=cur.next
                cur=cur.next
            else:
                p=p.next
                cur=cur.next
        return res.next
```
2. 递归法
```python
class Solution:
    def deleteDuplicates(self, head: ListNode) -> ListNode:
        if not head:
            return None
        if head.next and head.next.val==head.val:
            while head.next and head.next.val==head.val:
                head=head.next
            return self.deleteDuplicates(head.next)
        else:
            head.next=self.deleteDuplicates(head.next)
            return head
```

3. 快慢节点法
```python
class Solution:
    def deleteDuplicates(self, head: ListNode) -> ListNode:
        if not head or not head.next:
            return head
        res=ListNode(0)
        res.next=head
        slow,fast=res,head
        while fast:
            while fast.next and fast.next.val==slow.next.val:
                fast=fast.next
            if slow.next==fast:
                slow=slow.next
            else:
                slow.next=fast.next
            fast=fast.next
        return res.next 
```
