**Tugas 6 Sistem Operasi**  
**Nama:** Sulistyo Fajar Pratama  
**NRP:** 3124500037  
**Dosen Pengajar:** Dr Ferry Astika Saputra ST, M.Sc  

---

### 1. Program Thread Sederhana (Compile Thread 3)

#### **Kode Program**
```c
#include <stdio.h>
#include <pthread.h>

pthread_cond_t cond1 = PTHREAD_COND_INITIALIZER;
pthread_cond_t cond2 = PTHREAD_COND_INITIALIZER;
pthread_cond_t cond3 = PTHREAD_COND_INITIALIZER;
pthread_mutex_t lock = PTHREAD_MUTEX_INITIALIZER;
int done = 1;

void *foo(void *arg) {
    int thread_num = *(int *)arg;
    while (1) {
        pthread_mutex_lock(&lock);
        while (done != thread_num) {
            if (thread_num == 1) pthread_cond_wait(&cond1, &lock);
            else if (thread_num == 2) pthread_cond_wait(&cond2, &lock);
            else pthread_cond_wait(&cond3, &lock);
        }
        printf("%d ", thread_num);
        fflush(stdout);

        if (done == 3) {
            done = 1;
            pthread_cond_signal(&cond1);
        } else if (done == 1) {
            done = 2;
            pthread_cond_signal(&cond2);
        } else {
            done = 3;
            pthread_cond_signal(&cond3);
        }
        pthread_mutex_unlock(&lock);
    }
    return NULL;
}

int main() {
    pthread_t tid1, tid2, tid3;
    int n1=1, n2=2, n3=3;
    pthread_create(&tid1, NULL, foo, &n1);
    pthread_create(&tid2, NULL, foo, &n2);
    pthread_create(&tid3, NULL, foo, &n3);

    pthread_join(tid1, NULL);
    pthread_join(tid2, NULL);
    pthread_join(tid3, NULL);
    return 0;
}
```

#### **Cara Menjalankan**
1. Buka terminal.
2. Buat file: `nano thread_123.c`.
3. Salin kode, simpan dengan `CTRL+O` dan `Enter`.
4. Compile: `gcc -pthread thread_123.c -o thread_123`.
5. Jalankan: `./thread_123`.

#### **Output**
```
1 2 3 1 2 3 1 2 3 ...
```
(Program berjalan terus hingga ditekan `CTRL+C`.)

#### **Analisa Kode**
- **Sinkronisasi**:
  - `pthread_mutex_t lock`: Memastikan hanya satu thread mengakses `done` secara bersamaan.
  - `pthread_cond_wait()`: Thread menunggu sinyal jika `done` tidak sesuai dengan ID-nya.
  - `pthread_cond_signal()`: Mengaktifkan thread berikutnya setelah `done` diubah.
- **Logika Giliran**:
  - Variabel `done` menentukan giliran thread (1 → 2 → 3 → 1...).
  - Setiap thread mencetak angka, mengubah `done`, lalu memberi sinyal ke thread selanjutnya.

---

### 2. Program Forking

#### **Kode Program**
```c
#include <stdio.h>
#include <unistd.h>

int main() {
    pid_t pid = fork();

    if (pid < 0) {
        perror("Fork gagal");
        return 1;
    } else if (pid == 0) {
        printf("Proses anak (PID: %d)\n", getpid());
    } else {
        printf("Proses induk (PID: %d) dengan anak PID: %d\n", getpid(), pid);
    }
    return 0;
}
```

#### **Cara Menjalankan**
1. Buka terminal.
2. Buat file: `nano forking.c`.
3. Salin kode, simpan dengan `CTRL+O` dan `Enter`.
4. Compile: `gcc forking.c -o forking`.
5. Jalankan: `./forking`.

#### **Output**
```
Proses induk (PID: 1234) dengan anak PID: 1235  
Proses anak (PID: 1235)
```
(Urutan output bisa berbeda tergantung eksekusi proses.)

#### **Analisa Kode**
- **Fungsi `fork()`**:
  - Membuat proses child yang identik dengan parent.
  - Return value:
    - `-1`: Gagal.
    - `0`: Proses child.
    - `>0`: PID child (di parent).
- **Alur**:
  - Parent dan child berjalan secara konkuren.
  - Parent mencetak PID diri dan child, sedangkan child mencetak PID diri.

---


### Soal Nomor 3  
**Pertanyaan:**  
*Apakah keuntungan menggunakan time quantum size di level yang berbeda dari sebuah antrian sistem multilevel?*

#### **Jawaban**
1. **Optimasi Jenis Proses**  
   - **Proses Interaktif (I/O-bound):** Time quantum kecil (misal 10ms) memastikan respons cepat.  
   - **Proses CPU-bound:** Time quantum besar (misal 100ms) mengurangi overhead context switching.  

2. **Pencegahan Starvation**  
   - Proses di level prioritas rendah tetap mendapat jatah CPU meski dieksekusi terakhir.  

3. **Maksimalkan Efisiensi**  
   - Mengurangi jumlah perpindahan proses tidak perlu, meningkatkan throughput sistem.  

4. **Adaptasi Beban Sistem**  
   - Saat sistem sibuk: Prioritaskan proses kritis (level atas).  
   - Saat sistem idle: Eksekusi proses batch (level bawah) secara maksimal.  

---

### Soal Nomor 4  
*Gambarkan 4 diagram Chart yang mengilustrasikan eksekusi dari proses-proses tersebut
menggunakan FCFS, SJF, prioritas nonpreemptive dan round robin.*    
**Tabel Proses:**  
| Proses | Burst Time | Prioritas |  
|--------|------------|-----------|  
| P1     | 10         | 3         |  
| P2     | 1          | 1         |  
| P3     | 2          | 3         |  
| P4     | 1          | 4         |  
| P5     | 5          | 2         |  

#### **Gantt Chart**  
**1. FCFS (First Come First Serve)**  
Urutan: P1 → P2 → P3 → P4 → P5  
```
P1 | P2 | P3 | P4 | P5  
0   10  11  13  14  19  
```  

**2. SJF Nonpreemptive**  
Urutan: P2 (1) → P4 (1) → P3 (2) → P5 (5) → P1 (10)  
```
P2 | P4 | P3 | P5 | P1  
0    1   2    4    9   19  
```  

**3. Prioritas Nonpreemptive**  
Urutan: P2 (1) → P5 (2) → P1 (3) → P3 (3) → P4 (4)  
```
P2 | P5 | P1 | P3 | P4  
0    1    6   16   18  19  
```  

**4. Round Robin (Quantum = 2)**  
Urutan eksekusi dengan pembagian waktu:  
```
P1 | P2 | P3 | P4 | P5 | P1 | P5 | P1 | P5 | P1 | P1  
0    2    3    5    6    8   10  12  13  15  17  19  
```  

---

**Penjelasan Singkat Algoritma:**  
- **FCFS**: Proses dieksekusi sesuai urutan kedatangan.  
- **SJF**: Proses dengan burst time terkecil didahulukan.  
- **Prioritas**: Proses dengan prioritas terkecil (tertinggi) dieksekusi pertama.  
- **Round Robin**: Setiap proses mendapat jatah waktu 2 unit, lalu di-switch ke proses berikutnya.
