### A Brief Guide to Equivalent Infinitesimals

#### 1. Core Principle

An infinitesimal $f(x)$ is **equivalent** to $g(x)$ as $x \to 0$, written $f(x) \sim g(x)$, if:
$$ \lim_{x \to 0} \frac{f(x)}{g(x)} = 1 $$
This allows us to replace a complex function with a simpler one inside a limit, but only under specific rules.

#### 2. Common Equivalents (as $x \to 0$)

*   $\sin(x) \sim x$
*   $\tan(x) \sim x$
*   $\arcsin(x) \sim x$
*   $\arctan(x) \sim x$
*   $1 - \cos(x) \sim \frac{x^2}{2}$
*   $\ln(1+x) \sim x$
*   $e^x - 1 \sim x$
*   $(1+x)^\alpha - 1 \sim \alpha x$

#### 3. Rules for Application

**Rule 1: Products & Quotients (SAFE)**
Substitution is almost always safe for terms that are multiplied or divided.

*   **Example:** $\lim_{x \to 0} \frac{\tan(x)(e^x - 1)}{1 - \cos(x)} \sim \lim_{x \to 0} \frac{x \cdot x}{x^2/2} = 2$

**Rule 2: Sums & Differences (Requires Caution)**
This is where errors are most common. Do not blindly substitute.

*   **The Dominance Rule:** In a sum or difference, you can simplify only if one term is of a **lower order** (i.e., it goes to zero slower) than the other. The entire expression can be replaced by this single **dominant term**.

    *   **Safe Example:** $\lim_{x \to 0} \frac{\sin(x) + x^2}{x}$
        *   In the numerator, $\sin(x) \sim x$ (order 1) and $x^2$ (order 2).
        *   $\sin(x)$ is dominant. So, $\sin(x) + x^2 \sim \sin(x) \sim x$.
        *   The limit becomes $\lim_{x \to 0} \frac{x}{x} = 1$.

    *   **Unsafe Example (The Red Flag):** $\lim_{x \to 0} \frac{x - \sin(x)}{x^3}$
        *   In the numerator, $x$ (order 1) and $\sin(x) \sim x$ (order 1).
        *   They are the same order and their coefficients cancel ($1-1=0$).
        *   **DO NOT SUBSTITUTE**. This cancellation hides higher-order terms.

#### 4. The Escape Hatch: What to Do When Substitution Fails

If you encounter the "cancellation of same-order terms" red flag, you must use a more powerful method:

1.  **Taylor Series Expansion:** The most reliable method. Expand the functions to a high enough order so that a non-zero term remains after the main terms cancel.
    *   For the unsafe example: $x - \sin(x) = x - (x - \frac{x^3}{6} + \dots) \sim \frac{x^3}{6}$.
    *   The limit is $\lim_{x \to 0} \frac{x^3/6}{x^3} = \frac{1}{6}$.

2.  **L'Hôpital's Rule:** A great alternative if the limit is of the form $\frac{0}{0}$ or $\frac{\infty}{\infty}$.

---
**Final Checklist:**
1.  **Products/Quotients?** $\to$ Safe to substitute.
2.  **Sums/Differences?** $\to$ Check the order of the terms.
    *   **Different Orders?** $\to$ Safe to replace the whole expression with the dominant (lower-order) term.
    *   **Same Order?** $\to$ **DANGER**. If they cancel, use Taylor Series or L'Hôpital's Rule.

---

#### Additional
1. Why we can't replace infinitesimally equivalent quantities in sums or differences?