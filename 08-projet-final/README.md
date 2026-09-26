# Projet 08 — Projet final

| Paramètre | Valeur |
|---|---|
| Niveau | Synthèse |
| Prérequis | Projets 01 à 07 terminés |
| Stack | PHP natif (POO), PDO, MySQL, Docker, CI/CD |
| Durée indicative | 20 heures et plus |

## Contexte

Il n'y a pas de cahier des charges fourni pour ce projet, volontairement. Après 7 projets guidés, l'objectif est de prouver que tu peux partir d'une idée à toi et la mener jusqu'au bout, avec les mêmes exigences que sur les projets précédents — sans que quelqu'un te tienne la main sur les règles métier.

## Ce que tu dois faire toi-même, avant de coder

1. **Choisir un thème** que tu maîtrises ou qui t'intéresse réellement (pas un simple remix d'un projet précédent — un vrai domaine avec ses propres règles).
2. **Rédiger ton propre cahier des charges** : contexte, entités, fonctionnalités, règles métier numérotées, contraintes techniques. Fais-le sérieusement, comme si tu le donnais à quelqu'un d'autre à développer.
3. **Faire valider ce cahier des charges avant de coder** (relis-le toi-même à froid le lendemain, ou fais-le relire).

## Exigences non négociables (héritées des 7 projets précédents)

- Architecture en couches (Controller / Service / Repository / Model), aucune règle métier dans une vue ou un contrôleur.
- Au moins une relation de données non triviale (comme au projet 03, 04 ou 06).
- Au moins un ensemble de règles métier numérotées et testées.
- Authentification et rôles si le domaine le justifie (sinon, documente pourquoi tu t'en passes).
- Git discipliné : commits atomiques, messages clairs, aucune solution codée d'un coup en un seul commit géant.
- Tests unitaires sur au moins la logique métier la plus complexe du projet.

## Nouveauté par rapport aux projets précédents : Docker et CI/CD complets

- Dockerfile fonctionnel (app + base de données via Docker Compose).
- Pipeline CI (GitHub Actions ou GitLab CI, au choix) qui exécute au minimum : installation des dépendances, analyse statique si tu en utilises une, et tests.
- Le pipeline doit être vert avant de considérer le projet terminé.

## DevLog obligatoire

Comme sur les projets précédents mais cette fois sur l'ensemble du projet plutôt que par incrément : documente ton cahier des charges, tes choix d'architecture, au moins une difficulté réelle rencontrée et comment tu l'as résolue, et ce que tu changerais si tu recommençais.

## Checklist d'auto-évaluation

- [ ] Le cahier des charges a été écrit avant le code, pas après coup pour justifier ce qui existe déjà.
- [ ] Aucune règle métier dans le contrôleur ou la vue.
- [ ] Le pipeline CI est vert.
- [ ] L'application tourne avec une seule commande (`docker compose up`).
- [ ] Le DevLog explique de vraies décisions, pas une liste de fichiers créés.

## Pour la suite

Une fois ce projet terminé, tu as tout ce qu'il faut pour attaquer un vrai projet avec framework (Laravel, par exemple) en sachant exactement ce que le framework automatise à ta place — et pourquoi.
