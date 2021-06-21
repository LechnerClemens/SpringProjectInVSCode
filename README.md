

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

## AResource

```
import lombok.extern.slf4j.Slf4j;
import org.hibernate.Hibernate;
import org.springframework.data.jpa.repository.JpaRepository;
import org.springframework.http.HttpStatus;
import org.springframework.http.MediaType;
import org.springframework.web.bind.annotation.*;
import javax.validation.Valid;
import java.io.Serializable;

@Slf4j
@SuppressWarnings("unchecked")
public abstract class AResource<R extends AEntity, ID extends Serializable> {

    private JpaRepository<R,ID> repository;

    public AResource(JpaRepository<R,ID> repository)
    {
        this.repository = repository;
    }
    
    @CrossOrigin
    @PostMapping(produces = MediaType.APPLICATION_JSON_VALUE)
    @ResponseStatus(HttpStatus.CREATED)
    public R create(@RequestBody @Valid R resource){
        R createdResource = repository.save(resource);
        log.info("created resource:" + createdResource.getId());

        return (R)Hibernate.unproxy(createdResource);
    }

    @CrossOrigin
    @GetMapping(path="/{id}")
    @ResponseStatus(HttpStatus.OK)
    public R read(@PathVariable("id") ID id)
    {
        R resource = repository.getOne(id);
        log.info("Fetching resource with id:" +resource.getId());
        return (R)Hibernate.unproxy(resource);
    }

    @CrossOrigin
    @PutMapping(path="/{id}")
    @ResponseStatus(HttpStatus.ACCEPTED)
    public void update(@RequestBody @Valid R resource, @PathVariable("id") ID id)
    {
        repository.save(resource);
        log.info("saved changes for resource: "+ id);
    }

    @CrossOrigin
    @DeleteMapping(path="/{id}")
    @ResponseStatus(HttpStatus.ACCEPTED)
    public void delete(@PathVariable("id") ID id){
        repository.deleteById(id);
        log.info("deleted resource: "+ id);
    }

}
```
### Import AResource

```
import org.hibernate.Hibernate;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.data.jpa.repository.JpaRepository;
import org.springframework.http.HttpStatus;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.bind.annotation.RestController;

import io.swagger.v3.oas.annotations.parameters.RequestBody;
import lombok.extern.slf4j.Slf4j;

import org.springframework.web.bind.annotation.CrossOrigin;
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.RequestParam;
import org.springframework.web.bind.annotation.ResponseStatus;


@RestController
@Slf4j
@RequestMapping(path="/users")
public class UserResource extends AResource<User, Long>{
    
    @Autowired
    private IUserRepository userRepository;

    public UserResource(@Autowired JpaRepository<User,Long> repository)
    {
        super(repository);
    }

}
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


