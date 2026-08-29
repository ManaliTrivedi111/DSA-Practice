# 2213. Longest Substring of One Repeating Character

# Problem Summary:

You are given a string s and k queries. Each query changes the character at a specified index in s. After every query, we need to return the length of the longest substring consisting of only one repeating character. Because the string can be modified many times, checking the entire string after every query would be inefficient.

# Approach Used:

The solution uses a Segment Tree to efficiently maintain information about the longest substring of equal characters.

For every segment of the string, the Segment Tree stores three values:
* pre = the length of the longest repeating-character substring starting from the beginning of the segment.
* suf = the length of the longest repeating-character substring ending at the end of the segment.
* best = the length of the longest repeating-character substring anywhere inside the segment.

When two child segments are combined, we first take the best result from either child.
If the last character of the left segment is equal to the first character of the right segment, the repeating substring can cross the boundary between the two segments.
In that case:
* pre may extend into the right segment if the entire left segment has the same character.
* suf may extend into the left segment if the entire right segment has the same character.
* best may be formed by combining the suffix of the left segment with the prefix of the right segment.

For each query, the character at the given index is updated. The Segment Tree then updates only the nodes on the path from that index to the root.

The value tree.best[1] at the root represents the longest repeating-character substring in the entire string after the current query.

# Steps:

1. Convert s into a character array cs.
2. Create the Segment Tree arrays pre, suf, and best.
3. Build the Segment Tree recursively.
4. For every leaf node, initialize pre, suf, and best to 1.
5. Combine child nodes using pushUp().
6. For each query, update the character in cs at the specified index.
7. Update the corresponding path in the Segment Tree.
8. Store tree.best[1] as the answer for that query.
9. Return the array of answers.

# Solution:

```
class Solution {

    private static class SegmentTree {
        private final int n;                                                 // T(n) = O(1), S(n) = O(1)
        private final int[] pre;                                             // T(n) = O(1), S(n) = O(n)
        private final int[] suf;                                             // T(n) = O(1), S(n) = O(n)
        private final int[] best;                                            // T(n) = O(1), S(n) = O(n)
        private final char[] cs;                                             // T(n) = O(1), S(n) = O(n)

        public SegmentTree(String s) {
            cs = s.toCharArray();                                            // T(n) = O(n), S(n) = O(n)
            n = cs.length;                                                   // T(n) = O(1), S(n) = O(1)

            final int size = n << 2;                                         // T(n) = O(1), S(n) = O(1)
            pre = new int[size];                                             // T(n) = O(n), S(n) = O(n)
            suf = new int[size];                                             // T(n) = O(n), S(n) = O(n)
            best = new int[size];                                            // T(n) = O(n), S(n) = O(n)
            
            build(1, 0, n - 1);                                              // T(n) = O(n), S(n) = O(log n)
        }

        private void build(int node, int l, int r) {
            if (l == r) {
                pre[node] = suf[node] = best[node] = 1;                      // T(n) = O(1), S(n) = O(1)
                return;
            }

            int mid = (l + r) >>> 1;                                         // T(n) = O(1), S(n) = O(1)

            build(node << 1, l, mid);                                        // T(n) = O(n), S(n) = O(log n)
            build(node << 1 | 1, mid + 1, r);                                // T(n) = O(n), S(n) = O(log n)

            pushUp(node, l, r);                                              // T(n) = O(1), S(n) = O(1)
        }

        private void pushUp(int node, int l, int r) {
            int left = node << 1;                                            // T(n) = O(1), S(n) = O(1)
            int right = node << 1 | 1;                                       // T(n) = O(1), S(n) = O(1)
            int mid = (l + r) >>> 1;                                         // T(n) = O(1), S(n) = O(1)
            int lenL = mid - l + 1;                                          // T(n) = O(1), S(n) = O(1)
            int lenR = r - mid;                                              // T(n) = O(1), S(n) = O(1)

            pre[node] = pre[left];                                           // T(n) = O(1), S(n) = O(1)
            suf[node] = suf[right];                                          // T(n) = O(1), S(n) = O(1)
            best[node] = Math.max(best[left], best[right]);                  // T(n) = O(1), S(n) = O(1)

            if(cs[mid] == cs[mid + 1]) {
                if(pre[left] == lenL) {
                    pre[node] = lenL + pre[right];                           // T(n) = O(1), S(n) = O(1)
                }

                if(suf[right] == lenR) {
                    suf[node] = lenR + suf[left];                            // T(n) = O(1), S(n) = O(1)
                }

                best[node] = Math.max(
                    best[node],
                    suf[left] + pre[right]
                );                                                           // T(n) = O(1), S(n) = O(1)
            }
        }

        public void update(int i) {
            update(1, 0, n - 1, i);                                          // T(n) = O(log n), S(n) = O(log n)
        }

        private void update(int node, int l, int r, int i) {
            if (l == r) {
                return;                                                      // T(n) = O(1), S(n) = O(1)
            }

            int mid = (l + r) >>> 1;                                         // T(n) = O(1), S(n) = O(1)

            if (i <= mid) {
                update(node << 1, l, mid, i);                                // T(n) = O(log n), S(n) = O(log n)
            } else {
                update(node << 1 | 1, mid + 1, r, i);                        // T(n) = O(log n), S(n) = O(log n)
            }

            pushUp(node, l, r);                                              // T(n) = O(1), S(n) = O(1)
        }

        public void updateChar(char c, int i) {
            cs[i] = c;                                                       // T(n) = O(1), S(n) = O(1)
        }
    }

    public int[] longestRepeating(String s, String queryCharacters, int[] queryIndices) {
        int k = queryIndices.length;                                         // T(n) = O(1), S(n) = O(1)
        SegmentTree tree = new SegmentTree(s);                               // T(n) = O(n), S(n)
        int[] ans = new int[k];                                              // T(n) = O(k), S(n)
        char[] qc = queryCharacters.toCharArray();                           // T(n) = O(k), S(n)

        for (int i = 0; i < k; i++) {
            int index = queryIndices[i];                                     // T(n) = O(1), S(n) = O(1)

            tree.updateChar(qc[i], index);                                   // T(n) = O(1), S(n) = O(1)
            tree.update(index);                                              // T(n) = O(log n), S(n) = O(log n)

            ans[i] = tree.best[1];                                           // T(n) = O(1), S(n) = O(1)
        }

        return ans;                                                          // T(n) = O(1), S(n) = O(1)
    }
}
```

# Time Complexity:
T(n, k) = O(n + k * log n), where n = length of string s, and k = number of queries

# Space Complexity:
S(n, k) = O(n + k), where n = length of string s, and k = number of queries
