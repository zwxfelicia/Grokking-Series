# Introduction

XOR is a logical bitwise operator that returns 0 (false) if both bits are the same and returns 1 (true) otherwise. In other words, it only returns 1 if exactly one bit is set to 1 out of the two bits in comparison.

![image-20210620181413019](/Users/zhengwenxuan/Desktop/Grokking-Series/Grokking-the-Coding-Interview-Patterns-for-Coding-Questions/image-20210620181413019.png)

It is surprising to know the approaches that the XOR operator enables us to solve certain problems. For example, let’s take a look at the following problem:

> Given an array of n-1*n*−1 integers in the range from 11 to n*n*, find the one number that is missing from the array.

**Example:**

```
Input: 1, 5, 2, 6, 4
Answer: 3
```

A straight forward approach to solve this problem can be:

1. Find the sum of all integers from 11 to n*n*; let’s call it `s1`.
2. Subtract all the numbers in the input array from `s1`; this will give us the missing number.

This is what the algorithm will look like:

```java
class MissingNumber {

  public static int findMissingNumber(int[] arr) {
    int n = arr.length + 1;
    // find sum of all numbers from 1 to n.
    int s1 = 0;
    for (int i = 1; i <= n; i++)
      s1 += i;

    // subtract all numbers in input from sum.
    for (int num : arr)
      s1 -= num;

    // s1, now, is the missing number
    return s1;
  }

  public static void main(String[] args) {
    int[] arr = new int[] { 1, 5, 2, 6, 4 };
    System.out.print("Missing number is: " + MissingNumber.findMissingNumber(arr));
  }
}
```

**Time & Space complexity:** The time complexity of the above algorithm is O(n)*O*(*n*) and the space complexity is O(1)*O*(1).

## What could go wrong with the above algorithm? [#](https://www.educative.io/courses/grokking-the-coding-interview/RLPGq6Vx0YY#what-could-go-wrong-with-the-above-algorithm)

> While finding the sum of numbers from 1 to n*n*, we can get integer overflow when n*n* is large.

How can we avoid this? Can XOR help us here?

Remember the important property of XOR that it returns 0 if both the bits in comparison are the same. In other words, XOR of a number with itself will always result in 0. This means that if we XOR all the numbers in the input array with all numbers from the range 11 to n*n* then each number in the input is going to get zeroed out except the missing number. Following are the set of steps to find the missing number using XOR:

1. XOR all the numbers from 1 to n*n*, let’s call it `x1`.
2. XOR all the numbers in the input array, let’s call it `x2`.
3. The missing number can be found by `x1 XOR x2`.

Here is what the algorithm will look like:

```java
class MissingNumber {

  public static int findMissingNumber(int[] arr) {
    int n = arr.length + 1;
    // find sum of all numbers from 1 to n.
    int x1 = 1;
    for (int i = 2; i <= n; i++)
      x1 = x1 ^ i;

    // x2 represents XOR of all values in arr
    int x2 = arr[0];
    for (int i = 1; i < n-1; i++)
      x2 = x2 ^ arr[i];

    // missing number is the xor of x1 and x2
    return x1 ^ x2;
  }

  public static void main(String[] args) {
    int[] arr = new int[] { 1, 5, 2, 6, 4 };
    System.out.print("Missing number is: " + MissingNumber.findMissingNumber(arr));
  }
}
```

**Time & Space complexity:** The time complexity of the above algorithm is O(n)*O*(*n*) and the space complexity is O(1)*O*(1). The time and space complexities are the same as that of the previous solution but, in this algorithm, we will not have any integer overflow problem.

## Important properties of XOR to remember [#](https://www.educative.io/courses/grokking-the-coding-interview/RLPGq6Vx0YY#important-properties-of-xor-to-remember)

Following are some important properties of XOR to remember:

- Taking XOR of a number with itself returns 0, e.g.,
  - 1 ^ 1 = 0
  - 29 ^ 29 = 0
- Taking XOR of a number with 0 returns the same number, e.g.,
  - 1 ^ 0 = 1
  - 31 ^ 0 = 31
- XOR is Associative & Commutative, which means:
  - (a ^ b) ^ c = a ^ (b ^ c)
  - a ^ b = b ^ a

In the following chapters, we will apply the XOR pattern to solve some interesting problems.

# Single Number (easy)

## Problem Statement [#](https://www.educative.io/courses/grokking-the-coding-interview/gk20xz4VwpG#problem-statement)

In a non-empty array of integers, every number appears twice except for one, find that single number.

**Example 1:**

```
Input: 1, 4, 2, 1, 3, 2, 3
Output: 4
```

**Example 2:**

```
Input: 7, 9, 7
Output: 9
```

## Solution [#](https://www.educative.io/courses/grokking-the-coding-interview/gk20xz4VwpG#solution)

One straight forward solution can be to use a **HashMap** kind of data structure and iterate through the input:

- If number is already present in **HashMap**, remove it.
- If number is not present in **HashMap**, add it.
- In the end, only number left in the **HashMap** is our required single number.

**Time and space complexity** Time Complexity of the above solution will be O(n)*O*(*n*) and space complexity will also be O(n)*O*(*n*).

Can we do better than this using the **XOR Pattern**?

### Solution with XOR [#](https://www.educative.io/courses/grokking-the-coding-interview/gk20xz4VwpG#solution-with-xor)

Recall the following two properties of XOR:

- It returns zero if we take XOR of two same numbers.
- It returns the same number if we XOR with zero.

So we can XOR all the numbers in the input; duplicate numbers will zero out each other and we will be left with the single number.

## Code [#](https://www.educative.io/courses/grokking-the-coding-interview/gk20xz4VwpG#code)

Here is what our algorithm will look like:

```java
class SingleNumber {
  public static int findSingleNumber(int[] arr) {
    int num = 0;
    for (int i=0; i < arr.length; i++) {
      num = num ^ arr[i];
    }
    return num;
  }

  public static void main( String args[] ) {
    System.out.println(findSingleNumber(new int[]{1, 4, 2, 1, 3, 2, 3}));
  }
}
```

**Time Complexity**: Time complexity of this solution is O(n) as we iterate through all numbers of the input once.

**Space Complexity**: The algorithm runs in constant space O(1).

# Two Single Numbers (medium)

## Problem Statement [#](https://www.educative.io/courses/grokking-the-coding-interview/N7VMDGgr9Vm#problem-statement)

In a non-empty array of numbers, every number appears exactly twice except two numbers that appear only once. Find the two numbers that appear only once.

**Example 1:**

```
Input: [1, 4, 2, 1, 3, 5, 6, 2, 3, 5]
Output: [4, 6]
```

**Example 2:**

```
Input: [2, 1, 3, 2]
Output: [1, 3]
```

## Solution [#](https://www.educative.io/courses/grokking-the-coding-interview/N7VMDGgr9Vm#solution)

This problem is quite similar to [Single Number](https://www.educative.io/pageeditor/5668639101419520/5671464854355968/5053280917913600), the only difference is that, in this problem, we have two single numbers instead of one. Can we still use XOR to solve this problem?

Let’s assume `num1` and `num2` are the two single numbers. If we do XOR of all elements of the given array, we will be left with XOR of `num1` and `num2` as all other numbers will cancel each other because all of them appeared twice. Let’s call this XOR `n1xn2`. Now that we have XOR of `num1` and `num2`, how can we find these two single numbers?

As we know that `num1` and `num2` are two different numbers, therefore, they should have at least one bit different between them. If a bit in `n1xn2` is ‘1’, this means that `num1` and `num2` have different bits in that place, as we know that we can get ‘1’ only when we do XOR of two different bits, i.e.,

```
1 XOR 0 = 0 XOR 1 = 1
```

We can take any bit which is ‘1’ in `n1xn2` and partition all numbers in the given array into two groups based on that bit. One group will have all those numbers with that bit set to ‘0’ and the other with the bit set to ‘1’. This will ensure that `num1` will be in one group and `num2` will be in the other. We can take XOR of all numbers in each group separately to get `num1` and `num2`, as all other numbers in each group will cancel each other. Here are the steps of our algorithm:

1. Taking XOR of all numbers in the given array will give us XOR of `num1` and `num2`, calling this XOR as `n1xn2`.
2. Find any bit which is set in `n1xn2`. We can take the rightmost bit which is ‘1’. Let’s call this `rightmostSetBit`.
3. Iterate through all numbers of the input array to partition them into two groups based on `rightmostSetBit`. Take XOR of all numbers in both the groups separately. Both these XORs are our required numbers.

### Code [#](https://www.educative.io/courses/grokking-the-coding-interview/N7VMDGgr9Vm#code)

Here is what our algorithm will look like:

```java
class TwoSingleNumbers {

  public static int[] findSingleNumbers(int[] nums) {
    // get the XOR of the all the numbers
    int n1xn2 = 0;
    for (int num : nums) {
      n1xn2 ^= num;
    }

    // get the rightmost bit that is '1'
    int rightmostSetBit = 1;
    while ((rightmostSetBit & n1xn2) == 0) {
      rightmostSetBit = rightmostSetBit << 1;
    }
    int num1 = 0, num2 = 0;
    for (int num : nums) {
      if ((num & rightmostSetBit) != 0) // the bit is set
        num1 ^= num;
      else // the bit is not set
        num2 ^= num;
    }
    return new int[] { num1, num2 };
  }

  public static void main(String[] args) {
    int[] arr = new int[] { 1, 4, 2, 1, 3, 5, 6, 2, 3, 5 };
    int[] result = TwoSingleNumbers.findSingleNumbers(arr);
    System.out.println("Single numbers are: " + result[0] + ", " + result[1]);

    arr = new int[] { 2, 1, 3, 2 };
    result = TwoSingleNumbers.findSingleNumbers(arr);
    System.out.println("Single numbers are: " + result[0] + ", " + result[1]);
  }
}
```

### Time Complexity [#](https://www.educative.io/courses/grokking-the-coding-interview/N7VMDGgr9Vm#time-complexity)

The time complexity of this solution is O(n) where ‘n’ is the number of elements in the input array.

### Space Complexity [#](https://www.educative.io/courses/grokking-the-coding-interview/N7VMDGgr9Vm#space-complexity)

The algorithm runs in constant space O(1).

# Complement of Base 10 Number (medium)

## Problem Statement [#](https://www.educative.io/courses/grokking-the-coding-interview/N0yqGR18jND#problem-statement)

Every non-negative integer N has a binary representation, for example, 8 can be represented as “1000” in binary and 7 as “0111” in binary.

The complement of a binary representation is the number in binary that we get when we change every 1 to a 0 and every 0 to a 1. For example, the binary complement of “1010” is “0101”.

For a given positive number N in base-10, return the complement of its binary representation as a base-10 integer.

**Example 1:**

```
Input: 8
Output: 7
Explanation: 8 is 1000 in binary, its complement is 0111 in binary, which is 7 in base-10.
```

**Example 2:**

```
Input: 10
Output: 5
Explanation: 10 is 1010 in binary, its complement is 0101 in binary, which is 5 in base-10.
```

## Solution [#](https://www.educative.io/courses/grokking-the-coding-interview/N0yqGR18jND#solution)

Recall the following properties of XOR:

1. It will return 1 if we take XOR of two different bits i.e. `1^0 = 0^1 = 1`.
2. It will return 0 if we take XOR of two same bits i.e. `0^0 = 1^1 = 0`. In other words, XOR of two same numbers is 0.
3. It returns the same number if we XOR with 0.

From the above-mentioned first property, we can conclude that XOR of a number with its complement will result in a number that has all of its bits set to 1. For example, the binary complement of “101” is “010”; and if we take XOR of these two numbers, we will get a number with all bits set to 1, i.e., `101 ^ 010 = 111`

We can write this fact in the following equation:

```
number ^ complement = all_bits_set
```

Let’s add ‘number’ on both sides:

```
number ^ number ^ complement = number ^ all_bits_set
```

From the above-mentioned second property:

```
0 ^ complement = number ^ all_bits_set
```

From the above-mentioned third property:

```
complement = number ^ all_bits_set
```

We can use the above fact to find the complement of any number.

**How do we calculate ‘all_bits_set’?** One way to calculate `all_bits_set` will be to first count the bits required to store the given number. We can then use the fact that for a number which is a complete power of ‘2’ i.e., it can be written as pow(2, n), if we subtract ‘1’ from such a number, we get a number which has ‘n’ least significant bits set to ‘1’. For example, ‘4’ which is a complete power of ‘2’, and ‘3’ (which is one less than 4) has a binary representation of ‘11’ i.e., it has ‘2’ least significant bits set to ‘1’.

## Code [#](https://www.educative.io/courses/grokking-the-coding-interview/N0yqGR18jND#code)

Here is what our algorithm will look like:

```java
import java.lang.Math;

class CalculateComplement {
  public static int bitwiseComplement(int num) {
    // count number of total bits in 'num'
    int bitCount = 0;
    int n = num;
    while (n > 0) {
      bitCount++;
      n = n >> 1;
    }

    // for a number which is a complete power of '2' i.e., it can be written as pow(2, n), if we
    // subtract '1' from such a number, we get a number which has 'n' least significant bits set to '1'.
    // For example, '4' which is a complete power of '2', and '3' (which is one less than 4) has a binary 
    // representation of '11' i.e., it has '2' least significant bits set to '1' 
    int all_bits_set = (int) Math.pow(2, bitCount) - 1;

    // from the solution description: complement = number ^ all_bits_set
    return num ^ all_bits_set;
  }

  public static void main(String[] args) {
    System.out.println("Bitwise complement is: " + CalculateComplement.bitwiseComplement(8));
    System.out.println("Bitwise complement is: " + CalculateComplement.bitwiseComplement(10));
  }
}
```

## Time Complexity [#](https://www.educative.io/courses/grokking-the-coding-interview/N0yqGR18jND#time-complexity)

Time complexity of this solution is O(b)  where ‘b’ is the number of bits required to store the given number.

## Space Complexity [#](https://www.educative.io/courses/grokking-the-coding-interview/N0yqGR18jND#space-complexity)

Space complexity of this solution is O(1).

# Problem Challenge 1

## Problem Statement (hard) [#](https://www.educative.io/courses/grokking-the-coding-interview/xoExKDNWq8n#problem-statement-hard)

Given a binary matrix representing an image, we want to flip the image horizontally, then invert it.

To flip an image horizontally means that each row of the image is reversed. For example, flipping [0, 1, 1] horizontally results in [1, 1, 0].

To invert an image means that each 0 is replaced by 1, and each 1 is replaced by 0. For example, inverting [1, 1, 0] results in [0, 0, 1].

**Example 1:**

```
Input: [
  [1,0,1],
  [1,1,1],
  [0,1,1]
]
Output: [
  [0,1,0],
  [0,0,0],
  [0,0,1]
]
```

**Explanation**: First reverse each row: `[[1,0,1],[1,1,1],[1,1,0]]`. Then, invert the image: `[[0,1,0],[0,0,0],[0,0,1]]`

**Example 2:**

```
Input: [
  [1,1,0,0],
  [1,0,0,1],
  [0,1,1,1], 
  [1,0,1,0]
]
Output: [
  [1,1,0,0],
  [0,1,1,0],
  [0,0,0,1],
  [1,0,1,0]
]
```

**Explanation**: First reverse each row: `[[0,0,1,1],[1,0,0,1],[1,1,1,0],[0,1,0,1]]`. Then invert the image: `[[1,1,0,0],[0,1,1,0],[0,0,0,1],[1,0,1,0]]`

## Solution

- **Flip**: We can flip the image in place by replacing *ith* element from left with the *ith* element from the right.
- **Invert**: We can take XOR of each element with 1. If it is 1 then it will become 0 and if it is 0 then it will become 1.

### Code [#](https://www.educative.io/courses/grokking-the-coding-interview/3j7zEJzL2y9#code)

Here is what our algorithm will look like:

```java
class Solution {
  public static int[][] flipAndInvertImage(int[][] arr) {
      int C = arr[0].length;
      for (int[] row: arr) {
          for (int i = 0; i < (C + 1) / 2; ++i) {
              int tmp = row[i] ^ 1;
              row[i] = row[C - 1 - i] ^ 1;
              row[C - 1 - i] = tmp;
          }
      }
      return arr;
  }

  public static void print(int[][] arr) {
    for(int i=0; i < arr.length; i++) {
      for (int j=0; j < arr[i].length; j++) {
        System.out.print(arr[i][j] + " ");
      }
      System.out.println();
    }

  }

  public static void main(String[] args) {
    int[][] arr = {{1, 0, 1}, {1, 1, 1}, {0, 1, 1}};
    print(flipAndInvertImage(arr));
    System.out.println();

    int[][]arr2 = {{1,1,0,0},{1,0,0,1},{0,1,1,1},{1,0,1,0}};
    print(flipAndInvertImage(arr2));
  }
}
```

### Time Complexity [#](https://www.educative.io/courses/grokking-the-coding-interview/3j7zEJzL2y9#time-complexity)

The time complexity of this solution is O(n) as we iterate through all elements of the input.

### Space Complexity [#](https://www.educative.io/courses/grokking-the-coding-interview/3j7zEJzL2y9#space-complexity)

The space complexity of this solution is O(1).



