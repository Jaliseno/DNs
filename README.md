# DNs
TTG DNS


**DNS Record** merupakan entri data yang disimpan dalam database DNS. DNS record menyimpan informasi tentang suatu domain name (nama domain) dan menghubungkannya dengan alamat IP yang terkait.

Menambahkan DNS Record pada domain bukanlah hal sulit. Kali ini kami akan memberikan pemahaman bagi para pengguna web hosting Indonesia untuk memahami record dan langkah penambahannya.

Pertama kami akan memberikan record yang umum digunakan untuk domain:

1. ***A Record (Address Record)***: Menghubungkan nama domain dengan alamat IPv4.
```
-----------
Contoh:
-----------
Hostname   : @ (untuk inisiasi root domain),  Sub (tanpa titik dibelakangnya (untuk inisiasi subdomain))
Type/Record: A (Address)
Address    : 192.128.12.111 (IPv4 Public VM/Hosting Anda)
TTL        : 3600
Priority   : N/A
```   
           
2. ***AAAA Record (IPv6 Address Record)***: Menghubungkan nama domain dengan alamat IPv6 (**Masih jarang digunakan**).

```
-----------
Contoh:
-----------
Hostname   : @ (untuk inisiasi root domain),  Sub (tanpa titik dibelakangnya (untuk inisiasi subdomain))
Type/Record: AAAA (Address)
Address    : 2701:fe9a:ed9e:1ee6 (IPv6 Public VM/Hosting Anda)
TTL        : 3600
Priority   : N/A
```  

3. CNAME Record (Canonical Name Record): Memberikan alias untuk nama domain lain (misalnya, subdomain.domain.tld bisa diarahkan ke www.domain.tld).

```
-----------
Contoh:
-----------
Hostname   : www (saran kami untuk CNAME record tidak untuk melakukan pointing subdomain)
Type/Record: CNAME (Canonical Name Record)
Address    : domain.tld (isikan domain tujuan Anda)
TTL        : 3600
Priority   : N/A
```   

4. MX Record (Mail Exchange Record): Menentukan server email yang bertanggung jawab untuk domain tertentu.

```
-----------
Contoh:
-----------
Hostname   : @ (untuk inisiasi root domain)
Type/Record: MX (Mail Exchange Record)
Address    : mail.domain.tld (domain mail server Anda)
TTL        : 3600
Priority   : 0-10 (sesuaikan dengan prioritas mail server Anda, semakin kecil nilai maka mail server akan menjadi prioritas)
```   

5. *TXT Record (Text Record)*: Menyimpan informasi teks tambahan, seperti verifikasi pengiriman email dan konfigurasi domain.
    - **Record SPF (Sender Policy Framework)** adalah jenis DNS record yang digunakan untuk mengautentikasi pengirim email dan mencegah email spoofing. SPF record memungkinkan pemilik domain untuk menentukan server mana yang diizinkan untuk mengirim email atas nama domain tersebut.
    ```
    Buat SPF record: Tambahkan atau edit record SPF dengan menentukan nilai SPF record yang tepat. 
    Nilai SPF record tergantung pada layanan email yang Anda gunakan.
    ```
    Contoh SPF record:
    ====================
    Jika Anda menggunakan Google Workspace:
    ```v=spf1 include:_spf.google.com ~all```

    Jika Anda menggunakan Microsoft 365:
    ```v=spf1 include:spf.protection.outlook.com -all```
    
    Jika Anda menggunakan Mail Server Sendiri:
    ```v=spf1 +a +mx ip4:192.128.12.111/32(IPv4 Public Mail Server Anda) -all```
    
    ```
    -----------
    Contoh Penambahan:
    -----------
    Hostname   : @ (untuk inisiasi root domain)
    Type/Record: TXT (Text Record)
    Address    : v=spf1 +a +mx ip4:192.128.12.111/32(IPv4 Public Mail Server Anda) -all [record SPF Anda]
    TTL        : 3600
    Priority   : N/A```   
    - 

6. NS Record (Name Server Record): Menunjukkan server DNS yang bertanggung jawab untuk domain tertentu.

7. SOA Record (Start of Authority Record): Menyimpan informasi penting tentang zona DNS, termasuk informasi kontak administratif.
