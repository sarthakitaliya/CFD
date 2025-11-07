# CFD Trading Platform (Exness‑style)

A compact CFD trading platform: leveraged long/short, instant order execution, margin + liquidation logic, and real‑time P/L.

## Features
- Live price feed (poller) and instant execution
- Long/short positions with leverage and margin checks
- Real-time balance, equity, P/L, and liquidation
- Order history, position snapshots, and audit trail
- Pluggable storage (DB store service) and queue-based processing

## Architecture
![Architecture](./architecture.png)

**Flow (high level):**
1. **Backend** receives orders and pushes messages to **queue**.
2. **Engine** consumes from queue, validates margin, executes trades, updates positions, and emits events.
3. **DB Store Service** persists events and state deltas to the **DB**.
4. **Snapshot** service consumes engine state to keep fast read models fresh.
5. **Poller** feeds prices to the central queue; engine uses these for P/L + liquidation.
6. Backend ACKs processed messages and returns results to clients.

## Tech Stack
- Backend/API: Node.js/Express
- Engine: TypeScript
- Queue: Redis Streams
- DB: Postgres (asset, closed orders)
