# Projet 06 — Mini e-commerce

## Contexte

Une boutique en ligne simple : catalogue, panier, commande. C'est le projet
qui combine tout ce que tu as musclé jusqu'ici (stock, logique métier,
transactions) dans un flux utilisateur complet.

## Objectifs pédagogiques

- Gérer un panier en session, indépendant de la base tant qu'il n'est pas
  validé
- Garantir la cohérence des données lors d'une opération à plusieurs étapes
  (transaction de base de données)
- Relier ce projet au Projet 01 (le stock doit vraiment se décrémenter à la
  commande)

## Fonctionnalités attendues

- Catalogue de produits avec stock (réutilise/adapte le Projet 01)
- Ajouter/retirer un produit du panier, modifier les quantités
- Passer commande : transformation du panier en commande figée
- Suivi du statut d'une commande : en attente → confirmée → expédiée →
  livrée (ou annulée)
- Historique des commandes d'un client

## Règles métier

- Au moment de la validation de la commande (pas avant), le stock de chaque
  produit est vérifié et décrémenté. Si un produit n'a plus assez de stock
  à ce moment précis, la commande entière est refusée (rien n'est
  partiellement décrémenté).
- Une commande "annulée" doit restituer le stock des produits qu'elle avait
  décrémenté.
- Le prix figé dans la commande est celui du moment de l'achat, même si le
  prix catalogue change ensuite.
- Une commande ne peut pas passer directement de "en attente" à "livrée" —
  chaque étape du statut doit être respectée dans l'ordre.
- Un panier vide ne peut pas être validé en commande.

## Contraintes techniques

- PHP 8.2+, architecture en couches
- Utiliser une transaction de base de données pour l'opération "validation
  de commande" (tout ou rien)
- Panier géré en session PHP (pas en base tant qu'il n'est pas validé)
- Tests unitaires sur : le refus de commande si stock insuffisant, la
  restitution de stock à l'annulation, le blocage des transitions de statut
  invalides

## Livrables attendus

- Code source en couches
- Schéma de base de données
- `README.md` de projet
- `docker-compose.yml` fonctionnel
- Tests unitaires, y compris un test qui simule un conflit (stock
  insuffisant au moment précis de la validation)

## Checklist d'auto-évaluation

- [ ] La décrémentation du stock et la création de la commande se font dans
      une seule transaction — teste explicitement qu'un échec n'en valide
      pas la moitié
- [ ] Le prix enregistré dans une commande passée ne change jamais après
      coup, même si tu modifies le prix catalogue
- [ ] Annuler une commande restitue exactement le bon stock, testé
- [ ] Aucune transition de statut illégale n'est possible

## Bonus

- Codes promo avec réduction (pourcentage ou montant fixe)
- Facture PDF générée à la commande
