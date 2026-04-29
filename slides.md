---
marp: true
theme: default
class:
  - invert
paginate: true
backgroundColor: '#1e1e2e'
color: '#cdd6f4'
size: 16:9
style: |
  section {
    font-family: "Segoe UI", sans-serif;
    padding: 1.2em;
  }
  h1 { color: #89b4fa; font-size: 1.8em; margin-bottom: 0.3em; }
  h2 { color: #b4befe; font-size: 1.3em; margin-bottom: 0.2em; }
  h3 { color: #cba6f7; font-size: 1em; }
  code {
    background-color: #313244;
    color: #a6e3a1;
    padding: 1px 4px;
    border-radius: 3px;
    font-size: 0.8em;
    font-family: "Fira Code", monospace;
  }
  pre {
    background-color: #181825;
    padding: 8px;
    border-radius: 4px;
    font-size: 0.7em;
    overflow-x: auto;
  }
  ul { line-height: 1.5; font-size: 0.85em; }
  li { margin-bottom: 0.2em; }
  blockquote {
    border-left: 3px solid #89b4fa;
    padding-left: 0.6em;
    color: #bac2de;
    font-size: 0.8em;
  }
  table { width: 100%; font-size: 0.75em; }
  th { background: #313244; color: #f9e2af; padding: 4px; }
  td { padding: 3px 5px; border-bottom: 1px solid #45475a; }
  .center { text-align: center; }
  .two-col { display: grid; grid-template-columns: 1fr 1fr; gap: 1em; }
  footer { color: #6c7086; font-size: 0.6em; }
---

<!-- _class: lead -->

# Linux Fundamental

### From Zero to Terminal Hero

---

# Agenda

<div class="two-col" style="font-size: 0.75em;">

1. Struktur Direktori
2. File Management
3. Navigasi
4. Membaca File
5. Text Processing
6. System Monitoring
7. ...

</div>

---

<div class="two-col" style="font-size: 0.75em;">

7. Process Management
8. Compression
9. Networking
10. Permission
11. SSH
12. Package Manager
13. Terminal Editor

</div>

---

<!-- _class: lead -->

# 1. Struktur Direktori

---

# FHS - Filesystem Hierarchy

| Direktori | Fungsi |
|-----------|--------|
| `/` | Root - titik awal semua direktori |
| `/bin` | Command dasar sistem (ls, cp, mv) |
| `/sbin` | System binaries (fdisk, reboot) |
| `/usr/bin` | User command (banyak installed apps) |
| `/usr/sbin` | System admin commands |
| `/usr/local/bin` | User-installed software |
| `/etc` | Konfigurasi sistem |
| `/home` | Direktori user |
| `/var` | Log, cache, mail |
| `/tmp` | File sementara |
| `/dev` | Device files |

---

# Konsep Penting

> Di Linux, **everything is a file** — hardware direpresentasikan sebagai file di `/dev`

```text
/
├── bin/     ← Command dasar
├── etc/     ← Konfigurasi
├── home/    ← Data user
├── var/     ← Log & cache
└── dev/     ← Devices
```

---

<!-- _class: lead -->

# 2. File Management

---

# mkdir — Buat Direktori

**Membuat folder baru**

```bash
mkdir myfolder
mkdir -p parent/child
```

> `-p` = buat parent jika belum ada

---

# touch — Buat File Kosong

**Membuat file atau update timestamp**

```bash
touch file.txt
touch -t 202401011200 file
```

---

# cp — Menyalin File

**Menyalin file atau direktori**

```bash
cp file.txt backup.txt
cp -r folder/ backup/
```

> `-r` = recursive untuk folder

---

# mv — Pindahkan / Rename

**Memindahkan atau rename file**

```bash
mv old.txt new.txt
mv file.txt /home/user/
```

---

# rm — Hapus File

**Menghapus file atau folder**

```bash
rm file.txt
rm -r folder/
rm -rf folder/
```

> ⚠️ `rm -rf /` = hapus seluruh sistem! Linux tidak ada Recycle Bin.

---

<!-- _class: lead -->

# 3. Navigasi

---

# pwd — Print Working Directory

**Menampilkan lokasi direktori sekarang**

```bash
pwd
# Output: /home/alice/projects
```

---

# cd — Change Directory

**Berpindah ke direktori lain**

```bash
cd /etc        # Absolute path
cd ..          # Parent directory
cd ~           # Home directory
cd -           # Direktori sebelumnya
```

---

# ls — List Isi Direktori

**Menampilkan isi direktori**

```bash
ls
ls -l
ls -a
ls -lh
ls -lt
```

---

# ls — Opsi Lengkap

| Opsi | Arti |
|------|------|
| `-l` | Format detail |
| `-a` | Tampilkan hidden files |
| `-lh` | Size dalam KB/MB/GB |
| `-lt` | Sort by modification time |

---

# find — Mencari File

**Mencari file berdasarkan nama, ukuran, tanggal**

```bash
find / -name "config.txt"
find . -name "*.log"
find . -mtime -7
find . -size +100M
```

---

<!-- _class: lead -->

# 4. Membaca File

---

# cat — Concatenate

**Menampilkan seluruh isi file**

```bash
cat file.txt
cat file1.txt file2.txt
```

> Cocok untuk file kecil

---

# head — Baris Awal

**Menampilkan 10 baris pertama**

```bash
head file.txt
head -n 5 file.txt
```

> Berguna untuk cek header file

---

# tail — Baris Akhir

**Menampilkan 10 baris terakhir**

```bash
tail file.txt
tail -f /var/log/syslog
```

> `-f` = follow mode untuk monitoring log real-time

---

# less — Pager Interaktif

**Menampilkan file dengan scroll**

```bash
less file.txt
```

**Navigasi:** Arrow, PageUp/Down, `/` search, `q` quit

> Cocok untuk file besar

---

# wc — Word Count

**Statistik file (baris, kata, byte)**

```bash
wc file.txt             # baris kata byte
wc -l file.txt          # Hanya jumlah baris
wc -w file.txt          # Hanya jumlah kata
wc -c file.txt          # Hanya jumlah byte
```

---

<!-- _class: lead -->

# 5. Text Processing

---

# grep — Pattern Matching

**Mencari text dalam file**

```bash
grep "error" file.log
grep -i "error" file.log    # Case insensitive
grep -r "pattern" folder/   # Recursive
grep -n "word" file.txt     # Dengan nomor baris
ps aux | grep nginx         # Cari di proses
```

---

# sed — Stream Editor

**Edit text secara stream**

```bash
sed 's/old/new/' file.txt        # Replace pertama
sed 's/old/new/g' file.txt       # Replace semua
sed -i 's/old/new/g' file.txt    # Edit in-place
sed '1,5d' file.txt              # Hapus baris 1-5
```

---

# awk — Text Processing

**Memproses text berbasis kolom**

```bash
awk '{print $1}' file.txt           # Print kolom 1
awk -F: '{print $1}' /etc/passwd    # Delimiter :
awk '$3 > 100' file.txt             # Filter angka
```

---

# Pipes & Redirects

**Menghubungkan output antar command**

```bash
echo "text" > file.txt      # Write (overwrite)
echo "text" >> file.txt     # Append
cat < file.txt              # Input dari file
cat file | grep error       # Pipe output
cmd > /dev/null 2>&1        # Suppress all output
```

---

<!-- _class: lead -->

# 6. System Monitoring

---

# top — Task Manager

**Menampilkan proses secara real-time**

```bash
top
```

| Kolom | Arti |
|-------|------|
| PID | Process ID |
| USER | Pemilik proses |
| %CPU | Penggunaan CPU |
| %MEM | Penggunaan RAM |
| COMMAND | Nama program |

**Shortcuts:** `k`=kill, `q`=quit, `M`=sort RAM, `P`=sort CPU

---

# df — Disk Free

**Menampilkan penggunaan disk**

```bash
df -h
```

---

# du — Disk Usage

**Menampilkan size folder**

```bash
du -sh /var/log
du -h --max-depth=1 .
```

---

# free — Memory Usage

**Menampilkan penggunaan RAM**

```bash
free -h
```

> `available` lebih penting dari `free` — Linux cache agresif

---

# uname & uptime — System Info

**Informasi sistem**

```bash
uname -a        # Semua info
uname -r        # Kernel version
uptime          # Waktu aktif sistem
whoami          # User sekarang
id              # User & group ID
```

---

<!-- _class: lead -->

# 7. Process Management

---

# ps — Process Status

**Melihat list proses**

```bash
ps              # Proses sendiri
ps aux          # Semua proses
ps aux | grep nginx
```

---

# kill — Menghapus Proses

**Menghentikan proses**

```bash
kill 1234              # Graceful (SIGTERM)
kill -9 1234           # Force kill (SIGKILL)
kill -15 1234          # Graceful (explicit)
pkill nginx            # Kill by name
killall nginx          # Kill semua instance
```

| Signal | Arti |
|--------|------|
| `-15` (TERM) | Graceful — proses cleanup dulu |
| `-9` (KILL) | Paksa — langsung terminate |
| `-1` (HUP) | Reload config |

---

# nice & renice — Priority

**Mengatur prioritas proses**

```bash
nice -n 10 ./script.sh    # Jalankan dengan priority rendah
renice 5 -p 1234          # Ubah priority proses berjalan
```

> Priority: -20 (tertinggi) sampai +20 (terendah)

---

# Background & Foreground

**Menjalankan proses di background**

```bash
./script.sh &            # Jalankan di background
Ctrl+Z                   # Pause & masukkan background
bg                       # Lanjutkan di background
fg                       # Bawa ke foreground
jobs                     # List background jobs
```

---

<!-- _class: lead -->

# 8. Compression

---

# tar — Tape Archive

**Membuat/extracting archive**

```bash
tar -cvf archive.tar folder/     # Create
tar -xvf archive.tar             # Extract
tar -cvzf archive.tar.gz folder/ # Compress gzip
tar -xvzf archive.tar.gz         # Extract gzip
tar -tvf archive.tar             # List isi
```

---

# zip & unzip

**Kompresi format zip**

```bash
zip -r backup.zip folder/
unzip backup.zip
```

---

# gzip — GNU Zip

**Kompresi single file**

```bash
gzip file.txt          # → file.txt.gz
gunzip file.txt.gz     # → file.txt
```

---

<!-- _class: lead -->

# 9. Networking

---

# ping — Test Konektivitas

**Menguji koneksi ke host**

```bash
ping google.com
ping -c 4 8.8.8.8      # 4 kali lalu stop
```

---

# curl — HTTP Client

**Mengambil data dari server**

```bash
curl https://api.example.com
curl -I https://google.com    # Headers saja
curl -o file.zip URL          # Download file
curl -X POST -d "k=v" URL     # POST request
```

---

# wget — Download

**Mendownload file dari web**

```bash
wget https://example.com/file.tar.gz
wget -c URL    # Resume download
```

---

# Network Tools

| Command | Fungsi |
|---------|--------|
| `ip addr` | Konfigurasi network |
| `netstat -tuln` | Port yang terbuka |
| `ss -tuln` | Alternatif modern netstat |
| `nslookup` | DNS lookup |
| `traceroute` | Trace routing path |

---

<!-- _class: lead -->

# 10. Permission

---

# Permission Model

**3-level permission: Owner | Group | Others**

```text
-rwxr-xr-- 1 alice dev file.txt
 |||||||||
 ||||||||└── Others (r--)
 ||||||└──── Group (r-x)
 ||||└────── Owner (rwx)
 |└───────── Type (-=file, d=dir)
```

| Simbol | Nilai | Arti |
|--------|-------|------|
| `r` | 4 | Read (baca) |
| `w` | 2 | Write (tulis) |
| `x` | 1 | Execute (jalankan) |

---

# chmod — Change Mode

**Mengubah permission file**

```bash
chmod 755 script.sh    # rwxr-xr-x
chmod 644 file.txt     # rw-r--r--
chmod 777 folder/      # Full access (WASPADA!)

# Symbolic mode
chmod u+x file         # Tambah execute owner
chmod go-w file        # Hapus write group/others
chmod a+r file         # Read untuk semua
```

| Target | Arti |
|--------|------|
| `u` | User (owner) |
| `g` | Group |
| `o` | Others |
| `a` | All |

---

# chown — Change Owner

**Mengubah owner dan group**

```bash
chown bob file.txt
chown bob:dev file.txt
chown -R bob:dev folder/
chgrp dev file.txt
```

---

# Special Permissions

**SetUID, SetGID, Sticky Bit**

```bash
chmod u+s program    # SetUID - jalankan sebagai owner
chmod g+s folder/    # SetGID - inherit group
chmod +t /tmp        # Sticky bit - hanya owner bisa hapus
```

---

<!-- _class: lead -->

# 11. SSH

---

# SSH — Secure Shell

**Akses remote terminal dengan enkripsi**

```text
Client (laptop) ←── Encrypted (port 22) ──→ Server
```

---

# SSH Connect

**Menghubungkan ke server remote**

```bash
ssh user@192.168.1.100
ssh -p 2222 user@server.com   # Port custom
```

---

# SSH Key Generation

**Membuat key pair untuk autentikasi**

```bash
ssh-keygen -t ed25519 -C "email@example.com"
# Output:
# ~/.ssh/id_ed25519      (private - JANGAN SEBAR)
# ~/.ssh/id_ed25519.pub  (public - copy ke server)
```

---

# SSH Key Copy

**Menyalin public key ke server**

```bash
ssh-copy-id user@server.com
# Sekarang login tanpa password!
```

---

# scp — Secure Copy

**Transfer file via SSH**

```bash
scp file.txt user@server:/path/      # Upload
scp user@server:/path/file.txt .     # Download
scp -r folder/ user@server:/path/   # Folder
```

---

# rsync — Remote Sync

**Sinkronisasi folder**

```bash
rsync -avz folder/ user@server:/backup/
rsync -avz --delete folder/ user@server:/backup/
```

> `-a`=archive, `-v`=verbose, `-z`=compress

---

<!-- _class: lead -->

# 12. Package Manager

---

# APT — Debian/Ubuntu

**Advanced Package Tool**

```bash
apt update                # Update repo list
apt upgrade               # Upgrade semua paket
apt install nginx         # Install paket
apt remove nginx          # Uninstall paket
apt search nginx          # Cari paket
apt list --installed      # List installed
apt autoremove            # Hapus unused deps
```

---

# YUM/DNF — RHEL/CentOS/Fedora

**Yellowdog Updater Modified / Dandified YUM**

```bash
yum update                # Update semua
yum install nginx         # Install
yum remove nginx          # Remove
yum search nginx          # Search
yum list installed        # List installed
dnf update                # DNF (newer)
```

---

# Pacman — Arch Linux

**Package Manager**

```bash
pacman -Syu               # Sync & upgrade
pacman -S nginx           # Install
pacman -R nginx           # Remove
pacman -Ss nginx          # Search
pacman -Q                 # List installed
```

---

# Snap & Flatpak — Universal

**Cross-distro package managers**

```bash
# Snap
snap install nginx
snap remove nginx
snap list

# Flatpak
flatpak install flathub org.nginx.Nginx
flatpak run org.nginx.Nginx
```

---

# Package Manager Comparison

| Distro | Manager | Command |
|--------|---------|---------|
| Debian/Ubuntu | APT | `apt install` |
| RHEL/CentOS/Fedora | DNF | `dnf install` |
| Arch Linux | Pacman | `pacman -S` |
| openSUSE | Zypper | `zypper install` |
| Universal | Snap | `snap install` |
| Universal | Flatpak | `flatpak install` |

---

<!-- _class: lead -->

# 13. Environment Variables

---

# Environment Variables

**Variabel yang menyimpan konfigurasi sistem**

```bash
echo $PATH              # Lihat PATH
echo $HOME              # Lihat home directory
echo $USER              # Lihat username
env                     # Lihat semua env var
printenv                # Sama dengan env
```

---

# Set & Export Variables

**Mengatur variabel lingkungan**

```bash
export VAR=value        # Set untuk session ini
VAR=value command       # Set untuk satu command
echo 'export VAR=value' >> ~/.bashrc  # Permanent
```

---

# PATH — Command Search

**Direktori yang dicari untuk command**

```bash
echo $PATH
# /usr/local/bin:/usr/bin:/bin

# Tambah direktori ke PATH
export PATH=$PATH:/new/directory
```

---

<!-- _class: lead -->

# 14. Shell Config Files

---

# ~/.bashrc & ~/.zshrc

**Konfigurasi shell untuk user**

```bash
# ~/.bashrc (bash)
# ~/.zshrc (zsh)

# Contoh konfigurasi
alias ll='ls -lh'
alias gs='git status'
export EDITOR=vim
export PATH=$PATH:/usr/local/bin
```

---

# Apply Changes

**Memuat ulang konfigurasi**

```bash
source ~/.bashrc        # Reload bash
source ~/.zshrc         # Reload zsh
. ~/.bashrc            # Shortcut
```

---

# ~/.bash_profile vs ~/.bashrc

| File | Ketika |
|------|--------|
| `~/.bash_profile` | Login shell (SSH, terminal baru) |
| `~/.bashrc` | Interactive non-login shell |

> Untuk consistency, source `.bashrc` dari `.bash_profile`

---

<!-- _class: lead -->

# 15. Sudo & Su

---

# sudo — Super User Do

**Menjalankan command sebagai root/superuser**

```bash
sudo apt update
sudo nano /etc/hosts
sudo -i                   # Interactive root shell
sudo -u bob command       # Sebagai user lain
sudo -k                   # Reset timestamp
```

---

# sudoers — Configure sudo

**Mengatur siapa boleh sudo**

```bash
sudo visudo              # Edit sudoers file

# Contoh: alice boleh sudo tanpa password
alice ALL=(ALL) NOPASSWD: ALL
```

---

# su — Switch User

**Berpindah ke user lain**

```bash
su -                    # Switch ke root
su - alice              # Switch ke alice
su -c "command" root    # Jalankan command sebagai root
```

---

# Perbedaan sudo vs su

| Command | Use Case |
|---------|----------|
| `sudo` | Sekali-sekali, lebih aman |
| `su` | Sering jadi user lain |
| `sudo -i` | Interactive root session |

> **Best Practice:** Gunakan `sudo` daripada `su` untuk keamanan

---

<!-- _class: lead -->

# 16. Terminal Editor

---

# nano — Editor Pemula

**Editor sederhana dengan shortcut di bawah**

```bash
nano file.txt
```

| Shortcut | Fungsi |
|----------|--------|
| `Ctrl+O` | Save |
| `Ctrl+X` | Exit |
| `Ctrl+W` | Search |
| `Ctrl+K` | Cut line |
| `Ctrl+U` | Paste |
| `Ctrl+G` | Help |

---

# vim — Editor Powerful

**3 mode: Normal | Insert | Command**

```text
Normal (default) ←→ Insert (i/a/o) ←→ Command (:)
```

---

# vim — Normal Mode

**Navigasi dan editing**

| Key | Aksi |
|-----|------|
| `h j k l` | Kiri / bawah / atas / kanan |
| `gg` | Ke awal file |
| `G` | Ke akhir file |
| `dd` | Hapus baris |
| `yy` | Copy baris |
| `p` | Paste |
| `u` | Undo |
| `x` | Hapus karakter |

---

# vim — Command Mode

**Save, quit, search**

```vim
:w          " Save
:q          " Quit
:wq / :x    " Save & quit
:q!         " Quit tanpa save
:set nu     " Tampilkan nomor baris
/search     " Cari teks
n           " Next match
```

---

# vim — Insert Mode

**Mengetik text**

| Key | Aksi |
|-----|------|
| `i` | Insert sebelum cursor |
| `a` | Insert setelah cursor |
| `o` | Baris baru di bawah |
| `O` | Baris baru di atas |
| `Esc` | Kembali ke Normal mode |

---

<!-- _class: lead -->

# Quick Reference

---

# Command Summary

<div style="font-size: 0.7em;">

| Kategori | Command |
|----------|---------|
| **Navigasi** | cd, pwd, ls, find |
| **File** | mkdir, cp, mv, rm, touch |
| **Baca** | cat, head, tail, less, wc -l |
| **Text** | grep, sed, awk |
| **Monitor** | top, df, du, free |
| **Process** | ps, kill, kill -9 |
| **Compress** | tar, zip, gzip |
| **Network** | ping, curl, wget, ssh |
| **Package** | apt, dnf, pacman |
| **Permission** | chmod, chown |
| **Env** | echo $VAR, export |
| **Config** | ~/.bashrc, ~/.zshrc |
| **Admin** | sudo, su |

</div>

---

<!-- _class: lead -->

# Terima Kasih

**Praktikkan setiap hari!**

```bash
echo "Linux is fun!"
```
