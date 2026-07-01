---
title: "NEETCODE002: Stack / Binary Search / Linked List"
date: 2026-07-01 00:00:00 +0800
categories: [Learning, Coding Interview]
tags: [pengyi-neetcode-map, neetcode002, stack, binary-search, linked-list, dsa, coding-interview]
---

这是 `PENGYI_NEETCODE_MAP` 的第三篇：

```text
NEETCODE002 -> Stack / Binary Search / Linked List
```

这三类题看起来不完全相关。
但它们共同训练的是：

```text
低层控制能力。
```

具体来说：

```text
Stack
    控制最近状态、延迟结算、单调结构。

Binary Search
    控制搜索边界、利用单调性。

Linked List
    控制指针、引用和结构修改。
```

这三类题很容易暴露工程基本功。
思路不难时，真正难的是边界、顺序和实现精度。

## 一句话模型

```text
Stack = remember unresolved recent states.
Binary Search = exploit monotonicity to eliminate half the space.
Linked List = mutate pointer structure without losing references.
```

中文：

```text
Stack 处理“最近还没解决”的东西。
Binary Search 处理“有单调性的答案空间”。
Linked List 处理“指针重连和结构修改”。
```

## Stack

Stack 是 LIFO：

```text
last in, first out
```

它最适合处理：

```text
最近打开但还没关闭的结构。
最近加入但还没被结算的元素。
```

典型信号：

```text
parentheses
valid expression
next greater
previous smaller
temperature
path simplification
min stack
```

Stack 的核心能力是：

```text
把暂时无法决定的元素放起来，等未来信息出现后再结算。
```

## 普通 Stack 模板

括号匹配：

```python
pairs = {")": "(", "]": "[", "}": "{"}
stack = []

for ch in s:
    if ch in pairs.values():
        stack.append(ch)
    elif ch in pairs:
        if not stack or stack[-1] != pairs[ch]:
            return False
        stack.pop()

return not stack
```

训练点：

```text
遇到 opening，push。
遇到 closing，必须匹配 stack top。
最后 stack 必须为空。
```

## Monotonic Stack

单调栈是 Stack 的高频高级用法。

它解决的是：

```text
对每个元素，找左边/右边第一个更大/更小的元素。
```

典型题：

```text
Daily Temperatures
Next Greater Element
Largest Rectangle in Histogram
Car Fleet
```

核心思想：

```text
栈里保持一个单调结构。
当新元素破坏单调性时，说明一些旧元素可以被结算。
```

Daily Temperatures 模板：

```python
ans = [0] * len(temperatures)
stack = []  # indexes with unresolved warmer day

for i, temp in enumerate(temperatures):
    while stack and temp > temperatures[stack[-1]]:
        j = stack.pop()
        ans[j] = i - j
    stack.append(i)

return ans
```

这里 stack 存 index，不是 value。
因为答案需要距离。

## Stack 常见坑

```text
1. stack 里到底存 value 还是 index
    如果要计算距离或回写答案，通常存 index。

2. while 还是 if
    单调栈通常需要 while，一次新元素可能结算多个旧元素。

3. 单调递增还是递减
    取决于要找 next greater 还是 next smaller。

4. 相等时是否 pop
    取决于题目要求 strictly greater 还是 greater or equal。

5. 最后未结算元素如何处理
    有些题默认 0，有些题需要最后清算。
```

## Binary Search

Binary Search 的本质不是查找一个数。
它的本质是：

```text
利用单调性，每次排除一半答案空间。
```

两大类型：

```text
1. Search in sorted structure
    在有序数组、旋转数组、矩阵里查找。

2. Search on answer
    在答案空间里找最小可行值或最大可行值。
```

第二种更重要。
因为它体现的是抽象能力。

## Binary Search 的三问

每道二分题先问：

```text
1. 搜索空间是什么？
    index range, value range, answer range

2. 单调性是什么？
    如果 x 可行，那么更大/更小是否也可行？

3. check(x) 怎么写？
    判断某个候选值是否满足条件。
```

如果这三问答不出来，不要急着写代码。

## 标准 Binary Search 模板

查找有序数组：

```python
left = 0
right = len(nums) - 1

while left <= right:
    mid = (left + right) // 2

    if nums[mid] == target:
        return mid
    elif nums[mid] < target:
        left = mid + 1
    else:
        right = mid - 1

return -1
```

找最小可行答案：

```python
left = min_possible
right = max_possible

while left < right:
    mid = (left + right) // 2

    if feasible(mid):
        right = mid
    else:
        left = mid + 1

return left
```

找最大可行答案：

```python
left = min_possible
right = max_possible

while left < right:
    mid = (left + right + 1) // 2

    if feasible(mid):
        left = mid
    else:
        right = mid - 1

return left
```

注意最大可行答案里 `mid` 要向上取整。
否则可能死循环。

## Binary Search 常见坑

```text
1. while left < right 还是 left <= right
    closed interval 和 half-open interval 不要混用。

2. mid 偏左还是偏右
    最大可行值通常用 (left + right + 1) // 2。

3. feasible 单调方向写反
    一旦方向错，二分完全错。

4. 返回 left 还是 right
    循环结束时要知道 invariant。

5. sorted 被忽略
    没有单调性就不能二分。
```

面试表达：

```text
We can binary search the answer because feasibility is monotonic.
If a candidate capacity works, any larger capacity also works.
So we search for the minimum feasible capacity.
```

这就是高质量表达。

## Linked List

Linked List 训练的是指针控制。

常见信号：

```text
reverse
merge
cycle
remove nth
reorder
partition
copy random pointer
```

核心对象：

```text
node
node.next
head
tail
dummy
prev
curr
nxt
```

Linked list 题的难点不是算法复杂。
难点是：

```text
修改结构时不要丢引用。
```

## Dummy Node

dummy node 是 linked list 的关键技巧。

它解决：

```text
头节点可能被删除、替换、合并时，返回头节点困难。
```

模板：

```python
dummy = ListNode(0)
dummy.next = head

prev = dummy
curr = head

while curr:
    ...

return dummy.next
```

有 dummy 后，删除 head 和删除中间节点可以统一处理。

## Reverse Linked List

最经典模板：

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

这段必须练到肌肉记忆。

顺序不能错：

```text
1. 保存 nxt
2. 改 curr.next
3. prev 前进
4. curr 前进
```

如果先改 `curr.next` 再保存 `nxt`，后面的链表就丢了。

## Fast / Slow Pointer

常见用途：

```text
detect cycle
find middle
remove nth from end
palindrome linked list
```

Cycle detection：

```python
slow = head
fast = head

while fast and fast.next:
    slow = slow.next
    fast = fast.next.next

    if slow == fast:
        return True

return False
```

直觉：

```text
如果有环，快指针一定会追上慢指针。
如果无环，fast 会先到 None。
```

## Merge Two Lists

模板：

```python
dummy = ListNode(0)
tail = dummy

while l1 and l2:
    if l1.val <= l2.val:
        tail.next = l1
        l1 = l1.next
    else:
        tail.next = l2
        l2 = l2.next
    tail = tail.next

tail.next = l1 or l2
return dummy.next
```

训练点：

```text
tail 负责构造新链。
l1/l2 负责遍历旧链。
```

不要混淆。

## Linked List 常见坑

```text
1. 忘记保存 next
    反转时最常见。

2. 返回 head 错
    结构变化后原 head 可能不再是头。

3. 空链表和单节点
    head is None 或 head.next is None。

4. fast.next 访问空指针
    while fast and fast.next。

5. 断链
    reorder / reverse sublist 时要处理边界连接。

6. 值相等和节点相等混淆
    cycle 检测要比较节点对象，不是值。
```

## 三类题的共同训练点

这三类题共同要求：

```text
精确控制状态变化。
```

Stack：

```text
push/pop 的时机。
```

Binary Search：

```text
left/right/mid 的更新。
```

Linked List：

```text
prev/curr/next 的引用。
```

它们都不允许模糊。
一行顺序错，整体就错。

## Agent Harness 视角

这三类题很适合测试 coding agent 的实现纪律。

评估点：

```text
Stack
    是否知道 stack 里存 index 还是 value。
    是否用 while 结算单调栈。

Binary Search
    是否能明确单调性。
    是否能避免 off-by-one 和死循环。

Linked List
    是否能在改指针前保存 next。
    是否能处理 head 被改变的情况。
```

如果一个 agent 经常错在这三类题，说明它的代码执行模型不稳。

## 面试表达模板

Stack：

```text
I use a stack to store unresolved elements.
When a new element gives enough information to resolve the top of the stack,
I pop and update the answer.
This makes each element pushed and popped at most once, so the time is O(n).
```

Binary Search：

```text
The key observation is monotonicity.
Once a candidate value is feasible, all larger values are also feasible.
So we binary search the minimum feasible value and implement a check function.
```

Linked List：

```text
Because the head may change, I use a dummy node.
When reversing or deleting nodes, I always save the next pointer before rewiring links.
This keeps the operation O(n) time and O(1) extra space.
```

## 当前结论

`NEETCODE002` 的核心结论：

```text
Stack 训练延迟结算。
Binary Search 训练单调性和边界。
Linked List 训练指针重连。
```

这三类题不是最“高级”的算法。
但它们最能暴露代码基本功。

如果我们要在 coding interview 和 coding agent harness 里建立可靠性，这三类必须打稳。
