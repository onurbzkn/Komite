<!DOCTYPE html>
<html lang="tr">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
    <title>Focus App v11</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <link href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.0.0/css/all.min.css" rel="stylesheet">
    <style>
        :root { --bg: #09090b; --accent: #3b82f6; }
        
        body { 
            background-color: var(--bg); 
            color: white; 
            font-family: -apple-system, sans-serif; 
            margin: 0; padding: 0;
            overflow-x: hidden;
            overflow-y: auto !important; /* Kaydırma artık özgür */
        }

        /* Liquid Glass Nav Bar - Dinamik ve Efektli */
        .liquid-nav {
            position: fixed; bottom: 30px; left: 50%; transform: translateX(-50%);
            width: 85%; max-width: 400px; height: 75px;
            background: rgba(255, 255, 255, 0.08);
            backdrop-filter: blur(25px) saturate(180%);
            -webkit-backdrop-filter: blur(25px) saturate(180%);
            border: 1px solid rgba(255, 255, 255, 0.15);
            border-radius: 38px; display: flex; justify-content: space-around; align-items: center;
            z-index: 9999; box-shadow: 0 20px 50px rgba(0,0,0,0.5);
        }

        /* Aktif Navigasyon İkonu Efekti */
        .nav-item { transition: all 0.4s cubic-bezier(0.175, 0.885, 0.32, 1.275); color: #666; cursor: pointer; }
        .nav-item.active { color: white; transform: translateY(-8px) scale(1.1); }
        .nav-item i { font-size: 22px; }

        /* Buton Basma Efekti (Active Scale) */
        .btn-ios { transition: all 0.2s ease; }
        .btn-ios:active { transform: scale(0.94); filter: brightness(1.1); }

        .glass-card {
            background: rgba(255, 255, 255, 0.04);
            border: 1px solid rgba(255, 255, 255, 0.08);
            border-radius: 35px; padding: 30px; margin-bottom: 25px;
            backdrop-filter: blur(10px);
        }

        .page-content { display: none; padding: 110px 20px 160px 20px; animation: slideUp 0.4s ease-out; }
        .page-content.active { display: block; }

        @keyframes slideUp { from { opacity: 0; transform: translateY(20px); } to { opacity: 1; transform: translateY(0); } }
    </style>
</head>
<body>

    <div class="fixed top-0 w-full z-[100] bg-[#09090b]/90 backdrop-blur-xl px-6 py-6 border-b border-white/5">
        <div class="flex justify-between items-center max-w-md mx-auto">
            <div>
                <h1 id="headerTitle" class="text-3xl font-black tracking-tighter m-0">ODAK</h1>
                <p class="text-[10px] text-blue-500 font-bold uppercase tracking-[0.2em] m-0">MEDİKAL ASİSTAN</p>
            </div>
            <div class="w-11 h-11 rounded-full bg-gradient-to-tr from-blue-600 to-indigo-600 flex items-center justify-center btn-ios shadow-lg shadow-blue-500/20">
                <i class="fas fa-user-md text-white"></i>
            </div>
        </div>
    </div>

    <div id="page-odak" class="page-content active max-w-md mx-auto">
        <div class="glass-card text-center">
            <p class="text-[10px] font-bold text-gray-500 uppercase tracking-widest mb-4">POMODORO</p>
            <div id="timer" class="text-8xl font-black mb-8 tracking-tighter">25:00</div>
            <button onclick="startTimer()" id="startBtn" class="btn-ios w-full bg-blue-600 py-5 rounded-2xl font-bold text-lg shadow-xl shadow-blue-900/30">BAŞLAT</button>
            <button onclick="resetTimer()" class="mt-6 text-gray-500 font-bold text-xs uppercase tracking-widest border-none bg-none">SIFIRLA</button>
        </div>

        <div class="glass-card text-center">
            <p class="text-[10px] font-bold text-gray-500 uppercase tracking-widest mb-4">KRONOMETRE</p>
            <div id="stopwatch" class="text-4xl font-mono font-bold mb-6">00:00.00</div>
            <div class="flex gap-4">
                <button onclick="toggleStopwatch()" id="swBtn" class="btn-ios flex-[2] bg-emerald-600 py-4 rounded-2xl font-bold shadow-lg shadow-emerald-900/20">BAŞLAT</button>
                <button onclick="resetStopwatch()" class="btn-ios flex-[1] bg-zinc-800 py-4 rounded-2xl font-bold">SIFIRLA</button>
            </div>
        </div>
    </div>

    <div id="page-komite" class="page-content max-w-md mx-auto">
        <div class="glass-card">
            <h3 class="text-sm font-bold text-gray-400 mb-6 text-center">Zamanlayıcı için Komite tarihini giriniz.</h3>
            <input type="date" id="examDate" class="w-full bg-zinc-900/50 p-5 rounded-2xl border border-zinc-800 text-white mb-8 outline-none focus:border-blue-500 transition-all">
            <div id="countdown" class="text-6xl font-black text-center text-transparent bg-clip-text bg-gradient-to-b from-white to-zinc-500 py-8">TARİH SEÇİN</div>
        </div>
    </div>

    <div id="page-ezber" class="page-content max-w-md mx-auto">
        <div class="glass-card min-h-[240px] flex items-center justify-center btn-ios bg-gradient-to-br from-blue-900/20 to-transparent mb-8" onclick="flipCard()">
            <p id="cardDisplay" class="text-2xl font-semibold px-6 text-center leading-tight">Dokun ve Öğren</p>
        </div>
        <div class="glass-card space-y-4">
            <input id="qInput" placeholder="Soru..." class="w-full bg-zinc-900 p-5 rounded-2xl outline-none border border-zinc-800">
            <input id="aInput" placeholder="Cevap..." class="w-full bg-zinc-900 p-5 rounded-2xl outline-none border border-zinc-800">
            <button onclick="addCard()" class="btn-ios w-full bg-blue-600 py-5 rounded-2xl font-bold uppercase tracking-widest">KARTI KAYDET</button>
        </div>
    </div>

    <div id="page-ayarlar" class="page-content max-w-md mx-auto">
        <div class="glass-card">
            <p class="font-bold mb-6">Uygulama Teması</p>
            <div class="flex gap-4">
                <div class="flex-1 h-14 bg-blue-600/20 rounded-2xl border-2 border-blue-500"></div>
                <div class="flex-1 h-14 bg-black rounded-2xl border border-zinc-800"></div>
            </div>
        </div>
        <div class="glass-card">
            <div class="flex justify-between items-center py-4 border-b border-white/5 mb-8">
                <span class="font-bold">Karanlık Mod</span>
                <div class="w-12 h-6 bg-blue-600 rounded-full flex items-center px-1"><div class="w-4 h-4 bg-white rounded-full ml-auto shadow-md"></div></div>
            </div>
            <button onclick="clearData()" class="btn-ios w-full py-5 bg-red-900/10 border border-red-500/30 text-red-500 rounded-2xl font-bold">VERİLERİMİ SIFIRLA</button>
            <p class="text-center text-[10px] text-gray-600 mt-8 font-bold">V11.0 - USTAM EDITION</p>
        </div>
    </div>

    <nav class="liquid-nav">
        <div onclick="showPage('odak')" id="nav-odak" class="nav-item active flex flex-col items-center">
            <i class="fas fa-stopwatch"></i><span class="text-[9px] font-black mt-1">ODAK</span>
        </div>
        <div onclick="showPage('komite')" id="nav-komite" class="nav-item flex flex-col items-center">
            <i class="fas fa-calendar-alt"></i><span class="text-[9px] font-black mt-1">KOMİTE</span>
        </div>
        <div onclick="showPage('ezber')" id="nav-ezber" class="nav-item flex flex-col items-center">
            <i class="fas fa-brain"></i><span class="text-[9px] font-black mt-1">EZBER</span>
        </div>
        <div onclick="showPage('ayarlar')" id="nav-ayarlar" class="nav-item flex flex-col items-center">
            <i class="fas fa-sliders-h"></i><span class="text-[9px] font-black mt-1">AYARLAR</span>
        </div>
    </nav>

    <script>
        function showPage(id) {
            document.querySelectorAll('.page-content').forEach(p => p.classList.remove('active'));
            document.getElementById('page-' + id).classList.add('active');
            document.querySelectorAll('.nav-item').forEach(n => n.classList.remove('active'));
            document.getElementById('nav-' + id).classList.add('active');
            document.getElementById('headerTitle').innerText = id.toUpperCase();
            window.scrollTo(0, 0);
        }

        // Pomodoro
        let timeLeft = 25 * 60, timerId = null;
        function updateTimer() {
            let m = Math.floor(timeLeft / 60), s = timeLeft % 60;
            document.getElementById('timer').innerText = `${m}:${s < 10 ? '0' : ''}${s}`;
        }
        function startTimer() {
            const btn = document.getElementById('startBtn');
            if (!timerId) {
                timerId = setInterval(() => { timeLeft--; updateTimer(); if(timeLeft<=0) clearInterval(timerId); }, 1000);
                btn.innerText = "DURAKLAT"; btn.classList.replace('bg-blue-600', 'bg-zinc-800');
            } else {
                clearInterval(timerId); timerId = null;
                btn.innerText = "DEVAM ET"; btn.classList.replace('bg-zinc-800', 'bg-blue-600');
            }
        }
        function resetTimer() { clearInterval(timerId); timerId = null; timeLeft = 25 * 60; updateTimer(); document.getElementById('startBtn').innerText = "BAŞLAT"; document.getElementById('startBtn').classList.add('bg-blue-600'); }

        // Kronometre
        let swTime = 0, swId = null;
        function toggleStopwatch() {
            const btn = document.getElementById('swBtn');
            if(!swId) {
                swId = setInterval(() => { swTime += 10; let d = new Date(swTime); document.getElementById('stopwatch').innerText = d.toISOString().substr(14, 8); }, 10);
                btn.innerText = "DURDUR";
            } else {
                clearInterval(swId); swId = null;
                btn.innerText = "DEVAM ET";
            }
        }
        function resetStopwatch() { clearInterval(swId); swId = null; swTime = 0; document.getElementById('stopwatch').innerText = "00:00.00"; document.getElementById('swBtn').innerText = "BAŞLAT"; }

        // Ezber & Komite
        let card = JSON.parse(localStorage.getItem('card')) || {q: "Dokun ve Öğren", a: "Cevap Burada!"};
        let isFront = true;
        function flipCard() { isFront = !isFront; document.getElementById('cardDisplay').innerText = isFront ? card.q : card.a; }
        function addCard() {
            const q = document.getElementById('qInput').value, a = document.getElementById('aInput').value;
            if(q && a) { card = {q, a}; localStorage.setItem('card', JSON.stringify(card)); isFront = true; flipCard(); }
        }

        const eInput = document.getElementById('examDate');
        if(localStorage.getItem('target')) { eInput.value = localStorage.getItem('target'); setInterval(upCount, 1000); }
        eInput.addEventListener('change', () => { localStorage.setItem('target', eInput.value); setInterval(upCount, 1000); });
        function upCount() {
            const d = new Date(eInput.value).getTime() - new Date().getTime();
            if(d > 0) document.getElementById('countdown').innerText = Math.floor(d/86400000) + " GÜN";
        }
        function clearData() { if(confirm("Sıfırlansın mı Hocam?")) { localStorage.clear(); location.reload(); } }
    </script>
</body>
</html>
