# Modul Praktikum 1: Setup Environment dan Pengenalan Laravel 11

## 1. Pendahuluan

Laravel adalah salah satu framework PHP yang paling populer dan digunakan secara luas dalam pengembangan aplikasi web. Pada modul ini, kita akan melakukan setup environment, instalasi Laravel menggunakan Composer, serta memahami struktur folder dan konsep MVC dalam Laravel.

## 2. Setup Environment

Sebelum memulai pengembangan dengan Laravel, pastikan sistem telah memiliki beberapa komponen berikut:

- **PHP** (Versi 8.2 atau lebih baru)
- **Composer** (Dependency manager untuk PHP)
- **Database Management System** (MySQL atau PostgreSQL)
- **Web Server** (Laravel dapat berjalan menggunakan built-in server PHP atau Apache)

### 2.1. Instalasi Composer

Jika Composer belum terinstal, unduh dan pasang dari situs resmi: [https://getcomposer.org/](https://getcomposer.org/)

Untuk memastikan Composer sudah terpasang, jalankan perintah berikut di terminal atau command prompt:

```sh
composer -V
```

## 3. Instalasi Laravel

Setelah Composer terpasang, Laravel dapat diinstal menggunakan perintah berikut:

```sh
composer create-project --prefer-dist laravel/laravel nama_proyek
```

Contoh instalasi:

```sh
composer create-project --prefer-dist laravel/laravel app_saya
```

Setelah instalasi selesai, masuk ke direktori proyek:

```sh
cd app_saya
```

Jalankan Laravel development server dengan perintah:

```sh
php artisan serve
```

Laravel akan berjalan di `http://127.0.0.1:8000/`.

## 4. Struktur Folder Laravel

Setelah instalasi, berikut adalah struktur folder utama Laravel:

- **app/** : Berisi kode utama aplikasi (Model, Controller, Middleware, dll.)
- **bootstrap/** : Berisi file bootstrapping aplikasi
- **config/** : Berisi file konfigurasi Laravel
- **database/** : Berisi migrasi, seeder, dan factory database
- **public/** : Direktori yang bisa diakses publik (termasuk index.php)
- **resources/** : Berisi tampilan (view), file lokalisasi, dan aset frontend
- **routes/** : Berisi file definisi routing Laravel
- **storage/** : Berisi file cache, logs, dan upload sementara
- **tests/** : Berisi file untuk pengujian aplikasi
- **vendor/** : Berisi dependensi dari Composer

## 5. Konsep MVC di Laravel

Laravel menggunakan arsitektur **Model-View-Controller (MVC)**:

- **Model**: Berinteraksi dengan database menggunakan Eloquent ORM
- **View**: Bagian antarmuka pengguna menggunakan Blade Template Engine
- **Controller**: Menghubungkan Model dan View serta menangani logika bisnis

### Diagram alur MVC:

```
Pengguna -> Request -> Route -> Controller -> Model -> Database -> Response -> View
```

### Contoh pembuatan Controller menggunakan Artisan CLI:

```sh
php artisan make:controller UserController
```

Setelah itu, file `UserController.php` akan muncul di dalam folder `app/Http/Controllers`.

## 6. Kesimpulan

Pada modul ini, kita telah melakukan setup environment, menginstal Laravel menggunakan Composer, serta memahami struktur folder dan konsep MVC dalam Laravel. Pada modul berikutnya, kita akan mempelajari cara menghubungkan Laravel dengan database.

### **Tugas:**

1. Install Laravel di komputer Anda.
2. Jalankan Laravel menggunakan `php artisan serve`.
3. Buat sebuah Controller dan Model menggunakan Artisan CLI.