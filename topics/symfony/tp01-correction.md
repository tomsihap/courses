# Cours Symfony 4.4 LTS : TP - CORRECTIONS
L'énoncé peut être retrouvé ici : [Énoncé](tp01.md)

- [Cours Symfony 4.4 LTS : TP - CORRECTIONS](#cours-symfony-44-lts--tp---corrections)
  - [Exercice 1 - Créez le MLD](#exercice-1---cr%c3%a9ez-le-mld)
  - [Exercice 2 - Créez le projet Symfony](#exercice-2---cr%c3%a9ez-le-projet-symfony)
  - [Exercice 3 - Faites la liste des routes utiles au projet et leurs rôles](#exercice-3---faites-la-liste-des-routes-utiles-au-projet-et-leurs-r%c3%b4les)
  - [Exercice 4 - Configurer le projet](#exercice-4---configurer-le-projet)
    - [Ajouter le projet à Git](#ajouter-le-projet-%c3%a0-git)
      - [SANS Github Desktop](#sans-github-desktop)
        - [CRÉER LE PROJET ET FAIRE DES COMMIT/PUSH](#cr%c3%89er-le-projet-et-faire-des-commitpush)
          - [a. Créer le projet sur Github](#a-cr%c3%a9er-le-projet-sur-github)
          - [b. Ajouter notre projet au repository Github](#b-ajouter-notre-projet-au-repository-github)
          - [c. Faire un commit pour initialiser le projet](#c-faire-un-commit-pour-initialiser-le-projet)
          - [d. Faire des commits/push pendant le projet](#d-faire-des-commitspush-pendant-le-projet)
        - [RÉCUPÉRER LE PROJET](#r%c3%89cup%c3%89rer-le-projet)
      - [AVEC Github Desktop](#avec-github-desktop)
        - [CRÉER LE PROJET ET FAIRE DES COMMIT/PUSH](#cr%c3%89er-le-projet-et-faire-des-commitpush-1)
        - [RÉCUPÉRER LE PROJET](#r%c3%89cup%c3%89rer-le-projet-1)
  - [Exercice 5 - Configurez le projet Symfony](#exercice-5---configurez-le-projet-symfony)
  - [Exercice 6 - Créer les modèles](#exercice-6---cr%c3%a9er-les-mod%c3%a8les)
    - [Création des Model](#cr%c3%a9ation-des-model)
      - [1. Restaurant: `bin/console make:entity Restaurant`](#1-restaurant-binconsole-makeentity-restaurant)
      - [2. City: `bin/console make:entity City`](#2-city-binconsole-makeentity-city)
      - [3. RestaurantPicture: `bin/console make:entity RestaurantPicture`](#3-restaurantpicture-binconsole-makeentity-restaurantpicture)
      - [4. Review: `bin/console make:entity Review`](#4-review-binconsole-makeentity-review)
    - [Création des relations](#cr%c3%a9ation-des-relations)
      - [Description des questions d'une relation](#description-des-questions-dune-relation)
      - [Exemple](#exemple)
  - [Exercice 7 - Faire les migrations](#exercice-7---faire-les-migrations)
  - [Exercice 8 - Créer les controllers et les routes](#exercice-8---cr%c3%a9er-les-controllers-et-les-routes)
    - [Exemple: RestaurantController](#exemple-restaurantcontroller)
  - [Exercice 9 - Faire la page d'accueil](#exercice-9---faire-la-page-daccueil)
  - [Exercice 10 - Améliorer la requête et ne retourner que les 10 meilleurs](#exercice-10---am%c3%a9liorer-la-requ%c3%aate-et-ne-retourner-que-les-10-meilleurs)

## Exercice 1 - Créez le MLD
D'après le brief du client, voici le MLD qui a été décidé :

```
USER
------
id
email
password
roles
firstname
lastname
city_id             # Dans quelle ville est l'utilisateur ?

RESTAURANT
------
id
name
description
created_at
city_id             # Quelle est la ville du restaurant ?
user_id             # Qui a créé le restaurant ?

CITY
-----
id
name
zipcode

RESTAURANT_PICTURE
-----
id
restaurant_id       # De quel restaurant est-ce la photo ?
name
file

REVIEW
-----
id
message
rating
created_at
user_id             # Qui a posé l'avis
restaurant_id       # Sur quel restaurant a t-il été posé
review_id           # Si c'est une réponse à un avis: quel est l'avis parent ?
```

Les relations sont :

Table A | Relation | Table B
---------|----------|---------
USER | `N:1` | CITY
RESTAURANT | `N:1` | CITY
RESTAURANT_PICTURE | `N:1` | RESTAURANT
REVIEW | `N:1` | USER
REVIEW | `N:1` | RESTAURANT
REVIEW | `N:1` | REVIEW

## Exercice 2 - Créez le projet Symfony
```
composer create-project symfony/website-skeleton:^4.4 notaresto
```

## Exercice 3 - Faites la liste des routes utiles au projet et leurs rôles
```
GET     /login                  anonyme
POST    /login                  anonyme
GET     /logout                 all
GET     /register               anonyme

GET     /restaurants            all
GET     /restaurant/new         restaurateur
POST    /restaurant             restaurateur
GET     /restaurant/{id}        all
DELETE  /restaurant/{id}        restaurateur, modérateur
GET     /restaurant/{id}/edit   restaurateur, modérateur
POST    /restaurant/{id}/edit   restaurateur, modérateur

GET     /reviews            modérateur
POST    /review             client, restaurateur
GET     /review/{id}        all
DELETE  /review/{id}        connecté
GET     /review/{id}/edit   connecté
POST    /review/{id}/edit   connecté

GET     /users            modérateur
GET     /user/{id}        connecté
DELETE  /user/{id}        connecté
GET     /user/{id}/edit   connecté
POST    /user/{id}/edit   connecté

GET     /cities           modérateur
POST    /city             modérateur
GET     /city/{id}        anonyme
DELETE  /city/{id}        modérateur
GET     /city/{id}/edit   modérateur
POST    /city/{id}/edit   modérateur


GET     /restaurant_pictures            modérateur
POST    /restaurant_picture             restaurateur, modérateur
GET     /restaurant_picture/{id}        all
DELETE  /restaurant_picture/{id}        restaurateur, modérateur
GET     /restaurant_picture/{id}/edit   restaurateur, modérateur
POST    /restaurant_picture/{id}/edit   restaurateur, modérateur
```

## Exercice 4 - Configurer le projet
### Ajouter le projet à Git

> Commits de correction :
> - https://github.com/tomsihap/notaresto/commit/d730addd1212c6070e777e4effe801db82e88e09

#### SANS Github Desktop
##### CRÉER LE PROJET ET FAIRE DES COMMIT/PUSH

###### a. Créer le projet sur Github
1. Créer le projet sur Github en allant sur [github.com > New](https://github.com/new)
2. Repository name: `notaresto`
3. Description : `Description de votre projet (par ex: TP Symfony lors de ma formation DWWM chez Human Booster)`
4. Public/Private: `selon la visiblité que vous voulez donner au repository`
5. Initialize this repository with a README: `Ne PAS cocher`
6. Add .gitignore: `None`
7. Add a license: `None`
8. Cliquer sur `Create repository`

###### b. Ajouter notre projet au repository Github
> **IMPORTANT** : Vérifiez que sous la ligne `Quick setup — if you’ve done this kind of thing before`, le bouton `HTTPS` soit bien cliqué.

1. Copiez la ligne `https://github.com/.../notaresto.git` qui apparaît dans le champ input de `Quick setup — if you’ve done this kind of thing before` (le bloc qui a un fond bleu).
2. Ouvrez un terminal situé dans votre projet et saisissez les lignes suivantes :

```
git init
git remote add origin https://github.com/..../notaresto.git
```

> Si vous avez fait une erreur dans l'URL lorsque vous avez tapé la commande (il aurait du s'agit de celle que vous avez copiée au point 1), vous pouvez la modifier en saisissant :
> 
> `git remote set-url origin https://github.com/..../notaresto.git`

###### c. Faire un commit pour initialiser le projet
```
git add -A
git commit -m "Init repository"
git push -u origin master
```

###### d. Faire des commits/push pendant le projet
```
git add -A
git commit -m "Nom du commit"
git push
```


##### RÉCUPÉRER LE PROJET
Si vous souhaitez ouvrir le projet par exemple sur un autre ordinateur :
1. Allez sur la page Github du projet à copier
2. Cliquez sur `Clone or download`
3. Vérifiez qu'il soit écrit `Clone  with HTTPS` (et non `Clone with SSH`, si ce n'est pas bon, cliquez sur `Use HTTPS`)
4. Copiez l'URL qui est dans le champ input
5. Ouvrez un terminal dans le dossier dans lequel le projet se situera (ATTENTION ! Le dossier qui ACCUEILLERA le projet ! C'est à dire par exemple `c:/users/tomsihap/projects`, pas besoin de créer à la main le sous dossier du futur projet !!)
6. Saisissez la ligne suivante :
```
git clone https://github.com/..../notaresto.git
```
7. Le dossier `notaresto` devrait maintenant être cloné ! Vous pouvez faire des commit/push dedans comme avant.

> **Note** : Attention, si vous clonez un projet de quelqu'un d'autre et que vous n'avez pas de droits en écriture, les push ne seront pas possibles. Dans le cas où vous voulez copier un projet existant et pouvoir faire vos propres commits dessus comme s'il s'agissait d'un nouveau projet, faites plutôt un fork : cela crééra le projet dans votre propre  Github.

#### AVEC Github Desktop
##### CRÉER LE PROJET ET FAIRE DES COMMIT/PUSH
1. Dans Github Desktop, cliquez sur `Add an Existing Repository from your Hard Drive...`
2. Cherchez le dossier du projet `notaresto`
3. Si le message `This directory does not appear to be a Git repository. Would you like to create a repositorry here instead?` apparaît, cliquez sur `create a repository` :
   - Name: `notaresto`
   - Description : `Description de votre projet (par ex: TP Symfony lors de ma formation DWWM chez Human Booster)`
   - Local Path : `Ne pas toucher, normalement il s'agit du chemin vers le dossier de projet`
   - Initialize this repository with a README: `Ne pas cocher`
   - Git Ignore: `None`
   - License: `None`
4. Cliquer sur `Create Repository`
5. Cliquer sur `Publish repository`
   - Name et Description: idem que en haut
   - Keep this code private: `À cocher uniquement si vous voulez le laisser en privé.`
   - Organization: `None`
   - Cliquer sur `Publish repository`
6. Pour faire des commits: après avoir fait des modifications dans le projet, remplissez le champ `Summary` puis cliquez sur `Commit to master`. Enfin, cliquez sur `Push` pour bien pusher les modifications.

##### RÉCUPÉRER LE PROJET
Si vous souhaitez ouvrir le projet par exemple sur un autre ordinateur :
1. Cliquez sur `Clone a Repository`
2. Cherchez le repository `pseudo/notaresto` (par exemple: `tomsihap/notaresto`)
3. Dans Local Path : remplissez le chemin vers lequel le projet sera cloné
4. Le projet est maintenant cloné ! Vous pouvez faire des commit/push comme avant.

> **Note** : Attention, si vous clonez un projet de quelqu'un d'autre et que vous n'avez pas de droits en écriture, les push ne seront pas possibles. Dans le cas où vous voulez copier un projet existant et pouvoir faire vos propres commits dessus comme s'il s'agissait d'un nouveau projet, faites plutôt un fork : cela crééra le projet dans votre propre  Github.

## Exercice 5 - Configurez le projet Symfony
1. Copiez-collez le fichier `.env` existant, pour créer le fichier `.env.local`
    > En ligne de commande c'est plus rapide : `cp .env .env.local`

2. Modifiez **uniquement** le fichier `.env.local` !
3. Adaptez la ligne `DATABASE_URL` selon votre configuration. La base de données n'a pas besoin d'exister encore pour être renseignée ici ! Par exemple :
   > `DATABASE_URL=mysql://root:root@127.0.0.1:8889/notaresto?serverVersion=5.7`
   >
    > `DATABASE_URL=mysql://root:@127.0.0.1:3306/notaresto?serverVersion=5.7`

À quoi sert le `.env.local` ? En fait, le `.env` est commité par défaut, tandis que `.env.local` est dans le `.gitignore` par défaut de Symfony. Deux avantages à l'utiliser :
1. C'est une énorme faute de sécurité que de  mettre des mots de passe dans un fichier qui se retrouve commité !
2. Si vous travaillez sur votre projet depuis plusieurs endroits (au travail, chez vous, version en serveur de prod, de préprod, de dev...), chaque version pourra avoir sa propre configuration `.env.local` sans qu'il n'y ait de conflits.

Mais à quoi sert alors d'avoir un `.env` ? Imaginez que vous rajoutiez des lignes de configuration dans votre `.env.local` (nom du site, clé d'API de Google Maps...). Si vous le laissez dans `.env.local`, ces clés n'apparaîtront pas dans Github. Si vous les ajoutez dans le `.env` avec des valeus par défaut (par exemple: `GOOGLE_API_KEY=**remplir_la_clé_d_api**)`, vous aurez un exemple de fichier `.env` à remplir dans un `.env.local` pour déployer votre projet.

> **Note en cas de CLONE du projet** : Si vous avez cloné le projet, n'oubliez pas de faire un `composer install` ! En effet, le dossier `vendor` n'est pas commité, il faut installer les dépendances après avoir cloné le projet.

## Exercice 6 - Créer les modèles

> Commits de correction : (Entités)
> - https://github.com/tomsihap/notaresto/commit/a354595066db9ddf820d729bf054a1355afa6bae
> - https://github.com/tomsihap/notaresto/commit/d4396566bfb243210c635e31787d737a73e315f9
> - https://github.com/tomsihap/notaresto/commit/46b6ee9bedc8ede61d307ddb61b7455cf5a0e4ee
> - https://github.com/tomsihap/notaresto/commit/b4076228388ae0004d2bbce9816d57894bb6f633
> - https://github.com/tomsihap/notaresto/commit/2905f4184edbd8aafad9d4d42e6b4cb7074b46c7

> Commits de correction : (Relations)
> - https://github.com/tomsihap/notaresto/commit/e7d73e087bbc3fb5fb7856f36da4ab43694d8c73
> - https://github.com/tomsihap/notaresto/commit/2a2a594ed30302f409845bba41845eeae16bbcdc
> - https://github.com/tomsihap/notaresto/commit/6e8e19fb55d6578ffbcc4174cf4bc45f7ca2beb8
> -  https://github.com/tomsihap/notaresto/commit/a07e4a4554e4347c98d52890ba45d3199c55df12

### Création des Model
Si vous suivez le MLD de la correction, les modèles à créer sont :
- User
- Restaurant
- City
- RestaurantPicture
- Review

> **IMPORTANT** : N'OUBLIEZ PAS de faire un commit après CHAQUE `make:entity` !!!! De cette façon, si vous faites une erreur à l'entité suivante, vous pouvez supprimer les fichiers créés de deux façons :
> 1. Console :
```
git checkout -- .
git clean -i
# Répondre "1" à la question
```
> 2. Github Desktop : clic droit sur `2 changed files` et `Discard changes`.

#### 1. Restaurant: `bin/console make:entity Restaurant`

Property name | Field type | Field length | Nullable ? |
---------|----------|---------|---------|
`name` | `string` | `255` | `no` |
`description` | `text` | | `yes` |
`createdAt` | `datetime` | | `no` |

#### 2. City: `bin/console make:entity City`

Property name | Field type | Field length | Nullable ? |
---------|----------|---------|---------|
`name` | `string` | `255` | `no` |
`zipcode` | `string` | `15` | `no` |

#### 3. RestaurantPicture: `bin/console make:entity RestaurantPicture`

Property name | Field type | Field length | Nullable ? |
---------|----------|---------|---------|

> On ne remplit rien pour le moment ! On créée l'entité vide. On la remplira lorsqu'on gèrera l'upload d'images.

#### 4. Review: `bin/console make:entity Review`

Property name | Field type | Field length | Nullable ? |
---------|----------|---------|---------|
`message` | `text` | | `yes` |
`rating` | `integer` | | `no` |
`createdAt` | `datetime` | | `no` |

### Création des relations
Les relations à créer seront :

> ATTENTION ! Prenez garde à la relation Review avec elle même (commentaire "parent")

Entité à modifier| Nouveau champ | Relation | Classe | Nullable ? | Accessors ? | Nouveau champ | Orphan Removal ?
---------|----------|---------|---------|---------|---------|---------|---------
Restaurant | `city` | `ManyToOne` | City | no | yes | `restaurants` | yes
RestaurantPicture | `restaurant` | `ManyToOne` | Restaurant | no | yes | `restaurantPictures` | yes
Review | `restaurant` | `ManyToOne` | Restaurant | no | yes | `reviews` | yes
Review | `parent` | `ManyToOne` | Review | yes | yes | `childs` |

#### Description des questions d'une relation
- `New property name (press <return> to stop adding fields):`
  - C'est le nouvel attribut que l'on ajoute à notre classe `Restaurant`. Ici : on ajoute un attribut `city` (**EN CAMEL CASE !!!! C'EST UN ATTRIBUT !**)

- `Field type (enter ? to see all types) [string]:`
  - Quel est le type de champ. On peut dire `relation` pour que l'interface nous propose les relations possibles, ou directement la relation voulue (`ManyToOne`, `OneToMany`...)

- `What class should this entity be related to?:`
  - C'est une relation : on veut rattacher notre entité à une autre entité, c'est à dire à une autre classe. On saisit le nom de la classe ici : `City` (**EN PASCAL CASE !!!! C'EST UNE CLASSE !**)

- `What type of relationship is this? Relation type? [ManyToOne, OneToMany, ManyToMany, OneToOne]:`
  - Comme on a indiqué `relation` à la question du FieldType tout à l'heure, l'interface nous propose une liste de relations possibles. Un restaurant n'a qu'une ville mais une ville a plusieurs restaurants : ce sera `ManyToOne`. Bien sûr, si nous avions modifié City au lieu de Restaurant, la relation aurait été inverse: `OneToMany`.

- `Is the Restaurant.city property allowed to be null (nullable)? (yes/no) [yes]:`
  - Peut-on rendre nullable le champ `city` ? Non, on répond `no`.

- `Do you want to add a new property to City so that you can access/update Restaurant objects from it - e.g. $city->getRestaurants()? (yes/no) [yes]:`
  - Comme ajoute un champ `city` à Restaurant, Symfony nous propose d'ajouter automatiquement un accesseur (un getter) dans l'entité City pour avoir tous les restaurants d'une ville ! Gardez la valeur par défaut, `yes`. 

- `New field name inside City [restaurants]:`
  - Comme un a un nouveau champ dans City (les restaurants), Symfony nous propose de l'ajouter. Gardez la valeur par défaut (déjà au pluriel !), `restaurants`.

- `Do you want to automatically delete orphaned App\Entity\Restaurant objects (orphanRemoval)? (yes/no) [no]:`
  - Le Orphan Removal n'existe que quand le champ n'est pas nullable: c'est la suppression des éléments orphelins en base de données, c'est à dire quand l'élément de la table parente n'existe plus. Par exemple, si je supprime une ville qui a des restaurants. Dans ce cas, dois-je supprimer les restaurants de la base de données ? Répondons ici `yes`.


#### Exemple

```bash
$ bin/console make:entity Restaurant

Your entity already exists! So lets add some new fields!

New property name (press <return> to stop adding fields):
> city

Field type (enter ? to see all types) [string]:
> relation

What class should this entity be related to?:
> City

What type of relationship is this?
------------ ----------------------------------------------------------------------
 Type         Description
------------ ----------------------------------------------------------------------
 ManyToOne    Each Restaurant relates to (has) one City.
              Each City can relate to (can have) many Restaurant objects

 OneToMany    Each Restaurant can relate to (can have) many City objects.
              Each City relates to (has) one Restaurant

 ManyToMany   Each Restaurant can relate to (can have) many City objects.
              Each City can also relate to (can also have) many Restaurant objects

 OneToOne     Each Restaurant relates to (has) exactly one City.
              Each City also relates to (has) exactly one Restaurant.
------------ ----------------------------------------------------------------------

Relation type? [ManyToOne, OneToMany, ManyToMany, OneToOne]:
> ManyToOne

Is the Restaurant.city property allowed to be null (nullable)? (yes/no) [yes]:
> no

Do you want to add a new property to City so that you can access/update Restaurant objects from it - e.g. $city->getRestaurants()? (yes/no) [yes]:
> yes

A new property will also be added to the City class so that you can access the related Restaurant objects from it.

New field name inside City [restaurants]:
>

Do you want to activate orphanRemoval on your relationship?
A Restaurant is "orphaned" when it is removed from its related City.
e.g. $city->removeRestaurant($restaurant)

NOTE: If a Restaurant may *change* from one City to another, answer "no".

Do you want to automatically delete orphaned App\Entity\Restaurant objects (orphanRemoval)? (yes/no) [no]:
> yes

updated: src/Entity/Restaurant.php
updated: src/Entity/City.php

Add another property? Enter the property name (or press <return> to stop adding fields):
>

Success!

Next: When you're ready, create a migration with make:migration
```

## Exercice 7 - Faire les migrations

> **Attention** : La base de données définie dans .env.local ne doit pas être encore crée (faite à la main par exemple) !

```bash
# Création de la base de données
php bin/console doctrine:database:create

# Création des premières migrations
php bin/console make:migration

# Exécution des premières migrations
php bin/console doctrine:migrations:migrate

# Par la suite, lorsque vous ferez d'autres migrations :
# 1. On teste si des migrations existent en essayant de les exécuter
# 2. On créée les nouvelles migrations
# 3. On relance l'exécution de migrations
php bin/console doctrine:migrations:migrate
php bin/console make:migration
php bin/console doctrine:migrations:migrate
```

## Exercice 8 - Créer les controllers et les routes
> Commits de correction:
> - https://github.com/tomsihap/notaresto/commit/5d6ba5d591772f9f6d07cff37d23efaa35be9c17
> - https://github.com/tomsihap/notaresto/commit/5a91d58f43a21f24bdd06bbdd81eaae318fe8bd6
> - https://github.com/tomsihap/notaresto/commit/dcdaf405e56ae9edeef2611a73a658102f454ffb
> - https://github.com/tomsihap/notaresto/commit/bab7403c9e2516f01c26ee1ddfe64e70e4b9da01


La liste de routes à utiliser est celle de l'exercice 3. Créons les controllers :

```
bin/console make:controller RestaurantController
bin/console make:controller RestaurantPictureController
bin/console make:controller CityController
bin/console make:controller ReviewController
```

> **Note** : ces commandes crééent des vues par défaut dans `templates`. Vous n'aurez peut être pas besoin de toutes, n'oubliez pas de supprimer les dossiers à l'avenir si vous ne vous en servirez pas pour ne pas avoir de fichiers inutiles !

> **Astuce** : Pensez à taper dans la console `bin/console debug:router` quand vous créez des routes pour vérifier si Symfony les a bien pris en compte ! Si elles n'aparaissent pas, c'est qu'elles sont mal placées dans la liste ou mal déclarées ou que deux routes ont le même nom. S'il y a une erreur dans la console, c'est une faute de frappe sans doute dans l'annotation.

### Exemple: RestaurantController
```php
<?php

namespace App\Controller;

use App\Entity\Restaurant;
use Symfony\Bundle\FrameworkBundle\Controller\AbstractController;
use Symfony\Component\Routing\Annotation\Route;

class RestaurantController extends AbstractController
{
    /**
     * Affiche la liste des restaurants
     * @Route("/restaurants", name="restaurant_index", methods={"GET"})
     */
    public function index()
    {
        return $this->render('restaurant/index.html.twig', [
            'controller_name' => 'RestaurantController',
        ]);
    }

    /**
     * Affiche un restaurant
     * @Route("/restaurant/{restaurant}", name="restaurant_show", methods={"GET"})
     * @param Restaurant $restaurant
     */
    public function show(Restaurant $restaurant)
    {
    }

    /**
     * Affiche le formulaire de création de restaurant
     * @Route("/restaurant/new", name="restaurant_new", methods={"GET"})
     */
    public function new()
    {
    }

    /**
     * Traite la requête d'un formulaire de création de restaurant
     * @Route("/restaurant", name="restaurant_create", methods={"POST"})
     */
    public function create()
    {
    }

    /**
     * Affiche le formulaire d'édition d'un restaurant (GET)
     * Traite le formulaire d'édition d'un restaurant (POST)
     * @Route("/restaurant/{restaurant}/edit", name="restaurant_edit", methods={"GET", "POST"})
     * @param Restaurant $restaurant
     */
    public function edit(Restaurant $restaurant)
    {
    }

    /**
     * Supprime un restaurant
     * @Route("/restaurant/{restaurant}", name="restaurant_delete", methods={"DELETE"})
     * @param Restaurant $restaurant
     */
    public function delete(Restaurant $restaurant)
    {
    }
}
```


## Exercice 9 - Faire la page d'accueil
## Exercice 10 - Améliorer la requête et ne retourner que les 10 meilleurs
