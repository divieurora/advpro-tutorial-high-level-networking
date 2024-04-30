# Advance Programming Tutorial 9
Tutorial for Advanced Programming 2024 Module 9 - Faculty of Computer Science, Universitas Indonesia

---

## Reflection

1. What are the key differences between unary, server streaming, and bi-directional streaming RPC (Remote Procedure Call) methods, and in what scenarios would each be most suitable?

    Pada Unary RPC, client mengirimkan satu request ke server dan menerima satu response dari server. Unary RPC memiliki perilaku yang mirip dengan ketika mengirimkan pesan "Halo, apa kabar?" dan menerima balasan pesan "Halo, baik, terima kasih." Unary RPC cocok digunakan untuk autentikasi atau pengiriman form ke server.

    Pada Server Streaming RPC, client mengirim request ke server dan menerima stream message dari server sebagai response. Client akan membaca stream message tersebut sampai tidak ada message lagi. Server Streaming RPC cocok digunakan dalam skenario streaming video atau musik.

    Sedangkan pada Bi-directional RPC, client mengirim request dan server mengirim response. Ini memungkinkan client dan server mengirimkan message secara independent sehingga masing-masing client dan server dapat melakukan read and write dengan urutan yang bebas. Bi-directional RPC cocok digunakan dalam skenario bermain game online.

2. What are the potential security considerations involved in implementing a gRPC service in Rust, particularly regarding authentication, authorization, and data encryption?

    Rust gRPC untuk proses authentication, authorization, dan data encryption memerlukan proses yang berulang untuk setiap data yang dikirimkan. Hal ini yang membedakan dengan REST yang hanya perlu sekali validasi untuk setiap request karena setelah response dikirimkan, koneksi akan ditutup. Ketika mengimplementasikan gRPC, keduanya baik client maupun server harus melakukan encryption terpisah untuk menjaga data yang dikirimkan. Sehingga karena gRPC mengharuskan adanya proses uthentication, authorization, dan data encryption berulang-ulang dalam satu stream data maka tingkat keamanan secara keseluruhan meningkat.

3. What are the potential challenges or issues that may arise when handling bidirectional streaming in Rust gRPC, especially in scenarios like chat applications?

    Beberapa tantangan utama yang mungkin dapat muncul ketika melakukan handling bidirectional streaming di Rust gRPC terutama dalam skenario aplikasi chat adalah:

    - Message Synchronization, yaitu memastikan message yang dikirim oleh masing-masing client dan server tersinkronisasi dengan benar agar urutan proses tidak berantakan.
    - Scalability, yaitu memastikan bahwa implementasi dapat menangani jumlah client yang sangat besar.
    - Error Handling, yaitu menangani error dengan baik, contohnya ketika pesan gagal terkirim atau koneksi yang gagal terhubung.

4. What are the advantages and disadvantages of using the `tokio_stream::wrappers::ReceiverStream` for streaming responses in Rust gRPC services?

    Advantages:

    - Terintegrasi dengan Tokio yang merupakan runtime asynchronous Rust, sehingga cocok digunakan dalam aplikasi asynchronous.
    - Mudah mengubah receiver menggunakan ReceiverStream dari Tokio menjadi stream untuk memungkinkan onsumsi data asynchronous dengan mudah.

    Disadvantages:

    - ReceiverStream tergantung pada ekosistem Tokio sehingga ketika menggunakan ekosistem lain memerlukan kompatibilitas yang berbeda.
    - Menyebabkan potensi overhead serta kompleks dalam penggunaannya.

5. In what ways could the Rust gRPC code be structured to facilitate code reuse and modularity, promoting maintainability and extensibility over time?

    Rust gRPC memfasilitasi maintainability dan extensibility dengan penggunaan protobuf dan interface yang jelas. Protocol Buffers mendefinisikan message yang ditukar antara client dan server yang mengarah pada pembuatan interface yang dapat diimplementasi pada class di dalam Rust. Karena sudah didefinisikan dengan jelas, perubahan dapat dilakukan dengan memperbarui definisi protobuf dan mengimplementasikan perubahan tersebut di server, tanpa memerlukan perubahan di sisi client.

6. In the MyPaymentService implementation, what additional steps might be necessary to handle more complex payment processing logic?

    Beberapa hal yang mungkin dapat di-handle lebih baik pada MyPaymentService implementation antara lain:

    - Input Validation, seperti memastikan semua input adalah berupa informasi yang diperlukan.
    - Error handling, untuk menangani error yang mungkin terjadi ketika proses pembayaran.
    - Transaction Management, untuk memastikan konsistensi data selama proses pembayaran berjalan.

7. What impact does the adoption of gRPC as a communication protocol have on the overall architecture and design of distributed systems, particularly in terms of interoperability with other technologies and platforms?

    Penggunaan gRPC mempengaruhi cara perancangan dan development sistem karena penggunaan Protocol Buffers yang dapat memastikan data request-response dapat ditukar dengan maksimal.  Selain itu, gRPC memungkinkan komunikasi antar layanan tanpa bergantung pada bahasa pemrograman tertentu sehingga integrasinya dapat lebih mudah dilakukan. gRPC juga membantu dalam pengoptimalan kinerja sistem dengan mengurangi bandwith serta mempermudah data stream real-time. Secara umum, penggunaan gRPC membuat development distributed systems menjadi lebih efisien dan mudah dilakukan.

8. What are the advantages and disadvantages of using HTTP/2, the underlying protocol for gRPC, compared to HTTP/1.1 or HTTP/1.1 with WebSocket for REST APIs?

    Advantages:

    - HTTP/2 mendukung multiplexing yang memungkinkan beberapa request-response diproses secara langsung dalam satu koneksi TCP sehingga dapat mengurangi koneksi overhead.
    - HTTP/2 mendukung stream prioritization yang memungkinkan client untuk menentukan prioritas request yang berbeda sehingga aplikasi dapat mengoptimalkan penggunaan sumber daya.

    Disadvantages:

    - Dibandingkan dengan HTTP/1.1, HTTP/2 lebih kompleks dan memerlukan pehamaman terkait multiplexing, header compression, dan lainnya.
    - Beberapa environment masih menggunakan HTTP/1.1 karena lebih umum dan lebih disukai, beberapa environment juga kurang mendukung protocol HTTP/2.

9. How does the request-response model of REST APIs contrast with the bidirectional streaming capabilities of gRPC in terms of real-time communication and responsiveness?

    Request-response model di REST API memungkinkan client untuk mengirim request ke server dan kemudian server memberikan response kembali kepada client. Response yang dikirimkan server dapat berupa data tunggal atau collections of data yang diperlukan client. Hal ini artinya komunikasi antara client dan server real-time menjadi terputus karena client harus mengirimkan request baru setiap kali informasi diperbarui atau setiap selesai menerima response baru dari server.

    Sedangkan bi-directional streaming gRPC memungkinkan client dan server untuk saling bertukar message real-time melalui open streams. Sehingga dalam hal ini, client dan server dapat secara efektif dan efisien melakukan proses request-response secara bersamaan tanpa proses tunggu menunggu. Maka, gRPC secara umum lebih responsif dibandingkan REST API karena proses request-response dapat dilakukan secara real-time tanpa perlu menunggu request maupun response baru.

10. What are the implications of the schema-based approach of gRPC, using Protocol Buffers, compared to the more flexible, schema-less nature of JSON in REST API payloads?

    Rust gRPC yang menggunakan Protocol Buffers jika dibandingkan JSON pada REST API memiliki perbedaan yang cukup signifikan. Protocol Buffers menggunakan pengkodean binary sehingga hasil ukuran pesan lebih kecil dibandingkan JSON. Protocol Buffers juga memungkinkan integrasi di berbagai bahasa pemrograman dan platform karena komunikasinya tidak tergantung pada bahasa. Selain itu, Protocol Buffers juga memungkinkan developer untuk menghasilkan struktur data yang dapat memastikan konsistensi di antara implementasi client dan server.

    JSON dalam REST API mungkin lebih fleksibel dan lebih mudah digunakan, namun gRPC dengan Protocol Buffers lebih efisien, lebih mudah di-maintain, dan lebih aman dalam pemeliharaannya.