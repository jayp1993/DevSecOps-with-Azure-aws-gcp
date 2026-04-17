# 📘 Class 7: Linux Notes

## 🧭 1. Path (Linux File System Basics)

- Root directory = `/`
- Home directory = `/home/user`

### 🔹 Types of Path

#### ✅ Absolute Path
- Always starts with `/`
- Full path from root

Example:
```
/home/manish
/root
/home/jay
```

#### ✅ Relative Path
- Based on current directory
- Does not start with `/`

Symbols:
```
.   = current directory
..  = previous directory
```

Example:
```
../home/manish
../jay
../../root
```

---

## 💻 2. Important Linux System Commands

- `lscpu` → CPU info  
- `free -h` → RAM usage  
- `df -h` → Disk usage  
- `cat /etc/os-release` → OS details  
- `hostname -i` → IP address  
- `uptime` → System uptime  
- `ps` → Running processes  
- `top` → Live process monitor  
- `kill <PID>` → Kill process  
- `du -h` → Directory size  

---

## 📦 3. Package Managers

| OS      | Package Manager |
|---------|----------------|
| Ubuntu  | apt            |
| RedHat  | yum            |
| CentOS  | dnf            |
| Alpine  | apk            |

### 🔹 APT Commands

```
apt update
apt list
apt search nginx
apt install nginx
apt list --installed
```

---

## ⚙️ 4. Systemctl Commands

```
systemctl status nginx
systemctl start nginx
systemctl stop nginx
systemctl restart nginx
systemctl enable nginx
systemctl disable nginx
```

---

## 🚀 5. 1-Tier App Deployment Steps

### Step 1: Clone Code
```
git clone https://github.com/jayp1993/Netflix-Clone.git
```

### Step 2: Install Nginx
```
apt update
apt install nginx
```

### Step 3: Deploy Code
```
/var/www/html/
```

---

## 🎯 Final Goal

Deploy a 1-Tier application on Linux.

---

## 🧠 Quick Revision

- `/` = Root  
- Absolute path = Full path  
- Relative path = Current location  
- APT = Package manager  
- Nginx = Web server  
- systemctl = Service control  
- `/var/www/html` = Web root directory  
