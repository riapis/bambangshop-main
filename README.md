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
-   [x] Clone https://gitlab.com/ichlaffterlalu/bambangshop to a new repository.
-   **STAGE 1: Implement models and repositories**
    -   [x] Commit: `Create Subscriber model struct.`
    -   [x] Commit: `Create Notification model struct.`
    -   [x] Commit: `Create Subscriber database and Subscriber repository struct skeleton.`
    -   [x] Commit: `Implement add function in Subscriber repository.`
    -   [x] Commit: `Implement list_all function in Subscriber repository.`
    -   [x] Commit: `Implement delete function in Subscriber repository.`
    -   [x] Write answers of your learning module's "Reflection Publisher-1" questions in this README.
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
1. In the Observer pattern diagram explained by the Head First Design Pattern book, Subscriber is defined as an interface. 
Explain based on your understanding of Observer design patterns, do we still need an interface (or trait in Rust) in this 
BambangShop case, or a single Model struct is enough?

    Jawaban: Dalam pola Observer, mengimplementasikan Subscriber sebagai antarmuka bertujuan untuk mengurangi ketergantungan 
Publisher terhadap kelas Subscriber konkret. Ini memungkinkan untuk membuat alternatif implementasi tanpa mengganggu kode 
yang sudah ada. Meskipun menggunakan Single Model Struct mungkin memadai dalam kasus BambangShop, menggunakan Subscriber 
sebagai antarmuka tetap menjadi pilihan yang lebih baik karena meningkatkan fleksibilitas dan kemudahan pemeliharaan program.

2. id in Program and url in Subscriber is intended to be unique. Explain based on your understanding, is using Vec (list) 
sufficient or using DashMap (map/dictionary) like we currently use is necessary for this case?

   Jawaban: Meskipun menggunakan Vec untuk menyimpan id pada Product dan url pada Subscriber bisa dilakukan dan memadai, 
   tetapi proses pencarian berdasarkan kunci (id dan url) akan memiliki kompleksitas yang lebih tinggi (O(N)). Sebagai alternatif 
   yang lebih disarankan, menggunakan DashMap akan mempercepat proses pencarian dengan kompleksitas O(1), sehingga meningkatkan 
   efisiensi program.

3. When programming using Rust, we are enforced by rigorous compiler constraints to make a thread-safe program. In the case 
of the List of Subscribers (SUBSCRIBERS) static variable, we used the DashMap external library for thread safe HashMap. 
Explain based on your understanding of design patterns, do we still need DashMap or we can implement Singleton pattern instead?

    Jawaban: Dalam konteks tersebut, menggunakan DashMap memungkinkan operasi pada variabel statis SUBSCRIBERS dapat dilakukan 
secara aman oleh beberapa thread secara bersamaan. Sebaliknya, meskipun Singleton Pattern berguna, tidak menjamin keamanan 
thread, terutama dalam situasi konkurensi. Oleh karena itu, memilih DashMap pada kasus tersebut menjadi opsi yang lebih terjamin keamanannya.

#### Reflection Publisher-2
1. In the Model-View Controller (MVC) compound pattern, there is no “Service” and “Repository”. Model in MVC covers both 
data storage and business logic. Explain based on your understanding of design principles, why we need to separate “Service” 
and “Repository” from a Model?

    Jawaban: Memisahkan "Service" dan "Repository" dari model merupakan aplikasi dari prinsip Separating Concerns dalam desain 
perangkat lunak. Ini sejalan dengan prinsip Single Responsibility Principle (SRP), yang menyatakan bahwa sebuah modul harus 
fokus pada satu fungsi tunggal. Dalam konteks ini, "Service" bertanggung jawab atas operasi logika bisnis, sementara "Repository" 
bertanggung jawab atas operasi akses data. Melalui pemisahan ini, dapat ditingkatkan kebutuhan non-fungsional yang sesuai 
dengan prinsip Desain yang Baik.

2. What happens if we only use the Model? Explain your imagination on how the interactions between each model (Program, Subscriber, Notification) 
affect the code complexity for each model?

    Jawaban: Jika hanya menggunakan Model, maka model akan menampung method-method yang mencakup semua fungsi, baik itu operasi 
logika bisnis maupun akses data. Akibatnya, interaksi langsung antara model-model akan terjadi, yang mungkin memerlukan 
penambahan method tambahan dalam model untuk mendukung interaksi tersebut. Seiring dengan perkembangan program, model akan 
menjadi terlalu "bloated" atau terlalu besar. Hal ini akan meningkatkan kompleksitas kode dan mengurangi modularitas serta 
kemudahan pemeliharaan.

3. Have you explored more about Postman? Tell us how this tool helps you to test your current work. You might want to also 
list which features in Postman you are interested in or feel like it is helpful to help your Group Project or any of your 
future software engineering projects?

    Jawaban: Postman adalah alat yang tak ternilai dalam pengembangan perangkat lunak. Saya sering menggunakan Postman untuk 
mengirimkan HTTP Request ke server yang telah dibuat, memastikan bahwa responsnya sesuai dengan yang diharapkan. Selain itu, 
Postman juga menawarkan fitur pembuatan Dokumentasi API yang sangat berguna. Saya yakin fitur ini akan sangat membantu dalam 
proyek kelompok kami, memungkinkan kami membuat dokumentasi API secara lebih terstruktur dan rapi.

#### Reflection Publisher-3
