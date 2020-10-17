---
layout: post
title:      "Cracking the Coding Interview Pt. 4"
date:       2020-10-17 00:05:53 +0000
permalink:  cracking_the_coding_interview_pt_4
---


## Rotate Matrix
Given an image represented by an N x N matrix, where each pixel in the image is repesented by an integer, write a method to rotate the image by 90 degrees. Can you do this in place?

### My Solution

While writing out a couple quick tests for this problem I stumbled upon the key logic to solve it. I can take each column of the grid, reverse it, and make those the rows. For example given the following matrix:

[
[1, 2, 3],
[4, 5, 6],
[7, 8, 9]
]

My desired outcome is:

[
[7, 4, 1],
[8, 5, 2],
[9, 6, 3]
]

The first column ([1, 4, 7]) can be reversed to become the first row of the solution ([7, 4, 1]). Let's code it!

```
def rotate_matrix(matrix)
        rotated_matrix = []
        matrix.transpose.each do |row|
            rotated_matrix << row.reverse
        end
        rotated_matrix
end
```

The only thing I wanted to add to this is some protection against improper data by adding the following conditional:

```
if matrix.length == 0 || matrix.length != matrix[0].length
            return false
end
```

### Second Thoughts 

I think this was a fairly clean solution, but I noticed the second part of the question asking if this can be done in place. By creating a second matrix with the new values I am using extra memory to store this matrix. I need an entirely different approach to reassign the values of the matrix in place. 

For this I came up with a layer based approach. For each layer I can swap index by index to rotate top => left => bottom => right. My first question is how to recognize how many layers there are to loop through. n (matrix.length) of 2 or 3 requires one layer of rotation (because the center value will remain in place), n of 4 or 5 requires two layers. So I can use counter < n/2 as a stopping point for my loop

```
def self.rotate_matrix(matrix)
        if matrix.length == 0 || matrix.length != matrix[0].length
            return false
        end

        n = matrix.length
        i = 0

        while i < n/2 do
            # ...code here
            
            i += 1
        end

        return matrix
end
```

Now the general pattern I want to follow is

```
for i=0 .. n
   temp_storage = top[i]
	 top[i] = left[i]
	 left[i] = bottom[i]
	 bottom[i] = right[i]
	 right[i] = temp_storage
```

Let's try to apply this logic to code.

```
def rotate_matrix(matrix)
        if matrix.length == 0 || matrix.length != matrix[0].length
            return false
        end

        n = matrix.length
        # layer counter
        layer = 0

        while layer < n/2 do
            start_index = layer
            finish_index = n - 1 - layer
            i = start_index

            while i < finish_index do 
                offset = i - start_index
								
                # temporarily store top
                top = matrix[start_index][i]
								
                # left => top
                matrix[start_index][i] = matrix[finish_index - offset][start_index]
								
                # bottom => left
                matrix[finish_index - offset][start_index] = matrix[finish_index][finish_index - offset]
								
                # right => bottom
                matrix[finish_index][finish_index - offset] = matrix[i][finish_index]
								
                # top => right
                matrix[i][finish_index] = top
								
                # increment counter
                i += 1
            end
						
            # increment layer
            layer += 1
        end

        matrix
end
```

### Final Thoughts

I'm pleased with this solution as well. Depending on the needs of the project I think both solutions could be viable. The latter uses less memory, whereas the former is cleaner code and uses less iterations.
