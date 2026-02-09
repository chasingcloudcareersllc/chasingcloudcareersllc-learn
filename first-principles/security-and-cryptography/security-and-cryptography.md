---
title: "Security and Cryptography"
description: "CIA triad, encryption math, PKI, authentication, OWASP, hardening, secrets management, and compliance."
position: 9
icon: "shield"
---

# Security and Cryptography

Security is not a feature you bolt on at the end. It is a property of a system that emerges from every design decision, every configuration choice, every line of code. This domain teaches you the principles, mathematics, and practical skills that make systems defensible. You will understand why cryptographic algorithms work — not just how to invoke them — because understanding the math underneath is what separates someone who can configure TLS from someone who can reason about whether a system is actually secure.

This domain builds directly on [Networking](/learn/first-principles/networking/) (Domain 7) and [Data Management](/learn/first-principles/data-management/) (Domain 8). You need to understand TCP/IP, DNS, HTTP, and TLS handshakes from Domain 7. You need to understand SQL and database operations from Domain 8. Both are prerequisites because security is not a standalone concern — it operates on networks, databases, applications, and operating systems simultaneously.

---

## A. Security Principles

### Theory

Security is the practice of protecting information and systems from unauthorized access, modification, and destruction. Every security control, every encryption algorithm, every firewall rule maps back to a small set of foundational principles.

#### The CIA Triad

The CIA triad is the foundational model for information security. Every security control exists to enforce one or more of these three properties:

| Principle | Definition | Violation Example |
|-----------|-----------|-------------------|
| **Confidentiality** | Information is accessible only to authorized parties | Database breach exposes user passwords |
| **Integrity** | Information is accurate and has not been tampered with | Attacker modifies a financial transaction in transit |
| **Availability** | Information and systems are accessible when needed | DDoS attack takes a website offline |

These three properties are in constant tension. Maximum confidentiality (encrypt everything, restrict all access) reduces availability. Maximum availability (open access, no authentication) destroys confidentiality. Security engineering is the discipline of finding the right balance for a given threat model.

Every security control maps to at least one CIA property:

| Control | Confidentiality | Integrity | Availability |
|---------|:-:|:-:|:-:|
| Encryption at rest | Yes | | |
| Digital signatures | | Yes | |
| RAID storage | | | Yes |
| TLS (HTTPS) | Yes | Yes | |
| Access control lists | Yes | Yes | |
| Backups | | Yes | Yes |
| DDoS mitigation | | | Yes |
| HMAC | | Yes | |

#### AAA: Authentication, Authorization, Accounting

AAA is the operational framework for controlling access to systems:

- **Authentication** answers "Who are you?" — verifying identity through credentials (passwords, certificates, biometrics).
- **Authorization** answers "What can you do?" — determining permissions after identity is established.
- **Accounting** answers "What did you do?" — logging actions for audit, forensics, and compliance.

These three functions are distinct and must remain separate. Confusing authentication with authorization is one of the most common security design errors. A system that authenticates a user (confirms their identity) has not yet determined what that user is allowed to do.

#### Least Privilege

The principle of least privilege states that every entity — user, process, service — should have only the minimum permissions required to perform its function. Nothing more.

This principle applies at every layer:

- A database application should connect with a user that has SELECT permission on the tables it reads, not a root account.
- A container should run as a non-root user with a read-only filesystem.
- An IAM role should grant access to the specific S3 bucket needed, not `s3:*` on `*`.
- A CI/CD pipeline should have a token scoped to the single repository it deploys, not an organization-wide admin token.

Violations of least privilege are the root cause of most privilege escalation attacks. If a web application running as root is compromised, the attacker gains root. If it runs as an unprivileged user, the attacker must find a second vulnerability to escalate.

#### Defense in Depth

Defense in depth means layering multiple independent security controls so that failure of any single control does not compromise the system. This is borrowed from military strategy: a castle has walls, a moat, guards, and a keep. Breaching one layer does not grant access to the treasure.

In practice, defense in depth looks like:

1. **Network perimeter** — Firewalls, WAFs, DDoS protection
2. **Network segmentation** — VLANs, security groups, microsegmentation
3. **Host hardening** — OS patches, minimal packages, SELinux/AppArmor
4. **Application security** — Input validation, parameterized queries, CSP headers
5. **Data protection** — Encryption at rest, encryption in transit, access controls
6. **Monitoring** — Logging, alerting, anomaly detection, SIEM

If the WAF fails to block a SQL injection attack, parameterized queries in the application stop it. If the application has a vulnerability, network segmentation limits lateral movement. If the attacker reaches the database, encryption at rest protects the data. No single layer is trusted to be sufficient.

#### Zero Trust

Traditional network security follows the "castle and moat" model: everything inside the network perimeter is trusted, everything outside is untrusted. Zero trust eliminates this distinction. Nothing is trusted by default, regardless of network location.

Zero trust principles:

1. **Verify explicitly** — Authenticate and authorize every request based on all available data points (identity, location, device health, data classification).
2. **Use least privilege access** — Grant minimum permissions with just-in-time and just-enough access.
3. **Assume breach** — Design systems as if an attacker is already inside the network. Segment, encrypt, monitor.

Zero trust is not a product. It is an architectural philosophy. Implementing it requires identity-aware proxies, microsegmentation, continuous verification, and comprehensive logging.

#### Threat Modeling: STRIDE

Threat modeling is the systematic process of identifying what can go wrong in a system. STRIDE is a framework developed by Microsoft that categorizes threats into six types:

| Threat | Property Violated | Example |
|--------|-------------------|---------|
| **S**poofing | Authentication | Attacker impersonates a legitimate user |
| **T**ampering | Integrity | Attacker modifies data in transit or at rest |
| **R**epudiation | Non-repudiation | User denies performing an action, and no log exists to prove it |
| **I**nformation Disclosure | Confidentiality | Sensitive data exposed through error messages or logs |
| **D**enial of Service | Availability | Attacker overwhelms system resources |
| **E**levation of Privilege | Authorization | Normal user gains admin access |

To apply STRIDE, decompose a system into components (clients, servers, databases, data flows, trust boundaries), then systematically ask which STRIDE threats apply to each component. The output is a prioritized list of threats that need mitigations.

#### Risk Assessment: Likelihood x Impact

Not all threats deserve equal attention. Risk assessment quantifies threats by combining two factors:

**Risk = Likelihood x Impact**

| | Low Impact | Medium Impact | High Impact |
|---|:-:|:-:|:-:|
| **High Likelihood** | Medium Risk | High Risk | Critical Risk |
| **Medium Likelihood** | Low Risk | Medium Risk | High Risk |
| **Low Likelihood** | Low Risk | Low Risk | Medium Risk |

**Likelihood** considers: How skilled does the attacker need to be? How accessible is the attack surface? Are there known exploits? Is the system internet-facing?

**Impact** considers: What data is exposed? What is the financial cost? What is the reputational damage? Are there regulatory consequences?

This matrix drives resource allocation. A critical risk (high likelihood, high impact) gets immediate attention. A low risk (low likelihood, low impact) gets accepted or deferred.

### Practice

Apply threat modeling to a simple web application:

```
User Browser  -->  Load Balancer  -->  Web Server  -->  Database
                                           |
                                      File Storage
```

> **Try It**: Draw this architecture on paper. For each component and each data flow between components, apply STRIDE. Identify at least one threat per STRIDE category. For each threat, propose a mitigation and identify which CIA property the mitigation protects.

### Connection

Every technical decision in this domain maps back to these principles. When you configure TLS in section B, you are enforcing confidentiality and integrity. When you set up RBAC in section E, you are implementing least privilege. When you harden a Linux system in section H, you are practicing defense in depth. The principles are not abstract — they are the reasoning framework that makes every practical decision coherent.

---

## B. Cryptography

Cryptography is the mathematical foundation of security. Without it, there is no confidentiality in transit, no integrity verification, no digital identity. This section teaches the math first, because the math is what makes the algorithms trustworthy — not magic, but provable properties.

### Theory

#### Math: Modular Arithmetic

Modular arithmetic is clock arithmetic. When you add hours on a 12-hour clock, 10 + 5 = 3 (because 15 mod 12 = 3). The `mod` operation returns the remainder after division.

**Definition**: `a mod n` is the remainder when `a` is divided by `n`.

```
17 mod 5 = 2    (because 17 = 3 * 5 + 2)
25 mod 7 = 4    (because 25 = 3 * 7 + 4)
100 mod 10 = 0  (because 100 = 10 * 10 + 0)
```

**Properties of modular arithmetic:**

```
(a + b) mod n = ((a mod n) + (b mod n)) mod n
(a * b) mod n = ((a mod n) * (b mod n)) mod n
```

These properties are essential for cryptography because they allow computation on large numbers without ever storing the full intermediate values.

**Modular exponentiation**: Computing `a^b mod n` efficiently. Naive computation of `a^b` then taking `mod n` fails for cryptographic-size numbers (hundreds of digits). Instead, use repeated squaring:

```
To compute 3^13 mod 7:
13 in binary = 1101
3^1  mod 7 = 3
3^2  mod 7 = 9 mod 7 = 2
3^4  mod 7 = 2^2 mod 7 = 4
3^8  mod 7 = 4^2 mod 7 = 2

3^13 = 3^8 * 3^4 * 3^1
3^13 mod 7 = (2 * 4 * 3) mod 7 = 24 mod 7 = 3
```

This method computes modular exponentiation in O(log b) multiplications, making RSA and Diffie-Hellman computationally feasible.

#### Math: Number Theory

**Primes**: A prime number has exactly two divisors: 1 and itself. Primes are the foundation of RSA because multiplying two large primes is easy, but factoring the product back into the original primes is computationally infeasible for sufficiently large numbers.

**GCD (Greatest Common Divisor)**: The largest number that divides both `a` and `b`. Two numbers are **coprime** (or relatively prime) if `gcd(a, b) = 1`.

**Euclidean Algorithm** for GCD:

```
gcd(252, 105):
  252 = 2 * 105 + 42
  105 = 2 * 42  + 21
   42 = 2 * 21  + 0
gcd(252, 105) = 21
```

**LCM (Least Common Multiple)**: `lcm(a, b) = (a * b) / gcd(a, b)`

**Euler's Totient Function** `phi(n)`: The count of integers from 1 to n that are coprime with n.

- For a prime `p`: `phi(p) = p - 1`
- For two primes `p, q`: `phi(p * q) = (p - 1)(q - 1)`

This is directly used in RSA key generation.

**Euler's Theorem**: If `gcd(a, n) = 1`, then `a^phi(n) mod n = 1`. This theorem is why RSA decryption reverses encryption.

#### Symmetric Encryption

Symmetric encryption uses the same key for encryption and decryption. It is fast — suitable for bulk data encryption. The security challenge is key distribution: both parties must possess the same secret key.

| Algorithm | Key Size | Block Size | Status |
|-----------|----------|------------|--------|
| AES-128 | 128 bits | 128 bits | Secure, widely used |
| AES-256 | 256 bits | 128 bits | Secure, recommended for high-security |
| ChaCha20 | 256 bits | Stream cipher | Secure, used in TLS, fast in software |
| 3DES | 168 bits | 64 bits | Deprecated, avoid |

**AES (Advanced Encryption Standard)** operates on 128-bit blocks using substitution-permutation networks. It transforms plaintext through multiple rounds (10 for AES-128, 14 for AES-256), each applying SubBytes (nonlinear substitution), ShiftRows (transposition), MixColumns (linear mixing), and AddRoundKey (XOR with round key).

**ChaCha20** is a stream cipher designed by Daniel Bernstein. Unlike AES, it does not process fixed-size blocks — it generates a pseudorandom keystream that is XORed with plaintext. ChaCha20 is faster than AES on platforms without hardware AES acceleration (like mobile devices) and has a simpler, more analyzable design.

#### Block Cipher Modes

A block cipher encrypts fixed-size blocks. Modes of operation define how to handle messages larger than one block.

| Mode | Authentication | Parallelizable | Status |
|------|:-:|:-:|--------|
| ECB (Electronic Codebook) | No | Yes | **Insecure** — identical plaintext blocks produce identical ciphertext blocks |
| CBC (Cipher Block Chaining) | No | Decrypt only | Secure with random IV, but no integrity guarantee |
| GCM (Galois/Counter Mode) | **Yes** | Yes | **Recommended** — provides both confidentiality and integrity (AEAD) |

ECB is catastrophically insecure for structured data. The classic demonstration: encrypting a bitmap image with ECB preserves the image's structure visibly in the ciphertext, because identical pixel blocks encrypt to identical ciphertext blocks.

GCM is an AEAD (Authenticated Encryption with Associated Data) mode. It provides both confidentiality (encryption) and integrity (authentication tag). If an attacker modifies any byte of the ciphertext, decryption fails with an authentication error rather than producing corrupted plaintext silently. Always use AEAD modes.

#### Asymmetric Encryption

Asymmetric encryption uses a key pair: a public key (shared openly) and a private key (kept secret). Data encrypted with the public key can only be decrypted with the private key, and vice versa.

| Algorithm | Basis | Key Size | Status |
|-----------|-------|----------|--------|
| RSA | Integer factorization | 2048+ bits | Secure at 2048+, 4096 recommended |
| ECDSA | Elliptic curve discrete log | 256 bits | Secure, smaller keys than RSA |
| Ed25519 | Twisted Edwards curve | 256 bits | Secure, fast, deterministic signatures |

**RSA Key Generation — Traced with Number Theory**:

RSA security depends on the difficulty of factoring large numbers. Here is the complete process with small numbers for illustration:

```
Step 1: Choose two primes
  p = 61, q = 53

Step 2: Compute n = p * q
  n = 61 * 53 = 3233

Step 3: Compute Euler's totient
  phi(n) = (p - 1)(q - 1) = 60 * 52 = 3120

Step 4: Choose public exponent e
  e must satisfy: 1 < e < phi(n) and gcd(e, phi(n)) = 1
  e = 17  (17 is coprime with 3120, verified: gcd(17, 3120) = 1)

Step 5: Compute private exponent d
  d is the modular inverse of e mod phi(n)
  d * e ≡ 1 (mod phi(n))
  d * 17 ≡ 1 (mod 3120)
  d = 2753  (because 2753 * 17 = 46801 = 15 * 3120 + 1)

Public key:  (e, n) = (17, 3233)
Private key: (d, n) = (2753, 3233)
```

**Encryption**: `ciphertext = plaintext^e mod n`
**Decryption**: `plaintext = ciphertext^d mod n`

```
Encrypt message m = 65:
  c = 65^17 mod 3233 = 2790

Decrypt ciphertext c = 2790:
  m = 2790^2753 mod 3233 = 65
```

This works because of Euler's theorem: `m^(e*d) mod n = m^(1 + k*phi(n)) mod n = m * (m^phi(n))^k mod n = m * 1^k mod n = m`.

An attacker who knows `n` (3233) must factor it into `p` and `q` to compute `phi(n)` and derive `d`. For small numbers this is trivial. For 2048-bit RSA, `n` has over 600 digits, and no known algorithm can factor it in any reasonable time.

#### Hashing

A cryptographic hash function takes arbitrary-length input and produces a fixed-length output (the hash, or digest). Hash functions are one-way: you cannot derive the input from the output.

**Properties of cryptographic hash functions:**

| Property | Definition |
|----------|-----------|
| **Deterministic** | Same input always produces same output |
| **Fixed output size** | SHA-256 always produces 256 bits regardless of input size |
| **Preimage resistant** | Given hash `h`, infeasible to find any `m` such that `hash(m) = h` |
| **Second preimage resistant** | Given `m1`, infeasible to find `m2 != m1` such that `hash(m1) = hash(m2)` |
| **Collision resistant** | Infeasible to find any two distinct inputs that produce the same hash |
| **Avalanche effect** | Changing one bit of input changes approximately 50% of output bits |

| Algorithm | Output Size | Status |
|-----------|------------|--------|
| MD5 | 128 bits | **Broken** — collisions found, never use for security |
| SHA-1 | 160 bits | **Broken** — collision demonstrated (SHAttered), deprecated |
| SHA-256 | 256 bits | Secure, widely used |
| SHA-3 | 256/512 bits | Secure, different internal design (Keccak sponge) |
| BLAKE2 | 256/512 bits | Secure, faster than SHA-256 |

**Password Hashing** is distinct from general-purpose hashing. General hash functions are designed to be fast. Password hashing functions are designed to be deliberately slow, to make brute-force attacks expensive.

| Algorithm | Properties | Status |
|-----------|-----------|--------|
| bcrypt | Adaptive cost factor, salt built-in | Good, widely supported |
| scrypt | Memory-hard (resists GPU attacks) | Good |
| Argon2 | Memory-hard, configurable parallelism | **Best**, winner of Password Hashing Competition |

Never store passwords with SHA-256 or MD5. Even with a salt, general-purpose hashes are too fast — an attacker with a GPU can compute billions of SHA-256 hashes per second. Argon2 with appropriate parameters takes hundreds of milliseconds per hash, making brute-force infeasible.

#### HMAC

HMAC (Hash-based Message Authentication Code) combines a hash function with a secret key to produce a message authentication code. It proves both integrity (the message was not modified) and authenticity (the message was produced by someone who knows the key).

```
HMAC(key, message) = hash((key XOR opad) || hash((key XOR ipad) || message))
```

HMAC is used in TLS, API authentication, JWT signatures, and anywhere you need to verify that a message was not tampered with and came from a known sender.

#### Digital Signatures

A digital signature proves three things: **authentication** (the signer is who they claim to be), **integrity** (the message was not altered), and **non-repudiation** (the signer cannot deny signing).

The process:

```mermaid
sequenceDiagram
    participant Signer
    participant Verifier
    Signer->>Signer: hash(message) → digest
    Signer->>Signer: encrypt(digest, private_key) → signature
    Signer->>Verifier: message + signature
    Verifier->>Verifier: hash(message) → digest
    Verifier->>Verifier: decrypt(signature, public_key) → original_digest
    Verifier->>Verifier: compare digest == original_digest
```

If the digests match, the message is authentic and unmodified. Only the holder of the private key could have produced the signature, and any modification to the message changes the hash.

#### Key Exchange: Diffie-Hellman

Diffie-Hellman solves the key distribution problem: how do two parties establish a shared secret over an insecure channel without ever transmitting the secret?

**The Math:**

```
Public parameters: prime p, generator g

Alice:
  Choose private a (random)
  Compute A = g^a mod p
  Send A to Bob

Bob:
  Choose private b (random)
  Compute B = g^b mod p
  Send B to Alice

Shared secret:
  Alice computes: s = B^a mod p = (g^b)^a mod p = g^(ab) mod p
  Bob computes:   s = A^b mod p = (g^a)^b mod p = g^(ab) mod p
```

An eavesdropper sees `g`, `p`, `A`, and `B`, but cannot compute `g^(ab) mod p` without knowing either `a` or `b`. This is the discrete logarithm problem — computationally infeasible for sufficiently large primes.

**ECDHE (Elliptic Curve Diffie-Hellman Ephemeral)** uses elliptic curves instead of modular exponentiation, providing equivalent security with smaller key sizes. The "ephemeral" part means new keys are generated for each session, providing **forward secrecy**: compromising a long-term key does not compromise past session keys.

ECDHE is the key exchange mechanism used in modern TLS. When you connect to a website over HTTPS, Diffie-Hellman (or its elliptic curve variant) establishes the symmetric session key.

#### Randomness

Cryptographic security depends on unpredictable random numbers. Predictable "random" numbers break everything — if an attacker can predict the random values used in key generation, the keys are compromised.

| Source | Type | Use Case |
|--------|------|----------|
| `/dev/random` | Blocks when entropy pool is empty | Historically used for key generation (Linux now aliases to urandom) |
| `/dev/urandom` | Never blocks, uses CSPRNG | **Use this for everything** |
| CSPRNG | Cryptographically Secure Pseudorandom Number Generator | Library-level: `secrets` module in Python, `crypto/rand` in Go |

On modern Linux kernels (5.6+), `/dev/random` and `/dev/urandom` behave identically after initial seeding. Use `/dev/urandom` or your language's CSPRNG library. Never use `Math.random()`, `rand()`, or similar non-cryptographic PRNGs for security-sensitive operations.

### Practice

#### Generate RSA and Ed25519 Keys

```bash
# Generate a 4096-bit RSA key pair
openssl genpkey -algorithm RSA -pkeyopt rsa_keygen_bits:4096 -out rsa_private.pem

# Extract the public key
openssl pkey -in rsa_private.pem -pubout -out rsa_public.pem

# Inspect the key components (n, e, d, p, q)
openssl pkey -in rsa_private.pem -text -noout

# Generate an Ed25519 key pair
openssl genpkey -algorithm Ed25519 -out ed25519_private.pem

# Extract the public key
openssl pkey -in ed25519_private.pem -pubout -out ed25519_public.pem

# Compare file sizes — Ed25519 keys are much smaller
ls -la rsa_private.pem ed25519_private.pem
```

#### Encrypt and Decrypt with AES-256-GCM

```bash
# Generate a random 256-bit key and 96-bit IV
openssl rand -hex 32 > aes_key.txt
openssl rand -hex 12 > aes_iv.txt

# Create a test file
echo "This is a secret message that must remain confidential." > plaintext.txt

# Encrypt with AES-256-GCM
openssl enc -aes-256-gcm \
  -in plaintext.txt \
  -out ciphertext.bin \
  -K $(cat aes_key.txt) \
  -iv $(cat aes_iv.txt)

# Decrypt
openssl enc -aes-256-gcm -d \
  -in ciphertext.bin \
  -out decrypted.txt \
  -K $(cat aes_key.txt) \
  -iv $(cat aes_iv.txt)

# Verify
diff plaintext.txt decrypted.txt
```

#### Hash a File and Observe the Avalanche Effect

```bash
# Create a file and hash it
echo "Hello, World!" > test.txt
sha256sum test.txt

# Modify one byte (change 'H' to 'h')
echo "hello, World!" > test_modified.txt
sha256sum test_modified.txt

# Compare the hashes — they are completely different
# Changing one character changes approximately 50% of the hash bits
```

> **Try It**: Create two files that differ by exactly one bit. Hash both with SHA-256. Count how many hex digits differ between the two hashes. You should observe roughly half of the 64 hex digits changed — this is the avalanche effect in action.

#### Trace RSA Key Generation

```bash
# Use OpenSSL to generate a small RSA key and inspect the math
openssl genpkey -algorithm RSA -pkeyopt rsa_keygen_bits:512 -out small_rsa.pem 2>/dev/null

# View all components
openssl pkey -in small_rsa.pem -text -noout

# You will see:
# - modulus (n = p * q)
# - publicExponent (e, typically 65537)
# - privateExponent (d)
# - prime1 (p)
# - prime2 (q)
# - exponent1 (d mod (p-1))
# - exponent2 (d mod (q-1))
# - coefficient (q^(-1) mod p, for CRT optimization)

# Verify: e * d mod phi(n) should equal 1
# You can verify this with Python:
python3 -c "
import subprocess, re
output = subprocess.check_output(['openssl', 'pkey', '-in', 'small_rsa.pem', '-text', '-noout'], text=True)
print(output)
print('Inspect the values and verify e * d ≡ 1 (mod (p-1)(q-1))')
"
```

> **Try It**: Generate a 512-bit RSA key (insecure, for educational purposes only). Extract p, q, n, e, d from the output. Verify by hand that `n = p * q`, that `gcd(e, (p-1)(q-1)) = 1`, and that `e * d mod (p-1)(q-1) = 1`. This is the number theory from earlier applied directly.

### Connection

Every tool you used in this section — `openssl`, `sha256sum`, `ssh-keygen` — is executing the mathematics described in the theory. When you generate an RSA key, OpenSSL is finding two large primes, computing the totient, and solving for the modular inverse. When you encrypt with AES-GCM, OpenSSL is performing Galois field multiplication to compute the authentication tag. Understanding the math means you can reason about the security properties of these operations, not just invoke them blindly.

---

## C. PKI and Certificates

### Theory

Public Key Infrastructure (PKI) solves the key distribution problem for asymmetric cryptography. If Alice has Bob's public key, she can encrypt a message only Bob can decrypt. But how does Alice know the public key actually belongs to Bob, and not an attacker performing a man-in-the-middle attack?

PKI introduces a trusted third party — the **Certificate Authority (CA)** — that vouches for the binding between a public key and an identity.

#### Certificate Authority Hierarchy

```mermaid
flowchart TD
    Root["Root CA<br/>(self-signed, offline, in trust stores)"]
    Root --> Int1["Intermediate CA 1"]
    Root --> Int2["Intermediate CA 2"]
    Int1 --> Leaf1["leaf: example.com"]
    Int1 --> Leaf2["leaf: api.example.com"]
    Int2 --> Leaf3["leaf: another.org"]
```

- **Root CA**: Self-signed certificate stored in OS/browser trust stores. Kept offline for security. Compromise of a root CA undermines all certificates it has issued.
- **Intermediate CA**: Signed by the root CA. Used for day-to-day certificate issuance. Can be revoked without replacing the root.
- **Leaf Certificate**: The certificate presented by a server (e.g., example.com). Signed by an intermediate CA.

Trust is transitive: if your OS trusts the root CA, and the root CA signed the intermediate, and the intermediate signed the leaf, then you trust the leaf. This is the **certificate chain**.

#### X.509 Certificate Structure

X.509 is the standard format for public key certificates. Key fields:

| Field | Purpose |
|-------|---------|
| **Subject** | The entity the certificate identifies (e.g., `CN=example.com`) |
| **Issuer** | The CA that signed the certificate |
| **Serial Number** | Unique identifier assigned by the CA |
| **Validity** | Not Before / Not After dates |
| **Public Key** | The subject's public key |
| **Subject Alternative Name (SAN)** | Additional identities (domain names, IP addresses) |
| **Signature Algorithm** | Algorithm used by the CA to sign (e.g., SHA256withRSA) |
| **Signature** | The CA's digital signature over the certificate contents |
| **Extensions** | Key Usage, Basic Constraints, CRL Distribution Points, etc. |

Modern certificates use the SAN extension rather than the Common Name (CN) for domain validation. A single certificate can cover multiple domains via SAN entries.

#### Chain Validation

When a client (browser, `curl`, application) connects to a server over TLS:

1. Server sends its leaf certificate and any intermediate certificates
2. Client builds a chain from the leaf to a trusted root in its trust store
3. Client verifies each certificate's signature using the issuer's public key
4. Client checks validity dates, revocation status, and that the requested hostname matches the SAN
5. If all checks pass, the connection is trusted

If any step fails — expired certificate, hostname mismatch, untrusted root, revoked intermediate — the connection is rejected.

#### Certificate Lifecycle

```mermaid
flowchart LR
    CSR["1. Generate CSR<br/>(key pair + identity)"] --> Sign["2. CA Signs<br/>(creates certificate)"]
    Sign --> Dist["3. Distribute<br/>(install on server)"]
    Dist --> Use["4. Use<br/>(serve TLS connections)"]
    Use --> Renew["5. Renew<br/>(before expiry)"]
    Renew --> Sign
    Use --> Revoke["6. Revoke<br/>(if compromised)"]
```

- **CSR (Certificate Signing Request)**: Contains the public key and identity information. The private key never leaves the server.
- **Signing**: The CA verifies the identity, signs the certificate with its private key.
- **Distribution**: The signed certificate is installed on the server alongside the private key.
- **Renewal**: Certificates have finite lifetimes (90 days for Let's Encrypt, 1 year maximum per CA/Browser Forum rules).
- **Revocation**: If a private key is compromised, the certificate must be revoked.

**Revocation Mechanisms:**

| Mechanism | How It Works | Drawback |
|-----------|-------------|----------|
| **CRL (Certificate Revocation List)** | CA publishes a list of revoked serial numbers | Lists grow large, clients must download them |
| **OCSP (Online Certificate Status Protocol)** | Client queries CA in real-time for certificate status | Privacy concern (CA sees which sites you visit), availability dependency |
| **OCSP Stapling** | Server fetches its own OCSP response and staples it to TLS handshake | Best option — no privacy leak, no extra round-trip |

#### Let's Encrypt and ACME

Let's Encrypt is a free, automated CA that issues domain-validated certificates. It uses the ACME (Automatic Certificate Management Environment) protocol to verify domain ownership:

1. Client (certbot, acme.sh) generates a key pair and CSR
2. ACME server issues challenges (HTTP-01: place a file at `/.well-known/acme-challenge/`, or DNS-01: create a TXT record)
3. Client completes the challenge, proving domain control
4. ACME server signs the certificate
5. Client installs the certificate and configures automatic renewal

Let's Encrypt certificates are valid for 90 days, encouraging automation. Manual certificate management does not scale.

#### Self-Signed Certificates

A self-signed certificate is signed by its own private key rather than a CA. It provides encryption but not third-party identity verification. Use cases:

- Development and testing environments
- Internal services with a private CA
- Never for public-facing production systems (browsers will reject them)

### Practice

#### Generate a Self-Signed CA and Issue a Leaf Certificate

```bash
# Step 1: Create a root CA
# Generate CA private key
openssl genpkey -algorithm RSA -pkeyopt rsa_keygen_bits:4096 -out ca_key.pem

# Create self-signed CA certificate (valid 10 years)
openssl req -x509 -new -key ca_key.pem -sha256 -days 3650 \
  -out ca_cert.pem \
  -subj "/C=US/ST=Lab/L=Lab/O=Lab CA/CN=Lab Root CA"

# Step 2: Create a leaf certificate for a server
# Generate server private key
openssl genpkey -algorithm RSA -pkeyopt rsa_keygen_bits:2048 -out server_key.pem

# Create CSR
openssl req -new -key server_key.pem -out server.csr \
  -subj "/C=US/ST=Lab/L=Lab/O=Lab/CN=app.lab.local"

# Create a SAN extension file
cat > san.cnf << 'EOF'
[v3_req]
subjectAltName = @alt_names
[alt_names]
DNS.1 = app.lab.local
DNS.2 = *.lab.local
IP.1 = 127.0.0.1
EOF

# Sign the CSR with the CA
openssl x509 -req -in server.csr \
  -CA ca_cert.pem -CAkey ca_key.pem -CAcreateserial \
  -out server_cert.pem -days 365 -sha256 \
  -extfile san.cnf -extensions v3_req

# Step 3: Verify the chain
openssl verify -CAfile ca_cert.pem server_cert.pem
# Output: server_cert.pem: OK
```

#### Inspect the Certificates

```bash
# View the CA certificate
openssl x509 -in ca_cert.pem -text -noout | head -30

# View the server certificate
openssl x509 -in server_cert.pem -text -noout

# Check specific fields
openssl x509 -in server_cert.pem -noout -subject -issuer -dates -ext subjectAltName
```

#### Inspect a Live Certificate

```bash
# Connect to a live server and retrieve its certificate chain
echo | openssl s_client -connect google.com:443 -showcerts 2>/dev/null | \
  openssl x509 -text -noout | head -40

# View the full chain
echo | openssl s_client -connect google.com:443 -showcerts 2>/dev/null

# Check certificate expiration
echo | openssl s_client -connect google.com:443 2>/dev/null | \
  openssl x509 -noout -dates
```

> **Try It**: Inspect the certificates for three different websites. For each, identify: the subject CN and SAN entries, the issuer (which CA signed it), the signature algorithm, and the expiration date. Verify the chain by identifying the root CA and checking whether it is in your system's trust store.

### Connection

PKI is the mechanism that makes HTTPS work. When you visit a website and see the padlock icon, your browser has validated a certificate chain from the server's leaf certificate to a trusted root CA in your operating system's trust store. The digital signatures from section B verify each link in the chain. The Diffie-Hellman key exchange from section B establishes the symmetric session key. PKI, asymmetric cryptography, symmetric encryption, and hashing all work together to create a secure channel.

---

## D. Authentication

### Theory

Authentication answers the question "Who are you?" It is the process of verifying that an entity is who it claims to be. Authentication is distinct from authorization (what you can do) — a system must authenticate first, then authorize.

#### Authentication Factors

Authentication factors are categories of evidence that prove identity:

| Factor | Type | Examples |
|--------|------|---------|
| **Knowledge** | Something you know | Password, PIN, security question |
| **Possession** | Something you have | Phone, hardware token, smart card |
| **Inherence** | Something you are | Fingerprint, face recognition, retina scan |

**Single-factor authentication** uses one factor (typically a password). **Multi-factor authentication (MFA)** requires two or more factors from different categories. Using a password and a PIN is NOT true MFA — both are knowledge factors. A password (knowledge) plus a TOTP code from a phone app (possession) is true MFA.

#### MFA Mechanisms

| Mechanism | Factor | Security | Phishing Resistant |
|-----------|--------|----------|:--:|
| SMS OTP | Possession | Low — SIM swap attacks, SS7 interception | No |
| TOTP (RFC 6238) | Possession | Medium — shared secret, time-based codes | No |
| Push notification | Possession | Medium — MFA fatigue attacks possible | No |
| FIDO2/WebAuthn | Possession + Inherence | **High** — asymmetric crypto, origin-bound | **Yes** |

**TOTP (Time-based One-Time Password)**: The server and authenticator app share a secret key. Both compute `HMAC-SHA1(secret, floor(time / 30))` and truncate to a 6-digit code. Because both sides use the same secret and the same time window, they produce the same code. The code changes every 30 seconds.

**FIDO2/WebAuthn**: The authenticator (hardware key or platform authenticator) generates a unique asymmetric key pair per site. The private key never leaves the device. Authentication is a challenge-response: the server sends a random challenge, the authenticator signs it with the private key, and the server verifies with the stored public key. Because the key pair is bound to the site's origin, phishing sites cannot replay the authentication.

#### Password Security

Password security is a solved problem with a clear hierarchy:

1. **Length over complexity** — `correct horse battery staple` (28 chars) is stronger than `P@$$w0rd!` (9 chars). Entropy scales with length, not character set diversity.
2. **Password managers** — Generate unique, random, high-entropy passwords for every site. The user memorizes one master password.
3. **Credential stuffing defense** — Attackers take breached username/password pairs from one site and try them on every other site. Unique passwords per site eliminate this attack vector entirely.
4. **Server-side** — Store passwords using Argon2 (or bcrypt/scrypt) with appropriate cost parameters. Never store plaintext. Never use SHA-256/MD5.

#### SSO: SAML and OIDC

Single Sign-On (SSO) allows users to authenticate once and access multiple services.

**SAML (Security Assertion Markup Language)**: XML-based protocol. The Identity Provider (IdP) authenticates the user and sends a signed XML assertion to the Service Provider (SP). Mature, enterprise-focused, complex. Used widely in corporate environments.

**OIDC (OpenID Connect)**: JSON-based protocol built on top of OAuth 2.0. The IdP returns a signed JWT (ID token) containing user identity claims. Modern, simpler than SAML, used by Google, Microsoft, Okta, and most SaaS applications.

#### OAuth 2.0 vs OIDC

This distinction is critical and frequently confused:

| | OAuth 2.0 | OIDC |
|---|-----------|------|
| **Purpose** | **Authorization** — grants access to resources | **Authentication** — verifies identity |
| **Token** | Access token (opaque or JWT) | ID token (JWT with identity claims) |
| **Question answered** | "Can this app access my photos?" | "Who is this user?" |
| **Use case** | Third-party app accessing user's Google Drive | "Sign in with Google" |

OAuth 2.0 is an **authorization** framework. It was never designed for authentication. Using OAuth 2.0 alone for authentication creates security vulnerabilities (confused deputy problem). OIDC adds an identity layer on top of OAuth 2.0, making authentication secure and standardized.

**OAuth 2.0 Authorization Code Flow:**

```mermaid
sequenceDiagram
    participant User
    participant App
    participant AuthServer
    participant ResourceServer

    User->>App: Click "Login with Provider"
    App->>AuthServer: Redirect to /authorize
    AuthServer->>User: Show login page
    User->>AuthServer: Enter credentials
    AuthServer->>App: Redirect with authorization code
    App->>AuthServer: Exchange code for tokens (+ client secret)
    AuthServer->>App: Access token (+ ID token if OIDC)
    App->>ResourceServer: API request with access token
    ResourceServer->>App: Protected resource
```

#### Service Authentication

Services (APIs, microservices, background workers) also need to authenticate to each other:

| Method | Security | Use Case |
|--------|----------|----------|
| **API Keys** | Low — static secret, easily leaked | Simple integrations, rate limiting |
| **mTLS (Mutual TLS)** | High — both sides present certificates | Service mesh, zero trust architectures |
| **Workload Identity** | High — cloud provider verifies the workload's identity | Cloud services authenticating to cloud APIs without static credentials |

API keys are shared secrets — equivalent to passwords. They should be treated with the same care (rotated, scoped, never committed to version control). mTLS is stronger because it uses asymmetric cryptography and certificates from section C. Workload identity eliminates static credentials entirely by tying authentication to the compute platform (e.g., an AWS Lambda function assumes an IAM role based on its configuration, not a stored access key).

### Practice

> **Try It**: Set up TOTP-based MFA on a service you use (GitHub, AWS, Google). Install an authenticator app (Google Authenticator, Authy, or a password manager with TOTP support). After setup, examine the QR code — it encodes a `otpauth://` URI containing the shared secret. Understand that both your app and the server now share this secret and independently compute the same time-based codes.

> **Try It**: Use `openssl s_client` to connect to a server that requires client certificates (mTLS). If you do not have such a server, set up nginx with `ssl_verify_client on` using the CA and certificates from section C. Observe the TLS handshake and note the `CertificateRequest` message from the server.

### Connection

Authentication integrates directly with cryptography (section B) and PKI (section C). TOTP uses HMAC. FIDO2 uses asymmetric key pairs. mTLS uses X.509 certificates. OAuth 2.0 and OIDC tokens are signed with the same digital signature algorithms. Every authentication mechanism is built on the cryptographic primitives you already understand.

---

## E. Authorization

### Theory

Authorization answers "What can you do?" after authentication has established "Who are you?" Authorization models define the rules that govern access.

#### RBAC (Role-Based Access Control)

RBAC assigns permissions to roles, then assigns roles to users. Users inherit the permissions of their assigned roles.

```
Users:        alice, bob, charlie
Roles:        admin, editor, viewer
Permissions:  create, read, update, delete

Assignments:
  admin  → create, read, update, delete
  editor → create, read, update
  viewer → read

  alice   → admin
  bob     → editor
  charlie → viewer
```

RBAC is simple, widely used, and sufficient for most applications. Its limitation is that access decisions are based solely on role membership — it cannot express context-dependent rules like "editors can only update articles they created" or "access is denied outside business hours."

#### ABAC (Attribute-Based Access Control)

ABAC makes access decisions based on attributes of the subject, resource, action, and environment:

```
Policy: Allow if
  subject.department == "engineering" AND
  resource.classification != "top-secret" AND
  action == "read" AND
  environment.time BETWEEN 08:00 AND 18:00
```

ABAC is more expressive than RBAC. It can encode any access rule, including context-dependent and fine-grained policies. The tradeoff is complexity — ABAC policies are harder to audit and understand.

#### ACLs (Access Control Lists)

ACLs attach permission lists directly to resources. Each resource has a list of who can do what:

```
/var/www/data/report.pdf:
  alice:   read, write
  bob:     read
  charlie: (no access)
```

ACLs are familiar from file system permissions (`chmod`, `chown` from [Operating Systems and Linux](/learn/first-principles/operating-systems-and-linux/)). They are simple for small systems but become unmanageable at scale — maintaining per-resource ACLs across thousands of resources and hundreds of users is operationally expensive.

#### Comparison

| Model | Complexity | Expressiveness | Scalability | Auditability |
|-------|:-:|:-:|:-:|:-:|
| RBAC | Low | Medium | High | High |
| ABAC | High | High | High | Medium |
| ACLs | Low | Low | Low | Low |

Most production systems use RBAC with ABAC-like extensions for specific cases. Cloud IAM systems (AWS IAM, Azure RBAC, GCP IAM) use RBAC as the primary model with condition-based policies for fine-grained control.

#### Policy Engines

Policy engines externalize authorization decisions from application code:

- **OPA (Open Policy Agent)**: General-purpose policy engine. Policies written in Rego (a declarative query language). Used in Kubernetes admission control, API gateways, microservices.
- **Cedar**: Policy language developed by AWS. Used in Amazon Verified Permissions. Designed for both RBAC and ABAC with formal verification support.

Externalizing policy from code means authorization rules can be audited, tested, and updated independently of application deployments.

#### Least Privilege in Practice

| Principle | Implementation |
|-----------|---------------|
| **Scoped tokens** | Issue API tokens with only the permissions needed for the specific operation |
| **Temporary credentials** | Use short-lived credentials that expire (AWS STS, GCP workload identity tokens) |
| **JIT (Just-In-Time) access** | Grant elevated permissions only when needed, automatically revoke after a time window |
| **Break-glass procedures** | Emergency access that bypasses normal controls, with comprehensive logging and post-incident review |

#### Cloud IAM Concepts

| Cloud | Model | Key Concepts |
|-------|-------|-------------|
| **AWS IAM** | Policy-based | Users, groups, roles, policies (JSON), trust policies for cross-account access |
| **Azure RBAC** | Role-based | Security principals, role definitions, scopes (management group/subscription/resource group/resource) |
| **GCP IAM** | Role-based | Members, roles (basic/predefined/custom), resource hierarchy (org/folder/project/resource) |

All three follow the same pattern: an identity is granted a role (set of permissions) at a scope (where the permissions apply). The principle of least privilege demands the narrowest role at the narrowest scope.

### Practice

> **Try It**: In a cloud account (AWS free tier, Azure free account, or GCP free tier), create two IAM roles: one that can read from a specific S3 bucket / Azure Blob container / GCS bucket, and one that can read and write. Assign each role to a different identity. Verify that the read-only identity cannot write. This is RBAC in action.

> **Try It**: Write an OPA policy in Rego that implements the following rule: "Users in the engineering department can read any resource except those classified as top-secret." Test it with the OPA CLI (`opa eval`). This is ABAC expressed as code.

### Connection

Authorization builds on authentication (section D) — you cannot authorize an unknown identity. Cloud IAM roles are enforced after authentication via OAuth 2.0 or workload identity. The principle of least privilege, introduced in section A, is operationalized through RBAC, scoped tokens, and JIT access. Authorization failures are a primary category in the OWASP Top 10 (section G) — broken access control is consistently the number one web application vulnerability.

---

## F. Secrets Management

### Theory

A **secret** is any piece of data that grants access to a system or resource if known. Secrets include:

- Database passwords and connection strings
- API keys and tokens
- TLS private keys and certificates
- SSH private keys
- Encryption keys
- OAuth client secrets
- Service account credentials

#### Why Hardcoded Secrets Are a Critical Vulnerability

Hardcoding a secret in source code means embedding it directly in a file that is version-controlled, deployed, and potentially public:

```python
# NEVER DO THIS
db_password = "SuperSecret123!"
aws_access_key = "AKIAIOSFODNN7EXAMPLE"
```

The consequences are severe:

1. **Version control persistence** — Git stores every version of every file. Even if you delete the secret in a later commit, it remains in the repository history forever. `git log -p` will reveal it.
2. **Wide exposure** — Anyone with repository access (current employees, past employees, contractors, open-source contributors) can see the secret.
3. **Automated scanning** — Attackers continuously scan GitHub for leaked AWS keys, database credentials, and API tokens. Exposed AWS keys are exploited within minutes.
4. **No rotation** — Changing a hardcoded secret requires a code change, review, and deployment. This friction discourages rotation.

#### Environment Variables: Better but Risky

```bash
# Better than hardcoding, but still has risks
export DB_PASSWORD="SuperSecret123!"
python app.py
```

Environment variables avoid committing secrets to code, but:
- They appear in process listings (`/proc/<pid>/environ` on Linux)
- They are logged by some frameworks and debugging tools
- They persist in shell history
- They cannot be audited, rotated, or access-controlled centrally

Environment variables are acceptable for local development and simple deployments. For production systems, use a secret manager.

#### Secret Managers

Secret managers provide centralized, auditable, access-controlled storage for secrets:

| Tool | Provider | Key Features |
|------|----------|-------------|
| **HashiCorp Vault** | Open source / HashiCorp | Dynamic secrets, encryption as a service, identity-based access, multi-cloud |
| **AWS Secrets Manager** | AWS | Native AWS integration, automatic rotation for RDS/Redshift, IAM-based access |
| **Azure Key Vault** | Microsoft | HSM-backed, Azure RBAC integration, certificate management |
| **GCP Secret Manager** | Google | IAM-based access, automatic replication, audit logging |

All secret managers provide:
- **Encryption at rest** — Secrets are stored encrypted, not in plaintext
- **Access control** — Fine-grained policies control who can read which secrets
- **Audit logging** — Every access is logged (who accessed what, when)
- **Rotation** — Secrets can be rotated programmatically without application redeployment
- **Versioning** — Previous secret values are retained for rollback

#### Vault Architecture

HashiCorp Vault uses a **seal/unseal** mechanism:

1. Vault starts in a **sealed** state — the encryption key is split using Shamir's Secret Sharing into multiple key shares
2. To **unseal**, a quorum of key holders (e.g., 3 of 5) must provide their shares
3. Once unsealed, Vault can decrypt its storage backend and serve requests
4. If Vault restarts, it reseals and requires the unseal process again

This prevents a single administrator from accessing all secrets. Even with full access to the storage backend (Consul, S3, etc.), the data is encrypted and cannot be read without the unseal keys.

#### Secret Rotation

Secrets should be rotated regularly and immediately after suspected compromise:

- **Automated rotation** — Secret managers can rotate database passwords, API keys, and certificates on a schedule
- **Dynamic secrets** — Vault can generate short-lived database credentials on demand. Each application instance gets unique credentials that expire automatically
- **Zero-downtime rotation** — Applications must handle secret rotation gracefully (reconnect with new credentials, support dual-active secrets during transition)

#### Secret Scanning

Prevent secrets from entering version control:

| Tool | Scope | How It Works |
|------|-------|-------------|
| **git-secrets** | Pre-commit hook | Scans staged changes for patterns matching known secret formats |
| **truffleHog** | Repository scan | Scans entire git history for high-entropy strings and known patterns |
| **GitHub Secret Scanning** | Repository/Organization | Automated scanning with provider notification (e.g., AWS is notified of leaked keys) |
| **gitleaks** | Pre-commit + CI | Configurable rules, fast, supports custom patterns |

Layer these tools: pre-commit hooks catch secrets before they are committed, CI scanning catches what hooks miss, and platform scanning (GitHub, GitLab) provides a safety net.

### Practice

#### Set Up Vault in Dev Mode

```bash
# Install Vault (macOS)
brew install vault

# Or download from https://releases.hashicorp.com/vault/

# Start Vault in dev mode (in-memory, auto-unsealed, DO NOT use in production)
vault server -dev &

# Set the address and token
export VAULT_ADDR='http://127.0.0.1:8200'
export VAULT_TOKEN='root'  # Dev mode root token

# Verify Vault is running
vault status
```

#### Store and Retrieve a Secret

```bash
# Write a secret
vault kv put secret/myapp/database \
  username="app_user" \
  password="db_secret_password_123" \
  host="db.internal.example.com"

# Read the secret
vault kv get secret/myapp/database

# Read a specific field
vault kv get -field=password secret/myapp/database

# Read as JSON (for programmatic access)
vault kv get -format=json secret/myapp/database
```

#### Configure a Policy

```bash
# Create a policy that only allows reading the database secret
cat > myapp-policy.hcl << 'EOF'
path "secret/data/myapp/database" {
  capabilities = ["read"]
}

path "secret/data/myapp/*" {
  capabilities = ["list"]
}
EOF

vault policy write myapp-read myapp-policy.hcl

# Create a token with this policy
vault token create -policy=myapp-read -ttl=1h

# Use the new token — it can read but not write
export VAULT_TOKEN="<token from above>"
vault kv get secret/myapp/database    # Works
vault kv put secret/myapp/database password="new"  # Permission denied
```

> **Try It**: Set up Vault in dev mode. Store a database credential. Create two policies — one read-only, one read-write. Create tokens for each policy and verify that the read-only token cannot modify secrets. Then delete the secret and verify the read-only token cannot recreate it. This is least privilege applied to secrets.

> **Try It**: Install `gitleaks` and run it against one of your repositories: `gitleaks detect --source /path/to/repo`. Review the findings. If it finds anything, rotate those secrets immediately — they are compromised the moment they entered version control.

### Connection

Secrets management ties together cryptography (section B) for encryption at rest, PKI (section C) for TLS certificate management, authentication (section D) for Vault's identity-based access, and authorization (section E) for fine-grained secret access policies. A secret manager is not a standalone tool — it is the integration point where all security primitives converge to protect the most sensitive data in your infrastructure.

---

## G. OWASP Top 10

### Theory

The OWASP (Open Worldwide Application Security Project) Top 10 is a standard awareness document for web application security. It represents the most critical security risks to web applications, based on broad consensus from security experts and data from hundreds of organizations.

Understanding each vulnerability, why it works, and how to prevent it is essential for building secure applications.

#### 1. Broken Access Control (A01:2021)

The most prevalent web application vulnerability. Occurs when users can act outside their intended permissions.

**IDOR (Insecure Direct Object Reference)**: The application exposes internal object identifiers (database IDs, file paths) without verifying that the requesting user is authorized to access them.

```
# Vulnerable: User 123 can access user 456's data by changing the URL
GET /api/users/456/profile
Authorization: Bearer <token_for_user_123>

# The server returns user 456's data without checking
# whether user 123 is authorized to view it
```

**Mitigation**: Check authorization on every request. The server must verify that the authenticated user has permission to access the requested resource. Never rely on client-side controls (hidden form fields, disabled buttons) for access control.

#### 2. Cryptographic Failures (A02:2021)

Formerly "Sensitive Data Exposure." Occurs when data is not properly protected through encryption.

**Examples:**
- Transmitting passwords over HTTP (not HTTPS)
- Storing passwords with MD5 or SHA-256 instead of Argon2/bcrypt
- Using weak or deprecated algorithms (DES, RC4, MD5)
- Failing to encrypt sensitive data at rest (PII, credit cards, health records)
- Hardcoding encryption keys in source code

**Mitigation**: Encrypt data in transit (TLS 1.2+). Encrypt sensitive data at rest (AES-256-GCM). Use strong password hashing (Argon2). Classify data by sensitivity and apply appropriate protections.

#### 3. Injection (A03:2021)

Injection occurs when untrusted input is sent to an interpreter as part of a command or query. The attacker's input is executed as code.

**SQL Injection:**

```sql
-- Vulnerable: User input concatenated into query
query = "SELECT * FROM users WHERE username = '" + user_input + "'"

-- Attacker input: ' OR '1'='1' --
-- Resulting query:
SELECT * FROM users WHERE username = '' OR '1'='1' --'
-- Returns ALL users
```

**Command Injection:**

```python
# Vulnerable: User input passed to shell command
os.system("ping " + user_input)

# Attacker input: 127.0.0.1; cat /etc/passwd
# Resulting command:
# ping 127.0.0.1; cat /etc/passwd
```

**Mitigation**: Use parameterized queries (prepared statements) for SQL. Never concatenate user input into queries or commands. Use allowlists for input validation. Apply the principle of least privilege to database accounts.

```python
# Safe: Parameterized query
cursor.execute("SELECT * FROM users WHERE username = %s", (user_input,))
```

#### 4. Insecure Design (A04:2021)

Flaws in the design itself, not the implementation. No amount of perfect coding fixes a broken design.

**Example**: A password reset flow that emails a reset link with a predictable token (sequential integer or short random string). Even if the implementation is bug-free, the design is insecure because tokens can be guessed.

**Mitigation**: Use threat modeling (STRIDE) during design. Follow secure design patterns. Perform design reviews with security-aware engineers.

#### 5. Security Misconfiguration (A05:2021)

The application, framework, server, or cloud service is configured insecurely.

**Examples:**
- Default credentials left unchanged (`admin/admin`)
- Unnecessary features enabled (directory listing, debug endpoints, sample applications)
- Verbose error messages exposing stack traces, database versions, or file paths to users
- Missing security headers (`X-Frame-Options`, `Content-Security-Policy`, `Strict-Transport-Security`)
- S3 buckets with public read access
- Cloud security groups with `0.0.0.0/0` ingress on all ports

**Mitigation**: Harden all environments. Remove defaults. Automate configuration with infrastructure as code. Regularly audit configurations against baselines (CIS Benchmarks).

#### 6. Vulnerable and Outdated Components (A06:2021)

Using libraries, frameworks, or system components with known vulnerabilities.

**Examples:**
- Running an application with a dependency that has a known remote code execution vulnerability
- Using an end-of-life operating system or runtime that no longer receives security patches
- Not monitoring dependency advisories

**Mitigation**: Maintain an inventory of all components and their versions. Monitor CVE databases and advisory feeds. Automate dependency updates with Dependabot, Renovate, or equivalent. Remove unused dependencies.

#### 7. Identification and Authentication Failures (A07:2021)

Weaknesses in the authentication mechanism.

**Examples:**
- Permitting brute-force attacks (no rate limiting, no account lockout)
- Allowing weak passwords
- Credential stuffing (attackers use leaked passwords from other sites)
- Session IDs in URLs
- Not invalidating sessions after logout or password change

**Mitigation**: Implement MFA. Use strong password policies (length over complexity). Rate-limit authentication attempts. Invalidate sessions properly. Use proven authentication frameworks (do not roll your own).

#### 8. Software and Data Integrity Failures (A08:2021)

Failures related to code and infrastructure that does not protect against integrity violations.

**Examples:**
- Insecure deserialization — untrusted data is deserialized, allowing arbitrary object creation or code execution
- CI/CD pipeline without integrity verification — an attacker modifies a build artifact or dependency
- Auto-update mechanisms without signature verification

**Mitigation**: Verify digital signatures on software updates and dependencies. Use integrity checks (checksums, SBOMs). Ensure CI/CD pipelines are tamper-resistant. Avoid deserializing untrusted data.

#### 9. Security Logging and Monitoring Failures (A09:2021)

Insufficient logging, detection, or response capability.

**Examples:**
- Login failures not logged
- No alerting on suspicious patterns (multiple failed logins, privilege escalation attempts)
- Logs not protected from tampering
- No incident response plan

**Mitigation**: Log all authentication events, access control failures, and input validation failures. Centralize logs in a SIEM. Set up alerts for anomalous patterns. Test incident response procedures regularly.

#### 10. Server-Side Request Forgery (SSRF) (A10:2021)

The application fetches a remote resource based on user-supplied input without validating the destination.

```
# Vulnerable: Application fetches user-supplied URL
POST /api/fetch-url
{"url": "http://169.254.169.254/latest/meta-data/iam/security-credentials/"}

# The server fetches the AWS metadata endpoint
# and returns IAM credentials to the attacker
```

**Mitigation**: Validate and sanitize all user-supplied URLs. Deny access to internal/private IP ranges. Use allowlists for permitted destinations. Disable HTTP redirects or validate the redirect target.

#### XSS (Cross-Site Scripting)

XSS deserves special attention because of its prevalence. It occurs when an application includes untrusted data in a web page without proper encoding.

| Type | Description | Example |
|------|------------|---------|
| **Reflected** | Malicious input is reflected from the request to the response | Search query `<script>alert(1)</script>` rendered in results page |
| **Stored** | Malicious input is stored in the database and served to other users | Comment containing `<script>` tag executed when other users view it |
| **DOM-based** | Client-side JavaScript processes untrusted data and modifies the DOM | `document.getElementById('output').innerHTML = location.hash` |

**Mitigation:**
- **Output encoding** — Encode all user-supplied data before rendering in HTML (`&lt;` instead of `<`)
- **Content Security Policy (CSP)** — HTTP header that restricts which scripts can execute: `Content-Security-Policy: script-src 'self'`
- **Use frameworks with auto-escaping** — React, Angular, and Vue auto-escape by default (unless you use `dangerouslySetInnerHTML` or equivalent)

### Practice

> **Try It**: Set up DVWA (Damn Vulnerable Web Application) or OWASP Juice Shop in a Docker container. These are intentionally vulnerable applications designed for security practice.
>
> ```bash
> # Run Juice Shop
> docker run -d -p 3000:3000 bkimminich/juice-shop
>
> # Or run DVWA
> docker run -d -p 8080:80 vulnerables/web-dvwa
> ```
>
> Work through at least three vulnerability categories: SQL injection (use the login bypass), XSS (inject a script tag), and broken access control (access another user's data by modifying a request parameter).

> **Try It**: Inspect the HTTP response headers of five websites you use regularly. Check for the presence of `Content-Security-Policy`, `Strict-Transport-Security`, `X-Frame-Options`, and `X-Content-Type-Options`. Use browser developer tools (Network tab) or `curl -I https://example.com`. Identify which sites are missing security headers.

### Connection

The OWASP Top 10 is where every prior section converges on real-world attacks. Cryptographic failures (A02) connect directly to section B. Authentication failures (A07) connect to section D. Broken access control (A01) connects to section E. Vulnerable components (A06) connect to supply chain security (section I). Understanding the vulnerabilities is necessary; understanding the underlying principles that make mitigations work is what this domain provides.

---

## H. System Hardening

### Theory

System hardening is the process of reducing a system's attack surface by disabling unnecessary features, configuring security controls, and applying patches. A freshly installed system is optimized for usability, not security. Hardening aligns the system's configuration with its security requirements.

This section focuses on Linux hardening, building directly on the knowledge from [Operating Systems and Linux](/learn/first-principles/operating-systems-and-linux/).

#### CIS Benchmarks

CIS (Center for Internet Security) Benchmarks are consensus-based configuration guidelines. They provide specific, actionable settings for hardening operating systems, cloud platforms, network devices, and applications. CIS Benchmarks are organized into scored (measurable) and unscored (advisory) recommendations.

Key areas covered by CIS Linux Benchmarks:

#### SSH Hardening

SSH is the primary remote access mechanism for Linux servers. Misconfigurations are a common attack vector.

| Setting | Insecure Default | Hardened Value | Why |
|---------|-----------------|----------------|-----|
| `PermitRootLogin` | yes | **no** | Root login provides direct access to the highest-privilege account |
| `PasswordAuthentication` | yes | **no** | Passwords are brutable; keys are not |
| `PubkeyAuthentication` | yes | **yes** | Key-based authentication is cryptographically strong |
| `MaxAuthTries` | 6 | **3** | Limits brute-force attempts per connection |
| `AllowUsers` | (all) | **specific users** | Restricts SSH access to named accounts |
| `Protocol` | 2 | **2** | SSHv1 is broken, ensure only v2 is used |
| `X11Forwarding` | yes | **no** | Disable if not needed, reduces attack surface |

```bash
# /etc/ssh/sshd_config hardened settings
PermitRootLogin no
PasswordAuthentication no
PubkeyAuthentication yes
MaxAuthTries 3
AllowUsers deployer admin
X11Forwarding no
ClientAliveInterval 300
ClientAliveCountMax 2
```

#### Firewall Configuration

A firewall should operate on a **default-deny** (allow-list) policy: block all traffic, then explicitly allow only what is needed.

```bash
# UFW (Uncomplicated Firewall) — Ubuntu
# Set default policies
sudo ufw default deny incoming
sudo ufw default allow outgoing

# Allow only necessary services
sudo ufw allow 22/tcp    # SSH
sudo ufw allow 443/tcp   # HTTPS
sudo ufw allow 80/tcp    # HTTP (redirect to HTTPS)

# Enable the firewall
sudo ufw enable

# Verify rules
sudo ufw status verbose
```

```bash
# iptables — Direct approach
# Flush existing rules
sudo iptables -F

# Default deny
sudo iptables -P INPUT DROP
sudo iptables -P FORWARD DROP
sudo iptables -P OUTPUT ACCEPT

# Allow loopback
sudo iptables -A INPUT -i lo -j ACCEPT

# Allow established connections
sudo iptables -A INPUT -m state --state ESTABLISHED,RELATED -j ACCEPT

# Allow SSH
sudo iptables -A INPUT -p tcp --dport 22 -j ACCEPT

# Allow HTTPS
sudo iptables -A INPUT -p tcp --dport 443 -j ACCEPT
```

#### Package Minimization

Every installed package is potential attack surface. Minimization means removing everything that is not required for the system's function.

```bash
# List installed packages
dpkg --list | wc -l

# Remove unnecessary packages
sudo apt purge telnet rsh-client rsh-redone-client
sudo apt autoremove

# Disable unnecessary services
sudo systemctl disable cups
sudo systemctl disable avahi-daemon
sudo systemctl disable bluetooth
```

#### Automatic Security Updates

```bash
# Ubuntu — unattended-upgrades
sudo apt install unattended-upgrades
sudo dpkg-reconfigure unattended-upgrades

# Verify configuration
cat /etc/apt/apt.conf.d/50unattended-upgrades
# Ensure security updates are enabled
```

#### File Permission Auditing

```bash
# Find world-writable files
find / -type f -perm -0002 -ls 2>/dev/null

# Find SUID/SGID binaries (potential privilege escalation)
find / -type f \( -perm -4000 -o -perm -2000 \) -ls 2>/dev/null

# Find files with no owner
find / -nouser -o -nogroup 2>/dev/null

# Ensure critical files have correct permissions
chmod 600 /etc/shadow
chmod 644 /etc/passwd
chmod 600 /etc/ssh/sshd_config
```

#### Audit Logging with auditd

`auditd` provides kernel-level auditing for security-relevant events:

```bash
# Install auditd
sudo apt install auditd audispd-plugins

# Add audit rules
# Monitor changes to /etc/passwd
sudo auditctl -w /etc/passwd -p wa -k identity

# Monitor changes to SSH configuration
sudo auditctl -w /etc/ssh/sshd_config -p wa -k sshd_config

# Monitor use of privileged commands
sudo auditctl -a always,exit -F path=/usr/bin/sudo -F perm=x -k sudo_usage

# Search audit logs
sudo ausearch -k identity
sudo aureport --auth
```

#### Mandatory Access Control: SELinux and AppArmor

Discretionary Access Control (DAC) — standard Unix permissions — lets file owners control access. Mandatory Access Control (MAC) enforces system-wide security policies that override DAC.

| Feature | SELinux | AppArmor |
|---------|---------|----------|
| Default in | RHEL, CentOS, Fedora | Ubuntu, SUSE |
| Policy model | Type enforcement, labeled security | Path-based profiles |
| Complexity | Higher | Lower |
| Granularity | Fine-grained | Per-application |

```bash
# SELinux — Check and set enforcing mode
getenforce                    # Permissive, Enforcing, or Disabled
sudo setenforce 1             # Set to Enforcing
# Permanent: edit /etc/selinux/config, set SELINUX=enforcing

# AppArmor — Check status
sudo aa-status

# Set a profile to enforce mode
sudo aa-enforce /etc/apparmor.d/usr.sbin.nginx
```

Running MAC in enforcing mode prevents processes from accessing resources outside their defined policy, even if Unix permissions would allow it. A compromised web server running under an AppArmor profile cannot read `/etc/shadow` even if the process runs as root, because the profile does not grant that access.

#### Kernel Hardening with sysctl

```bash
# /etc/sysctl.d/99-hardening.conf

# Disable IP forwarding (unless this is a router)
net.ipv4.ip_forward = 0

# Disable ICMP redirects
net.ipv4.conf.all.accept_redirects = 0
net.ipv4.conf.default.accept_redirects = 0

# Enable SYN flood protection
net.ipv4.tcp_syncookies = 1

# Disable source routing
net.ipv4.conf.all.accept_source_route = 0

# Enable reverse path filtering (anti-spoofing)
net.ipv4.conf.all.rp_filter = 1

# Log suspicious packets
net.ipv4.conf.all.log_martians = 1

# Disable core dumps for SUID programs
fs.suid_dumpable = 0

# Randomize memory layout (ASLR)
kernel.randomize_va_space = 2
```

```bash
# Apply settings
sudo sysctl -p /etc/sysctl.d/99-hardening.conf
```

#### Vulnerability Scanners

| Tool | Scope | Use Case |
|------|-------|----------|
| **Lynis** | Host-level security audit | Comprehensive Linux hardening audit with scoring |
| **Trivy** | Containers, filesystems, repos | Vulnerability scanning for OS packages and application dependencies |
| **OpenSCAP** | Compliance checking | Automated CIS Benchmark compliance verification |

### Practice

```bash
# Run Lynis audit
sudo apt install lynis
sudo lynis audit system

# Review the output — Lynis provides a hardening index (0-100)
# and specific recommendations for improvement
```

```bash
# Scan a container image with Trivy
trivy image ubuntu:22.04
trivy image nginx:latest

# Trivy reports CVEs with severity levels and fix versions
```

> **Try It**: Take a fresh Ubuntu VM (or container) and harden it systematically:
> 1. Disable root SSH login and enable key-based auth only
> 2. Configure UFW with a default-deny incoming policy, allowing only SSH and HTTPS
> 3. Remove unnecessary packages and disable unused services
> 4. Enable automatic security updates
> 5. Configure auditd to monitor `/etc/passwd`, `/etc/shadow`, and `/etc/ssh/sshd_config`
> 6. Enable AppArmor in enforce mode
> 7. Apply the sysctl hardening settings above
> 8. Run Lynis before and after hardening. Compare the hardening index scores.

### Connection

System hardening implements defense in depth (section A) at the operating system layer. SSH key-based authentication applies the cryptographic principles from section B. Firewall rules enforce least privilege at the network level. MAC systems extend the authorization models from section E to the kernel level. Hardening is not a one-time activity — it is a continuous process that must be automated (infrastructure as code, covered in [Infrastructure at Scale](/learn/first-principles/infrastructure-at-scale/)) and audited.

---

## I. Supply Chain Security

### Theory

Your software is not just your code. It includes every dependency, every base image, every build tool, and every CI/CD plugin. A supply chain attack compromises one of these components to attack all downstream consumers.

High-profile supply chain attacks demonstrate the severity:

- **SolarWinds (2020)** — Attackers compromised the build system and injected malicious code into a software update distributed to 18,000 organizations, including US government agencies.
- **Log4Shell (2021)** — A critical vulnerability in Log4j (a ubiquitous Java logging library) affected virtually every Java application. Organizations could not even determine which of their systems used Log4j.
- **Codecov (2021)** — Attackers modified a bash uploader script, exfiltrating environment variables (including secrets) from CI/CD pipelines of thousands of projects.
- **xz Utils (2024)** — A patient, multi-year social engineering campaign inserted a backdoor into a compression library used by SSH on many Linux distributions.

#### Dependency Scanning

```bash
# Node.js — npm audit
npm audit
npm audit fix

# Python — pip-audit
pip install pip-audit
pip-audit

# Go — govulncheck
go install golang.org/x/vuln/cmd/govulncheck@latest
govulncheck ./...

# Universal — Trivy (filesystem scan)
trivy fs --scanners vuln /path/to/project

# Automated via CI — Dependabot (GitHub), Renovate (multi-platform)
# Dependabot automatically opens PRs to update vulnerable dependencies
```

#### SBOM (Software Bill of Materials)

An SBOM is a complete inventory of all components in a software artifact — every library, every transitive dependency, every version number. It is the software equivalent of a nutritional label.

| Format | Standard Body | Use Case |
|--------|--------------|----------|
| **CycloneDX** | OWASP | Lightweight, JSON/XML, designed for security |
| **SPDX** | Linux Foundation | Comprehensive, includes licensing information |

```bash
# Generate an SBOM with Trivy
trivy fs --format cyclonedx --output sbom.json /path/to/project

# Generate an SBOM for a container image
trivy image --format spdx-json --output sbom.json nginx:latest
```

SBOMs enable rapid response to vulnerabilities. When Log4Shell was disclosed, organizations with SBOMs could immediately identify affected systems. Organizations without them spent weeks discovering where Log4j was used.

#### Artifact Signing

Signing build artifacts proves their provenance — that they were built from specific source code by a specific build system, and have not been modified since.

| Tool | What It Signs | How |
|------|--------------|-----|
| **Sigstore/cosign** | Container images, blobs | Keyless signing using OIDC identity, transparent logs |
| **Notary** | Container images | Docker Content Trust, TUF framework |

```bash
# Sign a container image with cosign
cosign sign --yes myregistry/myimage:v1.0

# Verify the signature
cosign verify myregistry/myimage:v1.0

# Cosign uses Sigstore's Rekor transparency log
# Anyone can verify that the signature was recorded
```

#### Reproducible Builds

A build is reproducible if building the same source code with the same build environment always produces a bit-identical output. Reproducible builds allow independent verification that a binary was built from the claimed source code.

Without reproducible builds, you must trust the build system. A compromised build system can inject arbitrary code that will not appear in the source code repository. With reproducible builds, anyone can rebuild from source and verify the output matches.

#### SLSA Framework

SLSA (Supply-chain Levels for Software Artifacts) is a framework that defines increasing levels of supply chain integrity:

| Level | Requirements |
|-------|-------------|
| **SLSA 1** | Build process is documented and automated |
| **SLSA 2** | Build service generates and signs provenance (metadata about how the artifact was built) |
| **SLSA 3** | Build platform is hardened, provenance is non-forgeable |
| **SLSA 4** | Hermetic, reproducible builds with two-person review |

SLSA provides a maturity model — organizations can incrementally improve their supply chain security rather than attempting everything at once.

### Practice

> **Try It**: Run `npm audit` or `pip-audit` on a project you maintain. Review the findings. For each vulnerability, read the advisory — understand what the vulnerability is, how it is exploited, and what the fix is. Update the vulnerable dependencies and re-scan.

> **Try It**: Generate an SBOM for a project or container image using Trivy. Open the JSON output and examine it. Count the total number of dependencies (including transitive dependencies). This number is your supply chain attack surface.

### Connection

Supply chain security applies cryptography (section B) through artifact signing, PKI (section C) through certificate-based trust in Sigstore, and secrets management (section F) through CI/CD pipeline security. The OWASP Top 10 (section G) includes "Vulnerable and Outdated Components" as a dedicated category. Supply chain security is not a niche concern — it is a fundamental property of modern software delivery.

---

## J. Incident Response

### Theory

Security incidents are inevitable. The quality of your response determines whether an incident is a brief disruption or a catastrophe. Incident response (IR) is the structured process for handling security events.

#### The Six Phases

```mermaid
flowchart LR
    P["1. Preparation"] --> D["2. Detection &<br/>Analysis"]
    D --> C["3. Containment"]
    C --> E["4. Eradication"]
    E --> R["5. Recovery"]
    R --> L["6. Lessons<br/>Learned"]
    L --> P
```

| Phase | Activities |
|-------|-----------|
| **1. Preparation** | IR plan, runbooks, communication templates, team roles, tools, training exercises |
| **2. Detection & Analysis** | SIEM alerts, anomaly detection, log analysis, triage, severity classification |
| **3. Containment** | Short-term: isolate affected systems. Long-term: apply temporary fixes while preserving evidence |
| **4. Eradication** | Remove the root cause — patch vulnerability, remove malware, revoke compromised credentials |
| **5. Recovery** | Restore systems from clean backups, monitor for recurrence, gradual return to production |
| **6. Lessons Learned** | Blameless post-mortem, timeline reconstruction, identify gaps, update procedures |

#### Math: Bayes' Theorem for False Positive Reasoning

Anomaly detection systems (IDS, SIEM rules, ML-based detectors) generate alerts. Not all alerts represent real attacks — some are false positives. Bayes' theorem quantifies how to reason about this:

```
P(Attack | Alert) = P(Alert | Attack) * P(Attack)
                    -----------------------------------
                                P(Alert)
```

Where:
- `P(Attack | Alert)` = Probability that an alert is a real attack (what we want to know)
- `P(Alert | Attack)` = True positive rate (sensitivity) of the detector
- `P(Attack)` = Prior probability of an attack (base rate)
- `P(Alert)` = Total probability of any alert = `P(Alert | Attack) * P(Attack) + P(Alert | No Attack) * P(No Attack)`

**Example**: An IDS has a 99% true positive rate and a 1% false positive rate. Attacks occur 0.1% of the time.

```
P(Attack | Alert) = (0.99 * 0.001) / (0.99 * 0.001 + 0.01 * 0.999)
                  = 0.00099 / (0.00099 + 0.00999)
                  = 0.00099 / 0.01098
                  ≈ 0.09 = 9%
```

Despite a 99% detection rate, only 9% of alerts are real attacks. This is the **base rate fallacy** — when the underlying event is rare, even highly accurate detectors produce mostly false positives. This is why alert triage, tuning, and context-enrichment are essential in security operations.

#### SIEM (Security Information and Event Management)

A SIEM aggregates logs from all systems (servers, network devices, applications, cloud services), normalizes them, correlates events, and generates alerts based on rules and anomaly detection.

SIEM components:
- **Log collection** — Agents, syslog, API integrations
- **Normalization** — Converting disparate log formats into a common schema
- **Correlation** — Identifying related events across systems (e.g., failed login on server A followed by successful login on server B from the same IP)
- **Alerting** — Rules-based and ML-based detection of suspicious patterns
- **Investigation** — Search and visualization tools for analysts

Common SIEM platforms: Splunk, Elastic SIEM, Microsoft Sentinel, Google Chronicle, Datadog Security Monitoring.

#### Blameless Post-Mortems

After an incident, the goal is learning, not punishment. A blameless post-mortem focuses on systemic factors:

- **What happened?** — Detailed timeline of events
- **Why did it happen?** — Root cause analysis (5 Whys, fault tree analysis)
- **What did we do well?** — What worked in the response
- **What could we improve?** — Gaps in detection, response, or prevention
- **Action items** — Concrete, assigned, time-bound improvements

The post-mortem document is shared widely. It becomes institutional knowledge that prevents the same class of incident from recurring.

### Practice

> **Try It**: Write an incident response plan for a scenario: "A developer's laptop is stolen, and it contained SSH keys and AWS credentials." Walk through all six phases. What do you do in each phase? Who do you notify? What credentials do you revoke? How do you verify the scope of exposure? What changes do you make afterward?

> **Try It**: Apply Bayes' theorem: Your web application firewall (WAF) blocks 95% of SQL injection attacks (true positive rate) and has a 2% false positive rate (blocks legitimate requests). If 0.5% of requests are actually malicious, what percentage of blocked requests are real attacks? Show your calculation.

### Connection

Incident response depends on the security logging from section G (OWASP A09), the audit trails from section H (auditd), and the secrets rotation from section F (compromised credential response). Detection relies on understanding what normal looks like — which requires the networking knowledge from [Networking](/learn/first-principles/networking/) and the system knowledge from [Operating Systems and Linux](/learn/first-principles/operating-systems-and-linux/). Every domain contributes to incident response capability.

---

## K. Compliance

### Theory

Compliance frameworks define minimum security and privacy requirements that organizations must meet. They exist because self-regulation has historically failed to protect sensitive data. Compliance is not security — a compliant system can still be insecure, and a secure system may not be compliant — but compliance frameworks codify baseline security practices that every organization should implement.

#### SOC 2

**Scope**: Service organizations that store, process, or transmit customer data.

**What it requires**: Controls across five Trust Services Criteria — Security (required), Availability, Processing Integrity, Confidentiality, Privacy (optional). Attestation by an independent auditor. Continuous monitoring over an audit period (typically 12 months for Type II).

**Why it matters**: SOC 2 Type II reports are the standard mechanism for enterprise customers to evaluate a vendor's security posture. Most B2B SaaS companies need SOC 2 to sell to enterprise customers.

#### ISO 27001

**Scope**: Any organization, any industry, globally recognized.

**What it requires**: An Information Security Management System (ISMS) — a formal management framework for information security. 93 controls across organizational, people, physical, and technological categories. Certification by an accredited auditor. Annual surveillance audits.

**Why it matters**: ISO 27001 is the international standard for information security management. It is recognized globally and often required for government contracts and international business.

#### PCI DSS

**Scope**: Any organization that stores, processes, or transmits credit card data.

**What it requires**: 12 requirement categories including network segmentation, encryption, access control, vulnerability management, monitoring, and security policies. Quarterly vulnerability scans. Annual assessment (self-assessment questionnaire for small merchants, on-site audit for large processors).

**Why it matters**: Non-compliance results in fines, increased transaction fees, and potential loss of the ability to process credit cards. A data breach with non-compliance massively increases liability.

#### HIPAA

**Scope**: Healthcare organizations and their business associates in the United States.

**What it requires**: Administrative safeguards (risk analysis, workforce training, incident response), physical safeguards (facility access, workstation security), and technical safeguards (access control, audit controls, transmission security, encryption). Privacy Rule governs use and disclosure of Protected Health Information (PHI). Breach notification within 60 days.

**Why it matters**: HIPAA violations carry penalties up to $1.9 million per violation category per year. Criminal penalties (up to 10 years imprisonment) apply for willful neglect.

#### GDPR

**Scope**: Any organization that processes personal data of EU/EEA residents, regardless of where the organization is located.

**What it requires**: Lawful basis for data processing (consent, contract, legitimate interest). Data subject rights (access, rectification, erasure, portability). Data Protection Impact Assessments for high-risk processing. Data breach notification within 72 hours. Data Protection Officer for certain organizations.

**Why it matters**: Fines up to 4% of global annual revenue or 20 million euros, whichever is greater. Extraterritorial reach means non-EU companies are subject to GDPR if they process EU residents' data.

#### Comparison

| Framework | Scope | Geographic | Mandatory | Penalty for Non-Compliance |
|-----------|-------|-----------|:-:|------|
| SOC 2 | Service orgs | US-centric | Market-driven | Loss of customers |
| ISO 27001 | Any org | Global | Market-driven | Loss of contracts |
| PCI DSS | Card data | Global | Yes (contractual) | Fines, lost processing ability |
| HIPAA | Healthcare | US | Yes (law) | Fines up to $1.9M/category/year, criminal penalties |
| GDPR | Personal data | EU/EEA (extraterritorial) | Yes (law) | Up to 4% global revenue |

### Practice

> **Try It**: For an application you use or are building, determine which compliance frameworks apply. Consider: Does it handle credit card data (PCI DSS)? Health records (HIPAA)? Data from EU residents (GDPR)? Customer data for enterprise clients (SOC 2)? For each applicable framework, list three specific technical controls you would need to implement.

> **Try It**: Download the CIS Benchmark for Ubuntu Linux (free registration required at cisecurity.org). Read through the first 20 recommendations. For each recommendation, identify which compliance framework(s) it helps satisfy and which CIA triad property it enforces.

### Connection

Compliance is the organizational context for every technical control in this domain. The encryption from section B satisfies PCI DSS requirement 4 (encrypt transmission of cardholder data) and HIPAA's transmission security. The access controls from section E satisfy SOC 2's logical access criteria and GDPR's data access requirements. The audit logging from section H satisfies PCI DSS requirement 10 (track and monitor all access). The incident response procedures from section J satisfy GDPR's 72-hour breach notification requirement. Compliance frameworks do not exist in isolation — they are checklists that map directly to the technical controls you have learned to implement throughout this domain.

---

## Exercises

### Fundamentals

1. Map the following security controls to the CIA triad properties they enforce: TLS encryption, database backups, multi-factor authentication, firewall rules, digital signatures, DDoS protection, access control lists, audit logging.

2. Perform a STRIDE threat model for a three-tier web application (client, API server, database). Identify at least two threats per STRIDE category and propose mitigations.

3. Calculate the risk (likelihood x impact) for three threats to a public-facing e-commerce application: SQL injection, DDoS attack, and insider data theft. Justify your likelihood and impact ratings.

### Cryptography

4. Generate RSA and Ed25519 key pairs using `openssl`. Compare the file sizes and generation times. Explain why Ed25519 keys are smaller yet provide equivalent security.

5. Encrypt a file with AES-256-GCM and decrypt it. Then modify one byte of the ciphertext and attempt to decrypt. Observe and explain the authentication failure.

6. Using the RSA math: Given `p = 61`, `q = 53`, compute `n`, `phi(n)`, choose `e = 17`, and derive `d`. Encrypt the message `m = 42` and decrypt the result. Show all steps.

7. Demonstrate the avalanche effect: Hash a file with SHA-256, change one character, hash again, and count the number of differing hex digits.

### PKI

8. Generate a self-signed CA, issue two leaf certificates for different domains, and verify both chains. Then revoke one certificate by generating a CRL and verify that the revoked certificate fails validation.

9. Inspect the certificate chain of five live HTTPS websites. For each, record: the leaf certificate's SAN entries, the intermediate CA, the root CA, and the expiration date. Identify which root CAs appear most frequently.

### Auth and Secrets

10. Set up HashiCorp Vault in dev mode. Store three different secrets. Create two policies with different access levels. Create tokens for each policy and verify access restrictions.

11. Configure a Git repository with `gitleaks` as a pre-commit hook. Attempt to commit a file containing an AWS-style access key. Verify that the commit is blocked.

12. Explain the OAuth 2.0 Authorization Code flow. Draw the sequence diagram from memory. Identify where each security mechanism (TLS, client secret, authorization code, access token) provides protection.

### Hardening

13. Take a fresh Ubuntu VM and harden it following the steps in section H. Run Lynis before and after hardening. Document the improvement in the hardening index and list every change you made.

14. Write iptables rules that allow only SSH (port 22) from a specific IP address and HTTPS (port 443) from anywhere. Block all other incoming traffic. Test by attempting connections from allowed and disallowed sources.

### Application Security

15. Set up OWASP Juice Shop and exploit at least five different vulnerability categories. For each, document: the vulnerability type, how you exploited it, why the vulnerability exists, and how to fix it.

16. Write a Python script that demonstrates SQL injection against a vulnerable query, then fix the query using parameterized queries. Show both the vulnerable and secure versions.

### Supply Chain and IR

17. Generate an SBOM for a project using Trivy. Count the total number of dependencies. Run a vulnerability scan and categorize findings by severity.

18. Write a complete incident response plan for the scenario: "An attacker gains access to your CI/CD pipeline and modifies a deployment script." Cover all six IR phases.

### Compliance

19. For a hypothetical healthcare SaaS application, identify which compliance frameworks apply and list ten specific technical controls that address requirements across multiple frameworks simultaneously.

20. Apply Bayes' theorem: Your anomaly detection system has a 97% true positive rate and a 3% false positive rate. If actual attacks represent 0.2% of all events, what percentage of alerts are true attacks? What does this mean for your SOC team's workload?

---

## Assessment Dimensions

### Explain

You can describe the CIA triad and map any security control to the principle it enforces. You can explain symmetric and asymmetric encryption with the underlying mathematics — modular arithmetic, Euler's totient, modular inverse. You can describe how RSA key generation works at the number theory level. You can explain the difference between OAuth 2.0 and OIDC. You can describe the OWASP Top 10 vulnerabilities and articulate why each mitigation works. You can explain why hardcoded secrets are a critical vulnerability and how secret managers solve the problem. You can apply Bayes' theorem to reason about false positive rates in detection systems.

### Build

You can generate RSA and Ed25519 keys. You can encrypt and decrypt data with AES-256-GCM. You can create a self-signed CA and issue leaf certificates. You can set up and operate HashiCorp Vault with appropriate policies. You can harden a Linux system following CIS Benchmark guidelines and verify the hardening with Lynis. You can configure SSH, firewalls, and mandatory access controls. You can generate SBOMs and scan for vulnerabilities. You can write parameterized queries that prevent SQL injection.

### Debug

Given a TLS connection failure, you can inspect the certificate chain with `openssl s_client`, identify the broken link (expired certificate, hostname mismatch, untrusted CA), and fix it. Given a system hardening audit that fails specific CIS checks, you can identify the misconfigured settings and correct them. Given a security alert, you can apply Bayes' theorem to assess whether the alert is likely a true positive, and reason through the incident response process. Given a broken authentication flow (OAuth/OIDC), you can trace the token exchange, identify where the failure occurs, and fix it.

---

## Key Takeaways

- The CIA triad (Confidentiality, Integrity, Availability) is the foundation of all security reasoning — every control maps to at least one property
- Cryptography is mathematics, not magic — modular arithmetic, Euler's totient, and modular exponentiation make RSA provably secure
- Symmetric encryption (AES-GCM) provides fast bulk encryption; asymmetric encryption (RSA, Ed25519) solves key distribution and digital signatures
- Hashing is one-way and deterministic; password hashing (Argon2) is intentionally slow to resist brute-force
- PKI creates a trust hierarchy from root CAs to leaf certificates, enabling HTTPS and all identity verification on the internet
- Authentication verifies identity through knowledge, possession, and inherence factors — MFA requires multiple factor types
- OAuth 2.0 is for authorization; OIDC is for authentication — confusing them creates vulnerabilities
- Authorization models (RBAC, ABAC, ACLs) enforce least privilege at different granularities
- Hardcoded secrets are a critical vulnerability — use secret managers with proper access control and rotation
- The OWASP Top 10 represents the most prevalent web application vulnerabilities — broken access control and injection are consistently at the top
- System hardening reduces attack surface through SSH configuration, firewall rules, package minimization, MAC enforcement, and kernel parameter tuning
- Supply chain attacks compromise dependencies, not your code — SBOMs, dependency scanning, and artifact signing are essential
- Incident response follows six phases — preparation and lessons learned are where most value is created
- Compliance frameworks (SOC 2, ISO 27001, PCI DSS, HIPAA, GDPR) codify baseline security requirements; compliance is necessary but not sufficient for security
- Bayes' theorem explains why even accurate detection systems produce mostly false positives when the base rate of attacks is low

---

## Resources & Further Reading

- [OWASP Top 10 (2021)](https://owasp.org/www-project-top-ten/)
- [OWASP Juice Shop](https://owasp.org/www-project-juice-shop/)
- [CIS Benchmarks](https://www.cisecurity.org/cis-benchmarks)
- [NIST Cryptographic Standards](https://csrc.nist.gov/publications)
- [Let's Encrypt — How It Works](https://letsencrypt.org/how-it-works/)
- [HashiCorp Vault Documentation](https://developer.hashicorp.com/vault/docs)
- [SLSA Framework](https://slsa.dev/)
- [Sigstore](https://www.sigstore.dev/)
- [OpenSSL Cookbook](https://www.feistyduck.com/library/openssl-cookbook/)
- [RFC 6238 — TOTP](https://datatracker.ietf.org/doc/html/rfc6238)
- [RFC 6749 — OAuth 2.0](https://datatracker.ietf.org/doc/html/rfc6749)
- [OpenID Connect Specification](https://openid.net/connect/)
- [NIST SP 800-61 — Incident Handling Guide](https://csrc.nist.gov/publications/detail/sp/800-61/rev-2/final)
- [GDPR Full Text](https://gdpr.eu/)
- [Trivy — Vulnerability Scanner](https://trivy.dev/)
- [Lynis — Security Auditing Tool](https://cisofy.com/lynis/)
- [Gitleaks](https://github.com/gitleaks/gitleaks)
