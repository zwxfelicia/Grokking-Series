# Introduction

This pattern describes an interesting approach to deal with problems involving arrays containing numbers in a given range. For example, take the following problem:

> You are given an unsorted array containing numbers taken from the range 1 to ‘n’. The array can have duplicates, which means that some numbers will be missing. Find all the missing numbers.

To efficiently solve this problem, we can use the fact that the input array contains numbers in the range of 1 to ‘n’. For example, to efficiently sort the array, we can try placing each number in its correct place, i.e., placing ‘1’ at index ‘0’, placing ‘2’ at index ‘1’, and so on. Once we are done with the sorting, we can iterate the array to find all indices that are missing the correct numbers. These will be our required numbers.

Let’s jump on to our first problem to understand the **Cyclic Sort** pattern in detail.

## Problem Statement [#](https://www.educative.io/courses/grokking-the-coding-interview/B8qXVqVwDKY#problem-statement)

We are given an array containing ‘n’ objects. Each object, when created, was assigned a unique number from 1 to ‘n’ based on their creation sequence. This means that the object with sequence number ‘3’ was created just before the object with sequence number ‘4’.

Write a function to sort the objects in-place on their creation sequence number in O(n)*O*(*n*) and without any extra space. For simplicity, let’s assume we are passed an integer array containing only the sequence numbers, though each number is actually an object.

**Example 1:**

```
Input: [3, 1, 5, 4, 2]
Output: [1, 2, 3, 4, 5]
```

**Example 2:**

```
Input: [2, 6, 4, 3, 1, 5]
Output: [1, 2, 3, 4, 5, 6]
```

**Example 3:**

```
Input: [1, 5, 6, 4, 3, 2]
Output: [1, 2, 3, 4, 5, 6]
```

## Solution [#](https://www.educative.io/courses/grokking-the-coding-interview/B8qXVqVwDKY#solution)

As we know, the input array contains numbers in the range of 1 to ‘n’. We can use this fact to devise an efficient way to sort the numbers. Since all numbers are unique, we can try placing each number at its correct place, i.e., placing ‘1’ at index ‘0’, placing ‘2’ at index ‘1’, and so on.

To place a number (or an object in general) at its correct index, we first need to find that number. If we first find a number and then place it at its correct place, it will take us O(N^2), which is not acceptable.

Instead, what if we iterate the array one number at a time, and if the current number we are iterating is not at the correct index, we swap it with the number at its correct index. This way we will go through all numbers and place them in their correct indices, hence, sorting the whole array.

Let’s see this visually with the above-mentioned Example-2:

![image-20210620170346103](/Users/zhengwenxuan/Desktop/Grokking-Series/Grokking-the-Coding-Interview-Patterns-for-Coding-Questions/image-20210620170346103.png)

## Code [#](https://www.educative.io/courses/grokking-the-coding-interview/B8qXVqVwDKY#code)

Here is what our algorithm will look like:

```java
class CyclicSort {

  public static void sort(int[] nums) {
    int i = 0;
    while (i < nums.length) {
      int j = nums[i] - 1;
      if (nums[i] != nums[j])
        swap(nums, i, j);
      else
        i++;
    }
  }

  private static void swap(int[] arr, int i, int j) {
    int temp = arr[i];
    arr[i] = arr[j];
    arr[j] = temp;
  }

  public static void main(String[] args) {
    int[] arr = new int[] { 3, 1, 5, 4, 2 };
    CyclicSort.sort(arr);
    for (int num : arr)
      System.out.print(num + " ");
    System.out.println();

    arr = new int[] { 2, 6, 4, 3, 1, 5 };
    CyclicSort.sort(arr);
    for (int num : arr)
      System.out.print(num + " ");
    System.out.println();

    arr = new int[] { 1, 5, 6, 4, 3, 2 };
    CyclicSort.sort(arr);
    for (int num : arr)
      System.out.print(num + " ");
    System.out.println();
  }
}
```

### Time complexity [#](https://www.educative.io/courses/grokking-the-coding-interview/B8qXVqVwDKY#time-complexity)

The time complexity of the above algorithm is O(n). Although we are not incrementing the index `i` when swapping the numbers, this will result in more than ‘n’ iterations of the loop, but in the worst-case scenario, the `while` loop will swap a total of ‘n-1’ numbers and once a number is at its correct index, we will move on to the next number by incrementing `i`. So overall, our algorithm will take O(n) + O(n-1) which is asymptotically equivalent to O(n).

### Space complexity [#](https://www.educative.io/courses/grokking-the-coding-interview/B8qXVqVwDKY#space-complexity)

The algorithm runs in constant space O(1).

# Find the Missing Number (easy)

## Problem Statement [#](https://www.educative.io/courses/grokking-the-coding-interview/JPnp17NYXE9#problem-statement)

We are given an array containing ‘n’ distinct numbers taken from the range 0 to ‘n’. Since the array has only ‘n’ numbers out of the total ‘n+1’ numbers, find the missing number.

**Example 1:**

```
Input: [4, 0, 3, 1]
Output: 2
```

**Example 2:**

```
Input: [8, 3, 5, 2, 4, 6, 0, 1]
Output: 7
```

## Solution [#](https://www.educative.io/courses/grokking-the-coding-interview/JPnp17NYXE9#solution)

This problem follows the **Cyclic Sort** pattern. Since the input array contains unique numbers from the range 0 to ‘n’, we can use a similar strategy as discussed in [Cyclic Sort](https://www.educative.io/collection/page/5668639101419520/5671464854355968/6003980930908160/) to place the numbers on their correct index. Once we have every number in its correct place, we can iterate the array to find the index which does not have the correct number, and that index will be our missing number.

However, there are two differences with [Cyclic Sort](https://www.educative.io/collection/page/5668639101419520/5671464854355968/6003980930908160/):

1. In this problem, the numbers are ranged from ‘0’ to ‘n’, compared to ‘1’ to ‘n’ in the

    

   Cyclic Sort

   . This will make two changes in our algorithm:

   - In this problem, each number should be equal to its index, compared to `index - 1` in the Cyclic Sort. Therefore => `nums[i] == nums[nums[i]]`
   - Since the array will have ‘n’ numbers, which means array indices will range from 0 to ‘n-1’. Therefore, we will ignore the number ‘n’ as we can’t place it in the array, so => `nums[i] < nums.length`

2. Say we are at index `i`. If we swap the number at index `i` to place it at the correct index, we can still have the wrong number at index `i`. This was true in Cyclic Sort too. It didn’t cause any problems in Cyclic Sort as over there, we made sure to place one number at its correct place in each step, but that wouldn’t be enough in this problem as we have one extra number due to the larger range. Therefore, we will not move to the next number after the swap until we have a correct number at the index `i`.

## Code [#](https://www.educative.io/courses/grokking-the-coding-interview/JPnp17NYXE9#code)

Here is what our algorithm will look like:

```java
class MissingNumber {

  public static int findMissingNumber(int[] nums) {
    int i = 0;
    while (i < nums.length) {
      if (nums[i] < nums.length && nums[i] != nums[nums[i]])
        swap(nums, i, nums[i]);
      else
        i++;
    }

    // find the first number missing from its index, that will be our required number
    for (i = 0; i < nums.length; i++)
      if (nums[i] != i)
        return i;

    return nums.length;
  }

  private static void swap(int[] arr, int i, int j) {
    int temp = arr[i];
    arr[i] = arr[j];
    arr[j] = temp;
  }

  public static void main(String[] args) {
    System.out.println(MissingNumber.findMissingNumber(new int[] { 4, 0, 3, 1 }));
    System.out.println(MissingNumber.findMissingNumber(new int[] { 8, 3, 5, 2, 4, 6, 0, 1 }));
  }
}
```

### Time complexity [#](https://www.educative.io/courses/grokking-the-coding-interview/JPnp17NYXE9#time-complexity)

The time complexity of the above algorithm is O(n). In the `while` loop, although we are not incrementing the index `i` when swapping the numbers, this will result in more than `n` iterations of the loop, but in the worst-case scenario, the `while` loop will swap a total of `n-1` numbers and once a number is at its correct index, we will move on to the next number by incrementing `i`. In the end, we iterate the input array again to find the first number missing from its index, so overall, our algorithm will take O(n) + O(n-1) + O(n)which is asymptotically equivalent to O(n)

### Space complexity [#](https://www.educative.io/courses/grokking-the-coding-interview/JPnp17NYXE9#space-complexity)

The algorithm runs in constant space O(1).

# Find all Missing Numbers (easy)

## Problem Statement [#](https://www.educative.io/courses/grokking-the-coding-interview/Y52qNM0ljWK#problem-statement)

We are given an unsorted array containing numbers taken from the range 1 to ‘n’. The array can have duplicates, which means some numbers will be missing. Find all those missing numbers.

**Example 1:**

```
Input: [2, 3, 1, 8, 2, 3, 5, 1]
Output: 4, 6, 7
Explanation: The array should have all numbers from 1 to 8, due to duplicates 4, 6, and 7 are missing.
```

**Example 2:**

```
Input: [2, 4, 1, 2]
Output: 3
```

**Example 3:**

```
Input: [2, 3, 2, 1]
Output: 4
```

## Solution [#](https://www.educative.io/courses/grokking-the-coding-interview/Y52qNM0ljWK#solution)

This problem follows the **Cyclic Sort** pattern and shares similarities with [Find the Missing Number](https://www.educative.io/collection/page/5668639101419520/5671464854355968/5319932059320320/) with one difference. In this problem, there can be many duplicates whereas in ‘Find the Missing Number’ there were no duplicates and the range was greater than the length of the array.

However, we will follow a similar approach though as discussed in [Find the Missing Number](https://www.educative.io/collection/page/5668639101419520/5671464854355968/5319932059320320/) to place the numbers on their correct indices. Once we are done with the cyclic sort we will iterate the array to find all indices that are missing the correct numbers.

## Code [#](https://www.educative.io/courses/grokking-the-coding-interview/Y52qNM0ljWK#code)

Here is what our algorithm will look like:

```java
import java.util.*;

class AllMissingNumbers {

  public static List<Integer> findNumbers(int[] nums) {
    int i = 0;
    while (i < nums.length) {
      if (nums[i] != nums[nums[i] - 1])
        swap(nums, i, nums[i] - 1);
      else
        i++;
    }

    List<Integer> missingNumbers = new ArrayList<>();
    for (i = 0; i < nums.length; i++)
      if (nums[i] != i + 1)
        missingNumbers.add(i + 1);

    return missingNumbers;
  }

  private static void swap(int[] arr, int i, int j) {
    int temp = arr[i];
    arr[i] = arr[j];
    arr[j] = temp;
  }

  public static void main(String[] args) {
    List<Integer> missing = AllMissingNumbers.findNumbers(new int[] { 2, 3, 1, 8, 2, 3, 5, 1 });
    System.out.println("Missing numbers: " + missing);

    missing = AllMissingNumbers.findNumbers(new int[] { 2, 4, 1, 2 });
    System.out.println("Missing numbers: " + missing);

    missing = AllMissingNumbers.findNumbers(new int[] { 2, 3, 2, 1 });
    System.out.println("Missing numbers: " + missing);
  }
}
```

### Time complexity [#](https://www.educative.io/courses/grokking-the-coding-interview/Y52qNM0ljWK#time-complexity)

The time complexity of the above algorithm is O(n).

### Space complexity [#](https://www.educative.io/courses/grokking-the-coding-interview/Y52qNM0ljWK#space-complexity)

Ignoring the space required for the output array, the algorithm runs in constant space O(1).

# Find the Duplicate Number (easy)

## Problem Statement [#](https://www.educative.io/courses/grokking-the-coding-interview/3wEkKy6Pr9A#problem-statement)

We are given an unsorted array containing ‘n+1’ numbers taken from the range 1 to ‘n’. The array has only one duplicate but it can be repeated multiple times. **Find that duplicate number without using any extra space**. You are, however, allowed to modify the input array.

**Example 1:**

```
Input: [1, 4, 4, 3, 2]
Output: 4
```

**Example 2:**

```
Input: [2, 1, 3, 3, 5, 4]
Output: 3
```

**Example 3:**

```
Input: [2, 4, 1, 4, 4]
Output: 4
```

## Solution [#](https://www.educative.io/courses/grokking-the-coding-interview/3wEkKy6Pr9A#solution)

This problem follows the **Cyclic Sort** pattern and shares similarities with [Find the Missing Number](https://www.educative.io/collection/page/5668639101419520/5671464854355968/5319932059320320/). Following a similar approach, we will try to place each number on its correct index. Since there is only one duplicate, if while swapping the number with its index both the numbers being swapped are same, we have found our duplicate!

## Code [#](https://www.educative.io/courses/grokking-the-coding-interview/3wEkKy6Pr9A#code)

Here is what our algorithm will look like:

```java
class FindDuplicate {

  public static int findNumber(int[] nums) {
    int i = 0;
    while (i < nums.length) {
      if (nums[i] != i + 1) {
        if (nums[i] != nums[nums[i] - 1])
          swap(nums, i, nums[i] - 1);
        else // we have found the duplicate
          return nums[i];
      } else {
        i++;
      }
    }

    return -1;
  }

  private static void swap(int[] arr, int i, int j) {
    int temp = arr[i];
    arr[i] = arr[j];
    arr[j] = temp;
  }

  public static void main(String[] args) {
    System.out.println(FindDuplicate.findNumber(new int[] { 1, 4, 4, 3, 2 }));
    System.out.println(FindDuplicate.findNumber(new int[] { 2, 1, 3, 3, 5, 4 }));
    System.out.println(FindDuplicate.findNumber(new int[] { 2, 4, 1, 4, 4 }));
  }
}
```

### Time complexity [#](https://www.educative.io/courses/grokking-the-coding-interview/3wEkKy6Pr9A#time-complexity)

The time complexity of the above algorithm is O(n).

### Space complexity [#](https://www.educative.io/courses/grokking-the-coding-interview/3wEkKy6Pr9A#space-complexity)

The algorithm runs in constant space O(1`) but modifies the input array.

------

## Similar Problems [#](https://www.educative.io/courses/grokking-the-coding-interview/3wEkKy6Pr9A#similar-problems)

**Problem 1:** Can we solve the above problem in O(1) space and without modifying the input array?

**Solution:** While doing the cyclic sort, we realized that the array will have a cycle due to the duplicate number and that the start of the cycle will always point to the duplicate number. This means that we can use the fast & the slow pointer method to find the duplicate number or the start of the cycle similar to [Start of LinkedList Cycle](https://www.educative.io/collection/page/5668639101419520/5671464854355968/6360082440781824/).

```java
class DuplicateNumber {

  public static int findDuplicate(int[] arr) {
    int slow = 0, fast = 0;
    do {
      slow = arr[slow];
      fast = arr[arr[fast]];
    } while (slow != fast);

    // find cycle length
    int current = arr[slow];
    int cycleLength = 0;
    do {
      current = arr[current];
      cycleLength++;
    } while (current != arr[slow]);

    return findStart(arr, cycleLength);
  }

  private static int findStart(int[] arr, int cycleLength) {
    int pointer1 = arr[0], pointer2 = arr[0];
    // move pointer2 ahead 'cycleLength' steps
    while (cycleLength > 0) {
      pointer2 = arr[pointer2];
      cycleLength--;
    }

    // increment both pointers until they meet at the start of the cycle
    while (pointer1 != pointer2) {
      pointer1 = arr[pointer1];
      pointer2 = arr[pointer2];
    }

    return pointer1;
  }

  public static void main(String[] args) {
    System.out.println(DuplicateNumber.findDuplicate(new int[] { 1, 4, 4, 3, 2 }));
    System.out.println(DuplicateNumber.findDuplicate(new int[] { 2, 1, 3, 3, 5, 4 }));
    System.out.println(DuplicateNumber.findDuplicate(new int[] { 2, 4, 1, 4, 4 }));
  }
}
```

The time complexity of the above algorithm is O(n) and the space complexity is O(1).



# Find all Duplicate Numbers (easy)

## Problem Statement [#](https://www.educative.io/courses/grokking-the-coding-interview/RLw1Pjk1GQ0#problem-statement)

We are given an unsorted array containing ‘n’ numbers taken from the range 1 to ‘n’. The array has some numbers appearing twice, **find all these duplicate numbers without using any extra space**.

**Example 1:**

```
Input: [3, 4, 4, 5, 5]
Output: [4, 5]
```

**Example 2:**

```
Input: [5, 4, 7, 2, 3, 5, 3]
Output: [3, 5]
```

## Solution [#](https://www.educative.io/courses/grokking-the-coding-interview/RLw1Pjk1GQ0#solution)

This problem follows the **Cyclic Sort** pattern and shares similarities with [Find the Duplicate Number](https://www.educative.io/collection/page/5668639101419520/5671464854355968/4522012447866880/). Following a similar approach, we will place each number at its correct index. After that, we will iterate through the array to find all numbers that are not at the correct indices. All these numbers are duplicates.

## Code [#](https://www.educative.io/courses/grokking-the-coding-interview/RLw1Pjk1GQ0#code)

Here is what our algorithm will look like:

```java
import java.util.*;

class FindAllDuplicate {

  public static List<Integer> findNumbers(int[] nums) {
    int i = 0;
    while (i < nums.length) {
      if (nums[i] != nums[nums[i] - 1])
        swap(nums, i, nums[i] - 1);
      else
        i++;
    }

    List<Integer> duplicateNumbers = new ArrayList<>();
    for (i = 0; i < nums.length; i++) {
      if (nums[i] != i + 1)
        duplicateNumbers.add(nums[i]);
    }

    return duplicateNumbers;
  }

  private static void swap(int[] arr, int i, int j) {
    int temp = arr[i];
    arr[i] = arr[j];
    arr[j] = temp;
  }

  public static void main(String[] args) {
    List<Integer> duplicates = FindAllDuplicate.findNumbers(new int[] { 3, 4, 4, 5, 5 });
    System.out.println("Duplicates are: " + duplicates);

    duplicates = FindAllDuplicate.findNumbers(new int[] { 5, 4, 7, 2, 3, 5, 3 });
    System.out.println("Duplicates are: " + duplicates);
  }
}
```

### Time complexity [#](https://www.educative.io/courses/grokking-the-coding-interview/RLw1Pjk1GQ0#time-complexity)

The time complexity of the above algorithm is O(n).

### Space complexity [#](https://www.educative.io/courses/grokking-the-coding-interview/RLw1Pjk1GQ0#space-complexity)

Ignoring the space required for storing the duplicates, the algorithm runs in constant space O(1).

# Find the Corrupt Pair (easy) [#](https://www.educative.io/courses/grokking-the-coding-interview/mE2LVDE3wp0#find-the-corrupt-pair-easy)

We are given an unsorted array containing ‘n’ numbers taken from the range 1 to ‘n’. The array originally contained all the numbers from 1 to ‘n’, but due to a data error, one of the numbers got duplicated which also resulted in one number going missing. Find both these numbers.

**Example 1:**

```
Input: [3, 1, 2, 5, 2]
Output: [2, 4]
Explanation: '2' is duplicated and '4' is missing.
```

**Example 2:**

```
Input: [3, 1, 2, 3, 6, 4]
Output: [3, 5]
Explanation: '3' is duplicated and '5' is missing.
```

## Solution [#](https://www.educative.io/courses/grokking-the-coding-interview/N7Vw2GBQr6D#solution)

This problem follows the **Cyclic Sort** pattern and shares similarities with [Find all Duplicate Numbers](https://www.educative.io/collection/page/5668639101419520/5671464854355968/5115834240335872/). Following a similar approach, we will place each number at its correct index. Once we are done with the cyclic sort, we will iterate through the array to find the number that is not at the correct index. Since only one number got corrupted, the number at the wrong index is the duplicated number and the index itself represents the missing number.

## Code [#](https://www.educative.io/courses/grokking-the-coding-interview/N7Vw2GBQr6D#code)

Here is what our algorithm will look like:

```java
class FindCorruptNums {

  public static int[] findNumbers(int[] nums) {
    int i = 0;
    while (i < nums.length) {
      if (nums[i] != nums[nums[i] - 1])
        swap(nums, i, nums[i] - 1);
      else
        i++;
    }

    for (i = 0; i < nums.length; i++)
      if (nums[i] != i + 1)
        return new int[] { nums[i], i + 1 };

    return new int[] { -1, -1 };
  }

  private static void swap(int[] arr, int i, int j) {
    int temp = arr[i];
    arr[i] = arr[j];
    arr[j] = temp;
  }

  public static void main(String[] args) {
    int[] nums = FindCorruptNums.findNumbers(new int[] { 3, 1, 2, 5, 2 });
    System.out.println(nums[0] + ", " + nums[1]);
    nums = FindCorruptNums.findNumbers(new int[] { 3, 1, 2, 3, 6, 4 });
    System.out.println(nums[0] + ", " + nums[1]);
  }
}

```

### Time complexity [#](https://www.educative.io/courses/grokking-the-coding-interview/N7Vw2GBQr6D#time-complexity)

The time complexity of the above algorithm is O(n).

### Space complexity [#](https://www.educative.io/courses/grokking-the-coding-interview/N7Vw2GBQr6D#space-complexity)

The algorithm runs in constant space O(1).

## Find the Smallest Missing Positive Number (medium) [#](https://www.educative.io/courses/grokking-the-coding-interview/3jEXWgB5ZmM#find-the-smallest-missing-positive-number-medium)

Given an unsorted array containing numbers, find the **smallest missing positive number** in it.

**Example 1:**

```
Input: [-3, 1, 5, 4, 2]
Output: 3
Explanation: The smallest missing positive number is '3'
```

**Example 2:**

```
Input: [3, -2, 0, 1, 2]
Output: 4
```

**Example 3:**

```
Input: [3, 2, 5, 1]
Output: 4
```

## Solution [#](https://www.educative.io/courses/grokking-the-coding-interview/R1GXQ071GQ0#solution)

This problem follows the **Cyclic Sort** pattern and shares similarities with [Find the Missing Number](https://www.educative.io/collection/page/5668639101419520/5671464854355968/5319932059320320/) with one big difference. In this problem, the numbers are not bound by any range so we can have any number in the input array.

However, we will follow a similar approach though as discussed in [Find the Missing Number](https://www.educative.io/collection/page/5668639101419520/5671464854355968/5319932059320320/) to place the numbers on their correct indices and ignore all numbers that are out of the range of the array (i.e., all negative numbers and all numbers greater than or equal to the length of the array). Once we are done with the cyclic sort we will iterate the array and the first index that does not have the correct number will be the smallest missing positive number!

## Code [#](https://www.educative.io/courses/grokking-the-coding-interview/R1GXQ071GQ0#code)

Here is what our algorithm will look like:

```java
class FirstSmallestMissingPositive {

  public static int findNumber(int[] nums) {
    int i = 0;
    while (i < nums.length) {
      if (nums[i] > 0 && nums[i] <= nums.length && nums[i] != nums[nums[i] - 1])
        swap(nums, i, nums[i] - 1);
      else
        i++;
    }
    
    for (i = 0; i < nums.length; i++)
      if (nums[i] != i + 1)
        return i + 1;

    return nums.length + 1;
  }

  private static void swap(int[] arr, int i, int j) {
    int temp = arr[i];
    arr[i] = arr[j];
    arr[j] = temp;
  }

  public static void main(String[] args) {
    System.out.println(FirstSmallestMissingPositive.findNumber(new int[] { -3, 1, 5, 4, 2 }));
    System.out.println(FirstSmallestMissingPositive.findNumber(new int[] { 3, -2, 0, 1, 2 }));
    System.out.println(FirstSmallestMissingPositive.findNumber(new int[] { 3, 2, 5, 1 }));
  }
}
```

### Time complexity [#](https://www.educative.io/courses/grokking-the-coding-interview/R1GXQ071GQ0#time-complexity)

The time complexity of the above algorithm is O(n).

### Space complexity [#](https://www.educative.io/courses/grokking-the-coding-interview/R1GXQ071GQ0#space-complexity)

The algorithm runs in constant space O(1).

## Find the First K Missing Positive Numbers (hard) [#](https://www.educative.io/courses/grokking-the-coding-interview/q2LA7G0ANX0#find-the-first-k-missing-positive-numbers-hard)

Given an unsorted array containing numbers and a number ‘k’, find the first ‘k’ missing positive numbers in the array.

**Example 1:**

```
Input: [3, -1, 4, 5, 5], k=3
Output: [1, 2, 6]
Explanation: The smallest missing positive numbers are 1, 2 and 6.
```

**Example 2:**

```
Input: [2, 3, 4], k=3
Output: [1, 5, 6]
Explanation: The smallest missing positive numbers are 1, 5 and 6.
```

**Example 3:**

```
Input: [-2, -3, 4], k=2
Output: [1, 2]
Explanation: The smallest missing positive numbers are 1 and 2.
```

## Solution [#](https://www.educative.io/courses/grokking-the-coding-interview/g286M2Gk3YY#solution)

This problem follows the **Cyclic Sort** pattern and shares similarities with [Find the Smallest Missing Positive Number](https://www.educative.io/collection/page/5668639101419520/5671464854355968/5753318183796736/). The only difference is that, in this problem, we need to find the first ‘k’ missing numbers compared to only the first missing number.

We will follow a similar approach as discussed in [Find the Smallest Missing Positive Number](https://www.educative.io/collection/page/5668639101419520/5671464854355968/5753318183796736/) to place the numbers on their correct indices and ignore all numbers that are out of the range of the array. Once we are done with the cyclic sort we will iterate through the array to find indices that do not have the correct numbers.

If we are not able to find ‘k’ missing numbers from the array, we need to add additional numbers to the output array. To find these additional numbers we will use the length of the array. For example, if the length of the array is 4, the next missing numbers will be 4, 5, 6 and so on. One tricky aspect is that any of these additional numbers could be part of the array. Remember, while sorting, we ignored all numbers that are greater than or equal to the length of the array. So all indices that have the missing numbers could possibly have these additional numbers. To handle this, we must keep track of all numbers from those indices that have missing numbers. Let’s understand this with an example:

```
    nums: [2, 1, 3, 6, 5], k =2
```

After the cyclic sort our array will look like:

```
    nums: [1, 2, 3, 6, 5]
```

From the sorted array we can see that the first missing number is ‘4’ (as we have ‘6’ on the fourth index) but to find the second missing number we need to remember that the array does contain ‘6’. Hence, the next missing number is ‘7’.

## Code [#](https://www.educative.io/courses/grokking-the-coding-interview/g286M2Gk3YY#code)

Here is what our algorithm will look like:

```java
import java.util.*;

class FirstKMissingPositive {

  public static List<Integer> findNumbers(int[] nums, int k) {
    int i = 0;
    while (i < nums.length) {
      if (nums[i] > 0 && nums[i] <= nums.length && nums[i] != nums[nums[i] - 1])
        swap(nums, i, nums[i] - 1);
      else
        i++;
    }

    List<Integer> missingNumbers = new ArrayList<>();
    Set<Integer> extraNumbers = new HashSet<>();
    for (i = 0; i < nums.length && missingNumbers.size() < k; i++)
      if (nums[i] != i + 1) {
        missingNumbers.add(i + 1);
        extraNumbers.add(nums[i]);
      }

    // add the remaining missing numbers
    for (i = 1; missingNumbers.size() < k; i++) {
      int candidateNumber = i + nums.length;
      // ignore if the array contains the candidate number
      if (!extraNumbers.contains(candidateNumber))
        missingNumbers.add(candidateNumber);
    }

    return missingNumbers;
  }

  private static void swap(int[] arr, int i, int j) {
    int temp = arr[i];
    arr[i] = arr[j];
    arr[j] = temp;
  }

  public static void main(String[] args) {
    List<Integer> missingNumbers = FirstKMissingPositive.findNumbers(new int[] { 3, -1, 4, 5, 5 }, 3);
    System.out.println("Missing numbers: " + missingNumbers);

    missingNumbers = FirstKMissingPositive.findNumbers(new int[] { 2, 3, 4 }, 3);
    System.out.println("Missing numbers: " + missingNumbers);

    missingNumbers = FirstKMissingPositive.findNumbers(new int[] { -2, -3, 4 }, 2);
    System.out.println("Missing numbers: " + missingNumbers);
  }
}
```

### Time complexity [#](https://www.educative.io/courses/grokking-the-coding-interview/g286M2Gk3YY#time-complexity)

The time complexity of the above algorithm is O(n + k), as the last two `for` loops will run for O(n) and O(k) times respectively.

### Space complexity [#](https://www.educative.io/courses/grokking-the-coding-interview/g286M2Gk3YY#space-complexity)

The algorithm needs O(k) space to store the `extraNumbers`.

