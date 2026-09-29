# 3483. Unique 3-Digit Even Numbers

# Problem Summary:

You are given an array of digits called digits. The task is to determine the number of distinct three-digit even numbers that can be formed using the given digits.

The rules are:
* The number must have exactly three digits, so its first digit cannot be 0.
* The number must be even, so its last digit must be an even digit.
* Each copy of a digit can be used at most once in a number.
* Different arrangements of the same digits can produce different numbers.
* Duplicate numbers formed using different copies of the same digit should be counted only once.

# Approach Used:

The solution uses three nested loops to choose the three positions of the number:
* i chooses the hundreds digit.
* j chooses the tens digit.
* k chooses the units digit.

For the hundreds digit, the solution skips 0 because a three-digit number cannot have a leading zero.

For the tens digit, the solution skips the index already used for the hundreds digit.

For the units digit, the solution skips indices already used for the hundreds and tens digits. It also checks whether the digit is even using: digits[k] % 2 == 0

Once three valid digits are selected, the number is constructed as: digits[i] * 100 + digits[j] * 10 + digits[k]

The number is added to a HashSet.

Using a HashSet is important because the input array may contain duplicate copies of the same digit. Different index combinations can therefore produce the same three-digit number. The set ensures that each distinct number is counted only once.

# Steps:

1. Create a HashSet<Integer> to store all distinct valid numbers.
2. Let n be the number of digits in the input array.
3. Use the first loop to select the hundreds digit.
4. Skip the digit if it is 0 because leading zeros are not allowed.
5. Use the second loop to select the tens digit.
6. Skip the index already used for the hundreds digit.
7. Use the third loop to select the units digit.
8. Skip indices already used by the hundreds or tens digits.
9. Skip the units digit if it is odd because the number must be even.
10. Construct the three-digit number.
11. Add the number to the HashSet, which automatically removes duplicates.
12. Return the size of the set.

# Solution:

```
class Solution {
    public int totalNumbers(int[] digits) {
        Set<Integer> set = new HashSet<>();                                  // T(n) = O(1), S(n) = O(1)
        final int n = digits.length;                                         // T(n) = O(1), S(n) = O(1)

        for(int i = 0; i < n; i++) {                                         // T(n) = O(n)
            if(digits[i] == 0) {                                             // T(n) = O(1), S(n) = O(1)
                continue;                                                    // T(n) = O(1), S(n) = O(1)
            }

            for(int j = 0; j < n; j++) {                                     // T(n) = O(n) for each i
                if(j == i) {                                                 // T(n) = O(1), S(n) = O(1)
                    continue;                                                // T(n) = O(1), S(n) = O(1)
                }

                for(int k = 0; k < n; k++) {                                 // T(n) = O(n) for each (i, j)
                    if(k == i || k == j || (digits[k] % 2 != 0)) {           // T(n) = O(1), S(n) = O(1)
                        continue;                                            // T(n) = O(1), S(n) = O(1)
                    }

                    set.add(digits[i] * 100 + digits[j] * 10 + digits[k]);   // T(n) = O(1) average, S(n) = O(1) per element
                }
            }
        }
        return set.size();                                                   // T(n) = O(1), S(n) = O(1)
    }
}
```

# Time Complexity:
T(n) = O(n³)

# Space Complexity:
S(n) = O(1)
