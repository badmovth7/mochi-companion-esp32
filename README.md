# 🤖 Mochi Companion ESP32

Proyek perangkat tersemat (*embedded systems*) sumber terbuka untuk membuat robot *desktop companion* interaktif yang terinspirasi dari Dasai Mochi. Perangkat ini menampilkan animasi ekspresi wajah berbasis piksel (*pixel art*) pada layar OLED dan merespons interaksi fisik secara *real-time*.

---

## 📌 Ikhtisar Proyek

Tujuan dari proyek ini adalah mengimplementasikan sistem antarmuka grafis ringan dan responsif pada mikrokontroler berdaya rendah. Proyek ini memadukan konsep pemrograman mikrokontroler, manajemen memori aset grafis, dan *state machine* non-blocking agar animasi berjalan mulus tanpa mengorbankan pembacaan input sensor.

### Fitur Utama
* **Non-Blocking Animation Loop:** Rendering grafis dikendalikan menggunakan penjadwalan waktu (`millis()`) sehingga transisi antar-frame berjalan stabil tanpa menggunakan fungsi `delay()`.
* **Dynamic Facial Expressions:** Memiliki berbagai mode ekspresi (Normal/Idle, Random Blink, dan Reaksi Sentuh).
* **Capacitive Touch Input:** Memanfaatkan pin sentuh kapasitif bawaan ESP32 untuk mendeteksi interaksi fisik tanpa memerlukan modul sensor eksternal.
* **Flash Memory Optimization:** Seluruh aset bitmap/sprite disimpan langsung pada memori Flash (`PROGMEM`) untuk menghemat konsumsi SRAM mikrokontroler.

---

## 🛠️ Kebutuhan Hardware & Pinout

### Komponen
* **ESP32 DevKit V1** (Mikrokontroler utama)
* **OLED Display 0.96" I2C (Driver SSD1306, 128x64 piksel)**
* **Kabel jumper (Female-to-Female)**
* **Kabel USB (Data & Power)**

### Skema Sambungan (Wiring)

Komunikasi layar menggunakan antarmuka serial I2C standar pada ESP32:

| Pin OLED SSD1306 | Pin ESP32 | Keterangan |
| :--- | :--- | :--- |
| **VCC** | **3V3** | Sumber tegangan 3.3V |
| **GND** | **GND** | Ground sirkuit |
| **SCL** | **GPIO 22** | I2C Clock Bus |
| **SDA** | **GPIO 21** | I2C Data Bus |
| *(Kabel Sentuh)* | **GPIO 4 (Touch0)** | Elektroda input sentuhan kapasitif |

---

## 🧠 Arsitektur Sistem & State Machine

Siklus animasi berjalan dengan alur *Finite State Machine* (FSM) berikut:

```text
       +---------------------------------------------+
       |                                             |
       v                                             |
[ STATE: IDLE ] --(Interval acak 3-5 detik)--> [ STATE: BLINK ]
       |                                             ^
       |                                             |
 (Sentuhan terdeteksi pada GPIO 4)                   |
       |                                             |
       v                                             |
[ STATE: REACTION (Surprised/Happy) ] --(2 detik)----+
