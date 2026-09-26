# Projet 01 — Gestion de stock

| Paramètre | Valeur |
|---|---|
| Niveau | Débutant |
| Prérequis | Variables, boucles, conditions, fonctions PHP, bases SQL |
| Stack | PHP natif (POO simple), PDO, MySQL ou SQLite |
| Durée indicative | 6 à 10 heures |

## Contexte

Tu tiens la boutique de quartier d'un ami parti en voyage pour un mois. Il t'a laissé les clés et une consigne : « ne me perds pas de stock ». Tu dois construire l'outil qui te permet de savoir à tout moment ce qu'il y a en rayon, ce qui manque, et ce qui a été vendu.

## Objectifs pédagogiques

- Manipuler un CRUD complet en PHP + PDO (requêtes préparées obligatoires).
- Séparer la logique métier de l'affichage dès ce premier projet.
- Comprendre pourquoi une règle métier ne doit jamais vivre dans le formulaire HTML.
- Pratiquer les migrations SQL écrites à la main (pas d'ORM).

## Entités et fonctionnalités attendues

**Produit** : nom, référence unique, catégorie, prix d'achat, prix de vente, quantité en stock, seuil d'alerte.
**Mouvement de stock** : produit, type (entrée/sortie), quantité, motif, date.

- Lister les produits avec leur quantité actuelle.
- Créer, modifier, désactiver un produit (jamais de suppression physique si des mouvements existent).
- Enregistrer une entrée de stock (réception fournisseur) et une sortie (vente, casse, don).
- Afficher les produits sous le seuil d'alerte.
- Calculer la valeur totale du stock (quantité × prix d'achat).

## Règles métier

1. Une référence produit est unique et ne change jamais après création.
2. Une sortie ne peut pas faire passer la quantité en négatif.
3. Un produit désactivé n'apparaît plus dans les formulaires de vente mais reste visible dans l'historique.
4. Chaque mouvement de stock est horodaté et irréversible : pour corriger une erreur, on crée un mouvement inverse, on ne modifie jamais un mouvement existant.
5. Le prix de vente doit toujours être strictement supérieur au prix d'achat (sinon avertissement, pas blocage — documente ton choix).

## Contraintes d'architecture

- Dossiers séparés : `src/Model`, `src/Repository`, `src/Service`, `public/` (points d'entrée).
- Le point d'entrée HTTP ne contient aucune requête SQL directe.
- La règle « pas de quantité négative » vit dans le Service, jamais dans le formulaire ni dans le Repository.
- Connexion PDO centralisée dans une seule classe/fonction, jamais recréée à chaque requête.

## Modèle de données à concevoir

Dessine ton schéma (tables, clés primaires/étrangères, contraintes) avant d'écrire la première migration. Réfléchis en particulier à comment un mouvement de stock référence un produit, et si le stock affiché est une colonne stockée ou une valeur recalculée depuis l'historique des mouvements (les deux approches ont des avantages — choisis et justifie).

## Questions de découverte

1. Pourquoi utiliser des requêtes préparées et pas de la concaténation de chaînes ?
2. Que se passe-t-il si deux sorties de stock sont enregistrées au même instant sur le même produit ?
3. Stocker la quantité comme colonne calculée à chaque fois ou la déduire de l'historique : quel est le compromis ?

## Scénarios d'acceptation

| # | Scénario | Résultat attendu |
|---|---|---|
| 1 | Sortie de 5 unités sur un produit qui en a 10 | Stock ramené à 5, mouvement enregistré |
| 2 | Sortie de 20 unités sur un produit qui en a 10 | Refus, message clair, aucun mouvement créé |
| 3 | Création d'un produit avec une référence déjà existante | Refus avec message explicite |
| 4 | Désactivation d'un produit ayant un historique | Produit masqué des formulaires, historique conservé |

## Livrables

- Dépôt Git avec historique de commits atomiques et messages clairs.
- Schéma de base de données (image ou fichier `.sql`).
- Code PHP structuré en couches.
- Notes de progression personnelles (difficultés, choix, ce que tu changerais).

## Checklist d'auto-évaluation

- [ ] Aucune requête SQL en dehors du Repository.
- [ ] Aucune règle métier dans un fichier de vue/formulaire.
- [ ] Impossible d'obtenir un stock négatif, même en testant volontairement.
- [ ] Chaque commit correspond à une intention claire et testable.

## Bonus

- Export CSV de l'inventaire.
- Historique filtrable par période et par produit.
- Alerte visuelle (couleur) selon le niveau de stock.
