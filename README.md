# WP AI REST Toolkit

Toolkit para que agentes de IA interactúen directamente con sitios WordPress mediante la REST API nativa usando `curl`, sin MCP.

## Estructura

```
wp-ai-rest-toolkit/
├── AGENTS.md.example  # Plantilla de configuración del proyecto
├── README.md          # Este archivo
├── .gitignore         # Para evitar subir credenciales por accidente
└── skills/
    └── wp-ai-rest-toolkit/
        └── SKILL.md   # Documentación completa de la REST API para agentes de IA
```

## Uso

### 1. Configurar tus sitios

Copia la plantilla `AGENTS.md.example` a `AGENTS.md` y reemplaza los placeholders con tus datos reales. `AGENTS.md` está ignorado por git para evitar subir credenciales:

```markdown
### Site 1
- **Name:** Mi Sitio
- **Site URL:** https://misitio.com
- **REST API Base:** https://misitio.com/wp-json/wp/v2
- **Auth:** miusuario:APPLICATION_PASSWORD
```

**Obtener Application Password en WordPress:**
1. Ve a tu panel de WordPress
2. Usuarios → Tu perfil
3. Desplázate hasta "Application Passwords"
4. Crea una nueva contraseña para "WP AI REST Toolkit" o "API"
5. Copia el token generado

### 2. Comandos básicos

**Verificar conexión:**
```bash
curl -u "USERNAME:PASSWORD" https://misitio.com/wp-json/wp/v2/
```

**Crear una página:**
```bash
curl -X POST \
  -u "USERNAME:PASSWORD" \
  -H "Content-Type: application/json" \
  https://misitio.com/wp-json/wp/v2/pages \
  -d '{"title":"Hola","content":"Hola","status":"publish"}'
```

**Crear un post:**
```bash
curl -X POST \
  -u "USERNAME:PASSWORD" \
  -H "Content-Type: application/json" \
  https://misitio.com/wp-json/wp/v2/posts \
  -d '{"title":"Mi Post","content":"Contenido","status":"publish"}'
```

### 3. Operaciones avanzadas

Ver `skills/wp-ai-rest-toolkit/SKILL.md` para ver todos los comandos disponibles:
- Crear/actualizar/eliminar páginas y posts
- Subir imágenes y asignar imágenes destacadas
- Crear categorías
- Crear usuarios
- Trabajar con bloques de Gutenberg
- Programar contenido para futuro

## Seguridad

- **NUNCA** subas el `AGENTS.md` con credenciales reales a GitHub público
- Usa siempre **Application Passwords** (no tu contraseña principal)
- El `.gitignore` está configurado para ignorar archivos con credenciales
- Si accidentalmente subiste credenciales, revoca las Application Passwords inmediatamente

## Compatibilidad

- WordPress 5.0+ (con REST API habilitada)
- Algunos plugins de seguridad pueden bloquear ciertos endpoints (ej. listado de usuarios)
- Requiere que la REST API esté activa en tu sitio WordPress

## Licencia

MIT - ver `LICENSE`.