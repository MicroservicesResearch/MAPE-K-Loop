# Adaptive Microservices Architecture with MAPE-K Loop

This project presents an extension of a microservices architecture aimed at automating the selection of adaptation rules in response to local changes in coordination requirements. The extended architecture is based on a **MAPE-K control loop** (Monitor, Analyze, Plan, Execute, Knowledge), commonly used in autonomic computing systems.

## Key Concepts

### Global View

The **Global View** represents the overall microservices composition. It defines how different microservices interact to fulfill a business process. The Global View is managed by the **Global Designer**, and it is modelled using BPMN to provide a clear and structured workflow.

### Local Fragment

Each microservice contains its own **Local Fragment**, which corresponds to the portion of the Global View relevant to that specific microservice. These fragments are derived from the Global View and define the local process of a microservice. Changes made to a Local Fragment may impact the global coordination, triggering the adaptation process managed by the MAPE-K loop.

> In this architecture, when a Local Fragment is modified, the change is logged in the Evolution Event Bus and may lead to an update in the Global View to ensure consistency across the system.

## Overview

The system dynamically adapts a microservices composition by detecting and reacting to local changes introduced in the **Local Fragment** of an individual microservice. These changes are automatically analyzed to determine an appropriate **adaptation rule**, which is then propagated to the **Global Designer** to adjust the global view of the system accordingly.

## Architecture Components

The extended architecture consists of the following components:

### New Components (Introduced in this version)

- **Evolution Event Bus** – A dedicated event channel that logs local changes in microservices. Acts as the system's Knowledge Base.
- **MAPE-K Controller** – A microservice that handles the first three phases of the MAPE-K loop:
  - **Monitoring**: Observes the Evolution Event Bus for change events.
  - **Analysis**: Characterizes the type and impact of detected changes.
  - **Planning**: Proposes suitable adaptation rule based on the analysis.
- **Adaptation Executor** – A new module integrated into the **Global Designer** responsible for executing selected adaptation rules in the global view.

### Existing Components (from the previous architecture)

- **Business Microservices** – Core business microservices such as `Customers`, `Inventory`, etc., which form the functional base of the system.
- **Global Designer** – Manages the composition logic and the global view of the system. In this version, it also receives and applies adaptation rules.

### Process Flow

1. A microservice introduces a change in its Local Fragment.
2. The change is published to the **Evolution Event Bus**.
3. The **MAPE-K Controller** monitors the bus for relevant changes.
4. Identified changes are analyzed and characterized.
5. An appropriate adaptation rule is proposed.
6. The rule is sent to the **Global Designer**, which updates the global view.
7. The updated global view is decomposed into BPMN fragments and sent to affected microservices.

## Technologies Used

- **Spring Boot** – Used to develop the microservices of the architecture.
- **RabbitMQ** – Implements the **Evolution Event Bus**.
- **BPMN (Business Process Model and Notation)** – Used for modeling and composing the global and local microservices processes.
- **MAPE-K Control Loop** – Implemented in Java to support self-adaptive behavior through Monitoring, Analysis, Planning, and Execution phases.


## Acknowledgements

This project has been funded by:

- The project PID2023-146224OB-I00 founded by MICIU/AEI/10.13039/501100011033, FEDER, and the UE.
- The Research and Development Aid Program (PAID-01-21) of the UPV.
- The Aid to First Research Projects (PAID-06-22), Research Vice-Rectorate of the Polytechnic University of Valencia (UPV).
