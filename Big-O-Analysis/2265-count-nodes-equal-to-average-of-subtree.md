# 2265. Count Nodes Equal to Average of Subtree

# Problem Summary:

You are given the root of a binary tree. For every node, consider the subtree rooted at that node, including the node itself and all of its descendants.

The task is to count how many nodes have a value equal to the average of all values in their subtree.

The average is calculated as: (sum of values / number of nodes) and rounded down to the nearest integer.

# Approach Used:

The solution uses a postorder traversal of the binary tree.

For every node, calcSumAndCountOfNodes() calculates and returns two pieces of information about the subtree rooted at that node:
* The total sum of all node values in the subtree.
* The total number of nodes in the subtree.

The method first recursively processes the left and right subtrees. Once their sums and node counts are available, it calculates the sum and count for the current node's entire subtree.

For the current node:
* totalSum = root.val + leftSum + rightSum
* totalCount = 1 + leftCount + rightCount

The average is: totalSum / totalCount

Because both values are integers in Java, integer division rounds the result down.

If the current node's value is equal to this average, ans is incremented.

The method then returns {totalSum, totalCount} so that the parent node can use this information when calculating its own subtree.

# Steps:

1. Initialize the global variable ans to 0.
2. Start the recursive traversal from the root.
3. For each node, recursively calculate the sum and node count of its left subtree.
4. Recursively calculate the sum and node count of its right subtree.
5. Calculate the total sum of the current node's subtree.
6. Calculate the total number of nodes in the current node's subtree.
7. Compute the subtree average using integer division.
8. If the current node's value equals the subtree average, increment ans.
9. Return the total sum and total count to the parent node.
10. After the traversal is complete, return ans.

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
    private int ans = 0;                                                     // T(n) = O(1), S(n) = O(1)

    public int averageOfSubtree(TreeNode root) {
        if(root != null) {                                                   // T(n) = O(1), S(n) = O(1)
            calcSumAndCountOfNodes(root);                                    // T(n) = O(n), S(n) = O(h)
        }
        return this.ans;                                                     // T(n) = O(1), S(n) = O(1)
    }

    private int[] calcSumAndCountOfNodes(TreeNode root) {
        int leftSum = 0;                                                     // T(n) = O(1), S(n) = O(1)
        int rightSum = 0;                                                    // T(n) = O(1), S(n) = O(1)
        int leftCount = 0;                                                   // T(n) = O(1), S(n) = O(1)
        int rightCount = 0;                                                  // T(n) = O(1), S(n) = O(1)

        if(root.left != null) {                                              // T(n) = O(1), S(n) = O(1)
            int[] leftSumAndCount = calcSumAndCountOfNodes(root.left);
                                                                             // T(n) = O(size of left subtree), S(n) = O(h)
            leftSum = leftSumAndCount[0];                                    // T(n) = O(1), S(n) = O(1)
            leftCount = leftSumAndCount[1];                                  // T(n) = O(1), S(n) = O(1)
        }

        if(root.right != null) {                                             // T(n) = O(1), S(n) = O(1)
            int[] rightSumAndCount = calcSumAndCountOfNodes(root.right);
                                                                             // T(n) = O(size of right subtree), S(n) = O(h)
            rightSum = rightSumAndCount[0];                                  // T(n) = O(1), S(n) = O(1)
            rightCount = rightSumAndCount[1];                                // T(n) = O(1), S(n) = O(1)
        }

        int totalSum = root.val + leftSum + rightSum;                        // T(n) = O(1), S(n) = O(1)
        int totalCount = 1 + leftCount + rightCount;                         // T(n) = O(1), S(n) = O(1)

        if(root.val == (totalSum / totalCount)) {                            // T(n) = O(1), S(n) = O(1)
            this.ans++;                                                      // T(n) = O(1), S(n) = O(1)
        }

        return new int[]{totalSum, totalCount};                              // T(n) = O(1), S(n) = O(1)
    }
}
```

# Time Complexity:
T(n) = O(n)

# Space Complexity:
S(n) = O(h), where h = height of the binary tree
