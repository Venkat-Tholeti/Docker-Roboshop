FROM maven
WORKDIR /opt/server
COPY pom.xml .
COPY src /opt/server/src
RUN mvn clean package
RUN mv /opt/server/target/shipping-1.0.jar /opt/server/shipping.jar
ENV CART_ENDPOINT = "Cart:8080" \
    DB_HOST="MySql"
CMD [ "java", "-jar", "shipping.jar" ]

