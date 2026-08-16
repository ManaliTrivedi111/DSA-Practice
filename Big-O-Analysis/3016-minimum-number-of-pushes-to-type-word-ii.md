# 3016. Minimum Number of Pushes to Type Word II

# Problem Summary:

You are given a string word containing lowercase English letters. There are 8 available telephone keys: 2 through 9. Each letter must be assigned to exactly one key. The first letter assigned to a key requires 1 push, the second letter requires 2 pushes, the third letter requires 3 pushes, and so on. Letters can appear multiple times in word. Therefore, frequently occurring letters should be assigned to fewer pushes.

# Approach Used:

The key observation is that letters with higher frequencies should be assigned to lower push counts.

There are 8 keys, so:
* The 8 most frequent letters require 1 push each.
* The next 8 most frequent letters require 2 pushes each.
* The next 8 most frequent letters require 3 pushes each.
* The remaining letters require 4 pushes each.

The solution first counts the frequency of every letter, then sorts these frequencies in ascending order. By traversing the sorted array from largest to smallest, the most frequent letters are assigned the smallest push counts.

# Steps:

1. Create a frequency array cnt of size 26.
2. Count how many times each lowercase letter appears in word.
3. Sort the frequency array.
4. Traverse the frequencies from largest to smallest.
5. Assign push count 1 to the first 8 letters, push count 2 to the next 8, and so on.
6. Add frequency * push to the total answer for each letter.
7. Return the total number of pushes.

# Solution:

```
class Solution {
    public int minimumPushes(String word) {
        int[] cnt = new int[26];                                           // T(n) = O(1), S(n) = O(1)
        int ans = 0;                                                       // T(n) = O(1), S(n) = O(1)
        int push = 1;                                                      // T(n) = O(1), S(n) = O(1)
        int counter = 2;                                                   // T(n) = O(1), S(n) = O(1)

        for(char c : word.toCharArray()) {
            cnt[c - 'a']++;                                                // T(n) = O(1)
        }

        Arrays.sort(cnt);                                                  // T(n) = O(26 log 26) = O(1)

        for(int i = 25; i >= 0 && cnt[i] > 0; i--) {
            ans += cnt[i] * push;                                          // T(n) = O(1)
            counter++;                                                     // T(n) = O(1)

            if(counter > 9) {
                counter = 2;                                               // T(n) = O(1)
                push++;                                                    // T(n) = O(1)
            }
        }
        return ans;                                                        // T(n) = O(1), S(n) = O(1)
    }
}
```

# Time Complexity:
T(n) = O(n)

# Space Complexity:
S(n) = O(1)
