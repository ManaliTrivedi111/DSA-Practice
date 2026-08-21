# 3310. Remove Methods From Project

# Problem Summary:

You are given n methods numbered from 0 to n - 1. An invocation [a, b] means method a invokes method b. Method k contains a known bug. Method k and every method that can be reached from k through direct or indirect invocations are considered suspicious. We want to remove all suspicious methods, but this is possible only if no non-suspicious method invokes a suspicious method. If every suspicious method can be safely removed, return all the remaining non-suspicious methods. Otherwise, return all methods because none of the suspicious methods can be removed.

# Approach Used:

The solution uses a directed graph and Breadth-First Search (BFS). Each method is represented as a node in the graph. If method u invokes method v, we add a directed edge from u to v.

First, we build the graph using an adjacency list.
Then, we start a BFS from method k. Every method reachable from k is marked as suspicious using the isSus boolean array.

After identifying all suspicious methods, we check every non-suspicious method. If any non-suspicious method invokes a suspicious method, then the suspicious methods cannot be removed because a method outside the group would be invoking a method inside the group. In that case, the solution returns all methods from 0 to n - 1.

If no non-suspicious method invokes a suspicious method, all suspicious methods can safely be removed. Therefore, we return only the non-suspicious methods.

# Steps:

1. Create a boolean array isSus to mark suspicious methods.
2. Create an adjacency list graph to represent the invocation relationships.
3. Add every invocation [u, v] as a directed edge from u to v.
4. Start a BFS from method k.
5. Mark every method reachable from k as suspicious.
6. Traverse every non-suspicious method and check whether it invokes a suspicious method.
7. If such an invocation exists, return all methods because the suspicious group cannot be removed.
8. Otherwise, add every non-suspicious method to ans.
9. Return ans.

# Solution:

```
class Solution {
    public List<Integer> remainingMethods(int n, int k, int[][] invocations) {
        boolean[] isSus = new boolean[n];                                    // T(n) = O(n), S(n) = O(n)
        List<Integer>[] graph = new List[n];                                 // T(n) = O(n), S(n) = O(n)
        List<Integer> ans = new ArrayList<>();                               // T(n) = O(1), S(n) = O(n)

        for(int i = 0; i < n; i++) {
            graph[i] = new ArrayList<>();                                    // T(n) = O(1), S(n) = O(1)
        }

        for(int[] inv : invocations) {
            int u = inv[0];                                                  // T(n) = O(1), S(n) = O(1)
            int v = inv[1];                                                  // T(n) = O(1), S(n) = O(1)

            graph[u].add(v);                                                 // T(n) = O(1), S(n) = O(1)
        }

        Queue<Integer> q = new ArrayDeque<>();                               // T(n) = O(1), S(n) = O(1)
        q.offer(k);                                                          // T(n) = O(1), S(n) = O(1)

        while(!q.isEmpty()) {
            int method1 = q.poll();                                          // T(n) = O(1), S(n) = O(1)
            isSus[method1] = true;                                           // T(n) = O(1), S(n) = O(1)

            for(int method2 : graph[method1]) {
                if(!isSus[method2]) {
                    q.offer(method2);                                        // T(n) = O(1), S(n) = O(1)
                }
            }
        }

        for(int i = 0; i < n; i++) {
            if(!isSus[i]) {
                for(int method : graph[i]) {
                    if(isSus[method]) {
                        for(int j = 0; j < n; j++) {
                            ans.add(j);                                      // T(n) = O(1), S(n) = O(1)
                        }
                        return ans;                                          // T(n) = O(1), S(n) = O(1)
                    }
                }
            }
        }
        
        for(int i = 0; i < n; i++) {
            if(!isSus[i]) {
                ans.add(i);                                                  // T(n) = O(1), S(n) = O(1)
            }
        }
        
        return ans;                                                          // T(n) = O(1), S(n) = O(1)
    }
}
```

# Time Complexity:
T(n) = O(n + E), where n = number of nodes, and E = number of edges

# Space Complexity:
S(n) = O(n + E), where n = number of nodes, and E = number of edges
