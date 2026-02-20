<!DOCTYPE html>
<html lang="tr">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
    <title>Focus App v18.1</title>
    <script src="https://cdn.tailwindcss.com"></script>
  <link href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.0.0/css/all.min.css" rel="stylesheet">
  <link rel="manifest" href="manifest.json">
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

        /* iOS Style Navigation */
        .liquid-nav {
            position: fixed; bottom: 30px; left: 50%; transform: translateX(-50%);
            width: 85%; max-width: 400px; height: 75px;
            background: rgba(255, 255, 255, 0.08); backdrop-filter: blur(25px);
            border: 1px solid rgba(255, 255, 255, 0.15); border-radius: 38px;
            display: flex; justify-content: space-around; align-items: center; z-index: 999;
        }

        .nav-item { transition: all 0.4s ease; color: #666; cursor: pointer; text-align: center; }
        .nav-item.active { color: var(--accent); transform: translateY(-8px); }
        .nav-item i { font-size: 20px; display: block; }
        .nav-item span { font-size: 9px; font-weight: 900; margin-top: 4px; display: block; }

        /* iOS Style Inputs & Buttons */
        .btn-ios { transition: all 0.2s ease; cursor: pointer; }
        .btn-ios:active { transform: scale(0.95); opacity: 0.8; }

        .glass-card {
            background: var(--card); border: 1px solid rgba(255, 255, 255, 0.08);
            border-radius: 35px; padding: 25px; margin-bottom: 20px; backdrop-filter: blur(10px);
        }

        .page-content { display: none; padding: 110px 20px 160px 20px; animation: slideUp 0.4s ease-out; }
        .page-content.active { display: block; }
        @keyframes slideUp { from { opacity: 0; transform: translateY(20px); } to { opacity: 1; transform: translateY(0); } }

        /* Modern iOS-ish Date Input */
        input[type="date"] {
            background: rgba(255,255,255,0.05); border: 1px solid rgba(255,255,255,0.1);
            border-radius: 15px; padding: 12px; color: white; width: 100%; outline: none;
            color-scheme: dark; font-weight: 600;
        }

        /* Mode Pill Style */
        .mode-pill { 
            padding: 10px 18px; border-radius: 20px; background: rgba(255,255,255,0.05); 
            border: 1px solid rgba(255,255,255,0.1); font-size: 10px; font-weight: 800; 
            white-space: nowrap; transition: 0.3s;
        }
        .mode-pill.active { background: var(--accent); color: black; border-color: var(--accent); }

        /* Heatmap Styles (Analiz) */
        .heatmap { display: grid; grid-template-columns: repeat(7, 1fr); gap: 6px; margin-top: 15px; }
        .heat-box { aspect-ratio: 1; border-radius: 6px; background: rgba(255,255,255,0.05); border: 1px solid rgba(255,255,255,0.02); }
        .heat-val-1 { background: #1e3a8a; }
        .heat-val-2 { background: #2563eb; }
        .heat-val-3 { background: #60a5fa; box-shadow: 0 0 10px rgba(96, 165, 250, 0.3); }

        #profile-img { width: 44px; height: 44px; border-radius: 50%; object-fit: cover; border: 2px solid var(--accent); background: #111; }
        #profile-menu { position: fixed; bottom: -100%; left: 0; width: 100%; padding: 20px; background: rgba(20, 20, 25, 0.98); backdrop-filter: blur(30px); border-radius: 30px 30px 0 0; z-index: 9999; transition: bottom 0.4s ease; }
        #profile-menu.show { bottom: 0; }
        .menu-btn { width: 100%; padding: 18px; text-align: center; border-bottom: 1px solid rgba(255,255,255,0.05); font-weight: 600; text-transform: uppercase; font-size: 13px; }
    </style>
</head>
<body class="theme-blue">

    <div class="fixed top-0 w-full z-[100] bg-inherit backdrop-blur-xl px-6 py-6 border-b border-white/5">
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
            <p class="text-[10px] font-bold text-gray-500 uppercase tracking-widest mb-2">POMODORO</p>
            <div id="timer" class="text-7xl font-black mb-6 tracking-tighter">30:00</div>
            <button onclick="startTimer()" id="startBtn" class="btn-ios w-full bg-blue-600 py-4 rounded-2xl font-bold text-lg mb-4 uppercase">BAŞLAT</button>
            <button onclick="resetTimer()" class="text-[10px] text-gray-500 font-bold uppercase tracking-widest btn-ios">SIFIRLA</button>
        </div>

        <div class="mb-6">
            <p class="text-[9px] font-black text-gray-500 uppercase tracking-widest mb-3 ml-2">ÇALIŞMA MODLARI</p>
            <div class="flex gap-2 overflow-x-auto pb-2 no-scrollbar">
                <div onclick="setMode('NORMAL', 30, 10)" id="mode-NORMAL" class="mode-pill active btn-ios">NORMAL (30/10)</div>
                <div onclick="setMode('SINAV', 90, 20)" id="mode-SINAV" class="mode-pill btn-ios">SINAV (90/20)</div>
                <div onclick="setMode('KİTAP', 50, 15)" id="mode-KİTAP" class="mode-pill btn-ios">KİTAP (50/15)</div>
                <div onclick="setMode('RAHAT', 60, 45)" id="mode-RAHAT" class="mode-pill btn-ios">RAHAT (60/45)</div>
            </div>
        </div>

        <div class="glass-card text-center">
            <p class="text-[10px] font-bold text-gray-500 uppercase tracking-widest mb-2">KRONOMETRE</p>
            <div id="stopwatch" class="text-4xl font-mono font-bold mb-4">00:00.00</div>
            <div class="flex gap-3">
                <button onclick="toggleStopwatch()" id="swBtn" class="btn-ios flex-[2] bg-emerald-600 py-3 rounded-xl font-bold uppercase text-sm">BAŞLAT</button>
                <button onclick="resetStopwatch()" class="btn-ios flex-[1] bg-zinc-800 py-3 rounded-xl font-bold uppercase text-sm">SIFIRLA</button>
            </div>
        </div>

        <div class="glass-card">
            <p class="font-bold mb-1 uppercase tracking-widest text-[10px] text-gray-400">HAFTALIK ÇALIŞMA ANALİZİ</p>
            <div class="heatmap" id="heatmapContainer"></div>
            <div class="flex justify-between mt-4 text-[8px] text-gray-500 font-black uppercase">
                <span>PZT</span><span>SAL</span><span>ÇAR</span><span>PER</span><span>CUM</span><span>CMT</span><span>PAZ</span>
            </div>
        </div>
    </div>

    <div id="page-komite" class="page-content max-w-md mx-auto">
        <div class="glass-card">
            <p class="text-[10px] font-black text-blue-400 uppercase tracking-[0.2em] mb-4 text-center">YILLIK PLANLAYICI</p>
            <div class="space-y-4">
                <input id="examName" placeholder="KOMİTE ADI..." class="w-full bg-black/20 p-4 rounded-xl outline-none border border-white/10 text-sm uppercase">
                <input type="date" id="examDate">
                <button onclick="addExam()" class="btn-ios w-full bg-blue-600 py-4 rounded-xl font-bold uppercase text-xs">PLANA EKLE</button>
            </div>
        </div>
        <div id="examList" class="space-y-3 mb-8"></div>
        <div class="glass-card">
            <p id="quoteBox" class="text-md font-medium italic opacity-80 text-center leading-relaxed italic">"Bugün çalış, yarın Onur Hocam desinler."</p>
        </div>
    </div>

    <div id="page-ezber" class="page-content max-w-md mx-auto">
        <div class="glass-card min-h-[220px] flex items-center justify-center btn-ios bg-gradient-to-br from-white/5 to-transparent mb-6" onclick="flipCard()">
            <p id="cardDisplay" class="text-2xl font-bold px-6 text-center uppercase tracking-tight">ÖNCE KART EKLE</p>
        </div>
        <div class="glass-card space-y-3">
            <input id="qInput" placeholder="SORU..." class="w-full bg-black/20 p-4 rounded-xl outline-none border border-white/10 text-sm uppercase">
            <input id="aInput" placeholder="CEVAP..." class="w-full bg-black/20 p-4 rounded-xl outline-none border border-white/10 text-sm uppercase">
            <button onclick="addCard()" class="btn-ios w-full bg-blue-600 py-4 rounded-xl font-bold uppercase text-xs">KARTI KAYDET</button>
        </div>
        <div id="deckList" class="space-y-3 pb-20"></div>
    </div>

    <div id="page-ayarlar" class="page-content max-w-md mx-auto">
        <div class="glass-card">
            <p class="font-bold mb-6 text-center uppercase tracking-widest text-[10px] text-gray-400">UYGULAMA TEMASI</p>
            <div class="grid grid-cols-2 gap-3">
                <div onclick="setTheme('theme-blue')" class="theme-swatch btn-ios bg-[#0f172a] h-14 flex items-center justify-center rounded-2xl text-[10px] font-bold border border-white/5">GECE MAVİSİ</div>
                <div onclick="setTheme('theme-dark')" class="theme-swatch btn-ios bg-[#000000] border border-white/10 h-14 flex items-center justify-center rounded-2xl text-[10px] font-bold uppercase">SAF SİYAH</div>
                <div onclick="setTheme('theme-green')" class="theme-swatch btn-ios bg-[#064e3b] h-14 flex items-center justify-center rounded-2xl text-[10px] font-bold uppercase">TIP YEŞİLİ</div>
                <div onclick="setTheme('theme-purple')" class="theme-swatch btn-ios bg-[#2e1065] h-14 flex items-center justify-center rounded-2xl text-[10px] font-bold uppercase">VİBE MOR</div>
            </div>
        </div>
        <div class="glass-card">
            <button onclick="clearData()" class="btn-ios w-full py-5 bg-red-900/10 border border-red-500/30 text-red-500 rounded-3xl font-black uppercase text-xs tracking-widest">VERİLERİMİ SIFIRLA</button>
            <p class="text-center text-[9px] text-gray-600 mt-8 font-bold uppercase tracking-widest">V18.1 - USTAM EDITION</p>
        </div>
    </div>

    <div id="profile-menu">
        <div class="menu-btn text-blue-400" onclick="triggerUpload()">FOTOĞRAFI DEĞİŞTİR</div>
        <div class="menu-btn text-red-500" onclick="removeProfilePic()">PROFİL RESMİNİ SİL</div>
        <div class="menu-btn !border-none text-gray-400" onclick="toggleProfileMenu()">VAZGEÇ</div>
        <input type="file" id="profile-upload" class="hidden" accept="image/*" onchange="processPic(event)">
    </div>

    <nav class="liquid-nav">
        <div onclick="showPage('odak')" id="nav-odak" class="nav-item active">
            <i class="fas fa-stopwatch"></i><span>ODAK</span>
        </div>
        <div onclick="showPage('komite')" id="nav-komite" class="nav-item">
            <i class="fas fa-calendar-day"></i><span>KOMİTE</span>
        </div>
        <div onclick="showPage('ezber')" id="nav-ezber" class="nav-item">
            <i class="fas fa-brain"></i><span>EZBER</span>
        </div>
        <div onclick="showPage('ayarlar')" id="nav-ayarlar" class="nav-item">
            <i class="fas fa-cog"></i><span>AYARLAR</span>
        </div>
    </nav>

    <script>
        const defaultPic = "https://img.icons8.com/ios-filled/100/ffffff/stethoscope.png";
        
        function toggleProfileMenu() { document.getElementById('profile-menu').classList.toggle('show'); }
        function triggerUpload() { document.getElementById('profile-upload').click(); toggleProfileMenu(); }
        function processPic(e) {
            const f = e.target.files[0];
            if(f) { const r = new FileReader(); r.onload = (ev) => { document.getElementById('profile-img').src = ev.target.result; localStorage.setItem('userProfilePic', ev.target.result); }; r.readAsDataURL(f); }
        }
        function removeProfilePic() { document.getElementById('profile-img').src = defaultPic; localStorage.removeItem('userProfilePic'); toggleProfileMenu(); }
        if(localStorage.getItem('userProfilePic')) document.getElementById('profile-img').src = localStorage.getItem('userProfilePic');

        function showPage(id) {
            document.querySelectorAll('.page-content').forEach(p => p.classList.remove('active'));
            document.getElementById('page-' + id).classList.add('active');
            document.querySelectorAll('.nav-item').forEach(n => n.classList.remove('active'));
            document.getElementById('nav-' + id).classList.add('active');
            document.getElementById('headerTitle').innerText = id.toUpperCase();
            if(id === 'odak') renderHeatmap();
            window.scrollTo(0,0);
        }

        let currentWorkMin = 30, timeLeft = 30 * 60, timerId = null;
        function setMode(name, work, breakTime) {
            document.querySelectorAll('.mode-pill').forEach(p => p.classList.remove('active'));
            document.getElementById('mode-' + name).classList.add('active');
            currentWorkMin = work; resetTimer();
        }
        function updateTimerDisplay() { let m = Math.floor(timeLeft/60), s = timeLeft%60; document.getElementById('timer').innerText = `${m}:${s<10?'0':''}${s}`; }
        function startTimer() {
            if(!timerId) {
                timerId = setInterval(() => { timeLeft--; updateTimerDisplay(); if(timeLeft <= 0) { clearInterval(timerId); timerId = null; saveFocusData(); alert("Bitti Hocam!"); } }, 1000);
                document.getElementById('startBtn').innerText = "DURAKLAT";
            } else { clearInterval(timerId); timerId = null; document.getElementById('startBtn').innerText = "DEVAM ET"; }
        }
        function resetTimer() { clearInterval(timerId); timerId = null; timeLeft = currentWorkMin * 60; updateTimerDisplay(); document.getElementById('startBtn').innerText = "BAŞLAT"; }

        function saveFocusData() {
            let data = JSON.parse(localStorage.getItem('focusHistory')) || {};
            let today = new Date().toISOString().split('T')[0];
            data[today] = (data[today] || 0) + 1;
            localStorage.setItem('focusHistory', JSON.stringify(data));
            renderHeatmap();
        }

        function renderHeatmap() {
            const container = document.getElementById('heatmapContainer'); if(!container) return;
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

        let swT = 0, swI = null;
        function toggleStopwatch() {
            if(!swI) { swI = setInterval(() => { swT+=10; document.getElementById('stopwatch').innerText = new Date(swT).toISOString().substr(14,8); }, 10); document.getElementById('swBtn').innerText = "DURDUR"; }
            else { clearInterval(swI); swI = null; document.getElementById('swBtn').innerText = "BAŞLAT"; }
        }
        function resetStopwatch() { clearInterval(swI); swI = null; swT=0; document.getElementById('stopwatch').innerText = "00:00.00"; document.getElementById('swBtn').innerText = "BAŞLAT"; }

        let exams = JSON.parse(localStorage.getItem('exams'))||[];
        function addExam() { const n = document.getElementById('examName').value, d = document.getElementById('examDate').value; if(n&&d){ exams.push({name:n, date:d}); exams.sort((a,b)=>new Date(a.date)-new Date(b.date)); localStorage.setItem('exams', JSON.stringify(exams)); renderExams(); } }
        function renderExams() {
            const l = document.getElementById('examList'); if(!l) return; l.innerHTML = '';
            const today = new Date(); today.setHours(0,0,0,0);
            exams.forEach((ex,i) => {
                const target = new Date(ex.date); target.setHours(0,0,0,0);
                const diff = Math.ceil((target-today)/86400000);
                let txt = diff===0?"BUGÜN!":(diff>0?diff+" GÜN":Math.abs(diff)+" GÜN ÖNCE");
                l.innerHTML += `<div class="list-item flex justify-between items-center bg-white/5 p-5 rounded-3xl mb-3"><div><p class="text-[9px] text-gray-500 font-bold uppercase">${ex.date}</p><p class="text-sm font-black uppercase">${ex.name}</p></div><div class="flex items-center gap-4"><p class="text-[10px] font-black ${diff>=0?'text-blue-400':'text-gray-600'}">${txt}</p><button onclick="deleteExam(${i})" class="text-red-500/30"><i class="fas fa-trash"></i></button></div></div>`;
            });
        }
        function deleteExam(i) { exams.splice(i,1); localStorage.setItem('exams', JSON.stringify(exams)); renderExams(); }

        let deck = JSON.parse(localStorage.getItem('deck'))||[];
        function addCard() { const q = document.getElementById('qInput').value, a = document.getElementById('aInput').value; if(q&&a){ deck.push({q,a}); localStorage.setItem('deck', JSON.stringify(deck)); renderDeck(); } }
        function renderDeck() {
            const l = document.getElementById('deckList'); if(!l) return; l.innerHTML = '';
            deck.forEach((c,i) => { l.innerHTML += `<div class="list-item flex justify-between items-center bg-white/5 p-5 rounded-3xl mb-3"><div><p class="text-xs font-bold uppercase leading-relaxed">${c.q}</p></div><button onclick="deleteCard(${i})" class="text-red-500/30"><i class="fas fa-trash"></i></button></div>`; });
            if(deck.length>0) document.getElementById('cardDisplay').innerText = deck[deck.length-1].q.toUpperCase();
        }
        function flipCard() { if(deck.length===0) return; const d = document.getElementById('cardDisplay'); d.innerText = d.innerText === deck[deck.length-1].q.toUpperCase() ? deck[deck.length-1].a.toUpperCase() : deck[deck.length-1].q.toUpperCase(); }
        function deleteCard(i) { deck.splice(i,1); localStorage.setItem('deck', JSON.stringify(deck)); renderDeck(); }

        function setTheme(t) { document.body.className = t; localStorage.setItem('selectedTheme', t); }
        if(localStorage.getItem('selectedTheme')) setTheme(localStorage.getItem('selectedTheme'));
        function clearData() { if(confirm("VERİLERİ SIFIRLA?")) { localStorage.clear(); location.reload(); } }
        
        window.onload = () => { renderHeatmap(); renderExams(); renderDeck(); };
    </script>
</body>
</html>
