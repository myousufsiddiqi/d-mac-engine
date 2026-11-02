# Deterministic Multi-Agent Web Compiler (D-MAC)

![Python](https://img.shields.io/badge/Python-3.12-blue?logo=python&logoColor=white)
![LangGraph](https://img.shields.io/badge/LangGraph-StateGraph-orange)
![React](https://img.shields.io/badge/React-18.2-61DAFB?logo=react&logoColor=black)
![PostgreSQL](https://img.shields.io/badge/PostgreSQL-PgBouncer-4169E1?logo=postgresql&logoColor=white)
![Zero_OpEx](https://img.shields.io/badge/OpEx-0.00-brightgreen)

> **⚠️ Release Notice:** The full source code repository will be made publicly available in December. The high-level system architecture, business unit economics, and deterministic gating logic are fully documented below.

An autonomous, 8-agent software engineering pipeline designed to transform unconstrained natural language into production-grade, full-stack web applications. 

This system operates on a hybrid methodology: utilizing zero-shot AI synthesis to dynamically architect PostgreSQL schemas and Express.js REST APIs from a blank canvas, while enforcing strict UI constraints via AST compiler gates to generate bespoke React/Tailwind interfaces.

---

## 🏗️ System Architecture & Visual Pipeline

*(Replace the placeholder image below with your actual LangGraph DAG diagram or architecture flowchart)*

![D-MAC Architecture Flowchart](System%20Architecture.jpg)
*The Directed Acyclic Graph (DAG) architecture routing payloads between specialized autonomous agents, featuring an in-memory esbuild validation gate and ChromaDB episodic memory repair loops.*

---

## 💼 Executive Summary & Business Unit Economics

| 🚀 Zero-Shot Synthesis | 🛡️ Secure by Default | 📈 Infinite Scaling | 💰 $0.00 Generation OpEx |
| :--- | :--- | :--- | :--- |
| Generates bespoke frontends and backends dynamically, bypassing rigid template constraints while maintaining structural integrity. | Autonomously injects bcrypt password hashing and JWT authentication into every generated application. | Orchestration layer allows concurrent agent pipelines for massive parallel generation. | Achieves Devin-level autonomy at zero marginal cost per application by avoiding expensive OpenAI API taxes. |

**The Zero-OpEx Advantage:** Traditional autonomous software agents rely heavily on expensive language models, resulting in high generation costs ($2-$5) per application. This system orchestrates Google Gemini 2.5 (for complex architectural reasoning) and Groq’s Llama-3 endpoints (for ultra-low latency component synthesis), achieving enterprise-grade software generation with an operational expenditure (OpEx) of exactly $0.00 per application.

---

## 🧠 System Design & End-to-End Data Flow

The system utilizes a Directed Acyclic Graph (DAG) architecture via LangGraph, routing payloads between specialized autonomous agents.

*   **State Machine & Telemetry:** User inputs are processed via a Flask ThreadWorker, utilizing a custom LogCatcher queue to stream real-time Server-Sent Events (SSE) telemetry to a React dashboard.
*   **State Persistence:** Pipeline execution state is serialized into an SQLite WAL checkpointer (`outputs/checkpoints.sqlite`), allowing the orchestration engine to pause, inspect, and resume across pipeline failures.
*   **Payload Contracts:** Pydantic v2 enforces strict type-safe JSON contracts across all agent nodes to guarantee deterministic data flow.

### The 7-Stage Generation Pipeline:

1.  **Requirements & Planning:** The `RequirementAgent` and `PlannerAgent` extract domain context and generate a validated `BlueprintSchema`. The `blueprint_enhancer` injects media fields and UX archetypes.
2.  **Database Synthesis:** The `DatabaseAgent` synthesizes a relational `schema.prisma` and executes `npx prisma validate` inside a Docker sandbox.
3.  **Backend Synthesis:** The `BackendAgent` writes Express.js REST routes, CRUD controllers, and JWT auth systems.
4.  **Frontend Synthesis:** The `FrontendAgent` generates atomic React 18 pages, Vite configurations, dynamic React Router mappings, and wraps the application root in an `ErrorBoundary`.
5.  **Testing & QA Verification:** The `TestingAgent` runs static validation, Vite build checks, and HTTP CRUD integration tests.
6.  **Automated Repair Loop:** Failures trigger the `RepairNode`, which queries a ChromaDB vector memory store for historical fixes and re-routes back to testing.
7.  **Cloud Deployment:** Upon passing all gates, the `CloudDeployer` executes a forced Supabase schema reset, commits the tree to GitHub, and triggers a live serverless deployment on Vercel Edge.

---

## 🛡️ Deterministic Gating (The "Black Box" Logic)

To counter non-deterministic LLM hallucinations during blank-canvas generation, the pipeline surrounds the AI nodes with rigorous compiler gates.

### 1. In-Memory AST Compiler Gate (The Hallucination Fix)
Before any generated `.jsx` file is saved, the agent executes an isolated Node.js child process calling Vite's internal `transformWithEsbuild` API. If syntax errors are caught, the exact AST offset and error stack are injected into a self-healing LLM reflection prompt. Furthermore, hallucinated UI icons are dynamically intercepted and aliased to a strict whitelist registry of 3,699 valid `lucide-react` exports.

### 2. Episodic RAG Vector Memory (The Infinite Loop Fix)
When the AI attempts to fix complex bugs, it frequently enters an infinite loop of breaking code. The system utilizes an embedded ChromaDB instance operating on a custom zero-dependency 128-dimensional sparse vector projection. It stores successful `error_trace -> code_fix` episodes, injecting validated historical patches directly into the agent's context to break cyclic repair loops.

### 3. Synthetic Headless Playwright Reflection (The Silent Crash Fix)
AI-generated React code might compile perfectly but crash silently in the browser. The QA node spawns a headless Chromium instance via Playwright, hooks listeners into `pageerror` events, and filters the DOM for `<vite-error-overlay>`. If a crash is detected, it feeds the stack trace back to the reasoning tier for hot-reload auto-patching.

### 4. Deterministic Schema Sanitization & Media Probing
*   **Relational Flattening:** Strips brittle LLM-generated cyclic foreign keys and unsupported data types (e.g., converting `Timestamp` to `DateTime`), ensuring serverless PostgreSQL deployment stability.
*   **Media Verification:** Interrogates the Wikipedia Live API for contextual assets, utilizing concurrent `ThreadPoolExecutor` HTTP probes to enforce `200 OK` statuses and `>3000` byte payloads, actively filtering out dead links and 1x1 tracking pixels.

---

## 🔒 Security & Authentication Infrastructure

Every dynamically generated application is injected with a highly robust, production-ready security layer by default:

*   **JWT & Bcrypt Encryption:** Passwords are salt-hashed using bcryptjs (work factor 10). Authentication routes are protected via JWTs signed with 7-day expirations.
*   **Graceful Guest Degradation:** If API token verification fails, the custom `authMiddleware` bypasses unhandled exceptions by safely defaulting to a Guest Context (`req.user = { role: 'GUEST' }`), preserving application uptime.
*   **Dynamic Role Seeding:** During Prisma migrations, the system executes `seedDefaultUsers()`, reading the Role-Based Access Control (RBAC) blueprint to automatically provision seeded Admin and User accounts for immediate evaluation.

---

## ⚙️ Enterprise Technology Stack

| Layer | Core Technologies |
| :--- | :--- |
| **AI & Orchestration** | Python 3.12, LangGraph (StateGraph), Pydantic v2, DeepEval, Langfuse |
| **LLM Tiering** | Google Gemini (2.5-Pro / 3.5-Flash), Groq (Llama-3 70B), Custom Sparse Vectors |
| **Backend Architecture**| Node.js (18+), Express.js 4.18, Prisma ORM 5.22, PostgreSQL (PgBouncer) |
| **Frontend Synthesis** | React 18.2, Vite 5.4 (esbuild), Tailwind CSS 3.3, React Router DOM 6 |
| **QA & Telemetry** | Playwright (Headless DOM), Pytest, Flask SSE, SQLite WAL Checkpointing |
