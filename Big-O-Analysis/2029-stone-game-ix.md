# 2029. Stone Game IX

# Problem Summary:

You are given an integer array stones, where each element represents the value of a stone. Alice and Bob take turns removing one stone from the array, with Alice going first. The sum of all removed stones is tracked. If a player removes a stone and the total sum of the removed stones becomes divisible by 3, that player loses immediately. If there are no stones remaining, Bob wins automatically.

Both players play optimally. Return true if Alice wins and false if Bob wins.

# Approach Used:

The solution does not need to simulate the game. Only the remainder of each stone when divided by 3 matters because we only care whether the running sum of the removed stones is divisible by 3.

There are only three possible remainders: 0, 1, and 2
So, we count how many stones have each remainder using the count array.
The game can then be analyzed based on the number of stones with remainder 0, 1, and 2.

There are two main cases:
* Case 1: count[0] is even
When the number of remainder-0 stones is even, the remainder-0 stones effectively cancel each other out in pairs.
For Alice to win, there must be at least one stone with remainder 1 and at least one stone with remainder 2.
Therefore, we return: Math.min(count[1], count[2]) > 0
This checks whether both types of non-zero remainder stones are available.

* Case 2: count[0] is odd
When the number of remainder-0 stones is odd, Alice can win only when the counts of remainder-1 and remainder-2 stones differ by more than 2.
Therefore, we return: Math.abs(count[1] - count[2]) > 2
This captures the winning condition that results from optimally alternating between stones with remainders 1 and 2.

# Steps:

1. Create an array count of size 3 to count stones according to their remainder when divided by 3.
2. Traverse all stones and increment count[stone % 3].
3. Check whether the number of remainder-0 stones is even.
4. If it is even, Alice wins if there is at least one remainder-1 stone and at least one remainder-2 stone.
5. If the number of remainder-0 stones is odd, Alice wins if the difference between the number of remainder-1 and remainder-2 stones is greater than 2.
6. Return the corresponding result.

# Solution:

```
class Solution {
    public boolean stoneGameIX(int[] stones) {
        int[] count = new int[3];                                            // T(n) = O(1), S(n) = O(1)

        for(int stone : stones) {
            count[stone % 3]++;                                              // T(n) = O(1), S(n) = O(1)
        }

        if(count[0] % 2 == 0) {
            return Math.min(count[1], count[2]) > 0;                         // T(n) = O(1), S(n) = O(1)
        }

        return Math.abs(count[1] - count[2]) > 2;                            // T(n) = O(1), S(n) = O(1)
    }
}
```

# Time Complexity:
T(n) = O(n)

# Space Complexity:
S(n) = O(1)
