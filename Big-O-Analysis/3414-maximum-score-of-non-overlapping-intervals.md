# 3414. Maximum Score of Non-overlapping Intervals

# Problem Summary

You are given intervals of the form [li, ri, weighti]. You can choose at most 4 non-overlapping intervals, where intervals sharing a boundary are considered overlapping. Return the lexicographically smallest array of original interval indices among all choices having the maximum total weight.

# Approach Used

The solution uses:
1. Sorting intervals by start position and then end position.
2. Binary search to compute next[i], the first interval that starts strictly after interval i ends.
3. Dynamic programming with at most 4 selected intervals.
4. State objects storing both the maximum score and the corresponding sorted original indices.
5. Lexicographic comparison when two choices have equal scores.

The DP state is: dp[c][i], which represents the best result starting from sorted interval i when at most c intervals can still be selected.

For each interval, we either:
* Skip it: dp[c][i + 1]
* Take it: add its weight and continue from next[i] with one fewer available selection.

Because at most 4 intervals can be selected, both lexSmaller() and insertSorted() operate on arrays of constant maximum length.

# Steps

1. Copy each input interval into arr together with its original index.
2. Sort arr by start position and then end position.
3. For each interval, use binary search to find the first interval whose start is greater than its end.
4. Initialize the DP table for the position after the last interval.
5. Process intervals from right to left.
6. For each possible count from 1 to 4:
   * Compute the result if the interval is skipped.
   * Compute the result if the interval is taken.
   * Choose the higher score.
   * If scores are equal, choose the lexicographically smaller index array.
7. Return dp[4][0].ids.

# Solution

```
class Solution {
    static class State {
        long score;
        int[] ids;

        State(long score, int[] ids) {
            this.score = score;                                              // T(n) = O(1), S(n) = O(1)
            this.ids = ids;                                                  // T(n) = O(1), S(n) = O(1)
        }
    }

    public int[] maximumWeight(List<List<Integer>> intervals) {
        final int n = intervals.size();                                      // T(n) = O(1), S(n) = O(1)
        long[][] arr = new long[n][4];                                       // T(n) = O(n), S(n) = O(n)

        for(int i = 0; i < n; i++) {                                         // T(n) = O(n)
            arr[i][0] = intervals.get(i).get(0);                             // T(n) = O(1), S(n) = O(1)
            arr[i][1] = intervals.get(i).get(1);                             // T(n) = O(1), S(n) = O(1)
            arr[i][2] = intervals.get(i).get(2);                             // T(n) = O(1), S(n) = O(1)
            arr[i][3] = i;                                                   // T(n) = O(1), S(n) = O(1)
        }

        Arrays.sort(arr, (a, b) -> {                                         // T(n) = O(n log n)
            if(a[0] != b[0]) {
                return Long.compare(a[0], b[0]);                             // T(n) = O(1), S(n) = O(1)
            }
            return Long.compare(a[1], b[1]);                                 // T(n) = O(1), S(n) = O(1)
        });

        int[] next = new int[n];                                             // T(n) = O(n), S(n) = O(n)

        for(int i = 0; i < n; i++) {                                         // T(n) = O(n log n)
            int lo = i + 1;                                                  // T(n) = O(1), S(n) = O(1)
            int hi = n;                                                      // T(n) = O(1), S(n) = O(1)

            while (lo < hi) {                                                // T(n) = O(log n) per i
                int mid = lo + (hi - lo) / 2;                                // T(n) = O(1), S(n) = O(1)

                if (arr[mid][0] > arr[i][1]) {                               // T(n) = O(1), S(n) = O(1)
                    hi = mid;                                                // T(n) = O(1), S(n) = O(1)
                } else {
                    lo = mid + 1;                                            // T(n) = O(1), S(n) = O(1)
                }
            }
            next[i] = lo;                                                    // T(n) = O(1), S(n) = O(1)
        }

        State[][] dp = new State[5][n + 1];                                  // T(n) = O(n), S(n) = O(n)

        for(int c = 0; c <= 4; c++) {                                        // T(n) = O(1)
            dp[c][n] = new State(0, new int[0]);                             // T(n) = O(1), S(n) = O(1)
        }

        for(int i = n - 1; i >= 0; i--) {                                    // T(n) = O(n)
            dp[0][i] = new State(0, new int[0]);                             // T(n) = O(1), S(n) = O(1)

            for(int c = 1; c <= 4; c++) {                                    // T(n) = O(1) per i
                State skip = dp[c][i + 1];                                   // T(n) = O(1), S(n) = O(1)
                State rest = dp[c - 1][next[i]];                             // T(n) = O(1), S(n) = O(1)

                long takeScore = arr[i][2] + rest.score;
                                                                             // T(n) = O(1), S(n) = O(1)

                int[] takeIds = insertSorted(
                    rest.ids,                                                // Array length is at most 3
                    (int) arr[i][3]
                );                                                           // T(n) = O(1), S(n) = O(1)

                State take = new State(takeScore, takeIds);                  // T(n) = O(1), S(n) = O(1)
                dp[c][i] = better(skip, take);                               // T(n) = O(1), S(n) = O(1)
            }
        }
        return dp[4][0].ids;                                                 // T(n) = O(1), S(n) = O(1)
    }

    private State better(State a, State b) {
        if(a.score > b.score) {                                              // T(n) = O(1), S(n) = O(1)
            return a;                                                        // T(n) = O(1), S(n) = O(1)
        }

        if(b.score > a.score) {                                              // T(n) = O(1), S(n) = O(1)
            return b;                                                        // T(n) = O(1), S(n) = O(1)
        }

        if(lexSmaller(a.ids, b.ids)) {                                       // T(n) = O(1), S(n) = O(1)
            return a;                                                        // T(n) = O(1), S(n) = O(1)
        }
        return b;                                                            // T(n) = O(1), S(n) = O(1)
    }

    private boolean lexSmaller(int[] a, int[] b) {
        int len = Math.min(a.length, b.length);                              // T(n) = O(1), S(n) = O(1)

        for(int i = 0; i < len; i++) {                                       // T(n) = O(1)
            if(a[i] != b[i]) {                                               // T(n) = O(1), S(n) = O(1)
                return a[i] < b[i];                                          // T(n) = O(1), S(n) = O(1)
            }
        }
        return a.length < b.length;                                          // T(n) = O(1), S(n) = O(1)
    }

    private int[] insertSorted(int[] arr, int value) {
        int[] result = new int[arr.length + 1];                              // T(n) = O(1), S(n) = O(1)
        int i = 0;                                                           // T(n) = O(1), S(n) = O(1)

        while(i < arr.length && arr[i] < value) {                            // T(n) = O(1), max length is 3
            result[i] = arr[i];                                              // T(n) = O(1), S(n) = O(1)
            i++;                                                             // T(n) = O(1), S(n) = O(1)
        }

        result[i] = value;                                                   // T(n) = O(1), S(n) = O(1)

        while(i < arr.length) {                                              // T(n) = O(1), max length is 3
            result[i + 1] = arr[i];                                          // T(n) = O(1), S(n) = O(1)
            i++;                                                             // T(n) = O(1), S(n) = O(1)
        }
        return result;                                                       // T(n) = O(1), S(n) = O(1)
    }
}
```

# Time Complexity:
T(n) = O(n log n)

# Space Complexity:
S(n) = O(n) 
