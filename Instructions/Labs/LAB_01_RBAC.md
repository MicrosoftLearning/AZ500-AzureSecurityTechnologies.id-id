---
lab:
  title: 01 - Access Control Berbasis Peran
  module: Module 01 - Manage Identity and Access
ms.openlocfilehash: 9520d720976926da7583c53a50cc25b216286d8a
ms.sourcegitcommit: 967cb50981ef07d731dd7548845a38385b3fb7fb
ms.translationtype: HT
ms.contentlocale: id-ID
ms.lasthandoff: 05/31/2022
ms.locfileid: "145955399"
---
# <a name="lab-01-role-based-access-control"></a>Lab 01: Access Control Berbasis Peran
# <a name="student-lab-manual"></a>Panduan lab siswa

## <a name="lab-scenario"></a>Skenario lab

Anda telah diminta untuk membuat bukti konsep yang menunjukkan bagaimana pengguna dan grup Azure dibuat. Juga, bagaimana kontrol akses berbasis peran digunakan untuk menetapkan peran ke grup. Secara khusus, Anda perlu:

- Membuat grup Admin Senior yang berisi akun pengguna Joseph Price sebagai anggotanya.
- Membuat grup Admin Junior yang berisi akun pengguna Isabel Garcia sebagai anggotanya.
- Membuat grup Service Desk yang berisi akun pengguna Dylan Williams sebagai anggotanya.
- Menetapkan peran Kontributor Komputer Virtual ke grup Service Desk. 

> Untuk semua sumber daya di lab ini, kita menggunakan wilayah **US Timur**. Pastikan pada instruktur Anda bahwa ini adalah wilayah yang akan digunakan untuk kelas. 

## <a name="lab-objectives"></a>Tujuan lab

Di lab ini, Anda akan menyelesaikan latihan berikut:

- Latihan 1: Membuat grup Admin Senior dengan akun pengguna Joseph Price sebagai anggotanya (portal Azure). 
- Latihan 2: Membuat grup Admin Junior dengan akun pengguna Isabel Garcia sebagai anggotanya (PowerShell).
- Latihan 3: Membuat grup Service Desk dengan pengguna Dylan Williams sebagai anggotanya (Azure CLI). 
- Latihan 4: Menetapkan peran Kontributor Komputer Virtual ke grup Service Desk.

## <a name="role-based-access-control-architecture-diagram"></a>Diagram arsitektur Access Control Berbasis Peran

![gambar](https://user-images.githubusercontent.com/91347931/157751243-5aa6e521-9bc1-40af-839b-4fd9927479d7.png)

## <a name="instructions"></a>Instruksi

### <a name="exercise-1-create-the-senior-admins-group-with-the-user-account-joseph-price-as-its-member"></a>Latihan 1: Membuat grup Admin Senior dengan akun pengguna Joseph Price sebagai anggotanya. 

#### <a name="estimated-timing-10-minutes"></a>Perkiraan waktu: 10 menit

Dalam latihan ini, Anda akan menyelesaikan tugas-tugas berikut:

- Tugas 1: Menggunakan portal Azure untuk membuat akun pengguna untuk Joseph Price.
- Tugas 2: Menggunakan portal Azure untuk membuat grup Admin Senior dan menambahkan akun pengguna Joseph Price ke grup.

#### <a name="task-1-use-the-azure-portal-to-create-a-user-account-for-joseph-price"></a>Tugas 1: Menggunakan portal Azure untuk membuat akun pengguna untuk Joseph Price 

Dalam tugas ini, Anda akan membuat akun pengguna untuk Joseph Price. 

1. Mulai sesi browser dan masuk ke portal Microsoft Azure **`https://portal.azure.com/`** .

    >**Catatan**: Masuk ke portal Azure menggunakan akun yang memiliki peran Pemilik atau Kontributor dalam langganan Azure yang Anda gunakan untuk lab ini dan peran Administrator Global di penyewa Azure AD yang terkait dengan langganan tersebut.

2. Dalam kotak teks **Cari sumber daya, layanan, dan dokumen** di bagian atas halaman portal Microsoft Azure, ketik **Azure Active Directory** dan tekan tombol **Enter**.

3. Pada bilah **Ringkasan** penyewa Azure Active Directory, di bagian **Kelola**, pilih **Pengguna**, lalu pilih **+ Pengguna baru**.

4. Pada panel **Pengguna Baru**, pastikan opsi **Buat pengguna** dipilih, dan tentukan pengaturan berikut:

   |Pengaturan|Nilai|
   |---|---|
   |Nama pengguna|**Joseph**|
   |Nama|**Joseph Price**|

5. Klik ikon salin di sebelah **Nama pengguna** untuk menyalin pengguna lengkap.

6. Pastikan sandi **Buat otomatis** dipilih, centang kotak **Tampilkan sandi** untuk mengidentifikasi sandi yang dibuat secara otomatis. Anda perlu memberikan kata sandi ini, bersama dengan nama pengguna untuk Joseph. 

7. Klik **Buat**.

8. Segarkan bilah **Pengguna \| Semua pengguna** untuk memverifikasi bahwa pengguna baru telah dibuat di penyewa Azure AD Anda.

#### <a name="task2-use-the-azure-portal-to-create-a-senior-admins-group-and-add-the-user-account-of-joseph-price-to-the-group"></a>Task2: Menggunakan portal Azure untuk membuat grup Admin Senior dan menambahkan akun pengguna Joseph Price ke grup.

Dalam tugas ini, Anda akan membuat grup *Admin Senior*, menambahkan akun pengguna Joseph Price ke grup, dan mengonfigurasinya sebagai pemilik grup.

1. Di portal Azure, navigasikan kembali ke bilah yang menampilkan penyewa Azure Active Directory Anda. 

2. Di bagian **Kelola**, klik **Grup**, lalu pilih **+ Grup baru**.
 
3. Pada bilah **Grup Baru**, tentukan pengaturan berikut (biarkan yang lain dengan nilai defaultnya):

   |Pengaturan|Nilai|
   |---|---|
   |Jenis grup|**Keamanan**|
   |Nama grup|**Admin Senior**|
   |Jenis keanggotaan|**Ditetapkan**|
    
4. Klik tautan **Tidak ada pemilik yang dipilih**, pada panel **Tambahkan pemilik**, pilih **Joseph Price**, dan klik **Pilih**.

5. Klik tautan **Tidak ada anggota yang dipilih**, pada panel **Tambahkan anggota**, pilih **Joseph Price**, dan klik **Pilih**.

6. Kembali ke bilah **Grup Baru**, klik **Buat**.

> Hasil: Anda menggunakan Portal Azure untuk membuat pengguna dan grup, dan menetapkan pengguna ke grup. 

### <a name="exercise-2-create-a-junior-admins-group-containing-the-user-account-of-isabel-garcia-as-its-member"></a>Latihan 2: Membuat grup Admin Junior yang berisi akun pengguna Isabel Garcia sebagai anggotanya.

#### <a name="estimated-timing-10-minutes"></a>Perkiraan waktu: 10 menit

Dalam latihan ini, Anda akan menyelesaikan tugas-tugas berikut:

- Tugas 1: Menggunakan PowerShell untuk membuat akun pengguna untuk Isabel Garcia.
- Tugas 2: Menggunakan PowerShell untuk membuat grup Admin Junior dan menambahkan akun pengguna Isabel Garcia ke grup. 

#### <a name="task-1-use-powershell-to-create-a-user-account-for-isabel-garcia"></a>Tugas 1: Menggunakan PowerShell untuk membuat akun pengguna untuk Isabel Garcia.

Dalam tugas ini, Anda akan membuat akun pengguna untuk Isabel Garcia dengan menggunakan PowerShell.

1. Buka Cloud Shell dengan mengklik ikon pertama di kanan atas Portal Azure. Jika diminta, pilih **PowerShell** dan **Buat penyimpanan**.

2. Pastikan **PowerShell** dipilih di menu drop-down di sudut kiri atas panel Cloud Shell.

   >**Catatan**: Untuk menempelkan teks yang disalin ke Cloud Shell, klik kanan di dalam jendela panel dan pilih **Tempel**. Atau, Anda dapat menggunakan kombinasi kunci **Shift+Insert**.

3. Di sesi PowerShell dalam panel Cloud Shell, jalankan elemen berikut ini untuk membuat objek profil kata sandi:

    ```powershell
    $passwordProfile = New-Object -TypeName Microsoft.Open.AzureAD.Model.PasswordProfile
    ```

4. Di sesi PowerShell dalam panel Cloud Shell, jalankan perintah berikut untuk mengatur nilai kata sandi dalam objek profil:
    ```powershell
    $passwordProfile.Password = "Pa55w.rd1234"
    ```

5. Di sesi PowerShell dalam panel Cloud Shell, jalankan perintah berikut untuk terhubung ke Azure Active Directory:

    ```powershell
    Connect-AzureAD
    ```
      
6. Di sesi PowerShell dalam panel Cloud Shell, jalankan elemen berikut ini untuk mengidentifikasi nama penyewa Azure AD Anda: 

    ```powershell
    $domainName = ((Get-AzureAdTenantDetail).VerifiedDomains)[0].Name
    ```

7. Di sesi PowerShell dalam panel Cloud Shell, jalankan perintah berikut untuk membuat akun pengguna untuk Isabel Garcia: 

    ```powershell
    New-AzureADUser -DisplayName 'Isabel Garcia' -PasswordProfile $passwordProfile -UserPrincipalName "Isabel@$domainName" -AccountEnabled $true -MailNickName 'Isabel'
    ```

8. Di sesi PowerShell dalam panel Cloud Shell, jalankan elemen berikut ini untuk mencantumkan pengguna Azure AD (akun Joseph dan Isabel akan muncul di daftar): 

    ```powershell
    Get-AzureADUser 
    ```

#### <a name="task2-use-powershell-to-create-the-junior-admins-group-and-add-the-user-account-of-isabel-garcia-to-the-group"></a>Task2: Menggunakan PowerShell untuk membuat grup Admin Junior dan menambahkan akun pengguna Isabel Garcia ke grup.

Dalam tugas ini, Anda akan membuat grup Admin Junior dan menambahkan akun pengguna Isabel Garcia ke grup dengan menggunakan PowerShell.

1. Di sesi PowerShell yang sama dalam panel Cloud Shell, jalankan perintah berikut untuk membuat grup keamanan baru bernama Admin Junior:
    
    ```powershell
    New-AzureADGroup -DisplayName 'Junior Admins' -MailEnabled $false -SecurityEnabled $true -MailNickName JuniorAdmins
    ```

2. Di sesi PowerShell dalam panel Cloud Shell, jalankan elemen berikut ini untuk membuat daftar grup di penyewa Azure AD Anda (daftar harus menyertakan grup Admin Senior dan Admin Junior):

    ```powershell
    Get-AzureADGroup
    ```

3. Di sesi PowerShell dalam panel Cloud Shell, jalankan perintah berikut untuk mendapatkan referensi ke akun pengguna Isabel Garcia:

    ```powershell
    $user = Get-AzureADUser -Filter "MailNickName eq 'Isabel'"
    ```

4. Di sesi PowerShell dalam panel Cloud Shell, jalankan perintah berikut untuk menambahkan akun pengguna Isabel ke grup Admin Junior:
    
    ```powershell
    Add-AzADGroupMember -MemberUserPrincipalName $user.userPrincipalName -TargetGroupDisplayName "Junior Admins" 
    ```

5. Di sesi PowerShell dalam panel Cloud Shell, jalankan perintah berikut untuk memverifikasi bahwa grup Admin Junior berisi akun pengguna Isabel:

    ```powershell
    Get-AzADGroupMember -GroupDisplayName "Junior Admins"
    ```

> Hasil: Anda menggunakan PowerShell untuk membuat akun pengguna dan grup, dan menambahkan akun pengguna ke akun grup. 


### <a name="exercise-3-create-a-service-desk-group-containing-the-user-account-of-dylan-williams-as-its-member"></a>Latihan 3: Membuat grup Service Desk yang berisi akun pengguna Dylan Williams sebagai anggotanya.

#### <a name="estimated-timing-10-minutes"></a>Perkiraan waktu: 10 menit

Dalam latihan ini, Anda akan menyelesaikan tugas-tugas berikut:

- Tugas 1: Menggunakan Azure CLI untuk membuat akun pengguna untuk Dylan Williams.
- Tugas 2: Menggunakan Azure CLI untuk membuat grup Service Desk dan menambahkan akun pengguna Dylan ke grup. 

#### <a name="task-1-use-azure-cli-to-create-a-user-account-for-dylan-williams"></a>Tugas 1: Menggunakan Azure CLI untuk membuat akun pengguna untuk Dylan Williams.

Dalam tugas ini, Anda akan membuat akun pengguna untuk Dylan Williams.

1. Pada menu drop-down di sudut kiri atas panel Cloud Shell, pilih **Bash**, dan, jika diminta, klik **Konfirmasi**. 

2. Di sesi Bash dalam panel Cloud Shell, jalankan elemen berikut ini untuk mengidentifikasi nama penyewa Azure AD Anda:

    ```cli
    DOMAINNAME=$(az ad signed-in-user show --query 'userPrincipalName' | cut -d '@' -f 2 | sed 's/\"//')
    ```

3. Di sesi Bash dalam panel Cloud Shell, jalankan perintah berikut untuk membuat pengguna, Dylan Williams. Gunakan *domainanda*.
 
    ```cli
    az ad user create --display-name "Dylan Williams" --password "Pa55w.rd1234" --user-principal-name Dylan@$DOMAINNAME
    ```
      
4. Di sesi Bash dalam panel Cloud Shell, jalankan elemen berikut ini untuk mencantumkan akun pengguna Azure AD (daftar tersebut harus menyertakan akun pengguna Joseph, Isabel, dan Dylan)
    
    ```cli
    az ad user list --output table
    ```

#### <a name="task-2-use-azure-cli-to-create-the-service-desk-group-and-add-the-user-account-of-dylan-to-the-group"></a>Tugas 2: Menggunakan Azure CLI untuk membuat grup Service Desk dan menambahkan akun pengguna Dylan ke grup. 

Dalam tugas ini, Anda akan membuat grup Service Desk dan menetapkan Dylan ke grup. 

1. Di sesi Bash yang sama dalam panel Cloud Shell, jalankan perintah berikut untuk membuat grup keamanan baru bernama Service Desk.

    ```cli
    az ad group create --display-name "Service Desk" --mail-nickname "ServiceDesk"
    ```
 
2. Di sesi Bash dalam panel Cloud Shell, jalankan elemen berikut untuk membuat daftar grup Azure AD (daftar harus mencakup grup Service Desk, Admin Senior, dan Admin Junior):

    ```cli
    az ad group list -o table
    ```

3. Di sesi Bash dalam panel Cloud Shell, jalankan perintah berikut untuk mendapatkan referensi ke akun pengguna Dylan Williams: 

    ```cli
    USER=$(az ad user list --filter "displayname eq 'Dylan Williams'")
    ```

4. Di sesi Bash dalam panel Cloud Shell, jalankan perintah berikut untuk mendapatkan properti objectId dari akun pengguna Dylan Williams: 

    ```cli
    OBJECTID=$(echo $USER | jq '.[].objectId' | tr -d '"')
    ```

5. Di sesi Bash dalam panel Cloud Shell, jalankan elemen berikut ini untuk menambahkan akun pengguna Dylan ke grup Service Desk: 

    ```cli
    az ad group member add --group "Service Desk" --member-id $OBJECTID
    ```

6. Di sesi Bash dalam panel Cloud Shell, jalankan elemen berikut ini untuk mencantumkan anggota grup Service Desk dan memverifikasi bahwa ini termasuk akun pengguna Dylan:

    ```cli
    az ad group member list --group "Service Desk"
    ```

7. Tutup panel Cloud Shell.

> Hasil: Menggunakan Azure CLI Anda buatkan akun pengguna dan grup, dan menambahkan akun pengguna ke grup. 


### <a name="exercise-4-assign-the-virtual-machine-contributor-role-to-the-service-desk-group"></a>Latihan 4: Menetapkan peran Kontributor Komputer Virtual ke grup Service Desk.

#### <a name="estimated-timing-10-minutes"></a>Perkiraan waktu: 10 menit

Dalam latihan ini, Anda akan menyelesaikan tugas-tugas berikut:

- Tugas 1: Membuat grup sumber daya. 
- Tugas 2: Menetapkan izin Kontributor Komputer Virtual Service Desk ke grup sumber daya.  

#### <a name="task-1-create-a-resource-group"></a>Tugas 1: Buat grup sumber daya

1. Di portal Microsoft Azure, di kotak teks **Cari sumber daya, layanan, dan dokumen** di bagian atas halaman portal Microsoft Azure, ketik **Grup sumber daya** dan tekan tombol **Enter**.

2. Pada bilah **Grup sumber daya**, klik **+ Buat** dan tentukan pengaturan berikut:

   |Pengaturan|Nilai|
   |---|---|
   |Nama langganan|nama langganan Azure Anda|
   |Nama grup sumber daya|**AZ500Lab01**|
   |Lokasi|**US Timur**|

3. Pilih **Tinjau + Buat**, lalu pilih **Buat**.

   >**Catatan**: Tunggu hingga grup sumber daya diterapkan. Gunakan ikon **Pemberitahuan** (kanan atas) untuk melacak kemajuan status penerapan.

4. Kembali ke bilah **Grup sumber daya**, segarkan laman dan verifikasi bahwa grup sumber daya baru Anda muncul dalam daftar grup sumber daya.


#### <a name="task-2-assign-the-service-desk-virtual-machine-contributor-permissions"></a>Tugas 2: Tetapkan izin Kontributor Komputer Virtual Service Desk. 

1. Pada bilah **Grup sumber daya**, klik entri grup sumber daya **AZ500LAB01**.

2. Pada bilah **AZ500Lab01**, klik **Access control (IAM)** di panel tengah.

3. Pada bilah **AZ500Lab01 \| Kontrol akses (IAM)** , klik **+ Tambahkan** lalu, di menu menurun, klik **Tambahkan penetapan peran**.

4. Pada bilah **Tambahkan penetapan peran**, tentukan pengaturan berikut dan klik **Berikutnya** setelah setiap langkah:

   |Pengaturan|Nilai|
   |---|---|
   |Peran di tab pencarian|**Kontributor Komputer Virtual**|
   |Tetapkan akses ke (Di Bawah Panel Anggota)|**Pengguna, grup, atau perwakilan layanan**|
   |Pilih (+Pilih Anggota)|**Meja pelayanan**|

5. Klik **Tinjau + tetapkan** dua kali untuk membuat penetapan peran.

6. Dari panel **Kontrol akses (IAM)** , pilih **Penetapan peran**.

7. Pada bilah **AZ500Lab01 \| Kontrol akses (IAM)** , pada tab **Periksa akses**, dalam kotak teks **Telusuri menurut nama atau alamat email**, ketik **Dylan Williams**.

8. Dalam daftar hasil penelusuran, pilih akun pengguna Dylan Williams dan, pada panel **penugasan Dylan Williams - AZ500Lab01**, lihat tugas yang baru dibuat.

9. Tutup bilah **tugas Dylan Williams - AZ500Lab01**.

10. Ulangi dua langkah terakhir yang sama untuk memeriksa akses **Joseph Price**. 

> Hasil: Anda telah menetapkan dan memeriksa izin RBAC. 

**Membersihkan sumber daya**

> Jangan lupa untuk menghapus sumber daya Azure yang baru dibuat dan yang tidak diperlukan lagi. Menghapus sumber daya yang tidak terpakai memastikan Anda tidak akan dikenakan biaya tidak terduga.

1. Di portal Microsoft Azure, buka Cloud Shell dengan mengklik ikon pertama di kanan atas Portal Microsoft Azure. 

2. Pada menu drop-down di sudut kiri atas panel Cloud Shell, pilih **PowerShell**, dan, jika diminta, klik **Konfirmasi**. 

3. Di sesi PowerShell dalam panel Cloud Shell, jalankan perintah berikut untuk menghapus grup sumber daya yang Anda buat di lab ini:
  
    ```
    Remove-AzResourceGroup -Name "AZ500LAB01" -Force -AsJob
    ```

4.  Tutup panel **Cloud Shell**. 
