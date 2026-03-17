# Mermaid Diagram Templates

## 1) Architecture / Data Flow

```mermaid
flowchart LR
  A[Producer] -->|OTLP| B[Collector]
  B --> C[Backend A]
  B --> D[Backend B]
```

Use for: platform architecture, observability pipelines, integration maps.

## 2) Sequence

```mermaid
sequenceDiagram
  participant App
  participant Collector
  participant Backend
  App->>Collector: export traces/metrics/logs
  Collector->>Backend: forward via exporter
  Backend-->>App: query/visualization available
```

Use for: runtime interactions, request lifecycle, protocol handshake.

## 3) State

```mermaid
stateDiagram-v2
  [*] --> Collecting
  Collecting --> Buffering: backend unavailable
  Buffering --> Retrying
  Retrying --> Collecting: connection restored
  Retrying --> Failed: retry budget exceeded
```

Use for: collector lifecycle, retry/error behavior, control loops.

## 4) Dependency / Ownership

```mermaid
flowchart TB
  O[OpenTelemetry Standard]
  O --> C[Collector Distribution]
  O --> E[Exporter Support]
  O --> B[Backend Ingestion]
```

Use for: support boundaries, standards alignment, capability mapping.

## 5) Styling for Readability

When using custom fill colors, always set an explicit text `color` so nodes stay readable in both light and dark themes.

```mermaid
flowchart LR
  A[Default] --> B[Highlighted]
  A --> C[Accent]

  style B fill:#cce5ff,color:#333,stroke:#333
  style C fill:#ffe0cc,color:#333,stroke:#333
```

For reusable classes:

```mermaid
flowchart LR
  A[Node 1]:::info --> B[Node 2]:::ok

  classDef info fill:#cce5ff,stroke:#333,color:#333
  classDef ok   fill:#c5f5c5,stroke:#333,color:#333
```

Rule: every `fill` must be paired with a contrasting `color`. Light fills get `color:#333`; dark fills get `color:#fff`.
