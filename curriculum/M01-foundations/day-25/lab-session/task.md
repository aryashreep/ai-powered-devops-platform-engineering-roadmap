# 🧪 Lab: Signed Commits & Supply Chain Security

> **Goal:** Verify your identity in Git using signed commits and understand supply chain security.

## Target Objectives

- Generate a GPG key pair
- Configure Git to sign commits
- Sign commits and tags
- Verify signed commits from others
- Understand the security model

---

## Hands-on Challenges

### 1. GPG Key Generation
```bash
# Generate a GPG key (ED25519 preferred)
gpg --full-generate-key
# Choose: (9) ECC (sign and encrypt)
# Choose: (1) Curve 25519
# Set expiry: 2y
# Use your email that matches Git config

# List keys
gpg --list-secret-keys --keyid-format LONG
gpg --list-keys --keyid-format LONG
```

### 2. Configure Git Signing
```bash
# Get your key ID
gpg --list-secret-keys --keyid-format LONG
# Configure Git
git config --global user.signingkey <KEY-ID>
git config --global commit.gpgsign true
git config --global tag.gpgsign true

# Optionally: set SSH signing
git config --global gpg.format ssh
```

### 3. Sign a Commit
```bash
# Create a signed commit
git commit -S -m "feat: add signed commit demo"
# Verify the signature
git log --show-signature -1
# Check on GitHub: the commit will show "Verified" badge
```

### 4. Sign a Tag
```bash
# Create an annotated and signed tag
git tag -s v0.1.0 -m "Release v0.1.0"
# Verify
git tag -v v0.1.0
```

### 5. Export and Share Your Public Key
```bash
# Export your public key
gpg --armor --export <KEY-ID>
# Add to GitHub:
# Settings → SSH and GPG keys → New GPG key
# Or register SSH signing key
```

### 6. Verify Others' Commits
```bash
# Import a signer's public key
# Then verify their commits
git log --show-signature
```

---

## Proof of Work

Save `solution/signed-commits-report.md` with:
- Your GPG key fingerprint
- A signed commit example (show the verification output)
- A signed tag example
- Screenshot evidence from GitHub "Verified" badge (or describe the process)
