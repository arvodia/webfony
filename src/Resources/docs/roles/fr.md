## Table des matières
  - [Rôle utilisateur](#role-utilisateur)
      - [Abonné :](#abonne)
      - [Contributeur :](#contributeur)
      - [Auteur :](#auteur)
      - [Modérateur :](#moderateur)
      - [Traducteur :](#traducteur)
      - [Éditeur :](#editeur)
      - [Gérant :](#gerant)
      - [Administrateur :](#administrateur)
      - [Directeur :](#directeur)
      - [Multi Rôles :](#multi-roles)
  - [Rôle application](#role-application)
      - [Éditeurs](#editeurs)
      - [Multimédia](#multimedia)

## Rôle utilisateur
Un rôle définit les autorisations pour les utilisateurs d’effectuer un groupe de tâches. Ces rôles sont définis sur la page de modification de compte utilisateur. 
Les utilisateurs qui ne sont pas authentifiés ont le rôle Utilisateur anonyme. Les utilisateurs qui sont authentifiés ont le rôle Abonné.

>`Remarque` : En attribuant un rôle à un utilisateur il va lui permettre d’accéder a la partie back-office de site web.

### Abonné :
 - il peut écrire des commentaires, si dans l'article les commentaires sont ouvert
 - il peut modifier sont compte

### Contributeur :
 - il peut voir ces propre articles, avec leurs commentaires privé
 - il peut écrire, modifier et supprimer un article et ça traduction
 - il ne peut pas publier un article ou sa traduction
 - il ne peut pas modifier si le statut est publier ou refuser
 - il ne peut pas supprimer si le statut est publie

### Auteur :
 - il peut voir ces propre articles, avec leurs commentaires privé
 - il peut écrire, publier, modifier et supprimer ces propre articles et leurs traductions
 - il ne peut pas modifier si le statut refuser

### Modérateur :
 - il peut voir tous les articles, traductions et commentaires
 - il peut modifier et publier tous les articles et traductions
 - il peut mettre le statut refuser pour les article
 - il ne peut plus modifier si le statut est refuser
 - l'approbation ou le refus des commentaires
 - il ne peut pas écrire ces propre articles (il faut lui ajouter le rôle Auteur ou Contributeur)

### Traducteur :
 - il peut voir tous les articles, rubriques et traductions
 - il peut écrire, publier, modifier et supprimer uniquement les traductions des articles ou rubriques
 - il ne peut pas modifier si le statut est refuser
 - il ne peut écrire ces propre articles (il faut lui ajouter le rôle Auteur ou Contributeur)

### Éditeur :
 - il a tous les autorisation pour tous les articles, traductions et commentaire
 - donc voir, écrire, publier, modifier et supprimer

### Gérant :
 - il hériter des autorisation de rôle Éditeur
 - il a tous les autorisation pour tous rubriques et utilisateurs   
 - donc il peut aussi attribues des rôles a des utilisateurs
 - il ne peut pas modifier un compte administrateur
 - il peut ajouter des rubriques

### Administrateur :
 - il hériter des autorisation de rôle Gérant
 - il peut accéder a la page tableau de bord
     - consume une tâche
     - redémarrer une tâche après son échec 
 - il peut accéder a la configuration du système
     - paramétrage du site
         - détails du site
         - contact
         - détails de l'e-mail 
         - fuseaux horaires
         - configurer les URLs
     - sélectionner les langues du site
     - paramétrage l'apparence du site
 - il peut accéder a la page de génération de modèles automatique de :
     - politique de confidentialité
     - conditions d'utilisation
 - il peut accéder a la page de configuration des éditeurs et média
 - il peut configurer les paramètres des comptes des utilisateurs

### Directeur : 
 - il hériter des autorisation de rôle Administrateur
 - il peut accéder a la page Développement
     - configurer la mise en cache
     - mettre le site en mode maintenance
     - il peut télécharger une sauvegarder de site
 - il peut accéder a la page Traduction de l'interface utilisateur
     - il peut écrire, modifier les traductions
 - il peut accéder a la session (Personnes en ligne)
 - Supprimer une Tâche dans (Tableau de bord)

### Multi Rôles :
Si vous ajouter le rôle Contributeur, a un utilisateur avec le rôle (Auteur, Modérateur ou Traducteur) il va hérite des autorisation de rôle Contributeur, donc il peut publier un contenu, mais après il ne peut pas le modifie.

## Rôle application
Un rôle application définit les autorisations pour les modules de système.

### Éditeurs
 - ajouter des rôles d'utilisateur à des éditeurs de texte pour permettre d'utiliser chaque éditeur. 

### Multimédia
 - ajouter des rôles d'utilisateur au module multimédia pour permettre d'accéder à la partie multimédia.