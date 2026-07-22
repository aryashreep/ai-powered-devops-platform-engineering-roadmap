# Day 24 — Reference Solution

## GPG Setup Commands
```bash
# Generate key
gpg --full-generate-key

# List keys with IDs
gpg --list-secret-keys --keyid-format=long
# Example output:
# sec   ed25519/ABC123DEF456 2024-01-15 [SC]
#       XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX

# Configure Git
git config --global user.signingkey ABC123DEF456
git config --global commit.gpgsign true

# Sign commit explicitly (-S flag)
git commit -S -m "feat: add feature with signature"
```

## Verification Commands
```bash
# Verify last commit
git log --show-signature -1

# Verify a tag
git tag -v v1.0.0

# Verify a specific commit
git verify-commit HEAD
```

## Why Signed Commits Matter
| Security Threat | How Signing Mitigates |
|----------------|----------------------|
| Impersonation | Only you can sign with your private key |
| Tampered commits | Signature breaks if content changes |
| Man-in-the-middle | Verified badge on GitHub/GitLab |
| Supply chain attacks | Prevents fake maintainer commits |

## SSH vs GPG Signing
| Method | Pro | Con |
|--------|-----|-----|
| GPG | Widely supported | More complex UX |
| SSH | Uses existing SSH keys | Newer, fewer tools support it |
