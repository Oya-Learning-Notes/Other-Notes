# Iterations

For all of the following 3 approaches, the first step for us is to **convert the equations system into a set of fixed-point equations**. See example below:

![image](https://github.com/user-attachments/assets/10a09ee5-da11-4d1f-82ca-d61949b37298)

Note:

- Coefficient for the fix point $x_i$ should be $1$.
- Single $x_i$ at left. All remaidars at right.

## Jacobi Interation

The most fundamental way of this category. Directly use the fix-points equations to iterate value.

## Gauss-Seidel

Just use newly calculated $x_1, x_2, \cdots, x_{i-1}$ when calculating $x_i$.

## SOR

Gauss-Seidel with a factor $\omega$. (Equivalent to G-S when $\omega = 1$)

$$
x^{k+1} = \omega \cdot GS(x^{k+1}) + (1 - \omega) \cdot x^k
$$

In which, $GS(x^{k+1})$ represents the iterations result of $x^{k+1}$ when using pure Gauss-Seidel approach.

## Examples

![image](https://github.com/user-attachments/assets/fe297bf8-d6fe-4fdc-b249-ded8b8835f6c)

![image](https://github.com/user-attachments/assets/74bd3cd8-9212-473e-9598-743a4008ef07)

## Matrix Representations

We could represents all iteration approaches using matrix $B$ and vectors $\vec{x}, \vec{b}$

![image](https://github.com/user-attachments/assets/5060bebc-f1ca-4697-9fd3-6b71f7ab73ae)

Above is the example of Jacobi iteration. Note that **how $L$ and $U$ change the symbol of original linear equation matrix coefficients**.

To be more unified, we use the following patterns:

$$
\vec{x}^{(k+1)} = B \vec{x}^{(k)} + \vec{b'}
$$

And the $B$ and $\vec{b'}$ could changes across different approaches.

- Jacobi: 
	- $B = D^{-1}(L + U)$
	- $\vec{b'} = D^{-1} \vec{b}$
- Gauss-Seidel
	- $B = (D- L)^{-1} \cdot U$
	- $\vec{b'} = (D- L)^{-1} \vec{b}$
- SOR
	- $B = (D- \omega L)^{-1} [(1 - \omega)D + \omega U]$
	- $\vec{b'} = (D- \omega L)^{-1} \vec{b}$
(Note that the jacobi forms actually just the fix-point equations forms lol)

Following showing how we get the Gauss-Seidel formula and SOR formula.

![image](https://github.com/user-attachments/assets/45336a64-1e41-4e12-b941-d12d6e262fa7)

![image](https://github.com/user-attachments/assets/33cce5e6-dbc7-40a7-962a-890b12d23917)

# Convergence Check

There are several ways to check the convergence of a Iteration method.

## Metrix Norms

Matrix norms are properties of a matrix.

列范数 (计算 每一列元素 的 绝对值之和，取最大): 

$$
||A||_1 = \max_{1 \leq j \leq n} \sum_{i=1}^n |a_{ij}|
$$

行范数  (计算 每一行元素 的 绝对值之和，取最大): 

$$
||A||_\infty = \max_{1 \leq i \leq n} \sum_{j=1}^n |a_{ij}|
$$

2 范数 (A^TA的 最大特征值 的 平方根):

$$
||A||_2 = \sqrt{ \lambda_{max}(A^TA)}
$$

F 范数（可以理解为 欧式范数 直接推广）**（不是算子范数）**: 

$$
||A||_F = \sqrt{\sum_{i,j=1}^n a_{ij}^2}
$$

![image](https://github.com/user-attachments/assets/5990445e-4179-4567-9fe4-9f6f93658cf5)

## Eigenvalue

For matrix $A$, let's say eigenvalues are $\lambda$, then:

$$
det(\lambda I - A) = 0
$$

Just calculate the $\lambda$

## Spectral Radius

$$
\rho(A) = \max_{1 \leq i \leq n} |\lambda_i|
$$

For a matrix, the Spectral value has strong relationship to the Matrix Norms:

$$
\rho(A) \leq \|A\|
$$

In which, $||A||$ could be all Operator norm (All norms above, $||A||_1, ||A||_2, ||A||_\infty$ expect $||A||_F$)

## Convergence

Finally, we could use the tools above to determine the convergence of the Iteration Method.

There are several ways to determine

- **Diagonally dominant**
	- Strict Column/Row dominant 
	- The $|x|$ at the diag bigger than any other in the same row/column
	- Work for Jacobi and Gauss-Seidel
- **Positive definite**
	- All sub-diagonal matrix determinant are non-negative
	- Work for Jacobi and Gauss-Seidel
- **Spectral Radius**
	- $\rho(B) < 1$
	- $B$ is the one in matrix representitive. 
	- Work for Jacobi and Gauss-Seidel (Use their own $B$)
	- Sufficient, not necessary
