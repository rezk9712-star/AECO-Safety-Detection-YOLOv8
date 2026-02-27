# Construction Activities Detection – YOLOv8 (FMP8 v2)

## Project Overview
This project trains a YOLOv8 object detection model to recognize construction activities using a Roboflow dataset with 9 classes: Concrete, Concrete Float, Concrete Mixer Truck, Concrete Pump Hose, Concrete Pump Truck, Concrete Trowel, Excavation, Steel-Reinforcement, and Excavator.

---

## Final Model Metrics (YOLOv8m – Validation Set)

| Metric              | Value  |
|---------------------|--------|
| **Mean Precision**  | 0.7934 |
| **Mean Recall**     | 0.3613 |
| **mAP50**           | 0.4536 |
| **mAP50-95**        | 0.3074 |

### Per-Class Breakdown

| Class                | Precision | Recall | mAP50  | mAP50-95 |
|----------------------|-----------|--------|--------|----------|
| Concrete             | 0.817     | 0.769  | 0.772  | 0.578    |
| Concrete Pump Hose   | 1.000     | 0.000  | 0.017  | 0.002    |
| Concrete Pump Truck  | 0.999     | 0.667  | 0.913  | 0.575    |
| Excavation           | 0.432     | 0.154  | 0.205  | 0.069    |
| Steel-Reinforcement  | 0.602     | 0.123  | 0.200  | 0.123    |
| Excavator            | 0.910     | 0.455  | 0.616  | 0.496    |

---

## Evidence

### Validation Set Predictions
Screenshots of predictions on the validation set are located in:
```
/results/evidence/val_predictions/
```
- `val_pred_01.png` through `val_pred_10.png` — 10 screenshots of bounding box predictions on validation images.

### New/Unseen Image Predictions
Screenshots of predictions on new/unseen images are located in:
```
/results/evidence/unseen_predictions/
```
- `unseen_pred_01.png` through `unseen_pred_05.png` — 5 screenshots of predictions on images the model has never seen.

---

## How to Reproduce Inference

### 1. Setup
```python
from ultralytics import YOLO

# Load the best trained model
model = YOLO('/content/runs/detect/train2/weights/best.pt')
```

### 2. Predict on Validation Set (10 screenshots)
```python
import os
import glob
from IPython.display import Image, display

val_img_dir = '/content/Construction-Activities---FMP8-2/valid/images/'
val_images = glob.glob(os.path.join(val_img_dir, '*.jpg'))[:10]

os.makedirs('/results/evidence/val_predictions', exist_ok=True)

for i, img_path in enumerate(val_images, 1):
    results = model.predict(img_path, conf=0.25, save=True,
                            project='/results/evidence/val_predictions',
                            name=f'val_pred_{i:02d}')
    # Display inline
    display(Image(img_path))
    print(f"Screenshot {i}: {os.path.basename(img_path)}")
    for r in results:
        for box in r.boxes:
            cls_name = model.names[int(box.cls)]
            conf = float(box.conf)
            print(f"  → {cls_name}: {conf:.2f}")
```

### 3. Predict on New/Unseen Images (5 screenshots)
```python
# Place 5 new images in a folder, or use URLs
new_images = [
    'path/to/new_image_1.jpg',
    'path/to/new_image_2.jpg',
    'path/to/new_image_3.jpg',
    'path/to/new_image_4.jpg',
    'path/to/new_image_5.jpg',
]

os.makedirs('/results/evidence/unseen_predictions', exist_ok=True)

for i, img_path in enumerate(new_images, 1):
    results = model.predict(img_path, conf=0.25, save=True,
                            project='/results/evidence/unseen_predictions',
                            name=f'unseen_pred_{i:02d}')
    display(Image(img_path))
    print(f"Unseen image {i}: {os.path.basename(img_path)}")
    for r in results:
        for box in r.boxes:
            cls_name = model.names[int(box.cls)]
            conf = float(box.conf)
            print(f"  → {cls_name}: {conf:.2f}")
```

### 4. Save screenshots programmatically (Colab)
```python
# In Google Colab, use this to save annotated prediction images
results = model.predict(source=val_img_dir, conf=0.25, save=True,
                        project='/results/evidence',
                        name='val_predictions')

# The annotated images are automatically saved in the output folder
# Download them via: files.download('/results/evidence/val_predictions/')
from google.colab import files
import zipfile

with zipfile.ZipFile('/results/evidence.zip', 'w') as zf:
    for root, dirs, filenames in os.walk('/results/evidence'):
        for fname in filenames:
            zf.write(os.path.join(root, fname))

files.download('/results/evidence.zip')
```

---

## Folder Structure
```
/results/
└── evidence/
    ├── val_predictions/
    │   ├── val_pred_01.png
    │   ├── val_pred_02.png
    │   ├── ...
    │   └── val_pred_10.png
    └── unseen_predictions/
        ├── unseen_pred_01.png
        ├── unseen_pred_02.png
        ├── ...
        └── unseen_pred_05.png
```

---

## Notes
- Model: YOLOv8m (medium), trained for 50 epochs on Roboflow dataset `construction-activities-fmp8 v2`
- Dataset: 125 train images, 25 validation images
- Hardware: Google Colab T4 GPU
- Best weights: `/content/runs/detect/train2/weights/best.pt`

[README.md](https://github.com/user-attachments/files/25616954/README.md)
