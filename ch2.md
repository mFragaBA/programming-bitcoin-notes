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

## Point Addition

We can define an "addition" operator of two points in a curve which we can apply on two points of the curve and get a third one in return. A particular property we'll assume is that any line crosses the elliptic curve in either $1$ or $3$ points (there are special cases though, we'll address those later)

So, for any two points $P_1$ and $P_2$, we define $P_1 + P_2$ as:

- Find the point intersecting the elliptic curve a third time by drawing a line through $P_1$ and $P2$.
- Reflect the point over the $x$-axis.

[](img/ch2-addition.png)

### Some Properties

Point addition satisfies:

- Identity ($I + A = A$)
- Commutativity ($A + B = B + A$)
- Associativity ($(A + B) + C = A + (B + C)$)
- Invertibility ($A + (-A) = I$)

To Code point addition we're going to split it into three cases:
- When the points are in a vertical line or using the identity point
- When points are not equal nor in a vertical line
- When the two points are equal

To represent Infinity we can use `None` in python (and in Rust as well).

[](img/ch2-addition-opposites.png)

- Define Infinity as the zero element
- If both points are different but have the same $x$ value return the zero element (`None`)
- When $x_1 != x_2$ (and are not additive inverses) we have (this required a bunch of algebraic manipulation, so you might want to check the book for a detailed explanation):
    - $s = \frac{(y_2 - y_1)}{x_2 - x_1}$ 
    - $x_3 = s^2 - x_1 - x_2$
    - $y_3 = s(x_1 - x_3) - y_1$

## Exercise 4

For the curve $y^2 = x^3 + 5x + 7$, what is $(2, 5) + (-1, -1)$?
- $s = \frac{-6}{-3} = 2$
- $x_3 = 4 - 2 + 1 = 3$
- $y_3 = 2(2 - 3) - (-1) = -1$
So the result is $(3, -1)$

## Point Addition for when $P_1 = P_2$
In this case, we have to calculate the tangent to the point and find the point that intersects with the curve, as in this case:

[](img/ch2-addition-equal.png)

- $s = \frac{(3 x_1^2 + a)}{2y_1}$
- $x_3 = s^2 - 2x_1$
- $y_3 = s(x_1 - x_3) - y_1$

## Exercise 6

For the curve $y^2 = x^3 + 5x + 7$, what is $(-1, -1) + (-1, -1)$?

- $s = \frac{(3 1 + 5)}{2(-1)} = -4$
- $x_3 16 + 2 = 18$
- $y_3 = (-4)(-19) + 1 = 77$

So the resulting point is $(18, 77)$.

## One more exception

There's a last case we have to take into account, which is when the tangent line is vertical, like this:

[](img/ch2-addition-vertical-tangent.png)

This only happens if $P_1 = P_2$ and the $y$ coordinate is $0$, in which case we return the point at infinity (`None`).
