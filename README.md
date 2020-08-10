# coding_algorithm
# 常见的数据结构和算法题的总结

#### 经典解法框架：
1. 滑动窗口双指针法: 可方便求解子串问题([最小覆盖子串_字符串的排列_找到字符串中所有字母异位词](https://github.com/WenwenTong/coding_algorithm/blob/master/数据结构/数组和字符串/滑动窗口法_最小覆盖子串_字符串的排列_找到字符串中所有字母异位词.md))
```python
left=0
right=0
while right<len(s)
    // 增大窗口，进行窗口内的更新
    window.add(s[right]);
    
    while (window needs shrink) {
        // 缩小窗口
        window.remove(s[left]);
        left=left+1
    right=right+1
```
