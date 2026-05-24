# Effica Boards

> Trabaja de forma más inteligente, no más dura.

Plataforma SaaS multi-tenant para gestión de trabajo en equipo. Alternativa liviana a Monday.com con enfoque en simplicidad, rendimiento y diseño API-first.

## Stack

| Capa | Tecnología |
|---|---|
| Monorepo | Turborepo |
| Frontend | Next.js 15 (App Router) |
| Backend | NestJS + Fastify |
| Base de datos | PostgreSQL (Neon) |
| ORM | Prisma |
| Auth | Better Auth |
| Real-time | Socket.io |
| Queue/Workers | BullMQ + Upstash Redis |
| Email | Resend |
| UI | shadcn/ui + Tailwind CSS |
| Estado | TanStack Query + Zustand |
| Drag & Drop | @dnd-kit |

## Arquitectura

```
effica-boards/
├── apps/
│   ├── web/          # Next.js 15 — frontend
│   └── api/          # NestJS — REST API + WebSockets
└── packages/
    ├── database/     # Prisma schema + client
    ├── types/        # Tipos TypeScript compartidos
    └── ui/           # Componentes base (shadcn/ui)
```

## Modelo de dominio

```
Account (tenant)
  └── Workspace (equipo / área)
        └── WorkspaceFolder (agrupador)
              └── Board (flujo de trabajo)
                    ├── Column (definición de campo tipado)
                    └── BoardItem (tarea / registro)
                          └── ColumnValue (valor por columna)
```

## Roles

| Rol | Alcance |
|---|---|
| `SUPER_ADMIN` | Toda la plataforma |
| `ACCOUNT_ADMIN` | Una cuenta específica |
| `MEMBER` | Workspaces asignados dentro de una cuenta |

## Desarrollo local

```bash
# Instalar dependencias
npm install

# Variables de entorno
cp apps/api/.env.example apps/api/.env
cp apps/web/.env.example apps/web/.env

# Levantar base de datos y aplicar migraciones
npx prisma migrate dev --prefix packages/database

# Iniciar todos los servicios
npx turbo dev
```

## Despliegue

| Servicio | Plataforma |
|---|---|
| Frontend (`apps/web`) | Vercel |
| API (`apps/api`) | Railway |
| Base de datos | Neon (PostgreSQL) |
| Redis | Upstash |
| Email | Resend |

## Skills disponibles (Claude Code)

| Comando | Descripción |
|---|---|
| `/create-feature` | Implementar una feature end-to-end |
| `/api-endpoint` | Scaffoldear un nuevo endpoint NestJS |
| `/db-migration` | Crear una migración Prisma |
| `/commit` | Commit con Conventional Commits |
