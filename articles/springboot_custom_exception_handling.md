# Exception Handling in Spring Boot

Exception handling is a crucial part of any application. In a Spring Boot application, properly managing exceptions ensures better debugging, improved user experience, and structured API responses. Poorly handled exceptions can lead to unpredictable behavior, security vulnerabilities, and a negative impact on application reliability.

This guide covers the best practices for handling exceptions in Spring Boot, making it both effective and clean. We will explore different techniques, from basic to advanced, ensuring robust exception management.

## Understanding Exception Handling in Java

Before diving into Spring Boot, let’s review the two types of exceptions in Java:

- **Checked Exceptions**: These exceptions must be handled explicitly using `try-catch` or declared using `throws`. Examples: `IOException`, `SQLException`
- **Unchecked Exceptions**: These extend `RuntimeException` and do not require explicit handling. Examples: `NullPointerException`, `IllegalArgumentException`

Spring Boot primarily deals with **unchecked exceptions**, as most exceptions in web applications fall into this category.

## Default Exception Handling in Spring Boot

Spring Boot provides default error handling using `BasicErrorController`. When an exception occurs, it returns a generic error response with:

- **HTTP Status Code** (e.g., 500, 404, 400)
- **Error Message**
- **Timestamp**
- **Path of the request**

📦 **Sample Validation Failure Response**

```bash
{
  "timestamp": "2025-03-22T19:53:55.394+00:00",
  "status": 400,
  "error": "Bad Request",
  "path": "/api/portfolio"
}
```

However, relying on this default behavior is not recommended as it lacks meaningful responses tailored to our business logic. Let's explore clean and effective ways to handle exceptions in Spring Boot.

## The Clean and Effective Way to Handle Exceptions

### Error Code Enum with HTTP Status Mapping

The `ErrorCode` enum is a central place to define all application-level error identifiers. Each error code is associated with:

- A unique string code (1001, 2001, etc.) for easier tracking and debugging.
- A human-readable description to convey the error meaning.
- An appropriate HTTP status code to standardize REST responses.

Use clearly defined code ranges for better organization and easier debugging:

- 1001–1999: Validation Errors
- 2001–2999: Storage Errors (e.g., database or file I/O)
- 3001–3999: Network Errors (e.g., API timeouts)
- 4001–4999: Security Errors (e.g., authentication/authorization failures)

Group your error codes by categories such as:

- VALIDATION_ERROR: Input or request validation failures.
- STORAGE_ERROR: Database or persistence issues.
- NETWORK_ERROR: Issues communicating with external systems.
- SECURITY_ERROR: Unauthorized access or security policy violations.

```java
@Getter
public enum ErrorCode {

    VALIDATION_ERROR("1001", "Validation failed", HttpStatus.BAD_REQUEST),
    STORAGE_ERROR("2001", "Storage operation failed", HttpStatus.INTERNAL_SERVER_ERROR),
    STORAGE_ERROR_NOTFOUND("2002", "Entoty not found", HttpStatus.INTERNAL_SERVER_ERROR),
    NETWORK_ERROR("3001", "Network issue encountered", HttpStatus.SERVICE_UNAVAILABLE),
    SECURITY_ERROR("4001", "Security violation", HttpStatus.UNAUTHORIZED),
    SYSTEM_ERROR("5001", "System error", HttpStatus.INTERNAL_SERVER_ERROR);

    private final String code;
    private final String description;
    private final HttpStatus httpStatus;

    ErrorCode(String code, String description, HttpStatus httpStatus) {
        this.code = code;
        this.description = description;
        this.httpStatus = httpStatus;
    }
}
```

### Defining a Generic ApplicationException

To standardize error handling across the application, we define a custom exception named `ApplicationException` that includes an error code and a meaningful message. By adding a constructor that accepts a `Throwable cause`, we ensure that the **stack trace is preserved**.


```java
public class ApplicationException extends RuntimeException {
    private final ErrorCode errorCode;

    public ApplicationException(ErrorCode errorCode, String message) {
        super(message);
        this.errorCode = errorCode;
    }

    public ApplicationException(ErrorCode errorCode, String message, Throwable cause) {
        super(message, cause);
        this.errorCode = errorCode;
    }

    public ErrorCode getErrorCode() {
        return errorCode;
    }
}
```

### Creating an Exception Utility Class

To enforce consistency and reduce code duplication, we introduce an **Exception Utility**. We deliberately avoid logging in this utility class to prevent duplicate logs and keep logging responsibility centralized in the global exception handler. **Services can pass the original exception**, ensuring the stack trace remains intact while avoiding duplicate logs.

```java
@Component
public class ExceptionUtil {
    public void throwException(ErrorCode errorCode, String message) {
        throw new ApplicationException(errorCode, message);
    }

    public void throwException(ErrorCode errorCode) {
        throw new ApplicationException(errorCode, errorCode.getDescription());
    }

    public void throwException(ErrorCode errorCode, String message, Throwable cause) {
        throw new ApplicationException(errorCode, message, cause);
    }
}
```

### Ensuring Services Always Throw `ApplicationException`

All services should use `ExceptionUtil` to throw exceptions. By always throwing a single, consistent `ApplicationException` from service methods, we simplify the exception handling flow, making it predictable and easier to debug.

```java
@Service
@RequiredArgsConstructor
public class PortfolioService {

    private final PortfolioRepository portfolioRepository;
    private final StockMapper stockMapper = StockMapper.INSTANCE;
    private final ExceptionUtil exceptionUtil;

    public StockDTO createStock(StockDTO stockDTO) {
        try {
            Stock stock = stockMapper.toStockEntity(stockDTO);
            Stock savedStock = portfolioRepository.save(stock);
            return stockMapper.toStockDTO(savedStock);
        } catch (Exception ex) {
            exceptionUtil.throwException(ErrorCode.STORAGE_ERROR, "Database error while saving user", ex);
        }
        return null;
    }

    public List<StockDTO> getAllStocks() {
        return portfolioRepository.findAll()
                .stream()
                .map(stockMapper::toStockDTO)
                .collect(Collectors.toList());
    }

    public StockDTO getStockById(Long id) {
        Stock stock = portfolioRepository.findById(id)
                .orElseThrow(exceptionUtil.exception(STORAGE_ERROR_NOTFOUND, "Stock not found with id :" + id));
        return stockMapper.toStockDTO(stock);
    }

    public StockDTO updateStock(Long id, StockDTO stockDTO) {
        Stock existingStock = portfolioRepository.findById(id)
                .orElseThrow(exceptionUtil.exception(STORAGE_ERROR_NOTFOUND, "Stock not found with id :" + id));

        existingStock.setName(stockDTO.getName());
        existingStock.setTickerSymbol(stockDTO.getTickerSymbol());
        existingStock.setQuantity(stockDTO.getQuantity());

        try {
            return stockMapper.toStockDTO(portfolioRepository.save(existingStock));
        } catch (Exception ex) {
            exceptionUtil.throwException(ErrorCode.STORAGE_ERROR, "Database error while saving user", ex);
        }
        return null;
    }

    public void deleteStock(Long id) {
        if (!portfolioRepository.existsById(id)) {
            exceptionUtil.throwException(STORAGE_ERROR_NOTFOUND, "Stock not found with id :" + id);
        }
        portfolioRepository.deleteById(id);
    }
}
```

### Standardized Error Response Format

To provide a **consistent API response**, we define an `ErrorResponse` DTO.

```java
public class ErrorResponse {
    private String errorCode;
    private String message;
    private LocalDateTime timestamp;

    public ErrorResponse(String errorCode, String message) {
        this.errorCode = errorCode;
        this.message = message;
        this.timestamp = LocalDateTime.now();
    }

    // Getters & Setters
}
```

### Handling Exceptions in `@ControllerAdvice` (With Logging)

The global exception handler ensures a uniform response format and logs exceptions.

```java
@Slf4j
@RestControllerAdvice
public class GlobalExceptionHandler {
    @ExceptionHandler(ApplicationException.class)
    public ResponseEntity<ErrorResponse> handleApplicationException(ApplicationException ex) {
        ErrorCode errorCode = ex.getErrorCode();
        
        // Log the exception stack trace here
        log.error("Handled Exception: [{}] - {}", errorCode.getCode(), ex.getMessage(), ex);
        
        ErrorResponse errorResponse = new ErrorResponse(errorCode.getCode(), ex.getMessage());
        return ResponseEntity.status(errorCode.getHttpStatus()).body(errorResponse);
    }
}
```

This ensures that **all exceptions are logged centrally,** eliminating duplicate logs from services.

## 🔎 Sample Exception Responses

Here are a few examples of structured error responses handled by our system:

### `MethodArgumentNotValidException`

```bash
{
  "errorCode": "1001",
  "message": "Validation failed",
  "timestamp": "2025-03-24T08:33:30.050217",
  "details": [
    "Quantity must be at least 1",
    "Ticker symbol is required"
  ]
}
```

### `ApplicationException`

```bash
{
  "errorCode": "2002",
  "message": "Stock not found with id :22",
  "timestamp": "2025-03-24T08:35:22.442719",
  "details": null
}
```

## Conclusion

By following these best practices, we can ensure:

- A single, standardized exception handling mechanism (ApplicationException).
- A reusable exception utility (``) to reduce redundancy.
- Meaningful error codes categorized for different scenarios.
- Consistent API responses with ErrorResponse.
- Automatic HTTP status mapping based on error types.
- Stack traces are preserved for better debugging.
- Centralized logging only in the Global Exception Handler, avoiding duplicate logs.

You can explore the complete working examples in our [GitHub repository](https://github.com/TechSparkWorkspace/tspark-springboot-exception-handling) ⭐️
