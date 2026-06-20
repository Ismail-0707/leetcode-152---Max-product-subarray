# LeetCode 152 - Maximum Product Subarray

## Problem Statement

Given an integer array `nums`, find a contiguous non-empty subarray within the array that has the largest product and return that product.

---

## Optimal Approach

### Intuition

For Maximum Sum Subarray (Kadane's Algorithm), we only track the maximum sum.

For Maximum Product Subarray, negative numbers make things tricky:

- A large positive product multiplied by a negative number becomes negative.
- A large negative product multiplied by a negative number becomes positive.

Therefore, at every index we must maintain:

1. `maxProd` = Maximum product ending at current index.
2. `minProd` = Minimum product ending at current index.

When a negative number appears, `maxProd` and `minProd` swap roles.

---

## Algorithm

1. Initialize:
   - `maxProd = nums[0]`
   - `minProd = nums[0]`
   - `ans = nums[0]`

2. Traverse the array from index `1`.

3. If the current number is negative:
   - Swap `maxProd` and `minProd`.

4. Update:
   - `maxProd = max(nums[i], maxProd * nums[i])`
   - `minProd = min(nums[i], minProd * nums[i])`

5. Update the answer:
   - `ans = max(ans, maxProd)`

6. Return `ans`.

---

## Java Code

```java
class Solution {
    public int maxProduct(int[] nums) {

        int maxProd = nums[0];
        int minProd = nums[0];
        int ans = nums[0];

        for (int i = 1; i < nums.length; i++) {

            if (nums[i] < 0) {
                int temp = maxProd;
                maxProd = minProd;
                minProd = temp;
            }

            maxProd = Math.max(nums[i], maxProd * nums[i]);
            minProd = Math.min(nums[i], minProd * nums[i]);

            ans = Math.max(ans, maxProd);
        }

        return ans;
    }
}
```

---

## Dry Run

### Input

```text
nums = [2,3,-2,4]
```

| i | nums[i] | maxProd | minProd | ans |
|---|---------|----------|----------|-----|
| 0 | 2 | 2 | 2 | 2 |
| 1 | 3 | 6 | 3 | 6 |
| 2 | -2 | -2 | -12 | 6 |
| 3 | 4 | 4 | -48 | 6 |

### Output

```text
6
```

---

## Example 2

### Input

```text
nums = [-2,3,-4]
```

### Dry Run

```text
Start:
maxProd = -2
minProd = -2
ans = -2

3:
maxProd = 3
minProd = -6
ans = 3

-4:
Swap maxProd and minProd

maxProd = 24
minProd = -12
ans = 24
```

### Output

```text
24
```

---

## Complexity Analysis

### Time Complexity

```text
O(n)
```

Single traversal of the array.

### Space Complexity

```text
O(1)
```

Only a few variables are used.

---

## Key Observation

```text
Positive × Positive = Positive
Negative × Negative = Positive
Positive × Negative = Negative
```

Because a negative number can convert the smallest product into the largest product, we track both maximum and minimum products at every index.

---

## Pattern

```text
Dynamic Programming (Kadane's Variation)
```
