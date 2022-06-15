---
lab:
  title: 08 - Azure Firewall
  module: Module 02 - Implement Platform Protection
ms.openlocfilehash: 1657a251f1355150d6386f8793825369be955705
ms.sourcegitcommit: e9389f8de66fec6d456a3f303bd350e380df7ff2
ms.translationtype: HT
ms.contentlocale: id-ID
ms.lasthandoff: 04/04/2022
ms.locfileid: "145195923"
---
# <a name="lab-08-azure-firewall"></a>Lab 08: Azure Firewall
# <a name="student-lab-manual"></a>Panduan lab siswa

## <a name="lab-scenario"></a>Skenario lab

Anda telah diminta untuk menginstal Azure Firewall. Ini akan membantu organisasi Anda mengontrol akses jaringan masuk dan keluar yang merupakan bagian penting dari rencana keamanan jaringan secara keseluruhan. Secara khusus, Anda perlu membuat dan menguji komponen infrastruktur berikut:

- Jaringan virtual dengan subnet beban kerja dan subnet host lompat.
- Mesin virtual adalah setiap subnet. 
- Rute kustom yang memastikan semua lalu lintas beban kerja keluar dari subnet beban kerja harus menggunakan firewall.
- Aturan Aplikasi Firewall yang hanya mengizinkan lalu lintas keluar ke www.bing.com. 
- Aturan Jaringan Firewall yang memperbolehkan pencarian server DNS eksternal.

> Untuk semua sumber daya di lab ini, kita menggunakan wilayah **US Timur**. Verifikasi dengan instruktur Anda bahwa ini adalah wilayah yang akan digunakan untuk kelas. 

## <a name="lab-objectives"></a>Tujuan lab

Di lab ini, Anda akan menyelesaikan latihan berikut:

- Latihan 1: Sebarkan dan uji Azure Firewall

## <a name="azure-firewall-diagram"></a>diagram Azure Firewall

![gambar](https://user-images.githubusercontent.com/91347931/157529954-a1bc434b-2eca-41c1-b875-1f0c977d5e20.png)

## <a name="instructions"></a>Instruksi

## <a name="lab-files"></a>File lab:

- **\\Allfiles\\Labs\\08\\template.json**

### <a name="exercise-1-deploy-and-test-an-azure-firewall"></a>Latihan 1: Sebarkan dan uji Azure Firewall

### <a name="estimated-timing-40-minutes"></a>Perkiraan waktu: 40 menit

> Untuk semua sumber daya di lab ini, kita menggunakan wilayah **Timur (US)** . Verifikasi dengan instruktur Anda bahwa ini adalah wilayah yang akan digunakan untuk kelas Anda. 

Dalam latihan ini, Anda akan menyelesaikan tugas-tugas berikut:

- Tugas 1: Gunakan templat untuk menyebarkan lingkungan lab. 
- Tugas 2: Menyebarkan Azure firewall.
- Tugas 3: Membuat rute default.
- Tugas 4: Mengonfigurasi aturan aplikasi.
- Tugas 5: Mengonfigurasi aturan jaringan. 
- Tugas 6: Mengonfigurasi server DNS.
- Tugas 7: Menguji firewall. 

#### <a name="task-1-use-a-template-to-deploy-the-lab-environment"></a>Tugas 1: Gunakan templat untuk menyebarkan lingkungan lab. 

Dalam tugas ini, Anda akan meninjau dan menyebarkan lingkungan lab. 

Dalam tugas ini, Anda akan membuat mesin virtual menggunakan templat ARM. Mesin virtual ini akan digunakan dalam latihan terakhir untuk lab ini. 

1. Masuk ke portal Microsoft Azure **`https://portal.azure.com/`** .

    >**Catatan**: Masuk ke portal Microsoft Azure menggunakan akun yang memiliki peran Pemilik atau Kontributor dalam langganan Azure yang Anda gunakan untuk lab ini.

2. Di portal Microsoft Azure, dalam kotak teks **Cari sumber daya, layanan, dan dokumen** di bagian atas halaman portal Microsoft Azure, ketik **Sebarkan template kustom** dan tekan tombol **Enter**.

3. Pada panel **Penyebaran kustom**, klik opsi **Buat templat Anda sendiri di editor**.

4. Pada panel **Edit templat**, klik **Muat file**, cari file **\\Allfiles\\Labs\\08\\template.json** dan klik **Buka**.

    >**Catatan**: Tinjau konten templat dan perhatikan bahwa templat itu menyebarkan Pusat Data mesin virtual Azure yang menghosting Windows Server 2019.

5. Pada panel **Edit templat**, klik **Simpan**.

6. Pada panel **Penyebaran kustom**, pastikan bahwa pengaturan berikut telah dikonfigurasi (biarkan yang lain dengan nilai defaultnya):

   |Pengaturan|Nilai|
   |---|---|
   |Langganan|nama langganan Azure yang akan Anda gunakan di lab ini|
   |Grup sumber daya|klik **Buat baru** dan ketik nama **AZ500LAB08**|
   |Lokasi|**(AS) AS Timur**|

    >**Catatan**: Untuk mengidentifikasi wilayah Azure tempat Anda dapat menyediakan mesin virtual Azure, lihat [ **https://azure.microsoft.com/en-us/regions/offers/** ](https://azure.microsoft.com/en-us/regions/offers/)

7. Klik **Review + create**, lalu klk **Create**.

    >**Catatan**: Tunggu hingga penyebaran selesai. Proses ini memerlukan waktu sekitar 2 menit. 

#### <a name="task-2-deploy-the-azure-firewall"></a>Tugas 2: Sebarkan firewall Azure

Dalam tugas ini Anda akan menyebarkan firewall Azure ke dalam jaringan virtual. 

1. Di portal Azure, di kotak teks **Cari sumber daya, layanan, dan dokumen** di bagian atas laman portal Azure, ketik **Firewall** dan tekan tombol **Enter**.

2. Pada panel **Firewall**, klik **+ Buat**.

3. Pada tab **Dasar-dasar** pada panel **Buat firewall**, tentukan setelan berikut (biarkan yang lain dengan nilai defaultnya):

   |Pengaturan|Nilai|
   |---|---|
   |Grup sumber daya|**AZ500LAB08**|
   |Nama|**Test-FW01**|
   |Wilayah|**(AS) AS Timur**|
   |Tingkat firewall|**Standar**|
   |Manajemen firewall|**Gunakan aturan Firewall (klasik) untuk mengelola firewall ini**|
   |Pilih jaringan virtual|klik opsi **Gunakan yang sudah ada** dan, di daftar drop-down, pilih **Test-FW-VN**|
   |Alamat IP publik|clck **Tambahkan baru** dan ketik nama **TEST-FW-PIP** dan klik **OK**|

4. Klik **Review + create**, lalu klik **Create**. 

    >**Catatan**: Tunggu hingga penyebaran selesai. Proses ini memerlukan waktu sekitar 5 menit. 

5. Di portal Microsoft Azure, di kotak teks **Sumber daya pencarian, layanan, dan dokumen** di bagian atas halaman portal Microsoft Azure, ketik **Grup sumber daya** dan tekan kunci **Enter**.

6. Pada panel **Grup sumber daya**, dalam daftar grup sumber daya, klik entri **AZ500LAB08**.

    >**Catatan**: Pada panel grup sumber daya **AZ500LAB08**, tinjau daftar sumber daya. Anda dapat mengurutkan menurut **Jenis**.

7. Dalam daftar sumber daya, klik entri yang mewakili firewall **Test-FW01**.

8. Pada panel **Test-FW01**, identifikasi alamat **IP Privat** yang ditetapkan ke firewall. 

    >**Catatan**: Anda akan membutuhkan informasi ini di tugas berikutnya.


#### <a name="task-3-create-a-default-route"></a>Tugas 3: Buat rute default

Dalam tugas ini, Anda akan membuat rute default untuk subnet **Workload-SN**. Rute ini akan mengonfigurasi lalu lintas keluar melalui firewall.

1. Di portal Azure, di kotak teks **Cari sumber daya, layanan, dan dokumen** di bagian atas halaman portal Azure, ketik **Tabel rute** dan tekan **Enter** kunci.

2. Pada panel **Tabel rute**, klik **+ Buat**.

3. Pada panel **Buat tabel rute**, tentukan setelan berikut:

   |Pengaturan|Nilai|
   |---|---|
   |Grup sumber daya|**AZ500LAB08**|
   |Wilayah| **US Timur**|
   |Nama|**Firewall-route**|

4. Klik **Tinjau + buat**, lalu klik **Buat**, dan tunggu hingga provisi selesai. 

5. Pada panel **Tabel rute**, klik **Refresh**, dan, dalam daftar tabel rute, klik entri **Rute firewall**.

6. Pada panel **Rute firewall**, di bagian **Pengaturan**, klik **Subnet** lalu, pada panel **Subnet rute \| Firewall**, klik **+ Kaitkan**.

7. Pada panel **Kaitkan subnet**, tentukan pengaturan berikut:

   |Pengaturan|Nilai|
   |---|---|
   |Jaringan virtual|**Test-FW-VN**|
   |Subnet|**Workload-SN**|

    >**Catatan**: Pastikan subnet **Workload-SN** dipilih untuk rute ini, jika tidak, firewall tidak akan berfungsi dengan benar.

8. Klik **OK** untuk mengaitkan firewall ke subnet jaringan virtual. 

9. Kembali ke panel **Rute firewall**, di bagian **Pengaturan**, klik **Rute** lalu klik **+ Tambahkan**. 

10. Pada panel **Tambahkan rute**, tentukan setelan berikut:  

   |Pengaturan|Nilai|
   |---|---|
   |Nama rute|**FW-DG**|
   |Sumber awalan alamat|**Alamat IP**|
   |Alamat IP sumber/rentang CIDR|**0.0.0.0/0**
   |Tipe Lompatan Berikutnya|**Appliance virtual**|
   |Alamat lompatan berikutnya|alamat IP pribadi firewall yang Anda identifikasi di tugas sebelumnya|

    >**Catatan**: Azure Firewall sebenarnya adalah layanan terkelola, tetapi appliance virtual berfungsi dalam situasi ini.
    
11.  Klik **Tambahkan** untuk menambahkan rute. 


#### <a name="task-4-configure-an-application-rule"></a>Tugas 4: Mengonfigurasi aturan aplikasi

Dalam tugas ini Anda akan membuat aturan aplikasi yang memungkinkan akses keluar ke `www.bing.com`.

1. Di portal Azure, navigasikan kembali ke firewall **Test-FW01**.

2. Pada panel **Test-FW01**, di bagian **Pengaturan**, klik **Aturan (klasik)** .

3. Pada panel **Aturan Test-FW01 \| (klasik),** klik tab **Kumpulan aturan aplikasi**, lalu klik **+ Tambahkan kumpulan aturan aplikasi**.

4. Pada panel **Tambahkan kumpulan aturan aplikasi**, tentukan setelan berikut (biarkan yang lain dengan nilai defaultnya):

   |Pengaturan|Nilai|
   |---|---|
   |Nama|**App-Coll01**|
   |Prioritas|**200**|
   |Tindakan|**Izinkan**|

5. Pada panel **Tambahkan kumpulan aturan aplikasi**, buat entri baru di bagian **Target FQDNs** dengan setelan berikut (biarkan yang lain dengan nilai defaultnya):

   |Pengaturan|Nilai|
   |---|---|
   |nama|**AllowGH**|
   |Jenis sumber|**Alamat IP**|
   |Sumber|**10.0.2.0/24**|
   |Port protokol|**http:80, https:443**|
   |Target FQDNS|**www.bing.com**|

6. Klik **Tambahkan** untuk menambahkan aturan aplikasi berbasis FQDN Target.

    >**Catatan**: Azure Firewall menyertakan kumpulan aturan bawaan untuk FQDN infrastruktur yang diizinkan secara default. FQDN ini khusus untuk platform dan tidak dapat digunakan untuk tujuan lain. 

#### <a name="task-5-configure-a-network-rule"></a>Tugas 5: Konfigurasi aturan jaringan

Dalam tugas ini, Anda akan membuat aturan jaringan yang memungkinkan akses keluar ke dua alamat IP pada port 53 (DNS).

1. Di portal Azure, navigasikan kembali ke panel **Test-FW01 \| Rules (klasik)** .

2. Pada panel **Test-FW01 \| Aturan (klasik)** , klik tab **Kumpulan aturan jaringan**, lalu klik **+ Tambahkan kumpulan aturan jaringan**.

3. Pada panel **Tambahkan kumpulan aturan jaringan**, tentukan setelan berikut (biarkan yang lain dengan nilai defaultnya):

   |Pengaturan|Nilai|
   |---|---|
   |Nama|**Net-Coll01**|
   |Prioritas|**200**|
   |Tindakan|**Izinkan**|

4. Pada panel **Tambahkan kumpulan aturan jaringan**, buat entri baru di bagian **Alamat IP** dengan setelan berikut (biarkan yang lain dengan nilai defaultnya):

   |Pengaturan|Nilai|
   |---|---|
   |Nama|**AllowDNS**|
   |Protokol|**UDP**|
   |Jenis sumber|**Alamat IP**|
   |Alamat Sumber|**10.0.2.0/24**|
   |Jenis tujuan|**Alamat IP**|
   |Alamat Tujuan|**209.244.0.3,209.244.0.4**|
   |Port tujuan|**53**|

5. Klik **Tambahkan** untuk menambahkan aturan jaringan.

    >**Catatan**: Alamat tujuan yang digunakan dalam hal ini dikenal sebagai server DNS publik. 

#### <a name="task-6-configure-the-virtual-machine-dns-servers"></a>Tugas 6: Mengonfigurasi server DNS mesin virtual

Dalam tugas ini, Anda akan mengonfigurasi alamat DNS primer dan sekunder untuk mesin virtual. Ini bukan persyaratan firewall. 

1. Di portal Azure, navigasikan kembali ke grup sumber daya **AZ500LAB08**.

2. Pada panel **AZ500LAB08**, dalam daftar sumber daya, klik mesin virtual **Srv-Work**.

3. Pada panel **Srv-Work**, di bagian **Pengaturan**, klik **Jaringan**.

4. Pada panel **Jaringan Srv-Work\|** , klik tautan di samping entri **Antarmuka jaringan**.

5. Pada panel antarmuka jaringan, di bagian **Setelan**, klik **server DNS**, pilih opsi **Kustom**, tambahkan dua server DNS yang dirujuk dalam aturan jaringan: **209.244.0.3** dan **209.244.0.4**, serta klik **Simpan** untuk menyimpan perubahan.

6. Kembali ke halaman mesin virtual **Srv-Work**.

    >**Catatan**: Tunggu pembaharuan selesai.

    >**Catatan**: Memperbarui server DNS untuk antarmuka jaringan akan secara otomatis memulai ulang mesin virtual tempat antarmuka itu terpasang, dan jika berlaku, mesin virtual lain dalam set ketersediaan yang sama.

#### <a name="task-7-test-the-firewall"></a>Tugas 7: Menguji firewall

Dalam tugas ini, Anda akan menguji firewall untuk mengonfirmasi bahwa firewall berfungsi seperti yang diharapkan.

1. Di portal Azure, navigasikan kembali ke grup sumber daya **AZ500LAB08**.

2. Pada panel **AZ500LAB08**, dalam daftar sumber daya, klik mesin virtual **Srv-Jump**.

3. Pada panel **Srv-Jump**, klik **Hubungkan** dan, di menu drop down, klik **RDP**. 

4. Klik **Unduh File RDP** dan gunakan untuk menyambung ke **Srv-Jump** mesin virtual Azure melalui Desktop Jauh. Saat diminta untuk mengautentikasi, berikan kredensial berikut:

   |Pengaturan|Nilai|
   |---|---|
   |Nama pengguna|**localadmin**|
   |Kata sandi|**Pa55w.rd1234**|

    >**Catatan**: Langkah-langkah berikut dilakukan di sesi Desktop Jauh ke mesin virtual Azure **Srv-Jump**. 

    >**Catatan**: Anda akan terhubung ke mesin virtual **Srv-Work**. Ini sedang dilakukan sehingga kita dapat menguji kemampuan untuk mengakses situs web bing.com.  

5. Dalam sesi Desktop Jauh ke **Srv-Jump**, klik kanan **Mulai**, di menu klik kanan, klik **Jalankan**, dan, dari kotak dialog **Jalankan, jalankan** yang berikut ini untuk menyambungkan ke **Srv-Work**. 

    ```
    mstsc /v:Srv-Work
    ```

6. Saat diminta untuk mengautentikasi, berikan kredensial berikut:

   |Pengaturan|Nilai|
   |---|---|
   |Nama pengguna|**localadmin**|
   |Kata sandi|**Pa55w.rd1234**|

    >**Catatan**: Tunggu sesi Remote Desktop dibuat dan antarmuka Server Manager dimuat.

7. Dalam sesi Desktop Jauh ke **Srv-Work**, di **Manajer Server**, klik **Server Lokal** lalu klik **Konfigurasi Keamanan Tingkat Tinggi IE**.

8. Di kotak dialog **Konfigurasi Keamanan yang Ditingkatkan Internet Explorer**, atur kedua opsi menjadi **Off** dan klik **OK**.

9. Dalam sesi Desktop Jauh ke **Srv-Work**, mulai Internet Explorer dan telusuri ke **`https://www.bing.com`** . 

    >**Catatan**: Situs web harus berhasil ditampilkan. Firewall memungkinkan Anda mengakses.

10. Telusuri **`http://www.microsoft.com/`**

    >**Catatan**: Di dalam halaman browser, Anda akan menerima pesan dengan teks yang menyerupai berikut ini: `HTTP request from 10.0.2.4:xxxxx to microsoft.com:80. Action: Deny. No rule matched. Proceeding with default action.` Hal ini diharapkan, karena firewall memblokir akses ke situs web ini. 

11. Hentikan kedua sesi Desktop Jauh.

> Hasil: Anda telah berhasil mengonfigurasi dan menguji Azure Firewall.

**Membersihkan sumber daya**

> Jangan lupa untuk menghapus sumber daya Azure yang baru dibuat dan yang tidak diperlukan lagi. Menghapus sumber daya yang tidak terpakai memastikan Anda tidak akan dikenakan biaya tak terduga. 

1. Di portal Microsoft Azure, buka Cloud Shell dengan mengklik ikon pertama di kanan atas Portal Azure. Jika diminta, klik **PowerShell** dan **Buat penyimpanan**.

2. Pastikan **PowerShell** dipilih di menu dropdown di sudut kiri atas panel Cloud Shell.

3. Di sesi PowerShell dalam panel Cloud Shell, jalankan perintah berikut untuk menghapus grup sumber daya yang Anda buat di lab ini:
  
    ```powershell
    Remove-AzResourceGroup -Name "AZ500LAB08" -Force -AsJob
    ```
4. Tutup panel **Cloud Shell**. 
