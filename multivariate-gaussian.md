---
name: multivariate-gaussian
description: Foundational reference for multivariate normal distribution; used throughout ESL for classification, EM, and probabilistic inference
metadata:
  type: reference
---
	
# Multivariate Gaussian (Normal) Distribution

**Purpose:** The multivariate Gaussian is the most common probability model in statistics and machine learning. You'll encounter it repeatedly in ESL, especially in classification (LDA, QDA), density estimation, and EM algorithms.

---

## Part 1: Definition

### Setup
- $\mathbf{X}$ is a $p$-dimensional random vector (random variable with $p$ components)
- We say $\mathbf{X} \sim \mathcal{N}(\boldsymbol{\mu}, \boldsymbol{\Sigma})$ if it follows a multivariate Gaussian

### Parameters

**Mean vector:** $\boldsymbol{\mu} = \begin{pmatrix} \mu_1 \\ \mu_2 \\ \vdots \\ \mu_p \end{pmatrix}$ (dimension $p \times 1$)
- The center of the distribution
- $E[\mathbf{X}] = \boldsymbol{\mu}$

**Covariance matrix:** $\boldsymbol{\Sigma}$ (dimension $p \times p$)
$$\boldsymbol{\Sigma} = \text{Cov}(\mathbf{X}) = E[(\mathbf{X} - \boldsymbol{\mu})(\mathbf{X} - \boldsymbol{\mu})^T]$$

- Entry $\Sigma_{ij}$ is the covariance between $X_i$ and $X_j$
- Diagonal: $\Sigma_{ii} = \text{Var}(X_i)$ (variance of component $i$)
- Off-diagonal: $\Sigma_{ij} = \text{Cov}(X_i, X_j)$ (how $i$ and $j$ co-vary)
- **Always symmetric:** $\Sigma = \Sigma^T$ (because covariance is symmetric)
- **Always positive semi-definite:** $\mathbf{v}^T \Sigma \mathbf{v} \geq 0$ for all vectors $\mathbf{v}$
  - Intuitively: variances and covariances can't be negative in a way that makes a variance negative

### Notation
- $\mathcal{N}(\boldsymbol{\mu}, \boldsymbol{\Sigma})$ — multivariate Gaussian with mean $\boldsymbol{\mu}$ and covariance $\boldsymbol{\Sigma}$
- $\mathcal{N}(0, I)$ — standard Gaussian (mean 0, identity covariance)

---

## Part 2: Probability Density Function (PDF)

### Formula

$$f(\mathbf{x}) = \frac{1}{(2\pi)^{p/2} |\boldsymbol{\Sigma}|^{1/2}} \exp\left( -\frac{1}{2}(\mathbf{x} - \boldsymbol{\mu})^T \boldsymbol{\Sigma}^{-1} (\mathbf{x} - \boldsymbol{\mu}) \right)$$

where:
- $|\boldsymbol{\Sigma}|$ is the **determinant** of $\boldsymbol{\Sigma}$ (measures the "volume" of covariance)
- $\boldsymbol{\Sigma}^{-1}$ is the **inverse** of $\boldsymbol{\Sigma}$ (called the precision matrix)
- The exponent is a **quadratic form** in $(\mathbf{x} - \boldsymbol{\mu})$

### Components

**Normalization constant:** $\frac{1}{(2\pi)^{p/2} |\boldsymbol{\Sigma}|^{1/2}}$
- Ensures the PDF integrates to 1
- Depends on dimension $p$ and the scale of covariance $|\boldsymbol{\Sigma}|$

**Quadratic form (Mahalanobis distance):** $(\mathbf{x} - \boldsymbol{\mu})^T \boldsymbol{\Sigma}^{-1} (\mathbf{x} - \boldsymbol{\mu})$
- Measures how far $\mathbf{x}$ is from the mean $\boldsymbol{\mu}$
- **NOT** Euclidean distance; accounts for correlations and scales (hence "Mahalanobis")
- Larger value → lower probability (in the exponent with negative sign)

### Interpretation

- **Peak:** Highest density at $\mathbf{x} = \boldsymbol{\mu}$
- **Shape:** Elliptical (in 2D) or hyper-ellipsoidal (in higher dimensions)
  - Axes of ellipse determined by eigenvectors of $\boldsymbol{\Sigma}$
  - Lengths of axes determined by eigenvalues of $\boldsymbol{\Sigma}$
- **Spread:** Controlled by $\boldsymbol{\Sigma}$
  - Larger variance → more spread
  - Large off-diagonal covariances → tilted ellipse

---

## Part 3: Key Properties

### 1. Linear Transformations

If $\mathbf{X} \sim \mathcal{N}(\boldsymbol{\mu}, \boldsymbol{\Sigma})$ and $\mathbf{Y} = A\mathbf{X} + \mathbf{b}$ (linear transformation), then:

$$\mathbf{Y} \sim \mathcal{N}(A\boldsymbol{\mu} + \mathbf{b}, A\boldsymbol{\Sigma} A^T)$$

**Why?** Gaussian is closed under linear transformations.

**Example (1D projection):**
$$Y = \mathbf{w}^T \mathbf{X} = \sum_{i=1}^p w_i X_i$$

Then:
$$Y \sim \mathcal{N}(\mathbf{w}^T \boldsymbol{\mu}, \mathbf{w}^T \boldsymbol{\Sigma} \mathbf{w})$$

This is used in **LDA** (Linear Discriminant Analysis) — we project high-dimensional data onto a line to separate classes.

### 2. Marginals (Individual Components)

If $\mathbf{X} \sim \mathcal{N}(\boldsymbol{\mu}, \boldsymbol{\Sigma})$, then each component is Gaussian:

$$X_i \sim \mathcal{N}(\mu_i, \Sigma_{ii})$$

**Key insight:** Each $X_i$ individually is univariate Gaussian with variance $\Sigma_{ii}$ (the diagonal entry).

### 3. Conditionals (Conditional Distributions)

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

### 4. Joint of Independent Gaussians

If $\mathbf{X} \sim \mathcal{N}(\boldsymbol{\mu}_1, \boldsymbol{\Sigma}_1)$ and $\mathbf{Y} \sim \mathcal{N}(\boldsymbol{\mu}_2, \boldsymbol{\Sigma}_2)$ are independent, then:

$$\begin{pmatrix} \mathbf{X} \\ \mathbf{Y} \end{pmatrix} \sim \mathcal{N}\left( \begin{pmatrix} \boldsymbol{\mu}_1 \\ \boldsymbol{\mu}_2 \end{pmatrix}, \begin{pmatrix} \boldsymbol{\Sigma}_1 & 0 \\ 0 & \boldsymbol{\Sigma}_2 \end{pmatrix} \right)$$

Independence ↔ zero off-diagonal covariance blocks.

### 5. Uncorrelated ↔ Independent

**Unique to Gaussian:** If $\mathbf{X} \sim \mathcal{N}(\boldsymbol{\mu}, \boldsymbol{\Sigma})$ and $\boldsymbol{\Sigma}$ is diagonal (uncorrelated components), then the components are independent.

**This is NOT true for general distributions!** For Gaussians only.

---

## Part 4: Why Gaussians Matter in ESL

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
Assume $Y | X \sim \mathcal{N}(X^T \beta, \sigma^2)$. Then least squares = MLE.

### 6. Bayesian Methods — Chapter 8
Gaussians are conjugate priors in many models (math works out nicely).

---

## Part 5: Standard Gaussian ($\mathcal{N}(0, I)$)

### Definition
Mean $\boldsymbol{\mu} = \mathbf{0}$ and covariance $\boldsymbol{\Sigma} = I$ (identity matrix).

### PDF
$$f(\mathbf{x}) = \frac{1}{(2\pi)^{p/2}} \exp\left( -\frac{1}{2} \mathbf{x}^T \mathbf{x} \right) = \frac{1}{(2\pi)^{p/2}} \exp\left( -\frac{1}{2} \|\mathbf{x}\|^2 \right)$$

Components are independent (identity covariance), each $N(0,1)$.

### Standardization
To convert $\mathbf{X} \sim \mathcal{N}(\boldsymbol{\mu}, \boldsymbol{\Sigma})$ to standard Gaussian:

$$\mathbf{Z} = \boldsymbol{\Sigma}^{-1/2} (\mathbf{X} - \boldsymbol{\mu})$$

Then $\mathbf{Z} \sim \mathcal{N}(0, I)$.

(Requires $\boldsymbol{\Sigma}$ to be positive definite; $\boldsymbol{\Sigma}^{-1/2}$ is the matrix square root.)

---

## Part 6: Bivariate Gaussian (2D intuition)

### Special Case: $p=2$

Let $\mathbf{X} = \begin{pmatrix} X_1 \\ X_2 \end{pmatrix}$, $\boldsymbol{\mu} = \begin{pmatrix} \mu_1 \\ \mu_2 \end{pmatrix}$, $\boldsymbol{\Sigma} = \begin{pmatrix} \sigma_1^2 & \rho\sigma_1\sigma_2 \\ \rho\sigma_1\sigma_2 & \sigma_2^2 \end{pmatrix}$

where $\rho$ is the **correlation coefficient** ($-1 \leq \rho \leq 1$).

### Visualization
- **$\rho = 0$ (uncorrelated):** Ellipse aligned with axes
- **$\rho > 0$ (positive correlation):** Ellipse tilted up-right
- **$\rho < 0$ (negative correlation):** Ellipse tilted down-right
- **$|\rho| = 1$ (perfect correlation):** Ellipse collapses to a line

### Conditional Mean (Regression)
$$E[X_2 | X_1 = x_1] = \mu_2 + \rho \frac{\sigma_2}{\sigma_1}(x_1 - \mu_1)$$

This is a **linear regression** of $X_2$ on $X_1$! The slope is $\rho \frac{\sigma_2}{\sigma_1}$.

---

## Part 7: Common Operations

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

## Part 8: Connection to Least Squares

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

---

## Related ESL Chapters

- **Ch. 4 (Classification):** LDA, QDA assume Gaussian per-class densities
- **Ch. 8 (EM, Bayesian):** Mixture models, missing data, Bayesian inference with Gaussians
- **Ch. 13 (Prototypes, Clustering):** Gaussian mixtures
- **Throughout:** Regression models assume Gaussian errors
