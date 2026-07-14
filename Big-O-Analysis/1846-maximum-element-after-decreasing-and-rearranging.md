# 1846. Maximum Element After Decreasing and Rearranging

# Problem Summary:
You are given an array of positive integers arr. You may perform the following operations any number of times:
* Rearrange the elements in any order.
* Decrease any element to a smaller positive integer.

After the operations, the array must satisfy:
* The first element is 1.
* The absolute difference between every pair of adjacent elements is at most 1.

Return the maximum possible value of any element in the final array.

# Approach Used:
The solution uses a counting sort approach instead of sorting the array. Since the maximum possible value in the final array cannot exceed the array length n, every value greater than n is grouped into a single bucket (n + 1). A frequency array stores the count of each value. The variable next represents the smallest value that has not yet been assigned in the final rearranged array. The algorithm processes values in increasing order:
* If the current value equals next, one occurrence is placed directly, and next is incremented.
* If the current value is larger than next, the extra occurrences can be decreased to fill the missing values one by one until either:
  * all occurrences are used, or
  * next becomes greater than the current value.

After all values are processed, next - 1 is the largest achievable value in the rearranged array.

# Steps:
1. Create a frequency array.
2. Count the occurrence of each value.
3. Place every value greater than n into the bucket n + 1.
4. Initialize next = 1.
5. Traverse the frequency array:
   * If the current value equals next, place it and increment next.
   * Otherwise, repeatedly decrease copies of the current value to fill the missing values.
6. Return next - 1.

# Solution:

```
class Solution {
    public int maximumElementAfterDecrementingAndRearranging(int[] arr) {
        final int n = arr.length;                              // T(n) = O(1)
        int[] cnt = new int[n + 2];                            // T(n) = O(1), S(n) = O(n)

        for(int num : arr) {
            if(num > n) {
                cnt[n + 1]++;                                  // T(n) = O(1) per iteration
            } else {
                cnt[num]++;                                    // T(n) = O(1) per iteration
            }
        }

        int next = 1;                                          // T(n) = O(1), S(n) = O(1)

        for(int i = 1; i <= n + 1; i++) {
            if(cnt[i] > 0) {
                if(i == next) {
                    next++;                                    // T(n) = O(1)
                } else {
                    while(cnt[i] > 0 && next <= i) {
                        next++;                                // T(n) = O(1) per iteration
                        cnt[i]--;                              // T(n) = O(1) per iteration
                    }
                }
            }
        }
        return next - 1;                                       // T(n) = O(1)
    }
}
```

# Time Complexity:
T(n) = O(n)

# Space Complexity:
S(n) = O(n)
