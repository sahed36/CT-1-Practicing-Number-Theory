Number Theory and Cryptography Workshop
Name: Md.Sahed
Student ID: IT23036

Abstract
This repository presents a practical study of fundamental number theory and its applications to cryptography. Mathematical concepts are explained with equations and implemented using Python. The experiments include Euclidean algorithms, modular arithmetic, modular inverses, fast modular exponentiation, the Chinese Remainder Theorem, the One-Time Pad, RSA, Diffie–Hellman key exchange, and selected demonstrations of RSA weaknesses.
Learning note: The small numerical keys used in demonstrations are for education only. They are not secure for real-world communication.

Open the notebook in Google Colab
After uploading this repository to GitHub, replace YOUR_GITHUB_USERNAME below with your GitHub username and keep the repository name and notebook path unchanged:

You can also open the notebook directly in Google Colab and upload the .ipynb file from your computer. The badge link becomes fully functional after the notebook is pushed to the GitHub repository.
Table of Contents
1. Introduction
2. Environment Setup
3. Number Theory Foundations
4. Euclidean and Extended Euclidean Algorithms
5. Linear Diophantine Equations
6. Modular Arithmetic and Modular Inverse
7. Fast Modular Exponentiation
8. Chinese Remainder Theorem
9. Cryptography Fundamentals
10. One-Time Pad and Many-Time Pad
11. RSA Cryptosystem
12. RSA Weakness Demonstrations
13. Diffie–Hellman Key Exchange
14. Shor's Algorithm: Mathematical Idea
15. Experiments and Results
16. Conclusion
17. References
1. Introduction
Number theory is the branch of mathematics concerned with integers and their properties. It provides the mathematical foundation for many cryptographic systems. Cryptography protects information by transforming readable data into a form that unauthorized users cannot understand.
Objectives
- Understand divisibility, prime numbers, GCD and LCM.
- Implement the Euclidean and Extended Euclidean algorithms.
- Solve linear Diophantine equations.
- Perform modular arithmetic and calculate modular inverses.
- Implement fast modular exponentiation and the Chinese Remainder Theorem.
- Demonstrate symmetric and public-key cryptography.
- Understand RSA encryption, decryption and selected weaknesses.
- Demonstrate Diffie–Hellman key exchange.
- Practice mathematical algorithms using Python and Jupyter Notebook.
2. Environment Setup
Requirements
- Python 3.9 or later
- Jupyter Notebook or JupyterLab, or Google Colab
- NumPy and Matplotlib (optional, for numerical experiments and figures)
Install locally:
python -m pip install notebook jupyterlab numpy matplotlib
Start Jupyter:
jupyter notebook
Alternatively, upload number_theory_cryptography_practice.ipynb to Google Colab and execute the cells from top to bottom.

No external package is required for the core algorithms in this repository.
3. Number Theory Foundations
3.1 Divisibility
For integers \(a\) and \(b\), where \(a \ne 0\), we say that \(a\) divides \(b\) if there exists an integer \(k\) such that
\[
b=ak.
\]
Notation: \(a\mid b\).

Example: \(4\mid 20\), because \(20=4\times5\).
3.2 Prime and Composite Numbers
A prime number is an integer greater than 1 whose only positive divisors are 1 and itself. A composite number has more than two positive divisors.

Examples: 2, 3, 5, 7 and 11 are prime; 4, 6, 8 and 9 are composite.
3.3 Greatest Common Divisor
The greatest common divisor, or GCD, of two integers is the largest positive integer that divides both.
\[
\gcd(24,36)=12.
\]
3.4 Least Common Multiple
The least common multiple, or LCM, is the smallest positive integer divisible by both inputs.
\[
\operatorname{lcm}(a,b)=\frac{|ab|}{\gcd(a,b)}
\]
for nonzero integers \(a,b\).
4. Euclidean and Extended Euclidean Algorithms
4.1 Euclidean Algorithm
The Euclidean algorithm repeatedly applies the division relation:
\[
a=bq+r,\quad 0\le r<b.
\]
Then
\[
\gcd(a,b)=\gcd(b,a\bmod b).
\]
The process stops when the remainder becomes zero.
def gcd_euclidean(a, b):
    a, b = abs(a), abs(b)
    while b:
        a, b = b, a % b
    return a

print(gcd_euclidean(48, 18))  # 6
4.2 Extended Euclidean Algorithm
The Extended Euclidean Algorithm finds integers \(x,y\) satisfying Bézout's identity:
\[
ax+by=\gcd(a,b).
\]
def extended_gcd(a, b):
    if b == 0:
        return (a, 1, 0)
    d, x1, y1 = extended_gcd(b, a % b)
    x = y1
    y = x1 - (a // b) * y1
    return (d, x, y)

d, x, y = extended_gcd(27, 15)
print("GCD:", d)
print("Coefficients:", x, y)
print("Verification:", 27*x + 15*y)
Application: The Extended Euclidean Algorithm is used to calculate modular inverses, which are essential in RSA key generation.
5. Linear Diophantine Equations
A linear Diophantine equation has the form
\[
ax+by=c,
\]
where \(a,b,c,x,y\) are integers.

A solution exists if and only if
\[
\gcd(a,b)\mid c.
\]
If Extended Euclid gives \(ax_0+by_0=d\), then for \(d\mid c\), one solution is
\[
x=x_0(c/d),\qquad y=y_0(c/d).
\]
def solve_diophantine(a, b, c):
    d, x, y = extended_gcd(a, b)
    if c % d != 0:
        return None
    factor = c // d
    return x * factor, y * factor

solution = solve_diophantine(15, 25, 5)
print(solution)
if solution:
    x, y = solution
    print("Verification:", 15*x + 25*y)
6. Modular Arithmetic and Modular Inverse
6.1 Modular Arithmetic
The expression
\[
a\equiv b\pmod n
\]
means that \(n\) divides \(a-b\).

Example:
\[
17\equiv5\pmod{12}.
\]
6.2 Multiplicative Inverse
The multiplicative inverse of \(a\) modulo \(n\) is an integer \(a^{-1}\) satisfying
\[
aa^{-1}\equiv1\pmod n.
\]
It exists if and only if
\[
\gcd(a,n)=1.
\]
def mod_inverse(a, n):
    d, x, _ = extended_gcd(a, n)
    if d != 1:
        raise ValueError("Modular inverse does not exist.")
    return x % n

print(mod_inverse(7, 26))  # 15
print((7 * mod_inverse(7, 26)) % 26)  # 1
6.3 Modular Division
To calculate \(b/a\pmod n\), multiply \(b\) by the modular inverse of \(a\):
\[
b/a\equiv b\cdot a^{-1}\pmod n.
\]
def modular_division(b, a, n):
    return (b * mod_inverse(a, n)) % n

print(modular_division(9, 5, 13))
7. Fast Modular Exponentiation
Directly calculating \(a^e\) can create an extremely large integer. Repeated squaring calculates the modular result efficiently.
\[
a^e\bmod n
\]
The algorithm uses approximately \(O(\log e)\) modular multiplications.
def fast_mod_exp(base, exponent, modulus):
    if modulus <= 0 or exponent < 0:
        raise ValueError("Use a positive modulus and non-negative exponent.")
    result = 1 % modulus
    base %= modulus

    while exponent > 0:
        if exponent & 1:
            result = (result * base) % modulus
        base = (base * base) % modulus
        exponent >>= 1

    return result

print(fast_mod_exp(4, 5, 3))  # 1
print(fast_mod_exp(7, 128, 13))
8. Chinese Remainder Theorem
The Chinese Remainder Theorem (CRT) states that if \(n_1,n_2\) are coprime, then the system
\[
x\equiv r_1\pmod{n_1}
\]
\[
x\equiv r_2\pmod{n_2}
\]
has a unique solution modulo \(n_1n_2\).

Using Extended Euclid, find \(u,v\) such that
\[
n_1u+n_2v=1.
\]
Then one solution is
\[
x=(r_1n_2v+r_2n_1u)\bmod(n_1n_2).
\]
def chinese_remainder(n1, r1, n2, r2):
    d, u, v = extended_gcd(n1, n2)
    if d != 1:
        raise ValueError("The moduli must be coprime.")
    return (r1*n2*v + r2*n1*u) % (n1*n2)

x = chinese_remainder(13, 7, 6, 5)
print(x)  # 59
print(x % 13, x % 6)  # 7 5
9. Cryptography Fundamentals
Cryptography is the science of protecting information using mathematical techniques.
Term	Meaning
Plaintext	Original readable message
Ciphertext	Encrypted message
Encryption	Converting plaintext into ciphertext
Decryption	Recovering plaintext from ciphertext
Key	Secret or public information used by an algorithm
Symmetric cryptography	Same secret key is used for encryption and decryption
Asymmetric cryptography	Public and private keys are used


Two broad categories are:
1. Symmetric-key cryptography: One shared secret key is used by both parties.
2. Public-key cryptography: A public key can be shared, while the private key must remain secret.
10. One-Time Pad and Many-Time Pad
10.1 XOR Operation
XOR (exclusive OR) operates on bits:
A	B	A XOR B
0	0	0
0	1	1
1	0	1
1	1	0


For a plaintext \(P\) and key \(K\):
\[
C=P\oplus K
\]
Decryption:
\[
P=C\oplus K.
\]
A correctly implemented One-Time Pad is information-theoretically secure when the key is truly random, at least as long as the message, kept secret, and used only once.
def otp_encrypt(message, key):
    p = message.encode("utf-8")
    k = key.encode("utf-8")
    if len(p) != len(k):
        raise ValueError("Key and message must have equal byte lengths.")
    return bytes(a ^ b for a, b in zip(p, k))

def otp_decrypt(ciphertext, key):
    k = key.encode("utf-8")
    if len(ciphertext) != len(k):
        raise ValueError("Key and ciphertext must have equal byte lengths.")
    plaintext = bytes(a ^ b for a, b in zip(ciphertext, k))
    return plaintext.decode("utf-8")

message = "HELLO"
key = "XMCKL"  # Demonstration key only; not a secure random OTP key.
ciphertext = otp_encrypt(message, key)
print("Ciphertext:", ciphertext.hex())
print("Recovered:", otp_decrypt(ciphertext, key))
10.2 Many-Time Pad
If the same key is reused:
\[
C_1=P_1\oplus K,\qquad C_2=P_2\oplus K.
\]
Therefore,
\[
C_1\oplus C_2=P_1\oplus P_2.
\]
The key cancels out. This leaks a relationship between the plaintext messages and can enable statistical or known-plaintext attacks.
def xor_bytes(a, b):
    return bytes(x ^ y for x, y in zip(a, b))

p1 = b"attack at dawn"
p2 = b"attack at dusk"
reused_key = b"0123456789abcd"

c1 = xor_bytes(p1, reused_key)
c2 = xor_bytes(p2, reused_key)

print("Ciphertext XOR:", xor_bytes(c1, c2).hex())
print("Plaintext XOR:", xor_bytes(p1, p2).hex())
assert xor_bytes(c1, c2) == xor_bytes(p1, p2)
11. RSA Cryptosystem
RSA is an asymmetric cryptographic algorithm based on the mathematical difficulty of factoring a sufficiently large composite integer.
11.1 Key Generation
1. Select two distinct large primes \(p,q\).
2. Calculate \(n=pq\).
3. Calculate Euler's totient:
\[
\phi(n)=(p-1)(q-1).
\]
4. Select \(e\) such that
\[
1<e<\phi(n),\qquad \gcd(e,\phi(n))=1.
\]
5. Calculate \(d\) such that
\[
ed\equiv1\pmod{\phi(n)}.
\]
Public key: \((n,e)\)
Private key: \((n,d)\)
11.2 Encryption
\[
C=M^e\bmod n.
\]
11.3 Decryption
\[
M=C^d\bmod n.
\]
11.4 Educational RSA Implementation
def rsa_key_generation(p, q, e):
    if p == q:
        raise ValueError("Choose distinct primes.")
    n = p * q
    phi = (p - 1) * (q - 1)
    if gcd_euclidean(e, phi) != 1:
        raise ValueError("e must be coprime to phi(n).")
    d = mod_inverse(e, phi)
    return (n, e), (n, d)

def rsa_encrypt_integer(message, public_key):
    n, e = public_key
    if not 0 <= message < n:
        raise ValueError("Message integer must be in [0, n).")
    return fast_mod_exp(message, e, n)

def rsa_decrypt_integer(ciphertext, private_key):
    n, d = private_key
    return fast_mod_exp(ciphertext, d, n)

public_key, private_key = rsa_key_generation(61, 53, 17)
message = 65
ciphertext = rsa_encrypt_integer(message, public_key)
recovered = rsa_decrypt_integer(ciphertext, private_key)

print("Public key:", public_key)
print("Private key:", private_key)
print("Ciphertext:", ciphertext)
print("Recovered message:", recovered)
assert recovered == message
Important: This is textbook RSA for learning. Real applications must use secure padding such as RSA-OAEP for encryption and a vetted cryptographic library. Do not use small primes or raw textbook RSA in production.
12. RSA Weakness Demonstrations
These experiments demonstrate why poor key generation and insecure RSA usage can expose private information. They are educational demonstrations using deliberately weak or compromised examples.
12.1 Small-Prime Factorization
If one factor of \(n=pq\) is very small, trial division can recover it.
def factor_small_prime(n, limit=1_000_000):
    if n % 2 == 0:
        return 2, n // 2
    candidate = 3
    while candidate <= limit and candidate * candidate <= n:
        if n % candidate == 0:
            return candidate, n // candidate
        candidate += 2
    return None

print(factor_small_prime(101 * 1000000007))
12.2 Fermat Factorization: Close Prime Factors
If \(p\) and \(q\) are close, then \(n=pq\) can be expressed as a difference of squares:
\[
n=a^2-b^2=(a-b)(a+b).
\]
from math import isqrt

def fermat_factor(n, max_steps=100_000):
    a = isqrt(n)
    if a * a < n:
        a += 1

    for _ in range(max_steps):
        b2 = a*a - n
        b = isqrt(b2)
        if b*b == b2:
            p, q = a-b, a+b
            if p*q == n:
                return min(p, q), max(p, q)
        a += 1
    return None

print(fermat_factor(1009 * 1013))
12.3 Common-Factor Attack
If two RSA moduli share a prime factor, the GCD reveals that factor:
\[
p=\gcd(n_1,n_2).
\]
def common_factor_attack(n1, n2):
    p = gcd_euclidean(n1, n2)
    if p == 1 or p == n1 or p == n2:
        return None
    return p, n1 // p, n2 // p

n1 = 101 * 107
n2 = 101 * 113
print(common_factor_attack(n1, n2))
12.4 Håstad's Broadcast Attack (Concept)
When the same small message is sent using the same low public exponent to multiple recipients without secure randomized padding, CRT can combine ciphertexts. If the combined integer is the exact \(e\)-th power of the message, taking its integer root can recover the message.

For a two-recipient demonstration with \(e=2\):
def integer_nth_root(value, n):
    if value < 0 or n <= 0:
        raise ValueError("Invalid input.")
    if value < 2:
        return value
    low, high = 0, 1
    while high ** n <= value:
        high *= 2
    while low + 1 < high:
        mid = (low + high) // 2
        if mid ** n <= value:
            low = mid
        else:
            high = mid
    return low

def hastad_two_recipient_demo(m, n1, n2, e=2):
    if e != 2:
        raise NotImplementedError("This demonstration is for e=2.")
    c1 = pow(m, e, n1)
    c2 = pow(m, e, n2)
    combined = chinese_remainder(n1, c1, n2, c2)
    recovered = integer_nth_root(combined, e)
    if recovered ** e != combined:
        raise ValueError("Exact-root condition not satisfied.")
    return c1, c2, combined, recovered

print(hastad_two_recipient_demo(42, 101, 103))
The demonstration requires the message power to be smaller than the product of the moduli and the moduli to be coprime. This is not a general attack against correctly padded modern RSA.
12.5 Mapping to the Workshop RSA Assignment
The original RSA notebook has seven programming tasks. The practice notebook includes corresponding helper functions:
Original task	Function in the practice notebook
Q1: RSA encryption	Encrypt
Q2: RSA decryption	Decrypt
Q3: Search a list of possible plaintexts	DecipherSimple
Q4: Factor a modulus with a small prime factor	DecipherSmallPrime
Q5: Factor a modulus with close prime factors	DecipherSmallDiff
Q6: Recover two keys that share a prime factor	DecipherCommonDivisor
Q7: Håstad broadcast attack for exponent 2	DecipherHastad


The functions use the number-theory helpers defined earlier in the notebook. RSA text encoding requires the encoded integer to be smaller than the modulus. The small-modulus illustration in the original Q3 is not suitable for uniquely encoding arbitrary strings; collisions are possible. Q4–Q7 are demonstrations of weak-key conditions, not instructions for attacking real systems.
13. Diffie–Hellman Key Exchange
Diffie–Hellman allows two parties to establish a shared secret over a public channel.

Public parameters: prime \(p\) and generator \(g\).
- Alice selects private value \(a\) and publishes \(A=g^a\bmod p\).
- Bob selects private value \(b\) and publishes \(B=g^b\bmod p\).
- Alice computes \(K=B^a\bmod p\).
- Bob computes \(K=A^b\bmod p\).
Both obtain:
\[
K=g^{ab}\bmod p.
\]
def diffie_hellman_demo(p, g, a, b):
    A = fast_mod_exp(g, a, p)
    B = fast_mod_exp(g, b, p)
    alice_key = fast_mod_exp(B, a, p)
    bob_key = fast_mod_exp(A, b, p)
    assert alice_key == bob_key
    return A, B, alice_key

print(diffie_hellman_demo(11, 7, 2, 4))
Security note: Basic Diffie–Hellman does not authenticate the communicating parties and is vulnerable to man-in-the-middle attacks unless combined with authentication.
14. Shor's Algorithm: Mathematical Idea
Shor's algorithm is a quantum algorithm that can factor integers efficiently in the ideal fault-tolerant quantum-computing model.

Its mathematical foundation includes period finding. For a chosen integer \(a\) coprime to \(N\), it seeks the smallest positive \(r\) such that
\[
a^r\equiv1\pmod N.
\]
When \(r\) is even and \(a^{r/2}\not\equiv-1\pmod N\), factors can be obtained from
\[
\gcd(a^{r/2}-1,N),\qquad
\gcd(a^{r/2}+1,N).
\]
For \(N=35\), choose \(a=8\). Its period is \(r=4\):
\[
8^2=64,\quad 64\bmod35=29.
\]
Then
\[
\gcd(29-1,35)=7,\qquad
\gcd(29+1,35)=5.
\]
from math import gcd

N = 35
a = 8
r = 4

factor1 = gcd(a**(r//2) - 1, N)
factor2 = gcd(a**(r//2) + 1, N)

print("Factors:", factor1, factor2)
This is a classical illustration of the number-theoretic post-processing, not a quantum implementation of Shor's algorithm.
15. Experiments and Results
Run the notebook and record the actual outputs. Suggested experiments:
Experiment	Input	Expected verification
GCD	48, 18	6
Extended GCD	27, 15	\(27x+15y=3\)
LCM	9, 3	9
Diophantine	15, 25, 5	\(15x+25y=5\)
Modular inverse	7 mod 26	15
Fast modular exponentiation	\(4^5\bmod3\)	1
CRT	(13,7), (6,5)	59
RSA	p=61, q=53, e=17, M=65	Decrypted message = 65
Diffie–Hellman	p=11, g=7, a=2, b=4	Shared key = 9
Fermat factorization	1009 × 1013	1009 and 1013


Result Discussion
The experiments demonstrate that number-theoretic algorithms can be implemented efficiently using Python. The Extended Euclidean Algorithm provides modular inverses, fast modular exponentiation reduces the number of multiplications, and CRT combines congruences. RSA and Diffie–Hellman show how these mathematical properties support public-key cryptography. The attack demonstrations highlight the importance of secure key generation, correct padding and non-reuse of secret material.
16. Conclusion
This practical study connected fundamental number theory with cryptographic applications. The implemented algorithms covered GCD, LCM, Bézout coefficients, Diophantine equations, modular inverses, fast modular exponentiation and CRT. The cryptography experiments demonstrated OTP, the danger of key reuse, RSA, Diffie–Hellman and selected RSA weaknesses. Overall, the work shows that a strong understanding of number theory is essential for understanding modern cryptographic systems.
17. References
1. Number Theory and Cryptography Workshop — Maths and Physics Club, IIT Bombay
2. Workshop README and setup instructions
3. Day 1 Number Theory Assignment Notebook
4. Day 2 Number Theory Assignment
5. Cryptography Handout and Notebooks
6. RSA Assignment Notebook
7. Many-Time Pad Notebook
8. Diffie–Hellman and RSA Demo Notebook
9. Python Documentation
10. Jupyter Documentation
