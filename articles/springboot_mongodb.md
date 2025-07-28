# 📘 Build a Real-Time Use Case with MongoDB and Spring Boot

In this guide, we’ll walk through building a real-time portfolio tracker using Spring Boot and MongoDB. You’ll learn how to build a clean CRUD backend that lets you record, update, and retrieve investment transactions.

## 💡 Why MongoDB?

MongoDB is flexible, fast to iterate with, and makes it easy to model loosely structured data like financial transactions. Benefits:

- ✅ Dynamic schema (no need to predefine every field)
- ✅ Easy to scale as your app grows
- ✅ Works well with REST APIs and JSON payloads

## Setting Up a MongoDB Instance for Development

Depending on your setup and preferences, here are a few ways to run MongoDB locally for experimentation:

### 🐳 Option 1: Docker (Recommended)

If you don’t want to install MongoDB directly on your machine, you can run it via Docker:

```bash
  docker run -d \
    --name mongo \
    -p 27017:27017 \
    -e MONGO_INITDB_DATABASE=dev-db \
    mongo:latest
```

This spins up a MongoDB container accessible at `mongodb://localhost:27017/dev-db`

You can stop it with:

```bash
docker stop mongo && docker rm mongo
```

*📌 Note:* Make sure you have Docker installed and running on your machine to use this option. We’ll be publishing a separate user guide soon to help you get Docker set up if you haven’t already.

### 🖥️ Option 2: Install MongoDB Locally

You can [download MongoDB](https://www.mongodb.com/try/download/community) and run it as a local service.

### 🧪 Option 3: Use Embedded MongoDB (Flapdoodle)

Ideal for tests or lightweight dev runs.

- Add the below libraries to `build.gradle`

```groovy
implementation 'de.flapdoodle.embed:de.flapdoodle.embed.mongo:4.6.3'
```

- 🛠️ application-dev.yml

To run the application without a real MongoDB in development, you can create a dev profile using embedded MongoDB:

```groovy
spring:
  data:
    mongodb:
      uri: mongodb://localhost:27017/dev-db
```

- ▶️ Running with the dev profile

Use the following command to activate the dev profile:

```groovy
./gradlew bootRun --args='--spring.profiles.active=dev'
```

## 🖥️ Visualize MongoDB with a GUI Client Tool

Before diving into development, it's often helpful to visualize your MongoDB data using a GUI client. One of the most popular tools is **MongoDB Compass**, the official GUI from MongoDB.

- [Download MongoDB Compass](https://www.mongodb.com/try/download/compass)
- After installing, launch MongoDB Compass and connect to your MongoDB instance using the URI format:

  ```bash
    mongodb://localhost:27017/dev-db
  ```

- You can:
  - Browse databases and collections
  - Run queries interactively
  - Insert and update documents
  - View schema summaries and indexes


## 🚀 Our Stack & Template

We’ll be using our [Spring Boot starter template](https://github.com/TechSparkWorkspace/tspark-springboot-starter-template) with below features.

- Spring Boot Web, Validation, JPA, and Actuator
- SpringDoc OpenAPI with Swagger UI
- Lombok for reducing boilerplate code
- MapStruct for DTO-to-Entity conversion
- Stock Portfolio CRUD with Service, Repo, Controller
- Global Exception Handling with @ControllerAdvice
- JUnit 5 & Mockito for unit testing
- Feature-based folder structure

You have two ways to get started with this project, based on how you'd like to experiment:

### ✨ Option 1: Create Your Own Repo from the Template

- Head over to our [Spring Boot starter template on GitHub]((https://github.com/TechSparkWorkspace/tspark-springboot-starter-template)).
- Click the `Use this template` button at the top right of the repository.
- Give your new repository a name, and choose visibility (public/private).
- Clone your repo locally:

```bash
git clone git@github.com:TechSparkWorkspace/tspark-springboot-mongodb.git
cd tspark-springboot-mongodb
```

- Open the project in IntelliJ or your preferred IDE, and run:

```bash
./gradlew bootRun
```

### 🛠 Option 2: Clone Template Repo Directly and Experiment

- Clone the template repo directly:

```bash
git clone https://github.com/your-org/spring-boot-starter-template.git
cd spring-boot-starter-template
```

- Modify the code freely without affecting the original template.
- Run the project and start experimenting.

### 🔧 Minor Customizations After Cloning

Before jumping into code, make these minor adjustments:

- Update the project name in `settings.gradle`:

    ```rootProject.name = 'your-repo-name'```

- Refactor the base package if needed to match your domain
- Update README.md to reflect your project purpose
- Update the Swagger configuration class (usually found under configuration package) to use the correct project title and description.
- Remove the `spring-boot-starter-data-jpa` and `h2` libraries from `build.gradle` file
- Delete all files under `portfolio` package to start fresh

These small tweaks will personalize the setup and prevent confusion later.

## 🧱 What We'll Build

Before diving into the code, here’s a quick summary of the components we’ll put together:

- **Domain Model:** `PortfolioTransaction` to represent investment entries like stock buys/sells
- **Repository Layer:** Using Spring Data MongoDB to persist transactions
- **Service Layer:** Business logic to manage transactions (CRUD)
- **Controller Layer:** REST APIs to expose portfolio endpoints
- **DTOs and Mappers:** Clean separation of data model from API contracts using MapStruct
- **Swagger UI:** To test and explore the APIs

This structure keeps the project clean, testable, and ready to scale.

### 📦 Domain: Portfolio Transaction

```java
@Document(collection = "portfolio")
@Data
@Builder
@NoArgsConstructor
@AllArgsConstructor
public class PortfolioTransaction {

    @Id
    private String id;

    @NotBlank
    private String symbol;

    @NotNull
    private TransactionType type; // BUY or SELL

    @NotNull
    private Double quantity;

    @NotNull
    private Double price;

    @NotNull
    private LocalDate date;

}
```

### Transaction Type

```java
public enum TransactionType {
    BUY,
    SELL
}
```

### 📚 Repository Layer

```java
public interface PortfolioRepository extends MongoRepository<PortfolioTransaction, String> {
    List<PortfolioTransaction> findBySymbol(String symbol);
}
```

### 🧩 Service Layer

```java
public interface PortfolioService {
    PortfolioTransactionDTO create(PortfolioTransactionDTO dto);
    List<PortfolioTransactionDTO> getAll();
    List<PortfolioTransactionDTO> getBySymbol(String symbol);
    PortfolioTransactionDTO update(String id, PortfolioTransactionDTO dto);
    void delete(String id);
}
```

```java
@Service
@RequiredArgsConstructor
public class PortfolioServiceImpl implements PortfolioService {

    private final PortfolioRepository repository;
    private final PortfolioTransactionMapper mapper;

    @Override
    public PortfolioTransactionDTO create(PortfolioTransactionDTO dto) {
        PortfolioTransaction entity = mapper.toEntity(dto);
        return mapper.toDto(repository.save(entity));    }

    @Override
    public List<PortfolioTransactionDTO> getAll() {
        return mapper.toDtoList(repository.findAll());
    }

    @Override
    public List<PortfolioTransactionDTO> getBySymbol(String symbol) {
        return mapper.toDtoList(repository.findBySymbol(symbol));
    }

    @Override
    public PortfolioTransactionDTO update(String id, PortfolioTransactionDTO dto) {
        PortfolioTransaction existing = repository.findById(id)
                .orElseThrow(() -> new RuntimeException("Transaction not found"));

        existing.setSymbol(dto.getSymbol());
        existing.setType(dto.getType());
        existing.setQuantity(dto.getQuantity());
        existing.setPrice(dto.getPrice());
        existing.setDate(dto.getDate());

        return mapper.toDto(repository.save(existing));
    }

    @Override
    public void delete(String id) {
        repository.deleteById(id);
    }
}
```

### 🎯 Controller Layer

```java
@RestController
@RequestMapping("/api/portfolio")
@RequiredArgsConstructor
@Tag(name = "Portfolio Management", description = "APIs for managing portfolio transactions")
public class PortfolioController {

    private final PortfolioService portfolioService;

    @Operation(
            summary = "Create a new portfolio transaction",
            description = "Adds a new portfolio transaction to the system.",
            responses = {
                    @ApiResponse(responseCode = "200", description = "Record created successfully",
                            content = @Content(schema = @Schema(implementation = PortfolioTransaction.class))),
                    @ApiResponse(responseCode = "400", description = "Validation error")
            }
    )
    @PostMapping
    public ResponseEntity<PortfolioTransactionDTO> create(@Valid @RequestBody PortfolioTransactionDTO dto) {
        return ResponseEntity.ok(portfolioService.create(dto));
    }

    @Operation(
            summary = "Get all portfolio transactions",
            description = "Retrieves all portfolio transactions from the system.",
            responses = {
                    @ApiResponse(responseCode = "200", description = "List of portfolio transactions retrieved successfully",
                            content = @Content(schema = @Schema(implementation = PortfolioTransactionDTO.class)))
            }
    )
    @GetMapping
    public ResponseEntity<List<PortfolioTransactionDTO>> getAll() {
        return ResponseEntity.ok(portfolioService.getAll());
    }

    @Operation(
            summary = "Get portfolio transactions by symbol",
            description = "Retrieves all portfolio transactions for a specific stock symbol.",
            responses = {
                    @ApiResponse(responseCode = "200", description = "List of portfolio transactions for the symbol retrieved successfully",
                            content = @Content(schema = @Schema(implementation = PortfolioTransactionDTO.class))),
                    @ApiResponse(responseCode = "404", description = "No transactions found for the given symbol")
            }
    )
    @GetMapping("/{symbol}")
    public ResponseEntity<List<PortfolioTransactionDTO>> getBySymbol(@PathVariable String symbol) {
        return ResponseEntity.ok(portfolioService.getBySymbol(symbol));
    }

    @Operation(
            summary = "Update a portfolio transaction",
            description = "Updates an existing portfolio transaction by ID.",
            responses = {
                    @ApiResponse(responseCode = "200", description = "Portfolio transaction updated successfully",
                            content = @Content(schema = @Schema(implementation = PortfolioTransactionDTO.class))),
                    @ApiResponse(responseCode = "404", description = "Transaction not found")
            }
    )
    @PutMapping("/{id}")
    public ResponseEntity<PortfolioTransactionDTO> update(@PathVariable String id, @RequestBody PortfolioTransactionDTO dto) {
        return ResponseEntity.ok(portfolioService.update(id, dto));
    }

    @Operation(
            summary = "Delete a portfolio transaction",
            description = "Deletes a portfolio transaction by ID.",
            responses = {
                    @ApiResponse(responseCode = "204", description = "Transaction deleted successfully"),
                    @ApiResponse(responseCode = "404", description = "Transaction not found")
            }
    )
    @DeleteMapping("/{id}")
    public ResponseEntity<Void> delete(@PathVariable String id) {
        portfolioService.delete(id);
        return ResponseEntity.noContent().build();
    }
}
```

## 🔗 Access the Complete Source Code

If you’d rather explore a working version or got stuck along the way, check out the [full implementation on GitHub](https://github.com/TechSparkWorkspace/tspark-springboot-mongodb)

## 📡 Consuming the API Endpoints

Here’s how you can interact with your API endpoints:

### ➕ Create Transaction

```bash
POST /api/portfolio
Content-Type: application/json

{
  "symbol": "AAPL",
  "type": "BUY",
  "quantity": 10,
  "price": 195.25,
  "date": "2025-07-21"
}
```

✅ Expected Response: 200 OK + JSON body of created transaction

### 📥 Get All Transactions

```GET /api/portfolio```

✅ Expected Response: 200 OK + List of all transactions

### 🔍 Get Transactions by Symbol

```GET /api/portfolio/AAPL```

✅ Expected Response: 200 OK + List of AAPL transactions

### 🔄 Update Transaction

```bash
PUT /api/portfolio/{id}
Content-Type: application/json

{
  "symbol": "AAPL",
  "type": "BUY",
  "quantity": 15,
  "price": 200.0,
  "date": "2025-07-22"
}
```
✅ Expected Response: 200 OK + Updated transaction

### ❌ Delete Transaction

```DELETE /api/portfolio/{id}```

✅ Expected Response: 204 No Content
