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

  /* ===== CUERPO ===== */
  .cuerpo {
    padding: 36px 44px;
    display: flex;
    flex-direction: column;
    gap: 28px;
  }

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
  .campo label {
    font-size: 10px;
    font-weight: 700;
    text-transform: uppercase;
    letter-spacing: 0.08em;
    color: var(--gris);
    display: block;
    margin-bottom: 5px;
  }
  .campo input, .campo textarea {
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
  }

  /* ===== TABLA ===== */
  .tabla-wrap { overflow-x: auto; }
  table { width: 100%; border-collapse: collapse; font-size: 13px; }
  thead tr { background: var(--verde); color: #fff; }
  thead th { padding: 12px 14px; text-align: left; font-size: 11px; text-transform: uppercase; }
  tbody td { padding: 10px 14px; border-bottom: 1px solid var(--borde); }

  /* ===== TOTALES (MODIFICADO) ===== */
  .totales-wrap {
    display: flex;
    justify-content: flex-end;
  }
  .totales-box {
    background: var(--verde-bg);
    border-radius: 12px;
    padding: 22px 28px;
    /* Se aumentó el ancho mínimo para que entren números grandes */
    min-width: 380px; 
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
    color: var(--gris);
  }
  .total-row .val input {
    font-family: 'Montserrat', sans-serif;
    font-size: 15px;
    font-weight: 700;
    color: var(--texto);
    background: transparent;
    border: none;
    border-bottom: 1.5px solid var(--borde);
    padding: 2px 4px;
    /* Se aumentó el ancho del input para cifras de millones */
    width: 180px; 
    text-align: right;
    outline: none;
  }
  .total-row.grande .lbl { font-size: 13px; color: var(--verde-oscuro); }
  .total-row.grande .val input {
    font-size: 22px;
    color: var(--verde);
    /* Ancho extra para el total principal */
    width: 200px; 
  }
  .separador { height: 1.5px; background: var(--borde); margin: 4px 0; }

  /* ===== FOOTER ===== */
  .footer {
    background: var(--texto);
    color: rgba(255,255,255,0.8);
    padding: 22px 44px;
    display: flex;
    justify-content: space-between;
    align-items: center;
  }

  .badge-num {
    background: rgba(255,255,255,0.2);
    border-radius: 6px;
    padding: 4px 12px;
    display: inline-block;
    margin-top: 6px;
  }

  @media (max-width: 640px) {
    .totales-box { min-width: 100%; }
    .total-row .val input { width: 140px; }
  }
</style>
</head>
<body>

<div class="toolbar">
  <button class="btn btn-secondary">🗑 Limpiar todo</button>
  <button class="btn btn-wa">WA Cliente</button>
  <button class="btn btn-primary">⬇ Descargar PDF</button>
</div>

<div id="documento">
  <div class="header">
    <div class="logo-wrap">
      <svg viewBox="0 0 812.6 312.6"><path fill="#fff" d="M89.3,226.9c0,3.4,2.1,5.6,5.5,5.6c2.9,0,5.1-2.4,5.1-5.6V123.3c0-14,0-43.6,0-43.6h11.4c0,0,0,0.4,0,1.2l5.3,29.7h0.2l4.8-30.8l0.2,0c0,0,0,0,0,0h11.6c0,0,0,57.7,0,86v61.4c0,2.5,2.1,5.1,4.2,5.2c3.4,0.2,5.9-1.5,6.1-4.5c0.2-3.8,0.2-7.6,0.2-11.4c0-7.9,0-15.7,0-23.6v-92.1c0-8.8,0-17.5,0-26.3c0-3.2-2.1-5.5-5.1-5.5c-14.8,0-29.6,0-44.4,0c-2.6,0-4.6,1.6-4.9,4.1c-0.3,2.4-0.2,4.8-0.2,7.3c0,7.8,0,15.7,0,23.6L89.3,226.9L89.3,226.9z"/></svg>
    </div>
    <div class="header-right">
      <h1>Recibo</h1>
      <div class="num-recibo">N°&nbsp;<span id="numDisplay">120426</span></div>
      <div class="badge-num">
        <input id="inputNumero" type="text" value="120426" style="background:transparent;border:none;color:#fff;font-family:'Montserrat';font-weight:600;width:90px;text-align:center;outline:none;"/>
      </div>
    </div>
  </div>

  <div class="banda">
    <div class="banda-item banda-fecha">
      <label>Fecha</label>
      <input type="text" id="fecha" value="06/05/2026"/>
    </div>
    <div class="banda-item banda-cliente">
      <label>Cliente</label>
      <input type="text" id="cliente" placeholder="Nombre y apellido"/>
    </div>
  </div>

  <div class="cuerpo">
    <div class="seccion">
      <div class="seccion-titulo">Detalle del Trabajo</div>
      <div class="tabla-wrap">
        <table>
          <thead>
            <tr>
              <th>Descripción</th>
              <th style="text-align:right">Importe</th>
            </tr>
          </thead>
          <tbody>
            <tr>
              <td><input type="text" placeholder="Ej: Mesada de Granito Gris Mara"/></td>
              <td class="monto"><input type="text" value="$ 0,00" style="text-align:right"/></td>
            </tr>
          </tbody>
        </table>
      </div>
    </div>

    <div class="totales-wrap">
      <div class="totales-box">
        <div class="total-row">
          <span class="lbl">Subtotal</span>
          <span class="val"><input type="text" value="$ 1.250.000,00"/></span>
        </div>
        <div class="total-row">
          <span class="lbl">Seña</span>
          <span class="val"><input type="text" value="$ 500.000,00"/></span>
        </div>
        <div class="separador"></div>
        <div class="total-row grande">
          <span class="lbl">Saldo Total</span>
          <span class="val"><input type="text" value="$ 750.000,00"/></span>
        </div>
      </div>
    </div>
  </div>

  <div class="footer">
    <div style="font-size:12px">Mármoles y Granitos Quilmes</div>
    <div style="font-size:11px; font-style:italic">Gracias por su confianza</div>
  </div>
</div>

</body>
</html>
