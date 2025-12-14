# `/usr` Directory Hierarchy Explanation:

### **`/usr/bin` – User Binaries (Primary command location)**
  
Contains the vast majority of **user executable commands**. On modern systems, `/bin` is typically a **symlink** here.
  
**Examples:**

* `vim`, `nano` – text editors
* `git` – version control system
* `python3`, `node` – programming language interpreters
* `ssh`, `curl`, `wget` – network utilities
  
  
### `/usr/sbin` – System Administration Binaries

Contains **system administration commands** that are typically run by the **root user**. On **modern** systems, `/sbin` is typically a **symlink** here.

**Examples:**
* `httpd`, `nginx` – web servers
* `cron`, `systemctl` – system services
* `sshd` – SSH server daemon
* `useradd`, `groupadd` – user management
  
  
### `/usr/lib` – Shared Libraries
  
Contains **shared library files** (.so files) needed by programs in `/usr/bin` and `/usr/sbin`. Architecture-dependent (contains **64-bit** or **32-bit** specific code).

Like:
* `lib` - Contains **32-bit** specific **library** files
* `lib64` - Contains **64-bit** specific **library** files

> **Analogous to:** DLL files in Windows' `System32` directory.
  
  
### `/usr/share` – Architecture-Independent Shared Data

Contains **data** files used by **applications** that don't depend on the computer's processor type. This includes **documentation, graphics, fonts, and configuration** schemas that work the same way whether you're on an **Intel, AMD, ARM** (like Raspberry Pi), or other CPU.

**Contains:**
* `icons/`, `themes/` – Graphical assets
* `fonts/` – Font files
* `doc/`, `man/` – Documentation and manual pages
* `applications/` – Desktop application entries (.desktop files)

### `/usr/local` – Locally installed software
For software installed manually **by the administrator**, not through package manager.

**Inside:**
* `/usr/local/bin`
* `/usr/local/sbin`
* `/usr/local/lib`

Useful for custom apps or source-compiled tools.