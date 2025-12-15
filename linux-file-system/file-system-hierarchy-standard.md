
# Linux Filesystem Hierarchy Standard (FHS)

## **What is FHS?**

The **Filesystem Hierarchy Standard (FHS)** defines **how directories and files are organized** on a Linux system.

Its purpose is to ensure **consistency** between **Linux distributions** so that:
* **Applications** know where to find **configuration** files
* **Sysadmins** know where **logs** are stored
* **Software packages** install files in **predictable places**
* **Scripts** work across different distributions

---

## Top-Level Directory Structure

Below is the complete standard set of Linux directories and what each one is used for.

**Summary Table**

| Directory  | Purpose                       |
| ---------- | ----------------------------- |
| /          | Root filesystem               |
| /bin       | Essential binaries            |
| /sbin      | Essential system binaries     |
| /usr       | User applications             |
| /usr/local | Locally installed apps        |
| /etc       | Configuration files           |
| /var       | Variable data                 |
| /tmp       | Temporary files               |
| /home      | User directories              |
| /root      | Root user home                |
| /opt       | Optional third-party software |
| /lib       | Core libraries                |
| /dev       | Device files                  |
| /proc      | Kernel info                   |
| /sys       | Kernel device tree            |
| /run       | Runtime PID/socket files      |
| /boot      | Bootloader + kernel           |
| /mnt       | Manual mount point            |
| /media     | Auto-mounted devices          |
| /srv       | Service-related data          |

---

### `/` - Root Tirectory

The **root directory** is the absolute starting point of the **entire Linux filesystem hierarchy**. Every single file, directory, device, and mount point originates from here. It's represented by a **single forward slash** (`/`).

* The top-level directory of the filesystem
* Everything starts here
* Root contains all other directories

**Subdirectories of `/root'**

  ```
  /                 # The root directory itself
  ├── bin/          # Essential user binaries (ls, cp, bash)
  ├── sbin/         # Essential system binaries (fdisk, fsck)
  ├── etc/          # System-wide configuration files
  ├── usr/          # User programs and data (secondary hierarchy)
  ├── var/          # Variable data (logs, spools, caches)
  ├── home/         # User home directories
  ├── root/         # Home directory for the root user
  ├── lib/          # Essential shared libraries and kernel modules
  ├── opt/          # Optional third-party software
  ├── mnt/          # Temporary mount point for filesystems
  ```

---

### `/bin` – Essential User Binaries

This directory contains **fundamental executable programs** (binaries) that are required for the **operating** system to function, even in a **minimal rescue** environment. Historically, it was **separated** from `/usr/bin` because its contents were needed to **mount** the `/usr` filesystem. On modern systems following the **FHS (Filesystem Hierarchy Standard)**, `/bin` is often a **symbolic** link to `/usr/bin`.

**Examples:**
* `ls` – list directory contents
* `cp` – copy files and directories
* `mv` – move/rename files
* `rm` – remove files
* `cat` – concatenate and print files
* `bash` – the Bourne-Again SHell (often the default system shell)

---

### `/sbin` – Essential System Binaries

This directory contains **binaries** essential for **system administration**, **maintenance**, and **booting**. Unlike `/bin`, programs in `/sbin` are typically not needed by **regular** users and often require **root** privileges. Historically, `/sbin` was separate from `/usr/sbin` because its contents were needed early in the boot process, before `/usr` was mounted.

**Modern Context:** Following the **FHS 3.0** standard and the **"usrmerge"** transition, `/sbin` is now commonly a **symbolic link** to `/usr/sbin` on most distributions. The distinction between **"essential"** and **"non-essential"** system binaries has largely been **eliminated**.

**Examples:**
- **System Control:** `systemctl`, `service`, `telinit`
- **Filesystem Management:** `mount`, `umount`, `fsck`, `mkfs`
- **Network Administration:** `ip`, `iptables`, `ifconfig` (deprecated but still present)
- **Boot & Recovery:** `init`, `reboot`, `shutdown`, `modprobe`

---

### `/lib`	- Core libraries
Contains **shared library files** (.so files) needed by programs in `/usr/bin` and `/usr/sbin`. Which is now **Symbolic** link to `/usr/lib` directory. 
- It contains **architecture-dependent 32-bit libraries**

**Example Libraries:**
- `ld-linux*.so.*` – Program interpreter
- `libc.so.*` – Standard C library (most critical)
- `libm.so.*` – Mathematical functions
- `libpthread.so.*` – Thread support

### Standard Directory Structure of `/lib`:
```
/lib/
├── ld-linux.so.2           # Dynamic linker (essential)
├── libc.so.6              # GNU C Library (glibc)
├── libm.so.6              # Math library
├── libpthread.so.0        # POSIX threads
├── modules/               # Kernel modules
│   └── $(uname -r)/
└── firmware/              # Device firmware
```

---

### `/lib64` – Core 64-bit libraries

Contains **shared library files** (`.so` files) required by **64-bit binaries** located in `/usr/bin` and `/usr/sbin`.
On **modern Linux systems (RHEL / CentOS / Rocky / Alma / Fedora)**, `/lib64` is typically a **symbolic link to `/usr/lib64`**, following the **/usr merge** design.

* It contains **architecture-dependent 64-bit libraries**
* Essential for **system boot**, **init**, and **core command execution**

**Example Libraries**

* `ld-linux-x86-64.so.2` – 64-bit dynamic program loader
* `libc.so.6` – GNU C Library (64-bit)
* `libm.so.6` – Mathematical functions
* `libpthread.so.0` – POSIX thread support
* `librt.so.1` – Real-time extensions


**Standard Directory Structure of `/lib64`**

```
/lib64/
├── ld-linux-x86-64.so.2    # 64-bit dynamic linker (essential)
├── libc.so.6              # GNU C Library (glibc)
├── libm.so.6              # Math library
├── libpthread.so.0        # POSIX threads
├── librt.so.1             # Real-time library
├── modules/               # Kernel modules (64-bit)
│   └── $(uname -r)/
└── firmware/              # Device firmware
```

**Note:**
- On **pure 64-bit systems**, `/lib` may exist only for **compatibility**, while `/lib64` is actively **used**.
- **If a binary is 64-bit, its core libraries must exist in `/lib64`.**

---

### `/usr` – User system resources

`/usr` is the **main software** directory, containing **programs**, **libraries**, and **shared data**. Most commands you run are in `/usr/bin`, while **system administration** tools are in `/usr/sbin`. 

The historical split between **"essential"** (in /bin, /sbin) and **"non-essential"** (in /usr/bin, /usr/sbin) binaries is largely **obsolete** on modern Linux systems.

>**Note:** Think of `/usr` as the equivalent of **"Program Files"** in Windows.

**Inside `/usr`**

Here are the core `/usr` subdirectories diagram:
  ```
  /usr/
  ├── bin/
  ├── sbin/
  ├── lib/
  ├── lib64/
  ├── libexec/
  ├── include/
  ├── share/
  ├── local/
  ├── src/
  └── games/
  ```

---

### `/boot` – Bootloader files

Contains **static files required to boot the system**, including the Linux kernel, initial RAM filesystem, and bootloader configuration.

**Key Contents:**
- **Kernel images:** `vmlinuz-*` (compressed kernel executable)
- **Initial RAM filesystem:** `initramfs-*.img` or `initrd-*.img` (temporary root filesystem loaded into memory)
- **Bootloader files:** GRUB configuration (`grub.cfg`), modules, and themes
- **EFI system partition mount point:** `/boot/efi/` (on UEFI systems)

**Directory Structure:**
```
/boot/
├── vmlinuz-6.1.0-amd64          # Linux kernel
├── initramfs-6.1.0-amd64.img    # Initial RAM filesystem
├── grub/                        # GRUB bootloader
│   ├── grub.cfg                 # Boot menu configuration
│   └── fonts/                   # Boot menu fonts
└── efi/                         # EFI partition (UEFI systems)
    └── EFI/ubuntu/grubx64.efi   # UEFI bootloader
```

---

### `/etc` – Configuration files

**The central repository for host-specific, system-wide configuration files.**
The name originates from early UNIX where **“etc” meant *et cetera***, but in modern Linux it strictly means **configuration only**.

  * Contains **no program binaries**
  * Mostly **human-readable text files**
  * Defines **how the system behaves**, not the data it uses
  * Changes here affect the **entire system**

**Common Configuration Examples**

* `/etc/hostname` – System hostname
* `/etc/hosts` – Local hostname resolution
* `/etc/localtime` – Timezone configuration
* `/etc/fstab` – Filesystem mount rules
* `/etc/ssh/sshd_config` – SSH server configuration
* `/etc/passwd`, `/etc/group` – User and group databases
* `/etc/shadow` – Encrypted user passwords (root only)
* `/etc/sudoers` – Sudo privilege rules


**Standard Directory Structure of `/etc`**

```
/etc/
├── passwd                  # User account information
├── shadow                  # Encrypted passwords
├── sudoers                 # Sudo permissions
├── hostname                # System hostname
├── hosts                   # Static DNS entries
├── ssh/
│   └── sshd_config         # SSH daemon configuration
├── systemd/
│   ├── system/             # System service unit 
├── cron.d/                 # System cron jobs
├── profile                 # System-wide shell environment
├── profile.d/              # Shell environment scripts
├── pam.d/                  # PAM authentication configs
└── sysconfig/              # Service-specific settings (RHEL-based)
```

**Note:**
- **`/etc` controls system behavior through configuration, not execution.**
- Files here are often **overridden by `/etc`**, even if defaults exist in `/usr/lib`

---

## `/var` – Variable data

The `/var` directory contains **variable data**—files whose **size, content, or existence changes frequently** during normal system operation.

If data **grows, shrinks, gets updated, queued**, or **rewritten repeatedly**, it belongs in `/var`.

* Data in `/var` is **dynamic**, not static.
* Often placed on a **separate filesystem** to prevent root (`/`) from filling up.

**Examples:**

  * `/var/log/secure` – Authentication and security events (SSH, sudo)
  * `/var/log/messages` – General system messages (RHEL/CentOS)
  * `/var/spool/mail/` – User mailboxes
  * `/var/lib/mysql/` – MySQL/MariaDB database files
  * `/var/lib/docker/` – Docker images, containers, volumes
  * `/var/cache/man/` – Cached man pages

**Important Subdirectories**

```
/var/
├── cache/
├── lib/
├── local/
├── lock/
├── log/
├── mail/
├── opt/
├── run/
├── spool/
├── tmp/
└── www/
```
>Logs, queues, databases, cache — all **activity results** go to `/var`.

---

## `/home` – User home directories

Contains the personal directories for all **regular users** (non-system users). Each user gets a subdirectory named after their **username**, which serves as their **personal workspace**.

**Example:**
- `/home/alice`
- `/home/robert`

---

## `/root` – Root user’s home directory

The **home directory of the superuser (`root`)**.
It is intentionally **separate from `/home`** for **security, reliability, and system recovery** purposes.

* Acts as the **personal working directory** for the `root` user.
* Used **only** by the `root` account.
* Located at `/root` (not `/home/root`).
* Exists even on **minimal or rescue systems**.

### **Common contents**
* Root’s shell configuration files:
  * `.bashrc`
  * `.bash_profile`
* Administrative scripts and notes
* Temporary files used during system maintenance

---

## `/opt` – Optional software packages

The **/opt** directory is reserved for the installation of **self-contained, optional, and usually 3rd-party software packages** that does not come with the operating system by default. 

**Puropse of `/opt`**

- Keeps **third-party** software separate from **system files**
- Prevents files from being **scattered** across `/usr`, `/etc`, `/var`
- Makes software easy to **install** and **remove**
- Follows the Linux **Filesystem Hierarchy Standard (FHS)**

**Examples:**

  ```
  /opt/google/chrome/
  /opt/microsoft/teams/
  /opt/oracle/virtualbox/
  /opt/jetbrains/intellij-idea/
  /opt/sublime_text_4/
  ```

**One-Line Memory Trick**
>`/opt = Optional, third-party, self-contained software`

---

### `/mnt` – Temporary mount point

The **/mnt** directory is a **standard temporary mount point** used by system administrators to **manually mount filesystems** for short-term or maintenance purposes.

In modern Linux systems, `/mnt` is kept **clean and simple** and is **not intended for permanent or automatic mounts** (those usually go under `/media` or custom directories).


**Purpose of `/mnt`**

* Used for **temporary/manual mounts**
* Commonly used in **troubleshooting, recovery, and maintenance**
* Keeps temporary mounts **separate** from permanent system directories
* Traditionally used by **root/admin only**


**Example:**

```bash
/mnt/backup/
/mnt/test_disk/
/mnt/iso/
```

**Standard Directory Structure of `/mnt`**

```
/mnt/
├── disk/                  # Temporary disk mount
├── usb/                   # Manually mounted USB device
├── backup/                # Temporary backup mount
├── iso/                   # Mounted ISO image
└── rescue/                # Rescue or recovery filesystem
```

**Note:**
- These subdirectories are **not created by default** — they are created **as needed** by the administrator.

---
