
# 📍 Inti dari **Raster Data Model**

### 1. **Perbedaan dengan Vector**

* **Vector**: cocok untuk data **diskrit** (jelas batasnya) → titik, garis, poligon.
  ➝ Contoh: lokasi pohon, jalan, batas desa.
* **Raster**: cocok untuk data **kontinu** (berubah terus di ruang/area) → disimpan dalam bentuk **grid/pixel**.
  ➝ Contoh: curah hujan, ketinggian tanah, suhu, citra satelit.

---

### 2. **Konsep Dasar Raster**

* Raster = **kotak-kotak (grid)**, tiap kotak punya **nilai**.

  * Misal: pixel bernilai 20 = suhu 20°C.
  * Pixel bernilai 200 = ketinggian 200 meter.
* Ukuran kotak disebut **cell size (resolusi)** → makin kecil cell, makin detail tapi file makin besar.

👉 Analogi: Raster itu seperti **foto digital**.

* Foto = kumpulan pixel.
* Kalau zoom terlalu jauh → jadi kotak-kotak (pixelated).

---

### 3. **Jenis Data Raster yang sering dipakai di GIS**

* **Citra satelit** → Sentinel, Landsat (RGB, inframerah, multi-band).
* **Digital Elevation Model (DEM)** → peta ketinggian, untuk analisis kontur, slope.
* **Ortofoto** → foto udara yang sudah dikoreksi.
* **Peta hasil scan** → misal peta kertas yang dijadikan digital.

---

### 4. **Masalah Raster**

* **Ukuran file besar** → karena simpan data di setiap cell.
* **Butuh kompresi & struktur data khusus** biar cepat diakses.

---

### 5. **Kelebihan Raster**

* Sangat bagus untuk analisis **perubahan di permukaan bumi**.
  ➝ Misalnya: menghitung NDVI (indeks kehijauan tanaman), memantau abrasi pantai, menilai tutupan hutan.
* Mudah diintegrasikan dengan **vector** (misalnya overlay polygon area tanam dengan citra satelit).

---

### 6. **Integrasi Raster + Vector dalam Web GIS**

* Raster = **background layer** (citra satelit, DEM, peta curah hujan).
* Vector = **overlay layer** (lokasi kebun, batas kelompok tani, titik penanaman).

👉 Jadi Web GIS modern hampir selalu **menggabungkan keduanya**.
Contoh:

* Raster: peta citra Sentinel-2 (lihat kondisi lahan).
* Vector: poligon 3 hektar yang jadi area tanam.

---

## 🔑 Inti Sari untuk Belajar Web GIS

1. **Raster** = data grid, cocok untuk fenomena kontinu (satelit, DEM, suhu).
2. **Vector** = data geometri, cocok untuk fenomena diskrit (jalan, desa, lahan).
3. **Web GIS** sering butuh **keduanya** → raster jadi basemap, vector jadi data spesifik.
4. Tantangan raster = **ukuran besar** → solusinya pakai tile server (GeoServer, GEE, Mapbox Tiles).

---

Mau saya bikinkan **contoh kasus nyata** pakai konsep raster + vector, misalnya untuk proyek **peta penanaman mangrove** kamu, biar langsung terasa hubungannya dengan Web GIS?

Mantap 👍 saya bikinkan contoh kasus nyata yang langsung nyambung ke **proyek Web GIS untuk penanaman mangrove** kamu.

---

# 🌱 **Contoh Kasus Nyata: Web GIS Penanaman Mangrove**

## 1. **Data Raster (background / monitoring kondisi lahan)**

* **Citra satelit Sentinel-2** (resolusi 10m) → bisa unduh gratis via Copernicus atau Google Earth Engine (GEE).
* Informasi yang bisa ditampilkan:

  * Warna asli (true color) → kondisi lahan seperti dilihat mata.
  * NDVI (Normalized Difference Vegetation Index) → menunjukkan tingkat kehijauan tanaman → cocok untuk melihat **pertumbuhan mangrove** dari bulan ke bulan.
  * NDWI (Normalized Difference Water Index) → bisa digunakan untuk melihat area tergenang air atau garis pantai.

👉 Contoh:
Bulan Januari = area tanam masih kosong.
Bulan Juni = area tanam mulai hijau → NDVI naik → menunjukkan keberhasilan penanaman.

---

## 2. **Data Vector (overlay / informasi detail proyek)**

* **Polygon** → batas lahan kelompok tani (misalnya 300 hektar).
* **Polygon kecil** → area yang ditanami (misalnya 3 hektar pertama).
* **Titik (point)** → lokasi lubang tanam atau titik sampling pohon.
* **Atribut tambahan**:

  * Tanggal tanam.
  * Jumlah bibit ditanam.
  * Jenis mangrove.
  * Status pertumbuhan (misalnya: bibit, 3 bulan, 6 bulan, siap monitoring).

---

## 3. **Integrasi ke Web GIS**

* **Layer Raster** → ditampilkan sebagai basemap dari satelit.
* **Layer Vector** → ditambahkan di atas raster untuk memberi informasi tambahan.
* Fitur Web GIS yang bisa dibuat:

  * **Popup info**: klik poligon 3 hektar → muncul info tanggal tanam, jumlah bibit, tingkat pertumbuhan.
  * **Time slider**: bisa geser timeline untuk lihat perubahan citra satelit (misalnya tiap bulan).
  * **Monitoring interaktif**: warnai polygon berdasarkan tingkat keberhasilan tanam (hijau = sehat, kuning = sedang, merah = gagal).

---

## 4. **Contoh Alur Data**

1. **Raster**: ambil citra Sentinel-2 Januari–Juni.
2. **Vector**: digitasi poligon area tanam 3 hektar di QGIS / ArcGIS.
3. Upload ke **PostGIS** → raster bisa disajikan via WMS/WMTS (GeoServer), vector via API (Laravel backend).
4. WebGIS (Leaflet / Mapbox):

   * Layer Raster = citra satelit Sentinel.
   * Layer Vector = area tanam + titik monitoring.

---

## 🎯 Contoh Tampilan di WebGIS

* Background: **Citra satelit (Raster)**
* Overlay:

  * **Poligon 300 ha** (batas lahan kelompok tani).
  * **Poligon 3 ha** (area penanaman pertama).
  * **Titik bibit** (misalnya 5000 titik tanam).
* Klik poligon/titik → muncul **info tanam & status pertumbuhan**.
* Geser slider waktu → lihat perubahan vegetasi (hijau makin luas = mangrove tumbuh).

---

✨ Dengan kombinasi **Raster + Vector**, kamu bisa bikin **WebGIS Monitoring Penanaman Mangrove** yang tidak hanya menampilkan lokasi, tapi juga perkembangan vegetasi secara waktu nyata (near real-time) pakai satelit.

---

Oke Mas Arif, mari saya sederhanakan isi **bab 4.1 tentang Raster Data Model** ini supaya lebih gampang dipahami dan bisa nyambung ke Web GIS nantinya 👍

---

## 📌 Inti Sederhana Raster Data

Bayangkan **peta raster itu seperti foto digital**.
Kalau kita zoom in ke sebuah foto, kita akan melihat **kotak-kotak kecil (pixel)**.
Nah, di GIS, raster juga sama → dibentuk dari **grid kotak-kotak (sel)**.

Setiap kotak (sel/pixel) punya **nilai tertentu** yang mewakili informasi di lokasi itu.
Misalnya:

* Nilai `1` = hutan 🌳
* Nilai `2` = sawah 🌾
* Nilai `3` = air 🌊

Atau kalau raster numerik (misalnya suhu, curah hujan), tiap kotak bisa punya nilai `28.3 °C`, `29.7 °C`, dst.

---

## 📌 Bagian Penting Raster Data

1. **Cell Value (Nilai Sel)**

   * Bisa **kategori (integer)** → contoh: 1=hutan, 2=jalan, 3=air.
   * Bisa **angka desimal (floating point)** → contoh: ketinggian tanah = 23.45 m, curah hujan = 120.5 mm.

👉 Kalau kategorikal = biasanya dipakai untuk klasifikasi (jenis tanah, tutupan lahan).
👉 Kalau numerik desimal = biasanya untuk data pengukuran (suhu, elevasi, curah hujan).

---

2. **Cell Size (Ukuran Sel / Resolusi)**

   * Menunjukkan seberapa besar area yang diwakili oleh satu sel.
   * Contoh:

     * Resolusi **10 m** → 1 sel = area 10x10 m (detail tinggi).
     * Resolusi **30 m** → 1 sel = area 30x30 m (lebih kasar).

👉 Makin kecil cell size = makin detail, tapi file makin besar.
👉 Makin besar cell size = lebih ringan, tapi kurang detail.

---

3. **Cell Depth (Kedalaman Sel)**

   * Berhubungan dengan **berapa banyak variasi nilai** yang bisa disimpan.
   * Misalnya:

     * 8-bit raster → bisa menyimpan **256 nilai unik** (0–255).
     * 16-bit raster → bisa menyimpan **65.536 nilai unik**.

👉 Sama kayak kualitas foto: semakin tinggi bit → semakin banyak variasi warna/detail.

---

4. **Raster Bands (Saluran Data)**

   * **Single band** → hanya 1 data per pixel.
     👉 Contoh: peta ketinggian (DEM).
   * **Multi band** → 1 pixel punya banyak data sekaligus.
     👉 Contoh: citra satelit Sentinel-2 (punya 13 band: merah, hijau, biru, inframerah, dll).

👉 Kalau di Web GIS, band ini bisa dipilih → misalnya gabungkan band RGB untuk tampilan warna asli, atau band inframerah untuk analisis vegetasi.

---

## 📌 Analoginya Supaya Lebih Mudah

* **Raster = foto digital dari bumi.**
* **Pixel = kotak kecil di foto.**
* **Nilai pixel = informasi yang disimpan (warna, suhu, tutupan lahan, dll).**
* **Resolusi = seberapa tajam fotonya.**
* **Bit depth = seberapa banyak gradasi warna/nilai yang bisa ditampung.**
* **Bands = filter kamera (RGB, inframerah, dll).**

---

## 📌 Hubungan ke Web GIS

Kalau Mas Arif mau bikin **Web GIS untuk pemantauan lahan**:

* Data **raster** cocok untuk **update citra satelit** (misalnya mau lihat hijau-tidaknya lahan).
* Bisa pakai **band inframerah** untuk tahu tingkat kesehatan tanaman.
* Bisa juga bikin **time-series raster** → tiap bulan ambil citra baru, lalu tampilkan di web (pakai slider waktu).

---

👉 Jadi singkatnya:

* **Vector** = bagus untuk batas area (shapefile, polygon 3 hektar).
* **Raster** = bagus untuk info kondisi di dalam area itu (misalnya hijau, gundul, lembab, kering, subur).

---

Siap Mas Arif 👍, saya sederhanakan lagi bagian ini biar lebih gampang dipahami untuk belajar Web GIS.

---

## 📌 Inti Konsep "Spatial Reference" di Raster

Kalau kita punya data raster (misalnya **citra satelit atau peta elevasi**), supaya bisa dipakai di GIS, **data itu harus tahu posisinya di dunia nyata** → inilah yang disebut **spatial reference (sistem koordinat)**.

Kalau tidak ada spatial reference, citra itu cuma jadi gambar biasa (kayak foto di HP). GIS tidak bisa tahu letaknya di bumi.

---

## 📌 Cara Raster Tahu Koordinatnya

1. **Baris (rows) = sumbu Y (utara–selatan)**
2. **Kolom (columns) = sumbu X (barat–timur)**
3. Titik awal raster biasanya ada di **pojok kiri atas** (beda dengan sistem koordinat kartesius yang mulai dari kiri bawah).
4. Dengan info:

   * jumlah baris/kolom,
   * ukuran cell (misalnya 30 m),
   * dan titik koordinat awal,
     kita bisa hitung posisi **setiap pixel di bumi**.

👉 Misalnya:

* Resolusi 30 m → artinya **1 pixel mewakili area 30 x 30 meter di bumi**.
* Kalau pojok bawah kiri ada di koordinat (499995, 5177175) dan pojok kanan atas di (509535, 5191065), maka software GIS bisa menghitung koordinat tengah tiap pixel.

---

## 📌 Contoh Nyata Perhitungan

* Raster: 463 baris, 318 kolom, ukuran cell = 30 m.
* Luas total area ≈ 14 km × 9,5 km.
* Kalau mau tahu koordinat **pixel baris 1, kolom 1**, tinggal dihitung dari pojok kiri bawah ditambah cell size.

👉 Jadi tiap pixel punya koordinat yang jelas di bumi → ini yang bikin raster bisa dipasang pas dengan data vektor (contoh: batas tanah kelompok tani).

---

## 📌 Ukuran File Raster (Data Volume)

Semakin tinggi resolusi, semakin besar ukuran file.

### Contoh 1: SPOT 5

* Area: 60 × 60 km
* Resolusi: 10 m → (6000 × 6000 pixel)
* Band: 3 (RGB)
* Kedalaman: 8 bit (1 byte)
* Total ukuran: 108 MB

### Contoh 2: IKONOS

* Area: 10 × 10 km
* Resolusi: 4 m → (2500 × 2500 pixel)
* Band: 4 (RGB + NIR)
* Kedalaman: 16 bit (2 byte)
* Total ukuran: 50 MB

👉 Perhatikan: meskipun area IKONOS lebih kecil, tapi karena **resolusi lebih tinggi (lebih detail)** dan bit lebih besar, ukuran file tetap besar.

---

## 📌 Analoginya Biar Lebih Gampang

* **Resolusi (cell size)** = seperti resolusi kamera HP.

  * 10 m = foto agak blur, tapi ukuran file kecil.
  * 1 m = super detail, tapi file besar.
* **Bit depth** = seberapa banyak warna/nilai yang bisa dibedakan.

  * 8 bit = seperti foto 256 warna.
  * 16 bit = seperti foto jutaan warna.
* **Spatial reference** = GPS di foto.

  * Foto tanpa GPS → hanya gambar.
  * Foto dengan GPS → bisa tahu lokasi persis di peta.

---

## 📌 Hubungannya dengan Web GIS

* Kalau Mas Arif mau tampilkan citra satelit di Web GIS, harus **pakai raster yang sudah georeferenced** (punya spatial reference).
* Kalau tidak, citra akan “melayang” atau tidak sejajar dengan data vektor (misalnya batas lahan).
* Untuk web GIS, biasanya data raster dipotong-potong jadi **tiles (seperti potongan Google Maps)** supaya cepat ditampilkan.

---

👉 Jadi kesimpulan bab ini:

1. **Raster harus punya spatial reference** supaya pas dengan data lain.
2. **Resolusi kecil = detail, tapi file besar.**
3. **Bit depth = makin besar, makin kaya informasi.**
4. Web GIS biasanya ambil raster dengan resolusi menengah (supaya seimbang antara detail dan ukuran file).

---

Mau saya bikinkan **contoh kasus khusus Web GIS untuk lahan mangrove** → misalnya update peta dari Sentinel-2 (10 m resolusi) dengan band inframerah untuk melihat pertumbuhan pohon?


Mantap mas Arif 🙌, saya bikinkan **contoh kasus nyata Web GIS khusus untuk lahan mangrove** biar langsung kebayang penerapannya.

---

# 🌱 **Contoh Kasus Web GIS untuk Lahan Mangrove**

## 🎯 Latar Belakang

PT Tanamin Bumi Nusantara ingin memantau program rehabilitasi mangrove di pesisir Balikpapan.
Total lahan: **300 hektar**.
Target awal penanaman: **3 hektar**.
Kebutuhan: Web GIS yang bisa menampilkan **lokasi penanaman** dan **perkembangan mangrove** tiap 6 bulan.

---

## 🗂️ **Data yang Digunakan**

1. **Raster (Citra Satelit Sentinel-2)**

   * Resolusi: 10 meter
   * Band yang dipakai:

     * **Band 4 (Red)**
     * **Band 8 (NIR)** → untuk menghitung **NDVI (Normalized Difference Vegetation Index)**.
   * Fungsi: mengetahui tingkat kehijauan / kesehatan mangrove.

2. **Vector (Batas Lahan dan Area Penanaman)**

   * Polygon batas 300 ha → seluruh area kelompok tani.
   * Polygon 3 ha → area penanaman baru.
   * Point lokasi penting → misalnya titik pemantauan atau lokasi kelompok tani.

---

## 🧮 **Analisis Raster**

1. Ambil citra Sentinel-2 setiap 6 bulan.

2. Hitung **NDVI**:
   [
   NDVI = \frac{NIR - Red}{NIR + Red}
   ]

   * NDVI > 0.5 → vegetasi sehat (mangrove tumbuh baik).
   * NDVI < 0.2 → area rusak atau gundul.

3. Buat peta klasifikasi NDVI:

   * Merah = gundul
   * Kuning = sedang tumbuh
   * Hijau = sehat

---

## 🌍 **Implementasi di Web GIS**

1. **Hosting Data Raster**

   * Citra Sentinel-2 dipotong sesuai area 300 ha.
   * Disajikan via **GeoServer (WMS/WMTS)** atau langsung dari **Google Earth Engine**.

2. **Integrasi Vector + Raster**

   * Leaflet/Mapbox digunakan di web.
   * Layer 1 → Polygon batas lahan (300 ha).
   * Layer 2 → Polygon penanaman (3 ha).
   * Layer 3 → Raster NDVI (berwarna merah–hijau).

3. **Fitur Interaktif**

   * **Slider waktu** → pilih citra bulan tertentu (misalnya Januari, Juli, Desember).
   * **Popup info** → klik area penanaman, muncul statistik:

     * NDVI rata-rata
     * Persentase area hijau
     * Foto lapangan (kalau ada).

---

## 📊 **Contoh Output di Web GIS**

* Peta dasar (OpenStreetMap).
* Area 300 ha tampak transparan abu-abu.
* Area 3 ha ditandai garis tebal biru.
* Raster NDVI ditampilkan dengan warna:

  * Hijau tua → mangrove sehat
  * Kuning → mangrove tumbuh sedang
  * Merah → area rusak / perlu rehabilitasi

Contoh: Setelah 6 bulan, area 3 ha yang awalnya merah (kosong) berubah jadi hijau (tumbuh sehat).

---

## 💡 Manfaat

* **Monitoring otomatis** tanpa perlu drone.
* **Transparansi ke donor/CSR**: progress bisa dilihat di peta online.
* **Analisis berkelanjutan**: bisa bandingkan tiap tahun perkembangan mangrove.

---

👉 Jadi Web GIS ini pakai **kombinasi data vector + raster**:

* **Vector** → batas lahan, lokasi penanaman.
* **Raster** → citra satelit Sentinel-2 untuk update pertumbuhan mangrove.

---

Mau saya bikinkan **sketsa tampilan Web GIS** (misalnya pakai Leaflet dengan layer polygon + slider waktu NDVI), biar lebih mudah divisualisasikan?

Oke mas Arif 👍 saya bikinkan **sketsa tampilan Web GIS sederhana** untuk monitoring lahan mangrove.
Saya buat konsepnya seperti **mockup UI** biar langsung kebayang.

---

# 🌍 **Sketsa Tampilan Web GIS Lahan Mangrove**

## 🖼️ Tampilan Utama

```
+-------------------------------------------------------------+
| [Logo Tanamin Bumi Nusantara]   [Menu: Home | Data | About] |
+-------------------------------------------------------------+
| 🔍 Search lokasi...   | 📅 Pilih Waktu [Jan 2025 ▼]         |
+-------------------------------------------------------------+
|                                                             |
|   [PETA INTERAKTIF - Leaflet/Mapbox]                       |
|                                                             |
|   -------------------------------------------------------   |
|   | Layer Control:                                        | |
|   | ☑ OpenStreetMap (Basemap)                            | |
|   | ☑ Batas Kelompok Tani (300 ha, polygon abu-abu)      | |
|   | ☑ Area Penanaman (3 ha, polygon biru tebal)          | |
|   | ☑ Raster NDVI (Hijau = sehat, Kuning = sedang,       | |
|   |   Merah = rusak)                                     | |
|   -------------------------------------------------------   |
|                                                             |
|   [Popup Info jika klik area]                               |
|   -------------------------------------------------------   |
|   | Area: 3 ha                                             | |
|   | NDVI rata-rata: 0.65 (sehat)                           | |
|   | Status: ✅ Tumbuh baik                                 | |
|   | Progress: 75% hijau, 25% kuning                        | |
|   | Foto lapangan: [thumbnail klik zoom]                   | |
|   -------------------------------------------------------   |
|                                                             |
+-------------------------------------------------------------+
| 📊 Statistik Perkembangan Mangrove                          |
|  • Januari 2025: NDVI 0.32 (awal tanam)                     |
|  • Juli 2025: NDVI 0.55 (mulai tumbuh)                      |
|  • Desember 2025: NDVI 0.72 (sehat)                         |
+-------------------------------------------------------------+
```

---

## 🎨 Penjelasan Fitur

1. **Basemap** → OpenStreetMap sebagai dasar.
2. **Polygon batas lahan (300 ha)** → warna abu-abu transparan.
3. **Polygon area tanam (3 ha)** → garis tebal biru.
4. **Layer Raster NDVI** → dari Sentinel-2:

   * Hijau → mangrove sehat
   * Kuning → sedang tumbuh
   * Merah → rusak / kosong
5. **Time Slider (Timeline)** → pilih citra satelit tiap bulan/semester.
6. **Popup Info** → muncul detail saat klik area tanam.
7. **Statistik Timeline** → grafik pertumbuhan NDVI.

---

## 📊 Visualisasi Tambahan

* Bisa ada **grafik batang/line chart** NDVI per bulan.
* Bisa tambahkan **legend warna** (Hijau = sehat, Merah = rusak).
* Kalau pakai **Leaflet TimeDimension**, user bisa "play" perubahan citra tiap bulan → animasi pertumbuhan mangrove.

---

Mas Arif, mau saya bikinkan **kode prototype Web GIS** pakai **Leaflet + GeoServer (atau GeoJSON dummy)** supaya langsung bisa dicoba di browser, atau cukup visual mockup dulu?

Oke, saya coba sederhanakan ya agar lebih gampang dipahami, apalagi untuk mendukung belajar **Web GIS**.

---

📌 **Inti pembahasan di teks tadi**:
Kalau kita punya **data raster** (misalnya citra satelit atau peta elevasi), supaya bisa dipakai di GIS atau Web GIS, kita harus memastikan data itu **georeferenced** → artinya sudah punya **koordinat yang sesuai dengan bumi nyata** (contohnya sistem UTM, WGS84, dll).

---

### 🧩 Cara kerjanya (disederhanakan):

1. **Raster itu kotak-kotak (grid pixel)** → setiap kotak punya ukuran tertentu (contohnya 30 meter × 30 meter di dunia nyata).
2. **Baris (rows)** pada raster dihitung dari atas ke bawah.

   * Jadi row 1 ada di paling atas, row terakhir di paling bawah.
3. **Kolom (columns)** dihitung dari kiri ke kanan.
4. Untuk menghubungkan kotak raster dengan koordinat bumi:

   * Tentukan titik awal → biasanya **pojok kiri bawah** (x_min, y_min).
   * Hitung koordinat ke kanan (x) dengan menambah ukuran sel.
   * Hitung koordinat ke atas (y) dengan menambah ukuran sel.

---

### 📊 Contoh sederhananya:

Bayangkan kita punya peta **kebun mangrove** yang diubah jadi raster:

* Ukuran cell = **10 meter**
* Titik koordinat pojok kiri bawah = (100, 200)

Maka:

* Cell **kolom 1, baris 1** mewakili area dari (100, 200) sampai (110, 210).
* Cell **kolom 2, baris 1** mewakili area dari (110, 200) sampai (120, 210).
* Cell **kolom 1, baris 2** mewakili area dari (100, 210) sampai (110, 220).

Setiap pixel di raster punya koordinat dunia nyata → inilah yang bikin raster bisa dipasang ke peta di Web GIS.

---

### 🎯 Inti sari untuk belajar Web GIS:

* **Vector** = data berbentuk titik, garis, poligon (misalnya lokasi tambak, garis pantai, batas hutan mangrove).
* **Raster** = data kotak-kotak (grid), biasanya untuk citra satelit, peta elevasi, kepadatan, suhu, kelembaban, tutupan lahan, dll.
* Supaya bisa ditampilkan di Web GIS, raster harus punya **koordinat bumi (georeferenced)** → sehingga saat dipetakan, letaknya pas di atas peta dasar (misalnya Google Maps atau OpenStreetMap).

---

➡️ Jadi gampangnya:
Raster itu seperti **foto satelit yang ditempelkan ke peta dunia**, tapi biar pas nempelnya, kita harus kasih tahu **koordinat setiap ujung fotonya**. Setelah itu, tiap pixel otomatis tahu koordinatnya.

---

Mau saya bikinkan ilustrasi visual sederhana (gambar kotak-kotak raster dengan koordinatnya) biar makin jelas?

Oke, saya sederhanakan isi dari bagian **4.2 Satellite Images** supaya lebih mudah dipahami dan bisa nyambung ke Web GIS nanti.

---

## 📷 **Citra Satelit dalam GIS** (Inti Sari)

1. **Jenis Sistem Satelit**

   * **Pasif (Optical):**

     * Hanya merekam cahaya atau energi yang **dipantulkan bumi**.
     * Contohnya: kamera satelit yang memotret warna bumi.
     * Data berupa **panchromatic (hitam-putih)** atau **multispektral (berwarna, banyak layer warna)**.
   * **Aktif (Radar / SAR):**

     * Satelit **memancarkan gelombang radar**, lalu merekam pantulannya.
     * Keunggulan: bisa dipakai meskipun ada **awan, hujan, atau malam hari**.
     * Jenis gelombang: L-band (paling panjang), C-band (sedang), X-band (pendek).

2. **Resolusi Spasial (Spatial Resolution)**

   * Menunjukkan ukuran detail peta (pixel = ukuran area di bumi).
   * Contoh: resolusi **30 m** berarti **1 pixel = area 30 x 30 m = 900 m²**.
   * Semakin kecil ukuran pixel → semakin detail.

3. **Program Satelit yang Terkenal**

   * **Landsat (AS, sejak 1972):**

     * Landsat 1–3 → pakai sensor MSS (resolusi 79 m).
     * Landsat 4–5 → sensor TM (7 band warna, resolusi 30 m).
     * Landsat 7 (1999) → ETM+, bisa dipakai untuk memantau **vegetasi, deforestasi, erosi, salju, urbanisasi**.
     * Landsat 8 (2013) → sensor lebih lengkap, ada **band biru dalam (deep blue)** untuk air dan **thermal infrared** untuk suhu.

---

## 🟢 **Contoh Nyata untuk Web GIS**

Misalnya kamu ingin buat **Web GIS untuk Mangrove di Balikpapan**:

* **Data pasif (optical, seperti Landsat 8):**

  * Bisa dipakai untuk melihat **warna vegetasi mangrove** (menggunakan band merah, hijau, biru, dan near infrared).
  * Bisa dianalisis NDVI (Normalized Difference Vegetation Index) → apakah mangrove sehat atau gundul.
* **Data aktif (SAR, misalnya Sentinel-1):**

  * Bisa dipakai untuk **monitoring area mangrove meskipun tertutup awan**, karena di daerah tropis sering berawan.
* **Resolusi:**

  * Kalau pakai Landsat (30 m) → bisa lihat kondisi mangrove secara umum.
  * Kalau mau lebih detail (contoh Sentinel-2, resolusi 10 m) → bisa bedakan area mangrove dari kebun atau pemukiman.

---

## 🔑 **Intinya**

* **Satelit Pasif (optical):** seperti kamera, butuh cahaya → bagus untuk analisis warna vegetasi.
* **Satelit Aktif (SAR):** seperti radar, bisa menembus awan → bagus untuk kondisi tropis.
* **Resolusi:** makin kecil ukuran pixel → makin detail peta di Web GIS.
* **Contoh program satelit yang sering dipakai:** Landsat (gratis dari USGS), Sentinel (gratis dari ESA).

---

👉 Jadi kalau untuk belajar Web GIS dan kasus **mangrove**, kamu bisa mulai dengan **data Landsat 8 atau Sentinel-2** (optical, untuk lihat vegetasi), lalu kombinasikan dengan **data radar Sentinel-1** untuk memastikan hasilnya tetap bisa dipantau walaupun berawan.

Mau saya bikinkan contoh **visual perbandingan citra satelit Landsat vs Sentinel untuk area mangrove** biar lebih mudah dipahami?

[![Comparison between a Landsat 8 true color composite (a) and a Sentinel... | Download Scientific Diagram](https://images.openai.com/thumbnails/url/QQv29nicu5meUVJSUGylr5-al1xUWVCSmqJbkpRnoJdeXJJYkpmsl5yfq5-Zm5ieWmxfaAuUsXL0S7F0Tw40zrVMC88LyXZxrsg2C0sLMvQtNPTPtcyNKqzKL48vCQgOznTOjC91Mw8oVyu2NTQAABTkJYc)](https://www.researchgate.net/figure/Comparison-between-a-Landsat-8-true-color-composite-a-and-a-Sentinel-2-composite-b_fig9_323144596?utm_source=chatgpt.com)

Berikut ini contoh visual perbandingan **citra satelit Landsat** vs **Sentinel** (dan terkadang radar) agar kamu bisa melihat perbedaan jelasnya:

* Gambar kiri atas: perbandingan tampilan Landsat 8 warna sejati dengan Sentinel-2.
* Gambar kiri bawah: perbandingan resolusi NDVI dari berbagai satelit (Sentinel-2, Landsat, MODIS).
* Gambar kanan atas: perbandingan komposit Sentinel-2, radar (SAR), dan Landsat.
* Gambar kanan bawah: contoh tampilan Landsat-7 / Landsat-8 dan Sentinel-2 di area yang sama.

---

## 🔍 Apa yang Bisa Kita Amati dari Perbandingan Ini

1. **Resolusi visual berbeda**

   * Sentinel-2 biasanya tampil lebih tajam / detail dibanding Landsat (pada kondisi yang sesuai).
   * Dalam perbandingan NDVI, kisi-kisi (pixel) pada Landsat lebih besar (lebih kasar) daripada Sentinel dalam area yang sama.

2. **Detail fitur vegetasi**

   * Di citra Sentinel, kita bisa melihat struktur semak, batas pulau kecil, atau garis pantai lebih jelas.
   * Di Landsat, beberapa fitur kecil mungkin terlihat “kabur” atau tertutup oleh ukuran pixel yang lebih besar.

3. **Kondisi awan / bayangan**

   * Kadang citra optical (Landsat / Sentinel) punya awan yang menghalangi sebagian area.
   * Di gambar perbandingan (kanan atas), radar (SAR) tetap bisa “melihat” meskipun ada awan, sedangkan citra optical mungkin tidak.

---

## ✅ Kesimpulan untuk Proyek Mangrove Kamu

* Jika kamu pakai **Sentinel-2**, tampilan vegetasinya akan lebih detail dibanding menggunakan **Landsat** (asalkan kondisi cuacanya baik).
* Untuk Web GIS monitoring mangrove, memakai Sentinel mungkin memberi nilai tambah karena kamu bisa menangkap detail lebih kecil (misalnya garis pantai mangrove, semak kecil, patahan vegetasi).
* Tapi Landsat juga tetap berguna — terutama kalau data Sentinel tidak selalu tersedia (awan, coverage, dsb.).

---

Mau saya kirimkan contoh nyata citra mangrove di area Indonesia dari Landsat & Sentinel supaya lebih dekat dengan kondisi lokalmu?

Oke, saya ringkas dan sederhanakan isi bagian **4.2.2 – 4.2.5** ini supaya lebih mudah dipahami, terutama dalam konteks belajarmu untuk Web GIS.

---

## 📡 Inti Penjelasan Satelit untuk Data Citra di GIS

### 1. **SPOT (Prancis)**

* Mulai diluncurkan sejak **1986**.
* Resolusi makin membaik tiap generasi:

  * SPOT 1–4 → 10 m (hitam putih) & 20 m (multiband).
  * SPOT 5 (2002) → 2.5 m – 5 m (hitam putih) & 10 m (multiband).
  * SPOT 6 & 7 (2012–2014) → **1.5 m (panchromatic)** & 6 m (multiband RGB + NIR).
* Sekarang citranya dijual oleh **Airbus Defence and Space**.
* Cocok buat: **pemetaan detail lahan, pertanian, dan monitoring hutan**.

➡️ **Analogi sederhana:** Bayangkan kamera HP dari tahun 2000-an yang resolusinya kecil → makin ke sini kualitas foto makin detail. Begitu juga SPOT, makin jelas tiap generasi.

---

### 2. **Digital Globe (Amerika)**

* Perusahaan swasta penyedia **citra resolusi sangat tinggi**.
* Produk populer: **Ikonos, QuickBird, GeoEye, WorldView (1–4)**.
* Resolusi:

  * **WorldView-4 → 31 cm (panchromatic), 1.24 m (multispectral)**.
* Ikonos & QuickBird sudah tidak aktif, tapi arsip citranya masih bisa dipakai.

➡️ **Contoh nyata:** Kalau kamu mau lihat mobil, jalan kecil, atau rumah di peta dengan jelas → pakai citra Digital Globe. Sangat detail, seperti Google Maps satelit yang zoom sampai atap rumah.

---

### 3. **Sentinel (Eropa / ESA)**

* Data **gratis** dan banyak dipakai di penelitian.
* Ada beberapa misi:

  * **Sentinel-1 (2014)** → Radar (SAR), bisa menembus awan & gelap, resolusi 20 m.
  * **Sentinel-2 (2015)** → 13 band (warna tampak, NIR, SWIR), resolusi **10 m, 20 m, 60 m**.
  * **Sentinel-3 (2016)** → Lebih fokus untuk **kelautan & atmosfer** (misalnya tinggi permukaan laut, suhu laut).

➡️ **Kelebihan:** Gratis + detail cukup bagus → cocok untuk monitoring lingkungan, hutan, mangrove, pertanian.

---

### 4. **Terra (NASA, 1999)**

* Bagian dari program **Earth Observing System**.
* Membawa instrumen:

  * **ASTER** → resolusi 15 m (visible & NIR), 30 m (SWIR), 90 m (thermal). Cocok untuk analisis lahan & perubahan tutupan.
  * **MODIS** → resolusi 250–1000 m, liputan global setiap **1–2 hari sekali**. Cocok untuk monitoring skala besar (misalnya kabut asap, suhu global, atau vegetasi seluruh benua).

➡️ **Analogi:**

* **ASTER** = kamera drone menengah (cukup detail untuk lahan / gunung).
* **MODIS** = kamera CCTV dari luar angkasa (kurang detail, tapi bisa memantau seluruh bumi setiap hari).

---

## 🎯 Kesimpulan (untuk Web GIS Mangrove)

* **SPOT** → bagus untuk detail menengah (misal lihat blok tanam mangrove).
* **Digital Globe (WorldView)** → paling detail (bisa lihat garis kecil, rumah, kapal). Tapi **berbayar**.
* **Sentinel** → **gratis + cukup detail** (10 m), ideal untuk monitoring mangrove jangka panjang di Web GIS.
* **Terra (ASTER/MODIS)** → cocok kalau mau lihat perubahan lingkungan besar (kabut asap, suhu laut, deforestasi luas), tapi kurang detail untuk plot kecil.

---

👉 Jadi kalau kamu bikin **Web GIS untuk mangrove**, **Sentinel-2** adalah pilihan paling realistis (gratis, cukup detail, update reguler). Kalau ada dana lebih, bisa kombinasikan dengan **Digital Globe** untuk detail tinggi.

---

Mau saya bikinkan **tabel perbandingan sederhana (SPOT vs Digital Globe vs Sentinel vs Terra)** biar kamu lebih cepat nangkep perbedaannya?

Oke, saya bantu sederhanakan ya agar mudah dipahami untuk bekal kamu di **Web GIS**.
Tadi teks panjangnya menjelaskan tentang **DEM (Digital Elevation Model)**, yaitu **data ketinggian permukaan bumi** yang bisa kita pakai untuk analisis topografi, kontur, ketinggian, lereng, dsb. DEM ini sangat penting dalam banyak aplikasi GIS, termasuk kalau kamu mau buat **Web GIS** untuk mangrove, pertanian, atau lingkungan.

---

## 📌 Inti Sari DEM (Digital Elevation Model)

1. **Apa itu DEM?**
   DEM adalah **peta digital** yang menyimpan informasi ketinggian permukaan bumi dalam bentuk grid (kotak-kotak).

   * Setiap kotak punya nilai tinggi (elevasi).
   * Bisa dipakai untuk membuat peta 3D, analisis banjir, aliran sungai, hingga peta lereng.

   👉 Bayangkan DEM seperti **versi digital dari peta kontur**, tapi lebih detail dan serba otomatis.

---

## 📌 Cara Membuat DEM (tiga teknologi utama)

1. **Optical Sensors (Sensor Optik / Satelit Foto Stereo)**

   * Menggunakan **foto satelit dari dua sudut berbeda** (seperti mata kiri & mata kanan melihat 3D).
   * Contoh: **ASTER** (30m resolusi), **SPOT 5** (20m).
   * Cocok untuk membuat DEM skala menengah (detail cukup, tapi tidak setajam LiDAR).

   👉 Analogi: kayak kamu ambil foto suatu benda dari kiri & kanan, lalu dibuat efek 3D.

---

2. **InSAR (Interferometric Synthetic Aperture Radar)**

   * Menggunakan **gelombang radar** (bukan cahaya). Radar menembus awan dan bisa dipakai siang atau malam.
   * Contoh: **SRTM (Shuttle Radar Topography Mission)**, resolusi 30m & 90m.
   * Data gratis dan banyak dipakai di Web GIS.

   👉 Analogi: kayak "sonar kapal selam" tapi ditembakkan dari satelit untuk ukur tinggi tanah.

---

3. **LiDAR (Light Detection and Ranging)**

   * Menggunakan **laser yang ditembakkan dari pesawat/drone**, lalu dihitung waktu pantulannya.
   * Sangat akurat, bisa sampai **0.5m – 2m resolusi**.
   * Bisa bedakan **tinggi tanah (ground)** dan **tinggi vegetasi (kanopi pohon mangrove misalnya)**.
   * Cocok banget kalau mau analisis pertumbuhan mangrove.

   👉 Analogi: kayak kamu teriak di gua "HALO!" dan dengar gema, dari lama gaungnya kamu bisa tahu jarak dinding gua.

---

## 📌 Kenapa DEM penting untuk Web GIS kamu?

* Kalau bikin **Web GIS Mangrove**, DEM bisa dipakai untuk:

  * **Analisis banjir & pasang surut** (apakah mangrove akan terendam air laut).
  * **Model pertumbuhan**: tinggi tanah + tinggi vegetasi.
  * **Monitoring perubahan**: dengan data LiDAR/Sentinel bisa lihat apakah mangrove makin tinggi atau gundul.

---

## 📌 Ringkasan mudah:

* **Optical Sensor → foto satelit stereo → detail menengah.**
* **InSAR → radar satelit → data gratis, tahan awan, detail sedang.**
* **LiDAR → laser dari pesawat/drone → super detail, bisa bedakan tanah vs vegetasi.**

Kalau untuk awal (tanpa drone), kamu bisa pakai **SRTM DEM (gratis, 30m)** atau **Copernicus DEM (10m, Sentinel)**.
Kalau nanti startup kamu sudah kuat, bisa tambah **LiDAR** buat detail pertumbuhan mangrove.

---

👉 Jadi, intinya DEM adalah **peta tinggi digital** yang jadi dasar penting di Web GIS.
Kalau kamu bikin Web GIS mangrove, kombinasikan **citra satelit (Landsat/Sentinel untuk sebaran tanaman)** + **DEM (SRTM/Sentinel untuk elevasi & resiko abrasi)** = sudah cukup kuat buat analisis awal.

Mau saya bikinkan **contoh workflow sederhana** bagaimana data DEM + citra satelit dipakai untuk **Web GIS mangrove**?

Oke 👍 saya bikinkan contoh **workflow sederhana** bagaimana **DEM + citra satelit** bisa dipakai untuk **Web GIS Mangrove**.

---

# 🌱 Workflow Sederhana Web GIS Mangrove (DEM + Citra Satelit)

## 1. **Data Input**

* **Citra Satelit**

  * Landsat (30m, gratis) → monitoring jangka panjang
  * Sentinel-2 (10m, gratis) → monitoring detail pertumbuhan mangrove
* **DEM (Digital Elevation Model)**

  * SRTM (30m, gratis) → dasar topografi & elevasi
  * Copernicus DEM (10m) → lebih detail
  * (opsional ke depan: LiDAR untuk detail tinggi pohon mangrove)

---

## 2. **Preprocessing Data**

* **Citra Satelit**

  * Koreksi awan & atmosfer (pakai Google Earth Engine / QGIS)
  * Band kombinasi untuk vegetasi (NDVI / NDMI → menghitung kehijauan/kelembaban mangrove)
* **DEM**

  * Clip sesuai area studi (misal lahan tanam 3 hektar)
  * Hitung slope (kemiringan) & elevation (ketinggian)

---

## 3. **Analisis GIS**

* Gabungkan **NDVI (dari citra satelit)** + **DEM (elevasi & slope)** untuk analisis:

  * **Kesehatan mangrove** (apakah hijau & rapat)
  * **Tinggi & lokasi lahan** (apakah rawan terendam air laut)
  * **Potensi pertumbuhan** (lereng landai + dekat pantai cocok untuk mangrove)
* Buat **zonasi**:

  * Area mangrove sehat
  * Area mangrove rusak / butuh rehabilitasi
  * Area rawan abrasi / banjir rob

---

## 4. **Output Peta**

* **Layer Citra Satelit** → warna hijau lebat = mangrove sehat, coklat = rusak
* **Layer DEM** → kontur, elevasi rendah = rawan abrasi
* **Layer Zonasi** → hasil overlay analisis (sehat, rusak, rawan)

---

## 5. **Publikasi Web GIS**

* Simpan data ke format **GeoJSON / GeoTIFF**
* Gunakan platform Web GIS:

  * **Leaflet.js** / **OpenLayers** (open-source, ringan)
  * **GeoServer** (jika butuh WMS/WFS)
  * **Mapbox / Google Maps API** (jika mau peta interaktif modern)
* Tampilkan layer:

  * **Basemap** (OpenStreetMap / Google Satelit)
  * **Layer DEM** (kontur / hillshade)
  * **Layer Citra Satelit** (NDVI/true color)
  * **Layer Zonasi Mangrove** (hasil analisis)

---

## 6. **Update Berkala**

* Setiap 6 bulan → update dengan citra Sentinel-2 terbaru
* Bandingkan NDVI lama vs baru → dapatkan tren pertumbuhan mangrove
* Simpan hasil ke database & tampilkan grafik di Web GIS

---

📌 **Contoh sederhana di Web GIS:**

* User buka website → pilih tahun (2024 / 2025)
* Peta tampil:

  * Background satelit
  * Overlay DEM (warna kontur)
  * Highlight area mangrove sehat vs rusak
* User klik area → keluar info: *elevasi 2m, NDVI 0.75 (sehat), status: aman dari abrasi*

---

👉 Jadi alurnya:
**Satelit (NDVI)** + **DEM (tinggi tanah)** → **Analisis kesehatan & resiko** → **Zonasi** → **Peta interaktif di Web GIS**.

---

Mau saya bikinkan **diagram visual workflow** (alur panah dari data → analisis → Web GIS) supaya lebih gampang dipahami?


Oke, saya jelaskan ulang bagian **4.4 Other Types of Raster Data** dengan bahasa lebih sederhana + contoh sehari-hari supaya mudah dipahami untuk bekal belajar Web GIS.

---

# 📌 Jenis-Jenis Data Raster Lainnya

## 1. **Digital Orthophoto (DOQ)**

* **Apa itu?**
  Foto udara atau citra yang sudah dikoreksi supaya tidak miring/terdistorsi akibat kamera miring atau permukaan tanah yang naik turun.
* **Analoginya:** seperti foto panorama di HP yang “diluruskan” biar tidak bengkok.
* **Kegunaan:** bisa dipakai sebagai **peta dasar** di GIS, cocok untuk mengecek keakuratan jalan, batas tanah, atau objek lain.

---

## 2. **Land Cover Data (Data Tutupan Lahan)**

* **Apa itu?**
  Peta yang menunjukkan jenis tutupan lahan: hutan, sawah, mangrove, permukiman, air, dll. Biasanya dibuat dari citra satelit yang diklasifikasikan.
* **Contoh:** dataset NLCD (di Amerika) yang membagi peta menjadi 16 kelas lahan dengan resolusi 30 meter.
* **Kegunaan:** sangat penting untuk analisis lingkungan, perubahan lahan, atau perencanaan wilayah.

---

## 3. **Bi-Level Scanned Files (Hitam-Putih)**

* **Apa itu?**
  Hasil scan peta kertas lama, tapi hanya dua nilai: **1 (hitam)** dan **0 (putih)**.
* **Analoginya:** seperti scan tanda tangan hitam-putih, tidak ada warna abu-abu.
* **Kegunaan:** dipakai untuk proses **digitasi** → mengubah gambar kertas jadi data vektor (jalan, batas tanah, dll).

---

## 4. **Digital Raster Graphics (DRG)**

* **Apa itu?**
  Hasil scan peta topografi kertas (misalnya peta gunung/lereng dari USGS) → dibuat digital dengan resolusi sekitar 2.4 meter.
* **Kegunaan:** dipakai sebagai **background peta** di GIS, untuk melengkapi analisis spasial.

---

## 5. **Graphic Files (File Gambar Umum)**

* **Apa itu?**
  File raster biasa yang kita kenal sehari-hari:

  * **TIFF** → kualitas tinggi, besar.
  * **JPEG** → umum, ukuran kecil, ada kompresi.
  * **GIF** → bisa animasi, tapi terbatas warnanya.
* **Kegunaan:** bisa dipakai di GIS kalau ada georeferensi (koordinat).

---

## 6. **GIS Software-Specific Raster Data**

* **Apa itu?**
  Format raster khusus yang dipakai software GIS tertentu.

  * Contoh: **Esri Grid** (format lama di ArcGIS).
  * Sekarang, banyak software sudah pakai **TIFF (GeoTIFF)** karena lebih umum dan kompatibel.
* **Kegunaan:** memastikan data raster bisa dipakai langsung di software GIS tanpa banyak konversi.

---

# 🌱 Inti Sari untuk Web GIS

Kalau mau membangun Web GIS (misalnya untuk mangrove):

* **Orthophoto / Citra Satelit** → jadi **background peta** yang realistik.
* **Land Cover Data** → untuk analisis area mangrove vs non-mangrove.
* **DEM (dari sebelumnya)** → untuk analisis ketinggian & abrasi.
* **Format umum (GeoTIFF, JPEG, PNG)** → lebih mudah dipublikasikan ke web.

---

👉 Jadi, pikirkan data raster seperti **lapisan gambar peta** dengan fungsi berbeda-beda:

* Foto satelit = background
* Land cover = klasifikasi jenis lahan
* DEM = ketinggian
* Scan peta lama = referensi tambahan

---

Mau saya bikinkan **contoh tabel ringkas + contoh aplikasinya di Web GIS mangrove** biar makin mudah dipahami?

Oke, saya jelaskan ulang tentang **Raster Data Structure (Struktur Data Raster)** dengan bahasa yang lebih sederhana + analogi biar gampang dipahami untuk belajar Web GIS 🚀.

---

# 📌 Struktur Data Raster (Raster Data Structure)

**Raster** = data spasial berbentuk **grid (kotak-kotak)**, di mana tiap kotak (cell/pixel) punya nilai.
Pertanyaannya: **gimana cara komputer menyimpan grid besar ini dengan efisien?**
Nah, ada 3 cara utama:

---

## 1. **Cell-by-Cell Encoding**

* **Cara kerja:** setiap pixel disimpan **satu per satu** berdasarkan baris dan kolom.
* **Contoh:** peta elevasi (DEM) → karena tiap kotak punya nilai ketinggian yang berbeda-beda.
* **Kelebihan:** sederhana, gampang dipahami.
* **Kekurangan:** boros memori kalau datanya besar.
* **Analoginya:** kayak bikin tabel Excel besar → semua angka ditulis cell per cell.

👉 **Cocok untuk:** citra satelit, DEM, data yang nilainya **berubah-ubah terus**.

---

## 2. **Run-Length Encoding (RLE)**

* **Cara kerja:** kalau banyak pixel bernilai sama, cukup simpan **rentang (run)**, bukan semua cell.
* **Contoh:** peta hitam putih hasil scan (0 = putih, 1 = hitam).
  Daripada tulis `0,0,0,0,0,0,1,1,1,0,0`, lebih hemat ditulis `mulai kolom 6 sampai 8 = 1`.
* **Kelebihan:** file jadi kecil (hemat memori).
* **Kekurangan:** kurang cocok untuk data yang sering berubah (seperti DEM).
* **Analoginya:** kayak nyatet barisan kursi bioskop: daripada bilang "kursi 1 kosong, kursi 2 kosong, kursi 3 kosong, kursi 4 terisi", cukup bilang **kursi 1–3 kosong, kursi 4 terisi**.

👉 **Cocok untuk:** data dengan **pola berulang** (peta hasil scan, citra biner).

---

## 3. **Quadtree**

* **Cara kerja:** raster dibagi terus jadi **empat bagian (quad)** → kalau di dalam kotak nilainya sama, simpan sebagai satu blok. Kalau berbeda, pecah lagi jadi 4.
* **Kelebihan:** hemat untuk data yang punya **pola blok-blok besar**.
* **Kekurangan:** agak rumit strukturnya.
* **Analoginya:** seperti peta hutan: kalau 1 kotak besar semuanya "hutan", cukup tulis "hutan" tanpa detail pixel. Tapi kalau separuh hutan, separuh laut → kotak itu dipecah jadi 4, lalu dicatat per bagian.

👉 **Cocok untuk:** menyimpan data area besar dengan nilai seragam (misalnya peta tutupan lahan).

---

## 4. **Header File**

* **Apa itu?** file tambahan yang berisi info dasar raster:

  * ukuran cell
  * jumlah baris/kolom
  * jumlah band (untuk citra satelit)
  * koordinat peta
  * nilai “no data”
* **Fungsinya:** biar software GIS tahu cara membaca isi raster.

👉 **Analoginya:** seperti “sampul buku” yang memberi info judul, penulis, halaman → tanpa itu, isinya susah dipahami.

---

# 🌱 Inti Sari untuk Web GIS

Kalau nanti kamu bikin Web GIS (contoh: **mangrove monitoring**):

* **Cell-by-cell** → dipakai untuk citra satelit & DEM (karena data detail).
* **RLE** → berguna kalau mau compress peta hasil scan.
* **Quadtree** → bisa dipakai untuk percepat pencarian data di peta besar (efisiensi server).
* **Header file** → penting biar aplikasi Web GIS tahu ukuran & posisi data.

---

👉 Jadi, intinya struktur raster ini soal **efisiensi penyimpanan dan pembacaan data peta**.

* Kalau **akurat & detail** → Cell-by-cell.
* Kalau **hemat & banyak pengulangan** → Run-length encoding.
* Kalau **blok-blok seragam** → Quadtree.

---

Mau saya bikinkan **contoh workflow kecil: bagaimana Web GIS mangrove bisa memanfaatkan DEM (cell-by-cell) + land cover (quadtree) → untuk analisis erosi/abrasi**?


Oke, saya bikinkan **contoh workflow kecil** yang sederhana bagaimana **data raster (DEM & citra satelit) dipakai di WebGIS mangrove**. Saya buat dalam bentuk langkah-langkah + ilustrasi alurnya biar mudah dipahami:

---

## 🌱 Workflow Kecil WebGIS Mangrove (dengan DEM + Citra Satelit)

1. **Koleksi Data**

   * **DEM (Digital Elevation Model)** → data ketinggian (raster).
   * **Citra Satelit (misalnya Sentinel-2)** → data kondisi tutupan lahan/mangrove.

   ➝ Contoh: Download dari USGS EarthExplorer atau Copernicus Hub.

---

2. **Pengolahan Data Raster**

   * **DEM** → diproses untuk membuat peta kontur, slope (kemiringan), dan ketinggian.
   * **Citra Satelit** → klasifikasi tutupan lahan (misalnya mangrove, air, non-mangrove).

   ➝ Hasilnya berupa raster siap pakai:

   * Raster kontur/slope (dari DEM).
   * Raster klasifikasi mangrove (dari citra satelit).

---

3. **Konversi ke Format WebGIS**

   * Raster disimpan dalam **GeoTIFF** atau dikonversi ke **tiles (XYZ/WMTS)**.
   * Vector hasil digitasi (misalnya batas mangrove) bisa disimpan dalam **GeoJSON / Shapefile**.

---

4. **Simpan di Server**

   * Gunakan **PostGIS** (jika database) atau simpan di **GeoServer** untuk publish layer.
   * Alternatif ringan: Simpan di GitHub / Tileserver untuk dataset statis.

---

5. **Tampilkan di WebGIS**

   * Gunakan **Leaflet.js** atau **OpenLayers**.
   * Tambahkan layer:

     * Layer DEM (hillshade/contour).
     * Layer mangrove (hasil klasifikasi citra satelit).

   ➝ Hasil: Peta interaktif WebGIS yang bisa zoom-in, klik lokasi, lihat tinggi tanah & tutupan mangrove.

---

### 📊 Ilustrasi Workflow Sederhana

```
[DEM] ----> [Ekstraksi Elevasi & Slope] ----\
                                             ---> [GeoServer/PostGIS] ---> [WebGIS (Leaflet)]
[Citra Satelit] --> [Klasifikasi Mangrove] --/
```

---

👉 Workflow kecil ini bisa jadi dasar untuk **sistem WebGIS monitoring mangrove**. Nanti bisa ditambah fitur analisis (misalnya: perbandingan citra tahun 2010 vs 2025 untuk lihat deforestasi mangrove).

Mau saya bikinkan **contoh kode kecil Leaflet** yang menampilkan layer DEM + Mangrove hasil klasifikasi?

Oke, saya bantu sederhanakan isi bagian **Raster Data Compression** biar lebih gampang dipahami dan bisa langsung nyambung ke penggunaan WebGIS 🚀

---

## 📌 Inti Konsep Raster Data Compression

**Raster Data Compression = teknik untuk mengecilkan ukuran file raster (citra satelit, DEM, peta hasil scanning)** supaya:

* lebih hemat ruang penyimpanan,
* lebih cepat dikirim/diakses (misalnya untuk WebGIS).

---

### 🟢 Jenis Utama Kompresi

1. **Lossless Compression (tanpa kehilangan data)**

   * Semua nilai piksel **tetap sama persis** setelah dikompres & diekstrak lagi.
   * Cocok untuk **analisis GIS** karena butuh nilai asli (misalnya nilai elevasi DEM atau NDVI citra satelit).
   * Contoh: **RLE (Run-Length Encoding), LZW, LZ77, LZMA**.

   ➝ Analogi: seperti kamu zip file Excel. Saat diextract, semua angka tetap sama, tidak ada yang berubah.

---

2. **Lossy Compression (dengan kehilangan data)**

   * Ada **sedikit perubahan nilai piksel** setelah dikompres.
   * Hasilnya tidak 100% sama dengan aslinya, tapi **ukuran file jauh lebih kecil**.
   * Cocok untuk **tampilan visual** (misalnya peta dasar/background di WebGIS).
   * Contoh: **JPEG, JPEG 2000, MrSID, ECW**.

   ➝ Analogi: seperti foto di HP, kalau dikompres jadi kecil, kualitas turun sedikit, tapi mata manusia hampir tidak bisa bedakan.

---

### 🟠 Wavelet-based Compression (MrSID, JPEG 2000, ECW)

* **Teknologi modern** untuk kompresi raster.
* Menggunakan **matematika “gelombang” (wavelet transform)** → gambar dipecah jadi komponen sederhana.
* Bagian yang **detail** (banyak variasi, misalnya tepi hutan, kota) disimpan dengan resolusi tinggi.
* Bagian yang **seragam** (misalnya laut atau langit biru) disimpan dengan resolusi rendah.

  ➝ Hasilnya: ukuran file jauh lebih kecil, tapi tetap terlihat detail pada area penting.

---

### ⚡ Kenapa Penting untuk WebGIS?

* Citra satelit atau DEM bisa **puluhan GB** → kalau tidak dikompres, WebGIS akan lemot.
* Dengan kompresi:

  * **Lossless** → dipakai kalau WebGIS butuh analisis data asli (misalnya DEM untuk perhitungan banjir).
  * **Lossy** → dipakai kalau hanya sebagai background peta (misalnya tampilan mangrove di dashboard).

---

### 📊 Contoh Sederhana

Bayangkan kamu punya **peta citra satelit ukuran 1 GB**:

* **Tanpa kompresi**: user butuh waktu lama untuk buka peta di WebGIS.
* **Lossless (zip/RLE)**: file jadi **200 MB**, semua piksel tetap asli → aman untuk analisis.
* **Lossy (JPEG 2000)**: file bisa jadi **50 MB** saja, kualitas tampilan masih oke → cepat untuk WebGIS background.

---

👉 Jadi intinya:

* **Kalau buat analisis GIS** → pakai **lossless** (data asli tetap terjaga).
* **Kalau buat tampilan WebGIS** → bisa pakai **lossy** (lebih ringan & cepat, tapi tidak cocok untuk analisis numerik).

---

Mau saya bikinkan **diagram visual alur kompresi raster (lossless vs lossy) dengan contoh DEM + citra satelit** biar makin mudah dipahami?

Oke, saya bantu sederhanakan isi bab **4.7 Data Conversion and Integration** biar lebih gampang dipahami dan langsung nyambung ke pemakaian WebGIS 🚀

---

## 📌 Inti Konsep

Di dunia GIS ada **dua jenis data utama**:

* **Raster** → berupa grid/pixel (contoh: citra satelit, DEM, foto udara).
* **Vector** → berupa titik, garis, dan poligon (contoh: lokasi pohon, jalan, batas lahan).

Masalahnya: **Raster dan Vector itu berbeda struktur datanya**, jadi kadang kita perlu **konversi** atau **integrasi** supaya bisa dipakai bersama.

---

## 🟢 1. Rasterization (Vector → Raster)

* Mengubah data **vektor** (titik, garis, poligon) jadi **raster** (grid).
* Caranya: buat grid, lalu isi piksel dengan nilai sesuai lokasi objek.

  * Titik → 1 piksel.
  * Garis → diwarnai sepanjang grid.
  * Poligon → semua piksel di dalamnya diisi nilai tertentu.

👉 Contoh di WebGIS mangrove:
Peta batas kawasan mangrove (poligon) diubah ke raster supaya bisa dianalisis dengan citra satelit (misalnya menghitung kerapatan vegetasi di dalam kawasan itu).

---

## 🟢 2. Vectorization (Raster → Vector)

* Kebalikannya, mengubah data **raster** jadi **vektor**.
* Biasanya dipakai untuk ekstraksi garis atau batas dari citra.
* Prosesnya:

  1. **Line thinning** → menipiskan garis raster biar jadi 1 piksel.
  2. **Line extraction** → ambil awal & akhir garis.
  3. **Topological reconstruction** → sambungkan garis-garis jadi poligon/objek vektor.

👉 Contoh:
Scan peta lama (hasil raster) dikonversi ke poligon batas desa supaya bisa dipakai di WebGIS interaktif.

---

## 🟢 3. Integrasi Raster + Vector

Biasanya dilakukan agar **analisis GIS lebih lengkap**:

* **Citra satelit / DEM (raster)** dipakai sebagai **background** atau sumber data.
* **Vector** dipakai untuk informasi tambahan: jalan, batas desa, lokasi kelompok tani, titik penanaman.

👉 Contoh dalam WebGIS mangrove:

* Raster: citra Sentinel-2 (warna vegetasi).
* Vector: batas kelompok tani, titik lokasi penanaman, poligon area rehabilitasi.
* Hasil integrasi: pengguna bisa melihat “di area siapa” dan “berapa luas mangrove tumbuh” dalam peta interaktif.

---

## 🟡 Analogi Sederhana

Bayangkan kamu bikin **game strategi**:

* **Raster** = peta dunia (gunung, hutan, sungai) → seperti background.
* **Vector** = unit tentara, kota, batas wilayah → yang bisa diklik & punya data tambahan.
* Supaya seru → kamu butuh **keduanya jalan bareng**.

---

## 📊 Inti Sari untuk WebGIS

1. **Rasterization** → kalau kamu mau analisis spasial berbasis grid (misalnya overlay citra satelit dengan batas lahan).
2. **Vectorization** → kalau kamu mau ambil garis/poligon dari data raster (misalnya scan peta jadi batas digital).
3. **Integrasi raster & vector** → inilah inti WebGIS modern (contoh: Google Maps, yang gabungin citra satelit + jalan + bangunan 3D).

---

👉 Jadi, untuk WebGIS mangrove:

* Raster = citra satelit (lihat kerapatan vegetasi).
* Vector = poligon batas lahan + titik lokasi penanaman.
* Integrasi = gabungkan keduanya supaya peta **informatif + analisis jalan**.

---

Mau saya bikinkan **workflow visual sederhana** (alur data raster + vector → integrasi → WebGIS) biar lebih gampang nempel?

Oke, saya coba sederhanakan istilah-istilah tadi biar lebih mudah dipahami dengan bahasa sehari-hari. Anggap saja kita sedang belajar GIS untuk lahan mangrove:

---

### 📌 Istilah Dasar Raster & Citra

* **Bi-level scanned file** → File hasil scan yang cuma punya **dua nilai**, hitam (0) atau putih (1).
  👉 Contoh: hasil scan peta garis hitam-putih.

* **Cell-by-cell encoding** → Data raster disimpan **sel per sel** dalam bentuk baris dan kolom.
  👉 Bayangkan Excel, tiap kotak berisi angka tinggi atau warna.

* **Raster data model** → Model data berupa **grid kotak-kotak** (baris-kolom) untuk mewakili permukaan bumi.
  👉 Seperti piksel pada foto.

* **Rasterization** → Mengubah data **vektor (garis/polygon)** jadi kotak-kotak raster.
  👉 Misalnya peta batas desa (vektor) diubah jadi raster.

* **Vectorization** → Kebalikannya: mengubah raster jadi garis/polygon vektor.
  👉 Misalnya foto satelit (raster) diubah jadi batas sungai (vektor).

---

### 📌 Jenis Raster

* **Digital Elevation Model (DEM)** → Raster berisi nilai **ketinggian tanah**.
  👉 Contoh: peta elevasi mangrove (0–20 meter).

* **Digital Orthophoto Quad (DOQ)** → Foto udara yang sudah **dikoreksi miring** kamera dan topografi.
  👉 Jadi ukurannya akurat, bisa dipakai ukur jarak.

* **Digital Raster Graphic (DRG)** → Peta topografi kertas hasil scan.
  👉 Contoh: peta gunung USGS versi digital.

* **Esri grid** → Format khusus raster milik software ArcGIS.

* **Integer raster** → Raster berisi angka bulat (contoh: 0 = air, 1 = mangrove, 2 = pemukiman).

* **Floating-point raster** → Raster berisi angka pecahan (contoh: ketinggian 1.25 m, 2.67 m).

* **Georeferenced raster** → Raster yang sudah ditempatkan di **koordinat peta** sebenarnya.
  👉 Jadi bisa “nempel” di dunia nyata.

---

### 📌 Satelit & Citra

* **Landsat** → Satelit pemantau bumi (citra gratis, resolusi 30m).
  👉 Bisa dipakai pantau mangrove tiap 16 hari.

---

### 📌 Kompresi Raster

* **Data compression** → Mengecilkan ukuran data raster biar hemat penyimpanan.

* **Lossless compression** → Kompres tapi **data aslinya tetap utuh**.
  👉 Seperti zip file → saat dibuka hasilnya sama persis.

* **Lossy compression** → Kompres tapi ada **informasi yang hilang**.
  👉 Seperti JPEG foto → ukuran kecil tapi agak blur.

* **Run-length encoding (RLE)** → Cara simpan data dengan menghitung berapa kali nilai yang sama muncul.
  👉 Misal baris raster: 0001111000 → disimpan sebagai (3x0, 4x1, 3x0).

* **Quadtree** → Raster dibagi jadi kotak besar, lalu dipecah jadi kotak kecil kalau butuh detail.
  👉 Hemat penyimpanan karena area kosong tidak disimpan detail.

* **Wavelet transform** → Teknik kompresi citra dengan memperlakukan gambar seperti “gelombang” yang diuraikan jadi lebih sederhana.
  👉 Banyak dipakai di JPEG2000.

---

📍 **Kesimpulan sederhana**:
Raster itu ibarat **foto grid** bumi, tiap kotaknya berisi angka (warna, ketinggian, atau kategori). Untuk ditampilkan di Web GIS mangrove, biasanya dipakai **citra satelit (Landsat, Sentinel)** + **DEM** → bisa dikompresi supaya lebih ringan (lossless kalau data penting, lossy kalau sekadar visual).

---

Mau saya bikinkan **contoh ilustrasi visual sederhana** (misalnya DEM + citra satelit mangrove → sebelum & sesudah kompresi) biar lebih gampang dicerna?


Oke, teks panjang tadi saya sederhanakan biar gampang dipahami, jadi semacam **tutorial ringan + poin-poin utama** tentang penggunaan **raster data model** di ArcGIS.

---

## 📘 Aplikasi Raster Data Model (dibagi 4 tugas utama)

### **Task 1 – Melihat & Impor Data DEM**

* **Bahan:** file `menanbuttes.txt` → berisi data elevasi dalam format ASCII DEM.
* **Langkah utama:**

  1. Buka file di ArcMap → header menunjukkan info:

     * Jumlah kolom = 341, baris = 466
     * Koordinat pojok kiri bawah = (419475, 4844265)
     * Ukuran sel = 30 meter
     * Nilai No Data = -9999
  2. Konversi ASCII → Raster (pakai tool *ASCII to Raster*).
  3. Cek properti raster → cell size = 30 m, nilai elevasi min = 4771 ft, max = 5619 ft.
  4. Atur **symbology** dengan color ramp (misalnya Elevation #1) supaya lebih mudah dibaca.

👉 **Tujuan:** belajar membaca dan menampilkan peta elevasi (DEM).

---

### **Task 2 – Melihat Citra Satelit (Landsat TM)**

* **Bahan:** file `tmrect.bil` → citra Landsat dengan 5 band.

* **Info dasar:**

  * 366 baris, 651 kolom, 5 band, kedalaman piksel 8 bit.
  * Awalnya ditampilkan sebagai RGB Composite (Band 1 = merah, Band 2 = hijau, Band 3 = biru).

* **Langkah:**

  1. Tambahkan ke ArcMap.
  2. Ubah komposisi warna:

     * (R: Band 3, G: Band 2, B: Band 1) → hasilnya mirip **foto warna asli**.
     * (R: Band 4, G: Band 3, B: Band 2) → hasilnya **foto inframerah** (vegetasi tampak merah cerah).

👉 **Tujuan:** belajar kombinasi band untuk menampilkan citra satelit berbeda.

---

### **Task 3 – Melihat Peta Penutup Lahan**

* **Bahan:** file `Hawaii_LandCover_2005.img` → peta tutupan lahan Hawaii, format ERDAS IMAGINE.
* **Langkah:**

  1. Tambahkan raster ke ArcMap.
  2. Lihat legenda → ada 8 tipe tutupan lahan (misalnya hutan, pertanian, pemukiman, dll).
  3. Bisa zoom untuk melihat detail distribusi tutupan lahan.

👉 **Tujuan:** mengenali **data kategorikal (land cover)** vs **data kontinu (DEM, citra satelit)**.

---

### **Task 4 – Konversi Data Vektor ke Raster**

* **Bahan:**

  * `nwroads.shp` → data jalan (line)
  * `nwcounties.shp` → data batas county (polygon)

* **Langkah:**

  1. Konversi jalan → raster (pakai field nomor jalan, cell size = 5000 m).

     * Hasil: raster jalan dengan warna berbeda sesuai nomor jalan.
  2. Konversi county → raster (pakai field FIPS, cell size = 5000 m).

     * Hasil: raster county dengan nilai unik per wilayah.

👉 **Tujuan:** memahami **konversi vektor → raster**, serta efek ukuran cell (semakin kecil = lebih detail, semakin besar = lebih kasar).

---

### **Challenge Task**

* **Bahan:** `cwater.img` → raster floating-point.
* **Langkah:** ubah ke **integer raster** (pakai tool Int).
* **Catatan:** floating-point butuh lebih banyak memori, integer lebih ringan.

👉 **Tujuan:** memahami perbedaan **floating raster** (data kontinu seperti ketinggian, suhu) vs **integer raster** (data kategorikal).

---

## 🎯 Inti yang harus dipahami

1. **DEM** → data ketinggian (kontinu).
2. **Citra satelit (Landsat)** → data multi-band, bisa dikombinasikan untuk analisis.
3. **Land cover** → data kategorikal hasil klasifikasi.
4. **Konversi vektor → raster** → penting kalau mau overlay dengan data raster lain.
5. **Resolusi cell** berpengaruh pada detail & ukuran file.
6. **Floating-point vs integer raster** → beda jenis data yang disimpan.

---

Kalau mau saya bikinkan **diagram alur visual singkat** (workflow: Input DEM → Citra Satelit → Land Cover → Vektor → Raster) biar lebih gampang dibanding hafalan teks, mau?
