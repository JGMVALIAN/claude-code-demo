# Sistema dot-claude - Metodología Completa

## Resumen Ejecutivo

El **sistema dot-claude** es una metodología de desarrollo optimizada para Claude Code que reduce el consumo de tokens en un 70% y aumenta la velocidad de desarrollo en un 83% mediante arquitecturas multi-agente especializadas y carga selectiva de contexto.

**Validación producción**: Proyecto LCSP RAG Suite (45k LOC, 96% coverage)

---

## Filosofía del Sistema

El enfoque se basa en **cinco pilares fundamentales**:

### 1. Carga Selectiva de Contexto

**Problema**: Cargar archivos completos del proyecto consume tokens innecesariamente y satura la ventana de contexto.

**Solución**: 
- Documentación compacta (CONTEXT.md ≤200 líneas)
- Referencias modulares con `@file:línea` específicas
- Agentes cargan solo información necesaria para su rol

**Impacto medido**: -70% reducción tokens vs enfoque ad-hoc

### 2. Agentes Especializados

**Problema**: Prompts monolíticos mezclan responsabilidades y generan respuestas inconsistentes.

**Solución**:
- 5-8 agentes con roles únicos (Planner, Specialists, Validators)
- Aislamiento de contexto entre agentes
- Invocación automática según tarea

**Estructura agente**:
```markdown
---
name: nombre-agente
description: Rol específico. USE PROACTIVELY cuando {condición}.
tools: search, fileread, filewrite
model: sonnet
---

# Eres {Rol} del proyecto {NOMBRE}

## Rol: {Manager/Tool Specialist/Validator}

Responsabilidad única: {1 línea clara}

## Especialización Única

SOLO {área específica}. NO hago:
- ❌ {Tarea 1} → Delegar a {AgentX}
- ❌ {Tarea 2} → Delegar a {AgentY}

SÍ hago:
- ✅ {Capacidad core 1}
- ✅ {Capacidad core 2}
```

### 3. Comandos Modulares Reutilizables

**Problema**: Workflows comunes requieren repetir secuencias de instrucciones.

**Solución**:
- Comandos Markdown en `.claude/commands/`
- Shortcuts para operaciones frecuentes
- Workflows automatizados end-to-end

**Ejemplo comando**:
```markdown
# /phase-next - Implementar siguiente paso roadmap

## Proceso Automático
1. Leer roadmap.md identificar fase actual
2. Ejecutar pasos fase siguiente
3. Actualizar CONTEXT.md con progreso
4. Generar tests automáticos
5. Validar con validator-agent

## Execution
Implementar paso X del roadmap...

## Post-execution
Actualizar documentación y métricas.
```

### 4. Patrón Manager-Tool Specialists

**Arquitectura**:

```
┌─────────────────┐
│  Manager Agent  │  ← Coordina, planifica, delega
│   (Planner)     │
└────────┬────────┘
         │
    ┌────┴────┬─────────┬──────────┐
    │         │         │          │
┌───▼────┐ ┌─▼─────┐ ┌─▼──────┐ ┌─▼────────┐
│Python  │ │Domain │ │Testing │ │Validator │
│Expert  │ │Expert │ │Engineer│ │QA Agent  │
└────────┘ └───────┘ └────────┘ └──────────┘
   Tool       Tool      Tool        Tool
Specialist Specialist Specialist  Specialist
```

**Flujos comunicación**:
- Manager → Tool Specialist: Asigna tareas específicas
- Tool Specialist → Manager: Reporta resultados
- Validator → Todos: Verifica calidad outputs
- ❌ Tool ↔ Tool: **NO comunicación directa** (evita contaminación contexto)

### 5. Documentación Auto-actualizable

**CONTEXT.md vivo**:
- Máximo 200 líneas (síntesis extrema)
- Actualización tras cada milestone
- Incluye: fase actual, arquitectura, decisiones críticas, next steps

**Estructura estándar**:
```markdown
# Proyecto {NOMBRE}

**Fecha**: {última actualización}
**Fase**: {actual} / {total}
**Progreso**: {X}%

## Arquitectura
{Diagrama compacto módulos}

## Decisiones Críticas
{Solo breaking decisions, ≤5 items}

## Estado Actual
{Qué funciona, qué falta, ≤10 líneas}

## Next Steps
{Top 3-5 prioridades}
```

---

## Workflow Estructurado (Ciclos 35-40 min)

### Distribución Temporal

**0-5 min**: Marco y contexto
- Leer CONTEXT.md + roadmap
- Identificar objetivo sesión
- Seleccionar agente apropiado

**5-20 min**: Construcción
- Implementar feature/fix
- Generar código modular
- Solicitar diffs compactos (no código completo)

**20-30 min**: Testing
- Tests automáticos con test-engineer
- Coverage ≥95% objetivo
- Validación edge cases

**30-35 min**: Revisión
- Validator agent verifica calidad
- Actualizar CONTEXT.md
- Commit + documentación

**35-40 min**: Planificación siguiente
- Definir siguiente paso roadmap
- Preparar contexto próxima sesión

### Técnicas Optimización Tokens

**Uso diffs en lugar de archivos completos**:
```
❌ NO: "Aquí está todo el archivo actualizado (500 líneas)"
✅ SÍ: "Cambios aplicados:
  - Línea 45: Añadido validación
  - Línea 120: Refactor función X"
```

**Referencias modulares**:
```
❌ NO: Cargar 1.txt-7.md completos
✅ SÍ: Consultar @3.md:líneas-450-480 solo cuando necesario
```

**Compactación periódica**:
- Tras cada milestone mayor
- Comando `/compact` resume sesiones anteriores
- Logs históricos (progress.md) mantienen trazabilidad

---

## Implementación Práctica

### Setup Inicial (15 min)

1. **Crear estructura**:
```bash
mkdir -p .claude/{agents,commands}
touch .claude/CONTEXT.md
```

2. **Definir agentes base** (mínimo 5):
   - `planner-{dominio}.md` - Manager
   - `python-{dominio}-expert.md` - Code specialist
   - `{dominio}-expert.md` - Domain specialist  
   - `test-engineer-{dominio}.md` - Testing specialist
   - `validator-{dominio}.md` - QA specialist

3. **Crear comandos core**:
   - `/phase-next` - Avanzar roadmap
   - `/{dominio}-validate` - Validación específica
   - `/backup-{recurso}` - Snapshots
   - `/generate-tests` - Tests automáticos
   - `/status-complete` - Reporte estado

4. **Inicializar CONTEXT.md**:
   - Resumen arquitectura (≤50 líneas)
   - Estado inicial proyecto
   - Roadmap fases

### Adaptación a Proyectos Existentes

**Para proyectos legacy (>10k LOC)**:

1. **Análisis inicial** (2-3 horas):
   - Mapear arquitectura actual
   - Identificar módulos críticos
   - Medir coverage baseline

2. **Creación agentes especializados** (1 día):
   - Agente por módulo principal
   - Especialización según dominio
   - Definir responsabilidades únicas

3. **Migración incremental** (1-2 semanas):
   - Nuevo desarrollo usa sistema dot-claude
   - Refactoring gradual código legacy
   - Actualización tests a ≥95% coverage

4. **Validación métricas** (continuo):
   - Medir reducción tokens
   - Tracking velocidad desarrollo
   - Monitorizar calidad código

---

## Métricas Validadas

### Proyecto LCSP RAG Suite (Caso Real)

**Contexto**:
- Dominio: Legal/Compliance (licitaciones públicas España)
- Stack: Python 3.11, FastAPI, ChromaDB
- Alcance: 45,000 LOC, 8 módulos, 175 tests

**Resultados sistema dot-claude**:

| Métrica | Antes | Después | Mejora |
|---------|-------|---------|--------|
| Tokens/sesión | 100% baseline | 30% | **-70%** |
| Velocidad desarrollo | 1x | 1.83x | **+83%** |
| Test coverage | 75% | 96% | **+21pp** |
| Tiempo setup nuevo feature | 3 días | 0.5 días | **-83%** |
| Bugs producción | 8/mes | 1/mes | **-87%** |

**Arquitectura agentes implementada**:
- `planner-legal.md` - Coordina roadmap legal
- `python-legal-expert.md` - Código Python especializado
- `legal-compliance-expert.md` - Validación LCSP/LPAC/LRJSP
- `test-engineer-legal.md` - Tests coverage 98%
- `validator-legal.md` - QA compliance normativo

**Comandos críticos**:
- `/lcsp-validate` - Batería tests legales automáticos
- `/generate-pliego` - Workflow generación pliegos
- `/backup-chromadb` - Snapshot 2,809 documentos vectorizados
- `/phase-next` - Implementación automática roadmap

---

## Escalabilidad Validada

**Proyectos pequeños** (1k-10k LOC):
- 3 agentes suficientes (Planner + Specialist + Validator)
- Setup 30 minutos
- ROI inmediato

**Proyectos medianos** (10k-100k LOC):
- 5-8 agentes recomendados
- Setup 1 día
- ROI semana 2

**Proyectos grandes** (100k-1M LOC):
- 10-15 agentes especializados
- Setup 3-5 días
- ROI mes 1

**Enterprise** (>1M LOC):
- Arquitectura jerárquica agentes
- Teams agentes por módulo
- Validado hasta **20M LOC** (caso Reddit usuario)

---

## Antipatrones a Evitar

### ❌ Agentes con Responsabilidades Solapadas

**Problema**: Dos agentes hacen tareas similares, generan inconsistencias.

**Ejemplo malo**:
- `backend-expert.md` escribe tests
- `test-engineer.md` escribe tests

**Solución**: Especialización estricta con sección "NO hago" explícita.

### ❌ CONTEXT.md >200 Líneas

**Problema**: Derrota propósito optimización tokens.

**Solución**: Síntesis extrema, delegar detalles a archivos específicos.

### ❌ Comandos Monolíticos

**Problema**: Comando hace 10 tareas, dificulta reutilización.

**Solución**: Comandos atómicos componibles (`/test` + `/deploy` vs `/test-and-deploy`).

### ❌ Falta Validación Outputs

**Problema**: Confiar ciegamente en outputs agentes.

**Solución**: Validator agent siempre revisa trabajo Tool Specialists.

---

## Recursos Adicionales

**Repositorio referencia**: https://github.com/flype/dot-claude  
**Video explicativo**: https://youtu.be/wi6Sz8Z94cE

**Templates**:
- Ver `/templates` de este repositorio
- Ejemplos por dominio: Legal, Fintech, Healthcare, E-commerce

**Comunidad**:
- Reddit: r/ClaudeAI
- GitHub Discussions en este repo

---

## Versión Documento

**v1.0** - Noviembre 2025  
Basado en implementación validada proyecto LCSP RAG Suite (45k LOC, 96% coverage)

## Próximos Pasos

1. **[Leer guía análisis proyectos](../guides/project-analysis.md)** - Cómo analizar proyecto nuevo
2. **[Estudiar caso LCSP](../case-studies/lcsp-rag-suite.md)** - Implementación real detallada
3. **[Explorar templates](../../templates/)** - Agentes y comandos reutilizables
4. **[Ver patrón Manager-Tool](manager-tool-pattern.md)** - Arquitectura multi-agente profunda
