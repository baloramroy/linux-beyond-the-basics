**Runtime data** means **temporary information that programs and the operating system need *while they are running***, but **do not need after a reboot or restart**.

Think of runtime data as **“live state information”** — it describes **what is happening right now**, not permanent configuration or long-term data.

---

## What runtime data is **NOT**

• Not configuration files (`/etc`)
• Not user data (`/home`)
• Not logs meant to be kept long-term (`/var/log`)
• Not program binaries (`/bin`, `/usr/bin`)

---

## What runtime data **IS**

Runtime data includes things like:

### 1. **Process identification**

Used to track running services.

* **PID files**
  Example:

  * `/run/sshd.pid` → Tells the system *which process ID sshd is using*

Why needed:

* To stop, reload, or monitor the service correctly

---

### 2. **Sockets (communication endpoints)**

Used for **inter-process communication (IPC)**.

* **UNIX sockets**
  Example:

  * `/run/docker.sock`
  * `/run/dbus/system_bus_socket`

Why needed:

* Allows programs to **talk to each other** while running

---

### 3. **Lock files**

Prevent multiple programs from using the same resource at once.

* Example:

  * `/run/lock/atd.lock`

Why needed:

* Avoids **conflicts and data corruption**

---

### 4. **Service and daemon state**

Stores the **current state** of a service.

* Example:

  * `/run/NetworkManager/`
  * `/run/systemd/`

Why needed:

* Remembers *current connections, sessions, or runtime flags*

---

### 5. **User session data**

Information related to logged-in users.

* Example:

  * `/run/user/1000/`

Why needed:

* Tracks **active sessions, user services, temporary sockets**

---

## Why runtime data is stored in `/run`

• Must be available **very early during boot**
• Must be **fast** (RAM-based)
• Must be **automatically cleaned** on reboot
• Must **not survive system restart**

That’s why `/run` is:

* **tmpfs (RAM-based)**
* **emptied on every boot**

---

## One-line mental model (very important)

> **Runtime data = “Information a program needs while it is running, and can safely lose when the system restarts.”**

---

## Quick comparison (at a glance)

| Location | Contains                        |
| -------- | ------------------------------- |
| `/etc`   | Configuration (how to run)      |
| `/var`   | Variable data (logs, databases) |
| `/run`   | Runtime data (current state)    |
| `/tmp`   | User temporary files            |

---

If you want, I can also give you a **real-life analogy** or a **RHCSA/RHCE exam-focused definition** next.
