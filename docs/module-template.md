# Template modul backend

Gunakan struktur berikut sebagai titik awal untuk setiap fitur. Ganti `<module>` dengan
nama fitur dalam bentuk tunggal, misalnya `user`, `product`, atau `order`.

```text
src/
  modules/
    <module>/
      <module>.routes
      <module>.controller
      <module>.service
      <module>.repository
      <module>.model
      <module>.validation
      <module>.test
```

Tambahkan ekstensi yang sesuai dengan bahasa proyek (misalnya `.ts` atau `.py`).
Jika framework mengharuskan satu file test per unit, gunakan file terpisah seperti
`<module>.service.test` dan `<module>.controller.test`.

## Tanggung jawab file

| File | Tanggung jawab |
| --- | --- |
| `<module>.routes` | Mendaftarkan endpoint, menghubungkannya ke controller, dan memasang middleware yang diperlukan. |
| `<module>.controller` | Membaca request, memanggil service, lalu membentuk response HTTP. Hindari aturan bisnis di sini. |
| `<module>.service` | Menjalankan aturan bisnis dan mengoordinasikan repository atau layanan lain. |
| `<module>.repository` | Mengisolasi query dan operasi penyimpanan data. Bila proyek kecil atau belum memakai pola repository, file ini boleh dihilangkan. |
| `<module>.model` | Mendefinisikan bentuk data domain dan relasinya, mengikuti ORM atau pola model proyek. |
| `<module>.validation` | Mendefinisikan validasi input serta bentuk data yang diterima atau dikembalikan. |
| `<module>.test` | Menguji aturan bisnis dan perilaku penting modul. Pisahkan unit test dan integration test sesuai pola test proyek. |

## Alur request

```text
Request -> Route -> Controller -> Service -> Repository -> Database
                                      |
Response <- Controller <-------------+
```

Validasi input dilakukan di batas request (route atau controller, sesuai pola proyek).
Service tetap bertanggung jawab menjaga aturan bisnis agar tetap berlaku saat dipanggil
dari jalur lain, bukan hanya HTTP.

## Checklist modul baru

- [ ] Tentukan endpoint dan skema input/output.
- [ ] Validasi input yang datang dari luar sistem.
- [ ] Letakkan aturan bisnis di service.
- [ ] Pisahkan akses database di repository bila kompleksitasnya memerlukannya.
- [ ] Gunakan penanganan error dan format response yang sudah berlaku secara global.
- [ ] Tambahkan test untuk alur sukses, input tidak valid, dan kondisi bisnis penting.
- [ ] Daftarkan route modul pada router utama.
- [ ] Perbarui dokumentasi API bila endpoint publik berubah.

Ikuti konvensi bahasa, framework, dependency injection, error handling, dan test runner
yang sudah dipilih proyek. Jangan membuat file hanya untuk memenuhi struktur: repository,
model, atau lapisan lain dapat disederhanakan jika tidak diperlukan oleh stack dan
kompleksitas fitur.
