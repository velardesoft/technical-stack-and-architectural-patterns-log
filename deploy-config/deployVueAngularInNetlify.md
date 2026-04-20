# 🚀 Deploy Manual en Netlify — Angular 19 & Vue

> **Guía de configuración para despliegue manual de proyectos Frontend hacia Netlify**

---

## 📋 Índice

- [Deploy con Angular 19](#-deploy-con-angular-19)
- [Deploy con Vue + Vue Router](#-deploy-con-vue--vue-router)
- [¿Por qué el archivo `_redirects`?](#-por-qué-el-archivo-_redirects)
- [Comparativa rápida](#-comparativa-rápida)

---

## 🅰️ Deploy con Angular 19

### Paso 1 — Build de producción

Ejecuta el siguiente comando en la raíz de tu proyecto Angular:

```bash
ng build --configuration=production
```

> ⏳ Este proceso compila y optimiza el proyecto. Al finalizar, se generará la siguiente estructura:

```
dist/
└── <nombre-de-tu-proyecto>/
    └── browser/        ← 📂 Esta es la carpeta que usarás
          ├── index.html
          ├── main.js
          ├── styles.css
          └── ...
```

---

### Paso 2 — Crear el archivo `_redirects`

Ingresa a la carpeta **`browser`** y crea manualmente el archivo llamado exactamente:

```
_redirects
```

> ⚠️ Sin extensión. Solo `_redirects`.

**Contenido del archivo:**

```
/*    /index.html   200
```

Tu estructura final debe verse así:

```
dist/
└── <nombre-de-tu-proyecto>/
    └── browser/
          ├── index.html
          ├── main.js
          ├── styles.css
          └── _redirects    ← ✅ Archivo creado aquí
```

---

### Paso 3 — Deploy en Netlify

1. Ingresa a [netlify.com](https://netlify.com) y accede a tu cuenta.
2. En el dashboard, ve a la sección **"Sites"**.
3. En la parte inferior encontrarás el área de drag & drop:

```
┌─────────────────────────────────────────┐
│  Want to deploy a new site without      │
│  connecting to Git?                     │
│                                         │
│     [ Drag and drop your site folder ]  │
└─────────────────────────────────────────┘
```

4. **Arrastra únicamente la carpeta `browser`** hacia esa zona.

> ✅ Netlify desplegará tu aplicación Angular en producción.

---

## 💚 Deploy con Vue + Vue Router

### Paso 1 — Build de producción

Ejecuta el siguiente comando en la raíz de tu proyecto Vue:

```bash
npm run build
```

> ⏳ Al finalizar, se generará la siguiente estructura:

```
dist/
  ├── index.html
  ├── assets/
  │   ├── main.js
  │   └── style.css
  └── ...
```

---

### Paso 2 — Crear el archivo `_redirects`

Dentro de la carpeta **`dist`** (raíz del build), crea el archivo:

```
_redirects
```

**Contenido del archivo:**

```
/*    /index.html   200
```

Tu estructura final debe verse así:

```
dist/
  ├── index.html
  ├── assets/
  │   └── ...
  └── _redirects    ← ✅ Archivo creado aquí
```

---

### Paso 3 — Deploy en Netlify

1. Ingresa a [netlify.com](https://netlify.com) y accede a tu cuenta.
2. En el dashboard, ve a la sección **"Sites"**.
3. **Arrastra todo el contenido de la carpeta `dist`** hacia la zona de drag & drop.

> ✅ Netlify desplegará tu aplicación Vue en producción con Vue Router funcionando correctamente.

---

## ❓ ¿Por qué el archivo `_redirects`?

Tanto Angular como Vue son **SPA (Single Page Applications)**. Esto significa que toda la navegación es manejada por JavaScript en el cliente, no por el servidor.

El problema ocurre cuando un usuario accede directamente a una ruta como:

```
https://mi-app.netlify.app/dashboard/perfil
```

Netlify buscaría un archivo físico en esa ruta en el servidor — que no existe — y retornaría un **error 404**.

### La solución: `_redirects`

```
/*    /index.html   200
```

| Parte | Significado |
|-------|-------------|
| `/*` | Cualquier ruta solicitada |
| `/index.html` | Redirige siempre al punto de entrada de la SPA |
| `200` | Responde con status OK (no es una redirección 301/302) |

Esto le indica a Netlify que **todas las rutas deben servir `index.html`**, y que sea el router del framework (Angular Router / Vue Router) quien maneje la navegación internamente.

---

## 📊 Comparativa rápida

| Aspecto | Angular 19 | Vue |
|--------|------------|-----|
| Comando de build | `ng build --configuration=production` | `npm run build` |
| Carpeta generada | `dist/<proyecto>/browser/` | `dist/` |
| Ubicación de `_redirects` | Dentro de `browser/` | Dentro de `dist/` |
| Qué arrastrar a Netlify | La carpeta `browser/` | El contenido de `dist/` |
| Archivo `_redirects` necesario | ✅ Sí | ✅ Sí |

---

## 📁 Estructura del repositorio sugerida

```
📦 mi-portafolio-dev/
├── 📄 README.md
├── 📂 solid-principles/
│   └── 📄 README.md
└── 📂 deploy-configs/
    └── 📄 deployVueAngularInNetlify.md    ← 📍 Este archivo
```

---

## 📚 Referencias

- 🌐 [Documentación oficial de Netlify — Redirects](https://docs.netlify.com/routing/redirects/)
- 🌐 [Angular CLI — ng build](https://angular.io/cli/build)
- 🌐 [Vue CLI — Build & Deploy](https://cli.vuejs.org/guide/deployment.html#netlify)

---

*© Portafolio de Ingeniería de Software — GitHub*
