# 3501. Maximize Active Section with Trade II

# Problem Summary:
You are given a binary string s, where:
* '1' represents an active section.
* '0' represents an inactive section.

You may perform at most one trade consisting of:
1. Converting a contiguous block of '1's that is surrounded by '0's into '0's.
2. Then converting a contiguous block of '0's that is surrounded by '1's into '1's.

You are also given multiple queries, where each query specifies a substring s[l...r]. For each query, consider only that substring (augmented with a '1' at both ends for determining surrounded groups) and return the maximum possible number of active sections after performing the optimal trade.

# Approach Used:
Instead of processing every query independently, the solution preprocesses the original string. It first groups consecutive equal characters together, storing each group's digit, length, and starting index.
Using these groups, two helper arrays are constructed:
1. arr1 stores the maximum gain obtainable when activating a particular zero group after removing the adjacent one group.
2. arr2 stores, for every position, the index of the next group containing '1'.

A Sparse Table is then built over arr1, allowing maximum-range queries to be answered in constant time. For every query, the algorithm identifies all valid trades inside the requested substring and computes the maximum possible increase in active sections.

# Steps:
1. Divide the string into groups of consecutive 0s and 1s.
2. Build arr1, storing the gain obtained from activating each eligible zero group.
3. Build arr2, storing the next one-group for every position.
4. Build a Sparse Table over arr1.
5. For every query:
   * Handle the case where the substring begins inside a zero group.
   * Locate candidate one-groups.
   * Use the Sparse Table to find the maximum gain.
   * Return the original number of active sections plus the best gain.
   
# Solution:

```
class Solution {
    static record Group(int digit, int count, int startIdx){};

    public List<Integer> maxActiveSectionsAfterTrade(String s, int[][] queries) {
        char[] str = s.toCharArray();                                    // T(n) = O(n), S(n) = O(n)
        final int n = str.length;                                        // T(n) = O(1), S(n) = O(1)
        int arr1[] = new int[n];                                         // T(n) = O(n), S(n) = O(n)
        int prev = str[0] - '0';                                         // T(n) = O(1), S(n) = O(1)
        int cnt = 1;                                                     // T(n) = O(1), S(n) = O(1)
        int total = str[0] - '0';                                        // T(n) = O(1), S(n) = O(1)
        int ind = 0;                                                     // T(n) = O(1), S(n) = O(1)
        ArrayList<Group> groups = new ArrayList<>();                     // T(n) = O(1), S(n) = O(n)

        for(int i = 1; i < n; i++){
            int digit = str[i] - '0';                                    // T(n) = O(1)
            total += digit;                                              // T(n) = O(1)

            if(prev != digit){
                groups.add(new Group(prev, cnt, ind));                   // T(n) = O(1)
                cnt = 1;                                                 // T(n) = O(1)
                ind = i;                                                 // T(n) = O(1)
                prev = digit;                                            // T(n) = O(1)
            }else {
                cnt++;                                                   // T(n) = O(1)
            }
        }

        groups.add(new Group(prev, cnt, ind));                           // T(n) = O(1)
        final int totalGroups = groups.size();                           // T(n) = O(1), S(n) = O(1)

        for(int i = 1; i < totalGroups - 1; i++){
            if(groups.get(i).digit == 1){
                Group currG = groups.get(i);                             // T(n) = O(1), S(n) = O(1)
                Group prevG = groups.get(i - 1);                         // T(n) = O(1), S(n) = O(1)
                Group nextG = groups.get(i + 1);                         // T(n) = O(1), S(n) = O(1)

                for(int j = 0; j < nextG.count; j++){
                    arr1[nextG.startIdx + j] = prevG.count + j + 1;      // T(n) = O(1)
                }
            }
        }

        int next = n;                                                    // T(n) = O(1), S(n) = O(1)
        int arr2[] = new int[n];                                         // T(n) = O(n), S(n) = O(n)
        ind = totalGroups - 1;                                           // T(n) = O(1)

        for(int i = n - 1; i >= 0; i--){
            arr2[i] = next;                                              // T(n) = O(1)

            if(groups.get(ind).startIdx == i){
                if(str[i] == '1') {
                    next = ind;                                          // T(n) = O(1)
                }
                ind--;                                                   // T(n) = O(1)
            }
        }

        int sp[][] = buildSparseTable(arr1);                             // T(n) = O(n log n), S(n) = O(n log n)
        List<Integer> ans = new ArrayList<>();                           // T(n) = O(1), S(n) = O(1)
        
	      for(int i = 0; i < queries.length; i++){
	          int max = Integer.MIN_VALUE;                                 // T(n) = O(1), S(n) = O(1)
	          int L = queries[i][0];                                       // T(n) = O(1), S(n) = O(1)
	          int R = queries[i][1];                                       // T(n) = O(1), S(n) = O(1)

	          if(str[L] == '0' && arr2[L] < totalGroups - 1 
	    	      && groups.get(arr2[L] + 1).startIdx <= R){
	    	
		            Group nextZeroGp = groups.get(arr2[L] + 1);              // T(n) = O(1), S(n) = O(1)
		            Group nextOneGp = groups.get(arr2[L]);                   // T(n) = O(1), S(n) = O(1)

		            max = Math.min(R + 1, nextZeroGp.startIdx + nextZeroGp.count)
		                - L - nextOneGp.count;                               // T(n) = O(1)
	          }
	
	          if(arr2[L] != n) {
		            Group nextOneGp = groups.get(arr2[L]);                   // T(n) = O(1), S(n) = O(1)
		            int nextOneGpIdx = arr2[nextOneGp.startIdx];             // T(n) = O(1), S(n) = O(1)

    		        if(str[L] == '1' && nextOneGp.startIdx <= R) {
    		            max = Math.max(max,
    				               query(nextOneGp.startIdx, R, sp));            // T(n) = O(1)
    		        }else if(nextOneGpIdx != n){
    		            nextOneGp = groups.get(nextOneGpIdx);                // T(n) = O(1)
    
    		            if(nextOneGp.startIdx <= R) {
    			              max = Math.max(max,
    				                   query(nextOneGp.startIdx, R, sp));        // T(n) = O(1)
    		            }
    		        }
	          }
	          ans.add(max == Integer.MIN_VALUE ? total : (max + total));   // T(n) = O(1)
	      }
	      return ans;                                                      // T(n) = O(1), S(n) = O(1)
    }

    public static int[][] buildSparseTable(int[] arr) {
	      final int n = arr.length;                                        // T(n) = O(1), S(n) = O(1)
	      final int m = (int)(Math.log(n) / Math.log(2)) + 1;              // T(n) = O(1), S(n) = O(1)
	      int[][] lookup = new int[n][m];                                  // T(n) = O(n log n), S(n) = O(n log n)

	      for(int i = 0; i < n; i++) {
	          lookup[i][0] = arr[i];                                       // T(n) = O(1)
	      }

	      for(int j = 1; j <= m; j++) {
	          int curr = 1 << j;                                           // T(n) = O(1), S(n) = O(1)
	          int prev = 1 << j - 1;                                       // T(n) = O(1), S(n) = O(1)

    	      for(int i = 0; (i + curr - 1) < n; i++) {
    		        if(lookup[i][j - 1] > lookup[i + prev][j - 1]) {
    		            lookup[i][j] = lookup[i][j - 1];                     // T(n) = O(1)
    		        }else {
    		            lookup[i][j] = lookup[i + prev][j - 1];              // T(n) = O(1)
    		        }
    	      }
	      }
	      return lookup;                                                   // T(n) = O(1), S(n) = O(1)
    }

    public static int query(int L, int R, int[][] lookup) {
	      int j = (int)(Math.log(R - L + 1) / Math.log(2));                // T(n) = O(1), S(n) = O(1)

	      if(lookup[L][j] >= lookup[R - (1 << j) + 1][j]) {
	          return lookup[L][j];                                         // T(n) = O(1)
	      }else {
	          return lookup[R - (1 << j) + 1][j];                          // T(n) = O(1)
	      }
    }
}
```

# Time Complexity:
T(n) = O(n * (log n) + q), where n = length of the string, and q = number of queries

# Space Complexity:
S(n) = O(n * (log n))
