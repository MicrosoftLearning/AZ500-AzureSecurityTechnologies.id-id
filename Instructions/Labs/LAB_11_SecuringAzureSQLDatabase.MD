---
lab:
  title: 11 - Mengamankan Database Azure SQL
  module: Module 03 - Secure data and applications
ms.openlocfilehash: 1112086c681d4d81e186ad87a8a01085f42f8490
ms.sourcegitcommit: a8470295248a6363987bd5ea47154fe39f8535c3
ms.translationtype: HT
ms.contentlocale: id-ID
ms.lasthandoff: 03/09/2022
ms.locfileid: "145195951"
---
# <a name="lab-11-securing-azure-sql-database"></a>Lab 11: Mengamankan Database Azure SQL
# <a name="student-lab-manual"></a>Panduan lab siswa

## <a name="lab-scenario"></a>Skenario lab

Anda telah diminta untuk meninjau fitur keamanan untuk database Azure SQL. Secara khusus, Anda tertarik pada:

- Perlindungan terhadap serangan seperti injeksi SQL dan penyelundupan data. 
- Kemampuan untuk menemukan dan mengklasifikasikan informasi database ke dalam kategori seperti Rahasia. 
- Kemampuan untuk mengaudit server basis data dan kueri basis data serta mencatat peristiwa. 

> Untuk semua sumber daya di lab ini, kita menggunakan wilayah **US Timur**. Pastikan pada instruktur Anda bahwa ini adalah wilayah yang akan digunakan untuk kelas. 

## <a name="lab-objectives"></a>Tujuan lab

Di lab ini, Anda akan menyelesaikan latihan berikut:

- Latihan 1: Menerapkan fitur keamanan Microsoft Azure SQL Database

## <a name="securing-azure-sql-database-diagram"></a>Mengamankan diagram Database Azure SQL

![gambar](https://user-images.githubusercontent.com/91347931/157533836-7250fa58-a109-4882-a55b-d3fa3baf34ab.png)

## <a name="instructions"></a>Instruksi

## <a name="lab-files"></a>File lab:

- **\\Allfiles\\Labs\\11\\azuredeploy.json**

### <a name="exercise-1-implement-sql-database-security-features"></a>Latihan 1: Menerapkan fitur keamanan Microsoft Azure SQL Database

### <a name="estimated-timing-30-minutes"></a>Perkiraan waktu: 30 menit

Dalam latihan ini, Anda akan menyelesaikan tugas-tugas berikut:

- Tugas 1: Menyebarkan Azure SQL Database
- Tugas 2: Konfigurasikan Perlindungan Data Tingkat Lanjut
- Tugas 3: Konfigurasikan Klasifikasi Data
- Tugas 4: Mengonfigurasi audit

#### <a name="task-1-deploy-an-azure-sql-database"></a>Tugas 1: Menyebarkan Azure SQL Database

Dalam tugas ini, Anda akan menggunakan template untuk menerapkan infrastruktur lab. 

1. Masuk ke portal Microsoft Azure **`https://portal.azure.com/`** .

    >**Catatan**: Masuk ke portal Microsoft Azure menggunakan akun yang memiliki peran Pemilik atau Kontributor dalam langganan Azure yang Anda gunakan untuk lab ini.

2. Di portal Microsoft Azure, dalam kotak teks **Cari sumber daya, layanan, dan dokumen** di bagian atas halaman portal Microsoft Azure, ketik **Sebarkan template kustom** dan tekan tombol **Enter**.

3. Pada panel **Penyebaran kustom**, klik opsi **Buat template Anda sendiri di penyunting**.

4. Pada panel **Edit template**, klik **Muat file**, cari file **\\Allfiles\\Labs\\11\\azuredeploy.json** dan klik **Buka**.

    >**Catatan**: Tinjau konten templat dan perhatikan bahwa template itu menyebarkan database Azure SQL.

5. Pada panel **Edit template**, klik **Simpan**.

6. Pada panel **Penyebaran kustom**, pastikan bahwa pengaturan berikut telah dikonfigurasi (biarkan opsi yang lain dengan nilai defaultnya):

   |Pengaturan|Nilai|
   |---|---|
   |Langganan|nama langganan Azure yang akan Anda gunakan di lab ini|
   |Grup sumber daya|klik **Buat baru** dan ketik nama **AZ500LAB11**|
   |Lokasi|**(AS) AS Timur**|

7. Klik **Tinjau + Buat**, lalu klik **Buat**.

    >**Catatan**: Tunggu hingga penyebaran selesai. 

#### <a name="task-2-configure-advanced-data-protection"></a>Tugas 2: Konfigurasikan Perlindungan Data Tingkat Lanjut

1. Di portal Microsoft Azure, di kotak teks **Cari sumber daya, layanan, dan dokumen** di bagian atas halaman portal Microsoft Azure, ketik **Grup sumber daya** dan tekan tombol **Enter**.

2. Pada panel **Grup sumber daya**, dalam daftar grup sumber daya, klik entri **AZ500LAB11**.

3. Pada panel **AZ500LAB11**, klik entri yang mewakili SQL Server yang baru dibuat.

4. Pada panel server SQL, di bagian **Keamanan**, klik **Microsoft Defender untuk Cloud**. Gulir ke bagian bawah laman **Memulai** dan pilih **Tingkatkan versi**. Pada tab **Pasang agen** pilih **Pasang agen**.

5. Pada panel server SQL, di bagian **Keamanan**, klik **Microsoft Defender untuk Cloud**, pilih **Aktifkan Microsoft Defender for SQL**.

      >**Catatan**: Tunggu hingga pemberitahuan menunjukkan Azure Defender for SQL telah berhasil diaktifkan.

6. Di panel server SQL, di bagian **Keamanan**, di laman **Microsoft Defender untuk Cloud**, di **Microsoft Defender for SQL: Aktifkan pada parameter tingkat server (Konfigurasi)** , klik **(konfigurasi)** .   

      >**Catatan**: Segarkan browser jika **(konfigurasi)** tidak ditampilkan.
    
7. Pada bilah **Pengaturan Server**, tinjau informasi tentang harga dan periode uji coba, **PENGATURAN PENILAIAN KERENTANAN**, dan **PENGATURAN PERLINDUNGAN ANCAMAN TINGKAT LANJUT**.

8. Kembali ke panel **Microsoft Defender untuk Cloud**, tinjau **Rekomendasi** dan **Peringatan keamanan**.

      >**Catatan**: Mungkin diperlukan waktu 10-15 menit agar rekomendasi muncul di panel **Microsoft Defender untuk Cloud**. Daripada menunggu, lanjutkan ke tugas berikutnya tetapi pertimbangkan untuk kembali ke panel ini setelah Anda menyelesaikan semua tugas yang tersisa.


#### <a name="task-3-configure-data-classification"></a>Tugas 3: Konfigurasikan Klasifikasi Data

Dalam tugas ini, Anda akan menemukan dan mengklasifikasikan informasi dalam database SQL untuk kepatuhan GPDR dan perlindungan data.

1. Pada panel server SQL, di bagian **Pengaturan**, klik **Database SQL**.

2. Dalam daftar database, pilih entri **AZ500LabDb**. 

3. Pada panel database SQL **AZ500LabDb**, di bagian **Keamanan**, klik **Penemuan & Klasifikasi Data**.

4. Pada panel **Penemuan & Klasifikasi Data**, klik tab **Klasifikasi**.

    >**Catatan**: Mesin klasifikasi memindai database Anda untuk kolom yang berisi data yang berpotensi sensitif dan memberikan daftar klasifikasi kolom yang direkomendasikan.

5. Klik pesan teks **Kami telah menemukan 15 kolom dengan rekomendasi klasifikasi** yang ditampilkan pada panel biru di bagian atas panel.

6. Tinjau kolom yang tercantum dan label sensitivitas yang direkomendasikan. 

7. Aktifkan kotak centang **Pilih semua**, lalu klik **Terima Rekomendasi yang Dipilih**.

    >**Catatan**: Atau, Anda hanya dapat memilih kolom tertentu dan mengabaikan yang lain. 

    >**Catatan**: Anda memiliki opsi untuk mengubah jenis informasi dan label sensitivitas. 

8. Setelah Anda menyelesaikan peninjauan, klik **Simpan**. 

    >**Catatan**: Ini akan menyelesaikan klasifikasi dan terus melabeli kolom database dengan metadata klasifikasi baru. 

9. Kembali ke tab **Gambaran Umum** panel **Penemuan & Klasifikasi Data**, perhatikan bahwa telah diperbarui untuk memperhitungkan informasi klasifikasi terbaru. 

#### <a name="task-4--configure-auditing"></a>Tugas 4 : Mengonfigurasi audit 

Dalam tugas ini, Anda akan terlebih dahulu mengonfigurasi audit tingkat server lalu mengonfigurasi audit tingkat database. 

1. Di portal Azure, navigasikan kembali ke panel SQL Server.

2. Pada panel SQL Server, di bagian **Keamanan**, klik **Audit**.

    >**Catatan**: Ini adalah audit tingkat server. Pengaturan audit default mencakup semua kueri dan prosedur tersimpan yang dijalankan terhadap database, serta login yang berhasil dan gagal.

3. Atur tombol beralih **Aktifkan Azure SQL Auditing** ke **ON** untuk mengaktifkan audit. 

4. Pilih kotak centang **Penyimpanan** dan kotak entri untuk **Langganan** dan **Akun Penyimpanan** akan ditampilkan.

5. Pilih **Langganan** Anda dari daftar dropdown.

6. Klik **Akun penyimpanan** dan pilih **Buat baru**.

7. Pada panel **Buat akun penyimpanan**, dalam kotak **Nama**, ketik nama unik global yang terdiri dari antara 3 dan 24 huruf kecil dan angka, klik **OK**, 

    >**Catatan**: Anda mungkin perlu merefresh browser sebelum akun penyimpanan tersedia.

8. Kembali ke panel **Auditing**, di bawah **Properti lanjutan** atur **Retensi (hari)** ke **5**. 

9. Pada panel Audit, klik **Simpan** untuk menyimpan pengaturan audit. 

    >**Catatan**: Jika Anda menerima pesan kesalahan mengenai jalur kontainer penyimpanan yang tidak valid, maka akun penyimpanan mungkin belum disediakan. Tunggu beberapa menit, klik **Akun penyimpanan**, pada panel **Pilih akun penyimpanan**, pilih akun penyimpanan yang baru dibuat dan kembali pada panel Audit, klik **Simpan**.

10. Pada panel server, di bagian **Pengaturan**, klik **Database SQL**.

11. Dalam daftar database, pilih entri **AZ500LabDb**. 

12. Pada panel database SQL **AZ500LabDb**, di bagian **Keamanan**, klik **Auditing**. 

    >**Catatan**: Ini adalah audit tingkat basis data. Audit tingkat server sudah diaktifkan. 
  
    >**Catatan**: Audit dapat ditulis ke akun penyimpanan Azure, ke ruang kerja Analitik Log, atau ke Pusat Aktivitas. Anda dapat mengonfigurasi kombinasi apa pun dari opsi ini.

    >**Catatan**: Jika audit berbasis penyimpanan diaktifkan di server, audit akan selalu berlaku untuk database, terlepas dari pengaturan database.

13. Klik **Lihat Log Audit**.

14. Pada panel **Catatan audit**, perhatikan bahwa Anda dapat beralih antara Audit server dan audit Database. 

    >**Catatan**: Karena server dan database SQL ini dibuat baru-baru ini, kecil kemungkinan peristiwa apa pun akan tersedia pada saat ini. 

> Hasil: Anda telah membuat server dan database SQL, mengonfigurasi klasifikasi data, dan mengaudit. 


**Membersihkan sumber daya**

> Jangan lupa untuk menghapus sumber daya Azure yang baru dibuat dan yang tidak diperlukan lagi. Menghapus sumber daya yang tidak terpakai memastikan Anda tidak akan dikenakan biaya tidak terduga. 

1. Di portal Microsoft Azure, buka Cloud Shell dengan mengklik ikon pertama di kanan atas Portal Microsoft Azure. Jika diminta, klik **PowerShell** dan **Buat penyimpanan**.

2. Pastikan **PowerShell** dipilih di menu drop-down di sudut kiri atas panel Cloud Shell.

3. Di sesi PowerShell dalam panel Cloud Shell, jalankan perintah berikut untuk menghapus grup sumber daya yang Anda buat di lab ini:
  
    ```powershell
    Remove-AzResourceGroup -Name "AZ500LAB11" -Force -AsJob
    ```
4. Tutup panel **Cloud Shell**. 
