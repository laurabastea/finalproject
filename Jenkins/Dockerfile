FROM ubuntu
RUN apt update && apt upgrade -y
RUN apt-get install openjdk-11-jdk wget -y

WORKDIR /opt/jenkins/
EXPOSE 8080
RUN wget https://updates.jenkins-ci.org/download/war/2.308/jenkins.war -O /opt/jenkins/jenkins.war
RUN wget https://github.com/jenkinsci/plugin-installation-manager-tool/releases/download/2.10.0/jenkins-plugin-manager-2.10.0.jar -O /opt/jenkins/jenkins-plugin-manager.jar
ENV JENKINS_HOME=/opt/jenkins/
VOLUME $JENKINS_HOME

ARG user=jenkins
ARG group=jenkins
ARG uid=1000
ARG gid=1000
ARG http_port=8080
ARG REF=/usr/share/jenkins/ref
RUN chown ${uid}:${gid} $JENKINS_HOME && groupadd -g ${gid} ${group} && useradd -d "$JENKINS_HOME" -u ${uid} -g ${gid} -m -s /bin/bash ${user}

#COPY ./plugins.txt /opt/jenkins/plugins.txt
#RUN java -jar /opt/jenkins/jenkins-plugin-manager.jar --war /opt/jenkins/jenkins.war -f /opt/jenkins/plugins.txt

CMD java -Djenkins.install.runSetupWizard=false -jar /opt/jenkins/jenkins.war 

RUN wget https://get.jenkins.io/plugins/github/latest/github.hpi
