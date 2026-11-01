# Security and customer photo privacy — DRAFT

## Threat model
Personal photos, potentially including children and identifiable family members; malicious image uploads; forged ownership; leaked signed URLs; unsafe SVG/PDF; denial-of-service through oversized photos or expensive AI inference; prompt injection if text/LLM integration is added later.

## Controls
- MIME magic/decoder verification, max bytes/pixels, pixel decompression limits, strip unsafe metadata from derivative images.
- Process user media in isolated worker with resource/time limits; patch image-processing libraries.
- Private storage, short-lived scoped read URLs, separated original/derivatives, no public index or raw photo logging.
- Authentication and object-level authorization in all API endpoints before multi-user launch.
- Server-generated file names and safe export paths; no arbitrary user paths; sanitize SVG content.
- Secure upload with CSRF/session protections as appropriate, authenticated endpoints, rate limiting and job concurrency quotas.
- No external model/API calls without explicit customer notice and clear data handling arrangements.
- Deletion workflow removes originals, derivatives, caches and backups per defined retention rules; confirm legal policy before launch.
- Treat customer-photo use for marketing as opt-in, separate from upload-processing consent.

## Internal engineering practices
Secrets from env/secret manager only; no passwords/API keys in repo, logs or client bundle. TLS, least-privilege database role, migration review, dependency scanning and backups.

## Privacy disclosure
Before commercial launch, write a legally reviewed photo-handling and retention notice, especially for identifiable portraits. Product policy must reflect actual backend behaviour, not aspirations.
