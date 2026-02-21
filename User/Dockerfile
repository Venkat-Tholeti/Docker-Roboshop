# FROM node:20
# WORKDIR /opt/server
# COPY package.json .
# COPY *.js  .
# RUN npm install
# ENV MONGO="true" \
#     MONGO_URL="mongodb://MongoDb:27017/users" \
#     REDIS_URL="redis://redis:6379" 
# CMD [ "node", "server.js" ] 
# #CMD IS USED HERE FOR STARTING CONTAINER 



FROM node:20-alpine3.21 AS builder
WORKDIR /opt/server
COPY package.json .
COPY *.js .
RUN npm install

FROM node:20-alpine3.21
RUN addgroup -S roboshopgroup && adduser -S roboshop -G roboshopgroup
ENV MONGO_URL="mongodb://mongodb:27017/users" \
    REDIS_URL="redis://redis:6379" \
    MONGO=true
WORKDIR /opt/server
USER roboshop
COPY --from=builder /opt/server /opt/server
CMD ["node","server.js"]