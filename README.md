# FS_IshaanGujaran
Problem Statement 1 - Student Commute Optimizer (Full Stack)

---

## MitraRide – Smart, Safe , Together.
Commute with Your Friends or Make New Friends while Communting !



## 1. Problem Statement
Daily student commutes are often unorganized, unsafe, and inefficient. Many students rely on public transport, private cabs, or riding alone, which leads to:
- High travel costs  
- Unpredictable travel times  
- Safety concerns, especially for late-night travel  
- Lack of sustainable, eco-friendly ride-sharing options  



## 2. Existing Solutions & Their Gaps
- **Generic ride-sharing apps (Ola, Uber, Rapido)**  
  - Not tailored for student communities  
  - Expensive for daily use  
  - Lack features like trusted co-riders, college-based grouping  

- **College WhatsApp/Telegram groups**  
  - Unorganized, hard to track rides  
  - No real-time matching of routes  
  - No safety verification  



## 3. Why MitraRide?
**MitraRide is built for students, by students.**  
It focuses on solving the commuting pain points through:  
- **Smart matching:** Connects students going in the same direction.  
- **Safety-first:** Verified student profiles + trusted ride groups.  
- **Affordable & sustainable:** Shared costs, reduced carbon footprint.  
- **Campus-centered design:** Built specifically for daily college commuting.  

---

## 4. Solution Overview

MitraRide matches students for daily commutes by converting routes into geospatial signatures (H3 hex cells), using a fast in-memory index (Redis) to fetch candidates, and a Match Engine that scores candidates by route overlap, time window, and detour cost (OSRM/GraphHopper). Secure anonymized chat (WebSocket) lets matched students coordinate pickups. The system is designed for low-latency results (<1s for listings) and horizontal scale.

