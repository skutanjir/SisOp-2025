
## a. Java dengan Fork/Join (SumTask.java)

Ketika kita memiliki kumpulan data besar—misal 10.000 bilangan acak—menjumlahkannya dengan satu thread tunggal bisa jadi lambat. **Fork/Join** di Java memecah tugas besar menjadi tugas‐tugas kecil, menjalankannya paralel, lalu menggabungkan hasilnya. Ini sangat berguna pada prosesor multicore.

1. **ForkJoinPool**  
   - Merupakan executor khusus yang mengelola antrean tugas dan thread‐thread pekerja.  
   - Menggunakan algoritma *work‐stealing*: jika satu thread kehabisan tugas, ia “mencuri” tugas dari antrean thread lain, sehingga beban kerja tersebar seimbang.

2. **RecursiveTask**  
   - Kelas abstrak yang memodelkan tugas yang menghasilkan nilai (di sini `Integer`).  
   - Kita override `compute()`: di sinilah logika divide‐and‐conquer terjadi.

3. **Threshold**  
   - Batas ukuran subtugas di mana kita hentikan pembagian lagi.  
   - Jika `end – begin < THRESHOLD`, berarti subtugas sudah kecil, kita langsung hitung dengan loop biasa—menghindari overhead pembuatan thread berlebihan.

4. **Fork & Join**  
   - `left.fork()` memulai subtugas kiri di thread pekerja, lalu `right.fork()` untuk kanan.  
   - `join()` menunggu penyelesaian dan mengembalikan hasil.  
   - Urutan `fork()` dan `join()` bisa memengaruhi performa—biasanya lebih efisien memanggil `fork()` pada kedua subtugas, lalu `join()` dua‐dua.

```java
// SumTask.java
import java.util.concurrent.*;

public class SumTask extends RecursiveTask<Integer> {
    static final int SIZE      = 10000;
    static final int THRESHOLD = 1000;

    private int begin, end;
    private int[] array;

    public SumTask(int begin, int end, int[] array) {
        this.begin = begin;
        this.end   = end;
        this.array = array;
    }

    @Override
    protected Integer compute() {
        if (end - begin < THRESHOLD) {
            // Subtugas cukup kecil: langsung hitung
            int sum = 0;
            for (int i = begin; i <= end; i++) {
                sum += array[i];
            }
            return sum;
        } else {
            // Bagi jadi dua subtugas
            int mid = begin + (end - begin) / 2;
            SumTask left  = new SumTask(begin, mid, array);
            SumTask right = new SumTask(mid + 1, end, array);

            // Jalankan paralel
            left.fork();
            right.fork();

            // Tunggu & gabungkan
            return left.join() + right.join();
        }
    }

    public static void main(String[] args) {
        // Siapkan pool sesuai jumlah core (default)
        ForkJoinPool pool = new ForkJoinPool();
        int[] array = new int[SIZE];
        java.util.Random rand = new java.util.Random();

        // Isi data
        for (int i = 0; i < SIZE; i++) {
            array[i] = rand.nextInt(10);
        }

        // Kick‐off task
        SumTask task = new SumTask(0, SIZE - 1, array);
        int sum = pool.invoke(task);

        System.out.println("The sum is " + sum);
    }
}
```

**Jalankan:**  
```bash
javac SumTask.java
java SumTask
```  
> Hasilnya akan menampilkan `The sum is <angka>`; ambil screenshot dari konsol.

---

## b. Linux dengan POSIX Threads (thrd-posix.c)

**POSIX threads** (pthreads) memberi kontrol manual: kamu menentukan atribut thread, memulai, lalu menyinkronkan sendiri. Contoh di bawah ini:

1. **Variabel global**  
   - `int sum` dibagikan antar‐thread.  
   - Perlu hati‐hati race condition jika ada banyak thread menulis; di contoh ini hanya satu thread worker, jadi aman.

2. **`pthread_create`**  
   - Parameter:  
     1. Pointer ke `pthread_t` (untuk menyimpan ID thread).  
     2. Atribut (bisa default via `pthread_attr_init`).  
     3. Fungsi runner yang dijalankan thread.  
     4. Argumen untuk runner (di‐cast ke `void *`).

3. **`pthread_join`**  
   - Menyebabkan thread utama menunggu hingga thread worker selesai.

4. **Error checking**  
   - Pastikan argumen valid (>=0) agar runner tak dipanggil dengan data negatif.

```c
// thrd-posix.c
#include <pthread.h>
#include <stdio.h>
#include <stdlib.h>

int sum;  // shared resource

void *runner(void *param) {
    int upper = atoi(param);
    sum = 0;
    for (int i = 1; i <= upper; i++) {
        sum += i;
    }
    pthread_exit(0);
}

int main(int argc, char *argv[]) {
    if (argc != 2 || atoi(argv[1]) < 0) {
        fprintf(stderr, "Usage: %s <non-negative integer>\n", argv[0]);
        return -1;
    }
    pthread_t tid;
    pthread_attr_t attr;
    pthread_attr_init(&attr);

    // create thread and run runner
    pthread_create(&tid, &attr, runner, argv[1]);

    // wait for it to finish
    pthread_join(tid, NULL);

    printf("sum = %d\n", sum);
    return 0;
}
```

**Jalankan:**  
```bash
gcc thrd-posix.c -lpthread -o posix_sum
./posix_sum 10
```  
> Pastikan muncul `sum = 55`; screenshot di terminal-mu.

---

## c. Windows dengan Win32 API (thrd-win32.c)

Win32 threading memakai **HANDLE** dan fungsi‐fungsi API:

1. **`CreateThread`**  
   - Parameter utama:  
     - Keamanan thread (bisa `NULL` untuk default).  
     - Ukuran stack (0 = default).  
     - Alamat fungsi worker (`Summation`).  
     - Argumen pointer (`&Param`).  
     - Flags (0 langsung berjalan).  
     - Penyimpanan ID thread.

2. **`WaitForSingleObject`**  
   - Menunggu hingga thread signaled (selesai).  
   - `INFINITE` artinya tanpa batas waktu timeout.

3. **`CloseHandle`**  
   - Membersihkan handle setelah selesai.

```c
// thrd-win32.c
#include <windows.h>
#include <stdio.h>

DWORD Sum;  // shared data

DWORD WINAPI Summation(PVOID Param) {
    DWORD upper = *(DWORD *)Param;
    Sum = 0;
    for (DWORD i = 0; i <= upper; i++) {
        Sum += i;
    }
    return 0;
}

int main(int argc, char *argv[]) {
    if (argc != 2) {
        fprintf(stderr, "An integer parameter is required\n");
        return -1;
    }
    DWORD Param = atoi(argv[1]);
    if ((int)Param < 0) {
        fprintf(stderr, "An integer >= 0 is required\n");
        return -1;
    }

    DWORD ThreadId;
    HANDLE hThread = CreateThread(
        NULL,      // default security
        0,         // default stack
        Summation, // worker function
        &Param,    // argumen
        0,         // flag: run immediately
        &ThreadId  // thread ID
    );

    if (hThread) {
        WaitForSingleObject(hThread, INFINITE);
        CloseHandle(hThread);
        printf("sum = %lu\n", Sum);
    }
    return 0;
}
```

**Jalankan (VS Dev Prompt):**  
```bat
cl thrd-win32.c
thrd-win32.exe 10
```  
> Setelah `sum = 55` muncul, langsung screenshot output-nya.

