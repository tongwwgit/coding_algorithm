## 排序算法，二分查找算法

* 二分查找可能的问题：mid=(left+right)//2 可能导致计算溢出， 可以改为mid=left+(right-left)//2
* 注意搜索区间(左开右闭，左闭右闭，左闭右开)和while终止的条件，存在漏掉的元素需要后续检查
* 注意重复元素的左侧边界和右侧边界

```python
#左侧边界

def getfirst():
    left,right=0,n-1
    while left<=right:
        mid=(left+right)//2
        if nums[mid]==target:
            right=mid-1
        elif nums[mid]>target:
            right=mid-1
        else:
            left=mid+1
    if left>=n or nums[left]!=target:
        return -1
    else:
        return left

#右侧边界
def getlast():
    left,right=0,n-1
    while left<=right:
        mid=(left+right)//2
        if nums[mid]==target:
            left=mid+1
        elif nums[mid]>target:
            right=mid-1
        else:
            left=mid+1
    if right<0 or nums[right]!=target:
        return -1
    else:
        return right
```
