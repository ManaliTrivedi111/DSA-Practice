# 3551. Minimum Swaps to Sort by Digit Sum

# Problem Summary:

Given an array nums of distinct positive integers, sort it by digit sum in ascending order. If two numbers have the same digit sum, place the smaller number first.

Return the minimum number of swaps required to rearrange the original array into this sorted order.

# Approach Used:

Create an Element object for each number, storing the number, its digit sum, and its original index.

Sort these objects by digit sum, using the number as the tie-breaker. After sorting, each element's position is its target position.

Build a pos array where pos[i] stores the current position of the element that originally appeared at index i.

Traverse the sorted array from left to right. If the element at index i is not the one that originally appeared at index i, use pos[i] to locate the element that belongs at index i, then swap them. Update both elements' positions in pos and increment the swap count.

Each swap places the correct element at the current index, so the algorithm fixes positions from left to right using the minimum number of swaps.

# Steps:

1. Create an Element object for every number, storing its value, digit sum, and original index.
2. Sort the objects by digit sum, then by numeric value.
3. Create a pos array to track the current position of each element by its original index.
4. Iterate through the sorted array:
   - If the element at index i is already in its original position, continue.
   - Otherwise, use pos[i] to find the current position of the element that belongs at index i.
   - Swap the two elements.
   - Update their positions in pos and increment swaps.
5. Return the total number of swaps.

# Solution:

```
import java.util.*;

class Solution {

    class Element {
        int number;                                                          // T(n) = O(1), S(n) = O(1)
        int digitSum;                                                        // T(n) = O(1), S(n) = O(1)
        int originalIndex;                                                   // T(n) = O(1), S(n) = O(1)

        Element(int number, int digitSum, int originalIndex) {
            this.number = number;                                            // T(n) = O(1)
            this.digitSum = digitSum;                                        // T(n) = O(1)
            this.originalIndex = originalIndex;                              // T(n) = O(1)
        }
    }

    public int sumDigit(int num) {
        int sum = 0;                                                         // T(n) = O(1), S(n) = O(1)

        while(num > 0) {                                                     
            sum += num % 10;                                                 // T(n) = O(1)
            num /= 10;                                                       // T(n) = O(1)
        }
        return sum;                                                          // T(n) = O(1)
    }

    public int minSwaps(int[] nums) {
        final int n = nums.length;                                           // T(n) = O(1), S(n) = O(1)
        Element[] arr = new Element[n];                                      // T(n) = O(n), S(n) = O(n)

        for(int i = 0; i < n; i++) {                                         // T(n) = O(n)
            arr[i] = new Element(nums[i], sumDigit(nums[i]), i);             // T(n) = O(1)
        }

        Arrays.sort(arr, (a, b) -> {                                         // T(n) = O(n * log n)
            if(a.digitSum != b.digitSum) {                                   // T(n) = O(1) per comparison
                return Integer.compare(a.digitSum, b.digitSum);              // T(n) = O(1)
            }
            return Integer.compare(a.number, b.number);                      // T(n) = O(1)
        });

        int[] pos = new int[n];                                              // T(n) = O(n), S(n) = O(n)

        for(int i = 0; i < n; i++) {                                         // T(n) = O(n)
            pos[arr[i].originalIndex] = i;                                   // T(n) = O(1)
        }

        int swaps = 0;                                                       // T(n) = O(1), S(n) = O(1)

        for(int i = 0; i < n; i++) {                                         // T(n) = O(n) overall
            if(arr[i].originalIndex == i) {                                  // T(n) = O(1)
                continue;
            }

            int correctPosition = pos[i];                                    // T(n) = O(1), S(n) = O(1)
            Element temp = arr[i];                                           // T(n) = O(1), S(n) = O(1)

            arr[i] = arr[correctPosition];                                   // T(n) = O(1)
            arr[correctPosition] = temp;                                     // T(n) = O(1)
            pos[arr[correctPosition].originalIndex] = correctPosition;       // T(n) = O(1)
            pos[arr[i].originalIndex] = i;                                   // T(n) = O(1)
            swaps++;                                                         // T(n) = O(1)
        }
        return swaps;                                                        // T(n) = O(1)
    }
}
```

# Time Complexity:
T(n) = O(n * log n)

# Space Complexity:
S(n) = O(n)
