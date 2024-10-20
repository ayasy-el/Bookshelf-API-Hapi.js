# Bookshelf API Documentation
Bookshelf API adalah sebuah API sederhana untuk mengelola data buku. API ini mendukung operasi CRUD (Create, Read, Update, Delete) untuk menambah, melihat, memperbarui, dan menghapus buku. Setiap buku memiliki beberapa properti seperti nama, penulis, tahun terbit, jumlah halaman, dan status membaca.

## Daftar Isi
1. [Project Overview](#project-overview)
2. [Instalasi dan Menjalankan Aplikasi](#instalasi-dan-menjalankan-aplikasi)
3. [Struktur Proyek](#struktur-proyek)
4. [Endpoints dan Response Format](#endpoints-dan-response-format)
    - [1. Menambah Buku Baru](#1-menambah-buku-baru)
    - [2. Mendapatkan Daftar Semua Buku](#2-mendapatkan-daftar-semua-buku)
    - [3. Mendapatkan Detail Buku Berdasarkan ID](#3-mendapatkan-detail-buku-berdasarkan-id)
    - [4. Memperbarui Buku Berdasarkan ID](#4-memperbarui-buku-berdasarkan-id)
    - [5. Menghapus Buku Berdasarkan ID](#5-menghapus-buku-berdasarkan-id)
5. [Query Parameters](#query-parameters)
6. [Fitur Tambahan](#fitur-tambahan)

---

## Project Overview

Proyek ini mengimplementasikan API RESTful untuk mengelola rak buku menggunakan framework Hapi.js. API ini memungkinkan pengguna untuk melakukan operasi CRUD (Create, Read, Update, Delete) pada entri buku. API ini dirancang untuk memenuhi persyaratan khusus terkait fungsionalitas, penanganan kesalahan, dan kinerja.

### Features

-   **Add a new book** to the collection.
-   **Retrieve all books** or a specific book by its ID.
-   **Update book details** by ID.
-   **Delete a book** from the collection by ID.
-   **Filter books** based on name, reading status, and completion status.

### Prerequisites

-   [Node.js](https://nodejs.org/) (version 14.x or above)
-   [npm](https://www.npmjs.com/) (version 6.x or above)
---

## Instalasi dan Menjalankan Aplikasi

1. **Clone Repository**:
   ```bash
   git clone <URL_REPOSITORY>
   ```
2. **Install Dependencies**:
   ```bash
   npm install
   ```
3. **Menjalankan Server**:
   Untuk menjalankan server, gunakan perintah berikut:
   ```bash
   npm run start
   ```

---

## Struktur Proyek

```
.
├── src/
│   ├── books.js           # Data buku yang disimpan
│   ├── handler.js         # Logika penanganan permintaan
│   ├── routes.js          # Definisi rute API
│   └── server.js          # Server utama Hapi
├── package.json           # File konfigurasi npm
└── README.md              # Dokumentasi proyek
```

---

## Endpoints dan Response Format

### 1. Menambah Buku Baru

- **URL**: `/books`
- **Method**: `POST`
- **Request Body**:
  ```json
  {
      "name": "Nama Buku",
      "year": 2023,
      "author": "Nama Penulis",
      "summary": "Ringkasan buku",
      "publisher": "Nama Penerbit",
      "pageCount": 320,
      "readPage": 120,
      "reading": true
  }
  ```

- **Response Berhasil**:
  - **Status Code**: `201 Created`
  - **Response Body**:
    ```json
    {
        "status": "success",
        "message": "Buku berhasil ditambahkan",
        "data": {
            "bookId": "1L7ZtDUFeGs7VlEt"
        }
    }
    ```

- **Response Gagal**:
  - **Jika `name` tidak dilampirkan**:
    - **Status Code**: `400 Bad Request`
    - **Response Body**:
      ```json
      {
          "status": "fail",
          "message": "Gagal menambahkan buku. Mohon isi nama buku"
      }
      ```
  - **Jika `readPage` lebih besar dari `pageCount`**:
    - **Status Code**: `400 Bad Request`
    - **Response Body**:
      ```json
      {
          "status": "fail",
          "message": "Gagal menambahkan buku. readPage tidak boleh lebih besar dari pageCount"
      }
      ```

### 2. Mendapatkan Daftar Semua Buku

- **URL**: `/books`
- **Method**: `GET`

- **Response Berhasil**:
  - **Status Code**: `200 OK`
  - **Response Body**:
    ```json
    {
        "status": "success",
        "data": {
            "books": [
                {
                    "id": "Qbax5Oy7L8WKf74l",
                    "name": "Buku A",
                    "publisher": "Dicoding Indonesia"
                },
                {
                    "id": "1L7ZtDUFeGs7VlEt",
                    "name": "Buku B",
                    "publisher": "Dicoding Indonesia"
                }
            ]
        }
    }
    ```

- **Jika Tidak Ada Buku**:
  - **Status Code**: `200 OK`
  - **Response Body**:
    ```json
    {
        "status": "success",
        "data": {
            "books": []
        }
    }
    ```

### 3. Mendapatkan Detail Buku Berdasarkan ID

- **URL**: `/books/{bookId}`
- **Method**: `GET`

- **Response Berhasil**:
  - **Status Code**: `200 OK`
  - **Response Body**:
    ```json
    {
        "status": "success",
        "data": {
            "book": {
                "id": "aWZBUW3JN_VBE-9I",
                "name": "Buku A Revisi",
                "year": 2011,
                "author": "Jane Doe",
                "summary": "Lorem ipsum dolor sit amet",
                "publisher": "Dicoding",
                "pageCount": 200,
                "readPage": 26,
                "finished": false,
                "reading": false,
                "insertedAt": "2021-03-05T06:14:28.930Z",
                "updatedAt": "2021-03-05T06:14:30.718Z"
            }
        }
    }
    ```

- **Jika Buku Tidak Ditemukan**:
  - **Status Code**: `404 Not Found`
  - **Response Body**:
    ```json
    {
        "status": "fail",
        "message": "Buku tidak ditemukan"
    }
    ```

### 4. Memperbarui Buku Berdasarkan ID

- **URL**: `/books/{bookId}`
- **Method**: `PUT`
- **Request Body**:
  ```json
  {
      "name": "Nama Buku Baru",
      "year": 2024,
      "author": "Penulis Baru",
      "summary": "Ringkasan Baru",
      "publisher": "Penerbit Baru",
      "pageCount": 350,
      "readPage": 150,
      "reading": false
  }
  ```

- **Response Berhasil**:
  - **Status Code**: `200 OK`
  - **Response Body**:
    ```json
    {
        "status": "success",
        "message": "Buku berhasil diperbarui"
    }
    ```

- **Response Gagal**:
  - **Jika `name` tidak dilampirkan**:
    - **Status Code**: `400 Bad Request`
    - **Response Body**:
      ```json
      {
          "status": "fail",
          "message": "Gagal memperbarui buku. Mohon isi nama buku"
      }
      ```
  - **Jika `readPage` lebih besar dari `pageCount`**:
    - **Status Code**: `400 Bad Request`
    - **Response Body**:
      ```json
      {
          "status": "fail",
          "message": "Gagal memperbarui buku. readPage tidak boleh lebih besar dari pageCount"
      }
      ```
  - **Jika Buku Tidak Ditemukan**:
    - **Status Code**: `404 Not Found`
    - **Response Body**:
      ```json
      {
          "status": "fail",
          "message": "Gagal memperbarui buku. Id tidak ditemukan"
      }
      ```

### 5. Menghapus Buku Berdasarkan ID

- **URL**: `/books/{bookId}`
- **Method**: `DELETE`

- **Response Berhasil**:
  - **Status Code**: `200 OK`
  - **Response Body**:
    ```json
    {
        "status": "success",
        "message": "Buku berhasil dihapus"
    }
    ```

- **Jika Buku Tidak Ditemukan**:
  - **Status Code**: `404 Not Found`
  - **Response Body**:
    ```json
    {
        "status": "fail

",
        "message": "Buku gagal dihapus. Id tidak ditemukan"
    }
    ```

---

## Query Parameters

Pada endpoint **GET /books**, terdapat beberapa query parameter opsional yang dapat digunakan untuk memfilter hasil:

- **`name`**: Mengambil buku yang namanya mengandung kata tertentu (case-insensitive).
- **`reading`**: Menyaring buku berdasarkan status membaca (`0` untuk belum membaca, `1` untuk sedang membaca).
- **`finished`**: Menyaring buku berdasarkan status selesai membaca (`0` untuk belum selesai, `1` untuk sudah selesai).

Contoh request:
```
GET /books?name=harry&reading=1
```
