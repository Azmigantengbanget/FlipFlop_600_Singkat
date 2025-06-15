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

  { "en": "Apa itu flip-flop?", "id": "Rangkaian digital penyimpan satu bit." },
  { "en": "Flip-flop menyimpan berapa bit?", "id": "Satu bit data digital." },
  { "en": "Flip-flop termasuk rangkaian apa?", "id": "Rangkaian logika sekuensial bistabil." },
  { "en": "Berapa kondisi stabil flip-flop?", "id": "Dua kondisi stabil berbeda." },
  { "en": "Flip-flop digunakan untuk apa?", "id": "Menyimpan dan mengingat informasi." },
  { "en": "Apa kepanjangan dari FF?", "id": "Flip-Flop dalam elektronika digital." },
  { "en": "Flip-flop memiliki berapa output?", "id": "Dua output saling berlawanan." },
  { "en": "Output flip-flop biasanya dilambangkan?", "id": "Q dan Q bar." },
  { "en": "Flip-flop SR kepanjangan dari?", "id": "Set Reset flip-flop circuit." },
  { "en": "Flip-flop JK kepanjangan dari?", "id": "Jack Kilby flip-flop circuit." },
  { "en": "Flip-flop D kepanjangan dari?", "id": "Data atau Delay flip-flop." },
  { "en": "Flip-flop T kepanjangan dari?", "id": "Toggle flip-flop switching circuit." },
  { "en": "Berapa jenis utama flip-flop?", "id": "Empat jenis utama flip-flop." },
  { "en": "Flip-flop bekerja dengan sinyal?", "id": "Sinyal clock atau trigger." },
  { "en": "Apa fungsi clock flip-flop?", "id": "Mengontrol waktu perubahan output." },
  { "en": "Flip-flop SR memiliki input?", "id": "Set dan Reset input." },
  { "en": "Kondisi terlarang flip-flop SR?", "id": "S dan R sama-sama high." },
  { "en": "Flip-flop JK mengatasi masalah?", "id": "Kondisi terlarang pada SR." },
  { "en": "Input flip-flop JK adalah?", "id": "J dan K input." },
  { "en": "Flip-flop D memiliki input?", "id": "Satu input data D." },
  { "en": "Flip-flop T memiliki input?", "id": "Satu input toggle T." },
  { "en": "Master-slave flip-flop adalah?", "id": "Dua flip-flop seri cascade." },
  { "en": "Edge-triggered flip-flop aktif saat?", "id": "Tepi naik atau turun." },
  { "en": "Level-triggered flip-flop aktif saat?", "id": "Level clock tinggi rendah." },
  { "en": "Positive edge-triggered aktif saat?", "id": "Transisi clock low ke high." },
  { "en": "Negative edge-triggered aktif saat?", "id": "Transisi clock high ke low." },
  { "en": "Setup time flip-flop adalah?", "id": "Waktu input stabil sebelum clock." },
  { "en": "Hold time flip-flop adalah?", "id": "Waktu input stabil setelah clock." },
  { "en": "Propagation delay flip-flop adalah?", "id": "Waktu tunda input ke output." },
  { "en": "Flip-flop digunakan dalam?", "id": "Counter, register, dan memori." },
  { "en": "Asynchronous input flip-flop adalah?", "id": "Input tidak tergantung clock." },
  { "en": "Synchronous input flip-flop adalah?", "id": "Input tergantung sinyal clock." },
  { "en": "Preset flip-flop berfungsi untuk?", "id": "Set output Q ke high." },
  { "en": "Clear flip-flop berfungsi untuk?", "id": "Reset output Q ke low." },
  { "en": "Flip-flop dibuat dari?", "id": "Gerbang logika NAND NOR." },
  { "en": "Bistable multivibrator sama dengan?", "id": "Flip-flop rangkaian penyimpan bit." },
  { "en": "Flip-flop SR NAND memerlukan?", "id": "Input aktif low logic." },
  { "en": "Flip-flop SR NOR memerlukan?", "id": "Input aktif high logic." },
  { "en": "Clocked flip-flop dikontrol oleh?", "id": "Sinyal clock timing control." },
  { "en": "Unclocked flip-flop dikontrol oleh?", "id": "Input data secara langsung." },
  { "en": "Flip-flop JK saat J=K=1?", "id": "Output akan toggle berubah." },
  { "en": "Flip-flop D saat D=1?", "id": "Output Q akan menjadi high." },
  { "en": "Flip-flop D saat D=0?", "id": "Output Q akan menjadi low." },
  { "en": "Flip-flop T saat T=1?", "id": "Output akan toggle berubah." },
  { "en": "Flip-flop T saat T=0?", "id": "Output tetap tidak berubah." },
  { "en": "Race condition flip-flop adalah?", "id": "Kondisi output tidak terdefinisi." },
  { "en": "Metastability flip-flop adalah?", "id": "Kondisi output tidak stabil." },
  { "en": "Flip-flop digunakan untuk counter?", "id": "Ya, untuk menghitung pulsa." },
  { "en": "Flip-flop digunakan untuk register?", "id": "Ya, untuk menyimpan data." },
  { "en": "Flip-flop digunakan untuk memori?", "id": "Ya, sebagai elemen memori." },
  { "en": "Flip-flop digunakan untuk divider?", "id": "Ya, untuk pembagi frekuensi." },
  { "en": "Latch sama dengan flip-flop?", "id": "Mirip tapi berbeda timing." },
  { "en": "Perbedaan latch dan flip-flop?", "id": "Latch level-triggered, FF edge-triggered." },
  { "en": "Flip-flop memerlukan power supply?", "id": "Ya, memerlukan tegangan catu." },
  { "en": "Flip-flop dapat dibuat dengan?", "id": "Transistor, CMOS, atau TTL." },
  { "en": "Flip-flop TTL bekerja pada?", "id": "Tegangan lima volt DC." },
  { "en": "Flip-flop CMOS bekerja pada?", "id": "Tegangan tiga sampai lima." },
  { "en": "Flip-flop ECL bekerja pada?", "id": "Tegangan negatif minus lima." },
  { "en": "Kecepatan flip-flop diukur dalam?", "id": "Megahertz atau gigahertz frekuensi." },
  { "en": "Flip-flop dapat cascade untuk?", "id": "Membuat counter atau register." },
  { "en": "Ripple counter menggunakan?", "id": "Flip-flop cascade tanpa clock." },
  { "en": "Synchronous counter menggunakan?", "id": "Flip-flop dengan clock sama." },
  { "en": "Shift register menggunakan?", "id": "Flip-flop untuk geser data." },
  { "en": "Flip-flop dalam CPU untuk?", "id": "Register dan unit kontrol." },
  { "en": "Flip-flop dalam RAM untuk?", "id": "Menyimpan bit data memori." },
  { "en": "Flip-flop dapat reset dengan?", "id": "Sinyal clear atau reset." },
  { "en": "Flip-flop dapat set dengan?", "id": "Sinyal preset atau set." },
  { "en": "Flip-flop output Q dan?", "id": "Q bar saling berlawanan." },
  { "en": "Flip-flop stabil pada kondisi?", "id": "Q tinggi atau rendah." },
  { "en": "Flip-flop tidak stabil saat?", "id": "Transisi atau kondisi terlarang." },
  { "en": "Flip-flop JK mengatasi?", "id": "Kondisi tidak terdefinisi SR." },
  { "en": "Flip-flop D disebut juga?", "id": "Transparent latch atau delay." },
  { "en": "Flip-flop T disebut juga?", "id": "Toggle atau trigger flip-flop." },
  { "en": "Flip-flop master-slave terdiri dari?", "id": "Dua latch seri berurutan." },
  { "en": "Flip-flop edge-triggered lebih?", "id": "Stabil dari level-triggered." },
  { "en": "Clock skew mempengaruhi?", "id": "Timing dan sinkronisasi flip-flop." },
  { "en": "Flip-flop dapat dibuat dari?", "id": "Dua gerbang NAND cross-coupled." },
  { "en": "Flip-flop dapat dibuat dari?", "id": "Dua gerbang NOR cross-coupled." },
  { "en": "Flip-flop SR-latch aktif saat?", "id": "Enable signal dalam kondisi." },
  { "en": "Flip-flop gated-latch dikontrol oleh?", "id": "Enable dan input data." },
  { "en": "Flip-flop transparent-latch saat enable?", "id": "Output mengikuti input langsung." },
  { "en": "Flip-flop opaque-latch saat disable?", "id": "Output tetap tidak berubah." },
  { "en": "Flip-flop digunakan dalam?", "id": "Sistem digital dan komputer." },
  { "en": "Flip-flop essential untuk?", "id": "Penyimpanan dan pengolahan data." },
  { "en": "Flip-flop dapat mengalami?", "id": "Glitch atau noise interference." },
  { "en": "Flip-flop noise immunity bergantung?", "id": "Teknologi dan desain rangkaian." },
  { "en": "Flip-flop power consumption bergantung?", "id": "Teknologi CMOS atau TTL." },
  { "en": "Flip-flop CMOS konsumsi daya?", "id": "Lebih rendah dari TTL." },
  { "en": "Flip-flop TTL kecepatan?", "id": "Lebih cepat dari CMOS." },
  { "en": "Flip-flop dapat menggunakan?", "id": "Feedback untuk stabilitas output." },
  { "en": "Flip-flop feedback mencegah?", "id": "Kondisi tidak stabil output." },
  { "en": "Flip-flop cross-coupled berarti?", "id": "Output saling terhubung silang." },
  { "en": "Flip-flop regenerative action adalah?", "id": "Penguatan sinyal output kembali." },
  { "en": "Flip-flop hysteresis membantu?", "id": "Stabilitas terhadap noise kecil." },
  { "en": "Flip-flop dapat digunakan untuk?", "id": "Debouncing switch mechanical contact." },
  { "en": "Flip-flop debouncing mencegah?", "id": "Multiple trigger dari bouncing." },
  { "en": "Flip-flop dalam oscillator untuk?", "id": "Menghasilkan sinyal clock periodik." },
  { "en": "Flip-flop frequency divider membagi?", "id": "Frekuensi input menjadi setengah." },
  { "en": "Flip-flop dapat cascade untuk?", "id": "Pembagi frekuensi lebih besar." },
  { "en": "Flip-flop binary counter menghitung?", "id": "Dalam sistem bilangan biner." },
  { "en": "Flip-flop decade counter menghitung?", "id": "Dari nol sampai sembilan." },
  { "en": "Flip-flop up-counter menghitung?", "id": "Naik dari nol ke atas." },
  { "en": "Flip-flop down-counter menghitung?", "id": "Turun dari nilai maksimum." },
  { "en": "Flip-flop up-down counter dapat?", "id": "Menghitung naik dan turun." },
  { "en": "Flip-flop ring counter output?", "id": "Berputar satu bit high." },
  { "en": "Flip-flop Johnson counter output?", "id": "Berputar dengan pola khusus." },
  { "en": "Flip-flop SISO register adalah?", "id": "Serial input serial output." },
  { "en": "Flip-flop SIPO register adalah?", "id": "Serial input parallel output." },
  { "en": "Flip-flop PISO register adalah?", "id": "Parallel input serial output." },
  { "en": "Flip-flop PIPO register adalah?", "id": "Parallel input parallel output." },
  { "en": "Flip-flop synchronous reset aktif saat?", "id": "Clock edge dan reset high." },
  { "en": "Flip-flop asynchronous reset aktif saat?", "id": "Reset high tanpa clock." },
  { "en": "Flip-flop dengan enable input?", "id": "Aktif saat enable high." },
  { "en": "Flip-flop tanpa enable input?", "id": "Selalu aktif setiap clock." },
  { "en": "Flip-flop output impedance tinggi saat?", "id": "Dalam kondisi tri-state." },
  { "en": "Flip-flop tri-state dikontrol oleh?", "id": "Output enable signal control." },
  { "en": "Flip-flop dapat mengalami latch-up?", "id": "Ya, pada kondisi tertentu." },
  { "en": "Flip-flop latch-up disebabkan oleh?", "id": "Arus berlebih atau noise." },
  { "en": "Flip-flop protection circuit untuk?", "id": "Melindungi dari latch-up kondisi." },
  { "en": "Flip-flop input protection menggunakan?", "id": "Dioda clamp atau resistor." },
  { "en": "Flip-flop output protection menggunakan?", "id": "Current limiting resistor circuit." },
  { "en": "Flip-flop dapat parallel untuk?", "id": "Meningkatkan drive current capability." },
  { "en": "Flip-flop fan-out adalah?", "id": "Jumlah input yang bisa." },
  { "en": "Flip-flop fan-in adalah?", "id": "Jumlah input ke satu." },
  { "en": "Flip-flop loading effect mempengaruhi?", "id": "Kecepatan dan timing circuit." },
  { "en": "Flip-flop capacitive loading memperlambat?", "id": "Transisi output signal edge." },
  { "en": "Flip-flop resistive loading menurunkan?", "id": "Level tegangan output signal." },
  { "en": "Flip-flop dapat buffer dengan?", "id": "Driver circuit untuk beban." },
  { "en": "Flip-flop open-collector output memerlukan?", "id": "Pull-up resistor eksternal circuit." },
  { "en": "Flip-flop totem-pole output dapat?", "id": "Source dan sink current." },
  { "en": "Flip-flop wired-OR connection menggunakan?", "id": "Open-collector atau open-drain output." },
  { "en": "Flip-flop bus connection memerlukan?", "id": "Tri-state output untuk sharing." },
  { "en": "Flip-flop clock distribution memerlukan?", "id": "Buffer untuk multiple flip-flop." },
  { "en": "Flip-flop clock tree mengurangi?", "id": "Skew dan delay timing." },
  { "en": "Flip-flop clock gating menghemat?", "id": "Power consumption dynamic switching." },
  { "en": "Flip-flop power gating mematikan?", "id": "Supply voltage untuk hemat." },
  { "en": "Flip-flop retention flip-flop menyimpan?", "id": "Data saat power off." },
  { "en": "Flip-flop scan chain untuk?", "id": "Testing dan debug circuit." },
  { "en": "Flip-flop BIST adalah?", "id": "Built-in self test capability." },
  { "en": "Flip-flop DFT adalah?", "id": "Design for test methodology." },
  { "en": "Flip-flop JTAG menggunakan?", "id": "Boundary scan test method." },
  { "en": "Flip-flop dapat simulasi dengan?", "id": "SPICE atau digital simulator." },
  { "en": "Flip-flop modeling menggunakan?", "id": "Verilog atau VHDL language." },
  { "en": "Flip-flop synthesis menghasilkan?", "id": "Netlist untuk implementasi hardware." },
  { "en": "Flip-flop place and route?", "id": "Menentukan posisi pada chip." },
  { "en": "Flip-flop timing analysis memverifikasi?", "id": "Setup dan hold time." },
  { "en": "Flip-flop static timing analysis?", "id": "Verifikasi timing tanpa simulasi." },
  { "en": "Flip-flop dynamic timing analysis?", "id": "Verifikasi dengan pattern simulasi." },
  { "en": "Flip-flop corner analysis memverifikasi?", "id": "Kondisi ekstrem process voltage." },
  { "en": "Flip-flop Monte Carlo analysis?", "id": "Analisis variasi process statistical." },
  { "en": "Flip-flop yield analysis memperkirakan?", "id": "Persentase chip yang berfungsi." },
  { "en": "Flip-flop reliability analysis memperkirakan?", "id": "Lifetime dan failure rate." },
  { "en": "Flip-flop aging effect mempengaruhi?", "id": "Timing dan threshold voltage." },
  { "en": "Flip-flop temperature effect mempengaruhi?", "id": "Speed dan leakage current." },
  { "en": "Flip-flop voltage scaling mempengaruhi?", "id": "Speed dan power consumption." },
  { "en": "Flip-flop process variation mempengaruhi?", "id": "Timing dan power characteristics." },
  { "en": "Flip-flop mismatch variation mempengaruhi?", "id": "Offset dan gain accuracy." },
  { "en": "Flip-flop layout matching mengurangi?", "id": "Mismatch antar device identical." },
  { "en": "Flip-flop common centroid layout?", "id": "Teknik matching untuk symmetry." },
  { "en": "Flip-flop dummy fill mengurangi?", "id": "Process variation density effect." },
  { "en": "Flip-flop antenna effect dapat?", "id": "Merusak gate oxide transistor." },
  { "en": "Flip-flop antenna diode melindungi?", "id": "Gate dari charging damage." },
  { "en": "Flip-flop latch-up prevention menggunakan?", "id": "Guard ring dan substrate." },
  { "en": "Flip-flop ESD protection menggunakan?", "id": "Dioda dan resistor circuit." },
  { "en": "Flip-flop soft error disebabkan?", "id": "Radiasi cosmic ray particle." },
  { "en": "Flip-flop SEU adalah?", "id": "Single event upset error." },
  { "en": "Flip-flop radiation hardening menggunakan?", "id": "Special process dan design." },
  { "en": "Flip-flop TMR adalah?", "id": "Triple modular redundancy protection." },
  { "en": "Flip-flop error correction menggunakan?", "id": "Redundancy dan voting logic." },
  { "en": "Flip-flop parity check mendeteksi?", "id": "Single bit error data." },
  { "en": "Flip-flop ECC adalah?", "id": "Error correction code protection." },
  { "en": "Flip-flop scrubbing adalah?", "id": "Periodic refresh memory content." },
  { "en": "Flip-flop refresh rate tergantung?", "id": "Error rate dan requirement." },
  { "en": "Flip-flop dapat cascade untuk?", "id": "Pipeline processing data stream." },
  { "en": "Flip-flop pipeline meningkatkan?", "id": "Throughput processing data rate." },
  { "en": "Flip-flop pipeline latency adalah?", "id": "Delay dari input ke." },
  { "en": "Flip-flop pipeline hazard dapat?", "id": "Menyebabkan incorrect data result." },
  { "en": "Flip-flop forwarding mengatasi?", "id": "Data hazard pipeline processor." },
  { "en": "Flip-flop stall mengatasi?", "id": "Hazard dengan menunda pipeline." },
  { "en": "Flip-flop branch prediction mengurangi?", "id": "Pipeline stall control hazard." },
  { "en": "Flip-flop speculation execution menggunakan?", "id": "Prediction untuk performance improvement." },
  { "en": "Flip-flop out-of-order execution?", "id": "Eksekusi tidak berurutan instruction." },
  { "en": "Flip-flop register renaming mengatasi?", "id": "False dependency antar instruction." },
  { "en": "Flip-flop scoreboard tracking?", "id": "Status register untuk dependency." },
  { "en": "Flip-flop reservation station menyimpan?", "id": "Instruction menunggu operand ready." },
  { "en": "Flip-flop reorder buffer menyimpan?", "id": "Result untuk commit order." },
  { "en": "Flip-flop commit stage memastikan?", "id": "Instruction result permanent update." },
  { "en": "Flip-flop rollback mengembalikan?", "id": "State sebelum exception occurred." },
  { "en": "Flip-flop checkpoint menyimpan?", "id": "State untuk recovery purpose." },
  { "en": "Flip-flop recovery mechanism mengatasi?", "id": "Exception dan interrupt handling." },
  { "en": "Flip-flop interrupt handling menggunakan?", "id": "Special register untuk context." },
  { "en": "Flip-flop context switch menyimpan?", "id": "Register state process lama." },
  { "en": "Flip-flop virtual memory menggunakan?", "id": "Translation lookaside buffer TLB." },
  { "en": "Flip-flop TLB menyimpan?", "id": "Virtual ke physical address." },
  { "en": "Flip-flop page table menyimpan?", "id": "Mapping virtual memory address." },
  { "en": "Flip-flop cache memory menggunakan?", "id": "Tag dan data array." },
  { "en": "Flip-flop cache tag menyimpan?", "id": "Address identifier untuk data." },
  { "en": "Flip-flop cache data menyimpan?", "id": "Copy dari main memory." },
  { "en": "Flip-flop cache controller mengelola?", "id": "Hit miss dan replacement." },
  { "en": "Flip-flop write-back cache menunda?", "id": "Write ke memory main." },
  { "en": "Flip-flop write-through cache langsung?", "id": "Write ke memory main." },
  { "en": "Flip-flop dirty bit menandakan?", "id": "Cache line sudah modified." },
  { "en": "Flip-flop valid bit menandakan?", "id": "Cache line berisi data." },
  { "en": "Flip-flop LRU replacement menggunakan?", "id": "Least recently used algorithm." },
  { "en": "Flip-flop FIFO replacement menggunakan?", "id": "First in first out." },
  { "en": "Flip-flop random replacement menggunakan?", "id": "Random selection untuk simplicity." },
  { "en": "Flip-flop coherence protocol menjaga?", "id": "Consistency multi-core cache system." },
  { "en": "Flip-flop MESI protocol adalah?", "id": "Modified exclusive shared invalid." },
  { "en": "Flip-flop snooping protocol menggunakan?", "id": "Bus monitoring untuk coherence." },
  { "en": "Flip-flop directory protocol menggunakan?", "id": "Central directory untuk coherence." },
  { "en": "Flip-flop memory barrier memastikan?", "id": "Order memory access operation." },
  { "en": "Flip-flop fence instruction memastikan?", "id": "Memory ordering constraint compliance." },
  { "en": "Flip-flop atomic operation menggunakan?", "id": "Special instruction untuk synchronization." },
  { "en": "Flip-flop lock mechanism menggunakan?", "id": "Mutual exclusion untuk resource." },
  { "en": "Flip-flop semaphore menggunakan?", "id": "Counter untuk resource management." },
  { "en": "Flip-flop mutex menggunakan?", "id": "Binary semaphore untuk exclusion." },
  { "en": "Flip-flop condition variable menggunakan?", "id": "Signaling antar thread process." },
  { "en": "Flip-flop barrier synchronization menggunakan?", "id": "Wait point untuk thread." },
  { "en": "Flip-flop producer-consumer menggunakan?", "id": "Buffer dan synchronization primitive." },
  { "en": "Flip-flop reader-writer menggunakan?", "id": "Multiple reader single writer." },
  { "en": "Flip-flop deadlock dapat terjadi?", "id": "Saat circular wait condition." },
  { "en": "Flip-flop livelock dapat terjadi?", "id": "Saat thread saling mengalah." },
  { "en": "Flip-flop starvation dapat terjadi?", "id": "Saat thread tidak dapat." },
  { "en": "Flip-flop priority inversion terjadi saat?", "id": "Low priority block high priority." },
  { "en": "Flip-flop priority inheritance mengatasi?", "id": "Priority inversion problem solution." },
  { "en": "Flip-flop real-time system memerlukan?", "id": "Deterministic timing response guarantee." },
  { "en": "Flip-flop hard real-time memerlukan?", "id": "Strict deadline constraint compliance." },
  { "en": "Flip-flop soft real-time memerlukan?", "id": "Best effort deadline meeting." },
  { "en": "Flip-flop scheduler menentukan?", "id": "Task execution order priority." },
  { "en": "Flip-flop preemptive scheduler dapat?", "id": "Interrupt running task execution." },
  { "en": "Flip-flop non-preemptive scheduler menunggu?", "id": "Task completion sebelum switch." },
  { "en": "Flip-flop round-robin scheduler menggunakan?", "id": "Time slice untuk fairness." },
  { "en": "Flip-flop priority scheduler menggunakan?", "id": "Task priority untuk ordering." },
  { "en": "Flip-flop earliest deadline first?", "id": "Schedule berdasarkan deadline terdekat." },
  { "en": "Flip-flop rate monotonic scheduling?", "id": "Priority berdasarkan task period." },
  { "en": "Flip-flop interrupt latency adalah?", "id": "Delay dari interrupt ke." },
  { "en": "Flip-flop interrupt jitter adalah?", "id": "Variasi interrupt response time." },
  { "en": "Flip-flop nested interrupt memungkinkan?", "id": "Interrupt dalam interrupt handler." },
  { "en": "Flip-flop interrupt priority menentukan?", "id": "Urutan handling multiple interrupt." },
  { "en": "Flip-flop interrupt vector table?", "id": "Alamat handler untuk interrupt." },
  { "en": "Flip-flop exception handling menggunakan?", "id": "Special routine untuk error." },
  { "en": "Flip-flop trap instruction menghasilkan?", "id": "Software interrupt untuk system." },
  { "en": "Flip-flop system call menggunakan?", "id": "Trap untuk kernel service." },
  { "en": "Flip-flop privilege level menentukan?", "id": "Access right ke resource." },
  { "en": "Flip-flop user mode memiliki?", "id": "Restricted access ke system." },
  { "en": "Flip-flop kernel mode memiliki?", "id": "Full access ke system." },
  { "en": "Flip-flop supervisor mode memiliki?", "id": "Intermediate privilege level access." },
  { "en": "Flip-flop hypervisor mode memiliki?", "id": "Control over virtual machine." },
  { "en": "Flip-flop virtual machine menggunakan?", "id": "Hypervisor untuk resource sharing." },
  { "en": "Flip-flop container menggunakan?", "id": "OS kernel untuk isolation." },
  { "en": "Flip-flop process isolation menggunakan?", "id": "Virtual memory dan protection." },
  { "en": "Flip-flop thread sharing menggunakan?", "id": "Same address space memory." },
  { "en": "Flip-flop fiber adalah?", "id": "Lightweight thread user space." },
  { "en": "Flip-flop coroutine adalah?", "id": "Cooperative multitasking function unit." },
  { "en": "Flip-flop generator adalah?", "id": "Function dengan yield statement." },
  { "en": "Flip-flop async/await menggunakan?", "id": "Promise untuk asynchronous operation." },
  { "en": "Flip-flop event loop mengelola?", "id": "Asynchronous event dan callback." },
  { "en": "Flip-flop callback function dipanggil?", "id": "Saat event atau condition." },
  { "en": "Flip-flop promise represents?", "id": "Future result asynchronous operation." },
  { "en": "Flip-flop future represents?", "id": "Value yang akan tersedia." },
  { "en": "Flip-flop observable stream menghasilkan?", "id": "Sequence nilai over time." },
  { "en": "Flip-flop reactive programming menggunakan?", "id": "Data stream dan propagation." },
  { "en": "Flip-flop functional programming menggunakan?", "id": "Pure function tanpa side." },
  { "en": "Flip-flop immutable data tidak?", "id": "Dapat diubah setelah creation." },
  { "en": "Flip-flop persistent data structure?", "id": "Preserve previous version efficiently." },
  { "en": "Flip-flop copy-on-write menggunakan?", "id": "Lazy copying untuk efficiency." },
  { "en": "Flip-flop garbage collection menggunakan?", "id": "Automatic memory management system." },
  { "en": "Flip-flop reference counting menghitung?", "id": "Jumlah pointer ke object." },
  { "en": "Flip-flop mark and sweep?", "id": "Garbage collection algorithm type." },
  { "en": "Flip-flop generational garbage collection?", "id": "Separate young dan old." },
  { "en": "Flip-flop weak reference tidak?", "id": "Mencegah garbage collection object." },
  { "en": "Flip-flop strong reference mencegah?", "id": "Garbage collection referenced object." },
  { "en": "Flip-flop memory leak terjadi?", "id": "Saat memory tidak dibebaskan." },
  { "en": "Flip-flop dangling pointer menunjuk?", "id": "Memory yang sudah dibebaskan." },
  { "en": "Flip-flop buffer overflow terjadi?", "id": "Saat data melebihi buffer." },
  { "en": "Flip-flop stack overflow terjadi?", "id": "Saat stack melebihi limit." },
  { "en": "Flip-flop heap overflow terjadi?", "id": "Saat heap memory habis." },
  { "en": "Flip-flop segmentation fault terjadi?", "id": "Saat access invalid memory." },
  { "en": "Flip-flop null pointer dereference?", "id": "Access memory melalui null." },
  { "en": "Flip-flop use after free?", "id": "Access memory setelah dibebaskan." },
  { "en": "Flip-flop double free error?", "id": "Membebaskan memory dua kali." },
  { "en": "Flip-flop memory corruption terjadi?", "id": "Saat memory content rusak." },
  { "en": "Flip-flop data race terjadi?", "id": "Concurrent access tanpa synchronization." },
  { "en": "Flip-flop race condition terjadi?", "id": "Output tergantung timing execution." },
  { "en": "Flip-flop atomic operation memastikan?", "id": "Indivisible execution tanpa interruption." },
  { "en": "Flip-flop memory model menentukan?", "id": "Ordering memory access operation." },
  { "en": "Flip-flop sequential consistency memastikan?", "id": "Program order dan global." },
  { "en": "Flip-flop relaxed memory model?", "id": "Allow reordering untuk performance." },
  { "en": "Flip-flop acquire semantics memastikan?", "id": "No reordering sebelum acquire." },
  { "en": "Flip-flop release semantics memastikan?", "id": "No reordering setelah release." },
  { "en": "Flip-flop acquire-release pair memastikan?", "id": "Synchronization antar thread execution." },
  { "en": "Flip-flop memory fence mencegah?", "id": "Reordering memory access operation." },
  { "en": "Flip-flop load fence mencegah?", "id": "Reordering load operation sequence." },
  { "en": "Flip-flop store fence mencegah?", "id": "Reordering store operation sequence." },
  { "en": "Flip-flop full fence mencegah?", "id": "Reordering load dan store." },
  { "en": "Flip-flop compiler barrier mencegah?", "id": "Compiler reordering instruction sequence." },
  { "en": "Flip-flop volatile keyword mencegah?", "id": "Compiler optimization pada variable." },
  { "en": "Flip-flop register keyword menyarankan?", "id": "Compiler untuk register allocation." },
  { "en": "Flip-flop inline function menyarankan?", "id": "Compiler untuk inline expansion." },
  { "en": "Flip-flop static variable memiliki?", "id": "Lifetime sepanjang program execution." },
  { "en": "Flip-flop global variable dapat?", "id": "Diakses dari semua function." },
  { "en": "Flip-flop local variable memiliki?", "id": "Scope terbatas pada function." },
  { "en": "Flip-flop automatic variable dialokasi?", "id": "Di stack saat function." },
  { "en": "Flip-flop dynamic allocation menggunakan?", "id": "Heap memory untuk variable." },
  { "en": "Flip-flop stack allocation menggunakan?", "id": "Stack memory untuk local." },
  { "en": "Flip-flop heap allocation menggunakan?", "id": "Dynamic memory untuk object." },
  { "en": "Flip-flop memory pool menggunakan?", "id": "Pre-allocated memory untuk efficiency." },
  { "en": "Flip-flop object pool menggunakan?", "id": "Reusable object untuk performance." },
  { "en": "Flip-flop thread pool menggunakan?", "id": "Reusable thread untuk task." },
  { "en": "Flip-flop connection pool menggunakan?", "id": "Reusable connection untuk database." },
  { "en": "Flip-flop resource pooling mengurangi?", "id": "Overhead creation dan destruction." },
  { "en": "Flip-flop lazy initialization menunda?", "id": "Object creation sampai dibutuhkan." },
  { "en": "Flip-flop eager initialization membuat?", "id": "Object saat program startup." },
  { "en": "Flip-flop singleton pattern memastikan?", "id": "Hanya satu instance object." },
  { "en": "Flip-flop factory pattern membuat?", "id": "Object tanpa specify class." },
  { "en": "Flip-flop builder pattern membuat?", "id": "Complex object step by." },
  { "en": "Flip-flop prototype pattern membuat?", "id": "Object dengan cloning existing." },
  { "en": "Flip-flop adapter pattern mengubah?", "id": "Interface untuk compatibility purpose." },
  { "en": "Flip-flop decorator pattern menambah?", "id": "Functionality tanpa modify object." },
  { "en": "Flip-flop facade pattern menyediakan?", "id": "Simple interface untuk complex." },
  { "en": "Flip-flop proxy pattern menyediakan?", "id": "Placeholder untuk object lain." },
  { "en": "Flip-flop observer pattern memungkinkan?", "id": "Object notify change ke." },
  { "en": "Flip-flop strategy pattern memungkinkan?", "id": "Algorithm selection at runtime." },
  { "en": "Flip-flop command pattern encapsulate?", "id": "Request sebagai object untuk." },
  { "en": "Flip-flop state pattern memungkinkan?", "id": "Object change behavior saat." },
  { "en": "Flip-flop template method pattern?", "id": "Define algorithm skeleton dengan." },
  { "en": "Flip-flop visitor pattern memungkinkan?", "id": "Operation pada object tanpa." },
  { "en": "Flip-flop iterator pattern menyediakan?", "id": "Access element collection sequentially." },
  { "en": "Flip-flop composite pattern menyusun?", "id": "Object dalam tree structure." },
  { "en": "Flip-flop chain of responsibility?", "id": "Pass request along handler." },
  { "en": "Flip-flop mediator pattern mengurangi?", "id": "Coupling antar object communication." },
  { "en": "Flip-flop memento pattern menyimpan?", "id": "Object state untuk restore." },
  { "en": "Flip-flop interpreter pattern mengimplementasi?", "id": "Grammar untuk language processing." },
  { "en": "Flip-flop dapat dibuat dengan?", "id": "Relay mechanical switching device." },
  { "en": "Flip-flop relay memerlukan?", "id": "Mechanical contact untuk switching." },
  { "en": "Flip-flop vacuum tube menggunakan?", "id": "Electron flow dalam vacuum." },
  { "en": "Flip-flop transistor menggunakan?", "id": "Semiconductor junction untuk switching." },
  { "en": "Flip-flop BJT menggunakan?", "id": "Bipolar junction transistor technology." },
  { "en": "Flip-flop MOSFET menggunakan?", "id": "Metal oxide semiconductor technology." },
  { "en": "Flip-flop JFET menggunakan?", "id": "Junction field effect transistor." },
  { "en": "Flip-flop MESFET menggunakan?", "id": "Metal semiconductor field effect." },
  { "en": "Flip-flop HEMT menggunakan?", "id": "High electron mobility transistor." },
  { "en": "Flip-flop FinFET menggunakan?", "id": "Three dimensional transistor structure." },
  { "en": "Flip-flop carbon nanotube menggunakan?", "id": "Carbon nanotube untuk switching." },
  { "en": "Flip-flop graphene menggunakan?", "id": "Graphene material untuk transistor." },
  { "en": "Flip-flop quantum dot menggunakan?", "id": "Quantum confinement untuk switching." },
  { "en": "Flip-flop single electron transistor?", "id": "Control single electron flow." },
  { "en": "Flip-flop molecular electronics menggunakan?", "id": "Molecule untuk switching function." },
  { "en": "Flip-flop DNA computing menggunakan?", "id": "DNA strand untuk computation." },
  { "en": "Flip-flop optical computing menggunakan?", "id": "Light untuk information processing." },
  { "en": "Flip-flop photonic crystal menggunakan?", "id": "Light confinement untuk switching." },
  { "en": "Flip-flop plasmonics menggunakan?", "id": "Surface plasmon untuk switching." },
  { "en": "Flip-flop metamaterial menggunakan?", "id": "Artificial material untuk control." },
  { "en": "Flip-flop spintronics menggunakan?", "id": "Electron spin untuk information." },
  { "en": "Flip-flop magnetic tunnel junction?", "id": "Spin dependent tunneling resistance." },
  { "en": "Flip-flop giant magnetoresistance menggunakan?", "id": "Magnetic field dependent resistance." },
  { "en": "Flip-flop spin valve menggunakan?", "id": "Magnetic layer untuk switching." },
  { "en": "Flip-flop magnetic random access?", "id": "Non-volatile memory dengan magnetism." },
  { "en": "Flip-flop phase change memory?", "id": "Crystalline amorphous state switching." },
  { "en": "Flip-flop resistive random access?", "id": "Resistance switching untuk memory." },
  { "en": "Flip-flop ferroelectric random access?", "id": "Ferroelectric polarization untuk memory." },
  { "en": "Flip-flop memristor menggunakan?", "id": "Resistance dengan memory effect." },
  { "en": "Flip-flop neuromorphic computing menggunakan?", "id": "Brain inspired computing architecture." },
  { "en": "Flip-flop spiking neural network?", "id": "Neuron communication dengan spike." },
  { "en": "Flip-flop artificial synapse menggunakan?", "id": "Plastic connection antar neuron." },
  { "en": "Flip-flop reservoir computing menggunakan?", "id": "Random network untuk computation." },
  { "en": "Flip-flop liquid state machine?", "id": "Recurrent network dengan memory." },
  { "en": "Flip-flop echo state network?", "id": "Reservoir dengan trained output." },
  { "en": "Flip-flop cellular automata menggunakan?", "id": "Local rule untuk global." },
  { "en": "Flip-flop game of life?", "id": "Cellular automata dengan simple." },
  { "en": "Flip-flop rule 110 adalah?", "id": "Turing complete cellular automata." },
  { "en": "Flip-flop Langton's ant adalah?", "id": "Two dimensional cellular automata." },
  { "en": "Flip-flop Conway's game menggunakan?", "id": "Birth death rule untuk." },
  { "en": "Flip-flop Wolfram code mengklasifikasi?", "id": "Cellular automata behavior pattern." },
  { "en": "Flip-flop fractal computation menggunakan?", "id": "Self similar pattern untuk." },
  { "en": "Flip-flop chaos theory menggunakan?", "id": "Sensitive dependence initial condition." },
  { "en": "Flip-flop strange attractor adalah?", "id": "Chaotic system long term." },
  { "en": "Flip-flop butterfly effect adalah?", "id": "Small change large consequence." },
  { "en": "Flip-flop Lorenz attractor adalah?", "id": "Chaotic system weather model." },
  { "en": "Flip-flop logistic map adalah?", "id": "Simple chaotic dynamical system." },
  { "en": "Flip-flop bifurcation diagram menunjukkan?", "id": "Parameter change system behavior." },
  { "en": "Flip-flop period doubling route?", "id": "Path ke chaos melalui." },
  { "en": "Flip-flop Feigenbaum constant adalah?", "id": "Universal constant chaos theory." },
  { "en": "Flip-flop edge of chaos?", "id": "Transition antara order dan." },
  { "en": "Flip-flop self organized criticality?", "id": "System naturally evolve ke." },
  { "en": "Flip-flop sandpile model adalah?", "id": "Example self organized criticality." },
  { "en": "Flip-flop power law distribution?", "id": "Scale free network property." },
  { "en": "Flip-flop small world network?", "id": "High clustering short path." },
  { "en": "Flip-flop scale free network?", "id": "Power law degree distribution." },
  { "en": "Flip-flop preferential attachment menghasilkan?", "id": "Scale free network topology." },
  { "en": "Flip-flop random graph model?", "id": "Erdos Renyi network model." },
  { "en": "Flip-flop Watts Strogatz model?", "id": "Small world network generation." },
  { "en": "Flip-flop Barabasi Albert model?", "id": "Scale free network generation." },
  { "en": "Flip-flop network topology mempengaruhi?", "id": "Information flow dan robustness." },
  { "en": "Flip-flop centrality measure mengukur?", "id": "Node importance dalam network." },
  { "en": "Flip-flop betweenness centrality mengukur?", "id": "Node position shortest path." },
  { "en": "Flip-flop closeness centrality mengukur?", "id": "Average distance ke node." },
  { "en": "Flip-flop degree centrality mengukur?", "id": "Number connection per node." },
  { "en": "Flip-flop eigenvector centrality mengukur?", "id": "Connection ke important node." },
  { "en": "Flip-flop PageRank algorithm mengukur?", "id": "Node importance dengan random." },
  { "en": "Flip-flop community detection mengidentifikasi?", "id": "Dense connection group node." },
  { "en": "Flip-flop modularity measure mengukur?", "id": "Quality community structure network." },
  { "en": "Flip-flop clustering coefficient mengukur?", "id": "Local density connection node." },
  { "en": "Flip-flop path length mengukur?", "id": "Distance antar node network." },
  { "en": "Flip-flop diameter network adalah?", "id": "Maximum shortest path length." },
  { "en": "Flip-flop radius network adalah?", "id": "Minimum eccentricity node network." },
  { "en": "Flip-flop connectivity mengukur?", "id": "Robustness network terhadap failure." },
  { "en": "Flip-flop percolation threshold adalah?", "id": "Critical point network connectivity." },
  { "en": "Flip-flop giant component adalah?", "id": "Largest connected component network." },
  { "en": "Flip-flop phase transition terjadi?", "id": "Saat parameter melewati threshold." },
  { "en": "Flip-flop critical phenomena menggunakan?", "id": "Statistical mechanics untuk analysis." },
  { "en": "Flip-flop renormalization group theory?", "id": "Scale invariance critical point." },
  { "en": "Flip-flop universality class mengkelompokkan?", "id": "System dengan critical exponent." },
  { "en": "Flip-flop scaling law menghubungkan?", "id": "System property dengan size." },
  { "en": "Flip-flop finite size scaling?", "id": "Extrapolate infinite system property." },
  { "en": "Flip-flop Monte Carlo simulation?", "id": "Random sampling untuk computation." },
  { "en": "Flip-flop Metropolis algorithm adalah?", "id": "Monte Carlo sampling method." },
  { "en": "Flip-flop simulated annealing menggunakan?", "id": "Temperature cooling untuk optimization." },
  { "en": "Flip-flop genetic algorithm menggunakan?", "id": "Evolution untuk optimization problem." },
  { "en": "Flip-flop particle swarm optimization?", "id": "Swarm intelligence untuk optimization." },
  { "en": "Flip-flop ant colony optimization?", "id": "Ant behavior untuk path." },
  { "en": "Flip-flop differential evolution menggunakan?", "id": "Population based optimization algorithm." },
  { "en": "Flip-flop evolutionary strategy menggunakan?", "id": "Mutation dan selection untuk." },
  { "en": "Flip-flop genetic programming menggunakan?", "id": "Evolution untuk program generation." },
  { "en": "Flip-flop neural evolution menggunakan?", "id": "Evolution untuk neural network." },
  { "en": "Flip-flop neuroevolution mengoptimalkan?", "id": "Neural network structure dan." },
  { "en": "Flip-flop reinforcement learning menggunakan?", "id": "Reward signal untuk learning." },
  { "en": "Flip-flop Q-learning adalah?", "id": "Model free reinforcement learning." },
  { "en": "Flip-flop temporal difference learning?", "id": "Update value dengan prediction." },
  { "en": "Flip-flop policy gradient method?", "id": "Direct optimization policy parameter." },
  { "en": "Flip-flop actor critic method?", "id": "Combine value dan policy." },
  { "en": "Flip-flop deep reinforcement learning?", "id": "Neural network untuk function." },
  { "en": "Flip-flop deep Q network?", "id": "Deep learning untuk Q." },
  { "en": "Flip-flop convolutional neural network?", "id": "Convolution untuk spatial pattern." },
  { "en": "Flip-flop recurrent neural network?", "id": "Feedback connection untuk sequence." },
  { "en": "Flip-flop long short term?", "id": "RNN dengan memory cell." },
  { "en": "Flip-flop gated recurrent unit?", "id": "Simplified LSTM architecture design." },
  { "en": "Flip-flop transformer architecture menggunakan?", "id": "Attention mechanism untuk sequence." },
  { "en": "Flip-flop attention mechanism memungkinkan?", "id": "Focus pada relevant input." },
  { "en": "Flip-flop self attention menghitung?", "id": "Attention dalam same sequence." },
  { "en": "Flip-flop multi head attention?", "id": "Multiple attention dalam parallel." },
  { "en": "Flip-flop positional encoding menambah?", "id": "Position information ke input." },
  { "en": "Flip-flop layer normalization menggunakan?", "id": "Normalize activation per layer." },
  { "en": "Flip-flop residual connection memungkinkan?", "id": "Skip connection untuk gradient." },
  { "en": "Flip-flop dropout regularization mencegah?", "id": "Overfitting dengan random neuron." },
  { "en": "Flip-flop batch normalization menggunakan?", "id": "Normalize input per batch." },
  { "en": "Flip-flop weight decay menambah?", "id": "L2 penalty untuk weight." },
  { "en": "Flip-flop early stopping mencegah?", "id": "Overfitting dengan validation monitoring." },
  { "en": "Flip-flop cross validation menggunakan?", "id": "Multiple fold untuk evaluation." },
  { "en": "Flip-flop hyperparameter tuning mengoptimalkan?", "id": "Model parameter untuk performance." },
  { "en": "Flip-flop grid search mencoba?", "id": "All combination parameter value." },
  { "en": "Flip-flop random search mencoba?", "id": "Random combination parameter value." },
  { "en": "Flip-flop Bayesian optimization menggunakan?", "id": "Gaussian process untuk optimization." },
  { "en": "Flip-flop gradient descent mengoptimalkan?", "id": "Parameter dengan gradient information." },
  { "en": "Flip-flop stochastic gradient descent?", "id": "Random sample untuk gradient." },
  { "en": "Flip-flop mini batch gradient?", "id": "Small batch untuk gradient." },
  { "en": "Flip-flop momentum gradient descent?", "id": "Previous gradient untuk acceleration." },
  { "en": "Flip-flop Nesterov accelerated gradient?", "id": "Look ahead untuk momentum." },
  { "en": "Flip-flop AdaGrad menggunakan?", "id": "Adaptive learning rate per." },
  { "en": "Flip-flop RMSprop menggunakan?", "id": "Exponential moving average gradient." },
  { "en": "Flip-flop Adam optimizer menggunakan?", "id": "Adaptive moment estimation method." },
  { "en": "Flip-flop AdamW menggunakan?", "id": "Adam dengan weight decay." },
  { "en": "Flip-flop learning rate schedule?", "id": "Change learning rate over." },
  { "en": "Flip-flop cosine annealing menggunakan?", "id": "Cosine function untuk learning." },
  { "en": "Flip-flop warm restart menggunakan?", "id": "Periodic learning rate reset." },
  { "en": "Flip-flop cyclical learning rate?", "id": "Oscillate learning rate antara." },
  { "en": "Flip-flop one cycle policy?", "id": "Single cycle learning rate." },
  { "en": "Flip-flop transfer learning menggunakan?", "id": "Pre-trained model untuk task." },
  { "en": "Flip-flop fine tuning menggunakan?", "id": "Small learning rate untuk." },
  { "en": "Flip-flop feature extraction menggunakan?", "id": "Fixed pre-trained feature extractor." },
  { "en": "Flip-flop domain adaptation menggunakan?", "id": "Source domain untuk target." },
  { "en": "Flip-flop few shot learning?", "id": "Learn dengan few training." },
  { "en": "Flip-flop zero shot learning?", "id": "Learn tanpa training example." },
  { "en": "Flip-flop meta learning menggunakan?", "id": "Learn to learn algorithm." },
  { "en": "Flip-flop model agnostic meta?", "id": "Meta learning untuk any." },
  { "en": "Flip-flop continual learning menggunakan?", "id": "Sequential task tanpa forgetting." },
  { "en": "Flip-flop catastrophic forgetting adalah?", "id": "Forget previous task saat." },
  { "en": "Flip-flop elastic weight consolidation?", "id": "Protect important weight dari." },
  { "en": "Flip-flop progressive neural network?", "id": "Add new column untuk." },
  { "en": "Flip-flop memory replay menggunakan?", "id": "Store example untuk rehearsal." },
  { "en": "Flip-flop generative replay menggunakan?", "id": "Generate example untuk rehearsal." },
  { "en": "Flip-flop federated learning menggunakan?", "id": "Distributed training tanpa data." },
  { "en": "Flip-flop differential privacy menggunakan?", "id": "Noise untuk privacy protection." },
  { "en": "Flip-flop homomorphic encryption memungkinkan?", "id": "Computation pada encrypted data." },
  { "en": "Flip-flop secure multiparty computation?", "id": "Joint computation tanpa reveal." },
  { "en": "Flip-flop zero knowledge proof?", "id": "Prove knowledge tanpa reveal." },
  { "en": "Flip-flop blockchain menggunakan?", "id": "Distributed ledger untuk trust." },
  { "en": "Flip-flop smart contract menggunakan?", "id": "Self executing contract dengan." },
  { "en": "Flip-flop consensus mechanism menggunakan?", "id": "Agreement dalam distributed system." },
  { "en": "Flip-flop proof of work?", "id": "Computational puzzle untuk consensus." },
  { "en": "Flip-flop proof of stake?", "id": "Stake based consensus mechanism." },
  { "en": "Flip-flop delegated proof of?", "id": "Representative based consensus mechanism." },
  { "en": "Flip-flop practical Byzantine fault?", "id": "Consensus dengan Byzantine failure." },
  { "en": "Flip-flop Raft consensus algorithm?", "id": "Leader based consensus protocol." },
  { "en": "Flip-flop Paxos consensus algorithm?", "id": "Fault tolerant consensus protocol." },
  { "en": "Flip-flop distributed hash table?", "id": "Decentralized key value storage." },
  { "en": "Flip-flop consistent hashing menggunakan?", "id": "Hash ring untuk load." },
  { "en": "Flip-flop gossip protocol menggunakan?", "id": "Epidemic information dissemination method." },
  { "en": "Flip-flop vector clock menggunakan?", "id": "Logical time untuk ordering." },
  { "en": "Flip-flop Lamport timestamp menggunakan?", "id": "Logical clock untuk event." },
  { "en": "Flip-flop causal ordering menggunakan?", "id": "Happen before relation untuk." },
  { "en": "Flip-flop eventual consistency memungkinkan?", "id": "Temporary inconsistency untuk availability." },
  { "en": "Flip-flop strong consistency memastikan?", "id": "All replica same value." },
  { "en": "Flip-flop weak consistency memungkinkan?", "id": "Temporary inconsistency antar replica." },
  { "en": "Flip-flop CAP theorem menyatakan?", "id": "Consistency availability partition tolerance." },
  { "en": "Flip-flop ACID property memastikan?", "id": "Atomicity consistency isolation durability." },
  { "en": "Flip-flop BASE property memungkinkan?", "id": "Basically available soft state." },
  { "en": "Flip-flop two phase commit?", "id": "Distributed transaction commit protocol." },
  { "en": "Flip-flop three phase commit?", "id": "Non blocking commit protocol." },
  { "en": "Flip-flop saga pattern menggunakan?", "id": "Compensating action untuk transaction." },
  { "en": "Flip-flop event sourcing menggunakan?", "id": "Event sequence untuk state." },
  { "en": "Flip-flop CQRS pattern memisahkan?", "id": "Command dan query responsibility." },
  { "en": "Flip-flop microservice architecture menggunakan?", "id": "Small independent service deployment." },
  { "en": "Flip-flop service mesh menggunakan?", "id": "Infrastructure layer untuk service." },
  { "en": "Flip-flop API gateway menggunakan?", "id": "Single entry point untuk." },
  { "en": "Flip-flop load balancer menggunakan?", "id": "Distribute request antar server." },
  { "en": "Flip-flop circuit breaker menggunakan?", "id": "Prevent cascade failure system." },
  { "en": "Flip-flop bulkhead pattern menggunakan?", "id": "Isolate resource untuk fault." },
  { "en": "Flip-flop timeout pattern menggunakan?", "id": "Time limit untuk operation." },
  { "en": "Flip-flop retry pattern menggunakan?", "id": "Repeat failed operation dengan." },
  { "en": "Flip-flop exponential backoff menggunakan?", "id": "Increasing delay antar retry." },
  { "en": "Flip-flop jitter menggunakan?", "id": "Random delay untuk avoid." },
  { "en": "Flip-flop rate limiting menggunakan?", "id": "Control request rate per." },
  { "en": "Flip-flop throttling menggunakan?", "id": "Limit resource usage untuk." },
  { "en": "Flip-flop caching menggunakan?", "id": "Store frequently accessed data." },
  { "en": "Flip-flop cache aside pattern?", "id": "Application manage cache explicitly." },
  { "en": "Flip-flop write through cache?", "id": "Write cache dan database." },
  { "en": "Flip-flop write behind cache?", "id": "Asynchronous write ke database." },
  { "en": "Flip-flop refresh ahead cache?", "id": "Proactive refresh sebelum expiry." },
  { "en": "Flip-flop cache stampede terjadi?", "id": "Multiple request same expired." },
  { "en": "Flip-flop cache warming menggunakan?", "id": "Pre-load cache dengan data." },
  { "en": "Flip-flop cache invalidation menggunakan?", "id": "Remove stale data dari." },
  { "en": "Flip-flop cache coherence memastikan?", "id": "Consistency antar cache instance." },
  { "en": "Flip-flop distributed cache menggunakan?", "id": "Multiple node untuk cache." },
  { "en": "Flip-flop Redis menggunakan?", "id": "In memory data structure." },
  { "en": "Flip-flop Memcached menggunakan?", "id": "Distributed memory caching system." },
  { "en": "Flip-flop Hazelcast menggunakan?", "id": "In memory data grid." },
  { "en": "Flip-flop Apache Ignite menggunakan?", "id": "In memory computing platform." },
  { "en": "Flip-flop content delivery network?", "id": "Geographically distributed cache server." },
  { "en": "Flip-flop edge computing menggunakan?", "id": "Computation near data source." },
  { "en": "Flip-flop fog computing menggunakan?", "id": "Intermediate layer antara cloud." },
  { "en": "Flip-flop cloud computing menggunakan?", "id": "Remote server untuk computation." },
  { "en": "Flip-flop serverless computing menggunakan?", "id": "Function as service model." },
  { "en": "Flip-flop container orchestration menggunakan?", "id": "Manage container deployment scaling." },
  { "en": "Flip-flop Kubernetes menggunakan?", "id": "Container orchestration platform system." },
  { "en": "Flip-flop Docker menggunakan?", "id": "Container platform untuk application." },
  { "en": "Flip-flop virtual machine menggunakan?", "id": "Hardware abstraction untuk isolation." },
  { "en": "Flip-flop hypervisor menggunakan?", "id": "Manage multiple virtual machine." },
  { "en": "Flip-flop bare metal server?", "id": "Physical server tanpa virtualization." },
  { "en": "Flip-flop infrastructure as code?", "id": "Manage infrastructure dengan code." },
  { "en": "Flip-flop configuration management menggunakan?", "id": "Automate system configuration deployment." },
  { "en": "Flip-flop continuous integration menggunakan?", "id": "Automate build test deployment." },
  { "en": "Flip-flop continuous deployment menggunakan?", "id": "Automate release ke production." },
  { "en": "Flip-flop DevOps menggunakan?", "id": "Collaboration development dan operation." },
  { "en": "Flip-flop site reliability engineering?", "id": "Apply software engineering untuk." },
  { "en": "Flip-flop chaos engineering menggunakan?", "id": "Intentional failure untuk test." },
  { "en": "Flip-flop fault injection menggunakan?", "id": "Introduce error untuk testing." },
  { "en": "Flip-flop disaster recovery menggunakan?", "id": "Plan untuk system failure." },
  { "en": "Flip-flop business continuity menggunakan?", "id": "Maintain operation during disruption." },
  { "en": "Flip-flop backup strategy menggunakan?", "id": "Regular data copy untuk." },
  { "en": "Flip-flop replication menggunakan?", "id": "Multiple copy data untuk." },
  { "en": "Flip-flop high availability menggunakan?", "id": "Redundancy untuk minimize downtime." },
  { "en": "Flip-flop fault tolerance menggunakan?", "id": "Continue operation despite failure." },
  { "en": "Flip-flop graceful degradation menggunakan?", "id": "Reduce functionality saat failure." },
  { "en": "Flip-flop fail fast principle?", "id": "Quick failure detection dan." },
  { "en": "Flip-flop fail safe design?", "id": "Safe state saat failure." },
  { "en": "Flip-flop redundancy menggunakan?", "id": "Multiple component untuk backup." },
  { "en": "Flip-flop hot standby menggunakan?", "id": "Active backup ready untuk." },
  { "en": "Flip-flop cold standby menggunakan?", "id": "Inactive backup activated saat." },
  { "en": "Flip-flop warm standby menggunakan?", "id": "Partially active backup untuk." },
  { "en": "Flip-flop active active configuration?", "id": "Multiple active instance untuk." },
  { "en": "Flip-flop active passive configuration?", "id": "One active one standby." },
  { "en": "Flip-flop master slave configuration?", "id": "Master control slave follow." },
  { "en": "Flip-flop master master configuration?", "id": "Multiple master untuk load." },
  { "en": "Flip-flop leader election menggunakan?", "id": "Choose leader dari multiple." },
  { "en": "Flip-flop split brain problem?", "id": "Multiple leader dalam cluster." },
  { "en": "Flip-flop quorum consensus menggunakan?", "id": "Majority vote untuk decision." },
  { "en": "Flip-flop witness node menggunakan?", "id": "Tie breaker untuk quorum." },
  { "en": "Flip-flop heartbeat mechanism menggunakan?", "id": "Periodic signal untuk liveness." },
  { "en": "Flip-flop health check menggunakan?", "id": "Monitor system component status." },
  { "en": "Flip-flop monitoring system menggunakan?", "id": "Collect metric untuk observation." },
  { "en": "Flip-flop alerting system menggunakan?", "id": "Notification saat threshold exceeded." },
  { "en": "Flip-flop logging system menggunakan?", "id": "Record event untuk debugging." },
  { "en": "Flip-flop tracing system menggunakan?", "id": "Track request flow antar." },
  { "en": "Flip-flop profiling menggunakan?", "id": "Analyze performance bottleneck application." },
  { "en": "Flip-flop benchmarking menggunakan?", "id": "Compare performance antar system." },
  { "en": "Flip-flop load testing menggunakan?", "id": "Simulate high load untuk." },
  { "en": "Flip-flop stress testing menggunakan?", "id": "Test system beyond normal." },
  { "en": "Flip-flop performance testing menggunakan?", "id": "Measure system response time." },
  { "en": "Flip-flop scalability testing menggunakan?", "id": "Test system scale capability." },
  { "en": "Flip-flop capacity planning menggunakan?", "id": "Estimate resource requirement untuk." },
  { "en": "Flip-flop resource allocation menggunakan?", "id": "Distribute resource antar component." },
  { "en": "Flip-flop auto scaling menggunakan?", "id": "Automatic resource adjustment based." },
  { "en": "Flip-flop horizontal scaling menggunakan?", "id": "Add more instance untuk." },
  { "en": "Flip-flop vertical scaling menggunakan?", "id": "Add more resource ke." },
  { "en": "Flip-flop elastic scaling menggunakan?", "id": "Dynamic resource adjustment based." },
  { "en": "Flip-flop predictive scaling menggunakan?", "id": "Forecast untuk proactive scaling." },
  { "en": "Flip-flop reactive scaling menggunakan?", "id": "Response ke current load." },
  { "en": "Flip-flop scheduled scaling menggunakan?", "id": "Pre-planned scaling based time." },
  { "en": "Flip-flop cost optimization menggunakan?", "id": "Minimize expense untuk same." },
  { "en": "Flip-flop resource utilization mengukur?", "id": "Efficiency resource usage system." },
  { "en": "Flip-flop right sizing menggunakan?", "id": "Optimal resource allocation untuk." },
  { "en": "Flip-flop spot instance menggunakan?", "id": "Cheaper compute dengan availability." },
  { "en": "Flip-flop reserved instance menggunakan?", "id": "Long term commitment untuk." },
  { "en": "Flip-flop on demand instance?", "id": "Pay per use compute." },
  { "en": "Flip-flop serverless function menggunakan?", "id": "Event driven execution model." },
  { "en": "Flip-flop cold start problem?", "id": "Initialization delay serverless function." },
  { "en": "Flip-flop warm start menggunakan?", "id": "Pre-initialized function untuk speed." },
  { "en": "Flip-flop function as service?", "id": "Serverless computing execution model." },
  { "en": "Flip-flop platform as service?", "id": "Development platform dalam cloud." },
  { "en": "Flip-flop infrastructure as service?", "id": "Virtual infrastructure dalam cloud." },
  { "en": "Flip-flop software as service?", "id": "Application delivery melalui internet." },
  { "en": "Flip-flop multi tenant architecture?", "id": "Single instance serve multiple." },
  { "en": "Flip-flop single tenant architecture?", "id": "Dedicated instance per customer." },
  { "en": "Flip-flop hybrid cloud menggunakan?", "id": "Combination public dan private." },
  { "en": "Flip-flop multi cloud menggunakan?", "id": "Multiple cloud provider untuk." },
  { "en": "Flip-flop cloud native application?", "id": "Design untuk cloud environment." },
  { "en": "Flip-flop twelve factor app?", "id": "Methodology untuk cloud native." },
  { "en": "Flip-flop stateless application menggunakan?", "id": "No persistent state antar." },
  { "en": "Flip-flop stateful application menggunakan?", "id": "Persistent state antar request." },
  { "en": "Flip-flop session affinity menggunakan?", "id": "Route request ke same." },
  { "en": "Flip-flop sticky session menggunakan?", "id": "Bind session ke specific." },
  { "en": "Flip-flop session replication menggunakan?", "id": "Copy session antar server." },
  { "en": "Flip-flop session clustering menggunakan?", "id": "Share session antar cluster." },
  { "en": "Flip-flop external session store?", "id": "Separate storage untuk session." },
  { "en": "Flip-flop JWT token menggunakan?", "id": "Self contained token untuk." },
  { "en": "Flip-flop OAuth protocol menggunakan?", "id": "Authorization framework untuk access." },
  { "en": "Flip-flop OpenID Connect menggunakan?", "id": "Identity layer pada OAuth." },
  { "en": "Flip-flop SAML protocol menggunakan?", "id": "XML based authentication protocol." },
  { "en": "Flip-flop single sign on?", "id": "One authentication multiple application." },
  { "en": "Flip-flop multi factor authentication?", "id": "Multiple verification method untuk." },
  { "en": "Flip-flop role based access?", "id": "Permission based user role." }




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
            }, 6500);
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
