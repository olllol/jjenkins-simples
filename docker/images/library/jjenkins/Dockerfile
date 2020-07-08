#------------------------------------------------------------------------------
# Set the base image for subsequent instructions:
#------------------------------------------------------------------------------
FROM jenkins/jenkins:2.237
#------------------------------------------------------------------------------
# Install prerequisites:
#------------------------------------------------------------------------------
USER root
ARG MVN_VER=3.6.3
RUN apt-get update -qq \
    && apt-get install -qqy apt-transport-https ca-certificates curl gnupg2 software-properties-common \
    && curl -fsSL https://download.docker.com/linux/debian/gpg | apt-key add - \
    && add-apt-repository "deb [arch=amd64] https://download.docker.com/linux/debian $(lsb_release -cs) stable" \
    && apt-get update  -qq \
    && apt-get install docker-ce -y \
    && usermod -aG docker jenkins \
    && wget https://downloads.apache.org/maven/maven-3/"${MVN_VER}"/binaries/apache-maven-"${MVN_VER}"-bin.tar.gz -P /tmp \
    && tar xf /tmp/apache-maven-*.tar.gz -C /opt \
    && ln -s /opt/apache-maven-"${MVN_VER}" /opt/maven \
    && curl -L https://github.com/docker/compose/releases/download/1.25.4/docker-compose-$(uname -s)-$(uname -m) -o /usr/local/bin/docker-compose \
    && chmod +x /usr/local/bin/docker-compose \
    && apt-get -q clean -y \
    && rm -rf /tmp/* /var/lib/apt/lists/* /var/cache/apt/*.bin

#------------------------------------------------------------------------------
# Add openjfx java lib - REMEMBER ! WILL WORK ONLY WITH CURRENT IMAGE due JDK8 IS BASE IMAGE FOR JENKINS
#------------------------------------------------------------------------------
COPY openjfx /usr/local/openjdk-8/
#------------------------------------------------------------------------------
# Set default locale:
#------------------------------------------------------------------------------
RUN echo "en_US.UTF-8 UTF-8" > /etc/locale.gen \
    && echo "LANGUAGE=en_US.UTF-8 UTF-8" >> /etc/default/locale \
    && echo "LC_ALL=en_US.UTF-8 UTF-8" >> /etc/default/locale
#------------------------------------------------------------------------------
# Set args and labels:
#------------------------------------------------------------------------------
ARG BUILD_DATE
ARG VCS_REF

LABEL build-date="${BUILD_DATE}" \
      vcs-ref="${VCS_REF}" \
      maintainer="olllol"
#------------------------------------------------------------------------------
# Going back to jenkins:
#------------------------------------------------------------------------------
USER jenkins
ENV M2_HOME=/opt/maven \
    MAVEN_HOME=/opt/maven
ENV PATH="${M2_HOME}"/bin:"$JAVA_HOME"/bin:"${PATH}"
