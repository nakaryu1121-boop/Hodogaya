<html lang="ja">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>HODOGAYA AREA DEFENSE - Ver. 10.0</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <link href="https://fonts.googleapis.com/css2?family=Noto+Sans+JP:wght@700;900&family=Share+Tech+Mono&display=swap" rel="stylesheet">
    <style>
        body { font-family: 'Noto Sans JP', sans-serif; background-color: #050505; color: #eee; overflow-x: hidden;
            background-image: linear-gradient(#111 1px, transparent 1px), linear-gradient(90deg, #111 1px, transparent 1px);
            background-size: 30px 30px;
        }
        .mono { font-family: 'Share Tech Mono', monospace; }
        .nerv-panel { background: rgba(15, 15, 15, 0.95); border: 1px solid #333; border-left: 4px solid #E60012; transition: all 0.5s ease; }
        
        /* 🚨 アラート（遅延発生時） */
        .line-alert { 
            border: 2px solid #ff0000 !important; 
            border-left-width: 10px !important;
            background: linear-gradient(90deg, rgba(80,0,0,0.9) 0%, rgba(15,15,15,0.95) 100%) !important;
            animation: alert-flash 1s infinite alternate;
        }
        @keyframes alert-flash {
            from { box-shadow: 0 0 10px rgba(255,0,0,0.3); }
            to { box-shadow: 0 0 30px rgba(255,0,0,0.7); }
        }

        .symbol { display: inline-flex; align-items: center; justify-content: center; width: 30px; height: 30px; border: 2px solid #fff; border-radius: 4px; font-size: 13px; font-weight: 900; margin-right: 8px; flex-shrink: 0; }
        
        /* 路線別カラー */
        .symbol-jo { background-color: #0072bc; } /* 横須賀線 */
        .symbol-js { background-color: #e21f26; } /* 湘南新宿 */
        .symbol-so { background-color: #003f8e; } /* 相鉄線 */
        .symbol-kk { background-color: #da041a; } /* 京急線 */

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
            <div class="flex items-center gap-2">
                <span class="w-2 h-2 bg-[#00ff41] rounded-full status-pulse"></span>
                SYSTEM STATUS: <span id="master-status" class="text-[#eee]">NORMAL</span>
            </div>
            <div class="flex gap-4">
                <span id="sync-status" class="mono">TRANSIT_SYNC: WAIT</span>
                <span>NODE: HODOGAYA_04_LINE</span>
            </div>
        </div>

        <header class="flex flex-col md:flex-row justify-between items-end gap-2">
            <h1 class="text-4xl font-black text-[#E60012] italic tracking-tighter leading-none">HODOGAYA <span class="text-white not-italic">AREA DEFENSE</span></h1>
            <div id="clock" class="text-5xl font-black mono text-white leading-none tracking-tighter">00:00:00</div>
        </header>

        <div class="grid grid-cols-1 lg:grid-cols-12 gap-6">
            <div class="lg:col-span-7 space-y-6">
                <!-- TRANSPORTATION UNIT (4路線版) -->
                <div class="nerv-panel p-5">
                    <h2 class="text-[10px] font-black bg-[#E60012] px-2 py-0.5 text-white tracking-widest uppercase mb-4 w-fit">Transportation Management</h2>
                    <div class="grid grid-cols-1 sm:grid-cols-2 gap-3">
                        <!-- 横須賀線 -->
                        <a href="https://transit.yahoo.co.jp/diainfo/29/0" target="_blank" id="panel-yokosuka" class="bg-[#000] border border-[#222] p-3 flex items-center hover:bg-[#1a1a1a] transition">
                            <div class="symbol symbol-jo text-white">JO</div>
                            <div class="flex-grow min-w-0">
                                <p class="text-[7px] text-[#666] font-black uppercase truncate">Yokosuka Line</p>
                                <span class="text-xs font-black truncate block">横須賀線</span>
                                <p id="status-yokosuka" class="text-[8px] font-bold mt-0.5 text-[#00ff41]">NORMAL</p>
                            </div>
                        </a>
                        <!-- 湘南新宿ライン -->
                        <a href="https://transit.yahoo.co.jp/diainfo/25/0" target="_blank" id="panel-ssline" class="bg-[#000] border border-[#222] p-3 flex items-center hover:bg-[#1a1a1a] transition">
                            <div class="symbol symbol-js text-white">JS</div>
                            <div class="flex-grow min-w-0">
                                <p class="text-[7px] text-[#666] font-black uppercase truncate">S-S Line</p>
                                <span class="text-xs font-black truncate block">湘南新宿ライン</span>
                                <p id="status-ssline" class="text-[8px] font-bold mt-0.5 text-[#00ff41]">NORMAL</p>
                            </div>
                        </a>
                        <!-- 相鉄線 -->
                        <a href="https://transit.yahoo.co.jp/diainfo/125/0" target="_blank" id="panel-sotetsu" class="bg-[#000] border border-[#222] p-3 flex items-center hover:bg-[#1a1a1a] transition">
                            <div class="symbol symbol-so text-white">SO</div>
                            <div class="flex-grow min-w-0">
                                <p class="text-[7px] text-[#666] font-black uppercase truncate">Sotetsu Line</p>
                                <span class="text-xs font-black truncate block">相模鉄道 本線</span>
                                <p id="status-sotetsu" class="text-[8px] font-bold mt-0.5 text-[#00ff41]">NORMAL</p>
                            </div>
                        </a>
                        <!-- 京急線 -->
                        <a href="https://transit.yahoo.co.jp/diainfo/120/0" target="_blank" id="panel-keikyu" class="bg-[#000] border border-[#222] p-3 flex items-center hover:bg-[#1a1a1a] transition">
                            <div class="symbol symbol-kk text-white">KK</div>
                            <div class="flex-grow min-w-0">
                                <p class="text-[7px] text-[#666] font-black uppercase truncate">Keikyu Line</p>
                                <span class="text-xs font-black truncate block">京急本線</span>
                                <p id="status-keikyu" class="text-[8px] font-bold mt-0.5 text-[#00ff41]">NORMAL</p>
                            </div>
                        </a>
                    </div>
                </div>

                <!-- OVERVIEW -->
                <div class="nerv-panel p-5 border-l-[#444]">
                    <div class="flex justify-between items-center mb-3">
                        <h2 class="text-[10px] font-black bg-[#333] px-2 py-1 text-white w-fit tracking-widest uppercase">Overview</h2>
                        <a href="https://www.jma.go.jp/bosai/warning/#area_code=140000&lang=ja&area_type=offices" target="_blank" class="text-[9px] text-[#E60012] font-black underline italic">JMA_OFFICIAL ≫</a>
                    </div>
                    <div id="overview-display" class="text-[13px] leading-relaxed text-[#bbb] bg-[#080808] p-4 border border-[#222] min-h-[100px]">Synchronizing...</div>
                </div>
            </div>

            <!-- NERV FEED -->
            <div class="lg:col-span-5 space-y-6">
                <div class="nerv-panel p-5 h-[400px] flex flex-col border-l-[#333]">
                    <h2 class="text-[10px] font-black bg-[#333] px-2 py-1 text-white mb-3 w-fit tracking-widest uppercase">NERV Feed</h2>
                    <ul id="nerv-display" class="overflow-y-auto text-[11px] space-y-3 text-[#888] pr-2 flex-grow"></ul>
                </div>
                <div class="grid grid-cols-2 gap-3">
                    <a href="https://www.jma.go.jp/bosai/nowc/#lat:35.44&lon:139.59&zoom:12" target="_blank" class="bg-[#111] hover:bg-[#E60012] text-center py-4 text-[10px] font-black transition border border-[#333] border-t-2 border-t-[#E60012]">RAIN RADAR</a>
                    <a href="https://www.jma.go.jp/bosai/risk/#lat:35.44&lon:139.59&zoom:12" target="_blank" class="bg-[#111] hover:bg-[#ffdb00] text-center py-4 text-[10px] font-black transition border border-[#333] border-t-2 border-t-[#ffdb00]">KIKIKURU</a>
                </div>
            </div>
        </div>
        
        <div class="pt-6 text-right">
            <button onclick="toggleTestAlert()" class="text-[8px] text-[#222] hover:text-[#E60012] font-bold mono uppercase">DEBUG: simulate_emergency_alert</button>
        </div>
    </div>

    <script>
        setInterval(() => {
            document.getElementById('clock').innerText = new Date().toLocaleTimeString('ja-JP', { hour12: false });
        }, 1000);

        async function checkTrainStatus() {
            const syncLabel = document.getElementById('sync-status');
            try {
                const targetUrl = "https://tetsudo.rti-giken.jp/free/tetsudo_now/api/delay.json";
                const res = await fetch(`https://api.allorigins.win/raw?url=${encodeURIComponent(targetUrl)}`);
                const delays = await res.json();
                
                // 4路線の判定
                const status = {
                    yokosuka: delays.some(d => d.name.includes("横須賀線")),
                    ssline: delays.some(d => d.name.includes("湘南新宿ライン")),
                    sotetsu: delays.some(d => d.name.includes("相鉄")),
                    keikyu: delays.some(d => d.name.includes("京急"))
                };

                for (let key in status) { updatePanel(key, status[key]); }
                
                syncLabel.innerText = "TRANSIT_SYNC: OK";
                syncLabel.style.color = "#00ff41";
            } catch (e) {
                syncLabel.innerText = "TRANSIT_SYNC: ERROR";
                syncLabel.style.color = "#ff4444";
            }
        }

        function updatePanel(id, isAlert) {
            const panel = document.getElementById(`panel-${id}`);
            const statusText = document.getElementById(`status-${id}`);
            const master = document.getElementById('master-status');

            if (isAlert) {
                panel.classList.add('line-alert');
                statusText.innerText = "🚨 DELAY_DETECTED";
                statusText.style.color = "#ff4444";
                master.innerText = "EMERGENCY";
                master.style.color = "#ff4444";
            } else {
                panel.classList.remove('line-alert');
                statusText.innerText = "NORMAL";
                statusText.style.color = "#00ff41";
            }
        }

        async function loadOverview() {
            try {
                const res = await fetch("https://www.jma.go.jp/bosai/forecast/data/overview_forecast/140000.json");
                const data = await res.json();
                document.getElementById('overview-display').innerText = data.text;
            } catch (e) {}
        }

        async function loadNerv() {
            try {
                const res = await fetch(`https://api.rss2json.com/v1/api.json?rss_url=${encodeURIComponent("https://unnerv.jp/@UN_NERV.rss")}`);
                const data = await res.json();
                document.getElementById('nerv-display').innerHTML = data.items.slice(0, 10).map(item => `
                    <li class="border-b border-[#222] pb-3">
                        <div class="text-[#bbb] mb-1 font-bold leading-relaxed">${item.description.replace(/<[^>]+>/g, '')}</div>
                        <div class="text-[8px] mono text-[#444] tracking-widest">${new Date(item.pubDate).toLocaleString()}</div>
                    </li>
                `).join('');
            } catch (e) {}
        }

        let isTestAlert = false;
        function toggleTestAlert() {
            isTestAlert = !isTestAlert;
            ["yokosuka", "ssline", "sotetsu", "keikyu"].forEach(id => updatePanel(id, isTestAlert));
        }

        checkTrainStatus();
        loadOverview();
        loadNerv();
        setInterval(checkTrainStatus, 60000);
        setInterval(loadOverview, 600000);
        setInterval(loadNerv, 300000);
    </script>
</body>
</html>
