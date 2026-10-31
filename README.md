# Streamy Video Streaming Backend Platform 
 
## Software Technology Course Laboratory Node 
*   **Student Identity:** [Vagif Naghiyev] 
*   **Neptun System Code:** [H3QDUX] 
*   **Course Section Reference:** GEIAL314-B2a 
 --- 
 
## Workspace Environment Architecture 
The diagram below maps out how our development environment connects local code 
authoring to remote version tracking, using a **Diagrams-as-Code** design pipeline. 
 

```mermaid
flowchart TD
    UserNode["💻 Workstation Client<br/>(Terminal CLI / Web Browser)"]
    LiveSandbox{"🌐 Cloud Sandbox<br/>(mermaid.live)"}
    LocalIDE["⚙️ VS Code IDE<br/>(Markdown Preview Extension)"]
    GitStage["📦 Local Git Index<br/>(Staging Ledger)"]
    RemoteHub["☁️ GitHub Remote Cloud<br/>(Streamy Repository Workspace)"]

    UserNode -- "1. Sandbox Syntax" --> LiveSandbox
    LiveSandbox -- "2. Verified Block Migration" --> LocalIDE
    LocalIDE -- "3. git add CLI Operations" --> GitStage
    GitStage -- "4. git push Pipeline Push" --> RemoteHub
```




## Streamy Application Engineering Development Lifecycle

To guarantee systematic software tracking across the implementation lifespan of **Streamy**, our individual developer pipeline follows the structured iterative gate review framework illustrated below.

```mermaid
---
title: Streamy Iterative SDLC Blueprint
---
flowchart TD
    %% Global Stylings and Direction Vector (Top-Down)
    %% Subgraph Process Isolation Boundaries

    subgraph SpecPhase [1. Requirements Specification Block]
        A[Inception Abstract Created] --> B[SRS Requirements Compiling]
    end

    subgraph DesignPhase [2. UML Architectural Modeling]
        B --> C[Draft System Border Graphs]
        C --> D[Construct Advanced Class Models]
    end

    subgraph ValidationGate [3. Verification Gatekeeper Loop]
        D --> E{Does Architecture Match Code Specs?}
        E -- No: Refactor Blueprint --> B
    end

    subgraph ProductionPhase [4. Python Construction Layer]
        E -- Yes: Pass Gate --> F[Compile Python Class Skeletons]
        F --> G[Implement Internal Class Logic]
    end

    %% Apply explicit geometric shape markers
    E:::decisionStyle

    classDef decisionStyle fill:#f9f,stroke:#333,stroke-width:2px;
```
## User Sign Up Sequence Diagram

The following sequence diagram illustrates the Streamy user registration process, including synchronous communication, asynchronous event publishing, input validation, and alternative execution paths.

```mermaid
---
title: User Sign Up Flow
---
sequenceDiagram
    autonumber

    actor Browser
    participant Signup as Sign Up Service
    participant Users as User Service
    participant Kafka

    links Users: {"Repository": "https://example.com/user-service"}

    Browser->>+Signup: GET /sign_up
    Signup-->>-Browser: 200 OK (HTML page)

    Browser->>+Signup: POST /sign_up
    Signup->>Signup: Validate input

    alt invalid input
        Signup-->>Browser: Error
    else valid input
        Signup->>+Users: POST /users
        Users--)Kafka: User Created Event Published
        Note left of Kafka: Other services take action based on this event
        Users-->>-Signup: 201 Created (User)
        Signup-->>-Browser: 301 Redirect (Login page)
    end
```