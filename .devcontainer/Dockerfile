# See here for image contents: https://github.com/microsoft/vscode-dev-containers/tree/v0.166.1/containers/java/.devcontainer/base.Dockerfile

# [Choice] Java version: 11, 15
ARG VARIANT="15"
FROM mcr.microsoft.com/vscode/devcontainers/java:0-${VARIANT}

# [Option] Install Maven
ARG INSTALL_MAVEN="false"
ARG MAVEN_VERSION=""
# [Option] Install Gradle
ARG INSTALL_GRADLE="false"
ARG GRADLE_VERSION=""
RUN if [ "${INSTALL_MAVEN}" = "true" ]; then su vscode -c "umask 0002 && . /usr/local/sdkman/bin/sdkman-init.sh && sdk install maven \"${MAVEN_VERSION}\""; fi \
    && if [ "${INSTALL_GRADLE}" = "true" ]; then su vscode -c "umask 0002 && . /usr/local/sdkman/bin/sdkman-init.sh && sdk install gradle \"${GRADLE_VERSION}\""; fi

# [Option] Install Node.js
ARG INSTALL_NODE="true"
ARG NODE_VERSION="lts/*"
RUN if [ "${INSTALL_NODE}" = "true" ]; then su vscode -c "umask 0002 && . /usr/local/share/nvm/nvm.sh && nvm install ${NODE_VERSION} 2>&1"; fi

# [Optional] Uncomment this section to install additional OS packages.
# RUN apt-get update && export DEBIAN_FRONTEND=noninteractive \
#     && apt-get -y install --no-install-recommends <your-package-list-here>
# Instalando Postgresql 11.
RUN RELEASE=$(lsb_release -cs) && apt-get update && wget --quiet -O - https://www.postgresql.org/media/keys/ACCC4CF8.asc | apt-key add - && echo "deb http://apt.postgresql.org/pub/repos/apt/ ${RELEASE}"-pgdg main | tee  /etc/apt/sources.list.d/pgdg.list && apt-get update && apt -y install postgresql-11 && service postgresql start \
    &&  su - postgres -c "psql postgres -c \"create user vscode with password 'vscode' \"" \
    &&   su - postgres -c "psql postgres -c 'create database vscode with owner vscode'" 

# && su - postgres -c "echo 'localhost:5432:test_db:test_l2:password' > .pgpass;createuser test_l2;createdb test_db -O test_l2"


RUN apt install -y dirmngr gnupg apt-transport-https software-properties-common ca-certificates curl \
&& curl -fsSL https://www.mongodb.org/static/pgp/server-4.2.asc | sudo apt-key add - \
&& add-apt-repository 'deb https://repo.mongodb.org/apt/debian buster/mongodb-org/4.2 main' \
&& apt update \
&& apt install -y mongodb-org \
&& systemctl enable mongod --now \
&& mkdir -p /data/mongodb

 
# [Optional] Uncomment this line to install global node packages.
# RUN su vscode -c "source /usr/local/share/nvm/nvm.sh && npm install -g <your-package-here>" 2>&1