<!DOCTYPE html>
<html lang="es">
<head>
<meta charset="UTF-8"/>
<meta name="viewport" content="width=device-width, initial-scale=1.0"/>
<title>Recibo – Mármoles y Granitos Quilmes</title>
<link href="https://fonts.googleapis.com/css2?family=Montserrat:wght@300;400;500;600;700&display=swap" rel="stylesheet"/>
<script src="https://cdnjs.cloudflare.com/ajax/libs/jspdf/2.5.1/jspdf.umd.min.js"></script>
<script src="https://cdnjs.cloudflare.com/ajax/libs/html2canvas/1.4.1/html2canvas.min.js"></script>
<style>
  :root {
    --verde: #507560;
    --verde-claro: #6a9a7e;
    --verde-oscuro: #3a5648;
    --verde-bg: #f0f5f2;
    --gris: #6b7280;
    --gris-claro: #f3f4f6;
    --borde: #d1d8d3;
    --texto: #1a2e23;
    --blanco: #ffffff;
  }

  * { box-sizing: border-box; margin: 0; padding: 0; }

  body {
    font-family: 'Montserrat', sans-serif;
    background: #e8ede9;
    color: var(--texto);
    min-height: 100vh;
    padding: 30px 20px;
  }

  /* ===== TOOLBAR ===== */
  .toolbar {
    max-width: 860px;
    margin: 0 auto 20px;
    display: flex;
    gap: 12px;
    justify-content: flex-end;
    flex-wrap: wrap;
  }

  .btn {
    font-family: 'Montserrat', sans-serif;
    font-size: 13px;
    font-weight: 600;
    padding: 10px 22px;
    border-radius: 8px;
    border: none;
    cursor: pointer;
    letter-spacing: 0.04em;
    text-transform: uppercase;
    transition: all 0.2s ease;
    display: flex;
    align-items: center;
    gap: 8px;
  }

  .btn-primary {
    background: var(--verde);
    color: #fff;
    box-shadow: 0 4px 14px rgba(80,117,96,0.35);
  }
  .btn-primary:hover { background: var(--verde-oscuro); transform: translateY(-1px); }

  .btn-secondary {
    background: #fff;
    color: var(--verde);
    border: 2px solid var(--verde);
  }
  .btn-secondary:hover { background: var(--verde-bg); }

  .btn-wa {
    background: #25d366;
    color: #fff;
    box-shadow: 0 4px 14px rgba(37,211,102,0.35);
  }
  .btn-wa:hover { background: #1da851; transform: translateY(-1px); }

  .btn-gmail {
    background: #EA4335;
    color: #fff;
    box-shadow: 0 4px 14px rgba(234,67,53,0.35);
  }
  .btn-gmail:hover { background: #c0392b; transform: translateY(-1px); }

  /* ===== HOJA / DOCUMENTO ===== */
  #documento {
    max-width: 860px;
    margin: 0 auto;
    background: #fff;
    border-radius: 16px;
    overflow: hidden;
    box-shadow: 0 20px 60px rgba(0,0,0,0.12);
  }

  /* ===== HEADER ===== */
  .header {
    background: var(--verde);
    padding: 28px 44px;
    display: flex;
    justify-content: space-between;
    align-items: center;
    gap: 20px;
  }

  .logo-wrap { flex: 1; min-width: 0; }
  .logo-wrap svg { width: 100%; max-width: 380px; height: auto; display: block; }
  .logo-wrap svg .st0 { fill: #fff !important; }

  .header-right { text-align: right; color: #fff; }
  .header-right h1 {
    font-size: 28px;
    font-weight: 700;
    letter-spacing: 0.06em;
    text-transform: uppercase;
  }
  .header-right .num-recibo {
    font-size: 13px;
    font-weight: 500;
    opacity: 0.8;
    margin-top: 4px;
    letter-spacing: 0.08em;
  }

  /* ===== BANDA VERDE CLARO ===== */
  .banda {
    background: var(--verde-bg);
    border-top: 3px solid var(--verde);
    padding: 14px 44px;
    display: flex;
    gap: 20px;
    align-items: flex-end;
    flex-wrap: wrap;
  }

  .banda-item { display: flex; flex-direction: column; min-width: 0; }
  .banda-fecha { flex: 0 0 auto; }
  .banda-cliente { flex: 1 1 140px; }
  .banda-tel { flex: 0 0 auto; }
  .banda-mail { flex: 2 1 200px; min-width: 0; }

  .banda-item label {
    font-size: 10px;
    font-weight: 700;
    text-transform: uppercase;
    letter-spacing: 0.1em;
    color: var(--verde);
    display: block;
    margin-bottom: 3px;
    white-space: nowrap;
  }
  .banda-item input {
    font-family: 'Montserrat', sans-serif;
    font-size: 14px;
    font-weight: 600;
    color: var(--texto);
    background: transparent;
    border: none;
    border-bottom: 1.5px solid var(--borde);
    padding: 2px 0;
    width: 100%;
    min-width: 0;
    outline: none;
    transition: border-color 0.2s;
  }
  .banda-item input:focus { border-bottom-color: var(--verde); }

  /* ===== BARRA ESTADO DE PAGO ===== */
  .barra-estado {
    background: var(--verde-bg);
    border-bottom: 3px solid var(--verde);
    padding: 10px 44px;
    display: flex;
    align-items: center;
    justify-content: flex-end;
    gap: 16px;
  }
  .barra-estado-lbl {
    font-size: 10px;
    font-weight: 700;
    text-transform: uppercase;
    letter-spacing: 0.1em;
    color: var(--verde);
    white-space: nowrap;
  }

  /* ===== CUERPO ===== */
  .cuerpo {
    padding: 36px 44px;
    display: flex;
    flex-direction: column;
    gap: 28px;
  }

  /* ===== SECCION ===== */
  .seccion-titulo {
    font-size: 10px;
    font-weight: 700;
    text-transform: uppercase;
    letter-spacing: 0.12em;
    color: var(--verde);
    border-bottom: 2px solid var(--verde-bg);
    padding-bottom: 6px;
    margin-bottom: 14px;
  }

  .grid-2 { display: grid; grid-template-columns: 1fr 1fr; gap: 16px 30px; }
  .grid-3 { display: grid; grid-template-columns: 1fr 1fr 1fr; gap: 16px 30px; }

  .campo label {
    font-size: 10px;
    font-weight: 700;
    text-transform: uppercase;
    letter-spacing: 0.08em;
    color: var(--gris);
    display: block;
    margin-bottom: 5px;
  }
  .campo input,
  .campo textarea,
  .campo select {
    font-family: 'Montserrat', sans-serif;
    font-size: 14px;
    font-weight: 500;
    color: var(--texto);
    background: var(--gris-claro);
    border: 1.5px solid transparent;
    border-radius: 8px;
    padding: 10px 14px;
    width: 100%;
    outline: none;
    transition: all 0.2s ease;
    resize: vertical;
  }
  .campo input:focus,
  .campo textarea:focus,
  .campo select:focus {
    border-color: var(--verde);
    background: #fff;
    box-shadow: 0 0 0 3px rgba(80,117,96,0.12);
  }
  .campo textarea { min-height: 70px; }

  /* ===== TABLA DE ITEMS ===== */
  .tabla-wrap { overflow-x: auto; }
  table {
    width: 100%;
    border-collapse: collapse;
    font-size: 13px;
  }
  thead tr {
    background: var(--verde);
    color: #fff;
  }
  thead th {
    padding: 12px 14px;
    text-align: left;
    font-weight: 600;
    font-size: 11px;
    letter-spacing: 0.06em;
    text-transform: uppercase;
  }
  tbody tr { border-bottom: 1px solid var(--borde); }
  tbody tr:last-child { border-bottom: none; }
  tbody td { padding: 10px 14px; }
  tbody td input {
    font-family: 'Montserrat', sans-serif;
    font-size: 13px;
    font-weight: 500;
    background: transparent;
    border: none;
    border-bottom: 1.5px solid transparent;
    padding: 2px 0;
    width: 100%;
    outline: none;
    transition: border-color 0.2s;
    color: var(--texto);
  }
  tbody td input:focus { border-bottom-color: var(--verde); }
  tbody td.monto input { text-align: right; font-weight: 600; }

  .btn-agregar {
    font-family: 'Montserrat', sans-serif;
    font-size: 12px;
    font-weight: 600;
    color: var(--verde);
    background: transparent;
    border: 1.5px dashed var(--verde);
    border-radius: 8px;
    padding: 8px 18px;
    cursor: pointer;
    margin-top: 10px;
    letter-spacing: 0.04em;
    transition: all 0.2s;
  }
  .btn-agregar:hover { background: var(--verde-bg); }

  .btn-eliminar {
    background: none;
    border: none;
    color: #ccc;
    cursor: pointer;
    font-size: 16px;
    padding: 0 4px;
    transition: color 0.2s;
  }
  .btn-eliminar:hover { color: #e05454; }

  /* ===== TOTALES ===== */
  .totales-wrap {
    display: flex;
    justify-content: flex-end;
  }
  /* --- ÚNICA MODIFICACIÓN: más ancho y inputs más amplios --- */
  .totales-box {
    background: var(--verde-bg);
    border-radius: 12px;
    padding: 22px 28px;
    min-width: 380px;          /* era 280px */
    width: 100%;
    max-width: 480px;          /* nuevo límite máximo */
    display: flex;
    flex-direction: column;
    gap: 10px;
  }
  .total-row {
    display: flex;
    justify-content: space-between;
    align-items: center;
    gap: 20px;
  }
  .total-row .lbl {
    font-size: 11px;
    font-weight: 700;
    text-transform: uppercase;
    letter-spacing: 0.08em;
    color: var(--gris);
    white-space: nowrap;
  }
  .total-row .val {
    font-size: 14px;
    font-weight: 700;
    color: var(--texto);
  }
  .total-row.grande .lbl { font-size: 13px; color: var(--verde-oscuro); }
  .total-row.grande .val { font-size: 20px; color: var(--verde); }
  .separador { height: 1.5px; background: var(--borde); margin: 4px 0; }

  .total-row .val input {
    font-family: 'Montserrat', sans-serif;
    font-size: 14px;
    font-weight: 700;
    color: var(--texto);
    background: transparent;
    border: none;
    border-bottom: 1.5px solid var(--borde);
    padding: 2px 4px;
    width: 180px;              /* era 120px */
    text-align: right;
    outline: none;
  }
  .total-row .val input:focus { border-bottom-color: var(--verde); }
  .total-row.grande .val input {
    font-size: 20px;
    color: var(--verde);
    width: 200px;              /* era 140px */
  }
  /* rojo para resta abonar */
  #restaInput { color: #c0392b !important; }

  /* ===== PLANOS ===== */
  .planos-zona {
    border: 2px dashed var(--borde);
    border-radius: 12px;
    padding: 24px;
    text-align: center;
    cursor: pointer;
    transition: all 0.2s;
    position: relative;
    min-height: 140px;
    display: flex;
    flex-direction: column;
    align-items: center;
    justify-content: center;
    gap: 10px;
  }
  .planos-zona:hover { border-color: var(--verde); background: var(--verde-bg); }
  .planos-zona .icono { font-size: 36px; opacity: 0.4; }
  .planos-zona .texto {
    font-size: 13px;
    font-weight: 500;
    color: var(--gris);
  }
  .planos-zona .subtexto {
    font-size: 11px;
    color: #aaa;
    margin-top: -4px;
  }
  #inputPlano { display: none; }
  #previewPlano {
    max-width: 100%;
    max-height: 400px;
    border-radius: 8px;
    display: none;
    margin-top: 10px;
  }
  .btn-quitar-plano {
    font-family: 'Montserrat', sans-serif;
    font-size: 12px;
    color: #e05454;
    background: transparent;
    border: 1.5px solid #e05454;
    border-radius: 6px;
    padding: 5px 12px;
    cursor: pointer;
    display: none;
    margin-top: 8px;
  }

  /* ===== OBSERVACIONES ===== */
  .obs-input {
    font-family: 'Montserrat', sans-serif;
    font-size: 13px;
    font-weight: 400;
    color: var(--texto);
    background: var(--gris-claro);
    border: 1.5px solid transparent;
    border-radius: 10px;
    padding: 12px 16px;
    width: 100%;
    min-height: 80px;
    outline: none;
    resize: vertical;
    transition: all 0.2s;
  }
  .obs-input:focus { border-color: var(--verde); background: #fff; }

  /* ===== ESTADO PAGO ===== */
  .estado-pago {
    display: flex;
    gap: 10px;
    flex-wrap: wrap;
  }
  .chip-estado {
    font-family: 'Montserrat', sans-serif;
    font-size: 11px;
    font-weight: 700;
    letter-spacing: 0.08em;
    text-transform: uppercase;
    padding: 8px 18px;
    border-radius: 20px;
    border: 2px solid var(--borde);
    background: transparent;
    color: var(--gris);
    cursor: pointer;
    transition: all 0.2s;
  }
  .chip-estado.activo.pendiente { background: #fff3cd; border-color: #f0a500; color: #7d5700; }
  .chip-estado.activo.seniado   { background: #cce5ff; border-color: #2b7bdc; color: #1a4a8a; }
  .chip-estado.activo.pagado    { background: #d4edda; border-color: var(--verde); color: var(--verde-oscuro); }

  /* ===== FOOTER ===== */
  .footer {
    background: var(--texto);
    color: rgba(255,255,255,0.8);
    padding: 22px 44px;
    display: flex;
    justify-content: space-between;
    align-items: center;
    gap: 20px;
    flex-wrap: wrap;
  }
  .footer-contacto {
    display: flex;
    flex-direction: column;
    gap: 6px;
    font-size: 12px;
    flex: 1;
    min-width: 0;
  }
  .footer-contacto .linea {
    display: flex;
    align-items: flex-start;
    gap: 8px;
    flex-wrap: wrap;
    word-break: break-all;
  }
  .footer-contacto .linea input {
    font-family: 'Montserrat', sans-serif;
    font-size: 12px;
    background: transparent;
    border: none;
    border-bottom: 1px solid rgba(255,255,255,0.2);
    color: rgba(255,255,255,0.85);
    outline: none;
    width: 100%;
    padding: 2px 0;
    transition: border-color 0.2s;
    word-break: break-all;
  }
  .footer-contacto .linea input:focus { border-bottom-color: rgba(255,255,255,0.7); }

  .footer-link {
    color: rgba(255,255,255,0.85);
    text-decoration: none;
    font-size: 12px;
    font-family: 'Montserrat', sans-serif;
  }
  .footer-link:hover { color: #fff; text-decoration: underline; }

  .footer-firma {
    text-align: right;
    font-size: 11px;
    color: rgba(255,255,255,0.5);
    font-style: italic;
    flex-shrink: 0;
  }

  /* ===== MODO IMPRESIÓN / PDF ===== */
  @media print {
    body { background: white; padding: 0; }
    .toolbar { display: none; }
    #documento { border-radius: 0; box-shadow: none; max-width: 100%; }
    .btn-agregar, .btn-eliminar, .btn-quitar-plano { display: none !important; }
    .estado-pago .chip-estado:not(.activo) { display: none; }
    input, textarea { border: none !important; background: transparent !important; box-shadow: none !important; }
    .campo input, .campo textarea { background: transparent !important; }
    tbody td input { background: transparent !important; }
    .planos-zona { border: none; }
    .planos-texto-hint { display: none; }
    #seccionPlanos.sin-imagen { display: none !important; }
  }

  /* ===== RESPONSIVE ===== */
  @media (max-width: 640px) {
    body { padding: 0; background: #fff; }
    .toolbar {
      padding: 12px 14px;
      gap: 8px;
      justify-content: center;
      background: #e8ede9;
      margin-bottom: 0;
      border-radius: 0;
    }
    .btn { font-size: 11px; padding: 8px 12px; gap: 5px; }
    #documento { border-radius: 0; box-shadow: none; }
    .header { flex-direction: column; text-align: center; padding: 20px 20px; gap: 12px; }
    .header-right { text-align: center; }
    .header-right h1 { font-size: 22px; }
    .logo-wrap svg { max-width: 260px; margin: 0 auto; }
    .banda { padding: 14px 16px; gap: 14px; }
    .banda-fecha, .banda-tel { flex: 0 0 calc(50% - 7px); }
    .banda-cliente, .banda-mail { flex: 1 1 100%; }
    .barra-estado { padding: 10px 16px; justify-content: flex-start; }
    .cuerpo { padding: 20px 16px; gap: 20px; }
    .footer { padding: 16px; flex-direction: column; align-items: flex-start; gap: 16px; }
    .footer-firma { text-align: left; }
    .grid-2, .grid-3 { grid-template-columns: 1fr; }
    .totales-wrap { justify-content: stretch; }
    .totales-box { min-width: 100%; max-width: 100%; }
    .tabla-wrap { -webkit-overflow-scrolling: touch; }
    .footer-link { font-size: 14px; line-height: 1.8; }
    #btnEditarContacto { font-size: 13px; padding: 8px 16px; }
  }

  .badge-num {
    background: rgba(255,255,255,0.2);
    border-radius: 6px;
    padding: 4px 12px;
    font-size: 14px;
    font-weight: 600;
    letter-spacing: 0.1em;
    display: inline-block;
    margin-top: 6px;
  }
</style>
</head>
<body>

<!-- TOOLBAR -->
<div class="toolbar">
  <button class="btn btn-secondary" onclick="limpiarTodo()">
    🗑 Limpiar todo
  </button>
  <button class="btn btn-wa" id="btnWaCliente" onclick="abrirWhatsAppCliente()" title="Descarga el PDF y abre WhatsApp al cliente">
    <svg width="16" height="16" viewBox="0 0 24 24" fill="currentColor"><path d="M17.472 14.382c-.297-.149-1.758-.867-2.03-.967-.273-.099-.471-.148-.67.15-.197.297-.767.966-.94 1.164-.173.199-.347.223-.644.075-.297-.15-1.255-.463-2.39-1.475-.883-.788-1.48-1.761-1.653-2.059-.173-.297-.018-.458.13-.606.134-.133.298-.347.446-.52.149-.174.198-.298.298-.497.099-.198.05-.371-.025-.52-.075-.149-.669-1.612-.916-2.207-.242-.579-.487-.5-.669-.51-.173-.008-.371-.01-.57-.01-.198 0-.52.074-.792.372-.272.297-1.04 1.016-1.04 2.479 0 1.462 1.065 2.875 1.213 3.074.149.198 2.096 3.2 5.077 4.487.709.306 1.262.489 1.694.625.712.227 1.36.195 1.871.118.571-.085 1.758-.719 2.006-1.413.248-.694.248-1.289.173-1.413-.074-.124-.272-.198-.57-.347m-5.421 7.403h-.004a9.87 9.87 0 01-5.031-1.378l-.361-.214-3.741.982.998-3.648-.235-.374a9.86 9.86 0 01-1.51-5.26c.001-5.45 4.436-9.884 9.888-9.884 2.64 0 5.122 1.03 6.988 2.898a9.825 9.825 0 012.893 6.994c-.003 5.45-4.437 9.884-9.885 9.884m8.413-18.297A11.815 11.815 0 0012.05 0C5.495 0 .16 5.335.157 11.892c0 2.096.547 4.142 1.588 5.945L.057 24l6.305-1.654a11.882 11.882 0 005.683 1.448h.005c6.554 0 11.89-5.335 11.893-11.893a11.821 11.821 0 00-3.48-8.413z"/></svg>
    WA Cliente
  </button>
  <button class="btn btn-gmail" id="btnGmailCliente" onclick="enviarGmail()" title="Descarga el PDF y abre Gmail al cliente">
    <svg width="16" height="16" viewBox="0 0 24 24" fill="currentColor"><path d="M24 5.457v13.909c0 .904-.732 1.636-1.636 1.636h-3.819V11.73L12 16.64l-6.545-4.91v9.273H1.636A1.636 1.636 0 0 1 0 19.366V5.457c0-2.023 2.309-3.178 3.927-1.964L5.455 4.64 12 9.548l6.545-4.91 1.528-1.145C21.69 2.28 24 3.434 24 5.457z"/></svg>
    Gmail Cliente
  </button>
  <button class="btn btn-primary" onclick="descargarPDF()">
    ⬇ Descargar PDF
  </button>
</div>

<!-- DOCUMENTO -->
<div id="documento">

  <!-- HEADER -->
  <div class="header">
    <div class="logo-wrap">
      <svg version="1.1" id="Capa_1" xmlns="http://www.w3.org/2000/svg" xmlns:xlink="http://www.w3.org/1999/xlink" viewBox="0 0 812.6 312.6" xml:space="preserve">
        <style type="text/css">.st0{fill:#ffffff;}</style>
        <g>
          <path class="st0" d="M89.3,226.9c0,3.4,2.1,5.6,5.5,5.6c2.9,0,5.1-2.4,5.1-5.6V123.3c0-14,0-43.6,0-43.6h11.4c0,0,0,0.4,0,1.2l5.3,29.7h0.2l4.8-30.8l0.2,0c0,0,0,0,0,0h11.6c0,0,0,57.7,0,86v61.4c0,2.5,2.1,5.1,4.2,5.2c3.4,0.2,5.9-1.5,6.1-4.5c0.2-3.8,0.2-7.6,0.2-11.4c0-7.9,0-15.7,0-23.6v-92.1c0-8.8,0-17.5,0-26.3c0-3.2-2.1-5.5-5.1-5.5c-14.8,0-29.6,0-44.4,0c-2.6,0-4.6,1.6-4.9,4.1c-0.3,2.4-0.2,4.8-0.2,7.3c0,7.8,0,15.7,0,23.6L89.3,226.9L89.3,226.9z"/>
          <path class="st0" d="M117.6,0C52.7,0,0,52.6,0,117.6c0,7,0.6,13.9,1.8,20.6c7.1,40.2,34.6,73.3,71.4,88.3c2,0.1,4.5-2.4,4.5-4.6V74.3c0-3.2-2-5.3-5.2-5.3c-8.3,0-16.6,0-24.9,0c-4.9,0-9.4,3.9-10.4,8.7c-1,5,1.8,10.3,6.4,12.2c7.6,3.1,15.3-2.3,15.3-10.7c0-2.5,0.8-4.6,3.1-5.7c2.1-1.1,4.3-1.1,6.2,0.5c1.9,1.6,2.3,3.7,2.2,6.2c-0.2,4.7-1.6,8.9-4.3,12.6c-4.1,5.6-9.5,8.8-16.4,9.6c-9.6,1.1-18.4-5.1-22.2-13.2c-3.8-8-2-18.4,4.3-24.8c4.6-4.7,10.3-7,16.9-7c9.5,0,18.9,0,28.4,0h106.7c9.7,0,17.6,5.4,21.3,14.3c3.7,9,0.9,19.5-6.9,25.8c-4.2,3.3-9,5.4-14.4,5c-8.4-0.6-15-4.4-19.1-11.9c-2.4-4.3-3.8-9-2.5-14.1c0.7-2.6,3-3.9,6.4-3.6c2.3,0.2,3.8,1.7,4.4,4c0.8,3.1,1.1,6.4,2.5,9.1c1.7,3.4,5.1,5.1,9,4.9c4.9-0.3,8.9-2.7,10.2-7.4c2-7-2.1-14.1-10.2-14.3c-7.8-0.2-15.6-0.1-23.4-0.1c-3.2,0-5.4,2.2-5.4,5.4v118.5c0,7.9-0.1,15.8,0,23.8c0,2.2-0.1,4.7,0.8,6.6c1.3,2.9,4.1,3.5,7.3,2.1l0.2-0.1c7.7-3.3,16-7.7,24-13.3c0.1-0.1,0.1-0.1,0.2-0.1c12-8.9,22.1-20.1,30-32.9c10.9-17.9,17.3-38.9,17.3-61.3C235.2,52.6,182.5,0,117.6,0z M79.5,17.3c6.4,0,12.9,0,19.4,0h23.9c8,0,15.9,0,23.9,0c2.6,0,5.3,0,7.9,0.2c2.7,0.3,4.9,3.2,4.7,5.9c-0.3,3-2.8,5.5-5.6,5.5h-74c-2.7,0-5.4-2.8-5.4-5.6C74.1,20.1,76.6,17.3,79.5,17.3z M176.4,46c-1.2,1.8-2.8,2.9-5.1,2.9c-7.5,0-14.9,0.1-22.4,0.1c-7,0-13.9-0.1-20.9-0.1c-7.8,0-15.7,0-23.5,0H61.9c-3.3,0-6-2.7-6-6c0-2.8,2.9-5.4,6-5.4c2.6,0,5.2,0,7.8,0h76.7c8.1,0,16.2,0.2,24.2-0.1C175.9,37.2,178.7,42.4,176.4,46z"/>
          <path class="st0" d="M179.4,257.7c-3.1-1.9-6.3-3-10-3.8c-23.3-5.5-55.9,8.1-68.1,13.9c-5.4,2.6-11.3,4.5-17.6,5.4c-3.7,0.6-7.9,0.8-11.5,0c-4.6-1.1-7.8-3.6-7.9-9.9c0-1.2,0.3-2.6,0.6-3.8c0.4-1.7,1.2-3.6,2.5-5.1c5.4-7.3,24.6-13.9,41.4-18.4c3.6-1,5.9-1.2,8.9-1.9c0.3,0,0.6-0.1,0.8-0.2c0.1,0,0.1,0,0.2,0l0,0c1.8-0.7,3.1-2.7,3.1-4.9V113.5l-3,9.6l-2.1,6.4l-2.1-6.4l-3.3-9.5v82.6c0,5.9,0,11.9,0,17.8c-0.4,5.5,1.6,14.7-4.6,17.9c-4.1,1.9-6.1,2.7-10.8,4.3c-1.2,0.4-2.4,0.9-3.6,1.3c-12.4,4.5-24,10.3-28,16.2c-1.1,1.8-2.1,3.7-2.6,5.7c-0.7,3.1-0.4,5.9,0.7,9.1c2.4,6.2,9.3,10.6,16.6,12.3c3,0.7,5.8,1.1,8.7,0.9c6.5-0.6,14.2-2,22.1-4.1c16.1-3.4,34.8-8,52.3-3.8c6.4,1.5,12.4,4.1,18.2,8.2c16.7,11.7,22,28.5,22.5,30.7l2.9-0.2C205.8,312.4,207.4,274.5,179.4,257.7z"/>
        </g>
        <g>
          <path class="st0" d="M264.4,141v-36.9h5.6l16.1,26.9h-3l15.9-26.9h5.6l0.1,36.9h-6.6v-26.7h1.3l-13.4,22.5H283l-13.7-22.5h1.6V141L264.4,141L264.4,141z"/>
          <path class="st0" d="M322.5,141.4c-2.1,0-4-0.4-5.5-1.1s-2.8-1.7-3.7-3c-0.9-1.3-1.3-2.7-1.3-4.3c0-1.6,0.4-3,1.1-4.3c0.8-1.3,2-2.3,3.7-3c1.7-0.7,4-1.1,6.9-1.1h8.2v4.4h-7.7c-2.2,0-3.7,0.4-4.5,1.1c-0.8,0.7-1.2,1.6-1.2,2.7c0,1.2,0.5,2.1,1.4,2.8c0.9,0.7,2.3,1,4,1c1.6,0,3.1-0.4,4.3-1.1c1.3-0.7,2.2-1.8,2.8-3.3l1,4c-0.6,1.6-1.8,2.9-3.4,3.8C327.1,140.9,325.1,141.4,322.5,141.4z M331.4,141v-5.7l-0.3-1.2v-10c0-1.9-0.6-3.4-1.8-4.5c-1.2-1.1-3-1.6-5.3-1.6c-1.5,0-3.1,0.2-4.6,0.7c-1.5,0.5-2.8,1.2-3.9,2.1l-2.6-4.8c1.5-1.2,3.3-2,5.4-2.6c2.1-0.6,4.2-0.9,6.5-0.9c4.1,0,7.3,1,9.5,3s3.3,5,3.3,9.1V141L331.4,141L331.4,141z M321,109l7.1-7.2h7.8l-9.2,7.2H321z"/>
          <path class="st0" d="M346.1,141v-28.1h6.3v7.7l-0.7-2.3c0.8-1.9,2.1-3.3,4-4.3c1.8-1,4.1-1.5,6.7-1.5v6.3c-0.2,0-0.5-0.1-0.7-0.1c-0.2,0-0.5,0-0.7,0c-2.5,0-4.5,0.7-6,2.2c-1.5,1.4-2.2,3.6-2.2,6.6V141L346.1,141L346.1,141z"/>
          <path class="st0" d="M403.4,112.5c2.2,0,4.2,0.4,6,1.3c1.7,0.9,3.1,2.2,4.1,4.1c1,1.8,1.5,4.2,1.5,7V141h-6.5v-15.3c0-2.5-0.5-4.4-1.7-5.6c-1.1-1.2-2.7-1.8-4.7-1.8c-1.4,0-2.7,0.3-3.8,0.9s-2,1.6-2.6,2.8c-0.6,1.2-0.9,2.8-0.9,4.7V141h-6.5v-15.3c0-2.5-0.5-4.4-1.7-5.6s-2.7-1.8-4.7-1.8c-1.4,0-2.7,0.3-3.8,0.9s-2,1.6-2.6,2.8c-0.6,1.2-0.9,2.8-0.9,4.7V141h-6.5v-28.1h6.3v7.5l-1.2-2.3c0.9-1.8,2.3-3.2,4.1-4.2c1.8-1,3.9-1.5,6.2-1.5c2.6,0,4.9,0.7,6.9,1.9c1.9,1.3,3.2,3.3,3.8,6l-2.5-0.9c0.9-2.1,2.4-3.8,4.5-5.1C398.2,113.1,400.6,112.5,403.4,112.5z"/>
          <path class="st0" d="M436,141.4c-2.8,0-5.4-0.6-7.7-1.9s-4.1-3-5.3-5.1c-1.3-2.2-1.9-4.7-1.9-7.4c0-2.8,0.7-5.3,1.9-7.5c1.3-2.1,3.1-3.8,5.3-5.1c2.2-1.2,4.8-1.9,7.7-1.9c2.9,0,5.5,0.6,7.8,1.9c2.3,1.3,4,2.9,5.3,5.1c1.3,2.1,1.9,4.6,1.9,7.5c0,2.8-0.6,5.3-1.9,7.5c-1.3,2.2-3.1,3.9-5.3,5.1C441.5,140.7,438.9,141.4,436,141.4z M436,135.8c1.6,0,3.1-0.4,4.3-1.1s2.2-1.8,3-3.1c0.7-1.4,1.1-2.9,1.1-4.7c0-1.8-0.4-3.3-1.1-4.7c-0.7-1.3-1.7-2.3-3-3.1c-1.3-0.7-2.7-1.1-4.3-1.1c-1.6,0-3,0.4-4.3,1.1s-2.3,1.7-3,3.1c-0.7,1.3-1.1,2.9-1.1,4.7c0,1.8,0.4,3.3,1.1,4.7c0.7,1.4,1.7,2.4,3,3.1C433.1,135.4,434.5,135.8,436,135.8z"/>
          <path class="st0" d="M457.4,141v-39.1h6.5V141H457.4z"/>
          <path class="st0" d="M485.8,141.4c-3.1,0-5.9-0.6-8.2-1.9s-4.1-3-5.4-5.1c-1.3-2.2-1.9-4.6-1.9-7.5c0-2.8,0.6-5.3,1.9-7.5c1.2-2.2,3-3.9,5.2-5.1c2.2-1.2,4.7-1.9,7.5-1.9c2.7,0,5.2,0.6,7.3,1.8s3.8,2.9,5.1,5.1c1.2,2.2,1.8,4.8,1.8,7.7c0,0.2,0,0.5,0,0.9s-0.1,0.7-0.1,1h-23.4v-4.4h20l-2.6,1.4c0-1.6-0.3-3-1-4.2s-1.6-2.2-2.8-2.9c-1.2-0.7-2.6-1-4.2-1c-1.6,0-3,0.3-4.2,1c-1.3,0.7-2.2,1.7-2.9,2.9c-0.7,1.3-1,2.7-1,4.4v1c0,1.7,0.4,3.2,1.1,4.5c0.8,1.3,1.8,2.3,3.2,3c1.4,0.7,3,1.1,4.9,1.1c1.6,0,3-0.3,4.3-0.8s2.4-1.3,3.4-2.3l3.6,4.1c-1.3,1.5-2.9,2.6-4.8,3.4C490.4,141,488.3,141.4,485.8,141.4z"/>
          <path class="st0" d="M514.1,141.4c-2.4,0-4.7-0.3-6.8-0.9c-2.1-0.6-3.9-1.4-5.2-2.2l2.5-5c1.3,0.8,2.8,1.5,4.5,2c1.8,0.5,3.5,0.8,5.3,0.8c2,0,3.5-0.3,4.4-0.8c0.9-0.5,1.4-1.3,1.4-2.2c0-0.8-0.3-1.4-0.9-1.8s-1.5-0.7-2.5-0.9c-1-0.2-2.1-0.4-3.4-0.6c-1.3-0.2-2.5-0.4-3.7-0.7c-1.3-0.3-2.4-0.7-3.4-1.3c-1-0.6-1.8-1.4-2.5-2.3c-0.6-1-0.9-2.3-0.9-4c0-1.8,0.5-3.3,1.5-4.6c1-1.3,2.5-2.3,4.3-3.1c1.8-0.7,4-1.1,6.6-1.1c1.9,0,3.8,0.2,5.7,0.7c1.9,0.4,3.6,1,4.9,1.8l-2.6,5c-1.3-0.8-2.6-1.3-4-1.6c-1.4-0.3-2.7-0.4-4.1-0.4c-2,0-3.4,0.3-4.4,0.9c-1,0.6-1.5,1.3-1.5,2.2c0,0.8,0.3,1.5,0.9,1.9c0.6,0.4,1.5,0.7,2.5,1c1,0.2,2.1,0.4,3.4,0.6c1.2,0.2,2.5,0.4,3.7,0.7c1.2,0.3,2.4,0.7,3.4,1.3c1,0.5,1.9,1.3,2.5,2.3c0.6,1,0.9,2.3,0.9,3.8c0,1.8-0.5,3.3-1.6,4.6c-1,1.3-2.5,2.3-4.4,3C519,141,516.7,141.4,514.1,141.4z"/>
          <path class="st0" d="M549.4,151.6c-1.4,0-2.8-0.2-4.2-0.7c-1.4-0.5-2.5-1.1-3.4-1.9l2.6-4.8c0.7,0.6,1.4,1,2.2,1.4c0.8,0.3,1.7,0.5,2.6,0.5c1.2,0,2.2-0.3,2.9-0.9s1.4-1.6,2.1-3.1l1.6-3.6l0.6-0.8l10.6-24.9h6.3l-13.2,30.4c-0.9,2.1-1.9,3.7-2.9,4.9s-2.3,2.1-3.6,2.6C552.3,151.3,550.9,151.6,549.4,151.6z M555.1,142l-12.7-29.1h6.8l10.4,24.3L555.1,142z"/>
          <path class="st0" d="M609.8,141.5c-2.9,0-5.5-0.5-7.9-1.4c-2.4-0.9-4.5-2.2-6.3-4c-1.8-1.7-3.2-3.7-4.2-6c-1-2.3-1.5-4.8-1.5-7.6c0-2.8,0.5-5.3,1.5-7.6c1-2.3,2.4-4.3,4.2-6c1.8-1.7,3.9-3,6.3-4c2.4-0.9,5.1-1.4,8-1.4c3.1,0,5.9,0.5,8.4,1.5c2.5,1,4.6,2.5,6.3,4.5l-4.3,4.2c-1.4-1.4-3-2.5-4.7-3.2c-1.7-0.7-3.5-1-5.5-1c-2,0-3.8,0.3-5.4,0.9s-3,1.5-4.2,2.7c-1.2,1.2-2.1,2.5-2.8,4.1c-0.7,1.6-1,3.3-1,5.2c0,1.9,0.3,3.6,1,5.2s1.6,3,2.8,4.1c1.2,1.2,2.6,2.1,4.2,2.7c1.6,0.6,3.4,0.9,5.4,0.9c1.8,0,3.6-0.3,5.3-0.9c1.7-0.6,3.3-1.5,4.9-2.9l3.8,5.1c-1.9,1.5-4.1,2.7-6.6,3.5C615,141.1,612.5,141.5,609.8,141.5z M617.8,135.9v-13.7h6.4v14.6L617.8,135.9z"/>
          <path class="st0" d="M632.7,141v-28.1h6.3v7.7l-0.7-2.3c0.8-1.9,2.1-3.3,4-4.3c1.8-1,4.1-1.5,6.7-1.5v6.3c-0.2,0-0.5-0.1-0.7-0.1c-0.2,0-0.5,0-0.7,0c-2.5,0-4.5,0.7-6,2.2c-1.5,1.4-2.2,3.6-2.2,6.6V141L632.7,141L632.7,141z"/>
          <path class="st0" d="M661,141.4c-2.1,0-4-0.4-5.5-1.1c-1.6-0.7-2.8-1.7-3.7-3c-0.9-1.3-1.3-2.7-1.3-4.3c0-1.6,0.4-3,1.1-4.3c0.8-1.3,2-2.3,3.7-3c1.7-0.7,4-1.1,6.9-1.1h8.2v4.4h-7.7c-2.2,0-3.7,0.4-4.5,1.1c-0.8,0.7-1.2,1.6-1.2,2.7c0,1.2,0.5,2.1,1.4,2.8s2.3,1,4,1c1.6,0,3.1-0.4,4.3-1.1c1.3-0.7,2.2-1.8,2.8-3.3l1,4c-0.6,1.6-1.8,2.9-3.4,3.8C665.6,140.9,663.5,141.4,661,141.4z M669.8,141v-5.7l-0.3-1.2v-10c0-1.9-0.6-3.4-1.8-4.5c-1.2-1.1-3-1.6-5.3-1.6c-1.5,0-3.1,0.2-4.6,0.7c-1.5,0.5-2.8,1.2-3.9,2.1l-2.6-4.8c1.5-1.2,3.3-2,5.4-2.6c2.1-0.6,4.2-0.9,6.5-0.9c4.1,0,7.3,1,9.5,3c2.2,2,3.3,5,3.3,9.1V141L669.8,141L669.8,141z"/>
          <path class="st0" d="M700.4,112.5c2.3,0,4.3,0.4,6,1.3c1.7,0.9,3.1,2.2,4.2,4.1c1,1.8,1.5,4.2,1.5,7V141h-6.6v-15.3c0-2.5-0.6-4.4-1.7-5.6s-2.8-1.8-4.9-1.8c-1.5,0-2.9,0.3-4.1,0.9s-2.1,1.6-2.8,2.8c-0.7,1.2-1,2.8-1,4.7V141h-6.5v-28.1h6.3v7.6l-1.1-2.4c0.9-1.8,2.4-3.2,4.2-4.2C695.8,113,698,112.5,700.4,112.5z"/>
          <path class="st0" d="M723.9,108.2c-1.2,0-2.2-0.4-3-1.2c-0.8-0.8-1.2-1.7-1.2-2.8c0-1,0.4-2,1.2-2.7c0.8-0.8,1.8-1.2,3-1.2s2.2,0.4,3,1.1s1.2,1.6,1.2,2.8c0,1.1-0.4,2.1-1.2,2.9C726.1,107.8,725.1,108.2,723.9,108.2z M720.6,141v-28.1h6.5V141H720.6z"/>
          <path class="st0" d="M732,118.3v-5.3h18.8v5.3H732z M746.4,141.4c-3.1,0-5.5-0.8-7.2-2.4c-1.7-1.6-2.5-3.9-2.5-7v-25.3h6.5v25.1c0,1.3,0.3,2.4,1,3.1c0.7,0.7,1.7,1.1,3,1.1c1.4,0,2.6-0.4,3.6-1.2l1.9,4.7c-0.8,0.6-1.8,1.1-2.9,1.4C748.6,141.2,747.5,141.4,746.4,141.4z"/>
          <path class="st0" d="M769.8,141.4c-2.8,0-5.4-0.6-7.7-1.9c-2.3-1.2-4.1-3-5.3-5.1c-1.3-2.2-1.9-4.7-1.9-7.4c0-2.8,0.7-5.3,1.9-7.5c1.3-2.1,3.1-3.8,5.3-5.1c2.2-1.2,4.8-1.9,7.7-1.9c2.9,0,5.5,0.6,7.8,1.9c2.3,1.3,4,2.9,5.3,5.1c1.3,2.1,1.9,4.6,1.9,7.5c0,2.8-0.6,5.3-1.9,7.5c-1.3,2.2-3.1,3.9-5.3,5.1C775.3,140.7,772.7,141.4,769.8,141.4z M769.8,135.8c1.6,0,3.1-0.4,4.3-1.1c1.3-0.7,2.2-1.8,3-3.1c0.7-1.4,1.1-2.9,1.1-4.7c0-1.8-0.4-3.3-1.1-4.7c-0.7-1.3-1.7-2.3-3-3.1c-1.3-0.7-2.7-1.1-4.3-1.1c-1.6,0-3,0.4-4.3,1.1c-1.3,0.7-2.3,1.7-3,3.1c-0.7,1.3-1.1,2.9-1.1,4.7c0,1.8,0.4,3.3,1.1,4.7c0.7,1.4,1.7,2.4,3,3.1C766.8,135.4,768.2,135.8,769.8,135.8z"/>
          <path class="st0" d="M799.9,141.4c-2.4,0-4.7-0.3-6.8-0.9c-2.1-0.6-3.9-1.4-5.2-2.2l2.5-5c1.3,0.8,2.8,1.5,4.5,2c1.8,0.5,3.5,0.8,5.3,0.8c2,0,3.5-0.3,4.4-0.8c0.9-0.5,1.4-1.3,1.4-2.2c0-0.8-0.3-1.4-0.9-1.8c-0.6-0.4-1.5-0.7-2.5-0.9c-1-0.2-2.1-0.4-3.4-0.6c-1.2-0.2-2.5-0.4-3.7-0.7c-1.3-0.3-2.4-0.7-3.4-1.3c-1-0.6-1.8-1.4-2.5-2.3s-0.9-2.3-0.9-4c0-1.8,0.5-3.3,1.5-4.6c1-1.3,2.5-2.3,4.3-3.1c1.8-0.7,4-1.1,6.6-1.1c1.9,0,3.8,0.2,5.7,0.7c1.9,0.4,3.6,1,4.9,1.8l-2.6,5c-1.3-0.8-2.6-1.3-4-1.6c-1.4-0.3-2.7-0.4-4.1-0.4c-2,0-3.4,0.3-4.4,0.9c-1,0.6-1.5,1.3-1.5,2.2c0,0.8,0.3,1.5,0.9,1.9c0.6,0.4,1.5,0.7,2.5,1c1,0.2,2.1,0.4,3.4,0.6c1.2,0.2,2.5,0.4,3.7,0.7c1.2,0.3,2.4,0.7,3.4,1.3s1.9,1.3,2.5,2.3c0.6,1,0.9,2.3,0.9,3.8c0,1.8-0.5,3.3-1.5,4.6c-1,1.3-2.5,2.3-4.4,3C804.8,141,802.5,141.4,799.9,141.4z"/>
          <path class="st0" d="M432.4,204.7c-2.8,0-5.5-0.5-7.9-1.4c-2.4-0.9-4.6-2.3-6.3-4s-3.2-3.7-4.2-6c-1-2.3-1.5-4.8-1.5-7.5c0-2.7,0.5-5.3,1.5-7.5c1-2.3,2.4-4.3,4.2-6s3.9-3.1,6.3-4c2.4-0.9,5.1-1.4,8-1.4c2.9,0,5.5,0.5,7.9,1.4c2.4,0.9,4.5,2.3,6.3,4c1.8,1.7,3.1,3.7,4.1,6c1,2.3,1.5,4.8,1.5,7.6s-0.5,5.3-1.5,7.6c-1,2.3-2.4,4.3-4.1,6c-1.8,1.7-3.9,3-6.3,4C437.9,204.3,435.3,204.7,432.4,204.7z M432.4,198.7c1.9,0,3.6-0.3,5.1-0.9c1.6-0.6,2.9-1.5,4.1-2.7c1.2-1.2,2.1-2.6,2.7-4.1c0.6-1.6,0.9-3.3,0.9-5.2c0-1.9-0.3-3.6-0.9-5.2c-0.6-1.6-1.5-2.9-2.7-4.1c-1.2-1.2-2.5-2.1-4.1-2.7s-3.3-0.9-5.1-0.9s-3.6,0.3-5.2,0.9c-1.6,0.6-3,1.5-4.1,2.7c-1.2,1.2-2.1,2.5-2.7,4.1c-0.7,1.6-1,3.3-1,5.2c0,1.9,0.3,3.6,1,5.2c0.7,1.6,1.6,3,2.7,4.1c1.2,1.2,2.5,2.1,4.1,2.7C428.8,198.4,430.6,198.7,432.4,198.7z M444.5,212.3c-1.3,0-2.6-0.2-3.8-0.5c-1.2-0.3-2.4-0.8-3.6-1.5c-1.2-0.7-2.4-1.6-3.7-2.7c-1.3-1.1-2.6-2.5-4.2-4.1l7.3-1.8c1,1.3,1.9,2.3,2.8,3.1s1.8,1.3,2.6,1.7c0.9,0.3,1.7,0.5,2.6,0.5c2.5,0,4.6-1,6.4-3l3.1,3.8C451.6,210.8,448.4,212.3,444.5,212.3z"/>
          <path class="st0" d="M475.3,204.7c-5,0-9-1.4-11.8-4.3c-2.9-2.9-4.3-7-4.3-12.4v-20.7h6.9v20.5c0,3.8,0.8,6.5,2.5,8.3c1.6,1.7,3.9,2.6,6.9,2.6c3,0,5.3-0.9,6.9-2.6c1.6-1.7,2.4-4.5,2.4-8.3v-20.5h6.7v20.7c0,5.4-1.4,9.5-4.3,12.4C484.3,203.3,480.3,204.7,475.3,204.7z"/>
          <path class="st0" d="M501,204.2v-36.9h6.9v36.9H501z"/>
          <path class="st0" d="M517.9,204.2v-36.9h6.9v31.1H544v5.8H517.9z"/>
          <path class="st0" d="M549.4,204.2v-36.9h5.6l16.1,26.9h-3l15.9-26.9h5.6l0.1,36.9h-6.6v-26.7h1.3L571.2,200h-3.1l-13.7-22.5h1.6v26.7L549.4,204.2L549.4,204.2z"/>
          <path class="st0" d="M606.5,198.4h20.8v5.8h-27.7v-36.9h26.9v5.8h-20.1L606.5,198.4L606.5,198.4z M606,182.6h18.3v5.6H606V182.6z"/>
          <path class="st0" d="M646.4,204.7c-2.9,0-5.6-0.4-8.2-1.2c-2.6-0.8-4.7-1.9-6.3-3.2l2.4-5.3c1.5,1.2,3.3,2.1,5.5,2.9c2.2,0.8,4.4,1.2,6.7,1.2c1.9,0,3.5-0.2,4.7-0.6c1.2-0.4,2.1-1,2.6-1.7c0.6-0.7,0.8-1.5,0.8-2.5c0-1.1-0.4-2-1.2-2.7c-0.8-0.7-1.9-1.2-3.1-1.6s-2.7-0.8-4.3-1.1c-1.6-0.3-3.1-0.7-4.7-1.2c-1.6-0.5-3-1.1-4.3-1.9s-2.3-1.8-3.1-3c-0.8-1.3-1.2-2.9-1.2-4.8c0-2,0.5-3.8,1.6-5.5s2.7-3,4.9-4c2.2-1,5-1.5,8.3-1.5c2.2,0,4.4,0.3,6.6,0.8c2.2,0.6,4.1,1.4,5.7,2.4l-2.2,5.3c-1.6-1-3.3-1.7-5.1-2.2c-1.8-0.5-3.4-0.7-5.1-0.7c-1.9,0-3.4,0.2-4.6,0.7c-1.2,0.5-2.1,1.1-2.6,1.8c-0.5,0.8-0.8,1.6-0.8,2.5c0,1.1,0.4,2,1.2,2.7c0.8,0.7,1.8,1.2,3.1,1.6c1.3,0.4,2.7,0.8,4.3,1.1c1.6,0.4,3.1,0.8,4.7,1.2c1.5,0.5,3,1.1,4.3,1.8s2.3,1.8,3.1,3c0.8,1.3,1.2,2.8,1.2,4.8c0,2-0.5,3.8-1.6,5.4c-1.1,1.7-2.7,3-4.9,4C652.6,204.2,649.8,204.7,646.4,204.7z"/>
        </g>
      </svg>
    </div>

    <div class="header-right">
      <h1>Recibo</h1>
      <div class="num-recibo">N°&nbsp;<span id="numDisplay">–</span></div>
      <div class="badge-num">
        <input id="inputNumero" type="text" placeholder="120426"
          style="background:transparent;border:none;color:#fff;font-family:'Montserrat',sans-serif;font-size:14px;font-weight:600;width:90px;text-align:center;outline:none;"
          oninput="document.getElementById('numDisplay').textContent=this.value||'–'"/>
      </div>
    </div>
  </div>

  <!-- BANDA DATOS BÁSICOS -->
  <div class="banda">
    <div class="banda-item banda-fecha">
      <label>Fecha</label>
      <input type="date" id="fechaRaw" style="position:absolute;opacity:0;pointer-events:none;width:0;height:0;" oninput="sincronizarFecha()"/>
      <input type="text" id="fecha" placeholder="DD/MM/AAAA" readonly
        style="cursor:pointer;"
        onclick="document.getElementById('fechaRaw').showPicker ? document.getElementById('fechaRaw').showPicker() : document.getElementById('fechaRaw').click()"
        title="Hacé clic para elegir la fecha"/>
    </div>
    <div class="banda-item banda-cliente">
      <label>Cliente</label>
      <input type="text" id="cliente" placeholder="Nombre y apellido"/>
    </div>
    <div class="banda-item banda-tel">
      <label>Teléfono cliente</label>
      <input type="text" id="telCliente" placeholder="+54 11 0000-0000"/>
    </div>
    <div class="banda-item banda-mail">
      <label>Mail cliente</label>
      <input type="email" id="mailCliente" placeholder="cliente@email.com" style="min-width:220px;max-width:100%;width:100%;"/>
    </div>
  </div>

  <!-- BARRA ESTADO DE PAGO -->
  <div class="barra-estado">
    <span class="barra-estado-lbl">Estado de pago</span>
    <div class="estado-pago">
      <button class="chip-estado pendiente activo" onclick="setEstado('pendiente',this)">Pendiente</button>
      <button class="chip-estado seniado" onclick="setEstado('seniado',this)">Señado</button>
      <button class="chip-estado pagado" onclick="setEstado('pagado',this)">Pagado</button>
    </div>
  </div>

  <!-- CUERPO -->
  <div class="cuerpo">

    <!-- DATOS DEL TRABAJO -->
    <div>
      <div class="seccion-titulo">📋 Datos del trabajo</div>
      <div class="grid-2">
        <div class="campo">
          <label>Tipo de trabajo</label>
          <input type="text" id="trabajo" placeholder="Ej: Mesada de cocina, escalera, etc."/>
        </div>
        <div class="campo">
          <label>Material</label>
          <input type="text" id="material" placeholder="Ej: Mármol Carrara, Granito Negro..."/>
        </div>
        <div class="campo">
          <label>Bacha / pileta</label>
          <input type="text" id="bacha" placeholder="Ej: Bacha simple, doble, sin bacha..."/>
        </div>
        <div class="campo">
          <label>Dirección de entrega / instalación</label>
          <input type="text" id="direccion" placeholder="Calle, número, localidad"/>
        </div>
      </div>
    </div>

    <!-- DETALLES / ITEMS -->
    <div>
      <div class="seccion-titulo">🔩 Detalle de trabajos</div>
      <div class="tabla-wrap">
        <table>
          <thead>
            <tr>
              <th style="width:50%">Descripción</th>
              <th style="width:25%">Medidas / cantidad</th>
              <th style="width:20%;text-align:right">Subtotal</th>
              <th style="width:5%"></th>
            </tr>
          </thead>
          <tbody id="tablaItems">
          </tbody>
        </table>
      </div>
      <button class="btn-agregar" onclick="agregarFila()">+ Agregar ítem</button>
    </div>

    <!-- TOTALES -->
    <div class="totales-wrap">
      <div class="totales-box">
        <div class="total-row">
          <span class="lbl">Subtotal</span>
          <span class="val">
            <input type="text" id="subtotalInput" placeholder="$ 0,00"
              style="font-size:14px;font-weight:700;color:var(--texto);"
              oninput="subtotalManual = true; recalcularDesdeSubtotal()"/>
          </span>
        </div>
        <div class="total-row">
          <span class="lbl">Descuento</span>
          <span class="val"><input type="text" id="descuento" placeholder="$ 0" oninput="recalcularDesdeSubtotal()"/></span>
        </div>
        <div class="separador"></div>
        <div class="total-row grande">
          <span class="lbl">TOTAL</span>
          <span class="val">
            <input type="text" id="totalInput" placeholder="$ 0,00"
              style="font-size:20px;font-weight:700;color:var(--verde);"
              oninput="totalManual = true; calcularRestante()"/>
          </span>
        </div>
        <div class="separador"></div>
        <div class="total-row">
          <span class="lbl">Abonado</span>
          <span class="val"><input type="text" id="abonado" placeholder="$ 0" oninput="calcularRestante()"/></span>
        </div>
        <div class="total-row grande">
          <span class="lbl" style="color:#c0392b">Resta abonar</span>
          <span class="val">
            <input type="text" id="restaInput" placeholder="$ 0,00" readonly
              style="font-size:20px;font-weight:700;color:#c0392b;cursor:default;"/>
          </span>
        </div>
      </div>
    </div>

    <!-- PLANOS -->
    <div id="seccionPlanos" class="sin-imagen">
      <div class="seccion-titulo" id="tituloPlanos">📐 Planos / croquis <span id="tagOpcional" style="font-weight:400;text-transform:none;letter-spacing:0;color:#aaa;font-size:9px;">(opcional)</span></div>
      <div class="planos-zona" onclick="document.getElementById('inputPlano').click()">
        <div class="icono">🗺</div>
        <div class="texto planos-texto-hint">Hacé clic para subir el plano (PNG o SVG)</div>
        <div class="subtexto planos-texto-hint">Podés cargar el archivo exportado desde Illustrator</div>
        <img id="previewPlano" src="" alt="Plano cargado"/>
        <button class="btn-quitar-plano" id="btnQuitarPlano" onclick="quitarPlano(event)">✕ Quitar plano</button>
      </div>
      <input type="file" id="inputPlano" accept="image/png,image/svg+xml" onchange="cargarPlano(event)"/>
    </div>

    <!-- OBSERVACIONES -->
    <div>
      <div class="seccion-titulo">📝 Observaciones</div>
      <textarea class="obs-input" id="observaciones" placeholder="Notas adicionales, acuerdos, garantía, condiciones de entrega..."></textarea>
    </div>

  </div>

  <!-- FOOTER -->
  <div class="footer">
    <div class="footer-contacto">
      <div style="font-size:13px;font-weight:700;color:#fff;margin-bottom:6px;letter-spacing:0.05em;">CONTACTO</div>

      <div class="linea">
        <span id="link-tel1"></span>
        <input type="text" id="tel1" placeholder="Teléfono / WhatsApp 1"
          style="position:absolute;opacity:0;pointer-events:none;width:0;height:0;" aria-hidden="true"/>
      </div>
      <div class="linea">
        <span id="link-tel2"></span>
        <input type="text" id="tel2" placeholder="Teléfono / WhatsApp 2"
          style="position:absolute;opacity:0;pointer-events:none;width:0;height:0;" aria-hidden="true"/>
      </div>
      <div class="linea">
        <span id="link-dir"></span>
        <input type="text" id="direccionLocal" placeholder="Dirección del local"
          style="position:absolute;opacity:0;pointer-events:none;width:0;height:0;" aria-hidden="true"/>
      </div>
      <div class="linea">
        <span id="link-email"></span>
        <input type="text" id="emailLocal" placeholder="Email"
          style="position:absolute;opacity:0;pointer-events:none;width:0;height:0;" aria-hidden="true"/>
      </div>

      <button id="btnEditarContacto" onclick="abrirEditorContacto()"
        style="margin-top:10px;font-family:'Montserrat',sans-serif;font-size:11px;font-weight:600;
               background:rgba(255,255,255,0.15);border:1px solid rgba(255,255,255,0.3);color:#fff;
               border-radius:6px;padding:5px 12px;cursor:pointer;letter-spacing:0.04em;">
        ✏️ Editar datos de contacto
      </button>
    </div>
    <div class="footer-firma">
      Mármoles y Granitos Quilmes<br/>
      <span style="opacity:0.4;font-size:10px;">Documento no fiscal – Comprobante de trabajo</span>
    </div>
  </div>

  <!-- MODAL EDITOR DE CONTACTO -->
  <div id="modalContacto" style="display:none;position:fixed;inset:0;background:rgba(0,0,0,0.55);z-index:9999;align-items:center;justify-content:center;">
    <div style="background:#fff;border-radius:16px;padding:32px;width:100%;max-width:420px;box-shadow:0 20px 60px rgba(0,0,0,0.3);font-family:'Montserrat',sans-serif;">
      <div style="font-size:15px;font-weight:700;color:var(--texto);margin-bottom:20px;">✏️ Datos de contacto del local</div>
      <div style="display:flex;flex-direction:column;gap:14px;">
        <div>
          <label style="font-size:10px;font-weight:700;text-transform:uppercase;letter-spacing:0.08em;color:var(--gris);display:block;margin-bottom:4px;">📞 WhatsApp 1</label>
          <input id="m-tel1" type="text" placeholder="+54 11 0000-0000"
            style="font-family:'Montserrat',sans-serif;font-size:14px;font-weight:500;color:var(--texto);background:var(--gris-claro);border:1.5px solid transparent;border-radius:8px;padding:10px 14px;width:100%;outline:none;"/>
        </div>
        <div>
          <label style="font-size:10px;font-weight:700;text-transform:uppercase;letter-spacing:0.08em;color:var(--gris);display:block;margin-bottom:4px;">📞 WhatsApp 2</label>
          <input id="m-tel2" type="text" placeholder="+54 11 0000-0000"
            style="font-family:'Montserrat',sans-serif;font-size:14px;font-weight:500;color:var(--texto);background:var(--gris-claro);border:1.5px solid transparent;border-radius:8px;padding:10px 14px;width:100%;outline:none;"/>
        </div>
        <div>
          <label style="font-size:10px;font-weight:700;text-transform:uppercase;letter-spacing:0.08em;color:var(--gris);display:block;margin-bottom:4px;">📍 Dirección</label>
          <input id="m-dir" type="text" placeholder="Calle, número, localidad"
            style="font-family:'Montserrat',sans-serif;font-size:14px;font-weight:500;color:var(--texto);background:var(--gris-claro);border:1.5px solid transparent;border-radius:8px;padding:10px 14px;width:100%;outline:none;"/>
        </div>
        <div>
          <label style="font-size:10px;font-weight:700;text-transform:uppercase;letter-spacing:0.08em;color:var(--gris);display:block;margin-bottom:4px;">✉️ Email</label>
          <input id="m-email" type="text" placeholder="info@marmoleria.com"
            style="font-family:'Montserrat',sans-serif;font-size:14px;font-weight:500;color:var(--texto);background:var(--gris-claro);border:1.5px solid transparent;border-radius:8px;padding:10px 14px;width:100%;outline:none;"/>
        </div>
      </div>
      <div style="display:flex;gap:12px;margin-top:24px;justify-content:flex-end;">
        <button onclick="cerrarEditorContacto(false)"
          style="font-family:'Montserrat',sans-serif;font-size:13px;font-weight:600;background:#fff;color:var(--gris);border:1.5px solid var(--borde);border-radius:8px;padding:10px 20px;cursor:pointer;">
          Cancelar
        </button>
        <button onclick="cerrarEditorContacto(true)"
          style="font-family:'Montserrat',sans-serif;font-size:13px;font-weight:600;background:var(--verde);color:#fff;border:none;border-radius:8px;padding:10px 20px;cursor:pointer;">
          Guardar
        </button>
      </div>
    </div>
  </div>

<script>
// ===== NÚMERO DE RECIBO: default = fecha de hoy DDMMAA =====
function numReciboPorDefecto() {
  const hoy = new Date();
  const dd = String(hoy.getDate()).padStart(2,'0');
  const mm = String(hoy.getMonth()+1).padStart(2,'0');
  const aa = String(hoy.getFullYear()).slice(-2);
  return dd + mm + aa;
}

// ===== FECHA =====
function sincronizarFecha() {
  const raw = document.getElementById('fechaRaw').value;
  if (!raw) return;
  const [y, m, d] = raw.split('-');
  document.getElementById('fecha').value = d + '/' + m + '/' + y;
}

function setFechaHoy() {
  const hoy = new Date();
  const dd = String(hoy.getDate()).padStart(2,'0');
  const mm = String(hoy.getMonth()+1).padStart(2,'0');
  const yyyy = hoy.getFullYear();
  document.getElementById('fechaRaw').value = yyyy + '-' + mm + '-' + dd;
  document.getElementById('fecha').value = dd + '/' + mm + '/' + yyyy;
}

// ===== INICIALIZAR =====
window.onload = () => {
  setFechaHoy();
  const numDefault = numReciboPorDefecto();
  document.getElementById('inputNumero').value = numDefault;
  document.getElementById('numDisplay').textContent = numDefault;
  agregarFila();
  agregarFila();
  cargarStorage();
  actualizarLinksFooter();
};

// ===== GENERAR PDF (compartido entre WA, Gmail y descarga) =====
async function generarPDFBlob() {
  const seccionPlanos = document.getElementById('seccionPlanos');
  const hayImagen     = !seccionPlanos.classList.contains('sin-imagen');

  document.querySelectorAll('.btn-agregar, .btn-eliminar, .btn-quitar-plano').forEach(el => el.style.visibility = 'hidden');
  document.querySelectorAll('.planos-texto-hint').forEach(el => el.style.display = 'none');
  document.getElementById('btnEditarContacto').style.display = 'none';
  if (!hayImagen) seccionPlanos.style.display = 'none';

  const doc    = document.getElementById('documento');
  const A4_PX  = 794;
  const prevStyle = doc.getAttribute('style') || '';
  doc.style.cssText += ';width:' + A4_PX + 'px!important;max-width:' + A4_PX + 'px!important;border-radius:0!important;';

  await new Promise(r => setTimeout(r, 120));

  const canvas = await html2canvas(doc, {
    scale: 2, useCORS: true, backgroundColor: '#ffffff', logging: false,
    width: A4_PX, height: doc.scrollHeight, windowWidth: A4_PX, windowHeight: doc.scrollHeight
  });

  doc.setAttribute('style', prevStyle);
  if (!hayImagen) seccionPlanos.style.display = '';
  document.getElementById('btnEditarContacto').style.display = '';
  document.querySelectorAll('.btn-agregar, .btn-eliminar, .btn-quitar-plano').forEach(el => el.style.visibility = '');
  document.querySelectorAll('.planos-texto-hint').forEach(el => el.style.display = '');

  const { jsPDF } = window.jspdf;
  const pdfW = 210;
  const pdfH = (canvas.height / canvas.width) * pdfW;
  const pdf  = new jsPDF({ orientation: 'portrait', unit: 'mm', format: [pdfW, pdfH] });
  pdf.addImage(canvas.toDataURL('image/jpeg', 0.95), 'JPEG', 0, 0, pdfW, pdfH);

  const num = document.getElementById('inputNumero').value || numReciboPorDefecto();
  const filename = 'MYGQrecibo' + num + '.pdf';
  pdf.save(filename);

  return filename;
}

// ===== WHATSAPP CLIENTE =====
async function abrirWhatsAppCliente() {
  const tel = document.getElementById('telCliente')?.value || '';
  const limpio = tel.replace(/[^\d+]/g, '');
  if (!limpio) { alert('Primero completá el Teléfono cliente en la banda superior.'); return; }

  const btn = document.getElementById('btnWaCliente');
  if (btn) { btn.textContent = '⏳ Generando PDF...'; btn.disabled = true; }

  try {
    const filename = await generarPDFBlob();
    const cliente = document.getElementById('cliente')?.value || '';
    const trabajo = document.getElementById('trabajo')?.value || '';
    const resta   = document.getElementById('restaInput')?.value || '';
    const num     = document.getElementById('inputNumero')?.value || '';
    let msg = '¡Hola' + (cliente ? ' ' + cliente : '') + '! Te enviamos tu recibo N°' + num + ' de Mármoles y Granitos Quilmes.';
    if (trabajo) msg += '\nTrabajo: ' + trabajo;
    if (resta && resta !== '$ 0,00') msg += '\nSaldo pendiente: ' + resta;
    msg += '\n\n📎 El PDF "' + filename + '" se descargó en tu carpeta de descargas — adjuntalo en esta conversación.';
    window.open('https://wa.me/' + limpio.replace('+','') + '?text=' + encodeURIComponent(msg), '_blank');
  } catch(e) {
    alert('Error: ' + e.message);
  } finally {
    if (btn) { btn.innerHTML = '<svg width="16" height="16" viewBox="0 0 24 24" fill="currentColor"><path d="M17.472 14.382c-.297-.149-1.758-.867-2.03-.967-.273-.099-.471-.148-.67.15-.197.297-.767.966-.94 1.164-.173.199-.347.223-.644.075-.297-.15-1.255-.463-2.39-1.475-.883-.788-1.48-1.761-1.653-2.059-.173-.297-.018-.458.13-.606.134-.133.298-.347.446-.52.149-.174.198-.298.298-.497.099-.198.05-.371-.025-.52-.075-.149-.669-1.612-.916-2.207-.242-.579-.487-.5-.669-.51-.173-.008-.371-.01-.57-.01-.198 0-.52.074-.792.372-.272.297-1.04 1.016-1.04 2.479 0 1.462 1.065 2.875 1.213 3.074.149.198 2.096 3.2 5.077 4.487.709.306 1.262.489 1.694.625.712.227 1.36.195 1.871.118.571-.085 1.758-.719 2.006-1.413.248-.694.248-1.289.173-1.413-.074-.124-.272-.198-.57-.347m-5.421 7.403h-.004a9.87 9.87 0 01-5.031-1.378l-.361-.214-3.741.982.998-3.648-.235-.374a9.86 9.86 0 01-1.51-5.26c.001-5.45 4.436-9.884 9.888-9.884 2.64 0 5.122 1.03 6.988 2.898a9.825 9.825 0 012.893 6.994c-.003 5.45-4.437 9.884-9.885 9.884m8.413-18.297A11.815 11.815 0 0012.05 0C5.495 0 .16 5.335.157 11.892c0 2.096.547 4.142 1.588 5.945L.057 24l6.305-1.654a11.882 11.882 0 005.683 1.448h.005c6.554 0 11.89-5.335 11.893-11.893a11.821 11.821 0 00-3.48-8.413z"/></svg> WA Cliente'; btn.disabled = false; }
  }
}

// ===== GMAIL CLIENTE =====
async function enviarGmail() {
  const mailDest = document.getElementById('mailCliente')?.value || '';
  if (!mailDest) { alert('Primero completá el Mail cliente en la banda superior.'); return; }

  const btn = document.getElementById('btnGmailCliente');
  if (btn) { btn.textContent = '⏳ Generando PDF...'; btn.disabled = true; }

  try {
    const filename = await generarPDFBlob();
    const cliente = document.getElementById('cliente')?.value || '';
    const num     = document.getElementById('inputNumero')?.value || '';
    const trabajo = document.getElementById('trabajo')?.value || '';
    const total   = document.getElementById('totalInput')?.value || '';
    const subject = encodeURIComponent('Recibo N°' + num + ' – Mármoles y Granitos Quilmes');
    let body = 'Hola' + (cliente ? ' ' + cliente : '') + ',\n\nTe adjuntamos el recibo N°' + num + ' de Mármoles y Granitos Quilmes.\n';
    if (trabajo) body += 'Trabajo: ' + trabajo + '\n';
    if (total)   body += 'Total: ' + total + '\n';
    body += '\nEl PDF "' + filename + '" se descargó en tu carpeta de descargas — adjuntalo a este mail antes de enviarlo.\n\nSaludos,\nMármoles y Granitos Quilmes';
    window.open('https://mail.google.com/mail/?view=cm&to=' + encodeURIComponent(mailDest) + '&su=' + subject + '&body=' + encodeURIComponent(body), '_blank');
  } catch(e) {
    alert('Error: ' + e.message);
  } finally {
    if (btn) { btn.innerHTML = '<svg width="16" height="16" viewBox="0 0 24 24" fill="currentColor"><path d="M24 5.457v13.909c0 .904-.732 1.636-1.636 1.636h-3.819V11.73L12 16.64l-6.545-4.91v9.273H1.636A1.636 1.636 0 0 1 0 19.366V5.457c0-2.023 2.309-3.178 3.927-1.964L5.455 4.64 12 9.548l6.545-4.91 1.528-1.145C21.69 2.28 24 3.434 24 5.457z"/></svg> Gmail Cliente'; btn.disabled = false; }
  }
}

// ===== ESTADO PAGO =====
function setEstado(tipo, btn) {
  document.querySelectorAll('.chip-estado').forEach(c => c.classList.remove('activo'));
  btn.classList.add('activo');
}

// ===== FILAS TABLA =====
let filaId = 0;
function agregarFila() {
  filaId++;
  const tbody = document.getElementById('tablaItems');
  const tr = document.createElement('tr');
  tr.id = 'fila-' + filaId;
  const fid = filaId;
  tr.innerHTML =
    `<td><input type="text" placeholder="Descripción del ítem"/></td>
     <td><input type="text" placeholder="Ej: 1.20m x 0.60m"/></td>
     <td class="monto"><input type="text" id="sub-${fid}" placeholder="0" oninput="recalcularDesdeFilas()" style="font-weight:700;color:var(--verde);text-align:right;"/></td>
     <td><button class="btn-eliminar" onclick="eliminarFila(${fid})">✕</button></td>`;
  tbody.appendChild(tr);
}

function eliminarFila(id) {
  const fila = document.getElementById('fila-' + id);
  if (fila) fila.remove();
  recalcularDesdeFilas();
}

// ===== CÁLCULOS =====
function parseMonto(str) {
  if (!str) return 0;
  return parseFloat(str.replace(/\./g,'').replace(',','.').replace(/[^0-9.]/g,'')) || 0;
}

function formatMonto(n) {
  return '$ ' + n.toLocaleString('es-AR', {minimumFractionDigits:2, maximumFractionDigits:2});
}

let subtotalManual = false;
let totalManual    = false;

function recalcularDesdeFilas() {
  subtotalManual = false;
  totalManual    = false;
  let sub = 0;
  document.querySelectorAll('#tablaItems tr').forEach(tr => {
    const inp = tr.querySelector('td.monto input');
    if (inp) sub += parseMonto(inp.value);
  });
  document.getElementById('subtotalInput').value = formatMonto(sub);
  recalcularDesdeSubtotal();
}

function recalcularDesdeSubtotal() {
  if (!totalManual) {
    const sub  = parseMonto(document.getElementById('subtotalInput').value);
    const desc = parseMonto(document.getElementById('descuento').value);
    document.getElementById('totalInput').value = formatMonto(Math.max(0, sub - desc));
  }
  calcularRestante();
}

function calcularRestante() {
  const total   = parseMonto(document.getElementById('totalInput').value);
  const abonado = parseMonto(document.getElementById('abonado').value);
  document.getElementById('restaInput').value = formatMonto(Math.max(0, total - abonado));
}

// ===== PLANOS =====
function cargarPlano(e) {
  const file = e.target.files[0];
  if (!file) return;
  const reader = new FileReader();
  reader.onload = ev => {
    const img = document.getElementById('previewPlano');
    img.src = ev.target.result;
    img.style.display = 'block';
    document.getElementById('btnQuitarPlano').style.display = 'inline-block';
    document.querySelectorAll('.planos-texto-hint').forEach(el => el.style.display = 'none');
    document.querySelector('.planos-zona .icono').style.display = 'none';
    document.getElementById('tagOpcional').style.display = 'none';
    document.getElementById('seccionPlanos').classList.remove('sin-imagen');
  };
  reader.readAsDataURL(file);
}

function quitarPlano(e) {
  e.stopPropagation();
  document.getElementById('previewPlano').style.display = 'none';
  document.getElementById('previewPlano').src = '';
  document.getElementById('btnQuitarPlano').style.display = 'none';
  document.querySelectorAll('.planos-texto-hint').forEach(el => el.style.display = '');
  document.querySelector('.planos-zona .icono').style.display = '';
  document.getElementById('inputPlano').value = '';
  document.getElementById('tagOpcional').style.display = '';
  document.getElementById('seccionPlanos').classList.add('sin-imagen');
}

// ===== FOOTER LINKS =====
function actualizarLinksFooter() {
  const tel1  = document.getElementById('tel1').value;
  const tel2  = document.getElementById('tel2').value;
  const dir   = document.getElementById('direccionLocal').value;
  const email = document.getElementById('emailLocal').value;

  function telHTML(num) {
    const limpio = num.replace(/[^\d+]/g, '');
    if (!limpio) return '<span style="color:rgba(255,255,255,0.3);font-size:12px;">📞 Sin número cargado</span>';
    return `<a href="https://wa.me/${limpio.replace('+','')}" target="_blank" class="footer-link">📞 ${num}</a>`;
  }
  function dirHTML(addr) {
    if (!addr) return '<span style="color:rgba(255,255,255,0.3);font-size:12px;">📍 Sin dirección cargada</span>';
    return `<a href="https://maps.google.com/?q=${encodeURIComponent(addr)}" target="_blank" class="footer-link">📍 ${addr}</a>`;
  }
  function emailHTML(mail) {
    if (!mail) return '<span style="color:rgba(255,255,255,0.3);font-size:12px;">✉️ Sin email cargado</span>';
    return `<a href="mailto:${mail}" class="footer-link">✉️ ${mail}</a>`;
  }

  document.getElementById('link-tel1').innerHTML  = telHTML(tel1);
  document.getElementById('link-tel2').innerHTML  = telHTML(tel2);
  document.getElementById('link-dir').innerHTML   = dirHTML(dir);
  document.getElementById('link-email').innerHTML = emailHTML(email);
}

// ===== MODAL EDITOR CONTACTO =====
function abrirEditorContacto() {
  document.getElementById('m-tel1').value  = document.getElementById('tel1').value;
  document.getElementById('m-tel2').value  = document.getElementById('tel2').value;
  document.getElementById('m-dir').value   = document.getElementById('direccionLocal').value;
  document.getElementById('m-email').value = document.getElementById('emailLocal').value;
  const modal = document.getElementById('modalContacto');
  modal.style.display = 'flex';
}

function cerrarEditorContacto(guardar) {
  if (guardar) {
    document.getElementById('tel1').value          = document.getElementById('m-tel1').value;
    document.getElementById('tel2').value          = document.getElementById('m-tel2').value;
    document.getElementById('direccionLocal').value = document.getElementById('m-dir').value;
    document.getElementById('emailLocal').value    = document.getElementById('m-email').value;
    ['tel1','tel2','direccionLocal','emailLocal'].forEach(id => {
      localStorage.setItem('mgq_' + id, document.getElementById(id).value);
    });
    actualizarLinksFooter();
  }
  document.getElementById('modalContacto').style.display = 'none';
}

document.getElementById('modalContacto').addEventListener('click', function(e) {
  if (e.target === this) cerrarEditorContacto(false);
});

// ===== LIMPIAR =====
function limpiarTodo() {
  if (!confirm('¿Limpiar todos los campos?')) return;
  document.getElementById('tablaItems').innerHTML = '';
  filaId = 0;
  subtotalManual = false;
  totalManual    = false;
  agregarFila();
  agregarFila();
  ['cliente','telCliente','mailCliente','trabajo','material','bacha','direccion',
   'descuento','abonado','observaciones'].forEach(id => {
    const el = document.getElementById(id);
    if (el) el.value = '';
  });
  ['subtotalInput','totalInput','restaInput'].forEach(id => {
    document.getElementById(id).value = '$ 0,00';
  });
  const numDefault = numReciboPorDefecto();
  document.getElementById('inputNumero').value = numDefault;
  document.getElementById('numDisplay').textContent = numDefault;
  quitarPlano({stopPropagation:()=>{}});
  setFechaHoy();
}

// ===== DESCARGAR PDF =====
async function descargarPDF() {
  const btn = document.querySelector('.btn-primary');
  btn.textContent = '⏳ Generando PDF...';
  btn.disabled = true;

  document.querySelectorAll('.btn-agregar, .btn-eliminar, .btn-quitar-plano').forEach(el => el.style.visibility = 'hidden');
  document.querySelectorAll('.planos-texto-hint').forEach(el => el.style.display = 'none');
  document.getElementById('btnEditarContacto').style.display = 'none';

  const seccionPlanos = document.getElementById('seccionPlanos');
  const hayImagen     = !seccionPlanos.classList.contains('sin-imagen');
  if (!hayImagen) seccionPlanos.style.display = 'none';

  try {
    const doc = document.getElementById('documento');
    const A4_PX = 794;
    const prevStyle = doc.getAttribute('style') || '';
    doc.style.cssText += ';width:' + A4_PX + 'px!important;max-width:' + A4_PX + 'px!important;border-radius:0!important;';

    await new Promise(r => setTimeout(r, 120));

    const canvas = await html2canvas(doc, {
      scale: 2,
      useCORS: true,
      backgroundColor: '#ffffff',
      logging: false,
      width: A4_PX,
      height: doc.scrollHeight,
      windowWidth: A4_PX,
      windowHeight: doc.scrollHeight
    });

    doc.setAttribute('style', prevStyle);

    const { jsPDF } = window.jspdf;
    const pdfW = 210;
    const pdfH = (canvas.height / canvas.width) * pdfW;
    const pdf  = new jsPDF({ orientation: 'portrait', unit: 'mm', format: [pdfW, pdfH] });
    pdf.addImage(canvas.toDataURL('image/jpeg', 0.95), 'JPEG', 0, 0, pdfW, pdfH);

    const num = document.getElementById('inputNumero').value || numReciboPorDefecto();
    pdf.save('MYGQrecibo' + num + '.pdf');

  } catch (err) {
    alert('Error al generar el PDF: ' + err.message);
  } finally {
    if (!hayImagen) seccionPlanos.style.display = '';
    document.getElementById('btnEditarContacto').style.display = '';
    document.querySelectorAll('.btn-agregar, .btn-eliminar, .btn-quitar-plano').forEach(el => el.style.visibility = '');
    document.querySelectorAll('.planos-texto-hint').forEach(el => el.style.display = '');
    btn.textContent = '⬇ Descargar PDF';
    btn.disabled = false;
  }
}

// ===== STORAGE =====
function cargarStorage() {
  ['tel1','tel2','direccionLocal','emailLocal'].forEach(id => {
    const val = localStorage.getItem('mgq_' + id);
    if (val) document.getElementById(id).value = val;
  });
}
</script>
</div>
</body>
</html>
