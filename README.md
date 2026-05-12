Modul 5 – Real-Time Operating System (RTOS)
Identitas
Nama : Reva Aura Ramadhani
NIM : H1H024059
Mata Kuliah : Praktikum Sistem Mikrokontroler
Modul : Modul 5 – RTOS
Asisten : (isi nama asisten)
Deskripsi Praktikum  

Praktikum ini mempelajari konsep Real-Time Operating System (RTOS) menggunakan FreeRTOS pada Arduino Uno. Percobaan meliputi penerapan multitasking dan komunikasi antar task menggunakan queue.

Percobaan 5A – Multitasking
Tujuan

Memahami konsep multitasking pada sistem embedded menggunakan FreeRTOS dan menjalankan beberapa task secara concurrent.

Komponen
Arduino Uno
Breadboard
LED
Resistor
Kabel jumper
Penjelasan Program

Program menggunakan fungsi xTaskCreate() untuk membuat beberapa task:

Task 1 → Mengontrol LED pertama
Task 2 → Mengontrol LED kedua
Task 3 → Menampilkan counter pada Serial Monitor

Setiap task dijalankan scheduler FreeRTOS secara bergantian menggunakan vTaskDelay().

Hasil
LED dapat berkedip dengan interval berbeda
Counter tampil pada Serial Monitor
Multitasking berjalan dengan baik
Percobaan 5B – Komunikasi Task
Tujuan

Memahami komunikasi antar task menggunakan queue pada FreeRTOS.

Komponen
Arduino Uno
Sensor DHT
Breadboard
Kabel jumper
Penjelasan Program

Program menggunakan:

xQueueCreate() untuk membuat queue
xQueueSend() untuk mengirim data
xQueueReceive() untuk menerima data

Task pertama membaca data sensor, sedangkan task kedua menampilkan data pada Serial Monitor.

Hasil
Data suhu dan kelembaban berhasil dikirim antar task
Queue bekerja dengan baik
Tidak terjadi race condition

Insight Praktikum

Melalui praktikum ini dipahami bahwa RTOS memungkinkan beberapa proses berjalan secara concurrent dengan pengaturan scheduler. Selain itu, queue mempermudah komunikasi data antar task secara aman dan terstruktur.

Jawaban Pertanyaan Percobaan 5A – Multitasking
1. Apakah ketiga task berjalan secara bersamaan atau bergantian? Jelaskan mekanismenya!
Ketiga task berjalan secara bergantian dengan sangat cepat sehingga terlihat seperti berjalan bersamaan (concurrent). FreeRTOS menggunakan scheduler untuk mengatur pembagian waktu eksekusi setiap task. Ketika sebuah task menjalankan fungsi vTaskDelay(), task tersebut akan masuk ke kondisi delay sehingga processor dapat digunakan oleh task lain. Mekanisme ini disebut multitasking.

2. Bagaimana cara menambahkan task keempat? Jelaskan langkahnya!
Task keempat dapat ditambahkan dengan langkah berikut:
Membuat fungsi task baru, misalnya TaskBlink3().
Menambahkan deklarasi fungsi task pada bagian awal program.
Memanggil fungsi xTaskCreate() di dalam setup() untuk membuat task baru.
Menentukan parameter task seperti nama task, ukuran stack, prioritas, dan handle task.

Contoh:

void TaskBlink3(void *pvParameters);

xTaskCreate(
  TaskBlink3,
  "task4",
  128,
  NULL,
  1,
  NULL
);

3. Modifikasilah program dengan menambah sensor (misalnya potensiometer), lalu gunakan nilainya untuk mengontrol kecepatan LED! Bagaimana hasilnya?
Program dapat dimodifikasi dengan menambahkan potensiometer pada pin analog Arduino. Nilai analog yang dibaca menggunakan analogRead() digunakan untuk mengubah nilai delay LED. Hasilnya, kecepatan kedipan LED berubah sesuai posisi potensiometer. Semakin besar nilai potensiometer, maka LED berkedip semakin lambat atau semakin cepat tergantung program yang dibuat.

Jawaban Pertanyaan Percobaan 5B – Komunikasi Task
1. Apakah kedua task berjalan secara bersamaan atau bergantian? Jelaskan mekanismenya!
Kedua task berjalan secara bergantian berdasarkan pengaturan scheduler FreeRTOS. Scheduler akan memberikan waktu eksekusi kepada setiap task secara bergilir sehingga sistem terlihat berjalan bersamaan. Pergantian task terjadi sangat cepat sehingga proses pengiriman dan penerimaan data tampak berlangsung secara concurrent.

2. Apakah program ini berpotensi mengalami race condition? Jelaskan!
Program ini tidak mengalami race condition karena komunikasi data dilakukan menggunakan queue. Queue pada FreeRTOS bekerja sebagai media pertukaran data yang aman antar task sehingga akses data dilakukan secara sinkron. Dengan demikian, data tidak diakses secara bersamaan oleh dua task yang berbeda.

3. Modifikasilah program dengan menggunakan sensor DHT sesungguhnya sehingga informasi yang ditampilkan dinamis. Bagaimana hasilnya?
Program dapat dimodifikasi dengan menambahkan library DHT dan sensor DHT11/DHT22 pada Arduino. Task pembaca sensor akan membaca nilai suhu dan kelembaban secara langsung dari lingkungan, kemudian mengirimkannya ke queue. Task display akan menerima data tersebut dan menampilkannya pada Serial Monitor. Hasilnya, nilai suhu dan kelembaban berubah secara dinamis sesuai kondisi lingkungan sekitar secara real-time.

   
