# Skill: GitHub Pull

Sincroniza la rama local con el remote haciendo `git pull`. Advierte si hay cambios locales sin commitear que puedan generar conflictos.

## Trigger

Usar cuando el usuario invoque `/core--gh-pull` o pida hacer pull o sincronizar desde el remote.

## Instrucciones

### 1. Ejecutar el script

```bash
bash .claude/skills/core--gh-pull/github_pull.sh [rama] [remote]
```

- Sin argumentos: usa la rama actual y `origin`.
- Con argumentos opcionales: `bash .claude/skills/core--gh-pull/github_pull.sh RAMA REMOTE`

### 2. Mostrar el output del script al usuario.
