
# Linux Filesystem Hierarchy Standard (FHS)

## **What is FHS?**

The **Filesystem Hierarchy Standard (FHS)** defines **how directories and files are organized** on a Linux system.

Its purpose is to ensure **consistency** between **Linux distributions** so that:
* **Applications** know where to find **configuration** files
* **Sysadmins** know where **logs** are stored
* **Software packages** install files in **predictable places**
* **Scripts** work across different distributions


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

#

### `/` - Root Tirectory

* The top-level directory of the filesystem
* Everything starts here
* Root contains all other directories

#

### `/bin` – Essential User Binaries

This directory contains **fundamental executable programs** (binaries) that are required for the **operating** system to function, even in a **minimal rescue** environment. Historically, it was **separated** from `/usr/bin` because its contents were needed to **mount** the `/usr` filesystem. On modern systems following the **FHS (Filesystem Hierarchy Standard)**, `/bin` is often a **symbolic** link to `/usr/bin`.

**Examples:**
* `ls` – list directory contents
* `cp` – copy files and directories
* `mv` – move/rename files
* `rm` – remove files
* `cat` – concatenate and print files
* `bash` – the Bourne-Again SHell (often the default system shell)

#

### `/sbin` – Essential System Binaries

This directory contains **binaries** essential for **system administration**, **maintenance**, and **booting**. Unlike `/bin`, programs in `/sbin` are typically not needed by **regular** users and often require **root** privileges. Historically, `/sbin` was separate from `/usr/sbin` because its contents were needed early in the boot process, before `/usr` was mounted.

**Modern Context:** Following the **FHS 3.0** standard and the **"usrmerge"** transition, `/sbin` is now commonly a **symbolic link** to `/usr/sbin` on most distributions. The distinction between **"essential"** and **"non-essential"** system binaries has largely been **eliminated**.

**Examples:**
- **System Control:** `systemctl`, `service`, `telinit`
- **Filesystem Management:** `mount`, `umount`, `fsck`, `mkfs`
- **Network Administration:** `ip`, `iptables`, `ifconfig` (deprecated but still present)
- **Boot & Recovery:** `init`, `reboot`, `shutdown`, `modprobe`

#

### `/usr` – User system resources

`/usr` is the **main software** directory, containing **programs**, **libraries**, and **shared data**. Most commands you run are in `/usr/bin`, while **system administration** tools are in `/usr/sbin`. 

The historical split between **"essential"** (in /bin, /sbin) and **"non-essential"** (in /usr/bin, /usr/sbin) binaries is largely **obsolete** on modern Linux systems.

>**Note:** Think of `/usr` as the equivalent of **"Program Files"** in Windows.

#

### **Inside `/usr`: The other Directories Inside `/usr` Directories**

- **`/usr/bin` – User Binaries (Primary command location)**
  
  Contains the vast majority of **user executable commands**. On modern systems, `/bin` is typically a **symlink** here.
  
  **Examples:**
    * `vim`, `nano` – text editors
    * `git` – version control system
    * `python3`, `node` – programming language interpreters
    * `ssh`, `curl`, `wget` – network utilities
  
  
- **`/usr/sbin` – System Administration Binaries**
  
  Contains **system administration commands** that are typically run by the **root user**. On **modern** systems, `/sbin` is typically a **symlink** here.
  
  **Examples:**
    * `httpd`, `nginx` – web servers
    * `cron`, `systemctl` – system services
    * `sshd` – SSH server daemon
    * `useradd`, `groupadd` – user management
  
  
- **`/usr/lib` – Shared Libraries**
  
  Contains **shared library files** (.so files) needed by programs in `/usr/bin` and `/usr/sbin`. Architecture-dependent (contains **64-bit** or **32-bit** specific code).
  Like:
    * `lib` - Contains **32-bit** specific **library** files
    * `lib64` - Contains **64-bit** specific **library** files
  
  > **Analogous to:** DLL files in Windows' `System32` directory.
  
  
- **`/usr/share` – Architecture-Independent Shared Data**
  
  Contains **data** files used by **applications** that don't depend on the computer's processor type. This includes **documentation, graphics, fonts, and configuration** schemas that work the same way whether you're on an **Intel, AMD, ARM** (like Raspberry Pi), or other CPU.
  
  **Contains:**
    * `icons/`, `themes/` – Graphical assets
    * `fonts/` – Font files
    * `doc/`, `man/` – Documentation and manual pages
    * `applications/` – Desktop application entries (.desktop files)

