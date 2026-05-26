# Actividad Bonus: Tipos de Pruebas en Node.js + Express

## Datos generales

**Asignatura:** TPY1101  
**Duración estimada:** 2 horas (120 minutos)  
**Modalidad:** Individual o en parejas  
**Sistema operativo considerado:** Windows  
**Herramientas principales:** Node.js, Express, Jest, Supertest, k6, PowerShell, VS Code  
**Carácter:** Bonus / refuerzo del portafolio (complementa al Ejercicio 5 - Plan de Pruebas)

---

## 1. Propósito de la actividad

En esta actividad construirás una **mini API REST en Node.js + Express** y aplicarás sobre ella **al menos una prueba de cada uno de los principales tipos de pruebas** que existen en la industria del software.

El objetivo no es escribir muchas pruebas, sino que entiendas **qué tipo de prueba estás escribiendo, qué responde y qué no responde**, y por qué cada una aporta evidencia distinta para defender la calidad de tu proyecto.

Al finalizar, deberías ser capaz de:

- diferenciar pruebas unitarias, de integración, funcionales/E2E, de regresión, de carga, de seguridad y de humo;
- escribir y ejecutar cada una con herramientas estándar (Jest, Supertest, k6, npm audit);
- registrar evidencia trazable de cada ejecución;
- explicar qué decisión técnica respalda cada tipo de prueba.

> Esta actividad **profundiza** lo que vimos en el Ejercicio 5 (Plan de Pruebas). Allí definimos *qué* y *cómo* probar a nivel de plan. Aquí lo **ejecutamos en código real**.

---

## 2. Contexto del caso

Una pequeña startup quiere lanzar una **API de gestión de tareas (to-do)** como MVP para un cliente interno. Antes de pasar a producción, el equipo de calidad exige demostrar que la API:

- funciona unidad por unidad (sin depender de otras capas);
- funciona correctamente cuando todas las capas se integran;
- cumple los flujos de negocio completos;
- no rompe funcionalidades anteriores cuando se hacen cambios;
- soporta carga moderada;
- no presenta vulnerabilidades evidentes;
- está viva en el ambiente desplegado (smoke check).

Tu trabajo será construir esta mini API y aplicar **una prueba representativa de cada tipo**.

---

## 3. Producto esperado

Al finalizar la actividad, cada estudiante o equipo debe contar con:

1. una mini API Node.js + Express funcional;
2. una carpeta `tests/` con archivos separados por tipo de prueba;
3. un archivo `loadtest.js` para la prueba de carga con k6;
4. un script de humo en PowerShell o Node;
5. el resultado de `npm audit` en un archivo de evidencia;
6. una **matriz consolidada** con los 7 tipos de prueba ejecutados;
7. una breve reflexión final.

---

## 4. Requisitos previos

Antes de comenzar, verifica que tienes instalado:

- **Node.js LTS**
- **VS Code**
- **PowerShell**
- **k6** (opcional, para la prueba de carga — instalación en Bloque 5)

### Verificación inicial en Windows

Abre **PowerShell** y ejecuta:

```powershell
node -v
npm -v
```

### Resultado esperado

Debes ver una versión válida para Node (>= 18) y npm.

**Checkpoint 0 aprobado** cuando ambas versiones se muestran sin error.

---

## 5. Marco teórico breve — los 7 tipos de prueba que cubriremos

Antes de programar, ten claro **qué responde cada tipo de prueba**. Esto es lo que debes poder explicar al defender tu portafolio.

| # | Tipo de prueba | Qué responde | Aislamiento | Herramienta que usaremos |
|---|---|---|---|---|
| 1 | **Unitaria** | ¿Esta función pequeña hace lo que dice? | Máximo (sin red, sin BD) | Jest |
| 2 | **Integración** | ¿Las capas (ruta + servicio + datos) se entienden entre sí? | Medio (sin browser) | Jest + Supertest |
| 3 | **Funcional / E2E (API)** | ¿El flujo completo del negocio funciona de punta a punta? | Bajo (sistema real) | Jest + Supertest |
| 4 | **Regresión** | ¿Volvió a aparecer un bug que ya habíamos arreglado? | Variable | Jest (reusa cualquier nivel) |
| 5 | **Carga (performance)** | ¿Aguanta usuarios concurrentes sin caerse ni degradarse? | Bajo | k6 |
| 6 | **Seguridad** | ¿Hay vulnerabilidades conocidas o malas configuraciones? | Variable | npm audit + Supertest |
| 7 | **Humo (smoke)** | ¿El sistema arranca y responde lo mínimo? | Muy bajo | Script + Supertest |

> Existen muchos más tipos (snapshot, contrato, mutación, accesibilidad, usabilidad, aceptación, exploratorias, etc.). En el Bloque 7 mencionamos cuándo agregarlos. Este bonus se enfoca en los 7 esenciales.

---

## 6. Organización del tiempo

| Bloque | Tema | Tiempo |
|---|---|---|
| 0 | Preparación del entorno | 5 min |
| 1 | Construir la mini API + configurar Jest | 20 min |
| 2 | Pruebas Unitarias + Humo | 20 min |
| 3 | Pruebas de Integración + Funcionales/E2E | 25 min |
| 4 | Pruebas de Regresión + Seguridad | 25 min |
| 5 | Prueba de Carga con k6 | 15 min |
| 6 | Matriz consolidada, evidencias y cierre | 10 min |

**Tiempo total estimado:** 120 minutos

---

# 7. Desarrollo paso a paso

---

## Bloque 1 — Construir la mini API y configurar Jest (20 minutos)

### Paso 1. Crear el proyecto

En PowerShell ejecuta:

```powershell
mkdir api-tareas-bonus
cd api-tareas-bonus
npm init -y
npm install express
npm install -D jest supertest nodemon
```

### Paso 2. Crear la estructura

```powershell
mkdir src, src\routes, src\services, src\utils, tests, evidencias

New-Item src\app.js -ItemType File
New-Item src\server.js -ItemType File
New-Item src\routes\tareas.routes.js -ItemType File
New-Item src\services\tareas.service.js -ItemType File
New-Item src\utils\validators.js -ItemType File

code .
```

> **Importante:** separamos `app.js` (la app Express) de `server.js` (el que escucha el puerto). Esto es clave para poder probar la app con Supertest **sin levantar puerto** (más rápido y sin choques).

### Paso 3. Funciones puras — `src/utils/validators.js`

Estas funciones son ideales para **pruebas unitarias** porque no dependen de Express ni de datos.

```js
function esTituloValido(titulo) {
  if (typeof titulo !== "string") return false;
  const limpio = titulo.trim();
  return limpio.length >= 3 && limpio.length <= 100;
}

function normalizarPrioridad(prioridad) {
  const valor = String(prioridad || "media").toLowerCase().trim();
  const validas = ["baja", "media", "alta"];
  return validas.includes(valor) ? valor : "media";
}

function calcularResumen(tareas) {
  const total = tareas.length;
  const completadas = tareas.filter(t => t.completada).length;
  const pendientes = total - completadas;
  return { total, completadas, pendientes };
}

module.exports = {
  esTituloValido,
  normalizarPrioridad,
  calcularResumen
};
```

### Paso 4. Servicio en memoria — `src/services/tareas.service.js`

```js
let tareas = [];
let siguienteId = 1;

function reset() {
  tareas = [];
  siguienteId = 1;
}

function listar() {
  return [...tareas];
}

function obtener(id) {
  return tareas.find(t => t.id === Number(id)) || null;
}

function crear({ titulo, prioridad }) {
  const nueva = {
    id: siguienteId++,
    titulo: titulo.trim(),
    prioridad,
    completada: false,
    creadaEn: new Date().toISOString()
  };
  tareas.push(nueva);
  return nueva;
}

function actualizarEstado(id, completada) {
  const tarea = obtener(id);
  if (!tarea) return null;
  tarea.completada = Boolean(completada);
  return tarea;
}

function eliminar(id) {
  const antes = tareas.length;
  tareas = tareas.filter(t => t.id !== Number(id));
  return tareas.length < antes;
}

module.exports = {
  reset,
  listar,
  obtener,
  crear,
  actualizarEstado,
  eliminar
};
```

### Paso 5. Rutas REST — `src/routes/tareas.routes.js`

```js
const express = require("express");
const servicio = require("../services/tareas.service");
const { esTituloValido, normalizarPrioridad, calcularResumen } = require("../utils/validators");

const router = express.Router();

router.get("/", (req, res) => {
  res.status(200).json(servicio.listar());
});

router.get("/resumen", (req, res) => {
  res.status(200).json(calcularResumen(servicio.listar()));
});

router.get("/:id", (req, res) => {
  const tarea = servicio.obtener(req.params.id);
  if (!tarea) return res.status(404).json({ error: "Tarea no encontrada" });
  res.status(200).json(tarea);
});

router.post("/", (req, res) => {
  const { titulo, prioridad } = req.body || {};

  if (!esTituloValido(titulo)) {
    return res.status(400).json({ error: "Título inválido (3 a 100 caracteres)" });
  }

  const prioridadFinal = normalizarPrioridad(prioridad);
  const nueva = servicio.crear({ titulo, prioridad: prioridadFinal });
  res.status(201).json(nueva);
});

router.put("/:id", (req, res) => {
  const { completada } = req.body || {};
  const actualizada = servicio.actualizarEstado(req.params.id, completada);
  if (!actualizada) return res.status(404).json({ error: "Tarea no encontrada" });
  res.status(200).json(actualizada);
});

router.delete("/:id", (req, res) => {
  const eliminada = servicio.eliminar(req.params.id);
  if (!eliminada) return res.status(404).json({ error: "Tarea no encontrada" });
  res.status(204).send();
});

module.exports = router;
```

### Paso 6. App Express — `src/app.js`

```js
const express = require("express");
const tareasRoutes = require("./routes/tareas.routes");

const app = express();

app.use(express.json());

app.get("/health", (req, res) => {
  res.status(200).json({ status: "ok", message: "API operativa" });
});

app.use("/api/tareas", tareasRoutes);

app.use((req, res) => {
  res.status(404).json({ error: "Ruta no encontrada" });
});

module.exports = app;
```

### Paso 7. Server — `src/server.js`

```js
const app = require("./app");
const PORT = 3000;

app.listen(PORT, () => {
  console.log(`API tareas en http://localhost:${PORT}`);
});
```

### Paso 8. Scripts en `package.json`

Reemplaza el bloque `scripts` por:

```json
"scripts": {
  "dev": "nodemon src/server.js",
  "start": "node src/server.js",
  "test": "jest --runInBand"
}
```

> `--runInBand` evita que Jest ejecute tests en paralelo. En este bonus usamos un estado en memoria compartido, así que los tests deben correr en serie para no pisarse.

### Paso 9. Levantar la API para verificar

```powershell
npm run dev
```

En otra ventana:

```powershell
Invoke-RestMethod -Uri http://localhost:3000/health
```

**Checkpoint 1 aprobado** cuando `/health` responde `status: ok`.

> Detén el `npm run dev` antes de seguir. Los tests levantarán la app por su cuenta cuando los necesiten.

---

## Bloque 2 — Pruebas Unitarias + Pruebas de Humo (20 minutos)

### 2.1 Prueba Unitaria

**¿Qué es?** Una prueba que valida **una función o unidad pequeña en aislamiento total**. No toca red, ni base de datos, ni framework. Si falla, sabes exactamente dónde.

**¿Por qué importan?** Son rápidas, baratas y precisas. Son la base de la pirámide de pruebas.

### Paso 10. Crear `tests/unit.validators.test.js`

```js
const {
  esTituloValido,
  normalizarPrioridad,
  calcularResumen
} = require("../src/utils/validators");

describe("Unitario - validators", () => {
  describe("esTituloValido", () => {
    test("acepta título dentro del rango permitido", () => {
      expect(esTituloValido("Comprar leche")).toBe(true);
    });

    test("rechaza título muy corto", () => {
      expect(esTituloValido("ok")).toBe(false);
    });

    test("rechaza valores no string", () => {
      expect(esTituloValido(null)).toBe(false);
      expect(esTituloValido(123)).toBe(false);
    });

    test("trata como inválido un título con solo espacios", () => {
      expect(esTituloValido("    ")).toBe(false);
    });
  });

  describe("normalizarPrioridad", () => {
    test("acepta valores válidos", () => {
      expect(normalizarPrioridad("alta")).toBe("alta");
      expect(normalizarPrioridad("BAJA")).toBe("baja");
    });

    test("cae a 'media' si el valor es inválido o vacío", () => {
      expect(normalizarPrioridad("urgente")).toBe("media");
      expect(normalizarPrioridad(undefined)).toBe("media");
    });
  });

  describe("calcularResumen", () => {
    test("cuenta correctamente completadas y pendientes", () => {
      const tareas = [
        { completada: true },
        { completada: false },
        { completada: true }
      ];
      expect(calcularResumen(tareas)).toEqual({
        total: 3,
        completadas: 2,
        pendientes: 1
      });
    });
  });
});
```

### Paso 11. Ejecutar solo las unitarias

```powershell
npx jest tests/unit.validators.test.js
```

### Resultado esperado

Todos los tests en verde (PASS). Si alguno falla, revisa primero la función real en `validators.js`, no el test.

### 2.2 Prueba de Humo (Smoke Test)

**¿Qué es?** Una prueba **muy rápida y mínima** que verifica que el sistema "prende". No prueba lógica de negocio: solo confirma que arrancó y responde.

**¿Cuándo se usa?** En CI, justo después del deploy, antes de correr el resto de pruebas. Si el smoke falla, no tiene sentido correr nada más.

### Paso 12. Crear `tests/smoke.test.js`

```js
const request = require("supertest");
const app = require("../src/app");

describe("Smoke - la API responde lo mínimo", () => {
  test("GET /health responde 200 y status ok", async () => {
    const res = await request(app).get("/health");
    expect(res.status).toBe(200);
    expect(res.body.status).toBe("ok");
  });

  test("GET /api/tareas responde 200 con un array", async () => {
    const res = await request(app).get("/api/tareas");
    expect(res.status).toBe(200);
    expect(Array.isArray(res.body)).toBe(true);
  });

  test("Ruta inexistente responde 404 controlado", async () => {
    const res = await request(app).get("/no-existe");
    expect(res.status).toBe(404);
    expect(res.body.error).toBeDefined();
  });
});
```

### Paso 13. Ejecutar smoke

```powershell
npx jest tests/smoke.test.js
```

**Checkpoint 2 aprobado** cuando ambos archivos están en verde y entiendes la diferencia: el unitario prueba **una función**, el smoke prueba **que el sistema vive**.

---

## Bloque 3 — Pruebas de Integración + Funcionales/E2E (25 minutos)

### 3.1 Prueba de Integración

**¿Qué es?** Verifica que **dos o más capas** del sistema funcionan juntas: ruta + servicio + persistencia, o servicio + validador. Ya no es aislamiento total.

**¿Por qué se distingue del unitario?** Porque si falla, no sabes exactamente cuál capa rompió. A cambio, descubre errores de contrato entre capas que las unitarias no ven.

### Paso 14. Crear `tests/integration.tareas.test.js`

```js
const request = require("supertest");
const app = require("../src/app");
const servicio = require("../src/services/tareas.service");

describe("Integración - ruta + servicio + validador", () => {
  beforeEach(() => {
    servicio.reset();
  });

  test("POST /api/tareas crea y persiste en el servicio", async () => {
    const res = await request(app)
      .post("/api/tareas")
      .send({ titulo: "Estudiar para defensa", prioridad: "alta" });

    expect(res.status).toBe(201);
    expect(res.body.id).toBe(1);
    expect(res.body.prioridad).toBe("alta");

    // Verificamos contra la capa de servicio (otra capa)
    expect(servicio.listar()).toHaveLength(1);
  });

  test("POST con título inválido NO toca el servicio", async () => {
    const res = await request(app)
      .post("/api/tareas")
      .send({ titulo: "ab" });

    expect(res.status).toBe(400);
    expect(servicio.listar()).toHaveLength(0);
  });

  test("GET /api/tareas/:id devuelve la tarea creada", async () => {
    servicio.crear({ titulo: "Tarea base", prioridad: "media" });

    const res = await request(app).get("/api/tareas/1");

    expect(res.status).toBe(200);
    expect(res.body.titulo).toBe("Tarea base");
  });

  test("GET /api/tareas/resumen agrega correctamente", async () => {
    servicio.crear({ titulo: "Una", prioridad: "media" });
    servicio.crear({ titulo: "Dos", prioridad: "media" });
    servicio.actualizarEstado(1, true);

    const res = await request(app).get("/api/tareas/resumen");

    expect(res.status).toBe(200);
    expect(res.body).toEqual({ total: 2, completadas: 1, pendientes: 1 });
  });
});
```

### Paso 15. Ejecutar integración

```powershell
npx jest tests/integration.tareas.test.js
```

### 3.2 Prueba Funcional / End-to-End de API

**¿Qué es?** Verifica un **flujo completo del negocio** simulando lo que haría un usuario real consumiendo la API. Aquí no nos importa qué capa hace qué — nos importa el caso de uso.

> Nota: cuando hay UI (React, móvil), las pruebas E2E se hacen con Cypress o Playwright. Como esta actividad es solo backend, hacemos E2E **a nivel de API** con Supertest. El criterio es el mismo: probar el flujo completo de punta a punta.

### Paso 16. Crear `tests/e2e.flujo.test.js`

```js
const request = require("supertest");
const app = require("../src/app");
const servicio = require("../src/services/tareas.service");

describe("E2E - flujo completo del usuario", () => {
  beforeAll(() => {
    servicio.reset();
  });

  test("usuario crea, lista, marca completada y elimina una tarea", async () => {
    // 1. Lista vacía al inicio
    let res = await request(app).get("/api/tareas");
    expect(res.status).toBe(200);
    expect(res.body).toEqual([]);

    // 2. Crea una tarea
    res = await request(app)
      .post("/api/tareas")
      .send({ titulo: "Preparar defensa de título", prioridad: "alta" });
    expect(res.status).toBe(201);
    const id = res.body.id;

    // 3. La encuentra en el listado
    res = await request(app).get("/api/tareas");
    expect(res.body).toHaveLength(1);

    // 4. La marca como completada
    res = await request(app)
      .put(`/api/tareas/${id}`)
      .send({ completada: true });
    expect(res.status).toBe(200);
    expect(res.body.completada).toBe(true);

    // 5. El resumen refleja el cambio
    res = await request(app).get("/api/tareas/resumen");
    expect(res.body.completadas).toBe(1);

    // 6. La elimina
    res = await request(app).delete(`/api/tareas/${id}`);
    expect(res.status).toBe(204);

    // 7. Ya no aparece
    res = await request(app).get(`/api/tareas/${id}`);
    expect(res.status).toBe(404);
  });
});
```

### Paso 17. Ejecutar E2E

```powershell
npx jest tests/e2e.flujo.test.js
```

**Checkpoint 3 aprobado** cuando integración y E2E están verdes y puedes explicar:

- la integración prueba **que dos capas se entienden**;
- el E2E prueba **un caso de uso completo del cliente**.

---

## Bloque 4 — Pruebas de Regresión + Seguridad (25 minutos)

### 4.1 Prueba de Regresión

**¿Qué es?** Es una prueba que **nace de un bug real ya arreglado**. Su propósito es asegurar que ese bug **no vuelva a aparecer** en el futuro.

**No es un "tipo nuevo" técnicamente:** un test de regresión puede ser unitario, de integración o E2E. Lo que lo hace de regresión es su **historia**: existe porque algo se rompió antes.

### Paso 18. Reproducir un bug a propósito

Supongamos que un compañero descubrió este bug en QA:

> "Si mando `prioridad` como número, el sistema lo acepta como prioridad válida y luego rompe el listado."

Vamos a verificarlo. **Primero**, edita temporalmente `src/routes/tareas.routes.js` para introducir el bug (comenta la línea de normalización):

```js
// const prioridadFinal = normalizarPrioridad(prioridad);
const prioridadFinal = prioridad; // BUG introducido a propósito
```

Reinicia los tests y observa: el flujo seguirá pasando, pero ahora puedes crear una tarea con `prioridad: 99`.

### Paso 19. Restaurar el código correcto

Vuelve a dejar:

```js
const prioridadFinal = normalizarPrioridad(prioridad);
```

### Paso 20. Escribir la prueba de regresión — `tests/regression.prioridad.test.js`

```js
const request = require("supertest");
const app = require("../src/app");
const servicio = require("../src/services/tareas.service");

/**
 * Regresión BUG-001:
 * Si el cliente envía 'prioridad' con un valor inesperado,
 * la API debe normalizar a 'media' y nunca aceptar el valor crudo.
 *
 * Bug original: la ruta no llamaba a normalizarPrioridad.
 * Esta prueba evita que el bug vuelva si alguien refactoriza la ruta.
 */
describe("Regresión - BUG-001 normalización de prioridad", () => {
  beforeEach(() => servicio.reset());

  test("prioridad numérica se normaliza a 'media'", async () => {
    const res = await request(app)
      .post("/api/tareas")
      .send({ titulo: "Tarea con prioridad rara", prioridad: 99 });

    expect(res.status).toBe(201);
    expect(res.body.prioridad).toBe("media");
  });

  test("prioridad inventada se normaliza a 'media'", async () => {
    const res = await request(app)
      .post("/api/tareas")
      .send({ titulo: "Otra tarea", prioridad: "urgentísima" });

    expect(res.status).toBe(201);
    expect(res.body.prioridad).toBe("media");
  });
});
```

### Paso 21. Ejecutar la regresión

```powershell
npx jest tests/regression.prioridad.test.js
```

**Tip de portafolio:** en la defensa puedes mostrar el commit donde se introdujo el bug, el commit donde se arregló, y el test que evita que vuelva. Eso vale mucho.

### 4.2 Prueba de Seguridad

**¿Qué es?** Verifica que el sistema **no tenga vulnerabilidades evidentes**: dependencias con CVE conocidos, rutas que aceptan datos peligrosos, falta de validación en servidor, malas configuraciones.

En este bonus haremos **dos** verificaciones:

1. **Estática (SCA):** `npm audit` sobre las dependencias.
2. **Dinámica (input validation):** Supertest enviando payloads maliciosos.

### Paso 22. Auditoría de dependencias

```powershell
npm audit --omit=dev > evidencias\npm-audit.txt
```

Abre `evidencias\npm-audit.txt` y revisa:

- ¿Cuántos hallazgos hay? ¿Qué severidad tienen?
- Si hay vulnerabilidades, ¿`npm audit fix` las resuelve?

**Criterio mínimo:** 0 hallazgos `high` o `critical`. Si los hay, ejecutar `npm audit fix` y volver a auditar.

### Paso 23. Prueba dinámica — `tests/security.input.test.js`

```js
const request = require("supertest");
const app = require("../src/app");
const servicio = require("../src/services/tareas.service");

describe("Seguridad - validación de entrada en servidor", () => {
  beforeEach(() => servicio.reset());

  test("rechaza payload sin campos requeridos", async () => {
    const res = await request(app).post("/api/tareas").send({});
    expect(res.status).toBe(400);
  });

  test("no confía en validación cliente: rechaza título muy largo", async () => {
    const tituloEnorme = "x".repeat(500);
    const res = await request(app)
      .post("/api/tareas")
      .send({ titulo: tituloEnorme });
    expect(res.status).toBe(400);
  });

  test("acepta intento de XSS como texto plano pero NO lo ejecuta ni lo refleja con HTML", async () => {
    const payloadXSS = "<script>alert('xss')</script>";
    // Nuestro endpoint guardará el texto tal cual, pero responde JSON, no HTML.
    // La defensa real es no renderizar como HTML en el front; aquí validamos
    // que el backend NO interprete el script (lo trata como string).
    const res = await request(app)
      .post("/api/tareas")
      .send({ titulo: payloadXSS, prioridad: "media" });

    expect(res.status).toBe(201);
    expect(typeof res.body.titulo).toBe("string");
    // El header debe ser JSON, no HTML — eso evita ejecución en browser
    expect(res.headers["content-type"]).toMatch(/application\/json/);
  });

  test("rechaza JSON malformado con 400 controlado", async () => {
    const res = await request(app)
      .post("/api/tareas")
      .set("Content-Type", "application/json")
      .send('{"titulo": "rota');

    expect(res.status).toBe(400);
  });
});
```

### Paso 24. Ejecutar seguridad

```powershell
npx jest tests/security.input.test.js
```

**Checkpoint 4 aprobado** cuando:

- regresión está en verde;
- `evidencias\npm-audit.txt` existe;
- las pruebas de seguridad están en verde.

---

## Bloque 5 — Prueba de Carga con k6 (15 minutos)

### 5.1 ¿Qué es una prueba de carga?

Mide **cómo se comporta el sistema bajo concurrencia**. No te dice si la lógica es correcta — eso ya lo cubren los otros tipos. Te dice si **escala** y dónde aparece el cuello de botella.

Métricas típicas:

- **p95** (percentil 95 de latencia): el 95% de las requests responden bajo este tiempo.
- **error rate:** porcentaje de respuestas con error.
- **rps:** requests por segundo.

### Paso 25. Instalar k6 en Windows

Opción rápida con `winget`:

```powershell
winget install k6 --source winget
```

Si `winget` no está disponible, descarga desde https://k6.io/docs/get-started/installation/.

Verifica:

```powershell
k6 version
```

### Paso 26. Crear `loadtest.js` en la raíz del proyecto

```js
import http from "k6/http";
import { check, sleep } from "k6";

export const options = {
  stages: [
    { duration: "20s", target: 10 },  // sube a 10 usuarios
    { duration: "40s", target: 10 },  // mantiene 10 usuarios
    { duration: "10s", target: 0 }    // baja a 0
  ],
  thresholds: {
    http_req_duration: ["p(95)<800"],  // p95 < 800 ms
    http_req_failed:   ["rate<0.01"]    // < 1% de errores
  }
};

export default function () {
  const resHealth = http.get("http://localhost:3000/health");
  check(resHealth, {
    "health 200": (r) => r.status === 200
  });

  const resLista = http.get("http://localhost:3000/api/tareas");
  check(resLista, {
    "listar 200": (r) => r.status === 200
  });

  sleep(1);
}
```

### Paso 27. Ejecutar la carga

En una ventana, levanta la API:

```powershell
npm run dev
```

En otra ventana, corre k6:

```powershell
k6 run loadtest.js > evidencias\k6-resumen.txt
```

### Resultado esperado

Al finalizar, k6 muestra un resumen. Confirma que se cumplen los umbrales:

- `http_req_duration p(95)` por debajo de 800 ms;
- `http_req_failed` por debajo de 1%.

Si los umbrales fallan, k6 saldrá con código distinto a 0. En la matriz, registra eso como `FAIL` y discute la causa probable (¿IO, JSON parsing, lectura de archivo?).

**Checkpoint 5 aprobado** cuando `evidencias\k6-resumen.txt` contiene el reporte de k6.

> Si no puedes instalar k6 en clase, alternativa rápida con Node puro: `npx autocannon -c 10 -d 60 http://localhost:3000/health > evidencias\autocannon.txt`. Pierde algo de detalle de thresholds, pero sirve como evidencia.

---

## Bloque 6 — Matriz consolidada, evidencias y cierre (10 minutos)

### Paso 28. Ejecutar TODAS las pruebas Jest juntas

```powershell
npm test
```

Esto corre `unit.*`, `integration.*`, `e2e.*`, `regression.*`, `security.*` y `smoke.*` en una sola pasada. Guarda el resultado:

```powershell
npm test > evidencias\jest-resumen.txt 2>&1
```

### Paso 29. Completar la matriz consolidada

Crea un archivo `evidencias\matriz-pruebas.md` con esta tabla:

| ID | Tipo | Archivo / herramienta | Qué valida | Resultado | Evidencia |
|---|---|---|---|---|---|
| T-01 | Unitaria | `tests/unit.validators.test.js` | Lógica pura de validadores y resumen | PASS / FAIL | jest-resumen.txt |
| T-02 | Humo | `tests/smoke.test.js` | La API arranca y responde lo mínimo | PASS / FAIL | jest-resumen.txt |
| T-03 | Integración | `tests/integration.tareas.test.js` | Ruta + servicio + validador trabajan juntos | PASS / FAIL | jest-resumen.txt |
| T-04 | E2E / Funcional | `tests/e2e.flujo.test.js` | Flujo completo crear → marcar → eliminar | PASS / FAIL | jest-resumen.txt |
| T-05 | Regresión | `tests/regression.prioridad.test.js` | BUG-001 no reaparece (prioridad inválida) | PASS / FAIL | jest-resumen.txt |
| T-06 | Seguridad | `npm audit` + `tests/security.input.test.js` | Sin CVEs altos + validación servidor | PASS / FAIL | npm-audit.txt + jest-resumen.txt |
| T-07 | Carga | `loadtest.js` (k6) | p95 < 800 ms y error rate < 1% con 10 VUs | PASS / FAIL | k6-resumen.txt |

### Paso 30. Reflexión final (escribe en `evidencias\conclusion.md`)

Responde brevemente:

1. ¿Qué diferencia práctica viste entre la unitaria de `validators` y la integración de la ruta?
2. ¿Qué bug detectó tu prueba de regresión que un unitario nunca habría visto?
3. ¿Qué tipo de prueba te dio **menos** confianza para tu defensa? ¿Por qué?
4. Si tuvieras que dejar **solo 3 tipos** en tu pipeline de CI, ¿cuáles dejarías y por qué?

**Checkpoint 6 aprobado** cuando:

- `npm test` termina en verde;
- la matriz está completa con 7 filas;
- existe la reflexión final.

---

## 8. Tipos de prueba adicionales — cuándo aplicarlos

Estos NO son obligatorios para el bonus, pero deberías reconocerlos y poder mencionarlos en tu defensa si el evaluador pregunta.

| Tipo | Cuándo aplicarlo | Herramienta sugerida |
|---|---|---|
| **De aceptación (UAT)** | Cuando el cliente o Product Owner valida el producto contra criterios del negocio. | Demo guiada + checklist firmada |
| **De contrato** | Cuando dos servicios se hablan vía API y necesitas asegurar que el contrato no rompa. | Pact, Postman + JSON schema |
| **De mutación** | Cuando quieres medir la **calidad** de tus tests (no del código). Introduce mutaciones y verifica que los tests las detecten. | Stryker |
| **Snapshot** | Cuando quieres detectar cambios no intencionados en una salida (HTML, JSON grande). | Jest `toMatchSnapshot()` |
| **De accesibilidad (a11y)** | Cuando hay UI. Mide contraste, ARIA, navegación por teclado. | Lighthouse, axe-core |
| **De usabilidad** | Cuando quieres validar que un usuario real puede completar tareas sin instrucciones. | Pruebas con usuarios + cronómetro |
| **Exploratorias** | Cuando el sistema es nuevo y no sabes qué romper aún. No automatizadas. | Sesiones cronometradas + bitácora |
| **De estrés** | Variante de carga: empujar hasta romper para saber dónde está el límite. | k6, JMeter |
| **De recuperación / chaos** | Verificar que el sistema vuelve a funcionar tras una caída forzada. | Toxiproxy, chaos-monkey |

---

## 9. Entregables

Cada estudiante o equipo debe entregar:

### Carpeta de trabajo

```
api-tareas-bonus/
├── src/
│   ├── app.js
│   ├── server.js
│   ├── routes/
│   ├── services/
│   └── utils/
├── tests/
│   ├── unit.validators.test.js
│   ├── smoke.test.js
│   ├── integration.tareas.test.js
│   ├── e2e.flujo.test.js
│   ├── regression.prioridad.test.js
│   └── security.input.test.js
├── loadtest.js
├── evidencias/
│   ├── jest-resumen.txt
│   ├── npm-audit.txt
│   ├── k6-resumen.txt
│   ├── matriz-pruebas.md
│   └── conclusion.md
└── package.json
```

### Evidencias mínimas obligatorias

- captura de `npm test` con todos los tests verdes;
- captura del reporte de k6 mostrando los thresholds;
- archivo `npm-audit.txt` con el resumen de auditoría;
- matriz consolidada completada;
- reflexión final.

---

## 10. Criterios de logro

Se espera que el estudiante:

- implemente al menos **una prueba de cada uno de los 7 tipos** trabajados;
- pueda **explicar la diferencia** entre cada tipo y cuándo aplicar cada uno;
- demuestre **evidencia trazable** de cada ejecución;
- justifique **decisiones técnicas** (por qué `--runInBand`, por qué separar `app.js` de `server.js`, por qué la prueba de regresión existe);
- relacione lo aprendido con el **Plan de Pruebas del Ejercicio 5** (qué tipo cubre qué umbral del plan).

---

## 11. Desafío opcional para quienes terminen antes

### Opción A — Cobertura

Agrega `jest --coverage` al script de test y reporta el porcentaje de líneas cubiertas. Meta sugerida: ≥ 80% en `src/utils` y `src/services`.

```json
"test:cov": "jest --coverage --runInBand"
```

### Opción B — Snapshot

Agrega un test de snapshot para la respuesta de `GET /api/tareas/resumen` con 3 tareas conocidas. Si la forma de la respuesta cambia, el test detectará la regresión visual del JSON.

### Opción C — Contrato

Define un JSON schema para la tarea (con `ajv`) y agrega un test que valide que toda respuesta de `POST /api/tareas` cumple el schema. Si un día el backend cambia el nombre del campo, el test caerá inmediatamente.

### Opción D — CI

Crea `.github/workflows/tests.yml` que ejecute `npm test` y `npm audit` en cada push. Adjunta el badge al README.

---

## 12. Reglas mínimas de la actividad

- Cada tipo de prueba debe estar en su **propio archivo** (no mezclar unitarias con integración en un mismo file).
- Cada archivo de tests debe tener un `describe` cuyo nombre indique **claramente el tipo** (`Unitario - …`, `Integración - …`, etc.).
- Las pruebas que modifican estado deben llamar a `servicio.reset()` en `beforeEach` para no contaminar a las siguientes.
- Las pruebas no pueden depender del orden de ejecución entre archivos.
- Todo hallazgo en `npm audit` con severidad **alta o crítica** debe quedar mitigado o explicado en la conclusión.

---

## 13. Errores frecuentes y cómo evitarlos

| Síntoma | Causa probable | Solución |
|---|---|---|
| `Cannot find module 'supertest'` | Falta instalar | `npm install -D supertest` |
| Tests pasan solos pero fallan en `npm test` | Estado compartido entre archivos | Usar `--runInBand` y `servicio.reset()` |
| `EADDRINUSE: address already in use :::3000` al correr tests | Algún test importó `server.js` en vez de `app.js` | Importar siempre `../src/app`, nunca `server.js` |
| k6 reporta timeout | Olvidaste levantar `npm run dev` antes de `k6 run` | Una ventana corre la API, otra corre k6 |
| `npm audit` muestra muchas vulnerabilidades en dev | Estás contando dependencias de Jest | Usar `npm audit --omit=dev` para auditar solo producción |

---

## 14. Cierre de la actividad

Al terminar, además de los entregables, prepárate para responder en defensa:

1. ¿Cuál es la diferencia conceptual entre una prueba unitaria y una de integración?
2. ¿Por qué una prueba E2E no reemplaza a las unitarias?
3. ¿En qué momento del ciclo de desarrollo agregaste el test de regresión y por qué?
4. ¿Qué decisiones tomaste cuando `npm audit` reportó hallazgos?
5. ¿Qué te dice un p95 de 800 ms en tu prueba de carga? ¿Y si fuera de 3 segundos?
6. Si tuvieras que defender esta calidad ante un cliente, ¿qué evidencia mostrarías primero?

---

## 15. Resumen rápido para estudiantes

Construye una mini API de tareas con Express, configura Jest + Supertest, y aplica **al menos una prueba de cada uno de estos 7 tipos**: unitaria, humo, integración, E2E/funcional, regresión, seguridad y carga (con k6). Deja evidencia de cada ejecución, completa la matriz consolidada y escribe una reflexión final. Esta actividad complementa el Ejercicio 5 (Plan de Pruebas): allí definiste *qué* probar; aquí lo *ejecutas* en código real.

---

**Fin del documento.**
