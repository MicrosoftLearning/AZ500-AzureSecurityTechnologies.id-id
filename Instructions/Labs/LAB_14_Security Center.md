---
lab:
  title: 14 - Microsoft Defender untuk Cloud
  module: Module 04 - Microsoft Defender for Cloud
ms.openlocfilehash: 6ec274b75692321577c8966e07349211209eaa02
ms.sourcegitcommit: a8470295248a6363987bd5ea47154fe39f8535c3
ms.translationtype: HT
ms.contentlocale: id-ID
ms.lasthandoff: 03/09/2022
ms.locfileid: "145195947"
---
# <a name="lab-14-microsoft-defender-for-cloud"></a>Lab 14: Microsoft Defender untuk Cloud
# <a name="student-lab-manual"></a>Panduan lab siswa

## <a name="lab-scenario"></a>Skenario lab

Anda telah diminta untuk membuat bukti konsep Microsoft Defender untuk lingkungan berbasis Cloud. Secara khusus, Anda ingin:

- Konfigurasikan Microsoft Defender for Cloud untuk memantau mesin virtual.
- Mengulas rekomendasi Microsoft Defender for Cloud untuk mesin virtual.
- Menerapkan rekomendasi untuk konfigurasi tamu dan akses VM tepat waktu. 
- Mengulas bagaimana Skor Aman dapat digunakan untuk menentukan kemajuan dalam menciptakan infrastruktur yang lebih aman.

> Untuk semua sumber daya di lab ini, kita menggunakan wilayah **US Timur**. Pastikan pada instruktur Anda bahwa ini adalah wilayah yang akan digunakan untuk kelas. 

## <a name="lab-objectives"></a>Tujuan lab

Di lab ini, Anda akan menyelesaikan latihan berikut:

- Latihan 1: Menerapkan Pertahanan Microsoft untuk Cloud

## <a name="microsoft-defender-for-cloud-diagram"></a>Diagaram Microsoft Defender untuk Cloud

![gambar](https://user-images.githubusercontent.com/91347931/157537800-94a64b6e-026c-41b2-970e-f8554ce1e0ab.png)

## <a name="instructions"></a>Instruksi

### <a name="exercise-1-implement-microsoft-defender-for-cloud"></a>Latihan 1: Menerapkan Pertahanan Microsoft untuk Cloud

Dalam latihan ini, Anda akan menyelesaikan tugas-tugas berikut:

- Tugas 1: Mengonfigurasi Pertahanan Microsoft untuk Cloud
- Tugas 2: Mengulas rekomendasi Microsoft Defender untuk Cloud
- Tugas 3: Menerapkan rekomendasi Microsoft Defender untuk Cloud untuk mengaktifkan Akses VM Tepat Waktu

#### <a name="task-1-configure-microsoft-defender-for-cloud"></a>Tugas 1: Mengonfigurasi Pertahanan Microsoft untuk Cloud

Dalam tugas ini, Anda akan mengaktifkan dan mengonfigurasi Microsoft Defender untuk Cloud.

1. Masuk ke portal Microsoft Azure **`https://portal.azure.com/`** .

    >**Catatan**: Masuk ke portal Microsoft Azure menggunakan akun yang memiliki peran Pemilik atau Kontributor dalam langganan Azure yang Anda gunakan untuk lab ini.

2. Di portal Azure, di kotak teks **Search resources, services, and docs** di bagian atas halaman portal Azure, ketik **Microsoft Defender untuk Cloud** dan tekan **Enter** kunci.

3. Jika belum selesai sebelumnya, pada panel **Microsoft Defender untuk Cloud \| Memulai**, klik **Perbarui**.
     
4. Jika belum selesai sebelumnya, pada panel **Microsoft Defender untuk Cloud \| Memulai**, di tab **Instal agen**, gulir ke bawah dan klik **Instal agen**.

5. Pada panel **Microsoft Defender untuk Cloud \| Memulai**, pada tab **Tingkatkan** >> di bagian **Pilih ruang kerja dengan fitur keamanan yang disempurnakan** >> aktifkan **paket Microsoft Defender** dengan memilih Ruang Kerja Analisis Log Anda. 

    >**Catatan**: Ulas semua fitur yang tersedia sebagai bagian dari paket Microsoft Defender. 

6. Pada panel **Defender plan**, pilih **Aktifkan semua Microsoft Defender untuk Cloud Plans** dan klik **Simpan**.

7. Navigasikan ke **Microsoft Defender untuk Cloud** dan klik **Pengaturan Lingkungan** di bawah Pengaturan Manajemen, di bilah menu vertikal di sisi kiri.

8. Di **Microsoft Defender untuk Cloud | panel Pengaturan Lingkungan**, klik langganan yang relevan. 

9. Pada **Pengaturan | Panel rencana Defender**, pada menu vertikal di sisi kiri, klik **Penyediaan otomatis**.

10. Pada **Pengaturan | panel Penyediaan otomatis**, pastikan Penyediaan otomatis diatur ke **Aktif** untuk item pertama **Log Analytics agent for Azure VMs**.

11. Pada panel **Pengaturan \| Automatisasi alur kerja**, klik **+ Tambahkan automatisasi alur kerja**.

12. Pada panel **Tambahkan automatisasi alur kerja**, tinjau pengaturan yang tersedia. 

    >**Catatan**: Anda dapat memicu peringatan deteksi ancaman berbasis tindakan dan rekomendasi Microsoft Defender untuk Cloud. Anda juga dapat mengonfigurasi tindakan berdasarkan aplikasi Logika. 

13. Pada panel **Tambahkan otomatisasi alur kerja**, klik **Batal**.

    >**Catatan**: Microsoft Defender untuk Cloud memberikan banyak wawasan tentang mesin virtual termasuk status pembaruan sistem, konfigurasi keamanan OS, dan perlindungan titik akhir.

14. Navigasikan kembali ke panel **Microsoft Defender untuk Cloud \| Pengaturan Lingkungan**, perluas langganan Anda, dan klik entri yang mewakili ruang kerja Log Analytics yang Anda buat di lab sebelumnya.

15. Pada panel **Pengaturan \| Paket Defender**, pastikan bahwa **Aktifkan semua paket Microsoft Defender untuk Cloud** dipilih dan klik **Simpan**.

16. Pilih **Pengumpulan data** dari panel **Pengaturan\|Microsoft Defender untuk Cloud**. Pilih **Semua Peristiwa** dan **Simpan**.


#### <a name="task-2-review-the-microsoft-defender-for-cloud-recommendation"></a>Tugas 2: Ulas rekomendasi Microsoft Defender untuk Cloud

Dalam tugas ini, Anda akan meninjau rekomendasi Microsoft Defender untuk Cloud. 

1. Di portal Azure, navigasikan kembali ke panel Gambaran Umum **Microsoft Defender untuk Cloud \|** . 

2. Pada panel **Microsoft Defender untuk Cloud \| Gambaran Umum**, ulas petak peta **Skor Aman**.

    >**Catatan**: Catat skor saat ini jika tersedia.

3. Navigasikan kembali ke panel **Microsoft Defender untuk Cloud \| Gambaran Umum**, pilih **Sumber daya yang dinilai**.

4. Pada panel **Inventaris**, pilih entri **myVM**.

    >**Catatan**: Anda mungkin harus menunggu beberapa menit dan menyegarkan halaman browser agar entri muncul.
    
5. Pada panel **Kesehatan sumber daya**, pada tab **Rekomendasi**, ulas daftar rekomendasi untuk **myVM**.


#### <a name="task-3-implement-the-microsoft-defender-for-cloud-recommendation-to-enable-just-in-time-vm-access"></a>Tugas 3: Menerapkan rekomendasi Microsoft Defender untuk Cloud untuk mengaktifkan Akses VM Tepat Waktu

Dalam tugas ini, Anda akan menerapkan rekomendasi Microsoft Defender untuk Cloud untuk mengaktifkan Akses VM Tepat Waktu di mesin virtual. 

1. Di portal Azure, navigasikan kembali ke panel **Microsoft Defender untuk Cloud \| Gambaran Umum** dan pilih **Perlindungan beban kerja** di bawah petak peta **Cloud Security**.

2. Pada bilah **Perlindungan beban kerja**, di bagian **Perlindungan lanjutan**, klik ubin **Akses VM tepat waktu** dan, pada panel **Akses VM tepat waktu** , klik **Coba Akses VM tepat waktu**.

    >**Catatan**: Jika VM tidak terdaftar, navigasikan ke panel **Mesin Virtual** dan klik **Konfigurasi**, Klik opsi **Aktifkan VM Tepat Waktu** di bawah **Akses Vm tepat waktu**. Ulangi langkah di atas untuk menavigasi kembali ke **Microsoft Defender untuk Cloud** dan menyegarkan halaman, VM akan muncul.

3. Pada **Akses VM tepat waktu**, pilih Tidak **Dikonfigurasi**, lalu klik entri **myVM**.

    >**Catatan**: Anda mungkin harus menunggu beberapa menit sebelum entri **myVM** tersedia.

4. Pilih **Aktifkan JIT pada 1 VM**.

5. Pada panel **Konfigurasi akses VM JIT**, di ujung kanan baris yang merujuk pada port **22**, klik tombol elipsis, lalu klik **Hapus**.

6. Pada panel **Konfigurasi akses VM JIT**, klik **Simpan**.

    >**Catatan**: Pantau kemajuan konfigurasi dengan mengeklik ikon **Pemberitahuan** di toolbar dan melihat panel **Pemberitahuan**. 

    >**Catatan**: Perlu beberapa waktu agar penerapan rekomendasi di lab ini dapat dicerminkan oleh Skor Aman. Periksa Skor Aman secara berkala untuk menentukan dampak penerapan fitur ini. 

> Hasil: Anda telah mengaktifkan Microsoft Defender untuk Cloud dan menerapkan rekomendasi mesin virtual. 

    >**Note**: Do not remove the resources from this lab as they are needed for the Azure Sentinel lab.
