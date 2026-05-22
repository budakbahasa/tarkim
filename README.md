<!DOCTYPE html>
<html lang="ms">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Aplikasi TarKiM - Tarik Kata Imbuhan</title>
    <!-- Tailwind CSS untuk rekaan moden yang responsif -->
    <script src="https://cdn.tailwindcss.com"></script>
    <!-- Google Fonts: Comfortaa untuk tajuk mesra kanak-kanak, Urbanist untuk pembacaan jelas -->
    <link rel="preconnect" href="https://fonts.googleapis.com">
    <link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
    <link href="https://fonts.googleapis.com/css2?family=Comfortaa:wght@500;700&family=Urbanist:wght@400;600;700&display=swap" rel="stylesheet">
    <!-- FontAwesome untuk ikon interaktif -->
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.5.1/css/all.min.css">
    <!-- SweetAlert2 untuk ganjaran visual & audio (feedback positif murid) -->
    <script src="https://cdn.jsdelivr.net/npm/sweetalert2@11"></script>
    
    <script>
        tailwind.config = {
            theme: {
                extend: {
                    fontFamily: {
                        heading: ['Comfortaa', 'sans-serif'],
                        body: ['Urbanist', 'sans-serif'],
                    },
                    colors: {
                        brand: {
                            cream: '#FAF6EE',
                            mint: '#10B981',
                            softMint: '#ECFDF5',
                            darkSlate: '#1E293B',
                            pastelBlue: '#DBEAFE',
                            pastelYellow: '#FEF3C7',
                            pastelPink: '#FCE7F3',
                            pastelPurple: '#F3E8FF',
                        }
                    }
                }
            }
        }
    </script>
    <style>
        body {
            font-family: 'Urbanist', sans-serif;
            background-color: #F1F5F9;
        }
        .bounce-btn:active {
            transform: scale(0.95);
        }
        /* Custom scrollbar mesra visual */
        ::-webkit-scrollbar {
            width: 8px;
        }
        ::-webkit-scrollbar-track {
            background: #F1F5F9;
        }
        ::-webkit-scrollbar-thumb {
            background: #CBD5E1;
            border-radius: 10px;
        }
        .drag-over {
            border-color: #10B981 !important;
            background-color: #ECFDF5 !important;
            transform: scale(1.02);
        }
        .draggable-item {
            cursor: grab;
            transition: all 0.2s ease;
        }
        .draggable-item:active {
            cursor: grabbing;
        }
        
        /* Gaya Balapan Track & Field Khas */
        .track-field {
            background: repeating-linear-gradient(
                90deg,
                #E11D48,
                #E11D48 12px,
                #BE123C 12px,
                #BE123C 24px
            );
            position: relative;
            border-top: 3px solid #FFFFFF;
            border-bottom: 3px solid #FFFFFF;
        }
        .track-line {
            position: absolute;
            top: 50%;
            left: 0;
            right: 0;
            height: 2px;
            background-color: rgba(255, 255, 255, 0.6);
            transform: translateY(-50%);
            border-style: dashed;
        }
    </style>
</head>
<body class="min-h-screen flex items-center justify-center p-0 sm:p-4 select-none">

    <!-- CONTAINER UTAMA -->
    <div id="app-canvas" class="w-full max-w-[1280px] h-[720px] bg-brand-cream rounded-2xl shadow-2xl overflow-hidden relative border-4 border-white flex flex-col justify-between">
        
        <!-- BOKEH BACKGROUND DECORATIONS -->
        <div class="absolute inset-0 pointer-events-none opacity-40 z-0">
            <div class="absolute top-10 left-10 w-72 h-72 bg-emerald-200 rounded-full filter blur-[100px]"></div>
            <div class="absolute bottom-10 right-10 w-80 h-80 bg-blue-200 rounded-full filter blur-[100px]"></div>
            <div class="absolute top-1/2 left-1/3 w-60 h-60 bg-amber-100 rounded-full filter blur-[80px]"></div>
        </div>

        <!-- HEADER UTAMA -->
        <header id="app-header" class="hidden w-full px-8 py-4 bg-white/80 backdrop-blur-md border-b border-slate-100 flex justify-between items-center z-10">
            <div class="flex items-center gap-3">
                <div class="w-10 h-10 bg-brand-mint rounded-full flex items-center justify-center text-white font-bold text-xl font-heading shadow-md shadow-emerald-200">T</div>
                <div>
                    <h1 class="font-heading font-bold text-lg text-brand-darkSlate">TarKiM</h1>
                    <p class="text-xs text-slate-500">Tarik Kata Imbuhan</p>
                </div>
            </div>
            <!-- Status Murid -->
            <div class="flex items-center gap-4">
                <div class="bg-emerald-50 border border-emerald-200 px-4 py-1.5 rounded-full flex items-center gap-2">
                    <i class="fa-solid fa-user-astronaut text-brand-mint text-sm"></i>
                    <span id="header-user-info" class="font-bold text-slate-700 text-sm">Murid</span>
                </div>
                <button onclick="logout()" class="text-slate-400 hover:text-red-500 transition-colors duration-200 text-sm flex items-center gap-1">
                    <i class="fa-solid fa-right-from-bracket"></i> Keluar
                </button>
            </div>
        </header>

        <!-- KANDUNGAN SKRIN (DYNAMIC VIEWS) -->
        <main class="flex-grow flex flex-col items-center justify-start px-12 py-6 z-10 overflow-y-auto w-full">
            
            <!-- 1. SKRIN LOG MASUK -->
            <div id="screen-login" class="w-full max-w-md bg-white p-8 rounded-2xl shadow-xl border border-slate-100 flex flex-col items-center my-auto">
                <div class="w-20 h-20 bg-brand-softMint rounded-full flex items-center justify-center text-brand-mint text-4xl mb-4 shadow-inner">
                    <i class="fa-solid fa-rocket"></i>
                </div>
                <h2 class="font-heading font-bold text-2xl text-brand-darkSlate mb-1 text-center">Selamat Datang!</h2>
                <p class="text-slate-500 text-sm mb-6 text-center">Sila masukkan nama dan kelas untuk mula belajar.</p>
                
                <div class="w-full space-y-4">
                    <div>
                        <label class="block text-xs font-bold uppercase tracking-wider text-slate-500 mb-2">Nama Panggilan</label>
                        <input type="text" id="input-name" placeholder="Contoh: Amirul" class="w-full px-4 py-3 rounded-xl border-2 border-slate-100 focus:border-brand-mint outline-none transition-all font-semibold text-slate-700 text-lg">
                    </div>
                    <div>
                        <label class="block text-xs font-bold uppercase tracking-wider text-slate-500 mb-2">Nama Kelas</label>
                        <input type="text" id="input-class" placeholder="Contoh: 3 Cemerlang" class="w-full px-4 py-3 rounded-xl border-2 border-slate-100 focus:border-brand-mint outline-none transition-all font-semibold text-slate-700 text-lg">
                    </div>
                    <button onclick="handleLogin()" class="w-full py-4 bg-brand-mint text-white font-heading font-bold text-lg rounded-xl shadow-lg shadow-emerald-200 hover:bg-emerald-600 transition-all bounce-btn mt-2">
                        Mula Belajar! <i class="fa-solid fa-arrow-right ml-2"></i>
                    </button>

                    <div class="pt-4 border-t border-slate-100 flex justify-center">
                        <button onclick="showAdminView()" class="text-slate-400 hover:text-slate-600 text-xs font-semibold flex items-center gap-1.5">
                            <i class="fa-solid fa-screwdriver-wrench"></i> Paparan Admin
                        </button>
                    </div>
                </div>
            </div>

            <!-- 2. SKRIN MENU UTAMA -->
            <div id="screen-menu" class="hidden w-full max-w-3xl flex flex-col items-center my-auto">
                <h2 class="font-heading font-bold text-3xl text-brand-darkSlate mb-8 text-center">Pilih Menu Pembelajaran</h2>
                
                <div class="w-full flex flex-col gap-4">
                    <!-- Nota Interaktif -->
                    <button onclick="showSection('nota')" class="bounce-btn w-full bg-white hover:bg-brand-pastelBlue/20 border border-slate-100 p-4 rounded-2xl flex items-center gap-6 shadow-sm transition-all text-left">
                        <div class="w-14 h-14 bg-brand-pastelBlue rounded-xl flex items-center justify-center text-blue-600 text-2xl shadow-inner">
                            <i class="fa-solid fa-book-open"></i>
                        </div>
                        <div>
                            <h3 class="font-heading font-bold text-xl text-brand-darkSlate">Nota Interaktif</h3>
                            <p class="text-slate-500 text-sm">Lihat nota grafik formula PTKS & Track & Field.</p>
                        </div>
                    </button>

                    <!-- Teknik Mudah TarKiM -->
                    <button onclick="showSection('teknik')" class="bounce-btn w-full bg-white hover:bg-brand-softMint border border-slate-100 p-4 rounded-2xl flex items-center gap-6 shadow-sm transition-all text-left">
                        <div class="w-14 h-14 bg-brand-softMint rounded-xl flex items-center justify-center text-brand-mint text-2xl shadow-inner">
                            <i class="fa-solid fa-lightbulb"></i>
                        </div>
                        <div>
                            <h3 class="font-heading font-bold text-xl text-brand-darkSlate">Teknik Mudah TarKiM</h3>
                            <p class="text-slate-500 text-sm">Panduan kedudukan atlet Track & Field dalam balapan.</p>
                        </div>
                    </button>

                    <!-- Video Pembelajaran -->
                    <button onclick="showSection('video')" class="bounce-btn w-full bg-white hover:bg-brand-pastelYellow/20 border border-slate-100 p-4 rounded-2xl flex items-center gap-6 shadow-sm transition-all text-left">
                        <div class="w-14 h-14 bg-brand-pastelYellow rounded-xl flex items-center justify-center text-amber-500 text-2xl shadow-inner">
                            <i class="fa-solid fa-circle-play"></i>
                        </div>
                        <div>
                            <h3 class="font-heading font-bold text-xl text-brand-darkSlate">Video Pembelajaran</h3>
                            <p class="text-slate-500 text-sm">Tonton video penerangan interaktif guru Bahasa Melayu.</p>
                        </div>
                    </button>

                    <!-- Permainan Interaktif -->
                    <button onclick="showSection('permainan')" class="bounce-btn w-full bg-white hover:bg-brand-pastelPink/20 border border-slate-100 p-4 rounded-2xl flex items-center gap-6 shadow-sm transition-all text-left">
                        <div class="w-14 h-14 bg-brand-pastelPink rounded-xl flex items-center justify-center text-pink-500 text-2xl shadow-inner">
                            <i class="fa-solid fa-gamepad"></i>
                        </div>
                        <div>
                            <h3 class="font-heading font-bold text-xl text-brand-darkSlate">Permainan Interaktif</h3>
                            <p class="text-slate-500 text-sm">Mulakan cabaran seronok tolak & tarik kata imbuhan.</p>
                        </div>
                    </button>

                    <!-- Kuiz Penilaian -->
                    <button onclick="showSection('kuiz')" class="bounce-btn w-full bg-white hover:bg-brand-pastelPurple/20 border border-slate-100 p-4 rounded-2xl flex items-center gap-6 shadow-sm transition-all text-left">
                        <div class="w-14 h-14 bg-brand-pastelPurple rounded-xl flex items-center justify-center text-purple-600 text-2xl shadow-inner">
                            <i class="fa-solid fa-circle-question"></i>
                        </div>
                        <div>
                            <h3 class="font-heading font-bold text-xl text-brand-darkSlate">Uji Minda (Kuiz)</h3>
                            <p class="text-slate-500 text-sm">Uji tahap kefahaman anda untuk kumpul bintang ganjaran.</p>
                        </div>
                    </button>
                </div>
            </div>

            <!-- 3. SEKSYEN NOTA INTERAKTIF -->
            <div id="section-nota" class="hidden w-full max-w-5xl flex flex-col">
                <div class="flex justify-between items-center mb-4">
                    <button onclick="goBackToMenu()" class="text-slate-500 hover:text-slate-700 font-semibold flex items-center gap-1.5 bg-white px-4 py-2 rounded-xl border border-slate-100 shadow-sm bounce-btn text-xs sm:text-sm">
                        <i class="fa-solid fa-chevron-left"></i> Kembali ke Menu
                    </button>
                    <h2 class="font-heading font-bold text-xl sm:text-2xl text-brand-darkSlate">Nota Interaktif Imbuhan</h2>
                </div>
                <div class="grid grid-cols-1 lg:grid-cols-12 gap-6 items-stretch">
                    <!-- Nota Kiri -->
                    <div class="bg-white p-5 rounded-2xl border border-slate-100 shadow-sm flex flex-col lg:col-span-7">
                        <h4 class="font-bold text-base text-slate-800 mb-2 flex items-center gap-2">
                            <span class="w-6 h-6 bg-red-100 text-red-600 rounded-lg flex items-center justify-center text-xs"><i class="fa-solid fa-scissors"></i></span>
                            Formula Peleburan Huruf P, T, K, S
                        </h4>
                        <div id="container-nota-1" class="w-full flex-grow bg-slate-50 rounded-xl overflow-hidden p-2 flex items-center justify-center border border-slate-200">
                            <!-- Dynamic Nota 1 -->
                        </div>
                    </div>
                    <!-- Nota Kanan -->
                    <div class="bg-white p-5 rounded-2xl border border-slate-100 shadow-sm flex flex-col lg:col-span-5">
                        <h4 class="font-bold text-base text-slate-800 mb-2 flex items-center gap-2">
                            <span class="w-6 h-6 bg-emerald-100 text-brand-mint rounded-lg flex items-center justify-center text-xs"><i class="fa-solid fa-person-running"></i></span>
                            Kit Kedudukan Imbuhan (Track & Field)
                        </h4>
                        <div id="container-nota-2" class="w-full flex-grow bg-slate-50 rounded-xl overflow-hidden p-2 flex items-center justify-center border border-slate-200">
                            <!-- Dynamic Nota 2 -->
                        </div>
                    </div>
                </div>
            </div>

            <!-- 4. SEKSYEN TEKNIK TARKIM -->
            <div id="section-teknik" class="hidden w-full max-w-4xl flex flex-col">
                <div class="flex justify-between items-center mb-6">
                    <button onclick="goBackToMenu()" class="text-slate-500 hover:text-slate-700 font-semibold flex items-center gap-1.5 bg-white px-4 py-2 rounded-xl border border-slate-100 shadow-sm bounce-btn">
                        <i class="fa-solid fa-chevron-left"></i> Kembali ke Menu
                    </button>
                    <h2 class="font-heading font-bold text-2xl text-brand-darkSlate">Teknik Mudah TarKiM</h2>
                </div>
                <div class="bg-white p-6 sm:p-8 rounded-2xl border border-slate-100 shadow-sm flex flex-col md:flex-row gap-8 items-center">
                    <div id="container-teknik-1" class="w-full md:w-1/2 h-[420px] bg-slate-100 rounded-xl overflow-hidden flex items-center justify-center border border-slate-200">
                        <!-- Dynamic Teknik -->
                    </div>
                    <div class="w-full md:w-1/2 space-y-4 text-left">
                        <h3 class="font-heading font-bold text-2xl text-brand-darkSlate">Bagaimana TarKiM Berfungsi?</h3>
                        <div class="flex gap-4">
                            <div class="w-8 h-8 bg-brand-softMint rounded-full flex items-center justify-center text-brand-mint font-bold shrink-0 shadow-sm">1</div>
                            <p class="text-slate-600 font-semibold text-sm">Lihat Kolam Pilihan Perkataan di bawah dan pilih mana-mana perkataan yang anda inginkan.</p>
                        </div>
                        <div class="flex gap-4">
                            <div class="w-8 h-8 bg-blue-100 rounded-full flex items-center justify-center text-blue-600 font-bold shrink-0 shadow-sm">2</div>
                            <p class="text-slate-600 font-semibold text-sm">Tuliskan **Imbuhan Kiri**, **Kata Dasar** di tengah, dan **Imbuhan Kanan** secara bebas merujuk kedudukan balapan larian.</p>
                        </div>
                        <div class="flex gap-4">
                            <div class="w-8 h-8 bg-pink-100 rounded-full flex items-center justify-center text-pink-600 font-bold shrink-0 shadow-sm">3</div>
                            <p class="text-slate-600 font-semibold text-sm">Pilih kategori imbuhan yang betul (Awalan, Akhiran, Apitan, atau Sisipan) merujuk kedudukan atlet larian.</p>
                        </div>
                        <div class="flex gap-4">
                            <div class="w-8 h-8 bg-purple-100 rounded-full flex items-center justify-center text-purple-600 font-bold shrink-0 shadow-sm">4</div>
                            <p class="text-slate-600 font-semibold text-sm">Tekan Sahkan Jawapan. Walau salah atau betul, markah anda tetap akan direkodkan tanpa disekat!</p>
                        </div>
                    </div>
                </div>
            </div>

            <!-- 5. SEKSYEN VIDEO PEMBELAJARAN -->
            <div id="section-video" class="hidden w-full max-w-4xl flex flex-col">
                <div class="flex justify-between items-center mb-6">
                    <button onclick="goBackToMenu()" class="text-slate-500 hover:text-slate-700 font-semibold flex items-center gap-1.5 bg-white px-4 py-2 rounded-xl border border-slate-100 shadow-sm bounce-btn">
                        <i class="fa-solid fa-chevron-left"></i> Kembali ke Menu
                    </button>
                    <h2 class="font-heading font-bold text-2xl text-brand-darkSlate">Video Pembelajaran</h2>
                </div>
                <div class="bg-white p-6 rounded-2xl border border-slate-100 shadow-sm flex flex-col items-center">
                    <div class="w-full max-w-3xl aspect-video bg-slate-900 rounded-xl overflow-hidden flex items-center justify-center relative shadow-inner">
                        <iframe id="video-frame" class="w-full h-full" src="https://www.youtube.com/embed/dQw4w9WgXcQ" title="Video Pembelajaran TarKiM" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" allowfullscreen></iframe>
                    </div>
                    <h3 class="font-heading font-bold text-xl text-slate-800 mt-6">Panduan Sebutan & Struktur Imbuhan</h3>
                    <p class="text-slate-500 text-sm mt-1 max-w-xl text-center">Tonton video ini untuk mengukuhkan pemahaman anda tentang kaedah mengimbuhkan kata dasar Bahasa Melayu dengan cara yang betul.</p>
                </div>
            </div>

            <!-- 6. SEKSYEN PERMAINAN INTERAKTIF -->
            <div id="section-permainan" class="hidden w-full max-w-4xl flex flex-col">
                <!-- Header Utama Permainan (Hanya muncul semasa pilihan tahap) -->
                <div id="permainan-main-header" class="flex justify-between items-center mb-6 w-full">
                    <button onclick="goBackToMenu()" class="text-slate-500 hover:text-slate-700 font-semibold flex items-center gap-1.5 bg-white px-4 py-2 rounded-xl border border-slate-100 shadow-sm bounce-btn text-xs sm:text-sm">
                        <i class="fa-solid fa-chevron-left"></i> Kembali ke Menu Utama
                    </button>
                    <h2 class="font-heading font-bold text-xl sm:text-2xl text-brand-darkSlate">Latihan Pengukuhan TarKiM</h2>
                </div>

                <!-- Pemilihan Tahap -->
                <div id="level-selection" class="grid grid-cols-1 md:grid-cols-3 gap-6 w-full">
                    <button onclick="startLevel(1)" class="bounce-btn bg-white hover:border-emerald-500 border-2 border-slate-100 p-8 rounded-2xl flex flex-col items-center text-center shadow-sm transition-all">
                        <div class="w-16 h-16 bg-emerald-100 text-emerald-600 rounded-full flex items-center justify-center text-3xl mb-4"><i class="fa-solid fa-seedling"></i></div>
                        <h3 class="font-heading font-bold text-xl text-brand-darkSlate mb-2">Tahap 1</h3>
                        <p class="text-xs font-bold uppercase text-emerald-600 tracking-wider mb-2">Aras Rendah</p>
                        <p class="text-slate-500 text-sm">Menulis pemisahan kata bagi kata berimbuhan asas secara bebas.</p>
                    </button>
                    <button onclick="startLevel(2)" class="bounce-btn bg-white hover:border-amber-500 border-2 border-slate-100 p-8 rounded-2xl flex flex-col items-center text-center shadow-sm transition-all">
                        <div class="w-16 h-16 bg-amber-100 text-amber-600 rounded-full flex items-center justify-center text-3xl mb-4"><i class="fa-solid fa-tree"></i></div>
                        <h3 class="font-heading font-bold text-xl text-brand-darkSlate mb-2">Tahap 2</h3>
                        <p class="text-xs font-bold uppercase text-amber-600 tracking-wider mb-2">Aras Sederhana</p>
                        <p class="text-slate-500 text-sm">Membongkar perkataan dengan perubahan huruf & sisipan.</p>
                    </button>
                    <button onclick="startLevel(3)" class="bounce-btn bg-white hover:border-indigo-500 border-2 border-slate-100 p-8 rounded-2xl flex flex-col items-center text-center shadow-sm transition-all">
                        <div class="w-16 h-16 bg-indigo-100 text-indigo-600 rounded-full flex items-center justify-center text-3xl mb-4"><i class="fa-solid fa-mountain"></i></div>
                        <h3 class="font-heading font-bold text-xl text-brand-darkSlate mb-2">Tahap 3</h3>
                        <p class="text-xs font-bold uppercase text-indigo-600 tracking-wider mb-2">Aras Tinggi</p>
                        <p class="text-slate-500 text-sm">Kesan & kelaskan kata berimbuhan daripada pelbagai teks petikan.</p>
                    </button>
                </div>

                <!-- PAPARAN PERMAINAN AKTIF (TAHAP 1 & 2) -->
                <div id="gameplay-area" class="hidden w-full bg-white border border-slate-100 rounded-2xl p-6 shadow-md flex flex-col items-center relative">
                    <!-- Navigasi Menu Atas Latihan -->
                    <div class="flex justify-between items-center w-full mb-4">
                        <button onclick="exitGameplayToMenu()" class="text-emerald-700 hover:text-white hover:bg-emerald-600 font-bold text-xs sm:text-sm flex items-center gap-1.5 bg-emerald-50 border border-emerald-200 px-4 py-2 rounded-xl transition-all duration-200 shadow-sm bounce-btn">
                            <i class="fa-solid fa-arrow-left"></i> Kembali Ke Pilihan Tahap
                        </button>
                        <span id="active-level-label" class="text-sm font-heading font-bold text-brand-mint">Tahap 1 (Aras Rendah)</span>
                    </div>

                    <!-- Progress Bar -->
                    <div class="w-full flex justify-between items-center mb-4 text-xs font-bold text-slate-500 uppercase tracking-wider">
                        <span id="game-step-counter">Latihan 1 daripada 5</span>
                        <div class="w-1/2 bg-slate-100 h-2.5 rounded-full overflow-hidden">
                            <div id="game-progress-bar" class="bg-brand-mint h-full transition-all duration-300" style="width: 20%;"></div>
                        </div>
                    </div>

                    <!-- ARAHAN INTERAKTIF -->
                    <div class="bg-emerald-50 border border-emerald-200 text-emerald-800 px-6 py-2.5 rounded-full text-center font-bold text-xs sm:text-sm mb-6 flex items-center gap-2">
                        <i class="fa-solid fa-bullhorn text-brand-mint"></i>
                        <span id="game-instruction-text">Pilih mana-mana perkataan di kolam bawah untuk mula!</span>
                    </div>

                    <!-- KANVAS INPUT DIKOSONGKAN -->
                    <div class="grid grid-cols-3 gap-4 w-full mb-6 relative items-stretch">
                        <!-- BAHAGIAN KIRI -->
                        <div class="flex flex-col items-center p-4 bg-blue-50/50 rounded-2xl border border-blue-100">
                            <span class="text-xs font-bold uppercase tracking-wider text-blue-600 mb-1 text-center font-heading">KIRI</span>
                            <span class="text-[10px] font-semibold text-slate-400 mb-3 text-center">Imbuhan Awalan / Sisipan</span>
                            <input type="text" id="input-imbuhan-kiri" placeholder="Contoh: me-" class="w-full px-3 py-3 rounded-xl border-2 border-slate-200 focus:border-blue-400 outline-none transition-all font-bold text-center text-slate-700 text-lg uppercase shadow-sm">
                        </div>

                        <!-- BAHAGIAN TENGAH -->
                        <div class="flex flex-col items-center p-4 bg-slate-50 border-2 border-dashed border-slate-300 rounded-2xl justify-center">
                            <span class="text-xs font-bold uppercase tracking-wider text-slate-500 mb-1 text-center font-heading">TENGAH</span>
                            <span class="text-[10px] font-semibold text-slate-400 mb-3 text-center">Kata Dasar</span>
                            <input type="text" id="input-kata-dasar-tengah" placeholder="Contoh: lukis" class="w-full px-3 py-3 rounded-xl border-2 border-slate-200 focus:border-emerald-400 outline-none transition-all font-bold text-center text-slate-700 text-lg uppercase shadow-sm">
                        </div>

                        <!-- BAHAGIAN KANAN -->
                        <div class="flex flex-col items-center p-4 bg-pink-50/50 rounded-2xl border border-pink-100">
                            <span class="text-xs font-bold uppercase tracking-wider text-pink-600 mb-1 text-center font-heading">KANAN</span>
                            <span class="text-[10px] font-semibold text-slate-400 mb-3 text-center">Imbuhan Akhiran</span>
                            <input type="text" id="input-imbuhan-kanan" placeholder="Contoh: -an" class="w-full px-3 py-3 rounded-xl border-2 border-slate-200 focus:border-pink-400 outline-none transition-all font-bold text-center text-slate-700 text-lg uppercase shadow-sm">
                        </div>
                    </div>

                    <!-- PEMILIH JENIS IMBUHAN -->
                    <div class="w-full bg-slate-50 border border-slate-200 rounded-xl p-4 mb-6">
                        <span class="block text-xs font-bold uppercase tracking-wider text-slate-400 mb-3 text-center">Kenal Pasti & Pilih Jenis Imbuhan</span>
                        <div class="grid grid-cols-2 md:grid-cols-4 gap-3">
                            <button type="button" onclick="selectAffixCategory('Imbuhan Awalan')" id="btn-cat-awalan" class="py-3 px-2 bg-white border-2 border-slate-200 text-slate-700 font-bold rounded-xl hover:bg-emerald-50 hover:border-brand-mint transition-all text-sm shadow-sm bounce-btn">Imbuhan Awalan</button>
                            <button type="button" onclick="selectAffixCategory('Imbuhan Akhiran')" id="btn-cat-akhiran" class="py-3 px-2 bg-white border-2 border-slate-200 text-slate-700 font-bold rounded-xl hover:bg-emerald-50 hover:border-brand-mint transition-all text-sm shadow-sm bounce-btn">Imbuhan Akhiran</button>
                            <button type="button" onclick="selectAffixCategory('Imbuhan Apitan')" id="btn-cat-apitan" class="py-3 px-2 bg-white border-2 border-slate-200 text-slate-700 font-bold rounded-xl hover:bg-emerald-50 hover:border-brand-mint transition-all text-sm shadow-sm bounce-btn">Imbuhan Apitan</button>
                            <button type="button" onclick="selectAffixCategory('Imbuhan Sisipan')" id="btn-cat-sisipan" class="py-3 px-2 bg-white border-2 border-slate-200 text-slate-700 font-bold rounded-xl hover:bg-emerald-50 hover:border-brand-mint transition-all text-sm shadow-sm bounce-btn">Imbuhan Sisipan</button>
                        </div>
                    </div>

                    <!-- RUANGAN KOTAK DRAG PERKATAAN -->
                    <div id="drag-target-zone" class="w-full max-w-lg border-4 border-dashed border-emerald-300 bg-emerald-50/40 rounded-2xl p-6 mb-6 flex flex-col items-center justify-center min-h-[110px] transition-all duration-200 relative cursor-pointer" ondragover="allowDrop(event)" ondragleave="dragLeave(event)" ondrop="handleDrop(event)">
                        <div id="drag-placeholder-text" class="text-slate-400 font-bold text-center flex flex-col items-center gap-1">
                            <i class="fa-solid fa-hand-pointer text-2xl text-emerald-500 animate-bounce"></i>
                            <span>Heret perkataan pilihan anda ke sini atau ketik di kolam bawah</span>
                        </div>
                        <div id="active-dragged-word" class="hidden text-4xl font-heading font-bold text-emerald-600 tracking-wide"></div>
                    </div>

                    <!-- KOLAM PILIHAN PERKATAAN TERBUKA -->
                    <div class="w-full bg-slate-100 rounded-xl p-4 border border-slate-200 mb-6">
                        <span class="block text-xs font-bold uppercase tracking-wider text-slate-500 text-center mb-3">Kolam Pilihan Perkataan (Bebas Memilih Mana-Mana)</span>
                        <div id="draggable-options-pool" class="flex flex-wrap gap-3 justify-center max-h-[150px] overflow-y-auto p-2"></div>
                    </div>

                    <div class="flex gap-4">
                        <button onclick="resetGameplayInput()" class="px-6 py-3 bg-slate-100 text-slate-600 font-semibold rounded-xl hover:bg-slate-200 transition-colors bounce-btn text-xs sm:text-sm">
                            <i class="fa-solid fa-arrow-rotate-left"></i> Padam Semua
                        </button>
                        <button onclick="verifyDeconstruction()" class="px-8 py-3 bg-brand-mint text-white font-heading font-bold rounded-xl shadow-lg shadow-emerald-200 hover:bg-emerald-600 transition-all bounce-btn text-xs sm:text-sm">
                            Sahkan Jawapan <i class="fa-solid fa-circle-check ml-1.5"></i>
                        </button>
                    </div>
                </div>

                <!-- PAPARAN PERMAINAN TAHAP 3 -->
                <div id="gameplay-area-level3" class="hidden w-full bg-white border border-slate-100 rounded-2xl p-6 shadow-md flex flex-col items-center">
                    <!-- Navigator Petikan dan Butang Kembali -->
                    <div class="w-full flex justify-between items-center mb-4">
                        <button onclick="exitGameplayToMenu()" class="text-indigo-700 hover:text-white hover:bg-indigo-600 font-bold text-xs sm:text-sm flex items-center gap-1.5 bg-indigo-50 border border-indigo-200 px-4 py-2 rounded-xl transition-all duration-200 shadow-sm bounce-btn">
                            <i class="fa-solid fa-arrow-left"></i> Kembali Ke Pilihan Tahap
                        </button>
                        <div id="passage-navigator-buttons" class="flex gap-2"></div>
                    </div>

                    <!-- ARAHAN -->
                    <div class="bg-indigo-50 border border-indigo-200 text-indigo-800 px-6 py-2.5 rounded-full text-center font-bold text-xs sm:text-sm mb-6 flex items-center justify-center gap-2 w-full">
                        <i class="fa-solid fa-magnifying-glass text-indigo-600"></i>
                        <span id="level3-instruction">Mula-mula: Ketik seberapa banyak kata berimbuhan dalam petikan di bawah.</span>
                    </div>

                    <!-- KOTAK PETIKAN TEKS INTERAKTIF -->
                    <div id="interactive-text-box" class="w-full bg-slate-50 p-6 rounded-2xl border border-slate-200 text-lg leading-relaxed text-slate-700 min-h-[140px] mb-6 select-none text-left"></div>

                    <!-- FASA 2: KATEGORISASI PERKATAAN -->
                    <div id="level3-classification-section" class="hidden w-full border-t border-slate-100 pt-6 mt-2">
                        <div class="bg-purple-50 border border-purple-200 text-purple-800 px-6 py-2.5 rounded-xl text-center font-bold text-xs sm:text-sm mb-6 flex items-center justify-center gap-2 w-full">
                            <i class="fa-solid fa-tags"></i>
                            <span>Seterusnya: Kategorikan perkataan yang anda pilih di bawah.</span>
                        </div>
                        <div class="overflow-y-auto max-h-[220px] pr-2 space-y-4" id="level3-classification-list"></div>
                    </div>

                    <div class="flex gap-4 mt-6">
                        <button id="btn-level3-action" onclick="proceedToClassification()" class="px-8 py-3 bg-indigo-600 text-white font-heading font-bold rounded-xl shadow-lg shadow-indigo-200 hover:bg-indigo-700 transition-all bounce-btn text-xs sm:text-sm">
                            Kategorikan Perkataan <i class="fa-solid fa-tags ml-1.5"></i>
                        </button>
                    </div>
                </div>
            </div>

            <!-- 7. SEKSYEN KUIZ -->
            <div id="section-kuiz" class="hidden w-full max-w-3xl flex flex-col my-auto">
                <div class="flex justify-between items-center mb-6">
                    <button onclick="goBackToMenu()" class="text-slate-500 hover:text-slate-700 font-semibold flex items-center gap-1.5 bg-white px-4 py-2 rounded-xl border border-slate-100 shadow-sm bounce-btn">
                        <i class="fa-solid fa-chevron-left"></i> Kembali ke Menu
                    </button>
                    <h2 class="font-heading font-bold text-2xl text-brand-darkSlate">Kuiz Formatif TarKiM</h2>
                </div>

                <!-- Papan Pertanyaan Kuiz -->
                <div class="bg-white border border-slate-100 rounded-2xl p-6 shadow-md flex flex-col items-center">
                    <div class="w-full flex justify-between text-xs font-bold text-slate-400 uppercase tracking-wider mb-4">
                        <span id="quiz-question-num">Soalan 1 daripada 6</span>
                        <span id="quiz-objective-tag">Objektif: Kategorikan Kata</span>
                    </div>

                    <!-- SOALAN -->
                    <div id="quiz-question-text" class="text-xl font-bold text-brand-darkSlate text-center mb-8 min-h-[60px]"></div>

                    <!-- PILIHAN JAWAPAN -->
                    <div id="quiz-options-container" class="grid grid-cols-1 md:grid-cols-2 gap-4 w-full mb-8"></div>

                    <button id="quiz-next-btn" onclick="nextQuizQuestion()" disabled class="px-8 py-3.5 bg-brand-mint text-white font-heading font-bold rounded-xl shadow-lg hover:bg-emerald-600 transition-all disabled:opacity-50 disabled:pointer-events-none bounce-btn">
                        Seterusnya <i class="fa-solid fa-chevron-right ml-1.5"></i>
                    </button>
                </div>
            </div>

            <!-- 8. SEKSYEN PAPARAN ADMIN -->
            <div id="section-admin" class="hidden w-full max-w-5xl flex flex-col h-full overflow-hidden">
                <div class="flex justify-between items-center mb-4">
                    <button onclick="exitAdminView()" class="text-slate-500 hover:text-slate-700 font-semibold flex items-center gap-1.5 bg-white px-4 py-2 rounded-xl border border-slate-100 shadow-sm bounce-btn">
                        <i class="fa-solid fa-chevron-left"></i> Kembali ke Log Masuk
                    </button>
                    <h2 class="font-heading font-bold text-xl sm:text-2xl text-brand-darkSlate flex items-center gap-2">
                        <i class="fa-solid fa-screwdriver-wrench text-brand-mint"></i> Urus Kandungan TarKiM
                    </h2>
                </div>

                <!-- TABS ADMIN -->
                <div class="flex border-b border-slate-200 mb-6 flex-wrap">
                    <button onclick="switchAdminTab('scores')" id="btn-tab-scores" class="px-3.5 py-2 font-bold text-xs sm:text-sm border-b-2 border-brand-mint text-brand-mint">
                        <i class="fa-solid fa-chart-line mr-1"></i> Markah Murid
                    </button>
                    <button onclick="switchAdminTab('edit-words')" id="btn-tab-words" class="px-3.5 py-2 font-bold text-xs sm:text-sm border-b-2 border-transparent text-slate-500 hover:text-slate-800">
                        <i class="fa-solid fa-pen-to-square mr-1"></i> Sunting Tahap 1 & 2
                    </button>
                    <button onclick="switchAdminTab('edit-passages')" id="btn-tab-passages" class="px-3.5 py-2 font-bold text-xs sm:text-sm border-b-2 border-transparent text-slate-500 hover:text-slate-800">
                        <i class="fa-solid fa-paragraph mr-1"></i> Sunting Tahap 3 (Petikan)
                    </button>
                    <button onclick="switchAdminTab('edit-quiz')" id="btn-tab-quiz" class="px-3.5 py-2 font-bold text-xs sm:text-sm border-b-2 border-transparent text-slate-500 hover:text-slate-800">
                        <i class="fa-solid fa-spell-check mr-1"></i> Sunting Kuiz
                    </button>
                    <button onclick="switchAdminTab('media')" id="btn-tab-media" class="px-3.5 py-2 font-bold text-xs sm:text-sm border-b-2 border-transparent text-slate-500 hover:text-slate-800">
                        <i class="fa-solid fa-photo-film mr-1"></i> Bahan Media & Video
                    </button>
                    <button onclick="switchAdminTab('tech-integration')" id="btn-tab-tech" class="px-3.5 py-2 font-bold text-xs sm:text-sm border-b-2 border-transparent text-slate-500 hover:text-slate-800">
                        <i class="fa-solid fa-network-wired mr-1"></i> Google Sheets API
                    </button>
                </div>

                <!-- KANDUNGAN TABS -->
                <div class="bg-white border border-slate-100 rounded-2xl p-6 shadow-md flex-grow overflow-y-auto max-h-[420px]">
                    
                    <!-- TAB 1: MARKAH MURID -->
                    <div id="admin-tab-scores" class="space-y-4">
                        <div class="flex justify-between items-center">
                            <h3 class="font-bold text-base text-brand-darkSlate uppercase tracking-wider">Laporan Skor</h3>
                            <button onclick="clearLocalStorageScores()" class="text-xs text-red-500 hover:text-red-700 font-semibold"><i class="fa-solid fa-trash-can"></i> Padam Rekod Skor</button>
                        </div>
                        <div class="overflow-x-auto">
                            <table class="w-full text-left border-collapse">
                                <thead>
                                    <tr class="bg-slate-50 text-slate-500 text-xs font-bold uppercase tracking-wider border-b border-slate-200">
                                        <th class="p-3">Nama</th>
                                        <th class="p-3">Kelas</th>
                                        <th class="p-3">Aktiviti / Tahap</th>
                                        <th class="p-3">Skor</th>
                                        <th class="p-3">Pencapaian / Status</th>
                                        <th class="p-3">Masa Selesai</th>
                                    </tr>
                                </thead>
                                <tbody id="table-scores-body" class="text-sm divide-y divide-slate-100"></tbody>
                            </table>
                        </div>
                    </div>

                    <!-- TAB 2: SUNTING TAHAP 1 & 2 -->
                    <div id="admin-tab-words" class="hidden space-y-6">
                        <div class="grid grid-cols-1 md:grid-cols-2 gap-6">
                            <!-- Borang -->
                            <div class="space-y-4 text-left">
                                <h3 class="font-bold text-lg text-brand-darkSlate">Tambah Perkataan Baharu</h3>
                                <div class="space-y-3">
                                    <div>
                                        <label class="block text-xs font-bold text-slate-500 uppercase mb-1">Tahap</label>
                                        <select id="admin-word-level" class="w-full px-3 py-2 rounded-lg border border-slate-200 text-sm">
                                            <option value="1">Tahap 1 (Morfologi Kekal)</option>
                                            <option value="2">Tahap 2 (Morfologi Berubah / Sisipan)</option>
                                        </select>
                                    </div>
                                    <div>
                                        <label class="block text-xs font-bold text-slate-500 uppercase mb-1">Kata Dasar</label>
                                        <input type="text" id="admin-word-base" placeholder="Contoh: lukis" class="w-full px-3 py-2 rounded-lg border border-slate-200 text-sm font-semibold">
                                    </div>
                                    <div class="grid grid-cols-2 gap-3">
                                        <div>
                                            <label class="block text-xs font-bold text-slate-500 uppercase mb-1">Imbuhan Awalan (Kiri)</label>
                                            <input type="text" id="admin-word-prefix" placeholder="me-" class="w-full px-3 py-2 rounded-lg border border-slate-200 text-sm font-semibold">
                                        </div>
                                        <div>
                                            <label class="block text-xs font-bold text-slate-500 uppercase mb-1">Imbuhan Akhiran (Kanan)</label>
                                            <input type="text" id="admin-word-suffix" placeholder="-an" class="w-full px-3 py-2 rounded-lg border border-slate-200 text-sm font-semibold">
                                        </div>
                                    </div>
                                    <div>
                                        <label class="block text-xs font-bold text-slate-500 uppercase mb-1">Kata Berimbuhan Lengkap</label>
                                        <input type="text" id="admin-word-full" placeholder="Contoh: melukis" class="w-full px-3 py-2 rounded-lg border border-slate-200 text-sm font-semibold">
                                    </div>
                                    <div>
                                        <label class="block text-xs font-bold text-slate-500 uppercase mb-1">Jenis Kategori</label>
                                        <select id="admin-word-type" class="w-full px-3 py-2 rounded-lg border border-slate-200 text-sm">
                                            <option value="Imbuhan Awalan">Imbuhan Awalan</option>
                                            <option value="Imbuhan Akhiran">Imbuhan Akhiran</option>
                                            <option value="Imbuhan Apitan">Imbuhan Apitan</option>
                                            <option value="Imbuhan Sisipan">Imbuhan Sisipan</option>
                                        </select>
                                    </div>
                                    <button onclick="saveAdminWord()" class="w-full py-2.5 bg-brand-mint text-white font-heading font-bold rounded-lg hover:bg-emerald-600 transition-colors shadow-md">Simpan Perkataan</button>
                                </div>
                            </div>
                            <!-- Senarai -->
                            <div class="space-y-4 text-left">
                                <h3 class="font-bold text-lg text-brand-darkSlate">Senarai Perkataan Aktif</h3>
                                <div class="overflow-y-auto max-h-[300px] border border-slate-100 rounded-lg divide-y divide-slate-100" id="admin-word-list"></div>
                            </div>
                        </div>
                    </div>

                    <!-- TAB 3: SUNTING TAHAP 3 -->
                    <div id="admin-tab-passages" class="hidden space-y-6">
                        <div class="grid grid-cols-1 md:grid-cols-12 gap-6 text-left">
                            <div class="md:col-span-5 space-y-4">
                                <h3 class="font-bold text-lg text-brand-darkSlate">Pilih & Edit Petikan</h3>
                                <div>
                                    <label class="block text-xs font-bold text-slate-500 uppercase mb-1">Pilih Petikan Untuk Disunting</label>
                                    <select id="admin-passage-select" onchange="loadPassageToEditor()" class="w-full px-3 py-2 rounded-lg border border-slate-200 text-sm">
                                        <option value="0">Petikan A</option>
                                        <option value="1">Petikan B</option>
                                        <option value="2">Petikan C</option>
                                    </select>
                                </div>
                                <div>
                                    <label class="block text-xs font-bold text-slate-500 uppercase mb-1">Tajuk Petikan</label>
                                    <input type="text" id="admin-passage-title" class="w-full px-3 py-2 rounded-lg border border-slate-200 text-sm font-semibold">
                                </div>
                                <div>
                                    <label class="block text-xs font-bold text-slate-500 uppercase mb-1">Teks Petikan Cerita</label>
                                    <textarea id="admin-passage-text" rows="5" class="w-full px-3 py-2 rounded-lg border border-slate-200 text-sm font-semibold leading-relaxed"></textarea>
                                </div>
                                <button onclick="generatePassageWordTokens()" class="w-full py-2.5 bg-indigo-600 text-white font-bold rounded-lg hover:bg-indigo-700 transition-all shadow-md text-xs sm:text-sm">
                                    Jana Skema Jawapan <i class="fa-solid fa-wand-magic-sparkles ml-1"></i>
                                </button>
                            </div>
                            
                            <div class="md:col-span-7 space-y-4 border-l border-slate-100 pl-6">
                                <h3 class="font-bold text-lg text-brand-darkSlate flex items-center gap-1.5">Skema Jawapan <span class="text-xs font-normal text-slate-400">(Tandakan jika ia kata berimbuhan)</span></h3>
                                <div class="overflow-y-auto max-h-[300px] border border-slate-200 rounded-xl p-4 bg-slate-50 space-y-3" id="admin-passage-tokens-list">
                                    <p class="text-xs text-slate-400 text-center py-8">Sila klik butang "Jana Skema Jawapan" setelah menaip teks petikan di sebelah kiri.</p>
                                </div>
                                <button onclick="saveActivePassage()" class="w-full py-3 bg-brand-mint text-white font-heading font-bold rounded-lg hover:bg-emerald-600 transition-colors shadow-md text-sm">
                                    Simpan Petikan & Skema Jawapan <i class="fa-solid fa-floppy-disk ml-1"></i>
                                </button>
                            </div>
                        </div>
                    </div>

                    <!-- TAB 4: SUNTING KUIZ FORMATIF -->
                    <div id="admin-tab-quiz" class="hidden space-y-6">
                        <div class="grid grid-cols-1 md:grid-cols-12 gap-6 text-left">
                            <div class="md:col-span-4 space-y-4">
                                <h3 class="font-bold text-lg text-brand-darkSlate">Pilih Soalan Kuiz</h3>
                                <div class="flex flex-col gap-2" id="admin-quiz-selector-list"></div>
                            </div>
                            <div class="md:col-span-8 space-y-4 border-l border-slate-100 pl-6">
                                <h3 class="font-bold text-lg text-brand-darkSlate">Butiran Soalan <span id="admin-quiz-active-q-label" class="text-brand-mint">1</span></h3>
                                <div class="space-y-3">
                                    <div>
                                        <label class="block text-xs font-bold text-slate-500 uppercase mb-1">Pernyataan Soalan</label>
                                        <input type="text" id="admin-quiz-q-text" class="w-full px-3 py-2 rounded-lg border border-slate-200 text-sm font-bold text-slate-700">
                                    </div>
                                    <div>
                                        <label class="block text-xs font-bold text-slate-500 uppercase mb-1">Objektif Soalan (Tag)</label>
                                        <input type="text" id="admin-quiz-q-objective" class="w-full px-3 py-2 rounded-lg border border-slate-200 text-sm font-semibold">
                                    </div>
                                    <div class="grid grid-cols-2 gap-3">
                                        <div>
                                            <label class="block text-xs font-bold text-slate-400 uppercase mb-1">Pilihan A</label>
                                            <input type="text" id="admin-quiz-opt-0" class="w-full px-3 py-2 rounded-lg border border-slate-200 text-sm font-semibold">
                                        </div>
                                        <div>
                                            <label class="block text-xs font-bold text-slate-400 uppercase mb-1">Pilihan B</label>
                                            <input type="text" id="admin-quiz-opt-1" class="w-full px-3 py-2 rounded-lg border border-slate-200 text-sm font-semibold">
                                        </div>
                                        <div>
                                            <label class="block text-xs font-bold text-slate-400 uppercase mb-1">Pilihan C</label>
                                            <input type="text" id="admin-quiz-opt-2" class="w-full px-3 py-2 rounded-lg border border-slate-200 text-sm font-semibold">
                                        </div>
                                        <div>
                                            <label class="block text-xs font-bold text-slate-400 uppercase mb-1">Pilihan D</label>
                                            <input type="text" id="admin-quiz-opt-3" class="w-full px-3 py-2 rounded-lg border border-slate-200 text-sm font-semibold">
                                        </div>
                                    </div>
                                    <div>
                                        <label class="block text-xs font-bold text-slate-500 uppercase mb-1">Jawapan Yang Betul</label>
                                        <select id="admin-quiz-correct-select" class="w-full px-3 py-2 rounded-lg border border-slate-200 text-sm font-bold text-emerald-600"></select>
                                    </div>
                                    <button onclick="saveActiveQuizQuestion()" class="w-full py-3 bg-brand-mint text-white font-heading font-bold rounded-lg hover:bg-emerald-600 transition-colors shadow-md text-sm">
                                        Simpan Soalan Kuiz <i class="fa-solid fa-floppy-disk ml-1"></i>
                                    </button>
                                </div>
                            </div>
                        </div>
                    </div>

                    <!-- TAB 5: URUS BAHAN MEDIA -->
                    <div id="admin-tab-media" class="hidden space-y-6">
                        <div class="grid grid-cols-1 md:grid-cols-2 gap-6">
                            <!-- Nota & Teknik -->
                            <div class="space-y-4 text-left">
                                <h3 class="font-bold text-lg text-brand-darkSlate">Nota & Teknik (Pilih Fail/URL)</h3>
                                
                                <div class="bg-slate-50 p-4 rounded-xl border border-slate-200 space-y-3">
                                    <span class="block text-xs font-bold uppercase text-slate-500">Imej Nota 1 (Jenis Imbuhan)</span>
                                    <input type="file" id="media-file-nota1" accept="image/*" onchange="handleMediaFileUpload('media-file-nota1', 'nota1', 'preview-admin-nota1')" class="block w-full text-xs text-slate-500 file:mr-4 file:py-2 file:px-4 file:rounded-md file:border-0 file:text-xs file:font-semibold file:bg-blue-50 file:text-blue-700 hover:file:bg-blue-100">
                                    <input type="text" id="media-url-nota1" placeholder="Atau pautan imej URL" class="w-full px-3 py-1.5 border border-slate-200 rounded-lg text-xs" onchange="saveMediaUrl('nota1', this.value, 'preview-admin-nota1')">
                                    <img id="preview-admin-nota1" class="h-20 object-contain rounded border bg-white" src="https://placehold.co/600x400/93c5fd/1e3a8a?text=Nota+Jenis+Imbuhan">
                                </div>

                                <div class="bg-slate-50 p-4 rounded-xl border border-slate-200 space-y-3">
                                    <span class="block text-xs font-bold uppercase text-slate-500">Imej Nota 2 (Morfologi Kata)</span>
                                    <input type="file" id="media-file-nota2" accept="image/*" onchange="handleMediaFileUpload('media-file-nota2', 'nota2', 'preview-admin-nota2')" class="block w-full text-xs text-slate-500 file:mr-4 file:py-2 file:px-4 file:rounded-md file:border-0 file:text-xs file:font-semibold file:bg-blue-50 file:text-blue-700 hover:file:bg-blue-100">
                                    <input type="text" id="media-url-nota2" placeholder="Atau pautan imej URL" class="w-full px-3 py-1.5 border border-slate-200 rounded-lg text-xs" onchange="saveMediaUrl('nota2', this.value, 'preview-admin-nota2')">
                                    <img id="preview-admin-nota2" class="h-20 object-contain rounded border bg-white" src="https://placehold.co/600x400/a7f3d0/064e3b?text=Nota+Morfologi+Kata">
                                </div>

                                <div class="bg-slate-50 p-4 rounded-xl border border-slate-200 space-y-3">
                                    <span class="block text-xs font-bold uppercase text-slate-500">Imej Teknik TarKiM (Panduan Langkah)</span>
                                    <input type="file" id="media-file-teknik1" accept="image/*" onchange="handleMediaFileUpload('media-file-teknik1', 'teknik1', 'preview-admin-teknik1')" class="block w-full text-xs text-slate-500 file:mr-4 file:py-2 file:px-4 file:rounded-md file:border-0 file:text-xs file:font-semibold file:bg-blue-50 file:text-blue-700 hover:file:bg-blue-100">
                                    <input type="text" id="media-url-teknik1" placeholder="Atau pautan imej URL" class="w-full px-3 py-1.5 border border-slate-200 rounded-lg text-xs" onchange="saveMediaUrl('teknik1', this.value, 'preview-admin-teknik1')">
                                    <img id="preview-admin-teknik1" class="h-20 object-contain rounded border bg-white" src="https://placehold.co/600x400/fbcfe8/831843?text=Panduan+Tolak+dan+Tarik">
                                </div>
                            </div>

                            <!-- Video Pembelajaran -->
                            <div class="space-y-4 text-left">
                                <h3 class="font-bold text-lg text-brand-darkSlate">Video Pembelajaran (YouTube)</h3>
                                <div class="bg-slate-50 p-4 rounded-xl border border-slate-200 space-y-3">
                                    <span class="block text-xs font-bold uppercase text-slate-500">Pautan Video YouTube</span>
                                    <input type="text" id="media-url-video" placeholder="Contoh: https://www.youtube.com/watch?v=dQw4w9WgXcQ" class="w-full px-3 py-2 border border-slate-200 rounded-lg text-sm" onchange="saveVideoLink(this.value)">
                                    <p class="text-[11px] text-slate-400 mt-1">Masukkan URL standard YouTube atau kongsi (youtu.be). Sistem akan menukarkannya kepada pautan embed secara automatik.</p>
                                </div>
                                <button onclick="resetMediaDefaults()" class="w-full py-2.5 bg-red-500 hover:bg-red-600 text-white font-heading font-bold rounded-lg transition-colors text-xs shadow-md">
                                    <i class="fa-solid fa-trash-can mr-1"></i> Padam Semua Media (Gunakan Placeholder Asal)
                                </button>
                            </div>
                        </div>
                    </div>

                    <!-- TAB 6: INTEGRASI GOOGLE SHEETS -->
                    <div id="admin-tab-tech" class="hidden space-y-4 text-left">
                        <h3 class="font-bold text-lg text-brand-darkSlate">Seni Bina Teknikal & Integrasi API</h3>
                        <p class="text-sm text-slate-500">Pindahkan markah murid secara langsung dari aplikasi web GitHub ke **Google Sheets** anda menerusi API **Google Apps Script**.</p>
                        
                        <div class="space-y-3">
                            <div>
                                <label class="block text-xs font-bold text-slate-500 uppercase mb-1">Web App URL Google Apps Script</label>
                                <input type="text" id="admin-api-url" placeholder="https://script.google.com/macros/s/.../exec" class="w-full px-4 py-2.5 rounded-lg border border-slate-200 text-sm">
                                <p class="text-[11px] text-slate-400 mt-1">Masukkan URL Web App dari Apps Script anda untuk integrasi pangkalan data yang stabil.</p>
                            </div>
                            
                            <div class="bg-slate-50 p-4 rounded-xl border border-slate-200 space-y-2">
                                <div class="flex justify-between items-center">
                                    <span class="text-xs font-bold text-slate-600">KOD GOOGLE APPS SCRIPT</span>
                                    <button onclick="copyAppsScriptCode()" class="text-xs text-brand-mint hover:underline font-bold">
                                        <i class="fa-solid fa-copy"></i> Salin Kod
                                    </button>
                                </div>
                                <textarea id="apps-script-textarea" readonly class="w-full h-36 bg-slate-900 text-emerald-400 font-mono text-xs p-3 rounded-lg border-none outline-none leading-relaxed">
function doPost(e) {
  var sheet = SpreadsheetApp.getActiveSpreadsheet().getActiveSheet();
  var params = JSON.parse(e.postData.contents);
  
  sheet.appendRow([
    new Date(),
    params.name,
    params.class,
    params.activity || "Kuiz Formatif",
    params.score,
    params.status
  ]);
  
  return ContentService.createTextOutput(JSON.stringify({"result": "success"}))
    .setMimeType(ContentService.MimeType.JSON);
}

function doGet() {
  return ContentService.createTextOutput(JSON.stringify({"result": "alive"}))
    .setMimeType(ContentService.MimeType.JSON);
}</textarea>
                            </div>
                        </div>
                    </div>

                </div>
            </div>

        </main>

        <!-- FOOTER UTAMA -->
        <footer class="w-full px-8 py-3 bg-white border-t border-slate-100 flex justify-between items-center z-10 text-xs text-slate-400 font-semibold">
            <div id="footer-stars-container" class="hidden flex items-center gap-1.5 text-amber-400 text-sm">
                <i class="fa-solid fa-star"></i>
                <span id="star-count" class="text-slate-600 text-xs font-bold ml-1">0.0 Bintang Diperoleh</span>
            </div>
            <div id="footer-watermark" class="ml-auto flex items-center gap-1">
                <span>Inovasi Pedagogi Cikgu Am</span>
                <i class="fa-solid fa-shield-heart text-brand-mint"></i>
            </div>
        </footer>

    </div>

    <!-- LOGIK JAVASCRIPT APLIKASI TARKIM -->
    <script>
        // === AUDIO SYNTHESIZER ===
        function playAudioFeedback(type) {
            try {
                const ctx = new (window.AudioContext || window.webkitAudioContext)();
                const osc = ctx.createOscillator();
                const gain = ctx.createGain();
                osc.connect(gain);
                gain.connect(ctx.destination);
                
                if (type === 'success') {
                    osc.frequency.setValueAtTime(523.25, ctx.currentTime); 
                    osc.frequency.setValueAtTime(659.25, ctx.currentTime + 0.1); 
                    osc.frequency.setValueAtTime(783.99, ctx.currentTime + 0.2); 
                    gain.gain.setValueAtTime(0.1, ctx.currentTime);
                    osc.start();
                    osc.stop(ctx.currentTime + 0.4);
                } else if (type === 'fail') {
                    osc.frequency.setValueAtTime(220, ctx.currentTime); 
                    osc.frequency.setValueAtTime(180, ctx.currentTime + 0.15); 
                    gain.gain.setValueAtTime(0.1, ctx.currentTime);
                    osc.start();
                    osc.stop(ctx.currentTime + 0.3);
                }
            } catch (e) {
                console.log("Audio contexts blocked or unsupported");
            }
        }

        // === STATE PERANTI & DATA ===
        let currentUser = { name: '', class: '' };
        let starsCollected = 0.0;
        let scoresDatabase = JSON.parse(localStorage.getItem('tarkim_scores_v3')) || [];

        // Database Bahan Media Tempatan
        let mediaDatabase = JSON.parse(localStorage.getItem('tarkim_media_v3')) || {
            nota1: 'interactive_template',
            nota2: 'interactive_template',
            teknik1: 'interactive_template',
            videoUrl: 'https://www.youtube.com/embed/dQw4w9WgXcQ'
        };

        // Senarai perkataan default Tahap 1 & 2
        let wordsDatabase = JSON.parse(localStorage.getItem('tarkim_words_v3')) || {
            1: [
                { id: 1, base: 'lukis', prefix: 'me-', suffix: '', full: 'melukis', type: 'Imbuhan Awalan' },
                { id: 2, base: 'kerja', prefix: 'be-', suffix: '', full: 'bekerja', type: 'Imbuhan Awalan' },
                { id: 3, base: 'jatuh', prefix: 'ter-', suffix: '', full: 'terjatuh', type: 'Imbuhan Awalan' },
                { id: 4, base: 'baca', prefix: '', suffix: '-an', full: 'bacaan', type: 'Imbuhan Akhiran' },
                { id: 5, base: 'kemas', prefix: '', suffix: '-kan', full: 'kemaskan', type: 'Imbuhan Akhiran' },
                { id: 6, base: 'tani', prefix: 'per-', suffix: '-an', full: 'pertanian', type: 'Imbuhan Apitan' },
                { id: 7, base: 'jalan', prefix: 'per-', suffix: '-an', full: 'perjalanan', type: 'Imbuhan Apitan' },
                { id: 8, base: 'turut', prefix: '', suffix: '-i', full: 'turuti', type: 'Imbuhan Akhiran' },
                { id: 9, base: 'baca', prefix: 'mem-', suffix: '', full: 'membaca', type: 'Imbuhan Awalan' },
                { id: 10, base: 'jalan', prefix: 'ber-', suffix: '', full: 'berjalan', type: 'Imbuhan Awalan' },
                { id: 11, base: 'lukis', prefix: '', suffix: '-an', full: 'lukisan', type: 'Imbuhan Akhiran' },
                { id: 12, base: 'jalan', prefix: '', suffix: '-an', full: 'jalanan', type: 'Imbuhan Akhiran' },
                { id: 13, base: 'kerja', prefix: 'pe-', suffix: '-an', full: 'pekerjaan', type: 'Imbuhan Apitan' },
                { id: 14, base: 'rupa', prefix: 'me-', suffix: '-kan', full: 'merupakan', type: 'Imbuhan Apitan' },
                { id: 15, base: 'baca', prefix: 'di-', suffix: '-kan', full: 'dibacakan', type: 'Imbuhan Apitan' },
                { id: 16, base: 'beritahu', prefix: 'mem-', suffix: '', full: 'memberitahu', type: 'Imbuhan Awalan' },
                { id: 17, base: 'surih', prefix: '', suffix: '-kan', full: 'surihkan', type: 'Imbuhan Akhiran' }
            ],
            2: [
                { id: 101, base: 'sambung', prefix: '-in-', suffix: '', full: 'sinambung', type: 'Imbuhan Sisipan' },
                { id: 102, base: 'serak', prefix: '-el-', suffix: '', full: 'selerak', type: 'Imbuhan Sisipan' },
                { id: 103, base: 'kuncup', prefix: '-em-', suffix: '', full: 'kemuncup', type: 'Imbuhan Sisipan' },
                { id: 104, base: 'sabut', prefix: '-er-', suffix: '', full: 'serabut', type: 'Imbuhan Sisipan' },
                { id: 105, base: 'serbak', prefix: '-em-', suffix: '', full: 'semerbak', type: 'Imbuhan Sisipan' },
                { id: 106, base: 'tapak', prefix: '-el-', suffix: '', full: 'telapak', type: 'Imbuhan Sisipan' },
                { id: 107, base: 'tulis', prefix: 'pe-', suffix: '-an', full: 'penulisan', type: 'Imbuhan Apitan' },
                { id: 108, base: 'sayang', prefix: 'me-', suffix: '-i', full: 'menyayangi', type: 'Imbuhan Apitan' },
                { id: 109, base: 'periksa', prefix: 'pe-', suffix: '-an', full: 'pemeriksaan', type: 'Imbuhan Apitan' },
                { id: 110, base: 'gilang', prefix: '-em-', suffix: '', full: 'gemilang', type: 'Imbuhan Sisipan' },
                { id: 111, base: 'suling', prefix: '-er-', suffix: '', full: 'seruling', type: 'Imbuhan Sisipan' },
                { id: 112, base: 'jujur', prefix: '-el-', suffix: '', full: 'jelujur', type: 'Imbuhan Sisipan' },
                { id: 113, base: 'kemas', prefix: 'me-', suffix: '', full: 'mengemas', type: 'Imbuhan Awalan' },
                { id: 114, base: 'kawal', prefix: 'me-', suffix: '', full: 'mengawal', type: 'Imbuhan Awalan' },
                { id: 115, base: 'kira', prefix: 'me-', suffix: '', full: 'mengira', type: 'Imbuhan Awalan' },
                { id: 116, base: 'tutup', prefix: 'pe-', suffix: '', full: 'penutup', type: 'Imbuhan Awalan' },
                { id: 117, base: 'kunjung', prefix: 'me-', suffix: '-i', full: 'mengunjungi', type: 'Imbuhan Apitan' }
            ]
        };

        // Senarai petikan Tahap 3 (Editable CMS)
        let passagesDatabase = JSON.parse(localStorage.getItem('tarkim_passages_v3')) || [
            {
                title: "Petikan A: Kebun Pak Cik",
                text: "Amirul melawat kawasan pertanian pak ciknya. Di sana, dia melihat pelbagai tanaman yang segar. Pekerjaan pak ciknya sangat menyeronokkan kerana dapat menghasilkan sayur-sayuran untuk perjalanan jualan ke bandar.",
                answers: [
                    { word: "melawat", type: "Imbuhan Awalan" },
                    { word: "pertanian", type: "Imbuhan Apitan" },
                    { word: "tanaman", type: "Imbuhan Akhiran" },
                    { word: "Pekerjaan", type: "Imbuhan Apitan" },
                    { word: "menyeronokkan", type: "Imbuhan Apitan" },
                    { word: "menghasilkan", type: "Imbuhan Apitan" },
                    { word: "perjalanan", type: "Imbuhan Apitan" },
                    { word: "jualan", type: "Imbuhan Akhiran" }
                ]
            },
            {
                title: "Petikan B: Di Perpustakaan",
                text: "Setiap pagi, kami membaca buku di perpustakaan. Guru menyuruh kami melakukan tugasan dengan kemas. Lukisan yang digantung pada dinding kelas amat berkilauan apabila terkena pancaran matahari.",
                answers: [
                    { word: "membaca", type: "Imbuhan Awalan" },
                    { word: "perpustakaan", type: "Imbuhan Apitan" },
                    { word: "menyuruh", type: "Imbuhan Awalan" },
                    { word: "melakukan", type: "Imbuhan Apitan" },
                    { word: "tugasan", type: "Imbuhan Akhiran" },
                    { word: "Lukisan", type: "Imbuhan Akhiran" },
                    { word: "digantung", type: "Imbuhan Awalan" },
                    { word: "berkilauan", type: "Imbuhan Apitan" },
                    { word: "pancaran", type: "Imbuhan Akhiran" }
                ]
            },
            {
                title: "Petikan C: Keindahan Alam",
                text: "Bunyi seruling bertiup lembut dari arah telapak kaki bukit. Hati menjadi tenang melihat selerak dedaun kering ditiup angin. Pengalaman ini amat gemilang dan sukar dilupakan.",
                answers: [
                    { word: "seruling", type: "Imbuhan Sisipan" },
                    { word: "bertiup", type: "Imbuhan Awalan" },
                    { word: "telapak", type: "Imbuhan Sisipan" },
                    { word: "melihat", type: "Imbuhan Awalan" },
                    { word: "selerak", type: "Imbuhan Sisipan" },
                    { word: "ditiup", type: "Imbuhan Awalan" },
                    { word: "Pengalaman", type: "Imbuhan Apitan" },
                    { word: "gemilang", type: "Imbuhan Sisipan" },
                    { word: "dilupakan", type: "Imbuhan Apitan" }
                ]
            }
        ];

        // Database Kuiz Formatif (Editable CMS)
        let quizDatabase = JSON.parse(localStorage.getItem('tarkim_quiz_v3')) || [
            {
                q: "Antara perkataan berikut, yang manakah mengandungi IMBUHAN AWALAN?",
                options: ["bacaan", "melukis", "perjalanan", "telapak"],
                correct: "melukis",
                objective: "Kategorikan Kata"
            },
            {
                q: "Kenal pasti perkataan yang menggunakan IMBUHAN SISIPAN.",
                options: ["kemaskan", "selerak", "terjatuh", "perjalanan"],
                correct: "selerak",
                objective: "Kategorikan Kata"
            },
            {
                q: "Apakah jenis imbuhan yang digunakan untuk kata 'pemeriksaan'?",
                options: ["Awalan", "Akhiran", "Apitan", "Sisipan"],
                correct: "Apitan",
                objective: "Jenis Imbuhan"
            },
            {
                q: "Apakah kata dasar asal bagi perkataan 'menyayangi'?",
                options: ["nyanyi", "sayang", "ayang", "syg"],
                correct: "sayang",
                objective: "Kata Dasar"
            },
            {
                q: "Cari kata dasar yang betul bagi perkataan 'pekerjaan'.",
                options: ["kerja", "kerjaya", "kejar", "raja"],
                correct: "kerja",
                objective: "Kata Dasar"
            },
            {
                q: "Apakah kata dasar asal bagi kata bersisip 'telapak'?",
                options: ["tapak", "lapak", "telap", "pak"],
                correct: "tapak",
                objective: "Kata Dasar"
            }
        ];

        let googleSheetsApiUrl = localStorage.getItem('tarkim_api_url') || '';

        // === SCREEN CONTROL SYSTEM ===
        function showScreen(screenId) {
            document.getElementById('screen-login').classList.add('hidden');
            document.getElementById('screen-menu').classList.add('hidden');
            document.getElementById('section-nota').classList.add('hidden');
            document.getElementById('section-teknik').classList.add('hidden');
            document.getElementById('section-video').classList.add('hidden');
            document.getElementById('section-permainan').classList.add('hidden');
            document.getElementById('section-kuiz').classList.add('hidden');
            document.getElementById('section-admin').classList.add('hidden');

            document.getElementById(screenId).classList.remove('hidden');
        }

        // LOAD & RENDER MEDIA ENGINE
        function applyMediaSettings() {
            document.getElementById('video-frame').src = formatYoutubeEmbed(mediaDatabase.videoUrl);

            // Update UI Previews di Panel Admin
            document.getElementById('preview-admin-nota1').src = mediaDatabase.nota1 === 'interactive_template' ? 'https://placehold.co/600x400/93c5fd/1e3a8a?text=Nota+Digital+P,T,K,S' : mediaDatabase.nota1;
            document.getElementById('preview-admin-nota2').src = mediaDatabase.nota2 === 'interactive_template' ? 'https://placehold.co/600x400/a7f3d0/064e3b?text=Nota+Digital+Track+Field' : mediaDatabase.nota2;
            document.getElementById('preview-admin-teknik1').src = mediaDatabase.teknik1 === 'interactive_template' ? 'https://placehold.co/600x400/fbcfe8/831843?text=Teknik+Digital+Track+Field' : mediaDatabase.teknik1;
            
            document.getElementById('media-url-nota1').value = mediaDatabase.nota1 === 'interactive_template' ? '' : mediaDatabase.nota1;
            document.getElementById('media-url-nota2').value = mediaDatabase.nota2 === 'interactive_template' ? '' : mediaDatabase.nota2;
            document.getElementById('media-url-teknik1').value = mediaDatabase.teknik1 === 'interactive_template' ? '' : mediaDatabase.teknik1;
            document.getElementById('media-url-video').value = mediaDatabase.videoUrl;

            renderMediaContent();
        }

        function renderMediaContent() {
            // RENDERING NOTA 1
            const containerNota1 = document.getElementById('container-nota-1');
            if (mediaDatabase.nota1 === 'interactive_template' || !mediaDatabase.nota1) {
                containerNota1.innerHTML = `
                    <div class="w-full bg-white p-4 sm:p-6 rounded-xl shadow-inner border border-slate-200 text-left h-full flex flex-col justify-between overflow-y-auto">
                        <div class="text-center mb-4 bg-amber-50 border border-amber-200 p-2.5 rounded-lg">
                            <span class="text-xs sm:text-sm font-bold text-slate-800">
                                Jika bertemu kata dasar yang bermula huruf <span class="text-red-600 font-extrabold text-base">P</span>, <span class="text-red-600 font-extrabold text-base">T</span>, <span class="text-red-600 font-extrabold text-base">K</span>, dan <span class="text-red-600 font-extrabold text-base">S</span>, ia akan melebur:
                            </span>
                        </div>
                        <div class="grid grid-cols-4 gap-2 mb-4 items-center">
                            <div class="flex flex-col items-center">
                                <div class="w-12 h-12 rounded-full bg-[#C2A385] border-2 border-slate-800 flex flex-col items-center justify-center shadow-md">
                                    <span class="text-xs font-bold text-slate-900"><span class="text-red-600 text-sm font-extrabold">P</span>ak</span>
                                </div>
                                <i class="fa-solid fa-arrow-down text-slate-400 my-1 text-sm"></i>
                                <div class="w-12 h-12 rounded-full bg-[#A5F3FC] border-2 border-slate-800 flex flex-col items-center justify-center shadow-md">
                                    <span class="text-xs font-bold text-slate-900"><span class="text-red-600 text-sm font-extrabold">M</span>ak</span>
                                </div>
                            </div>
                            <div class="flex flex-col items-center">
                                <div class="w-12 h-12 rounded-full bg-[#C2A385] border-2 border-slate-800 flex flex-col items-center justify-center shadow-md">
                                    <span class="text-xs font-bold text-slate-900"><span class="text-red-600 text-sm font-extrabold">T</span>am</span>
                                </div>
                                <i class="fa-solid fa-arrow-down text-slate-400 my-1 text-sm"></i>
                                <div class="w-12 h-12 rounded-full bg-[#A5F3FC] border-2 border-slate-800 flex flex-col items-center justify-center shadow-md">
                                    <span class="text-xs font-bold text-slate-900"><span class="text-red-600 text-sm font-extrabold">N</span>ab</span>
                                </div>
                            </div>
                            <div class="flex flex-col items-center">
                                <div class="w-12 h-12 rounded-full bg-[#C2A385] border-2 border-slate-800 flex flex-col items-center justify-center shadow-md">
                                    <span class="text-xs font-bold text-slate-900"><span class="text-red-600 text-sm font-extrabold">K</span>e</span>
                                </div>
                                <i class="fa-solid fa-arrow-down text-slate-400 my-1 text-sm"></i>
                                <div class="w-12 h-12 rounded-full bg-[#A5F3FC] border-2 border-slate-800 flex flex-col items-center justify-center shadow-md">
                                    <span class="text-xs font-bold text-slate-900"><span class="text-red-600 text-sm font-extrabold">Ng</span>anga</span>
                                </div>
                            </div>
                            <div class="flex flex-col items-center">
                                <div class="w-12 h-12 rounded-full bg-[#C2A385] border-2 border-slate-800 flex flex-col items-center justify-center shadow-md">
                                    <span class="text-xs font-bold text-slate-900"><span class="text-red-600 text-sm font-extrabold">S</span>urau</span>
                                </div>
                                <i class="fa-solid fa-arrow-down text-slate-400 my-1 text-sm"></i>
                                <div class="w-12 h-12 rounded-full bg-[#A5F3FC] border-2 border-slate-800 flex flex-col items-center justify-center shadow-md">
                                    <span class="text-xs font-bold text-slate-900"><span class="text-red-600 text-sm font-extrabold">Ny</span>anyi</span>
                                </div>
                            </div>
                        </div>
                        <div class="border-2 border-slate-800 p-3 rounded-lg bg-white shadow-sm font-body">
                            <span class="block font-heading font-bold text-xs uppercase text-slate-400 mb-1">Contoh Peleburan:</span>
                            <div class="grid grid-cols-2 gap-x-4 gap-y-1 text-xs sm:text-sm text-slate-700 font-semibold">
                                <div class="flex justify-between border-b border-slate-100 py-1">
                                    <span><span class="text-red-600 font-bold">p</span>erintah</span>
                                    <span>➔ me<span class="text-red-600 font-bold">m</span>erintah</span>
                                </div>
                                <div class="flex justify-between border-b border-slate-100 py-1">
                                    <span><span class="text-red-600 font-bold">t</span>ulis</span>
                                    <span>➔ me<span class="text-red-600 font-bold">n</span>ulis</span>
                                </div>
                                <div class="flex justify-between border-b border-slate-100 py-1">
                                    <span><span class="text-red-600 font-bold">k</span>ira</span>
                                    <span>➔ me<span class="text-red-600 font-bold">ng</span>ira</span>
                                </div>
                                <div class="flex justify-between border-b border-slate-100 py-1">
                                    <span><span class="text-red-600 font-bold">s</span>entuh</span>
                                    <span>➔ me<span class="text-red-600 font-bold">ny</span>entuh</span>
                                </div>
                            </div>
                        </div>
                    </div>
                `;
            } else {
                containerNota1.innerHTML = `<img src="${mediaDatabase.nota1}" alt="Nota 1" class="w-full h-full object-contain">`;
            }

            // RENDERING NOTA 2
            const containerNota2 = document.getElementById('container-nota-2');
            if (mediaDatabase.nota2 === 'interactive_template' || !mediaDatabase.nota2) {
                containerNota2.innerHTML = `
                    <div class="w-full bg-white p-4 sm:p-5 rounded-xl shadow-inner border border-slate-200 text-left h-full flex flex-col justify-between overflow-y-auto">
                        <div class="text-center mb-3 bg-indigo-50 border border-indigo-200 py-1 rounded-lg">
                            <span class="text-xs sm:text-sm font-bold text-indigo-900">Teknik Larian Kedudukan Imbuhan</span>
                        </div>
                        <div class="space-y-3">
                            <div class="space-y-1">
                                <div class="flex justify-between text-[11px] font-bold text-slate-500">
                                    <span>Imbuhan Awalan - Depan</span>
                                    <span class="text-emerald-600">me-, ber-, ter-, di-</span>
                                </div>
                                <div class="track-field h-7 rounded-md overflow-hidden shadow-inner">
                                    <div class="track-line"></div>
                                    <div class="absolute left-4 top-1/2 -translate-y-1/2 text-sm z-10">🏃‍♂️💨</div>
                                </div>
                            </div>
                            <div class="space-y-1">
                                <div class="flex justify-between text-[11px] font-bold text-slate-500">
                                    <span>Imbuhan Akhiran - Belakang</span>
                                    <span class="text-pink-600">-an, -i, -kan</span>
                                </div>
                                <div class="track-field h-7 rounded-md overflow-hidden shadow-inner">
                                    <div class="track-line"></div>
                                    <div class="absolute right-4 top-1/2 -translate-y-1/2 text-sm z-10">🏃‍♂️💨</div>
                                </div>
                            </div>
                            <div class="space-y-1">
                                <div class="flex justify-between text-[11px] font-bold text-slate-500">
                                    <span>Imbuhan Apitan - Depan & Belakang</span>
                                    <span class="text-purple-600">pe-...-an, me-...-kan</span>
                                </div>
                                <div class="track-field h-7 rounded-md overflow-hidden shadow-inner">
                                    <div class="track-line"></div>
                                    <div class="absolute left-4 top-1/2 -translate-y-1/2 text-sm z-10">🏃‍♂️</div>
                                    <div class="absolute right-4 top-1/2 -translate-y-1/2 text-sm z-10">🏃‍♂️</div>
                                </div>
                            </div>
                            <div class="space-y-1">
                                <div class="flex justify-between text-[11px] font-bold text-slate-500">
                                    <span>Imbuhan Sisipan - Tengah</span>
                                    <span class="text-amber-600">-el-, -er-, -em-, -in-</span>
                                </div>
                                <div class="track-field h-7 rounded-md overflow-hidden shadow-inner">
                                    <div class="track-line"></div>
                                    <div class="absolute left-1/2 -translate-x-1/2 top-1/2 -translate-y-1/2 text-sm z-10">🏃‍♂️</div>
                                </div>
                            </div>
                        </div>
                    </div>
                `;
            } else {
                containerNota2.innerHTML = `<img src="${mediaDatabase.nota2}" alt="Nota 2" class="w-full h-full object-contain">`;
            }

            // RENDERING TEKNIK 1
            const containerTeknik1 = document.getElementById('container-teknik-1');
            if (mediaDatabase.teknik1 === 'interactive_template' || !mediaDatabase.teknik1) {
                containerTeknik1.innerHTML = `
                    <div class="w-full bg-gradient-to-br from-red-50 to-orange-50 p-6 rounded-2xl border border-rose-100 shadow-inner overflow-y-auto h-full text-left flex flex-col justify-between">
                        <div class="text-center bg-rose-600 text-white py-2 rounded-xl shadow-md mb-4">
                            <span class="font-heading font-bold text-base sm:text-lg flex justify-center items-center gap-2">
                                <i class="fa-solid fa-flag-checkered animate-pulse"></i>
                                Simulasi Track & Field Imbuhan
                            </span>
                        </div>
                        <div class="space-y-4">
                            <div class="bg-white p-3 rounded-xl border border-rose-100 shadow-sm">
                                <div class="flex justify-between items-center text-xs font-bold mb-1">
                                    <span class="text-rose-700 uppercase">Imbuhan Awalan (Depan)</span>
                                    <span class="bg-rose-100 text-rose-800 px-2 py-0.5 rounded">me- , ber- , ter-</span>
                                </div>
                                <div class="track-field h-8 rounded-lg overflow-hidden">
                                    <div class="track-line"></div>
                                    <span class="absolute left-4 top-1/2 -translate-y-1/2 text-sm z-10">🏃‍♂️💨</span>
                                </div>
                            </div>
                            <div class="bg-white p-3 rounded-xl border border-rose-100 shadow-sm">
                                <div class="flex justify-between items-center text-xs font-bold mb-1">
                                    <span class="text-blue-700 uppercase">Imbuhan Akhiran (Belakang)</span>
                                    <span class="bg-blue-100 text-blue-800 px-2 py-0.5 rounded">-an , -i , -kan</span>
                                </div>
                                <div class="track-field h-8 rounded-lg overflow-hidden">
                                    <div class="track-line"></div>
                                    <span class="absolute right-4 top-1/2 -translate-y-1/2 text-sm z-10">🏃‍♂️💨</span>
                                </div>
                            </div>
                            <div class="bg-white p-3 rounded-xl border border-rose-100 shadow-sm">
                                <div class="flex justify-between items-center text-xs font-bold mb-1">
                                    <span class="text-purple-700 uppercase">Imbuhan Apitan (Kedua Belah)</span>
                                    <span class="bg-purple-100 text-purple-800 px-2 py-0.5 rounded">pe-...-an , me-...-kan</span>
                                </div>
                                <div class="track-field h-8 rounded-lg overflow-hidden">
                                    <div class="track-line"></div>
                                    <span class="absolute left-4 top-1/2 -translate-y-1/2 text-sm z-10">🏃‍♂️</span>
                                    <span class="absolute right-4 top-1/2 -translate-y-1/2 text-sm z-10">🏃‍♂️</span>
                                </div>
                            </div>
                            <div class="bg-white p-3 rounded-xl border border-rose-100 shadow-sm">
                                <div class="flex justify-between items-center text-xs font-bold mb-1">
                                    <span class="text-amber-700 uppercase">Imbuhan Sisipan (Tengah)</span>
                                    <span class="bg-amber-100 text-amber-800 px-2 py-0.5 rounded">-el- , -er- , -em-</span>
                                </div>
                                <div class="track-field h-8 rounded-lg overflow-hidden">
                                    <div class="track-line"></div>
                                    <span class="absolute left-1/2 -translate-x-1/2 top-1/2 -translate-y-1/2 text-sm z-10">🏃‍♂️</span>
                                </div>
                            </div>
                        </div>
                    </div>
                `;
            } else {
                containerTeknik1.innerHTML = `<img src="${mediaDatabase.teknik1}" alt="Teknik 1" class="w-full h-full object-contain">`;
            }
        }

        function formatYoutubeEmbed(url) {
            if (!url) return 'https://www.youtube.com/embed/dQw4w9WgXcQ';
            if (url.includes('embed')) return url;
            let videoId = '';
            if (url.includes('youtube.com/watch?v=')) {
                videoId = url.split('v=')[1].split('&')[0];
            } else if (url.includes('youtu.be/')) {
                videoId = url.split('youtu.be/')[1].split('?')[0];
            }
            if (videoId) {
                return `https://www.youtube.com/embed/${videoId}`;
            }
            return url;
        }

        window.onload = function() {
            applyMediaSettings();
        };

        function handleLogin() {
            const name = document.getElementById('input-name').value.trim();
            const selectedClass = document.getElementById('input-class').value.trim();

            if (!name || !selectedClass) {
                Swal.fire({
                    title: 'Opps!',
                    text: 'Sila masukkan nama dan kelas anda terlebih dahulu.',
                    icon: 'warning',
                    confirmButtonColor: '#10B981'
                });
                playAudioFeedback('fail');
                return;
            }

            currentUser = { name: name, class: selectedClass };
            document.getElementById('header-user-info').innerText = `${name} (${selectedClass})`;
            document.getElementById('app-header').classList.remove('hidden');
            document.getElementById('footer-stars-container').classList.remove('hidden');
            updateStarsDisplay();

            playAudioFeedback('success');
            showScreen('screen-menu');
        }

        function logout() {
            currentUser = { name: '', class: '' };
            document.getElementById('app-header').classList.add('hidden');
            document.getElementById('footer-stars-container').classList.add('hidden');
            document.getElementById('input-name').value = '';
            document.getElementById('input-class').value = '';
            showScreen('screen-login');
        }

        function showSection(section) {
            if (section === 'nota') {
                showScreen('section-nota');
            } else if (section === 'teknik') {
                showScreen('section-teknik');
            } else if (section === 'video') {
                showScreen('section-video');
            } else if (section === 'permainan') {
                showScreen('section-permainan');
                document.getElementById('permainan-main-header').classList.remove('hidden');
                document.getElementById('level-selection').classList.remove('hidden');
                document.getElementById('gameplay-area').classList.add('hidden');
                document.getElementById('gameplay-area-level3').classList.add('hidden');
            } else if (section === 'kuiz') {
                startQuiz();
            }
        }

        function goBackToMenu() {
            showScreen('screen-menu');
        }

        function exitGameplayToMenu() {
            document.getElementById('gameplay-area').classList.add('hidden');
            document.getElementById('gameplay-area-level3').classList.add('hidden');
            document.getElementById('level-selection').classList.remove('hidden');
            document.getElementById('permainan-main-header').classList.remove('hidden');
        }

        function updateStarsDisplay() {
            document.getElementById('star-count').innerText = `${starsCollected.toFixed(1)} Bintang Diperoleh`;
        }

        // === GAMEPLAY TAHAP 1 & 2 ===
        let activeLevel = 1;
        let activeWordIndex = 0; 
        let totalRounds = 5;     
        let currentTargetWord = null; 
        let selectedCategory = '';
        let currentLevelWordsList = [];
        let usedWordsThisSession = [];
        let levelScoreTracker = 0;

        function startLevel(level) {
            document.getElementById('permainan-main-header').classList.add('hidden');

            if (level === 3) {
                document.getElementById('level-selection').classList.add('hidden');
                document.getElementById('gameplay-area-level3').classList.remove('hidden');
                startLevel3();
                return;
            }

            activeLevel = level;
            document.getElementById('active-level-label').innerText = `Tahap ${level} (${level === 1 ? 'Aras Rendah' : 'Aras Sederhana'})`;
            document.getElementById('level-selection').classList.add('hidden');
            document.getElementById('gameplay-area').classList.remove('hidden');

            activeWordIndex = 0;
            levelScoreTracker = 0;
            usedWordsThisSession = [];
            currentLevelWordsList = [...wordsDatabase[level]];

            initGameplayRound();
        }

        function initGameplayRound() {
            if (activeWordIndex >= totalRounds || activeWordIndex >= currentLevelWordsList.length) {
                const effectiveRounds = Math.min(totalRounds, currentLevelWordsList.length);
                const percentage = effectiveRounds > 0 ? (levelScoreTracker / effectiveRounds) * 100 : 0;
                let status = percentage >= 80 ? 'Sangat Cemerlang!' : percentage >= 50 ? 'Bimbingan Lanjutan Diperlukan' : 'Bimbingan Asas Diperlukan';

                const timestamp = new Date().toLocaleString('ms-MY');
                const scoreRecord = {
                    name: currentUser.name,
                    class: currentUser.class,
                    activity: `Permainan Tahap ${activeLevel}`,
                    score: `${levelScoreTracker} / ${effectiveRounds}`,
                    status: status,
                    time: timestamp
                };

                scoresDatabase.push(scoreRecord);
                localStorage.setItem('tarkim_scores_v3', JSON.stringify(scoresDatabase));

                if (googleSheetsApiUrl) {
                    sendScoreToGoogleSheets(scoreRecord);
                }

                starsCollected += parseFloat(levelScoreTracker);
                updateStarsDisplay();

                playAudioFeedback('success');
                Swal.fire({
                    title: 'Sesi Selesai!',
                    html: `<p class="text-lg">Skor Sesi Ini: <strong class="text-brand-mint text-2xl">${levelScoreTracker} / ${effectiveRounds}</strong></p><p class="text-sm text-slate-500 mt-2">${status}</p>`,
                    icon: 'success',
                    confirmButtonColor: '#10B981',
                    confirmButtonText: 'Kembali'
                }).then(() => {
                    showSection('permainan');
                });
                return;
            }

            const effectiveRounds = Math.min(totalRounds, currentLevelWordsList.length);
            document.getElementById('game-step-counter').innerText = `Latihan ${activeWordIndex + 1} daripada ${effectiveRounds}`;
            document.getElementById('game-progress-bar').style.width = `${((activeWordIndex + 1) / effectiveRounds) * 100}%`;

            resetGameplayInput();
            renderWordPool();
        }

        function renderWordPool() {
            const poolContainer = document.getElementById('draggable-options-pool');
            poolContainer.innerHTML = '';

            if (currentLevelWordsList.length === 0) {
                poolContainer.innerHTML = `<p class="text-xs text-slate-400 py-4">Tiada perkataan berdaftar untuk tahap ini. Sila hubungi Admin.</p>`;
                return;
            }

            currentLevelWordsList.forEach(wordObj => {
                const isUsed = usedWordsThisSession.includes(wordObj.full);
                const btn = document.createElement('div');
                
                if (isUsed) {
                    btn.className = "px-5 py-2.5 bg-slate-200 border-2 border-slate-300 font-bold text-slate-400 text-lg rounded-xl opacity-50 cursor-not-allowed select-none";
                    btn.innerText = wordObj.full;
                } else {
                    btn.className = "draggable-item px-5 py-2.5 bg-white border-2 border-slate-200 hover:border-emerald-400 font-bold text-slate-700 text-lg rounded-xl shadow-sm hover:shadow-md transition-all";
                    btn.setAttribute("draggable", "true");
                    btn.innerText = wordObj.full;
                    btn.id = `drag-${wordObj.full}`;
                    
                    btn.ondragstart = (e) => {
                        e.dataTransfer.setData("text/plain", wordObj.full);
                    };
                    btn.onclick = () => selectWordDirectly(wordObj.full);
                }
                poolContainer.appendChild(btn);
            });

            document.getElementById('game-instruction-text').innerText = "Ketik atau seret mana-mana perkataan bebas dari kolam bawah ke dalam kotak hijau.";
        }

        function allowDrop(e) {
            e.preventDefault();
            document.getElementById('drag-target-zone').classList.add('drag-over');
        }

        function dragLeave(e) {
            document.getElementById('drag-target-zone').classList.remove('drag-over');
        }

        function handleDrop(e) {
            e.preventDefault();
            const zone = document.getElementById('drag-target-zone');
            zone.classList.remove('drag-over');
            const word = e.dataTransfer.getData("text/plain");
            if(word) {
                selectWordDirectly(word);
            }
        }

        function selectWordDirectly(word) {
            const wordObj = currentLevelWordsList.find(w => w.full.toLowerCase() === word.toLowerCase());
            if (!wordObj) return;

            currentTargetWord = wordObj;

            document.getElementById('drag-placeholder-text').classList.add('hidden');
            const activeZone = document.getElementById('active-dragged-word');
            activeZone.innerText = word;
            activeZone.classList.remove('hidden');

            document.getElementById('game-instruction-text').innerText = "Taipkan pemisahan kata (Kiri, Tengah, Kanan) dan pilih kategori imbuhan dengan tepat!";
            playAudioFeedback('success');
        }

        function selectAffixCategory(cat) {
            selectedCategory = cat;
            const categories = ['Imbuhan Awalan', 'Imbuhan Akhiran', 'Imbuhan Apitan', 'Imbuhan Sisipan'];
            const buttonIds = {
                'Imbuhan Awalan': 'btn-cat-awalan',
                'Imbuhan Akhiran': 'btn-cat-akhiran',
                'Imbuhan Apitan': 'btn-cat-apitan',
                'Imbuhan Sisipan': 'btn-cat-sisipan'
            };

            categories.forEach(c => {
                const btn = document.getElementById(buttonIds[c]);
                if (c === cat) {
                    btn.className = "py-3 px-2 bg-emerald-500 border-2 border-brand-mint text-white font-bold rounded-xl transition-all text-sm shadow-sm bounce-btn";
                } else {
                    btn.className = "py-3 px-2 bg-white border-2 border-slate-200 text-slate-700 font-bold rounded-xl hover:bg-emerald-50 hover:border-brand-mint transition-all text-sm shadow-sm bounce-btn";
                }
            });
        }

        function resetGameplayInput() {
            document.getElementById('input-imbuhan-kiri').value = '';
            document.getElementById('input-kata-dasar-tengah').value = '';
            document.getElementById('input-imbuhan-kanan').value = '';
            
            selectedCategory = '';
            currentTargetWord = null;

            const categories = ['btn-cat-awalan', 'btn-cat-akhiran', 'btn-cat-apitan', 'btn-cat-sisipan'];
            categories.forEach(id => {
                document.getElementById(id).className = "py-3 px-2 bg-white border-2 border-slate-200 text-slate-700 font-bold rounded-xl hover:bg-emerald-50 hover:border-brand-mint transition-all text-sm shadow-sm bounce-btn";
            });

            document.getElementById('drag-placeholder-text').classList.remove('hidden');
            document.getElementById('active-dragged-word').classList.add('hidden');
            document.getElementById('active-dragged-word').innerText = '';
        }

        function verifyDeconstruction() {
            if (!currentTargetWord) {
                Swal.fire('Opps!', 'Sila pilih dan letakkan perkataan dari kolam di bawah terlebih dahulu.', 'warning');
                playAudioFeedback('fail');
                return;
            }

            const inputKiri = document.getElementById('input-imbuhan-kiri').value.trim().toLowerCase().replace(/-/g, '');
            const inputTengah = document.getElementById('input-kata-dasar-tengah').value.trim().toLowerCase();
            const inputKanan = document.getElementById('input-imbuhan-kanan').value.trim().toLowerCase().replace(/-/g, '');

            const correctKiri = currentTargetWord.prefix.toLowerCase().replace(/-/g, '').trim();
            const correctTengah = currentTargetWord.base.toLowerCase().trim();
            const correctKanan = currentTargetWord.suffix.toLowerCase().replace(/-/g, '').trim();

            const isKiriMatch = (inputKiri === correctKiri) || 
                                (currentTargetWord.full.startsWith('me') && (inputKiri === 'me' || inputKiri === 'men' || inputKiri === 'mem' || inputKiri === 'meng' || inputKiri === 'meny')) ||
                                (currentTargetWord.full.startsWith('pe') && (inputKiri === 'pe' || inputKiri === 'pen' || inputKiri === 'pem' || inputKiri === 'peng' || inputKiri === 'peny'));
            
            const isTengahMatch = (inputTengah === correctTengah);
            const isKananMatch = (inputKanan === correctKanan);
            const isCategoryMatch = (selectedCategory === currentTargetWord.type);

            const isCorrect = isKiriMatch && isTengahMatch && isKananMatch && isCategoryMatch;

            usedWordsThisSession.push(currentTargetWord.full);

            if (isCorrect) {
                levelScoreTracker++;
                playAudioFeedback('success');
                Swal.fire({
                    title: 'Syabas! Betul!',
                    text: `Pecahan perkataan bagi "${currentTargetWord.full}" adalah tepat!`,
                    icon: 'success',
                    timer: 2000,
                    showConfirmButton: true,
                    confirmButtonText: 'Teruskan'
                }).then(() => {
                    activeWordIndex++;
                    initGameplayRound();
                });
            } else {
                playAudioFeedback('fail');
                let explanation = `Pembongkaran yang betul bagi "${currentTargetWord.full}" ialah:\n\n` +
                                  `• Kiri (Awalan/Sisipan): "${currentTargetWord.prefix || 'tiada'}"\n` +
                                  `• Tengah (Kata Dasar): "${currentTargetWord.base}"\n` +
                                  `• Kanan (Akhiran): "${currentTargetWord.suffix || 'tiada'}"\n` +
                                  `• Kategori Imbuhan: "${currentTargetWord.type}"`;

                Swal.fire({
                    title: 'Ulasan Jawapan',
                    html: `<p class="text-sm text-left whitespace-pre-line bg-slate-50 p-4 rounded-xl border border-slate-200">${explanation}</p><p class="text-xs text-red-500 mt-3 text-center font-bold">Keputusan disimpan! Mari cuba perkataan lain.</p>`,
                    icon: 'info',
                    showConfirmButton: true,
                    confirmButtonText: 'Seterusnya'
                }).then(() => {
                    activeWordIndex++;
                    initGameplayRound();
                });
            }
        }

        // === GAMEPLAY TAHAP 3 ===
        let activePassageIndex = 0; 
        let selectedL3Words = []; 
        let isClassificationMode = false; 

        function startLevel3() {
            activePassageIndex = 0;
            renderPassageNavulators();
            loadLevel3Passage();
        }

        function renderPassageNavulators() {
            const nav = document.getElementById('passage-navigator-buttons');
            nav.innerHTML = '';
            passagesDatabase.forEach((pas, idx) => {
                const btn = document.createElement('button');
                btn.className = `px-3 py-1.5 rounded-lg text-xs font-bold transition-all ${idx === activePassageIndex ? 'bg-indigo-600 text-white' : 'bg-slate-200 text-slate-700'}`;
                btn.innerText = pas.title.split(':')[0] || `Petikan ${idx+1}`;
                btn.onclick = () => switchLevel3Passage(idx + 1);
                nav.appendChild(btn);
            });
        }

        function switchLevel3Passage(pNum) {
            activePassageIndex = pNum - 1;
            renderPassageNavulators();
            loadLevel3Passage();
        }

        function loadLevel3Passage() {
            isClassificationMode = false;
            selectedL3Words = [];
            
            document.getElementById('level3-instruction').innerText = "Mula-mula: Ketik seberapa banyak kata berimbuhan dalam petikan di bawah.";
            document.getElementById('btn-level3-action').innerText = "Kategorikan Perkataan";
            document.getElementById('btn-level3-action').className = "px-8 py-3 bg-indigo-600 text-white font-heading font-bold rounded-xl shadow-lg shadow-indigo-200 hover:bg-indigo-700 transition-all bounce-btn";
            document.getElementById('level3-classification-section').classList.add('hidden');

            if (passagesDatabase.length === 0 || !passagesDatabase[activePassageIndex]) {
                document.getElementById('interactive-text-box').innerHTML = `<p class="text-sm text-slate-400 text-center py-8">Tiada petikan berdaftar. Sila tambah di Panel Admin.</p>`;
                return;
            }

            const passageData = passagesDatabase[activePassageIndex];
            const textBox = document.getElementById('interactive-text-box');
            textBox.innerHTML = '';

            const rawWords = passageData.text.split(' ');
            rawWords.forEach((word, idx) => {
                const cleanWord = word.replace(/[.,\/#!$%\^&\*;:{}=\-_`~()]/g,"");
                const span = document.createElement('span');
                span.innerText = word + ' ';
                span.className = "cursor-pointer hover:bg-indigo-100 px-1 py-0.5 rounded transition-all duration-150 inline-block m-0.5 font-semibold text-slate-700 text-lg";
                span.id = `l3-w-${idx}`;
                
                if (cleanWord.length > 0) {
                    span.onclick = () => toggleL3Word(cleanWord, `l3-w-${idx}`);
                }
                textBox.appendChild(span);
            });
        }

        function toggleL3Word(word, elementId) {
            if (isClassificationMode) return;

            const el = document.getElementById(elementId);
            const index = selectedL3Words.findIndex(item => item.id === elementId);

            if (index > -1) {
                selectedL3Words.splice(index, 1);
                el.classList.remove('bg-indigo-200', 'text-indigo-900', 'font-bold');
            } else {
                selectedL3Words.push({ id: elementId, word: word });
                el.classList.add('bg-indigo-200', 'text-indigo-900', 'font-bold');
            }
            playAudioFeedback('success');
        }

        function proceedToClassification() {
            if (selectedL3Words.length === 0) {
                Swal.fire('Opps!', 'Sila ketik sekurang-kurangnya satu perkataan berimbuhan daripada petikan sebelum meneruskan fasa pengelasan.', 'warning');
                return;
            }

            if (!isClassificationMode) {
                isClassificationMode = true;
                document.getElementById('level3-instruction').innerText = "Fasa Kedua: Sila pilih kategori yang betul bagi setiap perkataan di bawah.";
                document.getElementById('level3-classification-section').classList.remove('hidden');
                document.getElementById('btn-level3-action').innerText = "Sahkan Jawapan Petikan";
                document.getElementById('btn-level3-action').className = "px-8 py-3 bg-emerald-600 text-white font-heading font-bold rounded-xl shadow-lg shadow-emerald-200 hover:bg-emerald-700 transition-all bounce-btn";

                renderLevel3ClassificationList();
            } else {
                verifyLevel3Answers();
            }
        }

        function renderLevel3ClassificationList() {
            const container = document.getElementById('level3-classification-list');
            container.innerHTML = '';

            selectedL3Words.forEach((item, index) => {
                item.userCategory = item.userCategory || '';

                const row = document.createElement('div');
                row.className = "p-4 bg-slate-50 border border-slate-200 rounded-xl flex flex-col md:flex-row md:items-center justify-between gap-3";
                row.innerHTML = `
                    <div class="font-bold text-lg text-indigo-900">${item.word}</div>
                    <div class="flex flex-wrap gap-2">
                        <button onclick="selectCategoryForWord(${index}, 'Imbuhan Awalan')" id="btn-l3cat-${index}-awalan" class="px-3 py-1 bg-white border border-slate-200 rounded-lg text-xs font-semibold ${item.userCategory === 'Imbuhan Awalan' ? 'bg-blue-100 border-blue-500 text-blue-700 font-bold' : ''}">Awalan</button>
                        <button onclick="selectCategoryForWord(${index}, 'Imbuhan Akhiran')" id="btn-l3cat-${index}-akhiran" class="px-3 py-1 bg-white border border-slate-200 rounded-lg text-xs font-semibold ${item.userCategory === 'Imbuhan Akhiran' ? 'bg-pink-100 border-pink-500 text-pink-700 font-bold' : ''}">Akhiran</button>
                        <button onclick="selectCategoryForWord(${index}, 'Imbuhan Apitan')" id="btn-l3cat-${index}-apitan" class="px-3 py-1 bg-white border border-slate-200 rounded-lg text-xs font-semibold ${item.userCategory === 'Imbuhan Apitan' ? 'bg-emerald-100 border-emerald-500 text-emerald-700 font-bold' : ''}">Apitan</button>
                        <button onclick="selectCategoryForWord(${index}, 'Imbuhan Sisipan')" id="btn-l3cat-${index}-sisipan" class="px-3 py-1 bg-white border border-slate-200 rounded-lg text-xs font-semibold ${item.userCategory === 'Imbuhan Sisipan' ? 'bg-purple-100 border-purple-500 text-purple-700 font-bold' : ''}">Sisipan</button>
                    </div>
                `;
                container.appendChild(row);
            });
        }

        function selectCategoryForWord(wordIndex, category) {
            selectedL3Words[wordIndex].userCategory = category;
            renderLevel3ClassificationList();
            playAudioFeedback('success');
        }

        function verifyLevel3Answers() {
            const passageData = passagesDatabase[activePassageIndex];
            const correctAnswers = passageData.answers;

            let correctCount = 0;
            let totalPossible = correctAnswers.length;

            selectedL3Words.forEach(sel => {
                const cleanSelectedWord = sel.word.replace(/[.,\/#!$%\^&\*;:{}=\-_`~()]/g,"").toLowerCase();
                const matchedAnswer = correctAnswers.find(ans => ans.word.toLowerCase() === cleanSelectedWord);

                if (matchedAnswer && sel.userCategory === matchedAnswer.type) {
                    correctCount++;
                }
            });

            const timestamp = new Date().toLocaleString('ms-MY');
            const scoreRecord = {
                name: currentUser.name,
                class: currentUser.class,
                activity: `Petikan Tahap 3 (${passageData.title.split(':')[0]})`,
                score: `${correctCount} / ${totalPossible}`,
                status: correctCount === totalPossible ? 'Semua Ditandai 🎉' : 'Bimbingan Lanjutan Diperlukan',
                time: timestamp
            };

            scoresDatabase.push(scoreRecord);
            localStorage.setItem('tarkim_scores_v3', JSON.stringify(scoresDatabase));

            if (googleSheetsApiUrl) {
                sendScoreToGoogleSheets(scoreRecord);
            }

            starsCollected += parseFloat(correctCount * 0.5);
            updateStarsDisplay();

            playAudioFeedback('success');
            Swal.fire({
                title: 'Keputusan Petikan Direkod!',
                html: `<p class="text-lg">Markah Anda: <strong class="text-indigo-600 text-2xl">${correctCount} / ${totalPossible}</strong></p><p class="text-xs text-slate-500 mt-2">Keputusan telah disimpan dengan selamat dalam database admin!</p>`,
                icon: 'success',
                confirmButtonColor: '#4F46E5',
                confirmButtonText: 'Teruskan'
            }).then(() => {
                showSection('permainan');
            });
        }

        // === KUIZ FORMATIF ===
        let currentQuizIndex = 0;
        let quizAnswersCollected = [];
        let tempSelectedAnswer = '';

        function startQuiz() {
            if (quizDatabase.length === 0) {
                Swal.fire('Opps!', 'Tiada soalan kuiz berdaftar. Sila daftar soalan kuiz di Panel Admin.', 'warning');
                return;
            }
            showScreen('section-kuiz');
            currentQuizIndex = 0;
            quizAnswersCollected = [];
            loadQuizQuestion();
        }

        function loadQuizQuestion() {
            const qData = quizDatabase[currentQuizIndex];
            document.getElementById('quiz-question-num').innerText = `Soalan ${currentQuizIndex + 1} daripada ${quizDatabase.length}`;
            document.getElementById('quiz-objective-tag').innerText = `Objektif: ${qData.objective}`;
            document.getElementById('quiz-question-text').innerText = qData.q;

            const container = document.getElementById('quiz-options-container');
            container.innerHTML = '';

            qData.options.forEach(opt => {
                const btn = document.createElement('button');
                btn.className = "quiz-opt-btn bg-white hover:bg-brand-softMint border-2 border-slate-100 p-5 rounded-2xl text-left font-bold text-lg text-slate-700 transition-all flex items-center gap-4 bounce-btn";
                btn.innerHTML = `
                    <div class="w-8 h-8 rounded-full border-2 border-slate-200 flex items-center justify-center text-sm font-bold text-slate-400 select-indicator"></div>
                    <span>${opt}</span>
                `;
                btn.onclick = () => selectQuizOption(btn, opt);
                container.appendChild(btn);
            });

            document.getElementById('quiz-next-btn').disabled = true;
        }

        function selectQuizOption(btn, opt) {
            const buttons = document.querySelectorAll('.quiz-opt-btn');
            buttons.forEach(b => {
                b.classList.remove('border-brand-mint', 'bg-brand-softMint');
                b.querySelector('.select-indicator').classList.remove('bg-brand-mint', 'border-brand-mint', 'text-white');
                b.querySelector('.select-indicator').innerHTML = '';
            });

            btn.classList.add('border-brand-mint', 'bg-brand-softMint');
            const ind = btn.querySelector('.select-indicator');
            ind.classList.add('bg-brand-mint', 'border-brand-mint', 'text-white');
            ind.innerHTML = `<i class="fa-solid fa-check"></i>`;

            tempSelectedAnswer = opt;
            document.getElementById('quiz-next-btn').disabled = false;
        }

        function nextQuizQuestion() {
            quizAnswersCollected.push(tempSelectedAnswer);
            currentQuizIndex++;

            if (currentQuizIndex < quizDatabase.length) {
                loadQuizQuestion();
            } else {
                finishQuizAndSubmit();
            }
        }

        function finishQuizAndSubmit() {
            let score = 0;
            quizDatabase.forEach((q, idx) => {
                if (quizAnswersCollected[idx] === q.correct) {
                    score++;
                }
            });

            let status = '';
            const totalQs = quizDatabase.length;
            const percentage = (score / totalQs) * 100;

            if (percentage === 100) {
                status = 'Semua Objektif Tercapai 🎉';
            } else if (percentage >= 50) {
                status = 'Bimbingan Lanjutan Diperlukan';
            } else {
                status = 'Bimbingan Asas Diperlukan';
            }

            const timestamp = new Date().toLocaleString('ms-MY');
            const scoreRecord = {
                name: currentUser.name,
                class: currentUser.class,
                activity: 'Kuiz Formatif',
                score: `${score} / ${totalQs}`,
                status: status,
                time: timestamp
            };

            scoresDatabase.push(scoreRecord);
            localStorage.setItem('tarkim_scores_v3', JSON.stringify(scoresDatabase));

            if (googleSheetsApiUrl) {
                sendScoreToGoogleSheets(scoreRecord);
            }

            starsCollected += parseFloat(score);
            updateStarsDisplay();

            playAudioFeedback('success');
            Swal.fire({
                title: 'Tahniah, Selesai!',
                html: `<p class="text-lg">Skor Anda: <strong class="text-brand-mint text-2xl">${score} / ${totalQs}</strong></p><p class="text-sm text-slate-500 mt-2">${status}</p>`,
                icon: 'success',
                confirmButtonColor: '#10B981',
                confirmButtonText: 'Kembali ke Menu'
            }).then(() => {
                showScreen('screen-menu');
            });
        }

        // === PAPARAN ADMIN CMS LOGIK ===
        function showAdminView() {
            Swal.fire({
                title: 'Akses Pentadbir',
                text: 'Sila masukkan kata laluan untuk mengakses paparan admin:',
                input: 'password',
                inputAttributes: {
                    autocapitalize: 'off',
                    autocorrect: 'off'
                },
                showCancelButton: true,
                confirmButtonText: 'Masuk',
                cancelButtonText: 'Batal',
                confirmButtonColor: '#10B981',
                cancelButtonColor: '#64748B',
                preConfirm: (password) => {
                    if (password === 'cikgu123') {
                        return true;
                    } else {
                        Swal.showValidationMessage('Kata laluan salah! Cuba sekali lagi.');
                        return false;
                    }
                }
            }).then((result) => {
                if (result.isConfirmed) {
                    playAudioFeedback('success');
                    showScreen('section-admin');
                    switchAdminTab('scores');
                }
            });
        }

        function exitAdminView() {
            showScreen('screen-login');
        }

        function switchAdminTab(tabName) {
            document.getElementById('admin-tab-scores').classList.add('hidden');
            document.getElementById('admin-tab-words').classList.add('hidden');
            document.getElementById('admin-tab-passages').classList.add('hidden');
            document.getElementById('admin-tab-quiz').classList.add('hidden');
            document.getElementById('admin-tab-tech').classList.add('hidden');
            document.getElementById('admin-tab-media').classList.add('hidden');

            const allTabs = ['scores', 'words', 'passages', 'quiz', 'media', 'tech'];
            allTabs.forEach(t => {
                const btn = document.getElementById(`btn-tab-${t}`);
                if (btn) btn.className = "px-3.5 py-2 font-bold text-xs sm:text-sm border-b-2 border-transparent text-slate-500 hover:text-slate-800";
            });

            if (tabName === 'scores') {
                document.getElementById('admin-tab-scores').classList.remove('hidden');
                document.getElementById('btn-tab-scores').className = "px-3.5 py-2 font-bold text-xs sm:text-sm border-b-2 border-brand-mint text-brand-mint";
                loadAdminScoresTable();
            } else if (tabName === 'edit-words') {
                document.getElementById('admin-tab-words').classList.remove('hidden');
                document.getElementById('btn-tab-words').className = "px-3.5 py-2 font-bold text-xs sm:text-sm border-b-2 border-brand-mint text-brand-mint";
                loadAdminWordList();
            } else if (tabName === 'edit-passages') {
                document.getElementById('admin-tab-passages').classList.remove('hidden');
                document.getElementById('btn-tab-passages').className = "px-3.5 py-2 font-bold text-xs sm:text-sm border-b-2 border-brand-mint text-brand-mint";
                loadPassageToEditor();
            } else if (tabName === 'edit-quiz') {
                document.getElementById('admin-tab-quiz').classList.remove('hidden');
                document.getElementById('btn-tab-quiz').className = "px-3.5 py-2 font-bold text-xs sm:text-sm border-b-2 border-brand-mint text-brand-mint";
                loadQuizEditorQuestionsList();
            } else if (tabName === 'media') {
                document.getElementById('admin-tab-media').classList.remove('hidden');
                document.getElementById('btn-tab-media').className = "px-3.5 py-2 font-bold text-xs sm:text-sm border-b-2 border-brand-mint text-brand-mint";
                applyMediaSettings();
            } else if (tabName === 'tech-integration') {
                document.getElementById('admin-tab-tech').classList.remove('hidden');
                document.getElementById('btn-tab-tech').className = "px-3.5 py-2 font-bold text-xs sm:text-sm border-b-2 border-brand-mint text-brand-mint";
                document.getElementById('admin-api-url').value = googleSheetsApiUrl;
                document.getElementById('admin-api-url').onchange = (e) => {
                    googleSheetsApiUrl = e.target.value.trim();
                    localStorage.setItem('tarkim_api_url', googleSheetsApiUrl);
                };
            }
        }

        // --- SUB-CMS TAHAP 1 & 2 ---
        function loadAdminWordList() {
            const listContainer = document.getElementById('admin-word-list');
            listContainer.innerHTML = '';

            const renderItems = (items, lvl) => {
                items.forEach((w, idx) => {
                    const div = document.createElement('div');
                    div.className = "p-3 flex justify-between items-center text-sm border-b border-slate-100 hover:bg-slate-50";
                    div.innerHTML = `
                        <div>
                            <span class="text-[10px] font-bold uppercase tracking-wider px-2 py-0.5 rounded ${lvl === 1 ? 'bg-emerald-100 text-emerald-800' : 'bg-amber-100 text-amber-800'} mr-2">Tahap ${lvl}</span>
                            <span class="font-bold text-slate-700">${w.full}</span> 
                            <span class="text-xs text-slate-400">(${w.prefix || 'tiada'} + ${w.base} + ${w.suffix || 'tiada'})</span>
                        </div>
                        <button onclick="deleteAdminWord(${lvl}, ${w.id})" class="text-red-400 hover:text-red-600 px-2 py-1"><i class="fa-solid fa-trash-can"></i></button>
                    `;
                    listContainer.appendChild(div);
                });
            };

            renderItems(wordsDatabase[1], 1);
            renderItems(wordsDatabase[2], 2);
        }

        function saveAdminWord() {
            const lvl = parseInt(document.getElementById('admin-word-level').value);
            const base = document.getElementById('admin-word-base').value.trim();
            const prefix = document.getElementById('admin-word-prefix').value.trim();
            const suffix = document.getElementById('admin-word-suffix').value.trim();
            const full = document.getElementById('admin-word-full').value.trim();
            const type = document.getElementById('admin-word-type').value;

            if (!base || !full) {
                Swal.fire('Ralat', 'Kata dasar dan hasil gabungan kata lengkap wajib diisi!', 'error');
                return;
            }

            const newWord = {
                id: Date.now(),
                base: base,
                prefix: prefix,
                suffix: suffix,
                full: full,
                type: type
            };

            wordsDatabase[lvl].push(newWord);
            localStorage.setItem('tarkim_words_v3', JSON.stringify(wordsDatabase));
            
            document.getElementById('admin-word-base').value = '';
            document.getElementById('admin-word-prefix').value = '';
            document.getElementById('admin-word-suffix').value = '';
            document.getElementById('admin-word-full').value = '';

            Swal.fire('Berjaya', 'Perkataan baharu berjaya ditambahkan!', 'success');
            loadAdminWordList();
        }

        function deleteAdminWord(level, id) {
            const idx = wordsDatabase[level].findIndex(w => w.id === id);
            if (idx > -1) {
                wordsDatabase[level].splice(idx, 1);
                localStorage.setItem('tarkim_words_v3', JSON.stringify(wordsDatabase));
                loadAdminWordList();
            }
        }

        // --- SUB-CMS TAHAP 3 ---
        let tempGeneratedAnswers = [];

        function loadPassageToEditor() {
            const pIdx = parseInt(document.getElementById('admin-passage-select').value);
            const pas = passagesDatabase[pIdx];

            if (pas) {
                document.getElementById('admin-passage-title').value = pas.title;
                document.getElementById('admin-passage-text').value = pas.text;
                tempGeneratedAnswers = [...pas.answers];
                renderGeneratedTokensUI();
            }
        }

        function generatePassageWordTokens() {
            const text = document.getElementById('admin-passage-text').value.trim();
            if (!text) {
                Swal.fire('Ralat', 'Sila tulis teks petikan terlebih dahulu.', 'warning');
                return;
            }

            const uniqueWords = [];
            const rawWords = text.split(/\s+/);
            
            rawWords.forEach(w => {
                const clean = w.replace(/[.,\/#!$%\^&\*;:{}=\-_`~()]/g,"");
                if (clean.length > 1 && !uniqueWords.includes(clean)) {
                    uniqueWords.push(clean);
                }
            });

            const newAnswers = [];
            uniqueWords.forEach(word => {
                const existing = tempGeneratedAnswers.find(ans => ans.word.toLowerCase() === word.toLowerCase());
                if (existing) {
                    newAnswers.push(existing);
                }
            });
            tempGeneratedAnswers = newAnswers;

            renderGeneratedTokensUI(uniqueWords);
        }

        function renderGeneratedTokensUI(wordsList) {
            const container = document.getElementById('admin-passage-tokens-list');
            container.innerHTML = '';

            if (!wordsList) {
                wordsList = tempGeneratedAnswers.map(ans => ans.word);
            }

            if (wordsList.length === 0) {
                container.innerHTML = `<p class="text-xs text-slate-400 text-center py-8">Tiada kata dijumpai. Tekan "Jana Skema Jawapan" dahulu.</p>`;
                return;
            }

            wordsList.forEach((word, idx) => {
                const answerObj = tempGeneratedAnswers.find(ans => ans.word.toLowerCase() === word.toLowerCase());
                const isChecked = !!answerObj;
                const activeType = answerObj ? answerObj.type : 'Imbuhan Awalan';

                const div = document.createElement('div');
                div.className = "flex items-center justify-between p-2.5 bg-white rounded-lg border border-slate-200 text-sm";
                div.innerHTML = `
                    <div class="flex items-center gap-2">
                        <input type="checkbox" id="tok-check-${idx}" ${isChecked ? 'checked' : ''} onchange="toggleTokenAnswer(${idx}, '${word}')" class="w-4 h-4 text-brand-mint rounded border-slate-300 focus:ring-brand-mint">
                        <label for="tok-check-${idx}" class="font-bold text-slate-700">${word}</label>
                    </div>
                    <select id="tok-type-${idx}" onchange="updateTokenAffixType(${idx}, '${word}', this.value)" class="text-xs border border-slate-200 rounded p-1 ${isChecked ? '' : 'opacity-40 pointer-events-none'}">
                        <option value="Imbuhan Awalan" ${activeType === 'Imbuhan Awalan' ? 'selected' : ''}>Awalan</option>
                        <option value="Imbuhan Akhiran" ${activeType === 'Imbuhan Akhiran' ? 'selected' : ''}>Akhiran</option>
                        <option value="Imbuhan Apitan" ${activeType === 'Imbuhan Apitan' ? 'selected' : ''}>Apitan</option>
                        <option value="Imbuhan Sisipan" ${activeType === 'Imbuhan Sisipan' ? 'selected' : ''}>Sisipan</option>
                    </select>
                `;
                container.appendChild(div);
            });
        }

        function toggleTokenAnswer(idx, word) {
            const cb = document.getElementById(`tok-check-${idx}`);
            const sel = document.getElementById(`tok-type-${idx}`);
            
            if (cb.checked) {
                sel.classList.remove('opacity-40', 'pointer-events-none');
                tempGeneratedAnswers.push({ word: word, type: sel.value });
            } else {
                sel.classList.add('opacity-40', 'pointer-events-none');
                const fIdx = tempGeneratedAnswers.findIndex(ans => ans.word.toLowerCase() === word.toLowerCase());
                if (fIdx > -1) tempGeneratedAnswers.splice(fIdx, 1);
            }
        }

        function updateTokenAffixType(idx, word, value) {
            const answerObj = tempGeneratedAnswers.find(ans => ans.word.toLowerCase() === word.toLowerCase());
            if (answerObj) {
                answerObj.type = value;
            }
        }

        function saveActivePassage() {
            const pIdx = parseInt(document.getElementById('admin-passage-select').value);
            const title = document.getElementById('admin-passage-title').value.trim();
            const text = document.getElementById('admin-passage-text').value.trim();

            if (!title || !text) {
                Swal.fire('Ralat', 'Tajuk dan Teks petikan tidak boleh kosong!', 'error');
                return;
            }

            passagesDatabase[pIdx] = {
                title: title,
                text: text,
                answers: [...tempGeneratedAnswers]
            };

            localStorage.setItem('tarkim_passages_v3', JSON.stringify(passagesDatabase));
            Swal.fire('Selesai!', 'Petikan cerita dan skema jawapan telah disimpan!', 'success');
        }

        // --- SUB-CMS SUNTING KUIZ ---
        let activeQuizEditorIndex = 0;

        function loadQuizEditorQuestionsList() {
            const container = document.getElementById('admin-quiz-selector-list');
            container.innerHTML = '';

            quizDatabase.forEach((q, idx) => {
                const btn = document.createElement('button');
                btn.className = `w-full text-left p-3 rounded-lg text-sm font-semibold border transition-all ${idx === activeQuizEditorIndex ? 'border-brand-mint bg-emerald-50 text-emerald-800' : 'border-slate-200 hover:bg-slate-50'}`;
                btn.innerText = `S${idx+1}: ${q.q.substring(0, 30)}...`;
                btn.onclick = () => {
                    activeQuizEditorIndex = idx;
                    loadQuizQuestionToEditor();
                    loadQuizEditorQuestionsList();
                };
                container.appendChild(btn);
            });

            loadQuizQuestionToEditor();
        }

        function loadQuizQuestionToEditor() {
            const q = quizDatabase[activeQuizEditorIndex];
            if (!q) return;

            document.getElementById('admin-quiz-active-q-label').innerText = `Soalan ${activeQuizEditorIndex + 1}`;
            document.getElementById('admin-quiz-q-text').value = q.q;
            document.getElementById('admin-quiz-q-objective').value = q.objective;
            
            document.getElementById('admin-quiz-opt-0').value = q.options[0] || '';
            document.getElementById('admin-quiz-opt-1').value = q.options[1] || '';
            document.getElementById('admin-quiz-opt-2').value = q.options[2] || '';
            document.getElementById('admin-quiz-opt-3').value = q.options[3] || '';

            const correctSelect = document.getElementById('admin-quiz-correct-select');
            correctSelect.innerHTML = '';
            
            q.options.forEach((opt, idx) => {
                const o = document.createElement('option');
                o.value = opt;
                o.innerText = `Pilihan ${idx+1}: ${opt}`;
                if (opt === q.correct) o.selected = true;
                correctSelect.appendChild(o);
            });
            
            for(let i=0; i<4; i++) {
                document.getElementById(`admin-quiz-opt-${i}`).oninput = () => {
                    updateCorrectAnswerDropdownOptions();
                }
            }
        }

        function updateCorrectAnswerDropdownOptions() {
            const correctSelect = document.getElementById('admin-quiz-correct-select');
            const currentSelectedValue = correctSelect.value;
            correctSelect.innerHTML = '';

            for (let i = 0; i < 4; i++) {
                const optVal = document.getElementById(`admin-quiz-opt-${i}`).value.trim();
                if(optVal) {
                    const o = document.createElement('option');
                    o.value = optVal;
                    o.innerText = `Pilihan ${i+1}: ${optVal}`;
                    if (optVal === currentSelectedValue) o.selected = true;
                    correctSelect.appendChild(o);
                }
            }
        }

        function saveActiveQuizQuestion() {
            const qText = document.getElementById('admin-quiz-q-text').value.trim();
            const qObj = document.getElementById('admin-quiz-q-objective').value.trim();
            
            const options = [];
            for (let i = 0; i < 4; i++) {
                const optVal = document.getElementById(`admin-quiz-opt-${i}`).value.trim();
                if (optVal) options.push(optVal);
            }

            const correct = document.getElementById('admin-quiz-correct-select').value;

            if (!qText || options.length < 2 || !correct) {
                Swal.fire('Ralat', 'Pastikan pernyataan soalan, sekurang-kurangnya 2 pilihan jawapan, dan jawapan betul telah diisi!', 'error');
                return;
            }

            quizDatabase[activeQuizEditorIndex] = {
                q: qText,
                options: options,
                correct: correct,
                objective: qObj || 'Tatabahasa'
            };

            localStorage.setItem('tarkim_quiz_v3', JSON.stringify(quizDatabase));
            Swal.fire('Simpan!', 'Soalan kuiz berjaya dikemas kini!', 'success');
            loadQuizEditorQuestionsList();
        }

        // --- SCORES TABLE ---
        function loadAdminScoresTable() {
            const body = document.getElementById('table-scores-body');
            body.innerHTML = '';

            if (scoresDatabase.length === 0) {
                body.innerHTML = `<tr><td colspan="6" class="p-4 text-center text-slate-400">Tiada rekod keputusan kuiz murid buat masa ini.</td></tr>`;
                return;
            }

            [...scoresDatabase].reverse().forEach(score => {
                const tr = document.createElement('tr');
                tr.className = "hover:bg-slate-50";
                tr.innerHTML = `
                    <td class="p-3 font-bold text-slate-700">${score.name}</td>
                    <td class="p-3 text-slate-500">${score.class}</td>
                    <td class="p-3 text-slate-600 font-semibold">${score.activity || 'Latihan'}</td>
                    <td class="p-3 font-bold text-emerald-600">${score.score}</td>
                    <td class="p-3 text-slate-600">${score.status}</td>
                    <td class="p-3 text-xs text-slate-400">${score.time}</td>
                `;
                body.appendChild(tr);
            });
        }

        function clearLocalStorageScores() {
            Swal.fire({
                title: 'Padam Semua Rekod?',
                text: "Semua rekod markah murid akan dipadamkan secara kekal dari memori peranti ini!",
                icon: 'warning',
                showCancelButton: true,
                confirmButtonColor: '#EF4444',
                confirmButtonText: 'Ya, Padam!',
                cancelButtonText: 'Batal'
            }).then((result) => {
                if (result.isConfirmed) {
                    scoresDatabase = [];
                    localStorage.removeItem('tarkim_scores_v3');
                    loadAdminScoresTable();
                    Swal.fire('Dipadam!', 'Rekod laporan skor telah dibersihkan.', 'success');
                }
            });
        }

        // --- MEDIA FILES HANDLING ---
        function handleMediaFileUpload(inputElementId, dbKey, previewElementId) {
            const fileInput = document.getElementById(inputElementId);
            const file = fileInput.files[0];
            if (file) {
                const reader = new FileReader();
                reader.onload = function(e) {
                    mediaDatabase[dbKey] = e.target.result;
                    localStorage.setItem('tarkim_media_v3', JSON.stringify(mediaDatabase));
                    document.getElementById(previewElementId).src = e.target.result;
                    applyMediaSettings();
                    Swal.fire('Berjaya', 'Imej baharu berjaya disimpan!', 'success');
                };
                reader.readAsDataURL(file);
            }
        }

        function saveMediaUrl(dbKey, value, previewElementId) {
            if (value.trim()) {
                mediaDatabase[dbKey] = value.trim();
                localStorage.setItem('tarkim_media_v3', JSON.stringify(mediaDatabase));
                document.getElementById(previewElementId).src = value.trim();
                applyMediaSettings();
                Swal.fire('Berjaya', 'URL imej berjaya disimpan!', 'success');
            }
        }

        function saveVideoLink(value) {
            if (value.trim()) {
                mediaDatabase.videoUrl = value.trim();
                localStorage.setItem('tarkim_media_v3', JSON.stringify(mediaDatabase));
                applyMediaSettings();
                Swal.fire('Berjaya', 'Pautan video YouTube berjaya disimpan!', 'success');
            }
        }

        function resetMediaDefaults() {
            Swal.fire({
                title: 'Set Semula Media?',
                text: "Nota & Teknik akan dikembalikan ke tetapan asal infografik interaktif.",
                icon: 'warning',
                showCancelButton: true,
                confirmButtonColor: '#EF4444',
                confirmButtonText: 'Ya, Set Semula!'
            }).then((result) => {
                if (result.isConfirmed) {
                    mediaDatabase = {
                        nota1: 'interactive_template',
                        nota2: 'interactive_template',
                        teknik1: 'interactive_template',
                        videoUrl: 'https://www.youtube.com/embed/dQw4w9WgXcQ'
                    };
                    localStorage.setItem('tarkim_media_v3', JSON.stringify(mediaDatabase));
                    applyMediaSettings();
                    Swal.fire('Selesai', 'Media dikembalikan ke tetapan asal!', 'success');
                }
            });
        }

        function copyAppsScriptCode() {
            const textarea = document.getElementById('apps-script-textarea');
            textarea.select();
            document.execCommand('copy');
            Swal.fire({
                title: 'Disalin!',
                text: 'Kod Apps Script berjaya disalin.',
                icon: 'success',
                timer: 1500,
                showConfirmButton: false
            });
        }
        
        function sendScoreToGoogleSheets(scoreRecord) {
            fetch(googleSheetsApiUrl, {
                method: 'POST',
                mode: 'no-cors',
                headers: {
                    'Content-Type': 'application/json'
                },
                body: JSON.stringify(scoreRecord)
            }).then(() => {
                console.log("Scores updated dynamically in Google Sheet database");
            }).catch(err => {
                console.error("API Sheet Connection failed", err);
            });
        }
    </script>
</body>
</html>
