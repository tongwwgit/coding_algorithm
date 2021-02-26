#### 给定一个二叉树，判断其是否是一个有效的二叉搜索树。[leetcode 98](https://leetcode-cn.com/problems/validate-binary-search-tree/)
假设一个二叉搜索树具有如下特征：

节点的左子树只包含小于当前节点的数。
节点的右子树只包含大于当前节点的数。
所有左子树和右子树自身必须也是二叉搜索树。

1. 递归法：root.val不仅要大于左节点，还要大于左子树中的所有节点.以 root 为根的子树，判断子树中所有节点的值是否都在 (l,r) 的范围内
```python
class Solution:
    def isValidBST(self, root: TreeNode) -> bool:
        def valid(root,low,up):
            if not root:
                return True
            if root.val<=low or root.val>=up:
                return False
            return valid(root.left,low,root.val) & valid(root.right, root.val, up)
        return valid(root,float('-inf'),float('inf'))
```
2. 迭代法
```python
class Solution:
    def isValidBST(self, root: TreeNode) -> bool:
        ref = float('-inf')
        stack = []
        while stack or root:
            while root:
                stack.append(root)
                root =root.left
            root = stack.pop()
            if root.val<=ref:
                return False
            ref = root.val
            root = root.right
        return True
```
