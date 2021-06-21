# Introduction

This pattern helps us solve problems that involve a list of sorted arrays.

Whenever we are given ‘K’ sorted arrays, we can use a **Heap** to efficiently perform a sorted traversal of all the elements of all arrays. We can push the smallest (first) element of each sorted array in a **Min Heap** to get the overall minimum. While inserting elements to the **Min Heap** we keep track of which array the element came from. We can, then, remove the top element from the heap to get the smallest element and push the next element from the same array, to which this smallest element belonged, to the heap. We can repeat this process to make a sorted traversal of all elements.

Let’s see this pattern in action.

# Merge K Sorted Lists (medium)

## Problem Statement [#](https://www.educative.io/courses/grokking-the-coding-interview/Y5n0n3vAgYK#problem-statement)

Given an array of ‘K’ sorted LinkedLists, merge them into one sorted list.

**Example 1:**

```
Input: L1=[2, 6, 8], L2=[3, 6, 7], L3=[1, 3, 4]
Output: [1, 2, 3, 3, 4, 6, 6, 7, 8]
```

**Example 2:**

```
Input: L1=[5, 8, 9], L2=[1, 7]
Output: [1, 5, 7, 8, 9]
```

## Solution [#](https://www.educative.io/courses/grokking-the-coding-interview/Y5n0n3vAgYK#solution)

A brute force solution could be to add all elements of the given ‘K’ lists to one list and sort it. If there are a total of ‘N’ elements in all the input lists, then the brute force solution will have a time complexity of O(N*logN)*O*(*N*∗*l**o**g**N*) as we will need to sort the merged list. Can we do better than this? How can we utilize the fact that the input lists are individually sorted?

If we have to find the smallest element of all the input lists, we have to compare only the smallest (i.e. the first) element of all the lists. Once we have the smallest element, we can put it in the merged list. Following a similar pattern, we can then find the next smallest element of all the lists to add it to the merged list.

The best data structure that comes to mind to find the smallest number among a set of ‘K’ numbers is a **[Heap](https://en.wikipedia.org/wiki/Heap_(data_structure))**. Let’s see how can we use a heap to find a better algorithm.

1. We can insert the first element of each array in a **Min Heap**.
2. After this, we can take out the smallest (top) element from the heap and add it to the merged list.
3. After removing the smallest element from the heap, we can insert the next element of the same list into the heap.
4. We can repeat steps 2 and 3 to populate the merged list in sorted order.

Let’s take the Example-1 mentioned above to go through each step of our algorithm:

Given lists: `L1=[2, 6, 8]`, `L2=[3, 6, 7]`,` L3=[1, 3, 4]`

1. After inserting the 1st element of each list, the heap will have the following elements:

![image-20210621154428070](/Users/zhengwenxuan/Desktop/Grokking-Series/Grokking-the-Coding-Interview-Patterns-for-Coding-Questions/image-20210621154428070.png)

2. We’ll take the top number from the heap, insert it into the merged list and add the next number in the heap.

![image-20210621154550509](/Users/zhengwenxuan/Desktop/Grokking-Series/Grokking-the-Coding-Interview-Patterns-for-Coding-Questions/image-20210621154550509.png)

3. Again, we’ll take the top element of the heap, insert it into the merged list and add the next number to the heap.

![image-20210621154608003](/Users/zhengwenxuan/Desktop/Grokking-Series/Grokking-the-Coding-Interview-Patterns-for-Coding-Questions/image-20210621154608003.png)

4. Repeating the above step, take the top element of the heap, insert it into the merged list and add the next number to the heap. As there are two 3s in the heap, we can pick anyone but we need to take the next element from the corresponding list to insert in the heap.

![image-20210621154624923](/Users/zhengwenxuan/Desktop/Grokking-Series/Grokking-the-Coding-Interview-Patterns-for-Coding-Questions/image-20210621154624923.png)

We’ll repeat the above step to populate our merged array.

## Code [#](https://www.educative.io/courses/grokking-the-coding-interview/Y5n0n3vAgYK#code)

Here is what our algorithm will look like:

```java
import java.util.*;

class ListNode {
  int value;
  ListNode next;

  ListNode(int value) {
    this.value = value;
  }
}

class MergeKSortedLists {

  public static ListNode merge(ListNode[] lists) {
    PriorityQueue<ListNode> minHeap = new PriorityQueue<ListNode>((n1, n2) -> n1.value - n2.value);

    // put the root of each list in the min heap
    for (ListNode root : lists)
      if (root != null)
        minHeap.add(root);

    // take the smallest (top) element form the min-heap and add it to the result; 
    // if the top element has a next element add it to the heap
    ListNode resultHead = null, resultTail = null;
    while (!minHeap.isEmpty()) {
      ListNode node = minHeap.poll();
      if (resultHead == null) {
        resultHead = resultTail = node;
      } else {
        resultTail.next = node;
        resultTail = resultTail.next;
      }
      if (node.next != null)
        minHeap.add(node.next);
    }

    return resultHead;
  }

  public static void main(String[] args) {
    ListNode l1 = new ListNode(2);
    l1.next = new ListNode(6);
    l1.next.next = new ListNode(8);

    ListNode l2 = new ListNode(3);
    l2.next = new ListNode(6);
    l2.next.next = new ListNode(7);

    ListNode l3 = new ListNode(1);
    l3.next = new ListNode(3);
    l3.next.next = new ListNode(4);

    ListNode result = MergeKSortedLists.merge(new ListNode[] { l1, l2, l3 });
    System.out.print("Here are the elements form the merged list: ");
    while (result != null) {
      System.out.print(result.value + " ");
      result = result.next;
    }
  }
}
```

### Time complexity [#](https://www.educative.io/courses/grokking-the-coding-interview/Y5n0n3vAgYK#time-complexity)

Since we’ll be going through all the elements of all arrays and will be removing/adding one element to the heap in each step, the time complexity of the above algorithm will be O(N*logK), where ‘N’ is the total number of elements in all the ‘K’ input arrays.

### Space complexity [#](https://www.educative.io/courses/grokking-the-coding-interview/Y5n0n3vAgYK#space-complexity)

The space complexity will be O(K) because, at any time, our min-heap will be storing one number from all the ‘K’ input arrays.

# Kth Smallest Number in M Sorted Lists (Medium)

## Problem Statement [#](https://www.educative.io/courses/grokking-the-coding-interview/myAqDMyRXn3#problem-statement)

Given ‘M’ sorted arrays, find the K’th smallest number among all the arrays.

**Example 1:**

```
Input: L1=[2, 6, 8], L2=[3, 6, 7], L3=[1, 3, 4], K=5
Output: 4
Explanation: The 5th smallest number among all the arrays is 4, this can be verified from 
the merged list of all the arrays: [1, 2, 3, 3, 4, 6, 6, 7, 8]
```

**Example 2:**

```
Input: L1=[5, 8, 9], L2=[1, 7], K=3
Output: 7
Explanation: The 3rd smallest number among all the arrays is 7.
```

## Solution [#](https://www.educative.io/courses/grokking-the-coding-interview/myAqDMyRXn3#solution)

This problem follows the **K-way merge** pattern and we can follow a similar approach as discussed in [Merge K Sorted Lists](https://www.educative.io/collection/page/5668639101419520/5671464854355968/4611799594827776/).

We can start merging all the arrays, but instead of inserting numbers into a merged list, we will keep count to see how many elements have been inserted in the merged list. Once that count is equal to ‘K’, we have found our required number.

A big difference from [Merge K Sorted Lists](https://www.educative.io/collection/page/5668639101419520/5671464854355968/4611799594827776/) is that in this problem, the input is a list of arrays compared to LinkedLists. This means that when we want to push the next number in the heap we need to know what the index of the current number in the current array was. To handle this, we will need to keep track of the array and the element indices.

## Code [#](https://www.educative.io/courses/grokking-the-coding-interview/myAqDMyRXn3#code)

Here is what our algorithm will look like:

```java
import java.util.*;

class Node {
  int elementIndex;
  int arrayIndex;

  Node(int elementIndex, int arrayIndex) {
    this.elementIndex = elementIndex;
    this.arrayIndex = arrayIndex;
  }
}

class KthSmallestInMSortedArrays {

  public static int findKthSmallest(List<Integer[]> lists, int k) {
    PriorityQueue<Node> minHeap = new PriorityQueue<Node>(
        (n1, n2) -> lists.get(n1.arrayIndex)[n1.elementIndex] - lists.get(n2.arrayIndex)[n2.elementIndex]);

    // put the 1st element of each array in the min heap
    for (int i = 0; i < lists.size(); i++)
      if (lists.get(i) != null)
        minHeap.add(new Node(0, i));

    // take the smallest (top) element form the min heap, if the running count is equal to k return the number
    // if the array of the top element has more elements, add the next element to the heap
    int numberCount = 0, result = 0;
    while (!minHeap.isEmpty()) {
      Node node = minHeap.poll();
      result = lists.get(node.arrayIndex)[node.elementIndex];
      if (++numberCount == k)
        break;
      node.elementIndex++;
      if (lists.get(node.arrayIndex).length > node.elementIndex)
        minHeap.add(node);
    }
    return result;
  }

  public static void main(String[] args) {
    Integer[] l1 = new Integer[] { 2, 6, 8 };
    Integer[] l2 = new Integer[] { 3, 6, 7 };
    Integer[] l3 = new Integer[] { 1, 3, 4 };
    List<Integer[]> lists = new ArrayList<Integer[]>();
    lists.add(l1);
    lists.add(l2);
    lists.add(l3);
    int result = KthSmallestInMSortedArrays.findKthSmallest(lists, 5);
    System.out.print("Kth smallest number is: " + result);
  }
}
```

### Time complexity [#](https://www.educative.io/courses/grokking-the-coding-interview/myAqDMyRXn3#time-complexity)

Since we’ll be going through at most ‘K’ elements among all the arrays, and we will remove/add one element in the heap in each step, the time complexity of the above algorithm will be O(K*logM) where ‘M’ is the total number of input arrays.

### Space complexity [#](https://www.educative.io/courses/grokking-the-coding-interview/myAqDMyRXn3#space-complexity)

The space complexity will be O(M) because, at any time, our min-heap will be storing one number from all the ‘M’ input arrays.

------

## Similar Problems [#](https://www.educative.io/courses/grokking-the-coding-interview/myAqDMyRXn3#similar-problems)

**Problem 1:** Given ‘M’ sorted arrays, find the median number among all arrays.

**Solution:** This problem is similar to our parent problem with K=Median. So if there are ‘N’ total numbers in all the arrays we need to find the K’th minimum number where K=N/2.

**Problem 2:** Given a list of ‘K’ sorted arrays, merge them into one sorted list.

**Solution:** This problem is similar to [Merge K Sorted Lists](https://www.educative.io/collection/page/5668639101419520/5671464854355968/4611799594827776/) except that the input is a list of arrays compared to **LinkedLists**. To handle this, we can use a similar approach as discussed in our parent problem by keeping a track of the array and the element indices.

# Kth Smallest Number in a Sorted Matrix (Hard)

## Problem Statement [#](https://www.educative.io/courses/grokking-the-coding-interview/x1NJVYKNvqz#problem-statement)

Given an N * N*N*∗*N* matrix where each row and column is sorted in ascending order, find the Kth smallest element in the matrix.

**Example 1:**

```
Input: Matrix=[
    [2, 6, 8], 
    [3, 7, 10],
    [5, 8, 11]
  ], 
  K=5
Output: 7
Explanation: The 5th smallest number in the matrix is 7.
```

## Solution [#](https://www.educative.io/courses/grokking-the-coding-interview/x1NJVYKNvqz#solution)

This problem follows the **K-way merge** pattern and can be easily converted to [Kth Smallest Number in M Sorted Lists](https://www.educative.io/collection/page/5668639101419520/5671464854355968/4890648534581248/). As each row (or column) of the given matrix can be seen as a sorted list, we essentially need to find the Kth smallest number in ‘N’ sorted lists.

## Code [#](https://www.educative.io/courses/grokking-the-coding-interview/x1NJVYKNvqz#code)

Here is what our algorithm will look like:

```java
import java.util.*;

class Node {
  int row;
  int col;

  Node(int row, int col) {
    this.row = row;
    this.col = col;
  }
}

class KthSmallestInSortedMatrix {

  public static int findKthSmallest(int[][] matrix, int k) {
    PriorityQueue<Node> minHeap = new PriorityQueue<Node>((n1, n2) -> matrix[n1.row][n1.col] - matrix[n2.row][n2.col]);

    // put the 1st element of each row in the min heap
    // we don't need to push more than 'k' elements in the heap
    for (int i = 0; i < matrix.length && i < k; i++)
      minHeap.add(new Node(i, 0));

    // take the smallest (top) element form the min heap, if the running count is equal to k return the number
    // if the row of the top element has more elements, add the next element to the heap
    int numberCount = 0, result = 0;
    while (!minHeap.isEmpty()) {
      Node node = minHeap.poll();
      result = matrix[node.row][node.col];
      if (++numberCount == k)
        break;
      node.col++;
      if (matrix[0].length > node.col)
        minHeap.add(node);
    }
    return result;
  }

  public static void main(String[] args) {
    int matrix[][] = { { 2, 6, 8 }, { 3, 7, 10 }, { 5, 8, 11 } };
    int result = KthSmallestInSortedMatrix.findKthSmallest(matrix, 5);
    System.out.print("Kth smallest number is: " + result);
  }
}
```

### Time complexity [#](https://www.educative.io/courses/grokking-the-coding-interview/x1NJVYKNvqz#time-complexity)

First, we inserted at most ‘K’ or one element from each of the ‘N’ rows, which will take O(min(K, N))*O*(*m**i**n*(*K*,*N*)). Then we went through at most ‘K’ elements in the matrix and remove/add one element in the heap in each step. As we can’t have more than ‘N’ elements in the heap in any condition, therefore, the overall time complexity of the above algorithm will be O(min(K, N) + K*logN)

### Space complexity [#](https://www.educative.io/courses/grokking-the-coding-interview/x1NJVYKNvqz#space-complexity)

The space complexity will be O(N) because, in the worst case, our min-heap will be storing one number from each of the ‘N’ rows.

## An Alternate Solution [#](https://www.educative.io/courses/grokking-the-coding-interview/x1NJVYKNvqz#an-alternate-solution)

Since each row and column of the matrix is sorted, is it possible to use **Binary Search** to find the Kth smallest number?

The biggest problem to use **Binary Search**, in this case, is that we don’t have a straightforward sorted array, instead, we have a matrix. As we remember, in **Binary Search**, we calculate the middle index of the search space (‘1’ to ‘N’) and see if our required number is pointed out by the middle index; if not we either search in the lower half or the upper half. In a sorted matrix, we can’t really find a middle. Even if we do consider some index as middle, it is not straightforward to find the search space containing numbers bigger or smaller than the number pointed out by the middle index.

An alternative could be to apply the **Binary Search** on the “number range” instead of the “index range”. As we know that the smallest number of our matrix is at the top left corner and the biggest number is at the bottom right corner. These two numbers can represent the “range” i.e., the `start` and the `end` for the **Binary Search**. Here is how our algorithm will work:

1. Start the **Binary Search** with `start = matrix[0][0]` and `end = matrix[n-1][n-1]`.
2. Find `middle` of the `start` and the `end`. This `middle` number is NOT necessarily an element in the matrix.
3. Count all the numbers smaller than or equal to `middle` in the matrix. As the matrix is sorted, we can do this in O(N).
4. While counting, we can keep track of the “smallest number greater than the `middle`” (let’s call it `n1`) and at the same time the “biggest number less than or equal to the `middle`” (let’s call it `n2`). These two numbers will be used to adjust the “number range” for the **Binary Search** in the next iteration.
5. If the count is equal to ‘K’, `n1` will be our required number as it is the “biggest number less than or equal to the `middle`”, and is definitely present in the matrix.
6. If the count is less than ‘K’, we can update `start = n2` to search in the higher part of the matrix and if the count is greater than ‘K’, we can update `end = n1` to search in the lower part of the matrix in the next iteration.

Here is the visual representation of our algorithm:

![image-20210621155022591](/Users/zhengwenxuan/Desktop/Grokking-Series/Grokking-the-Coding-Interview-Patterns-for-Coding-Questions/image-20210621155022591.png)

Here is what our algorithm will look like:

```java
class KthSmallestInSortedMatrix {
  public static int findKthSmallest(int[][] matrix, int k) {
    int n = matrix.length;
    int start = matrix[0][0], end = matrix[n - 1][n - 1];
    while (start < end) {
      int mid = start + (end - start) / 2;
      // first number is the smallest and the second number is the largest
      int[] smallLargePair = { matrix[0][0], matrix[n - 1][n - 1] };

      int count = countLessEqual(matrix, mid, smallLargePair);

      if (count == k)
        return smallLargePair[0];

      if (count < k)
        start = smallLargePair[1]; // search higher
      else
        end = smallLargePair[0]; // search lower
    }
 
    return start;
  }

  private static int countLessEqual(int[][] matrix, int mid, int[] smallLargePair) {
    int count = 0;
    int n = matrix.length, row = n - 1, col = 0;
    while (row >= 0 && col < n) {
      if (matrix[row][col] > mid) {
        // as matrix[row][col] is bigger than the mid, let's keep track of the
        // smallest number greater than the mid
        smallLargePair[1] = Math.min(smallLargePair[1], matrix[row][col]);
        row--;
      } else {
        // as matrix[row][col] is less than or equal to the mid, let's keep track of the
        // biggest number less than or equal to the mid
        smallLargePair[0] = Math.max(smallLargePair[0], matrix[row][col]);
        count += row + 1;
        col++;
      }
    }
    return count;
  }

  public static void main(String[] args) {
    int matrix[][] = { { 1, 4 }, { 2, 5 } };
    int result = KthSmallestInSortedMatrix.findKthSmallest(matrix, 2);
    System.out.println("Kth smallest number is: " + result);

    int matrix1[][] = { { -5 } };
    result = KthSmallestInSortedMatrix.findKthSmallest(matrix1, 1);
    System.out.println("Kth smallest number is: " + result);

    int matrix2[][] = { { 2, 6, 8 }, { 3, 7, 10 }, { 5, 8, 11 } };
    result = KthSmallestInSortedMatrix.findKthSmallest(matrix2, 5);
    System.out.println("Kth smallest number is: " + result);

    int matrix3[][] = { { 1, 5, 9 }, { 10, 11, 13 }, { 12, 13, 15 } };
    result = KthSmallestInSortedMatrix.findKthSmallest(matrix3, 8);
    System.out.println("Kth smallest number is: " + result);

  }
}
```

### Time complexity [#](https://www.educative.io/courses/grokking-the-coding-interview/x1NJVYKNvqz#time-complexity-2)

The **Binary Search** could take O(log(max-min )) iterations where ‘max’ is the largest and ‘min’ is the smallest element in the matrix and in each iteration we take O(N) for counting, therefore, the overall time complexity of the algorithm will be O(N*log(max-min).

### Space complexity [#](https://www.educative.io/courses/grokking-the-coding-interview/x1NJVYKNvqz#space-complexity-2)

The algorithm runs in constant space O(1).

# Smallest Number Range (Hard)

## Problem Statement [#](https://www.educative.io/courses/grokking-the-coding-interview/JPGWDNRx3w2#problem-statement)

Given ‘M’ sorted arrays, find the smallest range that includes at least one number from each of the ‘M’ lists.

**Example 1:**

```
Input: L1=[1, 5, 8], L2=[4, 12], L3=[7, 8, 10]
Output: [4, 7]
Explanation: The range [4, 7] includes 5 from L1, 4 from L2 and 7 from L3.
```

**Example 2:**

```
Input: L1=[1, 9], L2=[4, 12], L3=[7, 10, 16]
Output: [9, 12]
Explanation: The range [9, 12] includes 9 from L1, 12 from L2 and 10 from L3.
```

## Solution [#](https://www.educative.io/courses/grokking-the-coding-interview/JPGWDNRx3w2#solution)

This problem follows the **K-way merge** pattern and we can follow a similar approach as discussed in [Merge K Sorted Lists](https://www.educative.io/collection/page/5668639101419520/5671464854355968/4611799594827776/).

We can start by inserting the first number from all the arrays in a min-heap. We will keep track of the largest number that we have inserted in the heap (let’s call it `currentMaxNumber`).

In a loop, we’ll take the smallest (top) element from the min-heap and`currentMaxNumber` has the largest element that we inserted in the heap. If these two numbers give us a smaller range, we’ll update our range. Finally, if the array of the top element has more elements, we’ll insert the next element to the heap.

We can finish searching the minimum range as soon as an array is completed or, in other terms, the heap has less than ‘M’ elements.

## Code [#](https://www.educative.io/courses/grokking-the-coding-interview/JPGWDNRx3w2#code)

Here is what our algorithm will look like:

```java
import java.util.*;

class Node {
  int elementIndex;
  int arrayIndex;

  Node(int elementIndex, int arrayIndex) {
    this.elementIndex = elementIndex;
    this.arrayIndex = arrayIndex;
  }
}

class SmallestRange {

  public static int[] findSmallestRange(List<Integer[]> lists) {
    PriorityQueue<Node> minHeap = new PriorityQueue<Node>(
        (n1, n2) -> lists.get(n1.arrayIndex)[n1.elementIndex] - lists.get(n2.arrayIndex)[n2.elementIndex]);

    int rangeStart = 0, rangeEnd = Integer.MAX_VALUE, currentMaxNumber = Integer.MIN_VALUE;
    // put the 1st element of each array in the min heap
    for (int i = 0; i < lists.size(); i++)
      if (lists.get(i) != null) {
        minHeap.add(new Node(0, i));
        currentMaxNumber = Math.max(currentMaxNumber, lists.get(i)[0]);
      }

    // take the smallest (top) element form the min heap, if it gives us smaller range, update the ranges
    // if the array of the top element has more elements, insert the next element in the heap
    while (minHeap.size() == lists.size()) {
      Node node = minHeap.poll();
      if (rangeEnd - rangeStart > currentMaxNumber - lists.get(node.arrayIndex)[node.elementIndex]) {
        rangeStart = lists.get(node.arrayIndex)[node.elementIndex];
        rangeEnd = currentMaxNumber;
      }
      node.elementIndex++;
      if (lists.get(node.arrayIndex).length > node.elementIndex) {
        minHeap.add(node); // insert the next element in the heap
        currentMaxNumber = Math.max(currentMaxNumber, lists.get(node.arrayIndex)[node.elementIndex]);
      }
    }
    return new int[] { rangeStart, rangeEnd };
  }

  public static void main(String[] args) {
    Integer[] l1 = new Integer[] { 1, 5, 8 };
    Integer[] l2 = new Integer[] { 4, 12 };
    Integer[] l3 = new Integer[] { 7, 8, 10 };
    List<Integer[]> lists = new ArrayList<Integer[]>();
    lists.add(l1);
    lists.add(l2);
    lists.add(l3);
    int[] result = SmallestRange.findSmallestRange(lists);
    System.out.print("Smallest range is: [" + result[0] + ", " + result[1] + "]");
  }
}
```

### Time complexity [#](https://www.educative.io/courses/grokking-the-coding-interview/JPGWDNRx3w2#time-complexity)

Since, at most, we’ll be going through all the elements of all the arrays and will remove/add one element in the heap in each step, the time complexity of the above algorithm will be O(N*logM) where ‘N’ is the total number of elements in all the ‘M’ input arrays.

### Space complexity [#](https://www.educative.io/courses/grokking-the-coding-interview/JPGWDNRx3w2#space-complexity)

The space complexity will be O(M) because, at any time, our min-heap will be store one number from all the ‘M’ input arrays.

# K Pairs with Largest Sums (Hard) [#](https://www.educative.io/courses/grokking-the-coding-interview/B1JKxRB8EDJ#k-pairs-with-largest-sums-hard)

Given two sorted arrays in descending order, find ‘K’ pairs with the largest sum where each pair consists of numbers from both the arrays.

**Example 1:**

```
Input: L1=[9, 8, 2], L2=[6, 3, 1], K=3
Output: [9, 3], [9, 6], [8, 6] 
Explanation: These 3 pairs have the largest sum. No other pair has a sum larger than any of these.
```

**Example 2:**

```
Input: L1=[5, 2, 1], L2=[2, -1], K=3
Output: [5, 2], [5, -1], [2, 2] 
```

## Solution [#](https://www.educative.io/courses/grokking-the-coding-interview/N767x7VoYmK#solution)

This problem follows the **K-way merge** pattern and we can follow a similar approach as discussed in [Merge K Sorted Lists](https://www.educative.io/collection/page/5668639101419520/5671464854355968/4611799594827776/).

We can go through all the numbers of the two input arrays to create pairs and initially insert them all in the heap until we have ‘K’ pairs in **Min Heap**. After that, if a pair is bigger than the top (smallest) pair in the heap, we can remove the smallest pair and insert this pair in the heap.

We can optimize our algorithms in two ways:

1. Instead of iterating over all the numbers of both arrays, we can iterate only the first ‘K’ numbers from both arrays. Since the arrays are sorted in descending order, the pairs with the maximum sum will be constituted by the first ‘K’ numbers from both the arrays.
2. As soon as we encounter a pair with a sum that is smaller than the smallest (top) element of the heap, we don’t need to process the next elements of the array. Since the arrays are sorted in descending order, we won’t be able to find a pair with a higher sum moving forward.

## Code [#](https://www.educative.io/courses/grokking-the-coding-interview/N767x7VoYmK#code)

Here is what our algorithm will look like:

```java
import java.util.*;

class LargestPairs {

  public static List<int[]> findKLargestPairs(int[] nums1, int[] nums2, int k) {
    PriorityQueue<int[]> minHeap = new PriorityQueue<int[]>((p1, p2) -> (p1[0] + p1[1]) - (p2[0] + p2[1]));
    for (int i = 0; i < nums1.length && i < k; i++) {
      for (int j = 0; j < nums2.length && j < k; j++) {
        if (minHeap.size() < k) {
          minHeap.add(new int[] { nums1[i], nums2[j] });
        } else {
          // if the sum of the two numbers from the two arrays is smaller than the smallest (top) element of
          // the heap, we can 'break' here. Since the arrays are sorted in the descending order, we'll not be
          // able to find a pair with a higher sum moving forward.
          if (nums1[i] + nums2[j] < minHeap.peek()[0] + minHeap.peek()[1]) {
            break;
          } else { // else: we have a pair with a larger sum, remove top and insert this pair in the heap
            minHeap.poll();
            minHeap.add(new int[] { nums1[i], nums2[j] });
          }
        }
      }
    }
    return new ArrayList<>(minHeap);
  }

  public static void main(String[] args) {
    int[] l1 = new int[] { 9, 8, 2 };
    int[] l2 = new int[] { 6, 3, 1 };
    List<int[]> result = LargestPairs.findKLargestPairs(l1, l2, 3);
    System.out.print("Pairs with largest sum are: ");
    for (int[] pair : result)
      System.out.print("[" + pair[0] + ", " + pair[1] + "] ");
  }
}
```

### Time complexity [#](https://www.educative.io/courses/grokking-the-coding-interview/N767x7VoYmK#time-complexity)

Since, at most, we’ll be going through all the elements of both arrays and we will add/remove one element in the heap in each step, the time complexity of the above algorithm will be O(N*M*logK) where ‘N’ and ‘M’ are the total number of elements in both arrays, respectively.

If we assume that both arrays have at least ‘K’ elements then the time complexity can be simplified to O(K^2logK), because we are not iterating more than ‘K’ elements in both arrays.

### Space complexity [#](https://www.educative.io/courses/grokking-the-coding-interview/N767x7VoYmK#space-complexity)

The space complexity will be O(K) because, at any time, our **Min Heap** will be storing ‘K’ largest pairs.

