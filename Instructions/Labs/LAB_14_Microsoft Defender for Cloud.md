---
lab:
  title: 14 - Microsoft Defender untuk Cloud
  module: Module 04 - Microsoft Defender for Cloud
ms.openlocfilehash: 647e2dc79012d6fedca9da9a78f6006f64be093b
ms.sourcegitcommit: 18d4f5ccc60ae6d43b27e8b7d4d3ef7f68a02e93
ms.translationtype: HT
ms.contentlocale: id-ID
ms.lasthandoff: 04/06/2022
ms.locfileid: "145195956"
---
# <a name="lab-14-microsoft-defender-for-cloud"></a>Lab 14: Microsoft Defender untuk Cloud
# <a name="student-lab-manual"></a>Panduan lab siswa

## <a name="lab-scenario"></a>Skenario lab

Anda telah diminta untuk membuat bukti konsep Microsoft Defender untuk lingkungan berbasis Cloud. Secara khusus, Anda ingin:

- Konfigurasikan Microsoft Defender untuk Cloud untuk memantau mesin virtual.
- Tinjau rekomendasi Microsoft Defender untuk Cloud untuk mesin virtual.
- Terapkan rekomendasi untuk konfigurasi tamu dan akses VM tepat waktu. 
- Tinjau bagaimana Skor Aman dapat digunakan untuk menentukan kemajuan dalam menciptakan infrastruktur yang lebih aman.

> Untuk semua sumber daya di lab ini, kita menggunakan wilayah **US Timur**. Verifikasi dengan instruktur Anda bahwa ini adalah wilayah yang akan digunakan untuk kelas. 

## <a name="lab-objectives"></a>Tujuan lab

Di lab ini, Anda akan menyelesaikan latihan berikut:

- Latihan 1: Menerapkan Pertahanan Microsoft untuk Cloud

## <a name="microsoft-defender-for-cloud-diagram"></a>Diagram Microsoft Defender untuk Cloud

![gambar](https://user-images.githubusercontent.com/91347931/157537800-94a64b6e-026c-41b2-970e-f8554ce1e0ab.png)

## <a name="instructions"></a>Instruksi

### <a name="exercise-1-implement-microsoft-defender-for-cloud"></a>Latihan 1: Menerapkan Pertahanan Microsoft untuk Cloud

Dalam latihan ini, Anda akan menyelesaikan tugas-tugas berikut:

- Tugas 1: Mengonfigurasi Pertahanan Microsoft untuk Cloud
- Tugas 2: Tinjau rekomendasi Microsoft Defender untuk Cloud
- Tugas 3: Terapkan rekomendasi Microsoft Defender untuk Cloud untuk mengaktifkan Akses VM Tepat Waktu

#### <a name="task-1-configure-microsoft-defender-for-cloud"></a>Tugas 1: Mengonfigurasi Pertahanan Microsoft untuk Cloud

Dalam tugas ini, Anda akan mengaktifkan dan mengonfigurasi Microsoft Defender untuk Cloud.

1. Masuk ke portal Microsoft Azure **`https://portal.azure.com/`** .

    >**Catatan**: Masuk ke portal Microsoft Azure menggunakan akun yang memiliki peran Pemilik atau Kontributor dalam langganan Azure yang Anda gunakan untuk lab ini.

2. Di portal Azure, di kotak teks **Sumber daya pencarian, layanan, dan dokumen** di bagian atas halaman portal Azure, ketik **Microsoft Defender untuk Cloud** dan tekan kunci **Enter**.

3. Jika belum selesai sebelumnya, di bilah **Microsoft Defender untuk Cloud | Memulai**, klik **Tingkatkan**.
     
4. Jika belum selesai sebelumnya, di bilah **Microsoft Defender untuk Cloud | Memulai**, di tab **Instal agen**, gulir ke bawah dan klik **Instal agen**.

5. Di bilah **Microsoft Defender untuk Cloud | Memulai**, pada tab **Tingkatkan versi** >> di bagian **Pilih ruang kerja dengan fitur keamanan yang disempurnakan** >> aktifkan **paket Microsoft Defender** dengan memilih Ruang Kerja Analisis Log Anda. 

    >**Catatan**: Tinjau semua fitur yang tersedia sebagai bagian dari paket Microsoft Defender. 

6. Navigasikan ke **Microsoft Defender untuk Cloud** dan klik **Pengaturan Lingkungan** di bawah Pengaturan Manajemen, di bilah menu vertikal di sisi kiri.

7. Di bilah **Microsoft Defender untuk Cloud | Pengaturan Lingkungan**, klik langganan yang relevan. 

8. Pada bilah **Paket pertahanan**, pilih **Aktifkan semua Paket Microsoft Defender untuk Cloud** dan klik **Simpan**.

9. Pada **Pengaturan | Paket Pertahanan**, pada menu vertikal di sisi kiri, klik **Penyediaan otomatis**. 

10. Pada bilah **Pengaturan | Penyediaan otomatis**, pastikan Penyediaan otomatis diatur ke **Aktif** untuk item pertama **Log Analytics agent for Azure VMs**.

11. Pada bilah **Pengaturan | Otomatisasi alur kerja**, tinjau pengaturan yang tersedia. 

    >**Catatan**: Anda dapat memicu peringatan deteksi ancaman berbasis tindakan dan rekomendasi Microsoft Defender untuk Cloud. Anda juga dapat mengonfigurasi tindakan berdasarkan aplikasi Logika. 
    
12. Pada bilah **Tambahkan otomatisasi alur kerja**, tinjau pengaturan yang tersedia.

    >**Catatan**: Microsoft Defender untuk Cloud memberikan banyak wawasan tentang mesin virtual termasuk status pembaruan sistem, konfigurasi keamanan OS, dan perlindungan titik akhir.

13. Pada bilah **Tambahkan otomatisasi alur kerja**, klik **Batal**.

14. Navigasikan kembali ke bilah **Microsoft Defender untuk Cloud | Pengaturan Lingkungan**, perluas langganan Anda, dan klik entri yang mewakili ruang kerja Analisis Log yang Anda buat di lab sebelumnya.

15. Pada bilah **Pengaturan | Paket pertahanan**, pastikan bahwa **Aktifkan semua paket Microsoft Defender untuk Cloud** dipilih dan klik **Simpan**.

16. Pilih **Pengumpulan data** dari bilah **Microsoft Defender untuk Cloud | Pengaturan**. Pilih **Semua Acara** dan **Simpan**.


#### <a name="task-2-review-the-microsoft-defender-for-cloud-recommendation"></a>Tugas 2: Tinjau rekomendasi Microsoft Defender untuk Cloud

Dalam tugas ini, Anda akan meninjau rekomendasi Microsoft Defender untuk Cloud. 

1. Di portal Azure, navigasikan kembali ke bilah **Microsoft Defender untuk Cloud | Gambaran Umum**. 

2. Di bilah **Microsoft Defender untuk Cloud | Gambaran Umum**, tinjau petak peta **Skor Aman**.

    >**Catatan**: Catat skor saat ini jika tersedia.

3. Navigasikan kembali ke bilah **Microsoft Defender untuk Cloud | Gambaran Umum**, pilih **Sumber daya yang dinilai**.

4. Pada bilah **Inventaris**, pilih entri **myVM**.

    >**Catatan**: Anda mungkin harus menunggu beberapa menit dan menyegarkan halaman browser agar entri muncul.
    
5. Pada bilah **Kesehatan sumber daya**, pada tab **Rekomendasi**, tinjau daftar rekomendasi untuk **myVM**.


#### <a name="task-3-implement-the-microsoft-defender-for-cloud-recommendation-to-enable-just-in-time-vm-access"></a>Tugas 3: Terapkan rekomendasi Microsoft Defender untuk Cloud untuk mengaktifkan Akses VM Tepat Waktu

Dalam tugas ini, Anda akan menerapkan rekomendasi Microsoft Defender untuk Cloud untuk mengaktifkan Akses VM Tepat Waktu di mesin virtual. 

1. Di portal Azure, navigasikan kembali ke bilah **Microsoft Defender untuk Cloud | Gambaran Umum** dan pilih **Perlindungan beban kerja** di bawah petak peta **Keamanan Cloud**.

2. Pada bilah **Perlindungan beban kerja**, di bagian **Perlindungan lanjutan**, klik petak peta **Akses VM tepat waktu** dan, pada bilah **Akses VM tepat waktu** , klik **Coba akses VM** .

    >**Catatan**: Jika VM tidak terdaftar, navigasikan ke bilah **Mesin Virtual** dan klik **Konfigurasi**, Klik opsi **Aktifkan VM Tepat Waktu** di bawah **Akses Vm tepat waktu**. Ulangi langkah di atas untuk menavigasi kembali ke **Microsoft Defender untuk Cloud** dan menyegarkan halaman, VM akan muncul.

3. Pada **Akses VM tepat waktu**, pilih **Tidak Dikonfigurasi**, lalu klik entri **myVM**.

    >**Catatan**: Anda mungkin harus menunggu beberapa menit sebelum entri **myVM** tersedia.

4. Pilih **Aktifkan JIT pada 1 VM**.

5. Pada bilah **Konfigurasi akses VM JIT**, di ujung kanan baris yang merujuk pada port **22**, klik tombol elipsis, lalu klik **Hapus**.

6. Pada panel **Konfigurasi akses VM JIT**, klik **Simpan**.

    >**Catatan**: Pantau kemajuan konfigurasi dengan mengeklik ikon **Pemberitahuan** di bilah alat dan melihat bilah **Pemberitahuan**. 

    >**Catatan**: Perlu beberapa waktu agar penerapan rekomendasi di lab ini dapat dicerminkan oleh Skor Aman. Periksa Skor Aman secara berkala untuk menentukan dampak penerapan fitur ini. 

> Hasil: Anda telah mengaktifkan Microsoft Defender untuk Cloud dan menerapkan rekomendasi mesin virtual. 

    >**Note**: Do not remove the resources from this lab as they are needed for the Azure Sentinel lab.
