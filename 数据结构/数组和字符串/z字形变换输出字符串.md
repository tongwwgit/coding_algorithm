#### 将一个给定字符串根据给定的行数，以从上往下、从左到右进行 Z 字形排列。Refer to [leetcode 6](https://leetcode-cn.com/problems/zigzag-conversion/).比如输入字符串为 "LEETCODEISHIRING" 行数为 3 时，排列如下：
```python
L   C   I   R
E T O E S I I G
E   D   H   N
```
1. 根据2*numRows-2一个周期来求的，判断每一行放哪些元素，求每一行的最后再join
```python
class Solution:
    def convert(self, s: str, numRows: int) -> str:
        if not s or numRows<=0:
            return ''
        if numRows==1:
            return s
        res=['' for i in range(numRows)]
        period=2*numRows-2
        for i in range(len(s)):
            m=i%period
            if m<numRows:
                res[m]=res[m]+s[i]
            else:
                res[period-m]+=s[i]
        return ''.join(res)
```
2. flag的巧妙运用，设置一个flag,判断是正序还是反序相加
```python
class Solution:
    def convert(self, s: str, numRows: int) -> str:
        if numRows<2:
            return s
        res=['']*numRows
        i=0
        flag=-1
        for c in s:
            res[i]=res[i]+c
            if i==0 or i==numRows-1:
                flag=-flag
            i=i+flag
        return ''.join(res)
```
