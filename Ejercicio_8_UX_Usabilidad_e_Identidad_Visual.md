# Actividad de Laboratorio: Usabilidad, UX e Identidad Visual de tu Producto

## Datos generales

**Asignatura:** TPY1101 – Taller Aplicado de Programación
**Duración estimada:** 2 horas
**Modalidad:** Individual o en parejas (idealmente en el equipo del proyecto)
**Sistema operativo considerado:** Windows / macOS / Linux
**Stack de referencia del curso:** React (frontend) + Node.js/Express (backend) + Supabase/PostgreSQL
**Herramientas principales:** Navegador web, editor de texto / VS Code, y al menos una herramienta de IA generativa (ChatGPT, Claude, Gemini, Microsoft Copilot, Leonardo.AI, Ideogram o similar).
**Aplicabilidad:** este ejercicio es **agnóstico al stack** y se aplica directamente a tu proyecto del portafolio (MapacheSecure, Deckora, NoLimits, 40dB, Pop Study, Landing Pages IA, Agente X u otros). No requiere instalar nada: todo se hace en el navegador.

---

## 1. Propósito de la actividad

En esta actividad evaluarás tu producto desde la mirada de la **Experiencia de Usuario (UX)** y la **Usabilidad**, y construirás los elementos básicos de una **identidad visual coherente** que puedas aplicar inmediatamente a tu proyecto: logo, paleta de colores, tipografía y un mini **design system** documentado.

El objetivo es que dejes de improvisar el aspecto visual ("uso azul porque me gusta") y empieces a justificarlo con criterios técnicos y de marca, igual que harías con una decisión de arquitectura.

Al finalizar, deberías ser capaz de:

- distinguir **usabilidad** de **experiencia de usuario** y explicar para qué sirve cada una;
- aplicar las **10 heurísticas de Nielsen** y al menos 3 **leyes de UX** a tu propio producto;
- realizar una **evaluación heurística** de tu interfaz actual y priorizar los hallazgos;
- justificar una **paleta de colores accesible** (WCAG AA) usando Coolors o Adobe Color;
- elegir y combinar **tipografías** desde Google Fonts con criterios de legibilidad y personalidad;
- generar un **logo y elementos gráficos** auxiliares con herramientas de IA, conociendo sus límites legales y de uso;
- consolidar todo en un **UI-KIT** mínimo y reproducible (documento + tokens) que tu equipo pueda usar a partir de hoy.

---

## 2. Contexto del caso

Cualquier proyecto del curso, por bien programado que esté, **se evalúa primero por su interfaz**: si el usuario no entiende qué hacer, si el contraste es ilegible, si el logo se ve hecho en cinco minutos o si las pantallas no parecen de la misma aplicación, todo el trabajo técnico queda devaluado en la presentación.

En la industria, **UX y usabilidad no son "decoración"**: son requisitos funcionales que se miden, se prueban y se documentan. Las grandes plataformas (Google, Apple, Microsoft, Atlassian, IBM) publican sus *design systems* públicamente porque saben que la coherencia visual reduce errores, baja el costo de soporte y aumenta la confianza del usuario.

En este laboratorio aplicarás esa misma lógica a tu producto. Vas a:

1. **Auditar** tu UI actual con criterios profesionales (heurísticas de Nielsen, leyes UX y accesibilidad WCAG).
2. **Definir** la personalidad de marca de tu producto (moodboard, atributos, tono).
3. **Construir** una paleta de colores accesible y una pareja tipográfica coherente.
4. **Generar** un logo y assets visuales con IA, validando que sean usables.
5. **Documentar** todo en un mini design system (`UI-KIT.md` + variables CSS) listo para entregar.

> **Idea clave:** un buen producto se ve bien **porque es coherente**, no porque sea "bonito". La coherencia es una decisión técnica que se documenta.

---

## 3. Producto esperado

Al finalizar la actividad, cada estudiante o equipo debe contar con:

1. un documento **`UX_AUDIT.md`** con la evaluación heurística de tu producto (mínimo 10 hallazgos priorizados);
2. un documento **`BRAND.md`** con la personalidad de marca, moodboard y atributos clave;
3. una **paleta de colores final** (mínimo 5 colores con códigos HEX) validada en contraste WCAG AA;
4. una **pareja tipográfica** seleccionada desde Google Fonts (encabezados + cuerpo);
5. un **logo principal** (versión a color y monocromática) generado y refinado, en formato PNG y/o SVG;
6. un archivo **`UI-KIT.md`** con el resumen del sistema visual (colores, tipografías, espaciados, componentes base);
7. un archivo **`tokens.css`** con las variables CSS listas para integrar al proyecto;
8. evidencias mínimas: capturas de Coolors/Adobe Color, capturas del verificador de contraste, capturas del prompt de IA usado y del resultado;
9. una breve reflexión final con las preguntas de cierre respondidas.

---

## 4. Requisitos previos

No necesitas instalar software pesado. Todo se hace en el navegador. Solo verifica que tienes:

- un **navegador moderno** (Chrome, Firefox, Edge o Safari actualizado);
- una **cuenta gratuita** en al menos una de estas herramientas (recomendado: las dos primeras):
  - [coolors.co](https://coolors.co) — generador de paletas;
  - [color.adobe.com](https://color.adobe.com) — herramienta avanzada de color con verificación de accesibilidad;
  - [fonts.google.com](https://fonts.google.com) — tipografías libres;
  - una herramienta de **IA generativa de imágenes** (Microsoft Copilot Designer, Leonardo.AI, Ideogram, Adobe Firefly, ChatGPT con DALL·E, Gemini, etc.) — algunas requieren cuenta;
- el **enlace al repositorio o pantallas actuales** de tu producto del portafolio (capturas o el sitio en local). Si no tienes UI todavía, usa los wireframes o el prototipo de la EP1.

### Verificación inicial

Abre cada herramienta en una pestaña distinta y comprueba que cargan correctamente. Ten a la vista una **captura de tu producto actual** (al menos pantalla principal y una secundaria). Si tu proyecto aún no tiene UI implementada, baja una captura del prototipo en Figma, una maqueta a mano escaneada o usa una de las propuestas de la EP1.

**Checkpoint 0 aprobado** cuando tienes abiertas las herramientas, una captura de tu producto y has elegido la herramienta de IA con la que vas a generar el logo.

---

## 5. Organización del tiempo

Distribuye el trabajo de la siguiente forma:

- **Bloque 1 – Conceptos clave (UX vs Usabilidad, Nielsen, leyes UX, accesibilidad):** 15 minutos
- **Bloque 2 – Evaluación heurística de tu producto:** 25 minutos
- **Bloque 3 – Personalidad de marca y moodboard:** 15 minutos
- **Bloque 4 – Paleta de colores accesible:** 20 minutos
- **Bloque 5 – Tipografía: pareja desde Google Fonts:** 15 minutos
- **Bloque 6 – Logo y elementos gráficos con IA:** 20 minutos
- **Bloque 7 – Mini design system y cierre:** 10 minutos

**Tiempo total estimado:** 120 minutos

---

# 6. Desarrollo paso a paso

---

## Bloque 1 – Conceptos clave (15 minutos)

### 1.1. Usabilidad vs Experiencia de Usuario (UX)

Estos dos términos se usan como sinónimos, pero **no lo son**. Confundirlos hace que muchos proyectos terminen "bonitos pero inutilizables", o "funcionales pero frustrantes".

| | **Usabilidad** | **Experiencia de Usuario (UX)** |
|---|---|---|
| Pregunta que responde | ¿Es fácil y eficiente de usar? | ¿Cómo se siente usarlo de principio a fin? |
| Foco | La interfaz y la tarea | Toda la relación con el producto |
| Se mide con | Tiempo, errores, tasa de éxito | Satisfacción, NPS, emociones, retención |
| Disciplina madre | Ergonomía cognitiva | Diseño, psicología, marketing, negocio |
| Norma asociada | ISO 9241-11 | ISO 9241-210 |

> **Clave:** la usabilidad es un **requisito mínimo** (que no se rompa nada al usarlo). La UX es el **diferenciador** (que el usuario quiera volver). Tu proyecto necesita ambas.

### 1.2. Las 10 heurísticas de Nielsen

Son la "checklist universal" de usabilidad publicada por Jakob Nielsen en 1994 y aún vigente. Memorízalas: te servirán para evaluar **cualquier interfaz** durante toda tu carrera.

| # | Heurística | Pregunta de auditoría |
|---|---|---|
| 1 | **Visibilidad del estado del sistema** | ¿El usuario sabe qué está pasando? (loaders, mensajes de éxito/error, breadcrumbs) |
| 2 | **Coincidencia con el mundo real** | ¿Usa el lenguaje y los conceptos del usuario, no de los programadores? |
| 3 | **Control y libertad del usuario** | ¿Puede deshacer, cancelar y salir fácilmente? (sin "callejones sin salida") |
| 4 | **Consistencia y estándares** | ¿Lo mismo se llama y se ve igual en toda la app? ¿Sigue convenciones de la plataforma? |
| 5 | **Prevención de errores** | ¿Evita que el usuario meta la pata? (validaciones, confirmaciones, restricciones) |
| 6 | **Reconocer mejor que recordar** | ¿El usuario ve las opciones o tiene que memorizarlas? |
| 7 | **Flexibilidad y eficiencia de uso** | ¿Hay atajos para usuarios expertos sin estorbar a novatos? |
| 8 | **Diseño estético y minimalista** | ¿Cada elemento aporta? ¿No hay ruido visual ni texto innecesario? |
| 9 | **Ayudar a reconocer y recuperarse de errores** | ¿Los mensajes de error son claros, en lenguaje humano, y dicen cómo arreglar? |
| 10 | **Ayuda y documentación** | ¿Existe ayuda contextual cuando se necesita? |

### 1.3. Leyes de UX (las 4 imprescindibles)

| Ley | Qué dice | Aplicación práctica |
|---|---|---|
| **Hick's Law** | El tiempo de decisión crece con el número de opciones | Reduce menús; agrupa; usa progressive disclosure |
| **Fitts's Law** | El tiempo para alcanzar un objetivo depende de su tamaño y distancia | Botones de acción primaria grandes; CTAs cerca del foco |
| **Jakob's Law** | Los usuarios esperan que tu sitio funcione como los que ya conocen | No reinventes patrones (carrito, login, búsqueda) sin razón |
| **Ley de Miller** | La memoria de trabajo maneja ~7±2 items | No pidas formularios de 20 campos; agrupa en pasos |

### 1.4. Accesibilidad: WCAG en 30 segundos

Las **WCAG 2.1** (Web Content Accessibility Guidelines) definen tres niveles: **A** (mínimo), **AA** (estándar legal en muchos países, incluido Chile) y **AAA** (excelencia). Para tu proyecto debes apuntar **mínimo a AA**.

Los tres criterios que vas a chequear hoy:

- **Contraste de color:** texto normal ≥ 4.5:1, texto grande ≥ 3:1.
- **No depender solo del color:** si quitas el color, ¿se sigue entendiendo? (no usar solo "rojo = error" sin un ícono o texto).
- **Tamaño de tipografía:** cuerpo mínimo 16px en web; nada de 12px gris claro sobre blanco.

> Accesibilidad no es "para discapacitados" únicamente: es para todos los usuarios bajo el sol fuerte, con un celular barato, con lentes, con prisa o con conexión mala.

**Checkpoint 1 aprobado** cuando puedes nombrar al menos 5 heurísticas de Nielsen, 2 leyes de UX y los 3 criterios mínimos de accesibilidad sin mirar la guía.

---

## Bloque 2 – Evaluación heurística de tu producto (25 minutos)

### Paso 1. Preparar el material a evaluar

Ten a la vista:

- la **pantalla principal** (dashboard, home o landing) de tu producto;
- al menos **una pantalla secundaria** (formulario, listado, detalle);
- el **flujo principal** del usuario (ej: registrarse → crear algo → verlo listado).

Si todavía no tienes UI implementada, usa el prototipo o los wireframes que entregaste en la EP1. La evaluación se puede hacer sobre cualquier representación visual.

### Paso 2. Plantilla de hallazgos

Crea el archivo `UX_AUDIT.md` con esta estructura:

```markdown
# Evaluación heurística — [Nombre del proyecto]

**Evaluador(es):** [tu nombre]
**Fecha:** [yyyy-mm-dd]
**Versión evaluada:** [commit / fecha del prototipo]
**Pantallas evaluadas:** [lista]

## Resumen ejecutivo

- Hallazgos totales: __
- Críticos: __ / Altos: __ / Medios: __ / Bajos: __
- Top 3 problemas a resolver:
  1. ...
  2. ...
  3. ...

## Tabla de hallazgos

| ID | Pantalla | Heurística violada | Descripción del problema | Severidad (1-4) | Captura | Recomendación |
|---|---|---|---|---|---|---|
| H01 | Login | #9 Errores claros | El error "Failed" no dice qué hacer | 3 | login.png | Mostrar "Email o contraseña incorrectos. ¿Olvidaste tu contraseña?" |
| H02 | ... | ... | ... | ... | ... | ... |
```

### Paso 3. Escala de severidad de Nielsen

Usa esta escala estándar para priorizar:

| Nivel | Significado | Cuándo usarlo |
|---|---|---|
| **0** | No es un problema | (no listar) |
| **1** | Cosmético | Se arregla si sobra tiempo |
| **2** | Menor | Frustra pero no bloquea |
| **3** | Mayor | Hace difícil completar la tarea |
| **4** | Catastrófico | Bloquea por completo o pone en riesgo al usuario |

### Paso 4. Hacer la evaluación (15 minutos cronometrados)

Recorre las pantallas y, para cada problema que detectes, registra una fila en la tabla. **Reglas de juego:**

1. **Una heurística por hallazgo.** Si un mismo problema viola dos heurísticas, escoge la más relevante.
2. **Describe el problema, no la solución.** "El botón es rojo" no es un hallazgo; "El botón primario usa rojo, lo que sugiere acción destructiva (heurística #4 consistencia)" sí lo es.
3. **Apunta a mínimo 10 hallazgos.** Si tu app es muy minimalista y no llegas, busca también en los **estados vacíos** (¿qué se ve cuando no hay datos?), **estados de error**, **pantallas de carga** y **mensajes del sistema**.
4. **Saca capturas** y pégalas en el documento.

### Paso 5. Priorización

Ordena la tabla por severidad descendente. Identifica los **top 3 problemas** y escríbelos en el resumen ejecutivo. Esos son los que tu equipo debería arreglar antes de la presentación final.

### Ejemplo concreto (para guiarte)

| ID | Pantalla | Heurística | Problema | Sev. | Recomendación |
|---|---|---|---|---|---|
| H01 | Registro | #5 Prevención de errores | El campo "email" no valida formato hasta enviar | 3 | Validar onBlur con regex y mostrar ✓ o ✗ inline |
| H02 | Dashboard | #1 Visibilidad del estado | Tras hacer click en "Guardar" no pasa nada visible durante 2 s | 3 | Spinner + texto "Guardando..." en el botón |
| H03 | Listado | #6 Reconocer mejor que recordar | Los iconos de acciones no tienen tooltip | 2 | Agregar `title` o tooltip nativo |
| H04 | Cualquiera | #4 Consistencia | El botón "Aceptar" aparece a veces a la izquierda y a veces a la derecha del modal | 2 | Definir convención (primario derecha) y aplicar en todos |

**Checkpoint 2 aprobado** cuando entregas `UX_AUDIT.md` con mínimo 10 hallazgos, con su severidad y top 3 priorizados.

---

## Bloque 3 – Personalidad de marca y moodboard (15 minutos)

Antes de elegir colores y tipografías, necesitas saber **qué quieres comunicar**. Si no, terminas eligiendo lo que te gusta visualmente, sin justificación profesional.

### Paso 6. Definir los atributos de marca

Crea el archivo `BRAND.md` y completa esta plantilla:

```markdown
# Identidad de marca — [Nombre del proyecto]

## Audiencia objetivo
[Describe en una frase: edad, contexto, dispositivo, conocimiento técnico]
Ej: "Padres y madres entre 30-50 años, que usan smartphone Android, no son técnicos, preocupados por el tiempo de pantalla de sus hijos."

## Promesa de valor
[Una frase: qué le entrega tu producto al usuario]
Ej: "Te ayudamos a poner límites de uso de pantalla sin convertirte en el villano de la película."

## 5 atributos de marca (elige 5 — no más, no menos)
1. ...
2. ...
3. ...
4. ...
5. ...

## Lo que NO somos (3 ejemplos)
1. No somos ...
2. No somos ...
3. No somos ...

## Tono de voz (un ejemplo)
- Cómo escribimos un mensaje de error: "[ejemplo]"
- Cómo escribimos un mensaje de éxito: "[ejemplo]"
```

### Paso 7. Banco de atributos (elige tus 5)

Para que no te bloquees, aquí tienes un menú agrupado. Elige **5 atributos coherentes entre sí** (no mezcles "serio" con "lúdico"):

| Familia | Atributos |
|---|---|
| **Confianza** | Profesional, riguroso, seguro, sobrio, institucional, formal |
| **Cercanía** | Cálido, amable, humano, accesible, conversacional, empático |
| **Energía** | Vibrante, audaz, dinámico, optimista, juvenil, atrevido |
| **Innovación** | Tecnológico, futurista, minimalista, limpio, moderno, eficiente |
| **Lúdico** | Divertido, juguetón, colorido, narrativo, gamificado, irreverente |
| **Naturaleza** | Orgánico, sustentable, sano, calmo, terroso, eco |

> **Anti-patrón clásico:** elegir "moderno, profesional, divertido, serio, juvenil" — son atributos contradictorios. Elige una **dirección**.

### Paso 8. Moodboard rápido con IA

Pídele a una herramienta de IA generativa o de búsqueda visual (Pinterest, Dribbble, Behance, o un generador con prompts):

> "Genera un moodboard visual para una app de [tu vertical] con personalidad [tus 5 atributos]. Estilo de referencia: [una marca que admires del rubro]. Incluye 6 referencias: 2 paletas de color, 2 ejemplos tipográficos, 2 ejemplos de UI."

Pega 4-6 imágenes de referencia en `BRAND.md` bajo el título `## Moodboard`. **Importante:** son referencias de inspiración, no para copiar.

**Checkpoint 3 aprobado** cuando tienes `BRAND.md` con audiencia, promesa, 5 atributos coherentes, lo que NO eres, ejemplo de tono y un moodboard de mínimo 4 imágenes.

---

## Bloque 4 – Paleta de colores accesible (20 minutos)

### Paso 9. Anatomía de una paleta profesional

Una paleta usable tiene **roles**, no solo "colores bonitos":

| Rol | Para qué sirve | Cuántos |
|---|---|---|
| **Primario (Brand)** | Identidad y CTAs principales | 1 + 2 variantes (light/dark) |
| **Secundario (Accent)** | Apoya al primario, no compite | 1 |
| **Neutros** | Fondos, textos, bordes | 4-6 grises (de blanco a casi-negro) |
| **Semánticos** | Estados de feedback | Éxito (verde), Error (rojo), Advertencia (amarillo), Info (azul) |

> **Regla 60-30-10:** 60% del lienzo es neutro, 30% es color secundario o variante del primario, 10% es el primario en su forma más saturada (los CTAs). Esto evita las pantallas saturadas estilo "circo".

### Paso 10. Generar la paleta en Coolors

1. Abre [coolors.co](https://coolors.co/generate).
2. Bloquea uno o dos colores que ya quieras (clic en el candado).
3. Presiona **barra espaciadora** para iterar hasta encontrar 5 colores coherentes con tu personalidad de marca.
4. Una vez tengas 5 que te gusten, abre **View Palette** → exporta como **PNG** y copia los códigos HEX.

> **Tip:** si tu marca debe transmitir confianza/seguridad, parte de un azul medio (#1E40AF) o teal (#0F766E). Si va por energía/juventud, parte de un naranja (#EA580C) o magenta (#DB2777). Si va por naturaleza/calma, parte de un verde oliva (#65A30D) o terracota (#B45309).

### Paso 10b. Alternativa avanzada con Adobe Color

Para mayor control, [color.adobe.com/create/color-wheel](https://color.adobe.com/create/color-wheel) te permite:

- usar **reglas armónicas**: análoga, complementaria, triádica, monocromática;
- ir a la pestaña **"Accessibility Tools"** para detectar combinaciones que **fallan WCAG**;
- guardar la paleta en tu librería.

Recomendado: empieza con **regla análoga o complementaria** para no equivocarte.

### Paso 11. Validar contraste WCAG

**Este paso no es opcional.** Una paleta lindísima que falla contraste no sirve.

1. Abre [webaim.org/resources/contrastchecker](https://webaim.org/resources/contrastchecker/).
2. Para cada combinación que vayas a usar (texto sobre fondo), introduce los HEX y verifica:
   - **Texto normal:** mínimo **4.5:1** (AA)
   - **Texto grande (≥18px regular o ≥14px bold):** mínimo **3:1** (AA)
   - **UI components y gráficos:** mínimo **3:1** (AA)
3. Toma captura del resultado.

### Paso 12. Documentar la paleta

En tu `UI-KIT.md` (lo cerrarás más tarde), abre una sección con esta tabla:

```markdown
## Paleta de colores

| Rol | Token | HEX | Uso |
|---|---|---|---|
| Primario | --color-primary | #1E40AF | Botones primarios, links |
| Primario hover | --color-primary-hover | #1E3A8A | Estado hover |
| Secundario | --color-accent | #F59E0B | Elementos destacados, badges |
| Texto principal | --color-text | #111827 | Cuerpo de texto |
| Texto secundario | --color-text-muted | #6B7280 | Descripciones, timestamps |
| Fondo | --color-bg | #FFFFFF | Fondo general |
| Fondo alternativo | --color-bg-alt | #F9FAFB | Tarjetas, hover de filas |
| Borde | --color-border | #E5E7EB | Separadores |
| Éxito | --color-success | #16A34A | Confirmaciones |
| Error | --color-danger | #DC2626 | Errores, eliminar |
| Advertencia | --color-warning | #CA8A04 | Avisos |
| Info | --color-info | #2563EB | Información neutra |

**Verificación de contraste (AA):**
- Texto principal sobre fondo: 16.1:1 ✅
- Texto sobre primario: 7.4:1 ✅
- Texto secundario sobre fondo: 5.6:1 ✅
```

### Paso 13. Exportar a `tokens.css`

Crea el archivo `tokens.css` con tus variables (ejemplo, sustituye los HEX por los tuyos):

```css
:root {
  /* Brand */
  --color-primary: #1E40AF;
  --color-primary-hover: #1E3A8A;
  --color-accent: #F59E0B;

  /* Neutrales */
  --color-text: #111827;
  --color-text-muted: #6B7280;
  --color-bg: #FFFFFF;
  --color-bg-alt: #F9FAFB;
  --color-border: #E5E7EB;

  /* Semánticos */
  --color-success: #16A34A;
  --color-danger: #DC2626;
  --color-warning: #CA8A04;
  --color-info: #2563EB;
}
```

Este archivo se importa una sola vez en tu app (ej: `index.css` o `App.css` en React) y a partir de ahí **nunca más escribes un HEX a mano**: siempre usas `var(--color-primary)`. Esto es lo que hace que el sistema sea coherente y fácil de mantener.

**Checkpoint 4 aprobado** cuando tienes 5 colores definidos con sus roles, **todas** las combinaciones validadas en WebAIM con AA, y `tokens.css` listo.

---

## Bloque 5 – Tipografía: pareja desde Google Fonts (15 minutos)

### Paso 14. Por qué dos tipografías y no más

La regla profesional es **2 tipografías máximo** (3 solo si hay una de uso muy puntual). Más fuentes = más peso de descarga, más caos visual y más esfuerzo cognitivo.

| Rol | Característica | Ejemplos |
|---|---|---|
| **Encabezados (Display)** | Personalidad, contraste, peso fuerte | Inter Bold, Poppins Bold, Space Grotesk, Montserrat |
| **Cuerpo (Body)** | Legibilidad a tamaños pequeños, neutralidad | Inter, Roboto, Open Sans, Source Sans Pro, Lato |

### Paso 15. Combinaciones probadas (úsalas si vas con prisa)

| Display | Body | Personalidad |
|---|---|---|
| Inter | Inter | Moderno, neutro, "SaaS profesional" |
| Poppins | Inter | Amigable, fintech-startup |
| Space Grotesk | Inter | Tech-forward, dev tools |
| Playfair Display | Source Sans Pro | Editorial, elegante |
| Montserrat | Open Sans | Corporativo accesible |
| DM Serif Display | DM Sans | Editorial moderno |
| Bricolage Grotesque | Inter | Creativo y joven |

> **Combo seguro si dudas:** Inter para todo. Es la tipografía de Linear, Notion, Vercel y muchísimo SaaS contemporáneo.

### Paso 16. Probar la pareja

1. Abre [fonts.google.com](https://fonts.google.com).
2. Busca tu fuente para encabezados y dale clic. Mira los pesos disponibles (300, 400, 600, 700).
3. Haz lo mismo para cuerpo.
4. Usa la sección **"Type Tester"** para escribir un titular y un párrafo de tu producto y ver cómo se siente.
5. **Toma captura** de cómo se ve un titular tipo "[Nombre de tu app] — [tagline]" y un párrafo real.

### Paso 17. Definir la escala tipográfica

Una **escala** es la lista finita de tamaños que tu app usará. Sin escala, cada desarrollador inventa tamaños y pierdes coherencia.

Escala recomendada (ratio 1.25, "Major Third"):

| Token | px | Uso |
|---|---|---|
| `--text-xs` | 12px | Etiquetas, captions |
| `--text-sm` | 14px | Texto secundario |
| `--text-base` | 16px | Cuerpo (default) |
| `--text-lg` | 20px | Subtítulos, leads |
| `--text-xl` | 25px | H3 |
| `--text-2xl` | 31px | H2 |
| `--text-3xl` | 39px | H1 |

Y los pesos:

| Token | Valor | Uso |
|---|---|---|
| `--font-regular` | 400 | Cuerpo |
| `--font-medium` | 500 | Énfasis suave, links |
| `--font-semibold` | 600 | Subtítulos |
| `--font-bold` | 700 | Titulares |

### Paso 18. Agregar al `tokens.css`

Añade al archivo:

```css
:root {
  /* Tipografía */
  --font-heading: 'Poppins', system-ui, sans-serif;
  --font-body: 'Inter', system-ui, sans-serif;

  /* Escala */
  --text-xs: 0.75rem;    /* 12px */
  --text-sm: 0.875rem;   /* 14px */
  --text-base: 1rem;     /* 16px */
  --text-lg: 1.25rem;    /* 20px */
  --text-xl: 1.5625rem;  /* 25px */
  --text-2xl: 1.9375rem; /* 31px */
  --text-3xl: 2.4375rem; /* 39px */

  /* Pesos */
  --font-regular: 400;
  --font-medium: 500;
  --font-semibold: 600;
  --font-bold: 700;

  /* Line height */
  --leading-tight: 1.2;
  --leading-normal: 1.5;
  --leading-relaxed: 1.7;
}
```

Y en tu `index.html` (o equivalente), enlaza la fuente desde Google Fonts:

```html
<link rel="preconnect" href="https://fonts.googleapis.com">
<link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
<link href="https://fonts.googleapis.com/css2?family=Inter:wght@400;500;600;700&family=Poppins:wght@600;700&display=swap" rel="stylesheet">
```

> **Tip de performance:** carga **solo los pesos que vas a usar**. Cargar la familia completa puede sumar 500 KB innecesarios.

**Checkpoint 5 aprobado** cuando tienes pareja tipográfica elegida, capturada en uso real con un titular y un párrafo de tu producto, y agregada a `tokens.css`.

---

## Bloque 6 – Logo y elementos gráficos con IA (20 minutos)

### Paso 19. Antes de generar: definir el brief del logo

Un mal prompt produce un logo genérico. Antes de abrir la IA, escribe el brief:

```markdown
## Brief del logo — [Nombre del proyecto]

- **Nombre:** [Nombre]
- **Categoría:** [ed-tech, fintech, salud, gaming...]
- **Atributos (de BRAND.md):** [tus 5]
- **Estilo deseado:** [minimalista / geométrico / orgánico / typo-only / con mascota]
- **Símbolo opcional:** [un objeto, animal o concepto que quieras evocar]
- **Colores base:** [tu primario y accent]
- **NO quiero:** [estilos a evitar — ej: "no acuarela, no degradados arcoíris, no gradientes de los 90s"]
```

### Paso 20. Prompt efectivo para IA generativa

Usa una herramienta como **Microsoft Copilot Designer** (gratuita), **Leonardo.AI**, **Ideogram** (excelente porque maneja texto), **Adobe Firefly** o **DALL·E** dentro de ChatGPT/Copilot.

#### Plantilla de prompt (en inglés rinde mejor)

```
Modern minimalist logo for "[Nombre]", a [categoría] app for [audiencia].
Style: [minimalist | geometric | flat | line art].
Symbol: [un símbolo evocador, ej: a stylized fox head with a circuit pattern].
Color palette: [tu HEX primario] and [tu HEX accent], plus white.
Typography: clean sans-serif, [bold | medium].
Background: solid white, centered, vector style, high contrast,
suitable for app icon and website header.
Format: square, 1:1 ratio, no text artifacts, no extra symbols,
flat 2D, no 3D, no photorealism.
```

> **Si usas Ideogram o Adobe Firefly**, el manejo de texto es mejor que en DALL·E. Si vas con DALL·E o Copilot, **muchas veces conviene generar el logo SIN texto** y agregar el nombre en Figma/Canva/PowerPoint usando tu tipografía oficial.

### Paso 21. Iterar con criterio

Genera **al menos 4 variantes**. Por cada una, evalúa:

| Criterio | ¿Cumple? |
|---|---|
| ¿Es reconocible a 32×32 px (favicon)? | sí / no |
| ¿Funciona en monocromático (todo negro)? | sí / no |
| ¿No tiene tipografía deformada o caracteres raros? | sí / no |
| ¿Refleja al menos 3 de tus 5 atributos de marca? | sí / no |
| ¿Es lo suficientemente único como para no confundirse con marcas existentes? | sí / no |

Descarta las que fallen. Quédate con **una**.

### Paso 22. Limpieza y refinamiento

Las imágenes de IA suelen tener:

- restos de fondo;
- bordes pixelados;
- "extra fingers" — en logos eso se traduce en líneas o detalles que sobran.

Opciones para limpiar:

- **Remove.bg** o **Adobe Express** (gratis): quitar fondo y dejar transparente;
- **Photopea** (Photoshop en navegador, gratis): retoque manual;
- **Vectorizer.AI** o **vectorizer.io** (gratis con límites): convertir PNG → SVG (escalable);
- una **IA conversacional** (Claude, ChatGPT, Gemini) puede ayudarte a generar el SVG directamente con prompts del tipo *"genera un SVG simple de una cabeza de mapache estilizada en líneas"* — luego lo refinas en Photopea o lo pegas en [boxy-svg.com](https://boxy-svg.com).

### Paso 23. Versiones del logo

Un logo profesional **no es una sola imagen**: es un **sistema**. Genera/exporta al menos:

| Versión | Para qué |
|---|---|
| **Logo principal a color** | Web, presentaciones, header |
| **Logo monocromático negro** | Documentos en blanco y negro, footers |
| **Logo monocromático blanco** | Sobre fondos oscuros |
| **Isotipo (solo el símbolo, sin texto)** | App icon, favicon, avatar |

Guárdalas en una carpeta `assets/logos/` con nombres claros:

```
assets/
└── logos/
    ├── logo-color.svg
    ├── logo-color.png
    ├── logo-mono-black.svg
    ├── logo-mono-white.svg
    ├── isotipo.svg
    └── favicon.png  (32x32)
```

### Paso 24. Reglas de uso (do's & don'ts)

Documenta en `UI-KIT.md`:

```markdown
## Uso del logo

### Espacio de seguridad
Mantener un margen mínimo equivalente al alto de la "X" del nombre alrededor del logo.

### Tamaño mínimo
- Web: 24px de alto
- Impreso: 12mm de alto

### Fondos permitidos
- Logo a color sobre blanco o tonos neutros muy claros (--color-bg, --color-bg-alt)
- Logo blanco sobre --color-primary o cualquier fondo oscuro con contraste ≥ 4.5:1

### NO hacer
- ❌ Deformar la proporción (estirar, comprimir)
- ❌ Cambiar los colores oficiales por otros
- ❌ Aplicar sombras, biseles o efectos
- ❌ Rotar el logo
- ❌ Usar el logo a color sobre fondos de bajo contraste
```

### Paso 25. Aviso legal y de uso de IA

**Importante:** documenta en `BRAND.md` que el logo fue generado con asistencia de IA, qué herramienta usaste y verifica los términos de licencia. La mayoría de plataformas (Adobe Firefly, Microsoft Copilot, Leonardo.AI, Ideogram en planes pagados) permiten uso comercial, pero confírmalo. Cita:

```markdown
## Atribución y licencias

- **Logo:** generado con [Herramienta] el [fecha], refinado manualmente.
  Términos de uso revisados: [link a la página de términos].
- **Tipografías:** Inter y Poppins desde Google Fonts (Open Font License).
- **Paleta de colores:** original, generada en [Coolors / Adobe Color].
```

**Checkpoint 6 aprobado** cuando tienes el logo en sus 4 versiones, dentro de `assets/logos/`, y las reglas de uso documentadas.

---

## Bloque 7 – Mini design system y cierre (10 minutos)

### Paso 26. Componentes base mínimos

Un design system real tiene cientos de componentes. Para esta actividad nos basta con definir **6 componentes base** en `UI-KIT.md`:

```markdown
## Componentes

### Botones
- **Primario:** fondo `--color-primary`, texto blanco, padding 12px 20px, radio 6px, peso 600
- **Secundario:** fondo transparente, borde 1px `--color-primary`, texto `--color-primary`
- **Destructivo:** fondo `--color-danger`, texto blanco
- **Estados:** hover (oscurecer 10%), focus (anillo de 2px `--color-primary` con offset), disabled (50% opacidad, cursor not-allowed)

### Inputs
- altura 40px, borde 1px `--color-border`, radio 6px, padding 0 12px, font-size --text-base
- focus: borde --color-primary + ring 3px primary @ 20%
- error: borde --color-danger + texto de error abajo en --text-sm

### Tarjetas (Cards)
- fondo --color-bg-alt, borde 1px --color-border, radio 8px, padding 20px, sombra suave (`0 1px 3px rgba(0,0,0,0.05)`)

### Mensajes (Alerts)
- éxito / error / advertencia / info: cada uno con su color semántico al 10% de fondo y al 100% en el borde izquierdo (4px)

### Tipografía aplicada
- H1: --text-3xl, --font-bold, --leading-tight, --font-heading
- H2: --text-2xl, --font-semibold, --font-heading
- H3: --text-xl, --font-semibold, --font-heading
- Body: --text-base, --font-regular, --leading-normal, --font-body
- Small: --text-sm, --color-text-muted

### Espaciado (escala 4px)
| Token | px |
|---|---|
| --space-1 | 4 |
| --space-2 | 8 |
| --space-3 | 12 |
| --space-4 | 16 |
| --space-6 | 24 |
| --space-8 | 32 |
| --space-12 | 48 |
| --space-16 | 64 |
```

Agrega estos espaciados a `tokens.css`:

```css
:root {
  /* Espaciado */
  --space-1: 0.25rem;
  --space-2: 0.5rem;
  --space-3: 0.75rem;
  --space-4: 1rem;
  --space-6: 1.5rem;
  --space-8: 2rem;
  --space-12: 3rem;
  --space-16: 4rem;

  /* Radios */
  --radius-sm: 4px;
  --radius-md: 6px;
  --radius-lg: 8px;
  --radius-full: 9999px;

  /* Sombras */
  --shadow-sm: 0 1px 2px rgba(0,0,0,0.05);
  --shadow-md: 0 4px 6px rgba(0,0,0,0.07);
  --shadow-lg: 0 10px 25px rgba(0,0,0,0.1);
}
```

### Paso 27. Aplicar a una pantalla real

Como mini-prueba, **rehaz una pantalla** de tu producto (puede ser solo en wireframe, mockup en PowerPoint o ya en código si te da el tiempo) usando exclusivamente:

- los colores definidos en tu paleta;
- las tipografías y escala definidas;
- los espaciados de la escala 4px;
- el logo nuevo en el header.

Toma una captura del **antes** y **después**. Esa pareja de imágenes es uno de los entregables más valiosos para tu presentación final del proyecto.

### Paso 28. Preguntas de reflexión (responder en `REFLEXION_UX.md`)

1. ¿Cuáles fueron los **3 hallazgos más graves** de tu evaluación heurística y qué heurística violaba cada uno?
2. ¿Por qué un **diseño accesible** (WCAG AA) **mejora la experiencia incluso de usuarios sin discapacidad**? Da dos ejemplos concretos.
3. ¿Por qué decidiste **estos 5 atributos de marca y no otros**? ¿Qué decisiones de color/tipografía se desprenden directamente de ellos?
4. Tu paleta tiene un color primario y un acento. ¿Por qué **no más**? ¿Qué problema causa una paleta con 8 colores primarios?
5. La **regla 60-30-10** te obliga a no saturar la pantalla. Identifica una zona de tu UI actual donde esta regla se rompe. ¿Cómo la corregirías?
6. Hoy generaste un logo con IA. Enumera **dos riesgos** de hacerlo así y cómo los mitigaste (legales, técnicos o de marca).
7. ¿Qué relación ves entre **tener un design system** y **escribir código mantenible**? Explica con un ejemplo de tu propio repositorio.
8. Si tuvieras que **defender** ante un cliente por qué eligieron Inter en vez de Comic Sans, ¿cuál sería tu argumento técnico (no "porque es más bonita")?

### Paso 29. Cierre

Escribe un párrafo breve en `REFLEXION_UX.md`:

- ¿Qué decisión visual de tu proyecto **tendrías que cambiar mañana** después de este laboratorio?
- ¿Qué cosa hacías "porque sí" antes y ahora puedes justificar técnicamente?

**Checkpoint 7 aprobado** cuando entregas `UI-KIT.md`, `tokens.css`, la pareja antes/después de la pantalla y `REFLEXION_UX.md`.

---

## 7. Entregables

Cada estudiante o equipo debe entregar (en una carpeta `entrega-ux/` dentro del repo del proyecto):

### Documentos

- `UX_AUDIT.md` con mínimo **10 hallazgos**, severidad y top 3 priorizado.
- `BRAND.md` con audiencia, promesa, 5 atributos coherentes, lo que NO eres, tono y moodboard (≥4 imágenes).
- `UI-KIT.md` con paleta, tipografía, escala, componentes base y reglas de uso del logo.
- `REFLEXION_UX.md` con las 8 preguntas respondidas y el cierre.

### Archivos técnicos

- `tokens.css` con variables de color, tipografía, escala, espaciado, radios y sombras.
- Carpeta `assets/logos/` con las **4 versiones** del logo (color, mono-negro, mono-blanco, isotipo) y `favicon.png`.

### Evidencias mínimas (capturas)

- captura de la **paleta en Coolors o Adobe Color** con los 5 HEX visibles;
- captura del **verificador de contraste WAVE/WebAIM** con resultado AA en al menos 3 combinaciones críticas (texto sobre fondo, texto sobre primario, texto secundario);
- captura del **type tester** de Google Fonts con un titular y un párrafo de tu producto en la pareja elegida;
- captura del **prompt** usado para generar el logo y el resultado;
- pareja de capturas **antes / después** de una pantalla rediseñada con el sistema.

---

## 8. Criterios de logro

Se espera que el estudiante:

- distinga claramente **usabilidad** de **UX** y aplique heurísticas de Nielsen a su producto real;
- justifique sus decisiones visuales con **atributos de marca**, no con gusto personal;
- entregue una paleta que **cumple WCAG AA** y lo demuestre con capturas;
- documente la pareja tipográfica con **roles, escala y pesos**;
- genere y refine un logo en **al menos 4 versiones**, con reglas de uso documentadas;
- consolide todo en un `UI-KIT.md` + `tokens.css` **listos para integrar** al código del proyecto.

---

## 9. Desafío opcional para quienes terminen antes

Si terminas antes del tiempo, elige al menos una mejora.

### Opción A: Modo oscuro

Duplica las variables de color en `tokens.css` y crea un set para `--theme-dark`. Implementa el switch con la pseudoclase `prefers-color-scheme: dark` o un toggle manual. **Verifica de nuevo el contraste:** los colores que funcionaban en claro pueden fallar en oscuro.

### Opción B: Test de usabilidad rápido (3 usuarios)

Pide a 3 personas que NO conocen tu app que intenten completar 1 tarea (ej: "regístrate y crea tu primer X"). Cronometra, anota errores y dudas. Un test de 3 personas detecta ~75% de los problemas mayores. Documenta hallazgos en `USABILITY_TEST.md`.

### Opción C: Auditoría con Lighthouse

Si tu app ya está desplegada o corre en `localhost`, abre Chrome DevTools → pestaña **Lighthouse** → audita Performance, Accessibility, Best Practices, SEO. Apunta a **≥80 en accesibilidad**. Documenta los hallazgos críticos y cómo los corregirías.

### Opción D: Componentes en Storybook

Si manejas React, instala [Storybook](https://storybook.js.org) y crea historias para tus componentes base (Button, Input, Card). Es la forma profesional de **documentar y probar** un design system.

### Opción E: Logo animado

Anima tu logo con CSS o Lottie. Útil para splash screen / loaders. No exageres: 1-2 segundos máximo.

---

## 10. Reglas mínimas de la actividad

### Reglas de la paleta

- Mínimo **5 colores con rol asignado** (no 5 colores random).
- **Todas** las combinaciones texto-fondo deben pasar **WCAG AA** (4.5:1 normal / 3:1 grande).
- **No** usar solo color para indicar estado (ej: el error siempre lleva texto e ícono además del rojo).
- HEX documentados como **tokens CSS**, nunca hardcodeados en componentes.

### Reglas de tipografía

- Máximo **2 familias tipográficas** (3 solo justificándolo).
- Cuerpo **mínimo 16px** en web.
- Escala definida y respetada (no inventar tamaños sueltos).
- Pesos cargados solo los necesarios.

### Reglas del logo

- Generado con IA es válido, pero **debe estar refinado y limpio** (sin fondos sucios, sin texto deformado).
- Mínimo **4 versiones** (color, mono-negro, mono-blanco, isotipo).
- **Atribución y licencia** documentadas.
- Reglas de uso (espacio de seguridad, tamaño mínimo, fondos, qué NO hacer) escritas en `UI-KIT.md`.

### Reglas del sistema

- `tokens.css` es la **única fuente de verdad** del estilo. Cualquier valor (color, tamaño, espacio) que no esté tokenizado es deuda técnica.
- Cada decisión visual debe poder **justificarse** apuntando a un atributo de marca o una heurística.

---

## 11. Cierre de la actividad

Al terminar, redacta una conclusión breve respondiendo:

1. ¿Qué parte de la actividad resultó más simple?
2. ¿Qué parte fue más difícil y por qué?
3. ¿Qué cambio concreto vas a aplicar en tu proyecto del portafolio en las próximas 48 horas como resultado de este laboratorio?

> **Recordatorio final:** un buen producto se ve bien **porque es coherente**, no porque sea bonito. Coherencia = decisiones documentadas y aplicadas con disciplina. Ese es exactamente el mismo principio que aplicaste en arquitectura, en seguridad y en pruebas. UX no es la excepción: es ingeniería visual.

---

## Anexo A — Recursos rápidos

### Color y contraste
- [coolors.co](https://coolors.co) — generador de paletas
- [color.adobe.com](https://color.adobe.com) — Adobe Color (incluye accessibility tools)
- [webaim.org/resources/contrastchecker](https://webaim.org/resources/contrastchecker/) — verificador WCAG
- [realtimecolors.com](https://realtimecolors.com) — previsualiza paleta + tipografía en una landing real

### Tipografía
- [fonts.google.com](https://fonts.google.com) — Google Fonts
- [fontpair.co](https://fontpair.co) — combinaciones probadas
- [type-scale.com](https://type-scale.com) — generador de escala tipográfica

### IA generativa de logos / imágenes
- [copilot.microsoft.com](https://copilot.microsoft.com) (Designer, gratis con cuenta Microsoft)
- [ideogram.ai](https://ideogram.ai) (excelente con texto)
- [leonardo.ai](https://leonardo.ai)
- [firefly.adobe.com](https://firefly.adobe.com) (Adobe Firefly)
- [chatgpt.com](https://chatgpt.com) (con DALL·E, en plan pagado o gratuito limitado)

### Limpieza y vectorización
- [remove.bg](https://www.remove.bg) — quitar fondo
- [photopea.com](https://www.photopea.com) — Photoshop en el navegador
- [vectorizer.ai](https://vectorizer.ai) o [vectorizer.io](https://vectorizer.io) — PNG → SVG

### Inspiración
- [dribbble.com](https://dribbble.com)
- [behance.net](https://www.behance.net)
- [mobbin.com](https://mobbin.com) — patrones reales de apps móviles
- [land-book.com](https://land-book.com) — landings reales

### Heurísticas y leyes
- [nngroup.com/articles/ten-usability-heuristics](https://www.nngroup.com/articles/ten-usability-heuristics/) — Nielsen, fuente oficial
- [lawsofux.com](https://lawsofux.com) — leyes de UX explicadas

### Design systems de referencia (lectura recomendada)
- [m3.material.io](https://m3.material.io) — Material Design (Google)
- [developer.apple.com/design/human-interface-guidelines](https://developer.apple.com/design/human-interface-guidelines/) — Apple HIG
- [atlassian.design](https://atlassian.design) — Atlassian Design System
- [carbondesignsystem.com](https://carbondesignsystem.com) — IBM Carbon
- [primer.style](https://primer.style) — GitHub Primer

---

## Anexo B — Plantilla de prompt mejorada para logos

Si tu primer intento con IA dio resultados pobres, prueba este prompt más estructurado (en inglés):

```
Create a professional, modern logo design for a [tipo de producto] called "[Nombre]".

Brand attributes: [tus 5 atributos en inglés].
Target audience: [audiencia en inglés].

Design specifications:
- Style: minimalist, geometric, flat 2D vector
- Symbol: [una metáfora visual concreta — ej: "a stylized owl made of geometric shapes representing wisdom and night-time learning"]
- Color palette: primary [#HEX_PRIMARY], accent [#HEX_ACCENT], on white background
- Typography (if included): clean modern sans-serif, bold weight
- Composition: centered, symmetrical, ample white space, square 1:1

Technical requirements:
- High contrast and recognizable at 32x32px
- Works in pure black & white version
- Vector style, no photorealism, no 3D, no gradients
- No textual artifacts or extra symbols
- Suitable for app icon, favicon, business card, and website header

Avoid: cliché icons (lightbulbs, gears, generic globes), AI artifacts,
deformed letters, complex illustrations, watercolor or painted style.
```

Si la IA insiste en poner texto deformado, **agrega "no text" o "icon only"** y luego compón el wordmark en Figma/Photopea con tu tipografía oficial.

---

## Anexo C — Plantilla rápida `UI-KIT.md` (úsala como punto de partida)

```markdown
# UI Kit — [Nombre del proyecto]

> Versión 1.0 · [fecha] · [responsable]

## 1. Identidad
- **Nombre:** ...
- **Tagline:** ...
- **Atributos de marca:** ...

## 2. Logo
[insertar imagen del logo principal]

### Versiones
- Color: `assets/logos/logo-color.svg`
- Mono negro: `assets/logos/logo-mono-black.svg`
- Mono blanco: `assets/logos/logo-mono-white.svg`
- Isotipo: `assets/logos/isotipo.svg`

### Reglas de uso
[copiar las del Paso 24]

## 3. Paleta de colores
[tabla del Paso 12]

[capturas del verificador de contraste]

## 4. Tipografía
- **Encabezados:** [familia], pesos [600, 700]
- **Cuerpo:** [familia], pesos [400, 500]
- **Escala:** ver tabla en `tokens.css`

[captura del type tester]

## 5. Espaciado
Escala base 4px. Tokens disponibles: `--space-1` (4) hasta `--space-16` (64).

## 6. Componentes base
[descripciones del Paso 26]

## 7. Tokens
Ver `tokens.css` en la raíz del proyecto.

## 8. Atribuciones
- Logo: generado con [...] el [fecha], refinado en [...].
- Tipografías: Google Fonts (OFL).
- Iconos: [lucide / heroicons / etc.] (MIT).
```

---

## Anexo D — Rúbrica de autoevaluación

Antes de entregar, marca cada ítem. Si tienes ≥ 14/16, estás listo:

- [ ] `UX_AUDIT.md` con ≥ 10 hallazgos y severidad asignada
- [ ] Top 3 problemas priorizados con recomendación
- [ ] `BRAND.md` con audiencia, promesa, 5 atributos coherentes
- [ ] Moodboard de ≥ 4 imágenes
- [ ] Paleta de 5+ colores con roles asignados
- [ ] Verificación WCAG AA documentada (capturas)
- [ ] Pareja tipográfica con justificación
- [ ] `tokens.css` completo (color + tipografía + espaciado + radios + sombras)
- [ ] Logo en sus 4 versiones dentro de `assets/logos/`
- [ ] Reglas de uso del logo (do's & don'ts) documentadas
- [ ] Atribución y licencia de IA documentadas
- [ ] `UI-KIT.md` con todos los componentes base
- [ ] Pareja de capturas antes/después de una pantalla
- [ ] `REFLEXION_UX.md` con las 8 preguntas
- [ ] Todo organizado en `entrega-ux/` dentro del repo
- [ ] Commit y push hechos

---

**Fin de la actividad.** Si llegaste hasta aquí, tu proyecto del portafolio acaba de pasar de "tiene una UI" a "tiene un sistema de diseño justificado y documentado". Eso es exactamente la diferencia entre un proyecto de estudiante y un proyecto profesional.
