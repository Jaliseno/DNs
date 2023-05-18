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
@ IN A 192.128.12.111

--------------------------------------------
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
@ IN AAAAA 2701:fe9a:ed9e:1ee6

--------------------------------------------
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
www IN CNAME domain.tld

--------------------------------------------
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
@ IN MX 10 mail.cloudkilat.com

--------------------------------------------
Hostname   : @ (untuk inisiasi root domain)
Type/Record: MX (Mail Exchange Record)
Address    : mail.domain.tld (domain mail server Anda)
TTL        : 3600
Priority   : 0-10 (sesuaikan dengan prioritas mail server Anda, semakin kecil nilai maka mail server akan menjadi prioritas)
```   

5. ***TXT Record (Text Record)***: Menyimpan informasi teks tambahan, seperti verifikasi pengiriman email dan konfigurasi domain.
    - **Record SPF (Sender Policy Framework)** adalah jenis DNS record yang digunakan untuk mengautentikasi pengirim email dan mencegah email spoofing. SPF record memungkinkan pemilik domain untuk menentukan server mana yang diizinkan untuk mengirim email atas nama domain tersebut.
    ```
    Buat SPF record: Tambahkan atau edit record SPF dengan menentukan nilai SPF record yang tepat. 
    Nilai SPF record tergantung pada layanan email yang Anda gunakan.
    ```
    **Contoh SPF record:**
   
    Jika Anda menggunakan Google Workspace:
    ```v=spf1 include:_spf.google.com ~all```

    Jika Anda menggunakan Microsoft 365:
    ```v=spf1 include:spf.protection.outlook.com -all```
    
    Jika Anda menggunakan Mail Server Sendiri:
    ```v=spf1 +a +mx ip4:192.128.12.111/32(IPv4 Public Mail Server Anda) -all```
    
    ```
    ------------------
    Contoh Penambahan:
    ------------------
    @ IN TXT v=spf1 +a +mx ip4:192.128.12.111/32 -all 
    
    --------------------------------------------
    Hostname   : @ (untuk inisiasi root domain)
    Type/Record: TXT (Text Record)
    Address    : v=spf1 +a +mx ip4:192.128.12.111/32(IPv4 Public Mail Server Anda) -all [record SPF Anda]
    TTL        : 3600
    Priority   : N/A
    ```   
    
    - ***DKIM record (DomainKeys Identified Mail)*** adalah jenis DNS record yang digunakan untuk mengautentikasi email dan memverifikasi bahwa email tersebut berasal dari domain yang dinyatakan dan tidak dimodifikasi selama pengiriman.
    
    ```
    ------------------
    Contoh Penambahan:
    ------------------
    example._domainkey.example.com. IN TXT "v=DKIM1; k=rsa; p=MIGfMA0G..."

    --------------------------------------------
    Hostname   : default._domainkey.example.com.
    Type/Record: TXT (Text Record)
    Address    : "v=DKIM1; k=rsa; p=MIGfMA0G..."
    TTL        : 3600
    Priority   : N/A
    
    NB: Informasi detail mengenai record DKIM ini silakan Anda menghubungi pihak penyedia Mail server Anda.
    ```
    
    - ***DMARC (Domain-based Message Authentication, Reporting, and Conformance)*** adalah mekanisme pengautentikan email yang memungkinkan pemilik domain untuk memverifikasi pengiriman email yang berasal dari domain mereka dan memberikan instruksi tentang apa yang harus dilakukan dengan email yang gagal verifikasi.
    
    ```
    ------------------
    Contoh Penambahan:
    ------------------
    _dmarc.example.com IN TXT "v=DMARC1; p=none; rua=mailto:dmarc@example.com; ruf=mailto:abuse@example.com; fo=1"
    
    --------------------------------------------
    Hostname   : _dmarc.example.com.
    Type/Record: TXT (Text Record)
    Address    : "v=DMARC1; p=none; rua=mailto:dmarc@example.com; ruf=mailto:abuse@example.com; fo=1"
    TTL        : 3600
    Priority   : N/A
    
    Keterangan: 
    -----------
        - v=DMARC1: Menunjukkan versi DMARC yang digunakan.
        - p=none: Menetapkan kebijakan tindakan saat gagal verifikasi (none, quarantine, atau reject). Dalam contoh ini, tindakan tidak diambil (none).
        - rua=mailto:dmarc@example.com: Menentukan alamat email yang akan menerima laporan agregat DMARC.
        - ruf=mailto:abuse@example.com: Menentukan alamat email yang akan menerima laporan forensik DMARC.
        - fo=1: Menentukan opsi tindakan forensik yang harus diambil saat terjadi pelanggaran DMARC.
  
  NB: Informasi detail mengenai record DMARC ini silakan Anda menghubungi pihak penyedia Mail server Anda.
 
    ```

*Sebagai informasi terakhir setelah perubahan/penambahan record, maka domain akan memasuki proses propagasi terlebih dahulu agar record yang telah diubah/ditambah dapat resolve sepenuhnya ke seluruh ISP, dan biasanya proses propagasi ini akan memakan waktu maksimal 2x24 jam bergantung pada kecepatan masing masing resolver ISP.*

Sekian informasi yang dapat kami sampaikan dan Apabila Anda mengalami kesuliatan dan kendala saat melakukan penambahan record DNS, mohon untuk menghubungi tim support kami melalui tiket atau e-mail ke info@cloudkilat.com
