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
    padding: 36px 44px;
    display: flex;
    justify-content: space-between;
    align-items: center;
    gap: 20px;
  }

  .logo-wrap svg {
    width: 180px;
    height: auto;
  }
  .logo-wrap svg .st0 { fill: #fff !important; }

  .header-right {
    text-align: right;
    color: #fff;
  }
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
    border-bottom: 3px solid var(--verde);
    padding: 14px 44px;
    display: flex;
    gap: 30px;
    flex-wrap: wrap;
  }

  .banda-item label {
    font-size: 10px;
    font-weight: 700;
    text-transform: uppercase;
    letter-spacing: 0.1em;
    color: var(--verde);
    display: block;
    margin-bottom: 3px;
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
    min-width: 160px;
    outline: none;
    transition: border-color 0.2s;
  }
  .banda-item input:focus { border-bottom-color: var(--verde); }

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
  .totales-box {
    background: var(--verde-bg);
    border-radius: 12px;
    padding: 22px 28px;
    min-width: 280px;
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
    width: 120px;
    text-align: right;
    outline: none;
  }
  .total-row .val input:focus { border-bottom-color: var(--verde); }
  .total-row.grande .val input {
    font-size: 20px;
    color: var(--verde);
    width: 140px;
  }

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
  .chip-estado.activo.parcial { background: #cce5ff; border-color: #2b7bdc; color: #1a4a8a; }
  .chip-estado.activo.pagado { background: #d4edda; border-color: var(--verde); color: var(--verde-oscuro); }

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
    gap: 4px;
    font-size: 12px;
  }
  .footer-contacto .linea {
    display: flex;
    align-items: center;
    gap: 8px;
  }
  .footer-contacto .linea input {
    font-family: 'Montserrat', sans-serif;
    font-size: 12px;
    background: transparent;
    border: none;
    border-bottom: 1px solid rgba(255,255,255,0.2);
    color: rgba(255,255,255,0.85);
    outline: none;
    min-width: 180px;
    padding: 2px 0;
    transition: border-color 0.2s;
  }
  .footer-contacto .linea input:focus { border-bottom-color: rgba(255,255,255,0.7); }
  .footer-firma {
    text-align: right;
    font-size: 11px;
    color: rgba(255,255,255,0.5);
    font-style: italic;
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
  }

  /* ===== RESPONSIVE ===== */
  @media (max-width: 640px) {
    .header { flex-direction: column; text-align: center; }
    .header-right { text-align: center; }
    .cuerpo { padding: 24px 20px; }
    .banda { padding: 14px 20px; }
    .footer { padding: 20px; flex-direction: column; align-items: flex-start; }
    .grid-2, .grid-3 { grid-template-columns: 1fr; }
    .totales-wrap { justify-content: stretch; }
    .totales-box { min-width: 100%; }
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
  <button class="btn btn-primary" onclick="descargarPDF()">
    ⬇ Descargar PDF
  </button>
</div>

<!-- DOCUMENTO -->
<div id="documento">

  <!-- HEADER -->
  <div class="header">
    <div class="logo-wrap">
      <!-- SVG LOGO MÁRMOLES Y GRANITOS QUILMES -->
      <svg version="1.1" viewBox="0 0 428.82 415.31" xmlns="http://www.w3.org/2000/svg">
        <style>.st0{fill:#ffffff;}</style>
        <g>
          <g>
            <path class="st0" d="M186.17,226.92c0,3.4,2.13,5.57,5.46,5.56c2.89,0,5.06-2.41,5.06-5.62V123.25c0-13.97,0-43.57,0-43.57h11.44c0,0,0,0.43,0,1.17l5.33,29.7h0.17l4.82-30.85l0.21,0.02c0-0.01,0-0.02,0-0.02h11.61c0,0,0.02,57.72,0.02,85.98v61.43c0,2.49,2.08,5.05,4.2,5.19c3.35,0.22,5.92-1.52,6.1-4.46c0.23-3.81,0.2-7.63,0.22-11.45c0.02-7.86,0-15.72,0-23.58v-92.08c0-8.76,0.02-17.52-0.01-26.28c-0.01-3.21-2.11-5.49-5.09-5.5c-14.8-0.02-29.6-0.02-44.4,0c-2.56,0-4.61,1.59-4.93,4.12c-0.29,2.39-0.2,4.83-0.2,7.26c-0.02,7.85-0.01,15.71-0.01,23.57V226.92z"/>
            <path class="st0" d="M214.41,0C149.47,0,96.82,52.64,96.82,117.58c0,7.02,0.62,13.9,1.8,20.58c7.09,40.19,34.6,73.34,71.43,88.34c2,0.12,4.49-2.42,4.49-4.63V74.26c0-3.2-1.99-5.3-5.17-5.31c-8.3-0.03-16.6-0.04-24.91,0c-4.88,0.03-9.41,3.88-10.4,8.73c-1.01,5.02,1.75,10.27,6.41,12.18c7.6,3.11,15.26-2.28,15.27-10.73c0.01-2.46,0.77-4.55,3.07-5.74c2.14-1.11,4.33-1.05,6.22,0.49c1.91,1.56,2.31,3.67,2.22,6.15c-0.17,4.7-1.62,8.88-4.33,12.63c-4.05,5.6-9.53,8.82-16.42,9.6c-9.57,1.09-18.4-5.06-22.24-13.15c-3.8-7.99-1.96-18.45,4.27-24.78c4.65-4.72,10.26-7.02,16.86-7.02c9.47,0.01,18.95,0,28.42,0h106.72c9.66,0,17.65,5.37,21.3,14.31c3.66,8.98,0.91,19.55-6.9,25.77c-4.17,3.32-8.99,5.38-14.4,5.01c-8.41-0.59-15.05-4.41-19.14-11.94c-2.35-4.35-3.84-9.03-2.51-14.06c0.69-2.64,2.96-3.94,6.35-3.59c2.26,0.24,3.78,1.66,4.35,3.98c0.76,3.06,1.12,6.36,2.52,9.1c1.72,3.38,5.1,5.08,8.97,4.88c4.94-0.25,8.85-2.66,10.22-7.45c2.01-6.99-2.06-14.08-10.24-14.33c-7.79-0.24-15.59-0.05-23.39-0.05c-3.23,0-5.4,2.21-5.4,5.45v118.46c0,7.92-0.06,15.84,0.03,23.76c0.02,2.22-0.07,4.67,0.81,6.6c1.33,2.95,4.06,3.47,7.28,2.1l0.2-0.08c7.67-3.26,16.04-7.74,23.97-13.27c0.07-0.05,0.14-0.1,0.21-0.15c11.96-8.94,22.15-20.11,29.98-32.88c10.94-17.86,17.25-38.87,17.25-61.35C331.99,52.64,279.35,0,214.41,0z M176.32,17.29c6.45-0.01,12.9-0.01,19.35-0.01h23.85c7.96,0,15.91-0.01,23.86,0.01c2.63,0.01,5.28-0.03,7.9,0.22c2.71,0.25,4.91,3.23,4.66,5.92c-0.29,3.02-2.78,5.48-5.59,5.48h-73.97c-2.7,0-5.4-2.8-5.41-5.61C170.96,20.12,173.46,17.29,176.32,17.29z M273.2,45.95c-1.22,1.82-2.84,2.89-5.14,2.9c-7.47,0.01-14.93,0.13-22.4,0.13c-6.97,0-13.94-0.12-20.91-0.14c-7.83-0.03-15.66-0.01-23.49-0.01h-42.52c-3.28,0-6.03-2.74-6.01-6c0-2.77,2.89-5.41,6-5.44c2.6-0.02,5.21,0,7.81,0h76.7c8.08,0,16.16,0.16,24.23-0.06C272.68,37.19,275.56,42.41,273.2,45.95z"/>
            <path class="st0" d="M276.22,257.73c-3.06-1.91-6.33-2.98-9.96-3.83c-23.31-5.49-55.89,8.07-68.12,13.87c-5.4,2.61-11.29,4.52-17.57,5.44c-3.66,0.63-7.86,0.84-11.49-0.01c-4.55-1.07-7.78-3.63-7.88-9.94c-0.04-1.2,0.3-2.62,0.56-3.75c0.4-1.7,1.17-3.62,2.47-5.1c5.39-7.33,24.63-13.92,41.42-18.4c3.65-0.98,5.88-1.24,8.87-1.89c0.28-0.03,0.56-0.09,0.83-0.18c0.06-0.01,0.11-0.02,0.16-0.04l0.02-0.03c1.78-0.71,3.13-2.66,3.13-4.89v-115.5l-3.03,9.64l-2.08,6.41l-2.08-6.41l-3.33-9.53v82.63c0,5.93,0,11.86,0,17.78c-0.44,5.48,1.56,14.67-4.57,17.86c-4.15,1.85-6.05,2.67-10.77,4.31c-1.22,0.41-2.43,0.85-3.64,1.3c-12.38,4.54-24.01,10.25-27.96,16.22c-1.07,1.83-2.14,3.68-2.61,5.66c-0.73,3.11-0.43,5.89,0.72,9.15c2.36,6.24,9.33,10.57,16.59,12.28c3.03,0.71,5.82,1.07,8.74,0.87c6.52-0.56,14.22-2.05,22.05-4.08c16.13-3.38,34.77-7.97,52.33-3.84c6.36,1.5,12.44,4.13,18.2,8.18c16.73,11.72,22.03,28.52,22.48,30.72l2.92-0.21C302.63,312.42,304.21,274.5,276.22,257.73z"/>
          </g>
          <g>
            <path class="st0" d="M0,359.52v-28.84h4.41l12.61,21.05h-2.31l12.4-21.05h4.41l0.08,28.84h-5.15v-20.89h1.03l-10.46,17.63h-2.43L3.87,338.63h1.24v20.89H0z"/>
            <path class="st0" d="M45.49,359.81c-1.65,0-3.09-0.28-4.33-0.84c-1.24-0.56-2.19-1.34-2.86-2.35c-0.67-1-1.01-2.14-1.01-3.4c0-1.24,0.29-2.35,0.89-3.34c0.59-0.99,1.56-1.77,2.9-2.35c1.35-0.58,3.13-0.87,5.36-0.87h6.39v3.42H46.8c-1.73,0-2.9,0.28-3.52,0.84c-0.62,0.56-0.93,1.27-0.93,2.12c0,0.91,0.37,1.63,1.11,2.18c0.74,0.55,1.77,0.82,3.09,0.82c1.26,0,2.4-0.29,3.4-0.87c1-0.58,1.74-1.43,2.2-2.55l0.82,3.09c-0.49,1.29-1.37,2.29-2.64,3.01C49.08,359.45,47.46,359.81,45.49,359.81z M52.41,359.52v-4.49l-0.25-0.91v-7.79c0-1.51-0.46-2.68-1.38-3.52c-0.92-0.84-2.31-1.26-4.18-1.26c-1.21,0-2.4,0.19-3.58,0.58c-1.18,0.38-2.2,0.92-3.05,1.61l-2.02-3.75c1.21-0.91,2.62-1.59,4.24-2.06c1.62-0.47,3.31-0.7,5.07-0.7c3.21,0,5.69,0.77,7.42,2.31s2.6,3.9,2.6,7.09v12.9H52.41z M44.25,334.51l5.56-5.65h6.1l-7.21,5.65H44.25z"/>
            <path class="st0" d="M63.9,359.52v-22h4.9v6.06l-0.58-1.77c0.63-1.48,1.66-2.62,3.09-3.4c1.43-0.78,3.17-1.17,5.23-1.17v4.9c-0.19-0.03-0.38-0.05-0.58-0.06c-0.19-0.01-0.37-0.02-0.54-0.02c-1.98,0-3.54,0.56-4.7,1.69c-1.15,1.13-1.73,2.84-1.73,5.15v10.63H63.9z"/>
            <path class="st0" d="M108.73,337.23c1.76,0,3.32,0.34,4.68,1.03c1.36,0.69,2.42,1.74,3.19,3.17c0.77,1.43,1.15,3.25,1.15,5.48v12.61h-5.11v-11.95c0-1.95-0.43-3.41-1.3-4.37c-0.87-0.96-2.09-1.44-3.69-1.44c-1.13,0-2.12,0.25-2.99,0.74c-0.87,0.49-1.54,1.23-2.02,2.2c-0.48,0.97-0.72,2.19-0.72,3.65v11.17h-5.11v-11.95c0-1.95-0.43-3.41-1.3-4.37c-0.87-0.96-2.09-1.44-3.69-1.44c-1.13,0-2.12,0.25-2.99,0.74c-0.87,0.49-1.54,1.23-2.02,2.2c-0.48,0.97-0.72,2.19-0.72,3.65v11.17H81v-22h4.9v5.89L85,341.64c0.74-1.4,1.82-2.49,3.23-3.25c1.41-0.77,3.04-1.15,4.88-1.15c2.06,0,3.85,0.51,5.36,1.52c1.51,1.02,2.51,2.58,3.01,4.7l-1.98-0.74c0.69-1.62,1.85-2.94,3.5-3.96C104.65,337.74,106.56,337.23,108.73,337.23z"/>
            <path class="st0" d="M134.27,359.81c-2.22,0-4.22-0.49-5.99-1.46s-3.17-2.31-4.18-4.02s-1.52-3.64-1.52-5.81c0-2.22,0.51-4.17,1.52-5.85c1.02-1.67,2.4-3,4.16-3.98c1.76-0.97,3.76-1.46,6.02-1.46c2.28,0,4.31,0.49,6.08,1.46c1.77,0.98,3.16,2.3,4.16,3.98c1,1.68,1.5,3.63,1.5,5.85c0,2.2-0.5,4.14-1.5,5.83c-1,1.69-2.4,3.02-4.18,4C138.54,359.32,136.52,359.81,134.27,359.81z M134.27,355.44c1.26,0,2.39-0.28,3.38-0.84c0.99-0.56,1.76-1.37,2.33-2.43c0.56-1.06,0.84-2.27,0.84-3.65c0-1.4-0.28-2.62-0.84-3.65c-0.56-1.03-1.34-1.83-2.33-2.39c-0.99-0.56-2.1-0.84-3.34-0.84c-1.24,0-2.35,0.28-3.34,0.84c-0.99,0.56-1.77,1.36-2.35,2.39c-0.58,1.03-0.87,2.25-0.87,3.65c0,1.37,0.29,2.59,0.87,3.65c0.58,1.06,1.36,1.87,2.35,2.43C131.97,355.16,133.06,355.44,134.27,355.44z"/>
            <path class="st0" d="M151,359.52v-30.57h5.11v30.57H151z"/>
            <path class="st0" d="M173.21,359.81c-2.45,0-4.58-0.49-6.41-1.46s-3.24-2.31-4.24-4c-1-1.69-1.5-3.63-1.5-5.83c0-2.2,0.49-4.14,1.46-5.83c0.97-1.69,2.32-3.02,4.04-4c1.72-0.97,3.67-1.46,5.87-1.46c2.14,0,4.05,0.47,5.73,1.42c1.68,0.95,2.99,2.27,3.96,3.98c0.96,1.7,1.44,3.72,1.44,6.06c0,0.19-0.01,0.43-0.02,0.72c-0.01,0.29-0.04,0.54-0.06,0.76h-18.33v-3.42h15.62l-2.06,1.07c0.03-1.24-0.23-2.34-0.76-3.32s-1.27-1.73-2.2-2.27c-0.93-0.54-2.03-0.8-3.3-0.8c-1.24,0-2.34,0.27-3.32,0.8c-0.98,0.54-1.72,1.3-2.25,2.29c-0.52,0.99-0.78,2.13-0.78,3.42v0.82c0,1.32,0.29,2.49,0.89,3.5c0.59,1.02,1.44,1.81,2.53,2.37c1.1,0.56,2.38,0.84,3.83,0.84c1.24,0,2.35-0.21,3.34-0.62c0.99-0.41,1.87-1.02,2.64-1.81l2.8,3.21c-1.02,1.15-2.27,2.03-3.77,2.64C176.84,359.51,175.13,359.81,173.21,359.81z"/>
            <path class="st0" d="M195.33,359.81c-1.87,0-3.64-0.24-5.31-0.72c-1.68-0.48-3.02-1.06-4.04-1.75l1.98-3.91c0.99,0.63,2.17,1.15,3.54,1.54c1.37,0.4,2.76,0.6,4.16,0.6c1.59,0,2.75-0.21,3.48-0.64c0.73-0.43,1.09-1.01,1.09-1.75c0-0.6-0.25-1.06-0.74-1.38c-0.49-0.32-1.14-0.56-1.94-0.72c-0.8-0.17-1.68-0.32-2.66-0.45c-0.98-0.14-1.95-0.32-2.93-0.56c-0.98-0.23-1.86-0.58-2.66-1.03c-0.8-0.45-1.44-1.06-1.94-1.83c-0.49-0.77-0.74-1.8-0.74-3.09c0-1.37,0.4-2.58,1.19-3.61c0.8-1.03,1.92-1.83,3.36-2.41c1.44-0.58,3.15-0.87,5.13-0.87c1.46,0,2.95,0.17,4.47,0.52c1.52,0.34,2.79,0.82,3.81,1.42l-2.02,3.91c-1.02-0.6-2.06-1.02-3.13-1.26c-1.07-0.23-2.13-0.35-3.17-0.35c-1.54,0-2.69,0.23-3.46,0.68c-0.77,0.45-1.15,1.04-1.15,1.75c0,0.66,0.25,1.15,0.74,1.48c0.49,0.33,1.14,0.58,1.94,0.76c0.8,0.18,1.68,0.34,2.66,0.5c0.97,0.15,1.94,0.34,2.9,0.58c0.96,0.23,1.85,0.56,2.66,0.99c0.81,0.43,1.46,1.02,1.96,1.79c0.49,0.77,0.74,1.77,0.74,3.01c0,1.4-0.41,2.6-1.21,3.61c-0.81,1-1.95,1.79-3.42,2.35C199.15,359.53,197.39,359.81,195.33,359.81z"/>
            <path class="st0" d="M270.23,359.93c-2.25,0-4.32-0.36-6.2-1.09c-1.88-0.73-3.52-1.76-4.92-3.11c-1.4-1.35-2.49-2.91-3.25-4.7c-0.77-1.79-1.15-3.76-1.15-5.93s0.38-4.15,1.15-5.93c0.77-1.79,1.86-3.35,3.28-4.7c1.41-1.35,3.06-2.38,4.94-3.11c1.88-0.73,3.98-1.09,6.28-1.09c2.42,0,4.61,0.4,6.57,1.19c1.96,0.8,3.62,1.96,4.96,3.5l-3.38,3.3c-1.13-1.13-2.34-1.96-3.65-2.49c-1.3-0.54-2.74-0.8-4.31-0.8c-1.54,0-2.95,0.25-4.22,0.74s-2.38,1.2-3.32,2.1s-1.66,1.98-2.18,3.21c-0.52,1.24-0.78,2.6-0.78,4.08c0,1.46,0.26,2.81,0.78,4.06c0.52,1.25,1.25,2.33,2.18,3.23s2.03,1.61,3.3,2.1c1.26,0.49,2.66,0.74,4.2,0.74c1.43,0,2.81-0.23,4.14-0.68s2.6-1.21,3.81-2.29l3.01,4c-1.48,1.21-3.21,2.12-5.19,2.74C274.31,359.62,272.29,359.93,270.23,359.93z M276.46,355.57v-10.75h5.03v11.45L276.46,355.57z"/>
            <path class="st0" d="M288.12,359.52v-22h4.9v6.06l-0.58-1.77c0.63-1.48,1.66-2.62,3.09-3.4c1.43-0.78,3.17-1.17,5.23-1.17v4.9c-0.19-0.03-0.38-0.05-0.58-0.06c-0.19-0.01-0.37-0.02-0.54-0.02c-1.98,0-3.54,0.56-4.7,1.69c-1.15,1.13-1.73,2.84-1.73,5.15v10.63H288.12z"/>
            <path class="st0" d="M310.24,359.81c-1.65,0-3.09-0.28-4.33-0.84c-1.24-0.56-2.19-1.34-2.86-2.35c-0.67-1-1.01-2.14-1.01-3.4c0-1.24,0.29-2.35,0.89-3.34c0.59-0.99,1.56-1.77,2.91-2.35c1.34-0.58,3.13-0.87,5.36-0.87h6.39v3.42h-6.02c-1.73,0-2.9,0.28-3.52,0.84c-0.62,0.56-0.93,1.27-0.93,2.12c0,0.91,0.37,1.63,1.11,2.18c0.74,0.55,1.77,0.82,3.09,0.82c1.26,0,2.4-0.29,3.4-0.87c1-0.58,1.74-1.43,2.2-2.55l0.82,3.09c-0.49,1.29-1.37,2.29-2.64,3.01C313.84,359.45,312.22,359.81,310.24,359.81z M317.16,359.52v-4.49l-0.25-0.91v-7.79c0-1.51-0.46-2.68-1.38-3.52s-2.31-1.26-4.18-1.26c-1.21,0-2.4,0.19-3.58,0.58c-1.18,0.38-2.2,0.92-3.05,1.61l-2.02-3.75c1.21-0.91,2.62-1.59,4.24-2.06c1.62-0.47,3.31-0.7,5.07-0.7c3.21,0,5.69,0.77,7.42,2.31s2.6,3.9,2.6,7.09v12.9H317.16z"/>
            <path class="st0" d="M341.1,337.23c1.79,0,3.36,0.34,4.72,1.03c1.36,0.69,2.44,1.74,3.25,3.17c0.81,1.43,1.21,3.25,1.21,5.48v12.61h-5.15v-11.95c0-1.95-0.45-3.41-1.36-4.37c-0.91-0.96-2.2-1.44-3.87-1.44c-1.21,0-2.28,0.25-3.21,0.74c-0.93,0.49-1.66,1.23-2.16,2.2c-0.51,0.97-0.76,2.2-0.76,3.69v11.12h-5.11v-22h4.9v5.97l-0.87-1.85c0.74-1.4,1.85-2.49,3.32-3.25C337.48,337.62,339.18,337.23,341.1,337.23z"/>
            <path class="st0" d="M359.43,333.89c-0.96,0-1.75-0.3-2.37-0.91c-0.62-0.6-0.93-1.35-0.93-2.22c0-0.82,0.31-1.54,0.93-2.14c0.62-0.6,1.41-0.91,2.37-0.91s1.75,0.28,2.37,0.84c0.62,0.56,0.93,1.28,0.93,2.16c0,0.88-0.3,1.63-0.91,2.25C361.22,333.59,360.42,333.89,359.43,333.89z M356.88,359.52v-22h5.11v22H356.88z"/>
            <path class="st0" d="M365.82,341.8v-4.12h14.67v4.12H365.82z M377.03,359.81c-2.42,0-4.29-0.62-5.6-1.87s-1.98-3.08-1.98-5.5v-19.78h5.11v19.61c0,1.04,0.27,1.85,0.82,2.43c0.55,0.58,1.32,0.87,2.31,0.87c1.13,0,2.06-0.3,2.8-0.91l1.48,3.67c-0.63,0.5-1.39,0.87-2.27,1.11C378.82,359.69,377.93,359.81,377.03,359.81z"/>
            <path class="st0" d="M395.36,359.81c-2.22,0-4.22-0.49-5.99-1.46s-3.17-2.31-4.18-4.02c-1.02-1.7-1.52-3.64-1.52-5.81c0-2.22,0.51-4.17,1.52-5.85c1.02-1.67,2.4-3,4.16-3.98c1.76-0.97,3.76-1.46,6.01-1.46c2.28,0,4.31,0.49,6.08,1.46c1.77,0.98,3.16,2.3,4.16,3.98c1,1.68,1.5,3.63,1.5,5.85c0,2.2-0.5,4.14-1.5,5.83c-1,1.69-2.4,3.02-4.18,4S397.61,359.81,395.36,359.81z M395.36,355.44c1.26,0,2.39-0.28,3.38-0.84c0.99-0.56,1.76-1.37,2.33-2.43c0.56-1.06,0.84-2.27,0.84-3.65c0-1.4-0.28-2.62-0.84-3.65c-0.56-1.03-1.34-1.83-2.33-2.39c-0.99-0.56-2.1-0.84-3.34-0.84c-1.24,0-2.35,0.28-3.34,0.84c-0.99,0.56-1.77,1.36-2.35,2.39c-0.58,1.03-0.87,2.25-0.87,3.65c0,1.37,0.29,2.59,0.87,3.65c0.58,1.06,1.36,1.87,2.35,2.43C393.05,355.16,394.15,355.44,395.36,355.44z"/>
            <path class="st0" d="M418.89,359.81c-1.87,0-3.64-0.24-5.31-0.72c-1.68-0.48-3.02-1.06-4.04-1.75l1.98-3.91c0.99,0.63,2.17,1.15,3.54,1.54c1.37,0.4,2.76,0.6,4.16,0.6c1.59,0,2.75-0.21,3.48-0.64c0.73-0.43,1.09-1.01,1.09-1.75c0-0.6-0.25-1.06-0.74-1.38c-0.5-0.32-1.14-0.56-1.94-0.72c-0.8-0.17-1.68-0.32-2.66-0.45c-0.97-0.14-1.95-0.32-2.92-0.56c-0.98-0.23-1.86-0.58-2.66-1.03c-0.8-0.45-1.44-1.06-1.94-1.83c-0.5-0.77-0.74-1.8-0.74-3.09c0-1.37,0.4-2.58,1.2-3.61c0.8-1.03,1.92-1.83,3.36-2.41c1.44-0.58,3.15-0.87,5.13-0.87c1.46,0,2.95,0.17,4.47,0.52c1.52,0.34,2.79,0.82,3.81,1.42l-2.02,3.91c-1.02-0.6-2.06-1.02-3.13-1.26c-1.07-0.23-2.13-0.35-3.17-0.35c-1.54,0-2.69,0.23-3.46,0.68c-0.77,0.45-1.15,1.04-1.15,1.75c0,0.66,0.25,1.15,0.74,1.48c0.49,0.33,1.14,0.58,1.94,0.76c0.8,0.18,1.68,0.34,2.66,0.5c0.97,0.15,1.94,0.34,2.9,0.58c0.96,0.23,1.85,0.56,2.66,0.99c0.81,0.43,1.46,1.02,1.96,1.79c0.49,0.77,0.74,1.77,0.74,3.01c0,1.4-0.41,2.6-1.21,3.61c-0.81,1-1.95,1.79-3.42,2.35C422.71,359.53,420.95,359.81,418.89,359.81z"/>
            <path class="st0" d="M131.45,409.38c-2.22,0-4.29-0.37-6.2-1.11c-1.91-0.74-3.56-1.79-4.96-3.13c-1.4-1.34-2.49-2.92-3.25-4.72s-1.15-3.76-1.15-5.87c0-2.14,0.38-4.11,1.15-5.89c0.77-1.79,1.85-3.35,3.25-4.7s3.05-2.39,4.94-3.13c1.9-0.74,3.97-1.11,6.22-1.11c2.23,0,4.29,0.36,6.18,1.09c1.9,0.73,3.54,1.77,4.92,3.11c1.39,1.35,2.46,2.92,3.23,4.72c0.77,1.8,1.15,3.77,1.15,5.91c0,2.14-0.38,4.11-1.15,5.91c-0.77,1.8-1.85,3.37-3.23,4.72c-1.39,1.35-3.03,2.38-4.92,3.11C135.74,409.01,133.68,409.38,131.45,409.38z M131.45,404.68c1.46,0,2.79-0.25,4.02-0.74s2.29-1.2,3.21-2.12c0.92-0.92,1.63-2,2.12-3.23c0.49-1.24,0.74-2.58,0.74-4.04c0-1.48-0.25-2.84-0.74-4.06c-0.49-1.22-1.2-2.29-2.12-3.21c-0.92-0.92-1.99-1.63-3.21-2.12s-2.56-0.74-4.02-0.74c-1.46,0-2.81,0.25-4.06,0.74c-1.25,0.49-2.33,1.2-3.23,2.12c-0.91,0.92-1.62,1.99-2.14,3.21c-0.52,1.22-0.78,2.58-0.78,4.06c0,1.46,0.26,2.8,0.78,4.04c0.52,1.24,1.24,2.31,2.14,3.23c0.91,0.92,1.98,1.63,3.23,2.12C128.64,404.43,129.99,404.68,131.45,404.68z M140.89,415.31c-1.04,0-2.03-0.12-2.97-0.37c-0.93-0.25-1.87-0.63-2.8-1.15c-0.93-0.52-1.9-1.22-2.88-2.1c-0.99-0.88-2.07-1.95-3.25-3.21l5.69-1.44c0.77,1.02,1.5,1.83,2.2,2.43c0.7,0.6,1.39,1.04,2.06,1.32c0.67,0.27,1.34,0.41,2,0.41c1.92,0,3.58-0.78,4.99-2.35l2.43,3.01C146.45,414.15,143.96,415.31,140.89,415.31z"/>
            <path class="st0" d="M164.99,409.38c-3.93,0-7.01-1.11-9.25-3.34c-2.24-2.23-3.36-5.45-3.36-9.68v-16.23h5.36v16.07c0,2.97,0.64,5.12,1.92,6.47c1.28,1.35,3.08,2.02,5.42,2.02c2.33,0,4.13-0.67,5.4-2.02c1.26-1.35,1.9-3.5,1.9-6.47v-16.07h5.27v16.23c0,4.23-1.12,7.46-3.36,9.68C172.04,408.26,168.94,409.38,164.99,409.38z"/>
            <path class="st0" d="M185.13,408.96v-28.84h5.36v28.84H185.13z"/>
            <path class="st0" d="M198.28,408.96v-28.84h5.36v24.31h15.08v4.53H198.28z"/>
            <path class="st0" d="M222.96,408.96v-28.84h4.41l12.61,21.05h-2.31l12.4-21.05h4.41l0.08,28.84h-5.15v-20.89h1.03l-10.46,17.63h-2.43l-10.71-17.63h1.24v20.89H222.96z"/>
            <path class="st0" d="M267.66,404.43h16.27v4.53H262.3v-28.84h21.05v4.53h-15.7V404.43z M267.25,392.11h14.34v4.41h-14.34V392.11z"/>
            <path class="st0" d="M298.85,409.38c-2.25,0-4.4-0.32-6.45-0.97c-2.05-0.64-3.69-1.48-4.92-2.49l1.85-4.16c1.15,0.91,2.58,1.66,4.29,2.27c1.7,0.6,3.45,0.91,5.23,0.91c1.51,0,2.73-0.16,3.67-0.49c0.93-0.33,1.62-0.78,2.06-1.34c0.44-0.56,0.66-1.2,0.66-1.92c0-0.88-0.32-1.59-0.95-2.12c-0.63-0.54-1.45-0.96-2.45-1.28c-1-0.32-2.12-0.6-3.34-0.87c-1.22-0.26-2.44-0.58-3.67-0.97s-2.34-0.87-3.36-1.46c-1.02-0.59-1.83-1.38-2.45-2.37c-0.62-0.99-0.93-2.25-0.93-3.79c0-1.57,0.42-3,1.26-4.31s2.12-2.35,3.83-3.13c1.72-0.78,3.89-1.17,6.53-1.17c1.73,0,3.45,0.22,5.15,0.66c1.7,0.44,3.19,1.07,4.45,1.9l-1.69,4.16c-1.29-0.77-2.62-1.34-4-1.71c-1.37-0.37-2.69-0.56-3.96-0.56c-1.46,0-2.65,0.18-3.58,0.54c-0.93,0.36-1.61,0.83-2.04,1.42c-0.43,0.59-0.64,1.24-0.64,1.96c0,0.88,0.31,1.59,0.93,2.12c0.62,0.54,1.43,0.96,2.43,1.26c1,0.3,2.12,0.59,3.36,0.87c1.24,0.28,2.46,0.6,3.67,0.97c1.21,0.37,2.32,0.85,3.34,1.44c1.02,0.59,1.83,1.37,2.45,2.35c0.62,0.98,0.93,2.22,0.93,3.73c0,1.54-0.42,2.96-1.26,4.26c-0.84,1.3-2.12,2.35-3.85,3.13C303.67,408.98,301.49,409.38,298.85,409.38z"/>
          </g>
        </g>
      </svg>
    </div>

    <div class="header-right">
      <h1>Recibo</h1>
      <div class="num-recibo">N°&nbsp;<span id="numDisplay">–</span></div>
      <div class="badge-num">
        <input id="inputNumero" type="text" placeholder="001" style="background:transparent;border:none;color:#fff;font-family:'Montserrat',sans-serif;font-size:14px;font-weight:600;width:80px;text-align:center;outline:none;" oninput="document.getElementById('numDisplay').textContent=this.value||'–'"/>
      </div>
    </div>
  </div>

  <!-- BANDA DATOS BÁSICOS -->
  <div class="banda">
    <div class="banda-item">
      <label>Fecha</label>
      <input type="date" id="fecha" value=""/>
    </div>
    <div class="banda-item">
      <label>Cliente</label>
      <input type="text" id="cliente" placeholder="Nombre y apellido"/>
    </div>
    <div class="banda-item">
      <label>Teléfono cliente</label>
      <input type="text" id="telCliente" placeholder="+54 11 0000-0000"/>
    </div>
    <div class="banda-item">
      <label>Estado de pago</label>
      <div class="estado-pago" style="margin-top:4px;">
        <button class="chip-estado pendiente activo" onclick="setEstado('pendiente',this)">Pendiente</button>
        <button class="chip-estado parcial" onclick="setEstado('parcial',this)">Parcial</button>
        <button class="chip-estado pagado" onclick="setEstado('pagado',this)">Pagado</button>
      </div>
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
              <th style="width:40%">Descripción</th>
              <th style="width:20%">Medidas / cantidad</th>
              <th style="width:20%">Precio unit.</th>
              <th style="width:15%;text-align:right">Subtotal</th>
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
          <span class="val" id="subtotalDisplay">$ 0,00</span>
        </div>
        <div class="total-row">
          <span class="lbl">Descuento</span>
          <span class="val"><input type="text" id="descuento" placeholder="$ 0" oninput="calcularTotales()"/></span>
        </div>
        <div class="separador"></div>
        <div class="total-row grande">
          <span class="lbl">TOTAL</span>
          <span class="val" id="totalDisplay">$ 0,00</span>
        </div>
        <div class="separador"></div>
        <div class="total-row">
          <span class="lbl">Abonado</span>
          <span class="val"><input type="text" id="abonado" placeholder="$ 0" oninput="calcularRestante()"/></span>
        </div>
        <div class="total-row grande">
          <span class="lbl" style="color:#c0392b">Resta abonar</span>
          <span class="val" id="restaDisplay" style="color:#c0392b">$ 0,00</span>
        </div>
      </div>
    </div>

    <!-- PLANOS -->
    <div>
      <div class="seccion-titulo">📐 Planos / croquis (opcional)</div>
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
        📞 <input type="text" placeholder="Teléfono / WhatsApp 1" id="tel1"/>
      </div>
      <div class="linea">
        📞 <input type="text" placeholder="Teléfono / WhatsApp 2" id="tel2"/>
      </div>
      <div class="linea">
        📍 <input type="text" placeholder="Dirección del local" id="direccionLocal"/>
      </div>
      <div class="linea">
        ✉️ <input type="text" placeholder="Email" id="email"/>
      </div>
    </div>
    <div class="footer-firma">
      Mármoles y Granitos Quilmes<br/>
      <span style="opacity:0.4;font-size:10px;">Documento no fiscal – Comprobante de trabajo</span>
    </div>
  </div>

</div><!-- fin #documento -->

<script>
// ===== INICIALIZAR =====
window.onload = () => {
  // Fecha de hoy por defecto
  const hoy = new Date().toISOString().split('T')[0];
  document.getElementById('fecha').value = hoy;
  agregarFila();
  agregarFila();

  // Cargar datos guardados del localStorage (para no perder si recargan)
  cargarStorage();
};

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
  tr.innerHTML = `
    <td><input type="text" placeholder="Descripción del ítem" onchange="calcularTotales()"/></td>
    <td><input type="text" placeholder="Ej: 1.20m x 0.60m"/></td>
    <td class="monto"><input type="text" placeholder="0" oninput="calcularSubtotalFila(this, ${filaId})"/></td>
    <td class="monto"><input type="text" id="sub-${filaId}" readonly placeholder="–" style="font-weight:700;color:var(--verde);"/></td>
    <td><button class="btn-eliminar" onclick="eliminarFila(${filaId})">✕</button></td>
  `;
  tbody.appendChild(tr);
}

function eliminarFila(id) {
  const fila = document.getElementById('fila-' + id);
  if (fila) fila.remove();
  calcularTotales();
}

function calcularSubtotalFila(input, id) {
  const val = parseMonto(input.value);
  const subEl = document.getElementById('sub-' + id);
  if (subEl) subEl.value = val > 0 ? formatMonto(val) : '–';
  calcularTotales();
}

// ===== CÁLCULOS =====
function parseMonto(str) {
  if (!str) return 0;
  return parseFloat(str.replace(/\./g,'').replace(',','.').replace(/[^0-9.]/g,'')) || 0;
}

function formatMonto(n) {
  return '$ ' + n.toLocaleString('es-AR', {minimumFractionDigits:2, maximumFractionDigits:2});
}

function calcularTotales() {
  let sub = 0;
  document.querySelectorAll('#tablaItems tr').forEach(tr => {
    const inputs = tr.querySelectorAll('td.monto input');
    if (inputs[0]) sub += parseMonto(inputs[0].value);
  });
  document.getElementById('subtotalDisplay').textContent = formatMonto(sub);
  const desc = parseMonto(document.getElementById('descuento').value);
  const total = Math.max(0, sub - desc);
  document.getElementById('totalDisplay').textContent = formatMonto(total);
  calcularRestante();
}

function calcularRestante() {
  const totalTxt = document.getElementById('totalDisplay').textContent;
  const total = parseMonto(totalTxt.replace('$',''));
  const abonado = parseMonto(document.getElementById('abonado').value);
  const resta = Math.max(0, total - abonado);
  document.getElementById('restaDisplay').textContent = formatMonto(resta);
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
}

// ===== LIMPIAR =====
function limpiarTodo() {
  if (!confirm('¿Limpiar todos los campos?')) return;
  document.getElementById('tablaItems').innerHTML = '';
  filaId = 0;
  agregarFila();
  agregarFila();
  ['fecha','cliente','telCliente','trabajo','material','bacha','direccion',
   'descuento','abonado','observaciones','tel1','tel2','direccionLocal','email','inputNumero'].forEach(id => {
    const el = document.getElementById(id);
    if (el) el.value = '';
  });
  document.getElementById('numDisplay').textContent = '–';
  document.getElementById('subtotalDisplay').textContent = '$ 0,00';
  document.getElementById('totalDisplay').textContent = '$ 0,00';
  document.getElementById('restaDisplay').textContent = '$ 0,00';
  quitarPlano({stopPropagation:()=>{}});
  const hoy = new Date().toISOString().split('T')[0];
  document.getElementById('fecha').value = hoy;
}

// ===== DESCARGAR PDF =====
async function descargarPDF() {
  const btn = document.querySelector('.btn-primary');
  btn.textContent = '⏳ Generando PDF...';
  btn.disabled = true;

  // Ocultar controles no imprimibles
  document.querySelectorAll('.btn-agregar, .btn-eliminar, .btn-quitar-plano').forEach(el => el.style.visibility = 'hidden');
  document.querySelectorAll('.planos-texto-hint').forEach(el => el.style.display = 'none');

  try {
    const doc = document.getElementById('documento');

    // Forzar ancho A4 exacto (794px a 96dpi) durante la captura
    const A4_PX = 794;
    const prevStyle = doc.getAttribute('style') || '';
    doc.style.cssText += ';width:' + A4_PX + 'px!important;max-width:' + A4_PX + 'px!important;border-radius:0!important;';

    // Pausa para que el navegador re-renderice con el nuevo ancho
    await new Promise(r => setTimeout(r, 100));

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

    // Restaurar estilos originales
    doc.setAttribute('style', prevStyle);

    const { jsPDF } = window.jspdf;

    // Ancho fijo A4 (210mm), alto proporcional al contenido — una sola hoja
    const pdfW = 210;
    const pdfH = (canvas.height / canvas.width) * pdfW;

    const pdf = new jsPDF({
      orientation: 'portrait',
      unit: 'mm',
      format: [pdfW, pdfH]
    });

    const imgData = canvas.toDataURL('image/jpeg', 0.95);
    pdf.addImage(imgData, 'JPEG', 0, 0, pdfW, pdfH);

    const num = document.getElementById('inputNumero').value || '001';
    const cliente = document.getElementById('cliente').value || 'cliente';
    pdf.save(`Recibo-${num}-${cliente}.pdf`);

  } catch (err) {
    alert('Error al generar el PDF: ' + err.message);
  } finally {
    document.querySelectorAll('.btn-agregar, .btn-eliminar, .btn-quitar-plano').forEach(el => el.style.visibility = '');
    document.querySelectorAll('.planos-texto-hint').forEach(el => el.style.display = '');
    btn.textContent = '⬇ Descargar PDF';
    btn.disabled = false;
  }
}

// ===== STORAGE (persistencia básica) =====
function cargarStorage() {
  const campos = ['tel1','tel2','direccionLocal','email'];
  campos.forEach(id => {
    const val = localStorage.getItem('mgq_' + id);
    if (val) document.getElementById(id).value = val;
  });
}

['tel1','tel2','direccionLocal','email'].forEach(id => {
  document.getElementById(id)?.addEventListener('input', function() {
    localStorage.setItem('mgq_' + id, this.value);
  });
});
</script>
</body>
</html>
