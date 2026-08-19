# 1406. Stone Game III

# Problem Summary:

You are given an array stoneValue representing stones arranged in a row. Alice and Bob take turns, with Alice starting first. On each turn, a player can take 1, 2, or 3 stones from the beginning of the remaining row. Each player's score is the sum of the values of the stones they take. Both players play optimally. Return "Alice" if Alice has a higher score, "Bob" if Bob has a higher score, or "Tie" if both players finish with the same score.

# Approach Used:

The solution uses dynamic programming with constant space. Let f[i] represent the maximum score the current player can obtain from the suffix starting at index i. Let suffSum be the total sum of the stones from index i onward.

The current player can take 1, 2, or 3 stones. After taking those stones, the opponent plays optimally on the remaining suffix. Therefore, the current player wants to leave the opponent with the minimum possible score.

So: f[i] = suffSum - min(f[i + 1], f[i + 2], f[i + 3])

The variables f1, f2, and f3 store the DP values for the next three positions. This avoids creating a DP array.

For every index: newF = suffSum - min(f1, f2, f3)

Then the variables are shifted:
* f3 = f2
* f2 = f1
* f1 = newF

After processing the entire array, f1 is Alice's maximum possible score.

Since suffSum is the total score of all stones, Bob's score is: Bob's score = suffSum - f1
Therefore: diff = f1 - (suffSum - f1)

If diff > 0, Alice wins. If diff < 0, Bob wins. Otherwise, the result is a tie.

# Steps:

1. Store the length of the array in n.
2. Initialize suffSum to store the sum of the suffix currently being processed.
3. Initialize f1, f2, and f3 to represent the DP values for the next three positions.
4. Traverse the array from right to left.
5. Add the current stone value to suffSum.
6. Calculate the current DP value by subtracting the minimum of the three possible future DP values from suffSum.
7. Shift f1, f2, and f3 for the next iteration.
8. After the loop, calculate the score difference between Alice and Bob.
9. Return "Alice" if the difference is positive, "Bob" if it is negative, or "Tie" if it is zero.

# Solution:

```
class Solution {
    public String stoneGameIII(int[] stoneValue) {
        final int n = stoneValue.length;                                     // T(n) = O(1), S(n) = O(1)
        int suffSum = 0;                                                     // T(n) = O(1), S(n) = O(1)
        int f1 = 0;                                                          // T(n) = O(1), S(n) = O(1)
        int f2 = 0;                                                          // T(n) = O(1), S(n) = O(1)
        int f3 = 0;                                                          // T(n) = O(1), S(n) = O(1)

        for (int i = n - 1; i >= 0; i--) {
            suffSum += stoneValue[i];                                        // T(n) = O(1), S(n) = O(1)

            int newF = suffSum - Math.min(Math.min(f1, f2), f3);             // T(n) = O(1), S(n) = O(1)

            f3 = f2;                                                         // T(n) = O(1), S(n) = O(1)
            f2 = f1;                                                         // T(n) = O(1), S(n) = O(1)
            f1 = newF;                                                       // T(n) = O(1), S(n) = O(1)
        }

        int diff = f1 - (suffSum - f1);                                      // T(n) = O(1), S(n) = O(1)
        return diff > 0 ? "Alice" : (diff < 0 ? "Bob" : "Tie");              // T(n) = O(1), S(n) = O(1)
    }
}
```

# Time Complexity:
T(n) = O(n)

# Space Complexity:
S(n) = O(1)
