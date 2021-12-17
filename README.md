# projet_final_devops

# Installation de GitLab

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

## 6- Redirection vers gitlab.example.com
Dans le fichier /etc/hosts, ajouter la ligne suivante pour avoir accès à l'interface de gitlab sur l'adresse gitlab.example.com:
163.172.241.102 gitlab.example.com

## 7- Récuperer le mdp pour accéder au gitlab
docker-compose exec web grep 'Password:' /etc/gitlab/initial_root_password

## 8- Accéder à l'interface via gitlab.example.com
Se connecter avec l'identifiant : root
Mot de passe : Celui que vous avez récuperer à l'étape précédente

## 9- Fin de l'Installation

# Utilisation de GitLab

## 1-  


# JENKINS

Nous avons d'abord essayez d'installer Jenkins avec ces deux premiers tutos : <br /><br />

https://dev.to/andresfmoya/install-jenkins-using-docker-compose-4cab <br />
https://adamtheautomator.com/jenkins-docker/ <br />

Puis nous avons finalement décidé de suivre celui-ci :
https://www.jenkins.io/doc/book/installing/docker/ <br />

Le tp a été réalisé sur une machine virtuelle linux ubuntu.

#1 Télécharger Docker Linux sur ce lien
https://docs.docker.com/engine/install/

 sudo apt-get update

 sudo apt-get install \
    ca-certificates \
    curl \
    gnupg \
    lsb-release
    
 curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /usr/share/keyrings/docker-archive-keyring.gpg
 
echo \
"deb [arch=$(dpkg --print-architecture) signed-by=/usr/share/keyrings/docker-archive-keyring.gpg] https://download.docker.com/linux/ubuntu \
$(lsb_release -cs) stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null

sudo apt-get update

sudo apt-get install docker-ce docker-ce-cli containerd.io

#2 Ouvrir un Terminal bash

#3 Créer un réseau bridge dans docker avec la commande suivante : docker network create jenkins

#4 Pour pouvoir exécuter les commandes docker à l'intérieur des noyaux jenkins, il faut installer docker:dind.
Pour cela exécuter cette commande dans votre terminal :

docker run \
  --name jenkins-docker \
  --rm \
  --detach \
  --privileged \
  --network jenkins \
  --network-alias docker \
  --env DOCKER_TLS_CERTDIR=/certs \
  --volume jenkins-docker-certs:/certs/client \
  --volume jenkins-data:/var/jenkins_home \
  --publish 2376:2376 \
  docker:dind \
  --storage-driver overlay2
  
  #5 Creer une Dockerfile et y écrire ceci :
  
  FROM jenkins/jenkins:2.319.1-jdk11
USER root
RUN apt-get update && apt-get install -y lsb-release
RUN curl -fsSLo /usr/share/keyrings/docker-archive-keyring.asc \
  https://download.docker.com/linux/debian/gpg
RUN echo "deb [arch=$(dpkg --print-architecture) \
  signed-by=/usr/share/keyrings/docker-archive-keyring.asc] \
  https://download.docker.com/linux/debian \
  $(lsb_release -cs) stable" > /etc/apt/sources.list.d/docker.list
RUN apt-get update && apt-get install -y docker-ce-cli
USER jenkins
RUN jenkins-plugin-cli --plugins "blueocean:1.25.2 docker-workflow:1.26"

#6 Build une nouvelle image docker grace au Dockerfile avec la commande : docker build -t myjenkins-blueocean:1.1 .

#7 Lancer l'image avec la commande qui suit :

 docker run \
  --name jenkins-blueocean \
  --rm \
  --detach \
  --network jenkins \
  --env DOCKER_HOST=tcp://docker:2376 \
  --env DOCKER_CERT_PATH=/certs/client \
  --env DOCKER_TLS_VERIFY=1 \
  --publish 8080:8080 \
  --publish 50000:50000 \
  --volume jenkins-data:/var/jenkins_home \
  --volume jenkins-docker-certs:/certs/client:ro \
  myjenkins-blueocean:1.1
  
#8 Accéder à http://localhost:8080 sur un navigateur
Un mot de passe va être demandé, pour le récupérer, faire un docker ps dans une console, puis récupérer l'id de myjenkins.
Ensuite faire un sudo docker logs containerId. Le mot de pass apparaitra dans la console.
Rentrer le mot de passe dans l'interface web.

#9 Choisir ensuite l'option installer les pluggins suggérés

#10 Créer ensuite le 1er utilisateur administrateur avec les informations voulues

Voila, Jenkins et installé et configuré !
