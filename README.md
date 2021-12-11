# Jarkom-Modul-5-D09-2021

Nama Anggota | NRP
------------------- | --------------		
Dias Tri Kurniasari | 05111940000035
Nazhwa Ameera H | 05111940000133
Nur Moh. Ihsanuddien | 05111940000142

## A.) Topologi 

<img src="img/M05-0.0.png">


## B.) Subnetting
Pembagian subnet pada topologi ini menggunakan metode VLSM.

<img src="img/M05-1.0.png">

Dari hasil pembagian subnet, didapatkan sejumlah <b>8 subnet<b>.

### Perhitungan VLSM
1. Menentukan jumlah alamat IP yang dibutuhkan oleh tiap subnet dari 8 subnet yang ada. 

    Subnet | Jumlah IP | Netmask
    -------| --------- | -------	
    A1 | 3 | /29
    A2 | 101 | /25
    A3 | 701 | /22
    A4 | 2 | /30
    A5 | 2 | /30
    A6 | 301 | /23
    A7 | 201 | /24
    A8 | 3 | /29
    Total | 1314 | /21

    Sehingga, kita dapat menggunakan netmask /21 untuk memberikan pangalamatan IP pada 8 subnet.

2. Subnet besar yang kami bentuk memiliki `NID 192.196.0.0` dengan netmask /21. Kemudian, melakukan perhitungan pembagian IP dengan bantuan pohon IP

<img src="img/M05-1.1.png">

Sehingga, pembagian IP yang memungkinkan untuk topologi yang ada adalah sebagai berikut:

Subnet | NID | Netmask
-------| --- | -------
A1 | 192.196.7.128 | /29
A2 | 192.196.7.0 | /25
A3 | 192.196.0.0 | /22
A4 | 192.196.7.144 | /30
A5 | 192.196.7.148 | /30
A6 | 192.196.4.0 | /23
A7 | 192.196.6.0 | /24
A8 | 192.196.7.136 | /29
    
## C.) Routing

### Foosha
   ![C_1](https://user-images.githubusercontent.com/65032157/145677965-5f741ce3-1ad3-45ba-8648-d426988477db.png)

    
### Guanhao
   ![C_2](https://user-images.githubusercontent.com/65032157/145677928-1114248b-3fc4-4069-ab8a-c95396291264.png)

    
### Water7
   ![C_3](https://user-images.githubusercontent.com/65032157/145677943-23d8a1ad-b5a1-4375-917e-64557e664afd.png)


## D.) IP DHCP pada subnet Blueno, Cipher, Fukurou, dan Elena

    DHCP Server diletakkan pada Jipangu dengan file konfigurasi `/etc/dhcp/dhcpd.conf` sebagai berikut. Konfigurasi ini digunakan untuk memberikan IP address untuk masing-masing client
```
#Blueno
subnet 192.196.7.0 netmask 255.255.255.128 {
    range 192.196.7.2 192.196.7.126;
    option routers 192.196.7.1;
    option broadcast-address 192.196.7.127;
    option domain-name-servers 192.196.7.130;
    default-lease-time 360;
    max-lease-time 7200;
}

subnet 192.196.7.128 netmask 255.255.255.248 {
}

#Cipher
subnet 192.196.0.0 netmask 255.255.252.0 {
    range 192.196.0.2 192.196.3.254;
    option routers 192.196.0.1;
    option broadcast-address 192.196.3.255;
    option domain-name-servers 192.196.7.130;
    default-lease-time 360;
    max-lease-time 7200;
}

#Elena
subnet 192.196.4.0 netmask 255.255.254.0 {
    range 192.196.4.2 192.196.5.254;
    option routers 192.196.4.1;
    option broadcast-address 192.196.5.255;
    option domain-name-servers 192.196.7.130;
    default-lease-time 360;
    max-lease-time 7200;
}

#Fukurou
subnet 192.196.6.0 netmask 255.255.255.0 {
    range 192.196.6.2 192.196.6.254;
    option routers 192.196.6.1;
    option broadcast-address 192.196.6.255;
    option domain-name-servers 192.196.7.130;
    default-lease-time 360;
    max-lease-time 7200;
}    
```
    

### Soal 1
    
    
### Soal 2
 
### Soal 3  

Pada soal ketiga, kita diminta untuk membatasi  koneksi ICMP DHCP dan DNS Server maksimal 3 secara bersamaan, selebihnya didrop. Kita bisa menyelesaikan soal tersebut dengan iptables di bawah.  
`iptables -A INPUT -p icmp -m connlimit --connlimit-above 3 --connlimit-mask 0 -j DROP`  
  
Dengan keterangan sebagai berikut.  
`-A INPUT` : Menggunakan chain INPUT karena dikonfigurasikan langsung pada Jipangu dan Doriki  

`-p icmp` : Protokol yang digunakan, yaitu icmp (ping)  

`-m connlimit` : Menggunakan rule connection limit  

`--connlimit-above 3` : Limit yang ditangkap paket adalah di atas 3  

`--connlimit-mask 0` : Hanya memperbolehkan 3 koneksi setiap subnet dalam satu waktu  

`-j DROP` : Paket di drop   

  
Lakukan pengetesan dengan ping IP Jipangu/Doriki pada 4 node secara bersamaan. `ping 192.196.7.131`.  
  
![3-1](https://user-images.githubusercontent.com/68385532/145676938-e930c8f8-72f4-44cd-b1b0-d61dd0146e57.PNG)  

![3-2](https://user-images.githubusercontent.com/68385532/145676940-d2725e45-d026-4dd7-819f-12321a4826a3.PNG)  

![3-3](https://user-images.githubusercontent.com/68385532/145676941-fdae190a-2c7d-4aaf-8d45-0f3ea82ff2ab.PNG)  

![3-4](https://user-images.githubusercontent.com/68385532/145676943-a786c10d-c681-41ce-afdb-cc4d3e7f4553.PNG)  


### Soal 4  

Pada soal keempat, kita diminta untuk membatasi akses ke Doriki yang berasal dari subnet Blueno dan Cipher - hanya diperbolehkan pada pukul 07.00 - 15.00 pada hari Senin sampai Kamis.  Kita bisa menyelesaikan soal tersebut dengan iptables di bawah.  
```
#Jalankan di Doriki
#Cipher A3
iptables -A INPUT -s 192.196.0.0/22 -d 192.196.7.128/29 -m time --timestart 07:00 --timestop 15:00 --weekdays Mon,Tue,Wed,Thu -j ACCEPT
iptables -A INPUT -s 192.196.0.0/22 -j REJECT
#Blueno A2
iptables -A INPUT -s 192.196.7.0/25 -d 192.196.7.128/29 -m time --timestart 07:00 --timestop 15:00 --weekdays Mon,Tue,Wed,Thu -j ACCEPT
iptables -A INPUT -s 192.196.7.0/25 -j REJECT
```
  
![4-1](https://user-images.githubusercontent.com/68385532/145676945-3b3ca6b0-febd-49e0-b2a5-260ff4174fe6.PNG)  

![4-2](https://user-images.githubusercontent.com/68385532/145676947-0f28bce4-539a-4b7c-874d-b63ec84573b9.PNG)  

### Soal 5  

Pada soal kelima, kita diminta untuk membatasi akses ke Doriki yang berasal dari subnet Elena dan Fukurou - hanya diperbolehkan pada pukul 15.01 - 06.59 setiap harinya.  Kita bisa menyelesaikan soal tersebut dengan iptables di bawah.  
```
#Elena A6
iptables -A INPUT -s 192.196.4.0/23 -m time --timestart 15:01 --timestop 06:59 -j ACCEPT
iptables -A INPUT -s 192.196.4.0/23 -j REJECT
#Fukurou A7
iptables -A INPUT -s 192.196.6.0/24 -m time --timestart 15:01 --timestop 06:59 -j ACCEPT
iptables -A INPUT -s 192.196.6.0/24 -j REJECT
```
  
Dengan keterangan sebagai berikut.  
`-A INPUT` : Menggunakan chain INPUT karena langsung dikonfigurasi pada Doriki  

`-s 192.196.0.0/22, -s 192.196.4.0/23, -s 192.196.6.0/24, dan -s 192.196.7.0/25`: Alamat asal paket  

`-d 192.196.7.128/29`: Mendifinisikan alamat tujuan dari paket  

`-m time`: Menggunakan rule time  

`--timestart 07:00, 15:01`: Waktu mulai  

`--timestop 15:00, 06:59`: Waktu berakhir  

`--weekdays Mon,Tue,Wed,Thu`: Hari  

`-j ACCEPT`: Paket diterima  

`-j REJECT`: Paket ditolak    

  
Lakukan pengetesan dengan `ping google.com` pada node client setelah tanggal diubah. `date -s "8 nov 2021 10:00:00"`/ `date -s "8 nov 2021 17:00:00"`.  
  
![5-1](https://user-images.githubusercontent.com/68385532/145676949-ca57e5f0-c625-41ae-9435-bd019e453b52.PNG)  

![5-2](https://user-images.githubusercontent.com/68385532/145676950-80a7994a-3a55-413d-a891-009d948c07ad.PNG)  


### Soal 6  

Pada soal keenam, kita diminta agar Guanhou disetting sehingga setiap request dari client yang mengakses DNS Server akan didistribusikan secara bergantian pada Jorge dan Maingate.  
```
iptables -A PREROUTING -t nat -p tcp -d 192.196.7.128 --dport 80 -m statistic --mode nth --every 2 --packet 0 -j DNAT --to-destination  192.196.7.138:80
iptables -A PREROUTING -t nat -p tcp -d 192.196.7.128 --dport 80 -j DNAT --to-destination 192.196.7.139:80
iptables -t nat -A POSTROUTING -p tcp -d 192.196.7.138 --dport 80 -j SNAT --to-source 192.196.7.128:80
iptables -t nat -A POSTROUTING -p tcp -d 192.196.7.139 --dport 80 -j SNAT --to-source 192.196.7.128:80
```   
   
Lakukan pengetesan dengan instalasi apt-get install netcat -y lalu jalankan perintah nc 192.196.7.128 80.  

![6-1](https://user-images.githubusercontent.com/68385532/145676953-26d4ce3c-7795-44d7-b1b3-4b5024572c0f.PNG)  

![6-1-1](https://user-images.githubusercontent.com/68385532/145676955-c23c2298-8247-4adc-b083-2a8c97673653.PNG)  

![6-2](https://user-images.githubusercontent.com/68385532/145676957-d1e88e55-1a29-4ded-8f83-25ac0be16f4e.PNG)  

![6-2-1](https://user-images.githubusercontent.com/68385532/145676958-5bf97552-9b0e-410c-891a-7c30aba709ad.PNG)  

## Error dan Kendala
1. Terkadang salah meletakkan iptables sehingga hasil pengetesan tidak sesuai dengan yang diharapkan.
