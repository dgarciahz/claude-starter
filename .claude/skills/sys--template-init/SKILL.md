# Skill: Init Template

Inicializa un proyecto nuevo (o existente) desde el template `claude-starter`: baja skills, agents y asiste al usuario para configurar los MCP servers que necesite.

## Trigger

Usar cuando el usuario invoque `/sys--template-init` o pida inicializar/actualizar el proyecto desde el template.

## Configuración

- **Repo template**: `https://github.com/dgarciahz/claude-starter`
- **Remote name**: `template`
- **Catálogo MCP**: `.claude/skills/sys--template-update/MCP_SERVERS.md` (se descarga junto con los skills)

## Instrucciones

Sigue estos pasos en orden:

### 1. Verificar o añadir el remote `template`

Comprueba si el remote `template` ya existe:
```bash
git remote get-url template
```
Si no existe, añádelo:
```bash
git remote add template https://github.com/dgarciahz/claude-starter
```

### 2. Fetch del template

```bash
git fetch template
```

### 3. Bajar skills y agents

```bash
git checkout template/main -- .claude/skills/
git checkout template/main -- .claude/agents/
```

### 4. Leer el catálogo de MCP servers

Lee el archivo `.claude/skills/sys--template-update/MCP_SERVERS.md` que acaba de descargarse.

### 5. Preguntar al usuario qué servers quiere configurar

Para cada servidor del catálogo, pregunta si quiere incluirlo en el proyecto. Usa `AskUserQuestion` con multiSelect si hay varios de golpe, o pregunta uno a uno si el catálogo es largo.

Para los servers que requieren credenciales (`N8N_API_URL`, `N8N_API_KEY`, etc.), solicítalas al usuario en este paso.

### 6. Detectar la ruta de npx según el SO

- **Windows**: comprueba si existe `C:\Program Files\nodejs\npx.cmd`. Si no, busca con `where npx`.
- **macOS / Linux**: usa simplemente `npx`.

### 7. Escribir `.mcp.json`

Construye el archivo `.mcp.json` en la raíz del proyecto con los servers elegidos. Ejemplo de estructura:

```json
{
  "mcpServers": {
    "nombre-server": {
      "command": "<ruta npx>",
      "args": ["-y", "<paquete>"],
      "env": {
        "VAR": "valor"
      }
    }
  }
}
```

Si ya existe un `.mcp.json`, combina los servers nuevos con los existentes (no sobreescribas los que ya estén configurados).

### 8. Actualizar `.claude/settings.local.json`

Añade los servers elegidos a `enabledMcpjsonServers`. Si el archivo no existe, créalo con esta estructura mínima:

```json
{
  "enableAllProjectMcpServers": false,
  "enabledMcpjsonServers": ["server1", "server2"]
}
```

### 9. Commit de los cambios

```bash
git add .claude/ .mcp.json
git commit -m "Inicializa proyecto desde template claude-starter — <fecha>"
```

### 10. Confirmar al usuario

Informa de:
- Qué skills y agents se descargaron
- Qué MCP servers quedaron configurados
- Si hay credenciales pendientes de rellenar en `.mcp.json`
- Recordatorio: reiniciar Claude Code para que los MCP servers nuevos se activen

## Notas

- NUNCA sobreescribas `.claude/settings.local.json` completo si ya existe — haz merge de `enabledMcpjsonServers`.
- Si el usuario ya tiene servers en `.mcp.json`, respétalos y añade solo los nuevos.
- Este skill cubre la inicialización base. Pasos adicionales de personalización de sesión se gestionan por separado.
