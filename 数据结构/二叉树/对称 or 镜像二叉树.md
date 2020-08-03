#### 给定一个二叉树，检查它是否是镜像对称的。Refer to [leetcode 110](https://leetcode-cn.com/problems/balanced-binary-tree/)
1. 递归法
```python
class Solution:
    def isSymmetric(self, root: TreeNode) -> bool:
        if not root:
            return True
        return self.symmetric(root,root)
    
    def symmetric(self,root1,root2):
        if root1==None and root2==None:
            return True
        if root1==None or root2==None:
            return False
        return root1.val==root2.val and self.symmetric(root1.left,root2.right) and \
               self.symmetric(root1.right,root2.left)
```

2. 循环法：依次比较两个相应的值，判断是否相等
```python
class Solution:
    def isSymmetric(self, root: TreeNode) -> bool:
        if not root:
            return True
        stack=[root.left,root.right]
        while stack:
            right=stack.pop()
            left=stack.pop()
            if left==None and right==None:
                continue
            if left==None or right==None:
                return False
            if left.val!=right.val:
                return False
            #先append哪边不重要，只要是一对就行
            stack.append(left.left)
            stack.append(right.right)
            stack.append(left.right)
            stack.append(right.left)
            
        return True
```
