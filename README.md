<!DOCTYPE html>
<html lang="tr">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
    <title>Focus App v14</title>
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
            font-family: -apple-system, sans-serif; margin: 0; overflow-x: hidden;
            overflow-y: auto !important; min-height: 100vh;
        }

        .liquid-nav {
            position: fixed; bottom: 30px; left: 50%; transform: translateX(-50%);
            width: 85%; max-width: 400px; height: 75px;
            background: rgba(255, 255, 255, 0.08); backdrop-filter: blur(25px);
            -webkit-backdrop-filter: blur(25px); border: 1px solid rgba(255, 255, 255, 0.15);
            border-radius: 38px; display: flex; justify-content: space-around; align-items: center; z-index: 9999;
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

        .list-item { background: rgba(255,255,255,0.02); border-radius: 20px; padding: 18px; margin-top: 10px; border-left: 4px solid var(--accent); }
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
        <div class="glass-card text-center mb-4">
            <p class="text-[10px] font-bold text-gray-500 uppercase tracking-widest mb-2">Pomodoro</p>
            <div class="flex justify-center items-center gap-2 mb-4">
                <input type="number" id="pomoInput" value="25" class="bg-white/10 w-12 text-center rounded-lg font-bold outline-none border border-white/10 text-xs py-1">
                <button onclick="setCustomTime()" class="text-[9px] font-bold text-blue-400 border border-blue-400/30 px-2 py-1 rounded-md uppercase">Uygula</button>
            </div>
            <div id="timer" class="text-7xl font-black mb-6 tracking-tighter">25:00</div>
            <button onclick="startTimer()" id="startBtn" class="btn-ios w-full bg-blue-600 py-4 rounded-2xl font-bold text-lg mb-2">BAŞLAT</button>
            <button onclick="resetTimer()" class="text-[10px] text-gray-500 font-bold uppercase">Sıfırla</button>
        </div>
        <div class="glass-card text-center">
            <p class="text-[10px] font-bold text-gray-500 uppercase tracking-widest mb-2">Kronometre</p>
            <div id="stopwatch" class="text-4xl font-mono font-bold mb-4">00:00.00</div>
            <div class="flex gap-3">
                <button onclick="toggleStopwatch()" id="swBtn" class="btn-ios flex-[2] bg-emerald-600 py-3 rounded-xl font-bold">BAŞLAT</button>
                <button onclick="resetStopwatch()" class="btn-ios flex-[1] bg-zinc-800 py-3 rounded-xl font-bold">SIFIRLA</button>
            </div>
        </div>
    </div>

    <div id="page-komite" class="page-content max-w-md mx-auto">
        <div class="glass-card">
            <p class="text-[10px] font-black text-blue-400 uppercase tracking-[0.2em] mb-4 text-center">YILLIK KOMİTE PLANI</p>
            <div class="space-y-3">
                <input id="examName" placeholder="Komite Adı (Örn: Anatomi)" class="w-full bg-black/20 p-4 rounded-xl outline-none border border-white/10 text-sm">
                <input type="date" id="examDate" class="w-full bg-black/20 p-4 rounded-xl border border-white/10 text-white outline-none text-sm">
                <button onclick="addExam()" class="btn-ios w-full bg-blue-600 py-4 rounded-xl font-bold uppercase text-xs">PLANA EKLE</button>
            </div>
        </div>

        <div id="examList" class="space-y-3 mb-8">
            </div>

        <div class="glass-card">
            <p class="text-[10px] font-bold text-blue-400 uppercase tracking-[0.2em] mb-3 text-center">MOTİVASYON</p>
            <p id="quoteBox" class="text-md font-medium italic opacity-80 text-center leading-relaxed">"Bugün çalış, yarın Onur Hocam desinler."</p>
        </div>
    </div>

    <div id="page-ezber" class="page-content max-w-md mx-auto">
        <div class="glass-card min-h-[180px] flex items-center justify-center btn-ios bg-gradient-to-br from-white/5 to-transparent mb-6" onclick="flipCard()">
            <p id="cardDisplay" class="text-xl font-semibold px-6 text-center">Ezber Kartı Seçilmedi</p>
        </div>
        <div class="glass-card space-y-3">
            <input id="qInput" placeholder="Soru..." class="w-full bg-black/20 p-3 rounded-xl outline-none border border-white/10 text-sm">
            <input id="aInput" placeholder="Cevap..." class="w-full bg-black/20 p-3 rounded-xl outline-none border border-white/10 text-sm">
            <button onclick="addCard()" class="btn-ios w-full bg-blue-600 py-3 rounded-xl font-bold uppercase text-xs">KARTI KAYDET</button>
        </div>
        <div id="deckList" class="space-y-3 pb-20"></div>
    </div>

    <div id="page-ayarlar" class="page-content max-w-md mx-auto">
        <div class="glass-card">
            <p class="font-bold mb-4 text-center uppercase tracking-widest text-[10px] text-gray-400">Uygulama Teması</p>
            <div class="grid grid-cols-2 gap-3">
                <div onclick="setTheme('theme-blue')" class="theme-swatch bg-[#0f172a] h-12 flex items-center justify-center rounded-xl text-[9px] font-bold">GECE MAVİSİ</div>
                <div onclick="setTheme('theme-dark')" class="theme-swatch bg-[#000000] border border-white/10 h-12 flex items-center justify-center rounded-xl text-[9px] font-bold">SAF SİYAH</div>
                <div onclick="setTheme('theme-green')" class="theme-swatch bg-[#064e3b] h-12 flex items-center justify-center rounded-xl text-[9px] font-bold">TIP YEŞİLİ</div>
                <div onclick="setTheme('theme-purple')" class="theme-swatch bg-[#2e1065] h-12 flex items-center justify-center rounded-xl text-[9px] font-bold">VİBE MOR</div>
            </div>
        </div>
        <div class="glass-card">
            <button onclick="clearData()" class="btn-ios w-full py-4 bg-red-900/10 border border-red-500/30 text-red-500 rounded-2xl font-bold uppercase text-xs">VERİLERİMİ SIFIRLA</button>
            <p class="text-center text-[9px] text-gray-500 mt-6 font-bold uppercase tracking-widest">V14.0 - FINAL VIBE</p>
        </div>
    </div>

    <nav class="liquid-nav">
        <div onclick="showPage('odak')" id="nav-odak" class="nav-item active flex flex-col items-center"><i class="fas fa-stopwatch"></i><span class="text-[9px] font-black mt-1 uppercase">Odak</span></div>
        <div onclick="showPage('komite')" id="nav-komite" class="nav-item flex flex-col items-center"><i class="fas fa-calendar-alt"></i><span class="text-[9px] font-black mt-1 uppercase">Komite</span></div>
        <div onclick="showPage('ezber')" id="nav-ezber" class="nav-item flex flex-col items-center"><i class="fas fa-brain"></i><span class="text-[9px] font-black mt-1 uppercase">Ezber</span></div>
        <div onclick="showPage('ayarlar')" id="nav-ayarlar" class="nav-item flex flex-col items-center"><i class="fas fa-cog"></i><span class="text-[9px] font-black mt-1 uppercase">Ayarlar</span></div>
    </nav>

    <script>
        const quotes = ["Tıp bir sanattır, sen de sanatçısısın.", "Zorlanıyorsan, gelişiyorsun demektir.", "Bugünün teri, yarının başarısıdır Hocam.", "Vazgeçmeyenler kazanır.", "Her kurul bir zaferdir!"];
        
        function showPage(id) {
            document.querySelectorAll('.page-content').forEach(p => p.classList.remove('active'));
            document.getElementById('page-' + id).classList.add('active');
            document.querySelectorAll('.nav-item').forEach(n => n.classList.remove('active'));
            document.getElementById('nav-' + id).classList.add('active');
            document.getElementById('headerTitle').innerText = id.toUpperCase();
            if(id === 'komite') {
                document.getElementById('quoteBox').innerText = quotes[new Date().getDay() % quotes.length];
                renderExams();
            }
            if(id === 'ezber') renderDeck();
            window.scrollTo(0, 0);
        }

        function setTheme(theme) { document.body.className = theme; localStorage.setItem('selectedTheme', theme); }
        if(localStorage.getItem('selectedTheme')) setTheme(localStorage.getItem('selectedTheme'));

        // ODAK & KRONOMETRE
        let timeLeft = 25 * 60, timerId = null;
        function updateTimer() { let m = Math.floor(timeLeft / 60), s = timeLeft % 60; document.getElementById('timer').innerText = `${m}:${s < 10 ? '0' : ''}${s}`; }
        function startTimer() {
            const btn = document.getElementById('startBtn');
            if (!timerId) {
                timerId = setInterval(() => { timeLeft--; updateTimer(); if(timeLeft<=0) { clearInterval(timerId); alert("Bitti Hocam!"); } }, 1000);
                btn.innerText = "DURAKLAT";
            } else { clearInterval(timerId); timerId = null; btn.innerText = "DEVAM ET"; }
        }
        function resetTimer() { clearInterval(timerId); timerId = null; setCustomTime(); document.getElementById('startBtn').innerText = "BAŞLAT"; }
        function setCustomTime() { timeLeft = document.getElementById('pomoInput').value * 60; updateTimer(); }

        let swTime = 0, swId = null;
        function toggleStopwatch() {
            const btn = document.getElementById('swBtn');
            if(!swId) { swId = setInterval(() => { swTime += 10; let d = new Date(swTime); document.getElementById('stopwatch').innerText = d.toISOString().substr(14, 8); }, 10); btn.innerText = "DURDUR"; }
            else { clearInterval(swId); swId = null; btn.innerText = "BAŞLAT"; }
        }
        function resetStopwatch() { clearInterval(swId); swId = null; swTime = 0; document.getElementById('stopwatch').innerText = "00:00.00"; document.getElementById('swBtn').innerText = "BAŞLAT"; }

        // KOMİTE LİSTESİ SİSTEMİ
        let exams = JSON.parse(localStorage.getItem('exams')) || [];
        function addExam() {
            const name = document.getElementById('examName').value;
            const date = document.getElementById('examDate').value;
            if(name && date) {
                exams.push({name, date});
                exams.sort((a,b) => new Date(a.date) - new Date(date));
                localStorage.setItem('exams', JSON.stringify(exams));
                document.getElementById('examName').value = '';
                document.getElementById('examDate').value = '';
                renderExams();
            }
        }

        function renderExams() {
            const list = document.getElementById('examList');
            list.innerHTML = '';
            const today = new Date(); today.setHours(0,0,0,0);
            
            exams.forEach((ex, i) => {
                const target = new Date(ex.date); target.setHours(0,0,0,0);
                const diff = Math.ceil((target - today) / 86400000);
                let text = diff === 0 ? "BUGÜN!" : (diff > 0 ? diff + " GÜN KALDI" : Math.abs(diff) + " GÜN ÖNCEYDİ");
                
                list.innerHTML += `
                    <div class="list-item flex justify-between items-center">
                        <div>
                            <p class="text-xs font-bold text-gray-500 uppercase">${ex.date}</p>
                            <p class="text-sm font-black">${ex.name}</p>
                        </div>
                        <div class="flex items-center gap-4">
                            <p class="text-[10px] font-black ${diff >= 0 ? 'text-blue-400' : 'text-gray-600'}">${text}</p>
                            <button onclick="deleteExam(${i})" class="text-red-500/30 text-xs"><i class="fas fa-trash"></i></button>
                        </div>
                    </div>`;
            });
        }
        function deleteExam(i) { exams.splice(i, 1); localStorage.setItem('exams', JSON.stringify(exams)); renderExams(); }

        // EZBER SİSTEMİ
        let deck = JSON.parse(localStorage.getItem('deck')) || [];
        function addCard() {
            const q = document.getElementById('qInput').value, a = document.getElementById('aInput').value;
            if(q && a) { deck.push({q, a}); localStorage.setItem('deck', JSON.stringify(deck)); renderDeck(); document.getElementById('qInput').value = ''; document.getElementById('aInput').value = ''; }
        }
        function renderDeck() {
            const list = document.getElementById('deckList'); list.innerHTML = '';
            deck.forEach((c, i) => { list.innerHTML += `<div class="list-item flex justify-between items-center"><div><p class="text-xs font-semibold">${c.q}</p></div><button onclick="deleteCard(${i})" class="text-red-500/30 p-2"><i class="fas fa-trash"></i></button></div>`; });
            if(deck.length > 0) document.getElementById('cardDisplay').innerText = deck[deck.length-1].q;
        }
        function flipCard() { if(deck.length === 0) return; const d = document.getElementById('cardDisplay'); d.innerText = d.innerText === deck[deck.length-1].q ? deck[deck.length-1].a : deck[deck.length-1].q; }
        function deleteCard(i) { deck.splice(i, 1); localStorage.setItem('deck', JSON.stringify(deck)); renderDeck(); }

        function clearData() { if(confirm("Tüm veriler temizlensin mi Hocam?")) { localStorage.clear(); location.reload(); } }
        renderExams();
    </script>
</body>
</html>
