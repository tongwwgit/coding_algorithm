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
```
#### k个一组翻转链表：给你一个链表，每 k 个节点一组进行翻转，请你返回翻转后的链表。k 是一个正整数，它的值小于或等于链表的长度。如果节点总数不是 k 的整数倍，那么请将最后剩余的节点保持原有顺序。Refer to [leetcode 25](https://leetcode-cn.com/problems/reverse-nodes-in-k-group/submissions/)
1. 递归法: 对于前k个节点依次从头部拿一个节点然后与后面的节点连接
```python
class Solution:
    def reverseKGroup(self, head: ListNode, k: int) -> ListNode:
        if not head:
            return head
        p=head
        for i in range(k): #判断是否够k个
            if p:
                p=p.next
            else:
                return head
        #此时p是第k+1个节点
        p=self.reverseKGroup(p,k)
        for i in range(k):  #依次从头部拿一个节点与后面的连接
            temp=head.next
            head.next=p
            p=head
            head=temp
        return p
```
2. 循环法：递归法的迭代实现
```python
class Solution:
    def reverseKGroup(self, head: ListNode, k: int) -> ListNode:
        if not head or not head.next:
            return head
        res=p=ListNode(0) # 需要一个哑节点
        while True:
            node=head
            count=k
            while node and count:
                count=count-1
                node=node.next
            if count:
                p.next=head
                break
            else: #node是第k+1个节点
                #将前k个节点翻转
                cur=head
                nextgroup=node
                for i in range(k):
                    temp=cur.next
                    cur.next=nextgroup
                    nextgroup=cur
                    cur=temp
                p.next=nextgroup
                p=head
                head=node
        return res.next
```
3. 循环法：使用栈来缓存，然后翻转节点
```python
class Solution:
    def reverseKGroup(self, head: ListNode, k: int) -> ListNode:
        res=p=ListNode(0)
        while True:
            count=k
            cur=head
            stack=[]
            while cur and count: #是否有k个
                stack.append(cur)
                cur=cur.next
                count=count-1
            if count:
                p.next=head
                break
            else:
                while stack: #利用栈来翻转链表更加简洁
                    p.next=stack.pop()
                    p=p.next
                head=cur
        return res.next
```
