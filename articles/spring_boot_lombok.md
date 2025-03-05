# Effective Lombok Use in Spring Boot

This guide provides a step-by-step approach to make effective use of Lombok with Spring Boot.

## Summary

Using Lombok effectively in Spring Boot applications empowers you to be more productive in your daily development tasks. It minimizes boilerplate code, enabling you to concentrate on delivering features and improving your codebase. Additionally, Lombok’s reduction of repetitive code structures simplifies writing unit tests, making it quicker and easier to meet CI-enforced code coverage requirements. This leads to time savings and a more efficient workflow, enhancing both your productivity and the quality of your code.

To help you get hands-on experience without spending time setting up a project, I’ve created a sample repository that showcases Lombok’s capabilities within a Spring Boot application. This repository is designed for you to explore, experiment, and see firsthand how Lombok can enhance your productivity.

Repository Link: [tspark-springboot-lombok](https://github.com/TechSparkWorkspace/tspark-springboot-lombok)

Alternatively if you’re a started or like me and always looking to get better at doing things efficiently, you might be interested in my other guide: [Spring Boot with Gradle](https://skillhunt.codrixtech.com/guide/list). This guide walks you through setting up Spring Boot projects using Gradle, providing a solid foundation for your applications. By following this guide, you’ll streamline your setup process.

## Setting Up Lombok in a Spring Boot Project

Add the Lombok dependency to your build.gradle file:

    ```
    dependencies {
        implementation 'org.projectlombok:lombok:1.18.36'
        annotationProcessor 'org.projectlombok:lombok:1.18.36'
    }

    ```

## Getter and Setter Annotations

- Automatically generates getter and setter methods for class fields
- Apply `@Getter` and `@Setter` at the class level to generate methods for all fields or on individual fields for fine-grained control

    ```java
    @Getter
    @Setter
    public class Puzzle {
        private Integer puzzleId;
        private String question;
        private String difficulty;
        private String answer;
        private String category;

    }
    ```

Here is a JUnit Test Case to validate the setters and getters

    ```java
    public class PuzzleTest {

        @Test
        void testGettersAndSetters() {
            Puzzle puzzle = new Puzzle();
            puzzle.setQuestion("The more you take, the more you leave behind. What am I?");
            puzzle.setDifficulty("Easy");
            puzzle.setAnswer("Footsteps");

            assertEquals("Footsteps", puzzle.getAnswer());
            assertEquals("Easy", puzzle.getDifficulty());
        }

    }
    ```

## ToString Annotations

- Generates a toString() method
- Use @ToString(exclude = "password") to exclude sensitive fields.

    ```java
    @Getter
    @Setter
    @ToString(exclude = "answer")
    public class Puzzle {
        private Integer puzzleId;
        private String question;
        private String difficulty;
        private String answer;
        private String category;

    }
    ```

Validate the `toString` annotation

    ```java
    @Test
        void testToString() {
            Puzzle puzzle = new Puzzle();
            puzzle.setQuestion("The more you take, the more you leave behind. What am I?");
            puzzle.setDifficulty("Easy");
            puzzle.setAnswer("Footsteps");

            System.out.println(puzzle.toString());
        }
    ```

Output of the `toString` method

`Puzzle(puzzleId=null, question=The more you take, the more you leave behind. What am I?, difficulty=Easy, category=null`

## EqualsAndHashCode Annotation

- Generates equals() and hashCode() method's
- Use @EqualsAndHashCode(onlyExplicitlyIncluded = true) and @EqualsAndHashCode.Include for precise control over which fields are included

    ```java
    @Getter
    @Setter
    @ToString(exclude = "answer")
    @EqualsAndHashCode(exclude = {"puzzleId", "difficulty", "category"})
    @AllArgsConstructor
    @NoArgsConstructor
    public class Puzzle {
        private Integer puzzleId;
        private String question;
        private String difficulty;
        private String answer;
        private String category;

    }
    ```

Validate the `equals()` and `hashCode()` methods

    ```java
        @Test
        public void testEqualsWithCustomEqualsAndHashCode() {
            Puzzle puzzle1 = new Puzzle(1, "The more you take, the more you leave behind. What am I?", "easy", "Footsteps", "riddle");
            Puzzle puzzle2 = new Puzzle(2, "What has many keys but can’t open a single lock?", "medium", "A piano", "riddle");
            Puzzle puzzle3 = new Puzzle(3, "What has many keys but can’t open a single lock?", "easy", "A piano", "riddle");
            Puzzle puzzle4 = new Puzzle(4, "What has hands but can’t clap?", "medium", "A clock", "riddle");

            assertEquals(puzzle2, puzzle3, "The puzzle are the same");
            assertNotEquals(puzzle1, puzzle4, "The puzzles are different");
        }
    ```

Notice that we are using `@AllArgsConstructor` and `@NoArgsConstructor` in the above example to automatically generate both parameterized and no-argument constructors for our purposes. This approach eliminates the need to manually code constructors, reducing boilerplate and potential errors, and enhancing productivity

- `@AllArgsConstructor`: Generates a constructor with parameters for all fields in the class. Useful when you want to quickly create fully-initialized objects.
- `@NoArgsConstructor`: Generates a default no-argument constructor. Essential for frameworks that require a default constructor (e.g., JPA/Hibernate, Jackson)

## Data Annotation

A shortcut that bundles `@Getter`, `@Setter`, `@ToString`, `@EqualsAndHashCode`, and `@RequiredArgsConstructor.Ideal` for simple data carrier classes (DTOs).

    ```java
    @ToString(exclude = "answer")
    @EqualsAndHashCode(exclude = {"puzzleId", "difficulty", "category"})
    @Data
    public class Puzzle {
        private final Integer puzzleId;
        private final String question;
        private final String difficulty;
        private final String answer;
        private final String category;
    }
    ```

## Builder Annotation

Implements the Builder pattern. Enhances readability and manageability when dealing with classes that have many fields.

    ```java
    @Data
    @Builder
    public class Puzzle {
        private final Integer puzzleId;
        private final String question;
        private final String difficulty;
        private final String answer;
        private final String category;

    }
    ```

Validate the `@Builder` annotation

    ```java
        @Test
        public void testBuilder() {
            Puzzle puzzle = Puzzle.builder()
                .puzzleId(1)
                .question("The more you take, the more you leave behind. What am I?")
                .difficulty("Easy")
                .answer("Footsteps")
                .category("riddle")
                .build();

            assertEquals("Footsteps", puzzle.getAnswer());
            assertEquals("Easy", puzzle.getDifficulty());
        }
    ```

## Logging Annotation

Lombok provides `@Slf4j` a logging annotation that generates a logger instance. This eliminates the need to manually declare and initialize logger instances.

    ```java
    @RestController
    @Slf4j
    public class GreetingController {
        @GetMapping("/greet")
        public String greet() {
            log.debug("Greeting controller triggered");
            return "Hello, TechSpark! Let's use Lombok effectively";
        }
    }
    ```

## Advanced Lombok Usage

- `@Value` : Creates immutable classes. Use for value objects where immutability is desired
- `@With` : Generates with methods for creating modified copies. Useful for immutable objects needing slight modifications
- `@Singular` : Used with @Builder to handle collection fields. Simplifies adding elements to collections in the builder pattern.
- `@Delegate` : Delegates methods to another object.

## Best Practices and Productivity Tips

- **Limit Scope of Annotations** : Be explicit with your annotations. Overusing class-level annotations can unintentionally include fields you didn’t mean to.
- **Understand Generated Code** : Use your IDE’s “Go to Declaration” or decompile features to view the code Lombok generates.
- **Be Cautious with `@Data`** : Avoid using @Data on entities managed by JPA/Hibernate to prevent issues with lazy loading and proxy objects.
- **Combine Annotations Wisely** : You can use combinations like `@Value` and `@Builder` to create immutable objects with builder patterns.
- **Consistent Use** : Establish team conventions for Lombok usage to maintain code consistency

Lombok is a powerful tool that, when used effectively, can significantly boost productivity in Spring Boot applications. By reducing boilerplate code, developers can focus on business logic and write cleaner, more maintainable code. However, it’s essential to understand the implications of using Lombok and adhere to best practices to avoid potential pitfalls.
