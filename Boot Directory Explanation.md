# **/boot – Boot Loader Files**

So as we learn before that, The **/boot** directory contains **static files required to start (boot) the Linux operating system** right?


---

When you power on a Linux system, Linux does **not start immediately**.

At that moment:

* There is **no `/` directory**
* There are **no services**
* There are **no drivers loaded**
* The system only knows one thing: *“I need instructions to start.”*

Those instructions live inside **`/boot`**.

---

### What Is `/boot`?

The **`/boot` directory contains the files that help Linux start running**.

You can think of `/boot` as:

> **A small instruction folder that tells the system how to wake up and start Linux.**

If these files are missing or damaged, **Linux cannot start at all**.

---

## Why `/boot` Exists in the Linux Filesystem Hierarchy

At power-on time, the system firmware (**BIOS/UEFI**) is very limited. 

At that time the **firmware**:

* **Cannot** Understand complex filesystems
* **Cannot** Load full drivers
* **Cannot** Access the complete Linux directory structure

So Linux needs a **simple, predictable place** where the most important **startup** files are stored.

That place is **`/boot`**.

**Simple Example**

Think of a **school**:

* The **main building** = `/`
* Classrooms, labs, library = `/usr`, `/var`, `/home`
* The **school gate with instructions and rules** = `/boot`

Without the **gate and rules**, students cannot even enter the school.

---

### What Files Are Inside `/boot` 

>Only the Important Ones

You don’t need to **memorize** everything. Just understand **what each file does**.

---

**1. Linux Kernel (`vmlinuz-*`)**

```
/boot/vmlinuz-5.14.0-427.el9.x86_64
```

This is the **core of Linux**.

* Controls CPU, memory, processes, and hardware
* Loaded first when Linux starts
* Without this file → **Linux does not exist**

👉 This is the **heart** of the operating system.

---

**1. Initial RAM Disk (`initramfs-*`)**

```
/boot/initramfs-5.14.0-427.el9.x86_64.img
```

This is a **temporary helper system**.

It helps Linux:

* Load disk drivers
* Find the root filesystem (`/`)
* Mount storage like LVM or RAID

Once Linux is fully running, this file is **no longer needed**.

👉 Think of it as **training wheels** for Linux startup.

---

### 3. Bootloader Files (GRUB)

```
/boot/grub2/grub.cfg
```

These files tell the system:

* Which kernel to start
* Which initramfs to use
* What options to pass during boot

That’s why you sometimes see a **boot menu** to choose kernel when starting Linux.

---

## Why `/boot` Is Critical (Real-World View)

In real systems:

* If `/boot` is **missing** → system will not boot
* If kernel files are **deleted** → Linux is gone
* If initramfs is **broken** → root filesystem cannot be mounted
* Kernel updates **directly modify `/boot`**

This is why:

* `/boot` is usually **small**
* Often placed on a **separate partition**
* System admins handle it **very carefully**

---

### Final One-Line Summary

> **`/boot` contains the essential startup files that guide a Linux system from power-on to a running operating system. Without it, Linux cannot start.**

---

This is why `/boot` is one of the **most critical directories** in Linux and is usually kept **small, clean, and untouched**.

