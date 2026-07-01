---
title: "NEETCODE005: Dynamic Programming / Greedy / Intervals"
date: 2026-07-01 00:00:00 +0800
categories: [Learning, Coding Interview]
tags: [pengyi-neetcode-map, neetcode005, dynamic-programming, greedy, intervals, dsa, coding-interview]
---

这是 `PENGYI_NEETCODE_MAP` 的第六篇：

```text
NEETCODE005 -> 1-D DP / 2-D DP / Greedy / Intervals
```

这一篇进入优化问题。

这几类题共同训练的是：

```text
如何在约束下找到最优解。
```

它们对应四种不同思维：

```text
1-D DP
    一维状态递推。

2-D DP
    多维状态递推。

Greedy
    局部选择导向全局最优。

Intervals
    区间排序、合并、冲突和事件流。
```

## 一句话模型

```text
DP = define state and transition.
Greedy = prove a local choice is globally safe.
Intervals = sort events and reason about overlap.
```

中文：

```text
DP 训练状态建模。
Greedy 训练最优性证明。
Intervals 训练时间段和冲突建模。
```

## Dynamic Programming 的本质

DP 不是背题。
DP 是：

```text
把一个大问题拆成重复出现的子问题，并保存子问题答案。
```

两个关键性质：

```text
overlapping subproblems
    子问题重复出现。

optimal substructure
    大问题最优解可以由子问题最优解组成。
```

如果没有这两个性质，DP 不一定适合。

## DP 五步法

每道 DP 题都按这五步：

```text
1. 定义 state
    dp[i] 或 dp[i][j] 到底代表什么？

2. 写 transition
    当前 state 如何由之前 state 得来？

3. 初始化 base case
    最小问题的答案是什么？

4. 确定 iteration order
    先算哪些 state，后算哪些 state？

5. 返回 answer
    dp[n]、max(dp)、dp[m][n]，还是别的？
```

DP 做不出来，通常是第一步没定义清楚。

## 1-D DP

1-D DP 的状态通常是：

```text
dp[i] = 前 i 个元素 / 到第 i 个位置 / 以 i 结尾 的最优值或数量。
```

典型题：

```text
climbing stairs
house robber
decode ways
coin change
longest increasing subsequence
word break
```

## 1-D DP 模板

```python
dp = [0] * (n + 1)
dp[0] = base

for i in range(1, n + 1):
    dp[i] = transition(dp, i)

return dp[n]
```

House Robber：

```text
dp[i] = 抢到第 i 间房之前的最大收益
```

转移：

```text
dp[i] = max(dp[i - 1], dp[i - 2] + nums[i - 1])
```

含义：

```text
不抢当前房 -> dp[i - 1]
抢当前房 -> dp[i - 2] + current
```

这比背公式重要。

## 1-D DP 常见坑

```text
1. dp[i] 语义不清
    是前 i 个，还是以 i 结尾？

2. 下标错位
    nums[i] 和 dp[i] 的关系。

3. base case 错
    n = 0 / 1 时出错。

4. 返回值错
    有些题返回 dp[-1]，有些题返回 max(dp)。

5. 遍历顺序错
    coin change 和 0/1 knapsack 的顺序不同。
```

## 2-D DP

2-D DP 通常出现在：

```text
grid
two strings
two sequences
interval
knapsack-like states
```

典型题：

```text
unique paths
longest common subsequence
edit distance
distinct subsequences
interleaving string
maximal square
```

2-D DP 的关键仍然是 state：

```text
dp[i][j] 到底代表什么？
```

## 2-D DP 示例：LCS

Longest Common Subsequence：

```text
dp[i][j] = text1 前 i 个字符和 text2 前 j 个字符的 LCS 长度。
```

转移：

```text
如果 text1[i-1] == text2[j-1]:
    dp[i][j] = dp[i-1][j-1] + 1
否则:
    dp[i][j] = max(dp[i-1][j], dp[i][j-1])
```

代码：

```python
dp = [[0] * (n + 1) for _ in range(m + 1)]

for i in range(1, m + 1):
    for j in range(1, n + 1):
        if text1[i - 1] == text2[j - 1]:
            dp[i][j] = dp[i - 1][j - 1] + 1
        else:
            dp[i][j] = max(dp[i - 1][j], dp[i][j - 1])

return dp[m][n]
```

这个例子说明：

```text
多开一行一列，可以让 base case 更干净。
```

## 2-D DP 常见坑

```text
1. i/j 对应哪个字符串或维度不清楚。
2. dp 多开一行一列后，原数组索引用 i-1 / j-1。
3. 初始化第一行第一列错误。
4. 遍历方向和依赖关系不匹配。
5. 把 substring 和 subsequence 混淆。
```

## Greedy

Greedy 的核心：

```text
每一步做局部最优选择，并且这个选择不会破坏全局最优。
```

典型题：

```text
jump game
gas station
partition labels
hand of straights
merge triplets
valid parenthesis string
```

Greedy 最难的不是实现。
而是证明：

```text
为什么这个局部选择是安全的？
```

## Greedy 的常见证明方式

```text
Exchange Argument
    如果最优解没有选择 greedy choice，可以交换成包含 greedy choice 的解，而且不会变差。

Staying Ahead
    greedy 解在每一步都不落后于任何其他解。

Invariant
    每一步维持一个全局有效的性质。

Sorting Key
    通过排序让局部选择顺序合理。
```

面试里，不一定要写形式化证明。
但必须讲出直觉。

## Greedy 和 DP 的关系

很多题可以用 DP，但 greedy 更快。

判断：

```text
如果局部选择可证明安全 -> greedy。
如果局部选择不安全，需要比较多个历史状态 -> DP。
```

例如：

```text
House Robber
    不能简单 greedy，因为当前最大不一定全局最优。

Jump Game
    可以 greedy，因为维护当前可达最远位置就足够。
```

Greedy 的危险是：

```text
看起来合理，但实际上错。
```

所以要有证明意识。

## Intervals

Intervals 题处理时间段或区间。

典型信号：

```text
meeting rooms
merge intervals
insert interval
erase overlap
minimum arrows
employee free time
```

核心操作：

```text
sort
merge
detect overlap
count conflicts
sweep line
```

## Interval 基础判断

两个区间：

```text
[a_start, a_end]
[b_start, b_end]
```

重叠条件常见写法：

```text
a_start <= b_end and b_start <= a_end
```

如果已经按 start 排序，且 `a` 在前：

```text
b_start <= a_end
```

就表示重叠。

合并：

```text
merged_end = max(a_end, b_end)
```

## Merge Intervals 模板

```python
intervals.sort(key=lambda x: x[0])
merged = []

for start, end in intervals:
    if not merged or start > merged[-1][1]:
        merged.append([start, end])
    else:
        merged[-1][1] = max(merged[-1][1], end)

return merged
```

训练点：

```text
排序后，只需要和最后一个 merged interval 比较。
```

## Meeting Rooms

判断是否能参加所有会议：

```python
intervals.sort(key=lambda x: x[0])

for i in range(1, len(intervals)):
    if intervals[i][0] < intervals[i - 1][1]:
        return False

return True
```

计算最少会议室：

```text
扫描所有 start/end 事件，或者用 min-heap 存当前会议结束时间。
```

这类题本质是资源冲突。

## Intervals 常见坑

```text
1. 边界开闭不清
    [1, 2] 和 [2, 3] 是否冲突？

2. 排序 key 错
    有些按 start，有些按 end。

3. 重叠条件错
    < 还是 <=。

4. 插入区间时三段逻辑不清
    before / overlap / after。

5. erase overlap 的 greedy 要按 end 排序
    因为越早结束越给后面留空间。
```

## 三类优化问题的关系

```text
DP
    穷举状态，但缓存结果。

Greedy
    不穷举，直接做安全局部选择。

Intervals
    通过排序把二维关系变成线性扫描。
```

它们共同服务于：

```text
在约束下找最优或可行解。
```

## Agent Harness 视角

这类题可以测试 agent 的高级思维。

```text
DP
    是否能定义 state。
    是否能写出 transition。
    是否能处理 base case。

Greedy
    是否能解释为什么局部选择正确。
    是否避免无证明的猜测。

Intervals
    是否能排序并处理边界。
    是否能判断冲突和合并。
```

很多 coding agent 会在 DP 上写出看似复杂但状态错误的代码。
所以 DP 是很好的 agent benchmark。

## Quant / Research OS 迁移

这一组和量化直接相关。

```text
DP
    sequential allocation
    path optimization
    portfolio transition with constraints

Greedy
    fast heuristic strategy
    candidate selection
    resource allocation

Intervals
    holding period
    event window
    risk exposure
    overlapping signals
    trading sessions
```

尤其 intervals，非常接近金融里的时间窗口处理。

## 面试表达模板

DP：

```text
I define dp[i] as the optimal answer for the prefix ending at i.
Then I derive the transition by considering whether we take the current item or skip it.
The base cases handle empty and single-element prefixes.
```

Greedy：

```text
The greedy choice is safe because choosing the earliest finishing interval leaves the maximum remaining room for future intervals.
Any optimal solution that chooses a later finishing interval can be exchanged with this one without reducing the number of intervals selected.
```

Intervals：

```text
After sorting intervals by start time, any overlap with the current interval only needs to be checked against the last merged interval.
If they overlap, we extend the end; otherwise we start a new interval.
```

## 当前结论

`NEETCODE005` 的核心结论：

```text
DP 训练状态转移。
Greedy 训练安全局部选择。
Intervals 训练区间冲突和事件流。
```

这一组是 coding interview 的优化核心。
如果能把 state、proof、boundary 讲清楚，面试质量会明显上一个台阶。
