---
lab:
  title: 15 - Microsoft Sentinel
  module: Module 04 - Manage Security Operations
ms.openlocfilehash: 63a24bbc17b846d3587cf3fb83ab46b7235d5fcd
ms.sourcegitcommit: 2f08105eaaf0413d3ec3c12a3b078678151fd211
ms.translationtype: HT
ms.contentlocale: id-ID
ms.lasthandoff: 04/04/2022
ms.locfileid: "145195957"
---
# <a name="lab-15-microsoft-sentinel"></a>Lab 15: Microsoft Sentinel
# <a name="student-lab-manual"></a>Panduan lab siswa

## <a name="lab-scenario"></a>Skenario lab

**Catatan:** **Azure Sentinel** berganti nama menjadi **Microsoft Sentinel** 

Anda telah diminta untuk membuat bukti konsep deteksi dan respons ancaman berbasis Microsoft Sentinel. Secara khusus, Anda ingin:

- Mulai mengumpulkan data dari Azure Activity dan Microsoft Defender untuk Cloud.
- Menambahkan peringatan bawaan dan kustom 
- Meninjau cara Playbook dapat digunakan untuk mengotomatiskan respons terhadap suatu insiden.

> Untuk semua sumber daya di lab ini, kita menggunakan wilayah **US Timur**. Pastikan pada instruktur Anda bahwa ini adalah wilayah yang akan digunakan untuk kelas. 

## <a name="lab-objectives"></a>Tujuan lab

Di lab ini, Anda akan menyelesaikan latihan berikut:

- Latihan 1: Menerapkan Microsoft Sentinel

## <a name="microsoft-sentinel-diagram"></a>Diagram Microsoft Sentinel

![gambar](https://user-images.githubusercontent.com/91347931/157538440-4953be73-90be-4edd-bd23-b678326ba637.png)

## <a name="instructions"></a>Instruksi

## <a name="lab-files"></a>File lab:

- **\\Allfiles\\Labs\\15\\changeincidentseverity.json**

### <a name="exercise-1-implement-microsoft-sentinel"></a>Latihan 1: Menerapkan Microsoft Sentinel

### <a name="estimated-timing-30-minutes"></a>Perkiraan waktu: 30 menit

Dalam latihan ini, Anda akan menyelesaikan tugas-tugas berikut:

- Tugas 1: Memasang Microsoft Sentinel
- Tugas 2: Menghubungkan Azure Activity ke Sentinel
- Tugas 3: Membuat aturan yang menggunakan konektor data Azure Activity. 
- Tugas 4: Membuat playbook
- Tugas 5: Membuat peringatan kustom dan mengonfigurasikan playbook sebagai respons otomatis.
- Tugas 6: Panggil insiden dan tinjau tindakan terkait.

#### <a name="task-1-on-board-azure-sentinel"></a>Tugas 1: Memasang Azure Sentinel

Dalam tugas ini, Anda akan mengaktifkan Microsoft Sentinel dan menghubungkan ruang kerja Log Analytics. 

1. Masuk ke portal Microsoft Azure **`https://portal.azure.com/`** .

    >**Catatan**: Masuk ke portal Microsoft Azure menggunakan akun yang memiliki peran Pemilik atau Kontributor dalam langganan Azure yang Anda gunakan untuk lab ini.

2. Di portal Microsoft Azure, dalam kotak teks **Cari sumber daya, layanan, dan dokumen** di bagian atas halaman portal Azure, ketik **Microsoft Sentinel** dan tekan tombol **Enter**.

    >**Catatan**: Jika ini adalah upaya pertama Anda untuk melakukan tindakan Microsoft Sentinel di dasbor Azure, selesaikan langkah-langkah berikut: Di portal Microsoft Azure, di kotak teks **Cari sumber daya, layanan, dan dokumen** di bagian atas halaman portal Azure, ketik **Microsoft Sentinel** dan tekan tombol **Enter**. Pilih **Microsoft Sentinel** dari tampilan **Layanan**.
  
3. Pada bilah **Microsoft Sentinel**, klik **+ Buat**.

4. Pada panel **Tambahkan Microsoft Sentinel ke ruang kerja**, pilih ruang kerja Log Analytics yang Anda buat di lab Azure Monitor dan klik **Tambahkan**.

    >**Catatan**: Microsoft Sentinel memiliki persyaratan yang sangat spesifik untuk ruang kerja. Misalnya, ruang kerja yang dibuat oleh Microsoft Defender untuk Cloud tidak dapat digunakan. Baca selengkapnya di [Quickstart: Memasang Azure Sentinel](https://docs.microsoft.com/en-us/azure/sentinel/quickstart-onboard)
    
#### <a name="task-2-configure-microsoft-sentinel-to-use-the-azure-activity-data-connector"></a>Tugas 2: Konfigurasikan Microsoft Sentinel untuk menggunakan konektor data Aktivitas Azure. 

Dalam tugas ini, Anda akan mengonfigurasi Sentinel untuk menggunakan konektor data Azure Activity.  

1. Di portal Azure, pada bilah **Microsoft Sentinel \| Ringkasan**, di bagian **Konfigurasi**, klik **Konektor data**. 

2. Pada panel **Microsoft Sentinel \| Konektor data**, tinjau daftar konektor yang tersedia, ketik **Azure** ke dalam panel pencarian dan pilih entri yang mewakili konektor **Azure Activity** (sembunyikan panel menu di sebelah kiri menggunakan \<< jika diperlukan), tinjau deskripsi dan statusnya, lalu klik **Buka halaman konektor**.

3. Pada bilah **Azure Activity**, tab **Petunjuk** harus dipilih, perhatikan **Prasyarat** dan gulir ke bawah ke **Konfigurasi**. Catat informasi yang menjelaskan pembaruan konektor. Langganan Azure Pass Anda tidak pernah menggunakan metode koneksi lama sehingga Anda dapat melewati langkah 1 (tombol **Putuskan Semua** akan berwarna abu-abu) dan lanjutkan ke langkah 2.

4. Pada langkah 2 **Hubungkan langganan Anda melalui saluran baru pengaturan diagnostik**, tinjau petunjuk "Luncurkan wizard Penetapan Kebijakan Azure dan ikuti langkah-langkahnya", lalu klik **Luncurkan wizard Penetapan Kebijakan Azure\>** .

5. Pada tab **Konfigurasikan log Azure Activity untuk melakukan streaming ke ruang kerja Log Analytics tertentu** (halaman Tetapkan Kebijakan) **Dasar**, klik tombol **Elipsis cakupan (...)** . Di halaman **Cakupan**, pilih langganan Azure Pass Anda dari daftar langganan dropdown dan klik tombol **Pilih** di bagian bawah halaman.

    >**Catatan**: *Jangan* memilih Grup Sumber Daya

6. Klik tombol **Berikutnya** di bagian bawah tab **Dasar** untuk melanjutkan ke tab **Parameter**. Pada tab **Parameters**, klik tombol **Elipsis ruang kerja Log Analytics Utama (...)** . Di halaman **ruang kerja Log Analytics Utama**, pastikan langganan Azure pass Anda dipilih dan gunakan dropdown **ruang kerja** untuk memilih ruang kerja Log Analytics yang Anda gunakan untuk Sentinel. Setelah selesai, klik tombol **Pilih** di bagian bawah halaman.

7. Klik tombol **Berikutnya** di bagian bawah tab **Parameter** untuk melanjutkan ke tab **Perbaikan**. Pada tab **Perbaikan** pilih kotak centang **Buat tugas perbaikan**. Tindakan ini akan mengaktifkan "Konfigurasikan log Azure Activity untuk melakukan streaming ke ruang kerja Log Analytics yang ditentukan" di dropdown **Kebijakan untuk memulihkan**. Di dropdown **Lokasi identitas yang ditetapkan sistem**, pilih wilayah (misalnya AS Timur) yang Anda pilih sebelumnya untuk ruang kerja Log Analytics Anda.

8. Klik tombol **Berikutnya** di bagian bawah tab **Perbaikan** untuk melanjutkan ke tab **Pesan ketidakpatuhan**.  Masukkan pesan Ketidakpatuhan jika Anda mau (opsi ini opsional) dan klik tombol **Tinjau + Buat** di bagian bawah tab **Pesan ketidakpatuhan**.

9. Klik tombol **Buat**. Anda akan mengamati tiga pesan status yang berhasil: **Pembuatan penetapan kebijakan berhasil, pembuatan Penetapan Peran berhasil, dan pembuatan tugas Perbaikan berhasil**.

    >**Catatan**: Anda dapat memeriksa Pemberitahuan, ikon lonceng untuk memverifikasi tiga tugas yang berhasil.

10. Pastikan panel **Azure Activity** menampilkan grafik **Data diterima** (Anda mungkin harus me-refresh halaman browser).  

    >**Catatan**: Mungkin perlu waktu lebih dari 15 menit sebelum Status menunjukkan "Tersambung" dan grafik menampilkan Data yang diterima.

#### <a name="task-3-create-a-rule-that-uses-the-azure-activity-data-connector"></a>Tugas 3: Membuat aturan yang menggunakan konektor data Azure Activity. 

Dalam tugas ini, Anda akan meninjau dan membuat aturan yang menggunakan konektor data Azure Activity. 

1. Pada bilah **Microsoft Sentinel \| Konfigurasi**, klik **Analytics**. 

2. Pada bilah **Microsoft Sentinel \| Analytics**, klik tab **Templat aturan**. 

    >**Catatan**: Tinjau jenis aturan yang dapat Anda buat. Setiap aturan dikaitkan dengan Sumber Data tertentu.

3. Dalam daftar templat aturan, ketikkan **Mencurigakan** ke dalam formulir bilah pencarian dan klik entri **Pembuatan atau penyebaran sumber daya yang mencurigakan** yang terkait dengan sumber data **Azure Activity** . Kemudian, di panel yang menampilkan properti templat aturan, klik **Buat aturan** (gulir ke kanan halaman jika perlu).

    >**Catatan**: Aturan ini memiliki tingkat keparahan sedang. 

4. Pada tab **Umum** pada bilah **Wizard aturan analitik - Buat aturan baru dari templat**, terima pengaturan default dan klik **Berikutnya: Atur logika aturan >** .

5. Pada tab **Atur logika aturan** pada bilah **Wizard aturan analitik - Buat aturan baru dari templat**, terima pengaturan default dan klik **Berikutnya: Pengaturan insiden (Pratinjau) >** .

6. Pada tab **Pengaturan insiden** pada bilah **Wizard aturan analitik - Buat aturan baru dari templat**, terima pengaturan default dan klik **Berikutnya: Respons otomatis >** . 

    >**Catatan**: Di sinilah Anda dapat menambahkan playbook, yang diimplementasikan sebagai Aplikasi Logika, ke aturan untuk mengotomatiskan perbaikan masalah.

7. Pada tab **Respons otomatis** pada bilah **Wizard aturan analitik - Buat aturan baru dari templat**, terima pengaturan default dan klik **Berikutnya: Tinjau >** . 

8. Pada tab **Tinjau dan buat** bilah **Wizard aturan analitik - Buat aturan baru dari templat**, klik **Buat**.

    >**Catatan**: Anda sekarang memiliki aturan aktif.

#### <a name="task-4-create-a-playbook"></a>Tugas 4: Membuat playbook

Dalam tugas ini, Anda akan membuat playbook. Playbook keamanan adalah kumpulan tugas yang dapat dipanggil oleh Microsoft Sentinel sebagai respons terhadap peringatan. 

1. Di portal Microsoft Azure, dalam kotak teks **Cari sumber daya, layanan, dan dokumen** di bagian atas halaman portal Microsoft Azure, ketik **Sebarkan template kustom** dan tekan tombol **Enter**.

2. Pada panel **Penyebaran kustom**, klik opsi **Buat template Anda sendiri di penyunting**.

3. Pada panel **Edit template**, klik **Muat file**, cari file **\\Allfiles\\Labs\\15\\changeincidentseverity.json** dan klik **Buka**.

    >**Catatan**: Anda dapat menemukan contoh playbooks di [https://github.com/Azure/Azure-Sentinel/tree/master/Playbooks](https://github.com/Azure/Azure-Sentinel/tree/master/Playbooks).

4. Pada panel **Edit template**, klik **Simpan**.

5. Pada panel **Penyebaran kustom**, pastikan bahwa pengaturan berikut telah dikonfigurasi (biarkan opsi yang lain dengan nilai defaultnya):

    |Pengaturan|Nilai|
    |---|---|
    |Langganan|nama langganan Azure yang Anda gunakan di lab ini|
    |Grup sumber daya|**AZ500LAB131415**|
    |Lokasi|**(AS) AS Timur**|
    |Nama Playbook|**Change-Incident-Severity**|
    |Nama Pengguna|Alamat email anda|

6. Klik **Review + create**, lalu klik **Create**.

    >**Catatan**: Tunggu hingga penyebaran selesai.

7. Di portal Microsoft Azure, di kotak teks **Cari sumber daya, layanan, dan dokumen** di bagian atas halaman portal Microsoft Azure, ketik **Grup sumber daya** dan tekan tombol **Enter**.

8. Pada bilah **Grup sumber daya**, dalam daftar grup sumber daya, klik entri **AZ500LAB131415**.

9. Pada bilah grup sumber daya **AZ500LAB131415**, dalam daftar sumber daya, klik entri yang mewakili aplikasi logika **Change-Incident-Severity** yang baru dibuat.

10. Pada bilah **Change-Incident-Severity**, klik **Edit**.

    >**Catatan**: Pada bilah **Desainer Logic Apps**, masing-masing dari empat koneksi menampilkan peringatan. Ini berarti bahwa masing-masing koneksi perlu ditinjau dan dikonfigurasi.

11. Pada bilah **Desainer Logic Apps**, klik langkah **Koneksi** pertama.

12. Klik **Tambahkan baru**, pastikan entri dalam daftar dropdown **Penyewa** berisi nama penyewa Azure AD Anda dan klik **Masuk**.

13. Saat diminta, masuk dengan akun pengguna yang memiliki peran Pemilik atau Kontributor di langganan Azure yang Anda gunakan untuk lab ini.

14. Klik langkah **Koneksi** kedua dan dalam daftar koneksi, pilih entri kedua yang mewakili koneksi yang Anda buat di langkah sebelumnya.

15. Ulangi langkah sebelumnya untuk dua langkah **Koneksi** yang tersisa.

    >**Catatan**: Pastikan tidak ada peringatan yang ditampilkan pada salah satu langkah.

16. Pada bilah **Desainer Logic Apps**, klik **Simpan** untuk menyimpan perubahan Anda.

#### <a name="task-5-create-a-custom-alert-and-configure-a-playbook-as-an-automated-response"></a>Tugas 5: Membuat peringatan kustom dan mengonfigurasikan playbook sebagai respons otomatis

1. Di portal Azure, navigasikan kembali ke bilah **Microsoft Sentinel \| Ringkasan**.

2. Pada bilah **Microsoft Sentinel \| Ringkasan**, di bagian **Konfigurasi**, klik **Analytics**.

3. Pada bilah **Microsoft Sentinel \| Analytics**, klik **+ Buat** dan, di menu dropdown, klik **Aturan kueri terjadwal**. 

4. Pada tab **Umum** pada bilah **Wizard aturan analitik - Buat aturan baru**, tentukan pengaturan berikut (biarkan opsi yang lain dengan nilai defaultnya):

    |Pengaturan|Nilai|
    |---|---|
    |Nama|**Demo Playbook**|
    |Taktik|**Akses Awal**|

5. Klik **Berikutnya: Atur logika aturan >** .

6. Pada tab **Atur logika aturan** dari bilah **Wizard aturan analitik - Buat aturan baru**, di kotak teks **Kueri aturan**, tempel kueri aturan berikut. 

    ```
    AzureActivity
     | where ResourceProviderValue =~ "Microsoft.Security" 
     | where OperationNameValue =~ "Microsoft.Security/locations/jitNetworkAccessPolicies/delete" 
    ```

    >**Catatan**: Aturan ini mengidentifikasi penghapusan kebijakan akses VM Just in time.

    >**Catatan** jika Anda menerima kesalahan penguraian, intellisense mungkin telah menambahkan nilai ke kueri Anda. Pastikan kueri cocok jika tidak, tempel kueri ke notepad lalu dari notepad ke kueri aturan. 


7. Pada tab **Atur logika aturan** dari bilah **Wizard aturan analitik - Buat aturan baru**, di bagian **Penjadwalan kueri**, atur **Jalankan kueri setiap** hingga **5 Menit**.

8. Pada tab **Atur logika aturan** pada bilah **Wizard aturan analitik - Buat aturan baru**, terima nilai default dari pengaturan yang tersisa dan klik **Berikutnya: Pengaturan insiden >** .

9. Pada tab **Pengaturan insiden** pada bilah **Wizard aturan analitik - Buat aturan baru**, terima pengaturan default dan klik **Berikutnya: Respons otomatis >** . 

10. Pada tab **Respons otomatis** pada panel **Wizard aturan analitik - Buat aturan baru**, dalam daftar dropdown **Otomatisasi peringatan**, centang kotak di samping entri **Change-Incident-Severity** dan klik **Berikutnya: Tinjau >** . 

11. Pada tab **Tinjau dan buat** bilah **Wizard aturan analitik - Buat aturan baru**, klik **Buat**.

    >**Catatan**: Anda sekarang memiliki aturan aktif baru yang disebut **Demo Playbook**. Jika suatu peristiwa yang diidentifikasi oleh logika rue terjadi, logika itu akan menghasilkan peringatan tingkat keparahan sedang, yang akan menghasilkan insiden yang sesuai.

#### <a name="task-6-invoke-an-incident-and-review-the-associated-actions"></a>Tugas 6: Panggil insiden dan tinjau tindakan terkait.

1. Di portal Azure, navigasikan ke bilah **Microsoft Defender untuk Cloud \|Ringkasan**.

    >**Catatan**: Periksa skor aman Anda. Sekarang nilainya pasti sudah diperbarui. 

2. Pada bilah **Microsoft Defender untuk Cloud \| Perlindungan beban kerja**, klik **Akses VM Just-in-time** pada **Perlindungan lanjutan**.

3. Pada bilah **Microsoft Defender untuk Cloud \| Akses VM just in time**, di sisi kanan baris yang merujuk ke komputer virtual **myVM**, klik **elipsis**, klik **Hapus**, lalu klik **Ya**.

    >**Catatan:** Jika VM tidak tercantum dalam **VM Just-in-time**, navigasikan ke bilah **Komputer Virtual** dan klik **Konfigurasi**, Klik opsi **Aktikan VM Just-in-time** pada **Akses VM Just-in-time**. Ulangi langkah di atas untuk menavigasi kembali ke **Microsoft Defender untuk Cloud** dan menyegarkan halaman, VM akan muncul.

4. Di portal Microsoft Azure, di kotak teks **Cari sumber daya, layanan, dan dokumen** di bagian atas halaman portal Azure, ketik **Log aktivitas** dan tekan tombol **Enter**.

5. Navigasikan ke bilah **Log aktivitas**, catat entri **Hapus Kebijakan Akses Jaringan JIT**. 

    >**Catatan**: Operasi ini mungkin membutuhkan satu menit untuk muncul. 

6. Di portal Azure, navigasikan kembali ke bilah **Microsoft Sentinel \| Ringkasan**.

7. Pada bilah **Microsoft Sentinel \| Ringkasan**, tinjau dan pastikan dasbor menampilkan peringatan yang sesuai dengan penghapusan kebijakan akses VM Tepat Waktu.

    >**Catatan**: Perlu waktu hingga 5 menit agar peringatan muncul di bilah **Microsoft Sentinel \| Ringkasan**. Jika Anda tidak melihat peringatan pada saat tersebut, jalankan aturan kueri yang direferensikan dalam tugas sebelumnya untuk memastikan bahwa aktivitas penghapusan kebijakan akses Just In Time telah disebarkan ke ruang kerja Log Analytics yang terkait dengan instans Microsoft Sentinel Anda. Jika bukan itu masalahnya, buat kembali kebijakan akses VM Just in time dan hapus lagi.

8. Pada bilah **Microsoft Sentinel \| Ringkasan**, di bagian **Pengelolaan Ancaman**, klik **Insiden**.

9. Pastikan bilah menampilkan insiden dengan tingkat keparahan sedang atau tinggi.

    >**Catatan**: Perlu waktu hingga 5 menit agar insiden muncul di bilah **Microsoft Sentinel \| Insiden**. 

    >**Catatan**: Tinjau bilah **Microsoft Sentinel \| Playbooks**. Anda akan menemukan hitungan berhasil dan gagal berjalan di bilah tersebut.

    >**Catatan**: Anda memiliki opsi untuk menetapkan tingkat keparahan dan status yang berbeda untuk sebuah insiden.

> Hasil: Anda telah membuat ruang kerja Microsoft Sentinel, menghubungkannya ke log Aktivitas Azure, membuat playbook dan peringatan kustom yang dipicu sebagai respons terhadap penghapusan kebijakan akses VM Just In Time, dan memverifikasi bahwa konfigurasi tersebut valid.

**Membersihkan sumber daya**

> Jangan lupa untuk menghapus sumber daya Azure yang baru dibuat dan yang tidak diperlukan lagi. Menghapus sumber daya yang tidak terpakai memastikan Anda tidak akan dikenakan biaya tidak terduga. 

1. Di portal Microsoft Azure, buka Cloud Shell dengan mengklik ikon pertama di kanan atas Portal Microsoft Azure. Jika diminta, klik **PowerShell** dan **Buat penyimpanan**.

2. Pastikan **PowerShell** dipilih di menu drop-down di sudut kiri atas panel Cloud Shell.

3. Di sesi PowerShell dalam panel Cloud Shell, jalankan perintah berikut untuk menghapus grup sumber daya yang Anda buat di lab ini:
  
    ```powershell
    Remove-AzResourceGroup -Name "AZ500LAB131415" -Force -AsJob
    ```
4. Tutup panel **Cloud Shell**.
