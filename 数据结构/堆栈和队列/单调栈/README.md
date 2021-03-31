#### 单调栈：通常是一维数组，要寻找任一元素右边（左边）第一个比自己大（小）的元素，且要求 O(n) 的时间复杂度，可以考虑使用单调栈
* 单调递增栈：从 栈底 到 栈顶 递增，栈顶大
* 单调递减栈：从 栈底 到 栈顶 递减，栈顶小

#### 模板套路

##### 当前项向右寻找第一个比自己大的元素
* 从右往左维护一个单调递减栈
``` python
def nextGreaterElement_01(nums: list):
    length = len(nums)
    res, stack = [-1] * length, []

    for i in range(length - 1, -1, -1):
        while stack and stack[-1] <= nums[i]:
            stack.pop()
        if stack:
            res[i] = stack[-1]
        stack.append(nums[i])

    return res
```

* 从左往右维护一个单调递减栈
```python
def nextGreaterElement_011(nums: list):
    length = len(nums)
    res, stack = [-1] * length, []

    for i in range(length):
        while stack and nums[stack[-1]] < nums[i]:
            idx = stack.pop()
            res[idx] = nums[i]
        stack.append(i)

    return res
```

##### 当前项向右找第一个比自己小的位置
* 从右向左维护一个单调递增栈
``` python
def nextGreaterElement_02(nums: list):
    length = len(nums)
    res, stack = [-1] * length, []

    for i in range(length - 1, -1, -1):
        while stack and stack[-1] >= nums[i]:
            stack.pop()
        if stack:
            res[i] = stack[-1]
        stack.append(nums[i])

    return res
```

##### 当前项向左找第一个比自己大的位置 —— 从左向右维护一个单调递减栈
```
def nextGreaterElement_03(nums: list):
    length = len(nums)
    res, stack = [-1] * length, []

    for i in range(length):
        while stack and stack[-1] <= nums[i]:
            stack.pop()
        if stack:
            res[i] = stack[-1]
        stack.append(nums[i])

    return res
```

##### 当前项向左找第一个比自己小的位置 —— 从左向右维护一个单调递增栈
```
def nextGreaterElement_04(nums: list):
    length = len(nums)
    res, stack = [-1] * length, []

    for i in range(length):
        while stack and stack[-1] >= nums[i]:
            stack.pop()
        if stack:
            res[i] = stack[-1]
        stack.append(nums[i])

    return res
```
