# Gestion de Congés - README

## Description

Ce projet est une application Symfony permettant de gérer les congés des utilisateurs et d’analyser diverses statistiques associées. Elle comprend plusieurs fonctionnalités :

- **Affichage des congés** : Voir tous les congés des utilisateurs.
- **Page d'accueil** : Page de bienvenue pour l'application.
- **Statistiques** : Analyse des congés, avec des statistiques par type et par utilisateur, ainsi que des comparaisons de performances entre requêtes DQL et SQL.

## Fonctionnalités

- **CongeController** : Gère la récupération et l'affichage des congés pour les utilisateurs.
- **HomeController** : Affiche la page d'accueil de l'application.
- **StatsController** : Fournit des statistiques avancées sur les congés, incluant le nombre total de congés par type et par utilisateur, avec un système de mesure des temps d'exécution entre requêtes DQL et SQL.

---

## Structure du Projet

- **src/Controller** :
    - **CongeController.php** : Gère la page des congés et affiche les congés en utilisant `CongeRepository`.
    - **HomeController.php** : Gère la page d'accueil de l'application.
    - **StatsController.php** : Fournit des statistiques sur les congés :
        - Récupération du nombre total de congés.
        - Groupement des congés par type et par utilisateur.
        - Comparaison de la performance entre DQL et SQL pour l'extraction des statistiques.
        
- **src/Entity** : Contient les entités pour la base de données, telles que `Conge`.
- **src/Repository** : Contient les dépôts de données, comme `CongeRepository` et `UserRepository`, pour interagir avec la base de données.
- **public/** : Dossier public contenant le point d'entrée de l'application (`index.php`).
- **config/** : Contient les fichiers de configuration de l'application Symfony.
- **templates/** : Contient les fichiers Twig pour le rendu des vues.

---

## Prérequis

- **PHP** : Version 8.1 ou plus récente.
- **Composer** : Gestionnaire de dépendances PHP.
- **Node.js & npm** : Pour gérer les assets front-end (si vous utilisez Webpack Encore pour gérer le JavaScript et CSS).
- **Base de données** : Un serveur de base de données SQL (comme MySQL ou PostgreSQL) pour stocker les informations des utilisateurs et des congés.

---

## Installation

Il existe deux manières d'installer le projet :

### 1. Cloner le dépôt depuis GitHub

1. Clonez le dépôt :
    ```bash
    git clone https://github.com/username/gestion_conges_stats.git
    cd gestion_conges_stats
    ```

2. Installez les dépendances avec Composer :
    ```bash
    composer install
    ```

3. Installez les dépendances front-end :
    ```bash
    npm install
    npm run build
    ```

### 2. Télécharger le projet depuis GitHub

Vous pouvez également télécharger le projet en ZIP depuis le dépôt GitHub :  
[https://github.com/ggaillard/gestion_conges_stats](https://github.com/ggaillard/gestion_conges_stats)

---

## Lancer le Serveur de Développement

Pour lancer le serveur de développement, entrez la commande suivante :

```bash
symfony serve -d
``` 

Si le projet ne démarre pas, cela peut provenir des mises à jour de Composer. Dans ce cas, mettez à jour Composer avec la commande suivante :

```bash
composer update
```

-----------
# Gestion des Versions avec Git
## 1. Ajouter des fichiers au suivi Git
Après avoir effectué des modifications dans votre projet, vous devez ajouter les fichiers modifiés à l'index Git avec la commande :

```bash
git add .
```

Cette commande ajoute tous les fichiers modifiés. Si vous souhaitez ajouter un fichier spécifique, remplacez le . par le chemin du fichier.

## 2. Committer vos modifications

Ensuite, vous pouvez committer vos changements avec un message décrivant les modifications effectuées :

```bash
git commit -m "Description de vos modifications"
```

## 3. Pousser les changements sur le dépôt distant

Enfin, poussez vos changements sur le dépôt GitHub. Si c'est la première fois que vous poussez, utilisez :

```bash
git push origin main
```

Si vous avez déjà une branche et que vous souhaitez pousser vos modifications, remplacez main par le nom de votre branche.

