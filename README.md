# ASHES
A modern, secure, real-time chat app built with Java 21 (Spring Boot) and React + TS. Operating on a zero-retention philosophy ("Messages are meant to be seen, not stored"), ASHES utilizes an explicit in-memory message store and asynchronous virtual threads to automatically incinerate messages shortly after they are read.

1. **In-Memory Volatility**: Messages exist exclusively in application heap space within thread-safe concurrent caches (`ConcurrentHashMap`). 
2. **Asynchronous Eviction**: Triggered by a `READ` frame confirmation, a dedicated high-throughput schedule coordinator using **Java 21 Virtual Threads** triggers a countdown.
3. **Hard Extinguish**: Upon expiration, the memory key is purged, and a hardware/socket disconnect event triggers an instantaneous clear across all connected client browser states. 
4. **Resiliency Through Reset**: If the backend container bounces, all unread operational arrays vanish automatically and completely. This is intentional.

---

## 🛠️ Technology Blueprint

### Backend (Java Infrastructure)
* **Runtime**: Java 21+ (Leveraging native Virtual Threads for massive concurrent eviction scheduling)
* **Framework**: Spring Boot 3.x (Spring Web, Spring Security, Spring WebSocket)
* **State Engine**: Non-blocking `ConcurrentHashMap` caching layer
* **Database**: PostgreSQL (**Strictly limited to User Credentials & Session Metadata**)

### Frontend (Premium UI Matrix)
* **Core**: React 18, TypeScript, Vite
* **Styling**: Tailwind CSS
* **Design Language**: Dark Premium minimalist layout inspired by Signal, Telegram, and dark terminal interfaces. 
* **Palette**: Primary `#FF6B00` (Ashes Blaze), Secondary `#121212` (Vantablack Minimal), Accent `#FF9E44` (Embers Accent)

---

## 📦 Architecture & Directory Hierarchy

```plaintext
ashes-secure-chat/
├── backend/
│   ├── src/main/java/com/ashes/
│   │   ├── config/             # Security configurations, WebSocket brokers, thread pool tuning
│   │   ├── domain/             # Core rich entities & enumerations (User, Message models)
│   │   ├── usecases/           # Inbound/Outbound port abstractions
│   │   ├── services/           # Security filters, Authentication tokens, Ephemeral lifecycle engines
│   │   ├── repository/         # Secure User DB (JPA) & Message Cache (In-Memory Maps)
│   │   └── delivery/           # REST Control Layer & Real-time STOMP WebSocket Handlers
│   ├── src/main/resources/     # Operational properties & migrations
│   └── pom.xml                 # Maven declaration
├── frontend/
│   ├── src/
│   │   ├── components/         # Reactive UI structures (Chat Panels, Real-time Countdown Clocks)
│   │   ├── hooks/              # Custom WebSocket state hooks using `@stomp/stompjs`
│   │   └── App.tsx             # Master App Interface Routing Matrix
│   ├── tailwind.config.js      # Custom theme definitions
│   └── package.json            # Client ecosystem descriptors
└── docker-compose.yml          # Global structural orchestration blueprint
