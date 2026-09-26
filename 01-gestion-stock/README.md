# Projet 01 — Gestion de stock

## Contexte

Une petite boutique a besoin d'une application web pour suivre son stock :
quels produits elle a, combien il en reste, et l'historique des entrées et
sorties.

## Objectifs pédagogiques

- Modéliser une base de données relationnelle simple (2-3 tables liées)
- Écrire une logique métier qui protège l'intégrité des données (le stock ne
  doit jamais devenir incohérent)
- Structurer un projet PHP en couches dès le départ
- Prendre l'habitude du commit atomique

## Fonctionnalités attendues

- Lister les produits avec leur stock actuel
- Ajouter un nouveau produit (nom, catégorie, prix, seuil d'alerte, stock
  initial)
- Enregistrer un mouvement d'entrée (réapprovisionnement)
- Enregistrer un mouvement de sortie (vente)
- Consulter l'historique des mouvements d'un produit
- Afficher une alerte visuelle pour les produits sous leur seuil

## Règles métier

- Un mouvement de sortie qui ferait passer le stock sous 0 doit être refusé.
- Chaque mouvement (entrée ou sortie) doit être historisé avec sa date et sa
  quantité — jamais de modification directe du stock sans passer par un
  mouvement.
- Le stock affiché d'un produit doit toujours être calculable/cohérent avec
  la somme de ses mouvements.
- Un produit avec un stock à 0 reste visible (pas de suppression), mais ne
  doit plus pouvoir faire l'objet d'une sortie.
- Le seuil d'alerte est défini par produit (pas une constante globale).

## Contraintes techniques

- PHP 8.2+, architecture en couches (Controller / Service / Repository /
  Model / Validation)
- Base de données relationnelle (MySQL ou PostgreSQL, au choix)
- Au moins 5 tests unitaires sur la logique métier (service de mouvement de
  stock), sans connexion base de données
- Git avec un historique de commits lisible

## Livrables attendus

- Le code source, avec architecture en couches respectée
- Un schéma de base de données (script SQL ou diagramme) dans le repo
- Un `README.md` de projet expliquant comment l'installer et le lancer
- `docker-compose.yml` fonctionnel (app + base de données)
- Des tests unitaires qui passent

## Checklist d'auto-évaluation

- [ ] Aucune requête SQL écrite en dehors d'un Repository
- [ ] Aucune règle métier écrite en dehors d'un Service
- [ ] Un mouvement de sortie invalide lève une erreur claire, pas un crash
- [ ] L'historique de mouvements permet de retrouver le stock à tout moment
- [ ] `docker-compose up` lance l'appli fonctionnelle en une commande
- [ ] Historique Git compréhensible sans avoir à demander "c'était quoi ce
      commit déjà ?"

## Bonus (si tu veux aller plus loin)

- Ajouter des catégories de produits avec filtrage
- Export CSV de l'historique des mouvements
- Endpoint JSON en plus des vues HTML (prépare le projet 05)
