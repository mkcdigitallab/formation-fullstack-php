# Projet 03 — Réservation de rendez-vous

## Contexte

Un salon (coiffure, cabinet médical, ou autre — à toi de choisir le thème)
propose plusieurs praticiens, chacun avec son propre agenda. Les clients
prennent rendez-vous en ligne.

C'est volontairement proche de ce qu'on a analysé ensemble dans
`Reservation-Salles`, mais avec une contrainte en plus : **plusieurs
ressources en parallèle** (plusieurs praticiens), ce qui complique la
détection de conflit.

## Objectifs pédagogiques

- Gérer des conflits temporels sur plusieurs ressources indépendantes
- Renforcer les réflexes déjà vus (DTO, Validator, Repository, Service) sur
  un cas plus riche
- Introduire des créneaux avec durée variable selon le type de prestation

## Fonctionnalités attendues

- Lister les praticiens et les prestations qu'ils proposent (avec durée)
- Afficher les créneaux disponibles d'un praticien sur une journée donnée
- Prendre un rendez-vous (client, praticien, prestation, date/heure)
- Annuler un rendez-vous
- Empêcher la prise de deux rendez-vous qui se chevauchent pour un même
  praticien

## Règles métier

- La durée du rendez-vous est déterminée par la prestation choisie (pas
  saisie librement par le client).
- Deux rendez-vous confirmés chez le même praticien ne peuvent pas se
  chevaucher ; des rendez-vous adjacents sont autorisés.
- Un rendez-vous ne peut pas être pris en dehors des horaires d'ouverture
  (à définir, ex. 8h-18h) ni un jour de fermeture du praticien.
- Un rendez-vous doit être pris au moins 1h à l'avance.
- Un rendez-vous annulé libère immédiatement le créneau.

## Contraintes techniques

- PHP 8.2+, architecture en couches
- FastRoute + PHP-DI (comme Reservation-Salles) ou équivalent de ton choix
- Base de données relationnelle avec au moins 4 tables liées (praticien,
  prestation, rendez-vous, client)
- Tests unitaires sur la détection de conflit avec plusieurs praticiens en
  parallèle (vérifier qu'un conflit chez A n'empêche pas une prise chez B)

## Livrables attendus

- Code source en couches
- Schéma de base de données
- `README.md` de projet
- `docker-compose.yml` fonctionnel
- Tests unitaires, y compris sur les cas limites (chevauchement exact aux
  bornes, créneaux adjacents)

## Checklist d'auto-évaluation

- [ ] La détection de conflit est testée avec au moins 2 praticiens
      différents pour vérifier qu'elle ne mélange pas leurs agendas
- [ ] Le cas limite "10h-11h puis 11h-12h" est explicitement testé et accepté
- [ ] Le cas limite "10h-11h et 10h30-11h30" est explicitement testé et
      refusé
- [ ] Aucun horaire hors ouverture n'est acceptable, même en modifiant le
      formulaire côté client

## Bonus

- Génération automatique des créneaux disponibles (pas juste validation à la
  prise)
- Rappel par email simulé (log dans un fichier) la veille du rendez-vous
