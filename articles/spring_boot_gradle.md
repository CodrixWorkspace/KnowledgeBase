# Spring Boot with Gradle

This guide provides a step-by-step approach, so you can master the art of creating a Spring Boot project with
Gradle using just the command line — and later bring it to life in IntelliJ IDEA.

## Prerequisites

To follow this guide, ensure you have the following installed and configured on your system:

1. Java Development Kit (JDK) :</b> Java 8 or higher (preferably Java 11 or 17)

    Verify installation by running `java -version`

2. Gradle

    Verify installation by running `gradle -v`

3. IntelliJ IDEA
4. Command Line Tool

## Setup Project Structure

- Open Terminal or Command prompt and navigate to the directory where you want to create the project

        cd ~/Documents/Workspace/EducateWorkspace
- Create a new directory for you project and switch to the newly created directory

        mkdir tspark-springboot-gradle

        cd tspark-springboot-gradle

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

Your project directory should look like this:
    <mark>Display the image here</mark>

## Opening the Project in IntelliJ IDEA or VS Code

Now is the perfect time to open the project in the IDE of your choice. You can launch your IDE and manually browse to open the project directory, or simply run the following command from the project folder for convenience

        open -na "IntelliJ IDEA CE.app" .   #For opening in Intellij

        code .   #For opening in vscode

_Note:_ Ensure your IDE is added to your system’s PATH for these commands to work.
