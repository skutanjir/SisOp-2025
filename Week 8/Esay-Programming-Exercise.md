# Penerapan Thread dalam Pemrograman

## Pendahuluan
Dalam dunia pemrograman, thread merupakan unit eksekusi terkecil yang memungkinkan tugas-tugas dapat berjalan secara paralel atau simultan. Esai ini membahas penerapan konsep threading menggunakan tiga pendekatan berbeda, yaitu dalam bahasa pemrograman Java, POSIX thread pada sistem operasi Linux, dan Win32 API pada Microsoft Windows, yang diilustrasikan melalui tiga program contoh yaitu `SumTask.java`, `thrd-posix.c`, dan `thrd-win32.c`.

## Penerapan Thread pada Java (SumTask.java)
Program `SumTask.java` menggunakan pendekatan fork/join parallelism, yang merupakan fitur khusus dalam Java untuk menangani tugas komputasi besar dengan membaginya ke dalam sub-tugas kecil. Program ini menghitung jumlah elemen-elemen dalam sebuah array. Jika ukuran tugas lebih kecil dari ambang batas tertentu, perhitungan dilakukan langsung dalam satu thread (tahap conquer). Sebaliknya, jika ukuran tugas besar, array dibagi menjadi dua bagian, masing-masing bagian dikerjakan oleh thread terpisah secara paralel (tahap divide). Hasil akhirnya diperoleh dengan menggabungkan hasil kedua thread. Model ini sangat efektif dalam mengoptimalkan penggunaan prosesor multi-core, dengan manajemen sumber daya yang sepenuhnya diatur oleh Java Virtual Machine (JVM).

## Penerapan Thread pada Linux (thrd-posix.c)
Pada sistem operasi Linux, thread diimplementasikan melalui POSIX threads (pthread). Dalam program `thrd-posix.c`, pthread digunakan untuk menghitung jumlah angka dalam thread terpisah. Thread dibuat menggunakan fungsi `pthread_create`, dan eksekusinya dimulai dari fungsi yang telah ditentukan (`runner`). Data global digunakan untuk menyimpan hasil akhir, menunjukkan sifat shared memory antar thread. Fungsi `pthread_join` digunakan agar proses utama menunggu hingga thread selesai bekerja sebelum melanjutkan eksekusi. POSIX threads menawarkan fleksibilitas tinggi di lingkungan Unix/Linux dan memiliki portabilitas antar platform Unix.

## Penerapan Thread pada Microsoft Windows (thrd-win32.c)
Dalam lingkungan Microsoft Windows, threading dilakukan melalui Win32 API menggunakan fungsi `CreateThread`. Program `thrd-win32.c` menunjukkan implementasi thread yang secara paralel menghitung jumlah angka dengan fungsi khusus (`Summation`). Variabel global digunakan untuk menyimpan hasil thread. Fungsi `WaitForSingleObject` digunakan agar thread utama menunggu thread tambahan menyelesaikan tugasnya sebelum melanjutkan proses. Pendekatan ini bersifat eksklusif untuk Windows, memberikan kendali penuh dan optimalisasi tinggi terhadap sistem operasi, namun tidak portabel ke platform lain.

## Perbandingan dan Kesimpulan
Ketiga pendekatan memiliki karakteristik unik masing-masing. Java menawarkan portabilitas tinggi dan abstraksi yang memudahkan pengelolaan sumber daya secara otomatis. POSIX threads memberikan solusi yang fleksibel dan portabel di lingkungan Unix/Linux dengan kontrol eksplisit terhadap thread. Win32 API menyediakan tingkat kendali tinggi khusus untuk Windows, sangat cocok untuk aplikasi yang membutuhkan optimasi kinerja dan interaksi yang erat dengan sistem operasi. Pemilihan pendekatan threading harus mempertimbangkan faktor seperti portabilitas, tingkat kontrol, kompleksitas implementasi, serta kebutuhan kinerja dari aplikasi yang dikembangkan.

