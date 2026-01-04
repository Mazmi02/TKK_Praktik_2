<html lang="id">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Kuis Pengetahuan Umum Dasar</title>
    <style>
        body {
            font-family: Arial, sans-serif;
            display: flex;
            justify-content: center;
            align-items: center;
            min-height: 100vh;
            background-color: #f0f0f0;
            margin: 0;
            padding: 20px;
            box-sizing: border-box;
        }

        .quiz-container {
            background-color: white;
            padding: 30px;
            border-radius: 10px;
            box-shadow: 0 0 15px rgba(0, 0, 0, 0.2);
            width: 100%;
            max-width: 600px;
            text-align: center;
        }

        h1 {
            color: #333;
            margin-bottom: 10px;
        }

        #completion-message {
            color: #28a745;
            font-size: 1.2em;
            font-weight: bold;
            margin-top: 5px;
            margin-bottom: 20px;
        }

        .question-counter-text {
            font-size: 0.9em;
            color: #666;
            margin-bottom: 20px;
        }

        #question-container {
            margin-bottom: 20px;
        }

        #question {
            font-size: 1.5em;
            font-weight: bold;
            margin-bottom: 25px;
            color: #444;
        }

        .btn-grid {
            display: grid;
            grid-template-columns: repeat(2, 1fr);
            gap: 10px;
            margin-bottom: 20px;
        }

        .btn {
            background-color: #007bff;
            color: white;
            border: none;
            padding: 12px 15px;
            border-radius: 5px;
            cursor: pointer;
            font-size: 1em;
            transition: background-color 0.2s ease, box-shadow 0.2s ease;
            word-wrap: break-word;
            min-height: 50px;
            display: flex;
            align-items: center;
            justify-content: center;
            outline: none;
            font-weight: bold;
        }

        .btn:not(.correct):not(.wrong):not(.skip-btn):not(.btn-prev-q) { background-color: #007bff; }
        .btn:not(.correct):not(.wrong):not(.skip-btn):not(.btn-prev-q):focus {
            background-color: #007bff;
            box-shadow: 0 0 0 3px rgba(0, 123, 255, 0.5);
        }
        .btn:not([disabled]):not(.correct):not(.wrong):not(.skip-btn):not(.btn-prev-q):hover {}
        .btn:not([disabled]):not(.correct):not(.wrong):not(.skip-btn):not(.btn-prev-q):focus:hover {
            background-color: #007bff;
            box-shadow: 0 0 0 3px rgba(0, 123, 255, 0.5);
        }

        .btn.correct { background-color: #28a745 !important; box-shadow: none; }
        .btn.correct:hover { background-color: #218838 !important; }
        .btn.correct:focus {
            background-color: #28a745 !important;
            box-shadow: 0 0 0 3px rgba(40, 167, 69, 0.6) !important;
        }

        .btn.wrong { background-color: #dc3545 !important; box-shadow: none; }
        .btn.wrong:hover { background-color: #c82333 !important; }
        .btn.wrong:focus {
            background-color: #dc3545 !important;
            box-shadow: 0 0 0 3px rgba(220, 53, 69, 0.6) !important;
        }

        .btn:disabled {
            cursor: not-allowed;
            opacity: 0.65;
        }
        /* Adjusted to not conflict with new button's disabled state if it's not a skip-btn or answer btn */
        .btn:disabled:not(.correct):not(.wrong):not(.skip-btn):not(.btn-prev-q) {
            background-color: #6c757d !important;
            color: #ccc !important;
        }


        .controls {
            display: flex;
            justify-content: center;
            gap: 10px;
        }

        #skip-navigation-controls {
            justify-content: space-between; /* Adjusted to space-around or similar if needed for 3 buttons */
            margin-top: 40px;
            margin-bottom: 10px;
        }

        .skip-btn { /* This style is for prev-50 and next-50 */
            background-color: #28a745; /* Green */
            color: white;
            padding: 8px 12px;
            font-size: 0.9em;
            min-width: 80px; /* Ensures same width for all skip-type buttons */
        }
        .skip-btn:hover {
            background-color: #218838; /* Darker Green */
            color: white;
        }
        .skip-btn:disabled { /* Default disabled for green skip buttons */
            background-color: #a3d8b0 !important;
            color: #e9f5ec !important;
            /* cursor: not-allowed; is inherited from .btn:disabled */
            /* opacity: 0.65; is inherited from .btn:disabled */
        }

        /* New button style for "Previous Question" */
        .btn-prev-q {
            background-color: #5F9EA0; /* CadetBlue - "biru terang" */
            color: white; /* Text color */
            padding: 8px 12px; /* Same padding as skip-btn */
            font-size: 0.9em; /* Same font size as skip-btn */
            min-width: 80px; /* Same min-width as skip-btn */
        }
        .btn-prev-q:hover:not([disabled]) {
            background-color: #4682B4; /* SteelBlue - darker for hover */
            color: white;
        }
        .btn-prev-q:disabled {
            background-color: #B0C4DE !important; /* LightSteelBlue - for disabled state */
            color: #666666 !important; /* Darker text for readability on light blue */
            /* opacity will be applied by .btn:disabled */
        }


        .hide { display: none !important; }
    </style>
</head>
<body>
    <div class="quiz-container">
        <h1>Pengetahuan Umum Dasar</h1>
        <p id="completion-message" class="hide">Selamat Kuis Sudah Selesai 🎉</p>
        <div id="initial-controls" class="controls">
            <button id="start-btn" class="btn">Mulai</button>
            <button id="continue-btn" class="btn hide">Lanjutkan</button>
        </div>
        <div id="question-counter" class="question-counter-text hide">0/0</div>
        <div id="question-container" class="hide">
            <div id="question">Kata Bahasa Inggris</div>
            <div id="answer-buttons" class="btn-grid">
            </div>
            <div id="skip-navigation-controls" class="controls hide">
                <button id="prev-50-btn" class="btn skip-btn">&laquo; 50</button>
                <button id="prev-question-btn" class="btn btn-prev-q">&lt;</button> <button id="next-50-btn" class="btn skip-btn">50 &raquo;</button>
            </div>
        </div>
    </div>

    <script>
        const startButton = document.getElementById('start-btn');
        const continueButton = document.getElementById('continue-btn');
        const initialControls = document.getElementById('initial-controls');
        const completionMessageElement = document.getElementById('completion-message');
        const questionContainerElement = document.getElementById('question-container');
        const questionElement = document.getElementById('question');
        const answerButtonsElement = document.getElementById('answer-buttons');
        const questionCounterElement = document.getElementById('question-counter');

        const skipNavigationControls = document.getElementById('skip-navigation-controls');
        const prev50Button = document.getElementById('prev-50-btn');
        const prevQuestionButton = document.getElementById('prev-question-btn'); // Referensi untuk tombol baru
        const next50Button = document.getElementById('next-50-btn');
        const JUMP_AMOUNT = 50;

        let orderedQuestions, currentQuestionIndex;
        let score = 0;
        let questionTimeout;

        // Daftar kata mentah dari PDF (Inggris: Indonesia) - Total 1580 kata
        const rawVocabularyList = [


  { "en": "Apa Fungsi Perintah Wipeout Pada AutoCAD (Computer Aided Design)?", "id": "Menutupi Objek Dengan Area Kosong." },
  { "en": "Apa Fungsi Perintah Revcloud Pada AutoCAD?", "id": "Membuat Gambar Awan Revisi." },
  { "en": "Shortcut Apa Untuk Membuka Design Center AutoCAD?", "id": "Tekan Ctrl + 2." },
  { "en": "Apa Fungsi Perintah Spell Pada AutoCAD?", "id": "Memeriksa Ejaan Teks Dalam Gambar." },
  { "en": "Apa Fungsi Perintah Find Pada AutoCAD?", "id": "Mencari Dan Mengganti Teks." },
  { "en": "Apa Fungsi Global Label Di Proteus?", "id": "Menghubungkan Jalur Antar Halaman Skematik." },
  { "en": "Apa Fungsi Instruksi SSET (Set Stack) PLC?", "id": "Menyimpan Data Ke Stack Memori." },
  { "en": "Apa Fungsi Instruksi RSET (Reset Stack) PLC?", "id": "Mengambil Data Dari Stack Memori." },
  { "en": "Komponen LM317T Di Proteus Berfungsi Sebagai?", "id": "Regulator Tegangan Positif Adjustable." },
  { "en": "Apa Fungsi Perintah Group Pada AutoCAD?", "id": "Mengelompokkan Objek Menjadi Satu Kesatuan." },
  { "en": "Apa Fungsi Fungsi Sizeof Pada Arduino?", "id": "Menghitung Ukuran Byte Variabel." },
  { "en": "Bagaimana Cara Mengatur Ukuran Pickbox AutoCAD?", "id": "Ketik Pickbox Lalu Masukkan Nilai." },
  { "en": "Apa Fungsi Tombol Ctrl Shift U AutoCAD?", "id": "Mengaktifkan Polar Tracking." },
  { "en": "Apa Fungsi Perintah Ungroup Pada AutoCAD?", "id": "Memisahkan Kelompok Grup Objek." },
  { "en": "Komponen IC 74154 Demultiplexer 4 To 16?", "id": "Dekoder Empat Input Enam Belas Output." },
  { "en": "Apa Fungsi Instruksi ASIN (Arc Sine) PLC?", "id": "Menghitung Sudut Dari Nilai Sinus." },
  { "en": "Apa Fungsi Instruksi ACOS (Arc Cosine) PLC?", "id": "Menghitung Sudut Dari Nilai Cosinus." },
  { "en": "Bagaimana Cara Mengatur Ukuran Grip AutoCAD?", "id": "Ketik Gripsize Lalu Masukkan Nilai." },
  { "en": "Apa Fungsi Interrupt Mode Low Level?", "id": "Picu Terus Selama Sinyal Rendah." },
  { "en": "Apa Itu Data Memory Latch (L) PLC?", "id": "Bit Tahan Saat Listrik Mati." },
  { "en": "Apa Itu Step Ladder (S) Pada PLC?", "id": "Relay Untuk Program Step Ladder." },
  { "en": "Shortcut Apa Untuk Insert Comment Arduino?", "id": "Tekan Ctrl + Slash." },
  { "en": "Apa Fungsi Library Wire Available Arduino?", "id": "Cek Jumlah Byte Di Buffer I2C." },
  { "en": "Bagaimana Cara Mengubah Warna Background Proteus?", "id": "Menu Template Set Design Colours." },
  { "en": "Apa Fungsi Instruksi BAND (Byte AND) PLC?", "id": "Operasi Logika AND 8 Bit." },
  { "en": "Apa Fungsi Instruksi BOR (Byte OR) PLC?", "id": "Operasi Logika OR 8 Bit." },
  { "en": "Apa Fungsi Perintah Dimjogline Pada AutoCAD?", "id": "Menambah Garis Jog Dimensi Linear." },
  { "en": "Apa Itu Start Bit Komunikasi Serial?", "id": "Bit Awal Tanda Pengiriman Data." },
  { "en": "Apa Itu Stop Bit Komunikasi Serial?", "id": "Bit Akhir Tanda Selesai Data." },
  { "en": "Shortcut Apa Untuk Find Replace GX Works?", "id": "Tekan Ctrl + H." },
  { "en": "Apa Fungsi LiquidCrystal ScrollDisplayLeft?", "id": "Geser Teks LCD Ke Kiri." },
  { "en": "Apa Fungsi Noise Graph Di Proteus?", "id": "Analisis Gangguan Sinyal Rangkaian." },
  { "en": "Apa Fungsi Timer Pulse (TP) PLC?", "id": "Output Pulsa Durasi Tetap." },
  { "en": "Bagaimana Cara Insert Block AutoCAD?", "id": "Ketik Insert Lalu Enter." },
  { "en": "Apa Fungsi Operator Bitwise XOR (Topi)?", "id": "Logika Exclusive OR Per Bit." },
  { "en": "Apa Itu Terminal Ground Di Proteus?", "id": "Titik Referensi Nol Volt." },
  { "en": "Shortcut Apa Untuk Layer Properties AutoCAD?", "id": "Ketik Layer Lalu Enter." },
  { "en": "Apa Fungsi Serial Write Buf Len?", "id": "Kirim Data Buffer Panjang Tertentu." },
  { "en": "Komponen Apa Penentu Tegangan Zener?", "id": "Dioda Zener." },
  { "en": "Apa Fungsi Instruksi BXOR (Byte XOR) PLC?", "id": "Operasi Logika XOR 8 Bit." },
  { "en": "Bagaimana Cara Xref Bind AutoCAD?", "id": "Gabungkan Referensi Jadi Blok." },
  { "en": "Apa Itu EEPROM Read Arduino?", "id": "Baca Satu Byte Dari Memori." },
  { "en": "Apa Fungsi Digital Analysis Graph Proteus?", "id": "Analisis Timing Sinyal Digital." },
  { "en": "Apa Shortcut Compile PLC GX Works?", "id": "Tekan Tombol F4." },
  { "en": "Apa Fungsi Perintah Sketch AutoCAD?", "id": "Gambar Garis Bebas Tangan." },
  { "en": "Apa Fungsi Operator Modulo (Persen)?", "id": "Sisa Bagi Dua Bilangan." },
  { "en": "Apa Itu Logic Toggle Proteus?", "id": "Saklar Logika 0 Atau 1." },
  { "en": "Bagaimana Cara Export PDF AutoCAD?", "id": "Gunakan Perintah Exportpdf." },
  { "en": "Apa Fungsi Perintah Solidedit Face Taper?", "id": "Miringkan Permukaan Solid 3D." },
  { "en": "Apa Tipe Data Unsigned Int Arduino?", "id": "Bilangan Bulat Positif 16 Bit." },
  { "en": "Komponen Apa Pemicu Transistor NPN?", "id": "Arus Positif Ke Basis." },
  { "en": "Apa Fungsi Instruksi CMP (Compare) PLC?", "id": "Bandingkan Dua Nilai Data." },
  { "en": "Shortcut Apa Untuk Calculator AutoCAD?", "id": "Tekan Ctrl + 8." },
  { "en": "Apa Fungsi Keyword Break Arduino?", "id": "Keluar Dari Loop Atau Switch." },
  { "en": "Apa Itu Resolusi ADC 12 Bit?", "id": "Nilai Nol Sampai 4095." },
  { "en": "Apa Fungsi Instruksi CLC (Clear Carry)?", "id": "Matikan Bit Carry Flag." },
  { "en": "Bagaimana Cara Paste Original Coords AutoCAD?", "id": "Pilih Paste To Original Coordinates." },
  { "en": "Apa Fungsi Keyword Volatile Arduino?", "id": "Variabel Akses Di Interupsi." },
  { "en": "Apa Itu Sensor LDR Proteus?", "id": "Resistor Peka Cahaya." },
  { "en": "Apa Shortcut Upload PLC Omron?", "id": "Tekan Ctrl + T." },
  { "en": "Apa Fungsi Perintah Chamfer Distance?", "id": "Atur Jarak Potong Sudut." },
  { "en": "Apa Fungsi LiquidCrystal Clear?", "id": "Hapus Semua Teks LCD." },
  { "en": "Komponen Apa Penyearah Gelombang Penuh?", "id": "Dioda Bridge." },
  { "en": "Apa Fungsi Carry Flag (CY) PLC?", "id": "Sisa Hasil Operasi Aritmatika." },
  { "en": "Shortcut Apa Untuk Auto Format Arduino?", "id": "Tekan Ctrl + T." },
  { "en": "Apa Fungsi Serial Println HEX?", "id": "Cetak Nilai Format Hexadesimal." },
  { "en": "Apa Itu Clock Source Proteus?", "id": "Pembangkit Sinyal Detak." },
  { "en": "Apa Fungsi Instruksi XNR (Exclusive NOR)?", "id": "Logika Jika Input Sama." },
  { "en": "Bagaimana Cara Setting Sun AutoCAD?", "id": "Ketik Sunproperties Lalu Enter." },
  { "en": "Apa Fungsi IsAlphaNumeric Arduino?", "id": "Cek Karakter Huruf Atau Angka." },
  { "en": "Apa Itu I2C Debugger Proteus?", "id": "Analisis Data Protokol I2C." },
  { "en": "Shortcut Apa Untuk Zoom All AutoCAD?", "id": "Ketik Z Lalu A." },
  { "en": "Apa Fungsi Perintah Thicken AutoCAD?", "id": "Ubah Surface Jadi Solid." },
  { "en": "Apa Fungsi String ToInt Arduino?", "id": "Ubah Teks Angka Ke Integer." },
  { "en": "Apa Itu IC 74138 Decoder?", "id": "Dekoder 3 Ke 8 Line." },
  { "en": "Apa Fungsi Instruksi DI (Disable Interrupt)?", "id": "Matikan Fungsi Interupsi PLC." },
  { "en": "Bagaimana Cara Ukur Luas Region?", "id": "Gunakan Perintah Massprop." },
  { "en": "Apa Fungsi Pin AREF Arduino?", "id": "Referensi Tegangan Analog Luar." },
  { "en": "Apa Itu Power VCC Proteus?", "id": "Sumber Tegangan 5 Volt." },
  { "en": "Shortcut Apa Untuk Text Window AutoCAD?", "id": "Tekan Tombol F2." },
  { "en": "Apa Fungsi Perintah Helix Turns?", "id": "Atur Jumlah Putaran Spiral." },
  { "en": "Apa Fungsi Karakter Backslash T?", "id": "Tabulasi Horizontal Teks." },
  { "en": "Apa Itu Regulator 7805?", "id": "Penstabil Tegangan 5 Volt Positif." },
  { "en": "Apa Fungsi Analog Reference Internal?", "id": "Referensi Tegangan 1.1 Volt." },
  { "en": "Bagaimana Cara Atur Tampilan Point?", "id": "Ketik Ptype Lalu Enter." },
  { "en": "Apa Fungsi Pin MISO Arduino?", "id": "Data Masuk Master SPI." },
  { "en": "Apa Itu Via Di PCB ARES?", "id": "Lubang Tembus Antar Layer." },
  { "en": "Apa Shortcut Verify Arduino?", "id": "Tekan Ctrl + R." },
  { "en": "Apa Fungsi Perintah Rotate Copy?", "id": "Putar Objek Sambil Salin." },
  { "en": "Apa Fungsi Operator Logika AND (Ampersand)?", "id": "Logika DAN Per Bit." },
  { "en": "Apa Fungsi Pin SDA Arduino?", "id": "Data Serial I2C." },
  { "en": "Apa Itu Resistor SIL Proteus?", "id": "Resistor Array Satu Baris." },
  { "en": "Bagaimana Cara Update Data Link?", "id": "Klik Kanan Update Data Link." },
  { "en": "Apa Fungsi Perintah Dimjogged Angle?", "id": "Dimensi Sudut Pusat Jauh." },
  { "en": "Apa Fungsi Wire Write Byte?", "id": "Kirim Satu Byte Data." },
  { "en": "Apa Fungsi Gerber Viewer Proteus?", "id": "Lihat File Produksi PCB." },
  { "en": "Apa Fungsi First Scan Flag?", "id": "Aktif Saat Start Pertama." },
  { "en": "Bagaimana Cara Embed OLE Object?", "id": "Pilih Paste Special." },
  { "en": "Apa Fungsi Perintah Subtract Solid?", "id": "Potong Volume Solid." },
  { "en": "Apa Fungsi Perintah Layfrz Pada AutoCAD (Computer Aided Design)?", "id": "Membekukan Layer Objek Yang Dipilih." },
  { "en": "Apa Fungsi Perintah Layoff Pada AutoCAD?", "id": "Mematikan Layer Objek Yang Dipilih." },
  { "en": "Shortcut Apa Untuk Membuka Visual Basic Editor AutoCAD?", "id": "Tekan Alt + F11." },
  { "en": "Apa Fungsi Perintah Dimbreak Pada Dimensi AutoCAD?", "id": "Memutus Garis Dimensi Yang Bertabrakan." },
  { "en": "Apa Fungsi Perintah Dimreassociate Pada AutoCAD?", "id": "Menghubungkan Ulang Dimensi Ke Objek." },
  { "en": "Apa Fungsi Ratnest Mode Pada Desain PCB Proteus?", "id": "Menampilkan Garis Koneksi Udara." },
  { "en": "Apa Fungsi Instruksi PPLS (Pulse Output) PLC?", "id": "Mengeluarkan Pulsa Sinyal Terkontrol." },
  { "en": "Apa Fungsi Instruksi ACC (Acceleration Control) PLC?", "id": "Mengontrol Percepatan Motor Stepper." },
  { "en": "Komponen NE555 Di Proteus Berfungsi Sebagai?", "id": "Timer Dan Pembangkit Pulsa." },
  { "en": "Apa Fungsi Perintah Xopen Pada AutoCAD?", "id": "Membuka File Referensi Di Jendela Baru." },
  { "en": "Apa Fungsi Fungsi Strcmp Pada C++ Arduino?", "id": "Membandingkan Dua String Karakter." },
  { "en": "Bagaimana Cara Mengatur Ukuran Crosshair AutoCAD?", "id": "Ketik Cursorsize Lalu Masukkan Nilai." },
  { "en": "Apa Fungsi Tombol Ctrl Shift L AutoCAD?", "id": "Memilih Objek Sebelumnya (Select Previous)." },
  { "en": "Apa Fungsi Perintah Txtexp Pada AutoCAD Express?", "id": "Meledakkan Teks Menjadi Garis Poligon." },
  { "en": "Komponen IC 74157 Quad 2 Input Multiplexer?", "id": "Pemilih Data Dua Ke Satu." },
  { "en": "Apa Fungsi Instruksi ASLL (Double Shift Left) PLC?", "id": "Geser Kiri Data 32 Bit." },
  { "en": "Apa Fungsi Instruksi ASRL (Double Shift Right) PLC?", "id": "Geser Kanan Data 32 Bit." },
  { "en": "Bagaimana Cara Mengatur Raster Image Frame AutoCAD?", "id": "Ubah Variabel Imageframe." },
  { "en": "Apa Fungsi Interrupt Mode Change?", "id": "Picu Interupsi Saat Nilai Berubah." },
  { "en": "Apa Itu Data Memory Special I/O (D) PLC?", "id": "Memori Data Unit Input Output Khusus." },
  { "en": "Apa Itu Counter Flag (C) Pada PLC?", "id": "Status Selesai Hitungan Counter." },
  { "en": "Shortcut Apa Untuk Membuka Library Manager Arduino?", "id": "Tekan Ctrl + Shift + I." },
  { "en": "Apa Fungsi Library SPI SetBitOrder?", "id": "Mengatur Urutan Bit MSB LSB." },
  { "en": "Bagaimana Cara Mengubah Grid Snap Proteus?", "id": "Tekan F4 F3 Atau F2." },
  { "en": "Apa Fungsi Instruksi NASR (NAND Shift Register)?", "id": "Geser Register Logika NAND." },
  { "en": "Apa Fungsi Instruksi WSFT (Word Shift) PLC?", "id": "Geser Data Antar Word." },
  { "en": "Apa Fungsi Perintah Dimedit Home AutoCAD?", "id": "Mengembalikan Teks Dimensi Ke Posisi Awal." },
  { "en": "Apa Itu Echo Character Pada Serial?", "id": "Mengirim Kembali Karakter Yang Diterima." },
  { "en": "Apa Itu Buffer Underrun Pada Arduino?", "id": "Membaca Data Saat Buffer Kosong." },
  { "en": "Shortcut Apa Untuk Zoom In GX Works?", "id": "Tekan Ctrl + Plus." },
  { "en": "Apa Fungsi LiquidCrystal CreateChar?", "id": "Membuat Pola Karakter Kustom." },
  { "en": "Apa Fungsi Fourier Analysis Graph Proteus?", "id": "Analisis Komponen Frekuensi Sinyal." },
  { "en": "Apa Fungsi Timer Delayed Off (TOF) PLC?", "id": "Menunda Matinya Output." },
  { "en": "Bagaimana Cara Membuat Block Attribute Definition?", "id": "Gunakan Perintah Attdef." },
  { "en": "Apa Fungsi Operator Bitwise NOT (Tanda Tilde)?", "id": "Membalik Nilai Bit 0 Jadi 1." },
  { "en": "Apa Itu Pin Label Di Proteus?", "id": "Menamai Kaki Komponen." },
  { "en": "Shortcut Apa Untuk Membuka Sheet Set Manager?", "id": "Tekan Ctrl + 4." },
  { "en": "Apa Fungsi Serial FindUntil Pada Arduino?", "id": "Cari Data Hingga Karakter Tertentu." },
  { "en": "Komponen Apa Yang Menghilangkan Ripple DC?", "id": "Kapasitor Filter Perata." },
  { "en": "Apa Fungsi Instruksi MLPX (Data Decoder) PLC?", "id": "Mendekode Data 4 Bit Ke 16 Bit." },
  { "en": "Bagaimana Cara Xref Unload AutoCAD?", "id": "Menonaktifkan Referensi Tanpa Menghapus." },
  { "en": "Apa Itu EEPROM Write Int Arduino?", "id": "Menulis Nilai Integer Ke Memori." },
  { "en": "Apa Fungsi DC Transfer Curve Proteus?", "id": "Grafik Hubungan Tegangan Input Output." },
  { "en": "Apa Shortcut Find String GX Works?", "id": "Tekan Ctrl + F." },
  { "en": "Apa Fungsi Perintah Ray Pada AutoCAD?", "id": "Garis Sinar Satu Arah Tak Terbatas." },
  { "en": "Apa Fungsi Operator Modulus (Tanda Persen)?", "id": "Mencari Sisa Hasil Pembagian." },
  { "en": "Apa Itu Voltage Source Sine Proteus?", "id": "Sumber Tegangan Gelombang Sinus." },
  { "en": "Bagaimana Cara Export Layout Ke DWF AutoCAD?", "id": "Gunakan Perintah Exportdwf." },
  { "en": "Apa Fungsi Perintah Solidedit Body Check?", "id": "Memvalidasi Geometri Objek Solid." },
  { "en": "Apa Tipe Data Unsigned Short Pada Arduino?", "id": "Bilangan Bulat 16 Bit Positif." },
  { "en": "Komponen Apa Pemicu MOSFET Tipe P?", "id": "Tegangan Negatif Pada Gate." },
  { "en": "Apa Fungsi Instruksi TCMP (Table Compare) PLC?", "id": "Bandingkan Nilai Dengan Tabel Data." },
  { "en": "Shortcut Apa Untuk Membuka Quick Calc AutoCAD?", "id": "Tekan Ctrl + 8." },
  { "en": "Apa Fungsi Keyword Continue Pada Loop Arduino?", "id": "Lanjut Ke Iterasi Berikutnya." },
  { "en": "Apa Itu Resolusi ADC 8 Bit Arduino?", "id": "Nilai Nol Sampai 255." },
  { "en": "Apa Fungsi Instruksi CLC (Clear Carry Flag)?", "id": "Mereset Bit Carry Flag." },
  { "en": "Bagaimana Cara Paste Sebagai Blok AutoCAD?", "id": "Tekan Ctrl + Shift + V." },
  { "en": "Apa Fungsi Keyword Static Pada Fungsi Arduino?", "id": "Variabel Lokal Tetap Tersimpan." },
  { "en": "Apa Itu Sensor Gas Smoke Proteus?", "id": "Sensor Deteksi Asap." },
  { "en": "Apa Shortcut Download Program Ke PLC?", "id": "Tekan Ctrl + T." },
  { "en": "Apa Fungsi Perintah Fillet Radius 0?", "id": "Menyambung Garis Menjadi Sudut Lancip." },
  { "en": "Apa Fungsi LiquidCrystal Cursor Pada Arduino?", "id": "Menampilkan Garis Bawah Kursor." },
  { "en": "Komponen Apa Yang Membatasi Arus LED?", "id": "Resistor Seri." },
  { "en": "Apa Fungsi Overflow Flag (V) Pada PLC?", "id": "Indikator Kelebihan Kapasitas Data." },
  { "en": "Shortcut Apa Untuk Auto Indent Arduino?", "id": "Tekan Ctrl + T." },
  { "en": "Apa Fungsi Serial Println BIN?", "id": "Cetak Nilai Format Biner." },
  { "en": "Apa Itu Signal Generator FM Proteus?", "id": "Pembangkit Sinyal Modulasi Frekuensi." },
  { "en": "Apa Fungsi Instruksi NAND (Not AND) PLC?", "id": "Logika Output 0 Jika Semua 1." },
  { "en": "Bagaimana Cara Setting Shadow AutoCAD?", "id": "Gunakan Perintah Shadowdisplay." },
  { "en": "Apa Fungsi IsPrint Pada Karakter Arduino?", "id": "Cek Karakter Bisa Dicetak." },
  { "en": "Apa Itu SPI Debugger Proteus?", "id": "Analisis Data Protokol SPI." },
  { "en": "Shortcut Apa Untuk Zoom Previous AutoCAD?", "id": "Ketik Z Lalu P." },
  { "en": "Apa Fungsi Perintah Interfere Pada Solid?", "id": "Cek Tabrakan Antar Objek 3D." },
  { "en": "Apa Fungsi String ToFloat Arduino?", "id": "Ubah Teks Angka Ke Desimal." },
  { "en": "Apa Itu IC 74148 Priority Encoder?", "id": "Encoder Prioritas 8 Ke 3 Line." },
  { "en": "Apa Fungsi Instruksi DI (Disable Interrupts)?", "id": "Menonaktifkan Semua Interupsi." },
  { "en": "Bagaimana Cara Ukur Volume Solid?", "id": "Gunakan Perintah Massprop." },
  { "en": "Apa Fungsi Pin VCC Arduino Nano?", "id": "Input Tegangan 5 Volt." },
  { "en": "Apa Itu Digital Power VDD Proteus?", "id": "Sumber Tegangan Positif Digital." },
  { "en": "Shortcut Apa Untuk Command Line AutoCAD?", "id": "Tekan Ctrl + 9." },
  { "en": "Apa Fungsi Perintah Helix Base Radius?", "id": "Atur Jari Jari Bawah Spiral." },
  { "en": "Apa Fungsi Karakter Backslash R?", "id": "Carriage Return Kembali Ke Awal." },
  { "en": "Apa Itu Regulator 7905?", "id": "Penstabil Tegangan 5 Volt Negatif." },
  { "en": "Apa Fungsi Analog Reference Default?", "id": "Referensi Tegangan 5 Volt." },
  { "en": "Bagaimana Cara Atur Ukuran Titik?", "id": "Ketik Pdsize Lalu Masukkan Nilai." },
  { "en": "Apa Fungsi Pin SCK Arduino?", "id": "Clock Sinkronisasi SPI." },
  { "en": "Apa Itu Pad Mode Proteus?", "id": "Menempatkan Kaki Komponen SMT THT." },
  { "en": "Apa Shortcut New Sketch Arduino?", "id": "Tekan Ctrl + N." },
  { "en": "Apa Fungsi Perintah Array Path AutoCAD?", "id": "Gandakan Objek Mengikuti Garis Jalur." },
  { "en": "Apa Fungsi Operator Logika OR (Pipa Ganda)?", "id": "Logika ATAU Boolean." },
  { "en": "Apa Fungsi Pin SCL Arduino?", "id": "Clock Sinkronisasi I2C." },
  { "en": "Apa Itu Resistor DIL Proteus?", "id": "Resistor Array Dual Inline." },
  { "en": "Bagaimana Cara Detach Data Link?", "id": "Klik Kanan Remove Data Link." },
  { "en": "Apa Fungsi Perintah Dimjogged Radius?", "id": "Dimensi Radius Pusat Jauh." },
  { "en": "Apa Fungsi Wire Read Arduino?", "id": "Baca Satu Byte Data I2C." },
  { "en": "Apa Fungsi 3D Visualization Proteus?", "id": "Melihat Bentuk Fisik PCB." },
  { "en": "Apa Fungsi Error Flag (ER) PLC?", "id": "Indikator Kesalahan Eksekusi Program." },
  { "en": "Bagaimana Cara Insert OLE Object?", "id": "Ketik Insertobj Lalu Enter." },
  { "en": "Apa Fungsi Perintah Union Solid?", "id": "Gabungkan Objek Solid Jadi Satu." },
  { "en": "Apa Fungsi Variabel Sistem Filedia Pada AutoCAD?", "id": "Menampilkan Kotak Dialog File." },
  { "en": "Apa Fungsi Variabel Sistem Pickfirst Pada AutoCAD?", "id": "Seleksi Objek Sebelum Perintah." },
  { "en": "Shortcut Apa Untuk Toggle Viewport AutoCAD?", "id": "Tekan Ctrl + R." },
  { "en": "Apa Fungsi Variabel Sistem Mbuttonpan Pada AutoCAD?", "id": "Mengaktifkan Pan Dengan Roda Mouse." },
  { "en": "Apa Fungsi Variabel Sistem Qtext Pada AutoCAD?", "id": "Menampilkan Teks Sebagai Kotak Kosong." },
  { "en": "Apa Fungsi Component Mode L293D Di Proteus?", "id": "Driver Motor Dual H Bridge." },
  { "en": "Apa Fungsi Relay Khusus M8000 Pada PLC Mitsubishi?", "id": "Kontak Selalu On Saat Run." },
  { "en": "Apa Fungsi Relay Khusus M8002 Pada PLC Mitsubishi?", "id": "Pulsa On Sesaat Awal Run." },
  { "en": "Komponen ULN2003 Di Proteus Berfungsi Sebagai?", "id": "Array Transistor Darlington Arus Tinggi." },
  { "en": "Apa Fungsi Perintah Bcount Pada AutoCAD Express?", "id": "Menghitung Jumlah Blok Terseleksi." },
  { "en": "Apa Fungsi Fungsi BitRead Pada Arduino?", "id": "Membaca Status Bit Tertentu." },
  { "en": "Bagaimana Cara Mengatur Kepadatan Mesh AutoCAD?", "id": "Ubah Variabel Surftab1 Dan Surftab2." },
  { "en": "Apa Fungsi Tombol Ctrl Shift H AutoCAD?", "id": "Menyembunyikan Semua Palet." },
  { "en": "Apa Fungsi Perintah Mocoro Pada AutoCAD Express?", "id": "Move Copy Rotate Sekaligus." },
  { "en": "Komponen IC 74HC595 Di Proteus Adalah?", "id": "Shift Register Serial To Parallel." },
  { "en": "Apa Fungsi Relay Khusus M8013 Pada PLC Mitsubishi?", "id": "Pulsa Clock Satu Detik." },
  { "en": "Apa Fungsi Relay Khusus M8011 Pada PLC Mitsubishi?", "id": "Pulsa Clock Sepuluh Milidetik." },
  { "en": "Bagaimana Cara Mengatur Smoothness 3D AutoCAD?", "id": "Ubah Variabel Facetres." },
  { "en": "Apa Fungsi Interrupts Pada Program Arduino?", "id": "Mengaktifkan Kembali Fungsi Interupsi." },
  { "en": "Apa Itu Flag P On Pada PLC Omron?", "id": "Bit Selalu Bernilai On." },
  { "en": "Apa Itu Flag P Off Pada PLC Omron?", "id": "Bit Selalu Bernilai Off." },
  { "en": "Shortcut Apa Untuk Include Library Arduino?", "id": "Tekan Ctrl + Shift + I." },
  { "en": "Apa Fungsi Macro BitSet Pada Arduino?", "id": "Mengubah Bit Menjadi Satu." },
  { "en": "Bagaimana Cara Menghapus Jalur Di Proteus?", "id": "Klik Kanan Dua Kali Jalur." },
  { "en": "Apa Fungsi Flag P 1s Pada PLC Omron?", "id": "Pulsa Clock Satu Detik." },
  { "en": "Apa Fungsi Flag P First Cycle PLC Omron?", "id": "On Hanya Saat Siklus Pertama." },
  { "en": "Apa Fungsi Perintah Textfill Pada AutoCAD?", "id": "Mengatur Isian Font TrueType." },
  { "en": "Apa Itu Komponen PC817 Di Proteus?", "id": "Optocoupler Satu Channel." },
  { "en": "Apa Itu Komponen MOC3021 Di Proteus?", "id": "Optocoupler Pemicu Triac." },
  { "en": "Shortcut Apa Untuk Switch View GX Works?", "id": "Tekan Alt + F1." },
  { "en": "Apa Fungsi Macro HighByte Pada Arduino?", "id": "Mengambil Byte Posisi Tinggi." },
  { "en": "Apa Fungsi Traffic Light Model Di Proteus?", "id": "Simulasi Visual Lampu Lalu Lintas." },
  { "en": "Apa Fungsi Relay Khusus M8020 PLC Mitsubishi?", "id": "Flag Hasil Operasi Nol." },
  { "en": "Bagaimana Cara Membuat Teks Melengkung AutoCAD?", "id": "Gunakan Perintah Arctext." },
  { "en": "Apa Fungsi Operator Bitwise Shift Left Arduino?", "id": "Geser Bit Ke Kiri." },
  { "en": "Apa Itu Bus Mode Di Skematik Proteus?", "id": "Mode Menggambar Jalur Bus." },
  { "en": "Shortcut Apa Untuk Open Sketch Folder Arduino?", "id": "Tekan Ctrl + K." },
  { "en": "Apa Fungsi Fungsi Radians Pada Arduino?", "id": "Konversi Derajat Ke Radian." },
  { "en": "Komponen Apa Yang Mengisolasi Tegangan Tinggi?", "id": "Optocoupler Atau Optoisolator." },
  { "en": "Apa Fungsi Relay Khusus M8022 PLC Mitsubishi?", "id": "Flag Carry Hasil Operasi." },
  { "en": "Bagaimana Cara Menampilkan Garis Isolines AutoCAD?", "id": "Ubah Variabel Isolines." },
  { "en": "Apa Itu EEPROM Put Template Arduino?", "id": "Menulis Objek Data Ke Memori." },
  { "en": "Apa Fungsi Logic Toggle Di Proteus?", "id": "Input Logika Dengan Pengunci." },
  { "en": "Apa Shortcut Online Edit Mode GX Works?", "id": "Tekan Shift + F3." },
  { "en": "Apa Fungsi Perintah Audit Pada AutoCAD?", "id": "Memeriksa Dan Memperbaiki Error Gambar." },
  { "en": "Apa Fungsi Operator Bitwise Shift Right Arduino?", "id": "Geser Bit Ke Kanan." },
  { "en": "Apa Itu Logic State Di Proteus?", "id": "Input Logika Manual 0 1." },
  { "en": "Bagaimana Cara Recover File Rusak AutoCAD?", "id": "Gunakan Perintah Recover." },
  { "en": "Apa Fungsi Perintah Purge All AutoCAD?", "id": "Hapus Semua Item Tidak Terpakai." },
  { "en": "Apa Tipe Data Word Pada Arduino?", "id": "Bilangan Positif 16 Bit." },
  { "en": "Komponen Apa Driver Motor DC Di Proteus?", "id": "IC L298 Atau L293." },
  { "en": "Apa Fungsi Relay Khusus M8003 PLC Mitsubishi?", "id": "Pulsa Off Sesaat Awal Run." },
  { "en": "Shortcut Apa Untuk Buka Settings Arduino?", "id": "Tekan Ctrl + Comma." },
  { "en": "Apa Fungsi Fungsi Degrees Pada Arduino?", "id": "Konversi Radian Ke Derajat." },
  { "en": "Apa Itu Resolusi ADC Arduino Uno?", "id": "Sepuluh Bit Atau 1024." },
  { "en": "Apa Fungsi Flag P Er Pada PLC Omron?", "id": "Indikator Kesalahan Instruksi." },
  { "en": "Bagaimana Cara Membuat Cincin Solid AutoCAD?", "id": "Gunakan Perintah Donut." },
  { "en": "Apa Fungsi Macro LowByte Pada Arduino?", "id": "Mengambil Byte Posisi Rendah." },
  { "en": "Apa Itu Sounder Buzzer Di Proteus?", "id": "Komponen Penghasil Suara Piezo." },
  { "en": "Apa Shortcut Work Online CX Programmer?", "id": "Tekan Ctrl + W." },
  { "en": "Apa Fungsi Perintah Trace Pada AutoCAD?", "id": "Membuat Garis Dengan Ketebalan." },
  { "en": "Apa Fungsi Macro BitClear Pada Arduino?", "id": "Mengubah Bit Menjadi Nol." },
  { "en": "Komponen Apa Pemicu Relay 5 Volt?", "id": "Transistor NPN Sebagai Saklar." },
  { "en": "Apa Fungsi Flag P GT PLC Omron?", "id": "Indikator Lebih Besar Dari." },
  { "en": "Shortcut Apa Untuk Auto Format Arduino?", "id": "Tekan Ctrl + T." },
  { "en": "Apa Fungsi Serial AvailableForWrite Arduino?", "id": "Cek Ruang Buffer Kirim." },
  { "en": "Apa Itu Motor Stepper Di Proteus?", "id": "Motor Bergerak Per Langkah Sudut." },
  { "en": "Apa Fungsi Flag P LT PLC Omron?", "id": "Indikator Lebih Kecil Dari." },
  { "en": "Bagaimana Cara Mengatur Blipmode AutoCAD?", "id": "Ubah Variabel Blipmode." },
  { "en": "Apa Fungsi Macro Word Pada Arduino?", "id": "Konversi Dua Byte Ke Word." },
  { "en": "Apa Itu Relay SPDT Di Proteus?", "id": "Relay Satu Induk Dua Posisi." },
  { "en": "Shortcut Apa Untuk Zoom Out Proteus?", "id": "Tekan Tombol F7." },
  { "en": "Apa Fungsi Perintah Solid Pada AutoCAD?", "id": "Membuat Area Solid 2D." },
  { "en": "Apa Fungsi Fungsi Sq Pada Arduino?", "id": "Menghitung Kuadrat Sebuah Angka." },
  { "en": "Apa Itu IC MAX232 Di Proteus?", "id": "Konverter Level TTL Ke RS232." },
  { "en": "Apa Fungsi Relay Khusus M8014 PLC Mitsubishi?", "id": "Pulsa Clock Satu Menit." },
  { "en": "Bagaimana Cara Mengatur Mirrtext AutoCAD?", "id": "Ubah Variabel Mirrtext." },
  { "en": "Apa Fungsi Pin AREF Pada Arduino?", "id": "Tegangan Referensi Analog Eksternal." },
  { "en": "Apa Itu Relay DPDT Di Proteus?", "id": "Relay Dua Induk Dua Posisi." },
  { "en": "Shortcut Apa Untuk Zoom In Proteus?", "id": "Tekan Tombol F6." },
  { "en": "Apa Fungsi Perintah Chspace Pada AutoCAD?", "id": "Pindah Objek Model Ke Layout." },
  { "en": "Apa Fungsi Karakter Escape Backslash Zero?", "id": "Karakter Null Terminasi String." },
  { "en": "Apa Itu Komponen 74HC14 Di Proteus?", "id": "Hex Inverter Schmitt Trigger." },
  { "en": "Apa Fungsi AnalogWriteResolution Pada Arduino Due?", "id": "Mengatur Resolusi Bit PWM." },
  { "en": "Bagaimana Cara Membuat Infinite Line AutoCAD?", "id": "Gunakan Perintah Xline." },
  { "en": "Apa Fungsi Pin VIN Pada Arduino?", "id": "Input Tegangan Sumber Eksternal." },
  { "en": "Apa Itu Power Rail Configuration Proteus?", "id": "Konfigurasi Tegangan Supply Global." },
  { "en": "Apa Shortcut Zoom All Proteus?", "id": "Tekan Tombol F8." },
  { "en": "Apa Fungsi Perintah Align Pada AutoCAD?", "id": "Menyelaraskan Objek 2D Atau 3D." },
  { "en": "Apa Fungsi Macro Bit Pada Arduino?", "id": "Menghitung Nilai Sebuah Bit." },
  { "en": "Apa Fungsi Pin IOREF Pada Arduino?", "id": "Referensi Level Tegangan IO." },
  { "en": "Apa Itu IC 74HC165 Di Proteus?", "id": "Shift Register Parallel To Serial." },
  { "en": "Bagaimana Cara Break Object 1 Point?", "id": "Gunakan Perintah Break At Point." },
  { "en": "Apa Fungsi Perintah Lengthen Pada AutoCAD?", "id": "Mengubah Panjang Garis Geometri." },
  { "en": "Apa Fungsi Wire SetClock Arduino?", "id": "Mengatur Kecepatan Clock I2C." },
  { "en": "Apa Fungsi Pre Production Check Proteus?", "id": "Cek Desain Sebelum Manufaktur." },
  { "en": "Apa Fungsi Flag P EQ PLC Omron?", "id": "Indikator Sama Dengan." },
  { "en": "Bagaimana Cara Insert OLE Object?", "id": "Ketik Insertobj Lalu Enter." },
  { "en": "Apa Fungsi Perintah Join Pada AutoCAD?", "id": "Menggabungkan Garis Segaris Terpisah." },
  { "en": "Apa Fungsi Perintah Laycur Pada AutoCAD (Computer Aided Design)?", "id": "Mengubah Layer Objek Ke Layer Aktif." },
  { "en": "Apa Fungsi Perintah Laymch Pada AutoCAD?", "id": "Menyamakan Layer Objek Dengan Objek Lain." },
  { "en": "Shortcut Apa Untuk Membuka Visual LISP Editor AutoCAD?", "id": "Ketik Vlide Lalu Enter." },
  { "en": "Apa Fungsi Perintah Dimspace Pada Dimensi AutoCAD?", "id": "Mengatur Jarak Spasi Antar Dimensi." },
  { "en": "Apa Fungsi Perintah Dimbreak Pada Dimensi AutoCAD?", "id": "Memutus Dimensi Yang Memotong Objek Lain." },
  { "en": "Apa Fungsi Pre-Production Check Di Proteus?", "id": "Memeriksa Kesalahan Desain Sebelum Dicetak." },
  { "en": "Apa Fungsi Instruksi BCNT (Bit Counter) PLC?", "id": "Menghitung Jumlah Bit Bernilai Satu." },
  { "en": "Apa Fungsi Instruksi GRY (Gray Code) PLC?", "id": "Konversi Kode Biner Ke Gray." },
  { "en": "Komponen LF351 Di Proteus Berfungsi Sebagai?", "id": "Wide Bandwidth JFET Operational Amplifier." },
  { "en": "Apa Fungsi Perintah Tcircle Pada AutoCAD Express?", "id": "Membuat Lingkaran Mengelilingi Teks." },
  { "en": "Apa Fungsi Fungsi Strcat Pada C++ Arduino?", "id": "Menggabungkan Dua String Char Array." },
  { "en": "Bagaimana Cara Mengatur Ukuran Grip AutoCAD?", "id": "Ubah Variabel Gripsize." },
  { "en": "Apa Fungsi Tombol Ctrl Shift A AutoCAD?", "id": "Toggle Group Selection On Off." },
  { "en": "Apa Fungsi Perintah Extrim Pada AutoCAD Express?", "id": "Memotong Objek Di Luar Batas Kurva." },
  { "en": "Komponen IC 74193 Up Down Counter?", "id": "Penghitung Biner Naik Turun 4 Bit." },
  { "en": "Apa Fungsi Instruksi ROL (Rotate Left) PLC?", "id": "Memutar Bit Data Ke Kiri." },
  { "en": "Apa Fungsi Instruksi ROR (Rotate Right) PLC?", "id": "Memutar Bit Data Ke Kanan." },
  { "en": "Bagaimana Cara Mengatur Transparansi Xref AutoCAD?", "id": "Ubah Variabel Xdwgfadectl." },
  { "en": "Apa Fungsi Interrupt Mode Change?", "id": "Picu Saat Logika Pin Berubah." },
  { "en": "Apa Itu Data Memory File (E) Pada PLC?", "id": "Bank Memori Data Eksternal." },
  { "en": "Apa Itu Task Flag (TK) Pada PLC?", "id": "Status Eksekusi Tugas Program." },
  { "en": "Shortcut Apa Untuk Membuka Library Manager Arduino?", "id": "Tekan Ctrl + Shift + I." },
  { "en": "Apa Fungsi Library SPI SetDataMode?", "id": "Mengatur Fase Dan Polaritas Clock." },
  { "en": "Bagaimana Cara Menampilkan Hidden Pins Proteus?", "id": "Edit Component Properties Hidden Pins." },
  { "en": "Apa Fungsi Instruksi SWAP (Swap Bytes) PLC?", "id": "Menukar Posisi Byte Atas Bawah." },
  { "en": "Apa Fungsi Instruksi XFER (Block Transfer) PLC?", "id": "Menyalin Blok Data Memori." },
  { "en": "Apa Fungsi Perintah Dimtedit Pada AutoCAD?", "id": "Mengedit Posisi Teks Dimensi." },
  { "en": "Apa Itu Handshaking Hardware RTS CTS?", "id": "Kontrol Aliran Data Berbasis Kabel." },
  { "en": "Apa Itu Heap Memory Pada Arduino?", "id": "Memori Dinamis Untuk Variabel." },
  { "en": "Shortcut Apa Untuk Device Test GX Works?", "id": "Tekan Alt + 1." },
  { "en": "Apa Fungsi LiquidCrystal SetCursor?", "id": "Pindahkan Kursor Ke Kolom Baris." },
  { "en": "Apa Fungsi Intermodulation Graph Proteus?", "id": "Analisis Intermodulasi Sinyal RF." },
  { "en": "Apa Fungsi Timer Retentive On Delay?", "id": "Timer Simpan Waktu Meski Mati." },
  { "en": "Bagaimana Cara Membuat Block Property Table?", "id": "Gunakan Block Editor Properties." },
  { "en": "Apa Fungsi Operator Bitwise Not (Tilde)?", "id": "Membalik Nilai Bit Satu Nol." },
  { "en": "Apa Itu Bus Label Di Proteus?", "id": "Memberi Nama Jalur Bus Data." },
  { "en": "Shortcut Apa Untuk Membuka Markup Set Manager?", "id": "Tekan Ctrl + 7." },
  { "en": "Apa Fungsi Serial ReadBytesUntil Arduino?", "id": "Baca Data Sampai Karakter Tertentu." },
  { "en": "Komponen Apa Yang Menstabilkan Titik Kerja?", "id": "Resistor Bias Pembagi Tegangan." },
  { "en": "Apa Fungsi Instruksi DECO (Decoder) PLC?", "id": "Mendekode Nilai Bit Ke Word." },
  { "en": "Bagaimana Cara Xref Path Type Absolute?", "id": "Set Jalur File Referensi Mutlak." },
  { "en": "Apa Itu EEPROM Update Int Arduino?", "id": "Tulis Integer Hanya Jika Berbeda." },
  { "en": "Apa Fungsi S-Parameter Graph Proteus?", "id": "Analisis Hamburan Parameter Frekuensi Tinggi." },
  { "en": "Apa Shortcut Find Device Comment GX Works?", "id": "Tekan Ctrl + F." },
  { "en": "Apa Fungsi Perintah Spline Method CV?", "id": "Membuat Spline Control Vertices." },
  { "en": "Apa Fungsi Operator Bitwise XOR (Topi)?", "id": "Logika Exclusive OR Per Bit." },
  { "en": "Apa Itu Current Probe Di Proteus?", "id": "Alat Ukur Arus Tanpa Putus." },
  { "en": "Bagaimana Cara Export Layout Ke EPS?", "id": "Gunakan Perintah Psout." },
  { "en": "Apa Fungsi Perintah Solidedit Face Offset?", "id": "Geser Permukaan Solid Jarak Tertentu." },
  { "en": "Apa Tipe Data Unsigned Char Array?", "id": "Larik Byte Data Positif." },
  { "en": "Komponen Apa Pemicu Diac?", "id": "Tegangan Breakdown Dua Arah." },
  { "en": "Apa Fungsi Instruksi ENCO (Encoder) PLC?", "id": "Mengencode Bit Aktif Ke Nilai." },
  { "en": "Shortcut Apa Untuk Design Feed AutoCAD?", "id": "Tekan Ctrl + 0." },
  { "en": "Apa Fungsi Keyword Goto Arduino?", "id": "Lompat Ke Label Program." },
  { "en": "Apa Itu Resolusi ADC 24 Bit?", "id": "Enam Belas Juta Tingkat Data." },
  { "en": "Apa Fungsi Instruksi STC (Set Carry)?", "id": "Mengaktifkan Flag Carry." },
  { "en": "Bagaimana Cara Copy With Base Point?", "id": "Tekan Ctrl + Shift + C." },
  { "en": "Apa Fungsi Keyword Extern Arduino?", "id": "Deklarasi Variabel Di File Lain." },
  { "en": "Apa Itu Sensor Hall Effect Linear?", "id": "Output Tegangan Proporsional Magnet." },
  { "en": "Apa Shortcut Verify Project GX Works?", "id": "Pilih Menu Project Verify." },
  { "en": "Apa Fungsi Perintah Chamfer Angle?", "id": "Potong Sudut Berdasarkan Derajat." },
  { "en": "Apa Fungsi LiquidCrystal Display On?", "id": "Hidupkan Tampilan Teks LCD." },
  { "en": "Komponen Apa Yang Mengatur Arus Basis?", "id": "Resistor Basis Transistor." },
  { "en": "Apa Fungsi Access Right Flag PLC?", "id": "Status Hak Akses Memori." },
  { "en": "Shortcut Apa Untuk Show Sketch Folder?", "id": "Tekan Ctrl + K." },
  { "en": "Apa Fungsi Serial Println BINARY?", "id": "Cetak Data Format Biner." },
  { "en": "Apa Itu Pulse Generator Di Proteus?", "id": "Sumber Pulsa Digital Terprogram." },
  { "en": "Apa Fungsi Instruksi XNR (Exclusive NOR)?", "id": "Logika Output 1 Jika Sama." },
  { "en": "Bagaimana Cara Membuat Spot Light AutoCAD?", "id": "Ketik Spotlight Lalu Enter." },
  { "en": "Apa Fungsi IsGraph Pada Karakter Arduino?", "id": "Cek Karakter Grafis Terlihat." },
  { "en": "Apa Itu SPI Debugger Proteus?", "id": "Monitor Komunikasi Protokol SPI." },
  { "en": "Shortcut Apa Untuk Zoom Previous AutoCAD?", "id": "Ketik Z Lalu P." },
  { "en": "Apa Fungsi Perintah Imprint AutoCAD?", "id": "Cetak Geometri 2D Ke 3D." },
  { "en": "Apa Fungsi String ToCharArray?", "id": "Salin String Ke Buffer Char." },
  { "en": "Apa Itu IC 74151 Multiplexer?", "id": "Pemilih Data 8 Ke 1." },
  { "en": "Apa Fungsi Interrupt Mask (MSKS) PLC?", "id": "Masking Interupsi Input Output." },
  { "en": "Bagaimana Cara Mengukur Jarak 3D?", "id": "Gunakan Perintah 3dpoly Dan List." },
  { "en": "Apa Fungsi Pin XTAL1 ATMega328?", "id": "Input Osilator Kristal." },
  { "en": "Apa Itu Analog Ground Proteus?", "id": "Ground Referensi Sinyal Analog." },
  { "en": "Shortcut Apa Untuk Info Center AutoCAD?", "id": "Tekan Ctrl + 5." },
  { "en": "Apa Fungsi Perintah Helix Height?", "id": "Atur Tinggi Total Spiral." },
  { "en": "Apa Fungsi Karakter Escape Backslash Question?", "id": "Menampilkan Tanda Tanya." },
  { "en": "Apa Itu Regulator Adjustable LM317?", "id": "Penstabil Tegangan Positif Variabel." },
  { "en": "Apa Fungsi Analog WriteFrequency Pin 9?", "id": "Ubah Frekuensi PWM Pin 9." },
  { "en": "Bagaimana Cara Atur Tampilan Titik Relatif?", "id": "Set Pdsize Ke Nilai Persen." },
  { "en": "Apa Fungsi Pin MISO Arduino Mega?", "id": "Data Masuk Master SPI." },
  { "en": "Apa Itu Zone Mode Keepout?", "id": "Area Terlarang Jalur PCB." },
  { "en": "Apa Shortcut Close Tab Arduino?", "id": "Tekan Ctrl + W." },
  { "en": "Apa Fungsi Perintah Scale Reference?", "id": "Skala Objek Berdasarkan Referensi." },
  { "en": "Apa Fungsi Operator Bitwise OR Assignment?", "id": "Logika OR Dan Simpan." },
  { "en": "Apa Fungsi Pin MOSI Arduino Mega?", "id": "Data Keluar Master SPI." },
  { "en": "Apa Itu DIP Switch 8 Way?", "id": "Saklar Geser Delapan Kutub." },
  { "en": "Bagaimana Cara Update Thumbnail Preview?", "id": "Ketik Updatethumbs Lalu Enter." },
  { "en": "Apa Fungsi Perintah Dimjogged Angle?", "id": "Dimensi Sudut Pusat Jauh." },
  { "en": "Apa Fungsi Wire Write Array?", "id": "Kirim Array Byte Ke I2C." },
  { "en": "Apa Fungsi Production File Generator?", "id": "Hasilkan File Manufaktur Gerber." },
  { "en": "Apa Fungsi Cycle Time Flag (P_Cycle)?", "id": "Waktu Eksekusi Satu Siklus." },
  { "en": "Bagaimana Cara Insert OLE Link?", "id": "Centang Link Di Paste Special." },
  { "en": "Apa Fungsi Perintah Subtract Solid Region?", "id": "Potong Area Region Solid." },
  { "en": "Apa Fungsi Perintah Attredef Pada AutoCAD (Computer Aided Design)?", "id": "Mendefinisikan Ulang Blok Dan Atribut." },
  { "en": "Apa Fungsi Perintah Attsync Pada AutoCAD?", "id": "Menyesuaikan Atribut Blok Dengan Definisi." },
  { "en": "Shortcut Apa Untuk Membuka Visual Basic Editor AutoCAD?", "id": "Tekan Alt + F11." },
  { "en": "Apa Fungsi Variabel Sistem Psltscale Pada AutoCAD?", "id": "Skala Tipe Garis Paper Space." },
  { "en": "Apa Fungsi Variabel Sistem Visretain Pada AutoCAD?", "id": "Menyimpan Status Layer Xref." },
  { "en": "Apa Fungsi Force Voltage Pada Simulasi Proteus?", "id": "Memaksa Tegangan Titik Tertentu." },
  { "en": "Apa Fungsi Instruksi RCL (Rotate Carry Left) PLC?", "id": "Putar Kiri Melalui Carry Flag." },
  { "en": "Apa Fungsi Instruksi RCR (Rotate Carry Right) PLC?", "id": "Putar Kanan Melalui Carry Flag." },
  { "en": "Komponen LM3914 Di Proteus Berfungsi Sebagai?", "id": "Driver Tampilan LED Bar Dot." },
  { "en": "Apa Fungsi Perintah Battman Pada AutoCAD?", "id": "Mengelola Definisi Atribut Blok." },
  { "en": "Apa Fungsi String ToCharArray Pada Arduino?", "id": "Menyalin String Ke Buffer Karakter." },
  { "en": "Bagaimana Cara Mengatur Ukuran Marker Snap AutoCAD?", "id": "Ubah Variabel Autosnap." },
  { "en": "Apa Fungsi Tombol Ctrl Shift H Pada AutoCAD?", "id": "Menyembunyikan Semua Palet Terbuka." },
  { "en": "Apa Fungsi Perintah Eattext Pada AutoCAD?", "id": "Ekstrak Data Atribut Ke Tabel." },
  { "en": "Komponen IC 74HC573 Octal Latch Adalah?", "id": "Pengunci Data 8 Bit Transparan." },
  { "en": "Apa Fungsi Instruksi NEG (Negate) Pada PLC?", "id": "Mengubah Tanda Positif Negatif Nilai." },
  { "en": "Apa Fungsi Instruksi SIGN (Sign Extension) PLC?", "id": "Ekstensi Bit Tanda Ke Word." },
  { "en": "Bagaimana Cara Mengatur Transparansi Seleksi AutoCAD?", "id": "Ubah Variabel Selectionareaopacity." },
  { "en": "Apa Fungsi Interrupt Mode Change Pin?", "id": "Picu Interupsi Saat Status Berubah." },
  { "en": "Apa Itu Data Memory Indirect (E) PLC?", "id": "Pengalamatan Memori Data Tidak Langsung." },
  { "en": "Apa Itu Trace Flag (TR) Pada PLC?", "id": "Bit Pelacakan Eksekusi Program." },
  { "en": "Shortcut Apa Untuk Previous Tab Arduino?", "id": "Tekan Ctrl + Alt + Left." },
  { "en": "Apa Fungsi Library SPI TransferBuffer Arduino?", "id": "Transfer Blok Data Lewat SPI." },
  { "en": "Bagaimana Cara Mengatur Warna Wire Proteus?", "id": "Menu Template Set Design Colours." },
  { "en": "Apa Fungsi Instruksi SLS (Shift Left Single)?", "id": "Geser Kiri Satu Bit Data." },
  { "en": "Apa Fungsi Instruksi SRS (Shift Right Single)?", "id": "Geser Kanan Satu Bit Data." },
  { "en": "Apa Fungsi Perintah Dimdisassociate Pada Dimensi?", "id": "Memutus Hubungan Dimensi Dan Geometri." },
  { "en": "Apa Itu Framing Error Komunikasi Serial?", "id": "Kesalahan Format Frame Data Diterima." },
  { "en": "Apa Itu Null Pointer Exception Arduino?", "id": "Akses Memori Ke Alamat Kosong." },
  { "en": "Shortcut Apa Untuk Monitoring Mode GX Works?", "id": "Tekan Tombol F3." },
  { "en": "Apa Fungsi LiquidCrystal ScrollDisplayRight?", "id": "Geser Tampilan Teks Ke Kanan." },
  { "en": "Apa Fungsi Transfer Characteristic Graph Proteus?", "id": "Analisis Hubungan Input Output DC." },
  { "en": "Apa Fungsi Timer Watchdog (WDT) PLC?", "id": "Reset Sistem Jika Program Macet." },
  { "en": "Bagaimana Cara Insert OLE Object AutoCAD?", "id": "Gunakan Perintah Insertobj." },
  { "en": "Apa Fungsi Operator Bitwise Shift Assignment?", "id": "Geser Bit Dan Simpan Nilai." },
  { "en": "Apa Itu Terminal Power Ground Proteus?", "id": "Ground Referensi Arus Besar." },
  { "en": "Shortcut Apa Untuk Membuka Layer States Manager?", "id": "Ketik Layerstate Lalu Enter." },
  { "en": "Apa Fungsi Serial ReadBytesUntil Arduino?", "id": "Baca Byte Sampai Karakter Terminator." },
  { "en": "Komponen Apa Yang Mengatur Arus Base?", "id": "Resistor Pembatas Arus Basis." },
  { "en": "Apa Fungsi Instruksi HEX (Ascii To Hex)?", "id": "Konversi Kode ASCII Ke Hex." },
  { "en": "Bagaimana Cara Xref Bind Insert Style?", "id": "Gabungkan Xref Hapus Nama File." },
  { "en": "Apa Itu EEPROM Get Template Arduino?", "id": "Membaca Objek Data Dari Memori." },
  { "en": "Apa Fungsi Digital Timing Analysis Proteus?", "id": "Analisis Waktu Sinyal Logika Digital." },
  { "en": "Apa Shortcut Step Run GX Works?", "id": "Tekan Tombol F8." },
  { "en": "Apa Fungsi Perintah Sketch Polyline AutoCAD?", "id": "Gambar Garis Bebas Sebagai Polyline." },
  { "en": "Apa Fungsi Operator Compound Modulo (Persen Sama Dengan)?", "id": "Sisa Bagi Dan Simpan Hasil." },
  { "en": "Apa Itu Voltage Probe Differential?", "id": "Ukur Selisih Tegangan Dua Titik." },
  { "en": "Bagaimana Cara Export Layout Ke SVG?", "id": "Gunakan Perintah Exportsvg." },
  { "en": "Apa Fungsi Perintah Solidedit Face Color?", "id": "Ubah Warna Permukaan Solid 3D." },
  { "en": "Apa Tipe Data Unsigned Long Long?", "id": "Integer Positif 64 Bit Arduino." },
  { "en": "Komponen Apa Pemicu SCR Light Activated?", "id": "Cahaya Masuk Ke Gate LASCR." },
  { "en": "Apa Fungsi Instruksi ZCP (Zone Compare) PLC?", "id": "Bandingkan Nilai Dalam Rentang Zona." },
  { "en": "Shortcut Apa Untuk Membuka Quick Select AutoCAD?", "id": "Ketik Qselect Lalu Enter." },
  { "en": "Apa Fungsi Keyword Goto Pada Arduino?", "id": "Lompat Ke Label Dalam Fungsi." },
  { "en": "Apa Itu Resolusi ADC 16 Bit?", "id": "Nilai Nol Sampai 65535." },
  { "en": "Apa Fungsi Instruksi STC (Set Carry Flag)?", "id": "Set Bit Carry Menjadi Satu." },
  { "en": "Bagaimana Cara Paste Ke Block AutoCAD?", "id": "Tekan Ctrl + Shift + V." },
  { "en": "Apa Fungsi Keyword Auto Pada Arduino?", "id": "Deduksi Tipe Data Otomatis." },
  { "en": "Apa Itu Sensor Ultrasonic Ping Proteus?", "id": "Modul Pengukur Jarak Suara." },
  { "en": "Apa Shortcut PLC Transfer To PLC?", "id": "Tekan Ctrl + T." },
  { "en": "Apa Fungsi Perintah Chamfer Trim Mode?", "id": "Potong Garis Ujung Chamfer." },
  { "en": "Apa Fungsi LiquidCrystal Blink Cursor?", "id": "Kedipkan Kursor Kotak LCD." },
  { "en": "Komponen Apa Yang Menghubungkan Arus Emitter?", "id": "Resistor Stabilisasi Panas Emitter." },
  { "en": "Apa Fungsi Underflow Flag (UF) PLC?", "id": "Indikator Hasil Bawah Batas Minimum." },
  { "en": "Shortcut Apa Untuk Increase Indent Arduino?", "id": "Tekan Tab." },
  { "en": "Apa Fungsi Serial Println Octal?", "id": "Cetak Nilai Format Oktal." },
  { "en": "Apa Itu Signal Generator Pulse Proteus?", "id": "Pembangkit Sinyal Pulsa Kotak." },
  { "en": "Apa Fungsi Instruksi XNR (Exclusive NOR)?", "id": "Logika Output Satu Jika Sama." },
  { "en": "Bagaimana Cara Setting Geographical Location AutoCAD?", "id": "Gunakan Perintah Geographiclocation." },
  { "en": "Apa Fungsi IsPunct Pada Karakter Arduino?", "id": "Cek Karakter Tanda Baca." },
  { "en": "Apa Itu I2C Debugger Spy Mode?", "id": "Intip Data Tanpa Ganggu Jalur." },
  { "en": "Shortcut Apa Untuk Zoom Center AutoCAD?", "id": "Ketik Z Lalu C." },
  { "en": "Apa Fungsi Perintah Thicken Surface?", "id": "Ubah Surface Menjadi Solid Tebal." },
  { "en": "Apa Fungsi String ToInt Base 10?", "id": "Konversi String Ke Integer Desimal." },
  { "en": "Apa Itu IC 74147 Decimal Encoder?", "id": "Encoder 10 Line Ke BCD." },
  { "en": "Apa Fungsi Instruksi EI (Enable Interrupts)?", "id": "Izinkan Proses Interupsi PLC." },
  { "en": "Bagaimana Cara Ukur Momen Inersia?", "id": "Gunakan Perintah Massprop." },
  { "en": "Apa Fungsi Pin XTAL2 ATMega328?", "id": "Output Osilator Kristal." },
  { "en": "Apa Itu Chassis Ground Proteus?", "id": "Ground Terhubung Body Alat." },
  { "en": "Shortcut Apa Untuk Ribbon Close AutoCAD?", "id": "Ketik Ribbonclose Lalu Enter." },
  { "en": "Apa Fungsi Perintah Helix Twist?", "id": "Atur Arah Putaran CW CCW." },
  { "en": "Apa Fungsi Karakter Escape Backslash F?", "id": "Form Feed Ganti Halaman." },
  { "en": "Apa Itu Regulator Switching Buck?", "id": "Penurun Tegangan Efisiensi Tinggi." },
  { "en": "Apa Fungsi Analog Reference Internal 1.1V?", "id": "Referensi ADC 1.1 Volt Tetap." },
  { "en": "Bagaimana Cara Atur Tampilan Point Relatif?", "id": "Set Pdsize Negatif Persen Layar." },
  { "en": "Apa Fungsi Pin SS Arduino Mega?", "id": "Slave Select SPI." },
  { "en": "Apa Itu Drill Hole Di PCB?", "id": "Lubang Bor Tanpa Plating." },
  { "en": "Apa Shortcut Next Tab Arduino?", "id": "Tekan Ctrl + Alt + Right." },
  { "en": "Apa Fungsi Perintah Scale Reference?", "id": "Skala Objek Menggunakan Panjang Acuan." },
  { "en": "Apa Fungsi Operator Bitwise XOR Assignment?", "id": "Logika XOR Dan Simpan." },
  { "en": "Apa Fungsi Pin SCL Arduino Mega?", "id": "Clock Sinkronisasi I2C." },
  { "en": "Apa Itu Resistor SIP Proteus?", "id": "Resistor Array Single Inline Package." },
  { "en": "Bagaimana Cara Create Data Link AutoCAD?", "id": "Gunakan Perintah Datalink." },
  { "en": "Apa Fungsi Perintah Dimjogged Linear?", "id": "Dimensi Linear Dengan Jog." },
  { "en": "Apa Fungsi Wire Write String?", "id": "Kirim Teks String Ke I2C." },
  { "en": "Apa Fungsi ODB++ Output Proteus?", "id": "Format Data Manufaktur PCB Universal." },
  { "en": "Apa Fungsi Step Flag (P_Step)?", "id": "Status Sedang Eksekusi Step." },
  { "en": "Bagaimana Cara Insert OLE Embed?", "id": "Pilih Create New Pada Insert." },
  { "en": "Apa Fungsi Perintah Subtract Solid Composite?", "id": "Potong Objek Solid Gabungan." },
  { "en": "Apa Fungsi Perintah Attdisp Pada AutoCAD (Computer Aided Design)?", "id": "Mengatur Visibilitas Atribut Blok Global." },
  { "en": "Apa Fungsi Perintah Dxfout Pada AutoCAD?", "id": "Mengekspor Gambar Ke Format DXF." },
  { "en": "Shortcut Apa Untuk Membuka Customize User Interface AutoCAD?", "id": "Ketik Cui Lalu Enter." },
  { "en": "Apa Fungsi Perintah Dxfin Pada AutoCAD?", "id": "Mengimpor File Gambar Format DXF." },
  { "en": "Apa Fungsi Perintah Menuload Pada AutoCAD?", "id": "Memuat File Menu Kustomisasi Tambahan." },
  { "en": "Apa Fungsi Gate Swap Pada PCB ARES Proteus?", "id": "Menukar Gerbang Logika Dalam Satu IC." },
  { "en": "Apa Fungsi Instruksi MEAN (Mean Value) PLC?", "id": "Menghitung Rata Rata Nilai Data." },
  { "en": "Apa Fungsi Instruksi WSFT (Word Shift) PLC?", "id": "Menggeser Data Word Kiri Kanan." },
  { "en": "Komponen LM324N Di Proteus Berfungsi Sebagai?", "id": "Quad Op Amp Low Power." },
  { "en": "Apa Fungsi Perintah Tjust Pada AutoCAD Express?", "id": "Mengubah Justifikasi Teks Tanpa Geser." },
  { "en": "Apa Fungsi Fungsi Strcpy Pada C++ Arduino?", "id": "Menyalin Isi String Ke Buffer." },
  { "en": "Bagaimana Cara Mengatur Ukuran Simbol Blip AutoCAD?", "id": "Ubah Variabel Blipmode." },
  { "en": "Apa Fungsi Tombol Ctrl Shift S AutoCAD?", "id": "Save As Gambar Nama Baru." },
  { "en": "Apa Fungsi Perintah Torient Pada AutoCAD Express?", "id": "Memutar Teks Agar Mudah Dibaca." },
  { "en": "Komponen IC 74HC4017 Decade Counter Adalah?", "id": "Penghitung Johnson 5 Tahap." },
  { "en": "Apa Fungsi Instruksi SER (Search) Pada PLC?", "id": "Mencari Nilai Data Dalam Range." },
  { "en": "Apa Fungsi Instruksi SUM (Summation) PLC?", "id": "Menjumlahkan Total Data Dalam Range." },
  { "en": "Bagaimana Cara Mengatur Mode Drag AutoCAD?", "id": "Ubah Variabel Dragmode." },
  { "en": "Apa Fungsi Interrupt Mode High Level?", "id": "Picu Interupsi Selama Sinyal Tinggi." },
  { "en": "Apa Itu Data Memory Special Register (SD) PLC?", "id": "Register Data Fungsi Khusus Sistem." },
  { "en": "Apa Itu Index Register (IX) Pada PLC?", "id": "Register Penunjuk Alamat Indeks X." },
  { "en": "Shortcut Apa Untuk Previous Window Arduino?", "id": "Tekan Ctrl + Shift + Tab." },
  { "en": "Apa Fungsi Library Wire SetClockStretchLimit?", "id": "Atur Batas Waktu Stretching Clock." },
  { "en": "Bagaimana Cara Mengubah Grid Dot Proteus?", "id": "Menu View Grid Dots." },
  { "en": "Apa Fungsi Instruksi RORW (Rotate Right Word)?", "id": "Putar Kanan Data Word." },
  { "en": "Apa Fungsi Instruksi ROLW (Rotate Left Word)?", "id": "Putar Kiri Data Word." },
  { "en": "Apa Fungsi Perintah Dimjogged Pada AutoCAD?", "id": "Dimensi Radius Dengan Garis Tekuk." },
  { "en": "Apa Itu Break Condition Komunikasi Serial?", "id": "Sinyal Logika Rendah Durasi Panjang." },
  { "en": "Apa Itu Out Of Memory Arduino?", "id": "Kehabisan RAM Untuk Alokasi Data." },
  { "en": "Shortcut Apa Untuk Step Back Simulation GX Works?", "id": "Fitur Tidak Tersedia Secara Langsung." },
  { "en": "Apa Fungsi LiquidCrystal DisplayOff?", "id": "Matikan Tampilan Tanpa Hapus Data." },
  { "en": "Apa Fungsi Digital Graph Proteus?", "id": "Analisis Waktu Sinyal Logika Digital." },
  { "en": "Apa Fungsi Timer Pulse Extend (T) PLC?", "id": "Memperpanjang Durasi Pulsa Input." },
  { "en": "Bagaimana Cara Membuat Block Icon AutoCAD?", "id": "Gunakan Perintah Blockicon." },
  { "en": "Apa Fungsi Operator Bitwise AND Assignment?", "id": "Logika AND Dan Simpan Nilai." },
  { "en": "Apa Itu Wire Label Di Proteus?", "id": "Nama Identifikasi Jalur Kabel." },
  { "en": "Shortcut Apa Untuk Membuka Markup Set Manager?", "id": "Tekan Ctrl + 7." },
  { "en": "Apa Fungsi Serial ReadString Arduino?", "id": "Baca Seluruh String Dari Buffer." },
  { "en": "Komponen Apa Yang Menstabilkan Tegangan Output?", "id": "Kapasitor Filter Output." },
  { "en": "Apa Fungsi Instruksi HEX (Hexadecimal) PLC?", "id": "Konversi Kode ASCII Ke Hex." },
  { "en": "Bagaimana Cara Xref Path Type No Path?", "id": "Hapus Jalur Penyimpanan Referensi." },
  { "en": "Apa Itu EEPROM Read Float Arduino?", "id": "Membaca Nilai Float Dari Memori." },
  { "en": "Apa Fungsi Interactive Analysis Graph Proteus?", "id": "Simulasi Grafik Waktu Nyata." },
  { "en": "Apa Shortcut Step Over GX Works?", "id": "Tekan Tombol F10." },
  { "en": "Apa Fungsi Perintah Spline Fit Points?", "id": "Spline Melewati Titik Titik Tentukan." },
  { "en": "Apa Fungsi Operator Compound Bitwise OR?", "id": "Logika OR Dan Simpan Hasil." },
  { "en": "Apa Itu Voltage Probe Marker?", "id": "Penanda Nilai Tegangan Di Grafik." },
  { "en": "Bagaimana Cara Export Layout Ke BMP?", "id": "Gunakan Perintah Bmpout." },
  { "en": "Apa Fungsi Perintah Solidedit Body Separate?", "id": "Pisahkan Solid Yang Tidak Bersatu." },
  { "en": "Apa Tipe Data Unsigned Long Long?", "id": "Integer Positif 64 Bit." },
  { "en": "Komponen Apa Pemicu Triac Fase Angle?", "id": "Rangkaian RC Dan Diac." },
  { "en": "Apa Fungsi Instruksi ZONE (Zone Control) PLC?", "id": "Menambah Bias Pada Nilai Zona." },
  { "en": "Shortcut Apa Untuk Membuka Visual Styles?", "id": "Ketik Visualstyles Lalu Enter." },
  { "en": "Apa Fungsi Keyword Switch Pada Arduino?", "id": "Struktur Kontrol Percabangan Banyak." },
  { "en": "Apa Itu Resolusi ADC 10 Bit?", "id": "Seribu Dua Puluh Empat Nilai." },
  { "en": "Apa Fungsi Instruksi CMC (Complement Carry)?", "id": "Membalik Status Carry Flag." },
  { "en": "Bagaimana Cara Paste As Hyperlink AutoCAD?", "id": "Pilih Paste As Hyperlink." },
  { "en": "Apa Fungsi Keyword Inline Pada Arduino?", "id": "Optimasi Fungsi Masuk Ke Kode." },
  { "en": "Apa Itu Sensor Ultrasonic SRF05 Proteus?", "id": "Modul Jarak Ultrasonik 5 Pin." },
  { "en": "Apa Shortcut PLC Upload GX Works?", "id": "Tekan Ctrl + T (Write)." },
  { "en": "Apa Fungsi Perintah Chamfer Edge Loop?", "id": "Pilih Seluruh Tepi Loop Solid." },
  { "en": "Apa Fungsi LiquidCrystal RightToLeft?", "id": "Arah Penulisan Teks Kanan Kiri." },
  { "en": "Komponen Apa Yang Mengatur Arus Gate IGBT?", "id": "Resistor Seri Gate." },
  { "en": "Apa Fungsi Message Flag (MSG) PLC?", "id": "Menampilkan Pesan Pada Alat Monitor." },
  { "en": "Shortcut Apa Untuk Decrease Indent Arduino?", "id": "Tekan Shift + Tab." },
  { "en": "Apa Fungsi Serial Println DEC?", "id": "Cetak Nilai Format Desimal." },
  { "en": "Apa Itu Signal Generator DC Proteus?", "id": "Sumber Tegangan DC Variabel." },
  { "en": "Apa Fungsi Instruksi ANLD (And Load)?", "id": "Seri Antara Dua Blok Paralel." },
  { "en": "Bagaimana Cara Setting Render Environment?", "id": "Ketik Renderenvironment Lalu Enter." },
  { "en": "Apa Fungsi IsLower Pada Karakter Arduino?", "id": "Cek Apakah Huruf Kecil." },
  { "en": "Apa Itu I2C Debugger Master Mode?", "id": "Simulasi Sebagai Master I2C." },
  { "en": "Shortcut Apa Untuk Zoom Object AutoCAD?", "id": "Ketik Z Lalu O." },
  { "en": "Apa Fungsi Perintah Thicken AutoCAD?", "id": "Ubah Surface Menjadi Solid." },
  { "en": "Apa Fungsi String ToFloat Arduino?", "id": "Konversi String Ke Desimal Float." },
  { "en": "Apa Itu IC 7447 BCD Decoder?", "id": "Dekoder BCD Ke 7 Segment." },
  { "en": "Apa Fungsi Instruksi EI (Enable Interrupts)?", "id": "Aktifkan Interupsi Global PLC." },
  { "en": "Bagaimana Cara Ukur Radius Geometri?", "id": "Gunakan Perintah Measureradius." },
  { "en": "Apa Fungsi Pin RESET Arduino Nano?", "id": "Restart Mikrokontroler." },
  { "en": "Apa Itu Power VDD Proteus?", "id": "Tegangan Positif IC Digital." },
  { "en": "Shortcut Apa Untuk Clean Screen AutoCAD?", "id": "Tekan Ctrl + 0." },
  { "en": "Apa Fungsi Perintah Helix Base Point?", "id": "Tentukan Titik Awal Spiral." },
  { "en": "Apa Fungsi Karakter Escape Backslash B?", "id": "Backspace Hapus Satu Karakter." },
  { "en": "Apa Itu Regulator 7812?", "id": "Penstabil Tegangan 12 Volt Positif." },
  { "en": "Apa Fungsi Analog Reference External?", "id": "Tegangan Referensi Dari Pin AREF." },
  { "en": "Bagaimana Cara Atur Tampilan Titik Mutlak?", "id": "Set Pdsize Ke Nilai Positif." },
  { "en": "Apa Fungsi Pin SS Arduino Uno?", "id": "Slave Select SPI." },
  { "en": "Apa Itu Pin Swap Di PCB ARES?", "id": "Tukar Pin Ekuivalen Komponen." },
  { "en": "Apa Shortcut Verify Sketch Arduino?", "id": "Tekan Ctrl + R." },
  { "en": "Apa Fungsi Perintah Scale Copy?", "id": "Skala Objek Dan Buat Salinan." },
  { "en": "Apa Fungsi Operator Bitwise XOR Assignment?", "id": "Logika XOR Dan Simpan." },
  { "en": "Apa Fungsi Pin SCL Arduino Nano?", "id": "Clock I2C." },
  { "en": "Apa Itu Resistor Array Proteus?", "id": "Paket Resistor Satu Badan." },
  { "en": "Bagaimana Cara Download Data Link?", "id": "Klik Kanan Download From Source." },
  { "en": "Apa Fungsi Perintah Dimjogged Angle?", "id": "Dimensi Sudut Pusat Jauh." },
  { "en": "Apa Fungsi Wire Write String?", "id": "Kirim Teks Lewat I2C." },
  { "en": "Apa Fungsi Follow Me Routing Proteus?", "id": "Jalur PCB Mengikuti Gerakan Mouse." },
  { "en": "Apa Fungsi Always ON Flag (P_On)?", "id": "Bit Selalu Hidup." },
  { "en": "Bagaimana Cara Insert OLE Link?", "id": "Pilih Create From File Link." },
  { "en": "Apa Fungsi Perintah Subtract Solid Region?", "id": "Potong Region Solid 2D." },
  { "en": "Apa Fungsi Perintah Scaletext Pada AutoCAD (Computer Aided Design)?", "id": "Mengubah Skala Objek Teks Tanpa Pindah." },
  { "en": "Apa Fungsi Perintah Justifytext Pada AutoCAD?", "id": "Mengubah Titik Justifikasi Teks." },
  { "en": "Shortcut Apa Untuk Membuka Visual Basic Editor AutoCAD?", "id": "Tekan Alt + F11." },
  { "en": "Apa Fungsi Variabel Sistem Mirrtext Pada AutoCAD?", "id": "Mengatur Orientasi Teks Saat Di-Mirror." },
  { "en": "Apa Fungsi Variabel Sistem Aunits Pada AutoCAD?", "id": "Mengatur Satuan Sudut Gambar." },
  { "en": "Apa Fungsi Connectivity Check Pada PCB Proteus?", "id": "Memastikan Semua Jalur Terhubung Benar." },
  { "en": "Apa Fungsi Instruksi FLT (Floating Point) PLC?", "id": "Konversi Integer Ke Bilangan Desimal." },
  { "en": "Apa Fungsi Instruksi INT (Integer) Pada PLC?", "id": "Konversi Desimal Ke Bilangan Bulat." },
  { "en": "Komponen LM7812 Di Proteus Berfungsi Sebagai?", "id": "Regulator Tegangan Positif 12 Volt." },
  { "en": "Apa Fungsi Perintah Qlatte Pada AutoCAD Express?", "id": "Melampirkan Leader Ke Teks Mtext." },
  { "en": "Apa Fungsi Fungsi Strcat Pada Arduino?", "id": "Menggabungkan Dua String Karakter C." },
  { "en": "Bagaimana Cara Mengatur Ukuran Aperture AutoCAD?", "id": "Ketik Aperture Lalu Masukkan Nilai." },
  { "en": "Apa Fungsi Tombol Ctrl Shift C AutoCAD?", "id": "Copybase Menyalin Dengan Titik Basis." },
  { "en": "Apa Fungsi Perintah Torient Pada AutoCAD?", "id": "Memutar Teks Agar Mudah Dibaca." },
  { "en": "Komponen IC 74HC00 Quad NAND Gate?", "id": "Empat Gerbang NAND CMOS." },
  { "en": "Apa Fungsi Instruksi RAD (Radian) Pada PLC?", "id": "Mengubah Derajat Menjadi Radian." },
  { "en": "Apa Fungsi Instruksi DEG (Degree) Pada PLC?", "id": "Mengubah Radian Menjadi Derajat." },
  { "en": "Bagaimana Cara Mengatur Warna Model Space AutoCAD?", "id": "Buka Options Display Colors." },
  { "en": "Apa Fungsi Interrupt Mode Falling?", "id": "Picu Saat Sinyal Turun." },
  { "en": "Apa Itu Data Memory Index (IX) PLC?", "id": "Register Index Untuk Pengalamatan." },
  { "en": "Apa Itu Data Memory Index (IY) PLC?", "id": "Register Index Y Untuk Pengalamatan." },
  { "en": "Shortcut Apa Untuk Membuka Serial Plotter Arduino?", "id": "Tekan Ctrl + Shift + L." },
  { "en": "Apa Fungsi Library Wire EndTransmission True?", "id": "Kirim Stop Condition Setelah Data." },
  { "en": "Bagaimana Cara Mengubah Layer PCB Proteus?", "id": "Klik Kanan Change Layer." },
  { "en": "Apa Fungsi Instruksi WSFT (Word Shift) PLC?", "id": "Geser Data Word Kiri Kanan." },
  { "en": "Apa Fungsi Instruksi ASFT (Asynchronous Shift) PLC?", "id": "Geser Register Secara Asinkron." },
  { "en": "Apa Fungsi Perintah Dimoverride Pada Dimensi?", "id": "Menimpa Variabel Sistem Dimensi Terpilih." },
  { "en": "Apa Itu Overrun Error Pada UART?", "id": "Data Baru Menimpa Data Lama." },
  { "en": "Apa Itu Stack Overflow Pada Mikrokontroler?", "id": "Tumpukan Memori Melebihi Batas RAM." },
  { "en": "Shortcut Apa Untuk Zoom Out GX Works?", "id": "Tekan Ctrl + Minus." },
  { "en": "Apa Fungsi LiquidCrystal Display On?", "id": "Menyalakan Tampilan Layar LCD." },
  { "en": "Apa Fungsi Distortion Graph Di Proteus?", "id": "Analisis Distorsi Harmonik Sinyal." },
  { "en": "Apa Fungsi Timer Pulse (TP) Pada PLC?", "id": "Menghasilkan Pulsa Durasi Tetap." },
  { "en": "Bagaimana Cara Insert Attribute AutoCAD?", "id": "Gunakan Perintah Attdef." },
  { "en": "Apa Fungsi Operator Bitwise AND Assignment?", "id": "Logika AND Dan Simpan Hasil." },
  { "en": "Apa Itu Bus Wire Di Proteus?", "id": "Jalur Data Tebal Banyak Sinyal." },
  { "en": "Shortcut Apa Untuk Membuka Visual Styles Manager?", "id": "Ketik Visualstyles Lalu Enter." },
  { "en": "Apa Fungsi Serial ReadBytes Arduino?", "id": "Membaca Byte Ke Buffer Array." },
  { "en": "Komponen Apa Yang Menstabilkan Arus Emitter?", "id": "Resistor Emitter Re." },
  { "en": "Apa Fungsi Instruksi ASCII (Convert To Ascii)?", "id": "Konversi Nilai Hex Ke ASCII." },
  { "en": "Bagaimana Cara Xref Path Type Relative?", "id": "Simpan Jalur Relatif File Induk." },
  { "en": "Apa Itu EEPROM Write Float Arduino?", "id": "Menulis Nilai Float Ke Memori." },
  { "en": "Apa Fungsi Reflection Coefficient Graph Proteus?", "id": "Analisis Pantulan Sinyal Saluran Transmisi." },
  { "en": "Apa Shortcut Step In GX Works?", "id": "Tekan Tombol F8." },
  { "en": "Apa Fungsi Perintah Spline CV AutoCAD?", "id": "Membuat Spline Control Vertices." },
  { "en": "Apa Fungsi Operator Compound Bitwise XOR?", "id": "Logika XOR Dan Simpan Hasil." },
  { "en": "Apa Itu Current Probe Marker?", "id": "Penanda Nilai Arus Pada Grafik." },
  { "en": "Bagaimana Cara Export Layout Ke WMF?", "id": "Gunakan Perintah Wmfout." },
  { "en": "Apa Fungsi Perintah Solidedit Face Rotate?", "id": "Putar Permukaan Solid Pada Sumbu." },
  { "en": "Apa Tipe Data Signed Char Arduino?", "id": "Byte Dengan Tanda Negatif Positif." },
  { "en": "Komponen Apa Pemicu SCR Zero Crossing?", "id": "Optocoupler Zero Crossing Detector." },
  { "en": "Apa Fungsi Instruksi LZ (Leading Zero) PLC?", "id": "Menghitung Jumlah Nol Di Depan." },
  { "en": "Shortcut Apa Untuk Membuka External References?", "id": "Ketik Xref Lalu Enter." },
  { "en": "Apa Fungsi Keyword Default Pada Switch?", "id": "Jalankan Jika Tidak Ada Case." },
  { "en": "Apa Itu Resolusi ADC 14 Bit?", "id": "Enam Belas Ribu Tingkat Data." },
  { "en": "Apa Fungsi Instruksi STC (Set Carry)?", "id": "Mengaktifkan Bit Carry Flag." },
  { "en": "Bagaimana Cara Paste As Block AutoCAD?", "id": "Tekan Ctrl + Shift + V." },
  { "en": "Apa Fungsi Keyword Constexpr Pada Arduino?", "id": "Evaluasi Konstanta Saat Kompilasi." },
  { "en": "Apa Itu Sensor Gas MQ135 Proteus?", "id": "Sensor Kualitas Udara." },
  { "en": "Apa Shortcut PLC Download GX Works?", "id": "Tekan Ctrl + T (Write)." },
  { "en": "Apa Fungsi Perintah Chamfer Edge Loop?", "id": "Chamfer Seluruh Tepi Loop." },
  { "en": "Apa Fungsi LiquidCrystal LeftToRight?", "id": "Arah Penulisan Kiri Ke Kanan." },
  { "en": "Komponen Apa Yang Mengatur Arus Drain?", "id": "Resistor Drain Transistor FET." },
  { "en": "Apa Fungsi Double Precision Float PLC?", "id": "Bilangan Desimal Presisi Ganda 64-Bit." },
  { "en": "Shortcut Apa Untuk Go To Line Arduino?", "id": "Tekan Ctrl + L." },
  { "en": "Apa Fungsi Serial Println OCT?", "id": "Cetak Nilai Format Oktal." },
  { "en": "Apa Itu Signal Generator Sine Proteus?", "id": "Sumber Sinyal Sinus Analog." },
  { "en": "Apa Fungsi Instruksi XNR (Exclusive NOR)?", "id": "Output 1 Jika Input Sama." },
  { "en": "Bagaimana Cara Setting Background Sky?", "id": "Gunakan Perintah Sunproperties Sky Status." },
  { "en": "Apa Fungsi IsDigit Pada Karakter Arduino?", "id": "Cek Apakah Karakter Angka 0-9." },
  { "en": "Apa Itu I2C Debugger Slave Mode?", "id": "Simulasi Sebagai Slave I2C." },
  { "en": "Shortcut Apa Untuk Zoom Dynamic AutoCAD?", "id": "Ketik Z Lalu D." },
  { "en": "Apa Fungsi Perintah Convert To Solid?", "id": "Ubah Objek 3D Menjadi Solid." },
  { "en": "Apa Fungsi String ToInt Base 8?", "id": "Konversi String Oktal Ke Integer." },
  { "en": "Apa Itu IC 74154 Demultiplexer?", "id": "Dekoder 4 Line Ke 16." },
  { "en": "Apa Fungsi Instruksi DI (Disable Interrupts)?", "id": "Matikan Interupsi Global Sementara." },
  { "en": "Bagaimana Cara Ukur Panjang Kurva?", "id": "Gunakan Perintah List." },
  { "en": "Apa Fungsi Pin VIN Arduino Nano?", "id": "Input Tegangan Eksternal 7-12V." },
  { "en": "Apa Itu Power VEE Proteus?", "id": "Sumber Tegangan Negatif Op-Amp." },
  { "en": "Shortcut Apa Untuk Clean Screen Off?", "id": "Tekan Ctrl + 0." },
  { "en": "Apa Fungsi Perintah Helix Base Position?", "id": "Tentukan Posisi Dasar Spiral." },
  { "en": "Apa Fungsi Karakter Backslash Single Quote?", "id": "Menampilkan Tanda Petik Satu." },
  { "en": "Apa Itu Regulator 7912?", "id": "Penstabil Tegangan 12 Volt Negatif." },
  { "en": "Apa Fungsi Analog Reference External?", "id": "Gunakan Pin AREF Sebagai Referensi." },
  { "en": "Bagaimana Cara Atur Tampilan Titik Kosong?", "id": "Set Pdmode Ke Nilai 1." },
  { "en": "Apa Fungsi Pin SS Arduino Leonardo?", "id": "Slave Select SPI." },
  { "en": "Apa Itu 3D Mechanical Model Proteus?", "id": "Model 3D Fisik Komponen." },
  { "en": "Apa Shortcut Comment Code Arduino?", "id": "Tekan Ctrl + Slash." },
  { "en": "Apa Fungsi Perintah Array Polar AutoCAD?", "id": "Gandakan Objek Melingkar." },
  { "en": "Apa Fungsi Operator Bitwise OR Assignment?", "id": "Logika OR Dan Simpan." },
  { "en": "Apa Fungsi Pin SDA Arduino Nano?", "id": "Data I2C." },
  { "en": "Apa Itu Capacitor Electrolytic Proteus?", "id": "Kapasitor Polaritas Positif Negatif." },
  { "en": "Bagaimana Cara Detach Xref?", "id": "Klik Kanan Detach." },
  { "en": "Apa Fungsi Perintah Dimjogged Linear?", "id": "Dimensi Linear Dengan Jog." },
  { "en": "Apa Fungsi Wire Write Buffer?", "id": "Kirim Buffer Data I2C." },
  { "en": "Apa Fungsi Bill Of Materials Report?", "id": "Laporan Daftar Komponen Proyek." },
  { "en": "Apa Fungsi Instruction Execution Flag?", "id": "Status Sedang Eksekusi Instruksi." },
  { "en": "Bagaimana Cara Paste Link AutoCAD?", "id": "Pilih Paste Special Paste Link." },
  { "en": "Apa Fungsi Perintah Subtract Solid Interfere?", "id": "Potong Volume Tabrakan Solid." },
  { "en": "Apa Fungsi Perintah Reverse Pada AutoCAD (Computer Aided Design)?", "id": "Membalik Arah Garis Atau Polyline." },
  { "en": "Apa Fungsi Perintah Texttofront Pada AutoCAD?", "id": "Membawa Semua Teks Ke Depan." },
  { "en": "Shortcut Apa Untuk Membuka Visual Basic Editor AutoCAD?", "id": "Tekan Alt + F11." },
  { "en": "Apa Fungsi Variabel Sistem Celtscale Pada AutoCAD?", "id": "Skala Tipe Garis Objek Saat Ini." },
  { "en": "Apa Fungsi Variabel Sistem Msltscale Pada AutoCAD?", "id": "Skala Tipe Garis Model Space." },
  { "en": "Apa Fungsi Follow Me Routing Pada PCB Proteus?", "id": "Membuat Jalur Mengikuti Gerakan Mouse." },
  { "en": "Apa Fungsi Instruksi SIN (Sine) Pada PLC?", "id": "Menghitung Sinus Dari Nilai Radian." },
  { "en": "Apa Fungsi Instruksi COS (Cosine) Pada PLC?", "id": "Menghitung Cosinus Dari Nilai Radian." },
  { "en": "Komponen LM358N Di Proteus Berfungsi Sebagai?", "id": "Dual Op Amp Low Power." },
  { "en": "Apa Fungsi Perintah Ncopy Pada AutoCAD Express?", "id": "Menyalin Objek Bersarang Dalam Blok." },
  { "en": "Apa Fungsi Fungsi Strlen Pada C++ Arduino?", "id": "Menghitung Panjang String Karakter." },
  { "en": "Bagaimana Cara Mengatur Ukuran Grip Object AutoCAD?", "id": "Ketik Gripsize Lalu Masukkan Nilai." },
  { "en": "Apa Fungsi Tombol Ctrl Shift P AutoCAD?", "id": "Toggle Quick Properties On Off." },
  { "en": "Apa Fungsi Perintah Overkill Pada AutoCAD?", "id": "Menghapus Geometri Yang Bertumpuk." },
  { "en": "Komponen IC 74HC74 Dual D Flip Flop?", "id": "Dua Flip Flop Tipe D." },
  { "en": "Apa Fungsi Instruksi TAN (Tangent) Pada PLC?", "id": "Menghitung Tangen Dari Nilai Radian." },
  { "en": "Apa Fungsi Instruksi ASIN (Arc Sine) PLC?", "id": "Menghitung Sudut Dari Nilai Sinus." },
  { "en": "Bagaimana Cara Mengatur Transparansi Xref AutoCAD?", "id": "Ubah Variabel Xdwgfadectl." },
  { "en": "Apa Fungsi Interrupt Mode Change Pin?", "id": "Picu Saat Logika Pin Berubah." },
  { "en": "Apa Itu Data Memory Special (D) PLC?", "id": "Register Data Fungsi Khusus Sistem." },
  { "en": "Apa Itu Index Register (Z) Pada PLC?", "id": "Register Penunjuk Alamat Indeks Z." },
  { "en": "Shortcut Apa Untuk Membuka Plotter Manager AutoCAD?", "id": "Ketik Plottermanager Lalu Enter." },
  { "en": "Apa Fungsi Library Wire SetClock Arduino?", "id": "Mengatur Frekuensi Clock I2C." },
  { "en": "Bagaimana Cara Mengubah Grid Lines Proteus?", "id": "Menu View Grid Lines." },
  { "en": "Apa Fungsi Instruksi ACOS (Arc Cosine) PLC?", "id": "Menghitung Sudut Dari Nilai Cosinus." },
  { "en": "Apa Fungsi Instruksi ATAN (Arc Tangent) PLC?", "id": "Menghitung Sudut Dari Nilai Tangen." },
  { "en": "Apa Fungsi Perintah Dimtiledit Pada Dimensi?", "id": "Memindahkan Teks Dimensi Sepanjang Garis." },
  { "en": "Apa Itu Break Condition Pada Serial?", "id": "Sinyal Low Lebih Dari Satu Frame." },
  { "en": "Apa Itu Heap Fragmentation Pada Arduino?", "id": "Memori RAM Terpecah Pecah." },
  { "en": "Shortcut Apa Untuk Zoom Page GX Works?", "id": "Fitur Tidak Tersedia Langsung." },
  { "en": "Apa Fungsi LiquidCrystal Blink Cursor?", "id": "Mengaktifkan Kedipan Kursor Kotak." },
  { "en": "Apa Fungsi Fourier Analysis Graph Proteus?", "id": "Analisis Spektrum Frekuensi Sinyal." },
  { "en": "Apa Fungsi Timer Watchdog (WDT) Refresh?", "id": "Mencegah Reset Sistem Oleh Watchdog." },
  { "en": "Bagaimana Cara Insert Dynamic Block AutoCAD?", "id": "Drag Drop Dari Tool Palettes." },
  { "en": "Apa Fungsi Operator Bitwise OR Assignment?", "id": "Logika OR Dan Simpan Hasil." },
  { "en": "Apa Itu Net Class Di Proteus?", "id": "Mengelompokkan Jalur Dengan Aturan Sama." },
  { "en": "Shortcut Apa Untuk Membuka Design Feed?", "id": "Tekan Ctrl + 0." },
  { "en": "Apa Fungsi Serial ReadBytesUntil Arduino?", "id": "Baca Byte Sampai Karakter Terminator." },
  { "en": "Komponen Apa Yang Menstabilkan Tegangan Basis?", "id": "Dioda Bias Pada Basis." },
  { "en": "Apa Fungsi Instruksi HEX (Ascii To Hex)?", "id": "Konversi Kode ASCII Ke Hex." },
  { "en": "Bagaimana Cara Xref Path Type Absolute?", "id": "Simpan Jalur File Referensi Mutlak." },
  { "en": "Apa Itu EEPROM Put Template Arduino?", "id": "Menulis Struktur Data Ke Memori." },
  { "en": "Apa Fungsi Conformance Analysis Graph Proteus?", "id": "Cek Kepatuhan Terhadap Standar Sinyal." },
  { "en": "Apa Shortcut Step Run GX Works?", "id": "Tekan Tombol F8." },
  { "en": "Apa Fungsi Perintah Spline Method Fit?", "id": "Spline Melewati Titik Titik." },
  { "en": "Apa Fungsi Operator Compound Bitwise XOR?", "id": "Logika XOR Dan Simpan Hasil." },
  { "en": "Apa Itu Voltage Probe Differential?", "id": "Ukur Beda Potensial Dua Titik." },
  { "en": "Bagaimana Cara Export Layout Ke EPS?", "id": "Gunakan Perintah Psout." },
  { "en": "Apa Fungsi Perintah Solidedit Face Move?", "id": "Memindahkan Permukaan Solid Jarak Tertentu." },
  { "en": "Apa Tipe Data Unsigned Long Long?", "id": "Integer Positif 64 Bit Arduino." },
  { "en": "Komponen Apa Pemicu Triac Optocoupler?", "id": "Cahaya LED Inframerah Internal." },
  { "en": "Apa Fungsi Instruksi LIMIT (Limit Control)?", "id": "Membatasi Nilai Atas Dan Bawah." },
  { "en": "Shortcut Apa Untuk Membuka Quick Select AutoCAD?", "id": "Ketik Qselect Lalu Enter." },
  { "en": "Apa Fungsi Keyword Goto Pada Arduino?", "id": "Lompat Ke Label Tertentu." },
  { "en": "Apa Itu Resolusi ADC 12 Bit?", "id": "Nilai Nol Sampai 4095." },
  { "en": "Apa Fungsi Instruksi CLC (Clear Carry Flag)?", "id": "Mereset Status Bit Carry." },
  { "en": "Bagaimana Cara Paste Ke Koordinat Asli?", "id": "Pilih Paste To Original Coordinates." },
  { "en": "Apa Fungsi Keyword Volatile Pada Arduino?", "id": "Cegah Optimasi Kompiler Pada Variabel." },
  { "en": "Apa Itu Sensor Gas LPG Proteus?", "id": "Sensor Deteksi Kebocoran Gas." },
  { "en": "Apa Shortcut PLC Transfer To PLC?", "id": "Tekan Ctrl + T." },
  { "en": "Apa Fungsi Perintah Chamfer Trim Mode?", "id": "Memotong Sisa Garis Sudut." },
  { "en": "Apa Fungsi LiquidCrystal NoBlink Arduino?", "id": "Matikan Kedipan Kursor LCD." },
  { "en": "Komponen Apa Yang Mengatur Arus Emitter?", "id": "Resistor Emitter Re." },
  { "en": "Apa Fungsi Underflow Flag (UF) PLC?", "id": "Indikator Hasil Bawah Batas Minimum." },
  { "en": "Shortcut Apa Untuk Decrease Indent Arduino?", "id": "Tekan Shift + Tab." },
  { "en": "Apa Fungsi Serial Println Octal?", "id": "Cetak Nilai Format Oktal." },
  { "en": "Apa Itu Signal Generator Pulse Proteus?", "id": "Pembangkit Sinyal Pulsa Kotak." },
  { "en": "Apa Fungsi Instruksi XNR (Exclusive NOR)?", "id": "Output Satu Jika Input Sama." },
  { "en": "Bagaimana Cara Setting Geographical Location?", "id": "Gunakan Perintah Geographiclocation." },
  { "en": "Apa Fungsi IsPunct Pada Karakter Arduino?", "id": "Cek Karakter Tanda Baca." },
  { "en": "Apa Itu I2C Debugger Master Mode?", "id": "Simulasi Sebagai Master I2C." },
  { "en": "Shortcut Apa Untuk Zoom Dynamic AutoCAD?", "id": "Ketik Z Lalu D." },
  { "en": "Apa Fungsi Perintah Thicken Surface?", "id": "Ubah Surface Menjadi Solid Tebal." },
  { "en": "Apa Fungsi String ToInt Base 10?", "id": "Konversi String Desimal Ke Integer." },
  { "en": "Apa Itu IC 74154 Demultiplexer?", "id": "Dekoder 4 Line Ke 16." },
  { "en": "Apa Fungsi Instruksi DI (Disable Interrupts)?", "id": "Matikan Interupsi Global Sementara." },
  { "en": "Bagaimana Cara Ukur Momen Inersia?", "id": "Gunakan Perintah Massprop." },
  { "en": "Apa Fungsi Pin XTAL2 ATMega328?", "id": "Output Ke Kristal Osilator." },
  { "en": "Apa Itu Chassis Ground Proteus?", "id": "Ground Terhubung Ke Casing." },
  { "en": "Shortcut Apa Untuk Ribbon Close AutoCAD?", "id": "Ketik Ribbonclose Lalu Enter." },
  { "en": "Apa Fungsi Perintah Helix Turns?", "id": "Atur Jumlah Putaran Spiral." },
  { "en": "Apa Fungsi Karakter Escape Backslash F?", "id": "Form Feed Ganti Halaman." },
  { "en": "Apa Itu Regulator Switching Buck?", "id": "Penurun Tegangan Efisiensi Tinggi." },
  { "en": "Apa Fungsi Analog Reference Internal 1.1V?", "id": "Referensi ADC 1.1 Volt Tetap." },
  { "en": "Bagaimana Cara Atur Tampilan Titik Relatif?", "id": "Set Pdsize Negatif Persen Layar." },
  { "en": "Apa Fungsi Pin SS Arduino Mega?", "id": "Slave Select SPI." },
  { "en": "Apa Itu Blind Via Di PCB?", "id": "Lubang Lapisan Luar Ke Dalam." },
  { "en": "Apa Shortcut Next Tab Arduino?", "id": "Tekan Ctrl + Alt + Right." },
  { "en": "Apa Fungsi Perintah Scale Reference?", "id": "Skala Objek Berdasarkan Panjang Acuan." },
  { "en": "Apa Fungsi Operator Bitwise XOR Assignment?", "id": "Logika XOR Dan Simpan." },
  { "en": "Apa Fungsi Pin SCL Arduino Mega?", "id": "Clock Sinkronisasi I2C." },
  { "en": "Apa Itu Resistor SIP Proteus?", "id": "Resistor Array Single Inline Package." },
  { "en": "Bagaimana Cara Download Data Link?", "id": "Klik Kanan Download From Source." },
  { "en": "Apa Fungsi Perintah Dimjogged Linear?", "id": "Dimensi Linear Dengan Jog." },
  { "en": "Apa Fungsi Wire Write String?", "id": "Kirim Teks Lewat I2C." },
  { "en": "Apa Fungsi ODB++ Output Proteus?", "id": "Ekspor Data Manufaktur PCB." },
  { "en": "Apa Fungsi Step Flag (P_Step)?", "id": "Status Eksekusi Step Program." },
  { "en": "Bagaimana Cara Insert OLE Embed?", "id": "Pilih Create New Pada Insert." },
  { "en": "Apa Fungsi Perintah Subtract Solid Region?", "id": "Potong Region Solid 2D." },
  { "en": "Apa Fungsi Perintah Torient Pada AutoCAD Express?", "id": "Memutar Teks Agar Mudah Dibaca." },
  { "en": "Apa Fungsi Perintah Rtext Pada AutoCAD Express?", "id": "Membuat Objek Teks Reaktif Dinamis." },
  { "en": "Shortcut Apa Untuk Membuka Visual Basic Editor?", "id": "Tekan Alt + F11." },
  { "en": "Apa Fungsi Variabel Sistem Mirrtext Pada AutoCAD?", "id": "Mengatur Teks Terbalik Saat Mirror." },
  { "en": "Apa Fungsi Variabel Sistem Ltscales Pada AutoCAD?", "id": "Skala Tipe Garis Global." },
  { "en": "Apa Fungsi Pad Stack Mode Di Proteus?", "id": "Mengelola Tumpukan Pad PCB." },
  { "en": "Apa Fungsi Instruksi FLT (Floating Point) PLC?", "id": "Konversi Integer Ke Bilangan Desimal." },
  { "en": "Apa Fungsi Instruksi INT (Integer) Pada PLC?", "id": "Konversi Bilangan Desimal Ke Integer." },
  { "en": "Komponen LM3915 Di Proteus Berfungsi Sebagai?", "id": "Driver Tampilan LED Skala Logaritmik." },
  { "en": "Apa Fungsi Perintah Btrim Pada AutoCAD Express?", "id": "Memotong Objek Dengan Batas Blok." },
  { "en": "Apa Fungsi Fungsi String Reserve Pada Arduino?", "id": "Alokasi Memori Untuk String." },
  { "en": "Bagaimana Cara Mengatur Ukuran Grip Object?", "id": "Ubah Variabel Gripsize." },
  { "en": "Apa Fungsi Tombol Ctrl Shift H Pada AutoCAD?", "id": "Menyembunyikan Palet Yang Terbuka." },
  { "en": "Apa Fungsi Perintah Xplode Pada Blok?", "id": "Pecah Blok Pertahankan Properti Layer." },
  { "en": "Komponen IC 74HC595 Shift Register Adalah?", "id": "Register Geser Serial Ke Paralel." },
  { "en": "Apa Fungsi Instruksi SIN (Sine) Pada PLC?", "id": "Hitung Sinus Dari Sudut Radian." },
  { "en": "Apa Fungsi Instruksi COS (Cosine) Pada PLC?", "id": "Hitung Cosinus Dari Sudut Radian." },
  { "en": "Bagaimana Cara Mengatur Transparansi Seleksi?", "id": "Ubah Variabel Selectionareaopacity." },
  { "en": "Apa Fungsi Interrupt Mode Rising?", "id": "Picu Saat Sinyal Naik." },
  { "en": "Apa Itu Data Memory File (D) PLC?", "id": "Area Penyimpanan Data File." },
  { "en": "Apa Itu Step Relay (S) Area PLC?", "id": "Area Bit Kontrol Step Ladder." },
  { "en": "Shortcut Apa Untuk Previous Tab Arduino IDE?", "id": "Tekan Ctrl + Alt + Left." },
  { "en": "Apa Fungsi Library SPI SetBitOrder?", "id": "Atur Urutan Kirim Bit Data." },
  { "en": "Bagaimana Cara Mengubah Grid Spacing Proteus?", "id": "Tekan F4 F3 Atau F2." },
  { "en": "Apa Fungsi Instruksi TAN (Tangent) PLC?", "id": "Hitung Tangen Dari Sudut Radian." },
  { "en": "Apa Fungsi Instruksi ASIN (Arc Sine) PLC?", "id": "Hitung Sudut Dari Nilai Sinus." },
  { "en": "Apa Fungsi Perintah Dimtmove Pada AutoCAD?", "id": "Geser Teks Dimensi Bebas." },
  { "en": "Apa Itu Noise Margin Pada Digital?", "id": "Batas Toleransi Gangguan Sinyal." },
  { "en": "Apa Itu Dangling Pointer C++ Arduino?", "id": "Pointer Menunjuk Memori Tidak Valid." },
  { "en": "Shortcut Apa Untuk Zoom Out GX Works?", "id": "Tekan Ctrl + Minus." },
  { "en": "Apa Fungsi LiquidCrystal Display?", "id": "Aktifkan Tampilan Layar LCD." },
  { "en": "Apa Fungsi Audio Graph Di Proteus?", "id": "Analisis Sinyal Gelombang Suara." },
  { "en": "Apa Fungsi Timer On Delay (TON)?", "id": "Output Aktif Setelah Waktu Tunda." },
  { "en": "Bagaimana Cara Insert Attribute Definition?", "id": "Gunakan Perintah Attdef." },
  { "en": "Apa Fungsi Operator Bitwise AND (Ampersand)?", "id": "Logika DAN Per Bit." },
  { "en": "Apa Itu Global Net Label Proteus?", "id": "Label Sambungan Antar Halaman Skematik." },
  { "en": "Shortcut Apa Untuk Sheet Set Manager?", "id": "Tekan Ctrl + 4." },
  { "en": "Apa Fungsi Serial ReadStringUntil?", "id": "Baca String Sampai Karakter Batas." },
  { "en": "Komponen Apa Yang Mengatur Bias Basis?", "id": "Resistor Pembagi Tegangan." },
  { "en": "Apa Fungsi Instruksi ACOS (Arc Cosine)?", "id": "Hitung Sudut Dari Nilai Cosinus." },
  { "en": "Bagaimana Cara Xref Path Type Relative?", "id": "Simpan Jalur Relatif File Induk." },
  { "en": "Apa Itu EEPROM Put Arduino?", "id": "Tulis Data Objek Ke Memori." },
  { "en": "Apa Fungsi Fourier Graph Di Proteus?", "id": "Analisis Spektrum Frekuensi." },
  { "en": "Apa Shortcut Step Over GX Works?", "id": "Tekan Tombol F10." },
  { "en": "Apa Fungsi Perintah Sketch Pada AutoCAD?", "id": "Menggambar Garis Bebas." },
  { "en": "Apa Fungsi Operator Modulo (Persen)?", "id": "Sisa Hasil Pembagian." },
  { "en": "Apa Itu Voltage Probe Marker?", "id": "Penanda Nilai Tegangan Grafik." },
  { "en": "Bagaimana Cara Export Layout Ke EPS?", "id": "Gunakan Perintah Psout." },
  { "en": "Apa Fungsi Perintah Solidedit Face Taper?", "id": "Miringkan Permukaan Solid." },
  { "en": "Apa Tipe Data Unsigned Long?", "id": "Integer Positif 32 Bit." },
  { "en": "Komponen Apa Pemicu Triac?", "id": "Diac Atau Optotriac." },
  { "en": "Apa Fungsi Instruksi ATAN (Arc Tangent)?", "id": "Hitung Sudut Dari Nilai Tangen." },
  { "en": "Shortcut Apa Untuk Quick Select AutoCAD?", "id": "Ketik Qselect Lalu Enter." },
  { "en": "Apa Fungsi Keyword Continue Arduino?", "id": "Lanjut Iterasi Loop Berikutnya." },
  { "en": "Apa Itu Resolusi ADC 10 Bit?", "id": "Seribu Dua Puluh Empat." },
  { "en": "Apa Fungsi Instruksi STC (Set Carry)?", "id": "Aktifkan Flag Carry." },
  { "en": "Bagaimana Cara Paste As Hyperlink?", "id": "Pilih Paste As Hyperlink." },
  { "en": "Apa Fungsi Keyword Static Arduino?", "id": "Variabel Lokal Nilai Tetap." },
  { "en": "Apa Itu Sensor Gas MQ2?", "id": "Sensor Asap Dan Gas." },
  { "en": "Apa Shortcut PLC Write GX Works?", "id": "Tekan Ctrl + T." },
  { "en": "Apa Fungsi Perintah Chamfer Distance?", "id": "Atur Jarak Potong Sudut." },
  { "en": "Apa Fungsi LiquidCrystal NoDisplay?", "id": "Matikan Tampilan LCD." },
  { "en": "Komponen Apa Yang Membatasi Arus LED?", "id": "Resistor Seri." },
  { "en": "Apa Fungsi Instruction Execution Error Flag?", "id": "Indikator Kesalahan Instruksi." },
  { "en": "Shortcut Apa Untuk Auto Format Code?", "id": "Tekan Ctrl + T." },
  { "en": "Apa Fungsi Serial Println HEX?", "id": "Cetak Nilai Format Hexadesimal." },
  { "en": "Apa Itu Signal Generator DC?", "id": "Sumber Tegangan DC Variabel." },
  { "en": "Apa Fungsi Instruksi XNR (Exclusive NOR)?", "id": "Logika Output 1 Jika Sama." },
  { "en": "Bagaimana Cara Setting Geographical Location?", "id": "Ketik Geographiclocation Lalu Enter." },
  { "en": "Apa Fungsi IsPunct Pada Arduino?", "id": "Cek Karakter Tanda Baca." },
  { "en": "Apa Itu I2C Debugger Monitor?", "id": "Pantau Data Bus I2C." },
  { "en": "Shortcut Apa Untuk Zoom Object?", "id": "Ketik Z Lalu O." },
  { "en": "Apa Fungsi Perintah Thicken?", "id": "Ubah Surface Jadi Solid." },
  { "en": "Apa Fungsi String ToInt Base 16?", "id": "Konversi String Hex Ke Integer." },
  { "en": "Apa Itu IC 74147 Encoder?", "id": "Encoder Desimal Ke BCD." },
  { "en": "Apa Fungsi Instruksi DI (Disable Interrupt)?", "id": "Matikan Interupsi." },
  { "en": "Bagaimana Cara Ukur Radius?", "id": "Gunakan Perintah Measureradius." },
  { "en": "Apa Fungsi Pin RESET Arduino?", "id": "Restart Mikrokontroler." },
  { "en": "Apa Itu Power VCC Proteus?", "id": "Sumber Tegangan Positif." },
  { "en": "Shortcut Apa Untuk Clean Screen?", "id": "Tekan Ctrl + 0." },
  { "en": "Apa Fungsi Perintah Helix Base Radius?", "id": "Atur Radius Dasar Spiral." },
  { "en": "Apa Fungsi Karakter Backslash T?", "id": "Tabulasi Horizontal." },
  { "en": "Apa Itu Regulator 7805?", "id": "Stabilizer 5 Volt Positif." },
  { "en": "Apa Fungsi Analog Reference Internal?", "id": "Referensi ADC 1.1 Volt." },
  { "en": "Bagaimana Cara Atur Tampilan Titik?", "id": "Ketik Ptype Lalu Enter." },
  { "en": "Apa Fungsi Pin MISO Arduino?", "id": "Data Masuk Master SPI." },
  { "en": "Apa Itu Via Mode Proteus?", "id": "Buat Lubang Antar Layer." },
  { "en": "Apa Shortcut Verify Sketch?", "id": "Tekan Ctrl + R." },
  { "en": "Apa Fungsi Perintah Scale Reference?", "id": "Skala Dengan Panjang Acuan." },
  { "en": "Apa Fungsi Operator Bitwise XOR Assignment?", "id": "Logika XOR Dan Simpan." },
  { "en": "Apa Fungsi Pin SDA Arduino?", "id": "Data Serial I2C." },
  { "en": "Apa Itu Resistor DIL Proteus?", "id": "Resistor Array Dual Inline." },
  { "en": "Bagaimana Cara Update Data Link?", "id": "Klik Kanan Update Data Link." },
  { "en": "Apa Fungsi Perintah Dimjogged Angle?", "id": "Dimensi Sudut Pusat Jauh." },
  { "en": "Apa Fungsi Wire Write String?", "id": "Kirim Teks String I2C." },
  { "en": "Apa Fungsi ODB++ Output?", "id": "Ekspor Data Manufaktur PCB." },
  { "en": "Apa Fungsi Step Flag (P_Step)?", "id": "Status Eksekusi Step." },
  { "en": "Bagaimana Cara Insert OLE Link?", "id": "Pilih Link Di Paste Special." },
  { "en": "Apa Fungsi Perintah Subtract Solid?", "id": "Potong Volume Solid." },
  { "en": "Apa Fungsi Perintah Surfsculpt Pada AutoCAD 3D?", "id": "Memotong Surface Dengan Geometri Tertutup." },
  { "en": "Apa Fungsi Perintah Convtonurbs Pada AutoCAD?", "id": "Mengubah Solid Menjadi NURBS Surface." },
  { "en": "Shortcut Apa Untuk Membuka Action Recorder AutoCAD?", "id": "Ketik Actrecord Lalu Enter." },
  { "en": "Apa Fungsi Variabel Sistem Surftab1 Pada AutoCAD?", "id": "Mengatur Kepadatan Mesh Arah M." },
  { "en": "Apa Fungsi Variabel Sistem Surftab2 Pada AutoCAD?", "id": "Mengatur Kepadatan Mesh Arah N." },
  { "en": "Apa Fungsi Output Pick And Place Proteus?", "id": "File Koordinat Penempatan Komponen Mesin." },
  { "en": "Apa Fungsi Instruksi SCL2 (Scaling 2) PLC?", "id": "Konversi Skala Linear Tingkat Lanjut." },
  { "en": "Apa Fungsi Instruksi AVG (Average Value) PLC?", "id": "Menghitung Rata Rata Nilai Input." },
  { "en": "Komponen AD620 Di Proteus Berfungsi Sebagai?", "id": "Instrumentation Amplifier Presisi Tinggi." },
  { "en": "Apa Fungsi Perintah Meshmood Pada AutoCAD?", "id": "Menghaluskan Objek Mesh 3D." },
  { "en": "Apa Fungsi Register DDRB Pada Arduino?", "id": "Mengatur Arah Data Port B." },
  { "en": "Bagaimana Cara Mengatur Facetres Di AutoCAD?", "id": "Ubah Variabel Sistem Facetres." },
  { "en": "Apa Fungsi Tombol Ctrl Shift E AutoCAD?", "id": "Menggunakan Perintah Extrude Face." },
  { "en": "Apa Fungsi Perintah Cvshow Pada NURBS AutoCAD?", "id": "Menampilkan Control Vertices Surface." },
  { "en": "Komponen IC 74HC595 Output Enable Pin?", "id": "Mengaktifkan Output Jika Logika Low." },
  { "en": "Apa Fungsi Instruksi ESD (Exponent) Pada PLC?", "id": "Menghitung Nilai Eksponensial Data." },
  { "en": "Apa Fungsi Instruksi LOG (Logarithm) Pada PLC?", "id": "Menghitung Logaritma Natural Data." },
  { "en": "Bagaimana Cara Mengatur Ucsfollow Di AutoCAD?", "id": "Ubah Variabel Sistem Ucsfollow." },
  { "en": "Apa Fungsi Register PORTD Pada Arduino?", "id": "Menulis Data Digital Port D." },
  { "en": "Apa Itu Data Memory Latch 2 (L) PLC?", "id": "Area Memori Latch Kedua." },
  { "en": "Apa Itu File Register (R) Pada PLC?", "id": "Memori Penyimpanan Data File Ekstensi." },
  { "en": "Shortcut Apa Untuk Upload Using Programmer?", "id": "Tekan Ctrl + Shift + U." },
  { "en": "Apa Fungsi Library Avr Sleep H?", "id": "Mengatur Mode Hemat Daya Mikrokontroler." },
  { "en": "Bagaimana Cara Mengatur Drill Table Proteus?", "id": "Menu Tools Drill Table." },
  { "en": "Apa Fungsi Instruksi SWAPW (Swap Word) PLC?", "id": "Tukar Posisi Word Dalam Double." },
  { "en": "Apa Fungsi Instruksi BSW (Byte Swap) PLC?", "id": "Tukar Posisi Byte Dalam Word." },
  { "en": "Apa Fungsi Perintah Dimjogang Pada Dimensi?", "id": "Dimensi Sudut Dengan Garis Jog." },
  { "en": "Apa Itu Break Signal Pada UART?", "id": "Sinyal Low Lebih Dari Frame." },
  { "en": "Apa Itu Watchdog Timeout Reset Arduino?", "id": "Reset Karena Waktu Habis." },
  { "en": "Shortcut Apa Untuk Step Back GX Works?", "id": "Tekan Shift + F10." },
  { "en": "Apa Fungsi Register PIND Pada Arduino?", "id": "Membaca Input Digital Port D." },
  { "en": "Apa Fungsi Eye Diagram Graph Proteus?", "id": "Analisis Kualitas Sinyal Komunikasi Digital." },
  { "en": "Apa Fungsi Timer Accumulative Off Delay?", "id": "Timer Akumulasi Penundaan Matinya Output." },
  { "en": "Bagaimana Cara Membuat Point Cloud AutoCAD?", "id": "Gunakan Perintah Pointcloudattach." },
  { "en": "Apa Fungsi Operator Bitwise Shift Right Zero Fill?", "id": "Geser Kanan Isi Nol (>>>)." },
  { "en": "Apa Itu Sub-Sheet Port Di Proteus?", "id": "Terminal Penghubung Halaman Induk Anak." },
  { "en": "Shortcut Apa Untuk Membuka Parameter Manager?", "id": "Ketik Parameters Lalu Enter." },
  { "en": "Apa Fungsi Serial Write Buf Size?", "id": "Kirim Buffer Dengan Ukuran Tertentu." },
  { "en": "Komponen Apa Yang Menghubungkan Kaki Reset?", "id": "Resistor Pull Up Reset." },
  { "en": "Apa Fungsi Instruksi FTOI (Float To Int)?", "id": "Konversi Float Ke Integer." },
  { "en": "Bagaimana Cara Xref Overlay AutoCAD?", "id": "Referensi Tanpa Membawa Referensi Anak." },
  { "en": "Apa Itu EEPROM Update Block Arduino?", "id": "Update Blok Data Jika Berbeda." },
  { "en": "Apa Fungsi Easy HDL Di Proteus?", "id": "Menulis Kode VHDL Atau Verilog." },
  { "en": "Apa Shortcut PLC Diagnostics GX Works?", "id": "Menu Diagnostics PLC Diagnostics." },
  { "en": "Apa Fungsi Perintah Loft Guides AutoCAD?", "id": "Loft Mengikuti Garis Panduan." },
  { "en": "Apa Fungsi Operator Sizeof Pada Arduino?", "id": "Mengembalikan Ukuran Byte Tipe Data." },
  { "en": "Apa Itu Voltage Source PWL Proteus?", "id": "Sumber Tegangan Piecewise Linear." },
  { "en": "Bagaimana Cara Export Layout Ke SAT?", "id": "Gunakan Perintah Acisout." },
  { "en": "Apa Fungsi Perintah Solidedit Face Material?", "id": "Mengubah Material Permukaan Solid." },
  { "en": "Apa Tipe Data Uint8_t Pada Arduino?", "id": "Integer 8 Bit Tanpa Tanda." },
  { "en": "Komponen Apa Pemicu SBS (Silicon Bilateral)?", "id": "Tegangan Tembus Dua Arah." },
  { "en": "Apa Fungsi Instruksi APR (Arithmetic Process)?", "id": "Kalkulasi Fungsi Trigonometri Kompleks." },
  { "en": "Shortcut Apa Untuk Membuka Action Recorder?", "id": "Ketik Actrecord Lalu Enter." },
  { "en": "Apa Fungsi Keyword Volatile Byte Arduino?", "id": "Byte Berubah Di Dalam Interupsi." },
  { "en": "Apa Itu Resolusi ADC 12 Bit Arduino?", "id": "Nilai 0 Sampai 4095." },
  { "en": "Apa Fungsi Instruksi STC (Set Carry)?", "id": "Set Flag Carry Menjadi 1." },
  { "en": "Bagaimana Cara Paste Ke Layout AutoCAD?", "id": "Gunakan Perintah Chspace." },
  { "en": "Apa Fungsi Keyword Register Pada Arduino?", "id": "Simpan Variabel Di Register CPU." },
  { "en": "Apa Itu Sensor Humidity DHT11 Proteus?", "id": "Sensor Suhu Dan Kelembaban Digital." },
  { "en": "Apa Shortcut Simulation Run GX Works?", "id": "Tekan Shift + Enter." },
  { "en": "Apa Fungsi Perintah Chamfer Face AutoCAD?", "id": "Memotong Sudut Permukaan Solid." },
  { "en": "Apa Fungsi LiquidCrystal ScrollDisplayLeft?", "id": "Geser Konten LCD Ke Kiri." },
  { "en": "Komponen Apa Yang Mencegah Osilasi Parasitik?", "id": "Kapasitor Decoupling Dekat IC." },
  { "en": "Apa Fungsi Zero Flag (Z) Pada PLC?", "id": "Indikator Hasil Operasi Nol." },
  { "en": "Shortcut Apa Untuk Comment Block Arduino?", "id": "Tekan Ctrl + Shift + Slash." },
  { "en": "Apa Fungsi Serial Println Float 4?", "id": "Cetak Float 4 Angka Desimal." },
  { "en": "Apa Itu Signal Generator Audio Proteus?", "id": "Sumber Sinyal Audio File WAV." },
  { "en": "Apa Fungsi Instruksi NOT (Logical Not)?", "id": "Membalik Nilai Bit Input." },
  { "en": "Bagaimana Cara Setting Render Preset?", "id": "Ketik Renderpresets Lalu Enter." },
  { "en": "Apa Fungsi IsSpace Pada Karakter C?", "id": "Cek Spasi Tab Atau Newline." },
  { "en": "Apa Itu I2C Debugger Analyzer Mode?", "id": "Analisis Timing Sinyal I2C." },
  { "en": "Shortcut Apa Untuk Zoom Scale AutoCAD?", "id": "Ketik Z Lalu S." },
  { "en": "Apa Fungsi Perintah Convtosolid AutoCAD?", "id": "Konversi Mesh Ke 3D Solid." },
  { "en": "Apa Fungsi String ToDouble Arduino?", "id": "Konversi String Ke Double Precision." },
  { "en": "Apa Itu IC 74192 Decade Counter?", "id": "Penghitung Naik Turun BCD." },
  { "en": "Apa Fungsi Instruksi EI (Enable Interrupt)?", "id": "Izinkan Eksekusi Interupsi." },
  { "en": "Bagaimana Cara Ukur Volume Massprop?", "id": "Analisis Massa Objek Solid." },
  { "en": "Apa Fungsi Pin ICP1 Pada Arduino?", "id": "Input Capture Timer 1." },
  { "en": "Apa Itu Power Rail VEE Proteus?", "id": "Sumber Tegangan Negatif." },
  { "en": "Shortcut Apa Untuk Toolbar Properties?", "id": "Tekan Ctrl + 1." },
  { "en": "Apa Fungsi Perintah Helix Axis Endpoint?", "id": "Tentukan Sumbu Pusat Spiral." },
  { "en": "Apa Fungsi Karakter Backslash A?", "id": "Bunyi Alarm Atau Beep." },
  { "en": "Apa Itu Regulator LM1117 3.3V?", "id": "Regulator LDO 3.3 Volt." },
  { "en": "Apa Fungsi Analog Reference AREF?", "id": "Referensi Tegangan Dari Luar." },
  { "en": "Bagaimana Cara Atur Tampilan Titik 3D?", "id": "Ubah Variabel Pdmode." },
  { "en": "Apa Fungsi Pin MOSI Arduino Uno?", "id": "Master Out Slave In." },
  { "en": "Apa Itu Zone Mode Cutout Proteus?", "id": "Lubang Pada Area Tembaga." },
  { "en": "Apa Shortcut Show Error Arduino?", "id": "Klik Ikon Error Di Bawah." },
  { "en": "Apa Fungsi Perintah Rotate Copy Reference?", "id": "Putar Salin Dengan Referensi." },
  { "en": "Apa Fungsi Operator Bitwise Not Assignment?", "id": "Inversi Bit Dan Simpan." },
  { "en": "Apa Fungsi Pin SDA Arduino Uno?", "id": "Serial Data Line I2C." },
  { "en": "Apa Itu Resistor Network SIP?", "id": "Resistor Array Single Inline." },
  { "en": "Bagaimana Cara Detach Data Extraction?", "id": "Hapus Link Di Data Manager." },
  { "en": "Apa Fungsi Perintah Dimjogged Text?", "id": "Posisi Teks Dimensi Jog." },
  { "en": "Apa Fungsi Wire Read Buffer?", "id": "Baca Buffer Penerimaan I2C." },
  { "en": "Apa Fungsi IDF Output Proteus?", "id": "Ekspor Ke Software MCAD." },
  { "en": "Apa Fungsi Clock Pulse 1s (P_1s)?", "id": "Bit Berkedip Setiap 1 Detik." },
  { "en": "Bagaimana Cara Insert OLE Package?", "id": "Pilih Package Di Insert Object." },
  { "en": "Apa Fungsi Perintah Subtract Solid Separate?", "id": "Pisahkan Solid Setelah Subtract." },
  { "en": "Apa Fungsi Perintah Rulesurf Pada AutoCAD?", "id": "Membuat Surface Di Antara Dua Kurva." },
  { "en": "Apa Fungsi Perintah Tabsurf Pada AutoCAD?", "id": "Membuat Surface Dari Kurva Dan Vektor Arah." },
  { "en": "Shortcut Apa Untuk Membuka Layer States Manager AutoCAD?", "id": "Ketik Layerstate Lalu Enter." },
  { "en": "Apa Fungsi Variabel Sistem Delobj Pada AutoCAD?", "id": "Mengatur Penghapusan Objek Asal Geometri." },
  { "en": "Apa Fungsi Variabel Sistem Dispsilh Pada AutoCAD?", "id": "Menampilkan Garis Siluet Objek 3D Solid." },
  { "en": "Apa Fungsi Design Rule Check (DRC) Di Proteus?", "id": "Memeriksa Pelanggaran Aturan Desain PCB." },
  { "en": "Apa Fungsi Instruksi ECMP (Floating Point Compare)?", "id": "Membandingkan Dua Nilai Bilangan Desimal." },
  { "en": "Apa Fungsi Instruksi EADD (Floating Point Add)?", "id": "Menjumlahkan Dua Nilai Bilangan Desimal." },
  { "en": "Komponen LM338K Di Proteus Berfungsi Sebagai?", "id": "Regulator Tegangan Adjustable 5 Ampere." },
  { "en": "Apa Fungsi Perintah Edgesurf Pada AutoCAD?", "id": "Membuat Surface Dari Empat Sisi Kurva." },
  { "en": "Apa Fungsi Register PORTC Pada Arduino Uno?", "id": "Output Data Digital Pada Pin A0-A5." },
  { "en": "Bagaimana Cara Mengatur Isolines Pada AutoCAD?", "id": "Ubah Variabel Sistem Isolines." },
  { "en": "Apa Fungsi Tombol Ctrl Shift O Pada AutoCAD?", "id": "Membuka File (Open) Melalui Dialog." },
  { "en": "Apa Fungsi Perintah Revsurf Pada AutoCAD?", "id": "Membuat Surface Dengan Memutar Profil." },
  { "en": "Komponen IC 74HC138 Decoder Demultiplexer?", "id": "Dekoder 3 Input Ke 8 Output Inverting." },
  { "en": "Apa Fungsi Instruksi ESUB (Floating Point Sub)?", "id": "Mengurangkan Dua Nilai Bilangan Desimal." },
  { "en": "Apa Fungsi Instruksi EMUL (Floating Point Mul)?", "id": "Mengalikan Dua Nilai Bilangan Desimal." },
  { "en": "Bagaimana Cara Mengatur Facetratio AutoCAD?", "id": "Ubah Variabel Sistem Facetratio." },
  { "en": "Apa Fungsi Register DDRC Pada Arduino Uno?", "id": "Mengatur Arah Data Pin A0-A5." },
  { "en": "Apa Itu Data Memory File Register (R) PLC?", "id": "Memori Penyimpanan Data File Bank." },
  { "en": "Apa Itu Link Relay (B) Pada PLC Mitsubishi?", "id": "Relay Internal Untuk Link Network." },
  { "en": "Shortcut Apa Untuk Export Sketch Arduino?", "id": "Menu Sketch Export Compiled Binary." },
  { "en": "Apa Fungsi Library EEPROM Put?", "id": "Menulis Data Sembarang Tipe Ke EEPROM." },
  { "en": "Bagaimana Cara Mengatur Layer Pair Proteus?", "id": "Menu Technology Layer Pairs." },
  { "en": "Apa Fungsi Instruksi EDIV (Floating Point Div)?", "id": "Membagi Dua Nilai Bilangan Desimal." },
  { "en": "Apa Fungsi Instruksi SQR (Square Root) PLC?", "id": "Menghitung Akar Kuadrat Bilangan." },
  { "en": "Apa Fungsi Perintah Dimjogline Pada AutoCAD?", "id": "Menambahkan Garis Jog Pada Dimensi." },
  { "en": "Apa Itu Parity Error Pada Komunikasi Serial?", "id": "Jumlah Bit 1 Tidak Sesuai Parity." },
  { "en": "Apa Itu External Interrupt Pada Arduino?", "id": "Interupsi Dari Perubahan Sinyal Pin Luar." },
  { "en": "Shortcut Apa Untuk Find Instruction GX Works?", "id": "Tekan Ctrl + F." },
  { "en": "Apa Fungsi Register PINC Pada Arduino Uno?", "id": "Membaca Input Digital Pin A0-A5." },
  { "en": "Apa Fungsi Fourier Graph Di Proteus?", "id": "Analisis Domain Frekuensi Sinyal." },
  { "en": "Apa Fungsi Timer One Shot (Monostable) PLC?", "id": "Output Aktif Sesaat Saat Trigger." },
  { "en": "Bagaimana Cara Membuat Point Cloud ReCap?", "id": "Gunakan Perintah Pointcloudattach." },
  { "en": "Apa Fungsi Macro bitWrite Pada Arduino?", "id": "Menulis Nilai 0 Atau 1 Ke Bit." },
  { "en": "Apa Itu Component Value Di Proteus?", "id": "Nilai Parameter Komponen (Ohm/Farad)." },
  { "en": "Shortcut Apa Untuk Membuka Visual LISP?", "id": "Ketik Vlide Lalu Enter." },
  { "en": "Apa Fungsi Serial SetTimeout Arduino?", "id": "Mengatur Batas Waktu Baca Data Serial." },
  { "en": "Komponen Apa Yang Menghubungkan Pin VCC?", "id": "Kapasitor Bypass Ke Ground." },
  { "en": "Apa Fungsi Instruksi FLT (Integer To Float)?", "id": "Konversi Bilangan Bulat Ke Desimal." },
  { "en": "Bagaimana Cara Xref Bind Insert?", "id": "Gabungkan Referensi Hapus Nama File." },
  { "en": "Apa Itu PROGMEM String Arduino?", "id": "Simpan String Di Flash Memory." },
  { "en": "Apa Fungsi Pre-Production Checker Proteus?", "id": "Verifikasi Akhir PCB Sebelum Manufaktur." },
  { "en": "Apa Shortcut PLC Read GX Works?", "id": "Tekan Ctrl + T (Read)." },
  { "en": "Apa Fungsi Perintah Sectionplane AutoCAD?", "id": "Membuat Bidang Potongan Objek 3D." },
  { "en": "Apa Fungsi Macro bitRead Pada Arduino?", "id": "Membaca Status Bit Dari Variabel." },
  { "en": "Apa Itu Voltage Source Pulse Proteus?", "id": "Sumber Tegangan Pulsa Periodik." },
  { "en": "Bagaimana Cara Export Layout Ke DXF?", "id": "Menu Output Export DXF." },
  { "en": "Apa Fungsi Perintah Solidedit Face Extrude?", "id": "Menarik Permukaan Solid Menjadi Tebal." },
  { "en": "Apa Tipe Data Float 32 Bit Arduino?", "id": "Bilangan Desimal Presisi Tunggal." },
  { "en": "Komponen Apa Pemicu UJT (Unijunction)?", "id": "Tegangan Puncak Emitter Tercapai." },
  { "en": "Apa Fungsi Instruksi NEG (Negate) PLC?", "id": "Mengubah Tanda Positif Negatif Nilai." },
  { "en": "Shortcut Apa Untuk Membuka Material Browser?", "id": "Ketik Matbrowseropen Lalu Enter." },
  { "en": "Apa Fungsi Keyword Volatile Pada Interupsi?", "id": "Memastikan Variabel Dibaca Dari RAM." },
  { "en": "Apa Itu Resolusi ADC 10 Bit Arduino?", "id": "Nilai Digital 0 Sampai 1023." },
  { "en": "Apa Fungsi Instruksi CLC (Clear Carry)?", "id": "Mereset Status Flag Carry." },
  { "en": "Bagaimana Cara Paste As Block AutoCAD?", "id": "Tekan Ctrl + Shift + V." },
  { "en": "Apa Fungsi Register ADMUX Pada AVR?", "id": "Memilih Channel Input ADC." },
  { "en": "Apa Itu Sensor Pressure MPX4115 Proteus?", "id": "Sensor Tekanan Udara Analog." },
  { "en": "Apa Shortcut Simulation Pause GX Works?", "id": "Tekan Ctrl + F10." },
  { "en": "Apa Fungsi Perintah Livesection AutoCAD?", "id": "Mengaktifkan Potongan Langsung Sectionplane." },
  { "en": "Apa Fungsi Macro bitSet Pada Arduino?", "id": "Set Bit Menjadi 1." },
  { "en": "Komponen Apa Yang Menghubungkan Osilator?", "id": "Kapasitor Keramik Ke Ground." },
  { "en": "Apa Fungsi Overflow Flag (V) PLC?", "id": "Indikator Hasil Melebihi Kapasitas Register." },
  { "en": "Shortcut Apa Untuk Uncomment Code Arduino?", "id": "Tekan Ctrl + Slash." },
  { "en": "Apa Fungsi Serial Println BIN?", "id": "Cetak Nilai Dalam Format Biner." },
  { "en": "Apa Itu Signal Generator FM Proteus?", "id": "Pembangkit Sinyal Modulasi Frekuensi." },
  { "en": "Apa Fungsi Instruksi WAND (Word And)?", "id": "Logika AND Tingkat Word." },
  { "en": "Bagaimana Cara Setting Render Exposure?", "id": "Ketik Renderexposure Lalu Enter." },
  { "en": "Apa Fungsi IsAlpha Pada Karakter Arduino?", "id": "Cek Apakah Karakter Huruf." },
  { "en": "Apa Itu SPI Debugger Proteus?", "id": "Analisis Protokol Serial Peripheral Interface." },
  { "en": "Shortcut Apa Untuk Zoom Extents AutoCAD?", "id": "Klik Dua Kali Roda Mouse." },
  { "en": "Apa Fungsi Perintah Convtonurbs AutoCAD?", "id": "Konversi Solid Atau Mesh Ke NURBS." },
  { "en": "Apa Fungsi String ToInt Arduino?", "id": "Konversi String Angka Ke Integer." },
  { "en": "Apa Itu IC 74HC573 Latch?", "id": "Octal Transparent D-Type Latch." },
  { "en": "Apa Fungsi Instruksi EI (Enable Interrupt)?", "id": "Mengaktifkan Interupsi Global." },
  { "en": "Bagaimana Cara Ukur Sudut Di AutoCAD?", "id": "Gunakan Perintah Dimangular." },
  { "en": "Apa Fungsi Register TCCR1A Pada AVR?", "id": "Mengatur Kontrol Timer Counter 1." },
  { "en": "Apa Itu Power Terminal Ground Proteus?", "id": "Titik Referensi Nol Volt." },
  { "en": "Shortcut Apa Untuk Hide Palettes AutoCAD?", "id": "Tekan Ctrl + Shift + H." },
  { "en": "Apa Fungsi Perintah Helix Height?", "id": "Menentukan Tinggi Total Spiral." },
  { "en": "Apa Fungsi Karakter Backslash N?", "id": "Newline Ganti Baris Baru." },
  { "en": "Apa Itu Regulator AMS1117?", "id": "Regulator LDO 3.3V Atau 5V." },
  { "en": "Apa Fungsi Analog Reference Default?", "id": "Tegangan Referensi Sesuai VCC." },
  { "en": "Bagaimana Cara Atur Tampilan Point Style?", "id": "Gunakan Perintah Ptype." },
  { "en": "Apa Fungsi Register ADCSRA Pada AVR?", "id": "Mengatur Status Dan Kontrol ADC." },
  { "en": "Apa Itu Ratsnest Mode Proteus?", "id": "Tampilkan Garis Koneksi Udara." },
  { "en": "Apa Shortcut Verify Code Arduino?", "id": "Tekan Ctrl + R." },
  { "en": "Apa Fungsi Perintah Scale Copy?", "id": "Skala Objek Sambil Menyalin." },
  { "en": "Apa Fungsi Macro bitClear Pada Arduino?", "id": "Set Bit Menjadi 0." },
  { "en": "Apa Fungsi Pin AREF Arduino?", "id": "Input Tegangan Referensi ADC." },
  { "en": "Apa Itu Resistor SIP Proteus?", "id": "Resistor Array Single Inline." },
  { "en": "Bagaimana Cara Update Field AutoCAD?", "id": "Gunakan Perintah Updatefield." },
  { "en": "Apa Fungsi Perintah Dimjogged Radius?", "id": "Dimensi Radius Pusat Jauh." },
  { "en": "Apa Fungsi Wire RequestFrom Arduino?", "id": "Minta Data Dari Slave I2C." },
  { "en": "Apa Fungsi Pick And Place File Proteus?", "id": "Data Koordinat Pemasangan Komponen." },
  { "en": "Apa Fungsi Carry Flag (C) PLC?", "id": "Indikator Sisa Hasil Operasi." },
  { "en": "Bagaimana Cara Insert OLE Object?", "id": "Gunakan Perintah Insertobj." },
  { "en": "Apa Fungsi Perintah Union Region?", "id": "Menggabungkan Dua Area 2D." },
  { "en": "Apa Fungsi Perintah Mview Pada AutoCAD Layout?", "id": "Membuat Viewport Baru Di Paper Space." },
  { "en": "Apa Fungsi Perintah Vplayer Pada AutoCAD?", "id": "Mengatur Visibilitas Layer Per Viewport." },
  { "en": "Shortcut Apa Untuk Membuka Page Setup Manager AutoCAD?", "id": "Klik Kanan Tab Layout Page Setup." },
  { "en": "Apa Fungsi Variabel Sistem Pstylemode Pada AutoCAD?", "id": "Cek Mode Plot Style Color Dependent Atau Named." },
  { "en": "Apa Fungsi Variabel Sistem Tilemode Pada AutoCAD?", "id": "Pindah Antara Model Space Dan Layout." },
  { "en": "Apa Fungsi Netlist Compiler Di Proteus?", "id": "Menerjemahkan Skematik Menjadi Data Netlist." },
  { "en": "Apa Fungsi Instruksi MC (Master Control) PLC?", "id": "Memulai Blok Logika Master Control." },
  { "en": "Apa Fungsi Instruksi MCR (Master Control Reset) PLC?", "id": "Mengakhiri Blok Logika Master Control." },
  { "en": "Komponen 74HC595 Pin ST_CP Berfungsi Sebagai?", "id": "Storage Register Clock Latch Data." },
  { "en": "Apa Fungsi Perintah Viewbase Pada AutoCAD?", "id": "Membuat Gambar Proyeksi 2D Dari Model 3D." },
  { "en": "Apa Fungsi Register TCNT1 Pada Arduino AVR?", "id": "Menyimpan Nilai Hitungan Timer 1 (16 Bit)." },
  { "en": "Bagaimana Cara Mengatur PSLTSCALE AutoCAD?", "id": "Ubah Variabel Psltscale Ke 0 Atau 1." },
  { "en": "Apa Fungsi Tombol Ctrl Shift V AutoCAD?", "id": "Paste Sebagai Blok (Paste As Block)." },
  { "en": "Apa Fungsi Perintah Solview Pada AutoCAD?", "id": "Membuat Viewport Ortografis Untuk 3D Solid." },
  { "en": "Komponen IC 555 Pin 3 Berfungsi Sebagai?", "id": "Output Sinyal Pulsa." },
  { "en": "Apa Fungsi Instruksi CJ (Conditional Jump) PLC?", "id": "Lompat Ke Pointer Jika Kondisi On." },
  { "en": "Apa Fungsi Instruksi JMP (Jump) Pada PLC?", "id": "Lompat Melewati Bagian Program Tertentu." },
  { "en": "Bagaimana Cara Mengatur Lwdisplay AutoCAD?", "id": "Toggle Tampilan Ketebalan Garis (Lineweight)." },
  { "en": "Apa Fungsi Register OCR1A Pada Arduino AVR?", "id": "Nilai Pembanding Output Compare Timer 1." },
  { "en": "Apa Itu Data Memory Index (IR) PLC Omron?", "id": "Register Index Untuk Pointer Alamat." },
  { "en": "Apa Itu Keep Relay (H) Pada PLC?", "id": "Bit Penahan Status Saat Power Off." },
  { "en": "Shortcut Apa Untuk Export Image Proteus?", "id": "Menu File Export Graphics." },
  { "en": "Apa Fungsi Library SPI SetDataMode SPI_MODE0?", "id": "Clock Idle Low Sample Rising Edge." },
  { "en": "Bagaimana Cara Mengatur Component Value Proteus?", "id": "Double Klik Komponen Edit Value." },
  { "en": "Apa Fungsi Instruksi BCD (Binary Coded Decimal)?", "id": "Konversi Data Biner Ke Format BCD." },
  { "en": "Apa Fungsi Instruksi BIN (Binary) Pada PLC?", "id": "Konversi Data BCD Ke Format Biner." },
  { "en": "Apa Fungsi Perintah Dimjogline Pada AutoCAD?", "id": "Menambahkan Garis Tekuk Pada Dimensi Linear." },
  { "en": "Apa Itu Framing Error UART Arduino?", "id": "Stop Bit Tidak Terbaca Dengan Benar." },
  { "en": "Apa Itu Watchdog Timer Reset?", "id": "Restart Sistem Karena Waktu Habis." },
  { "en": "Shortcut Apa Untuk Monitoring Mode GX Works?", "id": "Tekan Tombol F3." },
  { "en": "Apa Fungsi Register UBRR Pada Arduino AVR?", "id": "Mengatur Baud Rate Komunikasi Serial." },
  { "en": "Apa Fungsi I2C Protocol Analyser Proteus?", "id": "Menganalisis Paket Data Bus I2C." },
  { "en": "Apa Fungsi Timer High Speed (HSC) PLC?", "id": "Menghitung Input Pulsa Kecepatan Tinggi." },
  { "en": "Bagaimana Cara Membuat Slide AutoCAD?", "id": "Gunakan Perintah Mslide." },
  { "en": "Apa Fungsi Operator Bitwise Shift Left (<<)?", "id": "Geser Bit Ke Kiri (Kali 2)." },
  { "en": "Apa Itu Junction Dot Di Proteus?", "id": "Titik Pertemuan Sambungan Kawat." },
  { "en": "Shortcut Apa Untuk Membuka Visual Styles?", "id": "Ketik Visualstyles Lalu Enter." },
  { "en": "Apa Fungsi Serial Peek Pada Arduino?", "id": "Lihat Byte Data Tanpa Menghapusnya." },
  { "en": "Komponen Apa Yang Menghubungkan Pin VCC?", "id": "Kapasitor Decoupling Ke Ground." },
  { "en": "Apa Fungsi Instruksi SFT (Shift Register) Omron?", "id": "Geser Bit Data Serial." },
  { "en": "Bagaimana Cara Xref Bind Insert Style?", "id": "Gabung Xref Hapus Nama File Induk." },
  { "en": "Apa Itu PROGMEM Array Arduino?", "id": "Simpan Array Data Di Flash Memory." },
  { "en": "Apa Fungsi ERC (Electrical Rule Check) Proteus?", "id": "Cek Logika Kelistrikan Skematik." },
  { "en": "Apa Shortcut PLC Write Mode GX Works?", "id": "Tekan Tombol F2." },
  { "en": "Apa Fungsi Perintah Flatshot AutoCAD?", "id": "Proyeksi 2D Dari Tampilan 3D Saat Ini." },
  { "en": "Apa Fungsi Macro bitSet Pada Arduino?", "id": "Set Bit Tertentu Menjadi 1." },
  { "en": "Apa Itu Current Source DC Proteus?", "id": "Sumber Arus Konstan DC." },
  { "en": "Bagaimana Cara Export Layout Ke PDF?", "id": "Menu Output Export PDF." },
  { "en": "Apa Fungsi Perintah Solidedit Body Shell?", "id": "Membuat Dinding Tipis Pada Solid." },
  { "en": "Apa Tipe Data Double Pada Arduino Uno?", "id": "Sama Dengan Float (32 Bit)." },
  { "en": "Komponen Apa Pemicu Thyristor?", "id": "Arus Gate." },
  { "en": "Apa Fungsi Instruksi COM (Complement) PLC?", "id": "Inversi Logika Semua Bit Word." },
  { "en": "Shortcut Apa Untuk Membuka Material Browser?", "id": "Ketik Matbrowseropen Lalu Enter." },
  { "en": "Apa Fungsi Keyword Volatile Arduino?", "id": "Cegah Cache Variabel Di Register." },
  { "en": "Apa Itu Resolusi PWM 8 Bit?", "id": "Nilai Duty Cycle 0 Sampai 255." },
  { "en": "Apa Fungsi Instruksi CLC (Clear Carry)?", "id": "Reset Flag Carry Menjadi 0." },
  { "en": "Bagaimana Cara Paste Ke Koordinat Asli?", "id": "Menu Edit Paste To Original Coordinates." },
  { "en": "Apa Fungsi Register UDRE Pada UART?", "id": "Indikator Buffer Data Siap Diisi." },
  { "en": "Apa Itu Sensor Temperature LM35 Proteus?", "id": "Sensor Suhu Analog Linear." },
  { "en": "Apa Shortcut Simulation Step GX Works?", "id": "Tekan Shift + F10." },
  { "en": "Apa Fungsi Perintah Solprof AutoCAD?", "id": "Buat Profil 2D Di Layout Viewport." },
  { "en": "Apa Fungsi Macro bitClear Pada Arduino?", "id": "Set Bit Tertentu Menjadi 0." },
  { "en": "Komponen Apa Yang Mengatur Frekuensi?", "id": "Kristal Kuarsa Dan Kapasitor." },
  { "en": "Apa Fungsi Carry Flag (C) PLC?", "id": "Indikator Sisa Operasi Aritmatika." },
  { "en": "Shortcut Apa Untuk Uncomment Code?", "id": "Tekan Ctrl + Slash." },
  { "en": "Apa Fungsi Serial Println BIN?", "id": "Cetak Nilai Format Biner." },
  { "en": "Apa Itu Signal Generator Sine Proteus?", "id": "Sumber Sinyal Gelombang Sinus." },
  { "en": "Apa Fungsi Instruksi ORW (Logic OR Word)?", "id": "Logika OR Tingkat 16 Bit." },
  { "en": "Bagaimana Cara Setting Render Quality?", "id": "Ubah Preset Di Render Window." },
  { "en": "Apa Fungsi IsDigit Pada Karakter Arduino?", "id": "Cek Apakah Karakter Adalah Angka." },
  { "en": "Apa Itu SPI Protocol Analyser Proteus?", "id": "Menganalisis Paket Data Bus SPI." },
  { "en": "Shortcut Apa Untuk Zoom Previous AutoCAD?", "id": "Ketik Z Lalu P." },
  { "en": "Apa Fungsi Perintah Soldraw AutoCAD?", "id": "Gambar Profil Dan Potongan Solview." },
  { "en": "Apa Fungsi String ToInt Base 2?", "id": "Konversi String Biner Ke Integer." },
  { "en": "Apa Itu IC 4017 Decade Counter?", "id": "Penghitung Desimal Output Berurutan." },
  { "en": "Apa Fungsi Instruksi DI (Disable Interrupt)?", "id": "Nonaktifkan Semua Interupsi Global." },
  { "en": "Bagaimana Cara Ukur Luas Area Tertutup?", "id": "Gunakan Perintah Area Object." },
  { "en": "Apa Fungsi Pin AREF Arduino?", "id": "Tegangan Referensi Eksternal ADC." },
  { "en": "Apa Itu Power Rail VDD Proteus?", "id": "Sumber Tegangan Positif IC CMOS." },
  { "en": "Shortcut Apa Untuk Clean Screen AutoCAD?", "id": "Tekan Ctrl + 0." },
  { "en": "Apa Fungsi Perintah Helix Base Radius?", "id": "Atur Radius Bawah Spiral." },
  { "en": "Apa Fungsi Karakter Escape Backslash T?", "id": "Tabulasi Horizontal." },
  { "en": "Apa Itu Regulator 78L05?", "id": "Regulator 5V Arus Kecil (Low Power)." },
  { "en": "Apa Fungsi Analog Reference Internal 2.56V?", "id": "Referensi ADC 2.56V (Arduino Mega)." },
  { "en": "Bagaimana Cara Atur Tampilan Titik?", "id": "Ketik Ptype Lalu Enter." },
  { "en": "Apa Fungsi Pin SCK Arduino Uno?", "id": "Serial Clock Bus SPI." },
  { "en": "Apa Itu Thermal Relief Pad Proteus?", "id": "Pad Dengan Jari Jari Isolasi Panas." },
  { "en": "Apa Shortcut Verify Code Arduino?", "id": "Tekan Ctrl + R." },
  { "en": "Apa Fungsi Perintah Scale Reference?", "id": "Skala Objek Dengan Panjang Acuan." },
  { "en": "Apa Fungsi Operator Bitwise XOR (^)?", "id": "Logika Exclusive OR Per Bit." },
  { "en": "Apa Fungsi Pin SDA Arduino Uno?", "id": "Serial Data Line Bus I2C." },
  { "en": "Apa Itu Resistor DIL Network?", "id": "Paket Resistor Dual In Line." },
  { "en": "Bagaimana Cara Update Data Link?", "id": "Klik Kanan Data Link Update." },
  { "en": "Apa Fungsi Perintah Dimjogged Angle?", "id": "Dimensi Sudut Dengan Titik Pusat Jauh." },
  { "en": "Apa Fungsi Wire BeginTransmission?", "id": "Mulai Transaksi Ke Alamat I2C." },
  { "en": "Apa Fungsi Gerber Output Proteus?", "id": "File Data Produksi PCB." },
  { "en": "Apa Fungsi Flag P_On (Always ON)?", "id": "Bit Yang Selalu Bernilai 1." },
  { "en": "Bagaimana Cara Insert OLE Object?", "id": "Ketik Insertobj Lalu Enter." },
  { "en": "Apa Fungsi Perintah Subtract Solid?", "id": "Memotong Volume Objek 3D." },
  { "en": "Apa Fungsi Perintah Surfpatch Pada AutoCAD?", "id": "Menutup Lubang Surface Dengan Tambalan." },
  { "en": "Apa Fungsi Perintah Surfblend Pada AutoCAD?", "id": "Membuat Surface Peralihan Antar Dua Surface." },
  { "en": "Shortcut Apa Untuk Membuka External References Palette?", "id": "Ketik Xref Lalu Enter." },
  { "en": "Apa Fungsi Variabel Sistem Gtdefault Pada AutoCAD?", "id": "Mengatur Tampilan Grip Tips." },
  { "en": "Apa Fungsi Variabel Sistem Hpdlines Pada AutoCAD?", "id": "Mengatur Garis Tampilan Hatch Pattern." },
  { "en": "Apa Fungsi Autoplacer Di Proteus ARES?", "id": "Menempatkan Komponen PCB Secara Otomatis." },
  { "en": "Apa Fungsi Instruksi SCL3 (Scaling 3) PLC?", "id": "Konversi Skala Linear Dengan Titik." },
  { "en": "Apa Fungsi Instruksi WSFT (Word Shift) PLC?", "id": "Menggeser Data Word Kiri Kanan." },
  { "en": "Komponen TL074 Di Proteus Berfungsi Sebagai?", "id": "Quad Low Noise JFET Op Amp." },
  { "en": "Apa Fungsi Perintah Surfoffset Pada AutoCAD?", "id": "Membuat Surface Paralel Dengan Jarak." },
  { "en": "Apa Fungsi Register UCSR0A Pada Arduino AVR?", "id": "Status Kontrol Transmisi USART A." },
  { "en": "Bagaimana Cara Mengatur Cameradisplay AutoCAD?", "id": "Ubah Variabel Sistem Cameradisplay." },
  { "en": "Apa Fungsi Tombol Ctrl Shift L Pada AutoCAD?", "id": "Memilih Kembali Objek Sebelumnya (Previous)." },
  { "en": "Apa Fungsi Perintah Surffillet Pada AutoCAD?", "id": "Membuat Fillet Antara Dua Surface." },
  { "en": "Komponen IC 74HC4060 Counter Oscillator?", "id": "Penghitung Biner Dengan Osilator Internal." },
  { "en": "Apa Fungsi Instruksi SWAP (Swap Bytes) PLC?", "id": "Menukar Byte Atas Dan Bawah." },
  { "en": "Apa Fungsi Instruksi NEG (Two's Complement) PLC?", "id": "Mengubah Nilai Positif Jadi Negatif." },
  { "en": "Bagaimana Cara Mengatur Lightglyphdisplay AutoCAD?", "id": "Ubah Variabel Sistem Lightglyphdisplay." },
  { "en": "Apa Fungsi Register UCSR0B Pada Arduino AVR?", "id": "Mengaktifkan Transmisi Penerimaan USART." },
  { "en": "Apa Itu Data Memory Cascade (C) PLC?", "id": "Area Counter Kecepatan Tinggi." },
  { "en": "Apa Itu Link Register (W) Pada PLC?", "id": "Register Data Link Area Luas." },
  { "en": "Shortcut Apa Untuk Burn Bootloader Arduino?", "id": "Menu Tools Burn Bootloader." },
  { "en": "Apa Fungsi Library Servo Attach?", "id": "Menghubungkan Variabel Servo Ke Pin." },
  { "en": "Bagaimana Cara Mengatur Grid Snap Proteus?", "id": "Tekan F2 F3 Atau F4." },
  { "en": "Apa Fungsi Instruksi ASLL (Double Shift Left)?", "id": "Geser Kiri Data 32 Bit." },
  { "en": "Apa Fungsi Instruksi ASRL (Double Shift Right)?", "id": "Geser Kanan Data 32 Bit." },
  { "en": "Apa Fungsi Perintah Dimaligned Pada AutoCAD?", "id": "Dimensi Sejajar Dengan Objek Miring." },
  { "en": "Apa Itu Noise Margin High (VNH)?", "id": "Toleransi Tegangan Logika Tinggi." },
  { "en": "Apa Itu Brown Out Reset (BOR) Arduino?", "id": "Reset Saat Tegangan Turun Drastis." },
  { "en": "Shortcut Apa Untuk Find Replace GX Works?", "id": "Tekan Ctrl + F." },
  { "en": "Apa Fungsi Register SPCR Pada Arduino AVR?", "id": "Mengatur Kontrol Protokol SPI." },
  { "en": "Apa Fungsi Reflection Graph Di Proteus?", "id": "Analisis Pantulan Sinyal Transmisi." },
  { "en": "Apa Fungsi Timer Pulse Extend (T) PLC?", "id": "Memperpanjang Durasi Pulsa Trigger." },
  { "en": "Bagaimana Cara Membuat Camera AutoCAD?", "id": "Gunakan Perintah Camera." },
  { "en": "Apa Fungsi Operator Bitwise NOT (~)?", "id": "Membalik Nilai Bit (Invert)." },
  { "en": "Apa Itu Sub-Circuit Mode Di Proteus?", "id": "Membuat Blok Rangkaian Bertingkat." },
  { "en": "Shortcut Apa Untuk Membuka Layer Manager?", "id": "Ketik Layer Lalu Enter." },
  { "en": "Apa Fungsi Serial Flush Arduino?", "id": "Menunggu Pengiriman Data Selesai." },
  { "en": "Komponen Apa Yang Menghubungkan Pin Crystal?", "id": "Kapasitor Keramik Ke Ground." },
  { "en": "Apa Fungsi Instruksi FLT (Floating Point)?", "id": "Konversi Integer Ke Desimal." },
  { "en": "Bagaimana Cara Xref Bind Insert?", "id": "Gabung Xref Hapus Nama File." },
  { "en": "Apa Itu EEPROM Length Arduino?", "id": "Mendapatkan Ukuran Total EEPROM." },
  { "en": "Apa Fungsi Electrical Rule Check Proteus?", "id": "Memeriksa Kesalahan Koneksi Pin." },
  { "en": "Apa Shortcut PLC Upload GX Works?", "id": "Tekan Ctrl + T (Write)." },
  { "en": "Apa Fungsi Perintah Surftrim Pada AutoCAD?", "id": "Memotong Surface Menggunakan Surface Lain." },
  { "en": "Apa Fungsi Macro highByte Pada Arduino?", "id": "Mengambil 8 Bit Tertinggi Data." },
  { "en": "Apa Itu Voltage Source Sine Proteus?", "id": "Sumber Tegangan Gelombang Sinus." },
  { "en": "Bagaimana Cara Export Layout Ke BMP?", "id": "Menu Output Export Bitmap." },
  { "en": "Apa Fungsi Perintah Solidedit Face Color?", "id": "Mengubah Warna Permukaan Solid." },
  { "en": "Apa Tipe Data Unsigned Int Arduino?", "id": "Integer Positif 16 Bit." },
  { "en": "Komponen Apa Pemicu DIAC?", "id": "Tegangan Breakdown Tercapai." },
  { "en": "Apa Fungsi Instruksi PID (Control Loop)?", "id": "Kontrol Proporsional Integral Derivatif." },
  { "en": "Shortcut Apa Untuk Membuka Design Center?", "id": "Tekan Ctrl + 2." },
  { "en": "Apa Fungsi Keyword Static Pada Fungsi?", "id": "Variabel Tetap Ada Antar Panggilan." },
  { "en": "Apa Itu Resolusi PWM 8 Bit?", "id": "Nilai 0 Sampai 255." },
  { "en": "Apa Fungsi Instruksi CLC (Clear Carry)?", "id": "Hapus Flag Carry Menjadi 0." },
  { "en": "Bagaimana Cara Paste Ke Koordinat Asli?", "id": "Pilih Paste To Original Coordinates." },
  { "en": "Apa Fungsi Register SREG Pada Arduino AVR?", "id": "Status Register (Flag Global)." },
  { "en": "Apa Itu Sensor Light LDR Proteus?", "id": "Resistor Berubah Sesuai Cahaya." },
  { "en": "Apa Shortcut Simulation Stop GX Works?", "id": "Tekan Alt + F4." },
  { "en": "Apa Fungsi Perintah Surfextend Pada AutoCAD?", "id": "Memperpanjang Tepi Surface." },
  { "en": "Apa Fungsi Macro lowByte Pada Arduino?", "id": "Mengambil 8 Bit Terendah Data." },
  { "en": "Komponen Apa Yang Menghilangkan Noise DC?", "id": "Kapasitor Filter Power Supply." },
  { "en": "Apa Fungsi Negative Flag (N) PLC?", "id": "Indikator Hasil Operasi Negatif." },
  { "en": "Shortcut Apa Untuk Auto Format Code?", "id": "Tekan Ctrl + T." },
  { "en": "Apa Fungsi Serial Println OCT?", "id": "Cetak Nilai Format Oktal." },
  { "en": "Apa Itu Signal Generator PWL Proteus?", "id": "Pembangkit Sinyal Piecewise Linear." },
  { "en": "Apa Fungsi Instruksi WOR (Word OR)?", "id": "Logika OR Tingkat Word." },
  { "en": "Bagaimana Cara Setting Render Size?", "id": "Ubah Output Size Di Render." },
  { "en": "Apa Fungsi IsAlphaNumeric Arduino?", "id": "Cek Karakter Huruf Atau Angka." },
  { "en": "Apa Itu I2C Debugger Spy Mode?", "id": "Intip Data Bus Tanpa Ganggu." },
  { "en": "Shortcut Apa Untuk Zoom All AutoCAD?", "id": "Ketik Z Lalu A." },
  { "en": "Apa Fungsi Perintah Surfsculpt AutoCAD?", "id": "Memotong Surface Dengan Volume." },
  { "en": "Apa Fungsi String ToFloat Arduino?", "id": "Konversi String Ke Desimal Float." },
  { "en": "Apa Itu IC 74HC164 Shift Register?", "id": "Serial In Parallel Out Register." },
  { "en": "Apa Fungsi Instruksi EI (Enable Interrupt)?", "id": "Mengaktifkan Interupsi Global." },
  { "en": "Bagaimana Cara Ukur Volume Solid?", "id": "Gunakan Perintah Massprop." },
  { "en": "Apa Fungsi Pin OC1A Pada Arduino?", "id": "Output PWM Timer 1 Channel A." },
  { "en": "Apa Itu Power Terminal VDD Proteus?", "id": "Sumber Tegangan Positif CMOS." },
  { "en": "Shortcut Apa Untuk Command Line?", "id": "Tekan Ctrl + 9." },
  { "en": "Apa Fungsi Perintah Helix Turns?", "id": "Menentukan Jumlah Putaran Spiral." },
  { "en": "Apa Fungsi Karakter Backslash R?", "id": "Carriage Return." },
  { "en": "Apa Itu Regulator LM7812?", "id": "Regulator 12 Volt Positif." },
  { "en": "Apa Fungsi Analog Reference External?", "id": "Referensi ADC Dari Pin AREF." },
  { "en": "Bagaimana Cara Atur Tampilan Titik?", "id": "Ketik Ptype Lalu Enter." },
  { "en": "Apa Fungsi Register ACSR Pada AVR?", "id": "Kontrol Analog Comparator." },
  { "en": "Apa Itu Pad Mode Di Proteus ARES?", "id": "Menempatkan Kaki Komponen Manual." },
  { "en": "Apa Shortcut Verify Sketch?", "id": "Tekan Ctrl + R." },
  { "en": "Apa Fungsi Perintah Scale Reference?", "id": "Skala Objek Dengan Panjang Acuan." },
  { "en": "Apa Fungsi Operator Bitwise OR (|)?", "id": "Logika OR Per Bit." },
  { "en": "Apa Fungsi Pin MISO Arduino Uno?", "id": "Master In Slave Out SPI." },
  { "en": "Apa Itu Resistor SIL Network?", "id": "Resistor Array Single Inline." },
  { "en": "Bagaimana Cara Update Data Extraction?", "id": "Klik Kanan Update Data Extraction." },
  { "en": "Apa Fungsi Perintah Dimjogged Angle?", "id": "Dimensi Sudut Pusat Jauh." },
  { "en": "Apa Fungsi Wire Available Arduino?", "id": "Cek Jumlah Byte Di Buffer." },
  { "en": "Apa Fungsi Project Notes Proteus?", "id": "Catatan Dokumentasi Proyek." },
  { "en": "Apa Fungsi Flag P_First_Cycle?", "id": "Bit Aktif Hanya Saat Awal Run." },
  { "en": "Bagaimana Cara Insert OLE Link?", "id": "Pilih Link Di Paste Special." },
  { "en": "Apa Fungsi Perintah Union Solid?", "id": "Menggabungkan Objek Solid." },
  { "en": "Apa Fungsi Perintah Meshsmooth Pada AutoCAD?", "id": "Mengubah Objek 3D Menjadi Mesh Halus." },
  { "en": "Apa Fungsi Perintah Meshrefine Pada AutoCAD?", "id": "Menambah Jumlah Wajah Pada Objek Mesh." },
  { "en": "Shortcut Apa Untuk Membuka Visual Basic Editor?", "id": "Tekan Alt + F11." },
  { "en": "Apa Fungsi Variabel Sistem Smoothmeshconvert?", "id": "Mengatur Mode Konversi Mesh Ke Solid." },
  { "en": "Apa Fungsi Variabel Sistem Facetres Pada AutoCAD?", "id": "Mengatur Kepadatan Mesh Saat Render." },
  { "en": "Apa Fungsi Autorouter Di Proteus ARES?", "id": "Membuat Jalur PCB Secara Otomatis." },
  { "en": "Apa Fungsi Instruksi BSET (Block Set) PLC?", "id": "Mengisi Blok Memori Dengan Nilai Sama." },
  { "en": "Apa Fungsi Instruksi FMOV (Fill Move) PLC?", "id": "Menyalin Konstanta Ke Banyak Alamat." },
  { "en": "Komponen LM317L Di Proteus Berfungsi Sebagai?", "id": "Regulator Adjustable Arus Rendah (100mA)." },
  { "en": "Apa Fungsi Perintah Meshsplit Pada AutoCAD?", "id": "Membagi Wajah Mesh Menjadi Dua." },
  { "en": "Apa Fungsi Register EIMSK Pada Arduino AVR?", "id": "Mengaktifkan Masker Interupsi Eksternal." },
  { "en": "Bagaimana Cara Mengatur Xrefnotify AutoCAD?", "id": "Ubah Variabel Sistem Xrefnotify." },
  { "en": "Apa Fungsi Tombol Ctrl Shift U Pada AutoCAD?", "id": "Toggle Polar Tracking On Off." },
  { "en": "Apa Fungsi Perintah Meshmerge Pada AutoCAD?", "id": "Menggabungkan Wajah Mesh Yang Berdekatan." },
  { "en": "Komponen IC 74HC595 Output Storage Register?", "id": "Menyimpan Data Untuk Ditampilkan Di Pin." },
  { "en": "Apa Fungsi Instruksi XCHG (Exchange) PLC?", "id": "Menukar Isi Dua Register Data." },
  { "en": "Apa Fungsi Instruksi SWAP (Swap Bytes) PLC?", "id": "Menukar Byte Tinggi Dan Rendah." },
  { "en": "Bagaimana Cara Mengatur Layereval AutoCAD?", "id": "Ubah Variabel Sistem Layereval." },
  { "en": "Apa Fungsi Register EICRA Pada Arduino AVR?", "id": "Mengatur Pemicu Interupsi Eksternal (Edge)." },
  { "en": "Apa Itu Data Memory Index (IR) PLC?", "id": "Register Index Pointer Alamat Memori." },
  { "en": "Apa Itu Special Auxiliary Relay (M8000)?", "id": "Relay Run Monitor (Selalu ON)." },
  { "en": "Shortcut Apa Untuk Export Compiled Binary?", "id": "Menu Sketch Export Compiled Binary." },
  { "en": "Apa Fungsi Library Wire EndTransmission?", "id": "Mengakhiri Transmisi Data I2C." },
  { "en": "Bagaimana Cara Mengatur Grid Dots Proteus?", "id": "Menu View Grid Dots." },
  { "en": "Apa Fungsi Instruksi ROL (Rotate Left) PLC?", "id": "Putar Bit Register Ke Kiri." },
  { "en": "Apa Fungsi Instruksi ROR (Rotate Right) PLC?", "id": "Putar Bit Register Ke Kanan." },
  { "en": "Apa Fungsi Perintah Dimbaseline Pada AutoCAD?", "id": "Dimensi Beruntun Dari Satu Titik Basis." },
  { "en": "Apa Itu Noise Margin Low (VNL)?", "id": "Toleransi Tegangan Logika Rendah." },
  { "en": "Apa Itu Watchdog Interrupt Arduino?", "id": "Interupsi Sebelum Reset WDT Terjadi." },
  { "en": "Shortcut Apa Untuk Monitoring Mode GX Works?", "id": "Tekan Tombol F3." },
  { "en": "Apa Fungsi Register WDTCSR Pada AVR?", "id": "Register Kontrol Timer Watchdog." },
  { "en": "Apa Fungsi Fourier Graph Proteus?", "id": "Analisis Spektrum Frekuensi Sinyal." },
  { "en": "Apa Fungsi Timer High Speed Counter (HSC)?", "id": "Menghitung Input Pulsa Cepat Encoder." },
  { "en": "Bagaimana Cara Membuat Point Cloud AutoCAD?", "id": "Gunakan Perintah Pointcloudattach." },
  { "en": "Apa Fungsi Operator Bitwise Shift Right (>>)?", "id": "Geser Bit Ke Kanan (Bagi 2)." },
  { "en": "Apa Itu Sub-Circuit Port Di Proteus?", "id": "Terminal Input Output Sub-Rangkaian." },
  { "en": "Shortcut Apa Untuk Membuka Visual Styles?", "id": "Ketik Visualstyles Lalu Enter." },
  { "en": "Apa Fungsi Serial Available Arduino?", "id": "Cek Jumlah Byte Di Buffer Serial." },
  { "en": "Komponen Apa Yang Menghubungkan Pin Reset?", "id": "Resistor Pull-Up Dan Tombol Reset." },
  { "en": "Apa Fungsi Instruksi DIST (Data Distribute)?", "id": "Mendistribusikan Data Ke Alamat Stack." },
  { "en": "Bagaimana Cara Xref Bind Insert?", "id": "Gabung Xref Hapus Nama File Induk." },
  { "en": "Apa Itu EEPROM Read Arduino?", "id": "Membaca Satu Byte Dari EEPROM." },
  { "en": "Apa Fungsi Electrical Rule Check Proteus?", "id": "Cek Konsistensi Logika Rangkaian." },
  { "en": "Apa Shortcut PLC Read GX Works?", "id": "Tekan Ctrl + T (Read)." },
  { "en": "Apa Fungsi Perintah Meshcrease Pada AutoCAD?", "id": "Menajamkan Tepi Objek Mesh." },
  { "en": "Apa Fungsi Macro bitRead Pada Arduino?", "id": "Membaca Status Bit Tertentu." },
  { "en": "Apa Itu Voltage Source Pulse Proteus?", "id": "Sumber Tegangan Pulsa Periodik." },
  { "en": "Bagaimana Cara Export Layout Ke DXF?", "id": "Menu Output Export DXF." },
  { "en": "Apa Fungsi Perintah Solidedit Body Check?", "id": "Validasi Geometri 3D Solid." },
  { "en": "Apa Tipe Data Float Arduino?", "id": "Bilangan Desimal 32 Bit." },
  { "en": "Komponen Apa Pemicu Triac?", "id": "Diac Atau Optocoupler." },
  { "en": "Apa Fungsi Instruksi CML (Complement) PLC?", "id": "Membalik Logika Semua Bit Data." },
  { "en": "Shortcut Apa Untuk Membuka Material Browser?", "id": "Ketik Matbrowseropen Lalu Enter." },
  { "en": "Apa Fungsi Keyword Volatile Arduino?", "id": "Cegah Optimasi Kompiler Pada Variabel." },
  { "en": "Apa Itu Resolusi ADC 10 Bit?", "id": "Nilai Digital 0 Sampai 1023." },
  { "en": "Apa Fungsi Instruksi STC (Set Carry)?", "id": "Set Flag Carry Menjadi 1." },
  { "en": "Bagaimana Cara Paste Ke Koordinat Asli?", "id": "Pilih Paste To Original Coordinates." },
  { "en": "Apa Fungsi Register MCUCR Pada AVR?", "id": "Register Kontrol Unit Mikrokontroler." },
  { "en": "Apa Itu Sensor Pressure MPX4250 Proteus?", "id": "Sensor Tekanan Absolut Analog." },
  { "en": "Apa Shortcut Simulation Pause GX Works?", "id": "Tekan Ctrl + F10." },
  { "en": "Apa Fungsi Perintah Meshuncrease AutoCAD?", "id": "Menghaluskan Tepi Mesh Yang Tajam." },
  { "en": "Apa Fungsi Macro bitWrite Pada Arduino?", "id": "Menulis Nilai 0 Atau 1 Ke Bit." },
  { "en": "Komponen Apa Yang Menghubungkan Osilator?", "id": "Kapasitor Keramik Ke Ground." },
  { "en": "Apa Fungsi Overflow Flag (V) PLC?", "id": "Indikator Hasil Melebihi Kapasitas." },
  { "en": "Shortcut Apa Untuk Uncomment Code?", "id": "Tekan Ctrl + Slash." },
  { "en": "Apa Fungsi Serial Println BIN?", "id": "Cetak Nilai Format Biner." },
  { "en": "Apa Itu Signal Generator Audio Proteus?", "id": "Sumber Sinyal Audio File WAV." },
  { "en": "Apa Fungsi Instruksi WOR (Word OR)?", "id": "Logika OR Tingkat Word." },
  { "en": "Bagaimana Cara Setting Render Quality?", "id": "Ubah Preset Di Render Window." },
  { "en": "Apa Fungsi IsSpace Pada Karakter Arduino?", "id": "Cek Spasi Tab Atau Newline." },
  { "en": "Apa Itu I2C Debugger Analyzer Mode?", "id": "Analisis Timing Sinyal I2C." },
  { "en": "Shortcut Apa Untuk Zoom Extents AutoCAD?", "id": "Klik Dua Kali Roda Mouse." },
  { "en": "Apa Fungsi Perintah Meshextrude Pada AutoCAD?", "id": "Menarik Wajah Mesh Keluar." },
  { "en": "Apa Fungsi String ToInt Arduino?", "id": "Konversi String Angka Ke Integer." },
  { "en": "Apa Itu IC 74HC165 Shift Register?", "id": "Parallel In Serial Out Register." },
  { "en": "Apa Fungsi Instruksi EI (Enable Interrupt)?", "id": "Mengaktifkan Interupsi Global." },
  { "en": "Bagaimana Cara Ukur Sudut Di AutoCAD?", "id": "Gunakan Perintah Dimangular." },
  { "en": "Apa Fungsi Register TIMSK1 Pada AVR?", "id": "Masker Interupsi Timer 1." },
  { "en": "Apa Itu Power Terminal VEE Proteus?", "id": "Sumber Tegangan Negatif." },
  { "en": "Shortcut Apa Untuk Hide Palettes AutoCAD?", "id": "Tekan Ctrl + Shift + H." },
  { "en": "Apa Fungsi Perintah Helix Base Radius?", "id": "Atur Radius Bawah Spiral." },
  { "en": "Apa Fungsi Karakter Backslash T?", "id": "Tabulasi Horizontal." },
  { "en": "Apa Itu Regulator AMS1117?", "id": "Regulator LDO 3.3V Atau 5V." },
  { "en": "Apa Fungsi Analog Reference Internal?", "id": "Referensi ADC 1.1 Volt." },
  { "en": "Bagaimana Cara Atur Tampilan Titik?", "id": "Ketik Ptype Lalu Enter." },
  { "en": "Apa Fungsi Register ADCL Pada AVR?", "id": "Byte Rendah Hasil Konversi ADC." },
  { "en": "Apa Itu Ratsnest Mode Proteus?", "id": "Tampilkan Garis Koneksi Udara." },
  { "en": "Apa Shortcut Verify Sketch?", "id": "Tekan Ctrl + R." },
  { "en": "Apa Fungsi Perintah Scale Reference?", "id": "Skala Objek Dengan Panjang Acuan." },
  { "en": "Apa Fungsi Operator Bitwise XOR (^)?", "id": "Logika Exclusive OR Per Bit." },
  { "en": "Apa Fungsi Pin SCL Arduino Uno?", "id": "Serial Clock Bus I2C." },
  { "en": "Apa Itu Resistor DIL Network?", "id": "Paket Resistor Dual In Line." },
  { "en": "Bagaimana Cara Update Data Link?", "id": "Klik Kanan Data Link Update." },
  { "en": "Apa Fungsi Perintah Dimjogged Angle?", "id": "Dimensi Sudut Pusat Jauh." },
  { "en": "Apa Fungsi Wire Write Buffer?", "id": "Kirim Buffer Data I2C." },
  { "en": "Apa Fungsi Pick And Place Output?", "id": "File Koordinat Penempatan Komponen." },
  { "en": "Apa Fungsi Carry Flag (C) PLC?", "id": "Indikator Sisa Operasi Aritmatika." },
  { "en": "Bagaimana Cara Insert OLE Object?", "id": "Gunakan Perintah Insertobj." },
  { "en": "Apa Fungsi Perintah Union Solid?", "id": "Menggabungkan Objek Solid." },
  { "en": "Mengapa Serial Monitor Arduino Muncul Karakter Aneh?", "id": "Baud Rate Tidak Cocok Antara Kode Dan Monitor." },
  { "en": "Bedanya Variable Global Dan Lokal Arduino?", "id": "Global Bisa Diakses Semua Fungsi, Lokal Hanya Di Fungsi Itu." },
  { "en": "Mengapa Garis Putus Putus AutoCAD Terlihat Solid Di Layout?", "id": "Variabel PSLTSCALE Belum Diatur Ke 1." },
  { "en": "Trik Agar Zoom Wheel Mouse AutoCAD Lebih Cepat?", "id": "Naikkan Nilai Variabel ZOOMFACTOR." },
  { "en": "Jika Simulasi Proteus 'Timestep Too Small', Apa Solusinya?", "id": "Ubah System Timing Control Ke Toleransi Lebih Besar." },
  { "en": "Bedanya Instruksi MOV Dan MOVP Pada PLC?", "id": "MOVP Hanya Eksekusi Sekali Saat Trigger Naik (Pulse)." },
  { "en": "Mengapa LED Di Proteus Tidak Menyala Meski Logika 1?", "id": "Model Digital Tidak Memiliki Properti Analog/Tegangan." },
  { "en": "Cara Mengatasi Hatch AutoCAD Yang 'Too Dense'?", "id": "Perbesar Nilai Scale Pada Menu Hatch." },
  { "en": "Kapan Menggunakan millis() Daripada delay()?", "id": "Saat Ingin Multitasking Tanpa Menghentikan Program." },
  { "en": "Efek Menghilangkan Kapasitor Decoupling Pada Mikrokontroler?", "id": "Sistem Sering Reset Sendiri Akibat Noise." },
  { "en": "Mengapa Xref AutoCAD Menjadi Pudar?", "id": "Variabel XDWGFADECTL Bernilai Di Atas 0." },
  { "en": "Bedanya Coil Normal -( )- Dan Set -(S)- Pada PLC?", "id": "Coil Mati Saat Input Mati, Set Tetap Hidup (Latching)." },
  { "en": "Jika Upload Arduino Error 'Access Denied', Apa Sebabnya?", "id": "Port Serial Sedang Dipakai Aplikasi Lain." },
  { "en": "Mengapa Objek 3D AutoCAD Terlihat Kotak Kotak Kasar?", "id": "Nilai FACETRES Terlalu Rendah." },
  { "en": "Cara Reset Tampilan Toolbar AutoCAD Yang Hilang?", "id": "Gunakan Perintah MENULOAD Atau Reset Profile." },
  { "en": "Bedanya Tipe Data float Dan double Di Arduino Uno?", "id": "Sama Saja, Keduanya 32 Bit Presisi Tunggal." },
  { "en": "Mengapa Simulasi Motor DC Proteus Tidak Berputar?", "id": "Tegangan Supply Kurang Atau Pin Enable Mati." },
  { "en": "Cara Cepat Menutup Semua Polygon Di AutoCAD?", "id": "Pilih Polyline Lalu Ketik C (Close) Di Command Line." },
  { "en": "Kapan Harus Menggunakan Interrupt Service Routine (ISR)?", "id": "Untuk Merespon Kejadian Mendesak Secara Instan." },
  { "en": "Bahaya Menggunakan Variabel Float Untuk Counter Loop?", "id": "Presisi Tidak Akurat Menyebabkan Loop Tak Berhenti." },
  { "en": "Mengapa PLC Error 'Battery Low'?", "id": "Baterai Backup Memori SRAM Sudah Habis." },
  { "en": "Cara Mengubah Background Putih AutoCAD Jadi Hitam?", "id": "Menu Options Display Colors 2D Model Space." },
  { "en": "Bedanya SPI Dan I2C Pada Komunikasi Sensor?", "id": "SPI Lebih Cepat 4 Kabel, I2C Lebih Hemat 2 Kabel." },
  { "en": "Solusi Jika Text Dimensi AutoCAD Terlalu Kecil?", "id": "Ubah DIMSCALE Di Dimension Style Manager." },
  { "en": "Mengapa Relay Di Proteus Tidak Klik?", "id": "Tegangan Coil Tidak Sesuai Spesifikasi Model." },
  { "en": "Trik Memilih Objek Yang Bertumpuk Di AutoCAD?", "id": "Aktifkan SELECTIONCYCLING (Ctrl + W)." },
  { "en": "Apa Yang Terjadi Jika Lupa pinMode() Input Pullup?", "id": "Pin Floating, Nilai Pembacaan Tidak Stabil." },
  { "en": "Bedanya Instruksi RET Dan END Pada PLC?", "id": "RET Kembali Ke Induk, END Akhir Scan Program." },
  { "en": "Cara Mencegah 'Bouncing' Pada Tombol Mekanis?", "id": "Gunakan Teknik Debouncing Software Atau Kapasitor." },
  { "en": "Mengapa File AutoCAD .BAK Muncul?", "id": "Itu File Cadangan Otomatis Saat Save." },
  { "en": "Kapan Menggunakan volatile Pada Variabel Arduino?", "id": "Saat Variabel Diubah Di Dalam Fungsi Interrupt." },
  { "en": "Solusi Garis AutoCAD Tidak Mau Lurus (Miring Dikit)?", "id": "Aktifkan ORTHO MODE (F8)." },
  { "en": "Mengapa LCD 16x2 Hanya Menampilkan Kotak Hitam?", "id": "Potensiometer Kontras V0 Belum Diatur." },
  { "en": "Bedanya Layer Freeze Dan Layer Off AutoCAD?", "id": "Freeze Menghemat RAM (Unload), Off Hanya Sembunyi." },
  { "en": "Cara Membalik Arah Logika Motor Servo?", "id": "Ubah Mapping Sudut (Misal: 180 - Sudut)." },
  { "en": "Mengapa PLC Output Transistor Jebol?", "id": "Beban Induktif Tanpa Dioda Flyback." },
  { "en": "Trik Copy Paste Antar File AutoCAD Agar Posisi Sama?", "id": "Gunakan Paste To Original Coordinates." },
  { "en": "Apa Efek Baud Rate Terlalu Tinggi?", "id": "Data Corrupt Atau Gagal Kirim Jika Kabel Panjang." },
  { "en": "Mengapa Simulasi Proteus Sangat Lambat (Lag)?", "id": "Terlalu Banyak Komponen Analog Atau Animasi." },
  { "en": "Bedanya Block Dan Group Di AutoCAD?", "id": "Block Satu Entitas (Bisa Atribut), Group Hanya Kumpulan." },
  { "en": "Solusi Error 'Sketch Too Large' Arduino?", "id": "Optimasi Kode Atau Hapus Library Tidak Terpakai." },
  { "en": "Mengapa Objek Hatch Tidak Bisa Dipotong (Trim)?", "id": "Hatch Tidak Asosiatif Atau Boundary Rusak." },
  { "en": "Cara Mengunci Skala Viewport Layout AutoCAD?", "id": "Klik Ikon Gembok Di Status Bar Viewport." },
  { "en": "Kapan Menggunakan Analog Reference 'INTERNAL'?", "id": "Saat Mengukur Tegangan Rendah (<1.1V) Presisi Tinggi." },
  { "en": "Mengapa Timer PLC Tidak Menghitung?", "id": "Input Enable Timer Terputus Sebelum Waktu Habis." },
  { "en": "Bedanya Wire Dan Net Label Di Proteus?", "id": "Wire Koneksi Fisik, Label Koneksi Logika Jarak Jauh." },
  { "en": "Trik Mengurangi Ukuran File DWG AutoCAD?", "id": "Gunakan Perintah PURGE Dan AUDIT." },
  { "en": "Apa Masalah Jika Ground Arduino Dan Sensor Terpisah?", "id": "Referensi Tegangan Beda, Data Kacau." },
  { "en": "Mengapa Perintah Fillet AutoCAD Gagal (Invalid)?", "id": "Radius Fillet Lebih Besar Dari Panjang Garis." },
  { "en": "Cara Cepat Mengganti Semua Teks Tertentu Di AutoCAD?", "id": "Gunakan Perintah FIND." },
  { "en": "Bedanya void loop() Dan void setup()?", "id": "Setup Jalan Sekali, Loop Jalan Berulang Selamanya." },
  { "en": "Mengapa Sinyal Osiloskop Proteus Datar?", "id": "Channel Belum Diset Ke DC Atau AC." },
  { "en": "Solusi Kursor AutoCAD Hilang/Tidak Terlihat?", "id": "Ubah Warna Crosshair Atau Cek CURSORSIZE." },
  { "en": "Apa Resiko Menggunakan delay() Di Dalam Interrupt?", "id": "Program Macet Karena Timer Interrupt Diblokir." },
  { "en": "Bedanya Sink Dan Source Pada Wiring PLC?", "id": "Sink Terima Arus (0V), Source Kirim Arus (24V)." },
  { "en": "Mengapa Arduino Reset Saat Relay Aktif?", "id": "Arus Balik Koil Relay (Butuh Dioda) Atau Power Drop." },
  { "en": "Cara Memecah Blok AutoCAD Tanpa Kehilangan Atribut?", "id": "Gunakan Perintah BURST (Express Tools)." },
  { "en": "Kapan Menggunakan Tipe Data unsigned long?", "id": "Untuk Waktu (millis) Agar Tidak Minus Saat Overflow." },
  { "en": "Mengapa Jalur PCB Proteus ARES Warna Merah?", "id": "Itu Top Layer (Lapisan Atas)." },
  { "en": "Solusi Objek AutoCAD Tidak Bisa Di-Select?", "id": "Layer Terkunci Atau PICKFIRST Bernilai 0." },
  { "en": "Bedanya Instruksi ADD Dan ADDP Pada PLC?", "id": "ADDP Menjumlahkan Hanya Saat Trigger Naik Sesaat." },
  { "en": "Mengapa Sensor Ultrasonik Pembacaannya 0 Terus?", "id": "Pin Trigger Dan Echo Terbalik Atau Kabel Putus." },
  { "en": "Trik Membuat Garis Lengkung Halus Di AutoCAD?", "id": "Gunakan SPLINE Daripada Polyline." },
  { "en": "Apa Fungsi Kapasitor 100nF Di Kaki Power IC?", "id": "Menyaring Frekuensi Tinggi (Bypass Noise)." },
  { "en": "Mengapa Text AutoCAD Terbalik Saat Di Mirror?", "id": "Variabel MIRRTEXT Bernilai 1 (Harusnya 0)." },
  { "en": "Cara Menghemat Memori SRAM Arduino?", "id": "Gunakan F() Macro Pada Serial.print." },
  { "en": "Bedanya DigitalWrite Dan AnalogWrite?", "id": "Digital Hidup/Mati, Analog Sinyal PWM (Semi Analog)." },
  { "en": "Mengapa Simulasi Proteus Error 'GND not defined'?", "id": "Belum Menambahkan Terminal Ground." },
  { "en": "Solusi Print AutoCAD Terpotong Pinggir?", "id": "Cek Printer Margins Di Page Setup Manager." },
  { "en": "Kapan Menggunakan Resistor Pull-Down?", "id": "Agar Input Tidak Floating Saat Saklar Terbuka." },
  { "en": "Bedanya Memory Retentive Dan Non-Retentive PLC?", "id": "Retentive Ingat Data Saat Mati Lampu, Non Reset." },
  { "en": "Mengapa Lingkaran AutoCAD Terlihat Segi Banyak?", "id": "Perlu Regen (RE) Atau Naikkan VIEWRES." },
  { "en": "Cara Mengirim Data Float Antar Arduino via I2C?", "id": "Gunakan Union Atau Kirim Per Byte." },
  { "en": "Mengapa Komponen Proteus Tidak Ada Footprint?", "id": "Itu Komponen Simulasi Saja, Bukan Untuk PCB." },
  { "en": "Solusi Tombol F8 (Ortho) AutoCAD Tidak Jalan?", "id": "Kunci Fn Keyboard Laptop Mungkin Aktif." },
  { "en": "Bedanya Operator = Dan == Dalam Coding?", "id": "= Untuk Mengisi Nilai, == Untuk Membandingkan." },
  { "en": "Mengapa PLC Input Menyala Tapi Program Diam?", "id": "Alamat Input Salah Atau PLC Masih Mode STOP." },
  { "en": "Trik Mengukur Panjang Total Banyak Garis AutoCAD?", "id": "Gunakan LISP 'Total Length' Atau Join Pline." },
  { "en": "Apa Efek Short Circuit Pada Output Arduino?", "id": "Pin Rusak Permanen Atau Regulator Panas." },
  { "en": "Mengapa Autorouter Proteus Gagal 100%?", "id": "Desain Terlalu Padat Atau Aturan DRC Ketat." },
  { "en": "Cara Mengetahui Alamat I2C Perangkat?", "id": "Jalankan Sketsa I2C Scanner." },
  { "en": "Bedanya WCS Dan UCS Di AutoCAD?", "id": "WCS Koordinat Dunia Tetap, UCS Koordinat User." },
  { "en": "Mengapa Variabel Lokal Kehilangan Nilai?", "id": "Karena Direset Setiap Fungsi Dipanggil (Gunakan Static)." },
  { "en": "Solusi Plot Style Table (CTB) Hilang?", "id": "Cek Folder Plot Style Di Options Files." },
  { "en": "Kapan Menggunakan Transistor PNP?", "id": "Untuk Switching Sisi Positif (High Side)." },
  { "en": "Mengapa Counter PLC Mereset Sendiri?", "id": "Kondisi Reset Coil Aktif Terus." },
  { "en": "Trik Print Hitam Putih Di AutoCAD?", "id": "Pilih Plot Style 'Monochrome.ctb'." },
  { "en": "Bedanya ++i Dan i++ Dalam Loop?", "id": "++i Tambah Dulu Baru Pakai, i++ Pakai Dulu Baru Tambah." },
  { "en": "Mengapa Power Rail Proteus Error?", "id": "Konflik Tegangan (Misal VCC 5V Disambung 12V)." },
  { "en": "Solusi Mouse Tengah AutoCAD Tidak Bisa Pan?", "id": "Ubah Variabel MBUTTONPAN Menjadi 1." },
  { "en": "Apa Fungsi Dioda Paralel Dengan Motor?", "id": "Flyback Diode Untuk Mencegah Lonjakan Tegangan Balik." },
  { "en": "Mengapa Input Analog Arduino Tidak Stabil?", "id": "Impedansi Sumber Tinggi Atau Noise Power Supply." },
  { "en": "Bedanya MTEXT Dan DTEXT AutoCAD?", "id": "MTEXT Multibaris (Paragraf), DTEXT Satu Baris." },
  { "en": "Cara Reset Arduino Tanpa Tombol Reset?", "id": "Hubungkan Pin Reset Ke Ground Sesaat." },
  { "en": "Mengapa Simulasi LCD Proteus Kosong?", "id": "Library LCD Tidak Terhubung Ke Pin Data 4-Bit/8-Bit." },
  { "en": "Solusi AutoCAD Fatal Error Saat Save?", "id": "Gunakan RECOVER Pada File Atau Save Format Lama." },
  { "en": "Kapan Memakai Relay Solid State (SSR)?", "id": "Untuk Switching Cepat Tanpa Suara Dan Percikan." },
  { "en": "Bedanya Upload Dan Burn Bootloader?", "id": "Upload Kirim Program, Burn Pasang Sistem Dasar." },
  { "en": "Mengapa Perintah JOIN AutoCAD Tidak Menyambung Garis?", "id": "Ujung Garis Tidak Bertemu Atau Ada Celah (Gap)." },
  { "en": "Bedanya `const int` Dan `#define` Di Arduino?", "id": "Const Memiliki Tipe Data Dan Scope, Define Hanya Text Replacement." },
  { "en": "Solusi Jika Teks AutoCAD Saat Di-Print Bolong (Hollow)?", "id": "Set Variabel TEXTFILL Menjadi 1." },
  { "en": "Mengapa Simulasi Proteus Error 'Timestep too small'?", "id": "Rangkaian Mengalami Osilasi Cepat Atau Konvergensi Gagal." },
  { "en": "Apa Penyebab PLC Input X0 Aktif Tapi LED Mati?", "id": "Kabel Common (S/S) Belum Terhubung Ke Positif/Negatif." },
  { "en": "Trik Agar Hatch AutoCAD Tidak Menutupi Teks?", "id": "Gunakan Perintah TEXTTOFRONT Sebelum Hatching." },
  { "en": "Mengapa `Serial.read()` Arduino Mengembalikan Nilai -1?", "id": "Buffer Serial Kosong, Tidak Ada Data Diterima." },
  { "en": "Bedanya Kabel Merah Dan Biru Di Proteus ARES?", "id": "Merah Top Layer, Biru Bottom Layer." },
  { "en": "Cara Mengatasi 'Dimension Text' Yang Pindah Sendiri?", "id": "Matikan Fitur 'Dim Line Forced' Di Dimstyle." },
  { "en": "Mengapa Relay PLC Cepat Rusak?", "id": "Switching Beban Induktif Terlalu Sering (Gunakan Transistor)." },
  { "en": "Solusi Arduino Error 'stk500_getsync'?", "id": "Kabel USB Longgar, Driver Salah, Atau Bootloader Rusak." },
  { "en": "Apa Fungsi Dioda 1N4007 Di Output Relay?", "id": "Mencegah Arus Balik (Back EMF) Merusak Transistor." },
  { "en": "Mengapa Garis Lengkung AutoCAD Terlihat Patah-Patah?", "id": "Jalankan Perintah REGEN (RE) Untuk Refresh Tampilan." },
  { "en": "Bedanya Mode `INPUT` Dan `INPUT_PULLUP`?", "id": "Pullup Mengaktifkan Resistor Internal Agar Tidak Floating." },
  { "en": "Trik Menghapus Layer AutoCAD Yang Membandel?", "id": "Gunakan Perintah LAYDEL (Hati-hati Objek Terhapus)." },
  { "en": "Mengapa Simulasi Mikrokontroler Proteus Tidak Jalan?", "id": "File Hex/Elf Belum Dimasukkan Ke Properti Komponen." },
  { "en": "Cara Cepat Mengubah Skala Garis Putus-Putus?", "id": "Ubah Nilai LTS (LTSCALE) Di Command Line." },
  { "en": "Bedanya Timer T-ON Dan T-OFF Pada PLC?", "id": "T-ON Tunda Hidup, T-OFF Tunda Mati." },
  { "en": "Mengapa Servo Bergetar (Jitter) Di Posisi Diam?", "id": "Sinyal PWM Tidak Stabil Atau Power Supply Kurang." },
  { "en": "Solusi Jika File AutoCAD Terkena Virus Lisp?", "id": "Hapus File acaddoc.lsp Dan Gunakan Antivirus CAD." },
  { "en": "Apa Itu 'Floating Pin' Dan Mengapa Berbahaya?", "id": "Pin Tanpa Referensi Tegangan, Membaca Noise Acak." },
  { "en": "Mengapa Autorouter PCB Sering Gagal?", "id": "Setting Clearance Terlalu Ketat Atau Board Sempit." },
  { "en": "Bedanya `Serial.print` Dan `Serial.write`?", "id": "Print Kirim Teks ASCII, Write Kirim Nilai Byte Asli." },
  { "en": "Cara Mengunci Viewport Agar Skala Tidak Berubah?", "id": "Klik Kanan Viewport Pilih Display Locked > Yes." },
  { "en": "Mengapa Sensor Suhu LM35 Tidak Akurat?", "id": "Kabel Terlalu Panjang Menyebabkan Drop Tegangan." },
  { "en": "Trik Memilih Objek Di Belakang Hatch?", "id": "Tekan Shift + Spasi Saat Hover Mouse." },
  { "en": "Apa Fungsi `yield()` Pada ESP8266/ESP32?", "id": "Mencegah Watchdog Reset Saat Loop Panjang." },
  { "en": "Bedanya NC (Normally Closed) Dan NO (Normally Open)?", "id": "NC Nyambung Saat Diam, NO Putus Saat Diam." },
  { "en": "Mengapa LED Matrix Berkedip (Flickering)?", "id": "Scanning Rate Terlalu Rendah (Kurang Dari 50Hz)." },
  { "en": "Solusi Zoom Extents AutoCAD Malah Jauh Sekali?", "id": "Ada Objek Sampah Di Koordinat Sangat Jauh (Hapus)." },
  { "en": "Mengapa I2C LCD Menampilkan Karakter Acak?", "id": "Library Tidak Cocok Atau Alamat Hex I2C Salah." },
  { "en": "Cara Membuat Garis Tebal Tanpa Polyline?", "id": "Aktifkan Lineweight (LWIDTH) Di Status Bar." },
  { "en": "Bedanya Memory Flash Dan SRAM Arduino?", "id": "Flash Simpan Program (Permanen), SRAM Simpan Variabel (Sementara)." },
  { "en": "Mengapa Simulasi 7-Segment Proteus Mati?", "id": "Lupa Menambahkan Resistor Atau Common Anode/Cathode Salah." },
  { "en": "Trik Mengcopy Layout Page Setup Ke File Lain?", "id": "Gunakan Perintah PSETUPIN." },
  { "en": "Apa Penyebab Error 'Compiling for board Gagal'?", "id": "Board Manager Belum Diinstall Atau Versi Konflik." },
  { "en": "Mengapa PLC Output Relay Lengket (Welding)?", "id": "Arus Beban Melebihi Spesifikasi Kontak Relay." },
  { "en": "Solusi Kursor Mouse AutoCAD Lag/Patah-Patah?", "id": "Matikan Hardware Acceleration (GRAPHICSCONFIG)." },
  { "en": "Kapan Menggunakan `unsigned long` Untuk Waktu?", "id": "Saat Menggunakan `millis()` Agar Tidak Error Overflow." },
  { "en": "Bedanya Komponen Device Dan Subcircuit Di Proteus?", "id": "Device Adalah Part Tunggal, Subcircuit Adalah Blok Rangkaian." },
  { "en": "Mengapa Perintah TRIM AutoCAD Memotong Semua?", "id": "Mode TRIM Diubah Ke Standard (Ubah Ke Quick)." },
  { "en": "Cara Mengatasi 'Button Debounce' Tanpa Delay?", "id": "Cek Status Tombol Berdasarkan Selisih Waktu millis()." },
  { "en": "Apa Fungsi Transistor Pada Rangkaian Relay?", "id": "Sebagai Saklar Penguat Arus Dari Mikrokontroler." },
  { "en": "Mengapa Dimensi AutoCAD Tidak Muncul Angkanya?", "id": "Warna Teks Sama Dengan Background Atau Layer Off." },
  { "en": "Solusi Jalur PCB Proteus Terlalu Tipis?", "id": "Ubah Trace Style Di Design Rule Manager." },
  { "en": "Bedanya Instruksi `Set` Dan `Out` Pada Ladder?", "id": "`Set` Menahan Status ON, `Out` Mengikuti Input." },
  { "en": "Mengapa `float` Tidak Disarankan Untuk Uang?", "id": "Masalah Presisi Desimal (Rounding Error)." },
  { "en": "Trik Memutar Viewport AutoCAD Tanpa Putar Objek?", "id": "Gunakan Perintah VPROTATEASSOC." },
  { "en": "Apa Penyebab Sensor PIR Terus Menerus High?", "id": "Noise Power Supply Atau Interferensi Wi-Fi." },
  { "en": "Mengapa File DXF Tidak Bisa Dibuka Di Software Lain?", "id": "Versi DXF Terlalu Baru (Save As Versi R12/2000)." },
  { "en": "Cara Menampilkan Kaki VCC/GND Tersembunyi Di Proteus?", "id": "Klik Icon 'Invoke' Pada Komponen." },
  { "en": "Bedanya Loop `for` Dan `while`?", "id": "`for` Untuk Jumlah Pasti, `while` Untuk Kondisi Belum Pasti." },
  { "en": "Mengapa Motor Stepper Hanya Bergetar?", "id": "Urutan Kabel Coil (Phase) Terbalik." },
  { "en": "Solusi Objek AutoCAD Hilang Saat Di-Zoom?", "id": "Video Card Driver Masalah Atau Perlu 3DCONFIG." },
  { "en": "Kapan Menggunakan Protokol UART SoftwareSerial?", "id": "Saat Pin Hardware Serial (0 & 1) Sudah Terpakai." },
  { "en": "Apa Efek Menghubungkan 5V Ke Pin 3.3V?", "id": "Komponen 3.3V Akan Terbakar (Overvoltage)." },
  { "en": "Mengapa Ladder PLC Error Saat Compile?", "id": "Logic Gantung (Dangling Coil) Atau Double Coil." },
  { "en": "Trik Mengganti Block Editor Background Color?", "id": "Masuk Options > Display > Colors > Block Editor." },
  { "en": "Bedanya `break` Dan `continue` Dalam Loop?", "id": "`break` Keluar Loop, `continue` Loncat Ke Iterasi Berikut." },
  { "en": "Mengapa Simulasi Analog Proteus Error Konvergensi?", "id": "Gunakan Opsi 'GMIN Stepping' Di Analysis Settings." },
  { "en": "Cara Mengambil Koordinat Z Dari Objek 2D?", "id": "Gunakan Perintah ID Point Atau Properties Elevation." },
  { "en": "Apa Itu 'Blocking Code' Di Arduino?", "id": "Kode Yang Menghentikan Proses Lain (Contoh: delay)." },
  { "en": "Mengapa Sensor Load Cell Nilainya Tidak Stabil?", "id": "Power Supply Tidak Stabil Atau Kabel Kurang Shielding." },
  { "en": "Solusi Jika Menu Bar AutoCAD Hilang Total?", "id": "Ketik MENUBAR Set Nilai 1." },
  { "en": "Bedanya Analog Input Dan Analog Output (PWM)?", "id": "Input Baca Tegangan, Output Simulasi Tegangan Rata-Rata." },
  { "en": "Mengapa Rangkaian Oscilator Kristal Tidak Jalan?", "id": "Nilai Kapasitor Load Tidak Sesuai (Biasanya 22pF)." },
  { "en": "Trik Print PDF AutoCAD Agar Layer Bisa Di-Off?", "id": "Centang 'Include Layer Information' Di Plotter Config." },
  { "en": "Kapan Menggunakan Tipe Data `char` vs `String`?", "id": "`char` Hemat Memori, `String` Mudah Dimanipulasi Tapi Berat." },
  { "en": "Mengapa PLC Tidak Bisa Download Program?", "id": "Switch Fisik PLC Masih Di Posisi RUN (Ubah Ke STOP)." },
  { "en": "Cara Mengatasi 'Polygon Pour' Tidak Mengisi Area?", "id": "Cek Clearance Rule Atau Net Name Polygon." },
  { "en": "Bedanya Array 1 Dimensi Dan 2 Dimensi?", "id": "1D Seperti Daftar List, 2D Seperti Tabel Baris Kolom." },
  { "en": "Mengapa Modul Bluetooth HC-05 Tidak Merespon?", "id": "Baud Rate Salah Atau Lupa Voltage Divider Di RX." },
  { "en": "Solusi Garis Dimensi Menimpa Garis Objek?", "id": "Gunakan Perintah DIMBREAK." },
  { "en": "Apa Fungsi Pull-Down Resistor Pada Mosfet?", "id": "Membuang Muatan Gate Agar Mosfet Mati Sempurna." },
  { "en": "Mengapa Simulasi Proteus Berjalan Sangat Cepat?", "id": "CPU Load Rendah, Proteus Mempercepat Waktu Simulasi." },
  { "en": "Trik Mengembalikan File AutoCAD Yang Belum Di-Save?", "id": "Cari File Berestensi .sv$ Di Folder Temp." },
  { "en": "Bedanya Fungsi `void` Dan Fungsi Yang Mengembalikan Nilai?", "id": "`void` Tidak Menghasilkan Output, Lainnya Return Data." },
  { "en": "Mengapa Relay Cetak-Cetek Terus Menerus?", "id": "Tegangan Supply Drop Saat Relay Aktif (Looping)." },
  { "en": "Solusi Pesan Error 'Invalid Window Spec' Di AutoCAD?", "id": "Zoom Extents Dulu Sebelum Print Window." },
  { "en": "Kapan Harus Menggunakan Heatsink Pada Regulator?", "id": "Saat Perbedaan Tegangan Input-Output Dan Arus Besar." },
  { "en": "Mengapa High Speed Counter PLC Tidak Menghitung?", "id": "Input Filter Terlalu Besar (Delay), Kurangi Filter." },
  { "en": "Cara Membuat Jalur Melengkung Di Proteus ARES?", "id": "Tahan Tombol Ctrl Saat Routing Jalur." },
  { "en": "Bedanya `&` (Bitwise) Dan `&&` (Boolean) AND?", "id": "`&` Operasi Biner Angka, `&&` Operasi Logika Kondisi." },
  { "en": "Mengapa RTC DS3231 Lupa Waktu Saat Power Off?", "id": "Baterai Kancing (CR2032) Habis Atau Kontak Longgar." },
  { "en": "Solusi Blok AutoCAD Tidak Bisa Di-Explode?", "id": "Edit Blok Dan Centang 'Allow Exploding' Di Properties." },
  { "en": "Apa Fungsi Kapasitor Elco Di Jalur Power?", "id": "Penyimpan Energi Sementara Untuk Meratakan Tegangan." },
  { "en": "Mengapa Tulisan LCD 16x2 Huruf Aneh?", "id": "Inisialisasi `lcd.begin()` Salah Ukuran Kolom Baris." },
  { "en": "Trik Menggunakan Kalkulator Di Tengah Perintah AutoCAD?", "id": "Ketik `'CAL` (Apostrof Cal) Saat Diminta Input." },
  { "en": "Bedanya SRAM Dan EEPROM?", "id": "SRAM Hilang Saat Mati (Cepat), EEPROM Tetap Ada (Lambat)." },
  { "en": "Mengapa Simulasi Motor Stepper Proteus Skip Step?", "id": "Frekuensi Pulsa Terlalu Tinggi Untuk Model Simulasi." },
  { "en": "Cara Mengetahui Baud Rate Modul GPS?", "id": "Coba Brute Force Baud Rate Di Serial Monitor." },
  { "en": "Solusi Garis Putus-Putus Tidak Rapi Di Sudut?", "id": "Set Variabel PLINEGEN Menjadi 1." },
  { "en": "Kapan Menggunakan Relay vs Transistor?", "id": "Relay Untuk AC/Arus Tinggi, Transistor Untuk DC Cepat." },
  { "en": "Mengapa Password PLC Bisa Terkunci?", "id": "Salah Memasukkan Password 3 Kali Atau Lupa Unlock." },
  { "en": "Trik Memisahkan Hatch Yang Menyatu?", "id": "Pilih Hatch, Klik Kanan > Separate Hatches." },
  { "en": "Bedanya `if` Dan `switch case`?", "id": "`if` Fleksibel Untuk Range, `switch` Rapih Untuk Nilai Pasti." },
  { "en": "Mengapa SD Card Module Gagal Inisialisasi?", "id": "Format Kartu Bukan FAT32 Atau Kabel SPI Panjang." },
  { "en": "Solusi Mouse Wheel Zoom Terlalu Cepat/Lambat?", "id": "Ubah Nilai Variabel ZOOMFACTOR (Default 60)." },
  { "en": "Apa Itu 'Magic Smoke' Dalam Elektronika?", "id": "Istilah Candaan Saat Komponen Terbakar Keluar Asap." },
  { "en": "Apa Fungsi Perintah Oops Pada AutoCAD?", "id": "Mengembalikan Objek Terakhir Dihapus Tanpa Undo." },
  { "en": "Apa Fungsi Perintah Multiple Pada AutoCAD?", "id": "Mengulang Perintah Terakhir Secara Terus Menerus." },
  { "en": "Shortcut Apa Untuk Membuka External References?", "id": "Ketik Xref Lalu Enter." },
  { "en": "Apa Fungsi Variabel Sistem Angbase Pada AutoCAD?", "id": "Mengatur Arah Sudut Nol Derajat." },
  { "en": "Apa Fungsi Variabel Sistem Angdir Pada AutoCAD?", "id": "Mengatur Arah Putaran Sudut (CW/CCW)." },
  { "en": "Apa Fungsi Scripting Mode Di Proteus?", "id": "Mengotomatisasi Tugas Simulasi Dengan Kode Python." },
  { "en": "Apa Fungsi Instruksi SWAP (Swap Bytes) PLC?", "id": "Menukar 8 Bit Atas Dan Bawah." },
  { "en": "Apa Fungsi Instruksi XCHG (Block Exchange) PLC?", "id": "Menukar Isi Dua Register Data Word." },
  { "en": "Komponen LM324 Pin 4 Berfungsi Sebagai?", "id": "Input Tegangan Supply Positif (VCC)." },
  { "en": "Apa Fungsi Perintah Overkill Pada AutoCAD?", "id": "Menghapus Garis Ganda Atau Tumpang Tindih." },
  { "en": "Apa Fungsi Fungsi noInterrupts() Pada Arduino?", "id": "Mematikan Semua Interupsi Global." },
  { "en": "Bagaimana Cara Mengatur Pickbox Size AutoCAD?", "id": "Ubah Variabel Sistem Pickbox." },
  { "en": "Apa Fungsi Tombol Ctrl Shift V AutoCAD?", "id": "Paste Clip Sebagai Blok (Paste As Block)." },
  { "en": "Apa Fungsi Perintah Minsert Pada AutoCAD?", "id": "Memasukkan Blok Dalam Susunan Array." },
  { "en": "Komponen IC 555 Pin 5 Berfungsi Sebagai?", "id": "Control Voltage (Pengatur Ambang Komparator)." },
  { "en": "Apa Fungsi Instruksi BSET (Bit Set) PLC?", "id": "Mengubah Status Bit Tertentu Menjadi 1." },
  { "en": "Apa Fungsi Instruksi BRST (Bit Reset) PLC?", "id": "Mengubah Status Bit Tertentu Menjadi 0." },
  { "en": "Bagaimana Cara Mengatur Filedia AutoCAD?", "id": "Ubah Variabel Sistem Filedia (0 Teks, 1 Dialog)." },
  { "en": "Apa Fungsi Register TWCR Pada AVR (Arduino)?", "id": "Register Kontrol TWI (I2C) Hardware." },
  { "en": "Apa Itu Data Memory Cascade (C) PLC?", "id": "Register Counter (Penghitung)." },
  { "en": "Apa Itu Special Relay M8029 PLC Mitsubishi?", "id": "Flag Selesai Eksekusi Instruksi." },
  { "en": "Shortcut Apa Untuk Show Sketch Folder Arduino?", "id": "Tekan Ctrl + K." },
  { "en": "Apa Fungsi Library SPI SetClockDivider?", "id": "Mengatur Kecepatan Clock Bus SPI." },
  { "en": "Bagaimana Cara Mengatur Snap Angle Proteus?", "id": "Menu View Snap Angle." },
  { "en": "Apa Fungsi Instruksi NEG (Twos Complement)?", "id": "Membalik Nilai Positif Menjadi Negatif." },
  { "en": "Apa Fungsi Instruksi ABS (Absolute Value) PLC?", "id": "Mengubah Nilai Negatif Menjadi Positif." },
  { "en": "Apa Fungsi Perintah Dimtiledit Pada AutoCAD?", "id": "Menggeser Teks Dimensi Sepanjang Garis." },
  { "en": "Apa Itu Noise Pada Sinyal Analog?", "id": "Gangguan Sinyal Yang Tidak Diinginkan." },
  { "en": "Apa Itu Bootloader Pada Arduino?", "id": "Program Kecil Untuk Upload Kode Via Serial." },
  { "en": "Shortcut Apa Untuk Step Into Simulation GX Works?", "id": "Tekan Tombol F11." },
  { "en": "Apa Fungsi Register TWDR Pada AVR (Arduino)?", "id": "Register Data TWI (I2C) Untuk Kirim/Terima." },
  { "en": "Apa Fungsi Transfer Function Analysis Proteus?", "id": "Analisis Hubungan Input-Output DC." },
  { "en": "Apa Fungsi Timer Off-Delay (TOF) PLC?", "id": "Menunda Matinya Output Setelah Input Mati." },
  { "en": "Bagaimana Cara Membuat Wipeout AutoCAD?", "id": "Gunakan Perintah Wipeout." },
  { "en": "Apa Fungsi Operator Bitwise Shift Left (<<)?", "id": "Geser Bit Ke Kiri (Kali 2 Pangkat n)." },
  { "en": "Apa Itu Terminal Output Di Proteus?", "id": "Titik Keluaran Sinyal Ke Grafik/Probe." },
  { "en": "Shortcut Apa Untuk Membuka Visual Styles Manager?", "id": "Ketik Visualstyles Lalu Enter." },
  { "en": "Apa Fungsi Serial AvailableForWrite()?", "id": "Cek Sisa Byte Di Buffer Pengiriman." },
  { "en": "Komponen Apa Yang Menghubungkan Pin VCC Arduino?", "id": "Regulator Tegangan 5V." },
  { "en": "Apa Fungsi Instruksi ITOF (Int To Float)?", "id": "Konversi Integer Ke Bilangan Desimal." },
  { "en": "Bagaimana Cara Xref Reload AutoCAD?", "id": "Memuat Ulang File Referensi Yang Diubah." },
  { "en": "Apa Itu EEPROM Write Arduino?", "id": "Menulis Satu Byte Data Ke EEPROM." },
  { "en": "Apa Fungsi Pre-Production Checker Proteus?", "id": "Cek Integritas Desain Sebelum Layout PCB." },
  { "en": "Apa Shortcut PLC Batch Monitor GX Works?", "id": "Menu Online Monitor Batch Monitor." },
  { "en": "Apa Fungsi Perintah Psout Pada AutoCAD?", "id": "Ekspor Gambar Ke Format PostScript (EPS)." },
  { "en": "Apa Fungsi Macro bit() Pada Arduino?", "id": "Menghitung Nilai Desimal Dari Bit (2^n)." },
  { "en": "Apa Itu Voltage Source DC Proteus?", "id": "Sumber Tegangan Searah Konstan." },
  { "en": "Bagaimana Cara Export Layout Ke EMF?", "id": "Menu Output Export Metafile." },
  { "en": "Apa Fungsi Perintah Solidedit Body Separate?", "id": "Memisahkan Solid Yang Terputus." },
  { "en": "Apa Tipe Data Char Pada Arduino?", "id": "Menyimpan Satu Karakter ASCII (8 Bit)." },
  { "en": "Komponen Apa Pemicu Transistor PNP?", "id": "Arus Negatif (Ground) Ke Basis." },
  { "en": "Apa Fungsi Instruksi WAND (Word Logic AND)?", "id": "Operasi Logika AND 16-Bit." },
  { "en": "Shortcut Apa Untuk Membuka Material Editor?", "id": "Ketik Mateditoropen Lalu Enter." },
  { "en": "Apa Fungsi Keyword Interrupts() Arduino?", "id": "Mengaktifkan Kembali Interupsi Global." },
  { "en": "Apa Itu Resolusi PWM Arduino Uno?", "id": "8 Bit (0 Sampai 255)." },
  { "en": "Apa Fungsi Instruksi CLC (Clear Carry)?", "id": "Reset Flag Carry Menjadi 0." },
  { "en": "Bagaimana Cara Paste Link AutoCAD?", "id": "Menu Edit Paste Special Paste Link." },
  { "en": "Apa Fungsi Register ADMUX Pada AVR?", "id": "Memilih Channel Input Analog (ADC)." },
  { "en": "Apa Itu Sensor Ultrasonic Ping Proteus?", "id": "Modul Pengukur Jarak Suara." },
  { "en": "Apa Shortcut Simulation Run GX Works?", "id": "Tekan Shift + Enter." },
  { "en": "Apa Fungsi Perintah Rtext Pada AutoCAD?", "id": "Membuat Teks Reaktif (Express Tools)." },
  { "en": "Apa Fungsi Macro lowByte() Pada Arduino?", "id": "Mengambil 8 Bit Terendah Dari Data." },
  { "en": "Komponen Apa Yang Menstabilkan Clock?", "id": "Kristal Kuarsa." },
  { "en": "Apa Fungsi Carry Flag (C) PLC?", "id": "Indikator Sisa Hasil Operasi Aritmatika." },
  { "en": "Shortcut Apa Untuk Format Code Arduino?", "id": "Tekan Ctrl + T." },
  { "en": "Apa Fungsi Serial Println()?", "id": "Cetak Data Ditambah Baris Baru." },
  { "en": "Apa Itu Signal Generator Pulse Proteus?", "id": "Pembangkit Sinyal Pulsa Kotak." },
  { "en": "Apa Fungsi Instruksi WOR (Word Logic OR)?", "id": "Operasi Logika OR 16-Bit." },
  { "en": "Bagaimana Cara Setting Render Output?", "id": "Pilih Format Gambar Di Jendela Render." },
  { "en": "Apa Fungsi IsDigit() Pada Arduino?", "id": "Cek Apakah Karakter Adalah Angka." },
  { "en": "Apa Itu I2C Debugger Proteus?", "id": "Alat Analisis Protokol I2C." },
  { "en": "Shortcut Apa Untuk Zoom Dynamic AutoCAD?", "id": "Ketik Z Lalu D." },
  { "en": "Apa Fungsi Perintah Qselect Pada AutoCAD?", "id": "Seleksi Objek Berdasarkan Kriteria." },
  { "en": "Apa Fungsi String ToInt() Arduino?", "id": "Mengubah String Angka Menjadi Integer." },
  { "en": "Apa Itu IC 4017 Decade Counter?", "id": "Penghitung Desimal 10 Output." },
  { "en": "Apa Fungsi Instruksi EI (Enable Interrupt)?", "id": "Mengizinkan Interupsi Pada PLC." },
  { "en": "Bagaimana Cara Ukur Panjang Kurva?", "id": "Gunakan Perintah Lengthen Atau List." },
  { "en": "Apa Fungsi Pin AREF Arduino?", "id": "Input Tegangan Referensi Eksternal ADC." },
  { "en": "Apa Itu Power Terminal VDD Proteus?", "id": "Sumber Tegangan Positif IC Digital." },
  { "en": "Shortcut Apa Untuk Hide Palettes AutoCAD?", "id": "Tekan Ctrl + Shift + H." },
  { "en": "Apa Fungsi Perintah Helix Base Radius?", "id": "Mengatur Jari-Jari Dasar Spiral." },
  { "en": "Apa Fungsi Karakter Backslash T?", "id": "Membuat Tabulasi Horizontal." },
  { "en": "Apa Itu Regulator 7805?", "id": "Stabilizer Tegangan 5 Volt Positif." },
  { "en": "Apa Fungsi Analog Reference Internal?", "id": "Menggunakan Referensi Internal 1.1V." },
  { "en": "Bagaimana Cara Atur Tampilan Point?", "id": "Gunakan Perintah Ddptype." },
  { "en": "Apa Fungsi Register TIMSK0 Pada AVR?", "id": "Masker Interupsi Timer 0." },
  { "en": "Apa Itu Ratnest Mode Di Proteus?", "id": "Menampilkan Garis Koneksi Belum Dirut." },
  { "en": "Apa Shortcut Verify Sketch Arduino?", "id": "Tekan Ctrl + R." },
  { "en": "Apa Fungsi Perintah Scale Reference?", "id": "Skala Objek Berdasarkan Panjang Acuan." },
  { "en": "Apa Fungsi Operator Bitwise XOR (^)?", "id": "Logika Exclusive OR Per Bit." },
  { "en": "Apa Fungsi Pin SCL Arduino?", "id": "Jalur Clock Komunikasi I2C." },
  { "en": "Apa Itu Resistor DIL Network?", "id": "Paket Resistor Dual In-Line." },
  { "en": "Bagaimana Cara Update Data Link?", "id": "Klik Kanan Data Link Update." },
  { "en": "Apa Fungsi Perintah Dimjogged Angle?", "id": "Dimensi Sudut Dengan Pusat Jauh." },
  { "en": "Apa Fungsi Wire EndTransmission()?", "id": "Mengakhiri Transmisi I2C." },
  { "en": "Apa Fungsi Bill Of Materials Proteus?", "id": "Membuat Daftar Komponen Proyek." },
  { "en": "Apa Fungsi Always ON Flag (P_On)?", "id": "Bit Yang Selalu Bernilai 1." },
  { "en": "Bagaimana Cara Insert OLE Object?", "id": "Gunakan Perintah Insertobj." },
  { "en": "Apa Fungsi Perintah Subtract Solid?", "id": "Memotong Volume Objek 3D." },
  { "en": "Mengapa Hatch AutoCAD Tidak Bisa Di-Trim?", "id": "Hatch Tidak Asosiatif Atau Batas Boundary Rusak." },
  { "en": "Solusi Jika Ukuran Dimensi AutoCAD Terlalu Kecil Saat Di-Print?", "id": "Atur Skala Dimensi (DIMSCALE) Sesuai Skala Plot." },
  { "en": "Shortcut Apa Untuk Membuka Parameter Manager AutoCAD?", "id": "Ketik Parameters Lalu Enter." },
  { "en": "Apa Fungsi Variabel Sistem Hpdraworder Pada AutoCAD?", "id": "Mengatur Urutan Tampilan Hatch (Depan/Belakang)." },
  { "en": "Apa Fungsi Variabel Sistem Layereval Pada AutoCAD?", "id": "Mengevaluasi Layer Baru Di Xref." },
  { "en": "Mengapa Simulasi Proteus Error 'Simulation Failed to Start'?", "id": "Konflik Model SPICE Atau Netlist Rusak." },
  { "en": "Apa Fungsi Instruksi NEG (Negate) Pada PLC?", "id": "Mengubah Nilai Positif Menjadi Negatif (2's Complement)." },
  { "en": "Apa Fungsi Instruksi WDT (Watchdog Timer) PLC?", "id": "Mereset Timer Pengawas Agar Tidak Error." },
  { "en": "Komponen Optocoupler 4N35 Di Proteus Berfungsi Sebagai?", "id": "Pemisah Sinyal Input Output Secara Optik." },
  { "en": "Apa Fungsi Perintah Bactionbar Pada AutoCAD?", "id": "Menampilkan Bar Aksi Pada Blok Dinamis." },
  { "en": "Apa Fungsi Macro F() Pada Serial.print Arduino?", "id": "Menyimpan String Di Flash Memory (Hemat SRAM)." },
  { "en": "Bagaimana Cara Mengatur Constraint Bar Display AutoCAD?", "id": "Ubah Variabel Sistem Constraintbardisplay." },
  { "en": "Apa Fungsi Tombol Ctrl Shift U Pada AutoCAD?", "id": "Mengaktifkan Polar Tracking." },
  { "en": "Apa Fungsi Perintah Geomconstraint Pada AutoCAD?", "id": "Menerapkan Kendala Geometri Antar Objek." },
  { "en": "Komponen IC 74HC595 Pin OE (Output Enable) Berfungsi?", "id": "Mengaktifkan Output Jika Logika Low." },
  { "en": "Apa Fungsi Instruksi SQR (Square Root) PLC?", "id": "Menghitung Akar Kuadrat Dari Data." },
  { "en": "Apa Fungsi Instruksi EXP (Exponent) PLC?", "id": "Menghitung Nilai Eksponensial (e Pangkat n)." },
  { "en": "Bagaimana Cara Mengatur Selection Preview AutoCAD?", "id": "Ubah Variabel Sistem Selectionpreview." },
  { "en": "Apa Fungsi Register TIMSK1 Pada Arduino AVR?", "id": "Mengatur Masker Interupsi Timer 1." },
  { "en": "Apa Itu Data Memory Index (IR) PLC?", "id": "Register Index Pointer Alamat Memori." },
  { "en": "Apa Itu Special Relay M8002 PLC Mitsubishi?", "id": "Pulsa Awal (Initial Pulse) Saat Run." },
  { "en": "Shortcut Apa Untuk Upload Using Programmer Arduino?", "id": "Tekan Ctrl + Shift + U." },
  { "en": "Apa Fungsi Library Wire SetClockStretchLimit?", "id": "Mengatur Batas Waktu Clock Stretching I2C." },
  { "en": "Bagaimana Cara Mengatur Auto-Router Proteus?", "id": "Menu Tools Shape Based Auto Router." },
  { "en": "Apa Fungsi Instruksi ASL (Arithmetic Shift Left)?", "id": "Geser Bit Ke Kiri (Kali 2)." },
  { "en": "Apa Fungsi Instruksi ASR (Arithmetic Shift Right)?", "id": "Geser Bit Ke Kanan (Bagi 2, Pertahankan Tanda)." },
  { "en": "Apa Fungsi Perintah Dimjogline Pada AutoCAD?", "id": "Menambahkan Garis Tekuk Pada Dimensi Linear." },
  { "en": "Apa Itu Glitch Pada Sinyal Digital?", "id": "Lonjakan Sinyal Singkat Yang Tidak Diinginkan." },
  { "en": "Apa Itu Race Condition Pada Arduino?", "id": "Hasil Tak Tentu Karena Urutan Eksekusi Salah." },
  { "en": "Shortcut Apa Untuk Find Device GX Works?", "id": "Tekan Ctrl + F." },
  { "en": "Apa Fungsi Register ADCSRA Pada AVR?", "id": "Status Dan Kontrol ADC (Start Conversion)." },
  { "en": "Apa Fungsi Noise Analysis Graph Proteus?", "id": "Analisis Derau Sinyal Dalam Rangkaian." },
  { "en": "Apa Fungsi Timer Retentive On-Delay (RTO)?", "id": "Timer Yang Menyimpan Nilai Saat Power Off." },
  { "en": "Bagaimana Cara Membuat Point Cloud AutoCAD?", "id": "Gunakan Perintah Pointcloudattach." },
  { "en": "Apa Fungsi Operator Bitwise XOR (^)?", "id": "Logika Exclusive OR Per Bit." },
  { "en": "Apa Itu Sub-Sheet Terminal Di Proteus?", "id": "Penghubung Antara Sheet Induk Dan Anak." },
  { "en": "Shortcut Apa Untuk Membuka Visual Styles?", "id": "Ketik Visualstyles Lalu Enter." },
  { "en": "Apa Fungsi Serial Peek() Arduino?", "id": "Melihat Byte Data Tanpa Menghapusnya Dari Buffer." },
  { "en": "Komponen Apa Yang Menghubungkan Pin Reset?", "id": "Resistor Pull-Up Ke VCC." },
  { "en": "Apa Fungsi Instruksi HEX (Ascii to Hex)?", "id": "Konversi Karakter ASCII Ke Nilai Hex." },
  { "en": "Bagaimana Cara Xref Bind Insert Style?", "id": "Gabungkan Xref Tanpa Nama File Induk." },
  { "en": "Apa Itu PROGMEM Array Arduino?", "id": "Simpan Array Data Di Flash Memory." },
  { "en": "Apa Fungsi Electrical Rule Check (ERC) Proteus?", "id": "Memeriksa Kesalahan Logika Sambungan Pin." },
  { "en": "Apa Shortcut PLC Write Mode GX Works?", "id": "Tekan F2." },
  { "en": "Apa Fungsi Perintah Flatshot AutoCAD?", "id": "Proyeksi 2D Datar Dari Objek 3D." },
  { "en": "Apa Fungsi Macro bitClear() Pada Arduino?", "id": "Mengubah Bit Tertentu Menjadi 0." },
  { "en": "Apa Itu Current Controlled Voltage Source?", "id": "Sumber Tegangan Yang Dikendalikan Arus." },
  { "en": "Bagaimana Cara Export Layout Ke PDF?", "id": "Menu Output Export PDF." },
  { "en": "Apa Fungsi Perintah Solidedit Face Taper?", "id": "Miringkan Permukaan Solid Dengan Sudut." },
  { "en": "Apa Tipe Data Double Pada Arduino Uno?", "id": "Sama Dengan Float (32 Bit)." },
  { "en": "Komponen Apa Pemicu Triac?", "id": "Diac Atau Optocoupler Triac." },
  { "en": "Apa Fungsi Instruksi CML (Complement) PLC?", "id": "Membalik Logika Semua Bit (Invert)." },
  { "en": "Shortcut Apa Untuk Membuka Material Browser?", "id": "Ketik Matbrowseropen Lalu Enter." },
  { "en": "Apa Fungsi Keyword Volatile Arduino?", "id": "Mencegah Optimasi Compiler Pada Variabel." },
  { "en": "Apa Itu Resolusi PWM 8 Bit?", "id": "Nilai Duty Cycle 0 Sampai 255." },
  { "en": "Apa Fungsi Instruksi STC (Set Carry)?", "id": "Mengaktifkan Flag Carry Menjadi 1." },
  { "en": "Bagaimana Cara Paste Ke Koordinat Asli?", "id": "Pilih Paste To Original Coordinates." },
  { "en": "Apa Fungsi Register MCUCR Pada AVR?", "id": "Mengatur Kontrol Unit Mikrokontroler (Sleep)." },
  { "en": "Apa Itu Sensor Gas MQ-135 Proteus?", "id": "Sensor Kualitas Udara (Ammonia, CO2)." },
  { "en": "Apa Shortcut Simulation Step GX Works?", "id": "Tekan Shift + F10." },
  { "en": "Apa Fungsi Perintah Solprof AutoCAD?", "id": "Membuat Profil 2D Di Viewport Layout." },
  { "en": "Apa Fungsi Macro bitSet() Pada Arduino?", "id": "Mengubah Bit Tertentu Menjadi 1." },
  { "en": "Komponen Apa Yang Menstabilkan Osilator?", "id": "Kapasitor Keramik (Biasanya 22pF)." },
  { "en": "Apa Fungsi Carry Flag (C) PLC?", "id": "Indikator Sisa (Carry) Operasi Aritmatika." },
  { "en": "Shortcut Apa Untuk Uncomment Code Arduino?", "id": "Tekan Ctrl + Slash." },
  { "en": "Apa Fungsi Serial Println BIN?", "id": "Cetak Nilai Dalam Format Biner." },
  { "en": "Apa Itu Signal Generator Audio Proteus?", "id": "Sumber Sinyal Dari File Audio WAV." },
  { "en": "Apa Fungsi Instruksi WOR (Word OR)?", "id": "Logika OR Tingkat Word (16 Bit)." },
  { "en": "Bagaimana Cara Setting Render Quality AutoCAD?", "id": "Pilih Preset Di Jendela Render." },
  { "en": "Apa Fungsi IsDigit() Pada Arduino?", "id": "Cek Apakah Karakter Adalah Angka." },
  { "en": "Apa Itu I2C Debugger Analyzer Mode?", "id": "Menganalisis Timing Sinyal Data I2C." },
  { "en": "Shortcut Apa Untuk Zoom Extents AutoCAD?", "id": "Klik Dua Kali Roda Mouse (Scroll)." },
  { "en": "Apa Fungsi Perintah Meshextrude AutoCAD?", "id": "Menarik Wajah Mesh Menjadi 3D." },
  { "en": "Apa Fungsi String ToInt() Arduino?", "id": "Konversi String Angka Ke Integer." },
  { "en": "Apa Itu IC 74HC165 Shift Register?", "id": "Parallel In Serial Out (Input Expander)." },
  { "en": "Apa Fungsi Instruksi DI (Disable Interrupt)?", "id": "Mematikan Interupsi Global." },
  { "en": "Bagaimana Cara Ukur Panjang Kurva?", "id": "Gunakan Perintah Lengthen Atau List." },
  { "en": "Apa Fungsi Pin AREF Arduino?", "id": "Referensi Tegangan Eksternal ADC." },
  { "en": "Apa Itu Power Terminal VEE Proteus?", "id": "Sumber Tegangan Negatif (Op-Amp)." },
  { "en": "Shortcut Apa Untuk Hide Palettes AutoCAD?", "id": "Tekan Ctrl + Shift + H." },
  { "en": "Apa Fungsi Perintah Helix Base Radius?", "id": "Mengatur Jari-Jari Dasar Spiral." },
  { "en": "Apa Fungsi Karakter Backslash T?", "id": "Membuat Tabulasi Horizontal." },
  { "en": "Apa Itu Regulator AMS1117?", "id": "Regulator LDO (Low Dropout) 3.3V/5V." },
  { "en": "Apa Fungsi Analog Reference Internal?", "id": "Referensi Tegangan Internal 1.1V." },
  { "en": "Bagaimana Cara Atur Tampilan Point Style?", "id": "Gunakan Perintah Ddptype." },
  { "en": "Apa Fungsi Register EIFR Pada AVR?", "id": "Flag Interupsi Eksternal (Status)." },
  { "en": "Apa Itu Connectivity Check Proteus?", "id": "Memeriksa Integritas Sambungan Netlist." },
  { "en": "Apa Shortcut Verify Sketch Arduino?", "id": "Tekan Ctrl + R." },
  { "en": "Apa Fungsi Perintah Scale Reference?", "id": "Skala Objek Berdasarkan Panjang Referensi." },
  { "en": "Apa Fungsi Operator Bitwise AND (&)?", "id": "Logika AND Per Bit." },
  { "en": "Apa Fungsi Pin SDA Arduino?", "id": "Jalur Data Serial I2C." },
  { "en": "Apa Itu Resistor Network SIP?", "id": "Paket Resistor Single In-Line." },
  { "en": "Bagaimana Cara Update Data Link AutoCAD?", "id": "Klik Kanan Data Link Pilih Update." },
  { "en": "Apa Fungsi Perintah Dimjogged Angle?", "id": "Dimensi Sudut Dengan Pusat Jauh." },
  { "en": "Apa Fungsi Wire Read() Arduino?", "id": "Membaca Byte Data Dari Buffer I2C." },
  { "en": "Apa Fungsi Pick And Place File?", "id": "Data Koordinat X-Y Komponen Untuk Mesin." },
  { "en": "Apa Fungsi Always ON Flag (P_On)?", "id": "Bit Status Selalu ON (High)." },
  { "en": "Bagaimana Cara Insert OLE Object?", "id": "Gunakan Perintah Insertobj." },
  { "en": "Apa Fungsi Perintah Subtract Solid?", "id": "Memotong Volume Objek Solid." },
  { "en": "Mengapa Perintah EXTRUDE AutoCAD Gagal (Cannot Sweep/Extrude)?", "id": "Objek Bukan Polyline Tertutup Atau Ada Garis Menyilang (Self-Intersecting)." },
  { "en": "Apa Fungsi Perintah `pulseIn()` Pada Arduino?", "id": "Mengukur Durasi Pulsa (High/Low) Dalam Mikrosaat." },
  { "en": "Solusi Jika Viewport Layout AutoCAD Kosong/Blank?", "id": "Pastikan Layer Viewport 'On' Dan Tidak Di-Freeze (Thaw)." },
  { "en": "Bedanya `shiftOut` Dan `SPI.transfer`?", "id": "`shiftOut` Software (Lambat, Pin Bebas), SPI Hardware (Cepat, Pin Khusus)." },
  { "en": "Mengapa Simulasi Osiloskop Proteus Hanya Garis Lurus?", "id": "Trigger Level Tidak Sesuai Atau Coupling DC/AC Salah." },
  { "en": "Apa Fungsi Instruksi ALT (Alternate) Pada PLC?", "id": "Mengubah Status Output ON/OFF Setiap Input Ditekan (Toggle)." },
  { "en": "Trik Mengembalikan Menu Klasik AutoCAD?", "id": "Matikan RIBBON, Hidupkan MENUBAR, Aktifkan Toolbar." },
  { "en": "Mengapa `Serial.begin(9600)` Penting?", "id": "Sinkronisasi Kecepatan Data Antara Arduino Dan Komputer." },
  { "en": "Apa Itu 'Zone Compare' (ZCP) Pada PLC?", "id": "Membandingkan Data Apakah Berada Di Dalam Rentang Tertentu." },
  { "en": "Solusi Jika Teks Atribut Blok Tidak Muncul?", "id": "Cek Variabel ATTDISP (Set Ke Normal Atau On)." },
  { "en": "Kapan Menggunakan `PROGMEM` Di Arduino?", "id": "Saat Menyimpan Data Konstan Besar (Array/String) Agar Hemat RAM." },
  { "en": "Mengapa Jalur PCB Proteus Putus Saat Auto-Route?", "id": "Jarak Antar Komponen Terlalu Sempit (Clearance Error)." },
  { "en": "Bedanya `Dimassoc` Nilai 1 Dan 2?", "id": "1 Tidak Asosiatif (Exploded), 2 Asosiatif (Ikut Berubah)." },
  { "en": "Apa Fungsi Instruksi SRET (Subroutine Return) PLC?", "id": "Kembali Ke Program Utama Setelah Subroutine Selesai." },
  { "en": "Cara Menghitung RPM Motor Dengan Arduino?", "id": "Gunakan Interrupt Untuk Menghitung Pulsa Encoder Per Detik." },
  { "en": "Mengapa Garis Tepi (Border) Layout AutoCAD Hilang Saat Plot?", "id": "Garis Berada Di Area 'No Plot' Printer (Margin)." },
  { "en": "Apa Efek Baud Rate Mismatch (Misal 9600 vs 115200)?", "id": "Data Yang Diterima Menjadi Karakter Sampah (Gibberish)." },
  { "en": "Trik Seleksi Cepat Semua Dimensi Di AutoCAD?", "id": "Gunakan QSELECT > Object Type: Dimension." },
  { "en": "Mengapa Relay PLC Tidak Bisa Switching Cepat?", "id": "Kontak Mekanik Punya Batas Kecepatan Dan Usia Pakai." },
  { "en": "Solusi 'Library not found' Di Arduino IDE?", "id": "Cek Folder Documents/Arduino/libraries Atau Install Via Library Manager." },
  { "en": "Apa Itu 'Ghosting' Pada Keypad Matrix?", "id": "Tombol Tertekan Terdeteksi Salah Karena Arus Balik (Butuh Dioda)." },
  { "en": "Mengapa Simulasi Proteus Berhenti Dengan 'Fatal Error'?", "id": "Memori Komputer Habis Atau Model SPICE Tidak Stabil." },
  { "en": "Bedanya Perintah `COPY` Dan `COPYBASE` AutoCAD?", "id": "`COPY` Dalam File Sama, `COPYBASE` Menyalin Ke Clipboard Dengan Titik Acuan." },
  { "en": "Apa Fungsi Register `DDRx` Pada AVR?", "id": "Data Direction Register (Menentukan Pin Input/Output)." },
  { "en": "Mengapa Motor Stepper Panas Saat Diam?", "id": "Arus Holding Torque Terus Mengalir (Normal, Tapi Boros)." },
  { "en": "Solusi Blok Dinamis AutoCAD Tidak Berfungsi?", "id": "File Disimpan Dalam Format Lama Yang Tidak Mendukung Dynamic Block." },
  { "en": "Kapan Menggunakan Transistor Darlington?", "id": "Saat Membutuhkan Penguatan Arus (Gain) Sangat Tinggi." },
  { "en": "Bedanya `MCR` (Master Control Reset) Dan `JMP`?", "id": "`MCR` Mematikan Output Di Zonanya, `JMP` Melewati Instruksi (Output Tetap)." },
  { "en": "Mengapa Sensor Jarak IR (Infrared) Tidak Stabil Di Siang Hari?", "id": "Interferensi Cahaya Matahari (Sinar IR Alami)." },
  { "en": "Trik Memasukkan Koordinat GPS Ke AutoCAD?", "id": "Gunakan GEOLOCATION Atau Import Point File (CSV)." },
  { "en": "Apa Fungsi Kapasitor Pada Tombol Reset?", "id": "Membentuk Rangkaian RC Untuk Reset Otomatis Saat Power On." },
  { "en": "Mengapa 'Generate Netlist' Proteus Gagal?", "id": "Ada Pin Komponen Yang Tumpang Tindih Atau Tidak Terhubung." },
  { "en": "Solusi Mouse Tengah AutoCAD Malah Keluar Menu Snap?", "id": "Ubah Variabel MBUTTONPAN Menjadi 1." },
  { "en": "Apa Itu 'Bit Banging' Di Mikrokontroler?", "id": "Simulasi Protokol Komunikasi Menggunakan Software (Manual Toggle Pin)." },
  { "en": "Mengapa PLC Error 'Cycle Time Over'?", "id": "Program Terlalu Panjang Atau Terjebak Infinite Loop." },
  { "en": "Cara Mengubah Tampilan Kursor Jadi Layar Penuh (Crosshair)?", "id": "Options > Display > Crosshair Size (Set 100)." },
  { "en": "Bedanya `tone()` Dan PWM (`analogWrite`)?", "id": "`tone()` Mengatur Frekuensi (Suara), PWM Mengatur Duty Cycle (Kecerahan/Kecepatan)." },
  { "en": "Mengapa Simulasi LCD Muncul Karakter Jepang/Aneh?", "id": "Inisialisasi ROM Code Salah Atau Data Bit Terbalik." },
  { "en": "Trik Agar Zoom Tidak Menembus Dinding 3D (Walk)?", "id": "Gunakan Perintah 3DWALK Atau 3DFLY." },
  { "en": "Apa Penyebab Arduino Reset Saat Motor Start?", "id": "Lonjakan Arus Awal (Inrush Current) Menurunkan Tegangan Sistem." },
  { "en": "Mengapa Garis Konstruksi (XLINE) Tidak Bisa Di-Trim?", "id": "Bisa Di-Trim, Tapi Harus Memotong Garis Batas Dengan Benar." },
  { "en": "Solusi Error 'Avrdude: stk500_recv(): programmer is not responding'?", "id": "Cek Kabel USB, Port COM, Dan Pilihan Board Di IDE." },
  { "en": "Apa Fungsi 'Schmitt Trigger' Pada Input Digital?", "id": "Membersihkan Sinyal Input Yang Berisik/Noise (Histeresis)." },
  { "en": "Mengapa Hasil Render AutoCAD Gelap Gulita?", "id": "Tidak Ada Sumber Cahaya (Lights) Atau Exposure Salah." },
  { "en": "Bedanya Instruksi `INC` (Increment) Dan `ADD 1`?", "id": "Sama Fungsi, Tapi `INC` Lebih Hemat Memori Dan Cepat." },
  { "en": "Trik Mengisolasi Satu Layer Saja Di AutoCAD?", "id": "Gunakan Perintah LAYISO." },
  { "en": "Apa Fungsi `Internal Pull-up` Pada I2C?", "id": "Menjaga Jalur SDA/SCL High Saat Idle (Wajib Ada)." },
  { "en": "Mengapa Jalur Ground PCB Harus Lebar?", "id": "Mengurangi Impedansi Dan Noise (Ground Plane)." },
  { "en": "Solusi Jika Command Line AutoCAD Hilang?", "id": "Tekan Ctrl + 9." },
  { "en": "Kapan Menggunakan `attachInterrupt` Mode CHANGE?", "id": "Saat Ingin Mendeteksi Awal DAN Akhir Penekanan Tombol/Sensor." },
  { "en": "Bedanya Coil `-|P|-` (Pulse) Dan Coil Biasa?", "id": "Pulse Hanya Aktif Satu Siklus Scan Saat Transisi Input." },
  { "en": "Mengapa Sensor Suhu DS18B20 Bernilai -127?", "id": "Sensor Tidak Terdeteksi (Kabel Putus Atau Resistor Pull-up Hilang)." },
  { "en": "Trik Membuat Garis Miring Sudut Spesifik Di AutoCAD?", "id": "Ketik `<Sudut` (Contoh: `<45`) Saat Menggambar Garis." },
  { "en": "Apa Fungsi 'Watch Window' Di Debugging Proteus?", "id": "Memantau Nilai Variabel Atau Register Secara Real-Time." },
  { "en": "Mengapa File AutoCAD Menjadi 'Read Only'?", "id": "File Sedang Dibuka Orang Lain (Jaringan) Atau Terkunci Windows." },
  { "en": "Solusi Jika Servo Hanya Bergerak Ke Satu Arah?", "id": "Koneksi Ground Terputus Atau Sinyal Kontrol Noise." },
  { "en": "Apa Itu 'Scan Time' Pada PLC?", "id": "Waktu Yang Dibutuhkan Untuk Membaca Input, Proses, Dan Tulis Output." },
  { "en": "Mengapa Hatch AutoCAD Tidak Muncul Di Area Sempit?", "id": "Toleransi Gap (HPPGAPTOL) Terlalu Kecil." },
  { "en": "Bedanya Fungsi `map()` Dan `constrain()`?", "id": "`map` Konversi Rentang Nilai, `constrain` Membatasi Nilai Maks/Min." },
  { "en": "Apa Penyebab Mosfet Panas Berlebih?", "id": "Tegangan Gate Kurang (Tidak Saturasi Penuh) Atau Beban Berlebih." },
  { "en": "Trik Mengubah Teks Kapital Menjadi Huruf Kecil Di AutoCAD?", "id": "Ctrl + Shift + U (Kapital), Ctrl + Shift + L (Kecil) Di Text Editor." },
  { "en": "Mengapa Simulasi Proteus Tidak Real-Time?", "id": "Beban CPU Tinggi (Lihat Indikator CPU Load Di Bawah)." },
  { "en": "Solusi Jika Object Snap (Osnap) Tidak Mau Menempel?", "id": "Tekan F3 Atau Cek Apakah Grid Snap (F9) Aktif Mengganggu." },
  { "en": "Kapan Menggunakan Tipe Data `volatile`?", "id": "Untuk Variabel Yang Diubah Oleh Interrupt Service Routine." },
  { "en": "Bedanya Timer On-Delay Dan Pulse Timer PLC?", "id": "On-Delay Menunggu Waktu, Pulse Timer Langsung Hidup Sesaat." },
  { "en": "Mengapa Output PWM Arduino Tidak Rata (Flicker)?", "id": "Frekuensi PWM Terlalu Rendah Untuk Kamera/Mata Cepat." },
  { "en": "Trik Memotong Garis Di Antara Dua Titik Di AutoCAD?", "id": "Gunakan Perintah BREAK (Pilih Objek, Lalu Titik 1 Dan 2)." },
  { "en": "Apa Fungsi 'Bypass Capacitor' Dekat IC?", "id": "Menyediakan Cadangan Arus Sesaat Dan Membuang Noise Frekuensi Tinggi." },
  { "en": "Mengapa Xref Status 'Unreferenced'?", "id": "File Xref Ada Di Daftar Tapi Tidak Ada Instance Di Gambar." },
  { "en": "Solusi Jika Bootloader Arduino Corrupt?", "id": "Gunakan Arduino Lain Sebagai ISP (In-System Programmer) Untuk Burn Ulang." },
  { "en": "Apa Efek Resistor Basis Transistor Terlalu Besar?", "id": "Arus Basis Kurang, Transistor Tidak Saturasi (Saklar Gagal)." },
  { "en": "Mengapa Perintah OFFSET AutoCAD Gagal?", "id": "Jarak Offset Terlalu Besar Atau Objek Kompleks Self-Intersecting." },
  { "en": "Bedanya `while(1)` Dan `loop()`?", "id": "`while(1)` Loop Tak Terbatas Manual, `loop()` Fungsi Standar Arduino." },
  { "en": "Trik Melihat Urutan Draw Order Di AutoCAD?", "id": "Gunakan Perintah DRAWORDER (Bring to Front/Send to Back)." },
  { "en": "Apa Penyebab LCD I2C Hanya Menyala Backlight?", "id": "Kontras Potensiometer Belum Diatur Putar Obeng Di Belakang." },
  { "en": "Mengapa Output Analog PLC Tidak Keluar Tegangan?", "id": "Power Supply Eksternal Untuk Modul Analog Belum Dipasang." },
  { "en": "Solusi Layer AutoCAD Tidak Bisa Dihapus?", "id": "Layer Masih Dipakai Objek, Blok, Atau Defpoints (Sistem)." },
  { "en": "Kapan Menggunakan `sizeof()`?", "id": "Untuk Mengetahui Ukuran Byte Array Atau Tipe Data Secara Dinamis." },
  { "en": "Bedanya VCC Dan VDD?", "id": "VCC Biasanya Untuk Bipolar (5V), VDD Untuk CMOS (3.3V/5V) - Sering Dianggap Sama." },
  { "en": "Mengapa Simulasi Motor DC Berputar Terbalik?", "id": "Polaritas Sumber Tegangan Atau Koneksi Pin Terbalik." },
  { "en": "Trik Mengubah Sudut Pandang 3D Cepat?", "id": "Gunakan ViewCube Di Pojok Kanan Atas Layar." },
  { "en": "Apa Itu 'Debounce' Pada Tombol?", "id": "Mencegah Pembacaan Ganda Akibat Getaran Mekanis Kontak." },
  { "en": "Mengapa Perintah FILLET Radius 0 Berguna?", "id": "Cara Tercepat Menyambung Dua Garis Menjadi Sudut Lancip." },
  { "en": "Solusi Jika Serial Monitor Muncul Kotak-Kotak?", "id": "Baud Rate Salah (Samakan Dengan Serial.begin)." },
  { "en": "Apa Fungsi 'Ground Plane' Di PCB?", "id": "Area Tembaga Luas Untuk Ground, Mengurangi Noise Dan Interferensi." },
  { "en": "Mengapa PLC Input Aktif Tapi Program Tidak Jalan?", "id": "Lupa Download Program Ke PLC Atau PLC Masih Mode STOP." },
  { "en": "Trik Menghitung Luas Area Arsir (Hatch)?", "id": "Klik Hatch, Buka Properties (Ctrl+1), Lihat Nilai Area." },
  { "en": "Bedanya `High` Dan `Low` Pada Logika Negatif?", "id": "`High` (5V) Berarti OFF, `Low` (0V) Berarti ON (Active Low)." },
  { "en": "Mengapa Arduino Panas Saat Dicolok 12V?", "id": "Regulator Linier Membuang Kelebihan Tegangan Sebagai Panas." },
  { "en": "Solusi Garis Tepi (Viewport) Ikut Ter-Print?", "id": "Pindahkan Garis Viewport Ke Layer 'Defpoints' (Layer Ini Tidak Dicetak)." },
  { "en": "Apa Fungsi Perintah `noTone()`?", "id": "Menghentikan Sinyal PWM Suara Pada Pin Tertentu." },
  { "en": "Mengapa Simulasi Proteus Tidak Bisa Di-Stop?", "id": "Program Terjebak Loop Berat, Tekan Esc Atau Task Manager." },
  { "en": "Trik Membuat Garis Lurus Sempurna Di AutoCAD?", "id": "Tahan Shift Atau Tekan F8 (Ortho Mode)." },
  { "en": "Bedanya `SRAM`, `Flash`, Dan `EEPROM`?", "id": "SRAM (Sementara/Cepat), Flash (Program/Tetap), EEPROM (Data/Tetap)." },
  { "en": "Mengapa Sensor Ultrasonik Error Jarak Dekat?", "id": "Ada Batas Minimum Jarak (Blind Spot) Sekitar 2-3 cm." },
  { "en": "Solusi Ukuran File AutoCAD Membengkak Tiba-Tiba?", "id": "Gunakan Perintah PURGE Untuk Hapus Blok/Layer Tak Terpakai." },
  { "en": "Apa Itu 'Watchdog Timer' (WDT)?", "id": "Sistem Keamanan Yang Mereset Mikrokontroler Jika Program Macet." }


        ];

        let questions = [];

        rawVocabularyList.sort((a, b) => {
            const enA = a.en.toLowerCase();
            const enB = b.en.toLowerCase();
            if (enA < enB) return -1;
            if (enA > enB) return 1;
            return 0;
        });

        function generateQuestions() {
            const allIndonesianTranslations = rawVocabularyList.map(item => item.id);
            questions = [];
            rawVocabularyList.forEach(vocabItem => {
                const correctAnswer = vocabItem.id;
                const distractors = [];
                let attempts = 0;
                while (distractors.length < 3 && attempts < allIndonesianTranslations.length * 2) {
                    const randomIndex = Math.floor(Math.random() * allIndonesianTranslations.length);
                    const potentialDistractor = allIndonesianTranslations[randomIndex];
                    if (potentialDistractor !== correctAnswer && !distractors.includes(potentialDistractor)) {
                        distractors.push(potentialDistractor);
                    }
                    attempts++;
                }
                while (distractors.length < 3) {
                    const fallbackOptions = ["opsi lain A", "opsi lain B", "opsi lain C", "opsi lain D", "opsi lain E", "opsi lain F"];
                    let fallbackIndex = 0;
                    let safetyNet = 0;
                    while(distractors.length < 3 && safetyNet < fallbackOptions.length * 3) {
                        const fbOption = fallbackOptions[fallbackIndex % fallbackOptions.length] + `_${distractors.length}${Math.floor(Math.random()*100)}`;
                        if (fbOption !== correctAnswer && !distractors.includes(fbOption)) {
                             distractors.push(fbOption);
                        }
                        fallbackIndex++;
                        safetyNet++;
                    }
                     if(distractors.length < 3) {
                        for(let i=0; i < (3-distractors.length); i++){
                            distractors.push("pilihan default " + (i+1+distractors.length) + Math.random().toString(36).substring(7));
                        }
                     }
                }
                const answerOptions = [
                    { text: correctAnswer, correct: true },
                    { text: distractors[0], correct: false },
                    { text: distractors[1], correct: false },
                    { text: distractors[2], correct: false }
                ];
                questions.push({
                    question: vocabItem.en,
                    answers: answerOptions
                });
            });
        }

        generateQuestions();

        function saveProgress() {
            if (!questionContainerElement.classList.contains('hide') && orderedQuestions && currentQuestionIndex < orderedQuestions.length) {
                 const progress = {
                    currentQuestionIndex: currentQuestionIndex,
                    score: score,
                    orderedQuestions: orderedQuestions
                };
                localStorage.setItem('quizProgress', JSON.stringify(progress));
            }
        }

        function loadProgress() {
            const savedProgress = localStorage.getItem('quizProgress');
            if (savedProgress) {
                try {
                    const progressData = JSON.parse(savedProgress);
                    if (progressData && typeof progressData.currentQuestionIndex === 'number' &&
                        typeof progressData.score === 'number' && Array.isArray(progressData.orderedQuestions) &&
                        progressData.orderedQuestions.length > 0 &&
                        progressData.currentQuestionIndex < progressData.orderedQuestions.length &&
                        progressData.orderedQuestions.length === questions.length) { // Validasi tambahan: jumlah soal harus sama
                        return progressData;
                    } else {
                        clearProgress();
                        return null;
                    }
                } catch (e) {
                    console.error("Error parsing saved progress:", e);
                    clearProgress();
                    return null;
                }
            }
            return null;
        }

        function clearProgress() {
            localStorage.removeItem('quizProgress');
        }

        prev50Button.addEventListener('click', () => navigateQuestions(-JUMP_AMOUNT));
        prevQuestionButton.addEventListener('click', () => navigateQuestions(-1)); // Event listener untuk tombol baru
        next50Button.addEventListener('click', () => navigateQuestions(JUMP_AMOUNT));

        function navigateQuestions(amount) {
            clearTimeout(questionTimeout);
            if (!orderedQuestions || orderedQuestions.length === 0) return;

            let newIndex = currentQuestionIndex + amount;
            if (newIndex < 0) newIndex = 0;
            else if (newIndex >= orderedQuestions.length) newIndex = orderedQuestions.length - 1;

            if (newIndex !== currentQuestionIndex) {
                currentQuestionIndex = newIndex;
                setNextQuestion();
            } else {
                updateSkipButtonStates();
            }
        }

        function updateSkipButtonStates() {
            if (!orderedQuestions || orderedQuestions.length === 0 || questionContainerElement.classList.contains('hide')) {
                skipNavigationControls.classList.add('hide');
                if(prev50Button) prev50Button.disabled = true;
                if(prevQuestionButton) prevQuestionButton.disabled = true; // Nonaktifkan tombol baru
                if(next50Button) next50Button.disabled = true;
                return;
            }
            skipNavigationControls.classList.remove('hide');
            const isFirstQuestion = currentQuestionIndex === 0;
            const isLastQuestion = currentQuestionIndex === (orderedQuestions.length - 1);

            if(prev50Button) prev50Button.disabled = isFirstQuestion;
            if(prevQuestionButton) prevQuestionButton.disabled = isFirstQuestion; // Atur status disabled tombol baru
            if(next50Button) next50Button.disabled = isLastQuestion;

            if (orderedQuestions.length <= 1) {
                if(prev50Button) prev50Button.disabled = true;
                if(prevQuestionButton) prevQuestionButton.disabled = true; // Atur status disabled tombol baru
                if(next50Button) next50Button.disabled = true;
            }
        }


        window.addEventListener('load', () => {
            const savedData = loadProgress();
            startButton.innerText = 'Mulai';
            completionMessageElement.classList.add('hide');
            if (savedData) {
                continueButton.classList.remove('hide');
            } else {
                continueButton.classList.add('hide');
            }
            if (questionContainerElement.classList.contains('hide')) {
                initialControls.classList.remove('hide');
                skipNavigationControls.classList.add('hide');
            } else {
                 initialControls.classList.add('hide');
                 // Mungkin juga perlu updateSkipButtonStates() di sini jika kuis dilanjutkan
                 // dan langsung menampilkan soal.
            }
        });

        startButton.addEventListener('click', () => startGame(false));
        continueButton.addEventListener('click', () => startGame(true));

        function startGame(isContinuing = false) {
            clearTimeout(questionTimeout);
            completionMessageElement.classList.add('hide');
            if (!isContinuing) {
                startButton.innerText = 'Mulai';
            }
            initialControls.classList.add('hide');
            questionContainerElement.classList.remove('hide');
            questionCounterElement.classList.remove('hide');

            const savedData = loadProgress();
            if (isContinuing && savedData && savedData.orderedQuestions && savedData.orderedQuestions.length === questions.length) {
                orderedQuestions = savedData.orderedQuestions;
                currentQuestionIndex = savedData.currentQuestionIndex;
                score = savedData.score;
            } else {
                clearProgress();
                orderedQuestions = [...questions];
                currentQuestionIndex = 0;
                score = 0;
            }

            if (!orderedQuestions || orderedQuestions.length === 0) {
                showResults();
                completionMessageElement.innerText = "Tidak ada soal untuk ditampilkan.";
                completionMessageElement.style.color = "#dc3545";
                completionMessageElement.classList.remove('hide');
                startButton.innerText = 'Mulai';
                return;
            }
            setNextQuestion();
        }

        function setNextQuestion() {
            resetState();
            if (orderedQuestions && currentQuestionIndex < orderedQuestions.length) {
                questionCounterElement.innerText = `${currentQuestionIndex + 1} / ${orderedQuestions.length}`;
                showQuestion(orderedQuestions[currentQuestionIndex]);
                saveProgress();
                if (document.activeElement && typeof document.activeElement.blur === 'function') {
                    document.activeElement.blur();
                }
            } else {
                showResults();
            }
            updateSkipButtonStates(); // Panggil di sini untuk memastikan state tombol selalu update
        }

        function showQuestion(questionData) {
            questionElement.innerText = questionData.question;
            answerButtonsElement.innerHTML = '';
            const shuffledAnswers = [...questionData.answers].sort(() => Math.random() - 0.5);
            shuffledAnswers.forEach(answer => {
                const button = document.createElement('button');
                button.innerText = answer.text;
                button.classList.add('btn');
                if (answer.correct) {
                    button.dataset.correct = answer.correct;
                }
                button.addEventListener('click', selectAnswer);
                answerButtonsElement.appendChild(button);
            });
        }

        function resetState() {
            clearTimeout(questionTimeout);
            while (answerButtonsElement.firstChild) {
                answerButtonsElement.removeChild(answerButtonsElement.firstChild);
            }
        }

        function selectAnswer(e) {
            const selectedButton = e.target;
            const correct = selectedButton.dataset.correct === 'true';
            if (correct) { score++; }
            Array.from(answerButtonsElement.children).forEach(button => {
                setStatusClass(button, button.dataset.correct === 'true');
                button.disabled = true;
            });
            saveProgress();
            questionTimeout = setTimeout(() => {
                if (orderedQuestions && currentQuestionIndex < orderedQuestions.length -1) {
                    currentQuestionIndex++;
                    setNextQuestion();
                } else if (orderedQuestions && currentQuestionIndex === orderedQuestions.length -1) {
                    showResults();
                }
            }, 7000);
        }

        function setStatusClass(element, correct) {
            clearStatusClass(element);
            if (correct) { element.classList.add('correct'); }
            else { element.classList.add('wrong'); }
        }

        function clearStatusClass(element) {
            element.classList.remove('correct');
            element.classList.remove('wrong');
        }

        function showResults() {
            clearTimeout(questionTimeout);
            questionContainerElement.classList.add('hide');
            questionCounterElement.classList.add('hide');
            skipNavigationControls.classList.add('hide');
            clearProgress();
            completionMessageElement.innerText = "Selamat Kuis Sudah Selesai 🎉";
            completionMessageElement.style.color = "#28a745";
            completionMessageElement.classList.remove('hide');
            startButton.innerText = 'Ulangi Kuis';
            initialControls.classList.remove('hide');
            continueButton.classList.add('hide');
        }
    </script>
</body>
</html>
