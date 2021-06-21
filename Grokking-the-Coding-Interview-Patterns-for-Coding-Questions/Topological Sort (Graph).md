# Introduction

[Topological Sort](https://en.wikipedia.org/wiki/Topological_sorting) is used to find a linear ordering of elements that have dependencies on each other. For example, if event ‘B’ is dependent on event ‘A’, ‘A’ comes before ‘B’ in topological ordering.

This pattern defines an easy way to understand the technique for performing topological sorting of a set of elements and then solves a few problems using it.

Let’s see this pattern in action.

# Topological Sort (medium)

## Problem Statement [#](https://www.educative.io/courses/grokking-the-coding-interview/m25rBmwLV00#problem-statement)

[Topological Sort](https://en.wikipedia.org/wiki/Topological_sorting) of a directed graph (a graph with unidirectional edges) is a linear ordering of its vertices such that for every directed edge (U, V) from vertex `U` to vertex `V`, `U` comes before `V` in the ordering.

Given a directed graph, find the topological ordering of its vertices.

**Example 1:**

```
Input: Vertices=4, Edges=[3, 2], [3, 0], [2, 0], [2, 1]
Output: Following are the two valid topological sorts for the given graph:
1) 3, 2, 0, 1
2) 3, 2, 1, 0
```

![image-20210621162747095](/Users/zhengwenxuan/Desktop/Grokking-Series/Grokking-the-Coding-Interview-Patterns-for-Coding-Questions/image-20210621162747095.png)

**Example 2:**

```
Input: Vertices=5, Edges=[4, 2], [4, 3], [2, 0], [2, 1], [3, 1]
Output: Following are all valid topological sorts for the given graph:
1) 4, 2, 3, 0, 1
2) 4, 3, 2, 0, 1
3) 4, 3, 2, 1, 0
4) 4, 2, 3, 1, 0
5) 4, 2, 0, 3, 1
```

![image-20210621162806764](/Users/zhengwenxuan/Desktop/Grokking-Series/Grokking-the-Coding-Interview-Patterns-for-Coding-Questions/image-20210621162806764.png)

**Example 3:**

```
Input: Vertices=7, Edges=[6, 4], [6, 2], [5, 3], [5, 4], [3, 0], [3, 1], [3, 2], [4, 1]
Output: Following are all valid topological sorts for the given graph:
1) 5, 6, 3, 4, 0, 1, 2
2) 6, 5, 3, 4, 0, 1, 2
3) 5, 6, 4, 3, 0, 2, 1
4) 6, 5, 4, 3, 0, 1, 2
5) 5, 6, 3, 4, 0, 2, 1
6) 5, 6, 3, 4, 1, 2, 0

There are other valid topological ordering of the graph too.
```

![image-20210621162824465](/Users/zhengwenxuan/Desktop/Grokking-Series/Grokking-the-Coding-Interview-Patterns-for-Coding-Questions/image-20210621162824465.png)

## Solution [#](https://www.educative.io/courses/grokking-the-coding-interview/m25rBmwLV00#solution)

The basic idea behind the topological sort is to provide a partial ordering among the vertices of the graph such that if there is an edge from `U` to `V` then U≤V i.e., `U` comes before `V` in the ordering. Here are a few fundamental concepts related to topological sort:

1. **Source:** Any node that has no incoming edge and has only outgoing edges is called a source.
2. **Sink:** Any node that has only incoming edges and no outgoing edge is called a sink.
3. So, we can say that a topological ordering starts with one of the sources and ends at one of the sinks.
4. A topological ordering is possible only when the graph has no directed cycles, i.e. if the graph is a **Directed Acyclic Graph (DAG)**. If the graph has a cycle, some vertices will have cyclic dependencies which makes it impossible to find a linear ordering among vertices.

To find the topological sort of a graph we can traverse the graph in a **Breadth First Search (BFS)** way. We will start with all the sources, and in a stepwise fashion, save all sources to a sorted list. We will then remove all sources and their edges from the graph. After the removal of the edges, we will have new sources, so we will repeat the above process until all vertices are visited.

Here is the visual representation of this algorithm for Example-3:

![image-20210621162840205](/Users/zhengwenxuan/Desktop/Grokking-Series/Grokking-the-Coding-Interview-Patterns-for-Coding-Questions/image-20210621162840205.png)

This is how we can implement this algorithm:

**a. Initialization**

1. We will store the graph in **Adjacency Lists**, which means each parent vertex will have a list containing all of its children. We will do this using a **HashMap** where the ‘key’ will be the parent vertex number and the value will be a **List** containing children vertices.
2. To find the sources, we will keep a **HashMap** to count the in-degrees i.e., count of incoming edges of each vertex. Any vertex with ‘0’ in-degree will be a source.

**b. Build the graph and find in-degrees of all vertices**

1. We will build the graph from the input and populate the in-degrees **HashMap**.

**c. Find all sources**

1. All vertices with ‘0’ in-degrees will be our sources and we will store them in a **Queue**.

**d. Sort**

1. For each source, do the following things:
   - Add it to the sorted list.
   - Get all of its children from the graph.
   - Decrement the in-degree of each child by 1.
   - If a child’s in-degree becomes ‘0’, add it to the sources **Queue**.
2. Repeat step 1, until the source **Queue** is empty.

### Code [#](https://www.educative.io/courses/grokking-the-coding-interview/m25rBmwLV00#code)

Here is what our algorithm will look like:

```java
import java.util.*;

class TopologicalSort {
  public static List<Integer> sort(int vertices, int[][] edges) {
    List<Integer> sortedOrder = new ArrayList<>();
    if (vertices <= 0)
      return sortedOrder;

    // a. Initialize the graph
    HashMap<Integer, Integer> inDegree = new HashMap<>(); // count of incoming edges for every vertex
    HashMap<Integer, List<Integer>> graph = new HashMap<>(); // adjacency list graph
    for (int i = 0; i < vertices; i++) {
      inDegree.put(i, 0);
      graph.put(i, new ArrayList<Integer>());
    }

    // b. Build the graph
    for (int i = 0; i < edges.length; i++) {
      int parent = edges[i][0], child = edges[i][1];
      graph.get(parent).add(child); // put the child into it's parent's list
      inDegree.put(child, inDegree.get(child) + 1); // increment child's inDegree
    }

    // c. Find all sources i.e., all vertices with 0 in-degrees
    Queue<Integer> sources = new LinkedList<>();
    for (Map.Entry<Integer, Integer> entry : inDegree.entrySet()) {
      if (entry.getValue() == 0)
        sources.add(entry.getKey());
    }

    // d. For each source, add it to the sortedOrder and subtract one from all of its children's in-degrees
    // if a child's in-degree becomes zero, add it to the sources queue
    while (!sources.isEmpty()) {
      int vertex = sources.poll();
      sortedOrder.add(vertex);
      List<Integer> children = graph.get(vertex); // get the node's children to decrement their in-degrees
      for (int child : children) {
        inDegree.put(child, inDegree.get(child) - 1);
        if (inDegree.get(child) == 0)
          sources.add(child);
      }
    }

    if (sortedOrder.size() != vertices) // topological sort is not possible as the graph has a cycle
      return new ArrayList<>();

    return sortedOrder;
  }

  public static void main(String[] args) {
    List<Integer> result = TopologicalSort.sort(4,
        new int[][] { new int[] { 3, 2 }, new int[] { 3, 0 }, new int[] { 2, 0 }, new int[] { 2, 1 } });
    System.out.println(result);

    result = TopologicalSort.sort(5, new int[][] { new int[] { 4, 2 }, new int[] { 4, 3 }, new int[] { 2, 0 },
        new int[] { 2, 1 }, new int[] { 3, 1 } });
    System.out.println(result);

    result = TopologicalSort.sort(7, new int[][] { new int[] { 6, 4 }, new int[] { 6, 2 }, new int[] { 5, 3 },
        new int[] { 5, 4 }, new int[] { 3, 0 }, new int[] { 3, 1 }, new int[] { 3, 2 }, new int[] { 4, 1 } });
    System.out.println(result);
  }
}
```

### Time Complexity [#](https://www.educative.io/courses/grokking-the-coding-interview/m25rBmwLV00#time-complexity)

In step ‘d’, each vertex will become a source only once and each edge will be accessed and removed once. Therefore, the time complexity of the above algorithm will be O(V+E), where ‘V’ is the total number of vertices and ‘E’ is the total number of edges in the graph.

### Space Complexity [#](https://www.educative.io/courses/grokking-the-coding-interview/m25rBmwLV00#space-complexity)

The space complexity will be O(V+E), since we are storing all of the edges for each vertex in an adjacency list.

------

## Similar Problems [#](https://www.educative.io/courses/grokking-the-coding-interview/m25rBmwLV00#similar-problems)

**Problem 1:** Find if a given **Directed Graph** has a cycle in it or not.

**Solution:** If we can’t determine the topological ordering of all the vertices of a directed graph, the graph has a cycle in it. This was also referred to in the above code:

```
  if (sortedOrder.size() != vertices) // topological sort is not possible as the graph has a cycle
      return new ArrayList<>();
```

# Tasks Scheduling (medium)

## Problem Statement [#](https://www.educative.io/courses/grokking-the-coding-interview/gxJrM9goEMr#problem-statement)

There are ‘N’ tasks, labeled from ‘0’ to ‘N-1’. Each task can have some prerequisite tasks which need to be completed before it can be scheduled. Given the number of tasks and a list of prerequisite pairs, find out if it is possible to schedule all the tasks.

**Example 1:**

```
Input: Tasks=3, Prerequisites=[0, 1], [1, 2]
Output: true
Explanation: To execute task '1', task '0' needs to finish first. Similarly, task '1' needs to finish 
before '2' can be scheduled. A possible sceduling of tasks is: [0, 1, 2] 
```

**Example 2:**

```
Input: Tasks=3, Prerequisites=[0, 1], [1, 2], [2, 0]
Output: false
Explanation: The tasks have cyclic dependency, therefore they cannot be sceduled.
```

**Example 3:**

```
Input: Tasks=6, Prerequisites=[2, 5], [0, 5], [0, 4], [1, 4], [3, 2], [1, 3]
Output: true
Explanation: A possible sceduling of tasks is: [0 1 4 3 2 5] 
```

## Solution [#](https://www.educative.io/courses/grokking-the-coding-interview/gxJrM9goEMr#solution)

This problem is asking us to find out if it is possible to find a topological ordering of the given tasks. The tasks are equivalent to the vertices and the prerequisites are the edges.

We can use a similar algorithm as described in [Topological Sort](https://www.educative.io/collection/page/5668639101419520/5671464854355968/6010387461832704/) to find the topological ordering of the tasks. If the ordering does not include all the tasks, we will conclude that some tasks have cyclic dependencies.

### Code [#](https://www.educative.io/courses/grokking-the-coding-interview/gxJrM9goEMr#code)

Here is what our algorithm will look like (only the highlighted lines have changed):

```java
import java.util.*;

class TaskScheduling {
  public static boolean isSchedulingPossible(int tasks, int[][] prerequisites) {
    List<Integer> sortedOrder = new ArrayList<>();
    if (tasks <= 0)
      return false;

    // a. Initialize the graph
    HashMap<Integer, Integer> inDegree = new HashMap<>(); // count of incoming edges for every vertex
    HashMap<Integer, List<Integer>> graph = new HashMap<>(); // adjacency list graph
    for (int i = 0; i < tasks; i++) {
      inDegree.put(i, 0);
      graph.put(i, new ArrayList<Integer>());
    }

    // b. Build the graph
    for (int i = 0; i < prerequisites.length; i++) {
      int parent = prerequisites[i][0], child = prerequisites[i][1];
      graph.get(parent).add(child); // put the child into it's parent's list
      inDegree.put(child, inDegree.get(child) + 1); // increment child's inDegree
    }

    // c. Find all sources i.e., all vertices with 0 in-degrees
    Queue<Integer> sources = new LinkedList<>();
    for (Map.Entry<Integer, Integer> entry : inDegree.entrySet()) {
      if (entry.getValue() == 0)
        sources.add(entry.getKey());
    }

    // d. For each source, add it to the sortedOrder and subtract one from all of its children's in-degrees
    // if a child's in-degree becomes zero, add it to the sources queue
    while (!sources.isEmpty()) {
      int vertex = sources.poll();
      sortedOrder.add(vertex);
      List<Integer> children = graph.get(vertex); // get the node's children to decrement their in-degrees
      for (int child : children) {
        inDegree.put(child, inDegree.get(child) - 1);
        if (inDegree.get(child) == 0)
          sources.add(child);
      }
    }

    // if sortedOrder doesn't contain all tasks, there is a cyclic dependency between tasks, therefore, we
    // will not be able to schedule all tasks
    return sortedOrder.size() == tasks;
  }

  public static void main(String[] args) {

    boolean result = TaskScheduling.isSchedulingPossible(3, new int[][] { new int[] { 0, 1 }, new int[] { 1, 2 } });
    System.out.println("Tasks execution possible: " + result);

    result = TaskScheduling.isSchedulingPossible(3,
        new int[][] { new int[] { 0, 1 }, new int[] { 1, 2 }, new int[] { 2, 0 } });
    System.out.println("Tasks execution possible: " + result);

    result = TaskScheduling.isSchedulingPossible(6, new int[][] { new int[] { 2, 5 }, new int[] { 0, 5 },
        new int[] { 0, 4 }, new int[] { 1, 4 }, new int[] { 3, 2 }, new int[] { 1, 3 } });
    System.out.println("Tasks execution possible: " + result);
  }
}
```

### Time complexity [#](https://www.educative.io/courses/grokking-the-coding-interview/gxJrM9goEMr#time-complexity)

In step ‘d’, each task can become a source only once and each edge (prerequisite) will be accessed and removed once. Therefore, the time complexity of the above algorithm will be O(V+E), where ‘V’ is the total number of tasks and ‘E’ is the total number of prerequisites.

### Space complexity [#](https://www.educative.io/courses/grokking-the-coding-interview/gxJrM9goEMr#space-complexity)

The space complexity will be O(V+E), since we are storing all of the prerequisites for each task in an adjacency list.

------

## Similar Problems [#](https://www.educative.io/courses/grokking-the-coding-interview/gxJrM9goEMr#similar-problems)

**Course Schedule:** There are ‘N’ courses, labeled from ‘0’ to ‘N-1’. Each course can have some prerequisite courses which need to be completed before it can be taken. Given the number of courses and a list of prerequisite pairs, find if it is possible for a student to take all the courses.

**Solution:** This problem is exactly similar to our parent problem. In this problem, we have courses instead of tasks.

# Tasks Scheduling Order (medium)

## Problem Statement [#](https://www.educative.io/courses/grokking-the-coding-interview/BnnArPGKolJ#problem-statement)

There are ‘N’ tasks, labeled from ‘0’ to ‘N-1’. Each task can have some prerequisite tasks which need to be completed before it can be scheduled. Given the number of tasks and a list of prerequisite pairs, write a method to find the ordering of tasks we should pick to finish all tasks.

**Example 1:**

```
Input: Tasks=3, Prerequisites=[0, 1], [1, 2]
Output: [0, 1, 2]
Explanation: To execute task '1', task '0' needs to finish first. Similarly, task '1' needs to finish 
before '2' can be scheduled. A possible scheduling of tasks is: [0, 1, 2] 
```

**Example 2:**

```
Input: Tasks=3, Prerequisites=[0, 1], [1, 2], [2, 0]
Output: []
Explanation: The tasks have cyclic dependency, therefore they cannot be scheduled.
```

**Example 3:**

```
Input: Tasks=6, Prerequisites=[2, 5], [0, 5], [0, 4], [1, 4], [3, 2], [1, 3]
Output: [0 1 4 3 2 5] 
Explanation: A possible scheduling of tasks is: [0 1 4 3 2 5] 
```

## Solution [#](https://www.educative.io/courses/grokking-the-coding-interview/BnnArPGKolJ#solution)

This problem is similar to [Tasks Scheduling](https://www.educative.io/collection/page/5668639101419520/5671464854355968/5590021564268544/), the only difference being that we need to find the best ordering of tasks so that it is possible to schedule them all.

### Code [#](https://www.educative.io/courses/grokking-the-coding-interview/BnnArPGKolJ#code)

Here is what our algorithm will look like (only the highlighted lines have changed):

```java
import java.util.*;

class TaskSchedulingOrder {
  public static List<Integer> findOrder(int tasks, int[][] prerequisites) {
    List<Integer> sortedOrder = new ArrayList<>();
    if (tasks <= 0)
      return sortedOrder;

    // a. Initialize the graph
    HashMap<Integer, Integer> inDegree = new HashMap<>(); // count of incoming edges for every vertex
    HashMap<Integer, List<Integer>> graph = new HashMap<>(); // adjacency list graph
    for (int i = 0; i < tasks; i++) {
      inDegree.put(i, 0);
      graph.put(i, new ArrayList<Integer>());
    }

    // b. Build the graph
    for (int i = 0; i < prerequisites.length; i++) {
      int parent = prerequisites[i][0], child = prerequisites[i][1];
      graph.get(parent).add(child); // put the child into it's parent's list
      inDegree.put(child, inDegree.get(child) + 1); // increment child's inDegree
    }

    // c. Find all sources i.e., all vertices with 0 in-degrees
    Queue<Integer> sources = new LinkedList<>();
    for (Map.Entry<Integer, Integer> entry : inDegree.entrySet()) {
      if (entry.getValue() == 0)
        sources.add(entry.getKey());
    }

    // d. For each source, add it to the sortedOrder and subtract one from all of its children's in-degrees
    // if a child's in-degree becomes zero, add it to the sources queue
    while (!sources.isEmpty()) {
      int vertex = sources.poll();
      sortedOrder.add(vertex);
      List<Integer> children = graph.get(vertex); // get the node's children to decrement their in-degrees
      for (int child : children) {
        inDegree.put(child, inDegree.get(child) - 1);
        if (inDegree.get(child) == 0)
          sources.add(child);
      }
    }

    // if sortedOrder doesn't contain all tasks, there is a cyclic dependency between tasks, therefore, we
    // will not be able to schedule all tasks
    if (sortedOrder.size() != tasks)
      return new ArrayList<>();

    return sortedOrder;
  }

  public static void main(String[] args) {
    List<Integer> result = TaskSchedulingOrder.findOrder(3, new int[][] { new int[] { 0, 1 }, new int[] { 1, 2 } });
    System.out.println(result);

    result = TaskSchedulingOrder.findOrder(3,
        new int[][] { new int[] { 0, 1 }, new int[] { 1, 2 }, new int[] { 2, 0 } });
    System.out.println(result);

    result = TaskSchedulingOrder.findOrder(6, new int[][] { new int[] { 2, 5 }, new int[] { 0, 5 }, new int[] { 0, 4 },
        new int[] { 1, 4 }, new int[] { 3, 2 }, new int[] { 1, 3 } });
    System.out.println(result);
  }
}
```

### Time complexity [#](https://www.educative.io/courses/grokking-the-coding-interview/BnnArPGKolJ#time-complexity)

In step ‘d’, each task can become a source only once and each edge (prerequisite) will be accessed and removed once. Therefore, the time complexity of the above algorithm will be O(V+E), where ‘V’ is the total number of tasks and ‘E’ is the total number of prerequisites.

### Space complexity [#](https://www.educative.io/courses/grokking-the-coding-interview/BnnArPGKolJ#space-complexity)

The space complexity will be O(V+E), since we are storing all of the prerequisites for each task in an adjacency list.

------

## Similar Problems [#](https://www.educative.io/courses/grokking-the-coding-interview/BnnArPGKolJ#similar-problems)

**Course Schedule:** There are ‘N’ courses, labeled from ‘0’ to ‘N-1’. Each course has some prerequisite courses which need to be completed before it can be taken. Given the number of courses and a list of prerequisite pairs, write a method to find the best ordering of the courses that a student can take in order to finish all courses.

**Solution:** This problem is exactly similar to our parent problem. In this problem, we have courses instead of tasks.

# All Tasks Scheduling Orders (hard)

## Problem Statement [#](https://www.educative.io/courses/grokking-the-coding-interview/q2YmVjQMMr3#problem-statement)

There are ‘N’ tasks, labeled from ‘0’ to ‘N-1’. Each task can have some prerequisite tasks which need to be completed before it can be scheduled. Given the number of tasks and a list of prerequisite pairs, write a method to print all possible ordering of tasks meeting all prerequisites.

**Example 1:**

```
Input: Tasks=3, Prerequisites=[0, 1], [1, 2]
Output: [0, 1, 2]
Explanation: There is only possible ordering of the tasks.
```

**Example 2:**

```
Input: Tasks=4, Prerequisites=[3, 2], [3, 0], [2, 0], [2, 1]
Output: 
1) [3, 2, 0, 1]
2) [3, 2, 1, 0]
Explanation: There are two possible orderings of the tasks meeting all prerequisites.
```

**Example 3:**

```
Input: Tasks=6, Prerequisites=[2, 5], [0, 5], [0, 4], [1, 4], [3, 2], [1, 3]
Output: 
1) [0, 1, 4, 3, 2, 5]
2) [0, 1, 3, 4, 2, 5]
3) [0, 1, 3, 2, 4, 5]
4) [0, 1, 3, 2, 5, 4]
5) [1, 0, 3, 4, 2, 5]
6) [1, 0, 3, 2, 4, 5]
7) [1, 0, 3, 2, 5, 4]
8) [1, 0, 4, 3, 2, 5]
9) [1, 3, 0, 2, 4, 5]
10) [1, 3, 0, 2, 5, 4]
11) [1, 3, 0, 4, 2, 5]
12) [1, 3, 2, 0, 5, 4]
13) [1, 3, 2, 0, 4, 5]
```

## Solution [#](https://www.educative.io/courses/grokking-the-coding-interview/q2YmVjQMMr3#solution)

This problem is similar to [Tasks Scheduling Order](https://www.educative.io/collection/page/5668639101419520/5671464854355968/5066018374287360/), the only difference is that we need to find all the topological orderings of the tasks.

At any stage, if we have more than one source available and since we can choose any source, therefore, in this case, we will have multiple orderings of the tasks. We can use a recursive approach with **Backtracking** to consider all sources at any step.

### Code [#](https://www.educative.io/courses/grokking-the-coding-interview/q2YmVjQMMr3#code)

Here is what our algorithm will look like:

```java
import java.util.*;

class AllTaskSchedulingOrders {
  public static void printOrders(int tasks, int[][] prerequisites) {
    List<Integer> sortedOrder = new ArrayList<>();
    if (tasks <= 0)
      return;

    // a. Initialize the graph
    HashMap<Integer, Integer> inDegree = new HashMap<>(); // count of incoming edges for every vertex
    HashMap<Integer, List<Integer>> graph = new HashMap<>(); // adjacency list graph
    for (int i = 0; i < tasks; i++) {
      inDegree.put(i, 0);
      graph.put(i, new ArrayList<Integer>());
    }

    // b. Build the graph
    for (int i = 0; i < prerequisites.length; i++) {
      int parent = prerequisites[i][0], child = prerequisites[i][1];
      graph.get(parent).add(child); // put the child into it's parent's list
      inDegree.put(child, inDegree.get(child) + 1); // increment child's inDegree
    }

    // c. Find all sources i.e., all vertices with 0 in-degrees
    Queue<Integer> sources = new LinkedList<>();
    for (Map.Entry<Integer, Integer> entry : inDegree.entrySet()) {
      if (entry.getValue() == 0)
        sources.add(entry.getKey());
    }

    printAllTopologicalSorts(graph, inDegree, sources, sortedOrder);
  }

  private static void printAllTopologicalSorts(HashMap<Integer, List<Integer>> graph,
      HashMap<Integer, Integer> inDegree, Queue<Integer> sources, List<Integer> sortedOrder) {
    if (!sources.isEmpty()) {
      for (Integer vertex : sources) {
        sortedOrder.add(vertex);
        Queue<Integer> sourcesForNextCall = cloneQueue(sources);
        // only remove the current source, all other sources should remain in the queue for the next call
        sourcesForNextCall.remove(vertex);
        List<Integer> children = graph.get(vertex); // get the node's children to decrement their in-degrees
        for (int child : children) {
          inDegree.put(child, inDegree.get(child) - 1);
          if (inDegree.get(child) == 0)
            sourcesForNextCall.add(child); // save the new source for the next call
        }

        // recursive call to print other orderings from the remaining (and new) sources
        printAllTopologicalSorts(graph, inDegree, sourcesForNextCall, sortedOrder);

        // backtrack, remove the vertex from the sorted order and put all of its children back to consider 
        // the next source instead of the current vertex
        sortedOrder.remove(vertex);
        for (int child : children)
          inDegree.put(child, inDegree.get(child) + 1);
      }
    }

    // if sortedOrder doesn't contain all tasks, either we've a cyclic dependency between tasks, or 
    // we have not processed all the tasks in this recursive call
    if (sortedOrder.size() == inDegree.size())
      System.out.println(sortedOrder);
  }

  // makes a deep copy of the queue
  private static Queue<Integer> cloneQueue(Queue<Integer> queue) {
    Queue<Integer> clone = new LinkedList<>();
    for (Integer num : queue)
      clone.add(num);
    return clone;
  }

  public static void main(String[] args) {
    AllTaskSchedulingOrders.printOrders(3, new int[][] { new int[] { 0, 1 }, new int[] { 1, 2 } });
    System.out.println();

    AllTaskSchedulingOrders.printOrders(4,
        new int[][] { new int[] { 3, 2 }, new int[] { 3, 0 }, new int[] { 2, 0 }, new int[] { 2, 1 } });
    System.out.println();

    AllTaskSchedulingOrders.printOrders(6, new int[][] { new int[] { 2, 5 }, new int[] { 0, 5 }, new int[] { 0, 4 },
        new int[] { 1, 4 }, new int[] { 3, 2 }, new int[] { 1, 3 } });
    System.out.println();
  }
}
```

### Time and Space Complexity [#](https://www.educative.io/courses/grokking-the-coding-interview/q2YmVjQMMr3#time-and-space-complexity)

If we don’t have any prerequisites, all combinations of the tasks can represent a topological ordering. As we know, that there can be N!*N*! combinations for ‘N’ numbers, therefore the time and space complexity of our algorithm will be O(V! * E) where ‘V’ is the total number of tasks and ‘E’ is the total prerequisites. We need the ‘E’ part because in each recursive call, at max, we remove (and add back) all the edges.

# Alien Dictionary (hard)

## Problem Statement [#](https://www.educative.io/courses/grokking-the-coding-interview/R8AJWOMxw2q#problem-statement)

There is a dictionary containing words from an alien language for which we don’t know the ordering of the alphabets. Write a method to find the correct order of the alphabets in the alien language. It is given that the input is a valid dictionary and there exists an ordering among its alphabets.

**Example 1:**

```
Input: Words: ["ba", "bc", "ac", "cab"]
Output: bac
Explanation: Given that the words are sorted lexicographically by the rules of the alien language, so
from the given words we can conclude the following ordering among its characters:

1. From "ba" and "bc", we can conclude that 'a' comes before 'c'.
2. From "bc" and "ac", we can conclude that 'b' comes before 'a'

From the above two points, we can conclude that the correct character order is: "bac"
```

**Example 2:**

```
Input: Words: ["cab", "aaa", "aab"]
Output: cab
Explanation: From the given words we can conclude the following ordering among its characters:

1. From "cab" and "aaa", we can conclude that 'c' comes before 'a'.
2. From "aaa" and "aab", we can conclude that 'a' comes before 'b'

From the above two points, we can conclude that the correct character order is: "cab"
```

**Example 3:**

```
Input: Words: ["ywx", "wz", "xww", "xz", "zyy", "zwz"]
Output: ywxz
Explanation: From the given words we can conclude the following ordering among its characters:

1. From "ywx" and "wz", we can conclude that 'y' comes before 'w'.
2. From "wz" and "xww", we can conclude that 'w' comes before 'x'.
3. From "xww" and "xz", we can conclude that 'w' comes before 'z'
4. From "xz" and "zyy", we can conclude that 'x' comes before 'z'
5. From "zyy" and "zwz", we can conclude that 'y' comes before 'w'

From the above five points, we can conclude that the correct character order is: "ywxz"
```

## Solution [#](https://www.educative.io/courses/grokking-the-coding-interview/R8AJWOMxw2q#solution)

Since the given words are sorted lexicographically by the rules of the alien language, we can always compare two adjacent words to determine the ordering of the characters. Take Example-1 above: [“ba”, “bc”, “ac”, “cab”]

1. Take the first two words “ba” and “bc”. Starting from the beginning of the words, find the first character that is different in both words: it would be ‘a’ from “ba” and ‘c’ from “bc”. Because of the sorted order of words (i.e. the dictionary!), we can conclude that ‘a’ comes before ‘c’ in the alien language.
2. Similarly, from “bc” and “ac”, we can conclude that ‘b’ comes before ‘a’.

These two points tell us that we are actually asked to find the topological ordering of the characters, and that the ordering rules should be inferred from adjacent words from the alien dictionary.

This makes the current problem similar to [Tasks Scheduling Order](https://www.educative.io/collection/page/5668639101419520/5671464854355968/5066018374287360/), the only difference being that we need to build the graph of the characters by comparing adjacent words first, and then perform the topological sort for the graph to determine the order of the characters.

### Code [#](https://www.educative.io/courses/grokking-the-coding-interview/R8AJWOMxw2q#code)

Here is what our algorithm will look like (only the highlighted lines have changed):

```java
import java.util.*;

class AlienDictionary {
  public static String findOrder(String[] words) {
    if (words == null || words.length == 0)
      return "";

    // a. Initialize the graph
    HashMap<Character, Integer> inDegree = new HashMap<>(); // count of incoming edges for every vertex
    HashMap<Character, List<Character>> graph = new HashMap<>(); // adjacency list graph
    for (String word : words) {
      for (char character : word.toCharArray()) {
        inDegree.put(character, 0);
        graph.put(character, new ArrayList<Character>());
      }
    }

    // b. Build the graph
    for (int i = 0; i < words.length - 1; i++) {
      String w1 = words[i], w2 = words[i + 1]; // find ordering of characters from adjacent words
      for (int j = 0; j < Math.min(w1.length(), w2.length()); j++) {
        char parent = w1.charAt(j), child = w2.charAt(j);
        if (parent != child) { // if the two characters are different
          graph.get(parent).add(child); // put the child into it's parent's list
          inDegree.put(child, inDegree.get(child) + 1); // increment child's inDegree
          break; // only the first different character between the two words will help us find the order
        }
      }
    }

    // c. Find all sources i.e., all vertices with 0 in-degrees
    Queue<Character> sources = new LinkedList<>();
    for (Map.Entry<Character, Integer> entry : inDegree.entrySet()) {
      if (entry.getValue() == 0)
        sources.add(entry.getKey());
    }

    // d. For each source, add it to the sortedOrder and subtract one from all of its children's in-degrees
    // if a child's in-degree becomes zero, add it to the sources queue
    StringBuilder sortedOrder = new StringBuilder();
    while (!sources.isEmpty()) {
      Character vertex = sources.poll();
      sortedOrder.append(vertex);
      List<Character> children = graph.get(vertex); // get the node's children to decrement their in-degrees
      for (Character child : children) {
        inDegree.put(child, inDegree.get(child) - 1);
        if (inDegree.get(child) == 0)
          sources.add(child);
      }
    }

    // if sortedOrder doesn't contain all characters, there is a cyclic dependency between characters, therefore, we
    // will not be able to find the correct ordering of the characters
    if (sortedOrder.length() != inDegree.size())
      return "";

    return sortedOrder.toString();
  }

  public static void main(String[] args) {
    String result = AlienDictionary.findOrder(new String[] { "ba", "bc", "ac", "cab" });
    System.out.println("Character order: " + result);

    result = AlienDictionary.findOrder(new String[] { "cab", "aaa", "aab" });
    System.out.println("Character order: " + result);

    result = AlienDictionary.findOrder(new String[] { "ywx", "wz", "xww", "xz", "zyy", "zwz" });
    System.out.println("Character order: " + result);
  }
}
```

### Time complexity [#](https://www.educative.io/courses/grokking-the-coding-interview/R8AJWOMxw2q#time-complexity)

In step ‘d’, each task can become a source only once and each edge (a rule) will be accessed and removed once. Therefore, the time complexity of the above algorithm will be O(V+E), where ‘V’ is the total number of different characters and ‘E’ is the total number of the rules in the alien language. Since, at most, each pair of words can give us one rule, therefore, we can conclude that the upper bound for the rules is O(N) where ‘N’ is the number of words in the input. So, we can say that the time complexity of our algorithm is O(V+N)

### Space complexity [#](https://www.educative.io/courses/grokking-the-coding-interview/R8AJWOMxw2q#space-complexity)

The space complexity will be O(V+N), since we are storing all of the rules for each character in an adjacency list.

# Reconstructing a Sequence (hard) [#](https://www.educative.io/courses/grokking-the-coding-interview/m20NY0Rwz7A#reconstructing-a-sequence-hard)

Given a sequence `originalSeq` and an array of sequences, write a method to find if `originalSeq` can be uniquely reconstructed from the array of sequences.

Unique reconstruction means that we need to find if `originalSeq` is the only sequence such that all sequences in the array are subsequences of it.

**Example 1:**

```
Input: originalSeq: [1, 2, 3, 4], seqs: [[1, 2], [2, 3], [3, 4]]
Output: true
Explanation: The sequences [1, 2], [2, 3], and [3, 4] can uniquely reconstruct   
[1, 2, 3, 4], in other words, all the given sequences uniquely define the order of numbers 
in the 'originalSeq'. 
```

**Example 2:**

```
Input: originalSeq: [1, 2, 3, 4], seqs: [[1, 2], [2, 3], [2, 4]]
Output: false
Explanation: The sequences [1, 2], [2, 3], and [2, 4] cannot uniquely reconstruct 
[1, 2, 3, 4]. There are two possible sequences we can construct from the given sequences:
1) [1, 2, 3, 4]
2) [1, 2, 4, 3]
```

**Example 3:**

```
Input: originalSeq: [3, 1, 4, 2, 5], seqs: [[3, 1, 5], [1, 4, 2, 5]]
Output: true
Explanation: The sequences [3, 1, 5] and [1, 4, 2, 5] can uniquely reconstruct 
[3, 1, 4, 2, 5].
```



## Solution [#](https://www.educative.io/courses/grokking-the-coding-interview/m7VAO5OrQr3#solution)

Since each sequence in the given array defines the ordering of some numbers, we need to combine all these ordering rules to find two things:

1. Is it possible to construct the `originalSeq` from all these rules?
2. Are these ordering rules not sufficient enough to define the unique ordering of all the numbers in the `originalSeq`? In other words, can these rules result in more than one sequence?

Take Example-1:

```
originalSeq: [1, 2, 3, 4], seqs:[[1, 2], [2, 3], [3, 4]]
```

The first sequence tells us that ‘1’ comes before ‘2’; the second sequence tells us that ‘2’ comes before ‘3’; the third sequence tells us that ‘3’ comes before ‘4’. Combining all these sequences will result in a unique sequence: [1, 2, 3, 4].

The above explanation tells us that we are actually asked to find the topological ordering of all the numbers and also to verify that there is only one topological ordering of the numbers possible from the given array of the sequences.

This makes the current problem similar to [Tasks Scheduling Order](https://www.educative.io/collection/page/5668639101419520/5671464854355968/5066018374287360/) with two differences:

1. We need to build the graph of the numbers by comparing each pair of numbers in the given array of sequences.
2. We must perform the topological sort for the graph to determine two things:
   - Can the topological ordering construct the `originalSeq`?
   - That there is only one topological ordering of the numbers possible. This can be confirmed if we do not have more than one source at any time while finding the topological ordering of numbers.

### Code [#](https://www.educative.io/courses/grokking-the-coding-interview/m7VAO5OrQr3#code)

Here is what our algorithm will look like (only the highlighted lines have changed):

```java
import java.util.*;

class SequenceReconstruction {
  public static boolean canConstruct(int[] originalSeq, int[][] sequences) {
    List<Integer> sortedOrder = new ArrayList<>();
    if (originalSeq.length <= 0)
      return false;

    // a. Initialize the graph
    HashMap<Integer, Integer> inDegree = new HashMap<>(); // count of incoming edges for every vertex
    HashMap<Integer, List<Integer>> graph = new HashMap<>(); // adjacency list graph
    for (int[] seq : sequences) {
      for (int i = 0; i < seq.length; i++) {
        inDegree.putIfAbsent(seq[i], 0);
        graph.putIfAbsent(seq[i], new ArrayList<Integer>());
      }
    }

    // b. Build the graph
    for (int[] seq : sequences) {
      for (int i = 1; i < seq.length; i++) {
        int parent = seq[i - 1], child = seq[i];
        graph.get(parent).add(child);
        inDegree.put(child, inDegree.get(child) + 1);
      }
    }

    // if we don't have ordering rules for all the numbers we'll not able to uniquely construct the sequence
    if (inDegree.size() != originalSeq.length)
      return false;

    // c. Find all sources i.e., all vertices with 0 in-degrees
    Queue<Integer> sources = new LinkedList<>();
    for (Map.Entry<Integer, Integer> entry : inDegree.entrySet()) {
      if (entry.getValue() == 0)
        sources.add(entry.getKey());
    }

    // d. For each source, add it to the sortedOrder and subtract one from all of its children's in-degrees
    // if a child's in-degree becomes zero, add it to the sources queue
    while (!sources.isEmpty()) {
      if (sources.size() > 1)
        return false; // more than one sources mean, there is more than one way to reconstruct the sequence
      if (originalSeq[sortedOrder.size()] != sources.peek())
        return false; // the next source (or number) is different from the original sequence
      int vertex = sources.poll();
      sortedOrder.add(vertex);
      List<Integer> children = graph.get(vertex); // get the node's children to decrement their in-degrees
      for (int child : children) {
        inDegree.put(child, inDegree.get(child) - 1);
        if (inDegree.get(child) == 0)
          sources.add(child);
      }
    }

    // if sortedOrder's size is not equal to original sequence's size, there is no unique way to construct  
    return sortedOrder.size() == originalSeq.length;
  }

  public static void main(String[] args) {
    boolean result = SequenceReconstruction.canConstruct(new int[] { 1, 2, 3, 4 },
        new int[][] { new int[] { 1, 2 }, new int[] { 2, 3 }, new int[] { 3, 4 } });
    System.out.println("Can we uniquely construct the sequence: " + result);

    result = SequenceReconstruction.canConstruct(new int[] { 1, 2, 3, 4 },
        new int[][] { new int[] { 1, 2 }, new int[] { 2, 3 }, new int[] { 2, 4 } });
    System.out.println("Can we uniquely construct the sequence: " + result);

    result = SequenceReconstruction.canConstruct(new int[] { 3, 1, 4, 2, 5 },
        new int[][] { new int[] { 3, 1, 5 }, new int[] { 1, 4, 2, 5 } });
    System.out.println("Can we uniquely construct the sequence: " + result);
  }
}
```

### Time complexity [#](https://www.educative.io/courses/grokking-the-coding-interview/m7VAO5OrQr3#time-complexity)

In step ‘d’, each number can become a source only once and each edge (a rule) will be accessed and removed once. Therefore, the time complexity of the above algorithm will be O(V+E), where ‘V’ is the count of distinct numbers and ‘E’ is the total number of the rules. Since, at most, each pair of numbers can give us one rule, we can conclude that the upper bound for the rules is O(N) where ‘N’ is the count of numbers in all sequences. So, we can say that the time complexity of our algorithm is O(V+N).

### Space complexity [#](https://www.educative.io/courses/grokking-the-coding-interview/m7VAO5OrQr3#space-complexity)

The space complexity will be O(V+N), since we are storing all of the rules for each number in an adjacency list.

# Minimum Height Trees (hard) [#](https://www.educative.io/courses/grokking-the-coding-interview/3Ym408v7NQA#minimum-height-trees-hard)

We are given an undirected graph that has characteristics of a [k-ary tree](https://en.wikipedia.org/wiki/K-ary_tree). In such a graph, we can choose any node as the root to make a k-ary tree. The root (or the tree) with the minimum height will be called **Minimum Height Tree (MHT)**. There can be multiple MHTs for a graph. In this problem, we need to find all those roots which give us MHTs. Write a method to find all MHTs of the given graph and return a list of their roots.

**Example 1:**

```
Input: vertices: 5, Edges: [[0, 1], [1, 2], [1, 3], [2, 4]]
Output:[1, 2]
Explanation: Choosing '1' or '2' as roots give us MHTs. In the below diagram, we can see that the 
height of the trees with roots '1' or '2' is three which is minimum.
```

![image-20210621170235113](/Users/zhengwenxuan/Desktop/Grokking-Series/Grokking-the-Coding-Interview-Patterns-for-Coding-Questions/image-20210621170235113.png)

**Example 2:**

```
Input: vertices: 4, Edges: [[0, 1], [0, 2], [2, 3]]
Output:[0, 2]
Explanation: Choosing '0' or '2' as roots give us MHTs. In the below diagram, we can see that the 
height of the trees with roots '0' or '2' is three which is minimum.
```

![image-20210621170246676](/Users/zhengwenxuan/Desktop/Grokking-Series/Grokking-the-Coding-Interview-Patterns-for-Coding-Questions/image-20210621170246676.png)

**Example 3:**

```
Input: vertices: 4, Edges: [[0, 1], [1, 2], [1, 3]]
Output:[1]
```

## Solution [#](https://www.educative.io/courses/grokking-the-coding-interview/7nDN8y7JKVA#solution)

From the above-mentioned examples, we can clearly see that any leaf node (i.e., node with only one edge) can never give us an MHT because its adjacent non-leaf nodes will always give an MHT with a smaller height. All the adjacent non-leaf nodes will consider the leaf node as a subtree. Let’s understand this with another example. Suppose we have a tree with root ‘M’ and height ‘5’. Now, if we take another node, say ‘P’, and make the ‘M’ tree as its subtree, then the height of the overall tree with root ‘P’ will be ‘6’ (=5+1). Now, this whole tree can be considered a graph, where ‘P’ is a leaf as it has only one edge (connection with ‘M’). This clearly shows that the leaf node (‘P’) gives us a tree of height ‘6’ whereas its adjacent non-leaf node (‘M’) gives us a tree with smaller height ‘5’ - since ‘P’ will be a child of ‘M’.

This gives us a strategy to find MHTs. Since leaves can’t give us MHT, we can remove them from the graph and remove their edges too. Once we remove the leaves, we will have new leaves. Since these new leaves can’t give us MHT, we will repeat the process and remove them from the graph too. We will prune the leaves until we are left with one or two nodes which will be our answer and the roots for MHTs.

We can implement the above process using the topological sort. Any node with only one edge (i.e., a leaf) can be our source and, in a stepwise fashion, we can remove all sources from the graph to find new sources. We will repeat this process until we are left with one or two nodes in the graph, which will be our answer.

### Code [#](https://www.educative.io/courses/grokking-the-coding-interview/7nDN8y7JKVA#code)

Here is what our algorithm will look like:

```java
import java.util.*;

class MinimumHeightTrees {
  public static List<Integer> findTrees(int nodes, int[][] edges) {
    List<Integer> minHeightTrees = new ArrayList<>();
    if (nodes <= 0)
      return minHeightTrees;

    // with only one node, since its in-degree will be 0, therefore, we need to handle it separately
    if (nodes == 1) {
      minHeightTrees.add(0);
      return minHeightTrees;
    }

    // a. Initialize the graph
    HashMap<Integer, Integer> inDegree = new HashMap<>(); // count of incoming edges for every vertex
    HashMap<Integer, List<Integer>> graph = new HashMap<>(); // adjacency list graph
    for (int i = 0; i < nodes; i++) {
      inDegree.put(i, 0);
      graph.put(i, new ArrayList<Integer>());
    }

    // b. Build the graph
    for (int i = 0; i < edges.length; i++) {
      int n1 = edges[i][0], n2 = edges[i][1];
      // since this is an undirected graph, therefore, add a link for both the nodes
      graph.get(n1).add(n2);
      graph.get(n2).add(n1);
      // increment the in-degrees of both the nodes
      inDegree.put(n1, inDegree.get(n1) + 1);
      inDegree.put(n2, inDegree.get(n2) + 1);
    }

    // c. Find all leaves i.e., all nodes with only 1 in-degree
    Queue<Integer> leaves = new LinkedList<>();
    for (Map.Entry<Integer, Integer> entry : inDegree.entrySet()) {
      if (entry.getValue() == 1)
        leaves.add(entry.getKey());
    }

    // d. Remove leaves level by level and subtract each leave's children's in-degrees.
    // Repeat this until we are left with 1 or 2 nodes, which will be our answer.
    // Any node that has already been a leaf cannot be the root of a minimum height tree, because 
    // its adjacent non-leaf node will always be a better candidate.
    int totalNodes = nodes;
    while (totalNodes > 2) {
      int leavesSize = leaves.size();
      totalNodes -= leavesSize;
      for (int i = 0; i < leavesSize; i++) {
        int vertex = leaves.poll();
        List<Integer> children = graph.get(vertex);
        for (int child : children) {
          inDegree.put(child, inDegree.get(child) - 1);
          if (inDegree.get(child) == 1) // if the child has become a leaf
            leaves.add(child);
        }
      }
    }

    minHeightTrees.addAll(leaves);
    return minHeightTrees;
  }

  public static void main(String[] args) {
    List<Integer> result = MinimumHeightTrees.findTrees(5,
        new int[][] { new int[] { 0, 1 }, new int[] { 1, 2 }, new int[] { 1, 3 }, new int[] { 2, 4 } });
    System.out.println("Roots of MHTs: " + result);

    result = MinimumHeightTrees.findTrees(4,
        new int[][] { new int[] { 0, 1 }, new int[] { 0, 2 }, new int[] { 2, 3 } });
    System.out.println("Roots of MHTs: " + result);

    result = MinimumHeightTrees.findTrees(4,
        new int[][] { new int[] { 0, 1 }, new int[] { 1, 2 }, new int[] { 1, 3 } });
    System.out.println("Roots of MHTs: " + result);
  }
}
```

### Time complexity [#](https://www.educative.io/courses/grokking-the-coding-interview/7nDN8y7JKVA#time-complexity)

In step ‘d’, each node can become a source only once and each edge will be accessed and removed once. Therefore, the time complexity of the above algorithm will be O(V+E), where ‘V’ is the total nodes and ‘E’ is the total number of the edges.

### Space complexity [#](https://www.educative.io/courses/grokking-the-coding-interview/7nDN8y7JKVA#space-complexity)

The space complexity will be O(V+E), since we are storing all of the edges for each node in an adjacency list.

