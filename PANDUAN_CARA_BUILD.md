# Panduan Build APK & Penggunaan "SMANTSE CBT"

Folder ini berisi seluruh source code proyek Android resmi untuk **SMAN 4 Padang**.

## Fitur Utama di APK:
1. **FLAG_SECURE**: Mencegah screenshot & mematikan *Google Circle to Search* / Gemini AI (layar otomatis hitam total saat dilingkari).
2. **Immersive Fullscreen**: Menghilangkan status bar dan tombol navigasi HP agar layar penuh.
3. **Lock Back Button**: Dialog konfirmasi saat siswa menekan tombol kembali agar tidak sengaja keluar.
4. **Custom User-Agent**: Menyisipkan identitas `SMANTSE-EXAMBRO/1.0`.
5. **Anti Copy-Paste**: Mematikan long-click context menu.
6. **Domain Lock**: Mencegah siswa membuka link website di luar `smantse.web.id`.

---

## 🚀 Cara 1: Build Otomatis via GitHub (Paling Mudah, Gratis & 3 Menit Jadi)

Anda **tidak perlu menginstal Java / Android Studio** di laptop. Server GitHub akan mengompilasi APK untuk Anda secara gratis:

1. Buat repositori baru di akun GitHub Anda (misalnya beri nama `smantse-cbt-apk`, set ke **Public** atau **Private**).
2. Upload seluruh isi folder `android-exambro` ini ke repositori tersebut (termasuk folder `.github`).
3. Buka tab **Actions** di repositori GitHub Anda.
4. Workflow **Build SMANTSE CBT APK** akan berjalan otomatis selama ~2-3 menit.
5. Setelah selesai (centang hijau), klik workflow tersebut dan download file APK di bagian **Artifacts** (`SMANTSE-CBT-APK`).
6. Bagikan file `.apk` tersebut ke HP siswa (via Google Drive, WhatsApp, atau ditaruh di website sekolah).

---

## 💻 Cara 2: Build Lewat Android Studio (Jika Ada Laptop dengan Android Studio)

1. Buka software **Android Studio**.
2. Pilih **File** $\rightarrow$ **Open** $\rightarrow$ Arahkan ke folder `android-exambro`.
3. Tunggu proses *Gradle Sync* selesai.
4. Klik menu **Build** $\rightarrow$ **Build Bundle(s) / APK(s)** $\rightarrow$ **Build APK(s)**.
5. Setelah selesai, klik notifikasi **locate** untuk mengambil file `app-debug.apk`.
6. Ubah nama file menjadi `SMANTSE-CBT.apk` dan bagikan ke siswa.

---

## 🛡️ Langkah Selanjutnya: Mengunci Web PHP Hanya untuk APK Ini

Setelah APK dibagikan ke siswa, Anda bisa mengaktifkan proteksi di file `config.php` atau `includes/auth.php` dengan menambahkan pengecekan User-Agent:

```php
// Contoh fungsi proteksi di PHP:
function wajib_exambro(): void {
    $ua = $_SERVER['HTTP_USER_AGENT'] ?? '';
    $isExambro = str_contains($ua, 'SMANTSE-EXAMBRO');
    $isIOS = str_contains($ua, 'iPhone') || str_contains($ua, 'iPad');

    // Jika buka di Android tapi BUKAN dari APK resmi:
    if (str_contains($ua, 'Android') && !$isExambro) {
        http_response_code(403);
        die("<h3>Akses Ditolak</h3><p>Ujian wajib menggunakan aplikasi resmi <b>SMANTSE CBT</b>. Silakan buka melalui aplikasi.</p>");
    }
}
```
*(Catatan: Jangan aktifkan pengunci ini sebelum APK dibagikan dan diinstal oleh seluruh siswa Android).*
