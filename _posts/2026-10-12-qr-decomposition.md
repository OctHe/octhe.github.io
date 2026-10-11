---
title: "QR Decomposition: Gram–Schmidt, Householder, and Givens"
layout: default
ai_assist: true
---

This post discusses the QR decomposition of a full-column-rank $$m\times n$$ ($$m\ge n$$) matrix.

> **Notation**
>
> | Symbol | Meaning |
> |:---:|:---|
> | $$\mathbf{H}\in\mathbb{C}^{m\times n}$$ | full-column-rank matrix ($$m\ge n$$), with entries $$h_{ij}$$ |
> | $$(\cdot)^H$$ | conjugate transpose; on a scalar, conjugation |
> | $$\langle\mathbf{f},\mathbf{g}\rangle=\mathbf{f}^H\mathbf{g}$$ | inner product |
> | $$\|\cdot\|_2$$ | Euclidean (spectral) norm; the 2-norm of a vector is written $$\|\cdot\|$$ |
> | $$\operatorname{span}\{\cdot\}$$ | linear span |

# 1. QR Decomposition

Given $$\mathbf{H}\in\mathbb{C}^{m\times n}$$, its QR decomposition is

$$
\mathbf{H}=\mathbf{Q}\mathbf{R},\qquad
\mathbf{Q}\in\mathbb{C}^{m\times m}\ \text{unitary (orthogonal)},\quad
\mathbf{R}\in\mathbb{C}^{m\times n}\ \text{upper triangular}
$$

If only the first $$n$$ columns are required, the decomposition is written in thin/reduced form.

$$
\mathbf{H}=\mathbf{Q}_1\mathbf{R}_1,\qquad
\mathbf{Q}_1\in\mathbb{C}^{m\times n}\ (\mathbf{Q}_1^H\mathbf{Q}_1=\mathbf{I}_n),\quad
\mathbf{R}_1\in\mathbb{C}^{n\times n}\ \text{upper triangular}
$$

with $$\mathbf{Q}=[\mathbf{Q}_1\ \ \mathbf{Q}_2]$$ and $$\mathbf{R}=\begin{bmatrix}\mathbf{R}_1\\ \mathbf{0}\end{bmatrix}$$.
$$\mathbf{Q}_2\in\mathbb{C}^{m\times(m-n)}$$ is an orthonormal basis for the orthogonal complement of the column space.
Therefore, $$\mathbf{Q}$$ shrinks in width from $$m\times m$$ to $$m\times n$$, while $$\mathbf{R}$$ shrinks from $$m\times n$$ (the lower blocks are exactly zero) to $$n\times n$$.
When $$m\gg n$$, significant storage savings can be achieved.


The QR decomposition has the following two properties:
1. **Existence**.
Under the full column rank assumption, QR decomposition always exists.
2. **Uniqueness**.
The reduced QR decomposition is unique if $$\operatorname{diag}(\mathbf{R}_1)$$ has positive real diagonal entries.
The reason is that $$\mathbf{R}_1$$ is exactly the Cholesky decomposition of the $$\mathbf{H}^H\mathbf{H}$$ matrix (i.e., $$\mathbf{H}^H\mathbf{H}=\mathbf{R}_1^H\mathbf{R}_1$$), and the Cholesky factor of a positive definite matrix is unique when its diagonal entries are positive real numbers.

The goal of the QR decomposition is to find a sequence of unitary matrices that zero out specified entries of the original matrix ($$r_{ij} = 0$$ for $$i > j$$).

There are three common approaches: Gram–Schmidt, Givens, and Householder.
Typically, the diagonal entries produced by Givens and Householder may not be real numbers (Gram–Schmidt's $$r_{kk}$$ is positive real by construction).
In order to get the unique $$\mathbf{R}_1$$, we need to change the diagonal elements of $$\mathbf{R}_1$$ into real numbers.
We define

$$
d_i = \frac{r_{ii}}{\| r_{ii} \|}, \qquad \mathbf{D} = \operatorname{diag}(d_0, \dots, d_{n-1})
$$

and then we obtain

$$
\mathbf{R}_1 \leftarrow \mathbf{D}^H\mathbf{R}_1, \qquad \mathbf{Q}_1 \leftarrow \mathbf{Q}_1\mathbf{D}
$$

The new diagonal entries are the positive reals $$\lvert r_{ii}\rvert$$; $$\mathbf{Q}_1\mathbf{D}$$ still has orthonormal columns, $$(\mathbf{Q}_1\mathbf{D})(\mathbf{D}^H\mathbf{R}_1) = \mathbf{Q}_1\mathbf{R}_1$$ is unchanged.

This post considers only the full-column-rank case, that is

$$
\mathbf{H}\in\mathbb{C}^{m\times n},\qquad m\ge n,\qquad \operatorname{rank}(\mathbf{H})=n
$$

The general cases of rank deficiency and of wide matrices ($$m < n$$) are not treated here.


# 2. Gram–Schmidt Orthogonalization

Construct orthogonal vectors $$\hat{\mathbf{q}}_0,\dots,\hat{\mathbf{q}}_{n-1}$$ in sequence:

$$
\begin{cases}
\ \hat{\mathbf{q}}_0&=\mathbf{h}_0\\
\ \hat{\mathbf{q}}_1&=\mathbf{h}_1-\frac{\hat{\mathbf{q}}_0^H\mathbf{h}_1}{\hat{\mathbf{q}}_0^H\hat{\mathbf{q}}_0}\hat{\mathbf{q}}_0\\
\ \hat{\mathbf{q}}_2&=\mathbf{h}_2-\frac{\hat{\mathbf{q}}_0^H\mathbf{h}_2}{\hat{\mathbf{q}}_0^H\hat{\mathbf{q}}_0}\hat{\mathbf{q}}_0-\frac{\hat{\mathbf{q}}_1^H\mathbf{h}_2}{\hat{\mathbf{q}}_1^H\hat{\mathbf{q}}_1}\hat{\mathbf{q}}_1\\
&\ \ \vdots\\
\ \hat{\mathbf{q}}_k&=\mathbf{h}_k-\sum_{i=0}^{k-1}\frac{\hat{\mathbf{q}}_i^H\mathbf{h}_k}{\hat{\mathbf{q}}_i^H\hat{\mathbf{q}}_i}\,\hat{\mathbf{q}}_i
\end{cases}
$$

The unit vectors are:

$$
\mathbf{q}_i = \frac{\hat{\mathbf{q}}_i}{\|\hat{\mathbf{q}}_i \|}
$$

We obtain

$$
\begin{cases}
r_{ik}=\mathbf{q}_i^H\mathbf{h}_k\quad(i<k)\\[4pt]
r_{kk}=\|\hat{\mathbf{q}}_k\|
\end{cases}
$$

Gram–Schmidt has two expressions that are mathematically equivalent.

**Classical Gram–Schmidt (CGS)**: 

$$
\begin{cases}
\hat{\mathbf{q}}_0 = \mathbf{h}_0 \\
r_{00} = \| \mathbf{h}_0 \| \\
\mathbf{q}_0 = \dfrac{\mathbf{h}_0}{\| \mathbf{h}_0 \|}
\end{cases}
$$

$$
r_{ik} = \mathbf{q}_i^H \mathbf{h}_k, \qquad i = 0, \dots, k-1
$$

$$
\begin{cases}
\hat{\mathbf{q}}_k = \mathbf{h}_k - \sum_{i=0}^{k-1} r_{ik} \mathbf{q}_i \\
r_{kk} = \| \hat{\mathbf{q}}_k \| \qquad\qquad\qquad k = 1, \dots, n-1 \\
\mathbf{q}_k = \dfrac{\hat{\mathbf{q}}_k}{r_{kk}}
\end{cases}
$$

**Modified Gram–Schmidt (MGS)**: 

$$
r_{ik} = \mathbf{q}_i^H \hat{\mathbf{q}}_k^{(i)}, \qquad i = 0, \dots, k-1
$$

$$
\begin{cases}
\hat{\mathbf{q}}_k^{(i+1)} = \hat{\mathbf{q}}_k^{(i)} - r_{ik} \mathbf{q}_i \\
r_{kk} = \| \hat{\mathbf{q}}_k^{(k)} \| \qquad\qquad\qquad k = 1, \dots, n-1 \\
\mathbf{q}_k = \dfrac{\hat{\mathbf{q}}_k^{(k)}}{r_{kk}}
\end{cases}
$$

The only difference between the two approaches is whether $$\mathbf{h}_k$$ or $$\hat{\mathbf{q}}_k^{(i)}$$ is used to compute $$r_{ik}$$.

In floating point, MGS confines each rounding error to the directions already orthogonalized, where later steps keep cleaning it up.
When $$\mathbf{Q}_1$$ itself is required, MGS should be used since it can be obtained directly from the column-wise orthogonalization process.
In contrast, Householder and Givens require additional computation to obtain $$\mathbf{Q}_1$$ explicitly.

# 3. Givens Rotation

In the coordinate plane $$(i, j)$$, the rotation is described by two angles $$(\psi, \phi)$$: $$\psi \in [0, \pi/2]$$ is the rotation angle and $$\phi \in (-\pi, \pi]$$ is the phase angle.
Let $$\mathbf{G}_{ij}$$ be the identity with the $$2\times2$$ block at position $$(i,j)$$ replaced by

$$
\begin{bmatrix} \cos\psi & \sin\psi\,e^{j\phi} \\ -\sin\psi\,e^{-j\phi} & \cos\psi \end{bmatrix}
$$

The $$m \times m$$ rotation matrix differs from $$\mathbf{I}_m$$ only at the four entries where rows $$i$$, $$j$$ intersect columns $$i$$, $$j$$:

$$
\mathbf{G}_{ij} =
\begin{bmatrix}
1      &                &               &        &                       &        &   \\
       & \ddots         &               &        &                       &        &   \\
       &                & \cos\psi      &        & \sin\psi\,e^{j\phi}   &        &   \\
       &                &               & \ddots &                       &        &   \\
       &                & -\sin\psi\,e^{-j\phi} & & \cos\psi        &        &   \\
       &                &               &        &                       & \ddots &   \\
       &                &               &        &                       &        & 1
\end{bmatrix}
$$

Take the two rows $$\mathbf{H}_{i,:}$$ and $$\mathbf{H}_{j,:}$$ of $$\mathbf{H}$$.
To zero out the entry in row $$j$$ of column $$k$$, we define

$$
\begin{cases}
r = \sqrt{\lvert h_{ik} \rvert^2 + \lvert h_{jk} \rvert^2} > 0\\[4pt]
\psi = \arctan\!\left(\dfrac{\lvert h_{jk} \rvert}{\lvert h_{ik} \rvert}\right)\\[10pt]
\phi = \angle h_{jk} - \angle h_{ik}
\end{cases}
$$

Then, we obtain the following equation:

$$
\begin{bmatrix} \cos\psi & \sin\psi\,e^{j\phi} \\ -\sin\psi\,e^{-j\phi} & \cos\psi \end{bmatrix}
\begin{bmatrix} h_{ik} \\ h_{jk} \end{bmatrix}
= \begin{bmatrix} h_{ik}\,r / \lvert h_{ik} \rvert \\ 0 \end{bmatrix}
$$

One rotation can zero out one element, so zeroing out all subdiagonal elements requires

$$
p = \sum_{k=0}^{n-1}(m-1-k) = n(m-1) - \frac{n(n-1)}{2} = \frac{n(2m-n-1)}{2}
$$

Once all subdiagonal elements are zeroed out, the result is upper triangular:

$$
\mathbf{G}_{p-1} \cdots \mathbf{G}_0 \mathbf{H} = \mathbf{R}
$$

The $$\mathbf{V}$$ matrix in the 802.11 compressed beamforming feedback is compressed by Givens rotations into a set of quantized angles before being sent back.
The angles can be quantized to a fixed bit width, which costs far less than sending the whole unit vector.

# 4. Householder Reflection

Given a hyperplane passing through the origin, flipping a vector to its mirror is the reflection in that hyperplane.
It only does two things: keep the in-plane component unchanged and flip the sign of the component perpendicular to the plane.
Let the unit vector $$\mathbf{u}$$ be the normal of the hyperplane $$\mathbf{u}^{\perp} = \{\mathbf{h} : \mathbf{u}^H\mathbf{h} = 0\}$$, and divide $$\mathbf{h}$$ into two parts along the normal:

$$
\mathbf{h} = \underbrace{(\mathbf{I} - \mathbf{u}\mathbf{u}^H)\mathbf{h}}_{\mathbf{h}_\parallel \,\in\, \mathbf{u}^{\perp}} + \underbrace{(\mathbf{u}^H\mathbf{h})\mathbf{u}}_{\mathbf{h}_\perp \,\parallel\, \mathbf{u}}
$$

where $$\mathbf{u}^H\mathbf{h}$$ is a scalar, so $$\mathbf{u}\mathbf{u}^H\mathbf{h} = (\mathbf{u}^H\mathbf{h})\mathbf{u}$$.

Householder reflection only flips the sign of the normal component:

$$
\mathbf{P}\mathbf{h} = \mathbf{h}_\parallel - \mathbf{h}_\perp
$$

This is the standard form of the Householder reflection:

$$
\mathbf{P} = \mathbf{I} - 2\mathbf{u}\mathbf{u}^H
$$

Note that if the vector $$\mathbf{v}$$ is not normalized, the expression is $$\mathbf{P} = \mathbf{I} - 2\mathbf{v}\mathbf{v}^H / (\mathbf{v}^H\mathbf{v})$$.

Below are some properties of the Householder matrix.


| Property | Statement |
|:---|:---|
| Linear | $$\mathbf{P}$$ is a linear transformation |
| Involutory (self-inverse) | $$\mathbf{P}^2 = \mathbf{I}$$, i.e. $$\mathbf{P}^{-1} = \mathbf{P}$$; applying it twice returns the original |
| Hermitian | $$\mathbf{P}^H = \mathbf{P}$$ |
| Unitary (isometric) | $$\mathbf{P}^H\mathbf{P} = \mathbf{I}$$; it preserves lengths and inner products |
| Eigenvalues | $$-1$$ along $$\mathbf{u}$$, $$+1$$ inside the hyperplane |
| Determinant | $$\det(\mathbf{P}) = -1$$; it reverses orientation (a rotation has $$\det = +1$$) |

Householder reflection uses the right mirror to transform the tail of a column onto a coordinate axis in one step.
Let $$\mathbf{e}_i \in \mathbb{C}^m$$ be the $$i$$-th coordinate unit vector ($$1$$ in position $$i$$, $$0$$ elsewhere).
Step $$i$$ processes the **tail** of column $$i$$ of $$\mathbf{H}$$ located below the diagonal,

$$
\mathbf{h}_{i:m-1,\,i} = \begin{bmatrix} h_{i,i} & h_{i+1,i} & \cdots & h_{m-1,i} \end{bmatrix}^T \in \mathbb{C}^{m-i}
$$

and we want Householder reflection to convert it into a vector with only the first component nonzero, i.e.

$$
\tilde{\mathbf{P}}_i \mathbf{h}_{i:m-1,\,i} = \alpha\mathbf{e}_0, \qquad \lvert\alpha\rvert = \|\mathbf{h}_{i:m-1,\,i}\|
$$

where $$\mathbf{e}_0 = \begin{bmatrix} 1 & 0 & \cdots & 0 \end{bmatrix}^T \in \mathbb{C}^{m-i}$$.
The reflection is

$$
\tilde{\mathbf{P}}_i = \mathbf{I} - 2\frac{\mathbf{v}\mathbf{v}^H}{\mathbf{v}^H\mathbf{v}}
$$

where

$$
\mathbf{v} = \mathbf{h}_{i:m-1,\,i} - \alpha\mathbf{e}_0
$$

and

$$
\alpha = -\frac{h_{i,i}}{\lvert h_{i,i}\rvert}\|\mathbf{h}_{i:m-1,\,i}\|
$$


We embed it into the $$m \times m$$ matrix:

$$
\mathbf{P}_i = \begin{bmatrix} \mathbf{I}_{i} & \mathbf{0} \\ \mathbf{0} & \tilde{\mathbf{P}}_i \end{bmatrix}
$$

Process the matrix column by column, and finally obtain the upper triangular matrix:

$$
\mathbf{P}_{n-1} \cdots \mathbf{P}_0 \mathbf{H} = \mathbf{R}
$$

Householder reflection is typically used for tridiagonalization, not for QR decomposition itself.
