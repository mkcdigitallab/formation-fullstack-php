# Projet 05 — API de dépenses partagées

## Contexte

Un groupe de colocataires ou d'amis veut suivre qui a payé quoi lors de
sorties/achats communs, et savoir qui doit combien à qui — sans interface
graphique cette fois : uniquement une **API JSON**.

## Objectifs pédagogiques

- Concevoir et exposer une vraie API REST (codes HTTP corrects, JSON en
  entrée/sortie, pas de vues HTML)
- Écrire un algorithme de calcul (répartition de dettes) — le morceau de
  "logique pure" le plus exigeant de la formation jusqu'ici
- Manipuler des montants sans erreur d'arrondi

## Fonctionnalités attendues (endpoints)

- `POST /groupes` — créer un groupe avec ses membres
- `POST /groupes/{id}/depenses` — enregistrer une dépense (qui a payé,
  montant, participants concernés)
- `GET /groupes/{id}/depenses` — lister les dépenses du groupe
- `GET /groupes/{id}/soldes` — calculer combien chaque membre doit ou est dû
- `GET /groupes/{id}/remboursements` — proposer la liste minimale de
  virements pour tout solder

## Règles métier

- Une dépense est répartie équitablement entre les participants désignés
  (pas forcément tous les membres du groupe).
- Les montants sont manipulés en centimes (entiers), jamais en float, pour
  éviter les erreurs d'arrondi.
- Le calcul des soldes doit être exact : la somme de tous les soldes du
  groupe doit toujours être égale à 0.
- L'algorithme de remboursement doit minimiser le nombre de transactions
  nécessaires pour tout solder (pas juste "chacun rembourse chacun").
- Toute requête mal formée renvoie un code HTTP 422 avec un message JSON
  explicite, jamais un crash PHP brut.

## Contraintes techniques

- PHP 8.2+, architecture en couches, réponses en JSON strict
- Codes HTTP corrects : 201 (création), 200 (lecture), 404 (groupe
  inexistant), 422 (validation)
- Tests unitaires sur l'algorithme de répartition et de minimisation des
  remboursements — c'est le cœur du projet, à tester lourdement avec
  plusieurs scénarios (3 personnes, 5 personnes, montants qui ne se
  divisent pas rond)

## Livrables attendus

- Code source en couches
- Schéma de base de données
- `README.md` de projet avec exemples de requêtes (curl ou Postman)
- `docker-compose.yml` fonctionnel
- Tests unitaires sur les calculs

## Checklist d'auto-évaluation

- [ ] Aucun `float` utilisé pour manipuler de l'argent (uniquement des
      entiers en centimes)
- [ ] La somme des soldes d'un groupe est testée = 0 sur au moins 3
      scénarios différents
- [ ] Le nombre de remboursements proposés est minimal (teste : est-ce
      qu'un cas à 4 personnes donne bien 3 transactions max, pas 6 ?)
- [ ] Chaque endpoint renvoie le bon code HTTP, testé explicitement

## Bonus

- Authentification simple par token pour sécuriser l'API
- Export des soldes en CSV
- Pagination sur la liste des dépenses
