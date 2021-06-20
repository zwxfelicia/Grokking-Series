# Introduction

In many problems, where we are given a set of elements such that we can divide them into two parts. To solve the problem, we are interested in knowing the smallest element in one part and the biggest element in the other part. This pattern is an efficient approach to solve such problems.

This pattern uses two **Heaps** to solve these problems; A **Min Heap** to find the smallest element and a **Max Heap** to find the biggest element.

Let’s jump onto our first problem to see this pattern in action.

# Find the Median of a Number Stream (medium)

## Problem Statement [#](https://www.educative.io/courses/grokking-the-coding-interview/3Yj2BmpyEy4#problem-statement)

Design a class to calculate the median of a number stream. The class should have the following two methods:

1. `insertNum(int num)`: stores the number in the class
2. `findMedian()`: returns the median of all numbers inserted in the class

If the count of numbers inserted in the class is even, the median will be the average of the middle two numbers.

**Example 1:**

```
1. insertNum(3)
2. insertNum(1)
3. findMedian() -> output: 2
4. insertNum(5)
5. findMedian() -> output: 3
6. insertNum(4)
7. findMedian() -> output: 3.5
```

## Solution [#](https://www.educative.io/courses/grokking-the-coding-interview/3Yj2BmpyEy4#solution)

As we know, the median is the middle value in an ordered integer list. So a brute force solution could be to maintain a sorted list of all numbers inserted in the class so that we can efficiently return the median whenever required. Inserting a number in a sorted list will take O(N)*O*(*N*) time if there are ‘N’ numbers in the list. This insertion will be similar to the [Insertion sort](https://en.wikipedia.org/wiki/Insertion_sort). Can we do better than this? Can we utilize the fact that we don’t need the fully sorted list - we are only interested in finding the middle element?

Assume ‘x’ is the median of a list. This means that half of the numbers in the list will be smaller than (or equal to) ‘x’ and half will be greater than (or equal to) ‘x’. This leads us to an approach where we can divide the list into two halves: one half to store all the smaller numbers (let’s call it `smallNumList`) and one half to store the larger numbers (let’s call it `largNumList`). The median of all the numbers will either be the largest number in the `smallNumList` or the smallest number in the `largNumList`. If the total number of elements is even, the median will be the average of these two numbers.

The best data structure that comes to mind to find the smallest or largest number among a list of numbers is a [Heap](https://en.wikipedia.org/wiki/Heap_(data_structure)). Let’s see how we can use a heap to find a better algorithm.

1. We can store the first half of numbers (i.e., `smallNumList`) in a **Max Heap**. We should use a **Max Heap** as we are interested in knowing the largest number in the first half.
2. We can store the second half of numbers (i.e., `largeNumList`) in a **Min Heap**, as we are interested in knowing the smallest number in the second half.
3. Inserting a number in a heap will take O(logN)*O*(*l**o**g**N*), which is better than the brute force approach.
4. At any time, the median of the current list of numbers can be calculated from the top element of the two heaps.

Let’s take the Example-1 mentioned above to go through each step of our algorithm:

1. `insertNum(3)`: We can insert a number in the **Max Heap** (i.e. first half) if the number is smaller than the top (largest) number of the heap. After every insertion, we will balance the number of elements in both heaps, so that they have an equal number of elements. If the count of numbers is odd, let’s decide to have more numbers in max-heap than the **Min Heap**.

   ![image-20210620162920524](/Users/zhengwenxuan/Desktop/Grokking-Series/Grokking-the-Coding-Interview-Patterns-for-Coding-Questions/image-20210620162920524.png)

2. `insertNum(1)`: As ‘1’ is smaller than ‘3’, let’s insert it into the **Max Heap**.

![image-20210620162943747](/Users/zhengwenxuan/Desktop/Grokking-Series/Grokking-the-Coding-Interview-Patterns-for-Coding-Questions/image-20210620162943747.png)

Now, we have two elements in the **Max Heap** and no elements in **Min Heap**. Let’s take the largest element from the **Max Heap** and insert it into the **Min Heap**, to balance the number of elements in both heaps.

3. `findMedian()`: As we have an even number of elements, the median will be the average of the top element of both the heaps -> (1+3)/2 = 2.0(1+3)/2=2.0

4. `insertNum(5)`: As ‘5’ is greater than the top element of the **Max Heap**, we can insert it into the **Min Heap**. After the insertion, the total count of elements will be odd. As we had decided to have more numbers in the **Max Heap** than the **Min Heap**, we can take the top (smallest) number from the **Min Heap** and insert it into the **Max Heap**.

![image-20210620163007576](/Users/zhengwenxuan/Desktop/Grokking-Series/Grokking-the-Coding-Interview-Patterns-for-Coding-Questions/image-20210620163007576.png)

5. `findMedian()`: Since we have an odd number of elements, the median will be the top element of **Max Heap** -> 3. An odd number of elements also means that the **Max Heap** will have one extra element than the **Min Heap**.
6. `insertNum(4)`: Insert ‘4’ into **Min Heap**.

![image-20210620163028084](/Users/zhengwenxuan/Desktop/Grokking-Series/Grokking-the-Coding-Interview-Patterns-for-Coding-Questions/image-20210620163028084.png)

7. `findMedian()`: As we have an even number of elements, the median will be the average of the top element of both the heaps -> (3+4)/2 = 3.5(3+4)/2=3.5

   

   

   

## Code 

Here is what our algorithm will look like:

```java
import java.util.*;

class MedianOfAStream {

  PriorityQueue<Integer> maxHeap; //containing first half of numbers
  PriorityQueue<Integer> minHeap; //containing second half of numbers

  public MedianOfAStream() {
    maxHeap = new PriorityQueue<>((a, b) -> b - a);
    minHeap = new PriorityQueue<>((a, b) -> a - b);
  }

  public void insertNum(int num) {
    if (maxHeap.isEmpty() || maxHeap.peek() >= num)
      maxHeap.add(num);
    else
      minHeap.add(num);

    // either both the heaps will have equal number of elements or max-heap will have one 
    // more element than the min-heap
    if (maxHeap.size() > minHeap.size() + 1)
      minHeap.add(maxHeap.poll());
    else if (maxHeap.size() < minHeap.size())
      maxHeap.add(minHeap.poll());
  }

  public double findMedian() {
    if (maxHeap.size() == minHeap.size()) {
      // we have even number of elements, take the average of middle two elements
      return maxHeap.peek() / 2.0 + minHeap.peek() / 2.0;
    }
    // because max-heap will have one more element than the min-heap
    return maxHeap.peek();
  }

  public static void main(String[] args) {
    MedianOfAStream medianOfAStream = new MedianOfAStream();
    medianOfAStream.insertNum(3);
    medianOfAStream.insertNum(1);
    System.out.println("The median is: " + medianOfAStream.findMedian());
    medianOfAStream.insertNum(5);
    System.out.println("The median is: " + medianOfAStream.findMedian());
    medianOfAStream.insertNum(4);
    System.out.println("The median is: " + medianOfAStream.findMedian());
  }
}
```

### Time complexity [#](https://www.educative.io/courses/grokking-the-coding-interview/3Yj2BmpyEy4#time-complexity)

The time complexity of the `insertNum()` will be O(logN) due to the insertion in the heap. The time complexity of the `findMedian()` will be O(1) as we can find the median from the top elements of the heaps.

### Space complexity [#](https://www.educative.io/courses/grokking-the-coding-interview/3Yj2BmpyEy4#space-complexity)

The space complexity will be O(N) because, as at any time, we will be storing all the numbers.

# Sliding Window Median (hard)

## Problem Statement [#](https://www.educative.io/courses/grokking-the-coding-interview/3Y9jm7XRrXO#problem-statement)

Given an array of numbers and a number ‘k’, find the median of all the ‘k’ sized sub-arrays (or windows) of the array.

**Example 1:**

Input: nums=[1, 2, -1, 3, 5], k = 2
Output: [1.5, 0.5, 1.0, 4.0]
Explanation: Lets consider all windows of size ‘2’:

- [1, 2, -1, 3, 5] -> median is 1.5
- [1, 2, -1, 3, 5] -> median is 0.5
- [1, 2, -1, 3, 5] -> median is 1.0
- [1, 2, -1, 3, 5] -> median is 4.0

**Example 2:**

Input: nums=[1, 2, -1, 3, 5], k = 3
Output: [1.0, 2.0, 3.0]
Explanation: Lets consider all windows of size ‘3’:

- [1, 2, -1, 3, 5] -> median is 1.0
- [1, 2, -1, 3, 5] -> median is 2.0
- [1, 2, -1, 3, 5] -> median is 3.0

## Solution [#](https://www.educative.io/courses/grokking-the-coding-interview/3Y9jm7XRrXO#solution)

This problem follows the **Two Heaps** pattern and share similarities with [Find the Median of a Number Stream](https://www.educative.io/collection/page/5668639101419520/5671464854355968/6308926461050880/). We can follow a similar approach of maintaining a max-heap and a min-heap for the list of numbers to find their median.

The only difference is that we need to keep track of a sliding window of ‘k’ numbers. This means, in each iteration, when we insert a new number in the heaps, we need to remove one number from the heaps which is going out of the sliding window. After the removal, we need to rebalance the heaps in the same way that we did while inserting.

### Code 

Here is what our algorithm will look like:

```java
import java.util.*;

class SlidingWindowMedian {
  PriorityQueue<Integer> maxHeap = new PriorityQueue<>(Collections.reverseOrder());
  PriorityQueue<Integer> minHeap = new PriorityQueue<>();

  public double[] findSlidingWindowMedian(int[] nums, int k) {
    double[] result = new double[nums.length - k + 1];
    for (int i = 0; i < nums.length; i++) {
      if (maxHeap.size() == 0 || maxHeap.peek() >= nums[i]) {
        maxHeap.add(nums[i]);
      } else {
        minHeap.add(nums[i]);
      }
      rebalanceHeaps();

      if (i - k + 1 >= 0) { // if we have at least 'k' elements in the sliding window
        // add the median to the the result array
        if (maxHeap.size() == minHeap.size()) {
          // we have even number of elements, take the average of middle two elements
          result[i - k + 1] = maxHeap.peek() / 2.0 + minHeap.peek() / 2.0;
        } else { // because max-heap will have one more element than the min-heap
          result[i - k + 1] = maxHeap.peek();
        }

        // remove the element going out of the sliding window
        int elementToBeRemoved = nums[i - k + 1];
        if (elementToBeRemoved <= maxHeap.peek()) {
          maxHeap.remove(elementToBeRemoved);
        } else {
          minHeap.remove(elementToBeRemoved);
        }
        rebalanceHeaps();
      }
    }
    return result;
  }

  private void rebalanceHeaps() {
    // either both the heaps will have equal number of elements or max-heap will have 
    // one more element than the min-heap
    if (maxHeap.size() > minHeap.size() + 1)
      minHeap.add(maxHeap.poll());
    else if (maxHeap.size() < minHeap.size())
      maxHeap.add(minHeap.poll());
  }

  public static void main(String[] args) {
    SlidingWindowMedian slidingWindowMedian = new SlidingWindowMedian();
    double[] result = slidingWindowMedian.findSlidingWindowMedian(new int[] { 1, 2, -1, 3, 5 }, 2);
    System.out.print("Sliding window medians are: ");
    for (double num : result)
      System.out.print(num + " ");
    System.out.println();

    slidingWindowMedian = new SlidingWindowMedian();
    result = slidingWindowMedian.findSlidingWindowMedian(new int[] { 1, 2, -1, 3, 5 }, 3);
    System.out.print("Sliding window medians are: ");
    for (double num : result)
      System.out.print(num + " ");
  }

}
```

### Time complexity [#](https://www.educative.io/courses/grokking-the-coding-interview/3Y9jm7XRrXO#time-complexity)

The time complexity of our algorithm is O(N*K) where ‘N’ is the total number of elements in the input array and ‘K’ is the size of the sliding window. This is due to the fact that we are going through all the ‘N’ numbers and, while doing so, we are doing two things:

1. Inserting/removing numbers from heaps of size ‘K’. This will take O(logK)
2. Removing the element going out of the sliding window. This will take O(K)*O*(*K*) as we will be searching this element in an array of size ‘K’ (i.e., a heap).

### Space complexity [#](https://www.educative.io/courses/grokking-the-coding-interview/3Y9jm7XRrXO#space-complexity)

Ignoring the space needed for the output array, the space complexity will be O(K) because, at any time, we will be storing all the numbers within the sliding window.

# Maximize Capital (hard)

## Problem Statement [#](https://www.educative.io/courses/grokking-the-coding-interview/B6x69OLX4jY#problem-statement)

Given a set of investment projects with their respective profits, we need to find the **most profitable projects**. We are given an initial capital and are allowed to invest only in a fixed number of projects. Our goal is to choose projects that give us the maximum profit. Write a function that returns the maximum total capital after selecting the most profitable projects.

We can start an investment project only when we have the required capital. Once a project is selected, we can assume that its profit has become our capital.

**Example 1:**

**Input:** Project Capitals=[0,1,2], Project Profits=[1,2,3], Initial Capital=1, Number of Projects=2
**Output:** 6
**Explanation:**

1. With initial capital of ‘1’, we will start the second project which will give us profit of ‘2’. Once we selected our first project, our total capital will become 3 (profit + initial capital).
2. With ‘3’ capital, we will select the third project, which will give us ‘3’ profit.

After the completion of the two projects, our total capital will be 6 (1+2+3).

**Example 2:**

**Input:** Project Capitals=[0,1,2,3], Project Profits=[1,2,3,5], Initial Capital=0, Number of Projects=3
**Output:** 8
**Explanation:**

1. With ‘0’ capital, we can only select the first project, bringing out capital to 1.
2. Next, we will select the second project, which will bring our capital to 3.
3. Next, we will select the fourth project, giving us a profit of 5.

After selecting the three projects, our total capital will be 8 (1+2+5).

## Solution [#](https://www.educative.io/courses/grokking-the-coding-interview/B6x69OLX4jY#solution)

While selecting projects we have two constraints:

1. We can select a project only when we have the required capital.
2. There is a maximum limit on how many projects we can select.

Since we don’t have any constraint on time, we should choose a project, among the projects for which we have enough capital, which gives us a maximum profit. Following this greedy approach will give us the best solution.

While selecting a project, we will do two things:

1. Find all the projects that we can choose with the available capital.
2. From the list of projects in the 1st step, choose the project that gives us a maximum profit.

We can follow the **Two Heaps** approach similar to [Find the Median of a Number Stream](https://www.educative.io/collection/page/5668639101419520/5671464854355968/6308926461050880/). Here are the steps of our algorithm:

1. Add all project capitals to a min-heap, so that we can select a project with the smallest capital requirement.
2. Go through the top projects of the min-heap and filter the projects that can be completed within our available capital. Insert the profits of all these projects into a max-heap, so that we can choose a project with the maximum profit.
3. Finally, select the top project of the max-heap for investment.
4. Repeat the 2nd and 3rd steps for the required number of projects.

### Code [#](https://www.educative.io/courses/grokking-the-coding-interview/B6x69OLX4jY#code)

Here is what our algorithm will look like:

```java
import java.util.*;

class MaximizeCapital {
  public static int findMaximumCapital(int[] capital, int[] profits, int numberOfProjects, int initialCapital) {
    int n = profits.length;
    PriorityQueue<Integer> minCapitalHeap = new PriorityQueue<>(n, (i1, i2) -> capital[i1] - capital[i2]);
    PriorityQueue<Integer> maxProfitHeap = new PriorityQueue<>(n, (i1, i2) -> profits[i2] - profits[i1]);

    // insert all project capitals to a min-heap
    for (int i = 0; i < n; i++)
      minCapitalHeap.offer(i);

    // let's try to find a total of 'numberOfProjects' best projects
    int availableCapital = initialCapital;
    for (int i = 0; i < numberOfProjects; i++) {
      // find all projects that can be selected within the available capital and insert them in a max-heap
      while (!minCapitalHeap.isEmpty() && capital[minCapitalHeap.peek()] <= availableCapital)
        maxProfitHeap.add(minCapitalHeap.poll());

      // terminate if we are not able to find any project that can be completed within the available capital
      if (maxProfitHeap.isEmpty())
        break;

      // select the project with the maximum profit
      availableCapital += profits[maxProfitHeap.poll()];
    }

    return availableCapital;
  }

  public static void main(String[] args) {
    int result = MaximizeCapital.findMaximumCapital(new int[] { 0, 1, 2 }, new int[] { 1, 2, 3 }, 2, 1);
    System.out.println("Maximum capital: " + result);
    result = MaximizeCapital.findMaximumCapital(new int[] { 0, 1, 2, 3 }, new int[] { 1, 2, 3, 5 }, 3, 0);
    System.out.println("Maximum capital: " + result);
  }
}
```

### Time complexity [#](https://www.educative.io/courses/grokking-the-coding-interview/B6x69OLX4jY#time-complexity)

Since, at the most, all the projects will be pushed to both the heaps once, the time complexity of our algorithm is O(NlogN + KlogN), where ‘N’ is the total number of projects and ‘K’ is the number of projects we are selecting.

### Space complexity [#](https://www.educative.io/courses/grokking-the-coding-interview/B6x69OLX4jY#space-complexity)

The space complexity will be O(N) because we will be storing all the projects in the heaps.

# Next Interval (hard)

Given an array of intervals, find the next interval of each interval. In a list of intervals, for an interval ‘i’ its next interval ‘j’ will have the smallest ‘start’ greater than or equal to the ‘end’ of ‘i’.

Write a function to return an array containing indices of the next interval of each input interval. If there is no next interval of a given interval, return -1. It is given that none of the intervals have the same start point.

**Example 1:**

> **Input:** Intervals [[2,3], [3,4], [5,6]]
> **Output:** [1, 2, -1]
> **Explanation:** The next interval of [2,3] is [3,4] having index ‘1’. Similarly, the next interval of [3,4] is [5,6] having index ‘2’. There is no next interval for [5,6] hence we have ‘-1’.

**Example 2:**

> **Input:** Intervals [[3,4], [1,5], [4,6]]
> **Output:** [2, -1, -1]
> **Explanation:** The next interval of [3,4] is [4,6] which has index ‘2’. There is no next interval for [1,5] and [4,6].

## Solution [#](https://www.educative.io/courses/grokking-the-coding-interview/JP8VKGOEpXl#solution)

A brute force solution could be to take one interval at a time and go through all the other intervals to find the next interval. This algorithm will take O(N^2) where ‘N’ is the total number of intervals. Can we do better than that?

We can utilize the **Two Heaps** approach. We can push all intervals into two heaps: one heap to sort the intervals on maximum start time (let’s call it `maxStartHeap`) and the other on maximum end time (let’s call it `maxEndHeap`). We can then iterate through all intervals of the `maxEndHeap’ to find their next interval. Our algorithm will have the following steps:

1. Take out the top (having highest end) interval from the `maxEndHeap` to find its next interval. Let’s call this interval `topEnd`.
2. Find an interval in the `maxStartHeap` with the closest start greater than or equal to the start of `topEnd`. Since `maxStartHeap` is sorted by ‘start’ of intervals, it is easy to find the interval with the highest ‘start’. Let’s call this interval `topStart`.
3. Add the index of `topStart` in the result array as the next interval of `topEnd`. If we can’t find the next interval, add ‘-1’ in the result array.
4. Put the `topStart` back in the `maxStartHeap`, as it could be the next interval of other intervals.
5. Repeat the steps 1-4 until we have no intervals left in `maxEndHeap`.

### Code [#](https://www.educative.io/courses/grokking-the-coding-interview/JP8VKGOEpXl#code)

Here is what our algorithm will look like:

```java
import java.util.*;

class Interval {
  int start = 0;
  int end = 0;

  Interval(int start, int end) {
    this.start = start;
    this.end = end;
  }
}

class NextInterval {
  public static int[] findNextInterval(Interval[] intervals) {
    int n = intervals.length;
    // heap for finding the maximum start
    PriorityQueue<Integer> maxStartHeap = new PriorityQueue<>(n, (i1, i2) -> intervals[i2].start - intervals[i1].start);
    // heap for finding the minimum end
    PriorityQueue<Integer> maxEndHeap = new PriorityQueue<>(n, (i1, i2) -> intervals[i2].end - intervals[i1].end);
    int[] result = new int[n];
    for (int i = 0; i < intervals.length; i++) {
      maxStartHeap.offer(i);
      maxEndHeap.offer(i);
    }

    // go through all the intervals to find each interval's next interval
    for (int i = 0; i < n; i++) {
      int topEnd = maxEndHeap.poll(); // let's find the next interval of the interval which has the highest 'end' 
      result[topEnd] = -1; // defaults to -1
      if (intervals[maxStartHeap.peek()].start >= intervals[topEnd].end) {
        int topStart = maxStartHeap.poll();
        // find the the interval that has the closest 'start'
        while (!maxStartHeap.isEmpty() && intervals[maxStartHeap.peek()].start >= intervals[topEnd].end) {
          topStart = maxStartHeap.poll();
        }
        result[topEnd] = topStart;
        maxStartHeap.add(topStart); // put the interval back as it could be the next interval of other intervals
      }
    }
    return result;
  }

  public static void main(String[] args) {
    Interval[] intervals = new Interval[] { new Interval(2, 3), new Interval(3, 4), new Interval(5, 6) };
    int[] result = NextInterval.findNextInterval(intervals);
    System.out.print("Next interval indices are: ");
    for (int index : result)
      System.out.print(index + " ");
    System.out.println();

    intervals = new Interval[] { new Interval(3, 4), new Interval(1, 5), new Interval(4, 6) };
    result = NextInterval.findNextInterval(intervals);
    System.out.print("Next interval indices are: ");
    for (int index : result)
      System.out.print(index + " ");
  }
}
```

### Time complexity [#](https://www.educative.io/courses/grokking-the-coding-interview/JP8VKGOEpXl#time-complexity)

The time complexity of our algorithm will be O(NlogN),where ‘N’ is the total number of intervals.

### Space complexity [#](https://www.educative.io/courses/grokking-the-coding-interview/JP8VKGOEpXl#space-complexity)

The space complexity will be O(N) because we will be storing all the intervals in the heaps.



