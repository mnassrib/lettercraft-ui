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

Ce dépôt public a pour objectif de **configurer un pipeline CI/CD** en utilisant **GitHub Actions**, tout en gardant le code source de l'application dans un dépôt privé. L'idée principale est de **déclencher automatiquement le workflow CI/CD dans ce dépôt public lorsque des commits sont effectués dans le dépôt privé**.

L'automatisation du pipeline CI/CD inclut les éléments suivants :
- **Tests et validation** du code source de l'application.
- **Création d'une image Docker** de l'application.
- **Lancement de tests dans un environnement conteneurisé** pour valider le bon fonctionnement de l'application.
- **Déploiement automatique** de l'application sur [Render](https://render.com).

Ce processus permet de séparer la logique de déploiement (dans ce dépôt public) et le code source (gardé dans un dépôt privé), garantissant une automatisation fiable et sécurisée du déploiement continu.

## **Configuration des secrets GitHub**

Pour exécuter le pipeline CI/CD, les secrets suivants doivent être configurés dans les paramètres du dépôt GitHub :
- `SECRET_KEY`
- `RECAPTCHA_SITE_KEY`
- `RECAPTCHA_SECRET_KEY`
- `RENDER_API_KEY`

Ces secrets sont utilisés pour sécuriser les clés sensibles et les informations de configuration.

## **Aperçu du pipeline CI/CD**

Le pipeline CI/CD fonctionne ainsi :

1. **Déclenchement** : Le pipeline CI/CD se déclenche automatiquement lorsqu'un commit est effectué dans le dépôt privé.
2. **Tests** : Le pipeline exécute des tests pour valider le bon fonctionnement du code. Les tests sont exécutés à l'intérieur d'un conteneur Docker en utilisant `pytest`.
3. **Création d'image Docker** : Si les tests réussissent, une image Docker de l'application est construite.
4. **Tests supplémentaires dans le conteneur** : L'image Docker est validée en lançant un conteneur, et des tests supplémentaires sont exécutés pour s'assurer que l'application fonctionne correctement en environnement `testing`.
5. **Déploiement** : Après validation de l'image, l'application est automatiquement déployée sur [Render](https://render.com).
6. **Notification** : Une notification est envoyée pour indiquer le succès ou l'échec du déploiement.

### **Déclenchement du workflow**

Le workflow GitHub Actions est déclenché par des événements provenant du dépôt privé via un webhook, ce qui permet de démarrer le pipeline CI/CD dans ce dépôt public sans exposer le code source privé.

### **Détails du workflow CI/CD**

Le workflow CI/CD inclut les étapes suivantes :

- **Clone du dépôt privé** : Le dépôt privé contenant le code source est cloné.
- **Installation des dépendances** : Les dépendances nécessaires à l'application sont installées.
- **Exécution des tests** : Les tests unitaires et d'intégration sont exécutés pour vérifier le bon fonctionnement de l'application.
- **Construction de l'image Docker** : Une image Docker est construite à partir du code source.
- **Tests supplémentaires** : Un conteneur Docker est lancé pour tester l'application dans un environnement conteneurisé en mode `testing`.
- **Déploiement sur Render** : L'application est déployée automatiquement sur la plateforme Render après validation de l'image Docker.
