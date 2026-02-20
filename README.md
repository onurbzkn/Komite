<!DOCTYPE html>
<html lang="tr">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
    <title>Komite App V10</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <link href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.0.0/css/all.min.css" rel="stylesheet">
    <style>
        :root { --bg-color: #09090b; --card-bg: #18181b; --accent: #3b82f6; }
        body { background-color: var(--bg-color); color: white; font-family: -apple-system, BlinkMacSystemFont, "Segoe UI", Roboto, Helvetica, Arial, sans-serif; overflow-x: hidden; }
        .glass { background: rgba(24, 24, 27, 0.8); backdrop-filter: blur(12px); border: 1px solid rgba(255,255,255,0.1); }
        .tab-bar { position: fixed; bottom: 20px; left: 50%; transform: translateX(-50%); width: 90%; max-width: 400px; border-radius: 30px; display: flex; justify-content: space-around; padding: 12px; z-index: 1000; }
        .content-section { display: none; padding-bottom: 120px; }
        .content-section.active { display: block; animation: fadeIn 0.3s ease; }
        @keyframes fadeIn { from { opacity: 0; transform: translateY(10px); } to { opacity: 1; transform: translateY(0); } }
        .flip-card { perspective: 1000px; height: 220px; }
        .flip-card-inner { position: relative; width: 100%; height: 100%; transition: transform 0.6s; transform-style: preserve-3d; }
        .flip-card.flipped .flip-card-inner { transform: rotateY(180deg); }
        .flip-card-front, .flip-card-back { position: absolute; width: 100%; height: 100%; backface-visibility: hidden; border-radius: 24px; display: flex; align-items: center; justify-content: center; padding: 20px; font-size: 1.2rem; text-align: center; }
        .flip-card-back { transform: rotateY(180deg); background: #27272a; }
    </style>
</head>
<body>

<div class="p-6">
    <header class="flex justify-between items-center mb-8">
        <div>
            <h1 class="text-3xl font-extrabold tracking-tight">FOCUS</h1>
            <p class="text-xs text-gray-500 font-bold uppercase tracking-widest">Medikal Asistan</p>
        </div>
        <div class="w-10 h-10 rounded-full bg-blue-600 flex items-center justify-center">
            <i class="fas fa-user text-sm"></i>
        </div>
    </header>

    <section id="focus" class="content-section active">
        <div class="glass rounded-[32px] p-10 text-center mb-6">
            <p class="text-xs font-bold text-gray-400 uppercase mb-4 tracking-widest">Pomodoro</p>
            <div id="timer" class="text-8xl font-black mb-8 tracking-tighter">25:00</div>
            <button onclick="startTimer()" id="startBtn" class="w-full bg-blue-600 hover:bg-blue-700 py-4 rounded-2xl font-bold text-lg transition-all active:scale-95">BAŞLAT</button>
            <button onclick="resetTimer()" class="mt-4 text-gray-500 font-bold text-sm uppercase">Sıfırla</button>
        </div>
        
        <div class="glass rounded-[32px] p-6 text-center">
            <p class="text-xs font-bold text-gray-400 uppercase mb-2 tracking-widest">Kronometre</p>
            <div id="stopwatch" class="text-4xl font-mono font-bold mb-4">00:00.00</div>
            <button onclick="toggleStopwatch()" id="swBtn" class="w-full bg-emerald-600 py-3 rounded-xl font-bold transition-all active:scale-95">BAŞLAT</button>
        </div>
    </section>

    <section id="komite" class="content-section">
        <div class="glass rounded-[32px] p-8 mb-6">
            <h2 class="text-xl font-bold mb-6">Sınav Geri Sayımı</h2>
            <input type="date" id="examDate" class="w-full bg-zinc-800 p-4 rounded-2xl border-none text-white mb-6 outline-none">
            <div id="countdown" class="text-4xl font-black text-blue-500 text-center py-4">Tarih Seçin</div>
        </div>
    </section>

    <section id="ezber" class="content-section">
        <div id="flashcard" class="flip-card mb-8" onclick="this.classList.toggle('flipped')">
            <div class="flip-card-inner">
                <div class="flip-card-front bg-gradient-to-br from-blue-700 to-blue-900 shadow-2xl shadow-blue-900/20">
                    <p id="cardFrontText">Dokun ve Öğren</p>
                </div>
                <div class="flip-card-back shadow-2xl">
                    <p id="cardBackText">Cevap burada görünecek.</p>
                </div>
            </div>
        </div>
        <div class="glass rounded-[32px] p-6 space-y-3">
            <input id="qInput" placeholder="Soru yaz..." class="w-full bg-zinc-800 p-4 rounded-xl outline-none border-none">
            <input id="aInput" placeholder="Cevap yaz..." class="w-full bg-zinc-800 p-4 rounded-xl outline-none border-none">
            <button onclick="addCard()" class="w-full bg-blue-600 py-4 rounded-xl font-bold uppercase tracking-widest">Kartı Kaydet</button>
        </div>
    </section>

    <section id="ayarlar" class="content-section">
        <div class="glass rounded-[32px] p-6">
            <h2 class="text-xl font-bold mb-4">Uygulama Ayarları</h2>
            <div class="flex justify-between items-center py-4 border-b border-zinc-800">
                <span>Karanlık Mod</span>
                <div class="w-12 h-6 bg-blue-600 rounded-full relative"><div class="absolute right-1 top-1 w-4 h-4 bg-white rounded-full"></div></div>
            </div>
            <div class="py-4 text-sm text-gray-500">Versiyon 10.0 - Ustam Edition</div>
        </div>
    </section>
</div>

<nav class="tab-bar glass">
    <button onclick="showSection('focus')" class="flex flex-col items-center text-blue-500" id="nav-focus">
        <i class="fas fa-stopwatch text-xl mb-1"></i>
        <span class="text-[10px] font-bold uppercase">Focus</span>
    </button>
    <button onclick="showSection('komite')" class="flex flex-col items-center text-gray-500" id="nav-komite">
        <i class="fas fa-calendar-alt text-xl mb-1"></i>
        <span class="text-[10px] font-bold uppercase">Komite</span>
    </button>
    <button onclick="showSection('ezber')" class="flex flex-col items-center text-gray-500" id="nav-ezber">
        <i class="fas fa-brain text-xl mb-1"></i>
        <span class="text-[10px] font-bold uppercase">Ezber</span>
    </button>
    <button onclick="showSection('ayarlar')" class="flex flex-col items-center text-gray-500" id="nav-ayarlar">
        <i class="fas fa-cog text-xl mb-1"></i>
        <span class="text-[10px] font-bold uppercase">Ayarlar</span>
    </button>
</nav>

<script>
    // NAVIGATION
    function showSection(id) {
        document.querySelectorAll('.content-section').forEach(s => s.classList.remove('active'));
        document.getElementById(id).classList.add('active');
        document.querySelectorAll('.tab-bar button').forEach(b => b.classList.replace('text-blue-500', 'text-gray-500'));
        document.getElementById('nav-' + id).classList.replace('text-gray-500', 'text-blue-500');
    }

    // POMODORO LOGIC
    let timeLeft = 25 * 60, timerId = null;
    function updateTimer() {
        let m = Math.floor(timeLeft / 60), s = timeLeft % 60;
        document.getElementById('timer').innerText = `${m}:${s < 10 ? '0' : ''}${s}`;
    }
    function startTimer() {
        if (!timerId) {
            timerId = setInterval(() => {
                timeLeft--; updateTimer();
                if (timeLeft <= 0) { clearInterval(timerId); alert("Süre bitti Hocam!"); }
            }, 1000);
            document.getElementById('startBtn').innerText = "DURAKLAT";
        } else {
            clearInterval(timerId); timerId = null;
            document.getElementById('startBtn').innerText = "DEVAM ET";
        }
    }
    function resetTimer() {
        clearInterval(timerId); timerId = null; timeLeft = 25 * 60; updateTimer();
        document.getElementById('startBtn').innerText = "BAŞLAT";
    }

    // KRONOMETRE LOGIC
    let swTime = 0, swId = null;
    function toggleStopwatch() {
        if(!swId) {
            swId = setInterval(() => {
                swTime += 10;
                let d = new Date(swTime);
                document.getElementById('stopwatch').innerText = d.toISOString().substr(14, 8);
            }, 10);
            document.getElementById('swBtn').innerText = "DURDUR";
        } else {
            clearInterval(swId); swId = null;
            document.getElementById('swBtn').innerText = "BAŞLAT";
        }
    }

    // DATA PERSISTENCE
    const examInput = document.getElementById('examDate');
    if(localStorage.getItem('komiteTarihi')) {
        examInput.value = localStorage.getItem('komiteTarihi');
        setInterval(updateCountdown, 1000);
    }
    examInput.addEventListener('change', () => {
        localStorage.setItem('komiteTarihi', examInput.value);
        setInterval(updateCountdown, 1000);
    });
    function updateCountdown() {
        const diff = new Date(examInput.value).getTime() - new Date().getTime();
        if(diff < 0) return;
        const d = Math.floor(diff / 86400000), h = Math.floor((diff % 86400000) / 3600000);
        document.getElementById('countdown').innerText = `${d} GÜN ${h} SAAT`;
    }

    function addCard() {
        const q = document.getElementById('qInput').value, a = document.getElementById('aInput').value;
        if(q && a) {
            localStorage.setItem('card', JSON.stringify({q, a}));
            document.getElementById('cardFrontText').innerText = q;
            document.getElementById('cardBackText').innerText = a;
            document.getElementById('qInput').value = ''; document.getElementById('aInput').value = '';
        }
    }
    const saved = JSON.parse(localStorage.getItem('card'));
    if(saved) { document.getElementById('cardFrontText').innerText = saved.q; document.getElementById('cardBackText').innerText = saved.a; }
</script>
</body>
</html>
