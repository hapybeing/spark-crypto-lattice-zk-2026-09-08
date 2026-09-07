# CRYPTA // Zero-Knowledge Proofs & Post-Quantum Lattice Playground

[![Deploy to GitHub Pages](https://github.com/hapybeing/spark-crypto-lattice-zk-2026-09-08/actions/workflows/deploy.yml/badge.svg)](https://github.com/hapybeing/spark-crypto-lattice-zk-2026-09-08/actions/workflows/deploy.yml)
[![License: MIT](https://img.shields.io/badge/License-MIT-emerald.svg)](https://opensource.org/licenses/MIT)
[![Tech Stack](https://img.shields.io/badge/Stack-Post--Quantum%20Lattice%20%7C%20ZK--Proofs%20%7C%20ECC%20%7C%20Tailwind%20%7C%20ES6%2B-cyan.svg)](#architecture)

> **An interactive cryptographic laboratory exploring post-quantum lattice encryption (LWE & Closest Vector Problem), zero-knowledge $\Sigma$-protocols (Schnorr discrete log), finite field elliptic curve group arithmetic ($\mathbb{F}_p$), and SHA-256 bitwise avalanche state dispersion.**

### 🌐 Live Production Deployment
**Explore the live interactive laboratory here:**  
[https://hapybeing.github.io/spark-crypto-lattice-zk-2026-09-08/](https://hapybeing.github.io/spark-crypto-lattice-zk-2026-09-08/)

---

## 🔬 Mathematical & Algorithmic Foundations

### 1. Learning With Errors (LWE) & Lattice Geometry
The post-quantum module implements Regev's Learning With Errors public-key encryption scheme over the ring $\mathbb{Z}_q$:
- **Key Generation**: Given public matrix $A \in \mathbb{Z}_q^{m \times n}$ and private secret vector $\mathbf{s} \in \mathbb{Z}_q^n$, sample small error vector $\mathbf{e} \leftarrow \chi_\sigma$. The public key is:
  $$\mathbf{b} = A\mathbf{s} + \mathbf{e} \pmod q$$
- **Encryption**: To encrypt plaintext bit $m \in \{0, 1\}$, sample random vector $\mathbf{r} \in \{0, 1\}^m$:
  $$\mathbf{c}_1 = A^T \mathbf{r} \pmod q, \quad c_2 = \mathbf{b}^T \mathbf{r} + m \cdot \left\lfloor \frac{q}{2} \right\rfloor \pmod q$$
- **Decryption**: Compute phase difference:
  $$c_2 - \mathbf{s}^T \mathbf{c}_1 = \mathbf{e}^T \mathbf{r} + m \cdot \left\lfloor \frac{q}{2} \right\rfloor \pmod q$$
  If $\|\mathbf{e}^T \mathbf{r}\| < q/4$, Babai's nearest-plane rounding decodes bit $m$ without error.

### 2. Schnorr Zero-Knowledge Identity ($\Sigma$-Protocol)
Allows a Prover (Peggy) to prove to a Verifier (Victor) knowledge of discrete log $x$ such that $y = g^x \pmod p$ without leaking any bits of $x$:
1. **Commitment**: Peggy samples random nonce $r \in_R \mathbb{Z}_{p-1}$ and sends $t = g^r \pmod p$.
2. **Challenge**: Victor sends random challenge scalar $c \in \mathbb{Z}_{p-1}$.
3. **Response**: Peggy computes $s = r + c \cdot x \pmod{p-1}$.
4. **Verification**: Victor checks that $g^s \equiv t \cdot y^c \pmod p$.
- **Completeness**: If Peggy knows $x$, $g^s = g^{r + cx} = g^r \cdot (g^x)^c = t \cdot y^c \pmod p$.
- **Special Soundness**: Given two valid transcripts with differing challenges, $x$ can be extracted efficiently.
- **Zero-Knowledge**: A simulator can forge valid transcripts without $x$ by choosing $s$ and $c$ first and setting $t = g^s \cdot y^{-c}$.

### 3. Weierstrass Elliptic Curve Arithmetic over $\mathbb{F}_p$
Points satisfying $y^2 \equiv x^3 + ax + b \pmod p$ form an abelian group with group law:
- **Point Addition** ($P \ne Q$):
  $$\lambda = \frac{y_2 - y_1}{x_2 - x_1} \pmod p$$
  $$x_3 = \lambda^2 - x_1 - x_2 \pmod p, \quad y_3 = \lambda(x_1 - x_3) - y_1 \pmod p$$
- **Point Doubling** ($P = Q$):
  $$\lambda = \frac{3x_1^2 + a}{2y_1} \pmod p$$
  $$x_3 = \lambda^2 - 2x_1 \pmod p, \quad y_3 = \lambda(x_1 - x_3) - y_1 \pmod p$$

### 4. SHA-256 Bitwise Avalanche Effect
Visualizes the Merkle-Damgård round function operating on 8 32-bit state registers $(A, B, C, D, E, F, G, H)$ across 64 compression rounds using $\Sigma_0, \Sigma_1, \text{Ch}, \text{Maj}$ nonlinear boolean operations:
$$\text{Ch}(x, y, z) = (x \wedge y) \oplus (\neg x \wedge z)$$
$$\text{Maj}(x, y, z) = (x \wedge y) \oplus (x \wedge z) \oplus (y \wedge z)$$
Flipping a single input bit generates complete 50% entropy diffusion across all 256 output bits.

---

## 🎛️ Feature & Module Architecture

| Module | Core Functionality |
| :--- | :--- |
| **01 // LWE Lattice Matrix** | Interactive 2D Euclidean lattice rendering basis vectors, secret vector point, noise error balls, and ciphertext nearest-plane decoding. |
| **02 // Zero-Knowledge $\Sigma$** | 3-phase interactive cryptographic protocol stepper (Commitment, Challenge, Response, Verification) with simulated cheater detection. |
| **03 // Elliptic Curve** | Real discrete $\mathbb{F}_p$ affine coordinate canvas, chord and tangent lines, and point addition $P + Q = R$. |
| **04 // SHA-256 Heatmap** | 16x16 bitwise matrix comparison highlighting active diffusion flips between two user-supplied strings. |

---

## 🕹️ Interaction Guide

- **Tab Navigation**:
  - `[1]`: LWE Lattice Cryptosystem
  - `[2]`: Zero-Knowledge Proofs
  - `[3]`: Elliptic Curve Arithmetic
  - `[4]`: SHA-256 Avalanche Matrix
- **Global Shortcuts**:
  - `[R]`: Re-sample noise / re-execute active cryptographic cipher

---

## 🏗️ Architecture & Technical Stack

- **Single Production-Grade Bundle**: Self-contained `index.html` with zero build-tooling dependencies.
- **Modern CDN Dependencies**:
  - Tailwind CSS via CDN
  - Lucide Icons (UMD CDN)
  - Canvas-Confetti
  - Google Fonts (Inter & Geist Mono)
- **Deployment**: Automated GitHub Actions workflow pushing directly to GitHub Pages.

---

## 📄 License
Open source under the [MIT License](LICENSE).
