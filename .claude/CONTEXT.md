# Next.js AI Chat - Context

**Última Actualización**: 2025-11-19
**Fase**: Optimización Workflow | Production
**Progreso**: 85% completado

## Stack Técnico

- **Framework**: Next.js 13 (App Router), TypeScript 5.0.4
- **Backend**: OpenAI SDK, MongoDB (Atlas)
- **Frontend**: React 18.2, TanStack Query v5, shadcn/ui (new-york)
- **Testing**: Vitest, React Testing Library, mongodb-memory-server
- **Deploy**: Vercel

## Arquitectura (Hexagonal + DDD)

```
src/
├─ domain/         # Entidades puras (Conversation, Message, ToolInvocation)
├─ application/    # Use cases + Ports (interfaces)
└─ infrastructure/ # Adapters (OpenAI, MongoDB, Vercel Streaming)

app/
├─ features/conversation/   # Feature-based (hooks, services, components)
└─ api/conversations/       # Thin controllers (Next.js routes)
```

**Principio**: Domain no depende de nada. Infrastructure implementa ports de application.

## Decisiones Críticas

1. **MongoDB Repository Pattern**: Embedded messages (NOT referenced) - matches DDD aggregate
2. **Streaming**: Vercel AI SDK Data Stream Protocol v1 - integration con OpenAI
3. **State Management**: React Query (server state) + Context (feature state)
4. **Testing**: Dual approach - mocked unit tests + mongodb-memory-server integration
5. **UI Components**: shadcn/ui blocks (sidebar-07 para conversaciones)

## Estado Actual

### ✅ Completado
- Arquitectura hexagonal backend (domain, application, infrastructure)
- MongoDB persistence con connection pooling y retry logic
- Feature conversation completo (sidebar, list, load, delete)
- Dark/Light mode toggle con next-themes
- Testing strategy (≥95% backend, ≥80% frontend)

### 🔄 En Progreso
- Optimización workflow con patrón Manager-Tool Specialists
- Implementación agente planner-nextjs (Manager)
- Creación comandos core (/phase-next, /generate-tests, /status-complete)
- Compactación archivos de sesión >500 líneas

### 📋 Pendiente
- Validación QA con qa-criteria-validator (Playwright)
- Implementación comandos dominio (/ai-validate, /db-validate)
- Métricas de optimización (baseline tokens, velocidad)
- Deploy producción MongoDB Atlas

## Agentes Especializados (8 + 1 Manager)

**Manager**:
- `planner-nextjs` (NUEVO) - Coordina todos los Tool Specialists

**Tool Specialists**:
- `hexagonal-backend-architect` - Diseño backend hexagonal/DDD
- `frontend-developer` - Arquitectura React feature-based
- `shadcn-ui-architect` - Componentes UI shadcn/ui
- `backend-test-architect` - Testing backend (unit + integration)
- `frontend-test-engineer` - Testing frontend (RTL + Vitest)
- `typescript-test-explorer` - Diseño test cases comprehensivos
- `ui-ux-analyzer` - Validación UI/UX con Playwright
- `qa-criteria-validator` - Acceptance criteria + validación Playwright

## Comandos Disponibles

**Existentes**:
- `/explore-plan` - Workflow exploración → planificación → ejecución
- `/start-working-on-issue-new` - Iniciar trabajo en issue GitHub
- `/worktree` / `/worktree-tdd` - Gestión git worktree

**Nuevos (Propuestos)**:
- `/phase-next` - Avanzar siguiente fase roadmap
- `/generate-tests` - Tests automáticos para código modificado
- `/status-complete` - Reporte estado proyecto completo
- `/compact-context` - Compactar sesiones >500 líneas
- `/backup-context` - Snapshot contexto crítico
- `/validate-architecture` - Validar hexagonal + DDD
- `/optimize-tokens` - Analizar consumo tokens
- `/ai-validate` - Validar OpenAI integration
- `/db-validate` - Validar MongoDB integration

## Features Implementadas

### Chat History (Completado 2025-10-08)
- MongoDB persistence con embedded messages
- Sidebar collapsible con lista de conversaciones
- Filtros por status (Active/Archived/All)
- Delete con confirmación (hard delete)
- Load conversation on click
- Auto-title generation
- **Docs**: `.claude/doc/chat_history/` (7 archivos)

### Dark/Light Mode (Completado)
- next-themes integration
- Toggle en Navbar
- Persistencia en localStorage
- shadcn/ui theming via CSS variables
- **Docs**: `.claude/doc/dark_light_mode/` (3 archivos)

## Next Steps (Top 3 Prioridades)

1. **Implementar planner-nextjs Manager** → Coordinación centralizada
2. **Crear comandos core Fase 1** → /phase-next, /status-complete, /compact-context
3. **Establecer métricas baseline** → Tokens, velocidad, coverage

## Referencias Rápidas

**Arquitectura Backend**: @src/domain, @src/application, @src/infrastructure
**Feature Conversation**: @app/features/conversation
**Config Agentes**: @.claude/agents
**Sesiones Activas**: @.claude/sessions

---

**Target Optimización**: -70% tokens | +83% velocidad | ≥95% coverage
**Metodología**: workflow-optimizer (Manager-Tool Specialists pattern)
