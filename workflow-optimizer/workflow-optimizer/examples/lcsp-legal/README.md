# Ejemplo: LCSP RAG Suite (Legal/Compliance)

## Proyecto Real Validado

Sistema RAG para análisis licitaciones públicas españolas conforme LCSP 9/2017.

**Resultados**:
- 45,000 LOC Python
- 96% test coverage  
- -70% reducción tokens
- +83% velocidad desarrollo

## Estructura

```
.claude/
├── agents/           # 5 agentes especializados
├── commands/         # 5 comandos workflow
└── CONTEXT.md        # Contexto proyecto (185 líneas)

analysis/
├── project-analysis.md   # Análisis inicial
├── roadmap.md            # 8 fases implementación
└── metrics.md            # Métricas validadas
```

## Agentes Implementados

1. **planner-legal.md** - Manager coordinación
2. **python-legal-expert.md** - Implementación código
3. **legal-compliance-expert.md** - Validación LCSP/LPAC
4. **test-engineer-legal.md** - Tests coverage 96%
5. **validator-legal.md** - QA final

## Uso

```bash
# Copiar a tu proyecto
cp -r .claude /tu-proyecto-legal/

# Personalizar
# Editar .claude/agents/*.md con tu PROJECT_NAME

# Iniciar
cd /tu-proyecto-legal
claude /phase-next
```

## Métricas Reales

| Métrica | Antes | Después | Mejora |
|---------|-------|---------|--------|
| Tokens/sesión | 12,000 | 3,600 | -70% |
| Velocidad | 3 días/feature | 0.5 días | +83% |
| Coverage | 75% | 96% | +21pp |
| Bugs/mes | 8 | 1 | -87% |

Ver `analysis/metrics.md` para detalles completos.
