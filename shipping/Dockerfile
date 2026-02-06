# FROM maven:3.9.4 
# EXPOSE 8080
# WORKDIR /opt/server
# COPY pom.xml .
# COPY src ./src
# RUN mvn clean package
# RUN mv target/shipping-1.0.jar shipping.jar
# ENV CART_ENDPOINT="cart:8080"
# ENV DB_HOST="mysql"
# CMD [ "java", "-jar", "shipping.jar" ]

FROM maven:3.9.4 AS builder
WORKDIR /opt/server
COPY pom.xml .
COPY src ./src
RUN mvn clean package && \
    mv target/shipping-1.0.jar shipping.jar

FROM eclipse-temurin:17-jre-alpine
WORKDIR /opt/server
EXPOSE 8080
RUN addgroup -S roboshop && adduser -S roboshop -G roboshop && \
    chown -R roboshop:roboshop /opt/server
ENV CART_ENDPOINT="cart:8080" \ 
    DB_HOST="mysql"
USER roboshop
COPY --from=builder --chown=roboshop:roboshop /opt/server/shipping.jar .
ENTRYPOINT [ "java", "-jar", "shipping.jar" ]
