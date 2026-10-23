---
name: matrix-differentiation-basics
description: Foundational rules for differentiating with respect to vectors and matrices
metadata:
  type: reference
---

# Matrix Differentiation Basics

**Goal:** Learn the core rules for taking derivatives with respect to vectors/matrices. These appear constantly in machine learning.

---

## Part 1: Quick Review — Scalar Derivatives

You know this already, but it sets up the pattern.

### Single Variable (scalar $x$)
$$\frac{d}{dx}(x^2) = 2x$$

$$\frac{d}{dx}(ax + b) = a \quad \text{(linear)}$$

### Multiple Variables (gradient)

For a function $f(x_1, x_2, x_3)$, the **gradient** is:
$$\nabla f = \begin{pmatrix} \frac{\partial f}{\partial x_1} \\ \frac{\partial f}{\partial x_2} \\ \frac{\partial f}{\partial x_3} \end{pmatrix}$$

Each element is a partial derivative.

**Example:**
$$f(x_1, x_2) = 3x_1^2 + 2x_1 x_2 + x_2$$

$$\nabla f = \begin{pmatrix} 6x_1 + 2x_2 \\ 2x_1 + 1 \end{pmatrix}$$

---

## Part 2: Derivatives with Respect to Vectors

### Setup
- Let $\mathbf{x} = \begin{pmatrix} x_1 \\ x_2 \\ \vdots \\ x_p \end{pmatrix}$ be a column vector
- Let $f(\mathbf{x})$ be a scalar-valued function (input is a vector, output is a number)
- We want $\frac{\partial f}{\partial \mathbf{x}}$

### Definition

$$\frac{\partial f}{\partial \mathbf{x}} = \begin{pmatrix} \frac{\partial f}{\partial x_1} \\ \frac{\partial f}{\partial x_2} \\ \vdots \\ \frac{\partial f}{\partial x_p} \end{pmatrix}$$

This is a **column vector of partial derivatives** — just the gradient in vector notation.

### Rule 1: Linear Function
$$f(\mathbf{x}) = \mathbf{c}^T \mathbf{x} = c_1 x_1 + c_2 x_2 + \cdots + c_p x_p$$

Then:
$$\frac{\partial f}{\partial \mathbf{x}} = \mathbf{c}$$

**Why?** Each term $c_i x_i$ gives partial derivative $c_i$ with respect to $x_i$.

**Example:**
$$f(\mathbf{x}) = 5x_1 + 2x_2 = \begin{pmatrix} 5 \\ 2 \end{pmatrix}^T \begin{pmatrix} x_1 \\ x_2 \end{pmatrix}$$

$$\frac{\partial f}{\partial \mathbf{x}} = \begin{pmatrix} 5 \\ 2 \end{pmatrix}$$

---

### Rule 2: Quadratic Form (The Tricky One)
$$f(\mathbf{x}) = \mathbf{x}^T A \mathbf{x}$$

where $A$ is a square matrix (assume symmetric for now).

Then:
$$\frac{\partial f}{\partial \mathbf{x}} = 2A \mathbf{x}$$

**Why is this true? Expand it out:**

Let's do a simple 2D example. Say $\mathbf{x} = \begin{pmatrix} x_1 \\ x_2 \end{pmatrix}$ and $A = \begin{pmatrix} a & b \\ b & c \end{pmatrix}$.

$$\mathbf{x}^T A \mathbf{x} = \begin{pmatrix} x_1 & x_2 \end{pmatrix} \begin{pmatrix} a & b \\ b & c \end{pmatrix} \begin{pmatrix} x_1 \\ x_2 \end{pmatrix}$$

First, $A \mathbf{x} = \begin{pmatrix} ax_1 + bx_2 \\ bx_1 + cx_2 \end{pmatrix}$

Then, $\mathbf{x}^T A \mathbf{x} = x_1(ax_1 + bx_2) + x_2(bx_1 + cx_2)$
$$= ax_1^2 + bx_1 x_2 + bx_1 x_2 + cx_2^2 = ax_1^2 + 2bx_1 x_2 + cx_2^2$$

Now take partial derivatives:
$$\frac{\partial}{\partial x_1}(ax_1^2 + 2bx_1 x_2 + cx_2^2) = 2ax_1 + 2bx_2$$

$$\frac{\partial}{\partial x_2}(ax_1^2 + 2bx_1 x_2 + cx_2^2) = 2bx_1 + 2cx_2$$

So:
$$\frac{\partial f}{\partial \mathbf{x}} = \begin{pmatrix} 2ax_1 + 2bx_2 \\ 2bx_1 + 2cx_2 \end{pmatrix} = 2 \begin{pmatrix} a & b \\ b & c \end{pmatrix} \begin{pmatrix} x_1 \\ x_2 \end{pmatrix} = 2A\mathbf{x}$$

**Key insight:** Quadratic forms always give $2A\mathbf{x}$.

---

### Proof: Why $\frac{\partial}{\partial \mathbf{x}}(\mathbf{x}^T A \mathbf{x}) = 2A\mathbf{x}$ (General Case)

Let me prove this rigorously using summation notation.

**Setup:**
- $\mathbf{x} = (x_1, x_2, \ldots, x_p)^T$ is a column vector
- $A$ is a $p \times p$ matrix with entries $a_{ij}$
- We want to find $\frac{\partial}{\partial x_k}(\mathbf{x}^T A \mathbf{x})$ for an arbitrary component $x_k$

**Step 1: Expand $\mathbf{x}^T A \mathbf{x}$ in summation notation**

$$\mathbf{x}^T A \mathbf{x} = \sum_{i=1}^p \sum_{j=1}^p x_i a_{ij} x_j$$

This is just the definition: multiply each row of $\mathbf{x}^T$ by the corresponding entry in $A\mathbf{x}$.

**Step 2: Take the partial derivative with respect to $x_k$**

We differentiate the double sum term-by-term. Most terms don't involve $x_k$, so they vanish. Only terms with $x_k$ survive:

$$\frac{\partial}{\partial x_k} \sum_{i=1}^p \sum_{j=1}^p x_i a_{ij} x_j$$

A term $x_i a_{ij} x_j$ involves $x_k$ only if:
- $i = k$ (the first $x_i = x_k$), OR
- $j = k$ (the second $x_j = x_k$)

**Case 1: $i = k$**

$$\frac{\partial}{\partial x_k}(x_k a_{kj} x_j) = a_{kj} x_j$$

Summing over all $j$: $\sum_{j=1}^p a_{kj} x_j$

**Case 2: $j = k$**

$$\frac{\partial}{\partial x_k}(x_i a_{ik} x_k) = x_i a_{ik}$$

Summing over all $i$: $\sum_{i=1}^p x_i a_{ik}$

**Step 3: Combine both cases**

$$\frac{\partial}{\partial x_k}(\mathbf{x}^T A \mathbf{x}) = \sum_{j=1}^p a_{kj} x_j + \sum_{i=1}^p x_i a_{ik}$$

**Step 4: Recognize these as matrix products**

- First sum: $\sum_{j=1}^p a_{kj} x_j$ is the $k$-th component of $A\mathbf{x}$
- Second sum: $\sum_{i=1}^p x_i a_{ik}$ is the $k$-th component of $A^T \mathbf{x}$

So:
$$\frac{\partial}{\partial x_k}(\mathbf{x}^T A \mathbf{x}) = (A\mathbf{x})_k + (A^T \mathbf{x})_k$$

where subscript $k$ means "the $k$-th component."

**Step 5: Assume $A$ is symmetric**

If $A = A^T$ (symmetric), then $A^T \mathbf{x} = A\mathbf{x}$, so:

$$\frac{\partial}{\partial x_k}(\mathbf{x}^T A \mathbf{x}) = 2(A\mathbf{x})_k$$

So:
$$\frac{\partial}{\partial x_k}(\mathbf{x}^T A \mathbf{x}) = 2(A\mathbf{x})_k$$

This holds for all components $k$, so stacking them into a vector:

$$\boxed{\frac{\partial}{\partial \mathbf{x}}(\mathbf{x}^T A \mathbf{x}) = 2A\mathbf{x} \quad \text{(when } A \text{ is symmetric)}}$$

**What if $A$ is not symmetric?**

The general formula is:
$$\boxed{\frac{\partial}{\partial \mathbf{x}}(\mathbf{x}^T A \mathbf{x}) = (A + A^T)\mathbf{x} \quad \text{(always true)}}$$

When $A$ is symmetric, $A + A^T = 2A$, so it reduces to our case.

**Why focus on symmetric?** In machine learning (ESL), $A$ is almost always symmetric:
- $A = X^T X$ (always symmetric by construction: $(X^T X)^T = X^T X$)
- $A = \nabla^2 f$ (Hessian, always symmetric for twice-differentiable functions)

So the symmetric case covers 99% of what you'll see. But the general formula $(A + A^T)\mathbf{x}$ works regardless.

---

## Part 3: Applying These Rules to Least Squares

Now let's see where the confusion comes from in the RSS derivation.

### Recall: RSS in Matrix Form

$$\text{RSS}(\beta) = (y - X\beta)^T(y - X\beta)$$

Expand:
$$= y^T y - y^T X \beta - \beta^T X^T y + \beta^T X^T X \beta$$

### Let's Differentiate Each Term with Respect to $\beta$

**Term 1:** $y^T y$
- This doesn't depend on $\beta$ at all (it's a constant)
- $\frac{\partial}{\partial \beta}(y^T y) = 0$

**Term 2:** $-y^T X \beta = -(Xy)^T \beta$
- This is a **linear function** of $\beta$: $\mathbf{c}^T \beta$ where $\mathbf{c} = -X^T y$
- By **Rule 1**: $\frac{\partial}{\partial \beta}(\mathbf{c}^T \beta) = \mathbf{c}$
- So: $\frac{\partial}{\partial \beta}(-y^T X \beta) = -X^T y$

**Term 3:** $-\beta^T X^T y$
- This is also **linear** in $\beta$: $\mathbf{d}^T \beta$ where $\mathbf{d} = -X^T y$
- By **Rule 1**: $\frac{\partial}{\partial \beta}(-\beta^T X^T y) = -X^T y$

**Note:** Term 2 and Term 3 are the same thing! The notation just looks different.

**Term 4:** $\beta^T X^T X \beta$
- This is a **quadratic form** with $A = X^T X$
- By **Rule 2**: $\frac{\partial}{\partial \beta}(\beta^T X^T X \beta) = 2 X^T X \beta$

### Putting It Together

$$\frac{\partial}{\partial \beta} \text{RSS}(\beta) = 0 + (-X^T y) + (-X^T y) + 2X^T X \beta$$

$$= -2X^T y + 2X^T X \beta$$

Set to zero:
$$-2X^T y + 2X^T X \beta = 0$$

$$X^T X \beta = X^T y$$

$$\beta = (X^T X)^{-1} X^T y$$

---

## Part 4: Common Rules (Reference Table)

| Function | Derivative w.r.t. $\mathbf{x}$ |
|----------|-------|
| $\mathbf{c}^T \mathbf{x}$ (linear) | $\mathbf{c}$ |
| $\mathbf{x}^T A \mathbf{x}$ (quadratic, $A$ symmetric) | $2A\mathbf{x}$ |
| $\\|\mathbf{x}\\|^2 = \mathbf{x}^T \mathbf{x}$ | $2\mathbf{x}$ |
| $\mathbf{x}^T A \mathbf{y}$ (linear in $\mathbf{x}$, $\mathbf{y}$ constant) | $A\mathbf{y}$ |
| $\text{tr}(A\mathbf{x}^T \mathbf{x})$ (trace, $A$ constant) | $2A\mathbf{x}$ |

---

## Part 5: Key Intuitions

### Why Matrix Differentiation?

When you have many parameters ($\beta = (\beta_1, \ldots, \beta_p)$), computing partial derivatives individually is tedious. Matrix notation compacts it.

### The Gradient as a Vector

When you differentiate a scalar with respect to a vector, you get **a vector of partial derivatives**. This vector points in the direction of steepest increase.

**For minimization:** Set $\frac{\partial f}{\partial \mathbf{x}} = 0$ to find critical points (local minima, maxima, saddle points).

### Why $\mathbf{c}^T \beta$ gives $\mathbf{c}$

Think of $\mathbf{c}^T \beta$ as:
$$c_1 \beta_1 + c_2 \beta_2 + \cdots + c_p \beta_p$$

When you differentiate with respect to $\beta_i$, you get $c_i$. Stack them: you get $\mathbf{c}$.

### Why $\beta^T A \beta$ gives $2A\beta$

A quadratic form $\beta^T A \beta$ expands to many cross-terms (like $2x_1 x_2$ in the 2D example). When you differentiate:
- Terms like $\beta_i^2$ give $2\beta_i$
- Cross-terms like $2a_{ij}\beta_i \beta_j$ give contributions from all $a_{ij}$ entries
- Everything recombines into $2A\beta$

---

## Practice Examples

### Example 1: Linear Function
$$f(\beta) = (y - X\beta)^T (y - X\beta) = y^T y - 2\beta^T X^T y + \beta^T X^T X \beta$$

Wait, let me simplify. Just take:
$$g(\beta) = -\beta^T X^T y = -(X^T y)^T \beta$$

What is $\frac{\partial g}{\partial \beta}$?

**Solution:**
This is linear in $\beta$ with $\mathbf{c} = -X^T y$.
$$\frac{\partial g}{\partial \beta} = -X^T y$$

### Example 2: Quadratic Form
$$h(\beta) = \beta^T X^T X \beta$$

What is $\frac{\partial h}{\partial \beta}$?

**Solution:**
This is a quadratic form with $A = X^T X$.
$$\frac{\partial h}{\partial \beta} = 2 X^T X \beta$$

### Example 3: Full RSS (Putting It Together)
$$\text{RSS}(\beta) = y^T y - 2\beta^T X^T y + \beta^T X^T X \beta$$

What is $\frac{\partial \text{RSS}}{\partial \beta}$?

**Solution:**
- $\frac{\partial}{\partial \beta}(y^T y) = 0$
- $\frac{\partial}{\partial \beta}(-2\beta^T X^T y) = -2X^T y$
- $\frac{\partial}{\partial \beta}(\beta^T X^T X \beta) = 2X^T X \beta$

Therefore:
$$\frac{\partial \text{RSS}}{\partial \beta} = -2X^T y + 2X^T X \beta$$

Set to zero: $X^T X \beta = X^T y$, so $\hat{\beta} = (X^T X)^{-1} X^T y$.

---

---

## Part 6: What About Vector/Matrix-Valued Functions?

### The Question
So far we've only differentiated **scalar-valued** functions (input: vector, output: number). What if the output is itself a vector or matrix?

### Vector-Valued Functions

If $\mathbf{f}(\mathbf{x}): \mathbb{R}^p \to \mathbb{R}^m$ (output is an $m \times 1$ vector), then the derivative is called the **Jacobian** matrix:

$$\frac{\partial \mathbf{f}}{\partial \mathbf{x}} = J = \begin{pmatrix} 
\frac{\partial f_1}{\partial x_1} & \frac{\partial f_1}{\partial x_2} & \cdots & \frac{\partial f_1}{\partial x_p} \\
\frac{\partial f_2}{\partial x_1} & \frac{\partial f_2}{\partial x_2} & \cdots & \frac{\partial f_2}{\partial x_p} \\
\vdots & \vdots & \ddots & \vdots \\
\frac{\partial f_m}{\partial x_1} & \frac{\partial f_m}{\partial x_2} & \cdots & \frac{\partial f_m}{\partial x_p}
\end{pmatrix}$$

This is $m \times p$ (one row per output, one column per input).

**Example:**
$$\mathbf{f}(\mathbf{x}) = A\mathbf{x} \quad (\text{matrix-vector product, where } A \text{ is } m \times p)$$

Then:
$$\frac{\partial \mathbf{f}}{\partial \mathbf{x}} = A$$

**Why?** Each component of $\mathbf{f}$ is linear in $\mathbf{x}$, so the Jacobian is just $A$ itself.

### Matrix-Valued Functions

If $F(\mathbf{x}): \mathbb{R}^p \to \mathbb{R}^{m \times n}$ (output is an $m \times n$ matrix), the derivative becomes **very complicated**. You need:
- **Vectorization** (flatten the matrix into a vector)
- **Kronecker products** (new object in linear algebra)
- **Stack/reshape operations**

This is where most people stop. ESL doesn't go here.

### Why Machine Learning Stays with Scalar Outputs

**Key insight:** When optimizing, we need a **single number** to minimize.

- **Loss function:** always scalar (e.g., RSS, cross-entropy)
- **Regularization term:** always scalar
- **Total objective:** sum scalar + scalar = scalar

So even if your intermediate computations produce matrices (like predictions $\hat{Y}$ or residuals), you **always reduce to a scalar** before optimizing.

**Example from least squares:**
- Predictions $\hat{Y}$: $N \times 1$ vector (matrix-valued as a function of $\beta$)
- Residuals $(y - \hat{Y})$: $N \times 1$ vector
- RSS $(y - \hat{Y})^T(y - \hat{Y})$: scalar

We differentiate the **scalar** RSS, not the vector predictions.

### When Do Vector/Matrix Derivatives Show Up?

They appear in a few cases in machine learning:

1. **Neural networks:** Computing Jacobian for sensitivity analysis (not used in ESL)
2. **Probabilistic models:** Some EM algorithms use matrix derivatives (briefly in Ch. 8)
3. **Advanced optimization:** Hessians and higher-order derivatives
4. **Control theory / dynamical systems:** Common there, not here

### Bottom Line for ESL

- Focus on **scalar-valued objective functions** (loss, regularized loss, likelihood)
- Differentiate those scalars with respect to parameters
- Get a vector of gradients (one per parameter)
- That's it

If you ever see a matrix output in ESL, it's either:
1. Summed/reduced to a scalar before differentiating, or
2. Not differentiated at all (just computed)

---

- Now go back to the **Least Squares Derivation** in Chapter 2 notes
- It should make sense using **Rule 1** (linear) and **Rule 2** (quadratic)
- When you see matrix differentiation in future chapters, refer back to this reference

