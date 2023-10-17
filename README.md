# Jarkom-Modul-2-IT13-2023


|       Nama      | NRP        | 
| -----------     | :---------: 
| Della Setyowati | 5027211044 | 
| Wisnu Adjie Saka| 5027211051 | 

### NO 1. 
Yudhistira akan digunakan sebagai DNS Master, Werkudara sebagai DNS Slave, Arjuna merupakan Load Balancer yang terdiri dari beberapa Web Server yaitu Prabakusuma, Abimanyu, dan Wisanggeni. 

### Jawaban No.1
Kami membuat topologi bagian 6

![untitled](https://cdn.discordapp.com/attachments/901344920361656355/1163324715306323990/image.png?ex=653f2991&is=652cb491&hm=d88670c985e3596b6ca8c79f644d51559129e67de4d35ff2f7a1eac9481b8bf4&)

Langkah berikutnya kami akan membuat konfigurasi topologi pada setiap node yang ada : 

- Pandudewanata sebagai router
```
auto eth0
iface eth0 inet dhcp

auto eth1
iface eth1 inet static
	address 10.70.1.1
	netmask 255.255.255.0

auto eth2
iface eth2 inet static
	address 10.70.2.1
	netmask 255.255.255.0

auto eth3
iface eth3 inet static
	address 10.70.3.1
	netmask 255.255.255.0

```
- Nakula 
```
auto eth0
iface eth0 inet static
	address 10.70.1.2
	netmask 255.255.255.0
	gateway 10.70.1.1
. 
```
- Sadewa
```
auto eth0
iface eth0 inet static
	address 10.70.1.3
	netmask 255.255.255.0
	gateway 10.70.1.1

. 
```

- Yudhistira
```
auto eth0
iface eth0 inet static
	address 10.70.1.4
	netmask 255.255.255.0
	gateway 10.70.1.1
. 
```

- Werkudara
```
auto eth0
iface eth0 inet static
	address 10.70.1.5
	netmask 255.255.255.0
	gateway 10.70.1.1
. 
```

- Arjuna
```
auto eth0
iface eth0 inet static
	address 10.70.2.2
	netmask 255.255.255.0
	gateway 10.70.2.1
. 
```
- Prabakusuma
```
auto eth0
iface eth0 inet static
	address 10.70.2.2
	netmask 255.255.255.0
	gateway 10.70.3.1
. 
```
- Abimanyu
```
auto eth0
iface eth0 inet static
	address 10.70.2.3
	netmask 255.255.255.0
	gateway 10.70.3.1
. 
```
- Wisanggeni
```
auto eth0
iface eth0 inet static
	address 10.70.2.4
	netmask 255.255.255.0
	gateway 10.70.3.1
. 
```


### NO 2.
Buatlah website utama pada node arjuna dengan akses ke arjuna.yyy.com dengan alias www.arjuna.yyy.com dengan yyy merupakan kode kelompok.

### Jawaban No 2

Tambahkan telnet pada node Yudhistira dan jalankan perintah berikut
```
apt update
apt install dnsutils -yecho 
```

Mengedit konfigurasi lokal pada Yudhistira sebagai server di file /etc/bind/named.conf.local untuk menambahkan zone baru.

```
nano /etc/bind/named.conf.local
```
```
echo 'zone "arjuna.IT13.com" {
        type master;
        notify yes;
        file "/etc/bind/arjuna/arjuna.IT13.com";
};
```
Menambahkan domain di file /etc/bind/arjuna/arjuna.IT13.com

```
nano /etc/bind/arjuna/arjuna.IT13.com
```
```
; BIND data file for local loopback interface
;
$TTL    604800
@       IN      SOA     arjuna.IT13.com. root.arjuna.IT13.com. (
                              2         ; Serial
                         604800         ; Refresh
                          86400         ; Retry
                        2419200         ; Expire
                         604800 )       ; Negative Cache TTL
;
@       IN      NS      arjuna.IT13.com.
@       IN      A       10.70.2.2       ; IP Arjuna
@       IN      AAAA    ::1
www     IN      CNAME   arjuna.IT13.com.' > /etc/bind/arjuna/arjuna.IT13.com
```

``` Restart bind9 pada Yudhistira```

Testing pada client Nakula

Menambahkan nameserver pada konfigurasi Nakula sebagai client. 

Jalankan perintah berikut satu persatu

```echo "nameserver 10.70.1.4" > /etc/resolv.conf```

 Jalankan command berikut pada Nakula
 ```
 ping arjuna.IT13.com atau www.arjuna.IT13.com
 ```
 Berikut merupakan hasil dokumentasinya saat testing berjalan
![untitled](https://cdn.discordapp.com/attachments/901344920361656355/1163734497397837834/image.png?ex=6540a735&is=652e3235&hm=6d060469a367496488d12089093f366bab509892012d6de80d7f885ef218dc91&)


### No 3
Dengan cara yang sama seperti soal nomor 2, buatlah website utama dengan akses ke abimanyu.yyy.com dan alias www.abimanyu.yyy.com.

### Jawaban No 3
Tambahkan telnet pada node Yudhistira dan jalankan perintah berikut
```
apt update
apt install dnsutils -yecho 
```

Mengedit konfigurasi lokal pada Yudhistira sebagai server di file /etc/bind/named.conf.local untuk menambahkan zone baru.

```
nano /etc/bind/named.conf.local
```
```
echo 'zone "abimanyu.IT13.com" {
        type master;
        notify yes;
        file "/etc/bind/abimanyu/abimanyu.IT13.com";
};
```
Menambahkan domain di file /etc/bind/abimanyu/abimanyu.IT13.com

```
nano /etc/bind/abimanyu/abimanyu.IT13.com
```
```
; BIND data file for local loopback interface
;
$TTL    604800
@       IN      SOA     abimanyu.IT13.com. root.abimanyu.IT13.com. (
                              2         ; Serial
                         604800         ; Refresh
                          86400         ; Retry
                        2419200         ; Expire
                         604800 )       ; Negative Cache TTL
;
@       IN      NS      abimanyu.IT13.com.
@       IN      A       10.70.3.3       ; IP Abimanyu
@       IN      AAAA    ::1
www     IN      CNAME   abimanyu.IT13.com.

```

``` Restart bind9 pada Yudhistira```

Testing pada client Nakula

Menambahkan nameserver pada konfigurasi Nakula sebagai client. 

Jalankan perintah berikut satu persatu


```"nameserver 10.70.1.4" > /etc/resolv.conf```

 Jalankan command berikut pada Nakula 

 ```ping abimanyu.IT13.com atau www.abimanyu.IT13.com```
 
 Berikut merupakan hasil dokumentasinya saat testing berjalan

![untitled](https://cdn.discordapp.com/attachments/901344920361656355/1163734564326359171/image.png?ex=6540a745&is=652e3245&hm=176c064f3eb34ab491c283c65c24568b0a4f7655af11d2d09b0d16b8132479d2&)


### No 4
Kemudian, karena terdapat beberapa web yang harus di-deploy, buatlah subdomain parikesit.abimanyu.yyy.com yang diatur DNS-nya di Yudhistira dan mengarah ke Abimanyu.

### Jawaban No 4

Modifikasi file /etc/bind/abimanyu/abimanyu.IT13.com
```
;
; BIND data file for local loopback interface
;
$TTL    604800
@       IN      SOA     abimanyu.IT13.com. abimanyu.IT13.com. (
                              2         ; Serial
                         604800         ; Refresh
                          86400         ; Retry
                        2419200         ; Expire
                         604800 )       ; Negative Cache TTL
;
@       IN      NS      abimanyu.IT13.com.
@       IN      A       10.70.3.3       ; IP Abimanyu
@       IN      AAAA    ::1
www     IN      CNAME   abimanyu.IT13.com.
parikesit       IN A    10.70.3.3   ; IP
www.parikesit   IN CNAME parikesit.abimanyu.IT13.com

```
Kemudian, restart bind9 dengan command pada Yudhistira

``` service bind9 restart```

Lalu lakukan ping pada Nakula (client)

![untitled](https://cdn.discordapp.com/attachments/901344920361656355/1163734651198779462/image.png?ex=6540a759&is=652e3259&hm=40b56d4c98ebbe85da7c296c07b6bb3c9921ebae773c234dfb40196f26e879ac&)

### No 5
Buat juga reverse domain untuk domain utama. (Abimanyu saja yang direverse)

### Jawaban No 5

Menambahkan konfigurasi dibawah pada /etc/bind/named.conf.local
```
zone "3.70.10.in-addr.arpa" {
    type master;
    file "/etc/bind/abimanyu/3.70.10.in-addr.arpa";
};
```
Lalu, tambahkan reversed di file /etc/bind/abimanyu/3.70.10.in-addr.arpa.
```
; BIND data file for local loopback interface
;
$TTL    604800
@       IN      SOA     abimanyu.IT13.com. root.abimanyu.IT13.com. (
                              2         ; Serial
                         604800         ; Refresh
                          86400         ; Retry
                        2419200         ; Expire
                         604800 )       ; Negative Cache TTL
;
3.70.10.in-addr.arpa.   IN      NS      abimanyu.IT13.com.
3                       IN      PTR     abimanyu.IT13.com.
```
Kemudian, restart bind dengan command

```service bind9 restart```

Pengecekan dapat dilakukan dengan menjalankan perintah

```
host -t PTR 10.70.3.3

```
![untitled](https://cdn.discordapp.com/attachments/901344920361656355/1163317195951059045/image.png?ex=653f2290&is=652cad90&hm=ddcee9629fcae72da7598e27a3400b983f05e648e6379a7b8475e3f6233a9696&)

### No 6
Agar dapat tetap dihubungi ketika DNS Server Yudhistira bermasalah, buat juga Werkudara sebagai DNS Slave untuk domain utama.

### Jawaban No 6
Modifikasi konfigurasi pada /etc/bind/named.conf.local pada Yudhistira

```
zone "arjuna.IT13.com" {
        type master;
        notify yes;
        also-notify { 10.70.1.5; }; //IP werkudara
        allow-transfer { 10.70.1.5; }; //IP wekudara
        file "/etc/bind/arjuna/arjuna.IT13.com";
};

zone "abimanyu.IT13.com" {
        type master;
        notify yes;
        also-notify { 10.70.1.5; }; //IP werkudara
        allow-transfer { 10.70.1.5; }; //IP wekudara
        file "/etc/bind/abimanyu/abimanyu.IT13.com";
}; 
```

Kemudian lakukan konfigurasi pada Werkudara, pada file /etc/bind/named.conf.local

```
zone "arjuna.IT13.com" {
    type slave;
    masters { 10.70.1.4; }; // Masukan IP yudhistira
    file "/etc/bind/arjuna/slave.arjuna.IT13.com.zone";
};

zone "abimanyu.IT13.com" {
    type slave;
    masters { 10.70.1.4; }; // Masukan IP yudhistira
    file "/etc/bind/abimanyu/slave.abimanyu.IT13.com.zone";
};
```
Membuat folder zones dan memasukkan konfigurasi ke dalam file /etc/bind/abimanyu/slave.abimanyu.IT13.com.zone


```
;
; BIND data file for local loopback interface
;
$TTL    604800
@       IN      SOA     abimanyu.IT13.com. abimanyu.IT13.com. (
                              2         ; Serial
                         604800         ; Refresh
                          86400         ; Retry
                        2419200         ; Expire
                         604800 )       ; Negative Cache TTL
;
@       IN      NS      abimanyu.IT13.com.
@       IN      A       10.70.3.3       ; IP Abimanyu
@       IN      AAAA    ::1
www     IN      CNAME   abimanyu.IT13.com.
parikesit       IN A    10.70.3.3   ; IP
www.parikesit   IN CNAME parikesit.abimanyu.IT13.com
```
Memasukkan konfigurasi ke dalam file /etc/bind/arjuna/slave.arjuna.IT13.com.zone

```
;
; BIND data file for local loopback interface
;
$TTL    604800
@       IN      SOA     arjuna.IT13.com. root.arjuna.IT13.com. (
                              2         ; Serial
                         604800         ; Refresh
                          86400         ; Retry
                        2419200         ; Expire
                         604800 )       ; Negative Cache TTL
;
@       IN      NS      arjuna.IT13.com.
@       IN      A       10.70.2.2       ; IP Arjuna
@       IN      AAAA    ::1
www     IN      CNAME   arjuna.IT13.com.
```
Kemudian, restart bind dengan command

```service bind9 restart```

![untitled](https://cdn.discordapp.com/attachments/901344920361656355/1163316071097454752/image.png?ex=653f2184&is=652cac84&hm=d1713f57cbe9d3eea7d618c4c30f65119e3a444d612295eb0c4b91992984860f&)

### No 7
Seperti yang kita tahu karena banyak sekali informasi yang harus diterima, buatlah subdomain khusus untuk perang yaitu baratayuda.abimanyu.yyy.com dengan alias www.baratayuda.abimanyu.yyy.com yang didelegasikan dari Yudhistira ke Werkudara dengan IP menuju ke Abimanyu dalam folder Baratayuda.

### Jawaban No 7
Pada yudhistira mengubah dan menambahkan pada file /etc/bind/abimanyu/abimanyu.IT13.com.

```
;
; BIND data file for local loopback interface
;
$TTL    604800
@       IN      SOA     abimanyu.IT13.com. root.abimanyu.IT13.com. (
                              2         ; Serial
                         604800         ; Refresh
                          86400         ; Retry
                        2419200         ; Expire
                         604800 )       ; Negative Cache TTL
;
@       IN      NS      abimanyu.IT13.com.
@       IN      A       10.70.3.3       ; IP Abimanyu
www     IN      CNAME   abimanyu.IT13.com.
parikesit       IN A    10.70.3.3   ; IP
;www.parikesit   IN CNAME parikesit.abimanyu.IT13.com.
ns1     IN      A       10.70.1.5       ; IP warkudara
baratayuda      IN      NS      ns1
```
Pada werkudara menambahkan konfigurasi pada file /etc/bind/named.conf.local
```
zone "abimanyu.IT13.com" {
    type slave;
    masters { 10.70.1.4; }; // Masukan IP yudhistira
    file "/etc/bind/abimanyu/slave.abimanyu.IT13.com.zone";
};
``` 
Lalu menambahkan konfigurasi pada file /etc/bind/abimanyu/baratayuda.abimanyu.IT13.com.zone

```
; BIND data file for local loopback interface
;
$TTL    604800
@       IN      SOA     baratayuda.abimanyu.IT13.com. root.baratayuda.abimanyu.IT13.com. (
                              2         ; Serial
                         604800         ; Refresh
                          86400         ; Retry
                        2419200         ; Expire
                         604800 )       ; Negative Cache TTL
;
@       IN      NS      baratayuda.abimanyu.IT13.com.
; DNS Records
@           IN      A       10.70.3.3 ; IP Utama
www         IN      CNAME   baratayuda.abimanyu.IT13.com.
```
Lalu restart dengan  command
```
service bind9 restart
```
berikut merupakan dokumentasinya
![untitled](https://cdn.discordapp.com/attachments/901344920361656355/1163734792110616636/image.png?ex=6540a77b&is=652e327b&hm=a58ea27966b2548942219ac7dfbb58c01af981f77021d605d3840fbcb8aca66b&)

### No 8
Untuk informasi yang lebih spesifik mengenai Ranjapan Baratayuda, buatlah subdomain melalui Werkudara dengan akses rjp.baratayuda.abimanyu.yyy.com dengan alias www.rjp.baratayuda.abimanyu.yyy.com yang mengarah ke Abimanyu.

### Jawaban No 8
Menambahkan konfigurasi pada file  /etc/bind/named.conf.local
```
zone "rjp.baratayuda.abimanyu.IT13.com" {
    type master;
    file "/etc/bind/abimanyu/baratayuda.abimanyu.IT13.com";
};
```
Kemudian membuat konfigurasi /etc/bind/abimanyu/rjp.baratayuda.abimanyu.IT13.com.zone pada Werkudara

```
;
; BIND data file for local loopback interface
;
$TTL    604800
@       IN      SOA     rjp.baratayuda.abimanyu.IT13.com. root..rjp.baratayuda.abimanyu.IT13.com. (
                              2         ; Serial
                         604800         ; Refresh
                          86400         ; Retry
                        2419200         ; Expire
                         604800 )       ; Negative Cache TTL
;
@       IN      NS      rjp.baratayuda.abimanyu.IT13.com.
; DNS Records
@           IN      A       10.70.3.3 ; IP Utama
www         IN      CNAME   baratayuda.abimanyu.IT13.com.
```

Lalu restart dengan  command
```
service bind9 restart
```
Berikut merupakan dokumentasinya
;
![untitled](https://cdn.discordapp.com/attachments/901344920361656355/1163319606119772170/image.png?ex=653f24cf&is=652cafcf&hm=93e4945d48854d31081bdfa9343c92248ff8dd06be471591b077c164109f21c9&)

### No 9
Arjuna merupakan suatu Load Balancer Nginx dengan tiga worker (yang juga menggunakan nginx sebagai webserver) yaitu Prabakusuma, Abimanyu, dan Wisanggeni. Lakukan deployment pada masing-masing worker.

### Jawaban No 9
Pada nodes Wisanggeni

Membuat konfigurasi file pada /var/www/jarkom/index.php
```
<?php
echo "Hello ini wisanggeni";
?>
```

Lalu Kemudian membuat konfigurasi /etc/nginx/sites-available/jarkom
```
 server {

 	listen 8003;

 	root /var/www/jarkom;

 	index index.php index.html index.htm;
 	server_name _;

 	location / {
 			try_files $uri $uri/ /index.php?$query_string;
 	}

 	# pass PHP scripts to FastCGI server
 	location ~ \.php$ {
 	include snippets/fastcgi-php.conf;
 	fastcgi_pass unix:/var/run/php/php7.2-fpm.sock;
 	}

 location ~ /\.ht {
 			deny all;
 	}

 	error_log /var/log/nginx/jarkom_error.log;
 	access_log /var/log/nginx/jarkom_access.log;
 }
```
Lalu Kemudian jalankan command 

```
ln -s /etc/nginx/sites-available/wisanggeni /etc/nginx/sites-enabled
   service nginx restart
 nginx -t

rm /etc/nginx/sites-enabled/default

service nginx restart

service php7.0-fpm start

service php7.0-fpm status
```

Lalu akses pada client nakula dengan cara 
```
 lynx http://10.70.3.4:8003

```
![untitled](https://cdn.discordapp.com/attachments/901344920361656355/1163734883512889405/image.png?ex=6540a791&is=652e3291&hm=26a431b9ca4409c8a6363bef6d393f270576624c9eed04e40558eaf018c89bfd&)

Pada nodes Abimanyu

Membuat konfigurasi file pada /var/www/jarkom/index.php
```
<?php
echo "Hello ini abimanyu";
?>
```

Lalu Kemudian membuat konfigurasi /etc/nginx/sites-available/jarkom
```
 server {

 	listen 8002;

 	root /var/www/jarkom;

 	index index.php index.html index.htm;
 	server_name _;

 	location / {
 			try_files $uri $uri/ /index.php?$query_string;
 	}

 	# pass PHP scripts to FastCGI server
 	location ~ \.php$ {
 	include snippets/fastcgi-php.conf;
 	fastcgi_pass unix:/var/run/php/php7.0-fpm.sock;
 	}

 location ~ /\.ht {
 			deny all;
 	}

 	error_log /var/log/nginx/jarkom_error.log;
 	access_log /var/log/nginx/jarkom_access.log;
 }
```
Lalu Kemudian jalankan command 

```
ln -s /etc/nginx/sites-available/abimanyu /etc/nginx/sites-enabled
   service nginx restart
 nginx -t

rm /etc/nginx/sites-enabled/default

service nginx restart

service php7.0-fpm start

service php7.0-fpm status
```

Lalu akses pada client nakula dengan cara 
```
 lynx http://10.70.3.3:8002

```
![untitled](https://cdn.discordapp.com/attachments/901344920361656355/1163735015146926180/image.png?ex=6540a7b0&is=652e32b0&hm=df4d4953275b1cbd8a735c9dc118289b15b824e072233e213519cb12805fa9db&)


Pada nodes Prabakusuma
Membuat konfigurasi file pada /var/www/jarkom/index.php
```
<?php
echo "Hello ini prabukusuma";
?>
```

Lalu Kemudian membuat konfigurasi /etc/nginx/sites-available/jarkom
```
 server {

 	listen 8001;

 	root /var/www/jarkom;

 	index index.php index.html index.htm;
 	server_name _;

 	location / {
 			try_files $uri $uri/ /index.php?$query_string;
 	}

 	# pass PHP scripts to FastCGI server
 	location ~ \.php$ {
 	include snippets/fastcgi-php.conf;
 	fastcgi_pass unix:/var/run/php/php7.0-fpm.sock;
 	}

 location ~ /\.ht {
 			deny all;
 	}

 	error_log /var/log/nginx/jarkom_error.log;
 	access_log /var/log/nginx/jarkom_access.log;
 }
```
Lalu Kemudian jalankan command 

```
ln -s /etc/nginx/sites-available/abimanyu /etc/nginx/sites-enabled
   service nginx restart
 nginx -t

rm /etc/nginx/sites-enabled/default

service nginx restart

service php70-fpm start

service php7.0-fpm status
```

Lalu akses pada client nakula dengan cara 
```
 lynx http://10.70.3.2:8001

```


![untitled](https://cdn.discordapp.com/attachments/901344920361656355/1163801914845179974/image.png?ex=6540e5fe&is=652e70fe&hm=14716ca0067a219419ba094b74697657b886da595be10df8408048248bbf65fb&)


### No 10
Kemudian gunakan algoritma Round Robin untuk Load Balancer pada Arjuna. Gunakan server_name pada soal nomor 1. Untuk melakukan pengecekan akses alamat web tersebut kemudian pastikan worker yang digunakan untuk menangani permintaan akan berganti ganti secara acak. Untuk webserver di masing-masing worker wajib berjalan di port 8001-8003. Contoh
    - Prabakusuma:8001
    - Abimanyu:8002
    - Wisanggeni:8003

### Jawaban No 10
Kemudian pada arjuna membuat konfigurasi untuk load balancer pada file /etc/nginx/sites-available/load-balancer

```
upstream webserver  {
        server 10.70.3.4:8003; #IP Prabukusuma
        server 10.70.3.3:8002; #IP Abimanyu
        server 10.70.3.2:8001; #IP Wisanggeni
 }

 server {
        listen 80;
        server_name arjuna.IT13.com www.arjuna.IT13.com;

        location / {
        proxy_pass http://webserver;
        }
 }
```
kemudian menjalankan command 
```
ln -s /etc/nginx/sites-available/load-balancer /etc/nginx/sites-enabled/load-balancer

service nginx restart
```

Kita dapat melihat outputnya pada 
```
lynx http://www.arjuna.IT13.com
```
Berikut merupakan dokumentasinya
![untitled](https://cdn.discordapp.com/attachments/901344920361656355/1163734883512889405/image.png?ex=6540a791&is=652e3291&hm=26a431b9ca4409c8a6363bef6d393f270576624c9eed04e40558eaf018c89bfd&)

![untitled](https://cdn.discordapp.com/attachments/901344920361656355/1163735015146926180/image.png?ex=6540a7b0&is=652e32b0&hm=df4d4953275b1cbd8a735c9dc118289b15b824e072233e213519cb12805fa9db&)


![untitled](https://cdn.discordapp.com/attachments/901344920361656355/1163801914845179974/image.png?ex=6540e5fe&is=652e70fe&hm=14716ca0067a219419ba094b74697657b886da595be10df8408048248bbf65fb&)



### No 11
Selain menggunakan Nginx, lakukan konfigurasi Apache Web Server pada worker Abimanyu dengan web server www.abimanyu.yyy.com. Pertama dibutuhkan web server dengan DocumentRoot pada /var/www/abimanyu.yyy

### Jawaban No 11
Membuat konfigurasi pada file /etc/apache2/sites-available/abimanyu.IT13.conf

```
<VirtualHost *:80>
    ServerName abimanyu.IT13.com
    ServerAlias 10.70.3.3
    ServerAlias www.abimanyu.IT13.com
    ServerAdmin webmaster@localhost
    DocumentRoot /var/www/abimanyu.IT13

    RewriteEngine On

    ErrorLog ${APACHE_LOG_DIR}/error.log
    CustomLog ${APACHE_LOG_DIR}/access.log combined
</VirtualHost>
```
lalu melakukan donwload untuk kebutuhna website dari drive dan meletakannya pada /var/www

```
mkdir /var/www/abimanyu

wget --no-check-certificate 'https://docs.google.com/uc?export=download&id=1a4V23hwK9S7hQEDEcv9FL14UkkrHc-Zc' -O abimanyu

unzip abimanyu -d abimanyu.IT13

rm abimanyu

mv abimanyu.IT13/abimanyu.yyy.com/* abimanyu.IT13

rmdir abimanyu.IT13/abimanyu.yyy.com
```
Lalu mengecek pada pada client Nakula dengan menggunakan perintah ```lynx http://www.abimanyu.IT13.com``` dan berikut merupakan bentuk dokumentasinya

![untitled](https://cdn.discordapp.com/attachments/901344920361656355/1163440059786215485/image.png?ex=653f94fd&is=652d1ffd&hm=5bf82d2ead4d271c29429829d40433d3ac2be02f03bb485277bc27657674538d&)


### No 12
Setelah itu ubahlah agar url www.abimanyu.yyy.com/index.php/home menjadi www.abimanyu.yyy.com/home.



### Jawaban No 12
Setelah itu, kita mengubah url www.abimanyu.yyy.com/index.php/home menjadi www.abimanyu.yyy.com/home.

Lalu dilakukan konfigurasi dengan menambahkan kode pada ```/etc/apache2/sites-available/abimanyu.IT13.conf```

```
    <Directory /var/www/abimanyu.IT13>
        Options +Indexes
    </Directory>

    Alias "/home" "/var/www/abimanyu.IT13/home.html"
    Alias / /var/www/abimanyu.IT13/home.html
```

Kemudian menajalankan perintah untuk menajalankan konfigurasi

```
cd /etc/apache2/sites-available/
a2enmod rewrite
a2ensite abimanyu.IT13.conf
service apache2 reload
service apache2 start
service apache2 status
```

Lalu dilakukan pengecekan dengan cara menggunakan perintah ```lynx http://www.abimanyu.IT13.com/home``` maka dokumentasinya akan seperti ini![untitled](https://cdn.discordapp.com/attachments/901344920361656355/1163440136625856533/image.png?ex=653f9510&is=652d2010&hm=20f2f2e3c22ed2a15e36be65f089b814596b211f6e3d830554f0b3d713f3b5eb&)

### No 13
Selain itu, pada subdomain www.parikesit.abimanyu.yyy.com, DocumentRoot disimpan pada /var/www/parikesit.abimanyu.yyy

Pada nomer ini kita membuat website baru menggunakan resource baru yang tersedia pada drive (parikesit.abimanyu). Langkah-langkah nya hampir sama dengan nomer 11 tadi. Lebih lengkap nya ada pada script dibawah ini. Dan Untuk mengecek apakah website tersebut sudah jalan, pada sisi client bisa lakukan perintah “lynx www.parikesit.abimanyu.IT13.com”.

``` bash
#!/bin/bash

# 13. Konfigurasi subdomain www.parikesit.abimanyu.IT13.com
apt update
apt install -y apache2
apt-get install php
apt-get install unzip

#---------------------------------------------------------------------

# URL berkas yang akan diunduh dari Google Drive
file_url="https://drive.google.com/u/0/uc?id=1LdbYntiYVF_NVNgJis1GLCLPEGyIOreS&export=download"

# Nama berkas output yang akan diunduh
output_file="berkas.zip"

# Lakukan pengunduhan dengan curl
curl -k -c ./cookie.txt -s -L "$file_url" -o "$output_file"

# Periksa apakah unduhan berhasil (contoh menggunakan panjang file)
file_size=$(du -b "$output_file" | cut -f1)
if [ $file_size -le 1024 ]; then
    echo "Unduhan gagal. Mungkin tautan telah berubah atau batasan unduhan telah dicapai."
    rm -f "$output_file"
    exit 1
fi

# Ekstrak berkas
unzip "$output_file"

# Hapus berkas zip jika diperlukan
# rm -f "$output_file"

mv parikesit.abimanyu.yyy.com parikesit.abimanyu.IT13

# Tampilkan pesan ketika selesai
echo "Unduhan dan ekstraksi selesai."

mv parikesit.abimanyu.IT13 /var/www/parikesit.abimanyu.IT13

#--------------------------------------------------------------------------------

cat <<EOL > /etc/apache2/ports.conf
Listen 80

<IfModule ssl_module>
	Listen 443
</IfModule>

<IfModule mod_gnutls.c>
	Listen 443
</IfModule>
EOL

cat <<EOL > /etc/apache2/sites-available/parikesit.abimanyu.IT13.com.conf
<VirtualHost *:80>
    ServerAdmin webmaster@parikesit.abimanyu.IT13.com
    ServerName www.parikesit.abimanyu.IT13.com
    DocumentRoot /var/www/parikesit.abimanyu.IT13
 

    ErrorLog ${APACHE_LOG_DIR}/error.log
    CustomLog ${APACHE_LOG_DIR}/access.log combined
</VirtualHost>
EOL

a2ensite parikesit.abimanyu.IT13.com.conf

service apache2 restart
```
### Jawaban No 13
![untitled](https://cdn.discordapp.com/attachments/901344920361656355/1163440231916249138/image.png?ex=653f9526&is=652d2026&hm=f2c4a3df380997959c0965789b2dea1c2c61a063c3cee87df1f4d3197e4af9cf&)

### No 14
Pada subdomain tersebut folder /public hanya dapat melakukan directory listing sedangkan pada folder /secret tidak dapat diakses (403 Forbidden).

Jadi pada nomer ini kita akan membuat folder /public dapat melakukan directory listing dan membuat folder baru yaitu /secret dan folder itu tidak dapat diakses. Untuk melakukan nya, dapat dilihat di script dibawah ini. Dan Untuk mengecek apakah sudah benar, pada sisi client bisa lakukan perintah “lynx www.parikesit.abimanyu.IT13.com” , lalu coba membuka folder public dan secret.

```
#!/bin/bash

# 14. Konfigurasi directory listing di /public dan 403 Forbidden di /secret

mkdir -p /var/www/parikesit.abimanyu.IT13/secret

cat <<EOL > /etc/apache2/sites-available/parikesit.abimanyu.IT13.com.conf
<VirtualHost *:80>
    ServerAdmin webmaster@parikesit.abimanyu.IT13.com
    ServerName www.parikesit.abimanyu.IT13.com
    DocumentRoot /var/www/parikesit.abimanyu.IT13

     <Directory /var/www/parikesit.abimanyu.IT13/public>
     Options +Indexes
     </Directory>
 
     <Directory /var/www/parikesit.abimanyu.IT13/secret>
     Options -Indexes
     </Directory>

    ErrorLog ${APACHE_LOG_DIR}/error.log
    CustomLog ${APACHE_LOG_DIR}/access.log combined
</VirtualHost>
EOL

service apache2 restart
```

### Jawaban No 14
![untitled](https://cdn.discordapp.com/attachments/901344920361656355/1163436527980904468/image.png?ex=653f91b3&is=652d1cb3&hm=5070ae381d75a9f2e3593f033502c537c10fd52dffb0664dc1635c59f5562f03&)


![untitled](https://cdn.discordapp.com/attachments/901344920361656355/1163436742725079110/image.png?ex=653f91e7&is=652d1ce7&hm=3122da91abe1cbeeeb9aba634d685eccde926f57eac56ce5d32ed633b2ca21b1&)

### No 15
Buatlah kustomisasi halaman error pada folder /error untuk mengganti error kode pada Apache. Error kode yang perlu diganti adalah 404 Not Found dan 403 Forbidden.

Untuk nomer ini kita diminta untuk melakukan kostumisasi pada halaman 403 dan 404, jadi jika kita melakukan percobaan masuk kesuatu website dan dia error not found atau forbidend, maka halaman yang muncuk adalah halaman yang telah kita kustom. Html kustom nya sendiri itu terdapat pada folder /error. Dan Untuk mengecek apakah sudah benar, pada sisi client bisa membuka website asal atau website yg forbiden misal pada /secret. 

```
#!/bin/bash

# 15. Buat halaman kustomisasi error

cat <<EOL > /etc/apache2/sites-available/parikesit.abimanyu.IT13.com.conf
<VirtualHost *:80>
    ServerAdmin webmaster@parikesit.abimanyu.IT13.com
    ServerName www.parikesit.abimanyu.IT13.com
    DocumentRoot /var/www/parikesit.abimanyu.IT13

     <Directory /var/www/parikesit.abimanyu.IT13/public>
     Options +Indexes
     </Directory>
 
     <Directory /var/www/parikesit.abimanyu.IT13/secret>
     Options -Indexes
     </Directory>

     ErrorDocument 403 /error/403.html
     ErrorDocument 404 /error/404.html

    ErrorLog ${APACHE_LOG_DIR}/error.log
    CustomLog ${APACHE_LOG_DIR}/access.log combined
</VirtualHost>
EOL

service apache2 restart
```

### Jawaban No 15
![untitled](https://cdn.discordapp.com/attachments/901344920361656355/1163436909859713095/image.png?ex=653f920e&is=652d1d0e&hm=651a6496ad0840ee6d76b4085225dc99733cc9e81b8c4cf0942adf2b548b634c&)

![untitled](https://cdn.discordapp.com/attachments/901344920361656355/1163437028470423632/image.png?ex=653f922b&is=652d1d2b&hm=3a717082a551a2e39d9e982d380c3fb986d2ce16c7c7211254c5268cd6cc55bf&)

### No 16
Buatlah suatu konfigurasi virtual host agar file asset www.parikesit.abimanyu.yyy.com/public/js menjadi 
www.parikesit.abimanyu.yyy.com/js 

Pada nomor ini hampir mirip dengan nomer 12, tapi sekarang akan menggunakan “alias”. Untuk mengecek apakah sudah benar atau belum dapat jalankan perintah ini “lynx www.parikesit.abimanyu.yyy.com/js”, jika sudah bisa maka nomor ini sudah berhasil. Lebih lengkap nya dapat di cek di script dibawah ini. 

```
#!/bin/bash

# 16. Konfigurasi virtual host agar file asset

cat <<EOL > /etc/apache2/sites-available/parikesit.abimanyu.IT13.com.conf
<VirtualHost *:80>
    ServerAdmin webmaster@parikesit.abimanyu.IT13.com
    ServerName www.parikesit.abimanyu.IT13.com
    DocumentRoot /var/www/parikesit.abimanyu.IT13

     <Directory /var/www/parikesit.abimanyu.IT13/public>
     Options +Indexes
     </Directory>
 
     <Directory /var/www/parikesit.abimanyu.IT13/secret>
     Options -Indexes
     </Directory>

     ErrorDocument 403 /error/403.html
     ErrorDocument 404 /error/404.html

     Alias "/js" "/var/www/parikesit.abimanyu.IT13/public/js"

    ErrorLog ${APACHE_LOG_DIR}/error.log
    CustomLog ${APACHE_LOG_DIR}/access.log combined
</VirtualHost>
EOL

service apache2 restart
```

### Jawaban No 16
![untitled](https://cdn.discordapp.com/attachments/901344920361656355/1163438377379577856/image.png?ex=653f936c&is=652d1e6c&hm=d34e3dffd76ce2246978237bea1b6cf421be6daec978efbaee9964b21a4ae6f5&)

### No 17
Agar aman, buatlah konfigurasi agar www.rjp.baratayuda.abimanyu.yyy.com hanya dapat diakses melalui port 14000 dan 14400.

Jadi pada soal ini kita akan membuat website baru lagi dengan resource berasal dari drive (rjp.baratayuda.abimanyu) yang telah diberikan. Setelah dibuat website nya seperti nomer-nomer sebelum nya kita diminta untuk setting port nya pada 14000 dan 14400. Dan untuk mengecek kita sudah berhasil atau belum kita dapat melakukan perintah ini “lynx www.rjp.baratayuda.abimanyu.IT13.com:14000 (atau :14400)” jika bisa maka nomor ini sudah berhasil dikerjakan.

```
#!/bin/bash

# 17. konfigurasi agar www.rjp.baratayuda.abimanyu.yyy.com hanya dapat diakses melalui port 14000 dan 14400

apt update
apt install -y apache2
apt-get install php
apt-get install unzip

#---------------------------------------------------------------------

# URL berkas yang akan diunduh dari Google Drive
file_url="https://drive.google.com/u/0/uc?id=1pPSP7yIR05JhSFG67RVzgkb-VcW9vQO6&export=download"

# Nama berkas output yang akan diunduh
output_file="berkas.zip"

# Lakukan pengunduhan dengan curl
curl -k -c ./cookie.txt -s -L "$file_url" -o "$output_file"

# Periksa apakah unduhan berhasil (contoh menggunakan panjang file)
file_size=$(du -b "$output_file" | cut -f1)
if [ $file_size -le 1024 ]; then
    echo "Unduhan gagal. Mungkin tautan telah berubah atau batasan unduhan telah dicapai."
    rm -f "$output_file"
    exit 1
fi

# Ekstrak berkas
unzip "$output_file"

# Hapus berkas zip jika diperlukan
# rm -f "$output_file"

mv rjp.baratayuda.abimanyu.yyy.com rjp.baratayuda.abimanyu.IT13

# Tampilkan pesan ketika selesai
echo "Unduhan dan ekstraksi selesai."

mv rjp.baratayuda.abimanyu.IT13 /var/www/rjp.baratayuda.abimanyu.IT13

#--------------------------------------------------------------------------------

cat <<EOL > /etc/apache2/ports.conf
Listen 80

Listen 14000

Listen 14400

<IfModule ssl_module>
	Listen 443
</IfModule>

<IfModule mod_gnutls.c>
	Listen 443
</IfModule>
EOL

cat <<EOL > /etc/apache2/sites-available/rjp.baratayuda.abimanyu.IT13.com.conf
<VirtualHost *:14000 *:14400>
    ServerAdmin webmaster@rjp.baratayuda.abimanyu.IT13.com
    ServerName www.rjp.baratayuda.abimanyu.IT13.com
    DocumentRoot /var/www/rjp.baratayuda.abimanyu.IT13
 

    ErrorLog ${APACHE_LOG_DIR}/error.log
    CustomLog ${APACHE_LOG_DIR}/access.log combined
</VirtualHost>
EOL

a2ensite rjp.baratayuda.abimanyu.IT13.com.conf

service apache2 restart
```

### Jawaban No 17
![untitled](https://cdn.discordapp.com/attachments/901344920361656355/1163491757099589702/image.png?ex=653fc523&is=652d5023&hm=1e5c76b720c20de3d985938d4c5dc7503cd812cb227c900754fbd96d9cdad3c1&)

![untitled](https://cdn.discordapp.com/attachments/901344920361656355/1163491867971833938/image.png?ex=653fc53d&is=652d503d&hm=0cb5657f1011b21fe229244b107615d157d260c24b1147868802a55c01eaae47&)

![untitled](https://cdn.discordapp.com/attachments/901344920361656355/1163491940600381520/image.png?ex=653fc54f&is=652d504f&hm=28b8519ca9172b27992ac9809cfac2a05e33d85d11ce3ede11f6417afee944f0&)

### No 18
Untuk mengaksesnya buatlah autentikasi username berupa “Wayang” dan password “baratayudayyy” dengan yyy merupakan kode kelompok. Letakkan DocumentRoot pada /var/www/rjp.baratayuda.abimanyu.yyy.

Pada nomor ini kita diminta agar website “rjp” mempunyai autentikasi, dengan menambahkan id : Wayang  dan password : baratayudaIT13 . Untuk mengecek apakah nomor ini sudah benar, kita dapat melakukan perintah “lynx www.rjp.baratayuda.abimanyu.IT13.com:14000” jika pada saat sebelum masuk web diminta id dan password maka nomor ini telah berhasil dikerjakan. 

```
#!/bin/bash

# 18. Password Wayang

cat <<EOL > /etc/apache2/sites-available/rjp.baratayuda.abimanyu.IT13.com.conf
<VirtualHost *:14000 *:14400>
    ServerAdmin webmaster@rjp.baratayuda.abimanyu.IT13.com
    ServerName www.rjp.baratayuda.abimanyu.IT13.com
    DocumentRoot /var/www/rjp.baratayuda.abimanyu.IT13
 
    <Directory /var/www/rjp.baratayuda.abimanyu.IT13>
        AuthType Basic
        AuthName "Restricted Content"
        AuthUserFile /etc/apache2/.htpasswd
        Require user Wayang
    </Directory>

    ErrorLog ${APACHE_LOG_DIR}/error.log
    CustomLog ${APACHE_LOG_DIR}/access.log combined
</VirtualHost>
EOL


htpasswd -b -c /etc/apache2/.htpasswd Wayang baratayudaIT13

service apache2 restart
```

### Jawaban No 18
![untitled](https://cdn.discordapp.com/attachments/901344920361656355/1163497946164252784/image.png?ex=653fcae7&is=652d55e7&hm=87767d14d692b0cd0ef941a6ce4ecdd0bb5eb9ed9358635caad4743cf5f4bc26&)

### No 19
Buatlah agar setiap kali mengakses IP dari Abimanyu akan secara otomatis dialihkan ke www.abimanyu.yyy.com (alias)

Jadi pada soal ini kita diminta apabila pada saat kita mengakses IP dari abimanyu akan langsung masuk ke website www.abimanyu.IT13.com. Untuk mengecek nya kita dapat melakukan perintah “lynx 10.70.3.3”, jika langsung direct ke website “www.abimanyu.IT13.com” maka pengerjaan soal ini telah berhasil. 

```
#!/bin/bash

#19. IP alias

cat <<EOL > /etc/apache2/sites-available/abimanyu.IT13.com.conf
<VirtualHost *:80>
    ServerAdmin webmaster@abimanyu.IT13.com
    ServerName www.abimanyu.IT13.com
    ServerAlias www.abimanyu.IT13.com 10.70.3.3
    DocumentRoot /var/www/abimanyu.IT13
 
     <Directory /var/www/abimanyu.IT13>
     Options +FollowSymLinks -Multiviews
     AllowOverride All
     </Directory>

    ErrorLog ${APACHE_LOG_DIR}/error.log
    CustomLog ${APACHE_LOG_DIR}/access.log combined
</VirtualHost>
EOL


service apache2 restart
```

### Jawaban No 19
![untitled](https://cdn.discordapp.com/attachments/901344920361656355/1163499378430980258/image.png?ex=653fcc3c&is=652d573c&hm=c6a23db2b70c013de95cddf93c3beb750932c4261fd6a30dad82ff4c869ee408&)

### No 20
Karena website www.parikesit.abimanyu.yyy.com semakin banyak pengunjung dan banyak gambar gambar random, maka ubahlah request gambar yang memiliki substring “abimanyu” akan diarahkan menuju abimanyu.png.

Pada soal terakhir ini kita diminta agar jika website “www.parikesit.abimanyu.IT13.com” dimasukkan substring “abimanyu” maka akan langsung direct ke “abimanyu.png. Untuk mengecek nya kita dapat melakukan perintah “lynx www.parikesit.abimanyu.IT13.com/abimanyu” jika langsung ada perintah download maka soal ini telah berhasil dikerjakan. 

```
#!/bin/bash

#20. 

cat <<EOL > /etc/apache2/sites-available/parikesit.abimanyu.IT13.com.conf
<VirtualHost *:80>
    ServerAdmin webmaster@parikesit.abimanyu.IT13.com
    ServerName www.parikesit.abimanyu.IT13.com
    DocumentRoot /var/www/parikesit.abimanyu.IT13

     <Directory /var/www/parikesit.abimanyu.IT13/public>
     Options +Indexes
     </Directory>
 
     <Directory /var/www/parikesit.abimanyu.IT13/secret>
     Options -Indexes
     </Directory>

     ErrorDocument 403 /error/403.html
     ErrorDocument 404 /error/404.html

     RewriteEngine On
     RewriteCond %{REQUEST_URI} ^.*abimanyu.*
     RewriteRule ^(.*)$ /public/images/abimanyu.png [L]

     Alias "/js" "/var/www/parikesit.abimanyu.IT13/public/js"

    ErrorLog ${APACHE_LOG_DIR}/error.log
    CustomLog ${APACHE_LOG_DIR}/access.log combined
</VirtualHost>
EOL

service apache2 restart
```

### Jawaban No 20
![untitled](https://cdn.discordapp.com/attachments/901344920361656355/1163502359633219654/image.png?ex=653fcf03&is=652d5a03&hm=83b27c265a8f4f8844a61bf363c69920cd88c8f33aafb6556e00e737da628be0&)
