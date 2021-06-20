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

As we are iterating through both the lists once, the time complexity of the above algorithm is O(N + M), where ‘N’ and ‘M’ are the total number of intervals in the input arrays respectively.

## Space complexity

Ignoring the space needed for the result list, the algorithm runs in constant space O(1).

# Conflicting Appointments (medium)

## Problem Statement [#](https://www.educative.io/courses/grokking-the-coding-interview/qVV79nGVgAG#problem-statement)

Given an array of intervals representing ‘N’ appointments, find out if a person can **attend all the appointments**.

**Example 1:**

```
Appointments: [[1,4], [2,5], [7,9]]
Output: false
Explanation: Since [1,4] and [2,5] overlap, a person cannot attend both of these appointments.
```

**Example 2:**

```
Appointments: [[6,7], [2,4], [8,12]]
Output: true
Explanation: None of the appointments overlap, therefore a person can attend all of them.
```

**Example 3:**

```
Appointments: [[4,5], [2,3], [3,6]]
Output: false
Explanation: Since [4,5] and [3,6] overlap, a person cannot attend both of these appointments.
```

## Solution [#](https://www.educative.io/courses/grokking-the-coding-interview/qVV79nGVgAG#solution)

The problem follows the [Merge Intervals](https://www.educative.io/collection/page/5668639101419520/5671464854355968/5652017242439680/) pattern. We can sort all the intervals by start time and then check if any two intervals overlap. A person will not be able to attend all appointments if any two appointments overlap.

## Code [#](https://www.educative.io/courses/grokking-the-coding-interview/qVV79nGVgAG#code)

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

class ConflictingAppointments {

  public static boolean canAttendAllAppointments(Interval[] intervals) {
    // sort the intervals by start time
    Arrays.sort(intervals, (a, b) -> Integer.compare(a.start, b.start));

    // find any overlapping appointment
    for (int i = 1; i < intervals.length; i++) {
      if (intervals[i].start < intervals[i - 1].end) {
        // please note the comparison above, it is "<" and not "<="
        // while merging we needed "<=" comparison, as we will be merging the two
        // intervals having condition "intervals[i].start == intervals[i - 1].end" but
        // such intervals don't represent conflicting appointments as one starts right
        // after the other
        return false;
      }
    }
    return true;
  }

  public static void main(String[] args) {
    Interval[] intervals = { new Interval(1, 4), new Interval(2, 5), new Interval(7, 9) };
    boolean result = ConflictingAppointments.canAttendAllAppointments(intervals);
    System.out.println("Can attend all appointments: " + result);

    Interval[] intervals1 = { new Interval(6, 7), new Interval(2, 4), new Interval(8, 12) };
    result = ConflictingAppointments.canAttendAllAppointments(intervals1);
    System.out.println("Can attend all appointments: " + result);

    Interval[] intervals2 = { new Interval(4, 5), new Interval(2, 3), new Interval(3, 6) };
    result = ConflictingAppointments.canAttendAllAppointments(intervals2);
    System.out.println("Can attend all appointments: " + result);
  }
}

```

### Time complexity [#](https://www.educative.io/courses/grokking-the-coding-interview/qVV79nGVgAG#time-complexity)

The time complexity of the above algorithm is O(N*logN), where ‘N’ is the total number of appointments. Though we are iterating the intervals only once, our algorithm will take O(N * logN)since we need to sort them in the beginning.

### Space complexity [#](https://www.educative.io/courses/grokking-the-coding-interview/qVV79nGVgAG#space-complexity)

The space complexity of the above algorithm will be O(N), which we need for sorting. For Java, `Arrays.sort()` uses [Timsort](https://en.wikipedia.org/wiki/Timsort), which needs O(N) space.

------

## Similar Problems [#](https://www.educative.io/courses/grokking-the-coding-interview/qVV79nGVgAG#similar-problems)

**Problem 1:** Given a list of appointments, find all the conflicting appointments.

**Example:**

```
Appointments: [[4,5], [2,3], [3,6], [5,7], [7,8]]
Output: 
[4,5] and [3,6] conflict. 
[3,6] and [5,7] conflict.
```

# Minimum Meeting Rooms (hard) [#](https://www.educative.io/courses/grokking-the-coding-interview/xVoBRZz7RwP#minimum-meeting-rooms-hard)

Given a list of intervals representing the start and end time of ‘N’ meetings, find the **minimum number of rooms** required to **hold all the meetings**.

**Example 1:**

```
Meetings: [[1,4], [2,5], [7,9]]
Output: 2
Explanation: Since [1,4] and [2,5] overlap, we need two rooms to hold these two meetings. [7,9] can 
occur in any of the two rooms later.
```

**Example 2:**

```
Meetings: [[6,7], [2,4], [8,12]]
Output: 1
Explanation: None of the meetings overlap, therefore we only need one room to hold all meetings.
```

**Example 3:**

```
Meetings: [[1,4], [2,3], [3,6]]
Output:2
Explanation: Since [1,4] overlaps with the other two meetings [2,3] and [3,6], we need two rooms to 
hold all the meetings.
```

**Example 4:**

```
Meetings: [[4,5], [2,3], [2,4], [3,5]]
Output: 2
Explanation: We will need one room for [2,3] and [3,5], and another room for [2,4] and [4,5].

Here is a visual representation of Example 4:
```

![image-20210620165545275](/Users/zhengwenxuan/Desktop/Grokking-Series/Grokking-the-Coding-Interview-Patterns-for-Coding-Questions/image-20210620165545275.png)

## Solution [#](https://www.educative.io/courses/grokking-the-coding-interview/JQMAmrVPL7l#solution)

Let’s take the above-mentioned example (4) and try to follow our [Merge Intervals](https://www.educative.io/collection/page/5668639101419520/5671464854355968/5652017242439680/) approach:

**Meetings:** [[4,5], [2,3], [2,4], [3,5]]

**Step 1:** Sorting these meetings on their start time will give us: [[2,3], [2,4], [3,5], [4,5]]

**Step 2:** Merging overlapping meetings:

- [2,3] overlaps with [2,4], so after merging we’ll have => [[2,4], [3,5], [4,5]]
- [2,4] overlaps with [3,5], so after merging we’ll have => [[2,5], [4,5]]
- [2,5] overlaps [4,5], so after merging we’ll have => [2,5]

Since all the given meetings have merged into one big meeting ([2,5]), does this mean that they all are overlapping and we need a minimum of four rooms to hold these meetings? You might have already guessed that the answer is NO! As we can clearly see, some meetings are mutually exclusive. For example, [2,3] and [3,5] do not overlap and can happen in one room. So, to correctly solve our problem, we need to keep track of the mutual exclusiveness of the overlapping meetings.

Here is what our strategy will look like:

1. We will sort the meetings based on start time.
2. We will schedule the first meeting (let’s call it `m1`) in one room (let’s call it `r1`).
3. If the next meeting `m2` is not overlapping with `m1`, we can safely schedule it in the same room `r1`.
4. If the next meeting `m3` is overlapping with `m2` we can’t use `r1`, so we will schedule it in another room (let’s call it `r2`).
5. Now if the next meeting `m4` is overlapping with `m3`, we need to see if the room `r1` has become free. For this, we need to keep track of the end time of the meeting happening in it. If the end time of `m2` is before the start time of `m4`, we can use that room `r1`, otherwise, we need to schedule `m4` in another room `r3`.

We can conclude that we need to **keep track of the ending time of all the meetings currently happening** so that when we try to schedule a new meeting, we can see what meetings have already ended. We need to put this information in a data structure that can easily give us the smallest ending time. A **Min Heap** would fit our requirements best.

So our algorithm will look like this:

1. Sort all the meetings on their start time.
2. Create a min-heap to store all the active meetings. This min-heap will also be used to find the active meeting with the smallest end time.
3. Iterate through all the meetings one by one to add them in the min-heap. Let’s say we are trying to schedule the meeting `m1`.
4. Since the min-heap contains all the active meetings, so before scheduling `m1` we can remove all meetings from the heap that have ended before `m1`, i.e., remove all meetings from the heap that have an end time smaller than or equal to the start time of `m1`.
5. Now add `m1` to the heap.
6. The heap will always have all the overlapping meetings, so we will need rooms for all of them. Keep a counter to remember the maximum size of the heap at any time which will be the minimum number of rooms needed.

## Code [#](https://www.educative.io/courses/grokking-the-coding-interview/JQMAmrVPL7l#code)

Here is what our algorithm will look like:

```java
import java.util.*;

class Meeting {
  int start;
  int end;

  public Meeting(int start, int end) {
    this.start = start;
    this.end = end;
  }
};

class MinimumMeetingRooms {

  public static int findMinimumMeetingRooms(List<Meeting> meetings) {
    if (meetings == null || meetings.size() == 0)
      return 0;

    // sort the meetings by start time
    Collections.sort(meetings, (a, b) -> Integer.compare(a.start, b.start));

    int minRooms = 0;
    PriorityQueue<Meeting> minHeap = 
        new PriorityQueue<>(meetings.size(), (a, b) -> Integer.compare(a.end, b.end));
    for (Meeting meeting : meetings) {
      // remove all meetings that have ended
      while (!minHeap.isEmpty() && meeting.start >= minHeap.peek().end)
        minHeap.poll();
      // add the current meeting into the minHeap
      minHeap.offer(meeting);
      // all active meeting are in the minHeap, so we need rooms for all of them.
      minRooms = Math.max(minRooms, minHeap.size());
    }
    return minRooms;
  }

  public static void main(String[] args) {
    List<Meeting> input = new ArrayList<Meeting>() {
      {
        add(new Meeting(4, 5));
        add(new Meeting(2, 3));
        add(new Meeting(2, 4));
        add(new Meeting(3, 5));
      }
    };
    int result = MinimumMeetingRooms.findMinimumMeetingRooms(input);
    System.out.println("Minimum meeting rooms required: " + result);

    input = new ArrayList<Meeting>() {
      {
        add(new Meeting(1, 4));
        add(new Meeting(2, 5));
        add(new Meeting(7, 9));
      }
    };
    result = MinimumMeetingRooms.findMinimumMeetingRooms(input);
    System.out.println("Minimum meeting rooms required: " + result);

    input = new ArrayList<Meeting>() {
      {
        add(new Meeting(6, 7));
        add(new Meeting(2, 4));
        add(new Meeting(8, 12));
      }
    };
    result = MinimumMeetingRooms.findMinimumMeetingRooms(input);
    System.out.println("Minimum meeting rooms required: " + result);

    input = new ArrayList<Meeting>() {
      {
        add(new Meeting(1, 4));
        add(new Meeting(2, 3));
        add(new Meeting(3, 6));
      }
    };
    result = MinimumMeetingRooms.findMinimumMeetingRooms(input);
    System.out.println("Minimum meeting rooms required: " + result);

    input = new ArrayList<Meeting>() {
      {
        add(new Meeting(4, 5));
        add(new Meeting(2, 3));
        add(new Meeting(2, 4));
        add(new Meeting(3, 5));
      }
    };
    result = MinimumMeetingRooms.findMinimumMeetingRooms(input);
    System.out.println("Minimum meeting rooms required: " + result);
  }
}
```

### Time complexity [#](https://www.educative.io/courses/grokking-the-coding-interview/JQMAmrVPL7l#time-complexity)

The time complexity of the above algorithm is O(N*logN, where ‘N’ is the total number of meetings. This is due to the sorting that we did in the beginning. Also, while iterating the meetings we might need to poll/offer meeting to the priority queue. Each of these operations can take O(logN). Overall our algorithm will take O(NlogN).

### Space complexity

The space complexity of the above algorithm will be O(N) which is required for sorting. Also, in the worst case scenario, we’ll have to insert all the meetings into the **Min Heap** (when all meetings overlap) which will also take O(N) space. The overall space complexity of our algorithm is O(N).

------

## Similar Problems [#](https://www.educative.io/courses/grokking-the-coding-interview/JQMAmrVPL7l#similar-problems)

**Problem 1:** Given a list of intervals, find the point where the maximum number of intervals overlap.

**Problem 2:** Given a list of intervals representing the arrival and departure times of trains to a train station, our goal is to find the minimum number of platforms required for the train station so that no train has to wait.

Both of these problems can be solved using the approach discussed above.

# Maximum CPU Load (hard) [#](https://www.educative.io/courses/grokking-the-coding-interview/xVlyyv3rR93#maximum-cpu-load-hard)

We are given a list of Jobs. Each job has a Start time, an End time, and a CPU load when it is running. Our goal is to find the **maximum CPU load** at any time if all the **jobs are running on the same machine**.

**Example 1:**

```
Jobs: [[1,4,3], [2,5,4], [7,9,6]]
Output: 7
Explanation: Since [1,4,3] and [2,5,4] overlap, their maximum CPU load (3+4=7) will be when both the 
jobs are running at the same time i.e., during the time interval (2,4).
```

**Example 2:**

```
Jobs: [[6,7,10], [2,4,11], [8,12,15]]
Output: 15
Explanation: None of the jobs overlap, therefore we will take the maximum load of any job which is 15.
```

**Example 3:**

```
Jobs: [[1,4,2], [2,4,1], [3,6,5]]
Output: 8
Explanation: Maximum CPU load will be 8 as all jobs overlap during the time interval [3,4]. 
```

## Solution [#](https://www.educative.io/courses/grokking-the-coding-interview/YVwln9kYxV2#solution)

The problem follows the [Merge Intervals](https://www.educative.io/collection/page/5668639101419520/5671464854355968/5652017242439680/) pattern and can easily be converted to [Minimum Meeting Rooms](https://www.educative.io/collection/page/5668639101419520/5671464854355968/5710239819104256/). Similar to ‘Minimum Meeting Rooms’ where we were trying to find the maximum number of meetings happening at any time, for ‘Maximum CPU Load’ we need to find the maximum number of jobs running at any time. We will need to keep a running count of the maximum CPU load at any time to find the overall maximum load.

## Code [#](https://www.educative.io/courses/grokking-the-coding-interview/YVwln9kYxV2#code)

Here is what our algorithm will look like:

```java
import java.util.*;

class Job {
  int start;
  int end;
  int cpuLoad;

  public Job(int start, int end, int cpuLoad) {
    this.start = start;
    this.end = end;
    this.cpuLoad = cpuLoad;
  }
};

class MaximumCPULoad {

  public static int findMaxCPULoad(List<Job> jobs) {
    // sort the jobs by start time
    Collections.sort(jobs, (a, b) -> Integer.compare(a.start, b.start));

    int maxCPULoad = 0;
    int currentCPULoad = 0;
    PriorityQueue<Job> minHeap = new PriorityQueue<>(jobs.size(), (a, b) -> Integer.compare(a.end, b.end));
    for (Job job : jobs) {
      // remove all jobs that have ended
      while (!minHeap.isEmpty() && job.start > minHeap.peek().end)
        currentCPULoad -= minHeap.poll().cpuLoad;

      // add the current job into the minHeap
      minHeap.offer(job);
      currentCPULoad += job.cpuLoad;
      maxCPULoad = Math.max(maxCPULoad, currentCPULoad);
    }
    return maxCPULoad;
  }

  public static void main(String[] args) {
    List<Job> input = new ArrayList<Job>(Arrays.asList(new Job(1, 4, 3), new Job(2, 5, 4), new Job(7, 9, 6)));
    System.out.println("Maximum CPU load at any time: " + MaximumCPULoad.findMaxCPULoad(input));

    input = new ArrayList<Job>(Arrays.asList(new Job(6, 7, 10), new Job(2, 4, 11), new Job(8, 12, 15)));
    System.out.println("Maximum CPU load at any time: " + MaximumCPULoad.findMaxCPULoad(input));

    input = new ArrayList<Job>(Arrays.asList(new Job(1, 4, 2), new Job(2, 4, 1), new Job(3, 6, 5)));
    System.out.println("Maximum CPU load at any time: " + MaximumCPULoad.findMaxCPULoad(input));
  }
}
```

### Time complexity [#](https://www.educative.io/courses/grokking-the-coding-interview/YVwln9kYxV2#time-complexity)

The time complexity of the above algorithm is O(N*logN)*, where ‘N’ is the total number of jobs. This is due to the sorting that we did in the beginning. Also, while iterating the jobs, we might need to poll/offer jobs to the priority queue. Each of these operations can take O(logN). Overall our algorithm will take O(NlogN).

### Space complexity [#](https://www.educative.io/courses/grokking-the-coding-interview/YVwln9kYxV2#space-complexity)

The space complexity of the above algorithm will be O(N), which is required for sorting. Also, in the worst case, we have to insert all the jobs into the priority queue (when all jobs overlap) which will also take O(N) space. The overall space complexity of our algorithm is O(N).

# Employee Free Time (hard) [#](https://www.educative.io/courses/grokking-the-coding-interview/YQykDmBnvB0#employee-free-time-hard)

For ‘K’ employees, we are given a list of intervals representing the working hours of each employee. Our goal is to find out if there is a **free interval that is common to all employees**. You can assume that each list of employee working hours is sorted on the start time.

**Example 1:**

```
Input: Employee Working Hours=[[[1,3], [5,6]], [[2,3], [6,8]]]
Output: [3,5]
Explanation: Both the employees are free between [3,5].
```

**Example 2:**

```
Input: Employee Working Hours=[[[1,3], [9,12]], [[2,4]], [[6,8]]]
Output: [4,6], [8,9]
Explanation: All employees are free between [4,6] and [8,9].
```

**Example 3:**

```
Input: Employee Working Hours=[[[1,3]], [[2,4]], [[3,5], [7,9]]]
Output: [5,7]
Explanation: All employees are free between [5,7].
```

## Solution [#](https://www.educative.io/courses/grokking-the-coding-interview/RLwKZWgMJ1q#solution)

This problem follows the [Merge Intervals](https://www.educative.io/collection/page/5668639101419520/5671464854355968/5652017242439680/) pattern. Let’s take the above-mentioned example (2) and visually draw it:

```
Input: Employee Working Hours=[[[1,3], [9,12]], [[2,4]], [[6,8]]]
Output: [4,6], [8,9]
```

![image-20210620165900996](/Users/zhengwenxuan/Desktop/Grokking-Series/Grokking-the-Coding-Interview-Patterns-for-Coding-Questions/image-20210620165900996.png)

One simple solution can be to put all employees’ working hours in a list and sort them on the start time. Then we can iterate through the list to find the gaps. Let’s dig deeper. Sorting the intervals of the above example will give us:

```
[1,3], [2,4], [6,8], [9,12]
```

We can now iterate through these intervals, and whenever we find non-overlapping intervals (e.g., [2,4] and [6,8]), we can calculate a free interval (e.g., [4,6]). This algorithm will take O(N * logN) time, where ‘N’ is the total number of intervals. This time is needed because we need to sort all the intervals. The space complexity will be O(N), which is needed for sorting. Can we find a better solution?

### Using a Heap to Sort the Intervals [#](https://www.educative.io/courses/grokking-the-coding-interview/RLwKZWgMJ1q#using-a-heap-to-sort-the-intervals)

One fact that we are not utilizing is that each employee list is individually sorted!

How about we take the first interval of each employee and insert it in a **Min Heap**. This **Min Heap** can always give us the interval with the smallest start time. Once we have the smallest start-time interval, we can then compare it with the next smallest start-time interval (again from the **Heap**) to find the gap. This interval comparison is similar to what we suggested in the previous approach.

Whenever we take an interval out of the **Min Heap**, we can insert the same employee’s next interval. This also means that we need to know which interval belongs to which employee.

## Code [#](https://www.educative.io/courses/grokking-the-coding-interview/RLwKZWgMJ1q#code)

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

class EmployeeInterval {
  Interval interval; // interval representing employee's working hours 
  int employeeIndex; // index of the list containing working hours of this employee
  int intervalIndex; // index of the interval in the employee list

  public EmployeeInterval(Interval interval, int employeeIndex, int intervalIndex) {
    this.interval = interval;
    this.employeeIndex = employeeIndex;
    this.intervalIndex = intervalIndex;
  }
};

class EmployeeFreeTime {

  public static List<Interval> findEmployeeFreeTime(List<List<Interval>> schedule) {
    List<Interval> result = new ArrayList<>();
    // PriorityQueue to store one interval from each employee
    PriorityQueue<EmployeeInterval> minHeap = new PriorityQueue<>(
        (a, b) -> Integer.compare(a.interval.start, b.interval.start));

    // insert the first interval of each employee to the queue
    for (int i = 0; i < schedule.size(); i++)
      minHeap.offer(new EmployeeInterval(schedule.get(i).get(0), i, 0));

    Interval previousInterval = minHeap.peek().interval;
    while (!minHeap.isEmpty()) {
      EmployeeInterval queueTop = minHeap.poll();
      // if previousInterval is not overlapping with the next interval, insert a free interval
      if (previousInterval.end < queueTop.interval.start) {
        result.add(new Interval(previousInterval.end, queueTop.interval.start));
        previousInterval = queueTop.interval;
      } else { // overlapping intervals, update the previousInterval if needed
        if (previousInterval.end < queueTop.interval.end)
          previousInterval = queueTop.interval;
      }

      // if there are more intervals available for the same employee, add their next interval
      List<Interval> employeeSchedule = schedule.get(queueTop.employeeIndex);
      if (employeeSchedule.size() > queueTop.intervalIndex + 1) {
        minHeap.offer(new EmployeeInterval(employeeSchedule.get(queueTop.intervalIndex + 1), queueTop.employeeIndex,
            queueTop.intervalIndex + 1));
      }
    }

    return result;
  }

  public static void main(String[] args) {

    List<List<Interval>> input = new ArrayList<>();
    input.add(new ArrayList<Interval>(Arrays.asList(new Interval(1, 3), new Interval(5, 6))));
    input.add(new ArrayList<Interval>(Arrays.asList(new Interval(2, 3), new Interval(6, 8))));
    List<Interval> result = EmployeeFreeTime.findEmployeeFreeTime(input);
    System.out.print("Free intervals: ");
    for (Interval interval : result)
      System.out.print("[" + interval.start + ", " + interval.end + "] ");
    System.out.println();

    input = new ArrayList<>();
    input.add(new ArrayList<Interval>(Arrays.asList(new Interval(1, 3), new Interval(9, 12))));
    input.add(new ArrayList<Interval>(Arrays.asList(new Interval(2, 4))));
    input.add(new ArrayList<Interval>(Arrays.asList(new Interval(6, 8))));
    result = EmployeeFreeTime.findEmployeeFreeTime(input);
    System.out.print("Free intervals: ");
    for (Interval interval : result)
      System.out.print("[" + interval.start + ", " + interval.end + "] ");
    System.out.println();

    input = new ArrayList<>();
    input.add(new ArrayList<Interval>(Arrays.asList(new Interval(1, 3))));
    input.add(new ArrayList<Interval>(Arrays.asList(new Interval(2, 4))));
    input.add(new ArrayList<Interval>(Arrays.asList(new Interval(3, 5), new Interval(7, 9))));
    result = EmployeeFreeTime.findEmployeeFreeTime(input);
    System.out.print("Free intervals: ");
    for (Interval interval : result)
      System.out.print("[" + interval.start + ", " + interval.end + "] ");
  }
}
```

### Time complexity [#](https://www.educative.io/courses/grokking-the-coding-interview/RLwKZWgMJ1q#time-complexity)

The above algorithm’s time complexity is O(N*logK), where ‘N’ is the total number of intervals, and ‘K’ is the total number of employees. This is because we are iterating through the intervals only once (which will take O(N), and every time we process an interval, we remove (and can insert) one interval in the **Min Heap**, (which will take O(logK). At any time, the heap will not have more than ‘K’ elements.

### Space complexity [#](https://www.educative.io/courses/grokking-the-coding-interview/RLwKZWgMJ1q#space-complexity)

The space complexity of the above algorithm will be O(K) as at any time, the heap will not have more than ‘K’ elements.

