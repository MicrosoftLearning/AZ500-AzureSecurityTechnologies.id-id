---
lab:
  title: 04 - MFA, Akses Bersyarat, dan Perlindungan Identitas AAD
  module: Module 01 - Manage Identity and Access
ms.openlocfilehash: f63f8a24c0d9b7c870967ee8c83292bd80b617f9
ms.sourcegitcommit: 2f08105eaaf0413d3ec3c12a3b078678151fd211
ms.translationtype: HT
ms.contentlocale: id-ID
ms.lasthandoff: 04/04/2022
ms.locfileid: "145195928"
---
# <a name="lab-04-mfa-conditional-access-and-aad-identity-protection"></a>Lab 04: MFA, Akses Bersyarat, dan Perlindungan Identitas AAD
# <a name="student-lab-manual"></a>Panduan lab siswa

## <a name="lab-scenario"></a>Skenario lab

Anda telah diminta untuk membuat bukti konsep fitur yang meningkatkan autentikasi Azure Active Directory (Azure AD). Secara khusus, Anda ingin mengevaluasi:

- Autentikasi multifaktor Azure AD
- Akses bersyarat Azure AD
- Azure AD Identity Protection

> Untuk semua sumber daya di lab ini, kita menggunakan wilayah **US Timur**. Verifikasi dengan instruktur Anda bahwa ini adalah wilayah yang akan digunakan untuk kelas. 

## <a name="lab-objectives"></a>Tujuan Lab

Di lab ini, Anda akan menyelesaikan latihan berikut:

- Latihan 1: Menyebarkan VM Azure dengan templat Azure Resource Manager
- Latihan 2: Menerapkan Azure MFA
- Latihan 3: Menerapkan Kebijakan Akses Bersyarat Azure AD 
- Latihan 4: Menerapkan Perlindungan Identitas Azure AD

## <a name="mfa---conditional-access---identity-protection-diagram"></a>MFA - Akses Bersyarat - Diagram Perlindungan Identitas

![gambar](https://user-images.githubusercontent.com/91347931/157518628-8b4a9efe-0086-4ec0-825e-3d062748fa63.png)

## <a name="instructions"></a>Instruksi

## <a name="lab-files"></a>File lab:

- **\\Allfiles\\Labs\\04\\az-500-04_azuredeploy.json**
- **\\Allfiles\\Labs\\04\\az-500-04_azuredeploy.parameters.json** 

### <a name="exercise-1-deploy-an-azure-vm-by-using-an-azure-resource-manager-template"></a>Latihan 1: Menyebarkan VM Azure dengan templat Azure Resource Manager

### <a name="estimated-timing-10-minutes"></a>Perkiraan waktu: 10 menit

Dalam latihan ini, Anda akan menyelesaikan tugas-tugas berikut:

- Tugas 1: Menyebarkan Azure VM menggunakan templat Azure Resource Manager.

#### <a name="task-1-deploy-an-azure-vm-by-using-an-azure-resource-manager-template"></a>Tugas 1: Menyebarkan VM Azure dengan templat Azure Resource Manager

Dalam tugas ini, Anda akan membuat mesin virtual menggunakan templat ARM. Mesin virtual ini akan digunakan dalam latihan terakhir untuk lab ini. 

1. Masuk ke portal Microsoft Azure **`https://portal.azure.com/`** .

    >**Catatan**: Masuk ke portal Azure menggunakan akun yang memiliki peran Pemilik atau Kontributor dalam langganan Azure yang Anda gunakan untuk lab ini dan peran Administrator Global di penyewa Azure AD yang terkait dengan langganan tersebut.

2. Di portal Azure, di kotak teks **Cari sumber daya, layanan, dan dokumen** di bagian atas halaman portal Azure, ketik **Sebarkan templat khusus**.

    >**Catatan**: Anda juga dapat memilih **Penyebaran Templat (menyebarkan menggunakan templat kustom)** dari daftar **Marketplace**.

3. Pada panel **Penyebaran kustom**, klik opsi **Buat templat Anda sendiri di editor**.

4. Pada bilah **Edit templat**, klik **Muat file**, cari file **\\Allfiles\\Labs\\04\\az-500-04_azuredeploy.json** dan klik **Buka**.

    >**Catatan**: Tinjau konten templat dan perhatikan bahwa templat itu menyebarkan Pusat Data mesin virtual Azure yang menghosting Windows Server 2019.

5. Pada panel **Edit templat**, klik **Simpan**.

6. Kembali ke bilah **Penyebaran kustom**, klik **Edit parameter**.

7. Pada bilah **Edit parameter**, klik **Muat file**, cari parameter file **\\Allfiles\\Labs\\04\\az-500-04_azuredeploy.parameters.json** dan klik **Buka**.

    >**Catatan**: Tinjau isi file parameter dengan mencatat nilai adminUsername dan adminPassword.

8. Pada bilah **Edit parameter**, klik **Simpan**.

9. Pada panel **Penyebaran kustom**, pastikan bahwa pengaturan berikut telah dikonfigurasi (biarkan yang lain dengan nilai defaultnya):

>**Catatan**: Anda perlu membuat kata sandi unik yang akan digunakan untuk membuat VM (mesin virtual) selama sisa kursus ini. Kata sandi harus memiliki panjang setidaknya 12 karakter dan memenuhi persyaratan kompleksitas yang ditentukan (Kata sandi harus memiliki 3 hal berikut: 1 karakter huruf kecil, 1 karakter huruf besar, 1 angka, dan 1 karakter khusus). [Persyaratan kata sandi VM](https://docs.microsoft.com/en-us/azure/virtual-machines/windows/faq#what-are-the-password-requirements-when-creating-a-vm-). Harap dicatat kata sandinya.

   |Pengaturan|Nilai|
   |---|---|
   |Langganan|nama langganan Azure yang akan Anda gunakan di lab ini|
   |Grup sumber daya|klik **Buat baru** dan ketikkan nama **AZ500LAB04**|
   |Lokasi|**(AS) AS Timur**|
   |Ukuran mesin virtual|**Standard_D2s_v3**|
   |Nama VM|**az500-04-vm1**|
   |Nama Pengguna Admin|**Siswa**|
   |Kata Sandi Admin|**Harap buat kata sandi Anda sendiri dan catat untuk referensi di masa mendatang. Anda akan dimintai kata sandi ini untuk akses lab yang diperlukan.**|
   |Nama Virtual Network|**az500-04-vnet1**|

    >**Note**: To identify Azure regions where you can provision Azure VMs, refer to [**https://azure.microsoft.com/en-us/regions/offers/**](https://azure.microsoft.com/en-us/regions/offers/)

10. Klik **Review + create**, lalu klk **Create**.

    >**Catatan**: Jangan menunggu penyebaran selesai tetapi lanjutkan ke latihan berikutnya. Anda akan menggunakan mesin virtual yang disertakan dalam penyebaran ini dalam latihan terakhir lab ini.

> Hasil: Anda telah memulai penyebaran templat Azure VM **az500-04-vm1** yang akan Anda gunakan dalam latihan terakhir lab ini.


### <a name="exercise-2-implement-azure-mfa"></a>Latihan 2: Menerapkan Azure MFA

### <a name="estimated-timing-30-minutes"></a>Perkiraan waktu: 30 menit

Dalam latihan ini, Anda akan menyelesaikan tugas-tugas berikut:

- Tugas 1: Membuat penyewa Azure AD baru.
- Tugas 2: Mengaktifkan uji coba Azure AD Premium P2.
- Tugas 3: Membuat pengguna dan grup Azure AD.
- Tugas 4: Menetapkan lisensi Azure AD Premium P2 untuk pengguna Azure AD.
- Tugas 5: Mengonfigurasikan pengaturan Azure MFA.
- Tugas 6: Memvalidasi konfigurasi MFA

#### <a name="task-1-create-a-new-azure-ad-tenant"></a>Tugas 1: Membuat penyewa Azure AD baru

Dalam tugas ini, Anda akan membuat penyewa Azure AD baru. 

1. Di portal Azure, di kotak teks **Cari sumber daya, layanan, dan dokumen** di bagian atas halaman portal Azure, ketikkan **Azure Active Directory** dan tekan tombol **Enter**.

2. Pada bilah yang menampilkan **Ringkasan** penyewa Azure AD Anda saat ini, klik **Kelola penyewa**, lalu di layar berikutnya, klik **+ Buat**.

3. Pada tab **Dasar** bilah **Buat penyewa**, pastikan opsi **Azure Active Directory** dipilih dan klik **Berikutnya: Konfigurasi >** .

4. Pada tab **Konfigurasi** pada panel **Buat penyewa**, tentukan pengaturan berikut:

   |Pengaturan|Nilai|
   |---|---|
   |Nama organisasi|**AdatumLab500-04**|
   |Nama domain awal|nama unik yang terdiri dari kombinasi huruf dan angka|
   |Negara atau wilayah|**Amerika Serikat**|

    >**Catatan**: Catat nama domain awal. Anda akan membutuhkannya nanti di lab ini.

5. Klik **Tinjau + Buat**, lalu klik **Buat**.
6. Tambahkan kode Captcha pada bilah **Bantu kami membuktikan bahwa Anda bukan robot**, lalu klik tombol **Kirim**. 

    >**Catatan**: Tunggu penyewa baru dibuat. Gunakan ikon **Pemberitahuan** untuk memantau status penyebaran. 


#### <a name="task-2-activate-azure-ad-premium-p2-trial"></a>Tugas 2: Aktifkan uji coba Azure AD Premium P2

Dalam tugas ini, Anda akan mendaftar untuk uji coba gratis Azure AD Premium P2. 

1. Di portal Azure, di toolbar, klik ikon **Direktori + langganan**, yang terletak di sebelah kanan ikon Cloud Shell. 

2. Di bilah **Direktori + langganan**, klik penyewa yang baru dibuat **AdatumLab500-04** dan klik tombol **Beralih** untuk mengaturnya sebagai direktori saat ini.

    >**Catatan**: Anda mungkin perlu me-refresh jendela browser jika entri **AdatumLab500-04** tidak muncul dalam daftar filter **Direktori + langganan**.

3. Di portal Azure, di kotak teks **Cari sumber daya, layanan, dan dokumen** di bagian atas halaman portal Azure, ketikkan **Azure Active Directory** dan tekan tombol **Enter**. Pada bilah **AdatumLab500-04**, di bagian **Kelola**, klik **Lisensi**.

4. Pada bilah **Lisensi \| Ringkasan**, di bagian **Kelola**, klik **Semua produk** lalu klik **+ Coba/Beli**.

5. Pada bilah **Aktifkan**, di bagian Azure AD Premium P2, klik **Uji Coba Gratis**, lalu klik **Aktifkan**.


#### <a name="task-3-create-azure-ad-users-and-groups"></a>Tugas 3: Membuat pengguna dan grup Azure AD.

Dalam tugas ini, Anda akan membuat tiga pengguna: aaduser1 (Admin Global), aaduser2 (pengguna), dan aaduser3 (pengguna). Anda akan memerlukan nama utama dan kata sandi setiap pengguna untuk tugas selanjutnya. 

1. Navigasikan kembali ke bilah **AdatumLab500-04** Azure Active Directory dan, di bagian **Kelola**, klik **Pengguna**.

2. Pada bilah **Pengguna \| Semua pengguna**, klik **+ Pengguna Baru**. 

3. Pada bilah **Pengguna baru**, pastikan bahwa opsi **Buat pengguna** dipilih, dan tentukan pengaturan berikut (biarkan semua opsi yang lain dengan nilai defaultnya) dan klik **Buat** :

   |Pengaturan|Nilai|
   |---|---|
   |Nama pengguna|**aaduser1**|
   |Nama|**aaduser1**|
   |Kata sandi|pastikan opsi **Buat sandi otomatis** dipilih dan klik **Tampilkan Sandi**|
   |Grup|**0 grup dipilih**|
   |Peran|klik **Pengguna**, lalu klik **Administrator global**, dan klik **Pilih**|
   |Lokasi Penggunaan|**Amerika Serikat**|  

    >**Catatan**: Catat nama pengguna lengkap. Anda dapat menyalin nilainya dengan mengeklik tombol **Salin ke clipboard** di sisi kanan daftar dropdown yang menampilkan nama domain. 

    >**Catatan**: Catat kata sandi pengguna. Anda akan membutuhkannya nanti di lab ini. 

4. Kembali ke bilah **Pengguna \| Semua pengguna**, klik **+ Pengguna Baru**. 

5. Pada bilah **Pengguna baru**, pastikan bahwa opsi **Buat pengguna** dipilih, dan tentukan pengaturan berikut (biarkan opsi yang lainnya dengan nilai defaultnya):

   |Pengaturan|Nilai|
   |---|---|
   |Nama pengguna|**aaduser2**|
   |Nama|**aaduser2**|
   |Kata sandi|pastikan opsi **Buat sandi otomatis** dipilih dan klik **Tampilkan Sandi**|
   |Grup|**0 grup dipilih**|
   |Peran|**Pengguna**|
   |Lokasi Penggunaan|**Amerika Serikat**|  

    >**Catatan**: Catat nama pengguna lengkap dan kata sandi.

6. Kembali ke bilah **Pengguna \| Semua pengguna**, klik **+ Pengguna Baru**. 

7. Klik **Pengguna Baru**, selesaikan pengaturan konfigurasi pengguna baru, lalu klik **Buat**.

   |Pengaturan|Nilai|
   |---|---|
   |Nama pengguna|**aaduser3**|
   |Nama|**aaduser3**|
   |Kata sandi|pastikan opsi **Buat sandi otomatis** dipilih dan klik **Tampilkan Sandi**|
   |Grup|**0 grup dipilih**|
   |Peran|**Pengguna**|
   |Lokasi Penggunaan|**Amerika Serikat**|  

    >**Catatan**: Catat nama pengguna lengkap dan kata sandi.

    >**Catatan**: Pada tahapan ini, Anda sudah memiliki tiga pengguna baru yang terdaftar di halaman **Pengguna**. 
    
#### <a name="task-4-assign-azure-ad-premium-p2-licenses-to-azure-ad-users"></a>Tugas 4: Tetapkan lisensi Azure AD Premium P2 untuk pengguna Azure AD

Dalam tugas ini, Anda akan menetapkan setiap pengguna ke lisensi Azure Active Directory Premium P2.

1. Pada bilah **Pengguna \| Semua pengguna**, klik entri yang mewakili akun pengguna Anda. 

2. Pada bilah yang menampilkan properti akun pengguna Anda, klik **Edit**.  Pastikan Lokasi Penggunaan diatur menjadi **Amerika Serikat** jika tidak, atur lokasi penggunaan tersebut dan klik **Simpan**.

3. Navigasikan kembali ke bilah **AdatumLab500-04** Azure Active Directory dan, di bagian **Kelola**, klik **Lisensi**.

4. Pada bilah **Lisensi \| Ringkasan**, klik **Semua produk**, beri centang kotak **Azure Active Directory Premium P2**, dan klik **+ Tetapkan** .

5. Pada bilah **Tetapkan lisensi**, klik **+ Tambahkan pengguna dan grup**.

6. Pada bilah **Pengguna**, pilih **aaduser1**, **aaduser2**, **aaduser3**, dan akun pengguna Anda, lalu klik **Pilih**.

7. Kembali ke bilah **Tetapkan lisensi**, klik **Opsi penetapan**, pastikan semua opsi diaktifkan, klik **Tinjau + tetapkan**, klik **Tetapkan**.

8. Keluar dari portal Azure dan masuk kembali menggunakan akun yang sama. Langkah ini diperlukan agar penetapan lisensi dapat diterapkan.

    >**Catatan**: Pada tahapan ini, Anda menetapkan lisensi Azure Active Directory Premium P2 ke semua akun pengguna yang akan Anda gunakan di lab ini. Pastikan untuk keluar dan kemudian masuk kembali. 

#### <a name="task-5-configure-azure-mfa-settings"></a>Tugas 5: Mengonfigurasikan pengaturan Azure MFA.

Dalam tugas ini, Anda akan mengonfigurasi MFA dan mengaktifkan MFA untuk aaduser1. 

1. Di portal Azure, navigasikan kembali ke bilah penyewa **AdatumLab500-04** Azure Active Directory.

    >**Catatan**: Pastikan Anda menggunakan penyewa AdatumLab500-04 Azure AD.

2. Pada bilah penyewa **AdatumLab500-04** Azure Active Directory, di bagian **Kelola**, klik **Keamanan**.

3. Pada bilah **Keamanan \| Memulai**, di bagian **Kelola**, klik **MFA**.

4. Pada bilah **Autentikasi Multi-Faktor \| Memulai**, klik tautan **Pengaturan MFA berbasis cloud tambahan**. 

    >**Catatan**: Cara ini akan membuka tab browser baru, menampilkan halaman **autentikasi multi-faktor**.

5. Pada halaman **autentikasi multi-faktor**, klik tab **pengaturan layanan**. Tinjau **opsi verifikasi**. Perhatikan bahwa **Pesan teks ke telepon**, **Pemberitahuan melalui aplikasi seluler**, dan **Kode verifikasi dari aplikasi seluler atau token perangkat keras** diaktifkan. Klik **Simpan** lalu klik **tutup**.

6. Beralih ke tab **pengguna**, klik entri **aaduser1**, klik tautan **Aktifkan**, dan, jika diminta, klik **aktifkan autentikasi multifaktor** .

7. Perhatikan kolom **status Autentikasi Multi Faktor** untuk **aaduser1** sekarang **Diaktifkan**.

8. Klik **aaduser1** dan perhatikan bahwa pada tahapan ini, Anda juga memiliki opsi **Terapkan**. 

    >**Catatan**: Mengubah status pengguna dari Diaktifkan ke Diberlakukan hanya berdampak pada aplikasi terintegrasi Azure AD lama yang tidak mendukung Azure MFA dan, setelah status berubah menjadi Diberlakukan, memerlukan penggunaan kata sandi aplikasi.

9. Dengan entri **aaduser1** dipilih, klik **Kelola pengaturan pengguna** dan tinjau opsi yang tersedia: 

   - Wajibkan pengguna yang dipilih untuk memberikan metode kontak lagi.

   - Hapus semua kata sandi aplikasi yang ada yang dibuat oleh pengguna yang dipilih.

   - Pulihkan autentikasi multifaktor pada semua perangkat yang diingat.

10. Klik **Batal** dan beralih kembali ke tab browser yang menampilkan bilah **Autentikasi Multifaktor \| Memulai** di portal Azure.

11. Di bagian **Pengaturan**, klik **Peringatan penipuan**.

12. Pada bilah **Autentikasi multifaktor \| Peringatan penipuan**, konfigurasikan pengaturan berikut:

   |Pengaturan|Nilai|
   |---|---|
   |Izinkan pengguna mengirimkan peringatan penipuan|**Aktif**|
   |Secara otomatis memblokir pengguna yang melaporkan penipuan|**Aktif**|
   |Kode untuk melaporkan penipuan selama pesan pembuka|**0**|

13. Klik **Simpan**

    >**Catatan**: Pada tahapan ini, Anda telah mengaktifkan MFA untuk aaduser1 dan menyiapkan pengaturan peringatan penipuan. 

14. Navigasikan kembali ke bilah penyewa **AdatumLab500-04** Azure Active Directory, di bagian **Kelola**, klik **Properti**, selanjutnya klik tautan **Mengelola Keamanan defaults** di bagian bawah bilah, pada bilah **Aktifkan Default Keamanan**, klik **Tidak**. Pilih **Organisasi Saya menggunakan Akses Bersyarat** sebagai alasannya, lalu klik **Simpan**.

    >**Catatan**: Pastikan Anda masuk ke penyewa **AdatumLab500-04** Azure AD. Anda dapat menggunakan filter **Direktori + langganan** untuk beralih di antara penyewa Azure AD. Pastikan Anda masuk sebagai pengguna dengan peran Administrator Global di penyewa Azure AD.

#### <a name="task-6-validate-mfa-configuration"></a>Tugas 6: Memvalidasi konfigurasi MFA

Dalam tugas ini, Anda akan memvalidasi konfigurasi MFA dengan menguji upaya masuk akun pengguna aaduser1. 

1. Buka jendela browser InPrivate.

2. Navigasikan ke portal Azure dan masuk menggunakan akun pengguna **aaduser1**. 

    >**Catatan**: Untuk masuk, Anda harus memberikan nama akun pengguna **aaduser1** yang sepenuhnya memenuhi syarat, termasuk nama domain DNS penyewa Azure AD, yang Anda catat sebelumnya di lab ini. Nama pengguna ini dalam format aaduser1@`<your_tenant_name>`.onmicrosoft.com, ketika `<your_tenant_name>` adalah pengganti yang mewakili nama penyewa Azure AD unik Anda. 

3. Saat diminta, di kotak dialog **Diperlukan informasi selengkapnya**, klik **Berikutnya**.

    >**Catatan**: Sesi browser akan dialihkan ke halaman **Verifikasi keamanan tambahan**.

4. Pada halaman **Jaga keamanan akun Anda**, pilih tautan **Saya ingin menyiapkan metode lain**, di menu dropdown **Metode mana yang ingin Anda gunakan?** daftar dropdown, pilih **Ponsel**, dan pilih **Konfirmasi**.

5. Pada halaman **Jaga keamanan akun Anda**, pilih negara atau wilayah Anda, ketikkan nomor ponsel Anda di area **Masukkan nomor telepon**, pastikan opsi **Kirimkan saya kode** dipilih, dan klik **Berikutnya**.
 
6. Pada halaman **Jaga keamanan akun Anda**, ketik kode yang Anda terima dalam pesan teks di ponsel Anda, dan klik **Berikutnya**.

7. Pada halaman **Jaga keamanan akun Anda**, pastikan verifikasi berhasil dan klik **Berikutnya**.

8. Pada halaman **Jaga keamanan akun Anda**, klik **Saya ingin menggunakan metode lain**, pilih **Email** dari daftar dropdown, klik **Konfirmasi**, berikan alamat email yang ingin Anda gunakan, dan klik **Berikutnya**. Setelah Anda menerima email yang sesuai, identifikasi kode di isi email, berikan kode tersebut, lalu klik **Selesai**.

9. Saat diminta, ubah kata sandi Anda. Pastikan untuk mencatat kata sandi baru.

10. Pastikan Anda berhasil masuk ke portal Azure.

11. Keluar sebagai **aaduser1** dan tutup jendela browser InPrivate.

> Hasil: Anda telah membuat penyewa AD baru, mengonfigurasi pengguna AD, mengonfigurasi MFA, dan menguji pengalaman MFA untuk pengguna. 


### <a name="exercise-3-implement-azure-ad-conditional-access-policies"></a>Latihan 3: Menerapkan Kebijakan Akses Bersyarat Azure AD 

### <a name="estimated-timing-15-minutes"></a>Perkiraan waktu: 15 menit

Dalam latihan ini, Anda akan menyelesaikan tugas-tugas berikut: 

- Tugas 1: Mengonfigurasikan kebijakan akses bersyarat.
- Tugas 2: Menguji kebijakan akses bersyarat.

#### <a name="task-1---configure-a-conditional-access-policy"></a>Tugas 1 - Mengonfigurasi kebijakan akses bersyarat. 

Dalam tugas ini, Anda akan meninjau pengaturan kebijakan akses bersyarat dan membuat kebijakan yang memerlukan MFA saat masuk ke portal Azure. 

1. Di portal Azure, navigasikan kembali ke bilah penyewa **AdatumLab500-04** Azure Active Directory.

2. Pada bilah **AdatumLab500-04**, di bagian **Kelola**, klik **Keamanan**.

3. Pada bilah **Keamanan \| Memulai**, di bagian **Lindungi**, klik **Akses Bersyarat**.

4. Pada bilah **Kebijakan \|Akses Bersyarat**, klik **+ Kebijakan baru** pilih **Buat kebijakan baru** dari daftar dropdown. 

5. Pada bilah **Baru**, konfigurasikan pengaturan berikut:

   - Di kotak teks **Nama**, ketikkan **AZ500Policy1**
    
   - Klik **Pengguna atau identitas beban kerja yang dipilih**. Di sebelah kanan pada Apa kebijakan ini berlaku untuk >> Pengguna dan grup >> Sertakan >> Aktifkan **Pilih pengguna dan grup** >> pilih kotak centang **Pengguna dan Grup**, di **Pilih** bilah, klik **aaduser2**, dan klik **Pilih**.
    
   - Klik **Tindakan atau aplikasi cloud**, klik **Pilih aplikasi**, pada bilah **Pilih**, klik **Microsoft Azure Management**, dan klik **Pilih** . 

    >**Catatan**: Tinjau peringatan bahwa kebijakan ini memengaruhi akses ke Portal Azure.
    
   - Klik **Ketentuan**, klik **Risiko masuk**, pada bilah **Risiko masuk**, tinjau tingkat risiko tetapi jangan buat perubahan apa pun dan tutup bilah **Risiko masuk**.
    
   - Klik **Platform perangkat**, tinjau platform perangkat yang dapat disertakan, lalu klik **Selesai**.
    
   - Klik **Lokasi** dan tinjau opsi lokasi tanpa membuat perubahan apa pun.
    
   - Klik **Berikan** di bagian **Kontrol akses**, pada bilah **Berikan**, beri centang kotak **Memerlukan autentikasi multi-faktor** dan klik  **Pilih**
    
   - Atur **Aktifkan kebijakan** menjadi **Aktif**.

6. Pada bilah **Baru**, klik **Buat**. 

    >**Catatan**: Pada tahapan ini, Anda memiliki kebijakan akses bersyarat yang mengharuskan MFA untuk masuk ke portal Azure. 

#### <a name="task-2---test-the-conditional-access-policy"></a>Tugas 2 - Menguji kebijakan akses bersyarat.

Dalam tugas ini, Anda akan masuk ke portal Azure sebagai **aaduser2** dan memverifikasi bahwa MFA diperlukan. Anda juga akan menghapus kebijakan tersebut sebelum melanjutkan ke latihan berikutnya. 

1. Buka jendela Microsoft Edge InPrivate.

2. Di jendela browser baru, navigasikan ke portal Azure dan masuk dengan akun pengguna **aaduser2**.

3. Saat diminta, di kotak dialog **Diperlukan informasi selengkapnya**, klik **Berikutnya**.

    >**Catatan**: Tampilan browser akan dialihkan ke halaman **Jaga keamanan akun Anda**.
    
4. Pada halaman **Jaga keamanan akun Anda**, pilih tautan **Saya ingin menyiapkan metode lain**, di menu dropdown **Metode mana yang ingin Anda gunakan?** daftar dropdown, pilih **Ponsel**, dan pilih **Konfirmasi**.

5. Pada halaman **Jaga keamanan akun Anda**, pilih negara atau wilayah Anda, ketikkan nomor ponsel Anda di area **Masukkan nomor telepon**, pastikan opsi **Kirimkan saya kode** dipilih, dan klik **Berikutnya**.

6. Pada halaman **Jaga keamanan akun Anda**, ketik kode yang Anda terima dalam pesan teks di ponsel Anda, dan klik **Berikutnya**.

7. Pada halaman **Jaga keamanan akun Anda**, pastikan verifikasi berhasil dan klik **Berikutnya**.

8. Pada halaman **Jaga keamanan akun Anda**, klik **Selesai**.

9. Saat diminta, ubah kata sandi Anda. Pastikan untuk mencatat kata sandi baru.

10. Pastikan Anda berhasil masuk ke portal Azure.

11. Keluar sebagai **aaduser2** dan tutup jendela peramban InPrivate.

    >**Catatan**: Anda sekarang telah memverifikasi bahwa kebijakan akses bersyarat yang baru dibuat memberlakukan MFA saat aaduser2 masuk ke portal Azure.

12. Kembali ke jendela browser yang menampilkan portal Azure, navigasikan kembali ke bilah penyewa **AdatumLab500-04** Azure Active Directory.

13. Pada bilah **AdatumLab500-04**, di bagian **Kelola**, klik **Keamanan**.

14. Pada bilah **Keamanan \| Memulai**, di bagian **Lindungi**, klik **Akses Bersyarat**.

15. Pada bilah **Akses Bersyarat \| Kebijakan**, klik elipsis di samping **AZ500Policy1**, klik **Hapus**, dan, saat diminta untuk mengonfirmasi, klik **Ya** .

    >**Catatan**: Hasil: Dalam latihan ini Anda menerapkan kebijakan akses bersyarat untuk mewajibkan MFA saat pengguna masuk ke portal Azure. 

>Hasil: Anda telah mengonfigurasi dan menguji akses bersyarat Azure AD.

### <a name="exercise-4-implement-azure-ad-identity-protection"></a>Latihan 4: Menerapkan Perlindungan Identitas Azure AD

### <a name="estimated-timing-30-minutes"></a>Perkiraan waktu: 30 menit

Dalam latihan ini, Anda akan menyelesaikan tugas-tugas berikut: 

- Tugas 1: Melihat opsi Perlindungan Identitas Azure AD di portal Azure
- Tugas 2: Mengonfigurasikan kebijakan risiko pengguna
- Tugas 3: Mengonfigurasikan kebijakan risiko masuk
- Tugas 4: Mensimulasikan peristiwa risiko terhadap kebijakan Perlindungan Identitas Azure AD 
- Tugas 5: Meninjau laporan Perlindungan Identitas Azure AD

#### <a name="task-1-enable-azure-ad-identity-protection"></a>Tugas 1: Mengaktifkan Perlindungan Identitas Azure AD

Dalam tugas ini, Anda akan melihat opsi Perlindungan Identitas Azure AD di portal Azure. 

1. Jika perlu, masuk ke portal Azure **`https://portal.azure.com/`** .

    >**Catatan**: Pastikan Anda masuk ke penyewa **AdatumLab500-04** Azure AD. Anda dapat menggunakan filter **Direktori + langganan** untuk beralih di antara penyewa Azure AD. Pastikan Anda masuk sebagai pengguna dengan peran Administrator Global di penyewa Azure AD.

2. Pada bilah **AdatumLab500-04**, di bagian **Kelola**, klik **Keamanan**.

3. Pada bilah **Keamanan \| Memulai**, di bagian **Lindungi**, klik **Perlindungan Identitas**.

4. Pada bilah **Perlindungan Identitas \| Ringkasan**, tinjau diagram **Pengguna baru yang berisiko terdeteksi** dan **Masuk berisiko baru terdeteksi** serta informasi lain tentang pengguna berisiko. 

#### <a name="task-2-configure-a-user-risk-policy"></a>Tugas 2: Mengonfigurasikan kebijakan risiko pengguna

Dalam tugas ini, Anda akan membuat kebijakan risiko pengguna. 

1. Pada bilah **Perlindungan Identitas \| Ringkasan**, di bagian **Lindungi**, klik **kebijakan risiko pengguna**

2. Konfigurasikan **Kebijakan pemulihan risiko pengguna** dengan pengaturan berikut: 

   - Klik **Pengguna**; pada tab **Sertakan** bilah **Pengguna**, pastikan bahwa opsi **Semua pengguna** dipilih.

   - Pada bilah **Pengguna**, alihkan ke tab **Kecualikan**, klik **Pilih pengguna yang dikecualikan**, pilih akun pengguna Anda, lalu klik **Pilih**. 

   - Klik **Risiko pengguna**; pada bilah **Risiko pengguna**, pilih **Rendah dan di atasnya**, lalu klik **Selesai**. 

   - Klik **Akses**; pada bilah **Access**, pastikan bahwa opsi **Izinkan akses** dan kotak centang **Memerlukan perubahan sandi** dipilih dan klik **Selesai**.

   - Atur **Terapkan kebijakan** menjadi **Aktif** dan klik **Simpan**.

#### <a name="task-3-configure-sign-in-risk-policy"></a>Tugas 3: Konfigurasikan kebijakan risiko masuk

Dalam tugas ini, Anda akan mengonfigurasi kebijakan risiko masuk. 

1. Pada bilah **Perlindungan Identitas \| Kebijakan risiko pengguna**, di bagian **Lindungi**, klik **Kebijakan risiko masuk**

2. Konfigurasikan **Kebijakan pemulihan risiko masuk** dengan pengaturan berikut: 

   - Klik **Pengguna**; pada tab **Sertakan** bilah **Pengguna**, pastikan bahwa opsi **Semua pengguna** dipilih.

   - Klik **Risiko masuk**; pada bilah **Risiko masuk**, pilih **Sedang dan di atasnya**, lalu klik **Selesai**. 

   - Klik **Akses**; pada bilah **Akses**, pastikan bahwa opsi **Izinkan akses** dan kotak centang **Memerlukan autentikasi multi-faktor** dipilih dan klik **Selesai**.

   - Atur **Terapkan Kebijakan** menjadi **Aktif** dan klik **Simpan**.

#### <a name="task-4-simulate-risk-events-against-the-azure-ad-identity-protection-policies"></a>Tugas 4: Mensimulasikan peristiwa risiko terhadap kebijakan Perlindungan Identitas Azure AD 

> Sebelum Anda memulai tugas ini, pastikan bahwa penyebaran templat yang Anda mulai di Latihan 1 telah selesai. Penyebaran mencakup VM Azure bernama **az500-04-vm1**. 

1. Di portal Azure, atur filter **Direktori + langganan** ke penyewa Azure AD yang terkait dengan langganan Azure tempat Anda menyebarkan Azure VM **az500-04-vm1**.

2. Di portal Microsoft Azure, di kotak teks **Cari sumber daya, layanan, dan dokumen** di bagian atas halaman portal Microsoft Azure, ketik **Mesin virtual** dan tekan tombol **Enter**.

3. Pada panel **mesin virtual**, klik entri **az500-04-vm1**. 

4. Pada bilah **az500-04-vm1**, klik **Hubungkan** dan di menu dropdown, klik **RDP**. 

5. Klik **Unduh File RDP** dan gunakan untuk menyambung ke **az500-04-vm1** Azure VM melalui Desktop Jarak Jauh. Saat diminta untuk mengautentikasi, berikan kredensial berikut:

   |Pengaturan|Nilai|
   |---|---|
   |Nama pengguna|**Siswa**|
   |Kata sandi|**Gunakan kata sandi pribadi Anda yang dibuat di Lab 04 > Latihan 1 > Tugas 1 > Langkah 9.**|

    >**Catatan**: Tunggu sesi Desktop Jarak Jauh dan **Pengelola Server** dimuat.  

    >**Catatan**: Langkah-langkah berikut dilakukan di sesi Desktop Jarak Jauh ke **az500-04-vm1** Azure VM. 

6. Di **Pengelola Server**, klik **Server Lokal**, lalu klik **IE Enhanced Security Configuration**.

7. Di kotak dialog **Konfigurasi Keamanan yang Ditingkatkan Internet Explorer**, atur kedua opsi menjadi **Off** dan klik **OK**.

8. Mulai **Internet Explorer**, klik ikon roda gigi di toolbar, di menu dropdown, klik **Keamanan**, lalu klik **Penjelajahan InPrivate**.

9. Di jendela InPrivate Internet Explorer, navigasikan ke Proyek Browser ToR di **https://www.torproject.org/projects/torbrowser.html.en** .

10. Unduh dan instal ToR Browser versi Windows dengan pengaturan default. 

11. Setelah penginstalan selesai, mulai Peramban ToR, gunakan opsi **Hubungkan** di halaman awal, dan telusuri ke Panel Akses Aplikasi di **https://myapps.microsoft.com** .

12. Saat diminta, coba masuk dengan akun **aaduser3**. 

    >**Catatan**: Anda akan melihat pesan **Upaya Masuk Anda diblokir**. Hal ini diharapkan, karena akun ini tidak dikonfigurasi dengan autentikasi multifaktor, yang diperlukan karena peningkatan risiko masuk yang terkait dengan penggunaan Browser ToR.

13. Gunakan **Keluar dan masuk dengan opsi akun yang berbeda** atau pilih panah kembali untuk masuk sebagai akun **aaduser1** yang Anda buat dan konfigurasikan untuk autentikasi multifaktor sebelumnya di lab ini.

    >**Catatan**: Kali ini, Anda akan disajikan dengan pesan **Aktivitas yang mencurigakan terdeteksi**. Sekali lagi, hal ini diharapkan karena akun ini dikonfigurasi dengan autentikasi multi-faktor. Mempertimbangkan peningkatan risiko masuk yang terkait dengan penggunaan Browser ToR, Anda harus menggunakan autentikasi multifaktor.

14. Gunakan opsi **Verifikasi** dan tentukan apakah Anda ingin memverifikasi identitas Anda melalui SMS atau panggilan.

15. Selesaikan verifikasi dan pastikan Anda berhasil masuk ke Panel Akses Aplikasi.

16. Tutup sesi RDP Anda. 

    >**Catatan**: Pada titik ini, Anda mencoba dua proses masuk yang berbeda. Selanjutnya, Anda akan meninjau laporan Perlindungan Identitas Azure.

#### <a name="task-5-review-the-azure-ad-identity-protection-reports"></a>Tugas 5: Meninjau laporan Perlindungan Identitas Azure AD

Dalam tugas ini, Anda akan meninjau laporan Azure AD Identity Protection yang dihasilkan dari upaya masuk browser ToR.

1. Kembali ke portal Azure, gunakan filter **Direktori + langganan** untuk beralih ke penyewa **AdatumLab500-04** Azure Active Directory.

2. Pada bilah **AdatumLab500-04**, di bagian **Kelola**, klik **Keamanan**.

3. Pada bilah **Keamanan \| Memulai**, di bagian **Laporan**, klik **Pengguna berisiko**. 

4. Tinjau laporan dan identifikasi entri yang merujuk pada akun pengguna **aaduser3**.

5. Pada bilah **Keamanan \| Memulai**, di bagian **Laporan**, klik **Masuk yang berisiko**. 

6. Tinjau laporan dan identifikasi entri apa pun yang terkait dengan proses masuk dengan akun pengguna **aaduser3**.

7. Pada **Laporan** klik **Deteksi risiko**.

8. Tinjau laporan dan identifikasi entri apa pun yang mewakili proses masuk dari alamat IP anonim yang dihasilkan oleh browser ToR. 

 >**Catatan**: Mungkin diperlukan waktu 10-15 menit hingga risiko muncul dalam laporan.

> **Hasil**: Anda telah mengaktifkan Azure AD Identity Protection, mengonfigurasi kebijakan risiko pengguna dan kebijakan risiko masuk, serta memvalidasi konfigurasi Azure AD Identity Protection dengan mensimulasikan peristiwa risiko.

**Membersihkan sumber daya**

> Kita perlu menghapus sumber daya perlindungan identitas yang tidak lagi Anda gunakan. 

Gunakan langkah-langkah berikut untuk menonaktifkan kebijakan perlindungan identitas di penyewa Azure AD **AdatumLab500-04**.

1. Di portal Azure, navigasikan kembali ke bilah penyewa **AdatumLab500-04** Azure Active Directory.

2. Pada bilah **AdatumLab500-04**, di bagian **Kelola**, klik **Keamanan**.

3. Pada bilah **Keamanan \| Memulai**, di bagian **Lindungi**, klik **Perlindungan Identitas**.

4. Pada bilah **Perlindungan Identitas \| Ringkasan**, klik **Kebijakan risiko pengguna**.

5. Pada bilah **Perlindungan Identitas \| Kebijakan risiko pengguna**, atur **Terapkan kebijakan** menjadi **Nonaktif** lalu klik **Simpan**.

6. Pada bilah **Perlindungan Identitas \| Kebijakan risiko pengguna**, klik **Kebijakan risiko masuk**

7. Pada bilah **Perlindungan Identitas \| Kebijakan risiko masuk**, atur **Terapkan kebijakan** menjadi **Nonaktif** lalu klik **Simpan**.

Gunakan langkah-langkah berikut untuk menghentikan Azure VM yang Anda sediakan sebelumnya di lab.

1. Di portal Azure, atur filter **Direktori + langganan** ke penyewa Azure AD yang terkait dengan langganan Azure tempat Anda menyebarkan Azure VM **az500-04-vm1**.

2. Di portal Microsoft Azure, di kotak teks **Cari sumber daya, layanan, dan dokumen** di bagian atas halaman portal Microsoft Azure, ketik **Mesin virtual** dan tekan tombol **Enter**.

3. Pada panel **mesin virtual**, klik entri **az500-04-vm1**. 
 
4. Pada bilah **az500-04-vm1**, klik **Stop** dan saat diminta untuk mengonfirmasi, klik **OK** 

>  Jangan hapus sumber daya yang disediakan di lab ini, karena lab PIM memiliki ketergantungan pada sumber daya tersebut.
