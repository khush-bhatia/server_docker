# apache container; see https://hub.docker.com/_/httpd/
FROM httpd:2.4

# replace shell with bash so we can source files
RUN rm /bin/sh && ln -s /bin/bash /bin/sh

# install git
#RUN apt-get update && apt-get install git -y

# update the repository sources list
# and install dependencies
RUN apt-get update \
    && apt-get install -y curl \
    && apt-get -y autoclean

# nvm environment variables
RUN mkdir /nvm
ENV NVM_DIR /nvm
ENV NODE_VERSION 7.9.0

# install nvm
# https://github.com/creationix/nvm#install-script
RUN curl --silent -o- https://raw.githubusercontent.com/creationix/nvm/master/install.sh | bash

# install node and npm
RUN source $NVM_DIR/nvm.sh \
    && nvm install $NODE_VERSION \
    && nvm alias default $NODE_VERSION \
    && nvm use default

# add node and npm to path so the commands are available
ENV NODE_PATH $NVM_DIR/v$NODE_VERSION/lib/node_modules
ENV PATH $NVM_DIR/versions/node/v$NODE_VERSION/bin:$PATH

#install angular cli
RUN npm install -g @angular/cli

# The custom httpd.conf has rewrite rules for handling html5 routing
COPY ./httpd.conf /usr/local/apache2/conf/httpd.conf

# build website into the docker container 
COPY ./code/ /openchirp/
ADD ./config.ts /openchirp/src/app/
WORKDIR /openchirp
RUN npm install
RUN npm install typescript@'>=2.1.0 <2.4.0'
RUN ng build --prod --aot
RUN cp -R /openchirp/dist/* /usr/local/apache2/htdocs/
