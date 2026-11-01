# Production export and print specification — DRAFT

## Required bundle
- `ART-<id>_r<revision>_numbered_canvas.pdf` — vector outlines + numbers, physical dimensions in mm.
- `ART-<id>_r<revision>_colour_reference.pdf` — exact approved flat-colour result.
- `ART-<id>_r<revision>_palette_chart.pdf` — numbered swatches, names/SKUs, optional 2-per-A4 layout.
- `ART-<id>_r<revision>_packing_list.csv` — paint number, SKU, coverage area, estimated mL and quantity.
- `ART-<id>_r<revision>_manifest.json` — source/revision/hash/export parameters.
- Optional `numbered_canvas.svg` and tiled A4 transfer PDF for self-printing.

## Precision
Use physical page coordinates, not screen pixels. Standard canvas 300×400 mm; configurable safe margin. Include verified 100-mm calibration bar outside artwork area where possible. Preserve aspect ratio. Generate vector contours and text; do not rasterise thin linework unnecessarily.

## Print QA
Measure page boxes and dimensions with PDF tooling; verify 100% scale and printer settings. Compare generated print against a ruler; test several canvas-print providers. Verify stroke thickness/contrast experimentally (provisional 0.15–0.3 mm thin grey lines), no paint number below chosen physical legibility threshold.

## Colour chart
Support A4, two equal chart copies on one sheet, centre trim line, exact colour numbers and informative colour names/RGB metadata. Digital RGB is a preview; physical paints must use measured swatches.

## Fatal checks
Unmapped region, missing label without flagged manual handling, invalid paint SKU, export revision mismatch, incorrect physical dimensions, broken paths, empty/duplicated palette number.

## Export idempotency
Same approved revision + same export settings reuses matching previously generated bundle; retries do not overwrite approved outputs.
