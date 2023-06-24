# Median of Two Sorted Arrays

## Description

Given two sorted arrays `nums1` and `nums2` of size `m` and `n` respectively, return **the median** of the two sorted arrays.

The overall run time complexity should be `O(log (m+n))`.

**Example 1:**

```
Input: nums1 = [1,3], nums2 = [2]
Output: 2.00000
Explanation: merged array = [1,2,3] and median is 2.

```

**Example 2:**

```
Input: nums1 = [1,2], nums2 = [3,4]
Output: 2.50000
Explanation: merged array = [1,2,3,4] and median is (2 + 3) / 2 = 2.5.

```

**Constraints:**

-   `nums1.length == m`
-   `nums2.length == n`
-   `0 <= m <= 1000`
-   `0 <= n <= 1000`
-   `1 <= m + n <= 2000`
-   $-10^6 <=$ `nums1[i], nums2[i]` $<= 10^6$

## Solutions 

### Code

### Binary Search
```cpp
class Solution {
    
public:
    double findMedianSortedArrays(vector<int>& nums1, vector<int>& nums2) {
        int n = nums1.size();
        int m = nums2.size();

        // Find the kth smaller, 1-based
        if((n + m) % 2 == 0) {
            return (findKth(nums1, 0, n, nums2, 0, m, (n + m) / 2) + findKth(nums1, 0, n, nums2, 0, m, ((n + m) / 2) + 1)) / 2;
        } else {
            return findKth(nums1, 0, n, nums2, 0, m, ((n + m ) / 2) + 1);
        }
        
    }


    double findKth(vector<int>& nums1, int a, int n, vector<int>& nums2, int b, int m, int k) {

        // Find the kth smaller, 0-based
        if(n > m) 
            return findKth(nums2, b, m, nums1, a, n, k);
        if(n == 0) 
            return nums2[b + k - 1];
        if(k == 1) return min(nums1[a], nums2[b]);

        // since we assume n is always smaller
        int k1 = min(n, k / 2);
        int k2 = k - k1;

        if(nums1[a + k1 - 1] < nums2[b + k2 - 1])
            return findKth(nums1, a + k1, n - k1, nums2, b, m, k - k1);
        else 
            return findKth(nums1, a, n, nums2, b + k2, m - k2, k - k2);

    }

};
```
### Heap
```cpp
class Solution {
    struct comp {
        bool operator()(pair<int, int> p1, pair<int, int> p2) {
            return p1.first > p2.first;
        }
    };
    
public:
    double findMedianSortedArrays(vector<int>& nums1, vector<int>& nums2) {
        int n = nums1.size();
        int m = nums2.size();
        if((n + m) % 2 == 0) {
            return (findKth(nums1, 0, n, nums2, 0, m, (n + m) / 2) + findKth(nums1, 0, n, nums2, 0, m, ((n + m) / 2) + 1)) / 2;
        } else {
            return findKth(nums1, 0, n, nums2, 0, m, ((n + m ) / 2) + 1);
        }
        
    }

    double findKth(vector<int>& nums1, int a, int n, vector<int>& nums2, int b, int m, int k) {
        priority_queue<pair<int, int>, vector<pair<int, int>>, comp> pq;

        int i = 0, j = 0;
        if(nums1.size() != 0) {
            pq.push({nums1[i], 1});
            i++;
        }
        if(nums2.size() != 0) {
            pq.push({nums2[j], 2});
            j++;
        }
        
        int num = 0;
        int count = 0;
        while(!pq.empty() && count < k) {
            int type = pq.top().second;
            num = pq.top().first;
            pq.pop();
            count++;

            if(type == 1 && i < nums1.size()) {
                pq.push({nums1[i], 1});
                i++;
            } else if (type == 2 && j < nums2.size()) {
                pq.push({nums2[j], 2});
                j++;
            }
        }
        return num;
    }

};
```


## Source
- [Median of Two Sorted Arrays - LeetCode](https://leetcode.com/problems/median-of-two-sorted-arrays/)
- [wisdompeak - 4. Median of Two Sorted Arrays](https://github.com/wisdompeak/LeetCode/blob/master/Binary_Search/004.Median-of-Two-Sorted-Arrays/Readme.md)