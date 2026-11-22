# Ev entHub — Plateforme de gestion et de réservation des événements des clubs étudiants

> **Mini-projet Angular 22** — Évaluation Pratique  
> **Module** : Développement côté client · Framework côté client  
> **Public** : Étudiants de 2e année DSI — ISET Radès  
> **Année universitaire** : 2026 – 2027  
> **Auteur** : Ghaith Mnasri (`botti33`)  



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

For a complete list of available schematics (such as `components`, `directives`, or `pipes`), run:

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
