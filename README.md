# sigil

A CLI tool that signs and verifies files using SHA-256 and RSA, providing cryptographic evidence of non-repudiation and file integrity.

Final project for MAC 108: Introduction to Python.

## Overview

sigil is built to handle large inputs, such as thousands of pages of discovery for a lawsuit.

- **Sign:** hashes a file with SHA-256, then signs the digest with the signer's RSA private key.
- **Verify:** uses the sender's public key to check the signature against a fresh SHA-256 hash of the plaintext file.

## Usage

```
sigil -s pathname private_key
sigil -v plaintext_pathname signed_digest sender_public_key
```

## Libraries

**Non-standard**

- **PyCryptodome:** SHA-256 hashing (`Crypto.Hash`), RSA keys (`Crypto.PublicKey`), and signing and padding (`Crypto.Signature`)
- **tqdm:** progress bar while hashing large files in chunks
- **rich:** formatted CLI output, such as the verify result and tables
- **pyfiglet:** ASCII art banner for the program name

**Standard**

- **argparse:** `-s` and `-v` modes and their arguments

File handling uses `with open` throughout.

## Classes and Files

| File | Contents |
|---|---|
| `sigil.py` | Entry point: argparse setup, pyfiglet banner, and dispatch to sign or verify |
| `signing.py` | Signed-file class: holds the path, digest, and signature; hashes the file in chunks with tqdm, signs the digest, and saves the signature |
| `verifying.py` | Verify class: holds the plaintext path, signature, and public key; recomputes the hash, checks the signature, and stores the result |
| `keymanager.py` | Public key manager class: a lookup of PEM public keys used in verify mode |

## Design Notes

Both classes expose their data (digest, signature, result) so the rich output reads from the same objects that did the signing and verifying.
