<!DOCTYPE html>
<html lang="tr">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Medikal Komite Asistanı</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <style>
        .dark { background-color: #121212; color: #e0e0e0; }
        .card { background-color: #1e1e1e; border: 1px solid #333; }
        .flip-card { perspective: 1000px; height: 200px; cursor: pointer; }
        .flip-card-inner { position: relative; width: 100%; height: 100%; text-align: center; transition: transform 0.6s; transform-style: preserve-3d; }
        .flip-card.flipped .flip-card-inner { transform: rotateY(180deg); }
        .flip-card-front, .flip-card-back { position: absolute; width: 100%; height: 100%; backface-visibility: hidden; display: flex; align-items: center; justify-content: center; padding: 20px; border-radius: 12px; }
        .flip-card-back { transform: rotateY(180deg); background-color: #2d3748; }
    </style>
</head>
<body class="dark transition-colors duration-300">

<div class="container mx-auto p-6">
    <div class="flex justify-between items-center mb-10">
        <h1 class="text-3xl font-bold text-blue-400">Komite Asistanı v1.0</h1>
        <button onclick="toggleSettings()" class="p-2 bg-gray-800 rounded-full">⚙️ Ayarlar</button>
    </div>

    <div class="grid grid-cols-1 md:grid-cols-2 gap-8">
        
        <div class="space-y-6">
            <div class="card p-6 rounded-2xl shadow-xl">
                <h2 class="text-xl mb-4 font-semibold">📅 Komite Geri Sayım</h2>
                <input type="date" id="examDate" class="bg-gray-800 p-2 rounded w-full mb-4 outline-none border border-gray-700">
                <div id="countdown" class="text-2xl font-mono text-center text-yellow-500">Tarih Giriniz...</div>
            </div>

            <div class="card p-6 rounded-2xl shadow-xl text-center">
                <h2 class="text-xl mb-4 font-semibold">⏱️ Pomodoro</h2>
                <div id="timer" class="text-5xl font-bold mb-6">25:00</div>
                <div class="flex justify-center gap-4">
                    <button onclick="startTimer()" class="bg-green-600 px-6 py-2 rounded-lg">Başlat</button>
                    <button onclick="resetTimer()" class="bg-red-600 px-6 py-2 rounded-lg">Sıfırla</button>
                </div>
            </div>
        </div>

        <div class="card p-6 rounded-2xl shadow-xl">
            <h2 class="text-xl mb-4 font-semibold">🧠 Hafıza Kartları</h2>
            <div id="flashcard" class="flip-card mb-4" onclick="this.classList.toggle('flipped')">
                <div class="flip-card-inner">
                    <div class="flip-card-front bg-blue-900">
                        <p id="cardFrontText">Örn: Cushing Sendromu temel belirtisi?</p>
                    </div>
                    <div class="flip-card-back">
                        <p id="cardBackText">Ay dede çehresi, buffalo hörgücü.</p>
                    </div>
                </div>
            </div>
            <div class="space-y-2">
                <input id="qInput" placeholder="Soru..." class="bg-gray-800 p-2 rounded w-full border border-gray-700">
                <input id="aInput" placeholder="Cevap..." class="bg-gray-800 p-2 rounded w-full border border-gray-700">
                <button onclick="addCard()" class="w-full bg-blue-600 py-2 rounded-lg">Kart Ekle</button>
            </div>
        </div>
    </div>
</div>

<div id="settingsModal" class="hidden fixed inset-0 bg-black bg-opacity-50 flex items-center justify-center">
    <div class="bg-gray-900 p-8 rounded-2xl border border-gray-700 w-80">
        <h3 class="text-lg font-bold mb-4">Ayarlar</h3>
        <label class="flex items-center gap-3 cursor-pointer">
            <input type="checkbox" checked onchange="toggleDarkMode()"> Karanlık Mod
        </label>
        <button onclick="toggleSettings()" class="mt-6 w-full bg-gray-700 py-2 rounded">Kapat</button>
    </div>
</div>

<script>
    // Pomodoro Logic
    let timeLeft = 25 * 60;
    let timerId;
    function updateTimer() {
        let mins = Math.floor(timeLeft / 60);
        let secs = timeLeft % 60;
        document.getElementById('timer').innerText = `${mins}:${secs < 10 ? '0' : ''}${secs}`;
    }
    function startTimer() {
        if (!timerId) {
            timerId = setInterval(() => {
                timeLeft--;
                updateTimer();
                if (timeLeft <= 0) clearInterval(timerId);
            }, 1000);
        }
    }
    function resetTimer() {
        clearInterval(timerId);
        timerId = null;
        timeLeft = 25 * 60;
        updateTimer();
    }

    // Countdown Logic
    document.getElementById('examDate').addEventListener('change', function() {
        const exam = new Date(this.value).getTime();
        setInterval(() => {
            const now = new Date().getTime();
            const diff = exam - now;
            const days = Math.floor(diff / (1000 * 60 * 60 * 24));
            const hours = Math.floor((diff % (1000 * 60 * 60 * 24)) / (1000 * 60 * 60));
            document.getElementById('countdown').innerText = `${days} Gün ${hours} Saat Kaldı`;
        }, 1000);
    });

    // Flashcard Logic
    function addCard() {
        const q = document.getElementById('qInput').value;
        const a = document.getElementById('aInput').value;
        if(q && a) {
            document.getElementById('cardFrontText').innerText = q;
            document.getElementById('cardBackText').innerText = a;
            document.getElementById('qInput').value = '';
            document.getElementById('aInput').value = '';
        }
    }

    // Settings
    function toggleSettings() { document.getElementById('settingsModal').classList.toggle('hidden'); }
    function toggleDarkMode() { document.body.classList.toggle('dark'); }
</script>

</body>
</html>
