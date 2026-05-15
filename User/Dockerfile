FROM node:20
WORKDIR /opt/server
COPY package.json .
COPY *.js .
RUN npm install
ENV MONGO="true"
    REDIS_URL="redis://redis:6379"
    MONGO_URL="mongodb://mongodb:27017/users"
#here in mongodb:27017 , mongodb is the name we have given while creating the container, here it is the record
CMD ["node","server.js"]