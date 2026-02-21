
# Multi stage builds are like keeping multiple Dockerfiles in a single Dockerfile, one Dockerfile we can use it as builder, another Dockerfile we can use it as final image, we can copy what we want from builder to final image. this reduces memory and then unnecessary installations

FROM node:20-alpine3.21 AS builder
WORKDIR /opt/server
COPY package.json .
COPY *.js .
RUN npm install
#THIS WILL BE DELETED AUTOMATICALLY WHILE FINAL IMAGE IS CREATED

FROM node:20-alpine3.21
RUN addgroup -S roboshopgroup && adduser -S roboshop -G roboshopgroup
ENV MONGO="true" \
    MONGO_URL="mongodb://mongodb:27017/catalogue"
WORKDIR /opt/server
USER roboshop
COPY --from=builder /opt/server /opt/server  
#HERE COPYING /OPT/SERVER FROM BUILDER  TO /OPT/SERVER FINAL IMAGE
CMD [ "node", "server.js" ] 



# FROM node:20-alpine3.21
# RUN addgroup -S roboshopgroup && adduser -S roboshop -G roboshopgroup
# WORKDIR /opt/server
# RUN chown -R roboshop:roboshopgroup /opt/server
# USER roboshop
# COPY package.json .
# COPY *.js  .
# RUN npm install
# ENV MONGO="true" \
#     MONGO_URL="mongodb://MongoDb:27017/catalogue"
# CMD [ "node", "server.js" ] 
#CMD IS USED HERE FOR STARTING CONTAINER 