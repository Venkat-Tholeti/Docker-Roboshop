FROM node:20
WORKDIR /opt/server
COPY package.json .
COPY *.js  .
RUN npm install
ENV CATALOGUE_HOST="Catalogue" \
    REDIS_HOST="Redis" \
    CATALOGUE_PORT="8080"
CMD [ "node", "server.js" ] 
#CMD IS USED HERE FOR STARTING CONTAINER 