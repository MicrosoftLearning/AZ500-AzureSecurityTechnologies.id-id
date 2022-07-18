---
lab:
  title: " 12 - Titik Akhir Layanan dan Mengamankan Penyimpanan"
  module: Module 03 - Secure data and applications
ms.openlocfilehash: 094c35e70258656693d64a816b9ed9382eade199
ms.sourcegitcommit: 7c5e8e9a86553c6bd9b9a6651b60c6cb9676f0ff
ms.translationtype: HT
ms.contentlocale: id-ID
ms.lasthandoff: 07/18/2022
ms.locfileid: "147168507"
---
# <a name="lab-12-service-endpoints-and-securing-storage"></a>Lab 12: Titik Akhir Layanan dan Mengamankan Penyimpanan
# <a name="student-lab-manual"></a>Panduan lab siswa

## <a name="lab-scenario"></a>Skenario lab

Anda telah diminta untuk membuat bukti konsep untuk mendemonstrasikan pengamanan berbagi file Azure. Secara khusus, Anda ingin:
    
- Membuat titik akhir penyimpanan sehingga lalu lintas yang ditujukan ke Azure Storage selalu berada dalam jaringan tulang punggung Azure.
- Mengonfigurasi titik akhir penyimpanan sehingga hanya sumber daya dari subnet tertentu yang dapat mengakses penyimpanan.
- Mengonfirmasi bahwa sumber daya di luar subnet tertentu tidak dapat mengakses penyimpanan. 

> Untuk semua sumber daya di lab ini, kita menggunakan wilayah **US Timur**. Verifikasi dengan instruktur Anda bahwa ini adalah wilayah yang akan digunakan untuk kelas. 

## <a name="lab-objectives"></a>Tujuan lab

Di lab ini, Anda akan menyelesaikan latihan berikut:

- Latihan 1: Titik akhir layanan dan penyimpanan keamanan

## <a name="service-endpoints-and-securing-storage-diagram"></a>Titik Akhir Layanan dan diagram Mengamankan Penyimpanan

![gambar](https://user-images.githubusercontent.com/91347931/157534883-29664a05-85d1-4c70-99a7-f16d2360755d.png)

## <a name="instructions"></a>Instruksi

### <a name="exercise-1-service-endpoints-and-security-storage"></a>Latihan 1: Titik akhir layanan dan penyimpanan keamanan

### <a name="estimated-timing-45-minutes"></a>Perkiraan waktu: 45 menit

Dalam latihan ini, Anda akan menyelesaikan tugas-tugas berikut:

- Tugas 1: Membuat jaringan virtual
- Tugas 2: Menambahkan subnet ke jaringan virtual dan mengonfigurasi titik akhir penyimpanan
- Tugas 3: Mengonfigurasi grup keamanan jaringan untuk membatasi akses ke subnet
- Tugas 4: Membuat akun penyimpanan dengan berbagi file
- Tugas 5: Menyebarkan komputer virtual ke subnet yang ditunjuk
- Tugas 6: Uji koneksi penyimpanan dari subnet pribadi untuk mengonfirmasi bahwa akses diizinkan
- Tugas 7: Menguji koneksi penyimpanan dari subnet publik untuk mengonfirmasi bahwa akses ditolak 

#### <a name="task-1-create-a-virtual-network"></a>Tugas 1: Membuat jaringan virtual

Dalam tugas ini, Anda akan membuat jaringan virtual.

1. Masuk ke portal Microsoft Azure **`https://portal.azure.com/`** .

    >**Catatan**: Masuk ke portal Microsoft Azure menggunakan akun yang memiliki peran Pemilik atau Kontributor dalam langganan Azure yang Anda gunakan untuk lab ini.

2. Di portal Microsoft Azure, di kotak teks **Cari sumber daya, layanan, dan dokumen** di bagian atas halaman portal Microsoft Azure, ketik **Jaringan virtual** dan tekan tombol **Enter**.

3. Pada panel **Jaringan Virtual**, klik **+ Buat**.

4. Pada tab **Dasar** di bilah **Buat jaringan virtual**, tentukan pengaturan berikut (biarkan yang lain dengan nilai defaultnya) dan klik **Berikutnya: Alamat IP**:

    |Pengaturan|Nilai|
    |---|---|
    |Langganan|nama langganan Azure yang Anda gunakan di lab ini|
    |Grup sumber daya|klik **Buat baru** dan ketik nama **AZ500LAB12**|
    |Nama|**myVirtualNetwork**|
    |Wilayah|**(AS) AS Timur**|

5. Pada tab **alamat IP** bilah **Buat jaringan virtual**, atur **ruang alamat IPv4** ke **10.0.0.0/16**, di **Nama subnet**, klik **default** dan, pada bilah **Edit subnet**, tentukan pengaturan berikut dan klik **Simpan**:

    |Pengaturan|Nilai|
    |---|---|
    |Nama subnet|**Publik**|
    |Rentang alamat subnet|**10.0.0.0/24**|

6. Kembali ke tab **alamat IP** pada tab **Buat jaringan virtual**, klik **Tinjau + buat**.

7. Pada tab **Tinjau + buat** bilah **Buat jaringan virtual**, klik **Buat**.

#### <a name="task-2-add-a-subnet-to-the-virtual-network-and-configure-a-storage-endpoint"></a>Tugas 2: Menambahkan subnet ke jaringan virtual dan mengonfigurasi titik akhir penyimpanan

Dalam tugas ini, Anda akan membuat subnet lain dan mengaktifkan titik akhir layanan di subnet tersebut. Titik akhir layanan diaktifkan per layanan, per subnet. 

1. Di portal Azure, navigasikan kembali ke bilah **Jaringan Virtual**.

2. Pada bilah **Jaringan virtual**, klik entri **myVirtualNetwork**.

3. Pada bilah **myVirtualNetwork**, di bagian **Pengaturan**, klik **Subnet**.

4. Pada bilah **myVirtualNetwork \| Subnets**, klik **+ Subnet**. 

5. Pada bilah **Tambahkan subnet**, tentukan pengaturan berikut (biarkan yang lain dengan nilai defaultnya):

    |Pengaturan|Nilai|
    |---|---|
    |Nama subnet|**Privat**|
    |Rentang alamat subnet|**10.0.1.0/24**|
    |Titik akhir layanan|**Microsoft.Storage**|

6. Pada bilah **Tambahkan subnet**, klik **Simpan** untuk menambahkan subnet baru.

    >**Catatan**: Jaringan virtual sekarang memiliki dua subnet: Publik dan Privat. 
    
#### <a name="task-3-configure-a-network-security-group-to-restrict-access-to-the-subnet"></a>Tugas 3: Mengonfigurasi grup keamanan jaringan untuk membatasi akses ke subnet

Dalam tugas ini, Anda akan membuat grup keamanan jaringan dengan dua aturan keamanan keluar (Penyimpanan dan internet) dan satu aturan keamanan masuk (RDP). Anda juga akan mengaitkan grup keamanan jaringan dengan subnet Privat. Ini akan membatasi lalu lintas keluar dari Azure VM yang terhubung ke subnet tersebut.

1. Di portal Microsoft Azure, dalam kotak teks **Cari sumber daya, layanan, dan dokumen** di bagian atas halaman portal Microsoft Azure, ketik **Grup keamanan jaringan** dan tekan tombol **Enter**.

2. Pada panel **Kelompok keamanan jaringan**, klik **+ Buat**.

3. Pada tab **Dasar** pada tab **Buat kelompok keamanan jaringan**, tentukan pengaturan berikut: 

    |Pengaturan|Nilai|
    |---|---|
    |Langganan|nama langganan Azure yang Anda gunakan di lab ini|
    |Grup sumber daya|**AZ500LAB12**|
    |Nama|**myNsgPrivate**|
    |Wilayah|**AS Timur**|

4. Klik **Review + create**, lalu klik **Create**.

    >**Catatan**: Pada langkah berikutnya, Anda akan membuat aturan keamanan keluar yang memungkinkan komunikasi ke layanan Azure Storage. 

5. Di portal Azure, navigasikan kembali ke bilah **Grup keamanan jaringan** dan klik entri **myNsgPrivate**.

6. Pada bilah **myNsgPrivate**, di bagian **Pengaturan**, klik **Aturan keamanan keluar**.

7. Pada bilah **myNsgPrivate \| Aturan keamanan keluar**, klik **+ Tambahkan**.

8. Pada bilah **Tambahkan aturan keamanan keluar**, tentukan pengaturan berikut untuk secara eksplisit mengizinkan lalu lintas keluar ke Azure Storage (biarkan semua nilai lainnya dengan pengaturan defaultnya): 

    |Pengaturan|Nilai|
    |---|---|
    |Sumber|**Tag Layanan**|
    |Tag layanan sumber|**JaringanVirtual**|
    |Rentang port sumber|**\***|
    |Tujuan|**Tag Layanan**|
    |Tag layanan tujuan|**Penyimpanan**|
    |Rentang port tujuan|**\***|
    |Protokol|**Mana pun**|
    |Tindakan|**Izinkan**|
    |Prioritas|**1000**|
    |Nama|**Allow-Storage-All**|

9. Pada bilah **Tambahkan aturan keamanan keluar**, klik **Tambahkan** untuk membuat aturan keluar baru. 

10. Pada bilah **myNsgPrivate**, di bagian **Pengaturan**, klik **Aturan keamanan keluar**, lalu klik **+ Tambahkan**.

11. Pada bilah **Tambahkan aturan keamanan keluar**, tentukan pengaturan berikut untuk secara eksplisit menolak lalu lintas keluar ke Internet (biarkan semua nilai lain dengan pengaturan defaultnya): 

    |Pengaturan|Nilai|
    |---|---|
    |Sumber|**Tag Layanan**|
    |Tag layanan sumber|**JaringanVirtual**|
    |Rentang port sumber|**\***|
    |Tujuan|**Tag Layanan**|
    |Tag layanan tujuan|**Internet**|
    |Rentang port tujuan|**\***|
    |Protokol|**Mana pun**|
    |Tindakan|**Tolak**|
    |Prioritas|**1100**|
    |Nama|**Deny-Internet-All**|

    >**Catatan**: Aturan ini mengesampingkan aturan default di semua grup keamanan jaringan yang memungkinkan komunikasi internet keluar. 

    >**Catatan**: Pada langkah berikutnya, Anda akan membuat aturan keamanan masuk yang mengizinkan lalu lintas Protokol Desktop Jarak Jauh (RDP) ke subnet. Aturan ini mengesampingkan aturan keamanan default yang menolak semua lalu lintas masuk dari internet. Koneksi Desktop Jarak Jauh diizinkan ke subnet sehingga konektivitas dapat diuji di langkah selanjutnya.

12. Pada bilah **myNsgPrivate**, di bagian **Pengaturan**, klik **Aturan keamanan masuk** lalu klik **+ Tambahkan**.

13. Pada bilah **Tambahkan aturan keamanan masuk**, tentukan pengaturan berikut (biarkan semua nilai lain dengan nilai defaultnya): 

    |Pengaturan|Nilai|
    |---|---|
    |Sumber|**Mana pun**|
    |Rentang port sumber|**\***|
    |Tujuan|**Tag Layanan**|
    |Tag layanan tujuan|**JaringanVirtual**|
    |Rentang port tujuan|**3389**|
    |Protokol|**TCP**|
    |Tindakan|**Izinkan**|
    |Prioritas|**1200**|                                                    
    |Nama|**Allow-RDP-All**|

14. Pada bilah **Tambahkan aturan keamanan masuk**, klik **Tambahkan** untuk membuat aturan masuk baru. 

    >**Catatan**: Sekarang Anda akan mengaitkan grup keamanan jaringan dengan subnet Privat.

15. Pada panel **Subnet**, pilih **+ Kaitkan** dan tentukan pengaturan berikut di bagian **Kaitkan subnet**, lalu klik **OK**:

    |Pengaturan|Nilai|
    |---|---|
    |Jaringan virtual|**myVirtualNetwork**|
    |Subnet|**Privat**|
    
#### <a name="task-4-create-a-storage-account-with-a-file-share"></a>Tugas 4: Membuat akun penyimpanan dengan berbagi file

Dalam tugas ini, Anda akan membuat akun penyimpanan dengan berbagi file dan mendapatkan kunci akun penyimpanan.  

1. Di portal Microsoft Azure, di kotak teks **Cari sumber daya, layanan, dan dokumen** di bagian atas halaman portal Microsoft Azure, ketik **Akun penyimpanan** dan tekan tombol **Enter**.

2. Pada bilah **Akun penyimpanan**, klik **+ Buat**.

3. Pada tab **Dasar** pada bilah **Buat akun penyimpanan**, tentukan pengaturan berikut (biarkan yang lain dengan nilai defaultnya):

    |Pengaturan|Nilai|
    |---|---|
    |Langganan|nama langganan Azure yang Anda gunakan di lab ini|
    |Grup sumber daya|**AZ500LAB12**|
    |Nama akun penyimpanan|nama unik global apa pun yang panjangnya antara 3 dan 24 yang terdiri atas huruf dan angka|
    |Lokasi|**(US) US Timur**|
    |Performa|**Standar (akun general-purpose v2)**|
    |Redundansi|**Penyimpanan yang berlebihan secara lokal (LRS)**|

4. Pada tab **Dasar** di bilah **Buat akun penyimpanan**, klik **Tinjau + Buat**, tunggu proses validasi selesai, dan klik **Buat**.

    >**Catatan**: Tunggu hingga akun Penyimpanan dibuat. Proses ini memerlukan waktu sekitar 2 menit.

5. Di portal Microsoft Azure, di kotak teks **Cari sumber daya, layanan, dan dokumen** di bagian atas halaman portal Microsoft Azure, ketik **Grup sumber daya** dan tekan tombol **Enter**.

6. Pada bilah **Grup sumber daya**, dalam daftar grup sumber daya, klik entri **AZ500LAB12**.

7. Pada bilah grup sumber daya **AZ500LAB12**, dalam daftar sumber daya, klik entri yang mewakili akun penyimpanan yang baru dibuat.

8. Pada bilah **Ringkasan** akun penyimpanan, klik **Berbagi File** di bawah tab **Penyimpanan data**, lalu klik **+ Berbagi File**.

9. Pada bilah **Berbagi file baru**, tentukan pengaturan berikut:

    |Pengaturan|Nilai|
    |---|---|
    |Nama|**my-file-share**|

10. Pada bilah **Berbagi file baru**, klik **Buat**.

    >**Catatan**: Sekarang, ambil dan rekam skrip PowerShell yang membuat pemetaan drive ke berbagi file Azure. 

11. Pada bilah akun penyimpanan, dalam daftar file yang dibagikan, klik **my-file-share**.

12. Pada bilah **my-file-share**, klik **Hubungkan**.

13. Pada bilah **Connect**, pada tab **Windows**, salin skrip PowerShell yang membuat pemetaan drive Z ke berbagi file. 

    >**Catatan**: Rekam skrip ini. Anda akan memerlukan skrip ini nanti di lab ini untuk memetakan berbagi file dari komputer virtual Azure pada subnet **Pribadi**.

14. Navigasikan kembali ke panel akun penyimpanan, lalu di bagian **Keamanan + jaringan**, klik **Jaringan**.

15. Di bawah panel **Firewalls dan jaringan virtual**, pilih opsi **Diaktifkan dari jaringan virtual dan alamat IP yang dipilih** dan klik tautan **+ Tambahkan jaringan virtual yang ada**. 

16. Pada bilah **Tambahkan jaringan**, tentukan pengaturan berikut:

    |Pengaturan|Nilai|
    |---|---|
    |Langganan|nama langganan Azure yang Anda gunakan di lab ini|
    |Jaringan virtual|**myVirtualNetwork**|
    |Subnet|**Privat**|

17. Pada bilah **Tambahkan jaringan**, klik **Tambahkan**. 

18. Kembali ke bilah akun penyimpanan, klik **Simpan**.

    >**Catatan**: Pada titik ini di lab Anda telah mengonfigurasi jaringan virtual, grup keamanan jaringan, dan akun penyimpanan dengan berbagi file. 

#### <a name="task-5-deploy-virtual-machines-into-the-designated-subnets"></a>Tugas 5: Menyebarkan komputer virtual ke subnet yang ditunjuk

Dalam tugas ini, Anda akan membuat dua komputer virtual satu di subnet Pribadi dan satu di subnet Publik. 

>**Catatan**: Komputer virtual pertama akan terhubung ke subnet Privat.

1. Di portal Microsoft Azure, dalam kotak teks **Cari sumber daya, layanan, dan dokumen** di bagian atas halaman portal Microsoft Azure, ketik **Mesin virtual** dan tekan tombol **Enter**.

2. Pada bilah **Komputer virtual**, klik **+ Buat** dan, di daftar menurun, klik **+ Komputer Virtual Azure**

3. Pada tab **Dasar** pada bilah **Buat komputer virtual**, tentukan pengaturan berikut (biarkan yang lain dengan nilai defaultnya):

    |Pengaturan|Nilai|
    |---|---|
    |Langganan|nama langganan Azure yang akan Anda gunakan di lab ini|
    |Grup sumber daya|**AZ500LAB12**|
    |Nama komputer virtual|**myVmPrivate**|
    |Wilayah|**(AS) AS Timur**|
    |Gambar|**Pusat Data Windows Server 2022: Edisi Azure - Generasi 2**|
    |Nama Pengguna|**admin lokal**|
    |Kata sandi|**Gunakan kata sandi pribadi Anda yang dibuat di Lab 04 > Latihan 1 > Tugas 1 > Langkah 9.**|
    |Port masuk publik|**Tidak ada**|
    |Sudah memiliki lisensi Windows Server|**Tidak**|

    >**Catatan**: Untuk port masuk publik, kami akan mengandalkan NSG yang telah dibuat sebelumnya. 

4. Klik **Berikutnya: Disk >** dan, pada tab **Disk** panel **Buat komputer virtual**, atur **jenis disk OS** ke **HDD Standar** dan klik **Berikutnya: Jaringan >** .

5. Klik **Berikutnya: Jaringan >** , pada tab **Jaringan** di bilah **Buat komputer virtual**, tentukan pengaturan berikut (biarkan yang lain dengan nilai defaultnya):

    |Pengaturan|Nilai|
    |---|---|
    |Jaringan virtual|**myVirtualNetwork**|
    |Subnet|**Privat (10.0.1.0/24)**|
    |IP Publik|**(new)myVmPrivate-ip**|
    |Grup keamanan jaringan NIC|**Tidak ada**|

6. Klik **Berikutnya: Manajemen >** , pada tab **Manajemen** panel **Buat komputer virtual**, terima pengaturan default dan klik **Tinjau + buat**.

7. Pada panel **Tinjau + buat**, pastikan validasi berhasil dan klik **Buat**.

    >**Catatan**: Komputer virtual kedua akan terhubung ke subnet Publik.

8. Pada bilah **Komputer virtual**, klik **+ Tambahkan** dan, di daftar menurun, klik **+ Komputer Virtual Azure**.

9. Pada tab **Dasar** pada bilah **Buat komputer virtual**, tentukan pengaturan berikut (biarkan yang lain dengan nilai defaultnya):

    |Pengaturan|Nilai|
    |---|---|
    |Langganan|nama langganan Azure yang akan Anda gunakan di lab ini|
    |Grup sumber daya|**AZ500LAB12**|
    |Nama komputer virtual|**myVmPublic**|
    |Wilayah|**(AS) AS Timur**|
    |Gambar|**Pusat Data Windows Server 2022: Edisi Azure - Generasi 2**|
    |Nama Pengguna|**admin lokal**|
    |Kata sandi|**Gunakan kata sandi pribadi Anda yang dibuat di Lab 04 > Latihan 1 > Tugas 1 > Langkah 9.**|
    |Port masuk publik|**Tidak ada**|
    |Sudah memiliki lisensi Windows Server|**Tidak**|

    >**Catatan**: Untuk port masuk publik, kami akan mengandalkan NSG yang telah dibuat sebelumnya. 

10. Klik **Berikutnya: Disk >** dan, pada tab **Disk** panel **Buat komputer virtual**, atur **jenis disk OS** ke **HDD Standar** dan klik **Berikutnya: Jaringan >** .

11. Klik **Berikutnya: Jaringan >** , pada tab **Jaringan** di bilah **Buat komputer virtual**, tentukan pengaturan berikut (biarkan yang lain dengan nilai defaultnya):

    |Pengaturan|Nilai|
    |---|---|
    |Jaringan virtual|**myVirtualNetwork**|
    |Subnet|**Publik (10.0.0.0/24)**|
    |IP Publik|**(new)myVmPublic-ip**|
    |Grup keamanan jaringan NIC|**Tidak ada**|

12. Klik **Berikutnya: Manajemen >** , pada tab **Manajemen** panel **Buat komputer virtual**, terima pengaturan default dan klik **Tinjau + buat**.

13. Pada panel **Tinjau + buat**, pastikan validasi berhasil dan klik **Buat**.

    >**Catatan**: Anda dapat melanjutkan ke tugas berikutnya setelah penerapan Azure VM **myVMPrivate** selesai.

#### <a name="task-6-test-the-storage-connection-from-the-private-subnet-to-confirm-that-access-is-allowed"></a>Tugas 6: Uji koneksi penyimpanan dari subnet pribadi untuk mengonfirmasi bahwa akses diizinkan

Dalam tugas ini, Anda akan terhubung ke komputer virtual myVMPrivate melalui Desktop Jauh dan memetakan drive ke berbagi file. 

1. Navigasikan kembali ke bilah **Komputer virtual**. 

2. Pada bilah **Komputer virtual**, klik entri **myVMPrivate**.

3. Pada bilah **myVMPrivate**, klik **Hubungkan** dan, di menu menurun, klik **RDP**. 

4. Klik **Unduh File RDP** dan gunakan untuk menyambung ke Azure VM **myVMPrivate** melalui Desktop Jauh. Saat diminta untuk mengautentikasi, berikan kredensial berikut:

    |Pengaturan|Nilai|
    |---|---|
    |Nama pengguna|**localadmin**|
    |Kata sandi|**Gunakan kata sandi pribadi Anda yang dibuat di Lab 04 > Latihan 1 > Tugas 1 > Langkah 9.**|

    >**Catatan**: Tunggu sesi Remote Desktop terbuka dan Server Manager dimuat.

    >**Catatan**: Anda sekarang akan memetakan drive Z ke berbagi File Azure dalam sesi Desktop Jauh ke komputer Windows Server 2022

5. Dalam sesi Desktop Jauh ke **myVMPrivate**, klik **Mulai** lalu klik **Windows PowerShell ISE**.

6. Dalam jendela **Windows PowerShell ISE**, buka panel **Script**, lalu tempel dan jalankan skrip PowerShell yang Anda rekam sebelumnya di lab ini. Script memiliki format berikut:

    ```powershell
    $connectTestResult = Test-NetConnection -ComputerName <storage_account_name>.file.core.windows.net -Port 445
    if ($connectTestResult.TcpTestSucceeded) {
       # Save the password so the drive will persist on reboot
       cmd.exe /C "cmdkey /add:`"<storage_account_name>.file.core.windows.net`" /user:`"localhost\<storage_account_name>`"  /pass:`"<storage_account_key>`""
       # Mount the drive
       New-PSDrive -Name Z -PSProvider FileSystem -Root "\\<storage_account_name>.file.core.windows.net\my-file-share" -Persist
    } else {
       Write-Error -Message "Unable to reach the Azure storage account via port 445. Check to make sure your organization or ISP is not blocking port 445, or use Azure P2S VPN, Azure S2S VPN, or Express Route to tunnel SMB traffic over a different port."
    }
    ```
    >**Catatan**: Tempat penampung `<storage-account-name>` mewakili nama akun penyimpanan yang menghosting berbagi file dan `<storage_account_name>` satu kunci utamanya

7. Mulai File Explorer dan verifikasi bahwa Z: pemetaan drive telah berhasil dibuat.

8. Selanjutnya, dari panel konsol **Windows PowerShell ISE**, jalankan perintah berikut untuk memverifikasi bahwa komputer virtual tidak memiliki konektivitas keluar ke internet:

    ```powershell
    Test-NetConnection -ComputerName www.bing.com -Port 80
    ```

    >**Catatan**: Pengujian akan gagal karena grup keamanan jaringan yang terkait dengan subnet Privat tidak mengizinkan akses keluar ke internet.

9. Hentikan sesi Desktop Jauh ke Azure VM **myVMPrivate**.

    >**Catatan**: Pada titik ini, Anda telah mengonfirmasi bahwa komputer virtual di subnet Pribadi dapat mengakses akun penyimpanan. 

####  <a name="task-7-test-the-storage-connection-from-the-public-subnet-to-confirm-that-access-is-denied"></a>Tugas 7: Menguji koneksi penyimpanan dari subnet publik untuk mengonfirmasi bahwa akses ditolak 

1. Navigasikan kembali ke bilah **Komputer virtual**. 

2. Pada bilah **Komputer virtual**, klik entri **myVMPublic**.

3. Pada bilah **myVMPublic**, klik **Hubungkan** dan, di menu menurun, klik **RDP**. 

4. Klik **Unduh File RDP** dan gunakan untuk menyambung ke Azure VM **myVMPublic** melalui Desktop Jauh. Saat diminta untuk mengautentikasi, berikan kredensial berikut:

    |Pengaturan|Nilai|
    |---|---|
    |Nama pengguna|**localadmin**|
    |Kata sandi|**Gunakan kata sandi pribadi Anda yang dibuat di Lab 04 > Latihan 1 > Tugas 1 > Langkah 9.**|

    >**Catatan**: Tunggu sesi Remote Desktop terbuka dan Server Manager dimuat.

    >**Catatan**: Anda sekarang akan memetakan drive Z ke berbagi File Azure dalam sesi Desktop Jauh ke komputer Windows Server 2022

5. Dalam sesi Desktop Jauh ke **myVMPublic**, klik **Mulai** lalu klik **Windows PowerShell ISE**.

6. Dalam jendela **Windows PowerShell ISE**, buka panel **Script**, lalu tempel dan jalankan skrip PowerShell yang sama dengan yang Anda jalankan dalam sesi Remote Desktop ke Azure VM **myVMPrivate**.

    >**Catatan**: Kali ini, Anda akan menerima **New-PSDrive : Kesalahan akses ditolak**. 

    >**Catatan**: Akses ditolak karena komputer virtual *myVmPublic* disebarkan di subnet Publik. Subnet publik tidak memiliki titik akhir layanan yang diaktifkan untuk Azure Storage. Akun penyimpanan hanya mengizinkan akses jaringan dari subnet Privat.

7. Selanjutnya, dari panel konsol **Windows PowerShell ISE**, jalankan perintah berikut untuk memverifikasi bahwa komputer virtual memiliki konektivitas keluar ke internet: 

    ```powershell
    Test-NetConnection -ComputerName www.bing.com -Port 80
    ```

    >**Catatan**: Pengujian akan berhasil karena tidak ada grup keamanan jaringan yang terkait dengan subnet Publik.

8. Hentikan sesi Desktop Jauh ke **myVMPublic** Azure VM.

    >**Catatan**: Pada titik ini, Anda telah mengonfirmasi bahwa komputer virtual di subnet Publik tidak dapat mengakses akun penyimpanan, tetapi memiliki akses ke internet.


**Membersihkan sumber daya**

> Jangan lupa untuk menghapus sumber daya Azure yang baru dibuat dan yang tidak diperlukan lagi. Menghapus sumber daya yang tidak terpakai memastikan Anda tidak akan dikenakan biaya tak terduga. 

1. Buka Cloud Shell dengan mengklik ikon pertama di kanan atas Portal Azure. Jika diminta, pilih **PowerShell** dan **Buat penyimpanan**.

2. Pastikan **PowerShell** dipilih di menu drop-down di sudut kiri atas panel Cloud Shell.

3. Di sesi PowerShell dalam panel Cloud Shell, jalankan perintah berikut untuk menghapus grup sumber daya yang Anda buat di lab ini:
  
    ```powershell
    Remove-AzResourceGroup -Name "AZ500LAB12" -Force -AsJob
    ```

4.  Tutup panel **Cloud Shell**. 
 
