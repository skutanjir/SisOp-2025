

---

# Multithreading: Contoh Penerapan, Perhitungan Kecepatan, Jenis Paralelisme, Perbedaan Thread, dan Context Switching Kernel

Soal ini menjelaskan dengan lengkap beberapa topik penting terkait multithreading, yakni:

1. **4.1** Tiga contoh kode pemrograman yang menunjukkan bagaimana multithreading bisa memberikan performa yang lebih cepat dibanding solusi single-threaded.  
2. **4.2** Perhitungan peningkatan kecepatan (speedup) menggunakan Hukum Amdahl untuk aplikasi dengan 60% bagian yang bisa diparalelkan, baik untuk dua core maupun empat core.  
3. **4.3** Penjelasan apakah web server multithreaded dari contoh 4.1 menerapkan task parallelism atau data parallelism.  
4. **4.4** Dua perbedaan utama antara user-level thread dan kernel-level thread, serta situasi di mana masing-masing lebih cocok digunakan.  
5. **4.5** Uraian langkah demi langkah tentang apa saja yang dilakukan kernel saat berpindah dari satu thread ke thread lainnya (context switching).

---

## 4.1 Contoh Penerapan Multithreading yang Lebih Cepat

Multithreading memungkinkan kita menjalankan banyak tugas secara bersamaan, sehingga program bisa lebih cepat. Berikut tiga contoh aplikasinya:

### Contoh 1: Web Server Multithreaded

Bayangkan sebuah server web yang harus melayani banyak klien sekaligus. Dengan multithreading, setiap koneksi klien di-handle oleh thread tersendiri. Jadi, jika satu koneksi lambat karena operasi I/O, koneksi lain tetap bisa segera ditangani.

```python
import threading
import socket

def tangani_klien(client_socket):
    request = client_socket.recv(1024)
    print(f"Menerima permintaan: {request.decode()}")
    # Mengirimkan respon sederhana
    response = b"HTTP/1.1 200 OK\r\nContent-Length: 13\r\n\r\nHello, world!"
    client_socket.send(response)
    client_socket.close()

def server():
    server_socket = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
    server_socket.bind(("0.0.0.0", 8080))
    server_socket.listen(5)
    print("Server mendengarkan pada port 8080...")
    
    while True:
        client, addr = server_socket.accept()
        thread_klien = threading.Thread(target=tangani_klien, args=(client,))
        thread_klien.start()

if __name__ == "__main__":
    server()
```

*Penjelasan:* Setiap kali ada permintaan dari klien, dibuat thread baru untuk mengurusnya. Jadi, satu permintaan tidak menunggu thread lain selesai—semuanya berjalan secara bersamaan.

### Contoh 2: Pemrosesan File Secara Paralel

Misalnya, kita punya aplikasi yang perlu memproses beberapa file besar. Jika file diproses satu per satu, waktu total akan lama. Dengan multithreading, masing-masing file bisa diproses bersamaan.

```python
import threading

def proses_file(nama_file):
    with open(nama_file, 'r') as file:
        data = file.read()
        # Lakukan pemrosesan data di sini
    print(f"File {nama_file} telah diproses")

daftar_file = ["file1.txt", "file2.txt", "file3.txt"]
threads = []

for file in daftar_file:
    thread = threading.Thread(target=proses_file, args=(file,))
    threads.append(thread)
    thread.start()

for thread in threads:
    thread.join()

print("Semua file telah diproses.")
```

*Penjelasan:* Masing-masing file diolah dalam thread yang berbeda. Jika satu file memerlukan waktu lebih lama, file lainnya tetap bisa diproses tanpa harus menunggu.

### Contoh 3: Pemrosesan Gambar Secara Paralel

Untuk aplikasi pengolahan gambar—misalnya menerapkan filter pada sekumpulan gambar—multithreading memungkinkan setiap gambar diproses secara bersamaan. Ini sangat berguna jika terdapat banyak gambar yang harus diolah.

```python
import threading
from PIL import Image, ImageFilter

def proses_gambar(path_input, path_output):
    gambar = Image.open(path_input)
    # Contoh: menerapkan filter sharpen pada gambar
    gambar_filter = gambar.filter(ImageFilter.SHARPEN)
    gambar_filter.save(path_output)
    print(f"Gambar {path_input} telah diproses")

daftar_gambar = [("img1.jpg", "out1.jpg"), ("img2.jpg", "out2.jpg"), ("img3.jpg", "out3.jpg")]
threads = []

for path_input, path_output in daftar_gambar:
    thread = threading.Thread(target=proses_gambar, args=(path_input, path_output))
    threads.append(thread)
    thread.start()

for thread in threads:
    thread.join()

print("Semua gambar telah diproses.")
```

*Penjelasan:* Tiap gambar diproses oleh thread terpisah. Dengan begitu, keseluruhan proses menjadi lebih cepat karena gambar tidak diolah satu per satu secara berurutan.

---

## 4.2 Perhitungan Speedup dengan Hukum Amdahl

Hukum Amdahl memberikan perkiraan berapa banyak peningkatan kecepatan (speedup) yang bisa dicapai ketika sebagian program dapat diparalelkan. Rumusnya adalah:

\[
S(n) = \frac{1}{(1-p) + \frac{p}{n}}
\]

di mana:
- \( p \) adalah bagian program yang bisa diparalelkan (di sini 0.6 atau 60%),  
- \( n \) adalah jumlah core pemroses,  
- \( (1-p) \) adalah bagian yang harus dieksekusi secara serial.

### Untuk Dua Core Pemroses

\[
S(2) = \frac{1}{(1-0.6) + \frac{0.6}{2}} = \frac{1}{0.4 + 0.3} = \frac{1}{0.7} \approx 1.43
\]

### Untuk Empat Core Pemroses

\[
S(4) = \frac{1}{(1-0.6) + \frac{0.6}{4}} = \frac{1}{0.4 + 0.15} = \frac{1}{0.55} \approx 1.82
\]

*Penjelasan:* Peningkatan performa tidak akan linier karena ada bagian dari program yang tetap harus dijalankan secara berurutan (bagian serial). Jadi, meskipun jumlah core bertambah, peningkatan speedup terbatas oleh bagian tersebut.

---

## 4.3 Jenis Paralelisme pada Web Server Multithreaded

Web server multithreaded yang sudah dijelaskan pada 4.1 menerapkan **paralelisme tugas (task parallelism)**.

- **Apa itu Paralelisme Tugas?**  
  Artinya setiap thread menjalankan tugas yang berbeda—misalnya, masing-masing menangani permintaan dari klien yang berbeda.

- **Di Web Server:**  
  Walaupun setiap thread menjalankan fungsi yang sama (menangani permintaan HTTP), yang berbeda adalah data atau klien yang mereka layani. Dengan demikian, tugas-tugas yang berbeda dikerjakan secara bersamaan.

---

## 4.4 Perbedaan antara User-Level Thread dan Kernel-Level Thread

Berikut dua perbedaan utama antara user-level thread dan kernel-level thread:

### 1. Pengelolaan dan Penjadwalan

- **User-Level Threads:**  
  - Dikelola sepenuhnya oleh pustaka di ruang pengguna.  
  - Kernel tidak melihat masing-masing thread, sehingga context switching (pergantian antar thread) biasanya lebih cepat dan ringan dari sisi overhead.

- **Kernel-Level Threads:**  
  - Dikelola oleh sistem operasi (kernel).  
  - Kernel bisa menjadwalkan thread pada berbagai core CPU, memberikan keuntungan pada sistem multicore meskipun context switching bisa lebih berat karena melibatkan kernel.

### 2. Perilaku terhadap Operasi Blocking

- **User-Level Threads:**  
  - Jika salah satu thread melakukan operasi blocking (misalnya, menunggu I/O), seluruh proses bisa saja terblokir karena kernel hanya melihat proses secara keseluruhan.  
  - Cocok untuk tugas-tugas yang tidak terlalu banyak operasi blocking.

- **Kernel-Level Threads:**  
  - Jika satu thread blocked, kernel bisa dengan segera menjadwalkan thread lain dari proses yang sama, menjaga agar aplikasi tetap responsif.  
  - Lebih ideal untuk aplikasi yang sering melakukan operasi I/O dan membutuhkan eksekusi paralel di sistem multicore.

**Kesimpulan:**

- **User-Level Threads** cocok untuk aplikasi dengan banyak tugas ringan dan jika operasi blocking dapat dihindari atau diminimalkan.
- **Kernel-Level Threads** lebih unggul untuk aplikasi yang membutuhkan penanganan I/O intensif dan pemanfaatan penuh CPU multicore.

---

## 4.5 Proses Context Switching Antar Kernel-Level Threads

Berikut adalah langkah-langkah yang dilakukan kernel saat berpindah dari satu thread ke thread lain (context switching):

1. **Menyimpan Konteks Thread yang Sedang Berjalan:**  
   Kernel menyimpan informasi penting seperti register CPU, program counter, dan status thread ke dalam struktur yang disebut Thread Control Block (TCB).

2. **Memperbarui Scheduler:**  
   Kernel mengubah status thread yang sedang berjalan (misalnya menjadi 'ready' atau 'waiting') dan memilih thread berikutnya yang akan dijalankan berdasarkan kebijakan penjadwalan.

3. **Memuat Konteks Thread Berikutnya:**  
   Kernel mengambil informasi yang tersimpan (register, program counter, dan lain-lain) dari TCB thread yang baru dipilih, lalu menempatkannya ke CPU.

4. **Mengganti Konteks Memori (Jika Diperlukan):**  
   Jika thread berikutnya berasal dari proses yang berbeda, kernel akan mengubah ruang alamat yang digunakan, yaitu dengan mengatur ulang unit manajemen memori (MMU). Jika thread itu milik proses yang sama, ruang alamat biasanya tetap sama.

5. **Melanjutkan Eksekusi:**  
   Setelah semua informasi penting telah dipulihkan, CPU akan melanjutkan eksekusi thread yang baru mulai dari titik terakhir ia berhenti.


---
