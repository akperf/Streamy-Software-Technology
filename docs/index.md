# System Architecture Documentation

Welcome to the automated documentation site for the Streamy Video Streaming Backend Platform.

This website is generated automatically using GitHub Actions, Mermaid CLI, and GitHub Pages.

## Service Flow

```mermaid
sequenceDiagram
    autonumber
    Client->>+API Gateway: Request /data
    API Gateway->>+Service: Fetch Data
    Service-->>-API Gateway: Return Payload
    API Gateway-->>-Client: 200 OK
```