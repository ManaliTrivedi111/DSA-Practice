# 1732. Find the Highest Altitude

# Problem Summary:
A biker starts a road trip at altitude 0. You are given an integer array gain, where gain[i] represents the net change in altitude between point i and point i + 1. Return the highest altitude reached at any point during the trip.

# Approach Used:
The solution keeps track of the biker's current altitude as the trip progresses. A variable totalGain stores the cumulative altitude after each segment of the trip. Another variable alt stores the highest altitude encountered so far. For each value in the gain array:
* Add the current gain to the cumulative altitude.
* Update the highest altitude if the current altitude is greater than the previous maximum.
Since the biker starts at altitude 0, the initial highest altitude is also 0.

# Steps:
1. Initialize:
   * alt to 0 (highest altitude so far).
   * totalGain to 0 (current altitude).
2. Traverse the gain array.
3. For each gain:
   * Add it to totalGain.
   * Update alt with the larger of alt and totalGain.
4. Return alt.

# Solution:
```
class Solution {
    public int largestAltitude(int[] gain) {
        int alt = 0;                                    // T(n) = O(1), S(n) = O(1)
        int totalGain = 0;                              // T(n) = O(1), S(n) = O(1)

        for(int g : gain) {
            totalGain += g;                             // T(n) = O(1) per iteration
            alt = Math.max(alt, totalGain);             // T(n) = O(1) per iteration
        }
        return alt;                                     // T(n) = O(1)
    }
}
```

# Time Complexity:
T(n) = O(n)

# Space Complexity:
S(n) = O(1)
