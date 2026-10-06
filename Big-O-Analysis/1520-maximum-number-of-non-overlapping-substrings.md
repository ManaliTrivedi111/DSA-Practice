# 1520. Maximum Number of Non-Overlapping Substrings

# Problem Summary

Given a string s containing lowercase English letters, find the maximum number of non-overlapping substrings such that every substring satisfies this condition:

If a character appears inside a selected substring, the substring must contain all occurrences of that character in the entire string.

If multiple solutions contain the same maximum number of substrings, choose the solution with the minimum total length.

# Approach Used

The solution uses:
1. Two arrays, first and last, to store the first and last occurrence of each of the 26 lowercase letters.
2. For every character that appears in the string, construct the smallest valid interval starting at its first occurrence.
3. While constructing an interval, expand its end whenever a character inside the interval has a later occurrence.
4. If a character inside the interval has its first occurrence before the interval's start, the interval is invalid.
5. Store all valid intervals.
6. Sort the valid intervals by their ending index.
7. Greedily select an interval whenever its start is after the end of the previously selected interval.

Because there are only 26 possible lowercase characters, the interval-generation phase is bounded by a constant number of character ranges. The final greedy selection chooses the maximum number of non-overlapping valid intervals, and sorting by end position also gives the minimum total length among solutions with the maximum number of substrings.

# Steps

1. Store the length of the string in n.
2. Create first and last arrays of size 26.
3. Initialize every entry of both arrays to -1.
4. Scan the string once to determine the first and last occurrence of every character.
5. For every character that appears:
   1. Start an interval at its first occurrence.
   2. Initially set the end to its last occurrence.
   3. Scan the current interval.
   4. If a character inside the interval occurs before the interval's start, mark the interval invalid.
   5. Otherwise, expand the end to include all occurrences of the characters encountered.
6. Store every valid interval.
7. Sort the valid intervals by their ending index.
8. Iterate through the sorted intervals.
9. Select an interval if its start is after the end of the previously selected interval.
10. Add the corresponding substring to the result.
11. Return the result.

# Solution

```
class Solution {
    public List<String> maxNumOfSubstrings(String s) {
        final int n = s.length();                                            // T(n) = O(n), S(n) = O(1)
        int[] first = new int[26];                                           // T(n) = O(1), S(n) = O(1)
        int[] last = new int[26];                                            // T(n) = O(1), S(n) = O(1)
        Arrays.fill(first, -1);                                              // T(n) = O(1), S(n) = O(1)
        Arrays.fill(last, -1);                                               // T(n) = O(1), S(n) = O(1)

        for(int i = 0; i < n; i++) {                                         // T(n) = O(n)
            int charIdx = s.charAt(i) - 'a';                                 // T(n) = O(1)

            if(first[charIdx] == -1) {                                       // T(n) = O(1)
                first[charIdx] = i;                                          // T(n) = O(1)
            }
            last[charIdx] = i;                                               // T(n) = O(1)
        }

        List<int[]> validIntervals = new ArrayList<>();                      // T(n) = O(1), S(n) = O(1)

        for(int i = 0; i < 26; i++) {                                        // T(n) = O(1)
            if(first[i] == -1) continue;                                     // T(n) = O(1)

            int start = first[i];                                            // T(n) = O(1), S(n) = O(1)
            int end = last[i];                                               // T(n) = O(1), S(n) = O(1)
            boolean isValid = true;                                          // T(n) = O(1), S(n) = O(1)

            for(int j = start; j <= end; j++) {                              // T(n) = O(n) overall
                int currChar = s.charAt(j) - 'a';                            // T(n) = O(1)

                if(first[currChar] < start) {                                // T(n) = O(1)
                    isValid = false;                                         // T(n) = O(1)
                    break;                                                   // T(n) = O(1)
                }
                end = Math.max(end, last[currChar]);                         // T(n) = O(1)
            }

            if(isValid) {                                                    // T(n) = O(1)
                validIntervals.add(new int[]{start, end});                   // T(n) = O(1), S(n) = O(1)
            }
        }

        validIntervals.sort((a, b) -> Integer.compare(a[1], b[1]));          // T(n) = O(1), S(n) = O(1)

        int prevIdx = -1;                                                    // T(n) = O(1), S(n) = O(1)
        List<String> result = new ArrayList<>();                             // T(n) = O(1), S(n) = O(1)

        for(int[] interval : validIntervals) {                               // T(n) = O(1)
            int start = interval[0];                                         // T(n) = O(1)
            int end = interval[1];                                           // T(n) = O(1)

            if(start > prevIdx) {                                            // T(n) = O(1)
                result.add(s.substring(start, end + 1));                     // T(n) = O(n) overall
                prevIdx = end;                                               // T(n) = O(1)
            }
        }
        return result;                                                       // T(n) = O(1)
    }
}
```

# Time Complexity:
T(n) = O(n)

# Space Complexity:
S(n) = O(1)

