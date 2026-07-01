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
# Sebelum memulai Pull dari LFS
```
# Install Git LFS (jika belum)
sudo apt install git-lfs

# Inisialisasi Git LFS di repositori
git lfs install

# Pull file LFS
git lfs pull
```
Verifikasi file yang sudah terdownload:
```
file payload/iBSS.raw
ls -lh payload/iBSS.raw
```

# Install Python package
python3 -m pip install pyusb
Instalasi dengan Virtual Environment (Opsional)
Jika sistem Anda membatasi instalasi paket Python secara global:
```
python3 -m venv .venv
source .venv/bin/activate
python3 -m pip install pyusb
```

# 📁 Struktur Proyek
```
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
```

# 🚀 Panduan Penggunaan
```
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

&nbsp;&nbsp;&nbsp;&nbsp; ✅ Membuat direktori logs/ untuk menyimpan catatan

&nbsp;&nbsp;&nbsp;&nbsp; ✅ Memverifikasi semua file payload yang diperlukan

&nbsp;&nbsp;&nbsp;&nbsp; ✅ Menjalankan iBSS.raw dari mode pwned DFU

&nbsp;&nbsp;&nbsp;&nbsp; ⏳ Menunggu perangkat memasuki mode Recovery

&nbsp;&nbsp;&nbsp;&nbsp; 📤 Mengirim firmware image

&nbsp;&nbsp;&nbsp;&nbsp; 📤 Mengirim DeviceTree, Ramdisk, TrustCache, dan KernelCache

&nbsp;&nbsp;&nbsp;&nbsp; ⚙️ Mengatur boot-args

&nbsp;&nbsp;&nbsp;&nbsp; 🚀 Menjalankan perintah bootx

&nbsp;&nbsp;&nbsp;&nbsp; 🔌 Memulai iproxy 2222 22 untuk port forwarding

&nbsp;&nbsp;&nbsp;&nbsp; 🔑 Mencoba koneksi SSH sebagai root

&nbsp;&nbsp;&nbsp;&nbsp; 🔑 Koneksi SSH
Kata Sandi Default
```
alpine
```
&nbsp;&nbsp;&nbsp;&nbsp; Koneksi Otomatis
```
./ssh_connect.sh
```
# Koneksi Manual

Port forwarding
```
iproxy 2222 22
```
# Koneksi SSH (di terminal terpisah)
```
ssh root@localhost -p 2222
```
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

# Masalah umum & solusi cepat

1. Perangkat tidak pwned     → Ulangi pwned DFU
2. Timeout Recovery          → Cabut USB, ulangi pwned DFU
3. SSH gagal konek           → ./ssh_connect.sh
4. Port 2222 dipakai         → pkill -f 'iproxy .*2222.*22'
5. Permission denied         → chmod +x *.sh tools/*

📚 Alur Boot Diagram
```
pwned DFU → iBSS → Recovery Mode → Kirim Firmware → Kirim DeviceTree → 
Kirim Ramdisk → Kirim TrustCache → Kirim KernelCache → bootx → SSH Shell ✅
```

# 🙏 Ucapan Terima Kasih
&nbsp;&nbsp;&nbsp;&nbsp; Proyek ini berdiri di atas pundak para raksasa:

&nbsp;&nbsp;&nbsp;&nbsp; prdgmshift/usbliter8 - Untuk riset exploit SecureROM A12/A13 yang luar biasa

&nbsp;&nbsp;&nbsp;&nbsp; Komunitas Jailbreak - Untuk semangat eksplorasi dan berbagi pengetahuan

# ⚠️ Pernyataan Sanggahan

&nbsp;&nbsp;&nbsp;&nbsp; 1. Penting untuk dibaca:

&nbsp;&nbsp;&nbsp;&nbsp; 2. Repositori ini disediakan untuk tujuan edukasi dan riset keamanan semata

&nbsp;&nbsp;&nbsp;&nbsp; 3. Gunakan hanya pada perangkat yang Anda miliki atau memiliki izin resmi

&nbsp;&nbsp;&nbsp;&nbsp; 4. Penulis tidak bertanggung jawab atas kerusakan perangkat atau kehilangan data

&nbsp;&nbsp;&nbsp;&nbsp; 5. Pastikan untuk mematuhi hukum dan peraturan yang berlaku di wilayah Anda

&nbsp;&nbsp;&nbsp;&nbsp; 6. Penggunaan untuk kegiatan ilegal adalah tanggung jawab pengguna sepenuhnya

# 📝 Kontribusi
Kami sangat menghargai kontribusi Anda! Silakan:

&nbsp;&nbsp;&nbsp;&nbsp; 🍴 Fork repositori ini

&nbsp;&nbsp;&nbsp;&nbsp; 🌿 Buat branch fitur baru (git checkout -b fitur-keren)

&nbsp;&nbsp;&nbsp;&nbsp; 💾 Commit perubahan Anda (git commit -m 'Menambahkan fitur X')

&nbsp;&nbsp;&nbsp;&nbsp; 📤 Push ke branch (git push origin fitur-keren)

&nbsp;&nbsp;&nbsp;&nbsp; 🔄 Buka Pull Request

# 📄 Lisensi
Proyek ini dilisensikan di bawah MIT License - lihat file LICENSE untuk detail lebih lanjut.
