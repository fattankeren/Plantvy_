nama: Plantvy deskripsi: jangan sampe tanaman haus dan laper

fitur:

ada indikator hungru, thirsty, health.
button untuk kasih makan dan minum
ekspresi plant nya
arsitektur:

View: Semua yang tampil di layar HP, seperti gambar tanaman, bar status, dan tombol aksi.
ViewModel: Otak yang mengatur jalannya aplikasi, seperti menghitung level air dan darah, mengatur timer, serta mengolah data sensor.
Model: Tempat menyimpan data kondisi tanaman ke dalam database HP.
teknologi:

Kotlin: Bahasa utama untuk membuat aplikasi ini.
Jetpack Compose: Alat untuk membuat tampilan layar dan animasi tanaman agar mulus dan modern.
Room Database: Tempat menyimpan data kondisi tanaman di HP agar statusnya tidak hilang saat aplikasi ditutup.
Coroutines dan Flow: Pengatur proses otomatis di balik layar, seperti menghitung penurunan haus lapar dan membaca data sensor tanpa membuat aplikasi lemot.
Android SensorManager: Penghubung aplikasi ke sensor gerak dan sensor cahaya di HP.

- sensor: Sensor Gerak (Akselerometer)
  Fungsinya: Mendeteksi kalau HP kamu lagi digoyang-goyang Efeknya di aplikasi: Begitu HP digoyang, daun tanamannya bakal ikut goyang-goyang
  
- Sensor Cahaya (Ambient Light Sensor)
  Fungsinya: Membaca seberapa terang atau gelap ruangan tempat kamu berada. Kalau cahaya 25 Lux ke atas, statusnya jadi "Cahaya Cukup" Kalau di bawah 25 Lux, statusnya jadi "Redup"

<img width="640" height="1282" alt="WhatsApp Image 2026-11-21 at 22 26 07" src="https://github.com/user-attachments/assets/62b840a2-dcb3-4c89-9e61-e44511f7bd1a" />


cara menjalankannya, tinggal run app
