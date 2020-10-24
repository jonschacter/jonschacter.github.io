---
layout: post
title:      "Cracking the Coding Interview - String Rotation"
date:       2020-10-24 16:02:20 -0400
permalink:  cracking_the_coding_interview_-_string_rotation
---


Assume you have a method isSubstring which checks if one word is a substring of another. Given two strings, s1 and s2, write code to check if s2 is a rotation of s1 using only one call to isSubstring (e.g. "waterbottle" is a rotation of "erbottlewat").

## Initial Thoughts

My first instinct is to iterate through s1 to find if any point of rotation would create a string equal to s2. I know this is not efficient enough and the question must offer up the isSubstring method for a reason. I have to admit I was a little stumped on how to approach this one and checked the hints where I was given this clue:

> We are essentially asking if there's a way of splitting the first string into two parts, x and y, such that the first string is xy and the second string is yx. For example, x = wat and y = erbottle. The first string is xy = waterbottle. The second string is yx = erbottlewat
> 

Looking at this with variables, x and y, helped me think of a possible new solution. Given s1 = xy and s2 = yx, how can I use the isSubstring method to my advantage? If I combine s1 onto itself I get xyxy, which will include yx as a substring. Let's see if we can apply this.

## Solution

```
def string_rotation?(s1, s2)
        s1s1 = s1 + s1
        return isSubstring(s1s1, s2)
end
```

This is a pretty clear and simple solution to the problem. I don't think I can make this code any more efficient or elegant, but I also must make sure this method has some data-proofing.

```
def self.string_rotation?(s1, s2)
        if s1.length == s2.length && s1.length > 0
            s1s1 = s1 + s1
            return isSubstring(s1s1, s2)
        end
        
        return false
end
```

I added a conditional statement that will return false if the strings are empty or not equal in length. Final answer!
