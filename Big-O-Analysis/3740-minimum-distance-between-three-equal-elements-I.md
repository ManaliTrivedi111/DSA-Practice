# 3740. Minimum Distance Between Three Equal Elements I

# Problem Summary: 
You are given an integer array nums.
A tuple (i, j, k) of 3 distinct indices is good if nums[i] == nums[j] == nums[k].
The distance of a good tuple is abs(i - j) + abs(j - k) + abs(k - i), where abs(x) denotes the absolute value of x.
Return an integer denoting the minimum possible distance of a good tuple. If no good tuples exist, return -1.

# Approach Used: 
The solution keeps track of the first two occurrences and total count of each number using arrays. When a number appears for the third time (or later), the distance is calculated using the earliest valid triplet window and the answer os updated if it is smaller than the previous answer. The first and second occurrence indices are then shifted forward to continue checking future triplets efficiently.
This allows solving the problem in linear time.

# Steps:
1) Store the length of the array
2) Create three arrays to track first occurrence, second occurrence, and frequency count
3) Traverse the array once
4) If the number appears first time -> store index in first[]
5) If the number appears second time -> store index in second[]
6) If the number appears third time or more:
   Calculate distance
   Update answer if smaller
   Shift indices forward
7) Return the minimum distance found, otherwise return -1

# Solution:
```
class Solution {

    public int minimumDistance(int[] nums) {
        final int n = nums.length;                               // T(n) = O(1), S(n) = O(1)                   
        int[] first = new int[n + 1];                            // T(n) = O(n), S(n) = O(n)
        int[] second = new int[n + 1];                           // T(n) = O(n), S(n) = O(n)
        int[] count = new int[n + 1];                            // T(n) = O(n), S(n) = O(n)
        int answer = Integer.MAX_VALUE;                          // T(n) = O(1), S(n) = O(1)

	//loop runs n times
        for(int i = 0; i < n; i++) {
            final int val = nums[i];                             // T(n) = O(1) per iteration = O(n), S(n) = O(1)
            count[val]++;                                        // T(n) = O(1) per iteration = O(n)

	    //block with the maximum T(n) will be counted
            if(count[val] == 1) {                                // T(n) = O(1) per iteration = O(n)
                first[val] = i;                                  // T(n) = O(1) per iteration = O(n)
            }else if(count[val] == 2) {                          // T(n) = O(1) per iteration = O(n)
                second[val] = i;                                 // T(n) = O(1) per iteration = O(n)
            }else{                                       
                answer = Math.min(answer, (i - first[val]) * 2); // T(n) = O(1) per iteration = O(n)
                first[val] = second[val];                        // T(n) = O(1) per iteration = O(n)
                second[val] = i;                                 // T(n) = O(1) per iteration = O(n)
            }
        }
        return answer == Integer.MAX_VALUE ? -1 : answer;       // T(n) = O(1)
    }
}
```

# Time Complexity:
T(n) = O(n)
     
# Space Complexity:
S(n) = O(n)
