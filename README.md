# PhoneTech Accessories : Site vitrine WordPress

Activité pratique du module **Développement Full Stack et Microservices**.
Objectif : découvrir le fonctionnement d'un CMS monolithique en créant un site vitrine d'accessoires téléphoniques avec WordPress.

## 1. Présentation

**PhoneTech Accessories** est un site vitrine présentant des accessoires pour smartphones : coques, chargeurs, écouteurs, power banks et supports.

| Page | Contenu |
|------|---------|
| Accueil | Présentation de l'entreprise, accroche, produits phares, section « Pourquoi nous » |
| Catalogue | 10 accessoires gérés avec WooCommerce (5 catégories) |
| À propos | Mission, valeurs et présentation de l'entreprise |
| Blog | 3 articles (tendances, entretien, nouveautés technologiques) |
| Contact | Formulaire Contact Form 7 et coordonnées |

## 2. Environnement et outils

| Élément | Détail |
|---------|--------|
| Serveur local | XAMPP (Apache 2.4, PHP 8.2, MariaDB 10.4) |
| CMS | WordPress |
| Thème | Astra |
| Page builder | Elementor |
| E-commerce | WooCommerce |
| Formulaire | Contact Form 7 |
| SEO | Yoast SEO |
| Base de données | `phonetech` |
| URL locale | `http://localhost/phonetech` |

## 3. Étapes de réalisation

1. **Installation de l'environnement** : installation de XAMPP, démarrage d'Apache et MySQL, création de la base `phonetech` dans phpMyAdmin.
2. **Installation de WordPress** : téléchargement de WordPress, copie dans `C:\xampp\htdocs\phonetech`, configuration de `wp-config.php` et création du compte administrateur.
3. **Thème et plugins** : activation du thème Astra, installation d'Elementor, WooCommerce, Contact Form 7 et Yoast SEO.
4. **Catalogue WooCommerce** : création de 5 catégories (Coques, Chargeurs, Écouteurs, Power banks, Supports) et de 10 produits avec photo, prix et catégorie. La boutique est passée en mode « En direct ».
5. **Pages avec Elementor** : création des pages Accueil, À propos et Contact. Sections centrées, bouton « Voir le catalogue » relié à la page Catalogue.
6. **Formulaire de contact** : création du formulaire (nom, e-mail, sujet, message) et insertion du shortcode Contact Form 7 dans la page Contact, avec les coordonnées de l'entreprise.
7. **Blog** : publication de 3 articles avec image mise en avant et définition de la page « Blog » comme page des articles.
8. **Menu de navigation** : Accueil, Catalogue, À propos, Blog, Contact.
9. **Référencement (Yoast SEO)** : mot-clé principal, titre SEO et méta description pour l'accueil.
10. **Export** : export de la base de données (`phonetech.sql`) via phpMyAdmin et archive des fichiers (`phonetech.zip`).

### Problèmes rencontrés et solutions

- **MySQL ne démarrait plus** (table système `mysql.db` corrompue) : réparation avec `aria_chk -r` sur la table, puis redémarrage de MySQL.
- **Erreur MailPoet** (`max_allowed_packet`) : plugin non demandé, désactivé. La limite peut aussi être augmentée dans `my.ini` (`max_allowed_packet=64M`).
- **Modifications Elementor non visibles** : vérification de l'enregistrement et vidage du cache (Ctrl + Shift + R).

## 4. Architecture de WordPress

WordPress est un CMS **monolithique** écrit en PHP : l'interface d'administration, la logique métier, la gestion des utilisateurs et l'affichage public sont dans une même application, avec une seule base de données.

```
Navigateur (client)
        |
        v
Serveur web (Apache)
        |
        v
WordPress (PHP)
  |-- Noyau (wp-admin, wp-includes)
  |-- Thème (Astra)         -> affichage
  |-- Plugins               -> fonctionnalités
  |     Elementor, WooCommerce, Contact Form 7, Yoast SEO
        |
        v
Base de données MySQL/MariaDB (phonetech)
```

### Structure des dossiers

| Dossier / fichier | Rôle |
|-------------------|------|
| `wp-admin/` | Interface d'administration |
| `wp-includes/` | Noyau et bibliothèques de WordPress |
| `wp-content/themes/` | Thèmes (Astra) |
| `wp-content/plugins/` | Extensions (Elementor, WooCommerce, etc.) |
| `wp-content/uploads/` | Médias envoyés (images des produits et des articles) |
| `wp-config.php` | Configuration (accès à la base, clés de sécurité) |
| `index.php` | Point d'entrée de toutes les requêtes |

### Base de données

Tables principales (préfixe `wp_`) :

- `wp_posts` : pages, articles, produits, médias
- `wp_postmeta` : métadonnées (prix, données Elementor, SEO)
- `wp_users` et `wp_usermeta` : utilisateurs
- `wp_terms`, `wp_term_taxonomy`, `wp_term_relationships` : catégories et étiquettes
- `wp_options` : réglages du site

### Cycle d'une requête

1. Le navigateur demande une page.
2. Apache transmet la requête à `index.php`.
3. WordPress charge le noyau, le thème et les plugins actifs.
4. WordPress interroge la base de données.
5. Le thème génère le HTML, renvoyé au navigateur.

## 5. Architecture monolithique et microservices

| Critère | Monolithique (WordPress) | Microservices |
|---------|--------------------------|---------------|
| Structure | Une seule application | Plusieurs petits services indépendants |
| Base de données | Unique et partagée | Une base par service (souvent) |
| Déploiement | Tout en une fois | Service par service |
| Technologies | Une seule pile (PHP, MySQL) | Libres selon le service |
| Mise à l'échelle | Duplication de toute l'application | Seul le service chargé est multiplié |
| Développement | Rapide au départ, simple à démarrer | Plus complexe (réseau, API, orchestration) |
| Tolérance aux pannes | Une panne peut bloquer tout le site | Une panne reste isolée |
| Maintenance | Se complique quand le code grossit | Équipes autonomes par service |
| Tests | Simples au début | Tests d'intégration plus lourds |
| Coût | Faible (un serveur suffit) | Plus élevé (infrastructure, supervision) |

### Application au projet PhoneTech

Pour un site vitrine, l'approche monolithique est adaptée : mise en place rapide, peu de ressources, écosystème de plugins riche.

Si l'entreprise grandit, une architecture microservices pourrait séparer :

- un service **Catalogue** (produits, stocks)
- un service **Commandes et paiement**
- un service **Utilisateurs** (authentification)
- un service **Contenu** (blog) ou un CMS headless
- un service **Notifications** (e-mails)

Chaque service aurait sa base et communiquerait par API REST, derrière une passerelle API (API Gateway). WordPress peut d'ailleurs servir de CMS headless grâce à son API REST.

## 6. Livrables

```
PhoneTech_export/
|-- phonetech.sql   # export de la base de données
|-- phonetech.zip   # fichiers du site (htdocs/phonetech)
|-- README.md
```

### Restauration du site

1. Installer XAMPP et démarrer Apache et MySQL.
2. Dézipper `phonetech.zip` dans `C:\xampp\htdocs\`.
3. Créer la base `phonetech` dans phpMyAdmin, puis **Importer** `phonetech.sql`.
4. Vérifier les identifiants de la base dans `wp-config.php`.
5. Ouvrir `http://localhost/phonetech`.

## 7. Conclusion

Ce TP a permis de construire un site vitrine complet avec WordPress et de comprendre le fonctionnement d'un CMS monolithique : noyau, thème, plugins et base de données. Il a aussi montré ses limites (dépendance aux plugins, couplage fort, mise à l'échelle globale), ce qui introduit l'intérêt des microservices.
