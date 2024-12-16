# DDA

DDA, stands for _Digital Differential Analyzer_. This line-drawing method determines the choice of next point based on slope $k$ of this line.

$$
k = \frac{\Delta y}{\Delta x}
$$

## Choose Main Axis

For precision concern, we choose the axis with larger range as **main axis**. Every step, we go **1 single unit in main axis**.

## Value Update

Consider $x$ as the main axis:

$$
\begin{aligned}
x^{i+1} &= x^{i} + 1 \\
y^{i+1} &= y^{i} + k \\
\end{aligned}
$$

> This method introduce error, since floating point number could not stored without precision lose.

# Central Bresenham

## Choose Main Axis

Same as _DDA_ algorithm.

## Error Item

Same, we go **one unit per time in main axis**.

In another axis, we use an **error item $d$**.

Initially:
$$d = \Delta x - 2 \Delta y$$

$d<0$: Choose $y+1$, then:

$$d = d-2\Delta y + 2 \Delta x$$

> An additional $2\Delta x$

$d \ge 0$: Choose $y$, then:

$$
d = d - 2 \Delta y
$$

# Bresenham

Just another type of Bresenham algorithm:

Error item $e$.

Initially, $e= - \Delta x$

$e \le 0$: Choose $y$, then:

$$
e = e + 2 \Delta y
$$

$e > 0$: Choose $y+1$, then:

$$
e = e + 2 \Delta y - 2 \Delta x
$$

# Drawing Circle

```
Init: x=0, y=R, e=1-R
while not finish:
    Draw(x, y)

    if e<0:
        e=e+2x+3
        x=x+1, y=y    // choose middle right
    elif e>=0:
        e=e+2(x-y)+5
        x=x+1, y=y-1  // choose bottom right
```

- Update `e` first, update `x, y` afterward.
  - This means, we **use old value when updating `e` (error)**

# Bresenham Conclusion

|         | Central | Bresenham |
|---------|---------|-----------|
| Initial | X-2Y    | -X        |
| <       | -2Y+2X  | +2Y       |
| =       | >       | <         |
| >       | -2Y     | +2Y-2X    |

## Process

Central

**`Put` -> `Choose` -> `Update`** -> `Put` -> `...`

Bresenham

**`Put` -> `+2Y` -> `Choose` -> `-2X(if need)`** -> `Put` -> `...`

In Bresenham, the **update of error $e$ was seperated into two part**.

> `Put` will use result of previous `Choose`.
>
> First `Put` will put at $x_0, y_0$
