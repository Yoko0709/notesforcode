# 🧠 Linked List — Fundamentals

> Comprehensive overview of Linked List basics for LeetCode preparation.  
> Covers core concepts, pointer techniques, and common interview patterns.

---

## 1️⃣ What Is a Linked List?

A **linked list** is a linear data structure where each element (called a **node**) stores:
- A **value**
- A **pointer** (or reference) to the **next node**

Unlike arrays, linked lists **do not occupy continuous memory**, so random access is **O(n)**.

### 📘 Node Definition (Python)
```python
class ListNode:
    def __init__(self, val=0, next=None):
        self.val = val
        self.next = next
