---
description: Analiza el proyecto y genera automáticamente estructura workflow-optimizer completa (agentes, comandos, CONTEXT.md)
---

# /bootstrap-workflow - Bootstrap Workflow Optimizer Automático

## Descripción

Este comando es tu **comodín universal** para implementar la metodología workflow-optimizer en **cualquier proyecto**, ya sea nuevo o existente, independientemente del dominio o stack tecnológico.

**Tiempo estimado**: 5-10 minutos
**Resultado**: Estructura completa `.claude/` lista para usar

---

## Uso

```bash
# Proyecto nuevo o existente
claude /bootstrap-workflow

# Proyecto existente con código
claude /bootstrap-workflow --existing-project

# Forzar score específico (override análisis automático)
claude /bootstrap-workflow --force-score=12

# Modo verbose (mostrar análisis detallado)
claude /bootstrap-workflow --verbose
```

---

## Proceso Automático

### Fase 1: Análisis del Proyecto (2 min)

**1.1 Detectar Stack Tecnológico**

Analiza archivos del proyecto:
- `package.json` → Node.js, frameworks (Next.js, Express, React, etc.)
- `pyproject.toml` / `requirements.txt` → Python, frameworks (Django, FastAPI, Flask)
- `pom.xml` / `build.gradle` → Java, frameworks (Spring Boot)
- `go.mod` → Go
- `Cargo.toml` → Rust

**1.2 Calcular Métricas**

```bash
# Lines of Code (LOC)
find . -name "*.ts" -o -name "*.tsx" -o -name "*.py" -o -name "*.java" | xargs wc -l | tail -1

# Número de módulos/packages
ls -d src/*/ app/*/ | wc -l

# Coverage (si existe)
yarn test:coverage 2>/dev/null || pytest --cov 2>/dev/null

# Team size (intentar detectar de git)
git log --format='%ae' | sort -u | wc -l
```

**1.3 Detectar Dominio**

Heurísticas basadas en:
- Palabras clave en README.md
- Nombres de archivos/carpetas
- Dependencies en package.json
- Preguntar al usuario si no se puede determinar

Dominios detectables:
- E-commerce: `cart`, `product`, `order`, `payment`, `inventory`
- Fintech: `transaction`, `account`, `balance`, `compliance`, `regulatory`
- Healthcare: `patient`, `medical`, `hipaa`, `phi`, `ehr`, `emr`
- SaaS: `tenant`, `subscription`, `billing`, `onboarding`
- DevOps: `terraform`, `kubernetes`, `ci-cd`, `infrastructure`

**1.4 Calcular Score de Complejidad**

```
Score = LOC_points + Modules_points + Domain_points + Testing_points + Team_points

Donde:
- LOC_points: 1 (<10k), 2 (10k-100k), 3 (>100k)
- Modules_points: 1 (1-3), 2 (4-8), 3 (9+)
- Domain_points: 1 (Simple), 2 (Negocio), 3 (Regulado)
- Testing_points: 1 (<70%), 2 (70-90%), 3 (>90%)
- Team_points: 1 (1-2), 2 (3-5), 3 (6+)

Resultado:
- 5-7 puntos  → Simple   → 3 agentes
- 8-11 puntos → Medio    → 5 agentes
- 12-15 puntos → Complejo → 8 agentes
```

---

### Fase 2: Generación de Agentes (3 min)

**2.1 Seleccionar Template Según Score**

**Simple (3 agentes)**:
```
planner-{dominio}.md
code-specialist-{stack}.md
qa-validator-{dominio}.md
```

**Medio (5 agentes)**:
```
planner-{dominio}.md
backend-architect-{stack}.md
frontend-developer-{framework}.md
test-engineer-{stack}.md
qa-validator-{dominio}.md
```

**Complejo (8 agentes)**:
```
planner-{dominio}.md
backend-architect-{stack}.md
frontend-developer-{framework}.md
ui-ux-architect-{design-system}.md
backend-test-engineer.md
frontend-test-engineer.md
domain-expert-{dominio}.md
qa-validator-{dominio}.md
```

**2.2 Generar Archivos de Agentes**

Para cada agente:
1. Copiar template apropiado
2. Reemplazar variables:
   - `{DOMINIO}` → dominio detectado
   - `{STACK}` → stack detectado
   - `{FRAMEWORK}` → framework detectado
   - `{NOMBRE_PROYECTO}` → nombre del proyecto
3. Guardar en `.claude/agents/{nombre-agente}.md`

**2.3 Personalizar planner (Manager)**

El planner debe conocer todos los specialists generados:

```markdown
## Tool Specialists Disponibles

{LISTA_GENERADA_DINAMICAMENTE}

Ejemplo para proyecto Node.js + React (5 agentes):
- backend-architect-nodejs → Arquitectura backend Node.js/Express
- frontend-developer-react → Desarrollo React/TypeScript
- test-engineer-nodejs → Testing Jest/Vitest
- qa-validator-{dominio} → QA y validación final
```

---

### Fase 3: Generación de Comandos (2 min)

**3.1 Comandos Core (Siempre se generan)**

9 comandos universales:
1. `phase-next.md`
2. `generate-tests.md`
3. `status-complete.md`
4. `compact-context.md`
5. `backup-context.md`
6. `validate-architecture.md`
7. `optimize-tokens.md`
8. `ai-validate.md` (si usa AI/LLM)
9. `db-validate.md` (si usa database)

**3.2 Comandos Específicos Dominio**

Según dominio detectado:

**E-commerce**:
- `validate-inventory.md`
- `process-order.md`
- `payment-test.md`

**Fintech**:
- `compliance-check.md`
- `security-audit.md`
- `transaction-test.md`

**Healthcare**:
- `hipaa-validate.md`
- `phi-audit.md`
- `encryption-check.md`

**SaaS**:
- `tenant-isolate.md`
- `billing-test.md`
- `onboarding-flow.md`

**DevOps**:
- `infra-validate.md`
- `deploy-staging.md`
- `rollback.md`

**3.3 Adaptar Comandos al Stack**

Ejemplo `/generate-tests`:
- Node.js → Usa Jest/Vitest
- Python → Usa pytest
- Java → Usa JUnit
- Go → Usa testing package

---

### Fase 4: Generación de CONTEXT.md (1 min)

**4.1 Template CONTEXT.md**

```markdown
# {NOMBRE_PROYECTO} - Context

**Última Actualización**: {FECHA_ACTUAL}
**Fase**: Setup Inicial
**Progreso**: 0%

## Stack Técnico

{STACK_DETECTADO}

## Arquitectura

{ARQUITECTURA_DETECTADA_O_PENDIENTE}

## Decisiones Críticas

(A completar según desarrollo)

## Estado Actual

### ✅ Completado
- Estructura workflow-optimizer implementada
- {N} agentes especializados configurados
- {M} comandos core disponibles

### 🔄 En Progreso
(Ninguno - proyecto recién bootstrapped)

### 📋 Pendiente
- Implementar primera feature
- Establecer baseline métricas
- Validar workflow con feature pequeña

## Agentes Especializados ({N})

**Manager**: planner-{dominio}

**Tool Specialists**:
{LISTA_SPECIALISTS_GENERADOS}

## Comandos Disponibles ({M} core + {K} dominio)

**Core**: {LISTA_COMANDOS_CORE}
**Dominio**: {LISTA_COMANDOS_DOMINIO}

## Features Implementadas

(Ninguna - proyecto recién iniciado)

## Next Steps (Top 3 Prioridades)

1. **Implementar feature inicial** → Usar planner-{dominio}
2. **Ejecutar /generate-tests** → Establecer coverage baseline
3. **Ejecutar /status-complete** → Validar estructura completa

## Referencias Rápidas

**Arquitectura**: Ver archivos en src/ app/
**Agentes**: .claude/agents/
**Comandos**: .claude/commands/
**Sesiones**: .claude/sessions/

---

**Target Optimización**: -70% tokens | +83% velocidad | ≥95% coverage
**Metodología**: workflow-optimizer (Manager-Tool Specialists pattern)
```

---

### Fase 5: Crear Estructura Completa (1 min)

**5.1 Directorios**

```bash
mkdir -p .claude/{agents,commands,sessions,doc}
```

**5.2 Archivos Adicionales**

`.claude/sessions/README.md`:
```markdown
# Sessions

Archivos de contexto por feature.

Formato: `context_session_{feature_name}.md`

Cada sesión debe:
- Mantenerse ≤200 líneas (usar /compact-context si crece)
- Documentar fases del roadmap
- Listar decisiones tomadas
- Incluir next steps
```

`.claude/.gitignore`:
```
# Ignorar sesiones locales (opcionales)
sessions/*_local.md

# Backups (generados por /backup-context)
backups/
```

---

## Output del Comando

### Output Completo (Ejemplo)

```
🔍 Analizando proyecto...

📊 Análisis Completado:

Nombre: mi-app-saas
Ruta: /home/user/mi-app-saas

Stack Detectado:
- Backend: Node.js 18.17, Express 4.18
- Frontend: React 18.2, TypeScript 5.0
- Database: PostgreSQL 15
- Testing: Jest 29, React Testing Library

Métricas:
- LOC: 15,234 líneas (Media: 2pts)
- Módulos: 6 módulos (Media: 2pts)
- Dominio: SaaS multi-tenant (Alta: 3pts)
- Testing: 85% coverage actual (Alta: 3pts)
- Team: 4 contributors (Media: 2pts)

📈 Score Complejidad: 12 puntos
📊 Clasificación: Proyecto Complejo
👥 Agentes Recomendados: 8

✅ Generando Estructura...

Agentes creados (8):
  ✓ planner-saas.md (Manager)
  ✓ backend-architect-nodejs.md
  ✓ frontend-developer-react.md
  ✓ database-specialist-postgres.md
  ✓ backend-test-engineer.md
  ✓ frontend-test-engineer.md
  ✓ security-specialist.md
  ✓ qa-validator-saas.md

Comandos creados (12):
  Core (9):
    ✓ phase-next.md
    ✓ generate-tests.md
    ✓ status-complete.md
    ✓ compact-context.md
    ✓ backup-context.md
    ✓ validate-architecture.md
    ✓ optimize-tokens.md
    ✓ ai-validate.md
    ✓ db-validate.md

  Dominio SaaS (3):
    ✓ tenant-isolate.md
    ✓ billing-test.md
    ✓ onboarding-flow.md

Documentación:
  ✓ .claude/CONTEXT.md (proyecto inicial)
  ✓ .claude/sessions/README.md
  ✓ .claude/.gitignore

📁 Estructura Completa:
.claude/
├── CONTEXT.md (120 líneas)
├── agents/ (8 archivos)
├── commands/ (12 archivos)
├── sessions/ (README.md)
└── doc/ (vacío - se llenará con features)

✅ Bootstrap Completado!

📋 Próximos Pasos:

1. Revisar agentes generados:
   ls .claude/agents/

2. Personalizar CONTEXT.md si necesario:
   vim .claude/CONTEXT.md

3. Probar con feature pequeña:
   claude "Implementar endpoint GET /health para health checks"

4. Validar estructura:
   claude /status-complete

5. Establecer baseline métricas:
   - Medir tokens/sesión actual
   - Medir tiempo promedio feature
   - Ejecutar yarn test:coverage

🎯 Objetivos Optimización:
- Tokens: -60-70% por sesión
- Velocidad: +50-83% desarrollo
- Coverage: ≥95%

🚀 ¡Listo para empezar!
```

---

## Execution (Implementación del Comando)

Cuando usuario ejecuta `/bootstrap-workflow`:

1. **Analizar proyecto**:
   - Detectar stack (package.json, pyproject.toml, etc.)
   - Calcular LOC (wc -l)
   - Contar módulos (ls -d)
   - Detectar dominio (keywords en README, archivos)
   - Calcular score (suma métricas)

2. **Seleccionar configuración**:
   - Score 5-7 → 3 agentes
   - Score 8-11 → 5 agentes
   - Score 12-15 → 8 agentes

3. **Generar archivos**:
   - Para cada agente: copiar template, reemplazar variables, guardar
   - Para cada comando: copiar template, adaptar a stack, guardar
   - Generar CONTEXT.md con datos proyecto
   - Crear README.md en sessions/

4. **Reportar resultado**:
   - Mostrar output detallado arriba
   - Listar archivos creados
   - Indicar próximos pasos

---

## Post-execution

Después de ejecutar el comando:

1. **Revisar archivos generados**:
   ```bash
   # Agentes
   cat .claude/agents/planner-{dominio}.md

   # CONTEXT.md
   cat .claude/CONTEXT.md

   # Comandos
   ls .claude/commands/
   ```

2. **Personalizar si necesario**:
   - Nombres de proyecto
   - Descripciones específicas
   - Añadir specialists custom

3. **Commit estructura**:
   ```bash
   git add .claude/
   git commit -m "feat: bootstrap workflow-optimizer structure

   Generated by /bootstrap-workflow:
   - 8 specialized agents (Manager + 7 Tool Specialists)
   - 12 core commands (9 universal + 3 domain-specific)
   - CONTEXT.md initial state

   Project: {NOMBRE}
   Stack: {STACK}
   Score: {SCORE} points (Complejo)

   Ready for optimized development workflow."
   ```

4. **Probar workflow**:
   ```bash
   claude "Implementar feature inicial: {descripción}"
   ```

---

## Parámetros Opcionales

### `--existing-project`

Para proyectos con código existente:
- Analiza arquitectura actual (no genera desde cero)
- Detecta patrones (hexagonal, MVC, etc.)
- Identifica deuda técnica
- Genera agentes compatibles

### `--force-score=N`

Forzar score específico (override análisis):
```bash
claude /bootstrap-workflow --force-score=12
# Genera 8 agentes independientemente del análisis
```

### `--verbose`

Mostrar análisis detallado:
```bash
claude /bootstrap-workflow --verbose
# Muestra cada paso del análisis con detalles
```

### `--domain={dominio}`

Forzar dominio específico:
```bash
claude /bootstrap-workflow --domain=fintech
# Usa templates y comandos fintech
```

---

## Troubleshooting

### Stack no detectado correctamente

**Síntoma**: "No se pudo detectar stack tecnológico"

**Solución**:
```bash
# Forzar stack manualmente
claude /bootstrap-workflow --stack=nodejs,react,postgres
```

### Score parece incorrecto

**Síntoma**: "Score: 5 puntos pero proyecto es complejo"

**Solución**:
```bash
# Forzar score manualmente
claude /bootstrap-workflow --force-score=12
```

### Dominio mal identificado

**Síntoma**: "Detectó e-commerce pero es fintech"

**Solución**:
```bash
# Forzar dominio
claude /bootstrap-workflow --domain=fintech
```

---

## Validación Post-Bootstrap

Después de ejecutar bootstrap, validar:

```bash
# 1. Estructura creada
ls -la .claude/

# Expected:
# agents/ (3, 5 u 8 archivos)
# commands/ (9-15 archivos)
# sessions/ (README.md)
# doc/ (vacío)
# CONTEXT.md

# 2. CONTEXT.md válido
cat .claude/CONTEXT.md | wc -l
# Expected: 80-150 líneas

# 3. Agentes tienen Manager
ls .claude/agents/planner-*.md
# Expected: 1 archivo planner

# 4. Comandos core presentes
ls .claude/commands/phase-next.md
ls .claude/commands/generate-tests.md
ls .claude/commands/status-complete.md
# Expected: Existen

# 5. Probar comando
claude /status-complete
# Expected: Reporte estado proyecto
```

---

## Métricas de Éxito

**Inmediato** (post-bootstrap):
- ✅ Estructura .claude/ completa
- ✅ N agentes generados (3, 5 u 8)
- ✅ 9+ comandos disponibles
- ✅ CONTEXT.md ≤150 líneas

**1 semana después**:
- ✅ 1+ feature implementada con workflow
- ✅ planner delega correctamente
- ✅ /phase-next usado ≥3 veces

**1 mes después**:
- ✅ Tokens -50-70% vs baseline
- ✅ Velocidad +50-83% vs baseline
- ✅ Coverage ≥90%

---

**Comando listo para usar en cualquier proyecto**

**Versión**: 1.0
**Compatible con**: Node.js, Python, Java, Go, Rust, TypeScript
**Dominios soportados**: E-commerce, Fintech, Healthcare, SaaS, DevOps, Generic
