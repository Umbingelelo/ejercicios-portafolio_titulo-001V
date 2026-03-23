# Flujo de trabajo profesional con Git y GitHub

### Commits · Ramas · Pull Requests · Protección de ramas

> **Duración:** ~90 minutos · **Modalidad:** Individual o en parejas  
> **Nivel:** Intermedio (se asume conocimiento básico de Git)

---

## Objetivos de aprendizaje

Al finalizar esta actividad podrás:

1. Explicar cómo funciona Git internamente (objetos, DAG, HEAD).
2. Aplicar una política de commits clara usando **Conventional Commits**.
3. Manejar ramas con confianza: crear, fusionar y limpiar.
4. Entender la diferencia entre `merge`, `rebase` y `squash`.
5. Usar **Pull Requests** como herramienta de revisión y trazabilidad.
6. Configurar **protección de ramas** en GitHub.

---

## Agenda sugerida

| Bloque | Contenido | Tiempo estimado |
|---|---|---|
| A | Conceptos clave de Git | 20 min |
| B | Política de commits y ramas | 10 min |
| C | Actividad práctica paso a paso | 50 min |
| D | Entregables y rúbrica | 10 min |

---

# Parte A — Conceptos clave de Git (teoría antes de la práctica)

## A1) ¿Qué es Git realmente? El modelo de objetos

Git no es solo un "sistema de versiones". Es una **base de datos de objetos inmutables** identificados por su hash SHA-1. Existen 4 tipos de objetos:

| Objeto | Descripción |
|---|---|
| **blob** | Contenido de un archivo (sin nombre, sin ruta) |
| **tree** | Directorio: lista de blobs y otros trees con sus nombres |
| **commit** | Apunta a un tree + tiene autor, mensaje y uno o más padres |
| **tag** | Etiqueta firmada que apunta a un commit |

```
commit C3
│  tree ──► tree raíz ──► blob (README.md)
│                     └──► blob (src/main.js)
│  parent ──► commit C2
│  author: Ana
│  message: "feat: agregar login"
```

> 💡 **Clave conceptual:** Un commit no "guarda diferencias". Guarda un snapshot completo del árbol. Git calcula diffs *al mostrarlos*, no al almacenarlos.

---

## A2) El DAG (Grafo Acíclico Dirigido)

El historial de commits forma un **grafo dirigido sin ciclos**. Cada commit apunta a su(s) padre(s):

```
main:   A ── B ── C ── M (merge commit)
                  │   ↗
feature:          D ── E
```

- Las **ramas** son simplemente punteros (archivos en `.git/refs/heads/`) que apuntan al commit más reciente.
- **HEAD** es el puntero especial que indica dónde estás ahora mismo. Normalmente apunta a una rama; si apunta directo a un commit se llama "detached HEAD".

```bash
cat .git/HEAD          # → ref: refs/heads/main
cat .git/refs/heads/main  # → hash del último commit
```

---

## A3) Las tres zonas de trabajo

Este es uno de los conceptos más confusos para principiantes. Git tiene tres zonas:

```
 Working Directory   →   Staging Area (Index)   →   Repository (.git)
  (tus archivos)          git add                    git commit
```

| Zona | Descripción | Cómo ver su estado |
|---|---|---|
| **Working Directory** | Archivos en disco, con o sin cambios | `git status` |
| **Staging Area (Index)** | Cambios preparados para el próximo commit | `git diff --staged` |
| **Repository** | Historia permanente de commits | `git log` |

```bash
# Ver diferencia entre zonas
git diff              # Working Directory vs Staging
git diff --staged     # Staging vs último commit
git diff HEAD         # Working Directory vs último commit
```

---

## A4) `merge` vs `rebase` vs `squash` — ¿cuándo usar cada uno?

Esta es la decisión de flujo más importante del día a día.

### `git merge` (fast-forward o merge commit)

Integra una rama **preservando toda la historia**. Si el destino no ha avanzado, Git hace un *fast-forward* (solo mueve el puntero, sin commit de merge). Si ambas ramas divergieron, crea un **merge commit** con dos padres.

```
Antes:          A ── B (main)
                     └── C ── D (feature)

Después merge:  A ── B ── M (main)  ← merge commit
                     └── C ── D ──┘
```

**Cuándo usarlo:** PR de `dev` → `main` (release). Quieres conservar el contexto de cuándo se integró.

---

### `git rebase`

"Reescribe" los commits de una rama como si hubieran partido desde otro punto. **Cambia los hashes** de los commits rebasados (son objetos nuevos).

```
Antes:   A ── B (main)
              └── C ── D (feature)

Después rebase feature sobre main:
         A ── B (main)
              └── C' ── D' (feature)  ← nuevos hashes
```

**Cuándo usarlo:** Antes de abrir un PR, para que tu rama esté al día con `dev` sin crear merges innecesarios. **Nunca rebasees ramas compartidas (públicas).**

```bash
git checkout feature/mi-rama
git rebase dev         # "aplana" mi rama sobre dev actualizado
```

> ⚠️ **Regla de oro:** No hagas `rebase` sobre commits que ya están en el remoto y que otros pueden haber descargado. Reescribir historia compartida causa conflictos para todos.

---

### Squash and merge

Aplasta todos los commits de la rama en **un solo commit** al hacer merge.

```
Antes:  dev ... C1 (feature/login: feat + fix + typo + wip + más fix)

Después squash merge a dev:
        dev ... C1 ── [feat(auth): agregar login] ← un solo commit limpio
```

**Cuándo usarlo:** PRs de feature/bugfix hacia `dev`. Mantiene el historial de `dev` limpio y legible. Los commits individuales de trabajo (ej: "wip", "arreglé typo") no ensucian el log principal.

---

### Resumen de estrategias

| Situación | Estrategia recomendada |
|---|---|
| Feature/bugfix PR → `dev` | **Squash and merge** |
| `dev` → `main` (release) | **Merge commit** |
| Actualizar tu rama con `dev` antes de PR | **Rebase** (local) |
| Ramas de larga duración (excepcional) | **Merge** |

---

## A5) Comandos Git que todo desarrollador debe dominar

### Inspección del historial

```bash
git log --oneline --graph --all          # historial visual compacto
git log --oneline -10                    # últimos 10 commits
git show <hash>                          # detalle de un commit específico
git diff main..feature/mi-rama          # diferencia entre ramas
git blame archivo.js                     # quién escribió cada línea
```

### Deshacer cambios (con cuidado)

```bash
# Deshacer staging (no afecta archivos)
git restore --staged archivo.js

# Descartar cambios en working directory (¡destructivo!)
git restore archivo.js

# Crear un commit que revierte otro (seguro, no reescribe historia)
git revert <hash>

# Mover HEAD hacia atrás (reescribe historia — solo en local)
git reset --soft HEAD~1    # deshace commit, mantiene staging
git reset --mixed HEAD~1   # deshace commit y staging (default)
git reset --hard HEAD~1    # deshace todo (¡archivos incluidos!)
```

> 💡 `git revert` es seguro para ramas compartidas. `git reset --hard` nunca en `main` o `dev`.

### Guardar trabajo temporal

```bash
git stash                    # guarda working directory temporalmente
git stash pop                # recupera el último stash
git stash list               # ver todos los stashes
git stash drop stash@{0}     # eliminar un stash específico
```

### Buscar en la historia

```bash
git log --grep="PROJ-123"            # buscar en mensajes de commit
git log -S "nombre_funcion"          # buscar cuándo se agregó/eliminó código
git bisect start                     # búsqueda binaria para encontrar bug
```

---

# Parte B — Políticas del repositorio

## B1) Política de commits (Conventional Commits)

### Regla 1: Un commit = una intención

No mezclar "arreglé bug + cambié estilos + renombré carpeta" en un solo commit. Cada commit debe ser reversible de forma independiente.

### Regla 2: Formato estándar

```
<tipo>(<scope opcional>): <mensaje corto en presente imperativo>

[cuerpo opcional: explica el QUÉ y el POR QUÉ, no el cómo]

[footer opcional: referencias, breaking changes]
```

**Tipos:**

| Tipo | Uso |
|---|---|
| `feat` | Nueva funcionalidad |
| `fix` | Corrección de bug |
| `docs` | Solo documentación |
| `refactor` | Refactor sin cambio funcional |
| `test` | Agregar o corregir pruebas |
| `chore` | Mantenimiento (deps, configs) |
| `ci` | Cambios en pipelines/CI |
| `perf` | Mejoras de rendimiento |
| `revert` | Revertir un commit anterior |

**Ejemplos bien escritos:**

```bash
feat(auth): agregar login con Google OAuth
fix(api): validar token expirado en middleware
docs: actualizar README con instrucciones de despliegue
refactor(ui): simplificar componente Header eliminando prop drilling
test(auth): agregar casos de borde para token inválido
chore(deps): actualizar dependencias de seguridad
```

**Ejemplos MAL escritos (evitar):**

```bash
arreglos
cambios varios
update
fix
wip
final final2
```

### Regla 3: Primera línea ≤ 72 caracteres, en presente imperativo

"Agrega", "Corrige", "Actualiza" — no "Agregué" ni "Agregando".

### Regla 4: Referencia a tickets (si aplica)

```
feat(auth): agregar login con Google

Implementa flujo OAuth 2.0 con manejo de refresh tokens.
Se eligió este enfoque por compatibilidad con el SDK existente.

Refs: PROJ-123
```

### Regla 5: Nunca commitear secretos

- Usar `.gitignore` para `.env`, `*.pem`, `*.key`, `credentials.json`
- Incluir siempre un `.env.example` con las variables sin valores reales
- Si accidentalmente commiteaste un secreto: rotarlo inmediatamente (el historial de Git es público)

---

## B2) Estrategia de ramas (Gitflow simplificado)

```
main   ──────────────────────────────────────────────► (producción)
          ↑                                  ↑
          └── PR release                    PR release
                                              │
dev    ──────────────────────────────────────────────► (integración)
          ↑             ↑           ↑
          PR feat        PR fix      PR feat
          │              │           │
feature/  ───────        ────        ──────────
bugfix/
```

### Convención de nombres de ramas

```bash
feature/PROJ-123-login-google
bugfix/PROJ-219-token-expirado
hotfix/PROJ-310-caida-produccion   # urgente, sale desde main
docs/PROJ-400-actualizar-readme
refactor/PROJ-501-extraer-servicio-auth
```

### Reglas fundamentales

- ✅ Las ramas de trabajo **siempre salen desde `dev`**
- ✅ Todo cambio hacia `dev` o `main` entra por **Pull Request**
- ✅ Borrar ramas después del merge (GitHub puede hacerlo automáticamente)
- ❌ Nunca push directo a `main` o `dev`
- ❌ Nunca `git push --force` a ramas compartidas

---

# Parte C — Actividad práctica

## C1) Setup: Crear repositorio y ramas base

**Tiempo estimado: 5 minutos**

1. Crea un repositorio nuevo en GitHub (público o privado).
2. Clónalo localmente:

```bash
git clone https://github.com/TU_USUARIO/TU_REPO.git
cd TU_REPO
```

1. Verifica la configuración de Git:

```bash
git config --global user.name "Tu Nombre"
git config --global user.email "tu@email.com"
git config --global core.editor "code --wait"   # VS Code como editor por defecto
```

1. Crea la rama `dev` y súbela:

```bash
git checkout -b dev
git push -u origin dev
```

1. Verifica que ambas ramas existen en GitHub (pestaña "branches").

> 💡 **Por qué -u?** El flag `-u` establece el "upstream tracking", enlazando tu rama local con el remoto. De aquí en adelante, `git push` y `git pull` sin argumentos sabrán a dónde ir.

---

## C2) Configurar protección de ramas en GitHub

**Tiempo estimado: 10 minutos**

Ve a: **Settings → Branches → Add branch protection rule**

### Regla para `main` (estricta)

| Opción | Valor |
|---|---|
| Branch name pattern | `main` |
| Require a pull request before merging | ✅ |
| Require approvals | ✅ mínimo 1 |
| Dismiss stale PR approvals when new commits are pushed | ✅ |
| Require status checks to pass before merging | ✅ (si tienes CI) |
| Require conversation resolution before merging | ✅ |
| Require linear history | ✅ (recomendado) |
| Include administrators | ✅ |
| Allow force pushes | ❌ |
| Allow deletions | ❌ |

### Regla para `dev` (intermedia)

| Opción | Valor |
|---|---|
| Branch name pattern | `dev` |
| Require a pull request before merging | ✅ |
| Require approvals | ✅ mínimo 1 |
| Require conversation resolution before merging | ✅ |
| Allow force pushes | ❌ |
| Allow deletions | ❌ |

> 💡 **Por qué "Include administrators"?** Sin esta opción, los admins pueden saltarse las reglas. En equipos pequeños donde el dueño del repo es también desarrollador, es fácil hacer un push directo "por accidente". Activarla obliga a todos — sin excepciones.

---

## C3) Primera rama de trabajo: documentación

**Tiempo estimado: 10 minutos**

1. Asegúrate de estar en `dev` y actualizado:

```bash
git checkout dev
git pull origin dev
```

1. Crea tu rama de trabajo:

```bash
git checkout -b feature/PROJ-101-readme-inicial
```

1. Verifica dónde estás:

```bash
git branch                  # lista ramas locales, marca la activa con *
git log --oneline --all     # visualiza el historial
```

1. Crea el archivo `README.md`:

```markdown
# Mi Proyecto

## Descripción
[Descripción breve del proyecto]

## Requisitos
- Node.js >= 18
- npm >= 9

## Instalación

\```bash
git clone <url-del-repo>
cd mi-proyecto
npm install
\```

## Uso

\```bash
npm start         # modo desarrollo
npm run build     # build de producción
npm test          # ejecutar pruebas
\```

## Contribuir

Lee nuestras reglas del repo antes de contribuir:

- Trabajamos con `main` (estable) y `dev` (integración).
- Las ramas de trabajo salen **siempre desde `dev`**.
- Todo cambio entra por **Pull Request** (no push directo).
- Commits con formato **Conventional Commits**.
- `main` y `dev` requieren al menos **1 aprobación** para merge.

### Flujo básico

\```bash
git checkout dev && git pull
git checkout -b feature/PROJ-XXX-descripcion
# ... trabajas ...
git add .
git commit -m "feat: descripción del cambio"
git push -u origin feature/PROJ-XXX-descripcion
# Abre un Pull Request hacia dev en GitHub
\```
```

1. Haz el commit:

```bash
git add README.md
git commit -m "docs: agregar README con instalacion y guia de contribucion"
git push -u origin feature/PROJ-101-readme-inicial
```

---

## C4) Segundo commit en la misma rama (práctica de commits atómicos)

**Tiempo estimado: 5 minutos**

Agrega un archivo `.gitignore` en un commit **separado** (cambio independiente):

```bash
# Crea el .gitignore
cat > .gitignore << 'EOF'
# Dependencias
node_modules/
.pnp
.pnp.js

# Build
dist/
build/
.next/
out/

# Variables de entorno (NUNCA commitear)
.env
.env.local
.env.*.local

# Ejemplo de variables (SÍ commitear)
# .env.example → este SÍ va al repo

# Logs
*.log
npm-debug.log*

# IDE
.vscode/settings.json
.idea/
*.swp
*.swo

# OS
.DS_Store
Thumbs.db
EOF
```

```bash
git add .gitignore
git commit -m "chore: agregar gitignore para node y entorno"
```

Agrega también el `.env.example`:

```bash
cat > .env.example << 'EOF'
# Copia este archivo como .env y completa los valores
DATABASE_URL=postgres://user:password@localhost:5432/mydb
API_KEY=your_api_key_here
JWT_SECRET=your_secret_here
PORT=3000
EOF
```

```bash
git add .env.example
git commit -m "chore: agregar env.example con variables requeridas"
git push
```

> 🔍 **Observa:** Tienes 3 commits en tu rama, cada uno con una intención clara y diferente. Esto facilita los code reviews y hace que `git revert` sea quirúrgico si algo falla.

---

## C5) Abrir un Pull Request hacia `dev`

**Tiempo estimado: 10 minutos**

En GitHub, ve a tu repositorio y abre un **Pull Request**:

- Base: `dev`
- Compare: `feature/PROJ-101-readme-inicial`

Usa esta plantilla para la descripción:

---

**¿Qué cambia?**

- Agrega `README.md` con instrucciones de instalación, uso y contribución
- Agrega `.gitignore` para Node.js y variables de entorno
- Agrega `.env.example` con las variables requeridas por el proyecto

**¿Por qué?**

- El repo carecía de documentación mínima para que nuevos colaboradores puedan onboardearse
- El `.gitignore` previene que se acumulen archivos de build y credenciales en el repo

**Cómo probar**

- [ ] Clonar el repo desde cero y seguir las instrucciones del README
- [ ] Verificar que `.env` no aparece en `git status` tras crearlo localmente

**Checklist**

- [ ] Commits con formato Conventional Commits
- [ ] No hay secretos ni `.env` reales en los cambios
- [ ] El `.env.example` incluye todas las variables necesarias
- [ ] Sin comentarios `TODO` pendientes

---

### Conceptos clave de un PR

- **Reviewers**: personas asignadas a revisar. Sus comentarios pueden bloquear el merge.
- **Checks**: automatizaciones (CI/CD) que validan el código antes del merge.
- **Conversations**: hilos de discusión. Con la protección activada, deben resolverse antes de mergear.
- **Files changed**: diff visual de todos los cambios.

---

## C6) Simular un code review

**Tiempo estimado: 5 minutos**

Si trabajas en pareja, que tu compañero:

1. Revise los archivos cambiados.
2. Deje al menos **un comentario** (ej: sugerir mejorar la descripción de una variable en `.env.example`).
3. Apruebe el PR.

Si trabajas solo:

1. Intenta mergear sin aprobación → GitHub debería **bloquearlo**.
2. Ve a tu perfil → revisa el PR → "Approve".
3. Ahora sí puedes mergear.

**Tipo de merge recomendado:** `Squash and merge` — los 3 commits se aplastan en uno solo en `dev`.

---

## C7) Actualizar tu entorno local tras el merge

**Tiempo estimado: 3 minutos**

```bash
git checkout dev
git pull origin dev           # traer el commit squasheado

# Limpiar la rama local que ya no necesitas
git branch -d feature/PROJ-101-readme-inicial

# Ver el estado actual del historial
git log --oneline --graph --all
```

> 💡 **Buena práctica:** Borrar ramas después del merge evita acumular ramas obsoletas. GitHub puede configurarse para hacerlo automáticamente (Settings → General → "Automatically delete head branches").

---

## C8) Segunda iteración: agregar código real

**Tiempo estimado: 10 minutos**

Practica el flujo completo una segunda vez con un cambio más concreto.

1. Crea una nueva rama desde `dev`:

```bash
git checkout dev && git pull
git checkout -b feature/PROJ-102-estructura-proyecto
```

1. Crea la estructura básica de un proyecto:

```bash
mkdir -p src tests

cat > src/index.js << 'EOF'
/**
 * Punto de entrada principal de la aplicación.
 */
function main() {
  console.log("Hola desde el proyecto");
}

main();
EOF

cat > tests/index.test.js << 'EOF'
// Pruebas básicas del módulo principal
describe("main", () => {
  test("debería ejecutarse sin errores", () => {
    expect(true).toBe(true); // placeholder
  });
});
EOF
```

1. Haz commits separados (¡practica la atomicidad!):

```bash
git add src/
git commit -m "feat: agregar estructura inicial de src con punto de entrada"

git add tests/
git commit -m "test: agregar suite de pruebas placeholder para main"

git push -u origin feature/PROJ-102-estructura-proyecto
```

1. Abre PR hacia `dev`, completa la plantilla, aprueba y mergea con **Squash and merge**.

---

## C9) Simular un Release: PR de `dev` a `main`

**Tiempo estimado: 5 minutos**

Cuando `dev` tiene un conjunto coherente de cambios listos para producción, se abre un PR de release:

1. Abre PR en GitHub:
   - Base: `main`
   - Compare: `dev`
   - Título: `release: v0.1.0 — docs, estructura inicial y pruebas`

2. En la descripción del PR de release, documenta **qué incluye este release**:

```
## Release v0.1.0

### Cambios incluidos
- docs(PROJ-101): README con guía de instalación y contribución
- chore(PROJ-101): gitignore y env.example
- feat(PROJ-102): estructura inicial de src/
- test(PROJ-102): suite de pruebas placeholder

### Notas de despliegue
- Sin cambios de base de datos
- Sin variables de entorno nuevas
```

1. Consigue aprobación y haz merge con **Merge commit** (no squash — quieres conservar el contexto del release en el historial de `main`).

---

## C10) (Opcional) Resolución de conflictos

**Tiempo estimado: 7 minutos**

Practica la situación más incómoda del trabajo colaborativo: los conflictos.

1. Desde `dev`, crea dos ramas que modifiquen la misma línea:

```bash
git checkout dev && git pull

git checkout -b feature/PROJ-103-titulo-rama-a
# Edita README.md: cambia el título a "# Mi Super Proyecto"
git add README.md && git commit -m "docs: actualizar titulo del proyecto"
git push -u origin feature/PROJ-103-titulo-rama-a
# Mergea esta rama a dev (squash and merge en GitHub)

git checkout dev && git pull
git checkout -b feature/PROJ-104-titulo-rama-b
# Edita README.md: cambia el título a "# Proyecto Principal"
git add README.md && git commit -m "docs: renombrar titulo del proyecto"
git push -u origin feature/PROJ-104-titulo-rama-b
```

1. Al intentar abrir el PR de la segunda rama, GitHub mostrará conflictos. Resuélvelos localmente:

```bash
git checkout feature/PROJ-104-titulo-rama-b
git fetch origin
git rebase origin/dev      # intentará aplicar tus commits sobre dev actualizado
```

1. Git pausará en el conflicto. Abre el archivo, verás marcadores:

```
<<<<<<< HEAD (dev)
# Mi Super Proyecto
=======
# Proyecto Principal
>>>>>>> tu commit
```

1. Elige la versión correcta (o combina), elimina los marcadores, y continúa:

```bash
git add README.md
git rebase --continue
git push --force-with-lease   # único caso donde force push es aceptable
```

> ⚠️ `--force-with-lease` es más seguro que `--force`: verifica que nadie más haya pusheado a esa rama antes de sobreescribir.

---

# Parte D — Entregables

Entrega un comentario o documento con:

1. **Link al repositorio** en GitHub.
2. **Screenshot** de las reglas de protección configuradas para `main` y `dev`.
3. **Links a los PRs:**
   - PR feature → dev (PROJ-101 o PROJ-102)
   - PR dev → main (release)
4. **3 commits** que sigan la política. Incluye el hash completo y el mensaje:

   ```bash
   git log --oneline -5   # copia los hashes y mensajes
   ```

5. **Responde brevemente:**
   - ¿Qué diferencia hay entre `git merge` y `git rebase`? ¿Cuándo usarías cada uno?
   - ¿Por qué `squash and merge` mantiene el historial de `dev` más limpio?
   - ¿Qué pasa si haces `git reset --hard` en una rama que ya está en el remoto compartido?

---

# Parte E — Rúbrica de evaluación

| Criterio | Logrado ✅ | Parcial ⚠️ | No logrado ❌ |
|---|---|---|---|
| Commits con Conventional Commits (formato + claridad) | Todos los commits siguen el formato | Mayoría correctos, algunos sin tipo | Mensajes genéricos o sin formato |
| Commits atómicos (una intención por commit) | Cambios separados en commits distintos | Algunos commits mezclan cambios | Todo en un solo commit |
| Uso correcto de ramas (desde dev, nombres con convención) | Nombres correctos, salen desde dev | Nombre correcto pero sin prefijo PROJ | Ramas creadas desde main o sin convención |
| PR completo (descripción, checklist, pruebas) | Plantilla completa con todos los campos | Plantilla parcialmente completada | Sin descripción o plantilla vacía |
| Protección de ramas configurada (mín. 1 aprobación) | Ambas ramas protegidas correctamente | Solo main protegida | Sin protección configurada |
| Sin pushes directos a main/dev | Solo merges vía PR | 1 push directo accidental | Múltiples pushes directos |
| Conceptos teóricos (preguntas de la Parte D) | Respuestas claras y correctas | Respuestas parciales o confusas | Sin responder |

---

# Apéndice — Cheatsheet de comandos

## Flujo del día a día

```bash
# Empezar a trabajar
git checkout dev && git pull
git checkout -b feature/PROJ-XXX-descripcion

# Trabajando...
git status
git diff
git add <archivo>           # staging selectivo
git add -p                  # staging interactivo (fragmentos)
git commit -m "tipo(scope): mensaje"

# Subir y abrir PR
git push -u origin feature/PROJ-XXX-descripcion
# → Abrir PR en GitHub hacia dev

# Después del merge, limpiar
git checkout dev && git pull
git branch -d feature/PROJ-XXX-descripcion
```

## Inspección

```bash
git log --oneline --graph --all    # historial visual
git show <hash>                    # detalle de un commit
git diff rama-a..rama-b            # diferencia entre ramas
git blame archivo.js               # quién escribió cada línea
```

## Deshacer (con precaución)

```bash
git restore --staged archivo       # sacar del staging
git restore archivo                # descartar cambios (¡destructivo!)
git revert <hash>                  # revertir commit (seguro, crea nuevo commit)
git reset --soft HEAD~1            # deshacer último commit, mantener staging
git stash / git stash pop          # guardar/recuperar trabajo temporal
```

## Sincronización

```bash
git fetch origin                   # traer cambios del remoto sin mergear
git pull                           # fetch + merge (o rebase si configurado)
git push --force-with-lease        # push forzado seguro (solo en tu rama, tras rebase)
```

---

## Reglas del repositorio (para pegar en tu README)

```markdown
## Reglas del repo

- Trabajamos con `main` (estable) y `dev` (integración).
- Las ramas de trabajo **siempre salen desde `dev`**.
- Todo cambio entra por **Pull Request** (no push directo a `dev` o `main`).
- `main` y `dev` requieren al menos **1 aprobación** antes del merge.
- Commits con formato **Conventional Commits**.
- PRs hacia `dev`: usar **Squash and merge**.
- PRs de release (`dev` → `main`): usar **Merge commit**.
- **Prohibido** commitear secretos, tokens o credenciales.
- Borrar la rama después del merge.
```
