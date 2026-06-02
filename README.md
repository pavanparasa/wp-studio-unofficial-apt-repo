<div align="center">

# WordPress Studio (Unofficial) — Linux APT Repository

[![Automated Build](https://github.com/pavanparasa/wp-studio-unofficial-apt-repo/actions/workflows/build-and-deploy.yml/badge.svg)](https://github.com/pavanparasa/wp-studio-unofficial-apt-repo/actions/workflows/build-and-deploy.yml)
[![Platform](https://img.shields.io/badge/platform-Ubuntu%20%7C%20Debian%20%7C%20Mint-orange.svg)]()
[![License](https://img.shields.io/badge/license-GPL--2.0-blue.svg)]()

A fully automated, self-updating Debian/Ubuntu package repository for [WordPress Studio](https://developer.wordpress.com/studio/).

</div>

---

Automattic's Wordpress Studio is awesome! It uses WebAssembly (Wasm) to eliminate the need for traditional backend servers during local development. It has great Macos & Windows Apps but native Linux desktop integration usually requires downloading manual binaries & building locally & that does not update. This project automates that headache away. With one click, a GitHub Action pulls the official upstream source code, builds the Linux desktop binary, packages it into a standard `.deb` installer, and signs the release.

By adding this repository to your system, you get a native WordPress local environment with updates delivered safely alongside your normal system packages via `apt upgrade`.

> [!WARNING]
> **Disclaimer:** This repository is **100% unofficial**. It is a community-maintained packaging effort and is not affiliated with, maintained by, or endorsed by Automattic or the WordPress Foundation. The application is explicitly named `wordpress-studio-unofficial` to strictly respect corporate trademark guidelines.

---

## 🚀 Installation

You can securely add this package repository to your Debian or Ubuntu-based distribution (including Linux Mint, Zorin, Pop!_OS, Elementary OS, etc.) by running three simple terminal commands.

**1. Download and register the repository signing key:**

```bash
curl -fsSL https://github.com/pavanparasa/wp-studio-unofficial-apt-repo/releases/download/apt-repo/public.key \
  | sudo gpg --dearmor -o /usr/share/keyrings/wp-studio-unofficial-archive-keyring.gpg
```

**2. Link the signed repository to your local APT sources file:**

```bash
echo "deb [signed-by=/usr/share/keyrings/wp-studio-unofficial-archive-keyring.gpg] https://github.com/pavanparasa/wp-studio-unofficial-apt-repo/releases/download/apt-repo /" \
  | sudo tee /etc/apt/sources.list.d/wp-studio-unofficial.list > /dev/null
```

**3. Refresh your system package lists and install WordPress Studio (Unofficial):**

```bash
sudo apt update && sudo apt install wordpress-studio-unofficial
```

Once installed, WordPress Studio (Unofficial) will automatically appear with its system icon inside your desktop's application menu, ready to pin to your system dock.

---

## 🛠️ System Architecture

This repository operates on a completely transparent, zero-cost pipeline using modern cloud infrastructure:

| Stage | Description |
|-------|-------------|
| **⏰ Automation** | A manually triggered GitHub Actions workflow (`workflow_dispatch`) queries the GitHub API for the latest **stable** release tag before building, ensuring beta/development versions are never packaged. |
| **🔨 Compilation** | The pipeline checks the latest release tag on the official [Automattic/studio](https://github.com/Automattic/studio) repository, installs the targeted version of Node.js specified by the developers, and compiles the raw Linux production binaries out of the source monorepo layout. |
| **📦 Packaging** | The build invokes `electron-installer-debian` to dynamically map dependencies and bundle the native app interface. |
| **🚀 Distribution** | APT Installation & Deb Files |

---

## 🗑️ Removal Instructions

If you ever decide to remove the application and clean up the repository definitions from your operating system, execute the following commands:

```bash
sudo apt remove wordpress-studio-unofficial
sudo rm /etc/apt/sources.list.d/wp-studio-unofficial.list
sudo rm /usr/share/keyrings/wp-studio-unofficial-archive-keyring.gpg
sudo apt update
```