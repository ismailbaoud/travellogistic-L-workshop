# Analyse des Besoins du Projet travellogistic

## 🧭 Résumé du Projet
travellogistic est une plateforme SaaS pour la gestion de la logistique de voyage, permettant le suivi en temps réel, la visualisation des informations sur les chauffeurs et les véhicules, la gestion des statuts des clients, l'envoi de messages et la réception de notifications.

## Rôles Utilisateurs
1.  **Utilisateur**
2.  **Chauffeur**
3.  **Admin**

## Pile Technologique
*   **Frontend**: Next.js (React)
*   **Backend**: Express.js (Node.js REST API)
*   **Base de données**: PostgreSQL ou MongoDB (à choisir)
*   **Temps réel**: WebSocket (Socket.IO)
*   **Cartes**: Google Maps JavaScript API

## Planning Général
*   Développement en 4 sprints hebdomadaires (1 mois au total).
*   Frontend d'abord en prototype avec des données mock, puis backend et intégration.

## 🧱 Fonctionnalités Principales par Rôle

### Utilisateur:
*   Tableau de bord : voir infos chauffeur, véhicule, voyage.
*   Carte Google Maps : points de départ et d'arrivée.
*   Suivi de localisation en direct.
*   Messagerie en temps réel avec le chauffeur.
*   Réception de notifications.

### Chauffeur:
*   Voir voyage assigné avec carte et arrêts clients.
*   Mettre à jour statut client (Récupérer, Annulé, Absent).
*   Démarrer/Terminer le voyage (démarre le partage GPS).
*   Voir planning (calendrier & tableau).
*   Voir et gérer les clients.
*   Messagerie en temps réel avec l'utilisateur.
*   Réception de notifications.

### Admin:
*   Gérer utilisateurs, chauffeurs, et voyages (CRUD).
*   Voir logs et analytiques.
*   Envoyer des alertes à l'échelle du système.
*   Recevoir des notifications système.

## 🗓️ Répartition des Sprints

### Sprint 1: Prototype UI Frontend (Pas de Backend)
*   Configuration de l'application Next.js avec routage et layout.
*   Construction de toutes les pages avec des données mock:
    *   Tableau de bord Utilisateur
    *   Tableau de bord Chauffeur
    *   Vue Carte de l'Itinéraire
    *   Planning (tableau/calendrier)
    *   Liste des Clients & Vue Détaillée
    *   Page de Messages
    *   Notifications (statiques de base)
*   Simulation du suivi en direct & des marqueurs sur la carte avec de fausses données.

### Sprint 2: Configuration Backend + Authentification + APIs Utilisateur
*   Configuration du backend Express.js, modèles de BD (Utilisateur, Chauffeur, Voyage, Véhicule, Client).
*   Création d'API pour:
    *   Authentification (basée sur JWT)
    *   Obtenir le profil utilisateur
    *   Lister les voyages assignés
*   Connexion du frontend au backend pour la connexion, le tableau de bord.
*   Implémentation du routage basé sur les rôles.

### Sprint 3: Fonctionnalités de Voyage & Temps Réel
*   Création de points de terminaison d'API:
    *   Créer & mettre à jour les voyages
    *   Obtenir la localisation du chauffeur
    *   Mises à jour du statut client
*   Configuration de Socket.IO pour:
    *   Mises à jour GPS en direct du chauffeur
    *   Chat en temps réel entre l'utilisateur & le chauffeur
*   Connexion de l'itinéraire et du tableau de bord de voyage à l'API réelle.

### Sprint 4: Panneau Admin + Notifications + Test Final
*   Construction du tableau de bord admin (CRUD utilisateur/voyage/client).
*   Implémentation du système de notification (événements de voyage, messages, alertes système).
*   Peaufinage de l'UI, correction des bugs, écriture des tests.
*   Préparation pour le déploiement et la présentation.

## Points à Clarifier / Décisions à Prendre
*   **Choix de la base de données**: PostgreSQL ou MongoDB. L'utilisateur a mentionné les deux. Il faudra faire une recommandation ou demander une préférence. Pour l'instant, je vais planifier en gardant à l'esprit que ce choix doit être fait au début du Sprint 2.
*   **Détails des modèles de données**: Bien que les entités principales soient listées (Utilisateur, Chauffeur, Voyage, Véhicule, Client), les attributs spécifiques pour chaque modèle devront être définis.
*   **Format des données mock**: Pour le Sprint 1, la structure des données mock doit être anticipée pour faciliter la transition vers les données réelles.
*   **Gestion des clients par le chauffeur**: La fonctionnalité "Voir et gérer les clients" pour le chauffeur nécessite plus de détails. S'agit-il d'un CRUD complet pour les clients assignés à ses voyages, ou d'une simple consultation/mise à jour de statut ?
*   **Logs et analytiques pour l'Admin**: La nature des logs et des analytiques à afficher doit être spécifiée.
*   **Notifications**: Les types de notifications et leurs déclencheurs doivent être listés de manière exhaustive.

Cette analyse initiale semble couvrir les points essentiels. Je vais maintenant mettre à jour le fichier todo.md.
