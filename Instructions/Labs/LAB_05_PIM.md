---
lab:
  title: 05 - Azure AD Privileged Identity Management
  module: Module 01 - Manage Identity and Access
ms.openlocfilehash: 6ef7c51d334587e5e4e7116194fa46f2eb5d1df0
ms.sourcegitcommit: 1da29a6d959a7f91dbbcbabf5ec06869c98fc1f1
ms.translationtype: HT
ms.contentlocale: id-ID
ms.lasthandoff: 03/30/2022
ms.locfileid: "145195926"
---
# <a name="lab-05-azure-ad-privileged-identity-management"></a>Lab 05: Azure AD Privileged Identity Management
# <a name="student-lab-manual"></a>Panduan lab siswa

## <a name="lab-scenario"></a>Skenario lab

Anda telah diminta untuk membuat bukti konsep yang menggunakan Azure Privileged Identity Management (PIM) untuk mengaktifkan administrasi just-in-time dan mengontrol jumlah pengguna yang dapat melakukan operasi hak istimewa. Persyaratan khususnya adalah:

- Membuat penetapan permanen pengguna Azure AD aaduser2 ke peran Administrator Keamanan. 
- Mengonfigurasi pengguna Azure AD aaduser2 agar memenuhi syarat untuk peran Administrator Penagihan dan Pembaca Global.
- Mengonfigurasi aktivasi peran Pembaca Global untuk memerlukan persetujuan pengguna Azure AD aaduser3.
- Mengonfigurasi tinjauan akses peran Pembaca Global dan meninjau kemampuan audit.

> Untuk semua sumber daya di lab ini, kita menggunakan wilayah **US Timur**. Verifikasi dengan instruktur Anda bahwa ini adalah wilayah yang akan digunakan untuk kelas. 

> Sebelum melanjutkan, pastikan Anda telah menyelesaikan Lab 04: MFA, Akses Bersyarat, dan Identity Protection AAD. Anda akan memerlukan penyewa Azure AD, AdatumLab500-04, dan akun pengguna aaduser1, aaduser2, dan aaduser3.

## <a name="lab-objectives"></a>Tujuan lab

Di lab ini, Anda akan menyelesaikan latihan berikut:

- Latihan 1: Mengonfigurasi pengguna dan peran PIM.
- Latihan 2: Mengaktifkan peran PIM dengan dan tanpa persetujuan.
- Latihan 3: Membuat Tinjauan Akses dan meninjau fitur audit PIM.

## <a name="azure-ad-privileged-identity-management-diagram"></a>Diagram Azure Active Directory Privileged Identity Management

![gambar](https://user-images.githubusercontent.com/91347931/157522920-264ce57e-5c55-4a9d-8f35-e046e1a1e219.png)

## <a name="instructions"></a>Instruksi

### <a name="exercise-1---configure-pim-users-and-roles"></a>Latihan 1 - Mengonfigurasi pengguna dan peran PIM

#### <a name="estimated-timing-15-minutes"></a>Perkiraan waktu: 15 menit

Dalam latihan ini, Anda akan menyelesaikan tugas-tugas berikut:

- Tugas 1: Membuat pengguna agar memenuhi syarat untuk suatu peran.
- Tugas 2: Mengonfigurasikan peran untuk memerlukan persetujuan guna mengaktifkan dan menambahkan anggota yang memenuhi syarat.
- Tugas 3: Memberikan tugas permanen kepada sebuah peran. 

#### <a name="task-1-make-a-user-eligible-for-a-role"></a>Tugas 1: Membuat pengguna agar memenuhi syarat untuk peran

Dalam tugas ini, Anda akan membuat pengguna agar memenuhi syarat untuk peran direktori Azure AD.

1. Masuk ke portal Microsoft Azure **`https://portal.azure.com/`** .

    >**Catatan**: Pastikan Anda masuk ke penyewa **AdatumLab500-04** Azure AD. Anda dapat menggunakan filter **Direktori + langganan** untuk beralih di antara penyewa Azure AD. Pastikan Anda masuk sebagai pengguna dengan peran Administrator Global.
    
    >**Catatan**: Jika Anda masih belum melihat entri AdatumLab500-04, klik tautan Beralih Direktori, pilih baris AdatumLab500-04 dan klik tombol Switch.

2. Di portal Microsoft Azure, dalam kotak teks **Pencarian sumber daya, layanan, dan dokumen** di bagian atas halaman portal Microsoft Azure, ketikkan **Azure AD Privileged Identity Management** dan tekan tombol **Enter**.

3. Pada bilah **Privileged Identity Management**, di bagian **Kelola**, klik **Peran Azure AD**.

4. Pada bilah **AdatumLab500-04 \| Quick start**, di bagian **Kelola**, klik **Peran**.

5. Pada bilah **AdatumLab500-04 \| Peran**, klik **+ Tambahkan tugas**.

6. Pada bilah **Tambahkan tugas**, pada dropdown **Pilih peran**, pilih **Administrator Penagihan**.

7. Klik tautan **Tidak ada anggota yang dipilih**, pada bilah **Pilih anggota**, klik **aaduser2**, lalu klik **Pilih**.

8. Kembali ke bilah **Tambahkan tugas**, klik **Berikutnya**. 

9. Pastikan **Jenis tugas** diatur menjadi **Memenuhi syarat** dan klik **Tetapkan**.
 
10. Kembali ke bilah **AdatumLab500-04 \| Peran**, di bagian **Kelola**, klik **Tugas**.

11. Kembali ke bilah **AdatumLab500-04 \| Tugas**, perhatikan tab untuk **Tugas yang memenuhi syarat**, **Tugas aktif**, dan **Tugas yang kedaluwarsa**.

12. Pastikan pada tab **Tugas yang memenuhi syarat** bahwa **aaduser2** ditampilkan sebagai **Administrator penagihan**. 

    >**Catatan**: Selama proses masuk, aaduser2 akan memenuhi syarat untuk menggunakan peran administrator Penagihan. 

#### <a name="task-2-configure-a-role-to-require-approval-to-activate-and-add-an-eligible-member"></a>Tugas 2: Konfigurasikan peran untuk meminta persetujuan untuk mengaktifkan dan menambahkan anggota yang memenuhi syarat

1. Di Portal Azure, navigasikan kembali ke bilah **Privileged Identity Management** dan klik **Peran Azure AD**.

2. Pada bilah **AdatumLab500-04 \| Quick start**, di bagian **Kelola**, klik **Peran**.

3. Pada bilah **AdatumLab500-04 \| Peran**, klik entri peran **Pembaca global**. 

4. Pada bilah **Pembaca Global \| Tugas**, klik ikon **Pengaturan** di toolbar bilah dan tinjau pengaturan konfigurasi untuk peran tersebut, termasuk persyaratan Autentikasi Multi-Faktor Azure.

5. Klik **Edit**.

6. Pada tab **Aktivasi**, aktifkan kotak centang **Memerlukan persetujuan untuk mengaktifkan**.

7. Klik **Pilih pemberi persetujuan**, pada bilah **Pilih anggota**, klik **aaduser3**, lalu klik **Pilih**.

8. Klik **Berikutnya:Tugas**.

9. Kosongkan kotak centang **Izinkan penugasan yang memenuhi syarat secara permanen**, biarkan semua pengaturan lainnya dengan nilai defaultnya.

10. Klik **Berikutnya:Pemberitahuan**.

11. Tinjau pengaturan **Pemberitahuan**, biarkan semuanya diatur secara default, lalu klik **Perbarui**.

    >**Catatan**: Siapa pun yang mencoba menggunakan peran Pembaca Global sekarang memerlukan persetujuan dari aaduser3. 

12. Pada bilah **Pembacaan Global \| Tugas**, klik **+ Tambahkan tugas**.

13. Pada bilah **Tambahkan tugas**, klik **Tidak ada anggota yang dipilih**, pada bilah **Pilih anggota**, klik **aaduser2**, lalu klik **Pilih**.

14. Klik **Berikutnya**. 

15. Pastikan **Jenis tugas** adalah **Memenuhi syarat** dan tinjau pengaturan durasi yang memenuhi syarat.

16. Klik **Tetapkan**.

    >**Catatan**: Pengguna aaduser2 memenuhi syarat untuk peran Pembaca Global. 
 
#### <a name="task-3-give-a-user-permanent-assignment-to-a-role"></a>Tugas 3: Memberikan tugas permanen kepada sebuah peran.

1. Di Portal Azure, navigasikan kembali ke bilah **Privileged Identity Management** dan klik **Peran Azure AD**.

2. Pada bilah **AdatumLab500-04 \| Quick start**, di bagian **Kelola**, klik **Peran**.

3. Pada bilah **AdatumLab500-04 \| Peran**, klik **+ Tambahkan tugas**.

4. Pada bilah **Tambahkan tugas**, pada dropdown **Pilih peran**, pilih **Administrator Keamanan**.

5. Pada bilah **Tambahkan tugas**, klik **Tidak ada anggota yang dipilih**, pada bilah **Pilih anggota**, klik **aaduser2**, lalu klik **Pilih**.

6. Klik **Berikutnya**. 

7. Tinjau pengaturan **Jenis tugas** dan klik **Tugaskan**.

8. Pada halaman **Tugas** pada tab **Tugas yang Memenuhi Syarat**, pilih **Perbarui** untuk tugas **aaduser2**. Pilih **Memenuhi Syarat Secara Permanen** dan **Simpan**.

    >**Catatan**: Pengguna aaduser2 sekarang memenuhi syarat secara permanen untuk peran Administrator Keamanan.
    
### <a name="exercise-2---activate-pim-roles-with-and-without-approval"></a>Latihan 2 - Mengaktifkan peran PIM dengan dan tanpa persetujuan

#### <a name="estimated-timing-15-minutes"></a>Perkiraan waktu: 15 menit

Dalam latihan ini, Anda akan menyelesaikan tugas-tugas berikut:

- Tugas 1: Mengaktifkan peran yang tidak memerlukan persetujuan. 
- Tugas 2: Mengaktifkan peran yang memerlukan persetujuan. 

#### <a name="task-1-activate-a-role-that-does-not-require-approval"></a>Tugas 1: Mengaktifkan peran yang tidak memerlukan persetujuan.

Dalam tugas ini, Anda akan mengaktifkan peran yang tidak memerlukan persetujuan.

1. Buka jendela browser InPrivate.

2. Di jendela browser InPrivate, navigasikan ke portal Azure dan masuk menggunakan akun pengguna **aaduser2**.

    >**Catatan**: Untuk masuk, Anda harus memberikan nama akun pengguna **aaduser2** yang sepenuhnya memenuhi syarat, termasuk nama domain DNS penyewa Azure AD, yang Anda catat sebelumnya di lab ini. Nama pengguna ini dalam format aaduser2@`<your_tenant_name>`.onmicrosoft.com, ketika `<your_tenant_name>` adalah pengganti yang mewakili nama penyewa Azure AD unik Anda. 

3. Di portal Microsoft Azure, dalam kotak teks **Pencarian sumber daya, layanan, dan dokumen** di bagian atas halaman portal Microsoft Azure, ketikkan **Azure AD Privileged Identity Management** dan tekan tombol **Enter**.

4. Pada bilah **Privileged Identity Management**, di bagian **Tugas**, klik **Peran saya**.

5. Anda akan melihat tiga **Peran yang memenuhi syarat** untuk **aaduser2**: **Pembaca Global**, **Administrator Keamanan**, dan **Administrator Penagihan**. 

6. Pada baris yang menampilkan entri peran **Administrator Penagihan**, klik **Aktifkan**.

7. Jika perlu, klik peringatan **Diperlukan verifikasi tambahan. Klik untuk melanjutkan** dan ikuti petunjuk untuk memverifikasi identitas Anda.

8. Pada bilah **Aktifkan - Administrator Penagihan**, dalam kotak teks **Alasan**, ketik teks yang memberikan alasan untuk aktivasi, lalu klik **Aktifkan**.

    >**Catatan**: Saat Anda mengaktifkan peran di PIM, perlu waktu hingga 10 menit agar aktivasi diterapkan. 
    
    >**Catatan**: Setelah penetapan peran Anda aktif, browser Anda akan di-refresh (Jika terjadi kesalahan, cukup keluar dan masuk kembali ke portal Azure menggunakan akun pengguna **aaduser2**).

9. Navigasikan kembali ke bilah **Privileged Identity Management** dan, di bagian **Tugas**, klik **Peran saya**.

10. Pada bilah **Peran saya \|Peran Azure AD**, alihkan ke tab **Tugas aktif**. Perhatikan bahwa peran **Administrator Penagihan** adalah **Diaktifkan**.

    >**Catatan**: Setelah peran diaktifkan, peran tersebut secara otomatis dinonaktifkan ketika batas waktunya di bawah **Waktu berakhir**(durasi yang memenuhi syarat) tercapai.

    >**Catatan**: Jika Anda menyelesaikan tugas administrator lebih awal, Anda dapat menonaktifkan peran secara manual.

11.  Dalam daftar **Tugas Aktif**, di baris yang mewakili peran Administrator Penagihan, klik tautan **Nonaktifkan**.

12.  Pada bilah **Nonaktifkan - Administrator Penagihan**, klik **Nonaktifkan** lagi untuk mengonfirmasi.


#### <a name="task-2-activate-a-role-that-requires-approval"></a>Tugas 2: Mengaktifkan peran yang memerlukan persetujuan. 

Dalam tugas ini, Anda akan mengaktifkan peran yang memerlukan persetujuan.

1. Di jendela browser InPrivate, di portal Azure, saat masuk sebagai pengguna **aaduser2**, navigasikan kembali ke bilah **Privileged Identity Management \| Mulai cepat**. 

2. Pada bilah **Privileged Identity Management \| Mulai cepat**, di bagian **Tugas**, klik **Peran saya**.

3. Pada bilah **Peran saya \|Peran Azure AD**, dalam daftar **Tugas yang memenuhi syarat**, di baris yang menampilkan peran **Pembaca Global**, klik  **Aktifkan**. 

4. Pada bilah **Aktifkan - Pembaca Global**, di kotak teks **Alasan**, ketik teks yang memberikan alasan untuk aktivasi, lalu klik **Aktifkan**.

5. Klik ikon **Pemberitahuan** di bilah alat portal Azure dan lihat pemberitahuan yang menginformasikan bahwa permintaan Anda menunggu persetujuan.

    >**Catatan**: Sebagai administrator peran Hak Istimewa, Anda dapat meninjau dan membatalkan permintaan kapan saja. 

6. Pada bilah **Peran saya \| Peran Azure AD**, temukan peran **Administrator Keamanan**, dan klik **Aktifkan**. 

7. Klik peringatan **Diperlukan verifikasi tambahan. Klik untuk melanjutkan**. 

8. Ikuti petunjuk untuk memverifikasi identitas Anda.

    >**Catatan**: Anda hanya perlu mengautentikasi sekali per sesi. 

9. Setelah Anda kembali ke antarmuka Portal Azure, pada bilah **Aktifkan - Administrator Keamanan**, di kotak teks **Alasan**, ketik teks yang memberikan alasan untuk aktivasi, lalu klik **Aktifkan**.

    >**Catatan**: Proses persetujuan otomatis harus selesai.

10. Kembali ke bilah **Peran saya \| Peran Azure AD**, klik tab **Penetapan aktif** dan perhatikan bahwa daftar **tugas aktif** menyertakan **Administrator Keamanan**  tetapi bukan peran **Pembaca Global**.

    >**Catatan**: Anda sekarang akan menyetujui peran Pembaca Global.

11. Keluar dari portal Azure sebagai **aaduser2**.

12. Masuk ke portal Azure sebagai **aaduser3**.

    >**Catatan**: Jika Anda mengalami masalah dengan autentikasi menggunakan salah satu akun pengguna, Anda bisa masuk ke penyewa Azure AD menggunakan akun pengguna Anda untuk mengatur ulang kata sandi masing-masing pengguna atau mengonfigurasi ulang opsi masuk pengguna tersebut.

13. Di portal Microsoft Azure, navigasikan ke **Azure AD Privileged Identity Management** (Di kotak teks Pencarian sumber daya, layanan, dan dokumen di bagian atas halaman portal Microsoft Azure, ketik Azure AD Privileged Identity Management dan tekan tombol Enter).

14. Pada bilah **Privileged Identity Management \| Mulai cepat**, di bagian **Tugas**, klik **Setujui permintaan**.

15. Pada bilah **Setujui permintaan \| peran Azure AD**, di bagian **Permintaan untuk aktivasi peran**, centang kotak untuk entri yang mewakili permintaan aktivasi peran ke **Global Reader**  peran oleh **aaduser2**.

16. Klik **Setujui**. Pada bilah **Setujui Permintaan**, di kotak teks **Pertimbangan**, ketik alasan aktivasi, catat waktu mulai dan berakhir, lalu klik **Konfirmasi**. 

    >**Catatan**: Anda juga memiliki opsi untuk menolak permintaan.

17. Keluar dari portal Azure sebagai **aaduser3**.

18. Masuk ke portal Azure sebagai **aaduser2**

19. Di portal Microsoft Azure, navigasikan ke **Azure AD Privileged Identity Management** (Di kotak teks Pencarian sumber daya, layanan, dan dokumen di bagian atas halaman portal Microsoft Azure, ketik Azure AD Privileged Identity Management dan tekan tombol Enter).

20. Pada bilah **Privileged Identity Management \| Mulai cepat**, di bagian **Tugas**, klik **Peran saya**.

21. Pada bilah **Peran saya \|Peran Azure AD**, klik tab **Tugas Aktif** dan pastikan bahwa peran Pembaca Global sekarang aktif.

    >**Catatan**: Anda mungkin harus me-refresh halaman untuk melihat daftar tugas aktif yang diperbarui.

22. Keluar dan tutup jendela browser InPrivate.

> Hasil: Anda telah berlatih mengaktifkan peran PIM dengan dan tanpa persetujuan. 

### <a name="exercise-3---create-an-access-review-and-review-pim-auditing-features"></a>Latihan 3 - Membuat Tinjauan Akses dan meninjau fitur audit PIM

#### <a name="estimated-timing-10-minutes"></a>Perkiraan waktu: 10 menit

Dalam latihan ini, Anda akan menyelesaikan tugas-tugas berikut:

- Tugas 1: Mengonfigurasikan peringatan keamanan untuk peran direktori Azure AD di PIM
- Tugas 2: Meninjau peringatan PIM, informasi ringkasan, dan informasi audit terperinci

#### <a name="task-1-configure-security-alerts-for-azure-ad-directory-roles-in-pim"></a>Tugas 1: Mengonfigurasikan peringatan keamanan untuk peran direktori Azure AD di PIM

Dalam tugas ini, Anda akan mengurangi risiko yang terkait dengan penetapan peran "kedaluwarsa". Anda akan melakukannya dengan membuat tinjauan akses PIM untuk memastikan bahwa peran yang ditetapkan masih valid. Secara khusus, Anda akan meninjau peran Pembaca Global. 

1. Masuk ke portal Azure **`https://portal.azure.com/`** menggunakan akun Anda.

    >**Catatan**: Pastikan Anda masuk ke penyewa **AdatumLab500-04** Azure AD. Anda dapat menggunakan filter **Direktori + langganan** untuk beralih di antara penyewa Azure AD. Pastikan Anda masuk sebagai pengguna dengan peran Administrator Global.
    
    >**Catatan**: Jika Anda masih belum melihat entri AdatumLab500-04, klik tautan Beralih Direktori, pilih baris AdatumLab500-04 dan klik tombol Switch.

2. Di portal Microsoft Azure, dalam kotak teks **Pencarian sumber daya, layanan, dan dokumen** di bagian atas halaman portal Microsoft Azure, ketikkan **Azure AD Privileged Identity Management** dan tekan tombol **Enter**.

3. Navigasikan ke bilah **Privileged Identity Management**. 

4. Pada bilah **Privileged Identity Management \| Mulai cepat**, di bagian **Kelola**, klik **Peran Azure AD**.

5. Pada bilah **AdatumLab500-04 \| Mulai cepat**, di bagian **Kelola**, klik **Tinjauan akses**.

6. Pada bilah **AdatumLab500-04 \| Tinjauan akses**, klik **Baru**:

7. Pada bilah **Buat tinjauan akses**, tentukan pengaturan berikut (biarkan yang lain dengan nilai defaultnya): 

   |Pengaturan|Nilai|
   |---|---|
   |Nama Tinjauan|**Tinjauan Pembaca Global**|
   |Tanggal Mulai|tanggal hari ini| 
   |Frekuensi|**Satu kali**|
   |Tanggal Akhir|akhir bulan ini|
   |Peran, Pilih Peran Istimewa|**Pembaca Global**|
   |Peninjau|**Pengguna terpilih**|
   |Pilih pengulas|akun Anda|

8. Pada bilah **Buat tinjauan akses**, klik **Mulai**:
 
    >**Catatan**: Perlu waktu sekitar satu menit agar tinjauan diterapkan dan muncul di bilah **AdatumLab500-04 \| Tinjauan akses**. Anda mungkin harus me-refresh halaman web. Status peninjauan akan menjadi **Aktif**. 

9. Pada bilah **AdatumLab500-04 \| Tinjauan akses**, pada header **Tinjauan Pembaca Global**, klik entri **Pembaca Global**. 

10. Pada bilah **Tinjauan Pembaca Global**, periksa halaman **Ringkasan** dan perhatikan bahwa diagram **Progres** menunjukkan satu pengguna dalam kategori **Tidak ditinjau** . 

11. Pada bilah **Tinjauan Pembaca Global**, di bagian **Kelola**, klik **Hasil**. Perhatikan bahwa aaduser2 terdaftar memiliki akses ke peran ini.

12. Klik **lihat** pada baris **aaduser2** untuk melihat log audit mendetail dengan entri yang mewakili aktivitas PIM yang melibatkan pengguna tersebut.

13. Navigasikan kembali ke bilah **AdatumLab500-04 \| Tinjauan akses**.

14. Pada bilah **AdatumLab500-04 \| Tinjauan akses**, di bagian **Tugas**, klik **Tinjau akses** lalu, klik entri **Tinjauan Pembaca Global** . 

15. Pada bilah **Tinjauan Pembaca Global**, klik entri **aaduser2**. 

16. Di kotak teks **Alasan**, ketikkan alasan untuk persetujuan, lalu klik **Setuju** untuk mempertahankan keanggotaan peran saat ini atau **Tolak** untuk mencabutnya. 

17. Navigasikan kembali ke bilah **Privileged Identity Management** dan, di bagian **Kelola**, klik **Peran Azure AD**.

18. Pada bilah **AdatumLab500-04 \| Mulai cepat**, di bagian **Kelola**, klik **Tinjauan akses**.

19. Pilih entri yang mewakili tinjauan **Pembaca Global**. Perhatikan bahwa bagan **Kemajuan** telah diperbarui untuk menampilkan tinjauan Anda. 

#### <a name="task-2-review-pim-alerts-summary-information-and-detailed-audit-information"></a>Tugas 2: Tinjau peringatan PIM, informasi ringkasan, dan informasi audit terperinci. 

Dalam tugas ini, Anda akan meninjau peringatan PIM, informasi ringkasan, dan informasi audit terperinci. 

1. Navigasikan kembali ke bilah **Privileged Identity Management** dan, di bagian **Kelola**, klik **Peran Azure AD**.

2. Pada bilah **AdatumLab500-04 \| Mulai cepat**, di bagian **Kelola**, klik **Peringatan**, lalu klik **Pengaturan**.

3. Pada bilah **Pengaturan peringatan**, tinjau peringatan dan tingkat risiko yang telah dikonfigurasi sebelumnya. Klik salah satu dari peringatan untuk mengetahui informasi mendetail. 

4. Kembali ke bilah **AdatumLab500-04 \| Mulai cepat** dan klik **Ringkasan**. 

5. Pada bilah **AdatumLab500-04 \| Ringkasan**, tinjau informasi ringkasan tentang aktivasi peran, aktivitas PIM, peringatan, dan penetapan peran.

6. Pada bilah **AdatumLab500-04 \| Ringkasan**, di bagian **Aktivitas**, klik **Audit sumber daya**. 

    >**Catatan**: Riwayat audit tersedia untuk semua peran penetapan dan aktivasi yang istimewa dalam 30 hari terakhir.

7. Perhatikan bahwa Anda dapat mengambil informasi mendetail, termasuk **Waktu**, **Pemohon**, **Tindakan**, **Nama sumber daya**, **Cakupan**, **Target Utama**, dan **Subjek**. 

> Hasil: Anda telah mengonfigurasi tinjauan akses dan meninjau informasi audit. 

**Membersihkan sumber daya**

> Jangan lupa untuk menghapus sumber daya Azure yang baru dibuat dan yang tidak diperlukan lagi. Menghapus sumber daya yang tidak terpakai memastikan Anda tidak akan dikenakan biaya tak terduga. 

1. Di portal Azure, atur filter **Direktori + langganan** ke penyewa Azure AD yang terkait dengan langganan Azure tempat Anda menyebarkan Azure VM **az500-04-vm1**.

    >**Catatan**: Jika Anda tidak melihat entri penyewa Azure AD utama Anda, klik tautan Beralih Direktori, pilih baris penyewa utama Anda dan klik tombol Beralih.

2. Di portal Microsoft Azure, buka Cloud Shell dengan mengklik ikon pertama di kanan atas Portal Azure. Jika diminta, klik **PowerShell** dan **Buat penyimpanan**.

3. Pastikan **PowerShell** dipilih di menu dropdown di sudut kiri atas panel Cloud Shell.

4. Di sesi PowerShell dalam panel Cloud Shell, jalankan perintah berikut untuk menghapus grup sumber daya yang Anda buat di lab sebelumnya:
  
    ```powershell
    Remove-AzResourceGroup -Name "AZ500LAB04" -Force -AsJob
    ```

5. Tutup panel **Cloud Shell**. 

6. Kembali ke portal Azure, gunakan filter **Direktori + langganan** untuk beralih ke penyewa **AdatumLab500-04** Azure Active Directory.

7. Navigasikan ke bilah **AdatumLab500-04 Azure Active Directory** dan, di bagian **Kelola**, klik **Lisensi**.

8. Di **Lisensi** | Bilah ringkasan, klik **Semua produk**, centang kotak **Azure Active Directory Premium P2** dan klik untuk membukanya.

    >**Catatan**: Di Lab 4 - Latihan 2 - Tugas 4 **Menetapkan lisensi Azure AD Premium P2 kepada pengguna Azure AD** adalah tugas menetapkan Lisensi Premium kepada pengguna **aaduser1, aaduser2, dan aaduser3**, pastikan kita menghapus lisensi tersebut dari pengguna yang ditetapkan

9. Pada bilah **Azure Active Directory Premium P2 - Pengguna berlisensi**, beri centang kotak akun pengguna yang Anda tetapkan lisensi **Azure Active Directory Premium P2**. Klik **Hapus lisensi** dari panel atas dan saat diminta untuk mengonfirmasi, pilih **Ya**.

10. Di portal Azure, navigasikan ke bilah **Pengguna - Semua pengguna**, klik entri yang mewakili akun pengguna **aaduser1**, pada bilah **aaduser1 - Profil** klik **Hapus**, dan saat diminta untuk mengonfirmasi, pilih **Ya**.

11. Ulangi urutan langkah yang sama untuk menghapus akun pengguna yang tersisa yang Anda buat.

12. Navigasikan ke bilah **AdatumLab500-04 - Tinjauan** penyewa Azure AD, pilih **Kelola penyewa** lalu di layar berikutnya, beri centang kotak di samping **AdatumLab500-04** dan pilih **Hapus**. Pada bilah **Hapus penyewa 'AdatumLab500-04'** , pilih tautan **Dapatkan izin untuk menghapus sumber daya Azure**, pada bilah **Properti** Azure Active Directory, atur **Pengelolaan akses untuk sumber daya Azure** menjadi **Ya** dan pilih **Simpan**.

13. Keluar dari portal Azure dan masuk kembali. 

14. Navigasikan kembali ke bilah **Hapus direktori 'AdatumLab500-04'** dan klik **Hapus**.

    >**Catatan**: Tetap tidak bisa menghapus penyewa dan muncul kesalahan **Hapus semua langganan dan berbasis lisensi**, maka hal itu mungkin karena ada langganan yang sudah ditautkan ke penyewa. Di sini **Lisensi P2 Premium Gratis** dapat menimbulkan kesalahan validasi. Menghapus langganan uji coba Lisensi P2 Premium menggunakan ID admin Global dari admin M365>> **Produk Anda** dan dari portal **Business Store** akan menyelesaikan masalah ini. Perhatikan juga bahwa menghapus penyewa membutuhkan lebih banyak waktu. Periksa Tanggal akhir langganan, setelah akhir periode uji coba, kunjungi kembali direktori Azure Active, lalu coba hapus penyewa.    

> Untuk informasi tambahan apa pun mengenai tugas ini, lihat [https://docs.microsoft.com/en-us/azure/active-directory/users-groups-roles/directory-delete-howto](https://docs.microsoft.com/en-us/azure/active-directory/users-groups-roles/directory-delete-howto)
