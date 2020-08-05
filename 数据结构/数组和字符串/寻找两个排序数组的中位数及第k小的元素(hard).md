####  寻找两个排序数组的中位数: 给定两个大小为 m 和 n 的正序（从小到大）数组 nums1 和 nums2。请你找出这两个正序数组的中位数，并且要求算法的时间复杂度为 O(log(m + n))。Refer to [leetcode 4](https://leetcode-cn.com/problems/median-of-two-sorted-arrays/)
1. 归并排序后输出中位数：复杂度O(nlogn)
```python
class Solution:
    def findMedianSortedArrays(self, nums1: List[int], nums2: List[int]) -> float:
        def merge(nums1,nums2):
            merged=[]
            while nums1 and nums2:
                if nums1[0]<nums2[0]:
                    merged.append(nums1.pop(0))
                else:
                    merged.append(nums2.pop(0))
            merged.extend(nums1 or nums2)
            return merged
        nums=merge(nums1,nums2)
        n=len(nums)
        return nums[n//2] if n%2==1 else (nums[n//2-1]+nums[n//2])/2
```
2. 二分查找法可以保证时间复杂度
* 将两个数组分别以i,j为中心进行分割如下所示
    * A:左边i个元素，右边(m-i)个元素: 0,1,..,i-1 | i,i+1,m-1
    * B:左边j个元素，右边(n-j)个元素: 0,1,..,j-1 | j,j+1,n-1
* 中位数条件：
    * 若总数为偶数：i+j=m-i+n-j 
    * 若总数为奇数，i+j= m-i+n-j+1
    * 统一表示为 j=(m+n+1)//2-i,  奇数的话左半边比右半边多一个数
* 满足的条件 ：A[i-1]<B[j] and A[i]>B[j-1]。 可通过二分查找找到满足要求的i和j
```python
class Solution:
    def findMedianSortedArrays(self, nums1: List[int], nums2: List[int]) -> float:
        m,n=len(nums1),len(nums2)
        if m>n: #保证m<n为了后面判断更加方便
            m,n,nums1,nums2=n,m,nums2,nums1
        left,right=0,m     #查找i的范围
        while left<=right: 
            i=(left+right)//2
            j=(m+n+1)//2-i  #j不会越界
            if i>0 and nums1[i-1]>nums2[j]:
                right=i-1
            elif i<m and nums2[j-1]>nums1[i]:
                left=i+1
            else:
                break
        
        maxofleft1=nums1[i-1] if i>0 else float('-inf')
        maxofleft2=nums2[j-1] if j>0 else float('-inf')
        maxofleft=max(maxofleft1,maxofleft2)
        minofright1=nums1[i] if i<m else float('inf')
        minofright2=nums2[j] if j<n else float('inf')
        minofright=min(minofright1,minofright2)
        return maxofleft if (m+n)%2==1 else (maxofleft+minofright)/2
```
    
#### 给定两个排序数组，查找其合并后的第k小的元素，复杂度为logk，使用二分查找法
* 将两个数组分别以i,j为中心进行分割如下所示
    * A:左边i个元素，右边(m-i)个元素: 0,1,..,i-1 | i,i+1,m-1
    * B:左边j个元素，右边(n-j)个元素: 0,1,..,j-1 | j,j+1,n-1
* 满足的条件 ：A[i-1]<B[j] and A[i]>B[j-1] 且i+j=k
* 最后输出的结果为 max(A[i-1],B[j-1])
```python
def findksmall(nums1,nums2,k):
    m,n=len(nums1),len(nums2)
    if m>n:
        m,n,nums1,nums2=n,m,nums2,nums1
    left,right=0,m
    if k<m:
        right=k
    while left<=right:
        i=(left+right)//2
        j=k-i
        if j>n: j=n  #j可能会越界
        if i>0 and 0<=j<n and nums1[i-1]>nums2[j]:
            right=i-1
        elif i<m and 1<=j<=n and nums1[i]<nums2[j-1]:
            left=i+1
        else:
            break
        
    left1=nums1[i-1] if i>0 else float('-inf')
    left2=nums2[j-1] if j>0 else float('-inf')
    return max(left1,left2)


arra=[1,3,5,7,9,]
arrb=[2,4,6,8,10]
k=10
arr=arra+arrb
arr.sort()
print(arr[k-1])
print(findksmall(arra,arrb,k))
```
