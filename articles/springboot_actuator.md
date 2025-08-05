# 🚀 Decoding Spring Boot Actuator: The Key to Smarter Services

Spring Boot Actuator brings **production-ready features** to your applications without the overhead. From **live metrics and health checks** to environment insights and auditing, it makes your **services observably smarter and easier** to maintain.

💡 If you’re building **REST APIs or microservices**, this is a must-have toolkit baked right into Spring Boot!

## Project Setup

✅ If you haven’t yet created a Spring Boot project and want to start from scratch with Gradle, check out our **step-by-step guide** here:
[👉 How to Create a Spring Boot Project with Gradle (Beginner Friendly)](https://skillhunt.codrixtech.com/guide/view?src=spring_boot_gradle.md&id=10e96ec0-f43c-4cea-bd7f-ca3412fc268f)

This guide helps you scaffold a Spring Boot application using the **command line and IntelliJ IDEA**, so you’re all set before jumping into **experimenting actuator capabilities.** 🛠️

If you’re **short on time and want to dive straight into the practical implementation**, you can refer to my pre-configured GitHub [Spring Boot Actuator Guide repository](https://github.com/TechSparkWorkspace/tspark-springboot-actuator.git). You can clone it, run the application, and experiment with guide right away. 📚

## 📦 Adding Gradle Dependency

Make sure your build.gradle includes:

```groovy
dependencies {
    implementation 'org.springframework.boot:spring-boot-starter-actuator'
}
```

## 📡 Actuator Default Endpoints

Spring Boot Actuator exposes a set of **useful endpoints to monitor and manage** your application. By default, only `/actuator/health` is enabled for web access.

To expose additional endpoints like:

- `/actuator/info`
- `/actuator/metrics`
- `/actuator/env`
- `/actuator/mappings`

you’ll need to configure them explicitly:

```yaml
# application.yml
management:
  endpoints:
    web:
      exposure:
        include: "*"
```

_**Note:**  🔐 For production environments, it's best to expose only the necessary endpoints and secure them properly_

## ✅ `/actuator/health` – The Heartbeat of Your App

**🔍 What it does:**

- Returns `UP`, `DOWN`, `OUT_OF_SERVICE`, or `UNKNOWN`
- Aggregates health indicators (disk, DB, etc.)
- Ideal for uptime monitoring and container orchestration probes

### ⚙️ Configuration

```yaml
management:
  endpoint:
    health:
      show-details: always
```

This tells Spring Boot Actuator to **always include detailed health information** (like database or disk status) in the `/actuator/` health response, even when accessed over the web.

### 🧰 Add your own custom indicator

You can extend Actuator’s health checks by creating your own indicator. This lets you report the status of any custom service or resource in your application.

```java
@Component
public class CustomHealthIndicator implements HealthIndicator {
    @Override
    public Health health() {
        return Health.up().withDetail("customService", "Running smoothly").build();
    }
}
```

### 🧪 Try: `/actuator/health`

```json
{
  "status": "UP",
  "components": {
    "custom": {
      "status": "UP",
      "details": {
        "customService": "Running smoothly"
      }
    },
    "diskSpace": {
      "status": "UP",
      "details": {
        "total": 994662584320,
        "free": 79052988416,
        "threshold": 10485760,
        "path": "/Workspace/EducateWorkspace/tspark-springboot-actuator/.",
        "exists": true
      }
    },
    "ping": {
      "status": "UP"
    }
  }
}
```

## ℹ️ `/actuator/info` – App Metadata

The primary purpose of the `/actuator/info` endpoint is to **expose custom, non-sensitive metadata about your application**. This can include **version info, build details, team name, or anything** you'd like to make visible for documentation or dashboard purposes.

### ⚙️ Customize

Customize this endpoint with metadata like app version, name, etc.

```yaml
info:
  app:
    name: "TechSpark Actuator Demo"
    version: "1.0.0"
    description: "Spring Boot Actuator exploration"
```

### 🧪 Try: `/actuator/info`

```json
{
  "app": {
    "name": "TechSpark Actuator App",
    "version": "1.0.0",
    "description": "This is a Spring Boot application to showcase the Actuator capabilities"
  }
}
```

## 📈 /actuator/metrics – Real-Time Metrics

Exposes various metrics collected by Micrometer. Micrometer is the underlying **metrics library that Spring Boot Actuator uses out-of-the-box**. It comes **built-in with the Actuator starter**, so you don’t need to add it separately. It provides a **simple facade over popular monitoring systems like Prometheus, Datadog, New Relic**, and more.

By default, Micrometer captures **JVM stats, process metrics, HTTP request performance**, and more — making it a powerful tool for real-time observability.

### 🔍 Metrics include:

- jvm.memory.used
- system.cpu.usage
- http.server.requests

### 🧪 Try: ```/actuator/metrics/jvm.memory.used```

```json
{
  "name": "jvm.memory.used",
  "description": "The amount of used memory",
  "baseUnit": "bytes",
  "measurements": [
    {
      "statistic": "VALUE",
      "value": 80486680
    }
  ],
  "availableTags": [
    {
      "tag": "area",
      "values": [
        "heap",
        "nonheap"
      ]
    },
    {
      "tag": "id",
      "values": [
        "CodeHeap 'profiled nmethods'",
        "G1 Old Gen",
        "CodeHeap 'non-profiled nmethods'",
        "G1 Survivor Space",
        "Compressed Class Space",
        "Metaspace",
        "G1 Eden Space",
        "CodeHeap 'non-nmethods'"
      ]
    }
  ]
}
```

## 🌱 `/actuator/env` – Environment Variables & Configs

This endpoint provides **detailed visibility into the application's active environment**. It exposes **configuration properties, system properties, active Spring profiles, and values from the environment abstraction** — all of which can be incredibly helpful for **diagnosing configuration issues, verifying profile setups**, or understanding where property values are coming from.

### 🧪 Try: ```/actuator/env```

_🔒 Sensitive values like passwords, keys, and tokens are masked by default for security. To reveal them (not recommended for production), you can configure:_

```yaml
management:
  endpoint:
    env:
      show-values: ALWAYS
```

_⚠️ Use `show-values: ALWAYS` only in development or controlled environments. In production, prefer the default (`WHEN_AUTHORIZED`) to avoid exposing secrets.

## 🗺 `/actuator/mappings` – Request Routing Map

Shows all controller and route mappings in the app.

### 🧩 Sample Controller

To see something show up in the mappings, try adding this simple controller:

```java
@RestController
@RequestMapping("/api/products")
public class ProductController {

    @GetMapping
    public List<String> getProducts() {
        return List.of("Laptop", "Monitor", "Keyboard");
    }

    @PostMapping
    public ResponseEntity<String> addProduct(@RequestBody String product) {
        return ResponseEntity.ok("Product added: " + product);
    }
}
```

Once this controller is loaded, it will appear in the `/actuator/mappings` output under `/api/products` with both `GET` and `POST` methods.

### 🧪 Try: ```/actuator/mappings```

_🔧 If this endpoint doesn't work, make sure it's included in the `exposure.include` list in your configuration:_

```yaml
management:
  endpoints:
    web:
      exposure:
        include: ["mappings"]
```

You can also use include: "*" during local testing to expose all endpoints temporarily.

## 🧵 `/actuator/threaddump` – JVM Threads Snapshot

This endpoint provides a live snapshot of all JVM threads currently running in your application. It’s helpful for identifying performance bottlenecks, deadlocks, or stuck threads during high load or outages.

## 📴 `/actuator/shutdown` – Graceful Exit Trigger

⚙️ Enable explicitly:

```yaml
management:
  endpoint:
    shutdown:
      enabled: true
```

Use `POST` request to trigger shutdown.

## 💾 `/actuator/heapdump` – Memory Dump for Analysis

Used for offline memory analysis with tools like Eclipse MAT.


## 💡 Enjoying this guide on SkillHunt?

Spring Boot Actuator transforms your app into a **self-diagnosing, production-friendly service**. Whether you're **deploying to the cloud or monitoring on-prem**, it's your go-to companion for observability.

Don’t stop here — explore our growing collection of **hands-on user guides to sharpen your full-stack skills**. From Spring Boot and MongoDB to Angular, React, and DevOps tips — we’ve got you covered!
