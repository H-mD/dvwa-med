# dvwa-med
DVWA medium: SQL Injection & SQL Injection (blind)

## SQL INJECTION

### Serangan di DVWA dengan security level medium

> Task: sama seperti pada serangan low

Cara:
1. Ubah difficulty serangan menjadi medium pada tab `DVWA Security`

![change diff](src/difficulty.png)

2. Buka tab `SQL Injection` dan lihat source nya
>![source](src/source.png)
>Dapat dilihat bahwa pada bagian pengeksekusian query hampir sama dengan diff low hanya saja pada `$id` tidak ada tanda petik serta pengiriman data menggunakan _POST sehingga query yang akan di inject akan sedikit berbeda

3. Query yang akan di inject berupa:

```
1 UNION SELECT first_name, password FROM users
```
>karena menggunakan UNION, maka jumlah kolom yang ditampilkan harus sama dengan query sebelumnya
>
>query asli: first_name, last_name
>
>query injeksi: first_name, password

4. Injek query yang sudah disiapkan melalui `inspect`
>Kenapa menggunakan `inspect`? karena field id berupa optian bukan textbox sehingga tidak dapat mengetik secara langsung

>![inspect](src/inspect.png)
>tahapan inspect:
>
>klik kanan, pilih inspect, klik icon kotak-kursor pada bagian pojok kiri atas jendela inspect, lalu klik field option id

>![query injection](src/injection.png)
>letakkan query kedalam value pada salah satu tag option

5. Tekan tombol Submit dan hasilnya akan keluar

>![hasil](src/hasil.png)
>hasil injeksi dapat dilihat pada baris data setelah baris pertama (admin/admin)
>
>first_name = first_name
>surname = password

6. Hash yang sudah didapatkan dapat didecrypt seperti pada diff low untuk mendapatkan password sebenarnya

## SQL INJECTION (BLIND)

### Serangan di DVWA dengan security level medium
1. Ubah difficulty serangan menjadi medium pada tab `DVWA Security`

![change diff](src/blind_diff.png)

2. Buka tab `SQL Injection (Blind)` kemudian inspect, pilih tab network, lalu klik submit

![network](src/blind_network.png)

3. Pilih data teratas dan catat cookies dan raw requestnya
>Pilih tab `Cookies` pada bagian bawah untuk melihat cookies yang ada
>![cookies](src/blind_cookies.png)

>Pilih tab `Request` pada bagian bawah lalu tekan togle `Raw` untuk melihat raw request yang dikirimkan
>![request](src/blind_request.png)

>catat dengan format yang sesuai pada gambar dibawah
>![note](src/blind_note.png)

4. Buka terminal dan jalankan command sqlmap berikut:
```
sqlmap -u "[url]" --cookie="[cookies]" --data="[request]" --dbms --batch
```
>Sesuaikan command dengan data yang didapatkan sebelumnya
>
>`[url]` = url dari laman DVWA yang saat ini dibuka 
>
>`[cookies]` = cookies yang telah di catat sesuai format
>
>`[request]` = raw request yang telah dicatat sesuai format 

![sqlmap1](src/blind_sqlmap1.png)
Didapatkan daftar database yang ada pada server
```
[*] dvwa
[*] information_schema
```

5. Ubah command sqlmap sebelumnya menjadi:
```
sqlmap -u "[url]" --cookie="[cookies]" --data="[request]" -D dvwa --tables --batch
```
>command di atas akan menampilkan tabel yang ada dalam database dvwa

![sqlmap2](src/blind_sqlmap2.png)

6. Ubah lagi command sqlmap sebelumnya untuk menampilkan data dalam tabel users
```
sqlmap -u "[url]" --cookie="[cookies]" --data="[request]" -D dvwa -T users --dump --batch
```
>--dump digunakan untuk menampilkan semua data didalam sebuah tabel

![sqlmap3](src/blind_sqlmap3.png)
>karena enkripsi password yang sederhana menggunakan `md5`, sqlmap secara otomatis mendekripsi semua password yang ada