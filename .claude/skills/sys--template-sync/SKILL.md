# Skill: Template Sync

Actualiza los skills y agents del proyecto actual desde el template `claude-starter`, sin tocar la configuración local (`.mcp.json`, `settings.local.json`).

## Trigger

Usar cuando el usuario invoque `/sys--template-sync` o pida actualizar/sincronizar skills desde el template.

## Configuración

- **Repo template**: `https://github.com/dgarciahz/claude-starter`
- **Remote name**: `template`

## Instrucciones

Sigue estos pasos en orden:

### 1. Verificar o añadir el remote `template`

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

### 3. Mostrar qué ha cambiado antes de aplicar

Muestra al usuario un diff resumido de lo que va a cambiar:

```bash
git diff HEAD template/main -- .claude/skills/ .claude/agents/
```

Si no hay diferencias, informa al usuario y detente — el proyecto ya está al día.

### 4. Bajar skills y agents

```bash
git checkout template/main -- .claude/skills/
git checkout template/main -- .claude/agents/
```

### 5. Commit de los cambios

```bash
git add .claude/skills/ .claude/agents/
git commit -m "Sincroniza skills/agents desde template claude-starter — <fecha>"
```

### 6. Confirmar al usuario

Informa de:
- Qué skills/agents fueron actualizados
- Si hay skills nuevos que no existían antes en el proyecto
- Recordatorio: NO se ha tocado `.mcp.json` ni `settings.local.json`

## Notas

- NUNCA toques `.mcp.json`, `settings.local.json` ni ningún otro archivo fuera de `.claude/skills/` y `.claude/agents/`.
- Si el usuario quiere actualizar también los MCP servers, debe ejecutar `/sys--template-init`.
- Este skill es idempotente: ejecutarlo varias veces no causa daño.
