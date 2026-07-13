# 3020. Find the Maximum Number of Elements in Subset

# Problem Summary:
You are given an array of positive integers nums. Select the largest possible subset whose elements can be arranged in the following symmetric pattern:
[x, x^2, x^4, ..., x^(k/2), x^k, x^(k/2), ..., x^4, x^2, x]
where k is a non-negative power of 2.
Return the maximum number of elements that can form such a subset.

# Approach Used:
The solution first counts the frequency of every number using a HashMap. The number 1 is handled separately because:
* 1^2 = 1, so the sequence never grows.
* A valid symmetric sequence must have an odd length.
* Therefore, if there are an even number of 1s, one of them must be discarded.

For every remaining distinct number:
* Start building a chain by repeatedly squaring the current value.
* Each level of the chain requires at least two occurrences of the current number so that one copy can appear on each side of the symmetric sequence.
* If only one occurrence exists, it becomes the center of the sequence and the chain stops.
* The chain also stops if the next square would overflow the int range.

If the chain contains len levels, the resulting symmetric subset has: 2 × len - 1 elements.
The maximum length among all possible starting numbers is returned.

# Steps:
1. Count the frequency of every number.
2. Handle the special case for the number 1.
3. For every remaining distinct number:
   * Repeatedly square the number while it exists in the frequency map.
   * Stop if:
     * Only one occurrence remains, or
     * Squaring would overflow an integer.
   * Compute the subset length as 2 × len - 1.
   * Update the maximum answer.
4. Return the maximum subset length.

# Solution:

```
class Solution {
    public int maximumLength(int[] nums) {
        Map<Integer, Integer> cnt = new HashMap<>();          // T(n) = O(1), S(n) = O(n)
        int maxAns = 0;                                       // T(n) = O(1), S(n) = O(1)

        for(int num : nums) {
            cnt.merge(num, 1, Integer::sum);                  // T(n) = O(1) average per iteration
        }

        if(cnt.containsKey(1)) {
            int count = cnt.get(1);                           // T(n) = O(1)
            maxAns = (count % 2 == 0) ? count - 1 : count;    // T(n) = O(1)
            cnt.remove(1);                                    // T(n) = O(1)
        }

        for(int num : cnt.keySet()) {
            int len = 0;                                      // T(n) = O(1)

            while(cnt.containsKey(num)) {
                len++;                                        // T(n) = O(1) per iteration

                if(cnt.get(num) == 1 ||
                   (long) num * num > Integer.MAX_VALUE) {
                    break;
                }
                num *= num;                                   // T(n) = O(1)
            }
            maxAns = Math.max(maxAns, 2 * len - 1);           // T(n) = O(1)
        }
        return maxAns;                                        // T(n) = O(1)
    }
}
```

# Time Complexity:
T(n) = O(n)

# Space Complexity:
S(n) = O(n)
