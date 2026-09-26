# Projet 02 — Gestionnaire de tâches

| Paramètre | Valeur |
|---|---|
| Niveau | Débutant/Intermédiaire |
| Prérequis | Projet 01 terminé |
| Stack | PHP natif (POO), PDO, MySQL |
| Durée indicative | 8 à 12 heures |

## Contexte

Ton équipe (fictive, pour l'instant c'est juste toi et ton clavier) a besoin d'un outil pour suivre l'avancement des tâches d'un projet : ce qui est à faire, en cours, terminé, ou bloqué.

## Objectifs pédagogiques

- Modéliser des transitions d'état strictes (petite machine à états).
- Construire des filtres combinables (statut, priorité, échéance).
- Organiser un code qui commence à avoir plusieurs responsabilités croisées.

## Entités et fonctionnalités attendues

**Tâche** : titre, description, statut, priorité, date d'échéance, date de création, date de complétion.

Statuts autorisés : `à_faire`, `en_cours`, `terminée`, `bloquée`.

- Créer, modifier, lister les tâches.
- Changer le statut d'une tâche en respectant les transitions autorisées.
- Filtrer par statut, priorité, et tâches en retard (échéance dépassée et non terminée).
- Archiver une tâche terminée depuis plus de 30 jours (sans la supprimer).

## Règles métier

1. Transitions autorisées : `à_faire → en_cours`, `en_cours → terminée`, `en_cours → bloquée`, `bloquée → en_cours`. Toute autre transition est refusée.
2. Une tâche `terminée` ne peut plus être modifiée, sauf réouverture explicite vers `en_cours`.
3. Une date d'échéance ne peut pas être dans le passé à la création.
4. Une tâche `bloquée` doit obligatoirement avoir un motif de blocage renseigné.
5. Le passage à `terminée` enregistre automatiquement la date de complétion.

## Contraintes d'architecture

- Isole la logique de transition d'état dans une classe dédiée (`TaskStatusTransition` ou équivalent) — le Service l'appelle, il ne réimplémente pas la logique.
- Les filtres se composent : le Repository doit accepter plusieurs critères combinés sans que tu dupliques une méthode par combinaison.
- Aucune chaîne de statut en dur dispersée dans le code : centralise les valeurs autorisées à un seul endroit.

## Modèle de données à concevoir

Réfléchis à la façon de stocker le statut (chaîne contrainte vs table de référence) et à comment tu gardes une trace du motif de blocage sans complexifier inutilement le schéma.

## Questions de découverte

1. Pourquoi centraliser les transitions d'état plutôt que de vérifier `if ($statut == '...')` à chaque endroit du code ?
2. Comment un filtre combiné (statut + priorité + en retard) se traduit-il en SQL sans dupliquer les requêtes ?
3. Que change le fait qu'une tâche terminée soit « verrouillée » sur la conception de ton formulaire d'édition ?

## Scénarios d'acceptation

| # | Scénario | Résultat attendu |
|---|---|---|
| 1 | Passage de `à_faire` à `terminée` directement | Refus, transition non autorisée |
| 2 | Passage à `bloquée` sans motif | Refus |
| 3 | Filtre « en retard » sur une tâche `terminée` en retard | Non affichée (elle est terminée, donc pas « en retard ») |
| 4 | Réouverture d'une tâche terminée | Statut repasse à `en_cours`, date de complétion effacée |

## Livrables

Identiques au projet 01, plus un schéma explicite de la machine à états (diagramme simple, texte ou image).

## Checklist d'auto-évaluation

- [ ] Toute transition interdite est refusée avec un message clair.
- [ ] Aucun statut écrit en dur en dehors de la classe centrale.
- [ ] Les filtres se combinent sans duplication de code.

## Bonus

- Historique des changements de statut (qui/quand, même sans authentification réelle — un champ texte suffit).
- Tri par priorité puis échéance.
- Sous-tâches simples.
