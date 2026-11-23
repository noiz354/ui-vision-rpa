# ui-vision-rpa

Contoh macro dan catatan pemakaian untuk **[Ui.Vision](https://ui.vision/)** — RPA open-source
dari a9t9 software untuk otomasi browser dan desktop (Windows, macOS, Linux).

Repo ini bukan produk resmi Ui.Vision. Dokumentasi resmi ada di
<https://ui.vision/rpa/docs>, forum di <https://forum.ui.vision/>, source code di
<https://github.com/A9T9/RPA>.

## Apa yang bisa dilakukan Ui.Vision

- **Otomasi browser**: klik, isi form, ekstrak data, download/upload file, screenshot.
- **Otomasi desktop**: kontrol aplikasi native lewat XModules (butuh Desktop Automation app).
- **Visual record & replay**: rekam aksi di halaman, simpan sebagai macro yang bisa dijalankan ulang.
- **Selenium-style commands**: `click`, `type`, `assertText`, `storeText`, dll. — impor/ekspor
  dari Selenium IDE juga didukung.
- **JavaScript API** untuk logika yang tidak cukup dengan command klasik.
- **Data-driven testing**: baca CSV, loop baris, jalankan macro yang sama untuk tiap baris.
- **OCR / screen scraping**: ambil teks dari gambar atau area layar.
- **AI Macro Assistant (V10)**: deskripsikan tugasnya, assistant menulis macro, menjalankannya,
  dan membantu memperbaiki yang gagal. Tersedia di paket gratis.
- **MCP bridge**: hubungkan asisten AI milikmu sendiri agar bisa membuat, menjalankan, dan
  men-debug macro lewat browser. Bridge berjalan lokal di komputer kamu.
- **Command line API**: jalankan macro dari skrip/scheduler tanpa membuka UI.

Semua macro dieksekusi lokal di mesin kamu.

## Instalasi

| Platform | Sumber |
|---|---|
| Chrome | <https://chromewebstore.google.com/detail/uivision/gcbalfbdmfieckjlnblleoemohcganoc> |
| Edge | <https://microsoftedge.microsoft.com/addons/detail/uivision/goapmjinbaeomoemgdcnnhoedopjnddd> |
| Firefox | <https://addons.mozilla.org/firefox/addon/rpa> |
| Desktop app (XModules v2) | <https://ui.vision/rpa/x> |

Browser Vision (image recognition) di Chrome dan Edge V10 berjalan tanpa XModules, termasuk
saat jendela browser ada di background. Untuk aplikasi desktop dan akses file lokal, XModules
tetap diperlukan.

## Struktur repo

```
macros/hello-uivision.json   # contoh macro: buka ui.vision, cek judul, ambil teks
```

## Cara pakai macro di repo ini

1. Buka ekstensi Ui.Vision → tab **Macros**.
2. Import file `.json` dari folder `macros/` (atau drag & drop ke panel macro).
3. Pilih macro, tekan **Play**. Log muncul di tab **Logs**.

## Contoh macro

`macros/hello-uivision.json`:

```json
{
  "Name": "hello-uivision",
  "CreationDate": "2026-11-23",
  "Commands": [
    { "Command": "open", "Target": "https://ui.vision/", "Value": "" },
    { "Command": "waitForPageToLoad", "Target": "30000", "Value": "" },
    { "Command": "assertTitle", "Target": "*Ui.Vision*", "Value": "" },
    { "Command": "storeText", "Target": "css=h1", "Value": "heading" },
    { "Command": "echo", "Target": "Judul halaman: ${heading}", "Value": "" }
  ]
}
```

Kalau `assertTitle` gagal, judul halaman berubah — sesuaikan targetnya, jangan hapus assert-nya.

## Command yang sering dipakai

| Command | Fungsi |
|---|---|
| `open` | buka URL |
| `click` / `clickAndWait` | klik elemen (target bisa `css=`, `xpath=`, `id=`, atau gambar) |
| `type` | isi field dengan teks |
| `select` | pilih opsi dropdown |
| `storeText` | simpan teks elemen ke variabel `${...}` |
| `storeAttribute` | simpan atribut elemen |
| `assertText` / `verifyText` | cek teks (assert = stop saat gagal, verify = lanjut) |
| `waitForElementVisible` | tunggu elemen muncul |
| `executeScript` / `executeScript_Sandbox` | jalankan JavaScript |
| `csvRead` | baca file CSV untuk data-driven run |
| `screenshot` | simpan screenshot |
| `pause` | jeda (ms) |

Referensi lengkap: <https://ui.vision/rpa/docs/selenium-ide> (command klasik) dan
<https://ui.vision/rpa/docs/uiv> (JavaScript API).

## Catatan

- Target selektor paling rapuh adalah koordinat piksel; utamakan `id=`, `css=`, `xpath=`,
  dan simpan gambar acuan hanya untuk elemen yang tidak punya selektor stabil.
- `assert*` untuk hal yang harus benar, `verify*` untuk hal yang boleh dilaporkan tanpa
  menghentikan alur.
- Macro berisi data yang kamu ketik ke halaman. Jangan commit macro yang memuat kredensial;
  pakai variabel/`!DATASOURCE` dan simpan nilai sensitif di luar repo.

## Lisensi

Isi repo ini bebas dipakai dan diubah. Ui.Vision sendiri adalah produk pihak ketiga —
lihat <https://github.com/A9T9/RPA> untuk kode sumber dan ketentuan pemakaiannya.
