# Find First and Last Position of Element in Sorted Array

## Description

Given an array of integers `nums` sorted in non-decreasing order, find the starting and ending position of a given `target` value.

If `target` is not found in the array, return `[-1, -1]`.

You must write an algorithm with `O(log n)` runtime complexity.

**Example 1:**

```
Input: nums = [5,7,7,8,8,10], target = 8
Output: [3,4]

```

**Example 2:**

```
Input: nums = [5,7,7,8,8,10], target = 6
Output: [-1,-1]

```

**Example 3:**

```
Input: nums = [], target = 0
Output: [-1,-1]

```

**Constraints:**

-   `0 <= nums.length <= 10^5`
-   `-10^9 <= nums[i] <= 10^9`
-   `nums` is a non-decreasing array.
-   `-10^9 <= target <= 10^9`

## Solutions 

## Solving Technique

- We can use standard binary search to find the first position of `target` in `nums`, but how can we find the last position?
  - `int mid = (l + r) / 2 + 1`, which makes the mid biased toward the right.
    - Since the mid biased toward the right, use `if(nums[mid] > target)` to update the right pointer.
  - In contrast, `int mid = (l + r) / 2` which makes the mid biased toward the left.
    - Since the mid biased toward the left, use `if(nums[mid] < target)` to update the left pointer
### Code

```cpp
class Solution {
public:
    vector<int> searchRange(vector<int>& nums, int target) {
        int first = -1, last = -1;
        if(nums.size() == 0) {
            return {first, last};
        }
        
        int l = 0, r = nums.size() - 1;
        while(l < r) {
            int mid = (l + r) / 2;
            if(nums[mid] < target) {
                l = mid + 1;
            } else {
                r = mid;
            }
        }

        if(nums[l] != target) 
            return {first, last};
        
        first = l;

        l = 0, r = nums.size() - 1;
        while(l < r) {
            int mid = (l + r) / 2 + 1;
            if(nums[mid] > target) {
                r = mid - 1;
            } else {
                l = mid;
            }
        }
        last = l;

        return {first, last};
    }
};
```

## Source
- [Find First and Last Position of Element in Sorted Array - LeetCode](https://leetcode.com/problems/find-first-and-last-position-of-element-in-sorted-array/description/)