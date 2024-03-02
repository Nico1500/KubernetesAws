# Kubernetes_Aws
![image](https://github.com/Nico1500/Kubernetes_Aws/assets/63806892/e023dcb9-fe1e-4c23-b920-8db9fc893bbc)

Ce projet illustre le déploiement d'une application microservice sur AWS en utilisant Kubernetes. L'application se compose de trois principaux composants :

- **API Utilisateur** : Gère les utilisateurs.
- **API Tâches** : Gère les tâches.
- **API Authentification** : Gère l'authentification des utilisateurs. Pour des raisons de sécurité, cette API est uniquement accessible via une Cluster IP.

L'API Utilisateur est configurée pour utiliser un volume persistant AWS EFS CSI, pour la persistance des données des utilisateurs.

## Configuration sur AWS

Pour déployer ce projet sur AWS, j'ai pu tester ces étapes :

### 1. Configuration du Cluster avec VPC

- **Configuration du Cluster Public et Privé** : Configurer un VPC (Virtual Private Cloud) pour isoler votre cluster Kubernetes dans un réseau virtuel sur AWS.

- **Création des Rôles de VPC** : Faire les rôles IAM nécessaires pour permettre au cluster EKS d'interagir avec les autres services AWS.

### 2. Connexion à AWS

- **Connexion via un Terminal** : Utiliser l'AWS CLI pour connecter mon compte AWS. Configurer la région AWS pour déployer le cluster EKS.

  ```
  aws configure
  ```

### 3. Configuration des Nodes

- **Configurer une Node** : Spécifier les spécifications des machines (les instances EC2) et les options de scaling pour mes nodes.

- **Création des Rôles des Nodes** : Créer les rôles IAM nécessaires pour les nodes, pour les accès aux ressources AWS.

### 4. Déploiement Kubernetes

- **Application des Fichiers YAML** : Appliquer mes fichiers YAML Kubernetes pour configurer les nodes, déployer mes pods, et créer mes services. AWS a configuré automatiquement les Load Balancers pour exposer mes services avec des URLs accessibles.

  ```
  kubectl apply -f <fichier>.yaml
  ```

### 5. Configuration de Sécurité des fichiers

- **Création d'un Groupe de Sécurité** : j'ai fait un groupe de sécurité avec une règle NFS sur le VPC pour sécuriser l'accès au cluster et permettre au VPC de gérer le systeme de fichier réseau.

### 6. Volumes Persistants avec AWS CSI

- **Ajout des Volumes Persistants AWS CSI** : j'ai utilisé le CSI d'AWS pour créer des volumes persistants qui fonctionnent de manière similaire aux volumes Docker, mais avec une intégration native à AWS.

### 7. Système de Fichiers sur le VPC

- **Création d'un Système de Fichiers** : Créer un système de fichiers EFS sur le VPC pour qu'il soit accessible par les pods dans le même réseau.
- **Liaison du Système de Fichiers aux Régions** : Verifier que le système de fichiers est disponible dans les deux zones de disponibilité pour que les API utilise les mêmes données.
