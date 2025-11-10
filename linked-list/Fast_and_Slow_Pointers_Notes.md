# 🐢🐇 Fast & Slow Pointers — Complete Notes

> This document summarizes all key concepts and problems related to **Fast & Slow Pointers**.  
> Covers **middle node detection**, **cycle detection**, **cycle entry location**, and **list reordering**.  
> Mastering these four ensures you can handle most linked list structural manipulation problems.

---

## 🧩 0. Understanding the Fast–Slow Pointer Pattern

Two pointers traverse the linked list at different speeds:
- **slow** moves one step at a time  
- **fast** moves two steps at a time  

The difference in their speeds reveals structural information:
- When `fast` reaches the end → `slow` is at the **middle**  
- When `fast` catches up with `slow` → there is a **cycle**

---

## ⚙️ General Pattern Template

```python
slow, fast = head, head
while fast and fast.next:
    slow = slow.next
    fast = fast.next.next
```

---

## 🧠 Problem 1: LeetCode 876 — Middle of the Linked List (Easy)

### 📘 Problem Statement

Return the **middle node** of a linked list.  
If there are two middle nodes, return the **second one**.

---

### 💡 Approach — Fast & Slow Pointer

`slow` moves one step, `fast` moves two steps.  
When `fast` reaches the end, `slow` is in the middle.

---

### 💻 Code (Python)

```python
class Solution(object):
    def middleNode(self, head):
        slow = fast = head
        while fast and fast.next:
            slow = slow.next
            fast = fast.next.next
        return slow
```

---

### 🧠 Visualization

```
Input: 1 → 2 → 3 → 4 → 5 → 6
Steps:
slow: 1→2→3→4
fast: 1→3→5→None
Result: slow = 4 (second middle)
```

---

### ⏱️ Complexity

| Type | Complexity |
|------|-------------|
| Time | O(n) |
| Space | O(1) |

---

### ✅ Key Takeaways

- `fast` 两步、`slow` 一步 → 终点时 `slow` 正在中间。  
- 用于链表分半、归并排序、回文检查等。

---

## 🧠 Problem 2: LeetCode 141 — Linked List Cycle (Easy)

### 📘 Problem Statement

Determine whether a linked list contains a cycle.

---

### 💡 Approach — Floyd’s Tortoise and Hare Algorithm

Use fast and slow pointers:
- If `fast` catches up with `slow`, a cycle exists.  
- If `fast` reaches `None`, there is no cycle.

---

### 💻 Code (Python)

```python
class Solution(object):
    def hasCycle(self, head):
        fast = slow = head
        while fast and fast.next:
            slow = slow.next
            fast = fast.next.next
            if fast is slow:
                return True
        return False
```

---

### 🧠 Visualization

```
A → B → C → D → E
        ↑       ↓
        ←───────
slow: A→B→C→D→E→C
fast: A→C→E→D→C  (meet at C)
```

---

### ⏱️ Complexity

| Type | Complexity |
|------|-------------|
| Time | O(n) |
| Space | O(1) |

---

### ✅ Key Takeaways

- 若 `fast` 与 `slow` 相遇 → 有环。  
- 若 `fast` 或 `fast.next` 为 None → 无环。  
- 这是后续“找环入口”的基础阶段。

---

## 🧠 Problem 3: LeetCode 142 — Linked List Cycle II (Medium)

### 📘 Problem Statement

If there is a cycle, return the **node where the cycle begins**.  
If no cycle, return `None`.

---

### 💡 Approach — Floyd’s Algorithm (Two Phases)

1️⃣ Detect cycle using fast–slow pointer.  
2️⃣ When they meet, reset one pointer to `head`.  
   Move both one step at a time; when they meet again → **cycle start**.

---

### 💻 Code (Python)

```python
class Solution(object):
    def detectCycle(self, head):
        fast = slow = head
        # Phase 1: detect cycle
        while fast and fast.next:
            slow = slow.next
            fast = fast.next.next
            if fast is slow:
                break
        else:
            return None  # no cycle

        # Phase 2: find entry
        while head is not slow:
            head = head.next
            slow = slow.next
        return slow
```

---

### 🧩 Why It Works (Mathematical Insight)

Let:
- `a` = distance from head to cycle start  
- `b` = distance from cycle start to meeting point  
- `c` = remaining distance to complete the cycle  

At meeting:
```
fast_distance = 2 * slow_distance
=> a + b + n(b + c) = 2(a + b)
=> a = n(b + c) - b = (n - 1)(b + c) + c
```

Interpretation:
> If one pointer starts from `head` and another from the meeting point,  
> moving both one step at a time, they will meet at the cycle start (after `a = c` steps).

---

### 🧠 Visualization

```
head → a1 → a2 → (入口) → b1 → b2 → b3 → back to 入口
                   ↑               ↓
                  相遇点 ←───────────
```

---

### ⏱️ Complexity

| Type | Complexity |
|------|-------------|
| Time | O(n) |
| Space | O(1) |

---

### ✅ Key Takeaways

- “第一次相遇 → 有环；第二次相遇 → 环入口”。  
- 从 `head` 与相遇点同时出发，每次一步 → 入口相遇。  

---

## 🧠 Problem 4: LeetCode 143 — Reorder List (Medium)

### 📘 Problem Statement

Reorder the linked list as:
```
L0 → L1 → … → Ln-1 → Ln
↓
L0 → Ln → L1 → Ln-1 → L2 → Ln-2 → …
```
Modify **in place**.

---

### 💡 Approach — 3 Steps Template

1️⃣ Find middle node (fast–slow pointer).  
2️⃣ Reverse the second half.  
3️⃣ Merge two halves alternately.

---

### 💻 Code (Python)

```python
class Solution(object):
    def middleNode(self, head):
        fast = slow = head
        while fast and fast.next:
            slow = slow.next
            fast = fast.next.next
        return slow

    def reverseList(self, head):
        pre, cur = None, head
        while cur:
            nxt = cur.next
            cur.next = pre
            pre = cur
            cur = nxt
        return pre

    def reorderList(self, head):
        if not head or not head.next:
            return
        # 1️⃣ find mid
        mid = self.middleNode(head)
        # 2️⃣ reverse right half
        head2 = self.reverseList(mid.next)
        mid.next = None
        # 3️⃣ merge
        p1, p2 = head, head2
        while p2:
            n1, n2 = p1.next, p2.next
            p1.next = p2
            p2.next = n1
            p1, p2 = n1, n2
```

---

### 🧠 Visualization

Input:
```
1 → 2 → 3 → 4 → 5
```

Step-by-step:
```
Find mid: 3
Reverse right half: 5 → 4
Merge: 1 → 5 → 2 → 4 → 3
```

Output:
```
1 → 5 → 2 → 4 → 3
```

---

### ⏱️ Complexity

| Type | Complexity |
|------|-------------|
| Time | O(n) |
| Space | O(1) |

---

### ✅ Key Takeaways

- 核心框架：**找中点 → 反转 → 合并**  
- 一定要记得 `mid.next = None` 否则链表会成环。  
- 本题完美融合了前面三题的技巧。  

---

## 📊 Summary Table

| Problem | Description | Key Technique | Difficulty |
|----------|--------------|----------------|-------------|
| 876 | Middle of Linked List | Fast–Slow Pointer | 🟢 Easy |
| 141 | Linked List Cycle | Detect meeting | 🟢 Easy |
| 142 | Linked List Cycle II | Find cycle entry | 🟡 Medium |
| 143 | Reorder List | Split → Reverse → Merge | 🟡 Medium |

---

## 💬 Final Notes

- The **fast & slow pointer** technique underlies many linked list problems.  
- Once you master how `fast` laps `slow`, you can predict meeting and midpoints.  
- 876, 141, 142, and 143 form the **core pointer set** for mastering linked list operations.

**Key mental model:**  
> “快慢找结构 → 反转变形 → 合并重构”
