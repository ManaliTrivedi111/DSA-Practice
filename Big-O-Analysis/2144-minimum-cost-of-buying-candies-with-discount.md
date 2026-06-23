# 2144. Minimum Cost of Buying Candies With Discount

# Problem Summary:
A candy shop offers a discount where for every two candies purchased, a third candy can be taken for free, provided its cost is less than or equal to the cheaper of the two purchased candies.
Given an array cost where cost[i] represents the price of the ith candy, return the minimum amount of money required to buy all candies.

# Approach Used:
Since candy costs are limited to the range 1 to 100, the solution uses a counting array to avoid sorting.
The idea is to process candies from the most expensive to the least expensive. This maximizes the value of the free candy because the discount should be applied to every third candy among the most expensive remaining candies.
The counting array stores the frequency of each candy cost. While traversing the costs in descending order:
The first two candies are counted as purchased and added to the total cost.
The third candy is treated as free and is not added to the total cost.
This pattern repeats until all candies are processed.

# Steps:
1) Create a frequency array of size 101 to count occurrences of each candy cost
2) Traverse the input array and populate the frequency array
3) Start processing costs from 100 down to 1
4) For each available candy:
   * If fewer than two candies have been purchased in the current group:
      Add its cost to the total
      Increase the purchased count
   * Otherwise:
      Treat it as the free candy
      Reset the purchased count
5) Continue until all candies are processed
6) Return the total minimum cost

# Solution:
```
class Solution {

	public int minimumCost(int[] cost) {

    		final int n = cost.length;                 // T(n) = O(1), S(n) = O(1)
    		int[] costs = new int[101];                // T(n) = O(1), S(n) = O(1)
    		int totalCost = 0;                         // T(n) = O(1), S(n) = O(1)
    		int bought = 0;                            // T(n) = O(1), S(n) = O(1)

    		// Loop runs n times
    		for(int i : cost) {
        		costs[i]++;                            // T(n) = O(1) per execution, total executions = O(n)
    		}

    		int i = 100;                               // T(n) = O(1), S(n) = O(1)

    		// Processes all candy costs from 100 down to 1
    		while(i > 0) {

        		if(costs[i] == 0) {                    // T(n) = O(1) per execution
            			i--;                             // T(n) = O(1) per execution

        		}else{
            			if(bought < 2) {                 // T(n) = O(1) per execution
                			totalCost += i;              // T(n) = O(1) per execution
                			bought++;                    // T(n) = O(1) per execution

            			}else{
                			bought = 0;                  // T(n) = O(1) per execution
            			}

            		costs[i]--;                        // T(n) = O(1) per execution
        		}
    		}
    		return totalCost;                          // T(n) = O(1)
	}
}
```

# Time Complexity:
T(n) = O(n)

# Space Complexity:
S(n) = O(1)

