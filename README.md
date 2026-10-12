<!DOCTYPE html>
<html lang="es">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no" />
  <title>ICEMAN Móvil - Versión lista</title>
  <meta name="theme-color" content="#0284c7" />
  <style>
    :root{
      --sky-700:#0369a1; --sky-600:#0284c7; --sky-100:#e0f2fe;
      --slate-50:#f8fafc; --slate-100:#f1f5f9; --slate-200:#e2e8f0;
      --slate-400:#94a3b8; --slate-500:#64748b; --slate-600:#475569;
      --slate-800:#1e293b; --white:#fff; --red-500:#ef4444;
      --red-100:#fee2e2; --green-600:#16a34a; --green-100:#dcfce7;
      --amber-100:#fef3c7; --shadow:0 12px 28px rgba(15,23,42,.08);
      --shadow-soft:0 6px 18px rgba(15,23,42,.04);
    }

    *{box-sizing:border-box}
    html,body{margin:0;min-height:100%;background:var(--slate-50);color:var(--slate-800);font-family:system-ui,-apple-system,BlinkMacSystemFont,"Segoe UI",sans-serif;-webkit-tap-highlight-color:transparent;overscroll-behavior-y:contain;}
    body{display:flex;flex-direction:column;min-height:100vh;padding-bottom:92px}
    button,input,select{font:inherit}
    button{border:none;cursor:pointer;background:none}
    input,select{width:100%;border-radius:12px;border:1px solid var(--slate-200);background:var(--white);padding:12px;font-size:14px;color:var(--slate-800);outline:none}
    input:focus,select:focus{border-color:var(--sky-600);box-shadow:0 0 0 3px rgba(2,132,199,.12)}
    .hidden{display:none!important}
    .topbar{position:sticky;top:0;z-index:50;display:flex;align-items:center;justify-content:space-between;padding:12px 18px;background:var(--sky-700);color:var(--white);box-shadow:var(--shadow)}
    .brand{display:flex;align-items:center;gap:8px}.brand-emoji{font-size:24px}.brand h1{margin:0;font-size:1.08rem;font-weight:900;letter-spacing:-.06em;text-transform:uppercase}
    .badge{display:inline-flex;align-items:center;justify-content:center;padding:3px 7px;border-radius:999px;background:rgba(255,255,255,.14);font-size:9px;font-weight:800}
    .offline-indicator{font-size:10px;font-weight:900;background:#ef4444;color:white;border-radius:999px;padding:4px 8px;animation:pulse 1s infinite}
    @keyframes pulse{0%{opacity:1}50%{opacity:.55}100%{opacity:1}}
    .app{width:min(100%,540px);margin:0 auto;padding:18px 16px 0;flex:1}
    .section{display:none}.section.active{display:block}
    .section-head{display:flex;align-items:center;justify-content:space-between;gap:10px;margin-bottom:16px}
    .section-head h2{margin:0;font-size:1.2rem;font-weight:900;color:var(--slate-800)}
    .primary-btn,.success-btn,.neutral-btn,.danger-btn,.soft-btn,.dark-btn,.ghost-btn{border-radius:12px;padding:10px 14px;font-size:12px;font-weight:800;transition:transform .12s ease}
    .primary-btn:active,.success-btn:active,.neutral-btn:active,.danger-btn:active,.soft-btn:active,.dark-btn:active,.ghost-btn:active,.tab-btn:active{transform:scale(.98)}
    .primary-btn{background:var(--sky-600);color:white;box-shadow:0 6px 18px rgba(2,132,199,.18)}
    .success-btn{background:var(--green-600);color:white}.neutral-btn{background:var(--slate-100);color:var(--slate-600)}
    .danger-btn{background:var(--red-100);color:#b91c1c;border:1px solid rgba(239,68,68,.12)}.soft-btn{background:var(--amber-100);color:#92400e}
    .dark-btn{background:#0f172a;color:white}.ghost-btn{background:var(--slate-100);color:var(--slate-600);border-radius:10px;padding:8px 10px;font-size:11px;font-weight:800}
    .full{width:100%}
    .dashboard{display:grid;grid-template-columns:repeat(2,minmax(0,1fr));gap:10px;margin-bottom:16px}
    .stat-card{background:var(--white);border:1px solid var(--slate-200);border-radius:16px;padding:12px 10px;box-shadow:var(--shadow-soft)}
    .stat-card .label{display:block;margin-bottom:6px;font-size:10px;letter-spacing:.08em;text-transform:uppercase;color:var(--slate-400)}
    .stat-card strong{font-size:1.08rem;font-weight:900}
    .toolbar{display:flex;gap:8px;align-items:center;margin-bottom:12px}.toolbar input{flex:1}.toolbar select{width:120px;min-width:120px}
    .filter-bar{display:flex;gap:8px;overflow-x:auto;margin-bottom:14px;padding-bottom:4px}
    .status-chip{flex-shrink:0;background:var(--white);border:1px solid var(--slate-200);border-radius:999px;padding:8px 12px;font-size:11px;font-weight:800;color:var(--slate-600)}
    .status-chip.active{background:var(--sky-100);color:var(--sky-700);border-color:rgba(2,132,199,.2)}
    .card-stack{display:flex;flex-direction:column;gap:12px}.card{background:var(--white);border:1px solid var(--slate-200);border-radius:16px;padding:14px;box-shadow:var(--shadow-soft)}
    .card-head{display:flex;justify-content:space-between;align-items:flex-start;gap:8px;margin-bottom:10px}.card-head h3{margin:0;font-size:1rem;font-weight:900}.card-head p{margin:4px 0 0;font-size:12px;color:var(--slate-500)}
    .status-badge{display:inline-block;padding:6px 10px;border-radius:999px;font-size:10px;font-weight:800;text-transform:uppercase}
    .status-pending{background:var(--amber-100);color:#92400e}.status-moving{background:var(--sky-100);color:var(--sky-700)}.status-done{background:var(--green-100);color:#166534}
    .stats-grid{display:grid;grid-template-columns:repeat(3,minmax(0,1fr));gap:8px;margin-top:10px}
    .stat-box{background:var(--slate-50);border-radius:10px;padding:8px 6px;text-align:center}
    .stat-box span{display:block;font-size:9px;color:var(--slate-400);margin-bottom:4px}.stat-box strong{font-size:12px}
    .meta-note{margin-top:10px;font-size:12px;color:var(--slate-500)}.tiny-actions{display:flex;gap:6px;flex-wrap:wrap;margin-top:10px}
    .panel-card{background:var(--white);border:1px solid var(--slate-200);border-radius:18px;padding:16px;box-shadow:var(--shadow-soft)}
    .field-label{display:block;margin:12px 0 8px;font-size:10px;font-weight:800;letter-spacing:.08em;color:var(--slate-400);text-transform:uppercase}
    .field-label:first-child{margin-top:0}.separator{height:1px;background:var(--slate-200);margin:10px 0}.inline-actions{display:flex;gap:8px;align-items:center}.inline-actions input{flex:1}
    .split-actions{display:grid;grid-template-columns:repeat(2,minmax(0,1fr));gap:8px}
    .ticket{margin-top:16px;background:var(--white);border:1px solid var(--slate-200);border-radius:18px;padding:16px;font-family:monospace;font-size:12px}
    .ticket-header{text-align:center;border-bottom:1px dashed var(--slate-200);padding-bottom:10px;margin-bottom:12px}.ticket-header h3{margin:0 0 4px;font-size:14px;font-weight:900}.ticket-header p,.ticket-header span{margin:0;font-size:10px;color:var(--slate-500)}
    .ticket-meta p { margin: 6px 0; }
    .ticket-lines{border-top:1px dashed var(--slate-200);border-bottom:1px dashed var(--slate-200);padding:10px 0;display:flex;flex-direction:column;gap:6px}
    .ticket-line{display:flex;justify-content:space-between;gap:8px}.ticket-total{text-align:right;font-size:15px;font-weight:900;margin:12px 0 16px}
    .log-box{background:var(--white);border:1px solid var(--slate-200);border-radius:18px;padding:14px;box-shadow:var(--shadow-soft)}
    .log-box h3{margin:0 0 10px;font-size:.95rem;font-weight:900}.report-list{display:flex;flex-direction:column;gap:8px}.report-item{display:flex;justify-content:space-between;align-items:center;font-size:12px;padding-bottom:6px;border-bottom:1px dashed var(--slate-200)}
    .report-item:last-child{border-bottom:none;padding-bottom:0}
    .bottom-nav{position:fixed;left:0;right:0;bottom:0;background:var(--white);border-top:1px solid var(--slate-200);box-shadow:0 -8px 18px rgba(15,23,42,.08);z-index:40}
    .bottom-nav{max-width:540px;margin:0 auto;display:grid;grid-template-columns:repeat(5,minmax(0,1fr));gap:6px;padding:8px 10px 12px}
    .tab-btn{border-radius:12px;padding:10px 4px;font-size:18px;font-weight:800;color:var(--slate-600)}
    .tab-btn.active{background:var(--sky-100);color:var(--sky-700)}
    .modal{position:fixed;inset:0;display:flex;align-items:flex-end;justify-content:center;background:rgba(15,23,42,.62);padding:18px;z-index:100}
    @media(min-width:640px){.modal{align-items:center}}.modal-card{width:min(100%,430px);background:var(--white);border-radius:22px;padding:18px;box-shadow:0 24px 60px rgba(15,23,42,.24)}
    .modal-card h3{margin:0 0 12px;font-size:1.1rem;font-weight:900}.two-col{display:grid;grid-template-columns:repeat(2,minmax(0,1fr));gap:10px}.modal-actions{display:flex;gap:8px;margin-top:12px}.modal-actions>*{flex:1}
    .empty-state{text-align:center;padding:18px 12px;border:1px dashed var(--slate-200);border-radius:12px;color:var(--slate-400);background:rgba(255,255,255,.5)}
    .delete-btn{
      border:1px solid var(--slate-200); background:var(--slate-50); color:var(--slate-600);
      border-radius:10px; width:30px; height:30px; font-size:16px; line-height:1; display:grid; place-items:center;
      cursor:pointer;
    }
    .small-note{color:var(--slate-400);font-size:12px}
    @media(max-width:380px){.dashboard,.two-col,.split-actions{grid-template-columns:1fr}.toolbar{flex-direction:column}.toolbar select{width:100%}.section-head{flex-direction:column;align-items:flex-start}}
  </style>
</head>
<body>

  <header class="topbar">
    <div class="brand">
      <span class="brand-emoji">🧊</span>
      <h1 id="app-title">ICEMAN</h1>
      <span class="badge">MÓVIL</span>
    </div>
    <div id="offline-indicator" class="offline-indicator hidden">OFFLINE</div>
  </header>

  <main class="app">
    <section id="tab-inicio" class="section active">
      <div class="section-head"><h2>📊 Dashboard</h2></div>
      <div id="dashboard" class="dashboard"></div>

      <div class="log-box">
        <h3>📈 Rendimiento por repartidor</h3>
        <div id="repartidor-report" class="report-list"></div>
      </div>
    </section>

    <section id="tab-pedidos" class="section">
      <div class="section-head">
        <h2>📦 Repartos</h2>
        <button class="primary-btn" onclick="abrirModalPedido()">+ Nuevo</button>
      </div>

      <div class="toolbar">
        <input id="search-pedidos" type="search" placeholder="Buscar cliente, nota o repartidor" />
        <select id="filter-fecha">
          <option value="todos">Todos</option>
          <option value="hoy">Hoy</option>
          <option value="7dias">7 días</option>
          <option value="mes">Mes</option>
        </select>
      </div>

      <div id="status-filter" class="filter-bar">
        <button class="status-chip active" data-status="todos">Todos</button>
        <button class="status-chip" data-status="pendiente">Pendiente</button>
        <button class="status-chip" data-status="en_ruta">En ruta</button>
        <button class="status-chip" data-status="entregado">Entregado</button>
      </div>

      <div id="lista-pedidos" class="card-stack"></div>
    </section>

    <section id="tab-clientes" class="section">
      <div class="section-head">
        <h2>👥 Clientes</h2>
        <button class="primary-btn" onclick="abrirModalCliente()">+ Añadir</button>
      </div>

      <div class="toolbar">
        <input id="search-clientes" type="search" placeholder="Buscar cliente" />
      </div>

      <div id="lista-clientes" class="card-stack"></div>
    </section>

    <section id="tab-repartidores" class="section">
      <div class="section-head"><h2>🚚 Repartidores</h2></div>
      <div class="toolbar"><input id="search-repartidores" type="search" placeholder="Buscar repartidor" /></div>
      <div id="lista-repartidores" class="card-stack"></div>
    </section>

    <section id="tab-facturacion" class="section">
      <div class="section-head"><h2>🧾 Liquidación rápida</h2></div>

      <div class="panel-card">
        <label class="field-label">Seleccionar cliente</label>
        <select id="fact-cliente-sel"></select>
        <button class="success-btn full" style="margin-top:12px;" onclick="generarBorradorFactura()">Generar Borrador</button>
      </div>

      <div id="vista-factura" class="ticket hidden">
        <div class="ticket-header">
          <h3 id="fact-empresa-header">HIELOS ICEMAN</h3>
          <p>Comprobante de Entrega</p>
          <span id="fact-fecha"></span>
        </div>

        <div class="ticket-meta">
          <p><strong>CLI:</strong> <span id="fact-nom-cliente"></span></p>
          <p><strong>DIR:</strong> <span id="fact-dir-cliente"></span></p>
        </div>

        <div id="fact-lineas" class="ticket-lines"></div>

        <div class="ticket-total">Total: <span id="fact-total-final">0.00</span>€</div>

        <button class="dark-btn full" onclick="window.print()">🖨️ Imprimir Ticket</button>
      </div>
    </section>

    <section id="tab-configuracion" class="section">
      <div class="section-head"><h2>⚙️ Opciones</h2></div>

      <div class="panel-card">
        <label class="field-label">Nombre empresa</label>
        <div class="inline-actions">
          <input id="config-nombre-empresa" type="text" />
          <button class="primary-btn" onclick="cambiarNombreEmpresa()">OK</button>
        </div>

        <div class="separator"></div>

        <label class="field-label">PIN de acceso</label>
        <div class="inline-actions">
          <input id="config-pin" type="password" placeholder="Define un PIN" />
          <button class="soft-btn" onclick="guardarPin()">Guardar</button>
        </div>

        <div class="separator"></div>

        <label class="field-label">Respaldo local</label>
        <div class="split-actions">
          <button class="neutral-btn" onclick="exportarDatos()">Exportar JSON</button>
          <button class="danger-btn" onclick="limpiarTodo()">Reiniciar Demo</button>
        </div>

        <div class="split-actions" style="margin-top:10px;">
          <button class="neutral-btn" onclick="document.getElementById('import-file').click()">Importar JSON</button>
          <input id="import-file" type="file" accept="application/json" class="hidden">
        </div>
      </div>
    </section>
  </main>

  <nav class="bottom-nav" aria-label="Navegación principal">
    <button class="tab-btn active" data-tab="tab-inicio" aria-label="Inicio">🏠</button>
    <button class="tab-btn" data-tab="tab-pedidos" aria-label="Pedidos">📦</button>
    <button class="tab-btn" data-tab="tab-clientes" aria-label="Clientes">👥</button>
    <button class="tab-btn" data-tab="tab-repartidores" aria-label="Repartidores">🚚</button>
    <button class="tab-btn" data-tab="tab-configuracion" aria-label="Configuración">⚙️</button>
  </nav>

  <div id="modal-cliente" class="modal hidden" role="dialog" aria-modal="true" aria-labelledby="modal-cliente-title" aria-hidden="true" onclick="if(event.target === this) cerrarModalCliente()">
    <form class="modal-card" onsubmit="saveCliente(event)">
      <h3 id="modal-cliente-title">Añadir Cliente</h3>
      <label class="field-label">Nombre Comercial</label><input id="cliente-nombre" type="text" required />
      <label class="field-label">Dirección</label><input id="cliente-direccion" type="text" required />
      <label class="field-label">Teléfono</label><input id="cliente-telefono" type="tel" required />
      <div class="modal-actions"><button type="button" class="neutral-btn" onclick="cerrarModalCliente()">Cancelar</button><button type="submit" class="primary-btn">Guardar</button></div>
    </form>
  </div>

  <div id="modal-pedido" class="modal hidden" role="dialog" aria-modal="true" aria-labelledby="modal-pedido-title" aria-hidden="true" onclick="if(event.target === this) cerrarModalPedido()">
    <form class="modal-card" onsubmit="savePedido(event)">
      <h3 id="modal-pedido-title">Crear Pedido</h3>
      <label class="field-label">Cliente</label><select id="ped-cliente" required></select>
      <div class="two-col"><div><label class="field-label">Bolsas</label><input id="ped-cant" type="number" min="1" value="5" required /></div><div><label class="field-label">Precio (€)</label><input id="ped-precio" type="number" min="0" step="0.01" value="2.50" required /></div></div>
      <div class="two-col"><div><label class="field-label">Estado</label><select id="ped-estado"><option value="pendiente">Pendiente</option><option value="en_ruta">En ruta</option><option value="entregado">Entregado</option></select></div><div><label class="field-label">Repartidor</label><select id="ped-repartidor" required></select></div></div>
      <label class="field-label">Nota</label><input id="ped-nota" type="text" placeholder="Ej. Calle 3, puerta verde" />
      <div class="modal-actions"><button type="button" class="neutral-btn" onclick="cerrarModalPedido()">Cancelar</button><button type="submit" class="primary-btn">Guardar</button></div>
    </form>
  </div>

  <script>
    const STORAGE_KEY = "iceman-mobile-v6";
    const PIN_KEY = "iceman-mobile-pin-v6";

    function isoNow(){ return new Date().toISOString(); }
    function isoYesterday(offset=-1){ const d=new Date(); d.setDate(d.getDate()+offset); return d.toISOString(); }

    const defaultData = {
      empresa: "HIELOS ICEMAN",
      clientes: [
        { id:1,nombre:"Bar La Plaza",direccion:"Calle Mayor 12",telefono:"600111222" },
        { id:2,nombre:"Restaurante Sol",direccion:"Av. del Río 45",telefono:"600333444" },
        { id:3,nombre:"Hotel Miramar",direccion:"Paseo del Puerto 8",telefono:"600777888" }
      ],
      repartidores: [
        { id:1,nombre:"Luis",telefono:"611222333" },
        { id:2,nombre:"Marta",telefono:"622333444" },
        { id:3,nombre:"Paco",telefono:"633444555" }
      ],
      pedidos: [
        { id:1,clienteId:1,clienteNombre:"Bar La Plaza",cantidad:8,precio:2.5,repartidorId:1,repartidorNombre:"Luis",nota:"Puerta lateral",fecha:isoYesterday(-1),estado:"entregado" },
        { id:2,clienteId:2,clienteNombre:"Restaurante Sol",cantidad:5,precio:3,repartidorId:2,repartidorNombre:"Marta",nota:"Entrega antes de las 14:00",fecha:isoNow(),estado:"en_ruta" },
        { id:3,clienteId:3,clienteNombre:"Hotel Miramar",cantidad:12,precio:2.8,repartidorId:3,repartidorNombre:"Paco",nota:"Habitación 214",fecha:isoNow(),estado:"pendiente" }
      ]
    };

    let state = loadState();

    function clone(d){ return JSON.parse(JSON.stringify(d)); }
    function normalize(raw){
      return {
        empresa: raw && raw.empresa ? String(raw.empresa).trim() : defaultData.empresa,
        clientes: Array.isArray(raw?.clientes) ? raw.clientes.filter(Boolean) : clone(defaultData.clientes),
        repartidores: Array.isArray(raw?.repartidores) ? raw.repartidores.filter(Boolean) : clone(defaultData.repartidores),
        pedidos: Array.isArray(raw?.pedidos) ? raw.pedidos.filter(Boolean) : clone(defaultData.pedidos)
      };
    }

    function loadState(){
      const saved = localStorage.getItem(STORAGE_KEY);
      if(!saved){ const initial=clone(defaultData); localStorage.setItem(STORAGE_KEY, JSON.stringify(initial)); return initial }
      try{ return normalize(JSON.parse(saved)) } catch { const fallback=clone(defaultData); localStorage.setItem(STORAGE_KEY, JSON.stringify(fallback)); return fallback }
    }
    function saveState(){ localStorage.setItem(STORAGE_KEY, JSON.stringify(state)) }
    function getPin(){ return localStorage.getItem(PIN_KEY) || "" }

    async function hashPin(pin){
      const enc = new TextEncoder().encode(String(pin));
      const digest = await crypto.subtle.digest("SHA-256", enc);
      return Array.from(new Uint8Array(digest)).map(b => b.toString(16).padStart(2,"0")).join("");
    }

    async function guardarPin(){
      const pinInput = document.getElementById("config-pin");
      const pin = pinInput.value.trim();
      if(!pin){ alert("Introduce un PIN para activar el bloqueo."); return; }
      const hashed = await hashPin(pin);
      localStorage.setItem(PIN_KEY, hashed);
      pinInput.value = "";
      alert("PIN guardado correctamente.");
    }

    async function pedirPinSiExiste(){
      const storedHash = localStorage.getItem(PIN_KEY);
      if(!storedHash) return true;

      let attempts = 0;
      while(attempts < 3){
        const typed = prompt("Introduce tu PIN para entrar");
        if(typed === null) return false;
        const hashTyped = await hashPin(String(typed).trim());
        if(hashTyped === storedHash) return true;

        attempts++;
        alert(`PIN incorrecto. Intentos ${attempts}/3`);
      }

      alert("Demasiados intentos. Acceso denegado.");
      return false;
    }

    function escapeHtml(s){ return String(s ?? "").replace(/&/g,"&amp;").replace(/</g,"&lt;").replace(/>/g,"&gt;").replace(/"/g,"&quot;").replace(/'/g,"&#039;") }
    function formatMoney(v){ const n=Number(v||0); return Number.isFinite(n)?n.toFixed(2):"0.00" }
    function labelEstado(st){ return {pendiente:"Pendiente",en_ruta:"En ruta",entregado:"Entregado"}[st]||"Pendiente" }
    function getDateFromISO(v){ const d=new Date(v); return Number.isNaN(d.getTime())?null:d }
    function isToday(v){ const d=getDateFromISO(v); if(!d) return false; const now=new Date(); return d.toDateString()===now.toDateString() }
    function isWithinDays(v,days){ const d=getDateFromISO(v); if(!d) return false; const now=new Date(); const limit=new Date(); limit.setDate(limit.getDate()-days); return d>=limit && d<=now }
    function isWithinMonth(v){ const d=getDateFromISO(v); if(!d) return false; const now=new Date(); return d.getMonth()===now.getMonth() && d.getFullYear()===now.getFullYear() }

    function isValidNumber(n){ return typeof n === "number" && !Number.isNaN(n) && Number.isFinite(n); }

    function getFilteredPedidos(){
      const fechaMode=document.getElementById("filter-fecha")?.value||"todos";
      const status=document.querySelector(".status-chip.active")?.dataset.status||"todos";
      const search=(document.getElementById("search-pedidos")?.value||"").trim().toLowerCase();
      let items=[...state.pedidos];
      if(fechaMode==="hoy") items=items.filter(p=>isToday(p.fecha));
      if(fechaMode==="7dias") items=items.filter(p=>isWithinDays(p.fecha,7));
      if(fechaMode==="mes") items=items.filter(p=>isWithinMonth(p.fecha));
      if(status!=="todos") items=items.filter(p=>p.estado===status);
      if(search){ items=items.filter(p=>(`${p.clienteNombre} ${p.repartidorNombre} ${p.nota}`).toLowerCase().includes(search)) }
      return items.sort((a,b)=>new Date(b.fecha)-new Date(a.fecha));
    }

    function renderDashboard(){
      const totalVentas=state.pedidos.reduce((s,p)=>s+(Number(p.cantidad||0)*Number(p.precio||0)),0);
      const totalClientes=state.clientes.length; const totalRepartidores=state.repartidores.length;
      const pedidosHoy=state.pedidos.filter(p=>isToday(p.fecha)).length;
      const entregados=state.pedidos.filter(p=>p.estado==="entregado").length;
      const mesTotal=state.pedidos.filter(p=>isWithinMonth(p.fecha)).reduce((s,p)=>s+(Number(p.cantidad||0)*Number(p.precio||0)),0);
      const stats=[
        {label:"Pedidos",value:state.pedidos.length},
        {label:"Hoy",value:pedidosHoy},
        {label:"Clientes",value:totalClientes},
        {label:"Repartidores",value:totalRepartidores},
        {label:"Entregados",value:entregados},
        {label:"Venta total",value:`${formatMoney(totalVentas)}€`},
        {label:"Mes",value:`${formatMoney(mesTotal)}€`}
      ];
      document.getElementById("dashboard").innerHTML = stats.map(it=>`<div class="stat-card"><span class="label">${it.label}</span><strong>${it.value}</strong></div>`).join("");
    }

    function renderRepartidorReport(){
      const map=new Map();
      state.pedidos.forEach(p=>{ const key=p.repartidorNombre||"Sin nombre"; const total=Number(p.cantidad||0)*Number(p.precio||0); map.set(key,(map.get(key)||0)+total) });
      const list=[...map.entries()].sort((a,b)=>b[1]-a[1]); const box=document.getElementById("repartidor-report");
      if(!list.length){ box.innerHTML=`<div class="small-note">Sin datos por repartidor.</div>`; return }
      box.innerHTML = list.map(([n,t])=>`<div class="report-item"><span>${escapeHtml(n)}</span><strong>${formatMoney(t)}€</strong></div>`).join("");
    }

    function renderPedidos(){
      const items=getFilteredPedidos(); const list=document.getElementById("lista-pedidos");
      if(!items.length){ list.innerHTML=`<div class="empty-state">No hay pedidos con ese criterio.</div>`; return }
      list.innerHTML = items.map(p=>{
        const total=Number(p.cantidad||0)*Number(p.precio||0);
        const badgeClass = {pendiente:"status-pending",en_ruta:"status-moving",entregado:"status-done"}[p.estado]||"status-pending";
        return `
          <article class="card">
            <div class="card-head">
              <div><h3>${escapeHtml(p.clienteNombre)}</h3><p>${escapeHtml(p.nota||"Sin nota")}</p></div>
              <button class="delete-btn" onclick="eliminarPedido(${p.id})" aria-label="Eliminar pedido">✕</button>
            </div>
            <div class="status-badge ${badgeClass}">${labelEstado(p.estado)}</div>
            <div class="stats-grid">
              <div class="stat-box"><span>Bolsas</span><strong>${Number(p.cantidad||0)}</strong></div>
              <div class="stat-box"><span>Precio</span><strong>${formatMoney(p.precio)}€</strong></div>
              <div class="stat-box"><span>Total</span><strong>${formatMoney(total)}€</strong></div>
            </div>
            <div class="meta-note">🚚 ${escapeHtml(p.repartidorNombre)} · ${new Date(p.fecha).toLocaleDateString("es-ES")}</div>
            <div class="tiny-actions">
              <button class="ghost-btn" onclick="editarPedido(${p.id})">Editar</button>
              <button class="ghost-btn" onclick="cambiarEstadoPedido(${p.id},'pendiente')">Pendiente</button>
              <button class="ghost-btn" onclick="cambiarEstadoPedido(${p.id},'en_ruta')">En ruta</button>
              <button class="ghost-btn" onclick="cambiarEstadoPedido(${p.id},'entregado')">Entregado</button>
            </div>
          </article>`;
      }).join("");
    }

    function renderClientes(){
      const input=(document.getElementById("search-clientes")?.value||"").trim().toLowerCase(); const box=document.getElementById("lista-clientes");
      const items=state.clientes.filter(c=>{ if(!input) return true; return `${c.nombre} ${c.direccion} ${c.telefono}`.toLowerCase().includes(input) });
      if(!items.length){ box.innerHTML=`<div class="empty-state">No hay clientes con ese criterio.</div>`; return }
      box.innerHTML = items.map(c=>`<article class="card"><div class="card-head"><div><h3>${escapeHtml(c.nombre)}</h3><p>${escapeHtml(c.direccion)}</p></div><button class="delete-btn" onclick="eliminarCliente(${c.id})" aria-label="Eliminar cliente">✕</button></div><div class="meta-note">📞 ${escapeHtml(c.telefono)}</div><div class="tiny-actions"><button class="ghost-btn" onclick="editarCliente(${c.id})">Editar</button></div></article>`).join("");
    }

    function renderRepartidores(){
      const input=(document.getElementById("search-repartidores")?.value||"").trim().toLowerCase(); const box=document.getElementById("lista-repartidores");
      const items=state.repartidores.filter(r=>{ if(!input) return true; return `${r.nombre} ${r.telefono}`.toLowerCase().includes(input) });
      if(!items.length){ box.innerHTML=`<div class="empty-state">No hay repartidores con ese criterio.</div>`; return }
      box.innerHTML = items.map(r=>`<article class="card"><div class="card-head"><div><h3>${escapeHtml(r.nombre)}</h3><p>📞 ${escapeHtml(r.telefono)}</p></div><button class="delete-btn" onclick="eliminarRepartidor(${r.id})" aria-label="Eliminar repartidor">✕</button></div><div class="tiny-actions"><button class="ghost-btn" onclick="editarRepartidor(${r.id})">Editar</button></div></article>`).join("");
    }

    function renderSelects(){
      const pedCliente=document.getElementById("ped-cliente"), factCliente=document.getElementById("fact-cliente-sel"), pedRepartidor=document.getElementById("ped-repartidor");
      if(pedCliente){ pedCliente.innerHTML = state.clientes.length ? state.clientes.map(c=>`<option value="${c.id}">${escapeHtml(c.nombre)}</option>`).join("") : "<option value=''>No hay clientes</option>"; pedCliente.disabled = !state.clientes.length }
      if(factCliente){
        const previous = factCliente.value;
        factCliente.innerHTML = state.clientes.length ? state.clientes.map(c=>`<option value="${c.id}">${escapeHtml(c.nombre)}</option>`).join("") : "<option value=''>No hay clientes</option>";
        factCliente.disabled = !state.clientes.length;
        if(!previous || !Array.from(factCliente.options).some(o => o.value === previous)){
          if(state.clientes.length) factCliente.value = String(state.clientes[0].id);
        } else {
          factCliente.value = previous;
        }
      }
      if(pedRepartidor){ pedRepartidor.innerHTML = state.repartidores.length ? state.repartidores.map(r=>`<option value="${r.id}">${escapeHtml(r.nombre)}</option>`).join("") : "<option value=''>No hay repartidores</option>"; pedRepartidor.disabled = !state.repartidores.length }
    }

    function renderAll(){ renderDashboard(); renderRepartidorReport(); renderPedidos(); renderClientes(); renderRepartidores(); renderSelects(); actualizarNombreEmpresaVisual(); const configPin = document.getElementById("config-pin"); if(configPin) configPin.value = ""; }

    function actualizarNombreEmpresaVisual(){ const empresa = state.empresa||"HIELOS ICEMAN"; document.getElementById("app-title").textContent = empresa.toUpperCase(); document.getElementById("config-nombre-empresa").value = empresa; document.getElementById("fact-empresa-header").textContent = empresa.toUpperCase() }

    function openModal(modal){
      if(!modal) return;
      modal.classList.remove("hidden");
      modal.setAttribute("aria-hidden","false");

      const focusable = modal.querySelector('input,select,button,textarea,[tabindex]:not([tabindex="-1"])');
      if(focusable) setTimeout(()=>focusable.focus(), 80);

      function onKey(e){
        if(e.key === "Escape"){
          closeModal(modal);
        }
        if(e.key === "Tab"){
          const focusables = Array.from(modal.querySelectorAll('input,select,button,textarea,a[href],[tabindex]:not([tabindex="-1"])')).filter(el => !el.disabled && el.offsetParent !== null);
          if(focusables.length === 0) return;
          const idx = focusables.indexOf(document.activeElement);
          if(e.shiftKey && idx === 0){ e.preventDefault(); focusables[focusables.length - 1].focus(); }
          else if(!e.shiftKey && idx === focusables.length - 1){ e.preventDefault(); focusables[0].focus(); }
        }
      }

      modal.__onKey = onKey;
      document.addEventListener("keydown", onKey);
    }

    function closeModal(modal){
      if(!modal) return;
      modal.classList.add("hidden");
      modal.setAttribute("aria-hidden","true");
      if(modal.__onKey){ document.removeEventListener("keydown", modal.__onKey); delete modal.__onKey; }
      const form = modal.querySelector("form");
      if(form){
        if(modal.id === "modal-cliente") form.onsubmit = saveCliente;
        if(modal.id === "modal-pedido") form.onsubmit = savePedido;
      }
    }

    function abrirModalCliente(){ openModal(document.getElementById("modal-cliente")); setTimeout(()=>document.getElementById("cliente-nombre").focus(),80) }
    function cerrarModalCliente(){ closeModal(document.getElementById("modal-cliente")); const form = document.getElementById("modal-cliente").querySelector("form"); if(form) form.reset() }
    function abrirModalPedido(){ renderSelects(); openModal(document.getElementById("modal-pedido")); setTimeout(()=>document.getElementById("ped-cant").focus(),80) }
    function cerrarModalPedido(){
      const modal = document.getElementById("modal-pedido");
      closeModal(modal);
      const form = modal.querySelector("form");
      if(form){
        form.reset();
        form.onsubmit = savePedido;
      }
      const pedCant = document.getElementById("ped-cant");
      const pedPrecio = document.getElementById("ped-precio");
      const pedEstado = document.getElementById("ped-estado");
      if(pedCant) pedCant.value = 5;
      if(pedPrecio) pedPrecio.value = 2.5;
      if(pedEstado) pedEstado.value = "pendiente";
    }

    function saveCliente(e){ e.preventDefault(); const nombre=document.getElementById("cliente-nombre").value.trim(), direccion=document.getElementById("cliente-direccion").value.trim(), telefono=document.getElementById("cliente-telefono").value.trim(); if(!nombre||!direccion||!telefono) return; state.clientes.push({id:Date.now(),nombre,direccion,telefono}); saveState(); renderAll(); cerrarModalCliente() }

    function savePedido(e){
      e.preventDefault();
      const clienteId = Number(document.getElementById("ped-cliente").value);
      const cantidad = Number(document.getElementById("ped-cant").value);
      const precio = Number(document.getElementById("ped-precio").value);
      const estado = document.getElementById("ped-estado").value || "pendiente";
      const repartidorId = Number(document.getElementById("ped-repartidor").value);
      const nota = document.getElementById("ped-nota").value.trim();

      const cliente = state.clientes.find(c => c.id === clienteId);
      const repartidor = state.repartidores.find(r => r.id === repartidorId);

      if (!cliente) { alert("Selecciona un cliente válido."); return; }
      if (!repartidor) { alert("Selecciona un repartidor válido."); return; }
      if (!isValidNumber(cantidad) || cantidad < 1) { alert("Introduce una cantidad válida (>= 1)."); return; }
      if (!isValidNumber(precio) || precio < 0) { alert("Introduce un precio válido (>= 0)."); return; }

      state.pedidos.push({
        id: Date.now(),
        clienteId: cliente.id,
        clienteNombre: cliente.nombre,
        cantidad,
        precio,
        repartidorId: repartidor.id,
        repartidorNombre: repartidor.nombre,
        nota,
        fecha: new Date().toISOString(),
        estado
      });

      saveState();
      renderAll();
      cerrarModalPedido();
    }

    function editarCliente(id){ const c=state.clientes.find(x=>x.id===id); if(!c) return; const nombre=prompt("Nombre Comercial",c.nombre); if(nombre===null) return; const direccion=prompt("Dirección",c.direccion); if(direccion===null) return; const telefono=prompt("Teléfono",c.telefono); if(telefono===null) return; c.nombre=nombre.trim()||c.nombre; c.direccion=direccion.trim()||c.direccion; c.telefono=telefono.trim()||c.telefono; saveState(); renderAll() }
    function editarRepartidor(id){ const r=state.repartidores.find(x=>x.id===id); if(!r) return; const nombre=prompt("Nombre del repartidor",r.nombre); if(nombre===null) return; const telefono=prompt("Teléfono",r.telefono); if(telefono===null) return; r.nombre=nombre.trim()||r.nombre; r.telefono=telefono.trim()||r.telefono; saveState(); renderAll() }

    function editarPedido(id){
      const pedido=state.pedidos.find(p=>p.id===id); if(!pedido) return;
      renderSelects();
      const modal = document.getElementById("modal-pedido");
      const form = modal.querySelector("form");
      const pedCliente=document.getElementById("ped-cliente"), pedRepartidor=document.getElementById("ped-repartidor"), pedCant=document.getElementById("ped-cant"), pedPrecio=document.getElementById("ped-precio"), pedEstado=document.getElementById("ped-estado"), pedNota=document.getElementById("ped-nota");
      pedCliente.value=String(pedido.clienteId); pedRepartidor.value=String(pedido.repartidorId); pedCant.value=pedido.cantidad; pedPrecio.value=pedido.precio; pedEstado.value=pedido.estado||"pendiente"; pedNota.value=pedido.nota||"";
      openModal(modal);
      form.onsubmit=function(ev){ ev.preventDefault(); pedido.clienteId=Number(pedCliente.value); pedido.clienteNombre=state.clientes.find(c=>c.id===pedido.clienteId)?.nombre||pedido.clienteNombre; pedido.cantidad=Number(pedCant.value); pedido.precio=Number(pedPrecio.value); pedido.repartidorId=Number(pedRepartidor.value); pedido.repartidorNombre=state.repartidores.find(r=>r.id===pedido.repartidorId)?.nombre||pedido.repartidorNombre; pedido.estado=pedEstado.value||"pendiente"; pedido.nota=pedNota.value.trim(); saveState(); renderAll(); cerrarModalPedido(); };
    }

    function cambiarEstadoPedido(id,n){ const p=state.pedidos.find(x=>x.id===id); if(!p) return; p.estado=n; saveState(); renderAll() }
    function eliminarCliente(id){ if(!confirm("¿Eliminar cliente y sus pedidos? Esta acción no se puede deshacer.")) return; state.clientes=state.clientes.filter(c=>c.id!==id); state.pedidos=state.pedidos.filter(p=>p.clienteId!==id); saveState(); renderAll() }
    function eliminarRepartidor(id){ if(!confirm("¿Eliminar repartidor y sus pedidos? Esta acción no se puede deshacer.")) return; state.repartidores=state.repartidores.filter(r=>r.id!==id); state.pedidos=state.pedidos.filter(p=>p.repartidorId!==id); saveState(); renderAll() }
    function eliminarPedido(id){ if(!confirm("¿Eliminar este pedido?")) return; state.pedidos=state.pedidos.filter(p=>p.id!==id); saveState(); renderAll() }

    function cambiarNombreEmpresa(){ const v=(document.getElementById("config-nombre-empresa").value||"HIELOS ICEMAN").trim()||"HIELOS ICEMAN"; state.empresa=v; saveState(); actualizarNombreEmpresaVisual() }
    function exportarDatos(){ const blob=new Blob([JSON.stringify(state,null,2)],{type:"application/json"}); const url=URL.createObjectURL(blob); const a=document.createElement("a"); a.href=url; a.download="iceman-backup.json"; document.body.appendChild(a); a.click(); a.remove(); setTimeout(()=>URL.revokeObjectURL(url),1500); }
    function limpiarTodo(){ const ok=window.confirm("¿Seguro que quieres reiniciar la demo? Se perderán todos los datos locales."); if(!ok) return; localStorage.removeItem(STORAGE_KEY); localStorage.removeItem(PIN_KEY); state=clone(defaultData); saveState(); renderAll(); document.getElementById("vista-factura").classList.add("hidden") }
    function importarDatos(e){ const file=e.target.files?.[0]; if(!file) return; const r=new FileReader(); r.onload=()=>{ try{ const parsed=JSON.parse(r.result); state=normalize(parsed); saveState(); renderAll(); alert("Datos importados correctamente.") } catch{ alert("El archivo JSON no es válido.") } finally{ e.target.value="" } }; r.readAsText(file) }

    function generarBorradorFactura(){ const clienteId=Number(document.getElementById("fact-cliente-sel").value); const cliente=state.clientes.find(c=>c.id===clienteId); if(!cliente) return; const pedidosCliente=state.pedidos.filter(p=>p.clienteId===clienteId); if(!pedidosCliente.length){ alert("Este cliente no tiene pedidos en este momento."); return } const total=pedidosCliente.reduce((s,p)=>s+(Number(p.cantidad||0)*Number(p.precio||0)),0); document.getElementById("fact-nom-cliente").textContent=cliente.nombre; document.getElementById("fact-dir-cliente").textContent=cliente.direccion; document.getElementById("fact-fecha").textContent=new Date().toLocaleString("es-ES",{day:"2-digit",month:"2-digit",year:"numeric",hour:"2-digit",minute:"2-digit"}); document.getElementById("fact-lineas").innerHTML = pedidosCliente.map(p=>`<div class="ticket-line"><span>${Number(p.cantidad||0)}x ${escapeHtml(p.repartidorNombre||"Sin repartidor")}</span><span>${formatMoney(Number(p.cantidad||0)*Number(p.precio||0))}€</span></div>`).join(""); document.getElementById("fact-total-final").textContent=formatMoney(total); document.getElementById("vista-factura").classList.remove("hidden") }

    function setupTabs(){ document.querySelectorAll(".tab-btn").forEach(b=>b.addEventListener("click",()=>{ const t=b.dataset.tab; document.querySelectorAll(".section").forEach(s=>s.classList.toggle("active",s.id===t)); document.querySelectorAll(".tab-btn").forEach(x=>x.classList.toggle("active",x===b)) })) }
    function setupStatusChips(){ document.querySelectorAll(".status-chip").forEach(btn=>btn.addEventListener("click",()=>{ document.querySelectorAll(".status-chip").forEach(x=>x.classList.toggle("active",x===btn)); renderPedidos() })) }
    function setupSearch(){ document.getElementById("search-pedidos").addEventListener("input",renderPedidos); document.getElementById("filter-fecha").addEventListener("change",renderPedidos); document.getElementById("search-clientes").addEventListener("input",renderClientes); document.getElementById("search-repartidores").addEventListener("input",renderRepartidores); document.getElementById("import-file").addEventListener("change",importarDatos) }
    function updateOfflineIndicator(){ const el=document.getElementById("offline-indicator"); if(!el) return; if(!navigator.onLine) el.classList.remove("hidden"); else el.classList.add("hidden") }

    function showLockScreen(message){
      let overlay = document.getElementById("iceman-lock-overlay");
      if(!overlay){
        overlay = document.createElement("div");
        overlay.id = "iceman-lock-overlay";
        overlay.style = "position:fixed;inset:0;display:grid;place-items:center;z-index:9999;background:rgba(248,250,252,0.95);color:#475569;font-family:sans-serif;";
        overlay.innerHTML = `<div style="text-align:center;padding:24px;background:white;border-radius:12px;box-shadow:0 12px 40px rgba(0,0,0,.12);max-width:420px;">
          <h2 style="margin:0 0 8px;">Acceso bloqueado</h2>
          <p id="iceman-lock-msg" style="margin:0 0 12px;">${escapeHtml(message||"PIN incorrecto o no configurado.")}</p>
          <div style="display:flex;gap:8px;justify-content:center;">
            <button class="primary-btn" onclick="window.location.reload()">Recargar</button>
          </div>
        </div>`;
        document.body.appendChild(overlay);
      } else {
        document.getElementById("iceman-lock-msg").textContent = message || "PIN incorrecto o no configurado.";
        overlay.style.display = "grid";
      }
    }

    async function init(){
      const allowed = await pedirPinSiExiste();
      if(!allowed){
        showLockScreen("PIN incorrecto o no configurado.");
        return;
      }
      setupTabs(); setupStatusChips(); setupSearch(); renderAll(); updateOfflineIndicator(); window.addEventListener("online", updateOfflineIndicator); window.addEventListener("offline", updateOfflineIndicator);
    }

    init();
  </script>
</body>
</html>
