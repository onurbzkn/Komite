<!DOCTYPE html>
<html lang="tr">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
    <title>Focus App v18</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <link href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.0.0/css/all.min.css" rel="stylesheet">
    <style>
        :root { --bg: #09090b; --card: rgba(255, 255, 255, 0.04); --accent: #3b82f6; }
        .theme-blue { --bg: #0f172a; --accent: #3b82f6; }
        .theme-dark { --bg: #000000; --accent: #ffffff; }
        .theme-green { --bg: #064e3b; --accent: #10b981; }
        .theme-purple { --bg: #2e1065; --accent: #a855f7; }

        body { 
            background-color: var(--bg); color: white; transition: all 0.5s ease;
            font-family: -apple-system, system-ui, sans-serif; margin: 0; overflow-x: hidden;
            min-height: 100vh;
        }

        .liquid-nav {
            position: fixed; bottom: 30px; left: 50%; transform: translateX(-50%);
            width: 85%; max-width: 400px; height: 75px;
            background: rgba(255, 255, 255, 0.08); backdrop-filter: blur(25px);
            border: 1px solid rgba(255, 255, 255, 0.15); border-radius: 38px;
            display: flex; justify-content: space-around; align-items: center; z-index: 999;
        }

        .nav-item { transition: all 0.4s ease; color: #666; cursor: pointer; }
        .nav-item.active { color: var(--accent); transform: translateY(-8px); }
        .btn-ios:active { transform: scale(0.95); }

        .glass-card {
            background: var(--card); border: 1px solid rgba(255, 255, 255, 0.08);
            border-radius: 35px; padding: 25px; margin-bottom: 20px; backdrop-filter: blur(10px);
        }

        .page-content { display: none; padding: 110px 20px 160px 20px; animation: slideUp 0.4s ease-out; }
        .page-content.active { display: block; }
        @keyframes slideUp { from { opacity: 0; transform: translateY(20px); } to { opacity: 1; transform: translateY(0); } }

        /* Mode Pill Style */
        .mode-pill { 
            padding: 10px 18px; border-radius: 20px; background: rgba(255,255,255,0.05); 
            border: 1px solid rgba(255,255,255,0.1); font-size: 10px; font-weight: 800; 
            white-space: nowrap; transition: 0.3s;
        }
        .mode-pill.active { background: var(--accent); color: black; border-color: var(--accent); }

        /* Heatmap Styles */
        .heatmap { display: grid; grid-template-columns: repeat(7, 1fr); gap: 6px; margin-top: 15px; }
        .heat-box { aspect-ratio: 1; border-radius: 4px; background: rgba(255,255,255,0.05); position: relative; }
        .heat-val-1 { background: #1e3a8a; }
        .heat-val-2 { background: #2563eb; }
        .heat-val-3 { background: #60a5fa; }

        #profile-img { width: 44px; height: 44px; border-radius: 50%; object-fit: cover; border: 2px solid var(--accent); background: #111; }
        #profile-menu { position: fixed; bottom: -100%; left: 0; width: 100%; padding: 20px; background: rgba(20, 20, 25, 0.98); backdrop-filter: blur(30px); border-radius: 30px 30px 0 0; z-index: 9999; transition: bottom 0.4s ease; }
        #profile-menu.show { bottom: 0; }
        .menu-btn { width: 100%; padding: 18px; text-align: center; border-bottom: 1px solid rgba(255,255,255,0.05); font-weight: 600; }
    </style>
</head>
<body class="theme-blue">

    <div class="fixed top-0 w-full z-[100] bg-inherit backdrop-blur-xl px-6 py-6 border-b border-white/5 text-center">
        <div class="flex justify-between items-center max-w-md mx-auto">
            <div class="text-left">
                <h1 id="headerTitle" class="text-3xl font-black tracking-tighter m-0 uppercase">ODAK</h1>
                <p class="text-[10px] text-blue-500 font-bold uppercase tracking-[0.2em] m-0">MEDİKAL ASİSTAN</p>
            </div>
            <div onclick="toggleProfileMenu()" class="btn-ios cursor-pointer">
                <img id="profile-img" src="https://img.icons8.com/ios-filled/100/ffffff/stethoscope.png" alt="PP">
            </div>
        </div>
    </div>

    <div id="page-odak" class="page-content active max-w-md mx-auto">
        <div class="glass-card text-center">
            <p class="text-[10px] font-bold text-gray-500 uppercase tracking-widest mb-2" id="timerLabel">ODAKLANMA</p>
            <div id="timer" class="text-7xl font-black mb-6 tracking-tighter">25:00</div>
            <button onclick="startTimer()" id="startBtn" class="btn-ios w-full bg-blue-600 py-4 rounded-2xl font-bold text-lg mb-4">BAŞLAT</button>
            <button onclick="resetTimer()" class="text-[10px] text-gray-500 font-bold uppercase tracking-widest">SIFIRLA</button>
        </div>

        <div class="mb-6">
            <p class="text-[9px] font-black text-gray-500 uppercase tracking-widest mb-3 ml-2">ÇALIŞMA MODLARI</p>
            <div class="flex gap-2 overflow-x-auto pb-2 no-scrollbar">
                <div onclick="setMode('NORMAL', 30, 10)" id="mode-NORMAL" class="mode-pill active cursor-pointer">NORMAL (30/10)</div>
                <div onclick="setMode('SINAV', 90, 20)" id="mode-SINAV" class="mode-pill cursor-pointer">SINAV (90/20)</div>
                <div onclick="setMode('KİTAP', 50, 15)" id="mode-KİTAP" class="mode-pill cursor-pointer">KİTAP (50/15)</div>
                <div onclick="setMode('RAHAT', 60, 45)" id="mode-RAHAT" class="mode-pill cursor-pointer">RAHAT (60/45)</div>
            </div>
        </div>

        <div class="glass-card text-center">
            <p class="text-[10px] font-bold text-gray-500 uppercase tracking-widest mb-2 text-center">KRONOMETRE</p>
            <div id="stopwatch" class="text-4xl font-mono font-bold mb-4">00:00.00</div>
            <div class="flex gap-3">
                <button onclick="toggleStopwatch()" id="swBtn" class="btn-ios flex-[2] bg-emerald-600 py-3 rounded-xl font-bold uppercase">BAŞLAT</button>
                <button onclick="resetStopwatch()" class="btn-ios flex-[1] bg-zinc-800 py-3 rounded-xl font-bold uppercase">SIFIRLA</button>
            </div>
        </div>
    </div>

    <div id="page-komite" class="page-content max-w-md mx-auto">
        <div class="glass-card">
            <p class="text-[10px] font-black text-blue-400 uppercase tracking-[0.2em] mb-4 text-center">YILLIK PLANI</p>
            <input id="examName" placeholder="KOMİTE ADI..." class="w-full bg-black/20 p-4 rounded-xl outline-none border border-white/10 text-sm uppercase mb-3">
            <input type="date" id="examDate" class="mb-3">
            <button onclick="addExam()" class="btn-ios w-full bg-blue-600 py-4 rounded-xl font-bold uppercase text-xs">PLANA EKLE</button>
        </div>
        <div id="examList" class="space-y-3"></div>
    </div>

    <div id="page-ayarlar" class="page-content max-w-md mx-auto">
        <div class="glass-card">
            <p class="font-bold mb-1 uppercase tracking-widest text-[10px] text-gray-400">HAFTALIK ÇALIŞMA ANALİZİ</p>
            <p class="text-[9px] text-gray-600 uppercase mb-4">Son 7 günün odaklanma seviyesi</p>
            <div class="heatmap" id="heatmapContainer">
                </div>
            <div class="flex justify-between mt-4 text-[8px] text-gray-500 font-bold uppercase">
                <span>PZT</span><span>SAL</span><span>ÇAR</span><span>PER</span><span>CUM</span><span>CMT</span><span>PAZ</span>
            </div>
        </div>

        <div class="glass-card">
            <p class="font-bold mb-4 text-center uppercase tracking-widest text-[10px] text-gray-400">UYGULAMA TEMASI</p>
            <div class="grid grid-cols-2 gap-3">
                <div onclick="setTheme('theme-blue')" class="theme-swatch bg-[#0f172a] h-12 flex items-center justify-center rounded-xl text-[9px] font-bold">GECE MAVİSİ</div>
                <div onclick="setTheme('theme-dark')" class="theme-swatch bg-[#000000] border border-white/10 h-12 flex items-center justify-center rounded-xl text-[9px] font-bold uppercase">SAF SİYAH</div>
            </div>
        </div>

        <button onclick="clearData()" class="btn-ios w-full py-4 bg-red-900/10 border border-red-500/30 text-red-500 rounded-2xl font-bold uppercase text-xs tracking-widest">VERİLERİMİ SIFIRLA</button>
    </div>

    <div id="profile-menu">
        <div class="menu-btn text-blue-400" onclick="triggerUpload()">FOTOĞRAFI DEĞİŞTİR</div>
        <div class="menu-btn text-red-500" onclick="removeProfilePic()">PROFİL RESMİNİ SİL</div>
        <div class="menu-btn !border-none text-gray-400" onclick="toggleProfileMenu()">VAZGEÇ</div>
        <input type="file" id="profile-upload" class="hidden" accept="image/*" onchange="processPic(event)">
    </div>

    <nav class="liquid-nav">
        <div onclick="showPage('odak')" id="nav-odak" class="nav-item active flex flex-col items-center"><i class="fas fa-stopwatch text-xl"></i><span class="text-[9px] font-black mt-1 uppercase">ODAK</span></div>
        <div onclick="showPage('komite')" id="nav-komite" class="nav-item flex flex-col items-center"><i class="fas fa-calendar-alt text-xl"></i><span class="text-[9px] font-black mt-1 uppercase">KOMİTE</span></div>
        <div onclick="showPage('ayarlar')" id="nav-ayarlar" class="nav-item flex flex-col items-center"><i class="fas fa-chart-bar text-xl"></i><span class="text-[9px] font-black mt-1 uppercase">ANALİZ</span></div>
    </nav>

    <script>
        // Profil Mantığı
        const defaultPic = "https://img.icons8.com/ios-filled/100/ffffff/stethoscope.png";
        function toggleProfileMenu() { document.getElementById('profile-menu').classList.toggle('show'); }
        function triggerUpload() { document.getElementById('profile-upload').click(); toggleProfileMenu(); }
        function processPic(e) {
            const f = e.target.files[0];
            if(f) { const r = new FileReader(); r.onload = (ev) => { document.getElementById('profile-img').src = ev.target.result; localStorage.setItem('userProfilePic', ev.target.result); }; r.readAsDataURL(f); }
        }
        function removeProfilePic() { document.getElementById('profile-img').src = defaultPic; localStorage.removeItem('userProfilePic'); toggleProfileMenu(); }
        if(localStorage.getItem('userProfilePic')) document.getElementById('profile-img').src = localStorage.getItem('userProfilePic');

        // Sayfa Değişimi
        function showPage(id) {
            document.querySelectorAll('.page-content').forEach(p => p.classList.remove('active'));
            document.getElementById('page-' + id).classList.add('active');
            document.querySelectorAll('.nav-item').forEach(n => n.classList.remove('active'));
            document.getElementById('nav-' + id).classList.add('active');
            document.getElementById('headerTitle').innerText = id === 'ayarlar' ? 'ANALİZ' : id.toUpperCase();
            if(id === 'ayarlar') renderHeatmap();
        }

        // Pomodoro Modları
        let currentWorkMin = 30, currentBreakMin = 10, timeLeft = 30 * 60, timerId = null;
        function setMode(name, work, breakTime) {
            document.querySelectorAll('.mode-pill').forEach(p => p.classList.remove('active'));
            document.getElementById('mode-' + name).classList.add('active');
            currentWorkMin = work; currentBreakMin = breakTime;
            resetTimer();
        }

        function updateTimerDisplay() { let m = Math.floor(timeLeft/60), s = timeLeft%60; document.getElementById('timer').innerText = `${m}:${s<10?'0':''}${s}`; }
        function startTimer() {
            if(!timerId) {
                timerId = setInterval(() => { 
                    timeLeft--; updateTimerDisplay(); 
                    if(timeLeft <= 0) { 
                        clearInterval(timerId); timerId = null; alert("Süre Doldu Hocam!"); 
                        saveFocusData(); // Çalışma bittiğinde veriyi kaydet
                    } 
                }, 1000);
                document.getElementById('startBtn').innerText = "DURAKLAT";
            } else { clearInterval(timerId); timerId = null; document.getElementById('startBtn').innerText = "DEVAM ET"; }
        }
        function resetTimer() { clearInterval(timerId); timerId = null; timeLeft = currentWorkMin * 60; updateTimerDisplay(); document.getElementById('startBtn').innerText = "BAŞLAT"; }

        // Isı Haritası (Heatmap) Mantığı
        function saveFocusData() {
            let data = JSON.parse(localStorage.getItem('focusHistory')) || {};
            let today = new Date().toISOString().split('T')[0];
            data[today] = (data[today] || 0) + 1;
            localStorage.setItem('focusHistory', JSON.stringify(data));
        }

        function renderHeatmap() {
            const container = document.getElementById('heatmapContainer');
            container.innerHTML = '';
            let history = JSON.parse(localStorage.getItem('focusHistory')) || {};
            for(let i=6; i>=0; i--) {
                let d = new Date(); d.setDate(d.getDate() - i);
                let dateStr = d.toISOString().split('T')[0];
                let count = history[dateStr] || 0;
                let level = count > 3 ? 'heat-val-3' : (count > 1 ? 'heat-val-2' : (count > 0 ? 'heat-val-1' : ''));
                container.innerHTML += `<div class="heat-box ${level}"></div>`;
            }
        }

        // Kronometre
        let swT = 0, swI = null;
        function toggleStopwatch() {
            if(!swI) { swI = setInterval(() => { swT+=10; document.getElementById('stopwatch').innerText = new Date(swT).toISOString().substr(14,8); }, 10); document.getElementById('swBtn').innerText = "DURDUR"; }
            else { clearInterval(swI); swI = null; document.getElementById('swBtn').innerText = "BAŞLAT"; }
        }
        function resetStopwatch() { clearInterval(swI); swI = null; swT=0; document.getElementById('stopwatch').innerText = "00:00.00"; document.getElementById('swBtn').innerText = "BAŞLAT"; }

        function setTheme(t) { document.body.className = t; localStorage.setItem('selectedTheme', t); }
        if(localStorage.getItem('selectedTheme')) setTheme(localStorage.getItem('selectedTheme'));
        function clearData() { if(confirm("TÜM VERİLERİ SIFIRLA?")) { localStorage.clear(); location.reload(); } }
    </script>
</body>
</html>
