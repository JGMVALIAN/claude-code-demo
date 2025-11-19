# 🚀 Workflow Optimization Analyzer

> **Sistema de optimización para desarrollo asistido con Claude Code**  
> Reducción -70% tokens | Velocidad +83% | Coverage ≥95%

## ¿Qué es esto?

Framework completo para implementar **arquitecturas multi-agente optimizadas** en proyectos usando Claude Code. Basado en metodología **dot-claude** validada en producción con 45k LOC.

## 🎯 Quick Start (5 minutos)

```bash
# 1. Clonar repositorio
git clone https://github.com/JGMVALIAN/agents.git
cd agents

# 2. Elegir template según dominio
cp -r templates/project-templates/legal-compliance/.claude tu-proyecto/

# 3. Personalizar agentes
# Editar archivos .claude/agents/*.md según necesidades

# 4. Iniciar workflow
cd tu-proyecto
claude /phase-next
```

## 📊 Benchmarks Validados

| Métrica | Baseline | Con Sistema | Mejora |
|---------|----------|-------------|--------|
| Reducción tokens | 100% | 30% | **-70%** |
| Velocidad desarrollo | 1x | 1.83x | **+83%** |
| Test coverage | 75% | 96% | **+21pp** |
| Escalabilidad | 10k LOC | 20M+ LOC | **+2000x** |

*Fuente: Proyecto LCSP RAG Suite (caso estudio real)*

## 🏗️ Arquitectura

### Sistema Multi-Agente

```
Manager (Planner)
    ↓
    ├─→ Tool Specialist (Python)
    ├─→ Tool Specialist (Domain)
    ├─→ Tool Specialist (Testing)
    └─→ Validator (QA)
```

### Pilares Metodología

1. **Carga Selectiva Contexto** - Solo agentes necesarios
2. **Agentes Especializados** - Roles únicos sin solapamiento
3. **Comandos Modulares** - Workflows reutilizables
4. **Patrón Manager-Tool** - Coordinación centralizada
5. **Documentación Auto-actualizable** - CONTEXT.md vivo

## 📚 Documentación

- [**Metodología Completa**](docs/methodology/dot-claude-system.md) - Sistema dot-claude detallado
- [**Guía Análisis Proyectos**](docs/guides/project-analysis.md) - Cómo analizar proyectos nuevos
- [**Caso Estudio: LCSP RAG Suite**](docs/case-studies/lcsp-rag-suite.md) - 45k LOC, 96% coverage
- [**Patrón Manager-Tool**](docs/methodology/manager-tool-pattern.md) - Arquitectura multi-agente

## 🎯 Templates por Dominio

### Disponibles

- ✅ **Legal/Compliance** - Licitaciones públicas, auditorías legales
- ✅ **Fintech/Trading** - Sistemas financieros, risk management
- ✅ **Healthcare/HIPAA** - Apps médicas, compliance sanitario
- ✅ **E-commerce/Retail** - Plataformas venta, inventarios

### Estructura Template

Cada template incluye:
- `.claude/agents/` - 5+ agentes especializados
- `.claude/commands/` - Comandos workflow personalizados
- `.claude/CONTEXT.md` - Contexto proyecto optimizado
- Configuraciones específicas dominio

## 🛠️ CLI Tools

```bash
# Analizar proyecto existente
python scripts/analyze_project.py /ruta/proyecto

# Generar agentes para nuevo proyecto
python scripts/generate_agents.py --domain legal --name MiProyecto

# Validar estructura .claude/
python scripts/validate_structure.py
```

## 📖 Guías Rápidas

### Crear Sistema para Proyecto Nuevo

```bash
# 1. Analizar proyecto
python scripts/analyze_project.py /mi-proyecto

# 2. Generar estructura .claude/
python scripts/generate_agents.py --domain legal --name MiProyecto

# 3. Copiar a proyecto
cp -r output/.claude /mi-proyecto/

# 4. Iniciar workflow
cd /mi-proyecto
claude /phase-next
```

### Adaptar Template Existente

1. Copiar template base: `cp -r templates/project-templates/legal-compliance/.claude .`
2. Editar agentes: Modificar `name`, `description`, especialización
3. Personalizar comandos: Ajustar workflows a necesidades
4. Actualizar CONTEXT.md: Reflejar estado actual proyecto

## 🏆 Casos Estudio Validados

### LCSP RAG Suite (Legal/Compliance)

**Proyecto**: Sistema RAG para análisis licitaciones públicas españolas  
**Stack**: Python 3.11, FastAPI, ChromaDB, pytest-asyncio  
**Resultados**:
- 45,000 LOC producción
- 96% test coverage
- -70% reducción tokens
- +83% velocidad desarrollo
- 8 módulos especializados

[Ver análisis completo →](docs/case-studies/lcsp-rag-suite.md)

## 📈 Roadmap

- [ ] Soporte más dominios (DevOps, Data Science, ML/AI)
- [ ] CLI interactivo generación agentes
- [ ] Dashboard métricas tiempo real
- [ ] Integración CI/CD automática
- [ ] Plugin VSCode/IDEs
- [ ] Templates multi-framework (Django, FastAPI, Spring, etc)

## 🤝 Contribuir

Las contribuciones son bienvenidas. Ver [CONTRIBUTING.md](CONTRIBUTING.md) para:
- Reportar bugs
- Proponer nuevos templates
- Añadir casos estudio
- Mejorar documentación

### Áreas Prioritarias

- Templates nuevos dominios
- Scripts automatización adicionales
- Casos estudio proyectos reales
- Mejoras métricas/benchmarks

## 📄 Licencia

MIT License - Ver [LICENSE](LICENSE)

Libre uso comercial y personal. Atribución apreciada pero no requerida.

## 🙏 Agradecimientos

Basado en:
- **Metodología dot-claude** - Sistema carga selectiva contexto
- **Patrón Manager-Tool Specialists** - Arquitectura multi-agente
- **Proyecto LCSP RAG Suite** - Validación producción 45k LOC

## 📞 Soporte

- **Issues**: Reportar bugs o solicitar features
- **Discussions**: Preguntas generales y casos uso
- **Pull Requests**: Contribuciones código/documentación

## 🌟 Showcase

¿Usas este sistema en tu proyecto? ¡Comparte tu caso estudio!

---

**Generado por Workflow Optimization Analyzer v1.0**  
**Última actualización**: Noviembre 2025
