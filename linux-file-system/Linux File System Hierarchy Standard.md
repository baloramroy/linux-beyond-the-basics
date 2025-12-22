
# Linux Filesystem Hierarchy Standard (FHS)

### **What is FHS?**

The **Filesystem Hierarchy Standard (FHS)** defines **how directories and files are organized** on a Linux system.

Its purpose is to ensure **consistency** between **Linux distributions** so that:
* **Applications** know where to find **configuration** files
* **Sysadmins** know where **logs** are stored
* **Software packages** install files in **predictable places**
* **Scripts** work across different distributions

---

### Top-Level Directory Structure

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

### `/` - Root Directory

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

**Modern Context:** \
Following the **FHS 3.0** standard and the **"usrmerge"** transition, `/sbin` is now commonly a **symbolic link** to `/usr/sbin` on most distributions. The distinction between **"essential"** and **"non-essential"** system binaries has largely been **eliminated**.

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

**Standard Directory Structure of `/lib`:**
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

**Important Subdirectory of `/usr`**

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

The **/boot** directory contains **static files required to start (boot) the Linux operating system**.
Without the files inside `/boot`, the system **cannot load the kernel** and **will not start**.

>Think of `/boot` as the place where **Linux learns how to start itself**.

Suppose you power on a Linux server, right?

* The system firmware (**BIOS/UEFI**) needs to know **which kernel to load**
* It needs instructions on **where the root filesystem is**
* It needs temporary drivers to access disks before `/` is mounted

All this critical information comes from **/boot**.

**Example:**
- **Kernel images:** `vmlinuz-*` (compressed kernel executable)
- **Initial RAM filesystem:** `initramfs-*.img` or `initrd-*.img` (temporary root filesystem loaded into memory)
- **Bootloader files:** GRUB configuration (`grub.cfg`), modules, and themes
- **EFI system partition mount point:** `/boot/efi/` (on UEFI systems)

**Standard Directory Structure:**
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

### `/var` – Variable data

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

**Important Subdirectories of `/var`**

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

### `/home` – User home directories

Contains the personal directories for all **regular users** (non-system users). Each user gets a subdirectory named after their **username**, which serves as their **personal workspace**.

**Example:**
- `/home/alice`
- `/home/robert`

---

### `/root` – Root user’s home directory

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

### `/opt` – Optional application software

The **/opt** directory is reserved for **optional, add-on, and third-party application software** that is **not part of the default operating system**.

Software installed under `/opt` is usually **self-contained**, keeping its **binaries, libraries, and configuration files** together and **separate from system-managed directories** like `/usr` and `/etc`.

**Purpose of `/opt`**

* Stores **third-party or vendor-supplied software**
* Keeps optional software **isolated** from system files
* Prevents files from being **scattered** across the filesystem
* Simplifies **installation, upgrade, and removal**
* Follows **Filesystem Hierarchy Standard (FHS)** guidelines

**Examples:**

* `/opt/google/` – Google Chrome installation
* `/opt/oracle/` – Oracle database software
* `/opt/vmware/` – VMware products
* `/opt/zabbix/` – Zabbix server or agent
* `/opt/jetbrains/` – JetBrains IDEs
* `/opt/custom_app/` – In-house application

**Standard Directory Structure of `/opt`**

```
/opt/
├── google/                # Google Chrome
├── oracle/                # Oracle software
├── vmware/                # VMware products
├── zabbix/                # Zabbix components
├── jetbrains/             # JetBrains tools
└── custom_app/            # Custom or in-house application
```

**Notes:**

* Subdirectories are **created by the software installer**.
* Each application usually has its **own directory**.
* Configuration files may still reside in `/etc`.
* Logs and variable data may be stored in `/var`.

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

### `/media` – Removable media mount point

The **/media** directory is a **standard mount point for removable media** that are **automatically mounted** by the system when inserted or connected.

**Purpose of `/media`**

* Mount point for **USB drives, CDs, DVDs**
* Used for **automatic mounting** of removable devices
* Designed for **plug-and-play** media access
* Keeps removable media **separate** from system and temporary mounts


**Examples:**

* `/media/user/USB_DRIVE/` – USB flash drive
* `/media/user/ExternalHDD/` – External hard disk
* `/media/user/DVD/` – DVD or CD-ROM
* `/media/user/SD_CARD/` – SD card
* `/media/user/Camera/` – Camera or phone storage (MTP)

**Standard Directory Structure of `/media`**

```
/media/
├── user/
│   ├── USB_DRIVE/          # Auto-mounted USB device
│   ├── ExternalHDD/        # External hard disk
│   ├── DVD/                # Optical media
│   ├── SD_CARD/            # SD card
│   └── Camera/             # Camera or phone storage
```

**Notes:**

* Subdirectories are **created automatically** when a device is inserted.
* Directory names are usually based on the **filesystem label**.
* Mounts are typically **removed automatically** when the device is safely ejected.

---

### `/dev` – Device files

The **/dev** directory contains **special device files** that represent **hardware devices and virtual devices** in the system.

In Linux, **everything is treated as a file**, and `/dev` is the interface through which the **kernel communicates with hardware** and exposes devices to user space.
Modern systems use **udev** to **dynamically create and remove** device files as hardware appears or disappears.

>**udev** is Linux's **dynamic device manager** that runs in userspace (hence "userspace /dev") and **manages device nodes** in the `/dev` directory

**Purpose of `/dev`**

* Provides **access to hardware devices** through files
* Allows programs to **read from / write to devices** like disks, terminals, USB, etc.
* Device nodes are **created dynamically** by the kernel/udev
* Essential for **booting, mounting filesystems, and system operation**
* Used by both **system services and user applications**

**Examples:**

* `/dev/sda` – First SCSI/SATA disk
* `/dev/sda1` – First partition on `/dev/sda`
* `/dev/nvme0n1` – NVMe storage device
* `/dev/tty` – Current terminal
* `/dev/ttyS0` – Serial port
* `/dev/loop0` – Loopback block device
* `/dev/sr0` – CD/DVD drive

**Standard Directory Structure of `/dev`**

```
/dev/
├── block/                 # Block device links (disks, partitions)
├── char/                  # Character device links
├── disk/                  # Disk-related symbolic links
│   ├── by-id/             # Devices by unique hardware ID
│   ├── by-label/          # Devices by filesystem label
│   ├── by-uuid/           # Devices by filesystem UUID
│   └── by-path/           # Devices by physical connection path
├── input/                 # Keyboard, mouse, input devices
├── mapper/                # LVM and device-mapper devices
├── pts/                   # Pseudo-terminals (SSH, terminal emulators)
├── shm/                   # Shared memory (tmpfs)
├── net/                   # Network device interfaces
├── snd/                   # Sound devices
├── usb/                   # USB device nodes
├── fd/                    # File descriptor links (to /proc/self/fd)
├── stdin                  # Standard input
├── stdout                 # Standard output
└── stderr                 # Standard error
```

**Notes:**

* Files in `/dev` are **not regular files**; they are **device nodes**.
* Deleting files in `/dev` is **unsafe** and can break the system.
* `/dev` is usually mounted as **tmpfs** and exists **only in memory**.


---

### `/proc` – Process and kernel information (virtual filesystem)

The **/proc** directory is a **virtual (pseudo) filesystem** that provides **real-time information** about **running processes, kernel parameters, and system state**.

It does **not exist on disk**. Files in `/proc` are **generated dynamically by the kernel** and reflect the **current state of the system**.

**Purpose of `/proc`**

* Provides **live process information**
* Exposes **kernel parameters and system statistics**
* Used by system tools (`ps`, `top`, `free`, `uptime`)
* Allows **runtime kernel tuning** via `sysctl`
* Essential for **system monitoring and troubleshooting**

**Examples:**

* `/proc/cpuinfo` – CPU details
* `/proc/meminfo` – Memory usage
* `/proc/loadavg` – System load
* `/proc/uptime` – System uptime
* `/proc/version` – Kernel version
* `/proc/1/` – Information about PID 1 (systemd)
* `/proc/[pid]/status` – Process state and resources
* `/proc/net/tcp` – TCP connections
* `/proc/sys/net/ipv4/ip_forward` – Kernel network parameter

**Standard Directory Structure of `/proc`**

```
/proc/
├── [pid]/                 # One directory per running process
│   ├── cmdline             # Process command
│   ├── environ             # Environment variables
│   ├── fd/                 # File descriptors
│   ├── maps                # Memory mappings
│   ├── status              # Process status
│   └── stat                # Process statistics
├── cpuinfo                 # CPU information
├── meminfo                 # Memory information
├── loadavg                 # Load average
├── uptime                  # System uptime
├── mounts                  # Mounted filesystems
├── filesystems             # Supported filesystems
├── interrupts              # Interrupt statistics
├── swaps                    # Swap usage
├── net/                    # Network stack information
├── sys/                    # Kernel parameters (sysctl)
└── self/                   # Symlink to current process
```

**Notes:**

* Files are **read-only or writable** depending on kernel support.
* Writing to some files **changes kernel behavior immediately**.
* Changes made directly may **not persist after reboot**.


---

### `/sys` – Kernel device and driver interface (sysfs)

The **/sys** directory is a **virtual filesystem (sysfs)** that exposes the **kernel’s device model**, including **hardware devices, drivers, buses, and power states**.

It provides a **structured and hierarchical view** of hardware and is primarily used for **device management and kernel interaction**.

**Purpose of `/sys`**

* Exposes **hardware and driver information**
* Allows **fine-grained control** of devices
* Used by **udev** for dynamic device management
* Supports **runtime device configuration**

**Examples:**

* `/sys/class/net/eth0/` – Network interface details
* `/sys/class/block/sda/` – Block device information
* `/sys/devices/` – Physical device tree
* `/sys/bus/usb/devices/` – USB device information
* `/sys/power/state` – Power management states
* `/sys/class/thermal/` – Temperature sensors
* `/sys/class/backlight/` – Screen brightness control

**Standard Directory Structure of `/sys`**

```
/sys/
├── block/                 # Block devices
├── bus/                   # Device buses (PCI, USB, etc.)
├── class/                 # Device classes (net, block, sound)
├── devices/               # Physical device hierarchy
├── firmware/              # Firmware interfaces
├── fs/                    # Filesystem-related objects
├── kernel/                # Kernel objects and settings
├── module/                # Loaded kernel modules
└── power/                 # Power management
```

**Notes:**

* Files are **generated dynamically by the kernel**.
* Most files are **read-only**, some are **writable**.
* `/sys` is mounted as **sysfs** and exists **only in memory**.
* Intended mainly for **system utilities and administrators**, not casual users.

---

### `/run` – Runtime data (volatile)

The **/run** directory contains **runtime data** for the system and services that is **required while the system is running**.

It is a **temporary filesystem (tmpfs)**, created **early during boot**, and its contents exist **only in memory**. All data in `/run` is **lost on reboot**.
`/run` replaces older directories like `/var/run` and `/var/lock` (now symbolic links).

**Purpose of `/run`**

* Stores **runtime state information** for services and daemons
* Holds **PID files**, **sockets**, and **lock files**
* Required for **inter-process communication (IPC)**
* Available **very early in the boot process**
* Ensures runtime data is **clean at every boot**

**Examples:**

* `/run/sshd.pid` – SSH daemon PID file
* `/run/systemd/` – systemd runtime data
* `/run/log/journal/` – systemd journal runtime logs
* `/run/NetworkManager/` – NetworkManager state files
* `/run/docker/` – Docker runtime sockets and state
* `/run/dbus/` – D-Bus runtime communication sockets
* `/run/lock/` – Lock files (symlink from `/var/lock`)

**Standard Directory Structure of `/run`**

```
/run/
├── systemd/               # systemd runtime state
├── log/                   # Runtime logs (journal)
│   └── journal/
├── lock/                  # Lock files (symlink target)
├── user/
│   └── 1000/              # Per-user runtime data
├── NetworkManager/        # NetworkManager state
├── dbus/                  # D-Bus sockets
├── docker/                # Container runtime data
├── media/                 # Mount info for removable media
└── tmpfiles.d/            # systemd tmpfiles runtime data
```

**Notes:**

* Data **does not persist** after reboot.
* Manual file creation is **discouraged** unless required.
* `/var/run` and `/var/lock` are **symbolic links** to `/run`.

---

### `/srv` – Service data

The **/srv** directory contains **data served by system services** such as **web servers, FTP servers, NFS, and other network services**.

It is intended to hold **service-specific data** that is **provided to clients**, keeping it **separate from system files and user home directories**.
The name **srv** comes from **“service”**.

**Purpose of `/srv`**

* Stores **data that services provide to users or clients**
* Keeps service data **organized and predictable**
* Separates **service content** from system binaries and configs
* Makes backups and permissions **easier to manage**
* Recommended by **Filesystem Hierarchy Standard (FHS)**

**Examples:**

* `/srv/www/` – Website content (HTML, CSS, images)
* `/srv/ftp/` – FTP server files
* `/srv/nfs/` – NFS exported directories
* `/srv/samba/` – Samba shared data
* `/srv/tftp/` – TFTP boot files
* `/srv/git/` – Git repositories

**Standard Directory Structure of `/srv`**

```
/srv/
├── www/                   # Web server content
├── ftp/                   # FTP server data
├── nfs/                   # NFS exports
├── samba/                 # Samba shares
├── tftp/                  # TFTP service files
└── git/                   # Git repositories
```

**Notes:**

* Subdirectories are **not created by default**.
* Directory layout is **service-dependent**.
* Configuration files remain in `/etc`.
* Logs are stored in `/var/log`.
* Some distributions may still place service data in `/var/www`, but `/srv` is the **FHS-recommended location**.
* Mostly used on **servers**, rarely on desktops.

---

### `/tmp` – Temporary files

The **/tmp** directory is used to store **temporary files** created by **applications and users** during normal operation.

Files in `/tmp` are **short-lived**, **non-critical**, and can be **safely deleted**. The system may **automatically clean** this directory at boot or at regular intervals.

**Purpose of `/tmp`**

* Stores **temporary working files** for programs
* Used during **compilation, installation, and processing**
* Accessible by **all users**
* Ensures temporary data is **separate from permanent storage**
* Prevents clutter in user home directories

**Examples:**

* `/tmp/tmp.ABC123` – Application temporary file
* `/tmp/install.log` – Installer temporary log
* `/tmp/socket.tmp` – Temporary socket file
* `/tmp/cacheXXXX` – Program cache during execution
* `/tmp/.X11-unix/` – X11 temporary sockets
* `/tmp/systemd-private-*` – systemd private temp dirs

**Standard Directory Structure of `/tmp`**

```
/tmp/
├── tmp.ABC123              # Random temporary file
├── install.log             # Temporary installer log
├── socket.tmp              # Temporary socket
├── cacheXXXX               # Program cache
├── .X11-unix/              # X11 sockets
└── systemd-private-*/      # systemd private temp directories
```

**Notes:**

* `/tmp` is **world-writable** with the **sticky bit** set (`drwxrwxrwt`).
* Do **not** store important data in `/tmp`.
* Large temporary files should use `/var/tmp` instead.

---

## **5. FHS and SELinux**

SELinux follows FHS paths to define default contexts.

Examples:
- `/var/www/html` → httpd_sys_content_t
- `/var/log` → var_log_t

You should not place **service** data outside **expected FHS directories** unless you properly change **SELinux** labels.

---

## **6. FHS and Docker / Containerization**

Inside containers, a minimal FHS is used:

- A container usually includes:
  - `/bin`
  - `/sbin`
  - `/usr`
  - `/etc`
  - `/var`

- But does **not** need:
  - `/boot`
  - `/dev` (virtualized)
  - `/proc` (runtime mount)

---

## **7. Real-Life Usage for Sysadmins**

- Know where logs are stored → `/var/log`
- Know where configs are stored → `/etc`
- Know where binaries are → `/bin`, `/usr/bin`
- Know where service data is → `/var/lib/service`
- Know where to mount disks → `/mnt`, `/media`
- Know where to install custom software → `/usr/local`, `/opt`


---
