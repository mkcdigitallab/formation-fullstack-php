# Projet 07 — Authentification et rôles

| Paramètre | Valeur |
|---|---|
| Niveau | Avancé |
| Prérequis | Projet 06 terminé |
| Stack | PHP natif (POO), PDO, MySQL, sessions PHP natives |
| Durée indicative | 12 à 16 heures |

## Contexte

Reprends un des projets précédents (ou une base neuve) et transforme-le en application multi-utilisateurs : chacun a un compte, un rôle, et ne voit/ne fait que ce que son rôle autorise.

## Objectifs pédagogiques

- Implémenter une authentification sécurisée de bout en bout (hash, sessions, déconnexion).
- Construire un contrôle d'accès basé sur les rôles (RBAC) réutilisable.
- Comprendre les vecteurs d'attaque courants (session fixation, CSRF, injection) et s'en protéger.

## Entités et fonctionnalités attendues

**Utilisateur** : nom, e-mail unique, mot de passe (haché), rôle (`admin`, `gestionnaire`, `utilisateur`), actif.

- Inscription, connexion, déconnexion.
- Page de profil (modifier son propre mot de passe).
- Zone `admin` accessible uniquement aux administrateurs (ex : liste des utilisateurs, activation/désactivation de comptes).
- Middleware/garde qui protège chaque route selon le rôle requis.

## Règles métier

1. Le mot de passe est toujours haché avec `password_hash()` (jamais de MD5, SHA1, ou stockage en clair).
2. Un e-mail est unique dans le système.
3. Un compte désactivé ne peut plus se connecter, même avec les bons identifiants.
4. Un utilisateur ne peut accéder qu'aux ressources autorisées par son rôle ; toute tentative d'accès non autorisé renvoie une erreur explicite (403), jamais un plantage silencieux.
5. La session est régénérée à la connexion (protection contre la fixation de session).
6. Toute action qui modifie des données (formulaire POST) est protégée par un jeton CSRF.

## Contraintes d'architecture

- La vérification de rôle est centralisée dans un middleware/garde réutilisable, appelé avant chaque contrôleur protégé — jamais un `if ($role == 'admin')` copié-collé dans chaque contrôleur.
- Aucun mot de passe, même haché, n'apparaît dans un log ou un message d'erreur.
- La logique d'authentification (vérifier les identifiants, créer la session) est isolée du contrôleur HTTP dans un Service dédié, testable indépendamment.

## Modèle de données à concevoir

Réfléchis à comment stocker le rôle (colonne simple suffit ici, une table de rôles séparée est un bonus si tu veux des permissions plus fines), et à quelles colonnes techniques ajouter pour la sécurité (date de dernière connexion, nombre de tentatives échouées, etc. — optionnel).

## Questions de découverte

1. Pourquoi `password_hash()`/`password_verify()` et pas un simple `md5($password)` ?
2. Qu'est-ce que la fixation de session et pourquoi régénérer l'identifiant de session à la connexion protège contre ça ?
3. Pourquoi un jeton CSRF est nécessaire même si l'utilisateur est authentifié ?

## Scénarios d'acceptation

| # | Scénario | Résultat attendu |
|---|---|---|
| 1 | Connexion avec un mauvais mot de passe | Refus, message générique (pas « e-mail inconnu » vs « mot de passe faux » séparément) |
| 2 | Accès à la zone admin par un `utilisateur` simple | 403 |
| 3 | Connexion sur un compte désactivé | Refus |
| 4 | Formulaire de modification de profil sans jeton CSRF valide | Refus |
| 5 | Inscription avec un e-mail déjà utilisé | Refus avec message clair |

## Livrables

Identiques aux précédents, plus une note courte listant les mesures de sécurité mises en place et celles que tu as sciemment laissées de côté (avec la raison).

## Checklist d'auto-évaluation

- [ ] Aucun mot de passe en clair nulle part (code, logs, base).
- [ ] Le contrôle de rôle est centralisé, pas dupliqué.
- [ ] Toute mutation POST est protégée par CSRF.
- [ ] Message d'erreur de connexion générique (pas de fuite d'information sur l'existence d'un compte).

## Bonus

- Vérification d'e-mail par lien de confirmation (simulé).
- Réinitialisation de mot de passe par jeton à durée limitée.
- Journal des connexions par utilisateur.
