📋 Tentang Proyek
Proyek ini memungkinkan Anda untuk menjalankan ramdisk kustom pada iPhone XR dan mendapatkan akses shell penuh melalui SSH. Dibangun di atas fondasi riset luar biasa dari prdgmshift/usbliter8, skrip ini mengotomatiskan seluruh proses booting dari mode pwned DFU hingga shell SSH.

Catatan: Skrip masih dalam tahap pengembangan. Kontribusi berupa Pull Request untuk menambahkan payload atau mendukung perangkat lain sangat kami nantikan! ⭐ Jangan lupa beri bintang jika proyek ini bermanfaat.
```
🎯 Target Perangkat
Komponen	Detail
Perangkat	iPhone XR
Board	n841ap
Alur Boot	pwned DFU → iBSS → Recovery → Ramdisk → SSH
```
🔧 Persyaratan Perangkat Keras
```
Board Pengembangan PR2350-A

iPhone XR (perangkat target)

Kabel USB yang mendukung transfer data

Komputer dengan sistem operasi macOS atau Linux
```

💻 Persyaratan Perangkat Lunak
Dependensi Utama
```
python3
pyusb                     # Python package
irecovery                 # libirecovery
iproxy                    # libimobiledevice
idevice_id                # libimobiledevice
sshpass                   # Untuk otomatisasi SSH
ssh                       # Koneksi shell
Instalasi (macOS + Homebrew)
```
# Install dependencies melalui Homebrew
```
brew install libirecovery libimobiledevice usbmuxd sshpass
```
# Install Python package
python3 -m pip install pyusb
Instalasi dengan Virtual Environment (Opsional)
Jika sistem Anda membatasi instalasi paket Python secara global:

```
python3 -m venv .venv
source .venv/bin/activate
python3 -m pip install pyusb
📁 Struktur Proyek
text
📂 iPhone-XR-Ramdisk-Boot/
├── 📄 exploit.sh                 # Skrip utama booting
├── 📄 ssh_connect.sh             # Skrip koneksi ulang SSH
├── 📂 tools/
│   └── 📄 usbliter8ctl           # Helper untuk usbliter8
├── 📂 payload/
│   ├── 📄 iBSS.raw               # iBSS untuk pwned DFU
│   ├── 📄 firmware.img4          # Firmware image
│   ├── 📄 DeviceTree.img4        # DeviceTree
│   ├── 📄 ramdisk.img4           # Ramdisk custom
│   ├── 📄 trustcache.img4        # TrustCache
│   └── 📄 kernelcache.img4       # Kernel Cache
├── 📂 assets/
│   └── 🖼️ ssh-ramdisk.png        # Screenshot koneksi
└── 📂 logs/                      # Log file (auto-generated)
🚀 Panduan Penggunaan
Langkah 1: Persiapan
Pastikan semua dependensi telah terinstal dan perangkat keras terhubung dengan benar.

Langkah 2: Masuk ke Mode Pwned DFU
Gunakan board PR2350-A dengan alur usbliter8 untuk memasukkan iPhone XR ke mode pwned DFU.

Langkah 3: Jalankan Skrip
```
# Berikan izin eksekusi
```
chmod +x exploit.sh ssh_connect.sh tools/usbliter8ctl
```
# Jalankan skrip utama
```
./exploit.sh
```
Apa yang Terjadi di Balik Layar?
✅ Membuat direktori logs/ untuk menyimpan catatan

✅ Memverifikasi semua file payload yang diperlukan

✅ Menjalankan iBSS.raw dari mode pwned DFU

⏳ Menunggu perangkat memasuki mode Recovery

📤 Mengirim firmware image

📤 Mengirim DeviceTree, Ramdisk, TrustCache, dan KernelCache

⚙️ Mengatur boot-args

🚀 Menjalankan perintah bootx

🔌 Memulai iproxy 2222 22 untuk port forwarding

🔑 Mencoba koneksi SSH sebagai root

🔑 Koneksi SSH
Kata Sandi Default
text
alpine
Koneksi Otomatis
```
./ssh_connect.sh
Koneksi Manual
```
# Port forwarding
iproxy 2222 22

# Koneksi SSH (di terminal terpisah)
ssh root@localhost -p 2222
⚙️ Konfigurasi Lanjutan
Mengganti PATH Tools
Jika tools tidak berada di PATH default, gunakan environment variables:

```
IRECOVERY=/custom/path/irecovery \
PYTHON=/custom/path/python3 \
USBLITER8CTL=tools/usbliter8ctl \
IPROXY=/custom/path/iproxy \
SSHPASS=/custom/path/sshpass \
```
```
./exploit.sh
```
🐛 Pemecahan Masalah
Masalah	Solusi
usbliter8ctl melaporkan perangkat tidak pwned	Ulangi proses pwned DFU dengan PR2350-A
Timeout menunggu Recovery	Cabut USB, sambungkan kembali, ulangi pwned DFU
SSH tidak bisa terhubung	Tunggu 5-10 detik, jalankan ./ssh_connect.sh
Port 2222 sudah digunakan	pkill -f 'iproxy .*2222.*22'
Permission denied	Pastikan file memiliki izin eksekusi: chmod +x *.sh tools/*
📚 Alur Boot Diagram










🙏 Ucapan Terima Kasih
Proyek ini berdiri di atas pundak para raksasa:

prdgmshift/usbliter8 - Untuk riset exploit SecureROM A12/A13 yang luar biasa

Komunitas Jailbreak - Untuk semangat eksplorasi dan berbagi pengetahuan

⚠️ Pernyataan Sanggahan
Penting untuk dibaca:

Repositori ini disediakan untuk tujuan edukasi dan riset keamanan semata

Gunakan hanya pada perangkat yang Anda miliki atau memiliki izin resmi

Penulis tidak bertanggung jawab atas kerusakan perangkat atau kehilangan data

Pastikan untuk mematuhi hukum dan peraturan yang berlaku di wilayah Anda

Penggunaan untuk kegiatan ilegal adalah tanggung jawab pengguna sepenuhnya

📝 Kontribusi
Kami sangat menghargai kontribusi Anda! Silakan:

🍴 Fork repositori ini

🌿 Buat branch fitur baru (git checkout -b fitur-keren)

💾 Commit perubahan Anda (git commit -m 'Menambahkan fitur X')

📤 Push ke branch (git push origin fitur-keren)

🔄 Buka Pull Request

📄 Lisensi
Proyek ini dilisensikan di bawah MIT License - lihat file LICENSE untuk detail lebih lanjut.
