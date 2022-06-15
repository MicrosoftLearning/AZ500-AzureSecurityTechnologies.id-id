---
lab:
  title: 10 - Key Vault (Menerapkan Data Aman dengan mengatur Always Encrypted)
  module: Module 03 - Secure Data and Applications
ms.openlocfilehash: c31dd6e930e0f1d1b82e7c6ea502bb6fa51a7dd7
ms.sourcegitcommit: 967cb50981ef07d731dd7548845a38385b3fb7fb
ms.translationtype: HT
ms.contentlocale: id-ID
ms.lasthandoff: 05/31/2022
ms.locfileid: "145955382"
---
# <a name="lab-10-key-vault-implementing-secure-data-by-setting-up-always-encrypted"></a>Lab 10: Key Vault (Menerapkan Data Aman dengan mengatur Always Encrypted)
# <a name="student-lab-manual"></a>Panduan lab siswa

## <a name="lab-scenario"></a>Skenario lab

Anda telah diminta untuk membuat aplikasi bukti konsep yang menggunakan dukungan Azure SQL Database untuk fungsionalitas Always Encrypted. Semua rahasia dan kunci yang digunakan dalam skenario ini harus disimpan di Key Vault. Aplikasi harus terdaftar di Azure Active Directory (AAD) untuk meningkatkan postur keamanannya. Untuk mencapai tujuan ini, bukti konsep harus mencakup:

- Membuat Azure Key Vault dan menyimpan kunci serta rahasia di dalam vault.
- Membuat SQL Database dan mengenkripsi konten kolom dalam tabel database menggunakan Always Encrypted.

>**Catatan**: Untuk semua sumber daya di lab ini, kita menggunakan wilayah **US Timur**. Pastikan pada instruktur Anda bahwa ini adalah wilayah yang akan digunakan untuk kelas. 

Untuk tetap fokus pada aspek keamanan Azure, terkait dengan membangun bukti konsep ini, Anda akan memulai dari penyebaran template ARM otomatis, menyiapkan Mesin Virtual dengan Visual Studio 2019 dan SQL Server Management Studio 2018.

## <a name="lab-objectives"></a>Tujuan lab

Di lab ini, Anda akan menyelesaikan latihan berikut:

- Latihan 1: Menyebarkan infrastruktur dasar dari template ARM
- Latihan 2: Mengonfigurasi sumber daya Key Vault dengan kunci dan rahasia
- Latihan 3: Mengonfigurasi database Azure SQL dan aplikasi berbasis data
- Latihan 4: Mendemonstrasikan penggunaan Azure Key Vault dalam mengenkripsi database Azure SQL

## <a name="key-vault-diagram"></a>Diagram Key Vault

![gambar](https://user-images.githubusercontent.com/91347931/157532938-c724cc40-f64f-4d69-9e91-d75344c5e0a2.png)

## <a name="instructions"></a>Instruksi

## <a name="lab-files"></a>File lab:

- **\\Allfiles\\Labs\\10\\az-500-10_azuredeploy.json**

- **\\Allfiles\\Labs\\10\\program.cs**

### <a name="total-lab-time-estimate-60-minutes"></a>Total perkiraan Waktu Lab: 60 menit

### <a name="exercise-1-deploy-the-base-infrastructure-from-an-arm-template"></a>Latihan 1: Menyebarkan infrastruktur dasar dari template ARM

Dalam latihan ini, Anda akan menyelesaikan tugas-tugas berikut:

- Tugas 1: Menyebarkan Mesin Virtual Azure dan database Azure SQL

#### <a name="task-1-deploy-an-azure-vm-and-an-azure-sql-database"></a>Tugas 1: Menyebarkan Mesin Virtual Azure dan database Azure SQL

Dalam tugas ini, Anda akan menyebarkan Mesin Virtual Azure, yang secara otomatis akan menginstal Visual Studio 2019 dan SQL Server Management Studio 2018 sebagai bagian dari penyebaran. 

1. Masuk ke portal Microsoft Azure **`https://portal.azure.com/`** .

    >**Catatan**: Masuk ke portal Microsoft Azure menggunakan akun yang memiliki peran Pemilik atau Kontributor dalam langganan Azure yang Anda gunakan untuk lab ini.

2. Di portal Microsoft Azure, dalam kotak teks **Cari sumber daya, layanan, dan dokumen** di bagian atas halaman portal Microsoft Azure, ketik **Sebarkan template kustom** dan tekan tombol **Enter**.

3. Pada panel **Penyebaran kustom**, klik opsi **Buat template Anda sendiri di penyunting**.

4. Pada panel **Edit template**, klik **Muat file**, cari file **\\Allfiles\\Labs\\10\\az-500-10_azuredeploy.json** dan klik **Buka**.

5. Pada panel **Edit template**, klik **Simpan**.

6. Pada panel **Penyebaran kustom**, pada **Cakupan Penyebaran** pastikan bahwa pengaturan berikut telah dikonfigurasi (biarkan yang lain tetap dengan nilai default):

   |Pengaturan|Nilai|
   |---|---|
   |Langganan|nama langganan Azure yang akan Anda gunakan di lab ini|
   |Grup sumber daya|klik **Buat baru** dan ketik nama **AZ500LAB10**|
   |Lokasi|**(AS) AS Timur**|
   |Nama Pengguna Admin|**Siswa**|
   |Kata Sandi Admin|**Gunakan kata sandi pribadi Anda yang dibuat di Lab 04 > Latihan 1 > Tugas 1 > Langkah 9.**|
   
    >**Catatan**: Meskipun Anda dapat mengubah kredensial administratif yang digunakan untuk masuk ke Mesin Virtual, Anda tidak perlu melakukannya.

    >**Catatan**: Untuk mengidentifikasi wilayah Azure tempat Anda dapat memprovisikan Mesin Virtual Azure, lihat [ **https://azure.microsoft.com/en-us/regions/offers/** ](https://azure.microsoft.com/en-us/regions/offers/)

7. Klik tombol **Tinjau dan Buat**, dan konfirmasikan penyebaran dengan mengklik tombol **Buat**. 

    >**Catatan**: Hal ini memulai penyebaran Mesin Virtual Azure dan Azure SQL Database yang diperlukan untuk lab ini. 

    >**Catatan**: Jangan menunggu penyebaran template ARM selesai, tetapi lanjutkan ke latihan berikutnya. Penyebaran mungkin memakan waktu antara **20-25 menit**. 

### <a name="exercise-2-configure-the-key-vault-resource-with-a-key-and-a-secret"></a>Latihan 2: Mengonfigurasi sumber daya Key Vault dengan kunci dan rahasia

>**Catatan**: Untuk semua sumber daya di lab ini, kita menggunakan wilayah **Timur (US)** . Pastikan pada instruktur Anda bahwa ini adalah wilayah yang akan digunakan untuk kelas Anda. 

Dalam latihan ini, Anda akan menyelesaikan tugas-tugas berikut:

- Tugas 1: Membuat dan mengonfigurasi Key Vault
- Tugas 2: Menambahkan kunci ke Key Vault
- Tugas 3: Menambahkan rahasia ke Key Vault

#### <a name="task-1-create-and-configure-a-key-vault"></a>Tugas 1: Membuat dan mengonfigurasi Key Vault

Dalam tugas ini, Anda akan membuat sumber daya Azure Key Vault. Anda juga akan mengonfigurasi izin Azure Key Vault.

1. Buka Cloud Shell dengan mengklik ikon pertama (di sebelah bilah pencarian) di kanan atas portal Microsoft Azure. Jika diminta, pilih **PowerShell** dan **Buat penyimpanan**.

2. Pastikan **PowerShell** dipilih di menu drop-down di sudut kiri atas panel Cloud Shell.

3. Di sesi PowerShell dalam panel Cloud Shell, jalankan yang berikut ini untuk membuat Azure Key Vault di grup sumber daya **AZ500LAB10**. (Jika Anda memilih nama lain untuk Grup Sumber Daya lab ini dari Tugas 1, gunakan nama tersebut untuk tugas ini juga). Nama Key Vault harus unik. Ingat nama yang telah Anda pilih. Anda akan membutuhkan nama tersebut di seluruh lab ini.  

    ```powershell
    $kvName = 'az500kv' + $(Get-Random)

    $location = (Get-AzResourceGroup -ResourceGroupName 'AZ500LAB10').Location

    New-AzKeyVault -VaultName $kvName -ResourceGroupName 'AZ500LAB10' -Location $location
    ```

    >**Catatan**: Output dari perintah terakhir akan menampilkan nama vault dan URI vault. URI vault dalam format `https://<vault_name>.vault.azure.net/`

4. Tutup panel Cloud Shell. 

5. Di portal Microsoft Azure, di kotak teks **Cari sumber daya, layanan, dan dokumen** di bagian atas halaman portal Microsoft Azure, ketik **Grup sumber daya** dan tekan tombol **Enter**.

6. Pada panel **Grup sumber daya**, dalam daftar grup sumber daya, klik entri **AZ500LAB10** (atau nama lain yang Anda pilih sebelumnya untuk grup sumber daya).

7. Pada panel Grup Sumber Daya, klik entri yang mewakili Key Vault yang baru dibuat. 

8. Pada panel Key Vault, di bagian **Pengaturan**, klik **Kebijakan Akses**, lalu klik **+ Tambahkan Kebijakan Akses**.

9. Pada panel **Tambahkan kebijakan akses**, tentukan pengaturan berikut (biarkan yang lainnya diatur ke nilai default): 

    |Pengaturan|Nilai|
    |----|----|
    |Mengonfigurasi dari template (opsional)|**Manajemen Kunci, Rahasia, & Sertifikat**|
    |Izin kunci|klik **Pilih semua** menghasilkan **17 izin yang dipilih** (Pastikan izin untuk **Operasi Kebijakan Rotasi** **tidak dicentang**) |
    |Izin rahasia|klik **Pilih semua** menghasilkan total **8 izin yang dipilih**|
    |Izin sertifikasi|klik **Pilih semua** menghasilkan total **16 izin yang dipilih**|
    |Pilih utama|klik **Tidak ada yang dipilih**, pada panel **Prinsipal**, pilih akun pengguna Anda, dan klik **Pilih**|

10. Kembali pada panel **Tambahkan kebijakan akses**, klik **Tambahkan** untuk menambahkan kebijakan akses dan, kembali pada panel kebijakan akses Key Vault, klik **Simpan** pada perubahan. 

#### <a name="task-2-add-a-key-to-key-vault"></a>Tugas 2: Menambahkan kunci ke Key Vault

Dalam tugas ini, Anda akan menambahkan kunci ke Key Vault dan melihat informasi tentang kunci tersebut. 

1. Di portal Microsoft Azure, buka sesi PowerShell di panel Cloud Shell.

2. Pastikan **PowerShell** dipilih di menu menurun di sebelah kiri atas panel Cloud Shell.

3. Di sesi PowerShell dalam panel Cloud Shell, jalankan yang berikut ini untuk menambahkan kunci yang dilindungi perangkat lunak ke Key Vault: 

    ```powershell
    $kv = Get-AzKeyVault -ResourceGroupName 'AZ500LAB10'

    $key = Add-AZKeyVaultKey -VaultName $kv.VaultName -Name 'MyLabKey' -Destination 'Software'
    ```

    >**Catatan**: Nama kuncinya adalah **MyLabKey**

4. Di sesi PowerShell dalam panel Cloud Shell, jalankan yang berikut ini untuk memverifikasi bahwa kunci telah dibuat:

    ```powershell
    Get-AZKeyVaultKey -VaultName $kv.VaultName
    ```

5. Di sesi PowerShell dalam panel Cloud Shell, jalankan yang berikut ini untuk menampilkan pengidentifikasi kunci:

    ```powershell
    $key.key.kid
    ```

6. Kecilkan panel Cloud Shell. 

7. Kembali ke portal Microsoft Azure, pada panel Key Vault, di bagian **Pengaturan**, klik **Kunci**.

8. Dalam daftar kunci, klik entri **MyLabKey** lalu, pada panel **MyLabKey**, klik entri yang mewakili versi kunci saat ini.

    >**Catatan**: Periksa informasi tentang kunci yang Anda buat.

    >**Catatan**: Anda dapat mereferensikan kunci apa pun dengan menggunakan pengidentifikasi kunci. Untuk mendapatkan versi terbaru, referensikan `https://<key_vault_name>.vault.azure.net/keys/MyLabKey` atau dapatkan versi tertentu dengan: `https://<key_vault_name>.vault.azure.net/keys/MyLabKey/<key_version>`


#### <a name="task-3-add-a-secret-to-key-vault"></a>Tugas 3: Menambahkan Rahasia ke Key Vault

1. Beralih kembali ke panel Cloud Shell.

2. Di sesi PowerShell dalam panel Cloud Shell, jalankan yang berikut ini untuk membuat variabel dengan nilai string aman:

    ```powershell
    $secretvalue = ConvertTo-SecureString 'Pa55w.rd1234' -AsPlainText -Force
    ```

3.  Di sesi PowerShell dalam panel Cloud Shell, jalankan yang berikut ini untuk menambahkan rahasia ke vault:

    ```powershell
    $secret = Set-AZKeyVaultSecret -VaultName $kv.VaultName -Name 'SQLPassword' -SecretValue $secretvalue
    ```

    >**Catatan**: Nama rahasianya adalah SQLPassword. 

4.  Di sesi PowerShell dalam panel Cloud Shell, jalankan yang berikut ini untuk memverifikasi bahwa rahasia telah dibuat.

    ```powershell
    Get-AZKeyVaultSecret -VaultName $kv.VaultName
    ```

5. Kecilkan panel Cloud Shell. 

6. Di portal Microsoft Azure, navigasikan kembali ke panel Key Vault, di bagian **Pengaturan**, klik **Rahasia**.

7. Dalam daftar rahasia, klik entri **SQLPassword** lalu, pada panel **SQLPassword**, klik entri yang mewakili versi rahasia saat ini.

    >**Catatan**: Periksa informasi tentang rahasia yang Anda buat.

    >**Catatan**: Untuk mendapatkan versi rahasia terbaru, referensikan `https://<key_vault_name>.vault.azure.net/secrets/<secret_name>` atau dapatkan versi tertentu, referensikan `https://<key_vault_name>.vault.azure.net/secrets/<secret_name>/<secret_version>`


### <a name="exercise-3-configure-an-azure-sql-database-and-a-data-driven-application"></a>Latihan 3: Mengonfigurasi database Azure SQL dan aplikasi berbasis data

Dalam latihan ini, Anda akan menyelesaikan tugas-tugas berikut:

- Tugas 1: Mengaktifkan aplikasi klien untuk mengakses layanan Azure SQL Database.
- Tugas 2: Membuat kebijakan yang mengizinkan aplikasi mengakses Key Vault.
- Tugas 3: Mengambil String Koneksi ADO.NET database Azure SQL 
- Tugas 4: Masuk ke Mesin Virtual Azure yang menjalankan Visual Studio 2019 dan SQL Management Studio 2018
- Tugas 5: Membuat tabel di SQL Database dan memilih kolom data untuk enkripsi


#### <a name="task-1-enable-a-client-application-to-access-the-azure-sql-database-service"></a>Tugas 1: Mengaktifkan aplikasi klien untuk mengakses layanan Azure SQL Database. 

Dalam tugas ini, Anda akan mengaktifkan aplikasi klien untuk mengakses layanan Azure SQL Database. Hal ini akan dilakukan dengan mengatur autentikasi yang diperlukan dan mendapatkan ID Aplikasi dan Rahasia yang Anda perlukan untuk mengautentikasi aplikasi Anda. T

1. Di portal Microsoft Azure, dalam kotak teks **Cari sumber daya, layanan, dan dokumen** di bagian atas halaman portal Microsoft Azure, ketik **Pendaftaran Aplikasi** dan tekan tombol **Enter**.

2. Pada panel **Pendaftaran Aplikasi**, klik **+ Pendaftaran baru**. 

3. Pada panel **Daftarkan aplikasi**, tentukan pengaturan berikut (biarkan yang lainnya diatur ke nilai default):

    |Pengaturan|Nilai|
    |----|----|
    |Nama|**sqlApp**|
    |Pengalihan URI (opsional)|**Web** dan **https://sqlapp**|

4. Pada panel **Daftarkan aplikasi**, klik **Daftarkan**. 

    >**Catatan**: Setelah pendaftaran selesai, browser akan secara otomatis mengarahkan Anda ke panel **sqlApp**. 

5. Pada panel **sqlApp**, identifikasi nilai **ID Aplikasi (klien)** . 

    >**Catatan**: Catat nilai ini. Anda akan membutuhkannya di tugas berikutnya.

6. Pada panel **sqlApp**, di bagian **Kelola**, klik **Sertifikat & rahasia**.

7. Di panel **sqlApp | Sertifikat & rahasia** / bagian **Rahasia Klien**, klik **+ Rahasia klien baru**

8. Di panel **Tambahkan rahasia klien**, tentukan pengaturan berikut:

    |Pengaturan|Nilai|
    |----|----|
    |Deskripsi|**Key1**|
    |Kedaluwarsa|**12 bulan**|
    
9. Klik **Tambahkan** untuk memperbarui kredensial aplikasi.

10. Di panel **sqlApp | Sertifikat & rahasia**, identifikasi nilai **Key1**.

    >**Catatan**: Catat nilai ini. Anda akan membutuhkannya di tugas berikutnya. 

    >**Catatan**: Pastikan untuk menyalin nilai *sebelum* Anda menavigasi keluar dari panel. Setelah Anda melakukannya, tidak mungkin lagi untuk mengambil nilai teks yang jelas.


#### <a name="task-2-create-a-policy-allowing-the-application-access-to-the-key-vault"></a>Tugas 2: Membuat kebijakan yang mengizinkan aplikasi mengakses Key Vault.

Dalam tugas ini, Anda akan memberikan izin aplikasi yang baru didaftarkan untuk mengakses rahasia yang disimpan di Key Vault.

1. Di portal Microsoft Azure, buka sesi PowerShell di panel Cloud Shell.

2. Pastikan **PowerShell** dipilih di menu menurun di sebelah kiri atas panel Cloud Shell.

3. Di sesi PowerShell dalam panel Cloud Shell, jalankan yang berikut ini untuk membuat variabel yang menyimpan **ID Aplikasi (klien)** yang Anda catat di tugas sebelumnya (ganti tempat penampung `<Azure_AD_Application_ID>` dengan nilai **ID Aplikasi (klien)** ):
   
    ```powershell
    $applicationId = '<Azure_AD_Application_ID>'
    ```
4. Di sesi PowerShell dalam panel Cloud Shell, jalankan yang berikut ini untuk membuat variabel yang menyimpan nama Key Vault.
    ```
    $kvName = (Get-AzKeyVault -ResourceGroupName 'AZ500LAB10').VaultName

    $kvName
    ```

5. Di sesi PowerShell dalam panel Cloud Shell, jalankan yang berikut ini untuk memberikan izin di Key Vault ke aplikasi yang Anda daftarkan di tugas sebelumnya:

    ```powershell
    Set-AZKeyVaultAccessPolicy -VaultName $kvName -ResourceGroupName AZ500LAB10 -ServicePrincipalName $applicationId -PermissionsToKeys get,wrapKey,unwrapKey,sign,verify,list
    ```

6. Tutup panel Cloud Shell. 


#### <a name="task-3-retrieve-sql-azure-database-adonet-connection-string"></a>Tugas 3: Mengambil String Koneksi ADO.NET database Azure SQL 

Penyebaran template ARM di Latihan 1 memprovisikan instans Azure SQL Server dan database Azure SQL bernama **medis** . Anda akan memperbarui sumber database kosong dengan struktur tabel baru dan memilih kolom data untuk enkripsi

1. Di portal Microsoft Azure, dalam kotak teks **Cari sumber daya, layanan, dan dokumen** di bagian atas halaman portal Microsoft Azure, ketik **database SQL** dan tekan tombol **Enter**.

2. Dalam daftar database SQL, klik entri **medis(<randomsqlservername>)** .

    >**Catatan**: Jika database tidak dapat ditemukan, kemungkinan ini berarti penyebaran yang Anda mulai di Latihan 1 belum selesai. Anda dapat memvalidasi ini dengan menelusuri Grup Sumber Daya Azure "AZ500LAB10" (atau nama yang Anda pilih), dan memilih **Penyebaran** dari panel Pengaturan.  

3. Pada panel database SQL, di bagian **Pengaturan**, klik **String koneksi**. 

    >**Catatan**: Antarmuka mencakup string koneksi untuk ADO.NET, JDBC, ODBC, PHP, dan Go. 
   
4. Catat **String Koneksi ADO.NET**. Anda akan membutuhkannya nanti.

    >**Catatan**: Saat Anda menggunakan string koneksi, pastikan untuk mengganti tempat penampung `{your_password}` dengan kata sandi yang Anda konfigurasikan dengan penyebaran di Latihan 1.

#### <a name="task-4-log-on-to-the-azure-vm-running-visual-studio-2019-and-sql-management-studio-2018"></a>Tugas 4: Masuk ke Mesin Virtual Azure yang menjalankan Visual Studio 2019 dan SQL Management Studio 2018

Dalam tugas ini, Anda masuk ke Mesin Virtual Azure, yang penyebarannya Anda mulai di Latihan 1. Mesin Virtual Azure ini menghosting Visual Studio 2019 dan SQL Server Management Studio 2018.

    >**Note**: Before you proceed with this task, ensure that the deployment you initiated in the first exercise has completed successfully. You can validate this by navigating to the blade of the Azure resource group "Az500Lab10" (or other name you chose) and selecting **Deployments** from the Settings pane.  

1. Di portal Microsoft Azure, dalam kotak teks **Cari sumber daya, layanan, dan dokumen** di bagian atas halaman portal Microsoft Azure, ketik **mesin virtual** dan tekan tombol **Enter**.

2. Dalam daftar Virtual Machines yang ditampilkan, pilih entri **az500-10-vm1**. Pada panel **az500-10-vm1**, pada panel **Essentials**, catat **Alamat IP publik**. Anda akan menggunakan ini nanti. 

#### <a name="task-5-create-a-table-in-the-sql-database-and-select-data-columns-for-encryption"></a>Tugas 5: Membuat tabel di SQL Database dan memilih kolom data untuk enkripsi

Dalam tugas ini, Anda akan terhubung ke SQL Database dengan SQL Server Management Studio dan membuat tabel. Anda kemudian akan mengenkripsi dua kolom data menggunakan kunci yang dibuat secara otomatis dari Azure Key Vault. 

1. Di portal Microsoft Azure, navigasikan ke panel database SQL **medis**, di bagian **Essentials**, identifikasi **Nama server** (salin ke clipboard), lalu, di bar alat, klik **Atur firewall server**.  

    >**Catatan**: Catat nama servernya. Anda akan memerlukan nama server nanti dalam tugas ini.

2. Pada panel **Pengaturan firewall**, gulir ke bawah ke **Nama Aturan**, dan tentukan pengaturan berikut: 

    |Pengaturan|Nilai|
    |---|---|
    |Nama Aturan|**Mengizinkan Mesin Virtual Mgmt**|
    |Awal IP|Alamat IP Publik az500-10-vm1|
    |Akhir IP|Alamat IP Publik az500-10-vm1|

3. Klik **Simpan** dan **OK** untuk menyimpan perubahan dan menutup panel konfirmasi. 

    >**Catatan**: Hal ini mengubah pengaturan firewall server, memungkinkan koneksi ke database medis dari alamat IP publik Mesin Virtual Azure yang Anda sebarkan di lab ini.

4. Navigasikan kembali ke panel **az500-10-vm1**, klik **Gambaran Umum**, kemudian klik **Hubungkan** dan, di menu drop down, klik **RDP**. 

5. Klik **Unduh File RDP** dan gunakan untuk menghubungkan ke Mesin Virtual Azure **az500-10-vm1** melalui Desktop Jauh. Saat diminta untuk mengautentikasi, berikan kredensial berikut:

    |Pengaturan|Nilai|
    |---|---|
    |Nama pengguna|**Siswa**|
    |Kata sandi|**Gunakan kata sandi pribadi Anda yang dibuat di Lab 04 > Latihan 1 > Tugas 1 > Langkah 9.**|

    >**Catatan**: Tunggu sesi Desktop Jauh dan **Manajer Server** dimuat. Tutup Manajer Server. 

    >**Catatan**: Langkah selanjutnya di lab ini dilakukan dalam sesi Desktop Jauh ke Mesin Virtual Azure **az500-10-vm1**. 

6. Klik **Mulai**, di menu **Mulai**, luaskan folder **Alat Microsoft SQL 18**, dan klik item menu **Micosoft SQL Server Management Studio**.

7. Di kotak dialog **Hubungkan ke Server**, tentukan pengaturan berikut: 

    |Pengaturan|Nilai|
    |---|---|
    |Jenis Server|**Mesin Database**|
    |Nama Server|nama server yang Anda identifikasi sebelumnya dalam tugas ini|
    |Autentikasi|**Autentikasi SQL Server**|
    |Masuk|**Siswa**|
    |Kata sandi|**Gunakan kata sandi pribadi Anda yang dibuat di Lab 04 > Latihan 1 > Tugas 1 > Langkah 9.**|

8. Di kotak dialog **Hubungkan ke Server**, klik **Hubungkan**.

9. Dalam konsol **SQL Server Management Studio**, di panel **Object Explorer**, luaskan folder **Database**.

10. Di panel **Object Explorer**, klik kanan database **medis** dan klik **Kueri Baru**.

11. Tempel kode berikut ke dalam jendela kueri dan klik **Jalankan**. Ini akan membuat tabel **Pasien**.

     ```sql
     CREATE TABLE [dbo].[Patients](
        [PatientId] [int] IDENTITY(1,1),
        [SSN] [char](11) NOT NULL,
        [FirstName] [nvarchar](50) NULL,
        [LastName] [nvarchar](50) NULL,
        [MiddleName] [nvarchar](50) NULL,
        [StreetAddress] [nvarchar](50) NULL,
        [City] [nvarchar](50) NULL,
        [ZipCode] [char](5) NULL,
        [State] [char](2) NULL,
        [BirthDate] [date] NOT NULL 
     PRIMARY KEY CLUSTERED ([PatientId] ASC) ON [PRIMARY] );
     ```
12. Setelah tabel berhasil dibuat, di panel **Object Explorer**, luaskan node database **medis**, node **tabel**, klik kanan node **dbo.Patients**, dan klik **Enkripsi Kolom**. 

    >**Catatan**: Ini akan memulai wizard **Always Encrypted**.

13. Pada halaman **Pengantar**, klik **Berikutnya**.

14. Pada halaman **Pilihan Kolom**, pilih kolom **SSN** dan **Tanggal Lahir**, atur **Jenis Enkripsi** dari kolom **SSN** ke **Deterministik** dan kolom **Tanggal Lahir** ke **Acak**, dan klik **Berikutnya**.

    >**Catatan**: Saat melakukan enkripsi jika terdapat kesalahan yang muncul seperti **Pengecualian telah dimunculkan oleh target pemanggilan** terkait dengan **Rotary(Microsoft.SQLServer.Management.ServiceManagement)** maka pastikan nilai **Izin Kunci** dari **Operasi Kebijakan Rotasi** adalah **tidak dicentang**, jika tidak ada di portal Microsoft Azure, navigasikan ke **Key Vault** >> **Kebijakan Akses** >> **Izin Kunci** >> Hapus centang semua nilai pada **Operasi Kebijakan Rotasi** >> Pada **Operasi Kunci Hak Istimewa** >> Hapus centang **Rilis**.

15. Pada halaman **Konfigurasi Kunci Utama**, pilih **Azure Key Vault**, klik **Masuk**, saat diminta, autentikasi dengan menggunakan akun pengguna yang sama dengan yang Anda gunakan untuk memprovisikan instans Azure Key Vault sebelumnya di lab ini, pastikan bahwa Key Vault muncul di daftar drop down **Pilih Azure Key Vault**, dan klik **Berikutnya**.

16. Pada halaman **Jalankan Pengaturan**, klik **Berikutnya**.
    
17. Pada halaman **Ringkasan**, klik **Selesai** untuk melanjutkan enkripsi. Saat diminta, masuk lagi dengan menggunakan akun pengguna yang sama yang Anda gunakan untuk memprovisikan instans Azure Key Vault sebelumnya di lab ini.

18. Setelah proses enkripsi selesai, pada halaman **Hasil**, klik **Tutup**.

19. Di konsol **SQL Server Management Studio**, di panel **Object Explorer**, pada node **medis**, luaskan subnode **Keamanan** dan **Kunci Always Encrypted**. 

    >**Catatan**: Subnode **Kunci Always Encrypted** berisi subfolder **Kunci Master Kolom** dan **Kunci Enkripsi Kolom**.


### <a name="exercise-4-demonstrate-the-use-of-azure-key-vault-in-encrypting-the-azure-sql-database"></a>Latihan 4: Mendemonstrasikan penggunaan Azure Key Vault dalam mengenkripsi database Azure SQL

Dalam latihan ini, Anda akan menyelesaikan tugas-tugas berikut:

- Tugas 1: Menjalankan aplikasi berbasis data untuk mendemonstrasikan penggunaan Azure Key Vault dalam mengenkripsi database Azure SQL

#### <a name="task-1-run-a-data-driven-application-to-demonstrate-the-use-of-azure-key-vault-in-encrypting-the-azure-sql-database"></a>Tugas 1: Menjalankan aplikasi berbasis data untuk mendemonstrasikan penggunaan Azure Key Vault dalam mengenkripsi database Azure SQL

Anda akan membuat aplikasi Konsol menggunakan Visual Studio untuk memuat data ke dalam kolom terenkripsi dan kemudian mengakses data tersebut dengan aman menggunakan string koneksi yang mengakses kunci di Key Vault.

1. Dari sesi RDP ke **az500-10-vm1**, luncurkan **Visual Studio 2019** dari **Menu mulai**.

2. Beralih ke jendela yang menampilkan pesan sambutan Visual Studio 2019, klik tombol **Masuk** dan, saat diminta, berikan kredensial yang Anda gunakan untuk mengautentikasi ke langganan Azure yang Anda gunakan di lab ini.

3. Pada halaman **Memulai**, klik **Buat proyek baru**. 

4. Dalam daftar template proyek, cari **Aplikasi Konsol (.NET Framework)** , dalam daftar hasil, klik **Aplikasi Konsol (.NET Framework)** untuk **C#** , dan klik **Berikutnya**.

5. Pada halaman **Konfigurasikan proyek baru Anda**, tentukan pengaturan berikut (biarkan pengaturan lain memiliki nilai defaultnya), lalu klik **Buat**:

    |Pengaturan|Nilai|
    |---|---|
    |Nama proyek|**OpsEncrypt**|
    |Nama solusi|**OpsEncrypt**|
    |Kerangka Kerja|**.NET Framework 4.7.2**|

6. Di konsol Visual Studio, klik menu **Alat**, di menu drop down, klik **Manajer Paket NuGet**, dan, di menu menurun, klik **Konsol Manajer Paket**.

7. Di panel **Konsol Manajer Paket**, jalankan yang berikut ini untuk menginstal paket **NuGet** pertama yang diperlukan:

    ```powershell
    Install-Package Microsoft.SqlServer.Management.AlwaysEncrypted.AzureKeyVaultProvider
    ```

8. Di panel **Konsol Manajer Paket**, jalankan yang berikut ini untuk menginstal paket **NuGet** kedua yang diperlukan:

    ```powershell
    Install-Package Microsoft.IdentityModel.Clients.ActiveDirectory
    ```
    
9. Minimalkan sesi RDP ke mesin virtual Azure Anda, lalu navigasikan ke **\\Allfiles\\Labs\\10\\program.cs**, buka di Notepad, dan salin kontennya ke Clipboard.

10. Kembali ke sesi RDP, dan di konsol Visual Studio, di jendela **Penjelajah Solusi**, klik **Program.cs** dan ganti kontennya dengan kode yang Anda salin ke Clipboard.

11. Di jendela Visual Studio, di panel **Program.cs**, pada baris 15, ganti tempat penampung `<connection string noted earlier>` dengan string koneksi **ADO.NET** database Azure SQL yang Anda catat sebelumnya di lab. Di string koneksi, ganti tempat penampung `{your_password}`, dengan `Pa55w.rd1234`. Jika Anda menyimpan string di komputer lab, Anda mungkin perlu keluar dari sesi RDP untuk menyalin string ADO, lalu kembali ke mesin virtual Azure untuk menempel string ADO.

12. Di jendela Visual Studio, di panel **Program.cs**, pada baris 16, ganti tempat penampung `<client id noted earlier>` dengan nilai **ID Aplikasi (klien)** dari aplikasi terdaftar yang Anda catat sebelumnya di lab. 

13. Di jendela Visual Studio, di panel **Program.cs**, pada baris 17, ganti tempat penampung `<key value noted earlier>` dengan nilai **Key1** dari aplikasi terdaftar yang Anda catat sebelumnya di lab. 

14. Di konsol Visual Studio, klik tombol **Mulai** untuk memulai pembuatan aplikasi konsol dan memulainya.

15. Aplikasi akan memulai jendela Wantian Perintah. Saat diminta memberikan kata sandi, ketik kata sandi yang Anda tentukan dalam penyebaran di Latihan 1 untuk menghubungkan ke Azure SQL Database. 

16. Biarkan aplikasi konsol berjalan dan alihkan ke konsol **SQL Management Studio**. 

17. Di panel **Object Explorer**, klik kanan **database medis** dan, di menu klik kanan, klik **Kueri Baru**.

18. Dari jendela kueri, jalankan kueri berikut untuk memverifikasi bahwa data yang dimuat ke dalam database dari aplikasi konsol dienkripsi.

    ```sql
    SELECT FirstName, LastName, SSN, BirthDate FROM Patients;
    ```

19. Beralih kembali ke aplikasi konsol tempat Anda diminta memasukkan SSN yang valid. Ini akan mengkueri kolom terenkripsi untuk data. Di Perintah, ketik yang berikut ini dan tekan tombol Enter:

    ```cmd
    999-99-0003
    ```

    >**Catatan**: Pastikan bahwa data yang dikembalikan oleh kueri tidak dienkripsi.

20. Untuk menghentikan aplikasi konsol, tekan tombol Enter

**Membersihkan sumber daya**

> Jangan lupa untuk menghapus sumber daya Azure yang baru dibuat dan yang tidak diperlukan lagi. Menghapus sumber daya yang tidak terpakai memastikan Anda tidak akan dikenakan biaya tidak terduga.

1. Di portal Microsoft Azure, buka Cloud Shell dengan mengklik ikon pertama di kanan atas portal Microsoft Azure. 

2. Di menu drop-down kiri atas panel Cloud Shell, pilih **PowerShell** dan, jika diminta, klik **Konfirmasi**.

3. Di sesi PowerShell dalam panel Cloud Shell, jalankan yang berikut ini untuk menghapus grup sumber daya yang Anda buat di lab ini:
  
    ```powershell
    Remove-AzResourceGroup -Name "AZ500LAB10" -Force -AsJob
    ```

4.  Tutup panel **Cloud Shell**. 
