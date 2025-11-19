# Patrón Manager-Tool Specialists

## Resumen Ejecutivo

El **patrón Manager-Tool Specialists** es una arquitectura multi-agente donde un agente **Manager** coordina múltiples agentes **Tool Specialists** especializados. Este patrón es fundamental en el sistema dot-claude y permite escalabilidad, mantenibilidad y optimización de tokens.

**Origen**: Analizado desde proyecto Cabify demo (Next.js + Vercel AI SDK)  
**Aplicación validada**: Proyecto LCSP RAG Suite (45k LOC, 8 agentes especializados)

---

## Arquitectura

### Diagrama Roles

```
┌──────────────────────────────────────┐
│       MANAGER AGENT (Planner)        │
│                                      │
│  Responsabilidades:                  │
│  • Coordinar workflow general        │
│  • Delegar tareas a specialists      │
│  • Consolidar resultados             │
│  • Tomar decisiones arquitectónicas  │
│  • Mantener visión proyecto          │
└──────────┬───────────────────────────┘
           │
           │ Delegación
           │
    ┌──────┴──────┬─────────┬──────────┬───────────┐
    │             │         │          │           │
┌───▼─────┐  ┌───▼────┐ ┌──▼─────┐ ┌──▼──────┐ ┌─▼────────┐
│ Python  │  │ Domain │ │Testing │ │Database │ │Validator │
│ Expert  │  │ Expert │ │Engineer│ │ Expert  │ │QA Agent  │
└─────────┘  └────────┘ └────────┘ └─────────┘ └──────────┘
Tool Spec.   Tool Spec.  Tool Spec. Tool Spec.  Tool Spec.

NO COMUNICACIÓN DIRECTA ENTRE TOOL SPECIALISTS
```

### Principios Core

#### 1. Manager como Único Punto de Coordinación

**El Manager**:
- ✅ Recibe requests del usuario
- ✅ Analiza qué Tool Specialist(s) necesita
- ✅ Delega tareas específicas
- ✅ Consolida resultados
- ✅ Responde al usuario

**El Manager NO**:
- ❌ Implementa código directamente (delega a Python Expert)
- ❌ Valida compliance (delega a Domain Expert)
- ❌ Escribe tests (delega a Test Engineer)
- ❌ Hace trabajo especializado de Tools

#### 2. Tool Specialists: Especialización Única

**Cada Tool Specialist**:
- ✅ Tiene **UN rol único** claramente definido
- ✅ Es **experto profundo** en su dominio
- ✅ **Solo reporta al Manager**
- ✅ Genera outputs de **alta calidad** en su especialidad

**Tool Specialist NO**:
- ❌ Coordina con otros Tools directamente
- ❌ Toma decisiones arquitectónicas globales
- ❌ Sale de su área de especialización
- ❌ Ejecuta workflows completos (solo su parte)

#### 3. Aislamiento de Contexto

**Crítico para optimización tokens**:

```
Manager Context:
├─ Visión proyecto completa
├─ Roadmap y fases
├─ Decisiones arquitectónicas
└─ Historial coordinación

Python Expert Context:
├─ Solo código Python
├─ Patrones implementación
├─ Librerías/frameworks
└─ Best practices código

Domain Expert Context:
├─ Solo conocimiento dominio
├─ Reglas negocio
├─ Compliance requisitos
└─ Validaciones específicas

❌ NO mezclar contextos entre agentes
```

**Beneficio**: Cada agente carga solo tokens relevantes a su rol.

---

## Roles Detallados

### Manager Agent (Planner)

**Responsabilidad única**: Coordinación y orquestación

**Competencias**:
- Planificación de sprints y fases
- Análisis de requirements
- Delegación inteligente de tareas
- Consolidación de resultados
- Decisiones arquitectónicas high-level
- Gestión de dependencias entre tareas

**Workflow típico**:
1. Usuario solicita feature X
2. Manager analiza: ¿Qué specialists necesito?
3. Delega:
   - Python Expert: Implementa código
   - Domain Expert: Valida reglas negocio
   - Test Engineer: Genera tests
4. Recibe outputs de cada specialist
5. Consolida y verifica coherencia
6. Delega a Validator para QA final
7. Responde al usuario con resultado integrado

**Archivo**: `.claude/agents/planner-{dominio}.md`

**Template estructura**:
```markdown
---
name: planner-{dominio}
description: Manager que coordina desarrollo {DOMINIO}. USE PROACTIVELY para planificación.
tools: search, fileread, filewrite
model: sonnet
---

# Eres Manager del proyecto {NOMBRE}

## Rol: Manager (Coordinator)

Responsabilidad única: Coordinar especialistas y mantener visión proyecto

## Especialización Única

SOLO coordino y planifico. NO hago:
- ❌ Implementar código → Python Expert
- ❌ Validar compliance → Domain Expert
- ❌ Escribir tests → Test Engineer
- ❌ QA final → Validator

SÍ hago:
- ✅ Analizar requirements y dividir en tareas
- ✅ Delegar a specialists apropiados
- ✅ Consolidar outputs y verificar coherencia
- ✅ Tomar decisiones arquitectónicas
- ✅ Actualizar CONTEXT.md y roadmap
```

### Tool Specialist: Python Expert

**Responsabilidad única**: Implementación código Python de alta calidad

**Competencias**:
- Escritura código Python idiomático
- Patrones diseño (Factory, Strategy, Observer, etc.)
- Librerías estándar y frameworks específicos
- Optimización performance
- Type hints y documentación
- Manejo errores y edge cases

**NO hace**:
- ❌ Diseño arquitectónico (es Manager)
- ❌ Tests (es Test Engineer)
- ❌ Validación reglas negocio (es Domain Expert)
- ❌ QA (es Validator)

**Interacción típica**:
```
Manager: "Implementa función validar_licitacion() con estos requisitos..."
Python Expert: [genera código, NO tests, NO validación legal]
Manager: "Recibido. Ahora Domain Expert valide compliance..."
```

**Template estructura**:
```markdown
---
name: python-{dominio}-expert
description: Experto Python para {DOMINIO}. Implementa código de calidad.
tools: fileread, filewrite
model: sonnet
---

# Eres Python Expert del proyecto {NOMBRE}

## Rol: Tool Specialist (Code Implementation)

Responsabilidad única: Implementar código Python de alta calidad

## Especialización Única

SOLO escribo código Python. NO hago:
- ❌ Coordinar proyecto → Manager
- ❌ Validar reglas negocio → Domain Expert
- ❌ Escribir tests → Test Engineer
- ❌ QA código → Validator

SÍ hago:
- ✅ Implementar funciones/clases Python
- ✅ Aplicar patrones diseño apropiados
- ✅ Optimizar performance código
- ✅ Documentar código con docstrings
- ✅ Type hints completos
```

### Tool Specialist: Domain Expert

**Responsabilidad única**: Conocimiento profundo del dominio

**Ejemplos por dominio**:

**Legal/Compliance**:
- Interpretación LCSP/LPAC/LRJSP (España)
- Validación requisitos normativos
- Generación pliegos conforme a ley

**Fintech/Trading**:
- Estrategias trading algorítmico
- Risk management y compliance financiero
- Validación operaciones mercados

**Healthcare/HIPAA**:
- Compliance HIPAA/GDPR
- Protocolos médicos
- Privacidad datos salud

**NO hace**:
- ❌ Implementar código (es Python Expert)
- ❌ Escribir tests (es Test Engineer)
- ❌ Coordinar (es Manager)

**Template estructura**:
```markdown
---
name: {dominio}-expert
description: Experto en {DOMINIO} con conocimiento profundo normativa/reglas.
tools: search, fileread
model: sonnet
---

# Eres Domain Expert {DOMINIO} del proyecto {NOMBRE}

## Rol: Tool Specialist (Domain Knowledge)

Responsabilidad única: Validar compliance y reglas negocio {DOMINIO}

## Especialización Única

SOLO valido cumplimiento normativo {DOMINIO}. NO hago:
- ❌ Implementar código → Python Expert
- ❌ Escribir tests → Test Engineer
- ❌ Coordinar → Manager

SÍ hago:
- ✅ Validar compliance {normativa específica}
- ✅ Interpretar reglas negocio complejas
- ✅ Detectar violaciones normativas
- ✅ Proponer soluciones conformes a ley
```

### Tool Specialist: Test Engineer

**Responsabilidad única**: Testing exhaustivo y coverage

**Competencias**:
- Generación tests unitarios
- Tests integración
- Tests E2E cuando necesario
- Coverage ≥95% objetivo
- Fixtures y mocks
- Tests edge cases

**Framework específicos** (ejemplo Python):
- pytest + pytest-asyncio
- pytest-cov para coverage
- pytest-mock para mocking
- Hypothesis para property-based testing

**NO hace**:
- ❌ Implementar features (es Python Expert)
- ❌ Validar compliance (es Domain Expert)
- ❌ Coordinar (es Manager)

**Template estructura**:
```markdown
---
name: test-engineer-{dominio}
description: Ingeniero testing para {DOMINIO}. Coverage ≥95%.
tools: fileread, filewrite
model: sonnet
---

# Eres Test Engineer del proyecto {NOMBRE}

## Rol: Tool Specialist (Quality Assurance Testing)

Responsabilidad única: Garantizar coverage ≥95% con tests de calidad

## Especialización Única

SOLO escribo tests. NO hago:
- ❌ Implementar features → Python Expert
- ❌ Validar compliance → Domain Expert
- ❌ Coordinar → Manager
- ❌ QA manual → Validator

SÍ hago:
- ✅ Tests unitarios comprehensivos
- ✅ Tests integración críticos
- ✅ Fixtures y mocks realistas
- ✅ Coverage ≥95% verificado
- ✅ Tests edge cases y errores
```

### Tool Specialist: Validator

**Responsabilidad única**: QA final y verificación calidad

**Competencias**:
- Revisión código generado
- Verificación tests pasan
- Validación coverage alcanzado
- Coherencia arquitectónica
- Compliance con estándares proyecto
- Generación reportes QA

**Flujo típico**:
```
1. Manager: "Validator, verifica feature X completada"
2. Validator:
   - Revisa código Python Expert
   - Verifica tests Test Engineer
   - Valida compliance Domain Expert
   - Ejecuta tests y mide coverage
   - Genera reporte QA
3. Validator → Manager: "Feature X aprobada/rechazada porque..."
```

**NO hace**:
- ❌ Implementar código (es Python Expert)
- ❌ Escribir tests (es Test Engineer)
- ❌ Coordinar (es Manager)

**Template estructura**:
```markdown
---
name: validator-{dominio}
description: Validador QA final para {DOMINIO}. Verifica calidad outputs.
tools: fileread, bash
model: sonnet
---

# Eres Validator del proyecto {NOMBRE}

## Rol: Tool Specialist (Quality Assurance Validation)

Responsabilidad única: Verificar calidad final de todos los outputs

## Especialización Única

SOLO valido calidad. NO hago:
- ❌ Implementar código → Python Expert
- ❌ Escribir tests → Test Engineer
- ❌ Validar compliance → Domain Expert
- ❌ Coordinar → Manager

SÍ hago:
- ✅ Revisar código implementado
- ✅ Verificar tests pasan (ejecutar pytest)
- ✅ Medir coverage ≥95%
- ✅ Validar coherencia arquitectónica
- ✅ Generar reportes QA detallados
```

---

## Flujos de Comunicación

### ✅ Flujo Correcto

```
Usuario → Manager → Python Expert → Manager
                 ↓
              Domain Expert → Manager
                 ↓
              Test Engineer → Manager
                 ↓
              Validator → Manager → Usuario
```

**Características**:
- Manager como hub central
- Tool Specialists solo hablan con Manager
- Consolidación en Manager antes responder usuario

### ❌ Flujo Incorrecto (Evitar)

```
Usuario → Python Expert ← → Test Engineer
             ↓              ↓
          Domain Expert ← → Validator
```

**Problemas**:
- Comunicación directa contamina contextos
- Duplicación esfuerzos
- Inconsistencias decisiones
- Pérdida visión global
- Dificultad debugging

---

## Beneficios Patrón

### 1. Optimización Tokens

**Sin patrón**:
```
Prompt monolítico = 5000 tokens
(arquitectura + código + tests + compliance + QA)
× Cada request = 5000 tokens/request
```

**Con patrón**:
```
Manager prompt = 800 tokens
Python Expert = 600 tokens (solo código)
Domain Expert = 400 tokens (solo compliance)
Test Engineer = 500 tokens (solo tests)
Validator = 300 tokens (solo QA)
Total = 2600 tokens (ahorro 48%)
+ Reutilización agentes entre requests
= -70% tokens promedio
```

### 2. Escalabilidad

**Agregar nueva funcionalidad**:
- Sin patrón: Reescribir prompt monolítico completo
- Con patrón: Crear nuevo Tool Specialist específico

**Ejemplo**: Agregar Database Expert para optimización SQL
- NO modifica otros agentes
- Manager aprende a delegarle
- Aislamiento contexto preservado

### 3. Mantenibilidad

**Actualizar conocimiento dominio**:
- Sin patrón: Buscar referencias dispersas en prompts
- Con patrón: Actualizar solo Domain Expert

**Cambiar framework testing**:
- Sin patrón: Reescribir múltiples prompts
- Con patrón: Actualizar solo Test Engineer

### 4. Calidad

**Especialización profunda**:
- Cada agente es experto en SU área
- NO dilución conocimiento en prompts generales
- Best practices específicas por rol

**Validación multi-capa**:
- Python Expert: Código
- Domain Expert: Compliance
- Test Engineer: Coverage
- Validator: Integración QA

---

## Implementación Práctica

### Mínimo Viable (3 agentes)

Para proyectos pequeños (1k-10k LOC):

```
Manager (Planner)
    ↓
    ├─→ Code Specialist (Python/JavaScript/etc)
    └─→ Validator
```

**Cuándo usar**: Proyectos simples, un dominio, 1-2 desarrolladores

### Configuración Estándar (5 agentes)

Para proyectos medianos (10k-100k LOC):

```
Manager (Planner)
    ↓
    ├─→ Python Expert
    ├─→ Domain Expert
    ├─→ Test Engineer
    └─→ Validator
```

**Cuándo usar**: Proyectos con complejidad dominio, múltiples módulos, 3-5 desarrolladores

### Configuración Avanzada (8+ agentes)

Para proyectos grandes (>100k LOC):

```
Manager (Planner)
    ↓
    ├─→ Backend Expert (Python/Java/Go)
    ├─→ Frontend Expert (React/Vue/Angular)
    ├─→ Database Expert (SQL optimization)
    ├─→ Domain Expert (Business rules)
    ├─→ Test Engineer (Unit + Integration)
    ├─→ DevOps Expert (CI/CD, infra)
    ├─→ Security Expert (Vulnerabilities)
    └─→ Validator (QA final)
```

**Cuándo usar**: Proyectos enterprise, arquitecturas distribuidas, equipos grandes

---

## Caso Estudio: LCSP RAG Suite

### Arquitectura Implementada

**8 agentes especializados**:

1. **planner-legal.md** (Manager)
   - Coordina roadmap desarrollo
   - Gestiona fases proyecto
   - Toma decisiones arquitectónicas

2. **python-legal-expert.md** (Tool Specialist)
   - Implementa código Python
   - Patterns: Factory, Strategy, Observer
   - Optimización ChromaDB queries

3. **legal-compliance-expert.md** (Tool Specialist)
   - Experto LCSP/LPAC/LRJSP
   - Validación compliance normativo
   - Interpretación artículos ley

4. **test-engineer-legal.md** (Tool Specialist)
   - Tests pytest exhaustivos
   - Coverage 96% alcanzado
   - Fixtures realistas contratos

5. **validator-legal.md** (Tool Specialist)
   - QA integración módulos
   - Verificación workflows completos
   - Reportes calidad detallados

6. **menor-agent.md** (Especializado)
   - Solo contratos menores Art. 118 LCSP
   - Validación ≤40.000€

7. **abierto-agent.md** (Especializado)
   - Solo procedimiento abierto Art. 156-158
   - Gestión ofertas múltiples

8. **database-expert.md** (Tool Specialist)
   - Optimización ChromaDB
   - Gestión 2,809 documentos vectorizados
   - Queries eficientes

### Métricas Alcanzadas

| Métrica | Antes Patrón | Después Patrón | Mejora |
|---------|--------------|----------------|--------|
| Tokens/sesión | 12,000 | 3,600 | **-70%** |
| Velocidad feature | 3 días | 0.5 días | **+83%** |
| Coverage | 75% | 96% | **+21pp** |
| Bugs producción | 8/mes | 1/mes | **-87%** |
| Tiempo onboarding | 5 días | 1 día | **-80%** |

### Lecciones Aprendidas

**✅ Funciona bien**:
- Especialización estricta (UN rol por agente)
- Manager ligero, delega agresivamente
- Validator ejecuta tests realmente (bash tool)
- Domain Expert con normativa actualizada

**⚠️ Ajustes necesarios**:
- Algunos Tool Specialists originalmente solapaban (refactorizar)
- CONTEXT.md creció >200 líneas (compactar a 180)
- Validator necesita bash tool para ejecutar pytest

**❌ No funcionó**:
- Intentar Tool ↔ Tool comunicación directa (contaminación contexto)
- Manager implementando código (violar principio delegación)
- CONTEXT.md >300 líneas (derrota optimización tokens)

---

## Recursos Adicionales

**Templates completos**: Ver `/templates/.claude/` en este repositorio

**Caso estudio detallado**: [LCSP RAG Suite](../case-studies/lcsp-rag-suite.md)

**Sistema completo**: [Metodología dot-claude](dot-claude-system.md)

---

## Versión Documento

**v1.0** - Noviembre 2025  
Basado en análisis Cabify demo + validación LCSP RAG Suite

## Siguiente Lectura

**[Guía Análisis Proyectos](../guides/project-analysis.md)** - Cómo aplicar este patrón a proyectos nuevos
