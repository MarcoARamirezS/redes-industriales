# Práctica Integradora — Industrial Network Decision Lab

## Tema III: Redes Industriales

**Asignatura:** Control de Procesos Industriales a través de la Web  
**Tecnologías:** HTML5 + Tailwind CSS CDN + JavaScript ES Modules  
**Frameworks:** Ninguno  
**Backend:** No requerido  
**Publicación sugerida:** GitHub Pages  
**Duración sugerida:** 1 sesión de 90 minutos + trabajo independiente opcional  
**Modalidad:** Individual o parejas

---

## 1. Propósito

Construir una aplicación web que permita aplicar los conceptos estudiados en el Tema III:

- PROFIBUS: maestro/esclavo, direcciones, terminación y diagnóstico.
- WORLDFIP: productor, variable y consumidores.
- CAN: identificadores y arbitraje por prioridad.
- DeviceNet: CAN aplicado a dispositivos industriales.
- Integración conceptual: sensor → bus → PLC/gateway → API → dashboard web.

> La aplicación es una simulación didáctica; no sustituye software ni hardware industrial real.

---

## 2. Caso de estudio

Una planta piloto contiene:

- 1 PLC.
- 1 HMI.
- 1 variador.
- 1 bomba.
- 1 válvula.
- sensores de nivel, presión, temperatura y velocidad.
- señal de paro de emergencia.
- dashboard web.

El objetivo es seleccionar, configurar y comparar redes industriales y mostrar variables de proceso en una interfaz web.

---

## 3. Producto final

La aplicación tendrá cinco módulos:

1. Comparador PROFIBUS / WORLDFIP / CAN / DeviceNet.
2. Configuración PROFIBUS.
3. Mapa productor–consumidor WORLDFIP.
4. Simulador de prioridad CAN.
5. Dashboard industrial.

---

## 4. Estructura del repositorio

```text
industrial-network-lab/
│
├── index.html
├── README.md
├── .gitignore
└── src/
    ├── css/
    │   └── styles.css
    └── js/
        ├── app.js
        ├── data.js
        ├── profibus.js
        ├── worldfip.js
        ├── can.js
        └── dashboard.js
```

---

## 5. Crear el proyecto

### macOS / Linux

```bash
mkdir -p industrial-network-lab/src/css
mkdir -p industrial-network-lab/src/js
cd industrial-network-lab

touch index.html README.md .gitignore
touch src/css/styles.css
touch src/js/app.js src/js/data.js src/js/profibus.js src/js/worldfip.js src/js/can.js src/js/dashboard.js
```

### Windows PowerShell

```powershell
mkdir industrial-network-lab
cd industrial-network-lab

mkdir src
mkdir src\css
mkdir src\js

New-Item index.html
New-Item README.md
New-Item .gitignore
New-Item src\css\styles.css
New-Item src\js\app.js
New-Item src\js\data.js
New-Item src\js\profibus.js
New-Item src\js\worldfip.js
New-Item src\js\can.js
New-Item src\js\dashboard.js
```

---

## 6. Git y GitHub

Inicializa Git:

```bash
git init
git add .
git commit -m "chore: initialize industrial network lab"
```

Después crea un repositorio vacío en GitHub y ejecuta:

```bash
git remote add origin URL_DEL_REPOSITORIO
git branch -M main
git push -u origin main
```

### Commits recomendados

```text
chore: initialize industrial network lab
feat: add industrial network comparison
feat: add profibus validation
feat: add worldfip variable model
feat: add can priority simulator
feat: add industrial dashboard
docs: complete student report
```

No realices toda la práctica en un solo commit.

---

## 7. Archivo `.gitignore`

```gitignore
.DS_Store
Thumbs.db
.vscode/
```

---

## 8. Archivo `index.html`

```html
<!doctype html>
<html lang="es">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Industrial Network Decision Lab</title>

  <script src="https://cdn.tailwindcss.com"></script>
  <link rel="stylesheet" href="./src/css/styles.css">
</head>

<body class="bg-slate-100 text-slate-900">

  <header class="bg-slate-950 text-white">
    <div class="max-w-7xl mx-auto px-4 py-8">
      <p class="text-cyan-300 font-semibold">Tema III · Redes Industriales</p>
      <h1 class="text-3xl md:text-4xl font-bold mt-1">
        Industrial Network Decision Lab
      </h1>
      <p class="text-slate-300 mt-3">
        PROFIBUS · WORLDFIP · CAN · DeviceNet · Dashboard Web
      </p>
    </div>
  </header>

  <main class="max-w-7xl mx-auto px-4 py-8 space-y-8">

    <section class="card">
      <h2 class="section-title"><span class="step">1</span>Caso industrial</h2>

      <div class="grid md:grid-cols-5 gap-3 mt-5">
        <div class="device"><strong>PLC</strong><span>Control</span></div>
        <div class="device"><strong>VFD</strong><span>Motor</span></div>
        <div class="device"><strong>Sensores</strong><span>Proceso</span></div>
        <div class="device"><strong>HMI</strong><span>Operación</span></div>
        <div class="device"><strong>WEB</strong><span>Monitoreo</span></div>
      </div>
    </section>

    <section class="card">
      <h2 class="section-title"><span class="step">2</span>Comparador de redes</h2>
      <p class="mt-4">Selecciona una red y analiza sus características.</p>

      <div id="network-options" class="grid md:grid-cols-4 gap-4 mt-5"></div>
      <div id="network-detail" class="hidden mt-6"></div>
    </section>

    <section class="card">
      <h2 class="section-title"><span class="step">3</span>Configuración PROFIBUS</h2>
      <p class="mt-4">
        Configura direcciones y terminaciones de un segmento PROFIBUS-DP.
      </p>

      <div id="profibus-nodes"
           class="grid md:grid-cols-4 lg:grid-cols-7 gap-3 mt-5"></div>

      <button id="validate-profibus" class="btn-primary mt-5">
        Validar PROFIBUS
      </button>

      <div id="profibus-feedback" class="feedback hidden mt-4"></div>
    </section>

    <section class="card">
      <h2 class="section-title"><span class="step">4</span>Variables WORLDFIP</h2>
      <p class="mt-4">
        Identifica productor, variable y consumidor.
      </p>

      <div id="worldfip-table" class="mt-5 overflow-x-auto"></div>

      <button id="validate-worldfip" class="btn-primary mt-5">
        Evaluar variables
      </button>

      <div id="worldfip-feedback" class="feedback hidden mt-4"></div>
    </section>

    <section class="card">
      <h2 class="section-title"><span class="step">5</span>Prioridad CAN</h2>
      <p class="mt-4">
        Asigna identificadores. En esta práctica, un ID numéricamente
        menor representa mayor prioridad.
      </p>

      <div id="can-messages" class="space-y-3 mt-5"></div>

      <button id="simulate-can" class="btn-primary mt-5">
        Ejecutar arbitraje
      </button>

      <div id="can-result" class="feedback hidden mt-4"></div>
    </section>

    <section class="card">
      <h2 class="section-title"><span class="step">6</span>Dashboard industrial</h2>

      <button id="refresh-dashboard" class="btn-primary mt-5">
        Actualizar proceso
      </button>

      <div id="dashboard" class="grid md:grid-cols-4 gap-4 mt-6"></div>

      <div class="architecture mt-6">
        <span>Sensor</span><span>→</span>
        <span>Bus</span><span>→</span>
        <span>PLC</span><span>→</span>
        <span>API</span><span>→</span>
        <span>Web</span>
      </div>
    </section>

  </main>

  <footer class="bg-slate-950 text-slate-400 mt-12">
    <div class="max-w-7xl mx-auto px-4 py-6 text-center text-sm">
      Industrial Network Decision Lab
    </div>
  </footer>

  <script type="module" src="./src/js/app.js"></script>
</body>
</html>
```

---

## 9. Archivo `src/css/styles.css`

```css
html {
  scroll-behavior: smooth;
}

body {
  min-height: 100vh;
}

.card {
  background: white;
  border-radius: 1rem;
  padding: 1.5rem;
  box-shadow:
    0 1px 2px rgb(15 23 42 / 5%),
    0 8px 24px rgb(15 23 42 / 6%);
}

.section-title {
  display: flex;
  align-items: center;
  gap: 0.75rem;
  font-size: 1.35rem;
  font-weight: 700;
}

.step {
  width: 2rem;
  height: 2rem;
  display: inline-flex;
  align-items: center;
  justify-content: center;
  background: #0891b2;
  color: white;
  border-radius: 9999px;
}

.device {
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  min-height: 90px;
  background: #ecfeff;
  border: 1px solid #a5f3fc;
  border-radius: 0.75rem;
}

.device strong {
  color: #155e75;
  font-size: 1.15rem;
}

.device span {
  color: #64748b;
  font-size: 0.8rem;
}

.network-card {
  padding: 1rem;
  border: 2px solid #e2e8f0;
  border-radius: 0.75rem;
  cursor: pointer;
  transition: 0.2s ease;
}

.network-card:hover {
  border-color: #22d3ee;
  transform: translateY(-2px);
}

.network-card.active {
  border-color: #0891b2;
  background: #ecfeff;
}

.btn-primary {
  padding: 0.7rem 1rem;
  background: #0891b2;
  color: white;
  border-radius: 0.6rem;
  font-weight: 600;
}

.btn-primary:hover {
  background: #0e7490;
}

.feedback {
  padding: 1rem;
  border-radius: 0.7rem;
}

.feedback.success {
  background: #ecfdf5;
  border: 1px solid #86efac;
  color: #166534;
}

.feedback.error {
  background: #fef2f2;
  border: 1px solid #fca5a5;
  color: #991b1b;
}

.feedback.info {
  background: #eff6ff;
  border: 1px solid #bfdbfe;
  color: #1e40af;
}

.node-card {
  padding: 0.8rem;
  background: #f8fafc;
  border: 1px solid #cbd5e1;
  border-radius: 0.7rem;
  text-align: center;
}

.node-card input[type="number"] {
  width: 70px;
  margin-top: 0.4rem;
  padding: 0.3rem;
  text-align: center;
  border: 1px solid #cbd5e1;
  border-radius: 0.35rem;
}

.node-card label {
  display: block;
  margin-top: 0.5rem;
  font-size: 0.75rem;
}

.world-table {
  width: 100%;
  border-collapse: collapse;
}

.world-table th,
.world-table td {
  padding: 0.75rem;
  border: 1px solid #e2e8f0;
  text-align: left;
}

.world-table th {
  background: #0f172a;
  color: white;
}

.world-table select {
  width: 100%;
  padding: 0.4rem;
  border: 1px solid #cbd5e1;
  border-radius: 0.4rem;
}

.can-row {
  display: grid;
  grid-template-columns: minmax(220px, 1fr) 150px 170px;
  gap: 1rem;
  align-items: center;
  padding: 0.9rem;
  border: 1px solid #e2e8f0;
  border-radius: 0.7rem;
}

.can-row input {
  padding: 0.5rem;
  border: 1px solid #cbd5e1;
  border-radius: 0.4rem;
}

.dashboard-card {
  padding: 1rem;
  background: #0f172a;
  color: white;
  border-radius: 0.8rem;
}

.dashboard-card small {
  color: #94a3b8;
}

.dashboard-value {
  margin-top: 0.4rem;
  font-size: 1.8rem;
  font-weight: 700;
}

.architecture {
  display: flex;
  flex-wrap: wrap;
  gap: 0.6rem;
  align-items: center;
  justify-content: center;
  padding: 1rem;
  background: #f8fafc;
  border-radius: 0.8rem;
  font-weight: 600;
}

@media (max-width: 768px) {
  .can-row {
    grid-template-columns: 1fr;
  }
}
```

---

## 10. Archivo `src/js/data.js`

```javascript
export const networks = [
  {
    id: "profibus",
    name: "PROFIBUS",
    focus: "PLC ↔ E/S y dispositivos de campo",
    access: "Token + maestro/esclavo",
    strength: "Diagnóstico, perfiles y comunicación cíclica",
    example: "Celda de manufactura con PLC, variadores y E/S"
  },
  {
    id: "worldfip",
    name: "WORLDFIP",
    focus: "Variables de proceso",
    access: "Planificación de variables",
    strength: "Modelo productor–consumidor",
    example: "Proceso con variables compartidas por PLC, HMI e histórico"
  },
  {
    id: "can",
    name: "CAN",
    focus: "Mensajes con identificador",
    access: "Arbitraje no destructivo",
    strength: "Prioridad y robustez",
    example: "Máquina móvil con eventos críticos"
  },
  {
    id: "devicenet",
    name: "DeviceNet",
    focus: "Dispositivos industriales",
    access: "CAN + servicios de aplicación",
    strength: "Interoperabilidad de dispositivos",
    example: "Sensores, actuadores, arrancadores y E/S distribuidas"
  }
];

export const profibusNodes = [
  { id: "plc", name: "PLC", address: 2, terminator: true },
  { id: "vfd", name: "VFD", address: 10, terminator: false },
  { id: "io1", name: "E/S 1", address: 20, terminator: false },
  { id: "io2", name: "E/S 2", address: 21, terminator: false },
  { id: "io3", name: "E/S 3", address: 22, terminator: false },
  { id: "io4", name: "E/S 4", address: 23, terminator: false },
  { id: "hmi", name: "HMI", address: 30, terminator: true }
];

export const worldVariables = [
  {
    id: "level",
    name: "Nivel tanque",
    correctProducer: "sensor",
    correctConsumer: "plc"
  },
  {
    id: "temperature",
    name: "Temperatura",
    correctProducer: "sensor",
    correctConsumer: "hmi"
  },
  {
    id: "pressure",
    name: "Presión",
    correctProducer: "sensor",
    correctConsumer: "plc"
  },
  {
    id: "pump_command",
    name: "Comando bomba",
    correctProducer: "plc",
    correctConsumer: "actuator"
  }
];

export const canMessages = [
  { id: "emergency", name: "Paro de emergencia" },
  { id: "pressure_alarm", name: "Alarma de presión" },
  { id: "speed", name: "Velocidad" },
  { id: "temperature", name: "Temperatura" },
  { id: "maintenance", name: "Dato de mantenimiento" }
];
```

---

## 11. Archivo `src/js/profibus.js`

```javascript
export function renderProfibus(container, nodes) {
  container.innerHTML = "";

  nodes.forEach((node) => {
    const card = document.createElement("div");

    card.className = "node-card";

    card.innerHTML = `
      <strong>${node.name}</strong>

      <div>
        <input
          type="number"
          min="0"
          max="126"
          value="${node.address}"
          data-address="${node.id}"
        >
      </div>

      <label>
        <input
          type="checkbox"
          data-terminator="${node.id}"
          ${node.terminator ? "checked" : ""}
        >
        Terminador
      </label>
    `;

    container.appendChild(card);
  });

  container.querySelectorAll("[data-address]").forEach((input) => {
    input.addEventListener("change", (event) => {
      const node = nodes.find(
        (item) => item.id === event.target.dataset.address
      );

      node.address = Number(event.target.value);
    });
  });

  container.querySelectorAll("[data-terminator]").forEach((checkbox) => {
    checkbox.addEventListener("change", (event) => {
      const node = nodes.find(
        (item) => item.id === event.target.dataset.terminator
      );

      node.terminator = event.target.checked;
    });
  });
}

export function validateProfibus(nodes) {
  const errors = [];

  const addresses = nodes.map((node) => node.address);

  const duplicates = addresses.filter(
    (value, index) => addresses.indexOf(value) !== index
  );

  if (duplicates.length > 0) {
    errors.push("Existen direcciones duplicadas.");
  }

  const invalid = nodes.some(
    (node) =>
      !Number.isInteger(node.address) ||
      node.address < 0 ||
      node.address > 126
  );

  if (invalid) {
    errors.push("Las direcciones deben estar entre 0 y 126.");
  }

  const terminators = nodes.filter((node) => node.terminator);

  if (terminators.length !== 2) {
    errors.push("Deben existir exactamente dos terminadores.");
  }

  if (!nodes[0].terminator || !nodes[nodes.length - 1].terminator) {
    errors.push("Los terminadores deben estar en los extremos físicos.");
  }

  return {
    valid: errors.length === 0,
    errors
  };
}
```

---

## 12. Archivo `src/js/worldfip.js`

```javascript
const producerOptions = [
  { value: "", label: "Seleccionar" },
  { value: "sensor", label: "Sensor" },
  { value: "plc", label: "PLC" },
  { value: "hmi", label: "HMI" }
];

const consumerOptions = [
  { value: "", label: "Seleccionar" },
  { value: "plc", label: "PLC" },
  { value: "hmi", label: "HMI" },
  { value: "actuator", label: "Actuador" }
];

function createOptions(options) {
  return options
    .map(
      (item) =>
        `<option value="${item.value}">${item.label}</option>`
    )
    .join("");
}

export function renderWorldFip(container, variables) {
  container.innerHTML = `
    <table class="world-table">
      <thead>
        <tr>
          <th>Variable</th>
          <th>Productor</th>
          <th>Consumidor principal</th>
        </tr>
      </thead>

      <tbody>
        ${variables
          .map(
            (variable) => `
              <tr>
                <td>${variable.name}</td>

                <td>
                  <select data-producer="${variable.id}">
                    ${createOptions(producerOptions)}
                  </select>
                </td>

                <td>
                  <select data-consumer="${variable.id}">
                    ${createOptions(consumerOptions)}
                  </select>
                </td>
              </tr>
            `
          )
          .join("")}
      </tbody>
    </table>
  `;
}

export function evaluateWorldFip(variables) {
  let correct = 0;

  variables.forEach((variable) => {
    const producer = document.querySelector(
      `[data-producer="${variable.id}"]`
    ).value;

    const consumer = document.querySelector(
      `[data-consumer="${variable.id}"]`
    ).value;

    if (
      producer === variable.correctProducer &&
      consumer === variable.correctConsumer
    ) {
      correct++;
    }
  });

  return {
    correct,
    total: variables.length
  };
}
```

---

## 13. Archivo `src/js/can.js`

```javascript
export function renderCanMessages(container, messages) {
  container.innerHTML = "";

  messages.forEach((message) => {
    const row = document.createElement("div");

    row.className = "can-row";

    row.innerHTML = `
      <div>
        <strong>${message.name}</strong>
      </div>

      <input
        type="number"
        min="0"
        max="2047"
        placeholder="ID"
        data-can-id="${message.id}"
      >

      <span class="text-sm text-slate-500">
        Menor ID = mayor prioridad
      </span>
    `;

    container.appendChild(row);
  });
}

export function simulateArbitration(messages) {
  const values = messages.map((message) => {
    const input = document.querySelector(
      `[data-can-id="${message.id}"]`
    );

    const raw = input.value.trim();

    return {
      ...message,
      value: raw === "" ? null : Number(raw)
    };
  });

  const incomplete = values.some(
    (item) =>
      item.value === null ||
      !Number.isInteger(item.value) ||
      item.value < 0 ||
      item.value > 2047
  );

  if (incomplete) {
    return {
      valid: false,
      message: "Captura IDs enteros entre 0 y 2047 para todos los mensajes."
    };
  }

  const ids = values.map((item) => item.value);

  const duplicates = ids.some(
    (value, index) => ids.indexOf(value) !== index
  );

  if (duplicates) {
    return {
      valid: false,
      message: "Los identificadores deben ser únicos."
    };
  }

  const ordered = [...values].sort(
    (a, b) => a.value - b.value
  );

  return {
    valid: true,
    winner: ordered[0],
    ordered
  };
}
```

---

## 14. Archivo `src/js/dashboard.js`

```javascript
function randomBetween(min, max, decimals = 0) {
  const value = Math.random() * (max - min) + min;

  return Number(value.toFixed(decimals));
}

export function createProcessData() {
  const pressure = randomBetween(2.5, 6.5, 1);

  return {
    level: randomBetween(35, 95),
    temperature: randomBetween(24, 75, 1),
    pressure,
    pump: pressure < 5 ? "RUN" : "STOP"
  };
}

export function renderDashboard(container, data) {
  const cards = [
    { name: "Nivel", value: `${data.level}%` },
    { name: "Temperatura", value: `${data.temperature} °C` },
    { name: "Presión", value: `${data.pressure} bar` },
    { name: "Bomba", value: data.pump }
  ];

  container.innerHTML = cards
    .map(
      (card) => `
        <div class="dashboard-card">
          <small>${card.name}</small>
          <div class="dashboard-value">${card.value}</div>
        </div>
      `
    )
    .join("");
}
```

---

## 15. Archivo `src/js/app.js`

```javascript
import {
  networks,
  profibusNodes,
  worldVariables,
  canMessages
} from "./data.js";

import {
  renderProfibus,
  validateProfibus
} from "./profibus.js";

import {
  renderWorldFip,
  evaluateWorldFip
} from "./worldfip.js";

import {
  renderCanMessages,
  simulateArbitration
} from "./can.js";

import {
  createProcessData,
  renderDashboard
} from "./dashboard.js";

const networkOptions = document.querySelector("#network-options");
const networkDetail = document.querySelector("#network-detail");

const profibusContainer = document.querySelector("#profibus-nodes");
const profibusFeedback = document.querySelector("#profibus-feedback");

const worldFipContainer = document.querySelector("#worldfip-table");
const worldFipFeedback = document.querySelector("#worldfip-feedback");

const canContainer = document.querySelector("#can-messages");
const canResult = document.querySelector("#can-result");

const dashboard = document.querySelector("#dashboard");

function showFeedback(element, type, html) {
  element.className = `feedback ${type} mt-4`;
  element.innerHTML = html;
}

function renderNetworks() {
  networkOptions.innerHTML = "";

  networks.forEach((network) => {
    const card = document.createElement("button");

    card.className = "network-card text-left";
    card.dataset.network = network.id;

    card.innerHTML = `
      <strong class="text-lg">${network.name}</strong>
      <p class="text-sm text-slate-500 mt-2">${network.focus}</p>
    `;

    card.addEventListener("click", () => {
      document
        .querySelectorAll("[data-network]")
        .forEach((item) => item.classList.remove("active"));

      card.classList.add("active");

      networkDetail.className = "mt-6";

      networkDetail.innerHTML = `
        <div class="grid md:grid-cols-3 gap-4">
          <div class="p-4 bg-slate-50 rounded-lg">
            <strong>Acceso al medio</strong>
            <p class="mt-2">${network.access}</p>
          </div>

          <div class="p-4 bg-slate-50 rounded-lg">
            <strong>Fortaleza</strong>
            <p class="mt-2">${network.strength}</p>
          </div>

          <div class="p-4 bg-slate-50 rounded-lg">
            <strong>Ejemplo</strong>
            <p class="mt-2">${network.example}</p>
          </div>
        </div>
      `;
    });

    networkOptions.appendChild(card);
  });
}

document
  .querySelector("#validate-profibus")
  .addEventListener("click", () => {
    const result = validateProfibus(profibusNodes);

    if (result.valid) {
      showFeedback(
        profibusFeedback,
        "success",
        `
          <strong>Configuración válida.</strong><br>
          Direcciones únicas y terminadores en los extremos.
        `
      );

      return;
    }

    showFeedback(
      profibusFeedback,
      "error",
      `
        <strong>Problemas encontrados:</strong>

        <ul class="list-disc ml-5 mt-2">
          ${result.errors
            .map((error) => `<li>${error}</li>`)
            .join("")}
        </ul>
      `
    );
  });

document
  .querySelector("#validate-worldfip")
  .addEventListener("click", () => {
    const result = evaluateWorldFip(worldVariables);

    showFeedback(
      worldFipFeedback,
      result.correct === result.total ? "success" : "info",
      `
        Resultado:
        <strong>${result.correct} / ${result.total}</strong>
        <br>
        Recuerda pensar en productor → variable → consumidor.
      `
    );
  });

document
  .querySelector("#simulate-can")
  .addEventListener("click", () => {
    const result = simulateArbitration(canMessages);

    if (!result.valid) {
      showFeedback(canResult, "error", result.message);
      return;
    }

    const order = result.ordered
      .map((item) => `${item.name} (ID ${item.value})`)
      .join(" → ");

    showFeedback(
      canResult,
      "success",
      `
        <strong>Gana el arbitraje:</strong>
        ${result.winner.name}

        <br><br>

        <strong>Orden de prioridad:</strong>
        <br>
        ${order}
      `
    );
  });

document
  .querySelector("#refresh-dashboard")
  .addEventListener("click", () => {
    renderDashboard(
      dashboard,
      createProcessData()
    );
  });

renderNetworks();
renderProfibus(profibusContainer, profibusNodes);
renderWorldFip(worldFipContainer, worldVariables);
renderCanMessages(canContainer, canMessages);
renderDashboard(dashboard, createProcessData());
```

---

## 16. Archivo `README.md`

```markdown
# Industrial Network Decision Lab

Práctica integradora del Tema III de la asignatura
Control de Procesos Industriales a través de la Web.

## Redes estudiadas

- PROFIBUS
- WORLDFIP
- CAN
- DeviceNet

## Tecnologías

- HTML5
- Tailwind CSS CDN
- JavaScript ES Modules

No se utiliza ningún framework de desarrollo.

## Ejecutar

Usar Live Server en VSCode o un servidor HTTP local.

### macOS / Linux

```bash
python3 -m http.server 5500
```

### Windows

```powershell
python -m http.server 5500
```

Abrir:

```text
http://localhost:5500
```

## Importante

La aplicación tiene fines académicos.
No simula eléctricamente una red industrial real.
```

---

## 17. Ejecución local

No abras `index.html` directamente con doble clic.

Debido al uso de ES Modules, utiliza un servidor local.

### Con Live Server

1. Abre el proyecto en VSCode.
2. Abre `index.html`.
3. Clic derecho.
4. Selecciona `Open with Live Server`.

### Con Python

macOS/Linux:

```bash
python3 -m http.server 5500
```

Windows:

```powershell
python -m http.server 5500
```

Abre:

```text
http://localhost:5500
```

---

# 18. Desarrollo de la práctica

## Actividad 1 — Comparar redes

Selecciona cada una:

```text
PROFIBUS
WORLDFIP
CAN
DeviceNet
```

Completa:

| Red | Enfoque | Acceso al medio | Fortaleza |
|---|---|---|---|
| PROFIBUS | | | |
| WORLDFIP | | | |
| CAN | | | |
| DeviceNet | | | |

### Pregunta

¿Qué red elegirías para una celda con PLC, variadores y E/S remotas? Justifica.

---

## Actividad 2 — PROFIBUS

Primero valida la configuración inicial.

Después realiza estas pruebas.

### Prueba A — dirección duplicada

```text
VFD = 10
E/S 1 = 10
```

Valida.

### Prueba B — terminación faltante

Desactiva uno de los terminadores.

Valida.

### Prueba C — tres terminadores

Activa un terminador adicional en un nodo intermedio.

### Preguntas

1. ¿Por qué las direcciones deben ser únicas?
2. ¿Dónde deben colocarse los terminadores?
3. ¿Qué problemas físicos estudiaste para PROFIBUS?
4. ¿Qué diferencia existe entre datos cíclicos y acíclicos?

---

## Actividad 3 — WORLDFIP

Completa la tabla de variables.

Debes razonar con:

```text
Productor
   ↓
Variable
   ↓
Consumidor
```

Ejemplo:

```text
Sensor de nivel
      ↓
   NIVEL_101
      ↓
     PLC
```

### Preguntas

1. ¿Puede una variable tener más de un consumidor?
2. ¿Qué ventaja tiene publicar una variable para varios consumidores?
3. ¿Qué ocurriría si una variable crítica no se actualiza en el periodo esperado?
4. ¿Qué capas OSI son especialmente relevantes en WORLDFIP?

---

## Actividad 4 — CAN

Captura:

| Mensaje | ID sugerido |
|---|---:|
| Paro de emergencia | 10 |
| Alarma de presión | 50 |
| Velocidad | 200 |
| Temperatura | 400 |
| Mantenimiento | 800 |

Presiona:

```text
Ejecutar arbitraje
```

Después cambia:

```text
Paro de emergencia = 900
Mantenimiento = 10
```

Ejecuta otra vez.

### Preguntas

1. ¿Qué mensaje gana el arbitraje?
2. ¿Por qué un identificador menor representa mayor prioridad en este ejercicio?
3. ¿Por qué la segunda configuración sería una mala decisión de ingeniería?
4. ¿Qué significa arbitraje no destructivo?

---

## Actividad 5 — DeviceNet

Completa:

| Concepto | CAN | DeviceNet |
|---|---|---|
| Arbitraje | | |
| Mensajes | | |
| Modelo de dispositivo | | |
| Aplicación industrial | | |
| Interoperabilidad | | |

### Pregunta

¿Qué agrega DeviceNet sobre CAN para hacerlo útil como red industrial de dispositivos?

---

## Actividad 6 — Dashboard industrial

Presiona varias veces:

```text
Actualizar proceso
```

Observa:

```text
Nivel
Temperatura
Presión
Estado de bomba
```

Conceptualmente:

```text
Sensor
  ↓
Bus industrial
  ↓
PLC / Gateway
  ↓
API / servidor
  ↓
Dashboard Web
```

### Importante

Los valores de esta práctica son generados por JavaScript.

No se está leyendo un PLC real.

### Preguntas

1. ¿Qué función cumple el PLC o gateway?
2. ¿Por qué el dashboard no sustituye la red de campo?
3. ¿Qué tres variables publicarías obligatoriamente en una interfaz web?
4. ¿Qué variable usarías para generar una alarma?

---

# 19. Ejercicio integrador

Selecciona una red para cada escenario.

| Caso | Red seleccionada | Justificación |
|---|---|---|
| Línea de empaque con PLC y E/S | | |
| Variables compartidas por PLC, HMI e histórico | | |
| Máquina móvil con eventos críticos | | |
| Sensores y actuadores industriales basados en CAN | | |

La justificación debe considerar:

```text
Aplicación
+
Acceso al medio
+
Criticidad
+
Dispositivos
+
Diagnóstico
+
Instalación
```

---

# 20. Evidencias obligatorias

Incluye en tu reporte:

1. Captura del comparador.
2. PROFIBUS configurado correctamente.
3. Error por dirección duplicada.
4. Error por terminación incorrecta.
5. Tabla productor–consumidor WORLDFIP.
6. Arbitraje CAN.
7. Tabla CAN vs DeviceNet.
8. Dashboard industrial.
9. Historial de commits.
10. URL del repositorio GitHub.
11. URL de GitHub Pages.

---

# 21. Verificar historial Git

```bash
git log --oneline
```

Ejemplo esperado:

```text
a1b2c3d docs: complete student report
4d5e6f7 feat: add industrial dashboard
8g9h0i1 feat: add can priority simulator
2j3k4l5 feat: add worldfip variable model
6m7n8o9 feat: add profibus validation
0p1q2r3 feat: add industrial network comparison
4s5t6u7 chore: initialize industrial network lab
```

---

# 22. Flujo de trabajo Git

Después de cada módulo:

```bash
git status
```

```bash
git add .
```

```bash
git commit -m "MENSAJE"
```

```bash
git push
```

---

# 23. Publicar con GitHub Pages

En GitHub:

```text
Settings
→ Pages
→ Build and deployment
→ Deploy from a branch
```

Selecciona:

```text
Branch: main
Folder: /root
```

Guarda.

La URL tendrá una forma similar a:

```text
https://usuario.github.io/industrial-network-lab/
```

---

# 24. Preguntas finales

1. ¿Por qué la determinación temporal puede ser más importante que el volumen de datos?
2. ¿Cómo evita PROFIBUS conflictos entre maestros?
3. ¿Qué diferencia existe entre dato cíclico y acíclico?
4. ¿Qué función cumple la terminación física?
5. Explica productor–consumidor en WORLDFIP.
6. ¿Qué capas OSI son relevantes en WORLDFIP?
7. ¿Por qué CAN usa identificadores?
8. ¿Qué significa arbitraje no destructivo?
9. ¿Por qué un paro de emergencia debería tener alta prioridad?
10. ¿Qué agrega DeviceNet sobre CAN?
11. Menciona cuatro fallas físicas comunes en redes industriales.
12. ¿Qué función cumple un PLC/gateway antes de llevar información a la web?
13. ¿Por qué el dashboard no sustituye a la red industrial?
14. ¿Qué red seleccionarías para la planta piloto? Justifica y descarta al menos dos alternativas.

---

# 25. Entregable

Entrega:

```text
URL del repositorio GitHub
+
URL de GitHub Pages
+
Reporte PDF
```

El reporte deberá incluir:

```text
1. Portada
2. Objetivo
3. Tabla comparativa
4. Evidencias PROFIBUS
5. Mapa WORLDFIP
6. Priorización CAN
7. Comparación CAN / DeviceNet
8. Arquitectura hacia web
9. Respuestas finales
10. Conclusiones
```

---

# 26. Rúbrica

| Criterio | Porcentaje |
|---|---:|
| Proyecto ejecuta correctamente | 10% |
| Uso adecuado de Git/GitHub | 10% |
| Comparación de redes | 15% |
| Configuración PROFIBUS | 15% |
| Productor–consumidor WORLDFIP | 15% |
| Prioridad y arbitraje CAN | 15% |
| Comprensión de DeviceNet | 10% |
| Integración conceptual con web | 5% |
| Conclusiones técnicas | 5% |
| **Total** | **100%** |

---

# 27. Retos opcionales

## Reto 1 — Datos cíclicos/acíclicos

Agregar una sección para clasificar:

```text
Nivel
Temperatura
Run/Stop
Velocidad
Firmware
Historial
Parámetros
Alarmas
```

## Reto 2 — Riesgos físicos

Agregar opciones:

```text
Cable blindado
Separación potencia/datos
Terminación correcta
Puesta a tierra
Derivaciones largas
Direcciones duplicadas
```

## Reto 3 — Alarmas web

Mostrar alerta cuando:

```text
Presión > 6 bar
```

o:

```text
Temperatura > 70 °C
```

## Reto 4 — Historial

Guardar las últimas 10 lecturas en:

```text
localStorage
```

## Reto 5 — Dashboard avanzado

Agregar:

```text
Estado de red
ONLINE / OFFLINE

Calidad
%

Última actualización

Alarmas activas
```

---

# 28. Concepto final

```text
PROFIBUS
  ↓
Token + maestro/esclavo
  ↓
Datos cíclicos / diagnóstico

WORLDFIP
  ↓
Productor
  ↓
Variable
  ↓
Consumidores

CAN
  ↓
Mensajes
  ↓
Identificadores
  ↓
Arbitraje

DeviceNet
  ↓
CAN
  +
Servicios industriales
  +
Modelo de dispositivo
```

Y finalmente:

```text
RED INDUSTRIAL
      ↓
PLC / GATEWAY
      ↓
API / SERVIDOR
      ↓
DASHBOARD WEB
```

La interfaz web no sustituye la red industrial.

La web **visualiza, registra y presenta** información proveniente del proceso.
