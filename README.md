# Utilisation de ce modèle
Ce modèle intègre une configuration PHP, APACHE, MARIA DB, PMA. Il installe directement les extensions nécessaires à tout type de projet php, et installe directement composer, node et npm dans le container principal. 

## Modèle du projet
### Le dossier /bin
Pour configurer les versions de mariadb dans un Dockerfile. Créer un dossier du nom de la version et ajouter un Dockerfile avec la version de mariadb.

### Le dossier /config
Le fichier du paramètrage de php, serveur apache, certificat ssl et de base de données à importer.
#### php 
Le Dockerfile qui génére tout le paramètrage de php (extensions, ajout des fichiers de serveur dans le container, ajout de l'utilisateur dans le containeur, installations de composer, node, npm ajout des fichiers ssl, etc...)  
Le php.ini pour modifier ce que vous voulez dans le container.
#### ssl
C'est ici que doit être intégré les fichiers de certificats ssl. J'utilise mkcert pour le developpement en local  [lien vers la doc](https://github.com/FiloSottile/mkcert).  
Pour Générer les fichier ssl nécessaires : 
```shell
cd config/ssl
mkcert NOM DE DOMAINE
```
#### vhosts
Paramètrage du fichier apache à modifier selon le type de site (site simple ou fullstack).  


## 1. Modifier le fichier .env
C'est ici que les variables sont paramètrées pour l'environnement de docker. Vous pouvez utiliser le modèle pour des sites simples ou fullstack.  
Les urls seront faites à partir de project name. 
```
#TODO A changer pour les différents projets
PROJECT_NAME=docker-lamp
BACKOFFICE_PROJECT_NAME=backoffice.docker-lamp
HOST_MACHINE_UNSECURE_HOST_PORT=8000
HOST_MACHINE_SECURE_HOST_PORT=4433
```
Vous pouvez paramètrer les ports, les mdp de base de données

## 2. Faire un build de l'image 
C'est préférable de builder l'image à utiliser pour pouvoir directement la placer dans le docker compose que l'on souhaite utiliser (compose.yml pour un site simple ou compose-multisite.yml pour un site fullstack).  
Lancer la commande pour le build
```
docker build . -f config/php/Dockerfile -t NOM_DE_L_IMAGE:TAG_DEL_IMAGE --build-arg USER_ID=$(id -u) --build-arg GROUP_ID=$(id -g)
```
Les --build-arg sont pour enregistrer l'utilisateur de la machine hôte dans le container pour éviter les problèmes de permissions des fichiers.

## 3. Faire le docker compose nécessaire au projet
```shell
docker compose -f compose-multisite.yml up -d
ou
docker compose up -d
```
## 4. Travailler dans le dossier src généré 