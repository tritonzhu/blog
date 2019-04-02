---
title: "Two Sum and some Follow-up Problems"
date: 2019-03-27
markup: mmark
tags:
 - "algorithm"
categories:
 - "programming"
---



Two sum is the [first problem on leetcode](https://leetcode.com/problems/two-sum/), it's a simple one. However, by relaxing some restrictions, the problem could become more interesting.  In the post I'll discuss some cases.



### The Simplest Case: return only one solution

This one is just the problem 1 on leetcode, which is described as following:

>   Given an array of integers, return **indices** of the two numbers such that they add up to a specific target.
>
>   You may assume that each input would have **exactly** one solution, and you may not use the *same* element twice.
>
>   **Example:**
>
>
>   Given nums = [2, 7, 11, 15], target = 9,
>
>   Because nums[0] + nums[1] = 2 + 7 = 9,
>   return [0, 1].
>



The typical solution is to use a map to store the elements in the array and find whether `target - n`  is in the map for each `n` in the array.


``` java
public int[] twoSum(int[] nums, int target) {
    // check the input at first
    if(nums == null || nums.length < 2) {
        return null;
    }
  
    Map<Integer, Integer> Map = new HashMap<>();
    for(int i = 0; i < nums.length; ++i) {
        if(map.containsKey(target)) {
            return new int[]{i, map.get(target)};
        }
        map.put(target, i);
    }
  
    return null;
}
```



**Time complexity**: O(n), as every element in the array might be checked.

**Space complexity**: O(n), as every element in the array might be stored in the map.

Though the problem assumes that there's exact one solution, the above code could also deal with the situation that there's no solution by just returning `null`. 



### Sorted Case: the array is sorted

It's the [problem 167](https://leetcode.com/problems/two-sum-ii-input-array-is-sorted/) on leetcode, it's just the same as problem 1, except that the array is is already **sorted in ascending order**.

Two pointers would solve the problem perfectly, we keep update `left` and `right` pointer till `nums[left] + nums[right]` equals `target`.

```java
public int[] twoSum(int[] nums, int target) {
    // check the input at first
    if(nums == null || nums.length < 2) {
        return null;
    }
  
    int left = 0, right = nums.length - 1;
    while(left < right) {
        int sum = nums[left] + nums[right];  
        if(sum == target) {
            return new int[]{left, right};
        }
        else if(sum > target) {
            --right;
        }
        else {
            ++left;
        }
    }
    return null;
}
```

**Time complexity**: O(n), as every element in the array might be checked.

**Space complexity**: O(1), only two extra variables are used.

The code could also deal with the situation that there's no solution.



### A tiny change: return all solutions

The problem is just the same as problem 1, except that there might be zero or more solutions. Since for each solution pair `<i, j>`, `<j, i>` is also a valid solution, let `i < j` to reduce the output size. The problem is described as following:

>    Given an array of integers `nums`, return **all indices** of the two numbers `nums[i]`, `nums[j]`, `i < j` such that `nums[i] + nums[j]`  equals a specific target.
>
>    **Example1:**
>
>    Given nums = [2, 3, 6, 7, 11, 15], target = 9,
>
>    Because nums[0] + nums[3] = 2 + 7 = 9, nums[1] + nums[2] = 3 + 6 = 9
>    return 
>
>    [
>
>    ​    [0, 3],
>
>    ​    [1, 2]
>
>    ]
>
>    **Example2:**
>
>    Given nums = [1, 2], target = 5, return []



For this problem we have to consider another issue: **duplicates**, as duplicate numbers have different indexes and hence generate different solutions.  So we use `Map<Integer, List<Integer>>` to store the number and indexes, and then iterate the map to check for solutions.

``` java
public List<int[]> twoSum(int[] nums, int target) {
    List<int[]> list = new ArrayList<>();
    // check the input at first
    if (nums == null || nums.length < 2) {
        return list;
    }

    Map<Integer, List<Integer>> map = new HashMap<>();
    for (int i = 0; i < nums.length; ++i) {
        List<Integer> l = map.getOrDefault(nums[i], new ArrayList<>());
        l.add(i);
        map.put(nums[i], l);
    }

    Set<Integer> removed = new HashSet<>();
    // use var keyword to shorten the long class name
    for (var entry : map.entrySet()) {
        int key = entry.getKey();
        if (!removed.contains(key) && map.containsKey(target - key)) {
            var l1 = entry.getValue();
            var l2 = map.get(target - key);

            for (int i = 0; i < l1.size(); ++i) {
                for(int j = 0; j < l2.size(); ++j) {
                    add(list, l1.get(i), l2.get(j));
                }
            }
            
            // modifing map during iterating is not allowed, just record the removed keys in another set 
            removed.add(key);
            removed.add(target - key);
         }
    }

    return list;
}

public void add(List<int[]> list, int i, int j) {
    if (i < j) {
        list.add(new int[]{i, j});
    }
}
```

**Time complexity**: worst case O(n<sup>2</sup>), consider the case nums = [1, 1, ..., 1], target = 2, there would be n * (n - 1) / 2 loops.

**Space complexity**: O(n), all elements would be added to map.



### Fewer Limitations: every number could be used multiple times 

I met this problem in an interview and I failed as I treated it as a recursion problem but failed to find the recursion relation. After that I thought it carefully, and it's the initial reason why I wrote this post.

Firstly let's describe the problem:

> Given an array of integers, pick at lease one number from the array such such the sum of all the numbers  equals to a target. Each number could be used **zero or more** times. Return all solutions.
>
> **Example**:
>
> Given nums = [1, 2, 5], target = 6
>
> Returns: 
>
> [
>
> ​    [1, 1, 1, 1, 1, 1],
>
> ​    [1, 1, 1, 1, 2],
>
> ​    [1, 1, 2, 2],
>
> ​    [1, 5],
>
> ​    [2, 2, 2] 
>
> ]



However, there's a **trap** here: if the sum of two numbers equals 0, there would be infinite answers since we can generate a new answer by adding a pair of numbers with sum 0 to the original answer. For example, given the array `[-1, 1, 3]`, target = 3, `[3]`, `[-1, 1, 3]`, `[-1, -1, 1, 1, 3]`... would all be valid answers.

So we need to add some restrictions here, any one of the following three is fine:

* only allow positive integers in the array
* limit the output list size
* limit the number count in the answer

For simplicity, we assume that there're only positive integers.

Another noticeable point is that we output the original integers rather than their indexes as it's easy to check the answer, we should also pay attention to duplicate numbers to avoid duplicate answers. If we use indexes as output, then we don't need to filter the duplicates.

Now let's focus on the solution. It's a **backtracking** problem since we need to search each state after adding an integer to the answer.

```java
public List<List<Integer>> anySum(int[] nums, int target) {
	List<List<Integer>> list = new ArrayList<>();
    // check the input at first
    if (nums == null || nums.length == 0) {
        return list;
    }
    
    // filter duplicates with set
    Set<Integer> set = new HashSet<>();
    for (int i : nums) {
        set.add(i);
    }
    int[] unique = set.stream().mapToInt(i -> i).toArray();

    search(unique, 0,  0, target, list, new ArrayList<>());
    return list;
}

public void search(int[] nums, int sum, int index, int target, 
                   List<List<Integer>> list, List<Integer> subList) {
    if(index >= nums.length || sum > target) {
        return;
    }

    if (sum == target) {
        // add copy of current list to the final answer
        list.add(new ArrayList<>(subList));
        return;
    }

    for (int i = index; i < nums.length; ++i) {
        subList.add(nums[i]);
        // still search from index i as nums[i] could be used multi times
        search(nums, sum + nums[i], i, target, list, subList); 
        // back to last path, the sign of backtracking
        subList.remove(subList.size() - 1);
    }
}
```

**Time complexity**: 

It's a little complicated, the filter process contributes O(n) to the time, then let's observe the search space:

![search space](/img/2019-03/search-space.png)

The first number is the integer to be added to the solution, the second number is the sum of all the selected integers.

It's a N-ary tree with max N levels (if the array contains 1). Now let's do some math works:

> Assuming that there're $$n$$ integers in the filtered array, let $$N_i$$ be the $$i^{th}$$ integer, $$T$$  be the target,
>
> for simplicity, we also assume that the filtered array is sorted in ascending order (easily satisfied by sorting, which takes only $$O(nlogn)$$ time), then the search tree's max depth $$D = \lceil T / N_0\rceil$$
>
> use $$C_i$$ as the node count for level $$i$$, it's obvious that $$C_1 = n$$, 
>
> consider the children count $$C_1j$$ of each node $$j$$ in level 1, we have
>
> $$
> \begin {equation} 
> C_1j = 
> \begin{cases}
> n - j & & {T - N_j >= 0}\\
> 0 & & {T - N_j} < 0
> \end{cases}
> \end{equation}
> $$
>
> then $$ C_2 = \sum_{j=0}^{n-1}C_{1j} = n * (n + 1) / 2 $$ if $$T$$ is large enough. Though some paths have been skipped, it's still $$O(n^2)$$.
>
> For the level $$ l <= \lceil T/N_{n-1} \rceil, C_l = O(n^l) ​$$,  then for level $$ l > \lceil T / N_{n-1} \rceil ​$$ , $$ C_l ​$$ would go on being in $$O(n^l)​$$ and then decrease after $$l​$$ exceeds some threshold $$t​$$. The accurate value of $$t​$$ depends on the distribution of $$N_i​$$, but for average cases we can assume that $$ t = avg(N_i) = \sum_{i=0}^{n-1}N_i / n ​$$ .
>

Since the largest power dominates the time complexity, the time complexity of this algorithm is $$O(n^{avg(N_i)})$$.

**Space complexity:**

$$O(D)$$, where $$D$$ is the max search tree depth.



### Conclusion

To solve an algorithm problem, firstly clarify all the conditions and make no assumptions unless it's been given clearly. Then use proper data structures and strategies to solve the problem. Some factors should be considers as they may have an influence on the correctness and time complexity. For example, **order** and **duplicates** are two important factors for array related problems.

For many cases, use space as a cost to reduce the time complexity is a reasonable choice. If there's a brute force solution with $$O(n^2)$$, recusion, divide and conquer and sorting are typical ways to reduce the time complexity. And if all situations should be checked, backtracking is the first choice.