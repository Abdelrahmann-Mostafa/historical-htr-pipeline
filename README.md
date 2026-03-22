# Historical HTR Pipeline

An OCR pipeline for 16th-18th century Catalan and Spanish 
handwritten documents, built for the RenAIssance GSoC project.

## Approach
VLM-integrated pipeline using Kraken (fine-tuned) as OCR backbone
with Gemini Vision for multi-stage recognition and correction.

## Dataset
IEHHR Dataset — 968 training records, 253 test records
Catalan parish registers and notarial documents

## Results

| Model | CER | Notes |
|-------|-----|-------|
| Tesseract (cat) | 68.7% | Baseline 1 |
| Kraken McCATMuS | 54.2% | Baseline 2 |
| Kraken fine-tuned | TBD | In progress |
| Full VLM pipeline | TBD | Planned |

## Pipeline Stages
1. Image preprocessing (Sauvola binarisation, deskew)
2. Line segmentation (pre-segmented in IEHHR)
3. Kraken HTR (fine-tuned on IEHHR training set)
4. Gemini Vision reconciliation with semantic category labels
5. Historical linguistic correction (u/v, abbreviations)

## Setup
```bash
pip install -r requirements.txt
```

## Usage
See `notebooks/First.ipynb` for the full pipeline.
