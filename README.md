Spring Music
============

This is a sample application for using database services on [Kubo](https://pivotal.io/partners/kubo) with the [Spring Framework](http://spring.io) and [Spring Boot](http://projects.spring.io/spring-boot/).

This application has been built to store the same domain objects in one of a variety of different persistence technologies - relational, document, and key-value stores. This is not meant to represent a realistic use case for these technologies, since you would typically choose the one most applicable to the type of data you need to store, but it is useful for testing and experimenting with different types of services on Kubernetes. 


## Running the application locally

One Spring bean profile should be activated to choose the database provider that the application should use. The profile is selected by setting the system property `spring.profiles.active` when starting the app.

The application can be started locally using the following command:

~~~
$ ./gradlew clean assemble
$ java -jar -Dspring.profiles.active=<profile> build/libs/spring-music.jar
~~~

where `<profile>` is one of the following values:

* `in-memory` (no external database required)
* `mysql`
* `postgres`
* `mongodb`
* `redis`

If no profile is provided, `in-memory` will be used. If any other profile is provided, the appropriate database server must be started separately. The application will use the host name `localhost` and the default port to connect to the database. The connection parameters can be configured by setting the appropriate [Spring Boot properties](http://docs.spring.io/spring-boot/docs/current/reference/html/common-application-properties.html). 

If more than one of these profiles is provided, the application will throw an exception and fail to start.

## Running the application on Kubernetes

1. `docker build -t {your_tag} .`
1. `docker push {your_image}`
1. Edit the `spring-music.yml` file to utilise the container tag specified.
1. `kubectl create -f .`

The default database is `in-memory` for Kubernetes.

### Creating and binding services

Using the provided manifest, the application will be created without an external database (in the `in-memory` profile).

#### Database drivers

Database drivers for MySQL, Postgres, Microsoft SQL Server, MongoDB, and Redis are included in the project. To connect to an Oracle database, you will need to download the appropriate driver (e.g. from http://www.oracle.com/technetwork/database/enterprise-edition/jdbc-112010-090769.html?ssSourceSiteId=otnjp), add the driver .jar file to the `src/main/webapp/WEB-INF/lib` directory in the project, and re-build the application .jar file.
