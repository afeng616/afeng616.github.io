# 爬楼问题笔记

<!--more-->

[LeetCode.70.爬楼梯](https://leetcode.cn/problems/climbing-stairs/description/)被列为一道简单题，巧合下这几天又遇到了这道题的变种，于是又回头研究了一番。
先简述一下问题，已知有$n$个台阶，每次可爬1或2个台阶，求有多少种不同的爬楼方式。

## 递归求解

在确定问题终止节点后，可以使用递归算法求解。当还剩下0步时，返回计数1，即该方案已经到达终点；剩余多步则继续递归；小于0步则返回0，即无法到达终点。
```python
def climb(n: int) -> int:
    if n < 0:
        return 0
    elif n == 0:
        return 1
    else:
        return climb(n-1) + climb(n-2)
```

## 记忆优化

通过增加记忆减少重复计算。虽然时间复杂度并未发生变化，但避免了数值的重复计算，极大提升了效率。
```python
def climb(n: int) -> int:
    memo = {}
    def step(n: int) -> int:
        if n in memo.keys():
            return memo[n]

        if n < 0:
            return 0
        elif n == 0:
            return 1
        else:
            cnt = step(n-1) + step(n-2)
            memo[n] = cnt
            return cnt
    return step(n)
```

## 递推优化

当$n$较大时，递归算法会导致栈溢出，无法求解。这里尝试通过分析，找到新的求解关系。
不难看出递归求解$f(n)$时，实际计算了$f(n-1)$和$f(n-2)$，即每一项的值可以通过前两项的值求得，这看起来就很像是斐波那契数列。
```python
def climb(n: int) -> int:
    a, b = 1, 2
    for _ in range(n-1):
        a, b = b, a+b
    return a
```

## 问题推广

现在，对原有问题进一步推广，每次可爬1、2、...、m个台阶，求有多少种不同的爬楼方式。
今天翻了翻LeetCode，发现已经有大佬提出过这个[推广问题](https://leetcode.cn/problems/climbing-stairs/description/comments/1058976/)了。

相比每次爬1或2个台阶，此时每一步有$m$种选择。结合上面的递推分析，我们可以大胆猜测，此时$f(n)$的求解依赖前$m项的值。
```python
def climb(n: int, m: int) -> int:
    cnt = [0] * m
    cnt[0] = 1
    for i in range(1, m):
        cnt[i] = sum(cnt[:i]) + 1
    if n < m:
        return cnt[n-1]

    for i in range(m, n+1):
        cnt[i % m] = sum(cnt)
    return cnt[(n-1) % m]
```
为了节约空间，可以循环使用一个长度为$m$的数组，存储前$m$项的值。在求解$f(n)$前，需要先计算出$f(1)$到$f(m)$的值。

## 总结

这是一个考察动态规划、递归和递推分析的问题，重点在于分析，还是挺有意思的。

