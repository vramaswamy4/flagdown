# Decision log

One entry per non-obvious choice. Newest first. Format: what, why, what it was chosen over, what would change the decision.

## 2026-09-17 — Follow the course structure first, extend second
**What:** Build the course project as designed (gateway, trip, driver; gRPC sync, RabbitMQ async; Kubernetes) before adding anything.
**Why:** Learning Go and the service layout at the same time as designing new features spreads effort thin. A working baseline makes each extension a measurable diff.
**Over:** Designing my own domain from scratch.
**Would change if:** the course's structure turns out to block an extension (e.g. its messaging layer can't coexist with Kafka).

## 2026-09-17 — Kafka for driver locations, RabbitMQ for everything else
**What:** Driver position updates go through Kafka; command-style events (trip requested, driver assigned) stay on RabbitMQ as the course builds them.
**Why:** Locations are a high-volume, replayable stream where consumers want to re-read history (map replay, heatmap). Commands are low-volume, routed work items. Two brokers is the honest fit, and it lets me speak to both in interviews.
**Over:** Replacing RabbitMQ with Kafka wholesale.
**Would change if:** operating two brokers on the cluster proves too heavy for a laptop.
