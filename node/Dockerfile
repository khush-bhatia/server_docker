# node container; see https://hub.docker.com/r/_/node/
FROM node:7.10.0-alpine

# update node (uncomment, if needed...)
#sudo npm cache clean -f
#sudo npm install -g n
#sudo n stable

# copy code into the docker container 
COPY ./code/ /usr/src/app/

# change working directory
WORKDIR /usr/src/app/

# install dependencies according to package.json
RUN npm install

COPY startup-node.sh /
COPY docker.json.start /
RUN chmod +x /startup-node.sh
CMD ["/bin/sh","/startup-node.sh"]
