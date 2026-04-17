# 📘 Class 6: Linux File System (Advanced Notes)

## 🧠 1. Core Concept
- Linux follows a hierarchical file system
- Everything starts from `/` (root directory)
- No drives like C:, D:

> Golden Rule: Everything is a file

---

## 🏗️ 2. File System Structure

/
├── bin
├── boot
├── dev
├── etc
├── home
├── lib
├── var
├── tmp
├── usr
├── opt
├── mnt
├── proc
└── sys

---

## ⚙️ 3. Important Directories

### /
Root directory (God folder)

### /bin
Basic commands (ls, cp, mv, rm)

### /sbin
System admin commands

### /boot
Boot files (kernel, grub)

### /dev
Device files (hardware representation)

### /etc
Configuration files (passwd, shadow, hosts, fstab)

### /home
User directories

### /root
Root user home

### /lib, /lib64
Libraries for programs

### /var
Logs and dynamic data

### /tmp
Temporary files

### /opt
Third-party software

### /mnt
Manual mount point

### /media
Auto mount devices

### /usr
User programs and data

### /proc
Runtime system info

### /sys
Kernel and hardware info

---

## 💾 File Types

- File (-)
- Directory (d)
- Link (l)
- Block (b)
- Character (c)

---

## 🔗 Inodes
- Unique ID for each file
- Stores metadata

Command:
ls -i

---

## 🔐 Permissions

r = read
w = write
x = execute

Command:
chmod 777 file.txt

---

## 🔥 Mounting

mount /dev/sdb1 /mnt

Permanent:
/etc/fstab

---

## 📊 Disk Commands

df -h
du -sh
lsblk
fdisk -l

---

## 🧪 Practice

pwd
cd /
ls
cd /home
touch file.txt
mkdir demo
ls -l

---

## 🎯 Interview Questions

- What is Linux file system?
- What is inode?
- What is mounting?
- What is /etc?
- Difference between /mnt and /media?

---

## 🧩 Memory Trick

/ → God  
/home → Ghar  
/etc → Settings  
/var → Logs  
/dev → Hardware  
/tmp → Dustbin  
/opt → Apps  

---

## 📌 Summary

- Linux uses a single root structure
- Everything is a file
- /etc, /var, /home are key directories
