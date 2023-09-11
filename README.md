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

> Task: sama seperti pada serangan low

1. Ubah difficulty serangan menjadi medium pada tab `DVWA Security`

![change diff](src/diff_blind.png)

2. 