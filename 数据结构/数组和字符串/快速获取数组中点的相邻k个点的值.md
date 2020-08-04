#### 给一个含有n个元素的数组，设计一个复杂度为O(n)的算法，找出距离数组中点最近的k个元素
1. 使用partition函数获取数组的中位数，即随机选择一个元素，然后比它小的排在前面，比它大的排在后面
2. 将数组的前k个元素创建一个最大堆，其中堆顶的元素离中点的距离最远，再依次查找n-k-1个元素，若该元素到中点的距离比堆顶小，则替换堆顶
复杂度分析：获取中位数复杂度为O(n); 堆插入一次O(logk), 所以堆插入的总复杂度是O(nlogk); 总体时间复杂度为O(n)
```python
def partition(arr,start,end): 
    index=start-1
    val=arr[end]
    for i in range(start,end):
        if arr[i]<val:
            index=index+1
            arr[i],arr[index]=arr[index],arr[i]
    index=index+1
    arr[index],arr[end]=arr[end],arr[index]
    return index

#将arr元素调整顺序后，arr[mid]正好是中位数
def getmidpoint(arr):
    start=0
    end=len(arr)-1
    index=partition(arr,start,end)
    mid=len(arr)//2
    while index!=mid: #不断缩小查找区间
        if index<mid:
            start=index+1
            index=partition(arr,start,end)
        else:
            end=index-1
            index=partition(arr,start,end)
    return arr[index]

def constrcut_max_heap(arr,k,i): #长度为k的数组，构造第i个元素的最大堆
    left=2*i+1 #左右节点下标
    right=2*i+2
    largest=i
    if left<k and distance[arr[left]]>distance[arr[largest]]:
        largest=left
    if right<k and distance[arr[right]]>distance[arr[largest]]:
        largest=right
    if largest!=i:
        arr[i],arr[largest]=arr[largest],arr[i]
        constrcut_max_heap(arr,k,largest) #可能会破坏子树的最大堆结构

        
arr=[7,14,10,12,2,11,29,3,4]
#arr=[9,7,5,4,2,1,3,6,8]
midval=getmidpoint(arr)
print(arr)
distance={}
for i in range(len(arr)): #距离的映射关系
    distance[arr[i]]=abs(arr[i]-midval)
k=5
arrheap=arr[:k]
#前k个元素构造一个最大堆
print(arrheap)
for i in range(k//2,-1,-1):
    constrcut_max_heap(arrheap,k,i)
print(arrheap)
#检查后序的n-k-1个元素，替换最大堆的相应元素
for i in range(k,len(arr)):
    if distance[arr[i]]>=distance[arrheap[0]]:
        continue
    else:
        #替换最大堆的堆顶元素
        arrheap[0]=arr[i]
        constrcut_max_heap(arrheap,k,0)
print(arrheap)
```
