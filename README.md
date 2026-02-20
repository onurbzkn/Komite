<!DOCTYPE html>
<html lang="tr">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
    <title>Focus App v16</title>
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
            overflow-y: auto !important; min-height: 100vh;
        }

        .liquid-nav {
            position: fixed; bottom: 30px; left: 50%; transform: translateX(-50%);
            width: 85%; max-width: 400px; height: 75px;
            background: rgba(255, 255, 255, 0.08); backdrop-filter: blur(25px);
            -webkit-backdrop-filter: blur(25px); border: 1px solid rgba(255, 255, 255, 0.15);
            border-radius: 38px; display: flex; justify-content: space-around; align-items: center; z-index: 999;
        }

        .nav-item { transition: all 0.4s cubic-bezier(0.175, 0.885, 0.32, 1.275); color: #666; }
        .nav-item.active { color: var(--accent); transform: translateY(-8px) scale(1.1); }
        .btn-ios:active { transform: scale(0.94); }

        .glass-card {
            background: var(--card); border: 1px solid rgba(255, 255, 255, 0.08);
            border-radius: 35px; padding: 25px; margin-bottom: 20px; backdrop-filter: blur(10px);
        }

        .page-content { display: none; padding: 110px 20px 160px 20px; animation: slideUp 0.4s ease-out; }
        .page-content.active { display: block; }
        @keyframes slideUp { from { opacity: 0; transform: translateY(20px); } to { opacity: 1; transform: translateY(0); } }

        /* iOS Style Action Sheet for Profile */
        #profile-menu {
            position: fixed; bottom: -100%; left: 0; width: 100%; padding: 20px;
            background: rgba(20, 20, 25, 0.95); backdrop-filter: blur(30px);
            border-radius: 30px 30px 0 0; z-index: 9999; transition: bottom 0.4s ease;
            box-shadow: 0 -10px 40px rgba(0,0,0,0.5);
        }
        #profile-menu.show { bottom: 0; }
        .menu-btn { width: 100%; padding: 18px; text-align: center; border-bottom: 1px solid rgba(255,255,255,0.05); font-weight: 600; font-size: 15px; }

        input[type="date"] { color-scheme: dark; border: 1px solid rgba(255,255,255,0.1); border-radius: 15px; padding: 12px; background: rgba(255,255,255,0.05); width: 100%; }
        #profile-img { width: 44px; height: 44px; border-radius: 50%; object-fit: cover; border: 2px solid var(--accent); }
    </style>
</head>
<body class="theme-blue">

    <div class="fixed top-0 w-full z-[100] bg-inherit backdrop-blur-xl px-6 py-6 border-b border-white/5">
        <div class="flex justify-between items-center max-w-md mx-auto">
            <div>
                <h1 id="headerTitle" class="text-3xl font-black tracking-tighter m-0 uppercase">ODAK</h1>
                <p class="text-[10px] text-blue-500 font-bold uppercase tracking-[0.2em] m-0">MEDİKAL ASİSTAN</p>
            </div>
            <div onclick="toggleProfileMenu()" class="btn-ios cursor-pointer">
                <img id="profile-img" src="https://img.icons8.com/ios-filled/100/ffffff/stethoscope.png" alt="Profile">
            </div>
        </div>
    </div>

    <div id="page-odak" class="page-content active max-w-md mx-auto text-center">
        <div class="glass-card">
            <p class="text-[10px] font-bold text-gray-500 uppercase tracking-widest mb-2">POMODORO</p>
            <div class="flex justify-center items-center gap-2 mb-4">
                <input type="number" id="pomoInput" value="25" class="bg-white/10 w-12 text-center rounded-lg font-bold outline-none border border-white/10 text-xs py-1">
                <button onclick="setCustomTime()" class="text-[9px] font-bold text-blue-400 border border-blue-400/30 px-2 py-1 rounded-md uppercase">UYGULA</button>
            </div>
            <div id="timer" class="text-7xl font-black mb-6 tracking-tighter">25:00</div>
            <button onclick="startTimer()" id="startBtn" class="btn-ios w-full bg-blue-600 py-4 rounded-2xl font-bold text-lg mb-2">BAŞLAT</button>
            <button onclick="resetTimer()" class="text-[10px] text-gray-500 font-bold uppercase tracking-widest">SIFIRLA</button>
        </div>
        <div class="glass-card">
            <p class="text-[10px] font-bold text-gray-500 uppercase tracking-widest mb-2">KRONOMETRE</p>
            <div id="stopwatch" class="text-4xl font-mono font-bold mb-4">00:00.00</div>
            <div class="flex gap-3">
                <button onclick="toggleStopwatch()" id="swBtn" class="btn-ios flex-[2] bg-emerald-600 py-3 rounded-xl font-bold">BAŞLAT</button>
                <button onclick="resetStopwatch()" class="btn-ios flex-[1] bg-zinc-800 py-3 rounded-xl font-bold uppercase">SIFIRLA</button>
            </div>
        </div>
    </div>

    <div id="page-komite" class="page-content max-w-md mx-auto">
        <div class="glass-card">
            <p class="text-[10px] font-black text-blue-400 uppercase tracking-[0.2em] mb-4 text-center">YILLIK KOMİTE PLANI</p>
            <div class="space-y-4">
                <input id="examName" placeholder="KOMİTE ADI..." class="w-full bg-black/20 p-4 rounded-xl outline-none border border-white/10 text-sm uppercase">
                <input type="date" id="examDate">
                <button onclick="addExam()" class="btn-ios w-full bg-blue-600 py-4 rounded-xl font-bold uppercase text-xs">PLANA EKLE</button>
            </div>
        </div>
        <div id="examList" class="space-y-3 mb-8"></div>
    </div>

    <div id="page-ezber" class="page-content max-w-md mx-auto">
        <div class="glass-card min-h-[180px] flex items-center justify-center btn-ios bg-gradient-to-br from-white/5 to-transparent mb-6" onclick="flipCard()">
            <p id="cardDisplay" class="text-xl font-semibold px-6 text-center">KART SEÇİLMEDİ</p>
        </div>
        <div class="glass-card space-y-3">
            <input id="qInput" placeholder="SORU..." class="w-full bg-black/20 p-3 rounded-xl outline-none border border-white/10 text-sm uppercase">
            <input id="aInput" placeholder="CEVAP..." class="w-full bg-black/20 p-3 rounded-xl outline-none border border-white/10 text-sm uppercase">
            <button onclick="addCard()" class="btn-ios w-full bg-blue-600 py-3 rounded-xl font-bold uppercase text-xs">KARTI KAYDET</button>
        </div>
        <div id="deckList" class="space-y-3 pb-20"></div>
    </div>

    <div id="page-ayarlar" class="page-content max-w-md mx-auto">
        <div class="glass-card">
            <p class="font-bold mb-4 text-center uppercase tracking-widest text-[10px] text-gray-400">UYGULAMA TEMASI</p>
            <div class="grid grid-cols-2 gap-3">
                <div onclick="setTheme('theme-blue')" class="theme-swatch bg-[#0f172a] h-12 flex items-center justify-center rounded-xl text-[9px] font-bold">GECE MAVİSİ</div>
                <div onclick="setTheme('theme-dark')" class="theme-swatch bg-[#000000] border border-white/10 h-12 flex items-center justify-center rounded-xl text-[9px] font-bold">SAF SİYAH</div>
                <div onclick="setTheme('theme-green')" class="theme-swatch bg-[#064e3b] h-12 flex items-center justify-center rounded-xl text-[9px] font-bold">TIP YEŞİLİ</div>
                <div onclick="setTheme('theme-purple')" class="theme-swatch bg-[#2e1065] h-12 flex items-center justify-center rounded-xl text-[9px] font-bold">VİBE MOR</div>
            </div>
        </div>
        <div class="glass-card text-center">
            <button onclick="clearData()" class="btn-ios w-full py-4 bg-red-900/10 border border-red-500/30 text-red-500 rounded-2xl font-bold uppercase text-xs tracking-widest">VERİLERİMİ SIFIRLA</button>
            <p class="text-[9px] text-gray-500 mt-6 font-bold uppercase tracking-widest">V16.0 - IOS ELITE</p>
        </div>
    </div>

    <div id="profile-menu">
        <div class="menu-btn text-blue-400" onclick="triggerUpload()">FOTOĞRAFI DEĞİŞTİR</div>
        <div class="menu-btn text-red-500" onclick="removeProfilePic()">PROFİL RESMİNİ SİL</div>
        <div class="menu-btn !border-none text-gray-400" onclick="toggleProfileMenu()">VAZGEÇ</div>
        <input type="file" id="profile-upload" class="hidden" accept="image/*" onchange="processPic(event)">
    </div>

    <nav class="liquid-nav">
        <div onclick="showPage('odak')" id="nav-odak" class="nav-item active flex flex-col items-center"><i class="fas fa-stopwatch"></i><span class="text-[9px] font-black mt-1 uppercase">ODAK</span></div>
        <div onclick="showPage('komite')" id="nav-komite" class="nav-item flex flex-col items-center"><i class="fas fa-calendar-alt"></i><span class="text-[9px] font-black mt-1 uppercase">KOMİTE</span></div>
        <div onclick="showPage('ezber')" id="nav-ezber" class="nav-item flex flex-col items-center"><i class="fas fa-brain"></i><span class="text-[9px] font-black mt-1 uppercase">EZBER</span></div>
        <div onclick="showPage('ayarlar')" id="nav-ayarlar" class="nav-item flex flex-col items-center"><i class="fas fa-cog"></i><span class="text-[9px] font-black mt-1 uppercase">AYARLAR</span></div>
    </nav>

    <script>
        const defaultPic = "https://img.icons8.com/ios-filled/100/ffffff/stethoscope.png";
        function toggleProfileMenu() { document.getElementById('profile-menu').classList.toggle('show'); }
        function triggerUpload() { document.getElementById('profile-upload').click(); toggleProfileMenu(); }
        function processPic(e) {
            const file = e.target.files[0];
            if(file) {
                const reader = new FileReader();
                reader.onload = (ev) => { document.getElementById('profile-img').src = ev.target.result; localStorage.setItem('userProfilePic', ev.target.result); };
                reader.readAsDataURL(file);
            }
        }
        function removeProfilePic() { document.getElementById('profile-img').src = defaultPic; localStorage.removeItem('userProfilePic'); toggleProfileMenu(); }
        if(localStorage.getItem('userProfilePic')) document.getElementById('profile-img').src = localStorage.getItem('userProfilePic');

        function showPage(id) {
            document.querySelectorAll('.page-content').forEach(p => p.classList.remove('active'));
            document.getElementById('page-' + id).classList.add('active');
            document.querySelectorAll('.nav-item').forEach(n => n.classList.remove('active'));
            document.getElementById('nav-' + id).classList.add('active');
            document.getElementById('headerTitle').innerText = id.toUpperCase();
            if(id === 'komite') renderExams();
            if(id === 'ezber') renderDeck();
            window.scrollTo(0,0);
        }

        function setTheme(t) { document.body.className = t; localStorage.setItem('selectedTheme', t); }
        if(localStorage.getItem('selectedTheme')) setTheme(localStorage.getItem('selectedTheme'));

        let timeLeft = 25*60, tId = null;
        function updateTimer() { let m = Math.floor(timeLeft/60), s = timeLeft%60; document.getElementById('timer').innerText = `${m}:${s<10?'0':''}${s}`; }
        function startTimer() {
            if(!tId) { tId = setInterval(() => { timeLeft--; updateTimer(); if(timeLeft<=0) clearInterval(tId); }, 1000); document.getElementById('startBtn').innerText = "DURAKLAT"; }
            else { clearInterval(tId); tId = null; document.getElementById('startBtn').innerText = "DEVAM ET"; }
        }
        function resetTimer() { clearInterval(tId); tId = null; setCustomTime(); document.getElementById('startBtn').innerText = "BAŞLAT"; }
        function setCustomTime() { timeLeft = document.getElementById('pomoInput').value*60; updateTimer(); }

        let swT = 0, swI = null;
        function toggleStopwatch() {
            if(!swI) { swI = setInterval(() => { swT+=10; document.getElementById('stopwatch').innerText = new Date(swT).toISOString().substr(14,8); }, 10); document.getElementById('swBtn').innerText = "DURDUR"; }
            else { clearInterval(swI); swI = null; document.getElementById('swBtn').innerText = "BAŞLAT"; }
        }
        function resetStopwatch() { clearInterval(swI); swI = null; swT=0; document.getElementById('stopwatch').innerText = "00:00.00"; document.getElementById('swBtn').innerText = "BAŞLAT"; }

        let exams = JSON.parse(localStorage.getItem('exams'))||[];
        function addExam() { const n = document.getElementById('examName').value, d = document.getElementById('examDate').value; if(n&&d){ exams.push({name:n, date:d}); exams.sort((a,b)=>new Date(a.date)-new Date(b.date)); localStorage.setItem('exams', JSON.stringify(exams)); renderExams(); } }
        function renderExams() {
            const l = document.getElementById('examList'); l.innerHTML = '';
            const today = new Date(); today.setHours(0,0,0,0);
            exams.forEach((ex,i) => {
                const target = new Date(ex.date); target.setHours(0,0,0,0);
                const diff = Math.ceil((target-today)/86400000);
                let txt = diff===0?"BUGÜN!":(diff>0?diff+" GÜN":Math.abs(diff)+" GÜN ÖNCE");
                l.innerHTML += `<div class="list-item flex justify-between items-center bg-white/5 p-4 rounded-2xl mb-2"><div><p class="text-[9px] text-gray-500 uppercase">${ex.date}</p><p class="text-sm font-black uppercase">${ex.name}</p></div><div class="flex items-center gap-4"><p class="text-[10px] font-black ${diff>=0?'text-blue-400':'text-gray-600'}">${txt}</p><button onclick="deleteExam(${i})" class="text-red-500/30"><i class="fas fa-trash"></i></button></div></div>`;
            });
        }
        function deleteExam(i) { exams.splice(i,1); localStorage.setItem('exams', JSON.stringify(exams)); renderExams(); }

        let deck = JSON.parse(localStorage.getItem('deck'))||[];
        function addCard() { const q = document.getElementById('qInput').value, a = document.getElementById('aInput').value; if(q&&a){ deck.push({q,a}); localStorage.setItem('deck', JSON.stringify(deck)); renderDeck(); } }
        function renderDeck() {
            const l = document.getElementById('deckList'); l.innerHTML = '';
            deck.forEach((c,i) => { l.innerHTML += `<div class="list-item flex justify-between items-center bg-white/5 p-4 rounded-2xl mb-2"><div><p class="text-xs font-semibold uppercase">${c.q}</p></div><button onclick="deleteCard(${i})" class="text-red-500/30"><i class="fas fa-trash"></i></button></div>`; });
            if(deck.length>0) document.getElementById('cardDisplay').innerText = deck[deck.length-1].q.toUpperCase();
        }
        function flipCard() { if(deck.length===0) return; const d = document.getElementById('cardDisplay'); d.innerText = d.innerText === deck[deck.length-1].q.toUpperCase() ? deck[deck.length-1].a.toUpperCase() : deck[deck.length-1].q.toUpperCase(); }
        function deleteCard(i) { deck.splice(i,1); localStorage.setItem('deck', JSON.stringify(deck)); renderDeck(); }

        function clearData() { if(confirm("VERİLERİNİ SIFIRLA?")) { localStorage.clear(); location.reload(); } }
    </script>
</body>
</html>
