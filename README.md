## Auteur

YAO MIÉZAN SAM WILLIAM

<p align="center">
  <img src="./images/logo.png" alt="Logo Ayana Hair" width="140"/>
</p>

<h1 align="center">Ayana Hair</h1>
<p align="center">Application mobile e-commerce de produits capillaires, développée en Flutter</p>

<p align="center">
  <img src="https://img.shields.io/badge/Flutter-Frontend-02569B?logo=flutter&logoColor=white" alt="Flutter"/>
  <img src="https://img.shields.io/badge/Dart-Langage-0175C2?logo=dart&logoColor=white" alt="Dart"/>
  <img src="https://img.shields.io/badge/Node.js-Backend-339933?logo=nodedotjs&logoColor=white" alt="Node.js"/>
  <img src="https://img.shields.io/badge/Express.js-API_REST-000000?logo=express&logoColor=white" alt="Express"/>
  <img src="https://img.shields.io/badge/MySQL-8.x-4479A1?logo=mysql&logoColor=white" alt="MySQL"/>
  <img src="https://img.shields.io/badge/JWT-Authentification-black?logo=jsonwebtokens" alt="JWT"/>
</p>

## Présentation

Ayana Hair est une boutique spécialisée dans les produits capillaires destinés aux femmes africaines. Cette application mobile permet aux clientes de commander en ligne (catalogue, panier, tunnel de commande, suivi de commande, notifications) et à l'équipe de la boutique de gérer les opérations via un tableau de bord administrateur connecté à la base de données.

Projet réalisé dans le cadre d'un travail personnel de développement mobile, avec une architecture client-serveur complète.

## Fonctionnalités

**Côté client**
- Inscription et connexion sécurisées (JWT, mots de passe hashés bcrypt)
- Catalogue produits avec filtres par catégorie (Huiles, Shampoings, Soins)
- Panier persistant, synchronisé avec la base de données
- Tunnel de commande en 3 étapes : livraison, paiement, récapitulatif
- Paiement par Orange Money, MTN Mobile Money, Wave ou à la livraison
- Historique et suivi détaillé des commandes (statut, timeline)
- Centre de notifications automatiques
- Gestion du profil utilisateur (informations, mot de passe)

**Côté administrateur**
- Tableau de bord statistiques (chiffre d'affaires, commandes, clients)
- Gestion complète des commandes avec changement de statut
- Notification automatique du client à chaque changement de statut
- Gestion des stocks en temps réel, avec alerte de stock faible

## Stack technique

| Couche | Technologie |
|---|---|
| Frontend | Flutter (Dart), multi-plateforme Android / iOS / Web |
| Gestion d'état | Provider (ChangeNotifier) |
| Backend | Node.js, Express.js, API REST |
| Base de données | MySQL 8.x |
| Authentification | JWT, expiration 24h |
| Sécurité mots de passe | bcryptjs, salt factor 10 |
| Stockage local | SharedPreferences |

Architecture à 3 couches : `Flutter → requête HTTP → API Node.js → MySQL → réponse JSON → Flutter`.

## Galerie de l'application

| | | |
|---|---|---|
| ![Connexion](./images/screens/01-login.png) | ![Inscription](./images/screens/02-inscription.png) | ![Accueil](./images/screens/03-accueil.png) |
| Connexion | Inscription | Accueil |
| ![Produits phares](./images/screens/04-accueil-produits-phares.png) | ![Boutique](./images/screens/05-boutique.png) | ![Panier](./images/screens/06-panier.png) |
| Produits phares | Boutique | Panier |
| ![Checkout livraison](./images/screens/07-checkout-livraison.png) | ![Checkout paiement](./images/screens/08-checkout-paiement.png) | ![Checkout récapitulatif](./images/screens/09-checkout-recapitulatif.png) |
| Livraison (étape 1) | Paiement (étape 2) | Récapitulatif (étape 3) |
| ![Commande confirmée](./images/screens/10-commande-confirmee.png) | ![Historique](./images/screens/11-historique-commandes.png) | ![Détails commande](./images/screens/12-details-commande.png) |
| Commande confirmée | Historique des commandes | Détails d'une commande |
| ![Profil](./images/screens/13-profil.png) | ![Notifications](./images/screens/14-notifications.png) | ![Admin stats](./images/screens/15-admin-stats.png) |
| Profil | Notifications | Dashboard admin, stats |
| ![Admin commandes](./images/screens/16-admin-commandes.png) | ![Admin stocks](./images/screens/17-admin-stocks.png) | |
| Admin, gestion des commandes | Admin, gestion des stocks | |

## Modes de paiement

<p>
  <img src="./images/icons/orange_money.png" alt="Orange Money" height="40"/>
  <img src="./images/icons/mtn_money.png" alt="MTN Mobile Money" height="40"/>
  <img src="./images/icons/wave.png" alt="Wave" height="40"/>
  <img src="./images/icons/moov_money.png" alt="Moov Money" height="40"/>
</p>

Orange Money, MTN Mobile Money, Wave, ou paiement à la livraison.

## Base de données

6 tables principales : `Utilisateur`, `Produit`, `Panier`, `Commande`, `CommandeDetail`, `Notification`. Un utilisateur peut passer plusieurs commandes et recevoir plusieurs notifications. Une commande se décompose en plusieurs lignes de détail (`CommandeDetail`), chacune référençant un produit avec son prix et sa quantité au moment de l'achat, ce qui conserve un historique fiable même si le catalogue évolue par la suite.

Le script de création des tables se trouve dans [`AYANA_HAIR_BDD.sql`](./AYANA_HAIR_BDD.sql).

## API REST (extrait)

| Domaine | Route | Description |
|---|---|---|
| Authentification | `POST /api/inscription` | Créer un compte |
| Authentification | `POST /api/connexion` | Connexion, génère le token JWT |
| Produits | `GET /api/produits` | Liste des produits |
| Panier | `POST /api/panier` | Ajouter un article |
| Panier | `GET /api/panier/:utilisateur_id` | Récupérer le panier |
| Commandes | `POST /api/commandes` | Créer une commande |
| Commandes | `GET /api/commandes/:utilisateur_id` | Historique client |
| Admin | `GET /api/admin/stats` | Statistiques globales |
| Admin | `PUT /api/admin/commandes/:id/statut` | Changer le statut, notifie le client |
| Admin | `PUT /api/admin/stocks/:id` | Modifier le stock d'un produit |

Documentation complète des routes dans le [cahier des charges](./cahier_des_charges_ayana_hair.pdf).

## Sécurité

- Hashage des mots de passe avec bcryptjs (irréversible)
- Authentification par JWT signé, expiration 24h
- Requêtes SQL préparées, protection contre l'injection SQL
- Vérification côté serveur du rôle (client ou admin) sur les routes sensibles
- CORS activé sur le serveur Express

## Structure du projet

```
ayana_hair/
├── lib/
│   ├── main.dart
│   ├── services/
│   │   ├── api_service.dart
│   │   └── panier_manager.dart
│   └── screens/
│       ├── splash_screen.dart
│       ├── login_screen.dart
│       ├── register_screen.dart
│       ├── home_screen.dart
│       ├── boutique_screen.dart
│       ├── panier_screen.dart
│       ├── checkout_screen.dart
│       ├── historique_screen.dart
│       ├── commande_detail_screen.dart
│       ├── profil_screen.dart
│       ├── notification_screen.dart
│       └── admin_screen.dart
├── server/
│   ├── server.js
│   └── package.json
└── AYANA_HAIR_BDD.sql
```

## Installation

**Backend**

```bash
cd server
npm install
# configurer la connexion MySQL dans server.js
node server.js
```

**Frontend Flutter**

```bash
flutter pub get
flutter run
```

## Documentation

- [Cahier des charges technique](./cahier_des_charges_ayana_hair.pdf)
- [Guide de l'application](./Ayana_Hair_Guide.pdf)
