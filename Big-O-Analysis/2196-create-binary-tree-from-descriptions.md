# 2196. Create Binary Tree From Descriptions

# Problem Summary:
You are given a 2D array descriptions where: descriptions[i] = [parent, child, isLeft] indicates that:
1. parent is the parent node.
2. child is the child node.
3. If isLeft is 1, the child is the left child.
4. If isLeft is 0, the child is the right child.
All node values are unique, and the input is guaranteed to describe a valid binary tree. Return the root of the constructed binary tree.

# Approach Used:
The solution uses an array to store references to TreeNode objects based on their values.
The construction is performed in two passes:
1. Create TreeNode objects for all child nodes.
2. Traverse the descriptions again and connect each child to its parent.
Only the root node will not already exist in the array after the first pass, because every child node is created beforehand. Therefore, during the second pass, if a parent reference is null, that parent must be the root node; it is then created, stored in the array, and returned at the end.

# Steps:
1) Create an array to store TreeNode references indexed by node value
2) Traverse all descriptions
   * Create a TreeNode for every child value
4) Traverse all descriptions again
5) For each description:
   * Retrieve the parent node
   * If it does not exist, create it and store it
   * Connect the child node as either the left or right child
6) Return the root node

# Solution:
```
/**
 * Definition for a binary tree node.
 * public class TreeNode {
 *     int val;
 *     TreeNode left;
 *     TreeNode right;
 *     TreeNode() {}
 *     TreeNode(int val) { this.val = val; }
 *     TreeNode(int val, TreeNode left, TreeNode right) {
 *         this.val = val;
 *         this.left = left;
 *         this.right = right;
 *     }
 * }
 */
class Solution {
    public TreeNode createBinaryTree(int[][] descriptions) {
        TreeNode[] nodes = new TreeNode[1000001];        // T(n) = O(1), S(n) = O(1000001)
        TreeNode root = null;                            // T(n) = O(1), S(n) = O(1)

        // Loop runs descriptions.length times
        for(int[] description : descriptions) {
            int child = description[1];                  // T(n) = O(1)
            nodes[child] = new TreeNode(child);          // T(n) = O(1) per execution, total executions = O(n)
        }

        // Loop runs descriptions.length times
        for(int[] description : descriptions) {
            int parent = description[0];                  // T(n) = O(1)
            int child = description[1];                   // T(n) = O(1)
            boolean isLeft = description[2] == 1;         // T(n) = O(1)
            TreeNode p = nodes[parent];                   // T(n) = O(1)

            if(p == null) {
                p = new TreeNode(parent);                 // T(n) = O(1)
                nodes[parent] = p;                        // T(n) = O(1)
                root = p;                                 // T(n) = O(1)
            }
            if(isLeft) {
                p.left = nodes[child];                    // T(n) = O(1)
            }else{
                p.right = nodes[child];                   // T(n) = O(1)
            }
        }
        return root;                                      // T(n) = O(1)
    }
}
```

# Time Complexity:
T(n) = O(n)

# Space Complexity:
S(n) = O(1)

