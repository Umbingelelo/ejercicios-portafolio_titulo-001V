# Guía Paso a Paso: API REST de Donantes con Express (Node.js)

## 🎯 Objetivo

Construir una **API REST con Express** que permita:

- Listar donantes  
- Obtener un donante por **RUT**  
- Crear un donante  
- Eliminar un donante  
- Mantener `donantes.json` **ordenado por RUT** y **sin duplicados**  
- Responder con **códigos HTTP correctos**

---

## 🧰 Requisitos

- Node.js LTS instalado  
- Terminal  
- Editor de código (VS Code recomendado)  
- Postman / Insomnia (opcional) o `curl`

---

## 1️⃣ Crear proyecto desde cero

### 1.1 Crear carpeta e iniciar npm

```bash
mkdir api-donantes
cd api-donantes
npm init -y
```

### 1.2 Instalar dependencias

```bash
npm install express
npm install -D nodemon
```

### 1.3 Crear estructura de carpetas

```bash
mkdir src
mkdir src/routes src/services src/data
touch src/index.js
touch src/routes/donantes.routes.js
touch src/services/donantes.service.js
touch src/data/donantes.json
```

---

## 2️⃣ Crear el archivo JSON inicial

En `src/data/donantes.json` pega:

```json
[
  { "rut": 15274, "nombre": "Fulana de Tal", "monto": 200 },
  { "rut": 15891, "nombre": "Jean Dupont", "monto": 150 },
  { "rut": 16443, "nombre": "Erika Mustermann", "monto": 400 },
  { "rut": 16504, "nombre": "Perico Los Palotes", "monto": 80 },
  { "rut": 17004, "nombre": "Jan Kowalski", "monto": 200 }
]
```

---

## 3️⃣ Configurar scripts de ejecución

En `package.json` agrega:

```json
{
  "scripts": {
    "dev": "nodemon src/index.js",
    "start": "node src/index.js"
  }
}
```

---

## 4️⃣ Crear servidor Express

En `src/index.js`:

```js
const express = require("express");
const donantesRoutes = require("./routes/donantes.routes");

const app = express();
const PORT = 3000;

app.use(express.json());

app.get("/health", (req, res) => {
  res.json({ status: "ok" });
});

app.use("/api/donantes", donantesRoutes);

app.use((req, res) => {
  res.status(404).json({ error: "Ruta no encontrada" });
});

app.listen(PORT, () => {
  console.log(`API corriendo en http://localhost:${PORT}`);
});
```

---

## 5️⃣ Capa de servicio

Archivo `src/services/donantes.service.js`:

```js
const fs = require("fs");
const path = require("path");

const DATA_PATH = path.join(__dirname, "..", "data", "donantes.json");

function readDonantes() {
  try {
    return JSON.parse(fs.readFileSync(DATA_PATH, "utf-8"));
  } catch {
    return [];
  }
}

function writeDonantes(donantes) {
  const ordenados = [...donantes].sort((a, b) => a.rut - b.rut);
  fs.writeFileSync(DATA_PATH, JSON.stringify(ordenados, null, 2));
}

function findByRut(rut) {
  return readDonantes().find(d => d.rut === Number(rut)) || null;
}

function addDonante(donante) {
  const donantes = readDonantes();
  if (donantes.some(d => d.rut === donante.rut)) {
    return false;
  }
  donantes.push(donante);
  writeDonantes(donantes);
  return true;
}

function deleteByRut(rut) {
  const donantes = readDonantes();
  const filtrados = donantes.filter(d => d.rut !== Number(rut));
  if (filtrados.length === donantes.length) return false;
  writeDonantes(filtrados);
  return true;
}

module.exports = { readDonantes, findByRut, addDonante, deleteByRut };
```

---

## 6️⃣ Rutas REST

Archivo `src/routes/donantes.routes.js`:

```js
const express = require("express");
const {
  readDonantes,
  findByRut,
  addDonante,
  deleteByRut
} = require("../services/donantes.service");

const router = express.Router();

router.get("/", (req, res) => {
  res.json(readDonantes());
});

router.get("/:rut", (req, res) => {
  const d = findByRut(req.params.rut);
  if (!d) return res.status(404).json({ error: "No encontrado" });
  res.json(d);
});

router.post("/", (req, res) => {
  if (!addDonante(req.body)) {
    return res.status(409).json({ error: "RUT duplicado" });
  }
  res.status(201).json(req.body);
});

router.delete("/:rut", (req, res) => {
  if (!deleteByRut(req.params.rut)) {
    return res.status(404).json({ error: "No encontrado" });
  }
  res.status(204).send();
});

module.exports = router;
```

---

## 7️⃣ Levantar la API

```bash
npm run dev
```

---

## 8️⃣ Probar con curl

```bash
curl http://localhost:3000/api/donantes
curl http://localhost:3000/api/donantes/15274
```

```bash
curl -X POST http://localhost:3000/api/donantes \
  -H "Content-Type: application/json" \
  -d '{"rut":19999,"nombre":"Cristian Calderón","monto":500}'
```

```bash
curl -X DELETE http://localhost:3000/api/donantes/19999 -i
```
