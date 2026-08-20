# 3731. Find Missing Elements

# Problem Summary:

You are given an integer array nums containing unique integers. Originally, the array contained every integer within a certain range, but some integers may now be missing. The smallest and largest integers from the original range are still present in nums.

Return a sorted list containing all the missing integers between the smallest and largest values. If no integers are missing, return an empty list.

# Approach Used:

The solution uses a boolean array to keep track of which integers are present in nums. Since the values in nums are within a fixed range of 0 to 100, we can create a boolean array of size 101.

For every number in nums, we:
1. Mark its corresponding position in isPresent as true.
2. Update minNum to store the smallest number.
3. Update maxNum to store the largest number.

After that, we iterate from minNum to maxNum.

If isPresent[i] is false, then i is missing from the original range, so we add it to ans.

Because we check the numbers from smallest to largest, the resulting list is automatically sorted.

# Steps:

1. Create a boolean array isPresent of size 101.
2. Create an empty list ans to store the missing numbers.
3. Initialize minNum to 101 and maxNum to 0.
4. Traverse the nums array.
5. Mark every number as present in isPresent.
6. Update the minimum and maximum values.
7. Iterate from minNum to maxNum.
8. If a number is not marked as present, add it to ans.
9. Return ans.

# Solution:

```
class Solution {
    public List<Integer> findMissingElements(int[] nums) {
        boolean[] isPresent = new boolean[101];                              // T(n) = O(1), S(n) = O(1)
        List<Integer> ans = new ArrayList<>();                               // T(n) = O(1), S(n) = O(1)
        int minNum = 101;                                                    // T(n) = O(1), S(n) = O(1)
        int maxNum = 0;                                                      // T(n) = O(1), S(n) = O(1)

        for(int num : nums) {
            isPresent[num] = true;                                           // T(n) = O(1), S(n) = O(1)
            minNum = Math.min(minNum, num);                                  // T(n) = O(1), S(n) = O(1)
            maxNum = Math.max(maxNum, num);                                  // T(n) = O(1), S(n) = O(1)
        }
        
        for(int i = minNum; i <= maxNum; i++) {
            if(!isPresent[i]) {
                ans.add(i);                                                  // T(n) = O(1), S(n) = O(1)
            }
        }
        return ans;                                                          // T(n) = O(1), S(n) = O(1)
    }
}
```

# Time Complexity:
T(n) = O(n)

# Space Complexity:
S(n) = O(1)
