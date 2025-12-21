
# Sliding Window Mastery Notes

---

## 0. Sliding Window Core Principles

### When to Use Sliding Window
- Input is an array / string
- Subarray / substring must be continuous
- Asked for longest / shortest / count under a constraint
- Brute force is O(n^2)

### Two Fundamental Types

| Type | Window Behavior | Typical Question |
|---|---|---|
| Fixed Window | Window size = k | Max / average of length k |
| Variable Window | Window size changes | Longest / shortest / counting |

### Key Monotonicity Insight
Sliding window works because state changes monotonically when pointers move.

---

## 1. Minimum Size Subarray Sum (LeetCode 209)

### Idea
All numbers are positive.
- Expand right to increase sum
- Shrink left while sum >= target
- Track minimum length

```python
class Solution(object):
    def minSubArrayLen(self, target, nums):
        left = 0
        s = 0
        ans = len(nums) + 1

        for right, x in enumerate(nums):
            s += x
            while s >= target:
                ans = min(ans, right - left + 1)
                s -= nums[left]
                left += 1

        return ans if ans <= len(nums) else 0
```

Time: O(n)  
Space: O(1)

---

## 2. Longest Substring Without Repeating Characters (LeetCode 3)

```python
from collections import Counter

class Solution(object):
    def lengthOfLongestSubstring(self, s):
        cnt = Counter()
        left = 0
        ans = 0

        for right, c in enumerate(s):
            cnt[c] += 1
            while cnt[c] > 1:
                cnt[s[left]] -= 1
                left += 1
            ans = max(ans, right - left + 1)

        return ans
```

Time: O(n)  
Space: O(n)

---

## 3. Subarray Product Less Than K (LeetCode 713)

```python
class Solution(object):
    def numSubarrayProductLessThanK(self, nums, k):
        if k <= 1:
            return 0

        left = 0
        prod = 1
        ans = 0

        for right, x in enumerate(nums):
            prod *= x
            while prod >= k:
                prod //= nums[left]
                left += 1
            ans += right - left + 1

        return ans
```

Time: O(n)  
Space: O(1)

---

## 4. Max Consecutive Ones III (LeetCode 1004)

```python
class Solution(object):
    def longestOnes(self, nums, k):
        left = 0
        zeros = 0
        ans = 0

        for right, x in enumerate(nums):
            if x == 0:
                zeros += 1

            while zeros > k:
                if nums[left] == 0:
                    zeros -= 1
                left += 1

            ans = max(ans, right - left + 1)

        return ans
```

Time: O(n)  
Space: O(1)

---

## 5. Longest Semi-Repetitive Substring (LeetCode 2730)

```python
class Solution(object):
    def longestSemiRepetitiveSubstring(self, s):
        left = 0
        pairs = 0
        ans = 1

        for right in range(1, len(s)):
            if s[right] == s[right - 1]:
                pairs += 1

            while pairs > 1:
                if s[left] == s[left + 1]:
                    pairs -= 1
                left += 1

            ans = max(ans, right - left + 1)

        return ans
```

Time: O(n)  
Space: O(1)

---

## 6. Count Subarrays Where Max Appears ≥ K Times (LeetCode 2962)

```python
class Solution(object):
    def countSubarrays(self, nums, k):
        mx = max(nums)
        left = 0
        times = 0
        ans = 0

        for right, x in enumerate(nums):
            if x == mx:
                times += 1

            while times == k:
                if nums[left] == mx:
                    times -= 1
                left += 1

            ans += left

        return ans
```

Time: O(n)  
Space: O(1)

---

## Sliding Window Answer Update Patterns

| Problem Type | Update Rule |
|---|---|
| Longest / Maximum | ans = max(ans, window_len) |
| Shortest / Minimum | update ans inside while |
| Counting | ans += (right-left+1) or ans += left |
