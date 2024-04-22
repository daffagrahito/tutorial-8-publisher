### How many data your publisher program will send to the message broker in one run? 
Publisher programnya akan mengirimkan 5 messages ke message broker dalam satu run, karena tiap kali pemanggilan `p.publish_event` akan dikirimkan satu message dan ada 5 pemanggilan di dalam main methodnya.

### The url of: “amqp://guest:guest@localhost:5672” is the same as in the subscriber program, what does it mean?
URL `amqp://guest:guest@localhost:5672` diperlukan agar terhubung ke broker AMQP. Pada program publisher dan subscriber, URL tersebut menunjuk ke broker AMQP yang sama yang berjalan di mesin lokal. Penjelasan lebih detailnya seperti ini:
1. `amqp://` adalah protokol yang dipakai dan menunjukan koneksi akan dibuat menggunakan protokol AMQP.
2. `guest:guest@` pada `guest` pertama mewakili username untuk autentikasi, dan `guest` kedua mewakili kata sandi. Sama seperti penjelasan di subscriber bahwa username dan password secara default di RabbitMQ adalah `guest`.
3. `localhost` menunjukan nama host dari mesin di mana broker berjalan, yang berarti broker berjalan pada mesin yang sama di mana program publisher dan subscriber dijalankan.
4. `5672` Ini adalah port number (default) di mana broker AMQP listen connectionnya. 

Publisher dan subscriber harus connect ke broker yang sama untuk menerima dan mengirim *messages*, makanya mereka menggunakan connection string yang sama.

## Running RabbitMQ as message broker:
![Running RabbitMQ the first time](assets/images/RunningRabbitMQ.png)

## Sending and processing event:
![Sending Message Broker](assets/images/SendingMessageBroker.png)

Pada saat RabbitMQ berjalan dan kita menjalankan `cargo run` di publisher dan Subscriber, maka Publisher akan mengirimkan data berupa 5 event ke message broker yang diterima oleh Subscriber.

## Monitoring chart based on Publisher:
![Monitoring Chart based on Publisher](assets/images/MonitoringChart.png)

Apabila dijalankan `cargo run` berkali-kali selama satu detik lalu juga dalam satu detik hanya `cargo run` sekali, terlihat bahwa message ratesnya berbeda. Ini menunjukan kalau message rates meningkat saat message broker menerima data dari Publisher dan secepat apa message ratesnya disebabkan sebanyak apa message broker menerima data dari Publisher dalam satu detik.

## Simulation slow subscriber:
![Simulation Slow Subscriber](assets/images/SimulationSlowSubscriber.png)

Dari gambar tersebut terlihat bahwa Subscriber menjadi lambat dalam menerima data dari message broker yaitu dengan delay satu detik dalam setiap process. Queued messages pada message broker disini akan bertambah terus selagi bertambah juga delay karena Publisher dalam kasus ini akan lebih cepat daripada Subscriber sebagai penerima. Total banyak queue pada komputer saya disini adalah 20 dalam menjalakan 5 kali `cargo run` di Publisher.