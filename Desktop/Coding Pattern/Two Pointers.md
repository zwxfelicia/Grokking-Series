# Introduction

In problems where we deal with sorted arrays (or LinkedLists) and need to find a set of elements that fulfill certain constraints, the Two Pointers approach becomes quite useful. The set of elements could be a pair, a triplet or even a subarray. For example, take a look at the following problem:

> Given an array of sorted numbers and a target sum, find a pair in the array whose sum is equal to the given target.

To solve this problem, we can consider each element one by one (pointed out by the first pointer) and iterate through the remaining elements (pointed out by the second pointer) to find a pair with the given sum. The time complexity of this algorithm will be O(N^2)*O*(*N*2) where ‘N’ is the number of elements in the input array.

Given that the input array is sorted, an efficient way would be to start with one pointer in the beginning and another pointer at the end. At every step, we will see if the numbers pointed by the two pointers add up to the target sum. If they do not, we will do one of two things:

1. If the sum of the two numbers pointed by the two pointers is greater than the target sum, this means that we need a pair with a smaller sum. So, to try more pairs, we can decrement the end-pointer.
2. If the sum of the two numbers pointed by the two pointers is smaller than the target sum, this means that we need a pair with a larger sum. So, to try more pairs, we can increment the start-pointer.

Here is the visual representation of this algorithm:

![image-20210618154204420](/Users/zhengwenxuan/Desktop/Coding Pattern/image-20210618154204420.png)

The time complexity of the above algorithm will be O(N)*O*(*N*).

In the following chapters, we will apply the **Two Pointers** approach to solve a few problems.

# Pair with Target Sum (easy)

## Problem Statement 

Given an array of sorted numbers and a target sum, find a **pair in the array whose sum is equal to the given target**.

Write a function to return the indices of the two numbers (i.e. the pair) such that they add up to the given target.

**Example 1:**

```
Input: [1, 2, 3, 4, 6], target=6
Output: [1, 3]
Explanation: The numbers at index 1 and 3 add up to 6: 2+4=6
```

**Example 2:**

```
Input: [2, 5, 9, 11], target=11
Output: [0, 2]
Explanation: The numbers at index 0 and 2 add up to 11: 2+9=11
```

## Solution [#](https://www.educative.io/courses/grokking-the-coding-interview/xog6q15W9GP#solution)

Since the given array is sorted, a brute-force solution could be to iterate through the array, taking one number at a time and searching for the second number through **Binary Search**. The time complexity of this algorithm will be O(N*logN)*O*(*N*∗*l**o**g**N*). Can we do better than this?

We can follow the **Two Pointers** approach. We will start with one pointer pointing to the beginning of the array and another pointing at the end. At every step, we will see if the numbers pointed by the two pointers add up to the target sum. If they do, we have found our pair; otherwise, we will do one of two things:

1. If the sum of the two numbers pointed by the two pointers is greater than the target sum, this means that we need a pair with a smaller sum. So, to try more pairs, we can decrement the end-pointer.
2. If the sum of the two numbers pointed by the two pointers is smaller than the target sum, this means that we need a pair with a larger sum. So, to try more pairs, we can increment the start-pointer.

Here is the visual representation of this algorithm for Example-1:

![image-20210618155249180](/Users/zhengwenxuan/Desktop/Coding Pattern/image-20210618155249180.png)

```java
class PairWithTargetSum {

  public static int[] search(int[] arr, int targetSum) {
    int left = 0, right = arr.length - 1;
    while (left < right) {
      int currentSum = arr[left] + arr[right];
      if (currentSum == targetSum)
        return new int[] { left, right }; // found the pair

      if (targetSum > currentSum)
        left++; // we need a pair with a bigger sum
      else
        right--; // we need a pair with a smaller sum
    }
    return new int[] { -1, -1 };
  }

  public static void main(String[] args) {
    int[] result = PairWithTargetSum.search(new int[] { 1, 2, 3, 4, 6 }, 6);
    System.out.println("Pair with target sum: [" + result[0] + ", " + result[1] + "]");
    result = PairWithTargetSum.search(new int[] { 2, 5, 9, 11 }, 11);
    System.out.println("Pair with target sum: [" + result[0] + ", " + result[1] + "]");
  }
}
```

## Time Complexity [#](https://www.educative.io/courses/grokking-the-coding-interview/xog6q15W9GP#time-complexity)

The time complexity of the above algorithm will be O(N), where ‘N’ is the total number of elements in the given array.

## Space Complexity

The algorithm runs in constant space O(1)

## An Alternate approach [#](https://www.educative.io/courses/grokking-the-coding-interview/xog6q15W9GP#an-alternate-approach)

Instead of using a two-pointer or a binary search approach, we can utilize a **HashTable** to search for the required pair. We can iterate through the array one number at a time. Let’s say during our iteration we are at number ‘X’, so we need to find ‘Y’ such that “X + Y == Target”. We will do two things here:

1. Search for ‘Y’ (which is equivalent to “Target - X”) in the **HashTable**. If it is there, we have found the required pair.
2. Otherwise, insert “X” in the **HashTable**, so that we can search it for the later numbers.

Here is what our algorithm will look like:

````java
import java.util.HashMap;

class PairWithTargetSum {

  public static int[] search(int[] arr, int targetSum) {
    HashMap<Integer, Integer> nums = new HashMap<>(); // to store numbers and their indices
    for (int i = 0; i < arr.length; i++) {
      if (nums.containsKey(targetSum - arr[i]))
        return new int[] { nums.get(targetSum - arr[i]), i };
      else
        nums.put(arr[i], i); // put the number and its index in the map
    }
    return new int[] { -1, -1 }; // pair not found
  }

  public static void main(String[] args) {
    int[] result = PairWithTargetSum.search(new int[] { 1, 2, 3, 4, 6 }, 6);
    System.out.println("Pair with target sum: [" + result[0] + ", " + result[1] + "]");
    result = PairWithTargetSum.search(new int[] { 2, 5, 9, 11 }, 11);
    System.out.println("Pair with target sum: [" + result[0] + ", " + result[1] + "]");
  }
}
````

## Time Complexity [#](https://www.educative.io/courses/grokking-the-coding-interview/xog6q15W9GP#time-complexity-2)

The time complexity of the above algorithm will be O(N), where ‘N’ is the total number of elements in the given array.

## Space Complexity

The space complexity will also be O(N), as, in the worst case, we will be pushing ‘N’ numbers in the **HashTable**.

# Remove Duplicates (easy)

## Problem Statement [#](https://www.educative.io/courses/grokking-the-coding-interview/mEEA22L5mNA#problem-statement)

Given an array of sorted numbers, **remove all duplicates** from it. You should **not use any extra space**; after removing the duplicates in-place return the length of the subarray that has no duplicate in it.

**Example 1:**

```
Input: [2, 3, 3, 3, 6, 9, 9]
Output: 4
Explanation: The first four elements after removing the duplicates will be [2, 3, 6, 9].
```

**Example 2:**

```
Input: [2, 2, 2, 11]
Output: 2
Explanation: The first two elements after removing the duplicates will be [2, 11].
```

## Solution [#](https://www.educative.io/courses/grokking-the-coding-interview/mEEA22L5mNA#solution)

In this problem, we need to remove the duplicates in-place such that the resultant length of the array remains sorted. As the input array is sorted, therefore, one way to do this is to shift the elements left whenever we encounter duplicates. In other words, we will keep one pointer for iterating the array and one pointer for placing the next non-duplicate number. So our algorithm will be to iterate the array and whenever we see a non-duplicate number we move it next to the last non-duplicate number we’ve seen.

Here is the visual representation of this algorithm for Example-1:

![image-20210618155548960](/Users/zhengwenxuan/Desktop/Coding Pattern/image-20210618155548960.png)

## Code [#](https://www.educative.io/courses/grokking-the-coding-interview/mEEA22L5mNA#code)

Here is what our algorithm will look like:

```java
class RemoveDuplicates {

  public static int remove(int[] arr) {
    int nextNonDuplicate = 1; // index of the next non-duplicate element
    for (int i = 1; i < arr.length; i++) {
      if (arr[nextNonDuplicate - 1] != arr[i]) {
        arr[nextNonDuplicate] = arr[i];
        nextNonDuplicate++;
      }
    }

    return nextNonDuplicate;
  }

  public static void main(String[] args) {
    int[] arr = new int[] { 2, 3, 3, 3, 6, 9, 9 };
    System.out.println(RemoveDuplicates.remove(arr));

    arr = new int[] { 2, 2, 2, 11 };
    System.out.println(RemoveDuplicates.remove(arr));
  }
}
```

## Time Complexity

The time complexity of the above algorithm will be O(N), where ‘N’ is the total number of elements in the given array.

 ## Space Complexity 

The algorithm runs in constant space O(1).

------

## Similar Questions [#](https://www.educative.io/courses/grokking-the-coding-interview/mEEA22L5mNA#similar-questions)

**Problem 1:** Given an unsorted array of numbers and a target ‘key’, remove all instances of ‘key’ in-place and return the new length of the array.

**Example 1:**

```
Input: [3, 2, 3, 6, 3, 10, 9, 3], Key=3
Output: 4
Explanation: The first four elements after removing every 'Key' will be [2, 6, 10, 9].
```

**Example 2:**

```
Input: [2, 11, 2, 2, 1], Key=2
Output: 2
Explanation: The first two elements after removing every 'Key' will be [11, 1].
```

**Solution:** This problem is quite similar to our parent problem. We can follow a two-pointer approach and shift numbers left upon encountering the ‘key’. Here is what the code will look like:

```java
class RemoveElement {

  public static int remove(int[] arr, int key) {
    int nextElement = 0; // index of the next element which is not 'key'
    for (int i = 0; i < arr.length; i++) {
      if (arr[i] != key) {
        arr[nextElement] = arr[i];
        nextElement++;
      }
    }

    return nextElement;
  }

  public static void main(String[] args) {
    int[] arr = new int[] { 3, 2, 3, 6, 3, 10, 9, 3 };
    System.out.println(RemoveElement.remove(arr, 3));

    arr = new int[] { 2, 11, 2, 2, 1 };
    System.out.println(RemoveElement.remove(arr, 2));
  }
}
```

**Time and Space Complexity:** The time complexity of the above algorithm will be O(N), where ‘N’ is the total number of elements in the given array.

The algorithm runs in constant space O(1).

# Squaring a Sorted Array (easy)

## Problem Statement [#](https://www.educative.io/courses/grokking-the-coding-interview/R1ppNG3nV9R#problem-statement)

Given a sorted array, create a new array containing **squares of all the numbers of the input array** in the sorted order.

**Example 1:**

```
Input: [-2, -1, 0, 2, 3]
Output: [0, 1, 4, 4, 9]
```

**Example 2:**

```
Input: [-3, -1, 0, 1, 2]
Output: [0, 1, 1, 4, 9]
```

## Solution [#](https://www.educative.io/courses/grokking-the-coding-interview/R1ppNG3nV9R#solution)

This is a straightforward question. The only trick is that we can have negative numbers in the input array, which will make it a bit difficult to generate the output array with squares in sorted order.

An easier approach could be to first find the index of the first non-negative number in the array. After that, we can use **Two Pointers** to iterate the array. One pointer will move forward to iterate the non-negative numbers, and the other pointer will move backward to iterate the negative numbers. At any step, whichever number gives us a bigger square will be added to the output array. For the above-mentioned Example-1, we will do something like this:

![image-20210618155902705](/Users/zhengwenxuan/Desktop/Coding Pattern/image-20210618155902705.png)

Since the numbers at both ends can give us the largest square, an alternate approach could be to use two pointers starting at both ends of the input array. At any step, whichever pointer gives us the bigger square, we add it to the result array and move to the next/previous number according to the pointer. For the above-mentioned Example-1, we will do something like this:

![image-20210618155947245](/Users/zhengwenxuan/Desktop/Coding Pattern/image-20210618155947245.png)

```java
class SortedArraySquares {

  public static int[] makeSquares(int[] arr) {
    int n = arr.length;
    int[] squares = new int[n];
    int highestSquareIdx = n - 1;
    int left = 0, right = arr.length - 1;
    while (left <= right) {
      int leftSquare = arr[left] * arr[left];
      int rightSquare = arr[right] * arr[right];
      if (leftSquare > rightSquare) {
        squares[highestSquareIdx--] = leftSquare;
        left++;
      } else {
        squares[highestSquareIdx--] = rightSquare;
        right--;
      }
    }
    return squares;
  }

  public static void main(String[] args) {

    int[] result = SortedArraySquares.makeSquares(new int[] { -2, -1, 0, 2, 3 });
    for (int num : result)
      System.out.print(num + " ");
    System.out.println();

    result = SortedArraySquares.makeSquares(new int[] { -3, -1, 0, 1, 2 });
    for (int num : result)
      System.out.print(num + " ");
    System.out.println();
  }
}
```

## Time complexity [#](https://www.educative.io/courses/grokking-the-coding-interview/R1ppNG3nV9R#time-complexity)

The above algorithm’s time complexity will be O(N) as we are iterating the input array only once.

## Space complexity 

The above algorithm’s space complexity will also be O(N); this space will be used for the output array.

# Triplet Sum to Zero (medium)

## Problem Statement 

Given an array of unsorted numbers, find all **unique triplets in it that add up to zero**.

**Example 1:**

```
Input: [-3, 0, 1, 2, -1, 1, -2]
Output: [-3, 1, 2], [-2, 0, 2], [-2, 1, 1], [-1, 0, 1]
Explanation: There are four unique triplets whose sum is equal to zero.
```

**Example 2:**

```
Input: [-5, 2, -1, -2, 3]
Output: [[-5, 2, 3], [-2, -1, 3]]
Explanation: There are two unique triplets whose sum is equal to zero.
```

## Solution [#](https://www.educative.io/courses/grokking-the-coding-interview/gxk639mrr5r#solution)

This problem follows the **Two Pointers** pattern and shares similarities with [Pair with Target Sum](https://www.educative.io/collection/page/5668639101419520/5671464854355968/6618310940557312/). A couple of differences are that the input array is not sorted and instead of a pair we need to find triplets with a target sum of zero.

To follow a similar approach, first, we will sort the array and then iterate through it taking one number at a time. Let’s say during our iteration we are at number ‘X’, so we need to find ‘Y’ and ‘Z’ such that X + Y + Z == 0. At this stage, our problem translates into finding a pair whose sum is equal to “-X” (as from the above equation Y + Z == -X.

Another difference from [Pair with Target Sum](https://www.educative.io/collection/page/5668639101419520/5671464854355968/6618310940557312/) is that we need to find all the unique triplets. To handle this, we have to skip any duplicate number. Since we will be sorting the array, so all the duplicate numbers will be next to each other and are easier to skip.

## Code 

Here is what our algorithm will look like:

```java
import java.util.*;

class TripletSumToZero {

  public static List<List<Integer>> searchTriplets(int[] arr) {
    Arrays.sort(arr);
    List<List<Integer>> triplets = new ArrayList<>();
    for (int i = 0; i < arr.length - 2; i++) {
      if (i > 0 && arr[i] == arr[i - 1]) // skip same element to avoid duplicate triplets
        continue;
      searchPair(arr, -arr[i], i + 1, triplets);
    }

    return triplets;
  }

  private static void searchPair(int[] arr, int targetSum, int left, List<List<Integer>> triplets) {
    int right = arr.length - 1;
    while (left < right) {
      int currentSum = arr[left] + arr[right];
      if (currentSum == targetSum) { // found the triplet
        triplets.add(Arrays.asList(-targetSum, arr[left], arr[right]));
        left++;
        right--;
        while (left < right && arr[left] == arr[left - 1])
          left++; // skip same element to avoid duplicate triplets
        while (left < right && arr[right] == arr[right + 1])
          right--; // skip same element to avoid duplicate triplets
      } else if (targetSum > currentSum)
        left++; // we need a pair with a bigger sum
      else
        right--; // we need a pair with a smaller sum
    }
  }

  public static void main(String[] args) {
    System.out.println(TripletSumToZero.searchTriplets(new int[] { -3, 0, 1, 2, -1, 1, -2 }));
    System.out.println(TripletSumToZero.searchTriplets(new int[] { -5, 2, -1, -2, 3 }));
  }
}
```

## Time complexity

Sorting the array will take O(N * logN). The `searchPair()` function will take O(N). As we are calling `searchPair()` for every number in the input array, this means that overall `searchTriplets()` will take O(N * logN + N^2), which is asymptotically equivalent to O(N^2).

## Space complexity

Ignoring the space required for the output array, the space complexity of the above algorithm will be O(N) which is required for sorting.

# Triplet Sum Close to Target (medium)

## Problem Statement [#](https://www.educative.io/courses/grokking-the-coding-interview/3YlQz7PE7OA#problem-statement)

Given an array of unsorted numbers and a target number, find a **triplet in the array whose sum is as close to the target number as possible**, return the sum of the triplet. If there are more than one such triplet, return the sum of the triplet with the smallest sum.

**Example 1:**

```
Input: [-2, 0, 1, 2], target=2
Output: 1
Explanation: The triplet [-2, 1, 2] has the closest sum to the target.
```

**Example 2:**

```
Input: [-3, -1, 1, 2], target=1
Output: 0
Explanation: The triplet [-3, 1, 2] has the closest sum to the target.
```

**Example 3:**

```
Input: [1, 0, 1, 1], target=100
Output: 3
Explanation: The triplet [1, 1, 1] has the closest sum to the target.
```

## Solution [#](https://www.educative.io/courses/grokking-the-coding-interview/3YlQz7PE7OA#solution)

This problem follows the **Two Pointers** pattern and is quite similar to [Triplet Sum to Zero](https://www.educative.io/collection/page/5668639101419520/5671464854355968/5679549973004288/).

We can follow a similar approach to iterate through the array, taking one number at a time. At every step, we will save the difference between the triplet and the target number, so that in the end, we can return the triplet with the closest sum.

## Code

Here is what our algorithm will look like:

```java
import java.util.*;

class TripletSumCloseToTarget {

  public static int searchTriplet(int[] arr, int targetSum) {
    if (arr == null || arr.length < 3)
      throw new IllegalArgumentException();

    Arrays.sort(arr);
    int smallestDifference = Integer.MAX_VALUE;
    for (int i = 0; i < arr.length - 2; i++) {
      int left = i + 1, right = arr.length - 1;
      while (left < right) {
        // comparing the sum of three numbers to the 'targetSum' can cause overflow
        // so, we will try to find a target difference
        int targetDiff = targetSum - arr[i] - arr[left] - arr[right];
        if (targetDiff == 0) //  we've found a triplet with an exact sum
          return targetSum - targetDiff; // return sum of all the numbers

        // the second part of the above 'if' is to handle the smallest sum when we have more than one solution
        if (Math.abs(targetDiff) < Math.abs(smallestDifference)
            || (Math.abs(targetDiff) == Math.abs(smallestDifference) && targetDiff > smallestDifference))
          smallestDifference = targetDiff; // save the closest and the biggest difference  

        if (targetDiff > 0)
          left++; // we need a triplet with a bigger sum
        else
          right--; // we need a triplet with a smaller sum
      }
    }
    return targetSum - smallestDifference;
  }

  public static void main(String[] args) {
    System.out.println(TripletSumCloseToTarget.searchTriplet(new int[] { -2, 0, 1, 2 }, 2));
    System.out.println(TripletSumCloseToTarget.searchTriplet(new int[] { -3, -1, 1, 2 }, 1));
    System.out.println(TripletSumCloseToTarget.searchTriplet(new int[] { 1, 0, 1, 1 }, 100));
  }
}
```

## Time complexity 

Sorting the array will take O(N* logN). Overall, the function will take O(N * logN + N^2), which is asymptotically equivalent to O(N^2).

## Space complexity 

The above algorithm’s space complexity will be O(N), which is required for sorting.

# Triplets with Smaller Sum (medium)

## Problem Statement [#](https://www.educative.io/courses/grokking-the-coding-interview/mElknO5OKBO#problem-statement)

Given an array `arr` of unsorted numbers and a target sum, **count all triplets** in it such that **`arr[i] + arr[j] + arr[k] < target`** where `i`, `j`, and `k` are three different indices. Write a function to return the count of such triplets.

**Example 1:**

```
Input: [-1, 0, 2, 3], target=3 
Output: 2
Explanation: There are two triplets whose sum is less than the target: [-1, 0, 3], [-1, 0, 2]
```

**Example 2:**

```
Input: [-1, 4, 2, 1, 3], target=5 
Output: 4
Explanation: There are four triplets whose sum is less than the target: 
   [-1, 1, 4], [-1, 1, 3], [-1, 1, 2], [-1, 2, 3]
```

## Solution [#](https://www.educative.io/courses/grokking-the-coding-interview/mElknO5OKBO#solution)

This problem follows the **Two Pointers** pattern and shares similarities with [Triplet Sum to Zero](https://www.educative.io/collection/page/5668639101419520/5671464854355968/5679549973004288/). The only difference is that, in this problem, we need to find the triplets whose sum is less than the given target. To meet the condition `i != j != k` we need to make sure that each number is not used more than once.

Following a similar approach, first, we can sort the array and then iterate through it, taking one number at a time. Let’s say during our iteration we are at number ‘X’, so we need to find ‘Y’ and ‘Z’ such that X + Y + Z < target. At this stage, our problem translates into finding a pair whose sum is less than “target - X” (as from the above equation Y + Z == target - X. We can use a similar approach as discussed in [Triplet Sum to Zero](https://www.educative.io/collection/page/5668639101419520/5671464854355968/5679549973004288/).

## Code 

Here is what our algorithm will look like:

```java
import java.util.*;

class TripletWithSmallerSum {

  public static int searchTriplets(int[] arr, int target) {
    Arrays.sort(arr);
    int count = 0;
    for (int i = 0; i < arr.length - 2; i++) {
      count += searchPair(arr, target - arr[i], i);
    }
    return count;
  }

  private static int searchPair(int[] arr, int targetSum, int first) {
    int count = 0;
    int left = first + 1, right = arr.length - 1;
    while (left < right) {
      if (arr[left] + arr[right] < targetSum) { // found the triplet
        // since arr[right] >= arr[left], therefore, we can replace arr[right] by any number between 
        // left and right to get a sum less than the target sum
        count += right - left;
        left++;
      } else {
        right--; // we need a pair with a smaller sum
      }
    }
    return count;
  }

  public static void main(String[] args) {
    System.out.println(TripletWithSmallerSum.searchTriplets(new int[] { -1, 0, 2, 3 }, 3));
    System.out.println(TripletWithSmallerSum.searchTriplets(new int[] { -1, 4, 2, 1, 3 }, 5));
  }
}
```

## Time complexity

Sorting the array will take O(N * logN). The `searchPair()` will take O(N). So, overall `searchTriplets()` will take O(N * logN + N^2), which is asymptotically equivalent to O(N^2).

### Space complexity 

The space complexity of the above algorithm will be O(N) which is required for sorting if we are not using an in-place sorting algorithm.

## Similar Problems

**Problem:** Write a function to return the list of all such triplets instead of the count. How will the time complexity change in this case?

**Solution:** Following a similar approach we can create a list containing all the triplets. Here is the code - only the highlighted lines have changed:

```java
import java.util.*;

class TripletWithSmallerSum {

  public static List<List<Integer>> searchTriplets(int[] arr, int target) {
    Arrays.sort(arr);
    List<List<Integer>> triplets = new ArrayList<>();
    for (int i = 0; i < arr.length - 2; i++) {
      searchPair(arr, target - arr[i], i, triplets);
    }
    return triplets;
  }

  private static void searchPair(int[] arr, int targetSum, int first, List<List<Integer>> triplets) {
    int left = first + 1, right = arr.length - 1;
    while (left < right) {
      if (arr[left] + arr[right] < targetSum) { // found the triplet
        // since arr[right] >= arr[left], therefore, we can replace arr[right] by any number between 
        // left and right to get a sum less than the target sum
        for (int i = right; i > left; i--)
          triplets.add(Arrays.asList(arr[first], arr[left], arr[i]));
        left++;
      } else {
        right--; // we need a pair with a smaller sum
      }
    }
  }

  public static void main(String[] args) {
    System.out.println(TripletWithSmallerSum.searchTriplets(new int[] { -1, 0, 2, 3 }, 3));
    System.out.println(TripletWithSmallerSum.searchTriplets(new int[] { -1, 4, 2, 1, 3 }, 5));
  }
}
```

Another simpler approach could be to check every triplet of the array with three nested loops and create a list of triplets that meet the required condition.

## Time complexity

Sorting the array will take O(N * logN). The `searchPair()`, in this case, will take O(N^2); the main `while` loop will run in O(N) but the nested `for` loop can also take O(N)- this will happen when the target sum is bigger than every triplet in the array.

So, overall `searchTriplets()` will take O(N * logN + N^3), which is asymptotically equivalent to O(N^3).

## Space complexity

Ignoring the space required for the output array, the space complexity of the above algorithm will be O(N) which is required for sorting.

# Subarrays with Product Less than a Target (medium)

## Problem Statement [#](https://www.educative.io/courses/grokking-the-coding-interview/RMV1GV1yPYz#problem-statement)

Given an array with positive numbers and a target number, find all of its contiguous subarrays whose **product is less than the target number**.

**Example 1:**

```
Input: [2, 5, 3, 10], target=30 
Output: [2], [5], [2, 5], [3], [5, 3], [10]
Explanation: There are six contiguous subarrays whose product is less than the target.
```

**Example 2:**

```
Input: [8, 2, 6, 5], target=50 
Output: [8], [2], [8, 2], [6], [2, 6], [5], [6, 5] 
Explanation: There are seven contiguous subarrays whose product is less than the target.
```

## Solution [#](https://www.educative.io/courses/grokking-the-coding-interview/RMV1GV1yPYz#solution)

This problem follows the **Sliding Window** and the **Two Pointers** pattern and shares similarities with [Triplets with Smaller Sum](https://www.educative.io/collection/page/5668639101419520/5671464854355968/5554621957275648/) with two differences:

1. In this problem, the input array is not sorted.
2. Instead of finding triplets with sum less than a target, we need to find all subarrays having a product less than the target.

The implementation will be quite similar to [Triplets with Smaller Sum](https://www.educative.io/collection/page/5668639101419520/5671464854355968/5554621957275648/).

### Code [#](https://www.educative.io/courses/grokking-the-coding-interview/RMV1GV1yPYz#code)

Here is what our algorithm will look like:

```java
import java.util.*;

class SubarrayProductLessThanK {

  public static List<List<Integer>> findSubarrays(int[] arr, int target) {
    List<List<Integer>> result = new ArrayList<>();
    double product = 1;
    int left = 0;
    for (int right = 0; right < arr.length; right++) {
      product *= arr[right];
      while (product >= target && left < arr.length)
        product /= arr[left++];
      // since the product of all numbers from left to right is less than the target therefore,
      // all subarrays from left to right will have a product less than the target too; to avoid
      // duplicates, we will start with a subarray containing only arr[right] and then extend it
      List<Integer> tempList = new LinkedList<>();
      for (int i = right; i >= left; i--) {
        tempList.add(0, arr[i]);
        result.add(new ArrayList<>(tempList));
      }
    }
    return result;
  }

  public static void main(String[] args) {
    System.out.println(SubarrayProductLessThanK.findSubarrays(new int[] { 2, 5, 3, 10 }, 30));
    System.out.println(SubarrayProductLessThanK.findSubarrays(new int[] { 8, 2, 6, 5 }, 50));
  }
}
```

### Time complexity [#](https://www.educative.io/courses/grokking-the-coding-interview/RMV1GV1yPYz#time-complexity)

The main `for-loop` managing the sliding window takes O(N) but creating subarrays can take up to O(N^2)in the worst case. Therefore overall, our algorithm will take O(N^3).

### Space complexity [#](https://www.educative.io/courses/grokking-the-coding-interview/RMV1GV1yPYz#space-complexity)

Ignoring the space required for the output list, the algorithm runs in O(N) space which is used for the temp list.

Can you try estimating how much space will be required for the output list?

> The worst-case will happen when every subarray has a product less than the target!
>
> So the question will be, how many contiguous subarrays an array can have?
>
> It is definitely not all Permutations of the given array; is it all Combinations of the given array?

It is not all the Combinations of all elements of the array!

For an array with distinct elements, finding all of its contiguous subarrays is like finding the number of ways to choose two indices, `i` and `j`, in the array such that `i <= j`.

If there are a total of `n` elements in the array, here is how we can count all the contiguous subarrays:

- When `i = 0`, `j` can have any value from `0` to `n-1`, giving a total of `n` choices.
- When `i = 1`, `j` can have any value from `1` to `n-1`, giving a total of `n-1` choices.
- Similarly, when `i = 2`, `j` can have `n-2` choices.
  …
  …
- When `i = n-1`, `j` can only have only `1` choice.

Let’s combine all the choices:

```
    n + (n-1) + (n-2) + ... 3 + 2 + 1
```

Which gives us a total of: n*(n+1)/2

So, at most, we need space for O(n^2)output lists. At worst, each subarray can take O(n) space, so overall, our algorithm’s space complexity will be O(n^3).

# Dutch National Flag Problem (medium)

## Problem Statement [#](https://www.educative.io/courses/grokking-the-coding-interview/RMBxV6jz6Q0#problem-statement)

Given an array containing 0s, 1s and 2s, sort the array in-place. You should treat numbers of the array as objects, hence, we can’t count 0s, 1s, and 2s to recreate the array.

The flag of the Netherlands consists of three colors: red, white and blue; and since our input array also consists of three different numbers that is why it is called [Dutch National Flag problem](https://en.wikipedia.org/wiki/Dutch_national_flag_problem).

**Example 1:**

```
Input: [1, 0, 2, 1, 0]
Output: [0 0 1 1 2]
```

**Example 2:**

```
Input: [2, 2, 0, 1, 2, 0]
Output: [0 0 1 2 2 2 ]
```

## Solution [#](https://www.educative.io/courses/grokking-the-coding-interview/RMBxV6jz6Q0#solution)

The brute force solution will be to use an in-place sorting algorithm like [Heapsort](https://en.wikipedia.org/wiki/Heapsort) which will take O(N*logN). Can we do better than this? Is it possible to sort the array in one iteration?

We can use a **Two Pointers** approach while iterating through the array. Let’s say the two pointers are called `low` and `high` which are pointing to the first and the last element of the array respectively. So while iterating, we will move all 0s before `low` and all 2s after `high` so that in the end, all 1s will be between `low` and `high`.

### Code [#](https://www.educative.io/courses/grokking-the-coding-interview/RMBxV6jz6Q0#code)

Here is what our algorithm will look like:

```java
class DutchFlag {

  public static void sort(int[] arr) {
    // all elements < low are 0 and all elements > high are 2
    // all elements from >= low < i are 1
    int low = 0, high = arr.length - 1;
    for (int i = 0; i <= high;) {
      if (arr[i] == 0) {
        swap(arr, i, low);
        // increment 'i' and 'low'
        i++;
        low++;
      } else if (arr[i] == 1) {
        i++;
      } else { // the case for arr[i] == 2
        swap(arr, i, high);
        // decrement 'high' only, after the swap the number at index 'i' could be 0, 1 or 2
        high--;
      }
    }
  }

  private static void swap(int[] arr, int i, int j) {
    int temp = arr[i];
    arr[i] = arr[j];
    arr[j] = temp;
  }

  public static void main(String[] args) {
    int[] arr = new int[] { 1, 0, 2, 1, 0 };
    DutchFlag.sort(arr);
    for (int num : arr)
      System.out.print(num + " ");
    System.out.println();

    arr = new int[] { 2, 2, 0, 1, 2, 0 };
    DutchFlag.sort(arr);
    for (int num : arr)
      System.out.print(num + " ");
  }
}
```

### Time complexity [#](https://www.educative.io/courses/grokking-the-coding-interview/RMBxV6jz6Q0#time-complexity)

The time complexity of the above algorithm will be O(N) as we are iterating the input array only once.

### Space complexity [#](https://www.educative.io/courses/grokking-the-coding-interview/RMBxV6jz6Q0#space-complexity)

The algorithm runs in constant space O(1).

# Quadruple Sum to Target (medium)

Given an array of unsorted numbers and a target number, find all **unique quadruplets** in it, whose **sum is equal to the target number**.

**Example 1:**

```
Input: [4, 1, 2, -1, 1, -3], target=1
Output: [-3, -1, 1, 4], [-3, 1, 1, 2]
Explanation: Both the quadruplets add up to the target.
```

**Example 2:**

```
Input: [2, 0, -1, 1, -2, 2], target=2
Output: [-2, 0, 2, 2], [-1, 0, 1, 2]
Explanation: Both the quadruplets add up to the target.
```

## Solution [#](https://www.educative.io/courses/grokking-the-coding-interview/B6XOq8KlkWo#solution)

This problem follows the **Two Pointers** pattern and shares similarities with [Triplet Sum to Zero](https://www.educative.io/collection/page/5668639101419520/5671464854355968/5679549973004288/).

We can follow a similar approach to iterate through the array, taking one number at a time. At every step during the iteration, we will search for the quadruplets similar to [Triplet Sum to Zero](https://www.educative.io/collection/page/5668639101419520/5671464854355968/5679549973004288/) whose sum is equal to the given target.

## Code 

Here is what our algorithm will look like:

```java
import java.util.*;

class QuadrupleSumToTarget {

  public static List<List<Integer>> searchQuadruplets(int[] arr, int target) {
    Arrays.sort(arr);
    List<List<Integer>> quadruplets = new ArrayList<>();
    for (int i = 0; i < arr.length - 3; i++) {
      if (i > 0 && arr[i] == arr[i - 1]) // skip same element to avoid duplicate quadruplets
        continue;
      for (int j = i + 1; j < arr.length - 2; j++) {
        if (j > i + 1 && arr[j] == arr[j - 1]) // skip same element to avoid duplicate quadruplets
          continue;
        searchPairs(arr, target, i, j, quadruplets);
      }
    }
    return quadruplets;
  }

  private static void searchPairs(int[] arr, int targetSum, int first, int second, List<List<Integer>> quadruplets) {
    int left = second + 1;
    int right = arr.length - 1;
    while (left < right) {
      int sum = arr[first] + arr[second] + arr[left] + arr[right];
      if (sum == targetSum) { // found the quadruplet
        quadruplets.add(Arrays.asList(arr[first], arr[second], arr[left], arr[right]));
        left++;
        right--;
        while (left < right && arr[left] == arr[left - 1])
          left++; // skip same element to avoid duplicate quadruplets
        while (left < right && arr[right] == arr[right + 1])
          right--; // skip same element to avoid duplicate quadruplets
      } else if (sum < targetSum)
        left++; // we need a pair with a bigger sum
      else
        right--; // we need a pair with a smaller sum
    }
  }

  public static void main(String[] args) {
    System.out.println(QuadrupleSumToTarget.searchQuadruplets(new int[] { 4, 1, 2, -1, 1, -3 }, 1));
    System.out.println(QuadrupleSumToTarget.searchQuadruplets(new int[] { 2, 0, -1, 1, -2, 2 }, 2));
  }
}
```

### Time complexity [#](https://www.educative.io/courses/grokking-the-coding-interview/B6XOq8KlkWo#time-complexity)

Sorting the array will take O(N*logN)*. Overall `searchQuadruplets()` will take O(N * logN + N^3), which is asymptotically equivalent to O(N^3)*O*(*N*3).

### Space complexity [#](https://www.educative.io/courses/grokking-the-coding-interview/B6XOq8KlkWo#space-complexity)

The space complexity of the above algorithm will be O(N)which is required for sorting.

# Comparing Strings containing Backspaces (medium)

Given two strings containing backspaces (identified by the character ‘#’), check if the two strings are equal.

**Example 1:**

```
Input: str1="xy#z", str2="xzz#"
Output: true
Explanation: After applying backspaces the strings become "xz" and "xz" respectively.
```

**Example 2:**

```
Input: str1="xy#z", str2="xyz#"
Output: false
Explanation: After applying backspaces the strings become "xz" and "xy" respectively.
```

**Example 3:**

```
Input: str1="xp#", str2="xyz##"
Output: true
Explanation: After applying backspaces the strings become "x" and "x" respectively.
In "xyz##", the first '#' removes the character 'z' and the second '#' removes the character 'y'.
```

**Example 4:**

```
Input: str1="xywrrmp", str2="xywrrmu#p"
Output: true
Explanation: After applying backspaces the strings become "xywrrmp" and "xywrrmp" respectively.
```

## Solution [#](https://www.educative.io/courses/grokking-the-coding-interview/xVKE8MJDlzq#solution)

To compare the given strings, first, we need to apply the backspaces. An efficient way to do this would be from the end of both the strings. We can have separate pointers, pointing to the last element of the given strings. We can start comparing the characters pointed out by both the pointers to see if the strings are equal. If, at any stage, the character pointed out by any of the pointers is a backspace (’#’), we will skip and apply the backspace until we have a valid character available for comparison.

## Code

Here is what our algorithm will look like:

```java

class BackspaceCompare {

  public static boolean compare(String str1, String str2) {
    // use two pointer approach to compare the strings
    int index1 = str1.length() - 1, index2 = str2.length() - 1;
    while (index1 >= 0 || index2 >= 0) {

      int i1 = getNextValidCharIndex(str1, index1);
      int i2 = getNextValidCharIndex(str2, index2);

      if (i1 < 0 && i2 < 0) // reached the end of both the strings
        return true;

      if (i1 < 0 || i2 < 0) // reached the end of one of the strings
        return false;

      if (str1.charAt(i1) != str2.charAt(i2)) // check if the characters are equal
        return false;

      index1 = i1 - 1;
      index2 = i2 - 1;
    }

    return true;
  }

  private static int getNextValidCharIndex(String str, int index) {
    int backspaceCount = 0;
    while (index >= 0) {
      if (str.charAt(index) == '#') // found a backspace character
        backspaceCount++;
      else if (backspaceCount > 0) // a non-backspace character
        backspaceCount--;
      else
        break;

      index--; // skip a backspace or a valid character
    }
    return index;
  }

  public static void main(String[] args) {
    System.out.println(BackspaceCompare.compare("xy#z", "xzz#"));
    System.out.println(BackspaceCompare.compare("xy#z", "xyz#"));
    System.out.println(BackspaceCompare.compare("xp#", "xyz##"));    
    System.out.println(BackspaceCompare.compare("xywrrmp", "xywrrmu#p"));
  }
}
```

### Time complexity [#](https://www.educative.io/courses/grokking-the-coding-interview/xVKE8MJDlzq#time-complexity)

The time complexity of the above algorithm will be O(M+N) where ‘M’ and ‘N’ are the lengths of the two input strings respectively.

### Space complexity [#](https://www.educative.io/courses/grokking-the-coding-interview/xVKE8MJDlzq#space-complexity)

The algorithm runs in constant space O(1).

# Minimum Window Sort (medium)

Given an array, find the length of the smallest subarray in it which when sorted will sort the whole array.

**Example 1:**

```
Input: [1, 2, 5, 3, 7, 10, 9, 12]
Output: 5
Explanation: We need to sort only the subarray [5, 3, 7, 10, 9] to make the whole array sorted
```

**Example 2:**

```
Input: [1, 3, 2, 0, -1, 7, 10]
Output: 5
Explanation: We need to sort only the subarray [1, 3, 2, 0, -1] to make the whole array sorted
```

**Example 3:**

```
Input: [1, 2, 3]
Output: 0
Explanation: The array is already sorted
```

**Example 4:**

```
Input: [3, 2, 1]
Output: 3
Explanation: The whole array needs to be sorted.
```

## Solution [#](https://www.educative.io/courses/grokking-the-coding-interview/gxL951y9xj3#solution)

As we know, once an array is sorted (in ascending order), the smallest number is at the beginning and the largest number is at the end of the array. So if we start from the beginning of the array to find the first element which is out of sorting order i.e., which is smaller than its previous element, and similarly from the end of array to find the first element which is bigger than its previous element, will sorting the subarray between these two numbers result in the whole array being sorted?

Let’s try to understand this with Example-2 mentioned above. In the following array, what are the first numbers out of sorting order from the beginning and the end of the array:

```
    [1, 3, 2, 0, -1, 7, 10]
```

1. Starting from the beginning of the array the first number out of the sorting order is ‘2’ as it is smaller than its previous element which is ‘3’.
2. Starting from the end of the array the first number out of the sorting order is ‘0’ as it is bigger than its previous element which is ‘-1’

As you can see, sorting the numbers between ‘3’ and ‘-1’ will not sort the whole array. To see this, the following will be our original array after the sorted subarray:

```
    [1, -1, 0, 2, 3, 7, 10]
```

The problem here is that the smallest number of our subarray is ‘-1’ which dictates that we need to include more numbers from the beginning of the array to make the whole array sorted. We will have a similar problem if the maximum of the subarray is bigger than some elements at the end of the array. To sort the whole array we need to include all such elements that are smaller than the biggest element of the subarray. So our final algorithm will look like:

1. From the beginning and end of the array, find the first elements that are out of the sorting order. The two elements will be our candidate subarray.
2. Find the maximum and minimum of this subarray.
3. Extend the subarray from beginning to include any number which is bigger than the minimum of the subarray.
4. Similarly, extend the subarray from the end to include any number which is smaller than the maximum of the subarray.

### Code [#](https://www.educative.io/courses/grokking-the-coding-interview/gxL951y9xj3#code)

Here is what our algorithm will look like:

```java
class ShortestWindowSort {

  public static int sort(int[] arr) {
    int low = 0, high = arr.length - 1;
    // find the first number out of sorting order from the beginning
    while (low < arr.length - 1 && arr[low] <= arr[low + 1])
      low++;

    if (low == arr.length - 1) // if the array is sorted
      return 0;

    // find the first number out of sorting order from the end
    while (high > 0 && arr[high] >= arr[high - 1])
      high--;

    // find the maximum and minimum of the subarray
    int subarrayMax = Integer.MIN_VALUE, subarrayMin = Integer.MAX_VALUE;
    for (int k = low; k <= high; k++) {
      subarrayMax = Math.max(subarrayMax, arr[k]);
      subarrayMin = Math.min(subarrayMin, arr[k]);
    }

    // extend the subarray to include any number which is bigger than the minimum of the subarray 
    while (low > 0 && arr[low - 1] > subarrayMin)
      low--;
    // extend the subarray to include any number which is smaller than the maximum of the subarray
    while (high < arr.length - 1 && arr[high + 1] < subarrayMax)
      high++;

    return high - low + 1;
  }

  public static void main(String[] args) {
    System.out.println(ShortestWindowSort.sort(new int[] { 1, 2, 5, 3, 7, 10, 9, 12 }));
    System.out.println(ShortestWindowSort.sort(new int[] { 1, 3, 2, 0, -1, 7, 10 }));
    System.out.println(ShortestWindowSort.sort(new int[] { 1, 2, 3 }));
    System.out.println(ShortestWindowSort.sort(new int[] { 3, 2, 1 }));
  }
}
```

### Time complexity 

The time complexity of the above algorithm will be O(N).

### Space complexity

The algorithm runs in constant space O(1).

