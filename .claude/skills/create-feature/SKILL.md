---
name: create-feature
description: Implementar una feature completa end-to-end (schema, backend, frontend). Usar cuando se necesite desarrollar una funcionalidad nueva desde cero.
argument-hint: "[nombre de la feature]"
---

## Feature a implementar: $ARGUMENTS

Sigue este orden. Completa cada paso antes de avanzar al siguiente.

### 1. Schema (si aplica)

- Modifica `packages/database/prisma/schema.prisma`
- Crea la migración con `/db-migration <nombre-descriptivo>`
- Regenera el cliente Prisma

### 2. Tipos compartidos

- Agrega interfaces/types en `packages/types/src/`
- Exporta desde el index del paquete

### 3. Backend — `apps/api/`

- [ ] Service con todas las queries scopeadas por `accountId`
- [ ] DTOs con validación (`class-validator`)
- [ ] Controller con guards y decoradores Swagger
- [ ] Registrar en el módulo
- [ ] Tests unitarios del service (`<dominio>.service.spec.ts`)

Verifica que cada método del service reciba `accountId: string` como primer parámetro.

### 4. Frontend — `apps/web/`

- [ ] Hook TanStack Query en `hooks/use-<feature>.ts`
- [ ] Componentes en `components/<feature>/`
- [ ] Integrar en la página correspondiente
- [ ] Manejar estados: loading, error, empty

### 5. Tests de integración

- Cubre el happy path con PostgreSQL real (no mocks)
- Verifica que un usuario de un account no acceda a datos de otro account

---

**Checklist final:**
- [ ] Toda query tiene `accountId` en el `where`
- [ ] No hay `any` en TypeScript
- [ ] Los DTOs validan todos los campos de entrada
- [ ] Los errores usan `HttpException` de NestJS
- [ ] El comportamiento real-time (si aplica) tiene evento Socket.io
