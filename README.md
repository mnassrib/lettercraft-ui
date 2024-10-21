# **LetterCraft - Générateur de Lettres Personnalisées**

LetterCraft est une application web permettant de générer des lettres personnalisées. Que ce soit pour des lettres de motivation, des réclamations, ou tout autre type de correspondance formelle, l'application offre une solution simple et rapide pour créer des lettres professionnelles. En remplissant un formulaire interactif, les utilisateurs peuvent obtenir une lettre en format PDF, prête à être téléchargée ou envoyée.

## **Fonctionnalités**

- **Génération de lettres personnalisées :** Créez des lettres adaptées à vos besoins (lettres de motivation, réclamations, etc.) en remplissant un formulaire avec vos informations personnelles et celles du destinataire.
- **Prévisualisation de la lettre :** Visualisez la lettre avant de la générer en PDF pour vous assurer qu'elle répond à vos attentes.
- **Accessibilité multiplateforme :** Compatible avec toutes les plateformes (desktop, mobile, tablette) pour une expérience utilisateur fluide.
- **Interface conviviale :** Interface simple et intuitive avec des champs clairement indiqués, facilitant la saisie des informations.
- **Export PDF :** Téléchargez la lettre générée au format PDF directement depuis l'application.

## **Lien vers l'application**

L'application est accessible en ligne à l'adresse suivante :  
[https://lettercraft.onrender.com/](https://lettercraft.onrender.com/)

## **Contexte et objectif**

Ce dépôt public a pour objectif de **configurer un pipeline CI/CD** en utilisant **GitHub Actions**, tout en gardant le code source de l'application dans un dépôt privé. Le pipeline CI/CD est déclenché automatiquement par un événement personnalisé lorsqu'une action spécifique est effectuée dans le dépôt privé, comme un commit.

### **Automatisation du pipeline CI/CD**

Le pipeline CI/CD comprend les tâches suivantes :
- **Tests et validation** du code source sans conteneurisation.
- **Création d'une image Docker** de l'application.
- **Lancement de tests dans un conteneur Docker** pour valider le bon fonctionnement de l'application dans un environnement simulant la production.
- **Application des migrations de base de données et déploiement automatique** de l'application sur [Render](https://render.com) après validation.

### **Configuration des secrets GitHub**

Pour exécuter correctement le pipeline CI/CD, les secrets suivants doivent être configurés dans les paramètres du dépôt GitHub :
- `SECRET_KEY`
- `RECAPTCHA_SITE_KEY`
- `RECAPTCHA_SECRET_KEY`
- `POSTGRES_USER`
- `POSTGRES_PASSWORD`
- `POSTGRES_DB_TEST`
- `PROD_DATABASE_URL`
- `PERSONAL_ACCESS_TOKEN` (pour cloner le dépôt privé)
- `RENDER_API_KEY`
- `RENDER_DEPLOY_HOOK`

Ces secrets sont utilisés pour protéger les informations sensibles liées à l'application et aux environnements de test et de production.

## **Aperçu du pipeline CI/CD**

Le pipeline CI/CD se compose de trois grandes phases :

### 1. **Tests sans conteneurisation**
- **Installation de PostgreSQL** sur la machine virtuelle d'exécution.
- **Création de l'utilisateur et de la base de données** PostgreSQL nécessaires pour les tests.
- **Clonage du dépôt privé** contenant le code source de l'application.
- **Installation des dépendances Python** et **exécution des migrations** pour préparer la base de données.
- **Exécution des tests unitaires** via `pytest` pour valider le bon fonctionnement du code dans un environnement local sans Docker.

### 2. **Tests avec conteneurisation (Docker)**
- **Construction d'une image Docker** de l'application à partir du `Dockerfile`.
- **Lancement d'un conteneur Docker** avec les configurations d'environnement et exécution des tests unitaires dans un environnement conteneurisé.
  
  Cela permet de s'assurer que l'application fonctionne correctement dans un environnement isolé et proche de celui de production.

### 3. **Déploiement sur Render**
- **Application des migrations** sur la base de données de production hébergée par Render pour garantir la cohérence des données.
- **Déploiement de l'application** sur la plateforme Render après validation des tests.
- **Notification de succès** après déploiement (optionnel).

## **Déclenchement du workflow**

Le workflow GitHub Actions est déclenché par un événement `repository_dispatch` provenant du dépôt privé, via un webhook configuré. Cet événement de type `deploy` permet de lancer automatiquement le pipeline CI/CD dans ce dépôt public sans avoir à exposer le code source privé.

## **Détails techniques du workflow CI/CD**

Le pipeline inclut les étapes suivantes :

1. **Installation de PostgreSQL** et préparation de l'environnement de base de données.
2. **Clonage du dépôt privé** contenant le code source de l'application avec un jeton d'accès sécurisé.
3. **Installation des dépendances** du projet via `pip` et exécution des migrations pour synchroniser la base de données.
4. **Exécution des tests unitaires** pour s'assurer que le code fonctionne comme prévu sans conteneurisation.
5. **Construction de l'image Docker** à partir du `Dockerfile` présent dans le dépôt.
6. **Lancement d'un conteneur Docker** pour tester l'application dans un environnement `testing`, en simulant un environnement de production.
7. **Application des migrations** sur la base de données de production hébergée sur Render.
8. **Déploiement de l'application** sur la plateforme Render après validation des tests et des migrations.

Ce pipeline CI/CD garantit que l'application LetterCraft est correctement testée et déployée en production, en utilisant une approche automatisée et sécurisée.