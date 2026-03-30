🌐 [English](README.md) | [Español](README.es.md) | [Français](README.fr.md) | [Italiano](README.it.md)

# Review Staged

Un skill Claude Code qui lance une revue de code multi-agent en parallèle sur vos changements git en staging.

Deux réviseurs IA indépendants examinent votre code simultanément — l'un focalisé sur la sécurité et les bugs, l'autre sur l'architecture et la qualité du code. Un passage de simplification identifie la complexité inutile. Les résultats sont croisés pour filtrer les faux positifs, puis présentés dans un rapport consolidé avec un verdict PASS/FAIL/DISCUSS.

## Comment ça marche

1. **Deux réviseurs en parallèle** — Security Engineer + Software Architect examinent les changements en staging indépendamment
2. **Simplification du code** — Analyse le code modifié pour détecter la complexité inutile, le code mort et les opportunités de nettoyage
3. **Croisement des résultats** — L'orchestrateur valide les résultats, identifie les problèmes consensuels, filtre les faux positifs
4. **Rapport consolidé** — Problèmes classés par sévérité avec corrections, opportunités de simplification, verdict clair

## Installation

```bash
git clone https://github.com/entpnomad/review-staged.git ~/.claude/skills/review-staged
```

Ou pour un projet spécifique :

```bash
git clone https://github.com/entpnomad/review-staged.git .claude/skills/review-staged
```

## Utilisation

Ajoutez vos changements au staging, puis :

```
/review-staged
```

Ou demandez simplement à Claude de "review my staged changes."

## Personnalisation

Le skill est conçu pour s'adapter à votre projet. Consultez la section Customization dans SKILL.md pour :
- Modifier les rôles des réviseurs pour votre stack technique
- Ajouter des critères de blocage spécifiques au projet
- Ajouter des phases d'outils automatisées

## Licence

MIT
