####  删除二叉搜索树中的节点 [leetcode 450](https://leetcode-cn.com/problems/delete-node-in-a-bst/)

给定一个二叉搜索树的根节点 root 和一个值 key，删除二叉搜索树中的 key 对应的节点，并保证二叉搜索树的性质不变。返回二叉搜索树（有可能被更新）的根节点的引用。

一般来说，删除节点可分为两个步骤：首先找到需要删除的节点；如果找到了，删除它。

#### 删除分三种情况
```python
class Solution:
    def deleteNode(self, root: TreeNode, key: int) -> TreeNode:
        ## 要先找到这个node，然后删除它，删除时有三种情况需要考虑
        ## (1) 恰好是末端节点，直接删除
        ## (2) 只有一个非空节点，就让它的孩子接替它的位置
        ## (3) 若有两个子节点，必须找到左子树的最大的节点或者右子树的最小节点来替代自己
        if not root:
            return None
        if root.val==key:
            ##情况1，2
            if root.left==None:
                return root.right
            if root.right==None:
                return root.left
            ## 情况3
            #先找到右子树的最小节点
        #     minnode = root.right
        #     while minnode.left:
        #         minnode =minnode.left
        #    # 替换+删除
        #     root.val = minnode.val
        #     root.right = self.deleteNode(root.right,minnode.val)

            ##找到左子树的最大节点
            maxnode = root.left
            while maxnode.right:
                maxnode =maxnode.right
            root.val = maxnode.val
            root.left = self.deleteNode(root.left,maxnode.val)
        elif root.val>key:
            root.left = self.deleteNode(root.left, key)
        else:
            root.right = self.deleteNode(root.right, key)
        return root
    


```
