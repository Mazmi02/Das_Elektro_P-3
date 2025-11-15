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


  { "en": "Apa Satuan Kerapatan Fluks Magnet (B)?", "id": "Tesla (T)." },
  { "en": "Apa Hubungan Fluks (Φ), Kerapatan Fluks (B), Luas (A)?", "id": "Φ = B x A." },
  { "en": "Apa Itu Rangkaian Magnetik (Magnetic Circuit)?", "id": "Jalur Tertutup Untuk Fluks Magnet." },
  { "en": "Apa Analogi Hukum Ohm Rangkaian Magnetik?", "id": "MMF = Φ x Reluktansi." },
  { "en": "Apa Itu Kurva Magnetisasi (Magnetization Curve)?", "id": "Grafik Hubungan B (Medan Magnet) Dan H (Kekuatan Medan)." },
  { "en": "Apa Itu Saturasi (Saturation) Magnetik?", "id": "Kondisi Inti Tidak Bisa Dimagnetisasi Lagi." },
  { "en": "Apa Itu Remanensi (Remanence) Magnetik?", "id": "Sisa Magnetisme Setelah Medan Hilang." },
  { "en": "Apa Itu Koersivitas (Coercivity) Magnetik?", "id": "Medan Balik Menghilangkan Remanensi." },
  { "en": "Apa Itu Loop Histeresis (Hysteresis Loop)?", "id": "Grafik B-H Menunjukkan Rugi Histeresis." },
  { "en": "Apa Itu Rugi Histeresis (Hysteresis Loss)?", "id": "Energi Hilang Akibat Siklus Magnetisasi." },
  { "en": "Apa Itu Rugi Arus Eddy (Eddy Current Loss)?", "id": "Energi Hilang Akibat Arus Eddy." },
  { "en": "Bagaimana Mengurangi Rugi Arus Eddy (Eddy Current Loss)?", "id": "Menggunakan Inti Besi Berlaminasi Tipis." },
  { "en": "Apa Itu Bahan Magnet Lunak (Soft Magnetic)?", "id": "Mudah Dimagnetisasi, Mudah Hilang (Trafo)." },
  { "en": "Apa Itu Bahan Magnet Keras (Hard Magnetic)?", "id": "Sulit Dimagnetisasi, Sulit Hilang (Magnet Permanen)." },
  { "en": "Apa Itu Motor Linier (Linear Motor)?", "id": "Motor Menghasilkan Gerakan Lurus Langsung." },
  { "en": "Apa Prinsip Kerja Motor Linier (Linear Motor)?", "id": "Interaksi Medan Magnet Bergerak." },
  { "en": "Apa Itu Leviasi Magnetik (Magnetic Levitation) (Maglev)?", "id": "Mengambang Menggunakan Medan Magnet." },
  { "en": "Apa Kepanjangan Maglev (Magnetic Levitation)?", "id": "Levitasi Magnetik." },
  { "en": "Apa Itu Sensor Efek Wiegand (Wiegand Effect)?", "id": "Sensor Pulsa Magnetik Tanpa Daya." },
  { "en": "Apa Itu Efek Magnetoresistif (Magnetoresistance Effect)?", "id": "Perubahan Resistansi Bahan Medan Magnet." },
  { "en": "Apa Kepanjangan GMR (Giant Magnetoresistance)?", "id": "Magnetoresistansi Raksasa." },
  { "en": "Dimana GMR (Giant Magnetoresistance) Digunakan?", "id": "Sensor Kepala Pembaca Hard Disk." },
  { "en": "Apa Itu Spintronics (Spintronics)?", "id": "Elektronika Berbasis Spin Elektron." },
  { "en": "Apa Itu Resonansi Magnetik Nuklir (NMR)?", "id": "Nuclear Magnetic Resonance (NMR) (Pencitraan Medis)." },
  { "en": "Apa Kepanjangan NMR (Nuclear Magnetic Resonance)?", "id": "Resonansi Magnetik Nuklir." },
  { "en": "Apa Kepanjangan MRI (Magnetic Resonance Imaging)?", "id": "Pencitraan Resonansi Magnetik." },
  { "en": "Apa Itu Pengisian Nirkabel (Wireless Charging) Induktif?", "id": "Transfer Daya Medan Magnet Dekat." },
  { "en": "Apa Itu Sistem Tenaga Listrik (Power System)?", "id": "Jaringan Pembangkitan, Transmisi, Distribusi." },
  { "en": "Apa Itu Gardu Induk (Substation)?", "id": "Fasilitas Pengubah Tegangan, Switching." },
  { "en": "Apa Fungsi Utama Gardu Induk (Substation) Transmisi?", "id": "Menaikkan Tegangan Pembangkit Ke Transmisi." },
  { "en": "Apa Fungsi Utama Gardu Induk (Substation) Distribusi?", "id": "Menurunkan Tegangan Transmisi Ke Distribusi." },
  { "en": "Apa Itu Peralatan Hubung Bagi (Switchgear)?", "id": "Kombinasi Pemutus, Pemisah, Proteksi." },
  { "en": "Apa Itu Busbar (Busbar)?", "id": "Konduktor Penghubung Utama Di Switchgear." },
  { "en": "Apa Itu Pemutus Tenaga (PMT) (Circuit Breaker)?", "id": "Saklar Pemutus Arus (Normal, Gangguan)." },
  { "en": "Apa Itu Pemisah (PMS) (Disconnector)?", "id": "Saklar Pemisah Tanpa Beban." },
  { "en": "Apa Fungsi Pemisah (PMS) (Disconnector)?", "id": "Mengisolasi Peralatan Untuk Pemeliharaan." },
  { "en": "Apa Itu Transformator Instrumen (Instrument Transformer)?", "id": "CT (Current Transformer), PT (Potential Transformer)." },
  { "en": "Apa Itu Trafo Arus (Current Transformer) (CT)?", "id": "Menurunkan Arus Tinggi Untuk Pengukuran." },
  { "en": "Apa Itu Trafo Tegangan (Potential Transformer) (PT)?", "id": "Menurunkan Tegangan Tinggi Untuk Pengukuran." },
  { "en": "Mengapa Sekunder CT (Current Transformer) Tidak Boleh Terbuka?", "id": "Menghasilkan Tegangan Sangat Tinggi Berbahaya." },
  { "en": "Apa Itu Burden (Beban) Trafo Instrumen?", "id": "Total Beban Terhubung Sekunder (VA)." },
  { "en": "Apa Itu Kelas Akurasi (Accuracy Class) Trafo Instrumen?", "id": "Tingkat Ketelitian Pengukuran." },
  { "en": "Apa Itu Rele Proteksi (Protection Relay)?", "id": "Perangkat Pendeteksi Gangguan Listrik." },
  { "en": "Apa Fungsi Rele Proteksi (Protection Relay)?", "id": "Memberi Perintah Trip Ke PMT." },
  { "en": "Apa Itu Zona Proteksi (Zone of Protection)?", "id": "Area Dilindungi Oleh Rele Tertentu." },
  { "en": "Apa Itu Selektivitas (Selectivity) Proteksi?", "id": "Kemampuan Isolasi Gangguan Area Terkecil." },
  { "en": "Apa Itu Kecepatan (Speed) Proteksi?", "id": "Waktu Reaksi Rele Memutus Gangguan." },
  { "en": "Apa Itu Keandalan (Reliability) Proteksi?", "id": "Kemampuan Proteksi Bekerja Saat Dibutuhkan." },
  { "en": "Apa Itu Keamanan (Security) Proteksi?", "id": "Kemampuan Proteksi Tidak Trip Salah." },
  { "en": "Apa Itu Rele Arus Lebih (Overcurrent Relay) (OCR)?", "id": "Rele Sensitif Terhadap Arus Berlebih." },
  { "en": "Apa Itu Rele Waktu Tunda (Time Delay Relay)?", "id": "Rele Trip Setelah Waktu Tertentu." },
  { "en": "Apa Itu Rele Seketika (Instantaneous Relay)?", "id": "Rele Trip Tanpa Waktu Tunda." },
  { "en": "Apa Itu Kurva IDMT (Inverse Definite Minimum Time)?", "id": "Karakteristik Waktu Trip Terbalik OCR." },
  { "en": "Apa Kepanjangan IDMT (Inverse Definite Minimum Time)?", "id": "Waktu Minimum Tentu Terbalik." },
  { "en": "Apa Itu Rele Diferensial (Differential Relay)?", "id": "Membandingkan Arus Masuk, Keluar Peralatan." },
  { "en": "Dimana Rele Diferensial (Differential Relay) Digunakan?", "id": "Proteksi Internal Trafo, Generator, Busbar." },
  { "en": "Apa Itu Rele Jarak (Distance Relay)?", "id": "Mengukur Impedansi (Jarak) Gangguan Saluran." },
  { "en": "Apa Itu Rele Gangguan Tanah (Ground Fault Relay)?", "id": "Mendeteksi Arus Bocor Ke Tanah." },
  { "en": "Apa Itu Rele Frekuensi (Frequency Relay)?", "id": "Mendeteksi Perubahan Frekuensi Sistem." },
  { "en": "Apa Itu Rele Tegangan Kurang (Undervoltage Relay)?", "id": "Mendeteksi Penurunan Tegangan Berbahaya." },
  { "en": "Apa Itu Rele Tegangan Lebih (Overvoltage Relay)?", "id": "Mendeteksi Kenaikan Tegangan Berbahaya." },
  { "en": "Apa Itu SCADA (Supervisory Control and Data Acquisition)?", "id": "Sistem Pemantauan, Kontrol Jarak Jauh." },
  { "en": "Apa Kepanjangan RTU (Remote Terminal Unit)?", "id": "Unit Terminal Jarak Jauh." },
  { "en": "Apa Fungsi RTU (Remote Terminal Unit) SCADA?", "id": "Mengumpulkan Data, Eksekusi Perintah Lapangan." },
  { "en": "Apa Itu Master Station (Stasiun Master) SCADA?", "id": "Pusat Kontrol Pemantauan SCADA." },
  { "en": "Apa Itu HMI (Human Machine Interface) SCADA?", "id": "Antarmuka Grafis Operator SCADA." },
  { "en": "Protokol Komunikasi Apa Digunakan SCADA?", "id": "Modbus, DNP3, IEC 60870-5." },
  { "en": "Apa Kepanjangan DNP3 (Distributed Network Protocol 3)?", "id": "Protokol Jaringan Terdistribusi 3." },
  { "en": "Apa Itu Stabilitas Sistem Tenaga (Power System Stability)?", "id": "Kemampuan Sistem Kembali Normal Pasca Gangguan." },
  { "en": "Apa Itu Stabilitas Sudut Rotor (Rotor Angle Stability)?", "id": "Kemampuan Generator Tetap Sinkron." },
  { "en": "Apa Itu Stabilitas Transien (Transient Stability)?", "id": "Stabilitas Pasca Gangguan Besar (Detik)." },
  { "en": "Apa Itu Stabilitas Dinamis (Dynamic Stability)?", "id": "Stabilitas Terhadap Gangguan Kecil Berkelanjutan." },
  { "en": "Apa Itu Stabilitas Tegangan (Voltage Stability)?", "id": "Kemampuan Sistem Mempertahankan Tegangan." },
  { "en": "Apa Itu Kolaps Tegangan (Voltage Collapse)?", "id": "Penurunan Tegangan Drastis Tak Terkendali." },
  { "en": "Apa Itu Kurva P-V (Kurva Daya-Tegangan)?", "id": "Analisis Stabilitas Tegangan." },
  { "en": "Apa Itu Kurva Q-V (Kurva Reaktif-Tegangan)?", "id": "Analisis Stabilitas Tegangan." },
  { "en": "Apa Itu Batas Stabilitas (Stability Limit)?", "id": "Batas Operasi Aman Sistem Tenaga." },
  { "en": "Apa Itu Power System Stabilizer (PSS)?", "id": "Peredam Osilasi Daya Sistem." },
  { "en": "Apa Kepanjangan PSS (Power System Stabilizer)?", "id": "Penstabil Sistem Tenaga." },
  { "en": "Dimana PSS (Power System Stabilizer) Dipasang?", "id": "Pada Sistem Eksitasi Generator." },
  { "en": "Apa Itu FACTS (Flexible AC Transmission Systems)?", "id": "Peralatan Elektronika Daya Kontrol Transmisi." },
  { "en": "Apa Kepanjangan SVC (Static VAR Compensator)?", "id": "Kompensator VAR Statis." },
  { "en": "Apa Fungsi SVC (Static VAR Compensator)?", "id": "Kompensasi Daya Reaktif Cepat." },
  { "en": "Apa Kepanjangan STATCOM (Static Synchronous Compensator)?", "id": "Kompensator Sinkron Statis." },
  { "en": "Apa Keunggulan STATCOM (Static Synchronous Compensator) Dibanding SVC?", "id": "Respon Lebih Cepat, Kontrol Lebih Baik." },
  { "en": "Apa Kepanjangan TCSC (Thyristor Controlled Series Capacitor)?", "id": "Kapasitor Seri Terkontrol Thyristor." },
  { "en": "Apa Fungsi TCSC (Thyristor Controlled Series Capacitor)?", "id": "Mengatur Impedansi Saluran Transmisi." },
  { "en": "Apa Kepanjangan UPFC (Unified Power Flow Controller)?", "id": "Pengontrol Aliran Daya Terpadu." },
  { "en": "Apa Fungsi UPFC (Unified Power Flow Controller)?", "id": "Kontrol Komprehensif Aliran Daya." },
  { "en": "Apa Itu HVDC (High Voltage Direct Current) Transmission?", "id": "Transmisi Daya Arus Searah Tegangan Tinggi." },
  { "en": "Apa Keuntungan Transmisi HVDC (High Voltage Direct Current)?", "id": "Rugi Rendah Jarak Jauh, Interkoneksi Asinkron." },
  { "en": "Apa Komponen Utama Stasiun Konverter HVDC?", "id": "Transformator Konverter, Katup Thyristor/IGBT." },
  { "en": "Apa Itu Katup Thyristor (Thyristor Valve)?", "id": "Saklar Daya Tinggi Konverter LCC-HVDC." },
  { "en": "Apa Itu Konverter Sumber Tegangan (VSC) HVDC?", "id": "Teknologi HVDC (High Voltage Direct Current) Modern (IGBT)." },
  { "en": "Apa Kepanjangan LCC (Line Commutated Converter)?", "id": "Konverter Terkomutasi Saluran." },
  { "en": "Apa Kepanjangan VSC (Voltage Source Converter)?", "id": "Konverter Sumber Tegangan." },
  { "en": "Apa Itu Jaringan Listrik Cerdas (Smart Grid)?", "id": "Modernisasi Jaringan Listrik Komunikasi Digital." },
  { "en": "Apa Tujuan Jaringan Listrik Cerdas (Smart Grid)?", "id": "Efisiensi, Keandalan, Integrasi Energi Terbarukan." },
  { "en": "Apa Itu Meter Cerdas (Smart Meter)?", "id": "Meter Listrik Komunikasi Dua Arah." },
  { "en": "Apa Itu Infrastruktur Pengukuran Canggih (AMI)?", "id": "Advanced Metering Infrastructure (AMI)." },
  { "en": "Apa Kepanjangan AMI (Advanced Metering Infrastructure)?", "id": "Infrastruktur Pengukuran Canggih." },
  { "en": "Apa Itu Manajemen Sisi Permintaan (DSM)?", "id": "Demand Side Management (DSM)." },
  { "en": "Apa Kepanjangan DSM (Demand Side Management)?", "id": "Manajemen Sisi Permintaan." },
  { "en": "Apa Fungsi DSM (Demand Side Management)?", "id": "Mengelola Pola Konsumsi Listrik Pelanggan." },
  { "en": "Apa Itu Respon Beban (Demand Response)?", "id": "Perubahan Konsumsi Listrik Respon Sinyal." },
  { "en": "Apa Itu Pembangkit Terdistribusi (Distributed Generation) (DG)?", "id": "Pembangkit Listrik Skala Kecil Lokal." },
  { "en": "Contoh Pembangkit Terdistribusi (Distributed Generation) (DG)?", "id": "Panel Surya Atap, Generator Kecil." },
  { "en": "Apa Itu Jaringan Mikro (Microgrid)?", "id": "Jaringan Listrik Lokal Dapat Mandiri." },
  { "en": "Apa Itu Islanding (Islanding) Jaringan Mikro?", "id": "Mode Operasi Terpisah Dari Jaringan Utama." },
  { "en": "Apa Itu Sistem Penyimpanan Energi (ESS)?", "id": "Energy Storage System (ESS)." },
  { "en": "Apa Kepanjangan ESS (Energy Storage System)?", "id": "Sistem Penyimpanan Energi." },
  { "en": "Contoh Sistem Penyimpanan Energi (ESS)?", "id": "Baterai (BESS), Penyimpanan Udara Terkompresi." },
  { "en": "Apa Kepanjangan BESS (Battery Energy Storage System)?", "id": "Sistem Penyimpanan Energi Baterai." },
  { "en": "Apa Fungsi Sistem Penyimpanan Energi (ESS)?", "id": "Menstabilkan Jaringan, Integrasi Energi Terbarukan." },
  { "en": "Apa Itu Kendaraan Listrik (Electric Vehicle) (EV)?", "id": "Kendaraan Digerakkan Motor Listrik." },
  { "en": "Apa Kepanjangan EV (Electric Vehicle)?", "id": "Kendaraan Listrik." },
  { "en": "Apa Kepanjangan BEV (Battery Electric Vehicle)?", "id": "Kendaraan Listrik Baterai (Sepenuhnya Listrik)." },
  { "en": "Apa Kepanjangan PHEV (Plug-in Hybrid Electric Vehicle)?", "id": "Kendaraan Listrik Hibrida Plug-in." },
  { "en": "Apa Kepanjangan HEV (Hybrid Electric Vehicle)?", "id": "Kendaraan Listrik Hibrida (Non Plug-in)." },
  { "en": "Apa Itu Pengisian Kendaraan Listrik (EV Charging)?", "id": "Proses Pengisian Baterai Kendaraan Listrik." },
  { "en": "Apa Itu Pengisian AC (AC Charging) Level 1?", "id": "Pengisian Lambat (Stop Kontak Rumah)." },
  { "en": "Apa Itu Pengisian AC (AC Charging) Level 2?", "id": "Pengisian Sedang (Stasiun Pengisian Umum)." },
  { "en": "Apa Itu Pengisian DC (DC Charging) Cepat?", "id": "Pengisian Sangat Cepat (Level 3)." },
  { "en": "Apa Kepanjangan V2G (Vehicle-to-Grid)?", "id": "Kendaraan Ke Jaringan." },
  { "en": "Apa Fungsi V2G (Vehicle-to-Grid)?", "id": "Kendaraan Listrik Menyuplai Listrik Ke Jaringan." },
  { "en": "Apa Itu Elektromagnetisme Komputasi (CEM)?", "id": "Computational Electromagnetics (CEM)." },
  { "en": "Apa Kepanjangan CEM (Computational Electromagnetics)?", "id": "Elektromagnetisme Komputasi." },
  { "en": "Metode Apa Digunakan Dalam CEM (Computational Electromagnetics)?", "id": "FEM (Finite Element Method), FDTD (Finite Difference Time Domain)." },
  { "en": "Apa Kepanjangan FDTD (Finite Difference Time Domain)?", "id": "Beda Hingga Domain Waktu." },
  { "en": "Apa Itu Method of Moments (MoM)?", "id": "Metode Numerik Analisis Antena, Hamburan." },
  { "en": "Apa Kepanjangan MoM (Method of Moments)?", "id": "Metode Momen." },
  { "en": "Apa Itu Bahan Dielektrik (Dielectric Material)?", "id": "Bahan Isolator Dapat Terpolarisasi." },
  { "en": "Apa Itu Konstanta Dielektrik (Dielectric Constant) (εr)?", "id": "Permitivitas Relatif Bahan." },
  { "en": "Apa Itu Rugi Dielektrik (Dielectric Loss)?", "id": "Energi Hilang Bahan Dielektrik Medan AC." },
  { "en": "Apa Itu Tangen Rugi (Loss Tangent) (Tan δ)?", "id": "Ukuran Rugi Dielektrik Bahan." },
  { "en": "Apa Itu Kekuatan Dielektrik (Dielectric Strength)?", "id": "Medan Listrik Maksimum Tahan Isolator." },
  { "en": "Apa Itu Bahan Piezoelektrik (Piezoelectric Material)?", "id": "Hasilkan Listrik Tekanan Mekanis." },
  { "en": "Contoh Bahan Piezoelektrik (Piezoelectric Material)?", "id": "Kuarsa (Quartz), PZT (Lead Zirconate Titanate)." },
  { "en": "Dimana Bahan Piezoelektrik (Piezoelectric Material) Digunakan?", "id": "Sensor Tekanan, Osilator Kristal, Buzzer." },
  { "en": "Apa Itu Bahan Piroelektrik (Pyroelectric Material)?", "id": "Hasilkan Listrik Perubahan Suhu." },
  { "en": "Dimana Bahan Piroelektrik (Pyroelectric Material) Digunakan?", "id": "Sensor Gerak Inframerah (PIR)." },
  { "en": "Apa Itu Bahan Feroelektrik (Ferroelectric Material)?", "id": "Memiliki Polarisasi Spontan Dapat Dibalik." },
  { "en": "Dimana Bahan Feroelektrik (Ferroelectric Material) Digunakan?", "id": "Memori FeRAM (Ferroelectric RAM), Kapasitor." },
  { "en": "Apa Kepanjangan FeRAM (Ferroelectric RAM)?", "id": "RAM (Random Access Memory) Feroelektrik." },
  { "en": "Apa Itu Efek Elektro-Optik (Electro-Optic Effect)?", "id": "Perubahan Sifat Optik Medan Listrik." },
  { "en": "Contoh Efek Elektro-Optik (Electro-Optic Effect)?", "id": "Efek Pockels, Efek Kerr." },
  { "en": "Dimana Efek Elektro-Optik (Electro-Optic Effect) Digunakan?", "id": "Modulator Optik, Display LCD." },
  { "en": "Apa Itu Fotonik (Photonics)?", "id": "Ilmu Teknologi Pembangkitan, Deteksi Cahaya." },
  { "en": "Apa Itu Optoelektronika (Optoelectronics)?", "id": "Studi Perangkat Interaksi Cahaya, Elektron." },
  { "en": "Contoh Perangkat Optoelektronika (Optoelectronics)?", "id": "LED, Fotodioda, Optocoupler, Laser." },
  { "en": "Apa Itu Emisi Spontan (Spontaneous Emission)?", "id": "Pemancaran Foton Tanpa Stimulasi." },
  { "en": "Apa Itu Emisi Terstimulasi (Stimulated Emission)?", "id": "Pemancaran Foton Dipicu Foton Lain." },
  { "en": "Prinsip Dasar Apa Untuk Laser (Laser)?", "id": "Emisi Terstimulasi (Stimulated Emission)." },
  { "en": "Apa Itu Cahaya Koheren (Coherent Light)?", "id": "Gelombang Cahaya Memiliki Fasa Sama." },
  { "en": "Apa Sifat Cahaya Laser (Laser)?", "id": "Koheren, Monokromatik, Terarah (Direktif)." },
  { "en": "Apa Itu Monokromatik (Monochromatic)?", "id": "Cahaya Terdiri Satu Panjang Gelombang." },
  { "en": "Apa Itu Holografi (Holography)?", "id": "Teknik Perekaman, Rekonstruksi Gambar 3D." },
  { "en": "Apa Itu Interferensi (Interference) Gelombang?", "id": "Superposisi Gelombang (Konstruktif/Destruktif)." },
  { "en": "Apa Itu Difraksi (Diffraction) Gelombang?", "id": "Pelenturan Gelombang Lewati Celah Sempit." },
  { "en": "Apa Itu Kisi Difraksi (Diffraction Grating)?", "id": "Alat Pemisah Cahaya Berdasarkan Lambda." },
  { "en": "Apa Itu Efek Akusto-Optik (Acousto-Optic Effect)?", "id": "Interaksi Cahaya, Gelombang Suara Medium." },
  { "en": "Apa Itu Modulator Akusto-Optik (AOM)?", "id": "Perangkat Modulasi Cahaya Gelombang Suara." },
  { "en": "Apa Kepanjangan AOM (Acousto-Optic Modulator)?", "id": "Modulator Akusto-Optik." },
  { "en": "Apa Itu Elektroluminesensi (Electroluminescence)?", "id": "Emisi Cahaya Bahan Akibat Listrik." },
  { "en": "Prinsip Dasar Apa Untuk LED (Light Emitting Diode)?", "id": "Elektroluminesensi (Electroluminescence)." },
  { "en": "Apa Itu Luminansi (Luminance)?", "id": "Kecerahan Permukaan Yang Terlihat Mata." },
  { "en": "Apa Satuan Luminansi (Luminance)?", "id": "Candela Per Meter Persegi (cd/m²)." },
  { "en": "Apa Itu Iluminansi (Illuminance)?", "id": "Fluks Cahaya Jatuh Per Satuan Luas." },
  { "en": "Apa Satuan Iluminansi (Illuminance)?", "id": "Lux (lx)." },
  { "en": "Apa Itu Efisiensi Kuantum (Quantum Efficiency) Detektor?", "id": "Rasio Elektron Dihasilkan Per Foton." },
  { "en": "Apa Itu Arus Gelap (Dark Current) Fotodioda?", "id": "Arus Bocor Tanpa Cahaya Masuk." },
  { "en": "Apa Itu Fotomultiplier Tube (PMT)?", "id": "Detektor Cahaya Sangat Sensitif." },
  { "en": "Apa Kepanjangan PMT (Photomultiplier Tube)?", "id": "Tabung Fotomultiplier." },
  { "en": "Apa Itu Charged-Couple Device (CCD)?", "id": "Sensor Gambar Elektronik." },
  { "en": "Apa Kepanjangan CCD (Charged-Couple Device)?", "id": "Perangkat Muatan Terkopel." },
  { "en": "Apa Itu Sensor CMOS (Complementary Metal-Oxide-Semiconductor) Image?", "id": "Sensor Gambar Alternatif CCD (Charge-Coupled Device)." },
  { "en": "Dimana Sensor CCD (Charged-Couple Device), CMOS (Complementary Metal-Oxide-Semiconductor) Digunakan?", "id": "Kamera Digital, Ponsel." },
  { "en": "Apa Itu Bilangan Kuantum (Quantum Number)?", "id": "Angka Deskripsi Keadaan Elektron Atom." },
  { "en": "Apa Itu Prinsip Eksklusi Pauli (Pauli Exclusion Principle)?", "id": "Dua Elektron Tidak Boleh Sama Kuantum." },
  { "en": "Apa Itu Ikatan Kovalen (Covalent Bond)?", "id": "Ikatan Kimia (Berbagi Elektron Valensi)." },
  { "en": "Ikatan Apa Dalam Kristal Silikon (Si)?", "id": "Ikatan Kovalen (Covalent Bond)." },
  { "en": "Apa Itu Cacat (Defect) Kristal?", "id": "Ketidaksempurnaan Struktur Kristal Semikonduktor." },
  { "en": "Apa Itu Rekombinasi (Recombination) Pembawa Muatan?", "id": "Elektron Bebas Kembali Mengisi Hole." },
  { "en": "Apa Itu Generasi (Generation) Pembawa Muatan?", "id": "Penciptaan Pasangan Elektron-Hole (Panas/Cahaya)." },
  { "en": "Apa Itu Waktu Hidup (Lifetime) Pembawa Muatan?", "id": "Waktu Rata-Rata Sebelum Rekombinasi." },
  { "en": "Apa Itu Panjang Difusi (Diffusion Length)?", "id": "Jarak Rata-Rata Muatan Sebelum Rekombinasi." },
  { "en": "Apa Itu Persamaan Kontinuitas (Continuity Equation)?", "id": "Persamaan Kekekalan Muatan Semikonduktor." },
  { "en": "Apa Itu Efek Fotokonduktif (Photoconductive Effect)?", "id": "Peningkatan Konduktivitas Bahan Akibat Cahaya." },
  { "en": "Apa Itu Efek Fotovoltaik (Photovoltaic Effect)?", "id": "Pembangkitan Tegangan Akibat Cahaya (Sel Surya)." },
  { "en": "Apa Itu Epitaksi (Epitaxy)?", "id": "Pertumbuhan Lapisan Kristal Tipis." },
  { "en": "Apa Itu Deposisi Uap Kimia (CVD)?", "id": "Chemical Vapor Deposition (CVD)." },
  { "en": "Apa Kepanjangan MOCVD (Metalorganic Chemical Vapor Deposition)?", "id": "Deposisi Uap Kimia Metalorganik." },
  { "en": "Apa Itu MBE (Molecular Beam Epitaxy)?", "id": "Epitaksi Berkas Molekuler." },
  { "en": "Apa Kepanjangan MBE (Molecular Beam Epitaxy)?", "id": "Epitaksi Berkas Molekuler." },
  { "en": "Apa Itu Litografi (Lithography)?", "id": "Teknik Pencetakan Pola (Fabrikasi IC)." },
  { "en": "Apa Itu Litografi EUV (Extreme Ultraviolet)?", "id": "Litografi Generasi Berikut (Resolusi Tinggi)." },
  { "en": "Apa Kepanjangan EUV (Extreme Ultraviolet)?", "id": "Ultraviolet Ekstrem." },
  { "en": "Apa Itu Hukum Moore (Moore's Law)?", "id": "Prediksi Jumlah Transistor Chip Gandakan." },
  { "en": "Berapa Periode Penggandaan Hukum Moore (Moore's Law)?", "id": "Sekitar Setiap Dua Tahun." },
  { "en": "Apa Itu Skala Denard (Dennard Scaling)?", "id": "Aturan Penskalaan Transistor MOSFET." },
  { "en": "Apa Itu Tunneling Kuantum (Quantum Tunneling)?", "id": "Fenomena Partikel Menembus Penghalang Potensial." },
  { "en": "Kapan Tunneling Kuantum (Quantum Tunneling) Jadi Masalah?", "id": "Penskalaan Transistor Sangat Kecil." },
  { "en": "Apa Itu Efek Saluran Pendek (Short Channel Effect)?", "id": "Masalah Transistor MOSFET (Metal-Oxide-Semiconductor FET) Kecil." },
  { "en": "Apa Itu Transistor FinFET (Fin Field Effect Transistor)?", "id": "Struktur Transistor 3D Modern." },
  { "en": "Apa Itu GAAFET (Gate-All-Around FET)?", "id": "Struktur Transistor Generasi Berikut FinFET." },
  { "en": "Apa Kepanjangan GAAFET (Gate-All-Around Field Effect Transistor)?", "id": "FET (Field Effect Transistor) Gerbang Mengelilingi Semua." },
  { "en": "Apa Itu Sirkuit Terbuka (Open Circuit) Ideal?", "id": "Hambatan Tak Terhingga, Arus Nol." },
  { "en": "Apa Itu Hubungan Singkat (Short Circuit) Ideal?", "id": "Hambatan Nol, Tegangan Nol." },
  { "en": "Apa Satuan Konduktansi (Conductance) (G)?", "id": "Siemens (S)." },
  { "en": "Apa Hubungan Konduktansi (G) Dan Resistansi (R)?", "id": "G = 1 / R." },
  { "en": "Apa Satuan Suseptansi (Susceptance) (B)?", "id": "Siemens (S)." },
  { "en": "Apa Hubungan Suseptansi (B) Dan Reaktansi (X)?", "id": "B = -1 / X." },
  { "en": "Apa Itu Admitansi (Admittance) (Y)?", "id": "Ukuran Kemudahan Arus AC Mengalir." },
  { "en": "Apa Rumus Admitansi (Admittance) (Y) Kompleks?", "id": "Y = G + jB." },
  { "en": "Apa Itu Faktor Daya (Power Factor) Leading?", "id": "Arus Mendahului Tegangan (Kapasitif)." },
  { "en": "Apa Itu Faktor Daya (Power Factor) Lagging?", "id": "Arus Tertinggal Tegangan (Induktif)." },
  { "en": "Apa Itu Koreksi Faktor Daya (Power Factor Correction)?", "id": "Menambah Komponen Reaktif Kompensasi." },
  { "en": "Komponen Apa Untuk Koreksi Beban Induktif?", "id": "Kapasitor Paralel." },
  { "en": "Apa Manfaat Koreksi Faktor Daya (Power Factor Correction)?", "id": "Mengurangi Arus, Rugi, Denda PLN." },
  { "en": "Apa Itu Resonansi (Resonance) Seri RLC?", "id": "Impedansi Minimum, Arus Maksimum." },
  { "en": "Apa Itu Resonansi (Resonance) Paralel RLC?", "id": "Impedansi Maksimum, Arus Minimum." },
  { "en": "Apa Itu Frekuensi Cutoff (Cutoff Frequency) Filter?", "id": "Frekuensi Batas (-3 dB)." },
  { "en": "Apa Itu Bandwidth (Bandwidth) Filter Bandpass?", "id": "Rentang Frekuensi Antara Cutoff." },
  { "en": "Apa Itu Faktor Kualitas (Q Factor) Filter?", "id": "Ukuran Selektivitas Atau Tajamnya Filter." },
  { "en": "Apa Itu Gelombang Kotak (Square Wave)?", "id": "Sinyal Digital (Level Tinggi, Rendah)." },
  { "en": "Apa Komponen Frekuensi Gelombang Kotak (Square Wave)?", "id": "Fundamental Dan Harmonisa Ganjil." },
  { "en": "Apa Itu Gelombang Segitiga (Triangular Wave)?", "id": "Sinyal Naik Turun Linear Periodik." },
  { "en": "Apa Itu Gelombang Gigi Gergaji (Sawtooth Wave)?", "id": "Sinyal Naik Linear, Turun Cepat." },
  { "en": "Apa Itu Siklus Kerja (Duty Cycle)?", "id": "Persentase Waktu Sinyal Tinggi (On)." },
  { "en": "Berapa Siklus Kerja (Duty Cycle) Gelombang Kotak Simetris?", "id": "Lima Puluh Persen (50%)." },
  { "en": "Apa Itu Modulasi Lebar Pulsa (PWM)?", "id": "Pulse Width Modulation (PWM)." },
  { "en": "Apa Fungsi PWM (Pulse Width Modulation)?", "id": "Mengatur Daya Efektif Beban DC." },
  { "en": "Apa Itu Dioda Bridge (Bridge Rectifier)?", "id": "Empat Dioda Penyearah Gelombang Penuh." },
  { "en": "Apa Itu Filter Kapasitor (Capacitor Filter)?", "id": "Menghaluskan Tegangan DC Setelah Penyearah." },
  { "en": "Apa Itu Tegangan Riak (Ripple Voltage)?", "id": "Sisa Variasi AC Pada Output DC." },
  { "en": "Apa Itu Regulator Tegangan (Voltage Regulator)?", "id": "Menjaga Tegangan Output DC Stabil." },
  { "en": "Contoh IC (Integrated Circuit) Regulator Tegangan Linear?", "id": "Seri 78xx (Positif), 79xx (Negatif)." },
  { "en": "Apa Itu Regulator Switching (Switching Regulator)?", "id": "Regulator Efisiensi Tinggi (SMPS)." },
  { "en": "Apa Itu Konverter Buck (Buck Converter)?", "id": "Regulator Switching Penurun Tegangan (Step-Down)." },
  { "en": "Apa Itu Konverter Boost (Boost Converter)?", "id": "Regulator Switching Penaik Tegangan (Step-Up)." },
  { "en": "Apa Itu Konverter Buck-Boost (Buck-Boost Converter)?", "id": "Regulator Switching Penaik/Penurun (Inverting)." },
  { "en": "Apa Itu Isolasi Galvanik (Galvanic Isolation)?", "id": "Memisahkan Sirkuit Tanpa Koneksi Langsung." },
  { "en": "Bagaimana Isolasi (Isolation) Dicapai Catu Daya?", "id": "Menggunakan Transformator Frekuensi Tinggi." },
  { "en": "Apa Itu Optocoupler (Optocoupler)?", "id": "Isolator Sinyal Menggunakan Cahaya." },
  { "en": "Apa Fungsi Optocoupler (Optocoupler) Di SMPS?", "id": "Isolasi Sinyal Umpan Balik (Feedback)." },
  { "en": "Apa Itu Transistor BJT (Bipolar Junction Transistor) NPN?", "id": "Lapisan P Diapit Dua N." },
  { "en": "Apa Itu Transistor BJT (Bipolar Junction Transistor) PNP?", "id": "Lapisan N Diapit Dua P." },
  { "en": "Apa Daerah Operasi (Operating Region) BJT?", "id": "Cut-Off, Aktif, Saturasi." },
  { "en": "Kapan BJT (Bipolar Junction Transistor) Sebagai Saklar Tertutup (On)?", "id": "Saat Berada Di Daerah Saturasi." },
  { "en": "Kapan BJT (Bipolar Junction Transistor) Sebagai Saklar Terbuka (Off)?", "id": "Saat Berada Di Daerah Cut-Off." },
  { "en": "Kapan BJT (Bipolar Junction Transistor) Sebagai Penguat (Amplifier)?", "id": "Saat Berada Di Daerah Aktif." },
  { "en": "Apa Itu Penguatan Arus (Current Gain) (Beta) BJT?", "id": "Rasio Arus Kolektor Terhadap Basis (Ic/Ib)." },
  { "en": "Apa Itu FET (Field Effect Transistor)?", "id": "Transistor Efek Medan (Terkontrol Tegangan)." },
  { "en": "Apa Keunggulan FET (Field Effect Transistor) Dibanding BJT?", "id": "Impedansi Input Sangat Tinggi." },
  { "en": "Apa Dua Jenis Utama FET (Field Effect Transistor)?", "id": "JFET (Junction FET), MOSFET (Metal-Oxide-Semiconductor FET)." },
  { "en": "Apa Itu MOSFET (Metal-Oxide-Semiconductor FET) Enhancement Mode?", "id": "Perlu Tegangan Gate Untuk Konduksi." },
  { "en": "Apa Itu MOSFET (Metal-Oxide-Semiconductor FET) Depletion Mode?", "id": "Konduksi Tanpa Tegangan Gate." },
  { "en": "Apa Itu CMOS (Complementary Metal-Oxide-Semiconductor)?", "id": "Logika Kombinasi NMOS Dan PMOS." },
  { "en": "Apa Keunggulan Logika CMOS (Complementary Metal-Oxide-Semiconductor)?", "id": "Daya Statis Sangat Rendah." },
  { "en": "Apa Itu Gerbang Logika (Logic Gate) NOT?", "id": "Inverter (Pembalik Logika)." },
  { "en": "Apa Itu Gerbang Logika (Logic Gate) AND?", "id": "Output Tinggi Jika Semua Input Tinggi." },
  { "en": "Apa Itu Gerbang Logika (Logic Gate) OR?", "id": "Output Tinggi Jika Salah Satu Input Tinggi." },
  { "en": "Apa Itu Gerbang Logika (Logic Gate) NAND?", "id": "Gerbang AND Diikuti NOT." },
  { "en": "Apa Itu Gerbang Logika (Logic Gate) NOR?", "id": "Gerbang OR Diikuti NOT." },
  { "en": "Apa Itu Gerbang Logika (Logic Gate) XOR?", "id": "Output Tinggi Jika Input Berbeda." },
  { "en": "Apa Itu Gerbang Logika (Logic Gate) XNOR?", "id": "Output Tinggi Jika Input Sama." },
  { "en": "Gerbang Apa Yang Disebut Universal Gate?", "id": "Gerbang NAND Dan NOR." },
  { "en": "Apa Itu Aljabar Boolean (Boolean Algebra)?", "id": "Matematika Logika (Operasi AND, OR, NOT)." },
  { "en": "Apa Itu Tabel Kebenaran (Truth Table)?", "id": "Tabel Output Berdasarkan Kombinasi Input." },
  { "en": "Apa Itu Flip-Flop (Flip-Flop)?", "id": "Elemen Memori Penyimpan Satu Bit." },
  { "en": "Apa Beda Latch (Latch) Dan Flip-Flop (Flip-Flop)?", "id": "Latch (Level), Flip-Flop (Edge Triggered)." },
  { "en": "Apa Itu Register (Register) Digital?", "id": "Kumpulan Flip-Flop Penyimpan Data." },
  { "en": "Apa Itu Counter (Pencacah) Digital?", "id": "Rangkaian Pencacah Urutan Pulsa Clock." },
  { "en": "Apa Itu Multiplexer (Multiplexer) (MUX)?", "id": "Memilih Satu Input Ke Output Tunggal." },
  { "en": "Apa Itu Demultiplexer (Demultiplexer) (DEMUX)?", "id": "Mengarahkan Input Ke Banyak Output." },
  { "en": "Apa Itu Encoder (Encoder)?", "id": "Mengubah Input Aktif Menjadi Kode Biner." },
  { "en": "Apa Itu Decoder (Decoder)?", "id": "Mengubah Kode Biner Menjadi Output Aktif." },
  { "en": "Apa Itu Osilator (Oscillator)?", "id": "Rangkaian Penghasil Sinyal Periodik." },
  { "en": "Apa Itu Osilator (Oscillator) Kristal?", "id": "Osilator Frekuensi Sangat Stabil." },
  { "en": "Apa Bahan Kristal Osilator (Crystal Oscillator)?", "id": "Kuarsa (Quartz) Piezoelektrik." },
  { "en": "Apa Itu IC (Integrated Circuit) Timer 555?", "id": "IC (Integrated Circuit) Serbaguna (Timer, Osilator)." },
  { "en": "Apa Mode Operasi IC (Integrated Circuit) 555?", "id": "Astabil, Monostabil, Bistabil." },
  { "en": "Apa Itu Mode Astabil (Astable Mode) 555?", "id": "Sebagai Osilator Gelombang Kotak." },
  { "en": "Apa Itu Mode Monostabil (Monostable Mode) 555?", "id": "Sebagai Timer Satu Pulsa." },
  { "en": "Apa Itu Penguat Operasional (Op-Amp)?", "id": "Operational Amplifier (Op-Amp)." },
  { "en": "Apa Karakteristik Op-Amp (Operational Amplifier) Ideal?", "id": "Gain Tak Terhingga, Impedansi Input Tak Terhingga." },
  { "en": "Apa Itu Penguatan Loop Terbuka (Open-Loop Gain)?", "id": "Penguatan Op-Amp (Operational Amplifier) Tanpa Umpan Balik." },
  { "en": "Apa Itu Penguatan Loop Tertutup (Closed-Loop Gain)?", "id": "Penguatan Op-Amp (Operational Amplifier) Dengan Umpan Balik." },
  { "en": "Apa Itu Umpan Balik Negatif (Negative Feedback)?", "id": "Menstabilkan Penguatan Op-Amp." },
  { "en": "Apa Itu Penguat Inverting (Inverting Amplifier)?", "id": "Op-Amp (Operational Amplifier) Membalik Fasa Output." },
  { "en": "Apa Itu Penguat Non-Inverting (Non-Inverting Amplifier)?", "id": "Op-Amp (Operational Amplifier) Fasa Output Sama." },
  { "en": "Apa Itu Impedansi Input (Input Impedance) Op-Amp?", "id": "Hambatan Dilihat Dari Terminal Input." },
  { "en": "Apa Itu Impedansi Output (Output Impedance) Op-Amp?", "id": "Hambatan Dilihat Dari Terminal Output." },
  { "en": "Apa Itu Bandwidth (Bandwidth) Op-Amp?", "id": "Rentang Frekuensi Operasi Efektif." },
  { "en": "Apa Itu Gain-Bandwidth Product (GBP) Op-Amp?", "id": "Hasil Kali Gain Loop Tertutup, Bandwidth." },
  { "en": "Apa Itu Slew Rate (Slew Rate) Op-Amp?", "id": "Laju Perubahan Tegangan Output Maksimal." },
  { "en": "Apa Itu Tegangan Offset Input (Input Offset Voltage)?", "id": "Tegangan Input Agar Output Nol." },
  { "en": "Apa Itu Arus Bias Input (Input Bias Current)?", "id": "Arus DC Kecil Masuk Terminal Input." },
  { "en": "Apa Itu Common-Mode Rejection Ratio (CMRR)?", "id": "Kemampuan Menolak Sinyal Mode Bersama." },
  { "en": "Apa Itu Power Supply Rejection Ratio (PSRR)?", "id": "Kemampuan Menolak Noise Catu Daya." },
  { "en": "Apa Itu Komparator (Comparator)?", "id": "Op-Amp (Operational Amplifier) Tanpa Umpan Balik (Pembanding)." },
  { "en": "Apa Itu Schmitt Trigger (Schmitt Trigger)?", "id": "Komparator (Comparator) Dengan Histeresis." },
  { "en": "Apa Fungsi Histeresis (Hysteresis) Schmitt Trigger?", "id": "Mencegah Osilasi Output Dekat Threshold." },
  { "en": "Apa Itu Filter Aktif (Active Filter)?", "id": "Filter Menggunakan Op-Amp (Bisa Gain)." },
  { "en": "Apa Beda Filter Aktif (Active Filter), Pasif (Passive)?", "id": "Aktif (Ada Op-Amp), Pasif (R, L, C)." },
  { "en": "Apa Itu Antena (Antenna)?", "id": "Konverter Gelombang Elektromagnetik, Listrik." },
  { "en": "Apa Itu Pola Radiasi (Radiation Pattern) Antena?", "id": "Grafik Arah Pancaran Energi Antena." },
  { "en": "Apa Itu Penguatan (Gain) Antena?", "id": "Ukuran Keterarahan Pancaran Antena." },
  { "en": "Apa Itu Impedansi (Impedance) Antena?", "id": "Impedansi Input Terminal Antena." },
  { "en": "Apa Itu Bandwidth (Bandwidth) Antena?", "id": "Rentang Frekuensi Kerja Efektif Antena." },
  { "en": "Apa Itu Polarisasi (Polarization) Antena?", "id": "Orientasi Medan Listrik Gelombang Radio." },
  { "en": "Apa Itu VSWR (Voltage Standing Wave Ratio)?", "id": "Rasio Gelombang Tegangan Berdiri." },
  { "en": "Apa Kepanjangan VSWR (Voltage Standing Wave Ratio)?", "id": "Rasio Gelombang Tegangan Berdiri." },
  { "en": "Apa Nilai VSWR (Voltage Standing Wave Ratio) Ideal?", "id": "Satu Banding Satu (1:1)." },
  { "en": "Apa Penyebab VSWR (Voltage Standing Wave Ratio) Buruk?", "id": "Ketidakcocokan Impedansi (Mismatch)." },
  { "en": "Apa Itu Pencocokan Impedansi (Impedance Matching)?", "id": "Menyamakan Impedansi Sumber, Beban, Saluran." },
  { "en": "Apa Itu Kabel Balun (Balun Cable)?", "id": "Transformator Impedansi (Balanced Ke Unbalanced)." },
  { "en": "Kapan Kabel Balun (Balun Cable) Digunakan?", "id": "Menghubungkan Antena Dipol Ke Koaksial." },
  { "en": "Apa Itu Saluran Transmisi (Transmission Line) Seimbang?", "id": "Dua Konduktor Simetris Terhadap Ground." },
  { "en": "Contoh Saluran Transmisi (Transmission Line) Seimbang?", "id": "Kabel Twisted Pair, Twin-Lead." },
  { "en": "Apa Itu Saluran Transmisi (Transmission Line) Tidak Seimbang?", "id": "Satu Konduktor, Ground Kembali." },
  { "en": "Contoh Saluran Transmisi (Transmission Line) Tidak Seimbang?", "id": "Kabel Koaksial, Microstrip." },
  { "en": "Apa Itu Kabel Twin-Lead (Twin-Lead Cable)?", "id": "Dua Konduktor Paralel Terisolasi Plastik." },
  { "en": "Impedansi (Impedance) Kabel Twin-Lead (Twin-Lead Cable) TV Lama?", "id": "Sekitar 300 Ohm." },
  { "en": "Apa Itu Modulasi Amplitudo (Amplitude Modulation) (AM)?", "id": "Amplitudo Carrier Diubah Sesuai Informasi." },
  { "en": "Apa Itu Modulasi Frekuensi (Frequency Modulation) (FM)?", "id": "Frekuensi Carrier Diubah Sesuai Informasi." },
  { "en": "Apa Itu Modulasi Fasa (Phase Modulation) (PM)?", "id": "Fasa Carrier Diubah Sesuai Informasi." },
  { "en": "Mana Yang Lebih Kebal Noise, AM Atau FM?", "id": "FM (Frequency Modulation) Lebih Kebal Noise." },
  { "en": "Apa Itu Indeks Modulasi (Modulation Index) AM?", "id": "Ukuran Kedalaman Modulasi Amplitudo." },
  { "en": "Apa Itu Deviasi Frekuensi (Frequency Deviation) FM?", "id": "Pergeseran Frekuensi Maksimum Carrier." },
  { "en": "Apa Itu Bandwidth (Bandwidth) Sinyal FM (Frequency Modulation)?", "id": "Diperkirakan Dengan Aturan Carson." },
  { "en": "Apa Itu Pre-Emphasis (Pre-Emphasis) Dan De-Emphasis (De-Emphasis)?", "id": "Teknik Mengurangi Noise Sinyal FM." },
  { "en": "Apa Itu Penerima Superheterodyne (Superheterodyne Receiver)?", "id": "Arsitektur Penerima Radio Paling Umum." },
  { "en": "Apa Keuntungan Penerima Superheterodyne (Superheterodyne Receiver)?", "id": "Selektivitas, Sensitivitas Tinggi Konsisten." },
  { "en": "Apa Itu Frekuensi Gambar (Image Frequency)?", "id": "Frekuensi Interferensi Penerima Superheterodyne." },
  { "en": "Bagaimana Menekan Frekuensi Gambar (Image Frequency)?", "id": "Menggunakan Filter RF (Frekuensi Radio) Selektif." },
  { "en": "Apa Itu Pengatur Penguatan Otomatis (AGC)?", "id": "Automatic Gain Control (AGC)." },
  { "en": "Apa Kepanjangan AGC (Automatic Gain Control)?", "id": "Pengatur Penguatan Otomatis." },
  { "en": "Apa Fungsi AGC (Automatic Gain Control)?", "id": "Menjaga Level Output Penerima Konstan." },
  { "en": "Apa Itu Modulasi Digital (Digital Modulation)?", "id": "Merepresentasikan Data Digital Ke Sinyal Analog." },
  { "en": "Apa Itu ASK (Amplitude Shift Keying)?", "id": "Modulasi Digital Berbasis Amplitudo." },
  { "en": "Apa Itu FSK (Frequency Shift Keying)?", "id": "Modulasi Digital Berbasis Frekuensi." },
  { "en": "Apa Itu PSK (Phase Shift Keying)?", "id": "Modulasi Digital Berbasis Fasa." },
  { "en": "Apa Itu BPSK (Binary Phase Shift Keying)?", "id": "PSK (Phase Shift Keying) Dua Fasa (1 Bit)." },
  { "en": "Apa Itu QPSK (Quadrature Phase Shift Keying)?", "id": "PSK (Phase Shift Keying) Empat Fasa (2 Bit)." },
  { "en": "Apa Itu QAM (Quadrature Amplitude Modulation)?", "id": "Modulasi Digital (Amplitudo Dan Fasa)." },
  { "en": "Apa Itu Diagram Konstelasi (Constellation Diagram)?", "id": "Representasi Grafis Titik Simbol Modulasi." },
  { "en": "Apa Itu Noise (Derau) Aditif Putih Gaussian?", "id": "Additive White Gaussian Noise (AWGN)." },
  { "en": "Apa Kepanjangan AWGN (Additive White Gaussian Noise)?", "id": "Derau Aditif Putih Gaussian." },
  { "en": "Model Kanal Apa Paling Sederhana?", "id": "Kanal AWGN (Additive White Gaussian Noise)." },
  { "en": "Apa Itu Rasio Sinyal Terhadap Derau (SNR)?", "id": "Signal-to-Noise Ratio (SNR)." },
  { "en": "Apa Kepanjangan SNR (Signal-to-Noise Ratio)?", "id": "Rasio Sinyal Terhadap Derau." },
  { "en": "Apakah SNR (Signal-to-Noise Ratio) Tinggi Diinginkan?", "id": "Ya, Menunjukkan Kualitas Sinyal Baik." },
  { "en": "Apa Itu Batas Shannon (Shannon Limit)?", "id": "Batas Kapasitas Kanal Teoretis Maksimum." },
  { "en": "Apa Itu Laju Kesalahan Bit (BER)?", "id": "Bit Error Rate (BER)." },
  { "en": "Apa Kepanjangan BER (Bit Error Rate)?", "id": "Laju Kesalahan Bit." },
  { "en": "Apa Itu Pengkodean Kanal (Channel Coding)?", "id": "Menambah Redundansi Deteksi/Koreksi Error." },
  { "en": "Apa Itu Kode Blok (Block Code)?", "id": "Jenis Kode Kanal (Data Dibagi Blok)." },
  { "en": "Apa Itu Kode Konvolusi (Convolutional Code)?", "id": "Jenis Kode Kanal (Berbasis Jendela Geser)." },
  { "en": "Apa Itu Dekoder Viterbi (Viterbi Decoder)?", "id": "Algoritma Dekode Kode Konvolusi Optimal." },
  { "en": "Apa Itu Kode Reed-Solomon (Reed-Solomon Code)?", "id": "Kode Blok Kuat Melawan Burst Error." },
  { "en": "Apa Itu Interleaving (Interleaving)?", "id": "Mengacak Urutan Data (Sebar Burst Error)." },
  { "en": "Apa Itu Multipath (Multipath) Kanal Nirkabel?", "id": "Sinyal Diterima Lewat Banyak Lintasan." },
  { "en": "Apa Akibat Multipath (Multipath)?", "id": "Fading (Redaman) Dan Interferensi Antar Simbol." },
  { "en": "Apa Itu Interferensi Antar Simbol (ISI)?", "id": "Inter-Symbol Interference (ISI)." },
  { "en": "Apa Kepanjangan ISI (Inter-Symbol Interference)?", "id": "Interferensi Antar Simbol." },
  { "en": "Bagaimana Mengatasi ISI (Inter-Symbol Interference)?", "id": "Equalizer (Penyetara) Kanal." },
  { "en": "Apa Itu Equalizer (Penyetara) Kanal?", "id": "Filter Kompensasi Distorsi Kanal." },
  { "en": "Apa Itu Waktu Koherensi (Coherence Time) Kanal?", "id": "Durasi Kanal Dianggap Tetap." },
  { "en": "Apa Itu Bandwidth Koherensi (Coherence Bandwidth) Kanal?", "id": "Rentang Frekuensi Respon Kanal Mirip." },
  { "en": "Apa Itu Fading Cepat (Fast Fading)?", "id": "Waktu Koherensi Pendek Dari Simbol." },
  { "en": "Apa Itu Fading Lambat (Slow Fading)?", "id": "Waktu Koherensi Panjang Dari Simbol." },
  { "en": "Apa Itu Fading Selektif Frekuensi (Frequency Selective)?", "id": "Bandwidth Sinyal Lebih Besar Bandwidth Koherensi." },
  { "en": "Apa Itu Fading Datar (Flat Fading)?", "id": "Bandwidth Sinyal Lebih Kecil Bandwidth Koherensi." },
  { "en": "Apa Itu Keanekaragaman (Diversity) Komunikasi?", "id": "Teknik Mitigasi Efek Fading." },
  { "en": "Sebutkan Jenis Keanekaragaman (Diversity)?", "id": "Waktu, Frekuensi, Spasial, Polarisasi." },
  { "en": "Apa Itu MIMO (Multiple Input Multiple Output)?", "id": "Menggunakan Banyak Antena (Transmit, Receive)." },
  { "en": "Apa Keuntungan MIMO (Multiple Input Multiple Output)?", "id": "Keanekaragaman, Multiplexing Spasial (Kapasitas)." },
  { "en": "Apa Itu Multiplexing Spasial (Spatial Multiplexing)?", "id": "Mengirim Stream Data Berbeda Bersamaan." },
  { "en": "Apa Itu Beamforming (Pembentukan Berkas)?", "id": "Mengarahkan Energi Sinyal Ke Arah Tertentu." },
  { "en": "Apa Itu Seluler (Cellular) Komunikasi?", "id": "Area Dilayani Stasiun Pangkalan (Sel)." },
  { "en": "Apa Itu Handoff (Handoff) Seluler?", "id": "Perpindahan Koneksi Antar Sel." },
  { "en": "Apa Itu Interferensi Antar Sel (Inter-Cell Interference)?", "id": "Gangguan Sinyal Dari Sel Tetangga." },
  { "en": "Apa Itu Penggunaan Ulang Frekuensi (Frequency Reuse)?", "id": "Menggunakan Frekuensi Sama Sel Berjauhan." },
  { "en": "Apa Itu Sektor Antena (Antenna Sectoring)?", "id": "Membagi Sel Menjadi Sektor Arah." },
  { "en": "Apa Kepanjangan GSM (Global System for Mobile Communications)?", "id": "Sistem Global Komunikasi Bergerak (2G)." },
  { "en": "Apa Kepanjangan UMTS (Universal Mobile Telecommunications System)?", "id": "Sistem Telekomunikasi Bergerak Universal (3G)." },
  { "en": "Apa Kepanjangan LTE (Long Term Evolution)?", "id": "Evolusi Jangka Panjang (4G)." },
  { "en": "Apa Itu 5G (Fifth Generation)?", "id": "Generasi Kelima Teknologi Seluler." },
  { "en": "Apa Fitur Utama 5G (Fifth Generation)?", "id": "Kecepatan Tinggi, Latensi Rendah, Konektivitas Masif." },
  { "en": "Apa Itu Gelombang Milimeter (Millimeter Wave) (mmWave)?", "id": "Frekuensi Sangat Tinggi (Digunakan 5G)." },
  { "en": "Apa Kelemahan Gelombang Milimeter (Millimeter Wave) (mmWave)?", "id": "Jangkauan Pendek, Mudah Terhalang." },
  { "en": "Apa Itu Komunikasi Satelit (Satellite Communication)?", "id": "Komunikasi Menggunakan Satelit Orbit." },
  { "en": "Apa Itu Orbit Geostasioner (Geostationary Orbit) (GEO)?", "id": "Orbit Tetap Di Atas Khatulistiwa." },
  { "en": "Apa Itu Orbit Bumi Rendah (Low Earth Orbit) (LEO)?", "id": "Orbit Ketinggian Rendah (Konstelasi Satelit)." },
  { "en": "Apa Itu Orbit Bumi Menengah (Medium Earth Orbit) (MEO)?", "id": "Orbit Antara LEO (Low Earth Orbit), GEO (Geostationary Orbit)." },
  { "en": "Satelit GPS (Global Positioning System) Berada Di Orbit Mana?", "id": "Orbit Bumi Menengah (MEO)." },
  { "en": "Apa Itu Uplink (Uplink) Komunikasi Satelit?", "id": "Sinyal Dari Bumi Ke Satelit." },
  { "en": "Apa Itu Downlink (Downlink) Komunikasi Satelit?", "id": "Sinyal Dari Satelit Ke Bumi." },
  { "en": "Apa Itu Transponder (Transponder) Satelit?", "id": "Penerima, Penguat, Pemancar Di Satelit." },
  { "en": "Apa Itu Jejak Kaki (Footprint) Satelit?", "id": "Area Cakupan Sinyal Satelit Bumi." },
  { "en": "Apa Itu Propagasi (Propagation) Gelombang Radio?", "id": "Perambatan Gelombang Radio Di Atmosfer." },
  { "en": "Apa Itu Gelombang Tanah (Ground Wave)?", "id": "Gelombang Mengikuti Kontur Bumi (Frekuensi Rendah)." },
  { "en": "Apa Itu Gelombang Langit (Sky Wave)?", "id": "Gelombang Dipantulkan Lapisan Ionosfer (HF)." },
  { "en": "Apa Itu Gelombang Garis Pandang (Line-of-Sight)?", "id": "Propagasi Langsung (Frekuensi Tinggi)." },
  { "en": "Apa Itu Fading Hujan (Rain Fade)?", "id": "Atenuasi Sinyal Frekuensi Tinggi Hujan." },
  { "en": "Apa Itu Indeks Bias (Refractive Index) Atmosfer?", "id": "Mempengaruhi Pembelokan Gelombang Radio." },
  { "en": "Apa Itu Ducting (Ducting) Atmosfer?", "id": "Gelombang Terperangkap Lapisan Udara (Jangkauan Jauh)." },
  { "en": "Apa Itu Scintillation (Scintillation) Ionosfer?", "id": "Fluktuasi Cepat Sinyal Lewati Ionosfer." },
  { "en": "Apa Itu Penguat Umpan Balik (Feedback Amplifier) Seri-Shunt?", "id": "Input Seri, Output Shunt." },
  { "en": "Topologi Umpan Balik (Feedback Topology) Apa Nama Lainnya?", "id": "Penguat Transresistansi (Transresistance Amplifier)." },
  { "en": "Apa Itu Penguat Umpan Balik (Feedback Amplifier) Shunt-Seri?", "id": "Input Shunt, Output Seri." },
  { "en": "Topologi Umpan Balik (Feedback Topology) Shunt-Seri Nama Lainnya?", "id": "Penguat Transkonduktansi (Transconductance Amplifier)." },
  { "en": "Apa Itu Penguat Umpan Balik (Feedback Amplifier) Seri-Seri?", "id": "Input Seri, Output Seri." },
  { "en": "Topologi Umpan Balik (Feedback Topology) Seri-Seri Nama Lainnya?", "id": "Penguat Arus (Current Amplifier)." },
  { "en": "Apa Itu Penguat Umpan Balik (Feedback Amplifier) Shunt-Shunt?", "id": "Input Shunt, Output Shunt." },
  { "en": "Topologi Umpan Balik (Feedback Topology) Shunt-Shunt Nama Lainnya?", "id": "Penguat Tegangan (Voltage Amplifier)." },
  { "en": "Apa Itu Kriteria Kestabilan Barkhausen (Barkhausen Criterion)?", "id": "Syarat Terjadinya Osilasi." },
  { "en": "Apa Dua Syarat Kriteria Barkhausen (Barkhausen Criterion)?", "id": "Penguatan Loop Satu, Fasa Nol." },
  { "en": "Apa Itu Penguat Operasional (Op-Amp) Ideal?", "id": "Penguat Sempurna Tanpa Keterbatasan." },
  { "en": "Berapa Impedansi Input (Input Impedance) Op-Amp Ideal?", "id": "Tak Terhingga." },
  { "en": "Berapa Impedansi Output (Output Impedance) Op-Amp Ideal?", "id": "Nol." },
  { "en": "Berapa Penguatan Loop Terbuka (Open-Loop Gain) Op-Amp Ideal?", "id": "Tak Terhingga." },
  { "en": "Berapa Bandwidth (Bandwidth) Op-Amp Ideal?", "id": "Tak Terhingga." },
  { "en": "Berapa Tegangan Offset (Offset Voltage) Op-Amp Ideal?", "id": "Nol." },
  { "en": "Berapa Arus Bias (Bias Current) Op-Amp Ideal?", "id": "Nol." },
  { "en": "Berapa CMRR (Common Mode Rejection Ratio) Op-Amp Ideal?", "id": "Tak Terhingga." },
  { "en": "Berapa Slew Rate (Slew Rate) Op-Amp Ideal?", "id": "Tak Terhingga." },
  { "en": "Apa Itu Dioda Terobosan Balik (Breakdown Diode)?", "id": "Dioda Dirancang Operasi Daerah Breakdown." },
  { "en": "Contoh Dioda Terobosan Balik (Breakdown Diode)?", "id": "Dioda Zener, Dioda Avalanche." },
  { "en": "Apa Itu Koefisien Suhu (Temperature Coefficient) Dioda Zener?", "id": "Perubahan Tegangan Zener Terhadap Suhu." },
  { "en": "Apa Itu Resistansi Dinamis (Dynamic Resistance) Zener?", "id": "Hambatan Internal Dioda Zener." },
  { "en": "Apa Itu Arus Lutut (Knee Current) Zener?", "id": "Arus Minimum Agar Zener Stabil." },
  { "en": "Apa Itu Dioda Schottky (Schottky Diode) Barrier?", "id": "Sambungan Logam-Semikonduktor." },
  { "en": "Apa Keunggulan Sambungan Logam-Semikonduktor?", "id": "Tegangan Maju Rendah, Switching Cepat." },
  { "en": "Apa Itu Emitor (Emitter) BJT (Bipolar Junction Transistor)?", "id": "Daerah Doping Tinggi (Sumber Pembawa Muatan)." },
  { "en": "Apa Itu Basis (Base) BJT (Bipolar Junction Transistor)?", "id": "Daerah Tipis Pengontrol Arus Kolektor." },
  { "en": "Apa Itu Kolektor (Collector) BJT (Bipolar Junction Transistor)?", "id": "Daerah Pengumpul Pembawa Muatan." },
  { "en": "Mengapa Basis (Base) BJT (Bipolar Junction Transistor) Sangat Tipis?", "id": "Agar Rekombinasi Minimal (Beta Tinggi)." },
  { "en": "Apa Itu Efek Early (Early Effect)?", "id": "Modulasi Lebar Basis Efektif BJT." },
  { "en": "Apa Akibat Efek Early (Early Effect)?", "id": "Meningkatkan Arus Kolektor (Resistansi Output)." },
  { "en": "Apa Itu Tegangan Early (Early Voltage) (VA)?", "id": "Parameter Karakteristik Efek Early." },
  { "en": "Apa Itu Model Ebers-Moll (Ebers-Moll Model)?", "id": "Model Fisik Transistor BJT." },
  { "en": "Apa Itu Model Hybrid-Pi (Hybrid-Pi Model)?", "id": "Model Sinyal Kecil Transistor BJT." },
  { "en": "Apa Itu Frekuensi Transisi (Transition Frequency) (fT)?", "id": "Frekuensi Saat Beta (β) Turun Jadi Satu." },
  { "en": "Apa Makna Frekuensi Transisi (Transition Frequency) (fT)?", "id": "Ukuran Batas Kecepatan Transistor." },
  { "en": "Apa Itu Miller Capacitance (Kapasitansi Miller)?", "id": "Kapasitansi Efektif Akibat Efek Miller." },
  { "en": "Apa Itu Source (Source) FET (Field Effect Transistor)?", "id": "Terminal Sumber Pembawa Muatan." },
  { "en": "Apa Itu Gate (Gerbang) FET (Field Effect Transistor)?", "id": "Terminal Pengontrol Konduktivitas Saluran." },
  { "en": "Apa Itu Drain (Saluran) FET (Field Effect Transistor)?", "id": "Terminal Pengumpul Pembawa Muatan." },
  { "en": "Apa Itu Substrat (Substrate) MOSFET?", "id": "Badan (Body) Semikonduktor MOSFET." },
  { "en": "Apa Itu Tegangan Threshold (Threshold Voltage) (Vt)?", "id": "Tegangan Gate Minimum Aktifkan MOSFET." },
  { "en": "Apa Itu Modulasi Panjang Saluran (Channel Length Modulation)?", "id": "Efek Mirip Efek Early Di MOSFET." },
  { "en": "Apa Itu Parameter Lambda (λ) MOSFET?", "id": "Parameter Modulasi Panjang Saluran." },
  { "en": "Apa Itu Transkonduktansi (Transconductance) (gm)?", "id": "Perubahan Arus Drain Terhadap Gate." },
  { "en": "Apa Satuan Transkonduktansi (Transconductance) (gm)?", "id": "Siemens (S) Atau Ampere/Volt." },
  { "en": "Apa Itu Resistansi Output (Output Resistance) (ro) FET?", "id": "Hambatan Output Transistor (Efek Modulasi)." },
  { "en": "Apa Itu Memori (Memory) Semikonduktor?", "id": "Perangkat Penyimpan Data Berbasis Semikonduktor." },
  { "en": "Apa Itu Sel Memori (Memory Cell) 1T1C DRAM?", "id": "Satu Transistor, Satu Kapasitor." },
  { "en": "Apa Itu Sel Memori (Memory Cell) 6T SRAM?", "id": "Enam Transistor (Flip-Flop)." },
  { "en": "Apa Itu Floating Gate (Gerbang Mengambang) Memori Flash?", "id": "Gerbang Terisolasi Penyimpan Muatan (Data)." },
  { "en": "Apa Itu Operasi Tulis (Write) Memori Flash?", "id": "Menginjeksikan Elektron Ke Floating Gate." },
  { "en": "Apa Itu Operasi Hapus (Erase) Memori Flash?", "id": "Mengeluarkan Elektron Dari Floating Gate." },
  { "en": "Apa Itu Wear Leveling (Perataan Keausan)?", "id": "Distribusi Siklus Tulis Merata Sel Flash." },
  { "en": "Apa Itu Error Correction Code (ECC) Memori?", "id": "Kode Deteksi, Koreksi Kesalahan Data Memori." },
  { "en": "Apa Itu Multiplexer (Multiplexer) Analog?", "id": "Saklar Elektronik Pilih Sinyal Analog." },
  { "en": "Apa Itu Demultiplexer (Demultiplexer) Analog?", "id": "Saklar Elektronik Arahkan Sinyal Analog." },
  { "en": "Apa Itu Penguat Sampel Tahan (Sample and Hold)?", "id": "Mencuplik, Menahan Tegangan Analog Sesaat." },
  { "en": "Apa Itu Aperture Jitter (Jitter Apertur)?", "id": "Variasi Waktu Pengambilan Sampel (S&H)." },
  { "en": "Apa Itu Konverter Tegangan Ke Frekuensi (VFC)?", "id": "Voltage-to-Frequency Converter (VFC)." },
  { "en": "Apa Kepanjangan VFC (Voltage-to-Frequency Converter)?", "id": "Konverter Tegangan Ke Frekuensi." },
  { "en": "Apa Itu Konverter Frekuensi Ke Tegangan (FVC)?", "id": "Frequency-to-Voltage Converter (FVC)." },
  { "en": "Apa Kepanjangan FVC (Frequency-to-Voltage Converter)?", "id": "Konverter Frekuensi Ke Tegangan." },
  { "en": "Apa Itu Sensor Efek Seebeck (Seebeck Effect)?", "id": "Termokopel (Thermocouple)." },
  { "en": "Apa Itu Sensor Efek Hall (Hall Effect)?", "id": "Sensor Medan Magnet, Posisi, Arus." },
  { "en": "Apa Itu Sensor Efek Piezoelektrik (Piezoelectric Effect)?", "id": "Sensor Tekanan, Getaran, Akselerasi." },
  { "en": "Apa Itu Sensor Kapasitif (Capacitive Sensor)?", "id": "Sensor Berbasis Perubahan Kapasitansi." },
  { "en": "Contoh Sensor Kapasitif (Capacitive Sensor)?", "id": "Sensor Jarak, Kelembaban, Level Cairan." },
  { "en": "Apa Itu Sensor Induktif (Inductive Sensor)?", "id": "Sensor Berbasis Perubahan Induktansi." },
  { "en": "Contoh Sensor Induktif (Inductive Sensor)?", "id": "Sensor Jarak Logam (Proximity)." },
  { "en": "Apa Itu Sensor Resistif (Resistive Sensor)?", "id": "Sensor Berbasis Perubahan Resistansi." },
  { "en": "Contoh Sensor Resistif (Resistive Sensor)?", "id": "Potensiometer, Termistor, Strain Gauge, LDR." },
  { "en": "Apa Itu MEMS (Micro-Electro-Mechanical System)?", "id": "Sistem Mikro Elektro Mekanis." },
  { "en": "Apa Kepanjangan MEMS (Micro-Electro-Mechanical System)?", "id": "Sistem Mikro Elektro Mekanis." },
  { "en": "Contoh Sensor MEMS (Micro-Electro-Mechanical System)?", "id": "Akselerometer, Giroskop Di Ponsel." },
  { "en": "Apa Itu Akuisisi Data (Data Acquisition) (DAQ)?", "id": "Proses Pengumpulan Data Dunia Nyata." },
  { "en": "Apa Komponen Sistem DAQ (Data Acquisition System)?", "id": "Sensor, Pengkondisi Sinyal, ADC, Komputer." },
  { "en": "Apa Itu Resolusi (Resolution) Sistem DAQ?", "id": "Perubahan Terkecil Dapat Diukur." },
  { "en": "Apa Itu Akurasi (Accuracy) Sistem DAQ?", "id": "Kedekatan Pengukuran Nilai Sebenarnya." },
  { "en": "Apa Itu Laju Sampel (Sample Rate) DAQ?", "id": "Seberapa Cepat Data Dicuplik." },
  { "en": "Apa Itu Multiplexing (Multipleksasi) Kanal DAQ?", "id": "Berbagi ADC (Analog-to-Digital Converter) Banyak Kanal Input." },
  { "en": "Apa Itu Pemrosesan Sinyal Digital (DSP)?", "id": "Digital Signal Processing (DSP)." },
  { "en": "Apa Kepanjangan DSP (Digital Signal Processing)?", "id": "Pemrosesan Sinyal Digital." },
  { "en": "Apa Fungsi DSP (Digital Signal Processing)?", "id": "Memanipulasi Sinyal Digital (Filter, Analisis)." },
  { "en": "Apa Itu Prosesor DSP (Digital Signal Processor)?", "id": "Mikroprosesor Khusus Operasi DSP." },
  { "en": "Operasi Apa Yang Cepat Di DSP?", "id": "Perkalian-Akumulasi (Multiply-Accumulate, MAC)." },
  { "en": "Apa Kepanjangan MAC (Multiply-Accumulate)?", "id": "Perkalian-Akumulasi." },
  { "en": "Apa Itu Filter Digital (Digital Filter) FIR?", "id": "Finite Impulse Response (Respon Impuls Terbatas)." },
  { "en": "Apa Itu Filter Digital (Digital Filter) IIR?", "id": "Infinite Impulse Response (Respon Impuls Tak Terbatas)." },
  { "en": "Apa Itu Transformasi Fourier Cepat (FFT)?", "id": "Algoritma Efisien Hitung Spektrum Frekuensi." },
  { "en": "Apa Itu Efek Jendela (Windowing Effect) FFT?", "id": "Mengurangi Kebocoran Spektral Analisis FFT." },
  { "en": "Apa Itu Desimasi (Decimation)?", "id": "Mengurangi Laju Sampel Sinyal Digital." },
  { "en": "Apa Itu Interpolasi (Interpolation)?", "id": "Meningkatkan Laju Sampel Sinyal Digital." },
  { "en": "Apa Itu Modulasi Delta (Delta Modulation)?", "id": "Teknik Konversi Analog Ke Digital Sederhana." },
  { "en": "Apa Itu Modulasi Sigma-Delta (Sigma-Delta Modulation)?", "id": "Teknik Oversampling ADC/DAC Resolusi Tinggi." },
  { "en": "Apa Itu Oversampling (Oversampling)?", "id": "Mengambil Sampel Jauh Di Atas Nyquist." },
  { "en": "Apa Itu Pembentukan Derau (Noise Shaping)?", "id": "Memindahkan Error Kuantisasi Ke Frekuensi Tinggi." },
  { "en": "Apa Itu Kodek (Codec) Audio?", "id": "Coder-Decoder (Kompresi, Dekompresi Audio)." },
  { "en": "Contoh Kodek (Codec) Audio Lossy?", "id": "MP3 (MPEG Audio Layer III), AAC (Advanced Audio Coding)." },
  { "en": "Contoh Kodek (Codec) Audio Lossless?", "id": "FLAC (Free Lossless Audio Codec), ALAC (Apple Lossless Audio Codec)." },
  { "en": "Apa Itu Kompresi Lossy (Lossy Compression)?", "id": "Kompresi Data (Ada Informasi Hilang)." },
  { "en": "Apa Itu Kompresi Lossless (Lossless Compression)?", "id": "Kompresi Data (Tanpa Informasi Hilang)." },
  { "en": "Apa Itu JFET (Junction Field Effect Transistor) Kanal-N?", "id": "Saluran Konduksi Dibentuk Elektron." },
  { "en": "Apa Itu JFET (Junction Field Effect Transistor) Kanal-P?", "id": "Saluran Konduksi Dibentuk Hole." },
  { "en": "Bagaimana Bias Gerbang (Gate) JFET Normalnya?", "id": "Diberi Bias Mundur (Reverse Bias)." },
  { "en": "Apa Itu Arus Drain Saturasi (IDSS) JFET?", "id": "Arus Drain Maksimal (Vgs=0)." },
  { "en": "Apa Kepanjangan IDSS (Drain Current at Saturation)?", "id": "Arus Drain Pada Saturasi." },
  { "en": "Apa Itu Tegangan Pinch-Off (Vp) JFET?", "id": "Tegangan Gate Mematikan Saluran." },
  { "en": "Apa Itu Penguat Common Source (Common Source) JFET?", "id": "Konfigurasi Penguat FET (Field Effect Transistor) Umum." },
  { "en": "Apa Karakteristik Penguat Common Source (Common Source)?", "id": "Penguatan Tegangan Tinggi, Fasa Terbalik." },
  { "en": "Apa Itu Penguat Common Drain (Common Drain) JFET?", "id": "Konfigurasi Pengikut Source (Source Follower)." },
  { "en": "Apa Nama Lain Penguat Common Drain (Common Drain)?", "id": "Pengikut Source (Source Follower)." },
  { "en": "Apa Karakteristik Penguat Common Drain (Common Drain)?", "id": "Penguatan Tegangan Hampir Satu (Buffer)." },
  { "en": "Apa Itu Penguat Common Gate (Common Gate) JFET?", "id": "Konfigurasi Impedansi Input Rendah." },
  { "en": "Apa Itu MOSFET (Metal-Oxide-Semiconductor FET) Mode Peningkatan?", "id": "Enhancement Mode (Normal Off)." },
  { "en": "Apa Itu MOSFET (Metal-Oxide-Semiconductor FET) Mode Deplesi?", "id": "Depletion Mode (Normal On)." },
  { "en": "Apa Itu Lapisan Oksida (Oxide Layer) MOSFET?", "id": "Lapisan Isolator Tipis (SiO2)." },
  { "en": "Mengapa Gerbang (Gate) MOSFET (Metal-Oxide-Semiconductor FET) Terisolasi?", "id": "Oleh Lapisan Oksida (Impedansi Tinggi)." },
  { "en": "Apa Itu Tegangan Threshold (Threshold Voltage) (Vt)?", "id": "Tegangan Gate Mulai Konduksi." },
  { "en": "Apa Itu Kapasitansi Gerbang (Gate Capacitance) MOSFET?", "id": "Kapasitansi Parasitik Terminal Gerbang." },
  { "en": "Apa Akibat Kapasitansi Gerbang (Gate Capacitance)?", "id": "Membatasi Kecepatan Switching MOSFET." },
  { "en": "Apa Itu Arus Bocor Gerbang (Gate Leakage Current)?", "id": "Arus Sangat Kecil Lewat Oksida." },
  { "en": "Apa Itu Resistansi On (RDS(on)) MOSFET?", "id": "Resistansi Drain-Source Saat On." },
  { "en": "Apakah RDS(on) (Resistansi On) Rendah Diinginkan?", "id": "Ya, Mengurangi Rugi Konduksi." },
  { "en": "Apa Itu Body Diode (Dioda Badan) MOSFET?", "id": "Dioda Parasitik Internal MOSFET." },
  { "en": "Apa Fungsi Body Diode (Dioda Badan) Terkadang?", "id": "Sebagai Dioda Freewheeling (Aplikasi Tertentu)." },
  { "en": "Apa Itu Thyristor (Thyristor)?", "id": "Keluarga Saklar Semikonduktor Empat Lapis." },
  { "en": "Apa Struktur Dasar Thyristor (Thyristor)?", "id": "Struktur P-N-P-N." },
  { "en": "Sebutkan Anggota Keluarga Thyristor (Thyristor)?", "id": "SCR, TRIAC, DIAC, GTO." },
  { "en": "Apa Kepanjangan SCR (Silicon Controlled Rectifier)?", "id": "Penyearah Silikon Terkendali." },
  { "en": "Apa Fungsi SCR (Silicon Controlled Rectifier)?", "id": "Saklar Daya DC Terkendali Gerbang." },
  { "en": "Bagaimana Cara Menghidupkan SCR (Silicon Controlled Rectifier)?", "id": "Beri Pulsa Positif Ke Gerbang." },
  { "en": "Apa Itu Arus Latching (Latching Current) SCR?", "id": "Arus Anoda Minimum Agar Tetap On." },
  { "en": "Apa Itu Arus Holding (Holding Current) SCR?", "id": "Arus Anoda Minimum Jaga Tetap On." },
  { "en": "Bagaimana Cara Mematikan SCR (Silicon Controlled Rectifier) (Komutasi)?", "id": "Arus Dibawah Holding Atau Tegangan Balik." },
  { "en": "Apa Kepanjangan TRIAC (Triode for Alternating Current)?", "id": "Trioda Untuk Arus Bolak-Balik." },
  { "en": "Apa Fungsi TRIAC (Triode for Alternating Current)?", "id": "Saklar Daya AC Dua Arah." },
  { "en": "Bagaimana Cara Memicu (Trigger) TRIAC?", "id": "Pulsa Gerbang (Positif/Negatif)." },
  { "en": "Apa Kepanjangan DIAC (Diode for Alternating Current)?", "id": "Dioda Untuk Arus Bolak-Balik." },
  { "en": "Apa Fungsi DIAC (Diode for Alternating Current)?", "id": "Komponen Pemicu (Trigger) TRIAC." },
  { "en": "Apa Itu Tegangan Breakover (Breakover Voltage) DIAC?", "id": "Tegangan Saat DIAC Mulai Konduksi." },
  { "en": "Apa Kepanjangan GTO (Gate Turn-Off Thyristor)?", "id": "Thyristor (Thyristor) Gerbang Bisa Dimatikan." },
  { "en": "Apa Kelebihan GTO (Gate Turn-Off Thyristor)?", "id": "Dapat Dimatikan Melalui Sinyal Gerbang." },
  { "en": "Apa Kepanjangan UJT (Unijunction Transistor)?", "id": "Transistor Uni-Sambungan." },
  { "en": "Apa Fungsi UJT (Unijunction Transistor)?", "id": "Osilator Relaksasi, Pemicu SCR." },
  { "en": "Apa Itu Tegangan Puncak (Peak Voltage) (Vp) UJT?", "id": "Tegangan Emitor Saat Mulai Konduksi." },
  { "en": "Apa Itu Tegangan Lembah (Valley Voltage) (Vv) UJT?", "id": "Tegangan Emitor Saat Kembali Off." },
  { "en": "Apa Itu Rasio Intrinsik Standoff (η) UJT?", "id": "Parameter Penentu Tegangan Puncak UJT." },
  { "en": "Apa Itu Osilator Relaksasi (Relaxation Oscillator)?", "id": "Osilator Berbasis Pengisian Kapasitor." },
  { "en": "Apa Itu Optoelektronika (Optoelectronics)?", "id": "Interaksi Cahaya Dan Elektronika." },
  { "en": "Apa Itu LED (Light Emitting Diode)?", "id": "Dioda Memancarkan Cahaya Saat Bias Maju." },
  { "en": "Bahan Semikonduktor Apa Untuk LED (Light Emitting Diode) Merah?", "id": "Galium Arsenida Fosfida (GaAsP)." },
  { "en": "Bahan Semikonduktor Apa Untuk LED (Light Emitting Diode) Biru/Hijau?", "id": "Galium Nitrida (GaN), Indium Galium Nitrida (InGaN)." },
  { "en": "Apa Itu LED (Light Emitting Diode) Inframerah (Infrared)?", "id": "LED (Light Emitting Diode) Memancarkan Cahaya Inframerah." },
  { "en": "Dimana LED (Light Emitting Diode) Inframerah (Infrared) Digunakan?", "id": "Remote Control, Sensor Optik." },
  { "en": "Apa Itu Fotodioda (Photodiode)?", "id": "Dioda Detektor Cahaya (Bias Mundur)." },
  { "en": "Apa Itu Mode Fotokonduktif (Photoconductive Mode) Fotodioda?", "id": "Arus Mundur Proporsional Intensitas Cahaya." },
  { "en": "Apa Itu Mode Fotovoltaik (Photovoltaic Mode) Fotodioda?", "id": "Hasilkan Tegangan Tanpa Bias (Sel Surya)." },
  { "en": "Apa Itu Fototransistor (Phototransistor)?", "id": "Transistor (Basis Dikontrol Cahaya)." },
  { "en": "Apa Keuntungan Fototransistor (Phototransistor)?", "id": "Memiliki Penguatan Internal (Lebih Sensitif)." },
  { "en": "Apa Itu Optocoupler (Optocoupler)?", "id": "Isolasi Sinyal (LED Dan Fotodetektor)." },
  { "en": "Apa Tipe Output Optocoupler (Optocoupler)?", "id": "Transistor, Darlington, TRIAC, SCR." },
  { "en": "Apa Itu Current Transfer Ratio (CTR) Optocoupler?", "id": "Rasio Arus Output Terhadap Input LED." },
  { "en": "Apa Kepanjangan CTR (Current Transfer Ratio)?", "id": "Rasio Transfer Arus." },
  { "en": "Apa Itu Solid State Relay (SSR)?", "id": "Relay (Relay) Elektronik (Optocoupler, Saklar Daya)." },
  { "en": "Apa Jenis Saklar Daya SSR (Solid State Relay) AC?", "id": "Umumnya TRIAC Atau SCR (Silicon Controlled Rectifier) Anti-Paralel." },
  { "en": "Apa Jenis Saklar Daya SSR (Solid State Relay) DC?", "id": "Umumnya Transistor MOSFET Atau BJT." },
  { "en": "Apa Itu Dioda Laser (Laser Diode)?", "id": "Dioda Hasilkan Cahaya Laser Koheren." },
  { "en": "Dimana Dioda Laser (Laser Diode) Digunakan?", "id": "Pemutar CD/DVD, Serat Optik, Pointer." },
  { "en": "Apa Itu Tampilan Tujuh Segmen (7-Segment)?", "id": "Tampilan Numerik (Tujuh Segmen LED)." },
  { "en": "Apa Itu Dekoder BCD (Binary Coded Decimal) Ke 7-Segmen?", "id": "IC (Integrated Circuit) Penggerak Tampilan 7-Segmen." },
  { "en": "Contoh IC (Integrated Circuit) Dekoder 7-Segmen Common Cathode?", "id": "IC (Integrated Circuit) 74LS48." },
  { "en": "Contoh IC (Integrated Circuit) Dekoder 7-Segmen Common Anode?", "id": "IC (Integrated Circuit) 74LS47." },
  { "en": "Apa Itu Tampilan LCD (Liquid Crystal Display) Karakter?", "id": "Tampilan Teks Alfanumerik LCD." },
  { "en": "Kontroler Apa Umum Digunakan LCD (Liquid Crystal Display) Karakter?", "id": "Kontroler Hitachi HD44780." },
  { "en": "Apa Itu Backlight (Lampu Latar) LCD?", "id": "Sumber Cahaya Belakang Tampilan LCD." },
  { "en": "Apa Itu Tampilan LCD (Liquid Crystal Display) Grafis?", "id": "Tampilan Gambar, Teks Fleksibel LCD." },
  { "en": "Apa Itu Tampilan OLED (Organic Light Emitting Diode)?", "id": "Tampilan Berbasis LED (Light Emitting Diode) Organik." },
  { "en": "Apa Keunggulan Tampilan OLED (Organic Light Emitting Diode)?", "id": "Kontras Tinggi, Tipis, Hitam Pekat." },
  { "en": "Apa Itu Sirkuit Terpadu (Integrated Circuit) (IC)?", "id": "Komponen Elektronik Kompleks Satu Chip." },
  { "en": "Apa Itu Skala Integrasi (Scale of Integration)?", "id": "Ukuran Jumlah Komponen Dalam IC." },
  { "en": "Apa Itu SSI (Small Scale Integration)?", "id": "Integrasi Skala Kecil (Gerbang Logika)." },
  { "en": "Apa Itu MSI (Medium Scale Integration)?", "id": "Integrasi Skala Menengah (Counter, Decoder)." },
  { "en": "Apa Itu LSI (Large Scale Integration)?", "id": "Integrasi Skala Besar (Mikroprosesor Awal)." },
  { "en": "Apa Itu VLSI (Very Large Scale Integration)?", "id": "Integrasi Skala Sangat Besar (CPU Modern)." },
  { "en": "Apa Itu ULSI (Ultra Large Scale Integration)?", "id": "Integrasi Skala Ultra Besar." },
  { "en": "Apa Itu Wafer (Wafer) Silikon?", "id": "Cakram Silikon Bahan Dasar Pembuatan IC." },
  { "en": "Apa Itu Die (Die) IC (Integrated Circuit)?", "id": "Potongan Chip Individual Dari Wafer." },
  { "en": "Apa Itu Kemasan (Packaging) IC (Integrated Circuit)?", "id": "Wadah Pelindung IC (Integrated Circuit) Dengan Pin." },
  { "en": "Apa Itu Pin (Pin) IC (Integrated Circuit)?", "id": "Kaki Terminal Koneksi Eksternal IC." },
  { "en": "Apa Itu Datasheet (Lembar Data) IC?", "id": "Dokumen Spesifikasi Teknis IC." },
  { "en": "Informasi Apa Ada Di Datasheet (Lembar Data)?", "id": "Rating Maksimal, Karakteristik, Pinout." },
  { "en": "Apa Itu Pinout (Pinout) IC (Integrated Circuit)?", "id": "Diagram Susunan Fungsi Pin IC." },
  { "en": "Apa Itu Rating Maksimum Absolut (Absolute Maximum Ratings)?", "id": "Batas Kondisi Operasi Maksimum IC." },
  { "en": "Apa Itu Karakteristik Listrik (Electrical Characteristics)?", "id": "Parameter Kinerja IC (Tegangan, Arus)." },
  { "en": "Apa Itu Logika Positif (Positive Logic)?", "id": "Tegangan Tinggi = Logika 1." },
  { "en": "Apa Itu Logika Negatif (Negative Logic)?", "id": "Tegangan Tinggi = Logika 0." },
  { "en": "Apa Itu Noise Margin (Margin Derau)?", "id": "Toleransi Level Tegangan Terhadap Noise." },
  { "en": "Apa Itu Fan-Out (Fan-Out) Logika?", "id": "Jumlah Maksimal Input Dapat Digerakkan." },
  { "en": "Apa Itu Fan-In (Fan-In) Logika?", "id": "Jumlah Maksimal Input Gerbang Logika." },
  { "en": "Apa Itu Waktu Propagasi Tunda (Propagation Delay)?", "id": "Waktu Sinyal Merambat Lewat Gerbang." },
  { "en": "Apa Itu Daya Disipasi (Power Dissipation)?", "id": "Konsumsi Daya Rata-Rata Gerbang." },
  { "en": "Keluarga Logika Apa Paling Cepat?", "id": "ECL (Emitter-Coupled Logic)." },
  { "en": "Keluarga Logika Apa Paling Hemat Daya?", "id": "CMOS (Complementary Metal-Oxide-Semiconductor)." },
  { "en": "Apa Itu Antarmuka (Interfacing) Level Logika?", "id": "Menghubungkan Keluarga Logika Beda Tegangan." },
  { "en": "Apa Itu Level Shifter (Penggeser Level)?", "id": "Rangkaian Konversi Level Tegangan Logika." },
  { "en": "Apa Itu Open Collector (Kolektor Terbuka) Output?", "id": "Output BJT (Bipolar Junction Transistor) Tanpa Resistor Internal." },
  { "en": "Apa Itu Open Drain (Drain Terbuka) Output?", "id": "Output MOSFET (Metal-Oxide-Semiconductor FET) Tanpa Resistor Internal." },
  { "en": "Apa Keuntungan Open Collector/Drain (Kolektor/Drain Terbuka)?", "id": "Bisa Wired-AND, Fleksibel Level Tegangan." },
  { "en": "Apa Itu Wired-AND (AND Berkabel)?", "id": "Menghubungkan Output Open Collector Bersama." },
  { "en": "Apa Itu Resistor Pull-Down (Pull-Down Resistor)?", "id": "Resistor Ke Ground (Jaga Level Low)." },
  { "en": "Apa Itu Buffer (Buffer) Tri-State?", "id": "Output Bisa High, Low, Hi-Z." },
  { "en": "Kapan Kondisi Impedansi Tinggi (Hi-Z) Berguna?", "id": "Saat Berbagi Bus (Bus Sharing)." },
  { "en": "Apa Itu Keluarga Logika (Logic Family)?", "id": "Kelompok IC (Integrated Circuit) Teknologi Sama." },
  { "en": "Apa Kecepatan Switching (Switching Speed) Logika?", "id": "Seberapa Cepat Output Berubah." },
  { "en": "Apa Itu Waktu Naik (Rise Time) Sinyal?", "id": "Waktu Sinyal Naik (10% Ke 90%)." },
  { "en": "Apa Itu Waktu Turun (Fall Time) Sinyal?", "id": "Waktu Sinyal Turun (90% Ke 10%)." },
  { "en": "Apa Itu Propagasi Tunda (Propagation Delay)?", "id": "Waktu Tunda Sinyal Lewat Gerbang." },
  { "en": "Apa Itu Disipasi Daya Statis (Static Power Dissipation)?", "id": "Konsumsi Daya Saat Output Stabil." },
  { "en": "Apa Itu Disipasi Daya Dinamis (Dynamic Power Dissipation)?", "id": "Konsumsi Daya Saat Output Berubah." },
  { "en": "Keluarga Logika Apa Punya Daya Statis Rendah?", "id": "Keluarga CMOS (Complementary Metal-Oxide-Semiconductor)." },
  { "en": "Apa Itu Batas Derau (Noise Margin)?", "id": "Toleransi Level Tegangan Terhadap Derau." },
  { "en": "Apa Itu Rangkaian Generator Clock (Clock Generator)?", "id": "Rangkaian Penghasil Sinyal Clock Stabil." },
  { "en": "Apa Itu Osilator Cincin (Ring Oscillator)?", "id": "Osilator Sederhana (Inverter Ganjil)." },
  { "en": "Apa Itu PLL (Phase-Locked Loop) Sintesiser?", "id": "Penghasil Frekuensi Presisi Terkunci Fasa." },
  { "en": "Apa Itu Pembagi Frekuensi (Frequency Divider)?", "id": "Rangkaian Digital Pembagi Frekuensi Clock." },
  { "en": "Bagaimana Flip-Flop T (Toggle) Membagi Frekuensi?", "id": "Frekuensi Output Setengah Input Clock." },
  { "en": "Apa Itu Mesin Keadaan Hingga (FSM)?", "id": "Finite State Machine (FSM)." },
  { "en": "Apa Kepanjangan FSM (Finite State Machine)?", "id": "Mesin Keadaan Hingga." },
  { "en": "Apa Fungsi FSM (Finite State Machine)?", "id": "Model Komputasi Rangkaian Sekuensial." },
  { "en": "Apa Itu Diagram Keadaan (State Diagram)?", "id": "Representasi Grafis Transisi FSM." },
  { "en": "Apa Itu Tabel Keadaan (State Table)?", "id": "Representasi Tabel Transisi FSM." },
  { "en": "Apa Itu Mesin Moore (Moore Machine)?", "id": "Output FSM Tergantung Keadaan (State) Saja." },
  { "en": "Apa Itu Mesin Mealy (Mealy Machine)?", "id": "Output FSM Tergantung Keadaan, Input." },
  { "en": "Apa Itu Hazard (Hazard) Rangkaian Logika?", "id": "Output Spurious Sesaat (Glitch)." },
  { "en": "Apa Itu Hazard Statis (Static Hazard)?", "id": "Output Seharusnya Konstan Berubah Sesaat." },
  { "en": "Apa Itu Hazard Dinamis (Dynamic Hazard)?", "id": "Output Seharusnya Sekali Berubah Berkali-kali." },
  { "en": "Bagaimana Mencegah Hazard (Hazard) Kombinasional?", "id": "Menambah Term Redundan (Peta Karnaugh)." },
  { "en": "Apa Itu FPGA (Field Programmable Gate Array)?", "id": "IC (Integrated Circuit) Logika Dapat Diprogram." },
  { "en": "Apa Unit Dasar Logika FPGA?", "id": "Configurable Logic Block (CLB)." },
  { "en": "Apa Kepanjangan CLB (Configurable Logic Block)?", "id": "Blok Logika Dapat Dikonfigurasi." },
  { "en": "Apa Isi CLB (Configurable Logic Block) FPGA?", "id": "LUT (Look-Up Table), Flip-Flop." },
  { "en": "Apa Kepanjangan LUT (Look-Up Table)?", "id": "Tabel Pencarian." },
  { "en": "Apa Fungsi LUT (Look-Up Table) FPGA?", "id": "Implementasi Fungsi Logika Kombinasional." },
  { "en": "Apa Bahasa Deskripsi Perangkat Keras (HDL)?", "id": "Hardware Description Language (HDL)." },
  { "en": "Apa Kepanjangan HDL (Hardware Description Language)?", "id": "Bahasa Deskripsi Perangkat Keras." },
  { "en": "Contoh Bahasa HDL (Hardware Description Language)?", "id": "VHDL (VHSIC HDL), Verilog." },
  { "en": "Apa Itu Sintesis (Synthesis) HDL?", "id": "Menerjemahkan Kode HDL Ke Struktur Logika." },
  { "en": "Apa Itu Simulasi (Simulation) HDL?", "id": "Memverifikasi Fungsionalitas Desain HDL." },
  { "en": "Apa Itu ASIC (Application Specific Integrated Circuit)?", "id": "IC (Integrated Circuit) Didesain Fungsi Khusus." },
  { "en": "Apa Beda ASIC (Application Specific Integrated Circuit), FPGA (Field Programmable Gate Array)?", "id": "ASIC (Application Specific Integrated Circuit) Permanen, FPGA (Field Programmable Gate Array) Bisa Diprogram." },
  { "en": "Mana Lebih Cepat, ASIC (Application Specific Integrated Circuit) Atau FPGA (Field Programmable Gate Array)?", "id": "ASIC (Application Specific Integrated Circuit) Umumnya Lebih Cepat." },
  { "en": "Mana Biaya Pengembangan Lebih Tinggi?", "id": "ASIC (Application Specific Integrated Circuit) (NRE Tinggi)." },
  { "en": "Apa Kepanjangan NRE (Non-Recurring Engineering)?", "id": "Rekayasa Non-Berulang." },
  { "en": "Apa Itu SoC (System on a Chip)?", "id": "Sistem Lengkap Dalam Satu Chip IC." },
  { "en": "Apa Kepanjangan SoC (System on a Chip)?", "id": "Sistem Dalam Chip." },
  { "en": "Contoh SoC (System on a Chip)?", "id": "Prosesor Smartphone." },
  { "en": "Apa Itu IP (Intellectual Property) Core?", "id": "Blok Desain Logika Siap Pakai." },
  { "en": "Apa Itu Sensor Gambar (Image Sensor)?", "id": "Mengubah Cahaya Menjadi Sinyal Elektronik." },
  { "en": "Apa Dua Tipe Sensor Gambar Utama?", "id": "CCD (Charged-Couple Device), CMOS (Complementary Metal-Oxide-Semiconductor)." },
  { "en": "Mana Sensor Gambar Lebih Hemat Daya?", "id": "Sensor CMOS (Complementary Metal-Oxide-Semiconductor)." },
  { "en": "Apa Itu Piksel (Pixel)?", "id": "Elemen Gambar Terkecil." },
  { "en": "Apa Itu Resolusi (Resolution) Sensor Gambar?", "id": "Jumlah Total Piksel (Megapixel)." },
  { "en": "Apa Itu Global Shutter (Rana Global)?", "id": "Semua Piksel Merekam Bersamaan." },
  { "en": "Apa Itu Rolling Shutter (Rana Bergulir)?", "id": "Piksel Merekam Baris Per Baris." },
  { "en": "Apa Efek Rolling Shutter (Rana Bergulir)?", "id": "Distorsi Gerak Cepat (Skew, Wobble)." },
  { "en": "Apa Itu Filter Warna Bayer (Bayer Filter)?", "id": "Pola Filter Merah, Hijau, Biru Sensor." },
  { "en": "Mengapa Perlu Filter Bayer (Bayer Filter)?", "id": "Sensor Hanya Deteksi Intensitas (Monokrom)." },
  { "en": "Apa Proses Demosaicing (Demosaicing)?", "id": "Merekonstruksi Warna Penuh Dari Bayer." },
  { "en": "Apa Itu Rentang Dinamis (Dynamic Range) Sensor?", "id": "Rasio Intensitas Terang, Gelap Tercakup." },
  { "en": "Apa Satuan Rentang Dinamis (Dynamic Range)?", "id": "Stop Atau Desibel (dB)." },
  { "en": "Apa Itu ISO (ISO Sensitivity) Sensor?", "id": "Ukuran Sensitivitas Sensor Terhadap Cahaya." },
  { "en": "ISO (ISO Sensitivity) Tinggi Menyebabkan Apa?", "id": "Gambar Lebih Terang, Tapi Noise Meningkat." },
  { "en": "Apa Itu Noise (Derau) Sensor Gambar?", "id": "Gangguan Acak Pada Gambar." },
  { "en": "Sebutkan Jenis Noise (Derau) Sensor?", "id": "Shot Noise, Read Noise, Thermal Noise." },
  { "en": "Apa Itu Dark Current (Arus Gelap) Sensor?", "id": "Arus Dihasilkan Sensor Tanpa Cahaya." },
  { "en": "Apa Itu Lensa (Lens) Kamera?", "id": "Komponen Optik Fokuskan Cahaya Sensor." },
  { "en": "Apa Itu Panjang Fokus (Focal Length) Lensa?", "id": "Jarak Lensa Ke Titik Fokus (mm)." },
  { "en": "Apa Itu Aperture (Apertur) Lensa?", "id": "Bukaan Lensa Kontrol Jumlah Cahaya." },
  { "en": "Apa Satuan Aperture (Apertur)?", "id": "f-number (f/stop) (Contoh: f/2.8)." },
  { "en": "f-number Kecil Berarti Aperture (Apertur) Bagaimana?", "id": "Aperture (Apertur) Lebih Besar (Lebih Banyak Cahaya)." },
  { "en": "Apa Itu Depth of Field (Kedalaman Bidang)?", "id": "Rentang Jarak Objek Tampak Fokus." },
  { "en": "Aperture (Apertur) Besar Menghasilkan Depth of Field (Kedalaman Bidang) Bagaimana?", "id": "Depth of Field (Kedalaman Bidang) Sempit (Bokeh)." },
  { "en": "Apa Itu Shutter Speed (Kecepatan Rana)?", "id": "Durasi Sensor Terkena Cahaya." },
  { "en": "Shutter Speed (Kecepatan Rana) Cepat Untuk Apa?", "id": "Membekukan Gerakan Objek Cepat." },
  { "en": "Shutter Speed (Kecepatan Rana) Lambat Menyebabkan Apa?", "id": "Motion Blur (Buram Gerak)." },
  { "en": "Apa Itu White Balance (Keseimbangan Putih)?", "id": "Menyesuaikan Warna Agar Putih Akurat." },
  { "en": "Apa Itu Format Gambar RAW (RAW Image)?", "id": "Data Mentah Sensor (Belum Diproses)." },
  { "en": "Apa Itu Format Gambar JPEG (JPEG Format)?", "id": "Format Gambar Terkompresi (Lossy)." },
  { "en": "Apa Kepanjangan JPEG (Joint Photographic Experts Group)?", "id": "Kelompok Ahli Fotografi Bersama." },
  { "en": "Apa Itu Pemrosesan Gambar Digital (Digital Image Processing)?", "id": "Manipulasi Gambar Menggunakan Komputer." },
  { "en": "Contoh Operasi Pemrosesan Gambar (Image Processing)?", "id": "Filtering, Deteksi Tepi, Segmentasi." },
  { "en": "Apa Itu Filter Gambar (Image Filter)?", "id": "Operasi Mengubah Nilai Piksel." },
  { "en": "Contoh Filter Gambar (Image Filter)?", "id": "Gaussian Blur, Sharpening, Median Filter." },
  { "en": "Apa Itu Deteksi Tepi (Edge Detection)?", "id": "Menemukan Batas Objek Dalam Gambar." },
  { "en": "Algoritma Deteksi Tepi (Edge Detection) Populer?", "id": "Sobel, Canny, Prewitt." },
  { "en": "Apa Itu Segmentasi Gambar (Image Segmentation)?", "id": "Membagi Gambar Menjadi Region Bermakna." },
  { "en": "Apa Itu Pengenalan Objek (Object Recognition)?", "id": "Mengidentifikasi Objek Dalam Gambar." },
  { "en": "Apa Itu Pengenalan Wajah (Face Recognition)?", "id": "Mengidentifikasi Wajah Manusia." },
  { "en": "Apa Itu Optical Character Recognition (OCR)?", "id": "Pengenalan Karakter Optik." },
  { "en": "Apa Kepanjangan OCR (Optical Character Recognition)?", "id": "Pengenalan Karakter Optik." },
  { "en": "Apa Fungsi OCR (Optical Character Recognition)?", "id": "Mengubah Teks Gambar Ke Teks Digital." },
  { "en": "Apa Itu Steganografi (Steganography)?", "id": "Menyembunyikan Pesan Dalam Media Lain." },
  { "en": "Apa Itu Watermarking (Watermarking) Digital?", "id": "Menyisipkan Informasi Kepemilikan Ke Media." },
  { "en": "Apa Itu Visi Komputer (Computer Vision)?", "id": "Bidang AI (Artificial Intelligence) Pemahaman Gambar/Video." },
  { "en": "Apa Itu OpenCV (Open Source Computer Vision)?", "id": "Pustaka (Library) Populer Visi Komputer." },
  { "en": "Apa Itu Deep Learning (Pembelajaran Mendalam) Visi Komputer?", "id": "Menggunakan CNN (Convolutional Neural Network) Tugas Visi." },
  { "en": "Apa Itu Robotika (Robotics)?", "id": "Desain, Konstruksi, Operasi Robot." },
  { "en": "Apa Itu Derajat Kebebasan (DOF) Robot?", "id": "Jumlah Gerakan Independen Robot." },
  { "en": "Apa Itu Kinematika (Kinematics) Robot?", "id": "Studi Gerakan Tanpa Memperhitungkan Gaya." },
  { "en": "Apa Itu Dinamika (Dynamics) Robot?", "id": "Studi Gerakan Memperhitungkan Gaya, Momen." },
  { "en": "Apa Itu Kontrol Robot (Robot Control)?", "id": "Mengatur Gerakan Robot Capai Tujuan." },
  { "en": "Apa Itu Sensor Proprioceptive (Proprioceptive) Robot?", "id": "Sensor Internal (Posisi Sendi, Kecepatan)." },
  { "en": "Apa Itu Sensor Exteroceptive (Exteroceptive) Robot?", "id": "Sensor Eksternal (Kamera, Lidar, Sentuh)." },
  { "en": "Apa Itu Aktuator (Actuator) Robot?", "id": "Penggerak Robot (Motor, Silinder)." },
  { "en": "Apa Itu End Effector (End Effector) Robot?", "id": "Alat Kerja Di Ujung Lengan Robot." },
  { "en": "Contoh End Effector (End Effector)?", "id": "Gripper (Penjepit), Welding Tool (Alat Las)." },
  { "en": "Apa Itu Perencanaan Jalur (Path Planning) Robot?", "id": "Menemukan Jalur Bebas Hambatan Robot." },
  { "en": "Apa Itu Penghindaran Rintangan (Obstacle Avoidance)?", "id": "Kemampuan Robot Menghindari Tabrakan." },
  { "en": "Apa Itu Lokalisasi (Localization) Robot?", "id": "Menentukan Posisi Robot Di Lingkungan." },
  { "en": "Apa Itu Pemetaan (Mapping) Robot?", "id": "Membangun Peta Lingkungan Sekitar Robot." },
  { "en": "Apa Kepanjangan SLAM (Simultaneous Localization And Mapping)?", "id": "Lokalisasi Dan Pemetaan Simultan." },
  { "en": "Apa Itu Kontrol PID (Proportional Integral Derivative) Robot?", "id": "Kontroler Umum Gerakan Sendi Robot." },
  { "en": "Apa Itu Ruang Sendi (Joint Space) Robot?", "id": "Koordinat Berdasarkan Sudut Sendi Robot." },
  { "en": "Apa Itu Ruang Kartesian (Cartesian Space) Robot?", "id": "Koordinat Berdasarkan Posisi X, Y, Z." },
  { "en": "Apa Itu Matriks Transformasi Homogen (Homogeneous Transformation)?", "id": "Matriks Representasi Posisi, Orientasi." },
  { "en": "Apa Itu Parameter Denavit-Hartenberg (DH Parameters)?", "id": "Konvensi Mendeskripsikan Kinematika Robot." },
  { "en": "Apa Kepanjangan DH (Denavit-Hartenberg)?", "id": "Denavit-Hartenberg." },
  { "en": "Apa Itu Singularitas (Singularity) Robot?", "id": "Konfigurasi Robot Kehilangan Derajat Kebebasan." },
  { "en": "Apa Itu Jacobian (Jacobian Matrix) Robot?", "id": "Matriks Hubungan Kecepatan Sendi, End-Effector." },
  { "en": "Apa Itu Robotika Bergerak (Mobile Robotics)?", "id": "Studi Robot Yang Dapat Berpindah." },
  { "en": "Apa Itu Robot Beroda (Wheeled Robot)?", "id": "Robot Bergerak Menggunakan Roda." },
  { "en": "Apa Itu Robot Berkaki (Legged Robot)?", "id": "Robot Bergerak Menggunakan Kaki." },
  { "en": "Apa Itu Robot Terbang (Aerial Robot) (Drone)?", "id": "Robot Terbang (Pesawat Tanpa Awak)." },
  { "en": "Apa Itu Robot Bawah Air (Underwater Robot)?", "id": "Robot Beroperasi Di Bawah Air (ROV)." },
  { "en": "Apa Kepanjangan ROV (Remotely Operated Vehicle)?", "id": "Kendaraan Dioperasikan Jarak Jauh." },
  { "en": "Apa Kepanjangan AUV (Autonomous Underwater Vehicle)?", "id": "Kendaraan Bawah Air Otonom." },
  { "en": "Apa Itu Odometry (Odometry) Robot?", "id": "Estimasi Posisi Berdasarkan Gerakan Roda." },
  { "en": "Apa Itu Sensor LiDAR (Light Detection And Ranging)?", "id": "Sensor Jarak Menggunakan Laser." },
  { "en": "Apa Kepanjangan LiDAR (Light Detection And Ranging)?", "id": "Deteksi Dan Jangkauan Cahaya." },
  { "en": "Apa Itu Teleoperasi (Teleoperation) Robot?", "id": "Mengendalikan Robot Dari Jarak Jauh." },
  { "en": "Apa Itu Robotika Kawanan (Swarm Robotics)?", "id": "Koordinasi Banyak Robot Sederhana." },
  { "en": "Apa Itu Bio-Inspired Robotics (Robotika Bio-Inspired)?", "id": "Robot Terinspirasi Sistem Biologis." },
  { "en": "Apa Itu Soft Robotics (Robotika Lunak)?", "id": "Robot Terbuat Bahan Fleksibel, Lunak." },
  { "en": "Apa Itu Hukum Robotika Asimov (Asimov's Laws)?", "id": "Tiga Aturan Fiksi Etika Robot." },
  { "en": "Apa Itu Tes Turing (Turing Test)?", "id": "Tes Kecerdasan Buatan (AI)." },
  { "en": "Apa Itu Superkonduktivitas (Superconductivity)?", "id": "Hilangnya Hambatan Listrik Total." },
  { "en": "Pada Suhu Berapa Superkonduktivitas (Superconductivity) Terjadi?", "id": "Suhu Kritis Sangat Rendah (Tc)." },
  { "en": "Apa Itu Superkonduktor (Superconductor) Suhu Tinggi?", "id": "Superkonduktor (Superconductor) Tc Di Atas Nitrogen Cair." },
  { "en": "Apa Itu Efek Meissner (Meissner Effect)?", "id": "Penolakan Medan Magnet Superkonduktor." },
  { "en": "Aplikasi Superkonduktor (Superconductor)?", "id": "MRI (Magnetic Resonance Imaging), Maglev, Kabel Daya." },
  { "en": "Apa Itu Kriogenik (Cryogenics)?", "id": "Studi Suhu Sangat Rendah." },
  { "en": "Apa Itu Helium Cair (Liquid Helium)?", "id": "Pendingin Mencapai Suhu Sangat Rendah." },
  { "en": "Apa Itu Nitrogen Cair (Liquid Nitrogen)?", "id": "Pendingin Kriogenik Relatif Murah." },
  { "en": "Apa Itu SQUID (Superconducting Quantum Interference Device)?", "id": "Sensor Medan Magnet Sangat Sensitif." },
  { "en": "Apa Kepanjangan SQUID (Superconducting Quantum Interference Device)?", "id": "Perangkat Interferensi Kuantum Superkonduktor." },
  { "en": "Apa Itu Josephson Junction (Sambungan Josephson)?", "id": "Elemen Dasar Sirkuit Superkonduktor." },
  { "en": "Apa Itu Komputasi Kuantum (Quantum Computing)?", "id": "Komputasi Berbasis Mekanika Kuantum." },
  { "en": "Apa Itu Qubit (Qubit)?", "id": "Unit Informasi Kuantum." },
  { "en": "Apa Keadaan (State) Qubit (Qubit)?", "id": "Bisa 0, 1, Atau Superposisi." },
  { "en": "Apa Itu Superposisi (Superposition) Kuantum?", "id": "Kemampuan Sistem Kuantum Banyak Keadaan." },
  { "en": "Apa Itu Keterikatan Kuantum (Quantum Entanglement)?", "id": "Hubungan Terkorelasi Antar Qubit." },
  { "en": "Apa Itu Gerbang Kuantum (Quantum Gate)?", "id": "Operasi Logika Pada Qubit." },
  { "en": "Apa Itu Algoritma Shor (Shor's Algorithm)?", "id": "Algoritma Kuantum Faktorisasi Bilangan." },
  { "en": "Apa Implikasi Algoritma Shor (Shor's Algorithm)?", "id": "Mengancam Kriptografi Kunci Publik Saat Ini." },
  { "en": "Apa Itu Algoritma Grover (Grover's Algorithm)?", "id": "Algoritma Kuantum Pencarian Basis Data." },
  { "en": "Apa Itu Dekokerensi (Decoherence) Kuantum?", "id": "Hilangnya Sifat Kuantum Akibat Lingkungan." },
  { "en": "Tantangan Utama Komputasi Kuantum (Quantum Computing)?", "id": "Mengatasi Dekokerensi (Decoherence), Skalabilitas." },
  { "en": "Apa Itu Nanoteknologi (Nanotechnology)?", "id": "Teknologi Skala Nanometer." },
  { "en": "Berapa Ukuran Satu Nanometer (nm)?", "id": "Sepersemiliar Meter (10⁻⁹ m)." },
  { "en": "Apa Itu Karbon Nanotube (Carbon Nanotube)?", "id": "Struktur Silinder Karbon Skala Nano." },
  { "en": "Apa Sifat Karbon Nanotube (Carbon Nanotube)?", "id": "Sangat Kuat, Konduktivitas Listrik Tinggi." },
  { "en": "Apa Itu Graphene (Grafena)?", "id": "Lapisan Tunggal Atom Karbon (2D)." },
  { "en": "Apa Sifat Graphene (Grafena)?", "id": "Sangat Kuat, Konduktif, Transparan." },
  { "en": "Apa Itu Titik Kuantum (Quantum Dot)?", "id": "Nanokristal Semikonduktor (Sifat Optik Unik)." },
  { "en": "Dimana Titik Kuantum (Quantum Dot) Digunakan?", "id": "Display QLED (Quantum Dot LED), Pencitraan Biologis." },
  { "en": "Apa Itu Self-Assembly (Perakitan Diri)?", "id": "Proses Komponen Nano Merakit Otomatis." },
  { "en": "Apa Itu Litografi Nanoimprint (Nanoimprint Lithography)?", "id": "Teknik Pencetakan Pola Skala Nano." },
  { "en": "Apa Itu Mikroskop Elektron (Electron Microscope)?", "id": "Mikroskop Resolusi Tinggi (Gunakan Elektron)." },
  { "en": "Apa Itu SEM (Scanning Electron Microscope)?", "id": "Mikroskop Elektron Pemindai (Gambar Permukaan)." },
  { "en": "Apa Kepanjangan SEM (Scanning Electron Microscope)?", "id": "Mikroskop Elektron Pemindai." },
  { "en": "Apa Itu TEM (Transmission Electron Microscope)?", "id": "Mikroskop Elektron Transmisi (Gambar Internal)." },
  { "en": "Apa Kepanjangan TEM (Transmission Electron Microscope)?", "id": "Mikroskop Elektron Transmisi." },
  { "en": "Apa Itu AFM (Atomic Force Microscope)?", "id": "Mikroskop Gaya Atom (Pencitraan Permukaan Nano)." },
  { "en": "Apa Kepanjangan AFM (Atomic Force Microscope)?", "id": "Mikroskop Gaya Atom." },
  { "en": "Apa Itu Bahan Cerdas (Smart Material)?", "id": "Bahan Berubah Sifat Respons Stimulus." },
  { "en": "Contoh Bahan Cerdas (Smart Material)?", "id": "Bahan Piezoelektrik, Shape Memory Alloy." },
  { "en": "Apa Itu Paduan Memori Bentuk (SMA)?", "id": "Shape Memory Alloy (SMA)." },
  { "en": "Apa Kepanjangan SMA (Shape Memory Alloy)?", "id": "Paduan Memori Bentuk." },
  { "en": "Apa Sifat Paduan Memori Bentuk (SMA)?", "id": "Kembali Bentuk Asli Saat Dipanaskan." },
  { "en": "Apa Itu Bahan Elektrokromik (Electrochromic Material)?", "id": "Bahan Berubah Warna Akibat Listrik." },
  { "en": "Dimana Bahan Elektrokromik (Electrochromic Material) Digunakan?", "id": "Kaca Jendela Cerdas (Smart Window)." },
  { "en": "Apa Itu Rekayasa Biomedis (Biomedical Engineering)?", "id": "Aplikasi Teknik Di Bidang Medis." },
  { "en": "Apa Itu Pencitraan Medis (Medical Imaging)?", "id": "Teknik Visualisasi Bagian Dalam Tubuh." },
  { "en": "Contoh Teknik Pencitraan Medis (Medical Imaging)?", "id": "X-Ray, CT Scan, MRI, Ultrasound." },
  { "en": "Apa Itu Sinar-X (X-Ray)?", "id": "Radiasi Elektromagnetik Energi Tinggi." },
  { "en": "Apa Itu CT Scan (Computed Tomography)?", "id": "Pencitraan Sinar-X (X-Ray) Lintas Penampang." },
  { "en": "Apa Kepanjangan CT (Computed Tomography)?", "id": "Tomografi Terkomputasi." },
  { "en": "Apa Itu MRI (Magnetic Resonance Imaging)?", "id": "Pencitraan Menggunakan Medan Magnet, Radio." },
  { "en": "Apa Itu Ultrasonografi (Ultrasound)?", "id": "Pencitraan Menggunakan Gelombang Suara Ultrasonik." },
  { "en": "Apa Itu Elektrokardiogram (ECG/EKG)?", "id": "Perekaman Aktivitas Listrik Jantung." },
  { "en": "Apa Kepanjangan ECG (Electrocardiogram)?", "id": "Elektrokardiogram." },
  { "en": "Apa Itu Elektroensefalogram (EEG)?", "id": "Perekaman Aktivitas Listrik Otak." },
  { "en": "Apa Kepanjangan EEG (Electroencephalogram)?", "id": "Elektroensefalogram." },
  { "en": "Apa Itu Elektromiogram (EMG)?", "id": "Perekaman Aktivitas Listrik Otot." },
  { "en": "Apa Kepanjangan EMG (Electromyogram)?", "id": "Elektromiogram." },
  { "en": "Apa Itu Alat Pacu Jantung (Pacemaker)?", "id": "Perangkat Implant Stimulasi Detak Jantung." },
  { "en": "Apa Itu Defibrilator (Defibrillator)?", "id": "Perangkat Kejut Listrik Atasi Aritmia." },
  { "en": "Apa Itu Prostetik (Prosthetics)?", "id": "Penggantian Bagian Tubuh Artifisial." },
  { "en": "Apa Itu Antarmuka Otak-Komputer (BCI)?", "id": "Brain-Computer Interface (BCI)." },
  { "en": "Apa Kepanjangan BCI (Brain-Computer Interface)?", "id": "Antarmuka Otak-Komputer." },
  { "en": "Apa Fungsi BCI (Brain-Computer Interface)?", "id": "Komunikasi Langsung Otak Dengan Komputer." },
  { "en": "Apa Itu Rekayasa Jaringan (Tissue Engineering)?", "id": "Pengembangan Jaringan Biologis Artifisial." },
  { "en": "Apa Itu Pengiriman Obat (Drug Delivery) Tertarget?", "id": "Mengirim Obat Spesifik Ke Lokasi Tubuh." },
  { "en": "Apa Itu Telemedisin (Telemedicine)?", "id": "Pelayanan Medis Jarak Jauh (Teknologi)." },
  { "en": "Apa Itu Robot Bedah (Surgical Robot)?", "id": "Robot Membantu Prosedur Pembedahan." },
  { "en": "Apa Itu Bioinformatika (Bioinformatics)?", "id": "Aplikasi Komputasi Analisis Data Biologis." },
  { "en": "Apa Itu Sekuensing Genom (Genome Sequencing)?", "id": "Menentukan Urutan DNA Organisme." },
  { "en": "Apa Itu CRISPR (Clustered Regularly Interspaced Short Palindromic Repeats)?", "id": "Teknologi Penyuntingan Gen." },
  { "en": "Apa Kepanjangan CRISPR (Clustered Regularly Interspaced Short Palindromic Repeats)?", "id": "Pengulangan Palindromik Pendek Berjarak Teratur Berkelompok." },
  { "en": "Apa Itu Kekuatan Medan Listrik (E)?", "id": "Gaya Per Satuan Muatan Positif." },
  { "en": "Apa Itu Fluks Listrik (Electric Flux)?", "id": "Ukuran Jumlah Medan Listrik Menembus." },
  { "en": "Apa Itu Hukum Coulomb (Coulomb's Law)?", "id": "Gaya Antara Dua Muatan Listrik." },
  { "en": "Apa Itu Konstanta Coulomb (Coulomb Constant) (k)?", "id": "Konstanta Proporsionalitas Hukum Coulomb." },
  { "en": "Apa Itu Potensial Listrik (Electric Potential) (V)?", "id": "Energi Potensial Listrik Per Muatan." },
  { "en": "Apa Itu Tegangan (Voltage) Listrik?", "id": "Beda Potensial Antara Dua Titik." },
  { "en": "Apa Itu Permukaan Ekuipotensial (Equipotential Surface)?", "id": "Permukaan Dengan Potensial Sama Dimana-Mana." },
  { "en": "Kerja Apa Dilakukan Pindahkan Muatan Ekuipotensial?", "id": "Tidak Ada Kerja Yang Dilakukan (Nol)." },
  { "en": "Apa Itu Gradien (Gradient) Potensial?", "id": "Hubungan Antara Medan Listrik, Potensial." },
  { "en": "Apa Rumus Medan Listrik (E) Dari Potensial (V)?", "id": "E = -∇V (Gradien Negatif V)." },
  { "en": "Apa Itu Kapasitor (Capacitor)?", "id": "Komponen Penyimpan Muatan Listrik." },
  { "en": "Apa Satuan Kapasitansi (Capacitance)?", "id": "Farad (F)." },
  { "en": "Apa Rumus Muatan (Q) Kapasitor?", "id": "Q = C x V." },
  { "en": "Apa Rumus Kapasitansi (Capacitance) Pelat Sejajar?", "id": "C = ε₀ * εr * (A/d)." },
  { "en": "Apa Itu Dielektrik (Dielectric)?", "id": "Bahan Isolator Antara Pelat Kapasitor." },
  { "en": "Apa Fungsi Dielektrik (Dielectric) Kapasitor?", "id": "Meningkatkan Kapasitansi, Kekuatan Tegangan." },
  { "en": "Apa Itu Konstanta Dielektrik (Dielectric Constant) (εr)?", "id": "Permitivitas Relatif Bahan Dielektrik." },
  { "en": "Bagaimana Total Kapasitansi (Capacitance) Kapasitor Seri?", "id": "1/Ct = 1/C1 + 1/C2." },
  { "en": "Bagaimana Total Kapasitansi (Capacitance) Kapasitor Paralel?", "id": "Ct = C1 + C2." },
  { "en": "Apa Itu Energi (Energy) Tersimpan Kapasitor?", "id": "U = 1/2 * C * V²." },
  { "en": "Apa Itu Arus (Current) Listrik?", "id": "Laju Aliran Muatan Listrik." },
  { "en": "Apa Satuan Arus (Current) Listrik?", "id": "Ampere (A)." },
  { "en": "Apa Rumus Arus (Current) (I)?", "id": "I = dQ / dt." },
  { "en": "Apa Itu Rapat Arus (Current Density) (J)?", "id": "Arus Per Satuan Luas Penampang." },
  { "en": "Apa Itu Resistansi (Resistance) Listrik?", "id": "Hambatan Terhadap Aliran Arus." },
  { "en": "Apa Satuan Resistansi (Resistance)?", "id": "Ohm (Ω)." },
  { "en": "Apa Itu Hukum Ohm (Ohm's Law)?", "id": "V = I x R." },
  { "en": "Apa Itu Resistivitas (Resistivity) (ρ)?", "id": "Hambatan Jenis Intrinsik Bahan." },
  { "en": "Apa Rumus Resistansi (R) Dari Resistivitas (ρ)?", "id": "R = ρ * (L / A)." },
  { "en": "Apa Itu Konduktivitas (Conductivity) (σ)?", "id": "Kemampuan Bahan Hantarkan Listrik." },
  { "en": "Apa Hubungan Konduktivitas (σ), Resistivitas (ρ)?", "id": "σ = 1 / ρ." },
  { "en": "Apa Itu Daya (Power) Listrik?", "id": "Laju Transfer Energi Listrik." },
  { "en": "Apa Satuan Daya (Power) Listrik?", "id": "Watt (W)." },
  { "en": "Apa Rumus Daya (Power) (P)?", "id": "P = V x I = I²R = V²/R." },
  { "en": "Apa Itu Energi (Energy) Listrik?", "id": "Daya Kali Waktu (P x t)." },
  { "en": "Apa Satuan Energi (Energy) Listrik?", "id": "Joule (J) Atau Watt-Hour (Wh)." },
  { "en": "Bagaimana Total Resistansi (Resistance) Resistor Seri?", "id": "Rt = R1 + R2." },
  { "en": "Bagaimana Total Resistansi (Resistance) Resistor Paralel?", "id": "1/Rt = 1/R1 + 1/R2." },
  { "en": "Apa Itu Pembagi Tegangan (Voltage Divider)?", "id": "Rangkaian Resistor Seri Bagi Tegangan." },
  { "en": "Apa Rumus Tegangan Output Pembagi Tegangan?", "id": "Vout = Vin * (R2 / (R1+R2))." },
  { "en": "Apa Itu Pembagi Arus (Current Divider)?", "id": "Rangkaian Resistor Paralel Bagi Arus." },
  { "en": "Apa Itu Hukum Arus Kirchhoff (Kirchhoff's Current Law) (KCL)?", "id": "Total Arus Masuk Simpul Sama Dengan Nol." },
  { "en": "Apa Itu Hukum Tegangan Kirchhoff (Kirchhoff's Voltage Law) (KVL)?", "id": "Total Tegangan Dalam Loop Tertutup Nol." },
  { "en": "Apa Itu Simpul (Node) Rangkaian?", "id": "Titik Pertemuan Tiga Cabang Atau Lebih." },
  { "en": "Apa Itu Cabang (Branch) Rangkaian?", "id": "Jalur Antara Dua Simpul." },
  { "en": "Apa Itu Loop (Loop) Rangkaian?", "id": "Jalur Tertutup Dalam Rangkaian." },
  { "en": "Apa Itu Analisis Simpul (Nodal Analysis)?", "id": "Metode Analisis Rangkaian Berbasis KCL." },
  { "en": "Apa Itu Analisis Mesh (Mesh Analysis)?", "id": "Metode Analisis Rangkaian Berbasis KVL." },
  { "en": "Apa Itu Sumber Tegangan (Voltage Source) Independen?", "id": "Nilai Tegangan Tidak Tergantung Rangkaian." },
  { "en": "Apa Itu Sumber Arus (Current Source) Independen?", "id": "Nilai Arus Tidak Tergantung Rangkaian." },
  { "en": "Apa Itu Sumber Terikat (Dependent Source)?", "id": "Nilai Sumber Dikontrol Variabel Lain." },
  { "en": "Apa Itu Teorema Superposisi (Superposition Theorem)?", "id": "Analisis Rangkaian Linear Multi Sumber." },
  { "en": "Apa Itu Teorema Thevenin (Thevenin's Theorem)?", "id": "Sederhanakan Rangkaian Jadi Vth, Rth Seri." },
  { "en": "Apa Itu Teorema Norton (Norton's Theorem)?", "id": "Sederhanakan Rangkaian Jadi In, Rn Paralel." },
  { "en": "Bagaimana Mencari Resistansi Thevenin (Rth)?", "id": "Matikan Sumber Independen, Hitung Resistansi Ekuivalen." },
  { "en": "Bagaimana Mematikan Sumber Tegangan (Voltage Source) Ideal?", "id": "Ganti Dengan Hubungan Singkat (Short)." },
  { "en": "Bagaimana Mematikan Sumber Arus (Current Source) Ideal?", "id": "Ganti Dengan Rangkaian Terbuka (Open)." },
  { "en": "Apa Itu Teorema Transfer Daya Maksimum?", "id": "Transfer Daya Maksimal R Beban = Rth." },
  { "en": "Apa Itu Induktor (Inductor)?", "id": "Komponen Penyimpan Energi Medan Magnet." },
  { "en": "Apa Satuan Induktansi (Inductance)?", "id": "Henry (H)." },
  { "en": "Apa Rumus Tegangan (Voltage) Induktor?", "id": "V = L * (dI/dt)." },
  { "en": "Apa Sifat Induktor (Inductor) Terhadap Arus DC?", "id": "Bertindak Seperti Hubungan Singkat (Ideal)." },
  { "en": "Apa Sifat Induktor (Inductor) Terhadap Perubahan Arus?", "id": "Menentang Perubahan Arus Tiba-Tiba." },
  { "en": "Apa Rumus Induktansi (Inductance) Solenoida?", "id": "L = (μ * N² * A) / l." },
  { "en": "Bagaimana Total Induktansi (Inductance) Induktor Seri?", "id": "Lt = L1 + L2." },
  { "en": "Bagaimana Total Induktansi (Inductance) Induktor Paralel?", "id": "1/Lt = 1/L1 + 1/L2." },
  { "en": "Apa Itu Energi (Energy) Tersimpan Induktor?", "id": "U = 1/2 * L * I²." },
  { "en": "Apa Sifat Kapasitor (Capacitor) Terhadap Arus DC?", "id": "Bertindak Seperti Rangkaian Terbuka (Ideal)." },
  { "en": "Apa Sifat Kapasitor (Capacitor) Terhadap Perubahan Tegangan?", "id": "Menentang Perubahan Tegangan Tiba-Tiba." },
  { "en": "Apa Rumus Arus (Current) Kapasitor?", "id": "I = C * (dV/dt)." },
  { "en": "Apa Itu Rangkaian RC (Resistor Kapasitor) Orde Pertama?", "id": "Rangkaian Resistor, Kapasitor." },
  { "en": "Apa Itu Konstanta Waktu (Time Constant) RC (τ)?", "id": "τ = R x C." },
  { "en": "Apa Makna Konstanta Waktu (Time Constant) RC?", "id": "Waktu Capai 63.2 Persen Nilai Akhir." },
  { "en": "Apa Itu Rangkaian RL (Resistor Induktor) Orde Pertama?", "id": "Rangkaian Resistor, Induktor." },
  { "en": "Apa Itu Konstanta Waktu (Time Constant) RL (τ)?", "id": "τ = L / R." },
  { "en": "Apa Itu Respon Alami (Natural Response) Rangkaian?", "id": "Respon Tanpa Sumber Eksternal (Energi Awal)." },
  { "en": "Apa Itu Respon Paksa (Forced Response) Rangkaian?", "id": "Respon Akibat Sumber Eksternal." },
  { "en": "Apa Itu Respon Total (Total Response) Rangkaian?", "id": "Jumlah Respon Alami Dan Paksa." },
  { "en": "Apa Itu Respon Transien (Transient Response)?", "id": "Bagian Respon Yang Hilang Seiring Waktu." },
  { "en": "Apa Itu Respon Keadaan Tunak (Steady-State Response)?", "id": "Bagian Respon Yang Tersisa Setelah Lama." },
  { "en": "Apa Itu Rangkaian RLC (Resistor Induktor Kapasitor) Orde Kedua?", "id": "Rangkaian Mengandung R, L, Dan C." },
  { "en": "Apa Itu Faktor Redaman (Damping Factor) (α)?", "id": "Parameter Penentu Redaman RLC (α = R/2L)." },
  { "en": "Apa Itu Frekuensi Resonansi (Resonance Frequency) Alami (ω₀)?", "id": "Frekuensi Osilasi Tanpa Redaman (ω₀ = 1/√(LC))." },
  { "en": "Apa Itu Respon Overdamped (Redaman Lebih)?", "id": "α > ω₀ (Lambat, Tanpa Osilasi)." },
  { "en": "Apa Itu Respon Critically Damped (Redaman Kritis)?", "id": "α = ω₀ (Tercepat Tanpa Osilasi)." },
  { "en": "Apa Itu Respon Underdamped (Redaman Kurang)?", "id": "α < ω₀ (Berosilasi Sebelum Stabil)." },
  { "en": "Apa Itu Arus Bolak-Balik (Alternating Current) (AC)?", "id": "Arus Berubah Arah Periodik." },
  { "en": "Apa Bentuk Gelombang AC (Alternating Current) Paling Umum?", "id": "Gelombang Sinusoidal." },
  { "en": "Apa Itu Frekuensi (Frequency) (f) Sinyal AC?", "id": "Jumlah Siklus Per Detik (Hertz)." },
  { "en": "Apa Itu Periode (Period) (T) Sinyal AC?", "id": "Waktu Untuk Satu Siklus Lengkap." },
  { "en": "Apa Hubungan Frekuensi (f), Periode (T)?", "id": "f = 1 / T." },
  { "en": "Apa Itu Amplitudo (Amplitude) Sinyal AC?", "id": "Nilai Puncak Maksimum Gelombang." },
  { "en": "Apa Itu Fasa (Phase) Sinyal AC?", "id": "Posisi Relatif Gelombang Terhadap Referensi." },
  { "en": "Apa Itu Perbedaan Fasa (Phase Difference)?", "id": "Perbedaan Sudut Fasa Dua Gelombang." },
  { "en": "Apa Itu Fasor (Phasor)?", "id": "Representasi Vektor Kompleks Sinyal Sinusoidal." },
  { "en": "Apa Itu Impedansi (Impedance) (Z)?", "id": "Hambatan Total Rangkaian AC (Kompleks)." },
  { "en": "Apa Satuan Impedansi (Impedance)?", "id": "Ohm (Ω)." },
  { "en": "Apa Rumus Impedansi (Impedance) (Z) Kompleks?", "id": "Z = R + jX." },
  { "en": "Apa Itu Reaktansi (Reactance) (X)?", "id": "Bagian Imajiner Impedansi (Hambatan L/C)." },
  { "en": "Apa Reaktansi Induktif (Inductive Reactance) (XL)?", "id": "XL = ωL = 2πfL." },
  { "en": "Apa Reaktansi Kapasitif (Capacitive Reactance) (XC)?", "id": "XC = 1 / (ωC) = 1 / (2πfC)." },
  { "en": "Bagaimana Hubungan Fasa Tegangan, Arus Resistor?", "id": "Sefasa (In Phase)." },
  { "en": "Bagaimana Hubungan Fasa Tegangan, Arus Induktor?", "id": "Tegangan Mendahului Arus 90 Derajat." },
  { "en": "Bagaimana Hubungan Fasa Tegangan, Arus Kapasitor?", "id": "Arus Mendahului Tegangan 90 Derajat." },
  { "en": "Apa Itu Admitansi (Admittance) (Y)?", "id": "Kebalikan Dari Impedansi (1/Z)." },
  { "en": "Apa Satuan Admitansi (Admittance)?", "id": "Siemens (S)." },
  { "en": "Apa Itu Konduktansi (Conductance) (G)?", "id": "Bagian Riil Dari Admitansi." },
  { "en": "Apa Itu Suseptansi (Susceptance) (B)?", "id": "Bagian Imajiner Dari Admitansi." },
  { "en": "Apa Itu Daya Rata-Rata (Average Power) AC?", "id": "Daya Nyata Diserap Rangkaian (P)." },
  { "en": "Apa Rumus Daya Rata-Rata (Average Power) (P)?", "id": "P = Vrms * Irms * Cos(θ)." },
  { "en": "Apa Itu Daya Reaktif (Reactive Power) (Q)?", "id": "Daya Bolak-Balik Komponen Reaktif." },
  { "en": "Apa Satuan Daya Reaktif (Reactive Power)?", "id": "Volt-Ampere Reactive (VAR)." },
  { "en": "Apa Rumus Daya Reaktif (Reactive Power) (Q)?", "id": "Q = Vrms * Irms * Sin(θ)." },
  { "en": "Apa Itu Daya Semu (Apparent Power) (S)?", "id": "Magnitudo Daya Kompleks (|S|)." },
  { "en": "Apa Satuan Daya Semu (Apparent Power)?", "id": "Volt-Ampere (VA)." },
  { "en": "Apa Rumus Daya Semu (Apparent Power) (S)?", "id": "S = Vrms * Irms." },
  { "en": "Apa Itu Daya Kompleks (Complex Power) (S)?", "id": "Representasi Kompleks Daya AC." },
  { "en": "Apa Rumus Daya Kompleks (Complex Power) (S)?", "id": "S = P + jQ." },
  { "en": "Apa Itu Faktor Daya (Power Factor) (PF)?", "id": "Rasio Daya Nyata, Daya Semu." },
  { "en": "Apa Rumus Faktor Daya (Power Factor) (PF)?", "id": "PF = P / S = Cos(θ)." },
  { "en": "Apa Itu Sudut Faktor Daya (Power Factor Angle) (θ)?", "id": "Sudut Antara Tegangan Dan Arus." },
  { "en": "Apa Nilai Ideal Faktor Daya (Power Factor)?", "id": "Satu (1)." },
  { "en": "Apa Itu Koreksi Faktor Daya (Power Factor Correction)?", "id": "Memperbaiki Faktor Daya Mendekati Satu." },
  { "en": "Komponen Apa Digunakan Koreksi Beban Induktif?", "id": "Kapasitor Paralel." },
  { "en": "Apa Itu Nilai Efektif (RMS) Sinyal AC?", "id": "Nilai DC Ekuivalen Hasilkan Panas Sama." },
  { "en": "Apa Rumus RMS (Root Mean Square) Gelombang Sinus?", "id": "Vrms = Vpuncak / √2." },
  { "en": "Apa Itu Resonansi (Resonance) Rangkaian AC?", "id": "Kondisi Reaktansi Induktif Sama Kapasitif." },
  { "en": "Apa Frekuensi Resonansi (Resonance Frequency) (ω₀)?", "id": "ω₀ = 1 / √(LC)." },
  { "en": "Apa Impedansi (Impedance) Total RLC Seri Saat Resonansi?", "id": "Minimum (Sama Dengan R)." },
  { "en": "Apa Impedansi (Impedance) Total RLC Paralel Saat Resonansi?", "id": "Maksimum (Sama Dengan R)." },
  { "en": "Apa Itu Faktor Kualitas (Q Factor) Rangkaian?", "id": "Ukuran Rasio Energi Tersimpan, Hilang." },
  { "en": "Apa Rumus Q (Faktor Kualitas) RLC Seri?", "id": "Q = ω₀L / R = 1 / (ω₀CR)." },
  { "en": "Apa Itu Bandwidth (Bandwidth) Rangkaian Resonansi?", "id": "Rentang Frekuensi Antara Titik Setengah Daya." },
  { "en": "Apa Hubungan Bandwidth (BW), Frekuensi (ω₀), Q?", "id": "BW = ω₀ / Q." },
  { "en": "Apa Itu Filter Lolos Rendah (Low Pass Filter)?", "id": "Melewatkan Frekuensi Rendah, Redam Tinggi." },
  { "en": "Apa Itu Filter Lolos Tinggi (High Pass Filter)?", "id": "Melewatkan Frekuensi Tinggi, Redam Rendah." },
  { "en": "Apa Itu Filter Lolos Pita (Band Pass Filter)?", "id": "Melewatkan Rentang Frekuensi Tertentu." },
  { "en": "Apa Itu Filter Tolak Pita (Band Stop Filter)?", "id": "Menolak Rentang Frekuensi Tertentu." },
  { "en": "Apa Nama Lain Filter Tolak Pita (Band Stop)?", "id": "Notch Filter (Filter Takik)." },
  { "en": "Apa Itu Frekuensi Cutoff (Cutoff Frequency) (-3dB)?", "id": "Frekuensi Daya Turun Setengah." },
  { "en": "Apa Itu Orde (Order) Filter?", "id": "Menentukan Kecuraman (Roll-Off) Filter." },
  { "en": "Berapa Roll-Off (Roll-Off) Filter Orde Pertama?", "id": "-20 dB Per Dekade." },
  { "en": "Apa Itu Transformator (Transformer) Ideal?", "id": "Trafo Tanpa Rugi, Kopling Sempurna." },
  { "en": "Apa Fungsi Transformator (Transformer)?", "id": "Mengubah Level Tegangan AC." },
  { "en": "Apa Itu Rasio Belitan (Turns Ratio) (a)?", "id": "a = Np / Ns." },
  { "en": "Hubungan Tegangan (Voltage) Trafo Ideal?", "id": "Vp / Vs = Np / Ns = a." },
  { "en": "Hubungan Arus (Current) Trafo Ideal?", "id": "Ip / Is = Ns / Np = 1/a." },
  { "en": "Apa Itu Transformasi Impedansi (Impedance Transformation) Trafo?", "id": "Zp = a² * Zs." },
  { "en": "Apa Itu Kopling Magnetik (Magnetic Coupling)?", "id": "Interaksi Medan Magnet Antar Kumparan." },
  { "en": "Apa Itu Koefisien Kopling (Coupling Coefficient) (k)?", "id": "Ukuran Kekuatan Kopling Magnetik (0-1)." },
  { "en": "Apa Itu Induktansi Diri (Self Inductance) (L)?", "id": "Induktansi Kumparan Tunggal." },
  { "en": "Apa Itu Induktansi Mutual (Mutual Inductance) (M)?", "id": "Induktansi Akibat Kopling Dua Kumparan." },
  { "en": "Apa Rumus Induktansi Mutual (Mutual Inductance) (M)?", "id": "M = k * √(L1 * L2)." },
  { "en": "Apa Itu Konvensi Titik (Dot Convention)?", "id": "Menentukan Polaritas Tegangan Induksi Mutual." },
  { "en": "Apa Itu Rangkaian Magnetik Terkopel (Coupled Circuit)?", "id": "Rangkaian Dengan Induktansi Mutual." },
  { "en": "Apa Itu Sistem Tiga Fasa (Three-Phase System)?", "id": "Tiga Sumber AC Beda Fasa 120°." },
  { "en": "Apa Keuntungan Sistem Tiga Fasa (Three-Phase System)?", "id": "Lebih Efisien Transmisi Daya Besar." },
  { "en": "Apa Itu Koneksi Bintang (Wye/Y Connection)?", "id": "Titik Netral Bersama." },
  { "en": "Apa Itu Koneksi Delta (Delta/Δ Connection)?", "id": "Koneksi Bentuk Segitiga (Tanpa Netral)." },
  { "en": "Apa Itu Tegangan Fasa (Phase Voltage) (Vp)?", "id": "Tegangan Antara Fasa Dan Netral (Y)." },
  { "en": "Apa Itu Tegangan Saluran (Line Voltage) (Vl)?", "id": "Tegangan Antara Dua Fasa." },
  { "en": "Hubungan Vl, Vp Koneksi Bintang (Y)?", "id": "Vl = √3 * Vp." },
  { "en": "Hubungan Vl, Vp Koneksi Delta (Δ)?", "id": "Vl = Vp." },
  { "en": "Apa Itu Arus Fasa (Phase Current) (Ip)?", "id": "Arus Mengalir Lewat Beban Fasa." },
  { "en": "Apa Itu Arus Saluran (Line Current) (Il)?", "id": "Arus Mengalir Lewat Saluran Transmisi." },
  { "en": "Hubungan Il, Ip Koneksi Bintang (Y)?", "id": "Il = Ip." },
  { "en": "Hubungan Il, Ip Koneksi Delta (Δ)?", "id": "Il = √3 * Ip." },
  { "en": "Apa Rumus Daya Total (Total Power) Tiga Fasa?", "id": "P = √3 * Vl * Il * PF." },
  { "en": "Apa Itu Beban Seimbang (Balanced Load) Tiga Fasa?", "id": "Impedansi Sama Di Ketiga Fasa." },
  { "en": "Apa Itu Beban Tidak Seimbang (Unbalanced Load)?", "id": "Impedansi Berbeda Di Tiap Fasa." },
  { "en": "Apa Arus Netral (Neutral Current) Beban Seimbang?", "id": "Nol Ampere (Ideal)." },
  { "en": "Apa Itu Metode Dua Wattmeter (Two-Wattmeter Method)?", "id": "Mengukur Daya Tiga Fasa Tiga Kawat." },
  { "en": "Apa Itu Deret Fourier (Fourier Series)?", "id": "Representasi Sinyal Periodik Jumlahan Sinusoid." },
  { "en": "Apa Itu Koefisien Fourier (Fourier Coefficient)?", "id": "Amplitudo Komponen Frekuensi Deret Fourier." },
  { "en": "Apa Itu Simetri Setengah Gelombang (Half-Wave Symmetry)?", "id": "f(t) = -f(t + T/2)." },
  { "en": "Apa Implikasi Simetri Setengah Gelombang (Half-Wave Symmetry)?", "id": "Hanya Mengandung Harmonisa Ganjil." },
  { "en": "Apa Itu Simetri Genap (Even Symmetry)?", "id": "f(t) = f(-t)." },
  { "en": "Apa Implikasi Simetri Genap (Even Symmetry)?", "id": "Hanya Mengandung Komponen Cosinus (bn=0)." },
  { "en": "Apa Itu Simetri Ganjil (Odd Symmetry)?", "id": "f(t) = -f(-t)." },
  { "en": "Apa Implikasi Simetri Ganjil (Odd Symmetry)?", "id": "Hanya Mengandung Komponen Sinus (an=0)." },
  { "en": "Apa Itu Transformasi Fourier (Fourier Transform)?", "id": "Representasi Frekuensi Sinyal Non-Periodik." },
  { "en": "Apa Itu Spektrum Amplitudo (Amplitude Spectrum)?", "id": "Plot Amplitudo Komponen Frekuensi." },
  { "en": "Apa Itu Spektrum Fasa (Phase Spectrum)?", "id": "Plot Fasa Komponen Frekuensi." },
  { "en": "Apa Itu Transformasi Laplace (Laplace Transform)?", "id": "Generalisasi Transformasi Fourier (Analisis Sistem)." },
  { "en": "Apa Itu Variabel Kompleks (Complex Variable) (s) Laplace?", "id": "s = σ + jω." },
  { "en": "Apa Itu Fungsi Transfer (Transfer Function) H(s)?", "id": "Rasio Output Terhadap Input Domain-s." },
  { "en": "Apa Rumus Fungsi Transfer (Transfer Function) H(s)?", "id": "H(s) = Y(s) / X(s)." },
  { "en": "Apa Itu Pole (Kutub) Fungsi Transfer?", "id": "Akar Penyebut H(s) (Denominator)." },
  { "en": "Apa Itu Zero (Nol) Fungsi Transfer?", "id": "Akar Pembilang H(s) (Numerator)." },
  { "en": "Apa Hubungan Kestabilan (Stability), Posisi Pole?", "id": "Stabil Jika Semua Pole Di Kiri." },
  { "en": "Apa Itu Respon Frekuensi (Frequency Response) H(jω)?", "id": "Respon Sistem Terhadap Input Sinusoidal." },
  { "en": "Bagaimana Dapat Respon Frekuensi (Frequency Response) Dari H(s)?", "id": "Ganti s Dengan jω." },
  { "en": "Apa Itu Diagram Bode (Bode Plot)?", "id": "Plot Logaritmik Magnitudo, Fasa H(jω)." },
  { "en": "Apa Satuan Magnitudo (Magnitude) Diagram Bode?", "id": "Desibel (dB)." },
  { "en": "Apa Satuan Frekuensi (Frequency) Diagram Bode?", "id": "Radian Per Detik (rad/s) Skala Log." },
  { "en": "Apa Kemiringan (Slope) Pole Orde Pertama?", "id": "-20 dB Per Dekade." },
  { "en": "Apa Kemiringan (Slope) Zero Orde Pertama?", "id": "+20 dB Per Dekade." },
  { "en": "Apa Itu Jaringan Dua Port (Two-Port Network)?", "id": "Rangkaian Input Port, Output Port." },
  { "en": "Apa Itu Parameter Impedansi (Z-Parameters)?", "id": "V = Z * I." },
  { "en": "Apa Itu Parameter Admitansi (Y-Parameters)?", "id": "I = Y * V." },
  { "en": "Apa Itu Parameter Hibrid (h-Parameters)?", "id": "Campuran (Analisis Transistor)." },
  { "en": "Apa Itu Parameter Transmisi (ABCD Parameters)?", "id": "Parameter Analisis Saluran Transmisi." },
  { "en": "Apa Itu Parameter S (Scattering Parameters)?", "id": "Parameter Jaringan Frekuensi Tinggi (RF)." },
  { "en": "Apa Itu Smith Chart (Bagan Smith)?", "id": "Alat Grafis Analisis Rangkaian RF." },
  { "en": "Apa Fungsi Smith Chart (Bagan Smith)?", "id": "Impedansi, Admitansi, Pencocokan." },
  { "en": "Apa Itu Pencocokan Impedansi (Impedance Matching) RF?", "id": "Menyamakan Impedansi Transfer Daya Maksimal." },
  { "en": "Apa Itu Stub (Stub) Pencocokan?", "id": "Saluran Transmisi Pendek Untuk Matching." },
  { "en": "Apa Itu Filter Mikrostrip (Microstrip Filter)?", "id": "Filter Dibuat Jalur PCB Frekuensi Tinggi." },
  { "en": "Apa Itu Kopler Arah (Directional Coupler)?", "id": "Mencuplik Sebagian Daya Sinyal RF." },
  { "en": "Apa Itu Pembagi Daya (Power Divider) RF?", "id": "Membagi Daya Sinyal RF Rata." },
  { "en": "Apa Itu Penggabung Daya (Power Combiner) RF?", "id": "Menggabungkan Beberapa Sinyal RF." },
  { "en": "Apa Itu Sirkulator (Circulator) RF?", "id": "Perangkat Tiga Port Arah Sirkulasi." },
  { "en": "Apa Itu Isolator (Isolator) RF?", "id": "Perangkat Dua Port (Lewatkan Satu Arah)." },
  { "en": "Apa Itu Mixer (Mixer) Frekuensi?", "id": "Mencampur Dua Sinyal Hasilkan Frekuensi Baru." },
  { "en": "Apa Itu Frekuensi Jumlah (Sum Frequency) Mixer?", "id": "Frekuensi Hasil Penjumlahan Input (f1+f2)." },
  { "en": "Apa Itu Frekuensi Selisih (Difference Frequency) Mixer?", "id": "Frekuensi Hasil Pengurangan Input (|f1-f2|)." },
  { "en": "Apa Itu Osilator Lokal (Local Oscillator) (LO)?", "id": "Sumber Frekuensi Referensi Mixer." },
  { "en": "Apa Itu Frekuensi Menengah (Intermediate Frequency) (IF)?", "id": "Frekuensi Output Hasil Mixer Penerima." },
  { "en": "Apa Itu Konversi Naik (Upconversion)?", "id": "Mengubah Frekuensi Rendah Ke Tinggi." },
  { "en": "Apa Itu Konversi Turun (Downconversion)?", "id": "Mengubah Frekuensi Tinggi Ke Rendah." },
  { "en": "Apa Itu Derau Fasa (Phase Noise) Osilator?", "id": "Fluktuasi Fasa Acak Sinyal Osilator." },
  { "en": "Apa Efek Derau Fasa (Phase Noise)?", "id": "Membatasi Kinerja Sistem Komunikasi." },
  { "en": "Apa Itu Penguat Derau Rendah (LNA)?", "id": "Low Noise Amplifier (LNA)." },
  { "en": "Apa Kepanjangan LNA (Low Noise Amplifier)?", "id": "Penguat Derau Rendah." },
  { "en": "Apa Angka Derau (Noise Figure) (NF)?", "id": "Ukuran Tambahan Derau Oleh Penguat." },
  { "en": "Apakah NF (Noise Figure) Rendah Itu Baik?", "id": "Ya, Semakin Rendah Semakin Baik." },
  { "en": "Apa Itu Penguat Daya (Power Amplifier) (PA)?", "id": "Penguat Tahap Akhir Pemancar RF." },
  { "en": "Apa Itu Linearitas (Linearity) Penguat?", "id": "Kemampuan Penguat Tanpa Distorsi." },
  { "en": "Apa Itu Titik Kompresi 1-dB (P1dB)?", "id": "Daya Output Saat Gain Turun 1dB." },
  { "en": "Apa Itu Titik Intersep Orde Ketiga (IP3)?", "id": "Ukuran Linearitas (Intermodulasi)." },
  { "en": "Apa Kepanjangan IP3 (Third-Order Intercept Point)?", "id": "Titik Intersep Orde Ketiga." },
  { "en": "Apa Itu Distorsi Intermodulasi (Intermodulation Distortion) (IMD)?", "id": "Produk Frekuensi Baru Akibat Non-Linearitas." },
  { "en": "Apa Itu Filter SAW (Surface Acoustic Wave)?", "id": "Filter Akustik Permukaan (Selektif)." },
  { "en": "Apa Itu Filter BAW (Bulk Acoustic Wave)?", "id": "Filter Akustik Bulk (Frekuensi Tinggi)." },
  { "en": "Apa Itu Osilator Kristal Terkontrol Suhu (TCXO)?", "id": "Temperature Compensated Crystal Oscillator (TCXO)." },
  { "en": "Apa Kepanjangan TCXO (Temperature Compensated Crystal Oscillator)?", "id": "Osilator Kristal Terkompensasi Suhu." },
  { "en": "Apa Itu Osilator Kristal Kontrol Oven (OCXO)?", "id": "Oven Controlled Crystal Oscillator (OCXO)." },
  { "en": "Apa Kepanjangan OCXO (Oven Controlled Crystal Oscillator)?", "id": "Osilator Kristal Terkontrol Oven." },
  { "en": "Mana Lebih Stabil, TCXO (Temperature Compensated Crystal Oscillator) Atau OCXO (Oven Controlled Crystal Oscillator)?", "id": "OCXO (Oven Controlled Crystal Oscillator) Jauh Lebih Stabil." },
  { "en": "Apa Itu Getaran (Vibration) Mikrofonik?", "id": "Noise Osilator Akibat Getaran Mekanis." },
  { "en": "Apa Itu Resonator Dielektrik (Dielectric Resonator) (DR)?", "id": "Resonator Keramik Frekuensi Mikro." },
  { "en": "Apa Itu Sintesiser Frekuensi (Frequency Synthesizer)?", "id": "Penghasil Frekuensi Variabel Tapi Stabil." },
  { "en": "Apa Komponen Utama Sintesiser PLL (Phase-Locked Loop)?", "id": "PLL (Phase-Locked Loop), Referensi Kristal, Pembagi." },
  { "en": "Apa Itu Sintesiser DDS (Direct Digital Synthesizer)?", "id": "Direct Digital Synthesizer (DDS)." },
  { "en": "Apa Kepanjangan DDS (Direct Digital Synthesizer)?", "id": "Sintesiser Digital Langsung." },
  { "en": "Apa Keunggulan DDS (Direct Digital Synthesizer)?", "id": "Resolusi Frekuensi Halus, Switching Cepat." },
  { "en": "Apa Itu Spurious Signal (Sinyal Palsu)?", "id": "Frekuensi Tak Diinginkan Output Sintesiser." },
  { "en": "Apa Itu Harmonisa (Harmonics)?", "id": "Kelipatan Bulat Frekuensi Fundamental." },
  { "en": "Apa Itu Sub-harmonisa (Sub-harmonics)?", "id": "Pecahan Frekuensi Fundamental." },
  { "en": "Apa Itu Produk Intermodulasi (Intermodulation Products)?", "id": "Hasil Pencampuran Non-Linear Dua Sinyal." },
  { "en": "Apa Itu Sistem Komunikasi (Communication System)?", "id": "Pengiriman Informasi Dari Sumber Ke Tujuan." },
  { "en": "Apa Komponen Dasar Sistem Komunikasi?", "id": "Pemancar, Kanal, Penerima." },
  { "en": "Apa Itu Pemancar (Transmitter) (TX)?", "id": "Pengirim Sinyal Informasi." },
  { "en": "Apa Itu Penerima (Receiver) (RX)?", "id": "Penerima Sinyal Informasi." },
  { "en": "Apa Itu Kanal (Channel) Komunikasi?", "id": "Media Perambatan Sinyal." },
  { "en": "Apa Itu Noise (Derau) Kanal?", "id": "Gangguan Acak Ditambahkan Kanal." },
  { "en": "Apa Itu Atenuasi (Attenuation) Kanal?", "id": "Pelemahan Sinyal Oleh Kanal." },
  { "en": "Apa Itu Distorsi (Distortion) Kanal?", "id": "Perubahan Bentuk Sinyal Oleh Kanal." },
  { "en": "Apa Itu Teori Informasi (Information Theory)?", "id": "Studi Kuantifikasi Informasi, Komunikasi." },
  { "en": "Apa Itu Entropi (Entropy) Informasi?", "id": "Ukuran Ketidakpastian Sumber Informasi." },
  { "en": "Apa Satuan Entropi (Entropy) Informasi?", "id": "Bit (Bits) Per Simbol." },
  { "en": "Apa Itu Kapasitas Kanal (Channel Capacity)?", "id": "Laju Informasi Maksimum Lewat Kanal." },
  { "en": "Apa Itu Teorema Kapasitas Kanal Shannon?", "id": "Batas Kapasitas Kanal (Bandwidth, SNR)." },
  { "en": "Apa Itu Pengkodean Sumber (Source Coding)?", "id": "Kompresi Data (Hapus Redundansi)." },
  { "en": "Apa Itu Pengkodean Kanal (Channel Coding)?", "id": "Menambah Redundansi (Deteksi/Koreksi Error)." },
  { "en": "Apa Itu Deteksi Kesalahan (Error Detection)?", "id": "Mendeteksi Adanya Kesalahan Transmisi." },
  { "en": "Apa Itu Koreksi Kesalahan (Error Correction)?", "id": "Mendeteksi Dan Memperbaiki Kesalahan." },
  { "en": "Contoh Kode Deteksi Kesalahan (Error Detection)?", "id": "Paritas (Parity), Checksum (Checksum), CRC (Cyclic Redundancy Check)." },
  { "en": "Contoh Kode Koreksi Kesalahan (Error Correction)?", "id": "Kode Hamming, Reed-Solomon, Turbo, LDPC." },
  { "en": "Apa Itu Laju Kode (Code Rate)?", "id": "Rasio Bit Informasi Terhadap Total Bit." },
  { "en": "Apa Itu Jarak Hamming (Hamming Distance)?", "id": "Jumlah Posisi Bit Berbeda Dua Kode." },
  { "en": "Apa Itu Kemampuan Koreksi Error Kode?", "id": "Terkait Dengan Jarak Hamming Minimum." },
  { "en": "Apa Itu Pengacakan (Scrambling) Data?", "id": "Mengacak Pola Bit Cegah Urutan Panjang." },
  { "en": "Mengapa Perlu Pengacakan (Scrambling)?", "id": "Membantu Sinkronisasi Clock Penerima." },
  { "en": "Apa Itu Sinkronisasi (Synchronization) Komunikasi?", "id": "Menyelaraskan Waktu Pemancar, Penerima." },
  { "en": "Apa Itu Sinkronisasi Bit (Bit Synchronization)?", "id": "Menentukan Awal, Akhir Setiap Bit." },
  { "en": "Apa Itu Sinkronisasi Frame (Frame Synchronization)?", "id": "Menentukan Awal, Akhir Blok Data." },
  { "en": "Apa Itu Sinkronisasi Carrier (Carrier Synchronization)?", "id": "Menyelaraskan Frekuensi, Fasa Carrier." },
  { "en": "Apa Itu Kode Saluran (Line Coding)?", "id": "Representasi Bit Digital Sinyal Transmisi." },
  { "en": "Contoh Kode Saluran (Line Coding)?", "id": "NRZ, RZ, Manchester, AMI." },
  { "en": "Apa Kepanjangan NRZ (Non-Return-to-Zero)?", "id": "Tidak Kembali Ke Nol." },
  { "en": "Apa Kepanjangan RZ (Return-to-Zero)?", "id": "Kembali Ke Nol." },
  { "en": "Apa Itu Kode Manchester (Manchester Code)?", "id": "Transisi Tengah Bit (Self-Clocking)." },
  { "en": "Apa Kepanjangan AMI (Alternate Mark Inversion)?", "id": "Inversi Tanda Alternatif." },
  { "en": "Apa Itu Spektrum Daya (Power Spectrum) Sinyal?", "id": "Distribusi Daya Sinyal Terhadap Frekuensi." },
  { "en": "Apa Itu Densitas Spektral Daya (PSD)?", "id": "Power Spectral Density (PSD)." },
  { "en": "Apa Kepanjangan PSD (Power Spectral Density)?", "id": "Densitas Spektral Daya." },
  { "en": "Apa Satuan PSD (Power Spectral Density)?", "id": "Watt Per Hertz (W/Hz)." },
  { "en": "Apa Itu Lebar Pita (Bandwidth) Nyquist?", "id": "Lebar Pita Minimum Transmisi Bebas ISI." },
  { "en": "Apa Itu Kriteria Nyquist (Nyquist Criterion) ISI?", "id": "Syarat Transmisi Bebas ISI." },
  { "en": "Apa Itu Filter Raised Cosine (Raised Cosine)?", "id": "Filter Pembentuk Pulsa Bebas ISI." },
  { "en": "Apa Itu Faktor Roll-Off (Roll-Off Factor)?", "id": "Parameter Filter Raised Cosine." },
  { "en": "Apa Itu Diagram Mata (Eye Diagram)?", "id": "Visualisasi Kualitas Sinyal Digital." },
  { "en": "Apa Yang Diukur Diagram Mata (Eye Diagram)?", "id": "Jitter (Jitter), Noise Margin (Margin Derau), ISI." },
  { "en": "Mata Terbuka Lebar Menandakan Apa?", "id": "Kualitas Sinyal Sangat Baik." },
  { "en": "Apa Itu Multiple Access (Akses Ganda)?", "id": "Berbagi Sumber Daya Kanal Bersama." },
  { "en": "Apa Itu FDMA (Frequency Division Multiple Access)?", "id": "Akses Ganda Pembagian Frekuensi." },
  { "en": "Apa Itu TDMA (Time Division Multiple Access)?", "id": "Akses Ganda Pembagian Waktu." },
  { "en": "Apa Itu CDMA (Code Division Multiple Access)?", "id": "Akses Ganda Pembagian Kode." },
  { "en": "Apa Itu SDMA (Space Division Multiple Access)?", "id": "Akses Ganda Pembagian Ruang (Antena)." },
  { "en": "Apa Itu OFDMA (Orthogonal Frequency Division Multiple Access)?", "id": "Akses Ganda Berbasis OFDM." },
  { "en": "Dimana OFDMA (Orthogonal Frequency Division Multiple Access) Digunakan?", "id": "LTE (Long Term Evolution), Wi-Fi 6 (802.11ax)." },
  { "en": "Apa Itu Komunikasi Optik Ruang Bebas (FSO)?", "id": "Free Space Optics (FSO)." },
  { "en": "Apa Kepanjangan FSO (Free Space Optics)?", "id": "Optik Ruang Bebas." },
  { "en": "Apa Media Transmisi FSO (Free Space Optics)?", "id": "Cahaya Laser Lewat Udara." },
  { "en": "Apa Kelemahan FSO (Free Space Optics)?", "id": "Rentan Gangguan Cuaca (Kabut, Hujan)." },
  { "en": "Apa Itu Li-Fi (Light Fidelity)?", "id": "Komunikasi Nirkabel Menggunakan Cahaya LED." },
  { "en": "Apa Keunggulan Li-Fi (Light Fidelity)?", "id": "Bandwidth (Lebar Pita) Sangat Lebar, Aman." },
  { "en": "Apa Itu PLC (Power Line Communication)?", "id": "Komunikasi Data Lewat Jaringan Listrik." },
  { "en": "Apa Kepanjangan PLC (Power Line Communication)?", "id": "Komunikasi Saluran Listrik." },
  { "en": "Dimana PLC (Power Line Communication) Digunakan?", "id": "Jaringan Rumah, Meteran Cerdas." },
  { "en": "Apa Itu Radar Penetrasi Tanah (GPR)?", "id": "Ground Penetrating Radar (GPR)." },
  { "en": "Apa Kepanjangan GPR (Ground Penetrating Radar)?", "id": "Radar Penetrasi Tanah." },
  { "en": "Apa Fungsi GPR (Ground Penetrating Radar)?", "id": "Mendeteksi Objek Bawah Permukaan Tanah." },
  { "en": "Apa Itu Metrologi (Metrology)?", "id": "Ilmu Pengukuran." },
  { "en": "Apa Itu Standar Pengukuran (Measurement Standard)?", "id": "Referensi Fisik Satuan Ukur." },
  { "en": "Apa Itu Ketertelusuran (Traceability) Pengukuran?", "id": "Hubungan Pengukuran Ke Standar Nasional/Internasional." },
  { "en": "Apa Itu Ketidakpastian (Uncertainty) Pengukuran?", "id": "Rentang Keraguan Nilai Hasil Ukur." },
  { "en": "Apa Itu Kesalahan (Error) Pengukuran?", "id": "Selisih Hasil Ukur, Nilai Sebenarnya." },
  { "en": "Apa Itu Kesalahan Sistematik (Systematic Error)?", "id": "Kesalahan Tetap Atau Konsisten." },
  { "en": "Apa Itu Kesalahan Acak (Random Error)?", "id": "Kesalahan Fluktuatif Tidak Dapat Diprediksi." },
  { "en": "Bagaimana Mengurangi Kesalahan Acak (Random Error)?", "id": "Pengukuran Berulang, Rata-Rata." },
  { "en": "Bagaimana Mengurangi Kesalahan Sistematik (Systematic Error)?", "id": "Kalibrasi Alat Ukur." },
  { "en": "Apa Itu Kalibrasi (Calibration)?", "id": "Membandingkan Alat Ukur Dengan Standar." },
  { "en": "Apa Itu Akurasi (Accuracy)?", "id": "Kedekatan Hasil Ukur Nilai Sebenarnya." },
  { "en": "Apa Itu Presisi (Precision)?", "id": "Kedekatan Hasil Pengukuran Berulang." },
  { "en": "Bisakah Alat Ukur Presisi Tapi Tidak Akurat?", "id": "Ya, Bisa (Kesalahan Sistematik)." },
  { "en": "Apa Itu Resolusi (Resolution) Alat Ukur?", "id": "Perubahan Terkecil Dapat Dibaca Alat." },
  { "en": "Apa Itu Sensitivitas (Sensitivity) Alat Ukur?", "id": "Rasio Perubahan Output Terhadap Input." },
  { "en": "Apa Itu Linearitas (Linearity) Alat Ukur?", "id": "Kedekatan Respon Alat Garis Lurus." },
  { "en": "Apa Itu Histeresis (Hysteresis) Alat Ukur?", "id": "Perbedaan Output Saat Input Naik/Turun." },
  { "en": "Apa Itu Rentang Pengukuran (Measurement Range)?", "id": "Batas Nilai Minimum, Maksimum Alat." },
  { "en": "Apa Itu Span (Rentang Skala) Alat Ukur?", "id": "Selisih Batas Maksimum, Minimum Rentang." },
  { "en": "Apa Itu Zero Drift (Pergeseran Nol)?", "id": "Perubahan Pembacaan Nol Alat Ukur." },
  { "en": "Apa Itu Span Drift (Pergeseran Rentang)?", "id": "Perubahan Sensitivitas Alat Ukur." },
  { "en": "Apa Itu Alat Ukur Analog (Analog Instrument)?", "id": "Penunjukan Berbasis Jarum Skala Kontinu." },
  { "en": "Apa Itu Alat Ukur Digital (Digital Instrument)?", "id": "Penunjukan Berbasis Tampilan Angka Diskrit." },
  { "en": "Apa Itu Kesalahan Paralaks (Parallax Error)?", "id": "Kesalahan Baca Skala Analog Sudut Pandang." },
  { "en": "Apa Itu Gerakan D'Arsonval (D'Arsonval Movement)?", "id": "Prinsip Dasar Amperemeter Analog (PMMC)." },
  { "en": "Apa Kepanjangan PMMC (Permanent Magnet Moving Coil)?", "id": "Kumparan Bergerak Magnet Permanen." },
  { "en": "Apa Itu Efek Pemanasan (Loading Effect) Amperemeter?", "id": "Amperemeter Mengubah Arus Yang Diukur." },
  { "en": "Bagaimana Resistansi Internal Amperemeter Ideal?", "id": "Nol Ohm." },
  { "en": "Bagaimana Resistansi Internal Voltmeter Ideal?", "id": "Tak Terhingga Ohm." },
  { "en": "Apa Itu Ohmmeter (Ohmmeter) Tipe Seri?", "id": "Mengukur Resistansi (Baterai Internal Seri)." },
  { "en": "Apa Itu Ohmmeter (Ohmmeter) Tipe Shunt?", "id": "Mengukur Resistansi Rendah (Baterai Paralel)." },
  { "en": "Apa Itu Pengukur Jembatan (Bridge Measurement)?", "id": "Metode Pengukuran Presisi (Wheatstone, Kelvin)." },
  { "en": "Apa Itu Sensor Cerdas (Smart Sensor)?", "id": "Sensor Dengan Pemrosesan Internal, Komunikasi." },
  { "en": "Apa Itu Jaringan Sensor Nirkabel (WSN)?", "id": "Wireless Sensor Network (WSN)." },
  { "en": "Apa Kepanjangan WSN (Wireless Sensor Network)?", "id": "Jaringan Sensor Nirkabel." },
  { "en": "Protokol Apa Digunakan WSN (Wireless Sensor Network)?", "id": "Zigbee, LoRa, Bluetooth LE." },
  { "en": "Apa Itu Gateway (Gateway) WSN (Wireless Sensor Network)?", "id": "Penghubung Jaringan Sensor Ke Jaringan Lain." },
  { "en": "Apa Tantangan Utama WSN (Wireless Sensor Network)?", "id": "Konsumsi Daya Rendah, Jangkauan." },
  { "en": "Apa Itu Fusi Sensor (Sensor Fusion)?", "id": "Menggabungkan Data Dari Banyak Sensor." },
  { "en": "Apa Tujuan Fusi Sensor (Sensor Fusion)?", "id": "Meningkatkan Akurasi, Keandalan Pengukuran." },
  { "en": "Apa Itu Sistem Kontrol Terdistribusi (DCS)?", "id": "Distributed Control System (DCS)." },
  { "en": "Apa Kepanjangan DCS (Distributed Control System)?", "id": "Sistem Kontrol Terdistribusi." },
  { "en": "Arsitektur Apa Digunakan DCS (Distributed Control System)?", "id": "Kontroler Tersebar Terhubung Jaringan." },
  { "en": "Dimana DCS (Distributed Control System) Umum Digunakan?", "id": "Pabrik Proses Industri Skala Besar." },
  { "en": "Apa Beda DCS (Distributed Control System), PLC (Programmable Logic Controller)?", "id": "DCS (Lebih Proses), PLC (Lebih Logika Cepat)." },
  { "en": "Apa Itu Redundansi (Redundancy) DCS (Distributed Control System)?", "id": "Duplikasi Komponen Kritis (Kontroler, Jaringan)." },
  { "en": "Apa Itu Safety Instrumented System (SIS)?", "id": "Sistem Pengaman Proses Otomatis." },
  { "en": "Apa Kepanjangan SIS (Safety Instrumented System)?", "id": "Sistem Berinstrumen Keselamatan." },
  { "en": "Apa Fungsi SIS (Safety Instrumented System)?", "id": "Membawa Proses Ke Keadaan Aman Saat Bahaya." },
  { "en": "Apa Kepanjangan SIL (Safety Integrity Level)?", "id": "Tingkat Integritas Keselamatan." },
  { "en": "Apa Tingkatan SIL (Safety Integrity Level)?", "id": "SIL 1 Hingga SIL 4 (Tertinggi)." },
  { "en": "Apa Itu Analisis Bahaya Proses (PHA)?", "id": "Process Hazard Analysis (PHA)." },
  { "en": "Apa Kepanjangan PHA (Process Hazard Analysis)?", "id": "Analisis Bahaya Proses." },
  { "en": "Metode Apa Untuk PHA (Process Hazard Analysis)?", "id": "HAZOP (Hazard and Operability Study), FMEA (Failure Mode and Effects Analysis)." },
  { "en": "Apa Kepanjangan HAZOP (Hazard and Operability Study)?", "id": "Studi Bahaya Dan Operabilitas." },
  { "en": "Apa Itu Lapisan Proteksi (Layer of Protection)?", "id": "Berbagai Tingkatan Pengaman Proses." },
  { "en": "Apa Kepanjangan LOPA (Layer of Protection Analysis)?", "id": "Analisis Lapisan Proteksi." },
  { "en": "Apa Itu Alarm (Alarm) Sistem Kontrol?", "id": "Notifikasi Kondisi Abnormal Proses." },
  { "en": "Apa Itu Manajemen Alarm (Alarm Management)?", "id": "Praktik Mengelola Alarm Efektif." },
  { "en": "Apa Itu Banjir Alarm (Alarm Flood)?", "id": "Terlalu Banyak Alarm Aktif Bersamaan." },
  { "en": "Apa Itu Sistem Kontrol Batch (Batch Control)?", "id": "Kontrol Proses Produksi Per Batch." },
  { "en": "Standar Apa Untuk Kontrol Batch?", "id": "Standar ISA-88 (S88)." },
  { "en": "Apa Itu Sistem Eksekusi Manufaktur (MES)?", "id": "Manufacturing Execution System (MES)." },
  { "en": "Apa Kepanjangan MES (Manufacturing Execution System)?", "id": "Sistem Eksekusi Manufaktur." },
  { "en": "Apa Fungsi MES (Manufacturing Execution System)?", "id": "Menghubungkan ERP (Enterprise Resource Planning), Kontrol Pabrik." },
  { "en": "Apa Kepanjangan ERP (Enterprise Resource Planning)?", "id": "Perencanaan Sumber Daya Perusahaan." },
  { "en": "Apa Itu Otomatisasi Industri (Industrial Automation)?", "id": "Penggunaan Sistem Kontrol Otomatis Pabrik." },
  { "en": "Apa Itu Piramida Otomatisasi (Automation Pyramid)?", "id": "Model Tingkatan Sistem Otomatisasi." },
  { "en": "Sebutkan Tingkatan Piramida Otomatisasi?", "id": "Field, Control, Supervisory, Planning, Enterprise." },
  { "en": "Tingkat Apa Sensor, Aktuator Berada?", "id": "Tingkat Lapangan (Field Level)." },
  { "en": "Tingkat Apa PLC (Programmable Logic Controller), DCS (Distributed Control System) Berada?", "id": "Tingkat Kontrol (Control Level)." },
  { "en": "Tingkat Apa SCADA (Supervisory Control and Data Acquisition), HMI (Human Machine Interface) Berada?", "id": "Tingkat Pengawasan (Supervisory Level)." },
  { "en": "Tingkat Apa MES (Manufacturing Execution System) Berada?", "id": "Tingkat Perencanaan (Planning Level)." },
  { "en": "Tingkat Apa ERP (Enterprise Resource Planning) Berada?", "id": "Tingkat Perusahaan (Enterprise Level)." },
  { "en": "Apa Itu Industri 4.0 (Industry 4.0)?", "id": "Revolusi Industri Keempat (Digitalisasi)." },
  { "en": "Teknologi Apa Terkait Industri 4.0?", "id": "IoT (Internet of Things), AI (Artificial Intelligence), Big Data, Cloud." },
  { "en": "Apa Itu Kembar Digital (Digital Twin)?", "id": "Representasi Virtual Aset Fisik." },
  { "en": "Apa Fungsi Kembar Digital (Digital Twin)?", "id": "Simulasi, Pemantauan, Optimasi Proses." },
  { "en": "Apa Itu Keamanan Siber Industri (OT Security)?", "id": "Operational Technology (OT) Security." },
  { "en": "Apa Kepanjangan OT (Operational Technology)?", "id": "Teknologi Operasional." },
  { "en": "Apa Beda IT (Information Technology), OT (Operational Technology)?", "id": "IT (Fokus Data), OT (Fokus Proses Fisik)." },
  { "en": "Standar Keamanan Siber Apa Untuk OT?", "id": "Standar ISA/IEC 62443." },
  { "en": "Apa Itu Jaringan IT/OT Terkonvergensi?", "id": "Integrasi Jaringan Teknologi Informasi, Operasional." },
  { "en": "Apa Itu Air Gap (Celah Udara) Keamanan?", "id": "Memisahkan Jaringan Secara Fisik." },
  { "en": "Apa Itu Zona Demiliterisasi (DMZ)?", "id": "Demilitarized Zone (DMZ) (Jaringan Perantara)." },
  { "en": "Apa Kepanjangan DMZ (Demilitarized Zone)?", "id": "Zona Demiliterisasi." },
  { "en": "Apa Itu Intrusion Detection System (IDS)?", "id": "Sistem Deteksi Intrusi Jaringan." },
  { "en": "Apa Kepanjangan IDS (Intrusion Detection System)?", "id": "Sistem Deteksi Intrusi." },
  { "en": "Apa Itu Intrusion Prevention System (IPS)?", "id": "Sistem Pencegahan Intrusi Jaringan." },
  { "en": "Apa Kepanjangan IPS (Intrusion Prevention System)?", "id": "Sistem Pencegahan Intrusi." },
  { "en": "Apa Beda IDS (Intrusion Detection System), IPS (Intrusion Prevention System)?", "id": "IDS (Hanya Deteksi), IPS (Deteksi, Blokir)." },
  { "en": "Apa Itu SIEM (Security Information and Event Management)?", "id": "Manajemen Informasi, Kejadian Keamanan." },
  { "en": "Apa Kepanjangan SIEM (Security Information and Event Management)?", "id": "Manajemen Informasi Dan Kejadian Keamanan." },
  { "en": "Apa Fungsi SIEM (Security Information and Event Management)?", "id": "Analisis Log Keamanan Real-Time." },
  { "en": "Apa Itu Honeypot (Honeypot) Keamanan?", "id": "Sistem Jebakan Menarik Penyerang." },
  { "en": "Apa Itu Pemindaian Kerentanan (Vulnerability Scanning)?", "id": "Mencari Kelemahan Keamanan Sistem." },
  { "en": "Apa Itu Perangkat Lunak Antimalware (Antimalware)?", "id": "Perlindungan Terhadap Malware." },
  { "en": "Apa Itu Transformator Step-Up (Penaik Tegangan)?", "id": "Menaikkan Tegangan AC Primer Ke Sekunder." },
  { "en": "Apa Itu Transformator Step-Down (Penurun Tegangan)?", "id": "Menurunkan Tegangan AC Primer Ke Sekunder." },
  { "en": "Bagaimana Rasio Belitan (Turns Ratio) Trafo Step-Up?", "id": "Ns Lebih Besar Dari Np (a < 1)." },
  { "en": "Bagaimana Rasio Belitan (Turns Ratio) Trafo Step-Down?", "id": "Ns Lebih Kecil Dari Np (a > 1)." },
  { "en": "Apa Itu Rugi Tembaga (Copper Loss) Trafo?", "id": "Rugi Akibat Resistansi Belitan (I²R)." },
  { "en": "Apa Itu Rugi Inti (Core Loss) Trafo?", "id": "Rugi Histeresis Dan Arus Eddy." },
  { "en": "Kapan Rugi Inti (Core Loss) Trafo Terjadi?", "id": "Selama Trafo Bertegangan (Konstan)." },
  { "en": "Kapan Rugi Tembaga (Copper Loss) Trafo Berubah?", "id": "Berubah Sesuai Kuadrat Beban Arus." },
  { "en": "Apa Itu Efisiensi (Efficiency) Transformator?", "id": "Rasio Daya Output Terhadap Input." },
  { "en": "Kapan Efisiensi (Efficiency) Trafo Maksimal?", "id": "Saat Rugi Tembaga Sama Rugi Inti." },
  { "en": "Apa Itu Regulasi Tegangan (Voltage Regulation) Trafo?", "id": "Perubahan Tegangan Sekunder (Beban Penuh)." },
  { "en": "Regulasi Tegangan (Voltage Regulation) Rendah Berarti Apa?", "id": "Tegangan Sekunder Lebih Stabil." },
  { "en": "Apa Itu Autotransformator (Autotransformer)?", "id": "Transformator Dengan Satu Belitan Saja." },
  { "en": "Apa Keuntungan Autotransformator (Autotransformer)?", "id": "Lebih Kecil, Ringan, Efisien (Rasio Kecil)." },
  { "en": "Apa Kerugian Autotransformator (Autotransformer)?", "id": "Tidak Ada Isolasi Galvanik." },
  { "en": "Apa Itu Uji Sirkuit Terbuka (Open Circuit Test)?", "id": "Mengukur Rugi Inti, Parameter Shunt." },
  { "en": "Sisi Mana Diberi Tegangan Uji Sirkuit Terbuka?", "id": "Sisi Tegangan Rendah (LV)." },
  { "en": "Sisi Mana Terbuka Uji Sirkuit Terbuka?", "id": "Sisi Tegangan Tinggi (HV)." },
  { "en": "Apa Itu Uji Hubung Singkat (Short Circuit Test)?", "id": "Mengukur Rugi Tembaga, Parameter Seri." },
  { "en": "Sisi Mana Diberi Tegangan Uji Hubung Singkat?", "id": "Sisi Tegangan Tinggi (HV)." },
  { "en": "Sisi Mana Dihubung Singkat Uji Hubung Singkat?", "id": "Sisi Tegangan Rendah (LV)." },
  { "en": "Apa Itu Impedansi Persen (%Z) Trafo?", "id": "Ukuran Jatuh Tegangan Internal Trafo." },
  { "en": "Apa Fungsi Impedansi Persen (%Z)?", "id": "Perhitungan Arus Hubung Singkat." },
  { "en": "Apa Itu Operasi Paralel (Parallel Operation) Trafo?", "id": "Menghubungkan Dua Trafo Atau Lebih Paralel." },
  { "en": "Syarat Operasi Paralel (Parallel Operation) Trafo?", "id": "Rating Tegangan, Rasio, Polaritas, %Z Sama." },
  { "en": "Apa Itu Polaritas (Polarity) Trafo?", "id": "Arah Relatif Tegangan Induksi Sekunder." },
  { "en": "Bagaimana Tes Polaritas (Polarity Test) Trafo?", "id": "Menggunakan Voltmeter AC." },
  { "en": "Apa Itu Motor Induksi (Induction Motor) Tiga Fasa?", "id": "Motor AC Paling Umum Industri." },
  { "en": "Apa Prinsip Kerja Motor Induksi (Induction Motor)?", "id": "Induksi Elektromagnetik Medan Putar Stator." },
  { "en": "Apa Itu Medan Putar (Rotating Magnetic Field)?", "id": "Medan Magnet Berputar Dihasilkan Stator." },
  { "en": "Apa Itu Kecepatan Sinkron (Synchronous Speed) (Ns)?", "id": "Kecepatan Putaran Medan Magnet Stator." },
  { "en": "Apa Rumus Kecepatan Sinkron (Synchronous Speed) (Ns)?", "id": "Ns = (120 * f) / P." },
  { "en": "Apa Itu Slip (Slip) (s) Motor Induksi?", "id": "Perbedaan Kecepatan Sinkron, Kecepatan Rotor." },
  { "en": "Apa Rumus Slip (Slip) (s)?", "id": "s = (Ns - Nr) / Ns." },
  { "en": "Berapa Nilai Slip (Slip) Saat Start?", "id": "Satu (s = 1)." },
  { "en": "Berapa Nilai Slip (Slip) Saat Kecepatan Sinkron?", "id": "Nol (s = 0)." },
  { "en": "Apa Itu Rotor Sangkar Tupai (Squirrel Cage Rotor)?", "id": "Rotor Batang Konduktor Dihubung Singkat." },
  { "en": "Apa Itu Rotor Lilit (Wound Rotor)?", "id": "Rotor Dengan Belitan Terhubung Slip Ring." },
  { "en": "Apa Keuntungan Rotor Lilit (Wound Rotor)?", "id": "Kontrol Torsi Start, Kecepatan (Resistor)." },
  { "en": "Apa Itu Rangkaian Ekuivalen (Equivalent Circuit) Motor Induksi?", "id": "Model Rangkaian Analisis Kinerja Motor." },
  { "en": "Parameter Apa Ada Di Rangkaian Ekuivalen?", "id": "Resistansi, Reaktansi Stator, Rotor, Magnetisasi." },
  { "en": "Bagaimana Daya Mengalir Di Motor Induksi?", "id": "Input Listrik -> Rugi Stator -> Daya Celah Udara -> Rugi Rotor -> Mekanik." },
  { "en": "Apa Itu Daya Celah Udara (Air Gap Power)?", "id": "Daya Ditransfer Dari Stator Ke Rotor." },
  { "en": "Apa Itu Rugi Inti (Core Loss) Motor?", "id": "Rugi Histeresis, Arus Eddy Stator." },
  { "en": "Apa Itu Rugi Gesek, Angin (Friction, Windage)?", "id": "Rugi Mekanis Akibat Putaran Rotor." },
  { "en": "Apa Itu Kurva Torsi-Kecepatan (Torque-Speed Curve)?", "id": "Grafik Hubungan Torsi, Kecepatan Motor." },
  { "en": "Apa Itu Torsi Start (Starting Torque) Motor?", "id": "Torsi Saat Kecepatan Nol." },
  { "en": "Apa Itu Torsi Maksimum (Pull-Out Torque)?", "id": "Torsi Puncak Yang Dihasilkan Motor." },
  { "en": "Apa Itu Kelas Desain (Design Class) NEMA Motor?", "id": "Klasifikasi Karakteristik Torsi-Kecepatan (A,B,C,D)." },
  { "en": "Metode Start (Starting Method) Motor Induksi?", "id": "DOL, Star-Delta, Soft Starter, VFD." },
  { "en": "Apa Kepanjangan DOL (Direct On Line)?", "id": "Langsung Ke Jaringan." },
  { "en": "Apa Tujuan Start Star-Delta (Star-Delta Starting)?", "id": "Mengurangi Arus Start Motor." },
  { "en": "Apa Fungsi Soft Starter (Soft Starter)?", "id": "Start Motor Perlahan (Kurangi Tegangan)." },
  { "en": "Apa Fungsi VFD (Variable Frequency Drive)?", "id": "Kontrol Kecepatan Motor (Ubah Frekuensi)." },
  { "en": "Bagaimana Kontrol Kecepatan (Speed Control) Motor Induksi?", "id": "Mengubah Frekuensi (VFD), Resistansi Rotor." },
  { "en": "Apa Itu Pengereman (Braking) Motor Induksi?", "id": "Metode Menghentikan Putaran Motor Cepat." },
  { "en": "Metode Pengereman (Braking) Motor Induksi?", "id": "Plugging, Regeneratif, Dinamis, DC Injection." },
  { "en": "Apa Itu Plugging (Plugging) Pengereman?", "id": "Membalik Urutan Fasa (Hasilkan Torsi Balik)." },
  { "en": "Apa Itu Pengereman Regeneratif (Regenerative Braking)?", "id": "Motor Bertindak Generator (Kembalikan Energi)." },
  { "en": "Apa Itu Pengereman Dinamis (Dynamic Braking)?", "id": "Energi Pengereman Dibuang Ke Resistor." },
  { "en": "Apa Itu Pengereman Injeksi DC (DC Injection)?", "id": "Memberi Arus DC Ke Stator." },
  { "en": "Apa Itu Motor Sinkron (Synchronous Motor)?", "id": "Motor Kecepatan Rotor Sama Medan Putar." },
  { "en": "Bagaimana Start Motor Sinkron (Synchronous Motor)?", "id": "Start Induksi (Damper), Motor Bantu." },
  { "en": "Apa Itu Belitan Peredam (Damper/Amortisseur Winding)?", "id": "Belitan Bantu Start, Redam Osilasi." },
  { "en": "Apa Itu Eksitasi (Excitation) Motor Sinkron?", "id": "Pemberian Arus DC Ke Belitan Rotor." },
  { "en": "Apa Fungsi Eksitasi (Excitation)?", "id": "Membangkitkan Medan Magnet Rotor." },
  { "en": "Bagaimana Kontrol Faktor Daya (Power Factor) Motor Sinkron?", "id": "Mengatur Arus Eksitasi (Over/Under Excitation)." },
  { "en": "Apa Itu Kondisi Over-Excited (Eksitasi Berlebih)?", "id": "Motor Menyuplai VAR (Faktor Daya Leading)." },
  { "en": "Apa Itu Kondisi Under-Excited (Eksitasi Kurang)?", "id": "Motor Menyerap VAR (Faktor Daya Lagging)." },
  { "en": "Apa Itu Kurva V (V-Curve) Motor Sinkron?", "id": "Grafik Arus Jangkar Vs Arus Medan." },
  { "en": "Apa Itu Kondensor Sinkron (Synchronous Condenser)?", "id": "Motor Sinkron Tanpa Beban (Koreksi PF)." },
  { "en": "Apa Itu Generator Sinkron (Synchronous Generator)?", "id": "Pembangkit Listrik AC Utama (Alternator)." },
  { "en": "Apa Bagian Utama Generator Sinkron?", "id": "Stator (Jangkar), Rotor (Medan)." },
  { "en": "Belitan Apa Di Stator Generator Sinkron?", "id": "Belitan Jangkar (Output Listrik AC)." },
  { "en": "Belitan Apa Di Rotor Generator Sinkron?", "id": "Belitan Medan (Input Arus DC)." },
  { "en": "Apa Itu Rotor Kutub Menonjol (Salient Pole)?", "id": "Rotor Kecepatan Rendah (PLTA)." },
  { "en": "Apa Itu Rotor Silinder (Cylindrical Rotor)?", "id": "Rotor Kecepatan Tinggi (PLTU/PLTG)." },
  { "en": "Apa Itu Sistem Eksitasi (Excitation System) Generator?", "id": "Menyuplai Arus DC Ke Rotor Medan." },
  { "en": "Apa Itu Eksitasi Statis (Static Excitation)?", "id": "Menggunakan Penyearah Terkontrol (SCR)." },
  { "en": "Apa Itu Eksitasi Tanpa Sikat (Brushless Excitation)?", "id": "Menggunakan Generator AC Kecil Di Poros." },
  { "en": "Apa Itu AVR (Automatic Voltage Regulator) Generator?", "id": "Mengatur Tegangan Output Generator." },
  { "en": "Bagaimana AVR (Automatic Voltage Regulator) Mengatur Tegangan?", "id": "Mengontrol Arus Eksitasi Medan Rotor." },
  { "en": "Apa Itu Sinkronisasi (Synchronization) Generator?", "id": "Menghubungkan Generator Paralel Ke Jaringan." },
  { "en": "Syarat Sinkronisasi (Synchronization)?", "id": "Tegangan, Frekuensi, Fasa, Urutan Sama." },
  { "en": "Apa Itu Pembagian Beban (Load Sharing) Generator?", "id": "Distribusi Beban Aktif, Reaktif Paralel." },
  { "en": "Bagaimana Mengatur Pembagian Beban Aktif (P)?", "id": "Mengatur Input Penggerak Utama (Governor)." },
  { "en": "Bagaimana Mengatur Pembagian Beban Reaktif (Q)?", "id": "Mengatur Arus Eksitasi (AVR)." },
  { "en": "Apa Itu Kurva Kapabilitas (Capability Curve) Generator?", "id": "Batas Operasi Generator (MW, MVAR)." },
  { "en": "Apa Batas Stator (Stator Limit) Kurva Kapabilitas?", "id": "Batas Pemanasan Belitan Stator (MVA)." },
  { "en": "Apa Batas Rotor (Rotor Limit) Kurva Kapabilitas?", "id": "Batas Pemanasan Belitan Rotor (Eksitasi)." },
  { "en": "Apa Batas Stabilitas (Stability Limit) Kurva Kapabilitas?", "id": "Batas Operasi Stabil Generator." },
  { "en": "Apa Itu Motor DC (Direct Current)?", "id": "Motor Beroperasi Sumber Arus Searah." },
  { "en": "Apa Bagian Utama Motor DC (Direct Current)?", "id": "Stator (Medan), Rotor (Jangkar), Komutator." },
  { "en": "Apa Fungsi Komutator (Commutator) Motor DC?", "id": "Membalik Arah Arus Jangkar (Torsi Searah)." },
  { "en": "Apa Fungsi Sikat (Brush) Motor DC?", "id": "Menghantarkan Arus Ke Komutator." },
  { "en": "Apa Itu GGL Balik (Back EMF) Motor DC?", "id": "Tegangan Induksi Melawan Tegangan Sumber." },
  { "en": "Apa Hubungan GGL Balik (Back EMF), Kecepatan?", "id": "GGL Balik (Back EMF) Proporsional Kecepatan Putar." },
  { "en": "Apa Itu Motor DC (Direct Current) Shunt?", "id": "Belitan Medan Paralel Jangkar." },
  { "en": "Apa Karakteristik Motor DC (Direct Current) Shunt?", "id": "Kecepatan Relatif Konstan." },
  { "en": "Apa Itu Motor DC (Direct Current) Seri?", "id": "Belitan Medan Seri Jangkar." },
  { "en": "Apa Karakteristik Motor DC (Direct Current) Seri?", "id": "Torsi Start Sangat Tinggi." },
  { "en": "Dimana Motor DC (Direct Current) Seri Digunakan?", "id": "Traksi Listrik (Kereta, Derek)." },
  { "en": "Mengapa Motor DC (Direct Current) Seri Berbahaya Tanpa Beban?", "id": "Kecepatan Bisa Sangat Tinggi (Rusak)." },
  { "en": "Apa Itu Motor DC (Direct Current) Kompon?", "id": "Memiliki Belitan Medan Seri, Shunt." },
  { "en": "Apa Itu Motor DC (Direct Current) Kompon Kumulatif?", "id": "Medan Seri, Shunt Saling Memperkuat." },
  { "en": "Apa Itu Motor DC (Direct Current) Kompon Diferensial?", "id": "Medan Seri, Shunt Saling Melawan." },
  { "en": "Bagaimana Kontrol Kecepatan (Speed Control) Motor DC?", "id": "Ubah Tegangan Jangkar, Arus Medan, Resistansi." },
  { "en": "Apa Itu Kontrol Ward Leonard (Ward Leonard Control)?", "id": "Metode Kontrol Kecepatan Motor DC Presisi." },
  { "en": "Apa Itu Motor Universal (Universal Motor)?", "id": "Motor Seri Bisa Operasi AC/DC." },
  { "en": "Dimana Motor Universal (Universal Motor) Digunakan?", "id": "Peralatan Rumah Tangga (Bor, Blender)." },
  { "en": "Apa Itu Motor DC (Direct Current) Tanpa Sikat (BLDC)?", "id": "Brushless DC (Direct Current) Motor (Komutasi Elektronik)." },
  { "en": "Apa Itu Sensor Hall (Hall Sensor) Motor BLDC?", "id": "Mendeteksi Posisi Rotor Komutasi." },
  { "en": "Apa Itu Sensor Posisi (Position Sensor)?", "id": "Mendeteksi Lokasi Atau Perpindahan Objek." },
  { "en": "Contoh Sensor Posisi (Position Sensor) Linear?", "id": "Potensiometer Linear, LVDT (Linear Variable Differential Transformer)." },
  { "en": "Contoh Sensor Posisi (Position Sensor) Rotary?", "id": "Rotary Encoder, Potensiometer Rotary." },
  { "en": "Apa Itu Encoder (Encoder) Inkremental?", "id": "Menghasilkan Pulsa Relatif Terhadap Gerakan." },
  { "en": "Apa Itu Encoder (Encoder) Absolut?", "id": "Memberikan Kode Posisi Unik Absolut." },
  { "en": "Apa Itu Resolusi (Resolution) Encoder?", "id": "Jumlah Pulsa Atau Posisi Per Putaran." },
  { "en": "Apa Itu Saluran Kuadratur (Quadrature Output) Encoder?", "id": "Dua Output (A, B) Beda Fasa 90°." },
  { "en": "Apa Fungsi Saluran Kuadratur (Quadrature Output)?", "id": "Mendeteksi Arah Putaran Encoder." },
  { "en": "Apa Itu Indeks (Index) (Z) Encoder?", "id": "Satu Pulsa Referensi Per Putaran." },
  { "en": "Apa Itu Resolver (Resolver)?", "id": "Sensor Sudut Analog Elektromekanis." },
  { "en": "Apa Keunggulan Resolver (Resolver) Dibanding Encoder?", "id": "Lebih Tahan Lingkungan Keras." },
  { "en": "Apa Itu Tachometer (Tachometer)?", "id": "Sensor Pengukur Kecepatan Putaran (RPM)." },
  { "en": "Apa Itu Generator Tachometer (Tachogenerator)?", "id": "Generator Kecil Hasilkan Tegangan Proporsional RPM." },
  { "en": "Apa Itu Efek Piezoelektrik (Piezoelectric Effect) Terbalik?", "id": "Tegangan Listrik Hasilkan Deformasi Mekanis." },
  { "en": "Dimana Efek Piezoelektrik (Piezoelectric Effect) Terbalik Digunakan?", "id": "Aktuator Piezo, Buzzer Piezo." },
  { "en": "Apa Itu Aktuator Piezoelektrik (Piezo Actuator)?", "id": "Penggerak Presisi Gerakan Sangat Kecil." },
  { "en": "Apa Itu Motor Ultrasonik (Ultrasonic Motor)?", "id": "Motor Berbasis Getaran Piezoelektrik." },
  { "en": "Apa Itu Sensor Pyroelektrik (Pyroelectric Sensor)?", "id": "Sensor Deteksi Perubahan Radiasi Inframerah." },
  { "en": "Komponen Utama Sensor PIR (Passive Infrared)?", "id": "Sensor Pyroelektrik Dan Lensa Fresnel." },
  { "en": "Apa Fungsi Lensa Fresnel (Fresnel Lens)?", "id": "Memfokuskan Radiasi Inframerah Ke Sensor." },
  { "en": "Apa Itu Termopile (Thermopile)?", "id": "Sensor Suhu Inframerah (Banyak Termokopel)." },
  { "en": "Dimana Termopile (Thermopile) Digunakan?", "id": "Termometer Inframerah Non-Kontak." },
  { "en": "Apa Itu Bolometer (Bolometer)?", "id": "Sensor Radiasi Elektromagnetik (Ubah Resistansi)." },
  { "en": "Apa Itu Sel Fotoelektromagnetik (PEM Cell)?", "id": "Photoelectromagnetic (PEM) Cell Detektor Inframerah." },
  { "en": "Apa Itu Sel Fotokonduktif (Photoconductive Cell)?", "id": "Resistansi Berubah Terkena Cahaya (LDR)." },
  { "en": "Apa Itu Efek Fotovoltaik (Photovoltaic Effect)?", "id": "Cahaya Hasilkan Tegangan (Sel Surya)." },
  { "en": "Apa Itu Dioda Avalanche Photodiode (APD)?", "id": "Fotodioda Penguatan Internal (Sangat Sensitif)." },
  { "en": "Apa Kepanjangan APD (Avalanche Photodiode)?", "id": "Fotodioda Avalanche." },
  { "en": "Apa Itu Efisiensi Kuantum (Quantum Efficiency) (QE)?", "id": "Rasio Elektron Dihasilkan Per Foton." },
  { "en": "Apa Itu Responsivitas (Responsivity) Detektor Cahaya?", "id": "Arus Output Per Daya Cahaya Input." },
  { "en": "Apa Satuan Responsivitas (Responsivity)?", "id": "Ampere Per Watt (A/W)." },
  { "en": "Apa Itu Noise Equivalent Power (NEP)?", "id": "Daya Cahaya Minimum Dapat Dideteksi." },
  { "en": "Apa Kepanjangan NEP (Noise Equivalent Power)?", "id": "Daya Ekuivalen Derau." },
  { "en": "Apa Itu Detektivitas Spesifik (Specific Detectivity) (D*)?", "id": "Ukuran Kinerja Detektor Cahaya." },
  { "en": "Apa Itu Waktu Respon (Response Time) Detektor?", "id": "Seberapa Cepat Detektor Merespon Cahaya." },
  { "en": "Apa Itu LED (Light Emitting Diode) Organik (OLED)?", "id": "Organic Light Emitting Diode (OLED)." },
  { "en": "Apa Itu Tampilan Quantum Dot (QD Display)?", "id": "Tampilan Menggunakan Titik Kuantum." },
  { "en": "Apa Itu Elektroforesis (Electrophoresis)?", "id": "Pergerakan Partikel Bermuatan Medan Listrik." },
  { "en": "Prinsip Apa Digunakan Tampilan E-Ink?", "id": "Elektroforesis (Electrophoresis) Partikel Pigmen." },
  { "en": "Apa Itu Sistem Kontrol Linear (Linear Control)?", "id": "Sistem Kontrol Dideskripsikan Persamaan Linear." },
  { "en": "Apa Itu Sistem Kontrol Non-Linear (Non-Linear Control)?", "id": "Sistem Kontrol Dideskripsikan Persamaan Non-Linear." },
  { "en": "Mengapa Analisis Non-Linear (Non-Linear) Lebih Sulit?", "id": "Prinsip Superposisi Tidak Berlaku." },
  { "en": "Apa Itu Linierisasi (Linearization)?", "id": "Aproksimasi Sistem Non-Linear Menjadi Linear." },
  { "en": "Dimana Linierisasi (Linearization) Dilakukan?", "id": "Di Sekitar Titik Operasi Tertentu." },
  { "en": "Apa Itu Kontrol Bang-Bang (Bang-Bang Control)?", "id": "Kontroler Hanya Dua Keadaan (On/Off)." },
  { "en": "Apa Itu Kontrol Mode Geser (Sliding Mode Control)?", "id": "Teknik Kontrol Non-Linear Kuat (Robust)." },
  { "en": "Apa Itu Kontrol Adaptif (Adaptive Control)?", "id": "Parameter Kontroler Berubah Sesuai Sistem." },
  { "en": "Apa Itu Kontrol Kuat (Robust Control)?", "id": "Kontroler Tahan Terhadap Ketidakpastian Model." },
  { "en": "Apa Itu Kontrol Optimal (Optimal Control)?", "id": "Mencari Sinyal Kontrol Optimalkan Kinerja." },
  { "en": "Apa Itu Fungsi Biaya (Cost Function) Kontrol Optimal?", "id": "Ukuran Kinerja Yang Diminimalkan." },
  { "en": "Apa Itu Persamaan Hamilton-Jacobi-Bellman (HJB)?", "id": "Persamaan Dasar Teori Kontrol Optimal." },
  { "en": "Apa Itu Kontrol Prediktif Model (MPC)?", "id": "Model Predictive Control (MPC)." },
  { "en": "Apa Kepanjangan MPC (Model Predictive Control)?", "id": "Kontrol Prediktif Model." },
  { "en": "Bagaimana MPC (Model Predictive Control) Bekerja?", "id": "Menggunakan Model Prediksi Optimasi Kontrol." },
  { "en": "Apa Itu Estimasi Keadaan (State Estimation)?", "id": "Memperkirakan Variabel Internal Tak Terukur." },
  { "en": "Apa Itu Pengamat Luenberger (Luenberger Observer)?", "id": "Estimator Keadaan Sistem Linear." },
  { "en": "Apa Itu Filter Kalman (Kalman Filter)?", "id": "Estimator Keadaan Optimal (Ada Noise)." },
  { "en": "Apa Itu Filter Kalman Diperluas (EKF)?", "id": "Extended Kalman Filter (EKF) (Sistem Non-Linear)." },
  { "en": "Apa Kepanjangan EKF (Extended Kalman Filter)?", "id": "Filter Kalman Diperluas." },
  { "en": "Apa Itu Filter Kalman Tanpa Aroma (UKF)?", "id": "Unscented Kalman Filter (UKF) (Alternatif EKF)." },
  { "en": "Apa Kepanjangan UKF (Unscented Kalman Filter)?", "id": "Filter Kalman Tanpa Aroma." },
  { "en": "Apa Itu Filter Partikel (Particle Filter)?", "id": "Estimator Berbasis Simulasi Monte Carlo." },
  { "en": "Apa Itu Identifikasi Sistem (System Identification)?", "id": "Membangun Model Matematis Dari Data." },
  { "en": "Metode Apa Untuk Identifikasi Sistem (System Identification)?", "id": "Least Squares (Kuadrat Terkecil), ARMAX." },
  { "en": "Apa Kepanjangan ARMAX (Autoregressive Moving Average Exogenous)?", "id": "Model Runtun Waktu." },
  { "en": "Apa Itu Kecerdasan Buatan (Artificial Intelligence) (AI)?", "id": "Kemampuan Mesin Meniru Kecerdasan." },
  { "en": "Apa Itu Pembelajaran Mesin (Machine Learning) (ML)?", "id": "Mesin Belajar Dari Data." },
  { "en": "Apa Itu Pembelajaran Mendalam (Deep Learning) (DL)?", "id": "ML (Machine Learning) Menggunakan Jaringan Saraf Dalam." },
  { "en": "Apa Itu Jaringan Saraf Tiruan (ANN)?", "id": "Artificial Neural Network (ANN)." },
  { "en": "Apa Itu Neuron (Neuron) Tiruan?", "id": "Unit Pemroses Dasar ANN." },
  { "en": "Apa Itu Fungsi Aktivasi (Activation Function)?", "id": "Fungsi Non-Linear Output Neuron." },
  { "en": "Contoh Fungsi Aktivasi (Activation Function)?", "id": "Sigmoid, ReLU (Rectified Linear Unit), Tanh." },
  { "en": "Apa Kepanjangan ReLU (Rectified Linear Unit)?", "id": "Unit Linear Terektifikasi." },
  { "en": "Apa Itu Perceptron (Perceptron)?", "id": "Model Neuron Tiruan Paling Sederhana." },
  { "en": "Apa Itu Jaringan Maju (Feedforward Network)?", "id": "ANN (Artificial Neural Network) Koneksi Satu Arah." },
  { "en": "Apa Itu Jaringan Berulang (Recurrent Network) (RNN)?", "id": "ANN (Artificial Neural Network) Koneksi Membentuk Siklus." },
  { "en": "Apa Itu Backpropagation (Propagasi Balik)?", "id": "Algoritma Pelatihan Jaringan Saraf Tiruan." },
  { "en": "Apa Itu Pembelajaran Terbimbing (Supervised Learning)?", "id": "Belajar Dengan Data Berlabel (Input-Output)." },
  { "en": "Contoh Tugas Pembelajaran Terbimbing (Supervised Learning)?", "id": "Klasifikasi, Regresi." },
  { "en": "Apa Itu Klasifikasi (Classification)?", "id": "Memetakan Input Ke Kategori Diskrit." },
  { "en": "Apa Itu Regresi (Regression)?", "id": "Memetakan Input Ke Nilai Kontinu." },
  { "en": "Apa Itu Pembelajaran Tak Terbimbing (Unsupervised Learning)?", "id": "Belajar Pola Data Tak Berlabel." },
  { "en": "Contoh Tugas Pembelajaran Tak Terbimbing (Unsupervised Learning)?", "id": "Clustering (Pengelompokan), Reduksi Dimensi." },
  { "en": "Apa Itu Clustering (Pengelompokan)?", "id": "Mengelompokkan Data Mirip Bersama." },
  { "en": "Algoritma Clustering (Pengelompokan) Populer?", "id": "K-Means (K-Means)." },
  { "en": "Apa Itu Reduksi Dimensi (Dimensionality Reduction)?", "id": "Mengurangi Jumlah Fitur Data." },
  { "en": "Metode Reduksi Dimensi (Dimensionality Reduction) Populer?", "id": "PCA (Principal Component Analysis)." },
  { "en": "Apa Kepanjangan PCA (Principal Component Analysis)?", "id": "Analisis Komponen Utama." },
  { "en": "Apa Itu Pembelajaran Penguatan (Reinforcement Learning)?", "id": "Agen Belajar Lewat Interaksi Lingkungan." },
  { "en": "Apa Konsep Utama Pembelajaran Penguatan (Reinforcement Learning)?", "id": "Agen, Lingkungan, Aksi, State, Reward." },
  { "en": "Apa Itu Support Vector Machine (SVM)?", "id": "Algoritma Klasifikasi (Margin Maksimal)." },
  { "en": "Apa Kepanjangan SVM (Support Vector Machine)?", "id": "Mesin Vektor Pendukung." },
  { "en": "Apa Itu Decision Tree (Pohon Keputusan)?", "id": "Model Klasifikasi/Regresi Struktur Pohon." },
  { "en": "Apa Itu Random Forest (Hutan Acak)?", "id": "Ensemble (Kumpulan) Decision Tree." },
  { "en": "Apa Itu Gradient Boosting (Peningkatan Gradien)?", "id": "Teknik Ensemble (Boosting) Kuat." },
  { "en": "Apa Itu Natural Language Processing (NLP)?", "id": "Pemrosesan Bahasa Alami." },
  { "en": "Apa Kepanjangan NLP (Natural Language Processing)?", "id": "Pemrosesan Bahasa Alami." },
  { "en": "Apa Itu Computer Vision (Visi Komputer)?", "id": "Pemahaman Komputer Terhadap Gambar, Video." },
  { "en": "Apa Itu Convolutional Neural Network (CNN)?", "id": "Jaringan Saraf Konvolusional (Untuk Visi)." },
  { "en": "Apa Itu Recurrent Neural Network (RNN)?", "id": "Jaringan Saraf Berulang (Data Sekuensial)." },
  { "en": "Apa Itu Long Short-Term Memory (LSTM)?", "id": "Jenis RNN (Recurrent Neural Network) Atasi Vanishing Gradient." },
  { "en": "Apa Kepanjangan LSTM (Long Short-Term Memory)?", "id": "Memori Jangka Panjang Pendek." },
  { "en": "Apa Itu Transformer (Transformer) Model?", "id": "Arsitektur Jaringan Saraf (Perhatian)." },
  { "en": "Dimana Model Transformer (Transformer) Populer?", "id": "NLP (Natural Language Processing) (GPT, BERT)." },
  { "en": "Apa Itu Generative Adversarial Network (GAN)?", "id": "Model Generatif (Generator, Diskriminator)." },
  { "en": "Apa Kepanjangan GAN (Generative Adversarial Network)?", "id": "Jaringan Adversarial Generatif." },
  { "en": "Apa Fungsi Generator (Generator) GAN?", "id": "Menghasilkan Data Sintetis Mirip Asli." },
  { "en": "Apa Fungsi Diskriminator (Discriminator) GAN?", "id": "Membedakan Data Asli Dan Sintetis." },
  { "en": "Apa Itu Overfitting (Overfitting) Model ML?", "id": "Model Terlalu Pas Data Latih (Generalisasi Buruk)." },
  { "en": "Apa Itu Underfitting (Underfitting) Model ML?", "id": "Model Terlalu Sederhana (Tidak Tangkap Pola)." },
  { "en": "Bagaimana Mencegah Overfitting (Overfitting)?", "id": "Regularisasi, Validasi Silang, Data Lebih Banyak." },
  { "en": "Apa Itu Regularisasi (Regularization) ML?", "id": "Teknik Mengurangi Kompleksitas Model." },
  { "en": "Apa Itu Validasi Silang (Cross-Validation)?", "id": "Metode Evaluasi Model (Bagi Data Latih)." },
  { "en": "Apa Itu Data Latih (Training Data)?", "id": "Data Digunakan Melatih Model ML." },
  { "en": "Apa Itu Data Validasi (Validation Data)?", "id": "Data Penyetelan Hiperparameter Model." },
  { "en": "Apa Itu Data Uji (Test Data)?", "id": "Data Evaluasi Kinerja Akhir Model." },
  { "en": "Apa Itu Hiperparameter (Hyperparameter) Model?", "id": "Parameter Diatur Sebelum Pelatihan (Learning Rate)." },
  { "en": "Apa Itu Learning Rate (Laju Pembelajaran)?", "id": "Ukuran Langkah Penyesuaian Bobot Model." },
  { "en": "Apa Itu Fungsi Kerugian (Loss Function)?", "id": "Ukuran Kesalahan Prediksi Model." },
  { "en": "Apa Itu Optimasi Gradien Turun (Gradient Descent)?", "id": "Algoritma Minimalkan Fungsi Kerugian." },
  { "en": "Apa Itu Epoch (Epoch) Pelatihan?", "id": "Satu Kali Iterasi Lewati Seluruh Data Latih." },
  { "en": "Apa Itu Ukuran Batch (Batch Size)?", "id": "Jumlah Sampel Data Diproses Satu Iterasi." },
  { "en": "Apa Itu Stochastic Gradient Descent (SGD)?", "id": "Gradien Turun Menggunakan Satu Sampel." },
  { "en": "Apa Kepanjangan SGD (Stochastic Gradient Descent)?", "id": "Gradien Turun Stokastik." },
  { "en": "Apa Itu Mini-Batch Gradient Descent?", "id": "Gradien Turun Menggunakan Batch Kecil." },
  { "en": "Apa Itu Feature Engineering (Rekayasa Fitur)?", "id": "Membuat Fitur Baru Dari Data Asli." },
  { "en": "Apa Itu Seleksi Fitur (Feature Selection)?", "id": "Memilih Fitur Paling Relevan." },
  { "en": "Apa Itu Normalisasi (Normalization) Data?", "id": "Mengubah Skala Data Ke Rentang Tertentu (0-1)." },
  { "en": "Apa Itu Standardisasi (Standardization) Data?", "id": "Mengubah Data (Mean 0, Standar Deviasi 1)." },
  { "en": "Apa Itu One-Hot Encoding (Pengkodean One-Hot)?", "id": "Representasi Variabel Kategori Jadi Biner." },
  { "en": "Apa Itu Confusion Matrix (Matriks Kebingungan)?", "id": "Tabel Evaluasi Kinerja Klasifikasi." },
  { "en": "Apa Itu Akurasi (Accuracy) Klasifikasi?", "id": "Persentase Prediksi Benar Total." },
  { "en": "Apa Itu Presisi (Precision) Klasifikasi?", "id": "Proporsi Prediksi Positif Yang Benar." },
  { "en": "Apa Itu Recall (Recall) (Sensitivitas)?", "id": "Proporsi Positif Aktual Teridentifikasi Benar." },
  { "en": "Apa Itu F1-Score (Skor F1)?", "id": "Rata-Rata Harmonik Presisi, Recall." },
  { "en": "Apa Itu Kurva ROC (Receiver Operating Characteristic)?", "id": "Grafik Kinerja Klasifikasi (TPR vs FPR)." },
  { "en": "Apa Kepanjangan ROC (Receiver Operating Characteristic)?", "id": "Karakteristik Operasi Penerima." },
  { "en": "Apa Itu Area Di Bawah Kurva (AUC) ROC?", "id": "Ukuran Kinerja Klasifikasi (0-1)." },
  { "en": "Apa Kepanjangan AUC (Area Under the Curve)?", "id": "Area Di Bawah Kurva." },
  { "en": "Apa Itu Mean Squared Error (MSE)?", "id": "Ukuran Error Umum Regresi." },
  { "en": "Apa Kepanjangan MSE (Mean Squared Error)?", "id": "Rata-Rata Kesalahan Kuadrat." },
  { "en": "Apa Itu Root Mean Squared Error (RMSE)?", "id": "Akar Kuadrat MSE (Skala Sama Target)." },
  { "en": "Apa Kepanjangan RMSE (Root Mean Squared Error)?", "id": "Akar Rata-Rata Kesalahan Kuadrat." },
  { "en": "Apa Itu Mean Absolute Error (MAE)?", "id": "Rata-Rata Kesalahan Absolut Regresi." },
  { "en": "Apa Kepanjangan MAE (Mean Absolute Error)?", "id": "Rata-Rata Kesalahan Absolut." },
  { "en": "Apa Itu R-Squared (Koefisien Determinasi)?", "id": "Ukuran Seberapa Baik Model Cocok Data." },
  { "en": "Apa Itu Transfer Learning (Pembelajaran Transfer)?", "id": "Menggunakan Model Terlatih Tugas Lain." },
  { "en": "Apa Itu Ensemble Learning (Pembelajaran Ensemble)?", "id": "Menggabungkan Prediksi Banyak Model." },
  { "en": "Contoh Metode Ensemble (Ensemble Method)?", "id": "Bagging, Boosting, Stacking." },
  { "en": "Apa Itu Bagging (Bootstrap Aggregating)?", "id": "Ensemble (Melatih Model Data Bootstrap)." },
  { "en": "Apa Itu Boosting (Peningkatan)?", "id": "Ensemble (Melatih Model Secara Sekuensial)." },
  { "en": "Apa Itu TensorFlow (TensorFlow)?", "id": "Pustaka (Library) Machine Learning Google." },
  { "en": "Apa Itu PyTorch (PyTorch)?", "id": "Pustaka (Library) Machine Learning Facebook." },
  { "en": "Apa Itu Scikit-learn (Scikit-learn)?", "id": "Pustaka (Library) Machine Learning Python Populer." },
  { "en": "Apa Itu Keras (Keras)?", "id": "API (Application Programming Interface) Jaringan Saraf Tingkat Tinggi." },
  { "en": "Apa Itu Jupyter Notebook (Jupyter Notebook)?", "id": "Lingkungan Komputasi Interaktif Web." },
  { "en": "Apa Itu Pandas (Pandas)?", "id": "Pustaka (Library) Analisis, Manipulasi Data Python." },
  { "en": "Apa Itu NumPy (NumPy)?", "id": "Pustaka (Library) Komputasi Numerik Python." },
  { "en": "Apa Itu Matplotlib (Matplotlib)?", "id": "Pustaka (Library) Visualisasi Data Python." },
  { "en": "Apa Itu Seaborn (Seaborn)?", "id": "Pustaka (Library) Visualisasi Data Tingkat Tinggi." },
  { "en": "Apa Itu Data Mining (Penambangan Data)?", "id": "Menemukan Pola Tersembunyi Data Besar." },
  { "en": "Apa Itu Big Data (Big Data)?", "id": "Data Sangat Besar, Kompleks, Cepat." },
  { "en": "Apa Itu Tiga V (V) Big Data Awal?", "id": "Volume (Volume), Velocity (Kecepatan), Variety (Variasi)." },
  { "en": "Apa Itu Hadoop (Hadoop)?", "id": "Kerangka Kerja Pemrosesan Big Data Terdistribusi." },
  { "en": "Apa Itu MapReduce (MapReduce)?", "id": "Model Pemrograman Pemrosesan Data Paralel." },
  { "en": "Apa Itu Spark (Apache Spark)?", "id": "Mesin Pemrosesan Big Data Cepat." },
  { "en": "Apa Itu Basis Data Kolom (Columnar Database)?", "id": "Menyimpan Data Per Kolom (Analitik Cepat)." },
  { "en": "Apa Itu Gudang Data (Data Warehouse)?", "id": "Penyimpanan Data Terpusat Analisis Bisnis." },
  { "en": "Apa Itu ETL (Extract Transform Load)?", "id": "Proses Integrasi Data Gudang Data." },
  { "en": "Apa Kepanjangan ETL (Extract Transform Load)?", "id": "Ekstrak, Transformasi, Muat." },
  { "en": "Apa Itu Business Intelligence (BI)?", "id": "Kecerdasan Bisnis (Analisis Data Keputusan)." },
  { "en": "Apa Kepanjangan BI (Business Intelligence)?", "id": "Kecerdasan Bisnis." },
  { "en": "Apa Itu Dasbor (Dashboard) BI?", "id": "Visualisasi Data Ringkasan Kinerja Bisnis." },
  { "en": "Apa Itu Visualisasi Data (Data Visualization)?", "id": "Representasi Grafis Data Informasi." },
  { "en": "Jenis Grafik (Chart) Apa Untuk Tren Waktu?", "id": "Grafik Garis (Line Chart)." },
  { "en": "Jenis Grafik (Chart) Apa Untuk Perbandingan Kategori?", "id": "Grafik Batang (Bar Chart)." },
  { "en": "Jenis Grafik (Chart) Apa Untuk Proporsi Keseluruhan?", "id": "Grafik Pai (Pie Chart)." },
  { "en": "Jenis Grafik (Chart) Apa Untuk Hubungan Dua Variabel?", "id": "Grafik Sebar (Scatter Plot)." },
  { "en": "Apa Itu Histogram (Histogram)?", "id": "Grafik Distribusi Frekuensi Data." },
  { "en": "Apa Itu Box Plot (Diagram Kotak)?", "id": "Visualisasi Ringkasan Statistik Data." },
  { "en": "Apa Itu Heatmap (Peta Panas)?", "id": "Visualisasi Matriks Data Warna." },
  { "en": "Apa Itu Ilmu Data (Data Science)?", "id": "Bidang Interdisipliner Ekstraksi Pengetahuan Data." },
  { "en": "Apa Peran Seorang Ilmuwan Data (Data Scientist)?", "id": "Menganalisis Data, Membangun Model Prediktif." },
  { "en": "Apa Peran Seorang Analis Data (Data Analyst)?", "id": "Mengumpulkan, Membersihkan, Menganalisis Data." },
  { "en": "Apa Peran Seorang Insinyur Data (Data Engineer)?", "id": "Membangun, Memelihara Infrastruktur Data." },
  { "en": "Apa Itu Pembersihan Data (Data Cleaning)?", "id": "Memperbaiki, Menghapus Data Kotor/Salah." },
  { "en": "Apa Itu Data Hilang (Missing Data)?", "id": "Nilai Data Yang Tidak Tersedia." },
  { "en": "Bagaimana Menangani Data Hilang (Missing Data)?", "id": "Imputasi (Pengisian) Atau Penghapusan." },
  { "en": "Apa Itu Outlier (Pencilan) Data?", "id": "Nilai Data Jauh Berbeda Dari Lainnya." },
  { "en": "Apa Itu Etika Data (Data Ethics)?", "id": "Prinsip Moral Pengumpulan, Penggunaan Data." },
  { "en": "Apa Itu Privasi Data (Data Privacy)?", "id": "Perlindungan Informasi Pribadi Individu." },
  { "en": "Apa Itu GDPR (General Data Protection Regulation)?", "id": "Regulasi Privasi Data Uni Eropa." },
  { "en": "Apa Kepanjangan GDPR (General Data Protection Regulation)?", "id": "Regulasi Perlindungan Data Umum." },
  { "en": "Apa Itu Bias (Bias) Algoritma?", "id": "Hasil Sistematis Tidak Adil Algoritma." },
  { "en": "Apa Itu Penjelasan AI (Explainable AI) (XAI)?", "id": "Membuat Keputusan Model AI Dapat Dimengerti." },
  { "en": "Apa Kepanjangan XAI (Explainable Artificial Intelligence)?", "id": "Kecerdasan Buatan Yang Dapat Dijelaskan." },
  { "en": "Apa Itu Komputasi Awan (Cloud Computing)?", "id": "Layanan Komputasi Lewat Internet." },
  { "en": "Apa Itu IaaS (Infrastructure as a Service)?", "id": "Infrastruktur Komputasi Sewaan (VM, Storage)." },
  { "en": "Apa Kepanjangan IaaS (Infrastructure as a Service)?", "id": "Infrastruktur Sebagai Layanan." },
  { "en": "Apa Itu PaaS (Platform as a Service)?", "id": "Platform Pengembangan, Deployment Aplikasi." },
  { "en": "Apa Kepanjangan PaaS (Platform as a Service)?", "id": "Platform Sebagai Layanan." },
  { "en": "Apa Itu SaaS (Software as a Service)?", "id": "Perangkat Lunak Berbasis Langganan Web." },
  { "en": "Apa Kepanjangan SaaS (Software as a Service)?", "id": "Perangkat Lunak Sebagai Layanan." },
  { "en": "Contoh Penyedia Layanan Awan (Cloud Provider)?", "id": "AWS (Amazon Web Services), Azure, GCP (Google Cloud Platform)." },
  { "en": "Apa Itu Komputasi Tanpa Server (Serverless Computing)?", "id": "Eksekusi Kode Tanpa Kelola Server." },
  { "en": "Apa Itu Function as a Service (FaaS)?", "id": "Model Komputasi Tanpa Server (Event-Driven)." },
  { "en": "Apa Kepanjangan FaaS (Function as a Service)?", "id": "Fungsi Sebagai Layanan." },
  { "en": "Contoh Layanan FaaS (Function as a Service)?", "id": "AWS Lambda, Google Cloud Functions." },
  { "en": "Apa Itu Devops (Development and Operations)?", "id": "Praktik Kolaborasi Tim Pengembangan, Operasi." },
  { "en": "Apa Tujuan Devops (Development and Operations)?", "id": "Mempercepat Siklus Rilis Perangkat Lunak." },
  { "en": "Apa Itu Infrastruktur Sebagai Kode (IaC)?", "id": "Infrastructure as Code (IaC)." },
  { "en": "Apa Kepanjangan IaC (Infrastructure as Code)?", "id": "Infrastruktur Sebagai Kode." },
  { "en": "Apa Fungsi IaC (Infrastructure as Code)?", "id": "Mengelola Infrastruktur IT Lewat Kode." },
  { "en": "Alat Apa Untuk IaC (Infrastructure as Code)?", "id": "Terraform, Ansible, Chef, Puppet." },
  { "en": "Apa Itu Terraform (Terraform)?", "id": "Alat Provisioning Infrastruktur Open Source." },
  { "en": "Apa Itu Ansible (Ansible)?", "id": "Alat Otomatisasi Konfigurasi, Deployment." },
  { "en": "Apa Itu Pemantauan (Monitoring) Sistem?", "id": "Mengamati Kinerja, Kesehatan Sistem IT." },
  { "en": "Alat Pemantauan (Monitoring) Populer?", "id": "Prometheus, Grafana, Nagios, Zabbix." },
  { "en": "Apa Itu Pencatatan Log (Logging)?", "id": "Merekam Kejadian (Event) Sistem." },
  { "en": "Apa Itu Manajemen Log Terpusat (Centralized Logging)?", "id": "Mengumpulkan Log Dari Banyak Sumber." },
  { "en": "Alat Manajemen Log Populer?", "id": "ELK Stack (Elasticsearch, Logstash, Kibana)." },
  { "en": "Apa Kepanjangan ELK (Elasticsearch, Logstash, Kibana)?", "id": "Elasticsearch, Logstash, Kibana." },
  { "en": "Apa Itu Elasticsearch (Elasticsearch)?", "id": "Mesin Pencari, Analitik Data Log." },
  { "en": "Apa Itu Logstash (Logstash)?", "id": "Pipeline Pemrosesan Data Log." },
  { "en": "Apa Itu Kibana (Kibana)?", "id": "Antarmuka Visualisasi Data Log." },
  { "en": "Apa Itu Pelacakan Terdistribusi (Distributed Tracing)?", "id": "Melacak Permintaan Lewat Microservices." },
  { "en": "Alat Pelacakan Terdistribusi (Distributed Tracing)?", "id": "Jaeger, Zipkin." },
  { "en": "Apa Itu Site Reliability Engineering (SRE)?", "id": "Pendekatan Rekayasa Keandalan Operasi." },
  { "en": "Apa Kepanjangan SRE (Site Reliability Engineering)?", "id": "Rekayasa Keandalan Situs." },
  { "en": "Apa Konsep Utama SRE (Site Reliability Engineering)?", "id": "SLO (Service Level Objective), Error Budget." },
  { "en": "Apa Kepanjangan SLO (Service Level Objective)?", "id": "Tujuan Tingkat Layanan." },
  { "en": "Apa Itu Error Budget (Anggaran Kesalahan)?", "id": "Batas Toleransi Ketidakandalan Layanan." },
  { "en": "Apa Itu Postmortem (Postmortem) Insiden?", "id": "Analisis Penyebab Insiden (Blameless)." },
  { "en": "Apa Itu Chaos Engineering (Rekayasa Kekacauan)?", "id": "Menguji Ketahanan Sistem Eksperimen Gagal." },
  { "en": "Apa Itu Jaringan Pengiriman Konten (CDN)?", "id": "Content Delivery Network (CDN)." },
  { "en": "Apa Kepanjangan CDN (Content Delivery Network)?", "id": "Jaringan Pengiriman Konten." },
  { "en": "Apa Fungsi CDN (Content Delivery Network)?", "id": "Mempercepat Pengiriman Konten Web Global." },
  { "en": "Bagaimana CDN (Content Delivery Network) Bekerja?", "id": "Menyimpan Cache Konten Server Terdistribusi." },
  { "en": "Apa Itu Edge Location (Lokasi Tepi) CDN?", "id": "Server CDN (Content Delivery Network) Dekat Pengguna Akhir." },
  { "en": "Apa Itu DNS (Domain Name System)?", "id": "Sistem Penamaan Domain Internet." },
  { "en": "Apa Fungsi Utama DNS (Domain Name System)?", "id": "Menerjemahkan Nama Domain Ke Alamat IP." },
  { "en": "Apa Itu Server DNS (Domain Name System) Rekursif?", "id": "Server Menerima Kueri Dari Klien." },
  { "en": "Apa Itu Server DNS (Domain Name System) Otoritatif?", "id": "Server Menyimpan Catatan DNS (Domain Name System) Domain." },
  { "en": "Apa Itu Catatan DNS (Domain Name System) A?", "id": "Memetakan Nama Domain Ke Alamat IPv4." },
  { "en": "Apa Itu Catatan DNS (Domain Name System) AAAA?", "id": "Memetakan Nama Domain Ke Alamat IPv6." },
  { "en": "Apa Itu Catatan DNS (Domain Name System) CNAME?", "id": "Memetakan Alias Nama Domain Lain." },
  { "en": "Apa Itu Catatan DNS (Domain Name System) MX?", "id": "Menentukan Server Email Domain." },
  { "en": "Apa Itu Catatan DNS (Domain Name System) TXT?", "id": "Menyimpan Teks (Verifikasi Domain)." },
  { "en": "Apa Itu Time To Live (TTL) DNS?", "id": "Durasi Cache Catatan DNS." },
  { "en": "Apa Kepanjangan TTL (Time To Live)?", "id": "Waktu Untuk Hidup." },
  { "en": "Apa Itu Penyebaran DNS (DNS Propagation)?", "id": "Waktu Perubahan DNS Menyebar Global." },
  { "en": "Apa Itu DNSSEC (Domain Name System Security Extensions)?", "id": "Ekstensi Keamanan Verifikasi DNS." },
  { "en": "Apa Kepanjangan DNSSEC (Domain Name System Security Extensions)?", "id": "Ekstensi Keamanan Sistem Penamaan Domain." },
  { "en": "Apa Itu HTTP (Hypertext Transfer Protocol)?", "id": "Protokol Komunikasi Web." },
  { "en": "Apa Itu HTTPS (Hypertext Transfer Protocol Secure)?", "id": "Versi Aman HTTP (Hypertext Transfer Protocol) (Enkripsi)." },
  { "en": "Protokol Enkripsi Apa Digunakan HTTPS?", "id": "SSL (Secure Sockets Layer)/TLS (Transport Layer Security)." },
  { "en": "Apa Kepanjangan SSL (Secure Sockets Layer)?", "id": "Lapisan Soket Aman." },
  { "en": "Apa Kepanjangan TLS (Transport Layer Security)?", "id": "Keamanan Lapisan Transportasi." },
  { "en": "Apa Itu Sertifikat SSL/TLS (SSL/TLS Certificate)?", "id": "Verifikasi Identitas Server Web." },
  { "en": "Siapa Penerbit Sertifikat SSL/TLS?", "id": "Otoritas Sertifikat (Certificate Authority - CA)." },
  { "en": "Apa Itu Metode Permintaan (Request Method) HTTP?", "id": "GET, POST, PUT, DELETE, dll." },
  { "en": "Apa Fungsi Metode GET HTTP?", "id": "Meminta Sumber Daya Dari Server." },
  { "en": "Apa Fungsi Metode POST HTTP?", "id": "Mengirim Data Ke Server (Buat Baru)." },
  { "en": "Apa Fungsi Metode PUT HTTP?", "id": "Mengirim Data Ke Server (Ganti/Buat)." },
  { "en": "Apa Fungsi Metode DELETE HTTP?", "id": "Menghapus Sumber Daya Di Server." },
  { "en": "Apa Itu Kode Status (Status Code) HTTP?", "id": "Kode Tiga Digit Respon Server." },
  { "en": "Apa Arti Kode Status 2xx?", "id": "Sukses (Contoh: 200 OK)." },
  { "en": "Apa Arti Kode Status 3xx?", "id": "Pengalihan (Redirect) (Contoh: 301 Moved Permanently)." },
  { "en": "Apa Arti Kode Status 4xx?", "id": "Kesalahan Klien (Contoh: 404 Not Found)." },
  { "en": "Apa Arti Kode Status 5xx?", "id": "Kesalahan Server (Contoh: 500 Internal Server Error)." },
  { "en": "Apa Itu Cookie (Cookie) HTTP?", "id": "Data Kecil Disimpan Browser (Sisi Klien)." },
  { "en": "Apa Fungsi Cookie (Cookie) HTTP?", "id": "Menyimpan Status Sesi, Preferensi Pengguna." },
  { "en": "Apa Itu Sesi (Session) HTTP?", "id": "Mekanisme Pelacakan Status Pengguna (Sisi Server)." },
  { "en": "Apa Itu Header (Header) HTTP?", "id": "Metadata Tambahan Permintaan/Respon." },
  { "en": "Apa Itu Cache (Cache) Browser?", "id": "Penyimpanan Sementara Sumber Daya Web." },
  { "en": "Apa Manfaat Cache (Cache) Browser?", "id": "Mempercepat Pemuatan Halaman Web." },
  { "en": "Apa Itu Web Server (Server Web)?", "id": "Perangkat Lunak Melayani Permintaan HTTP." },
  { "en": "Contoh Web Server (Server Web) Populer?", "id": "Apache, Nginx, Microsoft IIS." },
  { "en": "Apa Itu Load Balancer (Penyeimbang Beban)?", "id": "Mendistribusikan Lalu Lintas Ke Banyak Server." },
  { "en": "Apa Itu Reverse Proxy (Proxy Terbalik)?", "id": "Server Perantara Antara Klien, Server Web." },
  { "en": "Fungsi Reverse Proxy (Proxy Terbalik)?", "id": "Load Balancing, Caching, Keamanan." },
  { "en": "Apa Itu Bahasa Pemrograman Sisi Server (Server-Side)?", "id": "Kode Dieksekusi Di Server Web." },
  { "en": "Contoh Bahasa Sisi Server (Server-Side)?", "id": "PHP, Python (Django/Flask), Node.js, Ruby." },
  { "en": "Apa Itu Bahasa Pemrograman Sisi Klien (Client-Side)?", "id": "Kode Dieksekusi Di Browser Pengguna." },
  { "en": "Contoh Bahasa Sisi Klien (Client-Side)?", "id": "JavaScript." },
  { "en": "Apa Itu HTML (HyperText Markup Language)?", "id": "Bahasa Markup Struktur Konten Web." },
  { "en": "Apa Kepanjangan HTML (HyperText Markup Language)?", "id": "Bahasa Markup Hiperteks." },
  { "en": "Apa Itu CSS (Cascading Style Sheets)?", "id": "Bahasa Styling (Tampilan) Halaman Web." },
  { "en": "Apa Kepanjangan CSS (Cascading Style Sheets)?", "id": "Lembar Gaya Bertingkat." },
  { "en": "Apa Itu JavaScript (JavaScript)?", "id": "Bahasa Skrip Interaktivitas Halaman Web." },
  { "en": "Apa Itu DOM (Document Object Model)?", "id": "Representasi Struktur HTML/XML Objek." },
  { "en": "Apa Kepanjangan DOM (Document Object Model)?", "id": "Model Objek Dokumen." },
  { "en": "Apa Fungsi DOM (Document Object Model)?", "id": "JavaScript Memanipulasi Konten, Struktur Halaman." },
  { "en": "Apa Itu AJAX (Asynchronous JavaScript and XML)?", "id": "Teknik Update Sebagian Halaman Web." },
  { "en": "Apa Kepanjangan AJAX (Asynchronous JavaScript and XML)?", "id": "JavaScript Dan XML Asinkron." },
  { "en": "Apa Itu Framework (Kerangka Kerja) JavaScript?", "id": "Kumpulan Kode Bantu Pengembangan Web." },
  { "en": "Contoh Framework (Kerangka Kerja) JavaScript Frontend?", "id": "React, Angular, Vue.js." },
  { "en": "Contoh Framework (Kerangka Kerja) JavaScript Backend?", "id": "Node.js (Express.js)." },
  { "en": "Apa Itu Node.js (Node.js)?", "id": "Lingkungan Runtime JavaScript Sisi Server." },
  { "en": "Apa Itu NPM (Node Package Manager)?", "id": "Manajer Paket Ekosistem Node.js." },
  { "en": "Apa Kepanjangan NPM (Node Package Manager)?", "id": "Manajer Paket Node." },
  { "en": "Apa Itu Single Page Application (SPA)?", "id": "Aplikasi Web Muat Satu Halaman HTML." },
  { "en": "Apa Kepanjangan SPA (Single Page Application)?", "id": "Aplikasi Halaman Tunggal." },
  { "en": "Apa Itu Progressive Web App (PWA)?", "id": "Aplikasi Web Fitur Mirip Aplikasi Native." },
  { "en": "Apa Kepanjangan PWA (Progressive Web App)?", "id": "Aplikasi Web Progresif." },
  { "en": "Apa Itu WebAssembly (Wasm)?", "id": "Format Biner Efisien Kode Web." },
  { "en": "Apa Kepanjangan Wasm (WebAssembly)?", "id": "Rakitan Web." },
  { "en": "Apa Itu Responsive Web Design (Desain Web Responsif)?", "id": "Desain Web Adaptasi Ukuran Layar." },
  { "en": "Apa Itu Mobile-First Design (Desain Mobile-First)?", "id": "Mendesain Web Mulai Ukuran Mobile." },
  { "en": "Apa Itu Cross-Browser Compatibility (Kompatibilitas Lintas Browser)?", "id": "Memastikan Web Tampil Baik Berbagai Browser." },
  { "en": "Apa Itu Aksesibilitas Web (Web Accessibility)?", "id": "Membuat Web Dapat Diakses Semua Orang." },
  { "en": "Standar Aksesibilitas Web (Web Accessibility)?", "id": "WCAG (Web Content Accessibility Guidelines)." },
  { "en": "Apa Kepanjangan WCAG (Web Content Accessibility Guidelines)?", "id": "Pedoman Aksesibilitas Konten Web." },
  { "en": "Apa Itu Search Engine Optimization (SEO)?", "id": "Optimasi Situs Web Mesin Pencari." },
  { "en": "Apa Kepanjangan SEO (Search Engine Optimization)?", "id": "Optimasi Mesin Pencari." },
  { "en": "Apa Itu Kata Kunci (Keyword) SEO?", "id": "Istilah Dicari Pengguna Mesin Pencari." },
  { "en": "Apa Itu Backlink (Tautan Balik) SEO?", "id": "Tautan Dari Situs Web Lain Ke Situs Anda." },
  { "en": "Apa Itu Analitik Web (Web Analytics)?", "id": "Mengukur, Menganalisis Lalu Lintas Web." },
  { "en": "Alat Analitik Web (Web Analytics) Populer?", "id": "Google Analytics." },
  { "en": "Apa Itu Pengujian A/B (A/B Testing)?", "id": "Membandingkan Dua Versi Halaman Web." },
  { "en": "Apa Tujuan Pengujian A/B (A/B Testing)?", "id": "Menentukan Versi Mana Lebih Efektif." },
  { "en": "Apa Itu Peta Panas (Heatmap) Analitik?", "id": "Visualisasi Area Paling Sering Diklik/Dilihat." },
  { "en": "Apa Itu Rekaman Sesi (Session Recording)?", "id": "Merekam Interaksi Pengguna Di Situs Web." },
  { "en": "Apa Itu Tingkat Pentalan (Bounce Rate)?", "id": "Persentase Pengunjung Tinggalkan Situs (Satu Halaman)." },
  { "en": "Apa Itu Tingkat Konversi (Conversion Rate)?", "id": "Persentase Pengunjung Lakukan Aksi Diinginkan." },
  { "en": "Apa Itu Optimasi Tingkat Konversi (CRO)?", "id": "Conversion Rate Optimization (CRO)." },
  { "en": "Apa Kepanjangan CRO (Conversion Rate Optimization)?", "id": "Optimasi Tingkat Konversi." },
  { "en": "Apa Itu Pemasaran Mesin Pencari (SEM)?", "id": "Search Engine Marketing (SEM)." },
  { "en": "Apa Kepanjangan SEM (Search Engine Marketing)?", "id": "Pemasaran Mesin Pencari." },
  { "en": "Apa Beda SEO (Search Engine Optimization) Dan SEM (Search Engine Marketing)?", "id": "SEO (Organik), SEM (Berbayar/PPC)." },
  { "en": "Apa Kepanjangan PPC (Pay-Per-Click)?", "id": "Bayar Per Klik." },
  { "en": "Platform Iklan PPC (Pay-Per-Click) Populer?", "id": "Google Ads, Facebook Ads." },
  { "en": "Apa Itu Pemasaran Konten (Content Marketing)?", "id": "Membuat, Mendistribusikan Konten Bernilai." },
  { "en": "Apa Itu Pemasaran Media Sosial (Social Media Marketing)?", "id": "Pemasaran Lewat Platform Media Sosial." },
  { "en": "Apa Itu Pemasaran Email (Email Marketing)?", "id": "Mengirim Pesan Pemasaran Lewat Email." },
  { "en": "Apa Itu Tingkat Buka (Open Rate) Email?", "id": "Persentase Email Dibuka Penerima." },
  { "en": "Apa Itu Tingkat Klik (Click-Through Rate) (CTR)?", "id": "Persentase Klik Tautan Dalam Email/Iklan." },
  { "en": "Apa Kepanjangan CTR (Click-Through Rate)?", "id": "Rasio Klik Tayang." },
  { "en": "Apa Itu Pemasaran Afiliasi (Affiliate Marketing)?", "id": "Promosi Produk Orang Lain (Komisi)." },
  { "en": "Apa Itu Manajemen Hubungan Pelanggan (CRM)?", "id": "Customer Relationship Management (CRM)." },
  { "en": "Apa Kepanjangan CRM (Customer Relationship Management)?", "id": "Manajemen Hubungan Pelanggan." },
  { "en": "Apa Fungsi Perangkat Lunak CRM (Customer Relationship Management)?", "id": "Mengelola Interaksi Dengan Pelanggan." },
  { "en": "Apa Itu Kecerdasan Buatan (Artificial Intelligence) (AI)?", "id": "Kemampuan Mesin Meniru Manusia." },
  { "en": "Apa Itu Pembelajaran Mesin (Machine Learning) (ML)?", "id": "AI (Artificial Intelligence) Belajar Dari Data." },
  { "en": "Apa Itu Pembelajaran Mendalam (Deep Learning) (DL)?", "id": "ML (Machine Learning) Dengan Jaringan Saraf Dalam." },
  { "en": "Apa Itu Jaringan Saraf Tiruan (ANN)?", "id": "Model Komputasi Terinspirasi Otak." },
  { "en": "Apa Itu Pemrosesan Bahasa Alami (NLP)?", "id": "Interaksi Komputer, Bahasa Manusia." },
  { "en": "Apa Itu Visi Komputer (Computer Vision)?", "id": "Pemahaman Komputer Terhadap Gambar/Video." },
  { "en": "Apa Itu Robotika (Robotics)?", "id": "Desain, Operasi Robot." },
  { "en": "Apa Itu Sistem Pakar (Expert System)?", "id": "AI (Artificial Intelligence) Meniru Kemampuan Pakar." },
  { "en": "Apa Itu Logika Fuzzy (Fuzzy Logic)?", "id": "Logika Derajat Kebenaran (Bukan Biner)." },
  { "en": "Apa Itu Algoritma Genetika (Genetic Algorithm)?", "id": "Algoritma Optimasi Terinspirasi Evolusi." },
  { "en": "Apa Itu Superkomputer (Supercomputer)?", "id": "Komputer Kinerja Sangat Tinggi." },
  { "en": "Apa Satuan Kinerja Superkomputer (Supercomputer)?", "id": "FLOPS (Floating Point Operations Per Second)." },
  { "en": "Apa Kepanjangan FLOPS (Floating Point Operations Per Second)?", "id": "Operasi Titik Mengambang Per Detik." },
  { "en": "Apa Itu Komputasi Paralel (Parallel Computing)?", "id": "Menjalankan Banyak Komputasi Bersamaan." },
  { "en": "Apa Itu Komputasi Terdistribusi (Distributed Computing)?", "id": "Komputasi Menggunakan Banyak Komputer Jaringan." },
  { "en": "Apa Itu Komputasi Grid (Grid Computing)?", "id": "Komputasi Terdistribusi Skala Besar." },
  { "en": "Apa Itu Mekanika Kuantum (Quantum Mechanics)?", "id": "Fisika Mengatur Skala Atom, Subatom." },
  { "en": "Apa Itu Prinsip Ketidakpastian Heisenberg?", "id": "Tidak Bisa Ukur Posisi, Momentum Akurat Bersamaan." },
  { "en": "Apa Itu Fungsi Gelombang (Wave Function)?", "id": "Deskripsi Matematis Keadaan Kuantum." },
  { "en": "Apa Itu Persamaan Schrödinger (Schrödinger Equation)?", "id": "Persamaan Evolusi Fungsi Gelombang." },
  { "en": "Apa Itu Kucing Schrödinger (Schrödinger's Cat)?", "id": "Eksperimen Pikiran Ilustrasi Superposisi." },
  { "en": "Apa Itu Fisika Partikel (Particle Physics)?", "id": "Studi Partikel Dasar Materi, Interaksi." },
  { "en": "Apa Itu Model Standar (Standard Model) Fisika Partikel?", "id": "Teori Deskripsi Partikel Dasar, Gaya." },
  { "en": "Apa Empat Gaya Fundamental (Fundamental Force)?", "id": "Gravitasi, Elektromagnetik, Nuklir Kuat, Lemah." },
  { "en": "Apa Itu Quark (Quark)?", "id": "Partikel Dasar Penyusun Hadron (Proton, Neutron)." },
  { "en": "Apa Itu Lepton (Lepton)?", "id": "Partikel Dasar (Elektron, Neutrino)." },
  { "en": "Apa Itu Boson (Boson)?", "id": "Partikel Pembawa Gaya (Foton, Gluon)." },
  { "en": "Apa Itu Boson Higgs (Higgs Boson)?", "id": "Partikel Terkait Pemberian Massa." },
  { "en": "Apa Itu Antimateri (Antimatter)?", "id": "Materi Muatan Berlawanan (Positron)." },
  { "en": "Apa Itu Anihilasi (Annihilation)?", "id": "Materi, Antimateri Bertemu Hasilkan Energi." },
  { "en": "Apa Itu Teori Relativitas (Theory of Relativity) Khusus?", "id": "Teori Einstein Ruang, Waktu, Gravitasi." },
  { "en": "Apa Itu E=mc² (E equals mc squared)?", "id": "Persamaan Kesetaraan Massa-Energi Einstein." },
  { "en": "Apa Itu Dilatasi Waktu (Time Dilation)?", "id": "Waktu Berjalan Lebih Lambat Kecepatan Tinggi." },
  { "en": "Apa Itu Kontraksi Panjang (Length Contraction)?", "id": "Objek Tampak Memendek Kecepatan Tinggi." },
  { "en": "Apa Itu Teori Relativitas (Theory of Relativity) Umum?", "id": "Teori Einstein Gravitasi Kelengkungan Ruangwaktu." },
  { "en": "Apa Itu Lubang Hitam (Black Hole)?", "id": "Wilayah Ruangwaktu Gravitasi Sangat Kuat." },
  { "en": "Apa Itu Horison Peristiwa (Event Horizon)?", "id": "Batas Lubang Hitam (Tidak Bisa Kembali)." },
  { "en": "Apa Itu Singularitas (Singularity) Gravitasi?", "id": "Titik Kepadatan Tak Terhingga Lubang Hitam." },
  { "en": "Apa Itu Radiasi Hawking (Hawking Radiation)?", "id": "Radiasi Termal Dipancarkan Lubang Hitam." },
  { "en": "Apa Itu Big Bang (Ledakan Dahsyat)?", "id": "Teori Awal Mula Alam Semesta." },
  { "en": "Apa Itu Radiasi Latar Belakang Kosmik (CMB)?", "id": "Sisa Cahaya Panas Dari Big Bang." },
  { "en": "Apa Kepanjangan CMB (Cosmic Microwave Background)?", "id": "Latar Belakang Gelombang Mikro Kosmik." },
  { "en": "Apa Itu Materi Gelap (Dark Matter)?", "id": "Materi Tak Terlihat (Interaksi Gravitasi)." },
  { "en": "Apa Itu Energi Gelap (Dark Energy)?", "id": "Penyebab Percepatan Ekspansi Alam Semesta." },
  { "en": "Apa Itu Kosmologi (Cosmology)?", "id": "Studi Asal, Evolusi Alam Semesta." },
  { "en": "Apa Itu Astrofisika (Astrophysics)?", "id": "Aplikasi Fisika Studi Objek Astronomi." },
  { "en": "Apa Itu Bintang (Star)?", "id": "Bola Plasma Raksasa (Reaksi Fusi Nuklir)." },
  { "en": "Apa Itu Galaksi (Galaxy)?", "id": "Sistem Masif Bintang, Gas, Debu." },
  { "en": "Apa Nama Galaksi (Galaxy) Kita?", "id": "Galaksi Bima Sakti (Milky Way)." },
  { "en": "Apa Itu Nebula (Nebula)?", "id": "Awan Antarbintang (Gas, Debu)." },
  { "en": "Apa Itu Supernova (Supernova)?", "id": "Ledakan Bintang Masif Akhir Hayat." },
  { "en": "Apa Itu Bintang Neutron (Neutron Star)?", "id": "Sisa Inti Bintang Supernova (Sangat Padat)." },
  { "en": "Apa Itu Pulsar (Pulsar)?", "id": "Bintang Neutron Berotasi Cepat (Pancaran Radio)." },
  { "en": "Apa Itu Kata Putih (White Dwarf)?", "id": "Sisa Inti Bintang Massa Rendah/Sedang." },
  { "en": "Apa Itu Raksasa Merah (Red Giant)?", "id": "Tahap Evolusi Bintang (Mengembang, Mendingin)." },
  { "en": "Apa Itu Planet (Planet)?", "id": "Benda Langit Mengorbit Bintang." },
  { "en": "Apa Itu Exoplanet (Exoplanet)?", "id": "Planet Di Luar Tata Surya Kita." },
  { "en": "Apa Itu Asteroid (Asteroid)?", "id": "Benda Berbatu Kecil Mengorbit Matahari." },
  { "en": "Apa Itu Komet (Comet)?", "id": "Benda Es, Debu Mengorbit Matahari." },
  { "en": "Apa Itu Meteoroid (Meteoroid)?", "id": "Batuan Kecil Luar Angkasa." },
  { "en": "Apa Itu Meteor (Meteor)?", "id": "Meteoroid Terbakar Masuk Atmosfer." },
  { "en": "Apa Nama Lain Meteor (Meteor)?", "id": "Bintang Jatuh (Shooting Star)." },
  { "en": "Apa Itu Meteorit (Meteorite)?", "id": "Sisa Meteor Sampai Permukaan Bumi." },
  { "en": "Apa Itu Teleskop (Telescope)?", "id": "Instrumen Pengamatan Objek Jauh." },
  { "en": "Apa Itu Teleskop (Telescope) Refraktor?", "id": "Menggunakan Lensa Kumpulkan Cahaya." },
  { "en": "Apa Itu Teleskop (Telescope) Reflektor?", "id": "Menggunakan Cermin Kumpulkan Cahaya." },
  { "en": "Apa Itu Teleskop (Telescope) Radio?", "id": "Mendeteksi Gelombang Radio Objek Astronomi." },
  { "en": "Apa Itu Teleskop (Telescope) Luar Angkasa Hubble?", "id": "Teleskop Mengorbit Bumi (Gambar Tajam)." },
  { "en": "Apa Itu Teleskop (Telescope) Luar Angkasa James Webb?", "id": "Teleskop Inframerah Pengganti Hubble." },
  { "en": "Apa Itu Spektroskopi (Spectroscopy)?", "id": "Analisis Cahaya Bedah Komposisi Objek." },
  { "en": "Apa Itu Pergeseran Merah (Redshift)?", "id": "Pergeseran Cahaya Ke Panjang Gelombang Merah." },
  { "en": "Apa Penyebab Pergeseran Merah (Redshift) Kosmologis?", "id": "Ekspansi Alam Semesta (Objek Menjauh)." },
  { "en": "Apa Itu Pergeseran Biru (Blueshift)?", "id": "Pergeseran Cahaya Ke Panjang Gelombang Biru." },
  { "en": "Apa Penyebab Pergeseran Biru (Blueshift)?", "id": "Objek Mendekat Ke Pengamat." },
  { "en": "Apa Itu Efek Doppler (Doppler Effect) Cahaya?", "id": "Penyebab Pergeseran Merah/Biru Gerakan." },
  { "en": "Apa Itu Tahun Cahaya (Light Year)?", "id": "Jarak Ditempuh Cahaya Satu Tahun." },
  { "en": "Apa Itu Satuan Astronomi (Astronomical Unit) (AU)?", "id": "Jarak Rata-Rata Bumi Ke Matahari." },
  { "en": "Apa Itu Parsec (Parsec)?", "id": "Satuan Jarak Astronomi (Paralaks Satu Detik Busur)." },
  { "en": "Apa Itu Diagram Hertzsprung-Russell (H-R Diagram)?", "id": "Plot Luminositas Bintang Vs Suhu." },
  { "en": "Apa Itu Urutan Utama (Main Sequence) Bintang?", "id": "Tahap Fusi Hidrogen Inti Bintang." },
  { "en": "Apa Itu Batas Chandrasekhar (Chandrasekhar Limit)?", "id": "Massa Maksimum Kata Putih Stabil." },
  { "en": "Apa Itu Termodinamika (Thermodynamics)?", "id": "Studi Energi, Panas, Kerja." },
  { "en": "Apa Hukum Pertama (First Law) Termodinamika?", "id": "Kekekalan Energi (Energi Tidak Musnah)." },
  { "en": "Apa Hukum Kedua (Second Law) Termodinamika?", "id": "Entropi (Entropy) Alam Semesta Selalu Meningkat." },
  { "en": "Apa Itu Entropi (Entropy)?", "id": "Ukuran Ketidakteraturan Atau Keacakan Sistem." },
  { "en": "Apa Hukum Ketiga (Third Law) Termodinamika?", "id": "Mustahil Capai Nol Absolut Sempurna." },
  { "en": "Apa Itu Nol Absolut (Absolute Zero)?", "id": "Suhu Terendah Teoretis (0 Kelvin)." },
  { "en": "Apa Skala Suhu Kelvin (Kelvin Scale)?", "id": "Skala Suhu Absolut (Mulai Nol Absolut)." },
  { "en": "Apa Itu Konduksi (Conduction) Panas?", "id": "Transfer Panas Lewat Kontak Langsung." },
  { "en": "Apa Itu Konveksi (Convection) Panas?", "id": "Transfer Panas Lewat Pergerakan Fluida." },
  { "en": "Apa Itu Radiasi (Radiation) Panas?", "id": "Transfer Panas Lewat Gelombang Elektromagnetik." },
  { "en": "Apa Itu Kapasitas Panas (Heat Capacity)?", "id": "Jumlah Panas Naikkan Suhu Zat." },
  { "en": "Apa Itu Panas Jenis (Specific Heat)?", "id": "Kapasitas Panas Per Satuan Massa." },
  { "en": "Apa Itu Panas Laten (Latent Heat)?", "id": "Panas Diserap/Lepas Saat Perubahan Fasa." },
  { "en": "Apa Itu Mesin Kalor (Heat Engine)?", "id": "Mesin Pengubah Energi Panas Ke Kerja." },
  { "en": "Apa Itu Siklus Carnot (Carnot Cycle)?", "id": "Siklus Mesin Kalor Ideal Efisiensi Maksimal." },
  { "en": "Apa Itu Efisiensi (Efficiency) Mesin Kalor?", "id": "Rasio Kerja Dihasilkan Terhadap Panas Masuk." },
  { "en": "Apa Itu Pompa Kalor (Heat Pump)?", "id": "Memindahkan Panas Dari Dingin Ke Panas." },
  { "en": "Apa Itu Koefisien Kinerja (COP)?", "id": "Coefficient of Performance (COP) Pompa Kalor/Pendingin." },
  { "en": "Apa Kepanjangan COP (Coefficient of Performance)?", "id": "Koefisien Kinerja." },
  { "en": "Apa Itu Mekanika Fluida (Fluid Mechanics)?", "id": "Studi Perilaku Fluida (Cair, Gas)." },
  { "en": "Apa Itu Tekanan (Pressure) Fluida?", "id": "Gaya Per Satuan Luas Permukaan." },
  { "en": "Apa Satuan Tekanan (Pressure)?", "id": "Pascal (Pa)." },
  { "en": "Apa Itu Hukum Pascal (Pascal's Law)?", "id": "Tekanan Cairan Tertutup Diteruskan Merata." },
  { "en": "Apa Itu Prinsip Archimedes (Archimedes' Principle)?", "id": "Gaya Apung Benda Dalam Fluida." },
  { "en": "Apa Itu Viskositas (Viscosity) Fluida?", "id": "Ukuran Kekentalan Atau Hambatan Alir." },
  { "en": "Apa Itu Aliran Laminar (Laminar Flow)?", "id": "Aliran Fluida Halus, Teratur." },
  { "en": "Apa Itu Aliran Turbulen (Turbulent Flow)?", "id": "Aliran Fluida Kacau, Berputar." },
  { "en": "Apa Itu Bilangan Reynolds (Reynolds Number)?", "id": "Parameter Penentu Jenis Aliran (Laminar/Turbulen)." },
  { "en": "Apa Itu Persamaan Bernoulli (Bernoulli's Equation)?", "id": "Hubungan Tekanan, Kecepatan, Ketinggian Fluida." },
  { "en": "Apa Itu Efek Venturi (Venturi Effect)?", "id": "Penurunan Tekanan Fluida Kecepatan Meningkat." },
  { "en": "Apa Itu Tegangan Permukaan (Surface Tension)?", "id": "Kecenderungan Permukaan Cairan Menyusut." },
  { "en": "Apa Itu Kapilaritas (Capillarity)?", "id": "Naik Turunnya Cairan Pipa Sempit." },
  { "en": "Apa Itu Optika (Optics)?", "id": "Studi Perilaku, Sifat Cahaya." },
  { "en": "Apa Itu Pemantulan (Reflection) Cahaya?", "id": "Cahaya Memantul Dari Permukaan." },
  { "en": "Apa Hukum Pemantulan (Law of Reflection)?", "id": "Sudut Datang Sama Dengan Sudut Pantul." },
  { "en": "Apa Itu Pembiasan (Refraction) Cahaya?", "id": "Pembelokan Cahaya Lewati Antarmuka Medium." },
  { "en": "Apa Hukum Snellius (Snell's Law) Pembiasan?", "id": "Hubungan Sudut Datang, Bias, Indeks Bias." },
  { "en": "Apa Itu Indeks Bias (Refractive Index) (n)?", "id": "Ukuran Pembelokan Cahaya Dalam Medium." },
  { "en": "Apa Itu Pemantulan Internal Total (Total Internal Reflection)?", "id": "Cahaya Dipantulkan Sempurna Dalam Medium." },
  { "en": "Syarat Pemantulan Internal Total (Total Internal Reflection)?", "id": "Cahaya Dari Medium Rapat Ke Kurang Rapat." },
  { "en": "Apa Itu Lensa (Lens)?", "id": "Benda Transparan Membiaskan Cahaya." },
  { "en": "Apa Itu Lensa (Lens) Cembung (Konvergen)?", "id": "Mengumpulkan Sinar Cahaya Paralel." },
  { "en": "Apa Itu Lensa (Lens) Cekung (Divergen)?", "id": "Menyebarkan Sinar Cahaya Paralel." },
  { "en": "Apa Itu Panjang Fokus (Focal Length) Lensa?", "id": "Jarak Lensa Ke Titik Fokus." },
  { "en": "Apa Itu Kekuatan Lensa (Lens Power)?", "id": "Ukuran Kemampuan Lensa Membelokkan Cahaya." },
  { "en": "Apa Satuan Kekuatan Lensa (Lens Power)?", "id": "Dioptri (Diopter)." },
  { "en": "Apa Itu Aberasi (Aberration) Lensa?", "id": "Cacat Pembentukan Bayangan Lensa." },
  { "en": "Apa Itu Aberasi Sferis (Spherical Aberration)?", "id": "Cahaya Tepi Fokus Beda Pusat." },
  { "en": "Apa Itu Aberasi Kromatik (Chromatic Aberration)?", "id": "Warna Cahaya Berbeda Fokus Beda." },
  { "en": "Apa Itu Difraksi (Diffraction) Cahaya?", "id": "Pelenturan Cahaya Sekitar Penghalang." },
  { "en": "Apa Itu Interferensi (Interference) Cahaya?", "id": "Superposisi Gelombang Cahaya." },
  { "en": "Apa Itu Eksperimen Celah Ganda Young?", "id": "Menunjukkan Sifat Gelombang Cahaya (Interferensi)." },
  { "en": "Apa Itu Polarisasi (Polarization) Cahaya?", "id": "Orientasi Arah Getar Medan Listrik." },
  { "en": "Apa Itu Filter Polarisasi (Polarizing Filter)?", "id": "Melewatkan Cahaya Polarisasi Tertentu." },
  { "en": "Apa Itu Efek Doppler (Doppler Effect) Cahaya?", "id": "Pergeseran Frekuensi Akibat Gerakan Relatif." },
  { "en": "Apa Itu Spektrum Elektromagnetik (Electromagnetic Spectrum)?", "id": "Rentang Semua Frekuensi Radiasi EM." },
  { "en": "Apa Itu Cahaya Tampak (Visible Light)?", "id": "Bagian Spektrum EM Terlihat Mata." },
  { "en": "Urutan Warna Cahaya Tampak (Visible Light) Terpendek Terpanjang?", "id": "Ungu, Biru, Hijau, Kuning, Oranye, Merah." },
  { "en": "Apa Itu Inframerah (Infrared) (IR)?", "id": "Radiasi EM Panjang Gelombang Lebih Merah." },
  { "en": "Apa Itu Ultraviolet (Ultraviolet) (UV)?", "id": "Radiasi EM Panjang Gelombang Lebih Ungu." },
  { "en": "Apa Itu Mekanika Klasik (Classical Mechanics)?", "id": "Fisika Gerak Benda Makroskopik." },
  { "en": "Apa Tiga Hukum Gerak Newton (Newton's Laws)?", "id": "Hukum Inersia, F=ma, Aksi-Reaksi." },
  { "en": "Apa Itu Hukum Inersia (Law of Inertia)?", "id": "Benda Diam/Gerak Lurus Tetap Kecuali Gaya." },
  { "en": "Apa Itu Hukum Kedua Newton (Newton's Second Law)?", "id": "Percepatan Sebanding Gaya, Berbanding Terbalik Massa." },
  { "en": "Apa Itu Hukum Ketiga Newton (Newton's Third Law)?", "id": "Setiap Aksi Ada Reaksi Sama Berlawanan." },
  { "en": "Apa Itu Massa (Mass)?", "id": "Ukuran Inersia Benda (Jumlah Materi)." },
  { "en": "Apa Itu Berat (Weight)?", "id": "Gaya Gravitasi Pada Benda Bermassa." },
  { "en": "Apa Itu Momentum (Momentum)?", "id": "Ukuran Kesulitan Menghentikan Benda (Massa x Kecepatan)." },
  { "en": "Apa Itu Hukum Kekekalan Momentum (Conservation of Momentum)?", "id": "Momentum Total Sistem Terisolasi Konstan." },
  { "en": "Apa Itu Energi Kinetik (Kinetic Energy)?", "id": "Energi Akibat Gerakan Benda." },
  { "en": "Apa Rumus Energi Kinetik (Kinetic Energy)?", "id": "EK = 1/2 * m * v²." },
  { "en": "Apa Itu Energi Potensial (Potential Energy)?", "id": "Energi Tersimpan Akibat Posisi/Konfigurasi." },
  { "en": "Contoh Energi Potensial (Potential Energy)?", "id": "Energi Potensial Gravitasi, Pegas." },
  { "en": "Apa Itu Hukum Kekekalan Energi (Conservation of Energy)?", "id": "Energi Total Sistem Terisolasi Konstan." },
  { "en": "Apa Itu Kerja (Work) Dalam Fisika?", "id": "Transfer Energi Akibat Gaya (Gaya x Jarak)." },
  { "en": "Apa Satuan Kerja (Work), Energi (Energy)?", "id": "Joule (J)." },
  { "en": "Apa Itu Daya (Power)?", "id": "Laju Melakukan Kerja (Kerja / Waktu)." },
  { "en": "Apa Satuan Daya (Power)?", "id": "Watt (W)." },
  { "en": "Apa Itu Gerak Melingkar (Circular Motion)?", "id": "Gerak Benda Mengikuti Lintasan Lingkaran." },
  { "en": "Apa Itu Kecepatan Sudut (Angular Velocity) (ω)?", "id": "Laju Perubahan Sudut Posisi." },
  { "en": "Apa Itu Percepatan Sentripetal (Centripetal Acceleration)?", "id": "Percepatan Menuju Pusat Lintasan Melingkar." },
  { "en": "Apa Itu Gaya Sentripetal (Centripetal Force)?", "id": "Gaya Penyebab Percepatan Sentripetal." },
  { "en": "Apa Itu Momen Inersia (Moment of Inertia)?", "id": "Ukuran Kelembaman Rotasi Benda." },
  { "en": "Apa Itu Momentum Sudut (Angular Momentum)?", "id": "Ukuran Kelembaman Gerak Rotasi." },
  { "en": "Apa Itu Hukum Kekekalan Momentum Sudut?", "id": "Momentum Sudut Total Konstan (Tanpa Torsi)." },
  { "en": "Apa Itu Torsi (Torque)?", "id": "Ukuran Kemampuan Gaya Sebabkan Rotasi." },
  { "en": "Apa Itu Gerak Harmonik Sederhana (Simple Harmonic Motion)?", "id": "Gerak Osilasi Periodik (Gaya Pemulih)." },
  { "en": "Contoh Gerak Harmonik Sederhana (Simple Harmonic Motion)?", "id": "Gerak Pegas, Bandul Sederhana." },
  { "en": "Apa Itu Frekuensi (Frequency) Osilasi?", "id": "Jumlah Osilasi Per Detik." },
  { "en": "Apa Itu Periode (Period) Osilasi?", "id": "Waktu Untuk Satu Osilasi Lengkap." },
  { "en": "Apa Itu Amplitudo (Amplitude) Osilasi?", "id": "Simpangan Maksimum Dari Titik Setimbang." },
  { "en": "Apa Itu Resonansi (Resonance) Mekanis?", "id": "Amplitudo Osilasi Maksimal Frekuensi Alami." },
  { "en": "Apa Itu Gelombang (Wave)?", "id": "Getaran Merambat Membawa Energi." },
  { "en": "Apa Itu Gelombang Transversal (Transverse Wave)?", "id": "Getaran Tegak Lurus Arah Rambat." },
  { "en": "Contoh Gelombang Transversal (Transverse Wave)?", "id": "Gelombang Cahaya, Gelombang Tali." },
  { "en": "Apa Itu Gelombang Longitudinal (Longitudinal Wave)?", "id": "Getaran Sejajar Arah Rambat." },
  { "en": "Contoh Gelombang Longitudinal (Longitudinal Wave)?", "id": "Gelombang Suara." }



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
