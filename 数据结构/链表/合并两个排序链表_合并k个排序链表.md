#### 合并两个有序链表：将两个升序链表合并为一个新的 升序 链表并返回。新链表是通过拼接给定的两个链表的所有节点组成的。 [leetcode 21](https://leetcode-cn.com/problems/merge-two-sorted-lists/)
* 输入：1->2->4, 1->3->4
* 输出：1->1->2->3->4->4
1. 迭代法
```python
class Solution:
    def mergeTwoLists(self, l1: ListNode, l2: ListNode) -> ListNode:
        if not l1:
            return l2
        if not l2:
            return l1
        res=p=ListNode(0)
        while l1 and l2:
            if l1.val<l2.val:
                p.next=l1
                l1=l1.next
            else:
                p.next=l2
                l2=l2.next
            p=p.next
        p.next=l1 or l2
        return res.next
```
2. 递归法
```python
class Solution:
    def mergeTwoLists(self, l1: ListNode, l2: ListNode) -> ListNode:
        if not l1:
            return l2
        if not l2:
            return l1
        if l1.val<l2.val:
            l1.next=self.mergeTwoLists(l1.next,l2)
            return l1
        else:
            l2.next=self.mergeTwoLists(l1,l2.next)
            return l2
```

#### 合并k个排序链表： 归并排序，对left和right使用合并两个排序链表的方法， 时间复杂度O(nlogk) [leetcode 23](https://leetcode-cn.com/problems/merge-k-sorted-lists/)
```python
class Solution:
    def mergeKLists(self, lists: List[ListNode]) -> ListNode:
        if not lists:
            return None
        if len(lists)==1:
            return lists[0]

        mid=len(lists)//2
        left=self.mergeKLists(lists[:mid])
        right=self.mergeKLists(lists[mid:])
        
        ##对left和right进行合并
        res=p=ListNode(0)
        while left and right:
            if left.val<right.val:
                p.next=left
                left=left.next
            else:
                p.next=right
                right=right.next
            p=p.next
        p.next=left or right
        return res.next
```
暴力法：取出所有的元素O(nlogn)排序好后再重新创建链表
```python
class Solution:
    def mergeKLists(self, lists: List[ListNode]) -> ListNode:
        if not lists:
            return None
        if len(lists)==1:
            return lists[0]
        lst=[]
        for item in lists:
            while item:
                lst.append(item.val)
                item=item.next
        p=res=ListNode(0)
        for i in sorted(lst):
            p.next=ListNode(i)
            p=p.next
        return res.next
```
