# Jarkom-Modul-2-D11-2021

Anggota kelompok:
- 05111940000067 - Fika Nur Aini
- 05111940000095 - Fuad Elhasan Irfani
- 05111940000203 - Fidhia Ainun Khofifah
---
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
