---
name: svg-post-cleanup
description: Clean and conservatively compress standalone SVG assets after export. When a commit or PR adds or modifies SVGs, ask the user whether to run this cleanup first. Invoke only after user confirmation or an explicit cleanup request; do not trigger automatically.
---

# SVG post-export cleanup

Run only when the user has confirmed or explicitly requested cleanup. Apply to added or modified SVGs; leave unrelated assets alone.

1. Keep originals for comparison. Check paths for invalid coordinates such as `NaN` or `Infinity`; XML parsing alone will not catch them. Repair only from a reliable reference, then visually verify the repair.
2. Preserve root `xmlns` and `viewBox`. For scalable icons, omit fixed root `width`/`height` unless consumers require them. Keep nested viewport sizing, `preserveAspectRatio`, overflow, backgrounds, fills, strokes, opacity, gradients, masks, clips, and transforms.
3. Compress conservatively: remove exporter comments, editor metadata, unnecessary inter-element whitespace, and unreferenced layer IDs. **For Figma exports, explicitly remove Figma-only layer information:** unreferenced layer `id` attributes, layer-name annotations, and editor-only metadata/attributes. Keep structural groups and preserve accessibility titles/descriptions and IDs or attributes needed by rendering, CSS, ARIA, links, or animation. Avoid default optimizer presets, precision reduction, path merging, transform flattening, and structural group removal.
4. Namespace retained functional IDs with variant/product/size prefixes and update every reference. Verify references resolve and IDs do not collide when the assets are inlined together.
5. For standalone vector icons, verify no external resources, embedded raster payloads, `<image>`, scripts, or exporter style blocks remain. Do not silently strip content that affects appearance or accessibility; resolve it before submission.
6. Run `xmllint --noout` (or an XML parser), check references and finite coordinates, and compare before/after renders at source size and 256×256. Cleanup should be pixel-identical; intentional artwork repairs need a separate visual check against the reference. Report byte savings and checks actually performed. Keep previews local unless requested in the PR.
