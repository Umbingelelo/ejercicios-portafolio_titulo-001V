# Actividad: Flujo de trabajo correcto en GitHub (commits, PRs y protección de ramas)

## Contexto
En esta actividad configurarás y practicarás un flujo estándar de trabajo con **dos ramas principales**:

- **`main`**: representa el código estable/producción (o entregable final).
- **`dev`**: representa la rama de integración para desarrollo continuo.

Regla base: **siempre se trabaja desde `dev`** (nunca desde `main`), usando ramas de trabajo y **Pull Requests (PRs)**.

---

## Objetivos de aprendizaje
Al finalizar, podrás:

1. Aplicar una **política de commits** clara y consistente.
2. Entender y usar correctamente el concepto de **Pull Request** (revisión, checks, merge).
3. Trabajar con **ramas `main` y `dev`** usando un flujo “branch off dev”.
4. Configurar **protección de ramas** en GitHub (reglas y requisitos de aprobación).

---

## Duración sugerida
60–90 minutos (individual o en parejas).

---

## Requisitos
- Cuenta de GitHub
- Git instalado
- Un editor (VS Code recomendado)

---

# Parte A — Políticas del repositorio (lectura corta)

## 1) Política de commits (obligatoria)
### Regla 1: Commits pequeños y con intención única
- Un commit = un cambio lógico (no mezclar “arreglé bug + cambié UI + renombré carpetas”).

### Regla 2: Formato recomendado (Conventional Commits)
Usa este formato:

```
<tipo>(<scope opcional>): <mensaje corto en presente>
```

Tipos sugeridos:
- `feat`: nueva funcionalidad
- `fix`: corrección de bug
- `docs`: documentación
- `refactor`: refactor sin cambio funcional
- `test`: pruebas
- `chore`: tareas de mantención (deps, scripts, etc.)
- `ci`: cambios en pipelines/automatización

Ejemplos:
- `feat(auth): agregar login con Google`
- `fix(api): validar token expirado`
- `docs: actualizar README de despliegue`
- `refactor(ui): simplificar componente Header`

### Regla 3: Mensajes claros
- Primera línea idealmente ≤ 72 caracteres.
- Evita: “arreglos”, “cambios”, “update”, “final final”.

### Regla 4: Referencia a tarea/incidencia (si aplica)
Si usas tickets, incluye el ID en el cuerpo o al final:
- `Refs: PROJ-123`

### Regla 5: Prohibido commitear secretos
- Nunca subir keys, tokens, `.env`, credenciales, certificados privados.
- Usa `.env.example` y variables en CI/CD.

---

## 2) Concepto de Pull Request (PR)
Un PR es una **solicitud formal de integración** de cambios entre ramas. Sirve para:

- Revisar código (calidad, seguridad, estilo).
- Ejecutar checks automáticos (tests, lint, build).
- Registrar discusión y decisiones técnicas.
- Mantener control de lo que entra a `dev` y `main`.

**Regla general:**  
✅ Todo cambio hacia `dev` o `main` entra mediante PR.  
❌ No se permite push directo a `dev` o `main`.

---

## 3) Estrategia de ramas (main/dev)
### Flujo recomendado
1. Crear rama desde `dev`
2. Trabajar y commitear en tu rama
3. Abrir PR hacia `dev`
4. Revisar + aprobar + pasar checks
5. Hacer merge a `dev`
6. (Cuando corresponde) PR de `dev` → `main` para release

### Convención de nombres de ramas
- `feature/PROJ-123-login-google`
- `bugfix/PROJ-219-token-expirado`
- `hotfix/PROJ-310-fix-caida-prod` *(solo si hay urgencia real)*

---

# Parte B — Actividad práctica (paso a paso)

## B1) Crear repositorio y ramas base
1. Crea un repositorio nuevo en GitHub (privado o público).
2. Clónalo localmente.
3. Crea la rama `dev` desde `main` y súbela a remoto:

```bash
git checkout -b dev
git push -u origin dev
```

4. Asegúrate de que `main` y `dev` existan en GitHub.

---

## B2) Configurar protección de ramas (GitHub)
En tu repo: **Settings → Branches → Add branch protection rule**.

### Regla para `main` (recomendada “estricta”)
Aplicar a: `main`

Activa al menos:
- ✅ **Require a pull request before merging**
- ✅ **Require approvals**: mínimo **1** (ideal 2 en equipos grandes)
- ✅ **Dismiss stale pull request approvals when new commits are pushed** (recomendado)
- ✅ **Require status checks to pass before merging** *(si tienes CI; si no, déjalo listo para activar luego)*
- ✅ **Require conversation resolution before merging**
- ✅ **Require linear history** *(opcional; recomendado si usas rebase/squash)*
- ✅ **Include administrators** *(recomendado para evitar “saltarse reglas”)*  
Y desactiva:
- ❌ Allow force pushes
- ❌ Allow deletions

### Regla para `dev` (recomendada “intermedia”)
Aplicar a: `dev`

Activa al menos:
- ✅ Require a pull request before merging
- ✅ Require approvals: mínimo **1**
- ✅ Require conversation resolution before merging
- (Opcional) Require status checks to pass

Misma recomendación:
- ❌ No force push
- ❌ No deletions

---

## B3) Crear una rama de trabajo desde `dev`
1. Cámbiate a `dev` y actualiza:

```bash
git checkout dev
git pull
```

2. Crea una rama `feature/...`:

```bash
git checkout -b feature/PROJ-101-readme-mejorado
```

---

## B4) Implementar un cambio simple
Tarea: crea/edita un `README.md` y agrega:

- Un título del proyecto
- Cómo instalar
- Cómo ejecutar
- Una sección “Contribuir” que mencione PRs y ramas

Ejemplo de commit:

```bash
git add README.md
git commit -m "docs: agregar guia de instalacion y contribucion"
git push -u origin feature/PROJ-101-readme-mejorado
```

---

## B5) Abrir un Pull Request hacia `dev`
En GitHub:
1. Abre PR desde `feature/PROJ-101-readme-mejorado` → `dev`
2. Completa una descripción usando esta plantilla:

### Plantilla de PR (copia/pega)
**¿Qué cambia?**
- …

**¿Por qué?**
- …

**Cómo probar**
- [ ] `npm test` (si aplica)
- [ ] `npm run build` (si aplica)
- Pasos manuales: …

**Checklist**
- [ ] Commits con formato acordado
- [ ] No hay secretos/keys en los cambios
- [ ] Documentación actualizada (si aplica)

3. Pide revisión a un compañero (o revisa tú mismo si estás solo, pero simula el proceso).

---

## B6) Aprobar, mergear y verificar reglas
- Intenta mergear sin aprobación (debería bloquear).
- Consigue **1 aprobación** y verifica que ahora permite merge.
- Realiza el merge hacia `dev`.

> Recomendación de método de merge:
> - **Squash and merge** para PRs de feature/bugfix (historial limpio).
> - **Merge commit** para PR de `dev` → `main` (release) si quieres conservar contexto del release.

---

## B7) Simular un release (PR de `dev` a `main`)
1. Abre un PR desde `dev` → `main`
2. Titúlalo: `release: PROJ-101 README + docs`
3. Asegura al menos 1 aprobación y mergea.

---

# Parte C — Entregables
Entrega (en un comentario o documento):

1. Link al repositorio.
2. Screenshot (o descripción) de las reglas de protección aplicadas a:
   - `main`
   - `dev`
3. Link a:
   - PR feature → dev
   - PR dev → main
4. 3 commits que sigan la política (copiar el hash + mensaje).

---

# Parte D — Criterios de evaluación (rúbrica breve)
| Criterio | Logrado | Parcial | No logrado |
|---|---|---|---|
| Política de commits aplicada (formato + claridad) |  |  |  |
| Uso correcto de ramas (desde dev, nombres correctos) |  |  |  |
| PR completo (descripción, checklist, pruebas) |  |  |  |
| Protección de ramas configurada (mín. 1 aprobación) |  |  |  |
| No se realizaron pushes directos a main/dev |  |  |  |

---

# Apéndice — Reglas rápidas (para pegar en el README)
## Reglas del repo
- Trabajamos con `main` (estable) y `dev` (integración).
- Las ramas de trabajo **salen desde `dev`**.
- **Todo** cambio entra por PR (no push directo a `dev` o `main`).
- `main` y `dev` requieren al menos **1 aprobación** antes de merge.
- Commits con formato **Conventional Commits**.
- Prohibido commitear secretos.
