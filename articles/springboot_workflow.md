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

![Trade Approval Workflow Diagram](https://github.com/CodrixWorkspace/KnowledgeBase/blob/23bc5d693af389a874f6a8b0605ade3e13cf89cb/articles/images/trade_approval_process.png)

This BPMN diagram consists of the following elements:

- 🔹 Start Event – Triggers the workflow
- 🔹 Service Task (Validate Trade) – Validates the incoming trade request
- 🔹 Exclusive Gateway – Determines whether the trade is high-risk
    - 🔸 If Yes → routes to User Task for Manager Approval
    - 🔸 If No → proceeds directly
- 🔹 Service Task (Execute Trade) – Executes the trade
- 🔹 End Event – Ends the workflow

Once modeled, export the process as XML and place it under:

src/main/resources/processes/tradeApprovalProcess.bpmn

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
