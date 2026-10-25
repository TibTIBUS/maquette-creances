# Assistant IA de Créances (version 2, prête à brancher)

Sur GitHub Pages, la maquette utilise l'**assistant sans IA** : il cherche dans les pages du site et cite le passage exact. Gratuit, rien n'est envoyé à l'extérieur.

Ce dossier contient la **version IA** : un petit serveur (fonction Netlify) qui envoie la question à Claude avec, comme seule source, le contenu du site (`netlify/functions/kb.mjs`, 243 extraits). Claude doit :

- répondre uniquement avec ces extraits, en citant la page source ;
- refuser toute question hors sujet ou sans réponse sur le site (« Je ne trouve pas cette information… contactez la mairie ») ;
- recopier horaires, tarifs et numéros tels quels, sans rien deviner ;
- signaler les informations « à vérifier ».

Dès que le site est ouvert sur une adresse `*.netlify.app`, la page bascule automatiquement sur la version IA. Si le service IA ne répond pas, elle revient à l'assistant sans IA.

## Mise en route (3 étapes)

1. **Netlify** : *Add new site* › *Import an existing project* › GitHub › `maquette-creances`. Les réglages sont lus dans `netlify.toml`, il n'y a rien à changer.
2. **Clé API** (à faire soi-même : c'est un identifiant) : créer une clé sur console.anthropic.com › *API Keys*, puis dans Netlify › *Site configuration* › *Environment variables*, ajouter `ANTHROPIC_API_KEY` avec cette clé. Fixer aussi une **limite de dépense mensuelle** dans la console Anthropic.
3. **Redéployer** le site sur Netlify, puis tester sur l'adresse `…netlify.app`.

Options : `CLAUDE_MODEL` (par défaut `claude-haiku-5-5`), `ANTHROPIC_VERSION` (par défaut `2023-06-01`).

## Avant toute mise en service réelle

- **Coût** : facturé à l'usage (chaque question envoie le contenu du site, mis en cache pour réduire le coût). Voir la grille tarifaire sur docs.claude.com.
- **RGPD** : les questions des habitants transitent par Anthropic. À mentionner dans la politique de confidentialité et à faire valider par la mairie.
- **Abus** : l'adresse de l'assistant est publique ; la limite de dépense est la protection principale. Un filtrage par adresse IP pourra être ajouté.
- **Mise à jour** : quand le contenu du site change, régénérer `kb.mjs` à partir des pages (export automatique de la maquette).
