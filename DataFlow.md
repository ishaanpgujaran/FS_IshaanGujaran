# MitraRide – Data Flow

This document describes how information flows through the system when a student uses MitraRide.  
We cover both **Driver ride publishing** and **Passenger ride requesting**.

---

## 1. Driver Publishes a Ride
1. Driver enters **origin, destination, departure time, seats**.
2. API calls **Routing Service (OSRM/GraphHopper)** → returns polyline of route.
3. Polyline is converted into **H3 cells** (geospatial hex indexes).
4. Ride data is stored in **Postgres** (full details) and **Redis** (H3 sets for fast lookup).
5. Seats availability & route index remain cached with TTL until departure.

---

## 2. Passenger Requests a Ride
1. Passenger enters **origin, destination, preferred time**.
2. Routing Service generates polyline for passenger → converted into H3 cells.
3. System queries **Redis** with `SUNION` on H3 cells → gets candidate ride_ids.
4. Candidate rides are checked in **Postgres** for seat availability & time window.
5. Matching Engine ranks rides:
   - Jaccard overlap of H3 cells
   - Time similarity
   - Estimated detour (via Routing Service)
6. Top N matches returned to passenger.

---

## 3. Match & Communication
1. Passenger selects a suggested ride.
2. Chat session opens using **WebSocket Chat Service**.
3. Messages are anonymized and stored encrypted in **Postgres**.
4. Once confirmed, seat availability in Postgres is updated.

---

## Sequence Diagram (Mermaid)

```mermaid
sequenceDiagram
    participant D as Driver
    participant P as Passenger
    participant API as Core API
    participant ROUTE as Routing Service
    participant R as Redis (H3 Index)
    participant DB as Postgres

    D->>API: Publish ride (origin, dest, time, seats)
    API->>ROUTE: Get polyline
    ROUTE-->>API: Route polyline
    API->>R: Store ride_id in H3 sets
    API->>DB: Save ride details

    P->>API: Request ride (origin, dest, time)
    API->>ROUTE: Get polyline
    ROUTE-->>API: Passenger polyline
    API->>R: SUNION on H3 sets
    R-->>API: Candidate ride_ids
    API->>DB: Filter candidates (availability, time)
    API->>ROUTE: Estimate detour for top-k
    ROUTE-->>API: Detour estimates
    API-->>P: Return top N matches

    P->>API: Select ride
    API->>DB: Update seats
    P->>API: Open chat
    API->>DB: Store encrypted messages
