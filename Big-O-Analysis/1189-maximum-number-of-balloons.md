# 1189. Maximum Number of Balloons

# Problem Summary:
Given a string text, determine the maximum number of times the word "balloon" can be formed using its characters. Each character in text can be used at most once.

# Approach Used:
The solution counts the frequency of every character in the input string using a frequency array. To form the word "balloon", the following characters are required:
* 'b' → 1 occurrence
* 'a' → 1 occurrence
* 'l' → 2 occurrences
* 'o' → 2 occurrences
* 'n' → 1 occurrence
After counting the frequencies, the solution computes how many complete copies of "balloon" can be formed from each required character. Since 'l' and 'o' each appear twice in the word, their frequencies are divided by 2. The minimum among these values determines the maximum number of complete "balloon" words that can be formed.

# Steps:
1. Create a frequency array for all ASCII characters.
2. Traverse the input string and count the occurrence of each character.
3. Compute:
   * Number of 'b'
   * Number of 'a'
   * Number of 'n'
   * Number of 'l' / 2
   * Number of 'o' / 2
4. Return the minimum of these values.

# Solution:

```
class Solution {
    public int maxNumberOfBalloons(String text) {
        int[] count = new int[123];                             // T(n) = O(1), S(n) = O(1)

        for(char c : text.toCharArray()) {
            count[c]++;                                         // T(n) = O(1) per iteration
        }

        return Math.min(count['b'],
                   Math.min(count['a'],
                       Math.min(count['n'],
                           Math.min(count['l'] / 2, count['o'] / 2)
                        )
                )
        );                                                       // T(n) = O(1)
    }
}
```

# Time Complexity:
T(n) = O(n)

# Space Complexity:
S(n) = O(1)
