# Tabung Yuk (proyek Android)

Aplikasi WebView: seluruh tampilan & logika ada di satu file HTML, bagian native hanya
notifikasi, widget, dan pemilih foto.

## Struktur
| Bagian | Lokasi |
|---|---|
| Tampilan & logika (edit di sini) | `app/src/main/assets/index.html` |
| Nama aplikasi | `app/src/main/res/values/strings.xml` (`app_name`) |
| Ikon aplikasi (logo huruf C + tanda + + kalender) | `app/src/main/res/mipmap-*/` dan `drawable-nodpi/ic_launcher_foreground.png` |
| Widget Tabungan (gambar, terkumpul, kurang) | `TabungWidget.kt`, `res/layout/widget.xml`, `res/xml/widget_info.xml` |
| Widget Kalender (bulanan, ‹ ›) | `KalenderWidget.kt`, `res/layout/widget_kal.xml`, `res/xml/widget_kal_info.xml` |
| Data & gambar widget, pilih tabungan | `WidgetKit.kt`, `WidgetConfig.kt`; warna terang/gelap di `values/` dan `values-night/colors.xml` |
| Notifikasi & alarm | `Remind.kt` |
| Jembatan JS ke Android | `MainActivity.kt` (kelas `Bridge`) |
| Versi | `app/build.gradle.kts` (`versionCode`, `versionName`) |

## Cara jadi APK (pilih salah satu)
1. **Android Studio**: File > Open folder ini, tunggu sync, lalu Build > Build APK(s).
   Hasil: `app/build/outputs/apk/debug/app-debug.apk`
2. **Terminal** (perlu JDK 17, Android SDK, Gradle 8.7+): `gradle assembleDebug`
3. **GitHub**: upload isi folder ini ke repo, lalu buka tab Actions > Build APK > Run workflow.
   APK ada di bagian Artifacts. (`.github/workflows/build-apk.yml` sudah disiapkan)

File `gradle-wrapper.jar` tidak disertakan; Android Studio membuatnya otomatis.

## Fungsi yang dipanggil HTML (`window.Android`)
`sync(json)` ringkasan untuk widget/notifikasi (berisi daftar tabungan `L`), `img(id, dataUrl)` gambar tabungan untuk widget, `remind(on, jam, menit, maskHari)` jadwal alarm,
`canNotify()`, `ask()`, `test()` izin & tes notifikasi, `pickImage()` pilih foto, `openUrl(url)` buka tautan https di browser (menu Gift).
Native memanggil balik fungsi HTML: `render()`, `onPhoto()`, `refreshTK(); am(TK)`, `isOpen()`, `home()`.

Tambahan v2.7: `saveFile(nama, mime, teks)` simpan backup/CSV ke file (hasil: `onSaved(status)`), `pickFile()` pilih file backup (hasil: `onFileText(teks, error)`), `shareImg(dataUrl, teks)` bagikan gambar progres (lewat FileProvider), `weekly(on)` ringkasan mingguan tiap Minggu 19.00.

## Catatan
Data tabungan disimpan di localStorage WebView. Ganti kunci tanda tangan atau uninstall
membuat data lama hilang.
