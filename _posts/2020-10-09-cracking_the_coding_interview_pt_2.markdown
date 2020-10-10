---
layout: post
title:      "Cracking the Coding Interview Pt.2"
date:       2020-10-10 02:24:18 +0000
permalink:  cracking_the_coding_interview_pt_2
---


## One Away

There are three types of edits that can be performed on strings: insert a character, remove a character, or replace a character. Given two strings, write a function to check if they are one edit (or zero edits) away.

### Initial Thoughts

This is a tricky one. I think it would be fairly simple to do each check individually but I want to immediately start thinking of how to do them efficiently. 

```
def one_away?(str1, str2)
        if str1 == str2
            return true
        elsif (str1.length - str2.length).abs > 1
            return false
        end
end
```

So I started with the cases where I can skip iteration entirely. If the strings are identical I will return true, and if they are more than 1 character different in length I will return false.

```
def one_away?(str1, str2)
        if str1 == str2
            return true
        elsif (str1.length - str2.length).abs > 1
            return false
        end

        long_str = str1.length >= str2.length ? str1 : str2
        short_str = str1.length < str2.length ? str1 : str2

        i = 0
        j = 0
        diff = 0


end
```

Next I added some basic logic to assign my strings to variable based on their length. If I know which variable is longer it can save me some logic down the road. I also assigned variables to track the index of each string as I iterate through them, as well as count the differences between them

```
while (i < long_str.length)
            # some code here

            if diff > 1
                return false
            end
end

return true
```

Now for the meat of my logic. I know I will be iterating through each character until I reach the end of long_str (I don't need to worry about short_str because it is guaranteed to be shorter or equal to long_str). If along the way I reach a diff > 1 I will return false, but if I make it through the while loop I can return true.

## How should I increment?

I will have 4 situations to account for:
1. Characters are equal, string lengths are equal
2. Characters are equal, string lengths are different
3. Characters are different, string lengths are equal
4. Characters are different, string lengths are different.

For situations 1 and 2, because the characters are equal I can increment both indexes and NOT diff.
For situation 3, I can increment both index AND diff.
For situation 4, I need to increment diff and the LONGER word index only.

Let's apply this.

```
def one_away?(str1, str2)
        if str1 == str2
            return true
        elsif (str1.length - str2.length).abs > 1
            return false
        end

        long_str = str1.length >= str2.length ? str1 : str2
        short_str = str1.length < str2.length ? str1 : str2

        i = 0
        j = 0
        diff = 0

        while (i < long_str.length)
            if long_str[i] == short_str[j]
                i += 1
                j += 1
            elsif long_str.length == short_str.length
                diff += 1
                i += 1
                j += 1
            else
                diff += 1
                i += 1
            end

            if diff > 1
                return false
            end
        end

        return true
end
```

I liked this problem. I think it wouldn't have been very complex to tackle as three seperate checks, but trying to efficiently manage each within one iterate posed some extra difficulties. 
