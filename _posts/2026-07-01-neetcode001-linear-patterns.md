---
title: "NEETCODE001: Arrays & Hashing / Two Pointers / Sliding Window"
date: 2026-07-01 00:00:00 +0800
categories: [Learning, Coding Interview]
tags: [pengyi-neetcode-map, neetcode001, arrays, hashing, two-pointers, sliding-window, dsa, coding-interview]
---

这是 `PENGYI_NEETCODE_MAP` 的第二篇：

```text
NEETCODE001 -> Arrays & Hashing / Two Pointers / Sliding Window
```

这三类题是 NeetCode 训练的第一层。

它们共同训练的是：

```text
线性数据结构上的状态维护。
```

也就是说，我们面对数组、字符串、列表时，如何不暴力枚举所有组合，而是用一遍扫描、两个指针、动态窗口、hash map 把复杂度压下来。

## 总判断

这三类题可以这样理解：

```text
Arrays & Hashing
    用 hash map / hash set 维护已经见过的信息。

Two Pointers
    用两个边界缩小搜索空间。

Sliding Window
    用左右边界维护一个动态连续区间。
```

共同目标：

```text
把 O(n^2) 的 pair / subarray / substring 枚举，尽量压到 O(n) 或 O(n log n)。
```

这是 coding interview 最基础但最重要的能力。

## 一句话模型

```text
Arrays & Hashing = remember what we have seen.
Two Pointers = move boundaries with an invariant.
Sliding Window = maintain a valid contiguous range.
```

中文：

```text
Hashing 解决快速查找。
Two pointers 解决有结构的双边界推进。
Sliding window 解决连续区间的动态状态维护。
```

如果这三类题打不稳，后面的 graph、DP、agent harness 都会虚。

## Arrays & Hashing

这类题的核心问题：

```text
我能不能用一个 hash structure 记录历史信息，让当前查询变成 O(1)？
```

典型信号：

```text
duplicate
frequency
anagram
two sum
group
contains
seen before
count
unique
```

常用结构：

```text
hash set
    判断某个元素是否出现过。

hash map
    value -> count
    value -> index
    normalized key -> group
```

## Arrays & Hashing 的核心套路

### 1. Seen Set

用于判断是否出现过：

```python
seen = set()

for x in nums:
    if x in seen:
        return True
    seen.add(x)
```

训练点：

```text
用 O(n) 空间换 O(n) 时间。
```

### 2. Count Map

用于频次统计：

```python
count = {}

for x in nums:
    count[x] = count.get(x, 0) + 1
```

适合：

```text
valid anagram
top k frequent
group by frequency
majority candidate check
```

### 3. Complement Lookup

典型是 Two Sum：

```python
seen = {}

for i, x in enumerate(nums):
    need = target - x
    if need in seen:
        return [seen[need], i]
    seen[x] = i
```

训练点：

```text
不要先排序破坏 index。
不要先把所有值放进去导致同一个元素被使用两次。
```

### 4. Normalized Key

典型是 Group Anagrams。

思路：

```text
把同类对象映射到同一个 key。
```

例如：

```python
key = tuple(sorted(word))
groups[key].append(word)
```

或者更高效的字符计数 key：

```python
count = [0] * 26
for ch in word:
    count[ord(ch) - ord("a")] += 1
key = tuple(count)
```

训练点：

```text
算法设计时，经常要先定义一个 canonical representation。
```

这和 RAG 里的 chunk id、Graph RAG 里的 entity key、quant 里的 symbol-date key 都是同一个思想。

## Arrays & Hashing 常见坑

```text
1. key 不可 hash
    list 不能作为 dict key，需要 tuple。

2. 忘记 count 为 0 时删除
    sliding window 里尤其常见。

3. index 和 value 混淆
    two sum 返回 index，不是 value。

4. 排序改变原始位置
    如果题目要原始 index，要小心。

5. 只考虑正数
    hash 题通常不怕负数，但很多人写逻辑时默认正数。

6. 空输入
    nums = [] 或 string = "" 时要稳定。
```

## Two Pointers

Two Pointers 的核心不是“有两个变量”。
核心是：

```text
两个指针每移动一次，都能安全地排除一部分搜索空间。
```

典型信号：

```text
sorted array
palindrome
pair sum
reverse
in-place removal
fast and slow
container
```

常见形态：

```text
left / right
    从两端向中间。

slow / fast
    一个慢一个快，用于链表或原地覆盖。

read / write
    原地重写数组。
```

## Two Pointers 的不变量

Two pointers 必须有 invariant。

例如 sorted two sum：

```text
nums[left] + nums[right] 太小 -> left 右移
nums[left] + nums[right] 太大 -> right 左移
```

为什么成立？

```text
数组有序。
如果 sum 太小，固定 right 时，left 左边只会更小，不可能满足。
如果 sum 太大，固定 left 时，right 右边只会更大，不可能满足。
```

面试里要讲这段逻辑。
不然只是背模板。

## Two Pointers 模板

```python
left = 0
right = len(nums) - 1

while left < right:
    current = nums[left] + nums[right]

    if current == target:
        return [left, right]
    elif current < target:
        left += 1
    else:
        right -= 1
```

对于 palindrome：

```python
left = 0
right = len(s) - 1

while left < right:
    if s[left] != s[right]:
        return False
    left += 1
    right -= 1

return True
```

对于 in-place remove：

```python
write = 0

for read in range(len(nums)):
    if keep(nums[read]):
        nums[write] = nums[read]
        write += 1
```

这里的 invariant 是：

```text
nums[:write] 始终是已经处理过且应该保留的元素。
```

## Two Pointers 常见坑

```text
1. left < right 还是 left <= right
    pair 问题通常 left < right。
    单点搜索有时需要 <=。

2. 指针移动时机
    找到结果后是否继续，要看题目要一个解还是所有解。

3. 重复元素
    3Sum / 4Sum 类题要跳过 duplicates。

4. 是否需要排序
    排序会改变 index，也会增加 O(n log n)。

5. 不变量没讲清楚
    面试官会追问为什么可以移动这个指针。
```

## Sliding Window

Sliding Window 是连续区间题型。

核心问题：

```text
如何用 left / right 表示一个窗口，并动态维护窗口内状态？
```

典型信号：

```text
substring
subarray
contiguous
longest
shortest
at most k
without repeating
minimum window
```

Sliding Window 比 two pointers 更强调：

```text
窗口内状态。
```

例如：

```text
窗口里有哪些字符？
窗口里每个字符出现几次？
窗口和是多少？
窗口是否满足约束？
```

## Sliding Window 模板

最长合法窗口：

```python
left = 0
state = {}
ans = 0

for right in range(len(s)):
    add s[right] to state

    while window is invalid:
        remove s[left] from state
        left += 1

    ans = max(ans, right - left + 1)
```

最短覆盖窗口：

```python
left = 0
ans = inf

for right in range(len(s)):
    add s[right]

    while window is valid:
        ans = min(ans, right - left + 1)
        remove s[left]
        left += 1
```

最大区别：

```text
longest: 窗口合法后更新答案。
shortest: 窗口合法时尽量收缩，并在收缩前更新答案。
```

这点很关键。

## Sliding Window 的三问

每道 sliding window 题先问：

```text
1. 窗口状态是什么？
    count map, sum, distinct count, max frequency, matched count

2. 窗口什么时候非法？
    duplicate exists, distinct > k, sum > target, missing required chars

3. 答案什么时候更新？
    valid after shrink, or before shrink
```

如果这三问答不出来，代码就会乱。

## Sliding Window 常见坑

```text
1. 忘记删除 count 为 0 的 key
    distinct count 会错。

2. right - left + 1 写错
    长度边界容易错。

3. while 写成 if
    有些窗口需要连续收缩到合法。

4. 最短窗口更新时机错
    应该在仍然 valid 时更新，然后再收缩。

5. 状态更新顺序错
    先 remove 再判断，还是先判断再 remove，要根据题意。
```

## 三类题的关系

可以这样理解：

```text
Arrays & Hashing
    记录历史信息。

Two Pointers
    控制两个位置。

Sliding Window
    控制两个位置 + 维护中间状态。
```

也可以这样分类：

```text
需要快速查询 -> hashing
输入有序或可以排序 -> two pointers
连续子数组/子串 -> sliding window
```

很多题会组合：

```text
sliding window + hash map
two pointers + sorting
hashing + prefix sum
```

所以不要死记题型名称。
要看结构。

## 面试表达模板

Arrays & Hashing：

```text
The brute force solution checks every pair, which is O(n^2).
We can reduce lookup to O(1) by storing previously seen values in a hash map.
This gives O(n) time and O(n) space.
```

Two Pointers：

```text
Because the array is sorted, we can maintain two pointers.
If the current sum is too small, moving the left pointer right is the only way to increase it.
If the sum is too large, moving the right pointer left is the only way to decrease it.
So each step safely eliminates part of the search space.
```

Sliding Window：

```text
Since the target is a contiguous subarray or substring, we maintain a window with left and right pointers.
We expand the right boundary to include new elements, and shrink the left boundary while the window violates the constraint.
The state inside the window is updated incrementally, so the total time is O(n).
```

## Agent Harness 视角

如果把这三类题给 coding agent 做评测，要看：

```text
1. 是否识别连续区间和非连续结构。
2. 是否选择 hash map 而不是暴力嵌套循环。
3. 是否能讲清楚指针移动的 invariant。
4. 是否能处理空字符串、重复元素、负数、单元素。
5. 是否能给出 time / space complexity。
6. 失败后是否能从 edge case 定位窗口更新顺序问题。
```

这三类题非常适合作为 coding agent 的基础 reasoning benchmark。

因为它们不需要复杂库，主要考：

```text
pattern recognition
state maintenance
boundary discipline
implementation precision
```

## Quant / Engineering 迁移

这些能力在量化和工程里也很常见。

```text
Arrays & Hashing
    symbol -> features
    date -> bars
    factor_name -> values
    order_id -> status

Sliding Window
    rolling mean
    rolling volatility
    moving max/min
    drawdown window

Two Pointers
    merge sorted event streams
    align timestamps
    scan buy/sell boundaries
```

所以这不是“刷题技巧”。
这是处理序列数据的基本工程能力。

## 当前结论

`NEETCODE001` 的核心结论：

```text
Arrays & Hashing 训练快速查询和状态记录。
Two Pointers 训练边界推进和不变量。
Sliding Window 训练连续区间的动态状态维护。
```

这三类题是 NeetCode 的地基。
后面所有更复杂题型，都会依赖这里的状态意识和边界纪律。
