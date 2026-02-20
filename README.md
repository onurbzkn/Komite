<!DOCTYPE html>
<html lang="tr">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
    <title>Focus App v12</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <link href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.0.0/css/all.min.css" rel="stylesheet">
    <style>
        :root { --bg: #09090b; --card: rgba(255, 255, 255, 0.04); --accent: #3b82f6; }
        
        /* Temalar */
        .theme-blue { --bg: #0f172a; --accent: #3b82f6; }
        .theme-dark { --bg: #000000; --accent: #ffffff; }
        .theme-green { --bg: #064e3b; --accent: #10b981; }
        .theme-purple { --bg: #2e1065; --accent: #a855f7; }

        body { 
            background-color: var(--bg); 
            color: white; 
            transition: all 0.5s ease;
            font-family: -apple-system, sans-serif; 
            margin: 0; overflow-x: hidden; overflow-y: auto !important;
        }

        .liquid-nav {
            position: fixed; bottom: 30px; left: 50%; transform: translateX(-50%);
            width: 85%; max-width: 400px; height: 75px;
            background: rgba(255, 255, 255, 0.08);
            backdrop-filter: blur(25px) saturate(180%);
            -webkit-backdrop-filter: blur(25px) saturate(180%);
            border: 1px solid rgba(255, 255, 255, 0.15);
            border-radius: 38px; display: flex; justify-content: space-around; align-items: center;
            z-index: 9999;
        }

        .nav-item { transition: all 0.4s cubic-bezier(0.175, 0.885, 0.32, 1.275); color: #666; }
        .nav-item.active { color: var(--accent); transform: translateY(-8px) scale(1.1); }

        .btn-ios { transition: all 0.2s ease; }
        .btn-ios:active { transform: scale(0.94); }

        .glass-card {
            background: var(--card);
            border: 1px solid rgba(255, 255, 255, 0.08);
            border-radius: 35px; padding: 30px; margin-bottom: 25px;
            backdrop-filter: blur(10px);
        }

        .page-content { display: none; padding: 110px 20px 160px 20px; animation: slideUp 0.4s ease-out; }
        .page-content.active { display: block; }

        @keyframes slideUp { from { opacity: 0; transform: translateY(20px); } to { opacity: 1; transform: translateY(0); } }

        /* Tema Butonları */
        .theme-swatch { width: 100%; height: 60px; border-radius: 18px; cursor: pointer; transition: 0.3s; border: 2px solid transparent; }
        .theme-swatch:active { transform: scale(0.9); }
        .theme-swatch.active { border-color: white; }
    </style>
</head>
<body class="theme-blue">

    <div class="fixed top-0 w-full z-[100] bg-inherit backdrop-blur-xl px-6 py-6 border-b border-white/5">
        <div class="flex justify-between items-center max-w-md mx-auto">
            <div>
                <h1 id="headerTitle" class="text-3xl font-black tracking-tighter m-0 uppercase">ODAK</h1>
                <p class="text-[10px] text-blue-500 font-bold uppercase tracking-[0.2em] m-0">MEDİKAL ASİSTAN</p>
            </div>
            <div class="w-11 h-11 rounded-full bg-gradient-to-tr from-blue-600 to-indigo-600 flex items-center justify-center btn-ios">
                <i class="fas fa-user-md text-white"></i>
            </div>
        </div>
    </div>

    <div id="page-odak" class="page-content active max-w-md mx-auto">
        <div class="glass-card text-center">
            <div class="flex justify-center items-center gap-2 mb-4">
                <input type="number" id="pomoInput" value="25" class="bg-white/10 w-16 text-center rounded-lg font-bold outline-none border border-white/10">
                <span class="text-[10px] font-bold text-gray-500 uppercase tracking-widest">Dakika Pomodoro</span>
            </div>
            <div id="timer" class="text-8xl font-black mb-8 tracking-tighter">25:00</div>
            <div class="flex flex-col gap-3">
                <button onclick="startTimer()" id="startBtn" class="btn-ios w-full bg-blue-600 py-5 rounded-2xl font-bold text-lg">BAŞLAT</button>
                <button onclick="setCustomTime()" class="text-[10px] font-bold text-blue-400 uppercase">Süreyi Uygula</button>
                <button onclick="resetTimer()" class="mt-2 text-gray-500 font-bold text-xs uppercase border-none bg-none">SIFIRLA</button>
            </div>
        </div>

        <div class="glass-card text-center">
            <p class="text-[10px] font-bold text-gray-500 uppercase tracking-widest mb-4">KRONOMETRE</p>
            <div id="stopwatch" class="text-4xl font-mono font-bold mb-6">00:00.00</div>
            <div class="flex gap-4">
                <button onclick="toggleStopwatch()" id="swBtn" class="btn-ios flex-[2] bg-emerald-600 py-4 rounded-2xl font-bold">BAŞLAT</button>
                <button onclick="resetStopwatch()" class="btn-ios flex-[1] bg-zinc-800 py-4 rounded-2xl font-bold">SIFIRLA</button>
            </div>
        </div>
    </div>

    <div id="page-komite" class="page-content max-w-md mx-auto">
        <div class="glass-card">
            <h3 class="text-sm font-bold text-gray-400 mb-6 text-center">Komite tarihini giriniz.</h3>
            <input type="date" id="examDate" class="w-full bg-black/20 p-5 rounded-2xl border border-white/10 text-white mb-8 outline-none">
            <div id="countdown" class="text-6xl font-black text-center py-8">TARİH SEÇİN</div>
        </div>
    </div>

    <div id="page-ezber" class="page-content max-w-md mx-auto">
        <div class="glass-card min-h-[240px] flex items-center justify-center btn-ios bg-gradient-to-br from-white/5 to-transparent mb-8" onclick="flipCard()">
            <p id="cardDisplay" class="text-2xl font-semibold px-6 text-center">Dokun ve Öğren</p>
        </div>
        <div class="glass-card space-y-4">
            <input id="qInput" placeholder="Soru..." class="w-full bg-black/20 p-5 rounded-2xl outline-none border border-white/10">
            <input id="aInput" placeholder="Cevap..." class="w-full bg-black/20 p-5 rounded-2xl outline-none border border-white/10">
            <button onclick="addCard()" class="btn-ios w-full bg-blue-600 py-5 rounded-2xl font-bold uppercase">KARTI KAYDET</button>
        </div>
    </div>

    <div id="page-ayarlar" class="page-content max-w-md mx-auto">
        <div class="glass-card">
            <p class="font-bold mb-6 text-center uppercase tracking-widest text-xs">Uygulama Teması</p>
            <div class="grid grid-cols-2 gap-4">
                <div onclick="setTheme('theme-blue')" class="theme-swatch bg-[#0f172a] flex items-center justify-center text-[10px] font-bold">GECE MAVİSİ</div>
                <div onclick="setTheme('theme-dark')" class="theme-swatch bg-[#000000] border border-white/20 flex items-center justify-center text-[10px] font-bold">SAF SİYAH</div>
                <div onclick="setTheme('theme-green')" class="theme-swatch bg-[#064e3b] flex items-center justify-center text-[10px] font-bold">TIP YEŞİLİ</div>
                <div onclick="setTheme('theme-purple')" class="theme-swatch bg-[#2e1065] flex items-center justify-center text-[10px] font-bold">VİBE MOR</div>
            </div>
        </div>
        <div class="glass-card">
            <button onclick="clearData()" class="btn-ios w-full py-5 bg-red-900/20 border border-red-500/30 text-red-500 rounded-2xl font-bold uppercase text-xs tracking-widest">VERİLERİMİ SIFIRLA</button>
            <p class="text-center text-[10px] text-gray-500 mt-8 font-bold">V12.0 - PLAY STORE READY</p>
        </div>
    </div>

    <nav class="liquid-nav">
        <div onclick="showPage('odak')" id="nav-odak" class="nav-item active flex flex-col items-center"><i class="fas fa-stopwatch"></i><span class="text-[9px] font-black mt-1">ODAK</span></div>
        <div onclick="showPage('komite')" id="nav-komite" class="nav-item flex flex-col items-center"><i class="fas fa-calendar-alt"></i><span class="text-[9px] font-black mt-1">KOMİTE</span></div>
        <div onclick="showPage('ezber')" id="nav-ezber" class="nav-item flex flex-col items-center"><i class="fas fa-brain"></i><span class="text-[9px] font-black mt-1">EZBER</span></div>
        <div onclick="showPage('ayarlar')" id="nav-ayarlar" class="nav-item flex flex-col items-center"><i class="fas fa-cog"></i><span class="text-[9px] font-black mt-1">AYARLAR</span></div>
    </nav>

    <script>
        // Sayfa ve Tema Yönetimi
        function showPage(id) {
            document.querySelectorAll('.page-content').forEach(p => p.classList.remove('active'));
            document.getElementById('page-' + id).classList.add('active');
            document.querySelectorAll('.nav-item').forEach(n => n.classList.remove('active'));
            document.getElementById('nav-' + id).classList.add('active');
            document.getElementById('headerTitle').innerText = id;
            window.scrollTo(0, 0);
        }

        function setTheme(theme) {
            document.body.className = theme;
            localStorage.setItem('selectedTheme', theme);
        }
        if(localStorage.getItem('selectedTheme')) setTheme(localStorage.getItem('selectedTheme'));

        // Dinamik Pomodoro
        let timeLeft = 25 * 60, timerId = null;
        function setCustomTime() {
            const mins = document.getElementById('pomoInput').value;
            timeLeft = mins * 60;
            updateTimer();
            alert("Süre " + mins + " dakika olarak ayarlandı Hocam.");
        }
        function updateTimer() {
            let m = Math.floor(timeLeft / 60), s = timeLeft % 60;
            document.getElementById('timer').innerText = `${m}:${s < 10 ? '0' : ''}${s}`;
        }
        function startTimer() {
            const btn = document.getElementById('startBtn');
            if (!timerId) {
                timerId = setInterval(() => { timeLeft--; updateTimer(); if(timeLeft<=0) { clearInterval(timerId); alert("Süre bitti Hocam!"); } }, 1000);
                btn.innerText = "DURAKLAT"; btn.style.opacity = "0.7";
            } else {
                clearInterval(timerId); timerId = null;
                btn.innerText = "DEVAM ET"; btn.style.opacity = "1";
            }
        }
        function resetTimer() { clearInterval(timerId); timerId = null; setCustomTime(); document.getElementById('startBtn').innerText = "BAŞLAT"; }

        // Kronometre
        let swTime = 0, swId = null;
        function toggleStopwatch() {
            const btn = document.getElementById('swBtn');
            if(!swId) {
                swId = setInterval(() => { swTime += 10; let d = new Date(swTime); document.getElementById('stopwatch').innerText = d.toISOString().substr(14, 8); }, 10);
                btn.innerText = "DURDUR";
            } else { clearInterval(swId); swId = null; btn.innerText = "DEVAM ET"; }
        }
        function resetStopwatch() { clearInterval(swId); swId = null; swTime = 0; document.getElementById('stopwatch').innerText = "00:00.00"; document.getElementById('swBtn').innerText = "BAŞLAT"; }

        // Kartlar & Geri Sayım (Kalıcı)
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
        function clearData() { if(confirm("Tüm verileriniz silinecek Hocam, emin misiniz?")) { localStorage.clear(); location.reload(); } }
    </script>
</body>
</html>
