---
name: gaussian-from-scratch
description: Build the Gaussian distribution from first principles, starting with univariate through multivariate
metadata:
  type: reference
---

# Gaussian Distribution from Scratch

**Goal:** Understand where the Gaussian formula comes from, not just memorize it.

---

## Part 1: What is a Probability Distribution?

### Intuition
A probability distribution describes how likely different outcomes are.

**Example:** Height of people
- Most people are around 5'9"
- Some taller, some shorter
- Very few are 7 feet tall or 3 feet tall
- **Distribution:** captures this "most likely around here, fewer extremes" idea

### Probability Density Function (PDF)
For a continuous random variable $X$, the PDF $f(x)$ tells us:
- **Higher $f(x)$** at some value $x$ → that value is more likely
- **Lower $f(x)$** at some value $x$ → that value is less likely

**Key property:** $\int_{-\infty}^{\infty} f(x) \, dx = 1$ (total probability is 1)

---

## Part 2: Building the Univariate (1D) Gaussian from Intuition

### What Properties Should the Distribution Have?

We want a distribution that:
1. **Is symmetric** around a center point (call it $\mu$)
   - Equally likely to be above or below the mean
   - "Bell-shaped"

2. **Has a single peak** at the mean
   - Most likely value is $\mu$
   - Probability decreases as we move away from $\mu$

3. **Has a parameter that controls spread**
   - In one dataset, values cluster tightly around $\mu$ (small spread)
   - In another dataset, values are scattered (large spread)
   - Call this parameter the **standard deviation** $\sigma$

4. **Has light tails**
   - Extreme values are possible but very rare
   - Not a distribution with heavy tails (like Cauchy)

### Designing the PDF

**Start simple: the core shape**

We want something that:
- Is symmetric around $\mu$
- Decreases as we move away from $\mu$

A natural choice: **squared distance from the mean**

$$\text{Distance}^2 = (x - \mu)^2$$

This is symmetric (squaring removes the sign) and grows as we move away from $\mu$.

**But we want probability to decrease when distance increases.** So use an exponential (which decays):

$$f(x) \propto \exp(-(x - \mu)^2)$$

Let's call this the **unnormalized** PDF. The proportionality sign ($\propto$) means "proportional to" — we haven't fixed the constant yet.

### Adding the Spread Parameter $\sigma$

The formula above doesn't account for spread. Let's think about it:

- If $\sigma$ is **small** (tightly clustered), we want the exponent to decrease quickly
  - Points far from $\mu$ should be very unlikely
- If $\sigma$ is **large** (spread out), the exponent decreases slowly
  - Points far from $\mu$ are still somewhat likely

**Solution:** Divide the distance by $\sigma$ before squaring:

$$f(x) \propto \exp\left(-\frac{(x - \mu)^2}{2\sigma^2}\right)$$

Why the $\frac{1}{2}$ in the denominator? We'll see later; it makes the math cleaner.

Now:
- If $\sigma$ is small, $\frac{(x-\mu)^2}{\sigma^2}$ is large, so the exponent is very negative → low probability
- If $\sigma$ is large, $\frac{(x-\mu)^2}{\sigma^2}$ is small, so the exponent isn't as negative → higher probability

✓ This captures the spreading behavior.

### Finding the Normalization Constant

We have:
$$f(x) \propto \exp\left(-\frac{(x - \mu)^2}{2\sigma^2}\right)$$

But we need $\int_{-\infty}^{\infty} f(x) \, dx = 1$.

So:
$$\int_{-\infty}^{\infty} C \exp\left(-\frac{(x - \mu)^2}{2\sigma^2}\right) dx = 1$$

where $C$ is the constant we need to find.

**To compute this integral:**

**Step 1:** Substitute $u = \frac{x - \mu}{\sigma}$ (standardize)

Then $x = \mu + \sigma u$, so $dx = \sigma \, du$.

$$\int_{-\infty}^{\infty} C \exp\left(-\frac{u^2}{2}\right) \sigma \, du = C\sigma \int_{-\infty}^{\infty} \exp\left(-\frac{u^2}{2}\right) du = 1$$

**Step 2:** This integral is a famous one (Gaussian integral):

$$\int_{-\infty}^{\infty} \exp\left(-\frac{u^2}{2}\right) du = \sqrt{2\pi}$$

(You can compute this using polar coordinates or look it up; it's a standard result.)

**Step 3:** Solve for $C$:

$$C \sigma \sqrt{2\pi} = 1$$

$$C = \frac{1}{\sigma \sqrt{2\pi}}$$

### Final Formula: Univariate Gaussian

$$\boxed{f(x) = \frac{1}{\sigma\sqrt{2\pi}} \exp\left(-\frac{(x - \mu)^2}{2\sigma^2}\right)}$$

This is the **probability density function of the Gaussian (normal) distribution** with parameters $\mu$ (mean) and $\sigma^2$ (variance).

---

## Part 3: Understanding the Univariate Gaussian

### Notation
$$X \sim \mathcal{N}(\mu, \sigma^2)$$

Reads: "$X$ follows a normal distribution with mean $\mu$ and variance $\sigma^2$."

(Note: Some books write $\mathcal{N}(\mu, \sigma)$ using standard deviation instead. ESL uses $\sigma^2$ for variance.)

### Parameters

**Mean $\mu$:**
- Location of the peak
- $E[X] = \mu$
- The distribution is symmetric around $\mu$

**Variance $\sigma^2$:**
- Controls the spread/width
- $\text{Var}(X) = \sigma^2$
- $\sigma = \sqrt{\sigma^2}$ is the **standard deviation**

**Relationship:**
- Larger $\sigma^2$ → wider bell curve
- Smaller $\sigma^2$ → narrower, taller bell curve
- $\sigma^2 = 0$ → distribution collapses to a point at $\mu$ (degenerate)

### Shape

**68-95-99.7 Rule:**
- ~68% of data within $\mu \pm 1\sigma$
- ~95% of data within $\mu \pm 2\sigma$
- ~99.7% of data within $\mu \pm 3\sigma$

Very extreme values (beyond $\mu \pm 4\sigma$) are extremely unlikely.

### Standard Gaussian

$$Z \sim \mathcal{N}(0, 1)$$

Mean 0, variance 1. The simplest case.

$$f(z) = \frac{1}{\sqrt{2\pi}} \exp\left(-\frac{z^2}{2}\right)$$

Any Gaussian can be standardized:
$$Z = \frac{X - \mu}{\sigma} \sim \mathcal{N}(0, 1)$$

---

## Part 4: Why the Gaussian is Special

### 1. Central Limit Theorem

**Theorem:** If you average many independent random variables (from any distribution), the average is approximately Gaussian.

**Why it matters:** Most data we see in the real world is an average or sum of many effects → often Gaussian.

**Example:**
- Human height = genetic + nutrition + environment + ... (many factors) → approximately Gaussian
- Measurement error = many small random errors combined → approximately Gaussian

### 2. Mathematically Convenient

- Linear transformations of Gaussians are Gaussian
- Sums of independent Gaussians are Gaussian
- Conditional distributions (given part of the data) are Gaussian
- MLE (maximum likelihood estimates) have closed-form solutions

**Why it matters for ESL:** Makes computations tractable.

### 3. Maximum Entropy

Among all distributions with a given mean and variance, the Gaussian has the maximum entropy (most "uncertain").

**Implication:** If you only know the mean and variance of data, Gaussian is the least-biased assumption.

---

## Part 5: From Univariate to Bivariate

### Two Correlated Variables: $(X_1, X_2)$

Now suppose we have **two** random variables. We can't just assume they're independent Gaussians (that would be too restrictive).

**Key idea:** We need to capture correlation between $X_1$ and $X_2$.

### Parameters for Bivariate Gaussian

**Means:**
- $\mu_1 = E[X_1]$
- $\mu_2 = E[X_2]$

**Variances:**
- $\sigma_1^2 = \text{Var}(X_1)$
- $\sigma_2^2 = \text{Var}(X_2)$

**Covariance:**
- $\sigma_{12} = \text{Cov}(X_1, X_2) = E[(X_1 - \mu_1)(X_2 - \mu_2)]$
- Measures how $X_1$ and $X_2$ move together
- If positive: when $X_1$ is above its mean, $X_2$ tends to be above its mean too
- If negative: when $X_1$ is above its mean, $X_2$ tends to be below its mean
- If zero: $X_1$ and $X_2$ are uncorrelated (but not necessarily independent)

### The Bivariate PDF

We stack means and variances into matrices:

$$\boldsymbol{\mu} = \begin{pmatrix} \mu_1 \\ \mu_2 \end{pmatrix}, \quad \boldsymbol{\Sigma} = \begin{pmatrix} \sigma_1^2 & \sigma_{12} \\ \sigma_{12} & \sigma_2^2 \end{pmatrix}$$

$\boldsymbol{\mu}$ is the **mean vector**, $\boldsymbol{\Sigma}$ is the **covariance matrix**.

The PDF becomes:

$$f(x_1, x_2) = \frac{1}{2\pi \sqrt{|\boldsymbol{\Sigma}|}} \exp\left( -\frac{1}{2} (\mathbf{x} - \boldsymbol{\mu})^T \boldsymbol{\Sigma}^{-1} (\mathbf{x} - \boldsymbol{\mu}) \right)$$

where $\mathbf{x} = \begin{pmatrix} x_1 \\ x_2 \end{pmatrix}$ and $|\boldsymbol{\Sigma}|$ is the determinant of $\boldsymbol{\Sigma}$.

### What Changed?

**Univariate:**
$$f(x) \propto \exp\left(-\frac{(x-\mu)^2}{2\sigma^2}\right)$$

**Bivariate:**
$$f(\mathbf{x}) \propto \exp\left(-\frac{1}{2}(\mathbf{x} - \boldsymbol{\mu})^T \boldsymbol{\Sigma}^{-1}(\mathbf{x} - \boldsymbol{\mu})\right)$$

**The pattern:**
- $(x - \mu)^2$ becomes $(\mathbf{x} - \boldsymbol{\mu})^T \boldsymbol{\Sigma}^{-1} (\mathbf{x} - \boldsymbol{\mu})$ (quadratic form)
- $\sigma^2$ becomes $\boldsymbol{\Sigma}$ (covariance matrix)
- $\frac{1}{\sigma\sqrt{2\pi}}$ becomes $\frac{1}{\sqrt{(2\pi)^p |\boldsymbol{\Sigma}|}}$

### Why the Quadratic Form?

The expression $(\mathbf{x} - \boldsymbol{\mu})^T \boldsymbol{\Sigma}^{-1} (\mathbf{x} - \boldsymbol{\mu})$ is called the **Mahalanobis distance** (squared).

**Intuition:**
- In univariate case: $(x - \mu)^2 / \sigma^2$ measures distance in units of standard deviations
- In bivariate case: $\boldsymbol{\Sigma}^{-1}$ "normalizes" by covariance — accounts for both spread and correlation
- Points at the same Mahalanobis distance have the same probability
- These form **ellipses** (not circles) because of correlation

---

## Part 6: From Bivariate to Multivariate ($p$ Dimensions)

### General Form

For $p$-dimensional Gaussian $\mathcal{N}(\boldsymbol{\mu}, \boldsymbol{\Sigma})$:

$$f(\mathbf{x}) = \frac{1}{(2\pi)^{p/2} |\boldsymbol{\Sigma}|^{1/2}} \exp\left(-\frac{1}{2}(\mathbf{x} - \boldsymbol{\mu})^T \boldsymbol{\Sigma}^{-1}(\mathbf{x} - \boldsymbol{\mu})\right)$$

where:
- $\mathbf{x}$ is $p \times 1$ (vector)
- $\boldsymbol{\mu}$ is $p \times 1$ (mean vector)
- $\boldsymbol{\Sigma}$ is $p \times p$ (covariance matrix, symmetric)
- $|\boldsymbol{\Sigma}|$ is the determinant (generalizes $\sigma^2$ from 1D)
- $(2\pi)^{p/2}$ generalizes $\sqrt{2\pi}$ from 1D

### Pattern Recognition

$$\text{1D:} \quad \frac{1}{\sqrt{2\pi \sigma^2}} \exp\left(-\frac{(x-\mu)^2}{2\sigma^2}\right)$$

$$\text{p-D:} \quad \frac{1}{(2\pi)^{p/2} \sqrt{|\boldsymbol{\Sigma}|}} \exp\left(-\frac{1}{2}(\mathbf{x} - \boldsymbol{\mu})^T \boldsymbol{\Sigma}^{-1}(\mathbf{x} - \boldsymbol{\mu})\right)$$

**The structure is identical; we just generalize scalars to matrices/vectors.**

---

## Part 6.5: Why Exponential? Not Something Else?

### The Question
You could imagine other functions:
- $f(x) \propto 1/(x-\mu)^2$ (reciprocal)
- $f(x) \propto 1/(1 + (x-\mu)^2)$ (Cauchy-like)
- $f(x) \propto (1 + (x-\mu)^2/\sigma^2)^{-\alpha}$ (power law)
- Many others...

Why exponential of a quadratic specifically? Let me show why other choices fail or are suboptimal.

### Why Not Reciprocal: $f(x) \propto 1/(x-\mu)^2$?

**Problem 1: Undefined at the peak**
$$f(\mu) = \frac{1}{0^2} = \infty$$

The peak should be finite! A PDF that blows up to infinity is pathological.

**Problem 2: Doesn't integrate to 1**
$$\int_{-\infty}^{\infty} \frac{1}{(x-\mu)^2} dx = \infty$$

We can never normalize this to a probability.

**Conclusion:** Reciprocal is not viable.

### Why Not Power Law: $f(x) \propto (1 + (x-\mu)^2)^{-\alpha}$?

This works mathematically (does integrate), but has **heavy tails**.

**Example:** For $\alpha = 1$, this is the Cauchy distribution.
$$P(|X| > M) \to \text{constant}$$ (doesn't decay to 0)

**Problem:** Extreme values are not rare. The distribution allows arbitrarily extreme outcomes with non-negligible probability.

**Compare to Gaussian:**
$$P(X > \mu + 5\sigma) \approx 3 \times 10^{-7}$$ (incredibly rare)

Most real phenomena (heights, errors, etc.) don't have such heavy tails.

### Three Deep Reasons for Exponential

There are three profound principles that *justify* the choice of exponential:

#### 1. Maximum Entropy Principle

**Question:** If I only know the mean $\mu$ and variance $\sigma^2$ of a distribution, which distribution should I assume?

**Answer (Jaynes):** The one with **maximum entropy** — maximum "uncertainty" or "spread" — given those constraints.

**Mathematical Result:** The distribution that maximizes entropy subject to:
- $\int f(x) dx = 1$ (normalization)
- $\int x f(x) dx = \mu$ (mean constraint)
- $\int (x-\mu)^2 f(x) dx = \sigma^2$ (variance constraint)

is exactly the Gaussian:
$$f(x) = \frac{1}{\sigma\sqrt{2\pi}} \exp\left(-\frac{(x-\mu)^2}{2\sigma^2}\right)$$

**Why this matters:** If you have limited information (only mean/variance) and must guess a distribution, Gaussian is the *least biased* choice. No other shape would be more defensible.

#### 2. Central Limit Theorem

**Theorem:** If you take $n$ independent random variables from *any* distribution (with finite mean/variance) and average them:
$$\overline{X} = \frac{1}{n}\sum_{i=1}^n X_i$$

As $n \to \infty$, the distribution of $\overline{X}$ approaches Gaussian, *regardless of what the $X_i$ were originally*.

**Why this matters:** Real-world data often represents averages (height = genetic + nutrition + environmental factors, ...). So Gaussian emerges *naturally*.

**Example:** Suppose individual genetic contributions are uniformly distributed. Heights (sum of many contributions) → Gaussian.

#### 3. Mathematical Tractability

Exponential of a quadratic is **incredibly convenient**:

1. **Integrates to 1** easily (using Gaussian integral)
2. **Linear transformations preserve it** (if $X$ is Gaussian, so is $aX + b$)
3. **Marginals are Gaussian** (easier distributions, not harder)
4. **Conditionals are Gaussian** (Bayesian updates stay in the family)
5. **Derivatives are clean** (smooth, differentiable, optimization works)
6. **MLE has closed form** (just compute mean and covariance)

Other families (heavy-tailed, ...) don't have these properties.

### What About Exponential of Higher Powers?

Could we use $f(x) \propto \exp(-(x-\mu)^4)$ instead?

**Mathematical perspective:**
- Yes, this would work (integrates to finite constant)
- Called Gaussian with $\ell^4$ instead of $\ell^2$ norm

**But why not?**
1. **Contradicts max entropy principle** (not maximum entropy for given variance)
2. **Breaks CLT** (sums of non-Gaussian don't converge to this)
3. **Heavier tails near the mean, lighter far away** (less realistic)

### Deeper Insight: Why Quadratic in the Exponent?

The **quadratic form** $(x-\mu)^2$ appears because:

1. **Symmetry:** We want equal probability above/below the mean
   - $(x-\mu)^2$ is symmetric: both $+$ and $-$ give the same value
   - $(x-\mu)$ alone is not symmetric

2. **Smooth decay:** We want probability to decrease smoothly, not abruptly
   - Quadratic grows slowly near $\mu$, faster far away
   - This gives the smooth bell shape

3. **Consistency:** For multivariate case, the **Mahalanobis distance** (which is quadratic) is the unique way to account for correlation and scale consistently

### Summary: Why This Specific Formula?

| Aspect | Why It Wins |
|--------|-----------|
| **Not reciprocal** | Integrates to 1, finite at peak |
| **Not power law** | Light tails (rare extremes), not heavy tails |
| **Exponential** | Max entropy + CLT justify it + mathematically clean |
| **Quadratic in exponent** | Symmetry + smooth decay + multivariate consistency |

**Bottom line:** The Gaussian isn't arbitrary. It emerges from three principles:
1. **Information theory** (maximum entropy)
2. **Probability theory** (central limit theorem)
3. **Mathematical elegance** (tractability and closure properties)

It's the *least biased, most natural, most useful* choice given what we know.

---

## Part 7: Intuitive Summary

### Why This Shape?

1. **Symmetric bell curve:** Makes sense for "centered" phenomena
2. **Exponential decay:** Light tails (extreme values are rare)
3. **Quadratic form in exponent:** Smooth, differentiable shape; math works out nicely
4. **Controls spread via $\sigma$ (or $\boldsymbol{\Sigma}$):** Flexible
5. **Closed under linear transformations:** If you transform Gaussian data, you get Gaussian

### Where It Comes From

**Philosophy:** The Gaussian is the "simplest" distribution that:
- Has finite mean and variance
- Has maximum entropy given those moments
- Appears as the limit in the Central Limit Theorem

**In practice:** We assume Gaussian because:
- It's mathematically convenient
- It's a good approximation for many real phenomena
- When in doubt (and you only know mean/variance), Gaussian is the least-biased choice

---

## Part 8: Key Properties of Multivariate Gaussians

### 1. Linear Transformations Preserve Gaussianity

If $\mathbf{X} \sim \mathcal{N}(\boldsymbol{\mu}, \boldsymbol{\Sigma})$ and $\mathbf{Y} = A\mathbf{X} + \mathbf{b}$ (linear transformation), then:

$$\mathbf{Y} \sim \mathcal{N}(A\boldsymbol{\mu} + \mathbf{b}, A\boldsymbol{\Sigma} A^T)$$

**Why?** Gaussian is closed under linear transformations.

**Example (1D projection):**
$$Y = \mathbf{w}^T \mathbf{X} = \sum_{i=1}^p w_i X_i$$

Then:
$$Y \sim \mathcal{N}(\mathbf{w}^T \boldsymbol{\mu}, \mathbf{w}^T \boldsymbol{\Sigma} \mathbf{w})$$

This is used in **LDA** (Linear Discriminant Analysis) — we project high-dimensional data onto a line to separate classes.

### 2. Marginals are Gaussian

If $\mathbf{X} \sim \mathcal{N}(\boldsymbol{\mu}, \boldsymbol{\Sigma})$, then each component is Gaussian:

$$X_i \sim \mathcal{N}(\mu_i, \Sigma_{ii})$$

**Key insight:** Each $X_i$ individually is univariate Gaussian with variance $\Sigma_{ii}$ (the diagonal entry).

### 3. Conditionals are Gaussian

Partition $\mathbf{X} = \begin{pmatrix} \mathbf{X}_1 \\ \mathbf{X}_2 \end{pmatrix}$ and $\boldsymbol{\mu} = \begin{pmatrix} \boldsymbol{\mu}_1 \\ \boldsymbol{\mu}_2 \end{pmatrix}$

$$\boldsymbol{\Sigma} = \begin{pmatrix} \boldsymbol{\Sigma}_{11} & \boldsymbol{\Sigma}_{12} \\ \boldsymbol{\Sigma}_{21} & \boldsymbol{\Sigma}_{22} \end{pmatrix}$$

Then the conditional distribution is:

$$\mathbf{X}_1 | \mathbf{X}_2 = \mathbf{x}_2 \sim \mathcal{N}(\boldsymbol{\mu}_{1|2}, \boldsymbol{\Sigma}_{1|2})$$

where:
$$\boldsymbol{\mu}_{1|2} = \boldsymbol{\mu}_1 + \boldsymbol{\Sigma}_{12} \boldsymbol{\Sigma}_{22}^{-1} (\mathbf{x}_2 - \boldsymbol{\mu}_2)$$

$$\boldsymbol{\Sigma}_{1|2} = \boldsymbol{\Sigma}_{11} - \boldsymbol{\Sigma}_{12} \boldsymbol{\Sigma}_{22}^{-1} \boldsymbol{\Sigma}_{21}$$

**Intuition:**
- Conditional mean is linear in $\mathbf{x}_2$
- Conditional covariance doesn't depend on $\mathbf{x}_2$ (only on the parameters)
- This is used in **Bayesian updating** and **recursive filters**

### 4. Independent Components ⟺ Zero Covariance (for Gaussians only)

If $\boldsymbol{\Sigma}$ is diagonal (all off-diagonal entries are zero), then components are independent.

**This is unique to Gaussians!** For other distributions, uncorrelated ≠ independent.

### 5. Joint of Independent Gaussians

If $\mathbf{X} \sim \mathcal{N}(\boldsymbol{\mu}_1, \boldsymbol{\Sigma}_1)$ and $\mathbf{Y} \sim \mathcal{N}(\boldsymbol{\mu}_2, \boldsymbol{\Sigma}_2)$ are independent, then:

$$\begin{pmatrix} \mathbf{X} \\ \mathbf{Y} \end{pmatrix} \sim \mathcal{N}\left( \begin{pmatrix} \boldsymbol{\mu}_1 \\ \boldsymbol{\mu}_2 \end{pmatrix}, \begin{pmatrix} \boldsymbol{\Sigma}_1 & 0 \\ 0 & \boldsymbol{\Sigma}_2 \end{pmatrix} \right)$$

Independence ⟺ zero off-diagonal covariance blocks.

---

## Part 9: Why Multivariate Gaussians Matter in ESL

### 1. Linear Discriminant Analysis (LDA) — Chapter 4
Assumes each class has Gaussian distribution with shared covariance.
$$P(\mathbf{X} | Y = k) = \mathcal{N}(\boldsymbol{\mu}_k, \boldsymbol{\Sigma})$$

Uses Bayes' rule to classify: $P(Y=k|\mathbf{X}) \propto P(\mathbf{X}|Y=k) P(Y=k)$

### 2. Quadratic Discriminant Analysis (QDA) — Chapter 4
Each class has its own Gaussian with different covariance:
$$P(\mathbf{X} | Y = k) = \mathcal{N}(\boldsymbol{\mu}_k, \boldsymbol{\Sigma}_k)$$

### 3. Gaussian Mixture Models (GMM) — Chapter 8
Assume data is mixture of $K$ Gaussians. Use EM algorithm to estimate parameters.

### 4. Maximum Likelihood Estimation (MLE) — Chapter 8
Under Gaussian assumption, MLE is closed-form (just compute sample mean and covariance).

### 5. Regression with Normal Errors — Chapter 3
Assume $Y | X = \mathbf{x} \sim \mathcal{N}(\mathbf{x}^T \beta, \sigma^2)$. Then least squares = MLE.

### 6. Bayesian Methods — Chapter 8
Gaussians are conjugate priors in many models (math works out nicely).

---

## Part 10: Common Operations

### Sampling
To sample from $\mathcal{N}(\boldsymbol{\mu}, \boldsymbol{\Sigma})$:
1. Sample $\mathbf{Z} \sim \mathcal{N}(0, I)$ (easy; sample each component independently from $N(0,1)$)
2. Compute $\mathbf{X} = L\mathbf{Z} + \boldsymbol{\mu}$ where $L$ is the Cholesky decomposition of $\boldsymbol{\Sigma}$ (so $LL^T = \boldsymbol{\Sigma}$)

### Log-Likelihood
For $n$ independent samples $\mathbf{x}_1, \ldots, \mathbf{x}_n$ from $\mathcal{N}(\boldsymbol{\mu}, \boldsymbol{\Sigma})$:

$$\log L(\boldsymbol{\mu}, \boldsymbol{\Sigma}) = -\frac{np}{2}\log(2\pi) - \frac{n}{2}\log|\boldsymbol{\Sigma}| - \frac{1}{2}\sum_{i=1}^n (\mathbf{x}_i - \boldsymbol{\mu})^T \boldsymbol{\Sigma}^{-1} (\mathbf{x}_i - \boldsymbol{\mu})$$

### MLE (Maximum Likelihood Estimates)
$$\hat{\boldsymbol{\mu}} = \frac{1}{n}\sum_{i=1}^n \mathbf{x}_i = \overline{\mathbf{x}} \quad \text{(sample mean)}$$

$$\hat{\boldsymbol{\Sigma}} = \frac{1}{n}\sum_{i=1}^n (\mathbf{x}_i - \hat{\boldsymbol{\mu}})(\mathbf{x}_i - \hat{\boldsymbol{\mu}})^T \quad \text{(sample covariance)}$$

---

## Part 11: Connection to Least Squares

**Key insight:** Least squares = MLE under Gaussian noise.

Assume regression model:
$$Y | X = \mathbf{x} \sim \mathcal{N}(\mathbf{x}^T \beta, \sigma^2)$$

The likelihood for $n$ samples is:
$$L(\beta, \sigma^2) = \prod_{i=1}^n \frac{1}{\sqrt{2\pi\sigma^2}} \exp\left(-\frac{1}{2\sigma^2}(y_i - \mathbf{x}_i^T \beta)^2\right)$$

Taking log:
$$\log L = -\frac{n}{2}\log(2\pi\sigma^2) - \frac{1}{2\sigma^2}\sum_{i=1}^n (y_i - \mathbf{x}_i^T \beta)^2$$

Maximizing over $\beta$ (ignoring constant terms):
$$\max_\beta \log L = \min_\beta \sum_{i=1}^n (y_i - \mathbf{x}_i^T \beta)^2 = \min_\beta \text{RSS}(\beta)$$

**So:** Least squares is the MLE under Gaussian errors. This gives a probabilistic interpretation to regression.

---

## Summary Checklist

- [ ] Know the parameters: mean $\boldsymbol{\mu}$ (location), covariance $\boldsymbol{\Sigma}$ (spread/correlation)
- [ ] Recognize the PDF formula (quadratic form in exponent)
- [ ] Understand marginals are Gaussian (each component individually Gaussian)
- [ ] Understand conditionals are Gaussian (with linear mean, constant covariance)
- [ ] Know linear transformations preserve Gaussianity
- [ ] Connect to LDA/QDA (each class is Gaussian)
- [ ] Connect to regression (errors are Gaussian)
- [ ] Recognize MLE for parameters (sample mean and covariance)
- [ ] Understand Mahalanobis distance (accounts for correlation and scale)
- [ ] See why Gaussian is special (CLT, conjugacy, tractability)

---

## Related ESL Chapters

- **Ch. 4 (Classification):** LDA, QDA assume Gaussian per-class densities
- **Ch. 8 (EM, Bayesian):** Mixture models, missing data, Bayesian inference with Gaussians
- **Ch. 13 (Prototypes, Clustering):** Gaussian mixtures
- **Throughout:** Regression models assume Gaussian errors
