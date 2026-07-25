# Hashcat Commands

This file contains the most commonly used Hashcat commands for identifying hashes, selecting attack modes, cracking passwords, and viewing recovered passwords.

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

The `-m` option tells Hashcat which hashing algorithm to use.

### MD5

```bash
hashcat -m 0 -a 0 hash.txt rockyou.txt
```

---

### NTLM

```bash
hashcat -m 1000 -a 0 hash.txt rockyou.txt
```

---

### SHA256

```bash
hashcat -m 1400 -a 0 hash.txt rockyou.txt
```

---

### bcrypt

```bash
hashcat -m 3200 -a 0 hash.txt rockyou.txt
```

---

# Attack Modes (-a)

## Dictionary Attack

Tries every password from the given wordlist.

```bash
hashcat -a 0
```

Example:

```bash
hashcat -m 1400 -a 0 hash.txt rockyou.txt
```

---

## Brute Force Attack

Generates every possible password combination.

```bash
hashcat -a 3
```

Example:

```bash
hashcat -m 1400 -a 3 hash.txt ?a?a?a?a?a?a
```

---

## Mask Attack

Attempts passwords matching a specific pattern.

Example:

```bash
hashcat -m 1400 -a 3 hash.txt Password?d?d
```

---

## Hybrid Attack

Combines a wordlist with generated characters.

Example:

```bash
hashcat -m 1400 -a 6 hash.txt rockyou.txt ?d?d
```

---

# Showing Cracked Passwords

Instead of cracking again, display previously recovered passwords.

```bash
hashcat --show -m MODE hash.txt
```

Example:

```bash
hashcat --show -m 1400 hash.txt
```

---

# Multiple Hashes

Hashcat can crack multiple hashes stored in one file.

```bash
hashcat -m 0 -a 0 hashes.txt rockyou.txt
```

---

# Identifying Unknown Hashes

Use `hashid` to identify possible hash algorithms.

```bash
hashid HASH
```

Example:

```bash
hashid b6b0d451bbf6fed658659a9e7e5598fe
```

---

# Common Commands

## Crack an MD5 Hash

```bash
hashcat -m 0 -a 0 hash.txt rockyou.txt
```

---

## Crack an NTLM Hash

```bash
hashcat -m 1000 -a 0 hash.txt rockyou.txt
```

---

## Crack a SHA256 Hash

```bash
hashcat -m 1400 -a 0 hash.txt rockyou.txt
```

---

## Crack a bcrypt Hash

```bash
hashcat -m 3200 -a 0 hash.txt rockyou.txt
```

---

## Show Recovered Passwords

```bash
hashcat --show -m 1400 hash.txt
```

---

## Identify Unknown Hash

```bash
hashid HASH
```

---

# Understanding a Command

Example:

```bash
hashcat -m 1400 -a 0 hash.txt rockyou.txt
```

| Option | Purpose |
|---------|---------|
| `-m 1400` | Use SHA256 algorithm |
| `-a 0` | Dictionary Attack |
| `hash.txt` | File containing target hash |
| `rockyou.txt` | Wordlist containing password guesses |

---

# Common Hash Modes

| Mode | Algorithm |
|------|-----------|
| `0` | MD5 |
| `1000` | NTLM |
| `1400` | SHA256 |
| `3200` | bcrypt |

---

# Useful Notes

- `-m` specifies the hashing algorithm.
- `-a` specifies the attack mode.
- `hashid` helps identify possible hash algorithms.
- `--show` displays previously cracked passwords.
- Hashcat compares generated hashes with the target hash.
- Hashcat does **not** decrypt hashes.

---

# Typical Workflow

```text
Receive Hash
      │
      ▼
Identify Algorithm
(hashid / Prefix / Length)
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
Show Recovered Passwords
```
