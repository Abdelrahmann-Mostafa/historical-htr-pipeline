# Hybrid OCR Pipeline for Early Modern Spanish Manuscripts

A hybrid OCR pipeline combining Kraken OCR with GLM Vision for transcribing 16th–18th century handwritten Spanish documents.

## Pipeline Overview

1. **Image Preprocessing** — Resizing, denoising, contrast enhancement
2. **Line Segmentation** — Peak-based projection profile detection
3. **OCR Ensemble** — Kraken (local) + GLM Vision (API)
4. **Output Refinement** — Paragraph-level correction with GLM

## Sample Output
Cuando conocel Senor Alcalde dentro de su casa abridan
Bautizo de Juanarecuita y Margarita d'Aurarcau
Informacion de la uacan lalaguay yamana de Oaxaca


## Requirements
pillow
numpy
opencv-python
kraken
editdistance
zhipuai
pdf2image
transformers
matplotlib
scipy


## Usage

1. Install dependencies: `pip install -r requirements.txt`
2. Set your ZhipuAI API key in the notebook
3. Run all cells in order

## Limitations

- Handwriting degradation affects accuracy
- GLM API calls add latency
- Some line segmentation issues with tightly spaced text

## License

MIT
