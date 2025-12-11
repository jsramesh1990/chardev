# Simulated Char Device Driver Project

## Overview
This project simulates a **full Linux char device driver** with:

- Open/Close, Read/Write
- Circular buffer management
- Sequence-numbered messages
- IOCTL commands (Reset / Status)
- Mutex for concurrency
- Userspace Python client for testing

## Structure
```
simulated-chardev/
│
├── src/
│   └── simulated_chardev.c     # Kernel module (driver)
│
├── include/
│   └── simulated_chardev.h     # IOCTL definitions, structs, constants
│
├── user/
│   └── test_client.py          # User-space test tool for read/write/ioctl
│
├── Makefile                    # Build system for kernel module
│
└── README.md                   # Documentation

```

## Build & Load
```
make
sudo insmod src/simulated_chardev.ko
dmesg | tail
sudo mknod /dev/simchardev c <major> 0
sudo chmod 666 /dev/simchardev
```
##    Features Implemented
Driver Capabilities

Registers a dynamic major number

Creates a simulated character device

Implements:
```
open()

read()

write()

release()

unlocked_ioctl()
```
##Maintains:

Circular buffer

Sequence numbers

CRC32 checksum for each message

Provides IOCTLs:

SIMC_RESET_BUFFER → Clear buffer

SIMC_GET_STATUS → Get message count

SIMC_GET_LAST_MSG_INFO → Get last message’s checksum + sequence

##    Build Instructions
1. Install kernel headers
   ```
sudo apt install build-essential linux-headers-$(uname -r)
```
3. Build the kernel module

From project root:
```
make
```

If successful, it generates:
```
src/simulated_chardev.ko
```
Working Flow
<img width="677" height="627" alt="char" src="https://github.com/user-attachments/assets/b70f0c97-0a66-4253-8a36-06b74d8da114" />


Working Diagram
```
flowchart TD
    A[load module: insmod] --> B[simchardev_init()]
    B --> C[register_chrdev]
    C --> D[/dev/simchardev]

    D --> E[open()]
    E --> F[device_open()]

    D --> G[write()]
    G --> H[device_write(): store msg + seq + CRC]

    D --> I[read()]
    I --> J[device_read(): fetch from circular buffer]

    D --> K[ioctl()]
    K --> L[RESET BUFFER]
    K --> M[GET STATUS]

    E --> N[close()]
    N --> O[device_release()]

    A --> P[unload module: rmmod]
    P --> Q[unregister_chrdev]
```

#  Test:
```
python3 user/test_client.py
```
#  Unload
```
sudo rmmod simulated_chardev

---

This gives you a **ready-to-use, full simulation project** demonstrating all the typical **char device driver logic, sequence handling, IOCTL, concurrency, buffer management, and testing workflow**.

---

