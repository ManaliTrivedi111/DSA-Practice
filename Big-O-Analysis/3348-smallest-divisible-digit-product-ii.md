# 3348. Smallest Divisible Digit Product II

# Problem Summary:

You are given a string num representing a positive integer and an integer t. Return the smallest zero-free number greater than or equal to num such that the product of its digits is divisible by t.
A zero-free number is a number that does not contain the digit 0.

# Approach Used:

The solution is based on the fact that every non-zero digit can only contribute the prime factors 2, 3, 5, and 7 to the digit product. Therefore, if t contains any other prime factor, no answer is possible.

The solution first factorizes t into counts of 2, 3, 5, and 7. It then determines the minimum number of digits required to satisfy those prime-factor requirements.

If num is too short, the solution directly constructs the smallest possible number with enough digits.

Otherwise, it first tries to keep the original number unchanged. If that is not possible, it works from right to left, increases one digit, and constructs the smallest possible suffix that satisfies the remaining prime-factor requirements.

# Steps:

1. Factorize t using the only possible prime factors of a digit product: 2, 3, 5, and 7.
2. If anything remains in t after this factorization, return "-1" because no digit product can contain that prime factor.
3. Calculate the minimum number of digits required to satisfy the prime-factor counts using getMinLength().
4. If num has fewer digits than this minimum length, construct the smallest valid number using buildSuffix().
5. Otherwise, copy the digits of num into result until a 0 is encountered.
6. Remove the prime-factor contribution of the copied digits from primeCount.
7. If all required prime factors have already been satisfied, the current number is valid. Fill any remaining positions with 1 and return it.
8. Otherwise, work from right to left and try increasing a digit.
9. After increasing a digit, remove its prime-factor contribution from primeCount.
10. Check whether the remaining prime factors can fit into the remaining positions using getMinLength().
11. If they can fit, construct the smallest possible suffix using buildSuffix() and return the result.
12. If no digit can be increased while keeping the number valid, construct the smallest valid number with one additional digit.
13. Return the resulting number.

# Solution:

```
class Solution {

    int primes[] = {2, 3, 5, 7};                                             // T(n) = O(1), S(n) = O(1)

    public String smallestNumber(String num, long t) {
        int primeCount[] = new int[8];                                       // T(n) = O(1), S(n) = O(1)
        char[] n = num.toCharArray();                                        // T(n) = O(n), S(n) = O(n)
        int numLength = n.length;                                            // T(n) = O(1), S(n) = O(1)
        int firstZeroIdx = 0;                                                // T(n) = O(1), S(n) = O(1)


        for(int prime : primes) {
            while(t % prime == 0) {
                t /= prime;                                                  // T(n) = O(log t), S(n) = O(1)
                primeCount[prime]++;                                         // T(n) = O(1), S(n) = O(1)
            }
        }


        if(t != 1) {
            return "-1";                                                     // T(n) = O(1), S(n) = O(1)
        }

        int minLength = getMinLength(primeCount);                            // T(n) = O(1), S(n) = O(1)

        if(numLength < minLength) {
            return buildSuffix(primeCount, minLength, new char[minLength]);  // T(n) = O(n + log t), S(n) = O(n)
        }

        char[] result = new char[numLength + 1];                             // T(n) = O(n), S(n) = O(n)

        for(int i = 1; (firstZeroIdx < numLength)
                && ((result[i] = n[firstZeroIdx]) != '0'); firstZeroIdx++, i++) {

            
            if(result[i] != '1') {
                logNum(primeCount, result[i], -1);                           // T(n) = O(1), S(n) = O(1)
            }
        }

        if(getMinLength(primeCount) == 0) {                                  // T(n) = O(1), S(n) = O(1)
            if(firstZeroIdx == numLength) {
                return num;                                                  // T(n) = O(1), S(n) = O(1)
            }
            
            Arrays.fill(result, ++firstZeroIdx, result.length, '1');         // T(n) = O(n), S(n) = O(1)
            
            return new String(result, 1, numLength);                         // T(n) = O(n), S(n) = O(n)
        }


        for(int last = numLength - 1, end = Math.min(firstZeroIdx, last); end >= 0; end--) {
            logNum(primeCount, result[end + 1], 1);                          // T(n) = O(1), S(n) = O(1)

            while(++result[end + 1] <= '9') {
                logNum(primeCount, result[end + 1], -1);                     // T(n) = O(1), S(n) = O(1)

                if(getMinLength(primeCount) <= last - end) {                 // T(n) = O(1), S(n) = O(1)
                    return buildSuffix(primeCount, last - end, result);      // T(n + log t), S(n)
                }

                logNum(primeCount, result[end + 1], 1);                      // T(n) = O(1), S(n) = O(1)
            }
        }

        return buildSuffix(primeCount, result.length, result);               // T(n + log t), S(n)
    }


    private void logNum(int[] primeCount, char num, int value) {
        if(num == '9') {
            primeCount[3] += (value * 2);                                    // T(n) = O(1), S(n) = O(1)

        }else if (num == '4') {
            primeCount[2] += (value * 2);                                    // T(n) = O(1), S(n) = O(1)

        }else if (num == '8') {
            primeCount[2] += (value * 3);                                    // T(n) = O(1), S(n) = O(1)

        }else if (num == '6') {
            primeCount[2] += value;                                          // T(n) = O(1), S(n) = O(1)
            primeCount[3] += value;                                          // T(n) = O(1), S(n) = O(1)

        }else {
            primeCount[num - '0'] += value;                                  // T(n) = O(1), S(n) = O(1)
        }
    }

    private String buildSuffix(int[] primeCount, int targetLength, char[] result) {
        int index = result.length;                                           // T(n) = O(1), S(n) = O(1)

        while(primeCount[3] > 1) {
            primeCount[3] -= 2;                                              // T(n) = O(log t), S(n) = O(1)
            result[--index] = '9';                                           // T(n) = O(1), S(n) = O(1)
        }


        while(primeCount[2] > 2) {
            primeCount[2] -= 3;                                              // T(n) = O(log t), S(n) = O(1)
            result[--index] = '8';                                           // T(n) = O(1), S(n) = O(1)
        }

        while(primeCount[7]-- > 0) {
            result[--index] = '7';                                           // T(n) = O(log t), S(n) = O(1)
        }

        if(primeCount[2] > 0 && primeCount[3] > 0) {
            result[--index] = '6';                                           // T(n) = O(1), S(n) = O(1)
            primeCount[2]--;                                                 // T(n) = O(1), S(n) = O(1)
            primeCount[3]--;                                                 // T(n) = O(1), S(n) = O(1)
        }

        while(primeCount[5]-- > 0) {
            result[--index] = '5';                                           // T(n) = O(log t), S(n) = O(1)
        }

        while(primeCount[2] > 1) {
            primeCount[2] -= 2;                                              // T(n) = O(log t), S(n) = O(1)
            result[--index] = '4';                                           // T(n) = O(1), S(n) = O(1)
        }

        while(primeCount[3] > 0) {
            primeCount[3]--;                                                 // T(n) = O(log t), S(n) = O(1)
            result[--index] = '3';                                           // T(n) = O(1), S(n) = O(1)
        } 

        while(primeCount[2] > 0) 
            primeCount[2]--;                                                 // T(n) = O(log t), S(n) = O(1)
            result[--index] = '2';                                           // T(n) = O(1), S(n) = O(1)
        }

        while(index + targetLength != result.length) {
            result[--index] = '1';                                           // T(n) = O(n), S(n) = O(1)
        }

        return targetLength == result.length
                ? new String(result)                                         // T(n) = O(n), S(n) = O(n)
                : new String(result, 1, result.length - 1);                  // T(n) = O(n), S(n) = O(n)
    }


    private int getMinLength(int[] primeCount) {
        int count2 = Math.max(0, primeCount[2]);                             // T(n) = O(1), S(n) = O(1)
        int count3 = Math.max(0, primeCount[3]);                             // T(n) = O(1), S(n) = O(1)
        int count23 = (count3 & 1) + (count2 % 3);                           // T(n) = O(1), S(n) = O(1)


        return (count3 / 2) + (count2 / 3)
                + Math.max(0, primeCount[7])
                + Math.max(0, primeCount[5])
                + (count23 >= 2 ? (count23 - 1) : count23);                  // T(n) = O(1), S(n) = O(1)
    }
}
```

# Time Complexity:
T(n) = O(n + log t)

# Space Complexity:
S(n) = O(n)
