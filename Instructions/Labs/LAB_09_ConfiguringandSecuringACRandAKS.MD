---
lab:
  title: 09 - Mengonfigurasi dan Mengamankan ACR dan AKS
  module: Module 02 - Implement Platform Protection
ms.openlocfilehash: 167dca080d65d97997b3a884015bfa2228c17a23
ms.sourcegitcommit: 967cb50981ef07d731dd7548845a38385b3fb7fb
ms.translationtype: HT
ms.contentlocale: id-ID
ms.lasthandoff: 05/31/2022
ms.locfileid: "145955418"
---
# <a name="lab-09-configuring-and-securing-acr-and-aks"></a>Lab 09: Mengonfigurasi dan Mengamankan ACR dan AKS
# <a name="student-lab-manual"></a>Panduan lab siswa

## <a name="lab-scenario"></a>Skenario lab

Anda telah diminta untuk menerapkan bukti konsep dengan Azure Container Registry dan Azure Kubernetes Service. Secara khusus, bukti konsep harus menunjukkan:

- Menggunakan Dockerfile untuk membuat gambar.
- Menggunakan Azure Container Registry untuk menyimpan gambar.
- Mengonfigurasi Azure Kubernetes Service.
- Mengamankan dan mengakses aplikasi kontainer baik internal maupun eksternal. 

> Untuk semua sumber daya di lab ini, kita menggunakan wilayah **US Timur**. Pastikan pada instruktur Anda bahwa ini adalah wilayah yang akan digunakan untuk kelas. 

## <a name="lab-objectives"></a>Tujuan lab

Di lab ini, Anda akan menyelesaikan latihan berikut:

- Latihan 1: Mengonfigurasi dan Mengamankan ACR dan AKS

## <a name="configuring-and-securing-acr-and-aks-diagram"></a>Mengonfigurasi dan Mengamankan diagram ACR dan AKS

![gambar](https://user-images.githubusercontent.com/91347931/157532250-1104a829-792a-4b6d-beff-fe976e2d5d4b.png)

## <a name="intructions"></a>Instruksi

## <a name="lab-files"></a>File lab:

- **\\Allfiles\\Labs\\09\\nginxexternal.yaml**
- **\\Allfiles\\Labs\\09\\nginxinternal.yaml**

### <a name="exercise-1-configuring-and-securing-acr-and-aks"></a>Latihan 1: Mengonfigurasi dan Mengamankan ACR dan AKS

### <a name="estimated-timing-45-minutes"></a>Perkiraan waktu: 45 menit

> Untuk semua sumber daya di lab ini, kita menggunakan wilayah **Timur (US)** . Pastikan pada instruktur Anda bahwa ini adalah wilayah yang akan digunakan untuk kelas Anda. 

Dalam latihan ini, Anda akan menyelesaikan tugas-tugas berikut:

- Tugas 1: Membuat Azure Container Registry
- Tugas 2: Membuat Dockerfile, membangun kontainer dan mendorongnya ke Azure Container Registry
- Tugas 3: Membuat kluster Azure Kubernetes Service
- Tugas 4: Memberikan izin kluster AKS untuk mengakses ACR
- Tugas 5: Menerapkan layanan eksternal ke AKS
- Tugas 6: Memverifikasi bahwa Anda dapat mengakses layanan eksternal yang dihosting AKS
- Tugas 7: Menerapkan layanan internal ke AKS
- Tugas 8: Memverifikasi bahwa Anda dapat mengakses layanan internal yang dihosting AKS

#### <a name="task-1-create-an-azure-container-registry"></a>Tugas 1: Membuat Azure Container Registry

Dalam tugas ini, Anda akan membuat grup sumber daya untuk lab dan Azure Container Registry.

1. Masuk ke portal Microsoft Azure **`https://portal.azure.com/`** .

    >**Catatan**: Masuk ke portal Azure menggunakan akun yang memiliki peran Pemilik atau Kontributor dalam langganan Azure yang Anda gunakan untuk lab ini dan peran Administrator Global di penyewa Azure AD yang terkait dengan langganan tersebut.

2. Di portal Microsoft Azure, buka Cloud Shell dengan mengklik ikon pertama di kanan atas Portal Microsoft Azure. Jika diminta, klik **Bash** dan **Buat penyimpanan**.

3. Pastikan **Bash** dipilih di menu drop-down di sudut kiri atas panel Cloud Shell.

4. Di sesi Bash dalam panel Cloud Shell, jalankan perintah berikut untuk membuat grup sumber daya baru untuk lab ini:

    ```sh
    az group create --name AZ500LAB09 --location eastus
    ```

5. Di sesi Bash dalam panel Cloud Shell, jalankan perintah berikut untuk memverifikasi bahwa grup sumber daya telah dibuat:

    ```
    az group list --query "[?name=='AZ500LAB09']" -o table
    ```

6. Di sesi Bash dalam panel Cloud Shell, jalankan perintah berikut untuk membuat instans Azure Container Registry (ACR) baru (Nama ACR harus unik secara global): 

    ```sh
    az acr create --resource-group AZ500LAB09 --name az500$RANDOM$RANDOM --sku Basic
    ```

7. Di sesi Bash dalam panel Cloud Shell, jalankan perintah berikut untuk mengonfirmasi bahwa ACR baru telah dibuat:

    ```sh
    az acr list --resource-group AZ500LAB09
    ```

    >**Catatan**: Catat nama ACR. Anda akan membutuhkannya di tugas berikutnya.

#### <a name="task-2-create-a-dockerfile-build-a-container-and-push-it-to-azure-container-registry"></a>Tugas 2: Membuat Dockerfile, membangun kontainer dan mendorongnya ke Azure Container Registry

Dalam tugas ini, Anda akan membuat Dockerfile, membuat gambar dari Dockerfile, dan menyebarkannya ke ACR. 

1. Di sesi Bash dalam panel Cloud Shell, jalankan perintah berikut untuk membuat Dockerfile untuk membuat gambar berbasis Nginx: 

    ```sh
    echo FROM nginx > Dockerfile
    ```

2. Di sesi Bash dalam panel Cloud Shell, jalankan perintah berikut untuk membuat gambar dari Dockerfile dan mendorong gambar tersebut ke ACR baru. 

    >**Catatan**: Periode akhir di akhir baris perintah diperlukan. Ini menunjuk direktori saat ini sebagai lokasi Dockerfile. 

    ```sh
    ACRNAME=$(az acr list --resource-group AZ500LAB09 --query '[].{Name:name}' --output tsv)

    az acr build --image sample/nginx:v1 --registry $ACRNAME --file Dockerfile .
    ```

    >**Catatan**: Tunggu hingga perintah berhasil diselesaikan. Ini mungkin memakan waktu sekitar 2 menit.

3. Tutup panel Cloud Shell.

4. Di portal Azure, navigasikan ke grup sumber daya **AZ500Lab09** dan, dalam daftar sumber daya, klik entri yang mewakili instans Azure Container Registry yang Anda sediakan di tugas sebelumnya.

5. Pada bilah registri Kontainer, di bagian **Layanan**, klik **Repositori**. 

6. Verifikasi bahwa daftar repositori menyertakan gambar kontainer baru bernama **sampel/nginx**.

7. Klik entri **sample/nginx** dan verifikasi keberadaan tag **v1** yang mengidentifikasi versi gambar.

8. Klik entri **v1** untuk melihat manifes gambar.

    >**Catatan**: Manifes mencakup intisari sha256, tanggal pembuatan manifes, dan entri platform. 

#### <a name="task-3-create-an-azure-kubernetes-service-cluster"></a>Tugas 3: Membuat kluster Azure Kubernetes Service

Dalam tugas ini, Anda akan membuat layanan Azure Kubernetes dan meninjau sumber daya yang digunakan. 

1. Di portal Microsoft Azure, di kotak teks **Cari sumber daya, layanan, dan dokumen** di bagian atas halaman portal Microsoft Azure, ketik **layanan Kubernetes** dan tekan tombol **Enter**.

2. Pada bilah **layanan Kubernetes**, klik **+ Buat** dan, di menu menurun, klik **+ Buat kluster Kubernetes**

3. Pada tab panel **Dasar** dari **Buat kluster Kubernetes**, pilih **Konfigurasi preset kluster**, pilih **Dev/Test ($)** . Sekarang tentukan pengaturan berikut (biarkan yang lain dengan nilai defaultnya):

    |Pengaturan|Nilai|
    |----|----|
    |Langganan|nama langganan Azure yang Anda gunakan di lab ini|
    |Grup sumber daya|**AZ500LAB09**|
    |Nama kluster Arc Kube|**MyKubernetesCluster**|
    |Wilayah|**(AS) AS Timur**|
    |Zona Ketersediaan |**Tidak ada**|
    |Metode skala|**Manual**|
    |Jumlah simpul|**1**|

4. Klik **Berikutnya: Kumpulan Node >** dan, pada tab **Kumpulan Node** bilah **Buat kluster Kubernetes**, tentukan pengaturan berikut (biarkan yang lain dengan nilai defaultnya):

    |Pengaturan|Nilai|
    |----|----|
    |Aktifkan node virtual|kotak centang dibersihkan|
    
5. Klik **Berikutnya: Access >** , pada tab **Access** bilah **Buat kluster Kubernetes**, terima defaultnya, dan klik **Berikutnya: Jaringan >** . 

6. Pada tab **Jaringan** bilah **Buat kluster Kubernetes**, tentukan pengaturan berikut (biarkan yang lain dengan nilai defaultnya):

    |Pengaturan|Nilai|
    |----|----|
    |Konfigurasi jaringan|**Azure CNI**|
    |Awalan nama DNS|**Gunakan nilai default**|

    >**Catatan**: AKS dapat dikonfigurasi sebagai kluster pribadi. Ini menetapkan IP pribadi ke server API untuk memastikan lalu lintas jaringan antara server API dan kumpulan node Anda tetap berada di jaringan pribadi saja. Untuk informasi selengkapnya, kunjungi laman [Membuat kluster Azure Kubernetes Service pribadi](https://docs.microsoft.com/en-us/azure/aks/private-clusters).

7. Klik **Berikutnya: Integrasi >** dan, pada tab **Integrasi** bilah **Buat kluster Kubernetes**, atur **Monitoring kontainer** ke **Nonaktif**. 

    >**Catatan**: Dalam skenario produksi, Anda ingin mengaktifkan monitoring. Monitoring dinonaktifkan dalam kasus ini karena tidak tercakup dalam lab. 

8. Klik **Tinjau + Buat**, lalu klik **Buat**.

    >**Catatan**: Tunggu hingga penyebaran selesai. Proses ini mungkin perlu waktu sekitar 10 menit.

9. Setelah penyebaran selesai, di portal Microsoft Azure, di kotak teks **Cari sumber daya, layanan, dan dokumen** di bagian atas halaman portal Microsoft Azure, ketik **Grup sumber daya** dan tekan tombol **Enter**.

10. Pada bilah **Grup sumber daya**, dalam daftar grup sumber daya, catat grup sumber daya baru bernama **MC_AZ500LAB09_MyKubernetesCluster_eastus** yang menyimpan komponen AKS Nodes. Tinjau sumber daya dalam grup sumber daya ini. 
    
11. Navigasikan kembali ke bilah **Grup sumber daya** dan klik entri **AZ500LAB09**. 

    >**Catatan**: Dalam daftar sumber daya, perhatikan Cluster AKS dan jaringan virtual yang sesuai.

12. Di portal Microsoft Azure, buka sesi Bash di Cloud Shell. 

    >**Catatan**: Pastikan **Bash** dipilih di menu drop-down di sudut kiri atas panel Cloud Shell.

13. Di sesi Bash dalam panel Cloud Shell, jalankan perintah berikut untuk terhubung ke kluster Kubernetes:

    ```sh
    az aks get-credentials --resource-group AZ500LAB09 --name MyKubernetesCluster
    ```

14. Di sesi Bash dalam panel Cloud Shell, jalankan perintah berikut untuk membuat daftar node dari kluster Kubenetes: 

    ```sh
    kubectl get nodes
    ```

    >**Catatan**: Verifikasi bahwa **Status** node kluster terdaftar sebagai **Siap**.

#### <a name="task-4-grant-the-aks-cluster-permissions-to-access-the-acr-and-manage-its-virtual-network"></a>Tugas 4: Memberikan izin kluster AKS untuk mengakses ACR dan mengelola jaringan virtualnya

Dalam tugas ini, Anda akan memberikan izin kluster AKS untuk mengakses ACR dan mengelola jaringan virtualnya. 

1. Di sesi Bash dalam panel Cloud Shell, jalankan perintah berikut untuk mengonfigurasi kluster AKS agar menggunakan instans Azure Container Registry yang Anda buat sebelumnya di lab ini. 

    ```sh
    ACRNAME=$(az acr list --resource-group AZ500LAB09 --query '[].{Name:name}' --output tsv)

    az aks update -n MyKubernetesCluster -g AZ500LAB09 --attach-acr $ACRNAME
    ```

    >**Catatan**: Perintah ini memberikan penetapan peran 'acrpull' ke ACR. 

    >**Catatan**: Mungkin perlu beberapa menit untuk menyelesaikan perintah ini. 

2. Di sesi Bash dalam panel Cloud Shell, jalankan perintah berikut untuk memberi kluster AKS peran Kontributor ke jaringan virtualnya. 

    ```sh
    RG_AKS=AZ500LAB09
    
    AKS_VNET_NAME=AZ500LAB09-vnet
    
    AKS_CLUSTER_NAME=MyKubernetesCluster
    
    AKS_VNET_ID=$(az network vnet show --name $AKS_VNET_NAME --resource-group $RG_AKS --query id -o tsv)
    
    AKS_MANAGED_ID=$(az aks show --name $AKS_CLUSTER_NAME --resource-group $RG_AKS --query identity.principalId -o tsv)
    
    az role assignment create --assignee $AKS_MANAGED_ID --role "Contributor" --scope $AKS_VNET_ID
    ```

#### <a name="task-5-deploy-an-external-service-to-aks"></a>Tugas 5: Menerapkan layanan eksternal ke AKS

Dalam tugas ini, Anda akan mengunduh file Manifest, mengedit file YAML, dan menerapkan perubahan Anda ke kluster. 

1. Di sesi Bash dalam panel Cloud Shell, klik ikon **Unggah/Unduh file**, di menu menurun, klik **Unggah**, di dialog **Buka** kotak, navigasikan ke lokasi tempat Anda mengunduh file lab, pilih **\\Allfiles\\Labs\\09\\nginxexternal.yaml** klik **Buka**. Selanjutnya, pilih **\\Allfiles\\Labs\\09\\nginxinternal.yaml**, dan klik **Buka**.

2. Di sesi Bash dalam panel Cloud Shell, jalankan perintah berikut untuk mengidentifikasi nama instans Azure Container Registry:

    ```sh
    echo $ACRNAME
    ```

    >**Catatan**: Catat nama instans Azure Container Registry. Anda akan membutuhkannya nanti dalam tugas ini.

3. Di sesi Bash dalam panel Cloud Shell, jalankan perintah berikut untuk membuka file nginxexternal.yaml, sehingga Anda dapat mengedit kontennya. 

    ```sh
    code ./nginxexternal.yaml
    ```

    >**Catatan**: Ini adalah file yaml *eksternal*.

4. Di panel editor, gulir ke bawah ke **baris 24** dan ganti tempat penampung **`<ACRUniquename>`** dengan nama ACR.

5. Di panel editor, di sudut kanan atas, klik ikon **elips**, klik **Simpan**, lalu klik **Tutup editor**. 

6. Di sesi Bash dalam panel Cloud Shell, jalankan perintah berikut untuk menerapkan perubahan ke kluster:

    ```sh
    kubectl apply -f nginxexternal.yaml
    ```

7. Di sesi Bash dalam panel Cloud Shell, tinjau output dari perintah yang Anda jalankan di tugas sebelumnya untuk memverifikasi bahwa penerapan dan layanan terkait telah dibuat. 

    ```
    deployment.apps/nginxexternal created
    service/nginxexternal created
    ```

#### <a name="task-6-verify-the-you-can-access-an-external-aks-hosted-service"></a>Tugas 6: Memverifikasi bahwa Anda dapat mengakses layanan eksternal yang dihosting AKS

Dalam tugas ini, verifikasi kontainer dapat diakses secara eksternal menggunakan alamat IP publik.

1. Di sesi Bash dalam panel Cloud Shell, jalankan perintah berikut untuk mengambil informasi tentang layanan nginxexternal termasuk nama, jenis, alamat IP, dan port. 

    ```sh
    kubectl get service nginxexternal
    ```

2. Di sesi Bash dalam panel Cloud Shell, tinjau output dan catat nilainya di kolom External-IP. Anda akan membutuhkannya di langkah berikutnya. 

3. Buka tab browser baru dan telusuri alamat IP yang Anda identifikasi di langkah sebelumnya.

4. Pastikan **Selamat datang di nginx!** tampilan halaman. 

#### <a name="task-7-deploy-an-internal-service-to-aks"></a>Tugas 7: Menerapkan layanan internal ke AKS

Dalam tugas ini, Anda akan menerapkan layanan internal yang dihadapi pada AKS. 

1. Di sesi Bash dalam panel Cloud Shell, jalankan perintah berikut untuk membuka file nginxintenal.yaml, sehingga Anda dapat mengedit kontennya. 

    ```sh
    code ./nginxinternal.yaml
    ```

    >**Catatan**: Ini adalah file yaml *internal*.

2. Di panel editor, gulir ke bawah ke baris yang berisi referensi ke gambar kontainer dan ganti **`<ACRUniquename>`** tempat penampung dengan nama ACR.

3. Di panel editor, di sudut kanan atas, klik ikon **elips**, klik **Simpan**, lalu klik **Tutup editor**. 

4. Di sesi Bash dalam panel Cloud Shell, jalankan perintah berikut untuk menerapkan perubahan ke kluster:

    ```sh
    kubectl apply -f nginxinternal.yaml
    ```

5.  Di sesi Bash dalam panel Cloud Shell, tinjau output untuk memverifikasi penerapan Anda dan layanan telah dibuat:

    ```
    deployment.apps/nginxinternal created
    service/nginxinternal created
    ```

6. Di sesi Bash dalam panel Cloud Shell, jalankan perintah berikut untuk mengambil informasi tentang layanan nginxinternal termasuk nama, jenis, alamat IP, dan port. 

    ```sh
    kubectl get service nginxinternal
    ```

7. Di sesi Bash dalam panel Cloud Shell, tinjau hasilnya. Eksternal-IP, dalam hal ini, adalah alamat IP pribadi. Jika dalam status **Tertunda**, jalankan kembali perintah sebelumnya.

    >**Catatan**: Catat alamat IP ini. Anda akan membutuhkannya di tugas berikutnya. 

    >**Catatan**: Untuk mengakses titik akhir layanan internal, Anda akan terhubung secara interaktif ke salah satu pod yang berjalan di kluster. 

    >**Catatan**: Atau Anda bisa menggunakan alamat CLUSTER-IP.

#### <a name="task-8-verify-the-you-can-access-an-internal-aks-hosted-service"></a>Tugas 8: Memverifikasi bahwa Anda dapat mengakses layanan internal yang dihosting AKS

Dalam tugas ini, Anda akan menggunakan salah satu pod yang berjalan di kluster AKS untuk mengakses layanan internal. 

1. Di sesi Bash dalam panel Cloud Shell, jalankan perintah berikut untuk membuat daftar pod di namespace default di kluster AKS:

    ```sh
    kubectl get pods
    ```

2. Dalam daftar pod, salin entri pertama di kolom **NAME**.

    >**Catatan**: Ini adalah pod yang akan Anda gunakan di langkah selanjutnya.

3. Di sesi Bash dalam panel Cloud Shell, jalankan perintah berikut untuk terhubung secara interaktif ke pod pertama (ganti `<pod_name>` tempat penampung dengan nama yang Anda salin di langkah sebelumnya):

    ```sh
    kubectl exec -it <pod_name> -- /bin/bash
    ```

4. Di sesi Bash dalam panel Cloud Shell, jalankan elemen berikut untuk memverifikasi bahwa situs web nginx tersedia melalui alamat IP pribadi layanan (ganti `<internal_IP>` tempat penampung dengan alamat IP yang Anda catat di tugas sebelumnya):

    ```sh
    curl http://<internal_IP>
    ```

5. Tutup panel Cloud Shell.

> Hasil: Anda telah mengonfigurasi dan mengamankan ACR dan AKS.


**Membersihkan sumber daya**

> Jangan lupa untuk menghapus sumber daya Azure yang baru dibuat dan yang tidak diperlukan lagi. Menghapus sumber daya yang tidak terpakai memastikan Anda tidak akan dikenakan biaya tidak terduga.

1. Di portal Microsoft Azure, buka Cloud Shell dengan mengklik ikon pertama di kanan atas Portal Microsoft Azure. 

2. Di menu drop-down kiri atas panel Cloud Shell, pilih **PowerShell** dan, jika diminta, klik **Konfirmasi**.

3. Di sesi PowerShell dalam panel Cloud Shell, jalankan yang berikut ini untuk menghapus grup sumber daya yang Anda buat di lab ini:
  
    ```powershell
    Remove-AzResourceGroup -Name "AZ500LAB09" -Force -AsJob
    ```

4.  Tutup panel **Cloud Shell**. 
