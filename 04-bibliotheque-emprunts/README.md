# Projet 04 — Bibliothèque avec emprunts et pénalités

| Paramètre | Valeur |
|---|---|
| Niveau | Intermédiaire |
| Prérequis | Projet 03 terminé |
| Stack | PHP natif (POO), PDO, MySQL |
| Durée indicative | 12 à 16 heures |

## Contexte

La petite bibliothèque de quartier gère encore ses prêts sur un cahier papier. Les retards ne sont jamais suivis, et personne ne sait combien d'exemplaires d'un même livre sont réellement disponibles. Tu digitalises tout ça.

## Objectifs pédagogiques

- Modéliser une relation à trois niveaux (Livre → Exemplaire → Emprunt) sans tout aplatir.
- Calculer des pénalités dérivées d'une donnée (le retard), pas stockées directement.
- Gérer un cycle de vie complet : disponibilité → emprunt → retour → pénalité éventuelle.

## Entités et fonctionnalités attendues

**Livre** : titre, auteur, ISBN, catégorie.
**Exemplaire** : livre, numéro d'inventaire, état (`disponible`, `emprunté`, `perdu`, `retiré`).
**Membre** : nom, contact, statut (`actif`, `suspendu`).
**Emprunt** : exemplaire, membre, date_emprunt, date_retour_prévue, date_retour_réelle (nulle tant que non rendu).

- Un livre peut avoir plusieurs exemplaires physiques.
- Emprunter un exemplaire disponible.
- Enregistrer le retour d'un exemplaire.
- Calculer la pénalité d'un retard au moment du retour.
- Lister les emprunts en cours et en retard.
- Suspendre automatiquement un membre ayant une pénalité impayée au-delà d'un seuil.

## Règles métier

1. Un exemplaire ne peut être emprunté que s'il est `disponible`.
2. Un membre `suspendu` ne peut emprunter aucun exemplaire.
3. La durée d'emprunt standard est de 14 jours (constante configurable, pas codée en dur partout).
4. Un membre ne peut avoir plus de 3 emprunts actifs simultanément.
5. La pénalité est calculée en fonction du nombre de jours de retard au moment du retour (ex : montant fixe par jour de retard) — définis et documente ta formule.
6. Un exemplaire rendu redevient `disponible`, sauf s'il est déclaré `perdu`.
7. Un membre est suspendu automatiquement si le cumul de ses pénalités impayées dépasse un seuil que tu définis et justifies.

## Contraintes d'architecture

- Le calcul de pénalité est une fonction pure et testable indépendamment de la base (donne-lui une date d'emprunt prévue et une date de retour réelle, elle retourne un montant).
- Aucune requête Eloquent-like en dur dans les vues : les disponibilités affichées viennent d'une méthode du Repository, jamais d'un comptage fait à la volée dans le contrôleur.
- La suspension automatique est déclenchée par un Service dédié, appelé après chaque retour — pas par une tâche cron externe à ce stade (garde ça pour un bonus).

## Modèle de données à concevoir

Réfléchis à pourquoi on modélise un `Exemplaire` séparément d'un `Livre` (plusieurs copies physiques du même titre), et à comment relier proprement Emprunt, Exemplaire et Membre avec les bonnes clés étrangères et contraintes d'unicité (un exemplaire ne peut avoir qu'un seul emprunt actif à la fois).

## Questions de découverte

1. Pourquoi ne pas stocker directement le montant de la pénalité au lieu de le recalculer à partir des dates ?
2. Que se passe-t-il si on modifie la durée standard d'emprunt après coup — quels emprunts en cours sont affectés ?
3. Pourquoi séparer Livre et Exemplaire plutôt que de mettre une simple colonne « quantité » sur Livre ?

## Scénarios d'acceptation

| # | Scénario | Résultat attendu |
|---|---|---|
| 1 | Emprunt d'un exemplaire disponible par un membre actif ayant 2 emprunts en cours | Accepté |
| 2 | Emprunt d'un 4ᵉ exemplaire par le même membre | Refus |
| 3 | Emprunt par un membre suspendu | Refus |
| 4 | Retour avec 5 jours de retard | Pénalité calculée et associée à l'emprunt |
| 5 | Retour à temps | Aucune pénalité, exemplaire redevient disponible |
| 6 | Cumul de pénalités dépassant le seuil | Membre automatiquement suspendu |

## Livrables

Identiques aux précédents, plus des tests unitaires isolés sur le calcul de pénalité (sans base de données).

## Checklist d'auto-évaluation

- [ ] Le calcul de pénalité est testé unitairement, sans dépendre de PDO.
- [ ] Impossible d'emprunter un exemplaire déjà emprunté.
- [ ] La limite de 3 emprunts actifs est vérifiée par un test.

## Bonus

- File d'attente de réservation sur un livre entièrement emprunté.
- Notification (simulée, log ou e-mail simple) avant l'échéance.
- Rapport mensuel des retards par membre.
