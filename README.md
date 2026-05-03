# 📜 Hybrid OCR Pipeline for Early Modern Spanish Manuscripts

A multi-stage OCR pipeline for transcribing **16th–19th century handwritten Spanish manuscripts**, combining a fine-tuned **Kraken HTR model** with a **vision-language model (GLM-4V)** for ensemble recognition and two-pass linguistic refinement.

Built as part of the [RenAIssance](https://www.renaissance.kul.be/) project for historical document digitization.

---

## 🧠 Pipeline Overview

```
Input (PDF / Image)
      ↓
Image Preprocessing
  - Resize (max 2000px), denoise, contrast enhancement
      ↓
Line Segmentation
  - Horizontal projection profiling + peak detection
  - Deskewing per line
      ↓
Ensemble OCR (per line)
  ┌─────────────────────────┐
  │  Kraken HTR (fine-tuned)│ ──┐
  └─────────────────────────┘   ├──► GLM-4 Merge
  ┌─────────────────────────┐   │    (word-level fusion)
  │  GLM-4V Vision          │ ──┘
  └─────────────────────────┘
      ↓
Paragraph-Level Refinement (Pass 2)
  - GLM-4 corrects cross-line errors, name inconsistencies,
    split words, and OCR noise using full-page context
      ↓
Output: RAW + REFINED transcription (.txt per page)
```

---

## 🔧 Key Components

### 1. Line Segmentation
- Horizontal projection profiling with smoothed peak detection
- Configurable `distance` and `prominence` thresholds
- Merges overlapping boundaries; filters lines shorter than 30px
- Per-line deskewing using `cv2.minAreaRect`

### 2. Kraken HTR
- Fine-tuned on the **IEHHR dataset** (Iberian Early Handwritten Records)
- Trained model: `finetuned_model_12.mlmodel`
- Preprocessing: Otsu binarization → Kraken `nlbin` → `pageseg` segmentation → `rpred` recognition

### 3. GLM-4V Vision (Auxiliary)
- Sends each line image as base64 to ZhipuAI's `glm-4v` model
- Context-aware prompt: carries previous line as context for continuity
- Handles cases where Kraken fails (segmentation mismatch, degraded ink)
- Marks uncertain characters with `[?]`

### 4. Ensemble Merge
- Both outputs sent to `glm-4` for word-level fusion
- Rules: prefer the more plausible reading, fix obvious OCR errors, preserve archaic spelling

### 5. Paragraph Refinement (Pass 2)
- After all lines are processed, the full page text is sent to `glm-4`
- Fixes: cross-line context errors, inconsistent name spelling, split words across lines
- Preserves all historical orthography — no modernization

---

## 📄 Datasets Processed

| Document | Period | Pages Processed |
|---|---|---|
| AHPG-GPAH 1:1716,A.35 – 1744 | 18th century | 2 |
| AHPG-GPAH AU61:2 – 1606 | 17th century | 2 |
| ES.28079.AHN::INQUISICIÓN,1667,Exp.12 – 1640 | 17th century | 2 |
| Pleito entre el Marqués de Viana | 17th century | 2 |
| PT3279:146:342 – 1857 | 19th century | 2 |

All documents sourced from the **IEHHR dataset** of Iberian historical handwritten records.

---

## 📝 Sample Output

**Raw OCR (Page 1, Doc 1):**
```
Informacion de la uacan lalaguay yamana de Oaxaca
Cuando conocel Senor Alcalde dentro de su casa abridan
Ayudantes & Muguerza y Baca despachadas en la Casa
Absolución de conformidad con =
Anno Domini pauso Comonavixi uox; &dio qui soy hue Lenmo d'domingo diliuauiza yB
Bautizo de Juanarecuita y Margarita d'Aurarcau
```

**After paragraph refinement**, cross-line errors, name inconsistencies, and split words are corrected while preserving historical spelling.

---

## ⚠️ Known Limitations

- Kraken's segmentation type mismatch (`baselines` vs `bbox`) degrades recognition on some pages — a known issue with the fine-tuned model's segmentation head
- Line merging occasionally groups multiple lines into a single boundary on dense pages
- GLM-4V hallucinates on very degraded or low-contrast line crops (marked with `[?]`)
- Pipeline is sequential and slow due to API rate limiting (0.5s sleep per line)

---

## 🛠️ Tech Stack

| Category | Tools |
|---|---|
| HTR Model | Kraken (fine-tuned on IEHHR) |
| Vision-Language | ZhipuAI GLM-4V, GLM-4 |
| Image Processing | OpenCV, Pillow, SciPy |
| PDF Handling | pdf2image |
| Evaluation | editdistance (CER/WER) |

---

## 🚀 Getting Started

```bash
pip install kraken editdistance zhipuai pdf2image opencv-python pillow scipy matplotlib
```

You will need:
- A **ZhipuAI API key** (for GLM-4V and GLM-4)
- The fine-tuned **Kraken model** (`.mlmodel` file) — available in the `Kraken_model/` folder
- **Poppler** installed for PDF processing

Update the paths in Cell 2 of the notebook to point to your data and model directories, then run all cells.

---

## 📂 Repository Structure

```
renaissance-ocr/
│
├── Kraken_model/              # Fine-tuned Kraken .mlmodel file
├── sample_outputs/            # Example transcription outputs
├── notebook.ipynb             # Full pipeline notebook
├── notebook.pdf               # PDF export with outputs
├── requirements.txt           # Dependencies
└── README.md
```

---

## 🏷️ Topics

`ocr` `historical-documents` `handwritten-text-recognition` `kraken` `nlp` `spanish` `vision-language-model` `document-digitization` `manuscript` `pytorch`
