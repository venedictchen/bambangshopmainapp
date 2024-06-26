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
-   [v] Clone https://gitlab.com/ichlaffterlalu/bambangshop to a new repository.
-   **STAGE 1: Implement models and repositories**
    -   [v] Commit: `Create Subscriber model struct.`
    -   [v] Commit: `Create Notification model struct.`
    -   [v] Commit: `Create Subscriber database and Subscriber repository struct skeleton.`
    -   [v] Commit: `Implement add function in Subscriber repository.`
    -   [v] Commit: `Implement list_all function in Subscriber repository.`
    -   [v] Commit: `Implement delete function in Subscriber repository.`
    -   [v] Write answers of your learning module's "Reflection Publisher-1" questions in this README.
-   **STAGE 2: Implement services and controllers**
    -   [v] Commit: `Create Notification service struct skeleton.`
    -   [v] Commit: `Implement subscribe function in Notification service.`
    -   [v] Commit: `Implement subscribe function in Notification controller.`
    -   [v] Commit: `Implement unsubscribe function in Notification service.`
    -   [v] Commit: `Implement unsubscribe function in Notification controller.`
    -   [v] Write answers of your learning module's "Reflection Publisher-2" questions in this README.
-   **STAGE 3: Implement notification mechanism**
    -   [v] Commit: `Implement update method in Subscriber model to send notification HTTP requests.`
    -   [v] Commit: `Implement notify function in Notification service to notify each Subscriber.`
    -   [v] Commit: `Implement publish function in Program service and Program controller.`
    -   [v] Commit: `Edit Product service methods to call notify after create/delete.`
    -   [v] Write answers of your learning module's "Reflection Publisher-3" questions in this README.

## Your Reflections
This is the place for you to write reflections:

### Mandatory (Publisher) Reflections

#### Reflection Publisher-1
1. Untuk kasus ini, kita tidak memerlukan interface/trait karena observer hanya berupa satu class, yaitu Subscriber sebagai observer. Kita memerlukan interface/trait saat kita memiliki observer yang banyak jenis berupa class-class yang beragam.

2. Penggunaan Dashmap akan mempermudah kita untuk menyimpan data masing-masing produk beserta subscribernya. Jika kita hanya menggunakan vektor, maka kita akan memerlukan 2 vektor dan proses query data akan lebih sulit.

3. Dashmap adalah built-in data structure yang berguna untuk multithreading. Pada kasus ini, aplikasi bambangshop menggunakan multithreading yang mana Map Subscriber akan diakses oleh banyak thread. Lalu, Singleton memiliki fungsi untuk memastikan selama program berjalan hanya akan ada 1 instance dari objek tersebut. Hal ini berfungsi agar daftar subscriber terhadap produk hanya berada pada 1 dashmap.

#### Reflection Publisher-2
1. Dengan memisahkan Service dari Repository, maka kita akan menerapkan prinsip SRP(Single Responsibility Principle). Service berfungsi untuk mendapatkan dan mengolah data, sedangkan Repository berfungsi untuk akses dan penyimpanan data ke dalam penyimpanan, seperti database. Dengan memisahkan Service dan Repository, setiap komponen memiliki tanggung jawab yang jelas dan terpisah, memungkinkan modifikasi atau perubahan fungsi masing-masing tanpa mengganggu fungsi lainnya. Ini membuat kode menjadi lebih terstruktur, mudah dipahami, dan lebih mudah untuk di-maintain.

2. Jika kita hanya menggunakan model tanpa layer lain, maka akan terjadi coupling yang tinggi. Jika dilakukan perubahan, maka hal itu dapat memengaruhi bagian code yang lain sehingga bagian code yang lain juga harus dilakukan perubahan.

3. Postman dapat digunakan untuk testing endpoint dari aplikasi kita. Postman dapat berfungsi untuk kita melihat behaviour dari aplikasi kita apakah sesuai dengan yang diharapkan. Pada Postman kita dapat mengirim berbagai macam method, memasukkan credential, form dan lain sebagainya. Postman juga akan memberikan response. Hal ini tentunya sangat berguna karena kita dapat mencoba akses endpoint kita sesuai yang kita harapkan atau tidak.

#### Reflection Publisher-3
1. Pada kasus ini, kita menggunakan `Push Model` yang mana saat terjadi create, delete, dan update akan dipanggil `NotificationService` yang akan mengiterasi semua subscriber dan memberikan update terbaru.

2. Keuntungan jika menggunakan pull adalah program akan lebih efisien karena hanya akan memberikan data saat subscriber membutuhkan. Observer juga dapat menentukan pilihan mereka sendiri apakah perubahan data yang ada relevan. Namun, konsiderasi untuk metode pull adalah observer menjadi perlu untuk mengetahui struktur dari data sumbernya agar bisa melakukan hal tersebut.

3. Apabila kita tidak melakukan multithreading, maka yang akan terjadi adalah akan terjadi antrian panjang karena NotificationService perlu notify tiap subscribernya satu-satu yang bisa menyebabkan pengiriman notifiksi terhambat karena bottleneck dengan komputasi.
