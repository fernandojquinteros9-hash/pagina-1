<!DOCTYPE html>
<html lang="es">
<head>
  <meta charset="utf-8" />
  <meta name="viewport" content="width=device-width,initial-scale=1" />
  <title>martilleros.net</title>

  <!-- Fuentes elegantes y legibles -->
  <link rel="preconnect" href="https://fonts.googleapis.com">
  <link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
  <link href="https://fonts.googleapis.com/css2?family=Inter:wght@400;600;700&family=Playfair+Display:wght@600;700&display=swap" rel="stylesheet">

  <style>
    /* Reset y base */
    * { box-sizing: border-box; }
    html, body { height: 100%; margin: 0; font-family: 'Inter', system-ui, -apple-system, Segoe UI, Roboto, Arial, Helvetica, sans-serif; background: #f6f3ee; color: #111; -webkit-font-smoothing:antialiased; -moz-osx-font-smoothing:grayscale; }
    a { color: inherit; }

    /* Parallax container (fondo base64) */
    .parallax {
      position: relative;
      width: 100%;
      height: 360px;
      overflow: hidden;
      display: flex;
      align-items: center;
      justify-content: center;
      text-align: center;
    }
    .parallax::before {
      content: "";
      position: absolute;
      top: 0; left: 0;
      width: 100%; height: 140%;
      /* Fallback: si por algún motivo no se setea la variable, usa el JPEG de placeholder */
      background-image: var(--parallax-bg, url("data:image/jpeg;base64,REPLACE_BASE64_HERE"));
      background-size: cover;
      background-position: center center;
      /* Parallax real usando variable que setea el JS */
      transform: translate3d(0, var(--parallax-y, 0), 0);
      will-change: transform, background-position;
      filter: brightness(0.7) saturate(1.02);
      z-index: -2;
    }
    /* overlay para legibilidad */
    .parallax::after {
      content: "";
      position: absolute;
      top: 0; left: 0;
      width: 100%; height: 100%;
      background: rgba(0,0,0,0.25);
      z-index: -1;
    }
    .hero {
      position: relative;
      z-index: 2;
      color: #fff;
      padding: 32px 18px;
    }
    /* Título grande, legible y elegante */
    .hero h1 { 
      margin: 0 0 8px 0; 
      font-family: "Playfair Display", Georgia, "Times New Roman", serif;
      font-size: clamp(32px, 6vw, 56px); 
      letter-spacing: .3px; 
      line-height: 1.1;
      font-weight: 700;
      text-shadow: 0 2px 18px rgba(0,0,0,.35);
    }
    .hero p { 
      margin: 0; 
      opacity: .98; 
      font-size: clamp(14px, 2.4vw, 18px);
      font-weight: 600;
      text-shadow: 0 1px 12px rgba(0,0,0,.25);
    }

    /* Main panel */
    #main-container { max-width: 1200px; margin: 0 auto 60px; padding: 12px; }

    /* Nav */
    header { display:flex; align-items:center; justify-content:space-between; gap:12px; margin-top: 12px; }
    .nav-left { display:flex; gap:10px; align-items:center; }
    .nav-left button, .nav-right button { background: rgba(0,0,0,0.06); border: none; padding: 8px 12px; border-radius:8px; cursor:pointer; font-weight:600; }
    .nav-left button:hover, .nav-right button:hover { background: rgba(0,0,0,0.12); }

    /* Filtros */
    #filtros {
      margin: 18px auto 0;
      padding: 14px;
      width: calc(100% - 24px);
      background: #fff;
      border-radius: 10px;
      display:flex;
      flex-wrap:wrap;
      gap:10px;
      align-items:center;
      justify-content:center;
      box-shadow: 0 8px 24px rgba(0,0,0,0.08);
    }
    #filtros input, #filtros select { padding:10px 12px; border-radius:8px; border:1px solid #e0e0e0; min-width:120px; font-size:14px; }
    #filtros .btn { padding:10px 14px; border-radius:8px; border:0; cursor:pointer; font-weight:700; }
    #buscarBtn{ background:#ff9800; color:#111; }
    #limpiarBtn{ background:#e0e0e0; color:#111; }

    /* Map */
    #map { width:100%; height:420px; margin-top: 32px; border-radius:12px; overflow:hidden; box-shadow: 0 8px 30px rgba(0,0,0,0.12); }

    /* Listado */
    #listado { margin-top:18px; display:grid; gap:14px; grid-template-columns: repeat(auto-fit, minmax(300px,1fr)); }
    .prop-card {
      background:#fff; color:#111; border-radius:10px; overflow:hidden; box-shadow:0 4px 14px rgba(0,0,0,0.08);
      display:flex; flex-direction:column;
    }
    .prop-card img { width:100%; height:180px; object-fit:cover; }
    .prop-card .meta { padding:12px; flex:1; }
    .prop-card h4 { margin:0 0 6px 0; font-size:16px; }
    .prop-card p { margin:6px 0; font-size:13px; color:#444; }
    .prop-card .actions { display:flex; gap:8px; padding:10px; justify-content:flex-end; background:#fafafa; }
    .btn-map { background:#1976d2; color:#fff; border:0; padding:8px 12px; border-radius:8px; cursor:pointer; font-weight:700; }
    .btn-view { background:#f1f1f1; color:#111; border:0; padding:8px 12px; border-radius:8px; cursor:pointer; }

    /* Empty message */
    .empty-msg { padding:18px; background:rgba(255,255,255,0.95); border-radius:8px; color:#444; text-align:center; }

    /* Responsive */
    @media (max-width:900px) {
      #filtros { flex-direction:column; align-items:stretch; }
      #filtros input, #filtros select { width:100%; min-width:unset; }
      #map { height:360px; margin-top:20px; }
      #listado { grid-template-columns: 1fr; }
      .hero h1 { font-size: clamp(28px, 8vw, 44px); }
    }
    @media (max-width:420px) {
      #map { height:320px; }
      .prop-card img { height:160px; }
      .prop-card .actions button { padding:12px; font-size:15px; }
    }

    /* small helper */
    .small { font-size:13px; color:#666; }
  </style>
</head>
<body>

  <!-- Parallax header con fondo en base64 -->
  <div class="parallax" id="parallax">
    <div class="hero">
      <h1>martilleros.net — Tandil</h1>
      <p class="small">Buscá propiedades en Tandil. Usá el rectángulo en el mapa para seleccionar zona.</p>
    </div>
  </div>

  <div id="main-container">
    <header>
      <div class="nav-left">
        <button onclick="showSection('propiedades')">Propiedades</button>
        <button onclick="showSection('servicios')">Servicios</button>
      </div>
      <div class="nav-right">
        <button onclick="logout()">Cerrar sesión</button>
      </div>
    </header>

    <!-- Filters -->
    <div id="filtros">
      <input id="precioMin" type="number" placeholder="Precio mínimo (USD)">
      <input id="precioMax" type="number" placeholder="Precio máximo (USD)">
      <select id="tipo">
        <option value="">Tipo</option><option value="Casa">Casa</option><option value="Departamento">Departamento</option><option value="Quinta">Quinta</option><option value="Lote">Lote</option>
      </select>
      <select id="habitaciones">
        <option value="">Habitaciones</option><option>1</option><option>2</option><option>3</option><option>4+</option>
      </select>
      <select id="banos">
        <option value="">Baños</option><option>1</option><option>2</option><option>3+</option>
      </select>
      <select id="cochera">
        <option value="">Cochera</option><option>Sí</option><option>No</option>
      </select>
      <select id="patio">
        <option value="">Patio</option><option>Sí</option><option>No</option>
      </select>
      <select id="pileta">
        <option value="">Pileta</option><option>Sí</option><option>No</option>
      </select>

      <button id="buscarBtn" class="btn" onclick="onBuscarClick()">Buscar</button>
      <button id="limpiarBtn" class="btn" onclick="limpiarFiltros()">Limpiar filtros</button>
    </div>

    <!-- Propiedades: mapa + listado -->
    <section id="propiedades" style="display:block; padding: 0 6px 30px;">
      <div id="map"></div>
      <div id="listado"></div>
    </section>

    <!-- Servicios -->
    <section id="servicios" style="display:none; padding:20px; background:#fff; border-radius:8px; margin-top:18px;">
      <h2 style="margin-top:0;">Servicios recomendados</h2>
      <ul>
        <li>Plomería</li>
        <li>Albañilería</li>
        <li>Electricidad</li>
      </ul>
    </section>
  </div>

  <script>
    /**************************************************************************
     * Datos: 15 propiedades en Tandil (coordenadas aproximadas)
     * Cada propiedad tiene: id, nombre, lat, lng, precio, tipo, hab, banos,
     * cochera, patio, pileta, img (Unsplash or placeholder), desc
     **************************************************************************/
    const propiedades = [
      { id:1, nombre:"Casa El Molino", lat:-37.3265, lng:-59.1390, precio:150000, tipo:"Casa", hab:3, banos:2, cochera:"Sí", patio:"Sí", pileta:"No", img:"https://images.unsplash.com/photo-1560185127-6ed189bf02f4?auto=format&fit=crop&w=1200&q=80", desc:"Casa clásica, patio y garage."},
      { id:2, nombre:"Quinta La Fortaleza", lat:-37.3340, lng:-59.1480, precio:300000, tipo:"Quinta", hab:4, banos:3, cochera:"Sí", patio:"Sí", pileta:"Sí", img:"https://images.unsplash.com/photo-1600585154340-be6161a56a0c?auto=format&fit=crop&w=1200&q=80", desc:"Quinta espaciosa con pileta y galería."},
      { id:3, nombre:"Depto Centro", lat:-37.3270, lng:-59.1335, precio:85000, tipo:"Departamento", hab:2, banos:1, cochera:"No", patio:"No", pileta:"No", img:"https://images.unsplash.com/photo-1600585154206-79a5d9b5a3f7?auto=format&fit=crop&w=1200&q=80", desc:"Departamento céntrico, luminoso."},
      { id:4, nombre:"Casa Vista Azul", lat:-37.3205, lng:-59.1310, precio:140000, tipo:"Casa", hab:3, banos:2, cochera:"Sí", patio:"Sí", pileta:"No", img:"https://images.unsplash.com/photo-1580587771525-78b9dba3b914?auto=format&fit=crop&w=1200&q=80", desc:"Construcción moderna con jardín."},
      { id:5, nombre:"Lote San Martín", lat:-37.3180, lng:-59.1365, precio:55000, tipo:"Lote", hab:0, banos:0, cochera:"No", patio:"Sí", pileta:"No", img:"https://images.unsplash.com/photo-1597092011895-8df03f092f31?auto=format&fit=crop&w=1200&q=80", desc:"Lote plano ideal para construir."},
      { id:6, nombre:"Casa del Parque", lat:-37.3150, lng:-59.1280, precio:125000, tipo:"Casa", hab:3, banos:2, cochera:"Sí", patio:"Sí", pileta:"Sí", img:"https://images.unsplash.com/photo-1600585154526-990dced4db0d?auto=format&fit=crop&w=1200&q=80", desc:"A pasos del parque, pileta y garage."},
      { id:7, nombre:"Depto Nuevo 7", lat:-37.3225, lng:-59.1270, precio:98000, tipo:"Departamento", hab:2, banos:1, cochera:"Sí", patio:"No", pileta:"No", img:"https://images.unsplash.com/photo-1560448075-bb4cde9cf1d0?auto=format&fit=crop&w=1200&q=80", desc:"Departamento moderno con amenities."},
      { id:8, nombre:"Quinta Los Robles", lat:-37.3370, lng:-59.1425, precio:320000, tipo:"Quinta", hab:5, banos:4, cochera:"Sí", patio:"Sí", pileta:"Sí", img:"https://images.unsplash.com/photo-1599423300746-b62533397364?auto=format&fit=crop&w=1200&q=80", desc:"Quinta de lujo con gran parque y pileta."},
      { id:9, nombre:"Casa en Esquina", lat:-37.3195, lng:-59.1395, precio:130000, tipo:"Casa", hab:3, banos:2, cochera:"No", patio:"Sí", pileta:"No", img:"https://images.unsplash.com/photo-1628744874010-cd15f4fb1512?auto=format&fit=crop&w=1200&q=80", desc:"Casa esquina con patio y parrilla."},
      { id:10, nombre:"Depto Económico", lat:-37.3235, lng:-59.1355, precio:65000, tipo:"Departamento", hab:1, banos:1, cochera:"No", patio:"No", pileta:"No", img:"https://images.unsplash.com/photo-1502672260266-1c1ef2d93688?auto=format&fit=crop&w=1200&q=80", desc:"Ideal primer vivienda o inversión."},
      { id:11, nombre:"Casa con Pileta", lat:-37.3285, lng:-59.1340, precio:175000, tipo:"Casa", hab:4, banos:3, cochera:"Sí", patio:"Sí", pileta:"Sí", img:"https://images.unsplash.com/photo-1582268611958-ebfd161ef9cf?auto=format&fit=crop&w=1200&q=80", desc:"Amplio fondo con pileta y quincho."},
      { id:12, nombre:"Casa Céntrica 12", lat:-37.3215, lng:-59.1325, precio:145000, tipo:"Casa", hab:3, banos:2, cochera:"Sí", patio:"No", pileta:"No", img:"https://images.unsplash.com/photo-1622760014404-20a7bb62a2c0?auto=format&fit=crop&w=1200&q=80", desc:"Cercana a comercios y servicios."},
      { id:13, nombre:"Quinta Galpón", lat:-37.3135, lng:-59.1415, precio:210000, tipo:"Quinta", hab:4, banos:3, cochera:"Sí", patio:"Sí", pileta:"Sí", img:"https://images.unsplash.com/photo-1625561938277-8f4c2dcab911?auto=format&fit=crop&w=1200&q=80", desc:"Con galpón y amplio terreno."},
      { id:14, nombre:"Lote Paraiso", lat:-37.3168, lng:-59.1430, precio:60000, tipo:"Lote", hab:0, banos:0, cochera:"No", patio:"Sí", pileta:"No", img:"https://images.unsplash.com/photo-1621534617104-2e2da9a1ef68?auto=format&fit=crop&w=1200&q=80", desc:"Lote esquina en zona tranquila."},
      { id:15, nombre:"Depto Nuevo 15", lat:-37.3116, lng:-59.1322, precio:98000, tipo:"Departamento", hab:2, banos:1, cochera:"Sí", patio:"No", pileta:"No", img:"https://images.unsplash.com/photo-1625586913130-f3b33b4d6bb1?auto=format&fit=crop&w=1200&q=80", desc:"Departamento con buena ventilación."}
    ];

    // Mapa, marcadores, rectangle
    let map, selectionRect, markers = [], markerById = {}, infoWindow;

    // Init map (callback)
    function initMap(){
      // centro en Tandil
      map = new google.maps.Map(document.getElementById('map'), {
        center: { lat: -37.321034, lng: -59.135002 },
        zoom: 13,
        gestureHandling: 'greedy'
      });

      infoWindow = new google.maps.InfoWindow();

      // Rectangle para seleccionar area
      const bounds = { north:-37.314, south:-37.336, east:-59.12, west:-59.152 };
      selectionRect = new google.maps.Rectangle({
        bounds,
        editable: true,
        draggable: true,
        map,
        strokeColor: '#FF9800',
        strokeWeight: 2,
        fillOpacity: 0.02
      });

      // listeners
      selectionRect.addListener('bounds_changed', updateFiltered);
      map.addListener('idle', updateFiltered);

      // inicial render
      updateFiltered();
    }

    // Clear markers
    function clearMarkers(){
      markers.forEach(m => m.setMap(null));
      markers = [];
      markerById = {};
    }

    // Abrir InfoWindow (acepta prop object or prop.id)
    function openInfo(propOrId){
      const prop = (typeof propOrId === 'number') ? propiedades.find(p => p.id === propOrId) : propOrId;
      if(!prop) return;
      const marker = markerById[prop.id];
      const html = `
        <div style="max-width:240px;">
          <img src="${prop.img}" style="width:100%; height:110px; object-fit:cover; border-radius:4px;">
          <h4 style="margin:8px 0 4px 0;">${prop.nombre}</h4>
          <p style="margin:0;font-size:13px;">${prop.desc}</p>
          <p style="margin:6px 0 0 0;font-size:13px;"><strong>Precio:</strong> $${prop.precio.toLocaleString()}</p>
          <p style="margin:4px 0 0 0;font-size:13px;">${prop.hab} hab • ${prop.banos} baños</p>
        </div>
      `;
      infoWindow.setContent(html);
      if(marker) infoWindow.open(map, marker);
    }

    // Centrar en propiedad y abrir info
    function goToProperty(id){
      const prop = propiedades.find(p => p.id === id);
      if(!prop) return;
      map.setCenter({ lat: prop.lat, lng: prop.lng });
      map.setZoom(16);
      setTimeout(()=> openInfo(prop), 250);
    }

    // Leer filtros UI
    function readFilters(){
      return {
        precioMin: Number(document.getElementById('precioMin').value) || 0,
        precioMax: Number(document.getElementById('precioMax').value) || Infinity,
        tipo: document.getElementById('tipo').value || '',
        habitaciones: document.getElementById('habitaciones').value || '',
        banos: document.getElementById('banos').value || '',
        cochera: document.getElementById('cochera').value || '',
        patio: document.getElementById('patio').value || '',
        pileta: document.getElementById('pileta').value || ''
      };
    }

    // Actualiza marcadores y listado según rectángulo + filtros
    function updateFiltered(){
      if(!selectionRect || !map) return;
      clearMarkers();
      const bounds = selectionRect.getBounds();
      if(!bounds) return;
      const f = readFilters();
      const listado = document.getElementById('listado');
      listado.innerHTML = '';

      propiedades.forEach(prop => {
        const pos = new google.maps.LatLng(prop.lat, prop.lng);
        if(!bounds.contains(pos)) return;

        // filtros
        if(prop.precio < f.precioMin || prop.precio > f.precioMax) return;
        if(f.tipo && prop.tipo && f.tipo !== prop.tipo) return;
        if(f.habitaciones && f.habitaciones !== '' && !(String(prop.hab) === f.habitaciones || (f.habitaciones==='4+' && prop.hab>=4))) return;
        if(f.banos && f.banos !== '' && !(String(prop.banos) === f.banos || (f.banos==='3+' && prop.banos>=3))) return;
        if(f.cochera && f.cochera !== '' && prop.cochera !== f.cochera) return;
        if(f.patio && f.patio !== '' && prop.patio !== f.patio) return;
        if(f.pileta && f.pileta !== '' && prop.pileta !== f.pileta) return;

        // marker
        const marker = new google.maps.Marker({
          position: { lat: prop.lat, lng: prop.lng },
          map,
          title: prop.nombre,
          animation: google.maps.Animation.DROP
        });
        markers.push(marker);
        markerById[prop.id] = marker;
        marker.addListener('click', () => openInfo(prop));

        // tarjeta en listado
        const card = document.createElement('div');
        card.className = 'prop-card';
        card.innerHTML = `
          <img src="${prop.img}" alt="${prop.nombre}">
          <div class="meta">
            <h4>${prop.nombre}</h4>
            <p class="small">${prop.desc}</p>
            <p style="margin-top:8px;"><strong>Precio:</strong> $${prop.precio.toLocaleString()}</p>
            <p>${prop.hab} hab • ${prop.banos} baños</p>
            <p class="small">Cochera: ${prop.cochera} • Patio: ${prop.patio} • Pileta: ${prop.pileta}</p>
          </div>
          <div class="actions">
            <button class="btn-map" onclick="goToProperty(${prop.id})">Ir al mapa</button>
            <button class="btn-view" onclick="openInfo(${prop.id})">Ver</button>
          </div>
        `;
        listado.appendChild(card);
      });

      if(!listado.hasChildNodes()){
        listado.innerHTML = '<div class="empty-msg">No hay propiedades en el área seleccionada.</div>';
      }
    }

    // Buscar: bajar al mapa y aplicar filtros
    function onBuscarClick(){
      document.getElementById('map').scrollIntoView({ behavior: 'smooth' });
      setTimeout(()=> {
        if(map) google.maps.event.trigger(map, 'resize');
        updateFiltered();
      }, 500);
    }

    // Limpiar filtros
    function limpiarFiltros(){
      document.querySelectorAll('#filtros input, #filtros select').forEach(el => el.value = '');
      updateFiltered();
    }

    // Cerrar sesión (mejorado)
    function logout(){
      try { sessionStorage.clear(); localStorage.clear(); } catch(e){}
      try {
        document.cookie.split(";").forEach(c => {
          const eq = c.indexOf("=");
          const name = eq > -1 ? c.substr(0, eq) : c;
          document.cookie = name + "=;expires=Thu, 01 Jan 1970 00:00:00 GMT;path=/";
        });
      } catch(e){}
      // Redirige a /login si existe; si no, recarga index
      const goLogin = () => window.location.href = "/login";
      try {
        fetch("/login", {method:"HEAD"}).then(()=>goLogin()).catch(()=>window.location.href = "/");
      } catch(e){
        window.location.href = "/";
      }
    }

    // Mostrar secciones
    function showSection(section){
      document.querySelectorAll('section').forEach(s => s.style.display = 'none');
      const el = document.getElementById(section) || document.querySelector(`#${section}`);
      if(section === 'propiedades') {
        document.getElementById('propiedades').style.display = 'block';
      } else if(section === 'servicios') {
        document.getElementById('servicios').style.display = 'block';
      }
      setTimeout(()=> { if(map) google.maps.event.trigger(map,'resize'); }, 300);
    }

    // Parallax scroll behavior
    (function setupParallax(){
      const par = document.getElementById('parallax');
      function onScroll(){
        const scrolled = window.pageYOffset || document.documentElement.scrollTop;
        const factor = 0.35;
        // Ajusta desplazamiento de ::before mediante variable CSS
        par.style.setProperty('--parallax-y', `${-scrolled * factor}px`);
        // Suave movimiento del contenido hero
        const hero = par.querySelector('.hero');
        if(hero) { hero.style.transform = `translateY(${scrolled * factor * 0.1}px)`; }
      }
      window.addEventListener('scroll', onScroll, { passive: true });
      onScroll();
    })();

    // === Inyectar imagen de fondo "paisaje con casa" como SVG -> base64 ===
    (function setParallaxBackground(){
      const svg = document.getElementById('parallax-bg-svg')?.textContent.trim();
      if(!svg) return;
      // codificar como base64 cuidando UTF-8
      const b64 = btoa(unescape(encodeURIComponent(svg)));
      const url = `url("data:image/svg+xml;base64,${b64}")`;
      document.getElementById('parallax').style.setProperty('--parallax-bg', url);
    })();
  </script>

  <!-- SVG embebido (paisaje con casa) que se convertirá a base64 en runtime -->
  <script id="parallax-bg-svg" type="image/svg+xml">
    <svg xmlns="http://www.w3.org/2000/svg" width="1600" height="900" viewBox="0 0 1600 900">
      <defs>
        <linearGradient id="sky" x1="0" y1="0" x2="0" y2="1">
          <stop offset="0%" stop-color="#EAF5FF"/>
          <stop offset="60%" stop-color="#87CEEB"/>
        </linearGradient>
        <linearGradient id="sun" x1="0" y1="0" x2="0" y2="1">
          <stop offset="0%" stop-color="#FFE69A"/>
          <stop offset="100%" stop-color="#FFD35A"/>
        </linearGradient>
        <linearGradient id="grass" x1="0" y1="0" x2="0" y2="1">
          <stop offset="0%" stop-color="#6BBE45"/>
          <stop offset="100%" stop-color="#3F8E25"/>
        </linearGradient>
        <filter id="soft" x="-20%" y="-20%" width="140%" height="140%">
          <feGaussianBlur stdDeviation="12" />
        </filter>
      </defs>

      <!-- Cielo -->
      <rect x="0" y="0" width="1600" height="900" fill="url(#sky)"/>

      <!-- Sol -->
      <circle cx="150" cy="140" r="90" fill="url(#sun)"/>

      <!-- Colinas -->
      <path d="M0,620 C300,560 500,660 800,600 C1100,540 1200,660 1600,600 L1600,900 L0,900 Z" fill="#5AA136" opacity="0.9"/>
      <path d="M0,660 C260,600 520,700 820,650 C1120,600 1360,730 1600,690 L1600,900 L0,900 Z" fill="#4C8D2E" opacity="0.95"/>

      <!-- Pasto -->
      <rect x="0" y="650" width="1600" height="250" fill="url(#grass)"/>

      <!-- Casa -->
      <g transform="translate(920,470)">
        <!-- Cuerpo -->
        <rect x="0" y="0" width="420" height="240" fill="#F5F5F5" stroke="#BEBEBE" stroke-width="3" />
        <!-- Techo -->
        <polygon points="-20,0 440,0 210,-150" fill="#A52A2A" stroke="#7A1B1B" stroke-width="3"/>
        <!-- Puerta -->
        <rect x="175" y="110" width="70" height="130" fill="#7A4B28"/>
        <!-- Ventanas -->
        <g fill="#BEE3FF" stroke="#9AA9B1">
          <rect x="30" y="40" width="80" height="70"/>
          <rect x="145" y="40" width="80" height="70"/>
          <rect x="260" y="40" width="80" height="70"/>
          <rect x="375" y="40" width="80" height="70"/>
        </g>
      </g>

      <!-- Camino -->
      <path d="M1129,710 C1160,790 1180,900 1180,900 L1020,900 C1020,900 1040,780 1070,710 Z" fill="#BDA89B" opacity="0.85"/>

      <!-- Árboles -->
      <g transform="translate(300,690)">
        <rect x="-12" y="-90" width="24" height="90" fill="#6B3F13"/>
        <circle cx="0" cy="-120" r="60" fill="#2E8B57"/>
        <circle cx="-40" cy="-140" r="40" fill="#3CB371"/>
        <circle cx="40" cy="-140" r="40" fill="#3CB371"/>
      </g>
      <g transform="translate(200,720)">
        <rect x="-14" y="-100" width="28" height="100" fill="#6B3F13"/>
        <circle cx="0" cy="-130" r="70" fill="#2E8B57"/>
        <circle cx="-50" cy="-150" r="45" fill="#3CB371"/>
        <circle cx="50" cy="-150" r="45" fill="#3CB371"/>
      </g>
      <g transform="translate(1300,720)">
        <rect x="-12" y="-90" width="24" height="90" fill="#6B3F13"/>
        <circle cx="0" cy="-120" r="60" fill="#2E8B57"/>
        <circle cx="-40" cy="-140" r="40" fill="#3CB371"/>
        <circle cx="40" cy="-140" r="40" fill="#3CB371"/>
      </g>
    </svg>
  </script>

  <!-- Google Maps API: reemplaza si querés tu propia key -->
  <script src="https://maps.googleapis.com/maps/api/js?key=AIzaSyA87DrL-PkY4aGrMgdQZRmqd_iCMzb8rEM&callback=initMap" async defer></script>
</body>
</html>
