# FROM node:20
# WORKDIR /opt/server
# COPY package.json .
# COPY *.js  .
# RUN npm install
# ENV CATALOGUE_HOST="Catalogue" \
#     REDIS_HOST="Redis" \
#     CATALOGUE_PORT="8080"
# CMD [ "node", "server.js" ] 
# #CMD IS USED HERE FOR STARTING CONTAINER 


FROM node:20-alpine3.21 AS builder
WORKDIR /opt/server
COPY package.json .
COPY *.js .
RUN npm install

FROM node:20-alpine3.21
RUN addgroup -S roboshopgroup && adduser -S roboshop -G roboshopgroup
ENV REDIS_HOST="redis" \
    CATALOGUE_HOST="catalogue" \
    CATALOGUE_PORT="8080"
WORKDIR /opt/server
USER roboshop
COPY --from=builder /opt/server /opt/server
CMD ["node","server.js"]