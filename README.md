<div align="center">

# 📚 BiblioTrack
### *Système de Gestion de Bibliothèque Web*

<p><em>Gérez livres, utilisateurs et emprunts depuis une interface web simple et intuitive.</em></p>

![Status](https://img.shields.io/badge/status-functional-success?style=flat)
![Version](https://img.shields.io/badge/version-1.0-blue?style=flat)
![License](https://img.shields.io/badge/license-MIT-green?style=flat)

<p><em>Built with:</em></p>

![PHP](https://img.shields.io/badge/PHP-8.x-777BB4?style=flat&logo=php&logoColor=white)
![MySQL](https://img.shields.io/badge/MySQL-8.0-4479A1?style=flat&logo=mysql&logoColor=white)
![Bootstrap](https://img.shields.io/badge/Bootstrap-4.5-7952B3?style=flat&logo=bootstrap&logoColor=white)
![jQuery](https://img.shields.io/badge/jQuery-3.6-0769AD?style=flat&logo=jquery&logoColor=white)
![JavaScript](https://img.shields.io/badge/JavaScript-ES6-F7DF1E?style=flat&logo=javascript&logoColor=black)
![CSS3](https://img.shields.io/badge/CSS3-custom-1572B6?style=flat&logo=css3&logoColor=white)
![AJAX](https://img.shields.io/badge/AJAX-async-orange?style=flat)
![Sessions](https://img.shields.io/badge/PHP-Sessions-blueviolet?style=flat)

</div>

---

## Table des Matières

- [Objectif du Projet](#objectif-du-projet)
- [Fonctionnalités par Rôle](#fonctionnalités-par-rôle)
- [Stack Technique](#stack-technique)
- [Architecture](#architecture)
- [Modèle de Données](#modèle-de-données)
- [Installation](#installation)
- [Structure du Projet](#structure-du-projet)
- [Idées d'amélioration](#idées-damélioration)

---

## Objectif du Projet

Développer une application web complète de gestion de bibliothèque avec **authentification et contrôle d'accès par rôle**. Chaque type d'utilisateur dispose d'un tableau de bord adapté à ses responsabilités : gestion globale pour l'admin, gestion des prêts pour le bibliothécaire, et consultation/réservation pour l'utilisateur final.

---

## Fonctionnalités par Rôle

### 👑 Administrateur
Accès complet à la gestion des ressources via modals Bootstrap dynamiques. Ajout, modification et suppression de livres et d'utilisateurs directement depuis le tableau de bord, sans rechargement de page.

### 📖 Bibliothécaire
Suivi en temps réel du catalogue avec recherche filtrée par titre, auteur, catégorie et état. Gestion complète des prêts : émission, réception, visualisation de tous les emprunts en cours, et **vérification automatique des retards**.

### 👤 Utilisateur
Recherche et filtrage des livres disponibles, gestion de ses propres réservations (ajout, consultation, annulation), et **notifications de retard** affichées directement sur le tableau de bord.

---

## Stack Technique

| Catégorie | Technologie |
|---|---|
| Backend | PHP 8.x (sessions, PDO/MySQLi) |
| Base de données | MySQL 8.0 |
| Frontend | Bootstrap 4.5 + jQuery 3.6 |
| Requêtes asynchrones | AJAX (jQuery) |
| Authentification | Sessions PHP natives |
| Styles | CSS3 custom + Bootstrap |

---

## Architecture

```
┌──────────────────────────────────────────────────────┐
│                BROWSER (HTML + JS)                   │
│                                                      │
│  ┌──────────────┐  ┌──────────────┐  ┌────────────┐  │
│  │  connexion   │  │  inscription │  │  logout    │  │
│  └──────────────┘  └──────────────┘  └────────────┘  │
│                                                      │
│  ┌──────────────┐  ┌──────────────┐  ┌────────────┐  │
│  │  admin.php   │  │  biblio.php  │  │  user.php  │  │
│  │  (Admin)     │  │  (Biblio.)   │  │  (User)    │  │
│  └──────────────┘  └──────────────┘  └────────────┘  │
│                     AJAX Requests                    │
└──────────────────────────┬───────────────────────────┘
                           │ PHP $_SESSION + MySQLi
                           ▼
┌──────────────────────────────────────────────────────┐
│                   MySQL Database                     │
│                   (sprintdev)                        │
│                                                      │
│  ┌──────────┐  ┌────────────┐  ┌──────────────────┐  │
│  │  Livres  │  │Utilisateurs│  │    Emprunts      │  │
│  └──────────┘  └────────────┘  └──────────────────┘  │
│                               ┌──────────────────┐   │
│                               │   Reservations   │   │
│                               └──────────────────┘   │
└──────────────────────────────────────────────────────┘
```

**Flux d'authentification :**
```
connexion.php → vérification email + BINARY mot de passe
             → $_SESSION[user_id, Nom, Email, Role]
             → redirection selon rôle (Admin / Bibliothecaire / User)
```

---

## Modèle de Données

```sql
-- Livres : catalogue complet
Livres       (ID, Titre, Auteur, Categorie, Etat)
-- Etat : 'Disponible' | 'Indisponible' | 'Reserve'

-- Utilisateurs : tous les comptes
Utilisateurs (ID, Nom, Email, MotDePasse, Role, DateInscription)
-- Role : 'Admin' | 'Bibliothecaire' | 'User'

-- Emprunts : prêts actifs
Emprunts     (ID, Utilisateur_ID, Livre_ID, DateEmprunt, DateEcheance, Retard)

-- Réservations : files d'attente
Reservations (ID, Utilisateur_ID, Livre_ID, DateReservation)
```

---

## Installation

### Prérequis
- Serveur PHP 8.x (XAMPP, WAMP, Laragon...)
- MySQL 8.0+
- Navigateur moderne

### 1. Cloner le projet

```bash
git clone https://github.com/votre-repo/bibliotrackphp.git
cd bibliotrackphp
```

### 2. Créer la base de données

Importez le fichier SQL fourni dans phpMyAdmin ou via le terminal :

```bash
mysql -u root -p < base.sql
```

### 3. Lancer l'application

Placez les fichiers dans le dossier `htdocs` (XAMPP) ou `www` (WAMP), puis accédez à :

```
http://localhost/bibliotrackphp/connexion.php
```

### Comptes de test

| Rôle | Email | Mot de passe |
|---|---|---|
| Admin | admin@gmail.com | admin |
| Bibliothécaire | bib@gmail.com | bib |
| Utilisateur | user@gmail.com | user |

---

## Structure du Projet

```
bibliotrackphp/
├── connexion.php        # Authentification + redirection par rôle
├── inscription.php      # Création de compte (rôle User par défaut)
├── deconnexion.php      # Destruction de session + redirection
├── admin.php            # Dashboard Admin (CRUD livres + utilisateurs)
├── bibliothecaire.php   # Dashboard Bibliothécaire (prêts + retards)
├── user.php             # Dashboard Utilisateur (recherche + réservations)
├── style.css            # Styles globaux (background, cards, modals)
└── base.sql             # Script de création + données de test
```

---

## Idées d'Amélioration

Ces fonctionnalités pourraient être ajoutées dans une prochaine version.

**Sécurité** — Hachage des mots de passe avec `password_hash()` / `password_verify()` au lieu du texte brut. Utilisation de requêtes préparées partout (certaines utilisent encore `real_escape_string`).

**UX** — Pagination du catalogue pour les grandes bibliothèques. Confirmation visuelle avant suppression (au lieu d'un simple submit). Affichage du nom du livre dans les modals d'emprunt à partir de l'ID.

**Fonctionnel** — Envoi de rappels par email (PHPMailer) lors des retards. Historique complet des emprunts passés par utilisateur. Statistiques et tableau de bord analytique pour l'admin.

**Technique** — Migration vers PDO pour uniformiser les accès base de données. Séparation de la logique métier dans des fichiers dédiés (type MVC léger).

---

<div align="center">
<em>Projet académique — PHP + MySQL + Bootstrap</em>
</div>
