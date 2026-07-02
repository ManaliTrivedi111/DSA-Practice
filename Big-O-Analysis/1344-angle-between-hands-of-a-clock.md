# 1344. Angle Between Hands of a Clock

# Problem Summary:
Given the current hour and minutes on an analog clock, return the smaller angle (in degrees) between the hour hand and the minute hand. The answer is accepted if it is within 10⁻⁵ of the actual value.

# Approach Used:
The solution calculates the position of both clock hands relative to the 12 o'clock position and then finds the absolute difference between them. The minute hand moves 6 degrees every minute since it completes 360 degrees in 60 minutes. The hour hand also moves continuously. Each hour mark represents 30 degrees, and the hour hand advances slightly every minute. Instead of directly calculating the angle in degrees, the solution first converts the hour hand's position into minute divisions on the clock:
* Each hour corresponds to 5 minute divisions.
* Every 12 minutes move the hour hand by one minute division.
The resulting position is then multiplied by 6 to convert it into degrees. Finally, the absolute difference between the two angles is computed, and the smaller of the two possible angles is returned.

# Steps:
1. If the hour is 12, treat it as 0.
2. Calculate the hour hand's angle from the 12 o'clock position.
3. Calculate the minute hand's angle from the 12 o'clock position.
4. Find the absolute difference between the two angles.
5. Return the smaller of:
   * the calculated difference
   * 360 minus the difference

# Solution:

```
class Solution {
    public double angleClock(int hour, int minutes) {
        if(hour == 12) {
            hour = 0;                                                             // T(n) = O(1)
        }

        double hrAngleWithZero = ((hour * 5) + ((double) minutes / 12)) * 6;      // T(n) = O(1), S(n) = O(1)
        double minAngleWithZero = minutes * 6;                                    // T(n) = O(1), S(n) = O(1)
        double diff = Math.abs(hrAngleWithZero - minAngleWithZero);               // T(n) = O(1), S(n) = O(1)
        return Math.min(diff, 360 - diff);                                        // T(n) = O(1)
    }
}
```

# Time Complexity:
T(n) = O(1)

# Space Complexity:
S(n) = O(1)
