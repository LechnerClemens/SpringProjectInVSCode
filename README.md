
# Create a simple Spring Project in VSCode

## Create Project

- Ctrl+Shift+P
- Type: Spring Initializr: Create A Maven Project...
- Specify Version
- Java
- Specify Input Groupd Id (e.g. com.projectname)
- Input Artifact Id (this is the name of the root folder)
- Jar
- Java Version

### Dependencies

- Spring Boot DevTools
- Spring Web
- Rest Repositories

You don´t need to select Lombok when you have the extension.

Now you can hit "Generate into this folder", the foldername that will be generated is named after the Artifact Id.

Where you have your main Application.java you can create three folders:

- Domain
- Model 
- Service

## Properties

application.properties
```
#---------------------------------------------------
#   service configuration
#---------------------------------------------------
spring.profiles.active=production
spring.main.banner-mode=console
#---------------------------------------------------
#   service layer configuration
#---------------------------------------------------
server.context-path=/<yourpath e.g projectname>
```

application-production.properties
```
#---------------------------------------------------
# server configuration
#---------------------------------------------------
server.port=8082
#---------------------------------------------------
# datasource configuration
#---------------------------------------------------
spring.datasource.url=jdbc:mysql://localhost:3306/<databasename>?useSSL=false
spring.datasource.username=<databaseuser>
spring.datasource.password=<databaseuserpassword>
#---------------------------------------------------
# jpa configuration
#---------------------------------------------------
spring.jpa.show-sql=true
spring.jpa.hibernate.ddl-auto = create
spring.datasource.initialization-mode=always
spring.jpa.defer-datasource-initialization = true
```
You can put a data.sql file with some sample data in the same folder and it will automatically fill the tables.


## pom.xml

These are only the dependencies wich can be copy pasted in the Project

```
<dependencies>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-data-jpa</artifactId>
        </dependency>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-data-rest</artifactId>
        </dependency>

        <dependency>
            <groupId>mysql</groupId>
            <artifactId>mysql-connector-java</artifactId>
            <scope>runtime</scope>
        </dependency>
        <dependency>
            <groupId>org.projectlombok</groupId>
            <artifactId>lombok</artifactId>
            <optional>true</optional>
        </dependency>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-test</artifactId>
            <scope>test</scope>
            <exclusions>
                <exclusion>
                    <groupId>org.junit.vintage</groupId>
                    <artifactId>junit-vintage-engine</artifactId>
                </exclusion>
            </exclusions>
        </dependency>
        <dependency>
            <groupId>junit</groupId>
            <artifactId>junit</artifactId>
            <scope>test</scope>
        </dependency>
        <dependency>
            <groupId>junit</groupId>
            <artifactId>junit</artifactId>
            <scope>test</scope>
        </dependency>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-validation</artifactId>
        </dependency>
        <dependency>
            <groupId>io.springfox</groupId>
            <artifactId>springfox-boot-starter</artifactId>
            <version>3.0.0</version>
        </dependency>
    </dependencies>
```

## Extensions for VSCode

- Debugger for Java
- Java Extension Pack
- Java Test Runner
- Lombok Annotations Support for VS Code
- Maven for Java
- MySQL
- Project Manager for Java
- Spring Boot Dashboard
- Spring Boot Extension Pack
- Spring Boot Tools
- Spring Initializr Tools
