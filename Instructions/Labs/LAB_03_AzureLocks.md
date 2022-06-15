---
lab:
  title: 03 - Kunci Azure Resource Manager
  module: Module 01 - Manage Identity and Access
ms.openlocfilehash: 54375454646bdcf0586b249f65349691c3a3b9c3
ms.sourcegitcommit: a8470295248a6363987bd5ea47154fe39f8535c3
ms.translationtype: HT
ms.contentlocale: id-ID
ms.lasthandoff: 03/09/2022
ms.locfileid: "145195927"
---
# <a name="lab-03-resource-manager-locks"></a>Lab 03: Kunci Azure Resource Manager
# <a name="student-lab-manual"></a>Panduan lab siswa

## <a name="lab-scenario"></a>Skenario lab 

Anda telah diminta untuk membuat bukti konsep yang menunjukkan bagaimana kunci sumber daya dapat digunakan untuk mencegah penghapusan atau perubahan yang tidak disengaja. Secara khusus, Anda perlu:

- buat kunci ReadOnly

- buat kunci Hapus

> Untuk semua sumber daya di lab ini, kita menggunakan wilayah **US Timur**. Pastikan pada instruktur Anda bahwa ini adalah wilayah yang akan digunakan untuk kelas. 
 
## <a name="lab-objectives"></a>Tujuan lab

Di lab ini, Anda akan menyelesaikan latihan berikut:

- Latihan 1: Kunci Azure Resource Manager

## <a name="resource-manager-locks-diagram"></a>Diagram Kunci Azure Resource Manager

![gambar](https://user-images.githubusercontent.com/91347931/157514986-1bf6a9ea-4c7f-4487-bcd7-542648f8dc95.png)

## <a name="instructions"></a>Instruksi

### <a name="exercise-1-resource-manager-locks"></a>Latihan 1: Kunci Azure Resource Manager

#### <a name="estimated-timing-20-minutes"></a>Perkiraan waktu: 20 menit

Dalam latihan ini, Anda akan menyelesaikan tugas-tugas berikut:

- Tugas 1: Buat grup sumber daya dengan akun penyimpanan.
- Tugas 2: Tambahkan kunci ReadOnly pada akun penyimpanan. 
- Tugas 3: Uji kunci ReadOnly. 
- Tugas 4: Hapus kunci ReadOnly dan buat kunci Hapus.
- Tugas 5: Uji kunci Hapus.

#### <a name="task-1-create-a-resource-group-with-a-storage-account"></a>Tugas 1: Buat grup sumber daya dengan akun penyimpanan.

Dalam tugas ini, Anda akan membuat grup sumber daya dan akun penyimpanan untuk lab. 

1. Masuk ke portal Microsoft Azure **`https://portal.azure.com/`** .

    >**Catatan**: Masuk ke portal Microsoft Azure menggunakan akun yang memiliki peran Pemilik atau Kontributor dalam langganan Azure yang Anda gunakan untuk lab ini.

1. Buka Cloud Shell dengan mengklik ikon pertama di kanan atas Portal Azure. Jika diminta, pilih **PowerShell** dan **Buat penyimpanan**.

1. Pastikan **PowerShell** dipilih di menu drop-down di sudut kiri atas panel Cloud Shell.

1. Di sesi PowerShell dalam panel Cloud Shell, jalankan yang berikut ini untuk membuat grup sumber daya (verifikasi dengan instruktur Anda mengenai nilai parameter lokasi):

    ```powershell
    New-AzResourceGroup -Name AZ500LAB03 -Location 'EastUS'
    ```

1. Di sesi PowerShell dalam panel Cloud Shell, jalankan berikut ini untuk membuat akun penyimpanan di grup sumber daya yang baru dibuat:
    
    ```powershell
    New-AzStorageAccount -ResourceGroupName AZ500LAB03 -Name (Get-Random -Maximum 999999999999999) -Location  EastUS -SkuName Standard_LRS -Kind StorageV2 
    ```

   >**Catatan**:  Tunggu hingga akun penyimpanan dibuat. Ini mungkin memakan waktu beberapa menit. 

1. Tutup panel Cloud Shell.

#### <a name="task-2-add-a-readonly-lock-on-the-storage-account"></a>Tugas 2: Tambahkan kunci ReadOnly pada akun penyimpanan. 

Dalam tugas ini, Anda akan menambahkan kunci hanya baca ke akun penyimpanan. Ini akan melindungi sumber daya dari penghapusan atau modifikasi yang tidak disengaja. 

1. Di portal Microsoft Azure, di kotak teks **Cari sumber daya, layanan, dan dokumen** di bagian atas halaman portal Microsoft Azure, ketik **Grup sumber daya** dan tekan tombol **Enter**.

1. Pada panel **Grup sumber daya**, pilih entri grup sumber daya **AZ500LAB03**.

1. Pada panel grup sumber daya **AZ500LAB03**, dalam daftar sumber daya, pilih akun penyimpanan baru. 

1. Di bagian **Pengaturan**, klik ikon "Kunci".

1. Klik **+ Tambahkan** dan tentukan pengaturan berikut:

   |Pengaturan|Nilai|
   |---|---|
   |Nama kunci|**Kunci Hanya Baca**|
   |Jenis kunci|**Baca-saja**|

1. Klik **OK**. 

   >**Catatan**:  Akun penyimpanan sekarang dilindungi dari penghapusan dan modifikasi yang tidak disengaja.

#### <a name="task-3-test-the-readonly-lock"></a>Tugas 3: Uji kunci ReadOnly 

1. Di bagian **Pengaturan** panel akun penyimpanan, klik **Konfigurasi**.

1. Atur **Diperlukan transfer aman** ke **Dinonaktifkan** lalu klik **Simpan**.

1. Anda seharusnya dapat melihat pemberitahuan yang menyatakan **Gagal memperbarui akun penyimpanan**.

1. Klik ikon **Pemberitahuan** pada toolbar di bagian atas portal Azure dan tinjau pemberitahuan tersebut, yang akan menyerupai teks berikut: 

    > **"Gagal memperbarui akun penyimpanan 'xxxxxxxx'. Kesalahan: Cakupan 'xxxxxxxx' tidak dapat melakukan operasi penulisan karena cakupan berikut dikunci: '/subscriptions/xxxxx-xxx-xxxx-xxxx-xxxxxxxx/resourceGroups/AZ500LAB03/providers/Microsoft.Storage/storageAccounts/xxxxxxx'. Hapus kunci dan coba lagi"**

1. Kembalikan panel **Konfigurasi** akun penyimpanan dan klik **Buang**. 

1. Pada panel akun penyimpanan, pilih **Gambaran Umum** dan, pada panel **Gambaran Umum**, klik **Hapus**.

1. Pada panel **Hapus akun penyimpanan**, ketik nama akun penyimpanan untuk mengonfirmasi bahwa Anda ingin melanjutkan, lalu klik **Hapus**.

1. Tinjau pemberitahuan yang baru dibuat, yang akan menyerupai teks berikut: 

    > **"Gagal menghapus akun penyimpanan 'xxxxxxx'. Kesalahan: Cakupan 'xxxxxxx' tidak dapat melakukan operasi penghapusan karena cakupan berikut dikunci: '/subscriptions/xxxx-xxxx-xxxx-xxxx-xxxxxx/resourceGroups/AZ500LAB03/providers/Microsoft.Storage/storageAccounts/xxxxxxx'. Hapus kunci dan coba lagi."**

   >**Catatan**:  Anda sekarang telah memverifikasi bahwa kunci ReadOnly akan menghentikan penghapusan dan modifikasi sumber daya yang tidak disengaja.

#### <a name="task-4-remove-the-readonly-lock-and-create-a-delete-lock"></a>Tugas 4: Hapus kunci ReadOnly dan buat kunci Hapus.

Dalam tugas ini, Anda menghapus kunci ReadOnly dari akun penyimpanan dan membuat kunci Hapus. 

1. Di portal Azure, navigasikan kembali ke panel yang menampilkan properti dari akun penyimpanan yang baru dibuat.

1. Di bagian **Pengaturan**, pilih **Kunci**.  

1. Pada bilah **Kunci**, klik ikon **Hapus** di ujung kanan entri **Kunci Hanya-Baca**.

1. Klik **+ Tambahkan** dan tentukan pengaturan berikut:

   |Pengaturan|Nilai|
   |---|---|
   |Nama kunci|**Hapus Kunci**|
   |Jenis kunci|**Hapus**|

1. Klik **OK**. 

#### <a name="task-5-test-the-delete-lock"></a>Tugas 5: Uji kunci Hapus.

Dalam tugas ini, Anda akan menguji kunci Hapus. Anda seharusnya dapat mengubah akun penyimpanan, tetapi tidak menghapusnya. 

1. Di bagian **Pengaturan** panel akun penyimpanan, klik **Konfigurasi**.

1. Atur **Diperlukan transfer aman** ke **Dinonaktifkan** lalu klik **Simpan**.

   >**Catatan**:  Kali ini, perubahan harus berhasil.

1. Pada panel akun penyimpanan, pilih **Gambaran Umum** dan, pada panel **Gambaran Umum**, klik **Hapus**.

1. Pada panel **Hapus akun penyimpanan**, ketik nama akun penyimpanan untuk mengonfirmasi bahwa Anda ingin melanjutkan, lalu klik **Hapus**.

1. Ulas pemberitahuan yang menyerupai teks berikut: 

    > **'xxxxxx' tidak dapat dihapus karena sumber daya ini atau induknya memiliki kunci hapus. Kunci harus dilepas sebelum sumber daya ini dapat dihapus"**

   >**Catatan**:  Anda sekarang telah memverifikasi bahwa kunci **Hapus** akan mengizinkan perubahan konfigurasi tetapi menghentikan penghapusan yang tidak disengaja.

   >**Catatan**:  Dengan menggunakan Resource Locks, Anda dapat menerapkan garis pertahanan ekstra terhadap perubahan yang tidak disengaja atau berbahaya dan/atau penghapusan sumber daya yang paling penting. Kunci sumber daya dapat dihapus oleh pengguna mana pun dengan peran **Pemilik**, tetapi untuk melakukannya memerlukan upaya yang sadar. Kunci melengkapi Microsoft Azure Access Control Service Berbasis Peran. 

> Hasil: Dalam latihan ini, Anda belajar menggunakan kunci Resource Manager untuk melindungi sumber daya dari modifikasi dan penghapusan yang tidak disengaja.

**Membersihkan sumber daya**

> Jangan lupa untuk menghapus sumber daya Azure yang baru dibuat dan yang tidak diperlukan lagi. Menghapus sumber daya yang tidak terpakai memastikan Anda tidak akan dikenakan biaya tidak terduga.

1. Di portal Microsoft Azure, buka Cloud Shell dengan mengklik ikon pertama di kanan atas Portal Microsoft Azure. Jika diminta, klik **Hubungkan kembali**.

1. Di sesi PowerShell dalam panel Cloud Shell, jalankan perintah berikut untuk menghapus kunci penghapusan:

    ```powershell
    $storageaccountname = (Get-AzStorageAccount -ResourceGroupName AZ500LAB03).StorageAccountName

    $lockName = (Get-AzResourceLock -ResourceGroupName AZ500LAB03 -ResourceName $storageAccountName -ResourceType Microsoft.Storage/storageAccounts).Name

    Remove-AzResourceLock -LockName $lockName -ResourceName $storageAccountName  -ResourceGroupName AZ500LAB03 -ResourceType Microsoft.Storage/storageAccounts -Force
    ```

1.  Di sesi PowerShell dalam panel Cloud Shell, jalankan perintah berikut untuk menghapus grup sumber daya:

    ```powershell
    Remove-AzResourceGroup -Name "AZ500LAB03" -Force -AsJob
    ```

1.  Tutup panel **Cloud Shell**. 
