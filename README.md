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
simulated-chardev/
├── src/simulated_chardev.c
├── include/simulated_chardev.h
├── user/test_client.py
├── Makefile
└── README.md


## Build & Load
```bash
make
sudo insmod src/simulated_chardev.ko
dmesg | tail
sudo mknod /dev/simchardev c <major> 0
sudo chmod 666 /dev/simchardev

#  Test
python3 user/test_client.py

#  Unload
sudo rmmod simulated_chardev

---

This gives you a **ready-to-use, full simulation project** demonstrating all the typical **char device driver logic, sequence handling, IOCTL, concurrency, buffer management, and testing workflow**.

---

If you want, I can also **add a full flow diagram in the README** showing **sequence from kernel module → buffer → Python client → IOCTL** visually, so it becomes fully educational and GitHub-ready.  

Do you want me to do that?
