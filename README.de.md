🌐 [English](README.md) | [Español](README.es.md) | [Français](README.fr.md) | [Italiano](README.it.md) | [Português](README.pt.md) | [Deutsch](README.de.md)

# Review Staged

Ein Claude Code Skill, der Multi-Agenten-Code-Review parallel auf deine gestagten Git-Änderungen ausführt.

Zwei unabhängige KI-Reviewer untersuchen deinen Code gleichzeitig — einer fokussiert auf Sicherheit und Bugs, der andere auf Architektur und Code-Qualität. Ein Vereinfachungs-Durchlauf identifiziert unnötige Komplexität. Die Ergebnisse werden gegengeprüft, um False Positives zu filtern, und dann als konsolidierter Bericht mit PASS/FAIL/DISCUSS-Urteil präsentiert.

## So funktioniert es

1. **Zwei Reviewer parallel** — Security Engineer + Software Architect untersuchen gestage Änderungen unabhängig voneinander
2. **Code-Vereinfachung** — Analysiert geänderten Code auf unnötige Komplexität, toten Code und Aufräum-Möglichkeiten
3. **Gegenprüfung** — Der Orchestrator validiert Ergebnisse, identifiziert Konsens-Probleme, filtert False Positives
4. **Konsolidierter Bericht** — Nach Schweregrad geordnete Probleme mit Fixes, Vereinfachungsmöglichkeiten, klares Urteil

## Installation

```bash
git clone https://github.com/entpnomad/review-staged.git ~/.claude/skills/review-staged
```

Oder für ein bestimmtes Projekt:

```bash
git clone https://github.com/entpnomad/review-staged.git .claude/skills/review-staged
```

## Verwendung

Stage deine Änderungen, dann:

```
/review-staged
```

Oder bitte Claude einfach, "review my staged changes."

## Anpassung

Der Skill ist dafür gemacht, an dein Projekt angepasst zu werden. Siehe die Customization-Sektion in SKILL.md für:
- Reviewer-Rollen für deinen Tech-Stack anpassen
- Projektspezifische Blockierkriterien hinzufügen
- Automatisierte Tool-Phasen hinzufügen

## Lizenz

MIT
