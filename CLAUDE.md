# Effica Boards — Contexto para Claude Code

## Qué es este proyecto

SaaS multi-tenant de gestión de trabajo. Jerarquía: `Account → Workspace → WorkspaceFolder → Board → BoardItem`.
Diseño API-first. Backend REST en NestJS, frontend en Next.js 15.

## Stack

- **Monorepo**: Turborepo
- **Frontend**: Next.js 15 App Router — `apps/web/`
- **Backend**: NestJS (Fastify adapter) — `apps/api/`
- **ORM**: Prisma — `packages/database/`
- **DB**: PostgreSQL (Neon en producción)
- **Auth**: Better Auth (JWT)
- **Real-time**: Socket.io embebido en NestJS
- **Queue**: BullMQ + Upstash Redis
- **Email**: Resend
- **UI**: shadcn/ui + Tailwind CSS
- **Estado**: TanStack Query (server) + Zustand (client)
- **Drag & Drop**: @dnd-kit

## Estructura de `apps/api/src/`

```
modules/
  account/
  workspace/
  board/
  board-item/
  column/
  auth/
  user/
common/
  guards/        # AuthGuard, RolesGuard, AccountScopeGuard
  decorators/    # @CurrentUser(), @AccountId(), @Roles()
  filters/       # GlobalExceptionFilter
  interceptors/  # ResponseEnvelopeInterceptor
  dto/           # PaginationDto, etc.
```

## Reglas de multi-tenancy — CRÍTICO

- Toda query a la base de datos debe estar scopeada por `accountId`.
- `accountId` siempre viene del JWT, nunca del body o de la URL.
- Usar el decorator `@AccountId()` para extraerlo en controllers.
- El `AccountScopeGuard` verifica que el recurso solicitado pertenezca al account del usuario.
- **Nunca** realizar queries cross-account.

## Convenciones de API

- Base path: `/api/v1/`
- Auth: `Authorization: Bearer <token>`
- Envelope de respuesta:
  ```json
  { "data": ..., "meta": { "timestamp": "..." } }
  ```
- Paginación:
  ```json
  { "data": [...], "meta": { "total": 100, "page": 1, "limit": 20 } }
  ```
- Errores HTTP con formato consistente:
  ```json
  { "statusCode": 404, "message": "Board not found", "error": "Not Found" }
  ```

## Convenciones de código

- TypeScript strict mode, sin `any` — usar `unknown` y narrowing.
- DTOs con `class-validator` y `class-transformer`. Siempre usar `@IsUUID()` para IDs.
- Servicios: primer parámetro siempre `accountId: string` para garantizar tenant scoping.
- No raw queries en Prisma salvo casos justificados con comentario.
- Preferir `const` sobre `let`. Nunca `var`.
- Nombres: `camelCase` variables/funciones, `PascalCase` clases, `SCREAMING_SNAKE_CASE` constantes, `kebab-case` archivos.

## Columnas dinámicas

Los boards tienen columnas con tipos flexibles (`TEXT`, `NUMBER`, `DATE`, `STATUS`, `PERSON`, `BOARD_RELATION`).
- Definición de columna en modelo `Column` (tipo + config JSON).
- Valores en modelo `ColumnValue` (referencia a `columnId` + `boardItemId` + valor JSON).
- Para `BOARD_RELATION`: la columna almacena el `boardId` de destino en su config.

## Convenciones NestJS por módulo

```
<domain>/
  <domain>.module.ts
  <domain>.controller.ts
  <domain>.service.ts
  <domain>.gateway.ts      # Solo si hay eventos Socket.io
  dto/
    create-<domain>.dto.ts
    update-<domain>.dto.ts
    <domain>-response.dto.ts
  entities/
    <domain>.entity.ts     # Tipo TypeScript (no confundir con Prisma model)
```

## Testing

- Tests unitarios con Jest. Mocks solo para servicios externos (Resend, Slack, etc.).
- **Nunca mockear la base de datos** — los tests de integración usan PostgreSQL real.
- E2E con Playwright para flujos críticos (login, crear board, mover item).
- Archivo de test junto al archivo fuente: `board.service.spec.ts`.

## Commits

Usar Conventional Commits:
- `feat(board): add column reordering`
- `fix(auth): handle expired JWT correctly`
- `chore(deps): update prisma to 5.x`
- `refactor(workspace): extract folder logic to service`
- `test(board-item): add integration tests for drag & drop`

Nunca `--no-verify`. Nunca amend en commits publicados.

## Skills disponibles

- `/create-feature <nombre>` — implementar feature end-to-end
- `/api-endpoint <recurso>` — scaffoldear endpoint NestJS
- `/db-migration <nombre>` — crear migración Prisma
- `/commit` — crear commit con Conventional Commits
