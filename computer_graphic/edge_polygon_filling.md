# AET

In AET, each edge is represented by a **table node**.

- $x_0$: The **minimum $x$ coordinate** of this line.
- $y_{max}$: The **maximum $y$ coordinate** of this line.
- $k^{-1}$: **1 divided by slope of this line**.
- `next` Pointer to next node.

And each table node are in the group of its $y_{min}$ coordinate.

![AET Basic Edge Table](https://github.com/user-attachments/assets/d6c975b3-0527-4255-9e1c-fb8bf088871a)

# Improved AET

Some points have two edges duplicated, which could lead to problem, below is improved version:

![AET Table Improved](https://github.com/user-attachments/assets/b1a96eb2-3d94-4658-a9bf-62496864ee2e)
