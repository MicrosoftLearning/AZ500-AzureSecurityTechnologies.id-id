---
lab:
  title: 06 - Menerapkan Sinkronisasi Direktori
  module: Module 01 - Manage Identity and Access
ms.openlocfilehash: 00c359e1875ab915ab697d8ed33e36d956540529
ms.sourcegitcommit: 1da29a6d959a7f91dbbcbabf5ec06869c98fc1f1
ms.translationtype: HT
ms.contentlocale: id-ID
ms.lasthandoff: 03/30/2022
ms.locfileid: "145195925"
---
# <a name="lab-06-implement-directory-synchronization"></a>Lab 06: Menerapkan Sinkronisasi Direktori
# <a name="student-lab-manual"></a>Panduan lab siswa

## <a name="lab-scenario"></a>Skenario lab

Anda telah diminta untuk membuat bukti konsep yang menunjukkan cara mengintegrasikan lingkungan Active Directory Domain Services (AD DS) lokal dengan penyewa Azure Active Directory (Azure AD). Secara khusus, Anda ingin:

- Menerapkan forest AD DS domain tunggal dengan menyebarkan VM Azure yang menghosting pengendali domain AD DS
- Membuat dan mengonfigurasi penyewa Azure Active Directory
- Menyinkronkan forest AD DS dengan penyewa Azure Active Directory

> Untuk semua sumber daya di lab ini, kita menggunakan wilayah **US Timur**. Pastikan pada instruktur Anda bahwa ini adalah wilayah yang akan digunakan untuk kelas. 

## <a name="lab-objectives"></a>Tujuan lab

Di lab ini, Anda akan menyelesaikan latihan berikut:

- Latihan 1: Menyebarkan VM Azure yang menghosting pengendali domain Active Directory
- Latihan 2: Membuat dan mengonfigurasi penyewa Azure Active Directory
- Latihan 3: Menyinkronkan forest Active Directory dengan penyewa Azure Active Directory

## <a name="implement-directory-synchronization"></a>Menerapkan Sinkronisasi Direktori

![gambar](https://user-images.githubusercontent.com/91347931/157525374-8f740f14-c2db-47b3-98f8-7feb9bc122b5.png)

## <a name="instructions"></a>Instruksi

### <a name="exercise-1-deploy-an-azure-vm-hosting-an-active-directory-domain-controller"></a>Latihan 1: Menyebarkan VM Azure yang menghosting pengendali domain Active Directory

### <a name="estimated-timing-10-minutes"></a>Perkiraan waktu: 10 menit

Dalam latihan ini, Anda akan menyelesaikan tugas-tugas berikut:

- Tugas 1: Mengidentifikasi nama DNS yang tersedia untuk penyebaran VM Azure
- Tugas 2: Menggunakan template ARM untuk menyebarkan VM Azure yang menghosting pengendali domain Active Directory

#### <a name="task-1-identify-an-available-dns-name-for-an-azure-vm-deployment"></a>Tugas 1: Mengidentifikasi nama DNS yang tersedia untuk penyebaran VM Azure

Dalam tugas ini, Anda akan mengidentifikasi nama DNS untuk penyebaran VM Azure Anda. 

1. Masuk ke portal Microsoft Azure **`https://portal.azure.com/`** .

    >**Catatan**: Masuk ke portal Microsoft Azure menggunakan akun yang memiliki peran Pemilik atau Kontributor dalam langganan Azure yang Anda gunakan untuk lab ini.

2. Buka Cloud Shell dengan mengklik ikon pertama di kanan atas Portal Azure. Jika diminta, klik **PowerShell** dan **Buat penyimpanan**.

3. Pastikan **PowerShell** dipilih di menu drop-down di sudut kiri atas panel Cloud Shell.

4. Di sesi PowerShell dalam panel Cloud Shell, jalankan perintah berikut untuk mengidentifikasi nama DNS yang tersedia yang dapat Anda gunakan untuk penyebaran VM Azure di tugas berikutnya dari latihan ini:

    ```powershell
    Test-AzDnsAvailability -DomainNameLabel <custom-label> -Location '<location>'
    ```

    >**Catatan**: Ganti tempat penampung `<custom-label>` dengan nama DNS valid yang kemungkinan bersifat unik secara global. Ganti tempat penampung `<location>` dengan nama wilayah tempat Anda ingin menerapkan VM Azure yang akan menghosting pengendali domain Active Directory yang akan digunakan di lab ini.

    >**Catatan**: Untuk mengidentifikasi wilayah Azure tempat Anda dapat memprovisikan Mesin Virtual Azure, lihat [ **https://azure.microsoft.com/en-us/regions/offers/** ](https://azure.microsoft.com/en-us/regions/offers/)

5. Verifikasi bahwa perintah mengembalikan **True**. Jika tidak, jalankan kembali perintah yang sama dengan nilai `<custom-label>` yang berbeda hingga perintah mengembalikan **True**.

6. Catat nilai `<custom-label>` yang menghasilkan hasil yang berhasil. Anda akan membutuhkannya di tugas berikutnya.

7. Tutup Cloud Shell.

#### <a name="task-2-use-an-arm-template-to-deploy-an-azure-vm-hosting-an-active-directory-domain-controller"></a>Tugas 2: Menggunakan template ARM untuk menyebarkan VM Azure yang menghosting pengendali domain Active Directory

Dalam tugas ini, Anda akan menyebarkan VM Azure yang akan menjadi host pengendali domain direktori aktif

1. Buka tab browser lain di jendela browser yang sama dan navigasikan ke **https://github.com/Azure/azure-quickstart-templates/tree/master/application-workloads/active-directory/active-directory-new-domain** .

2. Pada halaman **Buat VM Windows baru dan buat Forest AD, Domain, dan DC baru**, klik **Sebarkan ke Azure**. Perintah ini akan secara otomatis mengarahkan browser ke panel **Buat VM Azure dengan Forest AD baru** di portal Microsoft Azure.

3. Pada panel **Buat VM Azure dengan Forest AD baru**, klik **Edit parameter**.

4. Pada panel **Edit parameter**, klik **Muat file**, di kotak dialog **Buka**, navigasikan ke folder **\\\\AllFiles\Labs\\06\\active-directory-new-domain\\azuredeploy.parameters.json**, klik **Buka**, lalu klik **Simpan**.

5. Pada panel **Buat VM Azure dengan Forest AD baru**, tentukan pengaturan berikut (biarkan yang lain diatur ke nilai yang ada):

   |Pengaturan|Nilai|
   |---|---|
   |Langganan|nama langganan Azure Anda|
   |Grup sumber daya|klik **Buat baru** dan ketik nama **AZ500LAB06**|
   |Wilayah|wilayah Azure yang Anda identifikasi di tugas sebelumnya|
   |Nama Pengguna Admin|**Siswa**|
   |Kata Sandi Admin|**Gunakan kata sandi pribadi Anda yang dibuat di Lab 04 > Latihan 1 > Tugas 1 > Langkah 9.**|
   |Nama Domain|**adatum.com**|
   |Awalan Dns|nama host DNS yang Anda identifikasi di tugas sebelumnya|
   |Ukuran Komputer Virtual|**Standard_D2s_v3**|

6. Pada panel **Buat VM Azure dengan Forest AD baru**, klik **Tinjau + buat**, lalu klik **Buat**.

    >**Catatan**: Jangan menunggu hingga penyebaran selesai. Lanjutkan ke latihan berikutnya. Penyebaran mungkin perlu waktu sekitar 15 menit. Anda akan menggunakan mesin virtual yang digunakan dalam tugas ini dalam latihan ketiga lab ini.

> Hasil: Setelah menyelesaikan latihan ini, Anda telah memulai penyebaran VM Azure yang akan menjadi host pengendali domain direktori aktif menggunakan template Azure Resource Manager


### <a name="exercise-2-create-and-configure-an-azure-active-directory-tenant"></a>Latihan 2: Membuat dan mengonfigurasi penyewa Azure Active Directory 

### <a name="estimated-timing-20-minutes"></a>Perkiraan waktu: 20 menit

Dalam latihan ini, Anda akan menyelesaikan tugas-tugas berikut:

- Tugas 1: Membuat penyewa Azure Active Directory (AD)
- Tugas 2: Menambahkan nama DNS kustom ke penyewa Azure Active Directory baru
- Tugas 3: Membuat pengguna Azure Active Directory dengan peran Administrator Global

#### <a name="task-1-create-an-azure-active-directory-ad-tenant"></a>Tugas 1: Membuat penyewa Azure Active Directory (AD)

Dalam tugas ini, Anda akan membuat penyewa Azure Active Directory baru untuk digunakan di lab ini. 

1. Di portal Microsoft Azure, di kotak teks **Cari sumber daya, layanan, dan dokumen** di bagian atas halaman portal Microsoft Azure, ketik **Azure Active Directory** dan tekan tombol **Enter**.

2. Pada panel yang menampilkan **Ringkasan** penyewa Azure Active Directory Anda saat ini, klik **Kelola penyewa**, lalu di layar berikutnya, klik **+ Buat**.

3. Pada tab **Dasar** panel **Buat direktori**, pastikan opsi **Azure Active Directory** dipilih dan klik **Berikutnya: Konfigurasi >** .

4. Pada tab **Konfigurasi** panel **Buat direktori**, tentukan pengaturan berikut:

   |Pengaturan|Nilai|
   |---|---|
   |Nama organisasi|**AdatumSync**|
   |Nama domain awal|nama unik yang terdiri dari kombinasi huruf dan angka|
   |Negara atau wilayah|**Amerika Serikat**|

    >**Catatan**: Catat nama domain awal. Anda akan membutuhkannya nanti di lab ini.

    >**Catatan**: Tanda centang hijau di kotak teks **Nama domain awal** akan menunjukkan apakah nama domain yang Anda ketikkan valid dan unik. (Catat nama domain awal Anda untuk digunakan nanti).

5. Klik **Review + create**, lalu klik **Create**.

    >**Catatan**: Tunggu hingga penyewa baru dibuat. Gunakan ikon **Pemberitahuan** untuk memantau status penyebaran. 

#### <a name="task-2-add-a-custom-dns-name-to-the-new-azure-ad-tenant"></a>Tugas 2: Menambahkan nama DNS kustom ke penyewa Azure Active Directory baru

Dalam tugas ini, Anda akan menambahkan nama DNS kustom ke penyewa Azure Active Directory yang baru. 

1. Di portal Microsoft Azure, di toolbar, klik ikon **Direktori + langganan**, yang terletak di sebelah kanan ikon Cloud Shell. 

2. Di panel **Direktori + langganan**, pilih baris **AdatumSync** penyewa yang baru dibuat dan klik tombol **Beralih**.

    >**Catatan**: Anda mungkin perlu merefresh jendela browser jika entri **AdatumSync** tidak muncul dalam daftar filter **Direktori + langganan**.

3. Pada panel **AdatumSync \| Azure Active Directory**, di bagian **Kelola**, klik **Nama domain kustom**.

4. Pada panel **AdatumSync \| Nama domain kustom**, klik **+ Tambahkan domain kustom**.

5. Pada panel **Nama domain kustom**, dalam kotak teks **Nama domain kustom**, ketik **adatum.com** dan klik **Tambahkan Domain**.

6. Pada panel **adatum.com**, tinjau informasi yang diperlukan untuk melakukan verifikasi nama domain Azure Active Directory, lalu pilih **Hapus** dua kali. 

    >**Catatan**: Anda tidak akan dapat menyelesaikan proses validasi karena Anda tidak memiliki nama domain DNS **adatum.com**. Hal ini tidak akan mencegah Anda menyinkronkan domain AD DS **adatum.com** dengan penyewa Azure Active Directory. Anda akan menggunakan untuk tujuan ini nama DNS awal penyewa Azure Active Directory (nama yang diakhiri dengan akhiran **onmicrosoft.com**), yang Anda identifikasi di tugas sebelumnya. Namun, perlu diingat bahwa, sebagai hasilnya, nama domain DNS dari domain AD DS dan nama DNS penyewa Azure Active Directory akan berbeda. Ini berarti bahwa pengguna Adatum harus menggunakan nama yang berbeda saat masuk ke domain AD DS dan saat masuk ke penyewa Azure Active Directory.

#### <a name="task-3-create-an-azure-ad-user-with-the-global-administrator-role"></a>Tugas 3: Membuat pengguna Azure Active Directory dengan peran Administrator Global

Dalam tugas ini, Anda akan menambahkan pengguna Azure Active Directory baru dan menetapkan mereka ke peran Administrator Global. 

1. Pada panel penyewa **AdatumSync** Azure Active Directory, di bagian **Kelola**, klik **Pengguna**.

2. Pada panel **Pengguna \| Semua pengguna**, klik **+ Pengguna Baru**. 

3. Pada panel **Pengguna baru**, pastikan bahwa opsi **Buat pengguna** dipilih, tentukan pengaturan berikut (biarkan semua yang lain diatur ke nilai defaultnya) dan klik **Buat**:

   |Pengaturan|Nilai|
   |---|---|
   |Nama pengguna|**syncadmin**|
   |Nama|**syncadmin**|
   |Kata sandi|pastikan opsi **Buat kata sandi otomatis** dipilih dan klik **Tampilkan Kata Sandi**|
   |Grup|**0 grup dipilih**|
   |Peran|klik **Pengguna**, lalu klik **Administrator global**, dan klik **Pilih**|
   |Lokasi Penggunaan|**Amerika Serikat**|  

    >**Catatan**: Catat nama pengguna lengkap. Anda dapat menyalin nilainya dengan mengeklik tombol **Salin ke clipboard** di sisi kanan daftar dropdown yang menampilkan nama domain. 

    >**Catatan**: Catat kata sandi pengguna. Anda akan membutuhkannya nanti di lab ini. 

    >**Catatan**: Pengguna Azure Active Directory dengan peran Administrator Global diperlukan untuk menerapkan Azure Active Directory Connect.

4. Buka jendela browser InPrivate.

5. Navigasikan ke portal Microsoft Azure dan masuk menggunakan akun pengguna **syncadmin**. Saat diminta, ubah kata sandi yang Anda catat sebelumnya dalam tugas ini menjadi **Pa55w.rd1234**.

    >**Catatan**: Untuk masuk, Anda harus memberikan nama akun pengguna **syncadmin** yang sepenuhnya memenuhi syarat, termasuk nama domain DNS penyewa Azure Active Directory, yang Anda catat sebelumnya dalam tugas ini. Nama pengguna ini dalam format syncadmin@`<your_tenant_name>`.onmicrosoft.com, ketika `<your_tenant_name>` adalah tempat penampung yang mewakili nama penyewa Azure Active Directory unik Anda. 

6. Keluar sebagai **syncadmin** dan tutup jendela browser InPrivate.

> **Hasil**: Setelah menyelesaikan latihan ini, Anda telah membuat penyewa Azure Active Directory, menambahkan nama DNS kustom ke penyewa Azure Active Directory baru, dan membuat pengguna Azure Active Directory dengan peran Administrator Global.


### <a name="exercise-3-synchronize-active-directory-forest-with-an-azure-active-directory-tenant"></a>Latihan 3: Menyinkronkan forest Active Directory dengan penyewa Azure Active Directory

### <a name="estimated-timing-20-minutes"></a>Perkiraan waktu: 20 menit

Dalam latihan ini, Anda akan menyelesaikan tugas-tugas berikut:

- Tugas 1: Mempersiapkan AD DS untuk sinkronisasi direktori
- Tugas 2: Menginstal Azure Active Directory Connect
- Tugas 3: Memverifikasi sinkronisasi direktori

#### <a name="task-1-prepare-ad-ds-for-directory-synchronization"></a>Tugas 1: Mempersiapkan AD DS untuk sinkronisasi direktori

Dalam tugas ini, Anda akan terhubung ke VM Azure yang menjalankan pengendali domain AD DS dan membuat akun sinkronisasi direktori. 

   > Sebelum memulai tugas ini, pastikan bahwa penyebaran template yang Anda mulai di latihan pertama lab ini telah selesai.

1. Di portal Microsoft Azure, atur filter **Direktori + langganan** ke penyewa Azure Active Directory yang terkait dengan langganan Azure tempat Anda menyebarkan VM Azure dalam latihan pertama lab ini.

2. Di portal Microsoft Azure, dalam kotak teks **Cari sumber daya, layanan, dan dokumen** di bagian atas halaman portal Microsoft Azure, ketik **Mesin virtual** dan tekan tombol **Enter**.

3. Pada panel **Mesin virtual**, klik entri **adVM**. 

4. Pada panel **adVM**, klik **Hubungkan** dan, di menu dropdown, klik **RDP**. 

5. Di parameter **alamat IP**, pilih **Alamat IP publik load balancer**, lalu klik **Unduh File RDP** dan gunakan untuk terhubung ke VM Azure **adVM** melalui Desktop Jarak Jauh. Saat diminta untuk mengautentikasi, berikan kredensial berikut:

   |Pengaturan|Nilai|
   |---|---|
   |Nama pengguna|**Siswa**|
   |Kata sandi|**Gunakan kata sandi pribadi Anda yang dibuat di Lab 04 > Latihan 1 > Tugas 1 > Langkah 9.**|

    >**Catatan**: Tunggu sesi Desktop Jauh dan **Manajer Server** dimuat.  

    >**Catatan**: Langkah-langkah berikut dilakukan di sesi Desktop Jarak Jauh ke **adVM** VM Azure. 

6. Di **Pengelola Server**, klik **Server Lokal**, lalu klik **Konfigurasi Keamanan yang Ditingkatkan IE**.

7. Di kotak dialog **Konfigurasi Keamanan yang Ditingkatkan Internet Explorer**, atur kedua opsi ke **Mati** dan klik **OK**.

8. Buka Internet Explorer, navigasikan ke **https://www.microsoft.com/en-us/edge/business/download** , unduh biner penginstalan Microsoft Edge, jalankan penginstalan, dan konfigurasikan browser web dengan pengaturan default.

9. Di **Pengelola Server**, klik **Alat** dan, di menu dropdown, klik **Pusat Administrasi Active Directory**.

10. Di **Pusat Administrasi Active Directory**, klik **adatum (lokal)** , di panel **Tugas**, di bawah nama domain **adatum (lokal)** klik **Baru**, dan, di menu berjenjang, klik **Unit Organisasi**.

11. Di jendela **Buat Unit Organisasi**, di kotak teks **Nama**, ketik **ToSync** dan klik **OK**.

12. Klik dua kali unit organisasi **ToSync** yang baru dibuat sehingga kontennya muncul di panel detail konsol Pusat Administratif Active Directory. 

13. Di panel **Tugas**, di bagian **ToSync**, klik **Baru**, dan, di menu berjenjang, klik **Pengguna**.

14. Di jendela **Buat Pengguna**, buat akun pengguna baru dengan pengaturan berikut (biarkan yang lain diatur ke nilai yang ada) dan klik **OK**:

   |Pengaturan|Nilai|
   |---|---|
   |Nama Lengkap|**aduser1**|
   |Masuk UPN Pengguna|**aduser1**|
   |Masuk SamAccountName Pengguna|**aduser1**|
   |Kata Sandi dan Konfirmasi Kata Sandi|**Gunakan kata sandi pribadi Anda yang dibuat di Lab 04 > Latihan 1 > Tugas 1 > Langkah 9.**|
   |Opsi kata sandi lainnya|**Kata sandi tidak pernah kedaluwarsa**|

#### <a name="task-2-install-azure-ad-connect"></a>Tugas 2: Menginstal Azure Active Directory Connect

Dalam tugas ini, Anda akan menginstal AD Connect pada mesin virtual. 

1. Dalam sesi Desktop Jarak Jauh ke **adVM**, gunakan Microsoft Edge untuk menavigasi ke portal Microsoft Azure di **https://portal.azure.com** , dan masuk menggunakan akun pengguna **syncadmin** yang Anda buat di latihan sebelumnya. Saat diminta, tentukan nama lengkap pengguna yang Anda catat dan kata sandi **Pa55w.rd1234**.

2. Di portal Microsoft Azure, di kotak teks **Cari sumber daya, layanan, dan dokumen** di bagian atas halaman portal Microsoft Azure, ketik **Azure Active Directory** dan tekan tombol **Enter**.

3. Di portal Microsoft Azure, pada panel **AdatumSync \| Ringkasan**, klik **Azure Active Directory Connect**.

4. Pada panel **AdatumSync \| Azure Active Directory Connect**, klik tautan **Unduh Azure Active Directory Connect**. Anda akan diarahkan ke halaman unduhan **Microsoft Azure Active Directory Connect**.

5. Pada halaman unduhan **Microsoft Azure Active Directory Connect**, klik **Unduh**.

6. Saat diminta, klik **Jalankan** untuk memulai wizard **Microsoft Azure Active Directory Connect**.

7. Pada halaman **Selamat datang di Azure Active Directory Connect** dari wizard **Microsoft Azure Active Directory Connect**, klik kotak centang **Saya menyetujui persyaratan lisensi dan pemberitahuan privasi** dan klik **Lanjutkan**.

8. Pada halaman **Pengaturan Ekspres** dari wizard **Microsoft Azure Active Directory Connect**, klik opsi **Sesuaikan**.

9. Pada halaman **Instal komponen yang diperlukan**, biarkan semua opsi konfigurasi opsional tidak dipilih dan klik **Instal**.

10. Pada halaman **Masuk pengguna**, pastikan bahwa hanya **Sinkronisasi Hash Kata Sandi** yang diaktifkan dan klik **Berikutnya**.

11. Pada halaman **Hubungkan ke Azure Active Directory**, autentikasi dengan menggunakan kredensial akun pengguna **syncadmin** yang Anda buat di latihan sebelumnya dan klik **Berikutnya**. 

12. Pada halaman **Hubungkan direktori Anda**, klik tombol **Tambahkan Direktori** di sebelah kanan entri forest **adatum.com**.

13. Di jendela **akun forest AD**, pastikan opsi **Buat akun AD baru** dipilih, tentukan kredensial berikut, dan klik **OK**:

   |Pengaturan|Nilai|
   |---|---|
   |Nama Pengguna|**ADATUM\\Siswa**|
   |Kata sandi|**Gunakan kata sandi pribadi Anda yang dibuat di Lab 06 > Latihan 1 > Tugas 2**|

14. Kembali ke halaman **Hubungkan direktori Anda**, pastikan entri **adatum.com** muncul sebagai direktori yang dikonfigurasi dan klik **Berikutnya**

15. Pada halaman **Konfigurasi masuk Azure Active Directory**, perhatikan peringatan yang menyatakan **Pengguna tidak akan dapat masuk ke Azure Active Directory dengan kredensial lokal jika akhiran UPN tidak cocok dengan nama domain terverifikasi**, aktifkan kotak centang **Lanjutkan tanpa mencocokkan semua akhiran UPN dengan domain terverifikasi**, dan klik **Berikutnya**.

    >**Catatan**: Seperti yang dijelaskan sebelumnya, hal ini diharapkan, karena Anda tidak dapat memverifikasi domain Azure Active Directory DNS kustom **adatum.com**.

16. Pada halaman **Pemfilteran domain dan OU**, klik opsi **Sinkronkan domain dan OU yang dipilih**, nama domain **adatum.com** akan dicentang, luaskan **adatum.com** untuk melihat **ToSync**. Kosongkan semua kotak centang, klik hanya kotak centang di sebelah OU **ToSync**, dan klik **Berikutnya**.

17. Pada halaman **Mengidentifikasi pengguna Anda secara unik**, terima pengaturan defaultnya, dan klik **Berikutnya**.

18. Pada halaman **Filter pengguna dan perangkat**, terima pengaturan defaultnya, dan klik **Berikutnya**.

19. Pada halaman **Fitur opsional**, terima pengaturan defaultnya, dan klik **Berikutnya**.

20. Pada halaman **Siap untuk mengonfigurasi**, pastikan bahwa kotak centang **Mulai proses sinkronisasi saat konfigurasi selesai** dipilih dan klik **Instal**.

    >**Catatan**: Instalasi akan memerlukan waktu sekitar 2 menit.

21. Tinjau informasi di halaman **Konfigurasi selesai** dan klik **Keluar** untuk menutup jendela **Microsoft Azure Active Directory Connect**.


#### <a name="task-3-verify-directory-synchronization"></a>Tugas 3: Memverifikasi sinkronisasi direktori

Dalam tugas ini, Anda akan memverifikasi bahwa sinkronisasi direktori berfungsi. 

1. Dalam sesi Desktop Jarak Jauh ke **adVM**, di jendela Microsoft Edge yang menampilkan portal Microsoft Azure, navigasikan ke panel **Pengguna - Semua pengguna (Pratinjau)** Lab Adatum penyewa Azure Active Directory.

2. Pada panel **Pengguna \| Semua pengguna**, perhatikan bahwa daftar objek pengguna menyertakan akun **aduser1**. 

>**Catatan**: Anda mungkin harus menunggu beberapa menit dan memilih **Refresh** agar akun pengguna **aduser1** muncul.

3. Pilih akun **aduser1** dan, di bagian **Profil > Identitas**, perhatikan bahwa atribut **Sumber** diatur ke **Windows Server AD**.

4. Pada panel Profil **aduser1 \|** , di bagian **Info pekerjaan**, perhatikan bahwa atribut **Departemen** tidak diatur.

5. Dalam sesi Desktop Jarak Jauh ke **adVM**, alihkan ke **Pusat Administratif Active Directory**, pilih entri **aduser1** dalam daftar objek di **ToSyncOU**, dan, di panel **Tugas**, di bagian **aduser1**, pilih **Properti**.

6. Di jendela **aduser1**, di bagian **Organisasi**, di kotak teks **Departemen**, ketik **Penjualan**, dan pilih **OK**.

7. Dalam sesi Desktop Jarak Jauh ke **adVM**, mulai **Windows PowerShell**.

8. Dari **Administrator: Konsol Windows PowerShell**, jalankan perintah berikut untuk memulai sinkronisasi delta Azure Active Directory Connect:

    ```powershell
    Import-Module -Name 'C:\Program Files\Microsoft Azure AD Sync\Bin\ADSync\ADSync.psd1'

    Start-ADSyncSyncCycle -PolicyType Delta
    ```

9. Beralih ke jendela Microsoft Edge yang menampilkan panel Profil **aduser1 \|** , refresh halaman dan perhatikan bahwa properti **Departemen** diatur ke **Penjualan**.

    >**Catatan**: Anda mungkin perlu menunggu satu menit lagi dan merefresh halaman lagi jika atribut **Departemen** tetap tidak diatur.

> **Hasil**: Setelah menyelesaikan latihan ini, Anda telah menyiapkan AD DS untuk sinkronisasi direktori, menginstal Azure Active Directory Connect, dan sinkronisasi direktori terverifikasi.


**Membersihkan sumber daya**

>**Catatan**: Mulailah dengan menonaktifkan sinkronisasi Azure Active Directory

1. Dalam sesi Desktop Jarak Jauh ke **adVM**, mulai Windows PowerShell sebagai Administrator.

2. Dari konsol Windows PowerShell, instal modul MsOnline PowerShell dengan menjalankan perintah berikut (saat diminta, di penyedia NuGet diperlukan untuk melanjutkan kotak dialog, ketik **Ya** dan tekan Enter.):

    ```powershell
    [Net.ServicePointManager]::SecurityProtocol = [Net.SecurityProtocolType]::Tls12
    Install-PackageProvider -Name NuGet -MinimumVersion 2.8.5.201 -Force
    Install-Module MsOnline -Force
    ```

3. Dari konsol Windows PowerShell, hubungkan ke penyewa AdatumSync Azure Active Directory dengan menjalankan perintah berikut (saat diminta, masuk dengan kredensial **syncadmin**):

    ```powershell
    Connect-MsolService
    ```

4. Dari konsol Windows PowerShell, nonaktifkan sinkronisasi Azure Active Directory Connect dengan menjalankan perintah berikut:

    ```powershell
    Set-MsolDirSyncEnabled -EnableDirSync $false -Force
    ```

5. Dari konsol Windows PowerShell, verifikasi bahwa operasi berhasil dengan menjalankan perintah berikut:

    ```powershell
    (Get-MSOLCompanyInformation).DirectorySynchronizationEnabled
    ```
    >**Catatan**: Hasilnya harus `False`. Jika bukan itu masalahnya, tunggu sebentar dan jalankan kembali perintahnya.

    >**Catatan**: Selanjutnya, hapus sumber daya Azure
6. Tutup sesi Desktop jarak jauh.

7. Di portal Microsoft Azure, atur filter **Direktori + langganan** ke penyewa Azure Active Directory yang terkait dengan langganan Azure tempat Anda menyebarkan VM Azure **adVM**.

8. Di portal Microsoft Azure, buka Cloud Shell dengan mengklik ikon pertama di kanan atas Portal Microsoft Azure. 

9. Pada menu drop-down di sudut kiri atas panel Cloud Shell, pilih **PowerShell**, dan, jika diminta, klik **Konfirmasi**. 

10. Di sesi PowerShell dalam panel Cloud Shell, jalankan perintah berikut untuk menghapus grup sumber daya yang Anda buat di lab ini:
  
    ```powershell
    Remove-AzResourceGroup -Name "AZ500LAB06" -Force -AsJob
    ```
11. Tutup panel **Cloud Shell**.

    >**Catatan**: Terakhir, hapus penyewa Azure Active Directory
    
    >**Catatan 2**: Menghapus penyewa adalah proses yang sangat sulit, sehingga tidak pernah dapat dilakukan secara tidak sengaja atau jahat.  Itu berarti menghapus penyewa sebagai bagian dari lab ini sering kali tidak berhasil.  Meskipun kami di sini memiliki langkah-langkah untuk menghapus penyewa, jangan beranggapan bahwa Anda telah menyelesaikan lab ini. Jika Anda perlu menghapus penyewa di dunia nyata, artikel di DOCS.Microsoft.com untuk membantu Anda.

12. Kembali ke portal Microsoft Azure, gunakan filter **Direktori + langganan** untuk beralih ke penyewa **AdatumSync** Azure Active Directory.

13. Di portal Microsoft Azure, navigasikan ke panel **Pengguna - Semua pengguna**, klik entri yang mewakili akun pengguna **syncadmin**, pada panel **syncadmin - Profil** klik **Hapus**, dan, saat diminta untuk mengonfirmasi, klik **Ya**.

14. Ulangi urutan langkah yang sama untuk menghapus akun pengguna **aduser1** dan **Akun Layanan Sinkronisasi Direktori Lokal**.

15. Navigasikan ke panel **AdatumSync - Ringkasan** penyewa Azure Active Directory, klik **Kelola penyewa** dan centang kotak direktori **AdatumSync**, klik **Hapus**, pada panel **Hapus penyewa 'AdatumSync'** , klik tautan **Dapatkan izin untuk menghapus sumber daya Azure**, pada panel **Properti** Azure Active Directory, atur **Manajemen akses untuk sumber daya Azure** ke **Ya** dan klik **Simpan**.

    >**Catatan**: Saat menghapus jika Anda menerima tanda peringatan seperti **Hapus semua pengguna** kemudian lanjutkan untuk menghapus pengguna yang telah Anda buat atau jika tanda peringatan mengatakan **Hapus aplikasi LinkedIn** klik pada pesan peringatan dan konfirmasi penghapusan aplikasi LinkedIn, semua peringatan harus ditangani untuk meneruskan penghapusan penyewa.

16. Keluar dari portal Microsoft Azure dan masuk kembali. 

17. Navigasikan kembali ke panel **Hapus penyewa 'AdatumSync'** dan klik **Hapus**.

> Untuk informasi tambahan apa pun mengenai tugas ini, lihat [https://docs.microsoft.com/en-us/azure/active-directory/users-groups-roles/directory-delete-howto](https://docs.microsoft.com/en-us/azure/active-directory/users-groups-roles/directory-delete-howto)
