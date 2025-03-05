# Define Custom Annotations with Aspect in Spring Boot

Custom annotations in Spring Boot provide a clean, declarative way to add metadata to methods, but they don’t have built-in behavior. By leveraging Aspect-Oriented Programming (AOP), we can add cross-cutting concerns like logging, validation, and security without cluttering the business logic. AOP makes our code modular, reusable, and maintainable.

We recommend using our [**Spring Boot Starter Template**](https://github.com/TechSparkWorkspace/tspark-springboot-template) to quickly set up a new project with best practices.

## What Are We Building?

In this guide, we’ll enhance our **Stock Portfolio Tracking** application by implementing a custom annotation, `@TrackStockChange`, that:

- Logs stock buy, sell, and update transactions.
- Provides an easy way to monitor stock transactions without modifying business logic.

We’ll achieve this using **AOP and Pointcuts** to intercept method calls and execute additional behavior.

## Understanding AOP and Pointcuts

**Aspect-Oriented Programming (AOP)** allows us to separate cross-cutting concerns from business logic. **Aspects** contain advice (code that runs before, after, or around method execution) and use **Pointcuts** to define where the advice should be applied.

In our case, we will use a **custom annotation** as a pointcut to track stock transactions dynamically.

## Implementing the Custom Annotation and Aspect

1. Define the Custom Annotation

First, we create `@TrackStockChange`, which accepts an action type:

    ```java
    @Retention(RetentionPolicy.RUNTIME)
    @Target(ElementType.METHOD)
    public @interface TrackStockChange {
        String action(); // "CREATE", "UPDATE", "DELETE"
    }
    ```

2. Implement the Aspect

The aspect will log the execution of methods annotated with `@TrackStockChange`:

    ```java
    @Aspect
    @Component
    class StockChangeAspect {
        private static final Logger logger = LoggerFactory.getLogger(StockChangeAspect.class);

        @Pointcut("@annotation(trackStockChange)")
        public void trackStock(TrackStockChange trackStockChange) {}

        @Around("trackStock(trackStockChange)")
        public Object logStockTransaction(ProceedingJoinPoint joinPoint, TrackStockChange trackStockChange) throws Throwable {
            logger.info("[Stock Tracking] Action: {} - Executing {}", trackStockChange.action(), joinPoint.getSignature().getName());
            Object result = joinPoint.proceed();
            logger.info("[Stock Tracking] Action: {} - Successfully executed {}", trackStockChange.action(), joinPoint.getSignature().getName());
            return result;
        }
    }
    ```

3. Apply the Annotation in the Portfolio Service

Now, we can annotate the stock transaction methods in PortfolioService:

    ```java
    @Service
    public class PortfolioService {
        private final PortfolioRepository portfolioRepository;
        private final StockMapper stockMapper = StockMapper.INSTANCE;

        @TrackStockChange(action = "CREATE")
        public StockDTO createStock(StockDTO stockDTO) {
            Stock stock = stockMapper.toStockEntity(stockDTO);
            return stockMapper.toStockDTO(portfolioRepository.save(stock));
        }

        @TrackStockChange(action = "UPDATE")
        public StockDTO updateStock(Long id, StockDTO stockDTO) {
            Stock existingStock = portfolioRepository.findById(id)
                    .orElseThrow(() -> new EntityNotFoundException("Stock not found"));
            existingStock.setName(stockDTO.getName());
            return stockMapper.toStockDTO(portfolioRepository.save(existingStock));
        }

        @TrackStockChange(action = "DELETE")
        public void deleteStock(Long id) {
            portfolioRepository.deleteById(id);
        }
    }
    ```

## Expected Outcome

With this setup, every stock creation, update, or deletion will be logged automatically, providing:

- Clear tracking of stock transactions.
- Improved debugging and auditing capabilities.
- A reusable and declarative way to manage stock operations.

The full source code is available in our [GitHub repository](https://github.com/TechSparkWorkspace/tspark-springboot-custom-annotation-aspect). Feel free to clone, modify, and explore!
