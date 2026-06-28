# 3633. Earliest Finish Time for Land and Water Rides

# Problem Summary:
A tourist must take exactly one land ride and one water ride, in any order.

For each land ride:
* landStartTime[i] represents the earliest time the ride can be started.
* landDuration[i] represents how long the ride lasts.

For each water ride:
* waterStartTime[j] represents the earliest time the ride can be started.
* waterDuration[j] represents how long the ride lasts.

A ride can be started at its opening time or any later time. After finishing the first ride, the tourist may immediately board the second ride if it is already open, otherwise they must wait until it opens. Return the earliest possible time at which both rides can be completed.

# Approach Used:
The solution evaluates both possible ride orders:
1) Land ride -> Water ride
2) Water ride -> Land ride

For the first order, only the earliest possible finishing time among all land rides matters. Once that minimum land finish time is known, every water ride can be considered as the second ride.
Similarly, for the second order, only the earliest possible finishing time among all water rides matters. Once that minimum water finish time is known, every land ride can be considered as the second ride.

For each possible second ride:
* If the ride has already opened when the first ride finishes, it can start immediately.
* Otherwise, the tourist waits until the ride opens.

This starting time is calculated using: maximum(firstRideFinishTime, secondRideStartTime)
The minimum finish time across both ride orders is returned.

# Steps:
1) Find the minimum finishing time among all land rides
   * landStartTime[i] + landDuration[i]
2) Find the minimum finishing time among all water rides
   * waterStartTime[i] + waterDuration[i]
3) Consider every water ride as the second ride
   * Calculate the finish time for the order: Land Ride -> Water Ride
4) Consider every land ride as the second ride
   * Calculate the finish time for the order: Water Ride -> Land Ride
5) Track the minimum finish time obtained from both cases
6) Return the minimum finish time

# Solution:
```
class Solution {

    public int earliestFinishTime(int[] landStartTime, int[] landDuration, int[] waterStartTime, int[] waterDuration) {

    		final int m = landStartTime.length;                     // T(n) = O(1), S(n) = O(1)
    		final int n = waterStartTime.length;                    // T(n) = O(1), S(n) = O(1)

    		int minLandRideTimeTaken = Integer.MAX_VALUE;           // T(n) = O(1), S(n) = O(1)
    		int minWaterRideTimeTaken = Integer.MAX_VALUE;          // T(n) = O(1), S(n) = O(1)
    		int minFinishTime = Integer.MAX_VALUE;                  // T(n) = O(1), S(n) = O(1)

    		// Loop runs m times
    		for(int i = 0; i < m; i++) {
        		minLandRideTimeTaken = Math.min(minLandRideTimeTaken, landStartTime[i] + landDuration[i]);                                                                                         // T(n) = O(1) per execution, total executions = O(m)
    		}

    		// Loop runs n times
    		for(int i = 0; i < n; i++) {
        		minWaterRideTimeTaken = Math.min(minWaterRideTimeTaken, waterStartTime[i] + waterDuration[i]);                                                                                     // T(n) = O(1) per execution, total executions = O(n)
    		}

    		// Loop runs n times
    		for(int i = 0; i < n; i++) {
        		minFinishTime = Math.min(minFinishTime, Math.max(minLandRideTimeTaken, waterStartTime[i]) + waterDuration[i]);                                                                     // T(n) = O(1) per execution, total executions = O(n)
    		}

    		// Loop runs m times
    		for(int i = 0; i < m; i++) {
        		minFinishTime = Math.min(minFinishTime, Math.max(minWaterRideTimeTaken, landStartTime[i]) + landDuration[i]);                                                                      // T(n) = O(1) per execution, total executions = O(m)
    		}

    		return minFinishTime;                         			    // T(n) = O(1)
	  }
}
```

# Time Complexity:
T(m, n) = O(m + n)

# Space Complexity:
S(m, n) = O(1)

