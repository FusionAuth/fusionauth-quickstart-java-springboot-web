# Quickstart: Spring Web and Thymeleaf with FusionAuth

This repository contains a SpringBoot application that works with a locally-running instance of [FusionAuth](https://fusionauth.io/), the authentication and authorization platform.

## Setup

### Prerequisites
You will need the following things properly installed on your computer.

* [Git](http://git-scm.com/): Presumably you already have this on your machine if you are looking at this project locally; if not, use your platform's package manager to install git, and `git clone` this repo.
* [Java](https://www.oracle.com/java/technologies/downloads/): Java can be installed via a variety of methods
* `mvn`: Maven, a java package manager. Install it by [following the instructions here](https://maven.apache.org/install.html).
* [Docker](https://www.docker.com): For standing up FusionAuth from within a Docker container. (You can [install it other ways](https://fusionauth.io/docs/v1/tech/installation-guide/), but for this example we will assume you are using Docker.)

This app was built using Java 17 and the maven wrapper. If you wish to use a different version of Java you will need to update the `java.version` in the [pom file](./complete-application/pom.xml).


### FusionAuth Installation via Docker

The root of this project directory (next to this README) are two files [a Docker compose file](./docker-compose.yml) and an [environment variables configuration file](./.env). Assuming you have Docker installed on your machine, you can stand up FusionAuth up on your machine with:

```
docker-compose up -d
```

The FusionAuth configuration files also make use of a unique feature of FusionAuth, called [Kickstart](https://fusionauth.io/docs/v1/tech/installation-guide/kickstart): when FusionAuth comes up for the first time, it will look at the [Kickstart file](./kickstart/kickstart.json) and mimic API calls to configure FusionAuth for use when it is first run.

> **NOTE**: If you ever want to reset the FusionAuth system, delete the volumes created by docker-compose by executing `docker-compose down -v`.

FusionAuth will be initially configured with these settings:

* Your client Id is: `e9fdb985-9173-4e01-9d73-ac2d60d1dc8e`
* Your client secret is: `super-secret-secret-that-should-be-regenerated-for-production`
* Your example username is `richard@example.com` and your password is `password`.
* Your admin username is `admin@example.com` and your password is `password`.
* Your fusionAuthBaseUrl is 'http://localhost:9011/'

You can log into the [FusionAuth admin UI](http://localhost:9011/admin) and look around if you want, but with Docker/Kickstart you don't need to.

### Spring Web complete-app

The `complete-application` directory contains a minimal SpringBoot app configured to authenticate with locally running FusionAuth.

Install the dependencies via the Maven wrapper and run the app server with:
```
cd complete-application
./mvnw package
./mvnw spring-boot:run
```
Note: If you are on Windows swap `./mvnw` with `.\mvnw.cmd`

Now vist the SpringBoot app at [http://localhost:8080](http://localhost:8080)
You can login with a user preconfigured during Kickstart, `richard@example.com` with the password of `password`.

### Further Information

Visit https://fusionauth.io/quickstarts/quickstart-springboot-web for a step by step guide on how to build this Rails app integrated with FusionAuth by scratch.

### Troubleshooting

* I get `This site can’t be reached  localhost refused to connect.` when I click the Login button

Ensure FusionAuth is running in the Docker container.  You should be able to login as the admin user, `admin@example.com` with the password of `password` at http://localhost:9011/admin

* I get an error page when I click on the Login button with message of `"error_reason" : "invalid_client_id"`

Ensure the value for `spring.security.oauth2.client.registration.fusionauth-client.client-id` in the [application properties file](complete-application/src/main/resources/application.properties) matches client id configured in FusionAuth for the Example App application at http://localhost:9011/admin/application/
