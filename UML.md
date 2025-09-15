# MitraRide: System Design & UML Diagrams

This document provides a visual representation of the MitraRide system's core components and their interactions using various diagrams.

---

### 1. Use Case Diagram

This diagram outlines the primary functions of the MitraRide application and the actors who interact with them.

```mermaid
graph TD
    subgraph Actors
      Student[Student]
      Admin[Admin]
    end

    subgraph System
      UC1[Request Ride]
      UC2[Offer Ride]
      UC3[View Matches]
      UC4[Join Ride Group]
      UC5[Live Tracking]
      UC6[SOS Alert]
      UC7[Verify Student]
      UC8[Manage Reports]
    end

    Student -- "1. Requests Ride" --> UC1
    Student -- "2. Offers Ride" --> UC2
    Student -- "3. Views Matches" --> UC3
    Student -- "4. Joins Ride" --> UC4
    Student -- "5. Tracks Ride" --> UC5
    Student -- "6. Triggers Alert" --> UC6

    Admin -- "7. Verifies Profile" --> UC7
    Admin -- "8. Manages Issues" --> UC8
```

---

### 2. Class Diagram
This diagram provides a high-level view of the key data entities and their relationships within the MitraRide system.

```mermaid
classDiagram
    class User {
        +String userId
        +String uniqueUsername
        +String collegeId
        +bool isVerified
        +List~Ride~ ridesOffered
        +List~Ride~ ridesJoined
    }

    class Ride {
        +String rideId
        +String driverId
        +String origin
        +String destination
        +DateTime plannedTime
        +List~String~ passengerIds
        +List~Waypoint~ route
    }

    class Safety {
        +String sosId
        +String userId
        +DateTime timestamp
        +String location
    }

    User "1" --> "0..*" Ride : offers/requests
    User "1" --> "0..*" Safety : triggers
    Ride "1" -- "0..*" User : hasPassengers
```

---

### 3. Sequence Diagram
This diagram illustrates the step-by-step interaction between a student, the system backend, and other users when a ride is requested.

```mermaid
sequenceDiagram
    participant Student as S
    participant API_Gateway as AG
    participant Core_API as CA
    participant Match_Engine as ME
    participant Driver as D

    S->>AG: 1. Request Ride (Origin, Dest)
    AG->>CA: 2. Authenticate & Forward
    CA->>ME: 3. Search for Matches
    ME->>ME: 4. Process Geospatial Query
    ME-->>CA: 5. Return List of Matches
    CA-->>AG: 6. Return Anonymized Matches
    AG-->>S: 7. Display Matches

    S->>CA: 8. Select Driver
    CA->>D: 9. Send Ride Request
    D-->>CA: 10. Accept Ride
    CA-->>S: 11. Send Confirmation
    S->>D: 12. Initiate In-App Chat
```
