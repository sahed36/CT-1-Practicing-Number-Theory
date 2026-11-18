Number Theory and Cryptography Workshop – Assignment
Name: MD Sahed
Student ID: IT23036
Course/Topic: Number Theory and Cryptography

 Topics Covered
Section	Topic	Practical work
Number Theory	Divisibility and primes	Divisibility test, prime checker
Number Theory	GCD and Euclidean Algorithm	GCD implementation
Number Theory	Extended Euclidean Algorithm	Bézout coefficients
Number Theory	Modular arithmetic	Addition, subtraction, multiplication, power
Number Theory	Modular inverse	Inverse calculator
Number Theory	Fermat's Little Theorem	Theorem verification
Number Theory	Euler's totient function	Calculate φ(n)
Number Theory	Chinese Remainder Theorem	Solve simultaneous congruences
Cryptography	Caesar cipher	Encrypt and decrypt
Cryptography	Affine cipher	Encrypt and decrypt
Cryptography	Diffie–Hellman	Shared secret generation
Cryptography	RSA	Key generation, encryption, decryption
Security	Hashing and safe practice	SHA-256 demonstration and limitations


2. Setup and Requirements
The assignment can be completed in Google Colab or Jupyter Notebook. The examples use Python's standard library; no external package is required for the core algorithms.
Google Colab
1. Open Google Colab.
2. Select New Notebook.
3. Copy each code cell from this README into a separate code cell.
4. Run the cells from top to bottom.
5. Save the notebook as Number_Theory_Cryptography_IT23036.ipynb.
Jupyter Notebook
Install and start Jupyter:
pip install notebook
jupyter notebook
Optional packages for plotting:
pip install matplotlib numpy
Part A: Number Theory
3. Divisibility and Prime Numbers
An integer a divides an integer b if there is an integer k such that:
\[
b = ak
\]
A prime number is an integer greater than 1 whose only positive divisors are 1 and itself. Examples: 2, 3, 5, 7, 11.
3.1 Python: Divisibility Test
def is_divisible(a, b):
    if a == 0:
        raise ValueError("The divisor cannot be zero.")
    return b % a == 0

print(is_divisible(3, 15))  # True
print(is_divisible(4, 15))  # False
Explanation: The modulo operator % returns the remainder. If b % a == 0, then a divides b.
3.2 Python: Prime Number Test
def is_prime(n):
    if n <= 1:
        return False
    if n <= 3:
        return True
    if n % 2 == 0 or n % 3 == 0:
        return False

    i = 5
    while i * i <= n:
        if n % i == 0 or n % (i + 2) == 0:
            return False
        i += 6
    return True

for number in [1, 2, 7, 15, 29, 35]:
    print(number, "is prime:", is_prime(number))
Expected observation: 2, 7, and 29 are prime; 1, 15, and 35 are not prime.

Time complexity: \(O(\sqrt n)\), because possible divisors only need to be checked up to the square root of n.
4. Greatest Common Divisor (GCD)
The greatest common divisor of two integers is the largest positive integer that divides both numbers.

For example:
\[
\gcd(48,18)=6
\]
4.1 Euclidean Algorithm
The Euclidean Algorithm repeatedly applies:
\[
\gcd(a,b)=\gcd(b,a\bmod b)
\]
until the remainder becomes zero.
def gcd(a, b):
    a, b = abs(a), abs(b)
    while b != 0:
        a, b = b, a % b
    return a

print("GCD(48, 18) =", gcd(48, 18))
print("GCD(101, 10) =", gcd(101, 10))
Expected output:
GCD(48, 18) = 6
GCD(101, 10) = 1
When \(\gcd(a,b)=1\), the two integers are called relatively prime or coprime.
5. Extended Euclidean Algorithm
The Extended Euclidean Algorithm finds integers \(x\) and \(y\) such that:
\[
ax+by=\gcd(a,b)
\]
These values are called Bézout coefficients.
def extended_gcd(a, b):
    if b == 0:
        return abs(a), 1 if a >= 0 else -1, 0

    g, x1, y1 = extended_gcd(b, a % b)
    x = y1
    y = x1 - (a // b) * y1
    return g, x, y

g, x, y = extended_gcd(30, 18)
print("GCD:", g)
print("x:", x, "y:", y)
print("Verification:", 30*x + 18*y)
Explanation: The final verification should equal the GCD. This algorithm is especially important for finding modular inverses.
6. Modular Arithmetic
Modular arithmetic works with remainders. The expression
\[
a \equiv b \pmod m
\]
means that a and b have the same remainder when divided by m, or equivalently that \(m\mid(a-b)\).

Example:
\[
17 \equiv 5 \pmod {12}
\]
6.1 Basic Operations
a, b, m = 17, 5, 12

print("Addition:", (a + b) % m)
print("Subtraction:", (a - b) % m)
print("Multiplication:", (a * b) % m)
print("Power:", pow(a, 3, m))
Python's pow(a, b, m) efficiently calculates \(a^b \bmod m\).
6.2 Modular Exponentiation
For large exponents, modular exponentiation avoids calculating the full power first.
\[
a^b \bmod m
\]
base = 7
exponent = 128
modulus = 13

result = pow(base, exponent, modulus)
print(f"{base}^{exponent} mod {modulus} =", result)
7. Modular Inverse
The modular inverse of a modulo m is an integer x satisfying:
\[
ax \equiv 1 \pmod m
\]
An inverse exists if and only if:
\[
\gcd(a,m)=1
\]
def modular_inverse(a, m):
    g, x, _ = extended_gcd(a, m)
    if g != 1:
        return None
    return x % m

print("Inverse of 3 mod 11:", modular_inverse(3, 11))
print("Verification:", (3 * modular_inverse(3, 11)) % 11)
print("Inverse of 6 mod 9:", modular_inverse(6, 9))
Expected observation: The inverse of 3 modulo 11 is 4 because \(3\times4=12\equiv1\pmod{11}\). No inverse exists for 6 modulo 9 because their GCD is 3, not 1.
8. Fermat's Little Theorem
If p is prime and p does not divide a, then:
\[
a^{p-1}\equiv1\pmod p
\]
An equivalent form is:
\[
a^p\equiv a\pmod p
\]
Python Verification
a = 3
p = 7

print("a^(p-1) mod p =", pow(a, p - 1, p))
print("Expected value = 1")
Explanation: Since 7 is prime and 7 does not divide 3, the result must be 1.

Application: Fermat's Little Theorem is useful in modular arithmetic and some primality-testing methods. It is not, by itself, a proof that an arbitrary number is prime.
9. Euler's Totient Function
Euler's totient function \(\phi(n)\) counts the positive integers from 1 to n that are relatively prime to n.

For a prime number \(p\):
\[
\phi(p)=p-1
\]
For two distinct primes \(p\) and \(q\):
\[
\phi(pq)=(p-1)(q-1)
\]
Python Implementation
def euler_totient(n):
    if n < 1:
        raise ValueError("n must be a positive integer")

    count = 0
    for k in range(1, n + 1):
        if gcd(k, n) == 1:
            count += 1
    return count

for n in [1, 5, 8, 9, 10, 12]:
    print(f"phi({n}) =", euler_totient(n))
Expected observation: \(\phi(9)=6\), because 1, 2, 4, 5, 7, and 8 are coprime to 9.
10. Euler's Theorem
If a and n are coprime, then:
\[
a^{\phi(n)}\equiv1\pmod n
\]
Python Verification
a = 3
n = 10
phi_n = euler_totient(n)

print("phi(n) =", phi_n)
print("a^phi(n) mod n =", pow(a, phi_n, n))
Explanation: Here, \(\gcd(3,10)=1\) and \(\phi(10)=4\), so \(3^4\equiv1\pmod{10}\).
11. Chinese Remainder Theorem (CRT)
The Chinese Remainder Theorem states that a system of congruences has a unique solution modulo the product of the moduli when the moduli are pairwise coprime.

Example:
\[
x\equiv2\pmod3
\]
\[
x\equiv3\pmod5
\]
\[
x\equiv2\pmod7
\]
The solution is \(x=23\pmod{105}\).
Python Implementation
def chinese_remainder(remainders, moduli):
    if len(remainders) != len(moduli):
        raise ValueError("Lists must have equal length")

    # This implementation assumes pairwise coprime moduli.
    for i in range(len(moduli)):
        for j in range(i + 1, len(moduli)):
            if gcd(moduli[i], moduli[j]) != 1:
                raise ValueError("Moduli must be pairwise coprime")

    product = 1
    for m in moduli:
        product *= m

    result = 0
    for r, m in zip(remainders, moduli):
        partial = product // m
        inverse = modular_inverse(partial, m)
        result += r * partial * inverse

    return result % product

r = [2, 3, 2]
m = [3, 5, 7]

x = chinese_remainder(r, m)
print("CRT solution:", x)
print("Verification:", [x % mod for mod in m])
Expected output:
CRT solution: 23
Verification: [2, 3, 2]
Part B: Cryptography
12. Introduction to Cryptography
Cryptography is the science of securing information through mathematical methods.

Important terms:
- Plaintext: Original readable message.
- Ciphertext: Encrypted, unreadable message.
- Encryption: Converting plaintext into ciphertext.
- Decryption: Recovering plaintext from ciphertext.
- Key: A value used by an algorithm to encrypt or decrypt data.
A basic encryption model is:
\[
C=E_K(P)
\]
\[
P=D_K(C)
\]
where P is plaintext, C is ciphertext, E is encryption, D is decryption, and K is the key.
Types of Cryptography
1. Symmetric-key cryptography: The same secret key is used for encryption and decryption.
2. Asymmetric-key cryptography: A public key and a private key are used.
3. Hash functions: Produce a fixed-length digest; hashing is not encryption and cannot normally be reversed.
13. Caesar Cipher
The Caesar cipher is a substitution cipher in which each letter is shifted by a fixed number.

Encryption:
\[
E(x)=(x+k)\bmod26
\]
Decryption:
\[
D(x)=(x-k)\bmod26
\]
Here, k is the shift key.
Python Implementation
def caesar_encrypt(message, shift):
    result = ""
    for char in message:
        if char.isalpha():
            base = ord('A') if char.isupper() else ord('a')
            result += chr((ord(char) - base + shift) % 26 + base)
        else:
            result += char
    return result

def caesar_decrypt(ciphertext, shift):
    return caesar_encrypt(ciphertext, -shift)

message = "HELLO WORLD"
key = 3

encrypted = caesar_encrypt(message, key)
decrypted = caesar_decrypt(encrypted, key)

print("Original:", message)
print("Encrypted:", encrypted)
print("Decrypted:", decrypted)
Expected output:
Original: HELLO WORLD
Encrypted: KHOOR ZRUOG
Decrypted: HELLO WORLD
Limitation: Caesar cipher is insecure because there are only 26 possible shifts and it is vulnerable to brute-force attacks.
14. Affine Cipher
The Affine cipher is another substitution cipher based on modular arithmetic.

Encryption:
\[
E(x)=(ax+b)\bmod26
\]
Decryption:
\[
D(x)=a^{-1}(x-b)\bmod26
\]
The key a must be coprime to 26 so that its modular inverse exists. The value b can be any integer from 0 to 25.
Python Implementation
def affine_encrypt(text, a, b):
    if gcd(a, 26) != 1:
        raise ValueError("a must be coprime to 26")

    output = ""
    for ch in text:
        if ch.isalpha():
            base = ord('A') if ch.isupper() else ord('a')
            x = ord(ch) - base
            output += chr((a * x + b) % 26 + base)
        else:
            output += ch
    return output

def affine_decrypt(text, a, b):
    inv_a = modular_inverse(a, 26)
    if inv_a is None:
        raise ValueError("a has no modular inverse modulo 26")

    output = ""
    for ch in text:
        if ch.isalpha():
            base = ord('A') if ch.isupper() else ord('a')
            y = ord(ch) - base
            output += chr((inv_a * (y - b)) % 26 + base)
        else:
            output += ch
    return output

message = "HELLO"
a, b = 5, 8

cipher = affine_encrypt(message, a, b)
plain = affine_decrypt(cipher, a, b)

print("Ciphertext:", cipher)
print("Decrypted:", plain)
15. Diffie–Hellman Key Exchange
Diffie–Hellman allows two parties to establish a shared secret over a public channel.

Public parameters:
- A prime number \(p\)
- A generator \(g\)
Private values:
- Alice chooses secret a.
- Bob chooses secret b.
Public values:
\[
A=g^a\bmod p
\]
\[
B=g^b\bmod p
\]
Shared secret:
\[
K=B^a\bmod p=A^b\bmod p
\]
Python Demonstration
# Public parameters (small educational values)
p = 23
g = 5

# Private keys
alice_private = 6
bob_private = 15

# Public keys
alice_public = pow(g, alice_private, p)
bob_public = pow(g, bob_private, p)

# Shared secret
alice_shared = pow(bob_public, alice_private, p)
bob_shared = pow(alice_public, bob_private, p)

print("Alice public key:", alice_public)
print("Bob public key:", bob_public)
print("Alice shared secret:", alice_shared)
print("Bob shared secret:", bob_shared)
print("Keys match:", alice_shared == bob_shared)
Expected observation: Both parties calculate the same shared secret.

Security note: These small numbers are only for learning. Real systems use carefully selected large parameters and authenticated key exchange to prevent man-in-the-middle attacks.
16. RSA Public-Key Cryptography
RSA is an asymmetric cryptographic algorithm. It uses a public key for encryption and a private key for decryption.
16.1 Key Generation
1. Select two distinct prime numbers \(p\) and \(q\).
2. Calculate:
\[
n=pq
\]
3. Calculate:
\[
\phi(n)=(p-1)(q-1)
\]
4. Choose e such that:
\[
1<e<\phi(n),\quad \gcd(e,\phi(n))=1
\]
5. Calculate d, the modular inverse of e modulo \(\phi(n)\):
\[
ed\equiv1\pmod{\phi(n)}
\]
Public key: \((e,n)\)

Private key: \((d,n)\)
16.2 Encryption and Decryption
Encryption:
\[
C=M^e\bmod n
\]
Decryption:
\[
M=C^d\bmod n
\]
Python Implementation
def rsa_key_generation(p, q):
    if not is_prime(p) or not is_prime(q) or p == q:
        raise ValueError("p and q must be distinct prime numbers")

    n = p * q
    phi = (p - 1) * (q - 1)

    e = 2
    while e < phi and gcd(e, phi) != 1:
        e += 1

    if e >= phi:
        raise ValueError("Could not find a valid public exponent")

    d = modular_inverse(e, phi)
    return (e, n), (d, n)

def rsa_encrypt(message, public_key):
    e, n = public_key
    if not 0 <= message < n:
        raise ValueError("Message must be an integer between 0 and n-1")
    return pow(message, e, n)

def rsa_decrypt(ciphertext, private_key):
    d, n = private_key
    return pow(ciphertext, d, n)

# Small values are used only for demonstration.
p, q = 61, 53
public_key, private_key = rsa_key_generation(p, q)

message = 65
ciphertext = rsa_encrypt(message, public_key)
decrypted_message = rsa_decrypt(ciphertext, private_key)

print("Public key:", public_key)
print("Private key:", private_key)
print("Original message:", message)
print("Encrypted message:", ciphertext)
print("Decrypted message:", decrypted_message)
Expected observation: The decrypted message should equal the original message, 65.

Important security limitation: This is textbook RSA for educational demonstration only. It is not safe for real communication. Production RSA requires secure padding such as RSA-OAEP, suitable key sizes, and vetted cryptographic libraries. Never use this small-number implementation to protect real data.
17. Cryptographic Hashing (SHA-256)
A cryptographic hash function maps input data to a fixed-length digest.

SHA-256 produces a 256-bit digest. It is designed to be one-way: the original input should not be practically recoverable from the digest.
import hashlib

message = "Number Theory and Cryptography"
digest = hashlib.sha256(message.encode("utf-8")).hexdigest()

print("Message:", message)
print("SHA-256:", digest)
print("Digest length in hexadecimal characters:", len(digest))
Important distinction: Hashing is not encryption. There is no decryption key to recover the original message. For password storage, use a password-hashing scheme such as Argon2id, scrypt, or bcrypt rather than plain SHA-256.
Part C: Integrated Practical Work
18. Mini Practical: RSA Message Demonstration
This final cell combines text conversion and the educational RSA functions. Each character is converted to its Unicode code point and encrypted separately. The example uses small demonstration keys, so it is not secure and is not a real text-encryption system.
public_key, private_key = rsa_key_generation(61, 53)

message = "HI"
encrypted_values = [
    rsa_encrypt(ord(char), public_key)
    for char in message
]

decrypted_text = "".join(
    chr(rsa_decrypt(value, private_key))
    for value in encrypted_values
)

print("Original text:", message)
print("Encrypted integer list:", encrypted_values)
print("Decrypted text:", decrypted_text)
Observation: The decrypted text should match the original text. This demonstrates the mathematical relationship between RSA encryption and decryption, not a secure way to encrypt real messages.
19. Results and Discussion
The practical implementations demonstrate the relationship between number theory and cryptography.
Experiment	Result / observation
Divisibility	Modulo operation identifies whether one integer divides another.
Prime checking	Trial division up to the square root identifies small prime numbers.
Euclidean Algorithm	Repeated remainders calculate the GCD efficiently.
Extended Euclidean Algorithm	Finds Bézout coefficients and supports modular inverse calculation.
Modular arithmetic	Arithmetic can be performed using remainders.
Modular inverse	An inverse exists exactly when the number and modulus are coprime.
Fermat's theorem	The modular power identity holds under its prime and coprimality conditions.
Euler's totient	Counts positive integers relatively prime to a given integer.
CRT	Combines compatible congruences into one solution modulo the product.
Caesar and Affine ciphers	Demonstrate substitution encryption and decryption.
Diffie–Hellman	Two participants independently calculate the same shared secret.
RSA	Public-key encryption and private-key decryption recover the original integer.
SHA-256	Produces a fixed-length digest for an input message.


19.1 Number Theory to Cryptography: Concept Map
                 NUMBER THEORY
                       |
       +---------------+----------------+
       |               |                |
   Prime Numbers      GCD        Modular Arithmetic
       |               |                |
       |        Extended GCD       Modular Inverse
       |               |                |
       +---------------+----------------+
                       |
                  CRYPTOGRAPHY
                       |
       +---------------+----------------+
       |               |                |
      RSA        Diffie–Hellman   Classical Ciphers
       |               |                |
  Public/Private   Shared Secret    Encryption/
      Keys           Exchange        Decryption
20. Conclusion
This assignment explored fundamental concepts of number theory and their applications in cryptography using Python. The Euclidean and Extended Euclidean Algorithms were implemented to calculate GCD and modular inverses. Modular arithmetic, Fermat's Little Theorem, Euler's totient function, Euler's theorem, and the Chinese Remainder Theorem were studied through practical examples.

The cryptography section demonstrated Caesar and Affine ciphers, Diffie–Hellman key exchange, RSA, and SHA-256 hashing. The experiments show that number theory provides the mathematical foundation for many cryptographic techniques. The assignment also highlights that educational implementations with small numbers are not suitable for real-world security.
21. Learning Outcomes
After completing this assignment, I can:
- Explain basic number-theory concepts.
- Implement GCD and Extended GCD in Python.
- Perform modular arithmetic and calculate modular inverses.
- Apply Fermat's Litt
