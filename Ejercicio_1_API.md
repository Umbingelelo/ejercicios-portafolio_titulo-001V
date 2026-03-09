# Actividad de Laboratorio: API REST de Donantes + Cliente Python

## Datos generales

**Asignatura:** TPY1101  
**Duración estimada:** 1 hora 30 minutos  
**Modalidad:** Individual o en parejas  
**Sistema operativo considerado:** Windows  
**Herramientas principales:** Node.js, Express, Python, PowerShell, VS Code

---

## 1. Propósito de la actividad

En esta actividad desarrollarás una **API REST local** para gestionar donantes y posteriormente crearás un **programa en Python** que consuma dicha API para consultar, registrar y eliminar datos.

Esta experiencia está pensada para que trabajes de forma progresiva, con verificaciones pequeñas durante todo el proceso, de manera que puedas comprobar si avanzas correctamente antes de pasar a la siguiente etapa.

Al finalizar, deberías ser capaz de:

- levantar una API en Node.js con Express;
- probar rutas REST desde Windows;
- validar respuestas HTTP;
- almacenar información en un archivo JSON;
- consumir la API desde Python;
- registrar pruebas, resultados y mejoras.

---

## 2. Contexto del caso

Una pequeña fundación necesita una aplicación simple para administrar donantes. Por ahora no requiere una base de datos compleja, por lo que se ha decidido crear una **API local** que permita:

- listar donantes;
- buscar un donante por RUT;
- agregar un nuevo donante;
- eliminar un donante;
- mantener el archivo ordenado por RUT;
- evitar registros duplicados.

Luego, un segundo equipo necesita conectarse a esa API desde un programa en **Python** para automatizar consultas y registros. Tu trabajo será construir ambas partes y demostrar que funcionan correctamente.

---

## 3. Producto esperado

Al finalizar la actividad, cada estudiante o equipo debe contar con:

1. una carpeta de proyecto Node.js funcionando;
2. una API REST operativa en `http://localhost:3000`;
3. un archivo `donantes.json` actualizado;
4. un programa Python que consuma la API;
5. evidencias mínimas de pruebas realizadas;
6. una breve bitácora de hallazgos y correcciones.

---

## 4. Requisitos previos

Antes de comenzar, verifica que tienes instalado:

- **Node.js LTS**
- **Python 3**
- **VS Code**
- **PowerShell**

### Verificación inicial en Windows

Abre **PowerShell** y ejecuta:

```powershell
node -v
npm -v
py --version
```

### Resultado esperado

Debes ver una versión para cada herramienta.

### Si algo falla

- Si `node` no existe, instala Node.js LTS.
- Si `py` no existe, instala Python y marca la opción para agregarlo al sistema.

**Checkpoint 0 aprobado** cuando el estudiante puede mostrar en pantalla las tres versiones.

---

## 5. Organización del tiempo

Distribuye el trabajo de la siguiente forma:

- **Bloque 1 – Preparación del entorno:** 10 minutos
- **Bloque 2 – Construcción de la API base:** 20 minutos
- **Bloque 3 – Implementación y pruebas REST:** 20 minutos
- **Bloque 4 – Consumo desde Python:** 20 minutos
- **Bloque 5 – Validación, mejora y evidencias:** 20 minutos

**Tiempo total estimado:** 90 minutos

---

# 6. Desarrollo paso a paso

---

## Bloque 1 – Preparar el proyecto (10 minutos)

### Paso 1. Crear la carpeta del proyecto

En PowerShell ejecuta:

```powershell
mkdir api-donantes
cd api-donantes
npm init -y
npm install express
npm install -D nodemon
```

### Paso 2. Crear la estructura del proyecto

```powershell
mkdir src, src\routes, src\services, src\data
ni src\index.js
ni src\routes\donantes.routes.js
ni src\services\donantes.service.js
ni src\data\donantes.json
code .
```

### Paso 3. Agregar datos iniciales

En `src/data/donantes.json` pega el siguiente contenido:

```json
[
  { "rut": 15274, "nombre": "Fulana de Tal", "monto": 200 },
  { "rut": 15891, "nombre": "Jean Dupont", "monto": 150 },
  { "rut": 16443, "nombre": "Erika Mustermann", "monto": 400 },
  { "rut": 16504, "nombre": "Perico Los Palotes", "monto": 80 },
  { "rut": 17004, "nombre": "Jan Kowalski", "monto": 200 }
]
```

### Paso 4. Configurar scripts

En `package.json`, reemplaza la sección `scripts` por esta:

```json
"scripts": {
  "dev": "nodemon src/index.js",
  "start": "node src/index.js"
}
```

### Verificación del bloque

Comprueba que existan:

- la carpeta `src`;
- los archivos `.js`;
- el archivo `donantes.json` con los 5 registros.

**Checkpoint 1 aprobado** cuando la estructura de carpetas y archivos esté completa.

---

## Bloque 2 – Crear la API base (20 minutos)

### Paso 5. Crear el servidor principal

En `src/index.js` pega el siguiente código:

```js
const express = require("express");
const donantesRoutes = require("./routes/donantes.routes");

const app = express();
const PORT = 3000;

app.use(express.json());

app.get("/health", (req, res) => {
  res.status(200).json({
    status: "ok",
    message: "API operativa"
  });
});

app.use("/api/donantes", donantesRoutes);

app.use((req, res) => {
  res.status(404).json({ error: "Ruta no encontrada" });
});

app.listen(PORT, () => {
  console.log(`API corriendo en http://localhost:${PORT}`);
});
```

### Paso 6. Crear la capa de servicio

En `src/services/donantes.service.js` pega lo siguiente:

```js
const fs = require("fs");
const path = require("path");

const DATA_PATH = path.join(__dirname, "..", "data", "donantes.json");

function readDonantes() {
  try {
    const contenido = fs.readFileSync(DATA_PATH, "utf-8");
    return JSON.parse(contenido);
  } catch (error) {
    return [];
  }
}

function writeDonantes(donantes) {
  const ordenados = [...donantes].sort((a, b) => a.rut - b.rut);
  fs.writeFileSync(DATA_PATH, JSON.stringify(ordenados, null, 2), "utf-8");
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

  if (filtrados.length === donantes.length) {
    return false;
  }

  writeDonantes(filtrados);
  return true;
}

module.exports = {
  readDonantes,
  findByRut,
  addDonante,
  deleteByRut
};
```

### Paso 7. Crear las rutas REST

En `src/routes/donantes.routes.js` pega lo siguiente:

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
  res.status(200).json(readDonantes());
});

router.get("/:rut", (req, res) => {
  const donante = findByRut(req.params.rut);

  if (!donante) {
    return res.status(404).json({ error: "Donante no encontrado" });
  }

  res.status(200).json(donante);
});

router.post("/", (req, res) => {
  const { rut, nombre, monto } = req.body;

  if (!rut || !nombre || monto === undefined) {
    return res.status(400).json({
      error: "Faltan campos obligatorios: rut, nombre, monto"
    });
  }

  if (typeof rut !== "number" || typeof nombre !== "string" || typeof monto !== "number") {
    return res.status(400).json({
      error: "Tipos de datos inválidos"
    });
  }

  const nuevoDonante = {
    rut,
    nombre: nombre.trim(),
    monto
  };

  const creado = addDonante(nuevoDonante);

  if (!creado) {
    return res.status(409).json({ error: "RUT duplicado" });
  }

  res.status(201).json(nuevoDonante);
});

router.delete("/:rut", (req, res) => {
  const eliminado = deleteByRut(req.params.rut);

  if (!eliminado) {
    return res.status(404).json({ error: "Donante no encontrado" });
  }

  res.status(204).send();
});

module.exports = router;
```

### Paso 8. Levantar la API

```powershell
npm run dev
```

### Paso 9. Probar el estado de la API

En otra ventana de PowerShell, ejecuta:

```powershell
Invoke-RestMethod -Uri http://localhost:3000/health
```

### Resultado esperado

```json
{
  "status": "ok",
  "message": "API operativa"
}
```

**Checkpoint 2 aprobado** cuando `/health` responde correctamente.

> **Importante para Windows:** en PowerShell es preferible usar `Invoke-RestMethod` o `Invoke-WebRequest`. Si usas `curl`, en algunos equipos puede comportarse diferente porque PowerShell interpreta ese comando de forma especial.

---

## Bloque 3 – Implementar y probar las operaciones REST (20 minutos)

### Paso 10. Probar listado completo

```powershell
Invoke-RestMethod -Uri http://localhost:3000/api/donantes
```

### Paso 11. Probar búsqueda por RUT existente

```powershell
Invoke-RestMethod -Uri http://localhost:3000/api/donantes/15274
```

### Paso 12. Probar búsqueda por RUT inexistente

```powershell
try {
  Invoke-RestMethod -Uri http://localhost:3000/api/donantes/99999
} catch {
  $_.Exception.Response.StatusCode.value__
}
```

### Paso 13. Probar creación de un donante nuevo

```powershell
$body = @{
  rut = 19999
  nombre = "Cristian Calderón"
  monto = 500
} | ConvertTo-Json

Invoke-RestMethod -Uri http://localhost:3000/api/donantes `
  -Method Post `
  -Body $body `
  -ContentType "application/json"
```

### Paso 14. Probar duplicado

Vuelve a ejecutar el mismo `POST` anterior.

### Resultado esperado

La API debe responder con código `409` porque no puede existir el mismo RUT dos veces.

### Paso 15. Probar eliminación

```powershell
Invoke-WebRequest -Uri http://localhost:3000/api/donantes/19999 -Method Delete
```

### Tabla de verificación mínima

| Prueba | Ruta | Resultado esperado |
|---|---|---|
| Salud API | GET `/health` | 200 |
| Listar donantes | GET `/api/donantes` | 200 |
| Buscar existente | GET `/api/donantes/15274` | 200 |
| Buscar inexistente | GET `/api/donantes/99999` | 404 |
| Crear válido | POST `/api/donantes` | 201 |
| Crear duplicado | POST `/api/donantes` | 409 |
| Eliminar existente | DELETE `/api/donantes/19999` | 204 |
| Eliminar inexistente | DELETE `/api/donantes/99999` | 404 |

**Checkpoint 3 aprobado** cuando el estudiante demuestra al menos 6 de las 8 pruebas.

---

## Bloque 4 – Consumir la API desde Python (20 minutos)

### Paso 16. Crear la carpeta del cliente Python

Puedes crearla dentro del proyecto o al lado de la API:

```powershell
mkdir cliente-python
cd cliente-python
py -m pip install requests
ni cliente_donantes.py
```

### Paso 17. Crear el cliente Python

En `cliente_donantes.py` pega el siguiente código:

```python
import requests

BASE_URL = "http://localhost:3000/api/donantes"


def listar_donantes():
    respuesta = requests.get(BASE_URL, timeout=5)
    if respuesta.status_code == 200:
        donantes = respuesta.json()
        print("\nLISTA DE DONANTES")
        for d in donantes:
            print(f"RUT: {d['rut']} | Nombre: {d['nombre']} | Monto: {d['monto']}")
    else:
        print("Error al listar donantes:", respuesta.status_code)


def buscar_por_rut():
    rut = input("Ingrese RUT numérico: ").strip()
    respuesta = requests.get(f"{BASE_URL}/{rut}", timeout=5)

    if respuesta.status_code == 200:
        d = respuesta.json()
        print("\nDONANTE ENCONTRADO")
        print(f"RUT: {d['rut']}")
        print(f"Nombre: {d['nombre']}")
        print(f"Monto: {d['monto']}")
    elif respuesta.status_code == 404:
        print("Donante no encontrado.")
    else:
        print("Error:", respuesta.status_code)


def crear_donante():
    try:
        rut = int(input("Ingrese RUT numérico: ").strip())
        nombre = input("Ingrese nombre: ").strip()
        monto = int(input("Ingrese monto de donación: ").strip())
    except ValueError:
        print("Debe ingresar valores numéricos válidos para RUT y monto.")
        return

    payload = {
        "rut": rut,
        "nombre": nombre,
        "monto": monto
    }

    respuesta = requests.post(BASE_URL, json=payload, timeout=5)

    if respuesta.status_code == 201:
        print("Donante creado correctamente.")
    elif respuesta.status_code == 409:
        print("No se puede crear: RUT duplicado.")
    elif respuesta.status_code == 400:
        print("Solicitud inválida:", respuesta.json())
    else:
        print("Error:", respuesta.status_code)


def eliminar_donante():
    rut = input("Ingrese RUT a eliminar: ").strip()
    respuesta = requests.delete(f"{BASE_URL}/{rut}", timeout=5)

    if respuesta.status_code == 204:
        print("Donante eliminado correctamente.")
    elif respuesta.status_code == 404:
        print("No se encontró el donante.")
    else:
        print("Error:", respuesta.status_code)


def menu():
    while True:
        print("\n=== CLIENTE PYTHON DONANTES ===")
        print("1. Listar donantes")
        print("2. Buscar donante por RUT")
        print("3. Crear donante")
        print("4. Eliminar donante")
        print("5. Salir")

        opcion = input("Seleccione una opción: ").strip()

        if opcion == "1":
            listar_donantes()
        elif opcion == "2":
            buscar_por_rut()
        elif opcion == "3":
            crear_donante()
        elif opcion == "4":
            eliminar_donante()
        elif opcion == "5":
            print("Programa finalizado.")
            break
        else:
            print("Opción inválida.")


if __name__ == "__main__":
    menu()
```

### Paso 18. Ejecutar el cliente

Asegúrate de que la API siga corriendo y luego ejecuta:

```powershell
py cliente_donantes.py
```

### Acciones mínimas obligatorias en Python

Debes demostrar:

1. listar donantes;
2. buscar un RUT existente;
3. crear un nuevo donante;
4. intentar crear un duplicado;
5. eliminar el donante creado.

**Checkpoint 4 aprobado** cuando el programa Python interactúa correctamente con la API.

---

## Bloque 5 – Validación, mejora y evidencias (20 minutos)

En esta etapa no basta con que funcione. Debes probar, detectar errores o debilidades y aplicar al menos una mejora.

### Paso 19. Completar plan de pruebas

Registra las pruebas en una tabla como esta:

| ID | Módulo | Acción | Datos de prueba | Resultado esperado | Resultado obtenido | Estado |
|---|---|---|---|---|---|---|
| P1 | API | GET `/health` | Sin datos | 200 + JSON status ok | Correcto | Aprobada |
| P2 | API | GET donante existente | RUT 15274 | 200 + objeto | Correcto | Aprobada |
| P3 | API | GET donante inexistente | RUT 99999 | 404 | Correcto | Aprobada |
| P4 | API | POST válido | Nuevo RUT | 201 | Correcto | Aprobada |
| P5 | API | POST duplicado | Mismo RUT | 409 | Correcto | Aprobada |
| P6 | Python | Crear desde menú | Nuevo registro | Registro visible en API | Correcto | Aprobada |
| P7 | Python | Eliminar desde menú | RUT creado | 204 y desaparece | Correcto | Aprobada |

### Paso 20. Aplicar una mejora real

Cada equipo debe implementar al menos una mejora después de probar la solución.

#### Opciones de mejora sugeridas

- validar que el nombre no venga vacío;
- impedir montos negativos;
- mejorar los mensajes de error en Python;
- capturar errores cuando la API no esté encendida;
- agregar una opción de resumen del total donado.

### Ejemplo de mejora

En la ruta `POST`, agrega esta validación para impedir montos negativos:

```js
if (monto < 0) {
  return res.status(400).json({
    error: "El monto no puede ser negativo"
  });
}
```

Luego debes probar desde PowerShell o desde Python que esta nueva validación efectivamente funciona.

**Checkpoint 5 aprobado** cuando existe una mejora implementada y validada.

---

## 7. Entregables

Cada estudiante o equipo debe entregar:

### Carpeta de trabajo

- `api-donantes/`
- `cliente-python/`

### Evidencias mínimas

- captura de `node -v`, `npm -v`, `py --version`;
- captura de `/health`;
- captura de una prueba `GET`;
- captura de un `POST`;
- captura del cliente Python funcionando;
- tabla de plan de pruebas;
- breve apartado con la mejora implementada.

---

## 8. Criterios de logro

Se espera que el estudiante:

- configure un ambiente funcional;
- desarrolle una API operativa;
- aplique pruebas de validación;
- corrija al menos un hallazgo;
- documente evidencias del proceso.

---

## 9. Desafío opcional para quienes terminen antes

Si finalizas antes del tiempo, implementa una funcionalidad extra.

### Opción A: filtrar por monto mínimo

Crear soporte para:

```http
GET /api/donantes?minMonto=200
```

### Opción B: resumen desde Python

Agregar en el menú una opción que calcule:

- cantidad total de donantes;
- suma total de donaciones;
- promedio de donación.

### Opción C: nueva ruta de resumen

Crear una ruta:

```http
GET /api/resumen
```

que devuelva algo similar a:

```json
{
  "cantidadDonantes": 5,
  "montoTotal": 1030,
  "promedio": 206
}
```

---

## 10. Reglas mínimas de la actividad

### Reglas de la API

- Debe responder en `http://localhost:3000`
- Debe incluir `/health`
- Debe listar donantes
- Debe buscar por RUT
- Debe crear donantes
- Debe eliminar donantes
- No debe permitir RUT duplicados
- Debe mantener el JSON ordenado por RUT
- Debe responder con códigos HTTP correctos

### Reglas del cliente Python

- Debe listar donantes
- Debe buscar un donante por RUT
- Debe crear donantes
- Debe eliminar donantes
- Debe mostrar mensajes claros de éxito o error

---

## 11. Cierre de la actividad

Al terminar, redacta una conclusión breve respondiendo estas preguntas:

1. ¿Qué parte de la actividad resultó más simple?
2. ¿Qué error o dificultad apareció durante el desarrollo?
3. ¿Qué mejora aplicaste y por qué?
4. ¿Qué aprendiste sobre el trabajo entre una API y un cliente Python?

---

## 12. Recomendaciones para trabajar bien en clase

- avanza por bloques y no saltes pasos;
- valida cada checkpoint antes de continuar;
- si algo falla, revisa primero rutas, nombres de archivos y puertos;
- deja capturas a medida que avanzas;
- prueba siempre después de cada cambio importante.

---

## 13. Resumen rápido para estudiantes

Debes construir una API en Node.js que administre donantes, probarla desde PowerShell y luego crear un programa en Python que se conecte a esa API. Durante el trabajo debes demostrar pruebas, aplicar una mejora y dejar evidencia de lo realizado.
