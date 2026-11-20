# Number Theory and Cryptography

**Submitted by:** Md.Sahed  
**Student ID:** IT23036

Collab: https://colab.research.google.com/drive/1FMWck-88Q7BVTS1YzNtG6TZ-aXVRsim6#scrollTo=MkKPhK-tcG3Y

## About This Assignment

This repository contains my practical work for the Number Theory and Cryptography workshop. I worked through the fundamental concepts of number theory and explored how some of these mathematical ideas are used in cryptography.

I used Python in Google Colab to implement the algorithms and test them with different input values. The main purpose was to understand how the algorithms work rather than only learning their mathematical definitions.

## What I Worked On

During this assignment, I covered the following topics:

- Divisibility and prime numbers
- Greatest Common Divisor (GCD) and Least Common Multiple (LCM)
- Euclidean Algorithm
- Extended Euclidean Algorithm
- Linear Diophantine Equations
- Modular Arithmetic and Modular Inverse
- Fast Modular Exponentiation
- Chinese Remainder Theorem (CRT)
- One-Time Pad (OTP)
- Many-Time Pad
- RSA Encryption and Decryption
- Basic RSA weaknesses and factorization
- Diffie-Hellman Key Exchange
- Basic concept of Shor's Algorithm

## Tools and Environment

I used the following tools to complete the practical exercises:

| Tool | Purpose |
|---|---|
| Python 3 | Implementing algorithms |
| Google Colab | Writing and running Python code |
| Jupyter Notebook | Organizing practical work |
| GitHub | Storing and submitting the assignment |

No additional Python package is required for most examples because I mainly used Python's built-in functions and standard library.

## 1. Number Theory Basics

Number theory is an important part of cryptography. Before implementing encryption algorithms, I practiced some basic mathematical operations.

### Checking Prime Numbers

A prime number is a positive integer greater than 1 that has only two factors: 1 and itself.

```python
def is_prime(n):
    if n <= 1:
        return False

    for i in range(2, int(n**0.5) + 1):
        if n % i == 0:
            return False

    return True

print(is_prime(29))
print(is_prime(15))
```

### Finding GCD and LCM

```python
import math

a = 24
b = 36

gcd = math.gcd(a, b)
lcm = abs(a * b) // gcd

print("GCD:", gcd)
print("LCM:", lcm)
```

## 2. Euclidean Algorithm

The Euclidean Algorithm is used to find the GCD of two integers. It repeatedly replaces the larger problem with a smaller one using the remainder operation.

The main relation is:

\[
\gcd(a,b)=\gcd(b,a\bmod b)
\]

```python
def euclidean_gcd(a, b):
    while b != 0:
        a, b = b, a % b

    return abs(a)

print("GCD:", euclidean_gcd(48, 18))
```

## 3. Extended Euclidean Algorithm

The Extended Euclidean Algorithm not only finds the GCD but also calculates two integers \(x\) and \(y\) satisfying:

\[
ax+by=\gcd(a,b)
\]

```python
def extended_gcd(a, b):
    if b == 0:
        return a, 1, 0

    gcd, x1, y1 = extended_gcd(b, a % b)

    x = y1
    y = x1 - (a // b) * y1

    return gcd, x, y

g, x, y = extended_gcd(35, 15)

print("GCD:", g)
print("x:", x)
print("y:", y)
print("Verification:", 35*x + 15*y)
```

## 4. Modular Arithmetic

Modular arithmetic deals with remainders after division. It is widely used in public-key cryptography.

For example:

\[
25\bmod 7=4
\]

Python implementation:

```python
print(25 % 7)

# Modular inverse of 3 modulo 11
print(pow(3, -1, 11))
```

A modular inverse exists when the number and modulus are relatively prime.

## 5. Fast Modular Exponentiation

This method calculates large powers without calculating the complete exponentiation first.

\[
a^b\bmod n
\]

```python
def fast_mod_exp(base, exponent, modulus):
    result = 1
    base = base % modulus

    while exponent > 0:
        if exponent % 2 == 1:
            result = (result * base) % modulus

        base = (base * base) % modulus
        exponent //= 2

    return result

print(fast_mod_exp(4, 13, 497))
```

## 6. Chinese Remainder Theorem

The Chinese Remainder Theorem helps solve a system of congruences when the moduli are pairwise coprime.

For example:

\[
x\equiv 2\pmod 3
\]

\[
x\equiv 3\pmod 5
\]

\[
x\equiv 2\pmod 7
\]

```python
import math

def chinese_remainder(remainders, moduli):
    M = math.prod(moduli)
    result = 0

    for r, m in zip(remainders, moduli):
        Mi = M // m
        inverse = pow(Mi, -1, m)
        result += r * Mi * inverse

    return result % M

remainders = [2, 3, 2]
moduli = [3, 5, 7]

print("Solution:", chinese_remainder(remainders, moduli))
```

## 7. One-Time Pad

The One-Time Pad is a symmetric encryption technique. In this exercise, I used the XOR operation to demonstrate how a message can be encrypted and decrypted using a key.

```python
def otp_encrypt(message, key):
    return bytes(
        ord(m) ^ ord(k)
        for m, k in zip(message, key)
    )

def otp_decrypt(ciphertext, key):
    return ''.join(
        chr(c ^ ord(k))
        for c, k in zip(ciphertext, key)
    )

message = "HELLO"
key = "XMCKL"

encrypted = otp_encrypt(message, key)
decrypted = otp_decrypt(encrypted, key)

print("Original:", message)
print("Encrypted:", encrypted.hex())
print("Decrypted:", decrypted)
```

**Note:** This is a simplified XOR demonstration. A secure One-Time Pad requires a truly random key of the same length as the message, and the key must never be reused.

## 8. Many-Time Pad

A Many-Time Pad problem occurs when the same OTP key is reused for multiple messages. Reusing the key can expose relationships between the original messages.

```python
def xor_bytes(a, b):
    return bytes(x ^ y for x, y in zip(a, b))

message1 = b"HELLO"
message2 = b"WORLD"
key = b"ABCDE"

cipher1 = xor_bytes(message1, key)
cipher2 = xor_bytes(message2, key)

result = xor_bytes(cipher1, cipher2)

print("Ciphertext 1:", cipher1.hex())
print("Ciphertext 2:", cipher2.hex())
print("XOR of ciphertexts:", result.hex())
```

The result is equal to the XOR of the two original messages because the reused key cancels out.

## 9. RSA Algorithm

RSA is an asymmetric encryption algorithm. It uses a public key for encryption and a private key for decryption.

The important mathematical steps are:

\[
n=pq
\]

\[
\phi(n)=(p-1)(q-1)
\]

\[
d=e^{-1}\bmod\phi(n)
\]

Here, \(p\) and \(q\) are prime numbers, \(e\) is the public exponent, and \(d\) is the private exponent.

### Python Implementation

```python
p = 61
q = 53

n = p * q
phi = (p - 1) * (q - 1)

e = 17
d = pow(e, -1, phi)

# Public and private keys
public_key = (e, n)
private_key = (d, n)

# Original message
message = 65

# Encryption
ciphertext = pow(message, e, n)

# Decryption
decrypted = pow(ciphertext, d, n)

print("Public Key:", public_key)
print("Private Key:", private_key)
print("Original Message:", message)
print("Encrypted Message:", ciphertext)
print("Decrypted Message:", decrypted)
```

## 10. RSA Weakness and Factorization

RSA security depends partly on the difficulty of factoring a sufficiently large integer into its prime factors.

For practice, I tested a simple factorization method using a small value of \(n\).

```python
def factor_n(n):
    for i in range(2, int(n**0.5) + 1):
        if n % i == 0:
            return i, n // i

    return None

print("Factors:", factor_n(n))
```

This method is suitable for small demonstration values. It is not an efficient method for factoring large RSA moduli.

## 11. Diffie-Hellman Key Exchange

Diffie-Hellman allows two users to establish a common secret key over a public communication channel.

The shared key is calculated using modular exponentiation.

```python
p = 23
g = 5

# Private values
a = 6
b = 15

# Public values
A = pow(g, a, p)
B = pow(g, b, p)

# Shared secret
key_alice = pow(B, a, p)
key_bob = pow(A, b, p)

print("Alice Public Value:", A)
print("Bob Public Value:", B)

print("Alice Shared Key:", key_alice)
print("Bob Shared Key:", key_bob)

print("Do both keys match?", key_alice == key_bob)
```

Both users calculate the same shared secret without directly sending their private values.

## 12. Shor's Algorithm

Shor's Algorithm is a quantum algorithm designed to factor integers efficiently compared with known classical factoring methods.

The following code demonstrates the classical period-finding and factor-extraction idea for a small number. It is not a complete quantum implementation.

```python
import math

def shor_demo(N, a):
    if math.gcd(a, N) != 1:
        return "Factor found:", math.gcd(a, N)

    r = 1

    while pow(a, r, N) != 1:
        r += 1

    if r % 2 != 0:
        return "Try another base."

    x = pow(a, r // 2, N)

    if x == N - 1:
        return "Try another base."

    factor1 = math.gcd(x - 1, N)
    factor2 = math.gcd(x + 1, N)

    return factor1, factor2

print("Factors:", shor_demo(15, 2))
```

## Practical Work

I organized the practical implementations in a Jupyter Notebook so that each topic can be executed and tested separately.

The notebook contains the code examples and their outputs. I used small input values to make it easier to observe the mathematical operations and understand the results.

## Repository Structure

```text
Number-Theory-and-Cryptography/
│
├── README.md
├── number_theory_cryptography_practice.ipynb
└── .gitignore
```

## What I Learned

After completing these exercises, I gained practical experience with:

- Finding GCD and solving number theory problems using Python.
- Understanding modular operations and modular inverses.
- Applying the Extended Euclidean Algorithm and Chinese Remainder Theorem.
- Understanding the basic working principles of symmetric and asymmetric encryption.
- Implementing RSA encryption and decryption.
- Understanding how Diffie-Hellman establishes a shared secret.
- Exploring why key reuse can create security problems.
- Learning the basic mathematical idea behind Shor's Algorithm.

## Conclusion

This assignment helped me connect mathematical concepts with practical cryptographic applications. Implementing the algorithms in Python made it easier to understand the relationship between number theory and information security.

The exercises also helped me understand that cryptographic security does not depend only on the algorithm itself; correct implementation and proper key management are equally important.

## References

1. [Number Theory and Cryptography Workshop – MNP Club](https://github.com/mnp-club/Number_Theory_and_Cryptography_Workshop)
2. [Python Documentation](https://docs.python.org/3/)
3. [Google Colab](https://colab.research.google.com/)
4. [RSA – Computer Security Resource Center (NIST)](https://csrc.nist.gov/)

---

**End of Assignment**
