---
lab:
  title: 02 - Azure Policy
  module: Module 01 - Manage Identity and Access
ms.openlocfilehash: d49ce05e4620310d45317fe582bddb3aa511430b
ms.sourcegitcommit: 967cb50981ef07d731dd7548845a38385b3fb7fb
ms.translationtype: HT
ms.contentlocale: id-ID
ms.lasthandoff: 05/31/2022
ms.locfileid: "145955390"
---
# <a name="lab-02-azure-policy"></a>Lab 02: Azure Policy
# <a name="student-lab-manual"></a>Panduan lab siswa

## <a name="lab-scenario"></a>Skenario lab

Anda telah diminta untuk membuat bukti konsep yang menunjukkan bagaimana kebijakan Azure dapat digunakan. Secara khusus, Anda perlu:

- Buat kebijakan Lokasi yang Diizinkan yang memastikan sumber daya hanya dibuat di wilayah tertentu.
- Uji untuk memastikan sumber daya hanya dibuat di lokasi yang diizinkan

> Untuk semua sumber daya di lab ini, kita menggunakan wilayah **US Timur**. Pastikan pada instruktur Anda bahwa ini adalah wilayah yang akan digunakan untuk kelas. 

## <a name="lab-objectives"></a>Tujuan lab

Di lab ini, Anda akan menyelesaikan hal berikut:

- Latihan 1: Menerapkan Azure Policy. 

## <a name="azure-policy-diagram"></a>diagram Azure Policy

![gambar](https://user-images.githubusercontent.com/91347931/157511920-19c1f06c-86bd-440d-80ac-d96aa27aefff.png)

## <a name="instructions"></a>Instruksi

### <a name="exercise-1-implement-azure-policy"></a>Latihan 1: Menerapkan Azure Policy

#### <a name="estimated-timing-20-minutes"></a>Perkiraan waktu: 20 menit

Dalam latihan ini, Anda akan menyelesaikan tugas-tugas berikut:

- Tugas 1: Buat grup sumber daya Azure. 
- Tugas 2: Buat penetapan kebijakan Lokasi yang Diizinkan.
- Tugas 3: Pastikan penetapan kebijakan Lokasi yang Diizinkan berfungsi. 

#### <a name="task-1-create-a-resource-group-for-the-lab"></a>Tugas 1: Buat grup sumber daya untuk lab. 

Dalam tugas ini, Anda akan membuat grup sumber daya untuk lab. 

1. Masuk ke portal Microsoft Azure **`https://portal.azure.com/`** .

    >**Catatan**: Masuk ke portal Microsoft Azure menggunakan akun yang memiliki peran Pemilik atau Kontributor dalam langganan Azure yang Anda gunakan untuk lab ini.

1. Buka Cloud Shell dengan mengklik ikon pertama di kanan atas Portal Azure. Jika diminta, pilih **PowerShell** dan **Buat penyimpanan**.

1. Pastikan **PowerShell** dipilih di menu drop-down di sudut kiri atas panel Cloud Shell.

1. Di sesi PowerShell dalam panel Cloud Shell, jalankan yang berikut ini untuk membuat grup sumber daya (verifikasi dengan instruktur Anda mengenai nilai parameter lokasi):

    ```powershell
    New-AzResourceGroup -Name AZ500LAB02 -Location 'East US'
    ```

1. Di sesi PowerShell dalam panel Cloud Shell, jalankan perintah berikut untuk membuat daftar grup sumber daya untuk memverifikasi bahwa grup sumber daya baru telah dibuat:

    ```powershell
    Get-AzResourceGroup | format-table
    ```

1. Tutup **Cloud Shell**.

#### <a name="task-2-create-an-allowed-locations-policy-assignment"></a>Tugas 2: Buat penetapan kebijakan Lokasi yang Diizinkan.

Dalam tugas ini, Anda akan membuat penetapan kebijakan Lokasi yang Diizinkan dan menentukan wilayah Azure mana yang dapat digunakan kebijakan. 

1. Di portal Microsoft Azure, dalam kotak teks **Cari sumber daya, layanan, dan dokumen** di bagian atas halaman portal Microsoft Azure, ketik **Kebijakan** dan tekan tombol **Enter**.

1. Pada panel **Kebijakan**, di bagian **Penulisan**, pilih **Definisi**.

1. Luangkan waktu sebentar untuk menelusuri definisi bawaan. Gunakan dropdown **Kategori** untuk memfilter daftar kebijakan.

1. Di kotak teks **Telusuri**, ketik **Lokasi yang diizinkan**. 

   >**Catatan**: Kebijakan **Lokasi yang diizinkan** memungkinkan Anda membatasi lokasi sumber daya, bukan grup sumber daya. Untuk membatasi lokasi grup sumber daya, Anda dapat menggunakan kebijakan **Lokasi yang diizinkan untuk grup sumber daya**.

1. Klik definisi kebijakan **Lokasi yang diizinkan** untuk menampilkan detailnya.. 

   >**Catatan**: Definisi kebijakan ini mengambil array lokasi sebagai parameter. Aturan kebijakan adalah pernyataan 'jika-maka'. Klausul 'jika' memeriksa apakah lokasi sumber daya disertakan dalam daftar parameter, dan jika tidak, klausul 'maka' menolak pembuatan sumber daya atau, untuk sumber daya yang ada, menandainya sebagai tidak sesuai.

1. Pada panel **Lokasi yang diizinkan**, klik **Tetapkan**.

1. Pada tab **Dasar** dari panel **Lokasi yang diizinkan**, klik tombol Elipsis (...) di samping kotak teks **Cakupan** dan, pada panel **Cakupan**, tentukan pengaturan berikut:

   |Pengaturan|Nilai|
   |---|---|
   |Langganan|nama langganan Azure Anda|
   |Grup sumber daya|**AZ500LAB02**|

1. Klik **Pilih**.

1. Pada panel **Lokasi yang diizinkan**, pada tab **Dasar-dasar**, tentukan setelan berikut (biarkan yang lain dengan nilai defaultnya):

   |Pengaturan|Nilai|
   |---|---|
   |Nama penetapan|**Izinkan UK Selatan untuk AZ500LAB02**|
   |Deskripsi|**Izinkan sumber daya dibuat di UK Selatan Hanya untuk AZ500LAB02**|
   |Pemberlakuan kebijakan|**Aktif**|

1. Klik **Berikutnya**.

1. Pada tab **Parameter** pada panel **Lokasi yang diizinkan**, dalam daftar dropbox **Lokasi yang diizinkan**, pilih **UK Selatan** sebagai satu-satunya yang diizinkan lokasi. 

   >**Catatan**: Anda dapat memilih lebih dari satu lokasi. Jika kebijakan memerlukan kumpulan parameter yang berbeda, tab ini akan memberikan pilihan tersebut. 

1. Klik **Tinjau + buat**, diikuti dengan **Buat** untuk membuat penetapan kebijakan. 

   >**Catatan**: Anda akan melihat pemberitahuan bahwa tugas telah berhasil, dan tugas tersebut mungkin memerlukan waktu sekitar 30 menit untuk diselesaikan.

   >**Catatan**: Alasan penetapan kebijakan Azure mungkin memerlukan waktu hingga 30 menit untuk diterapkan adalah karena harus direplikasi secara global. Biasanya ini hanya memakan waktu beberapa menit.  Jika tugas berikutnya gagal, cukup tunggu beberapa menit dan coba lagi langkahnya.

#### <a name="task-3-test-the-allowed-locations-policy-assignment"></a>Tugas 3: Uji penetapan kebijakan Lokasi yang Diizinkan

Dalam tugas ini, Anda akan menguji penetapan kebijakan Lokasi yang Diizinkan. 

1. Di portal Microsoft Azure, di kotak teks **Cari sumber daya, layanan, dan dokumen** di bagian atas halaman portal Microsoft Azure, ketik **Jaringan virtual** dan tekan tombol **Enter**.

1. Pada panel **Jaringan Virtual**, klik **+ Buat**.

   >**Catatan**: Pertama, Anda akan mencoba membuat jaringan virtual di US Timur. Karena ini bukan lokasi yang diizinkan, permintaan harus diblokir. 

1. Pada tab **Dasar** pada panel **Buat jaringan virtual**, tentukan pengaturan berikut (biarkan yang lain dengan nilai defaultnya):

    |Pengaturan|Nilai|
    |---|---|
    |Grup sumber daya|**AZ500LAB02**|
    |Nama|**myVnet**|
    |Wilayah|**(AS) AS Timur**|

1. Klik **Ulas + buat**. 

1. Pada tab **Tinjau + buat** panel **Buat jaringan virtual** perhatikan pesan **Validasi gagal**. 

    > **Catatan**: Jika peringatan **Validasi Gagal** tidak muncul, klik **Sebelumnya** dan tunggu beberapa menit lagi.

1. Pada tab **Dasar**, klik tautan pesan kesalahan untuk membuka panel **Penugasan Kebijakan**. Anda akan melihat penetapan kebijakan yang membatasi lokasi.

1. Tutup panel **Penugasan Kebijakan**, pada panel **Buat jaringan virtual**, klik tab **Dasar**, dan, di dropdown **Wilayah** daftar, pilih **(Eropa) UK Selatan**.

1. Klik **Tinjau + buat**, verifikasi bahwa validasi lulus, klik **Buat**, dan verifikasi bahwa jaringan virtual berhasil dibuat. 

> Hasil latihan: Dalam latihan ini, Anda belajar menerapkan kebijakan Azure dengan memilih definisi kebijakan bawaan dan menetapkannya ke grup sumber daya.

**Membersihkan sumber daya**

> Jangan lupa untuk menghapus sumber daya Azure yang baru dibuat dan yang tidak diperlukan lagi. Menghapus sumber daya yang tidak terpakai memastikan Anda tidak akan dikenakan biaya tidak terduga.

1. Di portal Microsoft Azure, buka Cloud Shell dengan mengklik ikon pertama di kanan atas Portal Microsoft Azure. Jika diminta, klik **Hubungkan kembali**.

1. Di sesi PowerShell dalam panel Cloud Shell, jalankan perintah berikut untuk menghapus grup sumber daya yang Anda buat di lab ini:
  
    ```powershell
    Remove-AzResourceGroup -Name "AZ500LAB02" -Force -AsJob
    ```
1.  Tutup panel **Cloud Shell**. 
  
1. Di portal Microsoft Azure, dalam kotak teks **Cari sumber daya, layanan, dan dokumen** di bagian atas halaman portal Microsoft Azure, ketik **Kebijakan** dan tekan tombol **Enter**.

1. Di bagian Penulisan, pilih **Penugasan**.

1. Dalam daftar tugas, pilih nama kebijakan **Lokasi yang Diizinkan** yang Anda buat di lab ini.

1. Pada penetapan kebijakan, pilih **Hapus penetapan**, lalu pilih **Ya**.
