# Actividad de Laboratorio: Autenticación y Seguridad con JWT, bcrypt y Middleware en Express

## Datos generales

**Asignatura:** TPY1101 – Taller Aplicado de Programación
**Duración estimada:** 2 horas
**Modalidad:** Individual o en parejas
**Sistema operativo considerado:** Windows / macOS / Linux
**Stack de referencia del curso:** React (frontend) + Node.js/Express (backend) + Supabase/PostgreSQL
**Herramientas principales:** Node.js, Express, Postman / Thunder Client / `curl`, VS Code
**Aplicabilidad:** este ejercicio es **agnóstico al dominio** del proyecto. Sirve para cualquier proyecto del portafolio (MapacheSecure, Deckora, NoLimits, 40dB, Pop Study, Landing Pages IA, Agente X u otros), porque trabaja sobre un mini-backend independiente que después podrás reutilizar como módulo de autenticación en tu aplicación real.

---

## 1. Propósito de la actividad

En esta actividad implementarás un **mini-servicio de autenticación y autorización** en Node.js + Express, aplicando las prácticas de seguridad mínimas que se esperan de cualquier backend profesional:

- separar **autenticación** (¿quién eres?) de **autorización** (¿qué puedes hacer?);
- almacenar contraseñas correctamente con **hashing usando bcrypt**;
- emitir y validar **tokens JWT (JSON Web Tokens)**;
- proteger rutas con un **middleware de autenticación** en Express;
- aplicar **headers de seguridad** y buenas prácticas para no exponer información sensible.

Al finalizar, deberías ser capaz de:

- explicar la diferencia entre autenticación y autorización con ejemplos concretos;
- registrar y autenticar usuarios sin guardar contraseñas en texto plano;
- emitir un JWT en `/login` y validarlo en un endpoint protegido;
- escribir un middleware de Express reutilizable para proteger cualquier ruta;
- detectar al menos tres errores comunes de seguridad y corregirlos.

---

## 2. Contexto del caso

Cualquier proyecto del curso, tarde o temprano, necesita responder dos preguntas básicas antes de dejar a un usuario hacer algo:

1. **¿Estoy seguro de quién es este usuario?** → Autenticación.
2. **¿Tiene permiso para hacer lo que está pidiendo?** → Autorización.

Para esta actividad construirás un **mini-API de autenticación** con tres rutas:

- `POST /api/auth/register` → crea un usuario y guarda su contraseña con hash.
- `POST /api/auth/login` → verifica credenciales y devuelve un JWT.
- `GET /api/perfil` → ruta **protegida** que solo responde si el JWT es válido.

Este mini-servicio es deliberadamente pequeño para que puedas terminarlo en 2 horas sin instalar herramientas pesadas. La idea es que, una vez que funcione, puedas **portarlo o adaptarlo** a tu proyecto del portafolio (login real, perfil de usuario, rutas administrativas, etc.).

---

## 3. Producto esperado

Al finalizar la actividad, cada estudiante o equipo debe contar con:

1. una carpeta de proyecto Node.js funcionando;
2. una API en `http://localhost:3000` con rutas `/api/auth/register`, `/api/auth/login` y `/api/perfil`;
3. un archivo `usuarios.json` con al menos 2 usuarios registrados, **con contraseñas hasheadas** (no en texto plano);
4. un middleware reutilizable `authMiddleware.js`;
5. headers de seguridad activados (con `helmet` o equivalente manual);
6. un archivo `.env` con la `JWT_SECRET` (no commiteado al repositorio);
7. evidencias mínimas de pruebas hechas con Postman / Thunder Client / `curl`;
8. una breve reflexión final con las preguntas de cierre respondidas.

---

## 4. Requisitos previos

Antes de comenzar, verifica que tienes instalado:

- **Node.js LTS** (≥ 18)
- **VS Code**
- **Postman**, **Thunder Client** (extensión de VS Code) o `curl`

### Verificación inicial

Abre una terminal (PowerShell, Terminal o bash) y ejecuta:

```bash
node -v
npm -v
```

### Resultado esperado

Debes ver una versión válida para cada herramienta (por ejemplo `v20.11.0` y `10.2.4`).

### Si algo falla

- Si `node` no existe, instala Node.js LTS desde el sitio oficial.
- Si quieres una alternativa gráfica a `curl`, instala Postman o la extensión **Thunder Client** en VS Code (un solo clic, sin cuenta).

**Checkpoint 0 aprobado** cuando puedes mostrar en pantalla las versiones de `node` y `npm`, y tienes elegida tu herramienta para probar la API (Postman, Thunder Client o `curl`).

---

## 5. Organización del tiempo

Distribuye el trabajo de la siguiente forma:

- **Bloque 1 – Conceptos clave (Auth vs AuthZ, JWT, hashing):** 15 minutos
- **Bloque 2 – Preparación del entorno y estructura del proyecto:** 15 minutos
- **Bloque 3 – Registro con bcrypt y login con JWT:** 30 minutos
- **Bloque 4 – Middleware de autenticación y ruta protegida:** 25 minutos
- **Bloque 5 – Endurecimiento de seguridad (headers, errores, secretos):** 20 minutos
- **Bloque 6 – Pruebas, reflexión y cierre:** 15 minutos

**Tiempo total estimado:** 120 minutos

---

# 6. Desarrollo paso a paso

---

## Bloque 1 – Conceptos clave (15 minutos)

### 1.1. Autenticación vs Autorización

Estos dos conceptos suenan parecidos pero **no son lo mismo**, y confundirlos es una de las causas más frecuentes de fallas de seguridad en proyectos de estudiantes.

| | **Autenticación (AuthN)** | **Autorización (AuthZ)** |
|---|---|---|
| Pregunta que responde | ¿Quién eres? | ¿Qué tienes permiso de hacer? |
| Cuándo ocurre | Al iniciar sesión | En cada operación protegida |
| Ejemplo cotidiano | Mostrar tu carnet en la puerta del edificio | El guardia decide a qué pisos puedes subir |
| Implementación típica | Email + contraseña, OAuth, JWT | Roles (`admin`, `user`), permisos, RBAC |
| Resultado | Un identificador del usuario (id, email) | Un permitir/denegar la acción |

> **Clave:** un usuario puede estar **autenticado** (sabemos quién es) pero **no autorizado** (no puede entrar a la ruta de admin). Son dos verificaciones distintas.

### 1.2. ¿Qué es un JWT?

Un **JSON Web Token (JWT)** es un string firmado digitalmente que contiene información del usuario en formato JSON, codificado en Base64. Tiene tres partes separadas por puntos:

```
HEADER.PAYLOAD.SIGNATURE
```

- **Header:** algoritmo de firma (por ejemplo `HS256`).
- **Payload:** los *claims* o datos que viajan dentro del token (id de usuario, email, rol, fecha de expiración…). **No es secreto: cualquiera puede decodificarlo**.
- **Signature:** firma calculada con un secreto que solo conoce el servidor. Permite verificar que el token no fue alterado.

**Idea clave:** el JWT no es "encriptación", es "firma". Cualquiera puede leer el contenido, pero **nadie puede falsificar uno válido sin el secreto**.

Flujo típico:

```
1. Cliente:    POST /login  { email, password }
2. Servidor:   verifica con bcrypt → genera JWT firmado → lo devuelve
3. Cliente:    guarda el token y lo envía en cada request
                Authorization: Bearer <token>
4. Servidor:   verifica firma + expiración → procesa o rechaza
```

### 1.3. ¿Por qué hashear contraseñas con bcrypt?

**Nunca** se guardan contraseñas en texto plano. Si tu base de datos se filtra, todas las cuentas quedan comprometidas, y como muchas personas reutilizan contraseñas, también quedan comprometidas sus otras cuentas.

La solución es guardar un **hash** de la contraseña: una transformación irreversible que produce una cadena fija. Cuando el usuario hace login, se hashea la contraseña ingresada y se compara con el hash guardado.

`bcrypt` es el estándar recomendado porque:

- es **lento por diseño** (resistente a ataques de fuerza bruta con GPU);
- incluye un **salt** automático en cada hash, evitando ataques con tablas precalculadas (rainbow tables);
- expone un parámetro `saltRounds` (factor de costo) que se puede subir a medida que el hardware mejora.

> Comparación rápida: `md5` o `sha256` **no son adecuados** para contraseñas; son demasiado rápidos. Usa siempre `bcrypt`, `argon2` o `scrypt`.

**Checkpoint 1 aprobado** cuando puedes explicar con tus palabras (a tu pareja o al docente) qué hace bcrypt, qué contiene un JWT y por qué autenticación ≠ autorización.

---

## Bloque 2 – Preparar el proyecto (15 minutos)

### Paso 1. Crear la carpeta del proyecto

En la terminal:

```bash
mkdir api-auth
cd api-auth
npm init -y
```

### Paso 2. Instalar dependencias

```bash
npm install express bcrypt jsonwebtoken dotenv helmet
npm install -D nodemon
```

¿Para qué sirve cada una?

| Paquete | Para qué se usa |
|---|---|
| `express` | servidor HTTP y enrutamiento |
| `bcrypt` | hashear y comparar contraseñas |
| `jsonwebtoken` | firmar y verificar JWT |
| `dotenv` | cargar variables de entorno desde `.env` |
| `helmet` | headers de seguridad HTTP por defecto |
| `nodemon` (dev) | reiniciar el servidor automáticamente al cambiar archivos |

### Paso 3. Crear la estructura

```
api-auth/
├── src/
│   ├── index.js
│   ├── routes/
│   │   ├── auth.routes.js
│   │   └── perfil.routes.js
│   ├── middleware/
│   │   └── authMiddleware.js
│   ├── services/
│   │   └── usuarios.service.js
│   └── data/
│       └── usuarios.json
├── .env
├── .gitignore
└── package.json
```

Crea las carpetas y archivos vacíos. En `src/data/usuarios.json` deja un arreglo vacío:

```json
[]
```

### Paso 4. Configurar `.env` y `.gitignore`

En `.env` (este archivo **NO se sube al repositorio**):

```env
PORT=3000
JWT_SECRET=cambia-esto-por-un-string-largo-y-aleatorio
JWT_EXPIRES_IN=1h
BCRYPT_SALT_ROUNDS=10
```

En `.gitignore`:

```
node_modules
.env
```

### Paso 5. Configurar scripts en `package.json`

Reemplaza la sección `scripts` por:

```json
"scripts": {
  "dev": "nodemon src/index.js",
  "start": "node src/index.js"
}
```

### Verificación del bloque

Comprueba que:

- existe `node_modules/` (se generó al instalar);
- existe `.env` con `JWT_SECRET` definido;
- existe `.gitignore` y **excluye `.env`**;
- la estructura de carpetas y archivos vacíos está completa.

**Checkpoint 2 aprobado** cuando la estructura del proyecto está lista y `.env` no se incluiría en un commit.

---

## Bloque 3 – Registro con bcrypt y login con JWT (30 minutos)

### Paso 6. Capa de servicio (lectura/escritura del JSON)

En `src/services/usuarios.service.js`:

```js
const fs = require("fs");
const path = require("path");

const DATA_PATH = path.join(__dirname, "..", "data", "usuarios.json");

function readUsuarios() {
  try {
    return JSON.parse(fs.readFileSync(DATA_PATH, "utf-8"));
  } catch {
    return [];
  }
}

function writeUsuarios(usuarios) {
  fs.writeFileSync(DATA_PATH, JSON.stringify(usuarios, null, 2), "utf-8");
}

function findByEmail(email) {
  return readUsuarios().find(u => u.email === email) || null;
}

function addUsuario(usuario) {
  const usuarios = readUsuarios();
  if (usuarios.some(u => u.email === usuario.email)) return false;
  usuarios.push(usuario);
  writeUsuarios(usuarios);
  return true;
}

module.exports = { readUsuarios, findByEmail, addUsuario };
```

### Paso 7. Rutas de autenticación

En `src/routes/auth.routes.js`:

```js
const express = require("express");
const bcrypt = require("bcrypt");
const jwt = require("jsonwebtoken");
const { findByEmail, addUsuario } = require("../services/usuarios.service");

const router = express.Router();

const SALT_ROUNDS = parseInt(process.env.BCRYPT_SALT_ROUNDS || "10", 10);

// POST /api/auth/register
router.post("/register", async (req, res) => {
  try {
    const { email, password, nombre } = req.body;

    if (!email || !password || !nombre) {
      return res.status(400).json({ error: "Faltan campos: email, password, nombre" });
    }
    if (typeof password !== "string" || password.length < 8) {
      return res.status(400).json({ error: "La contraseña debe tener al menos 8 caracteres" });
    }

    const existente = findByEmail(email);
    if (existente) {
      // Mensaje genérico para no filtrar si el email existe
      return res.status(409).json({ error: "No se pudo registrar el usuario" });
    }

    const hash = await bcrypt.hash(password, SALT_ROUNDS);

    const nuevo = {
      id: Date.now(),
      email: email.toLowerCase().trim(),
      nombre: nombre.trim(),
      passwordHash: hash,
      rol: "user",
      createdAt: new Date().toISOString()
    };

    addUsuario(nuevo);

    // Nunca devolver el hash al cliente
    return res.status(201).json({
      id: nuevo.id,
      email: nuevo.email,
      nombre: nuevo.nombre,
      rol: nuevo.rol
    });
  } catch (err) {
    console.error("[register]", err);
    return res.status(500).json({ error: "Error interno" });
  }
});

// POST /api/auth/login
router.post("/login", async (req, res) => {
  try {
    const { email, password } = req.body;

    if (!email || !password) {
      return res.status(400).json({ error: "Faltan credenciales" });
    }

    const usuario = findByEmail(email.toLowerCase().trim());

    // Mensaje genérico: no decir "usuario no existe" vs "contraseña incorrecta"
    const credencialesInvalidas = { error: "Credenciales inválidas" };

    if (!usuario) {
      return res.status(401).json(credencialesInvalidas);
    }

    const ok = await bcrypt.compare(password, usuario.passwordHash);
    if (!ok) {
      return res.status(401).json(credencialesInvalidas);
    }

    const token = jwt.sign(
      { sub: usuario.id, email: usuario.email, rol: usuario.rol },
      process.env.JWT_SECRET,
      { expiresIn: process.env.JWT_EXPIRES_IN || "1h" }
    );

    return res.status(200).json({
      token,
      usuario: { id: usuario.id, email: usuario.email, nombre: usuario.nombre, rol: usuario.rol }
    });
  } catch (err) {
    console.error("[login]", err);
    return res.status(500).json({ error: "Error interno" });
  }
});

module.exports = router;
```

### Paso 8. Servidor mínimo

En `src/index.js`:

```js
require("dotenv").config();
const express = require("express");
const helmet = require("helmet");

const authRoutes = require("./routes/auth.routes");

const app = express();
const PORT = process.env.PORT || 3000;

app.use(helmet());
app.use(express.json({ limit: "10kb" }));

app.get("/health", (req, res) => {
  res.status(200).json({ status: "ok" });
});

app.use("/api/auth", authRoutes);

app.use((req, res) => {
  res.status(404).json({ error: "Ruta no encontrada" });
});

app.listen(PORT, () => {
  console.log(`API corriendo en http://localhost:${PORT}`);
});
```

### Paso 9. Levantar el servidor

```bash
npm run dev
```

Deberías ver: `API corriendo en http://localhost:3000`.

### Paso 10. Probar registro

Con tu herramienta favorita (Postman, Thunder Client o `curl`):

**curl:**

```bash
curl -X POST http://localhost:3000/api/auth/register ^
  -H "Content-Type: application/json" ^
  -d "{\"email\":\"ana@test.cl\",\"password\":\"SuperPass123\",\"nombre\":\"Ana\"}"
```

> En PowerShell o bash usa `\` en vez de `^` para los saltos de línea, o pon todo en una sola línea.

**Postman / Thunder Client:**

- Método: `POST`
- URL: `http://localhost:3000/api/auth/register`
- Body → JSON:

```json
{
  "email": "ana@test.cl",
  "password": "SuperPass123",
  "nombre": "Ana"
}
```

### Resultado esperado

Código `201` y un JSON sin la contraseña ni el hash:

```json
{
  "id": 1714000000000,
  "email": "ana@test.cl",
  "nombre": "Ana",
  "rol": "user"
}
```

### Paso 11. Probar login

```json
POST http://localhost:3000/api/auth/login
{
  "email": "ana@test.cl",
  "password": "SuperPass123"
}
```

### Resultado esperado

Código `200` con un token JWT:

```json
{
  "token": "eyJhbGciOi...firma",
  "usuario": { "id": 1714000000000, "email": "ana@test.cl", "nombre": "Ana", "rol": "user" }
}
```

> Copia el token completo: lo necesitarás en el siguiente bloque.

### Paso 12. Inspeccionar el JWT

Abre [https://jwt.io](https://jwt.io) y pega tu token. Observa:

- el **header** indica `alg: HS256`;
- el **payload** muestra `sub`, `email`, `rol`, `iat`, `exp` en texto plano;
- la **signature** solo se verifica si pones tu `JWT_SECRET`.

> **Conclusión importante:** el JWT **no es secreto**. Cualquiera con el token puede leer su contenido. Por eso **no metas datos sensibles en el payload** (contraseñas, RUT, números de tarjeta, etc.).

### Verificación del bloque

Abre `src/data/usuarios.json` y confirma que la contraseña **NO** está en texto plano. Debes ver algo similar a:

```json
"passwordHash": "$2b$10$N9qo8uLOickgx2ZMRZoMyeIjZAgcfl7p92ldGxad68LJZdL17lhWy"
```

**Checkpoint 3 aprobado** cuando:

- el archivo `usuarios.json` contiene al menos un usuario con `passwordHash` (no plano);
- `/api/auth/login` devuelve un token JWT válido;
- una credencial incorrecta devuelve `401` con mensaje genérico.

---

## Bloque 4 – Middleware y ruta protegida (25 minutos)

### Paso 13. Crear el middleware de autenticación

En `src/middleware/authMiddleware.js`:

```js
const jwt = require("jsonwebtoken");

function authMiddleware(req, res, next) {
  const header = req.headers["authorization"] || "";
  const [tipo, token] = header.split(" ");

  if (tipo !== "Bearer" || !token) {
    return res.status(401).json({ error: "Token no provisto" });
  }

  try {
    const payload = jwt.verify(token, process.env.JWT_SECRET);
    // Adjuntamos el usuario al request para que las rutas lo usen
    req.usuario = { id: payload.sub, email: payload.email, rol: payload.rol };
    return next();
  } catch (err) {
    if (err.name === "TokenExpiredError") {
      return res.status(401).json({ error: "Token expirado" });
    }
    return res.status(401).json({ error: "Token inválido" });
  }
}

// Ejemplo de middleware de autorización por rol
function requireRol(...rolesPermitidos) {
  return (req, res, next) => {
    if (!req.usuario) return res.status(401).json({ error: "No autenticado" });
    if (!rolesPermitidos.includes(req.usuario.rol)) {
      return res.status(403).json({ error: "No autorizado" });
    }
    return next();
  };
}

module.exports = { authMiddleware, requireRol };
```

### Paso 14. Crear una ruta protegida

En `src/routes/perfil.routes.js`:

```js
const express = require("express");
const { authMiddleware, requireRol } = require("../middleware/authMiddleware");

const router = express.Router();

// Cualquier usuario autenticado
router.get("/", authMiddleware, (req, res) => {
  res.status(200).json({
    mensaje: "Estás autenticado",
    usuario: req.usuario
  });
});

// Solo admins
router.get("/admin", authMiddleware, requireRol("admin"), (req, res) => {
  res.status(200).json({ mensaje: "Bienvenido, admin", usuario: req.usuario });
});

module.exports = router;
```

### Paso 15. Registrar la ruta en `index.js`

Agrega en `src/index.js`, después de `app.use("/api/auth", authRoutes);`:

```js
const perfilRoutes = require("./routes/perfil.routes");
app.use("/api/perfil", perfilRoutes);
```

### Paso 16. Probar la ruta protegida

**Sin token (debe fallar con 401):**

```bash
curl http://localhost:3000/api/perfil
```

**Con token (debe responder 200):**

```bash
curl http://localhost:3000/api/perfil ^
  -H "Authorization: Bearer TU_TOKEN_AQUI"
```

En Postman / Thunder Client, ve a la pestaña `Headers` y agrega:

```
Authorization: Bearer eyJhbGciOiJIUzI1NiIsInR...
```

### Paso 17. Probar la ruta de admin

Con tu token actual (que tiene rol `user`), llama:

```
GET http://localhost:3000/api/perfil/admin
```

Debe responder `403 No autorizado`. Eso demuestra que **autenticación ≠ autorización**: el token es válido, pero el rol no alcanza.

### Tabla de verificación mínima

| Prueba | Acción | Resultado esperado |
|---|---|---|
| Salud API | GET `/health` | 200 |
| Registro válido | POST `/api/auth/register` | 201 sin password en respuesta |
| Registro duplicado | POST mismo email | 409 |
| Password débil | POST con password de 4 chars | 400 |
| Login correcto | POST `/api/auth/login` | 200 + token |
| Login mal | POST con password incorrecta | 401 mensaje genérico |
| Perfil sin token | GET `/api/perfil` | 401 |
| Perfil con token válido | GET `/api/perfil` con Bearer | 200 |
| Perfil con token alterado | GET con token modificado | 401 token inválido |
| Admin con rol `user` | GET `/api/perfil/admin` | 403 |

**Checkpoint 4 aprobado** cuando demuestras al menos 8 de las 10 pruebas anteriores.

---

## Bloque 5 – Endurecimiento de seguridad (20 minutos)

Ya tienes algo funcional. Ahora viene la parte que **separa un proyecto de estudiante de un proyecto profesional**: pulir los detalles de seguridad.

### Paso 18. Verificar headers de seguridad

Con la API corriendo, ejecuta:

```bash
curl -i http://localhost:3000/health
```

Gracias a `helmet`, deberías ver headers como:

```
X-Content-Type-Options: nosniff
X-Frame-Options: SAMEORIGIN
Strict-Transport-Security: max-age=15552000; includeSubDomains
Content-Security-Policy: default-src 'self'; ...
X-DNS-Prefetch-Control: off
```

¿Qué hace cada uno?

| Header | Para qué sirve |
|---|---|
| `X-Content-Type-Options: nosniff` | impide que el navegador "adivine" tipos MIME |
| `X-Frame-Options` | evita que tu sitio se cargue dentro de un `<iframe>` malicioso (clickjacking) |
| `Strict-Transport-Security` | obliga HTTPS en futuras visitas |
| `Content-Security-Policy` | limita de dónde se pueden cargar scripts/estilos |

Si por alguna razón no quieres usar `helmet`, puedes setearlos a mano con `app.use((req,res,next)=>{ res.setHeader(...); next(); })`, pero **`helmet` es la opción recomendada**.

### Paso 19. Revisar tu código contra esta checklist de seguridad

Marca cada ítem cuando lo hayas verificado en tu código:

- [ ] Las contraseñas se guardan **hasheadas con bcrypt** (no en texto plano, no con MD5/SHA1).
- [ ] El endpoint de login responde con un **mensaje genérico** ("Credenciales inválidas") sin revelar si el email existe.
- [ ] La respuesta del registro **no incluye** el hash ni la contraseña.
- [ ] El **`JWT_SECRET`** vive en `.env` y `.env` está en `.gitignore`.
- [ ] El JWT tiene **expiración** (`expiresIn`).
- [ ] El middleware **rechaza** tokens ausentes, inválidos y expirados.
- [ ] Hay **validación de entrada** mínima (campos presentes, tipos correctos, largo mínimo de password).
- [ ] El cuerpo JSON tiene un **límite razonable** (`express.json({ limit: "10kb" })`) para evitar abuso.
- [ ] Los errores `500` **no exponen** stack traces ni detalles internos al cliente.
- [ ] `helmet` está activo y devuelve headers de seguridad.

### Paso 20. Ejercicio anti-patrones (diagnóstico)

Para cada bloque de código siguiente, **identifica el problema** y propón cómo arreglarlo. Anota tus respuestas en tu bitácora.

**A.**

```js
const usuarioNuevo = { email, password };
addUsuario(usuarioNuevo);
```

**B.**

```js
if (!usuario) return res.status(404).json({ error: "El email no está registrado" });
const ok = await bcrypt.compare(password, usuario.passwordHash);
if (!ok) return res.status(401).json({ error: "La contraseña es incorrecta" });
```

**C.**

```js
const token = jwt.sign({ id: u.id, password: u.passwordHash }, "1234");
```

**D.**

```js
catch (err) {
  res.status(500).json({ error: err.stack });
}
```

**E.**

```js
// .env commiteado al repo
JWT_SECRET=12345
```

> Discusión sugerida (5 min con tu pareja o el grupo): ¿qué tienen en común estos errores? ¿En qué punto del flujo (almacenamiento, transmisión, manejo de errores, secretos) ocurre cada uno?

**Respuestas resumidas (consúltalas solo después de intentarlo):**

A. Guarda la contraseña en texto plano. *Fix:* hashear con `bcrypt.hash(password, SALT_ROUNDS)`.
B. Filtra si el email está o no registrado (oracle de enumeración). *Fix:* mensaje genérico `"Credenciales inválidas"` en ambos casos.
C. Mete el hash en el payload del JWT (que es legible) y usa un secreto débil hardcodeado. *Fix:* claims mínimos (`sub`, `email`, `rol`) y secreto largo desde `.env`.
D. Devuelve el stack trace al cliente, dándole pistas a un atacante. *Fix:* loggear en servidor, devolver mensaje genérico.
E. Secretos en el repositorio. *Fix:* `.env` en `.gitignore`, secreto largo y aleatorio, rotar si se filtró.

**Checkpoint 5 aprobado** cuando completaste la checklist y puedes explicar al menos 3 de los 5 anti-patrones.

---

## Bloque 6 – Pruebas, reflexión y cierre (15 minutos)

### Paso 21. Plan de pruebas registrado

Registra tus pruebas en una tabla así:

| ID | Acción | Datos | Esperado | Obtenido | Estado |
|---|---|---|---|---|---|
| P1 | POST `/api/auth/register` válido | email + password ≥ 8 | 201 sin hash en respuesta | | |
| P2 | POST `/api/auth/register` mismo email | duplicado | 409 | | |
| P3 | POST `/api/auth/register` password corta | "abc" | 400 | | |
| P4 | POST `/api/auth/login` correcto | credenciales válidas | 200 + token | | |
| P5 | POST `/api/auth/login` password mala | password incorrecta | 401 mensaje genérico | | |
| P6 | GET `/api/perfil` sin Authorization | sin header | 401 | | |
| P7 | GET `/api/perfil` token válido | Bearer válido | 200 | | |
| P8 | GET `/api/perfil` token alterado | una letra cambiada | 401 | | |
| P9 | GET `/api/perfil/admin` con rol user | token rol user | 403 | | |
| P10 | Headers de seguridad | curl -i /health | helmet headers presentes | | |

### Paso 22. Preguntas de reflexión (responder en `REFLEXION.md` o al final de tu bitácora)

1. ¿Cuál es la diferencia práctica entre **autenticación** y **autorización** en tu propio proyecto del portafolio? Da un ejemplo concreto de cada una.
2. Si alguien obtiene acceso al archivo `usuarios.json`, ¿puede usar las contraseñas de los usuarios? ¿Por qué sí o por qué no?
3. ¿Qué pasaría si tu `JWT_SECRET` fuera la cadena `"1234"`? Describe un ataque posible.
4. ¿Por qué el JWT incluye `expiresIn`? ¿Qué problema tendría un token sin expiración?
5. ¿Por qué el endpoint de login devuelve "Credenciales inválidas" en vez de "El usuario no existe"?
6. Identifica **un riesgo de seguridad** que aún tendría tu API si la pusieras en producción tal cual. (Pista: rate limiting, HTTPS, refresh tokens, almacenamiento del token en el frontend, CORS, etc.)
7. ¿Cómo aplicarías lo que aprendiste hoy a tu proyecto del portafolio? Nombra al menos dos rutas que deberían estar protegidas.

### Paso 23. Cierre

Escribe un párrafo breve respondiendo:

- ¿Qué fue lo más difícil de esta actividad?
- ¿Qué error de seguridad nunca volverías a cometer después de este laboratorio?

**Checkpoint 6 aprobado** cuando entregas la tabla de pruebas y las respuestas a las preguntas de reflexión.

---

## 7. Entregables

Cada estudiante o equipo debe entregar:

### Carpeta de trabajo

- `api-auth/` (sin `node_modules` ni `.env`).

### Evidencias mínimas

- captura de `node -v` y `npm -v`;
- captura del registro exitoso (`201`) y de la respuesta **sin password**;
- captura del login con token JWT;
- captura de `usuarios.json` mostrando el `passwordHash`;
- captura de `/api/perfil` con token (`200`) y sin token (`401`);
- captura de `/api/perfil/admin` con rol `user` (`403`);
- captura de `curl -i /health` mostrando los headers de helmet;
- tabla del plan de pruebas completada;
- archivo `REFLEXION.md` con las respuestas;
- `.gitignore` que excluya `.env` y `node_modules`.

---

## 8. Criterios de logro

Se espera que el estudiante:

- distinga claramente autenticación de autorización con ejemplos propios;
- implemente registro y login con bcrypt + JWT funcionando;
- proteja al menos una ruta con un middleware reutilizable;
- aplique al menos tres prácticas de endurecimiento (helmet, mensajes genéricos, secretos fuera del repo);
- documente pruebas y reflexione sobre riesgos residuales.

---

## 9. Desafío opcional para quienes terminen antes

Si terminas antes del tiempo, elige al menos una mejora.

### Opción A: Refresh tokens

- Emite un `refreshToken` con expiración larga (`7d`) además del `accessToken` corto (`15m`).
- Crea un endpoint `POST /api/auth/refresh` que reciba el refresh token y devuelva un nuevo access token.
- Discute: ¿dónde se guardan idealmente cada uno en el cliente?

### Opción B: Rate limiting en login

Instala `express-rate-limit` y aplícalo solo a `/api/auth/login`:

```bash
npm install express-rate-limit
```

```js
const rateLimit = require("express-rate-limit");
const loginLimiter = rateLimit({
  windowMs: 15 * 60 * 1000, // 15 minutos
  max: 5,
  message: { error: "Demasiados intentos, intenta más tarde" }
});
app.use("/api/auth/login", loginLimiter);
```

Demuéstralo: haz 6 intentos fallidos seguidos. El sexto debe responder con `429`.

### Opción C: Conectar con Supabase / PostgreSQL

Reemplaza `usuarios.json` por una tabla `usuarios` en Supabase con las columnas `id`, `email` (único), `nombre`, `password_hash`, `rol`, `created_at`. Ajusta `usuarios.service.js` para usar el cliente de Supabase. **Importante:** sigue hasheando con bcrypt en el backend antes de insertar.

### Opción D: Frontend mínimo en React

- Crea un formulario de login en React.
- Guarda el token recibido en memoria (no en `localStorage` si te preocupa XSS; discútelo con tu pareja).
- Llama a `/api/perfil` enviando el header `Authorization: Bearer <token>`.

---

## 10. Reglas mínimas de la actividad

### Reglas del backend

- Debe responder en `http://localhost:3000`
- Debe incluir `/health`
- Debe registrar usuarios con contraseña **hasheada**
- Debe emitir un JWT firmado con un secreto desde `.env`
- Debe proteger `/api/perfil` con un middleware
- Debe diferenciar 401 (no autenticado) de 403 (no autorizado)
- Debe activar `helmet`
- **No** debe exponer stack traces, hashes ni el `JWT_SECRET`
- **No** debe commitear `.env`

### Reglas de seguridad transversales

- `JWT_SECRET` largo y aleatorio (≥ 32 caracteres recomendado)
- Mensajes de error genéricos en autenticación
- Validación de entrada en todos los endpoints públicos
- Tokens con expiración

---

## 11. Cierre de la actividad

Al terminar, redacta una conclusión breve respondiendo:

1. ¿Qué parte de la actividad resultó más simple?
2. ¿Qué error o dificultad apareció durante el desarrollo?
3. ¿Qué decisión de seguridad te pareció más importante y por qué?
4. ¿Cómo vas a aplicar JWT y bcrypt en tu proyecto del portafolio? ¿Qué rutas protegerás primero?

---

## 12. Recomendaciones para trabajar bien en clase

- avanza por bloques y valida cada checkpoint antes de continuar;
- nunca pegues tu `JWT_SECRET` en una captura de pantalla;
- si algo falla, revisa primero: nombre de la cabecera (`Authorization`), prefijo (`Bearer `), y que el token no esté cortado;
- usa Postman/Thunder Client con una colección guardada para reutilizar tu token entre pruebas;
- cuando dudes si una ruta debe ser pública o protegida, asume **protegida** por defecto.

---

## 13. Glosario rápido

| Término | Definición corta |
|---|---|
| **Autenticación (AuthN)** | Verificar quién es el usuario. |
| **Autorización (AuthZ)** | Verificar qué puede hacer el usuario autenticado. |
| **Hash** | Transformación irreversible de un dato; se compara contra el original. |
| **Salt** | Valor aleatorio que se mezcla con la contraseña antes de hashear. |
| **bcrypt** | Algoritmo de hashing de contraseñas con salt y costo configurable. |
| **JWT** | Token JSON firmado, formato `header.payload.signature`. |
| **Bearer token** | Convención: `Authorization: Bearer <token>`. |
| **Claim** | Cada campo del payload del JWT (`sub`, `exp`, `rol`…). |
| **Middleware** | Función Express con firma `(req, res, next)` que se ejecuta antes del handler. |
| **CSRF / XSS** | Dos clases clásicas de ataque web a estudiar más adelante. |

---

## 14. Resumen rápido para estudiantes

Debes construir una API en Node.js + Express con tres rutas (`register`, `login`, `perfil`), almacenando contraseñas con **bcrypt**, emitiendo **JWT** firmados con un secreto desde `.env`, y protegiendo el acceso con un **middleware** reutilizable. Después debes endurecerla con `helmet`, mensajes de error genéricos y validación, probarla con Postman / Thunder Client / `curl`, y reflexionar sobre los riesgos que aún quedan abiertos.
