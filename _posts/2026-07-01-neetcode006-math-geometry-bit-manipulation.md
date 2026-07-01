---
title: "NEETCODE006: Math & Geometry / Bit Manipulation"
date: 2026-07-01 00:00:00 +0800
categories: [Learning, Coding Interview]
tags: [pengyi-neetcode-map, neetcode006, math, geometry, bit-manipulation, matrix, dsa, coding-interview]
---

这是 `PENGYI_NEETCODE_MAP` 的第七篇：

```text
NEETCODE006 -> Math & Geometry / Bit Manipulation
```

这一篇处理两类容易被低估的题：

```text
Math & Geometry
Bit Manipulation
```

它们的共同点是：

```text
思路通常不长，但实现必须精确。
```

很多人觉得这类题“不像算法题”。
但它们非常考工程师的底层表达能力：

```text
坐标
矩阵
边界
整数
取模
二进制表示
状态压缩
```

## 一句话模型

```text
Math & Geometry = convert the problem into formulas, coordinates, and invariants.
Bit Manipulation = use binary representation to encode parity, sets, and state.
```

中文：

```text
Math & Geometry 训练公式化和坐标边界。
Bit Manipulation 训练底层表示和状态压缩。
```

## Math & Geometry

典型信号：

```text
matrix
rotate
spiral
pow
integer
coordinates
rectangle
square
number theory
modulo
simulation
```

常见题型：

```text
rotate image
spiral matrix
set matrix zeroes
pow(x, n)
happy number
plus one
multiply strings
detect squares
rectangle overlap
```

这类题的核心不是复杂算法。
而是：

```text
把规则精确翻译成代码。
```

## Matrix 坐标纪律

矩阵题先明确：

```text
rows = len(matrix)
cols = len(matrix[0])
r in [0, rows)
c in [0, cols)
```

方向数组：

```python
directions = [(1, 0), (-1, 0), (0, 1), (0, -1)]
```

越界判断：

```python
0 <= r < rows and 0 <= c < cols
```

很多 matrix bug 都来自：

```text
row/col 混淆。
边界写成 len(matrix) 和 len(matrix[0]) 反了。
```

## Spiral Matrix

Spiral Matrix 是边界收缩。

四个边界：

```text
top
bottom
left
right
```

每走一圈：

```text
left -> right
top += 1

top -> bottom
right -= 1

right -> left
bottom -= 1

bottom -> top
left += 1
```

关键：

```text
每一段走之前检查边界是否仍然有效。
```

模板：

```python
res = []
top, bottom = 0, rows - 1
left, right = 0, cols - 1

while top <= bottom and left <= right:
    for c in range(left, right + 1):
        res.append(matrix[top][c])
    top += 1

    for r in range(top, bottom + 1):
        res.append(matrix[r][right])
    right -= 1

    if top <= bottom:
        for c in range(right, left - 1, -1):
            res.append(matrix[bottom][c])
        bottom -= 1

    if left <= right:
        for r in range(bottom, top - 1, -1):
            res.append(matrix[r][left])
        left += 1
```

## Rotate Image

旋转矩阵常用两步：

```text
transpose
reverse each row
```

顺时针 90 度：

```python
n = len(matrix)

for r in range(n):
    for c in range(r + 1, n):
        matrix[r][c], matrix[c][r] = matrix[c][r], matrix[r][c]

for row in matrix:
    row.reverse()
```

这类题训练的是：

```text
把几何变换拆成简单操作。
```

## Fast Power

Pow(x, n) 不能线性乘 n 次。
应该用快速幂：

```python
def my_pow(x, n):
    if n < 0:
        x = 1 / x
        n = -n

    ans = 1

    while n:
        if n % 2 == 1:
            ans *= x
        x *= x
        n //= 2

    return ans
```

核心：

```text
x^n 可以通过二进制展开。
每次平方 base，n 右移。
```

复杂度：

```text
O(log n)
```

## Happy Number

Happy number 本质是 cycle detection。

可以用 set：

```python
seen = set()

while n != 1 and n not in seen:
    seen.add(n)
    n = next_number(n)

return n == 1
```

也可以用 fast/slow pointer。

训练点：

```text
数学题也可能变成图或链表环检测。
```

## Math & Geometry 常见坑

```text
1. row / col 混淆。
2. 边界检查少一层。
3. n 为负数。
4. 整数除法和浮点除法混淆。
5. 取模时忘记负数行为。
6. 矩阵原地修改污染后续判断。
7. 坐标映射不一致。
```

## Bit Manipulation

Bit Manipulation 训练二进制表示。

常见操作：

```text
AND    &
OR     |
XOR    ^
NOT    ~
SHIFT  << >>
MASK
```

典型题：

```text
single number
number of 1 bits
counting bits
missing number
reverse bits
sum of two integers
subsets with bitmask
```

核心性质：

```text
x ^ x = 0
x ^ 0 = x
x & 1 判断最低位
x >> 1 右移一位
x & (x - 1) 去掉最低位的 1
```

## XOR

XOR 最经典用途：

```text
成对抵消。
```

Single Number：

```python
ans = 0

for x in nums:
    ans ^= x

return ans
```

为什么成立：

```text
相同数字 XOR 后为 0。
0 XOR 剩下的唯一数字就是它自己。
```

Missing Number：

```python
ans = len(nums)

for i, x in enumerate(nums):
    ans ^= i
    ans ^= x

return ans
```

## Count Bits

Brian Kernighan 技巧：

```python
count = 0

while n:
    n &= n - 1
    count += 1
```

每次：

```text
n & (n - 1) 会移除 n 最低位的 1。
```

所以循环次数等于 1 的数量。

DP 版本 counting bits：

```python
dp = [0] * (n + 1)

for i in range(1, n + 1):
    dp[i] = dp[i >> 1] + (i & 1)

return dp
```

## Bitmask 表示集合

如果有 n 个元素，可以用 n 位二进制表示子集。

```text
mask 的第 i 位为 1 -> 选中第 i 个元素。
```

枚举所有 subset：

```python
for mask in range(1 << n):
    subset = []
    for i in range(n):
        if mask & (1 << i):
            subset.append(nums[i])
```

这种方式适合：

```text
n 比较小。
需要枚举所有组合。
需要状态压缩 DP。
```

## Bit Manipulation 常见坑

```text
1. 运算优先级
    加括号更安全。

2. 负数表示
    Python 整数无限精度，和 32-bit 语言不同。

3. shift 方向
    << 乘 2，>> 除 2 的整数近似。

4. mask 越界
    1 << n 的 n 是否正确。

5. XOR 使用前提
    成对出现才适合抵消。
```

## Math 和 Bit 的关系

二者都要求：

```text
精确表达。
```

Math & Geometry：

```text
把空间、坐标、数值规则表达成代码。
```

Bit Manipulation：

```text
把状态、集合、奇偶性表达成二进制。
```

它们不一定长，但经常需要一次写对。

## Agent Harness 视角

这类题可以测试 coding agent 的底层精度。

```text
Math & Geometry
    是否能正确处理 row/col。
    是否能稳定处理边界。
    是否能解释公式和不变量。

Bit Manipulation
    是否理解 XOR 抵消。
    是否能设计 mask。
    是否能处理 32-bit 限制或语言差异。
```

这类题特别适合检查：

```text
edge-case precision
low-level reasoning
implementation discipline
```

## Quant / Engineering 迁移

```text
Math & Geometry
    coordinate transform
    matrix operations
    numerical stability
    simulation
    feature transformation

Bit Manipulation
    compact state
    flags
    permissions
    subset enumeration
    efficient encoding
```

在工程系统里，bitmask 也经常用于：

```text
feature flags
permission masks
state compression
fast membership
```

## 面试表达模板

Math & Geometry：

```text
I first define the coordinate system and boundary variables.
Then I translate the geometric transformation into a sequence of index operations.
The main risk is off-by-one and row-column confusion, so I explicitly maintain top, bottom, left, and right boundaries.
```

Bit Manipulation：

```text
I use XOR because identical numbers cancel out:
x XOR x is 0 and x XOR 0 is x.
So if every number appears twice except one, XOR over all numbers leaves exactly the unique number.
```

## 当前结论

`NEETCODE006` 的核心结论：

```text
Math & Geometry 训练公式化、坐标和边界。
Bit Manipulation 训练二进制表示、mask 和状态压缩。
```

这类题不会总是很长。
但它们非常能检测工程师是否有精确实现能力。
