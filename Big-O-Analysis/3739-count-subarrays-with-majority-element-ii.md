# 3739. Count Subarrays With Majority Element II

# Problem Summary:
You are given an integer array nums and an integer target. Return the number of subarrays in which target is the majority element. An element is considered the majority element of a subarray if it appears strictly more than half of the total number of elements in that subarray.

# Approach Used:
The solution uses a prefix balance technique together with a Fenwick Tree (Binary Indexed Tree) to count valid subarrays efficiently.

For each element:
* Add 1 to the balance if the element equals target.
* Subtract 1 otherwise.

For any subarray:
* Let t be the number of occurrences of target.
* Let o be the number of all other elements.
* balance = t - o

The target is the majority element if: t > o, or equivalently: balance > 0
The algorithm maintains the prefix balance while traversing the array. Suppose the current prefix balance is currBalance and a previous prefix balance is prevBalance. 
The balance of the subarray between them is: currBalance - prevBalance
This subarray is valid when: currBalance - prevBalance > 0 or prevBalance < currBalance

Therefore, for every position, the problem reduces to counting how many previous prefix balances are smaller than the current balance. A Fenwick Tree efficiently maintains the frequencies of previous balances and supports:
* Updating a balance frequency.
* Querying how many balances are smaller than the current balance.

Since balances can be negative, an offset is added before storing them in the Fenwick Tree.

# Steps:
1. Create a Fenwick Tree large enough to store all possible balance values.
2. Initialize the balance to 0.
3. Insert the initial balance into the Fenwick Tree.
4. Traverse the array:
   * Increase the balance by 1 if the current element equals target; otherwise decrease it by 1.
   * Query the Fenwick Tree to count previous balances smaller than the current balance.
   * Add this count to the answer.
   * Insert the current balance into the Fenwick Tree.
5. Return the total number of valid subarrays.

# Solution:

```
class Solution {
    public long countMajoritySubarrays(int[] nums, int target) {
        int n = nums.length;                                      // T(n) = O(1)
        int fenwickSize = 2 * n + 5;                              // T(n) = O(1)
        int[] fenwick = new int[fenwickSize];                     // T(n) = O(1), S(n) = O(n)
        int offset = n + 2;                                       // T(n) = O(1)
        int balance = 0;                                          // T(n) = O(1), S(n) = O(1)
        long result = 0;                                          // T(n) = O(1), S(n) = O(1)

        update(fenwick, offset, 1);                               // T(n) = O(log n)

        for(int num : nums) {
            balance += (num == target) ? 1 : -1;                  // T(n) = O(1)
            result += prefixSum(fenwick, offset + balance - 1);   // T(n) = O(log n)
            update(fenwick, offset + balance, 1);                 // T(n) = O(log n)
        }
        
        return result;                                            // T(n) = O(1)
    }

    private void update(int[] fenwick, int index, int value) {
    
        while(index < fenwick.length) {
            fenwick[index] += value;                              // T(n) = O(1) per iteration
            index += index & -index;                              // T(n) = O(1) per iteration
        }
    }

    private int prefixSum(int[] fenwick, int index) {
        int sum = 0;                                              // T(n) = O(1)

        while(index > 0) {
            sum += fenwick[index];                                // T(n) = O(1) per iteration
            index -= index & -index;                              // T(n) = O(1) per iteration
        }
        
        return sum;                                               // T(n) = O(1)
    }
}
```

# Time Complexity:
T(n) = O(n log n)

# Space Complexity:
S(n) = O(n)
