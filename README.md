## Reflection 
1. What are the key differences between unary, server streaming, and bi-directional streaming RPC (Remote Procedure Call) methods, and in what scenarios would each be most suitable?
    - Unary RPC
        - Pada Unary RPC, client akan mengirimkan sebuah request ke server dan akan menerima sebuah response (satu request menerima satu response saja) dan selesai. 
        - Biasanya digunakan untuk mengambil data dari server, operasi CRUD pada database, sistem authentication, dll
    - Server Streaming RPC
        - Pada Server Streaming RPC, client akan mengirimkan sebuah request ke server dan akan menerima responses. Server akan mengirimkan response terus menerus sampai stream selesai
        - Biasanya digunakan untuk skenario dimana client membutuhkan data terus menerus dari server, seperti log streaming, data update secara real time, dll
    - Bi-Directional Streaming RPC
        - Pada Bi-Directional Streaming RPC, client mengirimkan request terus menerus dan server mengirimkan response terus menerus secara bersamaan.
        - Cocok digunakan untuk skenario di mana client dan server perlu untuk saling mengirimkan data terus menerus, seperti aplikasi chat, alat kolaborasi, dll

1. What are the potential security considerations involved in implementing a gRPC service in Rust, particularly regarding authentication, authorization, and data encryption?
    - Untuk authentication dan authorization, kita perlu untuk melakukan validasi berkali-kali karena tidak bersifat unary. Encryption juga demikian, kita perlu untuk melakukan enkripsi berkali-kali.  

1. What are the potential challenges or issues that may arise when handling bidirectional streaming in Rust gRPC, especially in scenarios like chat applications?
    - Ketika mengimplementasikan Bi-Directional Streaming, terdapat beberapa tantangan yang dapat timbul:
        - Concurrency dan synchronization
            - Karena dalam BiDirectional Streaming, client dan server mengirimkan data dalam waktu bersamaan, maka muncul potensi terjadi race condition dan deadlock. Oleh karena itu, perlu dilakukan pencegahan agar tidak terjadi race condition dan deadlock
        - Flow control
            - Dalam satu stream, client dan server dapat mengirimkan banyak sekali data secara terus menerus. Jika ternyata terdapat banyak stream, server dapat menjadi kewalahan dalam mengelola semua request yang ada. Oleh karena itu, kita harus memastikan agar server dapat menghandle beban request yang sangat banyak sekali.

1. What are the advantages and disadvantages of using the tokio_stream::wrappers::ReceiverStream for streaming responses in Rust gRPC services?
    - Keuntungan:
        - Dapat menghandle stream yang banyak secara asinkronus sehingga tidak menghambat execution thread
        - Memudahkkan untuk mengintegrasikan dengan streaming service
        - Memudahkan untuk mengirimkan data langsung ketika data tersedia
    - Kerugian:
        - Menambah kompleksitas untuk aplikasi
        - Jika tidak dihandle untuk maksimal beban server, server dapat menjadi kesulitan untuk mengmaintain alur data
        - Jika tidak dicegah dapat menimbulkan race condition 

1. In what ways could the Rust gRPC code be structured to facilitate code reuse and modularity, promoting maintainability and extensibility over time?
    - Kita dapat mengorganisasikan kode gRPC menjadi sebuah hierarki seperti memisahkan antara business logic, data access, implementasi service, dll agar lebih maintainable
    - Menerapkan SOLID principle untuk maintainability
    - Menerapkan proto untuk memudahkan extention bagi aplikasi

1. In the MyPaymentService implementation, what additional steps might be necessary to handle more complex payment processing logic?
    - Dapat diubah implementasinya misalkan menjadi server streaming sehingga client cukup mengirim sebuah request tetapi server akan mengirimkan data terus-menerus.

1. What impact does the adoption of gRPC as a communication protocol have on the overall architecture and design of distributed systems, particularly in terms of interoperability with other technologies and platforms?
    - Pada saat kita menggunakan gRPC, kita dapat mempermudah konektivitas dan operasi antar teknologi karena di dalam gRPC, jika terjadi pemanggilan method, maka kita tidak perlu membuat HTTP Method untuk method tersebut, tetapi hal itu sudah diatur secara otomatis melalui file proto.

1. What are the advantages and disadvantages of using HTTP/2, the underlying protocol for gRPC, compared to HTTP/1.1 or HTTP/1.1 with WebSocket for REST APIs?
    - Keuntungan:
        - Meningkatkan efisiensi dengan mengurangi koneksi-koneksi yang dibutuhkan
        - Memungkinkan untuk mengirimkan request dan response tanpa perlu mematikan koneksi
    - Kerugian: 
        - Performance dan memory dapat menjadi tidak efisien karena dapat mempertahankan koneksi untuk banyak request dan response

1. How does the request-response model of REST APIs contrast with the bidirectional streaming capabilities of gRPC in terms of real-time communication and responsiveness?
    - REST APIs hanya mengikuti sistem unary, di mana satu request akan dibalas dengan satu response saja, berbeda dengan gRPC yang mengikuti sistem bidirectional, di mana banyak request akan dibalas dengan banyak request secara terus menerus, tidak hanya sekali saja.
    - gRPC lebih mendukung kasus real time dibandingkan REST APIs
    - REST APIs dapat terdampak oleh koneksi jaringan dikarenakan request-response model, sedangkan gRPC yang memanfaatkan bidirectional streaming memungkinkan untuk lebih responsif

1. What are the implications of the schema-based approach of gRPC, using Protocol Buffers, compared to the more flexible, schema-less nature of JSON in REST API payloads?
    - Dengan menggunakan Protocol Buffer, gRPC dan Protocol Buffer nantinya akan membuat sebuah interface baru di mana membantu untuk mengserialize dan mengdeserialize keluar masuknya pesan secara otomatis sehingga data yang keluar masuk tersebut akan terjamin.
