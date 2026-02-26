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
Precision,"[]"
Recall,"[]"
mAP @ 50,"[]"

Goal Evaluation: Our target was a $mAP@50$ of 0.75. The final model [exceeded/met] this target, demonstrating high reliability for machinery detection in dusty site conditions.

📂 Project Structure

📁 Notebooks: Google Colab training scripts.

📁 Documentation: Contains the Error Analysis & Improvement Plan and the Governance Checklist.

Quality & Governance Summary
Analysis Findings: Through our Error Analysis, we identified that while the model is highly effective at detecting large machinery, it faces specific challenges with Class Confusion (mistaking earth mounds for equipment) and Occlusion (workers obscured by structural elements).

Safety & Governance: Our Governance Checklist confirms that the model is a prototype for educational purposes and highlights the critical safety risk of False Negatives in high-stakes AECO environments.

Future Roadmap: To increase reliability, our prioritized improvement plan recommends implementing Hard Negative Mining and Mosaic Augmentation in the next training iteration.

📁 Results: Inference Evidence and training Metrics.
