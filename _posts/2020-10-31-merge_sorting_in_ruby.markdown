---
layout: post
title:      "Merge Sorting in Ruby"
date:       2020-11-01 02:25:06 +0000
permalink:  merge_sorting_in_ruby
---


In my quest to become a better developer I have embarked on a journey to learn about sorting algorithms. I have decided to start with a Merge Sort and will be creating Ruby code to do it!

## Approach
Merge Sorting can be divided into two steps:

1. Divide an array into halves until only one value remains
2. Recombine these values while sorting

## Code
Let's actually start with step 2, the merge method.

```
def merge(l, r)
    sorted_array = []
    
    until l.empty? || r.empty?
        if l.first <= r.first
            sorted_array << l.shift
        else
            sorted_array << r.shift
        end
    end

    sorted_array + left + right
end
```

I create a new empty array and then compare the first value of each given half, adding the lesser until one is empty. Then return the new sorted array with the remaining value added on. Now let's work on step 1.

```
def merge_sort(array)
    return array if array.length <= 1

    mid = array.length / 2
    left = merge_sort(array[0..mid])
    right = merge_sort(array[mid..array.length])

    merge(left, right)
end
```

I first set up my base case. If the array length is already 1 it doesn't require further division. Then I find the midpoint and separate the array into two halves and rerun the function on those halves. Finally using our merge method I recombine them! I'm excited to start applying this pattern in my code and learning more sorting algorithms soon! Stay tuned.
