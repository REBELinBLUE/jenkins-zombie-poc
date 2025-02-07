FROM jenkins/jenkins:2.479.3-lts

USER root

RUN mkdir /var/log/jenkins

RUN chown -R root:root /var/log/jenkins \
    && chown -R root:root /usr/share/jenkins \
    && chown -R root:root /var/jenkins_home

RUN echo 2.0 > /usr/share/jenkins/ref/jenkins.install.UpgradeWizard.state

COPY plugins.txt /usr/share/jenkins/ref/

RUN jenkins-plugin-cli --plugin-file /usr/share/jenkins/ref/plugins.txt

USER jenkins
