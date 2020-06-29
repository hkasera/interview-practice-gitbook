# Two Sum Problem & Variations

This is a very popular interview question. I came across many variations to this problem on LeetCode, GeeksForGeeks and other places, but the core idea is the same in all. For the core concept, itself there are many different approaches. Each approach has a different trade off and knowing them can come in handy to solve the different variations. 

This post is inspired by a [Stanford handout](https://web.stanford.edu/class/cs9/sample_probs/TwoSum.pdf), that I read for this problem. In this post, I will introduce the typical two sum problem, discusses a few different approaches to solve them and then share the different variations of this problem, I came across and the intuition to solve them.

{% hint style="warning" %}
Before, we dive further make sure you are comfortable with basic concepts of sorting, **binary search,** hash set and hash maps.

For binary search specifically, I would recommend practicing a couple of variations [https://leetcode.com/explore/learn/card/binary-search/](https://leetcode.com/explore/learn/card/binary-search/)
{% endhint %}

## **Two Sum Problem**

> You are given n integers and a number k. You need to determine whether there is a pair of elements that sums to exactly k.

| Sample Input | Sample Output | Comments |
| :--- | :--- | :--- |
| Input array \[3, 4, 5\] with k = 8 | True | \(3,5\) sums to exactly 8 |

As usual in interview questions, there are a lot of details left out with the intention that we will ask follow up questions to get more clarity. Here are some typical **follow up** questions which can be asked -

* Can we modify the array? _**Yes, that's fine.**_
* Do we know something about the range of the numbers in the array? _**No, they can be arbitrary integers.**_ 
* Are the array elements necessarily positive? **No, they can be positive, negative, or zero.** 
* Do we know anything about the value of k relative to n or the numbers in the array? **No, it can be arbitrary**
* Can we consider pairs of an element and itself? **No, the pair should consist of two different array elements.**
* Can the array contain duplicates? **Sure, that's a possibility.**
* Is the array necessarily in sorted order?  **No, that's not guaranteed.**

Also, there are a couple of edge cases one might need to watch out for -

* List might just have zero or one element.
* We should handle the following two cases when the target sum can be achieved with adding the same number again, for example 6 is the sum of 3 and 3. Two interesting cases can arrive in this case :
  * If the element appears twice in the array \(for example, \[3, 4, 3\] with k=6 \)
  * If the element appears twice in the array 

     \(for example, \[1, 3\] with k = 6\). In this case we shouldn’t return \[3, 3\] as there is only one 3 in the array.

Here are the some sample test cases covering all these cases :

* Input : \[3, 4, 5, 3\] with k=6, Output : True
* Input : \[3, 4, 5, 7\] with k=6, Output : False
* Input : \[1, 4, 5, 7\] with k=8, Output : True
* Input : \[2, 4, 5, 7\] with k=9, Output : True
* Input : \[-2, 4, 5, 7\] with k=9, Output : True

Now, that we are clear of what the problem is asking and have a mental model of the question, let's see how we can solve this programmatically.

### **Approach 1 : Brute Force**

As in any brute force solution, we explore all possible options and evaluate if they are the right answers, we would do the same here. Basically, we can try all the pairs in the array and see if any of them add up to the number k. 

Let's take an example input array \[7, 4, 5, 3\] with target sum as 9. 

In this approach, we want to explore all the possible pair combinations : \(7,4\), \(7,5\), \(7,3\), \(4,5\), \(4,3\) and \(5,3\) and for each pair check if the sum of the two elements add up to 9. As soon as we find one, we can return true. If at the end, we don't find any then we return false. 

Below code shows the java implementation of this approach, that is provided in the handout.

```java
private boolean sumsToTarget(int[] arr, int k) {
   for (int i = 0; i < arr.length; i++) {
      for (int j = i + 1; j < arr.length; j++) {
          if (arr[i] + arr[j] == k) {
             return true;
          }
      }
   }
   return false;
}
```

#### Time complexity

We run a nested for loop and explore all the n\*n possible pairs, this will takes Θ\(n^2\) time in the worst ­case. Hence the time complexity is Θ\(n^2\).

#### Space complexity

This solution uses only Θ\(1\) space, since we didn't create any additional data structure.

#### Analysis

This approach works correctly, but we always ask to ourselves can we do it better. In this case, yes we can. Let's see in the next approach how we can reduce the time complexity.

### **Approach 2 : Sorting + Binary Search**

In this next approach, we want to reduce the search space. To understand this approach, let's take the same example of input array \[7, 4, 5, 3\] with target sum as 9. 

In the brute force approach we explored all the possible pair combinations : \(7,4\), \(7,5\), \(7,3\), \(4,5\), \(4,3\) and \(5,3\) and eventually found that \(4,5\) satisfies the condition. 

Essentially we are solving something like this -  `An element + Another element = Target Sum`, find if such two elements exist in the array.

If we know the sum and we have one element, the second element of pair is the difference of target sum and the first element we have found. So, in the above example, 4 is one element and sum is 9, so second element is 9 - 4 = 5. We can call 5 as the _**complement**_ to 4.

So, if we pick elements one by one, say we pick 7 first and then search for 9-7 which is 2 in the array. So, we want to do this -

| Action | Complement | Next Action |
| :--- | :--- | :--- |
| Pick 7 | 9-7 = 2 | Search for 2 in the array  |
| Pick 4 | 9-4 = 5 | Search for 5 in the array |
| Pick 5 | 9-5 = 4 | Search for 4 in the array |
| Pick 3 | 9-3 = 6 | Search for 6 in the array |

Now, finding an element in array will take Θ\(n\) time. We will do this n times so, time complexity is again Θ\(n^2\). This is not better than Approach 1. 

How can we reduce the search time time in the array? **Think 🤔**

{% hint style="info" %}
Searching in sorted array  =&gt;  Binary search 
{% endhint %}

If the array is sorted and we want to search in a sorted array, we could use _**binary search,**_ which takes Θ\(logn\) time. So, we could sort the array in the beginning and search then! As we do this n times so, we will essentially reduce the time complexity to Θ\(n\*logn\).

{% hint style="warning" %}
We need to be careful about the edge case here where the target sum is the sum of same element added twice. 
{% endhint %}

Let's be clear about this edge case by taking an example. The input array is \[3, 7, 5, 4, 2\] with target sum as 6. Now, let's repeat the steps.

We sort the array =&gt; \[2, 3, 4, 5, 7\]

| Action | Complement | Next Action |
| :--- | :--- | :--- |
| Pick 2 | 6-2 = 4 | Search for 4 in the array =&gt; not found |
| Pick 3 | 6-3 = 3 | **Search for 3 in the array =&gt; found** |

We will end up picking the same 3 again!! This is incorrect as there exists only one occurrence of 3 in the array.

We don't want to pick the same element as the second element in the pair unless they are in the array twice. We can do an index check when we find the element for this case, i.e. elements in the pair should have different indices in the array.

So, the basic steps of the algorithm are :

* Sort the array
* For every element in the array, find the complement using binary search.
  * If a complement is found and it is not the same element, then return true.
* If we never find the complement, return false.

Below code shows the java implementation of this approach, that is provided in the handout.

```java
private boolean sumsToTarget(int[] arr, int k) {
    Arrays.sort(arr);
    for (int i = 0; i < arr.length; i++) {
        int siblingIndex = Arrays.binarySearch(arr, k– A[i]);
        if (siblingIndex >= 0) { // Found it!
            /* If this points at us, then the pair exists only if
             * there is another copy of the element. Look ahead of
             * us and behind us.
             */
            if (siblingIndex != i ||
                (i > 0 && arr[i - 1] == arr[i]) ||
                (i < arr.length– 1 && arr[i + 1] == arr[i])) {
                return true;
            }
        }
    }
    return false;
}
```

#### Time complexity

We sort the array. In-place sorting methods like quick sort takes Θ\(n\*logn\) time. Then for each element, we use binary search to find the complement element in the pair. This will also take Θ\(n\*logn\) time.  Hence the time complexity is Θ\(n\*logn\).

#### Space complexity

This solution uses only Θ\(1\) space, since we didn't create any additional data structure. For sorting also, we used in-place sorting method.

#### Analysis

This approach is better than brute force as we reduced the time complexity and if the array provided us is sorted then this approach can come in handy! Again, we want to do better. There is another variant of this approach where we use sorted array but instead of binary search we do something else. Let's see the next approach.

### **Approach 3 : Sorting + Two Pointers**

This is a slight variation from previous approach. We will still sort the array but instead of doing binary search for every n element, we will explore a different route. 

* We walk two pointers inward from the ends of the array, at each point looking at their sum.
* If sum is exactly k, then we're done. 
* If sum exceeds k, then any sum using the larger element is too large \(_**why is this the case, think**_ **🤔** _****_\), so we walk that pointer inwards. 
* If it's less than  k, then any sum using the lower element is too small, so we walk that pointer inwards. 
* We stop when the two pointers meet.

Let's take an example \[3 , 4, 5, 7\] and target sum is 9. Now, we start with two pointers :

| Left Pointer | Right Pointer | Sum | Note |
| :--- | :--- | :--- | :--- |
| 3 | 7 | 10 | 10 &gt; 9, so we should pick an element less than 7 |
| 3 | 5 | 8 | 8 &lt; 9, so we should pick an element greater than 3 |
| 4 | 5 | 9 | 9 = 9, we found it!!! |

By eliminating candidates in each step, in a simple iteration of array, we can get the two elements in the pair that add up to sum, using the fact that the array is **sorted**!! 

_Also, this approach is more cleaner to handle the case when the target sum is the sum of two same elements added. Since, we stop when two pointers meet, we will never grab the same element as the other element in pair so we don't need to do extra checks for handling that._

Below code shows the java implementation of this approach, that is provided in the handout.

```java
private boolean sumsToTarget(int[] arr, int k) {
    Arrays.sort(arr);
    int lhs = 0, rhs = arr.length– 1;
    while (lhs < rhs) {
        int sum = arr[lhs] + arr[rhs];
        if (sum == k) {
            return true;
        } else if (sum < k) {
            lhs++;
        } else {
            rhs--;
        }
    }
    return false;
}
```

#### Time complexity

The search process will take Θ\(n\) time, much better than Θ\(n\*logn\) time in Approach 2. However, we didn't do any better in overall time complexity. **Think why?** _****_**🤔**

{% hint style="warning" %}
Remember we still need to sort the array!
{% endhint %}

The bottle neck in the overall process is **sorting** which will take Θ\(n\*logn\) time. So overall, the time complexity will still be Θ\(n\*logn\)

#### Space complexity

This solution uses only Θ\(1\) space, since we didn't create any additional data structure. For sorting also, we used in-place sorting method.

#### Analysis

Compared to Approach \#1, we reduced the time complexity so we did good. Although overall time complexity wise, we didn't do any better in this approach, but if the array provided us is sorted then this approach is better than approach \#2! Since, in case of sorted arrays, we will only take Θ\(n\) time.

In both approaches \#2 and \#3, we modified the array! But, what if that is not allowed? Also, what if we are okay to use some space to improve our algorithm. Can we reduce it further to Θ\(n\). Let's see how we can achieve even a better time complexity by trading off with space complexity.

### **Approach 4 : Hash Set**

If we want to search something in an array, it takes Θ\(n\) time. In approach \#2 and approach \#3, we were sorting the array, to reduce our search space. However, let's see if by using an additional data structure, can we solve this search problem differently!!

{% hint style="info" %}
There is a data structure that is efficient for searching / look-ups 
{% endhint %}

**Hash Sets are really efficient for constant time look-ups.**

Let's think if we create a hash set of all the elements in the array. Then scan over the array and check, for each element , whether the complement \(target sum - current element\) exists in the set. Would this work? **Think 🤔**

{% hint style="warning" %}
Make sure their solution correctly handles the case where there are duplicated elements in the array and doesn't accidentally let you pair an element with itself.
{% endhint %}

Hash Sets cannot store duplicate values. So, if at the beginning we store all elements in the array, we will not handle the edge case of same number adding up to create target sum. 

In order to handle this, we could first check if a complement exists in the set and if not, we add that element in set. By doing a check before adding the element to set, we can make sure, we never count the same element as the complement.  Let's see how it will work with an example - The input array is \[3, 9, -1, 3 \] with target sum as 6. Now, let's repeat the steps.

| Element | Complement | Set | Note |
| :--- | :--- | :--- | :--- |
| 3 | 6-3  = 3 | {} | Nothing in set yet. Add 3 to set |
| 9 | 6-9 = -3 | {3} | Not in set, Add 9 to set |
| -1 | 6-\(-1\) = 7 | {3,9} | Not in set, Add 7 to set |
| 3 | 6-3=3 | {3,9,-1} | Exists in set. Return true |

Below code shows the java implementation of this approach, that is provided in the handout.

```java
private boolean sumsToTarget(int[] arr, int k) { 
   HashSet values = new HashSet(); 
   for (int i = 0; i < arr.length; i++) { 
       if (values.contains(k - A[i])) return true; 
       values.add(A[i]); 
   } 
   return false; 
}
```

#### Time complexity

Insertion and lookup of one element takes Θ\(1\) time, but we do this n times. Hence, time complexity is Θ\(n\) because n insertions and n lookups in a hash set takes expected time Θ\(n\).

#### Space complexity

There can be n entries in the hash set, hence space complexity is Θ\(n\)

#### Analysis

We improved the time complexity to Θ\(n\) which is amazing but we also introduced the space complexity. In a trade off between time and space, in general we prioritize time over space, this is considered a very efficient solution as there is no clear way to improve on both the time complexity or the space complexity simultaneously.

| Approach | Time Complexity | Space Complexity |
| :--- | :--- | :--- |
| Approach 1 : Brute Force | Θ\(n^2\) | Θ\(1\) |
| Approach 2 : Sorting + Binary Search | Θ\(n\*logn\) | Θ\(1\) |
| Approach 3 : Sorting + Two Pointers | Θ\(n\*logn\) | Θ\(1\) |
| Approach 4 : Hash Set | Θ\(n\) | Θ\(n\) |

To brief, on what we saw so far, we saw there are 4 approaches to solve the Two Sum Problem. Typically Approach \#4, where we use hash sets is considered the most efficient as we reduce the time complexity to Θ\(n\) here but we do a trade off with space.

## **\#1 : Two Sum Return Indexes**

> Given an array of integers, return indices of the two numbers such that they add up to a specific target. You may assume that each input would have exactly one solution, and you may not use the same element twice.

| Sample Input | Sample Output | Comments |
| :--- | :--- | :--- |
| Input array \[3, 4, 5\] with k = 8 | \[0,2\] | \(3,5\) sums to exactly 8, so we return their indices |

This question ****is available on [LeetCode](https://leetcode.com/problems/two-sum/)

This is same as the typical two sum problem, however instead of returning true/false, we can return the indexes. This approach will work for all cases except for Approach 4 where we used the hash set. **Think why?** _****_**🤔**

{% hint style="warning" %}
Hash Set doesn't maintain order so we can only store the elements and we will loose its index value.
{% endhint %}

However, in order to make it work, we can use HashMap instead of HashSet and store index of the element as value. The time and space complexities remain the same.

Below code shows the java implementation of this approach, that is provided in LeetCode -

```java
private boolean sumsToTarget(int[] arr, int k) { 
   Map<Integer, Integer> map = new HashMap<>();
    for (int i = 0; i < nums.length; i++) {
        int complement = target - nums[i];
        if (map.containsKey(complement)) {
            return new int[] { map.get(complement), i };
        }
        map.put(nums[i], i);
    }
    throw new IllegalArgumentException("No two sum solution");
}
```

#### Time complexity

Insertion and lookup of one element takes Θ\(1\) time, but we do this n times. Hence, time complexity is Θ\(n\) because n insertions and n lookups in a hash map takes expected time Θ\(n\).

#### Space complexity

There can be n entries in the hash map, hence space complexity is Θ\(n\)

## **\#2 : Two Sum Return Any Pair**

> Write a program that, given an array A\[\] of n numbers and another number x, determines whether or not there exist two elements in S whose sum is exactly x. If it exists, return those two elements

Instead of returning indexes as we did in the Variation \#1 - Two Sum Return Indexes, we can return the elements itself. Note we can already return as soon as we find one pair, as we just need to find one pair.

This question ****is available on [Techie Delight](https://www.techiedelight.com/find-pair-with-given-sum-array/)

```java
private boolean sumsToTarget(int[] arr, int k) { 
   Map<Integer, Integer> map = new HashMap<>();
    for (int i = 0; i < nums.length; i++) {
        int complement = target - nums[i];
        if (map.containsKey(complement)) {
            return new int[] { complement, arr[i] };
        }
        map.put(nums[i], i);
    }
    throw new IllegalArgumentException("No two sum solution");
}
```

## **\#3 : Two Sum Return All Pairs**

> Given an array of integers, and a number ‘sum’, print all pairs in the array whose sum is equal to ‘sum’.

| Sample Input | Sample Output | Comments |
| :--- | :--- | :--- |
| \[2, 4, 5, 7\] with k = 9 | \(2,7\)  \(4,5\) | 2+7 and 4+5 both sum up to 9 |
| \[1, 5, 7, -1, 5\] with k = 6 | \(1, 5\) \(7, -1\) \(1, 5\) | Two 5’s which create two pairs with the element 1 |

Instead of returning as soon as we find one pair in Variation \#2 - Two Sum Return Any Pair, we need to keep checking for every element.

This question is available on [GeeksforGeeks](https://www.geeksforgeeks.org/print-all-pairs-with-given-sum/)

## **\#4 : Two Sum Return Count of Pairs**

> Given an array of integers, and a number ‘sum’, find the number of pairs of integers in the array whose sum is equal to ‘sum’.

| Sample Input | Sample Output | Comments |
| :--- | :--- | :--- |
| \[2, 4, 5, 7\] with k = 9 | 2 | 2+7 and 4+5 both sum up to 9 |
| \[1, 5, 7, -1, 5\] with k = 6 | 3 | Two 5’s which create two pairs with the element 1 |

Instead of storing and returning the pairs as we do in Variation \#3 - Two Sum Return All Pairs, we can increment the count every-time we find a pair and return this value.

This question is available on [GeeksforGeeks](https://www.geeksforgeeks.org/count-pairs-with-given-sum/)

## **\#5 : Two Sum Return Count of Distinct Pairs**

> Count the number of distinct pairs whose sum exists in the given array.

| Sample Input | Sample Output | Comments |
| :--- | :--- | :--- |
| \[2, 4, 5, 7\] with k = 9 | 2 | 2+7 and 4+5 both sum up to 9 |
| \[1, 5, 7, -1, 5\] with k = 6 | 2 | Two 5’s will create only one pair with the element 1 |

As opposed to storing all pairs in Variation \#3 - Two Sum Return All Pairs, we need to store only distinct pairs. We can create a hash set of pairs that add up to sum and return the size of this set. Storing in Set can guarantee the **distinct** pairs.

This question is available on [GeeksforGeeks](https://www.geeksforgeeks.org/count-number-distinct-pairs-whose-sum-exists-given-array/)

## **\#6 : Two Sum Sorted Array**

> Given an array of integers that is already sorted in ascending order, find two numbers such that they add up to a specific target number.

It is exactly same as typical two sum problem, with an additional advantage that the array is sorted. Since, the array is already sorted, we can use either the Approach 2 Θ\(n\*logn\) or Approach 3 Θ\(n\) directly without worrying about sorting. **Approach 3 is a good solution here given the time complexity is linear.**

This question is available on [LeetCode](https://leetcode.com/problems/two-sum-ii-input-array-is-sorted/)

## **\#7 : Two Sum Data Structure**

> Design and implement a TwoSum class. It should support the following operations: add and find.
>
> add - Add the number to an internal data structure.
>
> find - Find if there exists any pair of numbers which sum is equal to the value.

This is the same as the typical two sum problem, the only difference being here we also create the array.

The add function is simply creating an array so the Time Complexity for add will be always Θ\(1\) for all the approaches.

The find function is where the actual find logic is happening, which based on the different approaches might have different time complexities.

This question is available on [LeetCode](https://leetcode.com/problems/two-sum-iii-data-structure-design/)

## **\#8 : Three Sum**

> Given an array of n integers, are there three elements which add up to k?

| Sample Input | Sample Output | Comments |
| :--- | :--- | :--- |
| \[2, 4, 5, 7\] with k = 11 | True | 2+4+5 sum up to 11 |
| \[2, 4, 5, 7\] with k = 12 | False | No three elements sum up to 12 |

This problem is a follow-up of typical Two Sum problem in interviews. 

Since instead of two elements, here we are dealing with three elements, basically the idea is for each element in the array, find two sum. 

If our input array is \[3, 4, 5, 6\] and target sum is 12, then for each element 3, 4, 5, 6 we will find two sum by subtracting the element by 12 and using that as target sum. 

| Element | Complement | Reduced Problem Statement |
| :--- | :--- | :--- |
| 3 | 12-3  = 9 | Find two elements in array that add up to 9 |
| 4 | 12-4 = 8 | Find two elements in array that add up to 8 |
| 5 | 12-5 = 7 | Find two elements in array that add up to 7 |
| 6 | 12-6=6 | Find two elements in array that add up to 6 |

So, for every element, we are repeating the two sum problem.

We can use all the four basic approaches of the two sum problem and the complexity will just increase by a factor of n.

| Approach | Time Complexity | Space Complexity |
| :--- | :--- | :--- |
| \#1 : Brute Force | Θ\(n\*n\*n\) | Θ\(1\) |
| \#2 : Sorting + Binary Search | Θ\(n\*logn\) + Θ\(n\*n\*logn\) | Θ\(1\) |
| \#3 : Sorting + Two Pointer | Θ\(n\*logn\) + Θ\(n\*n\) | Θ\(1\) |
| \#3 : Hash Set | Θ\(n\*n\) | Θ\(n\) |

Overall, the "Sorting + Two Pointer" approach handles both time and space complexity efficiently. Again in three sum there can be many variations but, I am sure we get an idea on how to tackle them now.

## Conclusion

So, as we saw a simple problem can be asked in various different ways by slight modification. If we know the core approach we can solve all of these very easily. 

Below is the list of the questions, we just understood.

* [Two Sum](https://leetcode.com/problems/two-sum/)
* [Two Sum II - Input array is sorted](https://leetcode.com/problems/two-sum-ii-input-array-is-sorted/)
* [Two Sum III - Data structure design](https://leetcode.com/problems/two-sum-iii-data-structure-design/)
* [Two Sum IV - Input is a BST](https://leetcode.com/problems/two-sum-iv-input-is-a-bst/)
* [Print all pairs with given sum](https://www.geeksforgeeks.org/print-all-pairs-with-given-sum/)
* [Find pair with given sum array](https://www.techiedelight.com/find-pair-with-given-sum-array/)

👏 👏 Congratulations for making this far!! 👏👏

## Practice Further

Now, that we know the core techniques, a couple of more variations of this question is possible, go ahead and try them out :

* [Given two unsorted arrays, find all pairs whose sum is x](https://www.geeksforgeeks.org/given-two-unsorted-arrays-find-pairs-whose-sum-x/)
* [2Sum Smaller](https://www.geeksforgeeks.org/count-pairs-array-whose-sum-less-x/)
* [3sum](https://leetcode.com/problems/3sum/)
* [4sum](https://leetcode.com/problems/4sum/)
* [3Sum Closest](https://leetcode.com/problems/3sum-closest/)
* [3Sum Smaller](https://leetcode.com/problems/3sum-smaller/)

Happy coding!!



\*\*\*\*

\*\*\*\*







