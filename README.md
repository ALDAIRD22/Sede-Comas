<!DOCTYPE html>
<html lang="es">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Dashboard - Olimpiadas Vonex 2026</title>
    <!-- Tailwind CSS -->
    <script src="https://cdn.tailwindcss.com"></script>
    <!-- Chart.js -->
    <script src="https://cdn.jsdelivr.net/npm/chart.js"></script>
    <!-- Librería para mostrar números sobre las barras de Chart.js -->
    <script src="https://cdn.jsdelivr.net/npm/chartjs-plugin-datalabels@2.2.0"></script>
    <!-- Google Fonts -->
    <link href="https://fonts.googleapis.com/css2?family=Plus+Jakarta+Sans:wght@400;500;600;700;800&display=swap" rel="stylesheet">
    <style>
        body { 
            font-family: 'Plus Jakarta Sans', sans-serif; 
            background: radial-gradient(circle at top right, #1e1b4b 0%, #0f172a 60%, #020617 100%);
        }
        .premium-card {
            background: rgba(22, 30, 49, 0.4);
            border: 1px solid rgba(255, 255, 255, 0.05);
            backdrop-filter: blur(12px);
        }
        .nav-card {
            transition: all 0.25s ease;
        }
    </style>
</head>
<body class="text-slate-100 min-h-screen antialiased">

    <!-- PANTALLA DE BIENVENIDA (SPLASH SCREEN) -->
    <div id="welcome-overlay" class="fixed inset-0 z-[100] bg-slate-950/90 backdrop-blur-2xl flex items-center justify-center transition-opacity duration-700 opacity-100">
        <div class="text-center space-y-6 transform transition-all scale-100 animate-pulse" id="welcome-content">
            <div class="inline-flex items-center justify-center w-24 h-24 rounded-full bg-gradient-to-tr from-indigo-600 to-violet-500 shadow-[0_0_40px_rgba(99,102,241,0.4)] mb-2">
                <span class="text-5xl text-white font-black">V</span>
            </div>
            <h2 class="text-3xl font-extrabold text-white tracking-tight">Olimpiadas Vonex 2026</h2>
            
            <div class="flex flex-col items-center justify-center space-y-3 mt-6">
                <p id="welcome-loading" class="text-slate-400 font-medium tracking-widest uppercase text-sm">Calculando datos en vivo...</p>
                
                <!-- Aquí se inyectan los porcentajes mágicamente -->
                <div id="welcome-stats" class="hidden flex-col items-center space-y-2 mt-2">
                    <p class="text-6xl font-black text-emerald-400 drop-shadow-[0_0_15px_rgba(52,211,153,0.3)]" id="welcome-avance">...%</p>
                    <div class="bg-rose-500/10 border border-rose-500/20 px-4 py-1.5 rounded-full mt-2">
                        <p class="text-sm font-bold text-rose-400 tracking-wide" id="welcome-falta">Falta ...%</p>
                    </div>
                </div>
            </div>
        </div>
    </div>

    <!-- Encabezado -->
    <header class="border-b border-slate-800 bg-slate-950/40 backdrop-blur-xl sticky top-0 z-50">
        <div class="max-w-7xl mx-auto px-4 sm:px-6 lg:px-8 h-16 flex items-center justify-between">
            <div class="flex items-center space-x-3.5">
                <div class="bg-indigo-600 p-2.5 rounded-xl text-white font-extrabold text-xl tracking-wider">V</div>
                <div>
                    <h1 class="text-lg font-bold text-white tracking-tight">OLIMPIADAS VONEX 2026</h1>
                    <p class="text-xs text-slate-400">Control de pagos automatizado</p>
                </div>
            </div>
            <div class="flex items-center space-x-2.5">
                <span class="flex h-2 w-2 relative">
                    <span class="animate-ping absolute inline-flex h-full w-full rounded-full bg-emerald-400 opacity-75"></span>
                    <span class="relative inline-flex rounded-full h-2 w-2 bg-emerald-500"></span>
                </span>
                <span class="text-xs font-bold text-emerald-400 bg-emerald-500/10 px-3 py-1 rounded-full border border-emerald-500/20 uppercase tracking-wider">Conectado en Vivo</span>
            </div>
        </div>
    </header>

    <!-- Alerta de Sincronización -->
    <div id="error-box" class="hidden max-w-7xl mx-auto px-4 sm:px-6 lg:px-8 mt-6">
        <div class="bg-rose-500/10 border border-rose-500/20 text-rose-400 p-4 rounded-xl text-sm font-medium">
            ⚠️ Alerta de Sincronización: No se pueden leer los datos. Verifica que la hoja esté compartida como "Cualquier persona con el enlace".
        </div>
    </div>

    <main class="max-w-7xl mx-auto px-4 sm:px-6 lg:px-8 py-8 space-y-8">
        
        <!-- MENÚ DE NAVEGACIÓN POR TARJETAS -->
        <nav class="grid grid-cols-2 lg:grid-cols-4 gap-5">
            <button onclick="switchTab('view-resumen')" id="btn-view-resumen" class="nav-card premium-card text-left rounded-2xl p-5 border-indigo-500/40 bg-indigo-500/5 ring-1 ring-indigo-500/20 shadow-lg shadow-indigo-500/5">
                <div class="text-3xl">📊</div>
                <div class="text-sm font-bold text-white mt-3">Resumen</div>
                <div class="text-[11px] text-indigo-300 mt-1 font-medium">Gráficos y Distribución</div>
            </button>
            <button onclick="switchTab('view-clasificacion')" id="btn-view-clasificacion" class="nav-card premium-card text-left rounded-2xl p-5 hover:bg-slate-800/30 hover:border-slate-700/50">
                <div class="text-3xl">🏆</div>
                <div class="text-sm font-bold text-slate-300 mt-3">Clasificación</div>
                <div class="text-[11px] text-slate-500 mt-1 font-medium">Ranking de Posiciones</div>
            </button>
            <button onclick="switchTab('view-pagos')" id="btn-view-pagos" class="nav-card premium-card text-left rounded-2xl p-5 hover:bg-slate-800/30 hover:border-slate-700/50">
                <div class="text-3xl">💰</div>
                <div class="text-sm font-bold text-slate-300 mt-3">Pagos</div>
                <div class="text-[11px] text-slate-500 mt-1 font-medium">Efectivo vs Yape</div>
            </button>
            <button onclick="switchTab('view-tutores')" id="btn-view-tutores" class="nav-card premium-card text-left rounded-2xl p-5 hover:bg-slate-800/30 hover:border-slate-700/50">
                <div class="text-3xl">👩‍🏫</div>
                <div class="text-sm font-bold text-slate-300 mt-3">Tutores</div>
                <div class="text-[11px] text-slate-500 mt-1 font-medium">Tabla de Detalle General</div>
            </button>
        </nav>

        <!-- TARJETAS DE INDICADORES GLOBALES -->
        <section class="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-4 gap-5">
            <div class="premium-card rounded-2xl p-5 flex flex-col justify-between shadow-xl relative overflow-hidden">
                <p class="text-xs font-bold text-slate-400 uppercase tracking-wider">💰 Total recaudado</p>
                <h3 class="text-3xl font-extrabold text-emerald-400 mt-2 tracking-tight" id="txt-recaudado-global">...</h3>
            </div>
            <div class="premium-card rounded-2xl p-5 flex flex-col justify-between shadow-xl">
                <p class="text-xs font-bold text-slate-400 uppercase tracking-wider">🎯 Meta en efectivo</p>
                <h3 class="text-3xl font-extrabold text-slate-100 mt-2 tracking-tight" id="txt-meta-global">...</h3>
            </div>
            <div class="premium-card rounded-2xl p-5 flex flex-col justify-between shadow-xl">
                <p class="text-xs font-bold text-slate-400 uppercase tracking-wider">⚠️ Falta recaudar</p>
                <h3 class="text-3xl font-extrabold text-rose-400 mt-2 tracking-tight" id="txt-falta-global">...</h3>
            </div>
            <div class="premium-card rounded-2xl p-5 flex flex-col justify-between shadow-xl">
                <p class="text-xs font-bold text-slate-400 uppercase tracking-wider">📈 Avance general</p>
                <h3 class="text-3xl font-extrabold text-indigo-400 mt-2 tracking-tight" id="txt-avance-global">...</h3>
                <div class="w-full bg-slate-800 rounded-full h-1.5 mt-3 overflow-hidden">
                    <div id="bar-avance-global" class="bg-indigo-500 h-full rounded-full transition-all duration-500" style="width: 0%"></div>
                </div>
            </div>
        </section>

        <!-- VISTA 1: RESUMEN -->
        <div id="view-resumen" class="tab-view space-y-8">
            <div class="grid grid-cols-1 lg:grid-cols-3 gap-6">
                <div class="lg:col-span-2 premium-card rounded-2xl p-6 shadow-xl">
                    <h3 class="text-sm font-bold uppercase tracking-wider text-slate-400 mb-5">Recaudación por tutor (Efectivo + Yape)</h3>
                    <div class="relative h-80"><canvas id="chartTutors"></canvas></div>
                </div>
                <div class="lg:col-span-1 premium-card rounded-2xl p-6 shadow-xl flex flex-col justify-between">
                    <h3 class="text-sm font-bold uppercase tracking-wider text-slate-400 mb-5">Distribución Efectivo vs Yape</h3>
                    <div class="relative h-64 flex items-center justify-center"><canvas id="chartDoughnut"></canvas></div>
                    <div class="grid grid-cols-2 gap-4 text-center mt-4 border-t border-slate-800/60 pt-4 text-xs font-semibold text-slate-400">
                        <div><span class="inline-block w-2 h-2 rounded-full bg-sky-400 mr-1.5"></span>Efectivo</div>
                        <div><span class="inline-block w-2 h-2 rounded-full bg-violet-500 mr-1.5"></span>Yape</div>
                    </div>
                </div>
            </div>
            
            <!-- GRÁFICA DE BARRAS CONTROL ALUMNOS -->
            <div class="premium-card rounded-2xl p-6 shadow-xl">
                <h3 class="text-sm font-bold uppercase tracking-wider text-slate-400 mb-5">Control General de Alumnos (Matriculados vs Meta vs Pagantes)</h3>
                <div class="relative h-80"><canvas id="chartStudents"></canvas></div>
            </div>
        </div>

        <!-- VISTA 2: CLASIFICACIÓN -->
        <div id="view-clasificacion" class="tab-view hidden space-y-6">
            <div class="premium-card rounded-2xl p-6 shadow-xl max-w-2xl mx-auto">
                <div class="mb-5 border-b border-slate-800/80 pb-3">
                    <h3 class="text-lg font-bold text-white tracking-tight">🏆 Ranking de tutores</h3>
                </div>
                <div class="space-y-4" id="leaderboard-container"></div>
            </div>
        </div>

        <!-- VISTA 3: BALANCE DE PAGOS -->
        <div id="view-pagos" class="tab-view hidden space-y-8">
            <div class="grid grid-cols-1 md:grid-cols-2 gap-8 max-w-4xl mx-auto">
                <div class="premium-card rounded-2xl p-6 shadow-xl space-y-5">
                    <div class="border-b border-slate-800/80 pb-3"><h3 class="text-base font-bold text-white">💵 Efectivo vs Yape</h3></div>
                    <div class="space-y-3">
                        <div class="flex justify-between items-center bg-slate-900/40 p-3.5 rounded-xl border border-slate-800/60">
                            <span class="text-sm text-slate-400">Efectivo</span>
                            <span class="text-lg font-bold text-sky-400" id="box-efectivo-total">...</span>
                        </div>
                        <div class="flex justify-between items-center bg-slate-900/40 p-3.5 rounded-xl border border-slate-800/60">
                            <span class="text-sm text-slate-400">Yape</span>
                            <span class="text-lg font-bold text-violet-400" id="box-yape-total">...</span>
                        </div>
                        <div class="flex justify-between items-center bg-slate-900/20 p-4 rounded-xl border border-slate-700/30">
                            <span class="text-sm text-slate-200 font-semibold">Total recolectado</span>
                            <span class="text-xl font-black text-emerald-400" id="box-recaudado-total">...</span>
                        </div>
                    </div>
                </div>
                <div class="premium-card rounded-2xl p-6 shadow-xl space-y-5">
                    <div class="border-b border-slate-800/80 pb-3"><h3 class="text-base font-bold text-white">👥 Pagantes vs Meta</h3></div>
                    <div class="space-y-3">
                        <div class="flex justify-between items-center bg-slate-900/40 p-3.5 rounded-xl border border-slate-800/60">
                            <span class="text-sm text-slate-400">Meta total alumnos</span>
                            <span class="text-lg font-bold text-slate-200" id="box-meta-alumnos">...</span>
                        </div>
                        <div class="flex justify-between items-center bg-slate-900/40 p-3.5 rounded-xl border border-slate-800/60">
                            <span class="text-sm text-slate-400">Pagantes actuales</span>
                            <span class="text-lg font-bold text-sky-400" id="box-pagantes-actuales">...</span>
                        </div>
                        <div class="flex justify-between items-center bg-slate-900/20 p-4 rounded-xl border border-rose-500/20 bg-rose-500/5">
                            <span class="text-sm text-rose-300 font-semibold">Faltan</span>
                            <span class="text-xl font-black text-rose-400" id="box-pagantes-falta">...</span>
                        </div>
                    </div>
                </div>
            </div>
        </div>

        <!-- VISTA 4: TABLA DE TUTORES -->
        <div id="view-tutores" class="tab-view hidden space-y-6">
            <section class="premium-card rounded-2xl overflow-hidden shadow-2xl">
                <div class="p-5 border-b border-slate-800/80 bg-slate-950/20">
                    <h3 class="text-base font-bold text-white tracking-tight">Detalle por Tutor</h3>
                </div>
                <div class="w-full overflow-x-auto lg:overflow-x-hidden">
                    <table class="w-full text-left border-collapse text-[11px] sm:text-xs">
                        <thead>
                            <tr class="bg-slate-950 text-slate-400 font-bold uppercase tracking-wider border-b border-slate-800 text-[10px]">
                                <th class="py-3.5 px-3">Tutor</th>
                                <th class="py-3.5 px-2">Ciclo</th>
                                <th class="py-3.5 px-2 text-center">Matr.</th>
                                <th class="py-3.5 px-2 text-center text-orange-400">Meta</th>
                                <th class="py-3.5 px-2 text-center text-sky-400">Pag.</th>
                                <th class="py-3.5 px-2 text-right text-amber-400">Meta (S/)</th>
                                <th class="py-3.5 px-2 text-right text-teal-400">Efectivo</th>
                                <th class="py-3.5 px-2 text-right text-violet-400">Yape</th>
                                <th class="py-3.5 px-2 text-right text-emerald-400">Total</th>
                                <th class="py-3.5 px-2 text-right text-rose-400">Falta</th>
                                <th class="py-3.5 px-3 text-center">Avance</th>
                            </tr>
                        </thead>
                        <tbody class="divide-y divide-slate-800/40 font-semibold text-slate-300" id="table-body-tutors"></tbody>
                    </table>
                </div>
            </section>
        </div>
    </main>

    <script>
        const SHEET_JSON_URL = 'https://docs.google.com/spreadsheets/d/1z2qJzZMr5c_lTLUDU2uXEZpUJ8JhL-tq8COC5GXKcVQ/gviz/tq?tqx=out:json&gid=6209676';

        let chartBar = null;
        let chartPie = null;
        let chartStudents = null;
        let isFirstLoad = true; // Control de la pantalla de bienvenida

        function switchTab(targetId) {
            document.querySelectorAll('.tab-view').forEach(view => view.classList.add('hidden'));
            document.getElementById(targetId).classList.remove('hidden');

            document.querySelectorAll('.nav-card').forEach(btn => {
                btn.className = "nav-card premium-card text-left rounded-2xl p-5 hover:bg-slate-800/30 hover:border-slate-700/50";
            });
            document.getElementById('btn-' + targetId).className = "nav-card premium-card text-left rounded-2xl p-5 border-indigo-500/40 bg-indigo-500/5 ring-1 ring-indigo-500/20 shadow-lg shadow-indigo-500/5";
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

                const tRow = rows[totalsIndex + 1] || rows[totalsIndex];
                let efGlobal = 0, yGlobal = 0, matrGlobal = 0, metaAlGlobal = 0, pagGlobal = 0;
                let avanceGlobalNum = 0;

                if(tRow && tRow.c) {
                    matrGlobal = getVal(tRow.c[2], true);
                    metaAlGlobal = getVal(tRow.c[3], true);
                    pagGlobal = getVal(tRow.c[4], true);
                    efGlobal = getVal(tRow.c[6], true);
                    yGlobal = getVal(tRow.c[7], true);

                    document.getElementById('txt-meta-global').innerText = `Meta Total: S/ ${getVal(tRow.c[5], true).toLocaleString('es-PE')}`;
                    document.getElementById('txt-recaudado-global').innerText = `S/ ${getVal(tRow.c[8], true).toLocaleString('es-PE')}`;
                    document.getElementById('txt-falta-global').innerText = `S/ ${getVal(tRow.c[9], true).toLocaleString('es-PE')}`;
                    
                    let avanceStr = getVal(tRow.c[10]);
                    if (avanceStr.includes('%')) {
                        avanceGlobalNum = parseInt(avanceStr);
                    } else {
                        let numA = getVal(tRow.c[10], true);
                        avanceGlobalNum = numA <= 1.5 ? Math.round(numA * 100) : Math.round(numA);
                    }
                    
                    document.getElementById('txt-avance-global').innerText = avanceGlobalNum + '%';
                    document.getElementById('bar-avance-global').style.width = avanceGlobalNum + '%';

                    document.getElementById('box-efectivo-total').innerText = `S/ ${efGlobal.toLocaleString('es-PE', {minimumFractionDigits:0})}`;
                    document.getElementById('box-yape-total').innerText = `S/ ${yGlobal.toLocaleString('es-PE', {minimumFractionDigits:0})}`;
                    document.getElementById('box-recaudado-total').innerText = `S/ ${getVal(tRow.c[8], true).toLocaleString('es-PE', {minimumFractionDigits:0})}`;

                    document.getElementById('box-meta-alumnos').innerText = metaAlGlobal;
                    document.getElementById('box-pagantes-actuales').innerText = pagGlobal;
                    document.getElementById('box-pagantes-falta').innerText = (metaAlGlobal - Math.floor(pagGlobal));

                    document.getElementById('error-box').className = 'hidden';
                }

                // GESTIÓN DE LA PANTALLA DE BIENVENIDA (SPLASH SCREEN)
                if (isFirstLoad) {
                    document.getElementById('welcome-avance').innerText = avanceGlobalNum + '% Avance';
                    let faltaProgreso = 100 - avanceGlobalNum;
                    if (faltaProgreso < 0) faltaProgreso = 0;
                    document.getElementById('welcome-falta').innerText = `Falta ${faltaProgreso}% para la meta`;
                    
                    document.getElementById('welcome-loading').classList.add('hidden');
                    document.getElementById('welcome-stats').classList.remove('hidden');
                    document.getElementById('welcome-stats').classList.add('flex');
                    document.getElementById('welcome-content').classList.remove('animate-pulse');
                    
                    setTimeout(() => {
                        const overlay = document.getElementById('welcome-overlay');
                        overlay.classList.remove('opacity-100');
                        overlay.classList.add('opacity-0');
                        setTimeout(() => overlay.remove(), 700);
                    }, 3500);
                    
                    isFirstLoad = false;
                }

                const tutorsData = [];
                for (let i = 0; i < totalsIndex; i++) {
                    const row = rows[i];
                    if (!row || !row.c || !row.c[0]) continue;

                    let tutorName = getVal(row.c[0]).trim();
                    if (tutorName === '' || tutorName.toUpperCase() === 'TUTOR' || tutorName.toUpperCase() === 'TOTALES') continue;

                    let avanceStr = getVal(row.c[10]);
                    let avanceNum = avanceStr.includes('%') ? parseInt(avanceStr) : Math.round(getVal(row.c[10], true) * 100);
                    if (avanceStr && !avanceStr.includes('%') && getVal(row.c[10], true) <= 1.5) avanceNum = Math.round(getVal(row.c[10], true) * 100);

                    tutorsData.push({
                        tutor: tutorName,
                        ciclo: getVal(row.c[1]).trim(),
                        matriculados: getVal(row.c[2], true),
                        metaEst: getVal(row.c[3], true),
                        pagantes: getVal(row.c[4], true),
                        metaDinero: getVal(row.c[5], true),
                        efectivo: getVal(row.c[6], true),
                        yape: getVal(row.c[7], true),
                        recaudado: getVal(row.c[8], true),
                        falta: getVal(row.c[9], true),
                        avance: avanceNum
                    });
                }

                renderTable(tutorsData);
                renderCharts(tutorsData, efGlobal, yGlobal, matrGlobal, metaAlGlobal, pagGlobal);
                
                const rankedData = [...tutorsData].sort((a, b) => b.avance - a.avance);
                renderLeaderboard(rankedData);

            } catch (error) {
                console.error(error);
                document.getElementById('error-box').className = 'max-w-7xl mx-auto px-4 sm:px-6 lg:px-8 mt-6 block';
                
                // Ocultar pantalla de bienvenida en caso de error
                const overlay = document.getElementById('welcome-overlay');
                if (overlay) overlay.remove();
            }
        }

        function renderTable(data) {
            const tbody = document.getElementById('table-body-tutors');
            tbody.innerHTML = '';
            data.forEach(row => {
                const tr = document.createElement('tr');
                tr.className = "hover:bg-slate-800/30 transition-colors border-b border-slate-800/40 text-xs";
                tr.innerHTML = `
                    <td class="py-3 px-3 font-bold text-slate-100 whitespace-nowrap">
                        <div class="flex items-center space-x-1">
                            <div class="h-1.5 w-1.5 rounded-full flex-shrink-0 ${row.avance >= 100 ? 'bg-emerald-400' : 'bg-indigo-400'}"></div>
                            <span>${row.tutor}</span>
                        </div>
                    </td>
                    <td class="py-3 px-2 text-slate-400 font-medium whitespace-nowrap">${row.ciclo}</td>
                    <td class="py-3 px-2 text-center text-slate-300 font-semibold">${row.matriculados}</td>
                    <td class="py-3 px-2 text-center text-orange-400 font-bold">${row.metaEst}</td>
                    <td class="py-3 px-2 text-center text-sky-400 font-bold">${row.pagantes}</td>
                    <td class="py-3 px-2 text-right text-amber-400 font-bold whitespace-nowrap">S/ ${row.metaDinero.toLocaleString('es-PE', {maximumFractionDigits:0})}</td>
                    <td class="py-3 px-2 text-right text-teal-400 font-medium whitespace-nowrap">S/ ${row.efectivo.toLocaleString('es-PE', {maximumFractionDigits:0})}</td>
                    <td class="py-3 px-2 text-right text-violet-400 font-medium whitespace-nowrap">S/ ${row.yape.toLocaleString('es-PE', {maximumFractionDigits:0})}</td>
                    <td class="py-3 px-2 text-right font-black text-emerald-400 whitespace-nowrap">S/ ${row.recaudado.toLocaleString('es-PE', {maximumFractionDigits:0})}</td>
                    <td class="py-3 px-2 text-right font-bold whitespace-nowrap ${row.falta < 0 ? 'text-emerald-400' : 'text-rose-400'}">S/ ${row.falta.toLocaleString('es-PE', {maximumFractionDigits:0})}</td>
                    <td class="py-3 px-3 text-center">
                        <span class="px-1.5 py-0.5 rounded text-[10px] font-extrabold ${row.avance >= 100 ? 'bg-emerald-500/10 text-emerald-400' : 'bg-indigo-500/10 text-indigo-400'}">${row.avance}%</span>
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
                    index === 2 ? 'bg-amber-700/10 border-amber-700/30' : 'bg-slate-800/30 border-slate-700/50'
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

        function renderCharts(data, efectivoGlobal, yapeGlobal, matrGlobal, metaAlGlobal, pagGlobal) {
            const ctxBar = document.getElementById('chartTutors').getContext('2d');
            if (chartBar) { chartBar.destroy(); }
            chartBar = new Chart(ctxBar, {
                type: 'bar',
                data: {
                    labels: data.map(r => r.tutor.split(' ')[0] + ' ' + (r.tutor.split(' ')[1] || '')),
                    datasets: [
                        { label: 'Meta Asignada (S/)', data: data.map(r => r.metaDinero), backgroundColor: 'rgba(71, 85, 105, 0.4)', borderRadius: 4 },
                        { label: 'Total Recaudado (S/)', data: data.map(r => r.recaudado), backgroundColor: 'rgba(16, 185, 129, 0.85)', borderRadius: 4 },
                        { label: 'Falta Recaudar (S/)', data: data.map(r => r.falta > 0 ? r.falta : 0), backgroundColor: 'rgba(239, 68, 68, 0.85)', borderRadius: 4 }
                    ]
                },
                options: {
                    indexAxis: 'y', responsive: true, maintainAspectRatio: false,
                    plugins: { legend: { labels: { color: '#94a3b8' } } },
                    scales: {
                        x: { grid: { color: 'rgba(51, 65, 85, 0.2)' }, ticks: { color: '#94a3b8' } },
                        y: { ticks: { color: '#e2e8f0' } }
                    }
                }
            });

            const ctxPie = document.getElementById('chartDoughnut').getContext('2d');
            if (chartPie) { chartPie.destroy(); }
            chartPie = new Chart(ctxPie, {
                type: 'doughnut',
                data: {
                    labels: ['Efectivo', 'Yape'],
                    datasets: [{
                        data: [efectivoGlobal, yapeGlobal],
                        backgroundColor: ['#38bdf8', '#8b5cf6'],
                        borderColor: '#0f172a', borderWidth: 3
                    }]
                },
                options: {
                    responsive: true, maintainAspectRatio: false,
                    plugins: { legend: { display: false } },
                    cutout: '75%'
                }
            });

            const ctxStudents = document.getElementById('chartStudents').getContext('2d');
            if (chartStudents) { chartStudents.destroy(); }
            chartStudents = new Chart(ctxStudents, {
                type: 'bar',
                plugins: [ChartDataLabels],
                data: {
                    labels: ['Matriculados', 'Meta Alumnos', 'Pagantes Actuales'],
                    datasets: [{
                        data: [matrGlobal, metaAlGlobal, pagGlobal],
                        backgroundColor: ['#6366f1', '#f97316', '#38bdf8'],
                        borderRadius: 6,
                        barThickness: 45
                    }]
                },
                options: {
                    responsive: true, maintainAspectRatio: false,
                    plugins: { 
                        legend: { display: false },
                        datalabels: {
                            anchor: 'end',
                            align: 'top',
                            color: '#f8fafc',
                            font: { family: 'Plus Jakarta Sans', weight: '800', size: 14 },
                            formatter: function(value) { return Math.round(value).toLocaleString('es-PE'); }
                        }
                    },
                    scales: {
                        x: { ticks: { color: '#94a3b8', font: { family: 'Plus Jakarta Sans', weight: '600', size: 12 } }, grid: { display: false } },
                        y: { grace: '15%', grid: { color: 'rgba(51, 65, 85, 0.15)' }, ticks: { color: '#64748b' } }
                    }
                }
            });
        }

        loadDashboardData();
        setInterval(loadDashboardData, 60000);
    </script>
</body>
</html>
