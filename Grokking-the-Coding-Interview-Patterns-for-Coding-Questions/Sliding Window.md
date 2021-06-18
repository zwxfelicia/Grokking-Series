# Introduction

In many problems dealing with an array (or a LinkedList), we are asked to find or calculate something among all the contiguous subarrays (or sublists) of a given size. For example, take a look at this problem:

> Given an array, find the average of all contiguous subarrays of size ‘K’ in it.

Let’s understand this problem with a real input:

```
Array: [1, 3, 2, 6, -1, 4, 1, 8, 2], K=5
```

Here, we are asked to find the average of all contiguous subarrays of size ‘5’ in the given array. Let’s solve this:

1. For the first 5 numbers (subarray from index 0-4), the average is: (1+3+2+6-1)/5 => 2.2(1+3+2+6−1)/5=>2.2
2. The average of next 5 numbers (subarray from index 1-5) is: (3+2+6-1+4)/5 => 2.8(3+2+6−1+4)/5=>2.8
3. For the next 5 numbers (subarray from index 2-6), the average is: (2+6-1+4+1)/5 => 2.4(2+6−1+4+1)/5=>2.4
   …

Here is the final output containing the averages of all contiguous subarrays of size 5:

```
Output: [2.2, 2.8, 2.4, 3.6, 2.8]
```

A brute-force algorithm will calculate the sum of every 5-element contiguous subarray of the given array and divide the sum by ‘5’ to find the average. This is what the algorithm will look like:

```java
import java.util.Arrays;

class AverageOfSubarrayOfSizeK {
  public static double[] findAverages(int K, int[] arr) {
    double[] result = new double[arr.length - K + 1];
    for (int i = 0; i <= arr.length - K; i++) {
      // find sum of next 'K' elements
      double sum = 0;
      for (int j = i; j < i + K; j++)
        sum += arr[j];
      result[i] = sum / K; // calculate average
    }

    return result;
  }

  public static void main(String[] args) {
    double[] result = AverageOfSubarrayOfSizeK.findAverages(5, new int[] { 1, 3, 2, 6, -1, 4, 1, 8, 2 });
    System.out.println("Averages of subarrays of size K: " + Arrays.toString(result));
  }
}
```

**Time complexity:** Since for every element of the input array, we are calculating the sum of its next ‘K’ elements, the time complexity of the above algorithm will be O(N*K) where ‘N’ is the number of elements in the input array.

Can we find a better solution? Do you see any inefficiency in the above approach?

The inefficiency is that for any two consecutive subarrays of size ‘5’, the overlapping part (which will contain four elements) will be evaluated twice. For example, take the above-mentioned input:

![image-20210617142710930](/Users/zhengwenxuan/Desktop/Coding Pattern/image-20210617142710930.png)

As you can see, there are four overlapping elements between the subarray (indexed from 0-4) and the subarray (indexed from 1-5). Can we somehow reuse the `sum` we have calculated for the overlapping elements?

The efficient way to solve this problem would be to visualize each contiguous subarray as a sliding window of ‘5’ elements. This means that we will slide the window by one element when we move on to the next subarray. To reuse the `sum` from the previous subarray, we will subtract the element going out of the window and add the element now being included in the sliding window. This will save us from going through the whole subarray to find the `sum` and, as a result, the algorithm complexity will reduce to O(N).

![image-20210617142745358](/Users/zhengwenxuan/Desktop/Coding Pattern/image-20210617142745358.png)

Here is the algorithm for the **Sliding Window** approach:

```java
import java.util.Arrays;

class AverageOfSubarrayOfSizeK {
  public static double[] findAverages(int K, int[] arr) {
    double[] result = new double[arr.length - K + 1];
    double windowSum = 0;
    int windowStart = 0;
    for (int windowEnd = 0; windowEnd < arr.length; windowEnd++) {
      windowSum += arr[windowEnd]; // add the next element
      // slide the window, we don't need to slide if we've not hit the required window size of 'k'
      if (windowEnd >= K - 1) {
        result[windowStart] = windowSum / K; // calculate the average
        windowSum -= arr[windowStart]; // subtract the element going out
        windowStart++; // slide the window ahead
      }
    }

    return result;
  }

  public static void main(String[] args) {
    double[] result = AverageOfSubarrayOfSizeK.findAverages(5, new int[] { 1, 3, 2, 6, -1, 4, 1, 8, 2 });
    System.out.println("Averages of subarrays of size K: " + Arrays.toString(result));
  }
}
```

In the following chapters, we will apply the **Sliding Window** approach to solve a few problems.

In some problems, the size of the sliding window is not fixed. We have to expand or shrink the window based on the problem constraints. We will see a few examples of such problems in the next chapters.

Let’s jump onto our first problem and apply the **Sliding Window** pattern.

# Maximum Sum Subarray of Size K (easy)

## Problem Statement

Given an array of positive numbers and a positive number ‘k,’ find the **maximum sum of any contiguous subarray of size ‘k’**.

**Example 1:**

```
Input: [2, 1, 5, 1, 3, 2], k=3 
Output: 9
Explanation: Subarray with maximum sum is [5, 1, 3].
```

**Example 2:**

```
Input: [2, 3, 4, 1, 5], k=2 
Output: 7
Explanation: Subarray with maximum sum is [3, 4].
```

## Solution

A basic brute force solution will be to calculate the sum of all ‘k’ sized subarrays of the given array to find the subarray with the highest sum. We can start from every index of the given array and add the next ‘k’ elements to find the subarray’s sum. Following is the visual representation of this algorithm for Example-1:

![image-20210617143205875](/Users/zhengwenxuan/Desktop/Coding Pattern/image-20210617143205875.png)

## Code

Here is what our algorithm will look like:

```java
class MaxSumSubArrayOfSizeK {
  public static int findMaxSumSubArray(int k, int[] arr) {
    int maxSum = 0, windowSum;
    for (int i = 0; i <= arr.length - k; i++) {
      windowSum = 0;
      for (int j = i; j < i + k; j++) {
        windowSum += arr[j];
      }
      maxSum = Math.max(maxSum, windowSum);
    }

    return maxSum;
  }
  
  public static void main(String[] args) {
    System.out.println("Maximum sum of a subarray of size K: "
        + MaxSumSubArrayOfSizeK.findMaxSumSubArray(3, new int[] { 2, 1, 5, 1, 3, 2 }));
    System.out.println("Maximum sum of a subarray of size K: "
        + MaxSumSubArrayOfSizeK.findMaxSumSubArray(2, new int[] { 2, 3, 4, 1, 5 }));
  }
}
```

The above algorithm’s time complexity will be O(N*K)*O*(*N*∗*K*), where ‘N’ is the total number of elements in the given array. Is it possible to find a better algorithm than this?

## A better approach 

If you observe closely, you will realize that to calculate the sum of a contiguous subarray, we can utilize the sum of the previous subarray. For this, consider each subarray as a **Sliding Window** of size ‘k.’ To calculate the sum of the next subarray, we need to slide the window ahead by one element. So to slide the window forward and calculate the sum of the new position of the sliding window, we need to do two things:

1. Subtract the element going out of the sliding window, i.e., subtract the first element of the window.
2. Add the new element getting included in the sliding window, i.e., the element coming right after the end of the window.

This approach will save us from re-calculating the sum of the overlapping part of the sliding window. Here is what our algorithm will look like:

```java
class MaxSumSubArrayOfSizeK {
  public static int findMaxSumSubArray(int k, int[] arr) {
    int windowSum = 0, maxSum = 0;
    int windowStart = 0;
    for (int windowEnd = 0; windowEnd < arr.length; windowEnd++) {
      windowSum += arr[windowEnd]; // add the next element
      // slide the window, we don't need to slide if we've not hit the required window size of 'k'
      if (windowEnd >= k - 1) {
        maxSum = Math.max(maxSum, windowSum);
        windowSum -= arr[windowStart]; // subtract the element going out
        windowStart++; // slide the window ahead
      }
    }

    return maxSum;
  }

  public static void main(String[] args) {
    System.out.println("Maximum sum of a subarray of size K: "
        + MaxSumSubArrayOfSizeK.findMaxSumSubArray(3, new int[] { 2, 1, 5, 1, 3, 2 }));
    System.out.println("Maximum sum of a subarray of size K: "
        + MaxSumSubArrayOfSizeK.findMaxSumSubArray(2, new int[] { 2, 3, 4, 1, 5 }));
  }
}
```

## Time Complexity

The time complexity of the above algorithm will be O(N).

## Space Complexity

The algorithm runs in constant space O(1).

# Smallest Subarray with a given sum (easy)

## Problem Statement 

Given an array of positive numbers and a positive number ‘S,’ find the length of the **smallest contiguous subarray whose sum is greater than or equal to ‘S’**. Return 0 if no such subarray exists.

**Example 1:**

```
Input: [2, 1, 5, 2, 3, 2], S=7 
Output: 2
Explanation: The smallest subarray with a sum greater than or equal to '7' is [5, 2].
```

**Example 2:**

```
Input: [2, 1, 5, 2, 8], S=7 
Output: 1
Explanation: The smallest subarray with a sum greater than or equal to '7' is [8].
```

**Example 3:**

```
Input: [3, 4, 1, 1, 6], S=8 
Output: 3
Explanation: Smallest subarrays with a sum greater than or equal to '8' are [3, 4, 1] 
or [1, 1, 6].
```

## Solution 

This problem follows the **Sliding Window** pattern, and we can use a similar strategy as discussed in [Maximum Sum Subarray of Size K](https://www.educative.io/collection/page/5668639101419520/5671464854355968/5177043027230720/). There is one difference though: in this problem, the sliding window size is not fixed. Here is how we will solve this problem:

1. First, we will add-up elements from the beginning of the array until their sum becomes greater than or equal to ‘S.’
2. These elements will constitute our sliding window. We are asked to find the smallest such window having a sum greater than or equal to ‘S.’ We will remember the length of this window as the smallest window so far.
3. After this, we will keep adding one element in the sliding window (i.e., slide the window ahead) in a stepwise fashion.
4. In each step, we will also try to shrink the window from the beginning. We will shrink the window until the window’s sum is smaller than ‘S’ again. This is needed as we intend to find the smallest window. This shrinking will also happen in multiple steps; in each step, we will do two things:
   - Check if the current window length is the smallest so far, and if so, remember its length.
   - Subtract the first element of the window from the running sum to shrink the sliding window.

Here is the visual representation of this algorithm for the Example-1:

![image-20210617164754672](/Users/zhengwenxuan/Desktop/Coding Pattern/image-20210617164754672.png)

## Code 

Here is what our algorithm will look like:

```java
class MinSizeSubArraySum {
  public static int findMinSubArray(int S, int[] arr) {
    int windowSum = 0, minLength = Integer.MAX_VALUE;
    int windowStart = 0;
    for (int windowEnd = 0; windowEnd < arr.length; windowEnd++) {
      windowSum += arr[windowEnd]; // add the next element
      // shrink the window as small as possible until the 'windowSum' is smaller than 'S'
      while (windowSum >= S) {
        minLength = Math.min(minLength, windowEnd - windowStart + 1);
        windowSum -= arr[windowStart]; // subtract the element going out
        windowStart++; // slide the window ahead
      }
    }

    return minLength == Integer.MAX_VALUE ? 0 : minLength;
  }

  public static void main(String[] args) {
    int result = MinSizeSubArraySum.findMinSubArray(7, new int[] { 2, 1, 5, 2, 3, 2 });
    System.out.println("Smallest subarray length: " + result);
    result = MinSizeSubArraySum.findMinSubArray(7, new int[] { 2, 1, 5, 2, 8 });
    System.out.println("Smallest subarray length: " + result);
    result = MinSizeSubArraySum.findMinSubArray(8, new int[] { 3, 4, 1, 1, 6 });
    System.out.println("Smallest subarray length: " + result);
  }
}

```

## Time Complexity

The time complexity of the above algorithm will be O(N). The outer `for` loop runs for all elements, and the inner `while` loop processes each element only once; therefore, the time complexity of the algorithm will be O(N+N)*O*(*N*+*N*), which is asymptotically equivalent to O(N).

## Space Complexity 

The algorithm runs in constant space O(1).

# Longest Substring with K Distinct Characters (medium)

## Problem Statement

Given a string, find the length of the **longest substring** in it **with no more than `K` distinct characters**.

You can assume that `K` is less than or equal to the length of the given string.

**Example 1:**

```
Input: String="araaci", K=2
Output: 4
Explanation: The longest substring with no more than '2' distinct characters is "araa".
```

**Example 2:**

```
Input: String="araaci", K=1
Output: 2
Explanation: The longest substring with no more than '1' distinct characters is "aa".
```

**Example 3:**

```
Input: String="cbbebi", K=3
Output: 5
Explanation: The longest substrings with no more than '3' distinct characters are "cbbeb" & "bbebi".
```

## Solution 

This problem follows the **Sliding Window** pattern, and we can use a similar dynamic sliding window strategy as discussed in [Smallest Subarray with a given sum](https://www.educative.io/collection/page/5668639101419520/5671464854355968/5177043027230720/). We can use a **HashMap** to remember the frequency of each character we have processed. Here is how we will solve this problem:

1. First, we will insert characters from the beginning of the string until we have `K` distinct characters in the **HashMap**.
2. These characters will constitute our sliding window. We are asked to find the longest such window having no more than `K` distinct characters. We will remember the length of this window as the longest window so far.
3. After this, we will keep adding one character in the sliding window (i.e., slide the window ahead) in a stepwise fashion.
4. In each step, we will try to shrink the window from the beginning if the count of distinct characters in the **HashMap** is larger than `K`. We will shrink the window until we have no more than `K` distinct characters in the **HashMap**. This is needed as we intend to find the longest window.
5. While shrinking, we’ll decrement the character’s frequency going out of the window and remove it from the **HashMap** if its frequency becomes zero.
6. At the end of each step, we’ll check if the current window length is the longest so far, and if so, remember its length.

Here is the visual representation of this algorithm for the Example-1:

![image-20210617173802147](/Users/zhengwenxuan/Desktop/Coding Pattern/image-20210617173802147.png)

## Code

Here is how our algorithm will look like:

```java
import java.util.*;

class LongestSubstringKDistinct {
  public static int findLength(String str, int k) {
    if (str == null || str.length() == 0 || str.length() < k)
      throw new IllegalArgumentException();

    int windowStart = 0, maxLength = 0;
    Map<Character, Integer> charFrequencyMap = new HashMap<>();
    // in the following loop we'll try to extend the range [windowStart, windowEnd]
    for (int windowEnd = 0; windowEnd < str.length(); windowEnd++) {
      char rightChar = str.charAt(windowEnd);
      charFrequencyMap.put(rightChar, charFrequencyMap.getOrDefault(rightChar, 0) + 1);
      // shrink the sliding window, until we are left with 'k' distinct characters in the frequency map
      while (charFrequencyMap.size() > k) {
        char leftChar = str.charAt(windowStart);
        charFrequencyMap.put(leftChar, charFrequencyMap.get(leftChar) - 1);
        if (charFrequencyMap.get(leftChar) == 0) {
          charFrequencyMap.remove(leftChar);
        }
        windowStart++; // shrink the window
      }
      maxLength = Math.max(maxLength, windowEnd - windowStart + 1); // remember the maximum length so far
    }

    return maxLength;
  }

  public static void main(String[] args) {
    System.out.println("Length of the longest substring: " + LongestSubstringKDistinct.findLength("araaci", 2));
    System.out.println("Length of the longest substring: " + LongestSubstringKDistinct.findLength("araaci", 1));
    System.out.println("Length of the longest substring: " + LongestSubstringKDistinct.findLength("cbbebi", 3));
  }
}

```

## Time Complexity 

The above algorithm’s time complexity will be O(N), where N*N* is the number of characters in the input string. The outer `for` loop runs for all characters, and the inner `while` loop processes each character only once; therefore, the time complexity of the algorithm will be O(N+N)*O*(*N*+*N*), which is asymptotically equivalent to O(N).

## Space Complexity

The algorithm’s space complexity is O(K), as we will be storing a maximum of K+1characters in the HashMap.

# Fruits into Baskets (medium)

## Problem Statement

Given an array of characters where each character represents a fruit tree, you are given **two baskets**, and your goal is to put **maximum number of fruits in each basket**. The only restriction is that **each basket can have only one type of fruit**.

You can start with any tree, but you can’t skip a tree once you have started. You will pick one fruit from each tree until you cannot, i.e., you will stop when you have to pick from a third fruit type.

Write a function to return the maximum number of fruits in both baskets.

**Example 1:**

```
Input: Fruit=['A', 'B', 'C', 'A', 'C']
Output: 3
Explanation: We can put 2 'C' in one basket and one 'A' in the other from the subarray ['C', 'A', 'C']
```

**Example 2:**

```
Input: Fruit=['A', 'B', 'C', 'B', 'B', 'C']
Output: 5
Explanation: We can put 3 'B' in one basket and two 'C' in the other basket. 
This can be done if we start with the second letter: ['B', 'C', 'B', 'B', 'C']
```

## Solution 

This problem follows the **Sliding Window** pattern and is quite similar to [Longest Substring with K Distinct Characters](https://www.educative.io/collection/page/5668639101419520/5671464854355968/5698217712812032/). In this problem, we need to find the length of the longest subarray with no more than two distinct characters (or fruit types!). This transforms the current problem into **Longest Substring with K Distinct Characters** where K=2.

## Code 

Here is what our algorithm will look like, only the highlighted lines are different from [Longest Substring with K Distinct Characters](https://www.educative.io/collection/page/5668639101419520/5671464854355968/5698217712812032/):

```java
import java.util.*;

class MaxFruitCountOf2Types {
  public static int findLength(char[] arr) {
    int windowStart = 0, maxLength = 0;
    Map<Character, Integer> fruitFrequencyMap = new HashMap<>();
    // try to extend the range [windowStart, windowEnd]
    for (int windowEnd = 0; windowEnd < arr.length; windowEnd++) {
      fruitFrequencyMap.put(arr[windowEnd], fruitFrequencyMap.getOrDefault(arr[windowEnd], 0) + 1);
      // shrink the sliding window, until we are left with '2' fruits in the frequency map
      while (fruitFrequencyMap.size() > 2) {
        fruitFrequencyMap.put(arr[windowStart], fruitFrequencyMap.get(arr[windowStart]) - 1);
        if (fruitFrequencyMap.get(arr[windowStart]) == 0) {
          fruitFrequencyMap.remove(arr[windowStart]);
        }
        windowStart++; // shrink the window
      }
      maxLength = Math.max(maxLength, windowEnd - windowStart + 1);
    }

    return maxLength;
  }

  public static void main(String[] args) {
    System.out.println("Maximum number of fruits: " + 
                          MaxFruitCountOf2Types.findLength(new char[] { 'A', 'B', 'C', 'A', 'C' }));
    System.out.println("Maximum number of fruits: " + 
                          MaxFruitCountOf2Types.findLength(new char[] { 'A', 'B', 'C', 'B', 'B', 'C' }));
  }
}

```

## Time Complexity

The above algorithm’s time complexity will be O(N), where ‘N’ is the number of characters in the input array. The outer `for` loop runs for all characters, and the inner `while` loop processes each character only once; therefore, the time complexity of the algorithm will be O(N+N), which is asymptotically equivalent to O(N).

## Space Complexity 

The algorithm runs in constant space O(1) as there can be a maximum of three types of fruits stored in the frequency map.

## Similar Problems 

**Problem 1: Longest Substring with at most 2 distinct characters**

Given a string, find the length of the longest substring in it with at most two distinct characters.

**Solution:** This problem is exactly similar to our parent problem.

# No-repeat Substring (hard)

## Problem Statement

Given a string, find the **length of the longest substring**, which has **no repeating characters**.

**Example 1:**

```
Input: String="aabccbb"
Output: 3
Explanation: The longest substring without any repeating characters is "abc".
```

**Example 2:**

```
Input: String="abbbb"
Output: 2
Explanation: The longest substring without any repeating characters is "ab".
```

**Example 3:**

```
Input: String="abccde"
Output: 3
Explanation: Longest substrings without any repeating characters are "abc" & "cde".
```

## Solution

This problem follows the **Sliding Window** pattern, and we can use a similar dynamic sliding window strategy as discussed in [Longest Substring with K Distinct Characters](https://www.educative.io/collection/page/5668639101419520/5671464854355968/5698217712812032/). We can use a **HashMap** to remember the last index of each character we have processed. Whenever we get a repeating character, we will shrink our sliding window to ensure that we always have distinct characters in the sliding window.

```java
import java.util.*;

class NoRepeatSubstring {
  public static int findLength(String str) {
    int windowStart = 0, maxLength = 0;
    Map<Character, Integer> charIndexMap = new HashMap<>();
    // try to extend the range [windowStart, windowEnd]
    for (int windowEnd = 0; windowEnd < str.length(); windowEnd++) {
      char rightChar = str.charAt(windowEnd);
      // if the map already contains the 'rightChar', shrink the window from the beginning so that
      // we have only one occurrence of 'rightChar'
      if (charIndexMap.containsKey(rightChar)) {
        // this is tricky; in the current window, we will not have any 'rightChar' after its previous index
        // and if 'windowStart' is already ahead of the last index of 'rightChar', we'll keep 'windowStart'
        windowStart = Math.max(windowStart, charIndexMap.get(rightChar) + 1);
      }
      charIndexMap.put(rightChar, windowEnd); // insert the 'rightChar' into the map
      maxLength = Math.max(maxLength, windowEnd - windowStart + 1); // remember the maximum length so far
    }

    return maxLength;
  }

  public static void main(String[] args) {
    System.out.println("Length of the longest substring: " + NoRepeatSubstring.findLength("aabccbb"));
    System.out.println("Length of the longest substring: " + NoRepeatSubstring.findLength("abbbb"));
    System.out.println("Length of the longest substring: " + NoRepeatSubstring.findLength("abccde"));
  }
}
```

## Time Complexity

The above algorithm’s time complexity will be O(N), where ‘N’ is the number of characters in the input string.

## Space Complexity 

The algorithm’s space complexity will be O(K), where K is the number of distinct characters in the input string. This also means K<=N,  because in the worst case, the whole string might not have any repeating character, so the entire string will be added to the **HashMap**. Having said that, since we can expect a fixed set of characters in the input string (e.g., 26 for English letters), we can say that the algorithm runs in fixed space O(1); in this case, we can use a fixed-size array instead of the **HashMap**.

# Longest Substring with Same Letters after Replacement (hard)

## Problem Statement 

Given a string with lowercase letters only, if you are allowed to **replace no more than ‘k’ letters** with any letter, find the **length of the longest substring having the same letters** after replacement.

**Example 1:**

```
Input: String="aabccbb", k=2
Output: 5
Explanation: Replace the two 'c' with 'b' to have a longest repeating substring "bbbbb".
```

**Example 2:**

```
Input: String="abbcb", k=1
Output: 4
Explanation: Replace the 'c' with 'b' to have a longest repeating substring "bbbb".
```

**Example 3:**

```
Input: String="abccde", k=1
Output: 3
Explanation: Replace the 'b' or 'd' with 'c' to have the longest repeating substring "ccc".
```

## Solution 

This problem follows the **Sliding Window** pattern, and we can use a similar dynamic sliding window strategy as discussed in [No-repeat Substring](https://www.educative.io/collection/page/5668639101419520/5671464854355968/5485010335301632/). We can use a HashMap to count the frequency of each letter.

- We will iterate through the string to add one letter at a time in the window.
- We will also keep track of the count of the maximum repeating letter in **any** window (let’s call it `maxRepeatLetterCount`).
- So, at any time, we know that we do have a window with one letter repeating **maxRepeatLetterCount** times; this means we should try to replace the remaining letters.
  - If the remaining letters are less than or equal to ‘k’, we can replace them all.
  - If we have more than ‘k’ remaining letters, we should shrink the window as we cannot replace more than ‘k’ letters.

While shrinking the window, we don’t need to update `maxRepeatLetterCount` (hence, it represents the maximum repeating count of ANY letter for ANY window). Why don’t we need to update this count when we shrink the window? Since we have to replace all the remaining letters to get the longest substring having the same letter in any window, we can’t get a better answer from any other window even though all occurrences of the letter with frequency `maxRepeatLetterCount` is not in the current window.

## Code

Here is what our algorithm will look like:

```java
import java.util.*;

class CharacterReplacement {
  public static int findLength(String str, int k) {
    int windowStart = 0, maxLength = 0, maxRepeatLetterCount = 0;
    Map<Character, Integer> letterFrequencyMap = new HashMap<>();
    // try to extend the range [windowStart, windowEnd]
    for (int windowEnd = 0; windowEnd < str.length(); windowEnd++) {
      char rightChar = str.charAt(windowEnd);
      letterFrequencyMap.put(rightChar, letterFrequencyMap.getOrDefault(rightChar, 0) + 1);
      maxRepeatLetterCount = Math.max(maxRepeatLetterCount, letterFrequencyMap.get(rightChar));

      // current window size is from windowStart to windowEnd, overall we have a letter which is
      // repeating 'maxRepeatLetterCount' times, this means we can have a window which has one letter 
      // repeating 'maxRepeatLetterCount' times and the remaining letters we should replace.
      // if the remaining letters are more than 'k', it is the time to shrink the window as we
      // are not allowed to replace more than 'k' letters
      if (windowEnd - windowStart + 1 - maxRepeatLetterCount > k) {
        char leftChar = str.charAt(windowStart);
        letterFrequencyMap.put(leftChar, letterFrequencyMap.get(leftChar) - 1);
        windowStart++;
      }

      maxLength = Math.max(maxLength, windowEnd - windowStart + 1);
    }

    return maxLength;
  }

  public static void main(String[] args) {
    System.out.println(CharacterReplacement.findLength("aabccbb", 2));
    System.out.println(CharacterReplacement.findLength("abbcb", 1));
    System.out.println(CharacterReplacement.findLength("abccde", 1));
  }
}j
```

## Time Complexity [#](https://www.educative.io/courses/grokking-the-coding-interview/R8DVgjq78yR#time-complexity)

The above algorithm’s time complexity will be O(N),  where ‘N’ is the number of letters in the input string.

## Space Complexity

As we expect only the lower case letters in the input string, we can conclude that the space complexity will be O(26) to store each letter’s frequency in the **HashMap**, which is asymptotically equal to O(1).

# Longest Subarray with Ones after Replacement (hard)

## Problem Statement [#](https://www.educative.io/courses/grokking-the-coding-interview/B6VypRxPolJ#problem-statement)

Given an array containing 0s and 1s, if you are allowed to **replace no more than ‘k’ 0s with 1s**, find the length of the **longest contiguous subarray having all 1s**.

**Example 1:**

```
Input: Array=[0, 1, 1, 0, 0, 0, 1, 1, 0, 1, 1], k=2
Output: 6
Explanation: Replace the '0' at index 5 and 8 to have the longest contiguous subarray of 1s having length 6.
```

**Example 2:**

```
Input: Array=[0, 1, 0, 0, 1, 1, 0, 1, 1, 0, 0, 1, 1], k=3
Output: 9
Explanation: Replace the '0' at index 6, 9, and 10 to have the longest contiguous subarray of 1s having length 9.
```

## Solution [#](https://www.educative.io/courses/grokking-the-coding-interview/B6VypRxPolJ#solution)

This problem follows the **Sliding Window** pattern and is quite similar to [Longest Substring with same Letters after Replacement](https://www.educative.io/collection/page/5668639101419520/5671464854355968/6497958910492672/). The only difference is that, in the problem, we only have two characters (1s and 0s) in the input arrays.

Following a similar approach, we’ll iterate through the array to add one number at a time in the window. We’ll also keep track of the maximum number of repeating 1s in the current window (let’s call it `maxOnesCount`). So at any time, we know that we can have a window with 1s repeating `maxOnesCount` time, so we should try to replace the remaining 0s. If we have more than ‘k’ remaining 0s, we should shrink the window as we are not allowed to replace more than ‘k’ 0s.

## Code [#](https://www.educative.io/courses/grokking-the-coding-interview/B6VypRxPolJ#code)

Here is how our algorithm will look like:

```java
class ReplacingOnes {
  public static int findLength(int[] arr, int k) {
    int windowStart = 0, maxLength = 0, maxOnesCount = 0;
    // try to extend the range [windowStart, windowEnd]
    for (int windowEnd = 0; windowEnd < arr.length; windowEnd++) {
      if (arr[windowEnd] == 1)
        maxOnesCount++;

      // current window size is from windowStart to windowEnd, overall we have a maximum of 1s
      // repeating a maximum of 'maxOnesCount' times, this means that we can have a window with
      // 'maxOnesCount' 1s and the remaining are 0s which should replace with 1s.
      // now, if the remaining 0s are more than 'k', it is the time to shrink the window as we
      // are not allowed to replace more than 'k' Os
      if (windowEnd - windowStart + 1 - maxOnesCount > k) {
        if (arr[windowStart] == 1)
          maxOnesCount--;
        windowStart++;
      }

      maxLength = Math.max(maxLength, windowEnd - windowStart + 1);
    }

    return maxLength;
  }

  public static void main(String[] args) {
    System.out.println(ReplacingOnes.findLength(new int[] { 0, 1, 1, 0, 0, 0, 1, 1, 0, 1, 1 }, 2));
    System.out.println(ReplacingOnes.findLength(new int[] { 0, 1, 0, 0, 1, 1, 0, 1, 1, 0, 0, 1, 1 }, 3));
  }
}

```

 ## Time Complexity 

The above algorithm’s time complexity will be O(N), where ‘N’ is the count of numbers in the input array.

## Space Complexity 

The algorithm runs in constant space O(1).

# Permutation in a String (hard) 

Given a string and a pattern, find out if the **string contains any permutation of the pattern**.

**Permutation** is defined as the re-arranging of the characters of the string. For example, “abc” has the following six permutations:

1. abc
2. acb
3. bac
4. bca
5. cab
6. cba

If a string has ‘n’ distinct characters, it will have n!*n*! permutations.

**Example 1:**

```
Input: String="oidbcaf", Pattern="abc"
Output: true
Explanation: The string contains "bca" which is a permutation of the given pattern.
```

**Example 2:**

```
Input: String="odicf", Pattern="dc"
Output: false
Explanation: No permutation of the pattern is present in the given string as a substring.
```

**Example 3:**

```
Input: String="bcdxabcdy", Pattern="bcdyabcdx"
Output: true
Explanation: Both the string and the pattern are a permutation of each other.
```

**Example 4:**

```
Input: String="aaacb", Pattern="abc"
Output: true
Explanation: The string contains "acb" which is a permutation of the given pattern.
```

## Solution

This problem follows the **Sliding Window** pattern, and we can use a similar sliding window strategy as discussed in [Longest Substring with K Distinct Characters](https://www.educative.io/collection/page/5668639101419520/5671464854355968/5698217712812032/). We can use a **HashMap** to remember the frequencies of all characters in the given pattern. Our goal will be to match all the characters from this **HashMap** with a sliding window in the given string. Here are the steps of our algorithm:

1. Create a **HashMap** to calculate the frequencies of all characters in the pattern.
2. Iterate through the string, adding one character at a time in the sliding window.
3. If the character being added matches a character in the **HashMap**, decrement its frequency in the map. If the character frequency becomes zero, we got a complete match.
4. If at any time, the number of characters matched is equal to the number of distinct characters in the pattern (i.e., total characters in the **HashMap**), we have gotten our required permutation.
5. If the window size is greater than the length of the pattern, shrink the window to make it equal to the pattern’s size. At the same time, if the character going out was part of the pattern, put it back in the frequency **HashMap**.

## Code

Here is what our algorithm will look like:

```java
import java.util.*;

class StringPermutation {
  public static boolean findPermutation(String str, String pattern) {
    int windowStart = 0, matched = 0;
    Map<Character, Integer> charFrequencyMap = new HashMap<>();
    for (char chr : pattern.toCharArray())
      charFrequencyMap.put(chr, charFrequencyMap.getOrDefault(chr, 0) + 1);

    // our goal is to match all the characters from the 'charFrequencyMap' with the current window
    // try to extend the range [windowStart, windowEnd]
    for (int windowEnd = 0; windowEnd < str.length(); windowEnd++) {
      char rightChar = str.charAt(windowEnd);
      if (charFrequencyMap.containsKey(rightChar)) {
        // decrement the frequency of the matched character
        charFrequencyMap.put(rightChar, charFrequencyMap.get(rightChar) - 1);
        if (charFrequencyMap.get(rightChar) == 0) // character is completely matched
          matched++;
      }

      if (matched == charFrequencyMap.size())
        return true;

      if (windowEnd >= pattern.length() - 1) { // shrink the window by one character
        char leftChar = str.charAt(windowStart++);
        if (charFrequencyMap.containsKey(leftChar)) {
          if (charFrequencyMap.get(leftChar) == 0)
            matched--; // before putting the character back, decrement the matched count
          // put the character back for matching
          charFrequencyMap.put(leftChar, charFrequencyMap.get(leftChar) + 1);
        }
      }
    }

    return false;
  }

  public static void main(String[] args) {
    System.out.println("Permutation exist: " + StringPermutation.findPermutation("oidbcaf", "abc"));
    System.out.println("Permutation exist: " + StringPermutation.findPermutation("odicf", "dc"));
    System.out.println("Permutation exist: " + StringPermutation.findPermutation("bcdxabcdy", "bcdyabcdx"));
    System.out.println("Permutation exist: " + StringPermutation.findPermutation("aaacb", "abc"));
  }
}

```

## Time Complexity 

The above algorithm’s time complexity will be O(N + M), where ‘N’ and ‘M’ are the number of characters in the input string and the pattern, respectively.

## Space Complexity

The algorithm’s space complexity is O(M) since, in the worst case, the whole pattern can have distinct characters that will go into the **HashMap**.

# String Anagrams (hard) 

Given a string and a pattern, find **all anagrams of the pattern in the given string**.

Every **anagram** is a **permutation** of a string. As we know, when we are not allowed to repeat characters while finding permutations of a string, we get N!*N*! permutations (or anagrams) of a string having N*N* characters. For example, here are the six anagrams of the string “abc”:

1. abc
2. acb
3. bac
4. bca
5. cab
6. cba

Write a function to return a list of starting indices of the anagrams of the pattern in the given string.

**Example 1:**

```
Input: String="ppqp", Pattern="pq"
Output: [1, 2]
Explanation: The two anagrams of the pattern in the given string are "pq" and "qp".
```

**Example 2:**

```

Input: String="abbcabc", Pattern="abc"
Output: [2, 3, 4]
Explanation: The three anagrams of the pattern in the given string are "bca", "cab", and "abc".
```

## Solution

This problem follows the **Sliding Window** pattern and is very similar to [Permutation in a String](https://www.educative.io/collection/page/5668639101419520/5671464854355968/5401934796161024/). In this problem, we need to find every occurrence of any permutation of the pattern in the string. We will use a list to store the starting indices of the anagrams of the pattern in the string.

## Code 

Here is what our algorithm will look like, only the highlighted lines have changed from [Permutation in a String](https://www.educative.io/collection/page/5668639101419520/5671464854355968/5401934796161024/):

```java
import java.util.*;

class StringAnagrams {
  public static List<Integer> findStringAnagrams(String str, String pattern) {
    int windowStart = 0, matched = 0;
    Map<Character, Integer> charFrequencyMap = new HashMap<>();
    for (char chr : pattern.toCharArray())
      charFrequencyMap.put(chr, charFrequencyMap.getOrDefault(chr, 0) + 1);

    List<Integer> resultIndices = new ArrayList<Integer>();
    // our goal is to match all the characters from the map with the current window
    for (int windowEnd = 0; windowEnd < str.length(); windowEnd++) {
      char rightChar = str.charAt(windowEnd);
      // decrement the frequency of the matched character
      if (charFrequencyMap.containsKey(rightChar)) {
        charFrequencyMap.put(rightChar, charFrequencyMap.get(rightChar) - 1);
        if (charFrequencyMap.get(rightChar) == 0)
          matched++;
      }

      if (matched == charFrequencyMap.size()) // have we found an anagram?
        resultIndices.add(windowStart);

      if (windowEnd >= pattern.length() - 1) { // shrink the window
        char leftChar = str.charAt(windowStart++);
        if (charFrequencyMap.containsKey(leftChar)) {
          if (charFrequencyMap.get(leftChar) == 0)
            matched--; // before putting the character back, decrement the matched count
          // put the character back
          charFrequencyMap.put(leftChar, charFrequencyMap.get(leftChar) + 1);
        }
      }
    }

    return resultIndices;
  }

  public static void main(String[] args) {
    System.out.println(StringAnagrams.findStringAnagrams("ppqp", "pq"));
    System.out.println(StringAnagrams.findStringAnagrams("abbcabc", "abc"));
  }
}
```

## Time Complexity 

The time complexity of the above algorithm will be O(N + M)where ‘N’ and ‘M’ are the number of characters in the input string and the pattern respectively.

## Space Complexity 

The space complexity of the algorithm is O(M) since in the worst case, the whole pattern can have distinct characters which will go into the **HashMap**. In the worst case, we also need O(N)space for the result list, this will happen when the pattern has only one character and the string contains only that character.

# Smallest Window containing Substring (hard) 

Given a string and a pattern, find the **smallest substring** in the given string which has **all the characters of the given pattern**.

**Example 1:**

```
Input: String="aabdec", Pattern="abc"
Output: "abdec"
Explanation: The smallest substring having all characters of the pattern is "abdec"
```

**Example 2:**

```
Input: String="abdbca", Pattern="abc"
Output: "bca"
Explanation: The smallest substring having all characters of the pattern is "bca".
```

**Example 3:**

```
Input: String="adcad", Pattern="abc"
Output: ""
Explanation: No substring in the given string has all characters of the pattern.
```

## Solution 

This problem follows the **Sliding Window** pattern and has a lot of similarities with [Permutation in a String](https://www.educative.io/collection/page/5668639101419520/5671464854355968/5401934796161024/) with one difference. In this problem, we need to find a substring having all characters of the pattern which means that the required substring can have some additional characters and doesn’t need to be a permutation of the pattern. Here is how we will manage these differences:

1. We will keep a running count of every matching instance of a character.
2. Whenever we have matched all the characters, we will try to shrink the window from the beginning, keeping track of the smallest substring that has all the matching characters.
3. We will stop the shrinking process as soon as we remove a matched character from the sliding window. One thing to note here is that we could have redundant matching characters, e.g., we might have two ‘a’ in the sliding window when we only need one ‘a’. In that case, when we encounter the first ‘a’, we will simply shrink the window without decrementing the matched count. We will decrement the matched count when the second ‘a’ goes out of the window.

## Code 

Here is how our algorithm will look; only the highlighted lines have changed from [Permutation in a String](https://www.educative.io/collection/page/5668639101419520/5671464854355968/5401934796161024/):

```java
import java.util.*;

class MinimumWindowSubstring {
  public static String findSubstring(String str, String pattern) {
    int windowStart = 0, matched = 0, minLength = str.length() + 1, subStrStart = 0;
    Map<Character, Integer> charFrequencyMap = new HashMap<>();
    for (char chr : pattern.toCharArray())
      charFrequencyMap.put(chr, charFrequencyMap.getOrDefault(chr, 0) + 1);

    // try to extend the range [windowStart, windowEnd]
    for (int windowEnd = 0; windowEnd < str.length(); windowEnd++) {
      char rightChar = str.charAt(windowEnd);
      if (charFrequencyMap.containsKey(rightChar)) {
        charFrequencyMap.put(rightChar, charFrequencyMap.get(rightChar) - 1);
        if (charFrequencyMap.get(rightChar) >= 0) // count every matching of a character
          matched++;
      }

      // shrink the window if we can, finish as soon as we remove a matched character
      while (matched == pattern.length()) {
        if (minLength > windowEnd - windowStart + 1) {
          minLength = windowEnd - windowStart + 1;
          subStrStart = windowStart;
        }

        char leftChar = str.charAt(windowStart++);
        if (charFrequencyMap.containsKey(leftChar)) {
          // note that we could have redundant matching characters, therefore we'll decrement the
          // matched count only when a useful occurrence of a matched character is going out of the window
          if (charFrequencyMap.get(leftChar) == 0)
            matched--;
          charFrequencyMap.put(leftChar, charFrequencyMap.get(leftChar) + 1);
        }
      }
    }

    return minLength > str.length() ? "" : str.substring(subStrStart, subStrStart + minLength);
  }

  public static void main(String[] args) {
    System.out.println(MinimumWindowSubstring.findSubstring("aabdec", "abc"));
    System.out.println(MinimumWindowSubstring.findSubstring("abdbca", "abc"));
    System.out.println(MinimumWindowSubstring.findSubstring("adcad", "abc"));
  }
}

```

## Time Complexity

The time complexity of the above algorithm will be O(N + M) where ‘N’ and ‘M’ are the number of characters in the input string and the pattern respectively.

## Space Complexity

The space complexity of the algorithm is O(M) since in the worst case, the whole pattern can have distinct characters which will go into the **HashMap**. In the worst case, we also need O(N) space for the resulting substring, which will happen when the input string is a permutation of the pattern.

# Words Concatenation (hard) 

Given a string and a list of words, find all the starting indices of substrings in the given string that are a **concatenation of all the given words** exactly once **without any overlapping** of words. It is given that all words are of the same length.

**Example 1:**

```
Input: String="catfoxcat", Words=["cat", "fox"]
Output: [0, 3]
Explanation: The two substring containing both the words are "catfox" & "foxcat".
```

**Example 2:**

```
Input: String="catcatfoxfox", Words=["cat", "fox"]
Output: [3]
Explanation: The only substring containing both the words is "catfox".
```

## Solution 

This problem follows the **Sliding Window** pattern and has a lot of similarities with [Maximum Sum Subarray of Size K](https://www.educative.io/collection/page/5668639101419520/5671464854355968/5177043027230720/). We will keep track of all the words in a **HashMap** and try to match them in the given string. Here are the set of steps for our algorithm:

1. Keep the frequency of every word in a **HashMap**.
2. Starting from every index in the string, try to match all the words.
3. In each iteration, keep track of all the words that we have already seen in another **HashMap**.
4. If a word is not found or has a higher frequency than required, we can move on to the next character in the string.
5. Store the index if we have found all the words.

## Code

Here is what our algorithm will look like:

```java
import java.util.*;

class WordConcatenation {
  public static List<Integer> findWordConcatenation(String str, String[] words) {
    Map<String, Integer> wordFrequencyMap = new HashMap<>();
    for (String word : words)
      wordFrequencyMap.put(word, wordFrequencyMap.getOrDefault(word, 0) + 1);

    List<Integer> resultIndices = new ArrayList<Integer>();
    int wordsCount = words.length, wordLength = words[0].length();

    for (int i = 0; i <= str.length() - wordsCount * wordLength; i++) {
      Map<String, Integer> wordsSeen = new HashMap<>();
      for (int j = 0; j < wordsCount; j++) {
        int nextWordIndex = i + j * wordLength;
        // get the next word from the string
        String word = str.substring(nextWordIndex, nextWordIndex + wordLength);
        if (!wordFrequencyMap.containsKey(word)) // break if we don't need this word
          break;

        wordsSeen.put(word, wordsSeen.getOrDefault(word, 0) + 1); // add the word to the 'wordsSeen' map

        // no need to process further if the word has higher frequency than required 
        if (wordsSeen.get(word) > wordFrequencyMap.getOrDefault(word, 0))
          break;

        if (j + 1 == wordsCount) // store index if we have found all the words
          resultIndices.add(i);
      }
    }

    return resultIndices;
  }

  public static void main(String[] args) {
    List<Integer> result = WordConcatenation.findWordConcatenation("catfoxcat", new String[] { "cat", "fox" });
    System.out.println(result);
    result = WordConcatenation.findWordConcatenation("catcatfoxfox", new String[] { "cat", "fox" });
    System.out.println(result);
  }
}

```

## Time Complexity

The time complexity of the above algorithm will be O(N * M * Len) where ‘N’ is the number of characters in the given string, ‘M’ is the total number of words, and ‘Len’ is the length of a word.

### Space Complexity [#](https://www.educative.io/courses/grokking-the-coding-interview/N8nMBvDQJ0m#space-complexity)

The space complexity of the algorithm is O(M) since at most, we will be storing all the words in the two **HashMaps**. In the worst case, we also need O(N) space for the resulting list. So, the overall space complexity of the algorithm will be O(M+N).

