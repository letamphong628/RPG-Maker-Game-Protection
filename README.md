# 🛡️ RPG Maker Asset Security & Pipeline Suite

A robust asset encryption and build pipeline designed for **RPG Maker (MZ, MV, VX Ace, XP, VX)** developers. Safeguard proprietary game logic, graphics, audio, and database assets against unauthorized extraction, reverse engineering, and tampering.

---

## 🏗️ Architecture Overview

The suite provides a dual-tool workflow tailored for game production environments:
1. **Packaging & Obfuscation Engine (`RPGMakerEncryptor.exe`)**: Encrypts raw game directories into compact, protected binary blobs (`.0` containers) and embeds an in-memory runtime bridge for the game engine.
2. **Authorized Asset Recovery Utility (`RPGMakerDecrypter.exe`)**: An internal pipeline tool allowing authorized project owners to extract, audit, or rebuild project sources using their private cryptographic keys.

```
                    ┌─────────────────────────┐
                    │    Raw Project Source   │
                    │ (data/, img/, js/, ...) │
                    └────────────┬────────────┘
                                 │
                   [ RPGMakerEncryptor.exe ]
                  (AES-256 / PBKDF2 / Keygen)
                                 │
         ┌───────────────────────┴───────────────────────┐
         ▼                                               ▼
┌─────────────────────────┐             ┌─────────────────────────┐
│     Production Build    │             │   Private Security Key  │
│   - Obfuscated (.0)     │             │  (Keep Secret / Vault)  │
│   - Virtual Runtime     │             └────────────┬────────────┘
└─────────────────────────┘                          │
         │                                           │
         │ (Game Launch:                             │ (Internal Pipeline /
         │  Decrypted in-memory                      │  Recovery / Auditing)
         │  without disk dump)                       ▼
                                        [ RPGMakerDecrypter.exe ]
                                                     │
                                                     ▼
                                        ┌─────────────────────────┐
                                        │   Restored Work Source  │
                                        └─────────────────────────┘
```

---

## 📦 Binaries

Location: `bin/`
- **`RPGMakerEncryptor.exe`**: Deployment & encryption interface.
- **`RPGMakerDecrypter.exe`**: Authorized recovery & pipeline restoration utility.

---

## ✨ Features & Capabilities

### 1. Production Encryption Tool (`RPGMakerEncryptor.exe`)
- **Automated Directory Verification**:
  - 🟢 **Verified**: Automatically detects valid RPG Maker structures (JSON databases, plugins, asset layouts).
  - 🔴 **Unverified**: Flags missing or abnormal directory trees to prevent incomplete or corrupt builds.
- **Cryptographic Key Management**:
  - Secure pseudo-random key generation (`RPG-XXXX-XXXX-XXXX-XXXX`).
  - Automatic timestamped key file export (`RPG_Maker_Key_[Timestamp].txt`) for developer archives and secrets management.
  - One-click key regeneration and clipboard copy.
- **Configurable Security Profiles (Advanced Settings)**:
  - **Easy (Low Overhead)**: Fast XOR Stream Obfuscation for lightweight prototype distribution.
  - **Medium**: Industry-standard AES-128 bit block encryption.
  - **Advanced (Production Recommended)**: Military-grade AES-256 bit + PBKDF2 key derivation.
- **Production Asset Hardening**:
  - Packs core directories into protected payloads (`data.0`, `img.0`, `audio.0`, `js.0`).
  - Integrates an in-memory Virtual Runtime Loader (`rpg_protect_runtime.js`) to stream encrypted assets directly into the engine heap without writing unencrypted assets to disk.
  - Automated pre-build backup routines (`_RPG_Original_Backup_...`).

### 2. Internal Recovery Utility (`RPGMakerDecrypter.exe`)
- **Pipeline Integrity Verification**: Validates encrypted container headers prior to processing.
- **Private Key Authentication**: Requires the exact symmetric key generated during the build step.
- **Lossless Project Restoration**:
  - Reconstructs complete directory structures (`data/`, `img/`, `audio/`, `js/`) with byte-for-byte fidelity.
  - Unhooks runtime injection hooks from `index.html` to restore clean development environments.

---

## 🚀 Workflow Guide

### Production Build (Encryption)
1. Launch `RPGMakerEncryptor.exe`.
2. Click **Select Folder Game** and select your project root.
3. Confirm the status displays 🟢 **Verified**.
4. Configure target cipher settings in **Advanced Settings** if needed.
5. Click **BUILD SECURE ASSETS**.
6. Securely store the output key (`RPG_Maker_Key_[Timestamp].txt`) in your private credentials vault or password manager. **Do not distribute this key with public game releases.**

### Source Recovery & Internal Auditing
1. Launch `RPGMakerDecrypter.exe`.
2. Select the packaged game directory (containing `.0` containers).
3. Specify the destination directory (defaults to `[GameTitle]_DecryptedSource`).
4. Enter the matching project recovery key.
5. Click **RESTORE PROJECT ASSETS**.

---

## 🔒 Security Best Practices for Developers

- **Key Segregation**: The decryption utility is strictly for project maintainers. Never ship `RPGMakerDecrypter.exe` or exported key files inside public release packages, game installers, or public repositories.
- **Version Control**: Add `*.txt` key files and `_RPG_Original_Backup_*/` to your `.gitignore` to prevent accidental credential leakage.
- **Code Hardening**: For maximum protection on commercial releases, combine this suite with JavaScript obfuscation (e.g., Javascript-Obfuscator) on your custom plugin scripts prior to packaging.
