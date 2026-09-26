# Projet 02 — Gestionnaire de tâches

## Contexte

Une application de suivi de tâches type "to-do" mais avec de vraies règles de
transition d'état — pas juste une case à cocher.

## Objectifs pédagogiques

- Modéliser des transitions d'état strictes (machine à états simple)
- Filtrer/trier des données proprement au niveau du Repository
- Renforcer l'habitude de séparer validation, logique métier et accès aux
  données

## Fonctionnalités attendues

- Créer une tâche (titre, description, priorité, échéance, statut initial
  "à faire")
- Changer le statut d'une tâche : à faire → en cours → terminée, ou → annulée
- Lister les tâches avec filtres : par statut, par priorité, en retard
- Modifier une tâche tant qu'elle n'est pas terminée
- Archiver les tâches terminées depuis plus de 30 jours

## Règles métier

- Une tâche ne peut pas passer directement de "à faire" à "terminée" : elle
  doit obligatoirement passer par "en cours".
- Une tâche "terminée" ou "annulée" ne peut plus être modifiée ni changer de
  statut (état final).
- Une tâche est "en retard" si sa date d'échéance est dépassée et qu'elle
  n'est ni terminée ni annulée.
- La priorité (basse/moyenne/haute/urgente) influence le tri par défaut des
  listes (urgente en premier).
- L'archivage ne supprime rien, il masque juste des vues par défaut.

## Contraintes techniques

- PHP 8.2+, architecture en couches
- Utiliser un enum PHP pour le statut et la priorité (pas des chaînes libres)
- Base de données relationnelle
- Tests unitaires sur : les transitions autorisées/refusées, le calcul "en
  retard", le tri par priorité

## Livrables attendus

- Code source en couches
- Schéma de base de données
- `README.md` de projet
- `docker-compose.yml` fonctionnel
- Tests unitaires

## Checklist d'auto-évaluation

- [ ] Impossible de forcer une transition d'état interdite, même en
      manipulant l'URL/formulaire directement
- [ ] Le calcul "en retard" est dans le Service ou le Model, jamais dupliqué
      dans une vue
- [ ] Les enums sont utilisés pour statut et priorité
- [ ] Les filtres de liste sont gérés au niveau du Repository (pas de
      filtrage en PHP après avoir tout chargé)

## Bonus

- Sous-tâches (une tâche parente ne peut être "terminée" que si toutes ses
  sous-tâches le sont)
- Historique des changements de statut (qui, quand)
- Vue "tableau kanban" en plus de la liste
