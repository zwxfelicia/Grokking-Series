# What is Dynamic Programming?

Dynamic Programming (DP) is an algorithmic technique for solving an optimization problem by breaking it down into simpler subproblems and utilizing the fact that the optimal solution to the overall problem depends upon the optimal solution to its subproblems.

Let’s take the example of the **Fibonacci numbers**. As we all know, Fibonacci numbers are a series of numbers in which each number is the sum of the two preceding numbers. The first few Fibonacci numbers are 0, 1, 1, 2, 3, 5, and 8, and they continue on from there.

If we are asked to calculate the nth Fibonacci number, we can do that with the following equation,

```
Fib(n) = Fib(n-1) + Fib(n-2), for n > 1
```

As we can clearly see here, to solve the overall problem (i.e. `Fib(n)`), we broke it down into two smaller subproblems (which are `Fib(n-1)` and `Fib(n-2)`). This shows that we can use DP to solve this problem.

## Characteristics of Dynamic Programming [#](https://www.educative.io/courses/grokking-dynamic-programming-patterns-for-coding-interviews/m2G1pAq0OO0#characteristics-of-dynamic-programming)

Before moving on to understand different methods of solving a DP problem, let’s first take a look at what are the characteristics of a problem that tells us that we can apply DP to solve it.

### 1. Overlapping Subproblems [#](https://www.educative.io/courses/grokking-dynamic-programming-patterns-for-coding-interviews/m2G1pAq0OO0#1-overlapping-subproblems)

Subproblems are smaller versions of the original problem. Any problem has overlapping sub-problems if finding its solution involves solving the same subproblem multiple times. Take the example of the Fibonacci numbers; to find the `fib(4)`, we need to break it down into the following sub-problems:

![image-20210707163002475](/Users/zhengwenxuan/Desktop/Grokking-Series/Grokking Dynamic Programming Patterns for Coding Interviews/image-20210707163002475.png)

We can clearly see the overlapping subproblem pattern here, as `fib(2)` has been evaluated twice and `fib(1)` has been evaluated three times.

### 2. Optimal Substructure Property [#](https://www.educative.io/courses/grokking-dynamic-programming-patterns-for-coding-interviews/m2G1pAq0OO0#2-optimal-substructure-property)

Any problem has optimal substructure property if its overall optimal solution can be constructed from the optimal solutions of its subproblems. For Fibonacci numbers, as we know,

```
Fib(n) = Fib(n-1) + Fib(n-2)
```

This clearly shows that a problem of size ‘n’ has been reduced to subproblems of size ‘n-1’ and ‘n-2’. Therefore, Fibonacci numbers have optimal substructure property.

## Dynamic Programming Methods [#](https://www.educative.io/courses/grokking-dynamic-programming-patterns-for-coding-interviews/m2G1pAq0OO0#dynamic-programming-methods)

DP offers two methods to solve a problem.

### 1. Top-down with Memoization [#](https://www.educative.io/courses/grokking-dynamic-programming-patterns-for-coding-interviews/m2G1pAq0OO0#1-top-down-with-memoization)

In this approach, we try to solve the bigger problem by recursively finding the solution to smaller sub-problems. Whenever we solve a sub-problem, we cache its result so that we don’t end up solving it repeatedly if it’s called multiple times. Instead, we can just return the saved result. This technique of storing the results of already solved subproblems is called **Memoization**.

We’ll see this technique in our example of Fibonacci numbers. First, let’s see the non-DP recursive solution for finding the nth Fibonacci number:

```java
class Fibonacci {

  public int CalculateFibonacci(int n) {
    if(n < 2)
      return n;
    return CalculateFibonacci(n-1) + CalculateFibonacci(n-2);
  }

  public static void main(String[] args) {
    Fibonacci fib = new Fibonacci();
    System.out.println("5th Fibonacci is ---> " + fib.CalculateFibonacci(5));
    System.out.println("6th Fibonacci is ---> " + fib.CalculateFibonacci(6));
    System.out.println("7th Fibonacci is ---> " + fib.CalculateFibonacci(7));
  }
}
```

As we saw above, this problem shows the overlapping subproblems pattern, so let’s make use of memoization here. We can use an array to store the already solved subproblems (see the changes in the highlighted lines).

```java
class Fibonacci {

  public int CalculateFibonacci(int n) {
    int memoize[] = new int[n+1];
    return CalculateFibonacciRecursive(memoize, n);
  }

  public int CalculateFibonacciRecursive(int[] memoize, int n) {
    if(n < 2)
      return n;

    // if we have already solved this subproblem, simply return the result from the cache
    if(memoize[n] != 0)
      return memoize[n];

    memoize[n] = CalculateFibonacciRecursive(memoize, n-1) + CalculateFibonacciRecursive(memoize, n-2);
    return memoize[n];
  }

  public static void main(String[] args) {
    Fibonacci fib = new Fibonacci();
    System.out.println("5th Fibonacci is ---> " + fib.CalculateFibonacci(5));
    System.out.println("6th Fibonacci is ---> " + fib.CalculateFibonacci(6));
    System.out.println("7th Fibonacci is ---> " + fib.CalculateFibonacci(7));
  }
}
```

### 2. Bottom-up with Tabulation [#](https://www.educative.io/courses/grokking-dynamic-programming-patterns-for-coding-interviews/m2G1pAq0OO0#2-bottom-up-with-tabulation)

Tabulation is the opposite of the top-down approach and avoids recursion. In this approach, we solve the problem “bottom-up” (i.e. by solving all the related sub-problems first). This is typically done by filling up an n-dimensional table. Based on the results in the table, the solution to the top/original problem is then computed.

Tabulation is the opposite of Memoization, as in Memoization we solve the problem and maintain a map of already solved sub-problems. In other words, in memoization, we do it top-down in the sense that we solve the top problem first (which typically recurses down to solve the sub-problems).

Let’s apply Tabulation to our example of Fibonacci numbers. Since we know that every Fibonacci number is the sum of the two preceding numbers, we can use this fact to populate our table.

Here is the code for our bottom-up dynamic programming approach:

```java
class Fibonacci {

  public int CalculateFibonacci(int n) {
    if (n==0) return 0;
    int dp[] = new int[n+1];

    //base cases
    dp[0] = 0;
    dp[1] = 1;

    for(int i=2; i<=n; i++)
      dp[i] = dp[i-1] + dp[i-2];

    return dp[n];
  }

  public static void main(String[] args) {
    Fibonacci fib = new Fibonacci();
    System.out.println("5th Fibonacci is ---> " + fib.CalculateFibonacci(5));
    System.out.println("6th Fibonacci is ---> " + fib.CalculateFibonacci(6));
    System.out.println("7th Fibonacci is ---> " + fib.CalculateFibonacci(7));
  }
}
```

**In this course, we will always start with a brute-force recursive solution, which is the best way to start solving any DP problem!** Once we have a recursive solution then we will apply Memoization and Tabulation techniques.

Let’s apply this knowledge to solve some of the frequently asked DP problems.

