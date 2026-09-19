# Roadmap

- [ ] Course build: gateway, trip, driver services, gRPC, async messaging, Kubernetes (target: 2026-09-21)
- [ ] README: services table + architecture, and starter attribution (starter has no LICENSE)
- [ ] CI: vet, test, build on every push
- [ ] Prometheus + Grafana
- [ ] Chaos test: pod kill mid-ride
- [ ] Driver-location pipeline: Kafka → consumer → Redis geo → nearest-driver matching
- [ ] Live map: moving drivers, H3 demand heatmap, trip replay
- [ ] Ops panel in the UI (lag, matches/min, p95 match latency)
