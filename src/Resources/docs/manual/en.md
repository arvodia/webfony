![EURL ARVODIA Logo](/src/img/arvodia.png)

The documentation it is not complete on all the code and the use, but it speaks of the few main lines
First of all I invite you to read [ReadMe](https://github.com/arvodia/webfony/blob/main/README.md) and [User role](https://github.com/arvodia/webfony/blob/main/src/Resources/docs/roles/en.md)

## Table of contents
  - [Configuration](#configuration)
      - [Database](#database)
      - [Mailer](#mailer)
      - [Sitemap](#sitemap)
      - [Website](#website)
  - [Update](#update)
  - [Datafixtures](#datafixtures)
  - [Cron Job](#cron-job)
  - [Contents](#contents)
  - [Asset and library](#asset-and-library)
      - [Icons](#icons)
      - [Development](#development)
  - [Twig Extension](#twig-extension)
  - [Routing et SEO](#routing-et-seo)
      - [Road name](#road-name)
      - [Translate Routes](#translate-routes)
      - [FrontPageController](#frontpagecontroller)
  - [Constants](#constants)
      - [Category menu](#category-menu)
      - [SiteMap Max Result](#sitemap-max-result)
      - [Configuration Storage Type](#configuration-storage-type)
      - [Clone Config](#clone-config)
  - [Terminology](#terminology)
  - [Traduction](#traduction)
      - [Additional languages](#additional-languages)
      - [User interface translation](#user-interface-translation)
  - [DataTable](#datatable)
  - [Sessions](#sessions)
      - [The session storage system](#the-session-storage-system)
      - [People Sessions](#people-sessions)
  - [Editors](#editors)
      - [example markdown syntax](#example-markdown-syntax)
      - [editorJS plugin example](#editorjs-plugin-example)
  - [Multimedia](#multimedia)
  - [Logs](#logs)
  - [Cache](#cache)
  - [Improve the project](#improve-the-project)

Configuration
-------------

Manually configuring Webfony

### Database

The databases supported by Webfony are : MySQL, MariaDB, PostgreSQL, SQLite.

To configure a connection to a database, modify the variable **DATABASE_URL** in the `.env` file and run the following command, to create the tables

```
$ php bin/console doctrine:migrations:migrate
```
With the command `doctrine:schema:update` the session table will not be added so it is recommended to use migrations.
In the folder `src/Resources/migrations/` the migrations for each type of database (mysql, postgresql and sqlite), if you used the command `webfony:install` they will be imported automatically.

### Mailer

You can send emails via SMTP by configuring **MAILER_DSN** in your `.env` file.

### Sitemap

Path configuration to your sitemap :

In the file `public/robots.txt` modify the Sitemap variable, and add the host of your website, respects http or https.

Example : https://www.example.net/sitemap.xml

### Website

In the `.env` file modify the variables **SITE_BASE_HOST** and **SITE_BASE_SCHEME**
these last two configuration are also used in the sending of mail by the console for the creation of link.

Once logged in, you will be able to configure the site on : /admin/system/site-information

Update
-------------

Update dependencies and Symfony

```
$ symfony composer update
```

display of symfony recipes, with the command :

```
$ symfony composer recipes
```

see in the details, how to update a recipe.

you can use the following command after the update to save or restore files, in particular restore configurations if they are overwritten by updates.

````
$ php bin/console webfony:file:backup
````

Datafixtures
------------

Webfony has its own command for datafixtures.
it allows, add fake data with the possibility of creating endless articles.

```
sc w:d:f -p -n
```

The -p option for purge, you can also delete all the files to start from scratch with

````
rm -fr private/* public/src/files/* var/* vendor/ public/src/libs/ node_modules/
````

Cron Job
---------

Webfony uses Messenger for certain tasks such as sending mail, and the behavior of Messenger has been changed so that it stops running if it has no more messages to consume, it is integrated into webfony cron along with other crons.
if you want to change this behavior so that Messenger is active all the time without stopping, see the constant `AUTO_STOP_WORKER` in the `src/Config/Consts.php` class

there are several cron to be executed such as
  - purge media and comments in trash (24 hour interval)
  - chartJS generation for the media (24 hour interval)
  - chartJS generation for sessions (30-day interval)
  - backup generation of the database, the media files or more (interval 30 days)
  - consumes messages (interval 1 minute)

since the cron command itself calculates the interval so schedule the execution of Cron Job every minute.

````
* * * * * /var/www/webfony/bin/console webfony:cron > /dev/null 2>&1
````

Contents
--------
The main content in webfony is section and article, categories are section with a parent section.

The system has a strict logic to display content on the front office, content must be with a language that is supported by its parent, otherwise it will not be displayed even if its status is published.
this behavior can be changed by activating the "Language all" option in the`/admin/regional/language`

we assume that an article has all that is necessary for a front office content, see a title, a description and a body, so it suffices to create a OneToOne relationship with the article entity to have another front office content, for example a new product or addition entity.
thus we avoided the dynamics of the section entity for which it adapts for all the other entities since the project is limited to not being too generic.

but if you want to go further you can add a section field to refer to an entity, but in this case all the logic needs to be changed
see the FrontPageController controller, RubriqueRepository and the new entity must have the same logic as ArticleRepository

the findByLocales () function in the RubriqueRepository class it allows you to generate the section cache, this function uses a "Recursive queries - SQL" which starts by selecting all the languages and then for each language searches for the section, then the categories at the end see if it contains at least one published content to display the published section.

Asset and library
-------------

Webfony uses the [grouper] plugin (https://github.com/arvodia/grouper) to compose, for asset management and with the grouper tasks they allow to copy and minify CSS/JS files, in the public folder automatically during installation or updates.
if you modify the application assets, then you have to minify the CSS and JS files of the application with the following grouper command :

````
$ composer grouper:task webfony-bs5 -r
````

the -r option for "run tasks",
you can add other tasks or group in the grouper.json file, or interactively with the following commands :

````
$ composer grouper:group
$ composer grouper:task
````

  - Note
    - The app.css or app.js files are executed on the back and front office
    - The Main and Secondary menu are equipped with a horizontal scroll

### Icons

webfony uses SVGs for icons, example : <svg><use href="#globe"/></svg>
les fichier svg "public/src/svg/app" sont compilée dans un seul fichier "templates/bs5/svg/app.svg"
the `public/src/svg/app` svg files are compiled into a single `templates/bs5/svg/app.svg` file with the following Grunt command :

````
# install npm dependances

$ npm install

# Combine multiple files

$ grunt
````

>`Remark` : the files "public/src/favicon/favicon.svg" and "public/src/favicon/apple-touch-icon.png" are used by the system

### Development

in development mode the internet browser keeps the asset cache,
there is a constant `ASSET_CACHE` in the file` src / Config / Consts.php`, which changes this behavior (with twig filter |addKey), and also in `public / src / bs5 / js / app.js` the constant` wfAssetNoCache`

Twig Extension
------------------------

webfony adds more than 45 functions and twig filters among these functions the one that creates links example linkAction(entity, action), it allows to have a green link the controller crud of the entity, so the action parameter can be ('show ', 'edit', 'trans', 'delete' or 'page'), its action name are the route name suffix, it is for its purposes it is recommended to use the `webfony command: generate: crud` to generate the crud.
there is also the generatePathPage() function which allows to have the link published on the front office of an article, all take into account the translation.

Routing et SEO
------------------------

the pages of the articles and sections are automatically referenced in the sitemap files
the action controllers routes are also referenced, if they have no required parameters (without default value), and each time you add an action controller, you have to update the names of the routes with the following command :

```
$ symfony console webfony:reserved
```

the command also generate reserved words, use incrementally slugs with the same path as the routes, and also for the automation of referencing new routes

### Road name

Admin routes must start with `admin_`

### Translate Routes

if multilingual is active, the routes are automatically prefixed with `_local` otherwise the prefix is empty
if you want to add a route without a `_local` prefix, add it in `src/Resources/config/routes.yml` at the bottom of the file.

and force the installation of the configuration with this instruction:

````
/** @var App\Config\Config $config */
$config->load('routes')->delete();
````

### FrontPageController
the "front_page" route with a priority of "-20" runs last once there is no more route executed and this route converts The URL to an SQL query, depending on the type configuration URL addresses in the `/admin/system/site-information`
  -  One level      : /section|category-1|category-1-2|article
  -  Two levels     : /section|category-1|category-1-2/article
  -  Three levels   : /section/category-1|category-1-2/article
  -  Infinite Depth : /section/category-1/category-1-2/article

You can choose how the page address is calculated. The vertical bar (|) in the examples, signified "or".

Constants
---------------------

even if webfony is equipped with a small configuration management system (database, YAML and JSON) dynamically, but it also has constancies that you would find in the `src/Config/Consts.php` class.

Other Constants which can be changed in other PHP classes :

### Category menu

The constant `ROOT_TREE`, in the class `src/Twig/RubriqueExtension.php`, it allows you to choose the beginning of the tree structure
  - TRUE  -> beginning of the tree structure from the root section
  - FALSE -> beginning of the tree structure from the current section

### SiteMap Max Result

Maximum SiteMap result is limited to 50,000, the number of URLs that Google retrieves in the sitemap
if you want to change the number of results, it's in The constant `FULL_MAX`, in the class `src/Repository/ArticleRepository.php`

### Configuration Storage Type

The constant `TYPE_STOCK`, in the class `src/Config/Config.php`
it allows you to choose Where to save the configuration, in database, YML file or both
there is also the constant FORCE_USE_YAML a list to force the save in a yaml format.
don't touch unless you are aware of the change.

### Clone Config

Change the behavior of the class config

Example:
```
$config_1 = $config->load('file_1');
dump($config_1->get()); // show data of file_1
$config->load('file_2');
dump($config_1->get()); // show data of file_2
```

so that $config_1->get() show data of file_1 in the second use
method 1 : change the constant `CLONE_LOAD` to true
method 2 : $config_1 = clone $config->load('file_1');

>`Remark` : the CHMODs used by the `webfony: permission` command they have nothing to do with the CHMOD constant of the `Consts.php` class

Terminology
-----

The term page means the display of content on the front-office part.
The term show signifies the display of content on the back-office part.
There is also a deference between page content and page which allows to change the number of results to display.

Traduction
----------

webfony manages the activation of a language dynamically on the `/admin/regional/language`
the language default (*) it allows to display content in all languages.
la langue default (*) c'est un simple paramètre mais sa programmation ajouter des vérification supplémentaire et donc performances inférieures, you can enable or disable the setting on the page `/admin/regional/language`

### Additional languages

the initial configuration of Webfony it allows you to choose between 22 languages, if you want to add a language, start by adding the language code in the `app.supported_locale` parameter of the `config/services.yaml` file, and create the translation files in the directory `/translations/` with the following command:

```
$ symfony console translation:update --force localeCode
```
at the end to activate your new language go to the page `/admin/regional/language`

>`Remark` : in order not to break the code avoided to change the configurations manually or with the Config.php class, because there are some config which are linked to scripts, so it is recommended to use the forms of the admin part.

### User interface translation

The texts of the translations are translated automatically, so they may contain errors, but you can correct them, modify or add these texts on the page `/admin/regional/translate`

DataTable
---------

webfony use Datatable for the listing of results, Datatable is also equipped with translation text, if you want to update these text see the link https://datatables.net/plug-ins/i18n/
or download from Github :
```
$ git clone https://github.com/DataTables/Plugins.git
```

after download move files from directory `/i18n/` to directory `/public/src/bs5/js/datatables/translate/`

Sessions
----------------

Note: If you want to store sessions in a database, a BLOBtype field stores up to 64KB. If user session data exceeds this value, an exception can be thrown or their session will be silently reset.
Consider using a MEDIUMBLOB, if you need more space.

### The session storage system

in the file `config/packages/framework.yaml`
for storage in the database used
`handler_id: Symfony\Component\HttpFoundation\Session\Storage\Handler\PdoSessionHandler`
for storage in files used
`handler_id: session.handler.native_file`

### People Sessions

The page `/admin/users/session` it is basic but allows to see online users with filtering, either you use the basic session to give or the session of file.
the SessionManager class has a BETA function that allows a user to log out of other active sessions, but it is BETA because it does not work if the user is logged in with cookie sessions, but if you deactivate an account, webfony is using another system to force the user to log out.

Editors
-------

webfony is equipped with several text editors (editorjs, trumbowyg, markdown), but with a customized version for example which allows the display of media or chartJS with editorjs, or other improvement.

h1 tags are not allowed since the title of the article is with an h1 tag, and having multiple h1 tags is not recommended, you can change this behavior only for markdown in the constant `MD_H1_TO_H2` of the class `Consts.php`.

### example markdown syntax

````
video : ![wf-video](/src/files/2021-08/ma-video.mp4 "/src/files/2021-08/ma-video.mp4")
youtube : ![wf-iframe](https://www.youtube.com/embed/xxxxxxxxxx "https://www.youtube.com/embed/xxxxxxxxxx")
````

### editorJS plugin example

a simple example how to add a plugin editorJS
  -  $ composer req npm-asset/editorjs--simple-image
  -  grouper.json : add require (simple-image) with tasks "file-mapping-overwrite"
  -  grouper.json : in editorjs add tasks "js-minifying-overwrite" to add the file to editorjs.full.min.js
  -  in public/src/js/editor/wfeditorjs.js function ejsGetConfig() add "image: SimpleImage"
  -  in src/Resources/editors/editorjs.json : add "image" with allowedTags and type
  -  in src/Manager/EditorManager.php : add image in function editorjsToHtml()

Configuration : editorjs.json
````
"image": {"url": {"type": "string","allowedTags": ""},"caption": {"type": "string","allowedTags": ""},"withBorder": {"type": "bool","allowedTags": ""},"withBackground": {"type": "bool","allowedTags": ""},"stretched": {"type": "bool","allowedTags": ""},"align": {"type": "string","canBeOnly": ["left", "center", "right", "justify"]}}
````

Multimedia
----------

Multimedia configuration can be set on the page `admin/media`, is it recommended to use the limitation the upload size of a file lower than that allowed by php.ini
media files can be public or private, so their path can be:

private : https://www.example.com/src/private/show/2021-08/ma-photo.jpg
public  : https://www.example.com/src/files/2021-08/ma-photo.jpg

private download : https://www.example.com/src/private/download/2021-08/ma-photo.jpg
public  : https://www.example.com/src/download/2021-08/ma-photo.jpg

The system allows you to apply three filters on an image (size|scale|max), just add the size and the filter in the URL, example :

/src/files/2021-08/ma-photo-size-240x180.jpg
/src/files/2021-08/ma-photo-scale-240x180.jpg
/src/files/2021-08/ma-photo-max-240x180.jpg

there are only certain sizes that are allowed, to add another size see the constant `ALLOWED_SIZE` in the class` FileController.php`

>`Remark` : in article editing only the author of the article who can add media, an administrator can only delete a media.

in the article edition we can add a media in the body or on the Multimedia control, and the media which are on the Multimedia control their status can no longer be changed if the article is published, unlike in media which this find on the body field which can be changed by the one who has access to the editing of the article.

Logs
--------

Only the application logs are saved in the database, and they can be viewed on the dashboard page.

Cache
--------

The system uses a webfony's own cache for the published sections and then with this chache searches only for the articles according to these sections.
mais aussi un cache http pour les articles récentes et autre information pas trop dynamique.
at the end another cache for rendering text editors.

>`Remark` : the "Total execution time" of the profiler bar in dev mode it turns out not to display correctly due to render_esi () cache

you can see http cache with this command:

````
$ curl -s -I -X GET https://127.0.0.1:8000/
````

Improve the project
-------------------

  - Add the limitation on the size of the media storage folder of each user
  - Add the tags entity for the article
  - Add the product characteristic entity and make a manyToMany relationship with the article entity
  - A newsletter system
  - Addition of filtering by date in the sidebar of the front office
  - Personal contact form for each user (or instant messaging)