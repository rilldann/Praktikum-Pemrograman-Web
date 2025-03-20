# Modul Praktikum 3: Database di Laravel

## 1. Pendahuluan
Database adalah bagian penting dalam pengembangan aplikasi web. Laravel menyediakan berbagai fitur untuk mengelola database dengan efisien, termasuk konfigurasi database, migration, seeder, Query Builder, dan Eloquent ORM.

Dalam modul ini, kita akan membahas:

- Konfigurasi database di Laravel.
- Penggunaan Migration untuk membuat tabel.
- Penggunaan Seeder untuk mengisi data awal.
- Query Builder untuk manipulasi data secara fleksibel.
- Eloquent ORM untuk bekerja dengan model data secara lebih intuitif.

---

## 2. Konfigurasi Database di Laravel
Laravel menggunakan file `.env` untuk mengatur koneksi database. 

### 2.1. Mengatur Koneksi Database

Buka file `.env` dan sesuaikan dengan pengaturan database Anda:

```env
DB_CONNECTION=mysql
DB_HOST=127.0.0.1
DB_PORT=3306
DB_DATABASE=nama_database
DB_USERNAME=root
DB_PASSWORD=
```

Kemudian jalankan perintah berikut untuk memastikan koneksi database:

```bash
php artisan migrate
```

---

## 3. Migration: Membuat Struktur Tabel
Migration digunakan untuk membuat dan mengelola struktur tabel di database.

### 3.1. Membuat Migration

Jalankan perintah berikut untuk membuat migration:

```bash
php artisan make:migration create_users_table
```

File migration akan tersimpan di `database/migrations/`. Buka file tersebut dan edit seperti berikut:

```php
use Illuminate\Database\Migrations\Migration;
use Illuminate\Database\Schema\Blueprint;
use Illuminate\Support\Facades\Schema;

return new class extends Migration {
    public function up() {
        Schema::create('users', function (Blueprint $table) {
            $table->id();
            $table->string('name');
            $table->string('email')->unique();
            $table->timestamps();
        });
    }

    public function down() {
        Schema::dropIfExists('users');
    }
};
```

### 3.1.2. Membuat Tabel Posts dengan Relasi ke Users

Jalankan perintah berikut untuk membuat migration:

```bash
php artisan make:migration create_posts_table
```

```php
<?php

use Illuminate\Database\Migrations\Migration;
use Illuminate\Database\Schema\Blueprint;
use Illuminate\Support\Facades\Schema;

return new class extends Migration {
    public function up() {
        Schema::create('posts', function (Blueprint $table) {
            $table->id();
            $table->string('title');
            $table->text('content');
            $table->foreignId('user_id')->constrained()->onDelete('cascade'); // Relasi ke tabel users
            $table->timestamps();
        });
    }

    public function down() {
        Schema::dropIfExists('posts');
    }
};
```

### 3.1.3. Membuat Tabel Comments dengan Relasi ke Posts dan Users

Jalankan perintah berikut untuk membuat migration:

```bash
php artisan make:migration create_posts_table
```

```php
<?php

use Illuminate\Database\Migrations\Migration;
use Illuminate\Database\Schema\Blueprint;
use Illuminate\Support\Facades\Schema;

return new class extends Migration {
    public function up() {
        Schema::create('comments', function (Blueprint $table) {
            $table->id();
            $table->text('comment');
            $table->foreignId('post_id')->constrained()->onDelete('cascade'); // Relasi ke tabel posts
            $table->foreignId('user_id')->constrained()->onDelete('cascade'); // Relasi ke tabel users
            $table->timestamps();
        });
    }

    public function down() {
        Schema::dropIfExists('comments');
    }
};

```

### 3.2. Menjalankan Migration

Setelah membuat migration, jalankan perintah berikut untuk menerapkannya:

```bash
php artisan migrate
```

---

## 4. Seeder: Mengisi Data Awal
Seeder digunakan untuk mengisi database dengan data awal.

### 4.1. Membuat Seeder

Jalankan perintah berikut:

```bash
php artisan make:seeder UserSeeder
```

Buka file `database/seeders/UserSeeder.php` dan edit seperti berikut:

```php
use Illuminate\Database\Seeder;
use Illuminate\Support\Facades\DB;
use Illuminate\Support\Str;
use Illuminate\Support\Facades\Hash;

class UserSeeder extends Seeder {
    public function run() {
        DB::table('users')->insert([
            'name' => 'Admin',
            'email' => 'admin@example.com',
            'password' => Hash::make('password')
        ]);
    }
}
```

### 4.2. Menjalankan Seeder

Tambahkan seeder ke dalam `DatabaseSeeder.php`:

```php
public function run() {
    $this->call(UserSeeder::class);
}
```

Kemudian jalankan perintah berikut untuk menjalankan seeder:

```bash
php artisan db:seed
```

---

## 5. Factory: Membuat Data Dummy Secara Otomatis

### 5.1. Membuat Factory Baru
```bash
php artisan make:factory PostFactory --model=Post
```

Edit file `database/factories/PostFactory.php`:
```php
use Illuminate\Database\Eloquent\Factories\Factory;

class PostFactory extends Factory
{
    protected $model = \App\Models\Post::class;

    public function definition()
    {
        return [
            'title' => $this->faker->sentence,
            'content' => $this->faker->paragraph,
        ];
    }
}
```

Jalankan perintah berikut di **Tinker** untuk membuat 10 data:
```bash
php artisan tinker
```
```php
\App\Models\Post::factory()->count(10)->create();
```

---

## 6. Model: Representasi Data
Eloquent ORM menggunakan model untuk berinteraksi dengan database.

### 6.1. Membuat Model

Jalankan perintah berikut:

```bash
php artisan make:model User
```

Model akan tersimpan di `app/Models/User.php`. Edit file tersebut:

```php
namespace App\Models;

use Illuminate\Database\Eloquent\Factories\HasFactory;
use Illuminate\Database\Eloquent\Model;

class User extends Model {
    use HasFactory;
    protected $fillable = ['name', 'email', 'password'];
}
```

---

## 7. Query Builder dan Eloquent ORM
Query Builder dan Eloquent ORM memungkinkan kita melakukan operasi database dengan lebih mudah.

### 7.1. Query Builder
Query Builder menyediakan cara fleksibel untuk menjalankan query database:

#### a) Mendapatkan Semua Data
```php
use Illuminate\Support\Facades\DB;

$users = DB::table('users')->get();
```

#### b) Menambahkan Data
```php
DB::table('users')->insert([
    'name' => 'John Doe',
    'email' => 'john@example.com',
    'password' => bcrypt('secret')
]);
```

#### c) Memperbarui Data
```php
DB::table('users')
    ->where('id', 1)
    ->update(['name' => 'Jane Doe']);
```

#### d) Menghapus Data
```php
DB::table('users')->where('id', 1)->delete();
```

### 7.2. Eloquent ORM
Eloquent menyediakan cara yang lebih intuitif untuk mengelola data menggunakan model.

#### a) Mendapatkan Semua Data
```php
$users = User::all();
```

#### b) Menambahkan Data
```php
User::create([
    'name' => 'John Doe',
    'email' => 'john@example.com',
    'password' => bcrypt('secret')
]);
```

#### c) Memperbarui Data
```php
$user = User::find(1);
$user->name = 'Jane Doe';
$user->save();
```

#### d) Menghapus Data
```php
$user = User::find(1);
$user->delete();
```

---


