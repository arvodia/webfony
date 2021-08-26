## Table of contents
  - [User role](#user-role)
      - [Subscriber:](#subscriber)
      - [Contributor:](#contributor)
      - [Author :](#author)
      - [Moderator:](#moderator)
      - [Translator:](#translator)
      - [Publisher:](#publisher)
      - [Manager :](#manager)
      - [Administrator :](#administrator)
      - [Director :](#director)
      - [Multi Roles:](#multi-roles)
  - [Application role](#application-role)
      - [Editors](#editors)
      - [Multimedia](#multimedia)

## User role
A role defines the permissions for users to perform a group of tasks. These roles are defined on the edit user account page.
Users who are not authenticated have the Anonymous User role. Users who are authenticated have the Subscriber role.

> `Note`: By assigning a role to a user it will allow him to access the back-office part of the website.

### Subscriber:
 - he can write comments, if in the article comments are open
 - he can modify his account

### Contributor:
 - he can see his own articles, with their private comments
 - he can write, modify and delete an article and that translation
 - he cannot publish an article or its translation
 - he cannot modify if the status is publish or refuse
 - it cannot delete if the status is published

### Author :
 - he can see his own articles, with their private comments
 - he can write, publish, modify and delete his own articles and their translations
 - he cannot modify if the status refuse

### Moderator:
 - he can see all the articles, translations and comments
 - he can edit and publish all articles and translations
 - he can set the status to refuse for the articles
 - he can no longer modify if the status is refused
 - approval or rejection of comments
 - he cannot write his own articles (you have to add the Author or Contributor role)

### Translator:
 - he can see all articles, sections and translations
 - he can write, publish, modify and delete only translations of articles or sections
 - he cannot modify if the status is refused
 - he cannot write his own articles (the Author or Contributor role must be added)

### Publisher:
 - he has all the permissions for all articles, translations and comments
 - so see, write, publish, modify and delete

### Manager :
 - he inherits the Editor role permissions
 - he has all the authorizations for all sections and users
 - so he can also assign roles to users
 - he cannot modify an administrator account
 - he can add headings

### Administrator :
 - he inherits the Manager role authorization
 - he can access the dashboard page
     - consumes a task
     - restart a task after its failure
 - he can access the system configuration
     - site settings
         - site details
         - contact
         - email details
         - time zones
         - configure URLs
     - select the languages ​​of the site
     - setting the appearance of the site
 - he can access the automatic model generation page of:
     - privacy policy
     - Terms of use
 - he can access the editors and media configuration page
 - he can configure user account settings

### Director :
 - he inherits the Administrator role permissions
 - he can access the Development page
     - configure caching
     - put the site in maintenance mode
     - he can download a site backup
 - he can access the Translation page of the user interface
     - he can write, modify translations
 - he can access the session (People online)
 - Delete a Task in (Dashboard)

### Multi Roles:
If you add the Contributor role, has a user with the role (Author, Moderator or Translator) he will inherit the Contributor role permissions, so he can publish content, but then he cannot modify it.

## Application role
An application role defines permissions for system modules.

### Editors
 - add user roles to text editors to allow use of each editor.

### Multimedia
 - add user roles to the multimedia module to allow access to the multimedia part.