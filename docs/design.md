System Design — BananaPeel
A) Context & Goals

Summary diagram (boxes & arrows)

B) Detailed Components

Client: canvas engine, mask application, transforms, z-order

LLM Orchestrator: prompt templates, grounding strategy

Segmentation: model choice, hardware, batching, warmup

API: endpoints, auth, rate limits

Data: object store layout, TTLs

C) Flows

NL → plan → ground → SAM → actions → apply

Manual box/points → SAM → mask → layer

D) Performance Planning

Budget per step (ms)

Caching layers/masks; RLE vs. PNG payloads

E) Security & Privacy

Data paths; encryption; logging policy; PII

F) Ops

CI/CD, canary, blue/green

Observability (metrics, traces, logs)
