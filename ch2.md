# Chapter 2 - Elliptic Curves

## Definition
They are 2 variable equations:

$$
y^2 = x^3 + ax + b
$$

One the right side, it seems like a cubic equation, but the difference is that on the left side you have a $y^2$ instead of $y$. So, instead of:

[](img/ch2-cubic-graph.png)

you get something like:

[](img/ch2-elliptic-graph.png)

This is not always true (it could actually be disjoint), but we're going to focus on a subset of all the possible elliptic curves which have a shape similar to the one in the picture.

## Representation

As we saw, one way of representing our elliptic curve is by writing its full definition. For example, bitcoin uses $y^2 = x^3 + 7$. This is equivalent to setting of $a = 0, b = 7$. 

## Exercise 1
Determine which of these points are on the curve $y^2 = x^3 + 5x + 7$:

- $(2, 4)$
    - No, it is not. One way of checking this is by noticing that the left side of the equation would be even but the right side is going to be odd.
- $(-1, -1)$
    - Yes, since $(-1)^2 = 1 = -1 - 5 + 7 = (-1)^3 + 5 (-1) + 7$.
- $(18, 77)$
    - Yes (just use a calculator). 
- $(5, 7)$
    - No, since $5^3 = 125$ is already greater than $7^2 = 49$ .
    -
## Exercise 2
Write the `__ne__` method for Point.

```python3
def __ne__(self, other):
    return !self.eq(other) # is this cheating?
```
