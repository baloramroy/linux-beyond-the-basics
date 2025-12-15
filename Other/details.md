
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

**Core Principles**

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
