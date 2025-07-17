# Spring Boot Project Creation with Gradle

This guide offers a detailed, step-by-step approach to creating a Spring Boot project with Gradle using the command line. We'll also show you how to get your project up and running in IntelliJ IDEA.

## Prerequisites

Before you begin, make sure you have the following installed and configured on your system:

1. **Java Development Kit (JDK):** You'll need Java 8 or higher, preferably Java 11 or 17. Verify your installation by running `java -version`.
2. **Gradle:** Verify your installation by running `gradle -v`.
3. **IntelliJ IDEA**
4. **Command Line Tool**

## Setting Up the Project Structure

- Open your Terminal or Command Prompt and navigate to the directory where you want to create the project:

        cd ~/Documents/Workspace/EducateWorkspace

- Create a new directory for your project and switch to the newly created directory:

        mkdir tspark-springboot-gradle
        cd tspark-springboot-gradle

## Initializing the Gradle Project

Generate the project structure using Gradle’s `init` command:

        gradle init

During initialization, follow these prompts:

- **Select type of project:** Application
- **Select implementation language:** Java
- **Enter target Java version (min: 7, default: 21):** 17
- **Project name:** tspark-springboot-gradle
- **Select application structure:** Single application project
- **Select build script DSL:** Groovy
- **Select test framework:** JUnit4

Generate a Gradle wrapper:

    gradle wrapper

This creates a `gradlew` script for Linux/Mac and `gradlew.bat` for Windows, along with the `gradle/wrapper` directory containing wrapper files.

## Initialize the Gradle Project

Generate the project structure using Gradle’s `init` command:

        gradle init

Follow these prompts during initialization:

- **Select type of project:** Application
- **Select implementation language:** Java
- **Enter target Java version (min: 7, default: 21):** 17
- **Project name:** tspark-springboot-gradle
- **Select application structure:** Single application project
- **Select build script DSL:** Groovy
- **Select test framework:** JUnit4

Generate a Gradle wrapper:

    gradle wrapper

This creates a `gradlew` script for Linux/Mac and `gradlew.bat` for Windows, along with the `gradle/wrapper` directory containing wrapper files.

## Opening the Project in IntelliJ IDEA or VS Code

You can now open the project in your preferred IDE. Either launch your IDE and manually browse to open the project directory, or run the following command from the project folder for convenience:

        open -na "IntelliJ IDEA CE.app" .   #For opening in Intellij
        code .   #For opening in vscode

_Note:_ Ensure your IDE is added to your system’s PATH for these commands to work.

## Updating the `build.gradle` File

Open the `build.gradle` file in your IDE and modify it to include Spring Boot dependencies and plugins:

```groovy
plugins {
  id 'org.springframework.boot' version '3.1.4'
  id 'io.spring.dependency-management' version '1.1.3'
  id 'java'
}

group = 'org.techspark'
version = '0.0.1-SNAPSHOT'
sourceCompatibility = '17'

repositories {
    mavenCentral()
}

dependencies {
    implementation 'org.springframework.boot:spring-boot-starter-web'
    testImplementation 'org.springframework.boot:spring-boot-starter-web-test'
}
```

## Creating the Main Application Class

The main application class in a Spring Boot project is the entry point for the application. It contains the main method, which starts the Spring Boot application and initializes its runtime environment.

- Create a package of your choice under the `src\main\java` folder. (e.g., `org.techspark`)
- Under the newly created package, create a new Java class file `Application.java` with the following content:

```java
package org.techspark;

import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;

@SpringBootApplication
public class Application {
    public static void main(String[] args) {
        SpringApplication.run(Application.class, args);
    }
}
```

- Add a simple controller under the same package `GreetingController.java`:

```java
package org.techspark;

import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.RestController;

@RestController
public class GreetingController {
    @GetMapping("/")
    public String greeting() {
        return "Greeting from TechSpark";
    }
}
```

## Running the Application

Run the application from the IDE or using the Gradle command `./gradlew bootRun`.

Open a browser or use curl to visit the default endpoint:

```shell
curl http://localhost:8080
```

You should see the following message:

```shell
Greeting from TechSpark
```

Whether you're a seasoned developer or just starting with Spring Boot, creating a project manually can deepen your understanding and give you greater control over your setup. To streamline your future projects, consider creating a skeleton repository with all the basic configurations and dependencies you frequently use. This can save you time and effort when starting new services.

You can find the final code at: [My Spring Boot Skeleton Repository](https://github.com/TechSparkWorkspace/tspark-springboot-gradle.git).
