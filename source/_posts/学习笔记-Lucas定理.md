---
title: '[学习笔记] Lucas定理'
categories:
  - OI
tags:
  - Lucas定理
  - 数论
date: 2020-08-09 20:57:28
widgets: null
thumbnail: https://cdn.luogu.com.cn/upload/image_hosting/65en13ki.png
---

## 定理

### 一般形式

$$
\binom{n}{m} \equiv \prod_{i = 0}^{k} \binom{n_i}{m_i} \pmod{p}
$$

其中 $m_{k}m_{k - 1} \cdots m_{2}m_{1}m_{0}$ 为 $m$ 的 $p$ 进制表示, 即 $m = \prod_{i = 0}^{k} m_{i} p^{i}$.

### 递归形式

$$
\binom{n}{m} \equiv \binom{\lfloor n / p \rfloor }{\lfloor m / p \rfloor} \cdot \binom{n \bmod{p}}{m \bmod{p}} \pmod{p}
$$

---

## 证明

考虑二项式定理
$$
(1 + x) ^ {n} = \prod_{i = 0}^{n} \binom{n}{i} x^{i}
$$
我们要求的 $\binom{n}{m}$ 即为 $x ^ {m}$ 的系数.

设


$$
\begin{aligned}
q_{x} &= \lfloor x / p \rfloor	\\
r_{x} &= x \bmod{p}	\\
\end{aligned}
$$
那么
$$
\begin{aligned}
(1 + x) ^ {n} &= (1 + x) ^ {q_{n}p} (1 + x) ^ {r_n}	\\
			  &= [(1 + x) ^ {p}] ^ {q_{n}} (1 + x) ^ {r_n}	\\
\end{aligned}
$$
考虑如何化简 $(1 + x) ^ p$.

对于 $\binom{p}{i} = \frac{p!}{i! \cdot (p - i)!}$, 易知在 $\bmod p$ 意义下时, 只有当 $i = 0$ 或 $i = p$ 时 $\binom{p}{i} \equiv 1$, 其他情况下 $\binom{n}{i} \equiv 0$. (因为当 $i \neq 0$ 且 $i \neq p$ 时, 分子中的因数 $p$ 无法被消去)

即
$$
\binom{p}{i} \equiv
\begin{cases}
0 \pmod{p},& i \neq 0 \ 且\  i \neq n.	\\
1 \pmod{p},& i = 0 \ 或\  i = n.	\\
\end{cases}
$$
那么根据二项式定理
$$
\begin{aligned}
(1 + x) ^ {p} &\equiv \sum_{i = 0} ^ {p} \binom{p}{i} x ^ {i}	\\
			  &\equiv 1 + x ^ {p} \pmod{p}	\\	
\end{aligned}
$$
所以
$$
\begin{aligned}
(1 + x) ^ {n}  &= [(1 + x) ^ {p}] ^ {q_{n}} (1 + x) ^ {r_n}	\\
			   &\equiv (1 + x ^ p) ^{q_n} (1 + x) ^{r_n}	\\
			   &\equiv \left[ \sum_{i = 0}^{q_n} \binom{q_n}{i} x^{pi} \right] \left[ \sum_{i = 0}^{r_n} \binom{r_n}{i} x^i \right]
\end{aligned}
$$
我们假设 $r_m \le r_n$, 因为 $r_m < p$, 所以 $m$ 可以被唯一地表示为 $q_{m}p\ ＋\ r_{m}$, 所以 $x^m$ 的系数为 $\binom{q_n}{q_m} \cdot \binom{r_n}{r_m}$. 即
$$
\binom{n}{m} \equiv \binom{\lfloor n / p \rfloor}{\lfloor m / p \rfloor} \cdot \binom{n \bmod{p}}{m \bmod{p}} \pmod{p}
$$
得证.

而当 $r_m > r_n$ 时, $(1 + x) ^ n$ 在 $\bmod{p}$ 下的 $x^m$ 系数为 0, 所以 $\binom{n}{m} \equiv 0  \pmod{p}$.

---

## 代码

先预处理出 $< p$ 的组合数 (或阶乘和阶乘逆元), 然后递归求解. 时间复杂度为 $O(p+\log_p m)$.

```cpp
int Lucas(int n, int m) {
  if (m == 0) return 1;
  return (ll)Lucas(n / P, m / P) * C(n % P, m % P) % P;
};
```

