![EURL ARVODIA Logo](/src/img/arvodia.png)

La documentation n'est pas complète sur tout le code et l'utilisation, mais elle parle des quelques lignes principales.
Tout d'abord je vous invite à lire [Lisez-moi](https://github.com/arvodia/webfony/blob/main/LISEZ-MOI.md) et [Rôle utilisateur](https://github.com/arvodia/webfony/blob/main/src/Resources/docs/roles/fr.md)

## Table des matières
  - [Configuration](#configuration)
      - [Base de données](#base-de-donnees)
      - [Mailer](#mailer)
      - [Plan du site](#plan-du-site)
      - [Site Web](#site-web)
  - [Mettre à jour](#mettre-a-jour)
  - [Datafixtures](#datafixtures)
  - [Tâches Cron](#taches-cron)
  - [Contenu](#contenu)
  - [Asset et bibliothèque](#asset-et-bibliotheque)
      - [Icônes](#icones)
      - [Développement](#developpement)
  - [Twig Extension](#twig-extension)
  - [Routing et Référencement](#routing-et-referencement)
      - [Nom des routes](#nom-des-routes)
      - [Translate Routes](#translate-routes)
      - [FrontPageController](#frontpagecontroller)
  - [Les Constantes](#les-constantes)
      - [Menu catégorie](#menu-categorie)
      - [Résultat maximum du SiteMap](#resultat-maximum-du-sitemap)
      - [Type de stockage de configuration](#type-de-stockage-de-configuration)
      - [Clone Config](#clone-config)
  - [Terminologie](#terminologie)
  - [Traduction](#traduction)
      - [Langues supplémentaires](#langues-supplementaires)
      - [Traduction de l'interface utilisateur](#traduction-de-l-interface-utilisateur)
  - [DataTable](#datatable)
  - [Sessions](#sessions)
      - [Le système de stockage des sessions](#le-systeme-de-stockage-des-sessions)
      - [Personnes Sessions](#personnes-sessions)
  - [Éditeurs](#editeurs)
      - [exemple de syntaxe de markdown](#exemple-de-syntaxe-de-markdown)
      - [exemple de plugin editorJS](#exemple-de-plugin-editorjs)
  - [Multimédia](#multimedia)
  - [Logs](#logs)
  - [Cache](#cache)
  - [Améliorer le projet](#ameliorer-le-projet)

Configuration
-------------

Configuration manuellement de Webfony

### Base de données

Les bases de données supportées par Webfony sont : MySQL, MariaDB, PostgreSQL, SQLite.

Pour configurer une connexion à une base de données, modifiez la variable **DATABASE_URL** dans le fichier `.env` et exécutez la commande suivante, pour créer les tables

```
$ php bin/console doctrine:migrations:migrate
```
Avec la commande `doctrine:schema:update` la table de session ne sera pas ajoutée donc il est recommandé d'utiliser les migrations.
Dans le dossier `src/Ressources/migrations/` les migrations pour chaque type de base de données (mysql, postgresql et sqlite), si vous avez utilisé la commande `webfony:install` elles seront importées automatiquement.

### Mailer

Vous pouvez envoyer des emails via SMTP en configurant **MAILER_DSN** dans votre fichier `.env`.

### Plan du site

Configuration du chemin vers votre sitemap :

Dans le fichier `public/robots.txt` modifiez la variable Sitemap, et ajoutez l'host de votre site web, respectez http ou https.

Exemple : https://www.example.net/sitemap.xml

### Site Web

Dans le fichier `.env` modifiez les variables **SITE_BASE_HOST** et **SITE_BASE_SCHEME**
ces deux dernières configuration sont également utilisées dans l'envoi de mail par la console pour la création de lien.

Une fois connecté, vous pourrez configurer le site sur : `/admin/system/site-information`

Mettre à jour
-------------

Mettre à jour les dépendances et Symfony

```
$ symfony composer update
```

affichage des recettes symfony, avec la commande :

```
$ symfony composer recipes
```

et voir dans les détails, comment mettre à jour une recette.

vous pouvez utiliser la commande suivante après la mise à jour pour enregistrer ou restaurer des fichiers, en particulier restaurer des configurations si elles sont écrasées par des mises à jour.

````
$ php bin/console webfony:file:backup
````

Datafixtures
------------
Webfony a sa propre commande pour les datafixtures.
elle permet, d'ajouter de fausses données avec la possibilité de créer des articles sans fin.

```
sc w:d:f -p -n
```

l'option -p pour purger, vous pouvez également supprimer tous les fichiers pour recommencer à zéro avec :

````
rm -fr private/* public/src/files/* var/* vendor/ public/src/libs/ node_modules/
````

Tâches Cron
---------

Webfony utilise Messenger pour certaines tâches telles que l'envoi de courrier, et le comportement de Messenger a été modifié pour qu'il cesse de fonctionner s'il n'a plus de messages à consommer, il est intégré à webfony cron avec les autres crons.
si vous voulez changer ce comportement pour que Messenger soit actif tout le temps sans s'arrêter, voyez la constante `AUTO_STOP_WORKER` dans la classe `src/Config/Consts.php`

il y a plusieurs cron à exécuter tels que :
  - purger les médias et les commentaires dans la corbeille (intervalle de 24 heures)
  - génération chartJS pour les médias (intervalle de 24 heures)
  - génération chartJS pour les sessions (intervalle de 30 jours)
  - génération de sauvegarde de la base de données, des fichiers média ou plus (intervalle 30 jours)
  - consomme des messages (intervalle 1 minute)

puisque la commande cron calcule elle-même l'intervalle, planifiez donc l'exécution de la tâche Cron toutes les minutes.

````
* * * * * /var/www/webfony/bin/console webfony:cron > /dev/null 2>&1
````

Contenu
--------
Le contenu principal de webfony est constitué de rubrique et d'article, les catégories sont des rubriques avec une rubrique parent.

Le système a une logique stricte pour afficher le contenu sur le front office, le contenu doit être dans une langue prise en charge par son parent, sinon il ne sera pas affiché même si son statut est publié.
ce comportement peut être modifié en activant l'option "Langage toutes" dans la partie `/admin/regional/language`

on suppose qu'un article a tout ce qu'il faut pour un contenu front office, voir un titre, une description et un corps, il suffit donc de créer une relation OneToOne avec l'entité article pour avoir un autre contenu front office, par exemple un nouveau produit ou entité d'addition.
ainsi on a évité la dynamique de l'entité rubrique pour quelle s'adapte pour toutes les autres entités puisque le projet se limite à ne pas être trop générique.

mais si vous voulez aller plus loin vous pouvez ajouter un champ de rubrique pour faire référence à une entité, mais dans ce cas toute la logique doit être modifiée
voir le contrôleur FrontPageController, RubriqueRepository et la nouvelle entité doivent avoir la même logique que ArticleRepository

la fonction findByLocales() dans la classe RubriqueRepository elle permet de générer le cache de rubrique, cette fonction utilise un "Recursive queries - SQL" qui commence par sélectionner toutes les langues puis pour chaque langue recherche rubrique, puis les catégories à la fin de voir s'il contient au moins un contenu publié pour afficher la rubrique publiée.

Asset et bibliothèque
-------------

Webfony utilise le plugin [grouper](https://github.com/arvodia/grouper) pour composer, pour la gestion des asset et avec les tâches de grouper qu'ils permettent de copier et minifier les fichiers CSS/JS, dans le dossier public automatiquement lors de l'installation ou de mettre à jour.
si vous modifiez les asset de l'application, vous devez alors minifier les fichiers CSS et JS de l'application avec la commande de grouper suivante :

````
$ composer grouper:task webfony-bs5 -r
````

l'option -r pour "exécuter des tâches",
vous pouvez ajouter d'autres tâches ou groupe dans le fichier grouper.json, ou de manière interactive avec les commandes suivantes :

````
$ composer grouper:group
$ composer grouper:task
````

  - Note
    - Les fichier app.css ou app.js sont exécuté sur le back et front office
    - Le menu principal et secondaire sont équipés d'un défilement horizontal

### Icônes

webfony utilise des SVG pour les icônes, exemple : <svg><use href="#globe"/></svg>
les fichiers svg `public/src/svg/app` sont compilés en un seul fichier `templates/bs5/svg/app.svg` avec la commande Grunt suivante :

````
# install npm dependances

$ npm install

# Combine multiple files

$ grunt
````

>`Remarque` : les fichiers "public/src/favicon/favicon.svg" et "public/src/favicon/apple-touch-icon.png" sont utilisés par le système

### Développement

en mode développement le navigateur internet conserve le cache des asset,
il y a une constante `ASSET_CACHE` dans le fichier` src/Config/Consts.php`, qui change ce comportement (avec twig filter |addKey), et aussi dans `public/src/bs5/js/app.js` la constante` wfAssetNoCache`

Twig Extension
------------------------

webfony ajoute plus de 45 fonctions et filtres de twig parmi ces fonctions celle qui crée des liens exemple linkAction(entité, action), il permet d'avoir un lien vert le contrôleur crud de l'entité, donc le paramètre d'action peut être ('show', 'edit', 'trans', 'delete' ou 'page'), son nom d'action est le suffixe du nom de la route, il est pour cela recommandé d'utiliser la commande `webfony:generate:crud` pour générer le crud.
il y a aussi la fonction generatePathPage() qui permet d'avoir le lien publié sur le front office d'un article, tout en tenant compte de la traduction.

Routing et Référencement
------------------------

les pages des articles et rubriques sont automatiquement référencées dans les fichiers sitemap
les routes des contrôleurs d'action sont également référencées, si elles n'ont pas de paramètres obligatoires (sans valeur par défaut), et à chaque fois que vous ajoutez un contrôleur d'action, vous devez mettre à jour les noms des routes avec la commande suivante :

```
$ symfony console webfony:reserved
```

la commande génère également des mots réservés, utilise de manière incrémentale des slugs avec le même chemin que les routes, et aussi pour l'automatisation du référencement de nouvelles routes.

### Nom des routes

Les routes d'administration doivent commencer par `admin_`

### Translate Routes

si multilingue est actif, les routes sont automatiquement préfixées par `_local` sinon le préfixe est vide
si vous souhaitez ajouter une route sans préfixe `_local`, ajoutez-le dans `src/Resources/config/routes.yml` au bas du fichier.

et forcez l'installation de la configuration avec cette instruction :

````
/** @var App\Config\Config $config */
$config->load('routes')->delete();
````

### FrontPageController
la route "front_page" avec une priorité de "-20" s'exécute en dernier une fois qu'il n'y a plus de route à exécutée et cette route convertit l'URL en une requête SQL, selon le type de configuration des adresses URL dans le `/admin/system/site-information`
  -  Un niveau         : /rubique|categorie-1|categorie-1-2|article
  -  Deux niveaux      : /rubique|categorie-1|categorie-1-2/article
  -  Trois niveaux     : /rubique/categorie-1|categorie-1-2/article
  -  Profondeur Infini : /rubique/categorie-1/categorie-1-2/article

Vous pouvez choisir comment l'adresse de la page est calculée. La barre verticale (|) dans les exemples, signifiait "ou".

Les Constantes
---------------------

même si webfony est équipé d'un petit système de gestion de configuration (base de données, YAML et JSON) dynamiquement, mais il a aussi des constances que vous retrouveriez dans la classe `src/Config/Consts.php`

Autres constantes pouvant être modifiées dans d'autres classes PHP :

### Menu catégorie

La constante `ROOT_TREE`, dans la class `src/Twig/RubriqueExtension.php`, il permet de choisir le début de l'arborescence
  - TRUE  -> début de l'arborescence à partir de rubrique racine
  - FALSE -> début de l'arborescence de la rubrique courante

### Résultat maximum du SiteMap

Résultat maximum du SiteMap est limité à 50000, le nombre d'URLs que Google récupère dans le sitemap
si vous voulez changer le nombre de résultats, c'est dans la constante `FULL_MAX`, dans la class `src/Repository/ArticleRepository.php`

### Type de stockage de configuration

La constante `TYPE_STOCK`, dans la class `src/Config/Config.php`,
il vous permet de choisir où enregistrer la configuration, dans la base de données, le fichier YML ou les deux
il y a aussi la constante FORCE_USE_YAML une liste pour forcer la sauvegarde au format yaml.
ne touchez pas à moins que vous ne soyez conscient du changement.

### Clone Config

Changer le comportement de la configuration de classe

Exemple:
```
$config_1 = $config->load('file_1');
dump($config_1->get()); // afficher les données de file_1
$config->load('file_2');
dump($config_1->get()); // afficher les données de file_2
```

pour que $config_1->get() afficher les données de file_1 dans la deuxième utilisation
méthode 1 : changer la constante `CLONE_LOAD` to true
méthode 2 : $config_1 = clone $config->load('file_1');

>`Remarque` : les CHMOD utilisés par la commande `webfony: permission` ils n'ont rien à voir avec la constante `CHMOD` de la classe `Consts.php`

Terminologie
-----

Le terme page désigne l'affichage de contenu sur la partie front-office.
Le terme show signifie l'affichage de contenu sur la partie back-office.
Il existe également une déférence entre le contenu de la page et la page qui permet de modifier le nombre de résultats à afficher.

Traduction
----------

webfony gère l'activation d'une langue de manière dynamique sur la page `/admin/regional/language`
la langue par défaut (*) permet d'afficher le contenu dans toutes les langues.
la langue par défaut (*) est un paramètre simple mais sa programmation ajoute une vérification supplémentaire et donc des performances inférieures, vous pouvez activer ou désactiver le paramètre sur la page `/admin/regional/language`

### Langues supplémentaires

la configuration initiale de Webfony il vous permet de choisir entre 22 langues, si vous souhaitez ajouter une langue, commencez par ajouter le code de la langue dans le paramètre `app.supported_locale` du fichier `config/services.yaml`, et créez les fichiers de traduction dans le répertoire `/translations/` avec la commande suivante :

```
$ symfony console translation:update --force localeCode
```
à la fin pour activer votre nouvelle langue allez sur la page `/admin/regional/language`

>`Remarque` : afin de ne pas casser le code évite de changer les configurations manuellement ou avec la classe Config.php, car il y a certaines config qui sont liées à des scripts, il est donc recommandé d'utiliser les formulaires de la partie admin.

### Traduction de l'interface utilisateur

Les textes des traductions sont traduits automatiquement, ils peuvent donc contenir des erreurs, mais vous pouvez les corriger, modifier ou ajouter ces textes sur la page `/admin/regional/translate`

DataTable
---------

webfony utilise Datatable pour la liste des résultats, Datatable est également équipé d'un texte de traduction, si vous souhaitez mettre à jour ces textes voir le lien https://datatables.net/plug-ins/i18n/
ou télécharger depuis Github :
```
$ git clone https://github.com/DataTables/Plugins.git
```

après le téléchargement, déplacez les fichiers du répertoire `/i18n/` vers le répertoire `/public/src/bs5/js/datatables/translate/`

Sessions
----------------

Remarque : Si vous souhaitez stocker des sessions dans une base de données, un champ BLOBtype stocke jusqu'à 64 Ko. Si les données de session utilisateur dépassent cette valeur, une exception peut être levée ou leur session sera réinitialisée en silence.
Envisagez d'utiliser un MEDIUMBLOB si vous avez besoin de plus d'espace.

### Le système de stockage des sessions

dans le fichier `config/packages/framework.yaml`
pour le stockage dans la base de données utilisée
`handler_id: Symfony\Component\HttpFoundation\Session\Storage\Handler\PdoSessionHandler`
pour le stockage dans des fichiers utilisée
`handler_id: session.handler.native_file`

### Personnes Sessions

La page `/admin/users/session` il est basique mais permet de voir les internautes en ligne avec filtrage, soit vous utilisez la session de base pour donner soit la session de fichier.
la classe SessionManager a une fonction BETA qui permet à un utilisateur de se déconnecter d'autres sessions actives, mais c'est BETA car cela ne fonctionne pas si l'utilisateur est connecté avec des sessions de cookies, mais si vous désactivez un compte, webfony utilise un autre système pour forcer l'utilisateur à se déconnecter.

Éditeurs
--------

webfony est équipé de plusieurs éditeurs de texte (editorjs, trumbowyg, markdown), mais d'une version personnalisée par exemple qui permet l'affichage de média ou chartJS avec editorjs, ou autre amélioration.

Les balises h1 ne sont pas autorisées car le titre de l'article est avec une balise h1, et avoir plusieurs balises h1 n'est pas recommandé, vous pouvez modifier ce comportement uniquement pour markdown dans la constante `MD_H1_TO_H2` de la classe `Consts.php`.

### exemple de syntaxe de markdown

````
video : ![wf-video](/src/files/2021-08/ma-video.mp4 "/src/files/2021-08/ma-video.mp4")
youtube : ![wf-iframe](https://www.youtube.com/embed/xxxxxxxxxx "https://www.youtube.com/embed/xxxxxxxxxx")
````

### exemple de plugin editorJS

un exemple simple comment ajouter un plugin de editorJS
  -  $ composer req npm-asset/editorjs--simple-image
  -  grouper.json : add require (simple-image) with tasks "file-mapping-overwrite"
  -  grouper.json : in editorjs add tasks "js-minifying-overwrite" to add the file to  editorjs.full.min.js
  -  in public/src/js/editor/wfeditorjs.js function ejsGetConfig() add "image: SimpleImage"
  -  in src/Resources/editors/editorjs.json : add "image" with allowedTags and type
  -  in src/Manager/EditorManager.php : add image in function editorjsToHtml()

Configuration : editorjs.json
````
"image": {"url": {"type": "string","allowedTags": ""},"caption": {"type": "string","allowedTags": ""},"withBorder": {"type": "bool","allowedTags": ""},"withBackground": {"type": "bool","allowedTags": ""},"stretched": {"type": "bool","allowedTags": ""},"align": {"type": "string","canBeOnly": ["left", "center", "right", "justify"]}}
````

Multimédia
----------

La configuration multimédia peut être définie sur la page `admin/media`, est-il recommandé d'utiliser la limitation de la taille de téléchargement d'un fichier inférieur à celui autorisé par php.ini
les fichiers multimédias peuvent être publics ou privés, leur chemin peut donc être :

private : https://www.example.com/src/private/show/2021-08/ma-photo.jpg
public  : https://www.example.com/src/files/2021-08/ma-photo.jpg

private download : https://www.example.com/src/private/download/2021-08/ma-photo.jpg
public  : https://www.example.com/src/download/2021-08/ma-photo.jpg

Le système permet d'appliquer trois filtres sur une image (size|scale|max),  il suffit d'ajouter la taille et le filtre dans l'URL, exemple :

/src/files/2021-08/ma-photo-size-240x180.jpg
/src/files/2021-08/ma-photo-scale-240x180.jpg
/src/files/2021-08/ma-photo-max-240x180.jpg

il n'y a que certaines tailles autorisées, pour ajouter une autre taille voir la constante `ALLOWED_SIZE` dans la classe` FileController.php`

>`Remarque` : dans l'édition d'article, seul l'auteur de l'article peut ajouter un média, un administrateur ne peut supprimer qu'un média.

dans l'édition de l'article on peut ajouter un média dans le corps ou sur le champ Multimédia, et les médias qui sont sur le champ Multimédia leur statut ne peut plus être modifié si l'article est publié, contrairement aux médias qui se trouvent sur le champ corps qui peut être modifié par celui qui a accès à l'édition de l'article.

Logs
--------

Seuls les journaux d'application sont enregistrés dans la base de données et peuvent être consultés sur la page du tableau de bord.

Cache
--------

Le système utilise un propre cache de webfony pour les rubriques publiées et ensuite avec ce cache recherche uniquement les articles en fonction de ces sections. 
mais aussi un cache http pour les articles récents et autres informations pas trop dynamiques.
à la fin un autre cache pour le rendu des éditeurs de texte.

>`Remarque` : le "Temps d'exécution total" de la barre du profileur en mode dev, il s'avère qu'il ne s'affiche pas correctement à cause du cache render_esi()

vous pouvez voir le cache http avec cette commande :

````
$ curl -s -I -X GET https://127.0.0.1:8000/
````

Améliorer le projet
-------------------

  - Ajout de la limitation de la taille du dossier de stockage multimédia de chaque utilisateur
  - Ajouter l'entité tags pour l'article
  - Ajouter l'entité caractéristique du produit et établir une relation OneToOne avec l'entité article
  - Un système de newsletter
  - Verrouillage de l'article à éditer par un seul utilisateur
  - Système de notifications instantanées (exemple avec Mercury)
  - Ajout du filtrage par date dans la barre latérale du front office
  - Formulaire de contact personnel pour chaque utilisateur (ou messagerie instantanée)  