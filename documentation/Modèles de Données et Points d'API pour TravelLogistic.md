# Modèles de Données et Points d'API pour travellogistic

Ce document compile les suggestions de modèles de données et les détails des points de terminaison d'API pour le projet travellogistic, basés sur la planification détaillée des sprints.

## 1. Modèles de Données Suggérés

Le choix final entre PostgreSQL et MongoDB influencera l'implémentation exacte (ORM/ODM, types de données spécifiques), mais la structure conceptuelle reste similaire.

### 1.1. Modèle `User` (Clients/Passagers)
*   **Attributs Suggérés**:
    *   `id`: Identifiant unique (PK)
    *   `firstName`: Chaîne
    *   `lastName`: Chaîne
    *   `email`: Chaîne (unique, indexé)
    *   `password`: Chaîne (hashé)
    *   `phoneNumber`: Chaîne (optionnel)
    *   `profilePictureUrl`: Chaîne (optionnel)
    *   `role`: Chaîne (valeur par défaut: "User")
    *   `createdAt`: Timestamp
    *   `updatedAt`: Timestamp
*   **Relations**:
    *   Un `User` peut avoir plusieurs `Trip`s (un-à-plusieurs).

### 1.2. Modèle `Driver` (Chauffeurs)
*   **Attributs Suggérés**:
    *   `id`: Identifiant unique (PK)
    *   `firstName`: Chaîne
    *   `lastName`: Chaîne
    *   `email`: Chaîne (unique, indexé)
    *   `password`: Chaîne (hashé)
    *   `phoneNumber`: Chaîne
    *   `profilePictureUrl`: Chaîne (optionnel)
    *   `licenseNumber`: Chaîne (unique)
    *   `vehicleId`: Identifiant du `Vehicle` assigné (FK, optionnel, peut être une relation plusieurs-à-plusieurs via une table de jonction si un véhicule peut avoir plusieurs chauffeurs ou un chauffeur plusieurs véhicules sur des périodes différentes).
    *   `availabilityStatus`: Énumération (par exemple, 'Available', 'OnTrip', 'Offline')
    *   `currentLocation`: GeoJSON (pour MongoDB) ou deux champs `latitude` (Nombre), `longitude` (Nombre) (pour PostgreSQL)
    *   `role`: Chaîne (valeur par défaut: "Driver")
    *   `createdAt`: Timestamp
    *   `updatedAt`: Timestamp
*   **Relations**:
    *   Un `Driver` peut être assigné à plusieurs `Trip`s (un-à-plusieurs).
    *   Un `Driver` est généralement assigné à un `Vehicle` (un-à-un ou un-à-plusieurs si partagé).

### 1.3. Modèle `Vehicle` (Véhicules)
*   **Attributs Suggérés**:
    *   `id`: Identifiant unique (PK)
    *   `make`: Chaîne (marque)
    *   `model`: Chaîne (modèle)
    *   `year`: Entier
    *   `licensePlate`: Chaîne (unique, indexé)
    *   `color`: Chaîne
    *   `capacity`: Entier (nombre de passagers)
    *   `type`: Chaîne (par exemple, 'Berline', 'SUV', 'Van')
    *   `imageUrl`: Chaîne (optionnel)
    *   `createdAt`: Timestamp
    *   `updatedAt`: Timestamp
*   **Relations**:
    *   Un `Vehicle` peut être conduit par un ou plusieurs `Driver`(s) (selon la modélisation: un-à-un, un-à-plusieurs, ou plusieurs-à-plusieurs avec `DriverVehicleAssignment`).
    *   Un `Vehicle` peut être utilisé dans plusieurs `Trip`s (un-à-plusieurs).

### 1.4. Modèle `Trip` (Voyages)
*   **Attributs Suggérés**:
    *   `id`: Identifiant unique (PK)
    *   `userId`: Identifiant du `User` (FK)
    *   `driverId`: Identifiant du `Driver` (FK, peut être null si non encore assigné)
    *   `vehicleId`: Identifiant du `Vehicle` (FK, peut être null si non encore assigné)
    *   `startLocationAddress`: Chaîne
    *   `startLocationLat`: Nombre
    *   `startLocationLng`: Nombre
    *   `endLocationAddress`: Chaîne
    *   `endLocationLat`: Nombre
    *   `endLocationLng`: Nombre
    *   `startTimeScheduled`: Timestamp
    *   `endTimeScheduled`: Timestamp (estimé)
    *   `startTimeActual`: Timestamp (optionnel)
    *   `endTimeActual`: Timestamp (optionnel)
    *   `status`: Énumération (par exemple, 'PendingConfirmation', 'Scheduled', 'AssignedToDriver', 'DriverEnRoute', 'InProgress', 'Completed', 'CancelledByUser', 'CancelledByDriver', 'AdminCancelled')
    *   `estimatedFare`: Nombre (optionnel)
    *   `actualFare`: Nombre (optionnel)
    *   `mapRouteData`: JSON ou Texte (pour stocker des waypoints, polyligne encodée, etc.)
    *   `createdAt`: Timestamp
    *   `updatedAt`: Timestamp
*   **Relations**:
    *   Appartient à un `User`.
    *   Appartient à un `Driver`.
    *   Utilise un `Vehicle`.
    *   Un `Trip` peut avoir plusieurs `Client`s (arrêts/passagers spécifiques au voyage) (un-à-plusieurs).
    *   Un `Trip` peut avoir plusieurs `Message`s (un-à-plusieurs).

### 1.5. Modèle `Client` (Arrêts/Clients spécifiques à un voyage)
*   **Attributs Suggérés**:
    *   `id`: Identifiant unique (PK)
    *   `tripId`: Identifiant du `Trip` (FK, indexé)
    *   `name`: Chaîne (nom du client à récupérer/déposer)
    *   `address`: Chaîne (adresse de l'arrêt)
    *   `phoneNumber`: Chaîne (optionnel)
    *   `pickupOrder`: Entier (si plusieurs clients/arrêts ordonnés)
    *   `status`: Énumération ('Pending', 'Recuperer', 'Annulé', 'Absent', 'Completed')
    *   `notes`: Texte (instructions spéciales)
    *   `createdAt`: Timestamp
    *   `updatedAt`: Timestamp
*   **Relations**:
    *   Appartient à un `Trip`.

### 1.6. Modèle `Message`
*   **Attributs Suggérés**:
    *   `id`: Identifiant unique (PK)
    *   `tripId`: Identifiant du `Trip` (FK, indexé, pour le contexte du chat)
    *   `senderId`: Identifiant du `User` ou `Driver` (FK)
    *   `senderRole`: Énumération ('User', 'Driver')
    *   `receiverId`: Identifiant du `User` ou `Driver` (FK)
    *   `receiverRole`: Énumération ('User', 'Driver')
    *   `text`: Texte (contenu du message)
    *   `timestamp`: Timestamp (indexé)
    *   `isRead`: Booléen (par le receveur)
    *   `createdAt`: Timestamp
*   **Relations**:
    *   Appartient à un `Trip` (pour le contexte).
    *   Envoyé par un `User` ou `Driver`.
    *   Reçu par un `User` ou `Driver`.

### 1.7. Modèle `Notification`
*   **Attributs Suggérés**:
    *   `id`: Identifiant unique (PK)
    *   `recipientId`: Identifiant du `User`, `Driver`, ou `Admin` (FK, polymorphique ou plusieurs champs FK optionnels)
    *   `recipientRole`: Énumération ('User', 'Driver', 'Admin')
    *   `type`: Énumération (par exemple, 'NewMessage', 'TripStarted', 'TripEnded', 'ClientStatusUpdate', 'SystemAlert', 'TripAssigned')
    *   `title`: Chaîne
    *   `message`: Texte
    *   `isRead`: Booléen (valeur par défaut: false)
    *   `link`: Chaîne (URL vers la ressource concernée, par exemple, `/trips/:id`)
    *   `createdAt`: Timestamp (indexé)
*   **Relations**:
    *   Appartient à un `User`, `Driver`, ou `Admin` (destinataire).

## 2. Points de Terminaison d'API Détaillés

Préfixe commun: `/api`
Middlewares: `isAuthenticated` (vérifie JWT), `hasRole('RoleName')` (vérifie le rôle).

### 2.1. Authentification (`/auth`)
*   **`POST /auth/register`**
    *   Description: Inscription d'un nouvel utilisateur ou chauffeur.
    *   Corps: `{ firstName, lastName, email, password, phoneNumber, role ('User' ou 'Driver'), ... (autres champs spécifiques au rôle comme licenseNumber pour Driver) }`
    *   Réponse: `201 Created` - `{ user: { id, email, role, ... }, token }`
*   **`POST /auth/login`**
    *   Description: Connexion d'un utilisateur ou chauffeur existant.
    *   Corps: `{ email, password }`
    *   Réponse: `200 OK` - `{ user: { id, email, role, ... }, token }`

### 2.2. Profils Utilisateur/Chauffeur (`/users`, `/drivers`)
*   **`GET /users/me`**
    *   Middleware: `isAuthenticated`, `hasRole('User')`
    *   Description: Récupérer le profil de l'utilisateur connecté.
    *   Réponse: `200 OK` - `{ id, firstName, lastName, email, ... }`
*   **`GET /drivers/me`**
    *   Middleware: `isAuthenticated`, `hasRole('Driver')`
    *   Description: Récupérer le profil du chauffeur connecté.
    *   Réponse: `200 OK` - `{ id, firstName, lastName, email, licenseNumber, availabilityStatus, ... }`
*   **`POST /drivers/location`** (Optionnel si WebSocket est utilisé directement pour la localisation)
    *   Middleware: `isAuthenticated`, `hasRole('Driver')`
    *   Description: Mettre à jour la localisation actuelle du chauffeur.
    *   Corps: `{ latitude, longitude }`
    *   Réponse: `200 OK` ou `204 No Content`

### 2.3. Voyages (`/trips`)
*   **`GET /trips/assigned`**
    *   Middleware: `isAuthenticated` (logique interne pour User ou Driver)
    *   Description: Lister les voyages assignés à l'utilisateur ou au chauffeur connecté.
    *   Réponse: `200 OK` - `[ { tripDetails... }, ... ]`
*   **`POST /trips`** (Principalement Admin, ou User pour demander un voyage)
    *   Middleware: `isAuthenticated` (permissions spécifiques selon le rôle)
    *   Description: Créer un nouveau voyage.
    *   Corps: `{ userId (si admin), driverId (optionnel), vehicleId (optionnel), startLocationAddress, startLocationLat, startLocationLng, endLocationAddress, endLocationLat, endLocationLng, startTimeScheduled, ... }`
    *   Réponse: `201 Created` - `{ tripDetails... }`
*   **`GET /trips/:id`**
    *   Middleware: `isAuthenticated` (vérifier que l'utilisateur/chauffeur est lié au voyage, ou Admin)
    *   Description: Obtenir les détails d'un voyage spécifique.
    *   Réponse: `200 OK` - `{ tripDetails..., clients: [...], user: {...}, driver: {...}, vehicle: {...} }`
*   **`PUT /trips/:id`**
    *   Middleware: `isAuthenticated` (permissions spécifiques)
    *   Description: Mettre à jour un voyage (par Admin ou Chauffeur pour certains champs).
    *   Corps: `{ ... (champs à mettre à jour) }`
    *   Réponse: `200 OK` - `{ updatedTripDetails... }`
*   **`POST /trips/:id/start`**
    *   Middleware: `isAuthenticated`, `hasRole('Driver')` (vérifier que le chauffeur est assigné au voyage)
    *   Description: Le chauffeur démarre le voyage.
    *   Réponse: `200 OK` - `{ message: 'Trip started', trip: { updatedTripDetails... } }`
*   **`POST /trips/:id/end`**
    *   Middleware: `isAuthenticated`, `hasRole('Driver')` (vérifier que le chauffeur est assigné au voyage)
    *   Description: Le chauffeur termine le voyage.
    *   Réponse: `200 OK` - `{ message: 'Trip ended', trip: { updatedTripDetails... } }`
*   **`PUT /trips/:tripId/clients/:clientId/status`**
    *   Middleware: `isAuthenticated`, `hasRole('Driver')` (vérifier que le chauffeur est assigné au voyage et le client au voyage)
    *   Description: Le chauffeur met à jour le statut d'un client/arrêt.
    *   Corps: `{ status: ('Recuperer' | 'Annulé' | 'Absent' | 'Completed') }`
    *   Réponse: `200 OK` - `{ updatedClientDetails... }`

### 2.4. Notifications (`/notifications`)
*   **`GET /notifications`**
    *   Middleware: `isAuthenticated`
    *   Description: Récupérer les notifications pour l'utilisateur connecté (avec filtres possibles: `?read=false`).
    *   Réponse: `200 OK` - `[ { notificationDetails... }, ... ]`
*   **`PUT /notifications/:id/read`**
    *   Middleware: `isAuthenticated` (vérifier que la notification appartient à l'utilisateur)
    *   Description: Marquer une notification comme lue.
    *   Réponse: `200 OK` - `{ updatedNotificationDetails... }`

### 2.5. Administration (`/admin/...`)
*   Tous les endpoints `/admin/*` nécessitent `isAuthenticated` et `hasRole('Admin')`.

*   **Gestion des Utilisateurs (`/admin/users`)**
    *   `GET /admin/users`: Lister tous les utilisateurs.
    *   `POST /admin/users`: Créer un utilisateur.
    *   `GET /admin/users/:id`: Obtenir un utilisateur.
    *   `PUT /admin/users/:id`: Mettre à jour un utilisateur.
    *   `DELETE /admin/users/:id`: Supprimer un utilisateur.

*   **Gestion des Chauffeurs (`/admin/drivers`)**
    *   `GET /admin/drivers`: Lister tous les chauffeurs.
    *   `POST /admin/drivers`: Créer un chauffeur.
    *   `GET /admin/drivers/:id`: Obtenir un chauffeur.
    *   `PUT /admin/drivers/:id`: Mettre à jour un chauffeur.
    *   `DELETE /admin/drivers/:id`: Supprimer un chauffeur.

*   **Gestion des Véhicules (`/admin/vehicles`)**
    *   `GET /admin/vehicles`: Lister tous les véhicules.
    *   `POST /admin/vehicles`: Créer un véhicule.
    *   `GET /admin/vehicles/:id`: Obtenir un véhicule.
    *   `PUT /admin/vehicles/:id`: Mettre à jour un véhicule.
    *   `DELETE /admin/vehicles/:id`: Supprimer un véhicule.

*   **Gestion des Voyages (extension des API `/trips` avec droits admin)**
    *   `GET /admin/trips`: Lister tous les voyages (plus de filtres/détails que pour un user/driver).
    *   (Les `POST`, `PUT`, `DELETE` sur `/api/trips/:id` sont aussi accessibles à l'admin avec plus de droits).

*   **Gestion des Clients d'un Voyage (`/admin/trips/:tripId/clients`)**
    *   `GET /admin/trips/:tripId/clients`: Lister les clients d'un voyage.
    *   `POST /admin/trips/:tripId/clients`: Ajouter un client à un voyage.
    *   `PUT /admin/trips/:tripId/clients/:clientId`: Mettre à jour un client.
    *   `DELETE /admin/trips/:tripId/clients/:clientId`: Supprimer un client.

*   **Alertes Système (`/admin/system-alert`)**
    *   `POST /admin/system-alert`
        *   Description: Envoyer une notification à tous les utilisateurs ou à des groupes ciblés.
        *   Corps: `{ title, message, targetRole ('All', 'User', 'Driver') (optionnel) }`
        *   Réponse: `200 OK` - `{ message: 'Alert sent' }`

## 3. Événements WebSocket (Socket.IO)

Authentification des connexions WebSocket via JWT.
Utilisation de `rooms` (par exemple, `trip:<tripId>`, `user:<userId>`).

### 3.1. Suivi de Localisation
*   **Client (Chauffeur) émet `driverLocationUpdate`**
    *   Payload: `{ tripId: string, latitude: number, longitude: number }`
    *   Description: Le chauffeur envoie ses coordonnées GPS mises à jour.
*   **Serveur émet `locationUpdatedToUser` (vers la room du voyage)**
    *   Payload: `{ driverId: string, latitude: number, longitude: number, tripId: string }`
    *   Description: Le serveur diffuse la nouvelle localisation du chauffeur aux utilisateurs concernés.

### 3.2. Messagerie en Temps Réel
*   **Client (User/Driver) émet `joinChatRoom`**
    *   Payload: `{ tripId: string }`
    *   Description: Un utilisateur ou chauffeur rejoint la room de chat pour un voyage spécifique.
*   **Client (User/Driver) émet `leaveChatRoom`**
    *   Payload: `{ tripId: string }`
    *   Description: Un utilisateur ou chauffeur quitte la room de chat.
*   **Client (User/Driver) émet `sendMessage`**
    *   Payload: `{ tripId: string, text: string }` (le serveur déduit `senderId` et `senderRole` du socket authentifié)
    *   Description: Un utilisateur ou chauffeur envoie un message dans la room du voyage.
*   **Serveur émet `receiveMessage` (vers la room du voyage)**
    *   Payload: `{ tripId: string, senderId: string, senderRole: string, text: string, timestamp: Date }`
    *   Description: Le serveur diffuse un nouveau message aux participants de la room de chat.

### 3.3. Notifications en Temps Réel
*   **Serveur émet `newNotification` (vers la room de l'utilisateur/chauffeur/admin destinataire, par exemple `user:<userId>`)**
    *   Payload: `{ notificationId: string, title: string, message: string, link: string, type: string, createdAt: Date }`
    *   Description: Le serveur envoie une nouvelle notification en temps réel au destinataire.

