![EURL ARVODIA Logo](https://raw.githubusercontent.com/arvodia/src/main/img/arvodia-logo.png)

## Table des matières
  - [Introduction](#introduction)
  - [À propos de Webfony](#a-propos-de-webfony)
  - [Objet](#objet)
  - [Principe de fonctionnement](#principe-de-fonctionnement)
      - [Services](#services)
        - [a) Front Office](#a-front-office)
        - [b) Back Office](#b-back-office)
        - [c) Multimédia](#c-multimedia)
  - [Code source](#code-source)
  - [Utilisation](#utilisation)
      - [Installation](#installation)
      - [Configuration](#configuration)
      - [Créer un administrateur](#creer-un-administrateur)
      - [Autorisation de fichiers](#autorisation-de-fichiers)
      - [Sauvegarder](#sauvegarder)
      - [Tâches Cron](#taches-cron)
      - [Test](#test)
  - [Rôle utilisateur](#role-utilisateur)
  - [Manuel](#manuel)
  - [Auteur](#auteur)
  - [Contact](#contact)
  - [License](#license)

Introduction
------------
Une vidéo d'introduction courte sur ce YouTube :
[ARVODIA Webfony intro](https://youtu.be/Eyrqmxy7RDI)

À propos de Webfony
-------------------

Webfony est un système de gestion de contenu (CMS), à la pointe de la technologie avec le puissant framework PHP Symfony, des outils de design et d'édition de texte d'une qualité haut de gamme (EditorJs, Bootstrap 5). destiné à la conception et la mise à jour dynamique des application web sans l'intervention de web-master pour l’évolution de l'information et de contenu du site.

Développer par l'agence ARVODIA en 2021, l'auteur de code **S.S.Redouane** est un développeur web des années 2000, a obtenu sa première attestation de "Conception de Site Web" en 2004, et sont diplôme "Développeur Web et Multimédia" en 2011, Avec mention 18.5/20 à "L'institut National Spécialise de la Formation Professionnelle en Arts et Industries Graphiques de Blida", le thème "Conception et Réalisation d'un Site Web Dynamique avec un Système de Gestion des Contenus" en « PHP pur ». En 2014 il a fondé l'agence de communication EURL ARVODIA.

Objet
-----

*Pourquoi sommes-nous intéressés ?* 

Aujourd’hui, PHP est le langage de script côté serveur le plus couramment utilisé pour le développement applications Web puissants et robustes. L'intégration d'un framework solide et stable telle que Symfony avec ces nombreux composent facilite l’évolution et l’immigration de projet, donc une longue durée de vie avant de refaire le site web. en finale le rendu HTML c'est la meilleure façon pour le référencement.

*Sous quel angle ?*

Un long processus a été fait pour rassembler toute cette technologie et faire un système de gestion de contenu, prêt pour la production et aussi un projet de base pour démarrer un grand projet.

*Quel est le problème ?* 

Autrefois, pour mettre en place un site internet, il fallait un système de gestion de contenu, est-ce que généralement ces systèmes sont trop génériques avec des fonctionnalités qui ne seront pas utilisées, et en plus il faut ajouter des modules et des configurations, pour obtenir les principaux concepts d'un site web voir connexion, publication, multilinguisme, ..etc 

- Problèmes : 
  - Des systèmes trop génériques.
  - Une difficulté d'entretien.
  - Durée de vie du site Web plus courte. 

Aujourd'hui avec des développeurs expérimentés vous pouvez avoir des projets plus spécifiques, et avec une longue durée de vie.

La question n'est plus de savoir si vous avez besoin d'une application web, mais comment ? avec quels outils ? et avec quels moyens ?

Principe de fonctionnement
--------------------------

La séparation entre contenu et présentation est un principe fondateur de la gestion de contenu :

Le contenu est le plus souvent stocké dans une base de données, structurée en tables et champs. C'est le contenu des champs de la base de données qui est créé et modifié par l'éditeur, et non la page elle-même. On parle de site "dynamique".

La présentation est définie par la mise en page elle-même - via des feuilles de style (dont CSS), la structuration des données et des informations extraites de la base de données (ainsi que l'endroit où elles doivent être affichées et dans quelles conditions).

### Services

#### a) Front Office

Le Front Office, accessible par les internautes. Cependant, une construction ordonnée des articles selon leur rubrique ou leur catégorie. Condition de navigation et de recherche : c'est-à-dire un site agréable à naviguer pour une bonne accessibilité à toutes les informations publiées, plan du site, flux RSS, formulaire de contact.

#### b) Back Office

Le Back Office fait partie d'un site Web ou d'un système informatique. Une traduction back office possible est arrière-guichet, ou arrière-boutique. Il s'agit de l'espace sécurisé qui permet l'administration et la gestion de son site. La gestion de contenu peut être trouvée en tant que service, par exemple : ajout, modification d'articles, gestion (rubriques, commentaires, utilisateurs, paramètres du site, apparence, langues, multimédia, ... etc.). 

#### c) Multimédia

Ceci est nouveau dans (CMS), il ne fait pas partie du front office puisqu'il faut être connecté et avoir des autorisations, il ne fait pas partie du back office puisqu'il n'a aucun lien avec l'administration du site. Il fait partie de la gestion du stockage des fichiers utilisateurs : photo, audio, vidéo, document, liens YouTube avec un système de gestion d'albums, corbeille et lecture multimédia.

Code source
-----------

Webfony est un produit propriétaire, pour obtenir le code source merci de nous envoyer une demande d'achat avec vos coordonnées par e-mail à cette adresse [arvodia@hotmail.com](mailto:arvodia@hotmail.com)

Utilisation
-----------

### Installation

Installation d'un nouveau projet Webfony décompresser les fichiers sources dans un répertoire puis importer les dépendances avec composer

```
$ tar xzf webfony.tar.gz
$ cd webfony/
$ composer install
```
 
Pour la production
```
$ SYMFONY_ENV=prod SYMFONY_DEBUG=0 composer install -o --no-dev
```

>`Remarque` : la commande "composer install" installera automatiquement la configuration avec la commande "webfony:file:backup" 

### Configuration

Pour une configuration interactive, tapez la commande suivante :

```
$ php bin/console webfony:install
```

Pour une configuration manuelle voir dans le [manuel](https://github.com/arvodia/webfony/blob/main/src/Resources/docs/manual/fr.md)

### Créer un administrateur

exécutez la commande suivante :

```
$ php bin/console webfony:account:add admin-name password admin@example.com --role=ROLE_SUPER_ADMIN
```

### Autorisation de fichiers

Si vous le souhaitez, vous pouvez modifier l'autorisation des fichiers avec la commande suivante :

```
$ php bin/console webfony:permission -h
```
vous pouvez changer l'option "--prod" en "--dev" pour le mode developpement, ou ajouter l'option "--chown-user" pour changer le nom d'utilisateur des fichiers.

>`Remarque` : Nous ne connaissons pas encore l'efficacité de la commande pour plus de détails sur le sujet voir [file_permissions](https://symfony.com/doc/current/setup/file_permissions.html)

### Sauvegarder

vous pouvez créer une sauvegarde de votre base de données et d'autres fichiers multimédias avec la commande suivante :

```
$ php bin/console webfony:backup -h
```

vous pouvez également créer une sauvegarde unique de certains fichiers sources, ou les importer avec la commande suivante : 

```
$ php bin/console webfony:file:backup -h
```

pour ajouter un nouveau fichier à la liste 'file:backup' copiez-le dans le dossier "src/Resources/backup/webfony/", il sera alors automatiquement détecté par la commande si le fichier est modifié.
la commande "webfony:file:backup" permet notamment de restaurer une version des fichiers de configuration s'ils sont écrasés par une mise à jour.
 
### Tâches Cron

Planifiez des tâches Cron sur votre serveur :

```
* * * * * /var/www/webfony/bin/console webfony:cron > /dev/null 2>&1
```
 
### Test

les classes du dossier de test, elles ne sont pas complètes, si vous le souhaitez, vous pouvez ajouter un autre test, pour démarrer le test avec la commande suivante :

```
$ php bin/console webfony:test --env=test --purge
```

Rôle utilisateur
----------------

Un rôle définit les autorisations pour les utilisateurs d'effectuer un groupe de tâches. Ces rôles sont définis sur la page de modification du compte utilisateur.

Les utilisateurs qui ne sont pas authentifiés ont le rôle d'utilisateur anonyme. Les utilisateurs authentifiés ont le rôle d'abonné.

[voir plus de détails sur les rôles.](https://github.com/arvodia/webfony/blob/main/src/Resources/docs/roles/fr.md)

Manuel
------

pour plus de détails voir [manuel ](https://github.com/arvodia/webfony/blob/main/src/Resources/docs/manual/fr.md)

Auteur
------

Webfony a été créé par **sidi said redouane**

Contact
-------

[arvodia@hotmail.com](mailto:arvodia@hotmail.com) - EURL ARVODIA

[sidisaidredouane@live.com](mailto:sidisaidredouane@live.com) - SIDI SAID REDOUANE

License
-------

**Tous les droits sont réservés**
