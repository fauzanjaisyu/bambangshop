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
-   [ ] Clone https://gitlab.com/ichlaffterlalu/bambangshop to a new repository.
-   **STAGE 1: Implement models and repositories**
    -   [ ] Commit: `Create Subscriber model struct.`
    -   [ ] Commit: `Create Notification model struct.`
    -   [ ] Commit: `Create Subscriber database and Subscriber repository struct skeleton.`
    -   [ ] Commit: `Implement add function in Subscriber repository.`
    -   [ ] Commit: `Implement list_all function in Subscriber repository.`
    -   [ ] Commit: `Implement delete function in Subscriber repository.`
    -   [ ] Write answers of your learning module's "Reflection Publisher-1" questions in this README.
-   **STAGE 2: Implement services and controllers**
    -   [ ] Commit: `Create Notification service struct skeleton.`
    -   [ ] Commit: `Implement subscribe function in Notification service.`
    -   [ ] Commit: `Implement subscribe function in Notification controller.`
    -   [ ] Commit: `Implement unsubscribe function in Notification service.`
    -   [ ] Commit: `Implement unsubscribe function in Notification controller.`
    -   [ ] Write answers of your learning module's "Reflection Publisher-2" questions in this README.
-   **STAGE 3: Implement notification mechanism**
    -   [ ] Commit: `Implement update method in Subscriber model to send notification HTTP requests.`
    -   [ ] Commit: `Implement notify function in Notification service to notify each Subscriber.`
    -   [ ] Commit: `Implement publish function in Program service and Program controller.`
    -   [ ] Commit: `Edit Product service methods to call notify after create/delete.`
    -   [ ] Write answers of your learning module's "Reflection Publisher-3" questions in this README.

## Your Reflections
This is the place for you to write reflections:

### Mandatory (Publisher) Reflections

#### Reflection Publisher-1

1. Pada modul dijelaskan bahwa penggunaan interface dilakukan apabila objek subscriber terdiri dari tipe yang beragam (contoh pada kasus Dog, Cat, Mouse), namun pada kasus bambangshop semua subscriber dianggap sama maka interface tidak diperlukan

2. Karena id pada product dan url pada subscriber dipastikan selalu unik, maka pengunaan DashMap akan lebih cocok dibanding Vec, variabel yang unik bisa dijadikan key dalam penggunaan DashMap, dan operasi pengaksesan data berdasarkan nilai key pada DashMap sangat efisien, biasanya memiliki kompleksitas O(1)

3. Walaupun singleton pattern menyediakan cara untuk mengakses satu instance dari kelas, implementasi dasarnya tidak secara otomatis thread-safe, oleh karena itu kita masih memerlukan DashMap untuk memastikan bahwa data yang diakses oleh thread yang berbeda tetap konsisten.

#### Reflection Publisher-2

1. Tujuan pemisahan antara service dan repository adalah untuk meningkatkan modularitas, fleksibilitas, dan keberlanjutan kode dengan mengurangi ketergantungan langsung antara logika bisnis dan akses data, membuat pengembangan, pengujian, dan pemeliharaan aplikasi menjadi lebih mudah.

2. Jika hanya menggunakan model, maka logika bisnis dan akses data akan bercampur, sehingga kode akan menjadi sulit dipahami, sulit diuji, dan sulit dipelihara. Setiap kali kita ingin mengubah bagian dari kode, kita harus memperhatikan bagaimana perubahan tersebut mempengaruhi bagian lain dari kode. Dengan menggunakan model, kita harus memperhatikan bagaimana model tersebut berinteraksi dengan model lainnya, sehingga kode akan menjadi lebih kompleks.

3. Postman sangat membantu dalam melakukan testing API, karena kita bisa mengirimkan request ke server tanpa harus membuat frontend terlebih dahulu, sehingga kita bisa fokus pada pengembangan backend terlebih dahulu.

#### Reflection Publisher-3

1. Pada kasus ini kita menggunakan push model, dimana subscriber akan menerima notifikasi dari publisher tanpa perlu meminta notifikasi tersebut, publisher akan secara aktif mengirimkan notifikasi ke subscriber.

2. Keuntungan dari menggunakan pull model adalah informasi subsciber lebih terjamin, namun kekurangannya adalah publisher harus mengetahui informasi subscriber, sehingga publisher harus melakukan polling secara berkala untuk mendapatkan informasi subscriber, hal ini akan mengakibatkan penggunaan resource yang lebih besar.

3. Jika kita tidak menggunakan multi-threading, maka notifikasi akan dikirimkan satu per satu, sehingga notifikasi akan dikirimkan secara berurutan, hal ini akan mengakibatkan waktu yang lebih lama untuk mengirimkan notifikasi ke semua subscriber, dan jika terdapat subscriber yang lambat dalam menerima notifikasi, maka notifikasi akan tertunda untuk subscriber selanjutnya.