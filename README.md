# JARKOM-MODUL 2-A04-2022

- SAMUEL (5025201187)
- MOHAMAD KHOLID BUGHOWI (5025201253)
- GLORIA DYAH PRAMESTI (5025201033)

## Soal 1

## Soal 2

## Soal 3

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
3.

## Soal 12

## Soal 13

## Soal 14

## Soal 15

## Soal 16

## Soal 17
