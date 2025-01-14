FROM maven:3.8.6-openjdk-11

WORKDIR /var/www/html

RUN addgroup --gid=1000 app
RUN adduser --disabled-password --quiet --uid=1000 --gid=1000 --no-create-home --gecos GECOS app

#EXPOSE 9000

# ENV MAVEN_CONFIG=/var/www/html/.m2

#ENTRYPOINT [ "mvn", "quarkus:dev", "-DdebugHost=0.0.0.0"]

#ENTRYPOINT [ "sleep",  "infinity" ]

# this is for keeping container running
ENTRYPOINT ["tail", "-f", "/dev/null"]
