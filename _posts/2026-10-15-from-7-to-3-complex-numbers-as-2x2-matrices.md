---
title: 'From 7 to 3: Complex Numbers as 2x2 Matrices'
layout: default
ai_assist: true
---

> **Notation**
>
> | Symbol | Meaning |
> |:---:|:---|
> | $$\mathbf{A}, \mathbf{B}, \mathbf{C}$$ | matrices and vectors are in bold; $$\mathbf{C} = \mathbf{A}\mathbf{B}$$ |
> | $$a_{ij}, b_{ij}, c_{ij}$$ | matrix entries; scalars are in ordinary italics |
> | $$\mathbf{I}, \mathbf{J}$$ | the identity matrix and the rotation matrix that represents $$j$$ |
> | $$j$$ | the imaginary unit, $$j^2 = -1$$ |
> | $$x, y$$ and $$u, v$$ | real and imaginary parts of the two complex numbers |
> | $$m_k$$ | the $$k$$-th product in a multiplication trick |


Multiplying two $$2 \times 2$$ matrices takes 7 multiplications, not 8.
Multiplying two complex numbers takes 3 real multiplications, not 4.

Complex multiplication is a special case of matrix multiplication: a complex number is a special $$2 \times 2$$ matrix.

# 1. A Complex Number as a $$2 \times 2$$ Matrix

A complex number is written

$$
z = x + jy
$$

where $$j$$ is the imaginary unit, $$j^2 = -1$$.
The same number is represented by the $$2 \times 2$$ matrix

$$
z = x + jy \;\longleftrightarrow\;
\begin{bmatrix} x & -y \\ y & x \end{bmatrix}
$$

Splitting the matrix into its diagonal and off-diagonal parts:

$$
\begin{bmatrix} x & -y \\ y & x \end{bmatrix}
=
x \begin{bmatrix} 1 & 0 \\ 0 & 1 \end{bmatrix}
+
y \begin{bmatrix} 0 & -1 \\ 1 & 0 \end{bmatrix}
=
x \mathbf{I} + y \mathbf{J}
$$


The matrix $$\mathbf{J}$$ plays the role of the imaginary unit:

$$
\mathbf{J}^2 =
\begin{bmatrix} 0 & -1 \\ 1 & 0 \end{bmatrix}
\begin{bmatrix} 0 & -1 \\ 1 & 0 \end{bmatrix}
=
\begin{bmatrix} -1 & 0 \\ 0 & -1 \end{bmatrix}
=
-\mathbf{I}
$$

Therefore, matrix multiplication of these matrices is exactly complex multiplication.

# 2. 7 Multiplications

For two matrices

$$
\mathbf{A} = \begin{bmatrix} a_{00} & a_{01} \\ a_{10} & a_{11} \end{bmatrix}, \qquad
\mathbf{B} = \begin{bmatrix} b_{00} & b_{01} \\ b_{10} & b_{11} \end{bmatrix}
$$

the product $$\mathbf{C} = \mathbf{A}\mathbf{B}$$ needs only 7 multiplications:

$$
m_1 = a_{00} (b_{01} - b_{11})
$$

$$
m_2 = (a_{00} + a_{01}) b_{11}
$$

$$
m_3 = (a_{10} + a_{11}) b_{00}
$$

$$
m_4 = a_{11} (b_{10} - b_{00})
$$

$$
m_5 = (a_{00} + a_{11})(b_{00} + b_{11})
$$

$$
m_6 = (a_{01} - a_{11})(b_{10} + b_{11})
$$

$$
m_7 = (a_{00} - a_{10})(b_{00} + b_{01})
$$

The entries of the result are

$$
\begin{cases}
c_{00} = m_5 + m_4 - m_2 + m_6 \\[4pt]
c_{01} = m_1 + m_2 \\[4pt]
c_{10} = m_3 + m_4 \\[4pt]
c_{11} = m_5 + m_1 - m_3 - m_7
\end{cases}
$$

# 3. 3 Multiplications

This is a special case of a $$2 \times 2$$ matrix.
Three products are enough:

$$
m_1 = xu
$$

$$
m_2 = yv
$$

$$
m_3 = (x + y)(u + v)
$$

The result is

$$
(x + yj)(u + vj) = (m_1 - m_2) + (m_3 - m_1 - m_2)\, j
$$

because the cross term is

$$
xv + yu = (x + y)(u + v) - xu - yv = m_3 - m_1 - m_2
$$
