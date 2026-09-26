# Projet 04 — Bibliothèque avec emprunts et pénalités

## Contexte

Une bibliothèque gère un catalogue de livres (avec plusieurs exemplaires par
titre) et les emprunts de ses adhérents, avec calcul de retard.

## Objectifs pédagogiques

- Gérer une relation "exemplaires multiples d'une même œuvre"
- Faire des calculs métier basés sur des dates (pénalités de retard)
- Renforcer la rigueur sur les états d'un emprunt

## Fonctionnalités attendues

- Catalogue de livres (titre, auteur, ISBN, nombre d'exemplaires total)
- Emprunter un exemplaire disponible
- Retourner un exemplaire emprunté
- Lister les emprunts en cours d'un adhérent, avec ceux en retard mis en
  évidence
- Calculer et afficher la pénalité due pour un retour en retard

## Règles métier

- Un adhérent ne peut pas emprunter un titre dont tous les exemplaires sont
  déjà empruntés.
- Un adhérent ne peut pas avoir plus de 3 emprunts en cours simultanément.
- La durée d'emprunt standard est de 14 jours.
- Une pénalité de 100 F par jour de retard s'applique au retour, jusqu'à un
  plafond de 3000 F.
- Un adhérent avec une pénalité impayée ne peut pas emprunter de nouveau
  livre tant qu'il n'a pas régularisé.

## Contraintes techniques

- PHP 8.2+, architecture en couches
- Modélisation correcte de la relation titre ↔ exemplaires ↔ emprunts (pas
  un simple champ "disponible" booléen sur le livre)
- Tests unitaires sur : le calcul de pénalité (plusieurs cas de durée de
  retard, y compris pile 14 jours = pas de pénalité), la limite des 3
  emprunts, le blocage si pénalité impayée

## Livrables attendus

- Code source en couches
- Schéma de base de données
- `README.md` de projet
- `docker-compose.yml` fonctionnel
- Tests unitaires couvrant les règles ci-dessus

## Checklist d'auto-évaluation

- [ ] Le calcul de pénalité est isolé dans une fonction/méthode testable
      indépendamment du reste
- [ ] Le plafond de pénalité est bien appliqué (teste un retard de 60 jours)
- [ ] La distinction "exemplaire" vs "titre" est claire dans le schéma —
      deux copies du même livre ont des lignes séparées
- [ ] Un adhérent bloqué ne peut pas contourner le blocage en modifiant
      l'URL du formulaire d'emprunt

## Bonus

- Système de réservation : un adhérent peut réserver un titre actuellement
  indisponible et être notifié (log) quand un exemplaire se libère
- Historique complet des emprunts d'un adhérent, même terminés
