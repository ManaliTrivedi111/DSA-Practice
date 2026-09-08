# 1927. Sum Game

# Problem Summary:

You are given an even-length string num containing digits and '?' characters. Alice and Bob take turns replacing '?' with digits from 0 to 9, with Alice going first.

At the end of the game:
* Bob wins if the sum of the digits in the first half equals the sum of the digits in the second half.
* Alice wins if the two sums are different.

# Approach Used:

The solution divides the string into two equal halves and calculates two values for each half:
* The sum of the known digits.
* The number of '?' characters.

Let:
* sum1 = sum of known digits in the first half.
* sum2 = sum of known digits in the second half.
* count1 = number of '?' characters in the first half.
* count2 = number of '?' characters in the second half.

There are two important cases:

1. Odd number of question marks

If count1 + count2 is odd, Alice makes the last move.
In this situation, Alice can always choose a digit that prevents the two sums from being equal.
Therefore, Alice wins and we return true.

2. Even number of question marks

If the total number of question marks is even, both players make the same number of moves.
The difference between the number of unknowns on the two sides determines how much the sums can be shifted.
The maximum possible adjustment is: ((count2 - count1) * 9) / 2

Bob can force equality only when the existing sum difference exactly matches this required adjustment.

Therefore, if sum1 - sum2 == ((count2 - count1) * 9) / 2, Bob wins, so we return false.
Otherwise, Alice wins and we return true.

# Steps:

1. Find the length n of the string.
2. Split the string into two equal halves.
3. Call getSumAndCount() for the first half.
4. Call getSumAndCount() for the second half.
5. Extract the known digit sums and question-mark counts for both halves.
6. If the total number of question marks is odd, return true because Alice can force the sums to be unequal.
7. Otherwise, calculate the required adjustment: ((count2 - count1) * 9) / 2.
8. Compare this adjustment with sum1 - sum2.
9. If they are equal, Bob can force equality, so return false.
10. Otherwise, Alice wins, so return true.

# Solution:

```
class Solution {
    public boolean sumGame(String num) {
        final int n = num.length();                                          // T(n) = O(1), S(n) = O(1)
        int[] left = getSumAndCount(num.substring(0, n / 2));                // T(n) = O(n), S(n) = O(n)
        int[] right = getSumAndCount(num.substring(n / 2));                  // T(n) = O(n), S(n) = O(n)

        int sum1 = left[0];                                                  // T(n) = O(1), S(n) = O(1)
        int count1 = left[1];                                                // T(n) = O(1), S(n) = O(1)
        int sum2 = right[0];                                                 // T(n) = O(1), S(n) = O(1)
        int count2 = right[1];                                               // T(n) = O(1), S(n) = O(1)

        if((count1 + count2) % 2 != 0) {                                     // T(n) = O(1)
            return true;                                                     // T(n) = O(1)
        }
        return (sum1 - sum2) != ((count2 - count1) * 9) / 2;                 // T(n) = O(1)
    }

    private int[] getSumAndCount(String num) {
        char[] str = num.toCharArray();                                      // T(n) = O(n), S(n) = O(n)
        int sum = 0;                                                         // T(n) = O(1), S(n) = O(1)
        int count = 0;                                                       // T(n) = O(1), S(n) = O(1)
        
        for(char c : str) {                                                  // T(n) = O(n), S(n) = O(1)
            if(c == '?') {                                                   
                count++;                                                     // T(n) = O(1) per execution
            }else{
                sum += c - '0';                                              // T(n) = O(1) per execution
            }
        }
        return new int[]{sum, count};                                        // T(n) = O(1), S(n) = O(1)
    }
}

# Time Complexity:
T(n) = O(n)

# Space Complexity:
S(n) = O(n)
