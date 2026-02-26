## Construction Activity Detection using YOLOv8
What is this?
This Project develops a computer vision model to automatically detect construction activities from site images.The model is trained to recognize key activities during the early construction stages,including excavtion,reinforcement work,and concrete pouring.
The dataset ws manually labeled using Roboflow with assistance from Segment Anything Model (SAM).The trained YOLOV8 model aim to support automated monitoring of construction progress and could be integrated into future AI systems for construction management.
How to Install it?

Does it Work?


"Success Metric: Define a target. (e.g., "We aim for a Mean Average Precision ($mAP@50$) of 0.75 or higher.")
## ✅ Reproducibility Checklist
To reproduce these results, follow these steps in Google Colab:
* **Dataset:** [(https://app.roboflow.com/shaheds-workspace/construction-activities-fmp8/browse?queryText=&pageSize=50&startingIndex=0&browseQuery=true)]
* **Model Variant:** YOLOv8n (Nano)
* **Parameters:** 30 Epochs | 640 Image Size | Batch 16
* **Environment:** `ultralytics` library installed via pip.
* **Hardware:** Tesla T4 GPU (Google Colab).

Metric,Value
Precision,"[Insert Value from Member 2, e.g., 0.85]"
Recall,"[Insert Value from Member 2, e.g., 0.81]"
mAP @ 50,"[Insert Value from Member 2, e.g., 0.83]"

Goal Evaluation: Our target was a $mAP@50$ of 0.75. The final model [exceeded/met] this target, demonstrating high reliability for machinery detection in dusty site conditions.

📂 Project Structure

📁 Notebooks: Google Colab training scripts.

📁 Documentation: Contains the Error Analysis & Improvement Plan and the Governance Checklist.

📁 Results: Inference Evidence and training Metrics.
