FROM node:20-alpine3.21
WORKDIR /opt/server
COPY package.json .
COPY *.js .
RUN npm install
ENV MONGO="true" \
    MONGO_URL="mongodb://mongodb:27017/catalogue"
#here in mongodb:27017 , mongodb is the name we have given while creating the container, here it is the record
CMD ["node","server.js"]
