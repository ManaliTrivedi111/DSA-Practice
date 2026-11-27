# 3841. Palindromic Path Queries in a Tree

# Problem Summary:

Given a tree whose nodes contain lowercase English letters, process two types of queries:
- update u c: Change the character at node u to c.
- query u v: Determine whether the characters on the path from u to v can be rearranged into a palindrome.
A path can be rearranged into a palindrome if at most one character occurs an odd number of times.

# Approach Used:

Use an Euler tour, a Binary Indexed Tree (BIT), and Binary Lifting for Lowest Common Ancestor (LCA) queries.
Represent each character with one bit in a 26-bit mask. The XOR of these masks indicates which characters occur an odd number of times.
DFS assigns each node an entry time l[u] and subtree exit time r[u], so each subtree corresponds to the Euler-tour interval [l[u], r[u]].
The BIT supports XOR updates over subtree intervals and prefix XOR queries. Updating a node's character applies the XOR difference between the old and new character masks to its subtree interval.
For a path from u to v, let w be their LCA. Its parity mask is: prefix(u) XOR prefix(v) XOR mask(w)
Binary Lifting finds the LCA in O(log n). The path can form a palindrome if the resulting mask contains at most one set bit.

# Steps:

1. Build an adjacency list from the edges.
2. Calculate LOG, and initialize the parent, depth, and Euler-tour arrays.
3. Run DFS from node 0 to assign entry and exit times, record each node's parent, and calculate depths.
4. Build the Binary Lifting table.
5. Initialize the BIT by applying each node's character mask to its subtree interval.
6. Process each query:
   - For an update, XOR the old and new character masks and apply the difference to the node's subtree interval.
   - For a path query, find the LCA, calculate the path's parity mask using two BIT prefix queries, and check whether it has at most one set bit.
7. Return the list of query results.

# Solution:

```
class BIT {

    private int[] tree;                                                   // T(n) = O(1), S(n) = O(n)
    private int n;                                                        // T(n) = O(1), S(n) = O(1)

    public BIT(int n) {
        this.n = n;                                                       // T(n) = O(1)
        tree = new int[n + 1];                                            // T(n) = O(n), S(n) = O(n)
    }

    private static int lowbit(int x) {
        return x & -x;                                                    // T(n) = O(1), S(n) = O(1)
    }

    public void update(int x, int v) {
        while (x <= n) {                                                  // T(n) = O(log n)
            tree[x] ^= v;                                                 // T(n) = O(1)
            x += lowbit(x);                                               // T(n) = O(1)
        }
    }

    public int query(int x) {
        int ret = 0;                                                      // T(n) = O(1), S(n) = O(1)

        while (x > 0) {                                                   // T(n) = O(log n)
            ret ^= tree[x];                                               // T(n) = O(1)
            x -= lowbit(x);                                               // T(n) = O(1)
        }
        return ret;                                                       // T(n) = O(1)
    }

    public void updateRange(int l, int r, int v) {
        update(l, v);                                                     // T(n) = O(log n)
        update(r + 1, v);                                                 // T(n) = O(log n)
    }
}

class Solution {

    private List<List<Integer>> g;                                        // T(n) = O(1), S(n) = O(n)
    private int[][] fa;                                                   // T(n) = O(1), S(n) = O(n * log n)
    private int[] dep;                                                    // T(n) = O(1), S(n) = O(n)
    private int[] l;                                                      // T(n) = O(1), S(n) = O(n)
    private int[] r;                                                      // T(n) = O(1), S(n) = O(n)
    private int timer;                                                    // T(n) = O(1), S(n) = O(1)
    private int LOG;                                                      // T(n) = O(1), S(n) = O(1)

    public List<Boolean> palindromePath(int n, int[][] edges, String s, String[] queries) {
        g = new ArrayList<>();                                            // T(n) = O(1), S(n) = O(1)

        for(int i = 0; i < n; i++) {                                      // T(n) = O(n)
            g.add(new ArrayList<>());                                     // T(n) = O(1), S(n) = O(1) per list
        }

        for(int[] e : edges) {                                            // T(n) = O(n)
            g.get(e[0]).add(e[1]);                                        // T(n) = O(1), S(n) = O(1) amortized
            g.get(e[1]).add(e[0]);                                        // T(n) = O(1), S(n) = O(1) amortized
        }

        LOG = 32 - Integer.numberOfLeadingZeros(n);                       // T(n) = O(1), S(n) = O(1)
        fa = new int[n][LOG];                                             // T(n) = O(n * log n), S(n) = O(n * log n)
        dep = new int[n];                                                 // T(n) = O(n), S(n) = O(n)
        l = new int[n];                                                   // T(n) = O(n), S(n) = O(n)
        r = new int[n];                                                   // T(n) = O(n), S(n) = O(n)
        timer = 0;                                                        // T(n) = O(1), S(n) = O(1)

        dfs(0, 0);                                                        // T(n) = O(n), S(n) = O(n) worst-case call stack

        for(int j = 1; j < LOG; j++) {                                    // T(n) = O(log n)
            for(int i = 0; i < n; i++) {                                  // T(n) = O(n) per outer iteration
                fa[i][j] = fa[fa[i][j - 1]][j - 1];                       // T(n) = O(1)
            }
        }

        BIT bit = new BIT(n + 1);                                         // T(n) = O(n), S(n) = O(n)
        char[] chars = s.toCharArray();                                   // T(n) = O(n), S(n) = O(n)

        for(int i = 0; i < n; i++) {                                      // T(n) = O(n * log n)
            bit.updateRange(l[i], r[i], 1 << (chars[i] - 'a'));           // T(n) = O(log n)
        }

        List<Boolean> ans = new ArrayList<>();                            // T(n) = O(1), S(n) = O(1)

        for(String q : queries) {                                         // T(n) = O(Q * log n)
            String[] parts = q.split(" ");                                // T(n) = O(log n), S(n) = O(log n) per query
            if(parts[0].equals("update")) {                               // T(n) = O(1)
                int u = Integer.parseInt(parts[1]);                       // T(n) = O(log n) for parsing
                char c = parts[2].charAt(0);                              // T(n) = O(1)
                bit.updateRange(l[u], r[u], (1 << (chars[u] - 'a')) ^ (1 << (c - 'a'))); // T(n) = O(log n)
                chars[u] = c;                                             // T(n) = O(1)
            }else {
                int u = Integer.parseInt(parts[1]);                       // T(n) = O(log n) for parsing
                int v = Integer.parseInt(parts[2]);                       // T(n) = O(log n) for parsing
                int w = lca(u, v);                                        // T(n) = O(log n)
                int mask = bit.query(l[u]) ^ bit.query(l[v]) ^ (1 << (chars[w] - 'a')); // T(n) = O(log n)
                ans.add(Integer.bitCount(mask) <= 1);                     // T(n) = O(1), S(n) = O(1)
            }
        }
        return ans;                                                       // T(n) = O(1)
    }

    private void dfs(int u, int p) {
        l[u] = ++timer;                                                   // T(n) = O(1)
        fa[u][0] = p;                                                     // T(n) = O(1)

        for(int v : g.get(u)) {                                           // T(n) = O(degree of u)
            if(v == p) {                                                  // T(n) = O(1)
                continue;                                                 // T(n) = O(1)
            }
            dep[v] = dep[u] + 1;                                          // T(n) = O(1)
            dfs(v, u);                                                    // T(n) = O(size of visited subtree)
        }
        r[u] = timer;                                                     // T(n) = O(1)
    }

    private int lca(int u, int v) {
        if(dep[u] < dep[v]) {                                             // T(n) = O(1)
            int t = u;                                                    // T(n) = O(1)
            u = v;                                                        // T(n) = O(1)
            v = t;                                                        // T(n) = O(1)
        }

        int diff = dep[u] - dep[v];                                       // T(n) = O(1)

        for(int i = 0; i < LOG; i++) {                                    // T(n) = O(log n)
            if(((diff >> i) & 1) == 1) {                                  // T(n) = O(1)
                u = fa[u][i];                                             // T(n) = O(1)
            }
        }

        if(u == v) {                                                      // T(n) = O(1)
            return u;                                                     // T(n) = O(1)
        }

        for(int i = LOG - 1; i >= 0; i--) {                               // T(n) = O(log n)
            if(fa[u][i] != fa[v][i]) {                                    // T(n) = O(1)
                u = fa[u][i];                                             // T(n) = O(1)
                v = fa[v][i];                                             // T(n) = O(1)
            }
        }
        return fa[u][0];                                                  // T(n) = O(1)
    }
}
```

# Time Complexity:
T(n, Q) = O((n + Q) * log n)

# Space Complexity:
S(n, Q) = O(n * log n + Q)

