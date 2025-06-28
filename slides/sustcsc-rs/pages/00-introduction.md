---
level: 2
---
# Challenge Overview

在量子计算蓬勃发展的时代，传统的密码学方法正面临威胁。

有远见的科学家们开发了新的"后量子"密码系统，其中一种基于 **带错误学习（LWE）** 问题。

LWE 的安全性依赖于解决涉及格的某些数学问题的假定困难性。

<div class="text-right text-sm opacity-70 mt-4">-- Google Gemini</div>

---
level: 2
---
# Learning With Errors (LWE)

带错误学习（LWE）是基于格的密码学中的一个基础问题。

<v-click>

南科大的学生应该已经修过线性代数课程，所以我们不会在这里重复线性代数的基础知识。

</v-click>

<v-click>

**整数格** 是在 n 维空间中生成离散网格的一组基向量。格中的所有向量都表示为基向量的整数线性组合。

</v-click>

---
level: 2
---
# LWE Construction

然而，如果我们使用以下方式构造一个向量：

$$
\mathbf{v} = \mathbf{A} \cdot \mathbf{x} + \mathbf{e}
$$

<v-click>

其中：
- **A** 是一个矩阵（格）
- **x** 是一个秘密值向量  
- **e** 是一个小的错误向量

</v-click>

<v-click>

在给定 **v**、**A** 和 **e** 的情况下，很难找到 **x**，因为你只能猜测或遍历 **x** 的每种组合，这对于大维度来说在计算上是不可行的。

</v-click>

---
level: 2
---
# Why LWE is Secure

<v-click>

有人可能会说：好吧，我们可以使用高斯消元法来解这个线性系统...

</v-click>

<v-click>

**但是** 错误向量 **e** 会在系统中累积错误，使得找到精确解变得不可能。

</v-click>

<v-click>

错误向量很小，但对系统来说是灾难性的。

</v-click>

<v-click>

**这就是 LWE 问题的安全性所在。**

</v-click>

---
level: 2
---
# Fully Homomorphic Encryption (FHE)

<br>
将 LWE 应用于密码学时，我们可以将秘密放在向量 **x** 中，并且我们可以对密文进行加法运算，因为假设：

<div>

$$
    b_1 = A \cdot x_1 + e_1
$$

$$
    b_2 = A \cdot x_2 + e_2
$$

</div>

<div>

如果我们将它们相加：

$$
b_1 + b_2 = A \cdot (x_1 + x_2) + (e_1 + e_2) = A \cdot (x_1 + x_2) + e^{\prime}
$$

</div>

---
level: 2
---
# FHE Implementation Issue

<v-click>

但是将 **e** 作为秘钥很奇怪，我们必须将秘钥相加才能恢复原始秘密。

</v-click>

<v-click>

我们怎么知道对密文进行了什么操作？所以这是不适用的。

</v-click>

<v-click>

最常见的方法是使用向量 **x** 作为秘钥，并将明文放在错误向量 **e** 中：

$$
    b = A \cdot x + e + m * \Delta
$$

</v-click>

<v-click>

这样我们就有 **(A, b)** 作为公钥，**x** 作为秘钥。

</v-click>

---
level: 2
---
# TFHE-rs Implementation

实际的实现会更复杂，例如：

<img src="https://cdn.prod.website-files.com/622ef9de9152c97467eac748/6734ce8356f7618c99eca317_626beffc06ae641cb1dc6078_GLWE_encryption.png" class="w-96 mx-auto">

这是 `tfhe-rs` 选择的 GLWE。

---
level: 2
---
# TFHE-rs Decryption

对于解密，这很简单：

<img src="https://cdn.prod.website-files.com/622ef9de9152c97467eac748/6734ce8356f7618c99eca314_626bf01306ae64175cdc611a_GLWE_decryption.png" class="w-96 mx-auto">

---
level: 2
---
# Ciphertext Structure

密文可能看起来像：

<img src="https://cdn.prod.website-files.com/622ef9de9152c97467eac748/6734ce8356f7618c99eca2f9_626bf08f2e754fa2b3537f09_GLWE_encoding_coefficient.png" class="w-96 mx-auto">

---
level: 2
---
# Homomorphic Addition

将它们相加就像：

<img src="https://cdn.prod.website-files.com/622ef9de9152c97467eac748/62754a57b2e64bf8df5c8b47_LWE_encode_msb_ADD.png" class="w-96 mx-auto">

在数学上，我们有：

$$
    b_1 + b_2 = A \cdot x + (e_1 + e_2) + (m_1 + m_2) * \Delta
$$

<v-click>

只要错误小于增量，错误就是微不足道的。密文的加法性质称为 **同态加法**。

</v-click>

---
level: 2
---
# Fully Homomorphic Encryption

<v-click>

如果我们将计算性质扩展到加法之外，比如乘法、比较等，它就变成了 **全同态加密（FHE）**。

</v-click>

<v-click>

有了这个，我们可以将计算任务安全地交给不受信任的服务器，服务器可以在不知道明文的情况下计算结果。

</v-click>

<v-click>

**这不是很酷吗？** 🤩

</v-click>

---
level: 2
---
# Playing it with a Game of Life

在这个挑战中，我们将使用 LWE 实现一个简单的 **生命游戏**。

<v-click>

生命游戏是英国数学家约翰·霍顿·康威在 1970 年设计的一个细胞自动机。

</v-click>

<v-click>

它由一个由可以 **存活** 或 **死亡** 的细胞组成的网格，每个细胞的状态根据其邻居的状态而改变。

</v-click>

<v-click>

然而，由于加密/解密/同态操作，FHE 比预期的更昂贵，所以我们需要优化生命游戏以使其高效。

</v-click>

<v-click>

幸运的是，生命游戏是 **天然可并行化的**。🚀

</v-click>

---
level: 2
---
# Cracking LWE (Bonus)

完成 _FHE 生命游戏_ 后，我们现在可以尝试破解 LWE 问题。

<v-click>

由于我们稍微缩小了参数，这并不是那么困难。这个挑战的重点是理解如何加速一个你可能一点都不了解的问题。

</v-click>

<v-click>

当我们说优化它时，我们是指：

1. **理解** 问题并研究能够解决它的算法
2. **实现** 算法，使其高效且可扩展，并且使用 Rust
3. **优化** 使用并行、SIMD 和其他技术的实现

</v-click>

<v-click>

为了破解 LWE 问题，我们将在给定公钥 **(A, b)** 的情况下找到秘密向量 **x**。

</v-click>

---
level: 2
---
# Closest Vector Problem (CVP)

<v-click>

破解 LWE 的一个直观方法是将其归约为 **最近向量问题（CVP）**。

</v-click>

<v-click>

CVP 定义了在格中找到最接近某个向量的向量的问题。

</v-click>

<v-click>

它的表述如下：给定一个不在格中的向量 **w**，找到格中的一个向量 **v**，使得 **w** 和 **v** 之间的距离最小。距离通常用欧几里得范数来衡量。

</v-click>

---
level: 2
---
# Babai's Nearest Plane Algorithm

通常，当维度较小时，Babai 的最近平面算法可以解决这个问题：

<v-click>

1. 使用 **LLL**（Lenstra–Lenstra–Lovász 格基约化）找到一个足够正交的基

</v-click>

<v-click>

2. 令 `b = t`（t 是加密的向量）

</v-click>

<v-click>

3. 对于 j 从 n 到 1，运行：
   ```
   b = b - B[j] * c[j]
   ```
   其中 `c = round(b * hat(B[j]) / (hat(B[j]) * hat(B[j])))`
   
   （hat(B[j]) 表示 B[j] 的 Gram-Schmidt，B[j] 是 LLL 约化后的第 j 个基向量）

</v-click>

<v-click>

4. 返回 `t - b`

</v-click>

---
level: 2
---
# Working with Modular Arithmetic

然而，在模 **q** 下，我们需要做一些技巧来使其工作。

<v-click>

由于：
$$
    b = A \cdot x + e \mod q
$$

</v-click>

<v-click>

我们有：
$$
    A x + q I_m k = b - e
$$

</v-click>

<v-click>

所以为了找到 **x**，我们可以构造一个新的基：
$$
    [A | q I_m] \begin{bmatrix} x \\ k \end{bmatrix} = b - e = b^{\prime}
$$

</v-click>

<v-click>

我们不需要关心解中的 **k**。最后一步是对以下进行高斯消元：
$$
    A x = b^{\prime} \mod q
$$

这是微不足道的。

</v-click>