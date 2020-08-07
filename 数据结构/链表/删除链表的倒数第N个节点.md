#### 给定一个链表，删除链表的倒数第 n 个节点，并且返回链表的头结点(n是有意义的). Refer to [leetcode 19](https://leetcode-cn.com/problems/remove-nth-node-from-end-of-list/)
* 给定一个链表: 1->2->3->4->5, 和 n = 2.
* 当删除了倒数第二个节点后，链表变为 1->2->3->5.

### 双指针法：p先从head走n步，然后q开始从head走，p到尾节点时, q.next正好是删除的节点
```python
class Solution:
    def removeNthFromEnd(self, head: ListNode, n: int) -> ListNode:
        if not head:
            return None
        p=head
        for i in range(n): #双指针法，先走n步，注意删除的是头指针的情况
            if p.next:
                p=p.next
            else:
                return head.next
        q=head
        while p.next:
            p=p.next
            q=q.next
        q.next=q.next.next
        return head
```
