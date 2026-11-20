# Materi Pewarisan (Inheritance) - PBO

> Modul untuk sesi asdos. Bahasa sengaja dibuat santai, tapi isi tetap lengkap.
> Semua contoh kode sudah dites di Java 22 dan bisa langsung di-copy-paste.

**Prasyarat:** class, object, constructor, method, access modifier.
**Durasi saran:** 90 menit (30 menit konsep + 50 menit ngoding + 10 menit tanya jawab).

---

## Tujuan sesi

Setelah sesi ini, mahasiswa diharapkan bisa:

1. Menjelaskan pewarisan dengan kalimat sendiri, bukan hafalan definisi.
2. Membedakan hubungan **is-a** (pewarisan) dan **has-a** (komposisi).
3. Menulis class turunan pakai `extends`, `super`, dan `@Override`.
4. Menjelaskan kenapa pewarisan jadi syarat polymorphism.

---

## 1. Apa itu pewarisan, versi bahasa awam

Bayangkan kamu punya **template dasar** daftar hadir: ada kolom nama, ada kolom NIM, ada format sapaan "Halo, saya ...".

Sekarang kamu butuh template **asdos**. Isinya 90% sama: nama, NIM, sapaan. Bedanya cuma ada kolom tambahan "mata kuliah".

Ada dua cara bikin template asdos:

**Cara capek (copy-paste).** Fotokopi template lama, tempel jadi file baru, edit dikit. Hasilnya dua file yang isinya nyaris identik. Suatu hari format sapaan berubah, kamu harus benerin di dua tempat, dan pasti ada satu yang kelupaan.

**Cara pewarisan.** Bilang saja: *"template asdos itu template mahasiswa, cuma ditambah kolom mata kuliah"*. File mahasiswa tetap satu. File asdos cuma menyimpan **bedanya** saja.

Jadi definisi singkatnya:

> **Pewarisan adalah cara membuat class baru yang otomatis memiliki semua sifat dan perilaku class lain, lalu menambah atau mengganti seperlunya.**

Kata kunci yang sering dipakai: class baru **mewarisi** class lama. Class baru = *subclass / child / derived*. Class lama = *superclass / parent / base*.

### Tes "is-a"

Pewarisan hanya sah kalau hubungannya **"adalah"**, bukan **"punya"**.

| Kalimat | Masuk akal? | Kesimpulan |
|---|---|---|
| Asdos **adalah** mahasiswa | ya | boleh `extends` |
| Kucing **adalah** hewan | ya | boleh `extends` |
| Mobil **adalah** mesin | tidak | jangan `extends`, pakai komposisi |
| Dosen **punya** jadwal | ya, tapi "punya" | komposisi, bukan pewarisan |

Kalau kalimat "X adalah Y" terasa aneh, berhenti. Jangan dipaksa.

---

## 2. Istilah yang wajib jelas

| Istilah | Arti |
|---|---|
| `extends` | kata kunci pewarisan di Java |
| `super` | merujuk ke anggota atau constructor class induk |
| `@Override` | penanda bahwa method ini sengaja menimpa versi induk |
| *override* | nama & parameter sama, isi diganti |
| *overload* | nama sama, parameter beda, method baru |
| `protected` | bisa diakses class sendiri, turunannya, dan satu package |

Java hanya mengizinkan **satu induk** untuk class (`extends A` saja). Kalau butuh banyak "kontrak", pakai `interface` (materi terpisah).

---

## 3. Sesi ngoding: tiga langkah

Tiap langkah menunjukkan masalah yang diselesaikan langkah berikutnya. Jalankan berurutan, jangan langsung ke langkah 3.

### Langkah 1 - tanpa pewarisan (biar kerasa sakitnya dulu)

```java
class Mahasiswa {
    String nama;
    String nim;

    Mahasiswa(String nama, String nim) {
        this.nama = nama;
        this.nim = nim;
    }

    void perkenalan() {
        System.out.println("Halo, saya " + nama + ", NIM " + nim);
    }
}

class Asdos {
    String nama;   // copy-paste dari Mahasiswa
    String nim;    // copy-paste dari Mahasiswa

    Asdos(String nama, String nim) {
        this.nama = nama;
        this.nim = nim;
    }

    void perkenalan() {  // copy-paste dari Mahasiswa
        System.out.println("Halo, saya " + nama + ", NIM " + nim);
    }
}

public class Langkah1 {
    public static void main(String[] args) {
        Mahasiswa m = new Mahasiswa("Rina", "2301001");
        Asdos a = new Asdos("Budi", "2301002");

        m.perkenalan();
        a.perkenalan();
    }
}
```

Output:

```
Halo, saya Rina, NIM 2301001
Halo, saya Budi, NIM 2301002
```

Programnya jalan. Tapi minta mahasiswa menghitung: ada berapa baris yang **persis sama** di dua class itu? Itu bagian yang mau kita hapus di langkah berikutnya.

Poin yang ditanam: *duplikasi kode bukan cuma bikin file panjang, tapi bikin perubahan jadi rawan lupa.*

### Langkah 2 - pakai `extends`

```java
class Mahasiswa {
    protected String nama;   // protected = boleh dipakai class turunan
    protected String nim;

    Mahasiswa(String nama, String nim) {
        this.nama = nama;
        this.nim = nim;
    }

    void perkenalan() {
        System.out.println("Halo, saya " + nama + ", NIM " + nim);
    }
}

class Asdos extends Mahasiswa {      // inti pewarisan
    String mataKuliah;

    Asdos(String nama, String nim, String mataKuliah) {
        super(nama, nim);            // titip nama & nim ke constructor induk
        this.mataKuliah = mataKuliah;
    }

    void asistensi() {               // kemampuan baru, cuma asdos yang punya
        System.out.println(nama + " sedang asistensi " + mataKuliah);
    }
}

public class Langkah2 {
    public static void main(String[] args) {
        Mahasiswa m = new Mahasiswa("Rina", "2301001");
        Asdos a = new Asdos("Budi", "2301002", "PBO");

        m.perkenalan();
        a.perkenalan();   // method ini TIDAK ditulis di class Asdos
        a.asistensi();
    }
}
```

Output:

```
Halo, saya Rina, NIM 2301001
Halo, saya Budi, NIM 2301002
Budi sedang asistensi PBO
```

Yang perlu ditunjukkan sambil nunjuk layar:

1. `Asdos` **tidak menulis** `nama`, `nim`, dan `perkenalan()`. Semua itu turun dari `Mahasiswa`.
2. `Budi` tetap bisa `perkenalan()` padahal method itu tidak ada di file `Asdos`.
3. `a.asistensi()` tidak bisa dipanggil dari variabel bertipe `Mahasiswa`, karena `asistensi()` hanya milik anak.
4. Ubah satu baris di `perkenalan()` milik `Mahasiswa`, jalankan ulang: kedua output ikut berubah. Ini bukti konkret manfaat pewarisan.
5. `super(nama, nim)` wajib ada karena constructor induk butuh dua parameter. Kalau dihapus, compile error.

### Langkah 3 - `@Override` dan sekilas polymorphism

```java
class Mahasiswa {
    protected String nama;
    protected String nim;

    Mahasiswa(String nama, String nim) {
        this.nama = nama;
        this.nim = nim;
    }

    void perkenalan() {
        System.out.println("Halo, saya " + nama + ", NIM " + nim);
    }
}

class Asdos extends Mahasiswa {
    String mataKuliah;

    Asdos(String nama, String nim, String mataKuliah) {
        super(nama, nim);
        this.mataKuliah = mataKuliah;
    }

    @Override
    void perkenalan() {
        super.perkenalan();                            // pakai versi induk dulu
        System.out.println("...dan saya asdos " + mataKuliah);
    }

    void asistensi() {
        System.out.println(nama + " sedang asistensi " + mataKuliah);
    }
}

public class Langkah3 {
    public static void main(String[] args) {
        Mahasiswa m = new Mahasiswa("Rina", "2301001");
        Asdos a = new Asdos("Budi", "2301002", "PBO");

        m.perkenalan();
        a.perkenalan();   // versi Asdos yang jalan
        a.asistensi();

        System.out.println();
        System.out.println("-- satu array, dua perilaku berbeda --");

        // Variabel bertipe Mahasiswa boleh diisi Asdos.
        // Java memilih method yang tepat saat program berjalan (runtime).
        Mahasiswa[] daftar = { m, a };
        for (Mahasiswa orang : daftar) {
            orang.perkenalan();
        }
    }
}
```

Output:

```
Halo, saya Rina, NIM 2301001
Halo, saya Budi, NIM 2301002
...dan saya asdos PBO
Budi sedang asistensi PBO

-- satu array, dua perilaku berbeda --
Halo, saya Rina, NIM 2301001
Halo, saya Budi, NIM 2301002
...dan saya asdos PBO
```

Satu array, satu perintah `perkenalan()`, hasilnya beda-beda. Itu **polymorphism**, dan dia bisa jalan justru karena ada pewarisan. Cukup disebut sebagai jembatan ke materi berikutnya, jangan dijelaskan dalam.

Catatan teknis kecil: tiga contoh di atas memakai nama class yang sama (`Mahasiswa`, `Asdos`). Kalau dikompilasi dalam satu folder, `javac` akan protes `duplicate class`. Itu normal. Solusinya: compile tiap langkah ke folder output sendiri (`javac -d out1 Langkah1.java`), atau bungkus tiap langkah dalam `package` berbeda.

---

## 4. Override vs overload

Ini dua istilah yang paling sering ketuker di praktikum.

| | Override | Overload |
|---|---|---|
| Nama method | sama | sama |
| Parameter | **sama persis** | **beda** (jumlah/tipe/urutan) |
| Terjadi di | subclass, menimpa induk | bisa di class mana saja |
| Anotasi | `@Override` | tidak ada |
| Tujuan | ganti perilaku | tambah variasi pemanggilan |

Contoh cepat:

```java
class Induk {
    void cetak(String s) { System.out.println(s); }
}

class Anak extends Induk {
    @Override
    void cetak(String s) { System.out.println("Anak: " + s); }  // OVERRIDE

    void cetak(int n) { System.out.println("Angka: " + n); }    // OVERLOAD
}
```

Kalau `@Override` dipasang tapi ternyata tidak menimpa apa pun (typo nama atau parameter beda), compiler langsung error. Itu gunanya anotasi ini: menangkap kesalahan lebih awal.

---

## 5. Access modifier dan pewarisan

| Modifier | Class sendiri | Turunan | Package sama | Luar |
|---|---|---|---|---|
| `private` | ya | **tidak** | tidak | tidak |
| *(tanpa modifier)* | ya | ya (kalau satu package) | ya | tidak |
| `protected` | ya | **ya** | ya | tidak |
| `public` | ya | ya | ya | ya |

Pesan yang mau ditanam: **`private` tidak ikut bisa diakses anaknya.** Field-nya tetap "ada" di object anak, tapi anak tidak boleh menyentuhnya langsung. Kalau butuh diakses turunan tapi tidak untuk publik, pakai `protected`.

Akses lewat getter/setter tetap boleh dan sering jadi jawaban yang lebih rapi:

```java
class Mahasiswa {
    private String nama;
    protected String nim;

    Mahasiswa(String nama, String nim) {
        this.nama = nama;
        this.nim = nim;
    }

    public String getNama() { return nama; }
}

class Asdos extends Mahasiswa {
    Asdos(String nama, String nim) {
        super(nama, nim);
    }

    void sapa() {
        // System.out.println(nama);        // ERROR: private
        System.out.println(getNama());      // OK: lewat getter
        System.out.println(nim);            // OK: protected
    }
}
```

---

## 6. Kesalahan umum dan cara memperbaikinya

| Gejala | Penyebab | Perbaikan |
|---|---|---|
| `constructor ... cannot be applied to given types` | lupa `super(...)` atau jumlah argumen salah | panggil `super(...)` dengan argumen yang sesuai |
| `cannot find symbol: nama` | field induk `private` | ubah ke `protected` atau pakai getter |
| `duplicate class` | dua file berisi class dengan nama sama di satu folder | pisah folder output atau pakai `package` |
| `method does not override ...` | `@Override` dipasang tapi signature tidak sama | samakan nama dan parameter |
| Output dobel | memanggil `perkenalan()` versi induk dan anak tanpa sadar | cek pemakaian `super.method()` |
| `cannot be converted to ...` | mengisi variabel induk dengan object yang bukan turunannya | pastikan class benar-benar turunan |

Tambahan yang sering muncul: field di subclass dengan nama sama seperti field induk (*shadowing*). Program tetap compile, tapi hasilnya membingungkan. Hindari, dan jadikan contoh "jangan dilakukan".

---

## 7. Kapan justru JANGAN pakai pewarisan

Bagian ini sering dilewat, padahal penting supaya mahasiswa tidak memaksa `extends` ke semua hal.

1. **Hubungannya "punya", bukan "adalah".** Mobil punya mesin, jadi `Mobil` menyimpan `Mesin` sebagai field, bukan `extends Mesin`.
2. **Cuma butuh satu method.** Kalau hanya mau memakai satu fungsi, panggil saja; tidak perlu membuat hierarki.
3. **Perilaku berubah-ubah saat runtime.** Kalau varian bisa berganti (misalnya metode pembayaran), komposisi atau `interface` lebih lentur.
4. **Hierarki jadi dalam dan bercabang.** `A extends B extends C extends D` cepat sekali jadi mimpi buruk. Dua level biasanya cukup.

Aturan praktis yang bisa dibawa pulang: **pakai pewarisan kalau subclass benar-benar spesialisasi dari induk dan bisa menggantikan induk tanpa merusak apa pun.** Kalau ragu, mulai dari komposisi (dibahas di bagian 8).

---

## 8. Komposisi: pasangan dari pewarisan

Kalau pewarisan bilang **"B adalah A"**, komposisi bilang **"B punya A"**.

Bedanya di cara menyimpan:

- **Pewarisan** -> `class Mobil extends Mesin` (Mobil jadi *sejenis* Mesin)
- **Komposisi** -> `class Mobil { private Mesin mesin; }` (Mobil *menyimpan* Mesin di dalamnya)

Ibaratnya: pewarisan itu **cetakan kue**, komposisi itu **kardus berisi barang**. Mobil bukan jenis mesin, tapi mobil punya mesin. Jadi yang benar adalah komposisi.

### Contoh yang bisa langsung dijalankan

```java
class Mesin {
    private String tipe;

    Mesin(String tipe) {
        this.tipe = tipe;
    }

    void nyalakan() {
        System.out.println("  Mesin " + tipe + " menyala: brummm...");
    }
}

class Mobil {
    private String merk;
    private Mesin mesin;   // KOMPOSISI: disimpan sebagai field

    Mobil(String merk, Mesin mesin) {
        this.merk = merk;
        this.mesin = mesin;
    }

    void start() {
        System.out.println(merk + " mau jalan...");
        mesin.nyalakan();          // delegasi: minta mesin yang bekerja
    }

    void gantiMesin(Mesin baru) {
        this.mesin = baru;         // bisa diganti kapan saja
    }
}

public class Komposisi {
    public static void main(String[] args) {
        Mobil m = new Mobil("Toyota", new Mesin("bensin 1500cc"));
        m.start();

        System.out.println();
        System.out.println("-- ganti mesin jadi listrik --");

        m.gantiMesin(new Mesin("listrik"));
        m.start();
    }
}
```

Output:

```
=== komposisi: Mobil punya Mesin ===
Toyota mau jalan...
  Mesin bensin 1500cc menyala: brummm...

-- ganti mesin jadi listrik --
Toyota mau jalan...
  Mesin listrik menyala: brummm...
```

Perhatikan baris terakhir: mesinnya **diganti saat program jalan**. Ini yang tidak bisa dilakukan pewarisan. Kalau `Mobil extends Mesin`, mobil lahir sudah terikat jadi satu jenis mesin dan tidak bisa berganti di tengah jalan.

### Delegasi

Di `start()`, `Mobil` tidak menyalakan dirinya sendiri. Dia **menyuruh** mesinnya:

```java
mesin.nyalakan();   // delegasi
```

Jadi kalau cara kerja mesin berubah, `Mobil` tidak perlu ikut diubah. Cukup bagian yang berubah. Ini yang membuat komposisi lebih lentur.

### Pewarisan vs komposisi

| | Pewarisan (`extends`) | Komposisi (field) |
|---|---|---|
| Hubungan | **is-a** | **has-a** |
| Cara bikin | `class A extends B` | `class A { B b; }` |
| Saat program jalan | kaku, tidak bisa ganti induk | fleksibel, bagian bisa diganti |
| Kalau induk berubah | anak bisa ikut rusak (*fragile base class*) | lebih aman, efeknya terbatas |
| Cocok untuk | spesialisasi yang stabil | merakit object dari bagian-bagian |

Aturan praktis yang sering dikutip: **"favor composition over inheritance"**. Kalau ragu, mulai dari komposisi. Pewarisan dipakai kalau memang ada hubungan is-a yang jelas dan stabil, bukan cuma biar hemat menulis kode.

### Komposisi vs agregasi

Di beberapa buku keduanya dibedakan tipis:

- **Komposisi** - bagian hidup-matinya ikut induk. Mobil dihancurkan, mesinnya ikut (kalau mobil yang membuat mesinnya sendiri).
- **Agregasi** - bagian bisa hidup sendiri. `Kelas` punya `Dosen`; kelasnya bubar, dosennya tetap ada.

Di praktikum biasanya cukup memakai istilah "komposisi" untuk keduanya. Tapi bagus kalau disebut sekali, biar mahasiswa kenal istilahnya.

### Jebakan: `extends` yang dipaksakan

```java
class Mobil extends Mesin { }   // SALAH
```

Cek pakai kalimat: "Mobil adalah mesin?" Aneh, berarti jangan. Contoh lain yang sering muncul: `class Dosen extends Mahasiswa` karena sama-sama punya nama dan NIM. Dosen bukan mahasiswa. Yang benar: buat `Orang` sebagai induk, lalu `Mahasiswa extends Orang` dan `Dosen extends Orang`.

Kebalikannya juga salah: memaksa komposisi untuk hal yang seharusnya berbagi atribut, misalnya `class Orang { Mahasiswa mhs; Dosen dsn; }`. Kalau yang dibutuhkan cuma atribut bersama, pakai induk bersama atau field biasa.


