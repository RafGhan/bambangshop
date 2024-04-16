# BambangShop Publisher App
Tutorial and Example for Advanced Programming 2024 - Faculty of Computer Science, Universitas Indonesia

---

## About this Project
In this repository, we have provided you a REST (REpresentational State Transfer) API project using Rocket web framework.

This project consists of four modules:
1.  `controller`: this module contains handler functions used to receive request and send responses.
    In Model-View-Controller (MVC) pattern, this is the Controller part.
2.  `model`: this module contains structs that serve as data containers.
    In MVC pattern, this is the Model part.
3.  `service`: this module contains structs with business logic methods.
    In MVC pattern, this is also the Model part.
4.  `repository`: this module contains structs that serve as databases and methods to access the databases.
    You can use methods of the struct to get list of objects, or operating an object (create, read, update, delete).

This repository provides a basic functionality that makes BambangShop work: ability to create, read, and delete `Product`s.
This repository already contains a functioning `Product` model, repository, service, and controllers that you can try right away.

As this is an Observer Design Pattern tutorial repository, you need to implement another feature: `Notification`.
This feature will notify creation, promotion, and deletion of a product, to external subscribers that are interested of a certain product type.
The subscribers are another Rocket instances, so the notification will be sent using HTTP POST request to each subscriber's `receive notification` address.

## API Documentations

You can download the Postman Collection JSON here: https://ristek.link/AdvProgWeek7Postman

After you download the Postman Collection, you can try the endpoints inside "BambangShop Publisher" folder.
This Postman collection also contains endpoints that you need to implement later on (the `Notification` feature).

Postman is an installable client that you can use to test web endpoints using HTTP request.
You can also make automated functional testing scripts for REST API projects using this client.
You can install Postman via this website: https://www.postman.com/downloads/

## How to Run in Development Environment
1.  Set up environment variables first by creating `.env` file.
    Here is the example of `.env` file:
    ```bash
    APP_INSTANCE_ROOT_URL="http://localhost:8000"
    ```
    Here are the details of each environment variable:
    | variable              | type   | description                                                |
    |-----------------------|--------|------------------------------------------------------------|
    | APP_INSTANCE_ROOT_URL | string | URL address where this publisher instance can be accessed. |
2.  Use `cargo run` to run this app.
    (You might want to use `cargo check` if you only need to verify your work without running the app.)

## Mandatory Checklists (Publisher)
-   [X] Clone https://gitlab.com/ichlaffterlalu/bambangshop to a new repository.
-   **STAGE 1: Implement models and repositories**
    -   [X] Commit: `Create Subscriber model struct.`
    -   [X] Commit: `Create Notification model struct.`
    -   [X] Commit: `Create Subscriber database and Subscriber repository struct skeleton.`
    -   [X] Commit: `Implement add function in Subscriber repository.`
    -   [X] Commit: `Implement list_all function in Subscriber repository.`
    -   [X] Commit: `Implement delete function in Subscriber repository.`
    -   [X] Write answers of your learning module's "Reflection Publisher-1" questions in this README.
-   **STAGE 2: Implement services and controllers**
    -   [X] Commit: `Create Notification service struct skeleton.`
    -   [X] Commit: `Implement subscribe function in Notification service.`
    -   [X] Commit: `Implement subscribe function in Notification controller.`
    -   [X] Commit: `Implement unsubscribe function in Notification service.`
    -   [X] Commit: `Implement unsubscribe function in Notification controller.`
    -   [X] Write answers of your learning module's "Reflection Publisher-2" questions in this README.
-   **STAGE 3: Implement notification mechanism**
    -   [X] Commit: `Implement update method in Subscriber model to send notification HTTP requests.`
    -   [X] Commit: `Implement notify function in Notification service to notify each Subscriber.`
    -   [X] Commit: `Implement publish function in Program service and Program controller.`
    -   [X] Commit: `Edit Product service methods to call notify after create/delete.`
    -   [X] Write answers of your learning module's "Reflection Publisher-3" questions in this README.

## Your Reflections
This is the place for you to write reflections:

### Mandatory (Publisher) Reflections

#### Reflection Publisher-1
1. In the Observer pattern diagram explained by the Head First Design Pattern book, Subscriber is defined as an interface. Explain based on your understanding of Observer design patterns, do we still need an interface (or trait in Rust) in this BambangShop case, or a single Model struct is enough?

Pada Observer Pattern, penggunaan interface atau trait bergantung pada keragaman perilaku yang dimiliki oleh objek observer. Jika observer terdiri dari berbagai tipe atau jenis yang memerlukan penanganan yang berbeda, maka penggunaan interface atau trait menjadi relevan. Namun, dalam konteks BambangShop, dimana semua subscriber dianggap memiliki perilaku yang sama, penggunaan interface atau trait tidak diperlukan. Sebuah single model struct sudah cukup untuk menyimpan data observer dan perilaku yang diperlukan.

2. id in Program and url in Subscriber is intended to be unique. Explain based on your understanding, is using Vec (list) sufficient or using DashMap (map/dictionary) like we currently use is necessary for this case?

Dalam kasus di mana id dan URL harus unik, DashMap lebih cocok untuk digunakan dibandingkan Vec. Dengan DashMap, kita dapat menggunakan id produk atau URL pelanggan sebagai key karena keduanya dijamin unik, mengurangi kebutuhan untuk dua Vec terpisah. Operasi pengaksesan data dengan key pada DashMap juga lebih efisien, mempercepat proses secara keseluruhan dan memudahkan pemetaan langsung antara produk dan pelanggan. 

3. When programming using Rust, we are enforced by rigorous compiler constraints to make a thread-safe program. In the case of the List of Subscribers (SUBSCRIBERS) static variable, we used the DashMap external library for thread safe HashMap. Explain based on your understanding of design patterns, do we still need DashMap or we can implement Singleton pattern instead?

Dalam pemrograman Rust, meskipun compiler memberlakukan aturan yang ketat terhadap thread-safety, penggunaan DashMap masih tetap diperlukan untuk memastikan keamanan akses dan modifikasi data dalam lingkungan multithreading. Implementasi Singleton pattern dapat memastikan hanya ada satu instance SUBSCRIBER, namun kombinasi dengan DashMap diperlukan untuk menjaga keamanan dan konsistensi data di antara thread yang berbeda.


#### Reflection Publisher-2

1. In the Model-View Controller (MVC) compound pattern, there is no “Service” and “Repository”. Model in MVC covers both data storage and business logic. Explain based on your understanding of design principles, why we need to separate “Service” and “Repository” from a Model?

Memisahkan Service dan Repository dari Model mendukung prinsip-prinsip SOLID, khususnya Prinsip Single Responsibility (SRP). Dengan Service bertanggung jawab atas logika aplikasi dan Repository mengelola akses data, kode menjadi lebih modular dan mudah dikembangkan serta dipelihara. Penyekatan fungsionalitas ini ke dalam file terpisah memungkinkan setiap bagian kode mudah dibaca dan diedit, menghindari kompleksitas yang berlebihan dalam satu file.

2. What happens if we only use the Model? Explain your imagination on how the interactions between each model (Program, Subscriber, Notification) affect the code complexity for each model?

Jika kita tidak melakukan pemisahan antara Service dan Repository dari Model, kompleksitas kode akan meningkat karena business logic dan akses data bercampur, sulit dipahami, sulit diuji, dan sulit dipelihara. Ketergantungan antar bagian kode juga akan meningkat, menyulitkan perubahan tanpa memperhatikan dampaknya pada bagian lain. 

3. Have you explored more about Postman? Tell us how this tool helps you to test your current work. You might want to also list which features in Postman you are interested in or feel like it is helpful to help your Group Project or any of your future software engineering projects.

Sebelumnya, saya sudah pernah menggunakan Postman pada beberapa project di mata kuliah Pemrograman Berbasis Platform (PBP) untuk mengirimkan http request dengan suatu data pada body atau parameter ke suatu endpoint. Postman sangat membantu memastikan bahwa endpoint mampu menerima dan merespons data dengan benar, memberikan kemudahan dalam pengujian fungsionalitas keseluruhan aplikasi.


#### Reflection Publisher-3

1. Observer Pattern has two variations: Push model (publisher pushes data to subscribers) and Pull model (subscribers pull data from publisher). In this tutorial case, which variation of Observer Pattern that we use?

Pada tutorial ini, observer patter yang kita gunakan adalah push model untuk sistem notifikasi pada setiap proses add, delete, dan update. Jika Pengguna yang telah berlangganan melakukan proses tersebut, maka akan secara langsung menerima notifikasi dari publisher tanpa perlu meminta notifikasi tersebut.

2. What are the advantages and disadvantages of using the other variation of Observer Pattern for this tutorial case? (example: if you answer Q1 with Push, then imagine if we used Pull)

Jika kita menggunakan metode pull model untuk kasus Subscriber pada tutorial ini, keuntungan yang diperoleh adalah fleksibilitas dalam pemilihan data karena subscriber dapat mengambil informasi yang relevan sesuai kebutuhan. Selain itu, model ini memberi keleluasaan bagi subscriber untuk mengakses data tanpa perlu mengetahui detail dari publisher. Namun, kelemahannya adalah subscriber harus memiliki pengetahuan mengenai data yang ingin diambil serta waktu pengambilannya. Hal ini dapat meningkatkan kompleksitas karena memerlukan implementasi logika tambahan untuk pemilihan data yang diinginkan dan bisa menyebabkan redundansi jika data yang sama diambil berulang kali.

3. Explain what will happen to the program if we decide to not use multi-threading in the notification process.

Jika tidak menggunakan multi-threading, notifikasi akan dikirimkan secara berurutan dalam satu thread, menyebabkan penundaan jika ada pengamat yang membutuhkan waktu pemrosesan lama atau mengalami kesalahan. Kehadiran multi-threading memungkinkan pengiriman notifikasi kepada semua pelanggan secara asinkron, mengurangi waktu pengiriman dan menghindari keterlambatan yang disebabkan oleh pemrosesan berurutan.