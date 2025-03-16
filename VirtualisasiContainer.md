**Nama:** Sulistyo Fajar Pratama  
**NRP:** 3124500037  
**Kelas:** D3 IT B

# Materi Sistem Operasi dan Virtualisasi Container

## 1. Perbedaan Multiprogramming dan Multitasking

Multiprogramming dan multitasking adalah dua konsep fundamental dalam dunia sistem operasi yang berkaitan dengan cara CPU mengelola dan menjalankan beberapa program secara bersamaan. Meskipun kedua konsep ini saling terkait, terdapat perbedaan mendasar di antara keduanya yang perlu dipahami dengan baik.

### 1.1 Multiprogramming
Multiprogramming merupakan teknik awal yang digunakan oleh sistem operasi untuk meningkatkan efisiensi CPU. Dalam multiprogramming, beberapa program dimuat ke dalam memori secara bersamaan. Ketika program yang sedang berjalan harus menunggu input/output (I/O) atau terjadi interupsi, CPU segera berpindah ke program lain yang sudah siap dijalankan. Dengan cara ini, CPU tidak menganggur dan penggunaan waktu proses menjadi lebih maksimal. Teknik ini terutama digunakan pada sistem yang memiliki perbedaan kecepatan antara CPU dan perangkat I/O, di mana proses I/O lebih lambat dibandingkan kecepatan CPU.

### 1.2 Multitasking
Multitasking adalah perkembangan dari multiprogramming yang memungkinkan sistem operasi menjalankan beberapa tugas atau proses secara "bersamaan" melalui mekanisme time-sharing. Di sini, CPU melakukan context switching secara cepat antar proses sehingga memberikan kesan bahwa beberapa tugas berjalan simultan. Walaupun pada kenyataannya hanya satu proses yang dieksekusi pada satu waktu, kecepatan switching yang tinggi membuat pengguna merasa bahwa proses-proses tersebut berjalan secara paralel. Contohnya, ketika Anda mendengarkan musik, mengetik dokumen, dan menggunakan browser secara bersamaan, sistem operasi akan secara bergantian mengalokasikan waktu CPU untuk masing-masing proses tersebut sehingga semua aplikasi dapat berfungsi dengan responsif.

**Secara ringkas:**  
- **Multiprogramming** fokus pada pemanfaatan CPU dengan memuat banyak program ke dalam memori.  
- **Multitasking** mengatur agar pengguna dapat menjalankan beberapa aplikasi secara interaktif dengan alokasi waktu CPU yang cepat.

## 2. Fungsi Sistem Operasi (OS)

Sistem Operasi adalah perangkat lunak inti yang menjadi penghubung antara perangkat keras dan aplikasi. Tanpa OS, pengguna tidak dapat berinteraksi dengan komputer secara efisien. OS memiliki peran sentral dalam mengelola sumber daya sistem, dan berikut adalah beberapa fungsi utamanya:

- **a. Manajemen Proses**  
  OS mengatur eksekusi program dengan melakukan penjadwalan proses. Hal ini meliputi pembuatan, eksekusi, penghentian, dan pengelolaan status setiap proses. Teknik seperti context switching memungkinkan CPU berpindah dari satu proses ke proses lain dengan cepat, sehingga penggunaan CPU menjadi maksimal dan responsivitas sistem meningkat.

- **b. Manajemen Memori**  
  Sistem operasi mengalokasikan memori utama ke masing-masing proses, memastikan tidak terjadi konflik antar proses. Fungsi ini mencakup penanganan fragmentasi memori, swapping, dan virtual memory management untuk mengoptimalkan penggunaan RAM yang terbatas.

- **c. Manajemen File**  
  OS menyediakan struktur penyimpanan untuk file dan direktori. Fungsi ini mencakup pengaturan hak akses, pembuatan, penghapusan, dan pemeliharaan file serta memastikan data tersimpan dengan aman dan terorganisir.

- **d. Manajemen Perangkat Keras**  
  OS mengatur komunikasi antara perangkat keras seperti printer, keyboard, dan disk drive dengan aplikasi. Driver perangkat keras memungkinkan OS untuk menerjemahkan instruksi dari aplikasi ke dalam perintah yang dapat dipahami oleh perangkat keras.

- **e. Keamanan dan Proteksi**  
  Melalui mekanisme otentikasi dan otorisasi, OS memastikan bahwa hanya pengguna yang berwenang yang dapat mengakses data dan sumber daya. Isolasi proses dan pengaturan hak akses merupakan bagian penting untuk menjaga integritas sistem dari serangan maupun kesalahan aplikasi.

- **f. Antarmuka Pengguna (User Interface)**  
  OS menyediakan antarmuka grafis (GUI) atau baris perintah (CLI) agar pengguna dapat berinteraksi dengan sistem dengan mudah. Antarmuka ini memungkinkan pengguna menjalankan aplikasi, mengelola file, dan mengatur perangkat keras secara intuitif.

Fungsi-fungsi tersebut bekerja secara sinergis untuk menciptakan lingkungan komputasi yang stabil, efisien, dan mudah dioperasikan.

## 3. Virtualisasi Container

Virtualisasi container merupakan teknologi OS-level virtualization yang memungkinkan aplikasi beserta semua dependensinya dikemas dalam unit yang terisolasi, yang disebut container. Teknologi ini telah merevolusi cara pengembangan dan deployment aplikasi dengan meningkatkan portabilitas, efisiensi, dan skalabilitas.

### 3.1 Konsep Dasar Virtualisasi Container
Berbeda dengan virtual machine (VM) yang memerlukan instalasi sistem operasi tamu lengkap pada setiap instance, container hanya membawa aplikasi dan pustaka yang diperlukan. Container berjalan pada kernel sistem operasi host, sehingga overhead yang dihasilkan jauh lebih rendah. Hal ini memungkinkan banyak container dijalankan secara bersamaan pada satu host tanpa mengganggu performa sistem.

### 3.2 Keunggulan Virtualisasi Container
Keunggulan utama dari container meliputi:
- **Ringan dan Efisien:**  
  Container tidak memuat sistem operasi lengkap sehingga membutuhkan sumber daya yang lebih sedikit dibandingkan VM. Hal ini membuat proses booting dan eksekusi aplikasi menjadi sangat cepat.
  
- **Portabilitas Tinggi:**  
  Karena container mengemas semua dependensi aplikasi, aplikasi tersebut dapat dijalankan di berbagai lingkungan, baik itu di mesin lokal, di server, atau di cloud tanpa perlu konfigurasi ulang yang signifikan.
  
- **Skalabilitas:**  
  Container dapat dengan mudah diduplikasi atau dikurangi jumlahnya sesuai dengan permintaan trafik atau beban kerja. Teknologi ini mendukung arsitektur microservices di mana setiap layanan berjalan secara independen dan dapat diskalakan secara terpisah.
  
- **Isolasi yang Baik:**  
  Setiap container berjalan secara terpisah sehingga jika terjadi kegagalan pada satu aplikasi, container lain tidak akan terpengaruh. Isolasi ini juga meningkatkan keamanan karena proses yang berjalan di dalam container memiliki akses yang terbatas terhadap sumber daya sistem lainnya.

### 3.3 Perbandingan dengan Virtual Machine
Meskipun baik container maupun virtual machine sama-sama digunakan untuk mengisolasi aplikasi, keduanya memiliki perbedaan yang signifikan:
- **Virtual Machine:**  
  Memerlukan hypervisor dan setiap VM menjalankan sistem operasi tamu secara lengkap, sehingga overheadnya jauh lebih besar.
  
- **Container:**  
  Berbagi kernel host dan hanya menyertakan komponen yang diperlukan oleh aplikasi, membuatnya lebih ringan dan lebih cepat dalam hal deployment dan scaling.

Hal ini membuat container ideal untuk lingkungan cloud-native dan arsitektur microservices, di mana efisiensi dan portabilitas sangat krusial. Container juga memungkinkan pemanfaatan sumber daya secara optimal karena dapat menjalankan lebih banyak instance pada satu host dibandingkan dengan VM. Selain itu, container mendukung pengembangan berkelanjutan (continuous integration/continuous deployment atau CI/CD) dengan memungkinkan pembaruan dan rollback aplikasi dilakukan secara cepat tanpa perlu memulai ulang seluruh sistem.
