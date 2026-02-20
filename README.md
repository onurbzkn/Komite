<!DOCTYPE html>
<html lang="tr">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
    <title>Focus App v17</title>
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

        /* Profile & Action Sheet */
        #profile-menu {
            position: fixed; bottom: -100%; left: 0; width: 100%; padding: 20px;
            background: rgba(20, 20, 25, 0.98); backdrop-filter: blur(30px);
            border-radius: 30px 30px 0 0; z-index: 9999; transition: bottom 0.4s ease;
        }
        #profile-menu.show { bottom: 0; }
        .menu-btn { width: 100%; padding: 18px; text-align: center; border-bottom: 1px solid rgba(255,255,255,0.05); font-weight: 600; font-size: 15px; }

        #profile-img { width: 44px; height: 44px; border-radius: 50%; object-fit: cover; border: 2px solid var(--accent); background: #111; }
        
        /* Privacy Overlay */
        #privacy-modal {
            position: fixed; top: 0; left: 0; width: 100%; height: 100%;
            background: rgba(0,0,0,0.9); z-index: 10000; display: none; padding: 30px; overflow-y: auto;
        }
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
            <div id="timer" class="text-7xl font-black mb-6 tracking-tighter">25:00</div>
            <button onclick="startTimer()" id="startBtn" class="btn-ios w-full bg-blue-600 py-4 rounded-2xl font-bold text-lg">BAŞLAT</button>
        </div>
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

        <div class="glass-card space-y-2">
            <p class="font-bold mb-4 text-center uppercase tracking-widest text-[10px] text-gray-400">YASAL & BİLGİ</p>
            <button onclick="openPrivacy()" class="w-full py-4 bg-white/5 rounded-2xl text-xs font-bold uppercase flex items-center justify-between px-6">
                <span>GİZLİLİK POLİTİKASI</span>
                <i class="fas fa-chevron-right opacity-30"></i>
            </button>
            <button class="w-full py-4 bg-white/5 rounded-2xl text-xs font-bold uppercase flex items-center justify-between px-6">
                <span>KULLANIM KOŞULLARI</span>
                <i class="fas fa-chevron-right opacity-30"></i>
            </button>
            <button onclick="alert('Ustam Edition v17.0\nPlay Store Ready.')" class="w-full py-4 bg-white/5 rounded-2xl text-xs font-bold uppercase flex items-center justify-between px-6">
                <span>UYGULAMA HAKKINDA</span>
                <i class="fas fa-info-circle opacity-30"></i>
            </button>
        </div>

        <div class="glass-card text-center">
            <button onclick="clearData()" class="btn-ios w-full py-4 bg-red-900/10 border border-red-500/30 text-red-500 rounded-2xl font-bold uppercase text-xs tracking-widest">VERİLERİMİ SIFIRLA</button>
            <p class="text-[9px] text-gray-500 mt-6 font-bold uppercase tracking-widest text-center">V17.0 - IOS ELITE</p>
        </div>
    </div>

    <div id="privacy-modal">
        <h2 class="text-2xl font-black mb-6 uppercase">GİZLİLİK POLİTİKASI</h2>
        <div class="text-sm text-gray-400 space-y-4 leading-relaxed">
            <p><strong>1. Veri Toplama:</strong> Focus App, kullanıcı verilerini (profil resmi, komite tarihleri) yalnızca cihazınızın yerel hafızasında (LocalStorage) saklar. Sunucularımıza hiçbir veri iletilmez.</p>
            <p><strong>2. Kamera ve Galeri:</strong> Profil resmi eklemeniz durumunda galeri erişimi istenir. Seçilen görsel yalnızca uygulama içinde görüntülenir.</p>
            <p><strong>3. Reklam ve Takip:</strong> Uygulamamızda hiçbir reklam veya üçüncü taraf takip yazılımı bulunmamaktadır.</p>
        </div>
        <button onclick="closePrivacy()" class="w-full bg-blue-600 py-4 rounded-2xl font-bold mt-10 uppercase">ANLADIM</button>
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
        function removeProfilePic() { 
            document.getElementById('profile-img').src = defaultPic; 
            localStorage.removeItem('userProfilePic'); 
            toggleProfileMenu(); 
        }
        if(localStorage.getItem('userProfilePic')) document.getElementById('profile-img').src = localStorage.getItem('userProfilePic');

        function openPrivacy() { document.getElementById('privacy-modal').style.display = 'block'; }
        function closePrivacy() { document.getElementById('privacy-modal').style.display = 'none'; }

        function showPage(id) {
            document.querySelectorAll('.page-content').forEach(p => p.classList.remove('active'));
            document.getElementById('page-' + id).classList.add('active');
            document.querySelectorAll('.nav-item').forEach(n => n.classList.remove('active'));
            document.getElementById('nav-' + id).classList.add('active');
            document.getElementById('headerTitle').innerText = id.toUpperCase();
            window.scrollTo(0,0);
        }

        function setTheme(t) { document.body.className = t; localStorage.setItem('selectedTheme', t); }
        if(localStorage.getItem('selectedTheme')) setTheme(localStorage.getItem('selectedTheme'));

        let timeLeft = 25*60, tId = null;
        function updateTimer() { let m = Math.floor(timeLeft/60), s = timeLeft%60; document.getElementById('timer').innerText = `${m}:${s<10?'0':''}${s}`; }
        function startTimer() {
            if(!tId) { tId = setInterval(() => { timeLeft--; updateTimer(); if(timeLeft<=0) clearInterval(tId); }, 1000); document.getElementById('startBtn').innerText = "DURAKLAT"; }
            else { clearInterval(tId); tId = null; document.getElementById('startBtn').innerText = "BAŞLAT"; }
        }

        function clearData() { if(confirm("VERİLERİNİ SIFIRLA?")) { localStorage.clear(); location.reload(); } }
    </script>
</body>
</html>
