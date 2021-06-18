[TOC]

# Introduction

A huge number of coding interview problems involve dealing with [Permutations](https://en.wikipedia.org/wiki/Permutation) and [Combinations](https://en.wikipedia.org/wiki/Combination) of a given set of elements. This pattern describes an efficient **Breadth First Search (BFS)** approach to handle all these problems.

Let’s jump onto our first problem to develop an understanding of this pattern.

# Subsets (easy)

## Problem Statement 

Given a set with distinct elements, find all of its distinct subsets

```
Input: [1, 3]
Output: [], [1], [3], [1,3]
```

```
Input: [1, 5, 3]
Output: [], [1], [5], [3], [1,5], [1,3], [5,3], [1,5,3]
```

## Solution

To generate all subsets of the given set, we can use the **Breadth First Search (BFS)** approach. We can start with an empty set, iterate through all numbers one-by-one, and add them to existing sets to create new subsets.

Let’s take the example-2 mentioned above to go through each step of our algorithm:

Given set: [1, 5, 3]

1. Start with an empty set: [[]]
2. Add the first number (1) to all the existing subsets to create new subsets: [[], **[1]]**;
3. Add the second number (5) to all the existing subsets: [[], [1], **[5], [1,5]**];
4. Add the third number (3) to all the existing subsets: [[], [1], [5], [1,5], **[3], [1,3], [5,3], [1,5,3]**].

Here is the visual representation of the above steps:

![image-20210616180926135](/Users/zhengwenxuan/Library/Application Support/typora-user-images/image-20210616180926135.png)

Since the input set has distinct elements, the above steps will ensure that we will not have any duplicate subsets.

## Code

```java
import java.util.*;

class Subsets {

  public static List<List<Integer>> findSubsets(int[] nums) {
    List<List<Integer>> subsets = new ArrayList<>();
    // start by adding the empty subset
    subsets.add(new ArrayList<>());
    for (int currentNumber : nums) {
      // we will take all existing subsets and insert the current number in them to create new subsets
      int n = subsets.size();
      for (int i = 0; i < n; i++) {
        // create a new subset from the existing subset and insert the current element to it
        List<Integer> set = new ArrayList<>(subsets.get(i));
        set.add(currentNumber);
        subsets.add(set);
      }
    }
    return subsets;
  }

  public static void main(String[] args) {
    List<List<Integer>> result = Subsets.findSubsets(new int[] { 1, 3 });
    System.out.println("Here is the list of subsets: " + result);

    result = Subsets.findSubsets(new int[] { 1, 5, 3 });
    System.out.println("Here is the list of subsets: " + result);
  }
}

```

## Time complexity 

Since, in each step, the number of subsets doubles as we add each element to all the existing subsets, therefore, we will have a total of O(2^N) subsets, where ‘N’ is the total number of elements in the input set. And since we construct a new subset from an existing set, therefore, the time complexity of the above algorithm will be O(N*2^N).

## Space complexity 

All the additional space used by our algorithm is for the output list. Since we will have a total of O(2^N) subsets, and each subset can take up to O(N)space, therefore, the space complexity of our algorithm will be O(N*2^N).

# Subsets With Duplicates

## Problem Statement

Given a set of numbers that might contain duplicates, find all of its distinct subsets.

```
Input: [1, 3, 3]
Output: [], [1], [3], [1,3], [3,3], [1,3,3]
```

```
Input: [1, 5, 3, 3]
Output: [], [1], [5], [3], [1,5], [1,3], [5,3], [1,5,3], [3,3], [1,3,3], [3,3,5], [1,5,3,3] 
```

## Solution

This problem follows the [Subsets](https://www.educative.io/collection/page/5668639101419520/5671464854355968/5670249378611200) pattern and we can follow a similar **Breadth First Search (BFS)** approach. The only additional thing we need to do is handle duplicates. Since the given set can have duplicate numbers, if we follow the same approach discussed in [Subsets](https://www.educative.io/collection/page/5668639101419520/5671464854355968/5670249378611200), we will end up with duplicate subsets, which is not acceptable. To handle this, we will do two extra things:

1. Sort all numbers of the given set. This will ensure that all duplicate numbers are next to each other.
2. Follow the same BFS approach but whenever we are about to process a duplicate (i.e., when the current and the previous numbers are same), instead of adding the current number (which is a duplicate) to all the existing subsets, only add it to the subsets which were created in the previous step.

Let’s take Example-2 mentioned above to go through each step of our algorithm:

```
Given set: [1, 5, 3, 3]  
Sorted set: [1, 3, 3, 5]
```

1. Start with an empty set: [[]]
2. Add the first number (1) to all the existing subsets to create new subsets: [[], [1]];
3. Add the second number (3) to all the existing subsets: [[], [1], [3], [1,3]].
4. The next number (3) is a duplicate. If we add it to all existing subsets we will get:

```
 [[], [1], [3], [1,3], [3], [1,3], [3,3], [1,3,3]]
```

```
We got two duplicate subsets: [3], [1,3]  
Whereas we only needed the new subsets: [3,3], [1,3,3]  
```

To handle this instead of adding (3) to all the existing subsets, we only add it to the new subsets which were created in the previous (3rd) step:

````
which is [[3], [1,3]] => [[3,3], [1,3,3]
````

```
 the result becomes: [[], [1], [3], [1,3]] + [[3,3], [1,3,3] =  [[], [1], [3], [1,3], [3,3], [1,3,3]]
```

5. Finally, add the forth number (5) to all the existing subsets: [[], [1], [3], [1,3], [3,3], [1,3,3], [5], [1,5], [3,5], [1,3,5], [3,3,5], [1,3,3,5]]

Here is the visual representation of the above steps:

![image-20210616183058932](/Users/zhengwenxuan/Library/Application Support/typora-user-images/image-20210616183058932.png)

## Code

```java
import java.util.*;

class SubsetWithDuplicates {

  public static List<List<Integer>> findSubsets(int[] nums) {
    // sort the numbers to handle duplicates
    Arrays.sort(nums);
    List<List<Integer>> subsets = new ArrayList<>();
    subsets.add(new ArrayList<>());
    int startIndex = 0, endIndex = 0;
    for (int i = 0; i < nums.length; i++) {
      startIndex = 0;
      // if current and the previous elements are same, create new subsets only from the subsets 
      // added in the previous step
      if (i > 0 && nums[i] == nums[i - 1])
        startIndex = endIndex + 1;
      	endIndex = subsets.size() - 1;
      for (int j = startIndex; j <= endIndex; j++) {
        // create a new subset from the existing subset and add the current element to it
        List<Integer> set = new ArrayList<>(subsets.get(j));
        set.add(nums[i]);
        subsets.add(set);
      }
    }
    return subsets;
  }

  public static void main(String[] args) {
    List<List<Integer>> result = SubsetWithDuplicates.findSubsets(new int[] { 1, 3, 3 });
    System.out.println("Here is the list of subsets: " + result);

    result = SubsetWithDuplicates.findSubsets(new int[] { 1, 5, 3, 3 });
    System.out.println("Here is the list of subsets: " + result);
  }
}

```

## Time complexity

Since, in each step, the number of subsets doubles (if not duplicate) as we add each element to all the existing subsets, therefore, we will have a total of O(2^N) subsets, where ‘N’ is the total number of elements in the input set. And since we construct a new subset from an existing set, therefore, the time complexity of the above algorithm will be O(N*2^N).

## Space complexity 

All the additional space used by our algorithm is for the output list. Since, at most, we will have a total of O(2^N) subsets, and each subset can take up to O(N) space, therefore, the space complexity of our algorithm will be O(N*2^N).

# Permutations

## Problem Statement 

Given a set of distinct numbers, find all of its permutations.

**Permutation** is defined as the re-arranging of the elements of the set. For example, {1, 2, 3} has the following six permutations:

1. {1, 2, 3}
2. {1, 3, 2}
3. {2, 1, 3}
4. {2, 3, 1}
5. {3, 1, 2}
6. {3, 2, 1}

If a set has ‘n’ distinct elements it will have n! permutations.

```
Input: [1,3,5]
Output: [1,3,5], [1,5,3], [3,1,5], [3,5,1], [5,1,3], [5,3,1]
```

## Solution

This problem follows the [Subsets](https://www.educative.io/collection/page/5668639101419520/5671464854355968/5670249378611200) pattern and we can follow a similar **Breadth First Search (BFS)** approach. However, unlike [Subsets](https://www.educative.io/collection/page/5668639101419520/5671464854355968/5670249378611200), every permutation must contain all the numbers.

Let’s take the example-1 mentioned above to generate all the permutations. Following a BFS approach, we will consider one number at a time:

1. If the given set is empty then we have only an empty permutation set: []
2. Let’s add the first element (1), the permutations will be: [1]
3. Let’s add the second element (3), the permutations will be: [3,1], [1,3]
4. Let’s add the third element (5), the permutations will be: [5,3,1], [3,5,1], [3,1,5], [5,1,3], [1,5,3], [1,3,5]

Let’s analyze the permutations in the 3rd and 4th step. How can we generate permutations in the 4th step from the permutations of the 3rd step?

If we look closely, we will realize that when we add a new number (5), we take each permutation of the previous step and insert the new number in every position to generate the new permutations. For example, inserting ‘5’ in different positions of [3,1] will give us the following permutations:

1. Inserting ‘5’ before ‘3’: [5,3,1]
2. Inserting ‘5’ between ‘3’ and ‘1’: [3,5,1]
3. Inserting ‘5’ after ‘1’: [3,1,5]

Here is the visual representation of this algorithm:

![image-20210616183610970](/Users/zhengwenxuan/Library/Application Support/typora-user-images/image-20210616183610970.png)

## Code

```java
import java.util.*;

class Permutations {

  public static List<List<Integer>> findPermutations(int[] nums) {
    List<List<Integer>> result = new ArrayList<>();
    Queue<List<Integer>> permutations = new LinkedList<>();
    permutations.add(new ArrayList<>());
    for (int currentNumber : nums) {
      // we will take all existing permutations and add the current number to create new permutations
      int n = permutations.size();
      for (int i = 0; i < n; i++) {
        List<Integer> oldPermutation = permutations.poll();
        // create a new permutation by adding the current number at every position
        for (int j = 0; j <= oldPermutation.size(); j++) {
          List<Integer> newPermutation = new ArrayList<Integer>(oldPermutation);
          newPermutation.add(j, currentNumber);
          if (newPermutation.size() == nums.length)
            result.add(newPermutation);
          else
            permutations.add(newPermutation);
        }
      }
    }
    return result;
  }

  public static void main(String[] args) {
    List<List<Integer>> result = Permutations.findPermutations(new int[] { 1, 3, 5 });
    System.out.print("Here are all the permutations: " + result);
  }
}

```

## Time complexity 

We know that there are a total of N! permutations of a set with ‘N’ numbers. In the algorithm above, we are iterating through all of these permutations with the help of the two ‘for’ loops. In each iteration, we go through all the current permutations to insert a new number in them on line 17 (line 23 for C++ solution). To insert a number into a permutation of size ‘N’ will take O(N), which makes the overall time complexity of our algorithm O*(*N*∗*N!).

## Space complexity 

All the additional space used by our algorithm is for the result list and the queue to store the intermediate permutations. If you see closely, at any time, we don’t have more than N! permutations between the result list and the queue. Therefore the overall space complexity to store N!*N*! permutations each containing N*N* elements will be O(N*N!).

## Recursive Solution [#](https://www.educative.io/courses/grokking-the-coding-interview/B8R83jyN3KY#recursive-solution)

Here is the recursive algorithm following a similar approach:

```java
import java.util.*;

class PermutationsRecursive {

  public static List<List<Integer>> generatePermutations(int[] nums) {
    List<List<Integer>> result = new ArrayList<>();
    generatePermutationsRecursive(nums, 0, new ArrayList<Integer>(), result);
    return result;
  }

  private static void generatePermutationsRecursive(int[] nums, int index, List<Integer> currentPermutation,
      List<List<Integer>> result) {
    if (index == nums.length) {
      result.add(currentPermutation);
    } else {
      // create a new permutation by adding the current number at every position
      for (int i = 0; i <= currentPermutation.size(); i++) {
        List<Integer> newPermutation = new ArrayList<Integer>(currentPermutation);
        newPermutation.add(i, nums[index]);
        generatePermutationsRecursive(nums, index + 1, newPermutation, result);
      }
    }
  }

  public static void main(String[] args) {
    List<List<Integer>> result = PermutationsRecursive.generatePermutations(new int[] { 1, 3, 5 });
    System.out.print("Here are all the permutations: " + result);
  }
}
```

# String Permutations by changing case (medium)

## Problem Statement [#](https://www.educative.io/courses/grokking-the-coding-interview/xVlKmyX542P#problem-statement)

Given a string, find all of its permutations preserving the character sequence but changing case.

**Example 1:**

```
Input: "ad52"
Output: "ad52", "Ad52", "aD52", "AD52" 
```

**Example 2:**

```
Input: "ab7c"
Output: "ab7c", "Ab7c", "aB7c", "AB7c", "ab7C", "Ab7C", "aB7C", "AB7C"
```

## Solution [#](https://www.educative.io/courses/grokking-the-coding-interview/xVlKmyX542P#solution)

This problem follows the [Subsets](https://www.educative.io/collection/page/5668639101419520/5671464854355968/5670249378611200) pattern and can be mapped to [Permutations](https://www.educative.io/collection/page/5668639101419520/5671464854355968/5720758194012160/).

Let’s take Example-2 mentioned above to generate all the permutations. Following a **BFS** approach, we will consider one character at a time. Since we need to preserve the character sequence, we can start with the actual string and process each character (i.e., make it upper-case or lower-case) one by one:

1. Starting with the actual string: `"ab7c"`
2. Processing the first character (‘a’), we will get two permutations: `"ab7c", "Ab7c"`
3. Processing the second character (‘b’), we will get four permutations: `"ab7c", "Ab7c", "aB7c", "AB7c"`
4. Since the third character is a digit, we can skip it.
5. Processing the fourth character (‘c’), we will get a total of eight permutations: `"ab7c", "Ab7c", "aB7c", "AB7c", "ab7C", "Ab7C", "aB7C", "AB7C"`

Let’s analyze the permutations in the 3rd and the 5th step. How can we generate the permutations in the 5th step from the permutations in the 3rd step?

If we look closely, we will realize that in the 5th step, when we processed the new character (‘c’), we took all the permutations of the previous step (3rd) and changed the case of the letter (‘c’) in them to create four new permutations.

Here is the visual representation of this algorithm:

![image-20210618161200995](/Users/zhengwenxuan/Desktop/Coding Pattern/image-20210618161200995.png)

## Code [#](https://www.educative.io/courses/grokking-the-coding-interview/xVlKmyX542P#code)

Here is what our algorithm will look like:

```java
import java.util.*;

class LetterCaseStringPermutation {

  public static List<String> findLetterCaseStringPermutations(String str) {
    List<String> permutations = new ArrayList<>();
    if (str == null)
      return permutations;

    permutations.add(str);
    // process every character of the string one by one
    for (int i = 0; i < str.length(); i++) {
      if (Character.isLetter(str.charAt(i))) { // only process characters, skip digits
        // we will take all existing permutations and change the letter case appropriately
        int n = permutations.size();
        for (int j = 0; j < n; j++) {
          char[] chs = permutations.get(j).toCharArray();
          // if the current character is in upper case change it to lower case or vice versa
          if (Character.isUpperCase(chs[i]))
            chs[i] = Character.toLowerCase(chs[i]);
          else
            chs[i] = Character.toUpperCase(chs[i]);
          permutations.add(String.valueOf(chs));
        }
      }
    }
    return permutations;
  }

  public static void main(String[] args) {
    List<String> result = LetterCaseStringPermutation.findLetterCaseStringPermutations("ad52");
    System.out.println(" String permutations are: " + result);

    result = LetterCaseStringPermutation.findLetterCaseStringPermutations("ab7c");
    System.out.println(" String permutations are: " + result);
  }
}
```

## Time complexity [#](https://www.educative.io/courses/grokking-the-coding-interview/xVlKmyX542P#time-complexity)

Since we can have 2^N permutations at the most and while processing each permutation we convert it into a character array, the overall time complexity of the algorithm will be O(N*2^N)*.

## Space complexity 

All the additional space used by our algorithm is for the output list. Since we can have a total of O(2^N) permutations, the space complexity of our algorithm is O(N*2^N).

# Balanced Parentheses (hard)

## Problem Statement [#](https://www.educative.io/courses/grokking-the-coding-interview/NEXBg8YA5A2#problem-statement)

For a given number ‘N’, write a function to generate all combination of ‘N’ pairs of balanced parentheses.

**Example 1:**

```
Input: N=2
Output: (()), ()()
```

**Example 2:**

```
Input: N=3
Output: ((())), (()()), (())(), ()(()), ()()()
```

## Solution [#](https://www.educative.io/courses/grokking-the-coding-interview/NEXBg8YA5A2#solution)

This problem follows the [Subsets](https://www.educative.io/collection/page/5668639101419520/5671464854355968/5670249378611200) pattern and can be mapped to [Permutations](https://www.educative.io/collection/page/5668639101419520/5671464854355968/5720758194012160/). We can follow a similar BFS approach.

Let’s take Example-2 mentioned above to generate all the combinations of balanced parentheses. Following a BFS approach, we will keep adding open parentheses `(` or close parentheses `)`. At each step we need to keep two things in mind:

- We can’t add more than ‘N’ open parenthesis.
- To keep the parentheses balanced, we can add a close parenthesis `)` only when we have already added enough open parenthesis `(`. For this, we can keep a count of open and close parenthesis with every combination.

Following this guideline, let’s generate parentheses for N=3:

1. Start with an empty combination: “”
2. At every step, let’s take all combinations of the previous step and add `(` or `)` keeping the above-mentioned two rules in mind.
3. For the empty combination, we can add `(` since the count of open parenthesis will be less than ‘N’. We can’t add `)` as we don’t have an equivalent open parenthesis, so our list of combinations will now be: “(”
4. For the next iteration, let’s take all combinations of the previous set. For “(” we can add another `(` to it since the count of open parenthesis will be less than ‘N’. We can also add `)` as we do have an equivalent open parenthesis, so our list of combinations will be: “((”, “()”
5. In the next iteration, for the first combination “((”, we can add another `(` to it as the count of open parenthesis will be less than ‘N’, we can also add `)` as we do have an equivalent open parenthesis. This gives us two new combinations: “(((” and “(()”. For the second combination “()”, we can add another `(` to it since the count of open parenthesis will be less than ‘N’. We can’t add `)` as we don’t have an equivalent open parenthesis, so our list of combinations will be: “(((”, “(()”, ()("
6. Following the same approach, next we will get the following list of combinations: “((()”, “(()(”, “(())”, “()((”, “()()”
7. Next we will get: “((())”, “(()()”, “(())(”, “()(()”, “()()(”
8. Finally, we will have the following combinations of balanced parentheses: “((()))”, “(()())”, “(())()”, “()(())”, “()()()”
9. We can’t add more parentheses to any of the combinations, so we stop here.

Here is the visual representation of this algorithm:

![image-20210618161411913](/Users/zhengwenxuan/Desktop/Coding Pattern/image-20210618161411913.png)

## Code [#](https://www.educative.io/courses/grokking-the-coding-interview/NEXBg8YA5A2#code)

Here is what our algorithm will look like:

```java
import java.util.*;

class ParenthesesString {
  String str;
  int openCount; // open parentheses count
  int closeCount; // close parentheses count

  public ParenthesesString(String s, int openCount, int closeCount) {
    str = s;
    this.openCount = openCount;
    this.closeCount = closeCount;
  }
}

class GenerateParentheses {

  public static List<String> generateValidParentheses(int num) {
    List<String> result = new ArrayList<String>();
    Queue<ParenthesesString> queue = new LinkedList<>();
    queue.add(new ParenthesesString("", 0, 0));
    while (!queue.isEmpty()) {
      ParenthesesString ps = queue.poll();
      // if we've reached the maximum number of open and close parentheses, add to the result
      if (ps.openCount == num && ps.closeCount == num) {
        result.add(ps.str);
      } else {
        if (ps.openCount < num) // if we can add an open parentheses, add it
          queue.add(new ParenthesesString(ps.str + "(", ps.openCount + 1, ps.closeCount));

        if (ps.openCount > ps.closeCount) // if we can add a close parentheses, add it
          queue.add(new ParenthesesString(ps.str + ")", ps.openCount, ps.closeCount + 1));
      }
    }
    return result;
  }

  public static void main(String[] args) {
    List<String> result = GenerateParentheses.generateValidParentheses(2);
    System.out.println("All combinations of balanced parentheses are: " + result);

    result = GenerateParentheses.generateValidParentheses(3);
    System.out.println("All combinations of balanced parentheses are: " + result);
  }
}
```

### Time complexity [#](https://www.educative.io/courses/grokking-the-coding-interview/NEXBg8YA5A2#time-complexity)

Let’s try to estimate how many combinations we can have for ‘N’ pairs of balanced parentheses. If we don’t care for the ordering - *that `)` can only come after `(`* - then we have two options for every position, i.e., either put open parentheses or close parentheses. This means we can have a maximum of 2^N combinations. Because of the ordering, the actual number will be less than 2^N.

If you see the visual representation of Example-2 closely you will realize that, in the worst case, it is equivalent to a binary tree, where each node will have two children. This means that we will have 2^N leaf nodes and 2^N-1 intermediate nodes. So the total number of elements pushed to the queue will be 2^N+ 2^N-1, which is asymptotically equivalent to O(2^N). While processing each element, we do need to concatenate the current string with `(` or `)`. This operation will take O(N), so the overall time complexity of our algorithm will be O(N*2^N). This is not completely accurate but reasonable enough to be presented in the interview.

The actual time complexity (O*(4*n /√n ) is bounded by the [Catalan number](https://en.wikipedia.org/wiki/Catalan_number) and is beyond the scope of a coding interview. See more details

### Space complexity [#](https://www.educative.io/courses/grokking-the-coding-interview/NEXBg8YA5A2#space-complexity)

All the additional space used by our algorithm is for the output list. Since we can’t have more than O(2^N) combinations, the space complexity of our algorithm is O(N*2^N).

## Recursive Solution

Here is the recursive algorithm following a similar approach:

```java
import java.util.*;

class GenerateParenthesesRecursive {

  public static List<String> generateValidParentheses(int num) {
    List<String> result = new ArrayList<String>();
    char[] parenthesesString = new char[2 * num];
    generateValidParenthesesRecursive(num, 0, 0, parenthesesString, 0, result);
    return result;
  }

  private static void generateValidParenthesesRecursive(int num, int openCount, int closeCount,
      char[] parenthesesString, int index, List<String> result) {

    // if we've reached the maximum number of open and close parentheses, add to the result
    if (openCount == num && closeCount == num) {
      result.add(new String(parenthesesString));
    } else {
      if (openCount < num) { // if we can add an open parentheses, add it
        parenthesesString[index] = '(';
        generateValidParenthesesRecursive(num, openCount + 1, closeCount, parenthesesString, index + 1, result);
      }

      if (openCount > closeCount) { // if we can add a close parentheses, add it
        parenthesesString[index] = ')';
        generateValidParenthesesRecursive(num, openCount, closeCount + 1, parenthesesString, index + 1, result);
      }
    }
  }

  public static void main(String[] args) {
    List<String> result = GenerateParenthesesRecursive.generateValidParentheses(2);
    System.out.println("All combinations of balanced parentheses are: " + result);

    result = GenerateParenthesesRecursive.generateValidParentheses(3);
    System.out.println("All combinations of balanced parentheses are: " + result);
  }
}
```

# Unique Generalized Abbreviations (hard)

## Problem Statement [#](https://www.educative.io/courses/grokking-the-coding-interview/NEOZDEg5PlN#problem-statement)

Given a word, write a function to generate all of its unique generalized abbreviations.

Generalized abbreviation of a word can be generated by replacing each substring of the word by the count of characters in the substring. Take the example of “ab” which has four substrings: “”, “a”, “b”, and “ab”. After replacing these substrings in the actual word by the count of characters we get all the generalized abbreviations: “ab”, “1b”, “a1”, and “2”.

**Example 1:**

```
Input: "BAT"
Output: "BAT", "BA1", "B1T", "B2", "1AT", "1A1", "2T", "3"
```

**Example 2:**

```
Input: "code"
Output: "code", "cod1", "co1e", "co2", "c1de", "c1d1", "c2e", "c3", "1ode", "1od1", "1o1e", "1o2", 
"2de", "2d1", "3e", "4"
```

## Solution [#](https://www.educative.io/courses/grokking-the-coding-interview/NEOZDEg5PlN#solution)

This problem follows the [Subsets](https://www.educative.io/collection/page/5668639101419520/5671464854355968/5670249378611200) pattern and can be mapped to [Balanced Parentheses](https://www.educative.io/collection/page/5668639101419520/5671464854355968/5753264117121024/). We can follow a similar BFS approach.

Let’s take Example-1 mentioned above to generate all unique generalized abbreviations. Following a BFS approach, we will abbreviate one character at a time. At each step we have two options:

- Abbreviate the current character, or
- Add the current character to the output and skip abbreviation.

Following these two rules, let’s abbreviate `BAT`:

1. Start with an empty word: “”
2. At every step, we will take all the combinations from the previous step and apply the two abbreviation rules to the next character.
3. Take the empty word from the previous step and add the first character to it. We can either abbreviate the character or add it (by skipping abbreviation). This gives us two new words: `_`, `B`.
4. In the next iteration, let’s add the second character. Applying the two rules on `_` will give us `_ _` and `1A`. Applying the above rules to the other combination `B` gives us `B_` and `BA`.
5. The next iteration will give us: `_ _ _`, `2T`, `1A_`, `1AT`, `B _ _`, `B1T`, `BA_`, `BAT`
6. The final iteration will give us:`3`, `2T`, `1A1`, `1AT`, `B2`, `B1T`, `BA1`, `BAT`

Here is the visual representation of this algorithm:

![image-20210618161801810](/Users/zhengwenxuan/Desktop/Coding Pattern/image-20210618161801810.png)

## Code [#](https://www.educative.io/courses/grokking-the-coding-interview/NEOZDEg5PlN#code)

Here is what our algorithm will look like:

```java
import java.util.*;

class AbbreviatedWord {
  StringBuilder str;
  int start;
  int count;

  public AbbreviatedWord(StringBuilder str, int start, int count) {
    this.str = str;
    this.start = start;
    this.count = count;
  }
}

class GeneralizedAbbreviation {

  public static List<String> generateGeneralizedAbbreviation(String word) {
    int wordLen = word.length();
    List<String> result = new ArrayList<String>();
    Queue<AbbreviatedWord> queue = new LinkedList<>();
    queue.add(new AbbreviatedWord(new StringBuilder(), 0, 0));
    while (!queue.isEmpty()) {
      AbbreviatedWord abWord = queue.poll();
      if (abWord.start == wordLen) {
        if (abWord.count != 0)
          abWord.str.append(abWord.count);
        result.add(abWord.str.toString());
      } else {
        // continue abbreviating by incrementing the current abbreviation count
        queue.add(new AbbreviatedWord(new StringBuilder(abWord.str), abWord.start + 1, abWord.count + 1));

        // restart abbreviating, append the count and the current character to the string
        if (abWord.count != 0)
          abWord.str.append(abWord.count);
        queue.add(
            new AbbreviatedWord(new StringBuilder(abWord.str).append(word.charAt(abWord.start)), abWord.start + 1, 0));
      }
    }

    return result;
  }

  public static void main(String[] args) {
    List<String> result = GeneralizedAbbreviation.generateGeneralizedAbbreviation("BAT");
    System.out.println("Generalized abbreviation are: " + result);

    result = GeneralizedAbbreviation.generateGeneralizedAbbreviation("code");
    System.out.println("Generalized abbreviation are: " + result);
  }
}
```

### Time complexity [#](https://www.educative.io/courses/grokking-the-coding-interview/NEOZDEg5PlN#time-complexity)

Since we had two options for each character, we will have a maximum of 2^Ncombinations. If you see the visual representation of Example-1 closely you will realize that it is equivalent to a binary tree, where each node has two children. This means that we will have 2^N leaf nodes and 2^N-1intermediate nodes, so the total number of elements pushed to the queue will be 2^N + 2^N-1, which is asymptotically equivalent to O(2^N). While processing each element, we do need to concatenate the current string with a character. This operation will take O(N), so the overall time complexity of our algorithm will be O(N*2^N)*.

### Space complexity [#](https://www.educative.io/courses/grokking-the-coding-interview/NEOZDEg5PlN#space-complexity)

All the additional space used by our algorithm is for the output list. Since we can’t have more than O(2^N) combinations, the space complexity of our algorithm is O(N*2^N).

## Recursive Solution [#](https://www.educative.io/courses/grokking-the-coding-interview/NEOZDEg5PlN#recursive-solution)

Here is the recursive algorithm following a similar approach:

```java
import java.util.*;

class GeneralizedAbbreviationRecursive {

  public static List<String> generateGeneralizedAbbreviation(String word) {
    List<String> result = new ArrayList<String>();
    generateAbbreviationRecursive(word, new StringBuilder(), 0, 0, result);
    return result;
  }

  private static void generateAbbreviationRecursive(String word, StringBuilder abWord, int start, int count,
      List<String> result) {

    if (start == word.length()) {
      if (count != 0)
        abWord.append(count);
      result.add(abWord.toString());
    } else {
      // continue abbreviating by incrementing the current abbreviation count
      generateAbbreviationRecursive(word, new StringBuilder(abWord), start + 1, count + 1, result);

      // restart abbreviating, append the count and the current character to the string
      if (count != 0)
        abWord.append(count);
      generateAbbreviationRecursive(word, new StringBuilder(abWord).append(word.charAt(start)), start + 1, 0, result);
    }
  }

  public static void main(String[] args) {
    List<String> result = GeneralizedAbbreviationRecursive.generateGeneralizedAbbreviation("BAT");
    System.out.println("Generalized abbreviation are: " + result);

    result = GeneralizedAbbreviationRecursive.generateGeneralizedAbbreviation("code");
    System.out.println("Generalized abbreviation are: " + result);
  }
}
```

# Evaluate Expression (hard) [#](https://www.educative.io/courses/grokking-the-coding-interview/gx7O7nO0R8l#evaluate-expression-hard)

Given an expression containing digits and operations (+, -, *), find all possible ways in which the expression can be evaluated by grouping the numbers and operators using parentheses.

**Example 1:**

```
Input: "1+2*3"
Output: 7, 9
Explanation: 1+(2*3) => 7 and (1+2)*3 => 9
```

**Example 2:**

```
Input: "2*3-4-5"
Output: 8, -12, 7, -7, -3 
Explanation: 2*(3-(4-5)) => 8, 2*(3-4-5) => -12, 2*3-(4-5) => 7, 2*(3-4)-5 => -7, (2*3)-4-5 => -3
```

## Solution [#](https://www.educative.io/courses/grokking-the-coding-interview/N0Q3PKRKMPz#solution)

This problem follows the [Subsets](https://www.educative.io/collection/page/5668639101419520/5671464854355968/5670249378611200) pattern and can be mapped to [Balanced Parentheses](https://www.educative.io/collection/page/5668639101419520/5671464854355968/5753264117121024/). We can follow a similar BFS approach.

Let’s take Example-1 mentioned above to generate different ways to evaluate the expression.

1. We can iterate through the expression character-by-character.
2. we can break the expression into two halves whenever we get an operator (+, -, *).
3. The two parts can be calculated by recursively calling the function.
4. Once we have the evaluation results from the left and right halves, we can combine them to produce all results.

## Code [#](https://www.educative.io/courses/grokking-the-coding-interview/N0Q3PKRKMPz#code)

Here is what our algorithm will look like:

```java
import java.util.*;

class EvaluateExpression {
  public static List<Integer> diffWaysToEvaluateExpression(String input) {
    List<Integer> result = new ArrayList<>();
    // base case: if the input string is a number, parse and add it to output.
    if (!input.contains("+") && !input.contains("-") && !input.contains("*")) {
      result.add(Integer.parseInt(input));
    } else {
      for (int i = 0; i < input.length(); i++) {
        char chr = input.charAt(i);
        if (!Character.isDigit(chr)) {
          // break the equation here into two parts and make recursively calls
          List<Integer> leftParts = diffWaysToEvaluateExpression(input.substring(0, i));
          List<Integer> rightParts = diffWaysToEvaluateExpression(input.substring(i + 1));
          for (int part1 : leftParts) {
            for (int part2 : rightParts) {
              if (chr == '+')
                result.add(part1 + part2);
              else if (chr == '-')
                result.add(part1 - part2);
              else if (chr == '*')
                result.add(part1 * part2);
            }
          }
        }
      }
    }
    return result;
  }

  public static void main(String[] args) {
    List<Integer> result = EvaluateExpression.diffWaysToEvaluateExpression("1+2*3");
    System.out.println("Expression evaluations: " + result);

    result = EvaluateExpression.diffWaysToEvaluateExpression("2*3-4-5");
    System.out.println("Expression evaluations: " + result);
  }

```

### Time complexity [#](https://www.educative.io/courses/grokking-the-coding-interview/N0Q3PKRKMPz#time-complexity)

The time complexity of this algorithm will be exponential and will be similar to [Balanced Parentheses](https://www.educative.io/collection/page/5668639101419520/5671464854355968/5753264117121024/). Estimated time complexity will be O(N*2^N)but the actual time complexity O*(4*n*/√*n*) is bounded by the [Catalan number](https://en.wikipedia.org/wiki/Catalan_number) and is beyond the scope of a coding interview. See more details [here](https://en.wikipedia.org/wiki/Central_binomial_coefficient).

### Space complexity [#](https://www.educative.io/courses/grokking-the-coding-interview/N0Q3PKRKMPz#space-complexity)

The space complexity of this algorithm will also be exponential, estimated at O(2^N)though the actual will be O*(4*n*/√*n).

## Memoized version [#](https://www.educative.io/courses/grokking-the-coding-interview/N0Q3PKRKMPz#memoized-version)

The problem has overlapping subproblems, as our recursive calls can be evaluating the same sub-expression multiple times. To resolve this, we can use memoization and store the intermediate results in a **HashMap**. In each function call, we can check our map to see if we have already evaluated this sub-expression before. Here is the memoized version of our algorithm; please see highlighted changes:

```java
import java.util.*;

class EvaluateExpression {
  // memoization map
  Map<String, List<Integer>> map = new HashMap<String, List<Integer>>();

  public List<Integer> diffWaysToEvaluateExpression(String input) {
    if (map.containsKey(input))
      return map.get(input);
    List<Integer> result = new ArrayList<>();
    // base case: if the input string is a number, parse and return it.
    if (!input.contains("+") && !input.contains("-") && !input.contains("*")) {
      result.add(Integer.parseInt(input));
    } else {
      for (int i = 0; i < input.length(); i++) {
        char chr = input.charAt(i);
        if (!Character.isDigit(chr)) {
          List<Integer> leftParts = diffWaysToEvaluateExpression(input.substring(0, i));
          List<Integer> rightParts = diffWaysToEvaluateExpression(input.substring(i + 1));
          for (int part1 : leftParts) {
            for (int part2 : rightParts) {
              if (chr == '+')
                result.add(part1 + part2);
              else if (chr == '-')
                result.add(part1 - part2);
              else if (chr == '*')
                result.add(part1 * part2);
            }
          }
        }
      }
    }
    map.put(input, result);
    return result;
  }

  public static void main(String[] args) {
    EvaluateExpression ee = new EvaluateExpression();
    List<Integer> result = ee.diffWaysToEvaluateExpression("1+2*3");
    System.out.println("Expression evaluations: " + result);
    
    ee = new EvaluateExpression();
    result = ee.diffWaysToEvaluateExpression("2*3-4-5");
    System.out.println("Expression evaluations: " + result);  }
}
```

## Structurally Unique Binary Search Trees (hard) [#](https://www.educative.io/courses/grokking-the-coding-interview/3j9V2QL3Ky9#structurally-unique-binary-search-trees-hard)

Given a number ‘n’, write a function to return all structurally unique Binary Search Trees (BST) that can store values 1 to ‘n’?

**Example 1:**

```
Input: 2
Output: List containing root nodes of all structurally unique BSTs.
Explanation: Here are the 2 structurally unique BSTs storing all numbers from 1 to 2:
```

![image-20210618162352052](/Users/zhengwenxuan/Desktop/Coding Pattern/image-20210618162352052.png)

**Example 2:**

```
Input: 3
Output: List containing root nodes of all structurally unique BSTs.
Explanation: Here are the 5 structurally unique BSTs storing all numbers from 1 to 3:
```

![image-20210618162403995](/Users/zhengwenxuan/Desktop/Coding Pattern/image-20210618162403995.png)

## Solution [#](https://www.educative.io/courses/grokking-the-coding-interview/xVQyDZBMpKE#solution)

This problem follows the [Subsets](https://www.educative.io/collection/page/5668639101419520/5671464854355968/5670249378611200) pattern and is quite similar to [Evaluate Expression](https://www.educative.io/collection/page/5668639101419520/5671464854355968/5712272949248000/). Following a similar approach, we can iterate from 1 to ‘n’ and consider each number as the root of a tree. All smaller numbers will make up the left sub-tree and bigger numbers will make up the right sub-tree. We will make recursive calls for the left and right sub-trees

## Code [#](https://www.educative.io/courses/grokking-the-coding-interview/xVQyDZBMpKE#code)

Here is what our algorithm will look like:

```java
import java.util.*;

class TreeNode {
  int val;
  TreeNode left;
  TreeNode right;

  TreeNode(int x) {
    val = x;
  }
};

class UniqueTrees {
  public static List<TreeNode> findUniqueTrees(int n) {
    if (n <= 0)
      return new ArrayList<TreeNode>();
    return findUniqueTreesRecursive(1, n);
  }

  public static List<TreeNode> findUniqueTreesRecursive(int start, int end) {
    List<TreeNode> result = new ArrayList<>();
    // base condition, return 'null' for an empty sub-tree
    // consider n=1, in this case we will have start=end=1, this means we should have only one tree
    // we will have two recursive calls, findUniqueTreesRecursive(1, 0) & (2, 1)
    // both of these should return 'null' for the left and the right child
    if (start > end) {
      result.add(null);
      return result;
    }

    for (int i = start; i <= end; i++) {
      // making 'i' root of the tree
      List<TreeNode> leftSubtrees = findUniqueTreesRecursive(start, i - 1);
      List<TreeNode> rightSubtrees = findUniqueTreesRecursive(i + 1, end);
      for (TreeNode leftTree : leftSubtrees) {
        for (TreeNode rightTree : rightSubtrees) {
          TreeNode root = new TreeNode(i);
          root.left = leftTree;
          root.right = rightTree;
          result.add(root);
        }
      }
    }
    return result;
  }

  public static void main(String[] args) {
    List<TreeNode> result = UniqueTrees.findUniqueTrees(2);
    System.out.print("Total trees: " + result.size());
  }
}
```

### Time complexity

The time complexity of this algorithm will be exponential and will be similar to [Balanced Parentheses](https://www.educative.io/collection/page/5668639101419520/5671464854355968/5753264117121024/). Estimated time complexity will be O(n*2^n)but the actual time complexity O*(4*n*/√n) is bounded by the [Catalan number](https://en.wikipedia.org/wiki/Catalan_number) and is beyond the scope of a coding interview. See more details [here](https://en.wikipedia.org/wiki/Central_binomial_coefficient).

### Space complexity 

The space complexity of this algorithm will be exponential too, estimated at O(2^n), but the actual will be O*(4*n*/√*n).

## Memoized version

Since our algorithm has overlapping subproblems, can we use memoization to improve it? We could, but every time we return the result of a subproblem from the cache, we have to clone the result list because these trees will be used as the left or right child of a tree. This cloning is equivalent to reconstructing the trees, therefore, the overall time complexity of the memoized algorithm will also be the same.

# Count of Structurally Unique Binary Search Trees (hard) [#](https://www.educative.io/courses/grokking-the-coding-interview/NE1V3EDAnWN#count-of-structurally-unique-binary-search-trees-hard)

Given a number ‘n’, write a function to return the count of structurally unique Binary Search Trees (BST) that can store values 1 to ‘n’.

**Example 1:**

```
Input: 2
Output: 2
Explanation: As we saw in the previous problem, there are 2 unique BSTs storing numbers from 1-2.
```

**Example 2:**

```
Input: 3
Output: 5
Explanation: There will be 5 unique BSTs that can store numbers from 1 to 3.
```

## Solution [#](https://www.educative.io/courses/grokking-the-coding-interview/NE1V3EDAnWN#solution)

This problem is similar to [Structurally Unique Binary Search Trees](https://www.educative.io/collection/page/5668639101419520/5671464854355968/5679974795182080/). Following a similar approach, we can iterate from 1 to ‘n’ and consider each number as the root of a tree and make two recursive calls to count the number of left and right sub-trees.

## Code [#](https://www.educative.io/courses/grokking-the-coding-interview/NE1V3EDAnWN#code)

Here is what our algorithm will look like:

```java
import java.util.*;

class TreeNode {
  int val;
  TreeNode left;
  TreeNode right;

  TreeNode(int x) {
    val = x;
  }
};

class CountUniqueTrees {
  public int countTrees(int n) {
    if (n <= 1)
      return 1;
    int count = 0;
    for (int i = 1; i <= n; i++) {
      // making 'i' root of the tree
      int countOfLeftSubtrees = countTrees(i - 1);
      int countOfRightSubtrees = countTrees(n - i);
      count += (countOfLeftSubtrees * countOfRightSubtrees);
    }
    return count;
  }

  public static void main(String[] args) {
    CountUniqueTrees ct = new CountUniqueTrees();
    int count = ct.countTrees(3);
    System.out.print("Total trees: " + count);
  }
}
```

### Time complexity [#](https://www.educative.io/courses/grokking-the-coding-interview/NE1V3EDAnWN#time-complexity)

The time complexity of this algorithm will be exponential and will be similar to [Balanced Parentheses](https://www.educative.io/collection/page/5668639101419520/5671464854355968/5753264117121024/). Estimated time complexity will be O(n*2^n) but the actual time complexity O(4*n*/√*n*) ) is bounded by the [Catalan number](https://en.wikipedia.org/wiki/Catalan_number) and is beyond the scope of a coding interview. See more details [here](https://en.wikipedia.org/wiki/Central_binomial_coefficient).

### Space complexity [#](https://www.educative.io/courses/grokking-the-coding-interview/NE1V3EDAnWN#space-complexity)

The space complexity of this algorithm will be exponential too, estimated O(2^n) but the actual will be O*(4*n*/√*n).

## Memoized version [#](https://www.educative.io/courses/grokking-the-coding-interview/NE1V3EDAnWN#memoized-version)

Our algorithm has overlapping subproblems as our recursive call will be evaluating the same sub-expression multiple times. To resolve this, we can use memoization and store the intermediate results in a **HashMap**. In each function call, we can check our map to see if we have already evaluated this sub-expression before. Here is the memoized version of our algorithm, please see highlighted changes:`

```java
import java.util.*;

class TreeNode {
  int val;
  TreeNode left;
  TreeNode right;

  TreeNode(int x) {
    val = x;
  }
};

class CountUniqueTrees {
  Map<Integer, Integer> map = new HashMap<>();

  public int countTrees(int n) {
    if (map.containsKey(n))
      return map.get(n);

    if (n <= 1)
      return 1;
    int count = 0;
    for (int i = 1; i <= n; i++) {
      // making 'i' root of the tree
      int countOfLeftSubtrees = countTrees(i - 1);
      int countOfRightSubtrees = countTrees(n - i);
      count += (countOfLeftSubtrees * countOfRightSubtrees);
    }
    map.put(n, count);
    return count;
  }

  public static void main(String[] args) {
    CountUniqueTrees ct = new CountUniqueTrees();
    int count = ct.countTrees(3);
    System.out.print("Total trees: " + count);
  }
}
```

The time complexity of the memoized algorithm will be O(n^2), since we are iterating from ‘1’ to ‘n’ and ensuring that each sub-problem is evaluated only once. The space complexity will be O(n) for the memoization map.

