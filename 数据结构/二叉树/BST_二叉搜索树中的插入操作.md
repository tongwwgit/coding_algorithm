#### 二叉搜索树中的插入操作 [leetcode 701](https://leetcode-cn.com/problems/insert-into-a-binary-search-tree/)
给定二叉搜索树（BST）的根节点和要插入树中的值，将值插入二叉搜索树。 返回插入后二叉搜索树的根节点。 输入数据 保证 ，新值和原始二叉搜索树中的任意节点值都不同。

注意，可能存在多种有效的插入方式，只要树在插入后仍保持为二叉搜索树即可。 你可以返回 任意有效的结果 。

1. 递归法
```python
class Solution:
    def insertIntoBST(self, root: TreeNode, val: int) -> TreeNode:
        if not root:
            return TreeNode(val)
        if root.val < val:
            root.right = self.insertIntoBST(root.right, val)
        else:
            root.left = self.insertIntoBST(root.left, val)
        return root
```
2. 迭代法
```python
class Solution:
    def insertIntoBST(self, root: TreeNode, val: int) -> TreeNode:
        if not root:
            return TreeNode(val)
        cur = root
        while cur:
            if val<cur.val:
                if cur.left:
                    cur=cur.left
                else:
                    cur.left =TreeNode(val)
                    break
            else:
                if cur.right:
                    cur = cur.right
                else:
                    cur.right =TreeNode(val)
                    break
        return root
```
