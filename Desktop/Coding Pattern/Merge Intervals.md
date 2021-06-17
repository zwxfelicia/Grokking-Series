[TOC]

# Introduction

This pattern describes an efficient technique to deal with overlapping intervals. In a lot of problems involving intervals, we either need to find overlapping intervals or merge intervals if they overlap.

Given two intervals (‘a’ and ‘b’), there will be six different ways the two intervals can relate to each other:

![image-20210617120804108](/Users/zhengwenxuan/Desktop/Coding Pattern/image-20210617120804108.png)

Understanding the above six cases will help us in solving all intervals related problems. Let’s jump onto our first problem to understand the **Merge Interval** pattern.

# Merge Intervals (medium)

## Problem Statement 

Given a list of intervals, **merge all the overlapping intervals** to produce a list that has only mutually exclusive intervals.

**Example 1:**

```
Intervals: [[1,4], [2,5], [7,9]]
Output: [[1,5], [7,9]]
Explanation: Since the first two intervals [1,4] and [2,5] overlap, we merged them into 
one [1,5].
```

![image-20210617120851576](/Users/zhengwenxuan/Desktop/Coding Pattern/image-20210617120851576.png)

**Example 2:**

```
Intervals: [[6,7], [2,4], [5,9]]
Output: [[2,4], [5,9]]
Explanation: Since the intervals [6,7] and [5,9] overlap, we merged them into one [5,9].
```

**Example 3:**

```
Intervals: [[1,4], [2,6], [3,5]]
Output: [[1,6]]
Explanation: Since all the given intervals overlap, we merged them into one.
```

## Solution 

Let’s take the example of two intervals (‘a’ and ‘b’) such that `a.start <= b.start`. There are four possible scenarios:

![image-20210617121110103](/Users/zhengwenxuan/Desktop/Coding Pattern/image-20210617121110103.png)

Our goal is to merge the intervals whenever they overlap. For the above-mentioned three overlapping scenarios (2, 3, and 4), this is how we will merge them:

![image-20210617121124297](/Users/zhengwenxuan/Desktop/Coding Pattern/image-20210617121124297.png)

The diagram above clearly shows a merging approach. Our algorithm will look like this:

1. Sort the intervals on the start time to ensure `a.start <= b.start`
2. If ‘a’ overlaps ‘b’ (i.e. `b.start <= a.end`), we need to merge them into a new interval ‘c’ such that:

```
    c.start = a.start
    c.end = max(a.end, b.end)
```

3. We will keep repeating the above two steps to merge ‘c’ with the next interval if it overlaps with ‘c’.

## Code

```java
import java.util.*;

class Interval {
  int start;
  int end;

  public Interval(int start, int end) {
    this.start = start;
    this.end = end;
  }
};

class MergeIntervals {

  public static List<Interval> merge(List<Interval> intervals) {
    if (intervals.size() < 2)
      return intervals;

    // sort the intervals by start time
    Collections.sort(intervals, (a, b) -> Integer.compare(a.start, b.start));

    List<Interval> mergedIntervals = new LinkedList<Interval>();
    Iterator<Interval> intervalItr = intervals.iterator();
    Interval interval = intervalItr.next();
    int start = interval.start;
    int end = interval.end;

    while (intervalItr.hasNext()) {
      interval = intervalItr.next();
      if (interval.start <= end) { // overlapping intervals, adjust the 'end'
        end = Math.max(interval.end, end);
      } else { // non-overlapping interval, add the previous interval and reset
        mergedIntervals.add(new Interval(start, end));
        start = interval.start;
        end = interval.end;
      }
    }
    // add the last interval
    mergedIntervals.add(new Interval(start, end));

    return mergedIntervals;
  }

  public static void main(String[] args) {
    List<Interval> input = new ArrayList<Interval>();
    input.add(new Interval(1, 4));
    input.add(new Interval(2, 5));
    input.add(new Interval(7, 9));
    System.out.print("Merged intervals: ");
    for (Interval interval : MergeIntervals.merge(input))
      System.out.print("[" + interval.start + "," + interval.end + "] ");
    System.out.println();

    input = new ArrayList<Interval>();
    input.add(new Interval(6, 7));
    input.add(new Interval(2, 4));
    input.add(new Interval(5, 9));
    System.out.print("Merged intervals: ");
    for (Interval interval : MergeIntervals.merge(input))
      System.out.print("[" + interval.start + "," + interval.end + "] ");
    System.out.println();

    input = new ArrayList<Interval>();
    input.add(new Interval(1, 4));
    input.add(new Interval(2, 6));
    input.add(new Interval(3, 5));
    System.out.print("Merged intervals: ");
    for (Interval interval : MergeIntervals.merge(input))
      System.out.print("[" + interval.start + "," + interval.end + "] ");
    System.out.println();
  }
}

```

## Time complexity

The time complexity of the above algorithm is O(N * logN)*O*(*N*∗*l**o**g**N*), where ‘N’ is the total number of intervals. We are iterating the intervals only once which will take O(N)*O*(*N*), in the beginning though, since we need to sort the intervals, our algorithm will take O(N * logN)*O*(*N*∗*l**o**g**N*).

## Space complexity

The space complexity of the above algorithm will be O(N)*O*(*N*) as we need to return a list containing all the merged intervals. We will also need O(N)*O*(*N*) space for sorting. For Java, depending on its version, `Collections.sort()` either uses [Merge sort](https://en.wikipedia.org/wiki/Merge_sort) or [Timsort](https://en.wikipedia.org/wiki/Timsort), and both these algorithms need O(N)*O*(*N*) space. Overall, our algorithm has a space complexity of O(N)*O*(*N*).

------

## Similar Problems 

**Problem 1:** Given a set of intervals, find out if any two intervals overlap.

**Example:**

```
Intervals: [[1,4], [2,5], [7,9]]
Output: true
Explanation: Intervals [1,4] and [2,5] overlap
```

**Solution:** We can follow the same approach as discussed above to find if any two intervals overlap.



# Insert Interval (medium)

## Problem Statement

Given a list of non-overlapping intervals sorted by their start time, **insert a given interval at the correct position** and merge all necessary intervals to produce a list that has only mutually exclusive intervals.

**Example 1:**

```
Input: Intervals=[[1,3], [5,7], [8,12]], New Interval=[4,6]
Output: [[1,3], [4,7], [8,12]]
Explanation: After insertion, since [4,6] overlaps with [5,7], we merged them into one [4,7].
```

**Example 2:**

```
Input: Intervals=[[1,3], [5,7], [8,12]], New Interval=[4,10]
Output: [[1,3], [4,12]]
Explanation: After insertion, since [4,10] overlaps with [5,7] & [8,12], we merged them into [4,12].
```

**Example 3:**

```
Input: Intervals=[[2,3],[5,7]], New Interval=[1,4]
Output: [[1,4], [5,7]]
Explanation: After insertion, since [1,4] overlaps with [2,3], we merged them into one [1,4].
```

## Solution 

If the given list was not sorted, we could have simply appended the new interval to it and used the `merge()` function from [Merge Intervals](https://www.educative.io/collection/page/5668639101419520/5671464854355968/5652017242439680/). But since the given list is sorted, we should try to come up with a solution better than O(N * logN

When inserting a new interval in a sorted list, we need to first find the correct index where the new interval can be placed. In other words, we need to skip all the intervals which end before the start of the new interval. So we can iterate through the given sorted listed of intervals and skip all the intervals with the following condition:

```
intervals[i].end < newInterval.start
```

Once we have found the correct place, we can follow an approach similar to [Merge Intervals](https://www.educative.io/collection/page/5668639101419520/5671464854355968/5652017242439680/) to insert and/or merge the new interval. Let’s call the new interval ‘a’ and the first interval with the above condition ‘b’. There are five possibilities:

![image-20210617141005523](/Users/zhengwenxuan/Desktop/Coding Pattern/image-20210617141005523.png)

The diagram above clearly shows the merging approach. To handle all four merging scenarios, we need to do something like this:

```
    c.start = min(a.start, b.start)
    c.end = max(a.end, b.end)
```

Our overall algorithm will look like this:

1. Skip all intervals which end before the start of the new interval, i.e., skip all `intervals` with the following condition:

```
    intervals[i].end < newInterval.start
```

1. Let’s call the last interval ‘b’ that does not satisfy the above condition. If ‘b’ overlaps with the new interval (a) (i.e. `b.start <= a.end`), we need to merge them into a new interval ‘c’:

```
    c.start = min(a.start, b.start)
    c.end = max(a.end, b.end)
```

1. We will repeat the above two steps to merge ‘c’ with the next overlapping interval.

## Code 

Here is what our algorithm will look like:

```java
import java.util.*;

class Interval {
  int start;
  int end;

  public Interval(int start, int end) {
    this.start = start;
    this.end = end;
  }
};

class InsertInterval {

  public static List<Interval> insert(List<Interval> intervals, Interval newInterval) {
    if (intervals == null || intervals.isEmpty())
      return Arrays.asList(newInterval);

    List<Interval> mergedIntervals = new ArrayList<>();

    int i = 0;
    // skip (and add to output) all intervals that come before the 'newInterval'
    while (i < intervals.size() && intervals.get(i).end < newInterval.start)
      mergedIntervals.add(intervals.get(i++));

    // merge all intervals that overlap with 'newInterval'
    while (i < intervals.size() && intervals.get(i).start <= newInterval.end) {
      newInterval.start = Math.min(intervals.get(i).start, newInterval.start);
      newInterval.end = Math.max(intervals.get(i).end, newInterval.end);
      i++;
    }

    // insert the newInterval
    mergedIntervals.add(newInterval);

    // add all the remaining intervals to the output
    while (i < intervals.size())
      mergedIntervals.add(intervals.get(i++));

    return mergedIntervals;
  }

  public static void main(String[] args) {
    List<Interval> input = new ArrayList<Interval>();
    input.add(new Interval(1, 3));
    input.add(new Interval(5, 7));
    input.add(new Interval(8, 12));
    System.out.print("Intervals after inserting the new interval: ");
    for (Interval interval : InsertInterval.insert(input, new Interval(4, 6)))
      System.out.print("[" + interval.start + "," + interval.end + "] ");
    System.out.println();

    input = new ArrayList<Interval>();
    input.add(new Interval(1, 3));
    input.add(new Interval(5, 7));
    input.add(new Interval(8, 12));
    System.out.print("Intervals after inserting the new interval: ");
    for (Interval interval : InsertInterval.insert(input, new Interval(4, 10)))
      System.out.print("[" + interval.start + "," + interval.end + "] ");
    System.out.println();

    input = new ArrayList<Interval>();
    input.add(new Interval(2, 3));
    input.add(new Interval(5, 7));
    System.out.print("Intervals after inserting the new interval: ");
    for (Interval interval : InsertInterval.insert(input, new Interval(1, 4)))
      System.out.print("[" + interval.start + "," + interval.end + "] ");
    System.out.println();
  }
}
```

## Time complexity

As we are iterating through all the intervals only once, the time complexity of the above algorithm is O(N)*O*(*N*), where ‘N’ is the total number of intervals.

## Space complexity

The space complexity of the above algorithm will be O(N)*O*(*N*) as we need to return a list containing all the merged intervals.

# Intervals Intersection (medium)

## Problem Statement 

Given two lists of intervals, find the **intersection of these two lists**. Each list consists of **disjoint intervals sorted on their start time**.

**Example 1:**

```
Input: arr1=[[1, 3], [5, 6], [7, 9]], arr2=[[2, 3], [5, 7]]
Output: [2, 3], [5, 6], [7, 7]
Explanation: The output list contains the common intervals between the two lists.
```

**Example 2:**

```
Input: arr1=[[1, 3], [5, 7], [9, 12]], arr2=[[5, 10]]
Output: [5, 7], [9, 10]
Explanation: The output list contains the common intervals between the two lists.
```

## Solution 

This problem follows the [Merge Intervals](https://www.educative.io/collection/page/5668639101419520/5671464854355968/5652017242439680/) pattern. As we have discussed under [Insert Interval](https://www.educative.io/collection/page/5668639101419520/5671464854355968/5718314357620736/), there are five overlapping possibilities between two intervals ‘a’ and ‘b’. A close observation will tell us that whenever the two intervals overlap, one of the interval’s start time lies within the other interval. This rule can help us identify if any two intervals overlap or not.

![image-20210617141353599](/Users/zhengwenxuan/Desktop/Coding Pattern/image-20210617141353599.png)

Now, if we have found that the two intervals overlap, how can we find the overlapped part?

Again from the above diagram, the overlapping interval will be equal to:

```
    start = max(a.start, b.start)
    end = min(a.end, b.end) 
```

That is, the highest start time and the lowest end time will be the overlapping interval.

So our algorithm will be to iterate through both the lists together to see if any two intervals overlap. If two intervals overlap, we will insert the overlapped part into a result list and move on to the next interval which is finishing early.

## Code 

Here is what our algorithm will look like:

```java
import java.util.*;

class Interval {
  int start;
  int end;

  public Interval(int start, int end) {
    this.start = start;
    this.end = end;
  }
};

class IntervalsIntersection {

  public static Interval[] merge(Interval[] arr1, Interval[] arr2) {
    List<Interval> result = new ArrayList<Interval>();
    int i = 0, j = 0;
    while (i < arr1.length && j < arr2.length) {
      // check if the interval arr[i] intersects with arr2[j]
      // check if one of the interval's start time lies within the other interval
      if ((arr1[i].start >= arr2[j].start && arr1[i].start <= arr2[j].end)
          || (arr2[j].start >= arr1[i].start && arr2[j].start <= arr1[i].end)) {
        // store the intersection part
        result.add(new Interval(Math.max(arr1[i].start, arr2[j].start), Math.min(arr1[i].end, arr2[j].end)));
      }

      // move next from the interval which is finishing first
      if (arr1[i].end < arr2[j].end)
        i++;
      else
        j++;
    }

    return result.toArray(new Interval[result.size()]);
  }

  public static void main(String[] args) {
    Interval[] input1 = new Interval[] { new Interval(1, 3), new Interval(5, 6), new Interval(7, 9) };
    Interval[] input2 = new Interval[] { new Interval(2, 3), new Interval(5, 7) };
    Interval[] result = IntervalsIntersection.merge(input1, input2);
    System.out.print("Intervals Intersection: ");
    for (Interval interval : result)
      System.out.print("[" + interval.start + "," + interval.end + "] ");
    System.out.println();

    input1 = new Interval[] { new Interval(1, 3), new Interval(5, 7), new Interval(9, 12) };
    input2 = new Interval[] { new Interval(5, 10) };
    result = IntervalsIntersection.merge(input1, input2);
    System.out.print("Intervals Intersection: ");
    for (Interval interval : result)
      System.out.print("[" + interval.start + "," + interval.end + "] ");
  }
}
```

## Time complexity

As we are iterating through both the lists once, the time complexity of the above algorithm is O(N + M)*O*(*N*+*M*), where ‘N’ and ‘M’ are the total number of intervals in the input arrays respectively.

## Space complexity

Ignoring the space needed for the result list, the algorithm runs in constant space O(1)*O*(1).