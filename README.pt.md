🌐 [English](README.md) | [Español](README.es.md) | [Français](README.fr.md) | [Italiano](README.it.md) | [Português](README.pt.md) | [Deutsch](README.de.md)

# Review Staged

Um skill de Claude Code que executa revisão de código multi-agente em paralelo sobre as tuas alterações git em staging.

Dois revisores de IA independentes examinam o teu código simultaneamente — um focado em segurança e bugs, o outro em arquitetura e qualidade de código. Um passe de simplificação identifica complexidade desnecessária. Os resultados são cruzados para filtrar falsos positivos e apresentados num relatório consolidado com veredicto PASS/FAIL/DISCUSS.

## Como Funciona

1. **Dois revisores em paralelo** — Security Engineer + Software Architect examinam as alterações em staging de forma independente
2. **Simplificação de código** — Analisa o código alterado à procura de complexidade desnecessária, código morto e oportunidades de limpeza
3. **Cruzamento de resultados** — O orquestrador valida os resultados, identifica problemas consensuais, filtra falsos positivos
4. **Relatório consolidado** — Problemas ordenados por severidade com correções, oportunidades de simplificação, veredicto claro

## Instalação

```bash
git clone https://github.com/entpnomad/review-staged.git ~/.claude/skills/review-staged
```

Ou para um projeto específico:

```bash
git clone https://github.com/entpnomad/review-staged.git .claude/skills/review-staged
```

## Uso

Adiciona as tuas alterações ao staging, depois:

```
/review-staged
```

Ou simplesmente pede ao Claude para "review my staged changes."

## Personalização

O skill foi desenhado para se adaptar ao teu projeto. Consulta a secção Customization no SKILL.md para:
- Modificar os papéis dos revisores para o teu stack tecnológico
- Adicionar critérios de bloqueio específicos do projeto
- Adicionar fases de ferramentas automatizadas

## Licença

MIT
