# LEETCODE-Array-1552
Let's walk through the `maxDistance` function step by step. The goal of this function is to determine the largest minimum distance between any two balls that can be placed in the sorted positions array, such that there are exactly `m` balls.

Here's a detailed breakdown of the code:

1. **Sorting the positions:**
   ```java
   Arrays.sort(position);
   ```
   First, we sort the `position` array to facilitate easy computation of distances between the balls.

2. **Setting the initial search boundaries:**
   ```java
   int l = 1;
   int r = position[position.length - 1] - position[0];
   ```
   - `l` is initialized to 1 because the minimum possible distance between any two balls is 1.
   - `r` is initialized to the distance between the first and the last position in the sorted array, as this is the maximum possible distance.

3. **Binary search loop:**
   ```java
   while (l < r) {
     final int mid = r - (r - l) / 2;
     if (numBalls(position, mid) >= m) // There're too many balls.
       l = mid;
     else // There're too few balls.
       r = mid - 1;
   }
   ```
   - We use binary search to find the largest minimum distance.
   - `mid` is calculated as the midpoint between `l` and `r`. This is our candidate distance to test.
   - `numBalls(position, mid)` checks how many balls can be placed with at least `mid` distance between each of them.
   - If the number of balls placed (`numBalls`) is greater than or equal to `m`, it means we can try a larger distance, so we move `l` up to `mid`.
   - Otherwise, we reduce `r` to `mid - 1` to try a smaller distance.

4. **Return the result:**
   ```java
   return l;
   ```
   - When the loop ends, `l` will be the largest minimum distance where we can place `m` balls.

5. **Helper function to count balls:**
   ```java
   private int numBalls(int[] position, int force) {
     int balls = 0;
     int prevPosition = -force;
     for (final int pos : position)
       if (pos - prevPosition >= force) {
         balls++;
         prevPosition = pos;
       }
     return balls;
   }
   ```
   - This function counts how many balls can be placed such that each ball is at least `force` distance apart.
   - It initializes `balls` to 0 and `prevPosition` to `-force` (to ensure the first ball is always placed).
   - It iterates through the `position` array, and if the current position minus `prevPosition` is at least `force`, it places a ball there and updates `prevPosition`.

### Dry Run Example
Let's go through a dry run with an example:

- **Input:**
  ```java
  int[] position = {1, 2, 8, 4, 9};
  int m = 3;
  ```
- **Sorted position array:**
  ```java
  position = [1, 2, 4, 8, 9];
  ```

- **Initial boundaries:**
  - `l = 1`
  - `r = 8` (position[4] - position[0])

- **First iteration:**
  - `mid = 5`
  - `numBalls(position, 5)` returns 2 (we can place balls at positions 1 and 8).
  - Since 2 < 3, update `r` to `mid - 1` = 4.

- **Second iteration:**
  - `mid = 3`
  - `numBalls(position, 3)` returns 3 (we can place balls at positions 1, 4, and 8).
  - Since 3 >= 3, update `l` to `mid` = 3.

- **Third iteration:**
  - `mid = 4`
  - `numBalls(position, 4)` returns 3.
  - Since 3 >= 3, update `l` to `mid` = 4.

Now, `l` equals `r` and the loop terminates. The result is `l`, which is 4.

Thus, the largest minimum distance where we can place 3 balls in the given positions is 4.
