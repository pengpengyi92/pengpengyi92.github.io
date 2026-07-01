---
title: "NEETCODE003: Trees / Heap / Tries"
date: 2026-07-01 00:00:00 +0800
categories: [Learning, Coding Interview]
tags: [pengyi-neetcode-map, neetcode003, trees, heap, priority-queue, tries, dsa, coding-interview]
---

这是 `PENGYI_NEETCODE_MAP` 的第四篇：

```text
NEETCODE003 -> Trees / Heap / Tries
```

这一篇进入三种非常重要的数据结构：

```text
Trees
Heap / Priority Queue
Tries
```

它们共同训练的是：

```text
非线性结构上的组织能力。
```

数组和字符串是一维线性结构。
Tree、Heap、Trie 则要求我们理解：

```text
层级
优先级
前缀索引
```

## 一句话模型

```text
Tree = hierarchical recursive structure.
Heap = dynamic priority structure.
Trie = prefix indexing structure.
```

中文：

```text
Tree 训练递归语义。
Heap 训练动态最值调度。
Trie 训练字符串前缀索引。
```

这三类题都非常工程化。
它们不是“会背算法”就够了，而是要知道结构为什么适合这个问题。

## Trees

Tree 是递归训练的核心。

典型信号：

```text
root
left
right
height
depth
path
subtree
ancestor
BST
level order
serialize
```

Tree 题最重要的问题不是“DFS 还是 BFS”。
而是：

```text
当前节点需要从子节点拿什么信息？
当前节点要向父节点返回什么信息？
是否需要维护全局答案？
```

## Tree DFS 模板

```python
def dfs(node):
    if not node:
        return base_value

    left = dfs(node.left)
    right = dfs(node.right)

    # use left, right, node.val
    # update answer if needed

    return value_to_parent
```

这段模板的核心是：

```text
递归函数的语义。
```

例如求树高：

```python
def height(node):
    if not node:
        return 0

    return 1 + max(height(node.left), height(node.right))
```

这里 `height(node)` 的语义非常明确：

```text
返回以 node 为根的子树高度。
```

## Tree BFS 模板

Level order：

```python
from collections import deque

if not root:
    return []

queue = deque([root])
ans = []

while queue:
    level = []

    for _ in range(len(queue)):
        node = queue.popleft()
        level.append(node.val)

        if node.left:
            queue.append(node.left)
        if node.right:
            queue.append(node.right)

    ans.append(level)

return ans
```

BFS 适合：

```text
level order
minimum depth
right side view
zigzag traversal
```

DFS 适合：

```text
height
diameter
path sum
LCA
BST validation
recursive serialization
```

## BST

Binary Search Tree 的性质：

```text
left subtree values < node.val < right subtree values
```

常见题：

```text
validate BST
kth smallest
lowest common ancestor in BST
insert / delete / search
```

Validate BST 不能只检查：

```text
node.left.val < node.val < node.right.val
```

这是错的。
必须检查整个区间：

```python
def valid(node, low, high):
    if not node:
        return True

    if not (low < node.val < high):
        return False

    return valid(node.left, low, node.val) and valid(node.right, node.val, high)
```

训练点：

```text
递归时传递约束，而不是只看局部。
```

这和 agent 任务里的 constraint propagation 很像。

## Tree 常见坑

```text
1. base case 错
    None 返回什么取决于题目。

2. 递归返回值语义不清
    return height, path, boolean, or tuple?

3. 全局答案和返回值混淆
    diameter 常见。

4. BST 只检查局部
    必须传 low/high。

5. path copy 问题
    回溯时要 append/pop 或 copy。

6. 空树
    root is None 必须处理。
```

## Heap / Priority Queue

Heap 是动态优先级结构。

适合：

```text
top k
merge k sorted lists
median from data stream
task scheduler
k closest
shortest path
```

核心问题：

```text
每一步我最应该处理哪个元素？
```

Heap 的能力：

```text
push: O(log n)
pop min/max: O(log n)
peek min/max: O(1)
```

Python 默认是 min-heap：

```python
import heapq

heap = []
heapq.heappush(heap, x)
smallest = heapq.heappop(heap)
```

Max-heap 通常用负数：

```python
heapq.heappush(heap, -x)
largest = -heapq.heappop(heap)
```

## Heap 的核心套路

### 1. Top K

如果要找 k largest，可以维护一个 size k 的 min-heap：

```python
heap = []

for x in nums:
    heapq.heappush(heap, x)
    if len(heap) > k:
        heapq.heappop(heap)

return heap[0]
```

直觉：

```text
heap 里始终保留目前最大的 k 个元素。
heap[0] 是这 k 个里最小的，也就是第 k 大。
```

### 2. K-way Merge

用于 merge k sorted lists：

```text
每个 list 当前头元素进 heap。
每次弹出最小值，再把它所在 list 的下一个元素放进 heap。
```

训练点：

```text
heap item 里要存 value 和来源。
```

### 3. Two Heaps

Median stream 常用：

```text
max heap for lower half
min heap for upper half
```

保持：

```text
len difference <= 1
all lower <= all upper
```

这类题训练动态平衡结构。

## Heap 常见坑

```text
1. heap 里存 tuple 时比较规则
    Python 会按 tuple 从前到后比较。

2. value 相等时对象不可比较
    需要加 index 作为 tie-breaker。

3. max-heap 忘记取负
    Python 默认 min-heap。

4. top k 用错堆大小
    k largest 用 size k min-heap。

5. lazy deletion
    动态删除不在堆顶的元素时，需要额外 map。
```

## Tries

Trie 是前缀树。

适合字符串集合：

```text
insert word
search word
startsWith prefix
word dictionary with wildcard
word search pruning
autocomplete
```

基本结构：

```python
class TrieNode:
    def __init__(self):
        self.children = {}
        self.is_word = False
```

插入：

```python
def insert(root, word):
    node = root
    for ch in word:
        if ch not in node.children:
            node.children[ch] = TrieNode()
        node = node.children[ch]
    node.is_word = True
```

查找：

```python
def search(root, word):
    node = root
    for ch in word:
        if ch not in node.children:
            return False
        node = node.children[ch]
    return node.is_word
```

前缀：

```python
def starts_with(root, prefix):
    node = root
    for ch in prefix:
        if ch not in node.children:
            return False
        node = node.children[ch]
    return True
```

## Trie 的价值

Trie 的核心是：

```text
共享前缀。
```

如果有这些词：

```text
car
cat
cart
care
```

它们共享：

```text
ca
```

Trie 把这个共享结构显式表达出来。

这和 retrieval index、prefix search、代码补全、搜索建议都有关。

## Trie 常见坑

```text
1. 忘记 is_word
    prefix 存在不等于 word 存在。

2. children 结构设计
    dict 灵活，array 更快但只适合固定字符集。

3. wildcard search 需要 DFS
    遇到 "." 要遍历所有 children。

4. Word Search 需要剪枝
    board DFS + trie 可以提前停止无效路径。

5. 删除 word 更复杂
    需要处理共享前缀，普通面试少问。
```

## 三类结构的关系

```text
Tree
    层级结构。

Heap
    带优先级的近似树结构，但只暴露 top。

Trie
    字符串路径构成的树。
```

从 coding interview 角度：

```text
Tree 训练递归。
Heap 训练调度。
Trie 训练索引。
```

从 agent / research OS 角度：

```text
Tree
    task decomposition, decision tree, syntax tree

Heap
    priority scheduler, top-k candidate selection

Trie
    prefix memory, command completion, structured retrieval
```

## Agent Harness 视角

这三类题可以测试 coding agent：

```text
Tree
    是否能定义递归函数语义。
    是否能处理 None base case。
    是否能区分返回值和全局答案。

Heap
    是否能选择正确 priority。
    是否知道 heap item 存什么。
    是否能处理 tie-breaker。

Trie
    是否能设计 node structure。
    是否区分 word 和 prefix。
    是否能把 DFS 和 Trie 结合。
```

这对 coding agent 很关键。
因为很多真实工程任务也要求：

```text
结构化表示
优先级调度
索引设计
```

## 面试表达模板

Tree：

```text
I define the recursive function by what it returns to its parent.
For each node, I solve the left and right subtrees first,
combine their results with the current node, update the global answer if needed,
and return the value required by the parent.
```

Heap：

```text
Since we repeatedly need the current smallest or largest candidate,
a heap gives O(log n) update and O(1) access to the top.
For top-k, I keep a heap of size k so the total complexity is O(n log k).
```

Trie：

```text
A trie is appropriate because the problem involves prefix queries over a set of words.
Each node represents a prefix, children represent next characters,
and an is_word flag distinguishes a complete word from a prefix.
```

## 当前结论

`NEETCODE003` 的核心结论：

```text
Tree 训练递归语义。
Heap 训练动态优先级。
Trie 训练前缀索引。
```

这三类题把我们从线性数组带到结构化数据。
它们也是工程系统里非常常见的三类结构：

```text
hierarchy
priority
index
```
