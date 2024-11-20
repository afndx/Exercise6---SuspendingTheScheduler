# STM32 LED Blinking dengan FreeRTOS dan Contoh Critical Section
Proyek ini menunjukkan cara menggunakan mikrokontroler STM32 dengan FreeRTOS untuk mengelola beberapa tugas dalam mengontrol LED. Program ini juga mencakup simulasi penanganan critical section untuk memastikan eksklusi mutual saat mengakses data bersama, serta mensimulasikan operasi baca/tulis.


## Program ini disusun dengan dua tugas FreeRTOS:

- GreenLEDTask: Mengontrol LED hijau, yang menyala dan mati sambil mengakses data bersama di dalam critical section.
- RedLEDTask: Mengontrol LED merah, yang berkedip lebih cepat daripada LED hijau dan juga mengakses data bersama dalam critical section.
- Simulasi Critical Section: Kedua tugas memasuki critical section saat mengakses data bersama untuk mencegah modifikasi bersamaan, memastikan konsistensi data.
## Fitur Utama:
- LED Hijau: Berkedip setiap 500ms, dengan critical section untuk mensimulasikan akses data bersama.
- LED Merah: Berkedip setiap 100ms, juga dengan critical section.
- LED Biru: Menyala jika terjadi interferensi dalam akses data bersama (misalnya, jika dua tugas mencoba mengaksesnya secara bersamaan).
- Operasi Baca/Tulis yang Disimulasikan: Sebuah loop penundaan meniru operasi baca/tulis untuk mensimulasikan waktu pemrosesan di dalam critical section.

## Pengaturan Perangkat Keras
Program ini menggunakan tiga LED yang terhubung ke pin berikut dari STM32:

- LED Hijau: Terhubung ke GPIO_PIN_0
- LED Merah: Terhubung ke GPIO_PIN_1
- LED Biru: Terhubung ke GPIO_PIN_2
## Penjelasan Fungsi
- main()
  - Inisialisasi: Menginisialisasi pustaka HAL, mengonfigurasi clock sistem, dan menginisialisasi pin GPIO.
  - Pembuatan Tugas RTOS: Mendefinisikan dan membuat tugas FreeRTOS (defaultTask, GreenLEDTask, RedLEDTask).
  - Memulai Scheduler: Memulai kernel FreeRTOS, yang menangani pergantian tugas.
- green_led()
  - Mengontrol LED hijau, menyalakannya dan mematikannya sambil mengakses data bersama dalam critical section. Tugas ini ditunda selama 500ms setelah setiap iterasi.
- red_led()
  - Mengontrol LED merah, mirip dengan tugas LED hijau, tetapi dengan penundaan 100ms antara iterasi.
- accessSharedData()
  - Mensimulasikan akses data bersama dengan memeriksa startFlag. Jika kedua tugas mengaksesnya secara bersamaan, LED biru akan menyala untuk menunjukkan interferensi. Setelah melakukan operasi baca/tulis dummy (disimulasikan dengan penundaan), flag direset dan LED biru dimatikan.
- SimulateReadWriteOperation()
  - Sebuah fungsi dummy yang mensimulasikan operasi baca/tulis menggunakan loop penundaan untuk memperkenalkan penundaan, yang mewakili waktu pemrosesan.
- Error_Handler()
  - Sebuah fungsi untuk menangani kesalahan. Fungsi ini menonaktifkan interrupt dan memasuki loop tak terhingga jika terjadi kegagalan.
## Cara Kerja
- Kedipan LED: LED hijau dan merah berkedip pada laju yang berbeda. LED hijau berkedip setiap 500ms, dan LED merah berkedip setiap 100ms.
- Critical Section: Baik tugas LED hijau maupun merah mengakses data bersama (startFlag) dalam critical section untuk menghindari kondisi balapan. Critical section memastikan bahwa hanya satu tugas yang dapat mengakses data bersama pada satu waktu.
- LED Biru untuk Interferensi: Jika kedua tugas mengakses data bersama secara bersamaan, LED biru akan menyala sebagai indikator interferensi akses data.

## Demo 

https://github.com/user-attachments/assets/5e1df7e7-09bd-42ba-a5e6-b3067310e2e2

