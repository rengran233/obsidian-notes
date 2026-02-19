在 Java 异常处理中，`throw` 和 `throws` 虽仅一字之差，但其**本质、位置和功能**完全不同。可以形象地理解为：`throw` 是**“引爆炸弹”**，而 `throws` 是**“贴危险标签”**。

以下是详细的对比分析：

### 1. 核心本质区别

| 特性 | **`throw` (动作)** | **`throws` (声明)** |
| :--- | :--- | :--- |
| **本质** | **抛出**异常的实际**动作**。 | **声明**方法可能产生的**风险**。 |
| **位置** | 位于 **方法体内部**。 | 位于 **方法签名（参数列表后）**。 |
| **操作对象** | 后面跟着的是 **异常的对象实例**。 | 后面跟着的是 **异常的类型名称**。 |
| **执行次数** | 每次只能抛出 **1 个** 异常实例。 | 可以声明 **多个** 异常类（用逗号隔开）。 |
| **效果** | 一旦执行，程序立即**中断**（除非被 catch）。 | 告知调用者必须处理这些异常。 |

---

### 2. 详细功能解析

#### **`throw`：主动出击**
`throw` 用于在代码逻辑中发现错误时，**显式地、主动地**抛出一个异常。
*   **用途**：用于校验参数、处理逻辑错误或转发捕获的异常。
*   **语法**：`throw new XxxException("错误信息");`

#### **`throws`：风险预警**
`throws` 用于方法签名中，作为一种“免责声明”，告诉调用者：这个方法内部可能会出问题，你自己看着办。
*   **用途**：实现异常的**向上传播**（Propagate）。
*   **强制性**：对于“受检异常”（Checked Exception），如果方法内部不处理（用 try-catch），则**必须**在签名上用 `throws` 声明。

---

### 3. 代码实例对比

```java
public class ExceptionDemo {
    
    // throws: 声明这个方法可能会抛出异常（危险标签）
    public static void checkAge(int age) throws IllegalArgumentException, Exception {
        if (age < 0) {
            // throw: 实际引爆异常（动作）
            throw new IllegalArgumentException("年龄不能为负数！");
        }
        System.out.println("年龄合法：" + age);
    }

    public static void main(String[] args) {
        try {
            checkAge(-5);
        } catch (Exception e) {
            System.err.println("捕获到错误: " + e.getMessage());
        }
    }
}
```

---

### 4. 深度联系：受检异常 vs 运行时异常

这两个关键字的配合方式受异常类型的影响：

1.  **受检异常 (Checked Exception)**（如 `IOException`）：
    *   如果你在方法里 `throw` 一个受检异常，你**必须**在方法头上 `throws` 它，或者用 `try-catch` 解决掉。否则编译器不让通过。
    *   **本质原因**：编译器强制要求开发者处理那些“外部环境不可控”的错误。

2.  **运行时异常 (Unchecked/Runtime Exception)**（如 `NullPointerException`）：
    *   你可以 `throw` 它，但**不需要**在方法头上 `throws` 声明。
    *   **本质原因**：这些异常通常是代码逻辑 Bug，Java 认为应该由程序员修复逻辑，而不是通过频繁声明来处理。

---

### 总结建议

*   **`throw` 怎么用？**：当你的代码逻辑走不通了（比如用户传了个空文件，或者余额不足），用 `throw` 把错误甩出去。
*   **`throws` 怎么用？**：当你写一个底层的、通用的工具方法时，不要在内部把异常“吞掉”（即不要随便写空的 catch），而是用 `throws` 抛给上层业务逻辑去决定如何展示给用户。

**本质逻辑总结：**
`throw` 是**抛出异常的源头**；`throws` 是**异常向上传递的通道**。