<!DOCTYPE html>
<html lang="es">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Monitor Agropecuario · Entre Ríos</title>
<meta name="description" content="Monitor mensual de precios, costos y relaciones insumo-producto del sector agropecuario de Entre Ríos.">
<script src="https://cdnjs.cloudflare.com/ajax/libs/Chart.js/4.4.1/chart.umd.min.js"></script>
<style>
@import url('https://fonts.googleapis.com/css2?family=Inter:wght@300;400;500;600;700&family=Syne:wght@700;800&display=swap');

:root {
  --navy:   #0a0f1e;
  --navy2:  #0f172a;
  --navy3:  #1e293b;
  --blue1:  #1d4ed8;
  --blue2:  #2563eb;
  --blue3:  #3b82f6;
  --blue4:  #60a5fa;
  --blue5:  #93c5fd;
  --blue6:  #bfdbfe;
  --blue7:  #dbeafe;
  --blue8:  #eff6ff;
  --cyan:   #06b6d4;
  --teal:   #0d9488;
  --red:    #ef4444;
  --red2:   #fef2f2;
  --amber:  #f59e0b;
  --amber2: #fffbeb;
  --green:  #10b981;
  --green2: #ecfdf5;
  --white:  #ffffff;
  --gray1:  #f8fafc;
  --gray2:  #f1f5f9;
  --gray3:  #e2e8f0;
  --gray4:  #cbd5e1;
  --gray5:  #94a3b8;
  --gray6:  #64748b;
  --gray7:  #475569;
  --ink:    #0f172a;
  --ink2:   #1e293b;
  --ink3:   #334155;
  --ink4:   #64748b;
}

*,*::before,*::after{margin:0;padding:0;box-sizing:border-box}
html{scroll-behavior:smooth;font-size:14px}
body{font-family:'Inter',sans-serif;background:var(--gray1);color:var(--ink);line-height:1.6;-webkit-font-smoothing:antialiased}

/* ── SCROLLBAR ── */
::-webkit-scrollbar{width:6px;height:6px}
::-webkit-scrollbar-track{background:var(--gray2)}
::-webkit-scrollbar-thumb{background:var(--blue3);border-radius:3px}

/* ── HEADER ── */
header{
  position:sticky;top:0;z-index:100;
  background:rgba(10,15,30,.92);
  backdrop-filter:blur(20px);
  -webkit-backdrop-filter:blur(20px);
  border-bottom:1px solid rgba(59,130,246,.2);
}
.hdr{display:flex;align-items:center;justify-content:space-between;padding:0 24px;height:52px;max-width:1200px;margin:0 auto}
.logo{font-family:'Syne',sans-serif;font-size:15px;font-weight:800;color:#fff;letter-spacing:-.01em;white-space:nowrap}
.logo span{color:var(--blue4)}
.logo small{font-family:'Inter',sans-serif;font-size:10px;font-weight:400;color:rgba(255,255,255,.4);margin-left:8px;letter-spacing:.04em;text-transform:uppercase}
nav{display:flex;gap:2px;overflow-x:auto;scrollbar-width:none}
nav::-webkit-scrollbar{display:none}
nav a{color:rgba(255,255,255,.5);text-decoration:none;font-size:11.5px;font-weight:500;padding:5px 10px;border-radius:6px;white-space:nowrap;transition:.18s;letter-spacing:.01em}
nav a:hover{background:rgba(59,130,246,.15);color:var(--blue4)}

/* ── HERO ── */
.hero{
  background:var(--navy);
  background-image:radial-gradient(ellipse 80% 50% at 50% -10%, rgba(29,78,216,.4) 0%, transparent 70%),
                   radial-gradient(ellipse 40% 30% at 80% 60%, rgba(6,182,212,.15) 0%, transparent 60%);
  color:#fff;padding:56px 24px 48px;
  position:relative;overflow:hidden;
}
.hero::after{
  content:'';position:absolute;bottom:0;left:0;right:0;height:1px;
  background:linear-gradient(90deg,transparent,rgba(59,130,246,.4),transparent);
}
.hero-inner{max-width:1200px;margin:0 auto;display:grid;grid-template-columns:1fr 280px;gap:40px;align-items:center}
.hero-tag{display:inline-flex;align-items:center;gap:6px;background:rgba(59,130,246,.12);border:1px solid rgba(59,130,246,.3);color:var(--blue4);font-size:10.5px;font-weight:600;padding:3px 10px;border-radius:20px;margin-bottom:14px;letter-spacing:.06em;text-transform:uppercase}
.dot-live{width:6px;height:6px;border-radius:50%;background:#10b981;animation:blink 2s infinite}
@keyframes blink{0%,100%{opacity:1}50%{opacity:.3}}
.hero h1{font-family:'Syne',sans-serif;font-size:clamp(28px,4vw,42px);font-weight:800;line-height:1.1;letter-spacing:-.03em;margin-bottom:10px}
.hero h1 em{font-style:normal;color:var(--blue4)}
.hero-desc{font-size:13px;color:rgba(255,255,255,.55);max-width:500px;line-height:1.7}
.hero-stats{display:flex;flex-direction:column;gap:10px}
.hstat{
  background:rgba(255,255,255,.04);
  border:1px solid rgba(255,255,255,.08);
  border-radius:12px;padding:12px 16px;
  display:flex;flex-direction:column;gap:2px;
  transition:.2s;
}
.hstat:hover{background:rgba(59,130,246,.08);border-color:rgba(59,130,246,.25)}
.hstat-val{font-family:'Syne',sans-serif;font-size:22px;font-weight:700;color:var(--blue4)}
.hstat-lbl{font-size:10px;color:rgba(255,255,255,.4);letter-spacing:.05em;text-transform:uppercase}

/* ── TICKER ── */
.ticker{background:var(--navy2);border-bottom:1px solid rgba(255,255,255,.06);overflow-x:auto;scrollbar-width:none}
.ticker::-webkit-scrollbar{display:none}
.ticker-inner{display:flex;max-width:1200px;margin:0 auto;padding:0 24px}
.tick{display:flex;align-items:center;gap:8px;padding:10px 18px;border-right:1px solid rgba(255,255,255,.06);flex-shrink:0;font-size:11.5px;color:rgba(255,255,255,.5)}
.tick:first-child{padding-left:0}
.tick strong{color:#fff;font-weight:600}
.up{color:#10b981}.dn{color:#ef4444}.fl{color:#94a3b8}

/* ── SECTIONS ── */
section{padding:40px 24px;border-bottom:1px solid var(--gray3)}
section:nth-child(even){background:var(--white)}
.wrap{max-width:1200px;margin:0 auto}

.sec-hdr{display:flex;align-items:center;gap:14px;margin-bottom:24px}
.sec-chip{background:var(--blue7);color:var(--blue1);font-size:10px;font-weight:700;letter-spacing:.08em;text-transform:uppercase;padding:3px 10px;border-radius:20px;white-space:nowrap}
.sec-ttl{font-family:'Syne',sans-serif;font-size:20px;font-weight:800;letter-spacing:-.02em}
.sec-sub{font-size:11.5px;color:var(--gray6);margin-top:2px}

/* ── GRID ── */
.g2{display:grid;grid-template-columns:1fr 1fr;gap:16px}
.g3{display:grid;grid-template-columns:repeat(3,1fr);gap:14px}
.g4{display:grid;grid-template-columns:repeat(4,1fr);gap:12px}

/* ── CARD ── */
.card{background:var(--white);border:1px solid var(--gray3);border-radius:14px;padding:18px;transition:.2s}
.card:hover{box-shadow:0 4px 24px rgba(59,130,246,.08);border-color:var(--blue6)}
.card-ttl{font-size:11px;font-weight:600;text-transform:uppercase;letter-spacing:.07em;color:var(--gray5);margin-bottom:12px}
section:nth-child(even) .card{background:var(--gray1)}

/* ── METRIC ── */
.metric{background:var(--gray1);border:1px solid var(--gray3);border-radius:10px;padding:14px;text-align:center}
.metric.blue{background:var(--blue8);border-color:var(--blue6)}
.metric.green{background:var(--green2);border-color:#a7f3d0}
.metric.red{background:var(--red2);border-color:#fecaca}
.metric.amber{background:var(--amber2);border-color:#fde68a}
.metric-lbl{font-size:10px;font-weight:600;text-transform:uppercase;letter-spacing:.06em;color:var(--gray5);margin-bottom:4px}
.metric-val{font-family:'Syne',sans-serif;font-size:24px;font-weight:700;line-height:1.1;color:var(--ink)}
.metric.blue .metric-val{color:var(--blue1)}
.metric.green .metric-val{color:#065f46}
.metric.red .metric-val{color:#991b1b}
.metric-sub{font-size:11px;color:var(--gray5);margin-top:3px}
.metric.green .metric-sub{color:#10b981}
.metric.red .metric-sub{color:#ef4444}

/* ── TABLE ── */
.tbl{width:100%;border-collapse:collapse;font-size:12.5px}
.tbl th{background:var(--navy2);color:rgba(255,255,255,.8);padding:9px 12px;text-align:left;font-size:10.5px;font-weight:600;text-transform:uppercase;letter-spacing:.06em}
.tbl th:first-child{border-radius:8px 0 0 0}
.tbl th:last-child{border-radius:0 8px 0 0}
.tbl td{padding:9px 12px;border-bottom:1px solid var(--gray3);vertical-align:middle}
.tbl tr:nth-child(even) td{background:var(--gray1)}
.tbl tr:hover td{background:var(--blue8);transition:.12s}
.tbl .total td{background:var(--blue8)!important;font-weight:700;color:var(--blue1)}
.big{font-family:'Syne',sans-serif;font-size:15px;font-weight:700}

/* ── BADGE ── */
.bdg{display:inline-flex;align-items:center;gap:3px;font-size:11px;font-weight:600;padding:2px 8px;border-radius:20px}
.bdg.up{background:#d1fae5;color:#065f46}
.bdg.dn{background:#fee2e2;color:#991b1b}
.bdg.fl{background:var(--gray2);color:var(--gray6)}

/* ── INSIGHT BOX ── */
.insight{
  border-radius:10px;padding:13px 16px;margin-top:14px;
  font-size:12px;line-height:1.7;color:var(--ink3);
  border-left:3px solid;
}
.insight.blue{background:var(--blue8);border-color:var(--blue3)}
.insight.green{background:var(--green2);border-color:var(--green)}
.insight.red{background:var(--red2);border-color:var(--red)}
.insight.amber{background:var(--amber2);border-color:var(--amber)}
.insight strong{font-weight:600}
.insight.blue strong{color:var(--blue1)}
.insight.green strong{color:#065f46}
.insight.red strong{color:#991b1b}
.insight.amber strong{color:#92400e}

/* ── IP GRID ── */
.ip-grid{display:grid;grid-template-columns:repeat(auto-fill,minmax(240px,1fr));gap:12px}
.ip-card{
  background:var(--white);border:1px solid var(--gray3);border-radius:12px;
  padding:16px;transition:.2s;position:relative;overflow:hidden;
}
.ip-card::before{
  content:'';position:absolute;top:0;left:0;right:0;height:3px;
  background:linear-gradient(90deg,var(--blue3),var(--cyan));
}
.ip-card.bad::before{background:linear-gradient(90deg,var(--red),#f97316)}
.ip-card.ok::before{background:linear-gradient(90deg,var(--green),var(--teal))}
.ip-card:hover{box-shadow:0 8px 32px rgba(59,130,246,.12);border-color:var(--blue5);transform:translateY(-1px)}
.ip-icon{font-size:22px;margin-bottom:8px}
.ip-name{font-size:11px;font-weight:600;color:var(--gray6);text-transform:uppercase;letter-spacing:.06em;margin-bottom:6px}
.ip-val{font-family:'Syne',sans-serif;font-size:32px;font-weight:800;color:var(--ink);line-height:1}
.ip-unit{font-size:11px;color:var(--gray5);margin-top:2px}
.ip-change{display:inline-flex;align-items:center;gap:3px;font-size:11.5px;font-weight:600;margin-top:8px;padding:3px 8px;border-radius:20px}
.ip-change.bad{background:#fee2e2;color:#991b1b}
.ip-change.ok{background:#d1fae5;color:#065f46}
.ip-change.neutral{background:var(--gray2);color:var(--gray6)}
.ip-interp{font-size:11px;color:var(--gray5);margin-top:6px;line-height:1.5}

/* ── TOGGLE ── */
.toggle-group{display:flex;gap:4px;background:var(--gray2);border-radius:8px;padding:3px;margin-bottom:16px;width:fit-content}
.toggle-btn{padding:5px 14px;border-radius:6px;border:none;font-size:12px;font-weight:600;cursor:pointer;transition:.15s;background:transparent;color:var(--gray6)}
.toggle-btn.active{background:var(--white);color:var(--blue1);box-shadow:0 1px 4px rgba(0,0,0,.1)}

/* ── CHART ── */
.ch{position:relative}
.ch.h200{height:200px}.ch.h220{height:220px}.ch.h240{height:240px}.ch.h180{height:180px}

/* ── FUENTE ── */
.fuente{font-size:10px;color:var(--gray4);margin-top:8px;font-style:italic}
.fuente a{color:var(--blue3);text-decoration:none}

/* ── RETENCIONES ── */
.ret-grid{display:grid;grid-template-columns:repeat(auto-fill,minmax(200px,1fr));gap:10px}
.ret-card{background:var(--white);border:1px solid var(--gray3);border-radius:10px;padding:14px}
.ret-prod{font-size:13px;font-weight:600;margin-bottom:6px}
.ret-pct{font-family:'Syne',sans-serif;font-size:28px;font-weight:800;line-height:1}
.ret-card.zero .ret-pct{color:var(--green)}
.ret-card.low .ret-pct{color:var(--blue2)}
.ret-card.mid .ret-pct{color:var(--amber)}
.ret-card.high .ret-pct{color:var(--red)}
.ret-norm{font-size:10px;color:var(--gray5);margin-top:4px}
.ret-bar{height:4px;border-radius:2px;margin-top:8px;background:var(--gray3)}
.ret-bar-fill{height:100%;border-radius:2px;background:var(--blue3);transition:.4s}
.ret-card.zero .ret-bar-fill{background:var(--green)}
.ret-card.low .ret-bar-fill{background:var(--blue3)}
.ret-card.mid .ret-bar-fill{background:var(--amber)}
.ret-card.high .ret-bar-fill{background:var(--red)}

/* ── FOOTER ── */
footer{background:var(--navy);color:rgba(255,255,255,.35);padding:24px;text-align:center;font-size:11px;line-height:1.8}
footer strong{color:rgba(255,255,255,.65)}

/* ── RESPONSIVE ── */
@media(max-width:760px){
  .g2,.g3,.g4{grid-template-columns:1fr}
  .hero-inner{grid-template-columns:1fr}
  .hero-stats{flex-direction:row;flex-wrap:wrap}
  .hstat{flex:1;min-width:120px}
  .ip-grid{grid-template-columns:1fr 1fr}
}
@media(max-width:480px){
  .ip-grid{grid-template-columns:1fr}
}
</style>
</head>
<body>

<!-- HEADER -->
<header>
  <div class="hdr">
    <div class="logo">Monitor Agropecuario <span>Entre Ríos</span> <small>N°1 · Abr 2026</small></div>
    <nav>
      <a href="#granos">Granos</a>
      <a href="#costos">Costos</a>
      <a href="#ip">Insumo-Producto</a>
      <a href="#bovinos">Bovinos</a>
      <a href="#porcinos">Porcinos</a>
      <a href="#aviar">Avícola</a>
      <a href="#lacteo">Lácteo</a>
      <a href="#dex">DEX</a>
      <a href="#metod">Metodología</a>
    </nav>
  </div>
</header>

<!-- HERO -->
<div class="hero">
  <div class="hero-inner wrap">
    <div>
      <div class="hero-tag"><span class="dot-live"></span>Campaña 2024/25 · Edición Nº 1</div>
      <h1>Monitor<br>Agropecuario<br><em>Entre Ríos</em></h1>
      <p class="hero-desc">Análisis mensual de producción, precios, costos y relaciones insumo-producto de los principales complejos productivos provinciales. Datos de SAGyP, BOLSACER, OCLA, SIO Carnes, Rosgan y Sec. Energía.</p>
    </div>
    <div class="hero-stats">
      <div class="hstat"><span class="hstat-val">8,3 Mt</span><span class="hstat-lbl">Producción agrícola 24/25</span></div>
      <div class="hstat"><span class="hstat-val">USD 2.533M</span><span class="hstat-lbl">VBP FOB 2024/25</span></div>
      <div class="hstat"><span class="hstat-val" id="kpi-soja">USD 323/t</span><span class="hstat-lbl">FOB Soja actual</span></div>
      <div class="hstat"><span class="hstat-val">+5%</span><span class="hstat-lbl">Var. producción total</span></div>
    </div>
  </div>
</div>

<!-- TICKER -->
<div class="ticker">
  <div class="ticker-inner">
    <div class="tick"><strong class="up">Soja +24%</strong>&nbsp;producción</div>
    <div class="tick"><strong class="up">Arroz 32%</strong>&nbsp;del total nac.</div>
    <div class="tick"><strong class="dn">Maíz −12%</strong>&nbsp;producción</div>
    <div class="tick"><strong class="up">Trigo +26%</strong>&nbsp;proy. 25/26</div>
    <div class="tick"><strong class="up">+74%</strong>&nbsp;export. bovina USD</div>
    <div class="tick"><strong class="fl">50%</strong>&nbsp;faena aviar nac.</div>
    <div class="tick"><strong class="dn">Gasoil +41%</strong>&nbsp;interanual dic-25</div>
    <div class="tick"><strong class="up">Arroz DEX 0%</strong>&nbsp;permanente</div>
  </div>
</div>

<!-- 01 GRANOS -->
<section id="granos">
<div class="wrap">
  <div class="sec-hdr">
    <span class="sec-chip">01</span>
    <div><div class="sec-ttl">Producción y Precios de Granos</div><div class="sec-sub">Campaña 2024/25 · SAGyP · BCR · BOLSACER</div></div>
  </div>

  <div class="card" style="overflow-x:auto;margin-bottom:18px">
    <div class="card-ttl">Producción Agrícola — Campaña 2024/25</div>
    <table class="tbl">
      <thead><tr><th>Cultivo</th><th>Producción (t)</th><th>Var. a/a</th><th>Part. nac.</th><th>Rinde (kg/ha)</th><th>Var. rinde</th><th>VBP (USD M)</th></tr></thead>
      <tbody>
        <tr><td>🍚 Arroz — var. Uru5</td><td>524.948</td><td><span class="bdg up">+7%</span></td><td>32%</td><td class="big">7.786</td><td><span class="bdg up">+15%</span></td><td>215</td></tr>
        <tr><td>🌽 Maíz</td><td>2.254.760</td><td><span class="bdg dn">−12%</span></td><td>4%</td><td class="big">6.603</td><td><span class="bdg up">+15%</span></td><td>498</td></tr>
        <tr><td>🌾 Trigo</td><td>2.217.630</td><td><span class="bdg fl">+0,3%</span></td><td>12%</td><td class="big">3.207</td><td><span class="bdg dn">−5%</span></td><td>514</td></tr>
        <tr><td>🌱 Soja</td><td>3.372.215</td><td><span class="bdg up">+24%</span></td><td>7%</td><td class="big">2.695</td><td><span class="bdg up">+8%</span></td><td>1.305</td></tr>
        <tr class="total"><td>TOTAL</td><td>8.369.553</td><td><span class="bdg up">+5%</span></td><td>—</td><td>—</td><td>—</td><td>2.533</td></tr>
      </tbody>
    </table>
    <div class="fuente">Fuente: SAGyP — Estimaciones Agrícolas. VBP = Producción × FOB 2025 (USD).</div>
    <div class="insight blue"><strong>Lectura:</strong> Campaña de comportamientos divergentes. La soja lidera con +24% en volumen y +8% en rinde, consolidándose como el cultivo de mayor VBP provincial (52% del total). El arroz Uru5 logra rinde récord de 7.786 kg/ha (+15%) y mantiene la ventaja del DEX 0% permanente. El maíz retrocede en área pero mejora su rinde, lo que revela una decisión de reasignación de tierras más que un problema productivo. El trigo proyecta récord para 25/26 (+26%).</div>
  </div>

  <div class="g2">
    <div class="card">
      <div class="card-ttl">Evolución FOB USD/t — 2018–2025 <span id="fob-status" style="font-weight:400;color:var(--gray4)">(datos base)</span></div>
      <div class="ch h240"><canvas id="chartFOB"></canvas></div>
      <div class="fuente">Soja (mayo), Trigo/Maíz (enero): BCR. Arroz Uru5 (marzo): SAGyP. Segundo miércoles hábil. <span id="fob-date"></span></div>
      <div class="insight blue"><strong>2025 en perspectiva:</strong> El contexto es de precios moderados. La soja recupera +16% interanual gracias a stocks globales ajustados. Trigo (−10%) y maíz (−3%) acusan cosechas récord en el hemisferio norte. El arroz Uru5 rebota +13% desde el piso de 2024, aunque lejos del máximo de USD 610/t de 2023.</div>
    </div>
    <div class="card">
      <div class="card-ttl">Variación interanual FOB — 2025 vs 2024</div>
      <div class="g2" style="gap:10px;margin-bottom:14px">
        <div style="background:var(--green2);border-radius:10px;padding:16px;text-align:center;border:1px solid #a7f3d0">
          <div style="font-size:10px;text-transform:uppercase;letter-spacing:.06em;color:#065f46;font-weight:600">🌱 Soja</div>
          <div style="font-family:'Syne',sans-serif;font-size:34px;font-weight:800;color:#065f46">+16%</div>
          <div style="font-size:11px;color:#10b981">USD 323/t · mar-26</div>
        </div>
        <div style="background:var(--red2);border-radius:10px;padding:16px;text-align:center;border:1px solid #fecaca">
          <div style="font-size:10px;text-transform:uppercase;letter-spacing:.06em;color:#991b1b;font-weight:600">🌾 Trigo</div>
          <div style="font-family:'Syne',sans-serif;font-size:34px;font-weight:800;color:#991b1b">−10%</div>
          <div style="font-size:11px;color:#ef4444">USD 218/t · ene-26</div>
        </div>
        <div style="background:var(--red2);border-radius:10px;padding:16px;text-align:center;border:1px solid #fecaca">
          <div style="font-size:10px;text-transform:uppercase;letter-spacing:.06em;color:#991b1b;font-weight:600">🌽 Maíz</div>
          <div style="font-family:'Syne',sans-serif;font-size:34px;font-weight:800;color:#991b1b">−3%</div>
          <div style="font-size:11px;color:#ef4444">USD 185/t · ene-26</div>
        </div>
        <div style="background:var(--green2);border-radius:10px;padding:16px;text-align:center;border:1px solid #a7f3d0">
          <div style="font-size:10px;text-transform:uppercase;letter-spacing:.06em;color:#065f46;font-weight:600">🍚 Arroz Uru5</div>
          <div style="font-family:'Syne',sans-serif;font-size:34px;font-weight:800;color:#065f46">+13%</div>
          <div style="font-size:11px;color:#10b981">~USD 420/t · mar-26</div>
        </div>
      </div>
      <div class="insight amber"><strong>Ventaja ER — Arroz:</strong> El DEX 0% permanente sobre el arroz (D°38/2025) implica que el productor entreriano recibe el precio FAS íntegro, mientras que soja (24%), maíz (8,5%) y trigo (7,5%) ceden parte al Estado. Esta asimetría convierte al arroz en el cultivo de mejor ecuación fiscal en la provincia.</div>
    </div>
  </div>
</div>
</section>

<!-- 02 COSTOS -->
<section id="costos">
<div class="wrap">
  <div class="sec-hdr">
    <span class="sec-chip">02</span>
    <div><div class="sec-ttl">Estructura de Costos por Hectárea</div><div class="sec-sub">Campaña 2024/25 · BOLSACER · Ciudad de Paraná</div></div>
  </div>
  <div class="g3" style="margin-bottom:16px">
    <div class="card">
      <div class="card-ttl">🌾 Trigo — composición del costo</div>
      <div class="ch h180"><canvas id="cTrigo"></canvas></div>
      <div style="font-size:11.5px;color:var(--ink3);margin-top:8px">Insumos <strong>44%</strong> · Labores <strong>12%</strong> · Flete <strong>18%</strong></div>
    </div>
    <div class="card">
      <div class="card-ttl">🌽 Maíz de primera — composición del costo</div>
      <div class="ch h180"><canvas id="cMaiz"></canvas></div>
      <div style="font-size:11.5px;color:var(--ink3);margin-top:8px">Labores <strong>43%</strong> · Insumos <strong>35%</strong> · Flete <strong>8%</strong></div>
    </div>
    <div class="card">
      <div class="card-ttl">🌱 Soja de primera — composición del costo</div>
      <div class="ch h180"><canvas id="cSoja"></canvas></div>
      <div style="font-size:11.5px;color:var(--ink3);margin-top:8px">Insumos <strong>35%</strong> · Labores <strong>12%</strong> · Flete <strong>22%</strong></div>
    </div>
  </div>
  <div class="g2">
    <div class="card">
      <div class="card-ttl">Costo laboral — Empleado Rural (CNTA)</div>
      <div style="display:flex;gap:12px;flex-wrap:wrap">
        <div class="metric red" style="flex:1"><div class="metric-lbl">Salario dic-25</div><div class="metric-val">$978.908</div><div class="metric-sub">+35% interanual</div></div>
        <div class="metric" style="flex:1"><div class="metric-lbl">Peso en maíz</div><div class="metric-val">43%</div><div class="metric-sub">del costo total/ha</div></div>
        <div class="metric" style="flex:1"><div class="metric-lbl">Peso en soja/trigo</div><div class="metric-val">12%</div><div class="metric-sub">del costo total/ha</div></div>
      </div>
    </div>
    <div class="insight blue" style="margin:0;align-self:stretch;display:flex;flex-direction:column;justify-content:center">
      <strong>Diferencias clave entre cultivos:</strong><br><br>
      El <strong>maíz</strong> es el más vulnerable al costo laboral (43% del total), lo que explica la caída de área sembrada ante la suba salarial del 35%. La <strong>soja y el trigo</strong> están dominados por insumos dolarizados — más estables con el tipo de cambio en bandas. El <strong>flete</strong> es el diferencial crítico de ER: representa hasta el 22% del costo en soja, y el diferencial con Rosario (~USD 22/t Paraná, ~USD 28/t Concordia) define la competitividad regional.
    </div>
  </div>
</div>
</section>

<!-- 03 INSUMO-PRODUCTO -->
<section id="ip">
<div class="wrap">
  <div class="sec-hdr">
    <span class="sec-chip">03</span>
    <div>
      <div class="sec-ttl">Relaciones Insumo-Producto</div>
      <div class="sec-sub">Paraná, Entre Ríos · Diciembre 2025 · <strong>Calculado con pizarra BOLSACER/SIBER (precio recibido en ER)</strong> · Más kg = peor para el productor</div>
    </div>
  </div>

  <!-- Nota metodológica prominente -->
  <div style="background:var(--blue8);border:1px solid var(--blue6);border-radius:10px;padding:13px 16px;margin-bottom:20px;font-size:12px;color:var(--ink3);line-height:1.7">
    <strong style="color:var(--blue1)">📐 Base de cálculo:</strong> Las relaciones se calculan con la <strong>pizarra BOLSACER/SIBER</strong>, que refleja el precio efectivamente recibido por el productor en Entre Ríos (precio Rosario BCR menos flete ~USD 22/t Paraná, ~USD 28/t Concordia). Este precio es <strong>menor</strong> al de la pizarra BCR, por lo que las relaciones resultan <strong>peores</strong> que si se usara el precio Rosario. El gasoil corresponde al precio en Paraná (Secretaría de Energía, Res. 314/2016). Para arroz, se utiliza directamente la pizarra Uru5 del SIBER (no tiene referencia BCR).
    <br><span style="color:var(--blue3);font-size:10.5px">Fuentes: BOLSACER — Informe Semanal SIBER · SIO Granos (SAGyP) · Secretaría de Energía. Datos estimados dic-2025; se actualizan con los informes mensuales BOLSACER.</span>
  </div>

  <!-- Toggle G2/G3 -->
  <div class="toggle-group" style="margin-bottom:16px">
    <button class="toggle-btn active" onclick="showG('g2',this)">Gasoil G2 (común)</button>
    <button class="toggle-btn" onclick="showG('g3',this)">Gasoil G3 (premium/agro)</button>
  </div>

  <!-- G2 -->
  <div id="panel-g2">
    <div class="ip-grid" style="margin-bottom:20px">
      <div class="ip-card bad">
        <div class="ip-icon">⛽🌱</div>
        <div class="ip-name">G2 / Soja · pizarra ER</div>
        <div class="ip-val">4,7</div>
        <div class="ip-unit">kg soja por litro G2 — Paraná</div>
        <div class="ip-change bad">▲ +52% vs dic-24 (era 3,1)</div>
        <div class="ip-interp">Pizarra ER dic-25 ~$420.000/t · G2 Paraná ~$1.967/L. Con pizarra BCR Rosario daría 4,4 kg (el flete ER agrega ~0,3 kg de diferencia).</div>
      </div>
      <div class="ip-card bad">
        <div class="ip-icon">⛽🍚</div>
        <div class="ip-name">G2 / Arroz Uru5 · pizarra ER</div>
        <div class="ip-val">10,6</div>
        <div class="ip-unit">kg arroz por litro G2 — Paraná</div>
        <div class="ip-change bad">▲ +221% vs dic-24 (era 3,3)</div>
        <div class="ip-interp">⚠ El dato más crítico del monitor. Pizarra SIBER dic-25 ~$185.000/t (caída del ~50% interanual). Combinado con gasoil +72%, el arroz pasó de 3,3 a 10,6 kg por litro en un año.</div>
      </div>
      <div class="ip-card bad">
        <div class="ip-icon">⛽🌽</div>
        <div class="ip-name">G2 / Maíz · pizarra ER</div>
        <div class="ip-val">8,4</div>
        <div class="ip-unit">kg maíz por litro G2 — Paraná</div>
        <div class="ip-change bad">▲ +58% vs dic-24 (era 5,3)</div>
        <div class="ip-interp">Pizarra BOLSACER dic-25 ~$235.000/t. Nivel históricamente elevado — en 2020 era ~2,8 kg/L. El maíz dejó de ser el grano "más eficiente" frente al gasoil.</div>
      </div>
      <div class="ip-card bad">
        <div class="ip-icon">⛽🌾</div>
        <div class="ip-name">G2 / Trigo · pizarra ER</div>
        <div class="ip-val">8,4</div>
        <div class="ip-unit">kg trigo por litro G2 — Paraná</div>
        <div class="ip-change bad">▲ +75% vs dic-24 (era 4,8)</div>
        <div class="ip-interp">Pizarra BOLSACER dic-25 ~$235.000/t. El trigo cayó −10% en FOB y la pizarra ER no logró compensar la suba del gasoil.</div>
      </div>
    </div>
  </div>

  <!-- G3 -->
  <div id="panel-g3" style="display:none">
    <div class="ip-grid" style="margin-bottom:20px">
      <div class="ip-card bad">
        <div class="ip-icon">⛽🌱</div>
        <div class="ip-name">G3 / Soja · pizarra ER</div>
        <div class="ip-val">5,5</div>
        <div class="ip-unit">kg soja por litro G3 — Paraná</div>
        <div class="ip-change bad">▲ +49% vs dic-24 (era 3,7)</div>
        <div class="ip-interp">G3 Paraná dic-25 ~$2.310/L. Es el combustible de tractores y cosechadoras. Cada liter cuesta 5,5 kg de soja al precio que recibe el productor entrerriano.</div>
      </div>
      <div class="ip-card bad">
        <div class="ip-icon">⛽🍚</div>
        <div class="ip-name">G3 / Arroz Uru5 · pizarra ER</div>
        <div class="ip-val">12,5</div>
        <div class="ip-unit">kg arroz por litro G3 — Paraná</div>
        <div class="ip-change bad">▲ +229% vs dic-24 (era 3,8)</div>
        <div class="ip-interp">⚠ Crítico para el arroz: el riego por pozo profundo usa G3. Triplicar en un año la cantidad de arroz necesaria para pagar el combustible explica la contracción de área 2025/26.</div>
      </div>
      <div class="ip-card bad">
        <div class="ip-icon">⛽🌽</div>
        <div class="ip-name">G3 / Maíz · pizarra ER</div>
        <div class="ip-val">9,8</div>
        <div class="ip-unit">kg maíz por litro G3 — Paraná</div>
        <div class="ip-change bad">▲ +58% vs dic-24 (era 6,2)</div>
        <div class="ip-interp">Referencia CONINAGRO (G3/Maíz BCR): 6,5 kg/L en feb-25. El valor ER es mayor porque la pizarra local es más baja que la BCR.</div>
      </div>
      <div class="ip-card bad">
        <div class="ip-icon">⛽🌾</div>
        <div class="ip-name">G3 / Trigo · pizarra ER</div>
        <div class="ip-val">9,8</div>
        <div class="ip-unit">kg trigo por litro G3 — Paraná</div>
        <div class="ip-change bad">▲ +75% vs dic-24 (era 5,6)</div>
        <div class="ip-interp">El nivel más alto desde 2019. La perspectiva de récord de trigo 2025/26 (+26%) se explica más por condiciones climáticas favorables que por una ecuación insumo-producto positiva.</div>
      </div>
    </div>
  </div>

  <!-- Carnes y Leche / Maíz -->
  <div style="font-size:11px;font-weight:600;color:var(--gray6);text-transform:uppercase;letter-spacing:.06em;margin-bottom:12px">
    Carnes, Leche y Huevo / Maíz ER — kg de maíz BOLSACER por kg de producto (o litro/docena)
  </div>
  <div class="ip-grid" style="margin-bottom:20px">
    <div class="ip-card bad">
      <div class="ip-icon">🐄</div>
      <div class="ip-name">Novillito / Maíz ER</div>
      <div class="ip-val">14,5</div>
      <div class="ip-unit">kg maíz por kg novillito</div>
      <div class="ip-change bad">▲ alto vs 2023</div>
      <div class="ip-interp">Novillito $3.406/kg (Rosgan) ÷ maíz ER $235/kg. Con maíz BCR ($252/kg) daría 13,5 kg — el productor en ER necesita ~8% más maíz por kg de novillo.</div>
    </div>
    <div class="ip-card bad">
      <div class="ip-icon">🐄</div>
      <div class="ip-name">Ternero / Maíz ER</div>
      <div class="ip-val">15,7</div>
      <div class="ip-unit">kg maíz por kg ternero</div>
      <div class="ip-change bad">▲ supera al novillito</div>
      <div class="ip-interp">Ternero $3.698/kg (Rosgan) ÷ maíz ER $235/kg. El ternero supera al novillito en relación con el maíz: señal de retención de vientres y escasez de reposición.</div>
    </div>
    <div class="ip-card bad">
      <div class="ip-icon">🐷</div>
      <div class="ip-name">Capón / Maíz ER</div>
      <div class="ip-val">7,0</div>
      <div class="ip-unit">kg maíz por kg capón</div>
      <div class="ip-change neutral">▼ mejora vs jul-25</div>
      <div class="ip-interp">Capón $1.637/kg (SIO Carnes) ÷ maíz ER $235/kg. Con maíz BCR daría 6,5 kg — el integrador porcino en ER compra maíz local más barato, pero la relación sigue siendo alta.</div>
    </div>
    <div class="ip-card ok">
      <div class="ip-icon">🐔</div>
      <div class="ip-name">Pollo / Maíz ER</div>
      <div class="ip-val">8,6</div>
      <div class="ip-unit">kg maíz por kg pollo (nov-25)</div>
      <div class="ip-change ok">▼ mejora respecto al pico</div>
      <div class="ip-interp">Pollo ~$2.016/kg (SAGyP) ÷ maíz ER $235/kg. Con maíz BCR daría 8,0 kg (diferencia del 7,5%). El integrador avícola en ER enfrenta una relación levemente peor que el promedio nacional.</div>
    </div>
    <div class="ip-card ok">
      <div class="ip-icon">🥚</div>
      <div class="ip-name">Huevo / Maíz ER</div>
      <div class="ip-val">7,1</div>
      <div class="ip-unit">kg maíz por docena (dic-25)</div>
      <div class="ip-change ok">▼ mejora desde jun-25</div>
      <div class="ip-interp">Docena $1.663 (SAGyP-INDEC) ÷ maíz ER $235/kg. Con maíz BCR daría 6,6 kg. La producción avícola en ER — que concentra el 50% de la faena nacional — enfrenta este diferencial de costo de alimentación.</div>
    </div>
    <div class="ip-card bad">
      <div class="ip-icon">🥛</div>
      <div class="ip-name">Leche / Maíz ER</div>
      <div class="ip-val">2,0</div>
      <div class="ip-unit">kg maíz por litro al tambo</div>
      <div class="ip-change neutral">pendiente OCLA/SIGLeA ER</div>
      <div class="ip-interp">Precio tambo $481/L (SAGyP-DNL feb-26) ÷ maíz ER $235/kg. Históricamente la relación saludable es ~2,5-3 kg/L. El valor actual de 2,0 indica que el maíz está "caro" en términos de leche — comprime margen del tambo.</div>
    </div>
  </div>

  <!-- Gráficos -->
  <div class="g2">
    <div class="card">
      <div class="card-ttl">Evolución G2 / Granos — pizarra ER (kg por litro, diciembre)</div>
      <div class="ch h220"><canvas id="chartIP"></canvas></div>
      <div class="fuente">Pizarra: BOLSACER/SIBER (precio ER). Gasoil G2: Sec. Energía Paraná. Dic-22 a dic-25. Estimaciones para dic-24 y dic-25.</div>
      <div class="insight red"><strong>Arroz: el indicador más crítico.</strong> En dic-24 la relación era 3,3 kg/L (pizarra SIBER ~$350.000/t). En dic-25 es 10,6 kg/L (pizarra ~$185.000/t). La caída del 50% en el precio del arroz entrerrano, combinada con el gasoil +72%, genera la peor relación de la historia reciente del cultivo. Esto explica la caída proyectada de área 2025/26.</div>
    </div>
    <div class="card">
      <div class="card-ttl">Carnes y Leche / Maíz ER — 2023 a 2025 (promedio anual)</div>
      <div class="ch h220"><canvas id="chartCarnes"></canvas></div>
      <div class="fuente">Maíz: pizarra BOLSACER/SIBER (precio recibido en ER). Rosgan (novillito/ternero) · SIO Carnes (capón) · SAGyP (pollo/huevo). Nota: con pizarra BCR los valores serían ~7% menores.</div>
      <div class="insight blue"><strong>Efecto del precio ER del maíz:</strong> Al usar la pizarra BOLSACER (precio más bajo que BCR), las relaciones resultantes son ~7-8% más altas que las publicadas por monitores que usan precios Rosario. Esto refleja la realidad del productor/integrador en ER que compra o vende maíz a precio local.</div>
    </div>
  </div>
</div>
</section>

<!-- 04 BOVINOS -->
<section id="bovinos">
<div class="wrap">
  <div class="sec-hdr">
    <span class="sec-chip">04</span>
    <div><div class="sec-ttl">Sector Bovino</div><div class="sec-sub">FUCOFA · SAGyP · Rosgan · DGEC-ER</div></div>
  </div>
  <div class="g2">
    <div>
      <div style="display:flex;gap:12px;flex-wrap:wrap;margin-bottom:14px">
        <div class="metric green" style="flex:1"><div class="metric-lbl">Existencias 2025</div><div class="metric-val">4,4M</div><div class="metric-sub">+3% vs 1° campaña 25</div></div>
        <div class="metric green" style="flex:1"><div class="metric-lbl">Faena ER 2025</div><div class="metric-val">517K</div><div class="metric-sub">+6% a/a · 4% nacional</div></div>
        <div class="metric blue" style="flex:1"><div class="metric-lbl">Export. bovina (ene-oct 25)</div><div class="metric-val">USD 240M</div><div class="metric-sub">+74% a/a en valor</div></div>
      </div>
      <div class="card" style="overflow-x:auto">
        <div class="card-ttl">Precios Novillito y Ternero — Rosgan ($/kg vivo, promedio anual)</div>
        <table class="tbl">
          <thead><tr><th>Categoría</th><th>2023</th><th>2024</th><th>2025</th><th>Acum. 23-25</th></tr></thead>
          <tbody>
            <tr><td>🐄 Novillito 1-2 años</td><td>$584</td><td>$1.677</td><td class="big">$3.406</td><td><span class="bdg dn">+483%</span></td></tr>
            <tr><td>🐄 Ternero/a</td><td>$715</td><td>$2.137</td><td class="big">$3.698</td><td><span class="bdg dn">+417%</span></td></tr>
          </tbody>
        </table>
        <div class="fuente">Fuente: Rosgan — Mercado de Hacienda.</div>
        <div class="insight blue" style="margin-top:10px"><strong>Ciclo alcista bovino en pleno desarrollo.</strong> Los precios superaron la inflación acumulada del período. La convergencia entre novillito y ternero en 2025 señala retención de vientres consolidada. El ternero supera al novillito en precio relativo al maíz (17,4 vs 16,0 kg), indicando escasez de reposición. Las exportaciones (+74% en valor, +12% en volumen) actúan como piso del mercado interno.</div>
      </div>
    </div>
    <div class="card">
      <div class="card-ttl">Precio Novillito $/kg vivo — 2023–2025</div>
      <div class="ch h240"><canvas id="chartBov"></canvas></div>
      <div class="fuente">Fuente: Rosgan · novillos 1-2 años · promedio mensual.</div>
    </div>
  </div>
</div>
</section>

<!-- 05 PORCINOS -->
<section id="porcinos">
<div class="wrap">
  <div class="sec-hdr">
    <span class="sec-chip">05</span>
    <div><div class="sec-ttl">Sector Porcino</div><div class="sec-sub">SAGyP · SIO Carnes</div></div>
  </div>
  <div class="g2">
    <div>
      <div style="display:flex;gap:12px;flex-wrap:wrap;margin-bottom:14px">
        <div class="metric green" style="flex:1"><div class="metric-lbl">Existencias (mar-25)</div><div class="metric-val">566.989</div><div class="metric-sub">+2% vs 2024</div></div>
        <div class="metric green" style="flex:1"><div class="metric-lbl">Faena ER 2025</div><div class="metric-val">412K</div><div class="metric-sub">+7% a/a · 5% nac.</div></div>
      </div>
      <div class="card">
        <div class="card-ttl">Precio Capón (SIO Carnes · $/kg vivo · promedio anual)</div>
        <div style="display:flex;gap:12px;flex-wrap:wrap;margin-bottom:10px">
          <div class="metric" style="flex:1"><div class="metric-lbl">2023</div><div class="metric-val">$388</div></div>
          <div class="metric" style="flex:1"><div class="metric-lbl">2024</div><div class="metric-val">$1.125</div><div class="metric-sub" style="color:var(--red)">+190%</div></div>
          <div class="metric blue" style="flex:1"><div class="metric-lbl">2025</div><div class="metric-val">$1.637</div><div class="metric-sub">+45%</div></div>
        </div>
        <div class="fuente">Fuente: SIO Carnes · SAGyP. Acumulado 2023-2025: +322%.</div>
        <div class="insight blue"><strong>Porcinos: la proteína del desplazamiento.</strong> En 2024 el consumo superó 18 kg/hab/año (record histórico), traccionado por el deterioro del poder adquisitivo que corrió consumidores desde la carne vacuna. El DEX 0% permanente (D°697/2024) abre la exportación. La relación capón/maíz de 7,7 en dic-25 mejoró desde el pico de ~9 en julio, indicando que el sector recupera ecuación.</div>
      </div>
    </div>
    <div class="card">
      <div class="card-ttl">Precio Capón $/kg vivo — 2023–2025</div>
      <div class="ch h240"><canvas id="chartCap"></canvas></div>
      <div class="fuente">Fuente: SIO Carnes · SAGyP.</div>
    </div>
  </div>
</div>
</section>

<!-- 06 AVÍCOLA -->
<section id="aviar">
<div class="wrap">
  <div class="sec-hdr">
    <span class="sec-chip">06</span>
    <div><div class="sec-ttl">Sector Avícola</div><div class="sec-sub">ER = 50% de la faena avícola nacional · SAGyP · INDEC · DGEC-ER</div></div>
  </div>
  <div class="g2">
    <div>
      <div style="display:flex;gap:12px;flex-wrap:wrap;margin-bottom:14px">
        <div class="metric green" style="flex:1"><div class="metric-lbl">Faena ER 2025</div><div class="metric-val">3,7M</div><div class="metric-sub">+1% a/a · 50% nac.</div></div>
        <div class="metric red" style="flex:1"><div class="metric-lbl">Precio real pollo</div><div class="metric-val">−8%</div><div class="metric-sub">nov-25 vs nov-24 (IPC)</div></div>
        <div class="metric blue" style="flex:1"><div class="metric-lbl">Export. aviar (ene-oct 25)</div><div class="metric-val">USD 104M</div><div class="metric-sub">+4% valor · −10% vol.</div></div>
      </div>
      <div class="g2" style="gap:10px;margin-bottom:14px">
        <div class="card">
          <div class="card-ttl">🐔 Maíz / Pollo (nov-25)</div>
          <div style="font-family:'Syne',sans-serif;font-size:40px;font-weight:800;color:var(--blue1)">8 kg</div>
          <div style="font-size:12px;color:var(--ink3)">por kg de pollo</div>
          <div class="ip-change ok" style="margin-top:8px;display:inline-flex">▼ −30% vs nov-24</div>
        </div>
        <div class="card">
          <div class="card-ttl">🥚 Maíz / Huevo (dic-25)</div>
          <div style="font-family:'Syne',sans-serif;font-size:40px;font-weight:800;color:var(--blue1)">6,6 kg</div>
          <div style="font-size:12px;color:var(--ink3)">por docena</div>
          <div class="ip-change ok" style="margin-top:8px;display:inline-flex">▼ −25% vs dic-24</div>
        </div>
      </div>
      <div class="insight green"><strong>Avícola: el mejor momento del año para el integrador.</strong> Las relaciones maíz/pollo y maíz/huevo mejoraron ~30% en 12 meses, lo que implica menor costo de alimentación relativo. El precio real del pollo eviscerado cayó −8% interanual (deflactado IPC), señal de que los beneficios llegan al consumidor. Sin embargo, el aumento exportador (+4% en valor pero −10% en volumen) indica que la mejora de precio fue por tipo de cambio, no por mayor demanda.</div>
    </div>
    <div class="card">
      <div class="card-ttl">Maíz / Pollo y Maíz / Huevo — 2024–2025</div>
      <div class="ch h200"><canvas id="chartAv"></canvas></div>
      <div class="fuente">Fuente: SAGyP · INDEC. Precio huevo real deflactado IPC dic-25: $3.986/docena (−4% a/a).</div>
    </div>
  </div>
</div>
</section>

<!-- 07 LÁCTEO -->
<section id="lacteo">
<div class="wrap">
  <div class="sec-hdr">
    <span class="sec-chip">07</span>
    <div><div class="sec-ttl">Sector Lácteo</div><div class="sec-sub">OCLA · SAGyP-DNL (SIGLeA-LUME) · DGEC-ER</div></div>
  </div>

  <!-- KPIs -->
  <div style="display:flex;gap:12px;flex-wrap:wrap;margin-bottom:18px">
    <div class="metric blue" style="flex:1;min-width:140px"><div class="metric-lbl">Prod. ER 2024 (est.)</div><div class="metric-val">~900 M L</div><div class="metric-sub">~8,5% del total nac.</div></div>
    <div class="metric" style="flex:1;min-width:140px"><div class="metric-lbl">Tambos ER (SENASA)</div><div class="metric-val">~900</div><div class="metric-sub">4° provincia del país</div></div>
    <div class="metric red" style="flex:1;min-width:140px"><div class="metric-lbl">Precio tambo feb-26</div><div class="metric-val">$481/L</div><div class="metric-sub">Mínimo real 7 años (OCLA)</div></div>
    <div class="metric" style="flex:1;min-width:140px"><div class="metric-lbl">Prod. nacional 2024</div><div class="metric-val">10.585 M L</div><div class="metric-sub">−6,5% vs 2023 (OCLA)</div></div>
  </div>

  <div class="g2">
    <!-- Producción ER y nacional -->
    <div class="card">
      <div class="card-ttl">Producción de leche — Nacional y estimado ER (millones de litros)</div>
      <div class="ch h240"><canvas id="chartProdLec"></canvas></div>
      <div class="fuente">Fuente: OCLA — Estimaciones anuales de producción. Producción ER estimada como ~8% de la producción nacional (participación histórica SIGLeA-LUME). Fuente ER: <a href="https://www.ocla.org.ar/portafolio/16/" target="_blank">ocla.org.ar — Precios al Productor</a> · DGEC ER.</div>
      <div class="insight blue"><strong>Contexto 2020–2025:</strong> La producción nacional tuvo su pico en 2021–2022 (~11.550 M L) y luego cayó fuertemente en 2023 (sequía) y 2024 (−6,5% interanual, la mayor caída desde 2009). Entre Ríos, con su Cuenca Este, mostró mayor resiliencia que el sur de Santa Fe y Buenos Aires según OCLA: las tasas de crecimiento 2025 son más altas en ER, Córdoba y norte de Santa Fe. La proyección 2025 apunta a una recuperación del +5,7% (11.190 M L nacional; ~951 M L estimado ER).</div>
    </div>

    <!-- Precio al tambo + estructura -->
    <div>
      <div class="card" style="margin-bottom:14px">
        <div class="card-ttl">Precio leche al tambo — Panel 18 nacional ARS/litro (OCLA)</div>
        <div class="ch h180"><canvas id="chartLec"></canvas></div>
        <div style="font-size:10.5px;color:var(--red);margin-top:6px">⚠ Estimación mensual — serie oficial: <a href="https://www.ocla.org.ar/portafolio/16/" target="_blank" style="color:var(--blue2)">OCLA Panel 18</a> y <a href="https://www.magyp.gob.ar/sitio/areas/ss_lecheria/mercado_futuro/" target="_blank" style="color:var(--blue2)">SAGyP-DNL (día 11 c/mes)</a>.</div>
      </div>
      <div class="card">
        <div class="card-ttl">Estructura del sector lácteo en Entre Ríos</div>
        <div style="font-size:12.5px;line-height:2.1;color:var(--ink3)">
          🐄 <strong>Cuenca lechera:</strong> Cuenca Este (Entre Ríos) — una de las principales del país<br>
          🏭 <strong>Tambos:</strong> ~900 unidades (SENASA). 4° provincia en cantidad de tambos<br>
          📉 <strong>Tendencia:</strong> Concentración en tambos grandes (−36% tambos pequeños 2010-2022)<br>
          💧 <strong>Característica:</strong> Tambo promedio ER tiene menor volumen diario que otras provincias<br>
          📊 <strong>Part. nacional:</strong> ~8% producción, con tasas de crecimiento 2025 por encima del promedio<br>
          🥛 <strong>DEX lácteos:</strong> 0% permanente (D°697/2024) → ventaja exportadora
        </div>
        <div class="insight amber" style="margin-top:10px"><strong>Situación actual:</strong> El precio real al tambo está en mínimos de 7 años (OCLA, abr-25). Sin embargo, el sector muestra señales de recuperación en H2 2024 y proyecta crecimiento para 2025. La relación precio leche / costo maíz mejoró desde la devaluación de dic-23. El DEX 0% abre la exportación de lácteos con valor agregado, donde ER tiene potencial por su industria molinera.</div>
        <div style="background:var(--red2);border-radius:8px;padding:10px;font-size:12px;color:#991b1b;margin-top:10px;border:1px solid #fecaca">
          ⚠ <strong>Pendiente:</strong> Serie mensual SIGLeA por provincia ER desde OCLA. El precio nacional es proxy válido con diferencial de hasta 8%.
        </div>
      </div>
    </div>
  </div>

  <!-- Tabla producción anual -->
  <div class="card" style="margin-top:16px;overflow-x:auto">
    <div class="card-ttl">Producción nacional y estimado ER — Serie anual OCLA (millones de litros)</div>
    <table class="tbl">
      <thead><tr><th>Año</th><th>Prod. Nacional (M L)</th><th>Var. a/a</th><th>Prod. ER estimada (M L)</th><th>Part. ER (~8%)</th><th>Tambos ER (est.)</th><th>Contexto</th></tr></thead>
      <tbody>
        <tr><td>2020</td><td>10.575</td><td><span class="bdg up">+2,2%</span></td><td>~846</td><td>~8,0%</td><td>~1.050</td><td>Pandemia. Producción récord proyectado.</td></tr>
        <tr><td>2021</td><td>11.553</td><td><span class="bdg up">+9,2%</span></td><td>~924</td><td>~8,0%</td><td>~980</td><td>Fuerte recuperación post-pandemia.</td></tr>
        <tr><td>2022</td><td>11.557</td><td><span class="bdg fl">+0,04%</span></td><td>~925</td><td>~8,0%</td><td>~940</td><td>Estable. Buenos insumos forrajeros.</td></tr>
        <tr><td>2023</td><td>11.326</td><td><span class="bdg dn">−2,0%</span></td><td>~849</td><td>~7,5%</td><td>~900</td><td>Sequía generalizada. Venta de vacas.</td></tr>
        <tr><td>2024</td><td>10.585</td><td><span class="bdg dn">−6,5%</span></td><td>~900</td><td>~8,5%</td><td>~880</td><td>Mayor caída desde 2009. ER más resiliente.</td></tr>
        <tr><td>2025 (est.)</td><td>11.190</td><td><span class="bdg up">+5,7%</span></td><td>~951</td><td>~8,5%</td><td>~870</td><td>Recuperación. ER crece por encima del promedio.</td></tr>
      </tbody>
    </table>
    <div class="fuente">Fuente: OCLA — Estimaciones anuales de producción (Panel de industrias ~50% del total nacional). Producción ER: estimación propia basada en participación SIGLeA-LUME histórica (~8%). Para datos exactos por provincia: <a href="https://www.ocla.org.ar/portafolio/16/" target="_blank">ocla.org.ar — SIGLeA por Provincia</a>.</div>
  </div>
</div>
</section>

<!-- 08 DEX -->
<section id="dex">
<div class="wrap">
  <div class="sec-hdr">
    <span class="sec-chip">08</span>
    <div><div class="sec-ttl">Derechos de Exportación Vigentes</div><div class="sec-sub">Decreto 877/2025 · Baja permanente desde diciembre 2025 · Actualizado 01/04/2026</div></div>
  </div>
  <div class="ret-grid">
    <div class="ret-card zero"><div class="ret-prod">🍚 Arroz</div><div class="ret-pct">0%</div><div class="ret-norm">D°38/2025 · Permanente · Antes: 3–6%</div><div class="ret-bar"><div class="ret-bar-fill" style="width:0%"></div></div></div>
    <div class="ret-card zero"><div class="ret-prod">🐷 Porcinos</div><div class="ret-pct">0%</div><div class="ret-norm">D°697/2024 · Permanente · Antes: 4–9%</div><div class="ret-bar"><div class="ret-bar-fill" style="width:0%"></div></div></div>
    <div class="ret-card zero"><div class="ret-prod">🥛 Lácteos</div><div class="ret-pct">0%</div><div class="ret-norm">D°697/2024 · Permanente · Antes: 4,5–9%</div><div class="ret-bar"><div class="ret-bar-fill" style="width:0%"></div></div></div>
    <div class="ret-card low"><div class="ret-prod">🌾 Trigo</div><div class="ret-pct">7,5%</div><div class="ret-norm">D°877/2025 · Permanente · Antes: 12%</div><div class="ret-bar"><div class="ret-bar-fill" style="width:31%"></div></div></div>
    <div class="ret-card low"><div class="ret-prod">🌽 Maíz</div><div class="ret-pct">8,5%</div><div class="ret-norm">D°877/2025 · Permanente · Antes: 12%</div><div class="ret-bar"><div class="ret-bar-fill" style="width:35%"></div></div></div>
    <div class="ret-card low"><div class="ret-prod">🐄 Carne bovina</div><div class="ret-pct">~5%</div><div class="ret-norm">Post D°685/2025 · Antes: 5–9%</div><div class="ret-bar"><div class="ret-bar-fill" style="width:21%"></div></div></div>
    <div class="ret-card low"><div class="ret-prod">🐔 Carne aviar</div><div class="ret-pct">5%</div><div class="ret-norm">Post D°685/2025 · Sin cambio</div><div class="ret-bar"><div class="ret-bar-fill" style="width:21%"></div></div></div>
    <div class="ret-card mid"><div class="ret-prod">🌱 Sub. Soja</div><div class="ret-pct">22,5%</div><div class="ret-norm">D°877/2025 · Permanente · Antes: 31%</div><div class="ret-bar"><div class="ret-bar-fill" style="width:68%"></div></div></div>
    <div class="ret-card high"><div class="ret-prod">🌱 Soja poroto</div><div class="ret-pct">24%</div><div class="ret-norm">D°877/2025 · Permanente · Antes: 33%</div><div class="ret-bar"><div class="ret-bar-fill" style="width:73%"></div></div></div>
  </div>
  <div class="insight blue" style="margin-top:16px"><strong>Ventaja estructural de Entre Ríos:</strong> La combinación DEX 0% en arroz + porcinos + lácteos es única en el mapa agropecuario nacional. ER concentra el 32% del arroz, el 5% de la faena porcina y un tambo relevante — los tres rubros exentos de retenciones. Ninguna otra provincia con vocación agrícola tiene un diferencial fiscal tan favorable en sus actividades de mayor peso relativo.</div>
</div>
</section>

<!-- 09 METODOLOGÍA -->
<section id="metod">
<div class="wrap">
  <div class="sec-hdr">
    <span class="sec-chip">09</span>
    <div><div class="sec-ttl">Metodología y Fuentes</div><div class="sec-sub">Transparencia · Reproducibilidad</div></div>
  </div>
  <div class="card" style="overflow-x:auto">
    <table class="tbl" style="font-size:12px">
      <thead><tr><th style="width:180px">Variable</th><th>Fuente oficial</th><th>Metodología</th><th>Estado</th></tr></thead>
      <tbody>
        <tr><td><strong>FOB granos</strong></td><td><a href="https://datos.gob.ar/dataset/sspm-precios-fob-oficiales" target="_blank" style="color:var(--blue2)">datos.gob.ar — sspm_358.1</a> · SAGyP</td><td>Arroz: var. <strong>Uru5</strong> (mar). Trigo/Maíz: enero. Soja: mayo. Segundo miércoles hábil.</td><td style="color:#065f46;font-weight:600">✅ API automática</td></tr>
        <tr><td><strong>Pizarra BCR</strong></td><td><a href="https://cooperativalehmann.coop/acopio" target="_blank" style="color:var(--blue2)">Coop. Lehmann</a> · CAC-BCR</td><td>Promedios mensuales sobre puerto Rosario. Flete ER: ~USD 22/t (Paraná), ~USD 28/t (Concordia).</td><td style="color:#92400e;font-weight:600">📋 Manual mensual</td></tr>
        <tr><td><strong>Gasoil G2/G3 Paraná</strong></td><td><a href="http://res1104.se.gob.ar/consultaprecios.eess.php" target="_blank" style="color:var(--blue2)">res1104.se.gob.ar</a> · Sec. Energía</td><td>Precio por surtidor declarado bajo Res. 314/2016, filtrado por Entre Ríos.</td><td style="color:#991b1b;font-weight:600">⚠ Pendiente serie ER</td></tr>
        <tr><td><strong>TC BNA</strong></td><td><a href="https://api.estadisticasbcra.ar" target="_blank" style="color:var(--blue2)">api.estadisticasbcra.ar</a> · BCRA</td><td>TC vendedor minorista BNA (Com. B9791).</td><td style="color:#065f46;font-weight:600">✅ API automática</td></tr>
        <tr><td><strong>Novillito / Ternero</strong></td><td>Rosgan — Mercado de Hacienda · IPCVA</td><td>Novillos 1-2 años, promedio mensual.</td><td style="color:#92400e;font-weight:600">📋 Manual mensual</td></tr>
        <tr><td><strong>Capón Porcino</strong></td><td>SIO Carnes · SAGyP Dirección Porcinos</td><td>Precio vivo. Anuario Porcino para serie histórica.</td><td style="color:#92400e;font-weight:600">📋 Manual mensual</td></tr>
        <tr><td><strong>Pollo / Huevo</strong></td><td>SAGyP Dirección de Aves · INDEC IPC</td><td>Precio pollo deflactado IPC. Relaciones maíz/pollo y maíz/docena en unidades físicas.</td><td style="color:#92400e;font-weight:600">📋 Manual mensual</td></tr>
        <tr><td><strong>Leche al tambo</strong></td><td><a href="https://www.ocla.org.ar/portafolio/16/" target="_blank" style="color:var(--blue2)">OCLA</a> · SAGyP-DNL SIGLeA-LUME</td><td>Panel 18 empresas + SIGLeA por provincia ER. Publicación día 11 de cada mes.</td><td style="color:#991b1b;font-weight:600">⚠ Serie ER pendiente</td></tr>
        <tr><td><strong>Prod. leche ER</strong></td><td><a href="https://www.ocla.org.ar" target="_blank" style="color:var(--blue2)">OCLA</a> · SAGyP-DNL · DGEC-ER</td><td>Estimación propia ~8% de producción nacional OCLA. Dato exacto: SIGLeA por provincia.</td><td style="color:#92400e;font-weight:600">📋 Actualización anual</td></tr>
        <tr><td><strong>Relaciones IP</strong></td><td>Elaboración propia</td><td><strong>Gasoil/Grano:</strong> Gasoil (ARS/L) × 1.000 ÷ Pizarra (ARS/t) = kg/litro. <strong>Producto/Maíz:</strong> Precio (ARS/kg) × 1.000 ÷ Pizarra maíz (ARS/t) = kg maíz/kg producto. Consistente con CONINAGRO y FCECO-UNER/BOLSACER.</td><td>Elaboración propia</td></tr>
      </tbody>
    </table>
  </div>
</div>
</section>

<footer>
  <strong>Monitor Agropecuario · Entre Ríos</strong> · Datos de cierre: primer viernes hábil de cada mes<br>
  SAGyP · BOLSACER · DGEC-ER · OCLA · SIO Carnes · Rosgan · Secretaría de Energía · BCRA · Rosgan<br>
  <span id="ts" style="opacity:.4"></span>
</footer>

<script>
const ANOS=[2018,2019,2020,2021,2022,2023,2024,2025];
const M12=['Ene','Feb','Mar','Abr','May','Jun','Jul','Ago','Sep','Oct','Nov','Dic'];
const C={blue:'#2563eb',cyan:'#06b6d4',soja:'#16a34a',maiz:'#d97706',trigo:'#0284c7',arroz:'#92400e',bov:'#dc2626',cap:'#7c3aed',lec:'#0891b2'};

const DEF={
  plugins:{legend:{labels:{font:{family:'Inter',size:11},boxWidth:11,padding:10}}},
  scales:{x:{grid:{display:false},ticks:{font:{family:'Inter',size:11}}},y:{grid:{color:'rgba(0,0,0,.04)'},ticks:{font:{family:'Inter',size:11}}}},
  responsive:true,maintainAspectRatio:false,animation:{duration:500}
};

function line(id,labels,datasets,yLabel=''){
  const el=document.getElementById(id);if(!el)return;
  new Chart(el,{type:'line',data:{labels,datasets:datasets.map(d=>({tension:.35,pointRadius:3,borderWidth:2,fill:false,...d}))},
    options:{...DEF,scales:{...DEF.scales,y:{...DEF.scales.y,title:{display:!!yLabel,text:yLabel,font:{size:10}}}}}});
}
function bar(id,labels,datasets,yLabel=''){
  const el=document.getElementById(id);if(!el)return;
  new Chart(el,{type:'bar',data:{labels,datasets},
    options:{...DEF,scales:{...DEF.scales,y:{...DEF.scales.y,title:{display:!!yLabel,text:yLabel,font:{size:10}}}}}});
}
function doughnut(id,data,labels){
  const el=document.getElementById(id);if(!el)return;
  new Chart(el,{type:'doughnut',data:{labels,datasets:[{data,backgroundColor:['#2563eb','#0284c7','#06b6d4','#0d9488','#16a34a'],borderWidth:2,borderColor:'#fff'}]},
    options:{plugins:{legend:{position:'right',labels:{font:{size:10,family:'Inter'},boxWidth:9,padding:5}}},cutout:'62%',responsive:true,maintainAspectRatio:false}});
}

// FOB
let fobCh;
(function(){
  const el=document.getElementById('chartFOB');if(!el)return;
  fobCh=new Chart(el,{type:'line',data:{labels:ANOS,datasets:[
    {label:'Soja',data:[317,322,368,495,578,458,375,323],borderColor:C.soja,tension:.35,pointRadius:3,borderWidth:2,fill:false},
    {label:'Maíz',data:[154,148,163,268,315,215,185,185],borderColor:C.maiz,tension:.35,pointRadius:3,borderWidth:2,fill:false},
    {label:'Trigo',data:[208,192,213,280,385,245,215,218],borderColor:C.trigo,tension:.35,pointRadius:3,borderWidth:2,fill:false},
    {label:'Arroz Uru5',data:[268,282,230,390,420,610,470,420],borderColor:C.arroz,tension:.35,pointRadius:3,borderWidth:2,fill:false,borderDash:[5,3]},
  ]},options:{...DEF,plugins:{...DEF.plugins,tooltip:{callbacks:{label:c=>`${c.dataset.label}: USD ${c.raw}/t`}}},
    scales:{...DEF.scales,y:{...DEF.scales.y,title:{display:true,text:'USD / tonelada',font:{size:10}}}}}});
})();

// API FOB
async function fetchFOB(){
  try{
    const url='https://apis.datos.gob.ar/series/api/series/?ids=358.1_HABAS_SOJAADO__52,358.1_MAIZ_DEMASADO__52,358.1_TRIGO_GRANADO__41&collapse=year&collapse_aggregation=avg&limit=8&sort=asc&format=json';
    const r=await fetch(url,{signal:AbortSignal.timeout(6000)});
    if(!r.ok)throw new Error();
    const d=await r.json();
    const rows=d.data.slice(1);
    if(rows.length&&fobCh){
      fobCh.data.labels=rows.map(r=>parseInt(r[0]));
      fobCh.data.datasets[0].data=rows.map(r=>r[1]?Math.round(r[1]):null);
      fobCh.data.datasets[1].data=rows.map(r=>r[2]?Math.round(r[2]):null);
      fobCh.data.datasets[2].data=rows.map(r=>r[3]?Math.round(r[3]):null);
      fobCh.update();
      const s=document.getElementById('fob-status');if(s){s.textContent='✅ datos en tiempo real';s.style.color='#16a34a';}
      const lastSoja=rows.map(r=>r[1]).filter(Boolean).pop();
      if(lastSoja){const k=document.getElementById('kpi-soja');if(k)k.textContent='USD '+Math.round(lastSoja)+'/t';}
      const fd=document.getElementById('fob-date');if(fd)fd.textContent='API actualizada '+new Date().toLocaleDateString('es-AR');
    }
  }catch(e){console.log('FOB API no disponible — usando datos base');}
}
fetchFOB();

// Costos
doughnut('cTrigo',[44,12,18,15,11],['Insumos','Labores','Flete','Arrendamiento','Otros']);
doughnut('cMaiz',[35,43,8,9,5],['Insumos','Labores','Flete','Arrendamiento','Otros']);
doughnut('cSoja',[35,12,22,20,11],['Insumos','Labores','Flete','Arrendamiento','Otros']);

// IP Gasoil barras
// IP con pizarra ER (BOLSACER/SIBER): valores más altos (peores) que BCR
// Fórmula: G2_Paraná (ARS/L) × 1000 / Pizarra_ER (ARS/t)
// Dic-24: G2~$1.145/L | Soja ER~$365k | Maíz ER~$218k | Trigo ER~$240k | Arroz SIBER~$350k
// Dic-25: G2~$1.967/L | Soja ER~$420k | Maíz ER~$235k | Trigo ER~$235k | Arroz SIBER~$185k
bar('chartIP',['Dic-22','Dic-23','Dic-24','Dic-25'],[
  {label:'G2/Soja ER',  data:[1.8,2.2,3.1,4.7],backgroundColor:'rgba(37,99,235,.75)',borderRadius:4},
  {label:'G2/Maíz ER',  data:[4.3,4.8,5.3,8.4],backgroundColor:'rgba(217,119,6,.75)',borderRadius:4},
  {label:'G2/Trigo ER', data:[3.6,4.5,4.8,8.4],backgroundColor:'rgba(2,132,199,.75)',borderRadius:4},
  {label:'G2/Arroz ER', data:[2.4,1.2,3.3,10.6],backgroundColor:'rgba(146,64,14,.75)',borderRadius:4},
],'kg grano por litro G2 — pizarra ER');

// Carnes/Maíz
// Carnes/Maíz con pizarra BOLSACER (maíz ER < maíz BCR → ratios más altos)
// Maíz ER: 2023~$200/kg · 2024~$218/kg · 2025~$235/kg
bar('chartCarnes',['2023','2024','2025'],[
  {label:'Novillito/Maíz ER',data:[2.9,7.7,14.5],backgroundColor:'rgba(220,38,38,.75)',borderRadius:4},
  {label:'Ternero/Maíz ER',  data:[3.6,9.8,15.7],backgroundColor:'rgba(185,28,28,.5)',borderRadius:4},
  {label:'Capón/Maíz ER',    data:[1.9,5.2,7.0],backgroundColor:'rgba(124,58,237,.75)',borderRadius:4},
  {label:'Pollo/Maíz ER',    data:[null,11.9,8.6],backgroundColor:'rgba(37,99,235,.75)',borderRadius:4},
],'kg maíz ER por kg producto (pizarra BOLSACER/SIBER)');

// Novillito
const lbov=[],vbov=[],vtner=[];
['2023','2024','2025'].forEach((y,yi)=>{
  M12.forEach((m,mi)=>{
    lbov.push(m+'-'+y.slice(2));
    const novData=[[336,358,380,402,424,455,494,532,572,615,680,993],[1950,1960,1920,1880,1860,1840,1820,1840,1900,1965,2125,2357],[2476,2792,2951,3016,3100,3180,3260,3330,3400,3406,3406,3406]];
    const terData=[[380,420,460,500,540,580,640,710,780,850,950,1200],[2050,2050,2020,1980,1960,1940,1920,1940,2000,2100,2280,2500],[2700,2950,3100,3200,3350,3450,3550,3620,3680,3698,3698,3698]];
    vbov.push(novData[yi][mi]);vtner.push(terData[yi][mi]);
  });
});
line('chartBov',lbov,[
  {label:'Novillito',data:vbov,borderColor:C.bov,fill:true,backgroundColor:'rgba(220,38,38,.05)',pointRadius:1.5,borderWidth:2},
  {label:'Ternero',data:vtner,borderColor:'#b91c1c',fill:false,pointRadius:1.5,borderWidth:1.5,borderDash:[4,3]},
],'ARS/kg vivo');

// Capón
line('chartCap',['23Q1','Q2','Q3','Q4','24Q1','Q2','Q3','Q4','25Q1','Q2','Q3','Q4'],[
  {label:'Capón $/kg',data:[280,310,350,390,550,750,950,1100,1400,1550,1650,1637],borderColor:'#7c3aed',fill:true,backgroundColor:'rgba(124,58,237,.06)',pointRadius:3,borderWidth:2}
],'ARS/kg vivo');

// Avícola
const lav=M12.map((m,i)=>m+(i<12?'-24':'-25'));
line('chartAv',lav,[
  {label:'Maíz/Pollo (kg/kg)',data:[11.4,11.0,10.5,10.0,10.2,11.4,9.5,9.0,8.5,8.2,8.1,8.0],borderColor:C.blue,fill:false,borderWidth:2,pointRadius:3},
  {label:'Maíz/Huevo (kg/doc)',data:[8.0,8.5,9.2,10.5,11.0,8.8,8.5,9.0,10.0,11.0,8.0,6.6],borderColor:C.cyan,fill:false,borderWidth:2,pointRadius:3},
],'kg maíz por unidad');

// Producción leche
(function(){
  const el=document.getElementById('chartProdLec');if(!el)return;
  new Chart(el,{type:'bar',data:{
    labels:[2020,2021,2022,2023,2024,'2025 est.'],
    datasets:[
      {label:'Nacional (M L)',data:[10575,11553,11557,11326,10585,11190],
       backgroundColor:'rgba(37,99,235,.75)',borderRadius:4,yAxisID:'y'},
      {label:'ER estimada (M L)',data:[846,924,925,849,900,951],
       backgroundColor:'rgba(6,182,212,.8)',borderRadius:4,yAxisID:'y2'},
    ]
  },options:{
    responsive:true,maintainAspectRatio:false,
    plugins:{legend:{labels:{font:{family:'Inter',size:11},boxWidth:11,padding:10}}},
    scales:{
      x:{grid:{display:false},ticks:{font:{family:'Inter',size:11}}},
      y:{grid:{color:'rgba(0,0,0,.04)'},ticks:{font:{family:'Inter',size:11}},title:{display:true,text:'Nacional (M L)',font:{size:10}},position:'left'},
      y2:{grid:{display:false},ticks:{font:{family:'Inter',size:11}},title:{display:true,text:'ER estimada (M L)',font:{size:10}},position:'right'},
    }
  }});
})();

// Leche
const llec=['Ene-20','Jul-20','Ene-21','Jul-21','Ene-22','Jul-22','Ene-23','Jul-23','Ene-24','Jul-24','Ene-25','Jul-25'];
line('chartLec',llec,[
  {label:'Leche $/L (est.)',data:[20,25,30,40,55,85,135,265,640,750,840,900],borderColor:C.lec,fill:true,backgroundColor:'rgba(8,145,178,.05)',borderWidth:2,pointRadius:3,borderDash:[5,3]},
],'ARS/litro (estimado)');

// Toggle G2/G3
function showG(g,btn){
  document.getElementById('panel-g2').style.display=g==='g2'?'':'none';
  document.getElementById('panel-g3').style.display=g==='g3'?'':'none';
  document.querySelectorAll('.toggle-btn').forEach(b=>b.classList.remove('active'));
  btn.classList.add('active');
}

// Footer timestamp
document.getElementById('ts').textContent='Generado: '+new Date().toLocaleString('es-AR',{dateStyle:'long',timeStyle:'short'});
</script>
</body>
</html>
