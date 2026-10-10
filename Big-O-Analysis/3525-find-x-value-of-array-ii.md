# 3525. Find X Value of Array II

# Problem Summary

Given an array nums, a positive integer k, and a list of queries, each query:
1. Updates one element of nums.
2. Removes the prefix before a given start index.
3. Counts how many non-empty remaining prefixes have a product whose remainder modulo k is equal to a given x.

The update persists for subsequent queries.

# Approach Used

The solution uses a segment tree. Each segment stores the number of prefixes having each possible product remainder modulo k, along with the product remainder of the entire segment.

The segment tree stores information for every interval:
* tree[o][0 ... k - 1] stores the number of non-empty prefixes of the interval whose product has each remainder modulo k.
* tree[o][k] stores the remainder of the product of all elements in the interval.
* makeLeaf() initializes the information for a single element.
* mergePre() combines two adjacent segments. All prefixes of the merged segment are either:
  * prefixes entirely inside the left segment, or
  * the entire left segment followed by a prefix of the right segment.
* Point updates modify one leaf and then recompute its ancestors.
* Range queries retrieve the information for nums[start ... n - 1].
* The answer for a query is the count stored at remainder x.

Because MAXK is fixed at 6, every merge operation processes a constant number of remainder states.

# Steps

1. Build a segment tree from nums.
2. For each leaf:
   * Calculate value % k.
   * Set the count for that remainder to 1.
   * Store the element's remainder as the product remainder of the segment.
3. Build internal nodes by merging their left and right children.
4. For each query:
   * Extract index, value, start, and x.
   * Update nums[index] in the segment tree.
   * Query the range [start, n - 1].
   * Store the count of prefixes whose product has remainder x.
5. Return all query answers.

# Solution

```
class SegmentTree {

    private static final int MAXK = 6;                                       // T(n) = O(1), S(n) = O(1)
    private int k;                                                           // T(n) = O(1), S(n) = O(1)
    private int n;                                                           // T(n) = O(1), S(n) = O(1)
    private int[][] tree;                                                    // T(n) = O(1), S(n) = O(n)

    public SegmentTree(int[] nums, int k) {
        this.k = k;                                                          // T(n) = O(1), S(n) = O(1)
        this.n = nums.length;                                                // T(n) = O(1), S(n) = O(1)
        int size = 2 << Integer.toBinaryString(n).length();                  // T(n) = O(log n), S(n) = O(1)
        tree = new int[size][MAXK];                                          // T(n) = O(n), S(n) = O(n)
        build(nums, 1, 0, n - 1);                                            // T(n) = O(n), S(n) = O(log n)
    }

    private void makeLeaf(int o, int value) {
        Arrays.fill(tree[o], 0);                                             // T(n) = O(1), S(n) = O(1)
        int r = value % k;                                                   // T(n) = O(1), S(n) = O(1)
        tree[o][r] = 1;                                                      // T(n) = O(1), S(n) = O(1)
        tree[o][k] = r;                                                      // T(n) = O(1), S(n) = O(1)
    }

    private void mergePre(int[] left, int[] right, int[] result) {
        int mulL = left[k];                                                  // T(n) = O(1), S(n) = O(1)
        int mulR = right[k];                                                 // T(n) = O(1), S(n) = O(1)
        result[k] = (mulL * mulR) % k;                                       // T(n) = O(1), S(n) = O(1)

        for(int x = 0; x < k; x++) {                                         // T(n) = O(k) = O(1)
            result[x] = left[x];                                             // T(n) = O(1), S(n) = O(1)
        }
        for(int x = 0; x < k; x++) {                                         // T(n) = O(k) = O(1)
            result[(mulL * x) % k] += right[x];                              // T(n) = O(1), S(n) = O(1)
        }
    }

    private void maintain(int o) {
        mergePre(tree[o * 2], tree[o * 2 + 1], tree[o]);                     // T(n) = O(k) = O(1), S(n) = O(1)
    }

    private void build(int[] nums, int o, int l, int r) {
        if(l == r) {                                                         // T(n) = O(1), S(n) = O(1)
            makeLeaf(o, nums[l]);                                            // T(n) = O(1), S(n) = O(1)
            return;                                                          // T(n) = O(1), S(n) = O(1)
        }
        int m = (l + r) / 2;                                                 // T(n) = O(1), S(n) = O(1)
        build(nums, o * 2, l, m);                                            // T(n) = O(n), S(n) = O(log n)
        build(nums, o * 2 + 1, m + 1, r);                                    // T(n) = O(n), S(n) = O(log n)
        maintain(o);                                                         // T(n) = O(k) = O(1), S(n) = O(1)
    }

    public void update(int o, int l, int r, int index, int value) {
        if(l == r) {                                                         // T(n) = O(1), S(n) = O(1)
            makeLeaf(o, value);                                              // T(n) = O(1), S(n) = O(1)
            return;                                                          // T(n) = O(1), S(n) = O(1)
        }
        int m = (l + r) / 2;                                                 // T(n) = O(1), S(n) = O(1)

        if(index <= m) {                                                     // T(n) = O(1), S(n) = O(1)
            update(o * 2, l, m, index, value);                               // T(n) = O(log n), S(n) = O(log n)
        }else {
            update(o * 2 + 1, m + 1, r, index, value);                       // T(n) = O(log n), S(n) = O(log n)
        }
        maintain(o);                                                         // T(n) = O(k) = O(1), S(n) = O(1)
    }

    public int[] query(int o, int l, int r, int L, int R) {
        if(L <= l && r <= R) {                                               // T(n) = O(1), S(n) = O(1)
            return tree[o];                                                  // T(n) = O(1), S(n) = O(1)
        }

        int m = (l + r) / 2;                                                 // T(n) = O(1), S(n) = O(1)

        if(R <= m) {                                                         // T(n) = O(1), S(n) = O(1)
            return query(o * 2, l, m, L, R);                                 // T(n) = O(log n), S(n) = O(log n)
        }
        if(L > m) {                                                          // T(n) = O(1), S(n) = O(1)
            return query(o * 2 + 1, m + 1, r, L, R);                         // T(n) = O(log n), S(n) = O(log n)
        }

        int[] left = query(o * 2, l, m, L, R);                               // T(n) = O(log n), S(n) = O(log n)
        int[] right = query(o * 2 + 1, m + 1, r, L, R);                      // T(n) = O(log n), S(n) = O(log n)
        int[] result = new int[MAXK];                                        // T(n) = O(1), S(n) = O(1)
        mergePre(left, right, result);                                       // T(n) = O(k) = O(1), S(n) = O(1)
        return result;                                                       // T(n) = O(1), S(n) = O(1)
    }
}

class Solution {

    public int[] resultArray(int[] nums, int k, int[][] queries) {
        int n = nums.length;                                                 // T(n) = O(1), S(n) = O(1)
        SegmentTree seg = new SegmentTree(nums, k);                          // T(n) = O(n), S(n) = O(n)
        int[] ans = new int[queries.length];                                 // T(n) = O(q), S(n) = O(q)

        for(int i = 0; i < queries.length; i++) {                            // T(n) = O(q)
            int[] q = queries[i];                                            // T(n) = O(1), S(n) = O(1)
            int index = q[0];                                                // T(n) = O(1), S(n) = O(1)
            int value = q[1];                                                // T(n) = O(1), S(n) = O(1)
            int start = q[2];                                                // T(n) = O(1), S(n) = O(1)
            int x = q[3];                                                    // T(n) = O(1), S(n) = O(1)

            seg.update(1, 0, n - 1, index, value);                           // T(n) = O(log n), S(n) = O(log n)
            int[] pre = seg.query(1, 0, n - 1, start, n - 1);                // T(n) = O(log n), S(n) = O(log n)
            ans[i] = pre[x];                                                 // T(n) = O(1), S(n) = O(1)
        }
        return ans;                                                          // T(n) = O(1), S(n) = O(1)
    }
}
```

# Time Complexity:
T(n, q) = O(n + q log n)

# Space Complexity:
S(n, q) = O(n + q)
