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

ID,Type,Observation (What),Root Cause Hypothesis (Why)

FP1,False Positive,Dirt Mound as Excavator: Model predicted an excavator with 0.75 confidence over a mound of earth.,"Texture Confusion: The jagged, brownish edges of the loose soil mimic the complex metallic silhouette of an excavator bucket or tread."

FP2,False Positive,Snow/Sky as Excavator: Model predicted an excavator in a blank area of snow and sky.,"Contrast Glitch: High-exposure white areas (snow/clouds) can create ""phantom"" edges that the model mistakenly associates with a machine's frame."

FP3,False Positive,"Snow/Dirt as Concrete: Model predicted a ""Concrete"" box on an area of dirty snow.","Color Invariance: The grayish-white mixture of snow and mud shares a similar color profile to wet concrete, causing class confusion."

FN1,False Negative,Ground-truth Excavation Missed: The model failed to detect a clearly visible excavation pit.,"Depth Variance: The model struggles to recognize the ""negative space"" of a pit when the lighting is flat or the color matches the surrounding terrain."


FN2,False Negative,Ground-truth Excavator Missed: A large excavator was completely ignored by the model.,Scale Variance: The object was likely too large for the YOLOv8n (nano) anchor boxes to process when positioned too close to the camera.

FN3,False Negative,Snow-Covered Excavation Missed: The model missed a pit because it was covered in snow.,"Occlusion/Atmosphere: The layer of snow changed the visual ""signature"" of the excavation class, 
which was likely trained on dry dirt examples."
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

