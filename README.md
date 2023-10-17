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
Menambahkan nameserver pada konfigurasi Nakula sebagai client. Jalankan perintah berikut satu persatu

```echo "nameserver 10.70.1.4" > /etc/resolv.conf
 ```
 Jalankan command berikut pada Nakula
 ```
 ping arjuna.IT13.com atau www.arjuna.IT13.com
 ```
 Berikut merupakan hasil dokumentasinya saat testing berjalan
![untitled](https://cdn.discordapp.com/attachments/901344920361656355/1163734497397837834/image.png?ex=6540a735&is=652e3235&hm=6d060469a367496488d12089093f366bab509892012d6de80d7f885ef218dc91&)


### No 3
Dengan cara yang sama seperti soal nomor 2, buatlah website utama dengan akses ke abimanyu.yyy.com dan alias www.abimanyu.yyy.com.

### Jawaban No 3
![untitled](https://cdn.discordapp.com/attachments/901344920361656355/1163734564326359171/image.png?ex=6540a745&is=652e3245&hm=176c064f3eb34ab491c283c65c24568b0a4f7655af11d2d09b0d16b8132479d2&)


### No 4
Kemudian, karena terdapat beberapa web yang harus di-deploy, buatlah subdomain parikesit.abimanyu.yyy.com yang diatur DNS-nya di Yudhistira dan mengarah ke Abimanyu.

### Jawaban No 4
![untitled](https://cdn.discordapp.com/attachments/901344920361656355/1163734651198779462/image.png?ex=6540a759&is=652e3259&hm=40b56d4c98ebbe85da7c296c07b6bb3c9921ebae773c234dfb40196f26e879ac&)

### No 5
Buat juga reverse domain untuk domain utama. (Abimanyu saja yang direverse)


### Jawaban No 5
![untitled](https://cdn.discordapp.com/attachments/901344920361656355/1163317195951059045/image.png?ex=653f2290&is=652cad90&hm=ddcee9629fcae72da7598e27a3400b983f05e648e6379a7b8475e3f6233a9696&)

### No 6
Agar dapat tetap dihubungi ketika DNS Server Yudhistira bermasalah, buat juga Werkudara sebagai DNS Slave untuk domain utama.

### Jawaban No 6
![untitled](https://cdn.discordapp.com/attachments/901344920361656355/1154393256273125376/image.png)

### No 7
Seperti yang kita tahu karena banyak sekali informasi yang harus diterima, buatlah subdomain khusus untuk perang yaitu baratayuda.abimanyu.yyy.com dengan alias www.baratayuda.abimanyu.yyy.com yang didelegasikan dari Yudhistira ke Werkudara dengan IP menuju ke Abimanyu dalam folder Baratayuda.

### Jawaban No 7
![untitled](https://cdn.discordapp.com/attachments/901344920361656355/1163734792110616636/image.png?ex=6540a77b&is=652e327b&hm=a58ea27966b2548942219ac7dfbb58c01af981f77021d605d3840fbcb8aca66b&)

### No 8
Untuk informasi yang lebih spesifik mengenai Ranjapan Baratayuda, buatlah subdomain melalui Werkudara dengan akses rjp.baratayuda.abimanyu.yyy.com dengan alias www.rjp.baratayuda.abimanyu.yyy.com yang mengarah ke Abimanyu.

### Jawaban No 8
![untitled](https://cdn.discordapp.com/attachments/901344920361656355/1163319606119772170/image.png?ex=653f24cf&is=652cafcf&hm=93e4945d48854d31081bdfa9343c92248ff8dd06be471591b077c164109f21c9&)

### No 9
Arjuna merupakan suatu Load Balancer Nginx dengan tiga worker (yang juga menggunakan nginx sebagai webserver) yaitu Prabakusuma, Abimanyu, dan Wisanggeni. Lakukan deployment pada masing-masing worker.

### Jawaban No 9
![untitled](https://cdn.discordapp.com/attachments/901344920361656355/1154393256273125376/image.png)


![untitled](https://cdn.discordapp.com/attachments/901344920361656355/1163734883512889405/image.png?ex=6540a791&is=652e3291&hm=26a431b9ca4409c8a6363bef6d393f270576624c9eed04e40558eaf018c89bfd&)

![untitled](https://cdn.discordapp.com/attachments/901344920361656355/1163735015146926180/image.png?ex=6540a7b0&is=652e32b0&hm=df4d4953275b1cbd8a735c9dc118289b15b824e072233e213519cb12805fa9db&)

![untitled](https://cdn.discordapp.com/attachments/901344920361656355/1163735061749833778/image.png?ex=6540a7bb&is=652e32bb&hm=3c4baa0f13d53b95d176d51b1724eac8f415f7e0735b1e16ce7a568bdf39e23f&)

### No 10
Kemudian gunakan algoritma Round Robin untuk Load Balancer pada Arjuna. Gunakan server_name pada soal nomor 1. Untuk melakukan pengecekan akses alamat web tersebut kemudian pastikan worker yang digunakan untuk menangani permintaan akan berganti ganti secara acak. Untuk webserver di masing-masing worker wajib berjalan di port 8001-8003. Contoh
    - Prabakusuma:8001
    - Abimanyu:8002
    - Wisanggeni:8003

### Jawaban No 10
![untitled](https://cdn.discordapp.com/attachments/901344920361656355/1163734883512889405/image.png?ex=6540a791&is=652e3291&hm=26a431b9ca4409c8a6363bef6d393f270576624c9eed04e40558eaf018c89bfd&)

![untitled](https://cdn.discordapp.com/attachments/901344920361656355/1163735015146926180/image.png?ex=6540a7b0&is=652e32b0&hm=df4d4953275b1cbd8a735c9dc118289b15b824e072233e213519cb12805fa9db&)

![untitled](https://cdn.discordapp.com/attachments/901344920361656355/1163735061749833778/image.png?ex=6540a7bb&is=652e32bb&hm=3c4baa0f13d53b95d176d51b1724eac8f415f7e0735b1e16ce7a568bdf39e23f&)


### No 11

### Jawaban No 11
![untitled](https://cdn.discordapp.com/attachments/901344920361656355/1163440059786215485/image.png?ex=653f94fd&is=652d1ffd&hm=5bf82d2ead4d271c29429829d40433d3ac2be02f03bb485277bc27657674538d&)


### No 12

### Jawaban No 12
![untitled](https://cdn.discordapp.com/attachments/901344920361656355/1163440136625856533/image.png?ex=653f9510&is=652d2010&hm=20f2f2e3c22ed2a15e36be65f089b814596b211f6e3d830554f0b3d713f3b5eb&)

### No 13

### Jawaban No 13
![untitled](https://cdn.discordapp.com/attachments/901344920361656355/1163440231916249138/image.png?ex=653f9526&is=652d2026&hm=f2c4a3df380997959c0965789b2dea1c2c61a063c3cee87df1f4d3197e4af9cf&)

![untitled](https://cdn.discordapp.com/attachments/901344920361656355/1163440812726702140/image.png?ex=653f95b1&is=652d20b1&hm=ebe86c7187afd6db8f76607d0620e711dafb559252d1ed2ba3582220f8654d5f&)

### No 14

### Jawaban No 14
![untitled](https://cdn.discordapp.com/attachments/901344920361656355/1163436527980904468/image.png?ex=653f91b3&is=652d1cb3&hm=5070ae381d75a9f2e3593f033502c537c10fd52dffb0664dc1635c59f5562f03&)


![untitled](https://cdn.discordapp.com/attachments/901344920361656355/1163436742725079110/image.png?ex=653f91e7&is=652d1ce7&hm=3122da91abe1cbeeeb9aba634d685eccde926f57eac56ce5d32ed633b2ca21b1&)

### No 15

### Jawaban No 15
![untitled](https://cdn.discordapp.com/attachments/901344920361656355/1163436909859713095/image.png?ex=653f920e&is=652d1d0e&hm=651a6496ad0840ee6d76b4085225dc99733cc9e81b8c4cf0942adf2b548b634c&)

![untitled](https://cdn.discordapp.com/attachments/901344920361656355/1163437028470423632/image.png?ex=653f922b&is=652d1d2b&hm=3a717082a551a2e39d9e982d380c3fb986d2ce16c7c7211254c5268cd6cc55bf&)

### No 16

### Jawaban No 16
![untitled](https://cdn.discordapp.com/attachments/901344920361656355/1163438377379577856/image.png?ex=653f936c&is=652d1e6c&hm=d34e3dffd76ce2246978237bea1b6cf421be6daec978efbaee9964b21a4ae6f5&)

### No 17

### Jawaban No 17
![untitled](https://cdn.discordapp.com/attachments/901344920361656355/1163491757099589702/image.png?ex=653fc523&is=652d5023&hm=1e5c76b720c20de3d985938d4c5dc7503cd812cb227c900754fbd96d9cdad3c1&)

![untitled](https://cdn.discordapp.com/attachments/901344920361656355/1163491867971833938/image.png?ex=653fc53d&is=652d503d&hm=0cb5657f1011b21fe229244b107615d157d260c24b1147868802a55c01eaae47&)

![untitled](https://cdn.discordapp.com/attachments/901344920361656355/1163491940600381520/image.png?ex=653fc54f&is=652d504f&hm=28b8519ca9172b27992ac9809cfac2a05e33d85d11ce3ede11f6417afee944f0&)

### No 18

### Jawaban No 18
![untitled](https://cdn.discordapp.com/attachments/901344920361656355/1163497946164252784/image.png?ex=653fcae7&is=652d55e7&hm=87767d14d692b0cd0ef941a6ce4ecdd0bb5eb9ed9358635caad4743cf5f4bc26&)

### No 19

### Jawaban No 19
![untitled](https://cdn.discordapp.com/attachments/901344920361656355/1163499378430980258/image.png?ex=653fcc3c&is=652d573c&hm=c6a23db2b70c013de95cddf93c3beb750932c4261fd6a30dad82ff4c869ee408&)

### No 20


### Jawaban No 20
![untitled](https://cdn.discordapp.com/attachments/901344920361656355/1163502359633219654/image.png?ex=653fcf03&is=652d5a03&hm=83b27c265a8f4f8844a61bf363c69920cd88c8f33aafb6556e00e737da628be0&)
