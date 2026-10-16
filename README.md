<!DOCTYPE html>
<html lang="id">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Pythagoras Rescue Adventure</title>
    <!-- Tailwind CSS CDN -->
    <script src="https://cdn.tailwindcss.com"></script>
    <style>
        @import url('https://fonts.googleapis.com/css2?family=Inter:wght@400;500;600;700;800&display=swap');
        body {
            font-family: 'Inter', sans-serif;
            background-color: #f7f7f2;
            color: #2d3748;
            touch-action: manipulation;
        }
        .game-card {
            background: #ffffff;
            border: 2px solid #e2e8f0;
            border-radius: 1rem;
            box-shadow: 0 4px 6px -1px rgba(0, 0, 0, 0.05), 0 2px 4px -1px rgba(0, 0, 0, 0.03);
        }
        .btn-primary {
            background-color: #f97316;
            color: #ffffff;
            font-weight: 600;
            border-radius: 0.75rem;
            padding: 0.75rem 1.5rem;
            transition: all 0.2s ease;
            box-shadow: 0 4px 14px rgba(249, 115, 22, 0.3);
        }
        .btn-primary:hover {
            background-color: #ea580c;
            transform: translateY(-1px);
        }
        .btn-secondary {
            background-color: #15803d;
            color: #ffffff;
            font-weight: 600;
            border-radius: 0.75rem;
            padding: 0.75rem 1.5rem;
            transition: all 0.2s ease;
            box-shadow: 0 4px 14px rgba(21, 128, 61, 0.3);
        }
        .btn-secondary:hover {
            background-color: #166534;
            transform: translateY(-1px);
        }
        .draggable-square {
            cursor: grab;
            user-select: none;
            touch-action: none;
            transition: transform 0.1s ease;
        }
        .draggable-square:active {
            cursor: grabbing;
        }
    </style>
</head>
<body class="min-h-screen flex flex-col justify-between" onload="initApp()">

    <!-- Header -->
    <header class="bg-[#15803d] text-white shadow-md sticky top-0 z-50">
        <div class="max-w-4xl mx-auto px-4 py-3 flex flex-wrap justify-between items-center">
            <div class="flex items-center space-x-3">
                <div class="bg-white text-[#15803d] p-2 rounded-xl font-extrabold text-xl shadow-inner">
                    📐
                </div>
                <div>
                    <h1 class="font-extrabold text-base sm:text-lg tracking-wide">PYTHAGORAS RESCUE</h1>
                    <p class="text-xs text-emerald-100 font-medium">Petualangan Matematika SMP</p>
                </div>
            </div>
            <div class="flex items-center space-x-2">
                <div class="bg-[#166534] px-3 py-1.5 rounded-lg text-xs font-semibold flex items-center space-x-1 shadow-sm">
                    <span>🏆 Skor:</span>
                    <span id="header-total-score" class="text-orange-300 font-bold text-sm">0</span>
                    <span class="text-emerald-200">/ 950</span>
                </div>
                <button onclick="toggleAiChat()" class="bg-amber-500 hover:bg-amber-600 text-white text-xs px-3 py-1.5 rounded-lg font-semibold transition shadow-sm flex items-center space-x-1">
                    <span>🤖 Asisten AI</span>
                </button>
                <button onclick="confirmReset()" class="bg-red-700 hover:bg-red-800 text-white text-xs px-3 py-1.5 rounded-lg font-semibold transition shadow-sm">
                    Ulangi
                </button>
            </div>
        </div>
    </header>

    <!-- Main Container -->
    <main class="max-w-3xl w-full mx-auto p-4 sm:p-6 my-auto flex-grow flex flex-col justify-center">

        <!-- 1. WELCOME SCREEN -->
        <div id="screen-welcome" class="game-card p-6 sm:p-10 text-center space-y-6">
            <div class="max-w-md mx-auto">
                <div class="w-24 h-24 bg-emerald-100 text-[#15803d] rounded-full flex items-center justify-center mx-auto text-4xl shadow-inner mb-4">
                    ⛰️🌲
                </div>
                <h2 class="text-2xl sm:text-3xl font-extrabold text-[#15803d]">Pythagoras Rescue Adventure</h2>
                <p class="text-gray-600 text-sm sm:text-base mt-2">
                    Bantu tim penyelamat menyelesaikan misi di berbagai medan dengan menguasai Teorema Pythagoras secara berurutan tanpa jalan pintas!
                </p>
            </div>

            <div class="bg-[#f7f7f2] p-4 rounded-xl max-w-md mx-auto text-left text-xs sm:text-sm space-y-2 text-gray-700 border border-emerald-100">
                <p class="font-bold text-[#15803d]">📌 Ketentuan Petualangan:</p>
                <ul class="list-disc list-inside space-y-1 text-gray-600">
                    <li>Alur wajib berurutan: Level 1 ➔ Asesmen ➔ Level 2 ➔ Asesmen ➔ dst.</li>
                    <li>Tidak ada lompatan level; setiap tahap harus diselesaikan secara tuntas.</li>
                    <li>Dilengkapi ilustrasi gambar di setiap level dan Asisten AI konsep mandiri.</li>
                </ul>
            </div>

            <div class="pt-2">
                <button onclick="startAdventure()" class="btn-primary text-base sm:text-lg px-8 py-3 shadow-lg">
                    🚀 Mulai Petualangan
                </button>
            </div>
        </div>

        <!-- 2. LEVEL 1: TRIANGLE EXPLORER -->
        <div id="screen-level1" class="game-card p-5 sm:p-8 space-y-6 hidden">
            <div class="flex justify-between items-center border-b pb-3">
                <div>
                    <span class="text-xs font-bold text-[#15803d] uppercase tracking-wider">Level 1 dari 4</span>
                    <h2 class="text-xl sm:text-2xl font-extrabold text-gray-800">Triangle Explorer</h2>
                </div>
                <span class="text-xs bg-emerald-100 text-emerald-800 px-3 py-1 rounded-full font-semibold">Maks. 100 Poin</span>
            </div>

            <p class="text-xs sm:text-sm text-gray-600">
                Eksplorasi bentuk geometri. Pilih bentuk segitiga yang merupakan <b>segitiga siku-siku</b> di antara pilihan berikut, lalu tekan konfirmasi.
            </p>

            <div class="grid grid-cols-1 sm:grid-cols-3 gap-4 py-2">
                <div onclick="selectLevel1Opt('A')" id="l1-opt-A" class="border-2 border-gray-200 bg-gray-50 rounded-xl p-4 text-center cursor-pointer hover:border-[#15803d] transition">
                    <svg viewBox="0 0 100 100" class="w-24 h-24 mx-auto fill-emerald-50 stroke-[#15803d] stroke-2">
                        <polygon points="15,85 85,85 15,15" />
                        <polyline points="15,70 30,70 30,85" fill="none" stroke="#15803d" stroke-width="1.5" />
                    </svg>
                    <span class="font-bold text-sm text-gray-700 mt-2 block">Segitiga A</span>
                </div>
                <div onclick="selectLevel1Opt('B')" id="l1-opt-B" class="border-2 border-gray-200 bg-gray-50 rounded-xl p-4 text-center cursor-pointer hover:border-[#15803d] transition">
                    <svg viewBox="0 0 100 100" class="w-24 h-24 mx-auto fill-emerald-50 stroke-[#15803d] stroke-2">
                        <polygon points="10,85 90,85 45,20" />
                    </svg>
                    <span class="font-bold text-sm text-gray-700 mt-2 block">Segitiga B</span>
                </div>
                <div onclick="selectLevel1Opt('C')" id="l1-opt-C" class="border-2 border-gray-200 bg-gray-50 rounded-xl p-4 text-center cursor-pointer hover:border-[#15803d] transition">
                    <svg viewBox="0 0 100 100" class="w-24 h-24 mx-auto fill-emerald-50 stroke-[#15803d] stroke-2">
                        <polygon points="20,85 80,85 50,15" />
                    </svg>
                    <span class="font-bold text-sm text-gray-700 mt-2 block">Segitiga C</span>
                </div>
            </div>

            <div id="feedback-l1" class="hidden p-3 rounded-lg text-sm font-medium text-center"></div>

            <div class="flex justify-end pt-2">
                <button onclick="submitLevel1()" class="btn-secondary">Konfirmasi Pilihan</button>
            </div>
        </div>

        <!-- 3. ASSESSMENT LEVEL 1 -->
        <div id="screen-assessment-1" class="game-card p-6 sm:p-8 space-y-6 hidden text-center">
            <div class="w-16 h-16 bg-emerald-100 text-[#15803d] rounded-full flex items-center justify-center mx-auto text-2xl shadow-inner">
                ⭐
            </div>
            <div>
                <span class="text-xs font-bold text-[#15803d] uppercase tracking-wider">Penilaian Level 1</span>
                <h2 class="text-2xl font-extrabold text-gray-800">Triangle Explorer Selesai</h2>
            </div>

            <div class="bg-emerald-50 border border-emerald-200 p-6 rounded-2xl max-w-md mx-auto space-y-3 shadow-inner text-left">
                <div class="flex justify-between text-sm">
                    <span class="font-medium text-gray-600">Status Level:</span>
                    <span class="font-bold text-emerald-700">Level Selesai</span>
                </div>
                <div class="flex justify-between text-sm">
                    <span class="font-medium text-gray-600">Skor Diperoleh:</span>
                    <span id="score-val-1" class="font-bold text-orange-600 text-base">0 / 100</span>
                </div>
                <div class="flex justify-between text-sm">
                    <span class="font-medium text-gray-600">Persentase Pencapaian:</span>
                    <span id="percent-val-1" class="font-bold text-gray-800">0%</span>
                </div>
                <div class="flex justify-between text-sm">
                    <span class="font-medium text-gray-600">Evaluasi Performa:</span>
                    <span id="perf-val-1" class="font-bold text-[#15803d]">-</span>
                </div>
            </div>

            <div class="pt-2">
                <button onclick="startLevel2()" class="btn-primary text-base px-8 py-3 shadow-lg">
                    Lanjut ke Level 2 ➔
                </button>
            </div>
        </div>

        <!-- 4. LEVEL 2: DISCOVER THE RULE -->
        <div id="screen-level2" class="game-card p-5 sm:p-8 space-y-6 hidden">
            <div class="flex justify-between items-center border-b pb-3">
                <div>
                    <span class="text-xs font-bold text-[#15803d] uppercase tracking-wider">Level 2 dari 4</span>
                    <h2 class="text-xl sm:text-2xl font-extrabold text-gray-800">Discover the Rule: Geometric Proof of Squares</h2>
                </div>
                <span class="text-xs bg-emerald-100 text-emerald-800 px-3 py-1 rounded-full font-semibold">Maks. 150 Poin</span>
            </div>

            <p class="text-xs sm:text-sm text-gray-600">
                <b>Tahap A:</b> Pasang ketiga persegi pada sisi segitiga siku-siku (sisi tegak <b>a = 3</b>, sisi tegak <b>b = 4</b>, dan sisi miring <b>c = 5</b>). Ketuk persegi di bawah lalu ketuk area zona penempatannya.
            </p>

            <!-- Exact Geometric Proof Illustration -->
            <div class="bg-[#f7f7f2] border border-emerald-200 p-4 rounded-xl flex flex-col items-center justify-center space-y-3">
                <div class="text-center font-bold text-xs sm:text-sm text-indigo-900 bg-white px-3 py-1.5 rounded-lg border shadow-sm w-full">
                    Geometric Proof of the Pythagorean Theorem<br>
                    <span class="text-emerald-700">a² + b² = c² &rarr; 9 + 16 = 25</span>
                </div>

                <div class="relative w-full max-w-sm h-80 bg-white border border-gray-200 rounded-xl shadow-inner flex items-center justify-center p-2">
                    <svg viewBox="0 0 340 300" class="w-full h-full">
                        <!-- Central Right Triangle (Orange) -->
                        <polygon points="120,120 120,220 220,220" fill="#fed7aa" stroke="#c2410c" stroke-width="2.5" />
                        <polyline points="120,200 140,200 140,220" fill="none" stroke="#c2410c" stroke-width="2" />
                        
                        <!-- Triangle side labels -->
                        <text x="160" y="155" font-size="12" font-weight="bold" fill="#2563eb">c = 5 (Sisi Miring)</text>
                        <text x="95" y="175" font-size="12" font-weight="bold" fill="#16a34a">b = 4</text>
                        <text x="165" y="238" font-size="12" font-weight="bold" fill="#dc2626">a = 3</text>

                        <!-- Square b=4 (Green) on left vertical side -->
                        <g id="svg-sq-4" class="transition-all duration-300">
                            <rect x="20" y="120" width="100" height="100" fill="#bbf7d0" stroke="#16a34a" stroke-width="2" rx="4" />
                            <text x="50" y="175" font-size="14" font-weight="bold" fill="#166534">b² = 16</text>
                        </g>

                        <!-- Square a=3 (Red/Pink) on bottom horizontal side -->
                        <g id="svg-sq-3" class="transition-all duration-300">
                            <rect x="120" y="220" width="100" height="100" fill="#fecaca" stroke="#dc2626" stroke-width="2" rx="4" />
                            <text x="150" y="275" font-size="14" font-weight="bold" fill="#991b1b">a² = 9</text>
                        </g>

                        <!-- Square c=5 (Blue) on hypotenuse -->
                        <g id="svg-sq-5" class="transition-all duration-300">
                            <polygon points="120,120 220,220 320,120 220,20" fill="#bfdbfe" stroke="#2563eb" stroke-width="2" />
                            <text x="205" y="90" font-size="14" font-weight="bold" fill="#1e40af">c² = 25</text>
                        </g>
                    </svg>

                    <!-- Click/Touch Drop Zones -->
                    <div id="zone-3" onclick="placeSquare(3)" class="absolute bottom-2 right-20 w-24 h-12 border-2 border-dashed border-red-400 bg-red-50/70 rounded flex items-center justify-center text-[10px] font-bold text-red-700 cursor-pointer shadow-sm">
                        <span id="label-zone-3">Pasang Persegi a=3</span>
                    </div>
                    <div id="zone-4" onclick="placeSquare(4)" class="absolute left-2 top-28 w-12 h-24 border-2 border-dashed border-emerald-400 bg-emerald-50/70 rounded flex items-center justify-center text-[10px] font-bold text-emerald-700 cursor-pointer shadow-sm">
                        <span id="label-zone-4">Pasang b=4</span>
                    </div>
                    <div id="zone-5" onclick="placeSquare(5)" class="absolute right-4 top-16 w-16 h-16 border-2 border-dashed border-blue-400 bg-blue-50/70 rounded flex items-center justify-center text-[10px] font-bold text-blue-700 cursor-pointer shadow-sm transform rotate-45">
                        <span id="label-zone-5" class="transform -rotate-45">c=5</span>
                    </div>
                </div>

                <!-- Tray for squares -->
                <div class="space-y-1.5 w-full max-w-sm text-center">
                    <p class="text-xs font-bold text-gray-700">Persegi Tersedia (Ketuk persegi di bawah, lalu ketuk zona):</p>
                    <div class="flex justify-center gap-3">
                        <div id="sq-3" onclick="selectSquareTray(3)" class="draggable-square w-12 h-12 bg-red-100 border-2 border-red-400 rounded-lg flex flex-col items-center justify-center text-xs font-bold text-red-800 shadow-sm cursor-pointer">
                            <span>a=3</span>
                        </div>
                        <div id="sq-4" onclick="selectSquareTray(4)" class="draggable-square w-12 h-12 bg-emerald-100 border-2 border-emerald-400 rounded-lg flex flex-col items-center justify-center text-xs font-bold text-emerald-800 shadow-sm cursor-pointer">
                            <span>b=4</span>
                        </div>
                        <div id="sq-5" onclick="selectSquareTray(5)" class="draggable-square w-12 h-12 bg-blue-100 border-2 border-blue-400 rounded-lg flex flex-col items-center justify-center text-xs font-bold text-blue-800 shadow-sm cursor-pointer">
                            <span>c=5</span>
                        </div>
                    </div>
                    <p id="placement-status" class="text-xs text-gray-500 font-medium">Status: Belum semua persegi terpasang.</p>
                </div>
            </div>

            <!-- Detailed Formula & Side Explanations -->
            <div id="level2-stage-b" class="space-y-4 pt-3 border-t hidden">
                <div class="bg-amber-50 border border-amber-200 p-4 rounded-xl text-xs sm:text-sm text-amber-900 space-y-2">
                    <p class="font-bold text-amber-900">📖 Penjelasan Konsep & Bagian Sisi Segitiga Siku-Siku:</p>
                    <ul class="list-disc list-inside space-y-1.5">
                        <li><b>Sisi Siku-Siku (a dan b / alas dan tinggi):</b> Dua sisi yang saling tegak lurus dan membentuk sudut siku-siku (90&deg;). Di sini a = 3 dan b = 4.</li>
                        <li><b>Sisi Miring (Hipotenusa / c):</b> Sisi terpanjang yang terletak tepat di seberang sudut siku-siku. Di sini c = 5.</li>
                        <li><b>Persamaan Rumus Pembuktian (a&sup2; + b&sup2; = c&sup2;):</b> Luas persegi merah (a&sup2; = 9) ditambah luas persegi hijau (b&sup2; = 16) persis sama dengan luas persegi biru pada sisi miring (c&sup2; = 25).</li>
                    </ul>
                </div>

                <p class="text-xs sm:text-sm text-gray-600 font-medium">
                    <b>Tahap B:</b> Masukkan hasil perhitungan luas masing-masing persegi untuk membuktikan hubungan Teorema Pythagoras.
                </p>
                <div class="grid grid-cols-1 sm:grid-cols-3 gap-3">
                    <div>
                        <label class="block text-xs font-semibold text-gray-700 mb-1">Luas Persegi Sisi a=3 (a&sup2;):</label>
                        <input type="number" id="input-l2-a" class="w-full border rounded-lg px-3 py-2 text-sm bg-white" placeholder="Masukkan luas...">
                    </div>
                    <div>
                        <label class="block text-xs font-semibold text-gray-700 mb-1">Luas Persegi Sisi b=4 (b&sup2;):</label>
                        <input type="number" id="input-l2-b" class="w-full border rounded-lg px-3 py-2 text-sm bg-white" placeholder="Masukkan luas...">
                    </div>
                    <div>
                        <label class="block text-xs font-semibold text-gray-700 mb-1">Luas Persegi Sisi Miring c=5 (c&sup2;):</label>
                        <input type="number" id="input-l2-c" class="w-full border rounded-lg px-3 py-2 text-sm bg-white" placeholder="Masukkan luas...">
                    </div>
                </div>
            </div>

            <div id="feedback-l2" class="hidden p-3 rounded-lg text-sm font-medium text-center"></div>

            <div class="flex justify-end pt-2">
                <button onclick="submitLevel2()" class="btn-secondary">Periksa Jawaban</button>
            </div>
        </div>

        <!-- 5. ASSESSMENT LEVEL 2 -->
        <div id="screen-assessment-2" class="game-card p-6 sm:p-8 space-y-6 hidden text-center">
            <div class="w-16 h-16 bg-emerald-100 text-[#15803d] rounded-full flex items-center justify-center mx-auto text-2xl shadow-inner">
                🔍
            </div>
            <div>
                <span class="text-xs font-bold text-[#15803d] uppercase tracking-wider">Penilaian Level 2</span>
                <h2 class="text-2xl font-extrabold text-gray-800">Discover the Rule Selesai</h2>
            </div>

            <div class="bg-emerald-50 border border-emerald-200 p-6 rounded-2xl max-w-md mx-auto space-y-3 shadow-inner text-left">
                <div class="
