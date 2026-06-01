# 2615. Sum of Distances

# Problem Summary: 
You are given a 0-indexed integer array nums. There exists an array arr of length nums.length, where arr[i] is the sum of |i - j| over all j such that nums[j] == nums[i] and j != i. If there is no such j, set arr[i] to be 0.
Return the array arr.

# Approach One: 
The idea is to group all indices having the same value in nums. For each group, we compute the distance of every index using prefix sums by separating contributions from indices on the left side and right side. Initially all indices are considered part of the right side and their sum is stored in rightSum, while leftSum starts from 0. As we iterate through the indices from left to right, the current index is moved from the right side to the left. For distance calculation, the current index is multiplied by the number of indices on the left and the leftSum value is subtracted from it to calculate the total contribution from the left side. Similarly, the current index is multiplied by the number of indices on the right and is subtracted from the rightSum value to compute the contribution from the right side. The sum of these two contributions gives the total distance for the current index. 

# Steps:
1) Store the length of the array
2) Create a HashMap to group indices having the same value
3) Create the answer array to return
4) Traverse the array and store each index in the list corresponding to its value in the map
5) For each list of indices in the map:
   -> Initialzie count = total number of indices
   -> Initialize leftSum = 0
   -> Calculate rightSum = sum of all indices in the group
6) Traverse the indices in the list from left to right
7) For each index:
   -> Subtract the current index from the rightSum value
   -> Calculate contribution from the left side = (index × (number of indices on the left = i)) − leftSum
   -> Calculate contribution from the right side = rightSum - (index x (number of indices on the right = count - i - 1))
   -> Add both contributions and store the result into the answer array at the current index position
   -> Add the current index to the leftSum value
8) Return the answer array

# Solution One: 
class Solution {

    public long[] distance(int[] nums) {
        final int n = nums.length;                                // T(n) = O(1), S(n) = O(1)
        Map<Integer, List<Integer>> valIdxMap = new HashMap<>();  // T(n) = O(1), S(n) = O(n)
        long[] answer = new long[n];                              // T(n) = O(n), S(n) = O(n)

        // loop runs n times
        for(int i = 0; i < n; i++) {
            if(!valIdxMap.containsKey(nums[i])) {                 // T(n) = O(1) per iteration = O(n)
                valIdxMap.put(nums[i], new ArrayList<>());        // T(n) = O(1) per iteration = O(n), S(n) = O(n) total
            }
            valIdxMap.get(nums[i]).add(i);                        // T(n) = O(1) per iteration = O(n)
        }

        // outer loop runs k times (k = number of unique values)
        // inner loops together process total n elements
        for(List<Integer> list : valIdxMap.values()) {

            final int count = list.size();                        // T(n) = O(1) per group = O(k), S(n) = O(1)
            long leftSum = 0L;                                    // T(n) = O(1) per group = O(k), S(n) = O(1)
            long rightSum = 0L;                                   // T(n) = O(1) per group = O(k), S(n) = O(1)

            // loop runs count times per group
            // total across all groups = n
            for(int i = 0; i < count; i++) {
                rightSum += list.get(i);                          // T(n) = O(1) per iteration = O(n)
            }

            // loop runs count times per group
            // total across all groups = n
            for(int i = 0; i < count; i++) {
                int index = list.get(i);                          // T(n) = O(1) per iteration = O(n), S(n) = O(1)

                rightSum -= index;                                // T(n) = O(1) per iteration = O(n)

                answer[index] =
                (((long)i * index) - leftSum)
                + (rightSum - ((long)(count - i - 1) * index));   // T(n) = O(1) per iteration = O(n)

                leftSum += index;                                 // T(n) = O(1) per iteration = O(n)
            }
        }

        return answer;                                            // T(n) = O(1)
    }
}

# Time Complexity:
T(n) = O(1) + O(1) + O(n) + O(n) + O(n) + O(n) + O(k) + O(k) + O(k) + O(n) + O(n) + O(n) + O(n) + O(n) + O(1)
     = O(3 + 3k + 9n)
     = O(3k + 9n) // Ignore smaller constant term
     = O(9n) // Ignore smaller term since k <= n
     = O(n) // Drop constant multiplier
     
# Space Complexity:
S(n) = O(1) + O(n) + O(n) + O(n) + O(1) + O(1) + O(1) + O(1)
     = O(5 + 3n)
     = O(3n) // Ignore smaller constant term
     = O(n) // Drop constant multiplier

# Approach Two: 
The solution uses a hashmap where each number stores a helper object containing leftSum, rightSum, leftCount, and rightCount. Initially, all occurrences of a number are treated as being on the right side, so their indices are added to the rightSum and their count is stored in the rightCount. Then the array is traversed from left to right. For each index, the total distance is calculated using contributions from already processed indices on the left side and remaining indices on the right side. After calculating the answer for the current index, it is moved from the right side to the left side by updating the sums and counts. 

# Steps:
1) Store the length of the array
2) Create a helper class to store:
   leftSum
   rightSum
   leftCount
   rightCount
3) Create a hashmap to store helper objects for each distinct number
4) Create the answer array of size n
5) Traverse the array once
6) For each index:
   If number is not present in hashmap -> create helper object
7) Add current index to rightSum and increase rightCount by 1
8) Traverse the array again from left to right
9) For each index:
   Get helper object of current number
   Calculate distance using: (rightSum - leftSum) + (index × leftCount) - (index × rightCount)
10) Store result in answer array
11) Move current index from right side to left side:
    leftSum += index
    rightSum -= index
    leftCount++
    rightCount--
12) Return answer array

# Solution:
class Solution {

    private class Data {
        long leftSum, rightSum;                                  // T(n) = O(1), S(n) = O(1) per object
        int leftCount, rightCount;                               // T(n) = O(1), S(n) = O(1) per object
    }

    public long[] distance(int[] nums) {

        final int n = nums.length;                               // T(n) = O(1), S(n) = O(1)
        Map<Integer, Data> dataMap = new HashMap<>();            // T(n) = O(1), S(n) = O(1)
        long[] answer = new long[n];                             // T(n) = O(n), S(n) = O(n)

        // loop runs n times
        for(int i = 0; i < n; i++) {
        
            if(!dataMap.containsKey(nums[i])) {                  // T(n) = O(1) per iteration = O(n)
                dataMap.put(nums[i], new Data());                // T(n) = O(1) per iteration = O(k), S(n) = O(k) 
            }

            Data d = dataMap.get(nums[i]);                       // T(n) = O(1) per iteration = O(n), S(n) = O(1)
            d.rightSum += i;                                     // T(n) = O(1) per iteration = O(n)
            d.rightCount++;                                      // T(n) = O(1) per iteration = O(n)
        }

        // loop runs n times
        for(int i = 0; i < n; i++) {

            Data d = dataMap.get(nums[i]);                       // T(n) = O(1) per iteration = O(n), S(n) = O(1)

            answer[i] = (d.rightSum - d.leftSum) 
                        + ((long)i * d.leftCount) 
                        - ((long)i * d.rightCount);              // T(n) = O(1) per iteration = O(n)

            d.leftSum += i;                                      // T(n) = O(1) per iteration = O(n)
            d.rightSum -= i;                                     // T(n) = O(1) per iteration = O(n)
            d.leftCount++;                                       // T(n) = O(1) per iteration = O(n)
            d.rightCount--;                                      // T(n) = O(1) per iteration = O(n)
        }
        return answer;                                           // T(n) = O(1)
    }
}

# Time Complexity:
T(n) = O(1) + O(1) + O(1) + O(1) + O(n) + O(n) + O(k) + O(n) + O(n) + O(n) + O(n) + O(n) + O(n) + O(n) + O(n) + O(n) + O(1)
     = O(5 + k + 11n)
     = O(k + 11n) // Ignore smaller constant term
     = O(12n) // Worst case: k = n
     = O(n) // Drop constant multiplier

# Space Complexity:
S(n) = O(1) + O(1) + O(1) + O(1) + O(n) + O(k) + O(1) + O(1)
     = O(6 + k + n)
     = O(6 + 2n) // Worst case: k = n
     = O(2n) // Ignore smaller constant term
     = O(n) // Drop constant multiplier
