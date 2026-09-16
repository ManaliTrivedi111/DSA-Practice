# 2058. Find the Minimum and Maximum Number of Nodes Between Critical Points

# Problem Summary:

You are given a linked list. A critical point is a node that is either a local maximum or a local minimum.

A node is a local maximum if its value is strictly greater than both its previous and next nodes.
A node is a local minimum if its value is strictly smaller than both its previous and next nodes.

The task is to return an array [minDistance, maxDistance], where:
*minDistance is the minimum distance between any two distinct critical points.
*maxDistance is the maximum distance between any two distinct critical points.

If there are fewer than two critical points, return [-1, -1].

# Approach Used:

The solution traverses the linked list once while keeping track of three consecutive nodes: prev, curr, and next.

Whenever curr is a local maximum or local minimum, it is identified as a critical point.

The solution stores the position of the first critical point in first and the most recently found critical point in last.

For every new critical point, the distance from the previous critical point is calculated and used to update minDis. The last position is then updated.

At the end, the distance between the first and last critical points gives the maximum distance.

If only one critical point was found, the method returns [-1, -1].

# Steps:

1. Initialize prev, curr, and next to represent three consecutive nodes.
2. Initialize variables to track the minimum distance, the first critical point, the most recently found critical point, and the current position.
3. Traverse the linked list while next is not null.
4. Check whether curr is a local maximum or local minimum.
5. If curr is the first critical point, store its position in first and last.
6. Otherwise, calculate the distance from the previous critical point, update minDis, and update last.
7. Move prev, curr, and next one position forward.
8. If only one critical point was found, return [-1, -1].
9. Otherwise, return the minimum distance and the distance between the first and last critical points.

# Solution:

```
/**
 * Definition for singly-linked list.
 * public class ListNode {
 *     int val;
 *     ListNode next;
 *     ListNode() {}
 *     ListNode(int val) { this.val = val; }
 *     ListNode(int val, ListNode next) { this.val = val; this.next = next; }
 * }
 */
class Solution {
    public int[] nodesBetweenCriticalPoints(ListNode head) {
        int minDis = Integer.MAX_VALUE;                                      // T(n) = O(1), S(n) = O(1)

        ListNode prev = head;                                                // T(n) = O(1), S(n) = O(1)
        ListNode curr = head.next;                                           // T(n) = O(1), S(n) = O(1)
        ListNode next = head.next.next;                                      // T(n) = O(1), S(n) = O(1)
        int first = 0;                                                       // T(n) = O(1), S(n) = O(1)
        int last = 0;                                                        // T(n) = O(1), S(n) = O(1)
        int counter = 0;                                                     // T(n) = O(1), S(n) = O(1)

        while(next != null) {                                                // T(n) = O(n)
            counter++;                                                       // T(n) = O(1)
            
            if((curr.val > prev.val && curr.val > next.val) || 
            (curr.val < prev.val && curr.val < next.val)) {                 
                if(first == 0) {                                          
                    first = counter;                                         // T(n) = O(1)
                    last = counter;                                          // T(n) = O(1)
                }else{ 
                    minDis = Math.min(minDis, counter - last);               // T(n) = O(1)
                    last = counter;                                          // T(n) = O(1)
                }
            }
            prev = curr;                                                     // T(n) = O(1)
            curr = curr.next;                                                // T(n) = O(1)
            next = curr.next;                                                // T(n) = O(1)
        }
        if(first == last) {
            return new int[]{-1, -1};                                        // T(n) = O(1), S(n) = O(1)
        }
        return new int[]{minDis, last - first};                              // T(n) = O(1), S(n) = O(1)
    }
}
```

# Time Complexity:
T(n) = O(n)

# Space Complexity:
S(n) = O(1)
