# Image base
FROM ubuntu:16.10 

# mise à jour de l'image
RUN apt-get update && apt-get -y upgrade

RUN apt-get update
# Install Oracle Java
RUN apt-get install -y --no-install-recommends openjdk-8-jdk 

#création d'un repertoire de travail 
VOLUME /codetomcat

RUN apt-get -y install  tomcat7

# Installation de l'administration de Tomcat
RUN apt-get -y install tomcat7-admin

# Exposition du port 8080 pour l'exterieur(machine physique)
EXPOSE 8080
RUN apt-get install -y  mysql-client

# Execution Tomcat  avec le lancement du  fichier supervisor
CMD ["/usr/bin/supervisord"]
