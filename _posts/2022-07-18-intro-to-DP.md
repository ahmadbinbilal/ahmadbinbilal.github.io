---
title: A Gentle Introduction to Dynamic Programming
subtitle: An introduction to elementary DP concepts for absolute beginners. 
layout: default
date: 2022-07-18
keywords: dynamic programming, competitive programming, algorithms, DSA
published: true
usemathjax: true
---

## Setting the Stage

Most brute force algorithms that we have written until now are quite boring. They do the same thing over and over again, recursively running some calculations until they find the answer. Such algorithms are usually the simplest solution to problems, and as it is with many simple solutions to problems, they are quite often inefficient. Our objective with dynamic programming is to introduce a style of programming in which we utilize our previous calculations to speed up our algorithm. It may sound simple because it really is very simple. 

Essentially what we do is we split up our problem into smaller subproblems that can easily be solved. Then, whenever we solve one of these subproblems, we remember the answer so that we can use it later on to solve more subproblems. Let's take a look at the most common example when it comes to dynamic programming: finding the nth Fibonacci number.

## Finding the Nth Fibonacci number

For the uninitiated, the Fibonacci numbers are a series of numbers, starting from 0 and 1, in which each number is the sum of the two numbers before it. Mathematically, the sequence can be defined as

$$ F_{0} = 0 $$

$$ F_{1} = 1 $$

$$ F_{n} = F_{n-1} + F_{n-2} $$

Walking through an example, the third Fibonacci number would be the sum of 0 and 1 which is 1. Similarly, the fourth Fibonacci number would be the sum of 1 and 1, which is 2. Continuing the pattern we get an infinite sequence of numbers that look like so:

$$ 0, 1, 1, 2, 3, 5, 8, 13, 21, 34, 55, 89, ... $$

The problem at hand is to find the nth Fibonacci number. Before we dive into how we could solve this with dynamic programming, let's try to solve this with a brute force algorithm. 

A naïve solution to this problem is to create a recursive function that returns the `n`th Fibonacci number by taking the sum of the previous two Fibonacci numbers. Our base case for the function will be the first and second Fibonacci numbers which we know are 0 and 1 respectively. Let's implement this in python.

{:.code}

```python
function fib(n):
  if(n<=2):
    return n-1 #returns 0 and 1 for fib(1) and fib(2) respectively
  return fib(n-1) + fib(n-2)
```

This solution works. If we input 5, the `fib` function will call `fib(4)` and `fib(3)` summing their results. `fib(4)` and `fib(3)` will also do the same, calling and summing their predecessors until we reach our base case. 

## Memoization

The algorithm that we have just implemented is, although straighforward, not very efficient. To get a feel for this we can draw a diagram of all the recursive calls to `fib()`

<div class='figure'>
    <img src="/assets/images/intro-to-dp/1.png"
         style="height: 200px; display: block; margin: 0 auto;"/>
    <div class='caption'>
        <span class='caption-label'>Figure 1.</span> A graphical representation of the brute force algorithm in which each node represents a call to the fib function with that input.
    </div>
</div>

This tree does seem a bit scary in terms of time complexity, but I do see a way we can make this algorithm much more efficient. Notice how some nodes are being repeated which means that sometimes `fib()` is being called on the same input over and over again.  This is really bad because not only is `fib()` getting called again but all of its children recursive calls as well. In fact, as n gets larger and larger, this happens more and more frequently. Why don't we store all the return values from `fib()` in one big n-sized array so that we can look up our answer if we have computed it before? For example whenever we compute `fib(x)` we can store the result in `array[x]`. Now we can look up our answer in `O(1)` time before each recursive call. This process of storing results of subproblems in an array to prevent needlessly computing the results of those subproblems again is known as memoization (deriving its name from the idea of noting down our answer on a 'memo notepad'). We just have to add a few lines of code to implement this. 

{:.code}

```python
def fib(n, memo):
  if(n<=2):
    return n-1 #returns 0 and 1 for fib(1) and fib(2) respectively
  if(memo[n] == -1):
    memo[n] = fib(n-1, memo) + fib(n-2, memo)
  return memo[n]
```

If you think about it, this algorithm takes `O(n)` time since we call `fib()` on every number less than the initial n and we never repeat calls on the same input. This is much better, but there is still room for some slight improvements in space complexity.

## Tabulation

As suggested by the previous sentence, the space complexity of our algorithm is still not optimal. You may notice that with this solution we are effectively working our way down the Fibonacci numbers until we get to the base case. However, there isn't anything stopping us from going the other way, working our way up from the base case until we reach the value of `fib(n)`. We can do that since we know:

$$ F_{n} = F_{n-1} + F_{n-2} $$

$$ \therefore $$ 

$$ F_{n+1} = F_{n} + F_{n-1} $$

We can initialize an array that we fill first with the initial values of 0 and 1. Then we can iteratively append the sum of the last 2 elements in the array as per the second equation above. Now, we have a solution with `O(n)` time complexity and `O(n)` space complexity. Awesome.  This method is known as tabulation since we are 'filling up a table', in a bottom-up style until we get our answer.  

{:.code}

```python
def fib(n):
  arr = [0,1]
  for i in range(n+1):
    arr.append(arr[i-1], arr[i-2])
  return table[-1]
```

## Wrapping up

Hopefully, these two concepts will help you understand where to start with dynamic programming problems, however dynamic programming is a concept that can only truly be learnt through practice. Only through solving various problems can you really understand when and how to use these tools correctly. 

As a general rule of thumb, I find it easiest to find memorization solutions to problems by first dividing my problem into subproblems which can be solved by brute force (often a byproduct of coming up with a brute force solution). Then I can go ahead and memorize the solution, finding where calculations are being repeated and storing them in some list. Tabulation is similar. I like to first divide the problem into subproblems and then figure out if one's answer is simply a small update away from the previous answer. Again, easier said then done. 