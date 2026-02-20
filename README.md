
<html lang="tr">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Komite App V10</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <style>
        :root { --p-color: #3b82f6; }
        .theme-blue { --p-color: #3b82f6; }
        .theme-red { --p-color: #ef4444; }
        .theme-green { --p-color: #10b981; }

        body { transition: background-color 0.3s, color 0.3s; height: 100vh; display: flex; flex-direction: column; overflow: hidden; margin: 0; font-family: -apple-system, BlinkMacSystemFont, "Segoe UI", Roboto, Helvetica, Arial, sans-serif; }
        
        .dark { background-color: #000000; color: #ffffff; }
        .light { background-color: #ffffff; color: #000000; }
        
        .text-main { color: inherit; }
        
        .tab-content { display: none; height: 100%; overflow-y: auto; padding-bottom: 140px; scrollbar-width: none; }
        .tab-content::-webkit-scrollbar { display: none; }
        .tab-content.active { display: block; animation: fadeIn 0.4s cubic-bezier(0.4, 0, 0.2, 1); }
        @keyframes fadeIn { from { opacity: 0; transform: scale(0.98); } to { opacity: 1; transform: scale(1); } }

        .nav-container {
            position: fixed; bottom: 30px; left: 50%; transform: translateX(-50%);
            width: 85%; max-width: 400px; height: 70px;
            background: rgba(30, 41, 59, 0.5); backdrop-filter: blur(25px);
            border: 1px solid rgba(255, 255, 255, 0.1);
            border-radius: 40px; display: flex; justify-content: space-around;
            align-items: center; z-index: 1000; box-shadow: 0 10px 30px rgba(0,0,0,0.5);
        }
        .light .nav-container { background: rgba(240, 240, 240, 0.8); border: 1px solid rgba(0,0,0,0.05); box-shadow: 0 10px 30px rgba(0,0,0,0.1); }

        .nav-item { position: relative; display: flex; flex-direction: column; align-items: center; cursor: pointer; z-index: 2; width: 25%; transition: 0.2s; }
        .nav-icon { font-size: 20px; transition: 0.3s cubic-bezier(0.175, 0.885, 0.32, 1.275); }
        .nav-text { font-size: 8px; font-weight: 700; text-transform: uppercase; opacity: 0.5; margin-top: 4px; transition: 0.3s; }
        
        .nav-item.active .nav-icon { transform: translateY(-4px); color: var(--p-color); }
        .nav-item.active .nav-text { opacity: 1; color: var(--p-color); }

        .indicator {
            position: absolute; height: 45px; width: 45px;
            background: var(--p-color); border-radius: 50%;
            transition: 0.4s cubic-bezier(0.175, 0.885, 0.32, 1.275);
            z-index: 1; opacity: 0.15; filter: blur(8px);
        }

        .card { background: rgba(28, 28, 30, 0.6); border-radius: 30px; border: 1px solid rgba(255,255,255,0.05); }
        .light .card { background: #f2f2f7; border: 1px solid rgba(0,0,0,0.05); }

        .ios-btn { transition: all 0.2s cubic-bezier(0.4, 0, 0.2, 1); }
        .ios-btn:active { transform: scale(0.92); opacity: 0.8; }

        #profilePic { width: 42px; height: 42px; border-radius: 50%; border: 2px solid var(--p-color); background: #1c1c1e; display: flex; align-items: center; justify-content: center; overflow: hidden; }
        .light #profilePic { background: #e5e5ea; }
    </style>
</head>
<body class="dark theme-blue" id="mainBody">

    <header class="pt-10 pb-4 px-8 flex justify-between items-center">
        <div>
            <h1 id="pageTitle" class="text-3xl font-black tracking-tighter uppercase text-main">Focus</h1>
            <p class="text-[10px] font-bold opacity-40 uppercase tracking-widest" id="subHeader">Medikal Asistan</p>
        </div>
        <div id="pomoProfile">
            <input type="file" id="imgUpload" hidden accept="image/*" onchange="loadImg(event)">
            <div onclick="document.getElementById('imgUpload').click()" id="profilePic" class="ios-btn shadow-xl">
                <svg id="anonIcon" viewBox="0 0 24 24" class="w-6 h-6 fill-slate-500">
                    <path d="M12 12c2.21 0 4-1.79 4-4s-1.79-4-4-4-4 1.79-4 4 1.79 4 4 4zm0 2c-2.67 0-8 1.34-8 4v2h16v-2c0-2.66-5.33-4-8-4z"/>
                </svg>
                <img id="userImg" class="hidden w-full h-full object-cover">
            </div>
        </div>
    </header>

    <main class="flex-1 px-6 overflow-hidden">
        
        <section id="pomoTab" class="tab-content active space-y-6">
            <div class="card p-6 text-center shadow-lg">
                <div class="flex justify-between items-center mb-4 px-2">
                    <span class="text-[10px] font-black opacity-40 uppercase tracking-widest">Pomodoro</span>
                    <input type="number" id="pomoInput" value="25" oninput="autoUpdateTimer()" class="bg-transparent text-right w-12 text-sm font-bold outline-none text-main">
                </div>
                <div id="timer" class="text-7xl font-black mb-6 tracking-tighter text-main">25:00</div>
                <div class="flex gap-3">
                    <button id="startBtn" onclick="toggleTimer()" class="flex-1 ios-btn bg-blue-600 text-white py-4 rounded-2xl font-bold uppercase text-sm" style="background-color: var(--p-color);">Başlat</button>
                    <button onclick="resetTimer()" class="px-6 bg-slate-800/40 py-4 rounded-2xl ios-btn text-main">↺</button>
                </div>
            </div>

            <div class="card p-6 text-center shadow-lg">
                <div class="flex justify-between items-center mb-4 px-2">
                    <span class="text-[10px] font-black opacity-40 uppercase tracking-widest">Kronometre</span>
                </div>
                <div id="stopwatchDisplay" class="text-5xl font-black mb-6 tracking-tighter text-main font-mono">00:00.00</div>
                <div class="flex gap-3">
                    <button id="swStartBtn" onclick="toggleStopwatch()" class="flex-1 ios-btn bg-emerald-600 text-white py-4 rounded-2xl font-bold uppercase text-sm">Başlat</button>
                    <button onclick="resetStopwatch()" class="px-6 bg-slate-800/40 py-4 rounded-2xl ios-btn text-main">↺</button>
                </div>
            </div>
        </section>

        <section id="komiteTab" class="tab-content">
            <div class="card p-8 text-center mt-4 shadow-xl">
                <h2 class="text-xs font-bold opacity-50 mb-6 text-main uppercase tracking-widest">Kalan zamanı görmek için tarih giriniz.</h2>
                <input type="date" id="examDate" class="bg-slate-900/20 p-5 rounded-2xl w-full outline-none border border-slate-500/30 font-bold text-main mb-6">
                <div id="countdown" class="text-4xl font-black py-4 tracking-tighter" style="color: var(--p-color);">00g 00s 00d</div>
            </div>
        </section>

        <section id="kartlarTab" class="tab-content">
            <div class="card p-6 space-y-4 mt-4 shadow-xl text-center">
                <div id="flashDisplay" class="py-12 px-4 bg-slate-800/10 rounded-[30px] border-2 border-dashed border-blue-500/20 mb-4 cursor-pointer ios-btn">
                    <p id="cardText" class="text-xl font-bold text-main">Kart Ekleyin...</p>
                </div>
                <div class="space-y-2">
                    <input id="qInput" placeholder="Soru..." class="bg-slate-900/10 p-4 rounded-2xl w-full border border-slate-500/10 text-main outline-none focus:border-blue-500/50">
                    <input id="aInput" placeholder="Cevap..." class="bg-slate-900/10 p-4 rounded-2xl w-full border border-slate-500/10 text-main outline-none focus:border-blue-500/50">
                </div>
                <button onclick="addCard()" class="w-full text-white py-5 rounded-2xl font-black ios-btn" style="background-color: var(--p-color);">GÜNCELLE</button>
            </div>
        </section>

        <section id="ayarlarTab" class="tab-content">
            <div class="card p-6 space-y-8 mt-4 shadow-xl">
                <div class="flex justify-between items-center p-2">
                    <span class="font-bold text-main text-sm uppercase tracking-widest">Görünüm Modu</span>
                    <button onclick="toggleDarkMode()" id="modeBtn" class="bg-slate-800/50 px-6 py-2 rounded-full text-[10px] font-black text-main uppercase">Karanlık</button>
                </div>
                <div class="p-2">
                    <span class="font-bold text-main text-sm block mb-4 uppercase tracking-widest">Tema Rengi</span>
                    <div class="flex gap-4">
                        <button onclick="setTheme('theme-blue')" class="w-10 h-10 rounded-full bg-blue-500 ios-btn"></button>
                        <button onclick="setTheme('theme-red')" class="w-10 h-10 rounded-full bg-red-500 ios-btn"></button>
                        <button onclick="setTheme('theme-green')" class="w-10 h-10 rounded-full bg-emerald-500 ios-btn"></button>
                    </div>
                </div>
                <div class="pt-6 border-t border-slate-500/10">
                    <button onclick="resetAllData()" class="w-full bg-red-600/10 text-red-500 py-4 rounded-2xl font-black border border-red-500/10 ios-btn">VERİLERİ SIFIRLA</button>
                </div>
            </div>
        </section>
    </main>

    <div class="nav-container">
        <div class="indicator" id="navIndicator"></div>
        <div class="nav-item active" onclick="openTab('pomoTab', 'Focus', this, 0)">
            <span class="nav-icon">⏱️</span>
            <span class="nav-text">Focus</span>
        </div>
        <div class="nav-item" onclick="openTab('komiteTab', 'Sınav', this, 1)">
            <span class="nav-icon">📅</span>
            <span class="nav-text">Komite</span>
        </div>
        <div class="nav-item" onclick="openTab('kartlarTab', 'Kartlar', this, 2)">
            <span class="nav-icon">🧠</span>
            <span class="nav-text">Ezber</span>
        </div>
        <div class="nav-item" onclick="openTab('ayarlarTab', 'Panel', this, 3)">
            <span class="nav-icon">⚙️</span>
            <span class="nav-text">Ayarlar</span>
        </div>
    </div>

    <script>
        const audioCtx = new (window.AudioContext || window.webkitAudioContext)();
        function playTick() {
            const osc = audioCtx.createOscillator(); const gain = audioCtx.createGain();
            osc.connect(gain); gain.connect(audioCtx.destination);
            osc.type = 'sine'; osc.frequency.setValueAtTime(880, audioCtx.currentTime);
            gain.gain.setValueAtTime(0.1, audioCtx.currentTime);
            gain.gain.exponentialRampToValueAtTime(0.0001, audioCtx.currentTime + 0.1);
            osc.start(); osc.stop(audioCtx.currentTime + 0.1);
        }

        function loadImg(e) {
            const reader = new FileReader();
            reader.onload = function() {
                document.getElementById('anonIcon').classList.add('hidden');
                const img = document.getElementById('userImg');
                img.src = reader.result; img.classList.remove('hidden');
            }
            reader.readAsDataURL(e.target.files[0]);
        }

        function openTab(tabId, title, el, idx) {
            document.getElementById('pageTitle').innerText = title;
            document.querySelectorAll('.tab-content').forEach(t => t.classList.remove('active'));
            document.getElementById(tabId).classList.add('active');
            document.querySelectorAll('.nav-item').forEach(i => i.classList.remove('active'));
            el.classList.add('active');
            const containerWidth = document.querySelector('.nav-container').offsetWidth;
            document.getElementById('navIndicator').style.left = `${(idx * (containerWidth / 4)) + (containerWidth / 8) - 22.5}px`;
        }

        // POMODORO MANTIĞI (GÜNCELLENDİ)
        let timeLeft, pomoId, isPomoRunning = false;
        function updatePomoDisplay() {
            let m = Math.floor(timeLeft / 60), s = timeLeft % 60;
            document.getElementById('timer').innerText = `${m}:${s < 10 ? '0' : ''}${s}`;
        }
        function autoUpdateTimer() {
            if (!isPomoRunning) {
                const val = document.getElementById('pomoInput').value;
                timeLeft = (val > 0) ? val * 60 : 0;
                updatePomoDisplay();
            }
        }
        function toggleTimer() {
            const btn = document.getElementById('startBtn');
            if (!isPomoRunning) {
                if (!timeLeft) timeLeft = document.getElementById('pomoInput').value * 60;
                isPomoRunning = true;
                btn.innerText = 'DURDUR'; btn.classList.replace('bg-blue-600', 'bg-red-600');
                btn.style.backgroundColor = ''; // Dinamik renk için CSS override
                pomoId = setInterval(() => {
                    timeLeft--; updatePomoDisplay();
                    if (timeLeft <= 0) { clearInterval(pomoId); playTick(); alert('Süre Bitti!'); resetTimer(); }
                }, 1000);
            } else {
                clearInterval(pomoId); isPomoRunning = false;
                btn.innerText = 'DEVAM ET'; btn.classList.replace('bg-red-600', 'bg-emerald-600');
            }
        }
        function resetTimer() {
            clearInterval(pomoId); isPomoRunning = false;
            timeLeft = document.getElementById('pomoInput').value * 60;
            updatePomoDisplay();
            const btn = document.getElementById('startBtn');
            btn.innerText = 'BAŞLAT';
            btn.className = "flex-1 ios-btn bg-blue-600 text-white py-4 rounded-2xl font-bold uppercase text-sm";
            btn.style.backgroundColor = "var(--p-color)";
        }

        // KRONOMETRE MANTIĞI
        let swTime = 0, swId, isSwRunning = false;
        function updateSwDisplay() {
            let ms = Math.floor((swTime % 1000) / 10);
            let s = Math.floor((swTime / 1000) % 60);
            let m = Math.floor((swTime / 60000) % 60);
            document.getElementById('stopwatchDisplay').innerText = 
                `${m < 10 ? '0' : ''}${m}:${s < 10 ? '0' : ''}${s}.${ms < 10 ? '0' : ''}${ms}`;
        }
        function toggleStopwatch() {
            const btn = document.getElementById('swStartBtn');
            if (!isSwRunning) {
                isSwRunning = true;
                btn.innerText = 'DURDUR'; btn.classList.replace('bg-emerald-600', 'bg-red-600');
                let lastTime = Date.now();
                swId = setInterval(() => {
                    const now = Date.now(); swTime += now - lastTime;
                    lastTime = now; updateSwDisplay();
                }, 10);
            } else {
                clearInterval(swId); isSwRunning = false;
                btn.innerText = 'DEVAM ET'; btn.classList.replace('bg-red-600', 'bg-emerald-600');
            }
        }
        function resetStopwatch() {
            clearInterval(swId); isSwRunning = false;
            swTime = 0; updateSwDisplay();
            document.getElementById('swStartBtn').innerText = 'BAŞLAT';
            document.getElementById('swStartBtn').className = "flex-1 ios-btn bg-emerald-600 text-white py-4 rounded-2xl font-bold uppercase text-sm";
        }

        document.getElementById('examDate').addEventListener('change', function() {
            setInterval(() => {
                const diff = new Date(this.value).getTime() - new Date().getTime();
                if (diff < 0) { document.getElementById('countdown').innerText = 'Sınav Günü!'; return; }
                const d = Math.floor(diff/(1000*60*60*24)), h = Math.floor((diff%(1000*60*60*24))/(1000*60*60)), m = Math.floor((diff%(1000*60*60))/(1000*60));
                document.getElementById('countdown').innerText = `${d}g ${h}s ${m}d`;
            }, 1000);
        });

        function resetAllData() { if(confirm('Tüm veriler temizlenecek?')) location.reload(); }
        function toggleDarkMode() {
            const b = document.getElementById('mainBody');
            const isDark = b.classList.toggle('dark');
            b.classList.toggle('light', !isDark);
            document.getElementById('modeBtn').innerText = isDark ? 'KARANLIK' : 'AÇIK';
        }
        function setTheme(theme) {
            const b = document.getElementById('mainBody');
            b.classList.remove('theme-blue', 'theme-red', 'theme-green');
            b.classList.add(theme);
        }

        let currentAns = "";
        function addCard() {
            const q = document.getElementById('qInput').value, a = document.getElementById('aInput').value;
            if(q && a) { document.getElementById('cardText').innerText = q; currentAns = a;
                document.getElementById('qInput').value = ""; document.getElementById('aInput').value = "";
            }
        }
        document.getElementById('flashDisplay').onclick = function() {
            const txt = document.getElementById('cardText');
            txt.innerText = (txt.innerText === currentAns) ? "Soruya Dön..." : currentAns;
        };

        window.onload = () => {
            const containerWidth = document.querySelector('.nav-container').offsetWidth;
            document.getElementById('navIndicator').style.left = `${(containerWidth / 8) - 22.5}px`;
            timeLeft = 25 * 60; updatePomoDisplay();
        };
    </script>
</body>
</html>
