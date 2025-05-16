# Module 9 subscriber

## a. What is amqp?

AMQP (Advanced Message Queuing Protocol) adalah protokol komunikasi pesan yang dirancang untuk message-oriented middleware. AMQP memungkinkan aplikasi yang berbeda (yang mungkin ditulis dalam bahasa atau berjalan pada sistem yang berbeda) untuk saling bertukar pesan dengan cara yang andal, aman, dan terstruktur.

AMQP sering digunakan dalam sistem terdistribusi untuk menghubungkan layanan-layanan melalui message broker seperti RabbitMQ.

## b. What does it mean? guest:guest@localhost:5672 , what is the first guest, and what is the second guest, and what is localhost:5672 is for?

guest:guest@localhost:5672 adalah bagian dari URL koneksi ke message broker (misalnya RabbitMQ) menggunakan protokol AMQP.

- guest:guest  
  Ini adalah kombinasi username dan password untuk autentikasi.
    - guest pertama adalah username.
    - guest kedua adalah password.

- localhost:5672
    - localhost menunjukkan bahwa message broker (misalnya RabbitMQ) berjalan di mesin lokal.
    - 5672 adalah port default yang digunakan oleh RabbitMQ untuk menerima koneksi AMQP.

Secara keseluruhan, amqp://guest:guest@localhost:5672 berarti:  
Terhubung ke RabbitMQ yang berjalan di komputer lokal, pada port 5672, menggunakan username guest dan password guest.

## Simulation: Slow Subscriber

![img.png](img.png)!

Dalam simulasi ini, kami mensimulasikan subscriber yang lambat dengan menambahkan `thread::sleep(ten_millis);` pada `main.rs`. Akibatnya, pemrosesan setiap pesan oleh subscriber menjadi tertunda beberapa milidetik.

Kemudian kami menjalankan publisher beberapa kali secara cepat (`cargo run` berkali-kali). Hal ini menyebabkan antrean pesan di RabbitMQ meningkat, karena publisher terus mengirim pesan meskipun subscriber belum selesai memproses pesan sebelumnya.

Total pesan dalam antrean saya mencapai **21** karena:
- Setiap kali `cargo run` dijalankan di publisher, lima event dikirim ke antrean.
- Saya menjalankan publisher sebanyak **4 kali** secara cepat → 4 × 5 = **20** pesan.
- Kemungkinan 1 pesan tambahan berasal dari pengujian sebelumnya atau eksekusi yang tertunda.
- Subscriber memproses secara lambat → antrean menjadi penuh sesaat sebelum akhirnya pelan-pelan dikonsumsi subscriber.

Ini mencerminkan sistem real-time, di mana producer bisa terus bekerja meski consumer lambat, selama RabbitMQ tetap stabil dan pesan tidak kedaluwarsa.

## Run 3 subscriber consoles
![img_1.png](img_1.png)

## RabbitMQ
Antrean cepat turun saat tiga subscriber berjalan.

![img_2.png](img_2.png)

## 💡 Refleksi dan Penjelasan

Sebelumnya, saya mensimulasikan subscriber yang lambat dengan menambahkan delay (`thread::sleep`) pada kode subscriber. Ketika hanya satu subscriber yang berjalan, antrean pesan (queue) cepat menumpuk karena publisher mengirim pesan lebih cepat daripada subscriber memprosesnya.

Namun setelah saya menjalankan tiga subscriber secara paralel (masing-masing di jendela konsol berbeda), terlihat bahwa:

- Pesan dibagi secara otomatis oleh RabbitMQ menggunakan strategi round-robin.
- Setiap subscriber hanya menangani sebagian pesan.
- Pemrosesan menjadi lebih cepat karena berjalan paralel.
- Lonjakan antrean pesan lebih cepat terurai dibanding sebelumnya.

Ini mencerminkan prinsip arsitektur event-driven, di mana sistem dapat di-scale out secara horizontal dengan menambah jumlah subscriber agar beban bisa didistribusikan.
