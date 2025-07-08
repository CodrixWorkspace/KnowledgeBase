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

Once modeled, export the process as XML and place it under `src/main/resources/processes/trade-approval.bpmn20.xml`

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

src/main/resources/processes/trade-approval.bpmn20.xml