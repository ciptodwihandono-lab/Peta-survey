# Peta Survei

Aplikasi pemetaan lapangan untuk HP, setara aplikasi GIS lapangan seperti
QField, SW Maps, Locus GIS, atau Avenza Maps, dalam bahasa Indonesia. Bekerja **tanpa
sinyal** setelah dibuka sekali, dan memperbarui diri sendiri saat ada sinyal.

Alamat: https://ciptodwihandono-lab.github.io/Peta-survey/

## Memasang di HP

Buka alamat di atas di **aplikasi Chrome** saat ada sinyal. Lalu ketuk
**Pasang** pada pita biru di atas, atau tombol **Pasang aplikasi** di tab Data.
Cara lain lewat menu Chrome: **⋮ → Instal aplikasi**. Setelah itu Peta Survei
muncul di daftar aplikasi HP, terbuka layar penuh, dan bisa dipakai tanpa sinyal.
Di iPhone: Safari → Bagikan → Tambahkan ke Layar Utama.

## Fitur

**Peta Saya (seperti Avenza Maps)**
- Tab **Peta Saya**: pustaka peta di HP dengan gambar mini, jenis, tingkat zoom,
  ukuran, dan status posisi: "Anda di dalam peta ini" atau jarak dan arah ke peta.
  Urutkan menurut terdekat, terbaru, atau nama; cari nama peta; ganti nama; hapus.
- **Impor GeoPDF** (ISO 32000: QGIS "Create Geospatial PDF", ArcGIS "Export
  georeference information", peta RBI): halaman digambar dengan PDF.js, dipotong
  sesuai garis tepi peta (legenda dan judul di luar bingkai tidak ikut), lalu
  dijadikan tile.
- **Impor GeoTIFF** (ortofoto drone, citra satelit): WGS 84, Web Mercator,
  UTM WGS 84 utara/selatan, DGN95 UTM dan TM-3. Nilai *nodata* menjadi transparan;
  data 16-bit/float diregangkan otomatis.
- **Impor KMZ** berisi GroundOverlay (Google Earth, Garmin Custom Maps), termasuk
  rotasi dan `gx:LatLonQuad`.
- Penanda peta aktif di layar peta: nama peta dan **"Di luar peta · jarak arah"**.

**Pencarian**
- Tombol kaca pembesar di layar peta. Mencari objek survei (kode, nama, habitat,
  vegetasi, catatan, lapisan), nama peta, nama objek di layer acuan, dan
  **koordinat** yang diketik: desimal (`-1.17, 104.37`), derajat-menit-detik
  (`1°10'12" LS 104°22'12" BT`), atau UTM (`48M 429910 9870672`).
- Nama tempat (desa, sungai, pantai) lewat OpenStreetMap saat ada sinyal.
- Hasil ditandai di peta dengan tombol **Simpan sebagai titik** dan **Navigasi**.

**Lapisan objek**
- Kelompokkan objek per kegiatan, hari, atau tim. Objek baru masuk ke lapisan
  aktif; tiap lapisan bisa ditampilkan/disembunyikan, diganti nama, dihapus
  (objeknya dipindah). Kolom `lapisan` ikut di CSV, GeoJSON, Shapefile
  (`LAPISAN`), dan folder KML.

**Baca foto dengan AI (Claude)**
- Setiap foto baru bisa dibaca Claude (Anthropic, model `claude-opus-5-5`) untuk mengisi Deskripsi dan Catatan.
  Isian yang sudah diketik tidak ditimpa; teks AI ditambahkan di baris baru dengan awalan "AI:".
  Hasil AI juga disimpan di data foto (kolom `deskripsi_ai`, `catatan_ai` di `Daftar_Foto.csv`).
- Butuh kunci API Anthropic milik pengguna (tab Data). Kunci hanya disimpan di HP itu, dipakai bersama oleh
  Survei Cemara dan Peta Survei, dan tidak ikut file cadangan.
- Tanpa sinyal, foto diantrekan lalu dibaca otomatis saat ada sinyal. Foto dikirim diperkecil ke 1280 px.
- Pustaka: `vendor/anthropic-sdk.js` (Anthropic TypeScript SDK 1.3.0, dibundel dengan esbuild; lisensi di
  `vendor/LICENSE-anthropic-sdk.txt`).

**Peta dan peta offline**
- Peta dasar online: citra satelit Esri World Imagery atau OpenStreetMap.
  Bagian peta yang pernah tampil tersimpan otomatis (maksimal ±6000 tile).
- **Peta offline penuh**: impor file **MBTiles** atau **ZIP tile `{z}/{x}/{y}`**,
  format yang sama dengan QField/Locus. Tile disalin ke penyimpanan HP. Di luar
  cakupan peta offline, citra online atau yang tersimpan tetap tampil.
- Panel **Lapisan**: pilih peta dasar, impor peta offline, tampilkan atau
  sembunyikan objek, label kode, dan layer acuan.
- **Layer acuan**: batas kawasan atau rencana jalur dari GeoJSON/KML/GPX.

**GPS dan koordinat**
- Indikator kualitas GPS di bar atas (hijau ≤ 5 m, kuning ≤ 15 m, merah).
- Panel koordinat langsung: **UTM WGS 84** (mis. `48M 429923 mT 9870676 mU`),
  derajat desimal, atau derajat-menit-detik. Ketuk panel untuk berganti format.
- **Titik GPS rata-rata**: 10 bacaan berbobot 1/akurasi², bacaan > 20 m
  diabaikan. Ketelitian dan sebaran tercatat di atribut.
- Navigasi ke objek: jarak dan arah kompas (derajat dan U/TL/T/…) dengan garis
  panduan di peta.

**Digitasi**
- Titik, garis, dan area dengan ketuk peta atau **tanda silang di tengah layar**
  ("+ Titik tengah") untuk penempatan presisi.
- **Rekam** jalur GPS (titik > ±30 m diabaikan, layar dijaga menyala).
- **Ubah bentuk / geser titik**: seret titik sudut, ketuk untuk menghapus,
  ketuk peta untuk menyisipkan, dengan tombol urungkan.
- **Alat ukur** jarak, keliling, dan luas (tidak disimpan).

**Atribut dan foto**
- Tombol **Foto** di layar peta: kamera langsung terbuka, foto menjadi titik baru
  di posisi GPS (atau di tengah peta bila GPS belum siap), lalu formulirnya terbuka.
- Habitat (15 kelas berwarna; bisa lebih dari satu per objek, warna peta mengikuti yang dipilih pertama), kode otomatis P/G/A###, nama lokasi, kondisi,
  ancaman, vegetasi dominan, deskripsi, catatan.
- **Foto lokasi** dari kamera atau galeri, disimpan tanpa kompresi (file asli kamera, resolusi penuh), dengan GPS saat memotret.
- Tab **Objek**: ringkasan (jumlah, hektar, km, foto), tabel per habitat,
  pencarian, filter, dan jarak dari posisi Anda.

## Ekspor

- **Paket lengkap (ZIP)**:
  - Shapefile `shp/titik|garis|area` (WGS 84, atribut UTF-8, kolom UTM);
  - `peta_survei.geojson`, `peta_survei.kml`, `peta_survei.gpx`;
  - `objek.csv` (lat/lon dan UTM);
  - `foto/` dengan `Daftar_Foto.csv`;
  - cadangan `.json`.
- **Shapefile** saja (ZIP) dan **KMZ Google Earth** dengan foto di setiap objek.
- **Cadangan .json** untuk digabung di HP anggota tim lain.

## Membuat peta offline (QGIS)

1. Tambahkan citra di QGIS: Browser → XYZ Tiles → New Connection dengan URL
   `https://server.arcgisonline.com/ArcGIS/rest/services/World_Imagery/MapServer/tile/{z}/{y}/{x}`,
   citra drone sendiri, atau Sentinel-2.
2. Perbesar peta ke area survei.
3. Processing Toolbox → Raster tools → **Generate XYZ tiles (MBTiles)**.
4. Extent: kanvas peta; zoom 12–17 (18 untuk detail; tiap tingkat ±4× lebih
   besar); format JPG, kualitas 75.
5. Pindahkan file `.mbtiles` ke HP, lalu di aplikasi pilih Lapisan → Impor peta offline.

Hormati ketentuan penyedia citra. Untuk laporan resmi, citra drone sendiri atau
Sentinel-2 (lisensi terbuka) paling aman.

## Catatan teknis

- `index.html` satu file, dengan `vendor/leaflet.js` (Leaflet 1.9.4, BSD-2). Pustaka
  impor dimuat hanya saat dipakai: `vendor/sql-wasm.js|.wasm` (sql.js 1.10.3, MIT),
  `vendor/pdf.min.js|pdf.worker.min.js` (PDF.js 3.11.174, Apache-2.0),
  `vendor/geotiff.js` (geotiff.js 2.1.3, MIT), `vendor/proj4.js` (proj4js 2.11.0, MIT).
- Georeferensi GeoPDF dibaca langsung dari `/VP` → `/Measure` (`/GPTS`, `/LPTS`,
  `/Bounds`), termasuk objek di dalam *object stream* terkompresi, lalu dipasang
  homografi bujur/lintang → koordinat halaman. Peta raster dipotong menjadi tile
  Web Mercator 256 px dengan transformasi afin per tile (maks. ±15.000 tile).
- Penyimpanan:
  - data objek di `localStorage` (`peta-survei-v1`);
  - foto, layer acuan, dan tile peta offline di IndexedDB (`peta-survei-foto` v2:
    `foto`, `thumb`, `ref`, `maps`, `tiles`).
- UTM dihitung dengan seri Krüger. Hasil uji dibandingkan dengan pyproj
  (EPSG:32748) dan selisihnya < 1 cm. Shapefile tervalidasi dengan pyshp, dan
  cincin poligon searah jarum jam.
- Service worker `sw.js` hanya melayani file aplikasi ini dan tile online, dan tidak menghapus
  cache aplikasi lain di domain yang sama. Diterbitkan oleh `.github/workflows/pages.yml`.
