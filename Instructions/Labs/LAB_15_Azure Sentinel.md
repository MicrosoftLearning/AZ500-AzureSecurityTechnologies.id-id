---
lab:
  title: 15 - Microsoft Sentinel
  module: Module 04 - Manage Security Operations
ms.openlocfilehash: 63a24bbc17b846d3587cf3fb83ab46b7235d5fcd
ms.sourcegitcommit: 18aa464705a8f1e78ccbbb82d5d8f57a830b6cea
ms.translationtype: HT
ms.contentlocale: id-ID
ms.lasthandoff: 03/16/2022
ms.locfileid: "145195943"
---
# <a name="lab-15-microsoft-sentinel"></a>Lab 15: Microsoft Sentinel
# <a name="student-lab-manual"></a>Panduan lab siswa

## <a name="lab-scenario"></a>Skenario lab

**Catatan:** **Azure Sentinel** diubah namanya menjadi **Microsoft Sentinel** 

Anda telah diminta untuk membuat bukti konsep deteksi dan respons ancaman berbasis Microsoft Sentinel. Secara khusus, Anda ingin:

- Mulai kumpulkan data dari Azure Activity dan Microsoft Defender untuk Cloud.
- Tambahkan peringatan bawaan dan khusus 
- Tinjau bagaimana Playbook dapat digunakan untuk mengotomatiskan respons terhadap suatu insiden.

> Untuk semua sumber daya di lab ini, kita menggunakan wilayah **US Timur**. Verifikasi dengan instruktur Anda bahwa ini adalah wilayah yang akan digunakan untuk kelas. 

## <a name="lab-objectives"></a>Tujuan lab

Di lab ini, Anda akan menyelesaikan latihan berikut:

- Latihan 1: Menerapkan Microsoft Sentinel

## <a name="microsoft-sentinel-diagram"></a>Diagram Microsoft Sentinel

![gambar](https://user-images.githubusercontent.com/91347931/157538440-4953be73-90be-4edd-bd23-b678326ba637.png)

## <a name="instructions"></a>Instruksi

## <a name="lab-files"></a>File lab:

- **\\Semuaberkas\\Labs\\15\\changeincidentseverity.json**

### <a name="exercise-1-implement-microsoft-sentinel"></a>Latihan 1: Menerapkan Microsoft Sentinel

### <a name="estimated-timing-30-minutes"></a>Perkiraan waktu: 30 menit

Dalam latihan ini, Anda akan menyelesaikan tugas-tugas berikut:

- Tugas 1: Microsoft Sentinel terpasang
- Tugas 2: Hubungkan Aktivitas Azure ke Sentinel
- Tugas 3: Buat aturan yang menggunakan konektor data Aktivitas Azure. 
- Tugas 4: Buat buku pedoman
- Tugas 5: Buat peringatan khusus dan konfigurasikan buku pedoman sebagai respons otomatis.
- Tugas 6: Panggil insiden dan tinjau tindakan terkait.

#### <a name="task-1-on-board-azure-sentinel"></a>Tugas 1: Azure Sentinel terpasang

Dalam tugas ini, Anda akan mengaktifkan Microsoft Sentinel dan menghubungkan ruang kerja Log Analytics. 

1. Masuk ke portal Microsoft Azure **`https://portal.azure.com/`** .

    >**Catatan**: Masuk ke portal Microsoft Azure menggunakan akun yang memiliki peran Pemilik atau Kontributor dalam langganan Azure yang Anda gunakan untuk lab ini.

2. Di portal Microsoft Azure, dalam kotak teks **Sumber daya pencarian, layanan, dokumen** di bagian atas halaman portal Microsoft Azure, ketik **Microsoft Sentinel** dan tekan kunci **Enter**.

    >**Catatan**: Jika ini adalah upaya pertama Anda untuk melakukan tindakan Microsoft Sentinel di dasbor Azure, selesaikan langkah-langkah berikut: Di portal Microsoft Azure, di kotak teks **Sumber daya pencarian, layanan, dan dokumen** di bagian atas halaman portal Microsoft Azure, ketik **Microsoft Sentinel** dan tekan kunci **Enter**. Pilih **Microsoft Sentinel** dari tampilan **Layanan**.
  
3. Pada bilah **Microsoft Sentinel**, klik **+ Buat**.

4. Pada bilah **Tambahkan Microsoft Sentinel ke ruang kerja**, pilih ruang kerja Analisis Log yang Anda buat di lab Azure Monitor dan klik **Tambahkan**.

    >**Catatan**: Microsoft Sentinel memiliki persyaratan yang sangat spesifik untuk ruang kerja. Misalnya, ruang kerja yang dibuat oleh Microsoft Defender untuk Cloud tidak dapat digunakan. Baca selengkapnya di [Mulai Cepat: Azure Sentinel terpasang](https://docs.microsoft.com/en-us/azure/sentinel/quickstart-onboard)
    
#### <a name="task-2-configure-microsoft-sentinel-to-use-the-azure-activity-data-connector"></a>Tugas 2: Konfigurasikan Microsoft Sentinel untuk menggunakan konektor data Aktivitas Azure. 

Dalam tugas ini, Anda akan mengonfigurasi Sentinel untuk menggunakan konektor data Azure Activity.  

1. Di portal Microsoft Azure, pada bilah **Gambaran Umum Microsoft Sentinel \|** , di bagian **Konfigurasi**, klik **Konektor data**. 

2. Pada bilah **Konektor Data Microsoft Sentinel \|** , tinjau daftar konektor yang tersedia, ketik **Azure** ke dalam bilah penelusuran dan pilih entri yang mewakili **Azure Activity** konektor (sembunyikan bilah menu di sebelah kiri menggunakan \<< jika diperlukan), tinjau deskripsi dan statusnya, lalu klik **Buka halaman konektor**.

3. Pada bilah **Aktivitas Azure**, tab **Petunjuk** harus dipilih, perhatikan **Prasyarat** dan gulir ke bawah ke **Konfigurasi**. Catat informasi yang menjelaskan pembaruan konektor. Langganan Azure Pass Anda tidak pernah menggunakan metode koneksi lama sehingga Anda dapat melewati langkah 1 (tombol **Putuskan Semua** akan berwarna abu-abu) dan lanjutkan ke langkah 2.

4. Pada langkah 2 **Hubungkan langganan Anda melalui saluran baru pengaturan diagnostik**, tinjau petunjuk "Luncurkan wizard Penetapan Kebijakan Azure dan ikuti langkah-langkahnya", lalu klik **Luncurkan wizard Penetapan Kebijakan Azure\>** .

5. Pada tab **Konfigurasikan log Aktivitas Azure untuk melakukan streaming ke ruang kerja Analisis Log tertentu** (Tetapkan halaman Kebijakan) **Dasar-dasar**, klik tombol **Cakupan elipsis (...)** . Di halaman **Cakupan**, pilih langganan Azure Pass Anda dari daftar tarik-turun langganan dan klik tombol **Pilih** di bagian bawah halaman.

    >**Catatan**: *Jangan* memilih Grup Sumber Daya

6. Klik tombol **Berikutnya** di bagian bawah tab **Dasar-dasar** untuk melanjutkan ke tab **Parameter**. Pada tab **Parameters**, klik tombol **Elipsis ruang kerja Analitik Log Utama (...)** . Di halaman **ruang kerja Analisis Log Utama**, pastikan langganan Azure pass Anda dipilih dan gunakan tarik-turun **ruang kerja** untuk memilih ruang kerja Analisis Log yang Anda gunakan untuk Sentinel. Setelah selesai, klik tombol **Pilih** di bagian bawah halaman.

7. Klik tombol **Berikutnya** di bagian bawah tab **Parameter** untuk melanjutkan ke tab **Remediasi**. Pada tab **Remediasi** pilih kotak centang **Buat tugas perbaikan**. Ini akan mengaktifkan "Konfigurasikan log Aktivitas Azure untuk melakukan streaming ke ruang kerja Analisis Log yang ditentukan" di tarik-turun **Kebijakan untuk memulihkan**. Di tarik-turun **Lokasi identitas yang ditetapkan sistem**, pilih wilayah (misalnya AS Timur) yang Anda pilih sebelumnya untuk ruang kerja Analisis Log Anda.

8. Klik tombol **Berikutnya** di bagian bawah tab **Remediasi** untuk melanjutkan ke tab **Pesan ketidakpatuhan**.  Masukkan pesan Ketidakpatuhan jika Anda mau (ini opsional) dan klik tombol **Tinjau + Buat** di bagian bawah tab **Pesan ketidakpatuhan**.

9. Klik tombol **Buat**. Anda harus mengamati tiga pesan status yang berhasil: **Pembuatan penetapan kebijakan berhasil, pembuatan Penetapan Peran berhasil, dan pembuatan tugas Remediasi berhasil**.

    >**Catatan**: Anda dapat memeriksa Notifikasi, ikon lonceng untuk memverifikasi tiga tugas yang berhasil.

10. Verifikasi bahwa panel **Aktivitas Azure** menampilkan grafik **Data diterima** (Anda mungkin harus menyegarkan halaman browser).  

    >**Catatan**: Mungkin diperlukan waktu lebih dari 15 menit sebelum Status menunjukkan "Tersambung" dan grafik menampilkan Data yang diterima.

#### <a name="task-3-create-a-rule-that-uses-the-azure-activity-data-connector"></a>Tugas 3: Buat aturan yang menggunakan konektor data Aktivitas Azure. 

Dalam tugas ini, Anda akan meninjau dan membuat aturan yang menggunakan konektor data Aktivitas Azure. 

1. Pada bilah **Konfigurasi Microsoft Sentinel \|** , klik **Analitik**. 

2. Pada bilah **Microsoft Sentinel \| Analytics**, klik tab **Templat aturan**. 

    >**Catatan**: Tinjau jenis aturan yang dapat Anda buat. Setiap aturan dikaitkan dengan Sumber Data tertentu.

3. Dalam daftar templat aturan, ketik **Mencurigakan** ke dalam formulir bilah penelusuran dan klik entri **Pembuatan atau penyebaran sumber daya yang mencurigakan** yang terkait dengan sumber data **Azure Activity**. Kemudian, di panel yang menampilkan properti templat aturan, klik **Buat aturan** (gulir ke kanan halaman jika perlu).

    >**Catatan**: Aturan ini memiliki tingkat keparahan sedang. 

4. Pada tab **Umum** pada bilah **Wizard aturan analitik - Buat aturan baru dari templat**, terima pengaturan default dan klik **Berikutnya: Atur logika aturan >** .

5. Pada tab **Atur logika aturan** pada bilah **Wizard aturan analitik - Buat aturan baru dari templat**, terima pengaturan default dan klik **Berikutnya: Pengaturan insiden (Pratinjau) >** .

6. Pada tab **Pengaturan insiden** pada bilah **Wizard aturan analitik - Buat aturan baru dari templat**, terima pengaturan default dan klik **Berikutnya: Respons otomatis >** . 

    >**Catatan**: Di sinilah Anda dapat menambahkan buku pedoman, yang diimplementasikan sebagai Aplikasi Logika, ke aturan untuk mengotomatiskan remediasi masalah.

7. Pada tab **Respons otomatis** pada bilah **Wizard aturan analitik - Buat aturan baru dari templat**, terima pengaturan default dan klik **Berikutnya: Tinjau >** . 

8. Pada tab **Tinjau dan buat** bilah **Wizard aturan analitik - Buat aturan baru dari templat**, klik **Buat**.

    >**Catatan**: Anda sekarang memiliki aturan aktif.

#### <a name="task-4-create-a-playbook"></a>Tugas 4: Buat buku pedoman

Dalam tugas ini, Anda akan membuat buku pedoman. Buku pedoman keamanan adalah kumpulan tugas yang dapat dipanggil oleh Microsoft Sentinel sebagai respons terhadap peringatan. 

1. Di portal Microsoft Azure, dalam kotak teks **Cari sumber daya, layanan, dan dokumen** di bagian atas halaman portal Microsoft Azure, ketik **Sebarkan template kustom** dan tekan tombol **Enter**.

2. Pada panel **Penyebaran kustom**, klik opsi **Buat templat Anda sendiri di editor**.

3. Pada bilah **Edit templat**, klik **Muat file**, cari file **\\Allfiles\\Labs\\15\\changeincidentseverity.json** dan klik **Buka**.

    >**Catatan**: Anda dapat menemukan contoh buku pedoman di [https://github.com/Azure/Azure-Sentinel/tree/master/Playbooks](https://github.com/Azure/Azure-Sentinel/tree/master/Playbooks).

4. Pada panel **Edit templat**, klik **Simpan**.

5. Pada panel **Penyebaran kustom**, pastikan bahwa pengaturan berikut telah dikonfigurasi (biarkan yang lain dengan nilai defaultnya):

    |Pengaturan|Nilai|
    |---|---|
    |Langganan|nama langganan Azure yang Anda gunakan di lab ini|
    |Grup sumber daya|**AZ500LAB131415**|
    |Lokasi|**(AS) AS Timur**|
    |Nama Playbook|**Ubah-Insiden-Keparahan**|
    |Nama Pengguna|alamat email anda|

6. Klik **Review + create**, lalu klik **Create**.

    >**Catatan**: Tunggu hingga penyebaran selesai.

7. Di portal Microsoft Azure, di kotak teks **Sumber daya pencarian, layanan, dan dokumen** di bagian atas halaman portal Microsoft Azure, ketik **Grup sumber daya** dan tekan kunci **Enter**.

8. Pada bilah **Grup sumber daya**, dalam daftar grup sumber daya, klik entri **AZ500LAB131415**.

9. Pada bilah grup sumber daya **AZ500LAB131415**, dalam daftar sumber daya, klik entri yang mewakili aplikasi logika **Ubah-Insiden-Keparahan** yang baru dibuat.

10. Pada bilah **Ubah-Insiden-Keparahan**, klik **Edit**.

    >**Catatan**: Pada bilah **Perancang Logic Apps**, masing-masing dari empat koneksi menampilkan peringatan. Ini berarti bahwa masing-masing perlu ditinjau dan dikonfigurasi.

11. Pada bilah **Perancang Logic Apps**, klik langkah **Koneksi** pertama.

12. Klik **Tambahkan baru**, pastikan entri dalam daftar tarik-turun **Penyewa** berisi nama penyewa Azure AD Anda dan klik **Masuk**.

13. Saat diminta, masuk dengan akun pengguna yang memiliki peran Pemilik atau Kontributor di langganan Azure yang Anda gunakan untuk lab ini.

14. Klik langkah **Koneksi** kedua dan, dalam daftar koneksi, pilih entri kedua, yang mewakili koneksi yang Anda buat di langkah sebelumnya.

15. Ulangi langkah sebelumnya untuk dua langkah **Koneksi** yang tersisa.

    >**Catatan**: Pastikan tidak ada peringatan yang ditampilkan pada salah satu langkah.

16. Pada bilah **Perancang Logic Apps**, klik **Simpan** untuk menyimpan perubahan Anda.

#### <a name="task-5-create-a-custom-alert-and-configure-a-playbook-as-an-automated-response"></a>Tugas 5: Buat peringatan khusus dan konfigurasikan buku pedoman sebagai respons otomatis

1. Di portal Microsoft Azure, navigasikan kembali ke bilah **Gambaran Umum Microsoft Sentinel \|** .

2. Pada bilah **Gambaran Umum Microsoft Sentinel \|** , di bagian **Konfigurasi**, klik **Analytics**.

3. Pada bilah **Analitik Microsoft Sentinel \|** , klik **+ Buat** dan, di menu tarik-turun, klik **Aturan kueri terjadwal**. 

4. Pada tab **Umum** pada bilah **Wizard aturan analitik - Buat aturan baru**, tentukan pengaturan berikut (biarkan yang lain dengan nilai defaultnya):

    |Pengaturan|Nilai|
    |---|---|
    |Nama|**Demo Playbook**|
    |Taktik|**Akses Awal**|

5. Klik **Berikutnya: Atur logika aturan >** .

6. Pada tab **Mengatur logika aturan** dari bilah **Wizard aturan Analytics - Buat aturan baru**, di kotak teks **Kueri aturan**, rekatkan kueri aturan berikut. 

    ```
    AzureActivity
     | where ResourceProviderValue =~ "Microsoft.Security" 
     | where OperationNameValue =~ "Microsoft.Security/locations/jitNetworkAccessPolicies/delete" 
    ```

    >**Catatan**: Aturan ini mengidentifikasi penghapusan kebijakan akses VM Tepat Waktu.

    >**Catatan** jika Anda menerima galat penguraian, intellisense mungkin telah menambahkan nilai ke kueri Anda. Pastikan kueri cocok jika tidak, tempel kueri ke notepad lalu dari notepad ke kueri aturan. 


7. Pada tab **Atur logika aturan** dari bilah **Wizard aturan analitik - Buat aturan baru**, di bagian **Penjadwalan kueri**, atur **Jalankan kueri setiap** hingga **5 Menit**.

8. Pada tab **Atur logika aturan** pada bilah **Wizard aturan analitik - Buat aturan baru**, terima nilai default dari pengaturan yang tersisa dan klik **Berikutnya: Pengaturan insiden >** .

9. Pada tab **Pengaturan insiden** pada bilah **Wizard aturan analitik - Buat aturan baru**, terima pengaturan default dan klik **Berikutnya: Respons otomatis >** . 

10. Pada tab **Respons otomatis** pada bilah **Wizard aturan Analytic - Buat aturan baru**, dalam daftar tarik-turun **Peringatan automatis**, centang kotak di samping  **Ubah-Insiden-Keparahan** dan klik **Berikutnya: Tinjau >** . 

11. Pada tab **Tinjau dan buat** bilah **Wizard aturan analitik - Buat aturan baru**, klik **Buat**.

    >**Catatan**: Anda sekarang memiliki aturan aktif baru yang disebut **Demo Playbook**. Jika suatu peristiwa yang diidentifikasi oleh logika rue terjadi, itu akan menghasilkan peringatan tingkat keparahan sedang, yang akan menghasilkan insiden yang sesuai.

#### <a name="task-6-invoke-an-incident-and-review-the-associated-actions"></a>Tugas 6: Panggil insiden dan tinjau tindakan terkait.

1. Di portal Microsoft Azure, navigasikan ke bilah **Gambaran Umum Microsoft Defender untuk Cloud \|** .

    >**Catatan**: Periksa skor aman Anda. Sekarang seharusnya sudah diperbarui. 

2. Pada bilah **Proteksi Beban Kerja Microsoft Defender untuk Cloud \|** , klik **Akses VM tepat waktu** di bawah **Perlindungan lanjutan**.

3. Pada bilah **Akses VM tepat waktu Microsoft Defender untuk Cloud \|** , di sisi kanan baris yang merujuk ke mesin virtual **myVM**, klik tombol **elips**, klik **Hapus**, lalu klik **Ya**.

    >**Catatan:** Jika VM tidak tercantum dalam **Just-in-time VM**, navigasikan ke bilah **Virutal Machine** dan klik **Konfigurasi**, Klik tombol **Aktifkan Opsi VM tepat waktu** di bawah **Akses Vm tepat waktu**. Ulangi langkah di atas untuk menavigasi kembali ke **Microsoft Defender untuk Cloud** dan menyegarkan halaman, VM akan muncul.

4. Di portal Microsoft Azure, di kotak teks **Sumber daya pencarian, layanan, dan dokumen** di bagian atas halaman portal Microsoft Azure, ketik **Activity log** dan tekan kunci **Enter**.

5. Navigasikan ke bilah **Log aktivitas**, catat entri **Hapus Kebijakan Akses Jaringan JIT**. 

    >**Catatan**: Ini mungkin membutuhkan satu menit untuk muncul. 

6. Di portal Microsoft Azure, navigasikan kembali ke bilah **Gambaran Umum Microsoft Sentinel \|** .

7. Pada bilah **Microsoft Sentinel \| Ikhtisar**, tinjau dasbor dan verifikasi bahwa itu menampilkan peringatan yang sesuai dengan penghapusan kebijakan akses VM Tepat Waktu.

    >**Catatan**: Diperlukan waktu hingga 5 menit agar peringatan muncul di bilah **Microsoft Sentinel \| Ikhtisar**. Jika Anda tidak melihat peringatan pada saat itu, jalankan aturan kueri yang dirujuk dalam tugas sebelumnya untuk memverifikasi bahwa aktivitas penghapusan kebijakan akses Tepat Waktu telah disebarkan ke ruang kerja Analisis Log yang terkait dengan instans Microsoft Sentinel Anda. Jika bukan itu masalahnya, buat kembali kebijakan akses VM Just in time dan hapus lagi.

8. Pada bilah **Microsoft Sentinel \| Ikhtisar**, di bagian **Pengelolaan Ancaman**, klik **Insiden**.

9. Pastikan bilah menampilkan insiden dengan tingkat keparahan sedang atau tinggi.

    >**Catatan**: Diperlukan waktu hingga 5 menit agar insiden muncul di bilah **Microsoft Sentinel \| Incidents**. 

    >**Catatan**: Tinjau bilah **Microsoft Sentinel \| Playbooks**. Anda akan menemukan di sana hitungan berhasil dan gagal berjalan.

    >**Catatan**: Anda memiliki pilihan untuk menetapkan tingkat keparahan dan status yang berbeda untuk sebuah insiden.

> Hasil: Anda telah membuat ruang kerja Microsoft Sentinel, menghubungkannya ke log Aktivitas Azure, membuat buku pedoman dan peringatan khusus yang dipicu sebagai respons terhadap penghapusan kebijakan akses VM Tepat Waktu, dan memverifikasi bahwa konfigurasi tersebut valid.

**Membersihkan sumber daya**

> Jangan lupa untuk menghapus sumber daya Azure yang baru dibuat dan yang tidak diperlukan lagi. Menghapus sumber daya yang tidak terpakai memastikan Anda tidak akan dikenakan biaya tak terduga. 

1. Di portal Microsoft Azure, buka Cloud Shell dengan mengklik ikon pertama di kanan atas Portal Azure. Jika diminta, klik **PowerShell** dan **Buat penyimpanan**.

2. Pastikan **PowerShell** dipilih di menu dropdown di sudut kiri atas panel Cloud Shell.

3. Di sesi PowerShell dalam panel Cloud Shell, jalankan perintah berikut untuk menghapus grup sumber daya yang Anda buat di lab ini:
  
    ```powershell
    Remove-AzResourceGroup -Name "AZ500LAB131415" -Force -AsJob
    ```
4. Tutup panel **Cloud Shell**.
