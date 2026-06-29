# 3635. Earliest Finish Time for Land and Water Rides II

# Problem Summary:
A tourist must experience exactly one land ride and one water ride, in any order.
For each land ride:
* landStartTime[i] represents the earliest time the ride can be boarded.
* landDuration[i] represents the duration of the ride.
For each water ride:
* waterStartTime[j] represents the earliest time the ride can be boarded.
* waterDuration[j] represents the duration of the ride.
A ride can be started at its opening time or any later time. After completing the first ride, the tourist may immediately board the second ride if it is already open; otherwise, they must wait until it opens. Return the earliest possible time at which both rides can be completed.

# Approach Used:
The solution considers both possible ride orders:
1. Land Ride -> Water Ride
2. Water Ride -> Land Ride

For the first order, only the earliest finishing land ride matters. Once the tourist finishes the earliest possible land ride, every water ride can be considered as the second ride. Similarly, for the second order, only the earliest finishing water ride matters. Once the tourist finishes the earliest possible water ride, every land ride can be considered as the second ride.
For each possible second ride:
* If the ride has already opened when the first ride finishes, it can start immediately.
* Otherwise, the tourist must wait until the ride opens.
The starting time of the second ride is therefore: maximum(firstRideFinishTime, secondRideStartTime)
The minimum finish time obtained from both ride orders is returned.

# Steps:
1) Find the earliest finishing time among all land rides
   * landStartTime[i] + landDuration[i]
2) Find the earliest finishing time among all water rides
   * waterStartTime[i] + waterDuration[i]
3) Consider every water ride as the second ride
   * Calculate the finish time for: Land Ride -> Water Ride
4) Consider every land ride as the second ride
   * Calculate the finish time for: Water Ride -> Land Ride
5) Track the minimum finish time across all possibilities
6) Return the minimum finish time

# Solution:
```
class Solution {

    public int earliestFinishTime(int[] landStartTime, int[] landDuration, int[] waterStartTime, int[] waterDuration) {

        final int m = landStartTime.length;                           // T(n) = O(1), S(n) = O(1)
        final int n = waterStartTime.length;                          // T(n) = O(1), S(n) = O(1)
        int minLandRideTimeTaken = Integer.MAX_VALUE;                 // T(n) = O(1), S(n) = O(1)
        int minWaterRideTimeTaken = Integer.MAX_VALUE;                // T(n) = O(1), S(n) = O(1)
        int minFinishTime = Integer.MAX_VALUE;                        // T(n) = O(1), S(n) = O(1)

        // Loop runs m times
        for(int i = 0; i < m; i++) {
            minLandRideTimeTaken = Math.min(minLandRideTimeTaken,
                         landStartTime[i] + landDuration[i]);         // T(n) = O(1) per execution, total executions = O(m)
        }

        // Loop runs n times
        for(int i = 0; i < n; i++) {
            minWaterRideTimeTaken = Math.min(minWaterRideTimeTaken,
                         waterStartTime[i] + waterDuration[i]);       // T(n) = O(1) per execution, total executions = O(n)
        }

        // Loop runs n times
        for(int i = 0; i < n; i++) {
            minFinishTime = Math.min(minFinishTime, Math.max(minLandRideTimeTaken,
                                  waterStartTime[i])
                         + waterDuration[i]);                        // T(n) = O(1) per execution, total executions = O(n)
        }

        // Loop runs m times
        for(int i = 0; i < m; i++) {
            minFinishTime = Math.min(minFinishTime, Math.max(minWaterRideTimeTaken,
                                  landStartTime[i])
                         + landDuration[i]);                         // T(n) = O(1) per execution, total executions = O(m)
        }
        return minFinishTime;                                        // T(n) = O(1)
    }
}
```

# Time Complexity:
T(m, n) = O(m + n)

# Space Complexity:
S(m, n) = O(1)
