# PBO

Materi dan catatan asistensi mata kuliah **Pemrograman Berorientasi Objek**.

Repositori ini berisi rangkuman konsep, contoh kode, dan catatan praktikum yang dipakai saat sesi asistensi.

## Topik

- [x] **Pewarisan (inheritance)**
  - `extends`, `super`, dan `@Override`
  - Perbedaan override dan overload
  - Access modifier `protected` dan kaitannya dengan pewarisan
  - Komposisi sebagai pembanding pewarisan
- [ ] Polymorphism
- [ ] Abstract class dan interface
- [ ] Exception handling

## Struktur

```
PBO/
├── README.md
└── Pertemuan 8/
    ├── 01-Pewarisan.md
    └── Istilah-Pewarisan.md
```

Materi per pertemuan ditambahkan bertahap.

## Menjalankan contoh

Semua contoh ditulis dalam **Java** dan diuji di Java 22 (kompatibel dengan Java 17 ke atas).

```bash
javac NamaFile.java
java NamaFile
```

Kalau satu folder berisi beberapa class dengan nama yang sama, kompilasi tiap langkah ke folder output terpisah:

```bash
javac -d out NamaFile.java
java -cp out NamaFile
```

## Catatan

- Materi ditujukan untuk keperluan asistensi dan belajar.
- Silakan pakai, modifikasi, dan sebarkan untuk pembelajaran.
