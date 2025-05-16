# Sprint 2: Configuration Backend + Authentification + APIs Utilisateur – Tâches Détaillées

**Objectif Principal**: Mettre en place l'infrastructure backend avec Express.js, choisir et configurer la base de données (PostgreSQL ou MongoDB), implémenter l'authentification des utilisateurs (JWT), et développer les premières API nécessaires aux fonctionnalités utilisateur. Connecter le prototype frontend à ces API pour les fonctionnalités d'authentification et d'affichage des données de base.

## 1. Configuration du Backend Express.js

*   **Tâche 1.1: Initialisation du projet Express.js**
    *   Sous-tâche 1.1.1: Créer un nouveau projet Node.js (`npm init` ou `yarn init`).
    *   Sous-tâche 1.1.2: Installer Express.js et les dépendances de base (par exemple, `nodemon` pour le développement, `dotenv` pour les variables d'environnement).
    *   Sous-tâche 1.1.3: Mettre en place une structure de dossiers pour le backend (par exemple, `src/`, `src/config/`, `src/routes/`, `src/controllers/`, `src/models/`, `src/middleware/`, `src/services/`).
    *   Sous-tâche 1.1.4: Configurer le point d'entrée principal de l'application (par exemple, `server.js` ou `app.js`).
*   **Tâche 1.2: Mise en place des Middlewares de Base**
    *   Sous-tâche 1.2.1: Intégrer `express.json()` pour le parsing des requêtes JSON.
    *   Sous-tâche 1.2.2: Intégrer `express.urlencoded()` pour le parsing des requêtes URL-encodées.
    *   Sous-tâche 1.2.3: Configurer CORS (Cross-Origin Resource Sharing) pour autoriser les requêtes depuis le frontend Next.js.
    *   Sous-tâche 1.2.4: Mettre en place un middleware de gestion globale des erreurs.
    *   Sous-tâche 1.2.5: (Optionnel) Intégrer un logger (par exemple, Morgan) pour les requêtes HTTP.

## 2. Choix, Configuration et Modélisation de la Base de Données

*   **Tâche 2.1: Choix de la Base de Données**
    *   Sous-tâche 2.1.1: Évaluer les avantages/inconvénients de PostgreSQL et MongoDB pour travellogistic (considérer les relations, la scalabilité, l'expérience de l'équipe).
    *   Sous-tâche 2.1.2: **Prendre une décision finale : PostgreSQL ou MongoDB.** (Ce choix impactera les tâches suivantes).
*   **Tâche 2.2: Installation et Configuration de la Base de Données Choisie**
    *   Sous-tâche 2.2.1: Installer le SGBD localement ou configurer un service cloud (par exemple, Docker, ElephantSQL pour PostgreSQL, MongoDB Atlas).
    *   Sous-tâche 2.2.2: Créer la base de données pour le projet.
    *   Sous-tâche 2.2.3: Configurer la connexion à la base de données dans l'application Express.js (via variables d'environnement).
*   **Tâche 2.3: Définition et Implémentation des Modèles de Données (Schémas)**
    *   *Note: Les attributs spécifiques sont des suggestions et devront être affinés.*
    *   Sous-tâche 2.3.1: **Modèle `User`** (pour les clients/passagers)
        *   Attributs suggérés: `id`, `firstName`, `lastName`, `email` (unique), `password` (hashé), `phoneNumber`, `profilePictureUrl`, `role` (par défaut 'User'), `createdAt`, `updatedAt`.
        *   Implémenter le schéma/modèle avec l'ORM/ODM choisi (par exemple, Sequelize/Prisma pour PostgreSQL, Mongoose pour MongoDB).
    *   Sous-tâche 2.3.2: **Modèle `Driver`**
        *   Attributs suggérés: `id`, `firstName`, `lastName`, `email` (unique), `password` (hashé), `phoneNumber`, `profilePictureUrl`, `licenseNumber`, `vehicleId` (relation), `availabilityStatus`, `currentLocation` (GeoJSON si MongoDB, ou lat/lon), `role` (par défaut 'Driver'), `createdAt`, `updatedAt`.
        *   Implémenter le schéma/modèle.
    *   Sous-tâche 2.3.3: **Modèle `Vehicle`**
        *   Attributs suggérés: `id`, `make`, `model`, `year`, `licensePlate` (unique), `color`, `capacity`, `type` (par exemple, Berline, SUV), `imageUrl`, `assignedDriverId` (relation optionnelle si un véhicule peut être conduit par plusieurs chauffeurs à des moments différents, sinon lien direct depuis Driver).
        *   Implémenter le schéma/modèle.
    *   Sous-tâche 2.3.4: **Modèle `Trip`**
        *   Attributs suggérés: `id`, `userId` (relation), `driverId` (relation), `vehicleId` (relation), `startLocation` (adresse, lat/lon), `endLocation` (adresse, lat/lon), `startTimeScheduled`, `endTimeScheduled`, `startTimeActual`, `endTimeActual`, `status` (par exemple, 'Pending', 'Assigned', 'InProgress', 'Completed', 'Cancelled'), `estimatedFare`, `actualFare`, `mapRouteData` (pourrait stocker des waypoints ou une polyligne encodée), `createdAt`, `updatedAt`.
        *   Implémenter le schéma/modèle.
    *   Sous-tâche 2.3.5: **Modèle `Client`** (pour les arrêts/clients du chauffeur lors d'un voyage)
        *   Attributs suggérés: `id`, `tripId` (relation), `name`, `address`, `phoneNumber`, `pickupOrder` (si plusieurs clients), `status` ('Pending', 'Recuperer', 'Annulé', 'Absent'), `notes`.
        *   Implémenter le schéma/modèle.
    *   Sous-tâche 2.3.6: Définir les relations entre les modèles (par exemple, un `User` peut avoir plusieurs `Trip`s, un `Driver` peut être assigné à plusieurs `Trip`s, un `Trip` a plusieurs `Client`s).
*   **Tâche 2.4: Scripts de Migration et de Seed (Données de Test)**
    *   Sous-tâche 2.4.1: Mettre en place un système de migration (par exemple, Sequelize CLI, Knex migrations, Mongoose versioning ou scripts manuels).
    *   Sous-tâche 2.4.2: Créer des scripts de seed pour peupler la base de données avec des données de test initiales (utilisateurs, chauffeurs, véhicules). 

## 3. Implémentation de l'Authentification (JWT)

*   **Tâche 3.1: Développement des Endpoints d'Authentification**
    *   Sous-tâche 3.1.1: **Endpoint `POST /api/auth/register`** (pour User et Driver)
        *   Controller pour gérer l'inscription: validation des entrées (email unique, mot de passe fort), hashage du mot de passe (par exemple, avec `bcryptjs`), sauvegarde du nouvel utilisateur/chauffeur en base.
        *   Retourner les informations de l'utilisateur créé (sans le mot de passe) et un JWT.
    *   Sous-tâche 3.1.2: **Endpoint `POST /api/auth/login`** (pour User et Driver)
        *   Controller pour gérer la connexion: validation des entrées, recherche de l'utilisateur/chauffeur par email, comparaison du mot de passe hashé.
        *   Si les identifiants sont valides, générer un JWT.
        *   Retourner les informations de l'utilisateur/chauffeur (sans le mot de passe) et le JWT.
*   **Tâche 3.2: Implémentation de la Logique JWT**
    *   Sous-tâche 3.2.1: Installer la bibliothèque `jsonwebtoken`.
    *   Sous-tâche 3.2.2: Créer des fonctions utilitaires pour générer des JWT (contenant `userId`, `role`, et une expiration) et les vérifier.
    *   Sous-tâche 3.2.3: Stocker la clé secrète JWT de manière sécurisée (variable d'environnement).
*   **Tâche 3.3: Middleware de Protection des Routes**
    *   Sous-tâche 3.3.1: Créer un middleware (`isAuthenticated`) qui vérifie la présence et la validité du JWT dans l'en-tête `Authorization` (`Bearer token`).
    *   Sous-tâche 3.3.2: Si le token est valide, attacher les informations de l'utilisateur (par exemple, `req.user = { id: userId, role: userRole }`) à l'objet requête.
    *   Sous-tâche 3.3.3: Créer un middleware (`hasRole`) qui vérifie si l'utilisateur authentifié a le rôle requis pour accéder à une route spécifique.

## 4. Développement des APIs Utilisateur de Base

*   **Tâche 4.1: Endpoint `GET /api/users/me` ou `GET /api/drivers/me`**
    *   Sous-tâche 4.1.1: Utiliser le middleware `isAuthenticated`.
    *   Sous-tâche 4.1.2: Controller pour récupérer les informations du profil de l'utilisateur/chauffeur connecté à partir de `req.user.id` et les retourner (sans le mot de passe).
*   **Tâche 4.2: Endpoint `GET /api/trips/assigned`**
    *   Sous-tâche 4.2.1: Utiliser le middleware `isAuthenticated`.
    *   Sous-tâche 4.2.2: Controller pour lister les voyages assignés à l'utilisateur connecté (si rôle 'User', basé sur `userId`) ou au chauffeur connecté (si rôle 'Driver', basé sur `driverId`).
    *   Sous-tâche 4.2.3: Inclure les informations pertinentes des modèles associés (par exemple, détails du chauffeur/utilisateur, véhicule).

## 5. Intégration Frontend-Backend (Partielle)

*   **Tâche 5.1: Connexion des Pages d'Authentification Frontend aux APIs Backend**
    *   Sous-tâche 5.1.1: Modifier la page d'inscription du frontend pour appeler `POST /api/auth/register`.
    *   Sous-tâche 5.1.2: Modifier la page de connexion du frontend pour appeler `POST /api/auth/login`.
    *   Sous-tâche 5.1.3: Gérer les réponses de l'API (succès, erreurs) et afficher des messages appropriés à l'utilisateur.
*   **Tâche 5.2: Gestion du JWT côté Frontend**
    *   Sous-tâche 5.2.1: Lors d'une connexion réussie, stocker le JWT reçu de manière sécurisée (par exemple, `localStorage`, `sessionStorage`, ou de préférence dans un contexte React/état global et potentiellement `HttpOnly cookie` si géré par le backend pour plus de sécurité).
    *   Sous-tâche 5.2.2: Inclure le JWT dans l'en-tête `Authorization` des requêtes API subséquentes.
    *   Sous-tâche 5.2.3: Implémenter la logique de déconnexion (supprimer le JWT stocké).
*   **Tâche 5.3: Connexion des Tableaux de Bord aux APIs**
    *   Sous-tâche 5.3.1: Modifier le tableau de bord utilisateur pour appeler `GET /api/users/me` et `GET /api/trips/assigned` afin d'afficher les données réelles.
    *   Sous-tâche 5.3.2: Modifier le tableau de bord chauffeur pour appeler `GET /api/drivers/me` et `GET /api/trips/assigned` afin d'afficher les données réelles.

## 6. Implémentation du Routage Basé sur les Rôles (Frontend)

*   **Tâche 6.1: Logique de Protection des Routes Frontend**
    *   Sous-tâche 6.1.1: Créer des composants d'ordre supérieur (HOC) ou utiliser des contextes React pour protéger les routes en fonction du statut d'authentification et du rôle de l'utilisateur (extrait du JWT ou de l'API `/me`).
    *   Sous-tâche 6.1.2: Rediriger les utilisateurs non authentifiés vers la page de connexion.
    *   Sous-tâche 6.1.3: Rediriger les utilisateurs vers leur tableau de bord respectif après connexion ou empêcher l'accès aux pages non autorisées (par exemple, un utilisateur ne peut pas accéder au tableau de bord chauffeur).

## 7. Tests et Documentation

*   **Tâche 7.1: Tests Unitaires et d'Intégration de Base (Backend)**
    *   Sous-tâche 7.1.1: Écrire des tests unitaires pour la logique d'authentification (génération/validation JWT, hashage de mot de passe).
    *   Sous-tâche 7.1.2: Écrire des tests d'intégration pour les endpoints API créés (par exemple, avec Supertest).
*   **Tâche 7.2: Documentation des API**
    *   Sous-tâche 7.2.1: Documenter les endpoints API créés (requêtes, réponses, codes de statut) en utilisant un outil comme Postman (créer une collection) ou générer une documentation Swagger/OpenAPI.
*   **Tâche 7.3: Revue de Code**
    *   Sous-tâche 7.3.1: Effectuer des revues de code pour le backend et les modifications frontend.

## 8. Gestion de Projet et Finalisation du Sprint

*   **Tâche 8.1: Mettre à jour le dépôt Git avec le code source du backend et les modifications frontend.**
*   **Tâche 8.2: Préparer une démonstration des fonctionnalités implémentées** (authentification, affichage des données réelles sur les tableaux de bord).

Ce plan détaillé pour le Sprint 2 vise à établir une base solide pour le backend et à commencer l'intégration avec le frontend.
