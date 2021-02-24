#### 给定前序遍历和中序遍历重构二叉树, Refer to [leetcode 105](https://leetcode-cn.com/problems/construct-binary-tree-from-preorder-and-inorder-traversal/)
#### 解法主要是先找到根节点的位置
1. 递归法：preorder[0]是根节点，根据preorder[0]在中序遍历中的位置可以将序列分为两份，分别是左子树和右子树
```python
class Solution:
    def buildTree(self, preorder: List[int], inorder: List[int]) -> TreeNode:
        if not preorder:
            return None
        root_val=preorder[0]
        root_index=inorder.index(root_val) #确定根节点在inorder中的下标索引
        root=TreeNode(root_val)
        root.left=self.buildTree(preorder[1:root_index+1],inorder[:root_index])
        root.right=self.buildTree(preorder[root_index+1:],inorder[root_index+1:])
        return root
```

2. 迭代法：根据中序遍历的结果判断该节点在根节点的左边还是右边，时间复杂度O(nlogn)。对preoder的结果从左往右遍历
```python
class Solution:
    def buildTree(self, preorder: List[int], inorder: List[int]) -> TreeNode:
        if not preorder:
            return None
        nodemap={}
        for i in range(len(inorder)):
            nodemap[inorder[i]]=i  #确定各元素的相对顺序
        root=TreeNode(preorder[0])
        for i in range(1,len(preorder)):
            val=preorder[i]
            cur=root #每次索引都从根节点开始
            while True:
                if nodemap[val]<nodemap[cur.val]: #当前值在root的左边
                    if cur.left:
                        cur=cur.left
                    else:
                        cur.left=TreeNode(val)
                        break
                
                else:
                    if cur.right:
                        cur=cur.right
                    else:
                        cur.right=TreeNode(val)
                        break
```

#### 从中序与后序遍历序列构造二叉树，[leetcode 106](https://leetcode-cn.com/problems/construct-binary-tree-from-inorder-and-postorder-traversal/)
1. 递归法
```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
class Solution:
    def buildTree(self, inorder: List[int], postorder: List[int]) -> TreeNode:
        ## 1.递归法
        if not postorder:
            return None
        
        root_val = postorder[-1]
        index = inorder.index(root_val)
        root = TreeNode(root_val)
        root.left = self.buildTree(inorder[:index], postorder[:index])
        root.right = self.buildTree(inorder[index+1:], postorder[index:-1])
        return root
```
2. 迭代法：对postoder的结果从右往左遍历
```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
class Solution:
    def buildTree(self, inorder: List[int], postorder: List[int]) -> TreeNode:
        if not postorder:
            return None
        nodemap={}
        for i in range(len(inorder)):
            nodemap[inorder[i]] = i

        root = TreeNode(postorder[-1])
        for i in range(len(postorder)-2,-1,-1):
            val = postorder[i]
            cur = root
            while True:
                if nodemap[val]>nodemap[cur.val]:
                    if cur.right:
                        cur = cur.right
                    else:
                        cur.right =TreeNode(val)
                        break
                else:
                    if cur.left:
                        cur= cur.left
                    else:
                        cur.left =TreeNode(val)
                        break
        return root
```
