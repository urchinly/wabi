# [WABI-SABI](https://github.com/unauthed/wabi-sabi) - Digital Asset Management

Wabi and sabi are two of the key Japanese aesthetic concepts. Their definitions are not exact, but one can get a sense of them from a short discussion of them. Over time, the two have been combined to form a new word, wabi-sabi, meaning an aesthetic sensibility which includes these two related ideas.

Open source Digital Asset Management platform of great simplicity and beauty.

## Wabi [![Build Status](https://travis-ci.org/unauthed/wabi.svg?branch=master)](https://travis-ci.org/unauthed/wabi)

Wabi means things that are fresh and simple. It denotes simplicity and quietude, and also incorporates rustic beauty. It includes both that which is made by nature, and that which is made by man. It also can mean an accidental or happenstance element (or perhaps even a small flaw) which gives elegance and uniqueness to the whole, such as the pattern made by a flowing glaze on a ceramic object.

Wabi modules form the core components of our digital asset management solution.

## [Sabi](https://github.com/unauthed/sabi) [![Build Status](https://travis-ci.org/unauthed/sabi.svg?branch=master)](https://travis-ci.org/unauthed/sabi)

Sabi means things whose beauty stems from age. It refers to the patina of age, and the concept that changes due to use may make an object more beautiful and valuable. This also incorporates an appreciation of the cycles of life, as well as careful, artful mending of damage.

Sabi modules form the custom components of our digital asset management solution.

### Build and run locally

First we build the Docker containers for our third-party services RabbitMQ, ElasticSearch and MongoDB. Next step is to build our platform micro-services as Docker containers.
Followed by starting the application with Docker Compose and testing the uploading and downloading of files with _curl_.

- Maven 3.2
- Java 8
- Docker 1.10
- Docker Compose 1.7.0

```
./elasticsearch/build.sh
./rabbitmq/build.sh

mvn clean install -P prod

./wabi-ingest/mvn docker:build
./wabi-expose/mvn docker:build
./wabi-search/mvn docker:build

docker-compose up

curl -v -F file=@yourFileName http://localhost:8888/upload
curl -v http://localhost:8888/assets
```

### Maven Tips

Build a module

* `mvn clean install`

Run a module

* `mvn spring-boot:run`

* `mvn spring-boot:run -DskipTests`

* `mvn clean install && java -jar target/wabi-*.jar`

Create Docker image

* `mvn clean install docker:build -P prod`

Check and format licence headers across the project

* `mvn license:check`

* `mvn license:format`

Check for latest versions

* `mvn versions:display-dependency-updates`

* `mvn versions:display-plugin-updates`

* `mvn versions:display-property-updates`

### Docker Tips

Run one module

* `docker run -d -e "SPRING_PROFILES_ACTIVE=prod" -p 8081:8081 --name ingest -h ingest urchinly/wabi-ingest`
* `docker exec -it ingest /bin/sh`

Run all modules

* `docker-compose up` and `docker-compose down`

Remove all containers

* `docker rm $(docker ps -q -a)`

Remove all images

* `docker rmi $(docker images -q -a)`

Remove all volumes

* `docker volume rm $(docker volume ls -q -f dangling=true)`

Remove all networks

* `docker network rm $(docker network ls -q)`
