[TOC]

# Introduction

In a lot of problems, we are asked to reverse the links between a set of nodes of a **LinkedList**. Often, the constraint is that we need to do this in-place, i.e., using the existing node objects and without using extra memory.

**In-place Reversal of a LinkedList** pattern describes an efficient way to solve the above problem. In the following chapters, we will solve a bunch of problems using this pattern.

Let’s jump on to our first problem to understand this pattern.

## Problem Statement [#](https://www.educative.io/courses/grokking-the-coding-interview/3wENz1N4WW9#problem-statement)

Given the head of a Singly LinkedList, reverse the LinkedList. Write a function to return the new head of the reversed LinkedList.

![image-20210620173415191](/Users/zhengwenxuan/Desktop/Grokking-Series/Grokking-the-Coding-Interview-Patterns-for-Coding-Questions/image-20210620173415191.png)

## Solution [#](https://www.educative.io/courses/grokking-the-coding-interview/3wENz1N4WW9#solution)

To reverse a LinkedList, we need to reverse one node at a time. We will start with a variable `current` which will initially point to the head of the LinkedList and a variable `previous` which will point to the previous node that we have processed; initially `previous` will point to `null`.

In a stepwise manner, we will reverse the `current` node by pointing it to the `previous` before moving on to the next node. Also, we will update the `previous` to always point to the previous node that we have processed. Here is the visual representation of our algorithm:

![image-20210620173439714](/Users/zhengwenxuan/Desktop/Grokking-Series/Grokking-the-Coding-Interview-Patterns-for-Coding-Questions/image-20210620173439714.png)

![image-20210620173448440](/Users/zhengwenxuan/Desktop/Grokking-Series/Grokking-the-Coding-Interview-Patterns-for-Coding-Questions/image-20210620173448440.png)

![image-20210620173457991](/Users/zhengwenxuan/Desktop/Grokking-Series/Grokking-the-Coding-Interview-Patterns-for-Coding-Questions/image-20210620173457991.png)

![image-20210620173506038](/Users/zhengwenxuan/Desktop/Grokking-Series/Grokking-the-Coding-Interview-Patterns-for-Coding-Questions/image-20210620173506038.png)

![image-20210620173513774](/Users/zhengwenxuan/Desktop/Grokking-Series/Grokking-the-Coding-Interview-Patterns-for-Coding-Questions/image-20210620173513774.png)

![image-20210620173521455](/Users/zhengwenxuan/Desktop/Grokking-Series/Grokking-the-Coding-Interview-Patterns-for-Coding-Questions/image-20210620173521455.png)

![image-20210620173530180](/Users/zhengwenxuan/Desktop/Grokking-Series/Grokking-the-Coding-Interview-Patterns-for-Coding-Questions/image-20210620173530180.png)

### Code [#](https://www.educative.io/courses/grokking-the-coding-interview/3wENz1N4WW9#code)

Here is what our algorithm will look like:

```java
class ListNode {
  int value = 0;
  ListNode next;

  ListNode(int value) {
    this.value = value;
  }
}

class ReverseLinkedList {

  public static ListNode reverse(ListNode head) {
    ListNode current = head; // current node that we will be processing
    ListNode previous = null; // previous node that we have processed
    ListNode next = null; // will be used to temporarily store the next node

    while (current != null) {
      next = current.next; // temporarily store the next node
      current.next = previous; // reverse the current node
      previous = current; // before we move to the next node, point previous to the current node
      current = next; // move on the next node
    }
    // after the loop current will be pointing to 'null' and 'previous' will be the new head
    return previous;
  }

  public static void main(String[] args) {
    ListNode head = new ListNode(2);
    head.next = new ListNode(4);
    head.next.next = new ListNode(6);
    head.next.next.next = new ListNode(8);
    head.next.next.next.next = new ListNode(10);

    ListNode result = ReverseLinkedList.reverse(head);
    System.out.print("Nodes of the reversed LinkedList are: ");
    while (result != null) {
      System.out.print(result.value + " ");
      result = result.next;
    }
  }
}
```

### Time complexity [#](https://www.educative.io/courses/grokking-the-coding-interview/3wENz1N4WW9#time-complexity)

The time complexity of our algorithm will be O(N) where ‘N’ is the total number of nodes in the LinkedList.

### Space complexity [#](https://www.educative.io/courses/grokking-the-coding-interview/3wENz1N4WW9#space-complexity)

We only used constant space, therefore, the space complexity of our algorithm is O(1).

# Reverse a Sub-list (medium)

## Problem Statement [#](https://www.educative.io/courses/grokking-the-coding-interview/qVANqMonoB2#problem-statement)

Given the head of a LinkedList and two positions ‘p’ and ‘q’, reverse the LinkedList from position ‘p’ to ‘q’.

![image-20210620173623336](/Users/zhengwenxuan/Desktop/Grokking-Series/Grokking-the-Coding-Interview-Patterns-for-Coding-Questions/image-20210620173623336.png)

## Solution [#](https://www.educative.io/courses/grokking-the-coding-interview/qVANqMonoB2#solution)

The problem follows the **In-place Reversal of a LinkedList** pattern. We can use a similar approach as discussed in [Reverse a LinkedList](https://www.educative.io/collection/page/5668639101419520/5671464854355968/4519653420302336/). Here are the steps we need to follow:

1. Skip the first `p-1` nodes, to reach the node at position `p`.
2. Remember the node at position `p-1` to be used later to connect with the reversed sub-list.
3. Next, reverse the nodes from `p` to `q` using the same approach discussed in [Reverse a LinkedList](https://www.educative.io/collection/page/5668639101419520/5671464854355968/4519653420302336/).
4. Connect the `p-1` and `q+1` nodes to the reversed sub-list.

### Code [#](https://www.educative.io/courses/grokking-the-coding-interview/qVANqMonoB2#code)

Here is what our algorithm will look like:

```java
import java.util.*;

class ListNode {
  int value = 0;
  ListNode next;

  ListNode(int value) {
    this.value = value;
  }
}

class ReverseSubList {

  public static ListNode reverse(ListNode head, int p, int q) {
    if (p == q)
      return head;

    // after skipping 'p-1' nodes, current will point to 'p'th node
    ListNode current = head, previous = null;
    for (int i = 0; current != null && i < p - 1; ++i) {
      previous = current;
      current = current.next;
    }

    // we are interested in three parts of the LinkedList, part before index 'p', part between 'p' and 
    // 'q', and the part after index 'q'
    ListNode lastNodeOfFirstPart = previous; // points to the node at index 'p-1'
    // after reversing the LinkedList 'current' will become the last node of the sub-list
    ListNode lastNodeOfSubList = current;
    ListNode next = null; // will be used to temporarily store the next node
    // reverse nodes between 'p' and 'q'
    for (int i = 0; current != null && i < q - p + 1; i++) {
      next = current.next;
      current.next = previous;
      previous = current;
      current = next;
    }

    // connect with the first part
    if (lastNodeOfFirstPart != null)
      lastNodeOfFirstPart.next = previous; // 'previous' is now the first node of the sub-list
    else // this means p == 1 i.e., we are changing the first node (head) of the LinkedList
      head = previous;

    // connect with the last part
    lastNodeOfSubList.next = current;

    return head;
  }

  public static void main(String[] args) {
    ListNode head = new ListNode(1);
    head.next = new ListNode(2);
    head.next.next = new ListNode(3);
    head.next.next.next = new ListNode(4);
    head.next.next.next.next = new ListNode(5);

    ListNode result = ReverseSubList.reverse(head, 2, 4);
    System.out.print("Nodes of the reversed LinkedList are: ");
    while (result != null) {
      System.out.print(result.value + " ");
      result = result.next;
    }
  }
}
```

### Time complexity [#](https://www.educative.io/courses/grokking-the-coding-interview/qVANqMonoB2#time-complexity)

The time complexity of our algorithm will be O(N) where ‘N’ is the total number of nodes in the LinkedList.

### Space complexity [#](https://www.educative.io/courses/grokking-the-coding-interview/qVANqMonoB2#space-complexity)

We only used constant space, therefore, the space complexity of our algorithm is O(1).

------

## Similar Questions [#](https://www.educative.io/courses/grokking-the-coding-interview/qVANqMonoB2#similar-questions)

**Problem 1:** Reverse the first ‘k’ elements of a given LinkedList.

**Solution:** This problem can be easily converted to our parent problem; to reverse the first ‘k’ nodes of the list, we need to pass `p=1` and `q=k`.

**Problem 2:** Given a LinkedList with ‘n’ nodes, reverse it based on its size in the following way:

1. If ‘n’ is even, reverse the list in a group of n/2 nodes.
2. If n is odd, keep the middle node as it is, reverse the first ‘n/2’ nodes and reverse the last ‘n/2’ nodes.

**Solution:** When ‘n’ is even we can perform the following steps:

1. Reverse first ‘n/2’ nodes: `head = reverse(head, 1, n/2)`
2. Reverse last ‘n/2’ nodes: `head = reverse(head, n/2 + 1, n)`

When ‘n’ is odd, our algorithm will look like:

1. `head = reverse(head, 1, n/2)`
2. `head = reverse(head, n/2 + 2, n)`

Please note the function call in the second step. We’re skipping two elements as we will be skipping the middle element.

# Reverse every K-element Sub-list (medium)

## Problem Statement [#](https://www.educative.io/courses/grokking-the-coding-interview/RMZylvkGznR#problem-statement)

Given the head of a LinkedList and a number ‘k’, **reverse every ‘k’ sized sub-list** starting from the head.

If, in the end, you are left with a sub-list with less than ‘k’ elements, reverse it too.

![image-20210620173718738](/Users/zhengwenxuan/Desktop/Grokking-Series/Grokking-the-Coding-Interview-Patterns-for-Coding-Questions/image-20210620173718738.png)

## Solution [#](https://www.educative.io/courses/grokking-the-coding-interview/RMZylvkGznR#solution)

The problem follows the **In-place Reversal of a LinkedList** pattern and is quite similar to [Reverse a Sub-list](https://www.educative.io/collection/page/5668639101419520/5671464854355968/5714632037629952/). The only difference is that we have to reverse all the sub-lists. We can use the same approach, starting with the first sub-list (i.e. `p=1, q=k`) and keep reversing all the sublists of size ‘k’.

### Code [#](https://www.educative.io/courses/grokking-the-coding-interview/RMZylvkGznR#code)

Most of the code is the same as [Reverse a Sub-list](https://www.educative.io/collection/page/5668639101419520/5671464854355968/5714632037629952/); only the highlighted lines have a majority of the changes:

```java
import java.util.*;

class ListNode {
  int value = 0;
  ListNode next;

  ListNode(int value) {
    this.value = value;
  }
}

class ReverseEveryKElements {

  public static ListNode reverse(ListNode head, int k) {
    if (k <= 1 || head == null)
      return head;

    ListNode current = head, previous = null;
    while (true) {
      ListNode lastNodeOfPreviousPart = previous;
      // after reversing the LinkedList 'current' will become the last node of the sub-list
      ListNode lastNodeOfSubList = current;
      ListNode next = null; // will be used to temporarily store the next node
      // reverse 'k' nodes
      for (int i = 0; current != null && i < k; i++) {
        next = current.next;
        current.next = previous;
        previous = current;
        current = next;
      }

      // connect with the previous part
      if (lastNodeOfPreviousPart != null)
        lastNodeOfPreviousPart.next = previous; // 'previous' is now the first node of the sub-list
      else // this means we are changing the first node (head) of the LinkedList
        head = previous;

      // connect with the next part
      lastNodeOfSubList.next = current;

      if (current == null) // break, if we've reached the end of the LinkedList
        break;
      // prepare for the next sub-list
      previous = lastNodeOfSubList;
    }

    return head;
  }

  public static void main(String[] args) {
    ListNode head = new ListNode(1);
    head.next = new ListNode(2);
    head.next.next = new ListNode(3);
    head.next.next.next = new ListNode(4);
    head.next.next.next.next = new ListNode(5);
    head.next.next.next.next.next = new ListNode(6);
    head.next.next.next.next.next.next = new ListNode(7);
    head.next.next.next.next.next.next.next = new ListNode(8);

    ListNode result = ReverseEveryKElements.reverse(head, 3);
    System.out.print("Nodes of the reversed LinkedList are: ");
    while (result != null) {
      System.out.print(result.value + " ");
      result = result.next;
    }
  }
}
```

### Time complexity [#](https://www.educative.io/courses/grokking-the-coding-interview/RMZylvkGznR#time-complexity)

The time complexity of our algorithm will be O(N) where ‘N’ is the total number of nodes in the LinkedList.

### Space complexity [#](https://www.educative.io/courses/grokking-the-coding-interview/RMZylvkGznR#space-complexity)

We only used constant space, therefore, the space complexity of our algorithm is O(1).

# Reverse alternating K-element Sub-list (medium) [#](https://www.educative.io/courses/grokking-the-coding-interview/m2YYJJRP9KG#reverse-alternating-k-element-sub-list-medium)

Given the head of a LinkedList and a number ‘k’, **reverse every alternating ‘k’ sized sub-list** starting from the head.

If, in the end, you are left with a sub-list with less than ‘k’ elements, reverse it too.

![image-20210620173817688](/Users/zhengwenxuan/Desktop/Grokking-Series/Grokking-the-Coding-Interview-Patterns-for-Coding-Questions/image-20210620173817688.png)

## Solution [#](https://www.educative.io/courses/grokking-the-coding-interview/q2lZKgLm980#solution)

The problem follows the **In-place Reversal of a LinkedList** pattern and is quite similar to [Reverse every K-element Sub-list](https://www.educative.io/collection/page/5668639101419520/5671464854355968/6119318955753472/). The only difference is that we have to skip ‘k’ alternating elements. We can follow a similar approach, and in each iteration after reversing ‘k’ elements, we will skip the next ‘k’ elements.

### Code [#](https://www.educative.io/courses/grokking-the-coding-interview/q2lZKgLm980#code)

Most of the code is the same as [Reverse every K-element Sub-list](https://www.educative.io/collection/page/5668639101419520/5671464854355968/6119318955753472/); only the highlighted lines have a majority of the changes:

```java
import java.util.*;

class ListNode {
  int value = 0;
  ListNode next;

  ListNode(int value) {
    this.value = value;
  }
}

class ReverseAlternatingKElements {

  public static ListNode reverse(ListNode head, int k) {
    if (k <= 1 || head == null)
      return head;

    ListNode current = head, previous = null;
    while (current != null) { // break if we've reached the end of the list
      ListNode lastNodeOfPreviousPart = previous;
      // after reversing the LinkedList 'current' will become the last node of the sub-list
      ListNode lastNodeOfSubList = current;
      ListNode next = null; // will be used to temporarily store the next node
      // reverse 'k' nodes
      for (int i = 0; current != null && i < k; i++) {
        next = current.next;
        current.next = previous;
        previous = current;
        current = next;
      }

      // connect with the previous part
      if (lastNodeOfPreviousPart != null)
        lastNodeOfPreviousPart.next = previous; // 'previous' is now the first node of the sub-list
      else // this means we are changing the first node (head) of the LinkedList
        head = previous;

      // connect with the next part
      lastNodeOfSubList.next = current;

      // skip 'k' nodes
      for (int i = 0; current != null && i < k; ++i) {
        previous = current;
        current = current.next;
      }
    }

    return head;
  }

  public static void main(String[] args) {
    ListNode head = new ListNode(1);
    head.next = new ListNode(2);
    head.next.next = new ListNode(3);
    head.next.next.next = new ListNode(4);
    head.next.next.next.next = new ListNode(5);
    head.next.next.next.next.next = new ListNode(6);
    head.next.next.next.next.next.next = new ListNode(7);
    head.next.next.next.next.next.next.next = new ListNode(8);

    ListNode result = ReverseAlternatingKElements.reverse(head, 2);
    System.out.print("Nodes of the reversed LinkedList are: ");
    while (result != null) {
      System.out.print(result.value + " ");
      result = result.next;
    }
  }
}
```

### Time complexity [#](https://www.educative.io/courses/grokking-the-coding-interview/q2lZKgLm980#time-complexity)

The time complexity of our algorithm will be O(N) where ‘N’ is the total number of nodes in the LinkedList.

### Space complexity [#](https://www.educative.io/courses/grokking-the-coding-interview/q2lZKgLm980#space-complexity)

We only used constant space, therefore, the space complexity of our algorithm is O(1).

# Rotate a LinkedList (medium)

Given the head of a Singly LinkedList and a number ‘k’, rotate the LinkedList to the right by ‘k’ nodes.

![image-20210620173954980](/Users/zhengwenxuan/Desktop/Grokking-Series/Grokking-the-Coding-Interview-Patterns-for-Coding-Questions/image-20210620173954980.png)

## Solution [#](https://www.educative.io/courses/grokking-the-coding-interview/gkAM9kxgY8Z#solution)

Another way of defining the rotation is to take the sub-list of ‘k’ ending nodes of the LinkedList and connect them to the beginning. Other than that we have to do three more things:

1. Connect the last node of the LinkedList to the head, because the list will have a different tail after the rotation.
2. The new head of the LinkedList will be the node at the beginning of the sublist.
3. The node right before the start of sub-list will be the new tail of the rotated LinkedList.

### Code [#](https://www.educative.io/courses/grokking-the-coding-interview/gkAM9kxgY8Z#code)

Here is what our algorithm will look like:

```java
import java.util.*;

class ListNode {
  int value = 0;
  ListNode next;

  ListNode(int value) {
    this.value = value;
  }
}

class RotateList {

  public static ListNode rotate(ListNode head, int rotations) {
    if (head == null || head.next == null || rotations <= 0)
      return head;

    // find the length and the last node of the list
    ListNode lastNode = head;
    int listLength = 1;
    while (lastNode.next != null) {
      lastNode = lastNode.next;
      listLength++;
    }

    lastNode.next = head; // connect the last node with the head to make it a circular list
    rotations %= listLength; // no need to do rotations more than the length of the list
    int skipLength = listLength - rotations;
    ListNode lastNodeOfRotatedList = head;
    for (int i = 0; i < skipLength - 1; i++)
      lastNodeOfRotatedList = lastNodeOfRotatedList.next;

    // 'lastNodeOfRotatedList.next' is pointing to the sub-list of 'k' ending nodes
    head = lastNodeOfRotatedList.next;
    lastNodeOfRotatedList.next = null;
    return head;
  }

  public static void main(String[] args) {
    ListNode head = new ListNode(1);
    head.next = new ListNode(2);
    head.next.next = new ListNode(3);
    head.next.next.next = new ListNode(4);
    head.next.next.next.next = new ListNode(5);
    head.next.next.next.next.next = new ListNode(6);

    ListNode result = RotateList.rotate(head, 3);
    System.out.print("Nodes of the reversed LinkedList are: ");
    while (result != null) {
      System.out.print(result.value + " ");
      result = result.next;
    }
  }
}
```

### Time complexity [#](https://www.educative.io/courses/grokking-the-coding-interview/gkAM9kxgY8Z#time-complexity)

The time complexity of our algorithm will be O(N) where ‘N’ is the total number of nodes in the LinkedList.

### Space complexity [#](https://www.educative.io/courses/grokking-the-coding-interview/gkAM9kxgY8Z#space-complexity)

We only used constant space, therefore, the space complexity of our algorithm is O(1).

