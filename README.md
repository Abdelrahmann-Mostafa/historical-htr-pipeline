# 🚀 Hybrid OCR Pipeline for Early Modern Spanish Manuscripts

## 📖 Overview

This project presents a hybrid OCR pipeline designed for **16th–18th century handwritten Spanish documents**, a domain where standard OCR systems struggle due to historical spelling variation and manuscript degradation.

The system combines:

- **Kraken OCR** (primary recognizer for historical text, using a model trained on the IEHHR dataset)
- **Vision-language model (GLM)** as auxiliary support for difficult or ambiguous regions

---

## ⚙️ Pipeline

- Image preprocessing (resizing and normalization)  
- Line segmentation (heuristic-based)  
- Text recognition using Kraken OCR  
- Auxiliary recognition using GLM Vision  
- Merging and normalization of outputs  

---

## 📝 Sample Output
Cuando conocel Senor Alcalde dentro de su casa abridan

Bautizo de Juanarecuita y Margarita d'Aurarcau

Informacion de la uacan lalaguay yamana de Oaxaca


---

## ⚠️ Challenges

- Variability in historical Spanish orthography  
- Degraded and noisy manuscript images  
- Line segmentation inconsistencies  
- Noise introduced by auxiliary vision-language outputs  

---

## 🔧 Future Improvements

- Integrate Kraken baseline segmentation for improved accuracy  
- Fine-tune OCR models on historical Spanish datasets  
- Replace vision model with multilingual text-based correction (e.g., BLOOMZ)  
- Introduce evaluation metrics (CER/WER)  

---

## 📂 Contents

- Jupyter notebook with full pipeline  
- PDF export with outputs  
- Sample processed data  

---

## 🎯 Goal

This project demonstrates the feasibility of combining specialized OCR systems with modern AI models for **historical document transcription**, while highlighting key challenges and directions for improvement.
