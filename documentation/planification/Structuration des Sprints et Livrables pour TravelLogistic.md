# Structuration des Sprints et Livrables pour travellogistic

Ce document détaille les objectifs et les livrables pour chaque sprint du projet travellogistic, conformément au plan fourni.

## Sprint 1: Prototype UI Frontend (Sans Backend)

*   **Objectif Principal**: Développer une maquette interactive complète de l'interface utilisateur (UI) de l'application travellogistic en utilisant Next.js et des données fictives (mock data). Cela permettra de valider l'expérience utilisateur (UX) et le design avant l'implémentation du backend.
*   **Livrables Clés**:
    1.  **Application Next.js Initialisée**: Projet Next.js configuré avec la structure de base des dossiers, le routage (Next.js App Router ou Pages Router), et un layout global (en-tête, pied de page, navigation principale).
    2.  **Composants UI Réutilisables**: Création d'une bibliothèque de composants UI de base (boutons, formulaires, cartes, modales, etc.) pour assurer la cohérence visuelle.
    3.  **Pages Statiques avec Données Mock**:
        *   **Tableau de Bord Utilisateur**: Affichage des informations fictives du chauffeur, du véhicule et du voyage en cours.
        *   **Tableau de Bord Chauffeur**: Affichage des informations fictives du voyage assigné, des arrêts clients, et des options de mise à jour de statut.
        *   **Vue Carte de l'Itinéraire**: Intégration de Google Maps JavaScript API affichant un itinéraire statique avec des points de départ et d'arrivée fictifs, et des marqueurs de position simulés.
        *   **Page Planning (Chauffeur)**: Affichage d'un planning fictif sous forme de tableau et/ou de calendrier.
        *   **Page Liste des Clients & Vue Détaillée (Chauffeur)**: Affichage d'une liste de clients fictifs avec la possibilité de voir les détails d'un client.
        *   **Page de Messages**: Interface de messagerie statique pour simuler la communication entre utilisateur et chauffeur.
        *   **Section Notifications**: Affichage de notifications statiques de base pour simuler les alertes.
    4.  **Simulation de Fonctionnalités Dynamiques**:
        *   Simulation du suivi en direct sur la carte (par exemple, un marqueur se déplaçant le long d'un itinéraire prédéfini).
        *   Simulation des interactions utilisateur (par exemple, clics sur des boutons qui affichent des messages de confirmation statiques).
    5.  **Code Source du Prototype Frontend**: Dépôt Git avec le code source complet du prototype frontend.
    6.  **Documentation des Données Mock**: Description de la structure des données fictives utilisées pour chaque page et composant.

## Sprint 2: Configuration Backend + Authentification + APIs Utilisateur

*   **Objectif Principal**: Mettre en place l'infrastructure backend avec Express.js, choisir et configurer la base de données (PostgreSQL ou MongoDB), implémenter l'authentification des utilisateurs (JWT), et développer les premières API nécessaires aux fonctionnalités utilisateur. Connecter le prototype frontend à ces API pour les fonctionnalités d'authentification et d'affichage des données de base.
*   **Livrables Clés**:
    1.  **Backend Express.js Initialisé**: Projet Express.js structuré, avec gestion des routes, middlewares de base (gestion des erreurs, parsing JSON, CORS).
    2.  **Choix et Configuration de la Base de Données**: Décision finale sur l'utilisation de PostgreSQL ou MongoDB. Installation, configuration et connexion à la base de données choisie.
    3.  **Modèles de Données (Schémas)**: Définition et implémentation des schémas/modèles pour les entités principales : `User`, `Driver`, `Trip`, `Vehicle`, `Client` (avec les attributs de base).
    4.  **API d'Authentification**: 
        *   Endpoints pour l'inscription (`/auth/register`) et la connexion (`/auth/login`) des utilisateurs et chauffeurs.
        *   Implémentation de la génération et validation de JSON Web Tokens (JWT) pour la gestion des sessions.
        *   Middleware de protection des routes nécessitant une authentification.
    5.  **API Utilisateur de Base**:
        *   Endpoint pour récupérer le profil de l'utilisateur connecté (`/api/users/me` ou `/api/drivers/me`).
        *   Endpoint pour lister les voyages assignés à un utilisateur ou à un chauffeur (`/api/trips/assigned`).
    6.  **Intégration Frontend-Backend (Partielle)**:
        *   Modification des pages de connexion et d'inscription du frontend pour appeler les API d'authentification.
        *   Stockage sécurisé du JWT côté client (par exemple, `localStorage` ou `HttpOnly cookie`).
        *   Modification du tableau de bord utilisateur/chauffeur pour afficher les données réelles provenant des API (profil, voyages assignés) au lieu des données mock.
    7.  **Routage Basé sur les Rôles (Frontend)**: Implémentation de la logique dans le frontend Next.js pour rediriger les utilisateurs vers les bonnes pages et protéger les routes en fonction de leur rôle (User, Driver) après authentification.
    8.  **Code Source du Backend (Initial)**: Dépôt Git avec le code source du backend.
    9.  **Documentation des API**: Documentation initiale des endpoints créés (par exemple, avec Postman ou Swagger/OpenAPI).
    10. **Scripts de Migration/Seed (si applicable)**: Scripts pour initialiser la base de données avec des données de test.

## Sprint 3: Fonctionnalités de Voyage & Temps Réel

*   **Objectif Principal**: Développer les fonctionnalités essentielles liées à la gestion des voyages (création, mise à jour, suivi de statut) et implémenter les communications en temps réel (suivi GPS, messagerie instantanée) en utilisant Socket.IO.
*   **Livrables Clés**:
    1.  **API de Gestion des Voyages (Backend)**:
        *   Endpoints CRUD pour les voyages (`/api/trips`) : création, lecture, mise à jour, suppression (principalement pour l'admin, mais certaines mises à jour pour le chauffeur).
        *   Endpoint pour que le chauffeur démarre/termine un voyage (`/api/trips/:id/start`, `/api/trips/:id/end`).
        *   Endpoint pour que le chauffeur mette à jour le statut d'un client sur un voyage (`/api/trips/:tripId/clients/:clientId/status`).
    2.  **API de Localisation (Backend)**:
        *   Endpoint pour que le chauffeur envoie ses coordonnées GPS (`/api/drivers/location`).
    3.  **Configuration de Socket.IO (Backend & Frontend)**:
        *   Mise en place du serveur Socket.IO côté backend (Express.js).
        *   Intégration du client Socket.IO côté frontend (Next.js).
    4.  **Fonctionnalité de Suivi GPS en Temps Réel**:
        *   Le frontend du chauffeur envoie périodiquement les coordonnées GPS au backend via API ou WebSocket.
        *   Le backend diffuse les mises à jour de localisation aux utilisateurs concernés via Socket.IO.
        *   Le frontend de l'utilisateur affiche la position du chauffeur en temps réel sur la carte Google Maps.
    5.  **Fonctionnalité de Messagerie en Temps Réel**:
        *   Implémentation de la logique de messagerie (envoi, réception, stockage basique des messages) via Socket.IO.
        *   Interface de chat fonctionnelle sur les pages Utilisateur et Chauffeur, permettant l'échange de messages en direct.
        *   (Optionnel) Stockage des messages en base de données.
    6.  **Intégration Frontend Complète pour les Voyages**: 
        *   La vue carte de l'itinéraire et le tableau de bord de voyage sont connectés aux API réelles et aux WebSockets pour afficher les informations dynamiques.
        *   Les chauffeurs peuvent démarrer/terminer des voyages et mettre à jour les statuts des clients depuis leur interface.
    7.  **Code Source (Backend et Frontend) Mis à Jour**: Dépôt Git avec les dernières modifications.
    8.  **Documentation des API et des Événements WebSocket**: Mise à jour de la documentation Postman/Swagger et ajout de la description des événements Socket.IO.

## Sprint 4: Panneau Admin + Notifications + Test Final

*   **Objectif Principal**: Construire le panneau d'administration pour la gestion des entités clés, implémenter un système de notifications complet, effectuer des tests exhaustifs (unitaires, intégration, E2E), peaufiner l'interface utilisateur, corriger les bugs et préparer l'application pour une éventuelle présentation ou un déploiement.
*   **Livrables Clés**:
    1.  **Panneau d'Administration (Frontend & Backend)**:
        *   Interface dédiée pour les administrateurs.
        *   Fonctionnalités CRUD (Créer, Lire, Mettre à jour, Supprimer) pour :
            *   Utilisateurs (Users)
            *   Chauffeurs (Drivers)
            *   Voyages (Trips)
            *   Clients (potentiellement, ou gestion via les voyages)
            *   Véhicules (Vehicles)
        *   API backend correspondantes pour supporter les opérations CRUD de l'admin.
    2.  **Système de Notifications (Backend & Frontend)**:
        *   Logique backend pour générer des notifications basées sur des événements (nouveau message, mise à jour de voyage, alerte système, etc.).
        *   Diffusion des notifications aux utilisateurs/chauffeurs/admins concernés (potentiellement via WebSockets ou une autre méthode).
        *   Affichage des notifications dans l'interface utilisateur (par exemple, un centre de notifications, des badges).
        *   API pour que l'admin envoie des alertes à l'échelle du système.
    3.  **Tests Approfondis**:
        *   **Tests Unitaires**: Pour les composants frontend critiques et la logique métier backend.
        *   **Tests d'Intégration**: Pour vérifier l'interaction entre les modules frontend et backend (API, WebSockets).
        *   **Tests End-to-End (E2E)** (si le temps le permet): Scénarios utilisateurs clés testés de bout en bout (par exemple, avec Cypress ou Playwright).
    4.  **Peaufinage de l'UI/UX**: Améliorations visuelles, corrections de l'ergonomie, s'assurer de la responsivité sur différentes tailles d'écran.
    5.  **Correction de Bugs**: Identification et résolution des bugs découverts lors des tests et de l'utilisation.
    6.  **Documentation Finale du Projet**: Mise à jour de toute la documentation (technique, utilisateur si besoin).
    7.  **Préparation au Déploiement**: 
        *   Optimisation des builds frontend et backend.
        *   Configuration des variables d'environnement pour la production.
        *   (Optionnel) Scripts de déploiement ou instructions.
    8.  **Code Source Final et Complet**: Dépôt Git avec la version finale de l'application.
    9.  **Rapport de Test**: Résumé des tests effectués et des résultats.
    10. **Présentation/Démonstration de l'Application**: Préparation d'une démonstration des fonctionnalités clés.

Cette structuration servira de guide pour la planification détaillée des tâches de chaque sprint.
