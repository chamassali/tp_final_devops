# projet_final_devops

#Installation de GitLab

## 1- Vérification des Versions :

$ docker -v
Docker version 20.10.7, build f0df350

$ docker-compose -v 
docker-compose version 1.29.1, build c34c88b2

## 2- Dans le fichier /etc/hosts, ajouter la ligne suivante pour pouvoir accéder à l'interface gitlab via gitlab.example.com

<IP_VM>    gitlab.example.com
Il faudra ensuite remplacer l'IP de votre machine

## 3- Créer un fichier "docker-compose.yml" pour l'installation de gitlab

## 4- Exécuter le fichier "docker-compose.yml"

docker-compose up -d

## 5- Le conteneur GitLab se démarre

Cette commande permet de voir ce qui se passe dans les logs
docker-compose logs -f

## 6- Récuperer le mdp pour accéder au gitlab
docker-compose exec web grep 'Password:' /etc/gitlab/initial_root_password

## 7- Accéder à l'interface via gitlab.example.com
Se connecter avec l'identifiant : root
Mot de passe : Celui que vous avez récuperer à l'étape précédente

## 6- Fin de l'Installation

# Utilisation de GitLab

## 1-  
