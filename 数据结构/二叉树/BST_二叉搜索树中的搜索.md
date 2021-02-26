####  二叉搜索树中的搜索 [leetcode 700](https://leetcode-cn.com/problems/search-in-a-binary-search-tree/)
给定二叉搜索树（BST）的根节点和一个值。 你需要在BST中找到节点值等于给定值的节点。 返回以该节点为根的子树。 如果节点不存在，则返回 NULL。

1. 暴力递归法
``` python
lass Solution:
    def searchBST(self, root: TreeNode, val: int) -> TreeNode:
        if not root:
            return None
        if root.val==val:
            return root
        return self.searchBST(root.left, val) or self.searchBST(root.right,val)
```
2. 二分递归法：复杂度O(logN)
``` python
lass Solution:
    def searchBST(self, root: TreeNode, val: int) -> TreeNode:
        if not root:
            return None
        if root.val ==val:
            return root
        elif root.val<val:
            return self.searchBST(root.right, val)
        else:
            return self.searchBST(root.left, val)
```

3. 迭代法:复杂度O(logN)
``` python
lass Solution:
    def searchBST(self, root: TreeNode, val: int) -> TreeNode:
        if not root:
            return None
        while root:
            if root.val==val:
                return root
            elif root.val<val:
                root = root.right
            else:
                root = root.left
        return None
```

4. 优化迭代法
``` python
lass Solution:
    def searchBST(self, root: TreeNode, val: int) -> TreeNode:
        while root and root.val!=val:
            root =root.left if root.val>val else root.right
        return root
```
