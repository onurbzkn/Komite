<!DOCTYPE html>
<html lang="tr">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Focus App v10.4</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <link href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.0.0/css/all.min.css" rel="stylesheet">
    <style>
        :root { --bg: #09090b; }
        body { 
            background-color: var(--bg); 
            color: white; 
            font-family: -apple-system, sans-serif; 
            margin: 0; padding: 0;
            overflow-y: auto !important; /* Kaydırmayı zorla açtık */
            min-height: 100vh;
        }
        
        .liquid-nav {
            position: fixed; bottom: 25px; left: 50%; transform: translateX(-50%);
            width: 85%; max-width: 400px; height: 70px;
            background: rgba(28, 28, 30, 0.8);
            backdrop-filter: blur(20px); -webkit-backdrop-filter: blur(20px);
            border: 1px solid rgba(255, 255, 255, 0.1);
            border-radius: 35px; display: flex; justify-content: space-around; align-items: center;
            z-index: 9999;
        }

        .glass-card {
            background: rgba(255, 255, 255, 0.04);
            border: 1px solid rgba(255, 255, 255, 0.1);
            border-radius: 30px; padding: 25px; margin-bottom: 20px;
        }

        .btn-ios:active { transform: scale(0.95); }
        
        /* Sayfa bölümleri */
        .page-content { display: none; padding: 100px 20px 150px 20px; }
        .page-content.active { display: block; }

        .nav-item { color: #666; transition: 0.3s; }
        .nav-item.active { color: #3b82f6; }
    </style>
</head>
<body>

    <div style="position: fixed; top: 0; width: 100%; background: rgba(9,9,11,0.9); backdrop-filter: blur(10px); z-index: 100; padding: 20px 25px;">
        <h1 id="headerTitle" style="font-size: 24px; font-weight: 900; letter-spacing: -1px; margin: 0;">ODAK</h1>
        <p style="font-size: 10px; color: #3b82f6; font-weight: bold; margin: 0; letter-spacing: 2px;">MEDİKAL ASİSTAN</p>
    </div>

    <div id="page-odak" class="page-content active">
        <div class="glass-card text-center">
            <p style="font-size: 11px; color: #888; font-weight: bold; margin-bottom: 10px;">POMODORO</p>
            <div id="timer" style="font-size: 64px; font-weight: 900; margin-bottom: 20px;">25:00</div>
            <button onclick="startTimer()" id="startBtn" class="btn-ios" style="width: 100%; background: #2563eb; padding: 15px; border-radius: 18px; font-weight: bold; font-size: 18px; border: none; color: white;">BAŞLAT</button>
            <button onclick="resetTimer()" style="margin-top: 15px; background: none; border: none; color: #555; font-weight: bold; font-size: 12px;">SIFIRLA</button>
        </div>

        <div class="glass-card text-center">
            <p style="font-size: 11px; color: #888; font-weight: bold; margin-bottom: 10px;">KRONOMETRE</p>
            <div id="stopwatch" style="font-size: 40px; font-weight: bold; margin-bottom: 20px; font-family: monospace;">00:00.00</div>
            <div style="display: flex; gap: 10px;">
                <button onclick="toggleStopwatch()" id="swBtn" class="btn-ios" style="flex: 2; background: #059669; padding: 15px; border-radius: 18px; font-weight: bold; border: none; color: white;">BAŞLAT</button>
                <button onclick="resetStopwatch()" class="btn-ios" style="flex: 1; background: #27272a; padding: 15px; border-radius: 18px; font-weight: bold; border: none; color: white;">SIFIRLA</button>
            </div>
        </div>
    </div>

    <div id="page-komite" class="page-content">
        <div class="glass-card">
            <p style="text-align: center; color: #aaa; margin-bottom: 20px; font-size: 14px;">Zamanlayıcı için Komite tarihini giriniz.</p>
            <input type="date" id="examDate" style="width: 100%; background: #1c1c1e; padding: 18px; border-radius: 15px; border: 1px solid #333; color: white; margin-bottom: 25px; outline: none;">
            <div id="countdown" style="text-align: center; font-size: 48px; font-weight: 900; background: linear-gradient(to bottom, #fff, #444); -webkit-background-clip: text; -webkit-text-fill-color: transparent;">TARİH SEÇİN</div>
        </div>
    </div>

    <div id="page-ezber" class="page-content">
        <div class="glass-card" onclick="flipCard()" style="min-height: 200px; display: flex; align-items: center; justify-content: center; background: linear-gradient(135deg, #1e3a8a 0%, #09090b 100%);">
            <p id="cardDisplay" style="font-size: 20px; font-weight: 500; text-align: center;">Dokun ve Öğren</p>
        </div>
        <div class="glass-card">
            <input id="qInput" placeholder="Soru..." style="width: 100%; background: #1c1c1e; padding: 15px; border-radius: 12px; border: none; color: white; margin-bottom: 10px; outline: none;">
            <input id="aInput" placeholder="Cevap..." style="width: 100%; background: #1c1c1e; padding: 15px; border-radius: 12px; border: none; color: white; margin-bottom: 15px; outline: none;">
            <button onclick="addCard()" style="width: 100%; background: #2563eb; padding: 15px; border-radius: 15px; font-weight: bold; border: none; color: white;">KARTI KAYDET</button>
        </div>
    </div>

    <div id="page-ayarlar" class="page-content">
        <div class="glass-card">
            <p style="font-weight: bold; margin-bottom: 15px;">Uygulama Teması</p>
            <div style="display: flex; gap: 10px;">
                <div style="flex: 1; height: 50px; background: #1e3a8a; border-radius: 12px; border: 2px solid #3b82f6;"></div>
                <div style="flex: 1; height: 50px; background: #000; border-radius: 12px; border: 1px solid #333;"></div>
            </div>
        </div>
        <div class="glass-card">
            <div style="display: flex; justify-content: space-between; align-items: center; margin-bottom: 30px;">
                <span style="font-weight: bold;">Karanlık Mod</span>
                <div style="width: 50px; height: 26px; background: #2563eb; border-radius: 15px; position: relative;">
                    <div style="position: absolute; right: 3px; top: 3px; width: 20px; height: 20px; background: white; border-radius: 50%;"></div>
                </div>
            </div>
            <button onclick="clearData()" style="width: 100%; padding: 18px; background: rgba(239, 68, 68, 0.1); border: 1px solid #ef4444; border-radius: 18px; color: #ef4444; font-weight: bold;">VERİLERİMİ SIFIRLA</button>
            <p style="text-align: center; font-size: 10px; color: #444; margin-top: 20px;">V10.4 - USTAM EDITION</p>
        </div>
    </div>

    <nav class="liquid-nav">
        <div onclick="showPage('odak')" id="nav-odak" class="nav-item active text-center"><i class="fas fa-clock"></i><div style="font-size: 8px; font-weight: bold;">ODAK</div></div>
        <div onclick="showPage('komite')" id="nav-komite" class="nav-item text-center"><i class="fas fa-calendar"></i><div style="font-size: 8px; font-weight: bold;">KOMİTE</div></div>
        <div onclick="showPage('ezber')" id="nav-ezber" class="nav-item text-center"><i class="fas fa-brain"></i><div style="font-size: 8px; font-weight: bold;">EZBER</div></div>
        <div onclick="showPage('ayarlar')" id="nav-ayarlar" class="nav-item text-center"><i class="fas fa-cog"></i><div style="font-size: 8px; font-weight: bold;">AYARLAR</div></div>
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
                btn.innerText = "DURAKLAT"; btn.style.background = "#3f3f46";
            } else {
                clearInterval(timerId); timerId = null;
                btn.innerText = "DEVAM ET"; btn.style.background = "#2563eb";
            }
        }
        function resetTimer() { clearInterval(timerId); timerId = null; timeLeft = 25 * 60; updateTimer(); document.getElementById('startBtn').innerText = "BAŞLAT"; document.getElementById('startBtn').style.background = "#2563eb"; }

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
