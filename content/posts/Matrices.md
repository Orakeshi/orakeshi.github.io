---
title: "Research - Matrices"
date: "20 Jan 2022"
showDate: true
comments: true
showSocial: true
showMeta: true
categories:
  - blog
tags:
  - algorithms
  - maths 
  - personal
autoThumbnailImage: false 
thumbnailImagePosition: left
thumbnailImage: https://i.imgur.com/psQany9.png
metaAlignment: center
---
  
On my journey to learn and become a better developer, I find myself researching matrices. Having never been exposed to matrices I will be approaching this learning as a complete beginner on the topic.

I will hopefully by the end of this blog have a better understanding about: What they are?, How are they used?, Basic operations and more!

{{< image classes="fancybox center clear" src="https://i.imgur.com/psQany9.png" thumbnail="https://i.imgur.com/psQany9.png" group="group:personal-matrices-research" thumbnail-width="70%" thumbnail-height="70%" title="Image of matrix transformations on a 3d axis" >}}

<!--more-->
{{< toc >}}

# Reason For Researching:
My reasons for researching matrices is due to several points. Other than the obvious points of "Wanting to be a better developer" or "Expand my knowledge"; My real answer is due to their large role being played on my daily life whilst I am blind to what they are.

What I mean by this is that I mainly spend my working days developing using [Unity3D](https://unity.com/). Within Unity, it is common place for myself to affect an object in some way. Whether I am changing an objects transform via rotation/position I am using Vectors or Quaternions as part of the transform class.

However, several functions within unity use Matrices (Matrix4X4) such as Transform, Camera, Material and more. With this I want to develop m understanding of them

{{< image classes="fancybox center clear" src="https://i.imgur.com/EiQOtzW.png" thumbnail="https://i.imgur.com/EiQOtzW.png" group="group:personal-matrices-research" thumbnail-width="70%" thumbnail-height="70%" title="Unity matrix example" >}}

# Research, Findings & Development:
## What Are Matrices:
Put simply: A Matrix is an array of numbers. They are widely used throughout various sectors & environments. In geometry, matrices are used for representing geometric transformations such as rotations, positions etc.

Matrices are a convenient and compact way to represent large sets of numbers.

{{< image classes="fancybox center clear" src="https://i.imgur.com/qTP6KfB.png" thumbnail="https://i.imgur.com/qTP6KfB.png" group="group:personal-matrices-research" title="Image showing 2x4 matrix" >}}        

## Matrix Uses:
Matrices, as mentioned above, are widely used. One such example is in 3D computer graphics. 4x4 transformation matrices are common place. These n+1 dimensional transformation matrices can be called affine transformation matrices, projective transformation matrices or non-linear transformation matrices (dependent on application)

The size of a matrix is defined by the number of rows and columns it contains. There is no limit to the numbers of rows and columns a matrix (in the usual sense) can have.

## Matrix Operations:
### Matrix Breakdown:
Before I dive into calculating certain operations of a given matrix, first I must break down and understand a matrix. By nature, they are very simple to understand. 

{{< image classes="fancybox center clear" src="https://i.imgur.com/ThGlnJU.png" thumbnail="https://i.imgur.com/ThGlnJU.png" group="group:personal-matrices-research" thumbnail-width="30%" thumbnail-height="30%" title="Image showing breakdown of a 2x3 matrix" >}}

As seen from the matrix above, they are very simple to understand. Matrices are written with row first, column second. This means the dimensions of the matrix above are 2x3

The bottom line of `A23` means in matrix of A find the entry at 23. After going two rows along and 3 columns down the answer is 5.

Below are some more examples of matrix breakdowns. I have written the dimensions for all the following matrices:
{{< image classes="fancybox center clear" src="https://i.imgur.com/By28m6x.png" thumbnail="https://i.imgur.com/By28m6x.png" group="group:personal-matrices-research" thumbnail-width="30%" thumbnail-height="30%" title="Image showing breakdown of a multitude of matrices" >}}

### Addition:
Now I understand the basic structure of a matrix I can move onto its operations. With addition, It's vital to point out that the TWO MATRICES MUST be the same size. The rows and columns must match in size else they cannot be added together.

{{< image classes="fancybox center clear" src="https://i.imgur.com/F56lXd2.png" thumbnail="https://i.imgur.com/F56lXd2.png" group="group:personal-matrices-research" thumbnail-width="40%" thumbnail-height="40%" title="Image showing addition of two matrices" >}}

To start adding you must first add the numbers at there matching position. Following the example above you would do 3+6 & 7+2 etc. After adding each value, the output will be the sum of both matrices.

### Subtraction:
To subtract matrices, we can simply subtract each value at the corresponding location. It's the exact same process as adding, except we are subtracting the values

{{< image classes="fancybox center clear" src="https://i.imgur.com/7A9PJ0M.png" thumbnail="https://i.imgur.com/7A9PJ0M.png" group="group:personal-matrices-research" thumbnail-width="40%" thumbnail-height="40%" title="Image showing subtraction of two matrices" >}}

We have to subtract each value at the matching location. Using the example above `A11`-`B11` is equivalent to 3-6. This will be position `11` of the subtracted matrix output.

### Multiplication:
Multiplying two matrices together requires the following condition to bear true:

**Columns in first matrix HAVE to be the SAME value as rows in second matrix**

Expanding on this. If we explore the next example it will be made easier to understand

{{< image classes="fancybox center clear" src="https://i.imgur.com/HOJNku8.png" thumbnail="https://i.imgur.com/HOJNku8.png" group="group:personal-matrices-research" thumbnail-width="40%" thumbnail-height="40%" title="Image showing A and B matrices dimensions" >}}

As shown above the columns of matrix A are the same as the rows on matrix B. If this condition does not hold true, then the matrices CANNOT be multiplied together.

the other two values (A row and B column) provide helpful information. They inform us that the output of the multiplication will be a matrix with 1 row and two columns.

To multiply two matrices we have to start by multiplying the first value at A11 with the vale at B11. We then multiply A12 bt B21. we repeat this until we have multiplied all the values in matrix A with every value in Matrix B.

{{< image classes="fancybox center clear" src="https://i.imgur.com/gIC7iJy.png" thumbnail="https://i.imgur.com/gIC7iJy.png" group="group:personal-matrices-research" thumbnail-width="40%" thumbnail-height="40%" title="Image showing A and B matrices dimensions" >}}

As seen from the example above the product of AxB is 60 44

{{< image classes="fancybox center clear" src="https://i.imgur.com/VweNN86.png" thumbnail="https://i.imgur.com/VweNN86.png" group="group:personal-matrices-research" thumbnail-width="40%" thumbnail-height="40%" title="Image showing product of A and B" >}}

Referencing my earlier point of "If this condition does not hold true than the matrices CANNOT be multiplied together." we can see this is the case when we try to do BxA. As B has 2 columns and A has 1 row, they do not match and thus the multiplication cannot take place.

{{< image classes="fancybox center clear" src="https://i.imgur.com/XfgKDZW.png" thumbnail="https://i.imgur.com/XfgKDZW.png" group="group:personal-matrices-research" thumbnail-width="20%" thumbnail-height="20%" title="Image showing AxB works and BxA doesnt" >}}

### Scalars
#### Multiplying By Scalars
Scalar multiplication is also very simple. You take a regular number and multiply it on every entry on the matrix.

{{< image classes="fancybox center clear" src="https://i.imgur.com/HuIH79u.png" thumbnail="https://i.imgur.com/HuIH79u.png" group="group:personal-matrices-research" thumbnail-width="40%" thumbnail-height="40%" title="Image showing matrix being multiplies by a scalar" >}}

As seen from above the matrix `A` has each value multiplies by 4. The output `4A` is shown above.

#### Addition With Scalars
Scalar multiplication is also very simple. You take a regular number and multiply it on every entry on the matrix.

{{< image classes="fancybox center clear" src="https://i.imgur.com/HuIH79u.png" thumbnail="https://i.imgur.com/HuIH79u.png" group="group:personal-matrices-research" thumbnail-width="40%" thumbnail-height="40%" title="Image showing matrix being multiplies by a scalar" >}}

As seen from above the matrix `A` has each value multiplies by 4. The output `4A` is shown above.

#### Subtraction With Scalars
Scalar multiplication is also very simple. You take a regular number and multiply it on every entry on the matrix.

{{< image classes="fancybox center clear" src="https://i.imgur.com/HuIH79u.png" thumbnail="https://i.imgur.com/HuIH79u.png" group="group:personal-matrices-research" thumbnail-width="40%" thumbnail-height="40%" title="Image showing matrix being multiplies by a scalar" >}}

As seen from above the matrix `A` has each value multiplies by 4. The output `4A` is shown above.

#
























































































































































































