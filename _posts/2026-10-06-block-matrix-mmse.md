---
title: Matrix Inversion-based MMSE
layout: default
ai_assist: true
---

This post derives adjugate-based expression for MMSE estimation.

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

We define $$\mathbf{A} = \mathbf{H}^H \mathbf{H} + \sigma^2 \mathbf{I}$$ and $$\mathbf{z} = \mathbf{H}^H \mathbf{y}$$.
The MMSE solution is

$$
\hat{\mathbf{x}} = (\mathbf{H}^H \mathbf{H} + \sigma^2 \mathbf{I})^{-1} \mathbf{H}^H \mathbf{y} = \mathbf{A}^{-1} \mathbf{z}
$$

Note that the minimum mean square error (MMSE) estimate is equivalent to solving the linear equation: $$ \mathbf{A} \hat{\mathbf{x}} = \mathbf{z} $$.

# 2. $$2 \times 2$$ MMSE

We define the matrix $$\mathbf{A}$$ in $$2 \times 2$$ dimention as

$$
\mathbf{A} = \begin{bmatrix} a_{00} & a_{01}\\ a_{01}^H & a_{11} \end{bmatrix}.
$$

The adjugate matrix of $$\mathbf{A}$$ under $$2 \times 2$$ dimension can be obtained by swapping the the elements in $$\mathbf{A}$$.

$$
\mathbf{A}^* = \begin{bmatrix} a_{11} & -a_{01}\\ -a_{01}^H & a_{00} \end{bmatrix}, \qquad
$$

$$
\begin{cases}
\hat{x}_0 = \dfrac{a_{11}z_0 - a_{01}z_1}{\det(\mathbf{A})}\\[10pt]
\hat{x}_1 = \dfrac{a_{00}z_1 - a_{01}^H z_0}{\det(\mathbf{A})}
\end{cases}
$$

where 
$$
\det(\mathbf{A}) = a_{00}a_{11} - \lvert a_{01} \rvert^2
$$
.

# 3. $$3 \times 3$$ MMSE

## 3.1 Method 1: Adjugate Matrix


We define

$$
\mathbf{A} = \begin{bmatrix}
a_{00} & a_{01} & a_{02}\\
a_{01}^H & a_{11} & a_{12}\\
a_{02}^H & a_{12}^H & a_{22}
\end{bmatrix}.
$$

Since the symmetry of $$\mathbf{A}$$, only the 6 upper-triangular cofactors are needed:

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

The MMSE result is

$$
\hat{\mathbf{x}} = \frac{\mathbf{A}^* \mathbf{z}}{\det(\mathbf{A})},
$$

where

$$
\mathbf{z} = [z_0, \ z_1, \ z_2]^T.
$$

## 3.2 Method 2: Expand $$2 \times 2$$

We split the $$ 3 \times 3 $$ $$\mathbf{A}$$ into a $$2 \times 2$$ block and a $$1 \times 1$$ block.
The first two components are denoted $$\hat{\mathbf{x}}_0$$ (a 2-vector), and the third component $$\hat{x}_2$$ degenerates to a scalar; $$\mathbf{z}$$ is split the same way, its first two components being $$\mathbf{z}_0 = [z_0, \ z_1]^T$$.

$$
\mathbf{A} = \begin{bmatrix}
\mathbf{A}_{00} & \mathbf{b}\\
\mathbf{b}^H & a_{22}
\end{bmatrix}, \qquad
\mathbf{A}_{00} = \begin{bmatrix}
a_{00} & a_{01}\\
a_{01}^H & a_{11}
\end{bmatrix}, \qquad
\mathbf{b} = \begin{bmatrix}
a_{02}\\
a_{12}
\end{bmatrix}
$$

We first eliminate $$\hat{x}_2$$ and denote the resulting matrix as 

$$
\mathbf{S} 
= a_{22} \mathbf{A}_{00} - \mathbf{b} \mathbf{b}^H 
= \begin{bmatrix} M_{11} & -M_{10}\\ -M_{01} & M_{00} \end{bmatrix}
$$

We then define a common vector:

$$
\mathbf{p} 
= \mathbf{S}^* (a_{22} \mathbf{z}_0 - \mathbf{b} z_2)
$$

Finally, the result is

$$
\begin{cases}
\hat{\mathbf{x}}_0 
= \dfrac{ \mathbf{p} }{ \det(\mathbf{S}) }
= \dfrac{a_{22}}{D} \mathbf{p} \\
\hat{x}_2 = \dfrac{\det(\mathbf{S}) z_2 - \mathbf{b}^H \mathbf{p}}{D}
\end{cases}
$$

where

$$
D = a_{22} \det(\mathbf{S})
$$

We can compute $$\hat{\mathbf{x}}_0$$ first and then substitute it back into $$\hat{x}_2$$ as

$$
\hat{x}_2 = \frac{\det(\mathbf{S})}{D} (z_2 - \mathbf{b}^H \hat{\mathbf{x}}_0).
$$

## 3.3 Complexity analysis

| Method 1 | Method 2 | Method 2 vs Method 1 |
|:---|:---|:---|
| $$\mathbf{A}^*$$ | $$\mathbf{S} = \begin{bmatrix} M_{11} & -M_{10}\\ -M_{01} & M_{00} \end{bmatrix}$$ |  3 fewer cofactors |
| $$\det(\mathbf{A}) = a_{00}M_{00} + a_{01}M_{01} + a_{02}M_{02}$$ | $$\det(\mathbf{S})$$ | 1 fewer complex-complex multiplication |
| - | $$D = a_{22} \det(\mathbf{S})$$ | 1 more real-complex multiplication |
| - | $$\mathbf{p} = \mathbf{S}^* (a_{22} \mathbf{z}_0 - \mathbf{b} z_2)$$ | 8 more complex-complex multiplications |
| $$\hat{\mathbf{x}}_0 = \dfrac{1}{\det(\mathbf{A})} \begin{bmatrix} M_{00}z_0 + M_{10}z_1 + M_{20}z_2\\ M_{01}z_0 + M_{11}z_1 + M_{21}z_2 \end{bmatrix}$$ | $$\hat{\mathbf{x}}_0 = \dfrac{a_{22}}{D} \mathbf{p}$$ | 3 fewer complex-complex multiplications |
| $$\hat{x}_2 = \dfrac{M_{02}z_0 + M_{12}z_1 + M_{22}z_2}{\det(\mathbf{A})}$$ | $$\hat{x}_2 = \dfrac{\det(\mathbf{S}) z_2 - \mathbf{b}^H \mathbf{p}}{D}$$ | Same |

Method 2 costs slightly less: the $$\mathbf{S}$$ are exactly 3 of the 6 upper-triangular cofactors of $$\mathbf{A}^*$$.
$$\hat{x}_2$$ is supplied by a one-dimensional back-substitution, and the determinant needed is only the $$2 \times 2$$ $$\det(\mathbf{S})$$.

# 4. $$4 \times 4$$ MMSE

## 4.1 Method 1: Block-based Elimination

If the inverse of a $4 \times 4$ matrix is calculated directly using the adjugate matrix, one must compute 16 third-order algebraic cofactors. 
Since each cofactor itself requires expansion into a $3 \times 3$ determinant, the computational cost is high.
Therefore, we employ a block-based approach, partitioning matrix $\mathbf{A}$ into $2 \times 2$ blocks:

$$
\mathbf{A} = \begin{bmatrix} \mathbf{A}_{00} & \mathbf{A}_{01}\\ \mathbf{A}_{01}^H & \mathbf{A}_{11} \end{bmatrix}, \qquad
\mathbf{A}_{00} = \begin{bmatrix} a_{00} & a_{01}\\ a_{01}^H & a_{11} \end{bmatrix}, \quad
\mathbf{A}_{11} = \begin{bmatrix} a_{22} & a_{23}\\ a_{23}^H & a_{33} \end{bmatrix}, \quad
\mathbf{A}_{01} = \begin{bmatrix} a_{02} & a_{03}\\ a_{12} & a_{13} \end{bmatrix}
$$

Next, partition the unknowns in the same way: split $$\hat{\mathbf{x}}$$ into two 2-vectors $$\hat{\mathbf{x}}_0, \hat{\mathbf{x}}_1$$, and $$\mathbf{z}$$ into $$\mathbf{z}_0 = [z_0, z_1]^T$$ and $$\mathbf{z}_1 = [z_2, z_3]^T$$.
The system becomes

$$
\begin{cases}
\mathbf{A}_{00} \hat{\mathbf{x}}_0 + \mathbf{A}_{01} \hat{\mathbf{x}}_1 = \mathbf{z}_0\\[4pt]
\mathbf{A}_{01}^H \hat{\mathbf{x}}_0 + \mathbf{A}_{11} \hat{\mathbf{x}}_1 = \mathbf{z}_1
\end{cases}
$$


$$
\begin{cases}
\hat{\mathbf{x}}_0 = (\mathbf{A}_{00} - \mathbf{B}_0 \mathbf{A}_{01}^H)^{-1}
 (\mathbf{z}_0 - \mathbf{B}_0 \mathbf{z}_1)\\[8pt]
\hat{\mathbf{x}}_1 = (\mathbf{A}_{11} - \mathbf{B}_1 \mathbf{A}_{01})^{-1}
 (\mathbf{z}_1 - \mathbf{B}_1 \mathbf{z}_0)
\end{cases}
$$

where 

$$
\begin{cases}
\mathbf{B}_0 = \mathbf{A}_{01} \mathbf{A}_{11}^{-1}\\[4pt]
\mathbf{B}_1 = \mathbf{A}_{01}^H \mathbf{A}_{00}^{-1}
\end{cases}
$$

The direct method needs 4 divisions.
We further define common variables as:

$$
\begin{cases}
\mathbf{C}_0 = \mathbf{A}_{01} \mathbf{A}_{11}^*\\[4pt]
\mathbf{C}_1 = \mathbf{A}_{01}^H \mathbf{A}_{00}^*
\end{cases}
$$

The number of division operations can be reduced by finding a common denominator, as follows:

$$
\begin{cases}
\hat{\mathbf{x}}_0 = (\det(\mathbf{A}_{11}) \mathbf{A}_{00} - \mathbf{C}_0 \mathbf{A}_{01}^H)^{-1}
 (\det(\mathbf{A}_{11}) \mathbf{z}_0 - \mathbf{C}_0 \mathbf{z}_1)\\[8pt]
\hat{\mathbf{x}}_1 = (\det(\mathbf{A}_{00}) \mathbf{A}_{11} - \mathbf{C}_1 \mathbf{A}_{01})^{-1}
 (\det(\mathbf{A}_{00}) \mathbf{z}_1 - \mathbf{C}_1 \mathbf{z}_0)
\end{cases}
$$

This method adds 8 multiplications by $$\det(\mathbf{A}_{ii})$$ and requires only 2 divisions.

This form exhibits symmetry.
However, since calculating the two components involves different matrices, the same set of intermediate variables cannot be shared during the computation.
To address this, we expand the two equations into a form that allows for the sharing of the same set of intermediate variables.

## 4.2 Method 2: Asymmetric Method with 2 Divisions

First, substitute $$\hat{\mathbf{x}}_1$$ into the equation to eliminate it.
The $$2 \times 2$$ matrix corresponding to the remaining variable $$\hat{\mathbf{x}}_0$$ is denoted as $$\mathbf{S}$$:

$$
\label{eq:S}
\mathbf{S} = \mathbf{A}_{00} - \mathbf{A}_{01} \mathbf{A}_{11}^{-1} \mathbf{A}_{01}^H
$$

We next define the common vector by $$\mathbf{p}$$:

$$
\mathbf{p} = \mathbf{S}^* (\mathbf{z}_0 - \mathbf{C} \mathbf{z}_1)
$$

$$ \mathbf{C} = \mathbf{A}_{01} \mathbf{A}_{11}^{-1} $$ also belongs to the common part of $$\mathbf{S}$$ and $$\mathbf{p}$$.
So the final result is

$$
\begin{cases}
\hat{\mathbf{x}}_0 = \dfrac{\mathbf{p}}{\det(\mathbf{S})}\\[10pt]
\hat{\mathbf{x}}_1 = \mathbf{A}_{11}^{-1} \biggl(\mathbf{z}_1 - \mathbf{A}_{01}^H \dfrac{\mathbf{p}}{\det(\mathbf{S})} \biggr)
\end{cases}
$$

The second equation can also be written by calculating $$\hat{\mathbf{x}}_0$$ and performing back-substituting:

$$
\hat{\mathbf{x}}_1 = \mathbf{A}_{11}^{-1} (\mathbf{z}_1 - \mathbf{A}_{01}^H \hat{\mathbf{x}}_0)
$$

This back-substitution form requires less computation,, but it first necessitates $$\hat{\mathbf{x}}_0$$, which imposes certain timing requirements.

## 4.3 Method 3: Asymmetric Method with 1 Division

In the previous section, calculating $\mathbf{S}^{-1}$ and $\mathbf{A}_{11}^{-1}$ requires a total of two divisions.
The inverse of a $2 \times 2$ matrix can be written directly using the adjugate matrix.
Thus, 2 division operations can be reduced into one.

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
We can get

$$
\mathbf{S}^{-1}
= \mathbf{T}^{-1} \det(\mathbf{A}_{11})
= \frac{\mathbf{T}^{*} \det(\mathbf{A}_{11})}{\det(\mathbf{T})}
$$

We replace $$\mathbf{p}$$ with an expression that does not contain the inverse operation:

$$
\mathbf{p} = \mathbf{T}^* (\det(\mathbf{A}_{11}) \mathbf{z}_0 - \mathbf{C} \mathbf{z}_1)
$$

The two denominators have also been combined into a single common denominator, so only one reciprocal is needed:

$$
D = \det(\mathbf{A}_{11}) \det(\mathbf{T})
$$

The result is

$$
\begin{cases}
\hat{\mathbf{x}}_0 
= \dfrac{\mathbf{p}}{\det(\mathbf{T})}
= \dfrac{\det(\mathbf{A}_{11})}{D} \mathbf{p}
\\
\hat{\mathbf{x}}_1 
= \dfrac{\mathbf{A}_{11}^*}{D} (\det(\mathbf{T}) \mathbf{z}_1 - \mathbf{A}_{01}^H \mathbf{p} )
\end{cases}
$$

Equivalently, we can substitute $$\hat{\mathbf{x}}_0$$ back into $$\hat{\mathbf{x}}_1$$.

$$
\hat{\mathbf{x}}_1 = \frac{\mathbf{A}_{11}^* \det(\mathbf{T})}{D} (\mathbf{z}_1 - \mathbf{A}_{01}^H \hat{\mathbf{x}}_0)
$$

However, in this case, the number of operations is not reduced.

## 4.4 Complexity analysis

| Method 1d | Method 3 | Method 3 vs Method 1 |
|:---|:---|:---|
| $$\mathbf{C}_0 = \mathbf{A}_{01} \mathbf{A}_{11}^*$$ <br> $$\mathbf{C}_1 = \mathbf{A}_{01}^H \mathbf{A}_{00}^*$$ | $$\mathbf{C} = \mathbf{A}_{01} \mathbf{A}_{11}^*$$ | Skip $$\mathbf{C}_{1}$$ |
| $$\mathbf{T}_0 = \det(\mathbf{A}_{11}) \mathbf{A}_{00} - \mathbf{C}_0 \mathbf{A}_{01}^H$$ $$\mathbf{T}_1 = \det(\mathbf{A}_{00}) \mathbf{A}_{11} - \mathbf{C}_1 \mathbf{A}_{01}$$ | $$\mathbf{T} = \det(\mathbf{A}_{11}) \mathbf{A}_{00} - \mathbf{C} \mathbf{A}_{01}^H$$ | Skip $$\mathbf{T}_{1}$$ |
| $$\mathbf{p}_0 = \mathbf{T}_0^* (\det(\mathbf{A}_{11}) \mathbf{z}_0 - \mathbf{C}_0 \mathbf{z}_1)$$, $$\mathbf{p}_1 = \mathbf{T}_1^* (\det(\mathbf{A}_{00}) \mathbf{z}_1 - \mathbf{C}_1 \mathbf{z}_0)$$ | $$\mathbf{p} = \mathbf{T}^* (\det(\mathbf{A}_{11}) \mathbf{z}_0 - \mathbf{C} \mathbf{z}_1)$$ | Skip $$\mathbf{p}_{1}$$ |
| - | $$D = \det(\mathbf{A}_{11}) \det(\mathbf{T})$$ | 1 more multiplication |
| $$1 / \det(\mathbf{T}_0)$$ <br> $$1 / \det(\mathbf{T}_1)$$ | $$1 / D$$ | 1 fewer division |
| $$\hat{\mathbf{x}}_0 = \mathbf{p}_0 / \det(\mathbf{T}_0)$$ | $$\hat{\mathbf{x}}_0 = \det(\mathbf{A}_{11}) \mathbf{p} / D$$ | 1 more multiplication |
| $$\hat{\mathbf{x}}_1 = \mathbf{p}_1 / \det(\mathbf{T}_1)$$ | $$\hat{\mathbf{x}}_1 = \mathbf{A}_{11}^* (\det(\mathbf{T}) \mathbf{z}_1 - \mathbf{A}_{01}^H \mathbf{p}) / D$$ | 8 more multiplications |

| Method 2 | Method 3 | Method 3 vs Method 2 |
|:---|:---|:---|
| $$1 / \det(\mathbf{A}_{11})$$ | - | Skip computing $$1 / \det(\mathbf{A}_{11})$$ |
| $$\mathbf{C} = \mathbf{A}_{01} \mathbf{A}_{11}^{-1}$$ | $$\mathbf{C} = \mathbf{A}_{01} \mathbf{A}_{11}^*$$ | Skip multiplying by $$1 / \det(\mathbf{A}_{11})$$ |
| $$\mathbf{S} = \mathbf{A}_{00} - \mathbf{C} \mathbf{A}_{01}^H$$ | $$\mathbf{T} = \det(\mathbf{A}_{11}) \mathbf{A}_{00} - \mathbf{C} \mathbf{A}_{01}^H$$ | 1 more multiplication by $$\det(\mathbf{A}_{11})$$ |
| $$\mathbf{p} = \mathbf{S}^* (\mathbf{z}_0 - \mathbf{C} \mathbf{z}_1)$$ | $$\mathbf{p} = \mathbf{T}^* (\det(\mathbf{A}_{11}) \mathbf{z}_0 - \mathbf{C} \mathbf{z}_1)$$ | 1 more multiplication by $$\det(\mathbf{A}_{11})$$ |
| - | $$D = \det(\mathbf{A}_{11}) \det(\mathbf{T})$$ | 1 more multiplication |
| $$1 / \det(\mathbf{S})$$ | $$1 / D$$ | same |
| $$\hat{\mathbf{x}}_0 = \mathbf{p} / \det(\mathbf{S})$$ | $$\hat{\mathbf{x}}_0 = \det(\mathbf{A}_{11}) \mathbf{p} / D$$ | 1 more multiplication by $$\det(\mathbf{A}_{11})$$ |
| $$\hat{\mathbf{x}}_1 = \mathbf{A}_{11}^{-1} (\mathbf{z}_1 - \mathbf{A}_{01}^H \mathbf{p} / \det(\mathbf{S}))$$ | $$\hat{\mathbf{x}}_1 = \mathbf{A}_{11}^* (\det(\mathbf{T}) \mathbf{z}_1 - \mathbf{A}_{01}^H \mathbf{p}) / D$$ | 1 more multiplication by $$\det(\mathbf{T})$$ |

Compare to method 2, method 3 reduces the number of division operations, at the cost of several additional multiplications.
