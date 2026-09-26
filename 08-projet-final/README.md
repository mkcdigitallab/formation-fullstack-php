# Projet 08 — Projet final : tout combiné

## Contexte

Pas de nouveau sujet imposé ici. Tu choisis un thème qui te motive
(association sportive, événementiel, garage auto, pharmacie, école — ce que
tu veux) et tu construis une application complète qui réutilise et combine
tout ce que tu as musclé dans les 7 projets précédents.

## Objectifs pédagogiques

- Concevoir un projet de A à Z sans cahier des charges détaillé fourni —
  c'est toi qui écris tes propres spécifications, comme en vrai
- Mettre en place un vrai workflow Git avec branches et pull requests
- Mettre en place une intégration continue simple (GitHub Actions) qui fait
  tourner les tests automatiquement
- Déployer via Docker de façon reproductible

## Ce que le projet doit obligatoirement contenir

- Au moins 2 entités métier liées avec des règles de gestion réelles (pas
  du simple CRUD sans contrainte)
- Une authentification avec au moins 2 rôles
- Une logique métier non triviale, testée unitairement (calcul, machine à
  états, ou détection de conflit — au choix)
- Une API JSON en plus des vues HTML pour au moins une ressource
- Un `docker-compose.yml` qui lance l'application et sa base de données en
  une commande depuis un clone frais du dépôt

## Méthode imposée

1. Écris toi-même un `README.md` de cahier des charges avant de coder,
   sur le modèle des projets précédents (Contexte / Fonctionnalités /
   Règles métier / Contraintes techniques).
2. Travaille avec des branches Git (`feature/xxx`) et fusionne via des Pull
   Requests sur GitHub, même en solo — relis ton propre diff avant de
   merger.
3. Ajoute un fichier `.github/workflows/tests.yml` qui lance
   `vendor/bin/phpunit` à chaque push.
4. Documente l'installation dans le README comme si un inconnu devait
   lancer le projet sans ton aide.

## Checklist d'auto-évaluation finale

- [ ] Un inconnu peut cloner le dépôt, lire le README, et lancer le projet
      sans te poser une seule question
- [ ] `git log` raconte une histoire cohérente du projet, pas une suite de
      "fix", "wip", "update"
- [ ] La CI passe au vert sur GitHub
- [ ] Aucune règle métier n'est dupliquée entre le contrôleur HTML et
      l'API JSON — elles partagent le même Service
- [ ] Tu es capable d'expliquer, sans relire le code, pourquoi chaque
      dossier de `src/` existe

Si tu coches tout ça honnêtement, tu es sorti du "je galère sur tout".
