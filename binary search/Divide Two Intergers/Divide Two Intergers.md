# Divide Two Integers

## Description

Given two integers `dividend` and `divisor`, divide two integers **without** using multiplication, division, and mod operator.

The integer division should truncate toward zero, which means losing its fractional part. For example, `8.345` would be truncated to `8`, and `-2.7335` would be truncated to `-2`.

Return _the **quotient** after dividing_ `dividend` _by_ `divisor`.

**Note:** Assume we are dealing with an environment that could only store integers within the **32-bit** signed integer range: `[−2<sup>31</sup>, 2<sup>31</sup> − 1]`. For this problem, if the quotient is **strictly greater than** `2<sup>31</sup> - 1`, then return `2<sup>31</sup> - 1`, and if the quotient is **strictly less than** `-2<sup>31</sup>`, then return `-2<sup>31</sup>`.

**Example 1:**

```
Input: dividend = 10, divisor = 3
Output: 3
Explanation: 10/3 = 3.33333.. which is truncated to 3.

```

**Example 2:**

```
Input: dividend = 7, divisor = -3
Output: -2
Explanation: 7/-3 = -2.33333.. which is truncated to -2.

```

**Constraints:**

-   $-2^31 <=$ `dividend, divisor` $<= 2^31 - 1$
-   `divisor != 0`

## Solutions 

### Code

```cpp
class Solution {
public:
    int divide(int dividend, int divisor) {
        if(divisor == 0) return INT_MAX;
        if(dividend == 0) return 0;

        // prevent abs() from overflow
        // ex: abs(INT_MIN) will overflow
        long long a = abs((long long) dividend);
        long long b = abs((long long) divisor);
        long long c;
        long long quotient = 0;

        int sign = 1;
        if(dividend < 0) sign *= -1;
        if(divisor < 0) sign *= -1;


        while(b <= a) {
            c = b;
            long long count = 1;
            while((c << 1) <= a) {
                c = c << 1;
                count = count << 1;
            }

            quotient += count;
            a = a - c;
        }

        quotient = quotient * sign;

        if(quotient > INT_MAX)
            return INT_MAX;
        
        return quotient;
    }
};
```

## Source
- [Divide Two Integers - LeetCode](https://leetcode.com/problems/divide-two-integers/)
- [029.Divide-Two-Integers](https://github.com/wisdompeak/LeetCode/tree/master/Binary_Search/029.Divide-Two-Integers)