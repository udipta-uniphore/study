# Algebra 101: Matrix Decompositions

Order: LU, Cholesky, QR, eigendecomposition, SVD. Each part uses the ideas of the one before it. LU is elimination written as matrices. Cholesky is LU for symmetric positive definite matrices. QR is orthogonalization. Eigendecomposition needs the spectral theorem. SVD uses eigendecomposition of $A^TA$.

---

# Background: inner products, adjoints, symmetric matrices

---

## 0. Inner product

For real vectors, the standard inner product is the dot product

$$\langle u, v\rangle = u\cdot v = u^Tv$$

It gives lengths $\|v\|^2 = \langle v,v\rangle$ and angles. Two vectors are perpendicular when $\langle u,v\rangle = 0$.

For complex vectors, the inner product conjugates one side:

$$\langle u, v\rangle = \bar u^T v$$

This keeps $\langle v,v\rangle = \|v\|^2$ real and nonnegative, which is why section 17 uses $\bar v$.

---

## 1. Adjoint

The adjoint $A^*$ of a matrix $A$ is defined by

$$\langle Au, v\rangle = \langle u, A^*v\rangle \quad\text{for all } u, v$$

With the standard inner product:

- For real matrices, $A^* = A^T$.
- For complex matrices, $A^* = \bar A^T$, the conjugate transpose.

The adjoint depends on the inner product. With a different inner product, such as a weighted one, the adjoint and the matrix conditions that come from it change.

---

## 2. Symmetric and Hermitian matrices

A matrix is **self-adjoint** when $A^* = A$:

- **Real symmetric:** $A^T = A$. This is the real self-adjoint case.
- **Complex Hermitian:** $A^* = A$, i.e. $\bar A^T = A$.

Two facts used throughout:

- **Real eigenvalues.** A self-adjoint matrix has real eigenvalues. Section 17 proves this with $\bar v$.
- **Orthogonal eigenvectors.** Eigenvectors for different eigenvalues are perpendicular. Section 17.1 proves this for real symmetric matrices.

---

# Part 1: LU decomposition

---

## 1. What it is

A square matrix $A$ can be written as

$$A = LU$$

with $L$ unit lower triangular (ones on the diagonal) and $U$ upper triangular. It's Gaussian elimination, recorded as matrices: the multipliers used to zero out entries go into $L$, and the resulting upper triangular matrix is $U$.

Pivoting (row swaps) is needed when a pivot is zero or small, giving $PA = LU$ with a permutation $P$.

---

## 2. Why it works

Assume every pivot is nonzero, so no row swaps are needed.

**Step 1: elimination is a sequence of lower triangular steps.** Each elimination step subtracts a multiple of one row from a later row. That step is multiplication on the left by a unit lower triangular matrix $E_k$. After $m$ steps,

$$E_m\cdots E_2E_1 A = U$$

with $U$ upper triangular, because every entry below the diagonal has been zeroed.

**Step 2: invert the product.** Each $E_k$ is unit lower triangular, so $E_k^{-1}$ is too. Products and inverses of unit lower triangular matrices stay unit lower triangular. Let $L = (E_m\cdots E_1)^{-1}$. Then $A = LU$, with $L$ unit lower triangular.

**Step 3: when does it exist?** The pivots are nonzero exactly when every leading principal minor is nonzero. That's the condition for LU without pivoting.

**Uniqueness.** If $L$ is required to be unit lower triangular, the factorization is unique when it exists.


---

## 3. Worked example

$$A = \begin{pmatrix}2 & 1\\ 4 & 3\end{pmatrix}$$

**Step 1.** Eliminate the entry below the first pivot: multiply row 1 by $2$ and subtract from row 2. The multiplier $\ell_{21} = 2$ goes into $L$.

**Step 2.** The new second row is $(0, 3 - 2\cdot1) = (0,1)$, so $u_{22} = 1$.

**Result:**
$$L = \begin{pmatrix}1 & 0\\ 2 & 1\end{pmatrix}, \qquad U = \begin{pmatrix}2 & 1\\ 0 & 1\end{pmatrix}$$

**Check.** $LU = \begin{pmatrix}2 & 1\\ 4 & 2+1\end{pmatrix} = A$. ✓ And $\det A = \det U = 2\cdot1 = 2 = 6-4$. ✓

---

## 4. Where it's used

- **Solving $Ax = b$.** Factor once, then solve $Ly = b$ and $Ux = y$. Repeated solves with the same $A$ are cheap.
- **Determinants and inverses.** $\det A = \pm\prod_i u_{ii}$, and inverses come from the same factors.

---

## 5. Practical notes

- `scipy.linalg.lu(A)` returns $P$, $L$, $U$ with pivoting, so `A = P @ L @ U`.
- `scipy.linalg.lu_factor` and `lu_solve` do the factor-then-solve pattern.

---

# Part 2: Cholesky decomposition

---

## 6. What it is

A symmetric positive definite matrix $A$ can be written as

$$A = LL^T$$

with $L$ lower triangular and positive diagonal. It's a square root of $A$, and it costs about half the work of LU for the same matrix.

Entries, computed column by column:
$$L_{jj} = \sqrt{A_{jj} - \sum_{k<j}L_{jk}^2}, \qquad L_{ij} = \frac{1}{L_{jj}}\Big(A_{ij} - \sum_{k<j}L_{ik}L_{jk}\Big) \text{ for } i>j$$

---

## 7. Why it works

Let $A$ be symmetric positive definite, so $x^TAx > 0$ for all $x \ne 0$. Induct on the size $n$.

**Base case.** For $n=1$, $A = (\alpha)$ with $\alpha > 0$, and $L = (\sqrt\alpha)$.

**Inductive step.** Write $A$ in block form, with $\alpha = A_{11} > 0$ (since $e_1^TAe_1 > 0$):
$$A = \begin{pmatrix}\alpha & a^T\\ a & B\end{pmatrix}$$

Let $S = B - \frac{1}{\alpha}aa^T$. Then

$$A = \begin{pmatrix}1 & 0\\ a/\alpha & I\end{pmatrix}\begin{pmatrix}\alpha & 0\\ 0 & S\end{pmatrix}\begin{pmatrix}1 & a^T/\alpha\\ 0 & I\end{pmatrix}$$

**$S$ is positive definite.** For $y \ne 0$, take $x = (-a^Ty/\alpha,\ y)$. Then $x \ne 0$ and $x^TAx = y^TSy$. Since $x^TAx > 0$, $y^TSy > 0$.

By induction, $S = L'L'^T$ for some lower triangular $L'$ with positive diagonal. Then

$$A = \begin{pmatrix}\sqrt\alpha & 0\\ a/\sqrt\alpha & L'\end{pmatrix}\begin{pmatrix}\sqrt\alpha & 0\\ a/\sqrt\alpha & L'\end{pmatrix}^T$$

**Uniqueness.** The first column of $L$ is forced: $L_{11}^2 = \alpha$ and $L_{i1}L_{11} = a_i$. The rest is forced by applying the same argument to $S$.

---

## 8. Worked example

$$A = \begin{pmatrix}4 & 2\\ 2 & 3\end{pmatrix}$$

**Step 1.** $L_{11} = \sqrt{4} = 2$, and $L_{21} = 2/2 = 1$.

**Step 2.** $L_{22} = \sqrt{3 - 1^2} = \sqrt2$.

**Result:** $L = \begin{pmatrix}2 & 0\\ 1 & \sqrt2\end{pmatrix}$.

**Check.** $LL^T = \begin{pmatrix}4 & 2\\ 2 & 1+2\end{pmatrix} = A$. ✓

**Log-determinant.** $\det A = (2\cdot\sqrt2)^2 = 8 = 12 - 4$. In general $\log\det A = 2\sum_i \log L_{ii}$, which is how Gaussian log-likelihoods are computed without forming $\det$ directly.

---

## 9. Where it's used

- **Sampling from $N(\mu,\Sigma)$.** Let $Z\sim N(0,I)$ and $X = \mu + LZ$. Then $\text{Cov}(X) = LL^T = \Sigma$. This is cheaper and more stable than the eigendecomposition route $\Sigma^{1/2}Z$ (stat101 §5).
- **Gaussian log-likelihood.** Needs $\log\det\Sigma$ and $\Sigma^{-1}$ applied to vectors, both available from $L$ by triangular solves.
- **Solving $\Sigma x = b$.** Solve $Ly = b$, then $L^Tx = y$.

---

## 10. Practical notes

- `np.linalg.cholesky(A)` returns $L$ and raises an error if $A$ is not positive definite. That error is a useful check on a covariance estimate.
- A sample covariance from few observations can be singular, so Cholesky fails. Add a small ridge $\epsilon I$ or use more data.

---

# Part 3: QR decomposition

---

## 11. What it is

Any $m\times n$ matrix $A$ with linearly independent columns (so $m \ge n$) can be written as

$$A = QR$$

- $Q$ is $m\times n$ with **orthonormal columns**: $Q^TQ = I$.
- $R$ is $n\times n$, **upper triangular**, with positive diagonal entries.

Plain meaning: the columns of $A$ are re-expressed in a new set of perpendicular unit directions (the columns of $Q$). $R$ records how much of each old column lies along each new direction.

---

## 12. Why it works: Gram-Schmidt

Take the columns $a_1, \dots, a_n$ of $A$, and define

1. $v_1 = a_1$, $q_1 = v_1/\|v_1\|$.
2. For $k \ge 2$: $v_k = a_k - \sum_{j<k}(q_j^Ta_k)\,q_j$, and $q_k = v_k/\|v_k\|$.

**Claim: $v_k \ne 0$, and $q_1,\dots,q_k$ are orthonormal with $\text{span}(q_1,\dots,q_k) = \text{span}(a_1,\dots,a_k)$.**

Induct on $k$. Assume it holds up to $k-1$.

- **$v_k \ne 0$.** $a_k$ is not in $\text{span}(a_1,\dots,a_{k-1})$, since the columns are independent. That span equals $\text{span}(q_1,\dots,q_{k-1})$, and $v_k$ is $a_k$ minus its projection onto that span, so $v_k \ne 0$.
- **Orthogonality.** For $j<k$, $q_j^Tv_k = q_j^Ta_k - (q_j^Ta_k)\,q_j^Tq_j = 0$, using $q_j^Tq_j = 1$ and $q_j^Tq_i = 0$ for $i<k$ with $i\neq j$. So $q_k$ is orthogonal to all earlier $q_j$, and it has unit length by construction.
- **Span.** $a_k = \sum_{j<k}(q_j^Ta_k)\,q_j + \|v_k\|\,q_k$, so $a_k$ is in $\text{span}(q_1,\dots,q_k)$, and the spans match.

Reading off the coefficients:
$$r_{jk} = q_j^Ta_k \ (j<k), \qquad r_{kk} = \|v_k\| > 0, \qquad r_{jk} = 0 \ (j>k)$$

gives $a_k = \sum_{j\le k} r_{jk}\,q_j$ for every $k$, which is $A = QR$.

---

## 13. Worked example

$$A = \begin{pmatrix}1 & 1\\ 1 & 0\\ 0 & 1\end{pmatrix}, \qquad a_1 = (1,1,0),\quad a_2 = (1,0,1)$$

**Step 1.** $\|a_1\| = \sqrt2$, so $q_1 = \tfrac{1}{\sqrt2}(1,1,0)$ and $r_{11} = \sqrt2$.

**Step 2.** $r_{12} = q_1^Ta_2 = \tfrac{1}{\sqrt2}$.
$$v_2 = a_2 - r_{12}q_1 = (1,0,1) - \tfrac12(1,1,0) = \left(\tfrac12, -\tfrac12, 1\right)$$
$\|v_2\|^2 = \tfrac14+\tfrac14+1 = \tfrac32$, so $r_{22} = \sqrt{\tfrac32} = \tfrac{\sqrt6}{2}$ and
$$q_2 = \tfrac{2}{\sqrt6}\left(\tfrac12,-\tfrac12,1\right) = \left(\tfrac{1}{\sqrt6}, -\tfrac{1}{\sqrt6}, \tfrac{2}{\sqrt6}\right)$$

**Result:**
$$Q = \begin{pmatrix}\tfrac{1}{\sqrt2} & \tfrac{1}{\sqrt6}\\ \tfrac{1}{\sqrt2} & -\tfrac{1}{\sqrt6}\\ 0 & \tfrac{2}{\sqrt6}\end{pmatrix},\qquad R = \begin{pmatrix}\sqrt2 & \tfrac{1}{\sqrt2}\\ 0 & \tfrac{\sqrt6}{2}\end{pmatrix}$$

**Check.** $a_2 = r_{12}q_1 + r_{22}q_2 = (\tfrac12,\tfrac12,0) + (\tfrac12,-\tfrac12,1) = (1,0,1)$. ✓

---

## 14. Where it's used: least squares

To fit $y \approx X\beta$ by least squares, the normal equations are $X^TX\beta = X^Ty$. With $X = QR$:

$$X^TX = R^TQ^TQR = R^TR$$

so the normal equations become

$$R\beta = Q^Ty$$

This is an upper-triangular system, solved by back substitution with no matrix inverse. QR is more stable than forming $X^TX$ directly, because forming $X^TX$ squares the condition number.

Connection to stat101 §12: $QQ^Ty$ is the projection of $y$ onto the column space of $X$, and the residual $y - QQ^Ty$ is perpendicular to it.

---

## 15. Practical notes

- **Sign convention.** $Q$ and $R$ are unique once the signs of the $r_{kk}$ are fixed positive. Libraries may flip column signs of $Q$ and row signs of $R$ together; the product $QR$ is the same.
- **Numerical stability.** Classical Gram-Schmidt loses orthogonality on ill-conditioned matrices. Modified Gram-Schmidt, or Householder reflections (used by LAPACK), are more stable.
- `np.linalg.qr(A)` should give $Q$ and $R$ matching the example up to column signs. Check $QR = A$ and $Q^TQ = I$.

---

# Part 4: Eigendecomposition

---

## 16. What it is

A square matrix $A$ with a full set of eigenvectors can be written as

$$A = V\Lambda V^{-1}$$

- The columns of $V$ are eigenvectors $v_i$.
- $\Lambda$ is diagonal, holding the eigenvalues $\lambda_i$.

Plain meaning: along each eigenvector, $A$ only stretches by $\lambda_i$. No rotation is involved.

For a **real symmetric** matrix, which is the case that matters here (covariance matrices), $V$ can be chosen orthogonal, so $V^{-1} = V^T$:

$$A = Q\Lambda Q^T, \qquad Q^TQ = I$$

The eigenvalues are real, and the eigenvectors are perpendicular.

---

## 17. Why it works: the spectral theorem

Let $A$ be real symmetric ($A^T = A$).

**Step 1: eigenvalues are real.** Let $Av = \lambda v$ with $v \in \mathbb{C}^n$, $v \ne 0$. Let $s = \bar v^TAv$. Then $s = \lambda\|v\|^2$. Also $\bar s = v^TA\bar v = \bar v^TA^Tv = \bar v^TAv = s$, since $A$ is real and symmetric. So $s$ is real, and $\lambda = s/\|v\|^2$ is real.

**Step 2: a real eigenvector exists.** Maximize $f(x) = x^TAx$ over the unit sphere $\|x\|=1$. The sphere is compact, so a maximum $x^*$ exists. At a maximum of $f$ restricted to the sphere, the gradient of $f$ is parallel to the gradient of $\|x\|^2$. That gives $2Ax^* = 2\mu x^*$ for some $\mu$, so $x^*$ is an eigenvector.

**Step 3: the orthogonal complement is invariant.** Let $Av = \lambda v$ with $\|v\| = 1$, and let $w \perp v$. Then

$$(Aw)^Tv = w^TAv = \lambda\, w^Tv = 0$$

So $Aw \perp v$. $A$ maps $v^\perp$ into itself, and the restriction is still symmetric on an $(n-1)$-dimensional space.

**Step 4: induct.** Repeat on $v^\perp$. The eigenvectors found this way are orthonormal, so they form the columns of $Q$, and $A = Q\Lambda Q^T$.

---

### 17.1 Orthogonality of eigenvectors

Let $A$ be real symmetric, with $Av = \lambda v$, $Aw = \mu w$, and $\lambda \ne \mu$. Then

$$\lambda\,(v\cdot w) = (Av)^Tw = v^TA^Tw = v^TAw = v^T(\mu w) = \mu\,(v\cdot w)$$

The third equality uses $A^T = A$. So $(\lambda-\mu)(v\cdot w) = 0$, and since $\lambda \ne \mu$, $v\cdot w = 0$. $\blacksquare$

Eigenvectors for different eigenvalues are perpendicular by this argument. Two eigenvectors for the same eigenvalue are not covered by it, so for a repeated eigenvalue, choose an orthonormal basis of its eigenspace (Gram-Schmidt within that space). That's what steps 3-4 of section 17 do.

---

## 18. Worked example

$$A = \begin{pmatrix}2 & 1\\ 1 & 2\end{pmatrix}$$

**Eigenvalues.** $\det(A - \lambda I) = (2-\lambda)^2 - 1 = 0$, so $\lambda = 3$ or $\lambda = 1$.

**Eigenvectors.** For $\lambda=3$: $v = \tfrac{1}{\sqrt2}(1,1)$. For $\lambda=1$: $v = \tfrac{1}{\sqrt2}(1,-1)$. These are perpendicular, as Step 3 predicts.

**Result:**
$$A = \begin{pmatrix}\tfrac{1}{\sqrt2} & \tfrac{1}{\sqrt2}\\ \tfrac{1}{\sqrt2} & -\tfrac{1}{\sqrt2}\end{pmatrix}\begin{pmatrix}3 & 0\\ 0 & 1\end{pmatrix}\begin{pmatrix}\tfrac{1}{\sqrt2} & \tfrac{1}{\sqrt2}\\ \tfrac{1}{\sqrt2} & -\tfrac{1}{\sqrt2}\end{pmatrix}$$

**Check.** $\det A = 3\cdot 1 = 3 = 4-1$. ✓

---

## 19. Where it's used

- **Covariance structure.** $\Sigma = Q\Lambda Q^T$: eigenvectors are the principal axes of the Gaussian ellipse, and eigenvalues are the variances along them (stat101 §6).
- **Whitening.** $\Sigma^{-1/2} = Q\Lambda^{-1/2}Q^T$ (stat101 §3).
- **PCA.** The eigendecomposition of $S$ gives the principal directions directly. The SVD of the data matrix gives the same result more stably (section 24).
- **Positive definite check.** A symmetric $A$ is positive definite exactly when all $\lambda_i > 0$.

---

## 20. Practical notes

- `np.linalg.eigh(A)` is for symmetric matrices. It returns real eigenvalues in ascending order and orthogonal eigenvectors as columns. Use `eig` only for non-symmetric matrices.
- Check: `Q @ np.diag(w) @ Q.T` should reproduce $A$.

---

# Part 5: Singular Value Decomposition (SVD)

---

## 21. What it is

Any $m\times n$ real matrix $A$ can be written as

$$A = U\Sigma V^T$$

- $U$ is $m\times m$, orthogonal: its columns $u_i$ are orthonormal.
- $V$ is $n\times n$, orthogonal: its columns $v_i$ are orthonormal.
- $\Sigma$ is $m\times n$, diagonal, with $\sigma_1 \ge \sigma_2 \ge \dots \ge 0$ on the diagonal. These are the **singular values**.

Plain meaning: every linear map is a rotation (or reflection), then a stretch along the coordinate axes, then another rotation. $V^T$ rotates the input, $\Sigma$ stretches each axis by $\sigma_i$, and $U$ rotates the output.

Equivalently, $A = \sum_i \sigma_i\,u_iv_i^T$, a sum of rank-1 pieces ordered by strength.

---

## 22. Why it works

**Step 1: eigendecompose $A^TA$.** $A^TA$ is real symmetric, so by section 17 it has orthonormal eigenvectors $v_1,\dots,v_n$ with eigenvalues $\lambda_i$. Also $\lambda_i = \|Av_i\|^2/\|v_i\|^2 \ge 0$, since $v^TA^TAv = \|Av\|^2$. Order them so $\lambda_1 \ge \dots \ge \lambda_n \ge 0$, and set $\sigma_i = \sqrt{\lambda_i}$.

**Step 2: define the left vectors.** Let $r$ be the number of nonzero $\sigma_i$. For $i \le r$, define $u_i = Av_i/\sigma_i$. These are orthonormal:

$$u_i^Tu_j = \frac{v_i^TA^TAv_j}{\sigma_i\sigma_j} = \frac{\lambda_j\,v_i^Tv_j}{\sigma_i\sigma_j} = \delta_{ij}$$

For $i > r$, $\|Av_i\|^2 = \lambda_i = 0$, so $Av_i = 0$. Extend $u_1,\dots,u_r$ to an orthonormal basis $u_1,\dots,u_m$ of $\mathbb{R}^m$ by adding any orthonormal vectors orthogonal to them.

**Step 3: assemble.** In every case $Av_i = \sigma_i u_i$. Stack these as $AV = U\Sigma$, then multiply on the right by $V^T$:
$$A = U\Sigma V^T$$

---

## 23. Worked example (rank 1)

$$A = \begin{pmatrix}1 & 1\\ 1 & 1\end{pmatrix}$$

**Step 1.** $A^TA = \begin{pmatrix}2 & 2\\ 2 & 2\end{pmatrix}$. Eigenvalues $4$ and $0$. So $\sigma_1 = 2$, $\sigma_2 = 0$.

**Step 2.** Right vectors: $v_1 = \tfrac{1}{\sqrt2}(1,1)$ for $\lambda=4$, and $v_2 = \tfrac{1}{\sqrt2}(1,-1)$ for $\lambda=0$.

**Step 3.** Left vectors: $u_1 = Av_1/\sigma_1$. Here $Av_1 = (\sqrt2,\sqrt2)$, so $u_1 = \tfrac{1}{\sqrt2}(1,1)$. Since $\sigma_2 = 0$, $u_2$ is any unit vector perpendicular to $u_1$, e.g. $\tfrac{1}{\sqrt2}(1,-1)$.

**Result:**
$$A = 2\cdot\tfrac{1}{\sqrt2}\begin{pmatrix}1\\1\end{pmatrix}\cdot\tfrac{1}{\sqrt2}\begin{pmatrix}1 & 1\end{pmatrix} = \begin{pmatrix}1&1\\1&1\end{pmatrix}$$

**Check.** The rank is the number of nonzero singular values, here 1. Both columns of $A$ are multiples of $(1,1)$, which matches.

---

## 24. Where it's used

- **Rank and conditioning.** Rank = count of nonzero $\sigma_i$. Condition number $\sigma_1/\sigma_r$ measures how sensitive solutions are.
- **Pseudoinverse.** $A^+ = V\Sigma^+U^T$, where $\Sigma^+$ inverts the nonzero singular values and keeps zeros as zeros. Least squares for rank-deficient problems uses it: $\hat\beta = A^+y$.
- **Low-rank approximation.** Keeping the top $k$ terms gives the best rank-$k$ approximation in Frobenius and spectral norm (Eckart-Young). Compression and denoising use this.
- **PCA (stat101 §3).** Let $X$ be the centered $n\times p$ data matrix. Then $S = \frac{1}{n-1}X^TX = V\,\frac{\Sigma^2}{n-1}\,V^T$. So the principal directions are the right singular vectors $v_i$, and the variances along them are $\sigma_i^2/(n-1)$. You can do PCA from the SVD of $X$ directly, without forming $S$, which is more stable.

---

## 25. Practical notes

- `np.linalg.svd(A)` returns $U$, the singular values as a 1D array, and $V^T$ (not $V$).
- Signs of paired columns of $U$ and $V$ can flip together. The product is unchanged.
- Check: `U @ np.diag(s) @ Vt` should reproduce $A$, and $U^TU$ should be $I$.

---

# Part 6: Principal component analysis (PCA)

---

## 26. What it is

Given centered data $X$ ($n$ observations, $p$ variables) with sample covariance $S = \frac{1}{n-1}X^TX$, PCA finds perpendicular directions $w_1, w_2, \dots$ along which the projected data has the largest variance.

The projection of observation $x$ onto direction $w$ is the score $w^Tx$. Its sample variance is

$$\text{Var}(Xw) = \frac{1}{n-1}w^TX^TXw = w^TSw$$

So PCA is: maximize $w^TSw$ subject to $\|w\| = 1$, then repeat with the constraint that each new direction is perpendicular to the earlier ones.

---

## 27. Why it works

**First direction.** Let $S = Q\Lambda Q^T$ with $\lambda_1 \ge \dots \ge \lambda_p$ (section 17). Write any unit $w = Q y$, with $\|y\|=1$. Then

$$w^TSw = y^T\Lambda y = \sum_i \lambda_i y_i^2 \le \lambda_1\sum_i y_i^2 = \lambda_1$$

with equality when $w = q_1$, the top eigenvector. So $w_1 = q_1$ and the maximal variance is $\lambda_1$.

**Later directions.** Maximize over directions perpendicular to $w_1,\dots,w_{k-1}$. The same argument, restricted to the remaining eigenvectors, gives $w_k = q_k$ with variance $\lambda_k$. This is the induction step from section 17, applied to the variance.

**Total variance.** $\sum_i \text{Var}(Xq_i) = \text{tr}(S) = \sum_i \lambda_i$. Rotating into the eigenbasis doesn't change the total variance, only how it's distributed.

---

## 28. Dimensionality reduction

Keep the top $k$ directions, $W_k = [q_1,\dots,q_k]$, and project: $Z = XW_k$ (an $n\times k$ matrix of scores).

- **Fraction of variance kept:** $\dfrac{\sum_{i\le k}\lambda_i}{\sum_i \lambda_i}$.
- **Reconstruction:** $\hat X = ZW_k^T$. The error is the variance in the discarded directions, $\sum_{i>k}\lambda_i$ (per observation, up to the $n-1$ factor).

---

## 29. Worked example

Suppose centered data has sample covariance

$$S = \begin{pmatrix}2 & 1\\ 1 & 2\end{pmatrix}$$

This is the matrix from section 18, with eigenvalues $3$ and $1$ and eigenvectors $q_1 = \tfrac{1}{\sqrt2}(1,1)$, $q_2 = \tfrac{1}{\sqrt2}(1,-1)$.

- **PC1** is $q_1$: the direction where the two variables move together. Variance $3$.
- **PC2** is $q_2$: the contrast between them. Variance $1$.
- **Keep one component:** fraction of variance $= 3/(3+1) = 75\%$.

The data is mostly spread along the diagonal, so one coordinate captures most of it.

---

## 30. Computing it: two routes

1. **Eigendecomposition of $S$.** Form $S = \frac{1}{n-1}X^TX$, then eigendecompose. Simple, but forming $X^TX$ squares the condition number.
2. **SVD of $X$ directly.** Write $X = U\Sigma V^T$. Then the principal directions are the columns of $V$, the variances are $\sigma_i^2/(n-1)$, and the scores are $Z = U\Sigma$. This is the stable route (section 24).

Both give the same directions. Use route 2 in code.

---

## 31. Practical notes

- **Center first.** PCA on uncentered data finds the direction of the mean, not of the spread.
- **Scale matters.** If variables have different units, PCA on $S$ is dominated by the largest-variance variable. Standardize each column first, which makes PCA act on the correlation matrix.
- `sklearn.decomposition.PCA` does centering and the SVD route for you. Check `explained_variance_ratio_` against the formula in section 28.

---

# Parked (exact question, revisit later)

- Why Householder reflections are more stable than Gram-Schmidt → later, when implementing QR
- Why pivoting is needed for stability, with an example → later, when implementing LU
- Eigenvalues for non-symmetric matrices, and the QR algorithm → later, after ESL
- Why the SVD of $X$ is more stable than the eigendecomposition of $X^TX$ → later, when implementing PCA
- Computing SVD numerically (Golub-Kahan, Jacobi) → later, after ESL
