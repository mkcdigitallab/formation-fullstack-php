# Formation Full-Stack PHP — Malang Kiya Cissé

Objectif : construire, projet après projet, les réflexes d'un développeur pro —
logique métier propre, organisation en couches, Git/GitHub disciplinés, Docker,
bases de données relationnelles.

## Méthode de travail

Chaque dossier `0X-nom-du-projet/` contient un `README.md` = ton cahier des
charges. **Aucune solution n'est fournie.** Le but n'est pas que le projet
existe, c'est que *toi* tu saches le refaire les yeux fermés dans six mois.

Pour chaque projet :

1. **Lis tout le README avant de coder.** Note sur papier/texte les entités,
   les règles métier, les cas limites.
2. **Dessine ton schéma de base de données** (tables, clés, contraintes) avant
   d'écrire la moindre migration.
3. **Découpe en couches** : Controller / Service / Repository / Model /
   Validation / DTO — comme dans un projet pro. Un contrôleur ne contient
   jamais de logique métier ; un service ne connaît jamais le SQL brut.
4. **Git dès la première minute** : `git init`, `.gitignore` correct, un
   commit par brique fonctionnelle avec un message clair (`feat: ajoute le
   modèle Produit`, pas `wip` ou `update`).
5. **Teste ta logique métier** avec PHPUnit, sans dépendre de la base.
6. **Dockerise en dernier**, une fois que ça marche en local.
7. **Auto-évalue-toi** avec la checklist en bas de chaque README avant de
   passer au projet suivant.

## Progression

| # | Projet | Ce qu'il muscle en priorité |
|---|--------|------------------------------|
| 01 | Gestion de stock | Logique métier de base, CRUD, règles simples |
| 02 | Gestionnaire de tâches | Transitions d'état, filtres, organisation du code |
| 03 | Réservation de rendez-vous | Conflits temporels, règles métier fines |
| 04 | Bibliothèque avec emprunts | Relations complexes, calculs (retards, pénalités) |
| 05 | API de dépenses partagées | API REST/JSON, algorithme de répartition |
| 06 | Mini e-commerce | Transactions, cohérence des données, panier |
| 07 | Authentification et rôles | Sécurité, sessions, contrôle d'accès |
| 08 | Projet final | Tout combiné + CI/CD + Docker complet |

Ne saute pas d'étape. Si un projet te semble trop facile, ajoute les
"Bonus" listés en fin de chaque README — c'est fait exprès pour ça.

## Ce que je peux faire pour toi

- Répondre à des questions précises pendant que tu codes ("pourquoi Eloquent
  fait ça", "comment structurer ce cas limite")
- Relire du code que **toi** tu as écrit et te dire ce qui cloche
- Débugger avec toi (pas à ta place)

Je n'écrirai pas les projets à ta place — c'est tout l'intérêt de ce dépôt.
