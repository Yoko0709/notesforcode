# 🔁 Linked List Reversal — Complete Notes

> This document summarizes all key concepts and problems related to **Linked List Reversal**.  
> Covers both **full reversal (LeetCode 206)** and **partial reversal (LeetCode 92)**.  
> Mastering these two ensures you can handle almost any pointer manipulation in linked lists.

---

## 🧩 0. Understanding Linked List Reversal

Reversing a linked list means changing the direction of every `next` pointer  
so that the list’s order is reversed.

### Example

```
Original: 1 → 2 → 3 → 4 → 5 → None  
Reversed: 5 → 4 → 3 → 2 → 1 → None
```

### Core Idea

Each node must point **backward** instead of forward.

---

## ⚙️ General Reversal Pattern

We use **three pointers**:

| Pointer | Meaning |
|----------|----------|
| `prev` | already reversed portion |
| `cur` | current node being processed |
| `nxt` | temporarily stores `cur.next` before it’s broken |

### Core Template

```python
prev, cur = None, head
while cur:
    nxt = cur.next      # store next
    cur.next = prev     # reverse link
    prev = cur          # move prev
    cur = nxt           # move cur
return prev
```

This is the universal skeleton used in both full and partial reversals.

---

## 🧠 Problem 1: LeetCode 206 — Reverse Linked List (Easy)

### 📘 Problem Statement

Given the head of a singly linked list, reverse the list and return its new head.

**Example**

```text
Input:  head = [1,2,3,4,5]
Output: [5,4,3,2,1]
```

---

### 💡 Approach — Iterative (Three Pointers)

1. Initialize `prev = None` and `cur = head`.
2. While `cur` is not `None`:
   - Save `cur.next` in `nxt`
   - Reverse the pointer: `cur.next = prev`
   - Move forward: `prev = cur; cur = nxt`
3. Return `prev` (new head)

---

### 🧠 Visualization

```
Initial:
None <- 1 -> 2 -> 3 -> 4 -> 5
 ^      ^
prev   cur

After 1st iteration:
None <- 1    2 -> 3 -> 4 -> 5
        ^     ^
      prev   cur

After 2nd iteration:
None <- 1 <- 2    3 -> 4 -> 5
             ^     ^
           prev   cur

Final:
None <- 1 <- 2 <- 3 <- 4 <- 5
                          ^
                         prev (new head)
```

---

### 💻 Code (Python)

```python
class Solution:
    def reverseList(self, head):
        prev, cur = None, head
        while cur:
            nxt = cur.next
            cur.next = prev
            prev = cur
            cur = nxt
        return prev
```

---

### 🧩 Recursive Version (Elegant but less efficient)

```python
class Solution:
    def reverseList(self, head):
        if not head or not head.next:
            return head
        new_head = self.reverseList(head.next)
        head.next.next = head
        head.next = None
        return new_head
```

---

### ⏱️ Complexity

| Type | Complexity |
|------|-------------|
| Time | O(n) |
| Space | O(1) for iterative, O(n) for recursion (stack depth) |

---

### ⚠️ Common Mistakes

- ❌ Forget to store `cur.next` before overwriting it  
- ❌ Return `head` instead of `prev`  
- ❌ Infinite loop: forgetting to break the last link  
- ❌ Confuse order of pointer movement  

---

### ✅ Key Takeaways

- Understand pointer flow (`prev → cur → nxt`)  
- Once mastered, you can reverse any part of a linked list.  
- This logic is reused in **LeetCode 92, 24, and 25**.

---

## 🧩 Problem 2: LeetCode 92 — Reverse Linked List II (Medium)

### 📘 Problem Statement

Given the head of a singly linked list and two integers `left` and `right`,  
where `1 ≤ left ≤ right ≤ n`, reverse the nodes of the list from position  
`left` to `right`, and return the modified list.

**Example**

```text
Input:  head = [1,2,3,4,5], left = 2, right = 4
Output: [1,4,3,2,5]
```

---

### 💡 Approach — Dummy Node + Partial Reversal

This is an extension of Problem 206.  
Instead of reversing the entire list, we only reverse the segment `[left, right]`.

---

### 🧩 Key Idea

We split the list into three parts:

1. **Left Segment** — nodes before `left`
2. **Middle Segment** — nodes between `[left, right]` (to reverse)
3. **Right Segment** — nodes after `right`

We'll use a **dummy node** to simplify the edge case when `left = 1`.

---

### 🧠 Step-by-Step Plan

#### Step 1: Create dummy node

```python
dummy = ListNode(-1, head)
pre = dummy
```

#### Step 2: Move `pre` to the node before position `left`

```python
for _ in range(left - 1):
    pre = pre.next
```

#### Step 3: Reverse the sublist `[left, right]`

```python
cur = pre.next
prev = None
for _ in range(right - left + 1):
    nxt = cur.next
    cur.next = prev
    prev = cur
    cur = nxt
```

#### Step 4: Reconnect

```python
pre.next.next = cur   # connect tail of reversed section to right part
pre.next = prev       # connect left part to new head
```

---

### 💻 Code (Python)

```python
class Solution:
    def reverseBetween(self, head, left, right):
        dummy = ListNode(-1, head)
        pre = dummy

        # Step 1: Move pre to node before 'left'
        for _ in range(left - 1):
            pre = pre.next

        # Step 2: Reverse [left, right]
        cur = pre.next
        prev = None
        for _ in range(right - left + 1):
            nxt = cur.next
            cur.next = prev
            prev = cur
            cur = nxt

        # Step 3: Reconnect
        pre.next.next = cur
        pre.next = prev

        return dummy.next
```

---

### 🧠 Visualization

**Input**

```
1 → 2 → 3 → 4 → 5, left=2, right=4
```

**Process**

```
pre at 1
reverse [2,3,4] → [4,3,2]
connect:
1 → 4 → 3 → 2 → 5
```

**Output**

```
[1,4,3,2,5]
```

---

### 🧪 Example Tests

| Input | Output | Explanation |
|--------|---------|-------------|
| [1,2,3,4,5], left=2, right=4 | [1,4,3,2,5] | middle segment reversed |
| [1,2,3,4,5], left=1, right=5 | [5,4,3,2,1] | full reversal |
| [1,2,3,4,5], left=3, right=3 | [1,2,3,4,5] | no change |
| [1,2], left=1, right=2 | [2,1] | short list |

---

### ⏱️ Complexity

| Type | Complexity |
|------|-------------|
| Time | O(n) |
| Space | O(1) |

---

### ⚠️ Common Mistakes

- ❌ Forget to reattach: `pre.next.next = cur`  
- ❌ Move `pre` wrong number of steps (`left - 1`)  
- ❌ Modify head incorrectly when `left == 1`  
- ❌ Overwrite pointers too early (lose reference)  

---

### 🧠 Key Takeaways

- Dummy node makes handling `left = 1` easy  
- Reuse full-reversal logic on a limited range  
- Ensure both connections are restored at the end:  
  - Left connection: `pre.next = prev`  
  - Right connection: `tail.next = cur`

---

## 📊 Summary Table

| Problem | Description | Technique | Difficulty |
|----------|--------------|------------|-------------|
| 206 | Reverse the entire list | Three pointers (`prev-cur-nxt`) | 🟢 Easy |
| 92 | Reverse a sublist | Dummy node + partial reverse | 🟡 Medium |

---

## 💬 Final Notes

- Drawing the list on paper while coding helps visualize pointer movement.  
- The **dummy node pattern** is essential for avoiding edge-case chaos.  
- Understanding 206 + 92 prepares you for advanced problems:
  - 24. Swap Nodes in Pairs  
  - Reverse Nodes in k-Group  
  - Palindrome Linked List  

**Key mental model:** “cut → reverse → reconnect”.
