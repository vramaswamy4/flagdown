# ride-dispatch

Ride-sharing backend built as Go microservices on Kubernetes: an API gateway, trip and driver services talking over gRPC, and asynchronous events between them. Built while working through Tiago Taquelim's *Complete Microservices with Go*, then extended with a real-time driver-location pipeline (Kafka, Redis geo), a live map, and production plumbing (CI, Prometheus/Grafana, chaos tests).

> Status: in progress. Course build first, extensions after. See [docs/roadmap.md](docs/roadmap.md).

## Architecture

_Diagram and service list land here once the first services exist._

## Running locally

```bash
# prerequisites: Go 1.23+, Docker, kubectl, kind (or minikube), tilt
make dev
```

## Services

| Service | Language | Talks to | Purpose |
|---|---|---|---|
| gateway | Go | trip, driver (gRPC), browser (HTTP + WebSocket) | Single public entry point |
| trip | Go | driver (gRPC), broker | Trip lifecycle: request, match, complete |
| driver | Go | broker | Driver availability and location |

## Decisions

Every non-obvious choice is written down in [docs/decisions.md](docs/decisions.md): what was decided, what it was decided against, and why.

## Extensions beyond the course

1. **CI** on every push (GitHub Actions: vet, test, build images).
2. **Observability**: Prometheus metrics on every service, one Grafana dashboard (request rate, error rate, match latency, consumer lag).
3. **Chaos test**: kill a pod mid-ride and prove the system recovers.
4. **Driver-location pipeline**: drivers publish positions to Kafka, a consumer maintains a Redis geo index, matching queries nearest available drivers, the gateway fans updates out over WebSocket.
5. **Live map**: moving drivers, an H3 demand heatmap, and a trip replay scrubber.

## Author

Vinith Ramaswamy · [LinkedIn](https://www.linkedin.com/in/vinith-ramaswamy-6a5964200) · [justbookapp.com](https://justbookapp.com)
