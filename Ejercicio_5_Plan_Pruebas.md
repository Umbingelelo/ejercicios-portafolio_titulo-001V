# Plan de Pruebas (TPY1101) — Guía detallada (React + Node.js)

**Objetivo:** que tu equipo diseñe y ejecute un **plan de pruebas completo**, con foco en **pruebas funcionales** y **no funcionales**, dejando **evidencia verificable** (reportes, capturas, logs y/o resultados automáticos).

> Esta guía está pensada para proyectos web con **Frontend React** (frecuente: Vite) + **Backend Node.js/Express** + BD (frecuente: Supabase/PostgreSQL), pero aplica a cualquier stack similar.

---

## 1) Qué es un Plan de Pruebas y por qué importa

Un **Plan de Pruebas** es el documento (y su evidencia asociada) que define:

- **Qué** se prueba (alcance / requisitos).
- **Cómo** se prueba (estrategia, tipos de prueba, herramientas).
- **Dónde** se prueba (ambientes y datos).
- **Cuándo** y **quién** ejecuta (roles y calendario).
- **Con qué criterios** se acepta o rechaza (umbrales mínimos / criterios de salida).
- **Qué evidencia** se genera para demostrar calidad y cumplimiento.

---

## 2) Entregables mínimos que deben existir

### 2.1 Documentos / archivos mínimos
1. **Plan de Pruebas** (este documento completado).
2. **Matriz de Casos de Prueba** (tabla) con:
   - ID
   - Historia de usuario
   - Criterio de aceptación
   - Tipo de prueba
   - Prioridad
   - Pasos y resultado esperado
   - Estado y evidencia
3. **Reporte de ejecución** (manual o automático).
4. **Evidencia** (capturas, reportes, enlaces a pipelines, logs, resultados de herramientas).

### 2.2 Criterios mínimos recomendados (tresholds globales)
Estos son **umbrales mínimos** para que tu plan sea considerado “sólido”:

- **Funcional (mínimo):**
  - 100% de los **flujos críticos** con estado **PASS** (login/registro si existe, CRUD principal, permisos, flujo core del negocio).
  - 0 **defectos Críticos/Altos** abiertos al cierre.
  - Para historias de usuario: idealmente **≥ 1 caso de prueba por cada criterio de aceptación**.

- **No funcional (mínimo):**
  - **Performance:** p95 de endpoints críticos **≤ 800 ms** (ambiente de prueba, carga moderada).
  - **Seguridad:** 0 hallazgos **Críticos/Altos** en verificación básica (dependencias, headers, auth, acceso).
  - **Accesibilidad:** Lighthouse **Accessibility ≥ 80** en las pantallas principales.
  - **Estabilidad:** error rate global **< 1%** en pruebas de carga moderada.

> Si tu proyecto es pequeño, ajusta la carga, pero mantén el **principio**: medir, evidenciar y justificar.

---

## 3) Alcance: qué se prueba (y qué NO)

### 3.1 En alcance (típico)
- Autenticación (si aplica): login, registro, recuperación, sesiones/tokens.
- Autorización/roles (si aplica): acceso por perfil.
- CRUD y lógica de negocio (principal).
- Integraciones: API externa / Supabase / almacenamiento / correo, etc.
- Validaciones de formularios (cliente y servidor).
- Manejo de errores: 4xx/5xx controlados, mensajes adecuados.
- Seguridad básica: control de acceso, sanitización, rate limit (si aplica), headers.

### 3.2 Fuera de alcance (ejemplos)
- Pruebas de penetración avanzadas (pentesting profundo).
- Certificaciones formales.
- Pruebas con hardware especializado.

---

## 4) Estrategia de pruebas para React + Node.js

### 4.1 Pirámide de pruebas (recomendada)
1. **Unitarias** (rápidas y muchas)
   - React: componentes y utilidades.
   - Node: servicios, validaciones, helpers.
2. **Integración**
   - API con BD (o BD mock).
   - Front con API mock (MSW).
3. **End-to-End (E2E)** (menos, pero críticas)
   - Flujos completos en UI (Cypress/Playwright).
4. **No funcionales**
   - Performance (k6/Artillery).
   - Accesibilidad (Lighthouse).
   - Seguridad (ZAP + npm audit).

> En proyectos académicos, lo más valorado es demostrar **criterios de aceptación + evidencia + calidad**.

---

## 5) Herramientas recomendadas (elige según tu proyecto)

### 5.1 Frontend (React)
- **Vitest** (si usas Vite) o **Jest**: unitarias.
- **React Testing Library**: pruebas de comportamiento del componente (render, interacción, estado).
- **MSW (Mock Service Worker)**: simular API en tests sin depender del backend real.
- **Cypress** o **Playwright**: E2E y pruebas funcionales en navegador.
- **Lighthouse** (Chrome DevTools): performance, accessibility, best practices, SEO.

### 5.2 Backend (Node.js / Express)
- **Jest/Vitest**: unitarias.
- **Supertest**: testear endpoints Express.
- **Postman** + **Newman**: colecciones de API y ejecución por consola (y CI).
- **k6** o **Artillery**: carga y performance.
- **OWASP ZAP**: escaneo básico de seguridad (DAST).
- **npm audit** (y opcional **Snyk**): vulnerabilidades de dependencias.

### 5.3 Gestión y evidencia
- GitHub Actions / GitLab CI: ejecutar tests y adjuntar reportes.
- Markdown + tablas (Google Sheets/Excel): matriz de casos.
- Capturas (PNG) / videos (E2E): evidencia.

---

## 6) Cómo construir tu Plan de Pruebas (paso a paso)

### Paso 1 — Lista historias de usuario y criterios de aceptación
- Tener el listado de historias (ideal: 6–8) con **criterios de aceptación claros**.
- Cada criterio debe ser **testeable** (observable, medible).

**Checklist:**
- ¿El criterio dice algo verificable? (ej. “Debe validar email” ✅ / “Debe ser bonito” ❌)
- ¿Tiene condiciones y resultado esperado?

---

### Paso 2 — Define la matriz de pruebas
Crea una tabla con estas columnas (mínimo):

| ID | Historia | Criterio de aceptación | Tipo (F/NF) | Nivel (U/INT/E2E) | Prioridad | Precondición | Pasos | Resultado esperado | Umbral mínimo | Estado | Evidencia |
|---|---|---|---|---|---|---|---|---|---|---|---|

**Prioridad sugerida:**
- P0: crítico (si falla, el sistema no cumple el objetivo).
- P1: importante.
- P2: deseable.

---

### Paso 3 — Define ambientes y datos de prueba
**Ambientes recomendados:**
- **DEV:** para desarrollo.
- **TEST/UAT:** datos de prueba y ejecución formal.
- **PROD:** NO ejecutar pruebas destructivas.

**Datos de prueba mínimos:**
- Usuarios: admin / usuario normal.
- Registros: datos válidos e inválidos.
- Casos borde: campos vacíos, límites de longitud, caracteres especiales, duplicados.

---

### Paso 4 — Ejecuta y captura evidencia
- Para pruebas manuales: captura pantalla + fecha + ID de caso.
- Para automáticas: adjunta reporte (consola, HTML, JSON) y link al pipeline.

---

### Paso 5 — Gestiona defectos y re-pruebas
Registra defectos con:
- ID, severidad (Crítica/Alta/Media/Baja),
- pasos para reproducir,
- evidencia,
- estado (Open / Fixed / Retest / Closed).

---

## 7) Pruebas funcionales (F) — qué incluir sí o sí

### 7.1 Tipos funcionales mínimos
- **Validación de formularios** (front y back).
- **Flujos core** del negocio (E2E).
- **CRUD principal** (crear, leer, actualizar, eliminar).
- **Permisos/roles** (si aplica).
- **Manejo de errores** (mensajes y códigos correctos).

### 7.2 Umbrales mínimos por caso funcional
Para declarar un caso funcional “PASS” debe cumplirse:

- El **resultado observado** coincide 100% con el **resultado esperado**.
- La respuesta no produce errores en consola (frontend) ni excepciones no controladas (backend).
- Si el caso afecta BD: el cambio queda persistido (o se revierte si corresponde).

**Treshold funcional recomendado:**
- **P0:** 100% PASS obligatorio.
- **P1:** ≥ 90% PASS.
- **P2:** ≥ 80% PASS.

---

## 8) Pruebas no funcionales (NF) — qué medir y mínimos

Las NF demuestran calidad más allá de “funciona o no”.

### 8.1 Performance (rendimiento)
**Qué medir:**
- Tiempo de respuesta API (p95).
- Tiempo de carga UI.
- Consumo razonable de recursos (sin leaks obvios).

**Umbrales mínimos recomendados (ambiente TEST):**
- API endpoints críticos:
  - **p95 ≤ 800 ms** con **20 usuarios concurrentes** durante **2–3 minutos**
  - **error rate < 1%**
- UI:
  - Lighthouse **Performance ≥ 70** en pantallas principales

**Herramientas:**
- k6 / Artillery para API.
- Lighthouse para UI.

---

### 8.2 Seguridad (básica)
**Qué verificar:**
- Autenticación/Autorización (no exponer datos sin permiso).
- Validaciones del lado servidor (no confiar en el front).
- Dependencias vulnerables.
- Configuración básica (headers, CORS razonable).

**Umbrales mínimos:**
- 0 hallazgos **Críticos/Altos** en:
  - `npm audit` (o justificar mitigación)
  - revisión de rutas privadas sin auth
- Autorización:
  - un usuario **NO** puede acceder a recursos de otro (si aplica)
- Contraseñas:
  - mínimo 8 caracteres + 1 mayúscula + 1 número (recomendado, ajustable)

**Herramientas:**
- npm audit / Snyk.
- OWASP ZAP (escaneo básico).
- Postman para intentos de acceso no autorizado.

---

### 8.3 Usabilidad (básica)
**Qué validar:**
- Flujo entendible sin instrucciones externas.
- Mensajes de error claros.
- Confirmaciones en acciones destructivas.

**Umbral mínimo (simple):**
- 5 tareas clave (ej. registrar, iniciar sesión, crear entidad, editar, buscar) completables en **≤ 2 minutos cada una** por un compañero externo al equipo.

---

### 8.4 Accesibilidad (A11y)
**Qué medir:**
- Contraste, labels, navegación teclado, headings, ARIA cuando aplique.

**Umbral mínimo:**
- Lighthouse **Accessibility ≥ 80** en 2–3 pantallas principales.

---

### 8.5 Compatibilidad / Responsividad
**Umbral mínimo:**
- UI usable sin romperse en:
  - 360x640 (móvil)
  - 1366x768 (desktop)
- Probado en al menos 1 navegador adicional (Chrome + Firefox/Edge).

---

## 9) Plantillas listas para usar

### 9.1 Plantilla de caso de prueba (individual)
**ID:** F-001  
**Historia:** HU-01  
**Criterio de aceptación:** CA-01  
**Tipo:** Funcional  
**Nivel:** E2E  
**Prioridad:** P0  
**Precondición:** usuario registrado  
**Pasos:**
1. Ir a /login  
2. Ingresar credenciales válidas  
3. Presionar “Ingresar”  
**Resultado esperado:** redirige a /dashboard y muestra nombre del usuario  
**Umbral mínimo:** 100% coincidencia con esperado; sin errores en consola  
**Evidencia:** captura/video + link commit/pipeline  

---

### 9.2 Ejemplos funcionales típicos (React + Node)

#### Caso F-LOGIN (P0)
- **Validación:** credenciales correctas -> token/sesión válida.
- **Negativo:** contraseña incorrecta -> mensaje controlado y código correcto (ej. 401).

#### Caso F-CRUD (P0/P1)
- Crear entidad principal (P0)
- Listar y paginar/buscar (P1)
- Editar (P0/P1)
- Eliminar (P1) con confirmación

#### Caso F-ROL (P0 si aplica)
- Usuario sin rol no ve botón ni accede por URL directa (403/401).

---

## 10) Cómo dejar evidencia (lo que vale en la entrega)

### Evidencia manual
- Captura con: ID del caso + fecha (puede ser en nombre del archivo).
- Video corto para flujos E2E (opcional, recomendado).

### Evidencia automática
- Reporte de tests (JUnit/HTML/console output).
- Captura de pipeline CI con ejecución PASS/FAIL.
- Archivos generados por k6 (summary) y Lighthouse (report).

---

## 11) Recomendación de estructura de carpeta (en el repo)

```
/docs
  /testing
    plan-de-pruebas.md
    matriz-casos.xlsx (o link a Google Sheets)
    reporte-ejecucion.md
    evidencias/
      F-001.png
      NF-k6-summary.txt
      lighthouse-home.html
/tests
  /frontend
  /backend
```

---

## 12) Criterios de salida (para decir “terminamos pruebas”)

Se considera **apto para defensa** cuando:

1. **P0 funcional:** 100% PASS.
2. **Defectos:** 0 críticos/altos abiertos.
3. **NF mínimas:** performance + seguridad básica + accessibility con evidencia.
4. Toda prueba ejecutada tiene **evidencia trazable** (archivo o link).

---

## 13) Checklist final (antes de entregar)

- [ ] Matriz completa (con IDs y evidencia).
- [ ] Pruebas P0 PASS.
- [ ] Reporte de performance (k6/artillery) adjunto.
- [ ] Reporte Lighthouse (performance + a11y) adjunto.
- [ ] Evidencia de seguridad (npm audit / ZAP / verificación de auth).
- [ ] Link al repo y commits/pull requests relevantes.
- [ ] Documento de cierre: conclusiones + lecciones aprendidas.

---

## Anexo A — Umbrales sugeridos (resumen rápido)

| Categoría | Métrica | Umbral mínimo sugerido |
|---|---|---|
| Funcional P0 | Pass rate | **100%** |
| Funcional P1 | Pass rate | **≥ 90%** |
| Performance API | p95 (endpoints críticos) | **≤ 800 ms** (20 VUs, 2–3 min) |
| Performance API | Error rate | **< 1%** |
| UI | Lighthouse Performance | **≥ 70** |
| Accesibilidad | Lighthouse Accessibility | **≥ 80** |
| Seguridad | Hallazgos críticos/altos | **0** |
| Compatibilidad | Viewports | móvil 360x640 + desktop 1366x768 |

---

## Anexo B — Sugerencia rápida de ejecución por roles
- Integrante A: lidera pruebas backend (Postman/Supertest) + performance (k6).
- Integrante B: lidera pruebas frontend (RTL/Vitest) + Lighthouse.
- Integrante C: lidera E2E (Cypress/Playwright) + evidencia y reporte final.

---

**Fin del documento.**
