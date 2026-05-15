FROM node:20-alpine3.21
RUN  addgroup -S roboshop && adduser -S roboshop -G roboshop
WORKDIR /opt/server
RUN npm install
RUN chown -R roboshop:roboshop /opt/server
COPY package.json .
COPY *.js .
ENV MONGO="true" \
    MONGO_URL="mongodb://mongodb:27017/catalogue"
#here in mongodb:27017 , mongodb is the name we have given while creating the container, here it is the record
CMD ["node","server.js"]
