# Non-functional requirements and production-readiness gates — DRAFT

## MVP requirements
- Asynchronous generation with explicit states and job retry/cancellation where supported.
- Detect duplicate generation requests; idempotent approval and export.
- Upload/file limits; worker memory/CPU ceilings and processing timeouts.
- Basic access logging with request IDs, privacy-safe job error logs, metrics for completion/failure/duration.
- Database migrations tested; connection pooling sized for actual deployment; indexes on project/owner/job state; backup/restore tested before business launch.
- API handles invalid inputs without crashing; long tasks do not hold synchronous web requests open.
- Visual performance: responsive previews with progressive image loading; benchmark on typical machines before setting fixed SLA.

## Deferred until justified
Load balancing, Kubernetes, sharding, multi-region active-active, service mesh, complicated circuit-breaker network, chaos experiments, sophisticated distributed tracing and SLO/error-budget programmes. They are real engineering subjects, not all prerequisites for an internal MVP.

## Release evolution
**Private MVP:** instrumentation + backups + secure uploads + QA + simple worker.
**Paid pilot:** audit logs, signed URLs, alerts, fulfilment tracing, recovery rehearsal, monitoring dashboards, clear SLIs/SLOs.
**Growth:** autoscaling workers, queue backpressure, CDN for public non-private assets, circuit breakers around external vendors and advanced operations as measured.

## Scalability questions
How many photos/day? Model runtime on target CPU/GPU? Average source pixel count? What percentage need manual repair? What is largest safe job batch? Decide using measurements, not social-media checklist cargo cult.
