# 📘 CATATAN BELAJAR – Number Theory & Modular Arithmetic

---

## 1️⃣ Modular Arithmetic

### 📌 Definisi

Modular arithmetic adalah sistem hitung berdasarkan sisa pembagian.

$$a \equiv b \pmod{n}$$

Artinya: **a** dan **b** punya sisa yang sama jika dibagi **n**.

### 📌 Contoh

```
17 % 5 = 2
```

$$17 \equiv 2 \pmod{5}$$

### 📌 Operasi Dasar

```python
(a + b) % n
(a * b) % n
(a ** k) % n
```

Python shorthand untuk modular exponentiation:

```python
pow(a, k, n)
```

---

## 2️⃣ Fermat's Little Theorem (Mod Prima)

Jika **p** prima dan `gcd(a, p) = 1`:

$$a^{p-1} \equiv 1 \pmod{p}$$

Dan:

$$a^{p} \equiv a \pmod{p}$$

### 📌 Contoh

Hitung $7^{999999} \mod 17$

Karena 17 prima:

```
999999 % 16 = 15
```

$$7^{999999} \equiv 7^{15} \pmod{17}$$

Dan hasilnya = **5**

### 📌 Insight Penting

Eksponen bisa direduksi:

$$a^k \equiv a^{k \bmod (p-1)} \pmod{p}$$

---

## 3️⃣ Modular Inverse

Cari **x** sehingga:

$$ax \equiv 1 \pmod{p}$$

### 📌 Contoh

$$7x \equiv 1 \pmod{17}$$

```
7 × 5 = 35 ≡ 1 mod 17
```

Jadi:

$$7^{-1} \equiv 5 \pmod{17}$$

---

## 4️⃣ Quadratic Residue

### 📌 Definisi

**x** adalah quadratic residue mod p jika:

$$a^2 \equiv x \pmod{p}$$

Artinya: x bisa didapat dari kuadrat suatu angka.

### 📌 Contoh (mod 29)

$$11^2 = 121$$

```
121 mod 29 = 5
```

Jadi:
- **5** adalah quadratic residue
- **11** adalah akar kuadratnya

### 📌 Akar Kuadrat Modular Selalu Ada Dua

Jika:

$$x^2 \equiv a \pmod{p}$$

Maka:

$$(p - x)^2 \equiv a \pmod{p}$$

**Contoh:**

```
8²  ≡ 6 mod 29
21² ≡ 6 mod 29
```

Karena: `29 − 8 = 21`

---

## 5️⃣ Euler Criterion – Cek Quadratic Residue Cepat

Jika **p** prima:

$$a^{(p-1)/2} \equiv \begin{cases} 1 & \text{jika } a \text{ adalah quadratic residue} \\ -1 & \text{jika } a \text{ adalah non-residue} \end{cases}$$