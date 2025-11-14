# 🐢🐇 Linked List — Today’s Practice Summary (Problems 19, 2095, 328)

This document summarizes all linked list problems you solved today:  
**19. Remove Nth Node From End**  
**2095. Delete the Middle Node**  
**328. Odd Even Linked List**  
plus pointer logic explanation.

---

# 🧩 Problem 19 — Remove Nth Node from End of List

## 💡 Insight — Fixed Distance Double Pointers

Technique:
1. Use a dummy node to simplify removing the head.
2. Move `fast` pointer `n+1` steps.
3. Move `fast` and `slow` together until `fast` hits None.
4. `slow.next` is the node to delete.

---

## 💻 Code

```python
class Solution(object):
    def removeNthFromEnd(self, head, n):
        dummy = ListNode(0, head)
        fast = slow = dummy

        for _ in range(n + 1):
            if fast:
                fast = fast.next

        while fast:
            fast = fast.next
            slow = slow.next

        slow.next = slow.next.next
        return dummy.next
```

---

# 🧩 Problem 2095 — Delete the Middle Node of a Linked List

## 💡 Insight — Fast–Slow to Locate Midpoint

- `fast` moves 2 steps each time  
- `slow` moves 1 step  
- When `fast` reaches the end, `slow.next` is the middle node

---

## 💻 Code

```python
class Solution(object):
    def deleteMiddle(self, head):
        if not head:
            return None

        dummy = ListNode(0, head)
        fast = slow = dummy

        while fast.next and fast.next.next:
            fast = fast.next.next
            slow = slow.next

        slow.next = slow.next.next
        return dummy.next
```

---

# 🧩 Problem 328 — Odd Even Linked List

## 💡 Insight — Build Two Chains, Then Stitch

Split chain into:
- `odd`  → 1, 3, 5, …
- `even` → 2, 4, 6, …

Finally:  
`odd_tail.next = even_head`

---

## 💻 Code

```python
class Solution(object):
    def oddEvenList(self, head):
        if not head or not head.next:
            return head

        odd = head
        even = head.next
        even_head = even

        while even and even.next:
            odd.next = even.next
            odd = odd.next

            even.next = odd.next
            even = even.next

        odd.next = even_head
        return head
```

---

# 📘 Summary Table

| Problem | Technique | Key Idea | Complexity |
|--------|-----------|----------|------------|
| 19 | 快慢指针固定距离 | fast 先走 n+1，再一起走 | O(n), O(1) |
| 2095 | 快慢指针找中点 | slow 停在中点前一个 | O(n), O(1) |
| 328 | 两链表拆解重组 | odd 链 + even 链 | O(n), O(1) |
