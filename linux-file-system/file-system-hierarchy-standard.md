
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

### `/lib`	- Core libraries
Contains **shared library files** (.so files) needed by programs in `/usr/bin` and `/usr/sbin`. Which is now **Symbolic** link to `/usr/lib` directory. 
- Its actually contains architecture dependent **32-bit** specific **library** files.

#

### `/lib64`	- Core libraries
Contains **shared library files** (.so files) needed by programs in `/usr/bin` and `/usr/sbin`. Which is now **Symbolic** link to `/usr/lib64` directory. 
- Its actually contains architecture dependent **64-bit** specific **library** files.

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
 
- **`/usr/local`** – Locally installed software
  For software installed manually **by the administrator**, not through package manager.

  **Inside:**
    * `/usr/local/bin`
    * `/usr/local/sbin`
    * `/usr/local/lib`
  
  Useful for custom apps or source-compiled tools.

#

### `/boot` – Bootloader files

Contains **static files required to boot the system**, including the Linux kernel, initial RAM filesystem, and bootloader configuration.
- Files here are **static** – they don't change during normal operation.
- **Multiple kernel** versions are kept here for **fallback booting**.

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

#

### `/etc` – Configuration files

**The central repository for system-wide configuration files** and scripts. The name originates from early Unix systems where "etc" stood for "etcetera" (miscellaneous files).

**Core Principles:**
1. **Host-specific:** Configurations are specific to *this* machine (not shared across networks)
2. **Text-based:** Primarily editable text files (no binaries)
3. **System-wide:** Affects all users (user-specific configs go in home directories)

#### **Key Categories & Examples:**

- **System Configuration:**
  - `/etc/fstab` – Filesystem mount table
  - `/etc/hostname`, `/etc/hosts` – Network naming
  - `/etc/resolv.conf` – DNS configuration
  - `/etc/localtime` – Timezone setting

- **Service Configuration:**
  - `/etc/ssh/sshd_config` – SSH server settings
  - `/etc/nginx/nginx.conf` – Web server configuration
  - `/etc/systemd/system/` – Systemd service units
  - `/etc/crontab`, `/etc/cron.d/` – Scheduled tasks

- **User & Security:**
  - `/etc/passwd`, `/etc/group` – User and group databases
  - `/etc/shadow` – Encrypted passwords (root only)
  - `/etc/sudoers` – Sudo permissions
  - `/etc/selinux/config` – SELinux policy (RHEL/Fedora)

- **Package Management:**
  - `/etc/apt/sources.list` – APT repositories (Debian/Ubuntu)
  - `/etc/yum.repos.d/` – YUM/DNF repositories (RHEL/Fedora)
  - `/etc/pacman.conf` – Pacman configuration (Arch)

#### **FHS rule:**
- No **binary** files allowed in `/etc`
- Only **text** configurations.

#### **Common Exceptions:**
- **Some binaries *are* allowed:**
  - `/etc/alternatives/` – Symbolic links managed by `update-alternatives`
  - `/etc/skel/` – Template files for new user home directories
  - `/etc/cron.hourly/`, `/etc/cron.daily/` – Script directories (though these are typically symlinks to `/etc/cron.d/`)
    
- **Some configuration isn't here:**
  - User-specific configs → `~/.config/`
  - Application data → `~/.local/share/`
  - System-wide variable data → `/var/`

### **Modern Developments:**
- **Configuration directories:** Many services use `/etc/[service]/conf.d/` for modular configs
- **Dot-directories:** Some apps use `/etc/.config/` pattern (following XDG standards)
- **Automatic config generators:** Tools like `netplan` (Ubuntu) generate configs in `/etc` from YAML files

#



#

## `/home` – User home directories

Contains the personal directories for all **regular users** (non-system users). Each user gets a subdirectory named after their **username**, which serves as their **personal workspace**.

**Example:**
- `/home/alice`
- `/home/robert`

#

## `/root` – Root user's home directory

The home directory for the **superuser (root)** account. It's kept separate from `/home` for critical security and functional reasons.

**Why `/root` is Separate from `/home`:**
1. **Early Availability:**
   - `/root` must be available during **single-user/maintenance mode**
   - `/home` might be on a separate filesystem that isn't mounted during recovery

2. **Security Isolation:**
   - Prevents regular users from browsing `/home/root` if `/home` permissions are misconfigured
   - Limits exposure of sensitive root configuration files

3. **System Integrity:**
   - Root's environment should not depend on potentially corrupted user filesystems
   - Ensures root can always log in and repair the system

#

## `/opt` – Optional software packages

The **/opt** directory is reserved for the installation of **self-contained, optional, and usually 3rd-party software packages**. 

**Purpose and Philosophy of `/opt`**

1. **Self-contained applications:**

    Software installed in `/opt` should **reside** completely within its own **subdirectory** including **binaries, libraries, configuration, and static data**. 
    ```text
    /opt/application_name/
    ```
    >This avoids **scattering** files across `/usr/bin`, `/etc`, `/var`, etc.

2. **Not managed by system package manager:**

   Programs here are often installed from
    -	tarballs `.tar.gz`
    -	proprietary `.run` installers
    -	or vendor **scripts**.
      
   >They typically don’t **integrate** into the **system's package database** like `apt`, `yum`, or `pacman` etc.

3. **Optional and additive**:  
    -	`/opt` is for software that adds functionality not required for basic system operation. 
    -	Removing the entire `/opt/programname` directory should cleanly uninstall the application.

**FHS Compliance**:  
- The **Filesystem Hierarchy Standard** (FHS) designates `/opt` for this purpose. If a package installs into `/opt`, its static files must stay inside `/opt`
  ```bash
  /opt/<package>/
  or
  /opt/<vendor>/<package>/
  ```
- But variable data (like logs or state) should go into `/var/opt/`.
  ```bash
  /var/opt/<package>/
  ```

**Common Layout**

*   **`/opt/<package>/`** or **`/opt/<provider>/<package>/`**  

    Examples:
    ```
    /opt/google/chrome/
    /opt/microsoft/teams/
    /opt/oracle/virtualbox/
    /opt/jetbrains/intellij-idea/
    /opt/sublime_text_4/
    ```
*   **`/var/opt/<package>/`** (if needed) – Variable data for packages in `/opt`.
*   **`/etc/opt/<package>/`** (if needed) – Configuration files for packages in `/opt`.

**One-Line Memory Trick**
>`/opt = Optional, third-party, self-contained software`
