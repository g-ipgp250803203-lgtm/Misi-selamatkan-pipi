# Prompt: Permainan Kemahiran Bertutur "Papa Zola: Selamatkan Pipi!"

Salin semua teks di bawah garisan ini dan tampal ke Claude atau AI lain.

---

Bertindak sebagai pembangun web dan guru Bahasa Melayu sekolah rendah. Hasilkan SATU fail HTML (CSS dan JavaScript di dalam fail yang sama, tanpa gambar atau fail audio luar) untuk permainan menguji kemahiran bertutur murid. Tema cerita: "Papa Zola: Selamatkan Pipi!". Semua teks antara muka dalam Bahasa Melayu.

## 1. Konsep
Setiap situasi memaparkan satu grafik animasi dan satu ayat. Murid menekan butang mikrofon dan menyebut ayat itu. Jika sebutan betul dan sepadan dengan ayat yang dipaparkan, keluar mesej "Tahniah jawapan anda tepat" dan butang "Seterusnya" menjadi aktif. Jika salah, murid mesti cuba lagi. Butang "Seterusnya" TIDAK boleh diaktifkan tanpa sebutan yang dikesan betul oleh mikrofon.

## 2. Senarai situasi dan ayat (5 situasi, mudah ke sukar)
1. Pipi berada di dalam makmal di belakang bukit. (Pipi melambai dari tingkap makmal yang bercahaya, lampu amaran berkelip.)
2. Papa Zola berlari ke arah bukit untuk menyelamatkan Pipi. (Papa Zola berlari, debu berterbangan.)
3. Papa Zola mendaki bukit yang curam. (Papa Zola memanjat cerun, batu kecil jatuh.)
4. Papa Zola memanggil Pipi dengan suara yang kuat. (Gelembung ucapan "Pipi!" membesar di hadapan makmal.)
5. Pipi gembira kerana Papa Zola datang menolongnya. (Pipi melompat gembira, bintang dan hati berkilauan.)

Simpan situasi dalam satu tatasusunan (array) data supaya guru mudah menambah atau menukar ayat.

## 3. Grafik (animasi 3D)
- Gunakan SVG berlapis dengan CSS 3D (perspective dan translateZ). Susun lapisan: langit, bukit jauh, makmal, bukit hadapan, hiasan latar depan, watak.
- Paparan condong mengikut pergerakan tetikus atau jari (kesan parallax).
- Animasi: awan bergerak, lampu berkelip, Pipi melambai, Papa Zola berlari dengan kaki berayun dan debu, pokok bergoyang.
- Buat watak dengan gaya sendiri (jangan tiru lukisan rasmi). Hormati prefers-reduced-motion.
- Setiap situasi ada kapsyen kecil (contoh "Situasi 3").

## 4. Pengecaman suara
- Guna Web Speech API (SpeechRecognition / webkitSpeechRecognition) dengan lang = "ms-MY", maxAlternatives = 5, interimResults = false.
- Butang mikrofon besar: berdenyut dan paparkan bar gelombang suara semasa merakam.
- Normalisasi teks (huruf kecil, buang tanda baca), kemudian bandingkan dengan ayat sasaran menggunakan jarak Levenshtein. Lulus jika kesamaan >= 0.82. Semak semua alternatif dan ambil yang terbaik.
- Selepas jawapan: tunjuk perkataan hijau (disebut betul) dan merah (tidak dikesan) pada ayat yang dipaparkan. Papar "Saya dengar: ..." supaya murid tahu apa yang dikesan.
- JANGAN letak butang "dengar ayat" (text-to-speech) kerana tidak menepati sebutan Bahasa Melayu. JANGAN letak kotak menaip sebagai pilihan lain. Hanya mikrofon.
- Kendalikan ralat dengan mesej jelas: mikrofon disekat, tiada suara dikesan, pelayar tidak menyokong (cadangkan Chrome atau Edge).

## 5. Bunyi dan kesan bunyi (WebAudio API, dijana melalui kod, tiada fail audio luar)

### 5.1 Bunyi antara muka
- Klik butang: bunyi "pop" pendek.
- Hover/fokus pada butang: bunyi "tik" sangat halus (volum rendah).
- Mula merakam: dua nada naik (seperti "ding-dong" ceria).
- Tamat merakam: satu nada pendek.
- Butang "Seterusnya" menjadi aktif: bunyi "klik terbuka" kecil.
- Butang yang masih terkunci ditekan: bunyi "bonk" lembut.
- Peralihan antara situasi: bunyi "whoosh" bergerak dari kiri ke kanan (guna StereoPanner).

### 5.2 Bunyi jawapan
- Jawapan betul: melodi naik 4 nota (do-mi-sol-do tinggi) disertai bunyi kilauan bintang, dan bunyi "pop" untuk setiap bintang yang muncul.
- Jawapan salah: dua nada turun yang lembut dan tidak menakutkan. Jangan gunakan bunyi buzzer yang kuat.
- Skrin tamat: fanfare kemenangan yang lebih panjang disertai bunyi confetti meletup.

### 5.3 Bunyi latar dan kesan bunyi mengikut situasi
Setiap situasi ada bunyi latar ringan (volum rendah, berulang) dan kesan bunyi pendek yang dimainkan sekali apabila situasi dibuka:
1. Makmal: dengung mesin lembut, bunyi "bip-bip" lampu amaran berkelip, dan bunyi gelembung kimia.
2. Papa Zola berlari: bunyi tapak kaki berirama, desiran angin, dan debu ("fsss").
3. Mendaki bukit: bunyi tapak kaki pada tanah dan batu kecil bergolek, serta nafas termengah-mengah yang kartun.
4. Memanggil Pipi: bunyi gema suara ("boing" dengan gema), diikuti bunyi burung terkejut terbang.
5. Gembira: bunyi lompatan "boing", kilauan bintang, dan bunyi hati ("ting").

Bunyi latar mesti berhenti atau pudar (fade out) dalam 0.3 saat apabila murid sedang merakam, supaya mikrofon tidak menangkap bunyi permainan.

### 5.4 Kawalan bunyi
- Butang senyap/bunyi yang jelas di bahagian atas.
- Peluncur volum (volume slider) untuk guru melaras.
- Bunyi tidak boleh bermula sendiri sebelum murid menyentuh skrin (patuhi had autoplay pelayar). Sediakan skrin permulaan dengan butang "Mula Bermain" dan mulakan AudioContext pada klik itu.
- Semua bunyi mesti mesra kanak-kanak: tiada bunyi yang terlalu kuat atau tajam. Hadkan volum maksimum.

## 6. Navigasi dan sistem permainan
- Butang "Kembali", "Seterusnya", dan penunjuk nombor situasi (1 hingga 5) di atas.
- Murid hanya boleh ke situasi seterusnya selepas lulus situasi semasa. Penunjuk nombor yang belum dibuka tidak boleh diklik.
- Papar bilangan percubaan setiap situasi dan skor bintang: lulus percubaan pertama = 3 bintang, kedua = 2, selebihnya = 1. Jumlah penuh ialah 15 bintang.
- Selepas situasi 5, papar skrin tamat: jumlah bintang, mesej pujian, dan butang "Main semula". Confetti untuk jawapan betul dan tamat.

## 7. Reka bentuk
- Sesuai untuk murid sekolah rendah: butang besar, tulisan jelas dan berwarna, jarak selesa. Fon Baloo 2 (Google Fonts) dengan fon sandaran.
- Responsif (telefon, tablet, komputer). Sokong mod gelap/cerah. Fokus papan kekunci kelihatan.
- Susun atur: bahagian atas tajuk dan navigasi, tengah pentas grafik 16:9, bawah panel ayat dan mikrofon.

## 8. Nota teknikal
- Satu fail HTML tunggal. Jangan guna localStorage untuk perkara penting.
- Mikrofon mungkin tidak berfungsi apabila halaman dipaparkan di dalam iframe. Letakkan nota ringkas dalam kod bahawa fail perlu dibuka terus dalam Chrome atau Edge dan kebenaran mikrofon dibenarkan.
- Kod mesti bersih, berkomen dalam Bahasa Melayu, dan mudah diubah oleh guru.
