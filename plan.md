# Implementation Plan (draft)

Language: Rust, f32. Checks: Python (numpy/scipy) against Rust output dumped to CSV/JSON. Compare with relative tolerance ~1e-5, not exact equality.

Source of topics: stat101.md and tracker/ steps. Cross out anything that isn't wanted.

## Linear algebra basics
- [x] Matrix multiply
- [x] Transpose
- [x] Solve a linear system (Gaussian elimination)
- [x] Inverse of a small matrix

## Statistics from stat101.md
- [ ] Sample mean and variance (n-1 divisor)
- [ ] Sample covariance matrix (stat101 §2, §3)
- [ ] Linear combinations: a^T Σ b (stat101 §3)
- [ ] Covariance transformation A Σ A^T (stat101 §3)
- [ ] Bivariate and multivariate Gaussian density (stat101 §5)
- [ ] Mahalanobis distance (stat101 §5)
- [ ] Whitening with Σ^{-1/2} (requires eigendecomposition or a matrix square root)
- [ ] Bias and MSE of an estimator, simulated (stat101 §10)

## Regression and prediction (ESL ch.2 material)
- [ ] Least squares via normal equations
- [ ] k-nearest neighbors
- [ ] Train vs test error on simulated data

## Optimization
- [ ] Gradient descent on a least-squares loss

## Open questions (park with exact question)
- Eigendecomposition: needed for whitening and PCA. Implement Jacobi or use a crate?
- Crate use: ndarray for storage, or flat Vec<f32> with shape?
