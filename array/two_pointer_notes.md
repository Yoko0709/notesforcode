# Two-Pointer Mastery Notes: Container With Most Water & Trapping Rain Water

---

# ## 1. Container With Most Water

### **Technique**
Two Pointers

---

## **1.1 Intuition**

For two vertical lines at positions `l` and `r`, the water they hold is:

\[
area = (r - l) \times \min(height[l], height[r])
\]

To maximize this area:

- Width decreases as pointers move inward.
- So the only way to increase area is to increase the **min height**.
- Therefore, always move the **shorter** line.

---

## **1.2 Code Implementation**

```python
class Solution(object):
    def maxArea(self, height):
        l, r = 0, len(height) - 1
        ans = 0

        while l < r:
            area = (r - l) * min(height[l], height[r])
            ans = max(ans, area)

            if height[l] < height[r]:
                l += 1
            else:
                r -= 1

        return ans
```

---

## **1.3 Complexity**

| Item | Complexity |
|------|------------|
| Time | **O(n)** |
| Space | **O(1)** |

---

---

# ## 2. Trapping Rain Water

You learned **two optimal solutions**:

- DP with prefix/suffix maxima (O(n) space)
- Two pointers optimized version (O(1) space)

---

# ## 2.1 DP Approach (Prefix Max + Suffix Max)

### **Key Formula**

\[
water[i] = \min(leftMax[i],\ rightMax[i]) - height[i]
\]

---

## **Steps**

1. Build `pre_max[i]` from left to right  
2. Build `surf_max[i]` from right to left  
3. Compute accumulated water using the formula

---

## **DP Code Implementation**

```python
class Solution(object):
    def trap(self, height):
        n = len(height)
        pre_max = [0] * n
        surf_max = [0] * n

        pre_max[0] = height[0]
        for i in range(1, n):
            pre_max[i] = max(pre_max[i-1], height[i])

        surf_max[-1] = height[-1]
        for i in range(n-2, -1, -1):
            surf_max[i] = max(surf_max[i+1], height[i])

        ans = 0
        for h, lmax, rmax in zip(height, pre_max, surf_max):
            ans += min(lmax, rmax) - h

        return ans
```

---

## **Complexity**

| Item | Complexity |
|------|------------|
| Time | **O(n)** |
| Space | **O(n)** |

---

---

# ## 2.2 Two-Pointer Optimized Solution (O(1) Space)

### **Core Idea**

- The side with the **smaller height** determines the possible water level.
- Water on the smaller side can be finalized immediately.
- Move the pointer on the **shorter** side inward.

---

## **Two-Pointer Code Implementation**

```python
class Solution(object):
    def trap(self, height):
        n = len(height)
        l, r = 0, n - 1
        l_max, r_max = 0, 0
        ans = 0

        while l < r:
            if height[l] < height[r]:
                if height[l] >= l_max:
                    l_max = height[l]
                else:
                    ans += l_max - height[l]
                l += 1
            else:
                if height[r] >= r_max:
                    r_max = height[r]
                else:
                    ans += r_max - height[r]
                r -= 1

        return ans
```

---

## **Complexity**

| Item | Complexity |
|------|------------|
| Time | **O(n)** |
| Space | **O(1)** |

---

# **End of Notes**
