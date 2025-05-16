# Sprint 3: Fonctionnalités de Voyage & Temps Réel – Tâches Détaillées

**Objectif Principal**: Développer les fonctionnalités essentielles liées à la gestion des voyages (création, mise à jour, suivi de statut) et implémenter les communications en temps réel (suivi GPS, messagerie instantanée) en utilisant Socket.IO.

## 1. Développement des API de Gestion des Voyages (Backend)

*   **Tâche 1.1: Endpoints CRUD pour les Voyages (`/api/trips`)**
    *   Sous-tâche 1.1.1: **`POST /api/trips`** (Création de voyage - principalement pour Admin, ou pré-assignation)
        *   Controller pour valider les données d'entrée (userId, driverId, vehicleId, start/end locations, etc.).
        *   Sauvegarder le nouveau voyage en base de données.
        *   Retourner les détails du voyage créé.
    *   Sous-tâche 1.1.2: **`GET /api/trips`** (Lister tous les voyages - pour Admin)
        *   Controller pour récupérer tous les voyages avec options de filtrage et pagination.
    *   Sous-tâche 1.1.3: **`GET /api/trips/:id`** (Obtenir les détails d'un voyage spécifique)
        *   Controller pour récupérer un voyage par son ID, incluant les clients associés, utilisateur, chauffeur, véhicule.
    *   Sous-tâche 1.1.4: **`PUT /api/trips/:id`** (Mettre à jour un voyage - Admin pour la plupart des champs, Chauffeur pour certains statuts)
        *   Controller pour mettre à jour les informations d'un voyage.
        *   Gérer les permissions (qui peut mettre à jour quels champs).
    *   Sous-tâche 1.1.5: **`DELETE /api/trips/:id`** (Supprimer un voyage - Admin)
        *   Controller pour supprimer un voyage (ou le marquer comme supprimé).
*   **Tâche 1.2: Endpoints pour les Actions du Chauffeur sur un Voyage**
    *   Sous-tâche 1.2.1: **`POST /api/trips/:id/start`** (Démarrer un voyage)
        *   Controller pour que le chauffeur authentifié démarre un voyage assigné.
        *   Mettre à jour le statut du voyage en 'InProgress', `startTimeActual`.
        *   Déclencher potentiellement une notification.
    *   Sous-tâche 1.2.2: **`POST /api/trips/:id/end`** (Terminer un voyage)
        *   Controller pour que le chauffeur authentifié termine un voyage.
        *   Mettre à jour le statut du voyage en 'Completed', `endTimeActual`.
        *   Déclencher potentiellement une notification.
    *   Sous-tâche 1.2.3: **`PUT /api/trips/:tripId/clients/:clientId/status`** (Mettre à jour le statut d'un client)
        *   Controller pour que le chauffeur mette à jour le statut d'un client spécifique sur son voyage actuel (Recuperer, Annulé, Absent).
        *   Valider que le chauffeur est assigné au voyage et que le client appartient à ce voyage.
        *   Déclencher potentiellement une notification.

## 2. Développement de l'API de Localisation (Backend)

*   **Tâche 2.1: Endpoint `POST /api/drivers/location`** (Optionnel si WebSocket est utilisé directement pour la localisation)
    *   Sous-tâche 2.1.1: Controller pour que le chauffeur authentifié envoie ses coordonnées GPS (latitude, longitude).
    *   Sous-tâche 2.1.2: Sauvegarder la dernière localisation connue du chauffeur dans le modèle `Driver` ou une collection/table dédiée si un historique est nécessaire.
    *   Sous-tâche 2.1.3: Diffuser cette information via Socket.IO (voir section 3).

## 3. Configuration et Implémentation de Socket.IO (Backend & Frontend)

*   **Tâche 3.1: Configuration du Serveur Socket.IO (Backend)**
    *   Sous-tâche 3.1.1: Installer la bibliothèque `socket.io`.
    *   Sous-tâche 3.1.2: Intégrer Socket.IO avec le serveur HTTP Express existant.
    *   Sous-tâche 3.1.3: Gérer l'authentification des connexions WebSocket (par exemple, en utilisant le JWT passé lors de la connexion initiale).
    *   Sous-tâche 3.1.4: Définir des `namespaces` ou des `rooms` pour cibler les communications (par exemple, une room par voyage `trip:<tripId>`).
*   **Tâche 3.2: Intégration du Client Socket.IO (Frontend - Next.js)**
    *   Sous-tâche 3.2.1: Installer la bibliothèque `socket.io-client`.
    *   Sous-tâche 3.2.2: Créer un service ou un contexte React pour gérer la connexion Socket.IO et l'abonnement aux événements.
    *   Sous-tâche 3.2.3: Gérer la connexion et la déconnexion du socket (par exemple, lors du montage/démontage des composants).

## 4. Implémentation du Suivi GPS en Temps Réel

*   **Tâche 4.1: Envoi des Coordonnées GPS (Frontend Chauffeur)**
    *   Sous-tâche 4.1.1: Utiliser l'API de géolocalisation du navigateur (`navigator.geolocation`) pour obtenir les coordonnées GPS du chauffeur.
    *   Sous-tâche 4.1.2: Envoyer périodiquement les coordonnées au backend via un événement Socket.IO dédié (par exemple, `driverLocationUpdate`) ou via l'API `POST /api/drivers/location`.
        *   Événement `driverLocationUpdate` : `{ tripId: '...', latitude: '...', longitude: '...' }`
*   **Tâche 4.2: Gestion et Diffusion des Coordonnées (Backend)**
    *   Sous-tâche 4.2.1: Écouter l'événement `driverLocationUpdate` sur le serveur Socket.IO.
    *   Sous-tâche 4.2.2: Mettre à jour la localisation du chauffeur en base de données (modèle `Driver`).
    *   Sous-tâche 4.2.3: Diffuser les nouvelles coordonnées aux clients (utilisateurs) abonnés à la room du voyage concerné (par exemple, événement `locationUpdatedToUser`: `{ driverId: '...', latitude: '...', longitude: '...' }`).
*   **Tâche 4.3: Affichage de la Localisation en Temps Réel (Frontend Utilisateur)**
    *   Sous-tâche 4.3.1: S'abonner à l'événement `locationUpdatedToUser` pour le voyage en cours.
    *   Sous-tâche 4.3.2: Mettre à jour la position du marqueur du chauffeur sur la carte Google Maps dynamiquement lorsque de nouvelles coordonnées sont reçues.
    *   Sous-tâche 4.3.3: (Optionnel) Animer le mouvement du marqueur pour une transition plus fluide.

## 5. Implémentation de la Messagerie en Temps Réel

*   **Tâche 5.1: Logique de Messagerie (Backend - Socket.IO)**
    *   Sous-tâche 5.1.1: Définir les événements Socket.IO pour la messagerie (par exemple, `joinChatRoom`, `leaveChatRoom`, `sendMessage`, `receiveMessage`).
        *   `sendMessage`: `{ tripId: '...', senderId: '...', senderRole: '...', text: '...' }`
        *   `receiveMessage`: `{ tripId: '...', senderId: '...', senderRole: '...', text: '...', timestamp: '...' }`
    *   Sous-tâche 5.1.2: Gérer l'adhésion des utilisateurs et chauffeurs aux rooms de chat spécifiques à un voyage.
    *   Sous-tâche 5.1.3: Lors de la réception d'un événement `sendMessage`, diffuser le message aux autres participants de la room.
    *   Sous-tâche 5.1.4: (Optionnel mais recommandé) Créer un modèle `Message` et sauvegarder les messages en base de données (`id`, `tripId`, `senderId`, `senderRole`, `text`, `timestamp`).
*   **Tâche 5.2: Interface de Messagerie (Frontend - Utilisateur et Chauffeur)**
    *   Sous-tâche 5.2.1: Sur la page de messages, permettre à l'utilisateur/chauffeur de rejoindre la room de chat du voyage actif.
    *   Sous-tâche 5.2.2: Afficher les messages reçus en temps réel.
    *   Sous-tâche 5.2.3: Permettre l'envoi de nouveaux messages via un champ de saisie, déclenchant l'événement `sendMessage`.
    *   Sous-tâche 5.2.4: (Si messages stockés) Charger l'historique des messages lors de l'entrée dans le chat.

## 6. Intégration Frontend Complète pour les Voyages

*   **Tâche 6.1: Connexion de la Vue Carte et du Tableau de Bord de Voyage aux API/WebSockets**
    *   Sous-tâche 6.1.1: Remplacer les données mock restantes par des appels aux API réelles pour les détails du voyage, les clients, etc.
    *   Sous-tâche 6.1.2: Intégrer la logique Socket.IO pour les mises à jour en temps réel (localisation, statuts).
*   **Tâche 6.2: Rendre Fonctionnelles les Actions du Chauffeur**
    *   Sous-tâche 6.2.1: Connecter les boutons "Démarrer/Terminer voyage" aux endpoints `POST /api/trips/:id/start` et `POST /api/trips/:id/end`.
    *   Sous-tâche 6.2.2: Connecter les options de mise à jour du statut client à l'endpoint `PUT /api/trips/:tripId/clients/:clientId/status`.
    *   Sous-tâche 6.2.3: Fournir un retour visuel à l'utilisateur après ces actions.

## 7. Tests et Documentation

*   **Tâche 7.1: Tests d'Intégration (Backend et Frontend)**
    *   Sous-tâche 7.1.1: Tester les endpoints API de gestion des voyages (CRUD, actions chauffeur).
    *   Sous-tâche 7.1.2: Tester la communication Socket.IO pour le suivi GPS (envoi, réception, diffusion).
    *   Sous-tâche 7.1.3: Tester la communication Socket.IO pour la messagerie.
    *   Sous-tâche 7.1.4: Simuler des scénarios multi-utilisateurs (un utilisateur suivant un chauffeur, deux personnes chattant).
*   **Tâche 7.2: Tests End-to-End (Scénarios Clés)**
    *   Sous-tâche 7.2.1: Un chauffeur démarre un voyage, l'utilisateur voit sa position se mettre à jour, ils échangent des messages.
    *   Sous-tâche 7.2.2: Un chauffeur met à jour le statut d'un client, l'information est reflétée (si applicable côté utilisateur ou pour l'admin plus tard).
*   **Tâche 7.3: Documentation des API et des Événements WebSocket**
    *   Sous-tâche 7.3.1: Mettre à jour la documentation Postman/Swagger avec les nouveaux endpoints API.
    *   Sous-tâche 7.3.2: Créer une documentation décrivant les événements Socket.IO (nom, payload, direction) et la logique des rooms.
*   **Tâche 7.4: Revue de Code**
    *   Sous-tâche 7.4.1: Effectuer des revues de code pour les nouvelles fonctionnalités backend et frontend.

## 8. Gestion de Projet et Finalisation du Sprint

*   **Tâche 8.1: Mettre à jour le dépôt Git avec le code source (backend et frontend).**
*   **Tâche 8.2: Préparer une démonstration des fonctionnalités de voyage et temps réel.**

Ce plan pour le Sprint 3 est axé sur la mise en œuvre des fonctionnalités dynamiques et interactives qui sont au cœur de l'application travellogistic.
