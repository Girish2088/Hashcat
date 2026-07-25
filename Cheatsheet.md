# Hashcat Cheat Sheet

---

# Basic Syntax

```bash
hashcat -m <hash_mode> -a <attack_mode> <hashfile> <wordlist>
```

Example:

```bash
hashcat -m 1400 -a 0 hash.txt rockyou.txt
```

---

# Hash Modes (-m)

| Mode | Algorithm |
|------|-----------|
| `0` | MD5 |
| `1000` | NTLM |
| `1400` | SHA256 |
| `3200` | bcrypt |

---

# Attack Modes (-a)

Dictionary Attack

```bash
-a 0
```

Brute Force Attack

```bash
-a 3
```

Mask Attack

```bash
-a 3
```

Hybrid Attack

```bash
-a 6
```

---

# Common Commands

Crack MD5

```bash
hashcat -m 0 -a 0 hash.txt rockyou.txt
```

Crack NTLM

```bash
hashcat -m 1000 -a 0 hash.txt rockyou.txt
```

Crack SHA256

```bash
hashcat -m 1400 -a 0 hash.txt rockyou.txt
```

Crack bcrypt

```bash
hashcat -m 3200 -a 0 hash.txt rockyou.txt
```

---

# Show Cracked Passwords

```bash
hashcat --show -m MODE hash.txt
```

Example:

```bash
hashcat --show -m 1400 hash.txt
```

---

# Crack Multiple Hashes

```bash
hashcat -m 0 -a 0 hashes.txt rockyou.txt
```

---

# Identify Unknown Hash

```bash
hashid HASH
```

Example:

```bash
hashid b6b0d451bbf6fed658659a9e7e5598fe
```

---

# Hash Recognition

### Prefix

| Prefix | Algorithm |
|---------|-----------|
| `$2a$` | bcrypt |
| `$6$` | sha512crypt |
| `$7$` | scrypt |
| `$y$` | yescrypt |

---

### Length

| Length | Possible Algorithm |
|--------:|--------------------|
| 32 | MD5 / NTLM / MD4 |
| 40 | SHA1 |
| 64 | SHA256 |
| 128 | SHA512 |

---

# Workflow

```text
Receive Hash
      │
      ▼
Prefix?
      │
      ├── Yes → Identify Algorithm
      │
      └── No
            │
            ▼
Check Length
            │
            ▼
Run hashid
            │
            ▼
Use Context
            │
            ▼
Choose Hash Mode (-m)
            │
            ▼
Choose Attack Mode (-a)
            │
            ▼
Choose Wordlist
            │
            ▼
Run Hashcat
            │
            ▼
Password Found?
```

---

# Memory Hook

```text
Hash
 │
 ▼
Identify Algorithm
 │
 ▼
Choose Mode (-m)
 │
 ▼
Choose Attack (-a)
 │
 ▼
Choose Wordlist
 │
 ▼
Hashcat
 │
 ▼
Recovered Password
```

---

# Quick Notes

- `-m` → Hash Algorithm
- `-a` → Attack Mode
- `hashid` → Suggest possible algorithms
- `--show` → Display cracked passwords
- Hashcat **does not decrypt** hashes.
- Hashcat **hashes password guesses and compares them**.
- GPU is much faster than CPU for Hashcat.
