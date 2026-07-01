---
title: "NEETCODE004: Backtracking / Graphs / Advanced Graphs"
date: 2026-07-01 00:00:00 +0800
categories: [Learning, Coding Interview]
tags: [pengyi-neetcode-map, neetcode004, backtracking, graphs, advanced-graphs, dfs, bfs, dijkstra, topological-sort]
---

这是 `PENGYI_NEETCODE_MAP` 的第五篇：

```text
NEETCODE004 -> Backtracking / Graphs / Advanced Graphs
```

这一篇进入搜索和图。

这三类题共同训练的是：

```text
状态空间建模。
```

数组题通常是一维扫描。
树题通常是层级递归。
Backtracking 和 Graph 则更像：

```text
在一个可能很大的状态空间里，系统地探索、剪枝、标记、回退、找路径或判断结构。
```

这和 agent、research planning、Graph RAG、quant strategy search 都高度相关。

## 一句话模型

```text
Backtracking = DFS over a decision tree with choose/explore/undo.
Graphs = traversal over nodes and edges with visited state.
Advanced Graphs = optimized traversal over weighted or constrained relations.
```

中文：

```text
Backtracking 训练组合搜索。
Graphs 训练关系网络遍历。
Advanced Graphs 训练加权关系优化。
```

## Backtracking

Backtracking 的核心是：

```text
搜索所有可能选择，但在每一步维护当前 path，并在回退时恢复状态。
```

典型信号：

```text
all combinations
all permutations
all subsets
all valid boards
word search
n queens
combination sum
generate parentheses
```

Backtracking 不是普通 DFS 的另一个名字。
它强调：

```text
做选择 -> 递归探索 -> 撤销选择
```

## Backtracking 标准模板

```python
def backtrack(path, start):
    if is_solution(path):
        ans.append(path.copy())
        return

    for i in range(start, len(choices)):
        if should_skip(i):
            continue

        path.append(choices[i])
        backtrack(path, next_start)
        path.pop()
```

三个关键点：

```text
1. path 表示当前已做选择。
2. start 或 visited 控制可选空间。
3. append 后必须 pop，恢复现场。
```

## Backtracking 的几种形态

### Subsets

每个元素选或不选：

```text
decision tree depth = n
branches = include / exclude
```

或者用 start 循环：

```python
def dfs(start):
    ans.append(path.copy())

    for i in range(start, len(nums)):
        path.append(nums[i])
        dfs(i + 1)
        path.pop()
```

### Permutations

每一层选择一个未使用元素：

```python
def dfs():
    if len(path) == len(nums):
        ans.append(path.copy())
        return

    for i in range(len(nums)):
        if used[i]:
            continue

        used[i] = True
        path.append(nums[i])
        dfs()
        path.pop()
        used[i] = False
```

### Combination Sum

关键是是否允许重复使用：

```text
允许重复 -> dfs(i)
不允许重复 -> dfs(i + 1)
```

这就是很多 backtracking 题的核心差异。

## Backtracking 常见坑

```text
1. 忘记 path.copy()
    ans 里会引用同一个 path 对象。

2. 忘记撤销选择
    path.pop 或 used[i] = False。

3. 去重逻辑错误
    排序后跳过同层重复，而不是跳过所有重复。

4. start 更新错误
    是否允许重复使用元素要清楚。

5. 剪枝条件太激进
    正确答案被剪掉。
```

## Graphs

Graph 的核心是：

```text
node + edge + traversal + visited
```

典型信号：

```text
islands
connected components
course schedule
clone graph
walls and gates
rotting oranges
pacific atlantic
valid tree
```

第一步不是写 BFS/DFS。
第一步是建模：

```text
什么是 node？
什么是 edge？
图是有向还是无向？
是否有权重？
是否需要检测环？
是否需要最短路径？
```

## Graph 表示方式

Adjacency list：

```python
graph = {node: [] for node in nodes}

for u, v in edges:
    graph[u].append(v)
    graph[v].append(u)  # if undirected
```

Grid graph：

```python
directions = [(1, 0), (-1, 0), (0, 1), (0, -1)]

for dr, dc in directions:
    nr = r + dr
    nc = c + dc
```

很多 matrix 题本质是 graph。

```text
cell = node
neighboring cell = edge
```

## DFS 模板

```python
def dfs(node):
    if node in visited:
        return

    visited.add(node)

    for nei in graph[node]:
        dfs(nei)
```

Grid DFS：

```python
def dfs(r, c):
    if r < 0 or r == rows or c < 0 or c == cols:
        return
    if grid[r][c] != "1":
        return

    grid[r][c] = "0"

    for dr, dc in directions:
        dfs(r + dr, c + dc)
```

训练点：

```text
visited 必须在进入时标记，避免重复和死循环。
```

## BFS 模板

```python
from collections import deque

queue = deque([start])
visited = {start}
distance = 0

while queue:
    for _ in range(len(queue)):
        node = queue.popleft()

        for nei in graph[node]:
            if nei not in visited:
                visited.add(nei)
                queue.append(nei)

    distance += 1
```

BFS 适合：

```text
unweighted shortest path
level expansion
multi-source spread
minimum steps
```

例如 rotting oranges：

```text
多个 rotten orange 同时作为 BFS 起点。
```

这叫 multi-source BFS。

## Topological Sort

拓扑排序用于有向无环图：

```text
dependencies
prerequisites
course schedule
build order
```

Kahn 算法：

```python
from collections import deque

indegree = {node: 0 for node in nodes}
graph = {node: [] for node in nodes}

for u, v in edges:
    graph[u].append(v)
    indegree[v] += 1

queue = deque([node for node in nodes if indegree[node] == 0])
order = []

while queue:
    node = queue.popleft()
    order.append(node)

    for nei in graph[node]:
        indegree[nei] -= 1
        if indegree[nei] == 0:
            queue.append(nei)

return order if len(order) == len(nodes) else []
```

核心判断：

```text
如果最终无法处理所有节点，说明存在 cycle。
```

## Union Find

Union Find 用于动态连通性。

适合：

```text
number of connected components
valid tree
redundant connection
minimum spanning tree
```

模板：

```python
parent = list(range(n))
rank = [1] * n

def find(x):
    while x != parent[x]:
        parent[x] = parent[parent[x]]
        x = parent[x]
    return x

def union(a, b):
    ra = find(a)
    rb = find(b)

    if ra == rb:
        return False

    if rank[ra] < rank[rb]:
        parent[ra] = rb
    elif rank[ra] > rank[rb]:
        parent[rb] = ra
    else:
        parent[rb] = ra
        rank[ra] += 1

    return True
```

训练点：

```text
find 找 root。
union 合并集合。
如果两个节点已经同 root，再 union 就发现环。
```

## Advanced Graphs

Advanced Graphs 主要进入：

```text
weighted graph
shortest path
minimum spanning tree
dependency order
```

关键是选算法。

```text
unweighted shortest path -> BFS
non-negative weighted shortest path -> Dijkstra
negative edge possible -> Bellman-Ford
minimum spanning tree -> Kruskal / Prim
dependency order -> topological sort
dynamic connectivity -> Union Find
```

这就是 graph 题的 algorithm router。

## Dijkstra

Dijkstra 用于：

```text
non-negative weighted shortest path
```

模板：

```python
import heapq

dist = {start: 0}
heap = [(0, start)]

while heap:
    cost, node = heapq.heappop(heap)

    if cost > dist.get(node, float("inf")):
        continue

    for nei, weight in graph[node]:
        new_cost = cost + weight
        if new_cost < dist.get(nei, float("inf")):
            dist[nei] = new_cost
            heapq.heappush(heap, (new_cost, nei))
```

核心：

```text
heap 每次弹出当前最短候选。
如果弹出的是旧状态，跳过。
```

常见坑：

```text
存在负权边时不能直接用 Dijkstra。
```

## Bellman-Ford

Bellman-Ford 可以处理负边。

核心思想：

```text
对所有边 relax n-1 轮。
```

适合：

```text
cheapest flights within k stops
negative edge discussion
limited stops path
```

NeetCode 里常见变体是限制 stops。
这时要注意：

```text
每一轮用上一轮 dist 的 copy，避免同一轮内连续使用多条边。
```

## Graph 常见坑

```text
1. visited 标记时机
    BFS 通常入队时标记，避免重复入队。

2. 有向 / 无向混淆
    edge 是否双向加入。

3. grid 边界
    r/c 是否越界。

4. cycle detection
    有向图和无向图方法不同。

5. topological indegree 方向
    prerequisite -> course，别反。

6. Dijkstra 过期状态
    heap 里可能有旧 cost，要跳过。

7. Union Find 节点编号
    0-index 和 1-index 要统一。
```

## Backtracking 和 Graph 的关系

Backtracking 可以看成：

```text
在隐式 decision graph 上 DFS。
```

Graph traversal 是：

```text
在显式 node-edge graph 上 DFS/BFS。
```

二者都需要：

```text
state
visited
choice
termination
rollback or traversal discipline
```

所以这两类题是 agent 思维的基础。

## Agent Harness 视角

这类题非常适合测试 agent 的长程推理。

评估点：

```text
Backtracking
    是否能定义 state 和 choices。
    是否能撤销状态。
    是否能处理重复解。

Graphs
    是否能建模 node / edge。
    是否正确使用 visited。
    是否判断 BFS / DFS / topo / union find。

Advanced Graphs
    是否能根据权重和约束选择 Dijkstra / Bellman-Ford / MST。
    是否能解释复杂度。
```

对 coding agent 来说，这类题比数组题更能暴露：

```text
state modeling
algorithm selection
long-horizon consistency
```

## Quant / Research OS 迁移

Graph 能力对我们很重要。

```text
Graph RAG
    entity / relation / community / path reasoning

Quant
    asset relation graph
    industry chain graph
    risk contagion graph
    lead-lag relation graph

Research OS
    paper citation graph
    idea dependency graph
    experiment lineage graph

Agent
    task graph
    tool call graph
    plan dependency graph
```

Backtracking 对应：

```text
hypothesis search
strategy combination search
parameter search
prompt/action sequence search
```

所以这一篇不是只为了算法题。
它直接连接我们的 agent 和 research system。

## 面试表达模板

Backtracking：

```text
I model the problem as a decision tree.
At each step, I choose one candidate, recurse into the next state,
and then undo the choice before trying the next candidate.
This systematically explores all valid combinations while allowing pruning.
```

Graphs：

```text
I first define the nodes and edges, then decide whether the graph is directed,
weighted, and whether we need shortest path, connectivity, or cycle detection.
For unweighted shortest path I use BFS; for traversal or components I use DFS/BFS;
for dependencies I use topological sort.
```

Advanced Graphs：

```text
If all edge weights are non-negative and we need shortest path, Dijkstra is appropriate.
If we need connectivity under dynamic unions, Union Find is appropriate.
If we need a dependency order, topological sorting is the right abstraction.
```

## 当前结论

`NEETCODE004` 的核心结论：

```text
Backtracking 训练隐式搜索树。
Graphs 训练显式关系网络。
Advanced Graphs 训练加权和约束关系优化。
```

这一组题是从 coding interview 走向 agent / Graph RAG / Research OS 的关键桥梁。
