# projet_docker_compose_copie_host
docker compose avec copie de fichier conteneur vers machine hôte

Ici les conteneurs vont communiquer avec la machine hôte qui va creer un repertoire pivôt entre les differents conteneurs pour leurs echanges de données

le conteneur "mysql" va copier le repertoire du site générer dans la machine grace à la comande "cp" puis le conteneur contenant tomcat va copier ce même repertoire et poursuivre l'execution
