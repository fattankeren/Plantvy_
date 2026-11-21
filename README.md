# Plantvy 🌿

**Plantvy** adalah aplikasi simulasi perawatan tanaman virtual (*virtual plant care / pet simulation*) interaktif berbasis Android modern. Aplikasi ini mengajak pengguna untuk merawat tanaman virtual layaknya merawat tanaman sungguhan di dalam ruangan—menjaga tingkat hidrasi, asupan nutrisi, dan kesehatan tanaman secara konsisten.

---

## 📌 Fitur-Fitur Utama

### 1. 📊 Indikator Status Melayang (*Floating Status Bars*)
Tepat di atas tanaman virtual terdapat panel status melayang berdesain *frosted glass* yang menampilkan kondisi vital tanaman secara *real-time* dengan animasi transisi yang mulus:
- **❤️ Health**: Menunjukkan tingkat kesehatan fisik tanaman (0% – 100%).
- **💧 Thirsty**: Menunjukkan tingkat ketersediaan air / hidrasi tanaman (0% – 100%).
- **🌿 Hungry**: Menunjukkan tingkat kecukupan nutrisi tanaman (0% – 100%).

### 2. 🎮 Aksi & Interaksi Perawatan
- **💧 Water (Siram)**: Menambah cadangan air tanaman (+25%) disertai efek animasi tetesan air.
- **🍃 Feed (Beri Nutrisi)**: Memberikan asupan pupuk/nutrisi (+25%) disertai efek animasi makanan.
- **✨ Revive (Rawat Kembali)**: Tombol pemulihan darurat untuk menghidupkan kembali tanaman yang telah layu/mati.

### 3. ⏳ Simulasi Penurunan Status (*Real-Time & Offline Decay*)
- **Real-Time Decay**: Saat aplikasi aktif dibuka, kebutuhan air dan makanan tanaman akan terus berkurang secara berkala setiap beberapa detik.
- **Offline Decay**: Jika aplikasi ditutup atau perangkat dimatikan, sistem mencatat waktu terakhir (*timestamp*) dan secara otomatis menghitung penurunan status selama aplikasi tidak dibuka (*elapsed time decay*).
- **Logika Kesehatan Otomatis**:
  - Jika tanaman kekurangan air (<20%) atau kekurangan makanan (<20%), kesehatan tanaman akan mulai menurun hingga layu/mati.
  - Jika air dan makanan berada di atas batas aman (≥50%), kesehatan tanaman akan memulihkan diri secara bertahap.

### 4. 🎭 Ekspresi & Visual Tanaman Dinamis
Ilustrasi tanaman akan berubah secara otomatis sesuai dengan kondisi tanaman:
- **Happy Plant**: Tanaman dalam kondisi prima, segar, dan bahagia.
- **Thirsty Plant**: Tanaman haus dan membutuhkan siraman air segera.
- **Hungry Plant**: Tanaman lapar dan membutuhkan nutrisi.
- **Sad / Withered Plant**: Tanaman dalam kondisi kritis (kesehatan rendah) atau telah layu.

### 5. 📱 Integrasi Sensor Perangkat Keras (*Hardware Sensors*)
- **Akselerometer (Shake Detection)**: Mendeteksi goyangan fisik pada smartphone. Saat ponsel digoyangkan (*shake*), daun tanaman akan merespons dengan animasi bergoyang (*leaf wiggle*).
- **Sensor Cahaya Sekitar (*Ambient Light Sensor*)**: Membaca intensitas pencahayaan ruangan sekitar dalam satuan Lux dan memberikan indikator kondisi pencahayaan (≥ 25 Lux: Cahaya Cukup ☀️, < 25 Lux: Redup ⚠️).

### 6. 💾 Penyimpanan Lokal Persisten (*Offline-First*)
Seluruh data kondisi tanaman, status kesehatan, level air, nutrisi, dan riwayat waktu tersimpan secara lokal dan aman di database internal perangkat menggunakan Room Database.

---

## 🛠️ Teknologi & Arsitektur

### 1. Pola Arsitektur (Architecture Pattern)
Aplikasi ini dibangun menggunakan arsitektur **MVVM (Model-View-ViewModel)** dengan prinsip *Unidirectional Data Flow (UDF)*:
- **View (UI Layer)**:
  - Dibangun 100% secara deklaratif menggunakan **Jetpack Compose** dan **Material 3**.
  - Terdiri dari komponen-komponen modular seperti `HomeScreen`, `PlantDisplay`, `PlantStatusBars`, `ActionButtons`, dan `EnvironmentCard`.
- **ViewModel Layer (`PlantViewModel`)**:
  - Mengelola logika bisnis, *state management* dengan `StateFlow` dan `SharedFlow`, penjadwalan *decay timer*, serta siklus hidup sensor (*lifecycle-aware*).
- **Data & Repository Layer (`PlantRepository`)**:
  - Berfungsi sebagai *Single Source of Truth* untuk pengolahan data lokal, perhitungan *decay*, dan operasi database.
- **Sensor Manager Layer**:
  - Mengisolasi interaksi dengan Android `SensorManager` (`LightSensorManager` dan `AccelerometerManager`) dari UI.

### 2. Teknologi & Pustaka (Tech Stack)
- **Bahasa Pemrograman**: [Kotlin](https://kotlinlang.org/) (v2.2.10)
- **UI Toolkit**: [Jetpack Compose](https://developer.android.com/jetpack/compose) & [Material 3](https://m3.material.io/)
- **State Management & Concurrency**:
  - Kotlin Coroutines & Flow (`StateFlow`, `SharedFlow`)
- **Database Lokal**:
  - [Room Database](https://developer.android.com/training/data-storage/room) (v2.8.5)
  - Google KSP (Kotlin Symbol Processing)
- **Lifecycle & Android Jetpack**:
  - AndroidX Lifecycle Runtime KTX (v2.11.0)
  - AndroidX Lifecycle ViewModel Compose (v2.8.7)
  - AndroidX Activity Compose (v1.13.0)
- **Build System**:
  - Gradle Version Catalogs (`gradle/libs.versions.toml`)
  - Android Gradle Plugin (v9.3.3)
- **Target Platform**:
  - Min SDK: `24` (Android 7.0 Nougat)
  - Target SDK: `36`
  - Compile SDK: `37`

---

## 📂 Struktur Direktori Proyek

```text
app/src/main/
├── AndroidManifest.xml
├── java/com/example/plantvy/
│   ├── MainActivity.kt               # Entry point activity & dependency wiring
│   ├── data/
│   │   ├── PlantEntity.kt            # Model tabel Room untuk status tanaman
│   │   ├── PlantDao.kt               # Data Access Object (Query, Insert, Update)
│   │   ├── PlantDatabase.kt          # Room Database instance (Singleton)
│   │   └── PlantRepository.kt        # Repository & kalkulasi logika decay
│   ├── sensor/
│   │   ├── AccelerometerManager.kt   # Pengelola sensor akselerometer & deteksi shake
│   │   └── LightSensorManager.kt     # Pengelola sensor intensitas cahaya (Lux)
│   ├── ui/
│   │   ├── HomeScreen.kt             # Layar utama aplikasi
│   │   ├── components/
│   │   │   ├── ActionButtons.kt      # Tombol aksi (Water, Feed, Revive)
│   │   │   ├── EnvironmentCard.kt    # Kartu informasi sensor lingkungan
│   │   │   ├── LightSensorBadge.kt   # Badge indikator simpel sensor cahaya (Lux)
│   │   │   ├── PlantDisplay.kt       # Komponen visual tanaman & efek aksi
│   │   │   └── PlantStatusCard.kt    # Komponen status bar (Health, Thirsty, Hungry)
│   │   └── theme/                    # Tema warna, tipografi, dan styling Material 3
│   └── viewmodel/
│       ├── PlantViewModel.kt         # ViewModel penghubung UI dan Data
│       └── PlantViewModelFactory.kt  # ViewModelProvider Factory
└── res/                              # Asset grafis (drawable, mipmap, values)
```

---

## 🚀 Cara Menjalankan Aplikasi

1. **Clone atau buka repositori** di Android Studio (disarankan versi terbaru yang mendukung Kotlin 2.x & Compose).
2. Pastikan Android SDK telah terpasang (Min SDK 24, Target SDK 36/37).
3. Sambungkan perangkat fisik Android melalui USB / Wireless Debugging (disarankan menggunakan perangkat fisik untuk menguji sensor akselerometer dan sensor cahaya).
4. Klik tombol **Run 'app'** (`Shift + F10`) di Android Studio.
5. Selamat merawat tanaman virtualmu di **Plantvy**! 🌱
