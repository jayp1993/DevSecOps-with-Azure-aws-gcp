# 🚀 Class 5 – Linux Basic Commands (Advanced Notes)

## 🔹 1. Linux File System (Root Concept)
- `/` = Root directory (Top-most folder)
- Everything in Linux starts from root
- All files, folders, and system data exist under `/`

### Example Structure:
```
/
├── home
├── etc
├── var
├── root
```

---

## 🔹 2. Root User
- The admin user in Linux
- Has full control over the system
- Can modify/delete anything ⚠️

### Command:
```
whoami
```

---

## 🔹 3. Navigation Commands

### 📌 Check Current Directory
```
pwd
```

### 📌 Change Directory
```
cd folder_name
```

### 📌 Current Directory
```
cd .
```

### 📌 Previous Directory
```
cd ..
```

---

## 🔹 4. Important Concepts
- `[OPTION]` = optional parameter
- small letters = fixed commands
- CAPITAL letters = variable (file/folder name)

---

## 🔹 5. Clear Terminal
```
clear
```

---

## 🔹 6. Help Command
```
ls --help
```

---

# ❤️ Practical Scenario (Banti & Babli)

## 🔸 Step 1: Create Folder
```
mkdir love_letter
```

## 🔸 Step 2: Create Empty File
```
touch letter.txt
```

## 🔸 Step 3: Write in File
```
nano letter.txt
```

## 🔸 Step 4: View File Content
```
cat letter.txt
```

## 🔸 Step 5: Create Backup
```
cp letter.txt backup_letter.txt
```

## 🔸 Step 6: Move / Rename File
```
mv letter.txt new_letter.txt
```

## 🔸 Step 7: Delete File
```
rm backup_letter.txt
```

---

## 🔥 rm Command (Important Options)

### Safe Delete (Interactive)
```
rm -i file.txt
```

### Force Delete
```
rm -f file.txt
```

### Delete Folder
```
rm -r love_letter
```

### ⚠️ Dangerous Command
```
rm -rf /
```

---

## 🔸 Step 8: Search in File
```
grep "love" letter.txt
```

### Case Insensitive Search
```
grep -i "love" letter.txt
```

---

# 🧠 Interview Tips
- `rm` is irreversible (no recycle bin)
- `cp` = copy, `mv` = move/rename
- `grep` is widely used for logs
- `nano` is beginner-friendly editor

---

# 💥 Real Life DevOps Usage
- Log searching → `grep`
- Backup → `cp`
- Rename during deployment → `mv`
- Cleanup → `rm`

---

# 🔥 Bonus Commands
```
ls -l   → detailed list
ls -a   → hidden files
history → command history
```
