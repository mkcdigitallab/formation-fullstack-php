# Projet 03 — Réservation de rendez-vous multi-praticiens

| Paramètre | Valeur |
|---|---|
| Niveau | Intermédiaire |
| Prérequis | Projets 01 et 02 terminés |
| Stack | PHP natif (POO), PDO, MySQL |
| Durée indicative | 10 à 15 heures |

## Contexte

Un petit cabinet (médical, coiffure, coaching — choisis ton thème) a plusieurs praticiens qui reçoivent des clients sur des créneaux. Il faut empêcher les doubles réservations tout en gérant plusieurs agendas en parallèle.

## Objectifs pédagogiques

- Détecter des conflits temporels par le calcul, pas par inspection visuelle.
- Gérer plusieurs ressources (praticiens) qui partagent une même logique de disponibilité.
- Distinguer validation de forme et invariant métier.

## Entités et fonctionnalités attendues

**Praticien** : nom, spécialité, actif.
**Rendez-vous** : praticien, client (nom, contact), date_début, date_fin, statut (`confirmé`, `annulé`).

- Lister les rendez-vous par praticien et par jour.
- Créer un rendez-vous.
- Annuler un rendez-vous (sans le supprimer).
- Afficher les créneaux libres d'un praticien sur une journée donnée.

## Règles métier

1. Le praticien existe et est actif.
2. La date de début précède la date de fin.
3. La durée est comprise entre 15 minutes et 3 heures.
4. Le rendez-vous commence dans le futur.
5. Aucun rendez-vous confirmé du même praticien ne chevauche la période demandée.
6. Deux créneaux adjacents (fin de l'un = début de l'autre) sont autorisés.
7. Un rendez-vous annulé ne bloque plus le créneau.

Chevauchement : conflit si `nouveau_début < fin_existante` ET `nouvelle_fin > début_existant`.

## Contraintes d'architecture

- La détection de conflit est une requête explicite et testable isolément (pas un `foreach` en PHP sur tous les rendez-vous chargés en mémoire, sauf si tu justifies ce choix pour un petit volume).
- Le contrôleur ne calcule jamais de disponibilité lui-même.
- Un Service `CreateRendezVous` orchestre les 7 règles dans un ordre que tu justifies.

## Modèle de données à concevoir

Pense à l'indexation nécessaire pour que la recherche de chevauchement reste rapide, et à comment tu distingues un rendez-vous annulé d'un rendez-vous actif dans tes requêtes.

## Questions de découverte

1. Pourquoi la formule de chevauchement utilise des inégalités strictes et pas `<=` / `>=` ?
2. Que se passe-t-il si deux utilisateurs réservent le même créneau à quelques millisecondes d'intervalle ? (Réfléchis-y, tu n'es pas obligé de le résoudre complètement ici — note le problème dans ton DevLog.)
3. Pourquoi séparer la requête de disponibilité de la création du rendez-vous plutôt que de tout faire dans une seule méthode ?

## Scénarios d'acceptation

| # | Scénario | Résultat attendu |
|---|---|---|
| 1 | Créneau chevauchant un rendez-vous confirmé | Refus |
| 2 | Créneau adjacent à un rendez-vous existant | Accepté |
| 3 | Créneau identique à un rendez-vous annulé | Accepté |
| 4 | Durée de 4 heures | Refus |
| 5 | Rendez-vous dans le passé | Refus |

## Livrables

Identiques aux précédents, plus un petit texte expliquant ta stratégie de détection de conflit et ses limites.

## Checklist d'auto-évaluation

- [ ] La requête de chevauchement est testée avec au moins les 5 scénarios ci-dessus.
- [ ] Aucun calcul de disponibilité dans le contrôleur.
- [ ] Les créneaux adjacents sont bien acceptés (erreur fréquente à ce niveau).

## Bonus

- Vue « planning de la semaine » par praticien.
- Annulation avec motif obligatoire.
- Empêcher qu'un même client ait deux rendez-vous qui se chevauchent, même chez des praticiens différents.
