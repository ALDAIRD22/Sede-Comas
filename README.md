<!DOCTYPE html>
<html lang="es" class="scroll-smooth">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Dashboard Rotativo - Olimpiadas Vonex 2026</title>
    <!-- Tailwind CSS -->
    <script src="https://cdn.tailwindcss.com"></script>
    <!-- Chart.js -->
    <script src="https://cdn.jsdelivr.net/npm/chart.js"></script>
    <!-- Google Fonts -->
    <link href="https://fonts.googleapis.com/css2?family=Plus+Jakarta+Sans:wght@400;500;600;700;800&display=swap" rel="stylesheet">
    <style>
        body { 
            font-family: 'Plus Jakarta Sans', sans-serif; 
            background: radial-gradient(circle at top right, #1e1b4b 0%, #0f172a 60%, #020617 100%);
        }
        .premium-card {
            background: rgba(15, 23, 42, 0.45);
            border: 1px solid rgba(255, 255, 255, 0.04);
            backdrop-filter: blur(16px);
        }
        .nav-card {
            transition: all 0.3s ease;
        }
        .nav-card:hover {
            transform: translateY(-3px);
            background: rgba(255, 255, 255, 0.05);
            border-color: rgba(99, 102, 241, 0.4);
            box-shadow: 0 10px 20px -10px rgba(99, 102, 241, 0.3);
        }
        .fade-transition {
            transition: opacity 0.5s ease-in-out, transform 0.5s ease-in-out;
        }
        /* Margen superior para que el menú pegajoso no tape los títulos al hacer clic */
        .scroll-mt-24 {
            scroll-margin-top: 6rem;
        }
    </style>
</head>
<body class="text-slate-100 min-h-screen antialiased">

    <!-- Encabezado Pegajoso -->
    <header class="border-b border-slate-800/80 bg-slate-950/80 backdrop-blur-xl sticky top-0 z-50">
        <div class="max-w-7xl mx-auto px-4 sm:px-6 lg:px-8 h-16 flex items-center justify-between">
            <div class="flex items-center space-x-3.5">
                <div class="bg-gradient-to-tr from-indigo-600 to-violet-500 p-2.5 rounded-xl text-white font-extrabold text-xl tracking-wider shadow-lg shadow-indigo-500/20">V</div>
                <div>
                    <h1 class="text-base font-extrabold text-white tracking-tight sm:text-lg">OLIMPIADAS VONEX 2026</h1>
                    <p class="text-xs text-slate-400">Dashboard de Pagos Automatizado</p>
                </div>
            </div>
            <div class="flex items-center space-x-2.5">
                <span class="flex h-2.5 w-2.5 relative">
                    <span class="animate-ping absolute inline-flex h-full w-full rounded-full bg-emerald-400 opacity-75"></span>
                    <span class="relative inline-flex rounded-full h-2.5 w-2.5 bg-emerald-500 shadow-[0_0_10px_#10b981]"></span>
                </span>
                <span class="text-xs font-bold text-emerald-400 bg-emerald-500/10 px-3 py-1 rounded-full border border-emerald-500/20 tracking-wider uppercase hidden sm:inline-block">Conectado en Vivo</span>
            </div>
        </div>
    </header>

    <main class="max-w-7xl mx-auto px-4 sm:px-6 lg:px-8 py-8 space-y-8">
        
        <!-- MENÚ DE NAVEGACIÓN INTERACTIVO -->
        <nav class="grid grid-cols-2 md:grid-cols-4 gap-4 mb-8">
            <a href="#resumen" class="premium-card nav-card rounded-2xl p-4 flex flex-col items-center justify-center cursor-pointer group">
                <span class="text-3xl mb-2 group-hover:scale-110 transition-transform">📊</span>
                <span class="text-sm font-bold text-slate-300 group-hover:text-white tracking-wide">Resumen</span>
            </a>
            <a href="#clasificacion" class="premium-card nav-card rounded-2xl p-4 flex flex-col items-center justify-center cursor-pointer group">
                <span class="text-3xl mb-2 group-hover:scale-110 transition-transform">🏆</span>
                <span class="text-sm font-bold text-slate-300 group-hover:text-white tracking-wide">Clasificación</span>
            </a>
            <a href="#pagos" class="premium-card nav-card rounded-2xl p-4 flex flex-col items-center justify-center cursor-pointer group">
                <span class="text-3xl mb-2 group-hover:scale-110 transition-transform">💰</span>
                <span class="text-sm font-bold text-slate-300 group-hover:text-white tracking-wide">Pagos</span>
            </a>
            <a href="#tutores" class="premium-card nav-card rounded-2xl p-4 flex flex-col items-center justify-center cursor-pointer group">
                <span class="text-3xl mb-2 group-hover:scale-110 transition-transform">👩‍🏫</span>
                <span class="text-sm font-bold text-slate-300 group-hover:text-white tracking-wide">Tutores</span>
            </a>
        </nav>

        <!-- SECCIÓN: RESUMEN -->
        <div id="resumen" class="space-y-8 scroll-mt-24">
            <!-- Tarjetas de KPIs -->
            <section class="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-4 gap-6">
                <div class="premium-card rounded-2xl p-6 flex flex-col justify-between shadow-xl relative overflow-hidden">
                    <div class="absolute top-0 left-0 w-full h-[2px] bg-gradient-to-r from-indigo-500 to-violet-500"></div>
                    <p class="text-xs font-bold text-slate-400 uppercase tracking-wider">Avance General</p>
                    <h3 class="text-4xl font-extrabold text-transparent bg-clip-text bg-gradient-to-r from-indigo-400 to-violet-400 mt-2" id="txt-avance-global">...</h3>
                    <div class="w-full bg-slate-800 rounded-full h-2 mt-5 overflow-hidden">
                        <div id="bar-avance-global" class="bg-gradient-to-r from-indigo-500 to-violet-500 h-full rounded-full transition-all duration-700" style="width: 0%"></div>
                    </div>
                </div>
                <div class="premium-card rounded-2xl p-6 shadow-xl relative overflow-hidden">
                    <div class="absolute top-0 left-0 w-full h-[2px] bg-gradient-to-r from-emerald-500 to-teal-500"></div>
                    <p class="text-xs font-bold text-slate-400 uppercase tracking-wider">Total Recaudado</p>
                    <h3 class="text-3xl font-extrabold text-emerald-400 mt-2" id="txt-recaudado-global">...</h3>
                    <p class="text-xs text-slate-500 font-semibold mt-2.5 border-t border-slate-800/60 pt-2" id="txt-meta-global">Meta Total: ...</p>
                </div>
                <div class="premium-card rounded-2xl p-6 shadow-xl relative overflow-hidden">
                    <div class="absolute top-0 left-0 w-full h-[2px] bg-gradient-to-r from-sky-500 to-blue-500"></div>
                    <p class="text-xs font-bold text-slate-400 uppercase tracking-wider">Estudiantes Pagantes</p>
                    <h3 class="text-3xl font-extrabold text-sky-400 mt-2" id="txt-pagantes-global">...</h3>
                    <p class="text-xs text-slate-500 font-semibold mt-2.5 border-t border-slate-800/60 pt-2" id="txt-meta-alumnos">Meta Alumnos: ...</p>
                </div>
                <div class="premium-card rounded-2xl p-6 shadow-xl relative overflow-hidden">
                    <div class="absolute top-0 left-0 w-full h-[2px] bg-gradient-to-r from-rose-500 to-red-500"></div>
                    <p class="text-xs font-bold text-slate-400 uppercase tracking-wider">Monto Pendiente (Falta)</p>
                    <h3 class="text-3xl font-extrabold text-rose-400 mt-2" id="txt-falta-global">...</h3>
                    <p class="text-xs text-slate-500 font-semibold mt-2.5 border-t border-slate-800/60 pt-2">Por recaudar</p>
                </div>
            </section>

            <!-- Carrusel Rotativo -->
            <section class="premium-card rounded-2xl p-6 shadow-2xl bg-gradient-to-b from-slate-950/40 to-transparent relative overflow-hidden">
                <div class="absolute top-3 right-5 flex items-center space-x-1.5">
                    <span class="h-2 w-2 rounded-full bg-indigo-500 animate-pulse"></span>
                    <span class="text-[10px] font-bold text-indigo-400 uppercase tracking-widest">Rotación Activa</span>
                </div>
                <h3 class="text-base font-bold text-white mb-5 flex items-center gap-2 tracking-tight">
                    <span>🔄 Monitoreo Individual en Tiempo Real</span>
                </h3>
                
                <div id="rotation-card" class="fade-transition opacity-100 transform scale-100 grid grid-cols-1 md:grid-cols-3 gap-6 items-center min-h-[140px]">
                    <div class="md:col-span-1 space-y-2">
                        <span id="rot-ciclo" class="bg-indigo-500/10 text-indigo-400 border border-indigo-500/20 text-[10px] font-extrabold uppercase px-2.5 py-1 rounded-md tracking-wider">...</span>
                        <h2 id="rot-tutor" class="text-xl font-extrabold text-white tracking-tight mt-2">Cargando Tutor...</h2>
                    </div>
                    <div class="md:col-span-2 grid grid-cols-2 sm:grid-cols-4 gap-4 text-center md:border-l border-slate-800/80 md:pl-6">
                        <div class="bg-slate-900/30 p-3 rounded-xl border border-slate-800/40">
                            <p class="text-[10px] font-bold text-slate-400 uppercase tracking-wider">Meta Dinero</p>
                            <p id="rot-meta" class="text-sm font-extrabold text-slate-200 mt-1">...</p>
                        </div>
                        <div class="bg-slate-900/30 p-3 rounded-xl border border-slate-800/40">
                            <p class="text-[10px] font-bold text-slate-400 uppercase tracking-wider">Efectivo</p>
                            <p id="rot-efectivo" class="text-sm font-extrabold text-slate-300 mt-1">...</p>
                        </div>
                        <div class="bg-slate-900/30 p-3 rounded-xl border border-slate-800/40">
                            <p class="text-[10px] font-bold text-slate-400 uppercase tracking-wider">Yape</p>
                            <p id="rot-yape" class="text-sm font-extrabold text-slate-300 mt-1">...</p>
                        </div>
                        <div class="bg-slate-900/30 p-3 rounded-xl border border-slate-800/40">
                            <p class="text-[10px] font-bold text-slate-400 uppercase tracking-wider">Avance</p>
                            <p id="rot-avance" class="text-sm font-black text-emerald-400 mt-1">...</p>
                        </div>
                    </div>
                </div>
            </section>
        </div>

        <!-- Gráfico, Tabla y Ranking -->
        <div class="grid grid-cols-1 lg:grid-cols-3 gap-8">
            <div class="lg:col-span-2 space-y-8">
                
                <!-- SECCIÓN: PAGOS (Gráfico) -->
                <section id="pagos" class="premium-card rounded-2xl p-6 shadow-xl scroll-mt-24">
                    <h3 class="text-lg font-semibold text-white mb-4 flex items-center gap-2">
                        💰 Comparativa de Recaudación (S/)
                    </h3>
                    <div class="relative h-80"><canvas id="chartTutors"></canvas></div>
                </section>

                <!-- SECCIÓN: TUTORES (Tabla) -->
                <section id="tutores" class="premium-card rounded-2xl overflow-hidden shadow-2xl scroll-mt-24">
                    <div class="p-5 border-b border-slate-800/80 bg-slate-950/20">
                        <h3 class="text-base font-bold text-white tracking-tight">👩‍🏫 Detalle por Tutor</h3>
                    </div>
                    <div class="overflow-x-auto">
                        <table class="w-full text-left border-collapse">
                            <thead>
                                <tr class="bg-slate-950/50 text-slate-400 text-[11px] font-bold uppercase tracking-wider border-b border-slate-800">
                                    <th class="py-4 px-5">Tutor</th>
                                    <th class="py-4 px-4">Ciclo</th>
                                    <th class="py-4 px-4 text-center">Meta Est.</th>
                                    <th class="py-4 px-4 text-center">Pagantes</th>
                                    <th class="py-4 px-4 text-right">Meta (S/)</th>
                                    <th class="py-4 px-4 text-right text-slate-400">Efectivo</th>
                                    <th class="py-4 px-4 text-right text-slate-400">Yape</th>
                                    <th class="py-4 px-5 text-right text-emerald-400">Recaudado</th>
                                    <th class="py-4 px-5 text-center">% Avance</th>
                                </tr>
                            </thead>
                            <tbody class="divide-y divide-slate-800/40 text-sm" id="table-body-tutors"></tbody>
                        </table>
                    </div>
                </section>
            </div>

            <!-- SECCIÓN: CLASIFICACIÓN (Ranking) -->
            <div class="lg:col-span-1 space-y-8">
                <section id="clasificacion" class="premium-card rounded-2xl p-6 shadow-xl flex flex-col h-full scroll-mt-24">
                    <div class="mb-5 border-b border-slate-800/80 pb-4">
                        <h3 class="text-base font-bold text-white flex items-center gap-2 tracking-tight">
                            🏆 Tabla de Posiciones
                        </h3>
                    </div>
                    <div class="space-y-3.5 flex-1" id="leaderboard-container"></div>
                </section>
            </div>
        </div>
    </main>

    <script>
        const SHEET_JSON_URL = 'https://docs.google.com/spreadsheets/d/1z2qJzZMr5c_lTLUDU2uXEZpUJ8JhL-tq8COC5GXKcVQ/gviz/tq?tqx=out:json&gid=6209676';

        let myChart = null;
        let tutorsGlobalArray = [];
        let currentRotateIndex = 0;

        function cleanNum(val) {
            if (!val) return 0;
            let clean = val.toString().replace(/[S\/%,\s]/g, '').trim();
            return parseFloat(clean) || 0;
        }

        function getVal(cell, isNum = false) {
            if (!cell) return isNum ? 0 : '';
            if (cell.v === null || cell.v === undefined) return isNum ? 0 : '';
            if (isNum) {
                let val = cell.v;
                if (typeof val === 'string') {
                    val = val.replace(/[S\/%,\s]/g, '').trim();
                    return parseFloat(val) || 0;
                }
                return parseFloat(val) || 0;
            }
            return cell.f || cell.v.toString();
        }

        async function loadDashboardData() {
            try {
                const response = await fetch(SHEET_JSON_URL);
                const text = await response.text();
                
                const startIdx = text.indexOf('{');
                const endIdx = text.lastIndexOf('}');
                const jsonString = text.substring(startIdx, endIdx + 1);
                const data = JSON.parse(jsonString);
                const rows = data.table.rows;

                let totalsIndex = rows.findIndex(r => r && r.c && r.c[0] && getVal(r.c[0]).trim().toUpperCase() === 'TOTALES');
                if (totalsIndex === -1) totalsIndex = rows.length - 2;

                let tRow = rows[totalsIndex + 1];
                if (!tRow && totalsIndex !== -1) tRow = rows[totalsIndex];

                if(tRow && tRow.c) {
                    document.getElementById('txt-meta-alumnos').innerText = `Meta Alumnos: ${getVal(tRow.c[2], true)}`;
                    document.getElementById('txt-pagantes-global').innerText = getVal(tRow.c[3], true).toLocaleString('es-PE');
                    document.getElementById('txt-meta-global').innerText = `Meta Total: S/ ${getVal(tRow.c[4], true).toLocaleString('es-PE')}`;
                    document.getElementById('txt-recaudado-global').innerText = `S/ ${getVal(tRow.c[7], true).toLocaleString('es-PE')}`;
                    document.getElementById('txt-falta-global').innerText = `S/ ${getVal(tRow.c[8], true).toLocaleString('es-PE')}`;
                    
                    let avanceStr = getVal(tRow.c[9]);
                    let avanceGlobalNum = 0;
                    if (avanceStr.includes('%')) {
                        avanceGlobalNum = parseInt(avanceStr);
                    } else {
                        let n = getVal(tRow.c[9], true);
                        avanceGlobalNum = n <= 1.5 ? Math.round(n * 100) : Math.round(n);
                    }
                    
                    document.getElementById('txt-avance-global').innerText = avanceGlobalNum + '%';
                    document.getElementById('bar-avance-global').style.width = avanceGlobalNum + '%';
                }

                tutorsGlobalArray = [];
                for (let i = 0; i < totalsIndex; i++) {
                    const row = rows[i];
                    if (!row || !row.c || !row.c[0]) continue;

                    let tutorName = getVal(row.c[0]).trim();
                    if (tutorName === '' || tutorName.toUpperCase() === 'TUTOR' || tutorName.toUpperCase() === 'TOTALES') continue;

                    let avanceStr = getVal(row.c[9]);
                    let avanceNum = 0;
                    if (avanceStr.includes('%')) {
                        avanceNum = parseInt(avanceStr);
                    } else {
                        let n = getVal(row.c[9], true);
                        avanceNum = n <= 1.5 ? Math.round(n * 100) : Math.round(n);
                    }

                    tutorsGlobalArray.push({
                        tutor: tutorName,
                        ciclo: getVal(row.c[1]).trim(),
                        metaEst: getVal(row.c[2], true),
                        pagantes: getVal(row.c[3], true),
                        metaDinero: getVal(row.c[4], true),
                        efectivo: getVal(row.c[5], true),
                        yape: getVal(row.c[6], true),
                        recaudado: getVal(row.c[7], true),
                        falta: getVal(row.c[8], true),
                        avance: avanceNum
                    });
                }

                renderTable(tutorsGlobalArray);
                renderChart(tutorsGlobalArray);
                
                const rankedData = [...tutorsGlobalArray].sort((a, b) => b.avance - a.avance);
                renderLeaderboard(rankedData);

            } catch (error) {
                console.error(error);
            }
        }

        function rotateTutorOneByOne() {
            if (tutorsGlobalArray.length === 0) return;
            
            const cardEl = document.getElementById('rotation-card');
            cardEl.style.opacity = 0;
            cardEl.style.transform = 'scale(0.98)';
            
            setTimeout(() => {
                const currentTutor = tutorsGlobalArray[currentRotateIndex];
                
                document.getElementById('rot-ciclo').innerText = currentTutor.ciclo;
                document.getElementById('rot-tutor').innerText = currentTutor.tutor;
                document.getElementById('rot-meta').innerText = `S/ ${currentTutor.metaDinero.toLocaleString('es-PE')}`;
                document.getElementById('rot-efectivo').innerText = `S/ ${currentTutor.efectivo.toLocaleString('es-PE')}`;
                document.getElementById('rot-yape').innerText = `S/ ${currentTutor.yape.toLocaleString('es-PE')}`;
                document.getElementById('rot-avance').innerText = `${currentTutor.avance}%`;
                
                cardEl.style.opacity = 1;
                cardEl.style.transform = 'scale(1)';
                
                currentRotateIndex = (currentRotateIndex + 1) % tutorsGlobalArray.length;
            }, 500);
        }

        function renderTable(data) {
            const tbody = document.getElementById('table-body-tutors');
            tbody.innerHTML = '';
            data.forEach(row => {
                const tr = document.createElement('tr');
                tr.className = "hover:bg-slate-800/40 transition-all border-b border-slate-800/30 text-slate-300";
                tr.innerHTML = `
                    <td class="py-4 px-5 font-semibold text-slate-100 whitespace-nowrap">
                        <div class="flex items-center space-x-2.5">
                            <div class="h-2 w-2 rounded-full ${row.avance >= 100 ? 'bg-emerald-400 shadow-[0_0_8px_rgba(52,211,153,0.6)]' : 'bg-indigo-400'}"></div>
                            <span>${row.tutor}</span>
                        </div>
                    </td>
                    <td class="py-4 px-4 whitespace-nowrap">
                        <span class="bg-slate-800/80 text-slate-300 text-[11px] font-bold px-2.5 py-0.5 rounded-md border border-slate-700/50 tracking-wide">${row.ciclo}</span>
                    </td>
                    <td class="py-4 px-4 text-center font-semibold text-slate-300">${row.metaEst}</td>
                    <td class="py-4 px-4 text-center font-semibold text-slate-300">${row.pagantes}</td>
                    <td class="py-4 px-4 text-right font-medium text-slate-400 whitespace-nowrap">S/ ${row.metaDinero.toLocaleString('es-PE', {minimumFractionDigits: 2, maximumFractionDigits: 2})}</td>
                    <td class="py-4 px-4 text-right font-medium text-slate-400/80 whitespace-nowrap">S/ ${row.efectivo.toLocaleString('es-PE', {minimumFractionDigits: 2, maximumFractionDigits: 2})}</td>
                    <td class="py-4 px-4 text-right font-medium text-slate-400/80 whitespace-nowrap">S/ ${row.yape.toLocaleString('es-PE', {minimumFractionDigits: 2, maximumFractionDigits: 2})}</td>
                    <td class="py-4 px-5 text-right font-bold text-emerald-400 whitespace-nowrap">S/ ${row.recaudado.toLocaleString('es-PE', {minimumFractionDigits: 2, maximumFractionDigits: 2})}</td>
                    <td class="py-4 px-5 text-center">
                        <span class="px-2.5 py-1 rounded-full text-xs font-bold border ${row.avance >= 100 ? 'bg-emerald-500/10 text-emerald-400 border-emerald-500/30' : 'bg-indigo-500/10 text-indigo-400 border-indigo-500/30'}">${row.avance}%</span>
                    </td>
                `;
                tbody.appendChild(tr);
            });
        }

        function renderLeaderboard(data) {
            const container = document.getElementById('leaderboard-container');
            container.innerHTML = '';
            data.forEach((row, index) => {
                const item = document.createElement('div');
                item.className = `flex items-center justify-between p-4 rounded-xl border ${
                    index === 0 ? 'bg-amber-500/10 border-amber-500/30' : 
                    index === 1 ? 'bg-slate-300/10 border-slate-400/30' : 
                    index === 2 ? 'bg-amber-700/10 border-amber-700/20' : 'bg-slate-800/30 border-slate-800/60'
                }`;
                
                let medal = `<span class="text-sm font-bold text-slate-400 w-6">${index + 1}</span>`;
                if (index === 0) medal = `<span class="text-xl w-6">🥇</span>`;
                if (index === 1) medal = `<span class="text-xl w-6">🥈</span>`;
                if (index === 2) medal = `<span class="text-xl w-6">🥉</span>`;

                item.innerHTML = `
                    <div class="flex items-center space-x-3 truncate">
                        ${medal}
                        <div class="truncate">
                            <p class="text-sm font-semibold text-slate-200 truncate">${row.tutor}</p>
                            <p class="text-xs text-slate-400 truncate">${row.ciclo}</p>
                        </div>
                    </div>
                    <div class="text-right ml-2 flex-shrink-0">
                        <p class="text-sm font-bold text-white">${row.avance}%</p>
                        <p class="text-[10px] text-slate-400">S/ ${row.recaudado.toLocaleString('es-PE')}</p>
                    </div>
                `;
                container.appendChild(item);
            });
        }

        function renderChart(data) {
            const ctx = document.getElementById('chartTutors').getContext('2d');
            if (myChart) { myChart.destroy(); }
            
            myChart = new Chart(ctx, {
                type: 'bar',
                data: {
                    labels: data.map(r => r.tutor.split(' ')[0] + ' ' + (r.tutor.split(' ')[1] || '')),
                    datasets: [
                        { label: 'Meta Fija (S/.)', data: data.map(r => r.metaDinero), backgroundColor: 'rgba(71, 85, 105, 0.4)', borderRadius: 4 },
                        { label: 'Recaudado (S/.)', data: data.map(r => r.recaudado), backgroundColor: 'rgba(99, 102, 241, 0.8)', borderRadius: 4 }
                    ]
                },
                options: {
                    indexAxis: 'y',
                    responsive: true,
                    maintainAspectRatio: false,
                    plugins: { legend: { labels: { color: '#94a3b8' } } },
                    scales: {
                        x: { grid: { color: 'rgba(51, 65, 85, 0.2)' }, ticks: { color: '#94a3b8' } },
                        y: { ticks: { color: '#e2e8f0' } }
                    }
                }
            });
        }

        loadDashboardData().then(() => {
            setInterval(rotateTutorOneByOne, 4000);
            setTimeout(rotateTutorOneByOne, 800);
        });
        
        setInterval(loadDashboardData, 60000);
    </script>
</body>
</html>
