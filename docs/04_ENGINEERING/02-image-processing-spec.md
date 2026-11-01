# Image processing engine specification — v0.1 DRAFT

## Input
Original image bytes (JPG/PNG/WebP), crop transform, physical width/height mm, palette configuration, desired colour count, difficulty, minimum paintable area and min-width guidance, optional important-object masks.

## Algorithm stages
1. Validate/decode, apply EXIF orientation; immutable source record and hash.
2. Crop/resample to resolution suited to physical target; preserve aspect and subject.
3. Preprocess noise/lighting conservatively; record exact settings.
4. Optional AI semantic segmentation (SAM-family masks) to protect subject/object boundaries. **Do not infer perfect masks or use it as ground truth.**
5. Apply quantisation in perceptual space; match against available physical paint swatches (CIELAB/ΔE candidate selection). May first use unrestricted colour clusters for preview, then constrain to inventory.
6. Generate spatial superpixels / region proposal (SLIC or comparable); construct region adjacency graph.
7. Merge small/narrow regions using colour difference + edge and semantic penalties. Ensure no pixel remains unlabeled.
8. Assign globally stable numbered colours; normalize connected components, contour vectorization and smoothing without topological loss.
9. Place numbers using distance-transform max-inscribed positions, never just centroid; multiple candidate positions when region concave; flag labels that cannot fit physically.
10. Validate geometry, colour map, region IDs, paint inventory; export all files from same frozen revision.

## Invariants
- Each printable pixel belongs to exactly one region (within print bleed/margins).
- Each region has one valid paint SKU and label number.
- Neighbouring regions share a boundary; no gaps due to independent contour rounding.
- Manual merge operations are reversible and auditable; updates are atomic.
- Seed/model version/config hash stored for reproducibility; don't promise bit-identical GPU results without verified determinism.

## Paintability heuristics
Physical area in mm², minimum inscribed diameter, thin-branch width, complexity/perimeter-to-area, label fit and isolated island count. Small/wrong regions produce warnings and repair suggestions.

## Quality evaluation dataset
At least 6 source images of different kinds: portrait, pet, landscape, architecture, low contrast, high detail. For each compare: visual likeness, count of unpaintable regions, total repairs, approval time, printed number visibility and physical colour discrepancy.

## Failure handling
Failed model unavailable → classical fallback where quality adequate; otherwise mark review required. No fabricated AI completion states or silent photo alteration.
