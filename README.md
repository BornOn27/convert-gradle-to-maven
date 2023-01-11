# convert-gradle-to-maven
This project will demonstrate every specific details to convert a `gradle` project to `maven`

# Steps for conversion
- Generate `pom.xml`
- Modify `pom.xml`
- Add missing tags
- Build the maven project

## Generate pom.xml

This is initial [`build.gradle`](https://github.com/BornOn27/convert-gradle-to-maven/blob/main/migration-resources/original-project-build.gradle) file 
- Add [`apply plugin: 'maven-publish'`](https://github.com/BornOn27/convert-gradle-to-maven/blob/main/build.gradle#L7) to `build.gradle`
- Add [`publishing`](https://github.com/BornOn27/convert-gradle-to-maven/blob/main/build.gradle#L23-L31) task in `build.gradle`
- Run `gradle publish` and `gradle generatePomFileForCustomLibraryPublication` (if `gradle` is not installed, refer [Terminal Commands](https://github.com/BornOn27/convert-gradle-to-maven#gradle))
- Copy `build/publications/customLibrary/pom-default.xml` and move  to project root as **`pom.xml`** 
- Add [`mvnw`](https://github.com/BornOn27/convert-gradle-to-maven/blob/main/mvnw) , [`mvnw.cmd`](https://github.com/BornOn27/convert-gradle-to-maven/blob/main/mvnw.cmd) & [`.mvn`](https://github.com/BornOn27/convert-gradle-to-maven/tree/main/.mvn/wrapper) to project root as these files to help `run maven commands` via terminal



## Modify `pom.xml`

When `gradle` task publishes the `pom.xml`, **it misses many `spring-boot` dependency which
we will add manually.**

This is initial [`pom.xml`](https://github.com/BornOn27/convert-gradle-to-maven/blob/main/migration-resources/pom-generated-via-gradle.xml) file

### Add `parent` tag
Add Spring-boot as parent tag as it will serve all its related dependency
   ```
   <parent>
	<groupId>org.springframework.boot</groupId>
	<artifactId>spring-boot-starter-parent</artifactId>
	<version>2.5.5</version>
	<relativePath/>
   </parent>
   ```

### Add missing tags
We need to add all spring-boot related dependency which is not present in pom.xml but added in build.gradle
- Add all missing `spring-boot` [dependencies](https://github.com/BornOn27/convert-gradle-to-maven/blob/main/pom.xml#L36-L55)
- Add maven [repositories](https://github.com/BornOn27/convert-gradle-to-maven/blob/main/pom.xml#L68-L73)
- Add [plugin](https://github.com/BornOn27/convert-gradle-to-maven/blob/main/pom.xml#L21-L33) to generate `spring-boot` specific fat jar as a part of `build` stage

### Build the maven project
All maven available goals - 
-   clean
-   validate
-   compile
-   test
-   package
-   verify
-   install
-   site
-   deploy

#### Run Maven commands to generate jar
    mvn clean package

# Terminal Commands
Running `gradle` and `maven` commands via terminal will require to install the packages, if not installed we can use the localized scripts present at project root.

## Gradle
If `gradle` terminal package installed.

    gradle clean build

If `gradle` terminal package not installed, run following commands using [`gradlew`](https://github.com/BornOn27/convert-gradle-to-maven/blob/main/gradlew) at project root.

    ./gradlew clean build

## Maven
If `maven` terminal package installed.

    mvn clean compile package

If `maven` terminal package not installed, run following commands using [`mvnw`](https://github.com/BornOn27/convert-gradle-to-maven/blob/main/mvnw) at project root.

    ./mvnw clean compile package

# References
- https://www.baeldung.com/gradle-build-to-maven-pom
