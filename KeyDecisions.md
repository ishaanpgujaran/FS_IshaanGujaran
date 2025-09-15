# Key Design Decisions & Trade-offs – MitraRide

## 1. Ride Matching Algorithm
**Decision:** Use route similarity + time-window based matching.  
**Reasoning:** Ensures students going in the same direction within ±15 mins are grouped together.  
**Trade-offs:**  
- Fast & simple for small data sets  
- May not scale well with 1000+ users (could require ML clustering later)  
**Future Improvement:** Explore graph-based route optimization or ML clustering.

---

## 2. Database Choice – MongoDB
**Decision:** Use MongoDB (NoSQL) with flexible schema.  
**Reasoning:**  
- Handles dynamic ride requests & user data  
- Easy integration with Node.js backend  
**Trade-offs:**  
- Flexible schema, fast prototyping  
- Complex queries (geo-search, route optimization) can get tricky compared to SQL  
**Future Improvement:** Consider hybrid approach (SQL + NoSQL).

---

## 3. Authentication & Verification
**Decision:** College email verification + optional ID proof upload.  
**Reasoning:** Safety-first, only verified students can join.  
**Trade-offs:**  
- Builds trust among students  
- Onboarding friction (slower signups if verification delays happen)  
**Future Improvement:** Auto-verification with institutional APIs.

---

## 4. Cost Sharing Model
**Decision:** Split cost among co-riders dynamically (based on route distance).  
**Reasoning:** Ensures fairness, promotes daily usage.  
**Trade-offs:**  
- Affordable for everyone  
- Complex edge cases (partial routes, last-minute dropouts)  
**Future Improvement:** Wallet system with penalties for cancellations.

---

## 5. Safety Features
**Decision:** SOS button + live ride tracking + trusted groups.  
**Reasoning:** Safety is a top concern for students & parents.  
**Trade-offs:**  
- High trust factor  
- Requires real-time infra & higher hosting cost  
**Future Improvement:** AI-based anomaly detection (suspicious detours, odd hours).

---

## 6. Tech Stack
**Decision:** MERN stack (MongoDB, Express, React, Node).  
**Reasoning:** Widely used, fast prototyping, full-stack JS.  
**Trade-offs:**  
- One language across stack  
- Scaling real-time features (chat, tracking) needs extra infra (Socket.io, Redis)  
**Future Improvement:** Modular microservices + Docker/K8s for scalability.
