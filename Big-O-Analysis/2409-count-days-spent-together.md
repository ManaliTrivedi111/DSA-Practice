# 2409. Count Days Spent Together

# Problem Summary:

Given Alice's and Bob's arrival and departure dates in the same non-leap year, return the number of days they are both in the city. The date ranges are inclusive.

# Approach Used:

Convert each date into its day number within the year using a prefix-sum array containing the total number of days before each month.

Then calculate the overlap between the two inclusive date ranges:
- The overlap starts at the later of the two arrival dates.
- The overlap ends at the earlier of the two departure dates.
- Add 1 to include both endpoints, and use 0 if the ranges do not overlap.

# Steps:

1. Create an array containing the number of days in each month.
2. Build a prefix-sum array where prefixSum[m] stores the total number of days before month m + 1.
3. Convert Alice's arrival and departure dates, and Bob's arrival and departure dates, into day numbers using calculateDayOfYear.
4. Find the later arrival day and the earlier departure day.
5. Return the inclusive overlap, or 0 if the date ranges do not overlap.

# Solution:

```
class Solution {
    public int countDaysTogether(String arriveAlice, String leaveAlice, String arriveBob, String leaveBob) {
        int[] datesOfMonths = {31, 28, 31, 30, 31, 30, 31, 31, 30, 31, 30, 31};     // T(n) = O(1), S(n) = O(1)
        int[] prefixSum = new int[13];                                              // T(n) = O(1), S(n) = O(1)

        for(int i = 0; i < 12; i++) {                                               // T(n) = O(1)
            prefixSum[i + 1] = prefixSum[i] + datesOfMonths[i];                     // T(n) = O(1)
        }

        int arriveAliceDay = calculateDayOfYear(arriveAlice, prefixSum);            // T(n) = O(1), S(n) = O(1)
        int leaveAliceDay = calculateDayOfYear(leaveAlice, prefixSum);              // T(n) = O(1), S(n) = O(1)
        int arriveBobDay = calculateDayOfYear(arriveBob, prefixSum);                // T(n) = O(1), S(n) = O(1)
        int leaveBobDay = calculateDayOfYear(leaveBob, prefixSum);                  // T(n) = O(1), S(n) = O(1)

        return Math.max(0, Math.min(leaveAliceDay, leaveBobDay) - 
                           Math.max(arriveAliceDay, arriveBobDay) + 1);             // T(n) = O(1), S(n) = O(1)
    }

    private int calculateDayOfYear(String day, int[] prefixSum) {
        int month = Integer.parseInt(day.substring(0, 2));                          // T(n) = O(1), S(n) = O(1)
        int date = Integer.parseInt(day.substring(3));                              // T(n) = O(1), S(n) = O(1)
        return prefixSum[month - 1] + date;                                         // T(n) = O(1), S(n) = O(1)
    }
}
```

# Time Complexity:
T(n) = O(1)

# Space Complexity:
S(n) = O(1)

