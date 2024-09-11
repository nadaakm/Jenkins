# Utilisation de l'image officielle de Jenkins basée sur la dernière version LTS de Debian
FROM jenkins/jenkins:lts
USER root
RUN apt-get update -qq \
    && apt-get install -qqy apt-transport-https ca-certificates curl gnupg2 software-properties-common
RUN curl -fsSL https://download.docker.com/linux/debian/gpg | apt-key add -
RUN add-apt-repository \
   "deb [arch=amd64] https://download.docker.com/linux/debian \
   $(lsb_release -cs) \
   stable"
RUN apt-get update  -qq \
    && apt-get -y install docker-ce
RUN usermod -aG docker jenkins
# Déclare le volume Jenkins pour persister les données entre les redémarrages
VOLUME /var/jenkins_home

# Expose le port Jenkins
EXPOSE 8080

# Expose le port d'agent pour les connexions d'agent d'esclave
EXPOSE 50000