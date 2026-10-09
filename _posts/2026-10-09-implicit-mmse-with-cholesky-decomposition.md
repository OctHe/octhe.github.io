---
title: Implicit MMSE with Cholesky Decomposition
layout: default
ai_assist: true
---

> **Notation**
>
> | Symbol | Meaning |
> |:---:|:---|
> | $$\mathbf{H}$$, $$\mathbf{x}$$ | matrices and vectors are always in **bold** |
> | $$a_{ij}$$, $$l_{ij}$$ | scalars are in italic with subscripts |
> | $$d_i$$ | real positive pivot of the LDL decomposition |
> | $$(\cdot)^H$$ | conjugate transpose |
> | $$\det(\cdot)$$ | determinant |
> | $$\lvert \cdot \rvert$$ | modulus |
> | $$\operatorname{rsqrt}(\cdot)$$ | reciprocal square root, defined by $$\operatorname{rsqrt}(\cdot) = 1 / \sqrt{\cdot}$$, counted as one operation of its own |
> | $$\operatorname{diag}(\cdot)$$ | diagonal matrix built from the listed entries |
> | $$\mathbf{L}$$ | lower triangular factor; its diagonal is unit for the LDL decomposition |
> | $$\mathbf{D}$$ | real positive diagonal factor |
> | $$\hat{\mathbf{z}}$$ | forward substitution intermediate |

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
\hat{\mathbf{x}} = (\mathbf{H}^H \mathbf{H} + \sigma^2 \mathbf{I})^{-1} \mathbf{H}^H \mathbf{y} = \mathbf{A}^{-1} \mathbf{z},
$$

so the estimate is the solution of the linear equation

$$
\mathbf{A} \hat{\mathbf{x}} = \mathbf{z}.
$$

The estimation approach does not construct weight matrix $$\mathbf{W}$$ and is therefore referred to as implicit estimation.
The matrix $$\mathbf{A}$$ is Hermitian, and with $$\sigma^2 > 0$$ it is also positive definite even when $$\mathbf{H}$$ is rank-deficient.
This is the property both decompositions below rely on: it guarantees that the triangular factors exist.

# 2. Cholesky decomposition

A Hermitian positive definite matrix $$\mathbf{A}$$ can be written as

$$
\mathbf{A} = \mathbf{L} \mathbf{L}^H
$$

With the positive-definiteness condition of $$l_{jj} > 0$$, the decomposition is unique.

$$
\begin{cases}
\dfrac{1}{l_{00}} = \operatorname{rsqrt}(a_{00}), & j = 0 \\[10pt]
\dfrac{1}{l_{jj}} = \operatorname{rsqrt}\biggl( a_{jj} - \sum_{k=0}^{j-1} \lvert l_{jk} \rvert^2 \biggr), & j \ge 1 \\[10pt]
l_{ij} = \dfrac{1}{l_{jj}} \biggl( a_{ij} - \sum_{k=0}^{j-1} l_{ik} l_{jk}^H \biggr), & i = j + 1, \dots, n - 1
\end{cases}
$$

This algorithm does not explicitly calculate the diagonal elements $$l_{jj}$$, thereby avoiding square root or division operations: processing each column requires only a single "reciprocal square root" operation, rather than first computing a square root followed by $$n - 1 - j$$ divisions.
Since the reciprocal square root can be obtained directly using the "fast inverse square root" algorithm, it is treated as a single operation rather than a combination of square root and division.

Substituting $$\mathbf{A} = \mathbf{L} \mathbf{L}^H$$ into $$\mathbf{A} \hat{\mathbf{x}} = \mathbf{z}$$ gives two triangular equations:

$$
\begin{cases}
\mathbf{L} \hat{\mathbf{z}} = \mathbf{z}\\
\mathbf{L}^H \hat{\mathbf{x}} = \hat{\mathbf{z}}
\end{cases}
$$

Forward substitution solves the first one:

$$
\begin{cases}
\hat{z}_0 = \dfrac{z_0}{l_{00}}, & i = 0 \\[10pt]
\hat{z}_i = \dfrac{1}{l_{ii}} \biggl( z_i - \sum_{k=0}^{i-1} l_{ik} \hat{z}_k \biggr), & i = 1, \dots, n - 1
\end{cases}
$$

Backward substitution solves the second one:

$$
\begin{cases}
\hat{x}_{n-1} = \dfrac{\hat{z}_{n-1}}{l_{n-1,n-1}}, & i = n - 1 \\[10pt]
\hat{x}_i = \dfrac{1}{l_{ii}} \biggl( \hat{z}_i - \sum_{k=i+1}^{n-1} l_{ki}^H \hat{x}_k \biggr), & i = n - 2, \dots, 0
\end{cases}
$$

# 3. LDL decomposition

The same matrix can be written with a unit triangular factor and a separate diagonal:

$$
\mathbf{A} = \mathbf{L} \mathbf{D} \mathbf{L}^H
$$

where

$$
\begin{cases}
l_{jj} = 1 \\[10pt]
l_{ij} = 0 \quad \text{for} \quad i < j \\[10pt]
\mathbf{D} = \operatorname{diag}(d_0, \dots, d_{n-1}), \qquad d_j > 0
\end{cases}
$$

This is a Cholesky decomposition in which the square-root terms have been factored out of the triangular factors.

$$
\begin{cases}
d_0 = a_{00}, & j = 0 \\[10pt]
d_j = a_{jj} - \sum_{k=0}^{j-1} \lvert l_{jk} \rvert^2 d_k, & j \ge 1 \\[10pt]
l_{ij} = \dfrac{1}{d_j} \biggl( a_{ij} - \sum_{k=0}^{j-1} l_{ik} l_{jk}^H d_k \biggr), & i = j + 1, \dots, n - 1
\end{cases}
$$

There is no square root in the algorithm.
Compared with Cholesky, every inner product now also carries a real multiplication by $$d_k$$, which is where the additional arithmetic goes.

With $$\mathbf{A} = \mathbf{L} \mathbf{D} \mathbf{L}^H$$ the equation splits into

$$
\begin{cases}
\mathbf{L} \hat{\mathbf{z}} = \mathbf{z}\\
\mathbf{L}^H \hat{\mathbf{x}} = \mathbf{D}^{-1} \hat{\mathbf{z}}
\end{cases}
$$

Forward substitution runs with a unit diagonal, so it does not need division:

$$
\begin{cases}
\hat{z}_0 = z_0, & i = 0 \\[10pt]
\hat{z}_i = z_i - \sum_{k=0}^{i-1} l_{ik} \hat{z}_k, & i = 1, \dots, n - 1
\end{cases}
$$

Backward substitution folds the diagonal scaling:

$$
\begin{cases}
\hat{x}_{n-1} = \dfrac{\hat{z}_{n-1}}{d_{n-1}}, & i = n - 1 \\[10pt]
\hat{x}_i = \dfrac{\hat{z}_i}{d_i} - \sum_{k=i+1}^{n-1} l_{ki}^H \hat{x}_k, & i = n - 2, \dots, 0
\end{cases}
$$

Only $$n$$ reciprocals $$1 / d_i$$ are required.

# 4. Complexity analysis

## 4.1 Cholesky vs LDL

| Cholesky | LDL |
|:---|:---|
| $$\frac{1}{l_{jj}} = \operatorname{rsqrt}( a_{jj} - \sum_{k=0}^{j-1} \lvert l_{jk} \rvert^2 )$$ | $$\frac{1}{d_j} = ( a_{jj} - \sum_{k=0}^{j-1} \lvert l_{jk} \rvert^2 d_k )^{-1}$$ |
| $$l_{ij} = \frac{1}{l_{jj}} ( a_{ij} - \sum_{k=0}^{j-1} l_{ik} l_{jk}^H )$$ | $$l_{ij} = \frac{1}{d_j} ( a_{ij} - \sum_{k=0}^{j-1} l_{ik} l_{jk}^H d_k )$$ |
| $$\hat{z}_i = \frac{1}{l_{ii}} ( z_i - \sum_{k=0}^{i-1} l_{ik} \hat{z}_k )$$ | $$\hat{z}_i = z_i - \sum_{k=0}^{i-1} l_{ik} \hat{z}_k$$ |
| $$\hat{x}_i = \frac{1}{l_{ii}} ( \hat{z}_i - \sum_{k=i+1}^{n-1} l_{ki}^H \hat{x}_k )$$ | $$\hat{x}_i = \frac{\hat{z}_i}{d_i} - \sum_{k=i+1}^{n-1} l_{ki}^H \hat{x}_k$$ |

Step by step, Cholesky differs from LDL as follows:

1. The diagonal pivot operation reduces the number of real multiplications by $$\frac{n(n-1)}{2}$$ and divisions by $$n$$, at the cost of adding $$n$$ reciprocal square root operations.
2. The off-diagonal column update operation reduces the number of real-complex multiplications by $$\frac{n(n-1)(n-2)}{6}$$, because the inner product terms do not contain the factor $$d_k$$.
3. The forward substitution operation adds $$n$$ real-complex multiplications, corresponding to a diagonal scaling for each component.
4. The computational cost of the backward substitution operation is the same.

