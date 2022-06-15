---
lab:
  title: 07 - Grup Keamanan Jaringan dan Grup Keamanan Aplikasi
  module: Module 02 - Implement Platform Protection
ms.openlocfilehash: d7cfed1e861215cf32c3b51c4a07aa6886575000
ms.sourcegitcommit: 2f08105eaaf0413d3ec3c12a3b078678151fd211
ms.translationtype: HT
ms.contentlocale: id-ID
ms.lasthandoff: 04/04/2022
ms.locfileid: "145195924"
---
# <a name="lab-07-network-security-groups-and-application-security-groups"></a>Lab 07: Grup Keamanan Jaringan dan Grup Keamanan Aplikasi
# <a name="student-lab-manual"></a>Panduan lab siswa

## <a name="lab-scenario"></a>Skenario lab

Anda telah diminta untuk mengimplementasikan infrastruktur jaringan virtual organisasi Anda dan mengujinya untuk memastikan infrastruktur tersebut berfungsi dengan benar. Secara khusus:

- Organisasi tersebut memiliki dua grup server: Server Web dan Server Manajemen.
- Setiap grup server harus berada dalam Kelompok Keamanan Aplikasinya sendiri. 
- Anda seharusnya dapat melakukan RDP ke server manajemen, tetapi bukan Server Web.
- Server Web harus menampilkan halaman web IIS ketika diakses dari internet. 
- Aturan kelompok keamanan jaringan harus digunakan untuk mengontrol akses jaringan. 

> Untuk semua sumber daya di lab ini, kita menggunakan wilayah **US Timur**. Verifikasi dengan instruktur Anda bahwa ini adalah wilayah yang akan digunakan untuk kelas. 

## <a name="lab-objectives"></a>Tujuan lab

Di lab ini, Anda akan menyelesaikan latihan berikut:

- Latihan 1: Buat infrastruktur jaringan virtual
- Latihan 2: Sebarkan mesin virtual dan uji filter jaringan

## <a name="network-and-application-security-groups-diagram"></a>Diagram Jaringan dan Kelompok Keamanan Aplikasi

![gambar](https://user-images.githubusercontent.com/91347931/157526438-6da4f68b-db88-4931-a041-8474e66d3fe5.png)

## <a name="instructions"></a>Instruksi

### <a name="exercise-1-create-the-virtual-networking-infrastructure"></a>Latihan 1: Buat infrastruktur jaringan virtual

### <a name="estimated-timing-20-minutes"></a>Perkiraan waktu: 20 menit

> Untuk semua sumber daya di lab ini, kita menggunakan wilayah **Timur (US)** . Verifikasi dengan instruktur Anda bahwa ini adalah wilayah yang akan digunakan untuk kelas Anda. 

Dalam latihan ini, Anda akan menyelesaikan tugas-tugas berikut:

- Tugas 1: Membuat jaringan virtual dengan satu subnet.
- Tugas 2: Buat dua kelompok keamanan aplikasi.
- Tugas 3: Buat kelompok keamanan jaringan dan kaitkan dengan subnet jaringan virtual.
- Tugas 4: Buat aturan keamanan NSG masuk ke semua lalu lintas ke server web dan RDP ke server manajemen.

#### <a name="task-1--create-a-virtual-network"></a>Tugas 1:  Membuat jaringan virtual

Dalam tugas ini, Anda akan membuat jaringan virtual untuk digunakan dengan kelompok keamanan jaringan dan aplikasi. 

1. Masuk ke portal Microsoft Azure **`https://portal.azure.com/`** .

    >**Catatan**: Masuk ke portal Microsoft Azure menggunakan akun yang memiliki peran Pemilik atau Kontributor dalam langganan Azure yang Anda gunakan untuk lab ini.

2. Di portal Microsoft Azure, di kotak teks **Cari sumber daya, layanan, dan dokumen** di bagian atas laman portal Microsoft Azure, ketik **Jaringan virtual** dan tekan kunci **Enter**.

3. Pada panel **Jaringan virtual**, klik **+ Buat**.

4. Pada tab **Dasar** di panel **Buat jaringan virtual**, tentukan pengaturan berikut (biarkan yang lain dengan nilai defaultnya) dan klik **Berikutnya: Alamat IP**:

    |Pengaturan|Nilai|
    |---|---|
    |Langganan|nama langganan Azure yang Anda gunakan di lab ini|
    |Grup sumber daya|klik **Buat baru** dan ketik nama **AZ500LAB07**|
    |Nama|**myVirtualNetwork**|
    |Wilayah|**US Timur**|

5. Pada tab **alamat IP** pada panel **Buat jaringan virtual**, setel **ruang alamat IPv4** ke **10.0.0.0/16**, dan, jika perlu, di kolom **Nama subnet**, klik **default**, pada panel **Edit subnet**, tentukan setelan berikut dan klik **Simpan**:

    |Pengaturan|Nilai|
    |---|---|
    |Nama subnet|**default**|
    |Rentang alamat subnet|**10.0.0.0/24**|

6. Kembali ke tab **alamat IP** pada tab **Buat jaringan virtual**, klik **Tinjau + buat**.

7. Pada tab **Tinjau + buat** tab **Buat jaringan virtual**, klik **Buat**.

#### <a name="task-2--create-application-security-groups"></a>Tugas 2:  Buat kelompok keamanan aplikasi

Dalam tugas ini, Anda akan membuat kelompok keamanan aplikasi.

1. Di portal Microsoft Azure, dalam kotak teks **Cari sumber daya, layanan, dan dokumen** di bagian atas laman portal Microsoft Azure, ketik **Kelompok keamanan aplikasi** dan tekan kunci **Enter**.

2. Pada panel **Kelompok keamanan aplikasi**, klik **+ Buat**.

3. Pada tab **Dasar-dasar** panel **Buat kelompok keamanan aplikasi**, tentukan setelan berikut: 

    |Pengaturan|Nilai|
    |---|---|
    |Grup sumber daya|**AZ500LAB07**|
    |Nama|**myAsgWebServers**|
    |Wilayah|**AS Timur**|

    >**Catatan**: Grup ini akan untuk server web.

4. Klik **Review + create**, lalu klik **Create**.

5. Navigasi kembali ke panel **Kelompok keamanan aplikasi** dan klik **+ Buat**.

6. Pada tab **Dasar-dasar** panel **Buat kelompok keamanan aplikasi**, tentukan setelan berikut: 

    |Pengaturan|Nilai|
    |---|---|
    |Grup sumber daya|**AZ500LAB07**|
    |Nama|**myAsgMgmtServers**|
    |Wilayah|**AS Timur**|

    >**Catatan**: Grup ini akan menjadi untuk server manajemen.

7. Klik **Review + create**, lalu klik **Create**.

#### <a name="task-3--create-a-network-security-group-and-associate-the-nsg-to-the-subnet"></a>Tugas 3:  Membuat kelompok keamanan jaringan dan mengaitkan NSG ke subnet

Dalam tugas ini, Anda akan membuat kelompok keamanan jaringan. 

1. Di portal Azure, dalam kotak teks **Cari sumber daya, layanan, dan dokumen** di bagian atas laman portal Azure, ketik **Kelompok keamanan jaringan** dan tekan kunci **Enter**.

2. Pada panel **Kelompok keamanan jaringan**, klik **+ Buat**.

3. Pada tab **Dasar** pada tab **Buat kelompok keamanan jaringan**, tentukan pengaturan berikut: 

    |Pengaturan|Nilai|
    |---|---|
    |Langganan|nama langganan Azure yang Anda gunakan di lab ini|
    |Grup sumber daya|**AZ500LAB07**|
    |Nama|**myNsg**|
    |Wilayah|**US Timur**|

4. Klik **Review + create**, lalu klik **Create**.

5. Di portal Azure, navigasikan kembali ke panel **Kelompok keamanan jaringan** dan klik entri **myNsg**.

6. Pada panel **myNsg**, di bagian **Setelan**, klik **Subnet**, lalu klik **+ Kaitkan**. 

7. Pada panel **Kaitkan subnet**, tentukan setelan berikut dan klik **OK**:

    |Pengaturan|Nilai|
    |---|---|
    |Jaringan virtual|**myVirtualNetwork**|
    |Subnet|**default**|

#### <a name="task-4-create-inbound-nsg-security-rules-to-all-traffic-to-web-servers-and-rdp-to-the-management-servers"></a>Tugas 4: Buat aturan keamanan NSG masuk ke semua lalu lintas ke server web dan RDP ke server manajemen. 

1. Pada panel **myNsg**, di bagian **Setelan**, klik **Aturan keamanan masuk**.

2. Tinjau aturan keamanan masuk default lalu klik **+ Tambahkan**.

3. Pada panel **Tambahkan aturan keamanan masuk**, tentukan pengaturan berikut untuk mengizinkan port TCP 80 dan 443 ke grup keamanan aplikasi **myAsgWebServers** (biarkan semua nilai lain dengan nilai defaultnya): 

    |Pengaturan|Nilai|
    |---|---|
    |Tujuan|di daftar drop-down, pilih **Kelompok keamanan aplikasi** lalu klik **myAsgWebServers**|
    |Rentang port tujuan|**80,443**|
    |Protokol|**TCP**|
    |Prioritas|**100**|                                                    
    |Nama|**Allow-Web-All**|

4. Pada panel **Tambahkan aturan keamanan masuk**, klik **Tambahkan** untuk membuat aturan masuk baru. 

5. Pada panel **myNsg**, di bagian **Setelan**, klik **Aturan keamanan masuk**, lalu klik **+ Tambahkan**.

6. Pada panel **Tambahkan aturan keamanan masuk**, tentukan setelan berikut untuk mengizinkan port RDP (TCP 3389) ke grup keamanan aplikasi **myAsgMgmtServers** (biarkan semua nilai lain dengan nilai defaultnya): 

    |Pengaturan|Nilai|
    |---|---|
    |Tujuan|di daftar drop-down, pilih **Kelompok keamanan aplikasi** lalu klik **myAsgMgmtServers**|
    |Rentang port tujuan|**3389**|
    |Protokol|**TCP**|
    |Prioritas|**110**|                                                    
    |Nama|**Allow-RDP-All**|

7. Pada panel **Tambahkan aturan keamanan masuk**, klik **Tambahkan** untuk membuat aturan masuk baru. 

> Hasil: Anda telah menyebarkan jaringan virtual, keamanan jaringan dengan aturan keamanan masuk, dan dua kelompok keamanan aplikasi. 

### <a name="exercise-2-deploy-virtual-machines-and-test-network-filters"></a>Latihan 2: Sebarkan mesin virtual dan uji filter jaringan

### <a name="estimated-timing-25-minutes"></a>Perkiraan waktu: 25 menit

Dalam latihan ini, Anda akan menyelesaikan tugas-tugas berikut:

- Tugas 1: Buat mesin virtual untuk digunakan sebagai server web.
- Tugas 2: Buat mesin virtual untuk digunakan sebagai server manajemen. 
- Tugas 3: Kaitkan setiap antarmuka jaringan mesin virtual ke kelompok keamanan aplikasinya.
- Tugas 4: Uji pemfilteran lalu lintas jaringan.

#### <a name="task-1-create-a-virtual-machine-to-use-as-a-web-server"></a>Tugas 1: Buat mesin virtual untuk digunakan sebagai server web.

Dalam tugas ini, Anda akan membuat mesin virtual untuk digunakan sebagai server web.

1. Di portal Microsoft Azure, di kotak teks **Cari sumber daya, layanan, dan dokumen** di bagian atas halaman portal Microsoft Azure, ketik **Mesin virtual** dan tekan tombol **Enter**.

2. Pada panel **Mesin virtual**, klik **+ Buat** dan, di daftar dropdown, klik **+ Mesin virtual Azure**.

3. Pada tab **Dasar** pada panel **Buat mesin virtual**, tentukan pengaturan berikut (biarkan yang lain dengan nilai defaultnya):

   |Pengaturan|Nilai|
   |---|---|
   |Langganan|nama langganan Azure yang akan Anda gunakan di lab ini|
   |Grup sumber daya|**AZ500LAB07**|
   |Nama komputer virtual|**myVmWeb**|
   |Wilayah|**(AS) AS Timur**|
   |Gambar|**Pusat Data Windows Server 2022: Azure Edition- Gen2**|
   |Ukuran|**Standar D2s v3**|
   |Nama Pengguna|**Siswa**|
   |Kata sandi|**Gunakan kata sandi pribadi Anda yang dibuat di Lab 04 > Latihan 1 > Tugas 1 > Langkah 9.**|
   |Konfirmasikan kata sandi|**Ketik ulang sandi Anda**|
   |Port masuk publik|**Tidak ada**|
   |Apakah Anda ingin menggunakan Lisensi Server Windows yang ada |**Tidak**|

    >**Catatan**: Untuk port masuk publik, kami akan mengandalkan NSG yang telah dibuat sebelumnya. 

4. Klik **Berikutnya: Disk >** dan, pada tab **Disk** panel **Buat mesin virtual**, setel **jenis disk OS** ke **HDD Standar** dan klik **Berikutnya: Jaringan >** .

5. Pada tab **Jaringan** dari panel **Buat mesin virtual**, pilih jaringan yang dibuat sebelumnya **myVirtualNetwork**.

6. Di bawah **Kelompok keamanan jaringan NIC** pilih **Tidak ada**.

7. Klik **Berikutnya: Manajemen >** , pada tab **Pengelolaan** panel **Buat mesin virtual**, verifikasi setelan berikut:

   |Pengaturan|Nilai|
   |---|---|
   |Diagnostik boot|**Diaktifkan dengan akun penyimpanan terkelola (disarankan)**|

8. Klik **Tinjau + buat**, pada panel **Tinjau + buat**, pastikan validasi berhasil dan klik **Buat**.

#### <a name="task-2-create-a-virtual-machine-to-use-as-a-management-server"></a>Tugas 2: Buat mesin virtual untuk digunakan sebagai server manajemen. 

Dalam tugas ini, Anda akan membuat mesin virtual untuk digunakan sebagai server manajemen.

1. Di portal Azure, navigasikan kembali ke panel **Mesin virtual**, klik **+ Buat**, dan, di daftar dropdown, klik **+ Mesin virtual Azure**.

2. Pada tab **Dasar** pada panel **Buat mesin virtual**, tentukan pengaturan berikut (biarkan yang lain dengan nilai defaultnya):

   |Pengaturan|Nilai|
   |---|---|
   |Langganan|nama langganan Azure yang akan Anda gunakan di lab ini|
   |Grup sumber daya|**AZ500LAB07**|
   |Nama komputer virtual|**myVMMgmt**|
   |Wilayah|(AS) AS Timur|
   |Gambar|**Pusat Data Windows Server 2022: Edisi Azure - Generasi 2**|
   |Ukuran|**Standar D2s v3**|
   |Nama Pengguna|**Siswa**|
   |Kata sandi|**Gunakan kata sandi pribadi Anda yang dibuat di Lab 04 > Latihan 1 > Tugas 1 > Langkah 9.**|
   |Port masuk publik|**Tidak ada**|
   |Sudah memiliki lisensi Windows Server|**Tidak**|

    >**Catatan**: Untuk port masuk publik, kami akan mengandalkan NSG yang telah dibuat sebelumnya. 

3. Klik **Berikutnya: Disk >** dan, pada tab **Disk** panel **Buat mesin virtual**, setel **jenis disk OS** ke **HDD Standar** dan klik **Berikutnya: Jaringan >** .

4. Pada tab **Jaringan** dari panel **Buat mesin virtual**, pilih jaringan yang dibuat sebelumnya **myVirtualNetwork**.

5. Di bawah **Kelompok keamanan jaringan NIC** pilih **Tidak ada**.

6. Klik **Berikutnya: Manajemen >** , pada tab **Manajemen** panel **Buat mesin virtual**, tentukan setelan berikut

   |Pengaturan|Nilai|
   |---|---|
   |Diagnostik boot|**Diaktifkan dengan akun penyimpanan terkelola (disarankan)**|

7. Klik **Tinjau + buat**, pada panel **Tinjau + buat**, pastikan validasi berhasil dan klik **Buat**.

    >**Catatan**: Tunggu kedua mesin virtual diprovisikan sebelum melanjutkan. 

#### <a name="task-3-associate-each-virtual-machines-network-interface-to-its-application-security-group"></a>Tugas 3: Kaitkan setiap antarmuka jaringan mesin virtual ke kelompok keamanan aplikasinya.

Dalam tugas ini, Anda akan mengaitkan setiap antarmuka jaringan mesin virtual dengan kelompok keamanan aplikasi yang sesuai. Antarmuka mesin virtual myVMWeb akan dikaitkan dengan myAsgWebServers ASG. Antarmuka mesin virtual myVMMgmt akan dikaitkan dengan ASG myAsgMgmtServers. 

1. Di portal Azure, navigasikan kembali ke panel **Mesin virtual** dan verifikasi bahwa kedua mesin virtual terdaftar dengan status **Berjalan**.

2. Dalam daftar mesin virtual, klik entri **myVMWeb**.

3. Pada panel **myVMWeb**, di bagian **Setelan**, klik **Jaringan** lalu, pada panel **myVMWeb \| Jaringan**, klik Tab **Kelompok keamanan aplikasi**.

4. Klik **Konfigurasikan kelompok keamanan aplikasi**, di daftar drop-down **Grup keamanan aplikasi**, pilih **myAsgWebServers**, lalu klik **Simpan**.

5. Navigasikan kembali ke panel **Mesin virtual** dan dalam daftar mesin virtual, klik entri **myVMMgmt**.

6. Pada panel **myVMMgmt**, di bagian **Setelan**, klik **Jaringan** lalu, pada panel **myVMMgmt \| Jaringan**, klik Tab **Kelompok keamanan aplikasi**.

7. Klik **Konfigurasikan kelompok keamanan aplikasi**, di daftar drop-down **Kelompok keamanan aplikasi**, pilih **myAsgMgmtServers**, lalu klik **Simpan**.

#### <a name="task-4-test-the-network-traffic-filtering"></a>Tugas 4: Uji pemfilteran lalu lintas jaringan

Dalam tugas ini, Anda akan menguji filter lalu lintas jaringan. Anda harus dapat RDP ke mesin virtual myVMMgmnt. Anda harus dapat terhubung dari internet ke mesin virtual myVMWeb dan melihat halaman web IIS default.  

1. Navigasi kembali ke panel mesin virtual **myVMMgmt**.

2. Pada panel **myVMMgmt**, klik **Hubungkan** dan, di menu dropdown, klik **RDP**. 

3. Klik **Unduh File RDP** dan gunakan untuk menyambung ke mesin virtual Azure **myVMMgmt** melalui Desktop Jauh. Saat diminta untuk mengautentikasi, berikan kredensial berikut:

   |Pengaturan|Nilai|
   |---|---|
   |Nama pengguna|**Siswa**|
   |Kata sandi|**Gunakan kata sandi pribadi Anda yang dibuat di Lab 04 > Latihan 1 > Tugas 1 > Langkah 9.**|

    >**Catatan**: Verifikasi bahwa koneksi Desktop Jauh berhasil. Pada titik ini Anda telah mengonfirmasi bahwa Anda dapat terhubung melalui Desktop Jauh ke myVMMgmt.

4. Di portal Azure, navigasikan ke panel mesin virtual **myVMWeb**.

5. Pada panel **myVMWeb**, di bagian **Operasi**, klik **Jalankan perintah**, lalu klik **RunPowerShellScript**.

6. Pada panel **Jalankan Skrip Perintah**, jalankan perintah berikut untuk memasang peran server Web di **myVmWeb**:

    ```powershell
    Install-WindowsFeature -name Web-Server -IncludeManagementTools
    ```

    >**Catatan**: Tunggu hingga penginstalan selesai. Ini mungkin memakan waktu beberapa menit. Pada saat itu, Anda dapat memverifikasi bahwa myVMWeb dapat diakses melalui HTTP/HTTPS.

7. Di portal Azure, navigasikan kembali ke panel **myVMWeb**.

8. Pada panel **myVMWeb**, identifikasi **alamat IP Publik** mesin virtual Azure myVmWeb.

9. Buka tab browser lain dan navigasikan ke alamat IP yang Anda identifikasi di langkah sebelumnya.

    >**Catatan**: Halaman browser harus menampilkan halaman selamat datang IIS default karena port 80 diizinkan masuk dari internet berdasarkan pengaturan kelompok keamanan aplikasi **myAsgWebServers**. Antarmuka jaringan mesin virtual Azure myVMWeb dikaitkan dengan kelompok keamanan aplikasi tersebut. 

> Hasil: Anda telah memvalidasi bahwa konfigurasi NSG dan ASG berfungsi dan lalu lintas dikelola dengan benar. 

**Membersihkan sumber daya**

> Jangan lupa untuk menghapus sumber daya Azure yang baru dibuat dan yang tidak diperlukan lagi. Menghapus sumber daya yang tidak terpakai memastikan Anda tidak akan dikenakan biaya tak terduga. 

1. Buka Cloud Shell dengan mengklik ikon pertama di kanan atas Portal Azure. Jika diminta, pilih **PowerShell** dan **Buat penyimpanan**.

2. Pastikan **PowerShell** dipilih di menu dropdown di sudut kiri atas panel Cloud Shell.

3. Di sesi PowerShell dalam panel Cloud Shell, jalankan perintah berikut untuk menghapus grup sumber daya yang Anda buat di lab ini:
  
    ```powershell
    Remove-AzResourceGroup -Name "AZ500LAB07" -Force -AsJob
    ```

4.  Tutup panel **Cloud Shell**. 
