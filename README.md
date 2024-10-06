# BP-traefik-mariadb

Ce projet permet de déployer une instance MariaDB sécurisée en utilisant Docker Compose.

## Description

 Il inclut les fonctionnalités suivantes :

- Utilisation de MariaDB avec persistance des données.
- Configuration des mots de passe root et utilisateur via des fichiers de secrets.
- Traefik est utilisé comme proxy pour fournir une connexion sécurisée (HTTPS) avec des certificats TLS via Let's Encrypt.
- Connexion MariaDB à distance activée pour un utilisateur spécifique.

## Prérequis

Assurez-vous d'avoir les éléments suivants installés sur votre machine avant de commencer :

- Docker
- Docker Compose
- [Le projet BP-traefik-v3-ssl-ovh-api](https://github.com/lgrdev/BP-traefik-v3-ssl-ovh-api)

## Structure du projet

```
.
├── docker-compose.yaml
├── secrets/
│   ├── root_password.sample
│   ├── user_password.sample
├── mariadb/
│   ├── data/  # Répertoire de persistance des données
│   ├── conf/  # Répertoire pour les configurations personnalisées
└── .env.sample
```

### Fichiers de secrets

Les mots de passe pour l'utilisateur root et l'utilisateur de la base de données sont stockés dans des fichiers de secrets. Par défaut, des fichiers d'exemple sont fournis avec l'extension `.sample`.

- `root_password.sample` : Contient le mot de passe pour l'utilisateur root de MariaDB.
- `user_password.sample` : Contient le mot de passe pour l'utilisateur normal de MariaDB.

## Installation

### 1. Préparation
Exécutez la commande suivante pour créer les fichiers de secrets et le fichier `.env` :

```bash
bash setup.sh
```


### 2. Modifier les fichiers de secrets

a. Modifiez le fichier `secrets/root_password.secret` pour y saisir le mot de passe root
b. Modifiez le fichier `secrets/user_password.secret` pour y saisir le mot de passe de votre utilisateur

Pour des raisons de sécurité, veillez à saisir 2 mots de passe différents avec au moins 12 caracteres (12 étant un minimum) 


### 2. Configurer l'environnement

Modifiez le fichier `.env` à la racine du projet avec les variables nécessaires :

```env
MARIADB_VERSION=10.6
DB_DATABASE=my_database
DB_USER=my_user
```

Le choix de la version mariadb doit être effectué à partir des tags disponibles sur [Image officielle mariadb](https://hub.docker.com/_/mariadb/tags)

### 3. Lancer le service

Une fois les secrets créés et les variables d'environnement configurées, vous pouvez lancer le service MariaDB avec Docker Compose :

```bash
docker-compose up -d
```

### 4. Accéder à MariaDB

- Le service MariaDB sera disponible à distance via l'hôte spécifié dans la variable d'environnement `${DB_SERVER_HOST}`.
- Le port MariaDB (3306) est exposé pour permettre une connexion à distance.

## Configuration de Traefik

Le fichier `docker-compose.yaml` inclut des labels pour Traefik afin de sécuriser les connexions à MariaDB avec HTTPS. Traefik obtient automatiquement des certificats SSL/TLS via Let's Encrypt.

## Fichiers importants

- **`docker-compose.yaml`** : Définit le service MariaDB, sa persistance, les secrets et les labels Traefik.
- **`secrets/`** : Contient les fichiers de mots de passe secrets.
- **`mariadb/`** : Contient les répertoires de persistance des données et des configurations personnalisées.
- **`.env`** : Contient les variables d'environnement pour MariaDB et Traefik.

## Contributions

Les contributions sont les bienvenues ! Si vous trouvez des problèmes ou souhaitez ajouter des fonctionnalités, n'hésitez pas à ouvrir une issue ou une pull request.

## License

Ce projet est sous licence MIT. Voir le fichier [LICENSE](LICENSE) pour plus de détails.
