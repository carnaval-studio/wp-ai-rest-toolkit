---
name: wp-ai-rest-toolkit
description: "Use when interacting with WordPress sites through the native REST API using curl, Basic Auth, Application Passwords, pages, posts, media, users, menus, taxonomies, plugins, themes, site settings, and Gutenberg HTML blocks without MCP."
---

# WP AI REST Toolkit Skill

## Descripción
Skill para que agentes de IA interactúen directamente con sitios WordPress mediante la REST API nativa, sin usar MCP.

## Instrucciones de Uso

1. Primero, leer el archivo `AGENTS.md` del proyecto para obtener las credenciales y URLs específicas del sitio WordPress
2. Usar `curl` con autenticación básica para todas las operaciones
3. El formato de auth es: `usuario:contraseña` (Application Password de WordPress)

## Endpoints Principales

### Verificación de Conexión
```bash
curl -u "USERNAME:PASSWORD" \
  https://TUSITIO.com/wp-json/wp/v2/
```

---

## PÁGINAS

### Crear una página
```bash
curl -X POST \
  -u "USERNAME:PASSWORD" \
  -H "Content-Type: application/json" \
  https://TUSITIO.com/wp-json/wp/v2/pages \
  -d '{
    "title": "Título de la página",
    "content": "Contenido de la página",
    "status": "publish"
  }'
```

### Crear una página con contenido HTML y bloques de Gutenberg
```bash
curl -X POST \
  -u "USERNAME:PASSWORD" \
  -H "Content-Type: application/json" \
  https://TUSITIO.com/wp-json/wp/v2/pages \
  -d '{
    "title": "Mi Página",
    "content": "<!-- wp:paragraph --><p>Este es un párrafo de ejemplo.</p><!-- /wp:paragraph --><!-- wp:heading --><h2>Subtítulo</h2><!-- /wp:heading -->",
    "status": "publish"
  }'
```

### Listar todas las páginas
```bash
curl -u "USERNAME:PASSWORD" \
  https://TUSITIO.com/wp-json/wp/v2/pages
```

### Obtener una página específica (por ID)
```bash
curl -u "USERNAME:PASSWORD" \
  https://TUSITIO.com/wp-json/wp/v2/pages/PAGE_ID
```

### Actualizar una página existente
```bash
curl -X POST \
  -u "USERNAME:PASSWORD" \
  -H "Content-Type: application/json" \
  https://TUSITIO.com/wp-json/wp/v2/pages/PAGE_ID \
  -d '{
    "title": "Nuevo título",
    "content": "Nuevo contenido"
  }'
```

### Agregar contenido/bloques a una página existente
```bash
curl -X POST \
  -u "USERNAME:PASSWORD" \
  -H "Content-Type: application/json" \
  https://TUSITIO.com/wp-json/wp/v2/pages/PAGE_ID \
  -d '{
    "content": "<!-- wp:heading --><h2>Nueva sección</h2><!-- /wp:heading --><!-- wp:paragraph --><p>Contenido agregado.</p><!-- /wp:paragraph -->"
  }'
```

### Eliminar una página
```bash
curl -X DELETE \
  -u "USERNAME:PASSWORD" \
  https://TUSITIO.com/wp-json/wp/v2/pages/PAGE_ID
```

---

## POSTS / ENTRADAS

### Crear un post
```bash
curl -X POST \
  -u "USERNAME:PASSWORD" \
  -H "Content-Type: application/json" \
  https://TUSITIO.com/wp-json/wp/v2/posts \
  -d '{
    "title": "Título del post",
    "content": "Contenido del post",
    "status": "publish"
  }'
```

### Crear un post con categoría
```bash
curl -X POST \
  -u "USERNAME:PASSWORD" \
  -H "Content-Type: application/json" \
  https://TUSITIO.com/wp-json/wp/v2/posts \
  -d '{
    "title": "Título del post",
    "content": "Contenido del post",
    "categories": [1],
    "status": "publish"
  }'
```

### Listar todos los posts
```bash
curl -u "USERNAME:PASSWORD" \
  https://TUSITIO.com/wp-json/wp/v2/posts
```

---

## CATEGORÍAS Y ETIQUETAS

### Crear una categoría
```bash
curl -X POST \
  -u "USERNAME:PASSWORD" \
  -H "Content-Type: application/json" \
  https://TUSITIO.com/wp-json/wp/v2/categories \
  -d '{
    "name": "Nombre de la categoría",
    "description": "Descripción de la categoría"
  }'
```

### Listar categorías
```bash
curl -u "USERNAME:PASSWORD" \
  https://TUSITIO.com/wp-json/wp/v2/categories
```

---

## IMÁGENES Y MEDIA

### Subir una imagen
```bash
curl -X POST \
  -u "USERNAME:PASSWORD" \
  -H "Content-Disposition: attachment; filename=mi-imagen.jpg" \
  -H "Content-Type: image/jpeg" \
  https://TUSITIO.com/wp-json/wp/v2/media \
  --data-binary @/ruta/a/la/imagen.jpg
```

### Asignar imagen destacada a una página
```bash
curl -X POST \
  -u "USERNAME:PASSWORD" \
  -H "Content-Type: application/json" \
  https://TUSITIO.com/wp-json/wp/v2/pages/PAGE_ID \
  -d '{
    "featured_media": MEDIA_ID
  }'
```

---

## USUARIOS

### Crear un usuario
```bash
curl -X POST \
  -u "USERNAME:PASSWORD" \
  -H "Content-Type: application/json" \
  https://TUSITIO.com/wp-json/wp/v2/users \
  -d '{
    "username": "nuevousuario",
    "email": "usuario@email.com",
    "password": "contraseña123",
    "roles": ["author"]
  }'
```

### Listar usuarios
```bash
curl -u "USERNAME:PASSWORD" \
  https://TUSITIO.com/wp-json/wp/v2/users
```

### Obtener usuario actual
```bash
curl -u "USERNAME:PASSWORD" \
  https://TUSITIO.com/wp-json/wp/v2/users/me
```

---

## MENÚS

### Listar menús (requiere soporte del tema o plugin)
```bash
curl -u "USERNAME:PASSWORD" \
  https://TUSITIO.com/wp-json/wp/v2/menu-items
```

---

## TIPO DE CONTENIDO

### Status disponibles
- `publish` - Publicado
- `draft` - Borrador
- `private` - Privado
- `pending` - Pendiente de revisión
- `future` - Programado (requiere `date` futura)

### Formato de fechas
Para programar contenido, usar formato ISO 8601:
```json
{
  "status": "future",
  "date": "2026-12-25T10:00:00"
}
```

---

## NOTAS IMPORTANTES

- **NO usar MCP Adapter** - Usar solo REST API directamente via curl
- Las credenciales deben ser **Application Passwords** de WordPress (perfil de usuario)
- El formato de autenticación es: `usuario:contraseña`
- Para contenido HTML simple: `"content": "<h1>Título</h1><p>Párrafo</p>"`
- Para bloques Gutenberg: usar formato `<!-- wp:blockname -->...<!-- /wp:blockname -->`
- Para actualizar contenido, usar `POST` en la URL del recurso con su ID
- Para eliminar, usar `DELETE` en la URL del recurso con su ID
- Para listar, usar `GET` sin body
- IDs de páginas, posts, categorías, media, etc. se obtienen de las respuestas JSON al crear o listar
- Reemplaza siempre `https://TUSITIO.com` por la URL real del sitio WordPress
- Algunos sitios pueden tener el endpoint de usuarios deshabilitado por seguridad
