# 2130. Maximum Twin Sum of a Linked List

# Problem Summary:
You are given the head of a singly linked list of even length n. For every node at index i, its twin is the node at index: n - 1 - i
The twin sum is defined as: node[i].val + node[n - 1 - i].val
Return the maximum twin sum among all twin pairs.
For example:
* Node 0 is paired with node n - 1
* Node 1 is paired with node n - 2

# Approach Used:
The solution uses the classic slow and fast pointer technique to find the middle of the linked list.
Once the middle is found:
1. Reverse the second half of the linked list.
2. Traverse the first half and the reversed second half simultaneously.
3. Compute the twin sum for each pair.
4. Track the maximum twin sum encountered.

Reversing the second half allows the twin nodes to become aligned positionally:

Original:
First Half -> A -> B
Second Half -> C -> D

After reversing:
First Half -> A -> B
Reversed Second Half -> D -> C

Now:
* A pairs with D
* B pairs with C
which corresponds exactly to the twin relationship.

# Steps:
1. Use slow and fast pointers to locate the middle of the linked list
2. Reverse the second half of the list
3. Initialize two pointers:
   -> One at the start of the first half
   -> One at the start of the reversed second half
4. Traverse both halves simultaneously
5. For each pair:
   -> Calculate the twin sum
   -> Update the maximum twin sum
6. Return the maximum twin sum

# Solution:
```
/**
 * Definition for singly-linked list.
 * public class ListNode {
 *     int val;
 *     ListNode next;
 *     ListNode() {}
 *     ListNode(int val) { this.val = val; }
 *     ListNode(int val, ListNode next) {
 *         this.val = val;
 *         this.next = next;
 *     }
 * }
 */
class Solution {

    public int pairSum(ListNode head) {

        ListNode slow = head;                                  // T(n) = O(1), S(n) = O(1)
        ListNode fast = head.next;                             // T(n) = O(1), S(n) = O(1)
        int maxSum = 0;                                        // T(n) = O(1), S(n) = O(1)

        // Finds the middle of the linked list
        while(fast.next != null && fast.next.next != null) {
            slow = slow.next;                                  // T(n) = O(1) per iteration
            fast = fast.next.next;                             // T(n) = O(1) per iteration
        }

        slow.next = reverse(slow.next);                        // T(n) = O(n / 2)
        ListNode l1 = head;                                    // T(n) = O(1)
        ListNode l2 = slow.next;                               // T(n) = O(1)

        // Traverses both halves simultaneously
        while(l2 != null) {
            maxSum = Math.max(maxSum, l1.val + l2.val);        // T(n) = O(1)
            l1 = l1.next;                                      // T(n) = O(1)
            l2 = l2.next;                                      // T(n) = O(1)
        }
        return maxSum;                                         // T(n) = O(1)
    }

    private ListNode reverse(ListNode head) {
        ListNode prev = null;                                 
        ListNode curr = head;

        while(curr != null) {
            ListNode next = curr.next;
            curr.next = prev;
            prev = curr;
            curr = next;
        }
        return prev;
    }
}
```

# Time Complexity:
T(n) = O(n)

# Space Complexity:
S(n) = O(1)
