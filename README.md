# WebServer-ExpressJS-05-Express-ContactAppV3
Lanjutan dari seri sebelumnya.

PADA SESI KALI INI AKAN DIJELASKAN CARA MENAMBAHKAN FITUR DELETE DAN UPDATE (EDIT) DATA KONTAK.

Pada sesi ini proses penambahan fitur update tidak menggunakan app.put() tapi masih memanfaatkan app.get().

Skenario.
01. Menambahkan tombol HAPUS pada halaman detail.ejs
02. Membuat function hapus data.


MENAMBAH TOMBOL HAPUS
01. Buka file detail.ejs
02. Buat tombol menggunakan tag <a> kemudian hias dengan bootstrap. arahkan href nya ke '/contact/delelte/:nama'.
03. Buat rout nya menggunaka metode app.get().

    app.get('/contact/delete/:nama', (req, res) => {

    })

04. hati-hati dalam meletakkan rout tersebut, jangan sampai berada dibawah metode yang merender halaman detail,
    //Render halaman detail
    app.get('/contact/:nama', (req, res) => {
        const contact = findContact(req.params.nama)
        res.render('detail', {
            title: "Halaman Detail Contact",
            layout: "layouts/main-layout",
            contact,
        }) 
    })

    Ket:
    Jika meletakkan rout delete setelah rout yang merender halaman detail, maka rout delete tidak akan pernah dijalankan, dan request delete dimana url nya mengarah /contact/delete/:nama akan ditangkap dan dieksekusi oleh rout yang merender halaman detail (dimana akan merespon request url /contact/:nama ). Oleh karen itu letakkan rout delete di atas rout halaman detail.

FUNCTION HAPUS DATA
Function ini akan digunakan pada metode
    app.get('/contact/delete/:nama', (req, res) => {

    })

Proses pembuatan function hapus/delete data
01. Lakukan pengecekan terhadap data yang dikirim. Apakah data yang dikirim tersebut ada di dalam database atau tidak. Sebab ada kemungkinan hal terjadi. Pertama, Jika user mengirim request hapus data melalui halaman detail dengan melakukan klik pada tombol hapus, maka bisa dipastikan data yang dikirim tersebut ada didalam database, sebab yang ditampilkan dihalaman detail adalah data-data yang tersimpan didalam database. Tapi, yang Kedua, jika ada user yang iseng dengan mengirim request delete data dengan mengetik url nya secara langsung di tab search browser, maka ada kemungkinan data yang dikirim itu tidak terdapat didalam database. Oleh karena itu perlu pengecekan. seperti pada sesi sebelumnya, cari terlebih dahulu data yang direquest di database kontak yang ada di contacts.json dengan function yang sudah dibuat sebelumnya, yaitu findContact(). Jika tidak ada maka tampilkan informasi bahwa data tidak ada. Jika ada maka lakukan proses penghapusan data

02. PROSES PENGHAPUSAN DATA menggunakan function deletContact()
    Buat dulu function tersebut di dalam contacts.js

    const deleteContact = (nama) => {}

03. Export dan import function tersebut
04. Gunakan didalam metode hapus data
05. Beri notifikasi bahwa data berhasil dihapus, dengan menggunakan flash message yang sudah dibuat sebelumnya.
06. Arahkan tampilan browser ke halaman contact.ejs setelah data berhasil dihapus.


UBAH (EDIT) DATA KONTAK
Tombol ubah data akan diletakkan di halaman detail.ejs didepan tombol hapus

BUAT TOMBOL EDIT
01. Buka kembali detail.ejs
02. duplikat tag yang menampilkan tombol hapus, kemudian edit beberap kelas nya, href nya, dan namanya

BUAT HALAMAN FORM EDIT DATA dan ROUT nya
Sebenarnya halaman dan rout form edit data ini hampir sama dengan halaman form tambah data. Oleh karena itu biar cepat bisa diduplikat kemudian diedit-edit.

01. Buat raout dengan metode app.get(). Atur agar requestnya berupa url '/contact/edit/:nama',
02. Lakukan pencarian data kontak berdasarkan nama yang dikirim di url nya. Nama yang ada di url tersebut telah ditangkap oleh req.params.nama,
03. Lakukan pencarian menggunakan function findContact() yang sudah dibuat sebelumnya.
04. Kirim hasil pencarian agar dapat ditampilkan dihalaman edit-contact.ejs. Kirim saat melakukan render halaman edit-contact.ejs

MENAMPILKAN DATA PADA FORM SESUAI DENGAN DATA YANG AKAN DIHAPUS
Pada tahap ini akan sedikit memberi value pada form input di edit-contact.ejs
01. Manfaatkan data yang dikirim oleh rout yang menangani edit data contact.
02. Cara menangkap data tersebut agar tampil dihalaman edit-contact.ejs adalah dengan menambahkan atribut value pada tag input, kemudian isi value tersebut dengan data yang sesuai dengan labelnya.
    contoh:
        value="<%= contact.email %>
03. Dengan begitu maka form edit data sudah terisi dengan data-data sesuai dengan data nama yang akan diedit.

PROSES UBAH DATA
Sebelum memproses data, sebaiknya terlebih dahulu dibuat sebuah elemen input pada form edit data contact. Elemen ini bersifat hidden (tidak terlihat dihalaman) yang value nya berisi nama lama, agar apabila terjadi perubahan pada nama yang dilakukan oleh user, maka nama lama  masih bisa di akses. Nama lama ini akan bermanfaat saat proses validasi nama nantinya.

01. Buat tag input dengan type hidden dan value nama yang akan diedit
    <input type="hidden" name="oldName" value="<%= contact.nama %> ">

    Ket:
    Tag tersebut harus disimpan di dalam tag form.

BUAT ROUT UBAH/EDIT DATA
Pada halaman edit-contact.ejs, tag form memiliki beberapa atribut seperti dibawah ini
    <form method="post" action="/contact/update"> ... </form>

Ket:
Method post bertujuan untuk memberikan tanda bahwa ada sebuah permintaan yang menginginkan data yang dimaksud untuk  diubah/diedit atau dihapus. Jadi pada Halaman Form Edit maupun Form tambah data, keduanya menggunkan method post.
Sedangkan atribut action adalah url request yang dikirim oleh user. url ini harus sama dengan url yang ada di rout edit data.

01. Buat Rout Proses Ubah data di app.js
    app.post('/contact/update', (req, res) = {})

    Ket:
    Metode pada rout harus sama dengan method yang ada pada tag form. Url requestnya juga harus sama dengan nilai dari atribut action yang ada di tag form tersebut

02. Untuk melihat data apa saja yang ditangkap oleh rout ini bisa memanfaatkan res.send(), yang mengirimkan data-data yang ada di tag form, dimana data-data ini telah ditangkap dan tersimpan di req.body .Caranya:
    app.post('/contact/update', (req, res) = {
        res.send(req.body)
    })

    maka, setelah tombol simpan perubahan data diklik, browser akan menampilkan semua data yang ada di form tadi.

    {
        oldName: "Nama5 ",
        nama: "NamaLima",
        email: "email5@gmail.com",
        nohp: "0812314234221"
    }

PROSES VALIDASI
Sama seperti pada proses Tambah Data Kontak, pada proses edit/ubah data ini juga perlu validasi ulang, baik itu nama, email, maupun no.hp.


VALIDASI NAMA
Kriteria Validasi Nama:
a. Boleh menggunakan nama lama
b. Tidak boleh ada nama duplikat

    if( value !== req.body.oldName && duplikat){
    
    }

Jika terjadi error pada saat simpan data perubahan, dan error itu ada pada nama. Maka jika ditekan simpan perubahan data sekali lagi, data tersebut akan dikirimkan ke database. Padahal nama tersebut masih error. Oleh karenaitu perlu diperbaiki.

Hal tersebut bisa di atasi dengan pengkondisian pada form edit data kontak. Pada tag form terdapat elemen input yang bersifat hidden, dimana value nya bisa diatur sedemikian rupa.
    
    <input type="hidden" name="oldName" value="<%= contact.nama %>">

Jika di dalam data contact yang dikirim tidak terdapat contact.oldName maka gunakan contact.nama, sehingga tag input tersebut akan berubah menjadi seperti di bawah ini

    <input type="hidden" name="oldName" value="<%= contact.oldName || contact.nama %>">

PROSESS PEMBUATAN FUNCTION UBAH / EDIT DATA
