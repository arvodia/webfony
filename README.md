![EURL ARVODIA Logo](https://raw.githubusercontent.com/arvodia/src/main/img/arvodia-logo.png)

## Table of contents
  - [Introduction](#introduction)
  - [About Webfony](#about-webfony)
  - [Object](#object)
  - [Working principle](#working-principle)
      - [Services](#services)
        - [a) Front Office](#a-front-office)
        - [b) Back Office](#b-back-office)
        - [c) Multimedia](#c-multimedia)
  - [Code source](#code-source)
  - [Use](#use)
      - [Installation](#installation)
      - [Configuration](#configuration)
      - [Create an administrator](#create-an-administrator)
      - [File authorization](#file-authorization)
      - [Backup](#backup)
      - [Cron Jobs](#cron-jobs)
      - [Test](#test)
  - [User role](#user-role)
  - [manual](#manual)
  - [Author](#author)
  - [Contact](#contact)
  - [License](#license)

Introduction
------------
A short intro video on this YouTube: 
[ARVODIA Webfony intro](https://youtu.be/Eyrqmxy7RDI)

About Webfony
-------------

Webfony is a content management system (CMS), at the cutting edge of technology with the powerful Symfony PHP framework, High quality text design and editing tools (EditorJs, Bootstrap 5). intended for the design and dynamic updating of web applications without the intervention of web-master for the evolution of information and site content.

Developed by the ARVODIA agency in 2021, the code author **S.S.Redouane** is a web developer from the 2000s, received his first "Website Design" certificate in 2004, and graduated "Web and Multimedia Developer" in 2011, With mention 18.5 / 20 at "The National Specialized Institute of Professional Training in Graphic Arts and Industries of Blida", the theme "Design and Realization of a Dynamic Website with a Content Management System" in "pure PHP". In 2014, he founded the communication agency EURL ARVODIA.

Object
------

*Why are we interested ?*

Today, PHP is the most commonly used server-side scripting language for developing powerful and robust web applications. The integration of a solid and stable framework such as Symfony with these many components facilitates the evolution and the immigration of projects, therefore a long lifespan before redoing the website. in the end, HTML rendering is the best way for SEO.

*From what angle ?* 

A long process has been done to bring all this technology together and make a content management system, ready for production and also a basic project to start a large project.

*What is the problem ?*

In the past, to set up a website, you need a content management system, is generally these systems are too generic with features that will not be used, and in addition you have to add modules and configurations, to obtain the main concepts of a website see connection, publication, multilingualism, ..etc

- Problems : 
  - Systems that are too generic.
  - A difficulty in maintenance.
  - Shorter website lifespan.

Today with experienced developers you can have more specific projects, and with a long lifespan.

The question is no longer whether you need a web application, but how? with what tools? and with what means?

Working principle
--------------------------

Separation between content and presentation is a founding principle of content management :

The content is most often stored in a database, structured in tables and fields. It is the content of the database controls that is created and modified by the editor, and not the page itself. We speak of a "dynamic" site.

The presentation is defined by the layout itself - via style sheets (among which CSS), the structuring of the data and the information extracted from the database (as well as where it should be. displayed and under what conditions).

### Services

#### a) Front Office

The Front Office, which is accessible by Internet users. However, an orderly construction of articles according to their heading or category. Condition navigation and search: that is to say, a pleasant site to navigate for good accessibility to all the information published, sitemap, RSS feed, contact form.

#### b) Back Office

The Back Office is a part of a website or a computer system. It concerns the secure space that allows the administration and management of its site. Content management can be found as a service, for example: adding, modifying articles, managing (sections, comments, users, site settings, appearance, languages, multimedia, ... etc.).

#### c) Multimedia

This is new in (CMS), it is not part of the front office since it is necessary to be connected and to have authorizations, it is not part of the back office since it has no link with the administration of the site. It is a part of management of the storage of user files: photo, audio, video, document, YouTube links with an album management system, recycle bin and multimedia playback.

Code source
-----------

Webfony is a proprietary product, to obtain the source code please send us a purchase request with your details by e-mail to this address [arvodia@hotmail.com](mailto:arvodia@hotmail.com)

Use
-----------

### Installation

Installation of a new Webfony project unzip the source files in a directory then import the dependencies with composer

```
$ tar xzf webfony.tar.gz
$ cd webfony/
$ composer install
```
 
For production
```
$ SYMFONY_ENV=prod SYMFONY_DEBUG=0 composer install -o --no-dev
```

>`Note`: the "composer install" command will automatically install the configuration with the "webfony:file:backup" command

### Configuration

For interactive configuration, type the following command :

```
$ php bin/console webfony:install
```

For a manual configuration see in the [manual](https://github.com/arvodia/webfony/blob/main/src/Resources/docs/manual/en.md)


### Create an administrator

run the following command :

```
$ php bin/console webfony:account:add admin-name password admin@example.com --role=ROLE_SUPER_ADMIN
```

### File authorization

If you want, you can change the permission of files with the following command :

```
$ php bin/console webfony:permission --prod
```
you can change the option "--prod" to "--dev" for development mode, or add the option "--chown-user" to change the username of files 

>`Note`: We do not yet know the effectiveness of the command for more details on the subject see [file_permissions](https://symfony.com/doc/current/setup/file_permissions.html)

### Backup

you can create a backup of your database and other media files with the following command :

```
$ php bin/console webfony:backup -h
```

you can also create a single backup of certain source files, or import them with the following command:

```
$ php bin/console webfony:file:backup -h
```

to add a new file to the 'file:backup' list copy it to the "src/Resources/backup/webfony/" folder, it will then be automatically detected by the command if the file is modified.
the "webfony:file:backup" command is used in particular to restore a version of the configuration files if they are overwritten by an update

### Cron Jobs

Schedule Cron Jobs on Your Server

```
* * * * * /var/www/webfony/bin/console webfony:cron > /dev/null 2>&1
```

### Test

the test folder classes, they are not complete, if you want, you can add another test, to start the test with the following command:

```
$ php bin/console webfony:test --env=test --purge
```

User role
---------

A role defines the permissions for users to perform a group of tasks. These roles are defined on the edit user account page.

Users who are not authenticated have the anonymous user role. Authenticated users have the role of subscriber.

[see more details on roles.](https://github.com/arvodia/webfony/blob/main/src/Resources/docs/roles/en.md)

manual
------

for more details see [manual](https://github.com/arvodia/webfony/blob/main/src/Resources/docs/manual/en.md)

Author
------

Webfony was created by **sidi said redouane**

Contact
-------

[arvodia@hotmail.com](mailto:arvodia@hotmail.com) - EURL ARVODIA

[sidisaidredouane@live.com](mailto:sidisaidredouane@live.com) - SIDI SAID REDOUANE

License
-------

**All Rights Reserved**
