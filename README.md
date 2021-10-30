# Jarkom-Modul-2-D11-2021

Anggota kelompok:
- 05111940000067 - Fika Nur Aini
- 05111940000095 - Fuad Elhasan Irfani
- 05111940000203 - Fidhia Ainun Khofifah
---
Laporan Resmi Modul 2
1. Buat peta seperti gambar yang tertera pada soal modul 2, EniesLobby akan dijadikan sebagai DNS Master, Water7 akan dijadikan DNS Slave, dan Skypie akan digunakan sebagai Web Server. Terdapat 2 Client yaitu Loguetown, dan Alabasta. Semua node terhubung pada router Foosha, sehingga dapat mengakses internet.
    - Hasil topologi yang kami buat seperti berikut
    ![image](https://user-images.githubusercontent.com/73324192/139509179-41fe23ac-0be5-4713-979e-a6f8b2a2e755.png)
    - Bukti root foosha sudah dapat mengakses internet
    ![image](https://user-images.githubusercontent.com/73324192/139509234-a8560086-4041-4cf6-998d-c3c6a9c87ed9.png)
    - Bukti node lain bisa mengakses internet
    ![image](https://user-images.githubusercontent.com/73324192/139509325-3fc996bb-0fa6-4382-8587-e3eb43b7b7b6.png)

2.  a. Setting alamat utama franky.d11.com
    - Jalankan ```apt-get update``` untuk update package list
    ![image](https://user-images.githubusercontent.com/73324192/139509585-82f8078e-fd8d-4884-a91e-33d65b2372b4.png)
    - Install bind9 dengan ```apt-get install bind9 -y```
    ![image](https://user-images.githubusercontent.com/73324192/139509703-847b684a-3081-4eb6-8ca1-4c7e9917fbc8.png)
    - Untuk membuat domain franky.d11.com , lakukan perintah ```nano etc/bind/named.conf.local``` pada EniesLobby lalu isi konfigurasi domain tersebut seperti di bawah ini.
    ![image](https://user-images.githubusercontent.com/73324192/139509781-6e4d7c50-2d93-42ff-ae6b-ab4baecaa731.png)
    - Buat folder kaizoku di dalam /etc/bind. Copykan file db.local pada path /etc/bind ke dalam folder kaizoku yang baru saja dibuat dan ubah namanya menjadi franky.d11.com. Kemudian buka file franky.d11.com dan edit seperti gambar berikut.
    ![image](https://user-images.githubusercontent.com/73324192/139509851-f9e2e662-b1c9-4126-8585-c69a5f06091e.png)
    - Restart bind9 dengan ```service bind9 restart```
    - Test dengan mengubah nameserver di LogueTown dan Alabasta dan ping franky.d11.com
    ![image](https://user-images.githubusercontent.com/73324192/139509928-575b3512-a5d1-4d06-95e4-9bd0f494eab1.png)
    
    b. Membuat alias ```www.franky.d11.com```
    - Buka file franky.d11.com pada server EniesLobby dan tambahkan konfigurasi seperti pada gambar berikut
    ![image](https://user-images.githubusercontent.com/73324192/139510175-0d70e4be-cca1-42bd-a649-9f0a2ee3675d.png)
    - Restart bind9
    - Untuk test, bisa ```ping www.franky.d11.com``` di LogueTown atau dapat memanggil ```host -t CNAME www.franky.d11.com```
    ![image](https://user-images.githubusercontent.com/73324192/139510242-6649a5af-084c-402a-9266-3fc990e5fca2.png)

3.  Buat subdomain super.franky.yyy.com dengan alias www.super.franky.yyy.com yang diatur DNS nya di EniesLobby dan mengarah ke Skypie
    
    a. Buat subdomain
    - Edit file ```/etc/bind/kaizoku/franky.d11.com``` lalu tambahkan subdomain untuk ```franky.d11.com``` yang mengarah ke IP Skypie ```192.197.2.4```
    ![image](https://user-images.githubusercontent.com/73324192/139510448-ca9ab8b5-976e-4626-8475-773af9982b2a.png)
    - Restart service bind dan test ```PING super.franky.d11.com``` di client
    ![image](https://user-images.githubusercontent.com/73324192/139510508-2b296050-c2b6-4094-bd38-3340b838febf.png)

    b. Membuat alias ```www.super.franky.yyy.com```
    - Buka file franky.d11.com pada server EniesLobby dan tambahkan konfigurasi seperti pada gambar berikut
    ![image](https://user-images.githubusercontent.com/73324192/139510560-0b59e7fa-17e0-4eeb-8b56-fdb72a5152cb.png)
    - Restart bind9
    - Untuk test, bisa PING ```www.super.franky.d11.com``` di LogueTown atau dapat memanggil ```host -t CNAME www.super.franky.d11.com```
    ![image](https://user-images.githubusercontent.com/73324192/139510623-a9293fb2-1e95-47b2-9bc5-2fd26018b2a8.png)

4.  Buat reverse domain untuk domain utama
    
    a. Konfigurasi
    - Edit file /etc/bind/named.conf.local pada EniesLobby
    ![image](https://user-images.githubusercontent.com/73324192/139513874-c653b82a-dafd-4d5b-adaa-4c3c8410bce4.png)
    - Copykan file db.local pada path /etc/bind ke dalam folder kaizoku dan ubah namanya menjadi 2.40.10.in-addr.arpa
    - Edit file 2.40.10.in-addr.arpa menjadi seperti gambar di bawah ini
    ![image](https://user-images.githubusercontent.com/73324192/139513899-80ce7265-d921-44cd-9a64-f501c1fb3ebc.png)
    - Restart service bind
    
    b. Testing
    - Pada Loguetown, edit nameserver pada resolv.conf dengan nameserver dari Foosha ```192.168.122.1```
    - Update package list dengan ```apt-get update``` lalu install dnsutils dengan ```apt-get install dnsutils```
    ![image](https://user-images.githubusercontent.com/73324192/139514065-c6b04ee3-35a8-4899-a33a-956355a4dc02.png)
    - Kembalikan nameserver ke EniesLobby ```192.197.2.2``` lalu cek reverse domainnya
    ![unnamed2](https://user-images.githubusercontent.com/73324192/139514161-d0d2b25f-b787-4088-8d1b-5e35d4288501.png)

5.  Buat Water7 sebagai DNS Slave untuk domain utama
    
    a. Konfigurasi pada server EniesLobby
    - Edit file /etc/bind/named.conf.local dan sesuaikan seperti berikut
    ![image](https://user-images.githubusercontent.com/73324192/139514225-9384307a-cc79-46ea-beec-6f4f6e21a5c9.png)
    - Restart sevice bind
    
    b. Konfigurasi pada server Water7
    - Update package list dengan ```apt-get update``` lalu install bind9 dengan ```apt-get install bind9 -y```
    - Kemudian buka file /etc/bind/named.conf.local pada Water7 dan tambahkan syntax berikut
    ![image](https://user-images.githubusercontent.com/73324192/139514283-95ccf575-78b0-4eb9-84b9-667a5951ba90.png)
    - Restart sevice bind
    
    c. Testing
    - Pada server EniesLobby, matikan service bind9 dengan ```service bind9 stop```
    - Pada client Loguetown pastikan pengaturan nameserver mengarah ke IP EniesLobby dan IP Water7
    - Lakukan PING ke franky.d11.com pada client Loguetown.PING berhasil dilakukan maka dapat disimpulkan bahwa konfigurasi DNS slave telah berhasil.
    ![image](https://user-images.githubusercontent.com/73324192/139514359-a2c73363-81f7-4650-b069-81a65f4140c2.png)

6.  Terdapat subdomain ```mecha.franky.d11.com``` dengan alias ```www.mecha.franky.d11.com``` yang didelegasikan dari EniesLobby ke Water7 dengan IP menuju ke Skypie dalam folder sunnygo
    
    a. Konfigurasi pada EniesLobby
    - Buka /etc/bind/kaizoku/franky.d11.com dan edit seperti dibawah ini (record A & NS)
    ![image](https://user-images.githubusercontent.com/73324192/139514387-03bb85f8-64cb-40d3-9ad5-28f082d3d3ee.png)
    - Edit ```/etc/bind/named.conf.options```. Comment ```dnssec-validation``` dan tambahkan ```allow-query{any;};```
    ![image](https://user-images.githubusercontent.com/73324192/139514458-e4591d2f-4f87-43f0-a673-5f40f9515498.png)
    - Edit /etc/bind/named.conf.local menjadi seperti dibawah (pada allow-transfer)
    ![image](https://user-images.githubusercontent.com/73324192/139514501-e3482d1e-08db-443f-9b9a-04e2c1536a72.png)
    
    b. Konfigurasi pada Water7
    - Edit ```/etc/bind/named.conf.options```. Comment ```dnssec-validation``` dan tambahkan ```allow-query{any;};```
    ![unnamed (1)2](https://user-images.githubusercontent.com/73324192/139514553-94ff9c48-ac4d-4a43-99c8-95a99b10104c.png)
    - Edit file /etc/bind/named.conf.local menjadi seperti di bawah ini
    ![image](https://user-images.githubusercontent.com/73324192/139514582-d47a0242-f5f7-434f-b999-12a34661d9be.png)
    - Buat direktori ```/etc/bind/sunnygo```. Copy db.local ke direktori dan edit namanya menjadi mecha.franky.d11.com. Kemudian edit menjadi seperti di bawah ini
    ![unnamed (2)](https://user-images.githubusercontent.com/73324192/139514652-c392c45f-cfc8-45bd-b0ea-7eb2e652a838.png)
    - Restart service bind
    
    c. Testing
    - Lakukan ping ke mecha.franky.d11.com dari Loguetown
    ![image](https://user-images.githubusercontent.com/73324192/139514681-35793dca-bafd-4992-9966-ce6f9211d02f.png)

7.  Buatkan subdomain melalui Water7 dengan nama ```general.mecha.franky.yyy.com``` dengan alias ```www.general.mecha.franky.yyy.com``` yang mengarah ke Skypie

    a. Membuat subdomain
    - Edit /etc/bind/sunnygo/mecha.franky.d11.com. Tambahkan konfigurasi berikut
    ![image](https://user-images.githubusercontent.com/73324192/139514860-a13376bb-344a-449e-85f2-07f3388a016d.png)
    - Restart service bind9
    
    b. Testing
    - Coba ping ```general.mecha.franky.d11.com``` dan host -t CNAME ```www.general.mecha.franky.d11.com```
    ![image](https://user-images.githubusercontent.com/73324192/139514903-186f0a4c-a054-4d78-bca4-4fcb29fe6fc6.png)

---
## Kendala
Kami tidak bisa mengerjakan soal web server. Saat mengikuti modul kami menemui error ini dan kami masih belum bisa mengatasinya.
![vlcsnap-2021-10-30-08h14m01s639 (2)](https://user-images.githubusercontent.com/73324192/139515153-bc1f44bf-143e-4c2c-a5d0-3701f67676c7.png)
