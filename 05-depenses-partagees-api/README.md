# Projet 05 — API de dépenses partagées

| Paramètre | Valeur |
|---|---|
| Niveau | Intermédiaire/Avancé |
| Prérequis | Projet 04 terminé, bases HTTP (verbes, codes de statut) |
| Stack | PHP natif (pas de framework), PDO, MySQL, réponses JSON pures |
| Durée indicative | 12 à 18 heures |

## Contexte

Toi et trois amis partagez un voyage. Chacun paie des choses différentes (hôtel, essence, courses) et à la fin il faut savoir qui doit combien à qui, sans y passer la soirée avec une calculatrice. Cette fois, pas d'interface web — uniquement une API JSON que n'importe quel client (mobile, front séparé, Postman) peut consommer.

## Objectifs pédagogiques

- Construire une API REST sans framework : routeur maison, codes de statut HTTP corrects, réponses JSON structurées.
- Implémenter un algorithme de répartition de dépenses et de calcul de soldes.
- Gérer les erreurs API (400, 404, 422) de façon cohérente plutôt qu'avec des pages d'erreur HTML.

## Entités et fonctionnalités attendues

**Groupe** : nom, membres.
**Membre** : nom (appartient à un groupe).
**Dépense** : groupe, payeur (membre), montant, libellé, date, liste des bénéficiaires (par défaut tous les membres du groupe, à parts égales).

Endpoints attendus (exemples, adapte les chemins) :
- `POST /groupes` — créer un groupe.
- `POST /groupes/{id}/membres` — ajouter un membre.
- `POST /groupes/{id}/depenses` — enregistrer une dépense.
- `GET /groupes/{id}/depenses` — lister les dépenses.
- `GET /groupes/{id}/soldes` — calculer qui doit combien à qui.

## Règles métier

1. Une dépense doit avoir un montant strictement positif.
2. Le payeur doit être membre du groupe concerné.
3. Les bénéficiaires listés doivent tous être membres du groupe.
4. La répartition par défaut est à parts égales entre bénéficiaires ; les centimes non divisibles exactement sont attribués selon une règle que tu définis et documentes (ex : au payeur, ou au premier bénéficiaire par ordre alphabétique).
5. Le calcul des soldes doit simplifier les dettes : si A doit 10 à B et B doit 10 à C, l'algorithme doit pouvoir réduire ça à « A doit 10 à C » plutôt que d'afficher les deux dettes séparément (réfléchis à un algorithme simple de compensation, pas besoin d'optimalité parfaite).

## Contraintes d'architecture

- Toutes les réponses sont en JSON, avec un code de statut HTTP cohérent (`201` à la création, `404` si la ressource n'existe pas, `422` pour une erreur de validation métier).
- Le routeur (fichier unique ou petite classe) fait correspondre méthode + chemin à un contrôleur, sans dupliquer de logique.
- Le calcul de solde est isolé dans une classe/fonction pure, testable avec un jeu de dépenses fourni en entrée, indépendamment de la base.
- Aucun `echo` de HTML nulle part dans ce projet.

## Modèle de données à concevoir

Réfléchis à comment stocker la relation « une dépense a plusieurs bénéficiaires » (table de liaison) et à la précision numérique à utiliser pour les montants (jamais de flottant pour de l'argent — documente ton choix : entiers en centimes, `DECIMAL`, etc.).

## Questions de découverte

1. Pourquoi ne jamais utiliser `float` pour des montants d'argent ?
2. Quelle différence entre une erreur 400, une erreur 404 et une erreur 422 — donne un exemple précis pour chacune dans ce projet.
3. Comment ton algorithme de simplification des dettes se comporte-t-il avec un groupe de 5 personnes et 10 dépenses croisées ?

## Scénarios d'acceptation

| # | Scénario | Résultat attendu |
|---|---|---|
| 1 | Création d'une dépense avec un montant négatif | 422, message clair |
| 2 | Dépense avec un payeur hors du groupe | 422 |
| 3 | Requête sur un groupe inexistant | 404 |
| 4 | Trois dépenses croisées entre 3 membres | Soldes finaux corrects et simplifiés |
| 5 | Dépense de 10 € partagée entre 3 personnes | Répartition cohérente avec la règle des centimes définie |

## Livrables

Identiques aux précédents, plus une collection Postman/Insomnia (ou fichier `.http`) documentant chaque endpoint avec un exemple de requête et de réponse.

## Checklist d'auto-évaluation

- [ ] Chaque endpoint renvoie le bon code HTTP dans les cas d'erreur.
- [ ] Le calcul de solde est testé unitairement avec plusieurs jeux de données.
- [ ] Aucun montant n'est manipulé en `float`.

## Bonus

- Authentification par clé API simple.
- Export du solde en CSV.
- Pagination sur la liste des dépenses.
