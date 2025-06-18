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


  { "en": "Rumah Krong Bade?", "id": "Aceh." },
  { "en": "Rumah Bolon?", "id": "Sumatera Utara." },
  { "en": "Rumah Gadang?", "id": "Sumatera Barat." },
  { "en": "Rumah Selaso Jatuh Kembar?", "id": "Riau." },
  { "en": "Rumah Panggung Kajang Leko?", "id": "Jambi." },
  { "en": "Rumah Limas?", "id": "Sumatera Selatan." },
  { "en": "Rumah Rakit?", "id": "Bangka Belitung." },
  { "en": "Rumah Bubungan Lima?", "id": "Bengkulu." },
  { "en": "Rumah Nuwo Sesat?", "id": "Lampung." },
  { "en": "Rumah Belah Bubung?", "id": "Kepulauan Riau." },
  { "en": "Rumah Kebaya?", "id": "DKI Jakarta." },
  { "en": "Rumah Kasepuhan?", "id": "Jawa Barat." },
  { "en": "Rumah Baduy?", "id": "Banten." },
  { "en": "Rumah Joglo?", "id": "Jawa Tengah." },
  { "en": "Rumah Bangsal Kencono?", "id": "DI Yogyakarta." },
  { "en": "Rumah Joglo Situbondo?", "id": "Jawa Timur." },
  { "en": "Gapura Candi Bentar?", "id": "Bali." },
  { "en": "Rumah Dalam Loka?", "id": "Nusa Tenggara Barat." },
  { "en": "Rumah Musalaki?", "id": "Nusa Tenggara Timur." },
  { "en": "Rumah Panjang?", "id": "Kalimantan Barat." },
  { "en": "Rumah Betang?", "id": "Kalimantan Tengah." },
  { "en": "Rumah Bubungan Tinggi?", "id": "Kalimantan Selatan." },
  { "en": "Rumah Lamin?", "id": "Kalimantan Timur." },
  { "en": "Rumah Baloy?", "id": "Kalimantan Utara." },
  { "en": "Rumah Walewangko?", "id": "Sulawesi Utara." },
  { "en": "Rumah Dulohupa?", "id": "Gorontalo." },
  { "en": "Rumah Tambi?", "id": "Sulawesi Tengah." },
  { "en": "Rumah Boyang?", "id": "Sulawesi Barat." },
  { "en": "Rumah Tongkonan?", "id": "Sulawesi Selatan." },
  { "en": "Rumah Buton?", "id": "Sulawesi Tenggara." },
  { "en": "Rumah Baileo?", "id": "Maluku." },
  { "en": "Rumah Sasadu?", "id": "Maluku Utara." },
  { "en": "Rumah Honai?", "id": "Papua Pegunungan." },
  { "en": "Rumah Kaki Seribu?", "id": "Papua Barat." },
  { "en": "Rumah Jew?", "id": "Papua Barat Daya." },
  { "en": "Rumah Kariwari?", "id": "Papua." },
  { "en": "Rumah Emawa?", "id": "Papua Selatan." },
  { "en": "Rumah Koba?", "id": "Papua Tengah." },
  { "en": "Omo Sebua?", "id": "Sumatera Utara." },
  { "en": "Bagas Godang?", "id": "Sumatera Utara." },
  { "en": "Siwaluh Jabu?", "id": "Sumatera Utara." },
  { "en": "Rumah Ulu?", "id": "Sumatera Selatan." },
  { "en": "Imah Julang Ngapak?", "id": "Jawa Barat." },
  { "en": "Imah Togog Anjing?", "id": "Jawa Barat." },
  { "en": "Rumah Limasan?", "id": "Jawa Tengah." },
  { "en": "Rumah Panggang Pe?", "id": "Jawa Tengah." },
  { "en": "Rumah Adat Osing?", "id": "Jawa Timur." },
  { "en": "Bale Manten?", "id": "Bali." },
  { "en": "Bale Sekapat?", "id": "Bali." },
  { "en": "Mbaru Niang?", "id": "Nusa Tenggara Timur." },
  { "en": "Rumah Adat Lio?", "id": "Nusa Tenggara Timur." },
  { "en": "Rumah Radakng?", "id": "Kalimantan Barat." },
  { "en": "Rumah Balai Bini?", "id": "Kalimantan Selatan." },
  { "en": "Rumah Bola Soba?", "id": "Sulawesi Selatan." },
  { "en": "Balla Lompoa?", "id": "Sulawesi Selatan." },
  { "en": "Rumah Adat Mandar?", "id": "Sulawesi Barat." },
  { "en": "Rumah Adat Tolaki?", "id": "Sulawesi Tenggara." },
  { "en": "Rumah Adat Sangihe?", "id": "Sulawesi Utara." },
  { "en": "Rumah Adat Tanimbar?", "id": "Maluku." },
  { "en": "Rumah Adat Kei?", "id": "Maluku." },
  { "en": "Rumah Rumsram?", "id": "Papua Barat." },
  { "en": "Rumah Adat Asmat?", "id": "Papua Selatan." },
  { "en": "Rumah Melayu Atap Lontik?", "id": "Riau." },
  { "en": "Lamban Pesagi?", "id": "Lampung." },
  { "en": "Imah Badak Heuay?", "id": "Jawa Barat." },
  { "en": "Imah Jolopong?", "id": "Jawa Barat." },
  { "en": "Rumah Adat Sasak?", "id": "Nusa Tenggara Barat." },
  { "en": "Rumah Adat Sumba?", "id": "Nusa Tenggara Timur." },
  { "en": "Rumah Lanting?", "id": "Kalimantan Selatan." },
  { "en": "Rumah Adat Luwu?", "id": "Sulawesi Selatan." },
  { "en": "Rumah Adat Kaili?", "id": "Sulawesi Tengah." },
  { "en": "Rumah Adat Muna?", "id": "Sulawesi Tenggara." },
  { "en": "Rumah Hibualamo?", "id": "Maluku Utara." },
  { "en": "Rumah Adat Dani?", "id": "Papua Pegunungan." },
  { "en": "Lambe Balinu?", "id": "Riau." },
  { "en": "Bale Lumbung?", "id": "Nusa Tenggara Barat." },
  { "en": "Rumah Adat Rote?", "id": "Nusa Tenggara Timur." },
  { "en": "Rumah Adat Sabu?", "id": "Nusa Tenggara Timur." },
  { "en": "Rumah Paser?", "id": "Kalimantan Timur." },
  { "en": "Rumah Adat Dayak?", "id": "Kalimantan Timur." },
  { "en": "Saoraja?", "id": "Sulawesi Selatan." },
  { "en": "Rumah Adat Talaud?", "id": "Sulawesi Utara." },
  { "en": "Rumah Adat Kulawi?", "id": "Sulawesi Tengah." },
  { "en": "Laikas?", "id": "Sulawesi Tenggara." },
  { "en": "Rumah Adat Tobelo?", "id": "Maluku Utara." },
  { "en": "Rumah Adat Biak?", "id": "Papua." },
  { "en": "Gaharu?", "id": "Bengkulu." },
  { "en": "Sesat Agung?", "id": "Lampung." },
  { "en": "Omah Tikel?", "id": "DI Yogyakarta." },
  { "en": "Ume Kbubu?", "id": "Nusa Tenggara Timur." },
  { "en": "Sao Ria Tenda Bewa Moni?", "id": "Nusa Tenggara Timur." },
  { "en": "Bale Jajar?", "id": "Nusa Tenggara Barat." },
  { "en": "Rumah Gajah Baliku?", "id": "Kalimantan Selatan." },
  { "en": "Rumah Souraja?", "id": "Sulawesi Tengah." },
  { "en": "Lgkojei?", "id": "Maluku Utara." },
  { "en": "Hanoi?", "id": "Papua Pegunungan." },
  { "en": "Bantayo Poboide?", "id": "Gorontalo." },
  { "en": "Imah Capit Gunting?", "id": "Jawa Barat." },
  { "en": "Rumah Tanean Lanjhang?", "id": "Jawa Timur." },
  { "en": "Bale Bonder?", "id": "Nusa Tenggara Barat." },
  { "en": "Rumah Panyun?", "id": "Kalimantan Barat." },
  { "en": "Rumah Baluk?", "id": "Kalimantan Barat." },
  { "en": "Lamin Gajah Oling?", "id": "Kalimantan Timur." },
  { "en": "Rumah Titing?", "id": "Kalimantan Utara." },
  { "en": "Baruga?", "id": "Sulawesi Selatan." },
  { "en": "Lakka?", "id": "Sulawesi Selatan." },
  { "en": "Rumah Adat Tojo Una-Una?", "id": "Sulawesi Tengah." },
  { "en": "Rumah Adat Halmahera?", "id": "Maluku Utara." },
  { "en": "Rumah Wamesa?", "id": "Papua Barat." },
  { "en": "Rumah Lontiok?", "id": "Riau." },
  { "en": "Rumah Godang Kuantan Singingi?", "id": "Riau." },
  { "en": "Rumah Kajang Padati?", "id": "Jambi." },
  { "en": "Rumah Tuo?", "id": "Jambi." },
  { "en": "Rumah Baghi?", "id": "Sumatera Selatan." },
  { "en": "Gedung Meneng?", "id": "Lampung." },
  { "en": "Rumah Srotong?", "id": "Jawa Tengah." },
  { "en": "Bale Gede?", "id": "Bali." },
  { "en": "Lepo Gete?", "id": "Nusa Tenggara Timur." },
  { "en": "Uma Mbatangu?", "id": "Nusa Tenggara Timur." },
  { "en": "Rumah Bulungan?", "id": "Kalimantan Utara." },
  { "en": "Jabu Parsakitan?", "id": "Sumatera Utara." },
  { "en": "Sopo Godang?", "id": "Sumatera Utara." },
  { "en": "Rumah Adat Gayo?", "id": "Aceh." },
  { "en": "Rumah Adat Alas?", "id": "Aceh." },
  { "en": "Alang Sura?", "id": "Bali." },
  { "en": "Rumah Adat Ngada?", "id": "Nusa Tenggara Timur." },
  { "en": "Rumah Adat Larantuka?", "id": "Nusa Tenggara Timur." },
  { "en": "Rumah Palimasan?", "id": "Kalimantan Selatan." },
  { "en": "Rumah Balai Laki?", "id": "Kalimantan Selatan." },
  { "en": "Rumah Adat Mamasa?", "id": "Sulawesi Barat." },
  { "en": "Rumah Adat Banggai?", "id": "Sulawesi Tengah." },
  { "en": "Rumah Adat Morotai?", "id": "Maluku Utara." },
  { "en": "Rumah Adat Yali?", "id": "Papua Pegunungan." },
  { "en": "Rumah Adat Mimika?", "id": "Papua Tengah." },
  { "en": "Rumah Adat Waropen?", "id": "Papua." },
  { "en": "Rumah Pesagi?", "id": "Lampung." },
  { "en": "Rumah Panggung Dorong?", "id": "Jambi." },
  { "en": "Imah Kasepuhan?", "id": "Jawa Barat." },
  { "en": "Rumah Adat Madura?", "id": "Jawa Timur." },
  { "en": "Berugaq Sekepat?", "id": "Nusa Tenggara Barat." },
  { "en": "Sao Ata Mosa Lakitana?", "id": "Nusa Tenggara Timur." },
  { "en": "Rumah Adat Dayak Iban?", "id": "Kalimantan Barat." },
  { "en": "Rumah Banjar?", "id": "Kalimantan Selatan." },
  { "en": "Rumah Adat Berau?", "id": "Kalimantan Timur." },
  { "en": "Rumah Adat Tidung?", "id": "Kalimantan Utara." },
  { "en": "Rumah Adat Minahasa?", "id": "Sulawesi Utara." },
  { "en": "Rumah Adat Padoe?", "id": "Sulawesi Tengah." },
  { "en": "Bante?", "id": "Sulawesi Tenggara." },
  { "en": "Rumah Adat Pattae?", "id": "Sulawesi Barat." },
  { "en": "Rumah Adat Aru?", "id": "Maluku." },
  { "en": "Rumah Adat Seram?", "id": "Maluku." },
  { "en": "Rumah Adat Buru?", "id": "Maluku." },
  { "en": "Rumah Adat Tidore?", "id": "Maluku Utara." },
  { "en": "Mod Aki Aksa?", "id": "Papua Barat." },
  { "en": "Jabu Bolon?", "id": "Sumatera Utara." },
  { "en": "Rumah Adat Mentawai?", "id": "Sumatera Barat." },
  { "en": "Rumah Adat Kubu?", "id": "Jambi." },
  { "en": "Rumah Gedang?", "id": "Jambi." },
  { "en": "Rumah Lamban Balak?", "id": "Lampung." },
  { "en": "Imah Perahu Kumureb?", "id": "Jawa Barat." },
  { "en": "Rumah Kampung Naga?", "id": "Jawa Barat." },
  { "en": "Rumah Tajug?", "id": "Jawa Tengah." },
  { "en": "Rumah Adat Tengger?", "id": "Jawa Timur." },
  { "en": "Pura?", "id": "Bali." },
  { "en": "Istana Sultan Sumbawa?", "id": "Nusa Tenggara Barat." },
  { "en": "Rumah Adat Bima?", "id": "Nusa Tenggara Barat." },
  { "en": "Rumah Adat Alor?", "id": "Nusa Tenggara Timur." },
  { "en": "Rumah Adat Dayak Kanayatn?", "id": "Kalimantan Barat." },
  { "en": "Rumah Adat Kutai?", "id": "Kalimantan Timur." },
  { "en": "Rumah Adat Bolaang Mongondow?", "id": "Sulawesi Utara." },
  { "en": "Rumah Adat Saluan?", "id": "Sulawesi Tengah." },
  { "en": "Rumah Adat Wakatobi?", "id": "Sulawesi Tenggara." },
  { "en": "Rumah Adat Kajang?", "id": "Sulawesi Selatan." },
  { "en": "Rumah Adat Tobelo?", "id": "Maluku Utara." },
  { "en": "Rumah Adat Raja Ampat?", "id": "Papua Barat Daya." },
  { "en": "Rumah Adat Fakfak?", "id": "Papua Barat." },
  { "en": "Omo Hada?", "id": "Sumatera Utara." },
  { "en": "Rumah Adat Simalungun?", "id": "Sumatera Utara." },
  { "en": "Rumah Adat Angkola?", "id": "Sumatera Utara." },
  { "en": "Rumah Ampek Baanjuang?", "id": "Riau." },
  { "en": "Bale Tiang Sanga?", "id": "Nusa Tenggara Barat." },
  { "en": "Rumah Adat Ende?", "id": "Nusa Tenggara Timur." },
  { "en": "Huma Gantung?", "id": "Kalimantan Tengah." },
  { "en": "Rumah Cahu?", "id": "Kalimantan Tengah." },
  { "en": "Lego-lego?", "id": "Nusa Tenggara Timur." },
  { "en": "Rumah Adat Sula?", "id": "Maluku Utara." },
  { "en": "Uma Lengge?", "id": "Nusa Tenggara Barat." },
  { "en": "Bale?", "id": "Nusa Tenggara Barat." },
  { "en": "Rumah Adat Kaimana?", "id": "Papua Barat." },
  { "en": "Sopo?", "id": "Sumatera Utara." },
  { "en": "Uma?", "id": "Sumatera Barat." },
  { "en": "Lumbung?", "id": "Jawa Barat." },
  { "en": "Rumah Gebyok?", "id": "Jawa Tengah." },
  { "en": "Lepo Tau?", "id": "Kalimantan Timur." },
  { "en": "Rumah Korowai?", "id": "Papua Selatan." },
  { "en": "Lopo?", "id": "Nusa Tenggara Timur." },
  { "en": "Uma Lulik?", "id": "Nusa Tenggara Timur." },
  { "en": "Bale Rung?", "id": "Nusa Tenggara Barat." },
  { "en": "Rumah Adat Mekongga?", "id": "Sulawesi Tenggara." },
  { "en": "Rumah Adat Mori?", "id": "Sulawesi Tengah." },
  { "en": "Rumah Adat Bungku?", "id": "Sulawesi Tengah." },
  { "en": "Rumah Adat Ternate?", "id": "Maluku Utara." },
  { "en": "Rumah Adat Banda?", "id": "Maluku." },
  { "en": "Rumah Tatak?", "id": "Bengkulu." },
  { "en": "Rumah Pucuk?", "id": "Lampung." },
  { "en": "Rumah Anjuang?", "id": "Riau." },
  { "en": "Rumah Sitinggil?", "id": "Banten." },
  { "en": "Rumah Banoa?", "id": "Kalimantan Barat." },
  { "en": "Panting?", "id": "Kalimantan Tengah." },
  { "en": "Pendopo?", "id": "Jawa Tengah." },
  { "en": "Alang?", "id": "Sulawesi Selatan." },
  { "en": "Uma Kelada?", "id": "Nusa Tenggara Timur." },
  { "en": "Rumah Adat Pakpak?", "id": "Sumatera Utara." },
  { "en": "Omo Hada?", "id": "Sumatera Utara." },
  { "en": "Jerro?", "id": "Sumatera Barat." },
  { "en": "Rumah Adat Kerinci?", "id": "Jambi." },
  { "en": "Rumah Pasemah?", "id": "Sumatera Selatan." },
  { "en": "Rumah Melayu Limas?", "id": "Kepulauan Riau." },
  { "en": "Imah Kokol?", "id": "Jawa Barat." },
  { "en": "Rumah Adat Ciptagelar?", "id": "Jawa Barat." },
  { "en": "Rumah Adat Banyuwangi?", "id": "Jawa Timur." },
  { "en": "Meru?", "id": "Bali." },
  { "en": "Alang Babale?", "id": "Nusa Tenggara Barat." },
  { "en": "Rumah Adat Adonara?", "id": "Nusa Tenggara Timur." },
  { "en": "Rumah Adat Lembata?", "id": "Nusa Tenggara Timur." },
  { "en": "Rumah Adat Ot Danum?", "id": "Kalimantan Tengah." },
  { "en": "Sandung?", "id": "Kalimantan Tengah." },
  { "en": "Rumah Adat Benuaq?", "id": "Kalimantan Timur." },
  { "en": "Rumah Adat Paser?", "id": "Kalimantan Timur." },
  { "en": "Rumah Adat Tidung Ulun Pagun?", "id": "Kalimantan Utara." },
  { "en": "Lawang Seketeng?", "id": "DI Yogyakarta." },
  { "en": "Rumah Bangsawan Banjar?", "id": "Kalimantan Selatan." },
  { "en": "Rumah Tadah Alas?", "id": "Kalimantan Selatan." },
  { "en": "Banua?", "id": "Sulawesi Selatan." },
  { "en": "Rumah Adat Konjo?", "id": "Sulawesi Selatan." },
  { "en": "Rumah Adat Pamona?", "id": "Sulawesi Tengah." },
  { "en": "Rumah Adat Balantak?", "id": "Sulawesi Tengah." },
  { "en": "Rumah Adat Wolio?", "id": "Sulawesi Tenggara." },
  { "en": "Rumah Adat Topoiyo?", "id": "Sulawesi Barat." },
  { "en": "Rumah Adat Bacan?", "id": "Maluku Utara." },
  { "en": "Rumah Adat Babar?", "id": "Maluku." },
  { "en": "Rumah Adat Kamoro?", "id": "Papua Tengah." },
  { "en": "Rumah Adat Marind Anim?", "id": "Papua Selatan." },
  { "en": "Rumah Adat Tehit?", "id": "Papua Barat Daya." },
  { "en": "Rumah Adat Sentani?", "id": "Papua." },
  { "en": "Sopo Jabu?", "id": "Sumatera Utara." },
  { "en": "Rumah Gonjong?", "id": "Sumatera Barat." },
  { "en": "Rumah Melayu Lipat Kajang?", "id": "Riau." },
  { "en": "Rumah Panggung Banten?", "id": "Banten." },
  { "en": "Pringgitan?", "id": "Jawa Tengah." },
  { "en": "Dalem?", "id": "Jawa Tengah." },
  { "en": "Bale Dangin?", "id": "Bali." },
  { "en": "Bale Delod?", "id": "Bali." },
  { "en": "Lare-lare?", "id": "Nusa Tenggara Barat." },
  { "en": "Rumah Adat Riung?", "id": "Nusa Tenggara Timur." },
  { "en": "Rumah Adat Ma'anyan?", "id": "Kalimantan Tengah." },
  { "en": "Lawang?", "id": "Kalimantan Tengah." },
  { "en": "Bara?", "id": "Kalimantan Timur." },
  { "en": "Rumah Adat Duri?", "id": "Sulawesi Selatan." },
  { "en": "Rumah Adat Enrekang?", "id": "Sulawesi Selatan." },
  { "en": "Rumah Adat Pitu Ulunna Salu?", "id": "Sulawesi Barat." },
  { "en": "Rumah Adat Kalumpang?", "id": "Sulawesi Barat." },
  { "en": "Rumah Adat Galela?", "id": "Maluku Utara." },
  { "en": "Rumah Adat Damar?", "id": "Maluku." },
  { "en": "Rumah Adat Mooi?", "id": "Papua." },
  { "en": "Rumah Adat Nabire?", "id": "Papua Tengah." },
  { "en": "Rumah Adat Paniai?", "id": "Papua Tengah." },
  { "en": "Rumah Adat Jayawijaya?", "id": "Papua Pegunungan." },
  { "en": "Rumah Adat Pegunungan Bintang?", "id": "Papua Pegunungan." },
  { "en": "Rumah Adat Boven Digoel?", "id": "Papua Selatan." },
  { "en": "Rumah Adat Sorong?", "id": "Papua Barat Daya." },
  { "en": "Rumah Adat Manokwari?", "id": "Papua Barat." },
  { "en": "Rumah Singgah?", "id": "Bangka Belitung." },
  { "en": "Rumah Panggung Palembang?", "id": "Sumatera Selatan." },
  { "en": "Rumah Adat Rejang?", "id": "Bengkulu." },
  { "en": "Rumah Adat Kaur?", "id": "Bengkulu." },
  { "en": "Nuwo Balak?", "id": "Lampung." },
  { "en": "Imah Adat Cikondang?", "id": "Jawa Barat." },
  { "en": "Rumah Pacul Gowang?", "id": "Jawa Tengah." },
  { "en": "Kraton Kasepuhan?", "id": "Jawa Barat." },
  { "en": "Kraton Kanoman?", "id": "Jawa Barat." },
  { "en": "Pura Mangkunegaran?", "id": "Jawa Tengah." },
  { "en": "Pura Pakualaman?", "id": "DI Yogyakarta." },
  { "en": "Bale Pawon?", "id": "Bali." },
  { "en": "Paon?", "id": "Bali." },
  { "en": "Berugaq?", "id": "Nusa Tenggara Barat." },
  { "en": "Lumbung?", "id": "Nusa Tenggara Barat." },
  { "en": "Istana Dalam Loka?", "id": "Nusa Tenggara Barat." },
  { "en": "Rumah Adat Flores?", "id": "Nusa Tenggara Timur." },
  { "en": "Istana Maimun?", "id": "Sumatera Utara." },
  { "en": "Istana Siak Sri Indrapura?", "id": "Riau." },
  { "en": "Istana Pagaruyung?", "id": "Sumatera Barat." },
  { "en": "Rangkiang?", "id": "Sumatera Barat." },
  { "en": "Balai Kerapatan Tinggi?", "id": "Riau." },
  { "en": "Kraton Kacirebonan?", "id": "Jawa Barat." },
  { "en": "Dalem Ageng?", "id": "Jawa Tengah." },
  { "en": "Bangsal Pagelaran?", "id": "DI Yogyakarta." },
  { "en": "Siti Hinggil?", "id": "DI Yogyakarta." },
  { "en": "Rumah Adat Penglipuran?", "id": "Bali." },
  { "en": "Puri Agung Ubud?", "id": "Bali." },
  { "en": "Asi Mbojo?", "id": "Nusa Tenggara Barat." },
  { "en": "Rumah Adat Bena?", "id": "Nusa Tenggara Timur." },
  { "en": "Rumah Adat Wologai?", "id": "Nusa Tenggara Timur." },
  { "en": "Istana Kadriah?", "id": "Kalimantan Barat." },
  { "en": "Kraton Kutai Kartanegara?", "id": "Kalimantan Timur." },
  { "en": "Lamin Mancong?", "id": "Kalimantan Timur." },
  { "en": "Istana Gunung Tabur?", "id": "Kalimantan Timur." },
  { "en": "Istana Sambaliung?", "id": "Kalimantan Timur." },
  { "en": "Bola Ake?", "id": "Sulawesi Selatan." },
  { "en": "Istana Wolio?", "id": "Sulawesi Tenggara." },
  { "en": "Kadato?", "id": "Maluku Utara." },
  { "en": "Rumah Adat Tanimbar Evav?", "id": "Maluku." },
  { "en": "Onai?", "id": "Papua Pegunungan." },
  { "en": "Rumah Adat Angkola?", "id": "Sumatera Utara." },
  { "en": "Jabu Ere?", "id": "Sumatera Utara." },
  { "en": "Rumah Adat Pasir?", "id": "Riau." },
  { "en": "Rumah Tatahan?", "id": "Sumatera Selatan." },
  { "en": "Rumah Adat Serawai?", "id": "Bengkulu." },
  { "en": "Bale Adat?", "id": "Banten." },
  { "en": "Omah Joglo?", "id": "Jawa Timur." },
  { "en": "Puri Agung Karangasem?", "id": "Bali." },
  { "en": "Puri Agung Klungkung?", "id": "Bali." },
  { "en": "Istana Air Taman Sari?", "id": "DI Yogyakarta." },
  { "en": "Rumah Adat Tenganan?", "id": "Bali." },
  { "en": "Bale Delod?", "id": "Nusa Tenggara Barat." },
  { "en": "Rumah Adat Manggarai?", "id": "Nusa Tenggara Timur." },
  { "en": "Lisan?", "id": "Nusa Tenggara Timur." },
  { "en": "Betang Tumbang Anoi?", "id": "Kalimantan Tengah." },
  { "en": "Istana Alwatzikoebillah?", "id": "Kalimantan Barat." },
  { "en": "Rumah Adat Tanjung?", "id": "Kalimantan Selatan." },
  { "en": "Rumah Gajah Manyusu?", "id": "Kalimantan Selatan." },
  { "en": "Lamin Sembayån?", "id": "Kalimantan Timur." },
  { "en": "Sapo?", "id": "Sulawesi Selatan." },
  { "en": "Banua Tada?", "id": "Sulawesi Tenggara." },
  { "en": "Kadaton?", "id": "Sulawesi Tenggara." },
  { "en": "Rumah Adat Wawonii?", "id": "Sulawesi Tenggara." },
  { "en": "Rumah Adat Sigi?", "id": "Sulawesi Tengah." },
  { "en": "Istana Kesultanan Tidore?", "id": "Maluku Utara." },
  { "en": "Rumah Adat Leti?", "id": "Maluku." },
  { "en": "Rumah Adat Kisar?", "id": "Maluku." },
  { "en": "Honai Laki-laki?", "id": "Papua Pegunungan." },
  { "en": "Ebei?", "id": "Papua Pegunungan." },
  { "en": "Wamai?", "id": "Papua Pegunungan." },
  { "en": "Sopo Gorat?", "id": "Sumatera Utara." },
  { "en": "Balai Tiang Bertingkat?", "id": "Jambi." },
  { "en": "Rumah Caram?", "id": "Bengkulu." },
  { "en": "Sesat?", "id": "Lampung." },
  { "en": "Pasisir?", "id": "Banten." },
  { "en": "Dalem Pangapit?", "id": "Jawa Barat." },
  { "en": "Gadri?", "id": "DI Yogyakarta." },
  { "en": "Rumah Adat Blambangan?", "id": "Jawa Timur." },
  { "en": "Rumah Sakenem?", "id": "Bali." },
  { "en": "Bale Pelik?", "id": "Nusa Tenggara Barat." },
  { "en": "Rumah Adat Lamaholot?", "id": "Nusa Tenggara Timur." },
  { "en": "Rumah Adat Lawa?", "id": "Kalimantan Tengah." },
  { "en": "Rumah Adat Taboyan?", "id": "Kalimantan Tengah." },
  { "en": "Betang Muara Mea?", "id": "Kalimantan Timur." },
  { "en": "Rumah Adat Bulungan?", "id": "Kalimantan Utara." },
  { "en": "Bala Reang?", "id": "Sulawesi Utara." },
  { "en": "Rumah Rakit Palembang?", "id": "Sumatera Selatan." },
  { "en": "Rumah Adat Ogan?", "id": "Sumatera Selatan." },
  { "en": "Rumah Adat Komering?", "id": "Sumatera Selatan." },
  { "en": "Rumah Adat Semendo?", "id": "Sumatera Selatan." },
  { "en": "Rumah Adat Ranau?", "id": "Lampung." },
  { "en": "Nuwo Lunik?", "id": "Lampung." },
  { "en": "Kuta?", "id": "Sumatera Utara." },
  { "en": "Rumah Adat Singkil?", "id": "Aceh." },
  { "en": "Rumah Adat Kluet?", "id": "Aceh." },
  { "en": "Balai?", "id": "Sumatera Barat." },
  { "en": "Surau?", "id": "Sumatera Barat." },
  { "en": "Rumah Adat Talang Mamak?", "id": "Riau." },
  { "en": "Rumah Adat Sakai?", "id": "Riau." },
  { "en": "Jineng?", "id": "Bali." },
  { "en": "Rumah Adat Sumbawa?", "id": "Nusa Tenggara Barat." },
  { "en": "Lumbung Padi?", "id": "Jawa Barat." },
  { "en": "Kandang?", "id": "Jawa Tengah." },
  { "en": "Cota?", "id": "Kalimantan Selatan." },
  { "en": "Rumah Terapung?", "id": "Kalimantan Selatan." },
  { "en": "Leuit?", "id": "Banten." },
  { "en": "Sulah Nyanda?", "id": "Banten." },
  { "en": "Pura Dalem?", "id": "Bali." },
  { "en": "Pura Puseh?", "id": "Bali." },
  { "en": "Rumah Adat Tulehu?", "id": "Maluku." },
  { "en": "Rumah Samin?", "id": "Jawa Tengah." },
  { "en": "Bale Wantilan?", "id": "Bali." },
  { "en": "Kori Agung?", "id": "Bali." },
  { "en": "Rumah Larik?", "id": "Riau." },
  { "en": "Anjung?", "id": "Kalimantan Selatan." },
  { "en": "Rumah Kalosuan?", "id": "Sulawesi Selatan." },
  { "en": "Uma Bokulu?", "id": "Nusa Tenggara Timur." },
  { "en": "Rumah Duan Lolat?", "id": "Maluku." },
  { "en": "Rumah Adat Topejawa?", "id": "Sulawesi Tengah." },
  { "en": "Gandhok?", "id": "Jawa Tengah." },
  { "en": "Senthong?", "id": "Jawa Tengah." },
  { "en": "Rumah Adat Semende?", "id": "Sumatera Selatan." },
  { "en": "Astana?", "id": "Riau." },
  { "en": "Rumah Bumbung Panjang?", "id": "Bengkulu." },
  { "en": "Gedong?", "id": "Bali." },
  { "en": "Pelinggih?", "id": "Bali." },
  { "en": "Balai Panjang?", "id": "Kalimantan Barat." },
  { "en": "Pasanggrahan?", "id": "Jawa Barat." },
  { "en": "Rumah Adat Wajo?", "id": "Sulawesi Selatan." },
  { "en": "Rumah Adat Soppeng?", "id": "Sulawesi Selatan." },
  { "en": "Rumah Adat Bone?", "id": "Sulawesi Selatan." },
  { "en": "Rumah Adat Gowa?", "id": "Sulawesi Selatan." },
  { "en": "Rumah Adat Sinjai?", "id": "Sulawesi Selatan." },
  { "en": "Rumah Adat Bantaeng?", "id": "Sulawesi Selatan." },
  { "en": "Rumah Adat Jeneponto?", "id": "Sulawesi Selatan." },
  { "en": "Rumah Adat Palu?", "id": "Sulawesi Tengah." },
  { "en": "Rumah Adat Donggala?", "id": "Sulawesi Tengah." },
  { "en": "Rumah Adat Toar-Lumumut?", "id": "Sulawesi Utara." },
  { "en": "Rumah Adat Dompu?", "id": "Nusa Tenggara Barat." },
  { "en": "Rumah Adat Sikka?", "id": "Nusa Tenggara Timur." },
  { "en": "Rumah Adat Belu?", "id": "Nusa Tenggara Timur." },
  { "en": "Rumah Adat Malaka?", "id": "Nusa Tenggara Timur." },
  { "en": "Rumah Adat Melayu Pontianak?", "id": "Kalimantan Barat." },
  { "en": "Rumah Adat Kapuas Hulu?", "id": "Kalimantan Barat." },
  { "en": "Rumah Adat Kotawaringin?", "id": "Kalimantan Tengah." },
  { "en": "Rumah Adat Barito?", "id": "Kalimantan Tengah." },
  { "en": "Rumah Adat Kutai Barat?", "id": "Kalimantan Timur." },
  { "en": "Keraton Sumenep?", "id": "Jawa Timur." },
  { "en": "Sopo Batak?", "id": "Sumatera Utara." },
  { "en": "Balai Adat Melayu?", "id": "Sumatera Utara." },
  { "en": "Surau Gadang?", "id": "Sumatera Barat." },
  { "en": "Rumah Adat Petalangan?", "id": "Riau." },
  { "en": "Rumah Adat Mukomuko?", "id": "Bengkulu." },
  { "en": "Nuwo Olagan?", "id": "Lampung." },
  { "en": "Rumah Adat Cireundeu?", "id": "Jawa Barat." },
  { "en": "Pura Ulun Danu?", "id": "Bali." },
  { "en": "Puri Pemecutan?", "id": "Bali." },
  { "en": "Uma Dalam?", "id": "Nusa Tenggara Timur." },
  { "en": "Betang Damang Batu?", "id": "Kalimantan Tengah." },
  { "en": "Rumah Adat Hulu Sungai?", "id": "Kalimantan Selatan." },
  { "en": "Rumah Adat Tidung?", "id": "Kalimantan Utara." },
  { "en": "Somba Opu?", "id": "Sulawesi Selatan." },
  { "en": "Luwu?", "id": "Sulawesi Selatan." },
  { "en": "Rumah Adat Kolaka?", "id": "Sulawesi Tenggara." },
  { "en": "Rumah Adat Polewali Mandar?", "id": "Sulawesi Barat." },
  { "en": "Rumah Adat Ambon?", "id": "Maluku." },
  { "en": "Rumah Adat Jailolo?", "id": "Maluku Utara." },
  { "en": "Rumah Adat Teluk Bintuni?", "id": "Papua Barat." },
  { "en": "Rumah Adat Sarmi?", "id": "Papua." },
  { "en": "Rumah Adat Mappi?", "id": "Papua Selatan." },
  { "en": "Rumah Adat Deiyai?", "id": "Papua Tengah." },
  { "en": "Rumah Adat Tolikara?", "id": "Papua Pegunungan." },
  { "en": "Rumah Adat Maybrat?", "id": "Papua Barat Daya." },
  { "en": "Gapura Wringin Lawang?", "id": "Jawa Timur." },
  { "en": "Candi Bajang Ratu?", "id": "Jawa Timur." },
  { "en": "Rumah Adat Pubian?", "id": "Lampung." },
  { "en": "Rumah Panggung Kesultanan Banten?", "id": "Banten." },
  { "en": "Lumbung Goak?", "id": "Banten." },
  { "en": "Bale Agung?", "id": "Bali." },
  { "en": "Paibon?", "id": "Bali." },
  { "en": "Rumah Adat Lombok?", "id": "Nusa Tenggara Barat." },
  { "en": "Rumah Adat Timor?", "id": "Nusa Tenggara Timur." },
  { "en": "Rumah Panjang Dayak?", "id": "Kalimantan Barat." },
  { "en": "Rumah Adat Banjar Kuala?", "id": "Kalimantan Selatan." },
  { "en": "Rumah Adat Dayak Tunjung?", "id": "Kalimantan Timur." },
  { "en": "Rumah Adat Punan?", "id": "Kalimantan Utara." },
  { "en": "Rumah Adat Toraja Sa'dan?", "id": "Sulawesi Selatan." },
  { "en": "Rumah Adat Buton Wolio?", "id": "Sulawesi Tenggara." },
  { "en": "Rumah Adat Gorontalo Utara?", "id": "Gorontalo." },
  { "en": "Rumah Adat Sula?", "id": "Maluku Utara." },
  { "en": "Rumah Adat Maluku Barat Daya?", "id": "Maluku." },
  { "en": "Rumah Honai Yali?", "id": "Papua Pegunungan." },
  { "en": "Rumah Adat Arfak?", "id": "Papua Barat." },
  { "en": "Rumah Adat Merauke?", "id": "Papua Selatan." },
  { "en": "Rumah Adat Dogiyai?", "id": "Papua Tengah." },
  { "en": "Rumah Adat Lanny Jaya?", "id": "Papua Pegunungan." },
  { "en": "Rumah Adat Tambrauw?", "id": "Papua Barat Daya." },
  { "en": "Keraton Gebang?", "id": "Jawa Barat." },
  { "en": "Rumah Kapitan?", "id": "Sumatera Selatan." },
  { "en": "Jabu Nagodang?", "id": "Sumatera Utara." },
  { "en": "Rumah Adat Lampung Barat?", "id": "Lampung." },
  { "en": "Puri Agung Jro Kuta?", "id": "Bali." },
  { "en": "Betang Pasir Panjang?", "id": "Kalimantan Tengah." },
  { "en": "Banua Niha?", "id": "Sumatera Utara." },



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
