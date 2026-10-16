# ✅ App de Tareas — React + Node.js/Express

Aplicación full-stack de lista de tareas. El **frontend** está hecho con React + Vite y el **backend** con Node.js + Express. Este repositorio está preparado para desarrollo local y para desplegar en **Render** (backend) y **Vercel** (frontend).

## 📁 Estructura del proyecto

```
tareas-app/
├── server/                # Backend (Node.js + Express)
│   ├── index.js           # API REST
│   ├── package.json
│   ├── .env               # Variables locales (NO se sube a git)
│   └── .env.example       # Plantilla de variables
├── client/                # Frontend (React + Vite)
│   ├── src/
│   │   ├── App.jsx
│   │   ├── main.jsx
│   │   ├── App.css
│   │   └── components/
│   ├── index.html
│   ├── vite.config.js
│   ├── package.json
│   ├── .env               # Variables locales (NO se sube a git)
│   └── .env.example       # Plantilla de variables
└── .gitignore
```

## 🛠️ Requisitos previos

- [Node.js](https://nodejs.org/) 18 o superior (recomendado 20 LTS)
- npm (viene incluido con Node.js)
- [Git](https://git-scm.com/) para subir el proyecto a GitHub

Verifica tu instalación:

```bash
node -v
npm -v
git --version
```

## 🚀 Instalación y ejecución local

Debes usar **dos terminales** (una para el backend y otra para el frontend).

### Terminal 1 — Backend

```bash
cd server
npm install
npm run dev        # corre en http://localhost:3001
```

### Terminal 2 — Frontend

```bash
cd client
npm install
npm run dev        # corre en http://localhost:5173
```

Luego abre http://localhost:5173 en el navegador.

## 🔐 Variables de entorno

El proyecto usa variables de entorno para separar la configuración local de la de producción. Las plantillas están en `.env.example`.

### Backend — `server/.env`

| Variable      | Descripción                                                        | Ejemplo local             |
|---------------|--------------------------------------------------------------------|---------------------------|
| `PORT`        | Puerto del servidor. Render lo asigna automáticamente en producción | `3001`                    |
| `CLIENTE_URL` | Origen del frontend autorizado por CORS                            | `http://localhost:5173`   |

```env
PORT=3001
CLIENTE_URL=http://localhost:5173
```

### Frontend — `client/.env`

| Variable        | Descripción                          | Ejemplo local           |
|-----------------|--------------------------------------|-------------------------|
| `VITE_API_URL`  | URL base de la API (backend)         | `http://localhost:3001` |

```env
VITE_API_URL=http://localhost:3001
```

> En Vite, **solo** las variables que empiezan con `VITE_` son accesibles desde el código del cliente mediante `import.meta.env`.

## 📡 Endpoints de la API

| Método | Ruta               | Descripción               |
|--------|--------------------|---------------------------|
| GET    | `/api/tareas`      | Obtener todas las tareas  |
| POST   | `/api/tareas`      | Crear una tarea           |
| PUT    | `/api/tareas/:id`  | Completar/descompletar    |
| DELETE | `/api/tareas/:id`  | Eliminar una tarea        |

### Prueba rápida de la API (sin navegador)

```bash
curl http://localhost:3001/api/tareas
curl -X POST http://localhost:3001/api/tareas -H "Content-Type: application/json" -d "{\"titulo\":\"Mi tarea\"}"
```

## 🧪 Cómo probar todo localmente (paso a paso)

1. **Abre dos terminales** y colócalas en la carpeta del proyecto.
2. En la **Terminal 1** ejecuta el backend:
   ```bash
   cd server
   npm install
   npm run dev
   ```
   Debes ver: `API corriendo en el puerto 3001`.
3. En la **Terminal 2** ejecuta el frontend:
   ```bash
   cd client
   npm install
   npm run dev
   ```
   Debes ver la URL `http://localhost:5173`.
4. Abre http://localhost:5173 en el navegador. Agrega, completa y elimina tareas.
5. Si quieres verificar el backend aislado, abre http://localhost:3001/api/tareas: debe devolver el JSON con las tareas.
6. Prueba el flujo completo:
   - Agrega una tarea desde el navegador → debería aparecer al instante.
   - Recarga la página (F5) → la tarea sigue ahí (viene del backend).
   - Revisa la consola de la Terminal 1: se registran las peticiones.
7. Para detener ambos servidores presiona `Ctrl + C` en cada terminal.

### Errores comunes

- **`CORS: Origen no permitido`**: verifica que `CLIENTE_URL` en `server/.env` coincida con el origen del frontend.
- **El frontend no conecta**: revisa que `VITE_API_URL` en `client/.env` apunte a `http://localhost:3001` y que el backend esté corriendo.
- **Después de cambiar un `.env` del cliente**: reinicia el servidor de Vite (`Ctrl + C` y `npm run dev`), porque las variables se cargan al iniciar.

## ☁️ Despliegue

### Backend en Render

1. Entra a [render.com](https://render.com/) y crea un **New → Web Service**.
2. Conecta tu repositorio de GitHub.
3. Configura:
   - **Root Directory**: `server`
   - **Build Command**: `npm install`
   - **Start Command**: `npm start`
   - **Environment Variables**:
     - `CLIENTE_URL` = URL pública de tu frontend en Vercel (ej. `https://tareas-app.vercel.app`)
     - (No definas `PORT`, Render lo inyecta automáticamente.)
4. Copia la URL pública del backend (ej. `https://tareas-api.onrender.com`).

### Frontend en Vercel

1. Entra a [vercel.com](https://vercel.com/) y elige **Add New → Project**.
2. Importa el repositorio de GitHub.
3. Configura:
   - **Root Directory**: `client`
   - **Framework Preset**: Vite
   - **Build Command**: `npm run build`
   - **Output Directory**: `dist`
   - **Environment Variables**:
     - `VITE_API_URL` = URL del backend en Render (ej. `https://tareas-api.onrender.com`)
4. Despliega. Vercel te dará la URL pública del frontend.

> ⚠️ Importante: al desplegar, vuelve a Render y actualiza `CLIENTE_URL` con la URL real de Vercel para que CORS funcione.

## 📤 Subir los cambios a GitHub

```bash
git init
git add .
git commit -m "Preparar app para despliegue en Render y Vercel"
git branch -M main
git remote add origin https://github.com/TU-USUARIO/TU-REPO.git
git push -u origin main
```

Antes de subir, confirma que `.env` esté ignorado:

```bash
git status        # los archivos .env NO deben aparecer
```

El archivo `.gitignore` ya excluye `node_modules/`, `dist/`, `.env` y otros archivos generados.
