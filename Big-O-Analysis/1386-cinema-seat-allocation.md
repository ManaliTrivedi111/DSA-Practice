# 1386. Cinema Seat Allocation

# Problem Summary:

You are given a cinema with n rows, where each row has 10 seats. Some seats are already reserved. 

A four-person group can sit together in one of three possible blocks:
* Seats 2, 3, 4, 5
* Seats 4, 5, 6, 7
* Seats 6, 7, 8, 9

For each row, we need to determine whether we can place two groups, one group, or no group. 

# Approach Used:

The solution uses a HashMap, and a bitmask to represent the reserved seats in each row. Since seats 1 through 10 can be represented by 10 bits, we can efficiently check whether the required four-seat blocks are available.

The HashMap stores only rows that have at least one reserved seat.

For each reserved seat, we set the corresponding bit in that row's bitmask.

We then check the bitmask for each affected row:
* If seats 2-9 are all free, we can place two groups.
* Otherwise, if any one of the three valid four-seat blocks is completely free, we can place one group.
* Otherwise, we cannot place any group in that row.

Rows with no reserved seats are not stored in the map. Every such row can accommodate two groups.

Finally, we add 2 groups for every completely unreserved row.

This works because the only seats that matter for four-person groups are seats 2 through 9. Seats 1 and 10 are never part of a group.

# Steps:

1. Create a HashMap called rowToSeats to store the bitmask of reserved seats for each row that has reservations.
2. Iterate through all reserved seats.
3. For each reserved seat, calculate its bit position using 1 << (seat - 1).
4. Combine that bit with the existing bitmask for the row using bitwise OR.
5. Iterate through all rows that have at least one reserved seat.
6. If seats 2 through 9 are all available, add 2 groups.
7. Otherwise, check the three possible four-seat blocks: seats 2-5, 4-7, and 6-9.
8. If at least one block is available, add 1 group.
9. Rows that are absent from the map have no reserved seats, so each of them can accommodate 2 groups.
10. Return the groups found in reserved rows plus 2 * (n - rowToSeats.size()).

# Solution:

```
class Solution {
    public int maxNumberOfFamilies(int n, int[][] reservedSeats) {
        int ans = 0;                                                                     // T(n) = O(1), S(n) = O(1)
        Map<Integer, Integer> rowToSeats = new HashMap<>();                              // T(n) = O(1), S(n) = O(R)

        for(int[] reservedSeat : reservedSeats) {                      
            final int row = reservedSeat[0];                                             // T(n) = O(1), S(n) = O(1)
            final int seat = reservedSeat[1];                                            // T(n) = O(1), S(n) = O(1)
            rowToSeats.put(row, rowToSeats.getOrDefault(row, 0) | 1 << (seat - 1));      // T(n) = O(1) average, S(n) = O(1)
        }

        for(final int seats : rowToSeats.values()) { 
            if((seats & 0b0111111110) == 0) {                                            // T(n) = O(1) per execution
                ans += 2;                                                                // T(n) = O(1) per execution
            }else if((seats & 0b0111100000) == 0 || 
                    (seats & 0b0001111000) == 0 || 
                    (seats & 0b0000011110) == 0) {                                       // T(n) = O(1) per execution
                        ans += 1;                                                        // T(n) = O(1) per execution
            }
                
        }
        return ans + (n - rowToSeats.size()) * 2;                                        // T(n) = O(1), S(n) = O(1)
    }
}
```

# Time Complexity:
T(R) = O(R), where R = the number of reserved seats.

# Space Complexity:
S(R) = O(R), where R = the number of reserved seats.
