# 860. Lemonade Change

# Problem Summary: At a lemonade stand, each lemonade costs $5. Customers are standing in a queue to buy from you and order one at a time in the order specified by the array bills. Each customer pays using either a $5, $10, or $20 bill. The goal is to determine whether correct change can be provided to every customer so that each lemonade effectively costs $5. Initially, no change is available.

# Approach Used: The solution keeps track of the number of $5 and $10 bills available while processing each customer one by one.
If a customer pays with $5, no change is required.
If a customer pays with $10, one $5 bill must be given as change.
If a customer pays with $20, the solution prioritizes giving one $10 bill and one $5 bill as change because it preserves more $5 bills for future transactions. If that is not possible, three $5 bills are used instead.
If correct change cannot be provided at any point, the method immediately returns false.

# Steps:
1) Initialize counters for $5 bills and $10 bills
2) Traverse each bill in the array
3) If the bill is $5:
   Increase the count of $5 bills
4) If the bill is $10:
   Check whether at least one $5 bill is available
   If not available -> return false
   Otherwise:
   Give one $5 bill as change
   Increase the count of $10 bills
5) If the bill is $20:
   First try to give one $10 bill and one $5 bill as change
   If not possible:
   Try to give three $5 bills
   If neither option is possible -> return false
6) If all customers are processed successfully -> return true

# Solution:
class Solution {

    public boolean lemonadeChange(int[] bills) {
        int five = 0;                                         // T(n) = O(1), S(n) = O(1)
        int ten = 0;                                          // T(n) = O(1), S(n) = O(1)

        // Loop runs n times
        for(int bill : bills) {

            if(bill == 5) {                                   // T(n) = O(1) per execution, total executions = O(n)
                five++;                                       // T(n) = O(1) per execution, total executions = O(n)

            }else if(bill == 10) {                            // T(n) = O(1) per execution, total executions = O(n)

                if(five == 0) {                               // T(n) = O(1) per execution, total executions = O(n)
                    return false;                             // T(n) = O(1)
                }

                five--;                                       // T(n) = O(1) per execution, total executions = O(n)
                ten++;                                        // T(n) = O(1) per execution, total executions = O(n)

            }else{

                if(ten > 0) {                                 // T(n) = O(1) per execution, total executions = O(n)

                    if(five == 0) {                           // T(n) = O(1) per execution, total executions = O(n)
                        return false;                         // T(n) = O(1)
                    }

                    ten--;                                    // T(n) = O(1) per execution, total executions = O(n)
                    five--;                                   // T(n) = O(1) per execution, total executions = O(n)

                }else{

                    if(five < 3) {                            // T(n) = O(1) per execution, total executions = O(n)
                        return false;                         // T(n) = O(1)
                    }

                    five -= 3;                                // T(n) = O(1) per execution, total executions = O(n)
                }
            }
        }
        return true;                                          // T(n) = O(1)
    }
}

# Time Complexity:
T(n) = O(n)

# Space Complexity:
S(n) = O(1)
