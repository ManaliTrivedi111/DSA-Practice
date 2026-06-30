# 2095. Delete the Middle Node of a Linked List

# Problem Summary:
You are given the head of a singly linked list. Delete the middle node and return the head of the modified linked list. The middle node is defined as the n/2th node using 0-based indexing, where n is the number of nodes in the list.

For example:
* n = 1 → middle index = 0
* n = 2 → middle index = 1
* n = 3 → middle index = 1
* n = 4 → middle index = 2
* n = 5 → middle index = 2

# Approach Used:
The solution uses the slow and fast pointer technique to locate the node immediately before the middle node.
* The slow pointer moves one node at a time.
* The fast pointer moves two nodes at a time.
The fast pointer starts at the second node instead of the head. This ensures that when the traversal ends, the slow pointer is positioned just before the middle node. Once the traversal is complete, the middle node is removed by updating the next pointer of the slow node to skip over the middle node.
A special case is handled separately: if the linked list contains only one node, deleting the middle node results in an empty list.

# Steps:
1. If the list contains only one node:
   * Return null
2. Initialize:
   * slow at the head
   * fast at the second node
3. Move:
   * slow one step at a time
   * fast two steps at a time
4. When the traversal finishes, slow points to the node immediately before the middle node
5. Delete the middle node by updating:
   * slow.next = slow.next.next
6. Return the head of the modified list

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

    public ListNode deleteMiddle(ListNode head) {

        if(head.next == null) {
            return null;                                    // T(n) = O(1)
        }

        ListNode slow = head;                               // T(n) = O(1), S(n) = O(1)
        ListNode fast = head.next;                          // T(n) = O(1), S(n) = O(1)

        // Finds the node before the middle node
        while(fast.next != null && fast.next.next != null) {
            slow = slow.next;                               // T(n) = O(1) per execution
            fast = fast.next.next;                          // T(n) = O(1) per execution
        }

        slow.next = slow.next.next;                         // T(n) = O(1)
        return head;                                        // T(n) = O(1)
    }
}
```

# Time Complexity:
T(n) = O(n)

# Space Complexity:
S(n) = O(1)
