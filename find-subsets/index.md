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

## 位运算求解
实际上，一个长度为$n$的列表有$2^n$个子集。对于每一个子集，可以用一个长度为$n$的二进制数来表示，每一个二进制位表示列表中对应位置的元素是否在子集中。如，列表`[1, 2, 3]`的某个子集为`[1, 3]`，可以用二进制数`101`表示；子集`[3]`可以用`001`表示。  

```python
def find_subsets(nums):
    n = len(nums)
    subsets = []
    for i in range(2<<n):
        subsets.append([nums[j] for j in range(n) if i>>j & 1])
    return subsets
```





