// --- MOTOR DE MEMORIA MASIVA (IndexedDB) ---
const DB_NAME = "MotorIndustrialDB_V6"; const STORE_NAME = "tablas_maestras";
function initDB() { return new Promise((resolve, reject) => { let request = indexedDB.open(DB_NAME, 1); request.onupgradeneeded = (e) => { let db = e.target.result; if (!db.objectStoreNames.contains(STORE_NAME)) db.createObjectStore(STORE_NAME); }; request.onsuccess = (e) => resolve(e.target.result); request.onerror = (e) => reject(e.target.error); }); }
async function guardarEstado(key, data) { try { let db = await initDB(); let tx = db.transaction(STORE_NAME, "readwrite"); tx.objectStore(STORE_NAME).put(data, key); } catch(e) {} }
async function cargarEstado(key) { try { let db = await initDB(); return new Promise((resolve, reject) => { let tx = db.transaction(STORE_NAME, "readonly"); let req = tx.objectStore(STORE_NAME).get(key); req.onsuccess = () => resolve(req.result); req.onerror = () => reject(req.error); }); } catch(e) { return null; } }

// Variables Globales
let baseDeDatos = []; let datosFiltrados = []; let baseDeCostos = []; let costosFiltrados = []; 
let archivosTemporalesLeidos = []; let reglasNormalizacion = {}; let reglasCalculos = []; 
let diccionarioLineas = {}; let diccionarioNombres = {}; let diccionarioCampos = {}; 
let listaCriticidad = []; let arbolLineaSeleccionada = "";

async function arrancarSistema() {
    baseDeDatos = (await cargarEstado('bd_industrial')) || []; baseDeCostos = (await cargarEstado('bd_costos')) || [];
    diccionarioLineas = (await cargarEstado('diccionario_lineas')) || {}; diccionarioNombres = (await cargarEstado('diccionario_nombres')) || {};
    diccionarioCampos = (await cargarEstado('diccionario_campos')) || {}; listaCriticidad = (await cargarEstado('bd_criticidad')) || [];
    let tc = localStorage.getItem('tipo_cambio'); if(tc) document.getElementById('inputTipoCambio').value = tc;
    actualizarSelectoresGlobales(); actualizarSelectoresCostos(); actualizarSelectoresConexionCriticidad(); actualizarListasFiltrosSecundarios(); actualizarSelectoresArbol(); aplicarFiltro(); actualizarPreviewCriticidad(); renderizarTablaCriticidad();
}

function guardarTipoCambio() { let val = document.getElementById('inputTipoCambio').value; localStorage.setItem('tipo_cambio', val); }
function obtenerTC() { let tc = parseFloat(document.getElementById('inputTipoCambio').value); return (tc && tc > 0) ? tc : 1; }
function obtenerSimboloMoneda() { return obtenerTC() > 1 ? "USD $" : "Q"; }
function limpiarFormato(llave, valor) { if (valor === undefined || valor === null) return ""; let v = String(valor).trim(); if (v === "") return ""; if (!isNaN(v) && !v.includes("/") && !v.includes(",")) return parseFloat(v); let esFecha = /^(\d{1,2})\/(\d{1,2})\/(\d{4})$/.exec(v); if (esFecha) return `${esFecha[3]}-${esFecha[2].padStart(2, '0')}-${esFecha[1].padStart(2, '0')}`; return v; }
function parsearMoneda(v) { if (v === undefined || v === null) return 0; let s = String(v).replace(/[^0-9\.,-]/g, ''); if (s === '') return 0; let ld = s.lastIndexOf('.'); let lc = s.lastIndexOf(','); if (lc > ld) { s = s.replace(/\./g, ''); s = s.replace(',', '.'); } else if (ld > lc) { s = s.replace(/,/g, ''); } else if (lc > -1 && ld === -1) { s = s.replace(',', '.'); } let val = parseFloat(s); return isNaN(val) ? 0 : val; }
function escaparRegExp(string) { return string.replace(/[.*+?^${}()|[\]\\]/g, '\\$&'); }

function cambiarPestana(pestana) {
    ['panelDatos', 'panelDashboard', 'panelDatosCostos', 'panelCostos', 'panelJerarquia', 'panelCriticidad'].forEach(id => document.getElementById(id).style.display = 'none');
    ['btnPestanaDatos', 'btnPestanaDashboard', 'btnPestanaDatosCostos', 'btnPestanaCostos', 'btnPestanaJerarquia', 'btnPestanaCriticidad'].forEach(id => document.getElementById(id).classList.remove('activa'));
    document.getElementById('panel' + pestana.charAt(0).toUpperCase() + pestana.slice(1)).style.display = 'block';
    document.getElementById('btnPestana' + pestana.charAt(0).toUpperCase() + pestana.slice(1)).classList.add('activa');
    if (pestana === 'datos' || pestana === 'datosCostos' || pestana === 'dashboard' || pestana === 'costos') aplicarFiltro();
    if (pestana === 'jerarquia') { actualizarSelectoresArbol(); renderizarBotoneraArbol(); }
    if (pestana === 'criticidad') renderizarTablaCriticidad();
}

// --- FASE 1: LEER ARCHIVOS Y UPSERT ---
function leerArchivosParaMapeo() { const input = document.getElementById('archivosCsv'); if (!input.files.length) return alert("Selecciona archivo."); archivosTemporalesLeidos = []; reglasNormalizacion = {}; reglasCalculos = []; document.getElementById('listaCalculosActivos').innerHTML=''; let archivosProcesados = 0; const conjunto = new Set(); Array.from(input.files).forEach(archivo => { Papa.parse(archivo, { header: true, skipEmptyLines: true, complete: function(res) { archivosTemporalesLeidos.push(res.data); if (res.data.length > 0) Object.keys(res.data[0]).forEach(k => conjunto.add(k.trim())); archivosProcesados++; if (archivosProcesados === input.files.length) construirModalMapeo(Array.from(conjunto)); }}); }); }
function construirModalMapeo(encabezados) { const cont = document.getElementById('contenedorMapeo'); cont.innerHTML = ''; const destino = document.getElementById('destinoImportacion').value; encabezados.sort().forEach((col, index) => { let sug = col; if (col.toLowerCase().includes('maquina') || col.toLowerCase().includes('máquina') || col.toLowerCase().includes('maquinaria')) sug = "Máquina Consolidada"; if (destino === 'costos' && (col.toLowerCase().includes('valor') || col.toLowerCase().includes('costo'))) sug = "Costo Total"; if (col.includes('[')) sug = "Aplicación / Falla Identificada"; let colEsc = col.replace(/"/g, '&quot;'); cont.innerHTML += `<div class="fila-mapeo" id="fila_${index}"><div style="width: 25%; font-weight: bold; word-break: break-all;">${col}</div><div style="width: 30%;"><input type="text" class="col-destino" id="map_${index}" data-original="${colEsc}" value="${sug}" style="width: 90%;" onkeyup="generarVistaPrevia()" onchange="generarVistaPrevia()"></div><div style="width: 15%; text-align: center;"><input type="checkbox" id="val_${index}" onchange="generarVistaPrevia()"></div><div style="width: 15%; text-align: center;"><input type="checkbox" id="importar_${index}" checked onchange="alternarColumnaIgnorada(${index})"></div><div style="width: 15%; text-align: center;"><button class="btn-chico" onclick="abrirModalNormalizar('${colEsc}')">✏️ Editar</button><span id="badge_norm_${index}" style="color: green; font-weight: bold;"></span></div></div>`; }); document.getElementById('modalMapeo').showModal(); generarVistaPrevia(); }
function alternarColumnaIgnorada(index) { const ok = document.getElementById(`importar_${index}`).checked; const f = document.getElementById(`fila_${index}`); document.getElementById(`map_${index}`).disabled = !ok; document.getElementById(`val_${index}`).disabled = !ok; if (!ok) f.classList.add('ignorada'); else f.classList.remove('ignorada'); generarVistaPrevia(); }
function cerrarModal() { document.getElementById('modalMapeo').close(); }

// --- NUEVO: CREAR COLUMNA MANUAL ---
function abrirModalAgregarColumna() { document.getElementById('addColNombre').value = ''; document.getElementById('addColValor').value = ''; document.getElementById('modalAgregarColumna').showModal(); }
function ejecutarAgregarColumna() {
    const destino = document.getElementById('addColBase').value; const nombre = document.getElementById('addColNombre').value.trim(); const valDef = document.getElementById('addColValor').value.trim();
    if(!nombre) return alert("Escribe un nombre para la columna.");
    if(destino === 'mantenimiento') { if(baseDeDatos.length===0) return alert("Base vacía."); baseDeDatos.forEach(f => f[nombre] = valDef); guardarEstado('bd_industrial', baseDeDatos); }
    else { if(baseDeCostos.length===0) return alert("Base vacía."); baseDeCostos.forEach(f => f[nombre] = valDef); guardarEstado('bd_costos', baseDeCostos); }
    document.getElementById('modalAgregarColumna').close(); actualizarSelectoresGlobales(); actualizarSelectoresCostos(); actualizarListasFiltrosSecundarios(); actualizarSelectoresArbol(); aplicarFiltro(); alert("¡Columna agregada!");
}
function cerrarModalAgregarColumna() { document.getElementById('modalAgregarColumna').close(); }

// Modales Estandarizar
function abrirModalUnificarCampos() { pintarListaUnificacionCampos(); document.getElementById('modalUnificarCampos').showModal(); }
function pintarListaUnificacionCampos() { const baseObj = document.getElementById('uniCampBaseSeleccionada').value === 'mantenimiento' ? baseDeDatos : baseDeCostos; const tbody = document.getElementById('listaUnificacionCampos'); tbody.innerHTML = ''; if (baseObj.length === 0) { tbody.innerHTML = '<tr><td colspan="2" style="text-align:center;">Base vacía.</td></tr>'; return; } const conj = new Set(); baseObj.forEach(f => Object.keys(f).forEach(llave => conj.add(llave))); Array.from(conj).sort().forEach((colCrudo, idx) => { let valEstandar = diccionarioCampos[colCrudo] || colCrudo; let esc = colCrudo.replace(/"/g, '&quot;'); tbody.innerHTML += `<tr><td style="padding: 5px;">${colCrudo}</td><td style="padding: 5px;"><input type="text" id="uniCamp_${idx}" data-crudo="${esc}" value="${valEstandar}" style="width: 95%; padding: 4px;"></td></tr>`; }); }
function guardarUnificacionCampos() { document.querySelectorAll('input[id^="uniCamp_"]').forEach(input => { let crudo = input.getAttribute('data-crudo'); let estandar = input.value.trim(); if (estandar && estandar !== crudo) diccionarioCampos[crudo] = estandar; }); guardarEstado('diccionario_campos', diccionarioCampos); function renombrar(baseArray) { return baseArray.map(obj => { let newObj = {}; for (let key in obj) { let newKey = diccionarioCampos[key] || key; if(newObj[newKey] !== undefined) { let strE = String(newObj[newKey]); let strV = String(obj[key]); if(!strE.includes(strV) && strV.trim()!=="") newObj[newKey] = strE + " | " + strV; } else { newObj[newKey] = obj[key]; } } return newObj; }); } if (baseDeDatos.length > 0) { baseDeDatos = renombrar(baseDeDatos); guardarEstado('bd_industrial', baseDeDatos); } if (baseDeCostos.length > 0) { baseDeCostos = renombrar(baseDeCostos); guardarEstado('bd_costos', baseDeCostos); } document.getElementById('modalUnificarCampos').close(); actualizarSelectoresGlobales(); actualizarSelectoresCostos(); actualizarListasFiltrosSecundarios(); actualizarSelectoresArbol(); aplicarFiltro(); alert("¡Columnas unificadas!"); }
function cerrarModalUnificarCampos() { document.getElementById('modalUnificarCampos').close(); }

function abrirModalUnificar() { actualizarSelectorColumnasUnificar(); document.getElementById('modalUnificar').showModal(); }
function actualizarSelectorColumnasUnificar() { const baseObj = document.getElementById('uniBaseSeleccionada').value === 'mantenimiento' ? baseDeDatos : baseDeCostos; if (baseObj.length === 0) { document.getElementById('uniColumnaObjetivo').innerHTML = '<option value="">(Base vacía)</option>'; return; } const conj = new Set(); baseObj.forEach(f => Object.keys(f).forEach(llave => conj.add(llave))); const selector = document.getElementById('uniColumnaObjetivo'); selector.innerHTML = ''; Array.from(conj).sort().forEach(col => { selector.innerHTML += `<option value="${col}">${col}</option>`; }); let ideal = Array.from(conj).find(c => c.toLowerCase().includes('maquina') || c.toLowerCase().includes('máquina') || c.toLowerCase().includes('seccion') || c.toLowerCase().includes('sección')); if (ideal) selector.value = ideal; pintarListaUnificacion(); }
function pintarListaUnificacion() { const baseObj = document.getElementById('uniBaseSeleccionada').value === 'mantenimiento' ? baseDeDatos : baseDeCostos; const columna = document.getElementById('uniColumnaObjetivo').value; const tbody = document.getElementById('listaUnificacion'); tbody.innerHTML = ''; if(!columna) return; const unicas = new Set(); baseObj.forEach(f => { let v = f[columna]; if(v && String(v).trim() !== "") unicas.add(String(v).trim()); }); Array.from(unicas).sort().forEach((valCrudo, idx) => { let valEstandar = diccionarioNombres[valCrudo] || valCrudo; let esc = valCrudo.replace(/"/g, '&quot;'); tbody.innerHTML += `<tr><td style="padding: 5px;">${valCrudo}</td><td style="padding: 5px;"><input type="text" id="uniLine_${idx}" data-crudo="${esc}" value="${valEstandar}" style="width: 95%; padding: 4px;"></td></tr>`; }); }
function guardarUnificacion() { document.querySelectorAll('input[id^="uniLine_"]').forEach(input => { let crudo = input.getAttribute('data-crudo'); let estandar = input.value.trim(); if (estandar && estandar !== crudo) diccionarioNombres[crudo] = estandar; }); guardarEstado('diccionario_nombres', diccionarioNombres); [baseDeDatos, baseDeCostos].forEach(baseObj => { if(baseObj.length === 0) return; let columnas = Array.from(new Set(baseObj.flatMap(Object.keys))); columnas.forEach(col => { baseObj.forEach(fila => { let v = fila[col]; if (v && diccionarioNombres[v]) fila[col] = diccionarioNombres[v]; }); }); }); guardarEstado('bd_industrial', baseDeDatos); guardarEstado('bd_costos', baseDeCostos); document.getElementById('modalUnificar').close(); actualizarSelectoresGlobales(); actualizarSelectoresCostos(); actualizarListasFiltrosSecundarios(); aplicarFiltro(); alert("¡Nombres estandarizados en todas las bases!"); }
function cerrarModalUnificar() { document.getElementById('modalUnificar').close(); }

function abrirModalCategorizar() { actualizarSelectorColumnasCategorizar(); document.getElementById('modalCategorizar').showModal(); }
function actualizarSelectorColumnasCategorizar() { const baseObj = document.getElementById('catBaseSeleccionada').value === 'mantenimiento' ? baseDeDatos : baseDeCostos; if (baseObj.length === 0) { document.getElementById('catColumnaMaquinas').innerHTML = '<option value="">(Base vacía)</option>'; return; } const conj = new Set(); baseObj.forEach(f => Object.keys(f).forEach(llave => conj.add(llave))); const selector = document.getElementById('catColumnaMaquinas'); selector.innerHTML = ''; Array.from(conj).sort().forEach(col => { selector.innerHTML += `<option value="${col}">${col}</option>`; }); let ideal = Array.from(conj).find(c => c.toLowerCase().includes('maquina') || c.toLowerCase().includes('máquina')); if (ideal) selector.value = ideal; pintarListaCategorizacion(); }
function pintarListaCategorizacion() { const baseObj = document.getElementById('catBaseSeleccionada').value === 'mantenimiento' ? baseDeDatos : baseDeCostos; const columna = document.getElementById('catColumnaMaquinas').value; const tbody = document.getElementById('listaCategorizacion'); tbody.innerHTML = ''; if(!columna) return; const unicas = new Set(); baseObj.forEach(f => { let maq = f[columna]; if(maq && String(maq).trim() !== "") unicas.add(String(maq).trim()); }); Array.from(unicas).sort().forEach((maq, idx) => { let val = diccionarioLineas[maq] || ""; let esc = maq.replace(/"/g, '&quot;'); tbody.innerHTML += `<tr><td style="padding: 5px; font-weight: bold;">${maq}</td><td style="padding: 5px;"><input type="text" id="catLine_${idx}" data-maquina="${esc}" value="${val}" style="width: 95%; padding: 4px;"></td></tr>`; }); }
function guardarCategorizacion() { document.querySelectorAll('input[id^="catLine_"]').forEach(input => { let m = input.getAttribute('data-maquina'); let l = input.value.trim(); if (l) diccionarioLineas[m] = l; }); guardarEstado('diccionario_lineas', diccionarioLineas); [baseDeDatos, baseDeCostos].forEach(baseObj => { if(baseObj.length === 0) return; let columnas = Array.from(new Set(baseObj.flatMap(Object.keys))); let colMaq = columnas.find(c => c.toLowerCase().includes('maquina') || c.toLowerCase().includes('máquina')); if (colMaq) { baseObj.forEach(fila => { let m = fila[colMaq]; if (m && diccionarioLineas[m]) fila["Línea de Producción"] = diccionarioLineas[m]; }); } }); guardarEstado('bd_industrial', baseDeDatos); guardarEstado('bd_costos', baseDeCostos); document.getElementById('modalCategorizar').close(); actualizarSelectoresGlobales(); actualizarSelectoresCostos(); actualizarListasFiltrosSecundarios(); aplicarFiltro(); alert("¡Líneas asignadas!"); }
function cerrarModalCategorizar() { document.getElementById('modalCategorizar').close(); }

// Formularios
function abrirModalCalculo() { const o1=document.getElementById('calcOrigen1'); const o2=document.getElementById('calcOrigen2'); o1.innerHTML=''; o2.innerHTML=''; const r=extraerReglasDeUI(); const u=Array.from(new Set(Object.values(r.mapaDestinos))); if(u.length===0)return alert("Mapea columnas primero."); u.sort().forEach(c=>{o1.innerHTML+=`<option value="${c}">${c}</option>`; o2.innerHTML+=`<option value="${c}">${c}</option>`;}); cambiarFormularioCalculo(); document.getElementById('modalCalculado').showModal(); }
function cambiarFormularioCalculo() { const t=document.getElementById('tipoCalculo').value; const c2=document.getElementById('contenedorOrigen2'); if(t==='tiempo_muerto'||t==='concatenar')c2.style.display='block'; else c2.style.display='none'; const n={"dia_semana":"Día de la Semana","mes_nombre":"Mes Texto","anio":"Año","tiempo_muerto":"Tiempo Muerto (Hrs)","concatenar":"Columna Unida"}; document.getElementById('calcDestino').value=n[t]; }
function cerrarModalCalculo() { document.getElementById('modalCalculado').close(); }
function guardarCalculo() { const t=document.getElementById('tipoCalculo').value; const o1=document.getElementById('calcOrigen1').value; const o2=document.getElementById('calcOrigen2').value; const d=document.getElementById('calcDestino').value.trim(); if(!d)return; reglasCalculos.push({tipo:t,origen1:o1,origen2:o2,destino:d}); cerrarModalCalculo(); pintarCalculosActivos(); generarVistaPrevia(); }
function borrarCalculo(i) { reglasCalculos.splice(i,1); pintarCalculosActivos(); generarVistaPrevia(); }
function pintarCalculosActivos() { const d=document.getElementById('listaCalculosActivos'); d.innerHTML=''; if(reglasCalculos.length===0)d.innerHTML='<span style="color:#999; font-style:italic;">No hay cálculos.</span>'; reglasCalculos.forEach((r,i)=>{ d.innerHTML+=`<span style="background:#e6ffed; border:1px solid #b7eb8f; padding:4px 8px; border-radius:4px; display:inline-block; margin:3px; color:#389e0d; font-weight:bold;">ƒ ${r.destino} <button onclick="borrarCalculo(${i})" style="border:none; background:none; cursor:pointer; color:red;">✖</button></span>`; }); }

function calcularDiferenciaHoras(inicio, fin) { 
    function aMs(t) { let s = String(t).toLowerCase().trim().replace(/[^0-9:apm]/g, ''); if(!s) return null; let isPM = s.includes('p'); let isAM = s.includes('a'); let parts = s.replace(/[apm]/g, '').split(':'); let h = parseInt(parts[0]) || 0; let m = parseInt(parts[1]) || 0; if(isPM && h < 12) h += 12; if(isAM && h === 12) h = 0; return (h * 60) + m; } 
    let mI = aMs(inicio); let mF = aMs(fin); if(mI === null || mF === null) return 0; if(mF < mI) mF += (24 * 60); let diffMinutos = mF - mI; return parseFloat((diffMinutos / 60).toFixed(2)); 
}

function abrirModalNormalizar(co) { columnaEnEdicionActual=co; document.getElementById('tituloColumnaNormalizar').innerText=co; document.getElementById('inputBuscar').value=''; document.getElementById('inputReemplazar').value=''; pintarReglasNormalizacion(); document.getElementById('modalNormalizar').showModal(); }
function agregarReglaNormalizacion() { const b=document.getElementById('inputBuscar').value; const r=document.getElementById('inputReemplazar').value; if(!b)return; if(!reglasNormalizacion[columnaEnEdicionActual])reglasNormalizacion[columnaEnEdicionActual]=[]; reglasNormalizacion[columnaEnEdicionActual].push({buscar:b,reemplazar:r}); document.getElementById('inputBuscar').value=''; document.getElementById('inputReemplazar').value=''; pintarReglasNormalizacion(); }
function eliminarReglaNormalizacion(i) { reglasNormalizacion[columnaEnEdicionActual].splice(i,1); pintarReglasNormalizacion(); }
function pintarReglasNormalizacion() { const t=document.getElementById('listaReglasNormalizacion'); t.innerHTML=''; const r=reglasNormalizacion[columnaEnEdicionActual]||[]; r.forEach((re,i)=>{ t.innerHTML+=`<tr><td>${re.buscar}</td><td>${re.reemplazar}</td><td style="text-align:center;"><button class="btn-chico" style="color:red;" onclick="eliminarReglaNormalizacion(${i})">X</button></td></tr>`; }); }
function cerrarModalNormalizar() { document.getElementById('modalNormalizar').close(); document.querySelectorAll('.col-destino').forEach((input)=>{ const i=input.id.split('_')[1]; const c=input.getAttribute('data-original'); const b=document.getElementById(`badge_norm_${i}`); if(reglasNormalizacion[c]&&reglasNormalizacion[c].length>0)b.innerText=`(${reglasNormalizacion[c].length} reglas)`; else b.innerText=''; }); generarVistaPrevia(); }

function extraerReglasDeUI() { let mD = {}; let mU = {}; document.querySelectorAll('.col-destino').forEach((input) => { const i = input.id.split('_')[1]; const cO = input.getAttribute('data-original'); const cD = input.value.trim(); const uE = document.getElementById(`val_${i}`).checked; const imp = document.getElementById(`importar_${i}`).checked; if (imp && cD !== "") { mD[cO] = cD; mU[cO] = uE; } }); return { mapaDestinos: mD, mapaUsarEncabezado: mU }; }
function aplicarMapeoAUnaFila(filaOriginal, reglas) {
    let filaNueva = {};
    for (let encabezado in filaOriginal) {
        let nO = encabezado.trim(); let nD = reglas.mapaDestinos[nO];
        if (nD) {
            let v = limpiarFormato(nO, filaOriginal[encabezado]);
            if (reglas.mapaUsarEncabezado[nO]) { let vc = String(v).toLowerCase().trim(); if (v !== "" && vc !== "no" && vc !== "falso" && vc !== "0") v = nO; else v = ""; }
            if (reglasNormalizacion[nO] && v !== "") { reglasNormalizacion[nO].forEach(r => { v = String(v).replace(new RegExp(escaparRegExp(r.buscar), 'gi'), r.reemplazar); }); }
            if (v !== "") { if (filaNueva[nD] !== undefined) { let sE = String(filaNueva[nD]); let sV = String(v); if (!sE.includes(sV)) filaNueva[nD] = sE + " | " + sV; } else { filaNueva[nD] = v; } }
        }
    }
    reglasCalculos.forEach(calc => {
        const v1 = filaNueva[calc.origen1] || ""; const v2 = filaNueva[calc.origen2] || ""; let res = "";
        if (v1 !== "") { try { if (calc.tipo === "dia_semana" && String(v1).includes("-")) { const d = new Date(v1 + "T12:00:00"); res = d.toLocaleDateString('es-ES', {weekday: 'long'}); res = res.charAt(0).toUpperCase() + res.slice(1); } else if (calc.tipo === "mes_nombre" && String(v1).includes("-")) { const d = new Date(v1 + "T12:00:00"); res = d.toLocaleDateString('es-ES', {month: 'long'}); res = res.charAt(0).toUpperCase() + res.slice(1); } else if (calc.tipo === "anio" && String(v1).includes("-")) { res = String(v1).split('-')[0]; } } catch(e){} }
        if (calc.tipo === "concatenar" && (v1 || v2)) { res = (v1 + " - " + v2).trim(); if (res === "-") res = ""; } else if (calc.tipo === "tiempo_muerto" && v1 && v2) { res = calcularDiferenciaHoras(v1, v2); }
        if (res !== "") filaNueva[calc.destino] = res;
    });
    return filaNueva;
}
function generarVistaPrevia() { 
    const reglas = extraerReglasDeUI(); const head = document.getElementById('headPreview'); const body = document.getElementById('bodyPreview'); head.innerHTML = ''; body.innerHTML = ''; if (archivosTemporalesLeidos.length === 0) return; 
    let destUnicos = Array.from(new Set(Object.values(reglas.mapaDestinos))); reglasCalculos.forEach(c => destUnicos.push(c.destino)); const colVisibles = Array.from(new Set(destUnicos)).sort(); 
    if (colVisibles.length === 0) { body.innerHTML = '<tr><td>Todo ignorado.</td></tr>'; return; } 
    [1,2,3].forEach(n => { let sel = document.getElementById('dupCol'+n); let val = sel.value; sel.innerHTML = '<option value="">-- Vacío --</option>'; colVisibles.forEach(c => sel.innerHTML += `<option value="${c}">${c}</option>`); if(colVisibles.includes(val)) sel.value = val; });
    let trHead = '<tr>'; colVisibles.forEach(c => trHead += `<th>${c}</th>`); head.innerHTML = trHead + '</tr>'; let filasPreview = []; for (let f = 0; f < archivosTemporalesLeidos.length; f++) { for (let i = 0; i < archivosTemporalesLeidos[f].length; i++) { let fila = aplicarMapeoAUnaFila(archivosTemporalesLeidos[f][i], reglas); if (Object.keys(fila).length > 0) { filasPreview.push(fila); if (filasPreview.length >= 10) break; } } if (filasPreview.length >= 10) break; } if (filasPreview.length === 0) { body.innerHTML = `<tr><td colspan="${colVisibles.length}">Ningún dato.</td></tr>`; return; } filasPreview.forEach(f => { let tr = '<tr>'; colVisibles.forEach(c => tr += `<td>${f[c] !== undefined ? f[c] : ''}</td>`); body.innerHTML += tr + '</tr>'; }); 
}

function ejecutarImportacionConMapeo() {
    const destino = document.getElementById('destinoImportacion').value; const reglas = extraerReglasDeUI(); let nuevosDatos = [];
    archivosTemporalesLeidos.forEach(datosArchivo => { const limpios = datosArchivo.map(f => aplicarMapeoAUnaFila(f, reglas)); nuevosDatos = nuevosDatos.concat(limpios.filter(f => Object.keys(f).length > 0)); });
    if (nuevosDatos.length === 0) return alert("No hay datos para importar.");

    if (Object.keys(diccionarioCampos).length > 0) { nuevosDatos = nuevosDatos.map(obj => { let nO = {}; for (let k in obj) { let nk = diccionarioCampos[k] || k; if(nO[nk] !== undefined) { let sE = String(nO[nk]); let sV = String(obj[k]); if(!sE.includes(sV) && sV.trim()!=="") nO[nk] = sE + " | " + sV; } else { nO[nk] = obj[k]; } } return nO; }); }
    if (Object.keys(diccionarioNombres).length > 0) { let columnas = Array.from(new Set(nuevosDatos.flatMap(Object.keys))); columnas.forEach(col => { nuevosDatos.forEach(fila => { let v = fila[col]; if (v && diccionarioNombres[v]) fila[col] = diccionarioNombres[v]; }); }); }
    if (Object.keys(diccionarioLineas).length > 0) { let columnas = Array.from(new Set(nuevosDatos.flatMap(Object.keys))); let colMaq = columnas.find(c => c.toLowerCase().includes('maquina') || c.toLowerCase().includes('máquina')); if (colMaq) { nuevosDatos.forEach(fila => { let maq = fila[colMaq]; if (maq && diccionarioLineas[maq]) fila["Línea de Producción"] = diccionarioLineas[maq]; }); } }

    let baseDestino = destino === 'mantenimiento' ? baseDeDatos : baseDeCostos;
    let checkDup = document.getElementById('checkEvitarDuplicados').checked;
    let d1 = document.getElementById('dupCol1').value; let d2 = document.getElementById('dupCol2').value; let d3 = document.getElementById('dupCol3').value;
    
    if (checkDup) {
        let colKeys = Array.from(new Set(nuevosDatos.flatMap(Object.keys))); let idColName = colKeys.find(k => k.toLowerCase().includes('reporte_id') || k.toLowerCase().includes('salida_id'));
        let usarComposite = d1 || d2 || d3; let act = 0; let ins = 0;
        nuevosDatos.forEach(nr => { 
            let idVal = "";
            if (usarComposite) idVal = (d1 ? String(nr[d1]||'').trim() : "") + "|" + (d2 ? String(nr[d2]||'').trim() : "") + "|" + (d3 ? String(nr[d3]||'').trim() : "");
            else if (idColName) idVal = String(nr[idColName]||'').trim();
            if (!idVal || idVal === "||" || idVal === "|") { baseDestino.push(nr); ins++; return; } 
            let idx = baseDestino.findIndex(r => { if (usarComposite) { let rVal = (d1 ? String(r[d1]||'').trim() : "") + "|" + (d2 ? String(r[d2]||'').trim() : "") + "|" + (d3 ? String(r[d3]||'').trim() : ""); return rVal === idVal; } else return String(r[idColName]||'').trim() === idVal; }); 
            if (idx !== -1) { baseDestino[idx] = { ...baseDestino[idx], ...nr }; act++; } else { baseDestino.push(nr); ins++; } 
        });
        alert(`UPSERT exitoso a ${destino}\nNuevos: ${ins} | Actualizados: ${act}`);
    } else { baseDestino = baseDestino.concat(nuevosDatos); alert(`Datos agregados a ${destino} (Sin revisión de duplicados)`); }

    if (destino === 'mantenimiento') guardarEstado('bd_industrial', baseDestino); else guardarEstado('bd_costos', baseDestino);
    archivosTemporalesLeidos = []; reglasNormalizacion = {}; reglasCalculos = [];
    actualizarSelectoresGlobales(); actualizarSelectoresCostos(); actualizarSelectoresConexionCriticidad(); actualizarListasFiltrosSecundarios(); actualizarSelectoresArbol(); document.getElementById('archivosCsv').value = ""; cerrarModal(); aplicarFiltro();
}

// --- FILTROS GLOBALES Y SECUNDARIOS ---
function actualizarSelectoresGlobales() {
    if (baseDeDatos.length === 0) { document.getElementById('selectorColumnaDashboard').innerHTML = ''; document.getElementById('selectorColumnaFecha').innerHTML = ''; return; }
    const conj = new Set(); baseDeDatos.forEach(f => Object.keys(f).forEach(k => conj.add(k))); const cols = Array.from(conj).sort();
    const sDash = document.getElementById('selectorColumnaDashboard'); sDash.innerHTML = ''; cols.forEach(c => sDash.innerHTML += `<option value="${c}">${c}</option>`);
    let cMaq = cols.find(c => c.toLowerCase().includes('maquina') || c.toLowerCase().includes('máquina') || c.toLowerCase() === 'línea de producción'); if (cMaq) sDash.value = cMaq;
    const sFec = document.getElementById('selectorColumnaFecha'); const vFec = sFec.value; sFec.innerHTML = ''; cols.forEach(c => sFec.innerHTML += `<option value="${c}">${c}</option>`);
    if (vFec && cols.includes(vFec)) sFec.value = vFec; else { let cFec = cols.find(c => c.toLowerCase().includes('fecha') || c.toLowerCase().includes('date')); if (cFec) sFec.value = cFec; }
}

function actualizarSelectoresCostos() {
    if (baseDeCostos.length === 0) { ['costoColMaquina', 'costoColNivel2', 'costoColNivel3', 'costoColFecha', 'costoColValor'].forEach(id => document.getElementById(id).innerHTML = ''); return; }
    const conj = new Set(); baseDeCostos.forEach(f => Object.keys(f).forEach(k => conj.add(k))); const cols = Array.from(conj).sort();
    const sMaq = document.getElementById('costoColMaquina'); const sN2 = document.getElementById('costoColNivel2'); const sN3 = document.getElementById('costoColNivel3'); const sFec = document.getElementById('costoColFecha'); const sVal = document.getElementById('costoColValor');
    sMaq.innerHTML = ''; sN2.innerHTML = '<option value="">-- Ninguno --</option>'; sN3.innerHTML = '<option value="">-- Ninguno --</option>'; sFec.innerHTML = ''; sVal.innerHTML = ''; 
    cols.forEach(c => { sMaq.innerHTML += `<option value="${c}">${c}</option>`; sN2.innerHTML += `<option value="${c}">${c}</option>`; sN3.innerHTML += `<option value="${c}">${c}</option>`; sFec.innerHTML += `<option value="${c}">${c}</option>`; sVal.innerHTML += `<option value="${c}">${c}</option>`; });
    let maq = cols.find(c => c.toLowerCase().includes('maquina') || c.toLowerCase().includes('máquina') || c.toLowerCase() === 'línea de producción'); if (maq) sMaq.value = maq;
    let cod = cols.find(c => c.toLowerCase().includes('codigo') || c.toLowerCase().includes('código') || c.toLowerCase().includes('articulo')); if (cod) sN2.value = cod;
    let des = cols.find(c => c.toLowerCase().includes('descripci')); if (des) sN3.value = des;
    let fec = cols.find(c => c.toLowerCase().includes('fecha')); if (fec) sFec.value = fec;
    let val = cols.find(c => c.toLowerCase().includes('costo') || c.toLowerCase().includes('valor') || c.toLowerCase().includes('total')); if (val) sVal.value = val;
}

function actualizarListasFiltrosSecundarios() {
    function llenar(selId, baseObj) {
        if(baseObj.length===0) return; const c = new Set(); baseObj.forEach(f => Object.keys(f).forEach(k => c.add(k)));
        const sel = document.getElementById(selId); let val = sel.value; sel.innerHTML = '<option value="">-- Sin Filtro --</option>'; Array.from(c).sort().forEach(col => sel.innerHTML += `<option value="${col}">${col}</option>`);
        if (val && Array.from(c).includes(val)) sel.value = val;
    }
    llenar('datosMtoFiltroCol1', baseDeDatos); llenar('datosMtoFiltroCol2', baseDeDatos); llenar('datosMtoFiltroCol3', baseDeDatos);
    llenar('dashFiltroCol1', baseDeDatos); llenar('dashFiltroCol2', baseDeDatos); llenar('dashFiltroCol3', baseDeDatos);
    llenar('datosCostoFiltroCol1', baseDeCostos); llenar('datosCostoFiltroCol2', baseDeCostos); llenar('datosCostoFiltroCol3', baseDeCostos);
    llenar('costoFiltroCol1', baseDeCostos); llenar('costoFiltroCol2', baseDeCostos); llenar('costoFiltroCol3', baseDeCostos);
    llenar('olapFiltroCol1', baseDeDatos); llenar('olapFiltroCol2', baseDeDatos);
}
function actualizarValoresFiltrosDinamicos(colId, valId, baseObj, renderFunc) {
    const col = document.getElementById(colId).value; const selVal = document.getElementById(valId); selVal.innerHTML = '<option value="">-- Todos --</option>'; if (!col) return;
    const u = new Set(); baseObj.forEach(f => { let v = f[col]; if(v && String(v).trim()!=="") u.add(String(v).trim()); }); Array.from(u).sort().forEach(v => selVal.innerHTML += `<option value="${v}">${v}</option>`);
    renderFunc();
}

function cambiarVistaFiltro() {
    const t = document.getElementById('tipoFiltro').value;
    ['controlDia', 'controlMes', 'controlAnio', 'controlMesHistorico'].forEach(id => document.getElementById(id).style.display = 'none');
    if (t === 'dia') document.getElementById('controlDia').style.display = 'inline-block';
    if (t === 'mes') document.getElementById('controlMes').style.display = 'inline-block';
    if (t === 'anio') { document.getElementById('controlAnio').style.display = 'inline-block'; if(!document.getElementById('inputAnio').value) document.getElementById('inputAnio').value = new Date().getFullYear(); }
    if (t === 'mes_historico') document.getElementById('controlMesHistorico').style.display = 'inline-block';
    aplicarFiltro();
}
function moverFecha(t, d) {
    if (t === 'dia') { let i = document.getElementById('inputDia'); if (!i.value) i.value = new Date().toISOString().split('T')[0]; let f = new Date(i.value); f.setDate(f.getDate() + d); i.value = f.toISOString().split('T')[0]; } 
    else if (t === 'mes') { let i = document.getElementById('inputMes'); if (!i.value) i.value = new Date().toISOString().substring(0,7); let [a, m] = i.value.split('-'); let f = new Date(a, m - 1 + d, 1); i.value = f.getFullYear() + '-' + String(f.getMonth() + 1).padStart(2, '0'); } 
    else if (t === 'anio') { let i = document.getElementById('inputAnio'); i.value = parseInt(i.value) + d; }
    aplicarFiltro();
}

function aplicarFiltro() {
    const tipo = document.getElementById('tipoFiltro').value; 
    
    // Filtro Mantenimiento
    const colFecMto = document.getElementById('selectorColumnaFecha').value; 
    if (tipo === 'todo') { datosFiltrados = [...baseDeDatos]; } 
    else {
        datosFiltrados = baseDeDatos.filter(f => {
            let vF = f[colFecMto]; if (!vF) return false; let vs = String(vF); 
            if (tipo === 'dia') return vs === document.getElementById('inputDia').value;
            if (tipo === 'mes') return vs.startsWith(document.getElementById('inputMes').value);
            if (tipo === 'anio') return vs.startsWith(document.getElementById('inputAnio').value);
            if (tipo === 'mes_historico') return vs.substring(5, 7) === document.getElementById('inputMesHistorico').value;
            return false;
        });
    }
    
    // Filtro Costos
    const colFecCosto = document.getElementById('costoColFecha').value;
    if (tipo === 'todo') { costosFiltrados = [...baseDeCostos]; }
    else {
        costosFiltrados = baseDeCostos.filter(f => {
            let vF = f[colFecCosto]; if (!vF) return false; let vs = String(vF); 
            if (tipo === 'dia') return vs === document.getElementById('inputDia').value;
            if (tipo === 'mes') return vs.startsWith(document.getElementById('inputMes').value);
            if (tipo === 'anio') return vs.startsWith(document.getElementById('inputAnio').value);
            if (tipo === 'mes_historico') return vs.substring(5, 7) === document.getElementById('inputMesHistorico').value;
            return false;
        });
    }

    if (document.getElementById('panelDatos').style.display === 'block') renderizarTablaMto();
    if (document.getElementById('panelDatosCostos').style.display === 'block') renderizarTablaCostos();
    if (document.getElementById('panelDashboard').style.display === 'block') renderizarDashboard();
    if (document.getElementById('panelCostos').style.display === 'block') renderizarDashboardCostos();
    if (document.getElementById('panelJerarquia').style.display === 'block') renderizarJerarquia();
}

// --- TABLAS DE DATOS ---
function renderizarTablaMto() { const cab = document.getElementById('cabeceraTabla'); const cue = document.getElementById('cuerpoTabla'); cab.innerHTML = ''; cue.innerHTML = ''; if (datosFiltrados.length === 0) { document.getElementById('contadorMto').innerText="0"; return; } let db = [...datosFiltrados]; let busq = document.getElementById('busquedaMto').value.toLowerCase().trim(); if (busq) db = db.filter(f => Object.values(f).some(v => String(v).toLowerCase().includes(busq))); let c1 = document.getElementById('datosMtoFiltroCol1').value; let v1 = document.getElementById('datosMtoFiltroVal1').value; let c2 = document.getElementById('datosMtoFiltroCol2').value; let v2 = document.getElementById('datosMtoFiltroVal2').value; let c3 = document.getElementById('datosMtoFiltroCol3').value; let v3 = document.getElementById('datosMtoFiltroVal3').value; if (c1 && v1) db = db.filter(f => String(f[c1]).trim() === v1); if (c2 && v2) db = db.filter(f => String(f[c2]).trim() === v2); if (c3 && v3) db = db.filter(f => String(f[c3]).trim() === v3); document.getElementById('contadorMto').innerText = db.length; if(db.length===0)return; const conj = new Set(); db.forEach(f => Object.keys(f).forEach(k => conj.add(k))); const cols = Array.from(conj); let trH = '<tr>'; cols.forEach(c => trH += `<th style="padding:5px;">${c}</th>`); cab.innerHTML = trH + '</tr>'; const lim = Math.min(db.length, 500); let trB = ''; for (let i = 0; i < lim; i++) { let f = db[i]; trB += '<tr>'; cols.forEach(c => trB += `<td style="padding:4px;">${f[c] !== undefined ? f[c] : ''}</td>`); trB += '</tr>'; } cue.innerHTML = trB; }
function renderizarTablaCostos() { const cab = document.getElementById('cabeceraTablaCostos'); const cue = document.getElementById('cuerpoTablaCostosTabla'); cab.innerHTML = ''; cue.innerHTML = ''; if (costosFiltrados.length === 0) { document.getElementById('contadorCostosTabla').innerText="0"; return; } let db = [...costosFiltrados]; let busq = document.getElementById('busquedaCostos').value.toLowerCase().trim(); if (busq) db = db.filter(f => Object.values(f).some(v => String(v).toLowerCase().includes(busq))); let c1 = document.getElementById('datosCostoFiltroCol1').value; let v1 = document.getElementById('datosCostoFiltroVal1').value; let c2 = document.getElementById('datosCostoFiltroCol2').value; let v2 = document.getElementById('datosCostoFiltroVal2').value; let c3 = document.getElementById('datosCostoFiltroCol3').value; let v3 = document.getElementById('datosCostoFiltroVal3').value; if (c1 && v1) db = db.filter(f => String(f[c1]).trim() === v1); if (c2 && v2) db = db.filter(f => String(f[c2]).trim() === v2); if (c3 && v3) db = db.filter(f => String(f[c3]).trim() === v3); document.getElementById('contadorCostosTabla').innerText = db.length; if(db.length===0)return; const conj = new Set(); db.forEach(f => Object.keys(f).forEach(k => conj.add(k))); const cols = Array.from(conj); let trH = '<tr>'; cols.forEach(c => trH += `<th style="padding:5px;">${c}</th>`); cab.innerHTML = trH + '</tr>'; const lim = Math.min(db.length, 500); let trB = ''; for (let i = 0; i < lim; i++) { let f = db[i]; trB += '<tr>'; cols.forEach(c => trB += `<td style="padding:4px;">${f[c] !== undefined ? f[c] : ''}</td>`); trB += '</tr>'; } cue.innerHTML = trB; }
function calcularPercentil(arr, pct) { if(arr.length===0)return 0; const idx=(pct/100)*(arr.length-1); const b=Math.floor(idx); const a=Math.ceil(idx); return arr[b]+(idx-b)*(arr[a]-arr[b]); }

// --- DASHBOARDS ---
function renderizarDashboard() { const cont = document.getElementById('contenedorGrafico'); cont.innerHTML = ''; const col = document.getElementById('selectorColumnaDashboard').value; if (!col || datosFiltrados.length === 0) return; const c1 = document.getElementById('dashFiltroCol1').value; const v1 = document.getElementById('dashFiltroVal1').value; const c2 = document.getElementById('dashFiltroCol2').value; const v2 = document.getElementById('dashFiltroVal2').value; const c3 = document.getElementById('dashFiltroCol3').value; const v3 = document.getElementById('dashFiltroVal3').value; let db = datosFiltrados; if (c1 && v1) db = db.filter(f => String(f[c1]).trim() === v1); if (c2 && v2) db = db.filter(f => String(f[c2]).trim() === v2); if (c3 && v3) db = db.filter(f => String(f[c3]).trim() === v3); const conteo = {}; db.forEach(f => { let m = f[col]; if (m && String(m).trim() !== "") conteo[m] = (conteo[m] || 0) + 1; }); const dG = Object.keys(conteo).map(k => ({ maquina: k, fallos: conteo[k] })).sort((a, b) => b.fallos - a.fallos); if (dG.length === 0) { cont.innerHTML = '<p>No hay datos.</p>'; return;} const sF = dG.map(d => d.fallos).sort((a, b) => a - b); const q1 = Math.round(calcularPercentil(sF, 25)); const q2 = Math.round(calcularPercentil(sF, 50)); const q3 = Math.round(calcularPercentil(sF, 75)); document.getElementById('dashTotalEquipos').innerText = dG.length; document.getElementById('dashQ1').innerText = q1; document.getElementById('dashQ2').innerText = q2; document.getElementById('dashQ3').innerText = q3; const vM = dG[0].fallos; let h = '<table style="width:100%; border-collapse:collapse;">'; dG.forEach(i => { let p = (i.fallos / vM) * 100; let c = 'darkgreen'; if (i.fallos >= q3) c = 'darkred'; else if (i.fallos >= q2) c = 'darkorange'; else if (i.fallos > q1) c = 'olive'; h += `<tr><td style="width: 30%; padding: 5px; font-size: 13px; text-align: right; border-right: 1px solid #ccc;">${i.maquina}</td><td style="width: 70%; padding: 5px;"><div class="barra-grafico" style="width: ${p}%; background-color: ${c}; color: white;">${i.fallos}</div></td></tr>`; }); cont.innerHTML = h + '</table>'; }
function renderizarDashboardCostos() { const colMaq = document.getElementById('costoColMaquina').value; const colN2 = document.getElementById('costoColNivel2').value; const colN3 = document.getElementById('costoColNivel3').value; const colVal = document.getElementById('costoColValor').value; const cue = document.getElementById('cuerpoTablaCostosDash'); cue.innerHTML = ''; const c1 = document.getElementById('costoFiltroCol1').value; const v1 = document.getElementById('costoFiltroVal1').value; const c2 = document.getElementById('costoFiltroCol2').value; const v2 = document.getElementById('costoFiltroVal2').value; const c3 = document.getElementById('costoFiltroCol3').value; const v3 = document.getElementById('costoFiltroVal3').value; let db = costosFiltrados; if (c1 && v1) db = db.filter(f => String(f[c1]).trim() === v1); if (c2 && v2) db = db.filter(f => String(f[c2]).trim() === v2); if (c3 && v3) db = db.filter(f => String(f[c3]).trim() === v3); document.getElementById('contadorCostosDash').innerText = db.length; document.getElementById('costoTotalGlobal').innerText = "0.00"; if (!colMaq || !colVal || db.length === 0) { document.getElementById('costoQ1').innerText="0"; document.getElementById('costoQ2').innerText="0"; document.getElementById('costoQ3').innerText="0"; document.querySelector('#tablaDesgloseCostos thead').innerHTML=""; return; } let tot = 0; const agr = {}; let tc = obtenerTC(); let sim = obtenerSimboloMoneda(); document.getElementById('simboloMonedaGlobal').innerText = sim; document.querySelectorAll('.simCosto').forEach(el => el.innerText = sim); db.forEach(f => { let m1 = f[colMaq] ? String(f[colMaq]).trim() : ""; if (m1 !== "") { let m2 = colN2 && f[colN2] ? String(f[colN2]).trim() : ""; let m3 = colN3 && f[colN3] ? String(f[colN3]).trim() : ""; let key = m1 + "|||" + m2 + "|||" + m3; let v = parsearMoneda(f[colVal]) / tc; agr[key] = (agr[key] || 0) + v; tot += v; } }); document.getElementById('costoTotalGlobal').innerText = tot.toLocaleString('en-US', {minimumFractionDigits: 2, maximumFractionDigits: 2}); const dG = Object.keys(agr).map(k => ({ key: k, costo: agr[k] })).sort((a, b) => b.costo - a.costo); if (dG.length === 0) return; const sC = dG.map(d => d.costo).sort((a, b) => a - b); const q1 = calcularPercentil(sC, 25); const q2 = calcularPercentil(sC, 50); const q3 = calcularPercentil(sC, 75); document.getElementById('costoQ1').innerText = q1.toLocaleString('en-US', {maximumFractionDigits: 0}); document.getElementById('costoQ2').innerText = q2.toLocaleString('en-US', {maximumFractionDigits: 0}); document.getElementById('costoQ3').innerText = q3.toLocaleString('en-US', {maximumFractionDigits: 0}); let headH = `<tr><th style="padding: 8px; text-align: left;">${colMaq}</th>`; if(colN2) headH += `<th style="padding: 8px; text-align: left;">${colN2}</th>`; if(colN3) headH += `<th style="padding: 8px; text-align: left;">${colN3}</th>`; headH += `<th style="padding: 8px; text-align: right;">Costo Total</th><th style="padding: 8px; text-align: center;">% del Gasto</th></tr>`; document.querySelector('#tablaDesgloseCostos thead').innerHTML = headH; let h = ''; dG.forEach(i => { let parts = i.key.split('|||'); let pct = tot > 0 ? ((i.costo / tot) * 100).toFixed(1) : 0; let cBar = 'darkgreen'; if (i.costo >= q3) cBar = 'darkred'; else if (i.costo >= q2) cBar = 'darkorange'; else if (i.costo > q1) cBar = 'olive'; h += `<tr><td style="padding: 8px; font-weight:bold; color:#0050b3;">${parts[0]}</td>`; if(colN2) h += `<td style="padding: 8px; font-size:12px;">${parts[1]}</td>`; if(colN3) h += `<td style="padding: 8px; font-size:12px;">${parts[2]}</td>`; h += `<td style="padding: 8px; text-align: right; font-family: monospace; font-size:14px;">${sim} ${i.costo.toLocaleString('en-US', {minimumFractionDigits: 2, maximumFractionDigits: 2})}</td><td style="padding: 8px; text-align: center;"><div style="width:100%; background:#eee; border-radius:3px; overflow:hidden; position:relative;"><div style="width:${pct}%; background:${cBar}; height:18px;"></div><span style="position:absolute; top:1px; left:0; width:100%; font-size:11px; text-align:center; color: #fff; font-weight:bold; text-shadow: 1px 1px 2px #000;">${pct}%</span></div></td></tr>`; }); cue.innerHTML = h; }

// --- ÁRBOL JERÁRQUICO (OLAP) ---
function actualizarSelectoresArbol() {
    function llenar(selId, baseObj, fallbackStr) { if(baseObj.length === 0) return; const c = new Set(); baseObj.forEach(f => Object.keys(f).forEach(k => c.add(k))); const sel = document.getElementById(selId); let val = sel.value; sel.innerHTML = '<option value="">-- No usar este nivel --</option>'; Array.from(c).sort().forEach(col => sel.innerHTML += `<option value="${col}">${col}</option>`); if (val && Array.from(c).includes(val)) sel.value = val; else { let i = Array.from(c).find(x => x.toLowerCase().includes(fallbackStr)); if(i) sel.value = i; } }
    const metrica = document.getElementById('arbolMetrica').value; const baseObj = metrica === 'costo' ? baseDeCostos : baseDeDatos;
    llenar('arbolColL1', baseObj, 'línea'); llenar('arbolColL2', baseObj, 'maquina'); llenar('arbolColL3', baseObj, 'seccion'); llenar('arbolColL4', baseObj, 'aplicacion'); llenar('arbolColCosto', baseDeCostos, 'costo'); llenar('arbolColTM', baseDeDatos, 'tiempo muerto'); llenar('arbolColHRep', baseDeDatos, 'reporte'); llenar('arbolColHIni', baseDeDatos, 'inicio'); llenar('arbolColTecnico', baseDeDatos, 'tecnico'); llenar('arbolColTurno', baseDeDatos, 'turno'); renderizarBotoneraArbol();
}
function renderizarBotoneraArbol() {
    const metrica = document.getElementById('arbolMetrica').value; const colL1 = document.getElementById('arbolColL1').value; const bot = document.getElementById('arbolBotoneraLineas'); bot.innerHTML = '';
    let baseObj = metrica === 'costo' ? costosFiltrados : datosFiltrados;
    let busq = document.getElementById('busquedaOlap').value.toLowerCase().trim(); if (busq) baseObj = baseObj.filter(f => Object.values(f).some(v => String(v).toLowerCase().includes(busq)));
    let c1 = document.getElementById('olapFiltroCol1').value; let v1 = document.getElementById('olapFiltroVal1').value; let c2 = document.getElementById('olapFiltroCol2').value; let v2 = document.getElementById('olapFiltroVal2').value;
    if (c1 && v1) baseObj = baseObj.filter(f => String(f[c1]).trim() === v1); if (c2 && v2) baseObj = baseObj.filter(f => String(f[c2]).trim() === v2);

    if(!colL1 || baseObj.length === 0) { bot.innerHTML = '<span style="color:#999; font-size:12px;">Selecciona la columna "Línea" y revisa los filtros.</span>'; document.getElementById('arbolTotalGlobal').innerText="0"; return; }
    
    let totalGlobal = 0; let suffix = "";
    if(metrica === 'frecuencia') { totalGlobal = baseObj.length; suffix = "Fallos Totales"; }
    else if(metrica === 'costo') { let colCos = document.getElementById('arbolColCosto').value; let tc = obtenerTC(); if(colCos) baseObj.forEach(f => { totalGlobal += (parsearMoneda(f[colCos])/tc); }); suffix = obtenerSimboloMoneda(); totalGlobal = totalGlobal.toLocaleString('en-US',{minimumFractionDigits:2, maximumFractionDigits:2}); }
    else if(metrica === 'tiempo_muerto') { let colTM = document.getElementById('arbolColTM').value; if(colTM) baseObj.forEach(f => { let v = f[colTM]; if(v) { let nv = parseFloat(String(v).replace(/[^0-9\.]/g, '')); if(!isNaN(nv)) totalGlobal += nv; } }); suffix = "Hrs de Paro"; totalGlobal = totalGlobal.toFixed(1); }
    else if(metrica === 'tiempo_atencion') { let cHRep = document.getElementById('arbolColHRep').value; let cHIni = document.getElementById('arbolColHIni').value; if(cHRep && cHIni) baseObj.forEach(f => { let hr = f[cHRep]; let hi = f[cHIni]; if(hr && hi) totalGlobal += calcularDiferenciaHoras(hr, hi); }); suffix = "Hrs Reacción"; totalGlobal = totalGlobal.toFixed(1); }
    
    document.getElementById('arbolTotalGlobal').innerText = totalGlobal; document.getElementById('arbolSuffixGlobal').innerText = suffix;

    const lineas = new Set(); baseObj.forEach(f => { let v = f[colL1]; if(v && String(v).trim()!=="") lineas.add(String(v).trim()); });
    Array.from(lineas).sort().forEach(linea => { let btn = document.createElement('button'); btn.className = 'btn-linea' + (arbolLineaSeleccionada === linea ? ' activa' : ''); btn.innerText = linea; btn.onclick = () => { arbolLineaSeleccionada = linea; renderizarBotoneraArbol(); renderizarJerarquia(); }; bot.appendChild(btn); });
    if(!Array.from(lineas).includes(arbolLineaSeleccionada)) { arbolLineaSeleccionada = ""; document.getElementById('arbolContenedor').innerHTML = '<p style="color:#64748b; font-style:italic;">Selecciona una línea arriba.</p>'; }
}
function renderizarJerarquia() {
    const contenedor = document.getElementById('arbolContenedor'); const metrica = document.getElementById('arbolMetrica').value; 
    let baseObj = metrica === 'costo' ? costosFiltrados : datosFiltrados;
    let busq = document.getElementById('busquedaOlap').value.toLowerCase().trim(); if (busq) baseObj = baseObj.filter(f => Object.values(f).some(v => String(v).toLowerCase().includes(busq)));
    let c1 = document.getElementById('olapFiltroCol1').value; let v1 = document.getElementById('olapFiltroVal1').value; let c2 = document.getElementById('olapFiltroCol2').value; let v2 = document.getElementById('olapFiltroVal2').value;
    if (c1 && v1) baseObj = baseObj.filter(f => String(f[c1]).trim() === v1); if (c2 && v2) baseObj = baseObj.filter(f => String(f[c2]).trim() === v2);

    if(baseObj.length === 0) { contenedor.innerHTML = '<p style="color:#999;">No hay datos.</p>'; return; }
    if(!arbolLineaSeleccionada) { contenedor.innerHTML = '<p style="color:#999;">Selecciona una línea.</p>'; return; }

    const cL1 = document.getElementById('arbolColL1').value; const cL2 = document.getElementById('arbolColL2').value; const cL3 = document.getElementById('arbolColL3').value; const cL4 = document.getElementById('arbolColL4').value;
    let dbLinea = baseObj.filter(f => String(f[cL1]).trim() === arbolLineaSeleccionada);

    let arbol = {};
    dbLinea.forEach(f => {
        let m = cL2 && f[cL2] ? String(f[cL2]).trim() : 'Máquina Única'; let s = cL3 && f[cL3] ? String(f[cL3]).trim() : 'Sección Única'; let a = cL4 && f[cL4] ? String(f[cL4]).trim() : 'Aplicación General';
        if(!arbol[m]) arbol[m] = {}; if(!arbol[m][s]) arbol[m][s] = {}; if(!arbol[m][s][a]) arbol[m][s][a] = []; arbol[m][s][a].push(f);
    });

    let html = '';
    Object.keys(arbol).sort().forEach(maq => {
        let filasMaq = []; Object.keys(arbol[maq]).forEach(sec => { Object.keys(arbol[maq][sec]).forEach(app => { filasMaq = filasMaq.concat(arbol[maq][sec][app]); }); });
        html += `<div class="caja-anidada nivel-maquina"><div class="caja-header" onclick="this.nextElementSibling.style.display = this.nextElementSibling.style.display==='none' ? 'block' : 'none'"><div class="tree-title-row"><span>⚙️ Máquina: <span style="color:#0ea5e9;">${maq}</span></span><div><span style="font-weight:normal; font-size:12px; margin-right:10px;">(${filasMaq.length} registros)</span> <span class="indicador-exp">🔽 Expandir</span></div></div>${generarHtmlStats(filasMaq, metrica)}</div><div class="caja-body" style="display:none;">`;
        Object.keys(arbol[maq]).sort().forEach(sec => {
            let filasSec = []; Object.keys(arbol[maq][sec]).forEach(app => { filasSec = filasSec.concat(arbol[maq][sec][app]); });
            html += `<div class="caja-anidada nivel-seccion"><div class="caja-header" onclick="this.nextElementSibling.style.display = this.nextElementSibling.style.display==='none' ? 'block' : 'none'"><div class="tree-title-row"><span>🔧 Sección: <span style="color:#d97706;">${sec}</span></span><div><span style="font-weight:normal; font-size:12px; margin-right:10px;">(${filasSec.length} registros)</span> <span class="indicador-exp">🔽 Expandir</span></div></div>${generarHtmlStats(filasSec, metrica)}</div><div class="caja-body" style="display:none;">`;
            Object.keys(arbol[maq][sec]).sort().forEach(app => {
                let filasApp = arbol[maq][sec][app];
                html += `<div class="caja-anidada nivel-aplicacion"><div class="caja-header"><div class="tree-title-row"><span>🎯 Aplicación: <span style="color:#16a34a;">${app}</span></span><span style="font-weight:normal; font-size:12px;">(${filasApp.length} registros)</span></div>${generarHtmlStats(filasApp, metrica)}</div></div>`;
            }); html += `</div></div>`; 
        }); html += `</div></div>`; 
    }); contenedor.innerHTML = html;
}

function generarHtmlStats(filas, metrica) {
    if(filas.length === 0) return ''; let dataArray = []; let prefix = ""; let suffix = ""; let isTiempo = false; let sumaTotal = 0;
    if(metrica === 'frecuencia') { let colFecha = document.getElementById('selectorColumnaFecha').value; let pM = {}; filas.forEach(f => { let d = f[colFecha]; if(d) { let m = String(d).substring(0,7); pM[m] = (pM[m] || 0) + 1; } }); dataArray = Object.values(pM).sort((a,b)=>a-b); if(dataArray.length===0) dataArray=[0]; suffix = " fallos/mes"; sumaTotal = filas.length; }
    else if (metrica === 'costo') { let colCos = document.getElementById('arbolColCosto').value; let tc = obtenerTC(); prefix = obtenerSimboloMoneda() + " "; filas.forEach(f => { let v = f[colCos]; if(v) { let money = parsearMoneda(v) / tc; dataArray.push(money); sumaTotal += money; } }); dataArray.sort((a,b)=>a-b); }
    else if (metrica === 'tiempo_muerto') { let colTM = document.getElementById('arbolColTM').value; isTiempo = true; suffix = " Hrs"; filas.forEach(f => { let v = f[colTM]; if(v) { let nv = parseFloat(String(v).replace(/[^0-9\.]/g, '')); if(!isNaN(nv)) { dataArray.push(nv); sumaTotal += nv; } } }); dataArray.sort((a,b)=>a-b); }
    else if (metrica === 'tiempo_atencion') { let cHRep = document.getElementById('arbolColHRep').value; let cHIni = document.getElementById('arbolColHIni').value; isTiempo = true; suffix = " Hrs"; filas.forEach(f => { let hr = f[cHRep]; let hi = f[cHIni]; if(hr && hi) { let diff = calcularDiferenciaHoras(hr, hi); dataArray.push(diff); sumaTotal += diff; } }); dataArray.sort((a,b)=>a-b); }

    if(dataArray.length === 0) dataArray = [0];
    const q0 = dataArray[0]; const q4 = dataArray[dataArray.length-1]; const q1 = calcularPercentil(dataArray, 25); const q2 = calcularPercentil(dataArray, 50); const q3 = calcularPercentil(dataArray, 75); const avg = dataArray.reduce((a,b)=>a+b, 0) / dataArray.length;
    let formatNum = (n) => { return isTiempo ? n.toFixed(1) : (metrica==='costo' ? n.toLocaleString('en-US',{minimumFractionDigits:2, maximumFractionDigits:2}) : Math.round(n)); };
    let formatTotal = (n) => { return metrica==='frecuencia' ? n : (metrica==='costo' ? n.toLocaleString('en-US',{minimumFractionDigits:2, maximumFractionDigits:2}) : n.toFixed(1)); };

    let htmlStats = `<div class="tree-stats-row"><span class="stats-badge stats-badge-avg" style="background:#e0f2fe; border-color:#3b82f6; color:#1e40af;"><b>TOTAL:</b> ${prefix}${formatTotal(sumaTotal)}${metrica==='frecuencia'?' Fallos':suffix}</span><span class="stats-badge stats-badge-q" title="Mínimo">Min: ${prefix}${formatNum(q0)}${suffix}</span><span class="stats-badge stats-badge-q" title="Cuartil 1 (25%)">Q1: ${prefix}${formatNum(q1)}</span><span class="stats-badge stats-badge-q" title="Mediana (50%)">Mediana: ${prefix}${formatNum(q2)}</span><span class="stats-badge stats-badge-q" title="Cuartil 3 (75%)">Q3: ${prefix}${formatNum(q3)}</span><span class="stats-badge stats-badge-q" title="Máximo">Max: ${prefix}${formatNum(q4)}</span><span class="stats-badge stats-badge-avg" title="Promedio">Prom: ${prefix}${formatNum(avg)}${suffix}</span></div>`;

    if(metrica !== 'costo') {
        const colTec = document.getElementById('arbolColTecnico').value; const colTur = document.getElementById('arbolColTurno').value; let cTec={}; let cTur={};
        filas.forEach(f => { let t = colTec && f[colTec] ? String(f[colTec]).trim() : null; if(t) cTec[t] = (cTec[t] || 0) + 1; let u = colTur && f[colTur] ? String(f[colTur]).trim() : null; if(u) cTur[u] = (cTur[u] || 0) + 1; });
        let topTec = Object.keys(cTec).sort((a,b)=>cTec[b]-cTec[a]).slice(0,3).map(k => `${k}(${cTec[k]})`).join(', ') || '-'; let topTur = Object.keys(cTur).sort((a,b)=>cTur[b]-cTur[a]).map(k => `T${k}(${cTur[k]})`).join(', ') || '-';
        htmlStats += `<div class="tree-extras"><span>👨‍🔧 <b>Top Técnicos:</b> ${topTec}</span><span>⏰ <b>Turnos:</b> ${topTur}</span></div>`;
    }
    return htmlStats;
}

// --- CRITICIDAD ---
function actualizarSelectoresConexionCriticidad() { const sB = document.getElementById('critColMto'); const sC = document.getElementById('critColBodega'); const sV = document.getElementById('critColValorBodega'); sB.innerHTML = '<option value="">-- Mto --</option>'; sC.innerHTML = '<option value="">-- Bodega --</option>'; sV.innerHTML = '<option value="">-- Costo Base --</option>'; if (baseDeDatos.length > 0) { let cM = new Set(); baseDeDatos.forEach(f => Object.keys(f).forEach(k => cM.add(k))); Array.from(cM).sort().forEach(c => sB.innerHTML += `<option value="${c}">${c}</option>`); let mId = Array.from(cM).find(c => c.toLowerCase() === 'línea de producción' || c.toLowerCase().includes('maquina') || c.toLowerCase().includes('máquina')); if(mId) sB.value = mId; } if (baseDeCostos.length > 0) { let cC = new Set(); baseDeCostos.forEach(f => Object.keys(f).forEach(k => cC.add(k))); Array.from(cC).sort().forEach(c => { sC.innerHTML += `<option value="${c}">${c}</option>`; sV.innerHTML += `<option value="${c}">${c}</option>`; }); let mId2 = Array.from(cC).find(c => c.toLowerCase() === 'línea de producción' || c.toLowerCase().includes('maquina') || c.toLowerCase().includes('máquina')); if(mId2) sC.value = mId2; let vId = Array.from(cC).find(c => c.toLowerCase().includes('costo') || c.toLowerCase().includes('valor')); if(vId) sV.value = vId; } llenarEquiposCriticidad(); }
function llenarEquiposCriticidad() { const colMto = document.getElementById('critColMto').value; const colCos = document.getElementById('critColBodega').value; const sE = document.getElementById('critEquipoSelect'); sE.innerHTML = '<option value="">-- Seleccionar Equipo --</option>'; const unicas = new Set(); if(colMto && baseDeDatos.length>0) baseDeDatos.forEach(f => { let m = f[colMto]; if(m) unicas.add(String(m).trim()); }); if(colCos && baseDeCostos.length>0) baseDeCostos.forEach(f => { let m = f[colCos]; if(m) unicas.add(String(m).trim()); }); Array.from(unicas).sort().forEach(m => { sE.innerHTML += `<option value="${m}">${m}</option>`; }); }
function sincronizarFrecYCostos() { const equipo = document.getElementById('critEquipoSelect').value; const periodo = document.getElementById('critPeriodo').value; const metodo = document.getElementById('critMetodoCalculo').value; const info = document.getElementById('critInfoAuto'); if(!equipo) { info.innerText = "Selecciona máquina."; return; } document.getElementById('critEquipo').value = equipo; const colMto = document.getElementById('critColMto').value; const colFecMto = document.getElementById('selectorColumnaFecha').value; let freq = 0; let mtoText = "0 fallos"; if(colMto && colFecMto && baseDeDatos.length>0) { let fallos = baseDeDatos.filter(f => String(f[colMto]).trim() === equipo); let perU = {}; fallos.forEach(f => { let fec = f[colFecMto]; if(fec) { let fs = String(fec); let pKey = ""; if(periodo === 'dia') pKey = fs; else if(periodo === 'mes') pKey = fs.substring(0,7); else if(periodo === 'anio') pKey = fs.substring(0,4); else if(periodo === 'semana') { let d = new Date(fs+"T12:00:00"); pKey = d.getFullYear()+"-"+Math.floor(d.getTime()/604800000); } if(pKey) perU[pKey] = (perU[pKey] || 0) + 1; } }); let arrF = Object.values(perU).sort((a,b)=>a-b); if(arrF.length === 0) arrF = [0]; if (metodo === 'promedio') freq = fallos.length / Math.max(1, arrF.length); else if (metodo === 'q0') freq = arrF[0]; else if (metodo === 'q1') freq = calcularPercentil(arrF, 25); else if (metodo === 'q2') freq = calcularPercentil(arrF, 50); else if (metodo === 'q3') freq = calcularPercentil(arrF, 75); else if (metodo === 'q4') freq = arrF[arrF.length-1]; mtoText = `${freq.toFixed(1)}/per`; let sFrec = document.getElementById('critFrec'); if (freq <= 1.5) sFrec.value = "1"; else if (freq > 1.5 && freq <= 4.5) sFrec.value = "2"; else if (freq > 4.5 && freq <= 7.5) sFrec.value = "3"; else sFrec.value = "4"; } const colCos = document.getElementById('critColBodega').value; const colValCos = document.getElementById('critColValorBodega').value; const colFecCos = document.getElementById('costoColFecha').value; let cost = 0; let cosText = "0"; let tc = obtenerTC(); let sim = obtenerSimboloMoneda(); if(colCos && colValCos && colFecCos && baseDeCostos.length>0) { let registros = baseDeCostos.filter(f => String(f[colCos]).trim() === equipo); let perUC = {}; registros.forEach(f => { let fec = f[colFecCos]; let valorDinero = (parsearMoneda(f[colValCos]) / tc); if(fec) { let fs = String(fec); let pKey = ""; if(periodo === 'dia') pKey = fs; else if(periodo === 'mes') pKey = fs.substring(0,7); else if(periodo === 'anio') pKey = fs.substring(0,4); else if(periodo === 'semana') { let d = new Date(fs+"T12:00:00"); pKey = d.getFullYear()+"-"+Math.floor(d.getTime()/604800000); } if(pKey) perUC[pKey] = (perUC[pKey] || 0) + valorDinero; } }); let arrC = Object.values(perUC).sort((a,b)=>a-b); if(arrC.length === 0) arrC = [0]; if (metodo === 'promedio') { let sumC = arrC.reduce((a,b)=>a+b,0); cost = sumC / Math.max(1, arrC.length); } else if (metodo === 'q0') cost = arrC[0]; else if (metodo === 'q1') cost = calcularPercentil(arrC, 25); else if (metodo === 'q2') cost = calcularPercentil(arrC, 50); else if (metodo === 'q3') cost = calcularPercentil(arrC, 75); else if (metodo === 'q4') cost = arrC[arrC.length-1]; cosText = `${sim} ${cost.toFixed(0)}/per`; let sCosto = document.getElementById('critCosto'); if (cost <= 5000) sCosto.value = "1"; else if (cost > 5000 && cost <= 10000) sCosto.value = "2"; else sCosto.value = "3"; } let lblM = document.getElementById('critMetodoCalculo').options[document.getElementById('critMetodoCalculo').selectedIndex].text; info.innerText = `Por ${periodo} (${lblM}): Frec: ${mtoText} | Gasto: ${cosText}.`; actualizarPreviewCriticidad(); }
function calcularFormulaCriticidad(frec, imp, flex, costo, seg) { const consecuencia = (imp * flex) + costo + seg; const criticidad = frec * consecuencia; let nivel = ''; let color = ''; let badgeClase = ''; if (criticidad <= 40) { nivel = 'BAJO'; color = '#1e40af'; badgeClase = 'badge-bajo'; } else if (criticidad > 40 && criticidad <= 100) { nivel = 'MEDIO'; color = '#166534'; badgeClase = 'badge-medio'; } else { nivel = 'ALTO'; color = '#991b1b'; badgeClase = 'badge-alto'; } return { total: criticidad, nivel, color, badgeClase }; }
function actualizarPreviewCriticidad() { const c = calcularFormulaCriticidad(parseInt(document.getElementById('critFrec').value), parseInt(document.getElementById('critImp').value), parseInt(document.getElementById('critFlex').value), parseInt(document.getElementById('critCosto').value), parseInt(document.getElementById('critSeguridad').value)); const p = document.getElementById('previewCriticidad'); document.getElementById('previewPuntos').innerText = c.total; document.getElementById('previewNivel').innerText = c.nivel; document.getElementById('previewPuntos').style.color = c.color; document.getElementById('previewNivel').style.color = c.color; if(c.nivel === 'BAJO') { p.style.background = '#dbeafe'; p.style.borderColor = '#93c5fd'; } else if(c.nivel === 'MEDIO') { p.style.background = '#dcfce3'; p.style.borderColor = '#86efac'; } else { p.style.background = '#fee2e2'; p.style.borderColor = '#fca5a5'; } }
function guardarActivoCriticidad() { const eq = document.getElementById('critEquipo').value.trim(); if (!eq) return alert("Ingresa equipo."); const c = calcularFormulaCriticidad(parseInt(document.getElementById('critFrec').value), parseInt(document.getElementById('critImp').value), parseInt(document.getElementById('critFlex').value), parseInt(document.getElementById('critCosto').value), parseInt(document.getElementById('critSeguridad').value)); listaCriticidad.push({ id: new Date().getTime(), equipo: eq, frec: document.getElementById('critFrec').value, imp: document.getElementById('critImp').value, flex: document.getElementById('critFlex').value, costo: document.getElementById('critCosto').value, seg: document.getElementById('critSeguridad').value, total: c.total, nivel: c.nivel, badgeClase: c.badgeClase }); guardarEstado('bd_criticidad', listaCriticidad); document.getElementById('critEquipo').value = ''; document.getElementById('critInfoAuto').innerText = ''; renderizarTablaCriticidad(); }
function eliminarActivoCriticidad(id) { listaCriticidad = listaCriticidad.filter(i => i.id !== id); guardarEstado('bd_criticidad', listaCriticidad); renderizarTablaCriticidad(); }
function renderizarTablaCriticidad() { const tb = document.getElementById('cuerpoTablaCriticidad'); tb.innerHTML = ''; if (listaCriticidad.length === 0) { tb.innerHTML = '<tr><td colspan="9" style="padding: 20px; color: #999;">No hay activos.</td></tr>'; return; } listaCriticidad.forEach(a => { tb.innerHTML += `<tr style="border-bottom: 1px solid #eee;"><td style="padding:8px;text-align:left;font-weight:bold;">${a.equipo}</td><td style="padding:8px;">${a.frec}</td><td style="padding:8px;">${a.imp}</td><td style="padding:8px;">${a.flex}</td><td style="padding:8px;">${a.costo}</td><td style="padding:8px;">${a.seg}</td><td style="padding:8px;font-weight:bold;">${a.total}</td><td style="padding:8px;"><span class="badge ${a.badgeClase}">${a.nivel}</span></td><td style="padding:8px;"><button onclick="eliminarActivoCriticidad(${a.id})" style="background:none;border:none;color:red;cursor:pointer;font-size:16px;">🗑️</button></td></tr>`; }); }

// --- EXPORTAR Y BORRAR ---
function descargarArchivo(cont, nom, t) { const blob = new Blob([cont], { type: t + ';charset=utf-8;' }); const link = document.createElement("a"); link.href = URL.createObjectURL(blob); link.download = nom; link.click(); URL.revokeObjectURL(link.href); }
function exportarCSVMantenimiento() { if (baseDeDatos.length === 0) return alert("Sin datos."); let expFilt = confirm("¿Exportar SOLO los datos visibles (filtrados)?\n\n[Aceptar] = Solo lo filtrado\n[Cancelar] = Exportar TODA la base completa"); let datosExp = expFilt ? datosFiltrados : baseDeDatos; if (datosExp.length === 0) return alert("No hay datos en la selección."); let cols = Array.from(new Set(datosExp.flatMap(Object.keys))); descargarArchivo(Papa.unparse(datosExp, { columns: cols }), `mantenimiento_${expFilt ? 'filtrado' : 'completo'}.csv`, 'text/csv'); }
function limpiarDatosMantenimiento() { if(!confirm("¿Borrar Mantenimiento?")) return; baseDeDatos = []; guardarEstado('bd_industrial', []); actualizarSelectoresGlobales(); actualizarSelectoresConexionCriticidad(); actualizarListasFiltrosSecundarios(); aplicarFiltro(); }
function exportarCSVCostos() { if (baseDeCostos.length === 0) return alert("Sin datos."); let expFilt = confirm("¿Exportar SOLO los datos visibles (filtrados)?\n\n[Aceptar] = Solo lo filtrado\n[Cancelar] = Exportar TODA la base completa"); let datosExp = expFilt ? costosFiltrados : baseDeCostos; if (datosExp.length === 0) return alert("No hay datos en la selección."); let cols = Array.from(new Set(datosExp.flatMap(Object.keys))); descargarArchivo(Papa.unparse(datosExp, { columns: cols }), `costos_bodega_${expFilt ? 'filtrado' : 'completo'}.csv`, 'text/csv'); }
function limpiarDatosCostos() { if(!confirm("¿Borrar Costos?")) return; baseDeCostos = []; guardarEstado('bd_costos', []); actualizarSelectoresCostos(); actualizarSelectoresConexionCriticidad(); actualizarListasFiltrosSecundarios(); aplicarFiltro(); }

arrancarSistema();
