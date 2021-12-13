# projet_final_devops

#Installation de GitLab

#1 Vérification des Versions :

$ docker -v
Docker version 20.10.7, build f0df350

$ docker-compose -v 
docker-compose version 1.29.1, build c34c88b2

#2 Vérifier la résolution du DNS

<IP_VM>    gitlab.example.com
Il faudra ensuite remplacer l'IP de votre machine

#3 Exécuter le fichier "docker-compose.yml"

docker-compose up -d

#4 Le conteneur GitLab se démarre

Cette commande permet de voir ce qui se passe dans les logs
docker-compose logs -f

#5 Fin de l'Installation

Après l'installation vous pouvez voir l'interface de connexion grace à l'url "http://gitlab.example.com"

#Utilisation de GitLab

#1 
