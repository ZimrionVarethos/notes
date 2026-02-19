# 📘 Chinese Remainder Theorem (CRT) — Catatan Lengkap & Praktis

---

## 1️⃣ Definisi

**Chinese Remainder Theorem (CRT)** adalah teorema dalam aritmetika modular yang menyatakan:

> Jika kita punya sistem kongruensi dengan modulus yang **saling relatif prima (coprime)**, maka terdapat **satu solusi unik** modulo hasil perkalian semua modulus.

Secara formal, diberikan sistem:

$$
x \equiv a_1 \pmod{m_1}
$$
$$
x \equiv a_2 \pmod{m_2}
$$
$$
\vdots
$$
$$
x \equiv a_n \pmod{m_n}
$$

Jika $\gcd(m_i, m_j) = 1$ untuk semua $i \ne j$, maka ada **solusi unik** modulo:

$$
M = m_1 \cdot m_2 \cdots m_n
$$

---

## 2️⃣ Intuisi Sederhana

CRT itu seperti mencari angka yang cocok di **beberapa sistem jam berbeda**.

Misalnya:
- Sisa **2** jika dibagi **3**
- Sisa **3** jika dibagi **5**

CRT mencari angka yang memenuhi **dua kondisi itu sekaligus**.

---

## 3️⃣ Contoh Manual

**Diketahui:**

$$
x \equiv 2 \pmod{3}, \quad x \equiv 3 \pmod{5}
$$

Karena $\gcd(3, 5) = 1$, maka solusi unik modulo $M = 3 \times 5 = 15$.

### Langkah 1 — Hitung M

$$
M = 15, \quad M_1 = \frac{15}{3} = 5, \quad M_2 = \frac{15}{5} = 3
$$

### Langkah 2 — Cari Modular Inverse

Cari $5^{-1} \pmod{3}$:

$$
5 \equiv 2 \pmod{3} \implies 2k \equiv 1 \pmod{3}
$$

$2 \times 2 = 4 \equiv 1 \pmod{3}$ ✅ → **inverse = 2**

Cari $3^{-1} \pmod{5}$:

$3 \times 2 = 6 \equiv 1 \pmod{5}$ ✅ → **inverse = 2**

### Langkah 3 — Gunakan Rumus CRT

$$
x = a_1 M_1 \cdot \text{inv}_1 + a_2 M_2 \cdot \text{inv}_2
$$
$$
x = 2 \times 5 \times 2 + 3 \times 3 \times 2 = 20 + 18 = 38
$$
$$
38 \mod 15 = 8
$$

**🎯 Jawaban:**

$$
x \equiv 8 \pmod{15}
$$

---

## 4️⃣ Algoritma Umum CRT

Untuk $n$ persamaan:

1. Hitung $M = \prod m_i$
2. Hitung $M_i = M / m_i$
3. Cari inverse $M_i^{-1} \mod m_i$
4. Hitung:

$$
x = \left(\sum a_i \cdot M_i \cdot M_i^{-1}\right) \mod M
$$

---

## 5️⃣ Source Code Python (Bersih & Benar)

```python
def crt(a, m):
    M = 1
    for mod in m:
        M *= mod

    x = 0
    for ai, mi in zip(a, m):
        Mi = M // mi
        inv = pow(Mi, -1, mi)   # modular inverse
        x += ai * Mi * inv

    return x % M


# Contoh
a = [2, 3]
m = [3, 5]

print(crt(a, m))   # Output: 8
```

---

## 6️⃣ Versi Manual (Extended Euclidean)

Kalau tidak boleh pakai `pow(..., -1, mod)`:

```python
def egcd(a, b):
    if b == 0:
        return a, 1, 0
    g, x1, y1 = egcd(b, a % b)
    x = y1
    y = x1 - (a // b) * y1
    return g, x, y

def modinv(a, m):
    g, x, y = egcd(a, m)
    if g != 1:
        raise Exception("Tidak ada inverse")
    return x % m
```

---

## 7️⃣ Implementasi CRT dalam Dunia Nyata

### 🔐 Dalam RSA

CRT dipakai untuk **mempercepat dekripsi** RSA. Jika $n = p \times q$, daripada menghitung $c^d \mod n$ langsung, RSA menghitung:

$$
c^d \mod p \quad \text{dan} \quad c^d \mod q
$$

Lalu hasilnya digabung menggunakan CRT.

> ⚡ **Hasil:** dekripsi bisa **3–4x lebih cepat**.

---

## 8️⃣ CRT dalam CTF 🔥

### 🎯 Case 1 — Broadcast Attack (Håstad's Attack)

Jika pesan yang sama dikirim ke beberapa orang dengan $e$ kecil (misal $e = 3$), modulus berbeda, dan tanpa padding:

$$
c_1 = m^3 \mod n_1, \quad c_2 = m^3 \mod n_2, \quad c_3 = m^3 \mod n_3
$$

Gunakan CRT untuk mendapatkan $m^3$, lalu ambil **cube root** → plaintext.

> Ini sering banget keluar di CTF crypto! 🔥

---

### 🎯 Case 2 — Multi Modulus Congruence

Kadang soal CTF langsung memberi sistem kongruensi:

```
x ≡ 123 mod 101
x ≡ 456 mod 103
x ≡ 789 mod 107
```

Diminta cari $x$ → langsung pakai **CRT**.

---

### 🎯 Case 3 — Partial Key Leak RSA

Jika diketahui $d \mod (p-1)$ dan $d \mod (q-1)$, bisa **rekonstruksi** $d$ menggunakan CRT.

---

## 9️⃣ Syarat Penting

Modulus harus memenuhi:

$$
\gcd(m_i, m_j) = 1 \quad \text{untuk semua } i \ne j
$$

Kalau tidak terpenuhi:
- Bisa **tidak ada solusi**
- Bisa **banyak solusi**
- Harus pakai **Extended CRT**

---

## 🔥 Ringkasan Cepat

| Penggunaan | Keterangan |
|---|---|
| Menggabungkan sistem kongruensi | Core CRT |
| Mempercepat RSA | CRT optimization |
| Menyerang RSA | Broadcast / Håstad's Attack |
| Soal modular di CTF | Sangat sering muncul |
| Competitive programming | Modular arithmetic |

---

