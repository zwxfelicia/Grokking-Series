# Introduction

This pattern is based on the **Depth First Search (DFS)** technique to traverse a tree.

We will be using recursion (or we can also use a stack for the iterative approach) to keep track of all the previous (parent) nodes while traversing. This also means that the space complexity of the algorithm will be O(H), where ‘H’ is the maximum height of the tree.

Let’s jump onto our first problem to understand this pattern.

# Binary Tree Path Sum (easy)

## Problem Statement [#](https://www.educative.io/courses/grokking-the-coding-interview/RMlGwgpoKKY#problem-statement)

Given a binary tree and a number ‘S’, find if the tree has a path from root-to-leaf such that the sum of all the node values of that path equals ‘S’.

![image-20210619234718711](/Users/zhengwenxuan/Desktop/Grokking-Series/Grokking-the-Coding-Interview-Patterns-for-Coding-Questions/image-20210619234718711.png)



## Solution [#](https://www.educative.io/courses/grokking-the-coding-interview/RMlGwgpoKKY#solution)

As we are trying to search for a root-to-leaf path, we can use the **Depth First Search (DFS)** technique to solve this problem.

To recursively traverse a binary tree in a DFS fashion, we can start from the root and at every step, make two recursive calls one for the left and one for the right child.

Here are the steps for our Binary Tree Path Sum problem:

1. Start DFS with the root of the tree.
2. If the current node is not a leaf node, do two things:
   - Subtract the value of the current node from the given number to get a new sum => `S = S - node.value`
   - Make two recursive calls for both the children of the current node with the new number calculated in the previous step.
3. At every step, see if the current node being visited is a leaf node and if its value is equal to the given number ‘S’. If both these conditions are true, we have found the required root-to-leaf path, therefore return `true`.
4. If the current node is a leaf but its value is not equal to the given number ‘S’, return false.

Let’s take the example-2 mentioned above to visually see our algorithm:

![image-20210619234749067](/Users/zhengwenxuan/Desktop/Grokking-Series/Grokking-the-Coding-Interview-Patterns-for-Coding-Questions/image-20210619234749067.png)

![image-20210619234757778](/Users/zhengwenxuan/Desktop/Grokking-Series/Grokking-the-Coding-Interview-Patterns-for-Coding-Questions/image-20210619234757778.png)

![image-20210619234812316](/Users/zhengwenxuan/Desktop/Grokking-Series/Grokking-the-Coding-Interview-Patterns-for-Coding-Questions/image-20210619234812316.png)

![image-20210619234820412](/Users/zhengwenxuan/Desktop/Grokking-Series/Grokking-the-Coding-Interview-Patterns-for-Coding-Questions/image-20210619234820412.png)

![image-20210619234829627](/Users/zhengwenxuan/Desktop/Grokking-Series/Grokking-the-Coding-Interview-Patterns-for-Coding-Questions/image-20210619234829627.png)

![image-20210619234846180](/Users/zhengwenxuan/Desktop/Grokking-Series/Grokking-the-Coding-Interview-Patterns-for-Coding-Questions/image-20210619234846180.png)

![image-20210619234856665](/Users/zhengwenxuan/Desktop/Grokking-Series/Grokking-the-Coding-Interview-Patterns-for-Coding-Questions/image-20210619234856665.png)

![image-20210619234906169](/Users/zhengwenxuan/Desktop/Grokking-Series/Grokking-the-Coding-Interview-Patterns-for-Coding-Questions/image-20210619234906169.png)

![image-20210619234913987](/Users/zhengwenxuan/Desktop/Grokking-Series/Grokking-the-Coding-Interview-Patterns-for-Coding-Questions/image-20210619234913987.png)

![image-20210619234928649](/Users/zhengwenxuan/Desktop/Grokking-Series/Grokking-the-Coding-Interview-Patterns-for-Coding-Questions/image-20210619234928649.png)

## Code [#](https://www.educative.io/courses/grokking-the-coding-interview/RMlGwgpoKKY#code)

Here is what our algorithm will look like:

```java
class TreeNode {
  int val;
  TreeNode left;
  TreeNode right;

  TreeNode(int x) {
    val = x;
  }
};

class TreePathSum {
  public static boolean hasPath(TreeNode root, int sum) {
    if (root == null)
      return false;

    // if the current node is a leaf and its value is equal to the sum, we've found a path
    if (root.val == sum && root.left == null && root.right == null)
      return true;

    // recursively call to traverse the left and right sub-tree
    // return true if any of the two recursive call return true
    return hasPath(root.left, sum - root.val) || hasPath(root.right, sum - root.val);
  }

  public static void main(String[] args) {
    TreeNode root = new TreeNode(12);
    root.left = new TreeNode(7);
    root.right = new TreeNode(1);
    root.left.left = new TreeNode(9);
    root.right.left = new TreeNode(10);
    root.right.right = new TreeNode(5);
    System.out.println("Tree has path: " + TreePathSum.hasPath(root, 23));
    System.out.println("Tree has path: " + TreePathSum.hasPath(root, 16));
  }
}
```

### Time complexity [#](https://www.educative.io/courses/grokking-the-coding-interview/RMlGwgpoKKY#time-complexity)

The time complexity of the above algorithm is O(N), where ‘N’ is the total number of nodes in the tree. This is due to the fact that we traverse each node once.

### Space complexity [#](https://www.educative.io/courses/grokking-the-coding-interview/RMlGwgpoKKY#space-complexity)

The space complexity of the above algorithm will be O(N) in the worst case. This space will be used to store the recursion stack. The worst case will happen when the given tree is a linked list (i.e., every node has only one child).

# All Paths for a Sum (medium)

## Problem Statement [#](https://www.educative.io/courses/grokking-the-coding-interview/B815A0y2Ajn#problem-statement)

Given a binary tree and a number ‘S’, find all paths from root-to-leaf such that the sum of all the node values of each path equals ‘S’.

![image-20210619234512290](/Users/zhengwenxuan/Desktop/Grokking-Series/Grokking-the-Coding-Interview-Patterns-for-Coding-Questions/image-20210619234512290.png)

## Solution [#](https://www.educative.io/courses/grokking-the-coding-interview/B815A0y2Ajn#solution)

This problem follows the [Binary Tree Path Sum](https://www.educative.io/collection/page/5668639101419520/5671464854355968/5642684278505472/) pattern. We can follow the same **DFS** approach. There will be two differences:

1. Every time we find a root-to-leaf path, we will store it in a list.
2. We will traverse all paths and will not stop processing after finding the first path.

## Code [#](https://www.educative.io/courses/grokking-the-coding-interview/B815A0y2Ajn#code)

Here is what our algorithm will look like:

````java
import java.util.*;

class TreeNode {
  int val;
  TreeNode left;
  TreeNode right;

  TreeNode(int x) {
    val = x;
  }
};

class FindAllTreePaths {
  public static List<List<Integer>> findPaths(TreeNode root, int sum) {
    List<List<Integer>> allPaths = new ArrayList<>();
    List<Integer> currentPath = new ArrayList<Integer>();
    findPathsRecursive(root, sum, currentPath, allPaths);
    return allPaths;
  }

  private static void findPathsRecursive(TreeNode currentNode, int sum, List<Integer> currentPath,
      List<List<Integer>> allPaths) {
    if (currentNode == null)
      return;

    // add the current node to the path
    currentPath.add(currentNode.val);

    // if the current node is a leaf and its value is equal to sum, save the current path
    if (currentNode.val == sum && currentNode.left == null && currentNode.right == null) {
      allPaths.add(new ArrayList<Integer>(currentPath));
    } else {
      // traverse the left sub-tree
      findPathsRecursive(currentNode.left, sum - currentNode.val, currentPath, allPaths);
      // traverse the right sub-tree
      findPathsRecursive(currentNode.right, sum - currentNode.val, currentPath, allPaths);
    }

    // remove the current node from the path to backtrack, 
    // we need to remove the current node while we are going up the recursive call stack.
    currentPath.remove(currentPath.size() - 1);
  }

  public static void main(String[] args) {
    TreeNode root = new TreeNode(12);
    root.left = new TreeNode(7);
    root.right = new TreeNode(1);
    root.left.left = new TreeNode(4);
    root.right.left = new TreeNode(10);
    root.right.right = new TreeNode(5);
    int sum = 23;
    List<List<Integer>> result = FindAllTreePaths.findPaths(root, sum);
    System.out.println("Tree paths with sum " + sum + ": " + result);
  }
}
````

### Time complexity [#](https://www.educative.io/courses/grokking-the-coding-interview/B815A0y2Ajn#time-complexity)

The time complexity of the above algorithm is O(N^2)), where ‘N’ is the total number of nodes in the tree. This is due to the fact that we traverse each node once (which will take O(N)), and for every leaf node, we might have to store its path (by making a copy of the current path) which will take O(N).

We can calculate a tighter time complexity of O(NlogN) from the space complexity discussion below.

### Space complexity [#](https://www.educative.io/courses/grokking-the-coding-interview/B815A0y2Ajn#space-complexity)

If we ignore the space required for the `allPaths` list, the space complexity of the above algorithm will be O(N)) in the worst case. This space will be used to store the recursion stack. The worst-case will happen when the given tree is a linked list (i.e., every node has only one child).

How can we estimate the space used for the `allPaths` array? Take the example of the following balanced tree:

![image-20210619234604215](/Users/zhengwenxuan/Desktop/Grokking-Series/Grokking-the-Coding-Interview-Patterns-for-Coding-Questions/image-20210619234604215.png)

Here we have seven nodes (i.e., `N = 7`). Since, for binary trees, there exists only one path to reach any leaf node, we can easily say that total root-to-leaf paths in a binary tree can’t be more than the number of leaves. As we know that there can’t be more than (N+1)/2(*N*+1)/2 leaves in a binary tree, therefore the maximum number of elements in `allPaths` will be O((N+1)/2) = O(N)*O*((*N*+1)/2)=*O*(*N*). Now, each of these paths can have many nodes in them. For a balanced binary tree (like above), each leaf node will be at maximum depth. As we know that the depth (or height) of a balanced binary tree is O(logN)*O*(*l**o**g**N*) we can say that, at the most, each path can have logN*l**o**g**N* nodes in it. This means that the total size of the allPaths list will be O(N*logN)*O*(*N*∗*l**o**g**N*). If the tree is not balanced, we will still have the same worst-case space complexity.

From the above discussion, we can conclude that our algorithm’s overall space complexity is O(N*logN)*O*(*N*∗*l**o**g**N*).

Also, from the above discussion, since for each leaf node, in the worst case, we have to copy log(N)*l**o**g*(*N*) nodes to store its path; therefore, the time complexity of our algorithm will also be O(N*logN)*O*(*N*∗*l**o**g**N*).

------

## Similar Problems [#](https://www.educative.io/courses/grokking-the-coding-interview/B815A0y2Ajn#similar-problems)

**Problem 1:** Given a binary tree, return all root-to-leaf paths.

*Solution:* We can follow a similar approach. We just need to remove the “check for the path sum.”

**Problem 2:** Given a binary tree, find the root-to-leaf path with the maximum sum.

*Solution:* We need to find the path with the maximum sum. As we traverse all paths, we can keep track of the path with the maximum sum.

# Sum of Path Numbers (medium)

## Problem Statement [#](https://www.educative.io/courses/grokking-the-coding-interview/YQ5o5vEXP69#problem-statement)

Given a binary tree where each node can only have a digit (0-9) value, each root-to-leaf path will represent a number. Find the total sum of all the numbers represented by all paths.

![image-20210620162126891](/Users/zhengwenxuan/Desktop/Grokking-Series/Grokking-the-Coding-Interview-Patterns-for-Coding-Questions/image-20210620162126891.png)

## Solution [#](https://www.educative.io/courses/grokking-the-coding-interview/YQ5o5vEXP69#solution)

This problem follows the [Binary Tree Path Sum](https://www.educative.io/collection/page/5668639101419520/5671464854355968/5642684278505472/) pattern. We can follow the same **DFS** approach. The additional thing we need to do is to keep track of the number representing the current path.

How do we calculate the path number for a node? Taking the first example mentioned above, say we are at node ‘7’. As we know, the path number for this node is ‘17’, which was calculated by: `1 * 10 + 7 => 17`. We will follow the same approach to calculate the path number of each node.

## Code [#](https://www.educative.io/courses/grokking-the-coding-interview/YQ5o5vEXP69#code)

Here is what our algorithm will look like:

```java
class TreeNode {
  int val;
  TreeNode left;
  TreeNode right;

  TreeNode(int x) {
    val = x;
  }
};

class SumOfPathNumbers {
  public static int findSumOfPathNumbers(TreeNode root) {
    return findRootToLeafPathNumbers(root, 0);
  }

  private static int findRootToLeafPathNumbers(TreeNode currentNode, int pathSum) {
    if (currentNode == null)
      return 0;

    // calculate the path number of the current node
    pathSum = 10 * pathSum + currentNode.val;

    // if the current node is a leaf, return the current path sum.
    if (currentNode.left == null && currentNode.right == null) {
      return pathSum;
    }

    // traverse the left and the right sub-tree
    return findRootToLeafPathNumbers(currentNode.left, pathSum) +
           findRootToLeafPathNumbers(currentNode.right, pathSum);
  }

  public static void main(String[] args) {
    TreeNode root = new TreeNode(1);
    root.left = new TreeNode(0);
    root.right = new TreeNode(1);
    root.left.left = new TreeNode(1);
    root.right.left = new TreeNode(6);
    root.right.right = new TreeNode(5);
    System.out.println("Total Sum of Path Numbers: " + SumOfPathNumbers.findSumOfPathNumbers(root));
  }
}
```

### Time complexity [#](https://www.educative.io/courses/grokking-the-coding-interview/YQ5o5vEXP69#time-complexity)

The time complexity of the above algorithm is O(N), where ‘N’ is the total number of nodes in the tree. This is due to the fact that we traverse each node once.

### Space complexity [#](https://www.educative.io/courses/grokking-the-coding-interview/YQ5o5vEXP69#space-complexity)

The space complexity of the above algorithm will be O(N) in the worst case. This space will be used to store the recursion stack. The worst case will happen when the given tree is a linked list (i.e., every node has only one child).

# Path With Given Sequence (medium)

## Problem Statement [#](https://www.educative.io/courses/grokking-the-coding-interview/m280XNlPOkn#problem-statement)

Given a binary tree and a number sequence, find if the sequence is present as a root-to-leaf path in the given tree.

![image-20210620162225541](/Users/zhengwenxuan/Desktop/Grokking-Series/Grokking-the-Coding-Interview-Patterns-for-Coding-Questions/image-20210620162225541.png)

## Solution [#](https://www.educative.io/courses/grokking-the-coding-interview/m280XNlPOkn#solution)

This problem follows the [Binary Tree Path Sum](https://www.educative.io/collection/page/5668639101419520/5671464854355968/5642684278505472/) pattern. We can follow the same **DFS** approach and additionally, track the element of the given sequence that we should match with the current node. Also, we can return `false` as soon as we find a mismatch between the sequence and the node value.

## Code [#](https://www.educative.io/courses/grokking-the-coding-interview/m280XNlPOkn#code)

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

class PathWithGivenSequence {
  public static boolean findPath(TreeNode root, int[] sequence) {
    if (root == null)
      return sequence.length == 0;

    return findPathRecursive(root, sequence, 0);
  }

  private static boolean findPathRecursive(TreeNode currentNode, int[] sequence, int sequenceIndex) {

    if (currentNode == null)
      return false;

    if (sequenceIndex >= sequence.length || currentNode.val != sequence[sequenceIndex])
      return false;

    // if the current node is a leaf, add it is the end of the sequence, we have found a path!
    if (currentNode.left == null && currentNode.right == null && sequenceIndex == sequence.length - 1)
      return true;

    // recursively call to traverse the left and right sub-tree
    // return true if any of the two recusrive call return true
    return findPathRecursive(currentNode.left, sequence, sequenceIndex + 1)
        || findPathRecursive(currentNode.right, sequence, sequenceIndex + 1);
  }

  public static void main(String[] args) {
    TreeNode root = new TreeNode(1);
    root.left = new TreeNode(0);
    root.right = new TreeNode(1);
    root.left.left = new TreeNode(1);
    root.right.left = new TreeNode(6);
    root.right.right = new TreeNode(5);

    System.out.println("Tree has path sequence: " + PathWithGivenSequence.findPath(root, new int[] { 1, 0, 7 }));
    System.out.println("Tree has path sequence: " + PathWithGivenSequence.findPath(root, new int[] { 1, 1, 6 }));
  }
}
```

### Time complexity [#](https://www.educative.io/courses/grokking-the-coding-interview/m280XNlPOkn#time-complexity)

The time complexity of the above algorithm is O(N), where ‘N’ is the total number of nodes in the tree. This is due to the fact that we traverse each node once.

### Space complexity [#](https://www.educative.io/courses/grokking-the-coding-interview/m280XNlPOkn#space-complexity)

The space complexity of the above algorithm will be O(N) in the worst case. This space will be used to store the recursion stack. The worst case will happen when the given tree is a linked list (i.e., every node has only one child).

# Count Paths for a Sum (medium)

## Problem Statement [#](https://www.educative.io/courses/grokking-the-coding-interview/xV2J7jvN1or#problem-statement)

Given a binary tree and a number ‘S’, find all paths in the tree such that the sum of all the node values of each path equals ‘S’. Please note that the paths can start or end at any node but all paths must follow direction from parent to child (top to bottom).

![image-20210620162404289](/Users/zhengwenxuan/Desktop/Grokking-Series/Grokking-the-Coding-Interview-Patterns-for-Coding-Questions/image-20210620162404289.png)

## Solution [#](https://www.educative.io/courses/grokking-the-coding-interview/xV2J7jvN1or#solution)

This problem follows the [Binary Tree Path Sum](https://www.educative.io/collection/page/5668639101419520/5671464854355968/5642684278505472/) pattern. We can follow the same **DFS** approach. But there will be four differences:

1. We will keep track of the current path in a list which will be passed to every recursive call.
2. Whenever we traverse a node we will do two things:
   - Add the current node to the current path.
   - As we added a new node to the current path, we should find the sums of all sub-paths ending at the current node. If the sum of any sub-path is equal to ‘S’ we will increment our path count.
3. We will traverse all paths and will not stop processing after finding the first path.
4. Remove the current node from the current path before returning from the function. This is needed to **Backtrack** while we are going up the recursive call stack to process other paths.

## Code [#](https://www.educative.io/courses/grokking-the-coding-interview/xV2J7jvN1or#code)

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

class CountAllPathSum {
  public static int countPaths(TreeNode root, int S) {
    List<Integer> currentPath = new LinkedList<>();
    return countPathsRecursive(root, S, currentPath);
  }

  private static int countPathsRecursive(TreeNode currentNode, int S, List<Integer> currentPath) {
    if (currentNode == null)
      return 0;

    // add the current node to the path
    currentPath.add(currentNode.val);
    int pathCount = 0, pathSum = 0;
    // find the sums of all sub-paths in the current path list
    ListIterator<Integer> pathIterator = currentPath.listIterator(currentPath.size());
    while (pathIterator.hasPrevious()) {
      pathSum += pathIterator.previous();
      // if the sum of any sub-path is equal to 'S' we increment our path count.
      if (pathSum == S) {
        pathCount++;
      }
    }

    // traverse the left sub-tree
    pathCount += countPathsRecursive(currentNode.left, S, currentPath);
    // traverse the right sub-tree
    pathCount += countPathsRecursive(currentNode.right, S, currentPath);

    // remove the current node from the path to backtrack, 
    // we need to remove the current node while we are going up the recursive call stack.
    currentPath.remove(currentPath.size() - 1);

    return pathCount;
  }

  public static void main(String[] args) {
    TreeNode root = new TreeNode(12);
    root.left = new TreeNode(7);
    root.right = new TreeNode(1);
    root.left.left = new TreeNode(4);
    root.right.left = new TreeNode(10);
    root.right.right = new TreeNode(5);
    System.out.println("Tree has path: " + CountAllPathSum.countPaths(root, 11));
  }
}
```

### Time complexity [#](https://www.educative.io/courses/grokking-the-coding-interview/xV2J7jvN1or#time-complexity)

The time complexity of the above algorithm is O(N^2) in the worst case, where ‘N’ is the total number of nodes in the tree. This is due to the fact that we traverse each node once, but for every node, we iterate the current path. The current path, in the worst case, can be O(N) (in the case of a skewed tree). But, if the tree is balanced, then the current path will be equal to the height of the tree, i.e., O(logN). So the best case of our algorithm will be O(NlogN).

### Space complexity [#](https://www.educative.io/courses/grokking-the-coding-interview/xV2J7jvN1or#space-complexity)

The space complexity of the above algorithm will be O(N). This space will be used to store the recursion stack. The worst case will happen when the given tree is a linked list (i.e., every node has only one child). We also need O(N) space for storing the `currentPath` in the worst case.

Overall space complexity of our algorithm is O(N).

# Tree Diameter (medium) [#](https://www.educative.io/courses/grokking-the-coding-interview/xVV1jA29YK9#tree-diameter-medium)

Given a binary tree, find the length of its diameter. The diameter of a tree is the number of nodes on the **longest path between any two leaf nodes**. The diameter of a tree may or may not pass through the root.

Note: You can always assume that there are at least two leaf nodes in the given tree.

![image-20210620162534731](/Users/zhengwenxuan/Desktop/Grokking-Series/Grokking-the-Coding-Interview-Patterns-for-Coding-Questions/image-20210620162534731.png)

## Solution [#](https://www.educative.io/courses/grokking-the-coding-interview/JYVW7l2L4EJ#solution)

This problem follows the [Binary Tree Path Sum](https://www.educative.io/collection/page/5668639101419520/5671464854355968/5642684278505472/) pattern. We can follow the same **DFS** approach. There will be a few differences:

1. At every step, we need to find the height of both children of the current node. For this, we will make two recursive calls similar to **DFS**.
2. The height of the current node will be equal to the maximum of the heights of its left or right children, plus ‘1’ for the current node.
3. The tree diameter at the current node will be equal to the height of the left child plus the height of the right child plus ‘1’ for the current node: `diameter = leftTreeHeight + rightTreeHeight + 1`. To find the overall tree diameter, we will use a class level variable. This variable will store the maximum diameter of all the nodes visited so far, hence, eventually, it will have the final tree diameter.

## Code [#](https://www.educative.io/courses/grokking-the-coding-interview/JYVW7l2L4EJ#code)

Here is what our algorithm will look like:

```java
class TreeNode {
  int val;
  TreeNode left;
  TreeNode right;

  TreeNode(int x) {
    val = x;
  }
};

class TreeDiameter {

  private static int treeDiameter = 0;

  public static int findDiameter(TreeNode root) {
    calculateHeight(root);
    return treeDiameter;
  }

  private static int calculateHeight(TreeNode currentNode) {
    if (currentNode == null)
      return 0;

    int leftTreeHeight = calculateHeight(currentNode.left);
    int rightTreeHeight = calculateHeight(currentNode.right);

    // if the current node doesn't have a left or right subtree, we can't have
    // a path passing through it, since we need a leaf node on each side
    if (leftTreeHeight != 0 && rightTreeHeight != 0) {

      // diameter at the current node will be equal to the height of left subtree +
      // the height of right sub-trees + '1' for the current node
      int diameter = leftTreeHeight + rightTreeHeight + 1;

      // update the global tree diameter
      treeDiameter = Math.max(treeDiameter, diameter);
    }

    // height of the current node will be equal to the maximum of the heights of
    // left or right subtrees plus '1' for the current node
    return Math.max(leftTreeHeight, rightTreeHeight) + 1;
  }

  public static void main(String[] args) {
    TreeNode root = new TreeNode(1);
    root.left = new TreeNode(2);
    root.right = new TreeNode(3);
    root.left.left = new TreeNode(4);
    root.right.left = new TreeNode(5);
    root.right.right = new TreeNode(6);
    System.out.println("Tree Diameter: " + TreeDiameter.findDiameter(root));
    root.left.left = null;
    root.right.left.left = new TreeNode(7);
    root.right.left.right = new TreeNode(8);
    root.right.right.left = new TreeNode(9);
    root.right.left.right.left = new TreeNode(10);
    root.right.right.left.left = new TreeNode(11);
    System.out.println("Tree Diameter: " + TreeDiameter.findDiameter(root));
  }
}
```

### Time complexity [#](https://www.educative.io/courses/grokking-the-coding-interview/JYVW7l2L4EJ#time-complexity)

The time complexity of the above algorithm is O(N), where ‘N’ is the total number of nodes in the tree. This is due to the fact that we traverse each node once.

### Space complexity [#](https://www.educative.io/courses/grokking-the-coding-interview/JYVW7l2L4EJ#space-complexity)

The space complexity of the above algorithm will be O(N) in the worst case. This space will be used to store the recursion stack. The worst case will happen when the given tree is a linked list (i.e., every node has only one child).

# Path with Maximum Sum (hard) 

Find the path with the maximum sum in a given binary tree. Write a function that returns the maximum sum.

A path can be defined as a **sequence of nodes between any two nodes** and doesn’t necessarily pass through the root. The path must contain at least one node.

![image-20210620162646200](/Users/zhengwenxuan/Desktop/Grokking-Series/Grokking-the-Coding-Interview-Patterns-for-Coding-Questions/image-20210620162646200.png)

## Solution [#](https://www.educative.io/courses/grokking-the-coding-interview/xVPgnOvWVJq#solution)

This problem follows the [Binary Tree Path Sum](https://www.educative.io/collection/page/5668639101419520/5671464854355968/5642684278505472/) pattern and shares the algorithmic logic with [Tree Diameter](https://www.educative.io/collection/page/5668639101419520/5671464854355968/5691878833913856/). We can follow the same **DFS** approach. The only difference will be to ignore the paths with negative sums. Since we need to find the overall maximum sum, we should ignore any path which has an overall negative sum.

## Code [#](https://www.educative.io/courses/grokking-the-coding-interview/xVPgnOvWVJq#code)

Here is what our algorithm will look like, the most important changes are in the highlighted lines:

```java
class TreeNode {
  int val;
  TreeNode left;
  TreeNode right;

  TreeNode(int x) {
    val = x;
  }
};

class MaximumPathSum {

  private static int globalMaximumSum;

  public static int findMaximumPathSum(TreeNode root) {
    globalMaximumSum = Integer.MIN_VALUE;
    findMaximumPathSumRecursive(root);
    return globalMaximumSum;
  }

  private static int findMaximumPathSumRecursive(TreeNode currentNode) {
    if (currentNode == null)
      return 0;

    int maxPathSumFromLeft = findMaximumPathSumRecursive(currentNode.left);
    int maxPathSumFromRight = findMaximumPathSumRecursive(currentNode.right);

    // ignore paths with negative sums, since we need to find the maximum sum we should
    // ignore any path which has an overall negative sum.
    maxPathSumFromLeft = Math.max(maxPathSumFromLeft, 0);
    maxPathSumFromRight = Math.max(maxPathSumFromRight, 0);

    // maximum path sum at the current node will be equal to the sum from the left subtree +
    // the sum from right subtree + val of current node
    int localMaximumSum = maxPathSumFromLeft + maxPathSumFromRight + currentNode.val;

    // update the global maximum sum
    globalMaximumSum = Math.max(globalMaximumSum, localMaximumSum);

    // maximum sum of any path from the current node will be equal to the maximum of 
    // the sums from left or right subtrees plus the value of the current node
    return Math.max(maxPathSumFromLeft, maxPathSumFromRight) + currentNode.val;
  }

  public static void main(String[] args) {
    TreeNode root = new TreeNode(1);
    root.left = new TreeNode(2);
    root.right = new TreeNode(3);
    System.out.println("Maximum Path Sum: " + MaximumPathSum.findMaximumPathSum(root));
    
    root.left.left = new TreeNode(1);
    root.left.right = new TreeNode(3);
    root.right.left = new TreeNode(5);
    root.right.right = new TreeNode(6);
    root.right.left.left = new TreeNode(7);
    root.right.left.right = new TreeNode(8);
    root.right.right.left = new TreeNode(9);
    System.out.println("Maximum Path Sum: " + MaximumPathSum.findMaximumPathSum(root));
    
    root = new TreeNode(-1);
    root.left = new TreeNode(-3);
    System.out.println("Maximum Path Sum: " + MaximumPathSum.findMaximumPathSum(root));
  }
}
```

### Time complexity [#](https://www.educative.io/courses/grokking-the-coding-interview/xVPgnOvWVJq#time-complexity)

The time complexity of the above algorithm is O(N), where ‘N’ is the total number of nodes in the tree. This is due to the fact that we traverse each node once.

### Space complexity [#](https://www.educative.io/courses/grokking-the-coding-interview/xVPgnOvWVJq#space-complexity)

The space complexity of the above algorithm will be O(N) in the worst case. This space will be used to store the recursion stack. The worst case will happen when the given tree is a linked list (i.e., every node has only one child).

