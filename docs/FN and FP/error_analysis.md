# Error Analysis – Construction Activities YOLOv8 (FMP8 v2)

## Root Cause Legend

| Code | Meaning |
|------|---------|
| **Occlusion** | Construction elements or machinery are partially hidden by other site objects or structural components |
| **Scale Variance** | The object is too small or too far from the camera |
| **Illumination** | Lighting conditions such as shadows, fog, snow, or low light reduce visual clarity |
| **Class Confusion** | Objects with similar textures, colours, or shapes are visually difficult to distinguish |
| **Box Mismatch** | Predicted bounding box covers the correct region but does not sufficiently overlap the GT annotation (IoU < threshold) |

---

## False Positive (FP) Analysis

### FP-1 — `fp_01_excavator.png`
- **Predicted Class:** Excavator (conf: 0.75)
- **Actual Object:** Excavator correctly present, but predicted box does not sufficiently overlap GT annotation
- **Root Cause: Box Mismatch.** The model predicts a large box covering the full machine body including tracks, while the GT annotation covers only the upper arm/boom region. IoU falls below the match threshold so the prediction is counted as a false positive despite the excavator being genuinely present.

### FP-2 — `fp_02_excavator.png`
- **Predicted Class:** Excavator (conf: 0.63)
- **Actual Object:** Excavator bucket/upper structure — partially cut off at frame edge under snowy conditions
- **Root Cause: Illumination + Box Mismatch.** Heavy snowfall and grey overcast lighting desaturate the excavator's yellow paint. The model detects only the upper cab/bucket portion visible in the top-right corner as a separate instance, while the GT annotation covers the full machine — producing a low-IoU mismatch counted as FP.

### FP-3 — `fp_03_concrete.png`
- **Predicted Class:** Concrete (conf: 0.46)
- **Actual Object:** Snow-covered frozen ground / icy surface in the bottom-left corner of a winter excavation scene
- **Root Cause: Class Confusion + Illumination.** The pinkish-grey tone of snow-covered wet ground closely matches the colour distribution of concrete in the training set. The flat, smooth surface texture further reinforces the Concrete class signal, triggering a spurious low-confidence detection with no GT match.

---

## False Negative (FN) Analysis

### FN-1 — `fn_01_excavation.png`
- **Missed Class:** Excavation
- **Condition:** Dramatic warm sunset backlight; excavation pit occupies the lower foreground
- **Root Cause: Illumination.** Intense orange-yellow backlighting washes out depth cues and shadow contrast in the lower frame. The model correctly detects the excavator above but fails to fire on the earthworks pit beneath it, as the warm light fills the normally-dark pit region.

### FN-2 — `fn_02_excavator.png`
- **Missed Class:** Excavator (upper boom/arm sub-region)
- **Condition:** Same sunset scene; GT box covers only the arm — a tight sub-region of the full machine
- **Root Cause: Scale Variance + Box Mismatch.** The GT label annotates only the boom of the excavator as a separate tight box. The model's prediction covers the full machine body, so the tight sub-region GT annotation is unmatched (IoU too low), registering as a false negative even though the machine is clearly detected.

### FN-3 — `fn_03_excavation.png`
- **Missed Class:** Excavation
- **Condition:** Snowy winter construction site; deep trench clearly visible in the centre of frame
- **Root Cause: Illumination + Class Confusion.** Snow coverage whites out the normally-dark soil edges that define an excavation pit. The model predicts Excavation for a smaller left-side region and detects the excavator and concrete, but the main large GT excavation box in the centre goes unmatched — snow-blanketed trench walls lose the textural contrast the model expects.

---

## Prioritised Next Data Improvements

### Improvement 1 (Highest Priority) — Add Adverse Weather Training Data
- **Addresses:** FP-2, FP-3, FN-3
- **Why:** Three of six failures occur in snowy or overcast conditions. The model has not been trained on construction sites under snow, causing missed detections and spurious Concrete/Excavation predictions on snow-covered surfaces.
- **Action:** Collect 40+ images of excavation sites, excavators, and concrete work under snow, rain, and overcast lighting. Apply weather augmentations (fog overlay, brightness reduction, desaturation) in Roboflow for all affected classes.

### Improvement 2 — Standardise Bounding Box Annotation Protocol
- **Addresses:** FP-1, FP-2, FN-2
- **Why:** Several FP/FN pairs arise from the same object being detected correctly but counted wrong due to GT box inconsistency — sometimes the full machine is annotated, sometimes only a sub-component (arm, bucket). This artificially inflates both FP and FN counts.
- **Action:** Define a clear annotation rule: always annotate the full visible extent of each object instance. Re-review and re-label all existing excavator annotations for consistency before the next training run.

### Improvement 3 — Hard Negative Mining for Snow/Concrete Confusion
- **Addresses:** FP-3
- **Why:** Snow-covered icy ground is being mistaken for Concrete at low confidence. The model needs explicit negative examples showing snow and ice with no Concrete label.
- **Action:** Add 25+ images of snow-covered ground, frozen puddles, and icy surfaces as background-only (no Concrete label). Additionally apply Hue/Saturation augmentation to existing Concrete training images to make the class boundary more colour-invariant.
