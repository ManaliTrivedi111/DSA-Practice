# 1331. Rank Transform of an Array

# Problem Summary:
You are given an integer array arr. Replace every element with its rank, where:
* Rank starts from 1.
* Larger values receive larger ranks.
* Equal values receive the same rank.
* The assigned ranks should be as small as possible.

Return the transformed array.

# Approach Used:
The solution uses sorting and a HashMap. First, a copy of the original array is created and sorted. The sorted array is traversed to assign ranks:
* If a value has not been assigned a rank yet, store its rank in the HashMap.
* Duplicate values receive the same rank because they are already present in the map.

Finally, traverse the original array and replace every element with its corresponding rank from the HashMap.

# Steps:
1. If the array is empty, return it.
2. Create a copy of the original array.
3. Sort the copied array.
4. Traverse the sorted array:
   * Assign a new rank to each distinct value.
   * Store the mapping in a HashMap.
5. Traverse the original array and replace each value with its rank.
6. Return the transformed array.

# Solution:

```
class Solution {

    public int[] arrayRankTransform(int[] arr) {
        final int n = arr.length;                               // T(n) = O(1)

        if(n == 0) {
            return arr;                                         // T(n) = O(1)
        }

        int[] temp = arr.clone();                               // T(n) = O(n), S(n) = O(n)
        Arrays.sort(temp);                                      // T(n) = O(n log n)
        Map<Integer, Integer> map = new HashMap<>();            // T(n) = O(1), S(n) = O(n)
        int rank = 1;                                           // T(n) = O(1)

        for(int i = 0; i < n; i++) {

            if(!map.containsKey(temp[i])) {
                map.put(temp[i], rank);                         // T(n) = O(1)
                rank++;                                         // T(n) = O(1)
            }
        }

        for(int i = 0; i < n; i++) {
            arr[i] = map.get(arr[i]);                           // T(n) = O(1)
        }
        return arr;                                             // T(n) = O(1)
    }
}
```

# Time Complexity:
T(n) = O(n log n)

# Space Complexity:
S(n) = O(n)
