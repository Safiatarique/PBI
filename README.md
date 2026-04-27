<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>RM Dashboard — Habib Bank AG</title>
<link href="https://fonts.googleapis.com/css2?family=IBM+Plex+Sans:wght@300;400;500;600;700&family=IBM+Plex+Mono:wght@300;400;500&family=Playfair+Display:wght@400;600&display=swap" rel="stylesheet">
<script src="https://cdnjs.cloudflare.com/ajax/libs/Chart.js/4.4.0/chart.umd.min.js"></script>
<style>
:root {
--green: #1a6b3c;
--green-dark: #0f4526;
--green-deep: #0a2e1a;
--green-mid: #2d8653;
--green-light: #3dab6a;
--green-pale: #e8f5ee;
--green-tint: rgba(26,107,60,0.08);
--teal: #0e7c6b;
--amber: #e8a020;
--amber-light: #f5c842;
--red: #c0392b;
--red-light: #e74c3c;
--blue: #1a5276;
--blue-light: #2980b9;
--charcoal: #1c2127;
--slate: #2c3540;
--slate-mid: #3d4f5e;
--slate-light: #566879;
--border: #2e3d4a;
--border-light:#3d5060;
--text-1: #e8ecef;
--text-2: #a0b0bc;
--text-3: #6a8090;
--bg: #141c22;
--bg-card: #1a242c;
--bg-card-alt: #1e2c35;
--white: #ffffff;
}

* { margin:0; padding:0; box-sizing:border-box; }
html { font-size: 13px; }

body {
background: var(--bg);
font-family: 'IBM Plex Sans', sans-serif;
color: var(--text-1);
min-height: 100vh;
}

/* ── TOPBAR ──────────────────────────────────────────── */
.topbar {
height: 52px;
background: var(--green-dark);
display: flex;
align-items: center;
justify-content: space-between;
padding: 0 24px;
border-bottom: 2px solid var(--green);
position: sticky; top: 0; z-index: 200;
}

.brand {
display: flex;
align-items: center;
gap: 12px;
}
.brand-logo {
width: 32px; height: 32px;
background: var(--green);
display: flex; align-items: center; justify-content: center;
font-family: 'Playfair Display', serif;
font-size: 14px; font-weight: 600;
color: #fff;
letter-spacing: 1px;
}
.brand-name {
font-size: 13px;
font-weight: 600;
color: var(--white);
letter-spacing: 0.5px;
}
.brand-sub {
font-size: 9px;
color: rgba(255,255,255,0.5);
letter-spacing: 2px;
text-transform: uppercase;
margin-top: 1px;
}

.topbar-center {
display: flex;
align-items: center;
gap: 6px;
}

.filter-pill {
display: flex;
align-items: center;
gap: 6px;
background: rgba(255,255,255,0.07);
border: 1px solid rgba(255,255,255,0.12);
padding: 5px 12px;
border-radius: 2px;
font-size: 11px;
}
.filter-pill label {
color: rgba(255,255,255,0.45);
font-size: 9px;
letter-spacing: 1.5px;
text-transform: uppercase;
white-space: nowrap;
}
.filter-pill select {
background: transparent;
border: none;
color: #fff;
font-family: 'IBM Plex Sans', sans-serif;
font-size: 11px;
font-weight: 500;
cursor: pointer;
outline: none;
min-width: 90px;
}
.filter-pill select option { background: #0f4526; }

.topbar-right {
display: flex;
align-items: center;
gap: 16px;
}
.topbar-stat {
text-align: right;
}
.topbar-stat-val {
font-family: 'IBM Plex Mono', monospace;
font-size: 12px;
font-weight: 500;
color: #fff;
}
.topbar-stat-label {
font-size: 8px;
color: rgba(255,255,255,0.4);
letter-spacing: 1.5px;
text-transform: uppercase;
}
.divider-v {
width: 1px; height: 28px;
background: rgba(255,255,255,0.12);
}
.as-of {
font-size: 9px;
color: rgba(255,255,255,0.35);
letter-spacing: 1px;
}

/* ── SUBBAR ──────────────────────────────────────────── */
.subbar {
height: 32px;
background: var(--charcoal);
border-bottom: 1px solid var(--border);
display: flex;
align-items: center;
padding: 0 24px;
gap: 32px;
overflow-x: auto;
}
.subbar-item {
display: flex;
align-items: center;
gap: 8px;
white-space: nowrap;
font-size: 10px;
}
.subbar-label { color: var(--text-3); letter-spacing: 1px; text-transform: uppercase; }
.subbar-val { font-family: 'IBM Plex Mono', monospace; font-weight: 500; font-size: 10px; }
.tag-pos { color: var(--green-light); }
.tag-neg { color: var(--red-light); }
.tag-neu { color: var(--amber); }
.tag-arrow { font-size: 8px; }

/* ── PAGE BODY ───────────────────────────────────────── */
.page { padding: 16px 20px 24px; display: flex; flex-direction: column; gap: 14px; }

/* ── SECTION HEADER ──────────────────────────────────── */
.section-hdr {
display: flex;
align-items: center;
gap: 10px;
margin-bottom: 10px;
}
.section-hdr-bar { width: 3px; height: 14px; background: var(--green); }
.section-hdr-text {
font-size: 9px;
font-weight: 600;
letter-spacing: 2.5px;
text-transform: uppercase;
color: var(--text-2);
}
.section-hdr-line { flex: 1; height: 1px; background: var(--border); }

/* ── GRID ────────────────────────────────────────────── */
.row { display: grid; gap: 12px; }
.cols-5 { grid-template-columns: repeat(5,1fr); }
.cols-4 { grid-template-columns: repeat(4,1fr); }
.cols-3 { grid-template-columns: repeat(3,1fr); }
.cols-2 { grid-template-columns: repeat(2,1fr); }
.cols-2-1 { grid-template-columns: 2fr 1fr; }
.cols-1-2 { grid-template-columns: 1fr 2fr; }
.cols-3-2 { grid-template-columns: 3fr 2fr; }
.cols-5-3 { grid-template-columns: 5fr 3fr; }

/* ── CARD ────────────────────────────────────────────── */
.card {
background: var(--bg-card);
border: 1px solid var(--border);
padding: 16px;
position: relative;
overflow: hidden;
}
.card-alt { background: var(--bg-card-alt); }

.card-title {
font-size: 8.5px;
font-weight: 600;
letter-spacing: 2px;
text-transform: uppercase;
color: var(--text-3);
margin-bottom: 14px;
display: flex;
align-items: center;
justify-content: space-between;
}
.card-title-badge {
font-size: 8px;
padding: 2px 7px;
background: var(--green-tint);
color: var(--green-light);
border: 1px solid rgba(61,171,106,0.2);
letter-spacing: 1px;
border-radius: 1px;
}

/* ── KPI CARDS ───────────────────────────────────────── */
.kpi-big {
font-family: 'IBM Plex Mono', monospace;
font-size: 28px;
font-weight: 300;
color: var(--text-1);
line-height: 1;
margin-bottom: 4px;
letter-spacing: -1px;
}
.kpi-big .unit {
font-size: 14px;
color: var(--text-3);
font-weight: 300;
letter-spacing: 0;
}
.kpi-sub {
font-size: 9px;
color: var(--text-3);
margin-bottom: 10px;
}
.kpi-delta {
display: inline-flex;
align-items: center;
gap: 3px;
font-family: 'IBM Plex Mono', monospace;
font-size: 9px;
padding: 2px 7px;
border-radius: 1px;
}
.delta-pos { background: rgba(61,171,106,0.1); color: var(--green-light); border: 1px solid rgba(61,171,106,0.2); }
.delta-neg { background: rgba(192,57,43,0.1); color: var(--red-light); border: 1px solid rgba(192,57,43,0.2); }
.delta-neu { background: rgba(232,160,32,0.1); color: var(--amber); border: 1px solid rgba(232,160,32,0.2); }

.kpi-sparkbar {
height: 3px;
background: var(--border);
margin-top: 10px;
border-radius: 1px;
overflow: hidden;
}
.kpi-sparkbar-fill { height: 100%; border-radius: 1px; transition: width 1.2s ease; }

.kpi-accent-line {
position: absolute;
left: 0; top: 0; bottom: 0;
width: 3px;
}

/* ── TABLE ───────────────────────────────────────────── */
.data-table { width: 100%; border-collapse: collapse; }
.data-table thead tr { border-bottom: 1px solid var(--border); }
.data-table th {
font-size: 8px;
font-weight: 600;
letter-spacing: 1.5px;
text-transform: uppercase;
color: var(--text-3);
padding: 0 0 8px;
text-align: left;
white-space: nowrap;
}
.data-table th.r { text-align: right; }
.data-table td {
padding: 7px 0;
font-size: 11px;
border-bottom: 1px solid rgba(255,255,255,0.03);
color: var(--text-2);
vertical-align: middle;
}
.data-table td.r { text-align: right; font-family: 'IBM Plex Mono', monospace; font-size: 10px; }
.data-table td.primary { color: var(--text-1); font-weight: 500; }
.data-table td.green-val { color: var(--green-light); font-family: 'IBM Plex Mono', monospace; font-size: 10px; text-align: right; }
.data-table td.amber-val { color: var(--amber); font-family: 'IBM Plex Mono', monospace; font-size: 10px; text-align: right; }
.data-table tr:last-child td { border-bottom: none; }
.data-table tr:hover td { background: rgba(255,255,255,0.015); }

/* ── SEGMENT TAG ─────────────────────────────────────── */
.seg {
display: inline-block;
font-size: 7.5px;
font-weight: 700;
letter-spacing: 1px;
padding: 2px 5px;
border-radius: 1px;
text-transform: uppercase;
}
.seg-U { background: rgba(232,160,32,0.12); color: var(--amber); border: 1px solid rgba(232,160,32,0.25); }
.seg-H { background: rgba(41,128,185,0.12); color: #5dade2; border: 1px solid rgba(41,128,185,0.25); }
.seg-A { background: rgba(61,171,106,0.12); color: var(--green-light); border: 1px solid rgba(61,171,106,0.25); }
.seg-B { background: rgba(100,120,130,0.12); color: #8899aa; border: 1px solid rgba(100,120,130,0.25); }

/* ── KYC BADGE ───────────────────────────────────────── */
.kyc-badge {
font-family: 'IBM Plex Mono', monospace;
font-size: 11px;
font-weight: 600;
width: 32px; height: 24px;
display: inline-flex; align-items: center; justify-content: center;
border-radius: 1px;
}
.kyc-hot { background: rgba(192,57,43,0.15); color: #e74c3c; border: 1px solid rgba(192,57,43,0.3); }
.kyc-warn { background: rgba(232,160,32,0.12); color: var(--amber); border: 1px solid rgba(232,160,32,0.25); }
.kyc-ok { background: rgba(61,171,106,0.1); color: var(--green-light); border: 1px solid rgba(61,171,106,0.2); }

/* ── DONUT WRAP ──────────────────────────────────────── */
.donut-flex { display: flex; align-items: center; gap: 18px; }
.donut-canvas { flex-shrink: 0; }
.donut-legend-list { flex: 1; }
.dl-item {
display: flex;
align-items: center;
justify-content: space-between;
padding: 5px 0;
border-bottom: 1px solid rgba(255,255,255,0.04);
font-size: 10px;
}
.dl-item:last-child { border-bottom: none; }
.dl-left { display: flex; align-items: center; gap: 7px; }
.dl-dot { width: 7px; height: 7px; border-radius: 50%; }
.dl-label { color: var(--text-2); font-size: 10px; }
.dl-right { display: flex; gap: 10px; align-items: center; }
.dl-count { font-family: 'IBM Plex Mono', monospace; font-size: 10px; color: var(--text-1); }
.dl-pct { font-family: 'IBM Plex Mono', monospace; font-size: 9px; color: var(--text-3); }

/* ── RoA BARS ────────────────────────────────────────── */
.roa-item {
display: flex;
align-items: center;
gap: 10px;
padding: 5px 0;
border-bottom: 1px solid rgba(255,255,255,0.03);
}
.roa-item:last-child { border-bottom: none; }
.roa-name { font-size: 10px; color: var(--text-2); width: 120px; flex-shrink: 0; white-space: nowrap; overflow: hidden; text-overflow: ellipsis; }
.roa-track { flex: 1; height: 5px; background: rgba(255,255,255,0.05); border-radius: 2px; overflow: hidden; position: relative; }
.roa-fill { height: 100%; border-radius: 2px; transition: width 1.4s cubic-bezier(0.16,1,0.3,1); }
.roa-val { font-family: 'IBM Plex Mono', monospace; font-size: 9px; color: var(--text-2); width: 40px; text-align: right; flex-shrink: 0; }
.roa-avg-marker {
position: absolute;
top: -2px; bottom: -2px;
width: 1px;
background: var(--amber);
opacity: 0.7;
}

/* ── AuM RANK ────────────────────────────────────────── */
.aum-item {
display: flex;
align-items: center;
gap: 8px;
padding: 5px 0;
border-bottom: 1px solid rgba(255,255,255,0.03);
}
.aum-item:last-child { border-bottom: none; }
.aum-rank { font-family: 'IBM Plex Mono', monospace; font-size: 9px; color: var(--text-3); width: 18px; }
.aum-name { font-size: 10px; color: var(--text-2); flex: 1; overflow: hidden; text-overflow: ellipsis; white-space: nowrap; }
.aum-bar-wrap { width: 70px; }
.aum-mini-bar { height: 3px; background: rgba(255,255,255,0.05); border-radius: 1px; overflow: hidden; }
.aum-mini-fill { height: 100%; background: var(--green-mid); border-radius: 1px; }
.aum-val { font-family: 'IBM Plex Mono', monospace; font-size: 9px; color: var(--text-1); width: 48px; text-align: right; flex-shrink: 0; }

/* ── ACTIVE SPLIT ────────────────────────────────────── */
.active-split-bar {
display: flex;
height: 6px;
border-radius: 2px;
overflow: hidden;
gap: 2px;
margin: 12px 0 8px;
}
.as-active { background: var(--green-mid); border-radius: 2px 0 0 2px; }
.as-inactive { background: rgba(192,57,43,0.45); border-radius: 0 2px 2px 0; flex: 1; }
.active-nums { display: grid; grid-template-columns: 1fr 1fr; gap: 8px; margin-top: 12px; }
.active-num-card {
padding: 10px 12px;
border: 1px solid var(--border);
background: rgba(255,255,255,0.02);
text-align: center;
}
.active-num-val { font-family: 'IBM Plex Mono', monospace; font-size: 22px; font-weight: 300; line-height: 1; }
.active-num-val.v-active { color: var(--green-light); }
.active-num-val.v-inactive { color: var(--red-light); }
.active-num-label { font-size: 8px; letter-spacing: 1.5px; text-transform: uppercase; color: var(--text-3); margin-top: 4px; }

/* ── TCF LEGEND ──────────────────────────────────────── */
.chart-legend {
display: flex;
gap: 16px;
flex-wrap: wrap;
margin-bottom: 12px;
}
.cl-item { display: flex; align-items: center; gap: 5px; font-size: 9px; color: var(--text-3); }
.cl-box { width: 10px; height: 10px; border-radius: 1px; }
.cl-line { width: 16px; height: 2px; border-top: 2px dashed var(--text-3); margin: 0 1px; }

/* ── NNM TOTALS ──────────────────────────────────────── */
.nnm-mini-stats { display: grid; grid-template-columns: repeat(3,1fr); gap: 8px; margin-bottom: 12px; }
.nms {
padding: 8px 10px;
background: rgba(255,255,255,0.02);
border: 1px solid var(--border);
}
.nms-label { font-size: 8px; color: var(--text-3); letter-spacing: 1.5px; text-transform: uppercase; margin-bottom: 3px; }
.nms-val { font-family: 'IBM Plex Mono', monospace; font-size: 12px; font-weight: 500; }

/* ── AuM BIG SPLIT ───────────────────────────────────── */
.aum-split-nums { display: grid; grid-template-columns: 1fr 1fr; gap: 10px; margin-bottom: 14px; }
.aum-sn {
padding: 10px 12px;
border: 1px solid var(--border);
background: rgba(255,255,255,0.02);
}
.aum-sn-label { font-size: 8px; color: var(--text-3); letter-spacing: 1.5px; text-transform: uppercase; margin-bottom: 4px; }
.aum-sn-val { font-family: 'IBM Plex Mono', monospace; font-size: 18px; font-weight: 300; color: var(--text-1); }
.aum-sn-pct { font-size: 9px; color: var(--text-3); margin-top: 2px; }

/* ── FOOTER ──────────────────────────────────────────── */
.footer {
margin: 8px 20px 0;
padding: 10px 0;
border-top: 1px solid var(--border);
display: flex;
justify-content: space-between;
align-items: center;
}
.footer p { font-size: 8px; color: var(--text-3); letter-spacing: 1.5px; text-transform: uppercase; }

/* ── ANIMATIONS ──────────────────────────────────────── */
@keyframes fadeIn { from { opacity:0; transform: translateY(8px); } to { opacity:1; transform: translateY(0); } }
.card { animation: fadeIn 0.4s ease both; }
.row .card:nth-child(1) { animation-delay: 0.05s; }
.row .card:nth-child(2) { animation-delay: 0.10s; }
.row .card:nth-child(3) { animation-delay: 0.15s; }
.row .card:nth-child(4) { animation-delay: 0.20s; }
.row .card:nth-child(5) { animation-delay: 0.25s; }

/* ── CHART CANVAS HEIGHT HELPERS ─────────────────────── */
.h180 { height: 180px; }
.h150 { height: 150px; }
.h200 { height: 200px; }
.h240 { height: 240px; }
.chart-wrap { position: relative; width: 100%; }
</style>
</head>
<body>

<!-- ── TOPBAR ── -->
<div class="topbar">
<div class="brand">
<div class="brand-logo">HB</div>
<div>
<div class="brand-name">Habib Bank AG Zürich</div>
<div class="brand-sub">Relationship Manager Dashboard · Confidential</div>
</div>
</div>

<div class="topbar-center">
<div class="filter-pill">
<label>Branch</label>
<select onchange="applyFilter()">
<option>All Branches</option>
<option>DIFC</option>
<option>Zurich</option>
</select>
</div>
<div class="filter-pill">
<label>RM</label>
<select onchange="applyFilter()">
<option>All RMs</option>
<option>Ahmed Ishtiaq</option>
<option>Akhund Haseeb</option>
<option>Ali Salman</option>
<option>Choudhry Tariq</option>
<option>Gorana Gadafi</option>
<option>Gulati Alok</option>
<option>Kievits Maurice</option>
<option>Kohli Aneeta</option>
<option>Memon Ibad Z</option>
<option>Narayanan Sunil</option>
<option>Qureshi Qais</option>
<option>Waffa Wissam</option>
<option>Yunus Malik Imran</option>
</select>
</div>
<div class="filter-pill">
<label>Period</label>
<select>
<option>YTD Apr 2026</option>
<option>Q1 2026</option>
<option>Apr 2026</option>
<option>Mar 2026</option>
<option>Feb 2026</option>
<option>Jan 2026</option>
</select>
</div>
</div>

<div class="topbar-right">
<div class="topbar-stat">
<div class="topbar-stat-val">USD 2.84B</div>
<div class="topbar-stat-label">Grand TCF</div>
</div>
<div class="divider-v"></div>
<div class="topbar-stat">
<div class="topbar-stat-val" style="color:var(--green-light)">0.316%</div>
<div class="topbar-stat-label">Blended RoA</div>
</div>
<div class="divider-v"></div>
<div class="topbar-stat">
<div class="topbar-stat-val" style="color:var(--amber)">USD 312M</div>
<div class="topbar-stat-label">YTD NNM</div>
</div>
<div class="divider-v"></div>
<div class="as-of">As of 23 Apr 2026</div>
</div>
</div>

<!-- ── SUBBAR ── -->
<div class="subbar">
<div class="subbar-item">
<span class="subbar-label">TCF vs Dec-25</span>
<span class="subbar-val tag-pos">▲ +USD 112.4M</span>
</div>
<div class="subbar-item">
<span class="subbar-label">TCF vs Budget</span>
<span class="subbar-val tag-neg">▼ −USD 17.6M</span>
</div>
<div class="subbar-item">
<span class="subbar-label">NNM 2026 vs 2025</span>
<span class="subbar-val tag-pos">▲ +USD 18.2M</span>
</div>
<div class="subbar-item">
<span class="subbar-label">Active Clients</span>
<span class="subbar-val tag-pos">1,341 ▲ 12</span>
</div>
<div class="subbar-item">
<span class="subbar-label">NTB YTD</span>
<span class="subbar-val tag-pos">47 clients</span>
</div>
<div class="subbar-item">
<span class="subbar-label">KYC Overdue</span>
<span class="subbar-val tag-neg">23 clients ▲ 3</span>
</div>
<div class="subbar-item">
<span class="subbar-label">Top 10 AuM Conc.</span>
<span class="subbar-val tag-neu">68.3% of book</span>
</div>
</div>

<div class="page">

<!-- ── ROW 1: KPI CARDS ── -->
<div>
<div class="section-hdr">
<div class="section-hdr-bar"></div>
<div class="section-hdr-text">Performance Overview</div>
<div class="section-hdr-line"></div>
</div>
<div class="row cols-5">

<div class="card">
<div class="kpi-accent-line" style="background:var(--green)"></div>
<div class="card-title">Total TCF · Apr-26<span class="card-title-badge">▲ 4.2%</span></div>
<div class="kpi-big">2.84<span class="unit"> B</span></div>
<div class="kpi-sub">USD · D + S + F − Advances</div>
<div class="kpi-delta delta-pos">▲ +112.4M vs Dec-25</div>
<div class="kpi-sparkbar"><div class="kpi-sparkbar-fill" style="width:78%;background:var(--green-mid)"></div></div>
</div>

<div class="card">
<div class="kpi-accent-line" style="background:var(--amber)"></div>
<div class="card-title">YTD NNM 2026<span class="card-title-badge">▲ 6.2%</span></div>
<div class="kpi-big" style="color:var(--amber)">312<span class="unit">.4M</span></div>
<div class="kpi-sub">Jan–Apr 2026 cumulative</div>
<div class="kpi-delta delta-pos">▲ +18.2M vs same period '25</div>
<div class="kpi-sparkbar"><div class="kpi-sparkbar-fill" style="width:63%;background:var(--amber)"></div></div>
</div>

<div class="card">
<div class="kpi-accent-line" style="background:var(--blue-light)"></div>
<div class="card-title">Blended RoA</div>
<div class="kpi-big" style="color:var(--blue-light)">0.316<span class="unit">%</span></div>
<div class="kpi-sub">Revenue ÷ TCF · All RMs</div>
<div class="kpi-delta delta-neg">▼ −0.02% vs Dec-25</div>
<div class="kpi-sparkbar"><div class="kpi-sparkbar-fill" style="width:31.6%;background:var(--blue-light)"></div></div>
</div>

<div class="card">
<div class="kpi-accent-line" style="background:var(--green-light)"></div>
<div class="card-title">Active Clients</div>
<div class="kpi-big" style="color:var(--green-light)">1,341<span class="unit"></span></div>
<div class="kpi-sub">of 1,501 total · 89.3% active</div>
<div class="kpi-delta delta-pos">▲ +12 vs last month</div>
<div class="kpi-sparkbar"><div class="kpi-sparkbar-fill" style="width:89.3%;background:var(--green-light)"></div></div>
</div>

<div class="card">
<div class="kpi-accent-line" style="background:var(--red)"></div>
<div class="card-title">KYC Overdue</div>
<div class="kpi-big" style="color:var(--red-light)">23<span class="unit"> clients</span></div>
<div class="kpi-sub">Requiring urgent compliance review</div>
<div class="kpi-delta delta-neg">▲ +3 vs last month</div>
<div class="kpi-sparkbar"><div class="kpi-sparkbar-fill" style="width:23%;background:var(--red)"></div></div>
</div>

</div>
</div>

<!-- ── ROW 2: TCF + NNM ── -->
<div>
<div class="section-hdr">
<div class="section-hdr-bar"></div>
<div class="section-hdr-text">TCF Monthly Comparison — Dec-25 Baseline + 2026 YTD vs 2025 vs Budget</div>
<div class="section-hdr-line"></div>
</div>
<div class="row cols-3-2">

<!-- TCF -->
<div class="card">
<div class="card-title">TCF Comparison · Dec-25 | Jan–Apr 2026 | Budget</div>
<div class="chart-legend">
<div class="cl-item"><div class="cl-box" style="background:var(--green-mid)"></div>2026 Actual</div>
<div class="cl-item"><div class="cl-box" style="background:rgba(41,128,185,0.6)"></div>2025 Actual</div>
<div class="cl-item"><div class="cl-box" style="background:rgba(232,160,32,0.5)"></div>Dec-25 Baseline</div>
<div class="cl-item"><div class="cl-line" style="border-color:var(--amber);opacity:0.6"></div>Budget 2026</div>
</div>
<div class="chart-wrap h240"><canvas id="tcfChart"></canvas></div>
</div>

<!-- NNM -->
<div class="card">
<div class="card-title">NNM · 2025 vs 2026 Monthly</div>
<div class="nnm-mini-stats">
<div class="nms">
<div class="nms-label">YTD 2026</div>
<div class="nms-val tag-pos">+312.4M</div>
</div>
<div class="nms">
<div class="nms-label">YTD 2025</div>
<div class="nms-val tag-neu">+294.2M</div>
</div>
<div class="nms">
<div class="nms-label">Variance</div>
<div class="nms-val tag-pos">+18.2M</div>
</div>
</div>
<div class="chart-wrap h200"><canvas id="nnmChart"></canvas></div>
</div>

</div>
</div>

<!-- ── ROW 3: SEGMENTS + ACTIVE + KYC ── -->
<div>
<div class="section-hdr">
<div class="section-hdr-bar"></div>
<div class="section-hdr-text">Client Composition</div>
<div class="section-hdr-line"></div>
</div>
<div class="row cols-3">

<!-- Segments -->
<div class="card">
<div class="card-title">Client Segmentation · 1,501 Total</div>
<div class="donut-flex">
<canvas class="donut-canvas" id="segChart" width="120" height="120"></canvas>
<div class="donut-legend-list">
<div class="dl-item">
<div class="dl-left"><div class="dl-dot" style="background:var(--amber)"></div><span class="dl-label">UHNW</span></div>
<div class="dl-right"><span class="dl-count">187</span><span class="dl-pct">12.4%</span></div>
</div>
<div class="dl-item">
<div class="dl-left"><div class="dl-dot" style="background:#2980b9"></div><span class="dl-label">HNW</span></div>
<div class="dl-right"><span class="dl-count">423</span><span class="dl-pct">28.2%</span></div>
</div>
<div class="dl-item">
<div class="dl-left"><div class="dl-dot" style="background:var(--green-mid)"></div><span class="dl-label">Affluent</span></div>
<div class="dl-right"><span class="dl-count">614</span><span class="dl-pct">40.9%</span></div>
</div>
<div class="dl-item">
<div class="dl-left"><div class="dl-dot" style="background:var(--slate-mid)"></div><span class="dl-label">Below 250K</span></div>
<div class="dl-right"><span class="dl-count">277</span><span class="dl-pct">18.5%</span></div>
</div>
<div style="padding-top:8px;border-top:1px solid var(--border);margin-top:2px;display:flex;justify-content:space-between">
<span style="font-size:8px;color:var(--text-3);letter-spacing:1px;text-transform:uppercase">Total</span>
<span style="font-family:'IBM Plex Mono',monospace;font-size:11px;color:var(--text-1)">1,501</span>
</div>
</div>
</div>
</div>

<!-- Active vs Inactive -->
<div class="card">
<div class="card-title">Active vs Inactive Clients</div>
<div class="active-split-bar">
<div class="as-active" style="width:89.3%"></div>
<div class="as-inactive"></div>
</div>
<div style="display:flex;justify-content:space-between;font-size:8px;color:var(--text-3);margin-bottom:4px">
<span>89.3% Active</span><span>10.7% Inactive</span>
</div>
<div class="active-nums">
<div class="active-num-card">
<div class="active-num-val v-active">1,341</div>
<div class="active-num-label">Active</div>
</div>
<div class="active-num-card">
<div class="active-num-val v-inactive">160</div>
<div class="active-num-label">Inactive</div>
</div>
</div>
<div class="chart-wrap h100" style="height:95px;margin-top:12px">
<canvas id="activeChart"></canvas>
</div>
</div>

<!-- KYC -->
<div class="card">
<div class="card-title">KYC Overdue by RM · 23 Total<span class="card-title-badge" style="background:rgba(192,57,43,0.1);color:var(--red-light);border-color:rgba(192,57,43,0.3)">ACTION REQUIRED</span></div>
<table class="data-table">
<thead><tr>
<th>RM</th>
<th>Branch</th>
<th class="r">Overdue</th>
</tr></thead>
<tbody>
<tr><td class="primary">Gulati Alok</td><td>Zurich</td><td class="r"><span class="kyc-badge kyc-hot">7</span></td></tr>
<tr><td class="primary">Ali Salman</td><td>Zurich</td><td class="r"><span class="kyc-badge kyc-hot">5</span></td></tr>
<tr><td class="primary">Narayanan Sunil</td><td>Zurich</td><td class="r"><span class="kyc-badge kyc-warn">4</span></td></tr>
<tr><td class="primary">Choudhry Tariq</td><td>Zurich</td><td class="r"><span class="kyc-badge kyc-warn">3</span></td></tr>
<tr><td class="primary">Ahmed Ishtiaq</td><td>Zurich</td><td class="r"><span class="kyc-badge kyc-warn">2</span></td></tr>
<tr><td class="primary">Memon Ibad Z</td><td>DIFC</td><td class="r"><span class="kyc-badge kyc-warn">1</span></td></tr>
<tr><td class="primary">Qureshi Qais</td><td>DIFC</td><td class="r"><span class="kyc-badge kyc-warn">1</span></td></tr>
<tr><td class="primary">Kohli Aneeta</td><td>DIFC</td><td class="r"><span class="kyc-badge kyc-ok">0</span></td></tr>
</tbody>
</table>
</div>

</div>
</div>

<!-- ── ROW 4: AuM + RoA + NTB ── -->
<div>
<div class="section-hdr">
<div class="section-hdr-bar"></div>
<div class="section-hdr-text">AuM Concentration · Return on Assets · New to Bank</div>
<div class="section-hdr-line"></div>
</div>
<div class="row cols-3">

<!-- AuM Concentration -->
<div class="card">
<div class="card-title">AuM Concentration · Top 10 vs Rest</div>
<div class="aum-split-nums">
<div class="aum-sn">
<div class="aum-sn-label">Top 10 AuM</div>
<div class="aum-sn-val" style="color:var(--amber)">USD 1.94B</div>
<div class="aum-sn-pct">68.3% of total book</div>
</div>
<div class="aum-sn">
<div class="aum-sn-label">Other 1,491 Clients</div>
<div class="aum-sn-val">USD 0.90B</div>
<div class="aum-sn-pct">31.7% of total book</div>
</div>
</div>
<div class="chart-wrap" style="height:110px;margin-bottom:12px">
<canvas id="aumDonutChart"></canvas>
</div>
<div style="font-size:8px;color:var(--text-3);letter-spacing:1.5px;text-transform:uppercase;margin-bottom:8px;padding-bottom:6px;border-bottom:1px solid var(--border)">Top Client Ranking by AuM</div>
<div id="aumRankList">
<div class="aum-item"><span class="aum-rank">01</span><span class="aum-name">Alupak Limited</span><div class="aum-bar-wrap"><div class="aum-mini-bar"><div class="aum-mini-fill" style="width:100%"></div></div></div><span class="aum-val">318.9M</span></div>
<div class="aum-item"><span class="aum-rank">02</span><span class="aum-name">Munir & Maliha Bhimjee</span><div class="aum-bar-wrap"><div class="aum-mini-bar"><div class="aum-mini-fill" style="width:88%"></div></div></div><span class="aum-val">280.4M</span></div>
<div class="aum-item"><span class="aum-rank">03</span><span class="aum-name">Khan Ibrahim</span><div class="aum-bar-wrap"><div class="aum-mini-bar"><div class="aum-mini-fill" style="width:72%"></div></div></div><span class="aum-val">229.1M</span></div>
<div class="aum-item"><span class="aum-rank">04</span><span class="aum-name">Zarya Holding B.V.</span><div class="aum-bar-wrap"><div class="aum-mini-bar"><div class="aum-mini-fill" style="width:64%"></div></div></div><span class="aum-val">203.7M</span></div>
<div class="aum-item"><span class="aum-rank">05</span><span class="aum-name">FIFE Enterprises FZ-LLC</span><div class="aum-bar-wrap"><div class="aum-mini-bar"><div class="aum-mini-fill" style="width:56%"></div></div></div><span class="aum-val">178.2M</span></div>
</div>
</div>

<!-- RoA -->
<div class="card">
<div class="card-title">RoA by RM · Revenue ÷ TCF<span class="card-title-badge">Avg 0.316%</span></div>
<div style="font-size:8px;color:var(--text-3);margin-bottom:12px">
<span style="color:var(--amber)">│</span> Amber marker = firm average (0.316%)
</div>
<div id="roaList">
<div class="roa-item"><span class="roa-name">Narayanan Sunil</span><div class="roa-track"><div class="roa-fill" style="width:82%;background:var(--green-mid)"></div><div class="roa-avg-marker" style="left:67%"></div></div><span class="roa-val" style="color:var(--green-light)">0.386%</span></div>
<div class="roa-item"><span class="roa-name">Ali Salman</span><div class="roa-track"><div class="roa-fill" style="width:76%;background:var(--green-mid)"></div><div class="roa-avg-marker" style="left:67%"></div></div><span class="roa-val" style="color:var(--green-light)">0.358%</span></div>
<div class="roa-item"><span class="roa-name">Ahmed Ishtiaq</span><div class="roa-track"><div class="roa-fill" style="width:70%;background:var(--green-mid)"></div><div class="roa-avg-marker" style="left:67%"></div></div><span class="roa-val" style="color:var(--green-light)">0.330%</span></div>
<div class="roa-item"><span class="roa-name">Gorana Gadafi</span><div class="roa-track"><div class="roa-fill" style="width:67%;background:var(--amber)"></div><div class="roa-avg-marker" style="left:67%"></div></div><span class="roa-val" style="color:var(--amber)">0.316%</span></div>
<div class="roa-item"><span class="roa-name">Choudhry Tariq</span><div class="roa-track"><div class="roa-fill" style="width:60%;background:#e67e22"></div><div class="roa-avg-marker" style="left:67%"></div></div><span class="roa-val">0.282%</span></div>
<div class="roa-item"><span class="roa-name">Akhund Haseeb</span><div class="roa-track"><div class="roa-fill" style="width:52%;background:#e67e22"></div><div class="roa-avg-marker" style="left:67%"></div></div><span class="roa-val">0.245%</span></div>
<div class="roa-item"><span class="roa-name">Gulati Alok</span><div class="roa-track"><div class="roa-fill" style="width:44%;background:var(--red)"></div><div class="roa-avg-marker" style="left:67%"></div></div><span class="roa-val" style="color:var(--red-light)">0.207%</span></div>
<div class="roa-item"><span class="roa-name">Yunus Malik Imran</span><div class="roa-track"><div class="roa-fill" style="width:38%;background:var(--red)"></div><div class="roa-avg-marker" style="left:67%"></div></div><span class="roa-val" style="color:var(--red-light)">0.179%</span></div>
<div class="roa-item"><span class="roa-name">Kohli Aneeta</span><div class="roa-track"><div class="roa-fill" style="width:32%;background:var(--red)"></div><div class="roa-avg-marker" style="left:67%"></div></div><span class="roa-val" style="color:var(--red-light)">0.151%</span></div>
<div class="roa-item"><span class="roa-name">Kievits Maurice</span><div class="roa-track"><div class="roa-fill" style="width:24%;background:var(--red)"></div><div class="roa-avg-marker" style="left:67%"></div></div><span class="roa-val" style="color:var(--red-light)">0.113%</span></div>
<div class="roa-item"><span class="roa-name">Memon Ibad Z</span><div class="roa-track"><div class="roa-fill" style="width:18%;background:var(--red)"></div><div class="roa-avg-marker" style="left:67%"></div></div><span class="roa-val" style="color:var(--red-light)">0.085%</span></div>
<div class="roa-item"><span class="roa-name">Waffa Wissam</span><div class="roa-track"><div class="roa-fill" style="width:0%;background:var(--red)"></div><div class="roa-avg-marker" style="left:67%"></div></div><span class="roa-val" style="color:var(--text-3)">—</span></div>
</div>
</div>

<!-- NTB -->
<div class="card">
<div class="card-title">NTB Clients · New to Bank (Cumulative)</div>
<div style="display:grid;grid-template-columns:1fr 1fr;gap:8px;margin-bottom:14px">
<div class="aum-sn" style="text-align:center">
<div class="aum-sn-label">Total NTB</div>
<div style="font-family:'IBM Plex Mono',monospace;font-size:26px;font-weight:300;color:var(--green-light)">47</div>
</div>
<div class="aum-sn" style="text-align:center">
<div class="aum-sn-label">Total Funding</div>
<div style="font-family:'IBM Plex Mono',monospace;font-size:18px;font-weight:300;color:var(--amber)">USD 38M</div>
</div>
</div>
<table class="data-table">
<thead><tr>
<th>Client</th>
<th>RM</th>
<th>Seg</th>
<th class="r">Funding</th>
</tr></thead>
<tbody>
<tr><td class="primary" style="font-size:10px">Karis Investment PTE</td><td style="font-size:9px">Ahmed I.</td><td><span class="seg seg-U">UHNW</span></td><td class="green-val">9.77M</td></tr>
<tr><td class="primary" style="font-size:10px">Rasul Sheheryar</td><td style="font-size:9px">Ahmed I.</td><td><span class="seg seg-H">HNW</span></td><td class="green-val">5.33M</td></tr>
<tr><td class="primary" style="font-size:10px">Ispahani Zahida</td><td style="font-size:9px">Ahmed I.</td><td><span class="seg seg-H">HNW</span></td><td class="green-val">4.70M</td></tr>
<tr><td class="primary" style="font-size:10px">Silk Road Capital AG</td><td style="font-size:9px">Ahmed I.</td><td><span class="seg seg-U">UHNW</span></td><td class="green-val">3.91M</td></tr>
<tr><td class="primary" style="font-size:10px">Anwar Naveed</td><td style="font-size:9px">Ahmed I.</td><td><span class="seg seg-A">Affluent</span></td><td class="green-val">2.82M</td></tr>
<tr><td class="primary" style="font-size:10px">Habib Med & Education</td><td style="font-size:9px">Ahmed I.</td><td><span class="seg seg-A">Affluent</span></td><td class="green-val">2.00M</td></tr>
<tr><td class="primary" style="font-size:10px">Haji Ali Reza</td><td style="font-size:9px">Ahmed I.</td><td><span class="seg seg-H">HNW</span></td><td class="green-val">1.59M</td></tr>
<tr><td class="primary" style="font-size:10px">Ali Zainab & Panju S.</td><td style="font-size:9px">Ahmed I.</td><td><span class="seg seg-H">HNW</span></td><td class="green-val">1.24M</td></tr>
<tr><td class="primary" style="font-size:10px">Raheel Ambreen</td><td style="font-size:9px">Ahmed I.</td><td><span class="seg seg-B">Below 250K</span></td><td class="amber-val">793K</td></tr>
</tbody>
</table>
<div style="margin-top:10px;padding-top:8px;border-top:1px solid var(--border);display:flex;justify-content:space-between;align-items:center">
<span style="font-size:8px;color:var(--text-3)">Showing 9 of 47 · Sorted by funding</span>
<span style="font-size:8px;color:var(--green-light);cursor:pointer;letter-spacing:1px;text-transform:uppercase">View All →</span>
</div>
</div>

</div>
</div>

</div><!-- /page -->

<!-- FOOTER -->
<div class="footer">
<p>Habib Bank AG Zürich · Relationship Manager Dashboard</p>
<p>Strictly Confidential · Internal Use Only</p>
<p>All figures in USD · As of 23 April 2026</p>
</div>

<script>
Chart.defaults.color = '#6a8090';
Chart.defaults.font.family = "'IBM Plex Sans', sans-serif";
Chart.defaults.font.size = 9;

const G = '#2d8653';
const GL = '#3dab6a';
const BL = 'rgba(41,128,185,0.65)';
const AMB = '#e8a020';
const AMB2= 'rgba(232,160,32,0.55)';
const RED = 'rgba(192,57,43,0.7)';
const BDGRID = 'rgba(255,255,255,0.04)';
const TOOLTIP = {
backgroundColor: 'rgba(20,28,34,0.97)',
borderColor: 'rgba(45,134,83,0.4)',
borderWidth: 1,
titleColor: '#3dab6a',
bodyColor: '#a0b0bc',
padding: 10,
};

// ── TCF CHART ──────────────────────────────────────────
new Chart(document.getElementById('tcfChart').getContext('2d'), {
type: 'bar',
data: {
labels: ['Dec-25','Jan-26','Feb-26','Mar-26','Apr-26'],
datasets: [
{ label:'2026 Actual', data:[null,665,681,698,712], backgroundColor:G, borderRadius:1, order:1 },
{ label:'2025 Actual', data:[730,641,652,668,null], backgroundColor:BL, borderRadius:1, order:2 },
{ label:'Dec-25 Baseline', data:[730,null,null,null,null], backgroundColor:AMB2, borderRadius:1, order:3 },
{ label:'Budget 2026', data:[null,700,710,720,730], type:'line',
borderColor:AMB, borderDash:[5,4], borderWidth:1.5,
pointBackgroundColor:AMB, pointRadius:3, pointHoverRadius:5,
fill:false, tension:0.3, order:0 }
]
},
options: {
responsive:true, maintainAspectRatio:false,
interaction:{ mode:'index' },
plugins:{ legend:{ display:false }, tooltip:{ ...TOOLTIP, callbacks:{ label: c=>` ${c.dataset.label}: USD ${c.parsed.y}M` } } },
scales:{
x:{ grid:{ color:BDGRID }, ticks:{ color:'#6a8090' }, border:{ color:'rgba(255,255,255,0.06)' } },
y:{ grid:{ color:BDGRID }, border:{ color:'rgba(255,255,255,0.06)' },
ticks:{ color:'#6a8090', callback: v=>`${v}M` } }
}
}
});

// ── NNM CHART ──────────────────────────────────────────
new Chart(document.getElementById('nnmChart').getContext('2d'), {
type:'bar',
data:{
labels:['Jan','Feb','Mar','Apr'],
datasets:[
{ label:'2026', data:[88.4,72.1,91.6,60.3], backgroundColor:G, borderRadius:1 },
{ label:'2025', data:[74.2,68.9,85.1,66.0], backgroundColor:BL, borderRadius:1 }
]
},
options:{
responsive:true, maintainAspectRatio:false,
plugins:{ legend:{ display:true, labels:{ color:'#6a8090', font:{size:8}, boxWidth:8, boxHeight:8 } },
tooltip:{ ...TOOLTIP, callbacks:{ label:c=>` ${c.dataset.label}: USD ${c.parsed.y}M` } } },
scales:{
x:{ grid:{ color:BDGRID }, border:{ color:'rgba(255,255,255,0.06)' }, ticks:{ color:'#6a8090' } },
y:{ grid:{ color:BDGRID }, border:{ color:'rgba(255,255,255,0.06)' },
ticks:{ color:'#6a8090', callback:v=>`${v}M` } }
}
}
});

// ── SEGMENT DONUT ──────────────────────────────────────
new Chart(document.getElementById('segChart').getContext('2d'), {
type:'doughnut',
data:{
labels:['UHNW','HNW','Affluent','Below 250K'],
datasets:[{ data:[187,423,614,277],
backgroundColor:['#e8a020','#2980b9','#2d8653','#3d4f5e'],
borderColor:'#1a242c', borderWidth:3, hoverOffset:5 }]
},
options:{
responsive:false, cutout:'74%',
plugins:{ legend:{ display:false },
tooltip:{ ...TOOLTIP, callbacks:{ label:c=>` ${c.label}: ${c.parsed}` } } }
}
});

// ── ACTIVE TREND ──────────────────────────────────────
new Chart(document.getElementById('activeChart').getContext('2d'), {
type:'line',
data:{
labels:['Dec-25','Jan-26','Feb-26','Mar-26','Apr-26'],
datasets:[
{ label:'Active', data:[1312,1320,1328,1335,1341],
borderColor:GL, backgroundColor:'rgba(45,134,83,0.07)',
borderWidth:1.5, fill:true, tension:0.4, pointRadius:2 },
{ label:'Inactive', data:[178,172,168,163,160],
borderColor:'rgba(192,57,43,0.5)', backgroundColor:'rgba(192,57,43,0.04)',
borderWidth:1.5, fill:true, tension:0.4, pointRadius:2 }
]
},
options:{
responsive:true, maintainAspectRatio:false,
plugins:{ legend:{ display:true, labels:{ color:'#6a8090', font:{size:8}, boxWidth:8, boxHeight:8 } },
tooltip:{ ...TOOLTIP } },
scales:{
x:{ grid:{ display:false }, ticks:{ font:{size:7}, color:'#6a8090' }, border:{ display:false } },
y:{ display:false }
}
}
});

// ── AuM DONUT ─────────────────────────────────────────
new Chart(document.getElementById('aumDonutChart').getContext('2d'), {
type:'doughnut',
data:{
labels:['Top 10 Clients','Other Clients'],
datasets:[{ data:[68.3,31.7],
backgroundColor:['#e8a020','rgba(255,255,255,0.06)'],
borderColor:['#e8a020','rgba(255,255,255,0.08)'],
borderWidth:2, hoverOffset:4 }]
},
options:{
responsive:true, maintainAspectRatio:false, cutout:'70%',
plugins:{ legend:{ display:true, position:'right',
labels:{ color:'#6a8090', font:{size:8}, boxWidth:8, boxHeight:8, padding:12 } },
tooltip:{ ...TOOLTIP, callbacks:{ label:c=>` ${c.label}: ${c.parsed}%` } } }
}
});

function applyFilter() { /* Power BI handles this — illustrative */ }
</script>
</body>
</html>
