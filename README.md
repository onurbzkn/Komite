<!DOCTYPE html>
<html lang="tr">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
    <title>Focus App v10.2</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <link href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.0.0/css/all.min.css" rel="stylesheet">
    <style>
        :root { --bg: #09090b; --accent: #3b82f6; }
        body { background-color: var(--bg); color: white; font-family: -apple-system, system-ui, sans-serif; overflow: hidden; }
        
        /* iOS 26 Liquid Glass Navigation Bar */
        .liquid-nav {
            position: fixed; bottom: 30px; left: 50%; transform: translateX(-50%);
            width: 85%; max-width: 400px; height: 70px;
            background: rgba(255, 255, 255, 0.08);
            backdrop-filter: blur(25px) saturate(180%);
            -webkit-backdrop-filter: blur(25px) saturate(180%);
            border: 1px solid rgba(255, 255, 255, 0.15);
            border-radius: 35px; display: flex; justify-content: space-around; align-items: center;
            z-index: 1000; box-shadow: 0 20px 40px rgba(0,0,0,0.4);
        }

        /* Dinamik Buton Efektleri */
        .btn-ios {
            transition: all 0.2s cubic-bezier(0.4, 0, 0.2, 1);
            position: relative; overflow: hidden;
        }
        .btn-ios:active { transform: scale(0.92); filter: brightness(1.2); }

        .glass-card {
            background: rgba(255, 255, 255, 0.04);
            border: 1px solid rgba(255, 255, 255, 0.1);
            border-radius: 32px; backdrop-filter: blur(10px);
        }

        .content-section { display: none; height: 100vh; overflow-y: auto; padding-bottom: 150px; }
        .active-section { display: block; animation: sectionFade 0.4s ease-out; }
        @keyframes sectionFade { from { opacity: 0; transform: scale(0.98); } to { opacity: 1; transform: scale(1); } }

        .nav-item { transition: all 0.3s ease; color: #888; }
        .nav-item.active { color: white; transform: translateY(-5px); }
    </style>
</head>
<body class="p-6">

    <header class="flex justify-between items-center mb-10 pt-4">
        <div>
            <h1 class="text-3xl font-black tracking-tighter" id="pageTitle">ODAK</h1>
            <p class="text-[10px] text-blue-500 font-bold uppercase tracking-[0.2em]">Medikal Asistan</p>
        </div>
        <div class="w-10 h-10 rounded-full bg-gradient-to-tr from-blue-600 to-indigo-600 flex items-center justify-center btn-ios shadow-lg">
            <i class="fas fa-user-md"></i>
        </div>
    </header>

    <section id="odak" class="content-section active-section">
        <div class="glass-card p-8 text-center mb-6">
            <span class="text-[10px] font-bold text-gray-500 uppercase tracking-widest">Pomodoro</span>
            <div id="timer" class="text-7xl font-black my-6 tracking-tighter">25:00</div>
            <div class="flex flex-col gap-3">
                <button onclick="startTimer()" id="startBtn" class="btn-ios bg-blue-600 py-4 rounded-2xl font-bold text-lg shadow-lg shadow-blue-900/20">BAŞLAT</button>
                <button onclick="resetTimer()" class="btn-ios text-gray-500 font-bold text-xs uppercase tracking-widest">Sıfırla</button>
            </div>
        </div>

        <div class="glass-card p-6 text-center">
            <span class="text-[10px] font-bold text-gray-500 uppercase tracking-widest">Kronometre</span>
            <div id="stopwatch" class="text-4xl font-mono font-bold my-4">00:00.00</div>
            <div class="flex gap-4">
                <button onclick="toggleStopwatch()" id="swBtn" class="btn-ios flex-1 bg-emerald-600 py-3 rounded-2xl font-bold">BAŞLAT</button>
                <button onclick="resetStopwatch()" class="btn-ios flex-1 bg-zinc-800 py-3 rounded-2xl font-bold">SIFIRLA</button>
            </div>
        </div>
    </section>

    <section id="komite" class="content-section">
        <div class="glass-card p-8">
            <h3 class="text-sm font-bold text-gray-400 mb-6 text-center">Zamanlayıcı için Komite tarihini giriniz.</h3>
            <input type="date" id="examDate" class="w-full bg-zinc-900 p-5 rounded-2xl border border-zinc-800 text-white mb-8 outline-none focus:border-blue-500 transition-all">
            <div id="countdown" class="text-5xl font-black text-center text-transparent bg-clip-text bg-gradient-to-b from-white to-zinc-500 py-6">Tarih Seçin</div>
        </div>
    </section>

    <section id="ezber" class="content-section">
        <div class="glass-card p-8 mb-6 text-center min-h-[200px] flex items-center justify-center btn-ios bg-gradient-to-br from-blue-900/20 to-transparent" onclick="flipCard()">
            <p id="cardDisplay" class="text-xl font-medium px-4">Dokun ve Öğren</p>
        </div>
        <div class="glass-card p-6 space-y-4">
            <input id="qInput" placeholder="Soru..." class="w-full bg-zinc-900 p-4 rounded-xl outline-none">
            <input id="aInput" placeholder="Cevap..." class="w-full bg-zinc-900 p-4 rounded-xl outline-none">
            <button onclick="addCard()" class="btn-ios w-full bg-blue-600 py-4 rounded-xl font-bold uppercase tracking-widest">Kartı Kaydet</button>
        </div>
    </section>

    <section id="ayarlar" class="content-section">
        <div class="glass-card p-6 mb-4">
            <h2 class="font-bold mb-6">Uygulama Teması</h2>
            <div class="grid grid-cols-2 gap-4">
                <button class="btn-ios p-4 rounded-2xl bg-blue-600/20 border border-blue-500/50 text-xs font-bold">Gece Mavisi</button>
                <button class="btn-ios p-4 rounded-2xl bg-zinc-800 border border-zinc-700 text-xs font-bold text-gray-400">Saf Siyah</button>
            </div>
        </div>
        <div class="glass-card p-6">
            <div class="flex justify-between items-center py-4 border-b border-zinc-800/50">
                <span class="text-sm font-bold">Karanlık Mod</span>
                <div class="w-12 h-6 bg-blue-600 rounded-full flex items-center px-1"><div class="w-4 h-4 bg-white rounded-full ml-auto"></div></div>
            </div>
            <button onclick="clearAllData()" class="btn-ios w-full mt-10 bg-red-900/20 border border-red-500/30 text-red-500 py-4 rounded-2xl font-bold text-sm">
                VERİLERİMİ SIFIRLA
            </button>
            <p class="text-center text-[10px] text-gray-600 mt-6">Version 10.2 - Ustam Edition</p>
        </div>
    </section>

    <nav class="liquid-nav">
        <button onclick="nav('odak')" class="nav-item active flex flex-col items-center" id="btn-odak">
            <i class="fas fa-clock text-xl"></i><span class="text-[8px] font-bold mt-1">ODAK</span>
        </button>
        <button onclick="nav('komite')" class="nav-item flex flex-col items-center" id="btn-komite">
            <i class="fas fa-calendar-day text-xl"></i><span class="text-[8px] font-bold mt-1">KOMİTE</span>
        </button>
        <button onclick="nav('ezber')" class="nav-item flex flex-col items-center" id="btn-ezber">
            <i class="fas fa-brain text-xl"></i><span class="text-[8px] font-bold mt-1">EZBER</span>
        </button>
        <button onclick="nav('ayarlar')" class="nav-item flex flex-col items-center" id="btn-ayarlar">
            <i class="fas fa-sliders-h text-xl"></i><span class="text-[8px] font-bold mt-1">AYARLAR</span>
        </button>
    </nav>

    <script>
        // Navigation Logic
        function nav(id) {
            document.querySelectorAll('.content-section').forEach(s => s.classList.remove('active-section'));
            document.getElementById(id).classList.add('active-section');
            document.querySelectorAll('.nav-item').forEach(i => i.classList.remove('active'));
            document.getElementById('btn-' + id).classList.add('active');
            document.getElementById('pageTitle').innerText = id.toUpperCase();
        }

        // Pomodoro Logic
        let timeLeft = 25 * 60, timerId = null;
        function updateTimer() {
            let m = Math.floor(timeLeft / 60), s = timeLeft % 60;
            document.getElementById('timer').innerText = `${m}:${s < 10 ? '0' : ''}${s}`;
        }
        function startTimer() {
            const btn = document.getElementById('startBtn');
            if (!timerId) {
                timerId = setInterval(() => { timeLeft--; updateTimer(); if(timeLeft<=0) clearInterval(timerId); }, 1000);
                btn.innerText = "DURAKLAT"; btn.classList.replace('bg-blue-600', 'bg-zinc-700');
            } else {
                clearInterval(timerId); timerId = null;
                btn.innerText = "DEVAM ET"; btn.classList.replace('bg-zinc-700', 'bg-blue-600');
            }
        }
        function resetTimer() { clearInterval(timerId); timerId = null; timeLeft = 25 * 60; updateTimer(); document.getElementById('startBtn').innerText = "BAŞLAT"; }

        // Kronometre Logic
        let swTime = 0, swId = null;
        function toggleStopwatch() {
            const btn = document.getElementById('swBtn');
            if(!swId) {
                swId = setInterval(() => { swTime += 10; let d = new Date(swTime); document.getElementById('stopwatch').innerText = d.toISOString().substr(14, 8); }, 10);
                btn.innerText = "DURDUR"; btn.classList.replace('bg-emerald-600', 'bg-zinc-700');
            } else {
                clearInterval(swId); swId = null;
                btn.innerText = "DEVAM ET"; btn.classList.replace('bg-zinc-700', 'bg-emerald-600');
            }
        }
        function resetStopwatch() { clearInterval(swId); swId = null; swTime = 0; document.getElementById('stopwatch').innerText = "00:00.00"; document.getElementById('swBtn').innerText = "BAŞLAT"; }

        // Flashcards
        let currentCard = JSON.parse(localStorage.getItem('card')) || {q: "Dokun ve Öğren", a: "Cevap burada!"};
        let isFront = true;
        function flipCard() {
            isFront = !isFront;
            document.getElementById('cardDisplay').innerText = isFront ? currentCard.q : currentCard.a;
        }
        function addCard() {
            const q = document.getElementById('qInput').value, a = document.getElementById('aInput').value;
            if(q && a) { currentCard = {q, a}; localStorage.setItem('card', JSON.stringify(currentCard)); flipCard(); }
        }

        // Komite Countdown
        const examInput = document.getElementById('examDate');
        if(localStorage.getItem('target')) { examInput.value = localStorage.getItem('target'); setInterval(updateCount, 1000); }
        examInput.addEventListener('change', () => { localStorage.setItem('target', examInput.value); setInterval(updateCount, 1000); });
        function updateCount() {
            const d = new Date(examInput.value).getTime() - new Date().getTime();
            if(d<0) return;
            document.getElementById('countdown').innerText = Math.floor(d/86400000) + " GÜN";
        }

        function clearAllData() { if(confirm("Tüm veriler sıfırlansın mı?")) { localStorage.clear(); location.reload(); } }
    </script>
</body>
</html>
