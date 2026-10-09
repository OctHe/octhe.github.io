---
title: Explicit MMSE with Adjugate Matrix
layout: default
ai_assist: true
---

> **Notation**
>
> | Symbol | Meaning |
> |:---:|:---|
> | $$\mathbf{H}$$, $$\mathbf{x}$$ | matrices and vectors are always in **bold** |
> | $$\mathbf{(\cdot)}^*$$ | adjugate matrix |
> | $$(\cdot)^H$$ | conjugate transpose matrix |
> | $$\det(\cdot)$$ | determinant |
> | $$\lvert \cdot \rvert$$ | modulus |

# 1. System model

Multiple-input multiple-output (MIMO) system model:

$$
\mathbf{y} = \mathbf{H} \mathbf{x} + \mathbf{n}, \qquad
\mathbf{H} \in \mathbb{C}^{N_r \times N_t}, \quad
\mathbf{n} \sim \mathcal{CN}(\mathbf{0}, \sigma^2 \mathbf{I})
$$

We define $$\mathbf{A} = \mathbf{H}^H \mathbf{H} + \sigma^2 \mathbf{I}$$.
In the $$2 \times 2$$, $$3 \times 3$$ and $$4 \times 4$$ cases below $$\mathbf{H}$$ is taken square, $$N_r = N_t = n$$, so that $$\mathbf{W}$$ is $$n \times n$$.
The MMSE solution is

$$
\hat{\mathbf{x}} = \mathbf{W} \mathbf{y}, \qquad
\mathbf{W} = (\mathbf{H}^H \mathbf{H} + \sigma^2 \mathbf{I})^{-1} \mathbf{H}^H = \mathbf{A}^{-1} \mathbf{H}^H
$$

The estimator here is explicit.
The rest of the post focuses on the derivation of $$\mathbf{A}^{-1}$$.

# 2. $$2 \times 2$$ MMSE

We define the matrix $$\mathbf{A}$$ in $$2 \times 2$$ dimension as

$$
\mathbf{A} = \begin{bmatrix} a_{00} & a_{01}\\ a_{01}^H & a_{11} \end{bmatrix}.
$$

The adjugate matrix of $$\mathbf{A}$$ under $$2 \times 2$$ dimension can be obtained by swapping the elements in $$\mathbf{A}$$.

$$
\mathbf{A}^* = \begin{bmatrix} a_{11} & -a_{01}\\ -a_{01}^H & a_{00} \end{bmatrix}, \qquad
$$

$$
\mathbf{A}^{-1} = \frac{\mathbf{A}^*}{\det(\mathbf{A})}
$$

where

$$
\det(\mathbf{A}) = a_{00}a_{11} - \lvert a_{01} \rvert^2.
$$

# 3. $$3 \times 3$$ MMSE

We define

$$
\mathbf{A} = \begin{bmatrix}
a_{00} & a_{01} & a_{02}\\
a_{01}^H & a_{11} & a_{12}\\
a_{02}^H & a_{12}^H & a_{22}
\end{bmatrix}.
$$

Due to the symmetry of $$\mathbf{A}$$, only the 6 upper-triangular cofactors are needed:

$$
\left\{
\begin{aligned}
M_{00}& = a_{11}a_{22} - | a_{12} |^2 \\
M_{11}& = a_{00}a_{22} - | a_{02} |^2 \\
M_{22}& = a_{00}a_{11} - | a_{01} |^2 \\
M_{10}& = a_{02}a_{12}^H - a_{01}a_{22} \\
M_{20}& = a_{01}a_{12} - a_{02}a_{11} \\
M_{21}& = a_{02}a_{01}^H - a_{00}a_{12}
\end{aligned}
\right.
$$

The adjugate matrix of a general $$ 3 \times 3 $$ matrix is

$$
\mathbf{A}^* = \begin{bmatrix}
M_{00} & M_{10} & M_{20}\\[2pt]
M_{01} & M_{11} & M_{21}\\[2pt]
M_{02} & M_{12} & M_{22}
\end{bmatrix}
=  \begin{bmatrix}
a_{11}a_{22} - | a_{12} |^2 & a_{02}a_{12}^H - a_{01}a_{22} & a_{01}a_{12} - a_{02}a_{11}\\[3pt]
a_{02}^H a_{12} - a_{01}^H a_{22} & a_{00}a_{22} - | a_{02} |^2 & a_{02}a_{01}^H - a_{00}a_{12}\\[3pt]
a_{01}^H a_{12}^H - a_{11}a_{02}^H & a_{01}a_{02}^H - a_{00}a_{12}^H & a_{00}a_{11} - | a_{01} |^2
\end{bmatrix}
$$

The determinant is

$$
\det(\mathbf{A}) = a_{00}a_{11}a_{22} - a_{00} | a_{12} |^2 - a_{11} | a_{02} |^2 - a_{22} | a_{01} |^2 + a_{01}a_{02}^H a_{12} + a_{01}^H a_{02}a_{12}^H.
$$

The inverse is the adjugate over the determinant:

$$
\mathbf{A}^{-1} = \frac{\mathbf{A}^*}{\det(\mathbf{A})}.
$$

# 4. $$4 \times 4$$ MMSE

If the inverse of a $4 \times 4$ matrix is calculated directly using the adjugate matrix, one must compute 16 third-order algebraic cofactors. 
Since each cofactor itself requires expansion into a $3 \times 3$ determinant, the computational cost is high.
Therefore, we employ a block-based approach, partitioning matrix $\mathbf{A}$ into $2 \times 2$ blocks:

$$
\mathbf{A} = \begin{bmatrix} \mathbf{A}_{00} & \mathbf{A}_{01}\\ \mathbf{A}_{01}^H & \mathbf{A}_{11} \end{bmatrix}, \qquad
\mathbf{A}_{00} = \begin{bmatrix} a_{00} & a_{01}\\ a_{01}^H & a_{11} \end{bmatrix}, \quad
\mathbf{A}_{11} = \begin{bmatrix} a_{22} & a_{23}\\ a_{23}^H & a_{33} \end{bmatrix}, \quad
\mathbf{A}_{01} = \begin{bmatrix} a_{02} & a_{03}\\ a_{12} & a_{13} \end{bmatrix}
$$

## 4.1 Method 1: Schur Complement with 2 Divisions

First, eliminate the second block row.
The Schur complement of the $$\mathbf{A}_{11}$$ block is denoted as $$\mathbf{S}$$:

$$
\label{eq:S}
\mathbf{S} 
= \mathbf{A}_{00} - \mathbf{A}_{01} \mathbf{A}_{11}^{-1} \mathbf{A}_{01}^H
= \mathbf{A}_{00} - \mathbf{C} \mathbf{A}_{01}^H
$$

where $$ \mathbf{C} = \mathbf{A}_{01} \mathbf{A}_{11}^{-1} $$ belongs to the common part of $$\mathbf{S}$$ and of the off-diagonal blocks, so the inverse is

$$
\mathbf{A}^{-1} = \begin{bmatrix} \mathbf{S}^{-1} & -\mathbf{S}^{-1} \mathbf{C}\\ -\mathbf{C}^H \mathbf{S}^{-1} & \mathbf{A}_{11}^{-1} + \mathbf{C}^H \mathbf{S}^{-1} \mathbf{C} \end{bmatrix}
$$

## 4.2 Method 2: Schur Complement with 1 Division

In the previous section, calculating $\mathbf{S}^{-1}$ and $\mathbf{A}_{11}^{-1}$ requires a total of two divisions.
The inverse of a $2 \times 2$ matrix can be written directly using the adjugate matrix.
Thus, 2 division operations can be reduced to one.

In equation ($\ref{eq:S}$), the definition of $$\mathbf{S}$$ involves $$\mathbf{A}_{11}^{-1}$$.
To move this inverse matrix to the denominator, one must first express $$\mathbf{S}$$ with a common denominator:

$$
\begin{aligned}
\mathbf{S}
&= \mathbf{A}_{00} - \mathbf{A}_{01} \mathbf{A}_{11}^{-1} \mathbf{A}_{01}^H\\
&= \mathbf{A}_{00} - \frac{\mathbf{A}_{01} \mathbf{A}_{11}^* \mathbf{A}_{01}^H}{\det(\mathbf{A}_{11})}\\
&= \frac{\det(\mathbf{A}_{11}) \mathbf{A}_{00} - \mathbf{A}_{01} \mathbf{A}_{11}^* \mathbf{A}_{01}^H}{\det(\mathbf{A}_{11})}\\
&= \frac{\mathbf{T}}{\det(\mathbf{A}_{11})}
\end{aligned}
$$

$$\mathbf{A}_{01} \mathbf{A}_{11}^*$$ is a common part, so we denote it by $$ \mathbf{C} = \mathbf{A}_{01} \mathbf{A}_{11}^* $$.
Then we define

$$
\mathbf{T} = \det(\mathbf{A}_{11}) \mathbf{A}_{00} - \mathbf{C} \mathbf{A}_{01}^H
$$

There is no division in the expression of $$\mathbf{T}$$.
The two denominators are folded into

$$
D = \det(\mathbf{A}_{11}) \det(\mathbf{T})
$$

so that the inverse needs only one reciprocal:

$$
\mathbf{A}^{-1} = \frac{1}{D}
\begin{bmatrix}
\det(\mathbf{A}_{11})^2 \mathbf{T}^* & -\det(\mathbf{A}_{11}) \mathbf{T}^* \mathbf{C}\\
-\det(\mathbf{A}_{11}) \mathbf{C}^H \mathbf{T}^* & \det(\mathbf{T}) \mathbf{A}_{11}^* + \mathbf{C}^H \mathbf{T}^* \mathbf{C}
\end{bmatrix}.
$$

# 5. Signal-to-Noise Ratio (SNR)

With $$\hat{\mathbf{x}} = \mathbf{W} \mathbf{y}$$ and $$\mathbf{W} = \mathbf{A}^{-1} \mathbf{H}^H$$, the error $$\mathbf{e} = \hat{\mathbf{x}} - \mathbf{x}$$ splits into a signal part and a noise part,

$$
\mathbf{e} = (\mathbf{W} \mathbf{H} - \mathbf{I}) \mathbf{x} + \mathbf{W} \mathbf{n}
$$

and the first factor collapses because $$\mathbf{A} = \mathbf{H}^H \mathbf{H} + \sigma^2 \mathbf{I}$$:

$$
\mathbf{W} \mathbf{H} = \mathbf{A}^{-1} \mathbf{H}^H \mathbf{H} = \mathbf{A}^{-1} (\mathbf{A} - \sigma^2 \mathbf{I}) = \mathbf{I} - \sigma^2 \mathbf{A}^{-1}
$$

so

$$
\mathbf{e} = -\sigma^2 \mathbf{A}^{-1} \mathbf{x} + \mathbf{A}^{-1} \mathbf{H}^H \mathbf{n}
$$

With unit-power symbols and spatially white noise, $$\mathbb{E}[\mathbf{x} \mathbf{x}^H] = \mathbf{I}$$ and $$\mathbb{E}[\mathbf{n} \mathbf{n}^H] = \sigma^2 \mathbf{I}$$, the error covariance is

$$
\mathbf{R}_e = \mathbb{E}[\mathbf{e} \mathbf{e}^H] = \sigma^4 \mathbf{A}^{-2} + \sigma^2 \mathbf{A}^{-1} \mathbf{H}^H \mathbf{H} \mathbf{A}^{-1} = \sigma^2 \mathbf{A}^{-1}
$$

because $$\sigma^2 \mathbf{A}^{-1} \mathbf{H}^H \mathbf{H} \mathbf{A}^{-1} = \sigma^2 \mathbf{A}^{-1} - \sigma^4 \mathbf{A}^{-2}$$.
The mean square error of stream $$i$$ is therefore $$\sigma^2 [\mathbf{A}^{-1}]_{ii}$$.
The signal-to-noise ratio of stream $$i$$ is

$$
\mathrm{SNR}_i 
= \frac{1 - \sigma^2 [\mathbf{A}^{-1}]_{ii}}{\sigma^2 [\mathbf{A}^{-1}]_{ii}}
= \frac{1}{\sigma^2 [\mathbf{A}^{-1}]_{ii}} - 1
$$

The inverse matrix is equal to the adjugate matrix divided by the determinant.
Therefore, its diagonal elements are derived from the algebraic cofactors constructed using the aforementioned method:

$$
\mathbf{A}^{-1} = \frac{\mathbf{A}^*}{\det(\mathbf{A})}, \qquad [\mathbf{A}^{-1}]_{ii} = \frac{M_{ii}}{\det(\mathbf{A})}
$$

The block decomposition method for a $$4 \times 4$$ matrix constructs a Schur complement, so cofactors do not appear, and the diagonal entries must be completed using block matrix inverses.

$$
\mathbf{A}^{-1} = \begin{bmatrix} \mathbf{S}^{-1} & \cdot \\ \cdot & \mathbf{A}_{11}^{-1} + \mathbf{A}_{11}^{-1} \mathbf{A}_{01}^H \mathbf{S}^{-1} \mathbf{A}_{01} \mathbf{A}_{11}^{-1} \end{bmatrix}
$$

