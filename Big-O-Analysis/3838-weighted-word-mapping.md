# 3838. Weighted Word Mapping

# Problem Summary:
You are given an array of strings words, an integer array weights of length 26, where weights[i] represents the weight of the ith lowercase English letter. The weight of a word is the sum of the weights of all its characters. For each word:
1) Calculate its total weight.
2) Take the result modulo 26.
3) Convert the result into a lowercase English letter using reverse alphabetical order: 0 -> 'z', 1 -> 'y', 2 -> 'x', ...,25 -> 'a'
Return a string formed by concatenating the mapped character for every word in the original order.

# Approach Used:
The solution processes each word independently. For a given word:
* Traverse all its characters.
* Use the weights array to obtain the weight of each character.
* Compute the total weight of the word.
* Take the total modulo 26.
* Convert the resulting value into the corresponding reverse-alphabetical character.
The mapping is performed using ASCII values:
* 'z' has ASCII value 122.
* Therefore, the mapped character can be computed as: 122 − (wordWeight % 26)
The resulting character is appended to a StringBuilder. After all words have been processed, the final string is returned.

# Steps:
1) Create a StringBuilder to store the result
2) Traverse each word in the words array
3) For the current word:
   -> Initialize a running sum to 0
   -> Traverse all characters
   -> Add the corresponding character weight to the sum
4) Compute:
   -> sum % 26
5) Convert the result into a reverse-alphabetical character
6) Append the character to the StringBuilder
7) Return the final string

# Solution:
```
class Solution {

    public String mapWordWeights(String[] words, int[] weights) {

        StringBuilder sb = new StringBuilder();                // T(n) = O(1), S(n) = O(m)
        
        // Loop runs once for each word
        for(String word : words) {
            char[] chars = word.toCharArray();                // T(n) = O(length of word), S(n) = O(length of word)
            int sum = 0;                                      // T(n) = O(1), S(n) = O(1)

            // Loop runs length(word) times
            for(char c : chars) {
                sum += weights[c - 'a'];                      // T(n) = O(1) per execution
            }

            sum %= 26;                                        // T(n) = O(1)
            char c = (char)(122 - sum);                       // T(n) = O(1)
            sb.append(c);                                     // T(n) = O(1)
        }
        return sb.toString();                                 // T(n) = O(m)
    }
}
```

# Time Complexity:
Let:
-> n = number of words
-> L = total number of characters across all words
T(n, L) = O(L)

# Space Complexity:
Let:
-> n = number of words (output size)
-> m = length of the longest word
S(n) = O(m)
