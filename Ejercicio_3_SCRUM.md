# Actividad: Trello + Historias de Usuario + Criterios de Aceptación (desde Excel a tareas)

## Objetivo

En esta actividad vas a **organizar el trabajo de tu proyecto** usando un tablero estilo **Kanban** en Trello.  
Vas a escribir **6 a 8 historias de usuario** en un **Excel**, definir **criterios de aceptación**, y luego convertir esos criterios en **tareas** dentro de Trello. Finalmente, invitarás a tu equipo y al docente al tablero.

---

## Duración sugerida

60–90 minutos.

---

## Requisitos

- Cuenta de Trello (o acceso a Trello).
- Excel / Google Sheets / LibreOffice Calc (cualquier planilla sirve).
- Proyecto definido (puede ser el mismo del curso).

---

# Parte 1 — Conceptos (explicación clara + buenas prácticas)

## 1) ¿Qué es Trello?

Trello es una herramienta para organizar trabajo en un tablero con tarjetas. Lo más común es usarlo como **Kanban**:  

- **Listas** = estados del trabajo (ej: “Por hacer”)
- **Tarjetas** = trabajo a realizar (ej: una historia de usuario)
- **Checklists** = subtareas (ej: criterios convertidos a tareas)
- **Etiquetas** = categorías (UI, backend, documentación, etc.)

### Buenas prácticas en Trello

- Mantener un tablero simple (pocas listas, claros estados).
- Tarjetas con título corto + descripción con contexto.
- Checklist con tareas accionables (verbos claros).
- Un responsable por tarjeta cuando sea posible.
- Definir “hecho” (Definition of Done) para evitar dudas.

---

## 2) ¿Qué es Kanban?

Kanban es una forma visual de gestionar trabajo con foco en:

- **Flujo**: ver qué está “en proceso”
- **Límites WIP** (Work In Progress): evitar tener demasiadas cosas a medias
- **Entrega continua**: mover trabajo hasta “Hecho” antes de empezar demasiado nuevo

### Buenas prácticas Kanban (para estudiantes)

- No tener más de **2 tarjetas por persona** en “En progreso”.
- Mover tarjetas todos los días (aunque sea un pequeño avance).
- Si algo está bloqueado, marcarlo como **Bloqueado** (etiqueta o comentario).

---

## 3) ¿Qué es una Historia de Usuario?

Una historia de usuario describe una necesidad desde la perspectiva del usuario.

Formato recomendado:
> **Como** [tipo de usuario]  
> **quiero** [acción/funcionalidad]  
> **para** [beneficio/objetivo]

Ejemplo:
> Como estudiante, quiero iniciar sesión con correo y contraseña, para acceder a mis contenidos.

### Buenas prácticas de historias de usuario

- Una historia debe ser **pequeña y entregable**.
- Debe aportar valor observable al usuario.
- Evitar historias demasiado técnicas (“crear base de datos”), eso suele ser tarea técnica dentro de una historia con valor.

---

## 4) ¿Qué son los Criterios de Aceptación?

Son condiciones **claras y verificables** que deben cumplirse para considerar que la historia está “lista”.

Formato recomendado: **Given / When / Then** (Dado / Cuando / Entonces)

- **Dado** un contexto inicial
- **Cuando** ocurre una acción
- **Entonces** se espera un resultado

Ejemplo:

- Dado que el usuario está registrado
- Cuando ingresa correo y contraseña correctos
- Entonces el sistema inicia sesión y lo redirige al dashboard

### Buenas prácticas de criterios de aceptación

- Deben ser **medibles y testeables** (algo que se puede comprobar).
- Evitar “debería ser bonito/rápido” sin definición concreta.
- Incluir casos básicos y de error (por lo menos 1 caso de error por historia cuando aplique).

---

## 5) ¿Qué es una Tarea?

Una tarea es un paso concreto para cumplir un criterio o construir la historia.

Ejemplo de tareas (verbos de acción):

- “Crear formulario de login”
- “Validar formato de correo”
- “Conectar API de autenticación”
- “Mostrar mensaje de error”

**Regla clave:**  
✅ Historia = valor para usuario  
✅ Tareas = pasos concretos para implementar y verificar

---

# Parte 2 — Ejercicio paso a paso (lo que debes hacer)

## Paso A: Crear tu tablero en Trello

1. Crea un tablero nuevo con nombre:
   - `Proyecto - Equipo X`
2. Configura listas (mínimo estas 5):
   - **Backlog**
   - **To Do (Por hacer)**
   - **Doing (En progreso)**
   - **Review/Testing (Revisión/Pruebas)**
   - **Done (Hecho)**

> Sugerencia: Puedes agregar una lista opcional **Blocked (Bloqueado)** si tu equipo la necesita.

### Reglas del flujo (obligatorio)

- Las historias comienzan en **Backlog**.
- Cuando la historia esté lista para trabajarse, pasa a **To Do**.
- Solo se trabaja lo que está en **Doing**.
- Antes de cerrar una historia, debe pasar por **Review/Testing**.
- Solo pasa a **Done** cuando cumpla criterios de aceptación.

---

## Paso B: Crear un Excel con 6 a 8 historias de usuario

1. Crea un archivo Excel llamado:
   - `Historias_Usuario_EquipoX.xlsx`

2. Crea una hoja llamada:
   - `Historias`

3. Crea estas columnas (recomendadas):

- **ID** (HU-01, HU-02, …)
- **Título** (corto)
- **Historia (Como/Quiero/Para)**
- **Prioridad** (Alta/Media/Baja)
- **Criterios de aceptación** (lista o texto)
- **Notas / Alcance** (opcional)
- **Responsable** (opcional)

### Reglas obligatorias del Excel

- Debes escribir **entre 6 y 8 historias**.
- Cada historia debe tener **mínimo 2 criterios de aceptación**.
- Los criterios deben ser verificables (idealmente Given/When/Then).

> Tip: Si te queda largo en una celda, puedes numerar criterios:
>
> 1) …  
> 2) …

---

## Paso C: Pasar criterios de aceptación a tareas en Trello

Ahora llevarás tu Excel a Trello de esta manera:

### 1) Crear tarjetas (cards) por historia

- En Trello, crea una tarjeta por cada historia en la lista **Backlog**.
- Título recomendado:
  - `HU-01 - [título corto]`

En la **descripción** de la tarjeta incluye:

- La historia (Como/Quiero/Para)
- El alcance/notas si aplica

### 2) Convertir criterios a checklist de tareas

- En cada tarjeta, crea un **Checklist** llamado:
  - `Criterios / Tareas`
- Convierte cada criterio de aceptación en tareas concretas.
  - Si un criterio es grande, divídelo en 2–3 tareas.

Ejemplo:

- Criterio: “Entonces se muestra mensaje de error”
  - Tareas:
    - “Detectar credenciales incorrectas”
    - “Mostrar mensaje de error en UI”
    - “Probar caso incorrecto”

### 3) Etiquetas, responsables y fechas (recomendado)

- Agrega **labels** como:
  - UI, Backend, Docs, Testing
- Asigna un **miembro** responsable por tarjeta
- Si el curso maneja plazos, agrega **due dates** (opcional)

---

## Paso D: Invitar al equipo y al docente

### 1) Invitar compañeros

- Invita a tus compañeros del equipo al tablero (como miembros).

### 2) Invitar al docente (obligatorio)

Invítame a mí al tablero con este correo:

- **<cristian.calderons@sansano.usm.cl>**

> Importante: debe quedar como miembro del tablero (no solo link compartido).

---

# Parte 3 — Reglas de trabajo (buenas prácticas obligatorias)

1. **No mover a “Done”** si no se cumplen los criterios de aceptación.
2. Cada tarjeta debe tener:
   - Historia (Como/Quiero/Para)
   - Checklist con tareas derivadas de criterios
3. Mantener **Doing** con pocas tarjetas (máximo 2 por persona).
4. Comentar en la tarjeta decisiones importantes (ej: “se decidió usar X por Y”).
5. Si algo bloquea la historia, escribir:
   - “Bloqueado por: …” (y qué falta para desbloquear)

---

# Parte 4 — Entregables (lo que debes entregar)

1. **Archivo Excel** con 6–8 historias y criterios.
2. **Link al tablero Trello** (asegúrate de acceso).
3. Evidencia mínima (captura o descripción) de:
   - Listas del tablero
   - Tarjetas creadas (HU-01 a HU-0X)
   - Checklists con tareas
   - Invitación realizada al docente (miembro visible)

---

# Parte 5 — Criterios de evaluación (rúbrica breve)

| Criterio | Excelente | Satisfactorio | Insuficiente |
|---|---|---|---|
| Historias (6–8) con formato correcto | Todas claras y con valor | Algunas ambiguas | No cumple formato |
| Criterios de aceptación (mín. 2 por HU) | Verificables y completos | Verificables pero incompletos | No verificables |
| Trello bien estructurado (listas + flujo) | Flujo claro y consistente | Listas ok, flujo irregular | Desordenado |
| Conversión a tareas (checklists) | Tareas accionables y claras | Tareas genéricas | Sin checklist o incompleto |
| Colaboración (invitaciones) | Equipo + docente invitados | Falta alguien | No invitó |

---

# Plantillas útiles (para copiar y pegar)

## Plantilla de historia de usuario
>
> Como ______  
> quiero ______  
> para ______

## Plantilla de criterios (Given/When/Then)

- Dado ______  
- Cuando ______  
- Entonces ______

## Ejemplo de título de tarjeta

- `HU-03 - Registro de usuario`

---

## Nota final

Esta actividad busca que tu equipo transforme “ideas” en trabajo concreto y verificable, manteniendo un flujo visual y colaborativo.
