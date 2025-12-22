# Binary Search & Two Pointers Mastery Notes

---

## 0. Core Thinking Patterns

### 0.1 Binary Search on Answer Space
Binary search is not just for arrays — it is for **answer spaces with monotonicity**.

Common goals:
- Find the minimum feasible value
- Find the maximum feasible value
- Count elements in a valid interval

---

### 0.2 Three Typical Binary Search Tasks

| Task | Monotonic Pattern | Template |
|---|---|---|
| Min feasible | False → True | right = mid |
| Max feasible | True → False | left = mid (right-biased mid) |
| Interval count | Sorted array | lb(R+1) - lb(L) |

---

### 0.3 Floor / Ceil Cheat Sheet

| Meaning | Formula |
|---|---|
| Floor | a // b |
| Ceil | (a + b - 1) // b |
| x < ceil(a/b) | x <= (a-1)//b |

---

## 1. Lower Bound Template

```python
def lower_bound(nums, target, lo=0):
    left, right = lo, len(nums)
    while left < right:
        mid = (left + right) // 2
        if nums[mid] < target:
            left = mid + 1
        else:
            right = mid
    return left
```

### Interval Count Formula
```text
count in [L, R] = lb(R+1) - lb(L)
```

---

## 2. Range Frequency Query (LeetCode 2080)

```python
from bisect import bisect_left, bisect_right
from collections import defaultdict

class RangeFreqQuery(object):
    def __init__(self, arr):
        self.mp = defaultdict(list)
        for i, x in enumerate(arr):
            self.mp[x].append(i)

    def query(self, left, right, value):
        pos = self.mp.get(value, [])
        return bisect_right(pos, right) - bisect_left(pos, left)
```

---

## 3. Successful Pairs of Spells and Potions

```python
class Solution(object):
    def successfulPairs(self, spells, potions, success):
        potions.sort()
        n = len(potions)
        ans = []
        for x in spells:
            need = (success + x - 1) // x
            idx = lower_bound(potions, need)
            ans.append(n - idx)
        return ans
```

---

## 4. Find the Distance Value Between Two Arrays

```python
from bisect import bisect_left

class Solution(object):
    def findTheDistanceValue(self, arr1, arr2, d):
        arr2.sort()
        ans = 0
        for x in arr1:
            i = bisect_left(arr2, x - d)
            if i == len(arr2) or arr2[i] > x + d:
                ans += 1
        return ans
```

---

## 5. Count Fair Pairs (LeetCode 2563)

### Binary Search Version
```python
class Solution(object):
    def countFairPairs(self, nums, lower, upper):
        nums.sort()
        ans = 0
        n = len(nums)
        for i in range(n):
            L = lower - nums[i]
            R = upper - nums[i]
            ans += lower_bound(nums, R + 1, i + 1) - lower_bound(nums, L, i + 1)
        return ans
```

### Two Pointers Version
```python
class Solution(object):
    def countFairPairs(self, nums, lower, upper):
        nums.sort()
        def count_leq(S):
            l, r = 0, len(nums) - 1
            cnt = 0
            while l < r:
                if nums[l] + nums[r] <= S:
                    cnt += r - l
                    l += 1
                else:
                    r -= 1
            return cnt
        return count_leq(upper) - count_leq(lower - 1)
```

---

## 6. Koko Eating Bananas (LeetCode 875)

```python
class Solution(object):
    def minEatingSpeed(self, piles, h):
        left, right = 1, max(piles)
        def can(k):
            return sum((p + k - 1) // k for p in piles) <= h
        while left < right:
            mid = (left + right) // 2
            if can(mid):
                right = mid
            else:
                left = mid + 1
        return left
```

---

## 7. Minimum Time to Complete Trips (LeetCode 2187)

```python
class Solution(object):
    def minimumTime(self, time, totalTrips):
        left, right = 1, min(time) * totalTrips
        def can(T):
            trips = 0
            for t in time:
                trips += T // t
                if trips >= totalTrips:
                    return True
            return False
        while left < right:
            mid = (left + right) // 2
            if can(mid):
                right = mid
            else:
                left = mid + 1
        return left
```

---

## 8. H-Index (Binary Search)

```python
class Solution(object):
    def hIndex(self, citations):
        n = len(citations)
        def can(H):
            return sum(c >= H for c in citations) >= H
        left, right = 1, n + 1
        while left < right:
            mid = (left + right) // 2
            if can(mid):
                left = mid + 1
            else:
                right = mid
        return left - 1
```

---

## 9. Binary Search Templates

### Min True
```python
while l < r:
    mid = (l + r) // 2
    if can(mid):
        r = mid
    else:
        l = mid + 1
```

### Max True
```python
while l < r:
    mid = (l + r + 1) // 2
    if can(mid):
        l = mid
    else:
        r = mid - 1
```

---

## 10. Core Takeaways

- Binary search is about monotonic answers, not indices
- Always define can(x) clearly
- Interval problems reduce to prefix subtraction
- Master lower_bound and upper_bound patterns
