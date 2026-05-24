---
name: commit
description: Crear un commit siguiendo Conventional Commits del proyecto. Usar cuando el usuario quiera commitear cambios o pida crear un commit.
disable-model-invocation: true
allowed-tools: Bash(git status) Bash(git diff *) Bash(git add *) Bash(git commit *)
---

## Estado actual

!`git status --short`

## Cambios pendientes

!`git diff HEAD`

## Instrucciones

Crea un commit con Conventional Commits para los cambios listados arriba.

1. Revisa qué archivos tienen cambios (staged y unstaged)
2. Si no hay nada en staging, agrega los archivos relevantes con `git add` — nunca `git add -A` ni `git add .`
3. Elige el tipo correcto:
   - `feat` — nueva funcionalidad
   - `fix` — corrección de bug
   - `refactor` — refactorización sin cambio de comportamiento
   - `test` — tests
   - `chore` — dependencias, config, scripts
   - `docs` — documentación
4. Scope = nombre del módulo afectado (board, workspace, auth, user, etc.)
5. Descripción: imperativo, minúscula, sin punto al final
6. Formato: `<tipo>(<scope>): <descripción>`
7. Ejecuta el commit con `git commit -m "..."`

**Reglas:**
- Nunca `--no-verify`
- Nunca amend en commits ya publicados
- No incluir archivos `.env` ni credenciales
