# 📦 **Linux Software Package Types**

## 📑 Table of Contents

- [📦 **Linux Software Package Types**](#-linux-software-package-types)
  - [📑 Table of Contents](#-table-of-contents)
  - [🔍 Definition of a Package Manager \& Repository](#-definition-of-a-package-manager--repository)
  - [🏛️ Traditional Package Formats](#️-traditional-package-formats)
    - [1. **DEB** (Debian / Ubuntu)](#1-deb-debian--ubuntu)
      - [Anatomy of a DEB Package](#anatomy-of-a-deb-package)
    - [🔧 Inspecting a DEB Package: Step-by-Step](#-inspecting-a-deb-package-step-by-step)
- [📦 **RPM Package Anatomy**](#-rpm-package-anatomy)
  - [🔍 1. Download the RPM](#-1-download-the-rpm)
  - [⚙️ 2. Inspecting Package Metadata](#️-2-inspecting-package-metadata)
    - [2.1 Query Basic Info](#21-query-basic-info)
  - [🎯 3. Listing Package Contents](#-3-listing-package-contents)
  - [🧩 4. Viewing Installation Scripts](#-4-viewing-installation-scripts)
  - [🛠️ 5. Extracting the Payload](#️-5-extracting-the-payload)
- [**Snap and Flatpak Package Types**  🚀](#snap-and-flatpak-package-types--)
  - [🔍 Definition](#-definition)
  - [🌐 Why Snaps and Flatpaks?](#-why-snaps-and-flatpaks)
  - [⚙️ Snap Packages](#️-snap-packages)
    - [🗂️ Anatomy](#️-anatomy)
    - [🔧 Common Commands](#-common-commands)
  - [📦 Flatpak Packages](#-flatpak-packages)
    - [🗂️ Anatomy](#️-anatomy-1)
    - [🔧 Common Commands](#-common-commands-1)
  - [🔄 Key Differences](#-key-differences)

---

When you install or remove applications on Linux, you work with **software packages**—archived bundles containing executables, libraries, metadata, and installation scripts. Packages are stored in **repositories**, centrally maintained collections of software for each distribution.


## 🔍 Definition of a Package Manager & Repository

* **Package Manager**: A tool (e.g., `apt`, `dnf`, `zypper`) that installs, upgrades, and removes packages by interacting with repositories.
* **Repository**: A server or local directory hosting a collection of packages and metadata, ensuring you get compatible, tested software for your distribution.

## 🏛️ Traditional Package Formats

### 1. **DEB** (Debian / Ubuntu)

* **Extension**: `.deb`
* **Used by**: Debian, Ubuntu, and their derivatives
* **Compliance**: Follows Linux Standard Base (LSB) package-format specs

#### Anatomy of a DEB Package

A `.deb` file is a standard Unix archive containing four members:

| Member          | Purpose                                                        |
| --------------- | -------------------------------------------------------------- |
| `debian-binary` | Text file indicating package format version (e.g., `2.0`)      |
| `control.tar.*` | Metadata and maintainer scripts (pre-/post-installation hooks) |
| `data.tar.*`    | The actual program files and directories to install            |
| `_gpgorigin`    | (Optional) GPG signature metadata                              |

### 🔧 Inspecting a DEB Package: Step-by-Step

1. **Install `ar` (if needed)**

   ```bash
   apt update
   apt install sudo
   apt install wget
   sudo apt install binutils
   ```
2. **Download a sample `.deb`**

   ```bash
   wget https://downloads.1password.com/linux/debian/amd64/stable/1password-latest.deb
   ```
3. **List archive members**

   ```bash
   ar t 1password-latest.deb
   ```

   ```
   debian-binary
   control.tar.xz
   data.tar.xz
   _gpgorigin
   ```
4. **Extract all members**

   ```bash
   ar x 1password-latest.deb
   ls
   # → control.tar.xz  data.tar.xz  debian-binary  _gpgorigin  1password-latest.deb
   ```
5. **Read package format version**

   ```bash
   cat debian-binary
   # → 2.0
   ```
6. **Inspect program files**

   ```bash
   # List first entries of data archive (requires xz support)
   sudo apt install xz-utils
   tar tJf data.tar.xz | head
   ```
---

# 📦 **RPM Package Anatomy**

RPM (Red Hat Package Manager) is the standard package format for Red Hat–based distributions (Fedora, CentOS, RHEL, AlmaLinux, Rocky Linux) as well as SUSE and openSUSE. Like `.deb` files, an RPM is a structured archive containing:

1. **Signature & Header**
2. **Payload** (the actual program files, scripts, and configuration)

Below, we’ll walk through each component and demonstrate how to inspect and extract an RPM package—using 1Password as our example. 🛠️✨


## 🔍 1. Download the RPM

```bash
wget https://downloads.1password.com/linux/rpm/stable/x86_64/1password-latest.rpm
```

* Downloads `1password-latest.rpm` into your current directory.

## ⚙️ 2. Inspecting Package Metadata

### 2.1 Query Basic Info

```bash
rpm -qpi 1password-latest.rpm
```

* **`-q`**: query
* **`-p`**: specify package file
* **`-i`**: show package information

**Sample Output**

```
Name        : 1password
Version     : 8.9.0
Release     : 1
Architecture: x86_64
Install Date: (not installed)
Group       : Productivity/Password Management
Size        : 12345678
License     : Proprietary
Signature   : RSA/SHA512, ...
Source RPM  : 1password-8.9.0-1.src.rpm
Build Date  : Fri 05 Jul 2025 10:00:00 AM UTC
Build Host  : builder.example.com
Summary     : 1Password password manager
```

## 🎯 3. Listing Package Contents

To see every file and its intended installation path:

```bash
rpm -qpl 1password-latest.rpm
```

* **`-l`**: list files in package

**Sample Output**

```
/opt/1Password/1Password-BrowserSupport
/opt/1Password/1Password-Crash-Handler
/opt/1Password/1Password-LastPass-Exporter
/opt/1Password/1password
/opt/1Password/LICENSE.electron.txt
/opt/1Password/LICENSES.chromium.html
/opt/1Password/after-install.sh
/opt/1Password/after-remove.sh
/opt/1Password/chrome-sandbox
```

## 🧩 4. Viewing Installation Scripts

Many RPMs include scripts that run **before**, **during**, or **after** installation:

```bash
rpm -q --scripts -p 1password-latest.rpm
```

* **`--scripts`**: show scriptlets (pre/post install/remove)

**Sample Output**

```bash
preinstall scriptlet (using /bin/sh):
#!/bin/sh
# create symlink, set permissions…

postinstall scriptlet (using /bin/sh):
#!/bin/sh
# update desktop entries…

preuninstall scriptlet:
…

postuninstall scriptlet:
…
```

## 🛠️ 5. Extracting the Payload

RPM payloads are stored as a compressed **cpio** archive. You can extract them without installing:

1. **Convert RPM to CPIO stream**

   ```bash
   rpm2cpio 1password-latest.rpm > 1password.cpio
   ```
2. **Extract with `cpio`**

   ```bash
   mkdir rpm_extract && cd rpm_extract
   cpio -idmv < ../1password.cpio
   ```

   * **`-i`**: extract
   * **`-d`**: create directories as needed
   * **`-m`**: preserve modification times
   * **`-v`**: verbose output

**Result**
All files (e.g., `/opt/1Password/...`) appear under `rpm_extract/opt/1Password`.

---

# **Snap and Flatpak Package Types**  🚀

## 🔍 Definition

**Snap** and **Flatpak** are modern Linux package formats that bundle applications and all their needed libraries into a single, isolated file. This makes it easy to install the same app on many different Linux distributions without worrying about missing dependencies. Think of them like “app containers” for your desktop or server! 📦🛡️

## 🌐 Why Snaps and Flatpaks?

* **Cross-distribution support**: One Snap or Flatpak works on Ubuntu, Fedora, Debian, openSUSE, and more—no separate `.deb` or `.rpm` files required.
* **Sandboxed**: Apps run in a controlled environment, limiting their access to your system for extra security 🔒.
* **Easy updates**: Automatic, atomic updates mean you always get the latest version or can roll back if something breaks 🔄.

## ⚙️ Snap Packages

### 🗂️ Anatomy

* **SquashFS file**: An immutable (read-only) compressed filesystem containing the app.
* **`meta/` directory**: Inside the Snap, you’ll find metadata (app name, version, permissions) and hooks (scripts that run before or after install).

### 🔧 Common Commands

1. **Install a Snap**

   ```bash
   sudo snap install vlc
   ```
2. **List installed Snaps**

   ```bash
   snap list
   ```
3. **Update all Snaps**

   ```bash
   sudo snap refresh
   ```
4. **Remove a Snap**

   ```bash
   sudo snap remove vlc
   ```
5. **View Snap details**

   ```bash
   snap info vlc
   ```

## 📦 Flatpak Packages

### 🗂️ Anatomy

* **OSTree repository**: Flatpaks use a Git-like storage (libostree) to manage versions of app files.
* **Bubblewrap sandbox**: Each Flatpak runs in its own container using user-namespace isolation.

### 🔧 Common Commands

1. **Add Flathub (main repository)**

   ```bash
   flatpak remote-add --if-not-exists flathub https://flathub.org/repo/flathub.flatpakrepo
   ```
2. **Install a Flatpak**

   ```bash
   flatpak install flathub com.spotify.Client
   ```
3. **List installed Flatpaks**

   ```bash
   flatpak list
   ```
4. **Update all Flatpaks**

   ```bash
   flatpak update
   ```
5. **Run a Flatpak app**

   ```bash
   flatpak run com.spotify.Client
   ```
6. **Uninstall a Flatpak**

   ```bash
   flatpak uninstall com.spotify.Client
   ```

## 🔄 Key Differences

| Feature        | Snap                    | Flatpak                        |
| -------------- | ----------------------- | ------------------------------ |
| Origin         | Canonical (Ubuntu)      | GNOME/Red Hat community        |
| Scope          | Desktop, server, IoT    | Desktop only                   |
| Store          | Snap Store              | Flathub (and others)           |
| Isolation tech | AppArmor (on Ubuntu)    | Bubblewrap                     |
| Base system    | Built-in SquashFS image | OSTree versioned content store |

---
