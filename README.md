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


  { "en": "Apa Fungsi Utama Dari Resistor?", "id": "Untuk Menghambat Aliran Arus Listrik." },
  { "en": "Apa Satuan Dari Nilai Kapasitansi?", "id": "Satuan Kapasitansi Adalah Farad." },
  { "en": "Apa Fungsi Dari Komponen Dioda?", "id": "Sebagai Penyearah Arus Listrik." },
  { "en": "Bagaimana Bunyi Dari Hukum Ohm?", "id": "Tegangan Berbanding Lurus Dengan Arus." },
  { "en": "Apa Kepanjangan Dari LED?", "id": "Light Emitting Diode." },
  { "en": "Apa Fungsi Utama Dari Transistor?", "id": "Sebagai Saklar Dan Penguat Sinyal." },
  { "en": "Apa Nama Lain Arus Searah?", "id": "Arus DC Atau Direct Current." },
  { "en": "Apa Nama Lain Arus Bolak-Balik?", "id": "Arus AC Atau Alternating Current." },
  { "en": "Apa Fungsi Dari Sebuah Kapasitor?", "id": "Menyimpan Muatan Energi Listrik Sementara." },
  { "en": "Apa Satuan Untuk Hambatan Listrik?", "id": "Satuan Hambatan Adalah Ohm." },
  { "en": "Apa Itu Sinyal Analog?", "id": "Sinyal Kontinu Yang Berubah Waktu." },
  { "en": "Apa Itu Sinyal Digital?", "id": "Sinyal Diskrit Dengan Level Tertentu." },
  { "en": "Apa Fungsi Dari IC Regulator?", "id": "Untuk Menstabilkan Tegangan Output." },
  { "en": "Apa Satuan Dari Frekuensi?", "id": "Satuan Frekuensi Adalah Hertz." },
  { "en": "Apa Fungsi Utama Dari Induktor?", "id": "Menyimpan Energi Dalam Medan Magnet." },
  { "en": "Apa Satuan Untuk Induktansi?", "id": "Satuan Induktansi Adalah Henry." },
  { "en": "Disebut Apakah Gabungan Resistor Seri?", "id": "Hambatan Totalnya Adalah Penjumlahan." },
  { "en": "Apa Fungsi Alat Ukur Multimeter?", "id": "Mengukur Tegangan Arus Dan Hambatan." },
  { "en": "Apa Nama Kaki Positif Dioda?", "id": "Kaki Positif Dioda Adalah Anoda." },
  { "en": "Apa Nama Kaki Negatif Dioda?", "id": "Kaki Negatif Dioda Adalah Katoda." },
  { "en": "Apa Itu Gerbang Logika NOT?", "id": "Gerbang Yang Membalikkan Logika Input." },
  { "en": "Apa Itu Gerbang Logika AND?", "id": "Output Tinggi Jika Semua Input Tinggi." },
  { "en": "Apa Itu Gerbang Logika OR?", "id": "Output Tinggi Jika Salah Satu Input Tinggi." },
  { "en": "Apa Fungsi Osiloskop Dalam Elektronika?", "id": "Menampilkan Bentuk Gelombang Sinyal Listrik." },
  { "en": "Apa Itu Printed Circuit Board?", "id": "Papan Sirkuit Tempat Komponen Terpasang." },
  { "en": "Apa Kepanjangan Dari PCB?", "id": "Printed Circuit Board." },
  { "en": "Apa Bahan Utama Pembuat Semikonduktor?", "id": "Bahan Utamanya Adalah Silikon." },
  { "en": "Sebutkan Tiga Kaki Pada Transistor?", "id": "Basis Kolektor Dan Emitor." },
  { "en": "Apa Fungsi Transformator Step-Down?", "id": "Untuk Menurunkan Tegangan AC." },
  { "en": "Apa Fungsi Transformator Step-Up?", "id": "Untuk Menaikkan Tegangan AC." },
  { "en": "Apa Yang Dimaksud Dengan Ground?", "id": "Titik Referensi Tegangan Nol Volt." },
  { "en": "Apa Fungsi Dari Sebuah Sekering?", "id": "Melindungi Rangkaian Dari Arus Berlebih." },
  { "en": "Apa Itu Rangkaian Terintegrasi?", "id": "Komponen Elektronik Dalam Satu Chip." },
  { "en": "Apa Kepanjangan Dari IC?", "id": "Integrated Circuit Atau Rangkaian Terpadu." },
  { "en": "Apa Itu Potensiometer?", "id": "Resistor Yang Nilainya Dapat Diatur." },
  { "en": "Apa Fungsi Dari LDR?", "id": "Sensor Cahaya Berbasis Resistansi." },
  { "en": "Apa Kepanjangan Dari LDR?", "id": "Light Dependent Resistor." },
  { "en": "Apa Itu Frekuensi Resonansi?", "id": "Frekuensi Getaran Alami Suatu Sistem." },
  { "en": "Apa Fungsi Dari Sebuah Relay?", "id": "Saklar Yang Dikendalikan Secara Elektromagnetik." },
  { "en": "Apa Itu Dioda Zener?", "id": "Dioda Penstabil Tegangan Searah." },
  { "en": "Apa Itu Rangkaian Paralel?", "id": "Komponen Dengan Titik Awal Sama." },
  { "en": "Apa Itu Rangkaian Seri?", "id": "Komponen Terhubung Ujung Ke Ujung." },
  { "en": "Apa Fungsi Dari Sebuah Filter?", "id": "Melewatkan Frekuensi Yang Diinginkan." },
  { "en": "Apa Itu High Pass Filter?", "id": "Menyaring Frekuensi Rendah." },
  { "en": "Apa Itu Low Pass Filter?", "id": "Menyaring Frekuensi Tinggi." },
  { "en": "Apa Satuan Dari Daya Listrik?", "id": "Satuan Daya Listrik Adalah Watt." },
  { "en": "Apa Rumus Dasar Daya Listrik?", "id": "Daya Adalah Tegangan Dikali Arus." },
  { "en": "Apa Itu Konduktor?", "id": "Bahan Yang Mudah Menghantarkan Listrik." },
  { "en": "Apa Itu Isolator?", "id": "Bahan Yang Sulit Menghantarkan Listrik." },
  { "en": "Apa Itu Semikonduktor?", "id": "Bahan Diantara Konduktor Dan Isolator." },
  { "en": "Apa Fungsi Dari Sebuah Saklar?", "id": "Memutus Dan Menghubungkan Arus Listrik." },
  { "en": "Apa Itu Osilator?", "id": "Rangkaian Pembangkit Sinyal Periodik." },
  { "en": "Apa Itu Amplifier?", "id": "Rangkaian Untuk Menguatkan Sinyal Lemah." },
  { "en": "Apa Itu Tegangan Listrik?", "id": "Perbedaan Potensial Antara Dua Titik." },
  { "en": "Apa Satuan Dari Tegangan Listrik?", "id": "Satuan Tegangan Listrik Adalah Volt." },
  { "en": "Apa Itu Arus Listrik?", "id": "Aliran Muatan Listrik Dalam Rangkaian." },
  { "en": "Apa Satuan Dari Arus Listrik?", "id": "Satuan Arus Listrik Adalah Ampere." },
  { "en": "Apa Bunyi Hukum Kirchhoff Pertama?", "id": "Jumlah Arus Masuk Sama Dengan Keluar." },
  { "en": "Apa Nama Lain Hukum Kirchhoff Pertama?", "id": "Hukum Arus Kirchhoff Atau KCL." },
  { "en": "Apa Itu Sensor?", "id": "Perangkat Pendeteksi Perubahan Fisik." },
  { "en": "Apa Itu Aktuator?", "id": "Perangkat Penggerak Berdasarkan Sinyal Listrik." },
  { "en": "Apa Fungsi Dari Solder?", "id": "Melelehkan Timah Untuk Menyatukan Komponen." },
  { "en": "Apa Itu Doping Pada Semikonduktor?", "id": "Proses Penambahan Ketidakmurnian Pada Silikon." },
  { "en": "Apa Itu Semikonduktor Tipe-N?", "id": "Semikonduktor Dengan Kelebihan Elektron." },
  { "en": "Apa Itu Semikonduktor Tipe-P?", "id": "Semikonduktor Dengan Kelebihan Lubang." },
  { "en": "Apa Fungsi Dari Jembatan Dioda?", "id": "Mengubah Tegangan AC Menjadi DC." },
  { "en": "Apa Itu Noise Dalam Sinyal?", "id": "Gangguan Yang Tidak Diinginkan." },
  { "en": "Apa Itu Bandwidth?", "id": "Rentang Frekuensi Operasi Suatu Sistem." },
  { "en": "Apa Fungsi Dari Heatsink?", "id": "Menyerap Dan Melepas Panas Komponen." },
  { "en": "Apa Itu Mikrokontroler?", "id": "Komputer Kecil Dalam Satu Chip." },
  { "en": "Apa Itu Gerbang Logika NAND?", "id": "Inversi Dari Gerbang Logika AND." },
  { "en": "Apa Itu Gerbang Logika NOR?", "id": "Inversi Dari Gerbang Logika OR." },
  { "en": "Apa Itu Gerbang Logika XOR?", "id": "Output Tinggi Jika Input Berbeda." },
  { "en": "Apa Itu Flip-Flop?", "id": "Rangkaian Penyimpan Satu Bit Data." },
  { "en": "Apa Itu Rectifier?", "id": "Rangkaian Penyearah Gelombang AC." },
  { "en": "Apa Fungsi Dari Crystal Oscillator?", "id": "Menghasilkan Frekuensi Clock Yang Stabil." },
  { "en": "Apa Itu Impedansi?", "id": "Hambatan Total Rangkaian Pada Sinyal AC." },
  { "en": "Apa Satuan Dari Impedansi?", "id": "Satuan Impedansi Adalah Ohm." },
  { "en": "Apa Itu Kapasitor Variabel?", "id": "Kapasitor Yang Nilainya Bisa Diubah." },
  { "en": "Apa Itu Trimpot?", "id": "Potensiometer Ukuran Kecil Untuk Kalibrasi." },
  { "en": "Apa Itu Catu Daya?", "id": "Sumber Energi Listrik Untuk Rangkaian." },
  { "en": "Apa Itu Voltmeter?", "id": "Alat Untuk Mengukur Nilai Tegangan." },
  { "en": "Apa Itu Amperemeter?", "id": "Alat Untuk Mengukur Nilai Arus." },
  { "en": "Apa Itu Ohmmeter?", "id": "Alat Untuk Mengukur Nilai Resistansi." },
  { "en": "Apa Itu Anoda?", "id": "Elektroda Tempat Terjadinya Oksidasi." },
  { "en": "Apa Itu Katoda?", "id": "Elektroda Tempat Terjadinya Reduksi." },
  { "en": "Apa Itu Elektrolit?", "id": "Zat Yang Menghantarkan Listrik." },
  { "en": "Apa Itu Modulasi?", "id": "Proses Penumpangan Sinyal Informasi." },
  { "en": "Apa Itu Demodulasi?", "id": "Proses Pemisahan Sinyal Informasi." },
  { "en": "Apa Kepanjangan Dari AM?", "id": "Amplitude Modulation Atau Modulasi Amplitudo." },
  { "en": "Apa Kepanjangan Dari FM?", "id": "Frequency Modulation Atau Modulasi Frekuensi." },
  { "en": "Apa Itu Sistem Embedded?", "id": "Sistem Komputer Khusus Dalam Perangkat." },
  { "en": "Apa Itu Antena?", "id": "Perangkat Pengubah Sinyal Listrik." },
  { "en": "Apa Fungsi Utama Dari Speaker?", "id": "Mengubah Sinyal Listrik Menjadi Suara." },
  { "en": "Apa Fungsi Dari Mikrofon?", "id": "Mengubah Sinyal Suara Menjadi Listrik." },
  { "en": "Apa Itu Kode Warna Resistor?", "id": "Sistem Penandaan Nilai Resistor." },
  { "en": "Apa Itu Forward Bias?", "id": "Pemberian Tegangan Maju Pada Dioda." },
  { "en": "Apa Kepanjangan Dari Op-Amp?", "id": "Operational Amplifier Atau Penguat Operasional." },
  { "en": "Apa Fungsi Dari Rangkaian Clipper?", "id": "Memotong Sebagian Level Sinyal." },
  { "en": "Apa Fungsi Dari Rangkaian Clamper?", "id": "Menggeser Level DC Suatu Sinyal." },
  { "en": "Apa Yang Dimaksud Dengan Gain?", "id": "Tingkat Penguatan Sinyal Suatu Rangkaian." },
  { "en": "Apa Fungsi Dari Komponen ADC?", "id": "Mengubah Sinyal Analog Ke Digital." },
  { "en": "Apa Fungsi Dari Komponen DAC?", "id": "Mengubah Sinyal Digital Ke Analog." },
  { "en": "Sebutkan Dua Mode IC 555?", "id": "Astable Dan Monostable." },
  { "en": "Apa Itu Mode Astable?", "id": "Mode Osilator Tanpa Pemicu Eksternal." },
  { "en": "Apa Itu Mode Monostable?", "id": "Mode Pembangkit Pulsa Tunggal." },
  { "en": "Apa Itu Ripple Pada Tegangan DC?", "id": "Sisa Tegangan AC Yang Tidak Rata." },
  { "en": "Apa Kepanjangan Dari SCR?", "id": "Silicon Controlled Rectifier." },
  { "en": "Apa Fungsi Utama Komponen SCR?", "id": "Sebagai Saklar Elektronik Daya Tinggi." },
  { "en": "Apa Kepanjangan Dari TRIAC?", "id": "Triode For Alternating Current." },
  { "en": "Apa Fungsi Dari Sebuah Optocoupler?", "id": "Mengisolasi Dua Rangkaian Secara Optik." },
  { "en": "Apa Itu Rangkaian Voltage Divider?", "id": "Rangkaian Pembagi Tegangan Dari Sumber." },
  { "en": "Apa Itu Duty Cycle?", "id": "Persentase Sinyal Aktif Dalam Periode." },
  { "en": "Apa Itu Band-Pass Filter?", "id": "Filter Yang Meloloskan Pita Frekuensi." },
  { "en": "Apa Itu Band-Stop Filter?", "id": "Filter Yang Menolak Pita Frekuensi." },
  { "en": "Apa Fungsi Dari Multiplexer?", "id": "Memilih Satu Input Ke Satu Output." },
  { "en": "Apa Fungsi Dari Demultiplexer?", "id": "Meneruskan Satu Input Ke Banyak Output." },
  { "en": "Apa Kepanjangan Dari MUX?", "id": "Multiplexer." },
  { "en": "Apa Kepanjangan Dari DEMUX?", "id": "Demultiplexer." },
  { "en": "Apa Itu Photodioda?", "id": "Dioda Yang Peka Terhadap Cahaya." },
  { "en": "Apa Itu Transistor Foto?", "id": "Transistor Yang Dikendalikan Oleh Cahaya." },
  { "en": "Apa Itu Register Geser?", "id": "Register Yang Menggeser Data Bit." },
  { "en": "Apa Itu Counter Dalam Digital?", "id": "Rangkaian Pencacah Pulsa Digital." },
  { "en": "Apa Itu Slew Rate?", "id": "Kecepatan Perubahan Output Maksimal Op-Amp." },
  { "en": "Apa Itu Common Cathode?", "id": "Konfigurasi Kaki Katoda Terhubung Bersama." },
  { "en": "Apa Itu Common Anode?", "id": "Konfigurasi Kaki Anoda Terhubung Bersama." },
  { "en": "Apa Fungsi Schmitt Trigger?", "id": "Mengubah Sinyal Analog Menjadi Digital." },
  { "en": "Apa Itu Histeresis?", "id": "Perbedaan Level Tegangan Pemicuan." },
  { "en": "Apa Itu Thermistor?", "id": "Resistor Yang Nilainya Peka Suhu." },
  { "en": "Apa Kepanjangan Dari NTC?", "id": "Negative Temperature Coefficient." },
  { "en": "Apa Kepanjangan Dari PTC?", "id": "Positive Temperature Coefficient." },
  { "en": "Apa Sifat Dari Termistor NTC?", "id": "Resistansi Turun Jika Suhu Naik." },
  { "en": "Apa Sifat Dari Termistor PTC?", "id": "Resistansi Naik Jika Suhu Naik." },
  { "en": "Apa Itu Catu Daya Linier?", "id": "Catu Daya Dengan Regulator Linier." },
  { "en": "Apa Itu SMPS?", "id": "Catu Daya Berbasis Saklar Frekuensi Tinggi." },
  { "en": "Apa Kepanjangan Dari SMPS?", "id": "Switching Mode Power Supply." },
  { "en": "Apa Itu Bistable Multivibrator?", "id": "Nama Lain Dari Rangkaian Flip-Flop." },
  { "en": "Apa Itu Latch?", "id": "Elemen Penyimpan Data Dasar." },
  { "en": "Apa Itu Varistor?", "id": "Resistor Yang Bergantung Pada Tegangan." },
  { "en": "Apa Fungsi Utama Varistor?", "id": "Melindungi Rangkaian Dari Lonjakan Tegangan." },
  { "en": "Apa Itu Open Loop Gain?", "id": "Penguatan Op-Amp Tanpa Umpan Balik." },
  { "en": "Apa Itu Closed Loop Gain?", "id": "Penguatan Op-Amp Dengan Umpan Balik." },
  { "en": "Apa Satuan Tingkat Kebisingan?", "id": "Satuan Kebisingan Adalah Desibel (dB)." },
  { "en": "Apa Itu Fourier Transform?", "id": "Analisis Sinyal Dalam Domain Frekuensi." },
  { "en": "Apa Itu Logic Probe?", "id": "Alat Uji Level Logika Digital." },
  { "en": "Apa Itu Level Logika TTL?", "id": "Level Tegangan Standar Transistor-Transistor Logic." },
  { "en": "Apa Kepanjangan Dari TTL?", "id": "Transistor-Transistor Logic." },
  { "en": "Apa Itu Level Logika CMOS?", "id": "Level Tegangan Standar IC CMOS." },
  { "en": "Apa Kepanjangan Dari CMOS?", "id": "Complementary Metal-Oxide-Semiconductor." },
  { "en": "Apa Itu Pull-Up Resistor?", "id": "Memastikan Input Ke Level Logika Tinggi." },
  { "en": "Apa Itu Pull-Down Resistor?", "id": "Memastikan Input Ke Level Logika Rendah." },
  { "en": "Apa Itu Rise Time?", "id": "Waktu Sinyal Naik Dari Rendah Tinggi." },
  { "en": "Apa Itu Fall Time?", "id": "Waktu Sinyal Turun Dari Tinggi Rendah." },
  { "en": "Apa Itu Osilator RC?", "id": "Osilator Yang Menggunakan Resistor Kapasitor." },
  { "en": "Apa Itu Osilator LC?", "id": "Osilator Yang Menggunakan Induktor Kapasitor." },
  { "en": "Apa Itu Common Emitter Amplifier?", "id": "Konfigurasi Penguat Dengan Emitor Dibumikan." },
  { "en": "Apa Itu Common Collector Amplifier?", "id": "Konfigurasi Penguat Dengan Kolektor Dibumikan." },
  { "en": "Apa Itu Common Base Amplifier?", "id": "Konfigurasi Penguat Dengan Basis Dibumikan." },
  { "en": "Apa Itu Inverting Amplifier?", "id": "Penguat Yang Membalik Fasa Sinyal." },
  { "en": "Apa Itu Non-Inverting Amplifier?", "id": "Penguat Yang Tidak Membalik Fasa." },
  { "en": "Apa Itu Short Circuit?", "id": "Hubungan Singkat Dengan Resistansi Nol." },
  { "en": "Apa Itu Open Circuit?", "id": "Rangkaian Terbuka Tanpa Aliran Arus." },
  { "en": "Apa Itu Crowbar Circuit?", "id": "Rangkaian Proteksi Tegangan Berlebih." },
  { "en": "Apa Itu Decoupling Capacitor?", "id": "Kapasitor Untuk Menghilangkan Noise Catu Daya." },
  { "en": "Apa Itu Feedback?", "id": "Pengambilan Sebagian Output Ke Input." },
  { "en": "Apa Itu Umpan Balik Positif?", "id": "Feedback Yang Menguatkan Sinyal Asli." },
  { "en": "Apa Itu Umpan Balik Negatif?", "id": "Feedback Yang Melemahkan Sinyal Asli." },
  { "en": "Apa Itu Attenuator?", "id": "Rangkaian Untuk Melemahkan Sinyal." },
  { "en": "Apa Itu Dioda Schottky?", "id": "Dioda Dengan Tegangan Maju Rendah." },
  { "en": "Apa Itu Dioda Varactor?", "id": "Dioda Dengan Kapasitansi Yang Bervariasi." },
  { "en": "Apa Itu Logic Analyzer?", "id": "Alat Ukur Untuk Menganalisis Sinyal Digital." },
  { "en": "Apa Itu Bit Rate?", "id": "Jumlah Bit Yang Ditransfer Per Detik." },
  { "en": "Apa Itu Baud Rate?", "id": "Jumlah Simbol Yang Ditransfer Per Detik." },
  { "en": "Apa Itu Komunikasi Serial?", "id": "Pengiriman Data Satu Bit Sekaligus." },
  { "en": "Apa Itu Komunikasi Paralel?", "id": "Pengiriman Data Beberapa Bit Sekaligus." },
  { "en": "Apa Itu Microfarad?", "id": "Seperjuta Farad." },
  { "en": "Apa Itu Picofarad?", "id": "Sepertriliun Farad." },
  { "en": "Apa Itu Nanohenry?", "id": "Sepermiliar Henry." },
  { "en": "Apa Itu Kiloohm?", "id": "Seribu Ohm." },
  { "en": "Apa Itu Megaohm?", "id": "Satu Juta Ohm." },
  { "en": "Apa Itu Phase Shift?", "id": "Pergeseran Fasa Antara Dua Sinyal." },
  { "en": "Apa Itu Solder Wick?", "id": "Serabut Tembaga Penyerap Timah Solder." },
  { "en": "Apa Itu Solder Sucker?", "id": "Alat Penyedot Timah Solder Cair." },
  { "en": "Apa Itu Harmonik?", "id": "Frekuensi Kelipatan Dari Frekuensi Fundamental." },
  { "en": "Apa Itu Latch-Up?", "id": "Kondisi Hubung Singkat Pada IC CMOS." },
  { "en": "Apa Itu Power Amplifier?", "id": "Penguat Akhir Untuk Menggerakkan Beban." },
  { "en": "Apa Itu Preamplifier?", "id": "Penguat Awal Untuk Sinyal Sangat Lemah." },
  { "en": "Apa Itu Full-Wave Rectifier?", "id": "Penyearah Gelombang Penuh Tegangan AC." },
  { "en": "Apa Itu Half-Wave Rectifier?", "id": "Penyearah Setengah Gelombang Tegangan AC." },
  { "en": "Apa Itu Buffer?", "id": "Rangkaian Penguat Dengan Gain Satu." },
  { "en": "Apa Itu Datasheet?", "id": "Dokumen Spesifikasi Teknis Suatu Komponen." },
  { "en": "Apa Itu Breadboard?", "id": "Papan Untuk Merangkai Prototipe Sirkuit." },
  { "en": "Apa Kepanjangan Dari PWM?", "id": "Pulse Width Modulation." },
  { "en": "Apa Fungsi Sinyal PWM?", "id": "Mengatur Daya Dengan Mengubah Lebar Pulsa." },
  { "en": "Apa Itu Central Processing Unit?", "id": "Otak Utama Dari Sebuah Sistem Komputer." },
  { "en": "Apa Kepanjangan Dari CPU?", "id": "Central Processing Unit." },
  { "en": "Apa Kepanjangan Dari RAM?", "id": "Random Access Memory." },
  { "en": "Apa Sifat Dari RAM?", "id": "Memori Volatile Yang Datanya Hilang." },
  { "en": "Apa Kepanjangan Dari ROM?", "id": "Read-Only Memory." },
  { "en": "Apa Sifat Dari ROM?", "id": "Memori Non-Volatile Yang Datanya Tetap." },
  { "en": "Apa Kepanjangan Dari GPIO?", "id": "General Purpose Input Output." },
  { "en": "Apa Fungsi Dari Pin GPIO?", "id": "Pin Serbaguna Untuk Input Dan Output." },
  { "en": "Apa Kepanjangan Dari UART?", "id": "Universal Asynchronous Receiver-Transmitter." },
  { "en": "Apa Kepanjangan Dari SPI?", "id": "Serial Peripheral Interface." },
  { "en": "Apa Kepanjangan Dari I2C?", "id": "Inter-Integrated Circuit." },
  { "en": "Apa Itu Firmware?", "id": "Perangkat Lunak Tertanam Pada Perangkat Keras." },
  { "en": "Apa Itu Bootloader?", "id": "Program Awal Pemuat Sistem Operasi." },
  { "en": "Apa Kepanjangan Dari MOSFET?", "id": "Metal-Oxide-Semiconductor Field-Effect Transistor." },
  { "en": "Apa Keunggulan Utama Dari MOSFET?", "id": "Impedansi Input Sangat Tinggi." },
  { "en": "Apa Kepanjangan Dari JFET?", "id": "Junction Field-Effect Transistor." },
  { "en": "Apa Itu Rangkaian Jembatan Wheatstone?", "id": "Mengukur Nilai Resistansi Tidak Diketahui." },
  { "en": "Apa Itu Transduser?", "id": "Mengubah Satu Bentuk Energi Ke Lainnya." },
  { "en": "Apa Fungsi Dari Sensor Hall Effect?", "id": "Mendeteksi Keberadaan Medan Magnet." },
  { "en": "Apa Fungsi Sensor Ultrasonik?", "id": "Mengukur Jarak Menggunakan Gelombang Suara." },
  { "en": "Apa Fungsi Sensor Inframerah Pasif?", "id": "Mendeteksi Gerakan Berbasis Panas Tubuh." },
  { "en": "Apa Kepanjangan Dari PIR?", "id": "Passive Infrared." },
  { "en": "Apa Itu Propagasi Sinyal?", "id": "Cara Sinyal Bergerak Melalui Medium." },
  { "en": "Apa Fungsi Dari Spectrum Analyzer?", "id": "Menganalisis Spektrum Frekuensi Sinyal." },
  { "en": "Apa Fungsi Dari Function Generator?", "id": "Menghasilkan Berbagai Bentuk Gelombang Uji." },
  { "en": "Apa Itu Sampling Rate?", "id": "Seberapa Cepat Sinyal Analog Diukur." },
  { "en": "Apa Itu Efek Aliasing?", "id": "Distorsi Akibat Sampling Rate Rendah." },
  { "en": "Apa Itu Kuantisasi?", "id": "Proses Pemetaan Sampel Ke Nilai Diskrit." },
  { "en": "Apa Itu Rangkaian Darlington Pair?", "id": "Kombinasi Dua Transistor Untuk Gain Tinggi." },
  { "en": "Apa Itu Current Mirror?", "id": "Rangkaian Untuk Menyalin Arus Listrik." },
  { "en": "Apa Itu Sistem Bilangan Biner?", "id": "Sistem Angka Berbasis Dua." },
  { "en": "Apa Itu Sistem Bilangan Heksadesimal?", "id": "Sistem Angka Berbasis Enam Belas." },
  { "en": "Apa Kegunaan Utama Dioda Zener?", "id": "Sebagai Referensi Atau Regulator Tegangan." },
  { "en": "Apa Keuntungan Dioda Schottky?", "id": "Waktu Switching Sangat Cepat." },
  { "en": "Apa Fungsi Bypass Capacitor?", "id": "Menyaring Noise Frekuensi Tinggi." },
  { "en": "Apa Kepanjangan Dari ESR?", "id": "Equivalent Series Resistance." },
  { "en": "Apa Efek Dari ESR Kapasitor?", "id": "Menyebabkan Pemanasan Dan Kehilangan Daya." },
  { "en": "Apa Itu Fan-Out?", "id": "Jumlah Input Yang Dapat Digerakkan Output." },
  { "en": "Apa Itu Propagation Delay?", "id": "Waktu Tunda Sinyal Melalui Gerbang." },
  { "en": "Apa Itu Arithmetic Logic Unit?", "id": "Bagian CPU Untuk Operasi Aritmatika Logika." },
  { "en": "Apa Kepanjangan Dari ALU?", "id": "Arithmetic Logic Unit." },
  { "en": "Apa Itu Clock Signal?", "id": "Sinyal Denyut Untuk Sinkronisasi Rangkaian." },
  { "en": "Apa Itu EEPROM?", "id": "Memori Non-Volatile Yang Dapat Dihapus Listrik." },
  { "en": "Apa Kepanjangan Dari EEPROM?", "id": "Electrically Erasable Programmable Read-Only Memory." },
  { "en": "Apa Itu Flash Memory?", "id": "Jenis EEPROM Yang Dihapus Per Blok." },
  { "en": "Apa Itu Impedance Matching?", "id": "Penyesuaian Impedansi Untuk Transfer Daya Maksimal." },
  { "en": "Apa Itu Transceiver?", "id": "Perangkat Gabungan Transmitter Dan Receiver." },
  { "en": "Apa Itu Mixer Dalam RF?", "id": "Menggabungkan Dua Sinyal Frekuensi." },
  { "en": "Apa Itu Intermediate Frequency?", "id": "Frekuensi Hasil Dari Proses Mixing." },
  { "en": "Apa Kepanjangan Dari IF?", "id": "Intermediate Frequency." },
  { "en": "Apa Kepanjangan Dari RF?", "id": "Radio Frequency." },
  { "en": "Apa Itu Local Oscillator?", "id": "Pembangkit Sinyal Lokal Untuk Mixer." },
  { "en": "Apa Kepanjangan Dari SWR?", "id": "Standing Wave Ratio." },
  { "en": "Apa Indikasi SWR Yang Tinggi?", "id": "Adanya Mismatch Impedansi Pada Saluran." },
  { "en": "Apa Itu Gain Antena?", "id": "Kemampuan Antena Memfokuskan Sinyal." },
  { "en": "Apa Kepanjangan Dari DSP?", "id": "Digital Signal Processor." },
  { "en": "Apa Fungsi Utama DSP?", "id": "Memproses Sinyal Digital Secara Real-Time." },
  { "en": "Apa Itu LCR Meter?", "id": "Mengukur Induktansi Kapasitansi Dan Resistansi." },
  { "en": "Apa Itu Through-Hole Component?", "id": "Komponen Dengan Kaki Menembus PCB." },
  { "en": "Apa Itu Surface-Mount Device?", "id": "Komponen Yang Dipasang Di Permukaan PCB." },
  { "en": "Apa Kepanjangan Dari SMD?", "id": "Surface-Mount Device." },
  { "en": "Apa Itu Logic High?", "id": "Level Tegangan Yang Mewakili Logika 1." },
  { "en": "Apa Itu Logic Low?", "id": "Level Tegangan Yang Mewakili Logika 0." },
  { "en": "Apa Itu Floating Input?", "id": "Input Yang Tidak Terhubung Ke Apapun." },
  { "en": "Apa Itu Debouncing?", "id": "Menghilangkan Efek Pantulan Pada Saklar Mekanis." },
  { "en": "Apa Itu Watchdog Timer?", "id": "Timer Untuk Mereset Sistem Jika Macet." },
  { "en": "Apa Itu Bus Data?", "id": "Jalur Komunikasi Untuk Transfer Data." },
  { "en": "Apa Itu Bus Alamat?", "id": "Jalur Komunikasi Untuk Alamat Memori." },
  { "en": "Apa Itu Bus Kontrol?", "id": "Jalur Komunikasi Untuk Sinyal Kontrol." },
  { "en": "Apa Itu Paritas Bit?", "id": "Bit Tambahan Untuk Deteksi Kesalahan." },
  { "en": "Apa Itu Checksum?", "id": "Nilai Untuk Memeriksa Integritas Data." },
  { "en": "Apa Itu Handshaking?", "id": "Proses Sinkronisasi Komunikasi Antar Perangkat." },
  { "en": "Apa Itu Virtual Ground?", "id": "Titik Dengan Tegangan Nol Tanpa Ground." },
  { "en": "Apa Itu Common Mode Rejection Ratio?", "id": "Kemampuan Op-Amp Menolak Sinyal Common." },
  { "en": "Apa Kepanjangan Dari CMRR?", "id": "Common Mode Rejection Ratio." },
  { "en": "Apa Itu Nyquist Theorem?", "id": "Teorema Tentang Frekuensi Sampling Minimum." },
  { "en": "Apa Itu Skin Effect?", "id": "Arus AC Cenderung Mengalir Di Permukaan." },
  { "en": "Apa Itu Ferrite Bead?", "id": "Komponen Untuk Menekan Noise Frekuensi Tinggi." },
  { "en": "Apa Itu Ground Loop?", "id": "Masalah Noise Akibat Adanya Dua Ground." },
  { "en": "Apa Itu Opto-isolator?", "id": "Nama Lain Dari Komponen Optocoupler." },
  { "en": "Apa Itu Boolean Algebra?", "id": "Aljabar Yang Digunakan Dalam Logika Digital." },
  { "en": "Apa Itu Karnaugh Map?", "id": "Metode Untuk Menyederhanakan Ekspresi Boolean." },
  { "en": "Apa Itu De Morgan's Theorem?", "id": "Teorema Tentang Inversi Ekspresi Logika." },
  { "en": "Apa Itu Tristate Logic?", "id": "Logika Dengan Keadaan High Impedance." },
  { "en": "Apa Itu Latency?", "id": "Waktu Tunda Dalam Transmisi Data." },
  { "en": "Apa Itu Throughput?", "id": "Tingkat Transfer Data Efektif." },
  { "en": "Apa Itu Firmware Over-The-Air?", "id": "Pembaruan Firmware Melalui Jaringan Nirkabel." },
  { "en": "Apa Kepanjangan Dari FOTA?", "id": "Firmware Over-The-Air." },
  { "en": "Apa Itu Current Source?", "id": "Sumber Yang Memberikan Arus Konstan." },
  { "en": "Apa Itu Voltage Source?", "id": "Sumber Yang Memberikan Tegangan Konstan." },
  { "en": "Apa Itu Load Line Analysis?", "id": "Metode Grafis Analisis Rangkaian Transistor." },
  { "en": "Apa Itu Quiescent Point?", "id": "Titik Operasi DC Sebuah Transistor." },
  { "en": "Apa Fungsi Rangkaian Buck Converter?", "id": "Menurunkan Tegangan DC Dengan Efisien." },
  { "en": "Apa Fungsi Rangkaian Boost Converter?", "id": "Menaikkan Tegangan DC Dengan Efisien." },
  { "en": "Apa Itu Low-Dropout Regulator?", "id": "Regulator Dengan Perbedaan Tegangan Rendah." },
  { "en": "Apa Kepanjangan Dari LDO?", "id": "Low-Dropout." },
  { "en": "Apa Itu Inrush Current?", "id": "Arus Puncak Saat Perangkat Dinyalakan." },
  { "en": "Apa Fungsi Rangkaian Soft Start?", "id": "Membatasi Arus Puncak Saat Start." },
  { "en": "Apa Itu Crosstalk Pada Jalur PCB?", "id": "Interferensi Antar Jalur Sinyal Berdekatan." },
  { "en": "Apa Itu Jitter?", "id": "Variasi Waktu Kedatangan Sinyal Digital." },
  { "en": "Apa Itu Eye Diagram?", "id": "Visualisasi Kualitas Sinyal Digital." },
  { "en": "Apa Fungsi Termination Resistor?", "id": "Mencegah Pantulan Sinyal Di Ujung Saluran." },
  { "en": "Apa Itu Ground Bounce?", "id": "Fluktuasi Tegangan Ground Akibat Switching." },
  { "en": "Apa Kepanjangan Dari FPGA?", "id": "Field-Programmable Gate Array." },
  { "en": "Apa Fungsi Utama FPGA?", "id": "Gerbang Logika Yang Dapat Diprogram Ulang." },
  { "en": "Apa Kepanjangan Dari CPLD?", "id": "Complex Programmable Logic Device." },
  { "en": "Apa Kepanjangan Dari HDL?", "id": "Hardware Description Language." },
  { "en": "Sebutkan Dua Contoh Bahasa HDL?", "id": "Verilog Dan VHDL." },
  { "en": "Apa Itu System on a Chip?", "id": "Integrasi Seluruh Sistem Dalam Satu Chip." },
  { "en": "Apa Kepanjangan Dari SoC?", "id": "System on a Chip." },
  { "en": "Apa Kepanjangan Dari ASIC?", "id": "Application-Specific Integrated Circuit." },
  { "en": "Apa Fungsi Dari ASIC?", "id": "IC Yang Dirancang Untuk Tugas Spesifik." },
  { "en": "Apa Kepanjangan Dari MEMS?", "id": "Micro-Electro-Mechanical Systems." },
  { "en": "Apa Itu Sensor Accelerometer?", "id": "Mengukur Akselerasi Dan Getaran." },
  { "en": "Apa Itu Sensor Gyroscope?", "id": "Mengukur Kecepatan Sudut Atau Rotasi." },
  { "en": "Apa Itu Sensor Magnetometer?", "id": "Mengukur Kekuatan Medan Magnet." },
  { "en": "Apa Kepanjangan Dari CAN Bus?", "id": "Controller Area Network." },
  { "en": "Dimana CAN Bus Biasa Digunakan?", "id": "Jaringan Komunikasi Dalam Otomotif." },
  { "en": "Apa Itu Electromagnetic Interference?", "id": "Gangguan Elektromagnetik Dari Sumber Eksternal." },
  { "en": "Apa Kepanjangan Dari EMI?", "id": "Electromagnetic Interference." },
  { "en": "Apa Itu Electromagnetic Compatibility?", "id": "Kemampuan Perangkat Berfungsi Tanpa Interferensi." },
  { "en": "Apa Kepanjangan Dari EMC?", "id": "Electromagnetic Compatibility." },
  { "en": "Apa Fungsi Dari Faraday Cage?", "id": "Melindungi Isi Dari Medan Elektromagnetik." },
  { "en": "Apa Itu Kabel Coaxial?", "id": "Kabel Dengan Konduktor Pusat Dan Pelindung." },
  { "en": "Apa Fungsi Dari Kabel Twisted Pair?", "id": "Mengurangi Interferensi Elektromagnetik Eksternal." },
  { "en": "Apa Fungsi Dari Balun?", "id": "Menghubungkan Saluran Seimbang Dan Tidak Seimbang." },
  { "en": "Apa Itu Wafer Silikon?", "id": "Bahan Dasar Pembuatan Sirkuit Terpadu." },
  { "en": "Apa Itu Die Dalam Semikonduktor?", "id": "Potongan Kecil Wafer Berisi Satu Sirkuit." },
  { "en": "Apa Itu Photolithography?", "id": "Proses Pencetakan Pola Sirkuit Ke Wafer." },
  { "en": "Apa Itu P-N Junction?", "id": "Pertemuan Semikonduktor Tipe-P Dan Tipe-N." },
  { "en": "Apa Itu Depletion Region?", "id": "Area Tanpa Pembawa Muatan Di P-N Junction." },
  { "en": "Apa Ciri Khas Amplifier Kelas A?", "id": "Transistor Aktif Sepanjang Siklus Sinyal." },
  { "en": "Apa Ciri Khas Amplifier Kelas B?", "id": "Dua Transistor Aktif Bergantian." },
  { "en": "Apa Ciri Khas Amplifier Kelas AB?", "id": "Gabungan Efisiensi Kelas B Kualitas A." },
  { "en": "Apa Prinsip Kerja Amplifier Kelas D?", "id": "Menggunakan Teknik Switching Atau PWM." },
  { "en": "Apa Itu Total Harmonic Distortion?", "id": "Ukuran Distorsi Harmonik Pada Sinyal." },
  { "en": "Apa Kepanjangan Dari THD?", "id": "Total Harmonic Distortion." },
  { "en": "Apa Itu Signal-to-Noise Ratio?", "id": "Rasio Kekuatan Sinyal Terhadap Noise." },
  { "en": "Apa Kepanjangan Dari SNR?", "id": "Signal-to-Noise Ratio." },
  { "en": "Apa Tujuan Dari Teorema Thevenin?", "id": "Menyederhanakan Rangkaian Menjadi Sumber Tegangan." },
  { "en": "Apa Tujuan Dari Teorema Norton?", "id": "Menyederhanakan Rangkaian Menjadi Sumber Arus." },
  { "en": "Apa Prinsip Teorema Superposisi?", "id": "Menganalisis Efek Setiap Sumber Secara Terpisah." },
  { "en": "Apa Itu Real-Time Clock?", "id": "Sirkuit Pewaktu Yang Terus Berjalan." },
  { "en": "Apa Kepanjangan Dari RTC?", "id": "Real-Time Clock." },
  { "en": "Apa Itu Brown-Out Detection?", "id": "Fitur Mendeteksi Penurunan Tegangan Catu Daya." },
  { "en": "Apa Kepanjangan Dari BOD?", "id": "Brown-Out Detection." },
  { "en": "Apa Fungsi Rangkaian H-Bridge?", "id": "Mengendalikan Arah Putaran Motor DC." },
  { "en": "Apa Fungsi Rangkaian Phase-Locked Loop?", "id": "Menyinkronkan Frekuensi Sinyal Internal Eksternal." },
  { "en": "Apa Kepanjangan Dari PLL?", "id": "Phase-Locked Loop." },
  { "en": "Apa Itu Charge Pump?", "id": "Pengganda Tegangan DC Menggunakan Kapasitor." },
  { "en": "Apa Itu Finite State Machine?", "id": "Model Komputasi Abstrak Untuk Desain Digital." },
  { "en": "Apa Kepanjangan Dari FSM?", "id": "Finite State Machine." },
  { "en": "Apa Itu Metastability?", "id": "Kondisi Tidak Stabil Dalam Sistem Digital." },
  { "en": "Apa Itu Verilog?", "id": "Bahasa Deskripsi Perangkat Keras." },
  { "en": "Apa Itu VHDL?", "id": "Bahasa Deskripsi Perangkat Keras Lainnya." },
  { "en": "Apa Itu Bit?", "id": "Unit Informasi Terkecil Dalam Komputasi." },
  { "en": "Apa Itu Byte?", "id": "Kumpulan Delapan Bit." },
  { "en": "Apa Itu Word Dalam Komputasi?", "id": "Ukuran Data Alami Sebuah Prosesor." },
  { "en": "Apa Itu Endianness?", "id": "Urutan Byte Dalam Representasi Data." },
  { "en": "Apa Itu Big-Endian?", "id": "Byte Paling Signifikan Disimpan Pertama." },
  { "en": "Apa Itu Little-Endian?", "id": "Byte Paling Tidak Signifikan Disimpan Pertama." },
  { "en": "Apa Itu Floating Point?", "id": "Representasi Angka Dengan Titik Desimal." },
  { "en": "Apa Itu Fixed Point?", "id": "Representasi Angka Dengan Posisi Tetap." },
  { "en": "Apa Itu Galvanic Isolation?", "id": "Prinsip Mengisolasi Bagian Rangkaian Listrik." },
  { "en": "Apa Itu Hot-Swapping?", "id": "Memasang Melepas Komponen Tanpa Mematikan Sistem." },
  { "en": "Apa Itu Interleaving?", "id": "Teknik Mengatur Data Untuk Kinerja Memori." },
  { "en": "Apa Itu L1 Cache?", "id": "Cache Memori Tercepat Di Dalam CPU." },
  { "en": "Apa Itu L2 Cache?", "id": "Cache Memori Lebih Lambat Dari L1." },
  { "en": "Apa Itu Memory Controller?", "id": "Sirkuit Pengelola Aliran Data Ke Memori." },
  { "en": "Apa Itu Pipeline Dalam Prosesor?", "id": "Teknik Eksekusi Instruksi Secara Tumpang Tindih." },
  { "en": "Apa Itu Race Condition?", "id": "Kondisi Output Tergantung Urutan Kejadian." },
  { "en": "Apa Itu Schmitt Trigger Input?", "id": "Input Dengan Histeresis Untuk Sinyal Lambat." },
  { "en": "Apa Itu Thermal Shutdown?", "id": "Fitur Proteksi Komponen Dari Panas Berlebih." },
  { "en": "Apa Itu Time Domain Reflectometer?", "id": "Alat Uji Untuk Mencari Kesalahan Kabel." },
  { "en": "Apa Kepanjangan Dari TDR?", "id": "Time Domain Reflectometer." },
  { "en": "Apa Itu Unity Gain?", "id": "Penguatan Sama Dengan Satu." },
  { "en": "Apa Itu Virtual Memory?", "id": "Teknik Menggunakan Disk Sebagai RAM Tambahan." },
  { "en": "Apa Itu Volatile Memory?", "id": "Memori Yang Membutuhkan Daya Untuk Simpan Data." },
  { "en": "Apa Itu Non-Volatile Memory?", "id": "Memori Yang Menyimpan Data Tanpa Daya." },
  { "en": "Apa Itu Zero-Crossing Detector?", "id": "Rangkaian Pendeteksi Saat Sinyal Melewati Nol." },
  { "en": "Apa Itu Time Constant?", "id": "Ukuran Kecepatan Respon Rangkaian RC." },
  { "en": "Apa Itu Veroboard?", "id": "Papan Prototipe Dengan Jalur Tembaga Lurus." },
  { "en": "Apa Itu Active Component?", "id": "Komponen Yang Membutuhkan Sumber Energi." },
  { "en": "Apa Itu Passive Component?", "id": "Komponen Yang Tidak Membutuhkan Sumber Energi." },
  { "en": "Apa Itu Schematic Diagram?", "id": "Diagram Representasi Simbolik Sebuah Rangkaian." },
  { "en": "Apa Kepanjangan Dari USB?", "id": "Universal Serial Bus." },
  { "en": "Apa Fungsi Dari USB On-The-Go?", "id": "Memungkinkan Perangkat Jadi Host USB." },
  { "en": "Apa Kepanjangan Dari USB OTG?", "id": "USB On-The-Go." },
  { "en": "Apa Itu Alamat MAC?", "id": "Alamat Fisik Unik Perangkat Jaringan." },
  { "en": "Apa Kepanjangan Dari MAC Address?", "id": "Media Access Control Address." },
  { "en": "Apa Standar Komunikasi RS-232?", "id": "Standar Komunikasi Serial Point-to-Point." },
  { "en": "Apa Keunggulan Standar RS-485?", "id": "Mendukung Komunikasi Jarak Jauh." },
  { "en": "Apa Kepanjangan Dari BLE?", "id": "Bluetooth Low Energy." },
  { "en": "Apa Jaringan Nirkabel Zigbee?", "id": "Jaringan Mesh Nirkabel Daya Rendah." },
  { "en": "Apa Kepanjangan Dari LCD?", "id": "Liquid Crystal Display." },
  { "en": "Apa Kepanjangan Dari OLED?", "id": "Organic Light Emitting Diode." },
  { "en": "Apa Itu Seven Segment Display?", "id": "Display Untuk Menampilkan Angka Desimal." },
  { "en": "Apa Kepanjangan Dari HDMI?", "id": "High-Definition Multimedia Interface." },
  { "en": "Apa Perbedaan SRAM Dan DRAM?", "id": "SRAM Lebih Cepat Tetapi Mahal." },
  { "en": "Apa Kepanjangan Dari SRAM?", "id": "Static Random Access Memory." },
  { "en": "Apa Kepanjangan Dari DRAM?", "id": "Dynamic Random Access Memory." },
  { "en": "Apa Karakteristik NOR Flash?", "id": "Memungkinkan Akses Acak Cepat." },
  { "en": "Apa Karakteristik NAND Flash?", "id": "Cocok Untuk Penyimpanan Data Sekuensial." },
  { "en": "Apa Itu Antena Isotropik?", "id": "Antena Ideal Pemancar Segala Arah." },
  { "en": "Apa Itu Antena Dipole?", "id": "Antena Dasar Dengan Dua Elemen Konduktif." },
  { "en": "Apa Itu Polarisasi Antena?", "id": "Orientasi Medan Listrik Gelombang Radio." },
  { "en": "Apa Kepanjangan Dari IGBT?", "id": "Insulated-Gate Bipolar Transistor." },
  { "en": "Apa Karakteristik Utama IGBT?", "id": "Gabungan Keunggulan BJT Dan MOSFET." },
  { "en": "Apa Itu Dioda Tunnel?", "id": "Dioda Dengan Efek Terowongan Kuantum." },
  { "en": "Apa Itu PIN Dioda?", "id": "Dioda Untuk Aplikasi Saklar RF." },
  { "en": "Apa Itu DIAC?", "id": "Komponen Pemicu Untuk TRIAC." },
  { "en": "Apa Itu Interrupt Request?", "id": "Sinyal Untuk Menginterupsi Prosesor." },
  { "en": "Apa Kepanjangan Dari IRQ?", "id": "Interrupt Request." },
  { "en": "Apa Itu Direct Memory Access?", "id": "Transfer Data Tanpa Melibatkan CPU." },
  { "en": "Apa Kepanjangan Dari DMA?", "id": "Direct Memory Access." },
  { "en": "Apa Kepanjangan Dari RoHS?", "id": "Restriction of Hazardous Substances." },
  { "en": "Apa Tujuan Direktif RoHS?", "id": "Membatasi Penggunaan Bahan Berbahaya." },
  { "en": "Apa Arti Tanda CE?", "id": "Produk Memenuhi Standar Keselamatan Eropa." },
  { "en": "Apa Itu Codec?", "id": "Perangkat Lunak Untuk Kompresi Dekompresi Data." },
  { "en": "Apa Itu Bit Depth?", "id": "Jumlah Bit Informasi Dalam Sampel." },
  { "en": "Apa Itu Progressive Scan?", "id": "Menampilkan Seluruh Baris Gambar Sekaligus." },
  { "en": "Apa Itu Interlaced Scan?", "id": "Menampilkan Baris Ganjil Lalu Genap." },
  { "en": "Apa Itu Via Pada PCB?", "id": "Lubang Konduktif Penghubung Antar Lapisan." },
  { "en": "Apa Itu Trace Pada PCB?", "id": "Jalur Tembaga Penghubung Antar Komponen." },
  { "en": "Apa Itu Silkscreen Pada PCB?", "id": "Lapisan Tinta Penanda Komponen." },
  { "en": "Apa Itu Solder Mask?", "id": "Lapisan Pelindung Jalur Tembaga." },
  { "en": "Apa Itu File Gerber?", "id": "Format File Standar Untuk Manufaktur PCB." },
  { "en": "Apa Itu Bill of Materials?", "id": "Daftar Semua Komponen Dalam Rangkaian." },
  { "en": "Apa Kepanjangan Dari BOM?", "id": "Bill of Materials." },
  { "en": "Apa Kegunaan Transformasi Laplace?", "id": "Analisis Rangkaian Dalam Domain Frekuensi." },
  { "en": "Apa Kegunaan Smith Chart?", "id": "Alat Grafis Untuk Analisis Saluran Transmisi." },
  { "en": "Apa Kegunaan Bode Plot?", "id": "Grafik Respon Frekuensi Sebuah Sistem." },
  { "en": "Apa Itu Decimation?", "id": "Proses Mengurangi Laju Sampling Sinyal." },
  { "en": "Apa Itu Interpolation?", "id": "Proses Meningkatkan Laju Sampling Sinyal." },
  { "en": "Apa Itu Filter FIR?", "id": "Filter Digital Dengan Respon Impuls Terbatas." },
  { "en": "Apa Kepanjangan Dari FIR?", "id": "Finite Impulse Response." },
  { "en": "Apa Itu Filter IIR?", "id": "Filter Digital Dengan Respon Impuls Tak Terbatas." },
  { "en": "Apa Kepanjangan Dari IIR?", "id": "Infinite Impulse Response." },
  { "en": "Apa Itu Kernel Dalam Sistem Operasi?", "id": "Inti Yang Mengelola Sumber Daya Sistem." },
  { "en": "Apa Itu Instruction Set?", "id": "Kumpulan Perintah Dasar Sebuah Prosesor." },
  { "en": "Apa Itu Microcode?", "id": "Lapisan Perintah Level Rendah Dalam CPU." },
  { "en": "Apa Itu Single-Board Computer?", "id": "Komputer Lengkap Dalam Satu Papan Sirkuit." },
  { "en": "Apa Kepanjangan Dari SBC?", "id": "Single-Board Computer." },
  { "en": "Apa Itu Form Factor?", "id": "Spesifikasi Fisik Papan Sirkuit." },
  { "en": "Apa Itu Surface Acoustic Wave Filter?", "id": "Filter Berbasis Gelombang Akustik Permukaan." },
  { "en": "Apa Kepanjangan Dari SAW Filter?", "id": "Surface Acoustic Wave Filter." },
  { "en": "Apa Itu Dielectric?", "id": "Bahan Isolator Dalam Kapasitor." },
  { "en": "Apa Itu Permeability?", "id": "Kemampuan Bahan Membentuk Medan Magnet." },
  { "en": "Apa Itu Permittivity?", "id": "Kemampuan Bahan Menyimpan Energi Listrik." },
  { "en": "Apa Itu Hysteresis Loop?", "id": "Grafik Hubungan Magnetisasi Medan Magnet." },
  { "en": "Apa Itu Eddy Current?", "id": "Arus Induksi Yang Timbul Di Konduktor." },
  { "en": "Apa Itu Quality Factor?", "id": "Ukuran Kualitas Rangkaian Resonansi." },
  { "en": "Apa Kepanjangan Dari Q Factor?", "id": "Quality Factor." },
  { "en": "Apa Itu Cutoff Frequency?", "id": "Frekuensi Batas Operasi Sebuah Filter." },
  { "en": "Apa Itu Roll-off?", "id": "Tingkat Atenuasi Filter Di Luar Passband." },
  { "en": "Apa Itu D-Type Flip-Flop?", "id": "Flip-Flop Yang Menyimpan Data Input." },
  { "en": "Apa Itu T-Type Flip-Flop?", "id": "Flip-Flop Yang Berubah Keadaan (Toggle)." },
  { "en": "Apa Itu JK-Type Flip-Flop?", "id": "Flip-Flop Universal Dengan Input J K." },
  { "en": "Apa Itu Race-Around Condition?", "id": "Masalah Pada Level-Triggered JK Flip-Flop." },
  { "en": "Apa Itu Master-Slave Flip-Flop?", "id": "Struktur Untuk Menghindari Race-Around Condition." },
  { "en": "Apa Itu Asynchronous Counter?", "id": "Counter Dimana Flip-Flop Tidak Serempak." },
  { "en": "Apa Itu Synchronous Counter?", "id": "Counter Dengan Sinyal Clock Bersama." },
  { "en": "Apa Itu Ring Counter?", "id": "Register Geser Dengan Output Terhubung Input." },
  { "en": "Apa Itu Johnson Counter?", "id": "Variasi Ring Counter Dengan Output Terbalik." },
  { "en": "Apa Itu Encoder?", "id": "Mengubah Data Ke Format Kode Tertentu." },
  { "en": "Apa Itu Decoder?", "id": "Mengubah Data Terkode Ke Format Asli." },
  { "en": "Apa Itu Priority Encoder?", "id": "Encoder Yang Memprioritaskan Input Tertinggi." },
  { "en": "Apa Itu Phase Detector?", "id": "Membandingkan Fasa Dua Sinyal Input." },
  { "en": "Apa Itu Voltage-Controlled Oscillator?", "id": "Osilator Dengan Frekuensi Terkendali Tegangan." },
  { "en": "Apa Kepanjangan Dari VCO?", "id": "Voltage-Controlled Oscillator." },
  { "en": "Apa Itu Analog MUX?", "id": "Multiplexer Untuk Melewatkan Sinyal Analog." },
  { "en": "Apa Itu Sample and Hold Circuit?", "id": "Rangkaian Pencuplik Dan Penahan Tegangan." },
  { "en": "Apa Itu Anti-Aliasing Filter?", "id": "Low-pass Filter Sebelum Proses Sampling." },
  { "en": "Apa Itu Reconstruction Filter?", "id": "Low-pass Filter Setelah Proses DAC." },
  { "en": "Apa Itu Delta-Sigma ADC?", "id": "Tipe ADC Dengan Teknik Oversampling." },
  { "en": "Apa Itu Flash ADC?", "id": "Tipe ADC Paling Cepat." },
  { "en": "Apa Itu Look-Up Table (LUT)?", "id": "Elemen Logika Dasar Dalam FPGA." },
  { "en": "Apa Kepanjangan Dari LUT (Look-Up Table)?", "id": "Look-Up Table." },
  { "en": "Apa Itu Programmable Logic Array (PLA)?", "id": "Perangkat Logika Dengan Gerbang Terprogram." },
  { "en": "Apa Kepanjangan Dari PLA (Programmable Logic Array)?", "id": "Programmable Logic Array." },
  { "en": "Apa Ciri Arsitektur CISC (Complex Instruction Set Computer)?", "id": "Memiliki Set Instruksi Yang Kompleks." },
  { "en": "Apa Kepanjangan Dari CISC?", "id": "Complex Instruction Set Computer." },
  { "en": "Apa Ciri Arsitektur RISC (Reduced Instruction Set Computer)?", "id": "Memiliki Set Instruksi Yang Sederhana." },
  { "en": "Apa Kepanjangan Dari RISC?", "id": "Reduced Instruction Set Computer." },
  { "en": "Apa Ciri Arsitektur Von Neumann?", "id": "Memori Program Dan Data Digabung." },
  { "en": "Apa Ciri Arsitektur Harvard?", "id": "Memori Program Dan Data Terpisah." },
  { "en": "Apa Itu Band Gap Energi?", "id": "Energi Minimum Untuk Eksitasi Elektron." },
  { "en": "Apa Itu Avalanche Breakdown?", "id": "Kerusakan Dioda Akibat Percepatan Elektron." },
  { "en": "Apa Itu Zener Breakdown?", "id": "Kerusakan Dioda Akibat Medan Listrik Tinggi." },
  { "en": "Apa Itu In-Circuit Testing (ICT)?", "id": "Pengujian PCB Saat Komponen Terpasang." },
  { "en": "Apa Kepanjangan Dari ICT (In-Circuit Testing)?", "id": "In-Circuit Testing." },
  { "en": "Apa Itu Reflow Soldering?", "id": "Proses Pemanasan Pasta Solder." },
  { "en": "Apa Itu Wave Soldering?", "id": "Proses Penyolderan Komponen Through-Hole." },
  { "en": "Apa Itu Quadrature Amplitude Modulation (QAM)?", "id": "Modulasi Menggunakan Amplitudo Dan Fasa." },
  { "en": "Apa Kepanjangan Dari QAM (Quadrature Amplitude Modulation)?", "id": "Quadrature Amplitude Modulation." },
  { "en": "Apa Itu Phase-Shift Keying (PSK)?", "id": "Modulasi Digital Dengan Mengubah Fasa." },
  { "en": "Apa Kepanjangan Dari PSK (Phase-Shift Keying)?", "id": "Phase-Shift Keying." },
  { "en": "Apa Itu Frequency-Shift Keying (FSK)?", "id": "Modulasi Digital Dengan Mengubah Frekuensi." },
  { "en": "Apa Kepanjangan Dari FSK (Frequency-Shift Keying)?", "id": "Frequency-Shift Keying." },
  { "en": "Apa Itu OFDM (Orthogonal Frequency-Division Multiplexing)?", "id": "Teknik Modulasi Untuk Komunikasi Kecepatan Tinggi." },
  { "en": "Apa Kepanjangan Dari OFDM?", "id": "Orthogonal Frequency-Division Multiplexing." },
  { "en": "Apa Itu Bit Error Rate (BER)?", "id": "Tingkat Kesalahan Bit Dalam Transmisi." },
  { "en": "Apa Kepanjangan Dari BER (Bit Error Rate)?", "id": "Bit Error Rate." },
  { "en": "Apa Itu Error Correction Code (ECC)?", "id": "Kode Untuk Mendeteksi Memperbaiki Kesalahan." },
  { "en": "Apa Kepanjangan Dari ECC (Error Correction Code)?", "id": "Error Correction Code." },
  { "en": "Apa Itu Kontroler PID (Proportional-Integral-Derivative)?", "id": "Kontroler Umpan Balik Proportional Integral Derivative." },
  { "en": "Apa Itu Sistem Open-Loop?", "id": "Sistem Kontrol Tanpa Umpan Balik." },
  { "en": "Apa Itu Sistem Closed-Loop?", "id": "Sistem Kontrol Dengan Umpan Balik." },
  { "en": "Apa Itu Transfer Function?", "id": "Representasi Matematis Hubungan Input-Output." },
  { "en": "Apa Itu Setpoint?", "id": "Nilai Target Dalam Sistem Kontrol." },
  { "en": "Apa Fungsi Perangkat Repeater?", "id": "Memperkuat Sinyal Untuk Jarak Jauh." },
  { "en": "Apa Fungsi Perangkat Hub?", "id": "Menghubungkan Beberapa Perangkat Dalam Jaringan." },
  { "en": "Apa Fungsi Perangkat Bridge?", "id": "Menghubungkan Dua Segmen Jaringan." },
  { "en": "Apa Fungsi Perangkat Switch?", "id": "Meneruskan Data Ke Port Tertentu." },
  { "en": "Apa Fungsi Perangkat Router?", "id": "Menghubungkan Jaringan Yang Berbeda." },
  { "en": "Apa Fungsi Dioda Varactor?", "id": "Sebagai Kapasitor Yang Dikendalikan Tegangan." },
  { "en": "Apa Itu Rangkaian Gyrator?", "id": "Mensimulasikan Induktor Dengan Kapasitor." },
  { "en": "Apa Itu Logarithmic Amplifier?", "id": "Penguat Dengan Output Logaritmik Input." },
  { "en": "Apa Kepanjangan Dari DSO (Digital Storage Oscilloscope)?", "id": "Digital Storage Oscilloscope." },
  { "en": "Apa Kepanjangan Dari MSO (Mixed-Signal Oscilloscope)?", "id": "Mixed-Signal Oscilloscope." },
  { "en": "Apa Kemampuan Osiloskop MSO (Mixed-Signal Oscilloscope)?", "id": "Menganalisis Sinyal Analog Dan Digital." },
  { "en": "Apa Kepanjangan Dari AWG (Arbitrary Waveform Generator)?", "id": "Arbitrary Waveform Generator." },
  { "en": "Apa Itu Power Factor?", "id": "Rasio Daya Nyata Terhadap Daya Semu." },
  { "en": "Apa Fungsi Power Factor Correction (PFC)?", "id": "Meningkatkan Efisiensi Daya Listrik." },
  { "en": "Apa Kepanjangan Dari PFC (Power Factor Correction)?", "id": "Power Factor Correction." },
  { "en": "Apa Itu Creepage Pada PCB?", "id": "Jarak Terpendek Antar Konduktor Di Permukaan." },
  { "en": "Apa Itu Clearance Pada PCB?", "id": "Jarak Terpendek Antar Konduktor Di Udara." },
  { "en": "Apa Itu Full Adder?", "id": "Penjumlah Biner Tiga Bit Input." },
  { "en": "Apa Itu Half Adder?", "id": "Penjumlah Biner Dua Bit Input." },
  { "en": "Apa Itu Ripple-Carry Adder?", "id": "Adder Yang Propagasi Carry Bit Berurutan." },
  { "en": "Apa Itu Comparator Digital?", "id": "Membandingkan Dua Nilai Biner." },
  { "en": "Apa Itu Cache Coherency?", "id": "Konsistensi Data Dalam Beberapa Cache." },
  { "en": "Apa Itu Semikonduktor Intrinsik?", "id": "Semikonduktor Murni Tanpa Doping." },
  { "en": "Apa Itu Semikonduktor Ekstrinsik?", "id": "Semikonduktor Dengan Penambahan Doping." },
  { "en": "Apa Itu Electron Mobility?", "id": "Kecepatan Elektron Bergerak Dalam Semikonduktor." },
  { "en": "Apa Itu Functional Testing (FCT)?", "id": "Pengujian Fungsi Perangkat Secara Keseluruhan." },
  { "en": "Apa Kepanjangan Dari FCT (Functional Testing)?", "id": "Functional Testing." },
  { "en": "Apa Kepanjangan Dari ATE (Automatic Test Equipment)?", "id": "Automatic Test Equipment." },
  { "en": "Apa Itu Solder Paste?", "id": "Campuran Timah Solder Dan Flux." },
  { "en": "Apa Itu Constellation Diagram?", "id": "Diagram Representasi Sinyal Modulasi Digital." },
  { "en": "Apa Itu Nyquist Rate?", "id": "Laju Sampling Minimum." },
  { "en": "Apa Itu Oversampling?", "id": "Sampling Pada Frekuensi Lebih Tinggi." },
  { "en": "Apa Itu Undersampling?", "id": "Sampling Pada Frekuensi Lebih Rendah." },
  { "en": "Apa Itu Dead Time?", "id": "Waktu Tunda Untuk Mencegah Hubung Singkat." },
  { "en": "Apa Itu Analog Front-End (AFE)?", "id": "Sirkuit Antarmuka Antara Sensor Analog." },
  { "en": "Apa Kepanjangan Dari AFE (Analog Front-End)?", "id": "Analog Front-End." },
  { "en": "Apa Itu Differential Signaling?", "id": "Menggunakan Dua Sinyal Komplementer." },
  { "en": "Apa Keuntungan Differential Signaling?", "id": "Ketahanan Terhadap Noise Sangat Baik." },
  { "en": "Apa Itu Common-Mode Signal?", "id": "Sinyal Yang Sama Pada Kedua Input." },
  { "en": "Apa Itu Firmware Development Kit (FDK)?", "id": "Paket Perangkat Lunak Pengembangan Firmware." },
  { "en": "Apa Kepanjangan Dari FDK (Firmware Development Kit)?", "id": "Firmware Development Kit." },
  { "en": "Apa Itu JTAG (Joint Test Action Group)?", "id": "Standar Untuk Debugging Dan Pengujian PCB." },
  { "en": "Apa Kepanjangan Dari JTAG?", "id": "Joint Test Action Group." },
  { "en": "Apa Itu Breakout Board?", "id": "Papan Sirkuit Kecil Untuk Akses Pin IC." },
  { "en": "Apa Itu Shield Pada Arduino?", "id": "Papan Tambahan Yang Dipasang Di Atas." },
  { "en": "Apa Itu Kernel Panic?", "id": "Kesalahan Fatal Pada Kernel Sistem Operasi." },
  { "en": "Apa Itu Bricking?", "id": "Membuat Perangkat Elektronik Tidak Berfungsi." },
  { "en": "Apa Itu Dongle?", "id": "Perangkat Keras Kecil Terhubung Port Lain." },
  { "en": "Apa Itu Network Interface Card (NIC)?", "id": "Kartu Untuk Menghubungkan Komputer Ke Jaringan." },
  { "en": "Apa Kepanjangan Dari NIC (Network Interface Card)?", "id": "Network Interface Card." },
  { "en": "Apa Itu Thermal Pad?", "id": "Material Konduktif Termal." },
  { "en": "Apa Itu Thermal Paste?", "id": "Pasta Untuk Transfer Panas." },
  { "en": "Apa Itu Data Logger?", "id": "Perangkat Perekam Data Seiring Waktu." },
  { "en": "Apa Itu Human-Machine Interface (HMI)?", "id": "Antarmuka Pengguna Antara Manusia Mesin." },
  { "en": "Apa Kepanjangan Dari HMI (Human-Machine Interface)?", "id": "Human-Machine Interface." },
  { "en": "Apa Itu Actuator?", "id": "Komponen Yang Menghasilkan Gerakan." },
  { "en": "Apa Itu Solenoid?", "id": "Aktuator Elektromagnetik Penghasil Gerak Linier." },
  { "en": "Apa Itu Servo Motor?", "id": "Motor Dengan Kontrol Posisi Umpan Balik." },
  { "en": "Apa Itu Stepper Motor?", "id": "Motor Yang Bergerak Dalam Langkah Diskrit." },
  { "en": "Apa Itu Emisi Spontan?", "id": "Pemancaran Foton Tanpa Stimulasi Eksternal." },
  { "en": "Apa Itu Emisi Terstimulasi?", "id": "Pemancaran Foton Akibat Stimulasi Foton Lain." },
  { "en": "Apa Prinsip Dasar Laser?", "id": "Penguatan Cahaya Melalui Emisi Terstimulasi." },
  { "en": "Apa Itu Photodetector?", "id": "Sensor Yang Mengubah Cahaya Menjadi Sinyal Listrik." },
  { "en": "Apa Perbedaan Serat Optik Single-Mode?", "id": "Hanya Melewatkan Satu Mode Cahaya." },
  { "en": "Apa Perbedaan Serat Optik Multi-Mode?", "id": "Melewatkan Beberapa Mode Cahaya." },
  { "en": "Apa Material Substrat PCB Paling Umum?", "id": "FR-4 (Flame Retardant 4)." },
  { "en": "Apa Itu Conformal Coating?", "id": "Lapisan Pelindung Tipis Untuk PCB." },
  { "en": "Apa Itu Proses Encapsulation?", "id": "Proses Pembungkusan Komponen Elektronik." },
  { "en": "Apa Kepanjangan Dari BMS?", "id": "Battery Management System." },
  { "en": "Apa Fungsi Utama BMS?", "id": "Melindungi Baterai Dari Kondisi Berbahaya." },
  { "en": "Apa Itu State of Charge?", "id": "Tingkat Kapasitas Baterai Saat Ini." },
  { "en": "Apa Kepanjangan Dari SoC (Baterai)?", "id": "State of Charge." },
  { "en": "Apa Itu State of Health?", "id": "Ukuran Kondisi Kesehatan Baterai." },
  { "en": "Apa Kepanjangan Dari SoH?", "id": "State of Health." },
  { "en": "Apa Itu Trickle Charging?", "id": "Pengisian Lambat Untuk Menjaga Kapasitas." },
  { "en": "Apa Itu Wireless Power Transfer?", "id": "Transfer Daya Listrik Tanpa Kabel." },
  { "en": "Apa Kepanjangan Dari WPT?", "id": "Wireless Power Transfer." },
  { "en": "Apa Itu Waveguide?", "id": "Struktur Logam Pemandu Gelombang Mikro." },
  { "en": "Apa Fungsi Perangkat Circulator?", "id": "Mengarahkan Sinyal Dari Port Ke Port Berikutnya." },
  { "en": "Apa Fungsi Perangkat Isolator RF?", "id": "Melewatkan Sinyal Hanya Ke Satu Arah." },
  { "en": "Apa Itu Directional Coupler?", "id": "Mencuplik Sebagian Kecil Sinyal." },
  { "en": "Apa Itu S-Parameters?", "id": "Parameter Untuk Karakterisasi Jaringan Frekuensi Tinggi." },
  { "en": "Apa Arti Parameter S11?", "id": "Return Loss Atau Pantulan Sinyal Input." },
  { "en": "Apa Arti Parameter S21?", "id": "Insertion Gain Atau Penguatan Maju." },
  { "en": "Apa Kepanjangan Dari SPICE?", "id": "Simulation Program with Integrated Circuit Emphasis." },
  { "en": "Apa Kegunaan Simulator SPICE?", "id": "Mensimulasikan Perilaku Rangkaian Elektronik." },
  { "en": "Apa Kepanjangan Dari EDA?", "id": "Electronic Design Automation." },
  { "en": "Apa Itu Mean Time Between Failures?", "id": "Waktu Rata-Rata Antara Kegagalan." },
  { "en": "Apa Kepanjangan Dari MTBF?", "id": "Mean Time Between Failures." },
  { "en": "Apa Itu Electrostatic Discharge?", "id": "Pelepasan Listrik Statis Tiba-Tiba." },
  { "en": "Apa Kepanjangan Dari ESD?", "id": "Electrostatic Discharge." },
  { "en": "Apa Fungsi Dioda TVS?", "id": "Melindungi Rangkaian Dari Lonjakan Tegangan." },
  { "en": "Apa Kepanjangan Dari FMEA?", "id": "Failure Mode and Effects Analysis." },
  { "en": "Apa Tipe ADC SAR?", "id": "ADC Dengan Metode Aproksimasi Berurutan." },
  { "en": "Apa Kepanjangan Dari SAR ADC?", "id": "Successive Approximation Register ADC." },
  { "en": "Apa Itu Quantization Error?", "id": "Kesalahan Pembulatan Dalam Konversi Analog Digital." },
  { "en": "Apa Kepanjangan Dari IoT?", "id": "Internet of Things." },
  { "en": "Apa Itu System in Package?", "id": "Integrasi Beberapa Chip Dalam Satu Paket." },
  { "en": "Apa Kepanjangan Dari SiP?", "id": "System in Package." },
  { "en": "Apa Itu Chiplet?", "id": "Komponen Chip Modular Untuk Desain SiP." },
  { "en": "Apa Kepanjangan Dari NFC?", "id": "Near-Field Communication." },
  { "en": "Apa Itu Crossover Network?", "id": "Sirkuit Pembagi Frekuensi Untuk Speaker." },
  { "en": "Apa Itu Sinyal Audio Balanced?", "id": "Menggunakan Tiga Konduktor Untuk Menolak Noise." },
  { "en": "Apa Itu Sinyal Audio Unbalanced?", "id": "Menggunakan Dua Konduktor." },
  { "en": "Apa Itu Phantom Power?", "id": "Daya DC Yang Dikirim Melalui Kabel Mikrofon." },
  { "en": "Apa Itu Ground Lift?", "id": "Memutus Koneksi Ground Untuk Hilangkan Dengung." },
  { "en": "Apa Itu Load Cell?", "id": "Sensor Untuk Mengukur Gaya Atau Beban." },
  { "en": "Apa Itu Strain Gauge?", "id": "Sensor Pengukur Regangan Pada Benda." },
  { "en": "Apa Itu Thermocouple?", "id": "Sensor Suhu Berdasarkan Efek Seebeck." },
  { "en": "Apa Itu RTD Sensor?", "id": "Sensor Suhu Berbasis Resistansi Logam." },
  { "en": "Apa Kepanjangan Dari RTD?", "id": "Resistance Temperature Detector." },
  { "en": "Apa Itu Peltier Device?", "id": "Komponen Termoelektrik Untuk Pendinginan Pemanasan." },
  { "en": "Apa Itu Piezoelectric Effect?", "id": "Kemampuan Material Menghasilkan Tegangan Saat Ditekan." },
  { "en": "Apa Itu Inter-Symbol Interference?", "id": "Distorsi Sinyal Akibat Simbol Berdekatan." },
  { "en": "Apa Kepanjangan Dari ISI?", "id": "Inter-Symbol Interference." },
  { "en": "Apa Itu Channel Capacity?", "id": "Tingkat Transfer Data Maksimum Suatu Kanal." },
  { "en": "Apa Itu Shannon-Hartley Theorem?", "id": "Teorema Tentang Channel Capacity." },
  { "en": "Apa Itu White Noise?", "id": "Noise Dengan Spektrum Daya Datar." },
  { "en": "Apa Itu Pink Noise?", "id": "Noise Dengan Daya Turun Per Oktaf." },
  { "en": "Apa Itu Brown Noise?", "id": "Noise Dengan Spektrum Seperti Gerak Brown." },
  { "en": "Apa Itu Finite Element Method?", "id": "Metode Numerik Untuk Analisis Elektromagnetik." },
  { "en": "Apa Kepanjangan Dari FEM?", "id": "Finite Element Method." },
  { "en": "Apa Itu Class AB Amplifier?", "id": "Kompromi Antara Kelas A Dan B." },
  { "en": "Apa Itu Class G Amplifier?", "id": "Amplifier Dengan Beberapa Level Tegangan Catu." },
  { "en": "Apa Itu Class H Amplifier?", "id": "Amplifier Yang Memodulasi Tegangan Catu Daya." },
  { "en": "Apa Itu Slew Rate Limiting?", "id": "Keterbatasan Laju Perubahan Tegangan Output." },
  { "en": "Apa Itu Input Offset Voltage?", "id": "Tegangan DC Kecil Di Input Op-Amp." },
  { "en": "Apa Itu Input Bias Current?", "id": "Arus DC Kecil Di Input Op-Amp." },
  { "en": "Apa Itu Rail-to-Rail Op-Amp?", "id": "Op-Amp Dengan Output Mencapai Tegangan Catu." },
  { "en": "Apa Itu Instrumentation Amplifier?", "id": "Penguat Diferensial Presisi Tinggi." },
  { "en": "Apa Itu Isolation Amplifier?", "id": "Penguat Dengan Isolasi Galvanik." },
  { "en": "Apa Itu Lock-In Amplifier?", "id": "Mengekstrak Sinyal Dengan Frekuensi Referensi." },
  { "en": "Apa Itu G-Code?", "id": "Bahasa Pemrograman Untuk Mesin CNC." },
  { "en": "Apa Itu Assembly Language?", "id": "Bahasa Pemrograman Level Rendah." },
  { "en": "Apa Itu Compiler?", "id": "Penerjemah Kode Sumber Ke Kode Mesin." },
  { "en": "Apa Itu Interpreter?", "id": "Mengeksekusi Kode Sumber Baris Per Baris." },
  { "en": "Apa Itu Debugger?", "id": "Alat Bantu Untuk Mencari Bug Program." },
  { "en": "Apa Itu Emulator?", "id": "Mensimulasikan Perangkat Keras Dalam Perangkat Lunak." },
  { "en": "Apa Itu Simulator?", "id": "Mensimulasikan Perilaku Sistem." },
  { "en": "Apa Itu Real-Time Operating System?", "id": "Sistem Operasi Dengan Batasan Waktu Ketat." },
  { "en": "Apa Kepanjangan Dari RTOS?", "id": "Real-Time Operating System." },
  { "en": "Apa Itu Time-Division Multiplexing?", "id": "Berbagi Saluran Berdasarkan Slot Waktu." },
  { "en": "Apa Kepanjangan Dari TDM?", "id": "Time-Division Multiplexing." },
  { "en": "Apa Itu Frequency-Division Multiplexing?", "id": "Berbagi Saluran Berdasarkan Pita Frekuensi." },
  { "en": "Apa Kepanjangan Dari FDM?", "id": "Frequency-Division Multiplexing." },
  { "en": "Apa Itu Spread Spectrum?", "id": "Teknik Menyebar Sinyal Di Pita Lebar." },
  { "en": "Apa Itu Code Division Multiple Access?", "id": "Akses Jamak Berbasis Kode Unik." },
  { "en": "Apa Kepanjangan Dari CDMA?", "id": "Code Division Multiple Access." },
  { "en": "Apa Itu Duplex Communication?", "id": "Komunikasi Dua Arah Secara Bersamaan." },
  { "en": "Apa Itu Half-Duplex Communication?", "id": "Komunikasi Dua Arah Secara Bergantian." },
  { "en": "Apa Itu Simplex Communication?", "id": "Komunikasi Satu Arah Saja." },
  { "en": "Apa Target Prosesor ARM Cortex-M?", "id": "Aplikasi Mikrokontroler Dan Sistem Tertanam." },
  { "en": "Apa Itu Arsitektur RISC-V?", "id": "Arsitektur Set Instruksi Bebas Royalti." },
  { "en": "Apa Itu System Management Bus (SMBus)?", "id": "Bus Komunikasi Untuk Manajemen Daya." },
  { "en": "Apa Kepanjangan Dari SMBus (System Management Bus)?", "id": "System Management Bus." },
  { "en": "Apa Fungsi Power Management IC (PMIC)?", "id": "Mengelola Berbagai Kebutuhan Daya Sistem." },
  { "en": "Apa Kepanjangan Dari PMIC (Power Management Integrated Circuit)?", "id": "Power Management Integrated Circuit." },
  { "en": "Apa Fungsi Dari Ethernet PHY?", "id": "Mengelola Lapisan Fisik Jaringan Ethernet." },
  { "en": "Apa Kepanjangan Dari VCSEL (Vertical-Cavity Surface-Emitting Laser)?", "id": "Vertical-Cavity Surface-Emitting Laser." },
  { "en": "Apa Itu Teknologi Quantum Dots?", "id": "Nanokristal Semikonduktor Untuk Aplikasi Display." },
  { "en": "Apa Itu Teknologi E-Ink?", "id": "Display Bi-stabil Dengan Konsumsi Daya Rendah." },
  { "en": "Apa Yang Diukur Contrast Ratio?", "id": "Perbandingan Antara Bagian Terterang Tergelap." },
  { "en": "Apa Itu Color Gamut?", "id": "Rentang Warna Yang Dapat Ditampilkan." },
  { "en": "Apa Kepanjangan Antarmuka SATA (Serial Advanced Technology Attachment)?", "id": "Serial Advanced Technology Attachment." },
  { "en": "Apa Kepanjangan Antarmuka NVMe (Non-Volatile Memory Express)?", "id": "Non-Volatile Memory Express." },
  { "en": "Apa Kepanjangan Dari UFS (Universal Flash Storage)?", "id": "Universal Flash Storage." },
  { "en": "Apa Itu Differential Pair?", "id": "Dua Jalur Sinyal Komplementer." },
  { "en": "Apa Itu Skew Dalam Sinyal?", "id": "Perbedaan Waktu Kedatangan Antar Sinyal." },
  { "en": "Apa Itu Pre-Emphasis?", "id": "Meningkatkan Amplitudo Frekuensi Tinggi." },
  { "en": "Apa Itu Equalization?", "id": "Mengkompensasi Distorsi Sinyal Pada Saluran." },
  { "en": "Apa Kepanjangan Dari LoRaWAN (Long Range Wide Area Network)?", "id": "Long Range Wide Area Network." },
  { "en": "Apa Itu Jaringan Sigfox?", "id": "Jaringan LPWAN Untuk Perangkat IoT." },
  { "en": "Apa Itu RFID (Radio-Frequency Identification) Aktif?", "id": "Tag RFID Dengan Sumber Baterai Internal." },
  { "en": "Apa Itu RFID (Radio-Frequency Identification) Pasif?", "id": "Tag RFID Tanpa Baterai Internal." },
  { "en": "Apa Itu Chemical Vapor Deposition (CVD)?", "id": "Proses Deposisi Film Tipis." },
  { "en": "Apa Kepanjangan Dari CVD (Chemical Vapor Deposition)?", "id": "Chemical Vapor Deposition." },
  { "en": "Apa Itu Proses Etching?", "id": "Proses Pengikisan Material Secara Selektif." },
  { "en": "Apa Itu Ion Implantation?", "id": "Proses Introduksi Ion Dopan." },
  { "en": "Apa Kepanjangan Dari VNA (Vector Network Analyzer)?", "id": "Vector Network Analyzer." },
  { "en": "Apa Fungsi Utama VNA (Vector Network Analyzer)?", "id": "Mengukur Karakteristik Jaringan Frekuensi Radio." },
  { "en": "Apa Itu Protocol Analyzer?", "id": "Menganalisis Dan Mendekode Protokol Komunikasi." },
  { "en": "Apa Itu Software Defined Radio (SDR)?", "id": "Sistem Radio Dengan Komponen Didefinisikan Software." },
  { "en": "Apa Kepanjangan Dari SDR (Software Defined Radio)?", "id": "Software Defined Radio." },
  { "en": "Apa Arti Intrinsic Safety?", "id": "Desain Peralatan Untuk Area Berbahaya." },
  { "en": "Apa Kepanjangan Dari SIL (Safety Integrity Level)?", "id": "Safety Integrity Level." },
  { "en": "Apa Arti Dari IP Rating?", "id": "Tingkat Proteksi Terhadap Benda Padat Cair." },
  { "en": "Apa Inti Dari Persamaan Maxwell?", "id": "Hubungan Antara Kelistrikan Dan Kemagnetan." },
  { "en": "Apa Itu Near Field?", "id": "Area Dekat Antena." },
  { "en": "Apa Itu Far Field?", "id": "Area Jauh Dari Antena." },
  { "en": "Apa Itu Electromagnetic Susceptibility?", "id": "Tingkat Kerentanan Perangkat Terhadap EMI." },
  { "en": "Apa Itu Efek Miller?", "id": "Peningkatan Kapasitansi Input Karena Penguatan." },
  { "en": "Apa Itu Efek Early?", "id": "Variasi Lebar Basis Transistor." },
  { "en": "Apa Itu MEMS (Micro-Electro-Mechanical Systems) Oscillator?", "id": "Osilator Berbasis Sistem Mikro-Elektro-Mekanis." },
  { "en": "Apa Itu Solid State Relay (SSR)?", "id": "Relay Elektronik Tanpa Bagian Bergerak." },
  { "en": "Apa Kepanjangan Dari SSR (Solid State Relay)?", "id": "Solid State Relay." },
  { "en": "Apa Itu State Machine?", "id": "Model Perilaku Berdasarkan Keadaan Transisi." },
  { "en": "Apa Itu Single Point of Failure?", "id": "Satu Komponen Penyebab Kegagalan Sistem." },
  { "en": "Apa Itu Redundancy?", "id": "Duplikasi Komponen Kritis Untuk Keandalan." },
  { "en": "Apa Itu Fail-Safe System?", "id": "Sistem Yang Dirancang Aman Saat Gagal." },
  { "en": "Apa Itu Fault Tolerance?", "id": "Kemampuan Sistem Beroperasi Meski Ada Kesalahan." },
  { "en": "Apa Itu Hot Standby?", "id": "Sistem Cadangan Yang Siap Mengambil Alih." },
  { "en": "Apa Itu Cold Standby?", "id": "Sistem Cadangan Yang Perlu Dinyalakan." },
  { "en": "Apa Itu Latching Circuit?", "id": "Rangkaian Yang Mempertahankan Keadaannya." },
  { "en": "Apa Itu Anti-Fuse?", "id": "Sekring Yang Awalnya Terbuka." },
  { "en": "Apa Itu Phase Noise?", "id": "Fluktuasi Fasa Acak Pada Sinyal." },
  { "en": "Apa Itu Signal Integrity?", "id": "Ukuran Kualitas Sinyal Listrik." },
  { "en": "Apa Itu Power Integrity?", "id": "Kualitas Catu Daya Pada Rangkaian." },
  { "en": "Apa Itu Decimation Filter?", "id": "Filter Untuk Mengurangi Laju Sampling." },
  { "en": "Apa Itu Boxcar Averager?", "id": "Teknik Untuk Mengurangi Noise Sinyal." },
  { "en": "Apa Itu Moving Average Filter?", "id": "Filter Digital Berbasis Rata-Rata Bergerak." },
  { "en": "Apa Itu Kalman Filter?", "id": "Algoritma Estimasi Untuk Mengurangi Noise." },
  { "en": "Apa Itu Pi Network?", "id": "Jaringan Filter Dengan Bentuk Pi." },
  { "en": "Apa Itu T Network?", "id": "Jaringan Filter Dengan Bentuk T." },
  { "en": "Apa Itu Parasitic Capacitance?", "id": "Kapasitansi Yang Tidak Diinginkan." },
  { "en": "Apa Itu Parasitic Inductance?", "id": "Induktansi Yang Tidak Diinginkan." },
  { "en": "Apa Itu Crosspoint Switch?", "id": "Matriks Saklar Untuk Menghubungkan Input Output." },
  { "en": "Apa Itu Space-Vector Modulation?", "id": "Teknik Modulasi PWM Untuk Inverter." },
  { "en": "Apa Itu Dead-Band?", "id": "Area Dimana Perubahan Input Tidak Berefek." },
  { "en": "Apa Itu Dropout Voltage?", "id": "Perbedaan Tegangan Minimum Regulator." },
  { "en": "Apa Itu Headroom?", "id": "Rentang Antara Sinyal Maksimal Batas Sistem." },
  { "en": "Apa Itu Crest Factor?", "id": "Rasio Nilai Puncak Terhadap Nilai RMS." },
  { "en": "Apa Itu RMS (Root Mean Square) Value?", "id": "Nilai Efektif Dari Sinyal AC." },
  { "en": "Apa Kepanjangan Dari RMS (Root Mean Square)?", "id": "Root Mean Square." },
  { "en": "Apa Itu Guard Band?", "id": "Pita Frekuensi Kosong Antar Saluran." },
  { "en": "Apa Itu Side Lobe?", "id": "Radiasi Minor Pada Pola Antena." },
  { "en": "Apa Itu Main Lobe?", "id": "Radiasi Utama Pada Pola Antena." },
  { "en": "Apa Itu Antenna Array?", "id": "Kumpulan Elemen Antena." },
  { "en": "Apa Itu Phase-Locked Dielectric Resonator Oscillator (PLDRO)?", "id": "Osilator Frekuensi Mikro Stabil." },
  { "en": "Apa Kepanjangan Dari PLDRO?", "id": "Phase-Locked Dielectric Resonator Oscillator." },
  { "en": "Apa Itu Gilbert Cell?", "id": "Sirkuit Mixer Empat-Kuadran." },
  { "en": "Apa Itu Cascode Amplifier?", "id": "Konfigurasi Penguat Untuk Bandwidth Tinggi." },
  { "en": "Apa Itu Current Feedback Amplifier?", "id": "Tipe Penguat Operasional Kecepatan Tinggi." },
  { "en": "Apa Itu Chopper Amplifier?", "id": "Penguat Untuk Mengurangi Offset DC." },
  { "en": "Apa Itu Flying Capacitor?", "id": "Kapasitor Untuk Transfer Muatan." },
  { "en": "Apa Itu Dickson Charge Pump?", "id": "Topologi Sirkuit Pengganda Tegangan." },
  { "en": "Apa Itu Gate Driver?", "id": "Sirkuit Penggerak Gerbang MOSFET IGBT." },
  { "en": "Apa Itu Level Shifter?", "id": "Mengubah Level Tegangan Sinyal Logika." },
  { "en": "Apa Itu Metastable Hardening?", "id": "Teknik Mengurangi Kemungkinan Metastabilitas." },
  { "en": "Apa Itu Gray Code?", "id": "Kode Biner Dimana Nilai Berurutan." },
  { "en": "Apa Itu One-Hot Encoding?", "id": "Representasi Status Dengan Satu Bit Aktif." },
  { "en": "Apa Itu Hamming Code?", "id": "Kode Deteksi Dan Koreksi Kesalahan." },
  { "en": "Apa Itu Cyclic Redundancy Check (CRC)?", "id": "Fungsi Pengecekan Kesalahan Data." },
  { "en": "Apa Kepanjangan Dari CRC (Cyclic Redundancy Check)?", "id": "Cyclic Redundancy Check." },
  { "en": "Apa Yang Direpresentasikan Oleh Nyquist Plot?", "id": "Stabilitas Sistem Kontrol Umpan Balik." },
  { "en": "Apa Itu Root Locus Analysis?", "id": "Metode Grafis Untuk Menganalisis Stabilitas." },
  { "en": "Apa Itu State-Space Representation?", "id": "Model Matematis Sistem Kontrol." },
  { "en": "Apa Itu Controllability?", "id": "Kemampuan Mengontrol Keadaan Internal Sistem." },
  { "en": "Apa Itu Observability?", "id": "Kemampuan Mengamati Keadaan Internal Sistem." },
  { "en": "Apa Itu Jalur Transmisi Microstrip?", "id": "Jalur Konduktor Di Atas Bidang Ground." },
  { "en": "Apa Itu Jalur Transmisi Stripline?", "id": "Jalur Konduktor Di Antara Dua Ground." },
  { "en": "Apa Itu Characteristic Impedance?", "id": "Impedansi Khas Sebuah Saluran Transmisi." },
  { "en": "Apa Itu Skin Depth?", "id": "Kedalaman Penetrasi Arus AC." },
  { "en": "Apa Itu Photomultiplier Tube (PMT)?", "id": "Detektor Cahaya Yang Sangat Sensitif." },
  { "en": "Apa Kepanjangan Dari PMT (Photomultiplier Tube)?", "id": "Photomultiplier Tube." },
  { "en": "Apa Itu Avalanche Photodiode (APD)?", "id": "Fotodioda Dengan Penguatan Internal." },
  { "en": "Apa Kepanjangan Dari APD (Avalanche Photodiode)?", "id": "Avalanche Photodiode." },
  { "en": "Apa Itu Thyristor?", "id": "Komponen Semikonduktor Empat Lapis." },
  { "en": "Apa Kepanjangan Algoritma FFT (Fast Fourier Transform)?", "id": "Fast Fourier Transform." },
  { "en": "Apa Fungsi Windowing Dalam DSP?", "id": "Mengurangi Kebocoran Spektral Saat Analisis." },
  { "en": "Apa Itu Proses Convolution?", "id": "Operasi Matematis Untuk Analisis Sistem." },
  { "en": "Apa Itu Proses Correlation?", "id": "Mengukur Kemiripan Antara Dua Sinyal." },
  { "en": "Apa Satuan Logaritmik dBm?", "id": "Daya Relatif Terhadap Satu Miliwatt." },
  { "en": "Apa Itu Charge-Coupled Device (CCD)?", "id": "Tipe Sensor Gambar Elektronik." },
  { "en": "Apa Kepanjangan Dari CCD (Charge-Coupled Device)?", "id": "Charge-Coupled Device." },
  { "en": "Apa Itu Bayer Filter?", "id": "Filter Pola Mosaik Warna." },
  { "en": "Apa Itu Pixel Binning?", "id": "Menggabungkan Beberapa Piksel Menjadi Satu." },
  { "en": "Apa Itu Local Interconnect Network (LIN) Bus?", "id": "Protokol Komunikasi Biaya Rendah." },
  { "en": "Apa Kepanjangan Dari LIN (Local Interconnect Network) Bus?", "id": "Local Interconnect Network." },
  { "en": "Apa Itu Protokol FlexRay?", "id": "Protokol Komunikasi Cepat Untuk Otomotif." },
  { "en": "Apa Itu Standar ISO 26262?", "id": "Standar Keamanan Fungsional Untuk Otomotif." },
  { "en": "Apa Kepanjangan Sinyal ECG (Electrocardiogram)?", "id": "Electrocardiogram." },
  { "en": "Apa Yang Diukur Oleh ECG?", "id": "Aktivitas Listrik Jantung." },
  { "en": "Apa Kepanjangan Sinyal EEG (Electroencephalogram)?", "id": "Electroencephalogram." },
  { "en": "Apa Yang Diukur Oleh EEG?", "id": "Aktivitas Listrik Otak." },
  { "en": "Apa Kepanjangan Sinyal EMG (Electromyography)?", "id": "Electromyography." },
  { "en": "Apa Yang Diukur Oleh EMG?", "id": "Aktivitas Listrik Otot." },
  { "en": "Bagaimana Cara Kerja Pulse Oximeter?", "id": "Mengukur Saturasi Oksigen Dalam Darah." },
  { "en": "Apa Topologi Cuk Converter?", "id": "Konverter DC-DC Dengan Output Negatif." },
  { "en": "Apa Topologi SEPIC Converter?", "id": "Konverter Yang Dapat Menaikkan Menurunkan Tegangan." },
  { "en": "Apa Kepanjangan Dari SEPIC?", "id": "Single-Ended Primary-Inductor Converter." },
  { "en": "Apa Topologi Flyback Converter?", "id": "Konverter Terisolasi Berbasis Buck-Boost." },
  { "en": "Apa Itu Qubit?", "id": "Unit Dasar Komputasi Kuantum." },
  { "en": "Apa Itu Superposition Kuantum?", "id": "Kemampuan Qubit Berada Dalam Banyak Keadaan." },
  { "en": "Apa Itu Entanglement Kuantum?", "id": "Keterikatan Antar Qubit." },
  { "en": "Apa Kepanjangan Dari IEEE?", "id": "Institute of Electrical and Electronics Engineers." },
  { "en": "Apa Kepanjangan Dari IEC?", "id": "International Electrotechnical Commission." },
  { "en": "Apa Kepanjangan Dari JEDEC?", "id": "Joint Electron Device Engineering Council." },
  { "en": "Apa Kepanjangan Dari IPC?", "id": "Association Connecting Electronics Industries." },
  { "en": "Apa Itu Delta-Wye Transformation?", "id": "Transformasi Antara Konfigurasi Rangkaian." },
  { "en": "Apa Itu Maximum Power Transfer Theorem?", "id": "Teorema Transfer Daya Maksimum." },
  { "en": "Apa Itu Supernode Analysis?", "id": "Teknik Analisis Rangkaian Dengan Sumber Tegangan." },
  { "en": "Apa Itu Supermesh Analysis?", "id": "Teknik Analisis Rangkaian Dengan Sumber Arus." },
  { "en": "Apa Itu Initial Condition?", "id": "Kondisi Awal Rangkaian Saat t=0." },
  { "en": "Apa Itu Steady-State Response?", "id": "Respon Rangkaian Setelah Waktu Lama." },
  { "en": "Apa Itu Transient Response?", "id": "Respon Sementara Rangkaian." },
  { "en": "Apa Itu Natural Response?", "id": "Respon Rangkaian Tanpa Sumber Eksternal." },
  { "en": "Apa Itu Forced Response?", "id": "Respon Rangkaian Akibat Sumber Eksternal." },
  { "en": "Apa Itu Damping Ratio?", "id": "Ukuran Redaman Osilasi Dalam Sistem." },
  { "en": "Apa Itu Overdamped System?", "id": "Sistem Yang Kembali Ke Keseimbangan Lambat." },
  { "en": "Apa Itu Critically Damped System?", "id": "Sistem Yang Kembali Ke Keseimbangan Cepat." },
  { "en": "Apa Itu Underdamped System?", "id": "Sistem Yang Berosilasi Sebelum Seimbang." },
  { "en": "Apa Itu Resonant Frequency?", "id": "Frekuensi Osilasi Alami Maksimum." },
  { "en": "Apa Itu Barkhausen Stability Criterion?", "id": "Kriteria Untuk Terjadinya Osilasi." },
  { "en": "Apa Itu Colpitts Oscillator?", "id": "Osilator LC Dengan Pembagi Tegangan Kapasitif." },
  { "en": "Apa Itu Hartley Oscillator?", "id": "Osilator LC Dengan Pembagi Tegangan Induktif." },
  { "en": "Apa Itu Wien Bridge Oscillator?", "id": "Osilator Gelombang Sinus Berbasis Jembatan Wien." },
  { "en": "Apa Itu Phase Margin?", "id": "Ukuran Stabilitas Relatif Sistem Kontrol." },
  { "en": "Apa Itu Gain Margin?", "id": "Ukuran Lain Stabilitas Relatif." },
  { "en": "Apa Itu Negative Feedback?", "id": "Umpan Balik Yang Menstabilkan Sistem." },
  { "en": "Apa Itu Positive Feedback?", "id": "Umpan Balik Yang Dapat Memicu Osilasi." },
  { "en": "Apa Itu Operational Transconductance Amplifier (OTA)?", "id": "Penguat Dengan Output Arus." },
  { "en": "Apa Kepanjangan Dari OTA?", "id": "Operational Transconductance Amplifier." },
  { "en": "Apa Itu Current Conveyor?", "id": "Blok Bangunan Sirkuit Analog." },
  { "en": "Apa Itu Switched Capacitor Circuit?", "id": "Mensimulasikan Resistor Dengan Kapasitor Saklar." },
  { "en": "Apa Itu Verilog-A?", "id": "Bahasa Pemodelan Perilaku Sirkuit Analog." },
  { "en": "Apa Itu Monte Carlo Analysis?", "id": "Analisis Statistik Untuk Rangkaian." },
  { "en": "Apa Itu Corner Analysis?", "id": "Analisis Performa Di Kondisi Ekstrem." },
  { "en": "Apa Itu Latch-Up Test?", "id": "Pengujian Kerentanan IC Terhadap Latch-Up." },
  { "en": "Apa Itu Soft Error?", "id": "Kesalahan Data Sementara Akibat Radiasi." },
  { "en": "Apa Itu Hard Error?", "id": "Kerusakan Fisik Permanen." },
  { "en": "Apa Itu Wear Leveling?", "id": "Teknik Distribusi Penulisan Di Memori Flash." },
  { "en": "Apa Itu Garbage Collection (Memori)?", "id": "Proses Mengklaim Kembali Ruang Memori." },
  { "en": "Apa Itu Bad Block Management?", "id": "Manajemen Blok Rusak Di NAND Flash." },
  { "en": "Apa Itu Serial Presence Detect (SPD)?", "id": "Informasi Konfigurasi Modul Memori." },
  { "en": "Apa Kepanjangan Dari SPD?", "id": "Serial Presence Detect." },
  { "en": "Apa Itu Error Detection And Correction (EDAC)?", "id": "Sistem Deteksi Dan Koreksi Kesalahan." },
  { "en": "Apa Kepanjangan Dari EDAC?", "id": "Error Detection And Correction." },
  { "en": "Apa Itu Parity Check?", "id": "Metode Sederhana Pengecekan Kesalahan." },
  { "en": "Apa Itu Inter-Integrated Sound (I2S) Bus?", "id": "Bus Komunikasi Audio Digital." },
  { "en": "Apa Kepanjangan Dari I2S?", "id": "Inter-Integrated Sound." },
  { "en": "Apa Itu S/PDIF Interface?", "id": "Format Antarmuka Audio Digital." },
  { "en": "Apa Kepanjangan Dari S/PDIF?", "id": "Sony/Philips Digital Interface Format." },
  { "en": "Apa Itu Audio Return Channel (ARC)?", "id": "Fitur HDMI Untuk Mengirim Audio Kembali." },
  { "en": "Apa Kepanjangan Dari ARC?", "id": "Audio Return Channel." },
  { "en": "Apa Itu Prinsip Design for Manufacturability (DFM)?", "id": "Desain Yang Memudahkan Proses Produksi." },
  { "en": "Apa Kepanjangan Dari DFM?", "id": "Design for Manufacturability." },
  { "en": "Apa Itu Prinsip Design for Testability (DFT)?", "id": "Desain Yang Memudahkan Proses Pengujian." },
  { "en": "Apa Kepanjangan Dari DFT?", "id": "Design for Testability." },
  { "en": "Apa Itu Design for Assembly (DFA)?", "id": "Desain Yang Memudahkan Proses Perakitan." },
  { "en": "Apa Itu Via-in-Pad?", "id": "Via Yang Ditempatkan Langsung Di Pad Komponen." },
  { "en": "Apa Itu Blind Via?", "id": "Via Penghubung Lapisan Luar Dan Dalam." },
  { "en": "Apa Itu Buried Via?", "id": "Via Terkubur Antara Dua Lapisan Dalam." },
  { "en": "Apa Itu Annular Ring?", "id": "Area Tembaga Sekitar Lubang Via." },
  { "en": "Apa Fungsi Fitur Teardrop PCB?", "id": "Memperkuat Sambungan Antara Trace Via." },
  { "en": "Apa Itu Controlled Impedance Routing?", "id": "Merutekan Jalur Dengan Impedansi Spesifik." },
  { "en": "Apa Fungsi Stitching Vias?", "id": "Menghubungkan Bidang Ground Di Lapisan Berbeda." },
  { "en": "Apa Itu Perusahaan Foundry Semikonduktor?", "id": "Perusahaan Manufaktur Chip Untuk Pihak Lain." },
  { "en": "Apa Itu Perusahaan Fabless?", "id": "Perusahaan Desain Chip Tanpa Fasilitas Pabrikasi." },
  { "en": "Apa Itu Integrated Device Manufacturer (IDM)?", "id": "Perusahaan Desain Dan Pabrikasi Chip." },
  { "en": "Apa Kepanjangan Dari IDM?", "id": "Integrated Device Manufacturer." },
  { "en": "Apa Itu Process Node?", "id": "Ukuran Teknologi Manufaktur Semikonduktor." },
  { "en": "Apa Itu Yield Dalam Manufaktur?", "id": "Persentase Produk Fungsional Dari Total Produksi." },
  { "en": "Apa Itu Return Path?", "id": "Jalur Kembali Arus Dalam Rangkaian." },
  { "en": "Apa Itu Power Delivery Network (PDN)?", "id": "Jaringan Penyedia Daya Di PCB." },
  { "en": "Apa Kepanjangan Dari PDN?", "id": "Power Delivery Network." },
  { "en": "Apa Itu Model IBIS?", "id": "Model Perilaku Input Output Buffer." },
  { "en": "Apa Kepanjangan Dari IBIS?", "id": "Input/Output Buffer Information Specification." },
  { "en": "Apa Itu Multipath Fading?", "id": "Pelemahan Sinyal Akibat Pantulan." },
  { "en": "Apa Itu Radiated Emissions?", "id": "Pancaran EMI Yang Tidak Diinginkan." },
  { "en": "Apa Itu Conducted Emissions?", "id": "EMI Yang Merambat Melalui Kabel." },
  { "en": "Apa Itu Sertifikasi UL?", "id": "Standar Keamanan Produk Dari Underwriters Laboratories." },
  { "en": "Apa Kepanjangan Dari DDR SDRAM?", "id": "Double Data Rate Synchronous Dynamic RAM." },
  { "en": "Apa Itu CAS (Column Address Strobe) Latency?", "id": "Penundaan Sebelum Transfer Data Dari RAM." },
  { "en": "Apa Kepanjangan Dari CAS?", "id": "Column Address Strobe." },
  { "en": "Apa Itu Time-of-Flight (ToF) Sensor?", "id": "Sensor Pengukur Jarak Berbasis Waktu." },
  { "en": "Apa Kepanjangan Dari ToF?", "id": "Time-of-Flight." },
  { "en": "Apa Itu LIDAR?", "id": "Metode Penginderaan Jauh Menggunakan Laser." },
  { "en": "Apa Kepanjangan Dari LIDAR?", "id": "Light Detection and Ranging." },
  { "en": "Apa Itu Barometric Pressure Sensor?", "id": "Sensor Pengukur Tekanan Atmosfer." },
  { "en": "Apa Itu Capacitive Sensing?", "id": "Teknik Deteksi Berbasis Perubahan Kapasitansi." },
  { "en": "Apa Itu Version Control System (VCS)?", "id": "Sistem Pengelola Perubahan Kode Sumber." },
  { "en": "Apa Itu Continuous Integration (CI)?", "id": "Praktik Mengintegrasikan Kode Secara Berkala." },
  { "en": "Apa Itu Hardware-in-the-Loop (HIL) Testing?", "id": "Pengujian Dengan Simulasi Komponen Fisik." },
  { "en": "Apa Kepanjangan Dari HIL?", "id": "Hardware-in-the-Loop." },
  { "en": "Apa Itu Unit Testing?", "id": "Pengujian Unit Terkecil Dari Kode." },
  { "en": "Apa Itu Voice Coil?", "id": "Aktuator Elektromagnetik Dalam Speaker." },
  { "en": "Apa Itu Potensiometer Logaritmik?", "id": "Potensiometer Untuk Kontrol Audio." },
  { "en": "Apa Itu Rotary Encoder?", "id": "Sensor Pengukur Posisi Sudut." },
  { "en": "Apa Itu Incremental Encoder?", "id": "Encoder Yang Memberikan Sinyal Pulsa Relatif." },
  { "en": "Apa Itu Absolute Encoder?", "id": "Encoder Yang Memberikan Posisi Absolut." },
  { "en": "Apa Itu Pick-and-Place Machine?", "id": "Mesin Otomatis Pemasang Komponen SMD." },
  { "en": "Apa Itu Bill of Materials (BOM) Scrubbing?", "id": "Proses Verifikasi Dan Pembersihan Data BOM." },
  { "en": "Apa Itu First Article Inspection (FAI)?", "id": "Inspeksi Produk Pertama Dari Jalur Produksi." },
  { "en": "Apa Itu Non-Recurring Engineering (NRE) Cost?", "id": "Biaya Satu Kali Untuk Desain Produk." },
  { "en": "Apa Kepanjangan Dari NRE?", "id": "Non-Recurring Engineering." },
  { "en": "Apa Itu Golden Sample?", "id": "Sampel Referensi Yang Telah Disetujui." },
  { "en": "Apa Itu System-on-Module (SoM)?", "id": "Modul Komputer Siap Pakai." },
  { "en": "Apa Kepanjangan Dari SoM?", "id": "System-on-Module." },
  { "en": "Apa Itu Carrier Board?", "id": "Papan Utama Tempat SoM Dipasang." },
  { "en": "Apa Itu Mezzanine Connector?", "id": "Konektor Untuk Menghubungkan Dua PCB Paralel." },
  { "en": "Apa Itu Pogo Pin?", "id": "Pin Kontak Berpegas." },
  { "en": "Apa Itu Ball Grid Array (BGA)?", "id": "Jenis Kemasan Komponen Surface-Mount." },
  { "en": "Apa Kepanjangan Dari BGA?", "id": "Ball Grid Array." },
  { "en": "Apa Itu Reflow Profile?", "id": "Grafik Suhu Terhadap Waktu Proses Reflow." },
  { "en": "Apa Itu Wetting Dalam Penyolderan?", "id": "Kemampuan Solder Cair Membasahi Permukaan." },
  { "en": "Apa Itu Tombstoning?", "id": "Cacat Solder Dimana Komponen Berdiri." },
  { "en": "Apa Itu Solder Bridging?", "id": "Hubungan Singkat Akibat Solder Berlebih." },
  { "en": "Apa Itu Cold Solder Joint?", "id": "Sambungan Solder Buruk Akibat Suhu Rendah." },
  { "en": "Apa Itu Convection Oven?", "id": "Oven Yang Digunakan Untuk Reflow Soldering." },
  { "en": "Apa Itu Flux?", "id": "Bahan Kimia Pembersih Permukaan Solder." },
  { "en": "Apa Itu No-Clean Flux?", "id": "Flux Yang Tidak Perlu Dibersihkan." },
  { "en": "Apa Itu Design Rule Check (DRC)?", "id": "Pemeriksaan Aturan Desain PCB Otomatis." },
  { "en": "Apa Kepanjangan Dari DRC?", "id": "Design Rule Check." },
  { "en": "Apa Itu Electrical Rule Check (ERC)?", "id": "Pemeriksaan Aturan Kelistrikan Skematik." },
  { "en": "Apa Kepanjangan Dari ERC?", "id": "Electrical Rule Check." },
  { "en": "Apa Itu Netlist?", "id": "Deskripsi Konektivitas Dalam Rangkaian." },
  { "en": "Apa Itu Pin Pitch?", "id": "Jarak Antara Pusat Pin Berdekatan." },
  { "en": "Apa Itu Keep-Out Area?", "id": "Area Di PCB Yang Dilarang." },
  { "en": "Apa Itu Fiducial Mark?", "id": "Tanda Referensi Optik Untuk Mesin." },
  { "en": "Apa Itu Test Point?", "id": "Titik Akses Untuk Pengujian Rangkaian." },
  { "en": "Apa Itu Bare Board Testing?", "id": "Pengujian PCB Tanpa Komponen." },
  { "en": "Apa Itu Flying Probe Test?", "id": "Metode Pengujian PCB Tanpa Fixture." },
  { "en": "Apa Itu Bed of Nails Fixture?", "id": "Fixture Pengujian Dengan Banyak Pin Kontak." },
  { "en": "Apa Itu Signal Tap?", "id": "Metode Debugging Logika Internal FPGA." },
  { "en": "Apa Itu Hard Processor System (HPS)?", "id": "Inti Prosesor Keras Dalam FPGA." },
  { "en": "Apa Kepanjangan Dari HPS?", "id": "Hard Processor System." },
  { "en": "Apa Itu Soft-Core Processor?", "id": "Prosesor Yang Diimplementasikan Dalam Logika FPGA." },
  { "en": "Apa Itu Intellectual Property (IP) Core?", "id": "Blok Desain Logika Siap Pakai." },
  { "en": "Apa Kepanjangan Dari IP (Intellectual Property) Core?", "id": "Intellectual Property." },
  { "en": "Apa Itu Synthesis Dalam Desain Digital?", "id": "Proses Menerjemahkan HDL Ke Gerbang Logika." },
  { "en": "Apa Itu Place and Route?", "id": "Proses Penempatan Perutean Logika Di Chip." },
  { "en": "Apa Itu Timing Closure?", "id": "Proses Memenuhi Persyaratan Waktu Desain." },
  { "en": "Apa Itu Static Timing Analysis (STA)?", "id": "Analisis Waktu Tanpa Simulasi." },
  { "en": "Apa Kepanjangan Dari STA?", "id": "Static Timing Analysis." },
  { "en": "Apa Itu Clock Skew?", "id": "Perbedaan Waktu Kedatangan Sinyal Clock." },
  { "en": "Apa Itu Clock Jitter?", "id": "Variasi Periode Sinyal Clock." },
  { "en": "Apa Itu Clock Domain Crossing (CDC)?", "id": "Transfer Data Antar Domain Clock Berbeda." },
  { "en": "Apa Kepanjangan Dari CDC?", "id": "Clock Domain Crossing." },
  { "en": "Apa Keunggulan Semikonduktor Gallium Nitride (GaN)?", "id": "Frekuensi Switching Dan Efisiensi Sangat Tinggi." },
  { "en": "Apa Keunggulan Semikonduktor Silicon Carbide (SiC)?", "id": "Tegangan Tembus Dan Konduktivitas Termal Tinggi." },
  { "en": "Apa Itu Graphene?", "id": "Lapisan Tunggal Atom Karbon." },
  { "en": "Apa Itu Spintronics?", "id": "Elektronik Berbasis Spin Elektron." },
  { "en": "Apa Itu Efek Quantum Tunneling?", "id": "Partikel Menembus Penghalang Potensial." },
  { "en": "Apa Itu Standard Cell Library?", "id": "Kumpulan Gerbang Logika Dasar Desain IC." },
  { "en": "Apa Itu Proses Floorplanning?", "id": "Tahap Awal Penataan Blok Desain IC." },
  { "en": "Apa Itu Power Gating?", "id": "Teknik Mematikan Daya Blok Tidak Aktif." },
  { "en": "Apa Itu Clock Gating?", "id": "Teknik Mematikan Clock Blok Tidak Aktif." },
  { "en": "Apa Tujuan Pemeriksaan LVS (Layout Versus Schematic)?", "id": "Memastikan Layout Cocok Dengan Skematik." },
  { "en": "Apa Kepanjangan Dari LVS?", "id": "Layout Versus Schematic." },
  { "en": "Apa Itu Antenna Effect Dalam Desain IC?", "id": "Kerusakan Gate Akibat Pengumpulan Muatan." },
  { "en": "Apa Fungsi Low-Noise Amplifier (LNA)?", "id": "Menguatkan Sinyal Lemah Tanpa Noise Signifikan." },
  { "en": "Apa Kepanjangan Dari LNA?", "id": "Low-Noise Amplifier." },
  { "en": "Apa Itu Gain Compression (P1dB)?", "id": "Titik Dimana Penguatan Turun 1 dB." },
  { "en": "Apa Itu Third-Order Intercept Point (IP3)?", "id": "Ukuran Linearitas Penguat Atau Mixer." },
  { "en": "Apa Kepanjangan Dari IP3?", "id": "Third-Order Intercept Point." },
  { "en": "Apa Itu Interrupt Vector Table?", "id": "Tabel Alamat Penangan Interupsi." },
  { "en": "Apa Itu Memory-Mapped I/O?", "id": "Mengakses Perangkat I/O Seperti Memori." },
  { "en": "Sebutkan Tujuh Lapisan Model OSI?", "id": "Physical Data-Link Network Transport Session Presentation Application." },
  { "en": "Apa Itu Ethernet Frame?", "id": "Struktur Paket Data Dalam Jaringan Ethernet." },
  { "en": "Apa Itu Virtual LAN (VLAN)?", "id": "Segmen Jaringan Logis Dalam Switch." },
  { "en": "Apa Kepanjangan Dari VLAN?", "id": "Virtual Local Area Network." },
  { "en": "Apa Itu Barrel Shifter?", "id": "Sirkuit Digital Penggeser Bit." },
  { "en": "Apa Keunggulan Carry-lookahead Adder?", "id": "Penjumlahan Biner Yang Sangat Cepat." },
  { "en": "Apa Itu Wallace Tree Multiplier?", "id": "Sirkuit Pengali Biner Berkecepatan Tinggi." },
  { "en": "Apa Kepanjangan Uji HALT (Highly Accelerated Life Test)?", "id": "Highly Accelerated Life Test." },
  { "en": "Apa Tujuan Pengujian HALT?", "id": "Menemukan Kelemahan Desain Secara Cepat." },
  { "en": "Apa Kepanjangan Uji HASS (Highly Accelerated Stress Screen)?", "id": "Highly Accelerated Stress Screen." },
  { "en": "Apa Tujuan Pengujian HASS?", "id": "Menyaring Kegagalan Produksi." },
  { "en": "Apa Itu Peringkat NEMA?", "id": "Standar Amerika Untuk Selungkup Peralatan Listrik." },
  { "en": "Apa Itu Ruang Warna YUV?", "id": "Sistem Pengkodean Warna Video Analog." },
  { "en": "Apa Itu Chroma Subsampling?", "id": "Teknik Kompresi Informasi Warna." },
  { "en": "Apa Itu I-frame Dalam Video?", "id": "Frame Video Lengkap Independen." },
  { "en": "Apa Itu P-frame Dalam Video?", "id": "Frame Prediksi Dari Frame Sebelumnya." },
  { "en": "Apa Itu B-frame Dalam Video?", "id": "Frame Prediksi Dua Arah." },
  { "en": "Apa Topologi Online UPS (Uninterruptible Power Supply)?", "id": "Baterai Selalu Terhubung Ke Inverter." },
  { "en": "Apa Kepanjangan Dari UPS?", "id": "Uninterruptible Power Supply." },
  { "en": "Apa Itu Teknologi Power over Ethernet (PoE)?", "id": "Daya Listrik Melalui Kabel Ethernet." },
  { "en": "Apa Kepanjangan Dari PoE?", "id": "Power over Ethernet." },
  { "en": "Apa Itu Haptics?", "id": "Teknologi Umpan Balik Sentuhan." },
  { "en": "Apa Itu Thermoelectric Generator?", "id": "Mengubah Panas Menjadi Listrik." },
  { "en": "Apa Itu Efek Seebeck?", "id": "Dasar Kerja Thermoelectric Generator." },
  { "en": "Apa Itu Efek Pyroelectric?", "id": "Kemampuan Bahan Menghasilkan Tegangan Dari Suhu." },
  { "en": "Apa Itu Ferromagnetism?", "id": "Sifat Magnetisme Kuat Pada Material." },
  { "en": "Apa Itu Dielectric Strength?", "id": "Kekuatan Maksimum Medan Listrik Isolator." },
  { "en": "Apa Itu Tan Delta?", "id": "Ukuran Kerugian Dielektrik Suatu Material." },
  { "en": "Apa Itu Curie Temperature?", "id": "Suhu Dimana Material Kehilangan Magnet Permanen." },
  { "en": "Apa Itu Hall Sensor?", "id": "Sensor Yang Mendeteksi Medan Magnet." },
  { "en": "Apa Itu Magnetoresistance?", "id": "Perubahan Resistansi Akibat Medan Magnet." },
  { "en": "Apa Itu Giant Magnetoresistance (GMR)?", "id": "Efek Magnetoresistance Yang Sangat Besar." },
  { "en": "Apa Kepanjangan Dari GMR?", "id": "Giant Magnetoresistance." },
  { "en": "Apa Itu Superparamagnetism?", "id": "Bentuk Magnetisme Pada Partikel Nano." },
  { "en": "Apa Itu Electromagnetic Pulse (EMP)?", "id": "Ledakan Radiasi Elektromagnetik Singkat." },
  { "en": "Apa Kepanjangan Dari EMP?", "id": "Electromagnetic Pulse." },
  { "en": "Apa Itu Near-Field Communication (NFC) Tag?", "id": "Tag Pasif Untuk Komunikasi NFC." },
  { "en": "Apa Itu Radio-Frequency Identification (RFID) Reader?", "id": "Pembaca Untuk Berkomunikasi Dengan Tag RFID." },
  { "en": "Apa Itu Backscatter Communication?", "id": "Komunikasi Dengan Memantulkan Sinyal RF." },
  { "en": "Apa Itu Energy Harvesting?", "id": "Memanen Energi Dari Lingkungan Sekitar." },
  { "en": "Apa Itu Photovoltaic Effect?", "id": "Dasar Kerja Sel Surya." },
  { "en": "Apa Itu Maximum Power Point Tracking (MPPT)?", "id": "Teknik Optimalisasi Daya Panel Surya." },
  { "en": "Apa Kepanjangan Dari MPPT?", "id": "Maximum Power Point Tracking." },
  { "en": "Apa Itu Buck-Boost Converter?", "id": "Konverter Penaik Penurun Tegangan." },
  { "en": "Apa Itu Forward Converter?", "id": "Topologi Konverter DC-DC Terisolasi." },
  { "en": "Apa Itu Full-Bridge Converter?", "id": "Konverter Jembatan Penuh Untuk Daya Tinggi." },
  { "en": "Apa Itu Half-Bridge Converter?", "id": "Konverter Jembatan Setengah." },
  { "en": "Apa Itu Push-Pull Converter?", "id": "Konverter Dengan Transformator Center-Tapped." },
  { "en": "Apa Itu Snubber Circuit?", "id": "Rangkaian Peredam Lonjakan Tegangan." },
  { "en": "Apa Itu Ringing Dalam Sinyal?", "id": "Osilasi Yang Tidak Diinginkan." },
  { "en": "Apa Itu Ground Plane?", "id": "Lapisan Tembaga Besar Sebagai Ground." },
  { "en": "Apa Itu Star Grounding?", "id": "Teknik Grounding Dengan Satu Titik Pusat." },
  { "en": "Apa Itu Mixed-Signal IC?", "id": "IC Dengan Sirkuit Analog Dan Digital." },
  { "en": "Apa Itu System-in-Package (SiP)?", "id": "Beberapa Chip Dalam Satu Kemasan." },
  { "en": "Apa Itu Chip Scale Package (CSP)?", "id": "Kemasan IC Seukuran Chipnya." },
  { "en": "Apa Kepanjangan Dari CSP?", "id": "Chip Scale Package." },
  { "en": "Apa Itu Wire Bonding?", "id": "Metode Menghubungkan Chip Ke Kemasan." },
  { "en": "Apa Itu Flip-Chip?", "id": "Metode Pemasangan Chip Terbalik." },
  { "en": "Apa Itu Underfill?", "id": "Bahan Pelindung Di Bawah Chip." },
  { "en": "Apa Itu Application Note?", "id": "Dokumen Cara Menggunakan Komponen." },
  { "en": "Apa Itu White Paper?", "id": "Laporan Teknis Mendalam Suatu Topik." },
  { "en": "Apa Itu Reference Design?", "id": "Desain Rangkaian Referensi Dari Pabrikan." },
  { "en": "Apa Itu Evaluation Kit?", "id": "Kit Untuk Mengevaluasi Kinerja Komponen." },
  { "en": "Apa Kepanjangan Dari EVK?", "id": "Evaluation Kit." },
  { "en": "Apa Itu Software Development Kit (SDK)?", "id": "Kumpulan Alat Pengembangan Perangkat Lunak." },
  { "en": "Apa Kepanjangan Dari SDK?", "id": "Software Development Kit." },
  { "en": "Apa Itu Integrated Development Environment (IDE)?", "id": "Lingkungan Terpadu Pengembangan Perangkat Lunak." },
  { "en": "Apa Kepanjangan Dari IDE?", "id": "Integrated Development Environment." },
  { "en": "Apa Itu Build Automation?", "id": "Otomatisasi Proses Kompilasi Kode." },
  { "en": "Apa Itu Static Code Analysis?", "id": "Analisis Kode Sumber Tanpa Eksekusi." },
  { "en": "Apa Itu Heuristic Analysis?", "id": "Analisis Berbasis Aturan Praktis." },
  { "en": "Apa Itu Regression Testing?", "id": "Pengujian Ulang Setelah Perubahan Kode." },
  { "en": "Apa Itu Manajemen Obsolescence?", "id": "Mengelola Komponen Yang Tidak Diproduksi Lagi." },
  { "en": "Apa Kepanjangan Status EOL (End-of-Life)?", "id": "End-of-Life." },
  { "en": "Apa Itu Last Time Buy (LTB)?", "id": "Kesempatan Terakhir Membeli Komponen EOL." },
  { "en": "Apa Itu Component Sourcing?", "id": "Proses Pencarian Dan Pengadaan Komponen." },
  { "en": "Apa Itu Contract Manufacturer (CM)?", "id": "Perusahaan Manufaktur Produk Untuk Pihak Lain." },
  { "en": "Apa Kepanjangan Dari Perusahaan OEM?", "id": "Original Equipment Manufacturer." },
  { "en": "Apa Kepanjangan Dari Perusahaan ODM?", "id": "Original Design Manufacturer." },
  { "en": "Apa Perbedaan OEM dan ODM?", "id": "ODM Mendesain Produk OEM Hanya Memproduksi." },
  { "en": "Apa Itu Standard Cell Dalam Desain IC?", "id": "Blok Logika Dasar Yang Telah Didesain." },
  { "en": "Apa Tujuan Clock Tree Synthesis (CTS)?", "id": "Mendistribusikan Clock Dengan Skew Minimum." },
  { "en": "Apa Kepanjangan Dari CTS?", "id": "Clock Tree Synthesis." },
  { "en": "Apa Itu Antenna Diode?", "id": "Dioda Proteksi Dari Antenna Effect." },
  { "en": "Apa Yang Diukur Noise Figure (NF)?", "id": "Degradasi Rasio Sinyal-Noise." },
  { "en": "Apa Kepanjangan Dari NF?", "id": "Noise Figure." },
  { "en": "Apa Itu Intermodulation Distortion (IMD)?", "id": "Produk Frekuensi Yang Tidak Diinginkan." },
  { "en": "Apa Kepanjangan Dari IMD?", "id": "Intermodulation Distortion." },
  { "en": "Apa Itu Spurious-Free Dynamic Range (SFDR)?", "id": "Rentang Sinyal Bebas Dari Spur." },
  { "en": "Apa Kepanjangan Dari SFDR?", "id": "Spurious-Free Dynamic Range." },
  { "en": "Apa Fungsi Vector Signal Analyzer (VSA)?", "id": "Menganalisis Sinyal Modulasi Digital Kompleks." },
  { "en": "Apa Fungsi Memory Management Unit (MMU)?", "id": "Menerjemahkan Alamat Virtual Ke Fisik." },
  { "en": "Apa Kepanjangan Dari MMU?", "id": "Memory Management Unit." },
  { "en": "Apa Fungsi Sebenarnya Watchdog Timer?", "id": "Mereset Sistem Jika Terjadi Kegagalan Software." },
  { "en": "Apa Itu Supervisor Mode?", "id": "Mode Operasi Prosesor Dengan Hak Penuh." },
  { "en": "Apa Itu User Mode?", "id": "Mode Operasi Prosesor Dengan Hak Terbatas." },
  { "en": "Apa Itu Trellis Coding?", "id": "Teknik Modulasi Dengan Koreksi Kesalahan." },
  { "en": "Apa Itu Viterbi Algorithm?", "id": "Algoritma Untuk Mendekode Kode Konvolusional." },
  { "en": "Apa Itu Turbo Codes?", "id": "Kode Koreksi Kesalahan Berkinerja Tinggi." },
  { "en": "Apa Itu Low-Density Parity-Check (LDPC) Codes?", "id": "Jenis Kode Koreksi Kesalahan Linier." },
  { "en": "Apa Kepanjangan Dari LDPC?", "id": "Low-Density Parity-Check." },
  { "en": "Apa Fungsi Hot-Swap Controller?", "id": "Memungkinkan Pemasangan Kartu Saat Sistem Nyala." },
  { "en": "Apa Fungsi OR-ing Controller?", "id": "Menggabungkan Beberapa Sumber Daya Secara Redundan." },
  { "en": "Apa Itu Active Power Factor Correction (PFC)?", "id": "Sirkuit Aktif Untuk Memperbaiki Faktor Daya." },
  { "en": "Apa Itu Synchronous Rectification?", "id": "Penyearahan Menggunakan MOSFET Untuk Efisiensi." },
  { "en": "Apa Perbedaan Laser Diode dan LED?", "id": "Laser Menghasilkan Cahaya Koheren." },
  { "en": "Apa Itu Quantum Efficiency?", "id": "Efisiensi Konversi Foton Menjadi Elektron." },
  { "en": "Apa Itu Responsivity?", "id": "Ukuran Output Fotodetektor Per Input Daya." },
  { "en": "Apa Itu Bandgap Reference?", "id": "Sirkuit Penghasil Tegangan Referensi Stabil." },
  { "en": "Apa Itu Current Mirror Tipe Wilson?", "id": "Current Mirror Dengan Impedansi Output Tinggi." },
  { "en": "Apa Itu Topologi Filter Sallen-Key?", "id": "Topologi Filter Aktif Populer." },
  { "en": "Apa Karakteristik Filter Butterworth?", "id": "Respon Frekuensi Datar Maksimal." },
  { "en": "Apa Karakteristik Filter Chebyshev?", "id": "Roll-off Tajam Dengan Ripple Di Passband." },
  { "en": "Apa Karakteristik Filter Bessel?", "id": "Respon Fasa Linier." },
  { "en": "Apa Itu Application Programming Interface (API)?", "id": "Antarmuka Untuk Program Berinteraksi." },
  { "en": "Apa Kepanjangan Dari API?", "id": "Application Programming Interface." },
  { "en": "Apa Fungsi Device Driver?", "id": "Perangkat Lunak Pengontrol Perangkat Keras." },
  { "en": "Apa Itu Scheduler Dalam RTOS?", "id": "Komponen Pengatur Penjadwalan Tugas." },
  { "en": "Apa Itu Semaphore?", "id": "Variabel Untuk Mengontrol Akses Sumber Daya." },
  { "en": "Apa Itu Mutex?", "id": "Objek Sinkronisasi Mirip Semaphore." },
  { "en": "Apa Itu Component Derating?", "id": "Mengoperasikan Komponen Di Bawah Batas Maksimal." },
  { "en": "Apa Itu Weibull Distribution?", "id": "Distribusi Statistik Untuk Analisis Keandalan." },
  { "en": "Apa Itu Single Event Latch-up (SEL)?", "id": "Latch-up Yang Dipicu Partikel Energi Tinggi." },
  { "en": "Apa Kepanjangan Dari SEL?", "id": "Single Event Latch-up." },
  { "en": "Apa Itu Single Event Upset (SEU)?", "id": "Perubahan Bit Dalam Memori Akibat Radiasi." },
  { "en": "Apa Kepanjangan Dari SEU?", "id": "Single Event Upset." },
  { "en": "Apa Itu Bus contention?", "id": "Konflik Saat Dua Perangkat Mengirim Data." },
  { "en": "Apa Itu Metastability?", "id": "Keadaan Tidak Terdefinisi Dalam Sirkuit Digital." },
  { "en": "Apa Itu Synchronizer Circuit?", "id": "Sirkuit Untuk Mengurangi Masalah Metastabilitas." },
  { "en": "Apa Itu FIFO Buffer?", "id": "Memori Yang Bekerja Secara First-In First-Out." },
  { "en": "Apa Kepanjangan Dari FIFO?", "id": "First-In, First-Out." },
  { "en": "Apa Itu LIFO Buffer?", "id": "Memori Yang Bekerja Secara Last-In First-Out." },
  { "en": "Apa Kepanjangan Dari LIFO?", "id": "Last-In, First-Out." },
  { "en": "Apa Itu Universal Asynchronous Receiver-Transmitter (UART) Frame?", "id": "Struktur Data Dalam Komunikasi UART." },
  { "en": "Apa Itu Start Bit?", "id": "Sinyal Awal Transmisi Data Serial." },
  { "en": "Apa Itu Stop Bit?", "id": "Sinyal Akhir Transmisi Data Serial." },
  { "en": "Apa Itu Parity Bit?", "id": "Bit Untuk Pengecekan Kesalahan Sederhana." },
  { "en": "Apa Itu Baud Rate Generator?", "id": "Sirkuit Pembangkit Clock Untuk Komunikasi Serial." },
  { "en": "Apa Itu Manchester Encoding?", "id": "Metode Pengkodean Data Dengan Transisi." },
  { "en": "Apa Itu Non-Return-to-Zero (NRZ) Encoding?", "id": "Metode Pengkodean Data Level Tegangan." },
  { "en": "Apa Kepanjangan Dari NRZ?", "id": "Non-Return-to-Zero." },
  { "en": "Apa Itu Electromagnetic Forming?", "id": "Proses Pembentukan Logam Menggunakan Medan Magnet." },
  { "en": "Apa Itu Thermal runaway?", "id": "Peningkatan Suhu Yang Tidak Terkendali." },
  { "en": "Apa Itu Safe Operating Area (SOA)?", "id": "Area Operasi Aman Sebuah Komponen." },
  { "en": "Apa Kepanjangan Dari SOA?", "id": "Safe Operating Area." },
  { "en": "Apa Itu Spice Model?", "id": "Model Matematis Komponen Untuk Simulasi." },
  { "en": "Apa Itu Behavioral Model?", "id": "Model Fungsional Tanpa Detail Sirkuit." },
  { "en": "Apa Itu Transistor-Level Model?", "id": "Model Sirkuit Berbasis Transistor." },
  { "en": "Apa Itu Gate-Level Model?", "id": "Model Sirkuit Berbasis Gerbang Logika." },
  { "en": "Apa Itu Register-Transfer Level (RTL) Design?", "id": "Desain Berbasis Aliran Data Antar Register." },
  { "en": "Apa Kepanjangan Dari RTL?", "id": "Register-Transfer Level." },
  { "en": "Apa Itu High-Level Synthesis (HLS)?", "id": "Sintesis RTL Dari Bahasa Level Tinggi." },
  { "en": "Apa Kepanjangan Dari HLS?", "id": "High-Level Synthesis." },
  { "en": "Apa Itu Equivalence Checking?", "id": "Memverifikasi Dua Representasi Desain Sama." },
  { "en": "Apa Itu Formal Verification?", "id": "Verifikasi Desain Menggunakan Metode Matematis." },
  { "en": "Apa Itu Code Coverage?", "id": "Metrik Persentase Kode Yang Diuji." },
  { "en": "Apa Itu Assertion-Based Verification?", "id": "Verifikasi Berbasis Properti Atau Aturan." },
  { "en": "Apa Itu Universal Verification Methodology (UVM)?", "id": "Metodologi Standar Untuk Verifikasi IC." },
  { "en": "Apa Kepanjangan Dari UVM?", "id": "Universal Verification Methodology." },
  { "en": "Apa Itu Testbench?", "id": "Lingkungan Untuk Memverifikasi Desain." },
  { "en": "Apa Itu Design Under Test (DUT)?", "id": "Desain Yang Sedang Dalam Proses Pengujian." },
  { "en": "Apa Kepanjangan Dari DUT?", "id": "Design Under Test." },
  { "en": "Apa Itu Constrained-Random Verification?", "id": "Verifikasi Menggunakan Stimulus Acak Terkendali." },
  { "en": "Apa Itu Functional Coverage?", "id": "Metrik Seberapa Baik Fitur Fungsional Diuji." },
  { "en": "Apa Itu Power-On Reset (POR) Circuit?", "id": "Sirkuit Pembangkit Sinyal Reset Saat Dinyalakan." },
  { "en": "Apa Kepanjangan Dari POR?", "id": "Power-On Reset." },
  { "en": "Apa Itu Kemasan IC QFP (Quad Flat Package)?", "id": "Paket IC Dengan Kaki Di Empat Sisi." },
  { "en": "Apa Kepanjangan Dari QFP?", "id": "Quad Flat Package." },
  { "en": "Apa Itu Kemasan IC QFN (Quad Flat No-leads)?", "id": "Paket IC Tanpa Kaki." },
  { "en": "Apa Kepanjangan Dari QFN?", "id": "Quad Flat No-leads." },
  { "en": "Apa Itu Proses Die Attach?", "id": "Proses Melekatkan Die IC Ke Kemasan." },
  { "en": "Apa Itu Lead Frame?", "id": "Kerangka Logam Internal Kemasan IC." },
  { "en": "Apa Itu Glitch Dalam Sinyal Digital?", "id": "Pulsa Transien Singkat Yang Tidak Diinginkan." },
  { "en": "Apa Itu Setup Time?", "id": "Waktu Data Harus Stabil Sebelum Clock." },
  { "en": "Apa Itu Hold Time?", "id": "Waktu Data Harus Stabil Setelah Clock." },
  { "en": "Apa Perbedaan State Machine Mealy Dan Moore?", "id": "Output Mesin Mealy Tergantung Input." },
  { "en": "Bagaimana Proses Arbitrasi Bus CAN?", "id": "ID Pesan Terendah Memenangkan Bus." },
  { "en": "Apa Prinsip Kerja CSMA/CD Di Ethernet?", "id": "Mendeteksi Tabrakan Dan Mengirim Ulang." },
  { "en": "Apa Kepanjangan Dari CSMA/CD?", "id": "Carrier Sense Multiple Access/Collision Detection." },
  { "en": "Apa Itu Standar MIPI (Mobile Industry Processor Interface)?", "id": "Antarmuka Serial Kecepatan Tinggi." },
  { "en": "Apa Kepanjangan Dari MIPI?", "id": "Mobile Industry Processor Interface." },
  { "en": "Apa Itu Quantum Well Laser?", "id": "Laser Semikonduktor Dengan Lapisan Aktif Tipis." },
  { "en": "Apa Itu Single-Photon Detector?", "id": "Detektor Yang Mampu Mendeteksi Foton Tunggal." },
  { "en": "Apa Fungsi Wilkinson Power Divider?", "id": "Membagi Sinyal RF Dengan Isolasi." },
  { "en": "Apa Itu Lange Coupler?", "id": "Directional Coupler Empat-Port Gelombang Mikro." },
  { "en": "Apa Itu Rat-Race Coupler?", "id": "Hybrid Coupler Dengan Bentuk Cincin." },
  { "en": "Apa Hubungan Return Loss Dan VSWR?", "id": "Keduanya Mengukur Pantulan Sinyal." },
  { "en": "Apa Kepanjangan Dari VSWR?", "id": "Voltage Standing Wave Ratio." },
  { "en": "Bagaimana Prinsip Kerja MEMS (Micro-Electro-Mechanical Systems) Microphone?", "id": "Menggunakan Diafragma Miniatur Pada Chip." },
  { "en": "Apa Itu Digital Micromirror Device (DMD)?", "id": "Array Cermin Mikro Untuk Proyektor." },
  { "en": "Apa Kepanjangan Dari DMD?", "id": "Digital Micromirror Device." },
  { "en": "Apa Itu Arsitektur DAC (Digital-to-Analog Converter) R-2R Ladder?", "id": "Menggunakan Jaringan Resistor R Dan 2R." },
  { "en": "Apa Itu Sigma-Delta Modulator?", "id": "Sirkuit Dasar Untuk ADC Dan DAC Modern." },
  { "en": "Apa Itu Stack Overflow?", "id": "Kondisi Memori Stack Habis." },
  { "en": "Apa Itu Heap Fragmentation?", "id": "Memori Heap Terfragmentasi Menjadi Blok Kecil." },
  { "en": "Apa Kepanjangan Dari DFU?", "id": "Device Firmware Update." },
  { "en": "Apa Aplikasi Utama Ferrite Bead?", "id": "Menekan Noise Frekuensi Tinggi." },
  { "en": "Apa Fungsi Common-Mode Choke?", "id": "Menekan Noise Common-Mode." },
  { "en": "Apa Fungsi Differential-Mode Choke?", "id": "Menekan Noise Differential-Mode." },
  { "en": "Apa Itu Shielding Effectiveness?", "id": "Ukuran Kemampuan Perisai Meredam Medan EM." },
  { "en": "Apa Maksud Istilah Tape-Out Dalam Desain IC?", "id": "Mengirimkan Desain Final Ke Pabrik." },
  { "en": "Apa Itu First Silicon?", "id": "Prototipe Chip Fisik Pertama." },
  { "en": "Apa Itu Silicon Revision?", "id": "Versi Perbaikan Dari Desain Chip." },
  { "en": "Apa Itu Proses Wafer Sort?", "id": "Pengujian Die Pada Wafer." },
  { "en": "Apa Itu Final Test?", "id": "Pengujian Chip Setelah Dikemas." },
  { "en": "Apa Itu Proses Characterization?", "id": "Mengukur Performa Komponen Di Berbagai Kondisi." },
  { "en": "Apa Itu Body Bias?", "id": "Tegangan Yang Diterapkan Ke Substrat Transistor." },
  { "en": "Apa Itu Hot Carrier Injection (HCI)?", "id": "Mekanisme Kegagalan Pada Transistor." },
  { "en": "Apa Kepanjangan Dari HCI?", "id": "Hot Carrier Injection." },
  { "en": "Apa Itu Electromigration?", "id": "Perpindahan Atom Logam Akibat Arus Listrik." },
  { "en": "Apa Itu Time-Dependent Dielectric Breakdown (TDDB)?", "id": "Mekanisme Kegagalan Isolator Seiring Waktu." },
  { "en": "Apa Kepanjangan Dari TDDB?", "id": "Time-Dependent Dielectric Breakdown." },
  { "en": "Apa Itu Process Variation?", "id": "Variasi Karakteristik Komponen Dalam Produksi." },
  { "en": "Apa Itu Binning?", "id": "Mengelompokkan Komponen Berdasarkan Kinerja." },
  { "en": "Apa Itu Corner Lot?", "id": "Lot Produksi Di Batas Ekstrem Proses." },
  { "en": "Apa Itu Process Control Monitor (PCM)?", "id": "Struktur Uji Untuk Memantau Proses Fabrikasi." },
  { "en": "Apa Kepanjangan Dari PCM?", "id": "Process Control Monitor." },
  { "en": "Apa Itu Optical Proximity Correction (OPC)?", "id": "Teknik Untuk Meningkatkan Akurasi Fotolitografi." },
  { "en": "Apa Kepanjangan Dari OPC?", "id": "Optical Proximity Correction." },
  { "en": "Apa Itu Chemical-Mechanical Planarization (CMP)?", "id": "Proses Untuk Meratakan Permukaan Wafer." },
  { "en": "Apa Kepanjangan Dari CMP?", "id": "Chemical-Mechanical Planarization." },
  { "en": "Apa Itu FinFET?", "id": "Arsitektur Transistor Non-Planar." },
  { "en": "Apa Itu Gate-All-Around (GAA) FET?", "id": "Arsitektur Transistor Lanjutan." },
  { "en": "Apa Kepanjangan Dari GAA?", "id": "Gate-All-Around." },
  { "en": "Apa Itu Through-Silicon Via (TSV)?", "id": "Koneksi Vertikal Melalui Wafer Silikon." },
  { "en": "Apa Kepanjangan Dari TSV?", "id": "Through-Silicon Via." },
  { "en": "Apa Itu 2.5D IC Packaging?", "id": "Menumpuk Die Secara Berdampingan Di Interposer." },
  { "en": "Apa Itu 3D IC Packaging?", "id": "Menumpuk Die Secara Vertikal." },
  { "en": "Apa Itu Interposer?", "id": "Lapisan Penghubung Antara Die Dan Substrat." },
  { "en": "Apa Itu Automatic Test Pattern Generation (ATPG)?", "id": "Generasi Pola Uji Otomatis." },
  { "en": "Apa Kepanjangan Dari ATPG?", "id": "Automatic Test Pattern Generation." },
  { "en": "Apa Itu Scan Chain?", "id": "Teknik Untuk Meningkatkan Kemampuan Uji." },
  { "en": "Apa Itu Built-In Self-Test (BIST)?", "id": "Sirkuit Uji Yang Tertanam Dalam Chip." },
  { "en": "Apa Kepanjangan Dari BIST?", "id": "Built-In Self-Test." },
  { "en": "Apa Itu Memory BIST?", "id": "BIST Khusus Untuk Menguji Memori." },
  { "en": "Apa Itu Logic BIST?", "id": "BIST Khusus Untuk Menguji Sirkuit Logika." },
  { "en": "Apa Itu Intellectual Property (IP) Vendor?", "id": "Perusahaan Yang Menjual Blok Desain IP." },
  { "en": "Apa Itu Royalty (dalam IP)?", "id": "Biaya Berbasis Penggunaan IP." },
  { "en": "Apa Itu License Fee (dalam IP)?", "id": "Biaya Awal Untuk Menggunakan IP." },
  { "en": "Apa Itu GDSII?", "id": "Format File Standar Untuk Layout IC." },
  { "en": "Apa Itu OASIS?", "id": "Format File Layout IC Pengganti GDSII." },
  { "en": "Apa Itu Standard Parasitic EXchange Format (SPEF)?", "id": "Format File Untuk Informasi Parasitik." },
  { "en": "Apa Kepanjangan Dari SPEF?", "id": "Standard Parasitic EXchange Format." },
  { "en": "Apa Itu Liberty Timing Format (.lib)?", "id": "Format File Untuk Informasi Waktu Sel." },
  { "en": "Apa Itu Synopsys Design Constraints (SDC)?", "id": "Format Untuk Mendefinisikan Batasan Desain." },
  { "en": "Apa Kepanjangan Dari SDC?", "id": "Synopsys Design Constraints." },
  { "en": "Apa Itu Noise Margin?", "id": "Toleransi Level Tegangan Terhadap Noise." },
  { "en": "Apa Itu Race Condition?", "id": "Kondisi Output Tergantung Urutan Sinyal." },
  { "en": "Apa Itu Critical Path?", "id": "Jalur Dengan Penundaan Terlama Dalam Sirkuit." },
  { "en": "Apa Itu False Path?", "id": "Jalur Logika Yang Tidak Pernah Diaktifkan." },
  { "en": "Apa Itu Multi-cycle Path?", "id": "Jalur Logika Yang Membutuhkan Beberapa Siklus." },
  { "en": "Apa Itu Latch-based Design?", "id": "Desain Digital Menggunakan Latch." },
  { "en": "Apa Itu Clock Gating Integrated Cell?", "id": "Sel Standar Untuk Implementasi Clock Gating." },
  { "en": "Apa Itu Power-Aware Simulation?", "id": "Simulasi Yang Memperhitungkan Konsumsi Daya." },
  { "en": "Apa Itu Unified Power Format (UPF)?", "id": "Standar Untuk Spesifikasi Desain Daya Rendah." },
  { "en": "Apa Kepanjangan Dari UPF?", "id": "Unified Power Format." },
  { "en": "Apa Itu Common Power Format (CPF)?", "id": "Standar Lain Spesifikasi Desain Daya Rendah." },
  { "en": "Apa Kepanjangan Dari CPF?", "id": "Common Power Format." },
  { "en": "Apa Itu IR Drop?", "id": "Penurunan Tegangan Akibat Resistansi Jaringan Daya." },
  { "en": "Apa Itu Model Kesalahan Stuck-at-Fault?", "id": "Asumsi Node Sirkuit Terjebak Di 0/1." },
  { "en": "Apa Itu Model Kesalahan Transition Fault?", "id": "Asumsi Node Gagal Bertransisi." },
  { "en": "Apa Itu Observability Dalam Konteks Uji?", "id": "Kemudahan Mengamati Status Node Internal." },
  { "en": "Apa Itu Controllability Dalam Konteks Uji?", "id": "Kemudahan Mengatur Status Node Internal." },
  { "en": "Apa Prinsip Kerja Boundary Scan?", "id": "Menguji Koneksi Antar IC Di PCB." },
  { "en": "Apa Itu Automatic Test Equipment (ATE)?", "id": "Sistem Otomatis Untuk Pengujian Elektronik." },
  { "en": "Apa Kepanjangan Dari ATE?", "id": "Automatic Test Equipment." },
  { "en": "Apa Itu Charge Injection?", "id": "Efek Error Pada Saklar Analog." },
  { "en": "Apa Itu Clock Feedthrough?", "id": "Kebocoran Sinyal Clock Ke Jalur Sinyal." },
  { "en": "Apa Itu Correlated Double Sampling (CDS)?", "id": "Teknik Pengurangan Noise Pada Sensor Gambar." },
  { "en": "Apa Kepanjangan Dari CDS?", "id": "Correlated Double Sampling." },
  { "en": "Apa Itu Chopper Stabilization?", "id": "Teknik Mengurangi Offset Dan Drift Amplifier." },
  { "en": "Apa Kepanjangan Sirkuit SERDES (Serializer/Deserializer)?", "id": "Serializer/Deserializer." },
  { "en": "Apa Fungsi Utama SERDES?", "id": "Mengubah Data Paralel Menjadi Serial." },
  { "en": "Apa Itu Pengkodean 8b/10b?", "id": "Metode Pengkodean Jalur Transmisi Data." },
  { "en": "Apa Itu Scrambling Dalam Komunikasi?", "id": "Mengacak Urutan Data Untuk Sinkronisasi." },
  { "en": "Apa Itu Spread-Spectrum Clocking (SSC)?", "id": "Teknik Mengurangi Puncak Emisi EMI." },
  { "en": "Apa Kepanjangan Dari SSC?", "id": "Spread-Spectrum Clocking." },
  { "en": "Apa Itu Bus Fabric?", "id": "Infrastruktur Interkoneksi Kompleks Dalam SoC." },
  { "en": "Apa Itu Network on a Chip (NoC)?", "id": "Arsitektur Komunikasi Antar Blok IP." },
  { "en": "Apa Kepanjangan Dari NoC?", "id": "Network on a Chip." },
  { "en": "Apa Kepanjangan Bus AMBA (Advanced Microcontroller Bus Architecture)?", "id": "Advanced Microcontroller Bus Architecture." },
  { "en": "Apa Fungsi Bus AHB Dalam AMBA?", "id": "Bus Utama Berkinerja Tinggi." },
  { "en": "Apa Fungsi Bus APB Dalam AMBA?", "id": "Bus Periferal Berkecepatan Rendah." },
  { "en": "Apa Fungsi Bus AXI Dalam AMBA?", "id": "Bus Antarmuka Generasi Berikutnya." },
  { "en": "Apa Itu Sinyal Kuadratur (I/Q)?", "id": "Dua Sinyal Dengan Perbedaan Fasa 90 Derajat." },
  { "en": "Apa Itu Image Frequency?", "id": "Frekuensi Yang Tidak Diinginkan Dalam Mixer." },
  { "en": "Apa Itu Local Oscillator (LO) Leakage?", "id": "Kebocoran Sinyal Osilator Lokal." },
  { "en": "Apa Itu Error Vector Magnitude (EVM)?", "id": "Ukuran Akurasi Sinyal Modulasi Digital." },
  { "en": "Apa Kepanjangan Dari EVM?", "id": "Error Vector Magnitude." },
  { "en": "Apa Itu Goertzel Algorithm?", "id": "Algoritma Untuk Deteksi Nada Frekuensi Tunggal." },
  { "en": "Apa Fungsi Hilbert Transform?", "id": "Membuat Komponen Kuadratur Dari Sinyal." },
  { "en": "Apa Itu Wavelet Transform?", "id": "Analisis Sinyal Dalam Domain Waktu-Frekuensi." },
  { "en": "Apa Itu Adaptive Filter?", "id": "Filter Yang Koefisiennya Dapat Beradaptasi." },
  { "en": "Apa Itu Negative-Bias Temperature Instability (NBTI)?", "id": "Mekanisme Degradasi Transistor PMOS." },
  { "en": "Apa Kepanjangan Dari NBTI?", "id": "Negative-Bias Temperature Instability." },
  { "en": "Apa Itu Positive-Bias Temperature Instability (PBTI)?", "id": "Mekanisme Degradasi Transistor NMOS." },
  { "en": "Apa Itu Stress Migration?", "id": "Kegagalan Akibat Stres Mekanis Termal." },
  { "en": "Apa Itu Bragg Grating?", "id": "Struktur Periodik Reflektor Cahaya." },
  { "en": "Apa Itu Fabry-Pérot Interferometer?", "id": "Resonator Optik Dengan Dua Cermin Paralel." },
  { "en": "Apa Itu Wavelength Division Multiplexing (WDM)?", "id": "Mengirim Beberapa Sinyal Cahaya Di Satu Serat." },
  { "en": "Apa Kepanjangan Dari WDM?", "id": "Wavelength Division Multiplexing." },
  { "en": "Apa Itu Metode Coulomb Counting?", "id": "Metode Estimasi Sisa Daya Baterai." },
  { "en": "Apa Itu Adaptive Voltage Scaling (AVS)?", "id": "Teknik Penyesuaian Tegangan Otomatis." },
  { "en": "Apa Kepanjangan Dari AVS?", "id": "Adaptive Voltage Scaling." },
  { "en": "Apa Itu Dynamic Voltage and Frequency Scaling (DVFS)?", "id": "Penyesuaian Tegangan Frekuensi Dinamis." },
  { "en": "Apa Kepanjangan Dari DVFS?", "id": "Dynamic Voltage and Frequency Scaling." },
  { "en": "Apa Itu Bahasa SystemVerilog?", "id": "Ekstensi Dari Verilog Untuk Desain Verifikasi." },
  { "en": "Apa Itu Property Specification Language (PSL)?", "id": "Bahasa Untuk Mendefinisikan Properti Desain." },
  { "en": "Apa Kepanjangan Dari PSL?", "id": "Property Specification Language." },
  { "en": "Apa Nama Alat Simulasi Spectre?", "id": "Simulator Sirkuit Dari Cadence." },
  { "en": "Apa Nama Alat Layout Virtuoso?", "id": "Editor Layout IC Dari Cadence." },
  { "en": "Apa Nama Alat Sintesis Design Compiler?", "id": "Alat Sintesis Logika Dari Synopsys." },
  { "en": "Apa Nama Alat Analisis Waktu PrimeTime?", "id": "Alat Analisis Waktu Statis Synopsys." },
  { "en": "Apa Itu Phase Interpolator?", "id": "Sirkuit Untuk Penyesuaian Fasa Halus." },
  { "en": "Apa Itu Clock and Data Recovery (CDR)?", "id": "Sirkuit Pemulih Clock Dari Data." },
  { "en": "Apa Kepanjangan Dari CDR?", "id": "Clock and Data Recovery." },
  { "en": "Apa Itu Frequency Synthesizer?", "id": "Sirkuit Pembangkit Frekuensi Yang Tepat." },
  { "en": "Apa Itu Direct Digital Synthesizer (DDS)?", "id": "Metode Sintesis Frekuensi Secara Digital." },
  { "en": "Apa Kepanjangan Dari DDS?", "id": "Direct Digital Synthesizer." },
  { "en": "Apa Itu Injection Locking?", "id": "Sinkronisasi Osilator Dengan Sinyal Eksternal." },
  { "en": "Apa Itu Temperature-Compensated Crystal Oscillator (TCXO)?", "id": "Osilator Kristal Dengan Kompensasi Suhu." },
  { "en": "Apa Kepanjangan Dari TCXO?", "id": "Temperature-Compensated Crystal Oscillator." },
  { "en": "Apa Itu Oven-Controlled Crystal Oscillator (OCXO)?", "id": "Osilator Dengan Suhu Kristal Dijaga Konstan." },
  { "en": "Apa Kepanjangan Dari OCXO?", "id": "Oven-Controlled Crystal Oscillator." },
  { "en": "Apa Itu Allan Variance?", "id": "Ukuran Stabilitas Frekuensi Osilator." },
  { "en": "Apa Itu Doppler Effect?", "id": "Perubahan Frekuensi Akibat Gerakan Relatif." },
  { "en": "Apa Itu Time of Arrival (ToA)?", "id": "Metode Penentuan Posisi Berbasis Waktu." },
  { "en": "Apa Kepanjangan Dari ToA?", "id": "Time of Arrival." },
  { "en": "Apa Itu Time Difference of Arrival (TDoA)?", "id": "Metode Penentuan Posisi Berbasis Perbedaan Waktu." },
  { "en": "Apa Kepanjangan Dari TDoA?", "id": "Time Difference of Arrival." },
  { "en": "Apa Itu Angle of Arrival (AoA)?", "id": "Metode Penentuan Posisi Berbasis Sudut." },
  { "en": "Apa Kepanjangan Dari AoA?", "id": "Angle of Arrival." },
  { "en": "Apa Itu Received Signal Strength Indicator (RSSI)?", "id": "Ukuran Kekuatan Sinyal Yang Diterima." },
  { "en": "Apa Kepanjangan Dari RSSI?", "id": "Received Signal Strength Indicator." },
  { "en": "Apa Itu Link Budget?", "id": "Perhitungan Semua Penguatan Kerugian Sistem Komunikasi." },
  { "en": "Apa Itu Fade Margin?", "id": "Cadangan Daya Untuk Mengatasi Pelemahan Sinyal." },
  { "en": "Apa Itu Free-Space Path Loss?", "id": "Pelemahan Sinyal Di Ruang Bebas." },
  { "en": "Apa Itu Antenna Polarization Mismatch?", "id": "Ketidakcocokan Polarisasi Antena Pemancar Penerima." },
  { "en": "Apa Itu Effective Isotropic Radiated Power (EIRP)?", "id": "Daya Pancar Efektif Sebuah Antena." },
  { "en": "Apa Kepanjangan Dari EIRP?", "id": "Effective Isotropic Radiated Power." },
  { "en": "Apa Itu Noise Floor?", "id": "Tingkat Dasar Noise Dalam Sistem." },
  { "en": "Apa Itu Signal-to-Interference-plus-Noise Ratio (SINR)?", "id": "Rasio Sinyal Terhadap Gangguan Dan Noise." },
  { "en": "Apa Kepanjangan Dari SINR?", "id": "Signal-to-Interference-plus-Noise Ratio." },
  { "en": "Apa Itu Cognitive Radio?", "id": "Radio Cerdas Yang Dapat Beradaptasi." },
  { "en": "Apa Itu Beamforming?", "id": "Teknik Memfokuskan Sinyal Nirkabel." },
  { "en": "Apa Itu Multiple-Input Multiple-Output (MIMO)?", "id": "Menggunakan Banyak Antena Untuk Tingkatkan Kinerja." },
  { "en": "Apa Kepanjangan Dari MIMO?", "id": "Multiple-Input Multiple-Output." },
  { "en": "Apa Itu Space-Time Coding?", "id": "Metode Pengkodean Untuk Sistem MIMO." },
  { "en": "Apa Itu Orthogonality?", "id": "Sifat Saling Independen Antar Sinyal." },
  { "en": "Apa Itu Zero-Forcing Equalizer?", "id": "Filter Untuk Menghilangkan Interferensi Antar Simbol." },
  { "en": "Apa Itu Matched Filter?", "id": "Filter Optimal Untuk Deteksi Sinyal." },
  { "en": "Apa Kontribusi Utama Claude Shannon?", "id": "Peletak Dasar Teori Informasi Modern." },
  { "en": "Apa Implikasi Teorema Nyquist?", "id": "Menentukan Laju Sampling Minimum." },
  { "en": "Apa Konsep Dasar Mesin Turing?", "id": "Model Komputasi Teoritis Universal." },
  { "en": "Apa Fungsi Guard Ring Dalam Layout IC?", "id": "Mengisolasi Noise Dan Mencegah Latch-up." },
  { "en": "Apa Itu Dummy Fill?", "id": "Penambahan Logam Untuk Kepadatan Seragam." },
  { "en": "Apa Itu Teknik Common-Centroid Layout?", "id": "Teknik Layout Untuk Kecocokan Presisi." },
  { "en": "Apa Itu Teknik Interdigitated Layout?", "id": "Layout Jari-Jari Berselang-Seling." },
  { "en": "Apa Arsitektur Pipeline ADC (Analog-to-Digital Converter)?", "id": "ADC Bertahap Untuk Kecepatan Tinggi." },
  { "en": "Apa Prinsip Kerja Dual-Slope ADC?", "id": "Mengintegrasikan Sinyal Untuk Akurasi Tinggi." },
  { "en": "Apa Itu Pipelining Hazard?", "id": "Kondisi Yang Mengganggu Aliran Pipeline." },
  { "en": "Apa Itu Branch Prediction?", "id": "Teknik Memprediksi Hasil Percabangan Kode." },
  { "en": "Apa Itu Komponen Pasif Memristor?", "id": "Resistor Dengan Memori Arus Listrik." },
  { "en": "Apa Itu Neuromorphic Computing?", "id": "Komputasi Yang Meniru Otak Manusia." },
  { "en": "Apa Itu Silicon Photonics?", "id": "Teknologi Optik Berbasis Silikon." },
  { "en": "Apa Itu Target Impedance?", "id": "Impedansi Maksimum Jaringan Distribusi Daya." },
  { "en": "Apa Fungsi Bulk Decoupling Capacitor?", "id": "Menyediakan Cadangan Arus Frekuensi Rendah." },
  { "en": "Bagaimana Cara Kerja Time-Domain Reflectometry (TDR)?", "id": "Menganalisis Pantulan Sinyal Di Kabel." },
  { "en": "Apa Kepanjangan Alat Uji BERT (Bit Error Rate Tester)?", "id": "Bit Error Rate Tester." },
  { "en": "Apa Fungsi Semiconductor Parameter Analyzer?", "id": "Menganalisis Karakteristik Perangkat Semikonduktor." },
  { "en": "Apa Contoh Semikonduktor III-V?", "id": "Gallium Arsenide (GaAs)." },
  { "en": "Apa Contoh Semikonduktor II-VI?", "id": "Cadmium Sulfide (CdS)." },
  { "en": "Apa Perbedaan Semikonduktor Intrinsik Dan Ekstrinsik?", "id": "Ekstrinsik Memiliki Atom Dopan." },
  { "en": "Apa Kepanjangan Dari CAN FD?", "id": "CAN with Flexible Data-Rate." },
  { "en": "Apa Keunggulan CAN FD?", "id": "Kecepatan Data Lebih Tinggi Dari CAN." },
  { "en": "Apa Itu Time-Sensitive Networking (TSN)?", "id": "Standar Ethernet Untuk Komunikasi Real-Time." },
  { "en": "Apa Kepanjangan Dari TSN?", "id": "Time-Sensitive Networking." },
  { "en": "Apa Fungsi Perangkat Lunak Linker?", "id": "Menggabungkan File Objek Menjadi Executable." },
  { "en": "Apa Fungsi Perangkat Lunak Loader?", "id": "Memuat Program Ke Memori." },
  { "en": "Apa Fungsi Dari Makefile?", "id": "Mengotomatiskan Proses Kompilasi Program." },
  { "en": "Apa Itu Cross-Compiler?", "id": "Compiler Untuk Platform Arsitektur Berbeda." },
  { "en": "Apa Kepanjangan Perangkat Debug ICE (In-Circuit Emulator)?", "id": "In-Circuit Emulator." },
  { "en": "Apa Kepanjangan Level Keselamatan ASIL?", "id": "Automotive Safety Integrity Level." },
  { "en": "Apa Standar DO-178C?", "id": "Standar Keamanan Perangkat Lunak Avionik." },
  { "en": "Apa Standar DO-254?", "id": "Standar Keamanan Perangkat Keras Avionik." },
  { "en": "Apa Itu Boolean Satisfiability Problem (SAT)?", "id": "Masalah Penentuan Kepuasan Formula Boolean." },
  { "en": "Apa Itu Binary Decision Diagram (BDD)?", "id": "Representasi Grafis Fungsi Boolean." },
  { "en": "Apa Kepanjangan Dari BDD?", "id": "Binary Decision Diagram." },
  { "en": "Apa Itu Don't-Care Condition?", "id": "Kondisi Input Yang Outputnya Tidak Penting." },
  { "en": "Apa Itu Static Hazard?", "id": "Potensi Glitch Akibat Penundaan Sirkuit." },
  { "en": "Apa Itu Dynamic Hazard?", "id": "Potensi Glitch Saat Output Seharusnya Berubah." },
  { "en": "Apa Itu Arithmetic Shift?", "id": "Operasi Geser Bit Yang Mempertahankan Tanda." },
  { "en": "Apa Itu Logical Shift?", "id": "Operasi Geser Bit Yang Mengisi Dengan Nol." },
  { "en": "Apa Itu Circular Shift?", "id": "Operasi Geser Bit Yang Berputar." },
  { "en": "Apa Itu Micro-operation?", "id": "Operasi Dasar Yang Dilakukan CPU." },
  { "en": "Apa Itu Instruction Fetch?", "id": "Tahap Pengambilan Instruksi Dari Memori." },
  { "en": "Apa Itu Instruction Decode?", "id": "Tahap Penerjemahan Instruksi." },
  { "en": "Apa Itu Execution Stage?", "id": "Tahap Eksekusi Instruksi Oleh ALU." },
  { "en": "Apa Itu Write-Back Stage?", "id": "Tahap Penulisan Hasil Kembali Ke Register." },
  { "en": "Apa Itu Superscalar Processor?", "id": "Prosesor Yang Dapat Mengeksekusi >1 Instruksi." },
  { "en": "Apa Itu Very Long Instruction Word (VLIW)?", "id": "Arsitektur Prosesor Dengan Instruksi Sangat Panjang." },
  { "en": "Apa Kepanjangan Dari VLIW?", "id": "Very Long Instruction Word." },
  { "en": "Apa Itu Out-of-Order Execution?", "id": "Eksekusi Instruksi Tidak Sesuai Urutan Program." },
  { "en": "Apa Itu Speculative Execution?", "id": "Eksekusi Instruksi Sebelum Tahu Diperlukan." },
  { "en": "Apa Itu Cache Hit?", "id": "Data Yang Dicari Ditemukan Di Cache." },
  { "en": "Apa Itu Cache Miss?", "id": "Data Yang Dicari Tidak Ditemukan Di Cache." },
  { "en": "Apa Itu Cache Eviction Policy?", "id": "Aturan Untuk Membuang Data Dari Cache." },
  { "en": "Apa Itu Least Recently Used (LRU)?", "id": "Salah Satu Kebijakan Eviction Cache." },
  { "en": "Apa Kepanjangan Dari LRU?", "id": "Least Recently Used." },
  { "en": "Apa Itu Translation Lookaside Buffer (TLB)?", "id": "Cache Untuk Terjemahan Alamat Memori." },
  { "en": "Apa Kepanjangan Dari TLB?", "id": "Translation Lookaside Buffer." },
  { "en": "Apa Itu Direct Memory Access (DMA) Controller?", "id": "Mengatur Transfer Data Langsung Ke Memori." },
  { "en": "Apa Itu Interrupt Latency?", "id": "Waktu Tunda Respon Terhadap Interupsi." },
  { "en": "Apa Itu Interrupt Service Routine (ISR)?", "id": "Kode Yang Dijalankan Saat Terjadi Interupsi." },
  { "en": "Apa Kepanjangan Dari ISR?", "id": "Interrupt Service Routine." },
  { "en": "Apa Itu Non-Maskable Interrupt (NMI)?", "id": "Interupsi Prioritas Tinggi Yang Tidak Dapat Diabaikan." },
  { "en": "Apa Kepanjangan Dari NMI?", "id": "Non-Maskable Interrupt." },
  { "en": "Apa Itu Vectorized Interrupt?", "id": "Interupsi Dengan Alamat ISR Spesifik." },
  { "en": "Apa Itu Polling?", "id": "Memeriksa Status Perangkat Secara Periodik." },
  { "en": "Apa Itu Preemption?", "id": "Penghentian Tugas Untuk Menjalankan Tugas Lain." },
  { "en": "Apa Itu Context Switching?", "id": "Proses Menyimpan Mengembalikan Konteks Tugas." },
  { "en": "Apa Itu Thread?", "id": "Unit Eksekusi Terkecil Dalam Proses." },
  { "en": "Apa Itu Race Condition (Software)?", "id": "Bug Akibat Urutan Eksekusi Thread." },
  { "en": "Apa Itu Deadlock?", "id": "Kondisi Dua Thread Saling Menunggu." },
  { "en": "Apa Itu Starvation?", "id": "Kondisi Thread Tidak Mendapat Waktu CPU." },
  { "en": "Apa Itu Priority Inversion?", "id": "Masalah Penjadwalan Tugas Berbasis Prioritas." },
  { "en": "Apa Itu Critical Section?", "id": "Bagian Kode Yang Hanya Boleh Diakses." },
  { "en": "Apa Itu Spinlock?", "id": "Mekanisme Kunci Dengan Penungguan Aktif." },
  { "en": "Apa Itu Endianness Conversion?", "id": "Proses Mengubah Urutan Byte Data." },
  { "en": "Apa Itu Board Support Package (BSP)?", "id": "Perangkat Lunak Pendukung Papan Perangkat Keras." },
  { "en": "Apa Kepanjangan Dari BSP?", "id": "Board Support Package." },
  { "en": "Apa Itu Cross-Platform Development?", "id": "Pengembangan Untuk Berbagai Platform." },
  { "en": "Apa Itu Toolchain?", "id": "Kumpulan Alat Pemrograman." },
  { "en": "Apa Itu Static Library?", "id": "Pustaka Kode Yang Digabung Saat Kompilasi." },
  { "en": "Apa Itu Dynamic Library (Shared Library)?", "id": "Pustaka Kode Yang Dimuat Saat Runtime." },
  { "en": "Apa Itu Profiling (Software)?", "id": "Analisis Kinerja Perangkat Lunak." },
  { "en": "Apa Itu Code Obfuscation?", "id": "Membuat Kode Sulit Dibaca." },
  { "en": "Apa Itu Reverse Engineering?", "id": "Menganalisis Produk Untuk Memahami Desainnya." },
  { "en": "Apa Itu Intellectual Property (IP) Protection?", "id": "Perlindungan Hak Kekayaan Intelektual." },
  { "en": "Apa Itu Johnson-Nyquist Noise?", "id": "Noise Termal Akibat Agitasi Elektron." },
  { "en": "Apa Itu Shot Noise?", "id": "Noise Akibat Sifat Diskrit Muatan." },
  { "en": "Apa Itu Flicker Noise (1/f)?", "id": "Noise Frekuensi Rendah Di Perangkat Semikonduktor." },
  { "en": "Apa Itu Shannon Limit?", "id": "Batas Atas Teoritis Laju Data." },
  { "en": "Apa Itu Verifikasi Clock Domain Crossing (CDC)?", "id": "Memverifikasi Transfer Data Antar Domain Clock." },
  { "en": "Apa Itu Proses Linting (Linting)?", "id": "Analisis Statis Kode Untuk Potensi Bug." },
  { "en": "Apa Itu Assertion Dalam Verifikasi?", "id": "Properti Atau Aturan Yang Harus Dipenuhi." },
  { "en": "Apa Itu Logic Equivalence Checking (LEC)?", "id": "Memastikan Dua Representasi Logika Ekuivalen." },
  { "en": "Apa Kepanjangan Dari LEC?", "id": "Logic Equivalence Checking." },
  { "en": "Apa Itu Hardware Trojan?", "id": "Modifikasi Jahat Yang Dimasukkan Ke Sirkuit." },
  { "en": "Apa Itu Side-Channel Attack?", "id": "Serangan Berdasarkan Informasi Fisik Perangkat." },
  { "en": "Apa Itu Power Analysis Attack?", "id": "Analisis Konsumsi Daya Untuk Mencuri Informasi." },
  { "en": "Apa Itu Physically Unclonable Function (PUF)?", "id": "Fungsi Fisik Yang Sulit Dikloning." },
  { "en": "Apa Kepanjangan Dari PUF?", "id": "Physically Unclonable Function." },
  { "en": "Apa Itu YIG Oscillator?", "id": "Osilator Gelombang Mikro Berbasis Yttrium Iron Garnet." },
  { "en": "Apa Itu Dielectric Resonator Oscillator (DRO)?", "id": "Osilator Berbasis Resonator Keramik." },
  { "en": "Apa Kepanjangan Dari DRO?", "id": "Dielectric Resonator Oscillator." },
  { "en": "Apa Kepanjangan Dari MMIC (Monolithic Microwave Integrated Circuit)?", "id": "Monolithic Microwave Integrated Circuit." },
  { "en": "Apa Keunggulan MMIC?", "id": "Ukuran Kecil Dan Performa Frekuensi Tinggi." },
  { "en": "Apa Itu Protokol SpaceWire?", "id": "Jaringan Data Untuk Pesawat Luar Angkasa." },
  { "en": "Apa Itu Protokol ARINC 429?", "id": "Standar Bus Data Untuk Avionik Komersial." },
  { "en": "Apa Fungsi Utama Protokol MIDI?", "id": "Komunikasi Antar Instrumen Musik Elektronik." },
  { "en": "Apa Itu Antarmuka DisplayPort?", "id": "Antarmuka Audio Dan Video Digital." },
  { "en": "Apa Itu Log-Antilog Amplifier?", "id": "Penguat Untuk Operasi Perkalian Dan Pembagian." },
  { "en": "Apa Fungsi Analog Multiplier?", "id": "Mengalikan Dua Sinyal Analog." },
  { "en": "Apa Fungsi Histeresis Pada Komparator?", "id": "Mencegah Osilasi Di Dekat Titik Ambang." },
  { "en": "Apa Itu Proses Wafer Dicing?", "id": "Proses Memotong Wafer Menjadi Die." },
  { "en": "Apa Itu Software Defined Networking (SDN)?", "id": "Arsitektur Jaringan Dengan Bidang Kontrol Terpisah." },
  { "en": "Apa Kepanjangan Dari SDN?", "id": "Software Defined Networking." },
  { "en": "Apa Itu Edge AI?", "id": "Kecerdasan Buatan Yang Dijalankan Di Perangkat Lokal." },
  { "en": "Apa Itu Konsep TinyML (Tiny Machine Learning)?", "id": "Machine Learning Untuk Mikrokontroler." },
  { "en": "Apa Itu Hardware Acceleration?", "id": "Menggunakan Perangkat Keras Khusus Untuk Komputasi." },
  { "en": "Apa Fungsi Dasar Vacuum Tube?", "id": "Menguatkan Atau Mengalihkan Sinyal Elektronik." },
  { "en": "Apa Kepanjangan Tampilan CRT (Cathode Ray Tube)?", "id": "Cathode Ray Tube." },
  { "en": "Apa Itu Nixie Tube?", "id": "Perangkat Tampilan Angka Berbasis Gas." },
  { "en": "Apa Itu Core Memory?", "id": "Jenis Memori Komputer Magnetik Awal." },
  { "en": "Apa Itu Standar MIL-STD-883?", "id": "Standar Metode Uji Untuk Mikroelektronika Militer." },
  { "en": "Apa Standar IPC-A-610?", "id": "Standar Keterterimaan Rakitan Elektronik." },
  { "en": "Apa Standar UL 94?", "id": "Standar Uji Kemudahan Terbakar Plastik." },
  { "en": "Apa Itu Gibbs Phenomenon?", "id": "Artefak Sinyal Dekat Diskontinuitas." },
  { "en": "Apa Itu Aliasing Spasial?", "id": "Artefak visual Akibat Sampling Gambar." },
  { "en": "Apa Itu Moiré Pattern?", "id": "Pola Interferensi Akibat Tumpang Tindih Kisi." },
  { "en": "Apa Itu Miller Capacitance?", "id": "Kapasitansi Yang Tampak Di Input Penguat." },
  { "en": "Apa Itu Early Voltage?", "id": "Parameter Yang Mengkarakterisasi Efek Early." },
  { "en": "Apa Itu Gummel Plot?", "id": "Grafik Karakteristik Arus Transistor Bipolar." },
  { "en": "Apa Itu Small-Signal Model?", "id": "Model Linier Perangkat Non-Linier." },
  { "en": "Apa Itu Large-Signal Model?", "id": "Model Non-Linier Perangkat Elektronik." },
  { "en": "Apa Itu Harmonic Distortion?", "id": "Distorsi Akibat Munculnya Frekuensi Harmonik." },
  { "en": "Apa Itu Total Harmonic Distortion plus Noise (THD+N)?", "id": "Ukuran Kualitas Sinyal Audio." },
  { "en": "Apa Itu Dynamic Range?", "id": "Rasio Antara Sinyal Terkuat Terlemah." },
  { "en": "Apa Itu Linearity?", "id": "Kemampuan Sistem Menghasilkan Output Proporsional." },
  { "en": "Apa Itu Monotonicity (dalam DAC)?", "id": "Karakteristik Kenaikan Output Seiring Input." },
  { "en": "Apa Itu Differential Non-Linearity (DNL)?", "id": "Ukuran Kesalahan Antara Langkah-Langkah DAC/ADC." },
  { "en": "Apa Kepanjangan Dari DNL?", "id": "Differential Non-Linearity." },
  { "en": "Apa Itu Integral Non-Linearity (INL)?", "id": "Ukuran Penyimpangan Dari Fungsi Transfer Ideal." },
  { "en": "Apa Kepanjangan Dari INL?", "id": "Integral Non-Linearity." },
  { "en": "Apa Itu Glitch Energy?", "id": "Energi Pulsa Glitch Pada Output DAC." },
  { "en": "Apa Itu Settling Time?", "id": "Waktu Output Mencapai Nilai Akhir." },
  { "en": "Apa Itu Acquisition Time?", "id": "Waktu Sirkuit Sample-and-Hold Mengunci Sinyal." },
  { "en": "Apa Itu Aperture Jitter?", "id": "Variasi Waktu Dalam Proses Sampling." },
  { "en": "Apa Itu System on a Programmable Chip (SOPC)?", "id": "Sistem Lengkap Dalam Satu FPGA." },
  { "en": "Apa Kepanjangan Dari SOPC?", "id": "System on a Programmable Chip." },
  { "en": "Apa Itu Platform Cable?", "id": "Kabel Pemrograman Dan Debug Untuk FPGA Xilinx." },
  { "en": "Apa Itu Bitstream?", "id": "File Konfigurasi Untuk Memprogram FPGA." },
  { "en": "Apa Itu JTAG Chain?", "id": "Rangkaian Perangkat Yang Terhubung Melalui JTAG." },
  { "en": "Apa Itu In-System Programming (ISP)?", "id": "Pemrograman Perangkat Saat Terpasang Di Sistem." },
  { "en": "Apa Kepanjangan Dari ISP?", "id": "In-System Programming." },
  { "en": "Apa Itu Voltage Droop?", "id": "Penurunan Tegangan Sementara Akibat Beban." },
  { "en": "Apa Itu Load Regulation?", "id": "Kemampuan Catu Daya Mempertahankan Output." },
  { "en": "Apa Itu Line Regulation?", "id": "Kemampuan Catu Daya Mengatasi Perubahan Input." },
  { "en": "Apa Itu Power Supply Rejection Ratio (PSRR)?", "id": "Kemampuan Sirkuit Menolak Noise Catu Daya." },
  { "en": "Apa Kepanjangan Dari PSRR?", "id": "Power Supply Rejection Ratio." },
  { "en": "Apa Itu Quiescent Current?", "id": "Arus Yang Dikonsumsi Sirkuit Saat Diam." },
  { "en": "Apa Itu Shutdown Current?", "id": "Arus Yang Dikonsumsi Sirkuit Saat Dimatikan." },
  { "en": "Apa Itu Leakage Current?", "id": "Arus Bocor Saat Perangkat Seharusnya Mati." },
  { "en": "Apa Itu Dark Current?", "id": "Arus Bocor Pada Fotodetektor Tanpa Cahaya." },
  { "en": "Apa Itu Ideality Factor (Dioda)?", "id": "Parameter Yang Mengukur Kedekatan Dioda Ideal." },
  { "en": "Apa Itu Reverse Recovery Time?", "id": "Waktu Dioda Pulih Dari Kondisi Forward." },
  { "en": "Apa Itu Channel Length Modulation?", "id": "Efek Pada MOSFET Mirip Efek Early." },
  { "en": "Apa Itu Subthreshold Conduction?", "id": "Konduksi Arus Kecil Di Bawah Tegangan Ambang." },
  { "en": "Apa Itu Body Effect?", "id": "Perubahan Tegangan Ambang Akibat Tegangan Substrat." },
  { "en": "Apa Itu Velocity Saturation?", "id": "Kecepatan Pembawa Muatan Mencapai Batas Maksimum." },
  { "en": "Apa Itu Mean Free Path?", "id": "Jarak Rata-Rata Partikel Sebelum Bertumbukan." },
  { "en": "Apa Itu Fermi Level?", "id": "Tingkat Energi Hipotetis Dalam Semikonduktor." },
  { "en": "Apa Itu Conduction Band?", "id": "Pita Energi Dimana Elektron Dapat Bergerak." },
  { "en": "Apa Itu Valence Band?", "id": "Pita Energi Terluar Yang Terisi Elektron." },
  { "en": "Apa Itu Intrinsic Carrier Concentration?", "id": "Konsentrasi Pembawa Muatan Dalam Semikonduktor Murni." },
  { "en": "Apa Itu Majority Carrier?", "id": "Pembawa Muatan Paling Banyak." },
  { "en": "Apa Itu Minority Carrier?", "id": "Pembawa Muatan Paling Sedikit." },
  { "en": "Apa Itu Recombination?", "id": "Proses Elektron Dan Lubang Bergabung Kembali." },
  { "en": "Apa Itu Josephson Junction?", "id": "Sambungan Dua Superkonduktor." },
  { "en": "Apa Kepanjangan Perangkat SQUID (Superconducting Quantum Interference Device)?", "id": "Superconducting Quantum Interference Device." },
  { "en": "Apa Fungsi Utama SQUID?", "id": "Mengukur Medan Magnet Yang Sangat Lemah." },
  { "en": "Apa Itu Quantum Hall Effect?", "id": "Efek Kuantum Pada Konduktansi Hall." },
  { "en": "Apa Itu Coulomb Blockade?", "id": "Peningkatan Resistansi Pada Junction Kecil." },
  { "en": "Apa Prinsip Landauer?", "id": "Batas Energi Minimum Untuk Menghapus Informasi." },
  { "en": "Apa Kepanjangan Proses ECO (Engineering Change Order)?", "id": "Engineering Change Order." },
  { "en": "Apa Tujuan Dari Proses ECO?", "id": "Mengimplementasikan Perubahan Pada Desain Final." },
  { "en": "Apa Itu Metal-Insulator-Metal (MIM) Capacitor?", "id": "Kapasitor Presisi Tinggi Dalam IC." },
  { "en": "Apa Itu Proses Parasitic Extraction?", "id": "Menghitung Elemen Parasitik Dari Layout." },
  { "en": "Apa Itu Stub Matching?", "id": "Teknik Penyesuaian Impedansi Menggunakan Stub." },
  { "en": "Apa Itu Self-Resonant Frequency (SRF)?", "id": "Frekuensi Resonansi Akibat Elemen Parasitik." },
  { "en": "Apa Kepanjangan Dari SRF?", "id": "Self-Resonant Frequency." },
  { "en": "Apa Itu Heterogeneous System Architecture (HSA)?", "id": "Arsitektur Yang Mengintegrasikan CPU Dan GPU." },
  { "en": "Apa Kepanjangan Dari GPGPU?", "id": "General-Purpose Computing on Graphics Processing Units." },
  { "en": "Apa Itu Protokol Koherensi Cache MESI?", "id": "Protokol Untuk Menjaga Konsistensi Cache." },
  { "en": "Apa Itu Cepstrum?", "id": "Hasil Transformasi Fourier Dari Spektrum Log." },
  { "en": "Apa Itu Wiener Filter?", "id": "Filter Optimal Untuk Mengurangi Noise." },
  { "en": "Apa Kepanjangan Dari GaN HEMT?", "id": "Gallium Nitride High-Electron-Mobility Transistor." },
  { "en": "Apa Keunggulan GaN HEMT?", "id": "Perangkat Frekuensi Sangat Tinggi." },
  { "en": "Apa Itu Zero-Voltage Switching (ZVS)?", "id": "Teknik Switching Saat Tegangan Nol." },
  { "en": "Apa Kepanjangan Dari ZVS?", "id": "Zero-Voltage Switching." },
  { "en": "Apa Itu Zero-Current Switching (ZCS)?", "id": "Teknik Switching Saat Arus Nol." },
  { "en": "Apa Kepanjangan Dari ZCS?", "id": "Zero-Current Switching." },
  { "en": "Apa Fungsi Keyword Volatile Dalam C?", "id": "Mencegah Optimisasi Kompiler Pada Variabel." },
  { "en": "Apa Itu Memory Barrier?", "id": "Memastikan Urutan Operasi Memori." },
  { "en": "Apa Itu Atomic Operation?", "id": "Operasi Yang Tidak Dapat Diinterupsi." },
  { "en": "Apa Itu Proses Epitaxy?", "id": "Pertumbuhan Lapisan Kristal Tipis." },
  { "en": "Apa Itu Proses Annealing?", "id": "Proses Pemanasan Untuk Memperbaiki Sifat Material." },
  { "en": "Apa Itu Proses Gettering?", "id": "Proses Menghilangkan Ketidakmurnian Dari Wafer." },
  { "en": "Apa Efek Probe Loading?", "id": "Pengaruh Probe Pengukuran Terhadap Rangkaian." },
  { "en": "Apa Implikasi Teorema Shannon-Hartley?", "id": "Menentukan Kapasitas Maksimum Suatu Saluran." },
  { "en": "Bagaimana Interpretasi Bode Plot?", "id": "Menganalisis Stabilitas Sistem Dari Gain Fasa." },
  { "en": "Apa Satuan Muatan Listrik?", "id": "Satuan Muatan Listrik Adalah Coulomb." },
  { "en": "Apa Satuan Energi?", "id": "Satuan Energi Adalah Joule." },
  { "en": "Apa Satuan Fluks Magnetik?", "id": "Satuan Fluks Magnetik Adalah Weber." },
  { "en": "Apa Satuan Kepadatan Fluks Magnetik?", "id": "Satuan Kepadatan Fluks Magnetik Adalah Tesla." },
  { "en": "Apa Itu Termokopel Tipe K?", "id": "Termokopel Populer Berbahan Chromel-Alumel." },
  { "en": "Apa Itu Cold Junction Compensation?", "id": "Kompensasi Untuk Suhu Referensi Termokopel." },
  { "en": "Apa Itu Radiation Hardening?", "id": "Membuat Komponen Elektronik Tahan Radiasi." },
  { "en": "Apa Itu Total Ionizing Dose (TID)?", "id": "Ukuran Kerusakan Akibat Radiasi Pengion." },
  { "en": "Apa Kepanjangan Dari TID?", "id": "Total Ionizing Dose." },
  { "en": "Apa Itu Optical Time-Domain Reflectometer (OTDR)?", "id": "Alat Uji Untuk Serat Optik." },
  { "en": "Apa Kepanjangan Dari OTDR?", "id": "Optical Time-Domain Reflectometer." },
  { "en": "Apa Itu Chromatic Dispersion?", "id": "Penyebaran Pulsa Cahaya Akibat Perbedaan Kecepatan." },
  { "en": "Apa Itu Birefringence?", "id": "Sifat Material Optik Anisotropik." },
  { "en": "Apa Itu Faraday Rotator?", "id": "Komponen Optik Yang Memutar Polarisasi." },
  { "en": "Apa Itu Photonic Crystal?", "id": "Struktur Periodik Yang Mempengaruhi Gerak Foton." },
  { "en": "Apa Itu Metamaterial?", "id": "Material Buatan Dengan Sifat Tidak Biasa." },
  { "en": "Apa Itu Surface Plasmon Resonance (SPR)?", "id": "Eksitasi Plasmon Permukaan Oleh Cahaya." },
  { "en": "Apa Kepanjangan Dari SPR?", "id": "Surface Plasmon Resonance." },
  { "en": "Apa Itu Verilog-AMS?", "id": "Bahasa Pemodelan Perilaku Analog Mixed-Signal." },
  { "en": "Apa Itu VHDL-AMS?", "id": "VHDL Dengan Kemampuan Pemodelan Analog." },
  { "en": "Apa Itu SystemC?", "id": "Bahasa Pemodelan Sistem Berbasis C++." },
  { "en": "Apa Itu Fuzzy Logic?", "id": "Logika Berbasis Derajat Kebenaran." },
  { "en": "Apa Itu Neural Network?", "id": "Model Komputasi Yang Terinspirasi Otak." },
  { "en": "Apa Itu Genetic Algorithm?", "id": "Algoritma Optimisasi Berbasis Evolusi." },
  { "en": "Apa Itu Finite Element Analysis (FEA)?", "id": "Metode Analisis Numerik Untuk Masalah Fisika." },
  { "en": "Apa Kepanjangan Dari FEA?", "id": "Finite Element Analysis." },
  { "en": "Apa Itu Computational Fluid Dynamics (CFD)?", "id": "Simulasi Aliran Fluida Menggunakan Komputer." },
  { "en": "Apa Kepanjangan Dari CFD?", "id": "Computational Fluid Dynamics." },
  { "en": "Apa Itu Thermal Resistance?", "id": "Ukuran Hambatan Aliran Panas." },
  { "en": "Apa Itu Junction Temperature?", "id": "Suhu Operasi Persambungan Semikonduktor." },
  { "en": "Apa Itu Ambient Temperature?", "id": "Suhu Lingkungan Sekitar." },
  { "en": "Apa Itu Case Temperature?", "id": "Suhu Pada Kemasan Luar Komponen." },
  { "en": "Apa Itu Power Dissipation?", "id": "Jumlah Daya Yang Dibuang Sebagai Panas." },
  { "en": "Apa Itu Derating Curve?", "id": "Grafik Batas Aman Operasi Komponen." },
  { "en": "Apa Itu Mean Time To Failure (MTTF)?", "id": "Waktu Rata-Rata Hingga Terjadi Kegagalan." },
  { "en": "Apa Kepanjangan Dari MTTF?", "id": "Mean Time To Failure." },
  { "en": "Apa Itu Bathtub Curve?", "id": "Grafik Tingkat Kegagalan Produk Seiring Waktu." },
  { "en": "Apa Itu Infant Mortality?", "id": "Tingkat Kegagalan Tinggi Di Awal Kehidupan." },
  { "en": "Apa Itu Random Failures?", "id": "Kegagalan Acak Selama Masa Pakai Normal." },
  { "en": "Apa Itu Wear-Out Failures?", "id": "Kegagalan Akibat Penuaan Di Akhir Kehidupan." },
  { "en": "Apa Itu Highly Accelerated Stress Test (HAST)?", "id": "Uji Keandalan Di Bawah Stres Ekstrem." },
  { "en": "Apa Kepanjangan Dari HAST?", "id": "Highly Accelerated Stress Test." },
  { "en": "Apa Itu Root Cause Analysis?", "id": "Proses Identifikasi Penyebab Dasar Masalah." },
  { "en": "Apa Itu Five Whys?", "id": "Teknik Bertanya Untuk Menemukan Akar Masalah." },
  { "en": "Apa Itu Fault Tree Analysis (FTA)?", "id": "Analisis Deduktif Untuk Mengidentifikasi Penyebab Kegagalan." },
  { "en": "Apa Kepanjangan Dari FTA?", "id": "Fault Tree Analysis." },
  { "en": "Apa Itu Return Material Authorization (RMA)?", "id": "Proses Pengembalian Produk Rusak." },
  { "en": "Apa Kepanjangan Dari RMA?", "id": "Return Material Authorization." },
  { "en": "Apa Itu End User?", "id": "Pengguna Akhir Sebuah Produk." },
  { "en": "Apa Itu Proof of Concept (PoC)?", "id": "Realisasi Metode Untuk Menunjukkan Kelayakan." },
  { "en": "Apa Kepanjangan Dari PoC?", "id": "Proof of Concept." },
  { "en": "Apa Itu Minimum Viable Product (MVP)?", "id": "Versi Produk Paling Sederhana." },
  { "en": "Apa Kepanjangan Dari MVP?", "id": "Minimum Viable Product." },
  { "en": "Apa Itu Alpha Testing?", "id": "Pengujian Internal Sebelum Rilis." },
  { "en": "Apa Itu Beta Testing?", "id": "Pengujian Eksternal Oleh Pengguna Terbatas." },
  { "en": "Apa Itu General Availability (GA)?", "id": "Rilis Produk Resmi Untuk Umum." },
  { "en": "Apa Kepanjangan Dari GA?", "id": "General Availability." },
  { "en": "Apa Itu Product Lifecycle Management (PLM)?", "id": "Manajemen Siklus Hidup Produk." },
  { "en": "Apa Kepanjangan Dari PLM?", "id": "Product Lifecycle Management." },
  { "en": "Apa Itu Moore's Law?", "id": "Observasi Tentang Peningkatan Kepadatan Transistor." }



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
