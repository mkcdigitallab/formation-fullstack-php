# Projet 07 — Authentification et rôles

## Contexte

Reprends n'importe lequel de tes projets précédents (idéalement le
mini e-commerce ou la gestion de stock) et ajoute une vraie couche
d'authentification avec des rôles différenciés.

## Objectifs pédagogiques

- Implémenter une authentification sécurisée de A à Z (pas juste un
  `if ($password === $input)`)
- Comprendre et appliquer le contrôle d'accès par rôle
- Prendre les bons réflexes de sécurité web (déjà entraperçus avec le CSRF
  de Reservation-Salles, ici on va plus loin)

## Fonctionnalités attendues

- Inscription (email, mot de passe)
- Connexion / déconnexion
- Deux rôles minimum : "client" et "administrateur", avec des pages/actions
  réservées à chacun
- Un administrateur peut voir/gérer des données que les clients ne voient
  pas (ex. tous les produits en gestion de stock, ou tous les emprunts en
  bibliothèque)
- Réinitialisation de mot de passe (simulée : génération d'un lien/token,
  pas d'envoi d'email réel nécessaire)

## Règles métier / sécurité

- Les mots de passe sont hashés avec `password_hash()` (bcrypt/argon2),
  jamais stockés en clair ni en MD5/SHA1.
- Un utilisateur non connecté qui tente d'accéder à une page protégée est
  redirigé vers la connexion, jamais servi avec une erreur brute.
- Un client qui tente d'accéder à une route admin (même en modifiant
  l'URL directement) reçoit un 403, pas une page qui fonctionne à moitié.
- Le token de réinitialisation de mot de passe expire après un délai
  raisonnable (ex. 1h) et n'est utilisable qu'une seule fois.
- Protection CSRF sur tous les formulaires qui modifient des données
  (comme dans Reservation-Salles).

## Contraintes techniques

- PHP 8.2+, architecture en couches
- Middleware ou vérification centralisée pour le contrôle d'accès (pas un
  `if` copié-collé dans chaque contrôleur)
- Tests unitaires sur : le hash/vérification de mot de passe, l'expiration
  du token de réinitialisation, le refus d'accès par rôle

## Livrables attendus

- Code source en couches, intégré à un projet précédent
- Schéma de base de données (table users avec rôle)
- `README.md` de projet
- `docker-compose.yml` fonctionnel
- Tests unitaires sur la sécurité

## Checklist d'auto-évaluation

- [ ] Aucun mot de passe en clair, même dans les logs ou les migrations de
      seed
- [ ] Une route admin testée directement en tant que client renvoie bien un
      refus, pas un affichage partiel
- [ ] Le contrôle d'accès est centralisé, pas répété dans chaque contrôleur
- [ ] Un token de reset expiré est explicitement testé comme refusé

## Bonus

- Connexion avec limitation du nombre de tentatives (anti brute-force)
- Journal des connexions (qui, quand, depuis quelle IP)
