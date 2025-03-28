# BambangShop Receiver App
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
4.  `repository`: this module contains structs that serve as databases.
    You can use methods of the struct to get list of objects, or operating an object (create, read, update, delete).

This repository provides a Rocket web framework skeleton that you can work with.

As this is an Observer Design Pattern tutorial repository, you need to implement a feature: `Notification`.
This feature will receive notifications of creation, promotion, and deletion of a product, when this receiver instance is subscribed to a certain product type.
The notification will be sent using HTTP POST request, so you need to make the receiver endpoint in this project.

## API Documentations

You can download the Postman Collection JSON here: https://ristek.link/AdvProgWeek7Postman

After you download the Postman Collection, you can try the endpoints inside "BambangShop Receiver" folder.

Postman is an installable client that you can use to test web endpoints using HTTP request.
You can also make automated functional testing scripts for REST API projects using this client.
You can install Postman via this website: https://www.postman.com/downloads/

## How to Run in Development Environment
1.  Set up environment variables first by creating `.env` file.
    Here is the example of `.env` file:
    ```bash
    ROCKET_PORT=8001
    APP_INSTANCE_ROOT_URL=http://localhost:${ROCKET_PORT}
    APP_PUBLISHER_ROOT_URL=http://localhost:8000
    APP_INSTANCE_NAME=Safira Sudrajat
    ```
    Here are the details of each environment variable:
    | variable                | type   | description                                                     |
    |-------------------------|--------|-----------------------------------------------------------------|
    | ROCKET_PORT             | string | Port number that will be listened by this receiver instance.    |
    | APP_INSTANCE_ROOT_URL   | string | URL address where this receiver instance can be accessed.       |
    | APP_PUUBLISHER_ROOT_URL | string | URL address where the publisher instance can be accessed.       |
    | APP_INSTANCE_NAME       | string | Name of this receiver instance, will be shown on notifications. |
2.  Use `cargo run` to run this app.
    (You might want to use `cargo check` if you only need to verify your work without running the app.)
3.  To simulate multiple instances of BambangShop Receiver (as the tutorial mandates you to do so),
    you can open new terminal, then edit `ROCKET_PORT` in `.env` file, then execute another `cargo run`.

    For example, if you want to run 3 (three) instances of BambangShop Receiver at port `8001`, `8002`, and `8003`, you can do these steps:
    -   Edit `ROCKET_PORT` in `.env` to `8001`, then execute `cargo run`.
    -   Open new terminal, edit `ROCKET_PORT` in `.env` to `8002`, then execute `cargo run`.
    -   Open another new terminal, edit `ROCKET_PORT` in `.env` to `8003`, then execute `cargo run`.

## Mandatory Checklists (Subscriber)
-   [X] Clone https://gitlab.com/ichlaffterlalu/bambangshop-receiver to a new repository.
-   **STAGE 1: Implement models and repositories**
    -   [X] Commit: `Create Notification model struct.`
    -   [X] Commit: `Create SubscriberRequest model struct.`
    -   [X] Commit: `Create Notification database and Notification repository struct skeleton.`
    -   [X] Commit: `Implement add function in Notification repository.`
    -   [X] Commit: `Implement list_all_as_string function in Notification repository.`
    -   [X] Write answers of your learning module's "Reflection Subscriber-1" questions in this README.
-   **STAGE 3: Implement services and controllers**
    -   [X] Commit: `Create Notification service struct skeleton.`
    -   [X] Commit: `Implement subscribe function in Notification service.`
    -   [X] Commit: `Implement subscribe function in Notification controller.`
    -   [X] Commit: `Implement unsubscribe function in Notification service.`
    -   [X] Commit: `Implement unsubscribe function in Notification controller.`
    -   [X] Commit: `Implement receive_notification function in Notification service.`
    -   [X] Commit: `Implement receive function in Notification controller.`
    -   [X] Commit: `Implement list_messages function in Notification service.`
    -   [X] Commit: `Implement list function in Notification controller.`
    -   [X] Write answers of your learning module's "Reflection Subscriber-2" questions in this README.

## Your Reflections
This is the place for you to write reflections:

### Mandatory (Subscriber) Reflections

#### Reflection Subscriber-1
1. Pada tutorial ini, kita menggunakan `RwLock<Vec<Notification>>` untuk memungkinkan _multiple thread_ membaca notifikasi secara bersamaan tanpa _blocking_, dengan tetap memastikan hanya satu thread yang dapat menulis pada satu waktu. Hal ini akan meningkatkan performa karena pembacaan lebih sering terjadi dibandingkan penulisan. Jika kita menggunakan `Mutex<Vec<Notification>>`, setiap operasi baca juga akan mengunci seluruh struktur data, yang mana dapat menyebabkan _bottleneck_ dan mengurangi efisiensi dalam lingkungan _multi-threaded_.

2. Rust tidak mengizinkan mutasi langsung pada variabel `static` seperti di Java untuk mencegah isu _race condition_ dalam lingkungan _multi-threaded_. Rust memerlukan mekanisme sinkronisasi seperti `Mutex` atau `RwLock` untuk mengelola mutasi pada variabel `static`, atau menggunakan `lazy_static!` untuk inisialisasi aman. Dengan pendekatan ini, Rust memastikan bahwa akses ke variabel global tetap aman tanpa menyebabkan masalah dalam eksekusi paralel.

#### Reflection Subscriber-2
1. Saya sudah mempelajari sekilas mengenai `src/lib.rs`. Dari yang saya pahami, `src/lib.rs` berfungsi sebagai modul utama proyek yang sedang kita bangun. file ini berisi import crate dan modul, struktur AppConfig, dan logika _error handling_.

2. Observer Pattern mempermudah penambahan `Subscriber` tanpa perlu mengubah kode publisher. Dengan multiple Receiver, setiap subscriber otomatis menerima notifikasi. Namun, jika ada lebih dari satu instance Main app, sinkronisasi state menjadi tantangan. Solusinya adalah menggunakan penyimpanan terpusat seperti database atau message broker agar semua instance berbagi daftar subscriber yang sama.

3. Saya telah mencoba membuat tes sendiri. Fitur ini sangat berguna untuk memastikan endpoint bekerja sesuai ekspektasi tanpa harus menguji secara manual setiap kali ada perubahan. Sementara itu, dokumentasi di Postman dapat membantu tim memahami API dengan lebih mudah, terutama dalam proyek kelompok, karena semua request dan respons sudah tertulis dalam dokumentasi dengan jelas.