# Jarkom-Modul-2-A04-2022

**Jarkom A04**<br>
**Anggota Kelompok**

| Nama                   | NRP        |
| ---------------------- | ---------- |
| Samuel                 | 5025201187 |
| Mohamad Kholid Bughowi | 5025201253 |
| Gloria Dyah Pramesti   | 5025201033 |

## Soal 1

Membuat topologi jaringan dimana WISE akan dijadikan sebagai DNS Master, Berlint akan dijadikan DNS Slave, dan Eden akan digunakan sebagai Web Server. Terdapat 2 Client yaitu SSS, dan Garden. Semua node terhubung pada router Ostania, sehingga dapat mengakses internet.

**Penjelasan**

1. Pertama buat topologi sesuai permintaan soal. Buat sebuah node yang terhubung dengan `NAT1`<br>
<br>![Topologi](https://user-images.githubusercontent.com/91613088/198669886-6c54261b-0b93-4bac-bb49-e2d1c8b62a39.PNG)<br>
<br>Lalu setting network masing-masing node dengan fitur `Edit network configuration`. Diganti dengan konfigurasi seperti berikut:<br>

- Ostania
```
auto eth0
iface eth0 inet dhcp

auto eth1
iface eth1 inet static
	address 10.1.1.1
	netmask 255.255.255.0

auto eth2
iface eth2 inet static
	address 10.1.2.1
	netmask 255.255.255.0

auto eth3
iface eth3 inet static
	address 10.1.3.1
	netmask 255.255.255.0
```
- Wise (DNS Master)
```
auto eth0
iface eth0 inet static
	address 10.1.3.2
	netmask 255.255.255.0
	gateway 10.1.3.1
```
- Berlint (DNS Slave)
```
auto eth0
iface eth0 inet static
	address 10.1.2.2
	netmask 255.255.255.0
	gateway 10.1.2.1
```
- Eden (Web Server)
```
auto eth0
iface eth0 inet static
	address 10.1.2.3
	netmask 255.255.255.0
	gateway 10.1.2.1
```
- SSS (Client)
```
auto eth0
iface eth0 inet static
	address 10.1.1.2
	netmask 255.255.255.0
	gateway 10.1.1.1
```
- Garden (Client)
```
auto eth0
iface eth0 inet static
	address 10.1.1.3
	netmask 255.255.255.0
	gateway 10.1.1.1
```
2. Restart semua node, lalu jalankan command pada Ostania:
`iptables -t nat -A POSTROUTING -o eth0 -j MASQUERADE -s 10.1.0.0/16`<br>

Lalu mengetikkan command berikut: `cat /etc/resolv.conf`dan menghasilkan hasil nameserver seperti berikut:<br>
<br>![1a](https://user-images.githubusercontent.com/91613088/198673231-1ef5a92e-436b-4367-81ad-e0d9316f5042.PNG)<br>

Agar semua node dapat terhubung ke internet, jalankan command berikut `echo nameserver 192.168.122.1 > /etc/resolv.conf`kecuali Ostania. Testing dengan ping ke google.com. Sebagai contoh pada Wise<br>
<br>![1b](https://user-images.githubusercontent.com/91613088/198674386-bea722c9-41d3-469d-be77-e23a26442faf.PNG)<br>

## Soal 2
Bantu Loid membuat website utama dengan akses wise.yyy.com dengan alias www.wise.yyy.com pada folder wise.

**Penjelasan**
1. Lakukan instalasi `bind9` pada Ostina dengan command seperti berikut:
```
apt-get update
apt-get install bind9 -y
```
2. Setelah instalasi selesai, buat domain wise.yyy.com. Dan lakukan command seperti berikut pada Wise `nano /etc/bind/named.conf.local`
3. Isi konfigurasi domain wise.a04.com dengan perintah berikut:
```
echo 'zone "wise.a04.com" {
    type master;
    file "/etc/bind/wise/wise.a04.com";
};' > /etc/bind/named.conf.local
```
4. Buat folder baru `mkdir -p /etc/bind/wise`
5. Copy file db.local ke dalam folder wise yang baru dibuat dan ubah namanya menjadi wise.a04.com `cp /etc/bind/db.local /etc/bind/wise/wise.a04.com`
6. Buka file dan edit konfigurasinya menjadi seperti berikut:<br>
![2a](https://user-images.githubusercontent.com/91613088/198836444-3082a633-490e-44e7-aad7-4eed9f75c2d6.PNG)<br>

7. Restart bind9 `service bind9 restart`
8. Lakukan testing untuk mengecek apakah www.wise.a04.com atau wise.a04.com dapat diakses:<br>
![2b](https://user-images.githubusercontent.com/91613088/198836567-869d3b59-be9e-426f-b9d5-50cd765e0cb0.PNG)<br>

## Soal 3
Buat subdomain eden.wise.yyy.com dengan alias www.eden.wise.yyy.com yang diatur DNS-nya di WISE dan mengarah ke Eden

**Penjelasan**
1. Buka file wise.a04.com dan edit konfigurasi seperti berikut:<br>
![3a](https://user-images.githubusercontent.com/91613088/198836716-d1173cce-ad96-4f13-896d-e91892ea1b12.PNG)<br>

2. Tambahkan konfigurasi berikut:
```
zone "eden.wise.a04.com" {
    type master;
    file "/etc/bind/wise/eden.wise.a04.com";
};
```
3. Lakukan testing untuk mengecek apakah www.eden.wise.a04.com atau eden.wise.a04.com dapat diakses:<br>
![3b](https://user-images.githubusercontent.com/91613088/198837033-bf494dce-4ac9-4ec1-b075-15a4a5587872.PNG)<br>

## Soal 4

## Soal 5

## Soal 6

## Soal 7

## Soal 8

## Soal 9

## Soal 10

## Soal 11

Pada soal ini diminta untuk hanya membuat directory listing pada folder `public` di webserver **www.eden.wise.a04.com**.

**Penjelasan**

1. Buka **Eden**
2. Ubah **/etc/apache2/sites-available/eden.wise.a04.com.conf**

```bash
nano /etc/apache2/sites-available/eden.wise.a04.com.conf
```

3. Sesuaikan sehingga isinya seperti berikut

```
<VirtualHost *:80>
    ServerName eden.wise.a04.com
    ServerAlias www.eden.wise.a04.com
    ServerAdmin webmaster@localhost
    DocumentRoot /var/www/eden.wise.a04.com

    <Directory /var/www/eden.wise.a04.com/public>
        Options +Indexes
    </Directory>

    <Directory /var/www/eden.wise.a04.com/public/css>
        Options -Indexes
    </Directory>

    <Directory /var/www/eden.wise.a04.com/public/images>
        Options -Indexes
    </Directory>

    <Directory /var/www/eden.wise.a04.com/public/js>
        Options -Indexes
    </Directory>

    ErrorLog ${APACHE_LOG_DIR}/error.log
    CustomLog ${APACHE_LOG_DIR}/access.log combined
</VirtualHost>
```

4. Restart apache2

```bash
service apache2 restart
```

**Testing**

1. Buka **SSS**
2. Ketik command `lynx www.eden.wise.a04.com/public`
   ![image](https://user-images.githubusercontent.com/49820990/198820375-7c6b1e2b-8022-45c6-8e01-33d345b7ab98.png)
3. Klik pada salah satu directory (`js/`)
   ![image](https://user-images.githubusercontent.com/49820990/198820407-a97f23ad-b15a-4520-b89e-51b1a9f716a5.png)

## Soal 12

Pada soal ini diminta untuk mengganti error kode pada webserver **www.eden.wise.a04.com** dengan file `404.html` yang berada dalam folder `error`.

**Penjelasan**

1. Buka **Eden**
2. Ubah **/etc/apache2/sites-available/eden.wise.a04.com.conf**
   ```bash
   nano /etc/apache2/sites-available/eden.wise.a04.com.conf
   ```
3. Sesuaikan isinya seperti berikut

   ```
   <VirtualHost *:80>
       ServerName eden.wise.a04.com
       ServerAlias www.eden.wise.a04.com
       ServerAdmin webmaster@localhost
       DocumentRoot /var/www/eden.wise.a04.com

       <Directory /var/www/eden.wise.a04.com/public>
           Options +Indexes
       </Directory>

       <Directory /var/www/eden.wise.a04.com/public/css>
           Options -Indexes
       </Directory>

       <Directory /var/www/eden.wise.a04.com/public/images>
           Options -Indexes
       </Directory>

       <Directory /var/www/eden.wise.a04.com/public/js>
           Options -Indexes
       </Directory>

       ErrorDocument 404 /error/404.html

       ErrorLog ${APACHE_LOG_DIR}/error.log
       CustomLog ${APACHE_LOG_DIR}/access.log combined
   </VirtualHost>
   ```

4. Restart apache2
   ```
   service apache2 restart
   ```

## Soal 13

Pada soal ini diminta untuk membuat konfigurasi virtual host yang dapat memberikan akses **www.eden.wise.a04.com/js** kepada **www.eden.wise.a04.com/public/js**.

**Penjelasan**

1. Buka **Eden**
2. Ubah **/etc/apache2/sites-available/eden.wise.a04.com.conf**

   ```bash
   nano /etc/apache2/sites-available/eden.wise.a04.com.conf
   ```

3. Sesuaikan isinya seperti berikut

   ```
   <VirtualHost *:80>
       ServerName eden.wise.a04.com
       ServerAlias www.eden.wise.a04.com
       ServerAdmin webmaster@localhost
       DocumentRoot /var/www/eden.wise.a04.com

       <Directory /var/www/eden.wise.a04.com/public>
           Options +Indexes
       </Directory>

       <Directory /var/www/eden.wise.a04.com/public/css>
           Options -Indexes
       </Directory>

       <Directory /var/www/eden.wise.a04.com/public/images>
           Options -Indexes
       </Directory>

       <Directory /var/www/eden.wise.a04.com/public/js>
           Options +Indexes
       </Directory>

       ErrorDocument 404 /error/404.html

       Alias "/js" "/var/www/eden.wise.a04.com/public/js"

       ErrorLog ${APACHE_LOG_DIR}/error.log
       CustomLog ${APACHE_LOG_DIR}/access.log combined
   </VirtualHost>
   ```

4. Restart apache2
   ```
   service apache2 restart
   ```

**Testing**

- Buka **SSS**
- Ketik command `lynx www.eden.wise.a04.com/js`
  ![image](https://user-images.githubusercontent.com/49820990/198821019-4a34e63b-66a2-4348-a330-a5c3b05997ec.png)

## Soal 14

Pada soal ini diminta untuk melakukan konfigurasi pada webserver **www.strix.operation.wise.a04.com** dengan port `15000` dan `15500` serta DocumentRoot terletak pada `/var/www/strix.operation.wise.a04.com`.

**Penjelasan**

1. Buka **Eden**
2. Copy **/etc/apache2/sites-available/000-default.conf** ke **/etc/apache2/sites-available/strix.operation.wise.a04.com.conf**

   ```bash
   cp /etc/apache2/sites-available/000-default.conf /etc/apache2/sites-available/strix.operation.wise.a04.com.conf
   ```

3. Ubah **/etc/apache2/sites-available/strix.operation.wise.a04.com.conf**

   ```bash
   nano /etc/apache2/sites-available/strix.operation.wise.a04.com.conf
   ```

4. Sesuaikan isinya seperti berikut

   ```
   <VirtualHost *:15000 *:15500>
       ServerName strix.operation.wise.a04.com
       ServerAlias www.strix.operation.wise.a04.com
       ServerAdmin webmaster@localhost
       DocumentRoot /var/www/strix.operation.wise.a04.com

       ErrorLog ${APACHE_LOG_DIR}/error.log
       CustomLog ${APACHE_LOG_DIR}/access.log combined
   </VirtualHost>
   ```

5. Ubah **/etc/apache2/ports.conf**

   ```bash
   nano /etc/apache2/ports.conf
   ```

6. Sesuaikan isinya seperti berikut

   ```
   # If you just change the port or add more ports here, you will likely also
   # have to change the VirtualHost statement in
   # /etc/apache2/sites-enabled/000-default.conf

   Listen 80
   Listen 15000
   Listen 15500

   <IfModule ssl_module>
           Listen 443
   </IfModule>

   <IfModule mod_gnutls.c>
           Listen 443
   </IfModule>

   # vim: syntax=apache ts=4 sw=4 sts=4 sr noet
   ```

7. Buat folder **strix.operation.wise.a04.com** di dalam **/var/www**

   ```bash
   mkdir /var/www/strix.operation.wise.a04.com
   ```

8. Unduh file website

   ```bash
   wget --no-check-certificate 'https://docs.google.com/uc?export=download&id=1bgd3B6VtDtVv2ouqyM8wLyZGzK5C9maT' -O strix.operation.wise.zip
   ```

9. Unzip file website

   ```bash
   unzip strix.operation.wise.zip
   ```

10. Pindahkan semua file website ke **/var/www/strix.operation.wise.a04.com**

    ```bash
    mv strix.operation.wise/* /var/www/strix.operation.wise.a04.com
    ```

11. Aktifkan konfigurasi website

    ```bash
    a2ensite strix.operation.wise.a04.com.conf
    ```

12. Restart apache

    ```bash
    service apache2 restart
    ```

**Testing**

- Buka **SSS**
- Ketik command `lynx 10.1.2.3:15000`
  ![image](https://user-images.githubusercontent.com/49820990/198821105-9a6f1d67-0687-43ce-b72f-8691e2a130c9.png)

## Soal 15

Pada soal ini diminta untuk memberikan autentikasi dengan username `Twilight` dan password `opStrix` pada webserver **www.strix.operation.wise.a04.com**.

**Penjelasan**

1. Buka **Eden**
2. Buat username dan password

   ```bash
   htpasswd -b -c /etc/apache2/.htpasswd Twilight opStrix
   ```

3. Ubah **/etc/apache2/sites-available/strix.operation.wise.a04.com.conf**
   ```bash
   nano /etc/apache2/sites-available/strix.operation.wise.a04.com.conf
   ```
4. Sesuaikan isinya seperti berikut
   ```bash
   <VirtualHost *:15000 *:15500>
       ServerName strix.operation.wise.a04.com
       ServerAlias www.strix.operation.wise.a04.com
       ServerAdmin webmaster@localhost
       DocumentRoot /var/www/strix.operation.wise.a04.com

        <Directory /var/www/strix.operation.wise.a04.com>
                Options +FollowSymLinks -Multiviews
                AllowOverride All
        </Directory>
       ErrorLog ${APACHE_LOG_DIR}/error.log
       CustomLog ${APACHE_LOG_DIR}/access.log combined
   </VirtualHost>
   ```
5. Buat file **.htaccess** di dalam **/var/www/strix.operation.wise.a04.com**

   ```bash
   nano /var/www/strix.operation.wise.a04.com/.htaccess
   ```

6. Sesuaikan isinya seperti berikut

   ```bash
   AuthType Basic
   AuthName "Restricted Content"
   AuthUserFile /etc/apache2/.htpasswd
   Require valid-user
   ```

7. Restart apache

   ```bash
   service apache2 restart
   ```

**Testing**

- Buka **SSS**
- Ketik command `lynx 10.1.2.3:15500`
- Masukkan username `Twilight`
  ![image](https://user-images.githubusercontent.com/49820990/198821257-3e9131f3-f6cc-4f0c-b98e-270d7a248af2.png)
- Masukkan password `opStrix`
  ![image](https://user-images.githubusercontent.com/49820990/198821268-6e8302ae-54e0-44c2-ace6-f56c2c026447.png)
- Tampil halaman web
  ![image](https://user-images.githubusercontent.com/49820990/198821274-968fad79-6262-48e1-804f-5ac42fd0a3a3.png)


## Soal 16

Pada soal ini diminta untuk mengarahkan IP Eden (**10.1.2.3**) ke **www.wise.a04.com**.

**Penjelasan**

1. Buka **Eden**
2. Buat file .htaccess pada /var/www/html

   ```bash
   nano /var/www/html/.htaccess
   ```

   ```
   RewriteEngine On
   RewriteBase /
   RewriteCond %{HTTP_HOST} ^10\.1\.2\.3$
   RewriteRule ^(.*)$ http://www.wise.a04.com [L,R=301]
   ```

3. Ubah /etc/apache2/sites-available/000-default.conf

   ```bash
   nano /etc/apache2/sites-available/000-default.conf
   ```

4. Sesuaikan isinya seperti berikut

   ```bash
   <VirtualHost *:80>
       ServerAdmin webmaster@localhost
       DocumentRoot /var/www/html

       <Directory /var/www/html>
           Options +FollowSymLinks -Multiviews
           AllowOverride All
       </Directory>

       ErrorLog ${APACHE_LOG_DIR}/error.log
       CustomLog ${APACHE_LOG_DIR}/access.log combined
   </VirtualHost>
   ```

5. Restart apache

   ```bash
   service apache2 restart
   ```

**Testing**

- Buka **SSS**
- Ketik command `lynx 10.1.2.3`
  ![image](https://user-images.githubusercontent.com/49820990/198821488-0e0b65f1-af97-46f6-a3d9-15636a94182a.png)

## Soal 17

Pada soal ini diminta untuk mengarahkan request gambar yang memiliki substring `eden`ke `eden.png` pada webserver **www.eden.wise.a04.com**.

**Penjelasan**

1. Buka **Eden**
2. Buat file **.htaccess** pada **/var/www/eden.wise.a04.com**

   ```bash
   nano /var/www/eden.wise.a04.com/.htaccess
   ```

3. Sesuaikan isinya seperti berikut

   ```bash
   RewriteEngine On
   RewriteRule ^(.*)eden(.*)$ http://www.eden.wise.a04.com/public/images/eden.png [L,R]
   ```

4. Ubah **/etc/apache2/sites-available/eden.wise.a04.com.conf**

   ```bash
   nano /etc/apache2/sites-available/eden.wise.a04.com.conf
   ```
5. Sesuaikan isinya seperti ini

   ```
   <VirtualHost *:80>
       ServerName eden.wise.a04.com
       ServerAlias www.eden.wise.a04.com
       ServerAdmin webmaster@localhost
       DocumentRoot /var/www/eden.wise.a04.com

       <Directory /var/www/eden.wise.a04.com>
           Options +FollowSymLinks -Multiviews
           AllowOverride All
       </Directory>

       <Directory /var/www/eden.wise.a04.com/public>
           Options +Indexes
       </Directory>

       <Directory /var/www/eden.wise.a04.com/public/css>
           Options -Indexes
       </Directory>

       <Directory /var/www/eden.wise.a04.com/public/images>
           Options -Indexes
       </Directory>

       <Directory /var/www/eden.wise.a04.com/public/js>
           Options -Indexes
       </Directory>

       ErrorDocument 404 /error/404.html

       Alias "/js" "/var/www/eden.wise.a04.com/public/js"

       ErrorLog ${APACHE_LOG_DIR}/error.log
       CustomLog ${APACHE_LOG_DIR}/access.log combined
   </VirtualHost>
   ```

5. Restart apache

   ```bash
   service apache2 restart
   ```

**Testing**

- Buka **SSS**
- Ketik command `lynx www.eden.wise.a04.com/aeden12.png` (dilakukan sebelum nomor 11)
  ![image](https://user-images.githubusercontent.com/49820990/198821838-436f1092-a5f5-4101-8854-11d5ab5606dc.png)
