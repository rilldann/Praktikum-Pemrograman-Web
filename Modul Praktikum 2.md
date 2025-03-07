<!-- # Modul Praktikum 2: Database di Laravel

## 1. Pendahuluan

Laravel menyediakan fitur yang kuat untuk mengelola database, termasuk konfigurasi koneksi, migrasi, dan Schema Builder. Pada modul ini, kita akan membahas pengaturan database dalam file `.env`, menghubungkan Laravel dengan MySQL, serta pembuatan dan modifikasi tabel menggunakan migration dan Schema Builder.

## 2. Pengaturan Database pada .env

Laravel menggunakan file `.env` untuk menyimpan konfigurasi lingkungan, termasuk pengaturan database. Buka file `.env` pada root proyek Laravel dan ubah konfigurasi berikut:

```env
DB_CONNECTION=mysql
DB_HOST=127.0.0.1
DB_PORT=3306
DB_DATABASE=nama_database
DB_USERNAME=root
DB_PASSWORD=your_password
```

Setelah mengatur konfigurasi database, jalankan perintah berikut untuk memastikan Laravel dapat terhubung dengan database:

```sh
php artisan migrate
```

Jika tidak ada error, berarti koneksi berhasil.

## 3. Pembuatan dan Modifikasi Tabel dengan Migration

Laravel menggunakan migration untuk mengelola struktur database secara terstruktur. Untuk membuat migration baru, gunakan perintah berikut:

```sh
php artisan make:migration create_users_table
```

File migration akan tersimpan di folder `database/migrations`. Buka file tersebut dan tambahkan struktur tabel:

```php
public function up()
{
    Schema::create('users', function (Blueprint $table) {
        $table->id();
        $table->string('name');
        $table->string('email')->unique();
        $table->timestamps();
    });
}
```

Jalankan migration untuk membuat tabel:

```sh
php artisan migrate
```

## 4. Penggunaan Schema Builder untuk Manajemen Database

Schema Builder digunakan untuk membuat, mengubah, dan menghapus tabel dengan lebih fleksibel. Contoh menambahkan kolom ke tabel `users`:

```sh
php artisan make:migration add_phone_to_users_table --table=users
```

Kemudian edit file migration yang dibuat:

```php
public function up()
{
    Schema::table('users', function (Blueprint $table) {
        $table->string('phone')->nullable();
    });
}

public function down()
{
    Schema::table('users', function (Blueprint $table) {
        $table->dropColumn('phone');
    });
}
```

Jalankan migration untuk memperbarui tabel:

```sh
php artisan migrate
```

## 5. Model dalam Laravel

Model dalam Laravel digunakan untuk berinteraksi dengan tabel database. Model mewakili entitas dalam database dan memungkinkan penggunaan ORM Eloquent untuk operasi CRUD.

### 5.1. Membuat Model

Untuk membuat model baru, gunakan perintah berikut:

```sh
php artisan make:model User
```

Secara default, model `User` akan terhubung dengan tabel `users`. Model ini terletak di direktori `app/Models` (atau `app/` dalam beberapa versi Laravel).

### 5.2. Menggunakan Model

Model dapat digunakan untuk mengambil, menyimpan, memperbarui, dan menghapus data dalam tabel database.

#### Contoh Penggunaan Model di Controller:

```php
use App\Models\User;

// Mengambil semua data pengguna
$users = User::all();

// Menambahkan pengguna baru
$user = new User();
$user->name = 'John Doe';
$user->email = 'johndoe@example.com';
$user->save();

// Mengupdate data pengguna
$user = User::find(1);
$user->name = 'Jane Doe';
$user->save();

// Menghapus pengguna
$user->delete();
```

## 6. Kesimpulan

Pada modul ini, kita telah mempelajari cara mengatur koneksi database di Laravel melalui file `.env`, membuat dan memodifikasi tabel menggunakan migration dan Schema Builder, serta memahami konsep Model dalam Laravel untuk berinteraksi dengan database.

### **Tugas:**
 -->

# Modul Praktikum 2: Routing dan Templating

## 1. Pendahuluan

Laravel adalah framework PHP yang menyediakan struktur dasar untuk pengembangan aplikasi web. Laravel memiliki berbagai fitur yang memudahkan pengembang dalam membangun aplikasi, termasuk sistem routing yang fleksibel, sistem templating dengan Blade, serta struktur folder yang rapi.

Dalam modul ini, kita akan membahas:
- Struktur dasar Laravel
- Konsep routing dan bagaimana mengelola rute dalam aplikasi
- Penggunaan templating dengan Blade untuk membuat tampilan yang lebih dinamis
- Implementasi layout dan komponen Blade agar kode tampilan lebih modular dan reusable
- Cara menggunakan asset seperti CSS dan gambar dalam Laravel

## 2. Struktur Dasar Laravel

Setelah menginstal Laravel, kita akan menemukan struktur folder utama berikut:

- **app/** : Berisi kode utama aplikasi, termasuk Model, Controller, Middleware, dan service lainnya.
- **bootstrap/** : Berisi file bootstrapping aplikasi, termasuk file `app.php` yang menginisialisasi Laravel.
- **config/** : Berisi file konfigurasi aplikasi, seperti pengaturan database, cache, dan lainnya.
- **database/** : Berisi file yang berhubungan dengan database, seperti migration, seeder, dan factory.
- **public/** : Direktori yang dapat diakses oleh pengguna, berisi file index.php serta aset publik seperti CSS dan JavaScript.
- **resources/** : Berisi view (Blade template), file lokalisasi, dan aset frontend.
- **routes/** : Berisi file definisi routing Laravel (web, API, console, dan channel).
- **storage/** : Berisi file cache, logs, serta file yang di-upload.
- **tests/** : Berisi file untuk pengujian aplikasi.
- **vendor/** : Berisi dependensi dari Composer.

## 3. Dasar-Dasar Routing di Laravel

Routing dalam Laravel berfungsi untuk menentukan bagaimana aplikasi menangani permintaan (request) dari pengguna. Semua rute dalam aplikasi web Laravel didefinisikan dalam file `routes/web.php`.

### 3.1. Mendefinisikan Route Sederhana

Route dasar dalam Laravel menggunakan metode HTTP seperti GET, POST, PUT, DELETE. Contoh route sederhana:

```php
use Illuminate\Support\Facades\Route;

Route::get('/', function () {
    return 'Selamat Datang di Laravel';
});
```

### 3.2. Route dengan Controller

Untuk mengorganisir kode, kita bisa menggunakan Controller:

```php
use App\Http\Controllers\HomeController;

Route::get('/home', [HomeController::class, 'index']);
```

### 3.3. Route dengan Parameter

Laravel memungkinkan kita untuk menangkap parameter dari URL:

```php
Route::get('/user/{id}', function ($id) {
    return "User ID: " . $id;
});
```

### 3.4. Route dengan Parameter Opsional

Kita bisa membuat parameter dalam route bersifat opsional dengan memberi nilai default:

```php
Route::get('/user/{name?}', function ($name = 'Guest') {
    return "Hello, " . $name;
});
```

## 4. Blade Templating

Blade templating adalah sistem template bawaan dari Laravel yang memungkinkan Anda untuk membuat tampilan web dengan cara yang lebih rapi dan terstruktur.

Dengan Blade, Anda bisa membuat bagian-bagian dari halaman web, seperti header atau footer, yang bisa digunakan kembali di banyak halaman tanpa perlu menulis ulang kode. Tentunya, hal ini membuat proses pengembangan web lebih cepat dan mudah.

Blade juga menyediakan fitur sederhana untuk menggabungkan logika PHP dengan HTML, seperti menampilkan data dari database atau memeriksa kondisi tertentu, menggunakan sintaks yang mudah dibaca dan dipahami.

### 4.1. Sintaks Dasar

‘{{  }}’ digunakan untuk mencetak data yang diproses oleh Laravel.

```html
<h1>{{ $judul }}</h1>
```

“@” digunakan untuk menginstruksikan perintah Blade.

```html
@if ($user)
    <p>Selamat datang, {{ $user->name }}</p>
@else
    <p>Selamat datang, Guest</p>
@endif
```

### 4.2. Blade directative

@extends: digunakan untuk mewariskan layout. <br>
@section: digunakan untuk mendefinisikan bagian konten.<br>
@yield: digunakan untuk menampilkan konten tertentu dari section <br>
@include: digunakan untuk menyertakan file blade lain.<br>

### 4.3. Membuat View dengan Blade

File Blade disimpan dalam `resources/views/`. Contoh membuat file `home.blade.php`:

```html
<!-- resources/views/home.blade.php -->
<!DOCTYPE html>
<html>
<head>
    <title>Halaman Home</title>
</head>
<body>
    <h1>Selamat Datang di Laravel</h1>
</body>
</html>
```

Menampilkan view dalam route:

```php
Route::get('/home', function () {
    return view('home');
});
```

## 5. Penggunaan Layout dan Komponen Blade

Blade memungkinkan penggunaan layout untuk menghindari kode duplikasi.

### 5.1. Membuat Layout Blade

Buat file `layout.blade.php` dalam `resources/views/layouts/` untuk mendefinisikan struktur dasar halaman:

```html
<!-- resources/views/layouts/layout.blade.php -->
<!DOCTYPE html>
<html>
<head>
    <title>@yield('title')</title>
</head>
<body>
    <header>
        <h1>My Laravel App</h1>
    </header>
    <div>
        @yield('content')
    </div>
</body>
</html>
```

### 5.2. Menggunakan Layout di View

Gunakan layout dalam `home.blade.php` dengan sintaks `@extends`:

```html
<!-- resources/views/home.blade.php -->
@extends('layouts.layout')

@section('title', 'Home Page')

@section('content')
    <h2>Selamat Datang di Halaman Home</h2>
@endsection
```

## 6. Menggunakan Asset seperti CSS dan Gambar

### 6.1. Menambahkan CSS
Tambahkan file CSS ke dalam folder public/css/ dan panggil dalam Blade dengan asset():
```html
<!-- resources/views/layouts/layout.blade.php -->
<!DOCTYPE html>
<html>
<head>
    <title>@yield('title')</title>
    <link rel="stylesheet" href="{{ asset('css/style.css') }}">
</head>
<body>
    <header>
        <h1>My Laravel App</h1>
    </header>
    <div>
        @yield('content')
    </div>
</body>
</html>
```

### 6.2. Menampilkan Gambar
Simpan gambar di folder public/images/ lalu tampilkan dengan:

```html
<!-- resources/views/home.blade.php -->
@extends('layouts.layout')

@section('title', 'Home Page')

@section('content')
    <h2>Selamat Datang di Halaman Home</h2>
    <img src="{{ asset('images/welcome.jpg') }}" alt="Welcome Image" width="300">
@endsection
```

Pada modul ini, kita telah mempelajari:
- Struktur dasar Laravel dan fungsinya dalam pengembangan aplikasi web.
- Dasar-dasar routing dan cara mengelola rute dalam Laravel.
- Konsep templating dengan Blade dan penggunaannya dalam view.
- Implementasi layout dan komponen Blade untuk membuat tampilan lebih modular.
- Cara menggunakan asset seperti CSS dan gambar dalam Laravel.

### **Tugas:**

1. Buat route yang menampilkan halaman profil pengguna.
2. Buat tampilan profil menggunakan Blade dan gunakan layout.
3. Tambahkan CSS untuk mempercantik tampilan profil.
4. Tambahkan gambar profil pengguna ke dalam tampilan.

note : boleh menggunakan template
