# Sprint 1: Prototype UI Frontend – Tâches Détaillées

**Objectif Principal**: Développer une maquette interactive complète de l'interface utilisateur (UI) de l'application travellogistic en utilisant Next.js et des données fictives (mock data).

## 1. Configuration du Projet Next.js et Environnement de Développement

*   **Tâche 1.1: Initialisation du projet Next.js**
    *   Sous-tâche 1.1.1: Utiliser `create-next-app` pour générer le squelette de l'application.
    *   Sous-tâche 1.1.2: Choisir les options appropriées (TypeScript si souhaité, ESLint, Tailwind CSS si pertinent, App Router).
    *   Sous-tâche 1.1.3: Initialiser un dépôt Git pour le projet.
*   **Tâche 1.2: Structuration des dossiers du projet**
    *   Sous-tâche 1.2.1: Définir une structure de dossiers claire (par exemple, `app/` pour les routes, `components/` pour les composants réutilisables, `lib/` pour les utilitaires, `public/` pour les assets statiques, `data/` pour les données mock).
*   **Tâche 1.3: Mise en place du Layout Global**
    *   Sous-tâche 1.3.1: Créer un composant `Layout` principal (par exemple, `app/layout.tsx`).
    *   Sous-tâche 1.3.2: Intégrer une navigation principale (barre de navigation, menu latéral si nécessaire) et un pied de page de base.
    *   Sous-tâche 1.3.3: Définir les styles globaux (par exemple, dans `app/globals.css`).
*   **Tâche 1.4: Configuration du Routage de Base**
    *   Sous-tâche 1.4.1: Définir les routes initiales pour les pages principales listées dans les objectifs du sprint (Tableaux de bord, Carte, Planning, etc.) en utilisant le système de routage de Next.js (App Router).

## 2. Développement des Composants UI Réutilisables

*   **Tâche 2.1: Conception et développement de la bibliothèque de composants de base**
    *   Sous-tâche 2.1.1: Boutons (primaires, secondaires, icônes).
    *   Sous-tâche 2.1.2: Cartes (pour afficher des informations résumées).
    *   Sous-tâche 2.1.3: Éléments de formulaire (champs de saisie, listes déroulantes, cases à cocher).
    *   Sous-tâche 2.1.4: Modales ou Pop-ups.
    *   Sous-tâche 2.1.5: Indicateurs de chargement.
    *   Sous-tâche 2.1.6: Éléments de navigation spécifiques (onglets, breadcrumbs).
    *   Sous-tâche 2.1.7: Composants d'affichage de listes.
*   **Tâche 2.2: S'assurer de la responsivité des composants**
    *   Sous-tâche 2.2.1: Tester et ajuster les composants pour différents tailles d'écran (mobile, tablette, bureau).

## 3. Création des Pages avec Données Mock

*   **Tâche 3.1: Définition de la structure des données mock**
    *   Sous-tâche 3.1.1: Créer des fichiers JSON (par exemple, dans `data/mockData.ts`) pour simuler les données des utilisateurs, chauffeurs, voyages, véhicules, clients, messages, notifications.
    *   Sous-tâche 3.1.2: Documenter la structure de ces données mock.
*   **Tâche 3.2: Développement du Tableau de Bord Utilisateur (`/dashboard/user`)**
    *   Sous-tâche 3.2.1: Intégrer les composants UI pour afficher les informations (chauffeur, véhicule, voyage en cours) à partir des données mock.
    *   Sous-tâche 3.2.2: Mise en page du tableau de bord.
*   **Tâche 3.3: Développement du Tableau de Bord Chauffeur (`/dashboard/driver`)**
    *   Sous-tâche 3.3.1: Afficher les informations du voyage assigné, les arrêts clients (liste) à partir des données mock.
    *   Sous-tâche 3.3.2: Intégrer des boutons (non fonctionnels pour le backend) pour "Démarrer/Terminer voyage", "Mettre à jour statut client".
*   **Tâche 3.4: Développement de la Vue Carte de l'Itinéraire (`/route-map`)**
    *   Sous-tâche 3.4.1: Intégrer Google Maps JavaScript API dans un composant React/Next.js.
        *   Obtenir une clé API Google Maps (pour le développement).
        *   Charger l'API Google Maps de manière asynchrone.
    *   Sous-tâche 3.4.2: Afficher une carte centrée sur une localisation par défaut.
    *   Sous-tâche 3.4.3: Afficher des marqueurs pour les points de départ et d'arrivée fictifs (depuis les données mock).
    *   Sous-tâche 3.4.4: (Optionnel) Tracer un itinéraire statique entre les points.
*   **Tâche 3.5: Développement de la Page Planning (Chauffeur) (`/schedule`)**
    *   Sous-tâche 3.5.1: Choisir une représentation (tableau simple et/ou un composant calendrier basique).
    *   Sous-tâche 3.5.2: Afficher une liste de voyages/événements fictifs à partir des données mock.
*   **Tâche 3.6: Développement de la Page Liste des Clients & Vue Détaillée (Chauffeur) (`/clients`, `/clients/[id]`)**
    *   Sous-tâche 3.6.1: Créer une page listant les clients fictifs (associés à un voyage mock) avec des informations de base.
    *   Sous-tâche 3.6.2: Créer une page de détail pour un client affichant plus d'informations (depuis les données mock).
    *   Sous-tâche 3.6.3: Simuler la navigation entre la liste et le détail.
*   **Tâche 3.7: Développement de la Page de Messages (`/messages`)**
    *   Sous-tâche 3.7.1: Créer une interface de type chat statique.
    *   Sous-tâche 3.7.2: Afficher une liste de conversations fictives et des messages mock.
    *   Sous-tâche 3.7.3: Inclure un champ de saisie de message (non fonctionnel pour l'envoi réel).
*   **Tâche 3.8: Développement de la Section Notifications (statique)**
    *   Sous-tâche 3.8.1: Créer un composant pour afficher une liste de notifications.
    *   Sous-tâche 3.8.2: Afficher des notifications statiques/mock (par exemple, dans un menu déroulant ou une page dédiée `/notifications`).

## 4. Simulation des Fonctionnalités Dynamiques (Frontend Uniquement)

*   **Tâche 4.1: Simulation du suivi en direct sur la carte**
    *   Sous-tâche 4.1.1: Créer une logique simple (par exemple, avec `useEffect` et `setTimeout`) pour mettre à jour la position d'un marqueur (véhicule) sur la carte Google Maps le long d'un chemin prédéfini ou entre des points mock.
*   **Tâche 4.2: Simulation des interactions utilisateur**
    *   Sous-tâche 4.2.1: Pour les actions comme "Mettre à jour statut client", afficher des confirmations visuelles temporaires ou des changements d'état dans l'UI (par exemple, changer la couleur d'un statut) sans appel backend.
    *   Sous-tâche 4.2.2: S'assurer que les éléments interactifs (boutons, liens) répondent au clic, même si l'action finale n'est pas implémentée.

## 5. Tests et Vérifications du Prototype

*   **Tâche 5.1: Tests de base de l'interface utilisateur**
    *   Sous-tâche 5.1.1: Vérifier la navigation entre toutes les pages créées.
    *   Sous-tâche 5.1.2: S'assurer que toutes les données mock s'affichent correctement.
    *   Sous-tâche 5.1.3: Tester l'affichage sur les navigateurs cibles principaux (Chrome, Firefox).
    *   Sous-tâche 5.1.4: Vérifier la responsivité de base des pages principales.
*   **Tâche 5.2: Vérification des simulations**
    *   Sous-tâche 5.2.1: Confirmer que la simulation de suivi GPS fonctionne comme prévu.
    *   Sous-tâche 5.2.2: Valider que les interactions utilisateur simulées donnent un feedback visuel approprié.

## 6. Documentation et Finalisation du Sprint

*   **Tâche 6.1: Documentation des données mock**
    *   Sous-tâche 6.1.1: Finaliser le document décrivant la structure des données mock utilisées (si non fait en 3.1.2).
*   **Tâche 6.2: Revue du code et nettoyage**
    *   Sous-tâche 6.2.1: S'assurer que le code est propre, commenté si nécessaire, et suit les conventions établies.
*   **Tâche 6.3: Préparation d'une démonstration du prototype**
    *   Sous-tâche 6.3.1: Lister les scénarios à présenter pour valider l'UI/UX.

Ce plan détaillé pour le Sprint 1 devrait fournir une feuille de route claire pour la construction du prototype frontend.
