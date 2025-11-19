# Guía Análisis Proyectos

## Quick Start

Esta guía te ayuda a analizar un proyecto existente o nuevo para implementar el sistema dot-claude optimizado.

**Tiempo estimado**: 30-45 minutos

---

## Proceso Análisis

### 1. Recopilación Información (5 min)

**Completar checklist básico**:
```markdown
- [ ] Nombre proyecto
- [ ] Dominio (legal/fintech/healthcare/ecommerce/otro)
- [ ] Stack tecnológico principal (Python/JavaScript/Java/etc)
- [ ] LOC estimadas (1k/10k/100k/1M+)
- [ ] Fase actual (inicio/desarrollo/producción)
- [ ] Tamaño equipo (solo/2-5/5-10/10+)
- [ ] Test coverage actual (si existe)
- [ ] Frameworks principales
```

### 2. Análisis Complejidad (10 min)

**Matriz evaluación**:

| Dimensión | Baja (1pt) | Media (2pts) | Alta (3pts) |
|-----------|------------|--------------|-------------|
| LOC | <10k | 10k-100k | >100k |
| Módulos | 1-3 | 4-8 | 9+ |
| Dominio | Simple | Regulado | Multi-regulado |
| Testing | <70% | 70-90% | >90% target |
| Team size | 1-2 | 3-5 | 6+ |

**Scoring**:
- **5-7 puntos**: Proyecto Simple → 3 agentes
- **8-11 puntos**: Proyecto Medio → 5 agentes  
- **12-15 puntos**: Proyecto Complejo → 8+ agentes

### 3. Diseño Arquitectura Agentes (15 min)

#### Configuración Mínima (3 agentes)

Para proyectos **Simple** (5-7 puntos):

```
planner-{domain}.md        # Manager
code-specialist.md         # Implementación
validator-{domain}.md      # QA
```

**Ejemplo**: App web simple, 1-2 devs, <10k LOC

#### Configuración Estándar (5 agentes)

Para proyectos **Medio** (8-11 puntos):

```
planner-{domain}.md            # Manager
python-{domain}-expert.md      # Code specialist
{domain}-expert.md             # Domain specialist
test-engineer-{domain}.md      # Testing
validator-{domain}.md          # QA
```

**Ejemplo**: API REST con reglas negocio, 3-5 devs, 10k-100k LOC

#### Configuración Avanzada (8+ agentes)

Para proyectos **Complejo** (12-15 puntos):

```
planner-{domain}.md            # Manager
backend-expert.md              # Backend code
frontend-expert.md             # Frontend code
database-expert.md             # DB optimization
{domain}-expert.md             # Domain rules
test-engineer.md               # Testing
devops-expert.md               # Infrastructure
security-expert.md             # Security review
validator.md                   # QA final
```

**Ejemplo**: Plataforma enterprise, >10 devs, >100k LOC, arquitectura distribuida

### 4. Definir Comandos Core (10 min)

#### Comandos Universales (Todos los proyectos)

```markdown
/phase-next            # Implementar siguiente paso roadmap
/generate-tests        # Generar tests automáticos
/status-complete       # Reporte estado completo proyecto
/backup-{resource}     # Snapshot recursos críticos
```

#### Comandos Específicos por Dominio

**Legal/Compliance**:
```markdown
/legal-validate        # Validación compliance normativo
/generate-pliego       # Generar pliego condiciones
/audit-trail           # Trazabilidad acciones
```

**Fintech/Trading**:
```markdown
/risk-check            # Análisis riesgos
/backtest-strategy     # Backtesting estrategia
/compliance-report     # Reporte regulatorio
```

**Healthcare/HIPAA**:
```markdown
/hipaa-validate        # Validación HIPAA compliance
/phi-audit             # Auditoría PHI access
/security-scan         # Scan vulnerabilidades
```

**E-commerce/Retail**:
```markdown
/inventory-sync        # Sincronizar inventario
/order-validate        # Validar pedidos
/analytics-report      # Métricas ventas
```

### 5. Crear Estructura (5 min)

```bash
# Crear carpetas base
mkdir -p .claude/{agents,commands}

# Crear archivo contexto inicial
touch .claude/CONTEXT.md

# Copiar templates apropiados
cp templates/project-templates/{domain}/.claude/agents/* .claude/agents/
cp templates/project-templates/{domain}/.claude/commands/* .claude/commands/

# Personalizar nombres
# Editar archivos .claude/agents/*.md reemplazar {PROJECT_NAME}, {DOMAIN}
```

---

## Roadmap Implementación

### Semana 1: Setup Base

**Objetivos**:
- [ ] Estructura .claude/ creada
- [ ] 3-5 agentes configurados
- [ ] CONTEXT.md inicial (<200 líneas)
- [ ] 3 comandos core funcionando

**Esfuerzo**: 4-8 horas

**Validación**:
```bash
# Test comando básico
claude /status-complete

# Verificar Manager carga
claude "¿Qué agentes tenemos disponibles?"
```

### Semana 2-4: Adopción Gradual

**Objetivos**:
- [ ] Nuevo desarrollo usa sistema dot-claude
- [ ] Medir baseline métricas (tokens, velocidad)
- [ ] Ajustar agentes según feedback
- [ ] Expandir comandos (5-7 total)

**Métricas esperadas**:
- +30-50% velocidad desarrollo
- -40-50% consumo tokens
- Coverage mantiene/mejora

### Mes 2-3: Optimización

**Objetivos**:
- [ ] Refactoring código legacy a sistema
- [ ] Comandos avanzados (8-10 total)
- [ ] Documentación consolidada
- [ ] Workflow team estandarizado

**Métricas esperadas**:
- +60-70% velocidad desarrollo
- -60-70% consumo tokens
- Coverage ≥95% si target
- Onboarding nuevos devs <1 día

---

## Templates por Dominio

### Ubicación Templates

```
templates/project-templates/
├── legal-compliance/
│   └── .claude/
│       ├── agents/
│       │   ├── planner-legal.md
│       │   ├── python-legal-expert.md
│       │   ├── legal-compliance-expert.md
│       │   ├── test-engineer-legal.md
│       │   └── validator-legal.md
│       ├── commands/
│       │   ├── legal-validate.md
│       │   ├── generate-pliego.md
│       │   └── phase-next.md
│       └── CONTEXT_template.md
│
├── fintech-trading/
├── healthcare-hipaa/
└── ecommerce-retail/
```

### Cómo Usar Templates

1. **Copiar template** dominio apropiado
2. **Reemplazar placeholders**:
   - `{PROJECT_NAME}` → Nombre proyecto
   - `{DOMAIN}` → Dominio específico
   - `{TECH_STACK}` → Stack tecnológico
3. **Personalizar** según necesidades específicas
4. **Validar** ejecutando comandos test

---

## Métricas Seguimiento

### KPIs Baseline (Medir antes implementar)

```markdown
## Métricas Pre-Optimización

**Velocidad desarrollo**:
- Tiempo promedio feature: X días
- Features completadas/semana: Y

**Consumo tokens**:
- Tokens/sesión promedio: Z
- Costo mensual Claude: $W

**Calidad**:
- Test coverage: X%
- Bugs producción/mes: Y
- Tiempo fix bugs: Z horas

**Team**:
- Tiempo onboarding: X días
- Context switching: Y veces/día
```

### KPIs Post-Optimización (Medir tras 1 mes)

```markdown
## Métricas Post-Optimización

**Velocidad desarrollo**:
- Tiempo promedio feature: -X% ✅
- Features completadas/semana: +Y% ✅

**Consumo tokens**:
- Tokens/sesión promedio: -70% ✅
- Costo mensual Claude: -$W ✅

**Calidad**:
- Test coverage: +Z pp ✅
- Bugs producción/mes: -X% ✅
- Tiempo fix bugs: -Y% ✅

**Team**:
- Tiempo onboarding: -80% ✅
- Context switching: -60% ✅
```

---

## Troubleshooting Común

### "Manager no delega correctamente"

**Síntoma**: Manager implementa código directamente

**Solución**:
1. Revisar sección "NO hago" en `planner-{domain}.md`
2. Hacer explícito: "❌ Implementar código → python-expert"
3. Reforzar en prompt: "SIEMPRE delegar a specialist apropiado"

### "Tool Specialists se comunican entre sí"

**Síntoma**: Python Expert menciona validar con Domain Expert

**Solución**:
1. Reforzar: "NO comunicación directa Tool ↔ Tool"
2. Aclarar: "SOLO reportar a Manager"
3. Manager debe coordinar explícitamente

### "CONTEXT.md crece >200 líneas"

**Síntoma**: Archivo contexto demasiado largo

**Solución**:
1. Compactar: Eliminar info obsoleta
2. Usar referencias `@archivo:línea` vs copiar contenido
3. Comando `/compact` periódico

### "Coverage no mejora"

**Síntoma**: Tests no alcanzan target ≥95%

**Solución**:
1. Revisar Test Engineer tiene herramientas necesarias (bash)
2. Validar Test Engineer ejecuta tests realmente
3. Manager debe verificar coverage antes aprobar

---

## Casos Estudio

### LCSP RAG Suite (Legal/Compliance)

**Contexto**:
- 45,000 LOC Python
- 8 módulos especializados
- 175 tests, 96% coverage
- Dominio: Licitaciones públicas España

**Resultados tras 3 meses**:
- **-70%** tokens/sesión
- **+83%** velocidad desarrollo
- **96%** coverage (target ≥95%)
- **-87%** bugs producción

[Ver análisis completo →](../case-studies/lcsp-rag-suite.md)

---

## Checklist Final Setup

```markdown
### Pre-Launch

- [ ] Estructura .claude/ creada
- [ ] Agentes configurados (3-8 según complejidad)
- [ ] Comandos core implementados (≥3)
- [ ] CONTEXT.md inicial (<200 líneas)
- [ ] Roadmap definido (≥5 fases)
- [ ] Métricas baseline medidas

### Launch

- [ ] Team capacitado en comandos
- [ ] Primer feature desarrollado con sistema
- [ ] Validación métricas positivas
- [ ] Ajustes iniciales aplicados

### Post-Launch (1 mes)

- [ ] Métricas comparadas vs baseline
- [ ] Workflow refinado según feedback
- [ ] Documentación actualizada
- [ ] Comandos expandidos (5-10 total)
- [ ] ROI validado (+50% velocidad mínimo)
```

---

## Recursos Adicionales

**Documentación**:
- [Sistema dot-claude](../methodology/dot-claude-system.md)
- [Patrón Manager-Tool](../methodology/manager-tool-pattern.md)

**Templates**:
- `/templates/project-templates/` - Por dominio
- `/templates/.claude/agents/` - Templates agentes genéricos

**Scripts**:
- `scripts/analyze_project.py` - Análisis automático
- `scripts/generate_agents.py` - Generación agentes
- `scripts/validate_structure.py` - Validación setup

---

**Versión**: 1.0  
**Última actualización**: Noviembre 2025  
**Mantenido por**: Workflow Optimization Analyzer
