🌐 [English](README.md) | [Español](README.es.md) | [Français](README.fr.md) | [Italiano](README.it.md) | [Português](README.pt.md) | [Deutsch](README.de.md)

# Review Staged

Uno skill di Claude Code che esegue revisione del codice multi-agente in parallelo sui tuoi cambiamenti git in staging.

Due revisori IA indipendenti esaminano il tuo codice simultaneamente — uno focalizzato su sicurezza e bug, l'altro su architettura e qualità del codice. Un passaggio di semplificazione identifica la complessità non necessaria. I risultati vengono incrociati per filtrare i falsi positivi, poi presentati in un report consolidato con verdetto PASS/FAIL/DISCUSS.

## Come Funziona

1. **Due revisori in parallelo** — Security Engineer + Software Architect esaminano i cambiamenti in staging in modo indipendente
2. **Semplificazione del codice** — Analizza il codice modificato cercando complessità non necessaria, codice morto e opportunità di pulizia
3. **Incrocio dei risultati** — L'orchestratore valida i risultati, identifica problemi consensuali, filtra i falsi positivi
4. **Report consolidato** — Problemi ordinati per severità con correzioni, opportunità di semplificazione, verdetto chiaro

## Installazione

```bash
git clone https://github.com/entpnomad/review-staged.git ~/.claude/skills/review-staged
```

O per un progetto specifico:

```bash
git clone https://github.com/entpnomad/review-staged.git .claude/skills/review-staged
```

## Uso

Aggiungi i tuoi cambiamenti allo staging, poi:

```
/review-staged
```

Oppure chiedi semplicemente a Claude di "review my staged changes."

## Personalizzazione

Lo skill è progettato per adattarsi al tuo progetto. Consulta la sezione Customization in SKILL.md per:
- Modificare i ruoli dei revisori per il tuo stack tecnologico
- Aggiungere criteri di blocco specifici del progetto
- Aggiungere fasi di strumenti automatizzati

## Licenza

MIT
