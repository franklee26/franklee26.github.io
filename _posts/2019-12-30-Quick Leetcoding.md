---
layout: post
title: Spiral Matrix
subtitle: Quick leetcoding problems!
gh-repo: franklee26/QEmbed
gh-badge: [star, fork, follow]
tags: [Leetcode]
---

See [this](https://leetcode.com/problems/spiral-matrix/) medium difficulty problem here. In quick summary, the input is a matrix (list of lists) and the expected output is a list. This list should contain elements from the input matrix in a spiral-traversal order. Take a look at this example from the leetcode site:

![Example](https://i.imgur.com/fQCYDE3.png)

There are many solutions out there, but I haven't found one that resembles my approach (but I'm sure this is not the first). My approach is as follows:

* Copy the first row of `matrix` into the solution array
* Delete the row
* Rotate the matrix counter-clockwise
* Repeat until there are no more rows
  
The code below is my submitted solution (in Python3):

{% highlight python linenos %}
def spiralOrder(self, matrix: List[List[int]]) -> List[int]:
    a = []
    while 1:
        if not matrix:
            break
        # first copy everything in the first row into a
        for e in matrix[0]:
            a.append(e)
        # delete the row
        matrix = matrix[1:]
        # rotate the matrix by first building the nxm array
        if matrix == []:
            # nothing left
            break
        n, m = len(matrix), len(matrix[0])
        new_matrix = [[0 for _ in range(n)] for _ in range(m)]
        for i in range(n):
            for j in range(m):
                new_matrix[m-1-j][i] = matrix[i][j]
        # assign
        matrix = new_matrix
    return a
{% endhighlight %}