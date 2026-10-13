# 1096. Brace Expansion II

# Problem Summary

You are given an expression that represents a set of lowercase words using three operations:
- A lowercase letter represents a single word.
- Comma-separated expressions represent the union of their word sets.
- Concatenated expressions represent all possible concatenations between the words from the two sets.

Return all distinct words represented by the expression in lexicographically sorted order.

# Approach Used

The solution recursively evaluates each expression using dfs.

For every expression, it scans the characters while tracking the current brace nesting level. Top-level commas separate union groups, while letters and completed brace expressions are merged into the current group.

The merge method handles concatenation. If the current group contains A words and the new group contains B words, it creates all A × B possible concatenations.

Finally, a TreeSet removes duplicate words and keeps the generated words sorted.

Because brace expansion can generate exponentially many words, the complexity depends on the number and length of generated words, not just the input expression length.

# Steps

1. Start the recursive evaluation with the complete expression.
2. Create a TreeSet for the final distinct and sorted words.
3. Maintain groups to represent expressions separated by top-level commas.
4. Scan the current expression from left to right.
5. Track brace nesting with layer.
6. When a complete brace expression is found, recursively evaluate it and merge its words into the current group.
7. When a top-level comma is found, start a new group.
8. When a top-level character is found, treat it as a singleton word and merge it with the current group.
9. Insert all words from the groups into the TreeSet.
10. Return the sorted words as an ArrayList.

# Solution

```
class Solution {
    public List<String> braceExpansionII(String expression) {
        return dfs(expression, 0, expression.length() - 1);                                    // T(n) = O(WL log W), S(n) = O(WL + N)
    }

    private List<String> dfs(final String expression, int s, int e) {
        TreeSet<String> ans = new TreeSet<>();                                                 // T(n) = O(1), S(n) = O(1)
        List<List<String>> groups = new ArrayList<>();                                         // T(n) = O(1), S(n) = O(1)
        groups.add(new ArrayList<>());                                                         // T(n) = O(1), S(n) = O(1)
        int layer = 0;                                                                         // T(n) = O(1), S(n) = O(1)
        int left = 0;                                                                          // T(n) = O(1), S(n) = O(1)

        for(int i = s; i <= e; i++) {                                                          // T(n) = O(N)
            if(expression.charAt(i) == '{' && ++layer == 1) {                                  // T(n) = O(1), S(n) = O(1)
                left = i + 1; // T(n) = O(1), S(n) = O(1)
            }else if(expression.charAt(i) == '}' && --layer == 0) {                            // T(n) = O(1), S(n) = O(1)
                merge(groups, dfs(expression, left, i - 1));                                   // T(n) = O(WL log W), S(n) = O(WL + N)
            }else if(expression.charAt(i) == ',' && layer == 0) {                              // T(n) = O(1), S(n) = O(1)
                groups.add(new ArrayList<>());                                                 // T(n) = O(1), S(n) = O(1)
            }else if(layer == 0) {                                                             // T(n) = O(1), S(n) = O(1)
                merge(groups, new ArrayList<>(List.of(String.valueOf(expression.charAt(i))))); // T(n) = O(1), S(n) = O(1)
            }
        }
        
        for(final List<String> group : groups) {                                               // T(n) = O(W)
            for(final String word : group) {                                                   // T(n) = O(W)
                ans.add(word);                                                                 // T(n) = O(L log W), S(n) = O(L)
            } 
        }
        return new ArrayList<>(ans);                                                           // T(n) = O(WL), S(n) = O(WL)
    }

    void merge(List<List<String>> groups, List<String> group) {
        if(groups.get(groups.size() - 1).isEmpty()) {                                          // T(n) = O(1), S(n) = O(1)
            groups.set(groups.size() - 1, group);                                              // T(n) = O(1), S(n) = O(1)
            return;                                                                            // T(n) = O(1), S(n) = O(1)
        }

        List<String> mergedGroup = new ArrayList<>();                                          // T(n) = O(1), S(n) = O(1)

        for(final String word1 : groups.get(groups.size() - 1)) {                              // T(n) = O(A)
            for(final String word2 : group) {                                                  // T(n) = O(A * B)
                mergedGroup.add(word1 + word2);                                                // T(n) = O(L), S(n) = O(L)
            } 
        } 
        groups.set(groups.size() - 1, mergedGroup);                                            // T(n) = O(1), S(n) = O(1)
    }
}
```

# Time Complexity:

T(n) = O(N + G · L + W · L log W), where:
- N = length of the input expression.
- W = number of generated words.
- L = maximum length of a generated word.
- G = total number of words generated by intermediate Cartesian-product merges.

# Space Complexity:

S(n) = O(G · L + W · L + N), where:
- N = length of the input expression.
- W = number of generated words.
- L = maximum length of a generated word.
- G = total number of words generated by intermediate Cartesian-product merges.
