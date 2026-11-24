# nodejs-complete-guide

> RestApi feeds App — proyecto base para construir una API REST con Node.js y Express.

Repositorio de aprendizaje/trabajo que parte de un servidor Express mínimo y ya incluye toda la infraestructura de desarrollo moderna: transpilación con Babel, recarga automática, linting, formateo, hooks de Git, compilación y minificación.

---

## ✨ Características

- **Servidor HTTP con Express 5** — punto de entrada en `src/index.js`, listo para crear rutas y middleware.
- **Puerto configurable** — se lee de `process.env.PORT` con valor por defecto `3000`.
- **Sintaxis ES Modules** (`import`/`export`) transpilada a CommonJS mediante **Babel** (`@babel/preset-env`, target `node: current`).
- **Alias `@` → `./src`** configurado con `babel-plugin-module-resolver` para imports más limpios.
- **Variables de entorno** cargadas con el flag nativo `--env-file=.env` (sin dotenv).
- **Modo desarrollo** con `nodemon` + `babel-node`: recarga automática al guardar cambios.
- **Build de producción** a `dist/` con `@babel/cli`.
- **Build minificado** con `terser-folder` (`*.min.js`, eliminación de `console` y `debugger`).
- **Linting con ESLint 10** para JavaScript, JSON y CSS (plugins `@eslint/js`, `@eslint/json`, `@eslint/css`).
- **Configuración de formateo con Prettier** (comillas simples, punto y coma, ancho 80, `arrowParens: avoid`, trailing comma), con reglas en `.prettierrc` y exclusiones en `.prettierignore`. Ver la nota sobre su instalación más abajo.
- **Estilo de editor unificado con EditorConfig** (UTF-8, LF, 2 espacios, newline final).
- **Hooks de Git con Husky 9** + **lint-staged**: cada commit aplica `eslint --fix` y `prettier --write` a los archivos modificados.
- **Limpieza de artefactos** con `rimraf`.

---

## 📋 Requisitos

- **Node.js 20.6+** (necesario para el flag nativo `--env-file`; se recomienda una versión LTS reciente, compatible también con Babel 8).
- **npm** (incluido con Node.js).
- **Git** (para los hooks de Husky).

---

## 🚀 Instalación

```bash
git clone https://github.com/wjuma19dev2026-wq/project_node_blank.git
cd project_node_blank
npm install
```

> `npm install` ejecuta el script `prepare`, que instala los hooks de Husky automáticamente.

### Configuración del entorno

Copia el archivo de ejemplo y define el puerto deseado:

```bash
cp .env.example .env
```

La plantilla `.env.example` contiene:

```env
PORT=
```

Un `.env` de ejemplo funcional sería:

```env
PORT=4000
```

Si `PORT` no está definido o está vacío, el servidor usa `3000`. El archivo `.env` está ignorado por Git (solo `.env.example` se versiona).

---

## ▶️ Uso

| Comando             | Descripción                                                                 |
| ------------------- | --------------------------------------------------------------------------- |
| `npm start`         | Inicia el servidor con `babel-node src/index.js`.                           |
| `npm run dev`       | Desarrollo con recarga automática (`nodemon` + variables de `.env`).        |
| `npm run build`     | Compila `src/` a `dist/` con Babel.                                         |
| `npm run build:min` | Compila y genera además archivos `*.min.js` minificados con Terser.         |
| `npm run serve`     | Ejecuta la versión compilada (`node dist/index.js`).                        |
| `npm run clean`     | Elimina el directorio `dist/`.                                              |
| `npm run prepare`   | Instala los hooks de Husky (se ejecuta automáticamente tras `npm install`). |

### Ejemplo de arranque

```bash
npm run dev
# Servidor corriendo en el puerto 4000
```

Al arrancar verás en consola:

```text
Servidor corriendo en el puerto 4000
```

El servidor queda escuchando en `http://localhost:4000` (o en el puerto configurado). Por ahora no hay rutas registradas, así que cualquier petición devolverá `404`; el proyecto es la base sobre la que construir la API.

---

## 🧩 Estado actual de la aplicación

Todo el código de la aplicación vive en un único archivo, `src/index.js`:

```js
import express from 'express';

const app = express();
const port = process.env.PORT || 3000;

app.listen(port, () => {
  console.log(`Servidor corriendo en el puerto ${port}`);
});
```

Puntos de extensión naturales:

- Registrar rutas y routers con `app.use(...)` / `app.get(...)`.
- Añadir middleware (CORS, body parser, manejo de errores).
- Crear controladores, servicios y modelos en `src/` e importarlos con el alias `@/...`.
- Escribir tests (aún no hay framework de testing configurado).

---

## 📁 Estructura del proyecto

```text
nodejs-complete-guide/
├── .editorconfig        # Configuración de estilo para editores
├── .env                 # Variables de entorno locales (ignorado por Git)
├── .env.example         # Plantilla de variables de entorno (PORT=)
├── .gitignore           # Archivos/carpetas excluidos del repositorio
├── .husky/
│   └── pre-commit       # Ejecuta `npx lint-staged` antes de cada commit
├── .prettierignore      # Archivos excluidos del formateo
├── .prettierrc          # Reglas de Prettier
├── babel.config.json    # Preset-env + alias "@" → ./src
├── eslint.config.mjs    # Config plana de ESLint (JS / JSON / CSS)
├── package.json         # Dependencias, scripts y lint-staged
├── package-lock.json
├── src/
│   └── index.js         # Punto de entrada del servidor Express
└── dist/                # Salida compilada (generada, ignorada por Git)
    ├── index.js
    └── index.min.js
```

---

## 🛠️ Control de calidad

El flujo está pensado para mantener el código limpio sin esfuerzo manual:

1. **Al editar:** `npm run dev` recarga el servidor automáticamente.
2. **Al hacer commit:** el hook `pre-commit` de Husky ejecuta `lint-staged`, que aplica:
   - `eslint --fix` y `prettier --write` a archivos `*.js`.
   - `prettier --write` a archivos `*.json` y `*.md`.
3. **Para verificar manualmente:**

```bash
npx eslint .          # revisa JS, JSON y CSS
npx prettier --check . # comprueba el formato
```

> No hay scripts dedicados de `lint`/`format` en `package.json`; se usan las herramientas directamente o a través de `lint-staged`.
>
> ⚠️ **Nota:** aunque `lint-staged` invoca `prettier --write`, el paquete `prettier` **no está declarado** en `devDependencies` ni instalado actualmente. Para que el formateo funcione en el hook, añádelo con `npm install --save-dev prettier`.

---

## 📦 Dependencias

**Producción**

- `express` (^5.3.0)

**Desarrollo (destacadas)**

- Babel: `@babel/cli`, `@babel/core`, `@babel/node`, `@babel/preset-env`, `babel-plugin-module-resolver`, `babel-preset-minify`
- Servidor de desarrollo: `nodemon`
- Calidad: `eslint` + `@eslint/js`, `@eslint/json`, `@eslint/css`, `globals`, `lint-staged` (Prettier requiere instalación; ver nota)
- Git: `husky`, `lint-staged`
- Build: `rimraf`, `terser`, `terser-folder`, `cross-env`

---

## 📄 Licencia

ISC — ver el campo `license` en `package.json`.
