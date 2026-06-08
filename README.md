[dashboard_preventivas.html](https://github.com/user-attachments/files/28704041/dashboard_preventivas.html)
<!DOCTYPE html>
<html lang="pt-BR">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Dashboard Preventivas – SPC</title>
<script src="https://cdnjs.cloudflare.com/ajax/libs/pdf.js/3.11.174/pdf.min.js"></script>
<style>
* { box-sizing: border-box; margin: 0; padding: 0; }
body { font-family: -apple-system, BlinkMacSystemFont, 'Segoe UI', sans-serif; background: #0f1117; color: #e0e0e0; font-size: 13px; }

/* TOP BAR */
.topbar { background: #1a1a2e; border-bottom: 2px solid #ffe600; padding: 12px 20px; display: flex; align-items: center; gap: 16px; }
.topbar .logo { background: #ffe600; border-radius: 8px; padding: 4px 10px; font-weight: 900; font-size: 13px; color: #1a1a1a; }
.topbar h1 { font-size: 16px; font-weight: 700; color: #fff; }

/* TABS */
.tabs { background: #16192a; padding: 0 20px; display: flex; gap: 4px; border-bottom: 1px solid #2a2d3e; }
.tab { padding: 10px 18px; cursor: pointer; font-size: 12px; font-weight: 700; color: #888; border-bottom: 3px solid transparent; transition: all .2s; display: flex; align-items: center; gap: 6px; }
.tab:hover { color: #fff; }
.tab.active { color: #fff; border-bottom-color: #ffe600; }
.tab .badge { background: #2a2d3e; color: #ccc; border-radius: 10px; padding: 1px 7px; font-size: 11px; }
.tab.active .badge { background: #ffe600; color: #1a1a1a; }

/* FILTERS */
.filters { background: #13151f; padding: 10px 20px; display: flex; gap: 10px; flex-wrap: wrap; align-items: center; border-bottom: 1px solid #2a2d3e; }
.filters input, .filters select { background: #1e2133; border: 1px solid #2a2d3e; border-radius: 6px; padding: 6px 10px; color: #ccc; font-size: 12px; }
.filters input:focus, .filters select:focus { outline: none; border-color: #ffe600; }
.filters input { width: 200px; }
.filter-label { font-size: 11px; color: #666; font-weight: 600; }

/* KPI ROW */
.kpi-row { display: flex; gap: 12px; padding: 14px 20px; background: #13151f; border-bottom: 1px solid #2a2d3e; }
.kpi { background: #1e2133; border-radius: 10px; padding: 12px 18px; flex: 1; min-width: 100px; border-left: 4px solid #2a2d3e; }
.kpi.s-nao  { border-left-color: #555; }
.kpi.s-and  { border-left-color: #2196f3; }
.kpi.s-atra { border-left-color: #ff9800; }
.kpi.s-con  { border-left-color: #4caf50; }
.kpi .kpi-num  { font-size: 26px; font-weight: 900; color: #fff; }
.kpi .kpi-lbl  { font-size: 11px; color: #888; text-transform: uppercase; letter-spacing: 0.5px; margin-top: 2px; }

/* LIST VIEW */
.list-view { padding: 16px 20px; display: flex; flex-direction: column; gap: 10px; }
.list-header { display: grid; grid-template-columns: 1fr 140px 180px 160px; gap: 12px; padding: 8px 14px; background: #13151f; border-radius: 8px; font-size: 11px; font-weight: 700; color: #666; text-transform: uppercase; letter-spacing: 0.5px; }

/* CARD LIST */
.card { background: #22263a; border-radius: 10px; padding: 14px; border: 1px solid #2a2d3e; transition: border-color .2s; display: grid; grid-template-columns: 1fr 140px 180px 160px; gap: 12px; align-items: center; }
.card:hover { border-color: #3a3d5e; }
.card-info { display: flex; flex-direction: column; gap: 3px; }

/* BOTÃO ENVIAR ORDEM */
.btn-enviar-ordem { display: flex; align-items: center; gap: 6px; padding: 8px 12px; background: #ffe600; color: #1a1a1a; border: none; border-radius: 8px; font-size: 11px; font-weight: 800; cursor: pointer; text-transform: uppercase; letter-spacing: 0.3px; transition: background .15s; white-space: nowrap; margin-top: 8px; }
.btn-enviar-ordem:hover { background: #f0d000; }

/* MODAL LINK */
.link-modal { display: none; position: fixed; inset: 0; background: rgba(0,0,0,.6); z-index: 600; align-items: flex-end; justify-content: center; }
.link-modal.open { display: flex; }
.link-box { background: #fff; width: 100%; max-width: 520px; border-radius: 20px 20px 0 0; padding: 24px 20px 32px; }
.link-box h3 { font-size: 16px; font-weight: 800; margin-bottom: 6px; }
.link-box p  { font-size: 12px; color: #888; margin-bottom: 16px; }
.link-url { background: #f5f5f5; border: 1px solid #e0e0e0; border-radius: 8px; padding: 12px 14px; font-size: 11px; color: #333; word-break: break-all; margin-bottom: 14px; line-height: 1.5; }
.link-btns { display: flex; gap: 10px; }
.link-btn-copy { flex: 1; padding: 13px; background: #1a1a1a; color: #ffe600; border: none; border-radius: 10px; font-size: 13px; font-weight: 700; cursor: pointer; }
.link-btn-wpp  { flex: 1; padding: 13px; background: #25d366; color: #fff; border: none; border-radius: 10px; font-size: 13px; font-weight: 700; cursor: pointer; }
.link-btn-fechar { width: 100%; padding: 12px; background: #f0f0f0; color: #666; border: none; border-radius: 10px; font-size: 13px; font-weight: 600; cursor: pointer; margin-top: 8px; }

/* PDF UPLOAD */
.pdf-area { display: flex; flex-direction: column; gap: 6px; align-items: flex-start; }
.btn-upload { display: flex; align-items: center; gap: 5px; padding: 6px 10px; background: #1a2a3a; border: 1px dashed #2a4a6a; border-radius: 7px; color: #64b5f6; font-size: 11px; font-weight: 700; cursor: pointer; transition: all .15s; white-space: nowrap; }
.btn-upload:hover { background: #1e3a5a; border-color: #64b5f6; }
.pdf-name { font-size: 10px; color: #4caf50; font-weight: 600; white-space: nowrap; overflow: hidden; text-overflow: ellipsis; max-width: 130px; }
.pdf-dados { margin-top: 6px; background: #1a2a1a; border: 1px solid #2a4a2a; border-radius: 7px; padding: 7px 10px; display: flex; flex-direction: column; gap: 3px; }
.pdf-dado-row { display: flex; justify-content: space-between; align-items: center; gap: 8px; }
.pdf-dado-lbl { font-size: 9px; color: #666; text-transform: uppercase; font-weight: 600; white-space: nowrap; }
.pdf-dado-val { font-size: 11px; color: #a5d6a7; font-weight: 800; }
.pdf-reading { font-size: 10px; color: #ffe600; font-style: italic; animation: pulse 1s infinite; }
@keyframes pulse { 0%,100%{opacity:1} 50%{opacity:.4} }
.pdf-link { font-size: 10px; color: #64b5f6; text-decoration: none; font-weight: 600; }
.pdf-link:hover { text-decoration: underline; }

/* RELATORIO */
.rel-area { display: flex; flex-direction: column; gap: 6px; }
.rel-link { font-size: 11px; color: #81c784; font-weight: 700; text-decoration: none; }
.rel-link:hover { text-decoration: underline; }
.rel-vazio { font-size: 11px; color: #444; font-style: italic; }
.rel-gerado { background: #1a2a1a; border: 1px solid #2a4a2a; border-radius: 8px; padding: 8px 10px; display: flex; flex-direction: column; gap: 5px; }
.rel-gerado .rel-badge { font-size: 11px; font-weight: 800; color: #81c784; }
.rel-gerado .rel-info  { font-size: 10px; color: #666; }
.btn-rel-pdf { padding: 6px 10px; background: #1a3a1a; color: #81c784; border: 1px solid #2a4a2a; border-radius: 7px; font-size: 10px; font-weight: 700; cursor: pointer; text-align: center; }

/* STATUS SELECT */
.status-sel { width: 100%; padding: 7px 10px; border-radius: 8px; border: 2px solid #2a2d3e; font-size: 12px; font-weight: 700; cursor: pointer; appearance: none; text-align: center; }
.status-sel:focus { outline: none; }
.status-sel.nao   { background: #252840; color: #aaa;     border-color: #3a3d5e; }
.status-sel.and   { background: #1a2a3a; color: #64b5f6;  border-color: #2a4a6a; }
.status-sel.atra  { background: #3a2a1a; color: #ff9800;  border-color: #6a4a2a; }
.status-sel.con   { background: #1a2a1a; color: #81c784;  border-color: #2a4a2a; }

/* CARDS */
.card { background: #22263a; border-radius: 10px; padding: 12px; border: 1px solid #2a2d3e; cursor: default; transition: border-color .2s, transform .15s; }
.card:hover { border-color: #ffe600; transform: translateY(-1px); }
.card-top { display: flex; justify-content: space-between; align-items: flex-start; margin-bottom: 6px; gap: 8px; }
.card-title { font-size: 16px; font-weight: 900; color: #ffffff; line-height: 1.3; flex: 1; letter-spacing: 0.3px; }
.card-title .equip-num { display: inline-block; background: transparent; color: #ffffff; font-size: 16px; font-weight: 900; margin-left: 4px; vertical-align: middle; }
.card-equip-sap { font-size: 13px; font-weight: 800; color: #ffffff; margin: 4px 0 8px; }
.card-equip-sap span { color: #aaa; font-weight: 400; font-size: 11px; margin-right: 4px; }
.card-plano { font-size: 10px; background: #2a2d3e; color: #aaa; border-radius: 5px; padding: 2px 7px; white-space: nowrap; flex-shrink: 0; }
.chip-categoria { font-size: 11px; font-weight: 700; background: #252840; color: #ffe600; border-radius: 5px; padding: 3px 10px; display: inline-block; margin-bottom: 4px; }
.card-row { display: flex; align-items: center; gap: 6px; margin-top: 5px; flex-wrap: wrap; }
.chip { font-size: 10px; font-weight: 700; border-radius: 5px; padding: 2px 8px; }
.chip.eletmec { background: #1a3a5c; color: #64b5f6; }
.chip.condomi { background: #3a1a5c; color: #ce93d8; }
.chip.ofc01   { background: #3a2a1a; color: #ffcc80; }
.chip.eletric { background: #1a3a1a; color: #a5d6a7; }
.chip.abc-a   { background: #3a1a1a; color: #ef9a9a; }
.chip.abc-b   { background: #3a3a1a; color: #fff59d; }
.chip.abc-c   { background: #1a2a1a; color: #a5d6a7; }
.card-local { font-size: 10px; color: #666; margin-top: 4px; white-space: nowrap; overflow: hidden; text-overflow: ellipsis; }
.card-ordem { font-size: 10px; color: #555; margin-top: 3px; }
.card-links { display: flex; gap: 6px; margin-top: 8px; }
.card-link { font-size: 10px; font-weight: 700; padding: 3px 8px; border-radius: 5px; text-decoration: none; }
.card-link.pdf  { background: #1a3a5c; color: #64b5f6; }
.card-link.rel  { background: #1a3a1a; color: #81c784; }
.card-link.form { background: #3a3a1a; color: #ffe600; }

/* VOLTAR */
.btn-voltar { display: none; align-items: center; gap: 8px; margin: 12px 20px 0; padding: 8px 16px; background: #1e2133; border: 1px solid #2a2d3e; border-radius: 8px; color: #ffe600; font-size: 12px; font-weight: 700; cursor: pointer; width: fit-content; }
.btn-voltar:hover { background: #252840; border-color: #ffe600; }

/* SUB-SELEÇÃO (Semanal / Mensal) */
.subsel-view { display: none; padding: 32px 20px; }
.subsel-title { font-size: 13px; color: #666; font-weight: 600; text-transform: uppercase; letter-spacing: 0.5px; margin-bottom: 20px; }
.subsel-grid { display: flex; gap: 16px; flex-wrap: wrap; }
.subsel-card { flex: 1; min-width: 200px; max-width: 280px; background: #1e2133; border: 2px solid #2a2d3e; border-radius: 16px; padding: 32px 24px; text-align: center; cursor: pointer; transition: all .2s; }
.subsel-card:hover { border-color: #ffe600; background: #252840; transform: translateY(-2px); }
.subsel-icon { font-size: 42px; margin-bottom: 12px; }
.subsel-name { font-size: 16px; font-weight: 900; color: #fff; text-transform: uppercase; letter-spacing: 0.5px; }
.subsel-desc { font-size: 11px; color: #666; margin-top: 6px; }

/* SEMANAS */
.semana-view { display: none; padding: 32px 20px; }
.semana-title { font-size: 13px; color: #666; font-weight: 600; text-transform: uppercase; letter-spacing: 0.5px; margin-bottom: 20px; }
.semana-grid { display: grid; grid-template-columns: repeat(auto-fill, minmax(200px, 1fr)); gap: 16px; }
.semana-card { background: #1e2133; border: 2px solid #2a2d3e; border-radius: 16px; padding: 28px 20px; text-align: center; cursor: pointer; transition: all .2s; position: relative; }
.semana-card:hover { border-color: #ffe600; background: #252840; transform: translateY(-2px); }
.semana-num { font-size: 13px; font-weight: 700; color: #ffe600; text-transform: uppercase; letter-spacing: 1px; margin-bottom: 6px; }
.semana-icon { font-size: 38px; margin-bottom: 10px; }
.semana-name { font-size: 15px; font-weight: 900; color: #fff; }
.semana-equip { font-size: 11px; color: #666; margin-top: 6px; }
.semana-status-bar { display: flex; gap: 4px; justify-content: center; margin-top: 10px; flex-wrap: wrap; }
.semana-pill { font-size: 9px; font-weight: 700; padding: 2px 7px; border-radius: 4px; }
.sp-nao  { background: #2a2d3e; color: #888; }
.sp-and  { background: #1a2a3a; color: #64b5f6; }
.sp-atra { background: #3a2a1a; color: #ff9800; }
.sp-con  { background: #1a2a1a; color: #81c784; }

/* EMPTY */
.empty { text-align: center; padding: 40px 20px; color: #444; font-size: 12px; }

/* HIDDEN */
.hidden { display: none !important; }

/* SOROCABA VIEW */
.sorocaba-view { padding: 24px 20px; display: none; }
.sorocaba-view.active { display: block; }
.sorocaba-title { font-size: 13px; color: #666; font-weight: 600; text-transform: uppercase; letter-spacing: 0.5px; margin-bottom: 16px; }
.equip-grid { display: grid; grid-template-columns: repeat(auto-fill, minmax(180px, 1fr)); gap: 14px; }
.equip-card { background: #1e2133; border: 1px solid #2a2d3e; border-radius: 14px; padding: 22px 16px; text-align: center; cursor: pointer; transition: all .2s; }
.equip-card:hover { border-color: #ffe600; transform: translateY(-2px); background: #252840; }
.equip-card.selected { border-color: #ffe600; background: #252840; }
.equip-icon { font-size: 36px; margin-bottom: 10px; line-height: 1; }
.equip-name { font-size: 12px; font-weight: 800; color: #e0e0e0; text-transform: uppercase; letter-spacing: 0.4px; line-height: 1.3; }
.equip-count { margin-top: 8px; font-size: 11px; color: #888; }
.equip-count .n { font-size: 20px; font-weight: 900; color: #fff; display: block; }
.equip-count .s { font-size: 10px; }
.equip-pills { display: flex; justify-content: center; gap: 4px; margin-top: 8px; flex-wrap: wrap; }
.equip-pill { font-size: 9px; font-weight: 700; padding: 2px 6px; border-radius: 4px; }
.pill-nao { background: #2a2d3e; color: #888; }
.pill-and { background: #1a2a3a; color: #64b5f6; }
.pill-con { background: #1a2a1a; color: #81c784; }
</style>
</head>
<body>

<div class="topbar">
  <div class="logo">🤝</div>
  <h1>Dashboard Preventivas – SPC E239</h1>
</div>

<div class="tabs" id="tabs">
  <div class="tab active" onclick="setTab('SOROCABA')" data-tab="SOROCABA">🏭 SOROCABA <span class="badge" id="badge-SOROCABA">9</span></div>
</div>

<div style="display:flex;gap:8px;flex-wrap:wrap;margin:12px 20px 0">
  <button class="btn-voltar" id="btnVoltar" style="margin:0" onclick="voltarSorocaba()">← Sorocaba</button>
  <button class="btn-voltar" id="btnVoltarSub" style="display:none;margin:0;border-color:#64b5f6;color:#64b5f6" onclick="voltarSubSel()">← Semanal / Mensal</button>
  <button class="btn-voltar" id="btnVoltarSemana" style="display:none;margin:0;border-color:#a5d6a7;color:#a5d6a7" onclick="voltarSemanas()">← Semanas</button>
</div>

<div class="filters">
  <span class="filter-label">Filtrar:</span>
  <input type="text" id="search" placeholder="🔍 Equipamento ou descrição..." oninput="render()">
  <select id="filtroABC" onchange="render()">
    <option value="">Código ABC – Todos</option>
    <option value="A">A – Crítico</option>
    <option value="B">B – Importante</option>
    <option value="C">C – Normal</option>
  </select>
  <select id="filtroPlano" onchange="render()">
    <option value="">Plano – Todos</option>
  </select>
</div>

<div class="kpi-row">
  <div class="kpi s-nao" ><div class="kpi-num" id="kpi-nao">0</div><div class="kpi-lbl">⏸ Não Iniciado</div></div>
  <div class="kpi s-and" ><div class="kpi-num" id="kpi-and">0</div><div class="kpi-lbl">🔄 Em Andamento</div></div>
  <div class="kpi s-atra"><div class="kpi-num" id="kpi-atra">0</div><div class="kpi-lbl">⚠️ Atrasado</div></div>
  <div class="kpi s-con" ><div class="kpi-num" id="kpi-con">0</div><div class="kpi-lbl">✅ Concluído</div></div>
  <div class="kpi" style="border-left-color:#ffe600"><div class="kpi-num" id="kpi-tot">0</div><div class="kpi-lbl">📋 Total</div></div>
</div>

<div class="link-modal" id="linkModal">
  <div class="link-box">
    <h3>📤 Enviar Ordem para o Time</h3>
    <p>Compartilhe o link abaixo com o técnico. Ele abrirá o formulário no celular.</p>
    <div class="link-url" id="linkUrl"></div>
    <div class="link-btns">
      <button class="link-btn-copy" onclick="copiarLink()">📋 Copiar Link</button>
      <button class="link-btn-wpp"  onclick="enviarWhatsApp()">💬 WhatsApp</button>
    </div>
    <button class="link-btn-fechar" onclick="fecharLinkModal()">Fechar</button>
  </div>
</div>

<div class="semana-view" id="semanaView">
  <div class="semana-title" id="semanaTitle">Selecione a Semana</div>
  <div class="semana-grid" id="semanaGrid"></div>
</div>

<div class="subsel-view" id="subselView">
  <div class="subsel-title" id="subselTitle">Selecione o tipo de inspeção</div>
  <div class="subsel-grid" id="subselGrid"></div>
</div>

<div class="sorocaba-view" id="sorocabaView">
  <div class="sorocaba-title">📍 Sorocaba E239 — Tipos de Equipamento</div>
  <div class="equip-grid" id="equipGrid"></div>
</div>

<div id="kanbanView" style="display:none">
  <div class="list-header">
    <div>Equipamento</div>
    <div>📄 PDF Preventiva</div>
    <div>📋 Relatório Preventiva</div>
    <div>🔖 Status</div>
  </div>
  <div class="list-view" id="listBody"></div>
</div>


<script>
// ── DADOS DA PLANILHA ──
const DATA = [
  {centro:'ELETRIC',local:'MLB-UTR05-E239-ADO-ELET',denomLocal:'INFRA ESTRUTURA ELÉTRICA',equip:'',abc:'',denom:'INFRA ESTRUTURA ELÉTRICA',plano:'9062',desc:'F_1A_ELETRIC_PREDITIVA ANUAL TERMOGRÁFIC',ordem:'5303593',pdfSAP:'',relatorio:''},
  {centro:'CONDOMI',local:'MLB-UTR05-E239-ADO-ELET',denomLocal:'INFRA ESTRUTURA ELÉTRICA',equip:'',abc:'',denom:'INFRA ESTRUTURA ELÉTRICA',plano:'9063',desc:'F_1A_CONDOM_LAUDO TÉCNICO SPDA',ordem:'5303596',pdfSAP:'',relatorio:''},
  {centro:'CONDOMI',local:'MLB-UTR05-E239-ADO-ELET',denomLocal:'INFRA ESTRUTURA ELÉTRICA',equip:'',abc:'',denom:'INFRA ESTRUTURA ELÉTRICA',plano:'9064',desc:'F_1A_CONDOM_LAUDO TÉCNICO NR10',ordem:'5303599',pdfSAP:'',relatorio:''},
  {centro:'CONDOMI',local:'MLB-UTR05-E239-PRD-TELH',denomLocal:'TELHADO',equip:'10199524',abc:'A',denom:'TELHADO',plano:'9065',desc:'F_1A_CONDOM_INSPECAO DE TELHADOS',ordem:'5303601',pdfSAP:'',relatorio:''},
  {centro:'ELETMEC',local:'MLB-UTR05-E239-AD1-SOPE',denomLocal:'SALA DE OPERAÇÃO',equip:'10199530',abc:'C',denom:'AR CONDICIONADO 001',plano:'9066',desc:'F_7D_ELETMEC_INSPEÇÃO SEMANAL DE CLIMATIZAÇÃO',ordem:'5303935',pdfSAP:'',relatorio:''},
  {centro:'ELETMEC',local:'MLB-UTR05-E239-AD1-SOPE',denomLocal:'SALA DE OPERAÇÃO',equip:'10199531',abc:'C',denom:'AR CONDICIONADO 002',plano:'9066',desc:'F_7D_ELETMEC_INSPEÇÃO SEMANAL DE CLIMATIZAÇÃO',ordem:'5304049',pdfSAP:'',relatorio:''},
  {centro:'ELETMEC',local:'MLB-UTR05-E239-AD1-SOPE',denomLocal:'SALA DE OPERAÇÃO',equip:'10199532',abc:'C',denom:'AR CONDICIONADO 003',plano:'9066',desc:'F_7D_ELETMEC_INSPEÇÃO SEMANAL DE CLIMATIZAÇÃO',ordem:'5304163',pdfSAP:'',relatorio:''},
  {centro:'ELETMEC',local:'MLB-UTR05-E239-AD1-REFE',denomLocal:'REFEITÓRIO',equip:'10199533',abc:'C',denom:'AR CONDICIONADO 004',plano:'9066',desc:'F_7D_ELETMEC_INSPEÇÃO SEMANAL DE CLIMATIZAÇÃO',ordem:'5304257',pdfSAP:'',relatorio:''},
  {centro:'ELETMEC',local:'MLB-UTR05-E239-AD1-SMAE',denomLocal:'SALA DE APOIO A MAMÃE',equip:'10199534',abc:'C',denom:'AR CONDICIONADO 005',plano:'9066',desc:'F_7D_ELETMEC_INSPEÇÃO SEMANAL DE CLIMATIZAÇÃO',ordem:'5304371',pdfSAP:'',relatorio:''},
  {centro:'ELETMEC',local:'MLB-UTR05-E239-AD1-SMON',denomLocal:'SALA DE MONITORAMENTO',equip:'10199535',abc:'C',denom:'AR CONDICIONADO 006',plano:'9066',desc:'F_7D_ELETMEC_INSPEÇÃO SEMANAL DE CLIMATIZAÇÃO',ordem:'5304485',pdfSAP:'',relatorio:''},
  {centro:'ELETMEC',local:'MLB-UTR05-E239-AD1-SCPD',denomLocal:'SALA CPD',equip:'10199536',abc:'A',denom:'AR CONDICIONADO MS.12-ML_SSP20',plano:'9066',desc:'F_7D_ELETMEC_INSPEÇÃO SEMANAL DE CLIMATIZAÇÃO',ordem:'5304599',pdfSAP:'',relatorio:''},
  {centro:'ELETMEC',local:'MLB-UTR05-E239-AD1-SCPD',denomLocal:'SALA CPD',equip:'10199537',abc:'A',denom:'AR CONDICIONADO MS.13-ML_SSP20',plano:'9066',desc:'F_7D_ELETMEC_INSPEÇÃO SEMANAL DE CLIMATIZAÇÃO',ordem:'5304693',pdfSAP:'',relatorio:''},
  {centro:'ELETMEC',local:'MLB-UTR05-E239-AD1-SRC1',denomLocal:'SALA DE REUNIÃO COLETIVA 1',equip:'10199538',abc:'C',denom:'AR CONDICIONADO 009',plano:'9066',desc:'F_7D_ELETMEC_INSPEÇÃO SEMANAL DE CLIMATIZAÇÃO',ordem:'5304807',pdfSAP:'',relatorio:''},
  {centro:'ELETMEC',local:'MLB-UTR05-E239-AD1-SRC2',denomLocal:'SALA DE REUNIÃO COLETIVA 2',equip:'10199539',abc:'C',denom:'AR CONDICIONADO 010',plano:'9066',desc:'F_7D_ELETMEC_INSPEÇÃO SEMANAL DE CLIMATIZAÇÃO',ordem:'5304921',pdfSAP:'',relatorio:''},
  {centro:'ELETMEC',local:'MLB-UTR05-E239-AD1-SRC3',denomLocal:'SALA DE REUNIÃO COLETIVA 3',equip:'10199540',abc:'C',denom:'AR CONDICIONADO 011',plano:'9066',desc:'F_7D_ELETMEC_INSPEÇÃO SEMANAL DE CLIMATIZAÇÃO',ordem:'5305015',pdfSAP:'',relatorio:''},
  {centro:'ELETMEC',local:'MLB-UTR05-E239-AD1-SRC3',denomLocal:'SALA DE REUNIÃO COLETIVA 3',equip:'10199541',abc:'C',denom:'AR CONDICIONADO 012',plano:'9066',desc:'F_7D_ELETMEC_INSPEÇÃO SEMANAL DE CLIMATIZAÇÃO',ordem:'5305129',pdfSAP:'',relatorio:''},
  {centro:'ELETMEC',local:'MLB-UTR05-E239-MOT-SMOT',denomLocal:'SALA APOIO AO MOTORISTA',equip:'10199542',abc:'C',denom:'AR CONDICIONADO 013',plano:'9066',desc:'F_7D_ELETMEC_INSPEÇÃO SEMANAL DE CLIMATIZAÇÃO',ordem:'5305243',pdfSAP:'',relatorio:''},
  {centro:'ELETMEC',local:'MLB-UTR05-E239-AD1-SOPE',denomLocal:'SALA DE OPERAÇÃO',equip:'10199530',abc:'C',denom:'AR CONDICIONADO 001',plano:'9067',desc:'P_1M_ELETMEC_PREVENTIVA MENSAL DE CLIMATIZAÇÃO',ordem:'5305445',pdfSAP:'',relatorio:''},
  {centro:'ELETMEC',local:'MLB-UTR05-E239-AD1-SOPE',denomLocal:'SALA DE OPERAÇÃO',equip:'10199531',abc:'C',denom:'AR CONDICIONADO 002',plano:'9067',desc:'P_1M_ELETMEC_PREVENTIVA MENSAL DE CLIMATIZAÇÃO',ordem:'5305487',pdfSAP:'',relatorio:''},
  {centro:'ELETMEC',local:'MLB-UTR05-E239-AD1-SOPE',denomLocal:'SALA DE OPERAÇÃO',equip:'10199532',abc:'C',denom:'AR CONDICIONADO 003',plano:'9067',desc:'P_1M_ELETMEC_PREVENTIVA MENSAL DE CLIMATIZAÇÃO',ordem:'5305509',pdfSAP:'',relatorio:''},
  {centro:'ELETMEC',local:'MLB-UTR05-E239-AD1-REFE',denomLocal:'REFEITÓRIO',equip:'10199533',abc:'C',denom:'AR CONDICIONADO 004',plano:'9067',desc:'P_1M_ELETMEC_PREVENTIVA MENSAL DE CLIMATIZAÇÃO',ordem:'5305611',pdfSAP:'',relatorio:''},
  {centro:'ELETMEC',local:'MLB-UTR05-E239-AD1-SMAE',denomLocal:'SALA DE APOIO A MAMÃE',equip:'10199534',abc:'C',denom:'AR CONDICIONADO 005',plano:'9067',desc:'P_1M_ELETMEC_PREVENTIVA MENSAL DE CLIMATIZAÇÃO',ordem:'5305713',pdfSAP:'',relatorio:''},
  {centro:'ELETMEC',local:'MLB-UTR05-E239-AD1-SMON',denomLocal:'SALA DE MONITORAMENTO',equip:'10199535',abc:'C',denom:'AR CONDICIONADO 006',plano:'9067',desc:'P_1M_ELETMEC_PREVENTIVA MENSAL DE CLIMATIZAÇÃO',ordem:'5305775',pdfSAP:'',relatorio:''},
  {centro:'ELETMEC',local:'MLB-UTR05-E239-AD1-SCPD',denomLocal:'SALA CPD',equip:'10199536',abc:'A',denom:'AR CONDICIONADO MS.12-ML_SSP20',plano:'9067',desc:'P_1M_ELETMEC_PREVENTIVA MENSAL DE CLIMATIZAÇÃO',ordem:'5305837',pdfSAP:'',relatorio:''},
  {centro:'ELETMEC',local:'MLB-UTR05-E239-AD1-SCPD',denomLocal:'SALA CPD',equip:'10199537',abc:'A',denom:'AR CONDICIONADO MS.13-ML_SSP20',plano:'9067',desc:'P_1M_ELETMEC_PREVENTIVA MENSAL DE CLIMATIZAÇÃO',ordem:'5305859',pdfSAP:'',relatorio:''},
  {centro:'ELETMEC',local:'MLB-UTR05-E239-AD1-SRC1',denomLocal:'SALA DE REUNIÃO COLETIVA 1',equip:'10199538',abc:'C',denom:'AR CONDICIONADO 009',plano:'9067',desc:'P_1M_ELETMEC_PREVENTIVA MENSAL DE CLIMATIZAÇÃO',ordem:'5305921',pdfSAP:'',relatorio:''},
  {centro:'ELETMEC',local:'MLB-UTR05-E239-AD1-SRC2',denomLocal:'SALA DE REUNIÃO COLETIVA 2',equip:'10199539',abc:'C',denom:'AR CONDICIONADO 010',plano:'9067',desc:'P_1M_ELETMEC_PREVENTIVA MENSAL DE CLIMATIZAÇÃO',ordem:'5305963',pdfSAP:'',relatorio:''},
  {centro:'ELETMEC',local:'MLB-UTR05-E239-AD1-SRC3',denomLocal:'SALA DE REUNIÃO COLETIVA 3',equip:'10199540',abc:'C',denom:'AR CONDICIONADO 011',plano:'9067',desc:'P_1M_ELETMEC_PREVENTIVA MENSAL DE CLIMATIZAÇÃO',ordem:'5306025',pdfSAP:'',relatorio:''},
  {centro:'ELETMEC',local:'MLB-UTR05-E239-AD1-SRC3',denomLocal:'SALA DE REUNIÃO COLETIVA 3',equip:'10199541',abc:'C',denom:'AR CONDICIONADO 012',plano:'9067',desc:'P_1M_ELETMEC_PREVENTIVA MENSAL DE CLIMATIZAÇÃO',ordem:'5306067',pdfSAP:'',relatorio:''},
  {centro:'ELETMEC',local:'MLB-UTR05-E239-MOT-SMOT',denomLocal:'SALA APOIO AO MOTORISTA',equip:'10199542',abc:'C',denom:'AR CONDICIONADO 013',plano:'9067',desc:'P_1M_ELETMEC_PREVENTIVA MENSAL DE CLIMATIZAÇÃO',ordem:'5306129',pdfSAP:'',relatorio:''},
  {centro:'ELETMEC',local:'MLB-UTR05-E239-AD1-SOPE',denomLocal:'SALA DE OPERAÇÃO',equip:'10199530',abc:'C',denom:'AR CONDICIONADO 001',plano:'9068',desc:'P_6M_ELETMEC_PREVENTIVA SEMESTRAL DE CLIMATIZAÇÃO',ordem:'5305265',pdfSAP:'',relatorio:''},
  {centro:'ELETMEC',local:'MLB-UTR05-E239-AD1-SOPE',denomLocal:'SALA DE OPERAÇÃO',equip:'10199531',abc:'C',denom:'AR CONDICIONADO 002',plano:'9068',desc:'P_6M_ELETMEC_PREVENTIVA SEMESTRAL DE CLIMATIZAÇÃO',ordem:'5305270',pdfSAP:'',relatorio:''},
  {centro:'ELETMEC',local:'MLB-UTR05-E239-AD1-SOPE',denomLocal:'SALA DE OPERAÇÃO',equip:'10199532',abc:'C',denom:'AR CONDICIONADO 003',plano:'9068',desc:'P_6M_ELETMEC_PREVENTIVA SEMESTRAL DE CLIMATIZAÇÃO',ordem:'5305275',pdfSAP:'',relatorio:''},
  {centro:'ELETMEC',local:'MLB-UTR05-E239-AD1-REFE',denomLocal:'REFEITÓRIO',equip:'10199533',abc:'C',denom:'AR CONDICIONADO 004',plano:'9068',desc:'P_6M_ELETMEC_PREVENTIVA SEMESTRAL DE CLIMATIZAÇÃO',ordem:'5305280',pdfSAP:'',relatorio:''},
  {centro:'ELETMEC',local:'MLB-UTR05-E239-AD1-SMAE',denomLocal:'SALA DE APOIO A MAMÃE',equip:'10199534',abc:'C',denom:'AR CONDICIONADO 005',plano:'9068',desc:'P_6M_ELETMEC_PREVENTIVA SEMESTRAL DE CLIMATIZAÇÃO',ordem:'5305285',pdfSAP:'',relatorio:''},
  {centro:'ELETMEC',local:'MLB-UTR05-E239-AD1-SMON',denomLocal:'SALA DE MONITORAMENTO',equip:'10199535',abc:'C',denom:'AR CONDICIONADO 006',plano:'9068',desc:'P_6M_ELETMEC_PREVENTIVA SEMESTRAL DE CLIMATIZAÇÃO',ordem:'5305290',pdfSAP:'',relatorio:''},
  {centro:'ELETMEC',local:'MLB-UTR05-E239-AD1-SCPD',denomLocal:'SALA CPD',equip:'10199536',abc:'A',denom:'AR CONDICIONADO MS.12-ML_SSP20',plano:'9068',desc:'P_6M_ELETMEC_PREVENTIVA SEMESTRAL DE CLIMATIZAÇÃO',ordem:'5305295',pdfSAP:'',relatorio:''},
  {centro:'ELETMEC',local:'MLB-UTR05-E239-AD1-SCPD',denomLocal:'SALA CPD',equip:'10199537',abc:'A',denom:'AR CONDICIONADO MS.13-ML_SSP20',plano:'9068',desc:'P_6M_ELETMEC_PREVENTIVA SEMESTRAL DE CLIMATIZAÇÃO',ordem:'5305300',pdfSAP:'',relatorio:''},
  {centro:'ELETMEC',local:'MLB-UTR05-E239-AD1-SRC1',denomLocal:'SALA DE REUNIÃO COLETIVA 1',equip:'10199538',abc:'C',denom:'AR CONDICIONADO 009',plano:'9068',desc:'P_6M_ELETMEC_PREVENTIVA SEMESTRAL DE CLIMATIZAÇÃO',ordem:'5305305',pdfSAP:'',relatorio:''},
  {centro:'ELETMEC',local:'MLB-UTR05-E239-AD1-SRC2',denomLocal:'SALA DE REUNIÃO COLETIVA 2',equip:'10199539',abc:'C',denom:'AR CONDICIONADO 010',plano:'9068',desc:'P_6M_ELETMEC_PREVENTIVA SEMESTRAL DE CLIMATIZAÇÃO',ordem:'5305310',pdfSAP:'',relatorio:''},
  {centro:'ELETMEC',local:'MLB-UTR05-E239-AD1-SRC3',denomLocal:'SALA DE REUNIÃO COLETIVA 3',equip:'10199540',abc:'C',denom:'AR CONDICIONADO 011',plano:'9068',desc:'P_6M_ELETMEC_PREVENTIVA SEMESTRAL DE CLIMATIZAÇÃO',ordem:'5305315',pdfSAP:'',relatorio:''},
  {centro:'ELETMEC',local:'MLB-UTR05-E239-AD1-SRC3',denomLocal:'SALA DE REUNIÃO COLETIVA 3',equip:'10199541',abc:'C',denom:'AR CONDICIONADO 012',plano:'9068',desc:'P_6M_ELETMEC_PREVENTIVA SEMESTRAL DE CLIMATIZAÇÃO',ordem:'5305320',pdfSAP:'',relatorio:''},
  {centro:'ELETMEC',local:'MLB-UTR05-E239-MOT-SMOT',denomLocal:'SALA APOIO AO MOTORISTA',equip:'10199542',abc:'C',denom:'AR CONDICIONADO 013',plano:'9068',desc:'P_6M_ELETMEC_PREVENTIVA SEMESTRAL DE CLIMATIZAÇÃO',ordem:'5305325',pdfSAP:'',relatorio:''},
  {centro:'ELETMEC',local:'MLB-UTR05-E239-UTI-BOMB',denomLocal:'SISTEMA DE INCÊNDIO',equip:'10199595',abc:'A',denom:'SISTEMA DE INCÊNDIO',plano:'9069',desc:'F_3M_ELETMEC_PREVENTIVA TRIMESTRAL SISTEMA INCÊNDIO',ordem:'5305336',pdfSAP:'',relatorio:''},
  {centro:'ELETMEC',local:'MLB-UTR05-E239-UTI-GERE',denomLocal:'GERADORES DE ENERGIA',equip:'10199523',abc:'A',denom:'GERADOR DE ENERGIA',plano:'9070',desc:'F_7D_ELETMEC_INSPEÇÃO DE GERADOR',ordem:'5305430',pdfSAP:'',relatorio:''},
  {centro:'ELETMEC',local:'MLB-UTR05-E239-UTI-GERE',denomLocal:'GERADORES DE ENERGIA',equip:'10199523',abc:'A',denom:'GERADOR DE ENERGIA',plano:'9071',desc:'F_1M_ELETMEC_INSPEÇÃO MENSAL DE GERADOR',ordem:'5305472',pdfSAP:'',relatorio:''},
  {centro:'ELETMEC',local:'MLB-UTR05-E239-OUT-DOCA',denomLocal:'DOCAS',equip:'10199513',abc:'A',denom:'NIVELADORA DA DOCA 002',plano:'9072',desc:'F_7D_ELETMEC_INSPEÇÃO SEMANAL DE NIVELADORA',ordem:'5305626',pdfSAP:'',relatorio:''},
  {centro:'ELETMEC',local:'MLB-UTR05-E239-INB-DOCA',denomLocal:'DOCAS',equip:'10199511',abc:'A',denom:'NIVELADORA DA DOCA 001',plano:'9072',desc:'F_7D_ELETMEC_INSPEÇÃO SEMANAL DE NIVELADORA',ordem:'5305740',pdfSAP:'',relatorio:''},
  {centro:'ELETMEC',local:'MLB-UTR05-E239-OUT-DOCA',denomLocal:'DOCAS',equip:'10199513',abc:'A',denom:'NIVELADORA DA DOCA 002',plano:'9073',desc:'P_1M_ELETMEC_PREVENTIVA MENSAL DE NIVELADORA',ordem:'5305782',pdfSAP:'',relatorio:''},
  {centro:'ELETMEC',local:'MLB-UTR05-E239-INB-DOCA',denomLocal:'DOCAS',equip:'10199511',abc:'A',denom:'NIVELADORA DA DOCA 001',plano:'9073',desc:'P_1M_ELETMEC_PREVENTIVA MENSAL DE NIVELADORA',ordem:'5305804',pdfSAP:'',relatorio:''},
  {centro:'ELETMEC',local:'MLB-UTR05-E239-OUT-DOCA',denomLocal:'DOCAS',equip:'10199513',abc:'A',denom:'NIVELADORA DA DOCA 002',plano:'9074',desc:'P_6M_ELETMEC_PREVENTIVA SEMESTRAL DE NIVELADORA',ordem:'5305808',pdfSAP:'',relatorio:''},
  {centro:'ELETMEC',local:'MLB-UTR05-E239-INB-DOCA',denomLocal:'DOCAS',equip:'10199511',abc:'A',denom:'NIVELADORA DA DOCA 001',plano:'9074',desc:'P_6M_ELETMEC_PREVENTIVA SEMESTRAL DE NIVELADORA',ordem:'5305812',pdfSAP:'',relatorio:''},
  {centro:'ELETMEC',local:'MLB-UTR05-E239-STR-ESTE',denomLocal:'ESTEIRAS TRANSPORTADORAS',equip:'10199514',abc:'A',denom:'QUADRO DE BAIXA TENSÃO 001',plano:'9075',desc:'P_1M_ELETMEC_PREVENTIVA MENSAL PAINEIS ELÉTRICOS',ordem:'5305894',pdfSAP:'',relatorio:''},
  {centro:'ELETMEC',local:'MLB-UTR05-E239-UTI-GERE',denomLocal:'GERADORES DE ENERGIA',equip:'10199522',abc:'A',denom:'QUADRO GERAL DE BAIXA TENSÃO 002',plano:'9075',desc:'P_1M_ELETMEC_PREVENTIVA MENSAL PAINEIS ELÉTRICOS',ordem:'5305916',pdfSAP:'',relatorio:''},
  {centro:'ELETMEC',local:'MLB-UTR05-E239-ADO-ELET',denomLocal:'INFRA ESTRUTURA ELÉTRICA',equip:'10199525',abc:'A',denom:'QUADRO GERAL DE BAIXA TENSÃO 003',plano:'9075',desc:'P_1M_ELETMEC_PREVENTIVA MENSAL PAINEIS ELÉTRICOS',ordem:'5305958',pdfSAP:'',relatorio:''},
  {centro:'ELETMEC',local:'MLB-UTR05-E239-ADO-ELET',denomLocal:'INFRA ESTRUTURA ELÉTRICA',equip:'10199526',abc:'A',denom:'QUADRO GERAL DE BAIXA TENSÃO 004',plano:'9075',desc:'P_1M_ELETMEC_PREVENTIVA MENSAL PAINEIS ELÉTRICOS',ordem:'5306000',pdfSAP:'',relatorio:''},
  {centro:'ELETMEC',local:'MLB-UTR05-E239-ADO-ELET',denomLocal:'INFRA ESTRUTURA ELÉTRICA',equip:'10199527',abc:'A',denom:'QUADRO GERAL DE BAIXA TENSÃO 005',plano:'9075',desc:'P_1M_ELETMEC_PREVENTIVA MENSAL PAINEIS ELÉTRICOS',ordem:'5306042',pdfSAP:'',relatorio:''},
  {centro:'ELETMEC',local:'MLB-UTR05-E239-ADO-ELET',denomLocal:'INFRA ESTRUTURA ELÉTRICA',equip:'10199528',abc:'A',denom:'QUADRO GERAL DE BAIXA TENSÃO 006',plano:'9075',desc:'P_1M_ELETMEC_PREVENTIVA MENSAL PAINEIS ELÉTRICOS',ordem:'5306084',pdfSAP:'',relatorio:''},
  {centro:'ELETMEC',local:'MLB-UTR05-E239-ADO-ELET',denomLocal:'INFRA ESTRUTURA ELÉTRICA',equip:'10199529',abc:'A',denom:'QUADRO GERAL DE BAIXA TENSÃO 007',plano:'9075',desc:'P_1M_ELETMEC_PREVENTIVA MENSAL PAINEIS ELÉTRICOS',ordem:'5306106',pdfSAP:'',relatorio:''},
  {centro:'ELETMEC',local:'MLB-UTR05-E239-STR-ESTE',denomLocal:'ESTEIRAS TRANSPORTADORAS',equip:'10199515',abc:'A',denom:'ESTEIRA TRANSPORTADORA RETA MÓDULO 01',plano:'9076',desc:'F_7D_ELETMEC_INSPEÇÃO SEMANAL DE ESTEIRAS',ordem:'5307161',pdfSAP:'https://drive.google.com/drive/folders/1lYpW6zXQcDiXWu9EOkVoxeR8jZskG694',relatorio:''},
  {centro:'ELETMEC',local:'MLB-UTR05-E239-STR-ESTE',denomLocal:'ESTEIRAS TRANSPORTADORAS',equip:'10199516',abc:'A',denom:'ESTEIRA TRANSPORTADORA RETA MÓDULO 02',plano:'9076',desc:'F_7D_ELETMEC_INSPEÇÃO SEMANAL DE ESTEIRAS',ordem:'5307975',pdfSAP:'https://drive.google.com/drive/folders/1lYpW6zXQcDiXWu9EOkVoxeR8jZskG694',relatorio:''},
  {centro:'ELETMEC',local:'MLB-UTR05-E239-STR-ESTE',denomLocal:'ESTEIRAS TRANSPORTADORAS',equip:'10199517',abc:'A',denom:'ESTEIRA TRANSPORTADORA RETA MÓDULO 03',plano:'9076',desc:'F_7D_ELETMEC_INSPEÇÃO SEMANAL DE ESTEIRAS',ordem:'5308269',pdfSAP:'https://drive.google.com/drive/folders/1lYpW6zXQcDiXWu9EOkVoxeR8jZskG694',relatorio:''},
  {centro:'ELETMEC',local:'MLB-UTR05-E239-STR-ESTE',denomLocal:'ESTEIRAS TRANSPORTADORAS',equip:'10199518',abc:'A',denom:'ESTEIRA TRANSPORTADORA RETA MÓDULO 04',plano:'9076',desc:'F_7D_ELETMEC_INSPEÇÃO SEMANAL DE ESTEIRAS',ordem:'5308363',pdfSAP:'https://drive.google.com/drive/folders/1lYpW6zXQcDiXWu9EOkVoxeR8jZskG694',relatorio:''},
  {centro:'ELETMEC',local:'MLB-UTR05-E239-STR-ESTE',denomLocal:'ESTEIRAS TRANSPORTADORAS',equip:'10199519',abc:'A',denom:'ESTEIRA TRANSPORTADORA RETA MÓDULO 05',plano:'9076',desc:'F_7D_ELETMEC_INSPEÇÃO SEMANAL DE ESTEIRAS',ordem:'5308458',pdfSAP:'https://drive.google.com/drive/folders/1lYpW6zXQcDiXWu9EOkVoxeR8jZskG694',relatorio:''},
  {centro:'ELETMEC',local:'MLB-UTR05-E239-STR-ESTE',denomLocal:'ESTEIRAS TRANSPORTADORAS',equip:'10199515',abc:'A',denom:'ESTEIRA TRANSPORTADORA RETA MÓDULO 01',plano:'9077',desc:'P_1M_ELETMEC_PREVENTIVA MENSAL PERIÓDICA DE ESTEIRAS',ordem:'5306151',pdfSAP:'',relatorio:''},
  {centro:'ELETMEC',local:'MLB-UTR05-E239-STR-ESTE',denomLocal:'ESTEIRAS TRANSPORTADORAS',equip:'10199516',abc:'A',denom:'ESTEIRA TRANSPORTADORA RETA MÓDULO 02',plano:'9077',desc:'P_1M_ELETMEC_PREVENTIVA MENSAL PERIÓDICA DE ESTEIRAS',ordem:'5306173',pdfSAP:'',relatorio:''},
  {centro:'ELETMEC',local:'MLB-UTR05-E239-STR-ESTE',denomLocal:'ESTEIRAS TRANSPORTADORAS',equip:'10199517',abc:'A',denom:'ESTEIRA TRANSPORTADORA RETA MÓDULO 03',plano:'9077',desc:'P_1M_ELETMEC_PREVENTIVA MENSAL PERIÓDICA DE ESTEIRAS',ordem:'5306195',pdfSAP:'',relatorio:''},
  {centro:'ELETMEC',local:'MLB-UTR05-E239-STR-ESTE',denomLocal:'ESTEIRAS TRANSPORTADORAS',equip:'10199518',abc:'A',denom:'ESTEIRA TRANSPORTADORA RETA MÓDULO 04',plano:'9077',desc:'P_1M_ELETMEC_PREVENTIVA MENSAL PERIÓDICA DE ESTEIRAS',ordem:'5306217',pdfSAP:'',relatorio:''},
  {centro:'ELETMEC',local:'MLB-UTR05-E239-STR-ESTE',denomLocal:'ESTEIRAS TRANSPORTADORAS',equip:'10199519',abc:'A',denom:'ESTEIRA TRANSPORTADORA RETA MÓDULO 05',plano:'9077',desc:'P_1M_ELETMEC_PREVENTIVA MENSAL PERIÓDICA DE ESTEIRAS',ordem:'5306239',pdfSAP:'',relatorio:''},
  {centro:'ELETMEC',local:'MLB-UTR05-E239-ADO-ELET',denomLocal:'INFRA ESTRUTURA ELÉTRICA',equip:'',abc:'',denom:'ILUMINAÇÃO GERAL',plano:'9078',desc:'F_1M_ELETMEC_INSPEÇÃO MENSAL ILUMINAÇÃO',ordem:'5306261',pdfSAP:'',relatorio:''},
  {centro:'ELETMEC',local:'MLB-UTR05-E239-INB-DOCA',denomLocal:'DOCAS',equip:'10199510',abc:'A',denom:'PORTA DA DOCA 001',plano:'9079',desc:'F_1M_ELETMEC_INSPEÇÃO MENSAL PORTAS E PORTÕES',ordem:'5306283',pdfSAP:'',relatorio:''},
  {centro:'ELETMEC',local:'MLB-UTR05-E239-OUT-DOCA',denomLocal:'DOCAS',equip:'10199512',abc:'A',denom:'PORTA DA DOCA 002',plano:'9079',desc:'F_1M_ELETMEC_INSPEÇÃO MENSAL PORTAS E PORTÕES',ordem:'5306305',pdfSAP:'',relatorio:''},
  {centro:'ELETMEC',local:'MLB-UTR05-E239-INB-DOCA',denomLocal:'DOCAS',equip:'10199510',abc:'A',denom:'PORTA DA DOCA 001',plano:'9080',desc:'P_6M_ELETMEC_INSP SEMESTRAL PORTA DOCA',ordem:'5306309',pdfSAP:'',relatorio:''},
  {centro:'ELETMEC',local:'MLB-UTR05-E239-OUT-DOCA',denomLocal:'DOCAS',equip:'10199512',abc:'A',denom:'PORTA DA DOCA 002',plano:'9080',desc:'P_6M_ELETMEC_INSP SEMESTRAL PORTA DOCA',ordem:'5306313',pdfSAP:'',relatorio:''},
  {centro:'OFC_01',local:'MLB-UTR05-E239-UTI-BOMB',denomLocal:'SISTEMA DE INCÊNDIO',equip:'10199595',abc:'A',denom:'SISTEMA DE INCÊNDIO',plano:'9232',desc:'F_1M_OFC_01_INSPEÇÃO MENSAL SISTEMA DE INCÊNDIO',ordem:'5326100',pdfSAP:'',relatorio:''},
  {centro:'OFC_01',local:'MLB-UTR05-E239-ADO-ELET',denomLocal:'INFRA ESTRUTURA ELÉTRICA',equip:'',abc:'',denom:'INFRA ESTRUTURA ELÉTRICA',plano:'9233',desc:'F_1M_OFC_01_INSPEÇÃO MENSAL PREDIAL SITE',ordem:'5326142',pdfSAP:'',relatorio:''},
  {centro:'OFC_01',local:'MLB-UTR05-E239-STR-RECE',denomLocal:'RECEBIMENTO',equip:'10199575',abc:'C',denom:'FLOWRACK 001',plano:'9234',desc:'F_15D_OFC_01_INSPEÇÃO QUINZENAL GAIOLAS',ordem:'5326549',pdfSAP:'',relatorio:''},
  {centro:'OFC_01',local:'MLB-UTR05-E239-STR-RECE',denomLocal:'RECEBIMENTO',equip:'10199576',abc:'C',denom:'FLOWRACK 002',plano:'9234',desc:'F_15D_OFC_01_INSPEÇÃO QUINZENAL GAIOLAS',ordem:'5326696',pdfSAP:'',relatorio:''},
  {centro:'OFC_01',local:'MLB-UTR05-E239-STR-RECE',denomLocal:'RECEBIMENTO',equip:'10199577',abc:'C',denom:'FLOWRACK 003',plano:'9234',desc:'F_15D_OFC_01_INSPEÇÃO QUINZENAL GAIOLAS',ordem:'5326783',pdfSAP:'',relatorio:''},
  {centro:'OFC_01',local:'MLB-UTR05-E239-STR-RECE',denomLocal:'RECEBIMENTO',equip:'10199578',abc:'C',denom:'FLOWRACK 004',plano:'9234',desc:'F_15D_OFC_01_INSPEÇÃO QUINZENAL GAIOLAS',ordem:'5326850',pdfSAP:'',relatorio:''},
  {centro:'OFC_01',local:'MLB-UTR05-E239-STR-RECE',denomLocal:'RECEBIMENTO',equip:'10199579',abc:'C',denom:'FLOWRACK 005',plano:'9234',desc:'F_15D_OFC_01_INSPEÇÃO QUINZENAL GAIOLAS',ordem:'5326897',pdfSAP:'',relatorio:''},
  {centro:'OFC_01',local:'MLB-UTR05-E239-STR-RECE',denomLocal:'RECEBIMENTO',equip:'10199580',abc:'C',denom:'FLOWRACK 006',plano:'9234',desc:'F_15D_OFC_01_INSPEÇÃO QUINZENAL GAIOLAS',ordem:'5326984',pdfSAP:'',relatorio:''},
  {centro:'OFC_01',local:'MLB-UTR05-E239-STR-RECE',denomLocal:'RECEBIMENTO',equip:'10199581',abc:'C',denom:'FLOWRACK 007',plano:'9234',desc:'F_15D_OFC_01_INSPEÇÃO QUINZENAL GAIOLAS',ordem:'5327131',pdfSAP:'',relatorio:''},
  {centro:'OFC_01',local:'MLB-UTR05-E239-STR-RECE',denomLocal:'RECEBIMENTO',equip:'10199582',abc:'C',denom:'FLOWRACK 008',plano:'9234',desc:'F_15D_OFC_01_INSPEÇÃO QUINZENAL GAIOLAS',ordem:'5327278',pdfSAP:'',relatorio:''},
  {centro:'OFC_01',local:'MLB-UTR05-E239-STR-RECE',denomLocal:'RECEBIMENTO',equip:'10199583',abc:'C',denom:'FLOWRACK 009',plano:'9234',desc:'F_15D_OFC_01_INSPEÇÃO QUINZENAL GAIOLAS',ordem:'5327385',pdfSAP:'',relatorio:''},
  {centro:'OFC_01',local:'MLB-UTR05-E239-STR-RECE',denomLocal:'RECEBIMENTO',equip:'10199584',abc:'C',denom:'FLOWRACK 010',plano:'9234',desc:'F_15D_OFC_01_INSPEÇÃO QUINZENAL GAIOLAS',ordem:'5327452',pdfSAP:'',relatorio:''},
  {centro:'OFC_01',local:'MLB-UTR05-E239-STR-RECE',denomLocal:'RECEBIMENTO',equip:'10199585',abc:'C',denom:'FLOWRACK 011',plano:'9234',desc:'F_15D_OFC_01_INSPEÇÃO QUINZENAL GAIOLAS',ordem:'5327499',pdfSAP:'',relatorio:''},
  {centro:'OFC_01',local:'MLB-UTR05-E239-STR-RECE',denomLocal:'RECEBIMENTO',equip:'10199586',abc:'C',denom:'FLOWRACK 012',plano:'9234',desc:'F_15D_OFC_01_INSPEÇÃO QUINZENAL GAIOLAS',ordem:'5327686',pdfSAP:'',relatorio:''},
  {centro:'OFC_01',local:'MLB-UTR05-E239-STR-RECE',denomLocal:'RECEBIMENTO',equip:'10199587',abc:'C',denom:'FLOWRACK 013',plano:'9234',desc:'F_15D_OFC_01_INSPEÇÃO QUINZENAL GAIOLAS',ordem:'5327773',pdfSAP:'',relatorio:''},
  {centro:'OFC_01',local:'MLB-UTR05-E239-STR-RECE',denomLocal:'RECEBIMENTO',equip:'10199588',abc:'C',denom:'FLOWRACK 014',plano:'9234',desc:'F_15D_OFC_01_INSPEÇÃO QUINZENAL GAIOLAS',ordem:'5327820',pdfSAP:'',relatorio:''},
  {centro:'OFC_01',local:'MLB-UTR05-E239-STR-RECE',denomLocal:'RECEBIMENTO',equip:'10199589',abc:'C',denom:'FLOWRACK 015',plano:'9234',desc:'F_15D_OFC_01_INSPEÇÃO QUINZENAL GAIOLAS',ordem:'5327907',pdfSAP:'',relatorio:''},
  {centro:'OFC_01',local:'MLB-UTR05-E239-STR-RECE',denomLocal:'RECEBIMENTO',equip:'10199590',abc:'C',denom:'FLOWRACK 016',plano:'9234',desc:'F_15D_OFC_01_INSPEÇÃO QUINZENAL GAIOLAS',ordem:'5328074',pdfSAP:'',relatorio:''},
];

// Categorias Sorocaba em ordem alfabética
const SOROCABA_CATS = [
  { nome: 'AR CONDICIONADO',    icon: '❄️',  keywords: ['AR CONDICIONADO','CLIMAT'] },
  { nome: 'ESTEIRA',            icon: '🔄',  keywords: ['ESTEIRA TRANSPORTADORA'] },
  { nome: 'FLOWRACK',           icon: '📦',  keywords: ['FLOWRACK','GAIOLA'] },
  { nome: 'GERADOR DE ENERGIA', icon: '⚡',  keywords: ['GERADOR'] },
  { nome: 'NIVELADORA',         icon: '🔩',  keywords: ['NIVELADORA'] },
  { nome: 'PORTA DA DOCA',      icon: '🚪',  keywords: ['PORTA DA DOCA','PORTÃO'] },
  { nome: 'QUADRO ELÉTRICO',    icon: '🗂️', keywords: ['QUADRO','PAINEL','BAIXA TENSÃO'] },
  { nome: 'SISTEMA DE INCÊNDIO',icon: '🔥',  keywords: ['INCÊNDIO','INCENDIO'] },
  { nome: 'TELHADO',            icon: '🏠',  keywords: ['TELHADO'] },
];

function matchCat(item, cat) {
  const haystack = (item.denom + ' ' + item.desc + ' ' + item.denomLocal).toUpperCase();
  return cat.keywords.some(k => haystack.includes(k));
}

function renderSorocaba() {
  const grid = document.getElementById('equipGrid');
  grid.innerHTML = SOROCABA_CATS.map(cat => {
    const items = DATA.filter(d => matchCat(d, cat));
    // Para ESTEIRA, conta equipamentos únicos (não planos)
    const displayCount = cat.nome === 'ESTEIRA'
      ? new Set(items.map(d => d.equip).filter(Boolean)).size
      : items.length;
    const nao = items.filter(d => getStatusItem(d) === 'nao').length;
    const and = items.filter(d => getStatusItem(d) === 'and').length;
    const con = items.filter(d => getStatusItem(d) === 'con').length;
    return `
    <div class="equip-card" onclick="filtrarPorCategoria('${cat.nome}')">
      <div class="equip-icon">${cat.icon}</div>
      <div class="equip-name">${cat.nome}</div>
      <div class="equip-count">
        <span class="n">${displayCount}</span>
        <span class="s">equipamentos</span>
      </div>
      <div class="equip-pills">
        ${nao ? `<span class="equip-pill pill-nao">⏸ ${nao}</span>` : ''}
        ${and ? `<span class="equip-pill pill-and">🔄 ${and}</span>` : ''}
        ${con ? `<span class="equip-pill pill-con">✅ ${con}</span>` : ''}
      </div>
    </div>`;
  }).join('');
}

// ── Estado local ──
const localState = JSON.parse(localStorage.getItem('prev_state') || '{}');
let cmtLogs = {};
let reportsCache = JSON.parse(localStorage.getItem('reportsCache') || '{}');
const APPS_SCRIPT_URL = 'https://script.google.com/a/macros/mercadolivre.com/s/AKfycbw7ZVMG0WYTX9pXWNh71VkQL_sDcW6VaD3eWtVx5nLFTH9SFyPuU_C3qjpXVg9mqPVl/exec';
const BASE_URL = window.location.origin + window.location.pathname.replace('dashboard_preventivas.html','');

function saveState() { localStorage.setItem('prev_state', JSON.stringify(localState)); }
function getStatusItem(item) { return localState[item.ordem]?.status || 'nao'; }

// ── Planos dropdown ──
const planos = [...new Set(DATA.map(d => d.plano))].sort((a,b)=>a-b);
const selPlano = document.getElementById('filtroPlano');
planos.forEach(p => { const o = document.createElement('option'); o.value=p; o.textContent='Plano '+p; selPlano.appendChild(o); });

// ── Navegação ──
let activeTab = 'SOROCABA';
let lastSubCat = null;
let semanaAtiva = null;

const SUB_OPCOES = {
  'ESTEIRA': [
    { nome:'Semanal', icon:'📅', desc:'Inspecao Semanal (Plano 9076)', plano:'9076' },
    { nome:'Mensal',  icon:'🗓️', desc:'Preventiva Mensal (Plano 9077)', plano:'9077' },
  ]
};

const SEMANAS = [
  {num:1,icon:'1️⃣',label:'Semana 1'},
  {num:2,icon:'2️⃣',label:'Semana 2'},
  {num:3,icon:'3️⃣',label:'Semana 3'},
  {num:4,icon:'4️⃣',label:'Semana 4'},
];

function getStatusSemana(semana, ordem) { return localState['SEM_'+semana+'_'+ordem]?.status || 'nao'; }

function setTab(tab) {
  activeTab = tab;
  document.querySelectorAll('.tab').forEach(t => t.classList.toggle('active', t.dataset.tab === tab));
  const isSorocaba = tab === 'SOROCABA';
  document.getElementById('sorocabaView').style.display = isSorocaba ? 'block' : 'none';
  document.getElementById('kanbanView').style.display   = isSorocaba ? 'none'  : 'block';
  document.getElementById('subselView').style.display   = 'none';
  document.getElementById('semanaView').style.display   = 'none';
  document.querySelector('.filters').style.display      = isSorocaba ? 'none'  : 'flex';
  document.querySelector('.kpi-row').style.display      = isSorocaba ? 'none'  : 'flex';
  if (isSorocaba) document.getElementById('btnVoltar').style.display = 'none';
  if (isSorocaba) { document.getElementById('btnVoltarSub').style.display='none'; document.getElementById('btnVoltarSemana').style.display='none'; }
  if (isSorocaba) renderSorocaba(); else render();
}

function ocultarTudo() {
  document.getElementById('sorocabaView').style.display = 'none';
  document.getElementById('subselView').style.display   = 'none';
  document.getElementById('semanaView').style.display   = 'none';
  document.getElementById('kanbanView').style.display   = 'none';
  document.querySelector('.filters').style.display      = 'none';
  document.querySelector('.kpi-row').style.display      = 'none';
}

function filtrarPorCategoria(catNome) {
  const cat = SOROCABA_CATS.find(c => c.nome === catNome);
  if (!cat) return;
  if (SUB_OPCOES[catNome]) { mostrarSubSel(catNome, cat); return; }
  entrarNaLista(cat.keywords[0], '');
}

function mostrarSubSel(catNome, cat) {
  lastSubCat = catNome;
  ocultarTudo();
  document.getElementById('subselView').style.display = 'block';
  document.getElementById('btnVoltar').style.display  = 'flex';
  document.getElementById('subselTitle').textContent  = catNome + ' — Selecione o tipo';
  const grid = document.getElementById('subselGrid');
  grid.innerHTML = SUB_OPCOES[catNome].map(op => {
    const onclick = (catNome==='ESTEIRA' && op.plano==='9076')
      ? `mostrarSemanas('${catNome}')`
      : `entrarNaLista('${cat.keywords[0]}','${op.plano}')`;
    return `<div class="subsel-card" onclick="${onclick}"><div class="subsel-icon">${op.icon}</div><div class="subsel-name">${op.nome}</div><div class="subsel-desc">${op.desc}</div></div>`;
  }).join('');
}

function mostrarSemanas(catNome) {
  lastSubCat = catNome;
  ocultarTudo();
  document.getElementById('semanaView').style.display   = 'block';
  document.getElementById('btnVoltar').style.display    = 'flex';
  document.getElementById('btnVoltarSub').style.display = 'flex';
  document.getElementById('semanaTitle').textContent    = 'ESTEIRA — Selecione a Semana';
  const equips = DATA.filter(d => d.plano==='9076' && d.denom.includes('ESTEIRA TRANSPORTADORA'));
  const grid = document.getElementById('semanaGrid');
  grid.innerHTML = SEMANAS.map(s => {
    const pills = {nao:0,and:0,atra:0,con:0};
    equips.forEach(e => { const st=getStatusSemana(s.num,e.ordem); pills[st]=(pills[st]||0)+1; });
    const pillsHTML = [
      pills.nao  ? `<span class="semana-pill sp-nao">⏸ ${pills.nao}</span>`  : '',
      pills.and  ? `<span class="semana-pill sp-and">🔄 ${pills.and}</span>`  : '',
      pills.atra ? `<span class="semana-pill sp-atra">⚠️ ${pills.atra}</span>` : '',
      pills.con  ? `<span class="semana-pill sp-con">✅ ${pills.con}</span>`   : '',
    ].filter(Boolean).join('');
    return `<div class="semana-card" onclick="entrarNaListaSemana(${s.num})">
      <div class="semana-num">Semana ${s.num}</div><div class="semana-icon">${s.icon}</div>
      <div class="semana-name">${s.label}</div><div class="semana-equip">${equips.length} equipamentos</div>
      <div class="semana-status-bar">${pillsHTML||`<span class="semana-pill sp-nao">⏸ ${equips.length}</span>`}</div>
    </div>`;
  }).join('');
}

function entrarNaLista(keyword, plano) {
  ocultarTudo();
  document.getElementById('kanbanView').style.display  = 'block';
  document.querySelector('.filters').style.display     = 'flex';
  document.querySelector('.kpi-row').style.display     = 'flex';
  document.getElementById('btnVoltar').style.display   = 'flex';
  const btnSub = document.getElementById('btnVoltarSub');
  if (lastSubCat) btnSub.style.display = 'flex'; else btnSub.style.display = 'none';
  activeTab = 'TODOS';
  document.getElementById('search').value = keyword;
  document.getElementById('filtroPlano').value = plano || '';
  render();
}

function entrarNaListaSemana(semana) {
  semanaAtiva = semana;
  ocultarTudo();
  document.getElementById('kanbanView').style.display      = 'block';
  document.querySelector('.kpi-row').style.display         = 'flex';
  document.getElementById('btnVoltar').style.display       = 'flex';
  document.getElementById('btnVoltarSub').style.display    = 'flex';
  document.getElementById('btnVoltarSemana').style.display = 'flex';
  activeTab = 'TODOS';
  document.getElementById('search').value = 'ESTEIRA TRANSPORTADORA';
  document.getElementById('filtroPlano').value = '9076';
  renderSemana(semana);
}

function voltarSemanas() { semanaAtiva=null; ocultarTudo(); document.getElementById('btnVoltarSemana').style.display='none'; mostrarSemanas(lastSubCat); }
function voltarSubSel()  { ocultarTudo(); document.getElementById('btnVoltarSemana').style.display='none'; const cat=SOROCABA_CATS.find(c=>c.nome===lastSubCat); mostrarSubSel(lastSubCat,cat); }
function voltarSorocaba() {
  lastSubCat=null; semanaAtiva=null;
  document.getElementById('search').value=''; document.getElementById('filtroPlano').value='';
  document.getElementById('btnVoltarSub').style.display='none'; document.getElementById('btnVoltarSemana').style.display='none';
  setTab('SOROCABA');
}

// ── Render ──
function render() {
  const search=document.getElementById('search').value.toLowerCase();
  const filtroABC=document.getElementById('filtroABC').value;
  const filtroPlano=document.getElementById('filtroPlano').value;
  let filtered=DATA.filter(d => {
    if (activeTab!=='TODOS' && d.centro!==activeTab) return false;
    if (filtroABC && d.abc!==filtroABC) return false;
    if (filtroPlano && d.plano!==filtroPlano) return false;
    if (search && !d.denom.toLowerCase().includes(search) && !d.desc.toLowerCase().includes(search) && !d.denomLocal.toLowerCase().includes(search)) return false;
    return true;
  });
  const cnts={nao:0,and:0,atra:0,con:0};
  filtered.forEach(d => { const s=getStatusItem(d); cnts[s]=(cnts[s]||0)+1; });
  document.getElementById('kpi-nao').textContent=cnts.nao;
  document.getElementById('kpi-and').textContent=cnts.and;
  document.getElementById('kpi-atra').textContent=cnts.atra;
  document.getElementById('kpi-con').textContent=cnts.con;
  document.getElementById('kpi-tot').textContent=filtered.length;
  const el=document.getElementById('badge-SOROCABA'); if(el) el.textContent=9;
  const body=document.getElementById('listBody');
  if(!filtered.length){body.innerHTML='<div class="empty">Nenhum item encontrado</div>';return;}
  body.innerHTML=filtered.map(card).join('');
}

function renderSemana(semana) {
  const equips=DATA.filter(d=>d.plano==='9076'&&d.denom.includes('ESTEIRA TRANSPORTADORA'));
  const cnts={nao:0,and:0,atra:0,con:0};
  equips.forEach(d=>{const s=getStatusSemana(semana,d.ordem);cnts[s]=(cnts[s]||0)+1;});
  document.getElementById('kpi-nao').textContent=cnts.nao;
  document.getElementById('kpi-and').textContent=cnts.and;
  document.getElementById('kpi-atra').textContent=cnts.atra;
  document.getElementById('kpi-con').textContent=cnts.con;
  document.getElementById('kpi-tot').textContent=equips.length;
  document.querySelector('.kpi-row').style.display='flex';
  const body=document.getElementById('listBody');
  body.innerHTML=equips.map(d=>cardSemana(d,semana)).join('');
}

function card(d) {
  const match=d.denom.match(/^(.*?)(\s+\d+[\w\-.]*)$/);
  const nomeBase=match?match[1]:d.denom; const numEquip=match?match[2].trim():'';
  const st=getStatusItem(d);
  const pdfLocal=localState[d.ordem]?.pdfName||'';
  const dadosPDF=localState[d.ordem]?.dadosPDF||null;
  const statusOpts=[{val:'nao',label:'⏸ Não Iniciado'},{val:'and',label:'🔄 Em Andamento'},{val:'atra',label:'⚠️ Atrasado'},{val:'con',label:'✅ Concluído'}]
    .map(o=>`<option value="${o.val}" ${st===o.val?'selected':''}>${o.label}</option>`).join('');
  return `<div class="card">
    <div class="card-info">
      <div class="card-title">${nomeBase}${numEquip?`<span class="equip-num"> ${numEquip}</span>`:''}</div>
      ${d.equip?`<div class="card-equip-sap"><span>Equip. SAP </span>${d.equip}</div>`:''}
      ${d.denomLocal?`<span class="chip-categoria">📍 ${d.denomLocal}</span>`:''}
      <div class="card-ordem" style="margin-top:4px">Ordem: ${d.ordem} · Plano ${d.plano}</div>
    </div>
    <div class="pdf-area">
      <label class="btn-upload">📎 Carregar PDF<input type="file" accept=".pdf" style="display:none" onchange="onPDFUpload(event,'${d.ordem}','${d.equip}')"></label>
      ${pdfLocal?`<div class="pdf-name">✅ ${pdfLocal}</div>`:''}
      ${dadosPDFHTML(dadosPDF,'pdfdados_'+d.ordem)}
      <button class="btn-enviar-ordem" onclick="abrirLinkOrdem('${d.ordem}','${d.equip}','${d.plano}','${d.denom}',null)">📤 Enviar Ordem Time</button>
    </div>
    <div class="rel-area">${relatorioHTML(d.ordem,d.equip)}</div>
    <select class="status-sel ${st}" onchange="onStatusChange(this,'${d.ordem}')">${statusOpts}</select>
  </div>`;
}

function cardSemana(d,semana) {
  const match=d.denom.match(/^(.*?)(\s+\d+[\w\-.]*)$/);
  const nomeBase=match?match[1]:d.denom; const numEquip=match?match[2].trim():'';
  const st=getStatusSemana(semana,d.ordem);
  const key='SEM_'+semana+'_'+d.ordem;
  const pdfLocal=localState[key]?.pdfName||'';
  const dadosPDF=localState[key]?.dadosPDF||null;
  const statusOpts=[{val:'nao',label:'⏸ Não Iniciado'},{val:'and',label:'🔄 Em Andamento'},{val:'atra',label:'⚠️ Atrasado'},{val:'con',label:'✅ Concluído'}]
    .map(o=>`<option value="${o.val}" ${st===o.val?'selected':''}>${o.label}</option>`).join('');
  return `<div class="card">
    <div class="card-info">
      <div class="card-title">${nomeBase}${numEquip?`<span class="equip-num"> ${numEquip}</span>`:''}</div>
      ${d.equip?`<div class="card-equip-sap"><span>Equip. SAP </span>${d.equip}</div>`:''}
      ${d.denomLocal?`<span class="chip-categoria">📍 ${d.denomLocal}</span>`:''}
      <div class="card-ordem" style="margin-top:4px">Ordem: ${d.ordem} · Semana ${semana}</div>
    </div>
    <div class="pdf-area">
      <label class="btn-upload">📎 Carregar PDF<input type="file" accept=".pdf" style="display:none" onchange="onPDFUploadSemana(event,'${d.ordem}',${semana},'${d.equip}')"></label>
      ${pdfLocal?`<div class="pdf-name">✅ ${pdfLocal}</div>`:''}
      ${dadosPDFHTML(dadosPDF,'pdfdados_sem_'+semana+'_'+d.ordem)}
      <button class="btn-enviar-ordem" onclick="abrirLinkOrdem('${d.ordem}','${d.equip}','${d.plano}','${d.denom}',${semana})">📤 Enviar Ordem Time</button>
    </div>
    <div class="rel-area">${relatorioHTML(d.ordem,d.equip)}</div>
    <select class="status-sel ${st}" onchange="onStatusSemana(this,'${d.ordem}',${semana})">${statusOpts}</select>
  </div>`;
}

function onStatusChange(sel,ordem){if(!localState[ordem])localState[ordem]={};localState[ordem].status=sel.value;sel.className='status-sel '+sel.value;saveState();render();}
function onStatusSemana(sel,ordem,semana){const key='SEM_'+semana+'_'+ordem;if(!localState[key])localState[key]={};localState[key].status=sel.value;sel.className='status-sel '+sel.value;saveState();renderSemana(semana);}

// ── PDF Upload & Leitura ──
if(typeof pdfjsLib!=='undefined'){pdfjsLib.GlobalWorkerOptions.workerSrc='https://cdnjs.cloudflare.com/ajax/libs/pdf.js/3.11.174/pdf.worker.min.js';}

async function lerDadosPDF(file,equipBuscado){
  const ab=await file.arrayBuffer();
  const pdf=await pdfjsLib.getDocument({data:ab}).promise;
  const paginas=[];
  for(let i=1;i<=pdf.numPages;i++){const p=await pdf.getPage(i);const c=await p.getTextContent();paginas.push(c.items.map(x=>x.str).join(' '));}
  let textoAlvo=null;
  if(equipBuscado){for(const t of paginas){if(t.includes(equipBuscado)){textoAlvo=t;break;}}}
  if(!textoAlvo)textoAlvo=paginas[0]||'';
  const ex=(t,ps)=>{for(const p of ps){const m=t.match(p);if(m)return m[1].trim();}return null;};
  return{
    nroOrdem:ex(textoAlvo,[/Nro\s+da\s+Ordem\s+(\d{6,8})/i,/(\d{7,8})\s+F_7D/i]),
    dataInicio:ex(textoAlvo,[/Data\s+Inicio\s+(\d{2}\.\d{2}\.\d{4})/i]),
    nroPlano:ex(textoAlvo,[/Nro\s+do\s+Plano\s+(\d{3,5})/i]),
    equipSAP:equipBuscado||ex(textoAlvo,[/Equipamento\s+(\d{7,8})/i])
  };
}

async function onPDFUpload(event,ordem,equipSAP){
  const file=event.target.files[0];if(!file)return;
  if(!localState[ordem])localState[ordem]={};localState[ordem].pdfName=file.name;localState[ordem].dadosPDF=null;saveState();render();
  try{const dados=await lerDadosPDF(file,equipSAP);localState[ordem].dadosPDF=dados;saveState();render();}catch(e){}
}

async function onPDFUploadSemana(event,ordem,semana,equipSAP){
  const file=event.target.files[0];if(!file)return;
  const key='SEM_'+semana+'_'+ordem;
  if(!localState[key])localState[key]={};localState[key].pdfName=file.name;localState[key].dadosPDF=null;saveState();renderSemana(semana);
  try{const dados=await lerDadosPDF(file,equipSAP);localState[key].dadosPDF=dados;saveState();renderSemana(semana);}catch(e){}
}

function dadosPDFHTML(dados,idSuffix){
  if(!dados)return`<div id="${idSuffix}"></div>`;
  const rows=[{lbl:'Nro da Ordem',val:dados.nroOrdem||'—'},{lbl:'Data Início',val:dados.dataInicio||'—'},{lbl:'Nro do Plano',val:dados.nroPlano||'—'},{lbl:'Equip. SAP',val:dados.equipSAP||'—'}]
    .map(r=>`<div class="pdf-dado-row"><span class="pdf-dado-lbl">${r.lbl}</span><span class="pdf-dado-val">${r.val}</span></div>`).join('');
  return`<div class="pdf-dados" id="${idSuffix}">${rows}</div>`;
}

// ── Relatório ──
function relatorioHTML(ordem,equip){
  const driveUrl=reportsCache[ordem+'_'+equip]||reportsCache['equip_'+equip];
  if(driveUrl)return`<div class="rel-gerado"><span class="rel-badge">📋 Relatório Gerado</span><span class="rel-info">☁️ Google Drive</span><a class="btn-rel-pdf" href="${driveUrl}" target="_blank">📥 Ver / Baixar PDF</a></div>`;
  const raw=localStorage.getItem('REPORT_'+ordem+'_'+equip);
  if(!raw)return`<span class="rel-vazio">Sem relatório</span>`;
  try{const rep=JSON.parse(raw);return`<div class="rel-gerado"><span class="rel-badge">📋 Relatório Gerado</span><span class="rel-info">📅 ${rep.geradoEm||'—'}</span><button class="btn-rel-pdf" onclick="baixarRelatorio('${ordem}','${equip}','${rep.filename||'relatorio.pdf'}')">📥 Baixar PDF</button></div>`;}
  catch(e){return`<span class="rel-vazio">Sem relatório</span>`;}
}

function baixarRelatorio(ordem,equip,filename){
  const raw=localStorage.getItem('REPORT_'+ordem+'_'+equip);if(!raw)return;
  try{const rep=JSON.parse(raw);const a=document.createElement('a');a.href=rep.pdfB64;a.download=filename;a.click();}catch(e){}
}

// ── Enviar Ordem ──
let _linkAtual='';

function abrirLinkOrdem(ordem,equip,plano,titulo,semana){
  const key=semana?'SEM_'+semana+'_'+ordem:ordem;
  const dados=localState[key]?.dadosPDF||{};
  const ordemFinal=dados.nroOrdem||ordem;
  const planoFinal=dados.nroPlano||plano;
  const dataFinal=dados.dataInicio||'';
  const params=new URLSearchParams({ordem:ordemFinal,equip,plano:planoFinal,data:dataFinal,titulo,...(semana?{semana}:{})});
  _linkAtual=BASE_URL+'form_ordem.html?'+params.toString();
  document.getElementById('linkUrl').textContent=_linkAtual;
  document.getElementById('linkModal').classList.add('open');
}

function fecharLinkModal(){document.getElementById('linkModal').classList.remove('open');}
function copiarLink(){if(navigator.clipboard){navigator.clipboard.writeText(_linkAtual).then(()=>{document.querySelector('.link-btn-copy').textContent='✅ Copiado!';setTimeout(()=>document.querySelector('.link-btn-copy').textContent='📋 Copiar Link',2000);});}else{const ta=document.createElement('textarea');ta.value=_linkAtual;document.body.appendChild(ta);ta.select();document.execCommand('copy');document.body.removeChild(ta);}}
function enviarWhatsApp(){window.open('https://wa.me/?text='+encodeURIComponent('📋 Ordem de Manutencao\nAcesse:\n\n'+_linkAtual),'_blank');}

setTab('SOROCABA');
</script>
</body>
</html>
