# Image base
FROM ubuntu:16.10 

# mise à jour de l'image
RUN apt-get update && apt-get -y upgrade

# Install Oracle Java
RUN apt-get install -y --no-install-recommends openjdk-8-jdk

# Installation de  maven
RUN apt-get update  
RUN apt-get install -y maven

#création d'un repertoire de travail 
WORKDIR /code


#installation de mysql serveur et client en mode non interactive
RUN DEBIAN_FRONTEND=noninteractive apt-get install -y  mysql-server mysql-client

# Exposition du port 3306
EXPOSE 3306

# Installation de  ant pour l'initialisation de la base de donnée
RUN apt-get install ant

# Installation du supervisor
RUN apt-get -y install supervisor

# Copie du fichier de configuration  supervisor.conf pour le demarrage de mysql et tomcat en même temps
COPY supervisord.conf /etc/supervisor/conf.d/supervisord.conf

# ajout du fichier pom.xml 
ADD pom.xml /code/pom.xml

# declation des variable et initialisation
ARG GOAL_LUTECE=lutece:site-assembly
ARG BD=lutece_bd
ARG LUTECE=lutece_site

# Compilation du fichier pom.xml pour générer le site
RUN mvn $GOAL_LUTECE

#configuration du fichier db.properties pour la communication avec la BD 
RUN sed -i 's/lutece/$BD/' /code/target/plugin-dockerlutece-1.0.0-SNAPSHOT/WEB-INF/conf/db.properties
RUN sed -i 's/motdepasse//' /code/target/plugin-dockerlutece-1.0.0-SNAPSHOT/WEB-INF/conf/db.properties
RUN sed -i 's/localhost/127.0.0.1/' /code/target/plugin-dockerlutece-1.0.0-SNAPSHOT/WEB-INF/conf/db.properties
RUN sed -i 's/fr.paris.$BD.util.pool.service.LuteceConnectionService/fr.paris.lutece.util.pool.service.LuteceConnectionService/' /code/target/plugin-dockerlutece-1.0.0-SNAPSHOT/WEB-INF/conf/db.properties

# création de la BD avec ant
RUN /bin/bash -c "/usr/bin/mysqld_safe &" && \
  sleep 5 && \
  ant -file /code/target/plugin-dockerlutece-1.0.0-SNAPSHOT/WEB-INF/sql/build.xml -v

# copie du site généré dans le webapps de tomcat pour son lancement

#RUN  -volumes-from dockerdctmj_tomcat_1 && \ /bin/bash -c cp -r /code/target/plugin-dockerlutece-1.0.0-SNAPSHOT /codetomcat/var/lib/tomcat7/webapps/$LUTECE

RUN cp mysql:/code/target/plugin-dockerlutece-1.0.0-SNAPSHOT .

# Execution mysql service and Tomcat service par l'execution du supervisor
CMD ["/usr/bin/supervisord"]
