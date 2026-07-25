# Hashcat

Hashcat is a password recovery tool used to recover passwords from hashes.

It **does not decrypt hashes**.

Instead, it repeatedly hashes password guesses and compares them with the target hash until a match is found.

Hashcat is one of the fastest password cracking tools and is widely used during security assessments, password audits, penetration testing, and CTFs.

---

# Why Do We Need Hashcat?

Suppose a database is leaked.

Instead of passwords, it contains hashes.

Example:

```
Username            Password Hash

Girish     → 5f4dcc3b5aa765d61d8327deb882cf99
Rahul      → ef92b778bafe771e...
```

You cannot directly read the original password from the hash.

Hashcat helps recover the password by trying different password guesses.

---

# How Hashcat Works

Hashcat follows a simple process.

```
Candidate Password
        │
        ▼
Hash Function
(MD5 / SHA256 / NTLM ...)
        │
        ▼
Generated Hash
        │
        ▼
Compare with Target Hash
        │
        ├── Match
        │      │
        │      ▼
        │ Password Found ✅
        │
        └── No Match
               │
               ▼
        Try Next Password
```

Hashcat repeats this process millions (or even billions) of times until a match is found.

---

# Hashcat Does NOT Decrypt

A common misconception is that Hashcat "decrypts" hashes.

It doesn't.

It simply keeps guessing passwords.

```
Guess Password

↓

Hash It

↓

Compare

↓

Match?

↓

YES → Password Found

NO → Try Again
```

---

# Why Does Hashcat Need the Hash Algorithm?

Suppose the target hash is:

```
5f4dcc3b5aa765d61d8327deb882cf99
```

Hashcat cannot automatically know whether it is:

- MD5
- NTLM
- MD4
- SHA1
- SHA256

We tell Hashcat which algorithm to use.

Example:

```
-m 0
```

means

```
Use MD5
```

Another example:

```
-m 1400
```

means

```
Use SHA256
```

---

# Attack Modes

Hashcat supports different ways of guessing passwords.

The most common attack mode is:

### Dictionary Attack

Hashcat reads passwords from a wordlist.

Example:

```
123456
password
football
admin
hello123
```

It hashes each password and compares it with the target hash.

---

Other attack modes include:

- Brute Force
- Mask Attack
- Hybrid Attack
- Rule-Based Attack

These are covered later as you progress with Hashcat.

---

# Identifying an Unknown Hash

Sometimes you only have the hash.

The workflow becomes:

```
Unknown Hash
      │
      ▼
Look for Prefix
      │
      ▼
Check Hash Length
      │
      ▼
Use hashid
      │
      ▼
Use Context
      │
      ▼
Choose Hashcat Mode
      │
      ▼
Recover Password
```

---

# Prefix Recognition

Some hashes contain prefixes that identify the algorithm.

Examples:

| Prefix | Algorithm |
|---------|-----------|
| `$2a$` | bcrypt |
| `$6$` | sha512crypt |
| `$7$` | scrypt |
| `$y$` | yescrypt |

If a prefix exists, identifying the algorithm becomes much easier.

---

# Hash Length Recognition

Sometimes there is no prefix.

Hash length can help narrow down the possibilities.

| Length | Common Algorithms |
|---------|-------------------|
| 32 | MD5, NTLM, MD4 |
| 40 | SHA1 |
| 64 | SHA256 |
| 128 | SHA512 |

Length alone is not enough, but it helps reduce the possibilities.

---

# Using hashid

`hashid` helps identify possible hash algorithms.

Example:

```bash
hashid HASH
```

Example output:

```
Possible Hashes:

MD5
NTLM
MD4
```

Remember:

`hashid` suggests possible algorithms.

It does **not** guarantee which one is correct.

---

# Using Context

Context is often the biggest clue.

Example:

Hash found inside:

```
Windows SAM
```

Probably:

```
NTLM
```

Hash found inside:

```
Website Database
```

Probably:

```
MD5
```

Understanding where the hash came from helps choose the correct algorithm.

---

# Multiple Hashes

Hashcat can attack multiple hashes at once.

Example:

```
hash1
hash2
hash3
```

Hashcat processes every hash in the file.

Example output:

```
Recovered:
2/3
```

Meaning:

- 2 passwords recovered
- 1 password still unknown

---

# CPU vs GPU

Hashcat is designed to use GPUs whenever possible.

Why?

Because password cracking is highly parallel.

```
GPU
↓

Thousands of cores

↓

Millions/Billions of guesses per second
```

Compared to:

```
CPU
↓

Few cores

↓

Much slower
```

---

# VM vs Host Machine

Hashcat works inside a Virtual Machine.

However:

```
VM

↓

Limited GPU Access

↓

Slower
```

Running Hashcat directly on the host machine provides much better performance.

---

# Where is Hashcat Used?

- Password Auditing
- Penetration Testing
- Digital Forensics
- Incident Response
- CTF Challenges
- Security Research

---

# Advantages

- Extremely fast
- GPU accelerated
- Supports thousands of hash algorithms
- Supports multiple attack modes
- Cross-platform
- Open Source

---

# Limitations

- Cannot decrypt hashes.
- Requires the correct hash algorithm.
- Strong passwords may take a very long time to recover.
- Success depends on the attack method and wordlist quality.

---

# Practical Workflow

```
Receive Hash
      │
      ▼
Identify Algorithm
(Prefix / Length / hashid / Context)
      │
      ▼
Choose Hash Mode
      │
      ▼
Select Attack Mode
      │
      ▼
Choose Wordlist
      │
      ▼
Run Hashcat
      │
      ▼
Password Found?
      │
      ├── Yes
      │      │
      │      ▼
      │ Password Recovered
      │
      └── No
             │
             ▼
Try Better Wordlist / Different Attack / Different Mode
```

---

# Where Hashcat Fits in Cybersecurity

```
Database Leak
        │
        ▼
Password Hashes
        │
        ▼
Identify Algorithm
        │
        ▼
Hashcat
        │
        ▼
Recovered Passwords
```

Hashcat is commonly used after password hashes have been obtained during a security assessment or forensic investigation.

---

# Interview Questions

### What is Hashcat?

A password recovery tool that attempts to recover passwords by comparing generated hashes with target hashes.

---

### Does Hashcat decrypt hashes?

No.

Hashcat does not decrypt hashes.

It hashes password guesses and compares them with the target hash.

---

### Why is Hash Mode required?

Hashcat must know which hashing algorithm to use when generating hashes for comparison.

---

### What does `hashid` do?

It suggests possible hashing algorithms based on the hash format.

---

### Why is GPU preferred?

Because GPUs can calculate millions or billions of hashes in parallel, making password recovery much faster.

---

### Can Hashcat recover every password?

No.

If the password is not present in the chosen attack or wordlist, Hashcat cannot recover it.

---

# Memory Hook

```
Password
     │
     ▼
Hash Function
     │
     ▼
Hash
     │
     ▼
Hashcat
     │
     ▼
Password Recovery
```

**Remember:**

> **Hashing creates the lock. Hashcat keeps trying keys until one fits.**
