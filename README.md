<html lang="ja">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>HODOGAYA AREA DEFENSE V15.0</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <link href="https://fonts.googleapis.com/css2?family=Noto+Sans+JP:wght@500;900&family=Share+Tech+Mono&display=swap" rel="stylesheet">
    <style>
        body { font-family: 'Noto Sans JP', sans-serif; background-color: #080808; color: #eee;
            background-image: linear-gradient(#111 1px, transparent 1px), linear-gradient(90deg, #111 1px, transparent 1px);
            background-size: 40px 40px; /* グリッドを大きくしてスッキリさせた */
        }
        .mono { font-family: 'Share Tech Mono', monospace; }
        
        /* パネルの視認性向上 */
        .nerv-panel { 
            background: rgba(15, 15, 15, 0.95); 
            border: 1px solid #333; 
            border-left: 6px solid #E60012; 
            box-shadow: 0 4px 20px rgba(0,0,0,0.5);
        }

        /* ボタンをより押しやすく、はっきりと */
        .btn-line { 
            transition: all 0.2s ease; 
            border: 2px solid #222; 
            background-color: #000;
            height: 70px; /* 高さを出して押しやすく */
        }
        .btn-line:hover { transform: scale(1.02); border-color: #555; }

        .hover-jo:hover { background-color: #0072bc !important; }
        .hover-js:hover { background-color: #e21f26 !important; }
        .hover-so:hover { background-color: #003f8e !important; }
        .hover-kk:hover { background-color: #da041a !important; }

        .symbol { width: 40px; height: 40px; border: 2px solid #fff; border-radius: 4px; font-weight: 900; flex-shrink: 0; }
        .symbol-jo { background-color: #0072bc; }
        .symbol-js { background-color: #e21f26; }
        .symbol-so { background-color: #003f8e; }
        .symbol-kk { background-color: #da041a; }

        /* フィード項目の視認性（ここが一番の改善点） */
        .feed-item {
            padding: 12px;
            margin-bottom: 8px;
            background: rgba(255,255,255,0.03);
            border-radius: 4px;
            line-height: 1.6; /* 行間を広く */
        }
        .feed-hit { 
            border: 1px solid #ff4444 !important;
            background: linear-gradient(90deg, rgba(150,0,0,0.4) 0%, rgba(20,20,20,0.9) 100%) !important;
        }

        .status-pulse { animation: pulse 2s infinite; }
        @keyframes pulse { 0%, 100% { opacity: 1; } 50% { opacity: .4; } }
    </style>
</head>
<body class="p-4 md:p-8">

    <div class="max-w-6xl mx-auto space-y-8">
        <!-- ヘッダー：スッキリと整理 -->
        <header class="flex flex-col md:flex-row justify-between items-center border-b-2 border-[#E60012] pb-4 gap-4">
            <div class="text-center md:text-left">
                <h1 class="text-5xl font-black text-[#E60012] italic tracking-tighter italic">HODOGAYA <span class="text-white not-italic text-4xl">AREA DEFENSE</span></h1>
                <div class="flex items-center gap-2 mt-2 justify-center md:justify-start">
                    <span class="w-3 h-3 bg-[#00ff41] rounded-full status-pulse"></span>
                    <span class="mono text-xs tracking-widest text-[#00ff41]">SYSTEM_ACTIVE // STABLE_V15</span>
                </div>
            </div>
            <div id="clock" class="text-6xl font-black mono text-white tracking-tighter">00:00:00</div>
        </header>

        <div class="grid grid-cols-1 lg:grid-cols-12 gap-8">
            <!-- 左：主要インフラ -->
            <div class="lg:col-span-7 space-y-8">
                <section class="nerv-panel p-6">
                    <h2 class="text-sm font-black bg-[#E60012] px-3 py-1 text-white tracking-[0.3em] uppercase mb-6 w-fit italic">Transportation</h2>
                    <div class="grid grid-cols-1 sm:grid-cols-2 gap-4">
                        <a href="https://transit.yahoo.co.jp/diainfo/29/0" target="_blank" class="btn-line hover-jo px-4 flex items-center gap-4">
                            <div class="symbol symbol-jo flex items-center justify-center">JO</div>
                            <span class="text-lg font-black uppercase">横須賀線 ≫</span>
                        </a>
                        <a href="https://transit.yahoo.co.jp/diainfo/25/0" target="_blank" class="btn-line hover-js px-4 flex items-center gap-4">
                            <div class="symbol symbol-js flex items-center justify-center">JS</div>
                            <span class="text-lg font-black uppercase">湘南新宿 ≫</span>
                        </a>
                        <a href="https://transit.yahoo.co.jp/diainfo/125/0" target="_blank" class="btn-line hover-so px-4 flex items-center gap-4">
                            <div class="symbol symbol-so flex items-center justify-center">SO</div>
                            <span class="text-lg font-black uppercase">相鉄線 ≫</span>
                        </a>
                        <a href="https://transit.yahoo.co.jp/diainfo/120/0" target="_blank" class="btn-line hover-kk px-4 flex items-center gap-4">
                            <div class="symbol symbol-kk flex items-center justify-center">KK</div>
                            <span class="text-lg font-black uppercase">京急本線 ≫</span>
                        </a>
                    </div>
                </section>

                <section class="nerv-panel p-6">
                    <h2 class="text-sm font-black bg-[#333] px-3 py-1 text-white tracking-[0.3em] uppercase mb-6 w-fit italic">Weather Overview</h2>
                    <div id="overview-display" class="text-lg leading-relaxed text-[#ddd] bg-[#000] p-6 border border-[#222] min-h-[120px]">読み込み中...</div>
                </section>
            </div>

            <!-- 右：ライブフィード -->
            <div class="lg:col-span-5 space-y-8">
                <!-- NERVフィード：高さを調整し、文字を見やすく -->
                <section class="nerv-panel p-6 border-l-[#E60012]">
                    <h2 class="text-sm font-black bg-[#E60012] px-3 py-1 text-white tracking-[0.3em] uppercase mb-4 w-fit italic">NERV Disaster</h2>
                    <ul id="nerv-feed" class="overflow-y-auto h-[300px] pr-2 space-y-2"></ul>
                </section>

                <!-- 運行情報フィード -->
                <section class="nerv-panel p-6 border-l-[#555]">
                    <h2 class="text-sm font-black bg-[#333] px-3 py-1 text-white tracking-[0.3em] uppercase mb-4 w-fit italic">Transit Live</h2>
                    <ul id="transit-feed" class="overflow-y-auto h-[300px] pr-2 space-y-2"></ul>
                </section>

                <!-- 下部ボタン：さらに大きく -->
                <div class="grid grid-cols-3 gap-3">
                    <a href="https://www.jma.go.jp/bosai/nowc/#lat:35.44&lon:139.59&zoom:12" target="_blank" class="bg-[#0033cc] hover:bg-[#0055ff] text-center py-5 text-xs font-black transition border-b-4 border-[#001144]">雨雲の動き</a>
                    <a href="https://www.jma.go.jp/bosai/risk/#lat:35.44&lon:139.59&zoom:12" target="_blank" class="bg-[#6600cc] hover:bg-[#8000ff] text-center py-5 text-xs font-black transition border-b-4 border-[#220044]">キキクル</a>
                    <a href="https://typhoon.yahoo.co.jp/weather/jp/earthquake/kyoshin/" target="_blank" class="bg-[#cc6600] hover:bg-[#ff8000] text-center py-5 text-xs font-black transition border-b-4 border-[#442200]">強震モニタ</a>
                </div>
            </div>
        </div>
    </div>

    <script>
        setInterval(() => {
            document.getElementById('clock').innerText = new Date().toLocaleTimeString('ja-JP', { hour12: false });
        }, 1000);

        async function loadOverview() {
            try {
                const res = await fetch("https://www.jma.go.jp/bosai/forecast/data/overview_forecast/140000.json");
                const data = await res.json();
                document.getElementById('overview-display').innerText = data.text;
            } catch (e) { document.getElementById('overview-display').innerText = "取得エラー"; }
        }

        async function loadNervFeed() {
            try {
                const res = await fetch(`https://api.rss2json.com/v1/api.json?rss_url=${encodeURIComponent("https://unnerv.jp/@UN_NERV.rss")}`);
                const data = await res.json();
                document.getElementById('nerv-feed').innerHTML = data.items.map(item => {
                    const content = item.description.replace(/<[^>]+>/g, '').trim();
                    return `<li class="feed-item"><div class="text-[#eee] font-bold text-[13px]">${content}</div><div class="text-[10px] mono text-[#666] mt-2">${new Date(item.pubDate).toLocaleString()}</div></li>`;
                }).join('');
            } catch (e) {}
        }

        async function loadTransitFeed() {
            try {
                const res = await fetch(`https://api.rss2json.com/v1/api.json?rss_url=${encodeURIComponent("https://mastodon.social/@delainfo.rss")}`);
                const data = await res.json();
                const targets = ["横須賀", "湘南新宿", "相鉄", "京急"];
                document.getElementById('transit-feed').innerHTML = data.items.map(item => {
                    const content = item.description.replace(/<[^>]+>/g, '').trim();
                    const isHit = targets.some(keyword => content.includes(keyword));
                    return `<li class="feed-item ${isHit ? 'feed-hit' : ''}"><div class="${isHit ? 'text-white font-black text-[15px]' : 'text-[#ccc] text-[13px]'}">${content}</div><div class="text-[10px] mono text-[#666] mt-2">${new Date(item.pubDate).toLocaleString()}</div></li>`;
                }).join('');
            } catch (e) {}
        }

        loadOverview(); loadNervFeed(); loadTransitFeed();
        setInterval(loadOverview, 600000); setInterval(loadNervFeed, 300000); setInterval(loadTransitFeed, 300000);
    </script>
</body>
</html>
