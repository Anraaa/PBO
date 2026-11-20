# Catatan Pribadi: Istilah Penting Pewarisan

> Catatan cepat buat diri sendiri. Bahasa sengaja santai.
> Pakai analogi keluarga biar semua istilah nyambung.

Isi: `extends`, `super`, `override`, `@Override`, `overload`, `protected`.

---

## 1. `extends` - "daftar jadi anak"

Anak lahir di keluarga tertentu, otomatis dapat nama keluarga, rumah, dan aturan rumah. Dia tidak perlu bikin ulang semuanya dari nol.

```java
class Asdos extends Mahasiswa { }
```

Artinya: Asdos itu **anak** dari Mahasiswa. Semua yang dipunya Mahasiswa (field, method) otomatis ikut. Asdos tinggal menambah atau mengganti sedikit.

Bukan "Asdos punya Mahasiswa", tapi "Asdos **adalah** Mahasiswa".

Cek cepat: ucapkan "X adalah Y". Kalau aneh, jangan pakai `extends`.

---

## 2. `super` - "telepon ortu"

Kalau butuh sesuatu yang cuma ortu yang punya, atau mau pakai versi ortu dulu sebelum menambah versi sendiri, ya telepon ortu.

```java
super(nama, nim);      // titip data ke constructor ortu
super.perkenalan();    // jalankan dulu versi ortu, baru tambah sendiri
```

Dua rasa:

- `super(...)` -> mengisi data ortu waktu object lahir (constructor)
- `super.method()` -> menjalankan method versi ortu

Catatan: `super(...)` wajib kalau constructor induk butuh parameter. Kalau induk punya constructor kosong, Java memanggilnya diam-diam.

---

## 3. `override` - "ganti resep ortu"

Ortu punya resep "cara memperkenalkan diri". Kamu sebagai anak merasa resepmu beda, jadi tulis ulang resep dengan **nama menu yang sama persis**.

```java
// di induk
void perkenalan() { System.out.println("Halo"); }

// di anak -> override
void perkenalan() { System.out.println("Halo, saya asdos"); }
```

Syarat override: nama sama **dan** parameter sama.

Kalau nama atau parameternya beda sedikit, itu **bukan** override, tapi method baru. Ini sumber kebingungan nomor satu.

---

## 4. `@Override` - "label pengaman"

Cuma label yang ditempel di atas method. Fungsinya bukan bikin program jalan, tapi bikin compiler **mengingatkan kalau salah ketik**.

```java
@Override
void perkenalan() { ... }
```

Kalau ternyata tidak ada method `perkenalan()` di ortu (typo, atau parameter beda), langsung error. Jadi label ini seperti "cek ulang dong, bener nih gantinya?"

Tanpa label, salah ketik tetap jalan diam-diam dan baru bikin bingung nanti.

---

## 5. `overload` - "menu sama, porsi beda"

Namanya sama, tapi **bahannya beda**. Bukan menggantikan resep ortu, cuma menambah variasi cara pesan.

```java
void cetak(String s) { ... }      // pesan versi teks
void cetak(int n) { ... }         // pesan versi angka
void cetak(String s, int n) { }   // pesan dua bahan
```

Bedanya dengan override cuma satu: **parameter**.

- Override = parameter sama (ganti isi)
- Overload = parameter beda (tambah varian)

---

## 6. `protected` - "kunci lemari keluarga"

Bayangkan ada lemari di rumah. Siapa yang boleh buka?

| Modifier | Siapa yang boleh |
|---|---|
| `private` | cuma diri sendiri, anak tidak boleh |
| `protected` | diri sendiri **dan anak-cucu**, orang luar tidak |
| *(tanpa modifier)* | diri sendiri dan orang satu komplek (satu package) |
| `public` | siapa saja, termasuk tetangga |

```java
class Mahasiswa {
    protected String nama;   // anak boleh akses langsung
    private String rahasia;  // anak TIDAK boleh
}
```

Jadi `protected` itu jalan tengah: terlalu terbuka untuk `public`, tapi mau tetap bisa diwarisi.

---

## Ringkasan sekali lihat

| Istilah | Analogi satu kalimat |
|---|---|
| `extends` | "gua anak dari keluarga ini" |
| `super` | "telepon ortu" |
| `override` | "resep sama, isi gua ganti" |
| `overload` | "nama sama, bahan beda" |
| `@Override` | "label: ini beneran ganti resep ortu, cek dong" |
| `protected` | "lemari keluarga: anak boleh, orang luar nggak" |

Gampangnya:

- **extends & super** -> soal hubungan
- **override & overload** -> soal method
- **protected** -> soal siapa yang boleh lihat

---

## Tes diri 30 detik

1. `class Dosen extends Mahasiswa` -- masuk akal? *(Jawab: tidak. Dosen bukan mahasiswa. Yang benar, keduanya turunan `Orang`.)*
2. Method di anak namanya sama, parameternya beda. Itu override atau overload? *(Overload.)*
3. Field induk `private`, anak mau akses. Bisa? *(Tidak. Ubah ke `protected` atau pakai getter.)*
4. Lupa `super(nama, nim)` padahal induk tidak punya constructor kosong. Apa yang terjadi? *(Compile error.)*
5. `@Override` dipasang di method yang tidak ada di induk. Apa yang terjadi? *(Compile error. Justru itu gunanya.)*
