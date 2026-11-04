# Deterministic Multi-Agent Web Compiler (D-MAC)

An autonomous, 8-agent software engineering pipeline designed to transform unconstrained natural language into production-grade, full-stack web applications.

This system operates on a hybrid methodology: utilizing zero-shot AI synthesis to dynamically architect PostgreSQL schemas and Express.js REST APIs from a blank canvas, while enforcing strict UI constraints via AST compiler gates to generate bespoke React/Tailwind interfaces.

## 🏗️ System Architecture & Visual Pipeline

![System Architecture](./System%20Architecture.jpg)

The system utilizes a Directed Acyclic Graph (DAG) architecture via LangGraph, routing payloads between specialized autonomous agents, featuring an in-memory `esbuild` validation gate and ChromaDB episodic memory repair loops.

## 💼 Executive Summary & Unit Economics

* **🚀 Zero-Shot Synthesis:** Generates bespoke frontends and backends dynamically, bypassing rigid template constraints while maintaining structural integrity.
* **🛡️ Secure by Default:** Autonomously injects `bcryptjs` password hashing and JWT RBAC authentication into every generated application.
* **📈 Concurrent Execution:** Stateful orchestration layer supports isolated process execution and sandboxed validation.
* **💰 Token-Optimized Unit Economics:** Eliminates the massive inference overhead typical of monolithic AI coding agents through deterministic AST gating and multi-model tier routing.

**The Multi-Model Cost Advantage:** 
Traditional autonomous software agents rely on brute-force LLM retry loops using expensive frontier models. D-MAC routes complex architectural reasoning to **Google Gemini 2.5**, offloads high-speed component synthesis to **Groq (Llama-3 70B)**, and catches syntax errors locally via in-memory `esbuild` gates before spending repair tokens—enabling full-stack generation at sub-cent marginal costs (and exactly $0.00 OpEx under standard developer rate-limit tiers).

## 🧠 System Design & End-to-End Data Flow

The pipeline executes through a LangGraph-orchestrated DAG, enforcing strict data schemas across 8 specialized Python agents.

* **State Machine & Telemetry:** User inputs are processed via a Flask ThreadWorker, utilizing a custom LogCatcher queue to stream real-time Server-Sent Events (SSE) telemetry to a React observability dashboard.
* **State Persistence:** Pipeline execution state is serialized into an SQLite WAL checkpointer (`outputs/checkpoints.sqlite`), allowing the orchestration engine to pause, inspect, and resume across pipeline failures.
* **Payload Contracts:** Pydantic v2 enforces strict type-safe JSON contracts across all agent nodes to guarantee deterministic data flow.

### The 7-Stage Generation Pipeline:
1. **Requirements & Planning:** The `RequirementAgent` and `PlannerAgent` extract domain context and generate a validated BlueprintSchema. The `UXLayout` tool injects media fields and component archetypes.
2. **Database Synthesis:** The `DatabaseAgent` synthesizes a relational `schema.prisma` and executes `npx prisma validate` inside a Docker sandbox.
3. **Backend Synthesis:** The `BackendAgent` writes Express.js REST routes, CRUD controllers, and JWT auth systems.
4. **Frontend Synthesis:** The `FrontendAgent` generates atomic React 18 pages, Vite configurations, dynamic React Router mappings, and wraps the application root in an ErrorBoundary.
5. **Testing & QA Verification:** The `TestingAgent` runs static validation, Vite build checks, and HTTP CRUD integration tests.
6. **Automated Repair Loop:** Failures trigger the `RepairNode`, which queries a ChromaDB vector memory store for historical fixes and re-routes back to testing.
7. **Cloud Deployment:** Upon passing all gates, the `DeploymentAgent` executes a forced database schema reset, commits the tree to GitHub, and triggers a live serverless deployment on Vercel Edge.

## 🛡️ Deterministic Gating (The "Black Box" Logic)

To counter non-deterministic LLM hallucinations during blank-canvas generation, the pipeline surrounds the AI nodes with rigorous compiler gates.

### 1. In-Memory AST Compiler Gate (The Hallucination Fix)
Before any generated `.jsx` file is saved, the agent executes an isolated Node.js child process calling Vite's internal `transformWithEsbuild` API. If syntax errors are caught, the exact AST offset and error stack are injected into a self-healing LLM reflection prompt. Furthermore, hallucinated UI icons are dynamically intercepted and aliased to a strict whitelist registry of 3,699 valid `lucide-react` exports.

### 2. Episodic RAG Vector Memory (The Infinite Loop Fix)
When the AI attempts to fix complex bugs, it frequently enters an infinite loop of breaking code. The system utilizes an embedded ChromaDB instance operating on a custom zero-dependency 128-dimensional sparse vector projection. It stores successful `error_trace -> code_fix` episodes, injecting validated historical patches directly into the agent's context to break cyclic repair loops.

### 3. Synthetic Headless Playwright Reflection (The Silent Crash Fix)
AI-generated React code might compile perfectly but crash silently in the browser. The QA node spawns a headless Chromium instance via Playwright, hooks listeners into `pageerror` events, and filters the DOM for `<vite-error-overlay>`. If a crash is detected, it feeds the stack trace back to the reasoning tier for hot-reload auto-patching.

### 4. Deterministic Schema Sanitization & Media Probing
* **Relational Flattening:** Strips brittle LLM-generated cyclic foreign keys and unsupported data types (e.g., converting Timestamp to DateTime), ensuring serverless PostgreSQL deployment stability.
* **Media Verification:** Interrogates the Wikipedia Live API for contextual assets, utilizing concurrent ThreadPoolExecutor HTTP probes to enforce 200 OK statuses and >3000 byte payloads, actively filtering out dead links and 1x1 tracking pixels.

## 🔒 Security & Authentication Infrastructure

Every dynamically generated application is injected with a production-ready security layer by default:

* **JWT & Bcrypt Encryption:** Passwords are salt-hashed using `bcryptjs` (work factor 10). Authentication routes are protected via JWTs signed with 7-day expirations.
* **Fault-Tolerant RBAC Middleware:** To prevent unauthenticated UI exploration from throwing unhandled runtime crashes in live sandbox previews, public read requests safely degrade to a read-only Guest Context (`req.user = { role: 'GUEST' }`), while protected mutation routes (`POST`, `PUT`, `DELETE`) strictly enforce `401/403` JWT role verification.
* **Dynamic Role Seeding:** During Prisma migrations, the system executes `seedDefaultUsers()`, reading the Role-Based Access Control (RBAC) blueprint to automatically provision seeded Admin and domain-specific accounts for immediate QA evaluation.

## ⚙️ Enterprise Technology Stack

| Layer | Core Technologies |
| :--- | :--- |
| **AI & Orchestration** | Python 3.12, LangGraph (`StateGraph`), Pydantic v2, ChromaDB, DeepEval |
| **LLM Tier Routing** | Google Gemini (2.5-Pro / 2.5-Flash / Flash-Lite), Groq (Llama-3 70B), Custom 128D Vectors |
| **Backend Synthesis** | Node.js (18+), Express.js 4.18, Prisma ORM 5.22, PostgreSQL (Supabase) / SQLite |
| **Frontend Synthesis** | React 18.2, Vite 5.4 (`esbuild`), Tailwind CSS 3.3, React Router DOM 6, Lucide React |
| **QA & Telemetry** | Playwright (Headless Chromium), Pytest, Docker (`node:18-alpine`), Flask SSE, SQLite WAL |
