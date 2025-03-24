# Ringkasan Sistem Operasi

## 1. Konsep Dasar Sistem Operasi
- **Definisi & Peran:**  
  Sistem operasi adalah perangkat lunak inti yang mengelola perangkat keras dan menyediakan layanan dasar bagi aplikasi.
- **Tujuan Utama:**  
  Menyederhanakan penggunaan komputer, mengoptimalkan efisiensi sumber daya, serta menjaga keamanan dan isolasi antar proses.

## 2. Arsitektur Sistem Operasi
- **Kernel:**  
  Bagian inti yang bertanggung jawab atas manajemen sumber daya seperti CPU, memori, dan perangkat I/O.
- **Struktur Modul:**  
  Pembagian fungsi ke dalam modul-modul (misalnya manajemen proses, memori, berkas, dan I/O) untuk meningkatkan fleksibilitas dan skalabilitas.

## 3. Manajemen Proses dan Thread
- **Proses vs. Thread:**  
  - Proses: Unit eksekusi dasar.  
  - Thread: Bagian dari proses yang dapat berjalan secara paralel untuk meningkatkan efisiensi.
- **Penjadwalan:**  
  Algoritma seperti FCFS (First-Come, First-Served), SJF (Shortest Job First), dan Round Robin untuk menentukan urutan eksekusi proses.
- **Switching Konteks:**  
  Prosedur untuk menyimpan dan memulihkan status proses saat CPU berpindah dari satu proses ke proses lainnya.

## 4. Manajemen Memori
- **Alokasi Memori:**  
  Penggunaan teknik alokasi memori kontigu dan dinamis untuk mengoptimalkan penggunaan ruang memori.
- **Paging dan Segmentation:**  
  Dua metode utama dalam pengelolaan ruang alamat, dengan penggunaan virtual memory yang memungkinkan program berjalan lebih besar daripada memori fisik yang tersedia.

## 5. Sistem Berkas
- **Struktur Organisasi:**  
  Data diatur secara hierarkis menggunakan direktori dan sub-direktori untuk memudahkan akses dan pengelolaan.
- **Teknik Pengalokasian:**  
  Metode seperti pengalokasian berturut-turut, linked allocation, dan indeksasi untuk mengoptimalkan penyimpanan dan kecepatan akses data.

## 6. Sistem Input/Output (I/O)
- **Prinsip Operasi I/O:**  
  Mengelola komunikasi antara perangkat keras dan sistem operasi melalui mekanisme seperti interrupt dan Direct Memory Access (DMA) untuk transfer data yang efisien.
- **Driver Perangkat:**  
  Abstraksi yang disediakan oleh sistem operasi untuk memudahkan interaksi dengan berbagai jenis perangkat keras tanpa harus memahami detail teknis masing-masing perangkat.

## 7. Perlindungan dan Keamanan Sistem
- **Mekanisme Keamanan:**  
  Implementasi otorisasi, autentikasi, dan kontrol akses untuk melindungi data dan sumber daya dari akses yang tidak sah.
- **Isolasi Proses:**  
  Strategi untuk memastikan bahwa proses berjalan secara terpisah guna mencegah gangguan atau penyalahgunaan data antar proses.

## 8. Sinkronisasi dan Penanganan Deadlock
- **Sinkronisasi:**  
  Teknik seperti mutex, semaphore, dan monitor digunakan untuk mengatur akses ke sumber daya bersama, mencegah kondisi race.
- **Deadlock:**  
  Kondisi ketika dua atau lebih proses saling menunggu sumber daya yang dipegang oleh proses lain. Sistem operasi menerapkan strategi pencegahan, deteksi, dan pemulihan deadlock agar sistem tetap berjalan lancar.

## 9. Evaluasi Kinerja Sistem Operasi
- **Metrik Kinerja:**  
  Pengukuran seperti throughput, latency, dan utilisasi CPU untuk menilai performa sistem.
- **Analisis dan Simulasi:**  
  Metode evaluasi melalui simulasi dan analisis matematis guna memahami dampak dari algoritma penjadwalan dan pengelolaan sumber daya terhadap kinerja secara keseluruhan.
