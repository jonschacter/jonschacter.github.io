---
layout: post
title:      "Cracking the Coding Interview with Ruby Pt.1"
date:       2020-10-02 22:01:52 +0000
permalink:  cracking_the_coding_interview_with_ruby_pt_1
---


I recently picked up [Cracking the Coding Interview 6thEd.](https://www.amazon.com/Cracking-Coding-Interview-Programming-Questions/dp/0984782850/) to help me prepare for technical interviews. Although the book is based in Java, I will be working my way through the problems using Ruby and blogging some of my solutions along the way. 

My main focus is to write clean, readable code with a focus on efficiency and scalability. 

## Problem 1.1 - Is Unique

Implement an algorithm to determine if a string has all unique characters.

### Initial Thoughts

I am immediately thinking two things with regards to efficiency.

1.) I want to avoid iterating through the strings characters more than once. I suspect I will be converting the characters into a hash so that I can take advantage of key lookup. 
2.) I want to be sure that my algorithm will stop running as soon as it finds the first duplicate. 

### Solution

```
def unique?(str)
        char_hash = {}
        
        str.chars.each do |char|
            if char_hash[char]
                return false
            else
                char_hash[char] = true
            end
        end

        return true
    end
```

I iterate through each character in the string and use hash key lookup to see if they character has already been added as a hash key. If it has then I can immediately return false, otherwise I add it to the hash. If the algorithm completes the iteration without any duplicates, the function will return true.

Let's try to apply this to a more complicated problem

## Problem 1.4 - Palindrome Permutation

Given a string, write a function to check if it is a permutation of a palindrome. You can ignore casing and non-letter characters.

### Initial Thoughts

This requires some more complex logic. I need to check if every letter has a duplicate, but I am allowed one single exception of an odd letter count. (i.e. "racecar" is a palindrome with only one 'e' or "aabbbcc")

### Solution

My first step is to iterate through the string to create a hash with {letter => count} key/value pair.

```
def palindromable?(str)
        letter_count_hash = Hash.new(0)
        
        str.chars.each do |char|
            letter_count_hash[char] += 1
        end
end
```

Now that I have my letter_count_hash I need to check for odd values. 
```
letter_count_hash.map{ |k, v| v }.select{ |i| i.odd? }
```
The above snippet will create an array with all my odd values. I can check the length of this array to determine my results.


```
def palindromable?(str)
        letter_count_hash = Hash.new(0)
        
        str.chars.each do |char|
            letter_count_hash[char] += 1
        end

        odd_letter_count = letter_count_hash.map{ |k, v| v }.select{ |i| i.odd? }.length

        odd_letter_count > 1 ? false : true
end
```

I could add more logic here when iterating through the letter count hash that would allow me to return false as soon as a second odd value comes up. However, I know the hash will be limited by alphabet characters and won't reach extreme sizes so I have opted for code clarity here.

I am excited to keep working through these challenges and hope you can find some enjoyment and value following along. Let me know if you've found better solutions and optimizations!
