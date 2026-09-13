# 3734. Lexicographically Smallest Palindromic Permutation Greater Than Target

# Problem Summary:

You are given two strings s and target, each of length n, consisting of lowercase English letters.

The goal is to find the lexicographically smallest string that is both a palindromic permutation of s and strictly greater than target.

If no such permutation exists, return an empty string.

# Approach Used:

The solution first counts the frequency of every character in s and checks whether a palindromic permutation is possible. At most one character can have an odd frequency; that character becomes the middle character of the palindrome. The remaining frequencies are divided by two because only the first half of the palindrome needs to be constructed.

It then tries to construct the smallest possible first half that can lead to a palindrome greater than target. It initially follows target as closely as possible using the available characters. If it becomes impossible to continue while staying equal to the target prefix, it backtracks to an earlier position and tries to place the smallest available character that is greater than the corresponding target character.

Once the first half is constructed, palind() creates the complete palindrome. The solution first checks whether this palindrome is already greater than target. If not, it generates the next lexicographical permutation of the first half using next_perm(), constructs the palindrome again, and checks it once more.

# Steps:

1. Count the frequency of each character in s.
2. Check the frequencies to determine whether a palindromic permutation is possible.
   * If more than one character has an odd frequency, return "".
   * Store the single odd-frequency character as mid if one exists.
   * Divide every frequency by two to obtain the characters needed for the first half.
3. Build the first half of the palindrome while trying to match target from left to right.
4. If the target character is available, use it and continue.
5. If the target character is unavailable, try a larger available character. Once one is chosen, the prefix is already greater than the target, so fill the remaining positions with the smallest available characters.
6. If no larger character can be chosen at the current position, backtrack through the previously constructed prefix and try to increase an earlier character.
7. Construct the complete palindrome using the first half and the middle character.
8. If the palindrome is strictly greater than target, return it.
9. Otherwise, generate the next lexicographical permutation of the first half, construct the palindrome again, and check it.
10. If neither candidate is greater than target, return "".

# Solution:

```
class Solution {
    public String lexPalindromicPermutation(String s, String target) {
        int[] a = new int[26];                                               // T(n) = O(1), S(n) = O(1)

        for(char ch : s.toCharArray()) {                                     // T(n) = O(n)
            a[ch - 'a']++;                                                   // T(n) = O(1), S(n) = O(1)
        }

        char mid = '.';                                                      // T(n) = O(1), S(n) = O(1)

        for(int i = 0; i < 26; i++) {                                        // T(n) = O(1)
            if((a[i]&1) == 1) { 
                if(mid != '.') { 
                    return "";                                               // T(n) = O(1)
                }
                mid = (char) (i + (int)'a');                                 // T(n) = O(1)
            }
            a[i] /= 2;                                                       // T(n) = O(1), S(n) = O(1)
        }

        StringBuilder str = new StringBuilder();                             // T(n) = O(1), S(n) = O(1)
        int flag = 0;                                                        // T(n) = O(1), S(n) = O(1)

        for(int i = 0; i < s.length() / 2; i++) {                            // T(n) = O(n)
            char ch = target.charAt(i);                                      // T(n) = O(1), S(n) = O(1)

            if(flag == 0) { 
                boolean ok  = false;                                         // T(n) = O(1), S(n) = O(1)

                if(a[ch - 'a'] > 0) { 
                    a[ch - 'a']--;                                           // T(n) = O(1), S(n) = O(1)
                    str.append(ch);                                          // T(n) = O(1), S(n) = O(1)
                    continue;                                                // T(n) = O(1), S(n) = O(1)
                }

                for(int j = ch - 'a'; j < 26; j++) {                         // T(n) = O(1)
                    if(a[j] > 0) {
                        a[j]--;                                              // T(n) = O(1)
                        ok = true;                                           // T(n) = O(1)
                        str.append((char) (j + 'a'));                        // T(n) = O(1)
                        break;                                               // T(n) = O(1)
                    }
                }

                if(ok) { 
                    flag = 1;                                                // T(n) = O(1), S(n) = O(1)
                    continue;                                                // T(n) = O(1), S(n) = O(1)
                }

                for(int j = i - 1; j >= 0 && !ok; j--) {                     // T(n) = O(n)
                    for(int k = 0; k < 26; k++) {                            // T(n) = O(1)
    
                        if(k > str.charAt(j) - 'a' && a[k] > 0) {      
                            a[str.charAt(j) - 'a']++;                        // T(n) = O(1)
                            str.setCharAt(j, (char) (k + 'a'));              // T(n) = O(1)
                            a[k]--;                                          // T(n) = O(1)
                            ok = true;                                       // T(n) = O(1)
                            break;                                           // T(n) = O(1)
                        }
                    }

                    if(ok) {
                        i = j;                                               // T(n) = O(1)
                        flag = 1;                                            // T(n) = O(1)
                        break;                                               // T(n) = O(1)
                    }
                    
                    a[str.charAt(j) - 'a']++;                                // T(n) = O(1)
                    str.deleteCharAt(str.length() - 1);                      // T(n) = O(n)
                }

                if(!ok) {
                    return "";                                               // T(n) = O(1)
                }
            }else {
                for(int j = 0; j < 26; j++) {                                // T(n) = O(1)
                    if(a[j] > 0) {
                        a[j]--;                                              // T(n) = O(1)
                        str.append((char) (j + 'a'));                        // T(n) = O(1)
                        break;                                               // T(n) = O(1)
                    }
                }
            }
            
        }

        StringBuilder con = palind(str, mid);                                // T(n) = O(n)

        if(comp(con, new StringBuilder(target)) == 1) { 
            return con.toString();                                           // T(n) = O(n)
        }
        
        next_perm(str);                                                      // T(n) = O(n)
        con = palind(str, mid);                                              // T(n) = O(n), S(n) = O(n)

        if(comp(con, new StringBuilder(target)) == 1) {
            return con.toString();                                           // T(n) = O(n)
        }
        return "";                                                           // T(n) = O(1)

    }

    int comp(StringBuilder s1, StringBuilder s2) {
        for(int i = 0; i < s1.length(); i++) {                               // T(n) = O(n)
            if(s1.charAt(i) == s2.charAt(i)) {
                continue;                                                    // T(n) = O(1)
            }
            return s1.charAt(i) < s2.charAt(i) ? -1 : 1;                     // T(n) = O(1)
        }
        return 0;                                                            // T(n) = O(1)
    }

    StringBuilder palind(StringBuilder s, char mid) {
        StringBuilder con = new StringBuilder();                             // T(n) = O(1), S(n) = O(1)
        con.append(s);                                                       // T(n) = O(n), S(n) = O(n)

        if(mid != '.') {
            con.append(mid);                                                 // T(n) = O(1), S(n) = O(1)
        }
        s.reverse();                                                         // T(n) = O(n), S(n) = O(1)
        con.append(s);                                                       // T(n) = O(n), S(n) = O(n)
        s.reverse();                                                         // T(n) = O(n), S(n) = O(1)
        return con;                                                          // T(n) = O(1), S(n) = O(1)
    }

    void swap(StringBuilder s, int i, int j) {
        s.setCharAt(i, (char) (s.charAt(i) ^ s.charAt(j)));                  // T(n) = O(1), S(n) = O(1)
        s.setCharAt(j, (char) (s.charAt(i) ^ s.charAt(j)));                  // T(n) = O(1), S(n) = O(1)
        s.setCharAt(i, (char) (s.charAt(i) ^ s.charAt(j)));                  // T(n) = O(1), S(n) = O(1)
    }

    void next_perm(StringBuilder s) {
        int n = s.length();                                                  // T(n) = O(1), S(n) = O(1)
        int pos = -1;                                                        // T(n) = O(1), S(n) = O(1)

        for(int i = n -2; i >= 0; i--) {                                     // T(n) = O(n)
            if(s.charAt(i) < s.charAt(i + 1)) {
                pos = i;                                                     // T(n) = O(1)
                break;                                                       // T(n) = O(1)
            }
        }
        if(pos == -1) {
            return;                                                          // T(n) = O(1)
        }
        for(int i = n - 1; i >= 0; i--) {                                    // T(n) = O(n)
            if(s.charAt(i) > s.charAt(pos)) {
                swap(s, i, pos);                                             // T(n) = O(1)

                for(int j = pos + 1, r = n - 1; j < r; j++, r--) {           // T(n) = O(n)
                    swap(s, j, r);                                           // T(n) = O(1)
                }
                break;                                                       // T(n) = O(1)
            }
        }
    }
}
```

# Time Complexity:
T(n) = O(n²)

# Space Complexity:
S(n) = O(n)
