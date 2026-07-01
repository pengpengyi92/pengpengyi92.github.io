---
title: "NEETCODE000: NeetCode 题型总地图 - DSA / Coding Interview / Agent Harness 训练系统"
date: 2026-07-01 00:00:00 +0800
categories: [Learning, Coding Interview]
tags: [pengyi-neetcode-map, neetcode000, neetcode, dsa, coding-interview, algorithms, data-structures, coding-agent, agent-harness]
---

这是一个新的基础训练系列：

```text
PENGYI_NEETCODE_MAP
```

这一篇是：

```text
NEETCODE000 -> NeetCode 题型总地图
```

这篇不是开始刷题。
这篇先做总地图。

目标是把 NeetCode 这些题型翻译成我们自己的训练系统：

```text
DSA 基础能力
Coding Interview 执行能力
工程师算法直觉
Coding Agent Harness 评测能力
```

NeetCode 对我们不是一个单纯的题单。
它应该成为一个训练框架：

```text
题型识别 -> 数据结构选择 -> 状态建模 -> 复杂度控制 -> 代码实现 -> 边界测试 -> 面试表达
```

## 当前参考基线

这篇基于当前 NeetCode 官方公开结构做整理：

```text
NeetCode 150
    Blind 75 + 75 more problems
    total = 150
    categories = 18

NeetCode DSA for Beginners
    Arrays
    Linked Lists
    Recursion
    Sorting
    Binary Search
    Trees
    Backtracking
    Heap / Priority Queue
    Hashing
    Graphs
    Dynamic Programming
    Bit Manipulation

NeetCode Effective Guide
    know time complexity
    know space complexity
    know how to implement
    know how to use the structure in your language
```

也就是说，它不是乱序刷题。
它的价值在于：

```text
用相对结构化的顺序，把 coding interview 高频题型压成一套 pattern library。
```

## 一句话定义

我现在对 NeetCode 的定义：

```text
NeetCode = coding interview pattern library + DSA execution training harness.
```

中文：

```text
NeetCode = 用题型组织起来的数据结构与算法训练系统。
```

它训练的不是记答案。
它训练的是：

```text
看到题目 -> 判断题型 -> 建模状态 -> 选择结构 -> 写出复杂度正确的代码 -> 解释为什么正确
```

这正好是 coding interview 的核心。

## 为什么我们现在需要它

我们现在同时在走几条线：

```text
AI / Agent / Harness
Quant Research OS
Research Engineering
GitHub Credit OS
RA / PhD / AI role application
```

这些方向看起来不一定直接需要 LeetCode。
但底层能力非常相关。

NeetCode 能训练三种能力：

```text
1. 人的 coding interview 能力
2. 工程师基本算法与数据结构直觉
3. coding agent harness 的任务设计和评测能力
```

尤其第三点很关键。

如果我们要研究 DeepSeek / Codex / Claude Code 这种 coding agent，NeetCode 类题目就是一种标准化任务集：

```text
agent 能不能读题？
能不能识别 pattern？
能不能写出正确算法？
能不能处理 edge case？
能不能解释 time / space complexity？
能不能在失败后 debug？
```

所以 NeetCode 不只是求职训练。
它也可以成为 coding agent harness 的基础 benchmark 类型库。

## NeetCode 150 的 18 类题型

当前 NeetCode 150 的公开分类是：

```text
Arrays & Hashing          9
Two Pointers              5
Sliding Window            6
Stack                     6
Binary Search             7
Linked List               11
Trees                     15
Heap / Priority Queue     7
Backtracking              10
Tries                     3
Graphs                    13
Advanced Graphs           6
1-D Dynamic Programming   12
2-D Dynamic Programming   11
Greedy                    8
Intervals                 6
Math & Geometry           8
Bit Manipulation          7
```

总数：

```text
150
```

这 18 类不是平级知识点。
可以按能力分成几组。

## 第一组：线性结构与局部状态

这一组包括：

```text
Arrays & Hashing
Two Pointers
Sliding Window
Stack
Binary Search
Intervals
```

它们的共同点是：

```text
输入通常是一维序列、数组、字符串、区间列表。
核心能力是维护状态、控制边界、把 O(n^2) 压到 O(n) 或 O(n log n)。
```

这是 coding interview 的入门核心。
也是最容易被问到的一组。

## 第二组：指针、递归与层级结构

这一组包括：

```text
Linked List
Trees
Tries
```

它们训练的是：

```text
指针操作
递归分解
层级遍历
结构修改
树形状态传递
prefix index
```

这组题对代码细节要求很高。
尤其 linked list 和 tree，bug 多数不是思路问题，而是指针、空节点、返回值、边界处理出错。

## 第三组：搜索、图与组合空间

这一组包括：

```text
Backtracking
Graphs
Advanced Graphs
```

它们训练的是：

```text
搜索空间建模
visited 状态
BFS / DFS
拓扑关系
连通性
最短路
约束搜索
剪枝
```

这组题最接近 agent 和 research search。
因为它们不是只处理局部数组，而是在一个状态空间里探索。

## 第四组：优化、优先级与动态规划

这一组包括：

```text
Heap / Priority Queue
Greedy
1-D Dynamic Programming
2-D Dynamic Programming
Math & Geometry
Bit Manipulation
```

它们训练的是：

```text
最优子结构
状态转移
局部最优选择
优先级调度
数学建模
压缩表示
```

其中 DP 是很多人的难点。
但 DP 的本质并不是背题。
而是：

```text
定义状态 -> 定义转移 -> 定义初始化 -> 定义遍历顺序 -> 处理边界
```

这套能力和 ML / RL 的 state thinking 是连通的。

## 18 类题型训练表

下面是我们的总表。

| 题型 | 训练的核心能力 | 典型识别信号 | 常见坑 |
|---|---|---|---|
| Arrays & Hashing | 频次、索引、集合、映射、O(n) 状态压缩 | 出现 duplicate、anagram、two sum、frequency | hash key 设计错误、忽略负数/空输入 |
| Two Pointers | 双边界推进、排序结构利用、不变量 | sorted array、pair、palindrome、container | 指针移动条件错、重复元素处理错 |
| Sliding Window | 连续区间、动态维护窗口状态 | substring、subarray、longest/shortest contiguous | 何时扩、何时缩、窗口合法性判断 |
| Stack | LIFO、括号、单调栈、延迟处理 | parentheses、next greater、temperature、expression | push/pop 时机错、空栈错 |
| Binary Search | 单调性、答案空间、边界控制 | sorted、minimum feasible、rotated array | off-by-one、循环条件、返回值 |
| Linked List | 指针重连、dummy node、快慢指针 | reverse、cycle、merge、remove nth | 断链、空指针、返回头节点错 |
| Trees | DFS/BFS、递归返回值、路径、BST | height、path sum、LCA、level order | base case、全局变量、左右子树返回值 |
| Heap / Priority Queue | top-k、流式最值、调度 | k largest、median stream、merge k lists | heap 存什么、比较规则错 |
| Backtracking | 搜索树、选择-撤销、剪枝 | subsets、permutations、combination、board search | 忘记撤销、重复解、剪枝错误 |
| Tries | prefix search、词典索引、字符串树 | word dictionary、autocomplete、prefix | end-of-word 标记、节点结构 |
| Graphs | BFS/DFS、连通性、visited、grid | island、course schedule、clone graph | visited 时机、环检测、方向性 |
| Advanced Graphs | 最短路、MST、拓扑、加权图 | network delay、cheapest flight、alien dictionary | 权重、优先队列、拓扑条件 |
| 1-D DP | 一维状态、递推、滚动变量 | climb stairs、house robber、decode ways | 状态定义错、初始化错 |
| 2-D DP | 双序列、网格、区间、多维状态 | LCS、edit distance、unique paths | 维度含义错、遍历顺序错 |
| Greedy | 局部选择、交换论证、排序策略 | jump game、gas station、partition | 贪心正确性无法证明 |
| Intervals | 排序、合并、扫描线、资源占用 | meeting rooms、merge intervals、insert interval | 边界开闭、排序 key 错 |
| Math & Geometry | 公式化、坐标、矩阵、数论 | rotate image、spiral matrix、pow、happy number | 边界、溢出、坐标转换 |
| Bit Manipulation | 二进制表示、XOR、mask、subset | single number、missing number、bit count | 运算优先级、负数、mask 设计 |

## Arrays & Hashing

这是最基础，也最重要的一类。

核心问题：

```text
如何用 hash map / hash set 把重复扫描变成常数时间查询？
```

训练能力：

```text
frequency counting
index mapping
deduplication
complement lookup
grouping by normalized key
```

典型思路：

```text
two sum:
    遍历 nums
    对每个 x 查 target - x 是否出现
    用 hash map 记录 value -> index
```

它训练的是：

```text
用空间换时间。
```

面试表达：

```text
The brute force solution is O(n^2).
We can use a hash map to store previously seen values and check complements in O(1),
reducing the total time to O(n) with O(n) extra space.
```

## Two Pointers

Two pointers 的核心是：

```text
用两个指针维护一个结构化关系。
```

常见指针：

```text
left / right
slow / fast
read / write
```

常见前提：

```text
sorted array
palindrome structure
linked list cycle
in-place rewrite
```

训练能力：

```text
边界推进
不变量维护
重复元素跳过
空间压缩
```

关键不是“两个指针”这个名字。
关键是：

```text
每次移动指针，搜索空间为什么一定缩小？
```

## Sliding Window

Sliding window 是 two pointers 的连续区间版本。

核心问题：

```text
如何维护一个动态窗口，使它满足某个条件？
```

常见题眼：

```text
longest substring
minimum window
subarray
contiguous
at most k
without repeating
```

训练能力：

```text
窗口扩张
窗口收缩
窗口内状态维护
合法性判断
结果更新时机
```

模板：

```python
left = 0
state = {}

for right in range(len(nums)):
    add nums[right] to state

    while window is invalid:
        remove nums[left] from state
        left += 1

    update answer
```

最大坑：

```text
不知道什么时候 update answer。
```

有些题在窗口合法后更新。
有些题在收缩前更新。
需要根据目标是 longest 还是 shortest 判断。

## Stack

Stack 训练的是：

```text
后进先出 + 延迟处理。
```

常见类型：

```text
括号匹配
表达式计算
路径简化
单调栈
递归模拟
```

单调栈很重要。
它解决的是：

```text
为每个元素找到左边/右边第一个更大/更小的元素。
```

典型题眼：

```text
next greater element
daily temperatures
largest rectangle
```

训练能力：

```text
维护一个单调结构
在新元素破坏单调性时结算旧元素
```

这类题很适合训练“何时结算”的意识。

## Binary Search

Binary search 的本质不是“查有序数组”。
它的本质是：

```text
在单调答案空间里缩小搜索范围。
```

两种主要类型：

```text
search in sorted structure
search on answer
```

第一种：

```text
sorted array
rotated sorted array
matrix search
```

第二种：

```text
minimum speed
minimum capacity
minimum days
maximum feasible value
```

训练能力：

```text
定义单调性
写 check 函数
控制 left/right
确定返回值
```

面试表达：

```text
We can binary search the answer because feasibility is monotonic:
if value x works, then every larger value also works.
```

这句话非常关键。

## Linked List

Linked list 是低层指针训练。

常见技巧：

```text
dummy node
prev / curr / next
fast / slow pointer
reverse links
merge two lists
detect cycle
```

最大坑：

```text
链表结构被你改坏之后，很难恢复。
```

所以写 linked list 要有纪律：

```text
先保存 next
再改 curr.next
再移动 prev / curr
```

典型反转：

```python
prev = None
curr = head

while curr:
    nxt = curr.next
    curr.next = prev
    prev = curr
    curr = nxt

return prev
```

这类题训练的是工程师对引用和状态修改的精确控制。

## Trees

Tree 是递归分解的核心训练。

常见问题：

```text
遍历
高度
路径
直径
平衡
BST
LCA
序列化
```

Tree 题要问：

```text
当前节点需要从子树拿什么信息？
当前节点要向父节点返回什么信息？
是否需要全局答案？
```

DFS 模板：

```python
def dfs(node):
    if not node:
        return base

    left = dfs(node.left)
    right = dfs(node.right)

    update answer
    return value_to_parent
```

Tree 题的核心不是遍历。
核心是：

```text
递归函数的语义。
```

## Heap / Priority Queue

Heap 用于处理动态最值。

常见场景：

```text
top k
k-way merge
median stream
task scheduling
shortest path
```

训练能力：

```text
把每一步最需要处理的元素放在堆顶。
```

常见坑：

```text
heap 里存什么？
按什么排序？
是否需要 lazy deletion？
Python 是 min-heap，max-heap 需要取负数。
```

Heap 很像一个小型调度器。
这和 agent harness 里的 action priority 也有相通处。

## Backtracking

Backtracking 是搜索树训练。

核心结构：

```text
choose
explore
undo
```

模板：

```python
def backtrack(path, choices):
    if is_solution(path):
        ans.append(path.copy())
        return

    for choice in choices:
        if invalid(choice):
            continue

        path.append(choice)
        backtrack(path, next_choices)
        path.pop()
```

常见题：

```text
subsets
permutations
combinations
combination sum
word search
n queens
```

训练能力：

```text
组合空间建模
递归搜索
剪枝
去重
状态恢复
```

这类题和 research search 很像：

```text
每一步做一个选择，探索一条可能路径，失败就回退。
```

## Tries

Trie 是字符串前缀索引。

核心结构：

```text
root
  -> char node
  -> char node
  -> ...
```

每个节点通常有：

```text
children
is_word
optional value
```

适合：

```text
prefix search
word dictionary
autocomplete
word search with pruning
```

训练能力：

```text
把字符串集合组织成可查询结构。
```

这和 retrieval / index 的思想是连通的。

## Graphs

Graph 是关系结构。

核心对象：

```text
node
edge
visited
adjacency list
```

常见问题：

```text
连通分量
岛屿数量
路径存在性
环检测
拓扑排序
图克隆
网格 BFS / DFS
```

训练能力：

```text
把问题抽象成 node 和 edge。
```

Graph 题的第一步通常不是写代码。
而是问：

```text
什么是 node？
什么是 edge？
图是有向还是无向？
是否有权重？
是否需要 visited？
```

这和 Graph RAG、knowledge graph、agent state graph 都有直接联系。

## Advanced Graphs

Advanced Graphs 进入加权和复杂关系。

常见算法：

```text
Dijkstra
Bellman-Ford
Kruskal
Prim
Union Find
Topological Sort variants
```

训练能力：

```text
最短路
最小生成树
有向依赖
动态连通性
```

面试里最重要的是判断问题类型：

```text
unweighted shortest path -> BFS
non-negative weighted shortest path -> Dijkstra
negative edge possibility -> Bellman-Ford
connect components cheaply -> MST / Union Find
dependency ordering -> topological sort
```

这类题训练“算法选择器”。

## 1-D Dynamic Programming

1-D DP 是状态转移入门。

核心问题：

```text
当前位置的最优解，如何由之前位置推出来？
```

常见题：

```text
climbing stairs
house robber
decode ways
coin change
longest increasing subsequence
```

DP 五步：

```text
1. 定义 dp[i] 的含义
2. 写状态转移
3. 初始化 base case
4. 确定遍历顺序
5. 返回答案
```

如果 DP 做不出来，通常是第一步没定义清楚。

## 2-D Dynamic Programming

2-D DP 常见于：

```text
grid
two strings
interval
knapsack-like states
```

典型题：

```text
unique paths
longest common subsequence
edit distance
interleaving string
distinct subsequences
```

训练能力：

```text
多维状态建模。
```

关键是：

```text
dp[i][j] 到底代表什么？
```

比如 LCS：

```text
dp[i][j] = text1 前 i 个字符和 text2 前 j 个字符的最长公共子序列长度
```

只要状态定义清楚，转移就更容易写。

## Greedy

Greedy 的核心是：

```text
每一步做局部最优选择，并且这个选择能导向全局最优。
```

难点不是写代码。
难点是证明：

```text
为什么局部最优不会破坏全局最优？
```

常见证明思路：

```text
exchange argument
staying ahead
sorting by key
invariant
```

Greedy 常见题：

```text
jump game
gas station
partition labels
merge triplets
hand of straights
```

面试里要能说：

```text
The greedy choice is safe because...
```

否则只是猜。

## Intervals

Intervals 题训练事件流建模。

常见操作：

```text
sort by start
merge overlaps
insert interval
count rooms
erase overlap
sweep line
```

核心问题：

```text
区间之间如何相交、覆盖、冲突、排序？
```

常见坑：

```text
边界是闭区间还是半开区间？
排序按 start 还是 end？
重叠条件是 < 还是 <=？
```

这类题和调度、资源分配、时间窗口、风控时间段都有关。

## Math & Geometry

Math & Geometry 是把问题公式化。

常见类型：

```text
matrix rotation
spiral traversal
pow
happy number
plus one
rotate image
detect square
```

训练能力：

```text
坐标变换
矩阵边界
数学不变量
取模
循环检测
```

这类题的坑通常在：

```text
边界
索引
整数溢出
坐标映射
```

它们不一定算法复杂，但很考精确实现。

## Bit Manipulation

Bit manipulation 训练底层表示。

常见工具：

```text
XOR
AND
OR
shift
mask
lowbit
```

典型性质：

```text
x ^ x = 0
x ^ 0 = x
x & (x - 1) removes the lowest set bit
```

适合：

```text
single number
missing number
count bits
reverse bits
subset mask
```

这类题训练的是：

```text
把集合、状态、奇偶性压缩到二进制表示。
```

## Coding Interview Execution OS

NeetCode 最终要训练成一套执行流程。

每道题都按这 8 步走：

```text
1. Restate
    用自己的话复述题目，确认输入输出。

2. Constraints
    看 n 的范围，推断目标复杂度。

3. Examples
    手动跑样例，找 pattern。

4. Brute Force
    先说最直接解法和复杂度。

5. Pattern Match
    判断属于哪类题型。

6. Optimize
    选择数据结构或算法，把复杂度压下来。

7. Code
    写清晰、可解释、边界稳定的代码。

8. Test
    跑样例、edge cases、复杂度说明。
```

这就是我们的 coding interview OS。

## 题型识别 Prompt

以后每道题可以先问自己：

```text
输入是什么结构？
    array / string / linked list / tree / graph / matrix / intervals

输出是什么？
    value / index / list / boolean / path / count / optimized score

是否有连续区间？
    sliding window

是否有排序或单调性？
    two pointers / binary search / greedy

是否需要快速查找？
    hash map / hash set

是否需要最近未匹配元素？
    stack

是否需要动态最值？
    heap

是否是树形递归？
    DFS / BFS / tree DP

是否是关系网络？
    graph traversal / topological sort / shortest path

是否是枚举所有方案？
    backtracking

是否有重叠子问题？
    dynamic programming

是否可以局部最优？
    greedy

是否能用二进制压缩？
    bit manipulation
```

这个 prompt 要练到自动化。

## 对 Coding Agent Harness 的意义

如果我们把 NeetCode 用于 coding agent harness，可以设计这些评估维度：

```text
Problem understanding
    agent 是否正确复述题意。

Pattern recognition
    agent 是否识别正确题型。

Algorithm choice
    是否选择合理数据结构和复杂度。

Implementation correctness
    代码是否通过测试。

Edge case handling
    空输入、重复、边界、负数、大数。

Complexity explanation
    能否说明 time / space complexity。

Debug behavior
    失败后是否能根据错误修复。

Communication
    最终解释是否简洁准确。
```

这和我们之前做的 harness 思路完全一致：

```text
task
  -> action
  -> execution
  -> observation
  -> evaluation
  -> diagnosis
  -> next attempt
```

NeetCode 可以成为 coding agent 的小型标准环境。

## 对 Quant / Research OS 的意义

NeetCode 表面是算法题。
但对我们 Quant / Research OS 也有意义。

对应关系：

```text
Arrays & Hashing
    factor table、symbol map、feature grouping

Sliding Window
    rolling window、moving average、time-series feature

Binary Search
    threshold search、parameter search、feasibility boundary

Heap
    top-k assets、priority scheduling、event queue

Graphs
    asset relation graph、supply chain、knowledge graph

DP
    sequential decision、path optimization、allocation under constraints

Intervals
    holding period、event window、risk exposure interval
```

所以 DSA 不只是面试用。
它提升的是我们对数据结构、状态、复杂度的底层敏感度。

## 我们的学习路线

建议后续拆成：

```text
NEETCODE000: NeetCode 总地图
NEETCODE001: Arrays & Hashing / Two Pointers / Sliding Window
NEETCODE002: Stack / Binary Search / Linked List
NEETCODE003: Trees / Heap / Tries
NEETCODE004: Backtracking / Graphs / Advanced Graphs
NEETCODE005: DP / Greedy / Intervals
NEETCODE006: Math / Geometry / Bit Manipulation
NEETCODE007: Coding Interview Execution OS
NEETCODE008: Coding Agent Harness Benchmark Design
```

每一篇不只是列题。
都要回答：

```text
这个题型训练什么？
如何识别？
标准模板是什么？
常见坑是什么？
面试怎么讲？
如何迁移到工程和 agent harness？
```

## 训练策略

我们不应该用“刷题数量”作为唯一指标。
更合理的指标是：

```text
pattern coverage
first-principles explanation
implementation fluency
edge case reliability
time complexity awareness
redo ability
interview communication
```

每道题要产出：

```text
1. pattern
2. key idea
3. code
4. complexity
5. edge cases
6. mistake log
7. one-minute explanation
```

这才是可复用的训练。

## 面试可用表达

如果被问“你怎么准备 coding interview”，可以这样说：

```text
I organize coding interview preparation by patterns rather than by memorizing problems.
For each NeetCode category, I focus on the underlying data structure, the recognition signal,
the invariant, the implementation template, common edge cases, and the time-space tradeoff.

For example, sliding window problems train dynamic state maintenance over contiguous ranges,
binary search trains monotonic feasibility reasoning,
tree problems train recursive return semantics,
graph problems train state-space traversal,
and DP problems train state definition and transition design.
```

如果要接到 AI agent：

```text
I also view these problems as useful coding-agent benchmark tasks.
They test whether an agent can understand the problem, identify the pattern,
write correct code, handle edge cases, explain complexity, and recover from failed tests.
```

这就是把 NeetCode 从刷题变成系统训练。

## 当前结论

`NEETCODE000` 的核心结论：

```text
NeetCode = DSA pattern library + coding interview execution harness + coding agent benchmark substrate.
```

我们后面要把它变成自己的训练系统。

不是为了背 150 题。
而是为了形成：

```text
题型识别能力
算法建模能力
代码实现能力
复杂度表达能力
边界测试能力
agent 评测设计能力
```

这条线会直接支撑：

```text
AI / Quant role coding interview
Research engineering ability
DeepSeek coding agent harness thinking
GitHub credit building
```

## Sources

```text
NeetCode 150:
https://neetcode.io/practice/practice/neetcode150

NeetCode DSA for Beginners:
https://neetcode.io/courses/dsa-for-beginners/0

How to Use NeetCode Effectively:
https://neetcode.io/courses/lessons/how-to-use-neetcode-effectively
```
