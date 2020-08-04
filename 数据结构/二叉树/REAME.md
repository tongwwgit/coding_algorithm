#### 二叉树定义
```python
#Definition for a binary tree node.
class TreeNode:
    def __init__(self, x):
        self.val = x
        self.left = None
        self.right = None
```
#### 二叉搜索树(BST)：对于一个父节点和两个子节点，值的大小关系满足 left.val<root.val<right.val
#### 堆(heap):是一个完全二叉树，包括最小堆和最大堆，可用一维数组来描述, 插入或删除的复杂度为O(logn)
* 最小堆(根节点在所有数据中最小)；最大堆(根节点在所有数据中最大)
* 完全二叉树 是 一种除了最后一层之外的其他每一层都被完全填充，并且所有结点都保持向左对齐的树；
* 堆中父节点和子节点下标关系
    * 若根节点下标从1开始，对于父节点i,其左右节点的下标是2i, 2i+1
    * 若根节点下标从0开始，对于父节点i,其左右节点的下标是2i+1, 2i+2
