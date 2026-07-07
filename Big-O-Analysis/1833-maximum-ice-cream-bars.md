# 1833. Maximum Ice Cream Bars

# Problem Summary:
You are given an array costs, where costs[i] is the price of the ith ice cream bar, and an integer coins representing the number of coins the boy has. The boy can buy the ice cream bars in any order. Return the maximum number of ice cream bars he can buy without exceeding the available coins. The problem requires using counting sort.

# Approach Used:
Instead of sorting the costs array, the solution uses a counting sort approach because the price of each ice cream bar is bounded. A frequency array stores how many ice cream bars exist for each possible price. After building this frequency array, the solution processes prices from the smallest to the largest.
For each price:
* If all bars at that price can be purchased, buy them all.
* Otherwise, buy as many as the remaining coins allow and return the answer immediately.
Buying the cheapest bars first always maximizes the total number of bars purchased.

# Steps:
1. Create a frequency array to count the number of ice cream bars for each price.
2. Find the maximum price present in the input.
3. Traverse prices from 1 to the maximum price.
4. For each price:
   * If all bars at that price can be afforded:
     * Buy all of them.
     * Decrease the remaining coins.
   * Otherwise:
     * Buy as many as possible.
     * Return the total number of bars purchased.
5. Return the total number of bars bought.

# Solution:

```
class Solution {
    public int maxIceCream(int[] costs, int coins) {
        final int n = costs.length;                          // T(n) = O(1)
        int[] count = new int[100001];                       // T(n) = O(1), S(n) = O(1)
        int maxPrice = 0;                                    // T(n) = O(1), S(n) = O(1)
        int canBuy = 0;                                      // T(n) = O(1), S(n) = O(1)

        for(int cost : costs) {
            count[cost]++;                                   // T(n) = O(1) per iteration
            maxPrice = Math.max(maxPrice, cost);             // T(n) = O(1) per iteration
        }

        for(int i = 1; i <= maxPrice && coins > 0; i++) {
            if(count[i] > 0) {
                if(count[i] * i <= coins) {
                    canBuy += count[i];                      // T(n) = O(1)
                    coins -= count[i] * i;                   // T(n) = O(1)
                } else {
                    canBuy += (coins / i);                   // T(n) = O(1)
                    return canBuy;                           // T(n) = O(1)
                }
            }
        }
        return canBuy;                                       // T(n) = O(1)
    }
}
```

# Time Complexity:
T(n) = O(n + k)

where n is the number of ice cream bars and k is the maximum ice cream bar price.

# Space Complexity:
S(n) = O(1)
