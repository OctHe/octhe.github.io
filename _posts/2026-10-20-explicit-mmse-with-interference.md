---
title: Explicit MMSE with Interference
layout: default
ai_assist: true
---

# 1. System model

The received signal is

$$
\mathbf{y} = \mathbf{H} \mathbf{x} + \mathbf{H}_{\text{intf}} \mathbf{x}_{\text{intf}} + \mathbf{n}, \qquad
\mathbf{H} \in \mathbb{C}^{N_r \times N_t}, \quad
\mathbf{H}_{\text{intf}} \in \mathbb{C}^{N_r \times N_{\text{intf}}}, \quad
\mathbf{n} \sim \mathcal{CN}(\mathbf{0}, \sigma^2 \mathbf{I})
$$

The interference and the Gaussian noise are kept independent: the colored interference is the term $$\mathbf{H}_{\text{intf}} \mathbf{x}_{\text{intf}}$$, and the white noise is the term $$\mathbf{n}$$.

The covariance of the interference and the noise is:

$$
\mathbf{R}_{nn} = \mathbb{E}[(\mathbf{H}_{\text{intf}} \mathbf{x}_{\text{intf}} + \mathbf{n})(\mathbf{H}_{\text{intf}} \mathbf{x}_{\text{intf}} + \mathbf{n})^H]
= \mathbf{H}_{\text{intf}} \mathbf{H}_{\text{intf}}^H + \sigma^2 \mathbf{I}
$$

where $\mathbb{E}[\mathbf{x}_{\text{intf}} \mathbf{x}_{\text{intf}}^H] = \mathbf{I}$ and $\mathbb{E}[\mathbf{n} \mathbf{n}^H] = \sigma^2 \mathbf{I}$.

$$
\mathbf{R}_{nn} = \mathbb{E}[(\mathbf{H}_{\text{intf}} \mathbf{x}_{\text{intf}} + \mathbf{n})(\mathbf{H}_{\text{intf}} \mathbf{x}_{\text{intf}} + \mathbf{n})^H]
= \mathbf{H}_{\text{intf}} \mathbf{H}_{\text{intf}}^H + \sigma^2 \mathbf{I}
$$

The $$\sigma^2 \mathbf{I}$$ term makes $$\mathbf{R}_{nn}$$ positive definite.

# 2. MMSE with LDL Decomposition

The estimate is

$$
\hat{\mathbf{x}} = \mathbf{W} \mathbf{y}
$$

where the weight matrix is

$$
\mathbf{W} = \mathbf{H}^H (\mathbf{H} \mathbf{H}^H + \mathbf{R}_{nn})^{-1}
$$

We define

$$
\mathbf{A} = \mathbf{H} \mathbf{H}^H + \mathbf{R}_{nn}
$$

The inverse of $$\mathbf{A}$$ is built from the LDL decomposition, i.e., $$ \mathbf{A} = \mathbf{L} \mathbf{D} \mathbf{L}^H $$.
$$\mathbf{L}$$ and $$\mathbf{D}$$ are

$$
\begin{cases}
d_0 = a_{00}, & j = 0 \\
d_j = a_{jj} - \sum_{k=0}^{j-1} \lvert l_{jk} \rvert^2 d_k, & j \ge 1 \\
l_{ij} = \dfrac{1}{d_j} \biggl( a_{ij} - \sum_{k=0}^{j-1} l_{ik} l_{jk}^H d_k \biggr), & i = j + 1, \dots, N_r - 1
\end{cases}
$$

Their inverses are, with the intermediate matrix $$\mathbf{M} = \mathbf{L}^{-1}$$,

$$
\begin{cases}
\mathbf{D}^{-1} = \operatorname{diag}(d_0^{-1}, d_1^{-1}, \dots, d_{N_r - 1}^{-1}) \\
M_{jj} = 1, & j = 0, \dots, N_r - 1 \\
M_{ij} = -\sum_{k=j}^{i-1} l_{ik} M_{kj}, & i = j + 1, \dots, N_r - 1
\end{cases}
$$

The inverse of $$\mathbf{A}$$ is

$$
\mathbf{A}^{-1} = \mathbf{L}^{-H} \mathbf{D}^{-1} \mathbf{L}^{-1} = \mathbf{M}^H \mathbf{D}^{-1} \mathbf{M}
$$

The weight matrix is

$$
\mathbf{W} = \mathbf{H}^H \mathbf{A}^{-1}
$$

# 3. MMSE with Interference Whitening

MMSE has another form:

$$
\mathbf{W} = (\mathbf{H}^H \mathbf{R}_{nn}^{-1} \mathbf{H} + \mathbf{I})^{-1} \mathbf{H}^H \mathbf{R}_{nn}^{-1}
$$

The two forms are equivalent:

$$
\mathbf{H}^H (\mathbf{H} \mathbf{H}^H + \mathbf{R}_{nn})^{-1}
= (\mathbf{H}^H \mathbf{R}_{nn}^{-1} \mathbf{H} + \mathbf{I})^{-1} \mathbf{H}^H \mathbf{R}_{nn}^{-1}
$$

In this section, we first whiten the interference by using Cholesky decomposition, and then applies the MMSE with the white noise.


The interference can be transferred to $ \mathbf{R}_{nn} = \mathbf{L}_n \mathbf{L}_n^H $

where

$$
\begin{cases}
l_{n,00} = \sqrt{r_{00}}, & j = 0 \\
l_{n,jj} = \sqrt{r_{jj} - \sum_{k=0}^{j-1} \lvert l_{n,jk} \rvert^2}, & j \ge 1 \\
l_{n,ij} = \dfrac{1}{l_{n,jj}} \biggl( r_{ij} - \sum_{k=0}^{j-1} l_{n,ik} l_{n,jk}^H \biggr), & i = j + 1, \dots, N_r - 1
\end{cases}
$$

The inverse of $$\mathbf{L}_n$$ is $$\mathbf{M}_n = \mathbf{L}_n^{-1}$$

$$
\begin{cases}
M_{n,jj} = \dfrac{1}{l_{n,jj}}, & j = 0, \dots, N_r - 1 \\
M_{n,ij} = -\dfrac{1}{l_{n,ii}} \sum_{k=j}^{i-1} l_{n,ik} M_{n,kj}, & i = j + 1, \dots, N_r - 1
\end{cases}
$$

The whitened signal and channel are

$$
\begin{cases}
\mathbf{y}_w = \mathbf{L}_n^{-1} \mathbf{y} \\
\mathbf{H}_w = \mathbf{L}_n^{-1} \mathbf{H}
\end{cases}
$$

The whitened model is

$$
\mathbf{y}_w = \mathbf{H}_w \mathbf{x} + \mathbf{n}_w
$$

where $$ \mathbf{n}_w = \mathbf{L}_n^{-1} (\mathbf{H}_{\text{intf}} \mathbf{x}_{\text{intf}} + \mathbf{n}) $$.

It is easy to prove that $\mathbf{n}_w$ carries no interference term with
$$
\mathbb{E}[\mathbf{n}_w \mathbf{n}_w^H]
= \mathbf{L}_n^{-1} \mathbf{R}_{nn} \mathbf{L}_n^{-H}
= \mathbf{I}
$$.


Finally, the interference model is transferred to a Gaussian noise model, and the problem can be solved with the MMSE for Gaussian noise:

$$
\mathbf{A}_w = \mathbf{H}_w^H \mathbf{H}_w + \mathbf{I}
$$

Its LDL decomposition is

$$
\mathbf{A}_w = \mathbf{L}_w \mathbf{D}_w \mathbf{L}_w^H
$$

The results of $$\mathbf{L}_w$$ and $$\mathbf{D}_w$$ are

$$
\begin{cases}
d_{w,0} = a_{w,00}, & j = 0 \\
d_{w,j} = a_{w,jj} - \sum_{k=0}^{j-1} \lvert l_{w,jk} \rvert^2 d_{w,k}, & j \ge 1 \\
l_{w,ij} = \dfrac{1}{d_{w,j}} \biggl( a_{w,ij} - \sum_{k=0}^{j-1} l_{w,ik} l_{w,jk}^H d_{w,k} \biggr), & i = j + 1, \dots, N_t - 1
\end{cases}
$$

Their inverses are

$$
\begin{cases}
\mathbf{D}_w^{-1} = \operatorname{diag}(d_{w,0}^{-1}, d_{w,1}^{-1}, \dots, d_{w,N_t - 1}^{-1}) \\
M_{w,jj} = 1, & j = 0, \dots, N_t - 1 \\
M_{w,ij} = -\sum_{k=j}^{i-1} l_{w,ik} M_{w,kj}, & i = j + 1, \dots, N_t - 1
\end{cases}
$$

The inverse of $$\mathbf{A}_w$$ is

$$
\mathbf{A}_w^{-1} = \mathbf{L}_w^{-H} \mathbf{D}_w^{-1} \mathbf{L}_w^{-1} = \mathbf{M}_w^H \mathbf{D}_w^{-1} \mathbf{M}_w
$$

The equalizer is

$$
\hat{\mathbf{x}} = \mathbf{W}_w \mathbf{y}_w, \qquad
\mathbf{W}_w = \mathbf{A}_w^{-1} \mathbf{H}_w^H
= (\mathbf{H}_w^H \mathbf{H}_w + \mathbf{I})^{-1} \mathbf{H}_w^H
$$

Approach 1 inverts an $$N_r \times N_r$$ matrix, while Approach 2 only whitens with it and moves the inversion onto the $$N_t \times N_t$$ matrix.
Approach 2 suits massive MIMO with $$N_r \gg N_t$$.


