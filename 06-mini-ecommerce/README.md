# Projet 06 — Mini e-commerce

| Paramètre | Valeur |
|---|---|
| Niveau | Avancé |
| Prérequis | Projets 01 à 05 terminés |
| Stack | PHP natif (POO), PDO, MySQL, transactions SQL |
| Durée indicative | 15 à 20 heures |

## Contexte

Une petite marque locale veut vendre ses produits en ligne. Panier, commande, paiement (simulé) : il faut que deux clients ne puissent jamais acheter le dernier exemplaire du même produit en même temps sans que l'un des deux soit bloqué proprement.

## Objectifs pédagogiques

- Utiliser de vraies transactions SQL (`BEGIN`/`COMMIT`/`ROLLBACK`) pour garantir la cohérence.
- Gérer un panier qui vit sur plusieurs requêtes (session).
- Réserver du stock au moment de la commande, pas juste au moment de l'ajout au panier.

## Entités et fonctionnalités attendues

**Produit** : nom, prix, stock disponible.
**Panier** : lié à une session (pas besoin d'authentification complète ici), lignes de panier (produit, quantité).
**Commande** : client (nom, e-mail), lignes de commande (produit, quantité, prix au moment de l'achat), statut (`en_attente`, `payée`, `annulée`), total.

- Ajouter/retirer un produit du panier, modifier une quantité.
- Passer commande à partir du panier.
- Simuler un paiement (un simple bouton qui bascule le statut, pas de vraie passerelle).
- Annuler une commande `en_attente` et restituer le stock réservé.

## Règles métier

1. Le prix enregistré dans une ligne de commande est celui du produit au moment de l'achat, pas une référence dynamique au prix actuel (le prix du produit peut changer après coup).
2. Le stock est décrémenté au moment de la validation de la commande, pas au moment de l'ajout au panier.
3. Une commande ne peut être créée que si tous les produits du panier ont un stock suffisant au moment de la validation.
4. Le total de la commande est recalculé et vérifié côté serveur, jamais fait confiance à une valeur envoyée par le client.
5. L'annulation d'une commande `en_attente` restitue le stock ; une commande `payée` ne peut plus être annulée directement (documente ce que tu ferais pour un remboursement, sans nécessairement l'implémenter).

## Contraintes d'architecture

- La création de commande (vérification du stock + décrémentation + création des lignes) se fait dans une transaction SQL unique : soit tout réussit, soit rien n'est appliqué.
- Le Service de commande ne fait confiance à aucune donnée de prix venant du panier client sans la revérifier contre la base au moment de la validation.
- Le panier vit dans une classe dédiée, indépendante de `$_SESSION` directement (facilite les tests).

## Modèle de données à concevoir

Réfléchis à pourquoi une ligne de commande duplique le prix du produit plutôt que de simplement pointer vers le produit, et à comment garantir qu'une décrémentation de stock concurrente ne fait jamais passer le stock en négatif (contrainte SQL, verrouillage, ou vérification applicative — choisis et justifie).

## Questions de découverte

1. Que se passe-t-il si deux clients valident une commande sur le dernier exemplaire d'un produit à la même seconde ? Comment ta transaction protège-t-elle contre ça ?
2. Pourquoi dupliquer le prix dans la ligne de commande est une bonne pratique et pas une redondance inutile ?
3. Pourquoi ne jamais recalculer le total à partir de ce qu'envoie le formulaire côté client ?

## Scénarios d'acceptation

| # | Scénario | Résultat attendu |
|---|---|---|
| 1 | Commande avec stock suffisant pour tous les produits | Commande créée, stock décrémenté |
| 2 | Commande avec un produit en rupture entre l'ajout au panier et la validation | Refus clair, aucune décrémentation partielle |
| 3 | Prix du produit modifié après ajout au panier | La commande utilise le prix du moment de la validation, enregistré tel quel |
| 4 | Annulation d'une commande `en_attente` | Stock restitué |
| 5 | Tentative d'annulation d'une commande `payée` | Refus |

## Livrables

Identiques aux précédents, plus un test qui simule explicitement une tentative de commande concurrente sur un stock limité (même en solo, écris le scénario).

## Checklist d'auto-évaluation

- [ ] La création de commande est bien dans une transaction avec rollback testé.
- [ ] Aucune confiance faite à un prix ou un total venant du client.
- [ ] Le stock ne peut jamais devenir négatif, même en testant volontairement.

## Bonus

- Codes promo simples (pourcentage ou montant fixe).
- Historique des commandes par e-mail client.
- Facture PDF générée à la validation.
