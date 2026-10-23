# Stat 101: Variance, Covariance, Multivariate Gaussian

---

## 1. Mean, variance, covariance

- Mean: $\mu = E[X]$
- Variance: $\text{Var}(X) = E[(X-\mu)^2] = E[X^2] - \mu^2$
- Covariance: $\text{Cov}(X,Y) = E[(X-\mu_X)(Y-\mu_Y)]$
- $\text{Cov}(X,X) = \text{Var}(X)$

Positive covariance: $X$ and $Y$ tend to be above (or below) their means together.

---

## 2. Covariance from data

For pairs $(x_i, y_i)$, $i=1..n$:

$$s_{xy} = \frac{1}{n-1}\sum_{i}(x_i-\bar x)(y_i-\bar y) = \frac{1}{n-1}\Big(\sum_i x_i y_i - n\bar x\bar y\Big)$$

$$s_x^2 = \frac{1}{n-1}\sum_i (x_i-\bar x)^2$$

**Example:** $x=(1,2,3)$, $y=(2,4,7)$, $\bar x=2$, $\bar y=\tfrac{13}{3}$.
- Sum of products of deviations $= 5$, so $s_{xy} = 2.5$
- $s_x^2 = 1$

**Why $n-1$:** the deviations sum to zero, so only $n-1$ are free. Also, $E[\sum(x_i-\bar x)^2] = (n-1)\sigma^2$, so dividing by $n-1$ makes $E[s_x^2]=\sigma^2$ (unbiased).

**Derivation of $E[\sum(x_i-\bar x)^2] = (n-1)\sigma^2$:**

1. Write $x_i - \bar x = (x_i - \mu) - (\bar x - \mu)$ and square and sum:
$$\sum_i (x_i-\bar x)^2 = \sum_i (x_i-\mu)^2 - 2(\bar x-\mu)\sum_i (x_i-\mu) + n(\bar x-\mu)^2$$

2. Since $\sum_i (x_i-\mu) = n(\bar x-\mu)$, the middle term is $-2n(\bar x-\mu)^2$. So
$$\sum_i (x_i-\bar x)^2 = \sum_i (x_i-\mu)^2 - n(\bar x-\mu)^2$$

3. Expectations:
   - $E\big[\sum_i (x_i-\mu)^2\big] = n\sigma^2$, since each term has expectation $\sigma^2$.
   - $\bar x = \tfrac{1}{n}\sum_i x_i$ with independent $x_i$, so $E[(\bar x-\mu)^2] = \text{Var}(\bar x) = \tfrac{1}{n^2}\cdot n\sigma^2 = \tfrac{\sigma^2}{n}$.

4. Therefore
$$E\Big[\sum_i (x_i-\bar x)^2\Big] = n\sigma^2 - n\cdot\frac{\sigma^2}{n} = (n-1)\sigma^2$$

Dividing by $n$ instead would underestimate the variance on average.

---

## 3. Covariance matrix

Stack $\mathbf X = (X_1,\dots,X_n)^T$ with mean $\mu$:

$$\Sigma = E\big[(\mathbf X-\mu)(\mathbf X-\mu)^T\big], \qquad \Sigma_{ij} = \text{Cov}(X_i,X_j)$$

Diagonal = variances, off-diagonal = covariances.

**Linear combinations.** For a fixed $a \in \mathbb{R}^n$, $a^T\mathbf X = \sum_i a_i X_i$ is a random variable. Covariance is linear in each argument, so

$$\text{Cov}(a^T\mathbf X,\ b^T\mathbf X) = a^T\Sigma\, b, \qquad \text{Var}(a^T\mathbf X) = a^T\Sigma\, a$$

$a$ lives in coefficient space $\mathbb{R}^n$. The $X_i$ are random variables, which form their own vector space.

---

### Proof of $\text{Cov}(a^T\mathbf{X}, b^T\mathbf{X}) = a^T\Sigma b$

Let $\mu = E[\mathbf{X}]$, and set $U = a^T\mathbf{X}$, $V = b^T\mathbf{X}$.

**Step 1: center $U$ and $V$.** Expectation is linear, so $E[U] = a^T\mu$ and $E[V] = b^T\mu$. Subtracting,
$$U - E[U] = a^T\mathbf{X} - a^T\mu = \sum_i a_i (X_i - \mu_i)$$
and similarly $V - E[V] = \sum_j b_j (X_j - \mu_j)$.

**Step 2: write the covariance as an expectation.**
$$\text{Cov}(U,V) = E\big[(U-E U)(V-E V)\big] = E\Big[\Big(\sum_i a_i (X_i-\mu_i)\Big)\Big(\sum_j b_j (X_j-\mu_j)\Big)\Big]$$

**Step 3: multiply out the product of sums.** Each term in the first sum multiplies each term in the second:
$$\Big(\sum_i a_i (X_i-\mu_i)\Big)\Big(\sum_j b_j (X_j-\mu_j)\Big) = \sum_i \sum_j a_i b_j (X_i-\mu_i)(X_j-\mu_j)$$

**Step 4: pull $E$ inside the sums.** Expectation is linear, and $a_i, b_j$ are fixed numbers:
$$\text{Cov}(U,V) = \sum_i \sum_j a_i b_j\, E\big[(X_i-\mu_i)(X_j-\mu_j)\big]$$

**Step 5: recognize the entries of $\Sigma$.** By definition, $E[(X_i-\mu_i)(X_j-\mu_j)] = \Sigma_{ij}$, so
$$\text{Cov}(U,V) = \sum_i\sum_j a_i\,\Sigma_{ij}\,b_j = a^T\Sigma\, b$$

The last equality is matrix multiplication: $\Sigma b$ is a column vector whose $i$-th entry is $\sum_j \Sigma_{ij} b_j$, and $a^T$ dots that with $a$.

**Setting $b = a$** gives the variance, since $\text{Cov}(U,U) = \text{Var}(U)$:
$$\text{Var}(a^T\mathbf{X}) = a^T\Sigma\, a$$

**Check with $n=2$.** Let $\Sigma = \begin{pmatrix}\sigma_1^2 & \rho\sigma_1\sigma_2\\ \rho\sigma_1\sigma_2 & \sigma_2^2\end{pmatrix}$.

- $a=(1,0)$, $b=(0,1)$: $a^T\Sigma b = \Sigma_{12} = \text{Cov}(X_1,X_2)$. ✓
- $a=(1,1)$: $\text{Var}(X_1+X_2) = \sigma_1^2 + 2\rho\sigma_1\sigma_2 + \sigma_2^2$, which matches $a^T\Sigma a$. ✓

**Why "bilinear."** The result is linear in $a$ when $b$ is fixed, and linear in $b$ when $a$ is fixed. That is exactly the bilinear-form definition from section 4.

---

### Transformation rule: $\text{Cov}(A\mathbf{X}) = A\Sigma A^T$

**Purpose.** $\Sigma$ describes the spread and tilt of your data in its current coordinates. Often you want different coordinates: rotated, rescaled, or made of combinations of the original variables. The transformation rule tells you the new covariance directly from $\Sigma$ and $A$, without going back to the raw data. It lets you plan a change of coordinates, check what it does, and reuse one estimate of $\Sigma$ for many questions.

**Why it's true.** Let $A$ be a fixed $m \times n$ matrix and $\mathbf{Y} = A\mathbf{X}$. Each new variable is a linear combination of the originals: $Y_i = r_i^T\mathbf{X}$, where $r_i^T$ is row $i$ of $A$. The covariance of two such combinations is $r_i^T\Sigma\, r_j$ by the proof above. That is exactly entry $(i,j)$ of $A\Sigma A^T$. So
$$\text{Cov}(\mathbf{Y}) = A\,\Sigma\,A^T$$

Dimensions: $(m\times n)(n\times n)(n\times m) = m\times m$. The result is a covariance for the $m$ new variables.

**How it's used, in four steps:**

1. Estimate $\Sigma$ from data (the sample covariance matrix $S$).
2. Choose $A$ to get the structure you want: uncorrelated, unit variance, lower dimension, or a specific combination.
3. Compute $A\Sigma A^T$. No new data needed.
4. Read off what you need: which variables are uncorrelated, how much spread each one has, or how risky a combination is.

*Common mistake:* $A\Sigma A^T$ applies when $A$ acts on the variables ($\mathbf{Y} = A\mathbf{X}$). If your data is stored with observations as rows, the formula takes a different form, so check which convention you're in.

**Application 1: PCA.** Purpose: find the directions where the data varies most, to reduce dimensions or visualize.

- Diagonalize $\Sigma = U\Lambda U^T$, where $U$ is orthogonal and $\Lambda$ is diagonal with the eigenvalues.
- Take $A = U^T$ on the centered data:
$$\text{Cov}(U^T(\mathbf{X}-\mu)) = U^T\Sigma U = \Lambda$$
- The new coordinates are uncorrelated, and their variances are the eigenvalues $\lambda_i$.
- Keep the $k$ largest. The fraction of spread retained is $\dfrac{\sum_{i \le k}\lambda_i}{\sum_i \lambda_i}$.

*Example:* eigenvalues $5, 2, 0.5$. Keeping the first two retains $\dfrac{7}{7.5} \approx 93\%$ of the total variance.

**Application 2: Whitening.** Purpose: remove correlations and scale every direction to unit variance, so distances become ordinary Euclidean distances. Common preprocessing before fitting statistical or machine learning models.

- Take $A = \Sigma^{-1/2}$ on centered data:
$$\text{Cov}(\Sigma^{-1/2}(\mathbf{X}-\mu)) = \Sigma^{-1/2}\Sigma\,\Sigma^{-1/2} = I$$
- The Mahalanobis distance from section 5 becomes the plain Euclidean distance in the whitened coordinates.

*Reverse direction:* to simulate correlated data, start with independent standard normals $Z$ and set $X = \mu + \Sigma^{1/2}Z$. The rule confirms $\text{Cov}(X) = \Sigma$.

**Application 3: Gaussian transformations.** Purpose: predict the distribution of a linear summary of Gaussian data without simulating it.

- If $\mathbf{X} \sim N(\mu, \Sigma)$ and $\mathbf{Y} = A\mathbf{X} + b$, then
$$\mathbf{Y} \sim N(A\mu + b,\ A\Sigma A^T)$$
- The shape stays Gaussian, and only the mean and covariance change.
- *Kalman filter:* the prediction step propagates uncertainty as $P_{k} = F P_{k-1} F^T + Q$, which is the same rule with an added noise term.

**Application 4: Portfolio risk.** Let $w$ be a vector of asset weights and $\mathbf{R}$ the vector of asset returns with covariance $\Sigma$. The portfolio return $w^T\mathbf{R}$ has variance
$$\text{Var}(w^T\mathbf{R}) = w^T\Sigma w$$
This is the quadratic form from section 4. Minimizing it over $w$ is the core of mean-variance portfolio optimization.

**Application 1: PCA (principal component analysis).** Diagonalize $\Sigma = U\Lambda U^T$, with $U$ orthogonal and $\Lambda$ diagonal. Take $A = U^T$:
$$\text{Cov}(U^T(\mathbf{X}-\mu)) = U^T\Sigma U = \Lambda$$
The new coordinates are uncorrelated, and their variances are the eigenvalues $\lambda_i$. To reduce dimension, keep the $k$ largest $\lambda_i$. The fraction of variance kept is $\frac{\sum_{i\le k}\lambda_i}{\sum_i \lambda_i}$.

**Application 2: Whitening.** Take $A = \Sigma^{-1/2}$ (applied to centered data):
$$\text{Cov}(\Sigma^{-1/2}(\mathbf{X}-\mu)) = \Sigma^{-1/2}\Sigma\Sigma^{-1/2} = I$$
The variables become uncorrelated with unit variance. In these coordinates, the Mahalanobis distance from earlier is just the ordinary Euclidean distance. This is standard preprocessing before many statistical and ML methods.

**Application 3: Gaussian transformations.** If $\mathbf{X} \sim N(\mu, \Sigma)$ and $\mathbf{Y} = A\mathbf{X} + b$, then
$$\mathbf{Y} \sim N(A\mu + b,\ A\Sigma A^T)$$
A linear map keeps the Gaussian shape, and the transformation rule gives the new covariance. The same rule gives portfolio risk: if $w$ is a vector of asset weights and $\Sigma$ is the covariance of asset returns, then $\text{Var}(w^T\mathbf{R}) = w^T\Sigma w$, which is the quadratic form from section 4.

---

## 4. Definitions

- **Bilinear form:** $B: V\times V\to\mathbb{R}$, linear in each slot. Example: $B(x,y)=x^TAy$.
- **Symmetric:** $B(x,y)=B(y,x)$, i.e. $A^T=A$.
- **Quadratic form:** $Q(x)=B(x,x)$. Example: $Q(x)=x^TAx$. Not linear: $Q(2x)=4Q(x)$.
- **Positive semidefinite:** symmetric $\Sigma$ with $a^T\Sigma a\ge 0$ for all $a$. Positive definite: $>0$ for all $a\ne 0$.

$\Sigma$ is a symmetric bilinear form, and its quadratic form is a variance, so it is always positive semidefinite.

---

## 4b. Where the Gaussian comes from

### Short history

- **1733, de Moivre.** Approximates binomial probabilities for many coin flips with a bell-shaped curve. This is the first appearance of the normal curve.
- **1809, Gauss.** Studies errors in astronomical measurements. He shows that if the sample mean is the most probable estimate and errors are symmetric, the error density must be Gaussian. This is the origin of the name "Gaussian."
- **1810s, Laplace.** Proves the central limit theorem in early form: sums of many small independent effects are approximately normal. This is why the Gaussian shows up so often.
- **Late 1800s, Galton, Pearson.** Apply the normal curve to human traits like height and develop correlation and covariance as the tools for relating variables.
- **Modern view, Jaynes-style.** If you know only the mean and covariance, the Gaussian is the distribution with the largest entropy (least extra assumptions) among all with those moments. That's a reason to use it, not a derivation from data.

### Derivation of the multivariate density

Goal: show that the density of a vector with mean $\mu$ and covariance $\Sigma$ is
$$f(x) = \frac{1}{(2\pi)^{n/2}\sqrt{\det\Sigma}}\exp\!\Big(-\tfrac12 (x-\mu)^T\Sigma^{-1}(x-\mu)\Big)$$

**Step 1: start from independent standard normals.** Let $Z = (Z_1, \ldots, Z_n)^T$ with $Z_i \sim N(0,1)$ independent. Independence multiplies densities:
$$f_Z(z) = \prod_{i=1}^n \frac{1}{\sqrt{2\pi}}e^{-z_i^2/2} = \frac{1}{(2\pi)^{n/2}}\exp\!\Big(-\tfrac12 z^T z\Big)$$

The constant comes from $\int e^{-z^2/2}\,dz = \sqrt{2\pi}$. To check: square the integral, $I^2 = \iint e^{-(x^2+y^2)/2}\,dx\,dy$, switch to polar coordinates: $\int_0^{2\pi}\int_0^\infty e^{-r^2/2}\,r\,dr\,d\theta = 2\pi$, so $I = \sqrt{2\pi}$.

**Step 2: build a vector with covariance $\Sigma$.** Let $X = \mu + AZ$ for a matrix $A$. Its covariance is
$$\text{Cov}(X) = A\,I\,A^T = AA^T$$
So we need $AA^T = \Sigma$. This is where PSD matters: write $\Sigma = U\Lambda U^T$ with $\lambda_i \ge 0$, and take $A = U\Lambda^{1/2}U^T$. Then $AA^T = \Sigma$. The square roots exist only because every $\lambda_i \ge 0$.

**Step 3: change of variables.** If $A$ is invertible (which needs $\Sigma$ positive definite), the density of $X$ is
$$f_X(x) = \frac{f_Z(A^{-1}(x-\mu))}{|\det A|}$$
Reason: probability is conserved, and the map $z \mapsto \mu + Az$ scales volumes by $|\det A|$, so densities are divided by that factor.

**Step 4: substitute.** Let $z = A^{-1}(x-\mu)$.

- Exponent: $z^T z = (x-\mu)^T (A^{-1})^T A^{-1} (x-\mu) = (x-\mu)^T (AA^T)^{-1}(x-\mu) = (x-\mu)^T\Sigma^{-1}(x-\mu)$
- Determinant: $|\det A| = \sqrt{\det(A)\det(A^T)} = \sqrt{\det\Sigma}$

Putting these into Steps 1 and 3 gives the formula above. $\blacksquare$

**What each piece means:**
- The exponent is the squared length of $z$, the standardized coordinates, so it's the squared Mahalanobis distance.
- $\sqrt{\det\Sigma}$ is the volume scaling, which normalizes the density.
- $A$ is the stretch-and-rotate that turns independent noise into correlated noise.

**When $\Sigma$ is singular** (like $X_2 = 2X_1$), $A$ isn't invertible, $\det\Sigma = 0$, and the distribution lives on a lower-dimensional subspace. The formula then doesn't apply as written.

---

### Entropy and Gibbs' inequality

**Where the entropy formula comes from.** Shannon (1948) wanted a measure $H$ of uncertainty. Start with surprise:

1. **Surprise of one outcome.** An outcome with probability $p$ carries surprise $I(p)$. Rarer outcomes are more surprising, so $I$ decreases in $p$.
2. **Independent surprises add.** Two independent events with probabilities $p$ and $q$ have joint probability $pq$, so $I(pq) = I(p) + I(q)$.
3. **Only the logarithm works.** The only decreasing function with that property is $I(p) = -\log p$, up to a constant. The base sets the unit (base 2 gives bits).

   *Proof.* Let $I$ be decreasing on $(0,1]$ with $I(pq) = I(p)+I(q)$. Define $g(t) = I(e^{-t})$ for $t \ge 0$.
   - **Additive:** $g(s+t) = I(e^{-s}e^{-t}) = I(e^{-s}) + I(e^{-t}) = g(s)+g(t)$.
   - **Monotone:** $e^{-t}$ decreases in $t$ and $I$ is decreasing, so $g$ is increasing.

   A monotone additive function on $\mathbb{R}$ is linear, so $g(t) = ct$. Since $g$ is increasing and nonconstant, $c>0$. Then $I(p) = g(-\log p) = -c\log p$. Without monotonicity, additive solutions that are not linear exist (via a Hamel basis), so the decreasing assumption is needed.

   *Converse (the formula does satisfy both conditions).* For $c>0$, $-c\log(pq) = -c\log p - c\log q$, so the additivity holds. And $\log$ is increasing with $-c<0$, so $-c\log p$ is decreasing. Together with the proof above, $-c\log p$ is exactly the family of functions with both properties.
4. **Average the surprise.** Entropy is expected surprise:
$$H = -\sum_x p(x)\log p(x)$$

For continuous distributions, the sum becomes an integral, $H[p] = -\int p\log p$. This is differential entropy. It changes under coordinate changes, but the Gaussian argument compares entropies on the same space, so it still holds.

**KL divergence, intuitively.** Suppose the world follows $p$, but you model it with $q$. You expect surprise $-\log q(x)$ for each outcome, but the outcome actually comes with the true surprise under $p$. On average, the gap is the cost of your wrong model:
$$\text{KL}(p\|q) = E_p[-\log q] - E_p[-\log p] = \sum_x p(x)\log\frac{p(x)}{q(x)}$$
So KL is cross-entropy minus entropy.

**Example (coin).** True coin $p = (0.5, 0.5)$. Model $q = (0.9, 0.1)$.
$$\text{KL}(p\|q) = 0.5\log\frac{0.5}{0.9} + 0.5\log\frac{0.5}{0.1} \approx 0.51 \text{ nats} \approx 0.74 \text{ bits}$$

**Properties:**
- Always $\ge 0$, and $0$ only when $p = q$ (Gibbs' inequality, below).
- Not symmetric: swapping roles gives $\text{KL}(q\|p) \approx 0.37$ nats in the example above, which differs from $0.51$. So KL is not a distance.

**KL divergence (formula).**
$$\text{KL}(p\|q) = \sum_x p(x)\log\frac{p(x)}{q(x)}$$

**Gibbs' inequality:** $\text{KL}(p\|q) \ge 0$, with equality only when $p = q$.

*Proof.* Use $\log t \le t - 1$ for every $t > 0$, with $t = q(x)/p(x)$:
$$\sum_x p(x)\log\frac{q(x)}{p(x)} \le \sum_x p(x)\left(\frac{q(x)}{p(x)} - 1\right) = \sum_x q(x) - \sum_x p(x) = 0$$
Flipping the sign gives $\text{KL}(p\|q) \ge 0$. Equality requires $q(x)/p(x) = 1$ wherever $p(x) > 0$, so $p = q$.

**Why this matters for the Gaussian proof.** For any $q$ with the same moments as the Gaussian $g$, Gibbs' inequality gives $-\int q\log g \ge H[q]$. Because $\log g$ is a quadratic, and $q$ and $g$ share the same second moments, the cross term equals $H[g]$. That's what forces $H[q] \le H[g]$.

---

### Maximum entropy derivation (Jaynes)

**Problem.** Among all densities $p$ on $\mathbb{R}^n$ with
$$\int p = 1, \qquad \int x\,p(x)\,dx = \mu, \qquad \int (x-\mu)(x-\mu)^T p(x)\,dx = \Sigma$$
find the one with largest entropy
$$H[p] = -\int p(x)\log p(x)\,dx$$

**Step 1: Lagrange multipliers (finds the candidate).** Introduce multipliers $\lambda_0 \in \mathbb{R}$, $\lambda \in \mathbb{R}^n$, and a symmetric matrix $M$. Setting the functional derivative of the Lagrangian with respect to $p(x)$ to zero gives
$$-\log p(x) - 1 - \lambda_0 - \lambda^T x - (x-\mu)^T M (x-\mu) = 0$$
so
$$p(x) = \exp\!\big(-1-\lambda_0 - \lambda^T x - (x-\mu)^T M(x-\mu)\big)$$

**Step 2: recognize a Gaussian.** Expand $(x-\mu)^TM(x-\mu) = x^TMx - 2\mu^TMx + \mu^TM\mu$. The exponent is a quadratic in $x$. Set $K = 2M$ and $b = 2M\mu - \lambda$. Then the exponent is $-\tfrac12 x^T K x + b^T x + \text{const}$, and completing the square gives
$$p(x) \propto \exp\!\big(-\tfrac12 (x-m)^T K (x-m)\big), \qquad m = K^{-1}b$$
This is a Gaussian with mean $m$ and covariance $K^{-1}$.

**Step 3: match the constraints.** The constraints require mean $\mu$ and covariance $\Sigma$, so $m = \mu$ and $K^{-1} = \Sigma$. Hence $K = \Sigma^{-1}$, so $M = \tfrac12\Sigma^{-1}$. Then $b = K\mu = \Sigma^{-1}\mu$, and $b = 2M\mu - \lambda = \Sigma^{-1}\mu - \lambda$ gives $\lambda = 0$. The multiplier $\lambda_0$ is set by normalization. The candidate is
$$p^*(x) = \frac{1}{(2\pi)^{n/2}\sqrt{\det\Sigma}}\exp\!\Big(-\tfrac12 (x-\mu)^T\Sigma^{-1}(x-\mu)\Big)$$
which is the multivariate Gaussian. Notice $\Sigma$ enters the exponent through the second-moment constraint.

**Step 4: prove it is the maximum (Gibbs inequality).** Step 1 only shows the Gaussian is a stationary point. Let $g = p^*$ and let $q$ be any density satisfying the same constraints. Then
$$0 \le \text{KL}(q\,\|\,g) = \int q\log\frac{q}{g} = -H[q] - \int q\log g$$
The Kullback-Leibler divergence $\text{KL}(q\|g)$ is nonnegative, with equality only when $q = g$; this is Gibbs' inequality, which follows from $\log t \le t - 1$.

Now $\log g$ is a constant plus a quadratic:
$$\log g(x) = -\tfrac12\log\big((2\pi)^n\det\Sigma\big) - \tfrac12 (x-\mu)^T\Sigma^{-1}(x-\mu)$$
Under $q$, the quadratic term has expectation
$$E_q\big[(x-\mu)^T\Sigma^{-1}(x-\mu)\big] = \text{tr}\big(\Sigma^{-1}\,E_q[(x-\mu)(x-\mu)^T]\big) = \text{tr}(\Sigma^{-1}\Sigma) = n$$
The same holds for $g$. So $\int q\log g = \int g\log g$, and therefore
$$0 \le -H[q] + H[g] \quad\Longrightarrow\quad H[q] \le H[g]$$
with equality only when $q = g$.

So the Gaussian is the unique maximizer. Its entropy is
$$H[g] = \tfrac12\log\big((2\pi e)^n\det\Sigma\big)$$
For $n = 1$ this is $\tfrac12\log(2\pi e\sigma^2)$.

**Interpretation (Jaynes).** Any other distribution with the same mean and covariance assumes something extra about the shape. The Gaussian is the distribution that assumes nothing beyond those two moments. It's a rule for assigning probabilities under limited information, not a claim that data are Gaussian.

---

## 5. Multivariate Gaussian

$$f(x) = \frac{1}{\sqrt{(2\pi)^n\det\Sigma}}\exp\!\Big(-\tfrac12 (x-\mu)^T\Sigma^{-1}(x-\mu)\Big)$$

- $\mu$: center.
- $(x-\mu)^T\Sigma^{-1}(x-\mu)$: squared Mahalanobis distance, distance in units of spread. Level sets are ellipses.
- $\Sigma^{-1}$: large variance in a direction makes distance there count less.
- $\det\Sigma$: volume scaling, so the density integrates to 1.
- **1D:** $\Sigma=\sigma^2$ gives the bell curve.
- The Gaussian is nonlinear (exponential of a quadratic), so it is not a multilinear map. It is built from a bilinear form.

**Sampling:** $X = \mu + \Sigma^{1/2}Z$ with $Z\sim N(0,I)$ has covariance $\Sigma$. So $\Sigma$ stretches independent noise into correlated noise.

---

## 6. Why it looked "codependent"

- The inputs $x_1, x_2$ are independent coordinates.
- The level sets are tilted ellipses, so the function couples them.
- Cross terms in $x^T\Sigma^{-1}x$ are the coupling.
- Eigendecomposition $\Sigma = U\Lambda U^T$ gives principal axes. In eigen-coordinates $y$, $Q=\sum_i \lambda_i^{-1}y_i^2$ has no cross terms.
- Whitening $y=\Sigma^{-1/2}(x-\mu)$ gives identity covariance. For jointly Gaussian variables, uncorrelated means independent.

**Caveat:** uncorrelated implies independent only for jointly Gaussian variables. "Independent" depends on coordinates; the level sets are coordinate-free.

---

## 7. Population vs. sample

**Vocabulary: parameter, statistic, estimator, estimate**

| Term | Meaning | Example |
|---|---|---|
| **Parameter** | True fixed quantity of the population (usually unknown) | $\mu$, $\sigma^2$, $\theta$ |
| **Statistic** | Any quantity computed from the sample | $\bar x$, $s^2$, the median |
| **Estimator** | A statistic used to estimate a parameter. A rule applied to random data, so itself a random variable | $\hat\theta = \bar X = \frac{1}{n}\sum_i X_i$ (capital $X$, before observing data) |
| **Estimate** | The number an estimator gives on one dataset | $\bar x = 3.2$ (lowercase $x$, observed data) |

Notes:
- "Parameter ↔ population" and "statistic ↔ sample" is a shorthand. A statistic is any function of the data; an estimator is a statistic used for a specific parameter.
- Every estimator is a statistic, but not every statistic is an estimator. The distinction is about use, not form. Examples of statistics that are not estimators:
  - **Sample range** (max minus min): describes spread, with no target parameter.
  - **Test statistic** $t = \frac{\bar x - \mu_0}{s/\sqrt{n}}$: used to test a hypothesis about $\mu$, not to estimate it.
  - **Sample size** $n$, or a count of observations above a threshold: a summary with no target parameter.
- "Sample parameter" is not standard. The sample version of a parameter is an estimator, and its value on your data is the estimate.
- Capital $X$ marks random variables; lowercase $x$ marks observed values.
- **Population vs. distribution:** the distribution is the model of how values are spread (e.g. $N(\mu,\sigma^2)$). The population is the group being sampled. Prefer "distribution" from here on.

- **Population:** the whole group or distribution you care about. Its summary numbers are **parameters**, fixed and usually unknown: $\mu$, $\sigma^2$.
- **Sample:** the subset you actually observe. Its summary numbers are **statistics**, computed from data and random: $\bar x$, $s^2$.

| | Population | Sample |
|---|---|---|
| What it is | Whole distribution or group | Observed subset |
| Mean | $\mu = E[X]$ | $\bar x = \frac{1}{n}\sum_i x_i$ |
| Variance | $\sigma^2 = E[(X-\mu)^2]$ | $s^2 = \frac{1}{n-1}\sum_i (x_i-\bar x)^2$ |
| Fixed or random? | Fixed (usually unknown) | Random (changes with sample) |
| Symbol style | Greek | Latin |

**Link:** a sample statistic estimates its population parameter. $E[\bar x] = \mu$, and $E[s^2] = \sigma^2$.

**Example:** a fair die has population mean $\mu = 3.5$. Ten rolls might give $\bar x = 3.2$. The die is the population; the ten rolls are the sample.

---

## 8. Variance of the sample mean

Since $E[\bar x] = \mu$:
$$E[(\bar x-\mu)^2] = \text{Var}(\bar x)$$

Scaling: $\text{Var}(cZ) = c^2\,\text{Var}(Z)$, so
$$\text{Var}(\bar x) = \frac{1}{n^2}\,\text{Var}\Big(\sum_i x_i\Big)$$

Independence: variances add when covariances are zero, so
$$\text{Var}\Big(\sum_i x_i\Big) = \sum_i \text{Var}(x_i) = n\sigma^2$$

Therefore
$$\text{Var}(\bar x) = \frac{1}{n^2}\cdot n\sigma^2 = \frac{\sigma^2}{n}$$

Averaging $n$ independent values cuts the variance by a factor of $n$, so the sample mean's spread shrinks as the sample grows.

---

## 9. Bias and unbiasedness

**Bias** of an estimator $\hat\theta$ for a parameter $\theta$:
$$\text{Bias}(\hat\theta) = E[\hat\theta] - \theta$$

An estimator is **unbiased** if $\text{Bias} = 0$: it hits the true value on average over repeated samples. A single sample can still miss.

**Proof that $s^2$ is unbiased.** Assume $x_1, \ldots, x_n$ are independent with mean $\mu$ and variance $\sigma^2$.

1. $E[s^2] = \dfrac{1}{n-1}\,E\Big[\sum_i (x_i-\bar x)^2\Big]$

2. Rewrite around $\mu$:
$$\sum_i (x_i-\bar x)^2 = \sum_i (x_i-\mu)^2 - n(\bar x-\mu)^2$$

3. Expectations:
   - $E\big[\sum_i (x_i-\mu)^2\big] = n\sigma^2$
   - $E\big[(\bar x-\mu)^2\big] = \dfrac{\sigma^2}{n}$, so $E\big[n(\bar x-\mu)^2\big] = \sigma^2$

4. Subtract: $E\Big[\sum_i (x_i-\bar x)^2\Big] = n\sigma^2 - \sigma^2 = (n-1)\sigma^2$

5. Divide by $n-1$: $E[s^2] = \sigma^2$

Therefore $\text{Bias}(s^2) = E[s^2] - \sigma^2 = 0$.

---

## 10. Bias-variance tradeoff

**Mean squared error** measures how far an estimator typically lands from the truth:
$$\text{MSE}(\hat\theta) = E\big[(\hat\theta-\theta)^2\big]$$

**Decomposition.** Let $m = E[\hat\theta]$. Then $\hat\theta - \theta = (\hat\theta - m) + (m - \theta)$. Square and take expectations; the cross term vanishes because $E[\hat\theta - m] = 0$:
$$\text{MSE}(\hat\theta) = \text{Var}(\hat\theta) + \text{Bias}(\hat\theta)^2$$

So total error = spread + systematic error. An unbiased estimator has MSE equal to its variance, but a biased one can still have lower MSE if its variance is much smaller.

**Example: estimating $\sigma^2$ with divisor $c$.** Use $S_c = \frac{1}{c}\sum_i (x_i-\bar x)^2$ for normal data. With $m = n-1$, the sum divided by $\sigma^2$ follows $\chi^2_m$, which has mean $m$ and variance $2m$. This gives:

- Bias $= \sigma^2\dfrac{m-c}{c}$
- Variance $= \sigma^4\dfrac{2m}{c^2}$
- MSE $= \sigma^4\dfrac{2m + (m-c)^2}{c^2}$

Minimizing over $c$ gives $c = m + 2 = n+1$.

| Divisor | Bias | Variance | Behavior |
|---|---|---|---|
| $n-1$ | 0 | larger | Unbiased |
| $n$ | negative | smaller | Biased low |
| $n+1$ | negative | smallest | Lowest MSE for normal data |

**Concrete check, $n=2$** (in units of $\sigma^4$):
- Divisor 1 ($n-1$): MSE $= 2$
- Divisor 2 ($n$): MSE $= 0.75$
- Divisor 3 ($n+1$): MSE $\approx 0.67$

So for $n=2$ the "unbiased" choice has the worst error.

**Why it matters.** Unbiasedness isn't automatically the goal. Reducing variance can be worth a little bias. The same tradeoff appears in regression and machine learning: simple models have high bias and low variance (underfitting), and complex models have low bias and high variance (overfitting).

---

## 11. Picturing covariance, and why it feels unreliable

**Picture.** Each data point $(x_i, y_i)$ is a dot. Put the center at $(\bar x, \bar y)$. Each dot's term $(x_i-\bar x)(y_i-\bar y)$ is the signed area of the rectangle from the center to that dot:

- Dots in the top-right or bottom-left quadrant: positive area.
- Dots in the top-left or bottom-right quadrant: negative area.

Covariance is the average of these signed areas. It measures the overall **tilt** of the cloud: positive if the cloud runs up-right, negative if down-right, near zero if the dots spread evenly across all four quadrants.

So covariance is a summary like mean and variance, but of **how two variables move together** instead of where one variable sits or how spread out it is.

**Outliers.** Covariance is as sensitive to outliers as variance is. One far-off point in the top-right corner can make the covariance look strong even if the rest of the cloud is flat. Look at the scatter plot before trusting the number.

**Units.** Covariance has units of $x$ times $y$, so its size depends on the scale of the measurements. Correlation fixes this:
$$\rho = \frac{\text{Cov}(X,Y)}{\sigma_X\,\sigma_Y} \in [-1, 1]$$
Multiplying $X$ by 1000 changes the covariance by 1000 but leaves $\rho$ unchanged.

**The real limitation: only linear association.** Covariance measures linear tilt, so it can miss strong nonlinear relationships entirely.

Example: $x \in \{-1, 0, 1\}$ and $y = x^2$. Then $\bar x = 0$, $\bar y = \tfrac{2}{3}$, and the products of deviations are $-\tfrac13$, $0$, and $\tfrac13$. They cancel, so $\text{Cov} = 0$, even though $y$ is completely determined by $x$.

Your exponential-versus-flat-line case is the same issue. A near-zero slope with an exponential partner gives a covariance that depends on a few extreme points, so it can look large and still say little about the overall shape.

**Resolution.** Covariance is not a general measure of dependence. It answers one specific question: is there a linear tilt, and how strong is it on average? To use it safely:

1. Plot the points first.
2. Use correlation to compare across scales.
3. Remember that zero covariance does not mean independence. For jointly Gaussian variables, zero covariance does imply independence, which is why it works so well there.

With those checks, covariance is useful. It's the right tool for linear structure, such as the tilted ellipses of the Gaussian, and a misleading one for everything else.

---

## 12. Probability as geometry (Hilbert space view)

**Setup.** Consider random variables with finite variance. Center them so each has mean zero. This collection is a vector space, and it has an inner product.

| Probability | Geometry |
|---|---|
| $\langle X, Y\rangle = E[XY]$ | inner product |
| $\|X\|^2 = E[X^2] = \text{Var}(X)$ | squared length |
| $\rho = \dfrac{\langle X,Y\rangle}{\|X\|\,\|Y\|}$ | cosine of the angle $\theta$ |
| $\text{Cov}(X,Y) = 0$ | orthogonal (perpendicular) |

For uncentered variables, apply these to $X - \mu_X$ and $Y - \mu_Y$. That's why covariance is the inner product of deviations.

**Consequences.**

- **Bounded correlation.** Since $|\cos\theta| \le 1$, we get $|\rho| \le 1$, which is Cauchy-Schwarz: $|\text{Cov}(X,Y)| \le \sigma_X\sigma_Y$.
- **Uncorrelated = perpendicular.** Two variables with zero covariance are at right angles, whatever their scales.

**Projection = best linear prediction.** Predict $Y$ by a multiple of $X$: $\hat Y = aX$. The error $Y - aX$ is shortest when it is perpendicular to $X$:
$$\langle Y - aX,\ X\rangle = 0 \quad\Longrightarrow\quad a = \frac{\text{Cov}(X,Y)}{\text{Var}(X)}$$

So the regression slope is the projection coefficient, and the residual is orthogonal to $X$.

**Your nonlinear case.** Let $x$ be symmetric about 0 and $y = x^2$. Then
$$\langle x, x^2\rangle = E[x^3] = 0$$
so $x$ and $x^2$ are perpendicular. They are completely dependent, yet they look orthogonal to the covariance. The geometry shows exactly what linear covariance cannot see.

**Caveats.**
- The theory needs finite second moments. Heavy-tailed variables can have infinite norm.
- Outliers still matter: a long vector dominates the geometry, just as an outlier dominates variance.
- In full generality, the space is infinite-dimensional, which needs completeness (Hilbert space). For finite data, ordinary linear algebra suffices.

**Where this leads.** Conditional expectation $E[Y \mid X]$ is the projection of $Y$ onto all functions of $X$, not just linear ones. That's why the geometric view grows into martingales and stochastic processes.

**Bias-variance as Pythagoras.** Treat the estimator $\hat\theta$ as a vector in the space of random variables, and the true parameter $\theta$ as a constant vector. The constant random variables form a subspace $C$.

- **Expectation is a projection.** $E[\hat\theta]$ is the constant closest to $\hat\theta$, and $\hat\theta - E[\hat\theta]$ is perpendicular to every constant. Concretely, $E[\hat\theta - E\hat\theta] = 0$ is that perpendicularity.
- **Error splits into two perpendicular pieces:**
$$\hat\theta - \theta = \underbrace{(\hat\theta - E\hat\theta)}_{\text{spread, in } C^\perp} + \underbrace{(E\hat\theta - \theta)}_{\text{bias, in } C}$$
- **Pythagoras:** the squared lengths add, which is the MSE decomposition:
$$\|\hat\theta-\theta\|^2 = \|\hat\theta - E\hat\theta\|^2 + \|E\hat\theta - \theta\|^2 \quad\Longleftrightarrow\quad \text{MSE} = \text{Var} + \text{Bias}^2$$

So bias is the distance from the target to the mean point, and variance is the spread of the estimator around that mean point. Bias-variance tradeoff is a geometric tradeoff: an estimator that shrinks toward a fixed point (say, zero) moves its mean closer to some targets and farther from others, and it can shorten the spread enough that the total length shrinks.

### Further reading (Hilbert space view)

- **Kreyszig, *Introductory Functional Analysis with Applications*.** Hilbert-space basics, inner products, projections. Prerequisite-level for the geometry.
- **Williams, *Probability with Martingales*.** $L^2$ spaces and conditional expectation as projection. Next step after this section.
- **Small & McLeish, *Hilbert Space Methods in Probability and Statistical Inference* (1994).** Connects the geometry to estimation and inference directly.
- **Durrett, *Probability: Theory and Examples*.** Measure-theoretic foundations, if you want the rigor behind "random variables with finite variance."

*Book names and editions are from memory; verify before relying on them.*

---

## 13. Statistical decision theory (ESL, Chapter 2)

**Setup.** Inputs $X \in \mathbb{R}^p$, output $Y \in \mathbb{R}$, with joint distribution $\Pr(X,Y)$. A predictor $f(X)$ incurs loss $L(Y, f(X))$. With squared-error loss, the **expected prediction error** is
$$\text{EPE}(f) = E\big[(Y - f(X))^2\big]$$

The goal is the function $f$ that makes EPE smallest. This is a population quantity: it uses the true distribution, not data.

**Step 1: condition on $X$.** Since $\Pr(X,Y) = \Pr(Y\mid X)\Pr(X)$,
$$\text{EPE}(f) = E_X\Big[E_{Y\mid X}\big[(Y - f(X))^2 \mid X\big]\Big]$$
The outer expectation averages over $x$. So it's enough to minimize the inner term for each $x$ separately, treating $f(x)$ as a constant $c$.

**Step 2: minimize pointwise.** Let $\mu(x) = E[Y\mid X=x]$. Then
$$E\big[(Y-c)^2 \mid X=x\big] = E\big[(Y-\mu(x))^2 \mid X=x\big] + (\mu(x)-c)^2$$
The cross term vanishes because $E[Y-\mu(x)\mid X=x] = 0$. The first term doesn't depend on $c$, and the second is minimized at $c = \mu(x)$.

**Result.** The best predictor is the conditional mean:
$$f^*(x) = E[Y\mid X=x]$$
This is the **regression function**. Its error is $E[\text{Var}(Y\mid X)]$, which no function of $X$ can remove.

**Error decomposition.** For any predictor $f$,
$$\text{EPE}(f) = E\big[\text{Var}(Y\mid X)\big] + E\big[(f^*(X) - f(X))^2\big]$$
The first term is irreducible error. The second is reducible error: the squared distance from $f$ to $f^*$. The decomposition is the same Pythagorean split as in section 10.

**Geometric view (section 12).** $f^*(X) = E[Y\mid X]$ is the projection of $Y$ onto all functions of $X$, and the residual $Y - f^*(X)$ is perpendicular to every such function.

**Linear case.** Restrict to $f(x) = x^T\beta$. Then
$$\text{EPE}(\beta) = E\big[(Y - X^T\beta)^2\big]$$
Setting the gradient to zero gives $E[X(Y - X^T\beta)] = 0$, the normal equations, which say the residual is perpendicular to $X$. So
$$\beta = \big(E[XX^T]\big)^{-1} E[XY]$$
With centered $X$ and $Y$, $E[XX^T] = \Sigma_{XX}$ and $E[XY] = \Sigma_{XY}$, so $\beta = \Sigma_{XX}^{-1}\Sigma_{XY}$. This is the covariance formula for regression, and it's the scalar $a = \text{Cov}/\text{Var}$ from section 12 in matrix form. It requires $\Sigma_{XX}$ to be invertible, which needs positive definiteness.

**Other losses.**
- **Absolute error** $|Y - f(X)|$: the minimizer is the conditional median.
- **Classification with 0-1 loss:** the minimizer is the Bayes classifier, which picks the class $g$ maximizing $\Pr(G = g \mid X = x)$.

**Why this matters.** EPE is the target, not something you can compute directly, because the true distribution is unknown. Nearest-neighbor and regression methods try to approximate $E[Y\mid X=x]$ from data. The bias-variance tradeoff (section 10) describes how well those approximations do.

---

## 14. Sources

**Population vs. sample (parameter vs. statistic)**
- **Moore & McCabe, *Introduction to the Practice of Statistics*.** Introduces "parameter" and "statistic" explicitly, early on.
- **Freedman, Pisani & Purves, *Statistics*.** Population vs. sample, and why $n-1$.
- **Wackerly, Mendenhall & Scheaffer, *Mathematical Statistics with Applications*.** Estimators, bias, unbiasedness (the formal $n-1$ derivation).

**Probability only (no sample/population framing)**
- **Feller, *An Introduction to Probability Theory and Its Applications*, Vol. 1.** Expectation, variance, LLN. $\mu = E[X]$ for a model.
- **Jaynes, *Probability Theory: The Logic of Science*.** Inference from data, Bayesian framing.
- **Williams, *Weighing the Odds*.** Rigorous probability.

**Next step after an intro text**
- **Hastie, Tibshirani & Friedman, *Elements of Statistical Learning*** (free PDF, Stanford). Assumes the distinction. Relevant for the multivariate Gaussian, covariance estimation (LDA/QDA), and bias-variance.

**Entropy and maximum entropy (for section 4b)**
- **Cover & Thomas, *Elements of Information Theory*.** Standard text. Entropy, KL divergence, maximum-entropy distributions including the Gaussian. Start with the chapters on entropy and relative entropy.
- **MacKay, *Information Theory, Inference, and Learning Algorithms*.** More conversational, with many examples. Free online. Connects information theory to Bayesian inference, close to Jaynes's framing.

**Suggested order for the entropy material:** Lagrange multipliers for ordinary functions → entropy for discrete distributions → KL divergence → Gibbs' inequality → differential entropy → Gaussian maximum-entropy proof.

*Book names are from memory; check edition and chapter numbers before relying on them.*
