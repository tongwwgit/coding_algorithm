#### 给定一个二叉树，检查它是否是镜像对称的。Refer to [leetcode 101](https://leetcode-cn.com/problems/symmetric-tree/)
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

#### 二叉树的镜像构造：请完成一个函数，输入一个二叉树，该函数输出它的镜像。Refer to [leetcode 27](https://leetcode-cn.com/problems/er-cha-shu-de-jing-xiang-lcof/)
1. 递归交换左右节点
```python
class Solution:
    def mirrorTree(self, root: TreeNode) -> TreeNode:
        if not root:
            return None
        root.left,root.right=self.mirrorTree(root.right),self.mirrorTree(root.left)
        return root
```
2. 利用栈（或队列）遍历树的所有节点 node ，并交换每个 node 的左 / 右子节点。
```python
class Solution:
    def mirrorTree(self, root: TreeNode) -> TreeNode:
        if not root:
            return None
        stack=[root]
        while stack:
            node=stack.pop()
            if node.left:
                stack.append(node.left)
            if node.right:
                stack.append(node.right)
            node.left,node.right=node.right,node.left
        return root
```
