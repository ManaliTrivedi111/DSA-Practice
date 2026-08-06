# 3499. Maximize Active Section with Trade I

# Problem Summary:
You are given a binary string s, where:
* '1' represents an active section.
* '0' represents an inactive section.

You may perform at most one trade consisting of:
1. Converting a contiguous block of '1's that is surrounded by '0's into '0's.
2. Then converting a contiguous block of '0's that is surrounded by '1's into '1's.

The string is treated as if it has an extra '1' at both ends ('1' + s + '1') when determining whether a block is surrounded. Return the maximum possible number of active sections after the optimal trade.

# Approach Used:
The solution observes that after removing one surrounded block of '1's, the two adjacent groups of '0's become one larger group. That merged group can then be converted entirely into '1's.

Therefore:
* Traverse the string and record the length of every contiguous group of '0's.
* Compute the largest sum of two consecutive zero groups, representing the largest merged zero block that can be activated.
* Count the number of '1's already present.
* The answer is the original number of active sections plus the largest merged zero block.

# Steps:
1. Traverse the string and store the length of every contiguous group of '0's.
2. Find the maximum sum of two consecutive zero groups.
3. Count the total number of '1's in the string.
4. Return: countOne + maxZeroMerge

# Solution:

```
class Solution {

    public int maxActiveSectionsAfterTrade(String s) {
        char[] str = s.toCharArray();                                    // T(n) = O(n), S(n) = O(n)
        final int n = str.length;                                        // T(n) = O(1), S(n) = O(1)
        List<Integer> zeroGroupLengths = new ArrayList<>();              // T(n) = O(1), S(n) = O(n)
        int maxZeroMerge = 0;                                            // T(n) = O(1), S(n) = O(1)
        int i = 0;                                                       // T(n) = O(1), S(n) = O(1)

        while(i < n) {
            if(str[i] == '0') {
                int count = 0;                                           // T(n) = O(1)

                while(i < n && str[i] == '0') {
                    count++;                                             // T(n) = O(1)
                    i++;                                                 // T(n) = O(1)
                }
                zeroGroupLengths.add(count);                             // T(n) = O(1)
            }
            i++;                                                         // T(n) = O(1)
        }

        final int m = zeroGroupLengths.size();                           // T(n) = O(1), S(n) = O(1)

        for(int j = 0; j < m - 1; j++) {
            maxZeroMerge = Math.max(maxZeroMerge,
                                    zeroGroupLengths.get(j)
                                  + zeroGroupLengths.get(j + 1));        // T(n) = O(1)
        }

        int countOne = 0;                                                // T(n) = O(1), S(n) = O(1)

        for(char c : str) {
            if(c == '1') {
                countOne++;                                              // T(n) = O(1)
            }
        }
        return countOne + maxZeroMerge;                                 // T(n) = O(1), S(n) = O(1)
    }
}


# Time Complexity:
T(n) = O(n)

# Space Complexity:
S(n) = O(n)
