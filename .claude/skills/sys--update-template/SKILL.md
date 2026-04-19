# Skill: Update Template

Sincroniza los skills, agents y el `.mcp.json` del proyecto actual al repositorio template `claude-starter`, para que los próximos proyectos partan de la versión más reciente.

## Trigger

Usar cuando el usuario invoque `/sys--update-template` o pida sincronizar / propagar cambios al template.

## Configuración

- **Ruta local del template**: `C:\david\development\claude-templates\claude-starter`
- **Repo remoto**: `https://github.com/dgarciahz/claude-starter`

## Agents incluidos en el template

Solo se sincronizan estos agents (lista explícita). El usuario puede tener agents propios en el proyecto que NO deben subirse al template:

- `test-architect.md`

Si el usuario pide añadir o quitar un agent de esta lista, actualiza el SKILL.md antes de continuar.

## Skills incluidos en el template

Solo se sincronizan estos skills (lista explícita). El usuario puede tener skills propios en el proyecto que NO deben subirse al template:

- `gh--push`
- `n8n--code-javascript`
- `n8n--code-python`
- `n8n--expression-syntax`
- `n8n--mcp-tools-expert`
- `n8n--node-configuration`
- `n8n--validation-expert`
- `n8n--workflow-patterns`
- `sys--context-report`
- `sys--update-template`

Si el usuario pide añadir o quitar un skill de esta lista, actualiza el SKILL.md antes de continuar.

## Instrucciones

Sigue estos pasos en orden:

### 1. Verificar que el clone del template existe

Comprueba que existe `C:\david\development\claude-templates\claude-starter\.git`. Si no existe, informa al usuario y detente — hay que clonar el repo primero.

### 2. Sincronizar skills

Copia **solo los skills de la lista anterior** desde `.claude/skills/` del proyecto actual a `C:\david\development\claude-templates\claude-starter\.claude\skills\`, sobreescribiendo los archivos existentes.

No copies skills que no estén en la lista — pueden ser skills específicos del proyecto activo.

No copies `.claude/settings.local.json` ni ningún otro archivo de `.claude/` que no sea `skills/`.

### 3. Sincronizar agents

Copia **solo los agents de la lista anterior** desde `.claude/agents/` del proyecto actual a `C:\david\development\claude-templates\claude-starter\.claude\agents\`, sobreescribiendo los archivos existentes.

No copies agents que no estén en la lista — pueden ser agents específicos del proyecto activo.

### 4. Sincronizar .mcp.json (con sanitización)

Lee el `.mcp.json` del proyecto actual. Antes de copiarlo al template:
- Reemplaza el valor de `N8N_API_KEY` por `"TU_API_KEY_AQUI"`
- Reemplaza el valor de `N8N_API_URL` por `"https://TU-INSTANCIA.duckdns.org/api/v1"` si contiene una URL real

Escribe el resultado en `C:\david\development\claude-templates\claude-starter\.mcp.json`.

### 5. Sincronizar CLAUDE.md (opcional)

Si el usuario indicó explícitamente que quiere actualizar también el `CLAUDE.md`, cópialo. Si no lo mencionó, pregunta antes de hacerlo — el CLAUDE.md suele tener contenido específico del proyecto activo.

### 6. Hacer commit y push en el template

Desde `C:\david\development\claude-templates\claude-starter`:
- `git add .`
- `git commit -m "Sincroniza skills/agents desde [nombre del proyecto actual] — [fecha]"`
- `git push`

### 7. Confirmar

Informa al usuario de:
- Qué archivos fueron actualizados
- Si hubo archivos nuevos (skills nuevos que no existían en el template)
- Si hubo archivos eliminados en el proyecto (y por tanto ausentes en la sincronización)
- La URL del repo: `https://github.com/dgarciahz/claude-starter`

## Notas

- NUNCA copies `.claude/settings.local.json` al template — contiene rutas absolutas y permisos específicos de cada proyecto.
- Si el usuario quiere actualizar solo un skill concreto en lugar de todos, acepta el nombre como argumento y copia únicamente ese directorio.
- Tras el push, el template queda actualizado pero los proyectos ya creados desde él NO reciben los cambios automáticamente — eso es por diseño.
