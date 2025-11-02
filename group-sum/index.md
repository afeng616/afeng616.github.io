# 组合求和问题

<!--more-->

这两天接连遇到这种寻找组合求和的问题，感觉有必要研究记录一下。  
问题很简单，对于给定的一个整数数组 `nums` 和一个目标值 `target`，我们需要从中选出一些数，使得它们的和为 `target`，每个数只能使用一次。  

这个问题我一开始看到的时候直接发懵，不知道该如何下手，也不清楚该如何定义问题边界，如何使用递归或者递推求解。  
最后看到寥寥几行代码，陷入呆滞，这我还真想不到……

```python
def find_sum(nums, res):
    dp = [False] * (res + 1)
    dp[0] = True
    for num in nums:
        for i in range(res, num-1, -1):
            dp[i] = dp[i] or dp[i-num]
    return dp[res]
```
传入数组和目标值，该函数将返回能否组成目标值。  
该算法使用一个数组`dp`记录0到目标值的状态，基于动态规划的思想，逐步更新状态。算法逐个处理`nums`中的数`num`，依照现有`dp`状态再加上`num`更新`dp`。值得注意的是，`dp`更新需要考虑原状态及和`num`的组合情况。且应当倒序更新`dp`，以避免在同一轮中使用`num`多次。  
实际上，这里还可以通过记录`dp`为`True`的索引来进一步优化，避免无必要的状态更新判断。

```python
def find_sum(nums, res):
    stash = set()
    for num in nums:
        tmp = set()
        for state in stash:
            tmp.add(state+num)
        stash.update(tmp)
        stash.add(num)
    return res in stash
```

如果我们想知道具体是哪些数能组成目标值，就没必要做这种优化了，继续沿用`dp`的方案更容易回溯得到结果。  
这里，我们使用`pre`数组记录下该组合数，仅在`dp`状态首次通过`num`改变为`True`时记录。最后通过目标值回溯`pre`数组，得到组成目标值的所有数。

```python
def find_sum(nums, res):
    pre = [-1] * (res+1)
    dp = [False] * (res+1)
    dp[0] = True
    for num in nums:
        for i in range(res, num-1, -1):
            if not dp[i] and dp[i-num]:
                dp[i] = True
                pre[i] = num
    
    group = []
    if dp[res]:
        idx = res
        while pre[idx] > 0:
            group.append(pre[idx])
            idx = idx - pre[idx]
        return True, group
    else:
        return False, []
```

