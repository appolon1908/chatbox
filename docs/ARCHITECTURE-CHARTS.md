# chatbox — Architecture Charts

> Repository: `appolon1908/chatbox`  
> Baseline branch: `main`  
> Repository-local visual architecture. Keep these diagrams aligned with implementation, contracts and runtime boundaries.

## 1. System context
```mermaid
flowchart LR
 A["Customers / agents"] --> B["Conversation workspace"]
 B --> R["chatbox<br/>Conversation and agent inbox application"]
 R --> S["threads / routing / handoff state"]
 R --> D["Middleware / messaging providers"]
```

## 2. Internal component architecture
```mermaid
flowchart TB
 I["Entrypoint / UI / API"] --> P["Identity, policy, validation"]
 P --> C["Core domain / orchestration"]
 C --> S["State / configuration / persistence"]
 C --> A["Adapters / integrations"]
 A --> X["Approved dependencies"]
 C --> O["Metrics, logs, traces, audit"]
```

## 3. Critical runtime flow
```mermaid
sequenceDiagram
 participant U as Caller
 participant B as chatbox
 participant P as Policy
 participant C as Core
 participant S as State
 participant X as Dependency
 U->>B: Request / event / action
 B->>P: Authenticate + validate
 P-->>B: Decision
 B->>C: Message intake, assignment, reply and handoff
 C->>S: Read / persist state
 C->>X: Bounded integration
 X-->>C: Result / readback
 C-->>U: Normalized response / view
```

## 4. Deployment and promotion
```mermaid
flowchart LR
 F["Feature branch"] --> T["Tests / validation"]
 T --> PR["Pull request + review"]
 PR --> CI["CI green"]
 CI --> ST["Staging / isolated verification"]
 ST --> EX["Exact-SHA certification"]
 EX --> G{"Production approval?"}
 G -- No --> ST
 G -- Yes --> P["Production promotion"]
 P --> H["Health/readiness + rollback check"]
```

## 5. Observability and recovery
```mermaid
flowchart LR
 R["chatbox"] --> M["Metrics"]
 R --> L["Logs / audit"]
 R --> T["Traces / correlation"]
 M --> O["Observability stack"]
 L --> O
 T --> O
 O --> A["Dashboards / alerts"]
 R --> B["Backup / config snapshot"]
 B --> RR["Restore / rollback rehearsal"]
```

## Ownership notes
- **Role:** Conversation and agent inbox application
- **Primary boundary:** Conversation workspace
- **State/config:** threads / routing / handoff state
- **Dependencies/consumers:** Middleware / messaging providers
- Cross-repository effects must use reviewed contracts; production effects remain separately gated.
