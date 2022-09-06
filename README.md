## Install Apache 2
```
sudo apt update
```

```
sudo apt install apache2
```

## Install UFW
```
sudo apt-get install ufw
```
<br/>
<p>Setelah proses instalasi selesai, berikan izin pada traffic HTTP dan HTTPS melewati firewall menggunakan opsi allow dengan perintah berikut.</p>

```
sudo ufw allow http
```
```
sudo ufw allow https
```
<br/>
<p>Selanjutnya, restart Apache agar perubahan yang sudah dilakukan dapat secepatnya diterapkan menggunakan perintah ini.</p>

```
sudo service apache2 restart
```

## Install MariaDB
```
sudo apt install mariadb-server
```
<br/>
<p>Install Skrip Keamanan MariaDB</p>

```
sudo mysql_secure_installation
```

- Masukkan pass root
- Ganti pass
- Remove anonymous users? Y
- Dissalow root login remotely? Y
- Remove test database and access to it? Y
- Reload privilege tables now? Y

<br/>
<p>Membuat User Baru di MariaDB </p>

```
sudo mariadb
```

```
GRANT ALL ON *.* TO 'admin'@'localhost' IDENTIFIED BY 'password' WITH GRANT OPTION;
```

```
FLUSH PRIVILEGES;
```

```
exit;
```
<br/>
<p>Test Login :</p>

```
mariadb -u admin -p
```

## Install PHP 7.4 on Debian 10 / Debian 9

```
sudo apt update
```
```
sudo apt upgrade -y && sudo reboot
```

<br/>
<p>Add SURY PHP PPA repository</p>

```
sudo apt -y install lsb-release apt-transport-https ca-certificates 
```
```
sudo wget -O /etc/apt/trusted.gpg.d/php.gpg https://packages.sury.org/php/apt.gpg
```
```
echo "deb https://packages.sury.org/php/ $(lsb_release -sc) main" | sudo tee /etc/apt/sources.list.d/php.list
```
```
sudo apt update
```

<br/>
<p>Install PHP 7.4</p>

```
sudo apt -y install php7.4
```

<br/>
<p>Install PHP 7.4 Package</p>

```
sudo apt install php7.4-common php7.4-mysql php7.4-xml php7.4-xmlrpc php7.4-curl php7.4-gd php7.4-imagick php7.4-cli php7.4-dev php7.4-imap php7.4-mbstring php7.4-opcache php7.4-soap php7.4-zip php7.4-intl -y
```
```
sudo service apache2 restart
```

## Install Manual phpMyAdmin
phpMyAdmin di install setelah menginstall semua paket-paket PHP 7.4
<br/>

```
sudo apt update
```

<br/>
<p>Sesuaikan link dengan versi terbaru yg ada disini : https://www.phpmyadmin.net/downloads/<br/>
* Copy link yang extensinya .tar.gz</p>

<br/>
<p>Download phpMyAdmin</p>

```
wget https://files.phpmyadmin.net/phpMyAdmin/5.2.0/phpMyAdmin-5.2.0-all-languages.tar.gz
```

<br/>
<p>Ekstrak file</p>

```
tar xvf phpMyAdmin-5.2.0-all-languages.tar.gz
```

<br/>
<p>Pindahkan file ke /usr/share/phpmyadmin</p>

```
sudo mv phpMyAdmin-5.2.0-all-languages/ /usr/share/phpmyadmin
```

## Konfigurasi phpMyAdmin secara manual

<br/>
<p>Buat direktori baru tempat phpMyAdmin akan menyimpan file-file sementaranya</p>

```
sudo mkdir -p /var/lib/phpmyadmin/tmp
```

<br/>
<p>Set www-data</p>

```
sudo chown -R www-data:www-data /var/lib/phpmyadmin
```

<br/>
<p>Duplikat config.sample.inc.php menjadi config.inc.php</p>

```
sudo cp /usr/share/phpmyadmin/config.sample.inc.php /usr/share/phpmyadmin/config.inc.php
```

<br/>
<p>Buka file config.sample.inc</p>

```
sudo nano /usr/share/phpmyadmin/config.inc.php
```

<br/>
<p>Masukkan 32 string acak ke blowfish_secret<br/>String acak bisa dibuat di www.pwgen.io</p>

```
$cfg['blowfish_secret'] = 'masukkan 32 string acak disini';
```

<br/>
<p>Hilangkan comment (//) pada config dibawah ini, dan ubah password PMA</p>

```
$cfg['Servers'][$i]['controluser'] = 'pma';
$cfg['Servers'][$i]['controlpass'] = 'password';
```

<br/>
<p>Hilangkan comment (//) semua baris dibawah ini</p>

```
/* Storage database and tables */
$cfg['Servers'][$i]['pmadb'] = 'phpmyadmin';
$cfg['Servers'][$i]['bookmarktable'] = 'pma__bookmark';
$cfg['Servers'][$i]['relation'] = 'pma__relation';
$cfg['Servers'][$i]['table_info'] = 'pma__table_info';
$cfg['Servers'][$i]['table_coords'] = 'pma__table_coords';
$cfg['Servers'][$i]['pdf_pages'] = 'pma__pdf_pages';
$cfg['Servers'][$i]['column_info'] = 'pma__column_info';
$cfg['Servers'][$i]['history'] = 'pma__history';
$cfg['Servers'][$i]['table_uiprefs'] = 'pma__table_uiprefs';
$cfg['Servers'][$i]['tracking'] = 'pma__tracking';
$cfg['Servers'][$i]['userconfig'] = 'pma__userconfig';
$cfg['Servers'][$i]['recent'] = 'pma__recent';
$cfg['Servers'][$i]['favorite'] = 'pma__favorite';
$cfg['Servers'][$i]['users'] = 'pma__users';
$cfg['Servers'][$i]['usergroups'] = 'pma__usergroups';
$cfg['Servers'][$i]['navigationhiding'] = 'pma__navigationhiding';
$cfg['Servers'][$i]['savedsearches'] = 'pma__savedsearches';
$cfg['Servers'][$i]['central_columns'] = 'pma__central_columns';
$cfg['Servers'][$i]['designer_settings'] = 'pma__designer_settings';
$cfg['Servers'][$i]['export_templates'] = 'pma__export_templates';
```


<br/>
<p>Tambahkan baris dibawah ini, untuk directory sementara phpMyAdmin</p>

```
$cfg['TempDir'] = '/var/lib/phpmyadmin/tmp';
```

<br/>
<p>Jalankan perintah berikut untuk menggunakan create_tables.sqlfile untuk membuat database dan tabel penyimpanan konfigurasi:</p>

```
sudo mariadb < /usr/share/phpmyadmin/sql/create_tables.sql
```

<br/>
<p>Setelah itu, Anda harus membuat pengguna pma administratif. Buka prompt MariaDB:</p>

```
sudo mariadb
```

<br/>
<p>Buat pengguna pma dan berikan izin yang sesuai, password diganti sesuai dengan yg di config :</p>

```
GRANT SELECT, INSERT, UPDATE, DELETE ON phpmyadmin.* TO 'pma'@'localhost' IDENTIFIED BY 'password';
```
```
exit;
```

## Konfigurasi Apache untuk phpMyAdmin

<br/>
<p>Buat phpmyadmin.conf</p>

```
sudo nano /etc/apache2/conf-available/phpmyadmin.conf
```


<br/>
<p>Tambahkan konten berikut ke file</p>

```
# phpMyAdmin default Apache configuration

Alias /phpmyadmin /usr/share/phpmyadmin

<Directory /usr/share/phpmyadmin>
    Options SymLinksIfOwnerMatch
    DirectoryIndex index.php

    <IfModule mod_php5.c>
        <IfModule mod_mime.c>
            AddType application/x-httpd-php .php
        </IfModule>
        <FilesMatch ".+\.php$">
            SetHandler application/x-httpd-php
        </FilesMatch>

        php_value include_path .
        php_admin_value upload_tmp_dir /var/lib/phpmyadmin/tmp
        php_admin_value open_basedir /usr/share/phpmyadmin/:/etc/phpmyadmin/:/var/lib/phpmyadmin/:/usr/share/php/php-gettext/:/usr/share/php/php-php-gettext/:/usr/share/javascript/:/usr/share/php/tcpdf/:/usr/share/doc/phpmyadmin/:/usr/share/php/phpseclib/
        php_admin_value mbstring.func_overload 0
    </IfModule>
    <IfModule mod_php.c>
        <IfModule mod_mime.c>
            AddType application/x-httpd-php .php
        </IfModule>
        <FilesMatch ".+\.php$">
            SetHandler application/x-httpd-php
        </FilesMatch>

        php_value include_path .
        php_admin_value upload_tmp_dir /var/lib/phpmyadmin/tmp
        php_admin_value open_basedir /usr/share/phpmyadmin/:/etc/phpmyadmin/:/var/lib/phpmyadmin/:/usr/share/php/php-gettext/:/usr/share/php/php-php-gettext/:/usr/share/javascript/:/usr/share/php/tcpdf/:/usr/share/doc/phpmyadmin/:/usr/share/php/phpseclib/
        php_admin_value mbstring.func_overload 0
    </IfModule>

</Directory>

# Authorize for setup
<Directory /usr/share/phpmyadmin/setup>
    <IfModule mod_authz_core.c>
        <IfModule mod_authn_file.c>
            AuthType Basic
            AuthName "phpMyAdmin Setup"
            AuthUserFile /etc/phpmyadmin/htpasswd.setup
        </IfModule>
        Require valid-user
    </IfModule>
</Directory>

# Disallow web access to directories that don't need it
<Directory /usr/share/phpmyadmin/templates>
    Require all denied
</Directory>
<Directory /usr/share/phpmyadmin/libraries>
    Require all denied
</Directory>
<Directory /usr/share/phpmyadmin/setup/lib>
    Require all denied
</Directory>
```


<br/>
<p>Simpan dan tutup file, lalu aktifkan dengan mengetik:</p>

```
sudo a2enconf phpmyadmin.conf
```
```
sudo systemctl reload apache2
```

<p>phpMyAdmin sudah bisa diakses di browser</p>


## Cara mengamankan phpMyAdmin

<p>Karena keberadaannya di mana-mana, phpMyAdmin adalah target populer bagi penyerang, dan Anda harus berhati-hati untuk mencegah akses yang tidak sah. Salah satu cara termudah untuk melakukannya adalah dengan menempatkan gateway di depan seluruh aplikasi dengan menggunakan .htaccessfungsi otentikasi dan otorisasi bawaan Apache.</p>

<p>Untuk melakukan ini, Anda harus terlebih dahulu mengaktifkan penggunaan .htaccesspenggantian file dengan mengedit file konfigurasi Apache Anda.</p>

<p>Edit file tertaut yang telah ditempatkan di direktori konfigurasi Apache Anda:</p>

```
sudo nano /etc/apache2/conf-available/phpmyadmin.conf

```


<br/>
<p>Tambahkan AllowOverride didalam file config, seperti ini:

</p>

```
<Directory /usr/share/phpmyadmin>
    Options FollowSymLinks
    DirectoryIndex index.php
    AllowOverride All

    <IfModule mod_php5.c>
    . . .
```
<p>Simpan file dan restart Apache</p>

```
sudo systemctl restart apache2
```

<br/>
<p>Buat file .htaccess pada directory phpMyAdmin</p>

```
sudo nano /usr/share/phpmyadmin/.htaccess

```
<p>Masukkan konten berikut kedalam file .htaccess</p>

```
AuthType Basic
AuthName "Restricted Files"
AuthUserFile /usr/share/phpmyadmin/.htpasswd
Require valid-user
```


<br/>
<p>Lokasi yang Anda pilih untuk file kata sandi Anda adalah /usr/share/phpmyadmin/.htpasswd. Anda sekarang dapat membuat file ini dan memberikannya kepada pengguna awal dengan htpasswdutilitas:</p>

```
sudo htpasswd -c /usr/share/phpmyadmin/.htpasswd username
```
