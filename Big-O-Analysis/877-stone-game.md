# 877. Stone Game

# Problem Summary:

You are given an even number of piles of stones arranged in a row. Alice and Bob take turns removing an entire pile from either the beginning or the end of the row. Alice goes first, and both players play optimally.

The total number of stones across all piles is odd, so there cannot be a tie. The goal is to determine whether Alice can win.

# Approach Used:

The key observation is that Alice can always guarantee a win when the number of piles is even.

Consider the piles at even and odd positions:
* Positions 0, 2, 4, ... form one group.
* Positions 1, 3, 5, ... form another group.

Because there is an even number of piles, both groups contain the same number of piles. Alice can choose the parity group that has the larger total number of stones and then always make moves that allow her to take piles from that group. Since the total number of stones is odd, the two parity groups cannot have the same total. Therefore, one group must contain strictly more stones than the other.

Alice can guarantee that she collects all piles from the group with the larger total, so she will always finish with more stones than Bob. Therefore, for every valid input, the answer is always true. The solution does not need to inspect the array or perform any calculations. It simply returns true.

# Steps:

1. Observe that the number of piles is always even.
2. Divide the pile positions into two groups based on their parity.
3. Both groups contain the same number of piles.
4. Since the total number of stones is odd, the two groups cannot have equal sums.
5. Alice can guarantee collecting the group with the larger sum.
6. Therefore, Alice always wins.
7. Return true.

# Solution:

```
class Solution {
    public boolean stoneGame(int[] piles) {
        return true;                                                        // T(n) = O(1), S(n) = O(1)
    }
}
```


# Time Complexity:
T(n) = O(1)

# Space Complexity:
S(n) = O(1)

