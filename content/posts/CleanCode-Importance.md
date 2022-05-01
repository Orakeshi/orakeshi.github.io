---
title: "Importance Of Clean Code"
date: "01 May 2022"
showDate: true
comments: true
showSocial: true
showMeta: true
categories:
  - blog
tags:
  - oop
  - cleancode 
  - code
  - fundamentals
autoThumbnailImage: false 
thumbnailImagePosition: left
thumbnailImage: https://i.imgur.com/lgYxYnU.png
metaAlignment: center
---
  
On my journey to learn and become a better developer, I find myself researching matrices. Having never been exposed to matrices I will be approaching this learning as a complete beginner on the topic.

I will hopefully by the end of this blog have a better understanding about: What they are?, How they are used?, Basic operations and more!

{{< image classes="fancybox center clear" src="https://i.imgur.com/psQany9.png" thumbnail="https://i.imgur.com/psQany9.png" group="group:personal-matrices-research" thumbnail-width="70%" thumbnail-height="70%" title="Image of matrix transformations on a 3d axis" >}}

<!--more-->
{{< toc >}}

# Reason For Researching:
My reasons for researching matrices is due to several points. Other than the obvious points of "Wanting to be a better developer" or "Expand my knowledge"; My real answer is due to their large role being played on my daily life whilst I am blind to them.

What I mean by this is that I mainly spend my working days developing using [Unity3D](https://unity.com/). Within [Unity](https://unity.com/), it is common place for myself to affect an object in some way. Whether I am changing an objects transform via rotation/position I am using Vectors or Quaternions as part of the transform class.

However, several functions within unity use Matrices (Matrix4X4) such as Transform, Camera, Material and more. With this I want to develop an understanding of them

{{< image classes="fancybox center clear" src="https://i.imgur.com/EiQOtzW.png" thumbnail="https://i.imgur.com/EiQOtzW.png" group="group:personal-matrices-research" thumbnail-width="70%" thumbnail-height="70%" title="Unity matrix example" >}}

# Research, Findings & Development:
## What Are Matrices:
Put simply: A Matrix is an array of numbers. They are widely used throughout various sectors & environments. In geometry, matrices are used for representing geometric transformations such as rotations, positions etc.

Matrices are a convenient and compact way to represent large sets of numbers.

{{< image classes="fancybox center clear" src="https://i.imgur.com/qTP6KfB.png" thumbnail="https://i.imgur.com/qTP6KfB.png" group="group:personal-matrices-research" title="Image showing 2x4 matrix" >}}        

## Matrix Uses:
Matrices, as mentioned above, are widely used. One such example is in 3D computer graphics. {{< hl-text cyan >}} 4x4{{< /hl-text >}} transformation matrices are common place. These {{< hl-text orange >}} n+1{{< /hl-text >}} dimensional transformation matrices can be called affine transformation matrices, projective transformation matrices or non-linear transformation matrices (dependent on application)

The size of a matrix is defined by the number of rows and columns it contains. There is no limit to the numbers of rows and columns a matrix (in the usual sense) can have.

## Matrix Operations:
### Matrix Breakdown:
Before I dive into calculating certain operations of a given matrix, first I must break down and understand a matrix. By nature, they are very simple to understand. 

{{< image classes="fancybox center clear" src="https://i.imgur.com/ThGlnJU.png" thumbnail="https://i.imgur.com/ThGlnJU.png" group="group:personal-matrices-research" thumbnail-width="30%" thumbnail-height="30%" title="Image showing breakdown of a 2x3 matrix" >}}

As seen from the matrix above, they are very simple to understand. Matrices are written with row first, column second. This means the dimensions of the matrix above are {{< hl-text cyan >}} 2x3{{< /hl-text >}}

The bottom line of {{< hl-text cyan >}} A2,3{{< /hl-text >}} means in matrix of A find the entry at 23. After going two rows along and 3 columns down the answer is 5.

Below are some more examples of matrix breakdowns. I have written the dimensions for all the following matrices:
{{< image classes="fancybox center clear" src="https://i.imgur.com/By28m6x.png" thumbnail="https://i.imgur.com/By28m6x.png" group="group:personal-matrices-research" thumbnail-width="30%" thumbnail-height="30%" title="Image showing breakdown of a multitude of matrices" >}}

### Addition:
Now I understand the basic structure of a matrix I can move onto its operations. With addition, It's vital to point out that the {{< hl-text red >}} TWO MATRICES MUST{{< /hl-text >}} be the same size. The rows and columns must match in size else they cannot be added together.

{{< image classes="fancybox center clear" src="https://i.imgur.com/F56lXd2.png" thumbnail="https://i.imgur.com/F56lXd2.png" group="group:personal-matrices-research" thumbnail-width="40%" thumbnail-height="40%" title="Image showing addition of two matrices" >}}

To start adding you must first add the numbers at there matching position. Following the example above you would do {{< hl-text orange >}} 3+6{{< /hl-text >}} & {{< hl-text orange >}} 7+2{{< /hl-text >}} etc. After adding each value, the output will be the sum of both matrices.

### Subtraction:
To subtract matrices, we can simply subtract each value at the corresponding location. It's the exact same process as adding, except we are subtracting the values

{{< image classes="fancybox center clear" src="https://i.imgur.com/7A9PJ0M.png" thumbnail="https://i.imgur.com/7A9PJ0M.png" group="group:personal-matrices-research" thumbnail-width="40%" thumbnail-height="40%" title="Image showing subtraction of two matrices" >}}

We have to subtract each value at the matching location. Using the example above {{< hl-text cyan >}} A1,1 - B1,1{{< /hl-text >}} is equivalent to {{< hl-text orange >}} 3-6{{< /hl-text >}}. This will be position {{< hl-text cyan >}} 1,1{{< /hl-text >}} of the subtracted matrix output.

### Multiplication:
Multiplying two matrices together requires the following condition to bear true:

{{< hl-text red >}} Columns in the first matrix HAVE to be the SAME value as rows in the second matrix{{< /hl-text >}}

Expanding on this. If we explore the next example it will be made easier to understand

{{< image classes="fancybox center clear" src="https://i.imgur.com/HOJNku8.png" thumbnail="https://i.imgur.com/HOJNku8.png" group="group:personal-matrices-research" thumbnail-width="40%" thumbnail-height="40%" title="Image showing A and B matrices dimensions" >}}

As shown above the columns of matrix {{< hl-text cyan >}} A{{< /hl-text >}} are the same as the rows on matrix {{< hl-text cyan >}} B{{< /hl-text >}}. If this condition does not hold true, then the matrices {{< hl-text red >}} CANNOT{{< /hl-text >}} be multiplied together.

the other two values ({{< hl-text cyan >}} A{{< /hl-text >}} row and {{< hl-text cyan >}} B{{< /hl-text >}} column) provide helpful information. They inform us that the output of the multiplication will be a matrix with 1 row and two columns.

To multiply two matrices we have to start by multiplying the first value at {{< hl-text cyan >}} A1,1{{< /hl-text >}} with the value at {{< hl-text cyan >}} B1,1{{< /hl-text >}}. We then multiply {{< hl-text cyan >}} A1,2{{< /hl-text >}} by {{< hl-text cyan >}} B2,1{{< /hl-text >}}. we repeat this until we have multiplied all the values in matrix {{< hl-text cyan >}} A{{< /hl-text >}} with every value in Matrix {{< hl-text cyan >}} B{{< /hl-text >}}.

{{< image classes="fancybox center clear" src="https://i.imgur.com/gIC7iJy.png" thumbnail="https://i.imgur.com/gIC7iJy.png" group="group:personal-matrices-research" thumbnail-width="40%" thumbnail-height="40%" title="Image showing A and B matrices dimensions" >}}

As seen from the example above the {{< hl-text orange >}} product{{< /hl-text >}} of {{< hl-text cyan >}} AxB{{< /hl-text >}} is {{< hl-text orange >}} 60 44{{< /hl-text >}}

{{< image classes="fancybox center clear" src="https://i.imgur.com/VweNN86.png" thumbnail="https://i.imgur.com/VweNN86.png" group="group:personal-matrices-research" thumbnail-width="40%" thumbnail-height="40%" title="Image showing product of A and B" >}}

Referencing my earlier point of "If this condition does not hold true than the matrices CANNOT be multiplied together." we can see this is the case when we try to do {{< hl-text cyan >}} BxA{{< /hl-text >}}. As {{< hl-text cyan >}} B{{< /hl-text >}} has 2 columns and {{< hl-text cyan >}} A{{< /hl-text >}} has 1 row, they do not match and thus the multiplication cannot take place.

{{< image classes="fancybox center clear" src="https://i.imgur.com/XfgKDZW.png" thumbnail="https://i.imgur.com/XfgKDZW.png" group="group:personal-matrices-research" thumbnail-width="20%" thumbnail-height="20%" title="Image showing AxB works and BxA doesnt" >}}

### Scalars
#### Multiplying By Scalars
Scalar multiplication is also very simple. You take a regular number and multiply it on every entry on the matrix.

{{< image classes="fancybox center clear" src="https://i.imgur.com/HuIH79u.png" thumbnail="https://i.imgur.com/HuIH79u.png" group="group:personal-matrices-research" thumbnail-width="40%" thumbnail-height="40%" title="Image showing matrix being multiplies by a scalar" >}}

As seen from above the matrix {{< hl-text cyan >}} A{{< /hl-text >}} has each value {{< hl-text orange >}} multiplied{{< /hl-text >}} by 4. The output {{< hl-text cyan >}} 4A{{< /hl-text >}} is shown above.

#### Addition With Scalars
Adding two matrices that are multiplied by scalars is also simple. That being said the process is a bit longer that just multiplying a matrix by a scalar.

If we consider the following two matrices:
{{< image classes="fancybox center clear" src="https://i.imgur.com/54LolzB.png" thumbnail="https://i.imgur.com/54LolzB.png" group="group:personal-matrices-research" thumbnail-width="50%" thumbnail-height="50%" title="Image showing two random matrices" >}}

A sample question could follow something along the lines of:
{{< image classes="fancybox center clear" src="https://i.imgur.com/prMVz3H.png" thumbnail="https://i.imgur.com/prMVz3H.png" group="group:personal-matrices-research" thumbnail-width="20%" thumbnail-height="20%" title="Image showing a question of addition with scalar matrices" >}}

If we break down what is being asked from the question above; {{< hl-text cyan >}} 3A{{< /hl-text >}} simply means {{< hl-text orange >}} multiply{{< /hl-text >}} matrix {{< hl-text cyan >}} A{{< /hl-text >}} by {{< hl-text orange >}} 3{{< /hl-text >}}. {{< hl-text orange >}} Add{{< /hl-text >}} this result to matrix {{< hl-text cyan >}} B{{< /hl-text >}} {{< hl-text orange >}} multiplied by 7{{< /hl-text >}}. The process will look as follows:
{{< image classes="fancybox center clear" src="https://i.imgur.com/gLJy7b2.png" thumbnail="https://i.imgur.com/gLJy7b2.png" group="group:personal-matrices-research" thumbnail-width="50%" thumbnail-height="50%" title="Image showing a both matrices multiplied by there scalars" >}}

The image above shows that I simply needed to multiply the matrix by the provided scalar. I am now left with two matrices that can be added together.
{{< image classes="fancybox center clear" src="https://i.imgur.com/40TRlWx.png" thumbnail="https://i.imgur.com/40TRlWx.png" group="group:personal-matrices-research" thumbnail-width="40%" thumbnail-height="40%" title="Image showing the final result of the equation" >}}

#### Subtraction With Scalars
I will not go into as much detail about subtraction as it's the exact same process as addition. I will just show an image of my workings for subtraction with scalars.

{{< image classes="fancybox center clear" src="https://i.imgur.com/pZbAP6C.png" thumbnail="https://i.imgur.com/pZbAP6C.png" group="group:personal-matrices-research" thumbnail-width="40%" thumbnail-height="40%" title="Image showing result of two scalar matrices being subtracted from each other" >}}

### Inverse
Just like a number has a reciprocal ({{< hl-text orange >}} 8 is 1/8{{< /hl-text >}}) a matrix has an inverse. The inverse of a matrix is {{< hl-text cyan >}} A{{< /hl-text >}}{{< hl-text orange >}} ^-1{{< /hl-text >}}. When we multiply a matrix by its inverse we get the identity matrix. ({{< hl-text cyan >}} AxA{{< /hl-text >}}{{< hl-text orange >}} ^-1 = I{{< /hl-text >}})

An Identity matrix has the following properties:
- It is "square"
- It has 1s on the diagonal and 0s everywhere else
- Its symbol is the capital letter I
- Identity matrices can be 2x2, 3x3, 4x4 ...

There is not always an inverse of a matrix and thus the following equation defines the inverse and when it can be applied:
The inverse of a matrix {{< hl-text cyan >}} A{{< /hl-text >}} is {{< hl-text cyan >}} A{{< /hl-text >}}{{< hl-text orange >}} ^-1{{< /hl-text >}} when:

{{< hl-text cyan >}} AA{{< /hl-text >}}{{< hl-text orange >}} ^-1{{< /hl-text >}} = {{< hl-text cyan >}} A{{< /hl-text >}}{{< hl-text orange >}} ^-1{{< /hl-text >}}{{< hl-text cyan >}} A{{< /hl-text >}}={{< hl-text orange >}} I{{< /hl-text >}}

The use of Inverse is significant. Matrices do not have any concept of division. Due to this, multiplying by an inverse achieves the same thing as division.

#### Inverse Of 2x2
To calculate the inverse of a {{< hl-text cyan >}} 2x2{{< /hl-text >}} matrix, we start by writing out the formula for calculating an inverse of a {{< hl-text cyan >}} 2x2{{< /hl-text >}} matrix.

{{< image classes="fancybox center clear" src="https://i.imgur.com/1y3KZml.png" thumbnail="https://i.imgur.com/1y3KZml.png" group="group:personal-matrices-research" thumbnail-width="40%" thumbnail-height="40%" title="Formula to calculate the inverse of a 2x2 matrix" >}}

The image above shows the formula we need. The {{< hl-text orange >}} ad-bc{{< /hl-text >}} part of the equation is called the determinant and will be explored further soon.

To calculate the inverse we need to swap the positions of {{< hl-text orange >}} a & d{{< /hl-text >}}, put negatives in front of {{< hl-text orange >}} b & c{{< /hl-text >}} and then divide everything by the determinant.

Given an example data set of the following:
{{< image classes="fancybox center clear" src="https://i.imgur.com/dztssZH.png" thumbnail="https://i.imgur.com/dztssZH.png" group="group:personal-matrices-research" thumbnail-width="40%" thumbnail-height="40%" title="2x2 matrix to calculate inverse with" >}}

We start by calculating the determinant. This comes to {{< hl-text orange >}} 1/10{{< /hl-text >}} once we multiply add and subtract from {{< hl-text orange >}} bc{{< /hl-text >}}.
{{< image classes="fancybox center clear" src="https://i.imgur.com/FcPU1v2.png" thumbnail="https://i.imgur.com/FcPU1v2.png" group="group:personal-matrices-research" thumbnail-width="40%" thumbnail-height="40%" title="Image showing determinant being calculated" >}}

Once that has been calculated we simply multiply this by each value in the matrix. This gives us the answer to "what is the inverse if this {{< hl-text cyan >}} 2x2{{< /hl-text >}} matrix".
{{< image classes="fancybox center clear" src="https://i.imgur.com/wBeN16w.png" thumbnail="https://i.imgur.com/wBeN16w.png" group="group:personal-matrices-research" thumbnail-width="30%" thumbnail-height="30%" title="Answer of the 2x2 matrix" >}}

Whilst above is the answer, it is only 50% of the solution. The other half is proving that this is the correct answer. The inverse is only true if this formula holds:

{{< hl-text cyan >}} AA{{< /hl-text >}}{{< hl-text orange >}} ^-1 = I{{< /hl-text >}}

We use this calculated inverse & apply it to the formula above.
{{< image classes="fancybox center clear" src="https://i.imgur.com/uArSacm.png" thumbnail="https://i.imgur.com/uArSacm.png" group="group:personal-matrices-research" thumbnail-width="60%" thumbnail-height="60%" title="Image showing both the A matrix and the inverse of A being multiplied together" >}}

After multiplying and adding the values together the result is the following:
{{< image classes="fancybox center clear" src="https://i.imgur.com/mVbPwqR.png" thumbnail="https://i.imgur.com/mVbPwqR.png" group="group:personal-matrices-research" thumbnail-width="40%" thumbnail-height="40%" title="Image of both matrices multiplied and added together" >}}

After calculating the values the output is:
{{< image classes="fancybox center clear" src="https://i.imgur.com/JSupcS7.png" thumbnail="https://i.imgur.com/JSupcS7.png" group="group:personal-matrices-research" thumbnail-width="30%" thumbnail-height="30%" title="Image proving that the matrix of A^-1 is the inverse" >}}

Yay! the result is the identity matrix. This proves that the {{< hl-text cyan >}} A{{< /hl-text >}}{{< hl-text orange >}} ^-1{{< /hl-text >}} we calculated earlier is indeed the inverse.

#### Determinant Of A Matrix:
As shown in the previous topic of calculating an inverse for a {{< hl-text cyan >}} 2x2{{< /hl-text >}} matrix the determinant was shown. The determinant is a special number that can be calculated from a matrix.

For a matrix to have a determinant it must be square. Calculating the determinant for a {{< hl-text cyan >}} 2x2{{< /hl-text >}} matrix is very simple. Take the following example:
{{< image classes="fancybox center clear" src="https://i.imgur.com/2PKk1fB.png" thumbnail="https://i.imgur.com/2PKk1fB.png" group="group:personal-matrices-research" thumbnail-width="30%" thumbnail-height="30%" title="Image showing how to calculate determinant for 2x2 matrix" >}}

This is how you calculate the determinant for a {{< hl-text cyan >}} 2x2{{< /hl-text >}} matrix. It is used to help find the inverse of a matrix.

##### 2x2 Matrix Formula:
If we take a matrix of {{< hl-text cyan >}} A{{< /hl-text >}}.
{{< image classes="fancybox center clear" src="https://i.imgur.com/yPy1y4H.png" thumbnail="https://i.imgur.com/yPy1y4H.png" group="group:personal-matrices-research" thumbnail-width="30%" thumbnail-height="30%" title="Formula to calculate determinant of a 2x2 matrix" >}}
We can use the following formula to work out its determinant.

##### 3x3 Matrix Formula:
Whilst calculating the determinant for a {{< hl-text cyan >}} 3x3{{< /hl-text >}} matrix is slightly more complicated there is a pattern to it.
{{< image classes="fancybox center clear" src="https://i.imgur.com/0aa0RqZ.png" thumbnail="https://i.imgur.com/0aa0RqZ.png" group="group:personal-matrices-research" thumbnail-width="30%" thumbnail-height="30%" title="Image of a 3x3 matrix" >}}

Above shows what a typical {{< hl-text cyan >}} 3x3{{< /hl-text >}} matrix looks like. If we use this for calculating the determinant we can use the following formula:
{{< image classes="fancybox center clear" src="https://i.imgur.com/Syi0sp2.png" thumbnail="https://i.imgur.com/Syi0sp2.png" group="group:personal-matrices-research" thumbnail-width="50%" thumbnail-height="50%" title="Image showing formula for calculating determinant of 3x3 matrix" >}}

Within the formula, the vertical bars represent the word "determinant". This makes understanding how to find the determinant of a 3x3 matrix a lot simpler. Additionally, I have drawn a visual representation below:
{{< image classes="fancybox center clear" src="https://i.imgur.com/5TjmV2X.png" thumbnail="https://i.imgur.com/5TjmV2X.png" group="group:personal-matrices-research" thumbnail-width="50%" thumbnail-height="50%" title="Image of the formula to calculate determinant visualised" >}}

The image above showcases the way to calculate the {{< hl-text cyan >}} 3x3{{< /hl-text >}} matrix. It's similar to that of the {{< hl-text cyan >}} 2x2{{< /hl-text >}} matrix, except there are a few more steps.

- We start by multiplying a by the determinant of the {{< hl-text cyan >}} 2xx2{{< /hl-text >}} matrix that is {{< hl-text red >}} NOT{{< /hl-text >}} in as row.
- The same is done for {{< hl-text orange >}} b & c{{< /hl-text >}}. We calculate the {{< hl-text cyan >}} 2x2{{< /hl-text >}} matrix where {{< hl-text orange >}} b & c{{< /hl-text >}} are not in the rows
- We then sum up all the results making sure to put a minus symbol in front of the {{< hl-text orange >}} b{{< /hl-text >}}

This method is the [Laplace expansion](https://en.wikipedia.org/wiki/Laplace_expansion) and is super simple to understand and apply to calculate the determinant of a given matrix

Applying the above learned knowledge to a matrix of {{< hl-text cyan >}} B{{< /hl-text >}} we get the following:
{{< image classes="fancybox center clear" src="https://i.imgur.com/vWjm8uv.png" thumbnail="https://i.imgur.com/vWjm8uv.png" group="group:personal-matrices-research" thumbnail-width="30%" thumbnail-height="30%" title="Image of a sample matrix B" >}}

With the matrix above with the values we can apply the formula learned. With this we can calculate the determinant.
{{< image classes="fancybox center clear" src="https://i.imgur.com/W8eJnbB.png" thumbnail="https://i.imgur.com/W8eJnbB.png" group="group:personal-matrices-research" thumbnail-width="60%" thumbnail-height="60%" title="Image of the formula applied to matrix B" >}}

The image showcases the determinant of the given matrix {{< hl-text cyan >}} B{{< /hl-text >}}.

##### 4x4 Matrix Formula:
{{< hl-text cyan >}} 4x4{{< /hl-text >}} doesn't need much explanation. As we are using [Laplace's expansion](https://en.wikipedia.org/wiki/Laplace_expansion) it's the same as a {{< hl-text cyan >}} 3x3{{< /hl-text >}} with a few more steps.
The pattern is the following:
- Plus {{< hl-text orange >}} a{{< /hl-text >}} times the determinant of the matrix not in {{< hl-text orange >}} a{{< /hl-text >}}'s row or column
- Minus {{< hl-text orange >}} b{{< /hl-text >}} times the determinant of the matrix that is not in {{< hl-text orange >}} b{{< /hl-text >}}'s row or column
- Plus {{< hl-text orange >}} c{{< /hl-text >}} times the determinant of the matrix that is not in {{< hl-text orange >}} c{{< /hl-text >}}'s row or column
- Minus {{< hl-text orange >}} d{{< /hl-text >}} times the determinant of the matrix that is not in {{< hl-text orange >}} d{{< /hl-text >}}'s row or column

As mentioned already, this is very similar to that of a {{< hl-text cyan >}} 3x3{{< /hl-text >}} matrix.
The pattern above is {{< hl-text orange >}} +-+-{{< /hl-text >}}

The formula to remember is as follows:
{{< image classes="fancybox center clear" src="https://i.imgur.com/8NGnbvx.png" thumbnail="https://i.imgur.com/8NGnbvx.png" group="group:personal-matrices-research" thumbnail-width="60%" thumbnail-height="60%" title="Image of the formula for 4x4 matrix" >}}

Whilst I won't work out a {{< hl-text cyan >}} 4x4{{< /hl-text >}} matrix I wil show the initial steps. If we take the following matrix {{< hl-text cyan >}} C{{< /hl-text >}}
{{< image classes="fancybox center clear" src="https://i.imgur.com/pCtOCtJ.png" thumbnail="https://i.imgur.com/pCtOCtJ.png" group="group:personal-matrices-research" thumbnail-width="30%" thumbnail-height="30%" title="Image showing sample data for matrix C" >}}

We have to take the {{< hl-text cyan >}} 4x4{{< /hl-text >}} matrix and using the formula split it up in 4 {{< hl-text cyan >}} 3x3{{< /hl-text >}} matrices:
{{< image classes="fancybox center clear" src="https://i.imgur.com/mIMuQu0.png" thumbnail="https://i.imgur.com/mIMuQu0.png" group="group:personal-matrices-research" thumbnail-width="40%" thumbnail-height="40%" title="Image showing 4x4 matrix being slit into 4 3x3 matrices" >}}

We would then proceed to use the {{< hl-text cyan >}} 3x3{{< /hl-text >}} formula on each of the given {{< hl-text cyan >}} 3x3{{< /hl-text >}} matrices. 

### Inverse Of A Matrix (3x3;4x4):
There are a set of standardised steps in order to calculate the inverse of a matrix. We use the following rules in order:
- Calculate the Matrix of Minors,
- Turn that into the Matrix of Cofactors,
- Then Adjugate,
- Multiply that by 1/Determinant

These rules seem complex/hard to follow but with the example below, it's easy.

Let's find the Inverse of {{< hl-text cyan >}} A{{< /hl-text >}} (image below):
{{< image classes="fancybox center clear" src="https://i.imgur.com/jsBCgC5.png" thumbnail="https://i.imgur.com/jsBCgC5.png" group="group:personal-matrices-research" thumbnail-width="40%" thumbnail-height="40%" title="Image showing 3x3 matrix to find the inverse" >}}

We start by calculating the Matrix of Minors. To do this we take each value in the matrix, ignore its column and row and calculate the determinant for the remaining values. Example of this is below. I have calculated the matrix of minors for the first two values in Matrix {{< hl-text cyan >}} A{{< /hl-text >}}
{{< image classes="fancybox center clear" src="https://i.imgur.com/cijXZ4V.png" thumbnail="https://i.imgur.com/cijXZ4V.png" group="group:personal-matrices-research" thumbnail-width="40%" thumbnail-height="40%" title="Image of the matrix of minors for first two values" >}}

The finished result of all the matrix of minors is the following:
{{< image classes="fancybox center clear" src="https://i.imgur.com/qjUSwLb.png" thumbnail="https://i.imgur.com/qjUSwLb.png" group="group:personal-matrices-research" thumbnail-width="30%" thumbnail-height="30%" title="Image of the matrix of minors for matrix A" >}}

Now we have the matrix of minors we move onto the next step. This is turning the matrix of minors into the Matrix of Cofactors
{{< image classes="fancybox center clear" src="https://i.imgur.com/ISfazEP.png" thumbnail="https://i.imgur.com/ISfazEP.png" group="group:personal-matrices-research" thumbnail-width="60%" thumbnail-height="60%" title="Image of the matrix of cofactors" >}}

This is very simple to achieve. We simply follow the pattern of "{{< hl-text orange >}} +-+-{{< /hl-text >}}" for the next row we start do "{{< hl-text orange >}} -+-+{{< /hl-text >}}" and then switch again back to "{{< hl-text orange >}} +-+-{{< /hl-text >}}" We repeat this checkered affect replacing each value with a "{{< hl-text orange >}} +{{< /hl-text >}}" or "{{< hl-text orange >}} -{{< /hl-text >}}" sign.
{{< image classes="fancybox center clear" src="https://i.imgur.com/TM0lLWe.png" thumbnail="https://i.imgur.com/TM0lLWe.png" group="group:personal-matrices-research" thumbnail-width="40%" thumbnail-height="40%" title="Image of the Adjugate" >}}

Above is the image of the Adjugate process. We transpose all elements of the Matrix of Cofactors. We swap there positions diagonally.

Finally, we have to find the determinant of the original matrix. Whilst I can use the original matrix and calculate the determinant like learned previously, it's simpler to just multiply the elements of the top row from the original matrix with the top row from the matrix of cofactors.
This means we multiply the following:

Elements of Top row: 3, 0, 2
Cofactors for top row: 2, -2, 2

Below is value of the determinant
{{< image classes="fancybox center clear" src="https://i.imgur.com/uzXlHD7.png" thumbnail="https://i.imgur.com/uzXlHD7.png" group="group:personal-matrices-research" thumbnail-width="40%" thumbnail-height="40%" title="Determinant of matrix" >}}

Below is the final image. This is where we calculate the inverse of the matrix. This is simple using the formula purposed previously.
{{< image classes="fancybox center clear" src="https://i.imgur.com/vDNVHLj.png" thumbnail="https://i.imgur.com/vDNVHLj.png" group="group:personal-matrices-research" thumbnail-width="40%" thumbnail-height="40%" title="Calculated inverse of matrix A" >}}

## Understanding & Notes:
My final thoughts and takeaways from my learnings are described below:
- I know what matrices are and there uses
- I can use Scalars to multiply matrices by
- I can apply basic operations when working with two matrices
- I can now successfully apply basic operations for matrices of {{< hl-text cyan >}} 3x3{{< /hl-text >}}, {{< hl-text cyan >}} 3x3{{< /hl-text >}}, {{< hl-text cyan >}} 4x4{{< /hl-text >}}
- I can find the inverse of matrices and why its important
- &MORE

## Resources Used:
- [Maths Is Fun](https://www.mathsisfun.com/)
- [The Organic Chemistry Tutor](https://www.youtube.com/channel/UCEWpbFLzoYGPfuWUMFPSaoA)
- [Wikipedia](https://en.wikipedia.org/wiki/Matrix_(mathematics))