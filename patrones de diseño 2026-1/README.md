# Patrones de Diseño 2026-1

**Asignatura:** TPY1101 — Taller Aplicado de Programación
**Semestre:** 2026-1
**Sección:** 001D

---

## Propósito de esta carpeta

Esta carpeta contiene **guías personalizadas** de patrones de diseño, arquitectura y stack tecnológico para cada uno de los siete equipos de la asignatura. Cada archivo está escrito específicamente para el proyecto de un equipo: se leyó la EP1 (Evaluación Parcial 1) del equipo, se identificaron las necesidades técnicas reales del dominio, y se seleccionaron los patrones de diseño y arquitectura que mejor resuelven esos desafíos.

El objetivo es que **cada equipo tenga una guía autodidáctica** que puedan leer de corrido antes de la actividad en clase, con:

- Análisis del problema específico desde el punto de vista técnico.
- Justificación de la arquitectura y el stack recomendado.
- Patrones frontend y backend con código adaptado a su dominio.
- Diagramas Mermaid de arquitectura y secuencia.
- Estructura de carpetas recomendada.
- Pasos concretos de despliegue en plataformas gratuitas.
- Riesgos, mitigaciones y checklist de buenas prácticas.
- Actividad de laboratorio de 1 hora 30 minutos.
- Desafíos opcionales y próximos pasos por sprint.

---

## Cómo usar esta carpeta

### Para estudiantes

1. **Identifiquen su archivo** según su número de grupo.
2. **Léanlo completo antes de la clase** de patrones (secciones 1 a 10).
3. Lleven el archivo abierto durante la actividad (sección 11).
4. Al terminar la actividad, suban el entregable (`ARQUITECTURA.md`) al repositorio de su equipo.

> Importante: **no compartan archivos entre equipos**. Cada archivo está afinado para un dominio distinto (móvil con bloqueo de apps, web TCG, agregador multimedia, IoT, gestión académica, generación con IA, BI conversacional). Lo que es ideal para un grupo puede ser un anti-patrón para otro.

### Para el docente

- El archivo maestro `Ejercicio_6_Patrones_Arquitectura_y_Despliegue.md` (en la carpeta `ejercicios-portafolio_titulo-001V/`) contiene los fundamentos teóricos comunes a todos los grupos.
- Los archivos de esta carpeta (`Grupo_N_NombreProyecto.md`) son la aplicación personalizada de esos fundamentos.
- La actividad de laboratorio de 90 minutos está calibrada en bloques (sección 11 de cada archivo) y produce un entregable estandarizado (`ARQUITECTURA.md`).

---

## Índice de archivos

| Grupo | Proyecto | Tipo | Patrón estrella | Archivo |
|---|---|---|---|---|
| **1** | MapacheSecure | Móvil — control parental gamificado | Repository + Middleware de perfil | [`Grupo_1_MapacheSecure.md`](./Grupo_1_MapacheSecure.md) |
| **2** | Deckora | Web — plataforma para TCG | Strategy (formatos) + Factory (tipos de carta) | [`Grupo_2_Deckora.md`](./Grupo_2_Deckora.md) |
| **3** | NoLimits | Web — agregador multimedia | Strategy + Adapter + Decorator de cache | [`Grupo_3_NoLimits.md`](./Grupo_3_NoLimits.md) |
| **4** | 40dB | Web + IoT — monitoreo de ruido urbano | Observer / Pub-Sub (MQTT) | [`Grupo_4_40dB.md`](./Grupo_4_40dB.md) |
| **5** | Pop Study | Web/PWA — gestión académica | Layered Architecture (monolito modular) | [`Grupo_5_PopStudy.md`](./Grupo_5_PopStudy.md) |
| **6** | Landing Pages IA | Web — generador asistido por IA | Strategy (proveedores IA) + Queue/Worker | [`Grupo_6_LandingPagesIA.md`](./Grupo_6_LandingPagesIA.md) |
| **7** | AGENTE X | Web — BI conversacional con agentes IA | Chain of Responsibility + Strategy + SSE | [`Grupo_7_AgenteX.md`](./Grupo_7_AgenteX.md) |

---

## Mapa rápido: dominio → patrones recomendados

La selección de patrones no es arbitraria. Se eligen por el problema de cada equipo:

- **MapacheSecure**: dos perfiles de usuario en el mismo dispositivo + offline-first → **Middleware de autorización por perfil**, **Repository** para cache local, **Provider** para AuthGate.
- **Deckora**: múltiples formatos de juego con reglas distintas → **Strategy** para validaciones de mazo; múltiples juegos a futuro → **Factory** para cartas.
- **NoLimits**: múltiples APIs externas con respuestas heterogéneas y rate limits → **Strategy + Adapter + Decorator de cache** (el triángulo clásico).
- **40dB**: ingesta de eventos desde sensores IoT → **Observer/Pub-Sub** vía MQTT, **Gateway** para encapsular, **Strategy** para clasificadores de ruido.
- **Pop Study**: proyecto de alcance medio con 2 personas → **Layered Architecture en monolito modular** (no microservicios), **Repository** y **Service** por módulo.
- **Landing Pages IA**: generación con IA lenta y costosa → **Queue/Worker (BullMQ)** y **Strategy por proveedor** para tolerancia a fallos.
- **AGENTE X**: orquestación de agentes IA con seguridad crítica → **Chain of Responsibility** en el pipeline, **Strategy** por agente, **SSE/Observer** para streaming.

---

## Estructura interna de cada archivo (15 secciones)

Todos los archivos siguen la misma estructura para facilitar la comparación entre equipos y para que el docente corrija con el mismo criterio:

1. Contexto del proyecto (según tu EP1).
2. Tarjeta técnica (resumen ejecutivo).
3. Análisis específico del problema.
4. Patrones recomendados para tu frontend.
5. Patrones recomendados para tu backend.
6. Arquitectura recomendada (2–3 diagramas Mermaid).
7. Estructura de carpetas recomendada.
8. Stack y despliegue paso a paso.
9. Riesgos y mitigaciones.
10. Checklist de buenas prácticas.
11. Actividad de 90 minutos (6 bloques).
12. Desafíos opcionales.
13. Próximos pasos (por sprint).
14. Recursos.
15. Cierre.

---

## Relación con el resto de la asignatura

Esta carpeta es **complementaria** al portafolio de título:

- El archivo maestro `Ejercicio_6_Patrones_Arquitectura_y_Despliegue.md` cubre la teoría general.
- Esta carpeta aterriza esa teoría en cada proyecto real.
- El entregable de cada equipo (`ARQUITECTURA.md`) debe quedar versionado en el repositorio del proyecto y servir como insumo para la EP2, EP3 y la defensa final.

---

## Entregable estandarizado

Al finalizar la actividad de 90 minutos, cada equipo debe haber producido en su propio repositorio un archivo `docs/ARQUITECTURA.md` con:

- Diagrama de arquitectura (Mermaid o imagen PNG) actualizado a su decisión final.
- Tabla de patrones elegidos + justificación de una línea.
- Estructura de carpetas decidida.
- Stack definitivo con links a las plataformas de despliegue.
- Tabla de riesgos con mitigación.
- Checklist marcado.

El docente revisa el commit y retroalimenta.

---

## Créditos

Guía elaborada para la sección 001D, semestre 2026-1, Escuela de Informática y Telecomunicaciones.
