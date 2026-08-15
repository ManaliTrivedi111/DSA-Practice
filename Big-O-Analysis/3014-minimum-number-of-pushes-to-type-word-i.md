# 3014. Minimum Number of Pushes to Type Word I

# Problem Summary:

You are given a string word containing distinct lowercase English letters. There are 8 available telephone keys: 2 through 9. Each letter must be assigned to exactly one key. The first letter assigned to a key requires 1 push, the second requires 2 pushes, the third requires 3 pushes, and so on.

You can remap the keys to minimize the total number of pushes needed to type word.

# Approach Used:

Since every character in word is distinct, every character needs to be typed exactly once. There are 8 available keys, so the first 8 letters can each be assigned to a different key and require only 1 push each.

The next 8 letters require 2 pushes each, because they become the second letter assigned to their respective keys. The following 8 letters require 3 pushes each, and so on. Therefore, we do not need to construct an actual keypad mapping. We simply assign the smallest possible push count to each character.

The push-cost groups are: 1, 1, 1, 1, 1, 1, 1, 1
then: 2, 2, 2, 2, 2, 2, 2, 2
then: 3, 3, ...

The variables counter and push keep track of these groups.

# Steps:

1. Initialize counter = 2 and push = 1.
2. Traverse every character in word.
3. Add the current push value to the result.
4. Increment counter to represent assigning the next character to another key.
5. After all 8 keys have received a character at the current push level, reset counter to 2 and increase push.
6. Return the total number of pushes.

# Solution:

```
class Solution {
    public int minimumPushes(String word) {
        int counter = 2;                                                   // T(n) = O(1), S(n) = O(1)
        int push = 1;                                                      // T(n) = O(1), S(n) = O(1)
        int res = 0;                                                       // T(n) = O(1), S(n) = O(1)

        for(char c : word.toCharArray()) {
            res += push;                                                   // T(n) = O(1)
            counter++;                                                     // T(n) = O(1)

            if(counter > 9) {
                counter = 2;                                               // T(n) = O(1)
                push++;                                                    // T(n) = O(1)
            }
        }
        return res;                                                        // T(n) = O(1), S(n) = O(1)
    }
}
```

# Time Complexity:
T(n) = O(n)

# Space Complexity:
S(n) = O(1)
