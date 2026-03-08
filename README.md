# Shelf Product Detection (YOLOv8m)

## 📋 Model Overview
The model was trained using the **YOLOv8 medium architecture (YOLOv8m)** from the Ultralytics framework. A pre-trained COCO model was used as the initialization checkpoint, enabling transfer learning to accelerate convergence. The model is fine-tuned to detect **16 different products** across various packaging sizes within the Nestlé Dairy portfolio.

## 📊 Data Splitting
The dataset was partitioned to ensure robust validation on unseen data:
* **Train:** 2,589 images (Used to train the model)
* **Valid:** 457 images (Used for final accuracy reporting and error analysis)

## 🛠️ Methodology & Hyperparameters

### Image Resolution (`imgsz = 768`)
A resolution of **768×768** was selected to improve detection of small packaging text and fine logo details that differentiate products with similar appearance.

### Loss Function Weights
* **Box Loss (7.5):** Increased to prioritize accurate localization.
* **Classification Loss (1.0):** Increased slightly to improve distinction between the 16 size variants.
* **Distribution Focal Loss (1.5):** Improves bounding box regression precision.
* **Label Smoothing (0.03):** Reduces overconfidence to improve generalization.

### Data Augmentation Strategy
* **Mosaic (0.8):** Combines four images to improve generalization.
* **MixUp (0.3):** Blends two images to introduce diversity without degrading text visibility.
* **Scale (0.5):** Robustness to products appearing at different distances.
* **Perspective (0.0005):** Simulates minor camera angle variations.

---

## 📈 Evaluation 
The following performance was achieved on the validation set:

| Metric | Value |
| :--- | :--- |
| **Precision** | 90.3% |
| **Recall** | 91.7% |
| **mAP@0.50** | 93.5% |
| **mAP@0.50:0.95** | 81.9% |

### Per-Class Performance (mAP50)
| Class | mAP50 |
| :--- | :--- |
| NESTLE NIDO FCMP(800GM) | 88.7% |
| NESTLE NIDO FCMP(375GM) | 87.0% |
| NIDO PREBIO 1+ TIN(1800GM) | 86.8% |
| NESTLE NIDO 1+(150GM) | 86.5% |
| NESTLE BUNYAD(900GM) | 86.3% |
| NESTLE BUNYAD(260GM) | 84.2% |
| NESTLE NIDO SCHOOL AGE(200GM) | 84.0% |
| NESTLE NIDO SCHOOL AGE(900GM) | 83.6% |
| NESTLE NIDO SCHOOL AGE(390GM) | 83.2% |
| NESTLE BUNYAD(600GM) | 83.0% |
| NESTLE NIDO 3+(150GM) | 81.9% |
| NESTLE NNS 3+ BOX(375GM) | 78.7% |
| NESTLE NNS 1+ BOX(375GM) | 77.6% |
| NESTLE NNS 3+ BOX(800GM) | 75.7% |
| NESTLE NNS 1+ BOX(900GM) | 75.3% |
| NESTLE NIDO SCHOOL AGE(650GM) | 67.6% |

---

## 🔍 Error Analysis
The analysis shows that errors are concentrated in three specific **"Similarity Clusters"** where packaging design is nearly identical across different volumes:

1. **The "Size Swap" (BUNYAD):** Frequent confusion between BUNYAD 600g and 900g.
2. **"NNS Box":** Cross-classification between NNS 1+ and NNS 3+ variants (75-78%). The boxy shapes are detected easily, but the model struggles with stage branding.
3. **The "School Age" Overlap:** The **650g** variant (67.6%) is the weakest class. It is frequently misidentified as either 390g or 900g in crowded shelf conditions.

### Precision-Recall Strategy
A strategic decision was made to **prioritize Recall over Precision** to ensure maximum product detection on the shelves.
* **Top Performers:** NIDO FCMP (800g) and PREBIO 1+ TIN (1800g) lead with individual AP scores of **0.987**.
* **Primary Challenge:** NIDO SCHOOL AGE (650g) remains the outlier at **0.797 AP**.

---

## 🚀 Conclusion & Future Improvements
The model performs well across most product classes; however, minor confusion occurs between visually similar packaging variants.
* **Two-Stage Architecture:** Detect with YOLO, then use a strong classifier like **EfficientNet** for specific size distinctions.
* **Data Collection:** Collect additional training images for confusing classes (NNS Box and 650g variant).

## 💻 Compute Environment
* **Platform:** Kaggle
* **GPU:** NVIDIA Tesla T4 (16 GB VRAM)
* **CUDA Support:** Enabled

---
**Thank You**
*Hussein Noweto*
