# Build a Lightweight Workflow with Spring Boot and Flowable

In this guide, we’ll walk through creating a lightweight business process using Spring Boot and Flowable BPM, modeled around a realistic use case: an options trade approval for the SPY ETF.

## 🎯 Real-World Scenario: SPY Options Trade Approval

Let’s say a trader wants to submit an options trade. Depending on the risk level, the trade might need automatic validation or manual approval. Here's how the process flows:

- Trader submits a trade request.
- The system validates the request using predefined rules.
- A gateway checks if the trade is high-risk.
- If high-risk, it’s routed for manager approval.
- Upon approval, the trade is executed automatically.
- Finally, the trader is notified about the outcome.

This mimics real-world scenarios where a mix of automation and human review is necessary.

## 🧰 Setting Up the Workflow

You can either use the locally hosted Flowable Modeler or experiment with a cloud-based BPMN editor to quickly prototype your workflow.

- 🔗 Try [Flowable Cloud Modeler](https://www.flowable.com/trial) or other online BPMN tools like bpmn.io.
- 📦 For production or exportable models, use the official Flowable Modeler with Docker or local setup.

Once modeled, export the process as XML (e.g., tradeApprovalProcess.bpmn) and place it under:

`src/main/resources/processes/tradeApprovalProcess.bpmn`

## 🧰 Visualizing the Workflow

Here's a snapshot of the workflow diagram that powers this project:

![Trade Approval Workflow Diagram](https://raw.githubusercontent.com/CodrixWorkspace/KnowledgeBase/main/articles/images/trade_approval_process.png)

This BPMN diagram consists of the following elements:

- 🔹 Start Event – Triggers the workflow
- 🔹 Service Task (Validate Trade) – Validates the incoming trade request
- 🔹 Exclusive Gateway – Determines whether the trade is high-risk
    - 🔸 If Yes → routes to User Task for Manager Approval
    - 🔸 If No → proceeds directly
- 🔹 Service Task (Execute Trade) – Executes the trade
- 🔹 End Event – Ends the workflow

## 🧠 Understanding the Workflow Components

If you're new to BPMN or Flowable, here's a quick breakdown of the core components used in our diagram:

- **Start Event** 🟢

    Represents the beginning of the workflow. It waits for a trigger—in this case, the API call that starts the trade process.

- **Service Task** ⚙️

    A fully automated task executed by the engine. Used here to validate trade data and execute the trade. In Spring Boot, these map to Java classes implementing JavaDelegate.

- **Exclusive Gateway** 🔀

    A decision point. It evaluates conditions to determine the next path in the process. For our case: is the trade high-risk?

- **User Task** 👤

    A manual step that requires human input—like manager approval for risky trades. Can be handled via a task UI or REST interface.

- **End Event** 🔴

    Marks the end of the workflow. No steps are executed beyond this point.

These components are the foundation of many business process flows and give you the power to model real-world operations with clarity and control.

## 🚀 Running the Demo

- Clone the repo:

```bash
git clone https://github.com/TechSparkWorkspace/tspark-trade-workflow.git
cd tspark-trade-workflow
```

- Start the app

```bash
./gradlew bootRun
```

- Open Swagger UI to test APIs:

[http://localhost:8080/swagger-ui.html](http://localhost:8080/swagger-ui.html)

## ⚙️ Interacting with the Workflow

This project includes a dedicated controller (WorkflowController) to help you inspect and manage your running workflows directly through REST APIs. You can access these endpoints via Swagger or Postman to streamline debugging and manual flow control.

### 📌 Endpoints Overview

- GET `/api/workflow/instances` — List all active process instances.
- GET `/api/workflow/tasks` — View user tasks. You can filter by:
  - `?assignee=john` → tasks assigned to a user
  - `?candidateGroup=managers` → tasks waiting for a group
- POST `/api/workflow/tasks/{taskId}/complete` — Complete a user task manually. Accepts an optional JSON body to pass variables.

Once the application is up and running, here’s how you can interact with the workflow and test different scenarios:

#### 🔄 Triggering the Workflow

Use the following `curl` command to simulate a trade request:

```bash
curl -X POST http://localhost:8080/api/trade/start \
  -H "Content-Type: application/json" \
  -d '{
        "symbol": "SPY",
        "quantity": 5,
        "orderType": "PUT_CREDIT_SPREAD"
      }'
```

- If the trade meets normal risk conditions, it will automatically proceed to execution.
- If it is flagged as high-risk, it will pause and wait for manual manager approval.

You can customize the `TradeValidationService` to simulate various risk thresholds.

- If the trade meets normal risk conditions, it will automatically proceed to execution.
- If it is flagged as high-risk, it will pause and wait for manual manager approval.

#### 🧾 Viewing Running Process Instances

Use the following curl command to view all active process instances:

```bash
curl -X GET http://localhost:8080/api/workflow/instances
```

This helps you debug or monitor ongoing workflows.

#### 👤 Viewing User Tasks (Manager Approval)

Since we haven't assigned the task to a specific user or group, you can list all unassigned tasks with:

`curl -X GET "http://localhost:8080/api/workflow/tasks"`

If you later update your process to assign tasks to users or groups, you can add filters like `?assignee=john` or `?candidateGroup=managers`

#### ✅ Completing a User Task

Once you have the task ID, complete it using this `curl` command:

```bash
curl -X POST http://localhost:8080/api/workflow/tasks/{taskId}/complete \
  -H "Content-Type: application/json" \
  -d '{}'
```

To pass additional variables when completing the task:

```bash
curl -X POST http://localhost:8080/api/workflow/tasks/{taskId}/complete \
  -H "Content-Type: application/json" \
  -d '{
        "approved": true,
        "reviewedBy": "john"
      }'
```

Replace `{taskId}` with the actual ID returned from the task list API.

This moves the workflow forward to the trade execution step.
