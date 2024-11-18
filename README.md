# Panduan Instalasi MariaDB Server di CentOS 9

MariaDB adalah salah satu database server open-source yang populer dan digunakan secara luas. Panduan ini akan membantu Anda menginstal MariaDB Server di sistem operasi **CentOS 9** dengan langkah-langkah sederhana.


## Persyaratan
1. **Sistem Operasi**: CentOS 9.
2. **Hak Akses**: Akses root atau pengguna dengan izin `sudo`.
3. **Koneksi Internet** untuk mengunduh paket.


## Langkah-langkah Instalasi

### 1. Perbarui Repositori Sistem
Sebelum menginstal paket apa pun, pastikan repositori Anda diperbarui:
```bash
sudo dnf update -y
```


### 2. Instal MariaDB Server
Gunakan perintah berikut untuk menginstal MariaDB Server:
```bash
sudo dnf install -y mariadb-server
```


### 3. Aktifkan dan Mulai Layanan MariaDB
Setelah instalasi selesai, aktifkan dan mulai layanan MariaDB:
```bash
sudo systemctl enable mariadb
sudo systemctl start mariadb
```


### 4. Verifikasi Status Layanan
Pastikan MariaDB berjalan dengan baik:
```bash
sudo systemctl status mariadb
```
Jika MariaDB berjalan, Anda akan melihat status **`active (running)`**.


### 5. Amankan Instalasi MariaDB
Amankan instalasi MariaDB menggunakan skrip bawaan:
```bash
sudo mysql_secure_installation
```

**Panduan selama skrip berjalan:**
- **Setel Password Root:** Pilih `Y` dan masukkan password untuk pengguna root MariaDB.
- **Hapus Anonymous Users:** Ketik `Y`.
- **Disallow Root Login Remotely:** Ketik `Y`.
- **Hapus Database Test:** Ketik `Y`.
- **Reload Privilege Tables:** Ketik `Y`.

---

### 6. Uji Koneksi ke MariaDB
Masuk ke MariaDB untuk memverifikasi instalasi:
```bash
sudo mysql -u root -p
```

Jika berhasil, Anda akan masuk ke shell MariaDB:
```sql
Welcome to the MariaDB monitor.  Commands end with ; or \g.
MariaDB [(none)]>
```

Ketik `exit` untuk keluar dari shell MariaDB.

---

# Panduan Instalasi phpMyAdmin di CentOS 9

Berikut adalah langkah-langkah untuk menambahkan instalasi **phpMyAdmin** pada MariaDB yang sudah diinstal di **CentOS 9**:  

### Langkah-langkah Instalasi
1. Tambahkan repositori EPEL dan instal phpMyAdmin:
   ```bash
   sudo dnf install -y epel-release
   sudo dnf install -y phpmyadmin
   ```
2. Ubah akses phpMyAdmin agar dapat diakses dari jaringan (opsional):
   ```bash
   sudo nano /etc/httpd/conf.d/phpMyAdmin.conf
   ```
   Ubah `Require local` menjadi `Require all granted`, lalu simpan.

   ```bash
   <Directory "/var/www/html/phpmyadmin">
       ...
       Require all granted
       ...
   </Directory>
   ```
4. Aktifkan dan mulai layanan Apache:
   ```bash
   sudo systemctl enable httpd
   sudo systemctl start httpd
   ```
5. Restart Apache:
   ```bash
   sudo systemctl restart httpd
   ```
6. Akses phpMyAdmin di browser:
   ```
   http://<IP-Server-Anda>/phpmyadmin
   ```
   
## Troubleshooting

1. **phpMyAdmin tidak bisa diakses:**
   - Periksa status Apache:
     ```bash
     sudo systemctl status httpd
     ```
   - Pastikan firewall mengizinkan HTTP/HTTPS:
     ```bash
     sudo firewall-cmd --add-service=http --permanent
     sudo firewall-cmd --add-service=https --permanent
     sudo firewall-cmd --reload
     ```

2. **Tidak dapat login ke phpMyAdmin:**
   - Pastikan Anda menggunakan username dan password MariaDB yang benar.

---
