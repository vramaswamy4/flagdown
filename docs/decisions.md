# Decision log

One entry per non-obvious choice. Newest first. Format: what, why, what it was chosen over, what would change the decision.

## 2026-09-18 — Frontend rebuild waits for extension 5
**What:** Leave the starter's Next.js frontend alone during the course build. The UI work happens in extension 5 (live map), and it adds routes rather than rewriting the instructor's components in place.
**Why:** There is no backend yet, so "preserve functionality" isn't verifiable — a rewrite now would be blind, and breakage would surface mid-lecture with no way to tell whose bug it is. The course also edits these files as it goes, so a revamped tree would diverge from every video. Extension 5 does the same work once there's real data to show, which is the better story anyway: a map fed by my own Kafka→Redis pipeline, not a restyle of someone else's.
**Over:** Revamping the UI now, before writing any Go.
**Would change if:** the course turns out barely to touch the frontend — then a restyle could land earlier without fighting the videos.

## 2026-09-18 — Keep the module path as `ride-sharing` until the course build is done
**What:** go.mod stays `module ride-sharing` rather than `github.com/vramaswamy4/ride-dispatch` for now.
**Why:** Renaming means rewriting every import in the starter plus every import I type across 20 hours of video, so my files would permanently disagree with what's on screen — bad import paths are a poor thing to debug while I'm still learning Go. Nothing here needs `go get`; it's a service monorepo, not a library.
**Over:** Renaming now for a tidier-looking public repo.
**Would change if:** I publish a package here for others to import. Otherwise it's a one-time mechanical change after the course build.

## 2026-09-18 — Vendor the course starter instead of forking it
**What:** Copied codealong-dev/microservices-go-starter in as ordinary files — no fork, no upstream remote, no imported history. My README kept, .gitignore merged. Attribution for the starter is still owed in the README.
**Why:** A fork brands the repo "forked from" and frames the work as someone else's with my patches on top; I want the history to be mine from the first commit. The starter also ships with no LICENSE, which makes it all-rights-reserved by default — my MIT covers my work, not the vendored files, and the README needs to say so plainly.
**Over:** Forking on GitHub, or adding the starter as a second remote and merging its history in.
**Would change if:** the instructor starts shipping starter fixes I need — a remote to cherry-pick from would then be worth the messier history.

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
