---
title: "NEETCODE007: Coding Interview Execution OS"
date: 2026-07-01 00:00:00 +0800
categories: [Learning, Coding Interview]
tags: [pengyi-neetcode-map, neetcode007, coding-interview, execution-os, dsa, interview-prep, coding-agent, harness]
---

这是 `PENGYI_NEETCODE_MAP` 的第八篇：

```text
NEETCODE007 -> Coding Interview Execution OS
```

前面几篇已经按题型拆过：

```text
NEETCODE000: 总地图
NEETCODE001: Arrays & Hashing / Two Pointers / Sliding Window
NEETCODE002: Stack / Binary Search / Linked List
NEETCODE003: Trees / Heap / Tries
NEETCODE004: Backtracking / Graphs / Advanced Graphs
NEETCODE005: DP / Greedy / Intervals
NEETCODE006: Math & Geometry / Bit Manipulation
```

这一篇不是再讲题型。
这一篇做执行系统。

目标是：

```text
把刷题变成一套可重复、可诊断、可提升的 coding interview OS。
```

## 一句话定义

```text
Coding Interview Execution OS = a repeatable workflow for understanding, modeling, coding, testing, explaining, and improving algorithm solutions.
```

中文：

```text
Coding Interview Execution OS = 面对一道题时，从读题到讲解到复盘的标准作战流程。
```

不是：

```text
看答案
背模板
刷数量
```

而是：

```text
题型识别
状态建模
算法选择
复杂度控制
代码实现
边界测试
面试表达
错误复盘
```

## 面试中的 8 步流程

每道题都按这 8 步走。

```text
1. Restate
    用自己的话复述题目。

2. Clarify
    确认输入、输出、约束、边界。

3. Examples
    手动跑样例，补充 edge cases。

4. Brute Force
    给出最直接解法和复杂度。

5. Pattern
    识别题型和核心结构。

6. Optimize
    选择数据结构或算法。

7. Code
    写清晰、稳定、可解释的代码。

8. Test & Explain
    跑样例、edge cases，并解释复杂度。
```

这就是我们的基本 OS。

## Step 1: Restate

复述题目不是浪费时间。
它有两个作用：

```text
1. 确认你理解了题目。
2. 给自己争取建模时间。
```

模板：

```text
Let me restate the problem.
We are given ...
We need to return ...
The important constraints seem to be ...
```

中文思路：

```text
输入是什么？
输出是什么？
是否只要一个答案？
是否要所有答案？
是否需要原地修改？
是否可以改变输入顺序？
```

## Step 2: Clarify

Clarify 问题要短，但关键。

常见问题：

```text
Can the input be empty?
Can values be negative?
Are there duplicates?
Is the input sorted?
Should I return indices or values?
If multiple answers exist, can I return any one?
Do we need to optimize for time or memory?
```

不是每题都要问一堆。
要根据题目结构问最关键的。

## Step 3: Examples

不要只看给定样例。
自己补 edge cases：

```text
empty input
single element
all same elements
duplicates
negative values
already sorted
reverse sorted
no solution
multiple solutions
minimum size
maximum stress
```

手动跑样例时，要观察 pattern：

```text
是 pair？
是 contiguous window？
是 prefix？
是 graph connectivity？
是 state transition？
```

这一步会决定题型识别。

## Step 4: Brute Force

先讲 brute force 是好的。

它展示：

```text
你理解问题。
你知道最直接的方法。
你能分析复杂度。
你知道为什么需要优化。
```

模板：

```text
The brute force approach is to check all pairs/subarrays/states.
That would take O(...), which may be too slow for the constraint.
We need to avoid repeated work by using ...
```

Brute force 不是低级。
它是优化的起点。

## Step 5: Pattern Recognition

题型识别 prompt：

```text
Array / string?
    hashing, two pointers, sliding window, prefix sum

Sorted or monotonic?
    binary search, two pointers

Contiguous?
    sliding window, prefix sum, intervals

Tree?
    DFS, BFS, recursion, LCA

Graph?
    BFS, DFS, topo, union find, Dijkstra

All combinations?
    backtracking

Optimal substructure?
    DP

Local optimal seems safe?
    greedy

Dynamic min/max?
    heap

Prefix words?
    trie

Bit representation?
    bit manipulation
```

这张 prompt 要练到自动化。

## Step 6: Optimize

优化不是乱加数据结构。
优化要回答：

```text
当前重复工作是什么？
什么结构可以消除重复？
是否能用空间换时间？
是否有单调性？
是否有可缓存的子问题？
是否可以排序后简化关系？
```

常见优化路线：

```text
O(n^2) pair search -> hash map
O(n^2) substring scan -> sliding window
linear search answer -> binary search on answer
repeated min/max scan -> heap
exponential repeated recursion -> memo / DP
interval pairwise compare -> sort and scan
```

优化要能讲 tradeoff。

```text
This reduces time from O(n^2) to O(n), at the cost of O(n) extra space.
```

## Step 7: Code

代码要求：

```text
清晰
正确
边界稳定
变量名可读
不炫技
```

写代码前先定 skeleton：

```text
初始化
主循环 / 递归
状态更新
返回值
```

重要习惯：

```text
1. 先写 base case。
2. 再写主逻辑。
3. 对复杂状态写短注释。
4. 不要在面试里写过度抽象。
5. 保持变量名语义明确。
```

代码不是越短越好。
面试里更重要的是：

```text
正确、可读、可解释。
```

## Step 8: Test & Explain

写完不要立刻说 done。
要测试。

测试顺序：

```text
1. sample input
2. minimum input
3. edge case
4. failure case
5. complexity stress case
```

解释复杂度：

```text
Time Complexity:
    每个元素被处理几次？
    是否排序？
    是否有 heap log k？
    是否有递归分支？

Space Complexity:
    hash map / stack / recursion depth / dp table / output size
```

面试表达：

```text
Each element is pushed and popped at most once, so the time complexity is O(n).
The stack can hold up to n elements, so the space complexity is O(n).
```

## 题型到执行动作映射

```text
Arrays & Hashing
    写 hash map key/value。
    检查 duplicate、count、index。

Two Pointers
    定义 invariant。
    决定移动 left 还是 right。

Sliding Window
    定义 window state。
    决定 valid / invalid。
    决定 answer update 时机。

Stack
    决定 stack 存 value 还是 index。
    决定何时 pop。

Binary Search
    定义 search space。
    定义 monotonic check。
    决定返回 left/right。

Linked List
    使用 dummy。
    保存 next。
    小心 head 改变。

Trees
    定义递归函数返回值。
    base case。
    是否需要全局答案。

Heap
    决定 priority。
    决定 heap item tuple。
    处理 tie-breaker。

Backtracking
    choose / explore / undo。
    path copy。
    去重和剪枝。

Graphs
    node / edge / direction / weight。
    visited 标记。
    BFS / DFS / topo / union find。

DP
    state / transition / base / order / answer。

Greedy
    local choice + proof.

Intervals
    sort key + overlap condition.

Math / Bit
    formula + boundary + representation.
```

## 错题复盘系统

每道错题都要记录：

```text
Problem
    题目名称。

Category
    题型。

Failed Point
    识别错 / 状态错 / 代码错 / 边界错 / 复杂度错。

Root Cause
    真正原因。

Correct Pattern
    正确套路。

Minimal Template
    最小可复用模板。

Edge Cases
    这题容易挂的输入。

One-minute Explanation
    面试口述版本。

Redo Date
    复刷时间。
```

错题不是羞耻记录。
错题是训练数据。

## 训练节奏

不要只追求一天刷很多题。
更推荐：

```text
每天 2-4 题高质量训练。
每周 1 次题型复盘。
每两周 1 次 mock interview。
每月 1 次总表重构。
```

每题要经历：

```text
独立思考
写出 brute force
优化
实现
测试
看参考
复写
隔天重做
一周后重做
```

真正掌握的标准：

```text
不用看答案能写出来。
能解释为什么这样做。
能说明复杂度。
能处理 edge cases。
能把题型迁移到变体。
```

## 面试沟通纪律

面试不是 silent coding。
要持续沟通。

好的沟通节奏：

```text
1. 先复述题目。
2. 讲 brute force。
3. 讲优化观察。
4. 讲数据结构选择。
5. 写代码时说明关键变量。
6. 写完主动跑样例。
7. 解释复杂度。
```

不要：

```text
一句话不说直接写。
写到一半突然换思路。
复杂度说错。
edge case 不测。
被指出 bug 后慌乱。
```

如果卡住：

```text
I am considering whether this can be modeled as a sliding window or prefix sum problem.
The key constraint is contiguous subarray, so I will try to maintain a window state.
```

这比沉默好。

## 语言选择和实现纪律

如果用 Python：

必须熟悉：

```text
dict / set
list as stack
collections.deque
collections.defaultdict
collections.Counter
heapq
bisect
sort key
recursion limit awareness
```

但面试里不要过度依赖库。
例如：

```text
heapq 可以用。
Counter 可以用。
但核心算法要自己讲清楚。
```

实现纪律：

```text
变量名短但清楚。
不要写一坨复杂 list comprehension。
递归要先写 base case。
循环边界要明确。
```

## Complexity OS

复杂度必须说准确。

常见：

```text
Hash map scan
    Time O(n), Space O(n)

Two pointers
    Time O(n), Space O(1)

Sorting + scan
    Time O(n log n), Space depends on sort

Heap top-k
    Time O(n log k), Space O(k)

BFS / DFS graph
    Time O(V + E), Space O(V)

Backtracking
    Time often exponential, explain by branching factor and depth

DP table
    Time number_of_states * transition_cost
    Space number_of_states
```

不要机械说 O(n)。
要知道为什么。

## Coding Agent Harness 版本

这套 OS 可以转成 coding agent harness。

任务结构：

```text
problem statement
hidden tests
expected complexity
edge cases
reference solution
rubric
```

agent trajectory：

```text
read problem
restate
choose pattern
write solution
run tests
debug
explain complexity
final answer
```

评估维度：

```text
problem understanding
pattern recognition
algorithmic correctness
implementation correctness
edge case coverage
complexity explanation
debug recovery
communication clarity
```

这就是把 NeetCode 从人类刷题变成 agent benchmark。

## Personal Credit OS 版本

如果要把 NeetCode 训练变成公开 credit，可以做：

```text
GitHub repo
    clean solutions
    categorized notes
    templates
    mistake logs

Website posts
    pattern maps
    explanation articles
    weekly progress

Mock interview notes
    question
    solution
    feedback
    improved version
```

不要只上传答案。
要上传：

```text
思路
复杂度
边界
复盘
迁移
```

这才是 credit。

## 最小每日训练模板

每天做题记录可以用：

```text
Date:
Problem:
Category:
Difficulty:

My first idea:
Brute force:
Optimized idea:
Key invariant/state:
Code:
Time complexity:
Space complexity:
Edge cases:
Mistake:
One-minute explanation:
Need redo:
```

这就是训练数据结构化。

## 面试最终表达模板

可以背一套高质量开场：

```text
Let me first restate the problem and clarify a few edge cases.
The brute force approach would be ...
Given the constraints, we need a more efficient approach.
The key observation is ...
This suggests using ...
The invariant is ...
Now I will implement it and then test it on the sample and edge cases.
```

收尾：

```text
The time complexity is ...
The space complexity is ...
The main edge cases are ...
```

这套表达非常重要。
coding interview 不只是代码。
它也是沟通。

## 当前结论

`NEETCODE007` 的核心结论：

```text
NeetCode 的终点不是刷完 150 题。
终点是形成一套可重复执行的 Coding Interview OS。
```

这套 OS 包括：

```text
读题
澄清
样例
暴力
识别题型
优化
实现
测试
解释
复盘
迁移
```

对我们来说，它还会变成：

```text
coding interview preparation system
coding agent harness benchmark system
GitHub / website credit system
research engineering foundation
```

这才是 `PENGYI_NEETCODE_MAP` 的真正目标。
