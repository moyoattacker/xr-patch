Skrip ini masih dalam tahap pengembangan dan belum sempurna. Kami sangat menghargai kontribusi Anda melalui Pull Request untuk menambahkan payload atau mendukung lebih banyak perangkat. Jika proyek ini bermanfaat bagi Anda, jangan lupa berikan tanda bintang (Star) sebagai bentuk dukungan.

Proyek ini memungkinkan Anda untuk menjalankan ramdisk kustom pada iPhone XR dan mengakses shell melalui usbmux / SSH.

Teknologi dasar yang digunakan berasal dari prdgmshift/usbliter8. tools/usbliter8ctl berfungsi untuk menjalankan payload/iBSS.raw dari mode pwned DFU, kemudian exploit.sh melanjutkan proses dengan mengirimkan firmware, DeviceTree, ramdisk, trustcache, dan kernelcache melalui irecovery.

https://assets/ssh-ramdisk.png

Target Perangkat
Perangkat: iPhone XR

Board: n841ap

Alur boot: pwned DFU -> iBSS -> Recovery -> ramdisk boot -> SSH

Persyaratan Perangkat Keras
Board pengembangan PR2350-A

iPhone XR

Kabel USB

Komputer dengan sistem operasi macOS atau Linux

Sebelum menjalankan skrip, pastikan Anda telah menggunakan PR2350-A bersama alur usbliter8 untuk memasukkan perangkat ke mode pwned DFU.

Dependensi Perangkat Lunak
python3

Paket Python: pyusb

irecovery

iproxy

idevice_id

sshpass

ssh

Untuk pengguna macOS dengan Homebrew, Anda dapat menginstal dependensi dengan perintah berikut:

bash
brew install libirecovery libimobiledevice usbmuxd sshpass
python3 -m pip install pyusb
Jika sistem Anda melarang instalasi paket Python secara global, Anda dapat menggunakan lingkungan virtual:

bash
python3 -m venv .venv
source .venv/bin/activate
python3 -m pip install pyusb
Struktur Berkas
text
exploit.sh                 Skrip utama untuk proses boot
ssh_connect.sh             Skrip untuk menghubungkan kembali SSH
tools/usbliter8ctl         Helper lokal berbasis alur USB usbliter8
payload/iBSS.raw           Raw iBSS yang dimuat dari pwned DFU
payload/*.img4             Firmware, DeviceTree, ramdisk, trustcache, kernelcache
assets/ssh-ramdisk.png     Tangkapan layar koneksi SSH berhasil
Panduan Penggunaan
Instal semua dependensi yang diperlukan.

Hubungkan board PR2350-A dan iPhone XR ke komputer.

Gunakan alur PR2350-A / usbliter8 untuk memasuki mode pwned DFU pada iPhone XR.

Jalankan perintah berikut dari direktori proyek:

bash
chmod +x exploit.sh ssh_connect.sh tools/usbliter8ctl
./exploit.sh
Proses yang akan dilakukan skrip:

Membuat direktori logs/

Memeriksa keberadaan berkas payload yang dibutuhkan

Menjalankan payload/iBSS.raw

Menunggu perangkat memasuki mode Recovery

Mengirim citra firmware

Mengirim DeviceTree, ramdisk, trustcache, dan kernelcache

Mengatur boot-args

Menjalankan bootx

Memulai iproxy 2222 22 secara lokal

Mencoba masuk ke ramdisk melalui SSH sebagai pengguna root

Kata sandi SSH default:

text
alpine
Untuk menghubungkan kembali SSH setelah proses boot:

bash
./ssh_connect.sh
Koneksi manual:

bash
iproxy 2222 22
ssh root@localhost -p 2222
Penggantian Variabel Lingkungan
Jika alat tidak ditemukan di PATH default, Anda dapat menentukannya melalui variabel lingkungan:

bash
IRECOVERY=/path/to/irecovery \
PYTHON=/path/to/python3 \
USBLITER8CTL=tools/usbliter8ctl \
IPROXY=/path/to/iproxy \
SSHPASS=/path/to/sshpass \
./exploit.sh
Pemecahan Masalah
usbliter8ctl melaporkan perangkat tidak dalam status pwned: Ulangi langkah memasukkan perangkat ke mode pwned DFU menggunakan PR2350-A.

Waktu tunggu Recovery habis (timeout): Cabut dan sambungkan kembali kabel USB, lalu ulangi langkah pwned DFU.

SSH tidak dapat terhubung: Tunggu beberapa saat, lalu jalankan:

bash
./ssh_connect.sh
Port lokal 2222 sedang digunakan:

bash
pkill -f 'iproxy .*2222.*22'
Ucapan Terima Kasih
Proyek ini berdasarkan pada prdgmshift/usbliter8.

Terima kasih kepada penulis usbliter8 atas publikasi riset exploit SecureROM A12/A13 yang sangat berharga.

Pernyataan Sanggahan
Repositori ini disediakan untuk keperluan riset keamanan, riset pemulihan perangkat, dan pengujian pada perangkat yang Anda miliki atau memiliki izin untuk mengujinya. Gunakan dengan penuh tanggung jawab dan patuhi hukum serta peraturan yang berlaku di wilayah Anda.

