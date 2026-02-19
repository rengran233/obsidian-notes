以下是一些常用数学公式的 **LaTeX 写法**，可直接在 Markdown 中使用（需确保 Markdown 解析器支持 LaTeX，如 Typora、VS Code + Markdown Preview Enhanced 或 Jupyter Notebook）：

---

### **1. 基础符号**
| 类型  | LaTeX 代码                          | 效果                                  |
| --- | --------------------------------- | ----------------------------------- |
| 上下标 | `x^2`, `a_{n}`                    | \(x^2\), \(a_{n}\)                  |
| 分式  | `\frac{a}{b}`                     | \\(\frac{a}{b}\\)                   |
| 根号  | `\sqrt{x}`, `\sqrt[n]{x}`         | \(\sqrt{x}\), \(\sqrt[n]{x}\)       |
| 求和  | `\sum_{i=1}^n i^2`                | \(\sum_{i=1}^n i^2\)                |
| 积分  | `\int_{a}^{b} f(x) \, dx`         | \(\int_{a}^{b} f(x) \, dx\)         |
| 极限  | `\lim_{x \to 0} \frac{\sin x}{x}` | \(\lim_{x \to 0} \frac{\sin x}{x}\) |
| 导数  | `f'(x)` 或 `\frac{dy}{dx}`         | \(f'(x)\), \(\frac{dy}{dx}\)        |

---

### **2. 希腊字母**
| 符号        | LaTeX 代码        | 符号        | LaTeX 代码         |
|-------------|-------------------|-------------|--------------------|
| α           | `\alpha`          | β           | `\beta`            |
| θ           | `\theta`          | π           | `\pi`              |
| Δ           | `\Delta`          | Σ           | `\Sigma`           |

---

### **3. 矩阵与向量**
```latex
<!-- 矩阵 -->
$$
\begin{bmatrix}
1 & 2 \\
3 & 4
\end{bmatrix}
$$

<!-- 行列式 -->
$$
\begin{vmatrix}
a & b \\
c & d
\end{vmatrix}
$$

<!-- 向量 -->
$$
\mathbf{v} = \langle v_1, v_2 \rangle
$$
```
**效果**：
\[
\begin{bmatrix}
1 & 2 \\
3 & 4
\end{bmatrix}, \quad
\begin{vmatrix}
a & b \\
c & d
\end{vmatrix}, \quad
\mathbf{v} = \langle v_1, v_2 \rangle
\]

---

### **4. 方程与方程组**
```latex
<!-- 单行公式 -->
$$
E = mc^2
$$

<!-- 多行对齐 -->
$$
\begin{aligned}
f(x) &= (x+1)^2 \\
&= x^2 + 2x + 1
\end{aligned}
$$

<!-- 方程组 -->
$$
\begin{cases}
x + 2y = 5 \\
3x - y = 1
\end{cases}
$$
```
**效果**：
\[
E = mc^2
\]
\[
\begin{aligned}
f(x) &= (x+1)^2 \\
&= x^2 + 2x + 1
\end{aligned}
\]
\[
\begin{cases}
x + 2y = 5 \\
3x - y = 1
\end{cases}
\]

---

### **5. 概率与统计**
```latex
<!-- 期望 -->
$$
E[X] = \sum_{i=1}^n p_i x_i
$$

<!-- 正态分布 -->
$$
X \sim \mathcal{N}(\mu, \sigma^2)
$$

<!-- 条件概率 -->
$$
P(A \mid B) = \frac{P(A \cap B)}{P(B)}
$$
```
**效果**：
\[
E[X] = \sum_{i=1}^n p_i x_i, \quad
X \sim \mathcal{N}(\mu, \sigma^2), \quad
P(A \mid B) = \frac{P(A \cap B)}{P(B)}
\]

---

### **6. 特殊符号与运算符**
| 符号        | LaTeX 代码                   | 效果                   |
|-------------|------------------------------|-----------------------|
| 约等于       | `\approx`                    | \(\approx\)           |
| 不等于       | `\neq`                       | \(\neq\)              |
| 属于         | `\in`                        | \(\in\)               |
| 梯度         | `\nabla f`                   | \(\nabla f\)          |
| 偏导数       | `\frac{\partial f}{\partial x}` | \(\frac{\partial f}{\partial x}\) |

---

### **7. 公式环境**
```latex
<!-- 行内公式 -->
这是行内公式：\( \sum_{i=1}^n i = \frac{n(n+1)}{2} \)

<!-- 块级公式 -->
$$
\forall x \in \mathbb{R}, \quad e^{i\pi} + 1 = 0
$$
```

---

### **注意事项**
1. **Markdown 兼容性**：
   - 使用 `$$` 包裹块级公式，`\( \)` 包裹行内公式。
   - GitHub Markdown 默认不支持 LaTeX，需借助扩展（如 MathJax）或工具（如 Typora）。
2. **转义字符**：在部分平台（如 Jekyll）中，反斜杠 `\` 可能需要转义为 `\\`。
3. **公式编号**：如需编号，可使用 `\tag{}`：
   ```latex
   $$
   a^2 + b^2 = c^2 \tag{1}
   $$
   ```

如果需要更复杂的公式（如张量、量子力学符号等），可进一步提供具体场景！

$$
E = mc^2
$$
