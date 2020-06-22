# Docker-SpringBoot

# 1. Create Spring Boot from Initializer
=> https://start.spring.io/

# 2. Write get method in main java file
Eg. 

{
	package com.dileepdail.springbootdemo;

	import org.springframework.boot.SpringApplication;
	import org.springframework.boot.autoconfigure.SpringBootApplication;
	import org.springframework.web.bind.annotation.GetMapping;
	import org.springframework.web.bind.annotation.RequestParam;
	import org.springframework.web.bind.annotation.RestController;

	@SpringBootApplication
	@RestController
	public class SpringbootdemoApplication {

		public static void main(String[] args) {
			SpringApplication.run(SpringbootdemoApplication.class, args);
		}

		@GetMapping("/")
		public String hello(@RequestParam(value = "name", defaultValue = "World") String name) {
			return String.format("Hello %s!", name);
		}
	}
}

# 3. Defining a docker image with Dockerfile
Create docker file in the root directory

Eg.

Start with a base image containing Java runtime
FROM openjdk:8-jdk-alpine

Add Maintainer Info
LABEL maintainer="dileepdail@gmail.com"

Add a volume pointing to /tmp
VOLUME /tmp

Make port 8080 available to the world outside this container
EXPOSE 8080

The application's jar file
ARG JAR_FILE=target/springbootdemo-0.0.1-SNAPSHOT.jar

Add the application's jar to the container
ADD ${JAR_FILE} springbootdemo.jar

Run the jar file 
ENTRYPOINT ["java","-Djava.security.egd=file:/dev/./urandom","-jar","/springbootdemo.jar"]


# 4. Building the Docker image

mvn clean package
docker build -t spring-boot-docker-demo .

# 5. Running the docker image
docker run -d -p 5000:8080 spring-boot-docker-demo
