# EventHub — Plateforme de gestion et de réservation des événements des clubs étudiants

> **Mini-projet Angular 22** — Évaluation Pratique  
> **Module** : Développement côté client · Framework côté client  
> **Public** : Étudiants de 2e année DSI — ISET Radès  
> **Année universitaire** : 2026 – 2027  
> **Auteur** : Ghaith Mnasri (`botti33`)  
> **Dépôt Git** : [https://github.com/botti33/angular-mini-project-ghaith-mnasri.git](https://github.com/botti33/angular-mini-project-ghaith-mnasri.git)

---

## 1. Présentation du projet

**EventHub** est une application web monopage (SPA) conçue avec **Angular 22** (CLI 22.x, architecture 100% composants **standalone**, signals réactifs et nouveau flux de contrôle `@if`, `@for`, `@switch`, `@defer`).

Elle résout le problème de gestion et d'inscription aux événements organisés par les différents clubs universitaires (robotique, informatique, entrepreneuriat, sport, loisirs, etc.) en permettant :
- La consultation, le filtrage et le tri des événements avec suivi en temps réel des places disponibles ;
- La réservation de places (1 à 4 places par étudiant) avec gestion immédiate du quota de places ;
- La gestion et l'annulation de ses réservations personnelles ;
- La publication de nouveaux événements pour les organisateurs autorisés ;
- L'authentification simulée côté client (rôles `etudiant` et `organisateur`) avec sécurisation des routes par guards fonctionnels.

---

## 2. Jalon actuel : J0 — Lancement

Conformément au planning et aux livrables prévus par le cahier des charges :

| Jalon | Échéance | Contenu attendu | Statut |
| :--- | :--- | :--- | :---: |
| **J0 — Lancement** | Semaine du 09/11/2026 | Dépôt créé et partagé, projet initialisé, modèles et données en place | **RÉALISÉ** |
| **Jn — Points de suivi** | Séances TP hebdomadaires | Suivi de l'avancement et encadrement | À venir |
| **Jf — Soutenance finale** | Semaine du 28/12/2026 | Dépôt final avec tag `v1.0`, captures dans `docs/`, démonstration | À venir |

### Éléments mis en place pour J0 :
- **Initialisation du projet** : Angular CLI 22.1.8 en mode standalone strict, sans `NgModule`.
- **Modèles de données TypeScript** (`src/app/modeles/`) :
  - `evenement.ts` : Type `Categorie` ('atelier' | 'conference' | 'competition' | 'sortie') et interface `Evenement`.
  - `reservation.ts` : Interface `Reservation`.
  - `utilisateur.ts` : Interface `Utilisateur` (rôles `'etudiant'` | `'organisateur'`).
  - `index.ts` : Baril d'export des modèles.
- **Jeu de données initial** (`src/app/donnees/`) :
  - `evenements.ts` : Jeu de données initial de 10 événements riches et réalistes respectant strictement les critères :
    - Couvre l'intégralité des 4 catégories (`atelier`, `conference`, `competition`, `sortie`).
    - Au moins un événement complet (`placesReservees === capacite`, ex: *Hackathon CodeSprint 2026*, 40/40 places).
    - Un événement avec une seule place restante (`capacite - placesReservees === 1`, ex: *Atelier Angular 22 & Signals*, 24/25 places).
    - Un événement déjà passé dans le temps (ex: *Conférence Révolution de l'IA Générative*, 15/10/2026).
    - Mix d'événements gratuits et payants.
  - `utilisateurs.ts` : Comptes de démonstration prédéfinis.
  - `reservations.ts` : Réservations initiales pour tests.
  - `index.ts` : Baril d'export des données.

---

## 3. Comptes de démonstration

L'authentification est simulée côté client avec conservation de la session en mémoire (signals) :

| Identifiant (Email) | Mot de passe | Rôle | Droits d'accès |
| :--- | :--- | :--- | :--- |
| `etudiant@iset.tn` | `etudiant123` | `etudiant` | Consulter, réserver (1 à 4 places), voir/annuler ses réservations |
| `club@iset.tn` | `club123` | `organisateur` | Tous les droits étudiant + créer de nouveaux événements |
| *Visiteur (non connecté)* | — | `visiteur` | Consultation de la liste, filtres, recherche, page détail |

---

## 4. Installation et exécution

### Prérequis
- **Node.js** (v20+ ou v22+)
- **NPM** (v10+ ou v11+)

### Installation des dépendances
```bash
npm install
```

### Lancement du serveur de développement
```bash
npm start
# ou
ng serve
```
Accéder à l'application sur [http://localhost:4200/](http://localhost:4200/).

### Compilation de production (vérification ET-11)
```bash
npm run build
```
Vérifie qu'il n'y a aucune erreur de compilation TypeScript ou Angular.

---

## 5. Architecture du projet

L'architecture suit scrupuleusement les recommandations du cahier des charges et la convention de nommage Angular 22 (fichiers sans suffixe redondant, `ET-02`) :

```
src/app/
├── app.ts                  # Composant racine (standalone)
├── app.html                # Template racine
├── app.css                 # Styles globaux du composant racine
├── app.routes.ts           # Configuration du routage
├── app.config.ts           # Configuration de l'application
├── modeles/                # Interfaces et types métier
│   ├── evenement.ts        # Interface Evenement & Type Categorie
│   ├── reservation.ts      # Interface Reservation
│   ├── utilisateur.ts      # Interface Utilisateur
│   └── index.ts            # Baril d'export
├── donnees/                # Données initiales en mémoire
│   ├── evenements.ts       # 10 événements initiaux (complet, 1 place, passé, 4 catégories)
│   ├── utilisateurs.ts     # Comptes de démonstration (etudiant & organisateur)
│   ├── reservations.ts     # Réservations initiales
│   └── index.ts            # Baril d'export
├── services/               # [Étapes suivantes] evenements, reservations, session
├── guards/                 # [Étapes suivantes] auth-guard, organisateur-guard
├── validateurs/            # [Étapes suivantes] nom-complet, places-disponibles, date-future
├── pipes/                  # [Étapes suivantes] filtre-evenements, tri-evenements, etat-places
├── composants/             # [Étapes suivantes] en-tete, pied-page, carte-evenement, badge-categorie
└── pages/                  # [Étapes suivantes] liste, detail, reservation, mes-reservations, ...
```

---

## 6. Suivi des exigences (Matrice de conformité)

| Identifiant | Description | Type | Statut |
| :--- | :--- | :---: | :---: |
| **ET-01** | Projet Angular CLI 22, composants standalone (0 NgModule) | Technique | Validé |
| **ET-02** | Convention de nommage sans suffixe (`carte-evenement.ts`, etc.) | Technique | En cours |
| **ET-03** | Nouvelle syntaxe de flux uniquement (`@if`, `@for`, `@switch`, `@defer`) | Technique | En cours |
| **ET-04** | Injection de dépendances avec `inject()` | Technique | Prévu |
| **ET-05** | Accès aux données via services uniquement | Technique | Prévu |
| **ET-06** | État partagé conservé dans des signals en lecture seule (`asReadonly()`) | Technique | Prévu |
| **ET-07** | Mises à jour immuables des tableaux (spread, filter, map) | Technique | Prévu |
| **ET-08** | Formulaires réactifs (`ReactiveFormsModule`) sans `ngModel` de saisie | Technique | Prévu |
| **ET-09** | Routes déclarées dans `app.routes.ts` avec lazy loading (`loadComponent`) | Technique | Prévu |
| **ET-10** | CSS fait main, aucun framework UI tiers | Technique | Prévu |
| **ET-11** | Projet compile sans erreur avec `ng build` | Technique | Validé |
| **EF-01** | Structure générale (en-tête, `<router-outlet>`, pied-page avec `@defer`) | Fonctionnelle | À venir |
| **EF-02** | Liste des événements (`@for`, carte, badge `@switch`, favoris `output()`) | Fonctionnelle | À venir |
| **EF-03** | Recherche, filtres et tri (2 pipes personnalisés + pipe `etatPlaces`) | Fonctionnelle | À venir |
| **EF-04** | Détail d'un événement (`withComponentInputBinding`, barre de remplissage) | Fonctionnelle | À venir |
| **EF-05** | Connexion et protection des routes (`authGuard`, `organisateurGuard`) | Fonctionnelle | À venir |
| **EF-06** | Réservation de places (formulaire réactif + validateur personnalisé) | Fonctionnelle | À venir |
| **EF-07** | Mes réservations (annulation, libération des places, compteur en-tête) | Fonctionnelle | À venir |
| **EF-08** | Création d'un événement organisateur (`dateFuture`, redirection) | Fonctionnelle | À venir |
| **EF-09** | Page introuvable (route wildcard `**`, page 404) | Fonctionnelle | À venir |
| **EF-10** | Fonctionnalités bonus (localStorage, dashboard `computed()`, responsive) | Bonus | À venir |
