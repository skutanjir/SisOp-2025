<code>Sulistyo Fajar Pratama 3124500037</code></br>

<code>Ibnu Habib Ridwansyah 3124500041</code></br>

<code>Hafizh Hammas Muntazar 3124500060</code></br>

# Analisis dan Pembahasan Algoritma SJF Non-Preemptive Tanpa Arrival Time


Kami melakukan analisis terhadap implementasi algoritma Shortest Job First (SJF) non-preemptive tanpa mempertimbangkan waktu kedatangan (arrival time). SJF memilih proses dengan waktu eksekusi (burst time) paling kecil berikutnya. Karena tidak ada arrival time, semua proses dianggap tiba bersamaan pada waktu 0.

## 1. Kode Program

```c
#include <stdio.h>

struct proc {
    int no, bt, ct, tat, wt;
};

struct proc read(int i) {
    struct proc p;
    printf("\nProcess No: %d\n", i);
    p.no = i;
    printf("Enter Burst Time: ");
    scanf("%d", &p.bt);
    return p;
}

int main() {
    struct proc p[10], tmp;
    float avgtat = 0, avgwt = 0;
    int n, ct = 0;
    printf("<--SJF Scheduling Algorithm Without Arrival Time (Non-Preemptive)-->\n");
    printf("Enter Number of Processes: ");
    scanf("%d", &n);

    // Baca burst time tiap proses
    for (int i = 0; i < n; i++)
        p[i] = read(i + 1);

    // Pengurutan burst time (Bubble Sort)
    for (int i = 0; i < n - 1; i++)
        for (int j = 0; j < n - i - 1; j++)
            if (p[j].bt > p[j + 1].bt) {
                tmp = p[j];
                p[j] = p[j + 1];
                p[j + 1] = tmp;
            }

    // Header output
    printf("\nProcessNo\tBT\tCT\tTAT\tWT\tRT\n");

    // Hitung CT, TAT, WT, dan RT
    for (int i = 0; i < n; i++) {
        ct += p[i].bt;
        p[i].ct  = ct;
        p[i].tat = p[i].ct;        // Karena arrival = 0, TAT = CT
        p[i].wt  = p[i].tat - p[i].bt;
        avgwt   += p[i].wt;
        avgtat  += p[i].tat;
        printf("P%d\t\t%d\t%d\t%d\t%d\t%d\n",
               p[i].no, p[i].bt, p[i].ct, p[i].tat, p[i].wt, p[i].wt);
    }

    // Rata-rata
    avgtat /= n;
    avgwt  /= n;
    printf("\nAverage TurnAroundTime=%f\nAverage WaitingTime=%f",
            avgtat, avgwt);

    return 0;
}
```

### 1.1 Struktur Data

* `struct proc` menyimpan:

  * `no` (Process ID)
  * `bt` (Burst Time)
  * `ct` (Completion Time)
  * `tat` (Turnaround Time)
  * `wt` (Waiting Time)

### 1.2 Mekanisme Input

* Fungsi `read()` membaca burst time setiap proses.
* Semua proses dianggap tiba pada waktu 0.

### 1.3 Pengurutan (Scheduling)

* Menggunakan Bubble Sort untuk menempatkan proses dalam urutan naik berdasarkan `bt`.
* Kompleksitas: O(n²) untuk n proses.

### 1.4 Perhitungan Waktu

* **Completion Time (CT):** `ct = previous_ct + bt`
* **Turnaround Time (TAT):** `tat = ct - arrival_time`; di sini `arrival_time = 0`, maka `tat = ct`.
* **Waiting Time (WT):** `wt = tat - bt`.
* **Response Time (RT):** Pada non-preemptive dengan arrival sama, `rt = wt`.

## 2. Contoh Output Program

![image](https://github.com/ibnuhabibr/SisOp-2025/blob/main/img/Output%20Non-Preemptive%20SJF.png)

## 3. Analisis Hasil

1. **Urutan eksekusi:** Proses dengan BT terkecil (P3) dieksekusi terlebih dahulu, kemudian P2 dan P4 (keduanya BT=4, urutan input menentukan tie-break), terakhir P1.
2. **CT dan TAT:** CT monoton meningkat sesuai akumulasi BT. Karena arrival=0, TAT sama dengan CT.
3. **WT dan RT:** WT terbesar pada proses terakhir (P1) karena menunggu semua proses lain selesai. RT mengikuti WT di non-preemptive.
4. **Rata-rata:** SJF optimal dalam meminimalkan average waiting time dan turnaround time pada kasus tanpa arrival time.

## 4. Kompleksitas dan Pertimbangan

* **Sorting:** O(n²) — dapat dioptimalkan dengan algoritma seperti Quick Sort atau Min-Heap (O(n log n)).
* **Overhead:** Untuk jumlah proses besar, bubble sort tidak efisien.
* **Generalitas:** Tanpa arrival time, model ini sederhana; pada kasus nyata perlu menangani kedatangan proses.

## 5. Kelebihan dan Kekurangan

| Aspek              | Kelebihan                                                                               | Kekurangan                                                                 |
| ------------------ | --------------------------------------------------------------------------------------- | -------------------------------------------------------------------------- |
| Optimalitas        | Meminimalkan average waiting time dan turnaround time pada skenario tanpa arrival time. | Tidak mempertimbangkan waktu kedatangan proses; kurang realistis.          |
| Kompleksitas       | Implementasi sederhana: sorting + perhitungan waktu.                                    | Bubble Sort O(n²) tidak efisien untuk jumlah proses besar.                 |
| Responsivitas      | Proses dengan burst time kecil cepat dieksekusi.                                        | Proses dengan burst time panjang menunggu lama; potensi starvation.        |
| Generalisasi Kasus | Dasar untuk memperluas ke SJF dengan arrival time dan varian preemptive.                | Harus memodifikasi struktur algoritma untuk arrival time, preemption, dll. |

## 6. Kesimpulan

Implementasi SJF non-preemptive tanpa mempertimbangkan arrival time menghasilkan rata-rata TAT = 7.75 dan rata-rata WT = 3.75 untuk data input burst time \[7,4,1,4]. Urutan eksekusi proses berdasarkan waktu burst terbukti meminimalkan waktu tunggu secara keseluruhan. Untuk skala lebih besar, penggunaan algoritma pengurutan yang lebih efisien dan penanganan arrival time perlu dipertimbangkan.
