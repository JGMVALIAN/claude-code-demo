# Arquitectura de Agentes Optimizada

## Resumen Ejecutivo

Este documento propone una arquitectura de agentes optimizada para el proyecto Next.js AI Chat, siguiendo el patrón **Manager-Tool Specialists** de la metodología workflow-optimizer, con adaptaciones específicas para la arquitectura hexagonal y el stack tecnológico actual.

**Objetivo**: Reducir consumo de tokens en ~70% y aumentar velocidad de desarrollo en ~83% mediante coordinación centralizada y especialización de agentes.

---

## Arquitectura Propuesta

### Diagrama de Roles

```
┌─────────────────────────────────────────────────┐
│       PLANNER-NEXTJS (Manager Agent)            │
│                                                 │
│  Responsabilidades:                             │
│  • Coordinar workflow de desarrollo completo    │
│  • Delegar a Tool Specialists según fase        │
│  • Consolidar outputs de todos los agentes      │
│  • Tomar decisiones arquitectónicas globales    │
│  • Mantener visión del proyecto completo        │
│  • Actualizar CONTEXT.md tras cada milestone    │
└──────────┬──────────────────────────────────────┘
           │
           │ Delegación
           │
    ┌──────┴──────┬─────────┬──────────┬───────────┬───────────┬───────────┬───────────┐
    │             │         │          │           │           │           │           │
┌───▼─────┐  ┌───▼────┐ ┌──▼─────┐ ┌──▼──────┐ ┌─▼────────┐ ┌─▼────────┐ ┌─▼────────┐ ┌─▼────────┐
│Hexagonal│  │Frontend│ │ShadcnUI│ │Backend  │ │Frontend │ │Typescript│ │UI/UX     │ │QA        │
│Backend  │  │Dev     │ │Architect│ │Test     │ │Test     │ │Test      │ │Analyzer  │ │Criteria  │
│Architect│  │        │ │        │ │Architect│ │Engineer │ │Explorer  │ │          │ │Validator │
└─────────┘  └────────┘ └────────┘ └─────────┘ └──────────┘ └──────────┘ └──────────┘ └──────────┘
Tool Spec.   Tool Spec.  Tool Spec. Tool Spec.  Tool Spec.   Tool Spec.   Tool Spec.   Tool Spec.

NO COMUNICACIÓN DIRECTA ENTRE TOOL SPECIALISTS - TODO A TRAVÉS DEL MANAGER
```

---

## Agentes Especializados (9 agentes)

### 1. Manager Agent: planner-nextjs.md (NUEVO)

**Rol**: Manager único que coordina todos los Tool Specialists

**Responsabilidades**:
- ✅ Analizar requirements y crear plan de implementación
- ✅ Delegar tareas específicas a Tool Specialists apropiados
- ✅ Consolidar outputs de todos los agentes
- ✅ Tomar decisiones arquitectónicas de alto nivel
- ✅ Actualizar `.claude/CONTEXT.md` tras cada milestone
- ✅ Gestionar roadmap y fases del proyecto
- ✅ Coordinar testing end-to-end

**NO hace**:
- ❌ Implementar código directamente → Delega a hexagonal-backend-architect o frontend-developer
- ❌ Diseñar UI → Delega a shadcn-ui-architect
- ❌ Escribir tests → Delega a backend-test-architect o frontend-test-engineer
- ❌ Validar QA → Delega a qa-criteria-validator
- ❌ Analizar UI/UX → Delega a ui-ux-analyzer

**Workflow típico**:
```
1. Usuario solicita feature X
2. Planner analiza: ¿Qué specialists necesito?
3. Delega en paralelo:
   - hexagonal-backend-architect: Diseña arquitectura backend
   - frontend-developer: Diseña arquitectura frontend
   - shadcn-ui-architect: Diseña componentes UI
4. Recibe outputs de cada specialist
5. Consolida en plan maestro en `.claude/sessions/context_session_{feature}.md`
6. Delega implementación:
   - hexagonal-backend-architect: Guía implementación backend
   - frontend-developer: Guía implementación frontend
7. Delega testing:
   - backend-test-architect: Define tests backend
   - frontend-test-engineer: Define tests frontend
8. Delega QA:
   - qa-criteria-validator: Define acceptance criteria
   - ui-ux-analyzer: Valida UI/UX
9. Consolida y responde al usuario con roadmap completo
```

**Archivo**: `.claude/agents/planner-nextjs.md` (a crear)

---

### 2-9. Tool Specialists (Existentes, a Adaptar)

#### 2. hexagonal-backend-architect.md
**Rol**: Tool Specialist - Arquitectura Backend Hexagonal

**Responsabilidad única**: Diseñar arquitectura backend con hexagonal + DDD

**Adaptaciones necesarias**:
```diff
+ ## Especialización Única
+
+ SOLO diseño arquitectura backend hexagonal/DDD. NO hago:
+ - ❌ Implementar código directamente (si fuera necesario, reporto al Manager)
+ - ❌ Escribir tests → backend-test-architect
+ - ❌ Coordinar proyecto → planner-nextjs
+ - ❌ Diseñar frontend → frontend-developer
+
+ SÍ hago:
+ - ✅ Diseñar estructura domain/application/infrastructure
+ - ✅ Definir ports/adapters
+ - ✅ Establecer patrones DDD (aggregates, value objects, entities)
+ - ✅ Proponer dependency injection strategy
+ - ✅ Reportar solo a planner-nextjs con plan detallado
```

#### 3. frontend-developer.md
**Rol**: Tool Specialist - Desarrollo React con Feature-Based Architecture

**Responsabilidad única**: Diseñar arquitectura frontend React/TypeScript

**Adaptaciones necesarias**:
```diff
+ ## Especialización Única
+
+ SOLO diseño arquitectura frontend React. NO hago:
+ - ❌ Diseñar componentes UI → shadcn-ui-architect
+ - ❌ Escribir tests → frontend-test-engineer
+ - ❌ Validar UX → ui-ux-analyzer
+ - ❌ Coordinar → planner-nextjs
+
+ SÍ hago:
+ - ✅ Diseñar feature services, schemas, hooks
+ - ✅ Definir query/mutation hooks con React Query
+ - ✅ Establecer context-based state management
+ - ✅ Reportar solo a planner-nextjs con plan detallado
```

#### 4. shadcn-ui-architect.md
**Rol**: Tool Specialist - Diseño UI con shadcn/ui

**Responsabilidad única**: Seleccionar y diseñar componentes shadcn/ui

**Adaptaciones necesarias**:
```diff
+ ## Especialización Única
+
+ SOLO diseño UI con shadcn/ui. NO hago:
+ - ❌ Implementar lógica de negocio → frontend-developer
+ - ❌ Validar UX → ui-ux-analyzer
+ - ❌ Coordinar → planner-nextjs
+
+ SÍ hago:
+ - ✅ Seleccionar componentes shadcn apropiados
+ - ✅ Diseñar layouts responsive
+ - ✅ Definir accessibility (WCAG 2.1 AA)
+ - ✅ Reportar solo a planner-nextjs con plan detallado
```

#### 5. backend-test-architect.md
**Rol**: Tool Specialist - Estrategia Testing Backend

**Responsabilidad única**: Definir tests backend (unit + integration)

**Adaptaciones necesarias**:
```diff
+ ## Especialización Única
+
+ SOLO defino tests backend. NO hago:
+ - ❌ Implementar código → hexagonal-backend-architect
+ - ❌ Tests frontend → frontend-test-engineer
+ - ❌ Coordinar → planner-nextjs
+
+ SÍ hago:
+ - ✅ Definir unit tests (domain, application, infrastructure)
+ - ✅ Definir integration tests (mongodb-memory-server)
+ - ✅ Establecer coverage targets (≥95%)
+ - ✅ Reportar solo a planner-nextjs con estrategia detallada
```

#### 6. frontend-test-engineer.md
**Rol**: Tool Specialist - Testing Frontend React

**Responsabilidad única**: Definir tests frontend (React Testing Library)

**Adaptaciones necesarias**:
```diff
+ ## Especialización Única
+
+ SOLO defino tests frontend. NO hago:
+ - ❌ Implementar features → frontend-developer
+ - ❌ Tests backend → backend-test-architect
+ - ❌ Coordinar → planner-nextjs
+
+ SÍ hago:
+ - ✅ Definir component tests (React Testing Library)
+ - ✅ Definir hook tests (renderHook)
+ - ✅ Establecer MSW mocking strategy
+ - ✅ Reportar solo a planner-nextjs con estrategia detallada
```

#### 7. typescript-test-explorer.md
**Rol**: Tool Specialist - Diseño Exhaustivo de Test Cases

**Responsabilidad única**: Identificar edge cases y diseñar test cases comprehensivos

**Adaptaciones necesarias**:
```diff
+ ## Especialización Única
+
+ SOLO exploro y diseño test cases. NO hago:
+ - ❌ Implementar tests → backend-test-architect o frontend-test-engineer
+ - ❌ Coordinar → planner-nextjs
+
+ SÍ hago:
+ - ✅ Identificar edge cases y failure modes
+ - ✅ Diseñar test cases comprehensivos
+ - ✅ Proponer property-based testing cuando apropiado
+ - ✅ Reportar solo a planner-nextjs con test cases detallados
```

#### 8. ui-ux-analyzer.md
**Rol**: Tool Specialist - Validación UI/UX con Playwright

**Responsabilidad única**: Validar UI/UX y proporcionar feedback de diseño

**Adaptaciones necesarias**:
```diff
+ ## Especialización Única
+
+ SOLO valido UI/UX. NO hago:
+ - ❌ Implementar componentes → shadcn-ui-architect
+ - ❌ Coordinar → planner-nextjs
+
+ SÍ hago:
+ - ✅ Capturar screenshots con Playwright
+ - ✅ Analizar diseño y proponer mejoras
+ - ✅ Validar responsive design
+ - ✅ Reportar solo a planner-nextjs con feedback detallado
```

#### 9. qa-criteria-validator.md
**Rol**: Tool Specialist - Validación Acceptance Criteria

**Responsabilidad única**: Definir y validar acceptance criteria con Playwright

**Adaptaciones necesarias**:
```diff
+ ## Especialización Única
+
+ SOLO defino/valido acceptance criteria. NO hago:
+ - ❌ Implementar features → hexagonal-backend-architect o frontend-developer
+ - ❌ Coordinar → planner-nextjs
+
+ SÍ hago:
+ - ✅ Definir acceptance criteria (Given/When/Then)
+ - ✅ Validar features con Playwright tests
+ - ✅ Generar reportes de validación
+ - ✅ Reportar solo a planner-nextjs con validation report
```

---

## Flujos de Comunicación

### ✅ Flujo Correcto (Patrón Manager-Tool)

```
Usuario → planner-nextjs → hexagonal-backend-architect → planner-nextjs
                         ↓
                      frontend-developer → planner-nextjs
                         ↓
                      shadcn-ui-architect → planner-nextjs
                         ↓
                      backend-test-architect → planner-nextjs
                         ↓
                      frontend-test-engineer → planner-nextjs
                         ↓
                      qa-criteria-validator → planner-nextjs → Usuario
```

**Características**:
- planner-nextjs como hub central único
- Tool Specialists solo reportan a planner-nextjs
- Consolidación en planner-nextjs antes de responder al usuario
- Delegación explícita desde planner-nextjs

### ❌ Flujo Incorrecto (Evitar)

```
Usuario → hexagonal-backend-architect ← → frontend-developer
             ↓                              ↓
          backend-test-architect ← → frontend-test-engineer
```

**Problemas**:
- Comunicación directa contamina contextos
- Duplicación de esfuerzos
- Inconsistencias en decisiones
- Pérdida de visión global
- Dificultad para consolidar outputs

---

## Configuración de Agentes

### planner-nextjs.md (NUEVO - Manager Agent)

```markdown
---
name: planner-nextjs
description: Manager que coordina desarrollo de proyectos Next.js con arquitectura hexagonal. USE PROACTIVELY para planificación, delegación y consolidación. Coordina todos los Tool Specialists y mantiene visión global del proyecto.
tools: Bash, Glob, Grep, Read, Edit, Write, TodoWrite, Task
model: sonnet
color: purple
---

# Eres Manager del proyecto Next.js AI Chat

## Rol: Manager (Coordinator)

Responsabilidad única: Coordinar Tool Specialists y mantener visión del proyecto

## Especialización Única

SOLO coordino y planifico. NO hago:
- ❌ Implementar código backend → hexagonal-backend-architect
- ❌ Implementar código frontend → frontend-developer
- ❌ Diseñar UI → shadcn-ui-architect
- ❌ Escribir tests backend → backend-test-architect
- ❌ Escribir tests frontend → frontend-test-engineer
- ❌ Explorar test cases → typescript-test-explorer
- ❌ Validar UI/UX → ui-ux-analyzer
- ❌ Validar QA → qa-criteria-validator

SÍ hago:
- ✅ Analizar requirements del usuario
- ✅ Dividir en tareas y delegar a specialists apropiados
- ✅ Consolidar outputs de todos los agentes
- ✅ Tomar decisiones arquitectónicas globales
- ✅ Actualizar `.claude/CONTEXT.md` tras cada milestone
- ✅ Gestionar roadmap en `.claude/sessions/context_session_{feature}.md`
- ✅ Coordinar testing end-to-end
- ✅ Responder al usuario con plan consolidado

## Workflow de Delegación

Cuando recibo un requirement:

1. **Fase Exploración**:
   - Leo `.claude/CONTEXT.md` para contexto global
   - Identifico qué Tool Specialists necesito
   - Creo archivo de sesión `.claude/sessions/context_session_{feature}.md`

2. **Fase Planificación**:
   - Delego en paralelo a specialists necesarios usando Task tool
   - hexagonal-backend-architect: Arquitectura backend
   - frontend-developer: Arquitectura frontend
   - shadcn-ui-architect: Componentes UI
   - backend-test-architect: Estrategia testing backend
   - frontend-test-engineer: Estrategia testing frontend

3. **Fase Consolidación**:
   - Leo todos los planes generados en `.claude/doc/{feature}/`
   - Consolido en plan maestro coherente
   - Identifico dependencias y orden de implementación
   - Actualizo archivo de sesión con plan final

4. **Fase Implementación** (guiada):
   - Coordino ejecución siguiendo el plan
   - Delego implementación específica a specialists
   - Monitoreo progreso y ajusto según necesario

5. **Fase Testing**:
   - Delego a backend-test-architect y frontend-test-engineer
   - Consolido estrategia de testing completa

6. **Fase QA**:
   - Delego a qa-criteria-validator para validación final
   - Delego a ui-ux-analyzer para feedback UI/UX
   - Consolido feedback y ajustes necesarios

7. **Fase Finalización**:
   - Actualizo `.claude/CONTEXT.md` con estado final
   - Genero reporte de completitud
   - Respondo al usuario con resumen ejecutivo

## Output Format

Siempre genero:
1. Plan maestro en `.claude/sessions/context_session_{feature}.md`
2. Reporte de delegación (qué specialist hace qué)
3. Timeline estimado de implementación
4. Lista de dependencias entre tareas

## Rules

- NUNCA implemento directamente, siempre delego
- SIEMPRE uso Task tool para invocar specialists
- SIEMPRE consolido outputs antes de responder al usuario
- SIEMPRE actualizo `.claude/CONTEXT.md` tras milestones
- SIEMPRE mantengo visión global del proyecto
```

---

## Beneficios de la Arquitectura Propuesta

### 1. Optimización de Tokens

**Sin Manager (actual)**:
```
Cada agente carga contexto completo:
- hexagonal-backend-architect: 7,811 caracteres + contexto completo
- frontend-developer: 7,848 caracteres + contexto completo
- shadcn-ui-architect: 6,610 caracteres + contexto completo
= ~22,269 caracteres base + contexto duplicado × 8 agentes
≈ Alto consumo de tokens por duplicación de contexto
```

**Con Manager (propuesto)**:
```
planner-nextjs: Carga contexto global (CONTEXT.md ≤200 líneas)
Tool Specialists: Solo cargan contexto específico de su dominio
hexagonal-backend-architect: Solo arquitectura backend
frontend-developer: Solo arquitectura frontend
shadcn-ui-architect: Solo componentes UI
= Reducción estimada -60-70% tokens
```

### 2. Coordinación Centralizada

**Antes**: Agentes operan independientemente, posible duplicación de esfuerzos

**Ahora**:
- planner-nextjs delega tareas específicas
- Tool Specialists reportan solo a planner-nextjs
- Consolidación garantiza coherencia
- Visión global mantenida en todo momento

### 3. Escalabilidad

**Agregar nueva funcionalidad**:
- Sin Manager: Actualizar múltiples agentes
- Con Manager: Crear nuevo Tool Specialist, planner-nextjs aprende a delegarle

**Ejemplo**: Agregar DevOps Specialist para CI/CD
- NO modifica otros agentes
- planner-nextjs lo incluye en workflow
- Aislamiento de contexto preservado

### 4. Mantenibilidad

**Actualizar conocimiento de dominio**:
- Sin Manager: Buscar referencias en múltiples agentes
- Con Manager: Actualizar solo Tool Specialist específico

**Cambiar framework**:
- Sin Manager: Reescribir múltiples agentes
- Con Manager: Actualizar solo Tool Specialist afectado

---

## Plan de Implementación

### Fase 1: Crear Manager Agent (1-2 horas)

1. **Crear archivo** `.claude/agents/planner-nextjs.md`
2. **Definir responsabilidades** según template arriba
3. **Establecer workflow** de delegación con Task tool
4. **Probar** con feature pequeña para validar flujo

### Fase 2: Adaptar Tool Specialists (2-3 horas)

1. **Añadir sección** "Especialización Única" a cada agente existente
2. **Clarificar** qué NO hacen (delegan al Manager)
3. **Definir output format** estándar para reportar a planner-nextjs
4. **Actualizar** reglas para reportar solo al Manager

### Fase 3: Validar con Feature Completo (1 semana)

1. **Seleccionar feature** de complejidad media
2. **Ejecutar workflow** completo con Manager
3. **Medir tokens** consumidos vs baseline actual
4. **Ajustar** basado en feedback

### Fase 4: Documentar y Estandarizar (Continuo)

1. **Crear CONTEXT.md** compacto (siguiente fase)
2. **Añadir comandos core** (siguiente fase)
3. **Establecer métricas** de optimización
4. **Iterar** basado en resultados

---

## Métricas de Éxito

### KPIs a Medir

**Baseline (Actual)**:
- Tokens/sesión promedio: [A medir]
- Tiempo implementación feature: [A medir]
- Inconsistencias entre agentes: [A medir]

**Target (Con Manager)**:
- Tokens/sesión: -60-70% vs baseline
- Tiempo implementación: +50-83% velocidad
- Inconsistencias: Reducción 90% (consolidación garantizada)

### Validación

Después de 3 features implementados con Manager:
1. Comparar tokens consumidos
2. Comparar tiempo de implementación
3. Evaluar coherencia de outputs
4. Ajustar arquitectura según resultados

---

## Próximos Pasos

1. ✅ Revisar arquitectura propuesta con Fran
2. Crear archivo `planner-nextjs.md`
3. Adaptar Tool Specialists existentes
4. Crear CONTEXT.md compacto
5. Definir comandos core faltantes
6. Validar con feature de prueba
7. Medir métricas y comparar con baseline
8. Iterar basado en resultados

---

**Documento Completo** - Listo para implementación
**Versión**: 1.0
**Fecha**: 2025-11-19
**Autor**: Workflow Optimization Analysis
