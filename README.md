# Cloud Config Server

This guide explains how you can configure your Spring Cloud Server. See https://cloud.spring.io/spring-cloud-config/reference/html/

## Stand up a Spring Cloud Config Server

The server is embeddable in a Spring Boot application, by using the @EnableConfigServer annotation.

```
@SpringBootApplication
@EnableConfigServer
public class ConfigServer {
  public static void main(String[] args) {
    SpringApplication.run(ConfigServer.class, args);
  }
}
```

## Dependencies

You have to add the following to your pom.xml:

``` 
    <dependency>
		<groupId>org.springframework.cloud</groupId>
		<artifactId>spring-cloud-config-server</artifactId>
	</dependency>
```

## Application properties

The default strategy for locating property sources is to use a git repository. You can set the spring.cloud.config.server.git.uri configuration property.

File:

application.properties

```
server.port: 8888
spring.cloud.config.server.git.uri: https://github.com/<user>/config-server.git
```

## Cloud Application properties

The application name that was configured when standing up the Config Server. This file is stored at a git repository. Example:

Git Repository: 

```
https://github.com/<user>/config-server.git
```

File:

a-bootiful-client.properties
    
```
quarkus.http.port=8083
message: Hello world!
```

See a example at https://github.com/ndigrazia/config-server

## Package and run the application
./mvnw clean compile spring-boot:run

## Test Spring Cloud Config Server:
Open your browser to http://localhost:8888/a-bootiful-client/main

# Quarkus Cloud Config Client

This guide explains how you can configure your Quarkus Client. See https://quarkus.io/guides/spring-cloud-config-client#stand-up-a-config-server

## Creating the Maven project

Run the following command:

```
mvn io.quarkus.platform:quarkus-maven-plugin:2.5.2.Final:create -DprojectGroupId="com.telefonica.hispam" -DprojectArtifactId=spring-cloud-config-quickstart -DclassName="com.telefonica.hispam.client.GreetingResource" -Dpath="/greeting" -Dextensions="spring-cloud-config-client"
```

## Dependencies

Run the following command:

```
./mvnw quarkus:add-extension -Dextensions="spring-cloud-config-client"
```

This will add the following to your pom.xml:

```
<dependency>
    <groupId>io.quarkus</groupId>
    <artifactId>quarkus-spring-cloud-config-client</artifactId>
</dependency>
```

## Controller

Update the code to:

```
@Path("/greeting")
public class GreetingResource {

    @ConfigProperty(name = "message", defaultValue="hello default")
    String message;

    @GET
    @Produces(MediaType.TEXT_PLAIN)
    public String hello() {
        return message;
    }
}
```

## Application properties

Quarkus application is going to be configured in application.properties as follows:

File:

application.properties

```
quarkus.application.name=a-bootiful-client
quarkus.spring-cloud-config.label=main
quarkus.spring-cloud-config.enabled=true
quarkus.spring-cloud-config.url=http://localhost:8888
```

## Package and run the application
Run the application with: ./mvnw clean compile quarkus:dev. Open your browser to http://localhost:8080/greeting.

