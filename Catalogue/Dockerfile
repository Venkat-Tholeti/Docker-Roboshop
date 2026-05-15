#STAGE 1 - BUILD
FROM node:20-alpine3.21 AS builder
WORKDIR /opt/server
COPY package.json .
COPY *.js .
RUN npm install


#STAGE 2
FROM node:20-alpine3.21
RUN  addgroup -S roboshop && adduser -S roboshop -G roboshop
ENV MONGO="true" \
    MONGO_URL="mongodb://mongodb:27017/catalogue"
#here in mongodb:27017 , mongodb is the name we have given while creating the container, here it is the record
WORKDIR /opt/server
USER roboshop
COPY --from=builder /opt/server /opt/server 
#output from builder is taken and stored in there
CMD ["node","server.js"]



# FROM node:20-alpine3.21
# RUN  addgroup -S roboshop && adduser -S roboshop -G roboshop
# WORKDIR /opt/server
# COPY package.json .
# COPY *.js .
# RUN npm install
# RUN chown -R roboshop:roboshop /opt/server
# ENV MONGO="true" \
#     MONGO_URL="mongodb://mongodb:27017/catalogue"
# #here in mongodb:27017 , mongodb is the name we have given while creating the container, here it is the record
# USER roboshop
# CMD ["node","server.js"]
