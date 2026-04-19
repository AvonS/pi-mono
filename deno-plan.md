## 📄 Download PI-LoYo PRD (Markdown)
------------------------------
## PRD: PI-LoYo (The Anti-YOLO Agentic Engine)## Vision
To evolve the pi-mono agentic core into a "Security-First" tiered architecture. PI-LoYo (Low-YOLO) prioritizes Zero Trust, ensuring that even if an LLM is compromised via prompt injection, it physically cannot exfiltrate secrets or damage the host system.
------------------------------
## Phase 1: Single-User Hardened Foundation## Objective
Migrate the monolithic pi-mono execution model into a local 3-tier structure using the Deno runtime.
## 1. Tier 1: The Orchestrator (The Vault)

* Role: Holds the Master API keys (OpenRouter) and RBAC policies.
* Runtime: Deno with --allow-run and --allow-read (config only).
* Communication: Spawns Tier 3 via Deno.Command.
* Safety: Never exposes API keys to the environment variables of child processes.

## 2. Tier 2: The Gateway (The Router)

* Role: Handles multi-channel I/O (Terminal WebSocket, Slack Webhook).
* Normalization: Translates platform-specific events into a Canonical Message Model.
* Runtime: Deno with --allow-net restricted to Slack/OpenRouter.

## 3. Tier 3: The Agent (The Hands)

* Role: Executes the pi-mono agent loop and tool calls.
* Isolation: Runs in an ephemeral OS process with --deny-all by default.
* CRUD Limits: --allow-read/write restricted strictly to a dedicated workspace folder.
* Tooling: High-risk bash commands are wrapped in bwrap (Linux) or sandbox-exec (macOS).

------------------------------
## Phase 2: Multi-User Distributed Orchestration## Objective
Scale PI-LoYo to support multiple concurrent users on a single VPS with total isolation and persistent state.
## 1. Multi-Tenancy

* Session Registry: Tier 1 uses Deno KV to track active users, roles, and session IDs.
* Ephemeral Lifecycle: Tier 1 provisions a fresh workspace and Tier 3 process for every session, destroying them upon logout/timeout.

## 2. Secure Inter-Process Communication (IPC)

* Unix Sockets: Tier 1, 2, and 3 communicate via dedicated Unix Domain Sockets with file-level permissions (chmod 600).
* Heartbeat: Tier 1 monitors Tier 3 liveness. Missing heartbeats trigger an immediate process kill to prevent "Ghost Agents."

## 3. Canonical Translation & Streaming

* Chunked Middleware: Tier 2 buffers streaming tokens from Tier 1 to accommodate Slack/Teams rate limits while maintaining 0ms latency for Terminal clients.
* RBAC Guard: Tier 1 intercepts all Tool Call fragments. Execution is paused until Tier 1 validates the call against the User's role or requests Human-in-the-Loop (HITL) approval. For high-risk operations, this can include interactive approval prompts (similar to sudo) with timeout-based permissions.

------------------------------
## Success Metrics

   1. Zero-Leak Guarantee: An agent in Tier 3 cannot read the Orchestrator's .env file even with a successful prompt injection.
   2. Fault Tolerance: A crash in one user's Tier 3 process has zero impact on other users.
   3. Cross-Platform: The Launcher supports Linux (bwrap) and macOS (sandbox-exec) with a unified Deno codebase.

------------------------------

https://extism.org/