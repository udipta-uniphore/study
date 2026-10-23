# Scratch: Linear combination vs. inner product

---

## Q: Linear combination of vectors is another vector, am I missing something?

---

## A: No, you're right. Two separate steps:

1. **Linear combination:** takes vectors and returns a **vector**.
2. **Inner product:** takes two vectors and returns a **number**.

**Example in $\mathbb{R}^2$:** let $u = (1, 2)$ and $v = (3, 4)$.

- $u + v = (4, 6)$ is a vector.
- $\langle u, v \rangle = 1\cdot 3 + 2\cdot 4 = 11$ is a number.

**Applied to random variables:** let $U = a^T\mathbf{X}$ and $V = b^T\mathbf{X}$. Each is a **vector** in the space of random variables, formed by a linear combination of the $X_i$. Covariance takes two such vectors and returns a **number**:

$$\text{Cov}(U, V) = \langle U, V \rangle = a^T \Sigma\, b$$

The left side forms the two combinations first, then takes the inner product. The right side gets the same number from $\Sigma$, which stores the inner products of the original $X_i$.

**Gram matrix remark:** $\Sigma$ collects all pairwise inner products $\langle \tilde X_i, \tilde X_j \rangle$ into a grid. It doesn't change the proof; it's just a way to read $\Sigma$.

---

## Q: Why is the proof's covariance a double sum, but the data formula a single sum?

---

## A: The indices mean different things.

- **Data formula:** $s_{xy} = \frac{1}{n-1}\sum_i (x_i-\bar x)(y_i-\bar y)$. Two variables $X$ and $Y$, and $n$ observations. The index $i$ runs over observations, and each term pairs the two values from the same observation. One sum is enough.

- **Proof:** $\sum_i\sum_j a_i b_j (X_i-\mu_i)(X_j-\mu_j)$. Here there are $n$ variables, and the index $i$ runs over which variable. Multiplying two sums forces every variable in $U$ to pair with every variable in $V$, giving all $n^2$ combinations.

The two agree when they describe the same case. Take $a = e_1$ and $b = e_2$, the unit vectors. Then $U = X_1$ and $V = X_2$, and the double sum collapses to the single term
$$\Sigma_{12} = E\big[(X_1-\mu_1)(X_2-\mu_2)\big]$$
Replacing $E$ with an average over observations gives the data formula.

So the single sum is the sample version of one expectation for one pair of variables. The double sum is what you get when you combine many variables at once.

---

## Q: They still look like different formulas. Show more steps that they are equal.

---

## A: Start from the proof's double sum and change one thing per step.

**Step 1: pick two variables.** Let $\mathbf{X} = (X_1, X_2)^T$ and take $a = (1,0)^T$, $b = (0,1)^T$. Then $a^T\mathbf{X} = X_1$ and $b^T\mathbf{X} = X_2$.

**Step 2: collapse the double sum.** The general formula is
$$\text{Cov}(a^T\mathbf{X}, b^T\mathbf{X}) = \sum_i \sum_j a_i\, b_j\, E\big[(X_i-\mu_i)(X_j-\mu_j)\big]$$
Since $a_i = 0$ unless $i = 1$, and $b_j = 0$ unless $j = 2$, only one term survives:
$$\text{Cov}(X_1, X_2) = E\big[(X_1-\mu_1)(X_2-\mu_2)\big]$$

**Step 3: rename.** Write $X = X_1$ and $Y = X_2$:
$$\text{Cov}(X,Y) = E\big[(X-\mu_X)(Y-\mu_Y)\big]$$
This is a single expectation, with no sum.

**Step 4: turn the expectation into an average of observations.** We have $n$ observed pairs $(x_i, y_i)$. The expectation of a function is approximated by the average of that function over observations:
$$E\big[(X-\mu_X)(Y-\mu_Y)\big] \approx \frac{1}{n}\sum_{i=1}^n (x_i-\mu_X)(y_i-\mu_Y)$$
Now there is a single sum over observations.

**Step 5: replace the unknown means with sample means.** We don't know $\mu_X$ and $\mu_Y$, so substitute $\bar x$ and $\bar y$:
$$\frac{1}{n}\sum_{i=1}^n (x_i-\bar x)(y_i-\bar y)$$
This is the plug-in version. Its expected value is $\frac{n-1}{n}\text{Cov}(X,Y)$, which is too small.

**Step 6: correct the bias.** Multiply by $\frac{n}{n-1}$:
$$\frac{n}{n-1}\cdot\frac{1}{n}\sum_{i=1}^n (x_i-\bar x)(y_i-\bar y) = \frac{1}{n-1}\sum_{i=1}^n (x_i-\bar x)(y_i-\bar y)$$

That's exactly the data formula $s_{xy}$.

**Summary of the chain:**
$$\underbrace{\sum_i\sum_j a_i b_j \Sigma_{ij}}_{\text{proof}} \;\to\; \underbrace{\Sigma_{12}}_{\text{one pair}} \;\to\; \underbrace{E[(X-\mu_X)(Y-\mu_Y)]}_{\text{one expectation}} \;\to\; \underbrace{\tfrac{1}{n}\textstyle\sum_i (x_i-\mu_X)(y_i-\mu_Y)}_{\text{average over data}} \;\to\; \underbrace{\tfrac{1}{n-1}\textstyle\sum_i (x_i-\bar x)(y_i-\bar y)}_{s_{xy}}$$

Steps 4 to 6 are where the data formula comes from. Steps 1 to 3 show the proof reduces to the same single-pair covariance.

---

## Q: Are the double sum in the proof and the data formula the same formula?

---

## A: No. They are different sums, and I overstated the link earlier.

| | Proof's double sum | Data formula |
|---|---|---|
| What $i$ runs over | Components of $\mathbf{X}$ (variables) | Sample points (observations) |
| What $j$ runs over | Components of $\mathbf{X}$ (variables) | Not used (one sum) |
| Number of terms | $n^2$, with $n$ = number of variables | $n-1$ after correction, with $n$ = number of observations |
| What it computes | Covariance of two linear combinations, using $\Sigma$ | Sample covariance of two variables from data |

The two are connected, but not by being equal:

1. Set $a = e_1$, $b = e_2$. The double sum becomes one term: $\Sigma_{12} = E[(X_1-\mu_1)(X_2-\mu_2)]$. This is a population quantity.
2. Estimate that expectation from $n$ observations by averaging over sample points, and replace the unknown means with sample means.
3. Correct for bias with $\frac{1}{n-1}$. The result is the data formula.

So the data formula is a sample estimate of one term from the proof. It is not a rewriting of the double sum. Saying the two "agree" or "reduce to" each other was misleading.

---

## Q: Plain version: are there two covariances?

---

## A: No. There is one covariance. Here is the hand check.

**Data** ($x = (1,2,3)$, $y = (2,4,7)$):

- $s_x^2 = 1$
- $s_y^2 = \tfrac{19}{3}$
- $s_{xy} = \tfrac{5}{2}$

Put these in a $2\times 2$ table, called $S$. The diagonal holds the variances, and the off-diagonal holds the covariance:

$$S = \begin{pmatrix} 1 & \tfrac52 \\ \tfrac52 & \tfrac{19}{3} \end{pmatrix}$$

**Check 1: pick $x$ alone.** Use $a = (1,0)$ and $b = (0,1)$. Then $a^T S b = \tfrac52$, which is $s_{xy}$. The matrix gives back the data formula.

**Check 2: pick $x + y$.** Use $a = (1,1)$. Then $a^T S a = 1 + 2\cdot\tfrac52 + \tfrac{19}{3} = \tfrac{37}{3}$.

Now compute it directly. Let $u = x + y = (3, 6, 10)$. Its mean is $\tfrac{19}{3}$, and its sample variance is $\tfrac{37}{3}$. Same number.

**What this means:**

- The proof's formula $a^T\Sigma b$ is the matrix version. $\Sigma$ is the population table, with true values.
- $S$ is the same table filled in with data. Both checks hold exactly for $S$.
- The data formula $s_{xy}$ is just one entry of $S$. The double sum in the proof is not a different formula; it's the same thing written for any pair of linear combinations.
- The earlier "chain" in this file was a shortcut from the population to the sample. It is not an algebra identity.

**One sentence:** Covariance is one idea; $\Sigma$ is the true table, $S$ is the table from data, and $a^T(\cdot)b$ reads any pair of combinations off either table.

---

## Q: Why are there no observations in the proof's double sum, when the data formula sums over observations?

---

## A: The proof is about the population, so it has no observation sum.

- $E[\cdot]$ already averages over all possible outcomes of the random vector. One "draw" is the whole vector, and $E$ accounts for every draw.
- The double sum $\sum_i\sum_j$ runs over the **components** $i, j$ of that one vector.

Observations appear only when you estimate $E$ from data. Each observation $k$ is one full draw of the vector, $\mathbf{x}^{(k)} = (x_1^{(k)}, \ldots, x_n^{(k)})^T$. The sample covariance matrix then sums over observations:
$$S = \frac{1}{m-1}\sum_{k=1}^{m} (\mathbf{x}^{(k)}-\bar{\mathbf{x}})(\mathbf{x}^{(k)}-\bar{\mathbf{x}})^T$$

Here $m$ is the number of observations. Each term is an outer product, which contains all the component-level pairs $(i,j)$ for that observation. So:

- **Population:** double sum over components, expectation over outcomes.
- **Data:** sum over observations $k$ replaces the expectation, and each term contains the component pairs.

The data formula $s_{xy}$ is the two-variable case of this. Each observation contributes one product $(x_k - \bar x)(y_k - \bar y)$.

---

## Q: So the actual data formula is a triple sum, with one step skipped, right?

---

## A: Yes. Writing the sample version of $\sum_i\sum_j a_i b_j \Sigma_{ij}$ directly gives:

$$a^T S\, b = \sum_{i}\sum_{j} a_i\, b_j\, S_{ij}, \qquad S_{ij} = \frac{1}{m-1}\sum_{k=1}^{m}(x_i^{(k)}-\bar x_i)(x_j^{(k)}-\bar x_j)$$

Substituting gives a sum over observations $k$, plus the two component sums $i, j$: a triple sum.

The skipped step is replacing $E$ with an average over observations. That's what introduces the sum over $k$.

The triple sum can collapse into one sum over observations. First form each observation's linear combinations, $u_k = a^T(\mathbf{x}^{(k)}-\bar{\mathbf{x}})$ and $v_k = b^T(\mathbf{x}^{(k)}-\bar{\mathbf{x}})$. Then
$$a^T S\, b = \frac{1}{m-1}\sum_{k=1}^{m} u_k\, v_k$$
This is the data formula applied to the combinations $u$ and $v$.

---

## Q: Are all covariance matrices positive semi-definite?

---

## A: Yes. Every covariance matrix is symmetric and PSD, for the same reason as before:
$$a^T\Sigma a = \text{Var}(a^T\mathbf{X}) \ge 0 \quad \text{for every } a$$

Symmetry comes from $\text{Cov}(X_i, X_j) = \text{Cov}(X_j, X_i)$.

- **Converse holds.** Any symmetric PSD matrix is a covariance matrix. For example, $N(0, \Sigma)$ has covariance $\Sigma$.
- **Not always positive definite.** $\Sigma$ is positive definite only when no nonzero combination has zero variance. If $X_2 = 2X_1$, then $a = (2, -1)$ gives $a^T\Sigma a = 0$, so $\Sigma$ is singular.
- **The sample version is PSD too.** $a^T S a = \frac{1}{m-1}\sum_k (u_k)^2 \ge 0$, where $u_k = a^T(\mathbf{x}^{(k)}-\bar{\mathbf{x}})$.

---

## Q: What is PSD? Write the definition and significance.

---

## A: PSD means **positive semidefinite**.

**Definition.** A real symmetric matrix $M$ is positive semidefinite if
$$a^T M a \ge 0 \quad \text{for every vector } a$$

It is **positive definite (PD)** if $a^T M a > 0$ for every $a \neq 0$. The "semi" allows zero.

The name comes from the scalar case: $a^2 \ge 0$ for every real number, and $M$ plays the role of a "nonnegative number." The quadratic form $a^T M a$ is the matrix version.

**Equivalent tests** (for a symmetric matrix $M$):
- All eigenvalues are $\ge 0$ (PSD) or $> 0$ (PD).
- $M = B^T B$ for some matrix $B$.

**Significance:**

- **Covariance is PSD.** Variance can't be negative, and $a^T\Sigma a = \text{Var}(a^T\mathbf{X})$ is a variance. That's why every covariance matrix is PSD.
- **Eigenvalues are variances.** In the eigenbasis, $\Sigma = U\Lambda U^T$ with $\lambda_i \ge 0$. Each $\lambda_i$ is the variance along a principal axis, so PSD guarantees no negative variances in any direction.
- **Square roots exist.** A PSD matrix has a PSD square root, $\Sigma^{1/2}$, which is what sampling ($X = \mu + \Sigma^{1/2}Z$) and whitening use.
- **Shapes are ellipsoids.** A PSD quadratic form $a^T M a = c$ defines an ellipsoid (or a degenerate one, if $M$ is singular). This is the tilted-ellipse picture of the Gaussian.
- **PD is needed for inverses.** The Gaussian density uses $\Sigma^{-1}$, which exists only when $\Sigma$ is PD. A singular $\Sigma$ (like $X_2 = 2X_1$) means the density has no ordinary form.

---

## Q: What is the proof that a covariance matrix is PSD? And what is the condition for being PSD?

---

## A:

**Proof.** Let $\mu = E[\mathbf{X}]$ and $Z = a^T(\mathbf{X}-\mu)$, a centered random variable. Then
$$Z^2 = a^T(\mathbf{X}-\mu)(\mathbf{X}-\mu)^T a$$
Taking expectations and pulling the constant vectors $a^T$ and $a$ outside:
$$E[Z^2] = a^T\, E\big[(\mathbf{X}-\mu)(\mathbf{X}-\mu)^T\big]\, a = a^T\Sigma a$$
So $a^T\Sigma a = E[Z^2] \ge 0$, because a square is never negative and the expectation of a nonnegative quantity is nonnegative. Since $Z$ is centered, $E[Z^2] = \text{Var}(Z)$. Symmetry holds because $\text{Cov}(X_i, X_j) = \text{Cov}(X_j, X_i)$.

**Condition for a covariance matrix.** Nothing extra is needed. The proof covers every vector $a$, so PSD is automatic.

**General condition for a symmetric matrix $M$ to be PSD:**
$$a^T M a \ge 0 \quad \text{for all } a$$

Equivalent checks:
- All eigenvalues are $\ge 0$. In an eigenbasis, $a^T M a = \sum_i \lambda_i y_i^2$, which is nonnegative for every $y$ exactly when every $\lambda_i \ge 0$.
- $M = B^T B$ for some matrix $B$, since then $a^T M a = \|Ba\|^2 \ge 0$.

---

---
