# Binary Tree Level Order Traversal (easy)

## Problem Statement [#](https://www.educative.io/courses/grokking-the-coding-interview/xV7E64m4lnz#problem-statement)

Given a binary tree, populate an array to represent its level-by-level traversal. You should populate the values of all **nodes of each level from left to right** in separate sub-arrays.

**Example 1:**

![image-20210618172433270](/Users/zhengwenxuan/Desktop/Coding Pattern/image-20210618172433270.png)

**Example 2:**

![image-20210618172452668](/Users/zhengwenxuan/Desktop/Coding Pattern/image-20210618172452668.png)

## Solution [#](https://www.educative.io/courses/grokking-the-coding-interview/xV7E64m4lnz#solution)

Since we need to traverse all nodes of each level before moving onto the next level, we can use the **Breadth First Search (BFS)** technique to solve this problem.

We can use a Queue to efficiently traverse in BFS fashion. Here are the steps of our algorithm:

1. Start by pushing the `root` node to the queue.
2. Keep iterating until the queue is empty.
3. In each iteration, first count the elements in the queue (let’s call it `levelSize`). We will have these many nodes in the current level.
4. Next, remove `levelSize` nodes from the queue and push their `value` in an array to represent the current level.
5. After removing each node from the queue, insert both of its children into the queue.
6. If the queue is not empty, repeat from step 3 for the next level.

Let’s take the example-2 mentioned above to visually represent our algorithm:

![image-20210618172523560](/Users/zhengwenxuan/Desktop/Coding Pattern/image-20210618172523560.png)

![image-20210618172532831](/Users/zhengwenxuan/Desktop/Coding Pattern/image-20210618172532831.png)

![image-20210618172539815](/Users/zhengwenxuan/Desktop/Coding Pattern/image-20210618172539815.png)

![image-20210618172546956](/Users/zhengwenxuan/Desktop/Coding Pattern/image-20210618172546956.png)

![image-20210618172554255](/Users/zhengwenxuan/Desktop/Coding Pattern/image-20210618172554255.png)

![image-20210618172601588](/Users/zhengwenxuan/Desktop/Coding Pattern/image-20210618172601588.png)

![image-20210618172610382](/Users/zhengwenxuan/Desktop/Coding Pattern/image-20210618172610382.png)

## Code [#](https://www.educative.io/courses/grokking-the-coding-interview/xV7E64m4lnz#code)

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

class LevelOrderTraversal {
  public static List<List<Integer>> traverse(TreeNode root) {
    List<List<Integer>> result = new ArrayList<List<Integer>>();
    if (root == null)
      return result;

    Queue<TreeNode> queue = new LinkedList<>();
    queue.offer(root);
    while (!queue.isEmpty()) {
      int levelSize = queue.size();
      List<Integer> currentLevel = new ArrayList<>(levelSize);
      for (int i = 0; i < levelSize; i++) {
        TreeNode currentNode = queue.poll();
        // add the node to the current level
        currentLevel.add(currentNode.val);
        // insert the children of current node in the queue
        if (currentNode.left != null)
          queue.offer(currentNode.left);
        if (currentNode.right != null)
          queue.offer(currentNode.right);
      }
      result.add(currentLevel);
    }

    return result;
  }

  public static void main(String[] args) {
    TreeNode root = new TreeNode(12);
    root.left = new TreeNode(7);
    root.right = new TreeNode(1);
    root.left.left = new TreeNode(9);
    root.right.left = new TreeNode(10);
    root.right.right = new TreeNode(5);
    List<List<Integer>> result = LevelOrderTraversal.traverse(root);
    System.out.println("Level order traversal: " + result);
  }
}
```

### Time complexity [#](https://www.educative.io/courses/grokking-the-coding-interview/xV7E64m4lnz#time-complexity)

The time complexity of the above algorithm is O(N), where ‘N’ is the total number of nodes in the tree. This is due to the fact that we traverse each node once.

### Space complexity [#](https://www.educative.io/courses/grokking-the-coding-interview/xV7E64m4lnz#space-complexity)

The space complexity of the above algorithm will be O(N) as we need to return a list containing the level order traversal. We will also need O(N) space for the queue. Since we can have a maximum of N/2 nodes at any level (this could happen only at the lowest level), therefore we will need O(N) space to store them in the queue.

# Reverse Level Order Traversal (easy)

## Problem Statement [#](https://www.educative.io/courses/grokking-the-coding-interview/m2N6GwARL8r#problem-statement)

Given a binary tree, populate an array to represent its level-by-level traversal in reverse order, i.e., the **lowest level comes first**. You should populate the values of all nodes in each level from left to right in separate sub-arrays.

**Example 1:**

![image-20210618172818592](/Users/zhengwenxuan/Desktop/Coding Pattern/image-20210618172818592.png)

**Example 2:**

![image-20210618172828768](/Users/zhengwenxuan/Desktop/Coding Pattern/image-20210618172828768.png)

## Solution [#](https://www.educative.io/courses/grokking-the-coding-interview/m2N6GwARL8r#solution)

This problem follows the [Binary Tree Level Order Traversal](https://www.educative.io/collection/page/5668639101419520/5671464854355968/5726607939469312/) pattern. We can follow the same **BFS** approach. The only difference will be that instead of appending the current level at the end, we will append the current level at the beginning of the result list.

## Code [#](https://www.educative.io/courses/grokking-the-coding-interview/m2N6GwARL8r#code)

Here is what our algorithm will look like; only the highlighted lines have changed. Please note that, for **Java**, we will use a `LinkedList` instead of an `ArrayList` for our result list. As in the case of `ArrayList`, appending an element at the beginning means shifting all the existing elements. Since we need to append the level array at the beginning of the result list, a `LinkedList` will be better, as this shifting of elements is not required in a `LinkedList`. Similarly, we will use a double-ended queue (deque) for **Python**, **C++**, and **JavaScript**.

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

class ReverseLevelOrderTraversal {
  public static List<List<Integer>> traverse(TreeNode root) {
    List<List<Integer>> result = new LinkedList<List<Integer>>();
    if (root == null)
      return result;

    Queue<TreeNode> queue = new LinkedList<>();
    queue.offer(root);
    while (!queue.isEmpty()) {
      int levelSize = queue.size();
      List<Integer> currentLevel = new ArrayList<>(levelSize);
      for (int i = 0; i < levelSize; i++) {
        TreeNode currentNode = queue.poll();
        // add the node to the current level
        currentLevel.add(currentNode.val);
        // insert the children of current node to the queue
        if (currentNode.left != null)
          queue.offer(currentNode.left);
        if (currentNode.right != null)
          queue.offer(currentNode.right);
      }
      // append the current level at the beginning
      result.add(0, currentLevel);
    }

    return result;
  }

  public static void main(String[] args) {
    TreeNode root = new TreeNode(12);
    root.left = new TreeNode(7);
    root.right = new TreeNode(1);
    root.left.left = new TreeNode(9);
    root.right.left = new TreeNode(10);
    root.right.right = new TreeNode(5);
    List<List<Integer>> result = ReverseLevelOrderTraversal.traverse(root);
    System.out.println("Reverse level order traversal: " + result);
  }
}
```

### Time complexity [#](https://www.educative.io/courses/grokking-the-coding-interview/m2N6GwARL8r#time-complexity)

The time complexity of the above algorithm is O(N), where ‘N’ is the total number of nodes in the tree. This is due to the fact that we traverse each node once.

### Space complexity [#](https://www.educative.io/courses/grokking-the-coding-interview/m2N6GwARL8r#space-complexity)

The space complexity of the above algorithm will be O(N) as we need to return a list containing the level order traversal. We will also need O(N) space for the queue. Since we can have a maximum of N/2 nodes at any level (this could happen only at the lowest level), therefore we will need O(N) space to store them in the queue.



# Zigzag Traversal (medium)

## Problem Statement [#](https://www.educative.io/courses/grokking-the-coding-interview/qVA27MMYYn0#problem-statement)

Given a binary tree, populate an array to represent its zigzag level order traversal. You should populate the values of all **nodes of the first level from left to right**, then **right to left for the next level** and keep alternating in the same manner for the following levels.

**Example 1:**

![image-20210618175913193](/Users/zhengwenxuan/Desktop/Coding Pattern/image-20210618175913193.png)

**Example 2:**

![image-20210618175926907](/Users/zhengwenxuan/Desktop/Coding Pattern/image-20210618175926907.png)

## Solution [#](https://www.educative.io/courses/grokking-the-coding-interview/qVA27MMYYn0#solution)

This problem follows the [Binary Tree Level Order Traversal](https://www.educative.io/collection/page/5668639101419520/5671464854355968/5726607939469312/) pattern. We can follow the same **BFS** approach. The only additional step we have to keep in mind is to alternate the level order traversal, which means that for every other level, we will traverse similar to [Reverse Level Order Traversal](https://www.educative.io/collection/page/5668639101419520/5671464854355968/5765606242516992/).

## Code [#](https://www.educative.io/courses/grokking-the-coding-interview/qVA27MMYYn0#code)

Here is what our algorithm will look like, only the highlighted lines have changed:

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

class ZigzagTraversal {
  public static List<List<Integer>> traverse(TreeNode root) {
    List<List<Integer>> result = new ArrayList<List<Integer>>();
    if (root == null)
      return result;

    Queue<TreeNode> queue = new LinkedList<>();
    queue.offer(root);
    boolean leftToRight = true;
    while (!queue.isEmpty()) {
      int levelSize = queue.size();
      List<Integer> currentLevel = new LinkedList<>();
      for (int i = 0; i < levelSize; i++) {
        TreeNode currentNode = queue.poll();

        // add the node to the current level based on the traverse direction
        if (leftToRight)
          currentLevel.add(currentNode.val);
        else
          currentLevel.add(0, currentNode.val);

        // insert the children of current node in the queue
        if (currentNode.left != null)
          queue.offer(currentNode.left);
        if (currentNode.right != null)
          queue.offer(currentNode.right);
      }
      result.add(currentLevel);
      // reverse the traversal direction
      leftToRight = !leftToRight;
    }

    return result;
  }

  public static void main(String[] args) {
    TreeNode root = new TreeNode(12);
    root.left = new TreeNode(7);
    root.right = new TreeNode(1);
    root.left.left = new TreeNode(9);
    root.right.left = new TreeNode(10);
    root.right.right = new TreeNode(5);
    root.right.left.left = new TreeNode(20);
    root.right.left.right = new TreeNode(17);
    List<List<Integer>> result = ZigzagTraversal.traverse(root);
    System.out.println("Zigzag traversal: " + result);
  }
}
```

### Time complexity [#](https://www.educative.io/courses/grokking-the-coding-interview/qVA27MMYYn0#time-complexity)

The time complexity of the above algorithm is O(N), where ‘N’ is the total number of nodes in the tree. This is due to the fact that we traverse each node once.

### Space complexity [#](https://www.educative.io/courses/grokking-the-coding-interview/qVA27MMYYn0#space-complexity)

The space complexity of the above algorithm will be O(N) as we need to return a list containing the level order traversal. We will also need O(N) space for the queue. Since we can have a maximum of N/2 nodes at any level (this could happen only at the lowest level), therefore we will need O(N) space to store them in the queue.

# Level Averages in a Binary Tree (easy)

## Problem Statement [#](https://www.educative.io/courses/grokking-the-coding-interview/YQWkA2l67GW#problem-statement)

Given a binary tree, populate an array to represent the **averages of all of its levels**.

**Example 1:**

![image-20210618180252335](/Users/zhengwenxuan/Desktop/Coding Pattern/image-20210618180252335.png)

**Example 2:**

![image-20210618180304516](/Users/zhengwenxuan/Desktop/Coding Pattern/image-20210618180304516.png)

## Solution [#](https://www.educative.io/courses/grokking-the-coding-interview/YQWkA2l67GW#solution)

This problem follows the [Binary Tree Level Order Traversal](https://www.educative.io/collection/page/5668639101419520/5671464854355968/5726607939469312/) pattern. We can follow the same **BFS** approach. The only difference will be that instead of keeping track of all nodes of a level, we will only track the running sum of the values of all nodes in each level. In the end, we will append the average of the current level to the result array.

## Code [#](https://www.educative.io/courses/grokking-the-coding-interview/YQWkA2l67GW#code)

Here is what our algorithm will look like; only the highlighted lines have changed:

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

class LevelAverage {
  public static List<Double> findLevelAverages(TreeNode root) {
    List<Double> result = new ArrayList<>();
    if (root == null)
      return result;

    Queue<TreeNode> queue = new LinkedList<>();
    queue.offer(root);
    while (!queue.isEmpty()) {
      int levelSize = queue.size();
      double levelSum = 0;
      for (int i = 0; i < levelSize; i++) {
        TreeNode currentNode = queue.poll();
        // add the node's value to the running sum
        levelSum += currentNode.val;
        // insert the children of current node to the queue
        if (currentNode.left != null)
          queue.offer(currentNode.left);
        if (currentNode.right != null)
          queue.offer(currentNode.right);
      }
      // append the current level's average to the result array
      result.add(levelSum / levelSize);
    }

    return result;
  }

  public static void main(String[] args) {
    TreeNode root = new TreeNode(12);
    root.left = new TreeNode(7);
    root.right = new TreeNode(1);
    root.left.left = new TreeNode(9);
    root.left.right = new TreeNode(2);
    root.right.left = new TreeNode(10);
    root.right.right = new TreeNode(5);
    List<Double> result = LevelAverage.findLevelAverages(root);
    System.out.print("Level averages are: " + result);
  }
}
```

### Time complexity [#](https://www.educative.io/courses/grokking-the-coding-interview/YQWkA2l67GW#time-complexity)

The time complexity of the above algorithm is O(N), where ‘N’ is the total number of nodes in the tree. This is due to the fact that we traverse each node once.

### Space complexity [#](https://www.educative.io/courses/grokking-the-coding-interview/YQWkA2l67GW#space-complexity)

The space complexity of the above algorithm will be O(N) which is required for the queue. Since we can have a maximum of N/2nodes at any level (this could happen only at the lowest level), therefore we will need O(N) space to store them in the queue.

------

## Similar Problems [#](https://www.educative.io/courses/grokking-the-coding-interview/YQWkA2l67GW#similar-problems)

**Problem 1:** Find the largest value on each level of a binary tree.

**Solution:** We will follow a similar approach, but instead of having a running sum we will track the maximum value of each level.

```
maxValue = max(maxValue, currentNode.val)
```

# Minimum Depth of a Binary Tree (easy)

## Problem Statement [#](https://www.educative.io/courses/grokking-the-coding-interview/3jwVx84OMkO#problem-statement)

Find the minimum depth of a binary tree. The minimum depth is the number of nodes along the **shortest path from the root node to the nearest leaf node**.

**Example 1:**

![image-20210618180416789](/Users/zhengwenxuan/Desktop/Coding Pattern/image-20210618180416789.png)

**Example 2:**

![image-20210618180426777](/Users/zhengwenxuan/Desktop/Coding Pattern/image-20210618180426777.png)

## Solution [#](https://www.educative.io/courses/grokking-the-coding-interview/3jwVx84OMkO#solution)

This problem follows the [Binary Tree Level Order Traversal](https://www.educative.io/collection/page/5668639101419520/5671464854355968/5726607939469312/) pattern. We can follow the same **BFS** approach. The only difference will be, instead of keeping track of all the nodes in a level, we will only track the depth of the tree. As soon as we find our first leaf node, that level will represent the minimum depth of the tree.

## Code [#](https://www.educative.io/courses/grokking-the-coding-interview/3jwVx84OMkO#code)

Here is what our algorithm will look like, only the highlighted lines have changed:

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

class MinimumBinaryTreeDepth {
  public static int findDepth(TreeNode root) {
    if (root == null)
      return 0;

    Queue<TreeNode> queue = new LinkedList<>();
    queue.add(root);
    int minimumTreeDepth = 0;
    while (!queue.isEmpty()) {
      minimumTreeDepth++;
      int levelSize = queue.size();
      for (int i = 0; i < levelSize; i++) {
        TreeNode currentNode = queue.poll();

        // check if this is a leaf node
        if (currentNode.left == null && currentNode.right == null)
          return minimumTreeDepth;

        // insert the children of current node in the queue
        if (currentNode.left != null)
          queue.add(currentNode.left);
        if (currentNode.right != null)
          queue.add(currentNode.right);
      }
    }
    return minimumTreeDepth;
  }

  public static void main(String[] args) {
    TreeNode root = new TreeNode(12);
    root.left = new TreeNode(7);
    root.right = new TreeNode(1);
    root.right.left = new TreeNode(10);
    root.right.right = new TreeNode(5);
    System.out.println("Tree Minimum Depth: " + MinimumBinaryTreeDepth.findDepth(root));
    root.left.left = new TreeNode(9);
    root.right.left.left = new TreeNode(11);
    System.out.println("Tree Minimum Depth: " + MinimumBinaryTreeDepth.findDepth(root));
  }
}
```

### Time complexity [#](https://www.educative.io/courses/grokking-the-coding-interview/3jwVx84OMkO#time-complexity)

The time complexity of the above algorithm is O(N), where ‘N’ is the total number of nodes in the tree. This is due to the fact that we traverse each node once.

### Space complexity [#](https://www.educative.io/courses/grokking-the-coding-interview/3jwVx84OMkO#space-complexity)

The space complexity of the above algorithm will be O(N) which is required for the queue. Since we can have a maximum of N/2 nodes at any level (this could happen only at the lowest level), therefore we will need O(N) space to store them in the queue.

------

## Similar Problems [#](https://www.educative.io/courses/grokking-the-coding-interview/3jwVx84OMkO#similar-problems)

**Problem 1:** Given a binary tree, find its maximum depth (or height).

**Solution:** We will follow a similar approach. Instead of returning as soon as we find a leaf node, we will keep traversing for all the levels, incrementing `maximumDepth` each time we complete a level. Here is what the code will look like:

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

class MaximumBinaryTreeDepth {
  public static int findDepth(TreeNode root) {
    if (root == null)
      return 0;

    Queue<TreeNode> queue = new LinkedList<>();
    queue.add(root);
    int maximumTreeDepth = 0;
    while (!queue.isEmpty()) {
      maximumTreeDepth++;
      int levelSize = queue.size();
      for (int i = 0; i < levelSize; i++) {
        TreeNode currentNode = queue.poll();
        // insert the children of current node in the queue
        if (currentNode.left != null)
          queue.add(currentNode.left);
        if (currentNode.right != null)
          queue.add(currentNode.right);
      }
    }

    return maximumTreeDepth;
  }

  public static void main(String[] args) {
    TreeNode root = new TreeNode(12);
    root.left = new TreeNode(7);
    root.right = new TreeNode(1);
    root.right.left = new TreeNode(10);
    root.right.right = new TreeNode(5);
    System.out.println("Tree Maximum Depth: " + MaximumBinaryTreeDepth.findDepth(root));
    root.left.left = new TreeNode(9);
    root.right.left.left = new TreeNode(11);
    System.out.println("Tree Maximum Depth: " + MaximumBinaryTreeDepth.findDepth(root));
  }
}
```

# Level Order Successor (easy)

## Problem Statement 

Given a binary tree and a node, find the level order successor of the given node in the tree. The level order successor is the node that appears right after the given node in the level order traversal.

**Example 1:**

![image-20210618180539888](/Users/zhengwenxuan/Desktop/Coding Pattern/image-20210618180539888.png)



**Example 2:**

![image-20210618180550819](/Users/zhengwenxuan/Desktop/Coding Pattern/image-20210618180550819.png)

**Example 3:**

![image-20210618180600754](/Users/zhengwenxuan/Desktop/Coding Pattern/image-20210618180600754.png)

## Solution [#](https://www.educative.io/courses/grokking-the-coding-interview/7nO4VmA74Lr#solution)

This problem follows the [Binary Tree Level Order Traversal](https://www.educative.io/collection/page/5668639101419520/5671464854355968/5726607939469312/) pattern. We can follow the same **BFS** approach. The only difference will be that we will not keep track of all the levels. Instead we will keep inserting child nodes to the queue. As soon as we find the given node, we will return the next node from the queue as the level order successor.

## Code [#](https://www.educative.io/courses/grokking-the-coding-interview/7nO4VmA74Lr#code)

Here is what our algorithm will look like; most of the changes are in the highlighted lines:

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

class LevelOrderSuccessor {
  public static TreeNode findSuccessor(TreeNode root, int key) {
    if (root == null)
      return null;

    Queue<TreeNode> queue = new LinkedList<>();
    queue.offer(root);
    while (!queue.isEmpty()) {
      TreeNode currentNode = queue.poll();
      // insert the children of current node in the queue
      if (currentNode.left != null)
        queue.offer(currentNode.left);
      if (currentNode.right != null)
        queue.offer(currentNode.right);

      // break if we have found the key
      if (currentNode.val == key)
        break;
    }

    return queue.peek();
  }

  public static void main(String[] args) {
    TreeNode root = new TreeNode(12);
    root.left = new TreeNode(7);
    root.right = new TreeNode(1);
    root.left.left = new TreeNode(9);
    root.right.left = new TreeNode(10);
    root.right.right = new TreeNode(5);
    TreeNode result = LevelOrderSuccessor.findSuccessor(root, 12);
    if (result != null)
      System.out.println(result.val + " ");
    result = LevelOrderSuccessor.findSuccessor(root, 9);
    if (result != null)
      System.out.println(result.val + " ");
  }
}
```

### Time complexity [#](https://www.educative.io/courses/grokking-the-coding-interview/7nO4VmA74Lr#time-complexity)

The time complexity of the above algorithm is O(N), where ‘N’ is the total number of nodes in the tree. This is due to the fact that we traverse each node once.

### Space complexity [#](https://www.educative.io/courses/grokking-the-coding-interview/7nO4VmA74Lr#space-complexity)

The space complexity of the above algorithm will be O(N) which is required for the queue. Since we can have a maximum of N/2 nodes at any level (this could happen only at the lowest level), therefore we will need O(N) space to store them in the queue.

# Connect Level Order Siblings (medium)

## Problem Statement [#](https://www.educative.io/courses/grokking-the-coding-interview/m2YYxXDOJ03#problem-statement)

Given a binary tree, connect each node with its level order successor. The last node of each level should point to a `null` node.

**Example 1:**

![image-20210618180653545](/Users/zhengwenxuan/Desktop/Coding Pattern/image-20210618180653545.png)

**Example 2:**

![image-20210618180707993](/Users/zhengwenxuan/Desktop/Coding Pattern/image-20210618180707993.png)

## Solution [#](https://www.educative.io/courses/grokking-the-coding-interview/m2YYxXDOJ03#solution)

This problem follows the [Binary Tree Level Order Traversal](https://www.educative.io/collection/page/5668639101419520/5671464854355968/5726607939469312/) pattern. We can follow the same **BFS** approach. The only difference is that while traversing a level we will remember the previous node to connect it with the current node.

## Code [#](https://www.educative.io/courses/grokking-the-coding-interview/m2YYxXDOJ03#code)

Here is what our algorithm will look like; only the highlighted lines have changed:

```java
import java.util.*;

class TreeNode {
  int val;
  TreeNode left;
  TreeNode right;
  TreeNode next;

  TreeNode(int x) {
    val = x;
    left = right = next = null;
  }

  // level order traversal using 'next' pointer
  public void printLevelOrder() {
    TreeNode nextLevelRoot = this;
    while (nextLevelRoot != null) {
      TreeNode current = nextLevelRoot;
      nextLevelRoot = null;
      while (current != null) {
        System.out.print(current.val + " ");
        if (nextLevelRoot == null) {
          if (current.left != null)
            nextLevelRoot = current.left;
          else if (current.right != null)
            nextLevelRoot = current.right;
        }
        current = current.next;
      }
      System.out.println();
    }
  }
};

class ConnectLevelOrderSiblings {
  public static void connect(TreeNode root) {
    if (root == null)
      return;

    Queue<TreeNode> queue = new LinkedList<>();
    queue.offer(root);
    while (!queue.isEmpty()) {
      TreeNode previousNode = null;
      int levelSize = queue.size();
      // connect all nodes of this level
      for (int i = 0; i < levelSize; i++) {
        TreeNode currentNode = queue.poll();
        if (previousNode != null)
          previousNode.next = currentNode;
        previousNode = currentNode;

        // insert the children of current node in the queue
        if (currentNode.left != null)
          queue.offer(currentNode.left);
        if (currentNode.right != null)
          queue.offer(currentNode.right);
      }
    }
  }

  public static void main(String[] args) {
    TreeNode root = new TreeNode(12);
    root.left = new TreeNode(7);
    root.right = new TreeNode(1);
    root.left.left = new TreeNode(9);
    root.right.left = new TreeNode(10);
    root.right.right = new TreeNode(5);
    ConnectLevelOrderSiblings.connect(root);
    System.out.println("Level order traversal using 'next' pointer: ");
    root.printLevelOrder();
  }
}
```

### Time complexity [#](https://www.educative.io/courses/grokking-the-coding-interview/m2YYxXDOJ03#time-complexity)

The time complexity of the above algorithm is O(N), where ‘N’ is the total number of nodes in the tree. This is due to the fact that we traverse each node once.

### Space complexity [#](https://www.educative.io/courses/grokking-the-coding-interview/m2YYxXDOJ03#space-complexity)

The space complexity of the above algorithm will be O(N), which is required for the queue. Since we can have a maximum of N/2 nodes at any level (this could happen only at the lowest level), therefore we will need O(N) space to store them in the queue.

# Connect All Level Order Siblings (medium)

Given a binary tree, connect each node with its level order successor. The last node of each level should point to the first node of the next level.

![image-20210618181125505](/Users/zhengwenxuan/Desktop/Coding Pattern/image-20210618181125505.png)

## Solution [#](https://www.educative.io/courses/grokking-the-coding-interview/qVxy1qop77p#solution)

This problem follows the [Binary Tree Level Order Traversal](https://www.educative.io/collection/page/5668639101419520/5671464854355968/5726607939469312/) pattern. We can follow the same **BFS** approach. The only difference will be that while traversing we will remember (irrespective of the level) the previous node to connect it with the current node.

## Code [#](https://www.educative.io/courses/grokking-the-coding-interview/qVxy1qop77p#code)

Here is what our algorithm will look like; only the highlighted lines have changed:

```java
import java.util.*;

class TreeNode {
  int val;
  TreeNode left;
  TreeNode right;
  TreeNode next;

  TreeNode(int x) {
    val = x;
    left = right = next = null;
  }

  // tree traversal using 'next' pointer
  public void printTree() {
    TreeNode current = this;
    System.out.print("Traversal using 'next' pointer: ");
    while (current != null) {
      System.out.print(current.val + " ");
      current = current.next;
    }
  }
};

class ConnectAllSiblings {
  public static void connect(TreeNode root) {
    if (root == null)
      return;

    Queue<TreeNode> queue = new LinkedList<>();
    queue.offer(root);
    TreeNode currentNode = null, previousNode = null;
    while (!queue.isEmpty()) {
      currentNode = queue.poll();
      if (previousNode != null)
        previousNode.next = currentNode;
      previousNode = currentNode;

      // insert the children of current node in the queue
      if (currentNode.left != null)
        queue.offer(currentNode.left);
      if (currentNode.right != null)
        queue.offer(currentNode.right);
    }
  }

  public static void main(String[] args) {
    TreeNode root = new TreeNode(12);
    root.left = new TreeNode(7);
    root.right = new TreeNode(1);
    root.left.left = new TreeNode(9);
    root.right.left = new TreeNode(10);
    root.right.right = new TreeNode(5);
    ConnectAllSiblings.connect(root);
    root.printTree();
  }
}
```

### Time complexity [#](https://www.educative.io/courses/grokking-the-coding-interview/qVxy1qop77p#time-complexity)

The time complexity of the above algorithm is O(N), where ‘N’ is the total number of nodes in the tree. This is due to the fact that we traverse each node once.

### Space complexity [#](https://www.educative.io/courses/grokking-the-coding-interview/qVxy1qop77p#space-complexity)

The space complexity of the above algorithm will be O(N) which is required for the queue. Since we can have a maximum of N/2 nodes at any level (this could happen only at the lowest level), therefore we will need O(N) space to store them in the queue.

# Right View of a Binary Tree (easy)

Given a binary tree, return an array containing nodes in its right view. The right view of a binary tree is the set of **nodes visible when the tree is seen from the right side**.

![image-20210618181240237](/Users/zhengwenxuan/Desktop/Coding Pattern/image-20210618181240237.png)

## Solution [#](https://www.educative.io/courses/grokking-the-coding-interview/gxVWnvZjMn9#solution)

This problem follows the [Binary Tree Level Order Traversal](https://www.educative.io/collection/page/5668639101419520/5671464854355968/5726607939469312/) pattern. We can follow the same **BFS** approach. The only additional thing we will be doing is to append the last node of each level to the result array.

## Code [#](https://www.educative.io/courses/grokking-the-coding-interview/gxVWnvZjMn9#code)

Here is what our algorithm will look like; only the highlighted lines have changed:

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

class RightViewTree {
  public static List<TreeNode> traverse(TreeNode root) {
    List<TreeNode> result = new ArrayList<>();
    if (root == null)
      return result;

    Queue<TreeNode> queue = new LinkedList<>();
    queue.offer(root);
    while (!queue.isEmpty()) {
      int levelSize = queue.size();
      for (int i = 0; i < levelSize; i++) {
        TreeNode currentNode = queue.poll();
        // if it is the last node of this level, add it to the result
        if (i == levelSize - 1)
          result.add(currentNode);
        // insert the children of current node in the queue
        if (currentNode.left != null)
          queue.offer(currentNode.left);
        if (currentNode.right != null)
          queue.offer(currentNode.right);
      }
    }

    return result;
  }

  public static void main(String[] args) {
    TreeNode root = new TreeNode(12);
    root.left = new TreeNode(7);
    root.right = new TreeNode(1);
    root.left.left = new TreeNode(9);
    root.right.left = new TreeNode(10);
    root.right.right = new TreeNode(5);
    root.left.left.left = new TreeNode(3);
    List<TreeNode> result = RightViewTree.traverse(root);
    for (TreeNode node : result) {
      System.out.print(node.val + " ");
    }
  }
}
```

### Time complexity [#](https://www.educative.io/courses/grokking-the-coding-interview/gxVWnvZjMn9#time-complexity)

The time complexity of the above algorithm is O(N), where ‘N’ is the total number of nodes in the tree. This is due to the fact that we traverse each node once.

### Space complexity [#](https://www.educative.io/courses/grokking-the-coding-interview/gxVWnvZjMn9#space-complexity)

The space complexity of the above algorithm will be O(N) as we need to return a list containing the level order traversal. We will also need O(N) space for the queue. Since we can have a maximum of N/2 nodes at any level (this could happen only at the lowest level), therefore we will need O(N) space to store them in the queue.

------

## Similar Questions [#](https://www.educative.io/courses/grokking-the-coding-interview/gxVWnvZjMn9#similar-questions)

**Problem 1:** Given a binary tree, return an array containing nodes in its left view. The left view of a binary tree is the set of nodes visible when the tree is seen from the left side.

**Solution:** We will be following a similar approach, but instead of appending the last element of each level, we will be appending the first element of each level to the output array.

