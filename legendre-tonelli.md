# Quadratic Residue → Legendre Symbol → Euler Criterion → Modular Square Root → Tonelli–Shanks

---

## 1. Quadratic Residue

### Definisi

Bilangan `a` disebut **quadratic residue modulo p (prima ganjil)** jika ada `x` sehingga:

$$x^2 \equiv a \pmod{p}$$

Artinya: `a` punya **modular square root**.

### Contoh

Ambil $p = 7$. Hitung semua kuadrat mod 7:

| x | x² mod 7 |
|---|----------|
| 1 | 1        |
| 2 | 4        |
| 3 | 2        |
| 4 | 2        |
| 5 | 4        |
| 6 | 1        |

Quadratic residues mod 7: $\{1, 2, 4\}$

Contoh:
- 2 adalah quadratic residue karena $3^2 \equiv 2 \pmod{7}$ dan $4^2 \equiv 2 \pmod{7}$
- Jadi: $\sqrt{2} \equiv 3 \text{ dan } 4 \pmod{7}$

### Python — Cek Manual

```python
p = 7
a = 2

roots = []
for x in range(p):
    if pow(x, 2, p) == a:
        roots.append(x)

print(roots)  # [3, 4]
```

---

## 2. Legendre Symbol

### Definisi

$$\left(\frac{a}{p}\right) = \begin{cases} 1 & \text{jika } a \text{ quadratic residue} \\ -1 & \text{jika bukan} \\ 0 & \text{jika } a \equiv 0 \pmod{p} \end{cases}$$

### Cara Hitung — Euler Criterion

$$\left(\frac{a}{p}\right) = a^{\frac{p-1}{2}} \mod p$$

Hasil:
- `1` → residue
- `p-1` → -1 (non-residue)
- `0` → habis dibagi p

### Contoh

Cari $\left(\frac{2}{7}\right)$:

$$2^{(7-1)/2} = 2^3 = 8 \equiv 1 \pmod{7}$$

Jadi 2 adalah quadratic residue.

### Python

```python
def legendre(a, p):
    result = pow(a, (p-1)//2, p)
    if result == 1:
        return 1
    elif result == p-1:
        return -1
    else:
        return 0

print(legendre(2, 7))  # 1
print(legendre(3, 7))  # -1
```

---

## 3. Modular Square Root

**Tujuan:** Diketahui `a` quadratic residue. Cari `x` sehingga:

$$x^2 \equiv a \pmod{p}$$

---

## 4. Case Mudah — p ≡ 3 (mod 4)

Gunakan rumus langsung:

$$x = a^{(p+1)/4} \mod p$$

### Contoh

Cari $\sqrt{2} \pmod{7}$.

Karena $7 \equiv 3 \pmod{4}$ ✔

$$x = 2^{(7+1)/4} = 2^2 = 4$$

Root lainnya: $7 - 4 = 3$

### Python

```python
def mod_sqrt_easy(a, p):
    return pow(a, (p+1)//4, p)

print(mod_sqrt_easy(2, 7))  # 4
```

---

## 5. Case Umum — Tonelli–Shanks Algorithm

Dipakai jika $p \equiv 1 \pmod{4}$. Ini adalah algoritma umum modular square root untuk semua prima ganjil.

### Contoh

Cari $\sqrt{10} \pmod{13}$.

Karena $13 \equiv 1 \pmod{4}$, harus pakai Tonelli–Shanks.

### Implementasi Lengkap

```python
def legendre(a, p):
    return pow(a, (p-1)//2, p)

def tonelli_shanks(a, p):
    if legendre(a, p) != 1:
        return None

    if p % 4 == 3:
        return pow(a, (p+1)//4, p)

    q = p - 1
    s = 0
    while q % 2 == 0:
        q //= 2
        s += 1

    z = 2
    while legendre(z, p) != p-1:
        z += 1

    m = s
    c = pow(z, q, p)
    t = pow(a, q, p)
    r = pow(a, (q+1)//2, p)

    while t != 1:
        i = 0
        temp = t
        while temp != 1:
            temp = pow(temp, 2, p)
            i += 1

        b = pow(c, 2**(m-i-1), p)
        m = i
        c = pow(b, 2, p)
        t = (t * c) % p
        r = (r * b) % p

    return r

print(tonelli_shanks(10, 13))  # 6
```

Output: `6`, root lainnya: `13 - 6 = 7`

**Verifikasi:**
- $6^2 = 36 \equiv 10 \pmod{13}$ ✔
- $7^2 = 49 \equiv 10 \pmod{13}$ ✔

---

## 6. Fakta Penting Crypto

**Jika p prima:**
- Quadratic residue → punya **2 root**
- Non-residue → **0 root**

**Jika n = p × q:**
- Bisa punya **4 root**
- Sulit dicari tanpa faktorisasi

Inilah dasar keamanan:
- **Rabin cryptosystem**
- **Goldwasser–Micali cryptosystem**

---

## Ringkasan Super Cepat

| # | Konsep | Keterangan |
|---|--------|------------|
| 1 | Quadratic Residue | Punya modular square root |
| 2 | Legendre Symbol | Cek residue atau tidak |
| 3 | Euler Criterion | Cara cepat hitung Legendre |
| 4 | p ≡ 3 mod 4 | Pakai rumus $a^{(p+1)/4}$ langsung |
| 5 | p ≡ 1 mod 4 | Pakai Tonelli–Shanks |
| 6 | Modulo komposit | Dasar crypto hardness |

---

