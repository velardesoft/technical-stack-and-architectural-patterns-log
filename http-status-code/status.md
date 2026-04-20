# 🌐 HTTP Status Codes — Guía para Ingenieros de Software

> **Referencia técnica completa de códigos de estado HTTP para desarrollo web profesional**

---

## 📋 Índice

- [¿Qué son los Status Codes?](#-qué-son-los-status-codes)
- [2xx — Éxito](#-2xx--éxito)
- [3xx — Redirecciones](#-3xx--redirecciones)
- [4xx — Errores del Cliente](#-4xx--errores-del-cliente)
- [5xx — Errores del Servidor](#-5xx--errores-del-servidor)
- [Aplicación práctica en desarrollo web](#-aplicación-práctica-en-desarrollo-web)
- [Tabla resumen rápida](#-tabla-resumen-rápida)

---

## 📡 ¿Qué son los Status Codes?

Los **códigos de estado HTTP** son respuestas estandarizadas que un servidor envía al cliente para indicar el resultado de una solicitud. Forman parte del protocolo **HTTP/1.1** definido en el [RFC 9110](https://www.rfc-editor.org/rfc/rfc9110).

```
Cliente (Browser / App)  ──── Request ────▶  Servidor
Cliente (Browser / App)  ◀─── Response ───  Servidor
                                  │
                          [ Status Code ]
                          [ Headers     ]
                          [ Body        ]
```

Se agrupan en **5 familias** según el primer dígito:

| Familia | Rango | Significado |
|---------|-------|-------------|
| **1xx** | 100–199 | Informacional |
| **2xx** | 200–299 | Éxito ✅ |
| **3xx** | 300–399 | Redirección 🔁 |
| **4xx** | 400–499 | Error del cliente ❌ |
| **5xx** | 500–599 | Error del servidor 🔥 |

---

## ✅ 2xx — Éxito

El servidor recibió, entendió y procesó correctamente la solicitud.

### `200 OK`
La respuesta estándar de éxito. Se usa en peticiones `GET`, `PUT`, `PATCH` y `DELETE` cuando la operación se completó correctamente.

```http
GET /api/users/1 HTTP/1.1

HTTP/1.1 200 OK
Content-Type: application/json

{ "id": 1, "name": "Carlos" }
```

> 💡 **Uso:** Consulta de recursos existentes, login exitoso, respuestas generales.

---

### `201 Created`
Se creó un nuevo recurso como resultado de la solicitud. Ideal como respuesta a un `POST` exitoso.

```http
POST /api/users HTTP/1.1

HTTP/1.1 201 Created
Location: /api/users/42
Content-Type: application/json

{ "id": 42, "name": "Ana" }
```

> 💡 **Uso:** Registro de usuarios, creación de entidades, subida de archivos.

---

### `202 Accepted`
La solicitud fue aceptada para procesamiento, pero aún **no ha sido completada**. Útil en operaciones asíncronas o tareas en cola.

```http
POST /api/reports/generate HTTP/1.1

HTTP/1.1 202 Accepted

{ "message": "Reporte en cola. Recibirás una notificación." }
```

> 💡 **Uso:** Envío de emails en background, generación de reportes, procesamiento batch.

---

### `204 No Content`
Operación exitosa, pero **no hay cuerpo de respuesta**. Común en `DELETE` o `PUT` cuando no hay nada que retornar.

```http
DELETE /api/users/42 HTTP/1.1

HTTP/1.1 204 No Content
```

> 💡 **Uso:** Eliminación de recursos, actualizaciones sin respuesta necesaria.

---

## 🔁 3xx — Redirecciones

El cliente debe realizar una acción adicional para completar la solicitud.

### `301 Moved Permanently`
El recurso fue movido **de forma definitiva** a una nueva URL. Los navegadores y buscadores actualizan su caché automáticamente.

```http
GET /old-page HTTP/1.1

HTTP/1.1 301 Moved Permanently
Location: https://mi-sitio.com/new-page
```

> 💡 **Uso:** Migraciones de dominio, reestructuración de URLs, SEO.

---

### `302 Found`
Redirección **temporal**. El cliente debe seguir usando la URL original en el futuro.

```http
HTTP/1.1 302 Found
Location: /maintenance.html
```

> 💡 **Uso:** Mantenimiento temporal, redirecciones condicionales.

---

### `304 Not Modified`
El recurso **no ha cambiado** desde la última vez que fue solicitado. El cliente puede usar su versión en caché.

```http
GET /api/assets/logo.png HTTP/1.1
If-None-Match: "abc123"

HTTP/1.1 304 Not Modified
```

> 💡 **Uso:** Optimización de performance, manejo de caché en APIs y assets estáticos.

---

## ❌ 4xx — Errores del Cliente

El error fue causado por la solicitud del cliente — datos incorrectos, falta de permisos, recurso inexistente.

### `400 Bad Request`
La solicitud tiene **sintaxis malformada** o parámetros inválidos que el servidor no puede procesar.

```http
POST /api/users HTTP/1.1

{ "email": "no-es-un-email" }

HTTP/1.1 400 Bad Request

{ "error": "El campo email no tiene un formato válido." }
```

> 💡 **Uso:** Validación de formularios, body JSON mal formado, parámetros faltantes.

---

### `401 Unauthorized`
El cliente **no está autenticado**. Debe proporcionar credenciales válidas (token, API key, sesión).

```http
GET /api/profile HTTP/1.1

HTTP/1.1 401 Unauthorized
WWW-Authenticate: Bearer

{ "error": "Token no proporcionado o expirado." }
```

> 💡 **Uso:** Rutas protegidas sin token JWT, sesión expirada, credenciales incorrectas.

---

### `403 Forbidden`
El cliente está autenticado, pero **no tiene permiso** para acceder al recurso.

```http
DELETE /api/admin/users/1 HTTP/1.1
Authorization: Bearer <token-de-usuario-normal>

HTTP/1.1 403 Forbidden

{ "error": "No tienes permisos para realizar esta acción." }
```

> 💡 **Diferencia clave con 401:** En el 401 el usuario no está identificado; en el 403 sí lo está, pero no tiene rol suficiente.

---

### `404 Not Found`
El recurso solicitado **no existe** en el servidor.

```http
GET /api/users/9999 HTTP/1.1

HTTP/1.1 404 Not Found

{ "error": "Usuario no encontrado." }
```

> 💡 **Uso:** ID inexistente en base de datos, ruta incorrecta, recurso eliminado.

---

### `405 Method Not Allowed`
El método HTTP usado (`GET`, `POST`, `DELETE`, etc.) **no está permitido** para esa ruta.

```http
DELETE /api/auth/login HTTP/1.1

HTTP/1.1 405 Method Not Allowed
Allow: POST, GET
```

> 💡 **Uso:** Cuando se llama con `DELETE` a un endpoint que solo acepta `POST`.

---

### `408 Request Timeout`
El servidor esperó demasiado tiempo para recibir la solicitud completa del cliente.

```http
HTTP/1.1 408 Request Timeout

{ "error": "La solicitud tardó demasiado en completarse." }
```

> 💡 **Uso:** Conexiones lentas, uploads pesados sin progreso, clientes inactivos.

---

## 🔥 5xx — Errores del Servidor

El servidor falló al procesar una solicitud válida. El error **no es culpa del cliente**.

### `500 Internal Server Error`
Error genérico del servidor. Algo falló internamente sin una causa específica identificada.

```http
GET /api/data HTTP/1.1

HTTP/1.1 500 Internal Server Error

{ "error": "Ocurrió un error inesperado. Intenta más tarde." }
```

> ⚠️ **En producción:** Nunca expongas el stack trace real. Loguea el error internamente y retorna un mensaje genérico al cliente.

---

### `501 Not Implemented`
El servidor **no soporta** la funcionalidad necesaria para completar la solicitud.

```http
PATCH /api/resource HTTP/1.1

HTTP/1.1 501 Not Implemented
```

> 💡 **Uso:** Métodos HTTP no implementados aún en el servidor.

---

### `502 Bad Gateway`
El servidor actuó como **gateway o proxy** y recibió una respuesta inválida del servidor upstream.

```http
HTTP/1.1 502 Bad Gateway

{ "error": "El servidor no pudo obtener una respuesta válida." }
```

> 💡 **Uso frecuente:** Nginx recibió una respuesta inválida de tu app Node/Python, microservicio caído detrás de un proxy.

---

### `503 Service Unavailable`
El servidor **no puede manejar la solicitud** en este momento — sobrecarga o mantenimiento.

```http
HTTP/1.1 503 Service Unavailable
Retry-After: 120

{ "error": "Servicio temporalmente no disponible." }
```

> 💡 **Uso:** Deploy en curso, servidor saturado, mantenimiento programado.

---

### `504 Gateway Timeout`
El servidor gateway **no recibió respuesta a tiempo** del servidor upstream.

```http
HTTP/1.1 504 Gateway Timeout
```

> 💡 **Diferencia con 502:** El 502 recibe una respuesta inválida; el 504 directamente no recibe respuesta.

---

## 🛠️ Aplicación práctica en desarrollo web

### Manejo correcto en una API REST

```javascript
// Node.js + Express — Ejemplo de respuestas semánticas correctas

// GET → 200
app.get('/api/users/:id', async (req, res) => {
  const user = await User.findById(req.params.id);
  if (!user) return res.status(404).json({ error: 'Usuario no encontrado' });
  res.status(200).json(user);
});

// POST → 201
app.post('/api/users', async (req, res) => {
  const user = await User.create(req.body);
  res.status(201).json(user);
});

// DELETE → 204
app.delete('/api/users/:id', async (req, res) => {
  await User.deleteById(req.params.id);
  res.status(204).send();
});

// Error global → 500
app.use((err, req, res, next) => {
  console.error(err.stack); // Log interno
  res.status(500).json({ error: 'Error interno del servidor' });
});
```

### Manejo en el cliente (Angular / Vue)

```typescript
// Angular — Interceptor HTTP para manejo global de errores
intercept(req: HttpRequest<any>, next: HttpHandler) {
  return next.handle(req).pipe(
    catchError((error: HttpErrorResponse) => {
      switch (error.status) {
        case 401: this.router.navigate(['/login']); break;
        case 403: this.router.navigate(['/forbidden']); break;
        case 404: this.router.navigate(['/not-found']); break;
        case 500: this.notifyService.error('Error del servidor'); break;
      }
      return throwError(() => error);
    })
  );
}
```

---

## 📊 Tabla resumen rápida

| Código | Nombre | Cuándo usarlo |
|--------|--------|---------------|
| `200` | OK | Respuesta exitosa general |
| `201` | Created | Recurso creado (POST) |
| `202` | Accepted | Proceso en background |
| `204` | No Content | Éxito sin body (DELETE) |
| `301` | Moved Permanently | Redirección definitiva |
| `302` | Found | Redirección temporal |
| `304` | Not Modified | Usar caché del cliente |
| `400` | Bad Request | Datos inválidos del cliente |
| `401` | Unauthorized | Sin autenticación |
| `403` | Forbidden | Sin permisos suficientes |
| `404` | Not Found | Recurso inexistente |
| `405` | Method Not Allowed | Método HTTP incorrecto |
| `408` | Request Timeout | Cliente tardó demasiado |
| `500` | Internal Server Error | Fallo genérico del servidor |
| `501` | Not Implemented | Método no soportado |
| `502` | Bad Gateway | Respuesta inválida upstream |
| `503` | Service Unavailable | Servidor no disponible |
| `504` | Gateway Timeout | Timeout desde upstream |

---

## 📚 Referencias

- 🌐 [RFC 9110 — HTTP Semantics (IETF)](https://www.rfc-editor.org/rfc/rfc9110)
- 🌐 [MDN Web Docs — HTTP Status Codes](https://developer.mozilla.org/es/docs/Web/HTTP/Status)
- 🌐 [HTTP Status Codes Glossary — whatishttp.com](https://www.whatishttp.com)

---

*© Portafolio de Ingeniería de Software — GitHub*
