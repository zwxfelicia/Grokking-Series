# Introduction

**0/1 Knapsack** pattern is based on the famous problem with the same name which is efficiently solved using **Dynamic Programming (DP)**.

In this pattern, we will go through a set of problems to develop an understanding of DP. We will always start with a brute-force recursive solution to see the overlapping subproblems, i.e., realizing that we are solving the same problems repeatedly.

After the recursive solution, we will modify our algorithm to apply advanced techniques of **Memoization** and **Bottom-Up Dynamic Programming** to develop a complete understanding of this pattern.

Let’s jump onto our first problem.

# 0/1 Knapsack (medium)

## Introduction [#](https://www.educative.io/courses/grokking-the-coding-interview/gkZNLjV2kBk#introduction)

Given the weights and profits of ‘N’ items, we are asked to put these items in a knapsack with a capacity ‘C.’ The goal is to get the maximum profit out of the knapsack items. Each item can only be selected once, as we don’t have multiple quantities of any item.

Let’s take Merry’s example, who wants to carry some fruits in the knapsack to get maximum profit. Here are the weights and profits of the fruits:

**Items:** { Apple, Orange, Banana, Melon }
**Weights:** { 2, 3, 1, 4 }
**Profits:** { 4, 5, 3, 7 }
**Knapsack capacity:** 5

Let’s try to put various combinations of fruits in the knapsack, such that their total weight is not more than 5:

Apple + Orange (total weight 5) => 9 profit
Apple + Banana (total weight 3) => 7 profit
Orange + Banana (total weight 4) => 8 profit
Banana + Melon (total weight 5) => 10 profit

This shows that **Banana + Melon** is the best combination as it gives us the maximum profit, and the total weight does not exceed the capacity.

## Problem Statement [#](https://www.educative.io/courses/grokking-the-coding-interview/gkZNLjV2kBk#problem-statement)

Given two integer arrays to represent weights and profits of ‘N’ items, we need to find a subset of these items which will give us maximum profit such that their cumulative weight is not more than a given number ‘C.’ Each item can only be selected once, which means either we put an item in the knapsack or we skip it.

## Basic Solution [#](https://www.educative.io/courses/grokking-the-coding-interview/gkZNLjV2kBk#basic-solution)

A basic brute-force solution could be to try all combinations of the given items (as we did above), allowing us to choose the one with maximum profit and a weight that doesn’t exceed ‘C’. Take the example of four items (A, B, C, and D), as shown in the diagram below. To try all the combinations, our algorithm will look like:

```
for each item 'i' 
  create a new set which INCLUDES item 'i' if the total weight does not exceed the capacity, and 
     recursively process the remaining capacity and items
  create a new set WITHOUT item 'i', and recursively process the remaining items 
return the set from the above two sets with higher profit 
```

Here is a visual representation of our algorithm:

![image-20210621160148619](/Users/zhengwenxuan/Desktop/Grokking-Series/Grokking-the-Coding-Interview-Patterns-for-Coding-Questions/image-20210621160148619.png)

All green boxes have a total weight that is less than or equal to the capacity (7), and all the red ones have a weight that is more than 7. The best solution we have is with items [B, D] having a total profit of 22 and a total weight of 7.

### Code [#](https://www.educative.io/courses/grokking-the-coding-interview/gkZNLjV2kBk#code)

Here is the code for the brute-force solution:

```java
class Knapsack {

  public int solveKnapsack(int[] profits, int[] weights, int capacity) {
    return this.knapsackRecursive(profits, weights, capacity, 0);
  }

  private int knapsackRecursive(int[] profits, int[] weights, int capacity, int currentIndex) {
    // base checks
    if (capacity <= 0 || currentIndex >= profits.length)
      return 0;

    // recursive call after choosing the element at the currentIndex
    // if the weight of the element at currentIndex exceeds the capacity, we shouldn't process this
    int profit1 = 0;
    if( weights[currentIndex] <= capacity )
        profit1 = profits[currentIndex] + knapsackRecursive(profits, weights,
                capacity - weights[currentIndex], currentIndex + 1);

    // recursive call after excluding the element at the currentIndex
    int profit2 = knapsackRecursive(profits, weights, capacity, currentIndex + 1);

    return Math.max(profit1, profit2);
  }

  public static void main(String[] args) {
    Knapsack ks = new Knapsack();
    int[] profits = {1, 6, 10, 16};
    int[] weights = {1, 2, 3, 5};
    int maxProfit = ks.solveKnapsack(profits, weights, 7);
    System.out.println("Total knapsack profit ---> " + maxProfit);
    maxProfit = ks.solveKnapsack(profits, weights, 6);
    System.out.println("Total knapsack profit ---> " + maxProfit);
  }
}
```

### Time and Space complexity [#](https://www.educative.io/courses/grokking-the-coding-interview/gkZNLjV2kBk#time-and-space-complexity)

The above algorithm’s time complexity is exponential O(2^n), where ‘n’ represents the total number of items. This can also be confirmed from the above recursion tree. As we can see, we will have a total of ‘31’ recursive calls – calculated through (2^n) + (2^n) - 1, which is asymptotically equivalent to O(2^n)

The space complexity is O(n). This space will be used to store the recursion stack. Since the recursive algorithm works in a depth-first fashion, which means that we can’t have more than ‘n’ recursive calls on the call stack at any time.

**Overlapping Sub-problems:** Let’s visually draw the recursive calls to see if there are any overlapping sub-problems. As we can see, in each recursive call, profits and weights arrays remain constant, and only capacity and currentIndex change. For simplicity, let’s denote capacity with ‘c’ and currentIndex with ‘i’:

![image-20210621160236627](/Users/zhengwenxuan/Desktop/Grokking-Series/Grokking-the-Coding-Interview-Patterns-for-Coding-Questions/image-20210621160236627.png)

We can clearly see that **‘c:4, i=3’** has been called twice. Hence we have an overlapping sub-problems pattern. We can use [Memoization](https://en.wikipedia.org/wiki/Memoization) to solve overlapping sub-problems efficiently.

## Top-down Dynamic Programming with Memoization [#](https://www.educative.io/courses/grokking-the-coding-interview/gkZNLjV2kBk#top-down-dynamic-programming-with-memoization)

Memoization is when we store the results of all the previously solved sub-problems and return the results from memory if we encounter a problem that has already been solved.

Since we have two changing values (`capacity` and `currentIndex`) in our recursive function `knapsackRecursive()`, we can use a two-dimensional array to store the results of all the solved sub-problems. As mentioned above, we need to store results for every sub-array (i.e., for every possible index ‘i’) and every possible capacity ‘c.’

### Code [#](https://www.educative.io/courses/grokking-the-coding-interview/gkZNLjV2kBk#code-2)

Here is the code with memoization (see changes in the highlighted lines):

```java
class Knapsack {

  public int solveKnapsack(int[] profits, int[] weights, int capacity) {
    Integer[][] dp = new Integer[profits.length][capacity + 1];
    return this.knapsackRecursive(dp, profits, weights, capacity, 0);
  }

  private int knapsackRecursive(Integer[][] dp, int[] profits, int[] weights, int capacity,
      int currentIndex) {

    // base checks
    if (capacity <= 0 || currentIndex >= profits.length)
      return 0;

    // if we have already solved a similar problem, return the result from memory
    if(dp[currentIndex][capacity] != null)
      return dp[currentIndex][capacity];

    // recursive call after choosing the element at the currentIndex
    // if the weight of the element at currentIndex exceeds the capacity, we shouldn't process this
    int profit1 = 0;
    if( weights[currentIndex] <= capacity )
        profit1 = profits[currentIndex] + knapsackRecursive(dp, profits, weights,
                capacity - weights[currentIndex], currentIndex + 1);

    // recursive call after excluding the element at the currentIndex
    int profit2 = knapsackRecursive(dp, profits, weights, capacity, currentIndex + 1);

    dp[currentIndex][capacity] = Math.max(profit1, profit2);
    return dp[currentIndex][capacity];
  }

  public static void main(String[] args) {
    Knapsack ks = new Knapsack();
    int[] profits = {1, 6, 10, 16};
    int[] weights = {1, 2, 3, 5};
    int maxProfit = ks.solveKnapsack(profits, weights, 7);
    System.out.println("Total knapsack profit ---> " + maxProfit);
    maxProfit = ks.solveKnapsack(profits, weights, 6);
    System.out.println("Total knapsack profit ---> " + maxProfit);
  }
}
```

### Time and Space complexity [#](https://www.educative.io/courses/grokking-the-coding-interview/gkZNLjV2kBk#time-and-space-complexity-2)

Since our memoization array `dp[profits.length][capacity+1]` stores the results for all subproblems, we can conclude that we will not have more than N*Csubproblems (where ‘N’ is the number of items and ‘C’ is the knapsack capacity). This means that our time complexity will be O(N*C).

The above algorithm will use O(N*C) space for the memoization array. Other than that, we will use O(N)space for the recursion call-stack. So the total space complexity will be O(N*C + N), which is asymptotically equivalent to O(N*C).

## Bottom-up Dynamic Programming [#](https://www.educative.io/courses/grokking-the-coding-interview/gkZNLjV2kBk#bottom-up-dynamic-programming)

Let’s try to populate our `dp[][]` array from the above solution by working in a bottom-up fashion. Essentially, we want to find the maximum profit for every sub-array and every possible capacity. **This means that `dp[i][c]` will represent the maximum knapsack profit for capacity ‘c’ calculated from the first ‘i’ items.**

So, for each item at index ‘i’ (0 <= i < items.length) and capacity ‘c’ (0 <= c <= capacity), we have two options:

1. Exclude the item at index ‘i.’ In this case, we will take whatever profit we get from the sub-array excluding this item => `dp[i-1][c]`
2. Include the item at index ‘i’ if its weight is not more than the capacity. In this case, we include its profit plus whatever profit we get from the remaining capacity and from remaining items => `profit[i] + dp[i-1][c-weight[i]]`

Finally, our optimal solution will be maximum of the above two values:

```
    dp[i][c] = max (dp[i-1][c], profit[i] + dp[i-1][c-weight[i]]) 
```

Let’s draw this visually and start with our base case of zero capacity:

![image-20210621160416170](/Users/zhengwenxuan/Desktop/Grokking-Series/Grokking-the-Coding-Interview-Patterns-for-Coding-Questions/image-20210621160416170.png)

![image-20210621160423361](/Users/zhengwenxuan/Desktop/Grokking-Series/Grokking-the-Coding-Interview-Patterns-for-Coding-Questions/image-20210621160423361.png)

![image-20210621160429300](/Users/zhengwenxuan/Desktop/Grokking-Series/Grokking-the-Coding-Interview-Patterns-for-Coding-Questions/image-20210621160429300.png)

![image-20210621160436187](/Users/zhengwenxuan/Desktop/Grokking-Series/Grokking-the-Coding-Interview-Patterns-for-Coding-Questions/image-20210621160436187.png)

![image-20210621160443756](/Users/zhengwenxuan/Desktop/Grokking-Series/Grokking-the-Coding-Interview-Patterns-for-Coding-Questions/image-20210621160443756.png)

![image-20210621160449981](/Users/zhengwenxuan/Desktop/Grokking-Series/Grokking-the-Coding-Interview-Patterns-for-Coding-Questions/image-20210621160449981.png)

![image-20210621160456521](/Users/zhengwenxuan/Desktop/Grokking-Series/Grokking-the-Coding-Interview-Patterns-for-Coding-Questions/image-20210621160456521.png)

![image-20210621160502418](/Users/zhengwenxuan/Desktop/Grokking-Series/Grokking-the-Coding-Interview-Patterns-for-Coding-Questions/image-20210621160502418.png)

![image-20210621160508536](/Users/zhengwenxuan/Desktop/Grokking-Series/Grokking-the-Coding-Interview-Patterns-for-Coding-Questions/image-20210621160508536.png)

![image-20210621160514589](/Users/zhengwenxuan/Desktop/Grokking-Series/Grokking-the-Coding-Interview-Patterns-for-Coding-Questions/image-20210621160514589.png)

![image-20210621160642322](/Users/zhengwenxuan/Desktop/Grokking-Series/Grokking-the-Coding-Interview-Patterns-for-Coding-Questions/image-20210621160642322.png)

![image-20210621160648559](/Users/zhengwenxuan/Desktop/Grokking-Series/Grokking-the-Coding-Interview-Patterns-for-Coding-Questions/image-20210621160648559.png)

![image-20210621160655355](/Users/zhengwenxuan/Desktop/Grokking-Series/Grokking-the-Coding-Interview-Patterns-for-Coding-Questions/image-20210621160655355.png)

![image-20210621160701791](/Users/zhengwenxuan/Desktop/Grokking-Series/Grokking-the-Coding-Interview-Patterns-for-Coding-Questions/image-20210621160701791.png)

![image-20210621160707783](/Users/zhengwenxuan/Desktop/Grokking-Series/Grokking-the-Coding-Interview-Patterns-for-Coding-Questions/image-20210621160707783.png)

![image-20210621160714087](/Users/zhengwenxuan/Desktop/Grokking-Series/Grokking-the-Coding-Interview-Patterns-for-Coding-Questions/image-20210621160714087.png)

![image-20210621160726221](/Users/zhengwenxuan/Desktop/Grokking-Series/Grokking-the-Coding-Interview-Patterns-for-Coding-Questions/image-20210621160726221.png)

![image-20210621160732863](/Users/zhengwenxuan/Desktop/Grokking-Series/Grokking-the-Coding-Interview-Patterns-for-Coding-Questions/image-20210621160732863.png)

![image-20210621160738811](/Users/zhengwenxuan/Desktop/Grokking-Series/Grokking-the-Coding-Interview-Patterns-for-Coding-Questions/image-20210621160738811.png)

![image-20210621160745260](/Users/zhengwenxuan/Desktop/Grokking-Series/Grokking-the-Coding-Interview-Patterns-for-Coding-Questions/image-20210621160745260.png)

![image-20210621160752045](/Users/zhengwenxuan/Desktop/Grokking-Series/Grokking-the-Coding-Interview-Patterns-for-Coding-Questions/image-20210621160752045.png)

![image-20210621160757429](/Users/zhengwenxuan/Desktop/Grokking-Series/Grokking-the-Coding-Interview-Patterns-for-Coding-Questions/image-20210621160757429.png)

### Code [#](https://www.educative.io/courses/grokking-the-coding-interview/gkZNLjV2kBk#code-3)

Here is the code for our bottom-up dynamic programming approach:

```java
class Knapsack {

  public int solveKnapsack(int[] profits, int[] weights, int capacity) {
    // basic checks
    if (capacity <= 0 || profits.length == 0 || weights.length != profits.length)
      return 0;

    int n = profits.length;
    int[][] dp = new int[n][capacity + 1];

    // populate the capacity=0 columns, with '0' capacity we have '0' profit
    for(int i=0; i < n; i++)
      dp[i][0] = 0;

    // if we have only one weight, we will take it if it is not more than the capacity
    for(int c=0; c <= capacity; c++) {
      if(weights[0] <= c)
        dp[0][c] = profits[0];
    }

    // process all sub-arrays for all the capacities
    for(int i=1; i < n; i++) {
      for(int c=1; c <= capacity; c++) {
        int profit1= 0, profit2 = 0;
        // include the item, if it is not more than the capacity
        if(weights[i] <= c)
          profit1 = profits[i] + dp[i-1][c-weights[i]];
        // exclude the item
        profit2 = dp[i-1][c];
        // take maximum
        dp[i][c] = Math.max(profit1, profit2);
      }
    }

    // maximum profit will be at the bottom-right corner.
    return dp[n-1][capacity];
  }

  public static void main(String[] args) {
    Knapsack ks = new Knapsack();
    int[] profits = {1, 6, 10, 16};
    int[] weights = {1, 2, 3, 5};
    int maxProfit = ks.solveKnapsack(profits, weights, 7);
    System.out.println("Total knapsack profit ---> " + maxProfit);
    maxProfit = ks.solveKnapsack(profits, weights, 6);
    System.out.println("Total knapsack profit ---> " + maxProfit);
  }
}
```

### Time and Space complexity [#](https://www.educative.io/courses/grokking-the-coding-interview/gkZNLjV2kBk#time-and-space-complexity-3)

The above solution has the time and space complexity of O(N*C), where ‘N’ represents total items, and ‘C’ is the maximum capacity.

### How can we find the selected items? [#](https://www.educative.io/courses/grokking-the-coding-interview/gkZNLjV2kBk#how-can-we-find-the-selected-items)

As we know, the final profit is at the bottom-right corner. Therefore, we will start from there to find the items that will be going into the knapsack.

As you remember, at every step, we had two options: include an item or skip it. If we skip an item, we take the profit from the remaining items (i.e., from the cell right above it); if we include the item, then we jump to the remaining profit to find more items.

Let’s understand this from the above example:

![image-20210621160822706](/Users/zhengwenxuan/Desktop/Grokking-Series/Grokking-the-Coding-Interview-Patterns-for-Coding-Questions/image-20210621160822706.png)

1. ‘22’ did not come from the top cell (which is 17); hence we must include the item at index ‘3’ (which is item ‘D’).
2. Subtract the profit of item ‘D’ from ‘22’ to get the remaining profit ‘6’. We then jump to profit ‘6’ on the same row.
3. ‘6’ came from the top cell, so we jump to row ‘2’.
4. Again, ‘6’ came from the top cell, so we jump to row ‘1’.
5. ‘6’ is different from the top cell, so we must include this item (which is item ‘B’).
6. Subtract the profit of ‘B’ from ‘6’ to get profit ‘0’. We then jump to profit ‘0’ on the same row. As soon as we hit zero remaining profit, we can finish our item search.
7. Thus, the items going into the knapsack are {B, D}.

Let’s write a function to print the set of items included in the knapsack.

```java
import java.util.*;

class Knapsack {

  public int solveKnapsack(int[] profits, int[] weights, int capacity) {
    // base checks
    if (capacity <= 0 || profits.length == 0 || weights.length != profits.length)
      return 0;

    int n = profits.length;
    int[][] dp = new int[n][capacity + 1];

    // populate the capacity=0 columns, with '0' capacity we have '0' profit
    for(int i=0; i < n; i++)
      dp[i][0] = 0;

    // if we have only one weight, we will take it if it is not more than the capacity
    for(int c=0; c <= capacity; c++) {
      if(weights[0] <= c)
        dp[0][c] = profits[0];
    }

    // process all sub-arrays for all the capacities
    for(int i=1; i < n; i++) {
      for(int c=1; c <= capacity; c++) {
        int profit1= 0, profit2 = 0;
        // include the item, if it is not more than the capacity
        if(weights[i] <= c)
          profit1 = profits[i] + dp[i-1][c-weights[i]];
        // exclude the item
        profit2 = dp[i-1][c];
        // take maximum
        dp[i][c] = Math.max(profit1, profit2);
      }
    }

    printSelectedElements(dp, weights, profits, capacity);
    // maximum profit will be at the bottom-right corner.
    return dp[n-1][capacity];
  }

 private void printSelectedElements(int dp[][], int[] weights, int[] profits, int capacity){
   System.out.print("Selected weights:");
   int totalProfit = dp[weights.length-1][capacity];
   for(int i=weights.length-1; i > 0; i--) {
     if(totalProfit != dp[i-1][capacity]) {
       System.out.print(" " + weights[i]);
       capacity -= weights[i];
       totalProfit -= profits[i];
     }
   }

   if(totalProfit != 0)
     System.out.print(" " + weights[0]);
   System.out.println("");
 }

  public static void main(String[] args) {
    Knapsack ks = new Knapsack();
    int[] profits = {1, 6, 10, 16};
    int[] weights = {1, 2, 3, 5};
    int maxProfit = ks.solveKnapsack(profits, weights, 7);
    System.out.println("Total knapsack profit ---> " + maxProfit);
    maxProfit = ks.solveKnapsack(profits, weights, 6);
    System.out.println("Total knapsack profit ---> " + maxProfit);
  }
}
```

## Challenge [#](https://www.educative.io/courses/grokking-the-coding-interview/gkZNLjV2kBk#challenge)

Can we improve our bottom-up DP solution even further? Can you find an algorithm that has O(C)space complexity?

```java
class Knapsack {

  static int solveKnapsack(int[] profits, int[] weights, int capacity) {
    // basic checks
    if (capacity <= 0 || profits.length == 0 || weights.length != profits.length)
      return 0;

    int n = profits.length;
    // we only need one previous row to find the optimal solution, overall we need '2' rows
    // the above solution is similar to the previous solution, the only difference is that 
    // we use `i%2` instead if `i` and `(i-1)%2` instead if `i-1`
    int[][] dp = new int[2][capacity+1];

    // if we have only one weight, we will take it if it is not more than the capacity
    for(int c=0; c <= capacity; c++) {
      if(weights[0] <= c)
        dp[0][c] = dp[1][c] = profits[0];
    }

    // process all sub-arrays for all the capacities
    for(int i=1; i < n; i++) {
      for(int c=0; c <= capacity; c++) {
        int profit1= 0, profit2 = 0;
        // include the item, if it is not more than the capacity
        if(weights[i] <= c)
          profit1 = profits[i] + dp[(i-1)%2][c-weights[i]];
        // exclude the item
        profit2 = dp[(i-1)%2][c];
        // take maximum
        dp[i%2][c] = Math.max(profit1, profit2);
      }
    }

    return dp[(n-1)%2][capacity];
  }
}
```

The solution above is similar to the previous solution; the only difference is that we use `i%2` instead of `i` and `(i-1)%2` instead of `i-1`. This solution has a space complexity of O(2*C) = O(C), where ‘C’ is the knapsack’s maximum capacity.

This space optimization solution can also be implemented using a single array. It is a bit tricky, but the intuition is to use the same array for the previous and the next iteration!

If you see closely, we need two values from the previous iteration: `dp[c]` and `dp[c-weight[i]]`

Since our inner loop is iterating over `c:0-->capacity`, let’s see how this might affect our two required values:

1. When we access `dp[c],` it has not been overridden yet for the current iteration, so it should be fine.
2. `dp[c-weight[i]]` might be overridden if “weight[i] > 0”. Therefore we can’t use this value for the current iteration.

To solve the second case, we can change our inner loop to process in the reverse direction: `c:capacity-->0`. This will ensure that whenever we change a value in `dp[],` we will not need it again in the current iteration.

Can you try writing this algorithm?

```java
class Knapsack {

  static int solveKnapsack(int[] profits, int[] weights, int capacity) {
    // basic checks
    if (capacity <= 0 || profits.length == 0 || weights.length != profits.length)
      return 0;

    int n = profits.length;
    int[] dp = new int[capacity + 1];

    // if we have only one weight, we will take it if it is not more than the
    // capacity
    for (int c = 0; c <= capacity; c++) {
      if (weights[0] <= c)
        dp[c] = profits[0];
    }

    // process all sub-arrays for all the capacities
    for (int i = 1; i < n; i++) {
      for (int c = capacity; c >= 0; c--) {
        int profit1 = 0, profit2 = 0;
        // include the item, if it is not more than the capacity
        if (weights[i] <= c)
          profit1 = profits[i] + dp[c - weights[i]];
        // exclude the item
        profit2 = dp[c];
        // take maximum
        dp[c] = Math.max(profit1, profit2);
      }
    }

    return dp[capacity];
  }
}
```

# Equal Subset Sum Partition (medium)

## Problem Statement [#](https://www.educative.io/courses/grokking-the-coding-interview/g7QYlD8RwRr#problem-statement)

Given a set of positive numbers, find if we can partition it into two subsets such that the sum of elements in both subsets is equal.

**Example 1:**

```
Input: {1, 2, 3, 4}
Output: True
Explanation: The given set can be partitioned into two subsets with equal sum: {1, 4} & {2, 3}
```

**Example 2:**

```
Input: {1, 1, 3, 4, 7}
Output: True
Explanation: The given set can be partitioned into two subsets with equal sum: {1, 3, 4} & {1, 7}
```

**Example 3:**

```
Input: {2, 3, 4, 6}
Output: False
Explanation: The given set cannot be partitioned into two subsets with equal sum.
```

## Basic Solution [#](https://www.educative.io/courses/grokking-the-coding-interview/g7QYlD8RwRr#basic-solution)

This problem follows the **0/1 Knapsack pattern**. A basic brute-force solution could be to try all combinations of partitioning the given numbers into two sets to see if any pair of sets has an equal sum.

Assume that `S` represents the total sum of all the given numbers. Then the two equal subsets must have a sum equal to `S/2`. This essentially transforms our problem to: "Find a subset of the given numbers that has a total sum of `S/2`".

So our brute-force algorithm will look like:

```
for each number 'i' 
  create a new set which INCLUDES number 'i' if it does not exceed 'S/2', and recursively 
      process the remaining numbers
  create a new set WITHOUT number 'i', and recursively process the remaining items 
return true if any of the above sets has a sum equal to 'S/2', otherwise return false
```

### Code [#](https://www.educative.io/courses/grokking-the-coding-interview/g7QYlD8RwRr#code)

Here is the code for the brute-force solution:

```java
class PartitionSet {

  public boolean canPartition(int[] num) {
    int sum = 0;
    for (int i = 0; i < num.length; i++)
      sum += num[i];

    // if 'sum' is a an odd number, we can't have two subsets with equal sum
    if(sum % 2 != 0)
      return false;

    return this.canPartitionRecursive(num, sum/2, 0);
  }

  private boolean canPartitionRecursive(int[] num, int sum, int currentIndex) {
    // base check
    if (sum == 0)
      return true;

    if(num.length == 0 || currentIndex >= num.length)
      return false;

    // recursive call after choosing the number at the currentIndex
    // if the number at currentIndex exceeds the sum, we shouldn't process this
    if( num[currentIndex] <= sum ) {
      if(canPartitionRecursive(num, sum - num[currentIndex], currentIndex + 1))
        return true;
    }

    // recursive call after excluding the number at the currentIndex
    return canPartitionRecursive(num, sum, currentIndex + 1);
  }

  public static void main(String[] args) {
    PartitionSet ps = new PartitionSet();
    int[] num = {1, 2, 3, 4};
    System.out.println(ps.canPartition(num));
    num = new int[]{1, 1, 3, 4, 7};
    System.out.println(ps.canPartition(num));
    num = new int[]{2, 3, 4, 6};
    System.out.println(ps.canPartition(num));
  }
}
```



### Time and Space complexity [#](https://www.educative.io/courses/grokking-the-coding-interview/g7QYlD8RwRr#time-and-space-complexity)

The time complexity of the above algorithm is exponential O(2^n), where ‘n’ represents the total number. The space complexity is O(n) which will be used to store the recursion stack.

## Top-down Dynamic Programming with Memoization [#](https://www.educative.io/courses/grokking-the-coding-interview/g7QYlD8RwRr#top-down-dynamic-programming-with-memoization)

We can use memoization to overcome the overlapping sub-problems. As stated in previous lessons, memoization is when we store the results of all the previously solved sub-problems so we can return the results from memory if we encounter a problem that has already been solved.

Since we need to store the results for every subset and for every possible sum, therefore we will be using a two-dimensional array to store the results of the solved sub-problems. The first dimension of the array will represent different subsets and the second dimension will represent different ‘sums’ that we can calculate from each subset. These two dimensions of the array can also be inferred from the two changing values (sum and currentIndex) in our recursive function `canPartitionRecursive()`.

### Code [#](https://www.educative.io/courses/grokking-the-coding-interview/g7QYlD8RwRr#code-2)

Here is the code for Top-down DP with memoization:

```java
class PartitionSet {

  public boolean canPartition(int[] num) {
    int sum = 0;
    for (int i = 0; i < num.length; i++)
      sum += num[i];

    // if 'sum' is a an odd number, we can't have two subsets with equal sum
    if (sum % 2 != 0)
      return false;

    Boolean[][] dp = new Boolean[num.length][sum / 2 + 1];
    return this.canPartitionRecursive(dp, num, sum / 2, 0);
  }

  private boolean canPartitionRecursive(Boolean[][] dp, int[] num, int sum, int currentIndex) {
    // base check
    if (sum == 0)
      return true;

    if (num.length == 0 || currentIndex >= num.length)
      return false;

    // if we have not already processed a similar problem
    if (dp[currentIndex][sum] == null) {
      // recursive call after choosing the number at the currentIndex
      // if the number at currentIndex exceeds the sum, we shouldn't process this
      if (num[currentIndex] <= sum) {
        if (canPartitionRecursive(dp, num, sum - num[currentIndex], currentIndex + 1)) {
          dp[currentIndex][sum] = true;
          return true;
        }
      }

      // recursive call after excluding the number at the currentIndex
      dp[currentIndex][sum] = canPartitionRecursive(dp, num, sum, currentIndex + 1);
    }

    return dp[currentIndex][sum];
  }

  public static void main(String[] args) {
    PartitionSet ps = new PartitionSet();
    int[] num = { 1, 2, 3, 4 };
    System.out.println(ps.canPartition(num));
    num = new int[] { 1, 1, 3, 4, 7 };
    System.out.println(ps.canPartition(num));
    num = new int[] { 2, 3, 4, 6 };
    System.out.println(ps.canPartition(num));
  }
}
```

### Time and Space complexity [#](https://www.educative.io/courses/grokking-the-coding-interview/g7QYlD8RwRr#time-and-space-complexity-2)

The above algorithm has the time and space complexity of O(N*S), where ‘N’ represents total numbers and ‘S’ is the total sum of all the numbers.

## Bottom-up Dynamic Programming [#](https://www.educative.io/courses/grokking-the-coding-interview/g7QYlD8RwRr#bottom-up-dynamic-programming)

Let’s try to populate our `dp[][]` array from the above solution by working in a bottom-up fashion. Essentially, we want to find if we can make all possible sums with every subset. **This means, `dp[i][s]` will be ‘true’ if we can make the sum ‘s’ from the first ‘i’ numbers.**

So, for each number at index ‘i’ (0 <= i < num.length) and sum ‘s’ (0 <= s <= S/2), we have two options:

1. Exclude the number. In this case, we will see if we can get ‘s’ from the subset excluding this number: `dp[i-1][s]`
2. Include the number if its value is not more than ‘s’. In this case, we will see if we can find a subset to get the remaining sum: `dp[i-1][s-num[i]]`

If either of the two above scenarios is true, we can find a subset of numbers with a sum equal to ‘s’.

Let’s start with our base case of zero capacity:

![image-20210621161334680](/Users/zhengwenxuan/Desktop/Grokking-Series/Grokking-the-Coding-Interview-Patterns-for-Coding-Questions/image-20210621161334680.png)

![image-20210621161341290](/Users/zhengwenxuan/Desktop/Grokking-Series/Grokking-the-Coding-Interview-Patterns-for-Coding-Questions/image-20210621161341290.png)

![image-20210621161347635](/Users/zhengwenxuan/Desktop/Grokking-Series/Grokking-the-Coding-Interview-Patterns-for-Coding-Questions/image-20210621161347635.png)

![image-20210621161354913](/Users/zhengwenxuan/Desktop/Grokking-Series/Grokking-the-Coding-Interview-Patterns-for-Coding-Questions/image-20210621161354913.png)

![image-20210621161400758](/Users/zhengwenxuan/Desktop/Grokking-Series/Grokking-the-Coding-Interview-Patterns-for-Coding-Questions/image-20210621161400758.png)

![image-20210621161409246](/Users/zhengwenxuan/Desktop/Grokking-Series/Grokking-the-Coding-Interview-Patterns-for-Coding-Questions/image-20210621161409246.png)

![image-20210621161415807](/Users/zhengwenxuan/Desktop/Grokking-Series/Grokking-the-Coding-Interview-Patterns-for-Coding-Questions/image-20210621161415807.png)

![image-20210621161421910](/Users/zhengwenxuan/Desktop/Grokking-Series/Grokking-the-Coding-Interview-Patterns-for-Coding-Questions/image-20210621161421910.png)

![image-20210621161428576](/Users/zhengwenxuan/Desktop/Grokking-Series/Grokking-the-Coding-Interview-Patterns-for-Coding-Questions/image-20210621161428576.png)

From the above visualization, we can clearly see that it is possible to partition the given set into two subsets with equal sums, as shown by bottom-right cell: `dp[3][5] => T`

### Code [#](https://www.educative.io/courses/grokking-the-coding-interview/g7QYlD8RwRr#code-3)

Here is the code for our bottom-up dynamic programming approach:

```java
class PartitionSet {

  public boolean canPartition(int[] num) {
    int n = num.length;
    // find the total sum
    int sum = 0;
    for (int i = 0; i < n; i++)
      sum += num[i];

    // if 'sum' is a an odd number, we can't have two subsets with same total
    if(sum % 2 != 0)
      return false;

    // we are trying to find a subset of given numbers that has a total sum of ‘sum/2’.
    sum /= 2;

    boolean[][] dp = new boolean[n][sum + 1];

    // populate the sum=0 columns, as we can always for '0' sum with an empty set
    for(int i=0; i < n; i++)
      dp[i][0] = true;

    // with only one number, we can form a subset only when the required sum is equal to its value
    for(int s=1; s <= sum ; s++) {
      dp[0][s] = (num[0] == s ? true : false);
    }

    // process all subsets for all sums
    for(int i=1; i < n; i++) {
      for(int s=1; s <= sum; s++) {
        // if we can get the sum 's' without the number at index 'i'
        if(dp[i-1][s]) {
          dp[i][s] = dp[i-1][s];
        } else if (s >= num[i]) { // else if we can find a subset to get the remaining sum
          dp[i][s] = dp[i-1][s-num[i]];
        }
      }
    }

    // the bottom-right corner will have our answer.
    return dp[n-1][sum];
  }

  public static void main(String[] args) {
    PartitionSet ps = new PartitionSet();
    int[] num = {1, 2, 3, 4};
    System.out.println(ps.canPartition(num));
    num = new int[]{1, 1, 3, 4, 7};
    System.out.println(ps.canPartition(num));
    num = new int[]{2, 3, 4, 6};
    System.out.println(ps.canPartition(num));
  }
}
```

### Time and Space complexity [#](https://www.educative.io/courses/grokking-the-coding-interview/g7QYlD8RwRr#time-and-space-complexity-3)

The above solution the has time and space complexity of O(N*S), where ‘N’ represents total numbers and ‘S’ is the total sum of all the numbers.

# Subset Sum (medium)

## Problem Statement [#](https://www.educative.io/courses/grokking-the-coding-interview/gxrnL0GQGqk#problem-statement)

Given a set of positive numbers, determine if a subset exists whose sum is equal to a given number ‘S’.

### Example 1: [#](https://www.educative.io/courses/grokking-the-coding-interview/gxrnL0GQGqk#example-1)

```
Input: {1, 2, 3, 7}, S=6
Output: True
The given set has a subset whose sum is '6': {1, 2, 3}
```

### Example 2: [#](https://www.educative.io/courses/grokking-the-coding-interview/gxrnL0GQGqk#example-2)

```
Input: {1, 2, 7, 1, 5}, S=10
Output: True
The given set has a subset whose sum is '10': {1, 2, 7}
```

### Example 3: [#](https://www.educative.io/courses/grokking-the-coding-interview/gxrnL0GQGqk#example-3)

```
Input: {1, 3, 4, 8}, S=6
Output: False
The given set does not have any subset whose sum is equal to '6'.
```

## Basic Solution [#](https://www.educative.io/courses/grokking-the-coding-interview/gxrnL0GQGqk#basic-solution)

This problem follows the **0/1 Knapsack pattern** and is quite similar to [Equal Subset Sum Partition](https://www.educative.io/collection/page/5668639101419520/5671464854355968/6336012772966400/). A basic brute-force solution could be to try all subsets of the given numbers to see if any set has a sum equal to ‘S’.

So our brute-force algorithm will look like:

```
for each number 'i' 
  create a new set which INCLUDES number 'i' if it does not exceed 'S', and recursively 
     process the remaining numbers
  create a new set WITHOUT number 'i', and recursively process the remaining numbers 
return true if any of the above two sets has a sum equal to 'S', otherwise return false
```

Since this problem is quite similar to [Equal Subset Sum Partition](https://www.educative.io/collection/page/5668639101419520/5671464854355968/6336012772966400/), let’s jump directly to the bottom-up dynamic programming solution.

## Bottom-up Dynamic Programming [#](https://www.educative.io/courses/grokking-the-coding-interview/gxrnL0GQGqk#bottom-up-dynamic-programming)

We’ll try to find if we can make all possible sums with every subset to populate the array `dp[TotalNumbers][S+1]`.

For every possible sum ‘s’ (where 0 <= s <= S), we have two options:

1. Exclude the number. In this case, we will see if we can get the sum ‘s’ from the subset excluding this number => `dp[index-1][s]`
2. Include the number if its value is not more than ‘s’. In this case, we will see if we can find a subset to get the remaining sum => `dp[index-1][s-num[index]]`

If either of the above two scenarios returns true, we can find a subset with a sum equal to ‘s’.

Let’s draw this visually, with the example input {1, 2, 3, 7}, and start with our base case of size zero:

![image-20210621161554399](/Users/zhengwenxuan/Desktop/Grokking-Series/Grokking-the-Coding-Interview-Patterns-for-Coding-Questions/image-20210621161554399.png)

![image-20210621161600310](/Users/zhengwenxuan/Desktop/Grokking-Series/Grokking-the-Coding-Interview-Patterns-for-Coding-Questions/image-20210621161600310.png)

![image-20210621161608996](/Users/zhengwenxuan/Desktop/Grokking-Series/Grokking-the-Coding-Interview-Patterns-for-Coding-Questions/image-20210621161608996.png)

![image-20210621161615505](/Users/zhengwenxuan/Desktop/Grokking-Series/Grokking-the-Coding-Interview-Patterns-for-Coding-Questions/image-20210621161615505.png)

![image-20210621161625009](/Users/zhengwenxuan/Desktop/Grokking-Series/Grokking-the-Coding-Interview-Patterns-for-Coding-Questions/image-20210621161625009.png)

![image-20210621161630671](/Users/zhengwenxuan/Desktop/Grokking-Series/Grokking-the-Coding-Interview-Patterns-for-Coding-Questions/image-20210621161630671.png)

![image-20210621161636712](/Users/zhengwenxuan/Desktop/Grokking-Series/Grokking-the-Coding-Interview-Patterns-for-Coding-Questions/image-20210621161636712.png)

![image-20210621161642369](/Users/zhengwenxuan/Desktop/Grokking-Series/Grokking-the-Coding-Interview-Patterns-for-Coding-Questions/image-20210621161642369.png)

![image-20210621161648552](/Users/zhengwenxuan/Desktop/Grokking-Series/Grokking-the-Coding-Interview-Patterns-for-Coding-Questions/image-20210621161648552.png)

![image-20210621161654846](/Users/zhengwenxuan/Desktop/Grokking-Series/Grokking-the-Coding-Interview-Patterns-for-Coding-Questions/image-20210621161654846.png)

### Code [#](https://www.educative.io/courses/grokking-the-coding-interview/gxrnL0GQGqk#code)

Here is the code for our bottom-up dynamic programming approach:

```java
class SubsetSum {

  public boolean canPartition(int[] num, int sum) {
    int n = num.length;
    boolean[][] dp = new boolean[n][sum + 1];

    // populate the sum=0 columns, as we can always form '0' sum with an empty set
    for (int i = 0; i < n; i++)
      dp[i][0] = true;

    // with only one number, we can form a subset only when the required sum is
    // equal to its value
    for (int s = 1; s <= sum; s++) {
      dp[0][s] = (num[0] == s ? true : false);
    }

    // process all subsets for all sums
    for (int i = 1; i < num.length; i++) {
      for (int s = 1; s <= sum; s++) {
        // if we can get the sum 's' without the number at index 'i'
        if (dp[i - 1][s]) {
          dp[i][s] = dp[i - 1][s];
        } else if (s >= num[i]) {
          // else include the number and see if we can find a subset to get the remaining
          // sum
          dp[i][s] = dp[i - 1][s - num[i]];
        }
      }
    }

    // the bottom-right corner will have our answer.
    return dp[num.length - 1][sum];
  }

  public static void main(String[] args) {
    SubsetSum ss = new SubsetSum();
    int[] num = { 1, 2, 3, 7 };
    System.out.println(ss.canPartition(num, 6));
    num = new int[] { 1, 2, 7, 1, 5 };
    System.out.println(ss.canPartition(num, 10));
    num = new int[] { 1, 3, 4, 8 };
    System.out.println(ss.canPartition(num, 6));
  }
}
```

### Time and Space complexity [#](https://www.educative.io/courses/grokking-the-coding-interview/gxrnL0GQGqk#time-and-space-complexity)

The above solution has the time and space complexity of O(N*S), where ‘N’ represents total numbers and ‘S’ is the required sum.

## Challenge [#](https://www.educative.io/courses/grokking-the-coding-interview/gxrnL0GQGqk#challenge)

Can we improve our bottom-up DP solution even further? Can you find an algorithm that has O(S) space complexity?

```java
class SubsetSum {

  static boolean canPartition(int[] num, int sum) {
    int n = num.length;
    boolean[] dp = new boolean[sum + 1];

    // handle sum=0, as we can always have '0' sum with an empty set
    dp[0] = true;

    // with only one number, we can have a subset only when the required sum is equal to its value
    for (int s = 1; s <= sum; s++) {
      dp[s] = (num[0] == s ? true : false);
    }

    // process all subsets for all sums
    for (int i = 1; i < n; i++) {
      for (int s = sum; s >= 0; s--) {
        // if dp[s]==true, this means we can get the sum 's' without num[i], hence we can move on to
        // the next number else we can include num[i] and see if we can find a subset to get the
        // remaining sum
        if (!dp[s] && s >= num[i]) {
          dp[s] = dp[s - num[i]];
        }
      }
    }

    return dp[sum];
  }
}
```

# Minimum Subset Sum Difference (hard)

## Problem Statement [#](https://www.educative.io/courses/grokking-the-coding-interview/mE53y85Wqw9#problem-statement)

Given a set of positive numbers, partition the set into two subsets with minimum difference between their subset sums.

### Example 1: [#](https://www.educative.io/courses/grokking-the-coding-interview/mE53y85Wqw9#example-1)

```
Input: {1, 2, 3, 9}
Output: 3
Explanation: We can partition the given set into two subsets where minimum absolute difference 
between the sum of numbers is '3'. Following are the two subsets: {1, 2, 3} & {9}.
```

### Example 2: [#](https://www.educative.io/courses/grokking-the-coding-interview/mE53y85Wqw9#example-2)

```
Input: {1, 2, 7, 1, 5}
Output: 0
Explanation: We can partition the given set into two subsets where minimum absolute difference 
between the sum of number is '0'. Following are the two subsets: {1, 2, 5} & {7, 1}.
```

### Example 3: [#](https://www.educative.io/courses/grokking-the-coding-interview/mE53y85Wqw9#example-3)

```
Input: {1, 3, 100, 4}
Output: 92
Explanation: We can partition the given set into two subsets where minimum absolute difference 
between the sum of numbers is '92'. Here are the two subsets: {1, 3, 4} & {100}.
```

## Basic Solution [#](https://www.educative.io/courses/grokking-the-coding-interview/mE53y85Wqw9#basic-solution)

This problem follows the **0/1 Knapsack pattern** and can be converted into a [Subset Sum](https://www.educative.io/collection/page/5668639101419520/5671464854355968/6126968124735488/) problem.

Let’s assume S1 and S2 are the two desired subsets. A basic brute-force solution could be to try adding each element either in S1 or S2 in order to find the combination that gives the minimum sum difference between the two sets.

So our brute-force algorithm will look like:

```
for each number 'i' 
  add number 'i' to S1 and recursively process the remaining numbers
  add number 'i' to S2 and recursively process the remaining numbers
return the minimum absolute difference of the above two sets 
```

### Code [#](https://www.educative.io/courses/grokking-the-coding-interview/mE53y85Wqw9#code)

Here is the code for the brute-force solution:

```java
class PartitionSet {

  public int canPartition(int[] num) {
    return this.canPartitionRecursive(num, 0, 0, 0);
  }

  private int canPartitionRecursive(int[] num, int currentIndex, int sum1, int sum2) {
    // base check
    if (currentIndex == num.length)
      return Math.abs(sum1 - sum2);

    // recursive call after including the number at the currentIndex in the first set
    int diff1 = canPartitionRecursive(num, currentIndex+1, sum1+num[currentIndex], sum2);

    // recursive call after including the number at the currentIndex in the second set
    int diff2 = canPartitionRecursive(num, currentIndex+1, sum1, sum2+num[currentIndex]);

    return Math.min(diff1, diff2);
  }

  public static void main(String[] args) {
    PartitionSet ps = new PartitionSet();
    int[] num = {1, 2, 3, 9};
    System.out.println(ps.canPartition(num));
    num = new int[]{1, 2, 7, 1, 5};
    System.out.println(ps.canPartition(num));
    num = new int[]{1, 3, 100, 4};
    System.out.println(ps.canPartition(num));
  }
}
```

### Time and Space complexity [#](https://www.educative.io/courses/grokking-the-coding-interview/mE53y85Wqw9#time-and-space-complexity)

Because of the two recursive calls, the time complexity of the above algorithm is exponential O(2^n), where ‘n’ represents the total number. The space complexity is O(n) which is used to store the recursion stack.

## Top-down Dynamic Programming with Memoization [#](https://www.educative.io/courses/grokking-the-coding-interview/mE53y85Wqw9#top-down-dynamic-programming-with-memoization)

We can use memoization to overcome the overlapping sub-problems.

We will be using a two-dimensional array to store the results of the solved sub-problems. We can uniquely identify a sub-problem from ‘currentIndex’ and ‘Sum1’ as ‘Sum2’ will always be the sum of the remaining numbers.

### Code [#](https://www.educative.io/courses/grokking-the-coding-interview/mE53y85Wqw9#code-2)

Here is the code:

```java
class PartitionSet {

  public int canPartition(int[] num) {
    int sum = 0;
    for (int i = 0; i < num.length; i++)
      sum += num[i];

    Integer[][] dp = new Integer[num.length][sum + 1];
    return this.canPartitionRecursive(dp, num, 0, 0, 0);
  }

  private int canPartitionRecursive(Integer[][] dp, int[] num, int currentIndex, int sum1, int sum2) {
    // base check
    if(currentIndex == num.length)
      return Math.abs(sum1 - sum2);

    // check if we have not already processed similar problem
    if(dp[currentIndex][sum1] == null) {
      // recursive call after including the number at the currentIndex in the first set
      int diff1 = canPartitionRecursive(dp, num, currentIndex + 1, sum1 + num[currentIndex], sum2);

      // recursive call after including the number at the currentIndex in the second set
      int diff2 = canPartitionRecursive(dp, num, currentIndex + 1, sum1, sum2 + num[currentIndex]);

      dp[currentIndex][sum1] = Math.min(diff1, diff2);
    }

    return dp[currentIndex][sum1];
  }

  public static void main(String[] args) {
    PartitionSet ps = new PartitionSet();
    int[] num = {1, 2, 3, 9};
    System.out.println(ps.canPartition(num));
    num = new int[]{1, 2, 7, 1, 5};
    System.out.println(ps.canPartition(num));
    num = new int[]{1, 3, 100, 4};
    System.out.println(ps.canPartition(num));
  }
}
```

## Bottom-up Dynamic Programming [#](https://www.educative.io/courses/grokking-the-coding-interview/mE53y85Wqw9#bottom-up-dynamic-programming)

Let’s assume ‘S’ represents the total sum of all the numbers. So, in this problem, we are trying to find a subset whose sum is as close to ‘S/2’ as possible, because if we can partition the given set into two subsets of an equal sum, we get the minimum difference, i.e. zero. This transforms our problem to [Subset Sum](https://www.educative.io/collection/page/5668639101419520/5671464854355968/6126968124735488/), where we try to find a subset whose sum is equal to a given number-- ‘S/2’ in our case. If we can’t find such a subset, then we will take the subset which has the sum closest to ‘S/2’. This is easily possible, as we will be calculating all possible sums with every subset.

Essentially, we need to calculate all the possible sums up to ‘S/2’ for all numbers. So how can we populate the array `db[TotalNumbers][S/2+1]` in the bottom-up fashion?

For every possible sum ‘s’ (where 0 <= s <= S/2), we have two options:

1. Exclude the number. In this case, we will see if we can get the sum ‘s’ from the subset excluding this number => `dp[index-1][s]`
2. Include the number if its value is not more than ‘s’. In this case, we will see if we can find a subset to get the remaining sum => `dp[index-1][s-num[index]]`

If either of the two above scenarios is true, we can find a subset with a sum equal to ‘s’. We should dig into this before we can learn how to find the closest subset.

Let’s draw this visually, with the example input {1, 2, 3, 9}. Since the total sum is ‘15’, we will try to find a subset whose sum is equal to the half of it, i.e. ‘7’.

![image-20210621161957490](/Users/zhengwenxuan/Desktop/Grokking-Series/Grokking-the-Coding-Interview-Patterns-for-Coding-Questions/image-20210621161957490.png)

![image-20210621162004295](/Users/zhengwenxuan/Desktop/Grokking-Series/Grokking-the-Coding-Interview-Patterns-for-Coding-Questions/image-20210621162004295.png)

![image-20210621162010413](/Users/zhengwenxuan/Desktop/Grokking-Series/Grokking-the-Coding-Interview-Patterns-for-Coding-Questions/image-20210621162010413.png)

![image-20210621162016572](/Users/zhengwenxuan/Desktop/Grokking-Series/Grokking-the-Coding-Interview-Patterns-for-Coding-Questions/image-20210621162016572.png)

![image-20210621162022925](/Users/zhengwenxuan/Desktop/Grokking-Series/Grokking-the-Coding-Interview-Patterns-for-Coding-Questions/image-20210621162022925.png)

![image-20210621162029026](/Users/zhengwenxuan/Desktop/Grokking-Series/Grokking-the-Coding-Interview-Patterns-for-Coding-Questions/image-20210621162029026.png)

![image-20210621162035744](/Users/zhengwenxuan/Desktop/Grokking-Series/Grokking-the-Coding-Interview-Patterns-for-Coding-Questions/image-20210621162035744.png)

![image-20210621162041200](/Users/zhengwenxuan/Desktop/Grokking-Series/Grokking-the-Coding-Interview-Patterns-for-Coding-Questions/image-20210621162041200.png)

![image-20210621162047386](/Users/zhengwenxuan/Desktop/Grokking-Series/Grokking-the-Coding-Interview-Patterns-for-Coding-Questions/image-20210621162047386.png)

![image-20210621162052787](/Users/zhengwenxuan/Desktop/Grokking-Series/Grokking-the-Coding-Interview-Patterns-for-Coding-Questions/image-20210621162052787.png)

![image-20210621162058406](/Users/zhengwenxuan/Desktop/Grokking-Series/Grokking-the-Coding-Interview-Patterns-for-Coding-Questions/image-20210621162058406.png)

The above visualization tells us that it is not possible to find a subset whose sum is equal to ‘7’. So what is the closest subset we can find? We can find the subset if we start moving backwards in the last row from the bottom right corner to find the first ‘T’. The first “T” in the diagram above is the sum ‘6’, which means that we can find a subset whose sum is equal to ‘6’. This means the other set will have a sum of ‘9’ and the minimum difference will be ‘3’.

### Code [#](https://www.educative.io/courses/grokking-the-coding-interview/mE53y85Wqw9#code-3)

Here is the code for our bottom-up dynamic programming approach:

```java
class PartitionSet {

  public int canPartition(int[] num) {
    int sum = 0;
    for (int i = 0; i < num.length; i++)
      sum += num[i];

    int n = num.length;
    boolean[][] dp = new boolean[n][sum/2 + 1];

    // populate the sum=0 columns, as we can always form '0' sum with an empty set
    for(int i=0; i < n; i++)
      dp[i][0] = true;

    // with only one number, we can form a subset only when the required sum is equal to that number
    for(int s=1; s <= sum/2 ; s++) {
      dp[0][s] = (num[0] == s ? true : false);
    }

    // process all subsets for all sums
    for(int i=1; i < num.length; i++) {
      for(int s=1; s <= sum/2; s++) {
        // if we can get the sum 's' without the number at index 'i'
        if(dp[i-1][s]) {
          dp[i][s] = dp[i-1][s];
        } else if (s >= num[i]) {
          // else include the number and see if we can find a subset to get the remaining sum
          dp[i][s] = dp[i-1][s-num[i]];
        }
      }
    }

    int sum1 = 0;
    // Find the largest index in the last row which is true
    for (int i = sum / 2; i >= 0; i--) {
      if (dp[n-1][i] == true) {
          sum1 = i;
          break;
      }
    }

    int sum2 = sum - sum1;
    return Math.abs(sum2-sum1);
  }

  public static void main(String[] args) {
    PartitionSet ps = new PartitionSet();
    int[] num = {1, 2, 3, 9};
    System.out.println(ps.canPartition(num));
    num = new int[]{1, 2, 7, 1, 5};
    System.out.println(ps.canPartition(num));
    num = new int[]{1, 3, 100, 4};
    System.out.println(ps.canPartition(num));
  }
}
```

### Time and Space complexity [#](https://www.educative.io/courses/grokking-the-coding-interview/mE53y85Wqw9#time-and-space-complexity-2)

The above solution has the time and space complexity of O(N*S), where ‘N’ represents total numbers and ‘S’ is the total sum of all the numbers.

# Count of Subset Sum (hard) [#](https://www.educative.io/courses/grokking-the-coding-interview/qZgJyPqwJ80#count-of-subset-sum-hard)

Given a set of positive numbers, find the total number of subsets whose sum is equal to a given number ‘S’.

### Example 1: [#](https://www.educative.io/courses/grokking-the-coding-interview/qZgJyPqwJ80#example-1)

```
Input: {1, 1, 2, 3}, S=4
Output: 3
The given set has '3' subsets whose sum is '4': {1, 1, 2}, {1, 3}, {1, 3}
Note that we have two similar sets {1, 3}, because we have two '1' in our input.
```

### Example 2: [#](https://www.educative.io/courses/grokking-the-coding-interview/qZgJyPqwJ80#example-2)

```
Input: {1, 2, 7, 1, 5}, S=9
Output: 3
The given set has '3' subsets whose sum is '9': {2, 7}, {1, 7, 1}, {1, 2, 1, 5
```

## Basic Solution [#](https://www.educative.io/courses/grokking-the-coding-interview/3w5ZMYAOLMA#basic-solution)

This problem follows the **0/1 Knapsack pattern** and is quite similar to [Subset Sum](https://www.educative.io/collection/page/5668639101419520/5671464854355968/6126968124735488/). The only difference in this problem is that we need to count the number of subsets, whereas in [Subset Sum](https://www.educative.io/collection/page/5668639101419520/5671464854355968/6126968124735488/) we only wanted to know if a subset with the given sum existed.

A basic brute-force solution could be to try all subsets of the given numbers to count the subsets that have a sum equal to ‘S’. So our brute-force algorithm will look like:

```
for each number 'i' 
  create a new set which includes number 'i' if it does not exceed 'S', and recursively   
      process the remaining numbers and sum
  create a new set without number 'i', and recursively process the remaining numbers 
return the count of subsets who has a sum equal to 'S'
```

### Code [#](https://www.educative.io/courses/grokking-the-coding-interview/3w5ZMYAOLMA#code)

Here is the code for the brute-force solution:

```java
class SubsetSum {

  public int countSubsets(int[] num, int sum) {
    return this.countSubsetsRecursive(num, sum, 0);
  }

  private int countSubsetsRecursive(int[] num, int sum, int currentIndex) {
    // base checks
    if (sum == 0)
      return 1;

    if(num.length == 0 || currentIndex >= num.length)
      return 0;

    // recursive call after selecting the number at the currentIndex
    // if the number at currentIndex exceeds the sum, we shouldn't process this
    int sum1 = 0;
    if( num[currentIndex] <= sum )
      sum1 = countSubsetsRecursive(num, sum - num[currentIndex], currentIndex + 1);

    // recursive call after excluding the number at the currentIndex
    int sum2 = countSubsetsRecursive(num, sum, currentIndex + 1);

    return sum1 + sum2;
  }

  public static void main(String[] args) {
    SubsetSum ss = new SubsetSum();
    int[] num = {1, 1, 2, 3};
    System.out.println(ss.countSubsets(num, 4));
    num = new int[]{1, 2, 7, 1, 5};
    System.out.println(ss.countSubsets(num, 9));
  }
}
```

### Time and Space complexity [#](https://www.educative.io/courses/grokking-the-coding-interview/3w5ZMYAOLMA#time-and-space-complexity)

The time complexity of the above algorithm is exponential O(2^n), where ‘n’ represents the total number. The space complexity is O(n), this memory is used to store the recursion stack.

## Top-down Dynamic Programming with Memoization [#](https://www.educative.io/courses/grokking-the-coding-interview/3w5ZMYAOLMA#top-down-dynamic-programming-with-memoization)

We can use memoization to overcome the overlapping sub-problems. We will be using a two-dimensional array to store the results of solved sub-problems. As mentioned above, we need to store results for every subset and for every possible sum.

### Code [#](https://www.educative.io/courses/grokking-the-coding-interview/3w5ZMYAOLMA#code-2)

Here is the code:

```java
class SubsetSum {

  public int countSubsets(int[] num, int sum) {
    Integer[][] dp = new Integer[num.length][sum + 1];
    return this.countSubsetsRecursive(dp, num, sum, 0);
  }

  private int countSubsetsRecursive(Integer[][] dp, int[] num, int sum, int currentIndex) {
    // base checks
    if (sum == 0)
      return 1;

    if(num.length == 0 || currentIndex >= num.length)
      return 0;

    // check if we have not already processed a similar problem
    if(dp[currentIndex][sum] == null) {
      // recursive call after choosing the number at the currentIndex
      // if the number at currentIndex exceeds the sum, we shouldn't process this
      int sum1 = 0;
      if( num[currentIndex] <= sum )
        sum1 = countSubsetsRecursive(dp, num, sum - num[currentIndex], currentIndex + 1);

      // recursive call after excluding the number at the currentIndex
      int sum2 = countSubsetsRecursive(dp, num, sum, currentIndex + 1);

      dp[currentIndex][sum] = sum1 + sum2;
    }

    return dp[currentIndex][sum];
  }

  public static void main(String[] args) {
    SubsetSum ss = new SubsetSum();
    int[] num = {1, 1, 2, 3};
    System.out.println(ss.countSubsets(num, 4));
    num = new int[]{1, 2, 7, 1, 5};
    System.out.println(ss.countSubsets(num, 9));
  }
}
```

## Bottom-up Dynamic Programming [#](https://www.educative.io/courses/grokking-the-coding-interview/3w5ZMYAOLMA#bottom-up-dynamic-programming)

We will try to find if we can make all possible sums with every subset to populate the array `db[TotalNumbers][S+1`].

So, at every step we have two options:

1. Exclude the number. Count all the subsets without the given number up to the given sum => `dp[index-1][sum]`
2. Include the number if its value is not more than the ‘sum’. In this case, we will count all the subsets to get the remaining sum => `dp[index-1][sum-num[index]]`

To find the total sets, we will add both of the above two values:

```
    dp[index][sum] = dp[index-1][sum] + dp[index-1][sum-num[index]])
```

Let’s start with our base case of size zero:

![image-20210621162332376](/Users/zhengwenxuan/Desktop/Grokking-Series/Grokking-the-Coding-Interview-Patterns-for-Coding-Questions/image-20210621162332376.png)

![image-20210621162338736](/Users/zhengwenxuan/Desktop/Grokking-Series/Grokking-the-Coding-Interview-Patterns-for-Coding-Questions/image-20210621162338736.png)

![image-20210621162344864](/Users/zhengwenxuan/Desktop/Grokking-Series/Grokking-the-Coding-Interview-Patterns-for-Coding-Questions/image-20210621162344864.png)

![image-20210621162351717](/Users/zhengwenxuan/Desktop/Grokking-Series/Grokking-the-Coding-Interview-Patterns-for-Coding-Questions/image-20210621162351717.png)

![image-20210621162358617](/Users/zhengwenxuan/Desktop/Grokking-Series/Grokking-the-Coding-Interview-Patterns-for-Coding-Questions/image-20210621162358617.png)

![image-20210621162404570](/Users/zhengwenxuan/Desktop/Grokking-Series/Grokking-the-Coding-Interview-Patterns-for-Coding-Questions/image-20210621162404570.png)

![image-20210621162410284](/Users/zhengwenxuan/Desktop/Grokking-Series/Grokking-the-Coding-Interview-Patterns-for-Coding-Questions/image-20210621162410284.png)

![image-20210621162415711](/Users/zhengwenxuan/Desktop/Grokking-Series/Grokking-the-Coding-Interview-Patterns-for-Coding-Questions/image-20210621162415711.png)

![image-20210621162420888](/Users/zhengwenxuan/Desktop/Grokking-Series/Grokking-the-Coding-Interview-Patterns-for-Coding-Questions/image-20210621162420888.png)

![image-20210621162427565](/Users/zhengwenxuan/Desktop/Grokking-Series/Grokking-the-Coding-Interview-Patterns-for-Coding-Questions/image-20210621162427565.png)

![image-20210621162433083](/Users/zhengwenxuan/Desktop/Grokking-Series/Grokking-the-Coding-Interview-Patterns-for-Coding-Questions/image-20210621162433083.png)

![image-20210621162439030](/Users/zhengwenxuan/Desktop/Grokking-Series/Grokking-the-Coding-Interview-Patterns-for-Coding-Questions/image-20210621162439030.png)

### Code [#](https://www.educative.io/courses/grokking-the-coding-interview/3w5ZMYAOLMA#code-3)

Here is the code for our bottom-up dynamic programming approach:

```java
class SubsetSum {

  public int countSubsets(int[] num, int sum) {
    int n = num.length;
    int[][] dp = new int[n][sum + 1];

    // populate the sum=0 columns, as we will always have an empty set for zero sum
    for(int i=0; i < n; i++)
      dp[i][0] = 1;

    // with only one number, we can form a subset only when the required sum is equal to its value
    for(int s=1; s <= sum ; s++) {
      dp[0][s] = (num[0] == s ? 1 : 0);
    }

    // process all subsets for all sums
    for(int i=1; i < num.length; i++) {
      for(int s=1; s <= sum; s++) {
        // exclude the number
        dp[i][s] = dp[i-1][s];
        // include the number, if it does not exceed the sum
        if(s >= num[i])
          dp[i][s] += dp[i-1][s-num[i]];
      }
    }

    // the bottom-right corner will have our answer.
    return dp[num.length-1][sum];
  }

  public static void main(String[] args) {
    SubsetSum ss = new SubsetSum();
    int[] num = {1, 1, 2, 3};
    System.out.println(ss.countSubsets(num, 4));
    num = new int[]{1, 2, 7, 1, 5};
    System.out.println(ss.countSubsets(num, 9));
  }
}

```

### Time and Space complexity [#](https://www.educative.io/courses/grokking-the-coding-interview/3w5ZMYAOLMA#time-and-space-complexity-2)

The above solution has the time and space complexity of O(N*S), where ‘N’ represents total numbers and ‘S’ is the desired sum.

## Challenge [#](https://www.educative.io/courses/grokking-the-coding-interview/3w5ZMYAOLMA#challenge)

Can we improve our bottom-up DP solution even further? Can you find an algorithm that has O(S) space complexity?

```java
class SubsetSum {
  static int countSubsets(int[] num, int sum) {
    int n = num.length;
    int[] dp = new int[sum + 1];
    dp[0] = 1;

    // with only one number, we can form a subset only when the required sum is equal to its value
    for(int s=1; s <= sum ; s++) {
      dp[s] = (num[0] == s ? 1 : 0);
    }

    // process all subsets for all sums
    for(int i=1; i < num.length; i++) {
      for(int s=sum; s >= 0; s--) {
        if(s >= num[i])
          dp[s] += dp[s-num[i]];
      }
    }

    return dp[sum];
  }
}
```

# Target Sum (hard) [#](https://www.educative.io/courses/grokking-the-coding-interview/qZWW7Ny0Dk3#target-sum-hard)

You are given a set of positive numbers and a target sum ‘S’. Each number should be assigned either a ‘+’ or ‘-’ sign. We need to find the total ways to assign symbols to make the sum of the numbers equal to the target ‘S’.

### Example 1: [#](https://www.educative.io/courses/grokking-the-coding-interview/qZWW7Ny0Dk3#example-1)

```
Input: {1, 1, 2, 3}, S=1
Output: 3
Explanation: The given set has '3' ways to make a sum of '1': {+1-1-2+3} & {-1+1-2+3} & {+1+1+2-3}
```

### Example 2: [#](https://www.educative.io/courses/grokking-the-coding-interview/qZWW7Ny0Dk3#example-2)

```
Input: {1, 2, 7, 1}, S=9
Output: 2
Explanation: The given set has '2' ways to make a sum of '9': {+1+2+7-1} & {-1+2+7+1}
```

## Solution [#](https://www.educative.io/courses/grokking-the-coding-interview/JE0BWB8DgAJ#solution)

This problem follows the **0/1 Knapsack pattern** and can be converted into [Count of Subset Sum](https://www.educative.io/collection/page/5668639101419520/5671464854355968/4874044023242752/). Let’s dig into this.

We are asked to find two subsets of the given numbers whose difference is equal to the given target ‘S’. Take the first example above. As we saw, one solution is {+1-1-2+3}. So, the two subsets we are asked to find are {1, 3} & {1, 2} because,

```
    (1 + 3) - (1 + 2 ) = 1
```

Now, let’s say ‘Sum(s1)’ denotes the total sum of set ‘s1’, and ‘Sum(s2)’ denotes the total sum of set ‘s2’. So the required equation is:

```
    Sum(s1) - Sum(s2) = S
```

This equation can be reduced to the subset sum problem. Let’s assume that ‘Sum(num)’ denotes the total sum of all the numbers, therefore:

```
    Sum(s1) + Sum(s2) = Sum(num)
```

Let’s add the above two equations:

```
    => Sum(s1) - Sum(s2) + Sum(s1) + Sum(s2) = S + Sum(num)
    => 2 * Sum(s1) =  S + Sum(num)
    => Sum(s1) = (S + Sum(num)) / 2
```

Which means that one of the set ‘s1’ has a sum equal to `(S + Sum(num)) / 2`. This essentially converts our problem to: "Find the count of subsets of the given numbers whose sum is equal to `(S + Sum(num)) / 2`"

### Code [#](https://www.educative.io/courses/grokking-the-coding-interview/JE0BWB8DgAJ#code)

Let’s take the dynamic programming code of [Count of Subset Sum](https://www.educative.io/collection/page/5668639101419520/5671464854355968/4874044023242752/) and extend it to solve this problem:

```java
class TargetSum {

  public int findTargetSubsets(int[] num, int s) {
    int totalSum = 0;
    for (int n : num)
        totalSum += n;

    // if 's + totalSum' is odd, we can't find a subset with sum equal to '(s + totalSum) / 2'
    if(totalSum < s || (s + totalSum) % 2 == 1)
        return 0;

    return countSubsets(num, (s + totalSum) / 2);
  }

  // this function is exactly similar to what we have in 'Count of Subset Sum' problem.
  private int countSubsets(int[] num, int sum) {
    int n = num.length;
    int[][] dp = new int[n][sum + 1];

    // populate the sum=0 columns, as we will always have an empty set for zero sum
    for(int i=0; i < n; i++)
      dp[i][0] = 1;

    // with only one number, we can form a subset only when the required sum is equal to the number
    for(int s=1; s <= sum ; s++) {
      dp[0][s] = (num[0] == s ? 1 : 0);
    }

    // process all subsets for all sums
    for(int i=1; i < num.length; i++) {
      for(int s=1; s <= sum; s++) {
          dp[i][s] = dp[i-1][s];
          if(s >= num[i])
            dp[i][s] += dp[i-1][s-num[i]];
      }
    }

    // the bottom-right corner will have our answer.
    return dp[num.length-1][sum];
  }

  public static void main(String[] args) {
    TargetSum ts = new TargetSum();
    int[] num = {1, 1, 2, 3};
    System.out.println(ts.findTargetSubsets(num, 1));
    num = new int[]{1, 2, 7, 1};
    System.out.println(ts.findTargetSubsets(num, 9));
  }
}
```

### Time and Space complexity [#](https://www.educative.io/courses/grokking-the-coding-interview/JE0BWB8DgAJ#time-and-space-complexity)

The above solution has time and space complexity of O(N*S), where ‘N’ represents total numbers and ‘S’ is the desired sum.

We can further improve the solution to use only O(S) space.

## Space Optimized Solution [#](https://www.educative.io/courses/grokking-the-coding-interview/JE0BWB8DgAJ#space-optimized-solution)

Here is the code for the space-optimized solution, using only a single array:

```java
class TargetSum {

  public int findTargetSubsets(int[] num, int s) {
    int totalSum = 0;
    for (int n : num)
        totalSum += n;

    // if 's + totalSum' is odd, we can't find a subset with sum equal to '(s + totalSum) / 2'
    if(totalSum < s || (s + totalSum) % 2 == 1)
        return 0;

    return countSubsets(num, (s + totalSum) / 2);
  }

  // this function is exactly similar to what we have in 'Count of Subset Sum' problem.
  private int countSubsets(int[] num, int sum) {
    int n = num.length;
    int[] dp = new int[sum + 1];
    dp[0] = 1;

    // with only one number, we can form a subset only when the required sum is equal to the number
    for(int s=1; s <= sum ; s++) {
      dp[s] = (num[0] == s ? 1 : 0);
    }

    // process all subsets for all sums
    for(int i=1; i < num.length; i++) {
      for(int s=sum; s >= 0; s--) {
          if(s >= num[i])
            dp[s] += dp[s-num[i]];
      }
    }

    return dp[sum];
  }

  public static void main(String[] args) {
    TargetSum ts = new TargetSum();
    int[] num = {1, 1, 2, 3};
    System.out.println(ts.findTargetSubsets(num, 1));
    num = new int[]{1, 2, 7, 1};
    System.out.println(ts.findTargetSubsets(num, 9));
  }
}
```

