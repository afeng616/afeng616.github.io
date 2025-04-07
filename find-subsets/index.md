# 寻找子集问题

<!--more-->

已知一个列表，求出它的所有子集。  

## 递归求解

以最小问题，即空列表作为递归的终止条件。每次进行递归求解时减少列表规模，直到列表为空，如每次去掉列表的第一个元素。通过逐步剔除首元素减小列表数量，在达到递归终止条件时，返回一个包含空列表的列表。然后再将首元素与返回的列表进行组合，形成新的子集列表。  
```python
def find_subsets(nums):
    if len(nums) == 0:
        return [[]]
    rest_sub = find_subsets(nums[1:])
    sub = rest_sub + [[nums[0]]+sub for sub in rest_sub]
    return sub
```




