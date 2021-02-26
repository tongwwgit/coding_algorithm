#### 把二叉搜索树转换为累加树 [leetcode 538](https://leetcode-cn.com/problems/convert-bst-to-greater-tree/) 与[leetcode 1038](https://leetcode-cn.com/problems/binary-search-tree-to-greater-sum-tree/)完全一样

#### 解法：利用反序的中序遍历法，累加和是计算大于等于当前值的所有元素之和，那么每个节点都去计算右子树的和
1. 递归法
```python
class Solution:
    def convertBST(self, root: TreeNode) -> TreeNode:
        def traverse(root):
            nonlocal tempsum
            if not root:
                return
            
            traverse(root.right)
            tempsum = tempsum + root.val
            root.val = tempsum
            traverse(root.left)
        
        tempsum=0
        traverse(root)
        return root
```

2. 迭代法
```python
class Solution:
    def convertBST(self, root: TreeNode) -> TreeNode:
        tempsum = 0
        stack = []
        cur =root
        while stack or cur:
            while cur:
                stack.append(cur)
                cur = cur.right
            
            cur = stack.pop()
            tempsum = tempsum+cur.val
            cur.val = tempsum
            cur = cur.left
        return root
```
