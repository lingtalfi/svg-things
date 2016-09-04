SVG matrices
==================
2016-09-04


These are the notes I collected on the web in the process of understanding what a matrix is and how to use it
in regard with SVG (needless to say matrices were an unknown concept to me until then).






Explanations from MDN
------------------------

https://developer.mozilla.org/en-US/docs/Web/SVG/Attribute/transform

In this page, they give you the matrix formulae to apply:


```
--     --       --     --  --     --       --                 --
| Xprev |       | a c e |  | Xnew  |       | aXnew + cYnew + e |
| Yprev |  =    | b d f |  | Ynew  |   =   | bXnew + dYnew + f |
| 1     |       | 0 0 1 |  | 1     |       |        1          |
--     --       --     --  --     --       --                 --
```


By reading more about matrices, especially the "physical explanation" section below,
you will understand the intermediary step:



```
--     --       --     --  --     --      --     --   --     --   --  --        --                --
| Xprev |       | a c e |  | Xnew  |      | aXnew |   | cYnew |   | 1e |       | aXnew + cYnew + e |
| Yprev |  =    | b d f |  | Ynew  |   =  | bXnew | + | dYnew | + | 1f |   =   | bXnew + dYnew + f |
| 1     |       | 0 0 1 |  | 1     |      | 0Xnew |   | 0Ynew |   | 1  |       |        1          |
--     --       --     --  --     --      --     --   --     --   --  --       --                 --
```


Now with this knowledge I can understand:

- how the translate movement is equivalent to matrix (1 0 0 1 x y)
- how the scale movement is equivalent to matrix (x 0 0 y 0 0)


But for the rotate matrix, which is represented as below, I wanted more proof.

```
--                  --
| cos(a)  -sin(a)  0 |
| sin(a)  cos(a)   0 |
| 0       0        1 |
--                  --
```

It turns out that in a right triangle with an hypothenuse of 1 (which is apparently/fortunately what we are
dealing with as far as SVG matrices are concerned), the cos is just (by definition) the value of the adjacent side
you read on the x axis, and the sinus is just the value of the opposite side you read on the y axis.



Wikipedia then gives a concrete resolution of that matrix with an angle of 90 degrees, which has simple cos and sin values.

https://en.wikipedia.org/wiki/Rotation_matrix

The transformation around the z-axis (2d rotation) has the following matrix:

```
--                  --
| cos(a)  -sin(a)  0 |
| sin(a)  cos(a)   0 |
| 0       0        1 |
--                  --
```

(which fortunately confirms what the mdn documentation was saying)
And then applying to a 90 degrees angle:

```
        -- --        --                    --  -- --        --        --  -- --       -- --
        | 1 |        | cos(90)  -sin(90)  0 |  | 1 |        | 0  -1  0 |  | 1 |       | 0 |
Rz(90)  | 0 |   =    | sin(90)  cos(90)   0 |  | 0 |   =    | 1   0  0 |  | 0 |   =   | 1 |
        | 0 |        | 0        0         1 |  | 0 |        | 0   0  1 |  | 0 |       | 0 |
        -- --        --                    --  -- --        --        --  -- --       -- --
```

So although not a proof, it works at least for a 90 degrees angle.



More on why those rotation values here:

https://www.youtube.com/watch?v=7jDy2IQV3C4







Physical explanation
-------------------------

This video explains the why, where the matrix comes from.

https://www.youtube.com/watch?v=IrggOvOSZr4

Below is a text transcription of the video.


Suppose we have a number.
By changing the value of this one number, we can represent any point in a one dimensional space.
Now suppose we have two numbers.
By changing the values of these two numbers, we can represent any point in a 2 dimensional space.
Now suppose we have three numbers.
We can now represent any point in a 3 dimensional space.

Notation:


```
-- --
| 3 |
| 5 |
| 4 |
-- --
```


Similarly, if we have a set of four numbers, we can represent any point in a 4 dimensional space.
If we have a set of five numbers, we can represent a point in 5 dimensional space.
And if we have an infinite set of numbers, we can represent a point in a space with an infinite number of dimensions.



Regardless of how many dimensions we have, each set of numbers which represents a point in space can be thought
of as an arrow, which we will call a vector.

```
-- --
| 3 |
| 5 |
| 4 |
-- --
```

Each vector has a length and a direction.
If we multiply the vector by a number, the vector's length will change, but its direction will stay the same.

```
   -- --        -- ---
   | 3 |        | 6  |
2x | 5 |   =>   | 10 |
   | 4 |        | 8  |
   -- --        -- ---
```


If we add two vectors together, the result is as shown. (3:22)

```
-- --   -- --        -- --
| 3 |   | 0 |        | 3 |
| 5 | + | 0 |   =>   | 5 |
| 4 |   | 4 |        | 8 |
-- --   -- --        -- --
```


Each vector can be thought of as the sum of vectors pointed in each of the different dimensions.

```
-- --          -- --   -- --   -- --
| 3 |          | 3 |   | 0 |   | 0 |
| 5 |   <=>    | 0 | + | 5 | + | 0 |
| 8 |          | 0 |   | 0 |   | 8 |
-- --          -- --   -- --   -- --
```

And each of the vectors pointed in each of the dimensions can be thought as a vector a length of one multiplied
by a number.

```
-- --           -- --      -- --      -- --
| 3 |           | 1 |      | 0 |      | 0 |
| 5 |   <=>  3x | 0 | + 5x | 1 | + 8x | 0 |
| 8 |           | 0 |      | 0 |      | 1 |
-- --           -- --      -- --      -- --
```


There are many transformations that can be applied to a vector.
For instance, the vector can be rotated in space around the z-axis, by a certain number of degrees.
Suppose we apply this rotation to a group of vectors.
And then we add all the vectors together.

Now suppose that instead of starting out by rotating the vectors, we instead first add all the vectors together,
and then afterwards we rotate the result.
It turns out that final vector that is produced is the same as before.

If the final vector that is produced is the same regardless of whether the vectors are added together before
or after the transformation, then we say that this is a linear transformation.


Linear Transformation Definition:
- T(X+Y) = T(X) + T(Y)
- T(aX) = aT(X)


Another example of a linear transformation is having the vector stretched or squeezed as shown. (6:21)
Or having the vector flipped into this mirror image of itself.
Or having the vector stretched in this dimension, and then flipped.
In each of these cases, the final vector that is produced is the same regardless of whether the vectors are
added together before or after the transformation.

This is the definition of a linear transformation.

We can represent a linear transformation mathematically b showing how the transformation would affect vectors with a
length of one, pointed in each of the dimensions.


The original vectors are vectors with a length of one, pointed in each of the dimensions.

```
-- --  -- --  -- --
| 1 |  | 0 |  | 0 |
| 0 |  | 1 |  | 0 |
| 0 |  | 0 |  | 1 |
-- --  -- --  -- --
```

After the transformation (stretched and flipped) is applied, the new vectors are the result.

```
-- --  -- --  -- --
| 0 |  | -1|  | 0 |
| -1|  | 0 |  | 0 |
| 0 |  | 0 |  | 2 |
-- --  -- --  -- --
```

Suppose we take each of the new vectors that result from this, and we line up all their numbers together as is shown.

```
--           --
| 0   -1   0  |
| -1   0   0  |
| 0    0   2  |
--           --
```


We now have what we call a matrix.
All linear transformations can be represented with a matrix.

When we apply the transformation, we say that we are multiplying the vector by the matrix, and we represent it like this.

```
--           --   --  --
| 0   -1   0  |   | X1 |
| -1   0   0  |   | X2 |
| 0    0   2  |   | X3 |
--           --   --  --
```


We defined the numbers in each column of the matrix based on the result of the transformation of vectors
of length one in each of the dimensions.

Therefore, when the matrix is multiplied by a vector of length one in one of the dimensions, the result is
as follows.


```
--           --   -- --            -- --      -- --      -- --               -- --
| 0   -1   0  |   | 1 |            | 0 |      | -1|      | 0 |               | 0 |
| -1   0   0  |   | 0 |   =>    1x | -1| + 0x | 0 | + 0x | 0 |      =>       | -1|
| 0    0   2  |   | 0 |            | 0 |      | 0 |      | 2 |               | 0 |
--           --   -- --            -- --      -- --      -- --               -- --
```


If we want to find out how the transformation affects a vector pointed in one of the dimensions that is not
length 1, then we can do the following.

```
--           --   -- --            -- --      -- --      -- --               -- --
| 0   -1   0  |   | 0 |            | 0 |      | -1|      | 0 |               | -5|
| -1   0   0  |   | 5 |   =>    0x | -1| + 5x | 0 | + 0x | 0 |      =>       | 0 |
| 0    0   2  |   | 0 |            | 0 |      | 0 |      | 2 |               | 0 |
--           --   -- --            -- --      -- --      -- --               -- --
```

And if we want to find out how the transformation affects any vector of any length pointed in any direction,
then we can do this as shown.

```
--           --   -- --            -- --      -- --      -- --               -- --
| 0   -1   0  |   | 3 |            | 0 |      | -1|      | 0 |               | -5|
| -1   0   0  |   | 5 |   =>    3x | -1| + 5x | 0 | + 4x | 0 |      =>       | -3|
| 0    0   2  |   | 4 |            | 0 |      | 0 |      | 2 |               | 8 |
--           --   -- --            -- --      -- --      -- --               -- --
```

This is how a matrix is multiplied by a vector, and it is how all linear transformations can be computed.


This is true for four dimensions.
For five dimensions.
And for an infinite number of dimensions.



Mathematical pragmatic
---------------------------

This link explains how to multiply matrices (which is needed to understand svg matrices)
in a pragmatic (tutorialish) way.

https://www.mathsisfun.com/algebra/matrix-multiplying.html


Basically:


```
--     --       --     --               --        --
| 1 2 3 |   x   | 7  8  |    =>         | 58   64  |
| 4 5 6 |       | 9  10 |               | 139  154 |
--     --       | 11 12 |               --        --
                --     --
```

Because:

- (1, 2, 3) . (7, 9, 11) = 1×7 + 2×9 + 3×11 = 58
- (1, 2, 3) . (8, 10, 12) = 1×8 + 2×10 + 3×12 = 64
- (4, 5, 6) . (7, 9, 11) = 4×7 + 5×9 + 6×11 = 139
- (4, 5, 6) . (8, 10, 12) = 4×8 + 5×10 + 6×12 = 154



