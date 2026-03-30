🌐 [English](README.md) | [Español](README.es.md) | [Français](README.fr.md) | [Italiano](README.it.md) | [Português](README.pt.md) | [Deutsch](README.de.md)

# Review Staged

Un skill de Claude Code que ejecuta revisión de código multi-agente en paralelo sobre tus cambios en staging de git.

Dos revisores de IA independientes examinan tu código simultáneamente — uno enfocado en seguridad y bugs, el otro en arquitectura y calidad de código. Un pase de simplificación identifica complejidad innecesaria. Los hallazgos se cruzan para filtrar falsos positivos, y se presentan en un informe consolidado con veredicto PASS/FAIL/DISCUSS.

## Cómo Funciona

1. **Dos revisores en paralelo** — Security Engineer + Software Architect examinan los cambios en staging de forma independiente
2. **Simplificación de código** — Analiza el código modificado buscando complejidad innecesaria, código muerto y oportunidades de limpieza
3. **Cruce de resultados** — El orquestador valida hallazgos, identifica problemas consensuados, filtra falsos positivos
4. **Informe consolidado** — Problemas ordenados por severidad con correcciones, oportunidades de simplificación, veredicto claro

## Instalación

```bash
git clone https://github.com/entpnomad/review-staged.git ~/.claude/skills/review-staged
```

O para un proyecto específico:

```bash
git clone https://github.com/entpnomad/review-staged.git .claude/skills/review-staged
```

## Uso

Añade tus cambios al staging, luego:

```
/review-staged
```

O simplemente pídele a Claude que "review my staged changes."

## Personalización

El skill está diseñado para adaptarse a tu proyecto. Consulta la sección Customization en SKILL.md para:
- Modificar los roles de los revisores para tu stack tecnológico
- Añadir criterios de bloqueo específicos del proyecto
- Añadir fases de herramientas automatizadas

## Licencia

MIT
