# Data model and provenance — DRAFT

## Entities
**users** (later multi-user), **projects**, **source_images**, **generation_runs**, **artwork_revisions**, **regions**, **paint_skus**, **revision_palette**, **validation_reports**, **export_bundles**, **audit_events**.

## Suggested fields
- `projects`: id UUID, owner_id nullable for single-user local mode, name, status, created_at, updated_at.
- `source_images`: id, project_id, storage_key, sha256, width_px, height_px, exif_orientation, consent_context, created_at.
- `generation_runs`: id, project_id, source_image_id, parameters JSONB, model_id, seed, state, started_at, completed_at, error_code, config_hash.
- `artwork_revisions`: id, project_id, generation_run_id, revision_no, stage (`draft|validated|approved`), previous_revision_id, created_at, approved_at.
- `regions`: revision_id, region_id, paint_sku_id, number, contour/mask reference, area_mm2, min_width_mm, label_x_mm, label_y_mm, warnings JSONB.
- `paint_skus`: id, supplier, name, observed_dry_lab, sample_method, available, calibrated_at.
- `revision_palette`: revision_id, paint_sku_id, label_number, coverage_mm2, estimated_ml.
- `validation_reports`: revision_id, errors/warnings JSONB, checker_version, checked_at.
- `export_bundles`: id, revision_id, settings_hash, manifest JSONB, storage_key, created_at.

## Relations and constraints
Source images are immutable. Unique `(project_id, revision_no)`. Unique `(revision_id, label_number)` palette mapping. Every region colour must FK to revision palette SKU. Approved revision cannot mutate; editing creates draft child revision.

## Storage constraints
Large region masks and vector contours stored as compressed objects with checksums; database stores path and metadata. Use private bucket and signed URLs only. Include purge rules for deleted uploads.

## Future business entities
orders, order_items, inventory_lots, packing_jobs, shipments, customer_approvals (not P0).
