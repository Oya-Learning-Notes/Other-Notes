# Min Square Error

Basic formula is:

$$
A^T \cdot A \cdot a = A \cdot y
$$

# Min Square Error For Continuous Function

Look back to the basic matrix:

$$
\begin{bmatrix} (
\varphi_0, \varphi_0) & (\varphi_0, \varphi_1) & (\varphi_0, \varphi_2) \\
(\varphi_1, \varphi_0) & (\varphi_1, \varphi_1) & (\varphi_1, \varphi_2) \\
(\varphi_2, \varphi_0) & (\varphi_2, \varphi_1) & (\varphi_2, \varphi_2) \end{bmatrix}
$$

In discrete version, we have:

$$
(\varphi_i, \varphi_j) = \sum_{p=1}^{n}\varphi_i(x_p)\cdot\varphi_j(x_p)
$$

In continuous version, we just change this into integration:

$$
(\varphi_i, \varphi_j) = \int_{low}^{high} \varphi_i(x) \varphi_j(x) dx
$$

Notice, the final calculated matrix $A$ must be symmetric, that means we only need to calcualate half of the elements. Like $(\varphi_0, \varphi_2) = (\varphi_2, \varphi_0)$.

## Examples

Let's see some example:

![Question Example 1](https://github.com/user-attachments/assets/7ba8e852-4852-4cb4-b269-6c4cd07c0b3b)

![Calc Elements](https://github.com/user-attachments/assets/fab78ac3-2e5d-475d-973b-eaae0732d40a)

![Answer](https://github.com/user-attachments/assets/7b4878cf-b654-495b-adaf-23e7dee817cd)
