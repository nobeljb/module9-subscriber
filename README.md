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