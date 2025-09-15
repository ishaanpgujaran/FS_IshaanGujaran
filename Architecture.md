# MitraRide System Architecture

This diagram illustrates the high-level architecture of the MitraRide platform.

```mermaid
flowchart LR
  subgraph CLIENTS
    C[Web / Mobile Client]
  end
  subgraph EDGE
    ALB[API Gateway / Load Balancer]
  end
  subgraph SERVICES
    AUTH[Auth Service JWT]
    API[Core API / User Service]
    MATCH[Match Engine]
    ROUTE[Routing Service OSRM/GraphHopper]
    CHAT[Chat Service WebSocket]
  end
  subgraph DATA
    PG[(Postgres + PostGIS)]
    REDIS[(Redis - H3 index)]
    S3[(S3 / Object Storage)]
  end

  C -->|HTTPS| ALB --> AUTH
  ALB --> API
  API --> PG
  API --> REDIS
  API --> MATCH
  MATCH --> REDIS
  MATCH --> ROUTE
  C -->|WS| CHAT
  CHAT --> PG
  PG --> S3
