
In this article, we are going to discuss basic **Transformation Matrix** in 2D plain.

# Usage Of Transformation Matrix

The basic usage of this matrix is to **multiply with a Coordination Vector** to perform 2D transform.

$$
\vec{x'} = \vec{x} A
$$

As we already known, the **order of vector and matrix matters** in this case. That means matrix could be different based on where you decided to put it, left or right.

All matrixes shown in this article is **put at the right side at default like$xA$**, in which $x$ should be a row vector.

## Translate

$$
\begin{bmatrix}
1 & 0 & 0 \\
0 & 1 & 0 \\
x & y & 1
\end{bmatrix}
$$

## Scale By Origin

$$
\begin{bmatrix}
s_x & 0 & 0 \\
0 & s_y & 0 \\
0 & 0 & 1
\end{bmatrix}
$$

## Rotate By Origin

### 2D

$$
\begin{bmatrix}
\cos\theta & \sin\theta & 0 \\
-\sin\theta & \cos\theta & 0 \\
0 & 0 & 1
\end{bmatrix}
$$

### 3D

About x-axis:

$$
\begin{bmatrix}
1 & 0 & 0 & 0 \\
0 & \cos\theta & \sin\theta & 0 \\
0 & -\sin\theta & \cos\theta & 0 \\
0 & 0 & 0 & 1
\end{bmatrix}
$$

About y-axis:

$$
\begin{bmatrix}
\cos\theta & 0 & -\sin\theta & 0 \\
0 & 1 & 0 & 0 \\
\sin\theta & 0 & \cos\theta & 0 \\
0 & 0 & 0 & 1
\end{bmatrix}
$$

About z-axis:

$$
\begin{bmatrix}
\cos\theta & \sin\theta & 0 & 0 \\
-\sin\theta & \cos\theta & 0 & 0 \\
0 & 0 & 1 & 0 \\
0 & 0 & 0 & 1
\end{bmatrix}
$$

## Shear

$$
\begin{bmatrix}
1 & k_y & 0 \\
k_x & 1 & 0 \\
0 & 0 & 1
\end{bmatrix}
$$
