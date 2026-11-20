# PBO

Materi dan catatan asistensi mata kuliah **Pemrograman Berorientasi Objek**.

Repositori ini berisi rangkuman konsep, contoh kode, slide, dan catatan praktikum yang dipakai saat sesi asistensi.

## Materi

| Pertemuan | Topik | Berkas |
|---|---|---|
| 8-9 | Pewarisan (inheritance) | [Modul lengkap](Pertemuan%208%20-%209/01-Pewarisan.md) · [Catatan istilah](Pertemuan%208%20-%209/Istilah-Pewarisan.md) · [Slide](Pertemuan%208%20-%209/Week%208-9%20-%20Inheritance%20%28Basic%20%20Detail%20%29.pptx) |

Materi per pertemuan ditambahkan bertahap.

## Isi berkas

- **`01-Pewarisan.md`** - modul utama: penjelasan bahasa awam, tiga langkah ngoding (`extends`, `super`, `@Override`), perbandingan override vs overload, access modifier, dan komposisi sebagai pembanding pewarisan.
- **`Istilah-Pewarisan.md`** - catatan cepat istilah penting dengan analogi, plus tes diri singkat.
- **`Week 8-9 - Inheritance (Basic  Detail ).pptx`** - slide presentasi sesi pewarisan.

## Struktur

```
PBO/
├── README.md
└── Pertemuan 8 - 9/
    ├── 01-Pewarisan.md
    ├── Istilah-Pewarisan.md
    └── Week 8-9 - Inheritance (Basic  Detail ).pptx
```

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

## Rencana materi berikutnya

- Polymorphism
- Abstract class dan interface
- Exception handling

## Catatan

- Materi ditujukan untuk keperluan asistensi dan belajar.
- Silakan pakai, modifikasi, dan sebarkan untuk pembelajaran.
