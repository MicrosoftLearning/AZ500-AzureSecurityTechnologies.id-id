---
lab:
  title: 13 - Azure Monitor
  module: Module 04 - Manage security operations
ms.openlocfilehash: e51e88d55193532e3c91c485d0a247b5e686a48f
ms.sourcegitcommit: 7c5e8e9a86553c6bd9b9a6651b60c6cb9676f0ff
ms.translationtype: HT
ms.contentlocale: id-ID
ms.lasthandoff: 07/18/2022
ms.locfileid: "147168498"
---
# <a name="lab-13-azure-monitor"></a>Lab 13: Azure Monitor
# <a name="student-lab-manual"></a>Panduan lab siswa

## <a name="lab-scenario"></a>Skenario lab

Anda telah diminta untuk membuat bukti konsep pemantauan performa mesin virtual. Secara khusus, Anda ingin:

- Mengonfigurasi mesin virtual agar telemetri dan log dapat dikumpulkan.
- Perlihatkan telemetri dan log apa yang dapat dikumpulkan.
- Perlihatkan bagaimana data dapat digunakan dan dikueri. 

> Untuk semua sumber daya di lab ini, kita menggunakan wilayah **US Timur**. Verifikasi dengan instruktur Anda bahwa ini adalah wilayah yang akan digunakan untuk kelas. 

## <a name="lab-objectives"></a>Tujuan lab

Di lab ini, Anda akan menyelesaikan latihan berikut:

- Latihan 1: Mengumpulkan data dari mesin virtual Azure dengan Azure Monitor

## <a name="azure-monitor"></a>Azure Monitor

![gambar](https://user-images.githubusercontent.com/91347931/157536648-0a286514-a7e2-4058-9dea-e42da21eef76.png)

## <a name="instructions"></a>Instruksi

### <a name="exercise-1-collect-data-from-an-azure-virtual-machine-with-azure-monitor"></a>Latihan 1: Mengumpulkan data dari mesin virtual Azure dengan Azure Monitor

### <a name="exercise-timing-20-minutes"></a>Waktu latihan: 20 menit

Dalam latihan ini, Anda akan menyelesaikan tugas-tugas berikut: 

- Tugas 1: Menyebarkan mesin virtual Azure 
- Tugas 2: Membuat ruang kerja Analitik Log
- Tugas 3: Mengaktifkan ekstensi mesin virtual Analitik Log
- Tugas 4: Mengumpulkan peristiwa mesin virtual dan data performa
- Tugas 5: Menampilkan dan mengkueri data yang dikumpulkan 

#### <a name="task-1-deploy-an-azure-virtual-machine"></a>Tugas 1: Menyebarkan mesin virtual Azure

1. Masuk ke portal Microsoft Azure **`https://portal.azure.com/`** .

    >**Catatan**: Masuk ke portal Microsoft Azure menggunakan akun yang memiliki peran Pemilik atau Kontributor dalam langganan Azure yang Anda gunakan untuk lab ini.

2. Buka Cloud Shell dengan mengklik ikon pertama di kanan atas Portal Azure. Jika diminta, pilih **PowerShell** dan **Buat penyimpanan**.

3. Pastikan **PowerShell** dipilih di menu dropdown di sudut kiri atas panel Cloud Shell.

4. Di sesi PowerShell dalam panel Cloud Shell, jalankan perintah berikut untuk membuat grup sumber daya yang akan digunakan di lab ini:
  
    ```powershell
    New-AzResourceGroup -Name AZ500LAB131415 -Location 'EastUS'
    ```

    >**Catatan**: Grup sumber daya ini akan digunakan untuk lab 13, 14, dan 15. 

5. Di sesi PowerShell dalam panel Cloud Shell, jalankan perintah berikut untuk membuat mesin virtual Azure baru. 

    >**Perhatian**: Perintah New-AzVm tidak berfungsi di Azure CLI versi 4.24 dan Microsoft sedang menyelidiki resolusi.  Solusi di lab ini adalah menginstal dan mengembalikan ke Az.Compute versi 4.23.0, yang tidak terpengaruh oleh masalah ini.
   
    >**Petunjuk**: Mengembalikan ke Az.Compute versi 4.23.0 
  
   #### <a name="step-1-download-the-working-version-of-the-module-4230-into-your-cloud-shell-session"></a>Langkah 1: Unduh versi modul yang berfungsi (4.23.0) ke dalam sesi cloud shell Anda 
   **Jenis**: Install-Module -Name Az.Compute -Force -RequiredVersion 4.23.0

   #### <a name="step-2-start-a-new-powershell-session-that-will-allow-the-azcompute-assembly-version-to-be-loaded"></a>Langkah 2: Mulai sesi PowerShell baru yang memungkinkan versi rakitan Az.Compute dimuat 
   **Jenis**: pwsh

   #### <a name="step-3-verify-that-version-4230-is-loaded"></a>Langkah 3: Verifikasi bahwa versi 4.23.0 dimuat
   **Jenis**: Get-Module -Name Az.Compute
   
    ```powershell
    New-AzVm -ResourceGroupName "AZ500LAB131415" -Name "myVM" -Location 'EastUS' -VirtualNetworkName "myVnet" -SubnetName "mySubnet" -SecurityGroupName   "myNetworkSecurityGroup" -PublicIpAddressName "myPublicIpAddress" -OpenPorts 80,3389
    ```

6.  Saat dimintai informasi masuk:

    |Pengaturan|Nilai|
    |---|---|
    |Pengguna |**localadmin**|
    |Kata sandi|**Gunakan kata sandi pribadi Anda yang dibuat di Lab 04 > Latihan 1 > Tugas 1 > Langkah 9.**|

    >**Catatan**: Tunggu hingga penyebaran selesai. 

7. Di sesi PowerShell dalam panel Cloud Shell, jalankan perintah berikut untuk mengonfirmasi bahwa mesin virtual bernama **myVM** telah dibuat dan **ProvisioningState**-nya **Berhasil**.

    ```powershell
    Get-AzVM -Name 'myVM' -ResourceGroupName 'AZ500LAB131415' | Format-Table
    ```

8. Tutup panel Cloud Shell. 

#### <a name="task-2-create-a-log-analytics-workspace"></a>Tugas 2: Membuat ruang kerja Analitik Log

Dalam tugas ini, Anda akan membuat ruang kerja Analitik Log. 

1. Di portal Microsoft Azure, dalam kotak teks **Cari sumber daya, layanan, dan dokumen** di bagian atas halaman portal Azure, ketik **Log Analytics workspaces** dan tekan kunci **Enter**.

2. Pada panel **ruang kerja Analitik Log**, klik **+ Buat**.

3. Pada tab **Dasar-dasar** pada panel **Buat ruang kerja Analytics Log**, tentukan pengaturan berikut (biarkan yang lain dengan nilai defaultnya):

    |Pengaturan|Nilai|
    |---|---|
    |Langganan|nama langganan Azure yang Anda gunakan di lab ini|
    |Grup sumber daya|**AZ500LAB131415**|
    |Nama|nama yang valid dan unik secara global|
    |Wilayah|**(AS) AS Timur**|

4. Pilih **Tinjau + buat**.

5. Pada tab **Tinjau + buat** panel **Buat ruang kerja Analytics Log**, pilih **Buat**.

#### <a name="task-3-enable-the-log-analytics-virtual-machine-extension"></a>Tugas 3: Mengaktifkan ekstensi mesin virtual Analitik Log

Dalam tugas ini, Anda akan mengaktifkan ekstensi mesin virtual Analitik Log. Ekstensi ini menginstal agen Analitik Log di mesin virtual Windows dan Linux. Agen ini mengumpulkan data dari mesin virtual dan mentransfernya ke ruang kerja Analitik Log yang Anda tentukan. Setelah agen diinstal, agen akan ditingkatkan secara otomatis memastikan Anda selalu memiliki fitur dan perbaikan terbaru. 

1. Di portal Microsoft Azure, navigasikan kembali ke panel **ruang kerja Analitik Log**, dan, dalam daftar ruang kerja, klik entri yang mewakili ruang kerja yang Anda buat di tugas sebelumnya.

2. Pada panel ruang kerja Analitik Log, pada halaman **Gambaran Umum**, di bagian **Koneksi Sumber Data**, klik entri **Mesin virtual (VM) Azure**.

    >**Catatan**: Agar agen berhasil diinstal, mesin virtual harus berjalan.

3. Dalam daftar mesin virtual, temukan entri yang mewakili mesin virtual Azure **myVM** yang Anda sebarkan dalam tugas pertama latihan ini dan perhatikan bahwa entri tersebut terdaftar sebagai **Tidak tersambung**.

4. Klik entri **myVM** lalu, pada panel **myVM**, klik **Koneksi**. 

5. Tunggu hingga mesin virtual tersambung ke ruang kerja Analitik Log.

    >**Catatan**: Ini mungkin membutuhkan waktu beberapa menit. **Status** yang ditampilkan pada panel **myVM**, akan berubah dari **Menyambungkan** ke **Ruang kerja ini**. 

#### <a name="task-4-collect-virtual-machine-event-and-performance-data"></a>Tugas 4: Mengumpulkan peristiwa mesin virtual dan data performa

Dalam tugas ini, Anda akan mengonfigurasi pengumpulan log sistem Windows dan beberapa penghitung kinerja umum. Anda juga akan meninjau sumber lain yang tersedia.

1. Di portal Microsoft Azure, navigasikan kembali ke ruang kerja Analitik Log yang Anda buat sebelumnya dalam latihan ini.

2. Pada panel ruang kerja Analitik Log, di bagian **Pengaturan**, klik **Konfigurasi agen**.

3. Pada panel **konfigurasi Agen**, tinjau pengaturan yang dapat dikonfigurasi termasuk Windows Log Peristiwa, Windows Penghitung Kinerja, Penghitung Kinerja Linux, Log IIS, dan Syslog. 

4. Pastikan **bahwa Windows Log Peristiwa** dipilih, klik **+ Tambahkan log peristiwa windows**, dalam daftar jenis log peristiwa, pilih **Sistem**.

    >**Catatan**: Ini adalah bagaimana Anda menambahkan log peristiwa ke ruang kerja. Pilihan lain termasuk, misalnya, **Peristiwa perangkat keras** atau **Layanan Manajemen Kunci**.  

5. Batalkan pilihan kotak centang **Informasi**, biarkan kotak centang **Kesalahan** dan **Peringatan** dipilih.

6. Klik **Windows Penghitung Kinerja**, klik **+ Tambahkan penghitung kinerja**, tinjau daftar penghitung kinerja yang tersedia, dan tambahkan penghitung kinerja berikut:

    - Memori(\*)\Mbyte Memori yang Tersedia
    - Proses(\*)\\% Waktu Prosesor
    - Pelacakan Peristiwa untuk Windows\Total Penggunaan Memori --- Kumpulan Non-Halaman
    - Pelacakan Peristiwa untuk Windows\Total Penggunaan Memori --- Paged Pool

    >**Catatan**: Penghitung ditambahkan dan dikonfigurasi dengan interval sampel pengumpulan 60 detik.
  
7. Pada panel **Konfigurasi agen**, klik **Terapkan**.

#### <a name="task-5-view-and-query-collected-data"></a>Tugas 5: Menampilkan dan mengkueri data yang dikumpulkan

Dalam tugas ini, Anda akan menjalankan pencarian log pada pengumpulan data Anda. 

1. Di portal Microsoft Azure, navigasikan kembali ke ruang kerja Analitik Log yang Anda buat sebelumnya dalam latihan ini.

2. Pada panel ruang kerja Analitik Log, di bagian **Umum**, klik **Log**.

3. Jika diperlukan, tutup jendela **Selamat Datang di Analitik Log**. 

4. Pada panel **Kueri**, di kolom **Semua Kueri**, gulir ke bawah ke bagian bawah daftar jenis sumber daya, dan klik **Mesin virtual**
    
5. Tinjau daftar kueri yang telah ditentukan sebelumnya, pilih **Penggunaan memori dan CPU**, dan klik tombol **Jalankan** yang sesuai.

    >**Catatan**: Anda dapat memulai dengan kueri **Mesin virtual yang tersedia memori**. Jika Anda tidak mendapatkan hasil apa pun, periksa cakupannya diatur ke mesin virtual

6. Kueri akan terbuka secara otomatis di tab kueri baru. 

    >**Catatan**: Analitik Log menggunakan bahasa kueri Kusto. Anda dapat mengkustomisasi kueri yang sudah ada atau membuat kueri Anda sendiri. 

    >**Catatan**: Hasil kueri yang Anda pilih secara otomatis ditampilkan di bawah panel kueri. Untuk menjalankan ulang kueri, klik **Jalankan**.

    >**Catatan**: Karena mesin virtual ini baru saja dibuat, mungkin belum ada data apa pun. 

    >**Catatan**: Anda memiliki pilihan untuk menampilkan data dalam format yang berbeda. Anda juga memiliki opsi untuk membuat aturan peringatan berdasarkan hasil kueri.

    >**Catatan**: Anda dapat menghasilkan beberapa beban tambahan pada virtual mesin Azure yang Anda terapkan sebelumnya di lab ini dengan menggunakan langkah-langkah berikut:

    1. Navigasikan ke panel mesin virtual Azure.
    2. Pada panel mesin virtual Azure, di bagian **Operasi**, pilih **Jalankan perintah**, pada panel **RunPowerShellScript**, ketik skrip berikut, dan klik **Jalankan**:
    3. 
       ```cmd
       cmd
       :loop
       dir c:\ /s > SWAP
       goto loop
       ```
       
    4. Beralih kembali ke panel Analitik Log dan jalankan kembali kueri. Anda mungkin perlu menunggu beberapa menit agar data dikumpulkan dan menjalankan kembali kueri lagi.

> Hasil: Anda menggunakan ruang kerja Analitik Log untuk mengonfigurasi sumber data dan log kueri. 

**Membersihkan sumber daya**

>**Catatan**: Jangan hapus sumber daya dari lab ini karena diperlukan untuk lab Azure Security Center dan lab Azure Sentinel.
 
