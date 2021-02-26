#### 二叉搜索树中第K小的元素 [leetcode 230](https://leetcode-cn.com/problems/kth-smallest-element-in-a-bst/)

##### 1. 先中序遍历得到递增的序列，复杂度O(N)
```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
class Solution:
    def kthSmallest(self, root: TreeNode, k: int) -> int:
        def midorder(root):
            res= []
            if root:
                res=res+midorder(root.left)
                res.append(root.val)
                res=res+midorder(root.right)
            return res

        res = midorder(root)
        return res[k-1]
```

##### 2. 迭代法，找到第k个就停止，复杂度O(h+k)
```python
class Solution:
    def kthSmallest(self, root: TreeNode, k: int) -> int:
        stack = []
        while stack or root:
            while root:
                stack.append(root)
                root = root.left
            root = stack.pop()
            k=k-1
            if k==0:
                return root.val
            root = root.right
```

##### 3. 二分法：需要知道子树节点的个数
* 如果左子树的个数n>k-1，就去左子树查找；
* 如果左子树的个数n=k-1, 返回node.val
* 如果左子树的个数n<k-1, 去右子树查找 k-n-1
```python
class Solution:
    def kthSmallest(self, root: TreeNode, k: int) -> int:
        def count(node):
            if not node:
                return 0
            return count(node.left)+count(node.right)+1
        
        n= count(root.left)
        if n==k-1:
            return root.val
        elif n>k-1:
            return self.kthSmallest(root.left,k)
        else:
            return self.kthSmallest(root.right,k-n-1)
```


##### 4. 二分法：稍微改进了下3
```python
class Solution:
    def kthSmallest(self, root: TreeNode, k: int) -> int:
        def count(node):
            if not node:
                return 0
            return count(node.left)+count(node.right)+1
        

        def getksmallest(root,k):
            n= count(root.left)
            if n==k-1:
                return root.val
            elif n>k-1:
                return getksmallest(root.left,k)
            else:
                return getksmallest(root.right,k-n-1)

        return getksmallest(root,k)
```
