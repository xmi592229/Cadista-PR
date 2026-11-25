<!DOCTYPE html>
<html lang="es">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0" />
  <title>ESCUELA CADISTA P.R. — Plantilla y Convocatoria</title>
  <script src="https://cdn.tailwindcss.com"></script>
  <style>
    :root {
      --cadiz-yellow: #FFD100;
      --cadiz-yellow-dark: #E5BA00;
      --cadiz-blue: #00338D;
      --cadiz-blue-dark: #002261;
    }
    body {
      font-family: system-ui, -apple-system, BlinkMacSystemFont, "Segoe UI", Roboto, sans-serif;
      background-color: #f4f6fa;
      color: #0f172a;
    }
    .bg-cadiz-yellow { background-color: var(--cadiz-yellow); }
    .bg-cadiz-blue { background-color: var(--cadiz-blue); }
    .bg-cadiz-blue-dark { background-color: var(--cadiz-blue-dark); }
    .text-cadiz-blue { color: var(--cadiz-blue); }
    .text-cadiz-yellow { color: var(--cadiz-yellow); }
    .border-cadiz-blue { border-color: var(--cadiz-blue); }
    .border-cadiz-yellow { border-color: var(--cadiz-yellow); }

    @media print {
      .no-print { display: none !important; }
      body { background: #ffffff !important; }
      .print-card {
        box-shadow: none !important;
        border: 2px solid #00338D !important;
      }
    }
  </style>
</head>
<body class="min-h-screen flex flex-col">

  <!-- CABECERA OFICIAL -->
  <header class="bg-cadiz-blue text-white border-b-4 border-cadiz-yellow shadow-md no-print">
    <div class="max-w-6xl mx-auto px-4 py-3 flex flex-wrap items-center justify-between gap-3">
      <div class="flex items-center gap-3 min-w-0">
        <!-- Escudo superior izquierda (SVG oficial + opción de subir imagen personalizada) -->
        <label for="crestUpload" title="Toca para cambiar el escudo por una imagen" class="relative shrink-0 cursor-pointer group">
          <div id="crestContainer" class="w-14 h-14 rounded-full bg-cadiz-yellow border-2 border-white shadow flex items-center justify-center overflow-hidden">
            <svg id="defaultCrest" viewBox="0 0 100 100" class="w-13 h-13">
              <path d="M50 6 L88 20 L82 62 C78 80 63 91 50 95 C37 91 22 80 18 62 L12 20 Z" fill="#FFD100" stroke="#00338D" stroke-width="5"/>
              <path d="M50 13 L81 24 L76 60 C72 75 61 84 50 88 C39 84 28 75 24 60 L19 24 Z" fill="#00338D"/>
              <path d="M27 32 L73 68 M73 32 L27 68" stroke="#FFD100" stroke-width="7" stroke-linecap="round"/>
              <circle cx="50" cy="50" r="14" fill="#FFD100" stroke="#00338D" stroke-width="3"/>
              <text x="50" y="54" text-anchor="middle" fill="#00338D" font-size="11" font-weight="900">PR</text>
            </svg>
            <img id="customCrestImg" src="" alt="Escudo Escuela Cadista P.R." class="hidden w-full h-full object-cover" />
          </div>
          <span class="sr-only">Cambiar escudo</span>
          <input id="crestUpload" type="file" accept="image/*" class="hidden" />
        </label>

        <div class="min-w-0">
          <h1 class="text-xl sm:text-2xl font-black tracking-wide text-cadiz-yellow uppercase leading-tight truncate">
            ESCUELA CADISTA P.R.
          </h1>
          <p class="text-xs sm:text-sm text-blue-100">
            Gestión de plantilla, lesiones y convocatoria
          </p>
        </div>
      </div>

      <!-- Indicador de sala compartida -->
      <div class="flex items-center gap-2 bg-cadiz-blue-dark/90 border border-cadiz-yellow/60 rounded-lg px-3 py-1.5 text-xs">
        <span id="syncDot" class="w-2.5 h-2.5 rounded-full bg-emerald-400 inline-block"></span>
        <span class="text-blue-100">Sala:</span>
        <strong id="roomBadge" class="text-cadiz-yellow font-mono">escuela-cadista-pr</strong>
        <button id="changeRoomBtn" type="button" class="ml-1 underline text-white hover:text-cadiz-yellow font-medium">
          Cambiar
        </button>
      </div>
    </div>

    <!-- PESTAÑAS PRINCIPALES -->
    <div class="max-w-6xl mx-auto px-4 flex gap-2 pt-1">
      <button
        id="tabPlayersBtn"
        type="button"
        class="px-5 py-2.5 rounded-t-lg font-bold text-sm sm:text-base transition cursor-pointer bg-cadiz-yellow text-cadiz-blue"
      >
        Jugadores (<span id="playersCountTab">0</span>)
      </button>
      <button
        id="tabSquadBtn"
        type="button"
        class="px-5 py-2.5 rounded-t-lg font-bold text-sm sm:text-base transition cursor-pointer bg-cadiz-blue-dark text-blue-100 hover:text-cadiz-yellow"
      >
        Convocados (<span id="squadCountTab">0</span>)
      </button>
    </div>
  </header>

  <!-- CONTENIDO PRINCIPAL -->
  <main class="flex-1 max-w-6xl w-full mx-auto px-4 py-6">

    <!-- PESTAÑA 1: JUGADORES (Layout fijo sin superposiciones) -->
    <section id="playersView" class="block">
      <div class="grid grid-cols-1 lg:grid-cols-[380px_minmax(0,1fr)] gap-6 items-start">

        <!-- Formulario de alta / edición -->
        <div class="bg-white rounded-xl border-2 border-cadiz-blue/20 shadow-sm p-5 min-w-0">
          <div class="flex items-center justify-between mb-4 pb-2 border-b border-slate-200">
            <h2 id="formTitle" class="text-lg font-extrabold text-cadiz-blue">
              Añadir jugador
            </h2>
            <button
              id="cancelEditBtn"
              type="button"
              class="hidden text-xs font-semibold text-slate-600 hover:text-cadiz-blue underline"
            >
              Cancelar edición
            </button>
          </div>

          <form id="playerForm" class="space-y-4">
            <input type="hidden" id="editingPlayerId" />

            <div>
              <label for="firstName" class="block text-sm font-bold text-slate-700 mb-1">
                Nombre *
              </label>
              <input
                id="firstName"
                type="text"
                required
                placeholder="Ej. Carlos"
                class="w-full rounded-lg border border-slate-300 px-3 py-2 text-sm focus:outline-none focus:border-cadiz-blue focus:ring-2 focus:ring-cadiz-yellow"
              />
            </div>

            <div>
              <label for="lastName" class="block text-sm font-bold text-slate-700 mb-1">
                Apellidos *
              </label>
              <input
                id="lastName"
                type="text"
                required
                placeholder="Ej. Fernández Gómez"
                class="w-full rounded-lg border border-slate-300 px-3 py-2 text-sm focus:outline-none focus:border-cadiz-blue focus:ring-2 focus:ring-cadiz-yellow"
              />
            </div>

            <div>
              <label for="positionSelect" class="block text-sm font-bold text-slate-700 mb-1">
                Posición *
              </label>
              <select
                id="positionSelect"
                class="w-full rounded-lg border border-slate-300 px-3 py-2 text-sm bg-white focus:outline-none focus:border-cadiz-blue focus:ring-2 focus:ring-cadiz-yellow"
              >
                <option value="Portero">Portero</option>
                <option value="Defensa">Defensa</option>
                <option value="Centrocampista">Centrocampista</option>
                <option value="Delantero">Delantero</option>
                <option value="Otra">Otra posición...</option>
              </select>
            </div>

            <div id="customPositionWrap" class="hidden">
              <label for="customPosition" class="block text-sm font-bold text-slate-700 mb-1">
                Especificar posición
              </label>
              <input
                id="customPosition"
                type="text"
                placeholder="Ej. Carrilero / Extremo"
                class="w-full rounded-lg border border-slate-300 px-3 py-2 text-sm focus:outline-none focus:border-cadiz-blue focus:ring-2 focus:ring-cadiz-yellow"
              />
            </div>

            <div>
              <span class="block text-sm font-bold text-slate-700 mb-1.5">
                Estado físico *
              </span>
              <div class="grid grid-cols-2 gap-2">
                <label class="flex items-center justify-center gap-2 rounded-lg border-2 border-emerald-500 bg-emerald-50 text-emerald-900 font-bold text-sm py-2 px-3 cursor-pointer has-[:checked]:ring-2 has-[:checked]:ring-emerald-600">
                  <input type="radio" name="injuryStatus" value="disponible" checked class="accent-emerald-600" />
                  <span>Disponible</span>
                </label>
                <label class="flex items-center justify-center gap-2 rounded-lg border-2 border-rose-500 bg-rose-50 text-rose-900 font-bold text-sm py-2 px-3 cursor-pointer has-[:checked]:ring-2 has-[:checked]:ring-rose-600">
                  <input type="radio" name="injuryStatus" value="lesionado" class="accent-rose-600" />
                  <span>Lesionado</span>
                </label>
              </div>
            </div>

            <button
              id="submitPlayerBtn"
              type="submit"
              class="w-full py-2.5 px-4 rounded-lg bg-cadiz-blue hover:bg-cadiz-blue-dark text-cadiz-yellow font-extrabold text-sm uppercase tracking-wide shadow transition cursor-pointer"
            >
              Guardar en plantilla
            </button>
          </form>
        </div>

        <!-- Listado de plantilla -->
        <div class="bg-white rounded-xl border-2 border-cadiz-blue/20 shadow-sm p-5 min-w-0">
          <div class="flex flex-wrap items-center justify-between gap-3 mb-4 pb-3 border-b border-slate-200">
            <div>
              <h2 class="text-lg font-extrabold text-cadiz-blue">Plantilla del equipo</h2>
              <p id="rosterSummary" class="text-xs text-slate-500">0 jugadores registrados</p>
            </div>

            <div class="flex flex-wrap items-center gap-2">
              <input
                id="searchPlayer"
                type="text"
                placeholder="Buscar jugador..."
                class="rounded-lg border border-slate-300 px-3 py-1.5 text-sm focus:outline-none focus:border-cadiz-blue"
              />
              <select
                id="filterStatus"
                class="rounded-lg border border-slate-300 px-3 py-1.5 text-sm bg-white focus:outline-none focus:border-cadiz-blue"
              >
                <option value="todos">Todos los estados</option>
                <option value="disponible">Solo disponibles</option>
                <option value="lesionado">Solo lesionados</option>
              </select>
            </div>
          </div>

          <div class="overflow-x-auto">
            <table class="w-full text-left border-collapse min-w-[540px]">
              <thead>
                <tr class="bg-cadiz-blue text-cadiz-yellow text-xs uppercase tracking-wider">
                  <th class="py-2.5 px-3 rounded-tl-lg">Nombre y apellidos</th>
                  <th class="py-2.5 px-3">Posición</th>
                  <th class="py-2.5 px-3">Estado</th>
                  <th class="py-2.5 px-3 text-right rounded-tr-lg">Acciones</th>
                </tr>
              </thead>
              <tbody id="playersTableBody" class="divide-y divide-slate-200 text-sm"></tbody>
            </table>
          </div>

          <div id="emptyRosterMsg" class="hidden text-center py-10 text-slate-500">
            <p class="font-semibold text-slate-700">No hay jugadores en la plantilla todavía.</p>
            <p class="text-xs mt-1">Usa el formulario de la izquierda para añadir al primer jugador.</p>
          </div>
        </div>

      </div>
    </section>

    <!-- PESTAÑA 2: CONVOCADOS -->
    <section id="squadView" class="hidden space-y-6">

      <!-- Datos de la convocatoria: Campo y Hora -->
      <div class="bg-white rounded-xl border-2 border-cadiz-blue/20 shadow-sm p-5 no-print">
        <h2 class="text-lg font-extrabold text-cadiz-blue mb-3">Datos de la convocatoria</h2>
        <div class="grid grid-cols-1 sm:grid-cols-3 gap-4">
          <div>
            <label for="matchField" class="block text-sm font-bold text-slate-700 mb-1">
              Campo / Instalación
            </label>
            <input
              id="matchField"
              type="text"
              placeholder="Ej. Ciudad Deportiva / Campo Municipal"
              class="w-full rounded-lg border border-slate-300 px-3 py-2 text-sm focus:outline-none focus:border-cadiz-blue focus:ring-2 focus:ring-cadiz-yellow"
            />
          </div>

          <div>
            <label for="callTime" class="block text-sm font-bold text-slate-700 mb-1">
              Hora de convocatoria
            </label>
            <input
              id="callTime"
              type="time"
              class="w-full rounded-lg border border-slate-300 px-3 py-2 text-sm focus:outline-none focus:border-cadiz-blue focus:ring-2 focus:ring-cadiz-yellow"
            />
          </div>

          <div>
            <label for="opponentName" class="block text-sm font-bold text-slate-700 mb-1">
              Partido / Rival (opcional)
            </label>
            <input
              id="opponentName"
              type="text"
              placeholder="Ej. vs CD Puerto Real"
              class="w-full rounded-lg border border-slate-300 px-3 py-2 text-sm focus:outline-none focus:border-cadiz-blue focus:ring-2 focus:ring-cadiz-yellow"
            />
          </div>
        </div>
      </div>

      <div class="grid grid-cols-1 lg:grid-cols-[minmax(0,1fr)_minmax(0,1.15fr)] gap-6 items-start">

        <!-- Selector de jugadores disponibles -->
        <div class="bg-white rounded-xl border-2 border-cadiz-blue/20 shadow-sm p-5 no-print">
          <div class="flex items-center justify-between gap-2 mb-3 pb-2 border-b border-slate-200">
            <div>
              <h2 class="text-lg font-extrabold text-cadiz-blue">Seleccionar jugadores</h2>
              <p class="text-xs text-slate-500">Marca o pulsa para incluir en la convocatoria</p>
            </div>
            <div class="flex gap-2">
              <button
                id="selectAllAvailableBtn"
                type="button"
                class="text-xs font-bold px-2.5 py-1.5 rounded bg-cadiz-yellow text-cadiz-blue hover:brightness-95 cursor-pointer"
              >
                Todos aptos
              </button>
              <button
                id="clearSquadBtn"
                type="button"
                class="text-xs font-bold px-2.5 py-1.5 rounded bg-slate-100 text-slate-700 hover:bg-slate-200 cursor-pointer"
              >
                Limpiar
              </button>
            </div>
          </div>

          <div id="selectablePlayersList" class="space-y-2 max-h-[480px] overflow-y-auto pr-1"></div>
        </div>

        <!-- Lista final de convocados -->
        <div class="bg-white rounded-xl border-2 border-cadiz-blue shadow-md overflow-hidden print-card">
          <div class="bg-cadiz-blue text-white px-5 py-4 border-b-4 border-cadiz-yellow flex items-center justify-between gap-3">
            <div>
              <p class="text-[11px] uppercase tracking-widest text-cadiz-yellow font-bold">
                Lista oficial de convocados
              </p>
              <h2 class="text-xl font-black tracking-wide uppercase">
                ESCUELA CADISTA P.R.
              </h2>
              <p id="matchSubtitle" class="text-xs text-blue-100 mt-0.5"></p>
            </div>
            <div class="bg-cadiz-yellow text-cadiz-blue rounded-lg px-3 py-2 text-center shrink-0">
              <div id="finalSquadCount" class="text-xl font-black leading-none">0</div>
              <div class="text-[10px] font-extrabold uppercase mt-0.5">Convocados</div>
            </div>
          </div>

          <div class="p-5 space-y-4">
            <!-- Resumen de Campo y Hora -->
            <div class="grid grid-cols-1 sm:grid-cols-2 gap-3 bg-amber-50 border border-cadiz-yellow rounded-lg p-3 text-sm">
              <div>
                <span class="block text-[11px] font-bold uppercase text-slate-500">Campo</span>
                <span id="summaryFieldText" class="font-extrabold text-cadiz-blue">Por confirmar</span>
              </div>
              <div>
                <span class="block text-[11px] font-bold uppercase text-slate-500">Hora de convocatoria</span>
                <span id="summaryTimeText" class="font-extrabold text-cadiz-blue">Por confirmar</span>
              </div>
            </div>

            <!-- Listado agrupado -->
            <div id="finalSquadContainer" class="space-y-3"></div>

            <div id="emptySquadMsg" class="text-center py-8 text-slate-500 border border-dashed border-slate-300 rounded-lg">
              <p class="font-semibold text-slate-700">Aún no has seleccionado convocados.</p>
              <p class="text-xs mt-1">Elige los jugadores disponibles en el panel de la izquierda.</p>
            </div>

            <!-- Botones de acción -->
            <div class="pt-3 border-t border-slate-200 flex flex-wrap gap-2 no-print">
              <button
                id="copyWhatsappBtn"
                type="button"
                class="flex-1 min-w-[160px] py-2.5 px-4 rounded-lg bg-emerald-600 hover:bg-emerald-700 text-white font-bold text-sm shadow transition cursor-pointer"
              >
                Copiar para WhatsApp
              </button>
              <button
                id="printSquadBtn"
                type="button"
                class="py-2.5 px-4 rounded-lg bg-cadiz-blue hover:bg-cadiz-blue-dark text-cadiz-yellow font-bold text-sm transition cursor-pointer"
              >
                Imprimir / Guardar PDF
              </button>
            </div>

            <p id="copyFeedback" class="text-xs font-semibold text-emerald-700 h-4 no-print"></p>
          </div>
        </div>

      </div>
    </section>

  </main>

  <!-- PIE DE PÁGINA CON SINCRONIZACIÓN COMPARTIDA -->
  <footer class="bg-cadiz-blue text-blue-100 text-xs py-3 px-4 border-t border-cadiz-yellow/40 no-print">
    <div class="max-w-6xl mx-auto flex flex-wrap items-center justify-between gap-2">
      <span>ESCUELA CADISTA P.R. • Trabajo sincronizado entre miembros de la sala</span>
      <span id="lastSyncText" class="text-cadiz-yellow font-mono">Sincronizado</span>
    </div>
  </footer>

  <script>
    const DEFAULT_ROOM = "escuela-cadista-pr";
    let currentRoom = localStorage.getItem("cadista_pr_room") || DEFAULT_ROOM;

    let state = {
      players: [],
      squadIds: [],
      matchField: "",
      callTime: "",
      opponentName: "",
      customCrest: "",
      updatedAt: 0
    };

    let channel = null;

    function storageKey(room) {
      return `cadista_pr_state_${room}`;
    }

    function loadInitialState() {
      const saved = localStorage.getItem(storageKey(currentRoom));
      if (saved) {
        try {
          const parsed = JSON.parse(saved);
          state = { ...state, ...parsed };
        } catch (e) {
          console.error("Error leyendo estado guardado:", e);
        }
      }
      setupSyncChannel();
      renderAll();
    }

    function setupSyncChannel() {
      if (channel) {
        channel.close();
      }
      if ("BroadcastChannel" in window) {
        channel = new BroadcastChannel(`cadista_pr_sync_${currentRoom}`);
        channel.onmessage = (event) => {
          if (event.data && event.data.type === "STATE_UPDATE") {
            state = { ...state, ...event.data.payload };
            localStorage.setItem(storageKey(currentRoom), JSON.stringify(state));
            markSynced();
            renderAll(true);
          }
        };
      }
      document.getElementById("roomBadge").textContent = currentRoom;
    }

    function saveAndBroadcast() {
      state.updatedAt = Date.now();
      localStorage.setItem(storageKey(currentRoom), JSON.stringify(state));
      if (channel) {
        channel.postMessage({ type: "STATE_UPDATE", payload: state });
      }
      markSynced();
    }

    function markSynced() {
      const now = new Date();
      const timeStr = now.toLocaleTimeString("es-ES", { hour: "2-digit", minute: "2-digit", second: "2-digit" });
      document.getElementById("lastSyncText").textContent = `Actualizado ${timeStr}`;
    }

    window.addEventListener("storage", (e) => {
      if (e.key === storageKey(currentRoom) && e.newValue) {
        try {
          state = { ...state, ...JSON.parse(e.newValue) };
          markSynced();
          renderAll(true);
        } catch (err) {}
      }
    });

    // Pestañas
    const tabPlayersBtn = document.getElementById("tabPlayersBtn");
    const tabSquadBtn = document.getElementById("tabSquadBtn");
    const playersView = document.getElementById("playersView");
    const squadView = document.getElementById("squadView");

    function switchTab(tab) {
      if (tab === "players") {
        playersView.classList.remove("hidden");
        squadView.classList.add("hidden");
        tabPlayersBtn.className = "px-5 py-2.5 rounded-t-lg font-bold text-sm sm:text-base transition cursor-pointer bg-cadiz-yellow text-cadiz-blue";
        tabSquadBtn.className = "px-5 py-2.5 rounded-t-lg font-bold text-sm sm:text-base transition cursor-pointer bg-cadiz-blue-dark text-blue-100 hover:text-cadiz-yellow";
      } else {
        squadView.classList.remove("hidden");
        playersView.classList.add("hidden");
        tabSquadBtn.className = "px-5 py-2.5 rounded-t-lg font-bold text-sm sm:text-base transition cursor-pointer bg-cadiz-yellow text-cadiz-blue";
        tabPlayersBtn.className = "px-5 py-2.5 rounded-t-lg font-bold text-sm sm:text-base transition cursor-pointer bg-cadiz-blue-dark text-blue-100 hover:text-cadiz-yellow";
      }
    }

    tabPlayersBtn.addEventListener("click", () => switchTab("players"));
    tabSquadBtn.addEventListener("click", () => switchTab("squad"));

    // Escudo personalizado
    document.getElementById("crestUpload").addEventListener("change", (e) => {
      const file = e.target.files[0];
      if (!file) return;
      const reader = new FileReader();
      reader.onload = () => {
        state.customCrest = reader.result;
        saveAndBroadcast();
        renderCrest();
      };
      reader.readAsDataURL(file);
    });

    function renderCrest() {
      const img = document.getElementById("customCrestImg");
      const svg = document.getElementById("defaultCrest");
      if (state.customCrest) {
        img.src = state.customCrest;
        img.classList.remove("hidden");
        svg.classList.add("hidden");
      } else {
        img.classList.add("hidden");
        svg.classList.remove("hidden");
      }
    }

    // Cambiar sala
    document.getElementById("changeRoomBtn").addEventListener("click", () => {
      const input = prompt("Código de sala compartida (debe ser igual para todos los miembros):", currentRoom);
      if (input !== null) {
        const clean = input.trim().toLowerCase().replace(/[^a-z0-9-_]/g, "-") || DEFAULT_ROOM;
        currentRoom = clean;
        localStorage.setItem("cadista_pr_room", currentRoom);
        loadInitialState();
      }
    });

    // Formulario de jugadores
    const playerForm = document.getElementById("playerForm");
    const positionSelect = document.getElementById("positionSelect");
    const customPositionWrap = document.getElementById("customPositionWrap");
    const customPositionInput = document.getElementById("customPosition");

    positionSelect.addEventListener("change", () => {
      customPositionWrap.classList.toggle("hidden", positionSelect.value !== "Otra");
    });

    function resetPlayerForm() {
      document.getElementById("editingPlayerId").value = "";
      document.getElementById("formTitle").textContent = "Añadir jugador";
      document.getElementById("submitPlayerBtn").textContent = "Guardar en plantilla";
      document.getElementById("cancelEditBtn").classList.add("hidden");
      playerForm.reset();
      customPositionWrap.classList.add("hidden");
    }

    document.getElementById("cancelEditBtn").addEventListener("click", resetPlayerForm);

    playerForm.addEventListener("submit", (e) => {
      e.preventDefault();
      const firstName = document.getElementById("firstName").value.trim();
      const lastName = document.getElementById("lastName").value.trim();
      if (!firstName || !lastName) return;

      let position = positionSelect.value;
      if (position === "Otra") {
        position = customPositionInput.value.trim() || "Otra posición";
      }

      const injuryStatus = document.querySelector('input[name="injuryStatus"]:checked').value;
      const editingId = document.getElementById("editingPlayerId").value;

      if (editingId) {
        const idx = state.players.findIndex((p) => p.id === editingId);
        if (idx !== -1) {
          state.players[idx] = {
            ...state.players[idx],
            firstName,
            lastName,
            position,
            injuryStatus
          };
          if (injuryStatus === "lesionado") {
            state.squadIds = state.squadIds.filter((id) => id !== editingId);
          }
        }
      } else {
        state.players.push({
          id: "p_" + Date.now() + "_" + Math.random().toString(36).slice(2, 6),
          firstName,
          lastName,
          position,
          injuryStatus
        });
      }

      saveAndBroadcast();
      resetPlayerForm();
      renderAll();
    });

    function toggleInjury(id) {
      const player = state.players.find((p) => p.id === id);
      if (!player) return;
      player.injuryStatus = player.injuryStatus === "disponible" ? "lesionado" : "disponible";
      if (player.injuryStatus === "lesionado") {
        state.squadIds = state.squadIds.filter((sid) => sid !== id);
      }
      saveAndBroadcast();
      renderAll();
    }

    function startEditPlayer(id) {
      const player = state.players.find((p) => p.id === id);
      if (!player) return;
      document.getElementById("editingPlayerId").value = player.id;
      document.getElementById("firstName").value = player.firstName;
      document.getElementById("lastName").value = player.lastName;

      const standardPos = ["Portero", "Defensa", "Centrocampista", "Delantero"];
      if (standardPos.includes(player.position)) {
        positionSelect.value = player.position;
        customPositionWrap.classList.add("hidden");
        customPositionInput.value = "";
      } else {
        positionSelect.value = "Otra";
        customPositionWrap.classList.remove("hidden");
        customPositionInput.value = player.position;
      }

      document.querySelector(`input[name="injuryStatus"][value="${player.injuryStatus}"]`).checked = true;
      document.getElementById("formTitle").textContent = "Editar jugador";
      document.getElementById("submitPlayerBtn").textContent = "Actualizar jugador";
      document.getElementById("cancelEditBtn").classList.remove("hidden");
      switchTab("players");
    }

    function deletePlayer(id) {
      state.players = state.players.filter((p) => p.id !== id);
      state.squadIds = state.squadIds.filter((sid) => sid !== id);
      saveAndBroadcast();
      renderAll();
    }

    // Filtros plantilla
    document.getElementById("searchPlayer").addEventListener("input", () => renderRosterTable());
    document.getElementById("filterStatus").addEventListener("change", () => renderRosterTable());

    function renderRosterTable() {
      const tbody = document.getElementById("playersTableBody");
      const emptyMsg = document.getElementById("emptyRosterMsg");
      const query = document.getElementById("searchPlayer").value.trim().toLowerCase();
      const statusFilter = document.getElementById("filterStatus").value;

      const availableCount = state.players.filter((p) => p.injuryStatus === "disponible").length;
      const injuredCount = state.players.filter((p) => p.injuryStatus === "lesionado").length;

      document.getElementById("rosterSummary").textContent =
        `${state.players.length} jugadores • ${availableCount} disponibles • ${injuredCount} lesionados`;
      document.getElementById("playersCountTab").textContent = state.players.length;

      const filtered = state.players.filter((p) => {
        const fullName = `${p.firstName} ${p.lastName} ${p.position}`.toLowerCase();
        const matchesQuery = !query || fullName.includes(query);
        const matchesStatus = statusFilter === "todos" || p.injuryStatus === statusFilter;
        return matchesQuery && matchesStatus;
      });

      tbody.innerHTML = "";
      if (state.players.length === 0) {
        emptyMsg.classList.remove("hidden");
        return;
      }
      emptyMsg.classList.add("hidden");

      if (filtered.length === 0) {
        tbody.innerHTML = `
          <tr>
            <td colspan="4" class="py-6 text-center text-slate-500">
              No hay jugadores que coincidan con el filtro.
            </td>
          </tr>`;
        return;
      }

      filtered.forEach((p) => {
        const isInjured = p.injuryStatus === "lesionado";
        const tr = document.createElement("tr");
        tr.className = "hover:bg-slate-50";

        tr.innerHTML = `
          <td class="py-3 px-3 font-bold text-slate-900">
            ${escapeHtml(p.firstName)} ${escapeHtml(p.lastName)}
          </td>
          <td class="py-3 px-3">
            <span class="inline-block bg-blue-50 text-cadiz-blue border border-cadiz-blue/20 font-semibold text-xs px-2.5 py-1 rounded-full">
              ${escapeHtml(p.position)}
            </span>
          </td>
          <td class="py-3 px-3">
            <button
              type="button"
              onclick="toggleInjury('${p.id}')"
              title="Pulsa para cambiar estado"
              class="inline-flex items-center gap-1.5 text-xs font-bold px-2.5 py-1 rounded-full cursor-pointer transition ${
                isInjured
                  ? "bg-rose-100 text-rose-800 border border-rose-300 hover:bg-rose-200"
                  : "bg-emerald-100 text-emerald-800 border border-emerald-300 hover:bg-emerald-200"
              }"
            >
              <span class="w-2 h-2 rounded-full ${isInjured ? "bg-rose-600" : "bg-emerald-600"}"></span>
              ${isInjured ? "Lesionado" : "Disponible"}
            </button>
          </td>
          <td class="py-3 px-3 text-right whitespace-nowrap space-x-1">
            <button
              type="button"
              onclick="startEditPlayer('${p.id}')"
              class="text-xs font-bold px-2.5 py-1 rounded bg-slate-100 hover:bg-slate-200 text-slate-800 cursor-pointer"
            >
              Editar
            </button>
            <button
              type="button"
              onclick="deletePlayer('${p.id}')"
              class="text-xs font-bold px-2.5 py-1 rounded bg-rose-50 hover:bg-rose-100 text-rose-700 cursor-pointer"
            >
              Eliminar
            </button>
          </td>
        `;
        tbody.appendChild(tr);
      });
    }

    // Datos de convocatoria (Campo y Hora)
    const matchFieldInput = document.getElementById("matchField");
    const callTimeInput = document.getElementById("callTime");
    const opponentNameInput = document.getElementById("opponentName");

    function syncMatchInputsFromState() {
      if (document.activeElement !== matchFieldInput) {
        matchFieldInput.value = state.matchField || "";
      }
      if (document.activeElement !== callTimeInput) {
        callTimeInput.value = state.callTime || "";
      }
      if (document.activeElement !== opponentNameInput) {
        opponentNameInput.value = state.opponentName || "";
      }
    }

    matchFieldInput.addEventListener("input", () => {
      state.matchField = matchFieldInput.value;
      saveAndBroadcast();
      renderFinalSquadSummary();
    });

    callTimeInput.addEventListener("input", () => {
      state.callTime = callTimeInput.value;
      saveAndBroadcast();
      renderFinalSquadSummary();
    });

    opponentNameInput.addEventListener("input", () => {
      state.opponentName = opponentNameInput.value;
      saveAndBroadcast();
      renderFinalSquadSummary();
    });

    function toggleSquadSelection(id) {
      const player = state.players.find((p) => p.id === id);
      if (!player || player.injuryStatus === "lesionado") return;

      if (state.squadIds.includes(id)) {
        state.squadIds = state.squadIds.filter((sid) => sid !== id);
      } else {
        state.squadIds.push(id);
      }
      saveAndBroadcast();
      renderSquadView();
    }

    document.getElementById("selectAllAvailableBtn").addEventListener("click", () => {
      state.squadIds = state.players
        .filter((p) => p.injuryStatus === "disponible")
        .map((p) => p.id);
      saveAndBroadcast();
      renderSquadView();
    });

    document.getElementById("clearSquadBtn").addEventListener("click", () => {
      state.squadIds = [];
      saveAndBroadcast();
      renderSquadView();
    });

    function renderSquadView() {
      const listEl = document.getElementById("selectablePlayersList");
      listEl.innerHTML = "";

      if (state.players.length === 0) {
        listEl.innerHTML = `<p class="text-sm text-slate-500 py-4 text-center">Añade jugadores primero en la pestaña "Jugadores".</p>`;
      } else {
        state.players.forEach((p) => {
          const isInjured = p.injuryStatus === "lesionado";
          const isSelected = !isInjured && state.squadIds.includes(p.id);

          const row = document.createElement("button");
          row.type = "button";
          row.disabled = isInjured;
          row.onclick = () => toggleSquadSelection(p.id);
          row.className = `w-full flex items-center justify-between gap-3 p-3 rounded-lg border text-left transition ${
            isInjured
              ? "bg-slate-50 border-slate-200 opacity-55 cursor-not-allowed"
              : isSelected
              ? "bg-amber-50 border-cadiz-blue ring-1 ring-cadiz-blue cursor-pointer"
              : "bg-white border-slate-200 hover:border-cadiz-blue/60 cursor-pointer"
          }`;

          row.innerHTML = `
            <div class="flex items-center gap-3 min-w-0">
              <span class="w-5 h-5 rounded flex items-center justify-center text-xs font-black shrink-0 ${
                isInjured
                  ? "bg-rose-100 text-rose-700"
                  : isSelected
                  ? "bg-cadiz-blue text-cadiz-yellow"
                  : "border border-slate-300 text-transparent"
              }">
                ${isInjured ? "✕" : "✓"}
              </span>
              <div class="min-w-0">
                <div class="font-bold text-sm text-slate-900 truncate">
                  ${escapeHtml(p.firstName)} ${escapeHtml(p.lastName)}
                </div>
                <div class="text-xs text-slate-500">${escapeHtml(p.position)}</div>
              </div>
            </div>
            <span class="text-[11px] font-bold px-2 py-0.5 rounded-full shrink-0 ${
              isInjured
                ? "bg-rose-100 text-rose-700"
                : isSelected
                ? "bg-cadiz-yellow text-cadiz-blue"
                : "bg-slate-100 text-slate-600"
            }">
              ${isInjured ? "Lesionado" : isSelected ? "Convocado" : "Disponible"}
            </span>
          `;
          listEl.appendChild(row);
        });
      }

      renderFinalSquadSummary();
    }

    function renderFinalSquadSummary() {
      const validIds = state.squadIds.filter((id) => {
        const p = state.players.find((pl) => pl.id === id);
        return p && p.injuryStatus === "disponible";
      });
      if (validIds.length !== state.squadIds.length) {
        state.squadIds = validIds;
      }

      const selectedPlayers = state.players.filter((p) => state.squadIds.includes(p.id));
      document.getElementById("squadCountTab").textContent = selectedPlayers.length;
      document.getElementById("finalSquadCount").textContent = selectedPlayers.length;

      const fieldText = state.matchField && state.matchField.trim() ? state.matchField.trim() : "Por confirmar";
      const timeText = state.callTime && state.callTime.trim() ? state.callTime.trim() : "Por confirmar";
      const oppText = state.opponentName && state.opponentName.trim() ? state.opponentName.trim() : "";

      document.getElementById("summaryFieldText").textContent = fieldText;
      document.getElementById("summaryTimeText").textContent = timeText;
      document.getElementById("matchSubtitle").textContent = oppText ? oppText : "Convocatoria oficial del equipo";

      const container = document.getElementById("finalSquadContainer");
      const emptySquadMsg = document.getElementById("emptySquadMsg");
      container.innerHTML = "";

      if (selectedPlayers.length === 0) {
        emptySquadMsg.classList.remove("hidden");
        return;
      }
      emptySquadMsg.classList.add("hidden");

      const grouped = {};
      selectedPlayers.forEach((p) => {
        const pos = p.position || "Sin posición";
        if (!grouped[pos]) grouped[pos] = [];
        grouped[pos].push(p);
      });

      Object.entries(grouped).forEach(([pos, list]) => {
        const block = document.createElement("div");
        block.className = "border border-slate-200 rounded-lg p-3";
        block.innerHTML = `
          <div class="text-xs font-black uppercase tracking-wider text-cadiz-blue mb-2 flex items-center justify-between">
            <span>${escapeHtml(pos)}</span>
            <span class="bg-slate-100 text-slate-700 px-2 py-0.5 rounded-full">${list.length}</span>
          </div>
          <ol class="list-decimal list-inside space-y-1 text-sm font-semibold text-slate-800">
            ${list.map((pl) => `<li>${escapeHtml(pl.firstName)} ${escapeHtml(pl.lastName)}</li>`).join("")}
          </ol>
        `;
        container.appendChild(block);
      });
    }

    // Copiar para WhatsApp
    document.getElementById("copyWhatsappBtn").addEventListener("click", async () => {
      const selectedPlayers = state.players.filter((p) => state.squadIds.includes(p.id));
      const lines = [
        "⚽ *ESCUELA CADISTA P.R.*",
        "📋 *CONVOCATORIA OFICIAL*"
      ];
      if (state.opponentName && state.opponentName.trim()) {
        lines.push(`🆚 ${state.opponentName.trim()}`);
      }
      lines.push(`📍 *Campo:* ${state.matchField && state.matchField.trim() ? state.matchField.trim() : "Por confirmar"}`);
      lines.push(`⏰ *Hora de convocatoria:* ${state.callTime && state.callTime.trim() ? state.callTime.trim() : "Por confirmar"}`);
      lines.push("");

      if (selectedPlayers.length === 0) {
        lines.push("_Sin jugadores seleccionados todavía._");
      } else {
        selectedPlayers.forEach((p, i) => {
          lines.push(`${i + 1}. ${p.firstName} ${p.lastName} (${p.position})`);
        });
      }

      const text = lines.join("
");
      const feedback = document.getElementById("copyFeedback");
      try {
        await navigator.clipboard.writeText(text);
        feedback.textContent = "Convocatoria copiada al portapapeles para pegar en WhatsApp.";
      } catch (e) {
        feedback.textContent = "No se pudo copiar automáticamente; puedes seleccionar el texto en pantalla.";
      }
      setTimeout(() => {
        feedback.textContent = "";
      }, 3500);
    });

    document.getElementById("printSquadBtn").addEventListener("click", () => {
      window.print();
    });

    function escapeHtml(str) {
      return String(str)
        .replace(/&/g, "&amp;")
        .replace(/</g, "&lt;")
        .replace(/>/g, "&gt;")
        .replace(/"/g, "&quot;");
    }

    function renderAll(fromRemote = false) {
      renderCrest();
      renderRosterTable();
      if (!fromRemote) {
        syncMatchInputsFromState();
      } else {
        syncMatchInputsFromState();
      }
      renderSquadView();
    }

    loadInitialState();
  </script>
</body>
</html>
