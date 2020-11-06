---
layout: post
title:      "Quick Sorting in Ruby"
date:       2020-11-06 23:54:25 +0000
permalink:  quick_sorting_in_ruby
---


Today I will continue my journey through sorting algorithms with quick sort. Let's take a look at  the process.

> Quicksort is a divide-and-conquer algorithm. It works by selecting a 'pivot' element from the array and partitioning the other elements into two sub-arrays, according to whether they are less than or greater than the pivot. The sub-arrays are then sorted recursively. This can be done in-place, requiring small additional amounts of memory to perform the sorting.
> [Wikipedia on Quicksort](https://en.wikipedia.org/wiki/Quicksort)
> 

```
def quick_sort(array, low_index = 0, high_index = array.length - 1)
    # code here
		
		return array
end
```

I started with arguments. I know my quick_sort method will receive an array and optionally a low and high index. If there is no low or high index I want to default to the first and last index of the array. I can also fill in that I want to return the sorted array at the end.


```
def quick_sort(array, low_index = 0, high_index = array.length - 1)
    if low_index < high_index
        pivot_index = partition(array, low_index, high_index)
        quick_sort(array, low_index, pivot_index - 1)
        quick_sort(array, pivot_index + 1, high_index)
    end
    
    return array
end
```

Here I am assuming a partition method (which I will create next) which takes an unsorted array with low and high indices, and moves items around the array so that the partition is in the correct spot, any elements to its left are lower or equal in value, and any elements to the right are higher. Then I can run the quick_sort method recursively on both halves of the array to continue sorting until finished. Now let's create this partition logic.

```
def partition(array, lo, hi)
    pivot = array[hi]
    current_index = lo
    
    i = lo
    while i < hi
        if array[i] <= pivot
            temp = array[i]
            array[i] = array[current_index]
            array[current_index] = temp
            current_index += 1
        end
        i += 1
    end

    array[hi] = array[current_index]
    array[current_index] = pivot
    
    return current_index
end
```

First I declared the last value in the provided range to be my pivot. Then, using i, we can iterate through the provided range of the array. At each index we check whether the value at that position is <= the pivot, and if it is, we swap it to the end of the "low section", tracked by my variable current_index. After that process is complete I move the pivot into its proper position in the array, and return that index.

Done! With those two pieces together we have built a Quicksort method using Ruby.
