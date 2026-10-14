# 1807. Evaluate the Bracket Pairs of a String

# Problem Summary

You are given a string s containing bracket pairs, where each pair contains a non-empty key. You are also given knowledge, which contains key-value pairs.

For every bracket pair:
- If its key exists in knowledge, replace the entire bracket pair with its corresponding value.
- If its key does not exist, replace the entire bracket pair with ?.

Return the resulting string after evaluating all bracket pairs.

# Approach Used

The solution first stores all key-value pairs from knowledge in a HashMap for efficient lookup.

It then scans the string from left to right:
1. When it encounters (, it finds the corresponding ) and extracts the key between them.
2. It looks up the key in the HashMap.
3. If the key exists, its value is appended to the result; otherwise, ? is appended.
4. When it encounters normal text, it finds the next ( and appends that entire substring to the result.
5. The scan continues until the entire string has been processed.

Finally, the StringBuilder is converted to a String.

# Steps

1. Create a HashMap containing every key-value pair from knowledge.
2. Convert s to a character array for character access.
3. Use i to scan through s.
4. If s[i] is (:
   - Use j to find the corresponding ).
   - Extract the key using substring.
   - Look up the key in the map.
   - Append its value or ? to the result.
   - Move i past the closing ).
5. Otherwise:
   - Use j to find the next (.
   - Append the intervening text to the result.
   - Move i to j.
6. Return the completed string.

# Solution

```
class Solution {
    public String evaluate(String s, List<List<String>> knowledge) {
        Map<String, String> map = new HashMap<>();                           // T(n) = O(K), S(n) = O(K)

        for(List<String> k : knowledge) {                                    // T(n) = O(K)
            map.put(k.get(0), k.get(1));                                     // T(n) = O(1), S(n) = O(1)
        }

        StringBuilder sb = new StringBuilder();                              // T(n) = O(1), S(n) = O(1)
        char[] str = s.toCharArray();                                        // T(n) = O(N), S(n) = O(N)
        final int n = str.length;                                            // T(n) = O(1), S(n) = O(1)
        int i = 0;                                                           // T(n) = O(1), S(n) = O(1)

        while(i < n) {                                                       // T(n) = O(N)
            if(str[i] == '(') {                                              // T(n) = O(1), S(n) = O(1)
                int j = i;                                                   // T(n) = O(1), S(n) = O(1)

                while(j < n && str[j] != ')') {                              // T(n) = O(N) total
                    j++;                                                     // T(n) = O(1), S(n) = O(1)
                }

                String sub = s.substring(i + 1, j);                          // T(n) = O(L), S(n) = O(L)
                
                if(map.containsKey(sub)) {                                   // T(n) = O(L), S(n) = O(1)
                    sb.append(map.get(sub));                                 // T(n) = O(V), S(n) = O(V)
                }else{                                                       // T(n) = O(1), S(n) = O(1)
                    sb.append('?');                                          // T(n) = O(1), S(n) = O(1)
                }

                i = j + 1;                                                   // T(n) = O(1), S(n) = O(1)
            }else{
                int j = i;                                                   // T(n) = O(1), S(n) = O(1)

                while(j < n && str[j] != '(') {                              // T(n) = O(N) total
                    j++;                                                     // T(n) = O(1), S(n) = O(1)
                }

                sb.append(s.substring(i, j));                                // T(n) = O(L), S(n) = O(L)
                i = j;                                                       // T(n) = O(1), S(n) = O(1)
            }
        }
        return sb.toString();                                                // T(n) = O(R), S(n) = O(R)
    }
}
```

# Time Complexity:

T(n) = O(N + K + R), where:
- N = length of the input string s.
- K = number of key-value pairs in knowledge.
- R = length of the resulting string.

# Space Complexity:

S(n) = O(N + K + R), where:
- N = length of the input string s.
- K = number of key-value pairs in knowledge.
- R = length of the resulting string.

