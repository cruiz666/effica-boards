---
name: api-endpoint
description: Scaffoldear un nuevo endpoint REST en NestJS siguiendo las convenciones del proyecto. Usar cuando se necesite crear un nuevo recurso o acción en la API.
argument-hint: "[recurso] [acción]"
---

## Instrucciones

Crea el endpoint para: $ARGUMENTS

Sigue esta estructura en `apps/api/src/modules/<dominio>/`:

### 1. DTO

```typescript
// dto/create-<recurso>.dto.ts
import { IsString, IsUUID } from 'class-validator';

export class Create<Recurso>Dto {
  @IsString()
  name: string;
}
```

### 2. Controller

```typescript
@ApiTags('<recurso>')
@ApiBearerAuth()
@UseGuards(AuthGuard, AccountScopeGuard)
@Controller('<recurso>')
export class <Recurso>Controller {
  constructor(private readonly <recurso>Service: <Recurso>Service) {}

  @Post()
  @ApiOperation({ summary: 'Create <recurso>' })
  create(
    @AccountId() accountId: string,
    @CurrentUser() user: User,
    @Body() dto: Create<Recurso>Dto,
  ) {
    return this.<recurso>Service.create(accountId, user.id, dto);
  }
}
```

### 3. Service

```typescript
async create(accountId: string, userId: string, dto: Create<Recurso>Dto) {
  // accountId siempre como primer parámetro — multi-tenancy obligatorio
  return this.prisma.<recurso>.create({
    data: { ...dto, accountId },
  });
}
```

### Reglas obligatorias

- `accountId` siempre como primer parámetro en el service, extraído del JWT con `@AccountId()`
- Respuesta envuelta automáticamente por `ResponseEnvelopeInterceptor`
- Errores con `throw new NotFoundException('...')` — no devolver null
- Agregar `@ApiOperation` y `@ApiResponse` en cada endpoint
- Registrar controller y service en el módulo correspondiente
