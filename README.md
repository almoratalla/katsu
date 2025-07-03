# 🥢 Katsu API

Katsu is a modular, TypeScript-based backend service built using **Clean Architecture** principles. It serves as the core API layer for multiple applications such as:

-   **DawnAI** – an AI-powered chatbot
-   **Lesson Plan AI** – a teacher-focused curriculum tool
-   Other client applications via **generated SDKs** in JavaScript and Python

This project is designed for **long-term scalability**, **testability**, and **language-agnostic access** through OpenAPI-defined contracts and SDKs.

---

## 🧱 Tech Stack

-   **Language**: TypeScript
-   **Framework**: Express / Fastify (TBD)
-   **Architecture**: Clean Architecture (Modular Monolith)
-   **API Spec**: OpenAPI 3.0
-   **Package Management**: npm or pnpm
-   **Containerization**: Docker
-   **Testing**: Jest
-   **Validation**: Zod or class-validator
-   **ORM/Database**: Prisma / PostgreSQL (planned)

---

## 📂 Folder Structure

```txt
src/
├── domain/             # Core business logic, entities, interfaces
├── application/        # Use cases and input/output boundaries
├── infrastructure/     # Database, third-party services, adapters
├── interfaces/         # HTTP controllers, routes, DTOs
├── config/             # DI container, environment, constants
└── main.ts             # App entry point
```

---

## 🧪 SDKs

This API supports **auto-generated SDKs** via OpenAPI Generator:

-   `katsu-sdk-js` – JavaScript/TypeScript SDK
-   `katsu-sdk-py` – Python SDK

These are published or imported by consuming apps (e.g., DawnAI, Lesson Plan AI).

---

## 🛠️ Getting Started

### 1. Clone the repository

```bash
git clone https://github.com/yourorg/katsu.git
cd katsu
```

### 2. Install dependencies

```bash
npm install
# or
pnpm install
```

### 3. Run the app

```bash
npm run dev
```

### 4. Generate SDKs (optional)

```bash
npm run generate:sdk
```

---

## 🚧 Roadmap & Future Plans

-   ✅ Use Clean Architecture with modular structure
-   ✅ Generate and distribute SDKs for Node and Python
-   🔄 Future migration to **microservices** for scaling and team autonomy
-   ⛴️ Containerize with Docker for deployment
-   🔐 Implement authentication and authorization (JWT/OAuth2)
-   📊 Add monitoring, logging, and observability tooling
-   🧪 Add unit, integration, and contract tests
-   🌐 Optional GraphQL support

---

## 👤 Author

**Alain Moratalla**  
[alainmoratalla.com](https://alainmoratalla.com)

---

## 📝 License

MIT
