# 🤖 Mochi Companion ESP32

Eksperimen bikin robot meja mini interaktif yang terinspirasi dari Dasai Mochi. Proyek ini nampilin animasi ekspresi wajah (*pixel art*) di layar OLED kecil dan bisa bereaksi saat disentuh langsung secara *real-time*.

Dibangun pakai ESP32 dengan fokus pada performa rendering yang mulus, penanganan animasi *non-blocking*, dan interaktivitas simpel tanpa modul sensor tambahan.

---

## ✨ Fitur Utama

* **Smooth Animation (Non-Blocking):** Rendering frame diatur pakai `millis()` (finite state machine), jadi transisi ekspresi tetap mulus tanpa bikin mikrokontroler nge-*freeze*.
* **Dynamic Expressions:** Animasi mata ngedip acak saat mode *idle*, dan otomatis berubah ekspresi (kaget / senang) saat disentuh.
* **Built-in Touch Detection:** Memanfaatkan fitur *Capacitive Touch* bawaan ESP32 di GPIO 4—cukup sentuh kabel jumper langsung bereaksi tanpa perlu beli modul sensor sentuh terpisah.
* **Hemat Memori (PROGMEM):** Seluruh aset bitmap disimpan langsung di Flash memory mikrokontroler biar konsumsi RAM tetap ringan.

---

## 🔌 Skema Wiring (Pinout)

Koneksi display OLED SSD1306 (I2C) ke board ESP32:

| Pin OLED SSD1306 | Pin ESP32 | Keterangan |
| :--- | :--- | :--- |
| **VCC** | **3V3** | Power 3.3V |
| **GND** | **GND** | Ground |
| **SCL** | **GPIO 22** | I2C Clock |
| **SDA** | **GPIO 21** | I2C Data |
| *(Kabel jumper)* | **GPIO 4 (Touch0)** | Sensor sentuh (cukup sentuh ujung kabel) |

---

## 🧠 Alur Animasi (State Machine)

```text
       +---------------------------------------------+
       |                                             |
       v                                             |
[ STATE: IDLE ] --(Interval acak 3-5 detik)--> [ STATE: BLINK ]
       |                                             ^
       |                                             |
 (Sentuhan di GPIO 4)                                |
       |                                             |
       v                                             |
[ STATE: REACTION (Happy/Surprised) ] --(2 detik)----+
