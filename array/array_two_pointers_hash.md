# Array Two-Pointers & Hash Table Notes (English Version)

---

## 1. Two Sum (Complete Summary)

### 1.1 Problem Overview
Given an integer array `nums` and an integer `target`, return **indices of the two numbers such that they add up to target**.

Three classic approaches exist:

1. **Brute Force → O(n²)**
2. **Hash Table (best for unsorted arrays) → O(n)**
3. **Two Pointers (requires sorted array) → O(n)**

---

## 1.2 Unsorted Array: Hash Table Solution (O(n))

### 🔥 Core Idea  
While scanning the array, store **numbers already seen** into a hash table (`dict`).  
For each number `num`, compute its complement:

```
complement = target - num
```

If the complement has appeared earlier, we immediately have the answer.

### ✔ Template Code (RECOMMENDED TO MEMORIZE)

```python
class Solution(object):
    def twoSum(self, nums, target):
        hashmap = {}  # value -> index
        for i, num in enumerate(nums):
            complement = target - num
            if complement in hashmap:
                return [hashmap[complement], i]
            hashmap[num] = i
```

### ✔ Key Notes
- Must **check complement first** and **store num later**.
- If reversed, `[3,3]` with target `6` will incorrectly match the same index twice.
- Hash table gives O(1) average lookup.

---

## 1.3 Sorted Array: Two-Pointer Solution (O(n))

Ideal when the input array is already sorted.

### ✔ Template Code

```python
class Solution(object):
    def twoSum(self, numbers, target):
        left, right = 0, len(numbers) - 1
        while left < right:
            s = numbers[left] + numbers[right]
            if s == target:
                return [left + 1, right + 1]  # 1-indexed result
            elif s < target:
                left += 1
            else:
                right -= 1
```

### Intuition  
- Sum too small → move left pointer to increase sum  
- Sum too large → move right pointer to decrease sum  
- Sum equal → return solution

---

## 2. Three Sum (3Sum)

### 2.1 Strategy Overview
> Sort the array → Fix one number `i` → Use two pointers (`j`, `k`) to find pairs that sum up to `-nums[i]`.

### ✔ Full Template (with pruning & deduplication)

```python
class Solution(object):
    def threeSum(self, nums):
        nums.sort()
        ans = []
        n = len(nums)

        for i in range(n - 2):
            # 1. Skip duplicates for i
            if i > 0 and nums[i] == nums[i - 1]:
                continue

            # 2. Pruning: if the current smallest possible sum is too small
            if nums[i] + nums[-1] + nums[-2] < 0:
                continue

            # 3. Pruning: nums[i] > 0 means all future sums will be > 0
            if nums[i] > 0:
                break

            # 4. Pruning: minimal possible sum > 0 means break
            if nums[i] + nums[i + 1] + nums[i + 2] > 0:
                break

            j, k = i + 1, n - 1

            while j < k:
                s = nums[i] + nums[j] + nums[k]
                if s < 0:
                    j += 1
                elif s > 0:
                    k -= 1
                else:
                    ans.append([nums[i], nums[j], nums[k]])
                    j += 1
                    k -= 1

                    # Skip duplicates on j
                    while j < k and nums[j] == nums[j - 1]:
                        j += 1
                    # Skip duplicates on k
                    while j < k and nums[k] == nums[k + 1]:
                        k -= 1

        return ans
```

### ✔ Things to Remember
- Outer loop deduplicates `i`.
- Inner loop deduplicates `j` and `k`.
- Use `continue` to skip invalid `i`.
- Use `break` to stop once remaining elements cannot form zero.

---

## 3. Move Zeroes (fast/slow Two-Pointer Pattern)

### 3.1 Problem  
Move all zeros to the end **while keeping the order of non-zero elements**.

Example:  
`[0,1,0,3,12] → [1,3,12,0,0]`

### ✔ Optimal Two-Pointer Solution (in-place, O(n))

```python
class Solution(object):
    def moveZeroes(self, nums):
        slow = 0
        for fast in range(len(nums)):
            if nums[fast] != 0:
                nums[slow], nums[fast] = nums[fast], nums[slow]
                slow += 1
```

### Meaning  
- `fast`: scans the array  
- `slow`: next position to place a non-zero element  
- Swap when `nums[fast] != 0`

This keeps the non-zero elements in order, pushing zeros to the end.

---

## 4. Important Concepts

### 4.1 Hash Table (dict / set)

Hash Table gives:
- **O(1)** average lookup
- **O(1)** insertion
- Key for solving Two Sum efficiently

Mnemonic:
> Store *actual numbers* → look for *their complements*.  
> Never store complements directly.

---

### 4.2 enumerate(nums)

Provides both index and value:

```python
for i, num in enumerate(nums):
    print(i, num)
```

Output:
```
0 num0
1 num1
2 num2
```

Used in Two Sum to track indices.

---

### 4.3 break vs continue

| Keyword   | Behavior | Meaning |
|-----------|----------|---------|
| `break`   | stops entire loop | Used in 3Sum pruning |
| `continue`| skips current iteration | Used to skip duplicates |

---

### 4.4 Why Linked List Cannot Swap Like Arrays?

Arrays allow:

```python
nums[i], nums[j] = nums[j], nums[i]
```

Because:
- Contiguous memory  
- Random access by index  

Linked lists do **not** support random access.  
To “swap nodes,” you must modify pointers, **not values**, which is complex.

---

## 5. Mindset Summary (Important)

1. **Hash Table** → Best for unsorted Two Sum  
2. **Two Pointers** → Best for sorted arrays  
3. **3Sum** → Sort + fix i + two pointers  
4. **Move Zeroes** → fast/slow pointer compression  
5. **Pruning + Deduplication** → must master for 3Sum  
6. **break vs continue** → control flow essentials

---

End of English Version Notes.
