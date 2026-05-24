---
name: db-migration
description: Crear una nueva migración Prisma para cambios de schema. Usar cuando se modifique prisma/schema.prisma o se necesite agregar/modificar tablas.
disable-model-invocation: true
allowed-tools: Bash(npx prisma *) Bash(cat *) Bash(ls *)
---

## Instrucciones

Crea una migración Prisma para: $ARGUMENTS

1. **Verifica el schema** — lee `packages/database/prisma/schema.prisma` y confirma que el cambio está aplicado
2. **Revisa migraciones existentes** para no duplicar
   ```bash
   ls packages/database/prisma/migrations/
   ```
3. **Genera la migración** desde `packages/database/`:
   ```bash
   npx prisma migrate dev --name $ARGUMENTS
   ```
4. **Revisa el SQL generado** en `prisma/migrations/<timestamp>_$ARGUMENTS/migration.sql`
5. **Regenera el cliente**:
   ```bash
   npx prisma generate
   ```
6. **Valida que la migración sea segura para producción:**
   - ¿Hay `DROP COLUMN` o `DROP TABLE` con datos?
   - ¿Hay columnas `NOT NULL` sin `DEFAULT` en tablas con registros?
   - Si hay riesgo, propone la migración en dos pasos

Nunca ejecutar `prisma migrate reset` en producción.
