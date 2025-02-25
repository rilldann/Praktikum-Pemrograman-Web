# Modul Praktikum 2: Database di Laravel

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

