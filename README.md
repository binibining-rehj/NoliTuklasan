# NoliTuklasan
Educational game about Noli Me Tangere Aralin 1
<!DOCTYPE html>
<html lang="fil">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>NoliTuklasan: Kasaysayan ng Noli Me Tangere</title>
    <!-- Tailwind CSS for modern adaptive layouts -->
    <script src="https://cdn.tailwindcss.com"></script>
    <!-- Google Fonts for Retro and Modern Clean typography -->
    <link href="https://fonts.googleapis.com/css2?family=Press+Start+2P&family=Plus+Jakarta+Sans:wght@400;500;600;700;800&display=swap" rel="stylesheet">
    <!-- Font Awesome for sleek icons -->
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css">
    
    <style>
        body {
            font-family: 'Plus Jakarta Sans', sans-serif;
            background: linear-gradient(135deg, #0f172a 0%, #1e293b 100%);
            color: #f8fafc;
            overflow-x: hidden;
        }
        .pixel-font {
            font-family: 'Press Start 2P', cursive;
        }
        /* Custom retro scanline effect for gaming immersion */
        .scanlines {
            position: relative;
            overflow: hidden;
        }
        .scanlines::before {
            content: " ";
            display: block;
            position: absolute;
            top: 0; left: 0; bottom: 0; right: 0;
            background: linear-gradient(rgba(18, 16, 16, 0) 50%, rgba(0, 0, 0, 0.25) 50%), linear-gradient(90deg, rgba(255, 0, 0, 0.06), rgba(0, 255, 0, 0.02), rgba(0, 0, 255, 0.06));
            z-index: 10;
            background-size: 100% 4px, 6px 100%;
            pointer-events: none;
        }
        /* Custom scrollbar for lore book */
        ::-webkit-scrollbar {
            width: 8px;
        }
        ::-webkit-scrollbar-track {
            background: #1e293b;
            border-radius: 8px;
        }
        ::-webkit-scrollbar-thumb {
            background: #475569;
            border-radius: 8px;
        }
        ::-webkit-scrollbar-thumb:hover {
            background: #64748b;
        }
        /* Animation keys */
        @keyframes bounce-slow {
            0%, 100% { transform: translateY(0); }
            50% { transform: translateY(-8px); }
        }
        .animate-bounce-slow {
            animation: bounce-slow 3s infinite ease-in-out;
        }
        @keyframes pulse-gold {
            0%, 100% { box-shadow: 0 0 10px rgba(234, 179, 8, 0.3); }
            50% { box-shadow: 0 0 25px rgba(234, 179, 8, 0.7); }
        }
        .pulse-gold {
            animation: pulse-gold 2s infinite;
        }
    </style>
</head>
<body class="min-h-screen flex flex-col justify-between items-center p-2 sm:p-4 md:p-6 select-none">

<div class="w-full max-w-6xl bg-slate-900 border-4 border-amber-600 rounded-3xl overflow-hidden shadow-2xl scanlines relative flex flex-col justify-between aspect-[16/9] min-h-[580px] md:min-h-[640px] lg:min-h-[680px]">
    
    <!-- Top Utility Bar (Score, Health, Level, Sound) -->
    <header class="bg-slate-950/90 border-b-4 border-amber-600 px-4 py-3 flex justify-between items-center z-20">
        <div class="flex items-center gap-3">
            <div class="bg-amber-600 text-slate-950 font-bold px-3 py-1 rounded-lg text-sm flex items-center gap-2 shadow-inner">
                <i class="fa-solid fa-gamepad"></i>
                <span class="pixel-font text-[10px] tracking-tight">NoliTuklasan</span>
            </div>
            <div class="hidden sm:flex items-center gap-2 bg-slate-800/80 px-3 py-1 rounded-lg border border-slate-700">
                <span class="text-xs text-slate-400">Antas:</span>
                <span id="header-level-badge" class="text-xs font-bold text-amber-400">Madali (Level 1)</span>
            </div>
        </div>

        <!-- Live Score and Health Meter -->
        <div class="flex items-center gap-4">
            <div class="flex items-center gap-2 bg-slate-800/90 border border-slate-700 px-3 py-1.5 rounded-xl shadow-lg">
                <i class="fa-solid fa-star text-yellow-400 animate-pulse"></i>
                <span class="text-slate-400 text-xs hidden xs:inline">Puntos:</span>
                <span id="score-counter" class="pixel-font text-yellow-400 text-xs sm:text-sm">0000</span>
            </div>
            
            <div class="flex items-center gap-2 bg-slate-800/90 border border-slate-700 px-3 py-1.5 rounded-xl shadow-lg">
                <i class="fa-solid fa-heart text-red-500"></i>
                <span class="text-slate-400 text-xs hidden xs:inline">Buhay:</span>
                <div class="flex gap-0.5" id="hearts-container">
                    <i class="fa-solid fa-heart text-red-500 text-xs"></i>
                    <i class="fa-solid fa-heart text-red-500 text-xs"></i>
                    <i class="fa-solid fa-heart text-red-500 text-xs"></i>
                </div>
            </div>

            <!-- Sound & Help buttons -->
            <div class="flex items-center gap-2">
                <button id="sound-toggle" onclick="toggleMusic()" class="bg-slate-800 hover:bg-slate-700 border border-slate-600 text-slate-300 hover:text-white p-2 rounded-xl transition-all duration-200 shadow-md flex items-center justify-center w-9 h-9" title="Tugtog">
                    <i id="sound-icon" class="fa-solid fa-volume-xmark"></i>
                </button>
                <button onclick="openLoreModal()" class="bg-slate-800 hover:bg-slate-700 border border-slate-600 text-slate-300 hover:text-white p-2 rounded-xl transition-all duration-200 shadow-md flex items-center justify-center w-9 h-9" title="Gabay sa Kasaysayan">
                    <i class="fa-solid fa-book-open"></i>
                </button>
            </div>
        </div>
    </header>

    <main class="relative flex-grow flex flex-col items-center justify-center p-4 overflow-hidden bg-slate-900">
        
        <!-- Screen 1: Splash / Main Menu Screen -->
        <div id="screen-splash" class="absolute inset-0 z-10 flex flex-col justify-between items-center p-6 text-center bg-[radial-gradient(ellipse_at_center,_var(--tw-gradient-stops))] from-slate-800 via-slate-900 to-slate-950">
            <!-- Retro Visual Accent -->
            <div class="w-full flex justify-between items-center opacity-40">
                <div class="h-[2px] bg-gradient-to-r from-transparent via-amber-600 to-transparent flex-grow"></div>
                <div class="pixel-font text-[9px] text-amber-500 mx-4">Edisyong Pang-Eskuwela (DepEd Grade 9)</div>
                <div class="h-[2px] bg-gradient-to-r from-transparent via-amber-600 to-transparent flex-grow"></div>
            </div>

            <div class="my-auto flex flex-col items-center justify-center max-w-xl">
                <!-- Glowing Game Logo -->
                <div class="relative inline-block mb-3 animate-bounce-slow">
                    <span class="pixel-font text-3xl sm:text-4xl md:text-5xl text-amber-400 drop-shadow-[0_5px_0_rgba(180,83,9,1)] tracking-widest block uppercase">NoliTuklasan</span>
                    <span class="text-xs sm:text-sm font-semibold tracking-widest text-emerald-400 block mt-2 bg-slate-950/80 px-4 py-1 rounded-full border border-emerald-500/30">Muling Tuklasin ang Kasaysayan ng Noli Me Tangere</span>
                </div>

                <p class="text-slate-300 text-sm sm:text-base mb-6 leading-relaxed max-w-md">
                    Maglakbay sa taong 1884 hanggang 1887. Alamin ang hirap, sakripisyo, at mga lihim sa likod ng pagsusulat ni Dr. Jose Rizal ng kaniyang dakilang nobela.
                </p>

                <!-- Menu Buttons -->
                <div class="flex flex-col sm:flex-row gap-4 w-full justify-center">
                    <button onclick="startGame()" class="pixel-font text-xs sm:text-sm bg-gradient-to-b from-amber-500 to-amber-700 hover:from-amber-400 hover:to-amber-600 text-slate-950 font-bold px-8 py-4 rounded-xl shadow-[0_4px_0_#92400e] hover:translate-y-[2px] hover:shadow-[0_2px_0_#92400e] transition-all duration-150 flex items-center justify-center gap-2 pulse-gold">
                        <i class="fa-solid fa-play"></i> MAGSIMULA NG LARO
                    </button>
                    <button onclick="openLoreModal()" class="pixel-font text-[10px] sm:text-xs bg-slate-800 hover:bg-slate-700 border-2 border-slate-600 text-amber-400 font-bold px-6 py-4 rounded-xl hover:text-white transition-all duration-200 flex items-center justify-center gap-2">
                        <i class="fa-solid fa-scroll"></i> AKLAT NG KASAYSAYAN
                    </button>
                </div>
            </div>

            <!-- Bottom Cartoon Avatars Promo -->
            <div class="flex justify-center items-center gap-8 bg-slate-950/50 px-6 py-2.5 rounded-2xl border border-slate-800/80 max-w-md w-full">
                <div class="flex items-center gap-2">
                    <svg class="w-10 h-10 drop-shadow-md" viewBox="0 0 100 100" id="avatar-ibarra-promo"></svg>
                    <div class="text-left">
                        <span class="text-xs font-bold text-amber-400 block">Ibarra</span>
                        <span class="text-[9px] text-slate-400 block">Ang Gabay Mo</span>
                    </div>
                </div>
                <div class="h-6 w-[1px] bg-slate-800"></div>
                <div class="flex items-center gap-2">
                    <svg class="w-10 h-10 drop-shadow-md" viewBox="0 0 100 100" id="avatar-maria-promo"></svg>
                    <div class="text-left">
                        <span class="text-xs font-bold text-emerald-400 block">Maria Clara</span>
                        <span class="text-[9px] text-slate-400 block">Katuwang sa Puntos</span>
                    </div>
                </div>
            </div>
        </div>

        <div id="screen-level-select" class="absolute inset-0 z-10 hidden flex-col justify-between items-center p-6 bg-[radial-gradient(ellipse_at_center,_var(--tw-gradient-stops))] from-slate-900 via-slate-950 to-slate-950">
            <div class="text-center mt-2">
                <h2 class="pixel-font text-xl sm:text-2xl text-amber-500 drop-shadow">PILIIN ANG IYONG ANTAS</h2>
                <p class="text-slate-400 text-xs sm:text-sm mt-1">Mayroong 3 antas na sumusubok sa iyong kaalaman sa kasaysayan ng Noli.</p>
            </div>

            <!-- 3 Columns for Easy, Medium, Hard Levels -->
            <div class="grid grid-cols-1 md:grid-cols-3 gap-4 w-full max-w-4xl my-auto">
                <!-- Level 1 Card -->
                <div class="bg-slate-900/95 border-2 border-emerald-500/50 hover:border-emerald-400 rounded-2xl p-4 flex flex-col justify-between transition-all duration-300 hover:scale-[1.02] shadow-xl relative group">
                    <div class="absolute -top-3 right-4 bg-emerald-500 text-slate-950 text-[9px] font-bold px-2 py-0.5 rounded-full uppercase pixel-font">Madali</div>
                    <div>
                        <h3 class="text-emerald-400 font-bold text-base flex items-center gap-2 mb-2">
                            <span class="pixel-font text-xs">A-1</span> Ang Inspirasyon
                        </h3>
                        <p class="text-xs text-slate-300 leading-relaxed mb-4">
                            Tuklasin ang simula ng pangarap ni Rizal. Bakit niya isinulat ang nobela? Anong aklat ang nagbigay-daan dito?
                        </p>
                    </div>
                    <button onclick="selectLevel(1)" class="w-full bg-emerald-500 hover:bg-emerald-400 text-slate-950 font-bold py-2.5 rounded-xl text-xs transition-all duration-150 uppercase pixel-font">
                        Pumasok <i class="fa-solid fa-arrow-right ml-1"></i>
                    </button>
                </div>

                <!-- Level 2 Card -->
                <div class="bg-slate-900/95 border-2 border-yellow-500/50 hover:border-yellow-400 rounded-2xl p-4 flex flex-col justify-between transition-all duration-300 hover:scale-[1.02] shadow-xl relative group">
                    <div class="absolute -top-3 right-4 bg-yellow-500 text-slate-950 text-[9px] font-bold px-2 py-0.5 rounded-full uppercase pixel-font">Katamtaman</div>
                    <div>
                        <h3 class="text-yellow-400 font-bold text-base flex items-center gap-2 mb-2">
                            <span class="pixel-font text-xs">A-2</span> Pagsulat sa Europa
                        </h3>
                        <p class="text-xs text-slate-300 leading-relaxed mb-4">
                            Sundan si Rizal sa Madrid, Paris, at Alemanya habang sinusulat ang bawat kabanata sa gitna ng matinding taglamig at gutom.
                        </p>
                    </div>
                    <button onclick="selectLevel(2)" class="w-full bg-yellow-500 hover:bg-yellow-400 text-slate-950 font-bold py-2.5 rounded-xl text-xs transition-all duration-150 uppercase pixel-font">
                        Pumasok <i class="fa-solid fa-arrow-right ml-1"></i>
                    </button>
                </div>

                <!-- Level 3 Card -->
                <div class="bg-slate-900/95 border-2 border-red-500/50 hover:border-red-400 rounded-2xl p-4 flex flex-col justify-between transition-all duration-300 hover:scale-[1.02] shadow-xl relative group">
                    <div class="absolute -top-3 right-4 bg-red-500 text-slate-950 text-[9px] font-bold px-2 py-0.5 rounded-full uppercase pixel-font">Mahirap</div>
                    <div>
                        <h3 class="text-red-400 font-bold text-base flex items-center gap-2 mb-2">
                            <span class="pixel-font text-xs">A-3</span> Pag-imprenta & Krisis
                        </h3>
                        <p class="text-xs text-slate-300 leading-relaxed mb-4">
                            Ang krisis sa Berlin noong 1887. Sino si Dr. Maximo Viola? Paano naligtas ang Noli mula sa pagkasunog at pagkaantala?
                        </p>
                    </div>
                    <button onclick="selectLevel(3)" class="w-full bg-red-500 hover:bg-red-400 text-slate-950 font-bold py-2.5 rounded-xl text-xs transition-all duration-150 uppercase pixel-font">
                        Pumasok <i class="fa-solid fa-arrow-right ml-1"></i>
                    </button>
                </div>
            </div>

            <button onclick="backToSplash()" class="text-slate-400 hover:text-white text-xs flex items-center gap-2 mb-2 transition-all">
                <i class="fa-solid fa-chevron-left"></i> Bumalik sa Pangunahing Menu
            </button>
        </div>

        <div id="screen-gameplay" class="absolute inset-0 z-10 hidden flex flex-col md:flex-row p-3 gap-3">
            
            <!-- Left Side: Interactive Graphic Panel / Visual Scene -->
            <div class="w-full md:w-[55%] bg-slate-950 border-2 border-slate-700 rounded-2xl overflow-hidden relative flex flex-col justify-between min-h-[220px] md:min-h-0">
                <!-- Scene Location / Title Header -->
                <div class="absolute top-2 left-2 z-20 bg-slate-900/90 border border-slate-700 px-3 py-1 rounded-xl flex items-center gap-2 shadow">
                    <span id="scene-tag-icon" class="text-amber-500"><i class="fa-solid fa-location-dot"></i></span>
                    <span id="scene-tag" class="text-[10px] font-bold tracking-wide uppercase text-slate-200">Madrid, Espanya (1884)</span>
                </div>

                <!-- Custom Canvas / SVG Retro Animation Scene -->
                <div class="flex-grow w-full relative flex items-center justify-center bg-gradient-to-b from-sky-950 to-indigo-950 select-none overflow-hidden" id="adventure-scene-viewport">
                    <!-- Layered backgrounds based on level -->
                    <div id="scene-bg-layer" class="absolute inset-0 transition-all duration-500 opacity-80">
                        <!-- Dynamically managed via JS templates -->
                    </div>

                    <!-- Pixel Animated Character Renderers (SVGs) -->
                    <div class="absolute bottom-2 left-[15%] w-24 h-24 sm:w-28 sm:h-28 z-10 transition-all duration-500 transform hover:scale-105" id="character-left-container">
                        <svg viewBox="0 0 100 100" class="w-full h-full" id="character-left"></svg>
                        <!-- Speaker Tag -->
                        <div class="absolute -bottom-1 left-1/2 -translate-x-1/2 bg-amber-500 text-slate-950 px-2 py-0.5 rounded text-[9px] font-bold pixel-font whitespace-nowrap shadow border border-amber-300" id="name-left">Rizal</div>
                    </div>

                    <div class="absolute bottom-2 right-[15%] w-24 h-24 sm:w-28 sm:h-28 z-10 transition-all duration-500 transform hover:scale-105 hidden" id="character-right-container">
                        <svg viewBox="0 0 100 100" class="w-full h-full" id="character-right"></svg>
                        <!-- Speaker Tag -->
                        <div class="absolute -bottom-1 left-1/2 -translate-x-1/2 bg-emerald-500 text-slate-950 px-2 py-0.5 rounded text-[9px] font-bold pixel-font whitespace-nowrap shadow border border-emerald-300" id="name-right">Ibarra</div>
                    </div>

                    <!-- Interactive Pointing Help Elements -->
                    <div id="interactive-point-bubble" class="absolute top-[35%] left-[50%] -translate-x-1/2 -translate-y-1/2 bg-slate-900/95 border-2 border-amber-500 p-2.5 rounded-xl shadow-2xl z-20 max-w-xs text-center hidden transform scale-95 transition-all">
                        <p id="interactive-point-text" class="text-[11px] text-amber-300 font-semibold leading-relaxed">Mag-click sa mga bagay upang makakuha ng clue!</p>
                    </div>
                </div>

                <!-- Live Tutorial Tip strip -->
                <div class="bg-slate-900/90 border-t border-slate-800 px-3 py-1.5 flex justify-between items-center z-10 text-[10px] text-slate-400">
                    <span class="flex items-center gap-1"><i class="fa-solid fa-circle-info text-amber-500"></i> Antas ng Kaalaman: <span id="knowledge-meter" class="text-amber-400 font-semibold">Bahagya</span></span>
                    <span id="level-progress-indicator" class="font-bold text-slate-300">Kabanata 1/5</span>
                </div>
            </div>

            <!-- Right Side: Narrative Panel & Multi-Choice Question Deck -->
            <div class="w-full md:w-[45%] flex flex-col justify-between gap-3 min-h-[300px] md:min-h-0">
                
                <!-- Dialogue / Scenario Presentation Panel -->
                <div class="bg-slate-900/90 border-2 border-slate-700 rounded-2xl p-4 flex-grow flex flex-col justify-between shadow-xl">
                    <div class="overflow-y-auto max-h-[140px] md:max-h-[180px] pr-1">
                        <!-- Current Dialogue Prompt -->
                        <div class="flex items-start gap-2 mb-2">
                            <i class="fa-solid fa-quote-left text-amber-500 text-xs mt-1"></i>
                            <h4 id="dialogue-character-title" class="text-xs font-extrabold uppercase text-amber-400 tracking-wider">Crisostomo Ibarra:</h4>
                        </div>
                        <p id="dialogue-text-content" class="text-xs sm:text-sm text-slate-200 leading-relaxed font-medium">
                            Nais mo bang malaman kung saan nakuha ni Rizal ang inspirasyon para sa pagsusulat ng Noli Me Tangere? Pindutin ang isa sa mga libro sa aking lamesa sa kaliwa!
                        </p>
                    </div>

                    <!-- Interactive Indicator (e.g., Progress meter, clues unlocked) -->
                    <div class="border-t border-slate-800 pt-3 mt-2 flex justify-between items-center">
                        <span class="text-[10px] uppercase text-slate-400 font-bold tracking-wider">Hulaan ang Wastong Sagot:</span>
                        <div class="flex gap-1" id="clues-indicator">
                            <!-- Clues tracker icons -->
                            <i class="fa-solid fa-lightbulb text-slate-600 text-xs" title="Clue 1"></i>
                            <i class="fa-solid fa-lightbulb text-slate-600 text-xs" title="Clue 2"></i>
                        </div>
                    </div>
                </div>

                <!-- Answer Grid Container (Will render 2 to 4 options dynamically) -->
                <div id="options-panel-container" class="grid grid-cols-1 gap-2">
                    <!-- Default State placeholders -->
                    <button class="bg-slate-800 hover:bg-slate-700 text-slate-100 p-3 rounded-xl text-left text-xs sm:text-sm transition-all border border-slate-700 hover:border-amber-500 flex items-center gap-3">
                        <span class="pixel-font bg-slate-900 text-amber-400 px-2.5 py-1 rounded border border-slate-700 text-[10px]">A</span>
                        <span>Naglo-load ng mga opsyon para sa laro...</span>
                    </button>
                </div>
            </div>

        </div>

        <div id="feedback-overlay" class="absolute inset-0 z-30 hidden items-center justify-center p-4 bg-slate-950/80 backdrop-blur-sm transition-all duration-300">
            <div id="feedback-card" class="bg-slate-900 border-4 border-emerald-500 rounded-2xl max-w-md w-full p-5 shadow-2xl transform scale-95 transition-all duration-300">
                <div class="text-center">
                    <div id="feedback-icon-container" class="w-16 h-16 bg-emerald-500/10 border-2 border-emerald-500 text-emerald-400 rounded-full flex items-center justify-center mx-auto mb-4 text-2xl animate-bounce">
                        <i class="fa-solid fa-circle-check"></i>
                    </div>
                    
                    <h3 id="feedback-title" class="pixel-font text-sm sm:text-base text-emerald-400 uppercase tracking-widest mb-1">Tumpak na Sagot!</h3>
                    <div class="bg-slate-950/80 rounded-xl p-3 border border-slate-800 text-left my-4">
                        <p id="feedback-explanation" class="text-xs sm:text-sm text-slate-300 leading-relaxed">
                            Napakagaling! Nakuha ni Rizal ang inspirasyon sa nobelang "Uncle Tom's Cabin" ni Harriet Beecher Stowe na naglalahad ng pang-aapi sa mga Amerikanong Itim.
                        </p>
                    </div>

                    <!-- Feedback Score change -->
                    <p class="text-xs text-yellow-400 font-bold mb-4 flex justify-center items-center gap-2">
                        <i class="fa-solid fa-star"></i> Makukuha mong Puntos: <span id="feedback-points-delta">+100 Puntos!</span>
                    </p>

                    <!-- Feedback Navigation -->
                    <button id="feedback-action-btn" onclick="advanceNext()" class="pixel-font text-xs bg-emerald-500 hover:bg-emerald-400 text-slate-950 font-bold w-full py-3.5 rounded-xl shadow-[0_4px_0_rgba(16,185,129,0.3)] hover:translate-y-[2px] hover:shadow-none transition-all duration-150 uppercase">
                        Ipagpatuloy ang Paglalakbay <i class="fa-solid fa-chevron-right ml-1"></i>
                    </button>
                </div>
            </div>
        </div>

        <div id="screen-game-over" class="absolute inset-0 z-10 hidden flex-col justify-between items-center p-6 bg-[radial-gradient(ellipse_at_center,_var(--tw-gradient-stops))] from-slate-900 via-slate-950 to-slate-950 text-center">
            
            <div class="my-auto flex flex-col items-center max-w-lg">
                <div id="gameover-visual-badge" class="w-20 h-20 bg-amber-500/10 border-4 border-amber-500 text-amber-500 rounded-full flex items-center justify-center mb-4 text-4xl">
                    <i class="fa-solid fa-award"></i>
                </div>

                <h2 id="gameover-title" class="pixel-font text-2xl text-amber-400 drop-shadow uppercase mb-2">MALIPAYONG PAGTATAPOS!</h2>
                <p id="gameover-desc" class="text-slate-300 text-xs sm:text-sm leading-relaxed mb-6 max-w-md">
                    Matagumpay mong natulungan si Dr. Jose Rizal na maisulat, maiprenta, at maipalaganap ang kaniyang nobelang Noli Me Tangere. Isa kang tunay na Bayaning Mag-aaral!
                </p>

                <!-- Final Stats Report -->
                <div class="bg-slate-900 border-2 border-slate-700 rounded-2xl p-4 w-full grid grid-cols-2 gap-4 mb-6">
                    <div class="text-left border-r border-slate-800 pr-2">
                        <span class="text-[10px] text-slate-400 uppercase block">Kabuuan ng Puntos</span>
                        <span id="final-score" class="pixel-font text-yellow-400 text-base sm:text-lg">0000</span>
                    </div>
                    <div class="text-left pl-2">
                        <span class="text-[10px] text-slate-400 uppercase block">Maling Sagot</span>
                        <span id="final-mistakes" class="pixel-font text-red-400 text-base sm:text-lg">0</span>
                    </div>
                </div>

                <!-- Navigation Action Buttons -->
                <div class="flex flex-col sm:flex-row gap-3 w-full justify-center">
                    <button onclick="restartGame()" class="pixel-font text-xs bg-gradient-to-b from-amber-500 to-amber-700 text-slate-950 font-bold px-6 py-3.5 rounded-xl hover:from-amber-400 hover:to-amber-600 transition-all shadow-[0_4px_0_#92400e]">
                        <i class="fa-solid fa-rotate-left"></i> Maglaro Muli
                    </button>
                    <button onclick="backToSplash()" class="pixel-font text-xs bg-slate-800 text-slate-200 font-bold px-6 py-3.5 rounded-xl hover:bg-slate-700 transition-all border border-slate-600">
                        <i class="fa-solid fa-house"></i> Pangunahing Menu
                    </button>
                </div>
            </div>

            <div class="text-xs text-slate-500">NoliTuklasan - DepEd Grade 9 Interactive Core</div>
        </div>

    </main>

    <!-- Bottom Dialogue Box Layout (Standardized footer) -->
    <footer class="bg-slate-950/95 border-t-4 border-amber-600 p-3 flex justify-between items-center text-xs z-20">
        <div class="flex items-center gap-2">
            <span class="inline-block w-2.5 h-2.5 bg-green-500 rounded-full animate-pulse"></span>
            <span class="text-slate-400 text-[10px] sm:text-xs">Grade 9 Aralin: Kaligirang Pangkasaysayan ng Noli Me Tangere</span>
        </div>
        <div class="text-slate-500 text-[9px] sm:text-xs font-mono">
            Copyright &copy; 2026 NoliTuklasan
        </div>
    </footer>

</div>

<div id="lore-modal" class="fixed inset-0 bg-slate-950/90 z-50 backdrop-blur-sm hidden items-center justify-center p-4">
    <div class="bg-slate-900 border-4 border-amber-600 max-w-2xl w-full h-[85vh] rounded-3xl overflow-hidden flex flex-col justify-between shadow-2xl">
        <!-- Modal Title -->
        <div class="bg-slate-950 p-4 border-b-2 border-amber-600 flex justify-between items-center">
            <h3 class="pixel-font text-xs sm:text-sm text-amber-500 flex items-center gap-2">
                <i class="fa-solid fa-book-bookmark"></i> GABAY SA KASAYSAYAN NG NOLI ME TANGERE
            </h3>
            <button onclick="closeLoreModal()" class="text-slate-400 hover:text-white p-2 rounded-lg text-lg transition-all duration-150">
                <i class="fa-solid fa-xmark"></i>
            </button>
        </div>

        <!-- Scrollable content area -->
        <div class="flex-grow overflow-y-auto p-5 sm:p-6 space-y-6 text-xs sm:text-sm text-slate-200 leading-relaxed">
            <!-- Section 1 -->
            <div>
                <h4 class="font-extrabold text-amber-400 text-sm border-b border-slate-700 pb-1 mb-2">1. Ang Inspirasyon sa Pagsusulat (Madrid, Espanya)</h4>
                <p class="mb-2">
                    Si Jose Rizal ay 24 na taong gulang pa lamang nang mapag-isipang sumulat ng nobelang magbubukas sa mga mata ng kaniyang mga kababayan. 
                </p>
                <p class="mb-2">
                    Nabasang inspirasyon ni Rizal ang nobelang <strong class="text-emerald-400">"Uncle Tom's Cabin"</strong> ni Harriet Beecher Stowe na naglalahad sa malupit na buhay ng mga aliping Itim sa ilalim ng mga Amerikanong Puti. Napagtanto niya na may matinding pagkakahawig ito sa paghihirap ng mga Pilipino sa kamay ng mga Kastila.
                </p>
                <p>
                    Iminungkahi niya sa kaniyang mga kasamahan sa <strong class="text-yellow-400">La Solidaridad</strong> na sama-sama nilang isulat ang nobela, ngunit tumanggi ang iba at mas ginusto pang magsulat ukol sa sugal at kababaihan, kaya nagpasya siyang isulat ito nang mag-isa.
                </p>
            </div>

            <!-- Section 2 -->
            <div>
                <h4 class="font-extrabold text-amber-400 text-sm border-b border-slate-700 pb-1 mb-2">2. Pagsusulat sa Gitna ng Krisis (Europa)</h4>
                <p class="mb-2">
                    Isinulat ni Rizal ang unang kalahati ng nobela sa <strong class="text-emerald-400">Madrid, Espanya</strong> (isang-kapat), nagpatuloy sa <strong class="text-emerald-400">Paris, Pransya</strong> (isang-kapat), at tinapos ang huling kalahati sa <strong class="text-emerald-400">Alemanya (Heidelberg, Wilhelmsfeld, at Berlin)</strong> mula 1884 hanggang 1887.
                </p>
                <p>
                    Sa Berlin, dumanas si Rizal ng matinding gutom, lamig, at kahirapan. Halos sunugin niya ang manuskrito dahil nawalan na siya ng pag-asa na ito ay maipapalathala, sanhi ng kawalan ng sapat na salapi.
                </p>
            </div>

            <!-- Section 3 -->
            <div>
                <h4 class="font-extrabold text-amber-400 text-sm border-b border-slate-700 pb-1 mb-2">3. Ang Tagapagligtas ng Noli (Berlin, 1887)</h4>
                <p class="mb-2">
                    Dumating si <strong class="text-yellow-400">Dr. Maximo Viola</strong>, isang mayamang kaibigan ni Rizal mula sa Bulacan. Nang makita ang malunos na kalagayan ni Rizal at ang kaniyang gawa, ipinahiram niya ang halagang <strong class="text-emerald-400">Php 300</strong> upang maiprenta ang unang <strong class="text-amber-400">2,000 sipi</strong> ng nobela.
                </p>
                <p>
                    Natapos ang pag-imprenta sa Berliner Buchdruckerei-Aktiengesellschaft noong <strong class="text-yellow-400">Marso 21, 1887</strong>. Bilang pasasalamat, inihandog ni Rizal kay Maximo Viola ang orihinal na manuskrito kasama ang ginamit niyang panulat.
                </p>
            </div>
        </div>

        <!-- Modal Footer -->
        <div class="bg-slate-950 p-4 border-t-2 border-amber-600 flex justify-center">
            <button onclick="closeLoreModal()" class="pixel-font text-xs bg-amber-500 hover:bg-amber-400 text-slate-950 font-bold px-6 py-3 rounded-xl transition-all duration-150">
                NAUNAWAAN KO NA <i class="fa-solid fa-check ml-1"></i>
            </button>
        </div>
    </div>
</div>

<script>
    /* GAME DATA & SCENARIOS DECK */
    const GAME_LEVELS_DATA = {
        1: {
            title: "MADALI (Antas 1)",
            sceneTag: "Madrid, Espanya - Enero 1884",
            backgroundTemplate: 'madrid',
            questions: [
                {
                    id: "L1_Q1",
                    character: "Crisostomo Ibarra",
                    speaker: "left",
                    avatarLeft: "ibarra",
                    avatarRight: "none",
                    dialogue: "Mabuhay! Ako si Crisostomo Ibarra. Alam mo ba kung anong sikat na nobelang Amerikano ang nagbigay-daan sa pangarap ni Dr. Jose Rizal na isulat ang Noli Me Tangere?",
                    options: [
                        { text: "Uncle Tom's Cabin ni Harriet Beecher Stowe", correct: true, points: 100, expl: "Tumpak! Ang Uncle Tom's Cabin ay tumatalakay sa pang-aapi sa mga aliping Itim sa Amerika, na nag-udyok kay Rizal na isulat ang kalagayan ng Pilipinas." },
                        { text: "Les Misérables ni Victor Hugo", correct: false, points: 0, expl: "Mali. Bagaman maganda ang Les Misérables, ang 'Uncle Tom's Cabin' ang direktang nagbigay ng inspirasyon sa pagsulat ng Noli." },
                        { text: "The Count of Monte Cristo ni Alexandre Dumas", correct: false, points: 0, expl: "Mali. Ito ay isa sa mga paboritong aklat ni Rizal, ngunit ang nobelang tumatalakay sa slaveri na 'Uncle Tom's Cabin' ang tunay na inspirasyon." }
                    ]
                },
                {
                    id: "L1_Q2",
                    character: "Crisostomo Ibarra",
                    speaker: "left",
                    avatarLeft: "ibarra",
                    avatarRight: "none",
                    dialogue: "Iminungkahi ni Rizal sa kaniyang mga kasamahan sa La Solidaridad na tulungan siyang magsulat ng nobela. Ano ang naging tugon nila?",
                    options: [
                        { text: "Sumang-ayon sila ngunit nanghingi ng bayad.", correct: false, points: 0, expl: "Mali. Mas pinili nilang magsulat ng ibang paksain tulad ng sugal at kababaihan." },
                        { text: "Tumanggi sila at mas ginustong magsulat ukol sa sugal, alak, at kababaihan.", correct: true, points: 100, expl: "Tumpak! Dahil sa hindi pakikiisa ng kaniyang mga kasamahan sa Espanya, nagpasya si Rizal na mag-isang isulat ang Noli." },
                        { text: "Binuo agad nila ang unang pito na kabanata.", correct: false, points: 0, expl: "Mali. Walang nakipagtulungan kay Rizal kaya't siya ang mag-isang gumawa ng kabuuan nito." }
                    ]
                },
                {
                    id: "L1_Q3",
                    character: "Maria Clara",
                    speaker: "right",
                    avatarLeft: "none",
                    avatarRight: "maria",
                    dialogue: "Isang malugod na pagbati! Ako si Maria Clara. Alam mo ba kung ano ang literal na kahulugan ng pariralang Latin na 'Noli Me Tangere' sa wikang Filipino at nakuha sa Ebanghelyo ni San Juan?",
                    options: [
                        { text: "Huwag Mo Akong Salingin (Touch Me Not)", correct: true, points: 100, expl: "Tumpak! Galing ito sa Ebanghelyo ni San Juan (20:17) nang magpakita ang nabuhay na Hesus kay Maria Magdalena." },
                        { text: "Iligtas Mo ang Aking Bayan", correct: false, points: 0, expl: "Mali. 'Huwag Mo Akong Salingin' ang tunay na kahulugan ng Noli Me Tangere." },
                        { text: "Katotohanan Lamang ang Papalaya", correct: false, points: 0, expl: "Mali. 'Huwag Mo Akong Salingin' ang literal na salin mula sa orihinal na Bibliang Latin." }
                    ]
                }
            ]
        },
        2: {
            title: "KATAMTAMAN (Antas 2)",
            sceneTag: "Wilhelmsfeld at Berlin, Alemanya - 1886",
            backgroundTemplate: 'berlin_winter',
            questions: [
                {
                    id: "L2_Q1",
                    character: "Jose Rizal sa Alemanya",
                    speaker: "left",
                    avatarLeft: "rizal",
                    avatarRight: "none",
                    dialogue: "Napakalamig dito sa Berlin! Habang isinusulat ko ang mga huling bahagi ng aking nobela, anong matinding sakripisyo ang aking hinarap sa bawat araw dahil sa kawalan ng pera?",
                    options: [
                        { text: "Kumain lamang ng isang pandesal at uminom ng tubig baha.", correct: false, points: 0, expl: "Mali. Bagaman nagtipid, hindi naman siya uminom ng tubig baha!" },
                        { text: "Dumanas ng gutom, matinding lamig, at muntik nang sunugin ang manuskrito.", correct: true, points: 120, expl: "Tumpak! Sa Berlin, dumanas si Rizal ng gutom, lamig, at muntik na niyang sunugin ang manuskrito dahil sa labis na depresyon at kawalan ng pera pamprenta." },
                        { text: "Nagtrabaho bilang sastre upang may pambili ng papel.", correct: false, points: 0, expl: "Mali. Nag-aral at nagsulat siya, ngunit hindi siya nagtrabaho bilang sastre." }
                    ]
                },
                {
                    id: "L2_Q2",
                    character: "Jose Rizal sa Alemanya",
                    speaker: "left",
                    avatarLeft: "rizal",
                    avatarRight: "none",
                    dialogue: "Bakit ko nga ba pinili na isulat ang nobelang ito sa wikang Espanyol sa halip na Tagalog, gayong para naman ito sa mga mamamayang Pilipino?",
                    options: [
                        { text: "Dahil hindi ko alam mag-Tagalog nang diretso.", correct: false, points: 0, expl: "Mali. Bihasa si Rizal sa Tagalog." },
                        { text: "Upang maunawaan ng mga Kastila at mga elitistang Pilipino ang kabulukan ng sistema.", correct: true, points: 120, expl: "Tumpak! Nais ni Rizal na basahin ito ng mga Kastilang opisyal at ng mga Ilustrado (mga nakapag-aral na Pilipino) upang mapukaw ang kanilang konsensya." },
                        { text: "Sapagkat bawal isulat ang anumang aklat sa wikang Tagalog noong panahong iyon.", correct: false, points: 0, expl: "Mali. Maaari namang magsulat sa Tagalog, ngunit nais niyang patamaan ang pamahalaang Kastila gamit ang kanilang wika." }
                    ]
                }
            ]
        },
        3: {
            title: "MAHIRAP (Antas 3)",
            sceneTag: "Imprentahan sa Berlin - Marso 1887",
            backgroundTemplate: 'printing_press',
            questions: [
                {
                    id: "L3_Q1",
                    character: "Maximo Viola",
                    speaker: "right",
                    avatarLeft: "rizal",
                    avatarRight: "viola",
                    dialogue: "Ako si Maximo Viola, kaibigan ni Rizal mula sa Bulacan. Nang makita ko ang kaniyang kahirapan at panganib, magkano ang ipinahiram ko upang mailigtas at maiprenta ang Noli?",
                    options: [
                        { text: "300 Piso para sa pag-imprenta ng 2,000 sipi", correct: true, points: 150, expl: "Tumpak! Ang Php 300 ay napakalaking halaga noon na naging dahilan upang maipalathala ang unang 2,000 sipi sa Berlin." },
                        { text: "1,000 Piso para sa 5,000 sipi", correct: false, points: 0, expl: "Mali. Php 300 lamang ang ipinahiram ni Viola para sa 2,000 kopya." },
                        { text: "50 Piso bilang pangkain lamang ni Rizal", correct: false, points: 0, expl: "Mali. Malaki ang naitulong ni Viola, sapat para sa buong pagpapaimprenta ng nobela." }
                    ]
                },
                {
                    id: "L3_Q2",
                    character: "Jose Rizal",
                    speaker: "left",
                    avatarLeft: "rizal",
                    avatarRight: "viola",
                    dialogue: "Pagkatapos ng matagumpay na pagprenta noong Marso 21, 1887, paano ko ipinakita ang aking walang-hanggang pasasalamat sa aking kaibigang si Maximo Viola?",
                    options: [
                        { text: "Ibinigay ko sa kaniya ang orihinal na manuskrito at ang aking panulat.", correct: true, points: 150, expl: "Tumpak! Inihandog ni Rizal kay Viola ang orihinal na manuskrito ng Noli Me Tangere at ang pluma o panulat na ginamit niya." },
                        { text: "Binayaran ko siya ng doble mula sa aking allowance.", correct: false, points: 0, expl: "Mali. Walang kakayahan si Rizal na magbayad ng doble noong panahong iyon." },
                        { text: "Ginawa ko siyang pangunahing tauhan sa nobelang El Filibusterismo.", correct: false, points: 0, expl: "Mali. Hindi ginawang karakter si Viola sa nobelang El Fili." }
                    ]
                }
            ]
        }
    };

    /* STATE VARIABLES */
    let currentLevel = 1;
    let currentQuestionIndex = 0;
    let score = 0;
    let lives = 3;
    let totalMistakes = 0;
    let soundEnabled = false;
    let audioCtx = null;
    let soundLoopId = null;

    /* GRAPHIC RENDERING UTILITIES (Pixel Art/Cartoon Style SVGs) */
    const SVG_AVATARS = {
        ibarra: `
            <g transform="translate(0, 0)">
                <!-- Hat -->
                <rect x="25" y="10" width="50" height="15" fill="#334155" rx="4"/>
                <rect x="15" y="20" width="70" height="6" fill="#1e293b" rx="2"/>
                <!-- Face -->
                <rect x="30" y="26" width="40" height="35" fill="#fbcfe8" rx="8"/>
                <rect x="30" y="26" width="40" height="10" fill="#db2777" opacity="0.1"/> <!-- Shadow -->
                <!-- Hair -->
                <rect x="28" y="25" width="44" height="8" fill="#451a03"/>
                <rect x="28" y="25" width="6" height="20" fill="#451a03"/>
                <rect x="66" y="25" width="6" height="20" fill="#451a03"/>
                <!-- Eyes -->
                <rect x="38" y="38" width="6" height="6" fill="#0f172a" rx="1"/>
                <rect x="56" y="38" width="6" height="6" fill="#0f172a" rx="1"/>
                <rect x="40" y="36" width="4" height="1.5" fill="#451a03"/>
                <rect x="56" y="36" width="4" height="1.5" fill="#451a03"/>
                <!-- Blush & Smile -->
                <rect x="34" y="44" width="4" height="4" fill="#f43f5e" opacity="0.6" rx="1"/>
                <rect x="62" y="44" width="4" height="4" fill="#f43f5e" opacity="0.6" rx="1"/>
                <path d="M 46 48 Q 50 53 54 48" stroke="#0f172a" stroke-width="3" fill="none" stroke-linecap="round"/>
                <!-- Mustache (Gentlemanly Ibarra) -->
                <path d="M 42 45 Q 50 47 58 45" stroke="#451a03" stroke-width="2.5" fill="none"/>
                <!-- Coat collar & Tie -->
                <path d="M 30 61 L 70 61 L 80 90 L 20 90 Z" fill="#0f172a"/>
                <path d="M 45 61 L 55 61 L 50 75 Z" fill="#ffffff"/>
                <rect x="49" y="65" width="2" height="15" fill="#991b1b"/>
                <!-- Shiny Vest Details -->
                <circle cx="50" cy="80" r="2" fill="#fbbf24"/>
                <circle cx="50" cy="86" r="2" fill="#fbbf24"/>
            </g>
        `,
        maria: `
            <g transform="translate(0, 0)">
                <!-- Hair Back/Band -->
                <rect x="20" y="15" width="60" height="65" fill="#172554" rx="20"/>
                <!-- Face -->
                <rect x="30" y="22" width="40" height="38" fill="#fce7f3" rx="10"/>
                <!-- Hair Front/Bangs -->
                <rect x="26" y="18" width="48" height="12" fill="#0f172a" rx="4"/>
                <rect x="26" y="25" width="8" height="25" fill="#0f172a" rx="2"/>
                <rect x="66" y="25" width="8" height="25" fill="#0f172a" rx="2"/>
                <!-- Hair Flower Accent -->
                <circle cx="28" cy="22" r="6" fill="#f43f5e"/>
                <circle cx="28" cy="22" r="2" fill="#fef08a"/>
                <!-- Eyes -->
                <rect x="38" y="34" width="6" height="6" fill="#0f172a" rx="1"/>
                <rect x="56" y="34" width="6" height="6" fill="#0f172a" rx="1"/>
                <path d="M 36 31 C 39 30 43 31 43 31" stroke="#0f172a" stroke-width="2" fill="none"/>
                <path d="M 54 31 C 57 30 61 31 61 31" stroke="#0f172a" stroke-width="2" fill="none"/>
                <!-- Blush & Smile -->
                <rect x="33" y="42" width="4" height="4" fill="#fda4af" rx="1"/>
                <rect x="63" y="42" width="4" height="4" fill="#fda4af" rx="1"/>
                <path d="M 46 45 Q 50 50 54 45" stroke="#0f172a" stroke-width="2.5" fill="none" stroke-linecap="round"/>
                <!-- Filipiniana Dress (Maria Clara Style) -->
                <path d="M 30 60 L 70 60 L 85 90 L 15 90 Z" fill="#047857"/>
                <!-- Sleeves (Puff) -->
                <ellipse cx="20" cy="72" rx="10" ry="12" fill="#fbcfe8"/>
                <ellipse cx="80" cy="72" rx="10" ry="12" fill="#fbcfe8"/>
                <!-- Beautiful Lace/Scarf -->
                <path d="M 35 60 L 65 60 L 50 82 Z" fill="#ffffff" stroke="#cbd5e1" stroke-width="1"/>
                <circle cx="50" cy="72" r="3" fill="#fbbf24"/>
            </g>
        `,
        rizal: `
            <g transform="translate(0, 0)">
                <!-- Famous Rizal Haircut -->
                <path d="M 24 15 L 76 13 L 74 32 L 68 28 L 32 28 L 26 32 Z" fill="#090d16"/>
                <rect x="25" y="20" width="8" height="20" fill="#090d16" rx="2"/>
                <!-- Face -->
                <rect x="30" y="24" width="40" height="38" fill="#ffedd5" rx="6"/>
                <!-- Eyes -->
                <rect x="38" y="36" width="6" height="6" fill="#090d16" rx="1"/>
                <rect x="56" y="36" width="6" height="6" fill="#090d16" rx="1"/>
                <line x1="36" y1="31" x2="44" y2="33" stroke="#000" stroke-width="2"/>
                <line x1="64" y1="31" x2="56" y2="33" stroke="#000" stroke-width="2"/>
                <!-- Nose & Mouth (Determined & Intellectual) -->
                <line x1="50" y1="38" x2="50" y2="44" stroke="#d97706" stroke-width="1.5"/>
                <path d="M 46 48 L 54 48" stroke="#090d16" stroke-width="2.5" stroke-linecap="round"/>
                <!-- Suit Coat -->
                <path d="M 28 62 L 72 62 L 82 90 L 18 90 Z" fill="#1e293b"/>
                <path d="M 42 62 L 58 62 L 50 76 Z" fill="#ffffff"/>
                <!-- Tie -->
                <path d="M 48 68 L 52 68 L 50 82 Z" fill="#090d16"/>
                <!-- Overcoat Lapel -->
                <path d="M 28 62 L 40 78 L 40 90 L 18 90 Z" fill="#0f172a"/>
                <path d="M 72 62 L 60 78 L 60 90 L 82 90 Z" fill="#0f172a"/>
            </g>
        `,
        viola: `
            <g transform="translate(0, 0)">
                <!-- Hair (Wealthy doctor hairstyle) -->
                <rect x="26" y="16" width="48" height="15" fill="#451a03" rx="4"/>
                <rect x="24" y="22" width="6" height="18" fill="#451a03"/>
                <rect x="70" y="22" width="6" height="18" fill="#451a03"/>
                <!-- Face -->
                <rect x="30" y="24" width="40" height="36" fill="#fef3c7" rx="8"/>
                <!-- Eyes with Glasses -->
                <circle cx="40" cy="38" r="6" fill="none" stroke="#fbbf24" stroke-width="2"/>
                <circle cx="60" cy="38" r="6" fill="none" stroke="#fbbf24" stroke-width="2"/>
                <line x1="46" y1="38" x2="54" y2="38" stroke="#fbbf24" stroke-width="2"/>
                <!-- Black pupil inside glasses -->
                <rect x="39" y="37" width="2" height="2" fill="#0f172a"/>
                <rect x="59" y="37" width="2" height="2" fill="#0f172a"/>
                <!-- Friendly Mustache -->
                <path d="M 40 46 Q 50 49 60 46" stroke="#451a03" stroke-width="2" fill="none"/>
                <!-- Happy Smile -->
                <path d="M 47 50 Q 50 53 53 50" stroke="#0f172a" stroke-width="1.5" fill="none"/>
                <!-- Formal Suit (European Style) -->
                <path d="M 28 60 L 72 60 L 82 90 L 18 90 Z" fill="#065f46"/>
                <path d="M 44 60 L 56 60 L 50 78 Z" fill="#fef08a"/>
                <path d="M 48 64 L 52 64 L 50 72 Z" fill="#991b1b"/> <!-- Red necktie -->
            </g>
        `,
        none: ``
    };

    /* BACKGROUND SCENE TEMPLATES (SVGs for Pixel Art Visuals) */
    const SCENE_TEMPLATES = {
        madrid: `
            <svg viewBox="0 0 400 225" class="w-full h-full object-cover">
                <!-- Warm Madrid Interior -->
                <rect width="400" height="225" fill="#78350f"/>
                <!-- Floor -->
                <rect y="170" width="400" height="55" fill="#451a03"/>
                <line x1="0" y1="170" x2="400" y2="170" stroke="#f59e0b" stroke-width="2"/>
                <!-- Retro Window with Spain Sunlight -->
                <rect x="30" y="30" width="80" height="100" fill="#38bdf8" rx="2" stroke="#451a03" stroke-width="4"/>
                <line x1="70" y1="30" x2="70" y2="130" stroke="#451a03" stroke-width="2"/>
                <line x1="30" y1="80" x2="110" y2="80" stroke="#451a03" stroke-width="2"/>
                <!-- Spanish Flag on Wall -->
                <rect x="300" y="30" width="60" height="40" fill="#b91c1c" rx="1"/>
                <rect x="300" y="43" width="60" height="14" fill="#facc15"/>
                <!-- Table with Writing Quill and Books -->
                <rect x="150" y="130" width="100" height="42" fill="#d97706" rx="2" stroke="#451a03" stroke-width="2"/>
                <rect x="170" y="120" width="20" height="10" fill="#1e293b" rx="1"/> <!-- Book 1 -->
                <rect x="175" y="112" width="15" height="8" fill="#15803d" rx="1"/> <!-- Book 2 (Uncle Tom's Cabin) -->
                <!-- Feather Quill pen -->
                <path d="M 210 100 Q 200 115 195 125" stroke="#f8fafc" stroke-width="2" fill="none"/>
                <rect x="193" y="125" width="4" height="5" fill="#0284c7"/> <!-- Ink well -->
                <!-- Subtle Wall Lamp -->
                <circle cx="200" cy="40" r="10" fill="#fef08a" opacity="0.3"/>
                <circle cx="200" cy="40" r="4" fill="#fbbf24"/>
                <!-- Decorative Frame of Maria Clara -->
                <rect x="140" y="30" width="35" height="45" fill="#fdf2f8" stroke="#b45309" stroke-width="3"/>
                <path d="M 145 55 Q 157 40 170 55" stroke="#ec4899" stroke-width="2" fill="none"/>
            </svg>
        `,
        berlin_winter: `
            <svg viewBox="0 0 400 225" class="w-full h-full object-cover">
                <!-- Cool Dark Berlin Blue-Gray tone -->
                <rect width="400" height="225" fill="#0f172a"/>
                <!-- Floor -->
                <rect y="170" width="400" height="55" fill="#1e293b"/>
                <line x1="0" y1="170" x2="400" y2="170" stroke="#334155" stroke-width="2"/>
                <!-- Frosty Window showing winter snow -->
                <rect x="40" y="25" width="90" height="100" fill="#e2e8f0" rx="3" stroke="#1e293b" stroke-width="4"/>
                <line x1="85" y1="25" x2="85" y2="125" stroke="#1e293b" stroke-width="2"/>
                <!-- Falling Snowflakes (pixelized dots) outside -->
                <circle cx="55" cy="40" r="2" fill="#ffffff"/>
                <circle cx="110" cy="60" r="2" fill="#ffffff"/>
                <circle cx="70" cy="90" r="3" fill="#ffffff"/>
                <circle cx="120" cy="110" r="2.5" fill="#ffffff"/>
                <!-- Shivering candle on Rizal's writing table -->
                <rect x="160" y="140" width="80" height="32" fill="#475569" stroke="#1e293b" stroke-width="2" rx="1"/>
                <rect x="195" y="125" width="6" height="15" fill="#cbd5e1"/> <!-- Candle body -->
                <path d="M 198 112 Q 195 120 198 125" fill="#f97316"/> <!-- Flame -->
                <!-- Scattered manuscripts (near-burning) -->
                <rect x="250" y="145" width="25" height="15" fill="#f8fafc" transform="rotate(15, 250, 145)"/>
                <rect x="255" y="152" width="25" height="15" fill="#f8fafc" transform="rotate(-5, 255, 152)"/>
                <!-- Shivering heater fireplace (Empty/No fire) -->
                <rect x="310" y="80" width="60" height="90" fill="#334155" rx="2"/>
                <rect x="320" y="120" width="40" height="40" fill="#0f172a" rx="1"/> <!-- Cold Grate -->
            </svg>
        `,
        printing_press: `
            <svg viewBox="0 0 400 225" class="w-full h-full object-cover">
                <!-- Industrial Printing Press Environment -->
                <rect width="400" height="225" fill="#1e293b"/>
                <!-- Floor (Concrete gray) -->
                <rect y="170" width="400" height="55" fill="#475569"/>
                <line x1="0" y1="170" x2="400" y2="170" stroke="#94a3b8" stroke-width="2"/>
                <!-- Big Mechanical Gears -->
                <circle cx="80" cy="70" r="40" fill="#334155" stroke="#0f172a" stroke-width="4"/>
                <circle cx="80" cy="70" r="10" fill="#1e293b"/>
                <!-- Gear spikes -->
                <rect x="75" y="20" width="10" height="10" fill="#334155"/>
                <rect x="75" y="110" width="10" height="10" fill="#334155"/>
                <rect x="30" y="65" width="10" height="10" fill="#334155"/>
                <rect x="120" y="65" width="10" height="10" fill="#334155"/>
                
                <circle cx="150" cy="110" r="25" fill="#475569" stroke="#0f172a" stroke-width="3"/>
                <circle cx="150" cy="110" r="6" fill="#1e293b"/>

                <!-- German Printing House poster -->
                <rect x="250" y="25" width="90" height="60" fill="#fef08a" stroke="#451a03" stroke-width="3"/>
                <text x="260" y="42" fill="#451a03" font-size="6" font-family="'Press Start 2P'">BERLINER</text>
                <text x="260" y="55" fill="#451a03" font-size="5" font-family="'Press Start 2P'">DRUCKEREI</text>
                <text x="260" y="68" fill="#16a34a" font-size="5" font-family="'Press Start 2P'">1887</text>

                <!-- Huge pile of printed Noli books -->
                <rect x="290" y="140" width="45" height="30" fill="#15803d" rx="2" stroke="#14532d" stroke-width="2"/>
                <rect x="295" y="130" width="45" height="12" fill="#15803d" rx="2" stroke="#14532d" stroke-width="2"/>
                <!-- Gold Tag on the books -->
                <rect x="312" y="133" width="11" height="6" fill="#fbbf24"/>
                <rect x="305" y="145" width="15" height="10" fill="#f8fafc" rx="1"/> <!-- Title label "NOLI" -->
            </svg>
        `
    };

    function initAudio() {
        if (!audioCtx) {
            audioCtx = new (window.AudioContext || window.webkitAudioContext)();
        }
    }

    function playTone(freq, type, duration, volume = 0.1) {
        if (!soundEnabled || !audioCtx) return;
        
        try {
            let osc = audioCtx.createOscillator();
            let gainNode = audioCtx.createGain();
            
            osc.connect(gainNode);
            gainNode.connect(audioCtx.destination);
            
            osc.type = type; // 'sine', 'square', 'sawtooth', 'triangle'
            osc.frequency.value = freq;
            
            gainNode.gain.setValueAtTime(volume, audioCtx.currentTime);
            gainNode.gain.exponentialRampToValueAtTime(0.00001, audioCtx.currentTime + duration);
            
            osc.start(audioCtx.currentTime);
            osc.stop(audioCtx.currentTime + duration);
        } catch(e) {
            console.error("Audio error: ", e);
        }
    }

    // Interactive Sound Effects
    function playSuccessSFX() {
        playTone(523.25, 'triangle', 0.15, 0.15); // C5
        setTimeout(() => {
            playTone(659.25, 'triangle', 0.15, 0.15); // E5
            setTimeout(() => {
                playTone(783.99, 'triangle', 0.3, 0.2); // G5
            }, 100);
        }, 100);
    }

    function playFailureSFX() {
        playTone(293.66, 'sawtooth', 0.2, 0.15); // D4
        setTimeout(() => {
            playTone(220.00, 'sawtooth', 0.4, 0.2); // A3
        }, 150);
    }

    function playLevelUpSFX() {
        playTone(523.25, 'square', 0.1, 0.1);
        setTimeout(() => playTone(587.33, 'square', 0.1, 0.1), 80);
        setTimeout(() => playTone(659.25, 'square', 0.1, 0.1), 160);
        setTimeout(() => playTone(698.46, 'square', 0.1, 0.1), 240);
        setTimeout(() => playTone(783.99, 'square', 0.15, 0.1), 320);
        setTimeout(() => playTone(880.00, 'square', 0.15, 0.1), 400);
        setTimeout(() => playTone(987.77, 'square', 0.4, 0.2), 480);
    }

    // Pleasant background chiptune melody (Kundiman-esque nostalgic feel)
    const melodyNotes = [
        329.63, 392.00, 440.00, 523.25, 493.88, 440.00, 392.00, 440.00,
        329.63, 349.23, 392.00, 440.00, 392.00, 349.23, 329.63, 293.66
    ];
    let melodyIndex = 0;

    function startMelodyLoop() {
        if (soundLoopId) clearInterval(soundLoopId);
        
        soundLoopId = setInterval(() => {
            if (soundEnabled && audioCtx) {
                let note = melodyNotes[melodyIndex];
                playTone(note, 'sine', 0.6, 0.04);
                melodyIndex = (melodyIndex + 1) % melodyNotes.length;
            }
        }, 650);
    }

    function toggleMusic() {
        soundEnabled = !soundEnabled;
        const icon = document.getElementById('sound-icon');
        const btn = document.getElementById('sound-toggle');
        
        if (soundEnabled) {
            initAudio();
            if (audioCtx && audioCtx.state === 'suspended') {
                audioCtx.resume();
            }
            icon.className = "fa-solid fa-volume-high text-emerald-400";
            btn.classList.add('border-emerald-500');
            startMelodyLoop();
            // Initial feedback sound
            playTone(440, 'sine', 0.1, 0.05);
        } else {
            icon.className = "fa-solid fa-volume-xmark text-slate-400";
            btn.classList.remove('border-emerald-500');
            if (soundLoopId) {
                clearInterval(soundLoopId);
                soundLoopId = null;
            }
        }
    }

    window.onload = function() {
        // Draw Promo Character Avatars on Splash screen
        document.getElementById('avatar-ibarra-promo').innerHTML = SVG_AVATARS.ibarra;
        document.getElementById('avatar-maria-promo').innerHTML = SVG_AVATARS.maria;
        
        // Setup initial UI states
        updateHeartsUI();
        loadHighScore();
    }

    function loadHighScore() {
        let saved = localStorage.getItem('nolituklasan_highscore');
        if (saved) {
            document.getElementById('score-counter').innerText = String(saved).padStart(4, '0');
            score = parseInt(saved);
        }
    }

    function saveHighScore() {
        localStorage.setItem('nolituklasan_highscore', score);
    }

    function backToSplash() {
        document.getElementById('screen-level-select').classList.add('hidden');
        document.getElementById('screen-gameplay').classList.add('hidden');
        document.getElementById('screen-game-over').classList.add('hidden');
        document.getElementById('screen-splash').classList.remove('hidden');
        playTone(392, 'triangle', 0.1, 0.1);
    }

    function startGame() {
        document.getElementById('screen-splash').classList.add('hidden');
        document.getElementById('screen-level-select').classList.remove('hidden');
        playTone(523.25, 'triangle', 0.1, 0.1);
    }

    function selectLevel(lvl) {
        initAudio();
        currentLevel = lvl;
        currentQuestionIndex = 0;
        
        // If they chose level 2 or 3 directly, allow it, but reset standard gameplay attributes
        document.getElementById('screen-level-select').classList.add('hidden');
        document.getElementById('screen-gameplay').classList.remove('hidden');
        
        document.getElementById('header-level-badge').innerText = `Antas: ${GAME_LEVELS_DATA[lvl].title}`;
        
        loadQuestion();
        playTone(659.25, 'square', 0.15, 0.1);
    }

    function loadQuestion() {
        const lvlData = GAME_LEVELS_DATA[currentLevel];
        const question = lvlData.questions[currentQuestionIndex];
        
        // 1. Update scene metadata
        document.getElementById('scene-tag').innerText = lvlData.sceneTag;
        document.getElementById('level-progress-indicator').innerText = `Tanong ${currentQuestionIndex + 1}/${lvlData.questions.length}`;
        
        // 2. Load background design
        document.getElementById('scene-bg-layer').innerHTML = SCENE_TEMPLATES[lvlData.backgroundTemplate];
        
        // 3. Render Avatars
        const leftSvg = document.getElementById('character-left');
        const leftCont = document.getElementById('character-left-container');
        if (question.avatarLeft !== 'none') {
            leftCont.classList.remove('hidden');
            leftSvg.innerHTML = SVG_AVATARS[question.avatarLeft];
            document.getElementById('name-left').innerText = question.character;
        } else {
            leftCont.classList.add('hidden');
        }

        const rightSvg = document.getElementById('character-right');
        const rightCont = document.getElementById('character-right-container');
        if (question.avatarRight !== 'none') {
            rightCont.classList.remove('hidden');
            rightSvg.innerHTML = SVG_AVATARS[question.avatarRight];
            document.getElementById('name-right').innerText = question.avatarRight === 'maria' ? "Maria Clara" : "Maximo Viola";
        } else {
            rightCont.classList.add('hidden');
        }

        // 4. Set Speaker highlighting
        if (question.speaker === 'left') {
            leftCont.classList.add('scale-105', 'border-2', 'border-amber-400', 'rounded-xl');
            rightCont.classList.remove('scale-105', 'border-2', 'border-amber-400', 'rounded-xl');
            document.getElementById('dialogue-character-title').innerText = question.character;
        } else {
            rightCont.classList.add('scale-105', 'border-2', 'border-emerald-400', 'rounded-xl');
            leftCont.classList.remove('scale-105', 'border-2', 'border-emerald-400', 'rounded-xl');
            document.getElementById('dialogue-character-title').innerText = question.avatarRight === 'maria' ? "Maria Clara" : "Maximo Viola";
        }

        // 5. Text content
        document.getElementById('dialogue-text-content').innerText = question.dialogue;

        // 6. Generate Answer options
        const optsContainer = document.getElementById('options-panel-container');
        optsContainer.innerHTML = '';
        
        const labels = ['A', 'B', 'C', 'D'];
        question.options.forEach((opt, idx) => {
            const btn = document.createElement('button');
            btn.className = "bg-slate-800 hover:bg-slate-700 text-slate-100 p-3 rounded-xl text-left text-xs sm:text-sm transition-all border border-slate-700 hover:border-amber-500 flex items-center gap-3 active:scale-[0.99] duration-100";
            btn.onclick = () => submitAnswer(opt);
            
            btn.innerHTML = `
                <span class="pixel-font bg-slate-900 text-amber-400 px-2.5 py-1 rounded border border-slate-700 text-[10px]">${labels[idx]}</span>
                <span class="font-medium">${opt.text}</span>
            `;
            optsContainer.appendChild(btn);
        });

        // 7. Reset Clues indicators for new question
        const cluesCont = document.getElementById('clues-indicator');
        cluesCont.innerHTML = `
            <i class="fa-solid fa-lightbulb text-amber-400 text-xs animate-pulse cursor-pointer" onclick="revealClueTip()" title="Humingi ng tulong!"></i>
        `;
    }

    function revealClueTip() {
        const bubble = document.getElementById('interactive-point-bubble');
        const text = document.getElementById('interactive-point-text');
        
        // Find correct option snippet to make a helper hint
        const currentQ = GAME_LEVELS_DATA[currentLevel].questions[currentQuestionIndex];
        const correctOpt = currentQ.options.find(o => o.correct);
        
        // Simple contextual hint
        let hint = "Pahiwatig: Isipin ang may kinalaman sa ";
        if (currentQ.id === "L1_Q1") hint += "isang sikat na kubo (Cabin) sa Amerika.";
        else if (currentQ.id === "L1_Q2") hint += "sugal, alak at mga layaw.";
        else if (currentQ.id === "L1_Q3") hint += "paghawak o pagsaling sa katawan.";
        else if (currentQ.id === "L2_Q1") hint += "matinding gutom at malamig na panahon.";
        else if (currentQ.id === "L2_Q2") hint += "wikang mauunawaan ng mga pinuno at Kastila.";
        else hint += "halagang higit sa daan ngunit sapat sa libu-libong sipi.";

        text.innerText = hint;
        bubble.classList.remove('hidden');
        bubble.classList.add('scale-100');
        playTone(440, 'sine', 0.2, 0.1);

        // Hide hint after 4 seconds automatically
        setTimeout(() => {
            bubble.classList.remove('scale-100');
            bubble.classList.add('hidden');
        }, 5000);
    }

    function submitAnswer(option) {
        const overlay = document.getElementById('feedback-overlay');
        const card = document.getElementById('feedback-card');
        const iconContainer = document.getElementById('feedback-icon-container');
        const title = document.getElementById('feedback-title');
        const explanation = document.getElementById('feedback-explanation');
        const pointsDelta = document.getElementById('feedback-points-delta');
        const actionBtn = document.getElementById('feedback-action-btn');

        overlay.classList.remove('hidden');
        overlay.classList.add('flex');
        
        if (option.correct) {
            // Correct answer workflow
            playSuccessSFX();
            score += option.points;
            document.getElementById('score-counter').innerText = String(score).padStart(4, '0');
            saveHighScore();

            card.className = "bg-slate-900 border-4 border-emerald-500 rounded-3xl max-w-md w-full p-5 shadow-2xl transform scale-100 transition-all duration-300";
            iconContainer.className = "w-16 h-16 bg-emerald-500/10 border-2 border-emerald-500 text-emerald-400 rounded-full flex items-center justify-center mx-auto mb-4 text-2xl animate-bounce";
            iconContainer.innerHTML = `<i class="fa-solid fa-circle-check"></i>`;
            title.className = "pixel-font text-sm sm:text-base text-emerald-400 uppercase tracking-widest mb-1";
            title.innerText = "Tumpak na Sagot!";
            explanation.innerText = option.expl;
            pointsDelta.innerText = `+${option.points} Puntos!`;
            pointsDelta.className = "text-xs text-emerald-400 font-bold mb-4 flex justify-center items-center gap-2";
            actionBtn.className = "pixel-font text-xs bg-emerald-500 hover:bg-emerald-400 text-slate-950 font-bold w-full py-3.5 rounded-xl transition-all duration-150 uppercase shadow-md";
            actionBtn.innerHTML = `Ipagpatuloy ang Paglalakbay <i class="fa-solid fa-chevron-right ml-1"></i>`;
        } else {
            // Wrong answer workflow
            playFailureSFX();
            lives--;
            totalMistakes++;
            updateHeartsUI();

            card.className = "bg-slate-900 border-4 border-red-500 rounded-3xl max-w-md w-full p-5 shadow-2xl transform scale-100 transition-all duration-300";
            iconContainer.className = "w-16 h-16 bg-red-500/10 border-2 border-red-500 text-red-400 rounded-full flex items-center justify-center mx-auto mb-4 text-2xl animate-pulse";
            iconContainer.innerHTML = `<i class="fa-solid fa-circle-xmark"></i>`;
            title.className = "pixel-font text-sm sm:text-base text-red-400 uppercase tracking-widest mb-1";
            title.innerText = "Napakalapit na Hula!";
            
            // Find correct answer explanation
            const currentQ = GAME_LEVELS_DATA[currentLevel].questions[currentQuestionIndex];
            const correctOpt = currentQ.options.find(o => o.correct);
            explanation.innerHTML = `<span class="text-red-400 font-bold block mb-1">Mali ang napili.</span>${correctOpt.expl}`;
            
            pointsDelta.innerText = "Walang nakuha. Bawasan ng 1 Buhay!";
            pointsDelta.className = "text-xs text-red-400 font-bold mb-4 flex justify-center items-center gap-2";
            actionBtn.className = "pixel-font text-xs bg-red-500 hover:bg-red-400 text-slate-950 font-bold w-full py-3.5 rounded-xl transition-all duration-150 uppercase shadow-md";
            
            if (lives <= 0) {
                actionBtn.innerHTML = `Tapusin ang Laro <i class="fa-solid fa-skull ml-1"></i>`;
            } else {
                actionBtn.innerHTML = `Subukan ang Susunod <i class="fa-solid fa-arrow-rotate-left ml-1"></i>`;
            }
        }
    }

    function updateHeartsUI() {
        const container = document.getElementById('hearts-container');
        container.innerHTML = '';
        for (let i = 0; i < 3; i++) {
            const heart = document.createElement('i');
            if (i < lives) {
                heart.className = "fa-solid fa-heart text-red-500 text-xs sm:text-sm animate-pulse mr-0.5";
            } else {
                heart.className = "fa-regular fa-heart text-slate-600 text-xs sm:text-sm mr-0.5";
            }
            container.appendChild(heart);
        }
    }

    function advanceNext() {
        // Close feedback panel
        document.getElementById('feedback-overlay').classList.add('hidden');
        document.getElementById('feedback-overlay').classList.remove('flex');

        // Check for Game Over condition
        if (lives <= 0) {
            triggerGameOver(false);
            return;
        }

        const lvlData = GAME_LEVELS_DATA[currentLevel];
        currentQuestionIndex++;

        if (currentQuestionIndex < lvlData.questions.length) {
            // Load next question in current level
            loadQuestion();
        } else {
            // Finished current level!
            if (currentLevel < 3) {
                playLevelUpSFX();
                currentLevel++;
                currentQuestionIndex = 0;
                
                // Show transitional message
                const overlay = document.getElementById('feedback-overlay');
                overlay.classList.remove('hidden');
                overlay.classList.add('flex');
                
                document.getElementById('feedback-card').className = "bg-slate-900 border-4 border-yellow-500 rounded-3xl max-w-md w-full p-5 shadow-2xl";
                document.getElementById('feedback-icon-container').className = "w-16 h-16 bg-yellow-500/10 border-2 border-yellow-500 text-yellow-400 rounded-full flex items-center justify-center mx-auto mb-4 text-2xl animate-bounce";
                document.getElementById('feedback-icon-container').innerHTML = `<i class="fa-solid fa-circle-up"></i>`;
                document.getElementById('feedback-title').innerText = "ANTAS NALAMPASAN!";
                document.getElementById('feedback-explanation').innerText = `Tagumpay mong naisakatuparan ang kabanatang ito. Maghanda para sa susunod na Antas: ${GAME_LEVELS_DATA[currentLevel].title}!`;
                document.getElementById('feedback-points-delta').innerText = "Dagdag 150 Puntos para sa Antas bonus!";
                score += 150;
                document.getElementById('score-counter').innerText = String(score).padStart(4, '0');
                saveHighScore();

                document.getElementById('feedback-action-btn').onclick = function() {
                    document.getElementById('feedback-overlay').classList.add('hidden');
                    document.getElementById('feedback-overlay').classList.remove('flex');
                    document.getElementById('header-level-badge').innerText = `Antas: ${GAME_LEVELS_DATA[currentLevel].title}`;
                    loadQuestion();
                };
            } else {
                // Completed all levels successfully!
                triggerGameOver(true);
            }
        }
    }

    function triggerGameOver(isVictory) {
        document.getElementById('screen-gameplay').classList.add('hidden');
        document.getElementById('screen-game-over').classList.remove('hidden');

        const title = document.getElementById('gameover-title');
        const desc = document.getElementById('gameover-desc');
        const badge = document.getElementById('gameover-visual-badge');

        if (isVictory) {
            playLevelUpSFX();
            badge.className = "w-20 h-20 bg-emerald-500/10 border-4 border-emerald-500 text-emerald-400 rounded-full flex items-center justify-center mb-4 text-4xl animate-bounce";
            badge.innerHTML = `<i class="fa-solid fa-trophy"></i>`;
            title.innerText = "MALIPAYONG PAGTATAPOS!";
            title.className = "pixel-font text-xl sm:text-2xl text-emerald-400 drop-shadow uppercase mb-2";
            desc.innerText = "Binabati kita! Matagumpay mong napatunayan ang iyong galing sa kasaysayan ng Noli Me Tangere. Matatag mong hinarap ang krisis at naitaguyod ang dakilang pangarap ni Rizal.";
        } else {
            playFailureSFX();
            badge.className = "w-20 h-20 bg-red-500/10 border-4 border-red-500 text-red-500 rounded-full flex items-center justify-center mb-4 text-4xl animate-pulse";
            badge.innerHTML = `<i class="fa-solid fa-skull"></i>`;
            title.innerText = "GABING MADILIM!";
            title.className = "pixel-font text-xl sm:text-2xl text-red-500 drop-shadow uppercase mb-2";
            desc.innerText = "Naubos ang iyong buhay. Huwag panghinaan ng loob! Basahing muli ang Aklat ng Kasaysayan at maglaro muli upang mailigtas ang Noli.";
        }

        document.getElementById('final-score').innerText = String(score).padStart(4, '0');
        document.getElementById('final-mistakes').innerText = totalMistakes;
    }

    function restartGame() {
        lives = 3;
        score = 0;
        totalMistakes = 0;
        currentLevel = 1;
        currentQuestionIndex = 0;
        updateHeartsUI();
        document.getElementById('score-counter').innerText = "0000";
        document.getElementById('screen-game-over').classList.add('hidden');
        document.getElementById('screen-level-select').classList.remove('hidden');
        playTone(523.25, 'triangle', 0.15, 0.1);
    }

    function openLoreModal() {
        document.getElementById('lore-modal').classList.remove('hidden');
        document.getElementById('lore-modal').classList.add('flex');
        playTone(440, 'triangle', 0.1, 0.1);
    }

    function closeLoreModal() {
        document.getElementById('lore-modal').classList.add('hidden');
        document.getElementById('lore-modal').classList.remove('flex');
        playTone(329.63, 'triangle', 0.1, 0.1);
    }
</script>
</body>
</html>
