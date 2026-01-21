# Actividad (COMPLETA): React + Vite — Formulario de Registro con **código completo** + **1 test unitario** + **1 test funcional**

> En este documento tienes **TODOS los archivos** necesarios (código completo) para:
> 1) crear el proyecto con React + Vite,  
> 2) implementar un formulario de registro funcional,  
> 3) separar la validación (para testearla),  
> 4) crear **1 prueba unitaria** y **1 prueba funcional** con Vitest + Testing Library.

---

## Objetivo
Construir un proyecto con **React + Vite** y desarrollar un **formulario de registro** que:
- Valide datos del usuario (nombre, email, password, confirmación).
- Muestre errores y un mensaje de éxito.
- Incluya:
  - **1 prueba unitaria** (lógica de validación sin UI)
  - **1 prueba funcional** (flujo completo de UI simulando al usuario)

---

## Duración sugerida
90–120 minutos.

---

## Requisitos
- Node.js LTS instalado.
- Un editor (VS Code recomendado).
- Terminal.

---

# Parte 1 — Conceptos (más profundo + ejemplos)

## 1) ¿Qué es Vite y por qué se usa?
**Vite** es un “bundler/dev server” moderno para proyectos frontend.
- En desarrollo, sirve archivos con un servidor rápido y recarga en caliente (**HMR**).
- En build, empaqueta el proyecto para producción.

**Ejemplo real:**  
Cuando guardas un archivo `.jsx`, Vite detecta el cambio y refresca la página sin recompilar todo el proyecto desde cero.

---

## 2) ¿Qué es React (en términos prácticos)?
React construye UI con **componentes**.

### Componente
Un componente es una función que retorna JSX:

```jsx
function Hola({ nombre }) {
  return <p>Hola {nombre}</p>
}
```

### Props
Son datos que “entran” al componente:

```jsx
<Hola nombre="Ana" />
```

### Estado (state)
Es data que vive en el componente y puede cambiar:

```jsx
const [contador, setContador] = useState(0)
```

Si el estado cambia, React re-renderiza el componente.

---

## 3) Formularios en React: “controlled components”
En React, la forma recomendada es usar **inputs controlados**:
- El valor del input vive en el **estado**.
- Cuando el usuario escribe, `onChange` actualiza el estado.

**Ejemplo mínimo:**
```jsx
const [email, setEmail] = useState("")

return (
  <input
    value={email}
    onChange={(e) => setEmail(e.target.value)}
  />
)
```

**¿Por qué esto es buena práctica?**
- Puedes validar en tiempo real.
- Puedes enviar todo el formulario con un solo objeto.
- Es más fácil testear porque el estado es predecible.

---

## 4) Validación: separar lógica de UI
Si mezclas validación dentro del componente, testear se vuelve más difícil.

**Mejor práctica:**
- Crear una función pura en `utils/validateRegister.js`
- El componente la usa, pero la función se puede testear sin React.

**Función pura =**
- No toca DOM
- No depende de React
- Misma entrada → misma salida

---

## 5) Testing: unitario vs funcional (con ejemplos)

### Test unitario (unit)
Valida una función o unidad pequeña.
- **No renderiza UI**
- **No usa navegador**
- Debe ser rápido

**Ejemplo:** `validateRegister({ email: "x" })` retorna error.

### Test funcional (integration/functional)
Simula el flujo real:
- Renderiza el componente
- Escribe en inputs
- Clic en botones
- Verifica lo que el usuario vería en pantalla

**Ejemplo:** usuario llena el formulario correctamente → aparece “Registro exitoso”.

---

## 6) Vitest + Testing Library (qué hace cada uno)
- **Vitest**: ejecuta los tests (similar a Jest) y provee `describe/it/expect`.
- **@testing-library/react**: renderiza componentes en un DOM simulado (JSDOM).
- **@testing-library/user-event**: simula acciones reales (teclear, click).
- **@testing-library/jest-dom**: agrega matchers útiles como `toBeInTheDocument()`.

**Filosofía Testing Library:**  
Testea como usuario, no como implementador.
- Prefiere `getByLabelText("Email")` a buscar clases CSS.

---

# Parte 2 — Paso a paso (con TODO el código)

## Paso A: Crear proyecto con Vite (React)
En terminal:

```bash
npm create vite@latest registro-react -- --template react
cd registro-react
npm install
npm run dev
```

Abre el link que te muestra (normalmente `http://localhost:5173`).

---

## Paso B: Estructura recomendada
Dentro de `src/` crea:

```
src/
  components/
  utils/
  __tests__/
```

---

# Parte 3 — Instalar y configurar testing (Vitest + RTL)

## Paso C: Instalar dependencias de testing
```bash
npm install -D vitest jsdom @testing-library/react @testing-library/user-event @testing-library/jest-dom
```

---

## Paso D: Configurar `vite.config.js` (archivo completo)
Crea/edita `vite.config.js` en la raíz del proyecto:

```js
import { defineConfig } from 'vite'
import react from '@vitejs/plugin-react'

export default defineConfig({
  plugins: [react()],
  test: {
    environment: 'jsdom',
    setupFiles: './src/setupTests.js',
  },
})
```

---

## Paso E: Crear `src/setupTests.js` (archivo completo)
```js
import '@testing-library/jest-dom'
```

---

## Paso F: Agregar scripts de test en `package.json`
En `"scripts"` agrega (o deja) algo así:

```json
{
  "scripts": {
    "dev": "vite",
    "build": "vite build",
    "preview": "vite preview",
    "test": "vitest",
    "test:run": "vitest run"
  }
}
```

---

# Parte 4 — Implementación del formulario (código completo)

## Paso G: Crear `src/utils/validateRegister.js` (archivo completo)
> Esta función retorna un objeto con errores. Si no hay errores, retorna `{}`.

```js
export function validateRegister(formData) {
  const errors = {}

  const name = (formData.name || "").trim()
  const email = (formData.email || "").trim()
  const password = formData.password || ""
  const confirmPassword = formData.confirmPassword || ""

  // Nombre
  if (!name) {
    errors.name = "El nombre es obligatorio"
  } else if (name.length < 2) {
    errors.name = "El nombre debe tener al menos 2 caracteres"
  }

  // Email (regex simple, suficiente para el curso)
  const emailRegex = /^[^\s@]+@[^\s@]+\.[^\s@]+$/
  if (!email) {
    errors.email = "El email es obligatorio"
  } else if (!emailRegex.test(email)) {
    errors.email = "Email inválido"
  }

  // Password
  if (!password) {
    errors.password = "La contraseña es obligatoria"
  } else if (password.length < 6) {
    errors.password = "La contraseña debe tener al menos 6 caracteres"
  }

  // Confirmación
  if (!confirmPassword) {
    errors.confirmPassword = "Debes confirmar la contraseña"
  } else if (password !== confirmPassword) {
    errors.confirmPassword = "Las contraseñas no coinciden"
  }

  return errors
}
```

---

## Paso H: Crear `src/components/RegisterForm.jsx` (archivo completo)
> Incluye HTML completo del formulario, errores visibles y mensaje de éxito.  
> Importante: se usan **labels** para que Testing Library encuentre inputs como lo haría un usuario.

```jsx
import { useMemo, useState } from "react"
import { validateRegister } from "../utils/validateRegister"

const initialForm = {
  name: "",
  email: "",
  password: "",
  confirmPassword: "",
}

export default function RegisterForm() {
  const [form, setForm] = useState(initialForm)
  const [errors, setErrors] = useState({})
  const [successMessage, setSuccessMessage] = useState("")

  // Para mostrar un resumen de errores (opcional)
  const errorCount = useMemo(() => Object.keys(errors).length, [errors])

  function handleChange(e) {
    const { name, value } = e.target
    setForm((prev) => ({ ...prev, [name]: value }))
  }

  function handleSubmit(e) {
    e.preventDefault()

    // Limpia mensaje de éxito anterior
    setSuccessMessage("")

    // Valida
    const validationErrors = validateRegister(form)
    setErrors(validationErrors)

    // Si hay errores, no “registra”
    if (Object.keys(validationErrors).length > 0) return

    // Simulación de éxito (sin backend)
    setSuccessMessage("Registro exitoso")
    setForm(initialForm)
  }

  return (
    <section style={{ maxWidth: 520, margin: "40px auto", padding: 16 }}>
      <h1 style={{ marginBottom: 8 }}>Registro</h1>
      <p style={{ marginTop: 0, marginBottom: 16 }}>
        Completa el formulario para crear una cuenta.
      </p>

      {/* Resumen de errores (opcional) */}
      {errorCount > 0 && (
        <div
          role="alert"
          aria-label="Resumen de errores"
          style={{
            border: "1px solid #ccc",
            padding: 12,
            marginBottom: 16,
          }}
        >
          <strong>Hay errores en el formulario.</strong>
          <ul style={{ marginTop: 8 }}>
            {Object.values(errors).map((msg, idx) => (
              <li key={idx}>{msg}</li>
            ))}
          </ul>
        </div>
      )}

      {/* Mensaje de éxito */}
      {successMessage && (
        <div
          role="status"
          style={{
            border: "1px solid #ccc",
            padding: 12,
            marginBottom: 16,
          }}
        >
          {successMessage}
        </div>
      )}

      <form onSubmit={handleSubmit} noValidate>
        {/* Nombre */}
        <div style={{ marginBottom: 12 }}>
          <label htmlFor="name" style={{ display: "block", marginBottom: 4 }}>
            Nombre
          </label>
          <input
            id="name"
            name="name"
            type="text"
            value={form.name}
            onChange={handleChange}
            placeholder="Ej: Juan Pérez"
            aria-describedby={errors.name ? "name-error" : undefined}
            style={{ width: "100%", padding: 10 }}
          />
          {errors.name && (
            <p id="name-error" role="alert" style={{ margin: "6px 0 0" }}>
              {errors.name}
            </p>
          )}
        </div>

        {/* Email */}
        <div style={{ marginBottom: 12 }}>
          <label htmlFor="email" style={{ display: "block", marginBottom: 4 }}>
            Email
          </label>
          <input
            id="email"
            name="email"
            type="email"
            value={form.email}
            onChange={handleChange}
            placeholder="Ej: correo@dominio.com"
            aria-describedby={errors.email ? "email-error" : undefined}
            style={{ width: "100%", padding: 10 }}
          />
          {errors.email && (
            <p id="email-error" role="alert" style={{ margin: "6px 0 0" }}>
              {errors.email}
            </p>
          )}
        </div>

        {/* Password */}
        <div style={{ marginBottom: 12 }}>
          <label
            htmlFor="password"
            style={{ display: "block", marginBottom: 4 }}
          >
            Contraseña
          </label>
          <input
            id="password"
            name="password"
            type="password"
            value={form.password}
            onChange={handleChange}
            placeholder="Mínimo 6 caracteres"
            aria-describedby={errors.password ? "password-error" : undefined}
            style={{ width: "100%", padding: 10 }}
          />
          {errors.password && (
            <p id="password-error" role="alert" style={{ margin: "6px 0 0" }}>
              {errors.password}
            </p>
          )}
        </div>

        {/* Confirmar Password */}
        <div style={{ marginBottom: 16 }}>
          <label
            htmlFor="confirmPassword"
            style={{ display: "block", marginBottom: 4 }}
          >
            Confirmar contraseña
          </label>
          <input
            id="confirmPassword"
            name="confirmPassword"
            type="password"
            value={form.confirmPassword}
            onChange={handleChange}
            placeholder="Repite la contraseña"
            aria-describedby={
              errors.confirmPassword ? "confirmPassword-error" : undefined
            }
            style={{ width: "100%", padding: 10 }}
          />
          {errors.confirmPassword && (
            <p
              id="confirmPassword-error"
              role="alert"
              style={{ margin: "6px 0 0" }}
            >
              {errors.confirmPassword}
            </p>
          )}
        </div>

        <button type="submit" style={{ padding: "10px 14px" }}>
          Registrarse
        </button>
      </form>
    </section>
  )
}
```

---

## Paso I: Editar `src/App.jsx` (archivo completo)
```jsx
import RegisterForm from "./components/RegisterForm"

export default function App() {
  return (
    <main>
      <RegisterForm />
    </main>
  )
}
```

---

## Paso J: Revisar `src/main.jsx` (archivo completo)
> Si tu template es React (JS), normalmente ya viene así. Confirma que esté correcto.

```jsx
import React from 'react'
import ReactDOM from 'react-dom/client'
import App from './App.jsx'

ReactDOM.createRoot(document.getElementById('root')).render(
  <React.StrictMode>
    <App />
  </React.StrictMode>,
)
```

---

# Parte 5 — Tests (código completo)

## Test 1 (OBLIGATORIO): Prueba unitaria de `validateRegister`
Crea: `src/__tests__/validateRegister.test.js`

```js
import { describe, it, expect } from "vitest"
import { validateRegister } from "../utils/validateRegister"

describe("validateRegister (unit test)", () => {
  it("debería devolver error si el email es inválido", () => {
    const errors = validateRegister({
      name: "Juan",
      email: "correo-invalido",
      password: "123456",
      confirmPassword: "123456",
    })

    expect(errors.email).toBe("Email inválido")
  })

  it("debería devolver error si las contraseñas no coinciden", () => {
    const errors = validateRegister({
      name: "Juan",
      email: "juan@test.com",
      password: "123456",
      confirmPassword: "xxxxxx",
    })

    expect(errors.confirmPassword).toBe("Las contraseñas no coinciden")
  })

  it("debería devolver {} si todo es válido", () => {
    const errors = validateRegister({
      name: "Juan",
      email: "juan@test.com",
      password: "123456",
      confirmPassword: "123456",
    })

    expect(errors).toEqual({})
  })
})
```

✅ Esto es unitario porque testea **solo** la función (sin UI).

---

## Test 2 (OBLIGATORIO): Prueba funcional del formulario (UI)
Crea: `src/__tests__/RegisterForm.test.jsx`

```jsx
import { describe, it, expect } from "vitest"
import { render, screen } from "@testing-library/react"
import userEvent from "@testing-library/user-event"
import RegisterForm from "../components/RegisterForm"

describe("RegisterForm (functional test)", () => {
  it("debería registrar exitosamente si el formulario es válido", async () => {
    const user = userEvent.setup()
    render(<RegisterForm />)

    // Inputs por label (como usuario)
    await user.type(screen.getByLabelText(/nombre/i), "Juan Pérez")
    await user.type(screen.getByLabelText(/email/i), "juan@test.com")
    await user.type(screen.getByLabelText(/^contraseña$/i), "123456")
    await user.type(screen.getByLabelText(/confirmar contraseña/i), "123456")

    // Submit
    await user.click(screen.getByRole("button", { name: /registrarse/i }))

    // Verifica mensaje de éxito visible en pantalla
    expect(screen.getByRole("status")).toHaveTextContent("Registro exitoso")
  })

  it("debería mostrar errores si se envía vacío (opcional recomendado)", async () => {
    const user = userEvent.setup()
    render(<RegisterForm />)

    await user.click(screen.getByRole("button", { name: /registrarse/i }))

    // Al menos uno de los errores debería aparecer
    expect(screen.getByText(/El nombre es obligatorio/i)).toBeInTheDocument()
    expect(screen.getByText(/El email es obligatorio/i)).toBeInTheDocument()
  })
})
```

✅ Esto es funcional/integración porque:
- Renderiza el componente
- Simula tecleo/click
- Verifica lo que el usuario ve

---

# Parte 6 — Cómo ejecutar todo

## Correr el proyecto
```bash
npm run dev
```

## Correr tests en modo watch
```bash
npm run test
```

## Correr tests una sola vez (para entregar)
```bash
npm run test:run
```

---

# Checklist final (obligatorio)
- [ ] Proyecto inicia y se ve el formulario en `http://localhost:5173`
- [ ] Formulario valida y muestra errores
- [ ] Formulario muestra “Registro exitoso” cuando todo es válido
- [ ] `npm run test:run` pasa con éxito (sin errores)
- [ ] Subiste el repo a GitHub

---

# Entregables
1. Link al repositorio GitHub.
2. Evidencia (captura o texto) del comando:
   - `npm run test:run` mostrando tests pasando.

---

# Rúbrica breve
| Criterio | Logrado | Parcial | No logrado |
|---|---|---|---|
| Formulario completo (campos + validaciones + mensajes) |  |  |  |
| Separación de validación en utils |  |  |  |
| 1 test unitario correcto y pasando |  |  |  |
| 1 test funcional correcto y pasando |  |  |  |
| Orden de proyecto (estructura, nombres) |  |  |  |

---

## Tips de depuración (si algo falla)
- Si Testing Library no encuentra un input, revisa que exista `<label htmlFor="...">` y que el input tenga el `id` correcto.
- Si `getByLabelText` falla, revisa mayúsculas/minúsculas y usa regex `/email/i`.
- Si falla el test por async, asegúrate de usar `await user.type(...)` y `await user.click(...)`.

¡Listo! Con esto el estudiante puede construir el proyecto completo y testearlo por su cuenta.
