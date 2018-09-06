FROM jenkins
USER root
RUN rm -f /usr/share/jenkins/jenkins.war
COPY jenkins.war /usr/share/jenkins/
USER jenkins
USER root
RUN mkdir /var/log/jenkins
RUN chown -R jenkins:jenkins /var/log/jenkins
RUN mkdir -p /tmp/download &&  curl -L https://get.docker.com/builds/Linux/x86_64/docker-1.13.1.tgz | tar -xz -C /tmp/download &&  rm -rf /tmp/download/docker/dockerd &&  mv /tmp/download/docker/docker* /usr/local/bin/ &&  rm -rf /tmp/download &&  groupadd -g 999 docker 
USER jenkins


