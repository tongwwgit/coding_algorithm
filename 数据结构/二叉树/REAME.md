#### 二叉树定义
```python
#Definition for a binary tree node.
class TreeNode:
    def __init__(self, x):
        self.val = x
        self.left = None
        self.right = None
```
#### 对于二叉树的题目，需要清楚根节点应该做的事，递归时需要明确递归函数的意义，不用跳到递归细节里面
#### 二叉搜索树(BST)：对于一个父节点和两个子节点，值的大小关系满足 left.val<root.val<right.val, BST中序遍历是个递增的序列
BST代码框架
```C++
void BST(TreeNode root, int target) {
    if (root.val == target)
        // 找到目标，做点什么
    if (root.val < target) 
        BST(root.right, target);
    if (root.val > target)
        BST(root.left, target);
}
```
#### 堆(heap):是一个完全二叉树，包括最小堆和最大堆，可用一维数组来描述, 插入或删除的复杂度为O(logn)
* 根节点一定是极值，该特性在堆中是递归的，根节点左右子树也是堆
* 最小堆(根节点在所有数据中最小)；最大堆(根节点在所有数据中最大)
* 完全二叉树 是 一种除了最后一层之外的其他每一层都被完全填充，并且所有结点都保持向左对齐的树；
* 堆中父节点和子节点下标关系
    * 若根节点下标从1开始，对于父节点i,其左右节点的下标是2i, 2i+1
    * 若根节点下标从0开始，对于父节点i,其左右节点的下标是2i+1, 2i+2
* 最大堆的应用
    * [快速获取数组中点的相邻k个点的值](https://github.com/WenwenTong/coding_algorithm/blob/master/数据结构/数组和字符串/快速获取数组中点的相邻k个点的值.md)
    * [数组中最小的k个数](https://github.com/WenwenTong/coding_algorithm/blob/master/数据结构/数组和字符串/数组中最小的k个数.md)
    * [堆排序](https://github.com/WenwenTong/coding_algorithm/blob/master/算法/排序和查找算法/经典排序算法.md)

