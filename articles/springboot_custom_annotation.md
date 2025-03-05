# Define Custom Annotations in Spring Boot

Annotations play a crucial role in Java and Spring Boot applications, simplifying configurations, reducing boilerplate code, and improving code readability. In Spring Boot, annotations are heavily used to define beans, handle dependency injection, manage transactions, and configure various aspects of the application effortlessly.

## Introduction

### What Are Annotations and Why Are They Useful?

Annotations in Java are metadata markers that provide additional information about the code without altering its functionality. They help frameworks like Spring Boot process and configure classes, methods, and fields dynamically. Common examples include `@Component`, `@Service`, `@RestController`, and `@Autowired`, which make it easier to develop Spring Boot applications with minimal XML configurations.

Using annotations, developers can:

- ✅ Define configurations concisely.
- ✅ Enable declarative programming, reducing the need for manual implementations.
- ✅ Improve code clarity and maintainability.

### When Are Custom Annotations Needed?

While Java and Spring Boot offer many built-in annotations, there are cases where you may need to define your own:

- 🔹 Validation: When built-in constraints (like `@NotNull`, `@Size`) don't cover specific business rules. Example: `@ValidAge` to check if an age falls within a given range.
- 🔹 Metadata Tagging: To add custom behavior or labels to classes and methods for processing later. Example: `@TrackUsage` to log method calls.
- 🔹 Code Enforcement: Ensuring consistent practices across teams, such as marking deprecated or experimental features. Example: `@BetaFeature`.

### What You Will Learn in This Article

In this guide, you'll learn:

- ✅ How to define a **custom annotation** in Java.
- ✅ How to use **meta-annotations** like `@Target` and `@Retention`.
- ✅ How to apply custom annotations effectively in a Spring Boot application.

By the end of this article, you'll be able to create **your own custom annotations** to enhance your Spring Boot projects with cleaner, reusable, and more maintainable code. 🚀

## Understanding Java Annotations

Annotations can be applied to classes, methods, fields, parameters, and other elements. They can be retained in the **source code, compiled class files, or available at runtime** depending on their configuration.

### Meta-Annotations in Java

**Meta-annotations** are annotations that are used to define or modify the behavior of other annotations. Essentially, they are **annotations for annotations** and help specify how an annotation should be processed by the compiler, runtime, or documentation tools.

1️⃣ **@Target - Specifies where an annotation can be applied**

This restricts an annotation's usage to specific elements like **methods, fields, or classes.**

```java
import java.lang.annotation.ElementType;
import java.lang.annotation.Target;

@Target(ElementType.METHOD) // Can only be applied to methods
public @interface CustomAnnotation {
}
```

_Common values:_

- `ElementType.TYPE` → Class, interface, or enum
- `ElementType.METHOD` → Methods
- `ElementType.FIELD` → Fields

2️⃣ **@Retention - Defines how long an annotation is retained**

It determines whether an annotation is available at **source, class, or runtime.**

```java
import java.lang.annotation.Retention;
import java.lang.annotation.RetentionPolicy;

@Retention(RetentionPolicy.RUNTIME) // Available at runtime for reflection
public @interface CustomAnnotation {
}
```

_Common values:_

- `RetentionPolicy.SOURCE` → Available only in source code, removed at compile time
- `RetentionPolicy.CLASS` → Retained in the class file but not at runtime
- `RetentionPolicy.RUNTIME` → Available at runtime via reflection (commonly used in frameworks)

3️⃣ **@Documented - Includes annotation in JavaDocs**

This ensures that the annotation appears in generated documentation.

```java
import java.lang.annotation.Documented;

@Documented
public @interface CustomAnnotation {
}
```

4️⃣ **@Inherited - Allows subclasses to inherit annotations**

By default, annotations are **not** inherited. Using `@Inherited`, a subclass automatically gets the annotation from its parent.

```java
import java.lang.annotation.Inherited;

@Inherited
public @interface CustomAnnotation {
}
```

## Creating Custom Annotations in Spring Boot

Today, we'll create **three types of custom annotations**, all aligned with **stock market use cases:**

### Basic Class-Level Annotation (@StockMetadata)

A class-level annotation to store metadata about stock-related classes.

```java
import java.lang.annotation.*;

@Target(ElementType.TYPE)  // Applies to classes
@Retention(RetentionPolicy.RUNTIME) // Available at runtime
@Documented
public @interface StockMetadata {
    String sector();   // Defines the stock sector (e.g., Tech, Finance)
    String exchange(); // Specifies the stock exchange (e.g., NYSE, NASDAQ)
}
```

_Usage:_

```java
@StockMetadata(sector = "Technology", exchange = "NASDAQ")
public class AppleStock {
}
```

📌 **Purpose:** This annotation can be used to **categorize stocks dynamically** in applications.

### Method Annotation with Parameters (@TrackStockPriceChange)

A method-level annotation to **track stock price updates.**

```java
import java.lang.annotation.*;

@Target(ElementType.METHOD) // Applies to methods
@Retention(RetentionPolicy.RUNTIME) // Available at runtime
@Documented
public @interface TrackStockPriceChange {
    String value() default "Tracking stock price change";
}
```

_Usage:_

```java
public class StockService {
    @TrackStockPriceChange
    public void updateStockPrice(String symbol, double newPrice) {
        System.out.println(symbol + " price updated to $" + newPrice);
    }
}
```

📌 **Purpose:** Can be used with **logging, event tracking or alerts** for price changes.

### Field Validation Annotation (@ValidStockSymbol)

A field-level annotation to **validate stock symbols**, ensuring they follow a correct format.

```java
import jakarta.validation.Constraint;
import jakarta.validation.Payload;
import java.lang.annotation.*;

@Target(ElementType.FIELD)  // Can only be applied to fields
@Retention(RetentionPolicy.RUNTIME) // Available at runtime for validation
@Constraint(validatedBy = StockSymbolValidator.class) // Links to the validator logic
@Documented
public @interface ValidStockSymbol {
    String message() default "Invalid stock symbol";  // Default error message
    Class<?>[] groups() default {};  // Required for validation grouping
    Class<? extends Payload>[] payload() default {}; // Used for advanced validation metadata
}
```

📌 **Purpose:** Ensures stock symbols are **valid, standardized, and prevent incorrect user inputs.**

This class contains the logic for validating stock symbols.

```java
import jakarta.validation.ConstraintValidator;
import jakarta.validation.ConstraintValidatorContext;
import java.util.regex.Pattern;

public class StockSymbolValidator implements ConstraintValidator<ValidStockSymbol, String> {
    private static final Pattern SYMBOL_PATTERN = Pattern.compile("^[A-Z]{1,5}$"); // Standard stock symbols

    @Override
    public boolean isValid(String value, ConstraintValidatorContext context) {
        return value != null && SYMBOL_PATTERN.matcher(value).matches();
    }
}
```

The **Regex Pattern `(^[A-Z]{1,5}$)`** in the validator ensures stock symbols are **1 to 5 uppercase letters** (common in stock markets).

Now, let's apply the annotation in a **StockRequest** DTO.

```java
import jakarta.validation.Valid;
import jakarta.validation.constraints.NotNull;

public class StockRequest {
    @NotNull(message = "Stock symbol cannot be null")
    @ValidStockSymbol  // Custom annotation applied here
    private String symbol;

    public StockRequest(String symbol) {
        this.symbol = symbol;
    }

    public String getSymbol() {
        return symbol;
    }

    public void setSymbol(String symbol) {
        this.symbol = symbol;
    }
}
```

Spring Boot's **`@Valid` annotation** ensures the validation is applied when handling API requests.

```java
import jakarta.validation.Valid;
import org.springframework.http.ResponseEntity;
import org.springframework.web.bind.annotation.*;

@RestController
@RequestMapping("/stocks")
public class StockController {

    @PostMapping("/validate")
    public ResponseEntity<String> validateStock(@Valid @RequestBody StockRequest stockRequest) {
        return ResponseEntity.ok("Valid stock symbol: " + stockRequest.getSymbol());
    }
}
```

🔹 If you're new to **Spring Boot validation**, check out my detailed guide on applying [validation for request parameters in Spring Boot](https://medium.com/fullstack-launchpad-novice-edition/validating-requests-in-spring-boot-controllers-42ca0315c81c)

## Best Practices for Creating Custom Annotations

When designing custom annotations in Spring Boot, it's important to follow best practices to ensure they are **efficient, maintainable, and easy to use.**

Here are some key guidelines:

- **✅ Keep It Simple** - Custom annotations should be concise and solve a specific problem without unnecessary complexity.
- **✅ Use Meta-Annotations Wisely** - Always specify appropriate `@Target`, `@Retention`, and `@Documented` properties based on the annotation's purpose.
- **✅ Ensure Runtime Availability When Needed** - If your annotation needs to be accessed dynamically (e.g., for validation or logging), set `@Retention(RetentionPolicy.RUNTIME)`.
- **✅ Combine with Existing Annotations** - Avoid reinventing the wheel; use built-in annotations (`@Valid`, `@Constraint`) whenever possible.
- **✅ Test Your Annotations** - Validate that they work correctly in real-world scenarios before deploying them in production.
- **✅ Document Your Annotations** - Use `@Documented` and provide clear JavaDocs so that team members understand their usage.

In this article, we explored **how to create and use custom annotations in Spring Boot**, focusing on:

- **✔ Basic annotations for metadata (@StockMetadata)**
- **✔ Method annotations with parameters (@TrackStockPriceChange)**
- **✔ Field validation annotations (@ValidStockSymbol)**

These annotations enhance code **readability, reusability, and maintainability,** making applications cleaner and more structured.

👉 **GitHub Repo:** [Your GitHub Repository Link](https://github.com/TechSparkWorkspace/tspark-springboot-custom-annotation)
