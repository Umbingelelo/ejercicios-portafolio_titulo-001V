# Actividad: Iniciar proyecto Frontend con **React + Vite** y crear un **formulario de registro** (con 1 prueba funcional + 1 prueba unitaria)

## Objetivo
En esta actividad crearás tu primer proyecto con **React + Vite**, construirás un **formulario de registro** (Register) y escribirás:
- **1 prueba unitaria** (unit test) para validar lógica del formulario.
- **1 prueba funcional** (integration/functional test) para simular el flujo completo del usuario en la UI.

La meta es que aprendas el flujo completo: **crear proyecto → construir UI → validar datos → testear**.

---

## Duración sugerida
90–120 minutos.

---

## Requisitos
- Node.js LTS instalado (recomendado).
- Editor (VS Code recomendado).
- Terminal (PowerShell/Terminal/Bash).

---

# Parte 1 — Conceptos clave (explicados)

## 1) ¿Qué es Vite?
Vite es una herramienta moderna para crear proyectos frontend rápido.  
Ventajas:
- Levanta un servidor local muy rápido.
- Compila y recarga en caliente (HMR: Hot Module Reload).

## 2) ¿Qué es React?
React es una librería para construir interfaces con componentes.
- Un **componente** es una función que retorna UI.
- Usamos **estado (state)** para manejar valores que cambian (inputs, errores, etc.).
- Usamos **props** para pasar datos entre componentes.

## 3) ¿Qué es una prueba unitaria?
Prueba que valida una pieza pequeña de lógica (una función) **sin depender de la UI** ni del navegador.
Ejemplo: una función `isValidEmail(email)`.

## 4) ¿Qué es una prueba funcional?
Prueba que simula el uso del sistema como lo haría un usuario:
- Escribir en inputs
- Hacer click en “Registrar”
- Ver mensajes de éxito/error en pantalla

---

# Parte 2 — Crear el proyecto (paso a paso)

## Paso A: Crear proyecto con Vite
1. Abre una terminal en una carpeta donde guardarás tu proyecto.
2. Ejecuta:

```bash
npm create vite@latest registro-react -- --template react
```

3. Entra al proyecto:

```bash
cd registro-react
```

4. Instala dependencias:

```bash
npm install
```

5. Levanta el proyecto:

```bash
npm run dev
```

6. Abre el link que aparece (normalmente `http://localhost:5173`).

✅ Si ves la página inicial de Vite + React, vas bien.

---

# Parte 3 — Ordenar la estructura de carpetas (buena práctica)
Dentro de `src/` crea esta estructura:

```
src/
  components/
  pages/
  utils/
  __tests__/
```

> Nota: `__tests__` es un estándar común para guardar tests.

---

# Parte 4 — Construir el formulario de registro

## Paso B: Crear componente RegisterForm
Crea el archivo:

- `src/components/RegisterForm.jsx`

### ¿Qué debe tener el formulario?
Campos mínimos:
- **Nombre**
- **Email**
- **Password**
- **Confirmar Password**
- Botón: **Registrarse**

Validaciones mínimas:
- Nombre: no vacío (mín. 2 caracteres recomendado).
- Email: formato válido.
- Password: mín. 6 caracteres recomendado.
- Confirmación: debe coincidir con password.

Mensajes:
- Si hay errores, mostrarlos debajo del input o al final.
- Si todo está ok, mostrar “Registro exitoso”.

### Recomendación técnica
- Usa `useState` para manejar:
  - valores del formulario
  - errores
  - mensaje de éxito

---

## Paso C: Montar el formulario en App
Abre `src/App.jsx` y deja el formulario visible.

---

# Parte 5 — Lógica de validación (separar para testear)
Para que sea fácil testear, extrae la validación a una función en `utils`.

## Paso D: Crear función de validación
Crea el archivo:

- `src/utils/validateRegister.js`

Debe exportar una función:

- `validateRegister(formData)` → retorna un objeto de errores.

Ejemplo de retorno:
```js
{
  name: "El nombre es obligatorio",
  email: "Email inválido"
}
```

✅ Si el objeto está vacío, el formulario es válido.

### Buenas prácticas en validación
- Una función pura: mismos inputs → mismo output.
- No depender del DOM ni de React.
- Mensajes claros para el usuario.

---

# Parte 6 — Agregar testing al proyecto

## Paso E: Instalar Vitest + Testing Library
Vitest es el framework de tests recomendado para Vite.

Ejecuta:

```bash
npm install -D vitest jsdom @testing-library/react @testing-library/user-event @testing-library/jest-dom
```

---

## Paso F: Configurar Vitest en Vite
Abre `vite.config.js` (o `vite.config.ts` si corresponde) y agrega configuración de test.

Ejemplo (JS):

```js
import { defineConfig } from 'vite'
import react from '@vitejs/plugin-react'

export default defineConfig({
  plugins: [react()],
  test: {
    environment: 'jsdom',
    setupFiles: './src/setupTests.js'
  }
})
```

---

## Paso G: Setup de Jest DOM
Crea el archivo:

- `src/setupTests.js`

Y agrega:

```js
import '@testing-library/jest-dom'
```

---

## Paso H: Agregar script de tests
En `package.json`, en `"scripts"`, agrega:

```json
"test": "vitest",
"test:run": "vitest run"
```

> Con `npm run test` dejas Vitest en modo watch.
> Con `npm run test:run` corre una vez (ideal para entregar).

---

# Parte 7 — Prueba Unitaria (obligatoria)

## Objetivo de la prueba unitaria
Validar que `validateRegister` funcione.

## Paso I: Crear Unit Test
Crea:

- `src/__tests__/validateRegister.test.js`

Casos mínimos:
1. Email inválido genera error.
2. Password y confirmación distintas genera error.
3. Datos correctos retornan `{}` (sin errores).

✅ Esto es prueba unitaria porque testea lógica pura.

---

# Parte 8 — Prueba Funcional (obligatoria)

## Objetivo de la prueba funcional
Simular al usuario:
- Escribir datos en inputs
- Click en “Registrarse”
- Ver mensaje “Registro exitoso”

## Paso J: Crear Functional Test del formulario
Crea:

- `src/__tests__/RegisterForm.test.jsx`

Requisitos:
- Renderizar `RegisterForm`
- Usar `userEvent` para escribir en inputs
- Hacer click en el botón
- Verificar que aparezca el mensaje de éxito
- (Opcional recomendado) probar que aparezcan errores si dejas campos vacíos

✅ Esto es funcional/integración porque toca UI + interacciones.

---

# Parte 9 — Checklist final (lo que debes lograr)

## Checklist técnico
- [ ] Proyecto creado con React + Vite
- [ ] Formulario de registro funcionando con validaciones
- [ ] Función `validateRegister` separada en `utils/`
- [ ] Instalación y configuración de Vitest + Testing Library
- [ ] 1 test unitario (validación)
- [ ] 1 test funcional (flujo del formulario)
- [ ] Tests corren con `npm run test:run` sin errores

---

# Entregables
1. Link a tu repositorio GitHub.
2. Carpeta con el proyecto completo.
3. Evidencia (captura o texto) del comando:
   - `npm run test:run` mostrando tests pasando.

---

# Rúbrica breve (evaluación)
| Criterio | Logrado | Parcial | No logrado |
|---|---|---|---|
| Proyecto React + Vite inicia correctamente |  |  |  |
| Formulario registra y valida correctamente |  |  |  |
| Prueba unitaria implementada y pasando |  |  |  |
| Prueba funcional implementada y pasando |  |  |  |
| Estructura/orden del proyecto |  |  |  |

---

# Tips (para trabajar por tu cuenta)
- Si algo falla, revisa el error en consola: normalmente te dice qué falta importar o instalar.
- Mantén la validación separada (utils) para que el test unitario sea simple.
- En el test funcional, piensa como usuario: “escribo → clic → veo resultado”.

¡Listo! Cuando termines, asegúrate de que tus tests pasen antes de subir a GitHub.
