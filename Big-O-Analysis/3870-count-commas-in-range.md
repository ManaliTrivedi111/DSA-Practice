# 3870. Count Commas in Range

# Problem Summary:

You are given an integer n. The task is to return the total number of commas used when writing all integers from 1 to n, inclusive, using standard number formatting.

A comma is inserted after every three digits from the right. Therefore:
* Numbers from 1 to 999 contain no commas.
* Every number from 1000 onward contains at least one comma.

# Approach Used:

The solution first checks whether n is less than 1000.

If n < 1000, every number from 1 to n has fewer than four digits, so none of them contains a comma. Therefore, the answer is 0.

If n >= 1000, every number from 1000 to n contains exactly one comma under the problem's constraints. The number of such integers is: n - 999

Therefore, the answer is simply n - 999.

# Steps:

1. Check whether n < 1000.
2. If true, return 0 because no number from 1 to n contains a comma.
3. Otherwise, count the integers from 1000 through n.
4. The number of integers in this range is n - 999.
5. Return n - 999.

# Solution:

```
class Solution {
    public int countCommas(int n) {
        if(n < 1000) {                                                       // T(n) = O(1), S(n) = O(1)
            return 0;                                                        // T(n) = O(1), S(n) = O(1)
        }
        return n - 999;                                                      // T(n) = O(1), S(n) = O(1)
    }
}
```

# Time Complexity:
T(n) = O(1)

# Space Complexity:
S(n) = O(1)
