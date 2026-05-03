<html lang="ja">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>HODOGAYA AREA DEFENSE V11.2</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <link href="https://fonts.googleapis.com/css2?family=Noto+Sans+JP:wght@700;900&family=Share+Tech+Mono&display=swap" rel="stylesheet">
    <style>
        body { font-family: 'Noto Sans JP', sans-serif; background-color: #050505; color: #eee; overflow-x: hidden;
            background-image: linear-gradient(#111 1px, transparent 1px), linear-gradient(90deg, #111 1px, transparent 1px);
            background-size: 30px 30px;
        }
        .mono { font-family: 'Share Tech Mono', monospace; }
        .nerv-panel { background: rgba(20, 20, 20, 0.9); border: 1px solid #333; border-left: 4px solid #E60012; }
        .symbol { display: inline-flex; align-items: center; justify-content: center; width: 32px; height: 32px; border: 2px solid #fff; border-radius: 4px; font-size: 14px; font-weight: 900; margin-right: 10px; flex-shrink: 0; }
        .symbol-jo { background-color: #0072bc; } .symbol-js { background-color: #e21f26; } .symbol-so { background-color: #003f8e; } .symbol-kk { background-color: #da041a; }
        .btn-official { transition: all 0.2s; position: relative; overflow: hidden; }
        .btn-official:hover { background-color: #E60012; transform: scale(1.02); }
        .status-pulse { animation: pulse 2s infinite; }
        @keyframes pulse { 0%, 100% { opacity: 1; } 50% { opacity: .3; } }
        ::-webkit-scrollbar { width: 5px; }
        ::-webkit-scrollbar-thumb { background: #E60012; }
    </style>
</head>
<body class="p-4 md:p-8">

    <div class="max-w-6xl mx-auto space-y-6">
        <!-- システムヘッダー -->
        <div class="flex justify-between items-center text-[10px] font-bold tracking-[0.2em] text-[#666] border-b border-[#333] pb-1">
            <div class="flex items-center gap-2 text-[#00ff41]">
                <span class="w-2 h-2 bg-[#00ff41] rounded-full status-pulse"></span>
                STABLE_MODE: ACTIVE
            </div>
            <div class="mono text-[#E60012]">UI_UPDATE: LABEL_COLOR_FIX</div>
        </div>

        <header class="flex flex-col md:flex-row justify-between items-end gap-2">
            <div>
                <h1 class="text-4xl font-black text-[#E60012] italic tracking-tighter leading-none">HODOGAYA <span class="text-white not-italic">AREA DEFENSE</span></h1>
            </div>
            <div id="clock" class="text-5xl font-black mono text-white leading-none tracking-tighter">00:00:00</div>
        </header>

        <div class="grid grid-cols-1 lg:grid-cols-12 gap-6">
            <div class="lg:col-span-7 space-y-6">
                <!-- 運行情報 -->
                <div class="nerv-panel p-5">
                    <h2 class="text-xs font-black bg-[#E60012] px-2 py-0.5 text-white tracking-widest uppercase mb-4 w-fit">Transportation</h2>
                    <div class="grid grid-cols-1 sm:grid-cols-2 gap-4">
                        <a href="https://transit.yahoo.co.jp/diainfo/29/0" target="_blank" class="btn-official bg-[#000] border border-[#222] p-4 flex items-center">
                            <div class="symbol symbol-jo">JO</div>
                            <span class="text-sm font-black text-[#eee]">横須賀線 ≫</span>
                        </a>
                        <a href="https://transit.yahoo.co.jp/diainfo/25/0" target="_blank" class="btn-official bg-[#000] border border-[#222] p-4 flex items-center">
                            <div class="symbol symbol-js">JS</div>
                            <span class="text-sm font-black text-[#eee]">湘南新宿ライン ≫</span>
                        </a>
                        <a href="https://transit.yahoo.co.jp/diainfo/125/0" target="_blank" class="btn-official bg-[#000] border border-[#222] p-4 flex items-center">
                            <div class="symbol symbol-so">SO</div>
                            <span class="text-sm font-black text-[#eee]">相鉄線 ≫</span>
                        </a>
                        <a href="https://transit.yahoo.co.jp/diainfo/120/0" target="_blank" class="btn-official bg-[#000] border border-[#222] p-4 flex items-center">
                            <div class="symbol symbol-kk">KK</div>
                            <span class="text-sm font-black text-[#eee]">京急本線 ≫</span>
                        </a>
                    </div>
                </div>

                <!-- 天気概況 -->
                <div class="nerv-panel p-5 border-l-[#444]">
                    <div class="flex justify-between items-center mb-4">
                        <h2 class="text-xs font-black bg-[#333] px-2 py-0.5 text-white tracking-widest uppercase">Overview</h2>
                        <a href="https://www.jma.go.jp/bosai/warning/#area_code=140000&lang=ja&area_type=offices" target="_blank" class="text-[9px] text-[#E60012] font-black underline italic">JMA OFFICIAL ≫</a>
                    </div>
                    <div id="overview-display" class="text-[13px] leading-relaxed text-[#bbb] bg-[#080808] p-4 border border-[#222]">Loading weather overview...</div>
                </div>
            </div>

            <!-- 右カラム -->
            <div class="lg:col-span-5 space-y-6">
                <div class="nerv-panel p-5 h-[400px] flex flex-col border-l-[#333]">
                    <h2 class="text-xs font-black bg-[#333] px-2 py-0.5 text-white mb-3 w-fit tracking-widest uppercase">NERV Feed</h2>
                    <ul id="nerv-display" class="overflow-y-auto text-[11px] space-y-3 text-[#888] pr-2 flex-grow"></ul>
                </div>
                <!-- 改修ポイント：ボタンラベルと色の変更 -->
                <div class="grid grid-cols-2 gap-3">
                    <a href="https://www.jma.go.jp/bosai/nowc/#lat:35.44&lon:139.59&zoom:12" target="_blank" 
                       class="bg-[#111] hover:bg-[#0055ff] text-center py-4 text-[11px] font-black transition border border-[#333] border-t-2 border-t-[#0055ff]">
                       雨雲の動き
                    </a>
                    <a href="https://www.jma.go.jp/bosai/risk/#lat:35.44&lon:139.59&zoom:12" target="_blank" 
                       class="bg-[#111] hover:bg-[#8000ff] text-center py-4 text-[11px] font-black transition border border-[#333] border-t-2 border-t-[#8000ff]">
                       キキクル
                    </a>
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
            } catch (e) {
                document.getElementById('overview-display').innerText = "データ取得エラー";
            }
        }

        async function loadNerv() {
            try {
                const res = await fetch(`https://api.rss2json.com/v1/api.json?rss_url=${encodeURIComponent("https://unnerv.jp/@UN_NERV.rss")}`);
                const data = await res.json();
                document.getElementById('nerv-display').innerHTML = data.items.slice(0, 15).map(item => `
                    <li class="border-b border-[#222] pb-3">
                        <div class="text-[#bbb] mb-1 font-bold leading-relaxed">${item.description.replace(/<[^>]+>/g, '')}</div>
                        <div class="text-[8px] mono text-[#444] tracking-widest uppercase">${new Date(item.pubDate).toLocaleString()}</div>
                    </li>
                `).join('');
            } catch (e) {
                document.getElementById('nerv-display').innerHTML = "<li>ニュース取得エラー</li>";
            }
        }

        loadOverview(); loadNerv();
        setInterval(loadOverview, 600000);
        setInterval(loadNerv, 300000);
    </script>
</body>
</html>
