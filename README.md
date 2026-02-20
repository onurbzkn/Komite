<!DOCTYPE html>
<html lang="tr">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
    <title>Medikal Komite Asistanı</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <style>
        body { background-color: #121212; color: #e0e0e0; overflow-y: auto !important; min-height: 100vh; }
        .card { background-color: #1e1e1e; border: 1px solid #333; }
        .flip-card { perspective: 1000px; height: 180px; cursor: pointer; }
        .flip-card-inner { position: relative; width: 100%; height: 100%; text-align: center; transition: transform 0.6s; transform-style: preserve-3d; }
        .flip-card.flipped .flip-card-inner { transform: rotateY(180deg); }
        .flip-card-front, .flip-card-back { position: absolute; width: 100%; height: 100%; backface-visibility: hidden; display: flex; align-items: center; justify-content: center; padding: 15px; border-radius: 12px; }
        .flip-card-front { background: linear-gradient(135deg, #1e3a8a, #1e40af); }
        .flip-card-back { transform: rotateY(180deg); background-color: #374151; }
    </style>
</head>
<body class="px-4 py-8">

<div class="max-w-md mx-auto pb-20"> <div class="flex justify-between items-center mb-8">
        <h1 class="text-2xl font-bold text-blue-400">Komite Asistanı</h1>
        <button onclick="toggleSettings()" class="text-xl">⚙️</button>
    </div>

    <div class="card p-5 rounded-xl mb-6 shadow-lg text-center">
        <h2 class="text-sm uppercase tracking-widest text-gray-400 mb-2 font-bold">Pomodoro</h2>
        <div id="timer" class="text-6xl font-black mb-4">25:00</div>
        <div class="flex justify-center gap-3">
            <button onclick="startTimer()" id="startBtn" class="bg-green-600 px-6 py-2 rounded-full font-bold">BAŞLAT</button>
            <button onclick="resetTimer()" class="bg-red-900/50 px-6 py-2 rounded-full">SIFIRLA</button>
        </div>
    </div>

    <div class="card p-5 rounded-xl mb-6 shadow-lg">
        <h2 class="text-sm uppercase tracking-widest text-gray-400 mb-4 font-bold">Komite Sayacı</h2>
        <input type="date" id="examDate" class="bg-gray-800 p-3 rounded-lg w-full mb-4 outline-none border border-gray-700 text-white">
        <div id="countdown" class="text-xl font-mono text-center text-yellow-500 font-bold">Tarih Seçin</div>
    </div>

    <div class="card p-5 rounded-xl shadow-lg">
        <h2 class="text-sm uppercase tracking-widest text-gray-400 mb-4 font-bold">Hafıza Kartı</h2>
        <div id="flashcard" class="flip-card mb-4" onclick="this.classList.toggle('flipped')">
            <div class="flip-card-inner">
                <div class="flip-card-front"><p id="cardFrontText">Soru?</p></div>
                <div class="flip-card-back"><p id="cardBackText">Cevap.</p></div>
            </div>
        </div>
        <div class="space-y-2">
            <input id="qInput" placeholder="Yeni Soru..." class="bg-gray-800 p-2 rounded w-full border border-gray-700 text-sm text-white">
            <input id="aInput" placeholder="Yeni Cevap..." class="bg-gray-800 p-2 rounded w-full border border-gray-700 text-sm text-white">
            <button onclick="addCard()" class="w-full bg-blue-600 py-2 rounded-lg font-bold">KARTI KAYDET</button>
        </div>
    </div>
</div>

<script>
    // POMODORO
    let timeLeft = 25 * 60, timerId;
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

    // COUNTDOWN & LOCAL STORAGE
    const examInput = document.getElementById('examDate');
    const countdownEl = document.getElementById('countdown');

    // Kayıtlı tarihi yükle
    if(localStorage.getItem('komiteTarihi')) {
        examInput.value = localStorage.getItem('komiteTarihi');
        baslatGeriSayim();
    }

    examInput.addEventListener('change', function() {
        localStorage.setItem('komiteTarihi', this.value);
        baslatGeriSayim();
    });

    function baslatGeriSayim() {
        setInterval(() => {
            const exam = new Date(examInput.value).getTime();
            const diff = exam - new Date().getTime();
            if (diff < 0) { countdownEl.innerText = "Sınav Günü!"; return; }
            const d = Math.floor(diff / (1000 * 60 * 60 * 24));
            const h = Math.floor((diff % (1000 * 60 * 60 * 24)) / (1000 * 60 * 60));
            const m = Math.floor((diff % (1000 * 60 * 60)) / (1000 * 60));
            countdownEl.innerText = `${d}g ${h}s ${m}d kaldı`;
        }, 1000);
    }

    // FLASHCARD & LOCAL STORAGE
    function addCard() {
        const q = document.getElementById('qInput').value, a = document.getElementById('aInput').value;
        if(q && a) {
            const card = { soru: q, cevap: a };
            localStorage.setItem('sonKart', JSON.stringify(card));
            kartGoster(q, a);
            document.getElementById('qInput').value = ''; document.getElementById('aInput').value = '';
        }
    }

    function kartGoster(q, a) {
        document.getElementById('cardFrontText').innerText = q;
        document.getElementById('cardBackText').innerText = a;
    }

    // Kayıtlı kartı yükle
    const kayitliKart = JSON.parse(localStorage.getItem('sonKart'));
    if(kayitliKart) kartGoster(kayitliKart.soru, kayitliKart.cevap);

    function toggleSettings() { alert("Ayarlar: Veriler telefonuna kaydediliyor."); }
</script>
</body>
</html>
