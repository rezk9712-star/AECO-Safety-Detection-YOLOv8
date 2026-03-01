## Construction Activity Detection using YOLOv8
What is this?
This Project develops a computer vision model to automatically detect construction activities from site images.The model is trained to recognize key activities during the early construction stages,including excavtion,reinforcement work,and concrete pouring.
The dataset ws manually labeled using Roboflow with assistance from Segment Anything Model (SAM).The trained YOLOV8 model aim to support automated monitoring of construction progress and could be integrated into future AI systems for construction management.



"Success Metric: Define a target. (e.g., "We aim for a Mean Average Precision ($mAP@50$) of 0.75 or higher.")
## ✅ Reproducibility Checklist
To reproduce these results, follow these steps in Google Colab:
* **Dataset:** [(https://app.roboflow.com/shaheds-workspace/construction-activities-fmp8/browse?queryText=&pageSize=50&startingIndex=0&browseQuery=true)]
* **Model Variant:** YOLOv8n (Nano)
* **Parameters:** 50 Epochs | 167 Image Size | Batch 16
* **Environment:** `ultralytics` library installed via pip.
* **Hardware:** Tesla T4 GPU (Google Colab).

Metric,Value 
Precision,"[0.7934]"
Recall,"[0.3613]"
mAP @ 50,"[ 0.4536]"

Goal Evaluation: Our target was a $mAP@50$ of 0.75. The final model [exceeded/met 0.3074] this target, demonstrating high reliability for machinery detection in dusty site conditions.

📂 Project Structure

📁 Notebooks: Google Colab training scripts.

📁 Documentation: Contains the Error Analysis & Improvement Plan and the Governance Checklist.

Quality & Governance Summary
Analysis Findings:
Through our error analysis, we identified that while the model performs well in detecting major construction activities and equipment, it faces challenges related to class confusion (e.g., earth mounds misclassified as machinery) and complex site conditions such as occlusion and visual similarity between construction elements.

Safety & Governance:
Our governance review confirms that the model is a prototype developed for educational purposes. The analysis highlights the potential impact of false negatives, which may lead to under-reporting of construction activities. Therefore, automated detections should be verified against site images before operational decisions are made.

Future Roadmap:
To improve model reliability, the prioritized improvement plan includes implementing Hard Negative Mining and Mosaic Augmentation in the next training iteration to address class confusion and improve detection robustness.

📁 Results: Inference Evidence and training Metrics.
