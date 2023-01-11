# convert-gradle-to-maven
This project will demonstrate every specific details to convert a gradle project to maven

# Steps for conversion
This is initial `build.gradle` file 
- Add `apply plugin: 'maven-publish'` to `build.gradle`
- Add `publishing` task in `build.gradle`
- Run `gradle publish` and `gradle generatePomFileForCustomLibraryPublication`
- Copy pom.xml file from `build/publications/customLibrary/pom-default.xml` to project root
- Copy `mvnw` , `mvnw.cmd` & `.mvn` to project root

# Modify `pom.xml`
When gradle task publishes the pom.xml it misses the many spring-boot dependency which
we will add manually.

- Add Spring-boot as parent tag as it will serve all its related dependency
`
<parent>
<groupId>org.springframework.boot</groupId>
<artifactId>spring-boot-starter-parent</artifactId>
<version>2.5.5</version>
<relativePath/>
</parent>
`

- Add the following other dependency to pom.xml

`<dependency>
<groupId>org.springframework.boot</groupId>
<artifactId>spring-boot-starter-test</artifactId>
<scope>test</scope>
</dependency>
<dependency>
<groupId>org.springframework.boot</groupId>
<artifactId>spring-boot-starter-web</artifactId>
<scope>runtime</scope>
<exclusions>
<exclusion>
<artifactId>spring-boot-starter-logging</artifactId>
<groupId>*</groupId>
</exclusion>
</exclusions>
</dependency>
<dependency>
<groupId>org.springframework.boot</groupId>
<artifactId>spring-boot-devtools</artifactId>
</dependency>`

- Add repositories
- Add plugin to generate spring-boot specific fat jar
`<build>
  <plugins>
  <plugin>
  <groupId>org.springframework.boot</groupId>
  <artifactId>spring-boot-maven-plugin</artifactId>
  <executions>
  <execution>
  <goals>
  <goal>repackage</goal>
  </goals>
  </execution>
  </executions>
  </plugin>
  </plugins>
  </build>`

# References
- https://www.baeldung.com/gradle-build-to-maven-pom
