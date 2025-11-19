# Diseño de Comandos Core

## Resumen Ejecutivo

Este documento propone comandos core que faltan en el proyecto actual, siguiendo las recomendaciones de la metodología workflow-optimizer. Los comandos están diseñados para workflows automatizados, reducir repetición de tareas y optimizar el ciclo de desarrollo.

---

## Comandos Existentes (9 comandos)

### ✅ Ya implementados
1. `explore-plan.md` - Workflow exploración, planificación y ejecución (similar a `/phase-next`)
2. `analyze_bug.md` - Análisis de bugs
3. `create-new-gh-issue.md` - Crear issues de GitHub
4. `implement-feedback.md` - Implementar feedback
5. `rule2hook.md` - Convertir reglas a hooks
6. `start-working-on-issue-new.md` - Iniciar trabajo en issue
7. `update-feedback.md` - Actualizar feedback
8. `worktree.md` - Gestión de git worktree
9. `worktree-tdd.md` - Git worktree con TDD

---

## Comandos Core Faltantes (Recomendados por workflow-optimizer)

### 1. `/phase-next` - Avanzar Siguiente Fase del Roadmap

**Propósito**: Automatizar avance de implementación según roadmap definido en sesión

**Workflow**:
1. Lee `.claude/sessions/context_session_{feature}.md` para identificar fase actual
2. Ejecuta pasos de la siguiente fase
3. Actualiza `.claude/CONTEXT.md` con progreso
4. Genera tests automáticos para la fase
5. Valida completitud con qa-criteria-validator

**Archivo**: `.claude/commands/phase-next.md`

```markdown
---
description: Avanza e implementa la siguiente fase del roadmap del feature actual
---

# /phase-next - Implementar Siguiente Paso Roadmap

## Proceso Automático

1. **Identificar contexto**:
   - Leer `.claude/CONTEXT.md` para ver feature actual
   - Leer `.claude/sessions/context_session_{feature}.md` para roadmap
   - Identificar fase actual y siguiente fase

2. **Ejecutar siguiente fase**:
   - Delegar a planner-nextjs para coordinar implementación
   - planner-nextjs delega a Tool Specialists según necesidad
   - Implementar cambios siguiendo plan de la fase

3. **Validar fase**:
   - Ejecutar tests relacionados con la fase
   - Validar no hay regresiones (run build)
   - Actualizar coverage si aplica

4. **Actualizar documentación**:
   - Marcar fase como completada en archivo de sesión
   - Actualizar `.claude/CONTEXT.md` con nuevo estado
   - Generar commit con cambios de la fase

5. **Reporte**:
   - Mostrar qué se completó
   - Indicar siguiente fase pendiente
   - Listar tests que pasaron/fallaron

## Execution

Implementar fase {N} del roadmap para {feature_name}

## Post-execution

✅ Fase completada
📝 Documentación actualizada
🔄 Siguiente fase: {fase_N+1}
```

**Beneficio**: Reduce de 30-60min a 5-10min el ciclo de implementar una fase completa

---

### 2. `/generate-tests` - Generar Tests Automáticos

**Propósito**: Generar tests comprehensivos para código implementado

**Workflow**:
1. Identifica archivos modificados recientemente
2. Delega a typescript-test-explorer para diseñar test cases
3. Delega a backend-test-architect o frontend-test-engineer para implementar
4. Ejecuta tests y valida coverage

**Archivo**: `.claude/commands/generate-tests.md`

```markdown
---
description: Genera tests automáticos para archivos modificados recientemente
---

# /generate-tests - Generar Tests Automáticos

## Proceso Automático

1. **Identificar archivos a testear**:
   - Git diff para ver archivos modificados últimas 24h
   - Filtrar solo archivos `.ts`, `.tsx` (excluir `.test.ts`)
   - Identificar si son backend (src/) o frontend (app/)

2. **Diseñar test cases**:
   - Delegar a typescript-test-explorer
   - Identificar edge cases y failure modes
   - Definir coverage esperado (≥95% backend, ≥80% frontend)

3. **Generar tests**:
   - Backend: Delegar a backend-test-architect
     - Unit tests con mocks
     - Integration tests si aplica (repositories)
   - Frontend: Delegar a frontend-test-engineer
     - Component tests (React Testing Library)
     - Hook tests con renderHook
     - MSW mocking para servicios

4. **Ejecutar y validar**:
   - Ejecutar `yarn test` para archivos generados
   - Validar coverage con `yarn test:coverage`
   - Reportar resultados

5. **Commit tests**:
   - Crear commit con tests generados
   - Mensaje: "test: add comprehensive tests for {archivos}"

## Execution

Generar tests para archivos modificados recientemente

## Post-execution

✅ Tests generados: {N} archivos
📊 Coverage: {X}%
🔴 Fallos: {Y} (si existen)
```

**Beneficio**: Asegura coverage ≥95% de forma automática, reduce tiempo de 2-3 horas a 15-30 minutos

---

### 3. `/status-complete` - Reporte Estado Completo

**Propósito**: Generar reporte exhaustivo del estado actual del proyecto

**Workflow**:
1. Lee `.claude/CONTEXT.md` y todos los archivos de sesión
2. Ejecuta métricas de código (LOC, coverage, tests)
3. Identifica features en progreso y completadas
4. Genera reporte consolidado

**Archivo**: `.claude/commands/status-complete.md`

```markdown
---
description: Genera reporte exhaustivo del estado actual del proyecto
---

# /status-complete - Reporte Estado Completo Proyecto

## Proceso Automático

1. **Métricas de código**:
   - Total LOC (src/ y app/)
   - Número de archivos TypeScript
   - Número de tests
   - Coverage actual (`yarn test:coverage`)

2. **Features y sesiones**:
   - Listar archivos en `.claude/sessions/`
   - Identificar features completadas vs en progreso
   - Estado de cada feature (fase actual)

3. **Agentes y comandos**:
   - Listar agentes disponibles
   - Listar comandos disponibles
   - Estado de documentación por feature

4. **Git status**:
   - Branch actual
   - Archivos modificados sin commit
   - Último commit

5. **Generar reporte**:
   - Formato Markdown
   - Secciones: Overview, Métricas, Features, Agentes, Git
   - Guardar en `.claude/STATUS_REPORT.md`

## Execution

Generar reporte estado completo del proyecto

## Post-execution

📊 Reporte generado en `.claude/STATUS_REPORT.md`

### Overview
- LOC: {N}
- Tests: {M}
- Coverage: {X}%

### Features
- ✅ Completadas: {lista}
- 🔄 En progreso: {lista}

### Agentes
- Total: {N}
- Manager: planner-nextjs
- Tool Specialists: {N-1}

### Git
- Branch: {branch}
- Último commit: {mensaje}
```

**Beneficio**: Visibilidad completa del proyecto en 30 segundos vs 10-15 min de exploración manual

---

### 4. `/backup-context` - Snapshot Contexto Crítico

**Propósito**: Crear backup de archivos de contexto críticos

**Workflow**:
1. Identifica archivos de contexto (.claude/)
2. Crea snapshot con timestamp
3. Comprime y almacena en `.claude/backups/`

**Archivo**: `.claude/commands/backup-context.md`

```markdown
---
description: Crea backup de archivos de contexto y sesiones (.claude/)
---

# /backup-context - Snapshot Contexto Crítico

## Proceso Automático

1. **Identificar archivos críticos**:
   - `.claude/CONTEXT.md`
   - `.claude/sessions/*.md`
   - `.claude/doc/**/*.md`
   - `.claude/agents/*.md`
   - `.claude/commands/*.md`
   - `.claude/settings.json`

2. **Crear backup**:
   - Timestamp: YYYYMMDD-HHMMSS
   - Directorio: `.claude/backups/backup-{timestamp}/`
   - Copiar todos los archivos críticos

3. **Comprimir** (opcional):
   - `tar -czf .claude/backups/backup-{timestamp}.tar.gz`
   - Eliminar directorio sin comprimir
   - Mantener últimos 10 backups

4. **Reporte**:
   - Archivos respaldados: {N}
   - Tamaño backup: {X} MB
   - Ubicación: `.claude/backups/backup-{timestamp}.tar.gz`

## Execution

Crear backup de contexto y sesiones

## Post-execution

✅ Backup creado: `.claude/backups/backup-{timestamp}.tar.gz`
📦 Archivos: {N}
💾 Tamaño: {X} MB
```

**Beneficio**: Recuperación rápida ante pérdida de contexto o rollback necesario

---

### 5. `/compact-context` - Compactar Contexto

**Propósito**: Reducir tamaño de archivos de sesión que han crecido demasiado

**Workflow**:
1. Identifica sesiones >500 líneas
2. Resume fases completadas
3. Mantiene solo fase actual detallada
4. Mueve logs históricos a archivo separado

**Archivo**: `.claude/commands/compact-context.md`

```markdown
---
description: Compacta archivos de sesión que han crecido demasiado (>500 líneas)
---

# /compact-context - Compactar Contexto de Sesiones

## Proceso Automático

1. **Identificar sesiones grandes**:
   - Buscar archivos en `.claude/sessions/*.md`
   - Filtrar archivos >500 líneas (recomendación: ≤200 líneas)
   - Listar candidatos para compactación

2. **Para cada sesión grande**:
   - Identificar fases completadas
   - Resumir fases completadas en 3-5 líneas por fase
   - Mantener fase actual con todos los detalles
   - Mover logs históricos a `.claude/doc/{feature}/progress.md`

3. **Crear versión compacta**:
   - Nueva estructura:
     ```markdown
     # {Feature} - Context Session

     **Status**: {status}
     **Fase Actual**: {N}

     ## Resumen Fases Completadas
     - ✅ Fase 1: {resumen 1 línea}
     - ✅ Fase 2: {resumen 1 línea}
     ...

     ## Fase Actual (Detalle Completo)
     {todos los detalles de fase actual}

     ## Next Steps
     {próximos pasos}
     ```

4. **Backup antes de compactar**:
   - Guardar versión original en `.claude/backups/session-backups/`
   - Aplicar compactación
   - Validar archivo resultante ≤250 líneas

5. **Reporte**:
   - Sesiones compactadas: {N}
   - Reducción total: {X} líneas → {Y} líneas ({Z}% reducción)

## Execution

Compactar sesiones >500 líneas

## Post-execution

✅ Compactación completada
📉 Reducción: {X} → {Y} líneas ({Z}%)
🗂️ Backups en: `.claude/backups/session-backups/`
```

**Beneficio**: Reducción -70% tokens al cargar contexto de sesión, mejora legibilidad

---

### 6. `/validate-architecture` - Validar Arquitectura Hexagonal

**Propósito**: Validar que el código sigue principios de arquitectura hexagonal

**Archivo**: `.claude/commands/validate-architecture.md`

```markdown
---
description: Valida que el código sigue principios de arquitectura hexagonal y DDD
---

# /validate-architecture - Validar Arquitectura Hexagonal

## Proceso Automático

1. **Validar estructura de directorios**:
   - `src/domain/` - Solo lógica de negocio pura (0 imports externos)
   - `src/application/` - Use cases y ports
   - `src/infrastructure/` - Adapters y repositories

2. **Validar dependencias**:
   - Domain NO debe importar de application/infrastructure
   - Application puede importar de domain
   - Infrastructure puede importar de domain y application
   - Generar grafo de dependencias

3. **Validar DDD patterns**:
   - Entities tienen métodos de negocio
   - Value objects son inmutables
   - Aggregates tienen consistency boundaries
   - Repositories usan interfaces (ports)

4. **Delegar a hexagonal-backend-architect**:
   - Revisar código reciente
   - Identificar violaciones de arquitectura
   - Proponer refactorings si necesario

5. **Reporte**:
   - ✅ Validaciones pasadas
   - ⚠️ Advertencias (violaciones menores)
   - ❌ Errores (violaciones críticas)

## Execution

Validar arquitectura hexagonal del proyecto

## Post-execution

✅ Arquitectura validada
📊 Resultado: {X} validaciones pasadas, {Y} advertencias, {Z} errores
📝 Reporte detallado en: `.claude/doc/architecture-validation.md`
```

**Beneficio**: Asegura adherencia a principios arquitectónicos, previene deuda técnica

---

### 7. `/optimize-tokens` - Analizar y Optimizar Consumo de Tokens

**Propósito**: Medir consumo de tokens actual y proponer optimizaciones

**Archivo**: `.claude/commands/optimize-tokens.md`

```markdown
---
description: Analiza consumo de tokens y propone optimizaciones
---

# /optimize-tokens - Analizar y Optimizar Consumo Tokens

## Proceso Automático

1. **Medir tamaño de contexto actual**:
   - Caracteres en `CLAUDE.md`: {X}
   - Caracteres en `.claude/CONTEXT.md`: {Y}
   - Caracteres en sesiones activas: {Z}
   - Total estimado tokens: {T} (chars / 4)

2. **Identificar oportunidades**:
   - Archivos >1000 líneas (candidatos a compactación)
   - Duplicación de información entre archivos
   - Sesiones que pueden moverse a archive
   - Documentación obsoleta

3. **Proponer optimizaciones**:
   - Compactar sesiones grandes (usar `/compact-context`)
   - Archivar features completadas
   - Actualizar `.claude/CONTEXT.md` (≤200 líneas)
   - Eliminar documentación obsoleta

4. **Estimar impacto**:
   - Reducción estimada tokens: {X}%
   - Sesiones afectadas: {N}
   - Acción recomendada: {lista comandos}

## Execution

Analizar consumo de tokens y proponer optimizaciones

## Post-execution

📊 Análisis completado:
- Tokens actuales (estimado): {T}
- Reducción potencial: -{X}%
- Comandos recomendados:
  1. /compact-context
  2. /backup-context
  3. Actualizar CONTEXT.md
```

**Beneficio**: Identificación proactiva de oportunidades de optimización

---

## Comandos Específicos del Dominio (NextJS + AI Chat)

### 8. `/ai-validate` - Validar Integración OpenAI

**Propósito**: Validar que la integración con OpenAI funciona correctamente

**Archivo**: `.claude/commands/ai-validate.md`

```markdown
---
description: Valida integración con OpenAI API y streaming
---

# /ai-validate - Validar Integración OpenAI

## Proceso Automático

1. **Validar variables de entorno**:
   - OPENAI_API_KEY configurada
   - Conexión a MongoDB (si aplica)

2. **Ejecutar tests de integración**:
   - StreamChatCompletionUseCase
   - OpenAI adapter
   - Vercel stream adapter

3. **Validar streaming**:
   - Iniciar dev server
   - Enviar mensaje de prueba
   - Validar respuesta streaming funciona

4. **Reporte**:
   - ✅ OpenAI API key válida
   - ✅ Streaming funciona
   - ✅ Tool execution funciona (weather)

## Execution

Validar integración OpenAI completa

## Post-execution

✅ OpenAI integración validada
🔑 API key: Configurada
💬 Streaming: Funcional
🛠️ Tools: {N} disponibles
```

---

### 9. `/db-validate` - Validar Integración MongoDB

**Propósito**: Validar conexión y operaciones MongoDB

**Archivo**: `.claude/commands/db-validate.md`

```markdown
---
description: Valida conexión MongoDB y operaciones CRUD
---

# /db-validate - Validar Integración MongoDB

## Proceso Automático

1. **Validar variables**:
   - MONGODB_URL configurada
   - DATABASE_NAME configurada

2. **Test conexión**:
   - MongoDBClient.connect()
   - Health check con ping

3. **Test CRUD**:
   - Crear conversación de prueba
   - Leer conversación
   - Actualizar conversación
   - Eliminar conversación

4. **Validar indexes**:
   - Listar indexes en collection
   - Validar indexes recomendados existen

5. **Reporte**:
   - ✅ Conexión exitosa
   - ✅ CRUD operations funcionan
   - ✅ Indexes optimizados

## Execution

Validar integración MongoDB completa

## Post-execution

✅ MongoDB validada
🔗 Conexión: Exitosa
📊 Latency: {X}ms
📑 Indexes: {N} configurados
```

---

## Prioridades de Implementación

### Fase 1 (Semana 1) - Comandos Críticos
1. `/phase-next` - Mayor impacto en velocidad
2. `/status-complete` - Visibilidad del proyecto
3. `/compact-context` - Optimización inmediata

### Fase 2 (Semana 2) - Comandos de Calidad
4. `/generate-tests` - Asegurar coverage
5. `/validate-architecture` - Prevenir deuda técnica
6. `/backup-context` - Seguridad

### Fase 3 (Semana 3) - Comandos de Optimización
7. `/optimize-tokens` - Análisis continuo
8. `/ai-validate` - Validación específica dominio
9. `/db-validate` - Validación específica dominio

---

## Métricas de Éxito

**Para cada comando**:
- Reducción de tiempo en tarea manual: Target -50-80%
- Reducción de tokens consumidos: Target -30-60%
- Incremento en consistencia: Target 90%+

**Ejemplo /phase-next**:
- Tiempo manual: 30-60 min por fase
- Tiempo con comando: 5-10 min
- Reducción: -80-90%

---

## Plantilla para Nuevos Comandos

```markdown
---
description: {descripción corta del comando}
---

# /{nombre-comando} - {Título}

## Proceso Automático

1. **{Paso 1}**:
   - {Detalles}

2. **{Paso 2}**:
   - {Detalles}

## Execution

{Descripción de lo que ejecuta}

## Post-execution

✅ {Resultado exitoso}
📊 {Métricas}
```

---

## Próximos Pasos

1. ✅ Revisar comandos propuestos con Fran
2. Crear archivos de comandos prioritarios (Fase 1)
3. Probar comandos con features existentes
4. Medir impacto en tokens y tiempo
5. Iterar basado en feedback
6. Implementar Fase 2 y Fase 3

---

**Documento Completo** - Listo para implementación
**Versión**: 1.0
**Fecha**: 2025-11-19
**Comandos Propuestos**: 9 nuevos comandos core
