## Commit 1 Reflection Notes

Method `handle_connection()` berguna untuk menghandle koneksi TCP. `buf_reader` akan membaca data yang berasal dari *TCP stream* sebagai `Vector` sampai menerima *empty line* yang menandakan akhir dari *header* HTTP. Setelah itu method ini akan menge-*print* seluruh HTTP *request* tersebut.

## Commit 2 Reflection Notes

![Commit 2 screen capture](/images/commit2.png)

Method `handle_connection()` pada commit ini menerima *ownership* `TcpStream` yang direpresentasikan oleh `stream`. Selanjutnya pada method ini dibuat `Buffered Reader` yang berguna untuk membaca data dari TCP stream dan kemudian disimpan pada variabel `http_request`. Setelah menerima request tersebut, method `handle_connection()` akan mengirim response yang berupa file `hello.html`, `stream.write_all(response.as_bytes()).unwrap();` berguna untuk mengirim kembali response ke client menggunakan TCP stream. 

## Commit 3 Reflection Notes

![Commit 3 screen capture](/images/commit3.png)

Method `handle_connection()` pada commit ini conditional tergantung dari baris pertama dari HTTP request, jika baris pertama yang dibaca adalah `GET / HTTP/1.1` yang menandakan berusaha mengambil data dari `root` server maka method ini akan mengembalikan isi file `hello.html`. Namun jika request tersebut berusaha mengambil data dari path selain `root` server seperti `GET /something-else HTTP/1.1` maka method ini akan mengembalikan isi file `404.html`.

Pada bagian ini dilakukan refactoring karena implementasi sebelumnya mengakibatkan adanya bagian kode yang *redundant* yaitu pada bagian *conditional* di mana pada implementasi sebelumnya bagian kode yang mengembalikan response ke client dilakukan pada setiap *branch* `if-else`. Namun setelah dilakukan refactoring *branch* `if-else` hanya menentukan `status_line` dan `filename` yang sesuai dengan HTTP request.

## Commit 4 Reflection Notes

Pada commit ini terdapat conditional tambahan yaitu saat client mengakses path `/sleep` server akan melakukan delay saat ingin mengirim response ke client. Hal ini dilakukan dengan mengeksekusi kode `thread::sleep(Duration::from_secs(5));` di mana kode ini akan memberhentikan thread yang mengeksekusi method `handle_connection()` selama 5 detik sebelum akhirnya mengirim response ke client.  

## Commit 5 Reflection Notes

Thread pool adalah kumpulan thread yang dipanggil (*spawned*) dan menunggu untuk mengeksekusi sebuah tugas. Saat suatu program menerima tugas baru, program tersebut akan meng-*assign* salah satu thread yang ada pada thread pool untuk mengerjakan tugas tersebut sementara thread yang lain akan tetap tersedia untuk mengeksekusi tugas lain. Saat thread yang di-*assign* untuk mengerjakan suatu tugas telah selesai, thread tersebut akan kembali ke thread pool. Dengan melakukan thread pool kita dapat mengeksekusi beberapa tugas secara *concurent*, namun perlu diperhatikan untuk tidak menggunakan terlalu banyak thread pada suatu thread pool untuk menghindari DOS attacks.