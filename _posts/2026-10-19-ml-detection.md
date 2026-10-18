---
title: Maximum Likelihood Detection
layout: default
ai_assist: true
---

# 1. System model

Multiple-input multiple-output (MIMO) system model:

$$
\mathbf{y} = \mathbf{H} \mathbf{x} + \mathbf{n}, \qquad
\mathbf{H} \in \mathbb{C}^{N_r \times N_t}, \quad
\mathbf{n} \sim \mathcal{CN}(\mathbf{0}, \sigma^2 \mathbf{I})
$$

where the symbol vector $\mathbf{x} = [x_0, \dots, x_{N_t-1}]^T$ has every component in the alphabet $\mathcal{A}$.
The ML detector solves

$$
\hat{\mathbf{x}} = \arg \min_{\mathbf{x} \in \mathcal{A}^{N_t}} \| \mathbf{y} - \mathbf{H} \mathbf{x} \|^2
$$

QR decomposition transfers the ML detector into a tree search problem.

$$
\mathbf{H} = \mathbf{Q} \mathbf{R}, \qquad
\mathbf{Q}^H \mathbf{Q} = \mathbf{I}_{N_t}, \qquad
\mathbf{R} \ \text{upper triangular}
$$

We define the transformed received vector by

$$
\mathbf{y}_r = \mathbf{Q}^H \mathbf{y}
$$

The final ML metric is

$$
\hat{\mathbf{x}} = \arg \min_{\mathbf{x} \in \mathcal{A}^{N_t}} \| \mathbf{y}_r - \mathbf{R} \mathbf{x} \|^2
$$

# 2. ML detection with $\mathbf{H}$

Since $\mathbf{R}$ is upper triangular, the $k$-th row of $\mathbf{R} \mathbf{x}$ involves only $x_k, \dots, x_{N_t-1}$.
Writing the Euclidean distance $D = \| \mathbf{y}_r - \mathbf{R} \mathbf{x} \|^2$ row by row gives

$$
D = \sum_{k=0}^{N_t-1} \Big\lvert y_{r,k} - \sum_{j=k}^{N_t-1} r_{kj} x_j \Big\rvert^2
$$

so each summand depends on the symbols from index $k$ upward.
This is what makes a sequential search possible: once $x_{N_t-1}$ is fixed, the last row is determined, and the search can descend one stream at a time.

Define the partial metric at depth $k$ by

$$
d_k(x_k, \dots, x_{N_t-1}) = \sum_{i=k}^{N_t-1} \Big\lvert y_{r,i} - \sum_{j=i}^{N_t-1} r_{ij} x_j \Big\rvert^2
$$

The partial metric is nested, since

$$
d_k(x_k, \dots, x_{N_t-1}) = d_{k+1}(x_{k+1}, \dots, x_{N_t-1}) + \Big\lvert y_{r,k} - \sum_{j=k}^{N_t-1} r_{kj} x_j \Big\rvert^2
$$

and because every term is nonnegative the partial metric never decreases as the search descends:

$$
d_k \ge d_{k+1}
$$

A candidate whose partial metric already exceeds the best complete metric found so far cannot become optimal by fixing further symbols, so its whole subtree may be pruned.
This monotonicity is what the tree search exploits to prune subtrees.

# 3. ML detection with $\bar{\mathbf{H}}$

The extended channel and received signal are

$$
\bar{\mathbf{H}} = \begin{bmatrix} \mathbf{H} \\ \sigma \mathbf{I}_{N_t} \end{bmatrix}, \qquad
\bar{\mathbf{y}} = \begin{bmatrix} \mathbf{y} \\ \mathbf{0} \end{bmatrix}
$$

The QR decomposition of $\bar{\mathbf{H}}$ is

$$
\bar{\mathbf{H}} = \bar{\mathbf{Q}} \bar{\mathbf{R}}, \qquad
\bar{\mathbf{Q}} = \begin{bmatrix} \mathbf{Q}_1 \\ \mathbf{Q}_2 \end{bmatrix}
$$

The triangular form relates to the ML metric by

$$
\| \mathbf{Q}_1^H \mathbf{y} - \bar{\mathbf{R}} \mathbf{x} \|^2
= \| \bar{\mathbf{y}} - \bar{\mathbf{H}} \mathbf{x} \|^2
= \| \mathbf{y} - \mathbf{H} \mathbf{x} \|^2 + \sigma^2 \| \mathbf{x} \|^2
$$

The ML metric can be obtained from

$$
\| \mathbf{y} - \mathbf{H} \mathbf{x} \|^2
= \| \mathbf{Q}_1^H \mathbf{y} - \bar{\mathbf{R}} \mathbf{x} \|^2 - \sigma^2 \| \mathbf{x} \|^2
$$

The relation between $\bar{\mathbf{R}}$ and $\mathbf{R}$ obeys

$$
\bar{\mathbf{R}}^H \bar{\mathbf{R}} = \mathbf{R}^H \mathbf{R} + \sigma^2 \mathbf{I}_{N_t}
$$

# 4. Complexity

Both approaches run the same triangular tree search, so the difference in complexity is the triangularization cost.

The reduced QR of the channel costs

$$
C_{\mathrm{QR}}(\mathbf{H}) = 2 N_r N_t^2 - \frac{2}{3} N_t^3
$$

while the QR of the $\bar{\mathbf{H}}$ costs

$$
C_{\mathrm{QR}}(\bar{\mathbf{H}}) = 2 N_r N_t^2 + \frac{4}{3} N_t^3
$$

