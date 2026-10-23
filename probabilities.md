# Probabilities
**Source:** Bishop & Bishop, *Deep Learning: Foundations and Concepts* — Ch. 2
**Focus:** Foundational probability concepts

---

## Expected Value: Reconciling $\int x f(x)\,dx$ vs. $\int x\,dP(x)$

Both are the same object — one is a special case of the other, not a competing definition.

### The General Definition

Works for any random variable: discrete, continuous, or a mix.
$$E[X] = \int_\Omega X(\omega)\, dP(\omega) = \int_{\mathbb{R}} x \, dP(x)$$

$dP(x)$ means: integrate $x$ against the probability measure $P$ itself (technically the **pushforward** of $P$ onto $\mathbb{R}$, but usually still called $P$). This is a Lebesgue integral with respect to a measure — no density required. Equivalently written $\int x\, dF(x)$, a **Lebesgue–Stieltjes integral** against the CDF $F$.

### The Density Version Is a Special Case

If $P$ (or $F$) is **absolutely continuous** — i.e. it has a density — the Radon–Nikodym theorem guarantees a function $f$ such that
$$dP(x) = f(x)\, dx$$
as a literal substitution. Plugging into the general formula:
$$\int x\, dP(x) = \int x f(x)\, dx$$
So $\int x f(x)\, dx$ is not a separate definition — it's what $\int x\, dP(x)$ reduces to **when a density exists**.

### Why They Look Unrelated

The two notations pull different levers:
- $\int x\, dP(x)$ is always valid — continuous, discrete, or neither.
- If $X$ is **discrete**, $P$ has no density $f$ at all (it's a sum of point masses, not absolutely continuous w.r.t. Lebesgue measure). There, $\int x\, dP(x)$ collapses instead to $\sum_i x_i\, P(X = x_i)$ — the familiar discrete expectation.

### Hierarchy

$$\int x\,dP(x) \quad\text{(always valid)} \;\;\longrightarrow\;\; \begin{cases} \int x f(x)\,dx & \text{if } P \text{ has a density } f \\ \sum_i x_i p_i & \text{if } P \text{ is discrete} \end{cases}$$

Same quantity in every case — the notation just adapts to what kind of distribution is being integrated against.
