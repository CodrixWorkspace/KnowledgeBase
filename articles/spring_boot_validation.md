# Guide to Request Validation in Spring Boot Controllers

This comprehensive guide will walk you through the process of implementing validations in Spring Boot Controllers.

## Overview

Request parameter validation is a vital component in building robust and secure APIs. Here's why it's indispensable:

- **Data Integrity Assurance**: Validation ensures that the data received by your API complies with the expected formats, lengths, or patterns, preventing invalid or malformed data from causing errors within your application. 🛡️
- **API Security Enhancement**: Proper validation helps mitigate security vulnerabilities, such as injection attacks, by ensuring inputs are sanitized and restricted to acceptable values. 🔒
- **User Experience Improvement**: By validating and providing clear error messages, developers and end-users gain insight into why a request failed, leading to faster debugging and smoother interactions. 🚀

By integrating validation for GET, POST, and PUT requests, you can create a robust API that not only safeguards your application but also fosters trust and reliability for its users.

## Project Setup

✅ If you haven’t yet created a Spring Boot project and want to start from scratch with Gradle, check out our **step-by-step guide** here:
[👉 How to Create a Spring Boot Project with Gradle (Beginner Friendly)](https://skillhunt.codrixtech.com/guide/view?src=spring_boot_gradle.md&id=10e96ec0-f43c-4cea-bd7f-ca3412fc268f)

This guide helps you scaffold a Spring Boot application using the **command line and IntelliJ IDEA**, so you’re all set before jumping into validation. 🛠️

If you’re short on time and want to dive straight into the practical implementation, you can refer to my pre-configured GitHub [Spring Boot Request Validation Guide repository](https://github.com/TechSparkWorkspace/tspark-springboot-validation). You can clone it, run the application, and experiment with validation techniques right away. 📚

## How to Set Up Validation in a Spring Boot Project

To set up validation, add the following dependency to your `build.gradle` file:

```groovy
dependencies {
    implementation 'org.springframework.boot:spring-boot-starter-validation'
}
```

Spring Boot automatically detects the `spring-boot-starter-validation` dependency and integrates it. No additional configuration is required to enable validation. 🎉

## Implementing Validation for GET Request

To validate a `GET` request:

- Add `@Validated` to the Class or Method.
- Add **validation constraints to the request params**. Refer to the [API documentation](https://jakarta.ee/specifications/bean-validation/3.0/apidocs/jakarta/validation/constraints/package-summary) to learn about the available constraints.

Here's an example:

```java
@RestController
@Validated
public class WishListController {

    @GetMapping("/api/wish")
    public String getWishes(
            @RequestParam @NotBlank(message = "Category must not be blank") String category) {
        return String.format("Fetching wishes in category: %s", category);
    }
}
```

Spring Boot validation errors are mapped to **`ConstraintViolationException`**. If no exception handler is defined, validation might not work properly. Add a **global exception handler** to handle validation errors:

```java
@ControllerAdvice
public class CustomExceptionHandler {

    @ExceptionHandler(ConstraintViolationException.class)
    public ResponseEntity<Map<String, String>> handleValidationExceptions(ConstraintViolationException ex) {
        Map<String, String> errors = new HashMap<>();
        ex.getConstraintViolations().forEach(violation -> {
            errors.put(violation.getPropertyPath().toString(), violation.getMessage());
        });
        return new ResponseEntity<>(errors, HttpStatus.BAD_REQUEST);
    }
}
```

Now lets query our endpoints with curl

```bash
 curl "http://localhost:8080/api/wish"   

 # Output :  {"timestamp":"2024-12-05T22:57:34.384+00:00","status":400,"error":"Bad Request","path":"/api/wish"}

curl "http://localhost:8080/api/wish?category="

 # Output : {"getWishes.category":"Category must not be blank"}

curl "http://localhost:8080/api/wish?category=activity"

 # Output : Fetching wishes in category: activity   

```

## Implementing Validation for GET Request with Path Param

When handling path parameters in a `GET` request, you can validate them using `@PathVariable` in combination with **validation annotations** like `@NotBlank`, `@Size`, or `@Pattern`.

Here's an example:

```java
@RestController
@Validated
public class WishListController {

    @GetMapping("/api/wish/{year}")
    public String getWishesByTargetYear(
            @PathVariable @NotBlank(message = "Year must not be blank")
            @Size(min = 2, max = 4, message = "Year must be between 2 and 4 characters")
            String year) {
        return String.format("Fetching wishes for year : %s", year);
    }

}

```

Let’s test this with various inputs using curl

```java
 curl "http://localhost:8080/api/wish/2" 

 # Output :  {"getWishesByTargetYear.year":"Year must be between 2 and 4 characters "}

curl "http://localhost:8080/api/wish/20245"

 # Output :  {"getWishesByTargetYear.year":"Year must be between 2 and 4 characters "}

curl "http://localhost:8080/api/wish/2024"

 # Output : Fetching wishes for year : 2024

```

## Implementing Validation for POST Request

In a `POST` request, validation is typically **applied to the request body**. We achieve this by using a **Data Transfer Object (DTO) and annotations** from the validation API.

Create the below DTO Object with the required constraints:

```java
@Data
@NoArgsConstructor
@AllArgsConstructor
public class WishRequest {

    @NotBlank(message = "Title must not be blank")
    @Size(min = 10, max = 50, message = "Title must be between 10 and 50 characters")
    private String title;

    @NotBlank(message = "Category must not be blank")
    private String category;

    @Min(value = 2000, message = "Year should be minimum 2000")
    @Max(value = 2024, message = "Year can be maximum 2024")
    private Long year;
}
```

Add the `@Validated` attribute to the post method

```java
@RestController
@Validated
public class WishListController {

    @PostMapping("/api/wish")
    public String createWish(@RequestBody @Validated WishRequest wishRequest) {
        return String.format("Wish created: %s, Category: %s", wishRequest.getTitle(), wishRequest.getCategory());
    }

}
```

Add the below **`ExceptionHandler`** to customize validation error responses:

```java
@ExceptionHandler(MethodArgumentNotValidException.class)
public ResponseEntity<Map<String, String>> handleValidationExceptions(MethodArgumentNotValidException ex) {
    Map<String, String> errors = new HashMap<>();
    for (FieldError error : ex.getBindingResult().getFieldErrors()) {
        errors.put(error.getField(), error.getDefaultMessage());
    }
    return new ResponseEntity<>(errors, HttpStatus.BAD_REQUEST);
}

```

Let’s validate the post method by executing curl requests

```bash
curl -X POST "http://localhost:8080/api/wish" \
        -H "Content-Type: application/json" \
        -d '{}'

 # Output - {"category":"Category must not be blank","title":"Title must not be blank"}


curl -X POST "http://localhost:8080/api/wish" \
      -H "Content-Type: application/json" \
      -d '{"title" : "Learn to play Ukelele", "category": "learning", "year" : 1990}'

# Output -  {"year":"Year should be minimum 2000"}


curl -X POST "http://localhost:8080/api/wish" \
      -H "Content-Type: application/json" \
      -d '{"title" : "Learn to play Ukelele", "category": "learning", "year" : 2005}'


# Output - Wish created: Learn to play Ukelele, Category: learning
```

As you can see in the tests above, error messages are returned when the input request does not comply with the rules defined in the object.

The validation for `PUT` and `POST` methods in Spring Boot is fundamentally the same because both typically involve validating the request body using a Data Transfer Object (DTO). However, there are a few key differences in their use cases and the associated validation strategies:

**POST**: Typically used for creating new resources. All fields are often required, and **validation ensures the completeness of the data**.

**PUT**: Typically used for updating existing resources. **Some fields might be optional**, especially if you are performing a partial update or allowing users to modify only specific attributes.

In this guide, we explored how to implement request parameter validation in Spring Boot for `GET`, `POST`, and `PUT` methods. By applying proper validation techniques, you can ensure the **integrity of your APIs, improve security, and provide a better experience** for users interacting with your services. 🎯

Validation is a critical aspect of **building reliable applications**, and with the examples shared here, you now have a solid foundation to enhance your APIs. 💪
