# Hybrid OCR Pipeline for Early Modern Spanish Manuscripts

## Overview
This project implements a hybrid OCR pipeline for 16th-18th century handwritten Spanish documents.
It combines a Kraken model trained on the IEHHR dataset, with a vision-language model to improve recognition of degraded and ambiguous text.

The submission is focused on a clear, reproducible notebook-based workflow for GSoC evaluation.

## Pipeline
- Load manuscript PDF pages and convert each page to grayscale images.
- Preprocess page images (contrast normalization and resizing for stable OCR).
- Segment each page into candidate text lines using projection-based line segmentation.
- Run OCR ensemble per line:
  - Kraken OCR recognition on the segmented line.
  - GLM Vision transcription on the same line image.
  - Merge both outputs with a reconciliation prompt.
- Apply context-aware post-correction:
  - Line-level language refinement.
  - Paragraph-level second-pass refinement for coherence.
- Export per-page transcriptions and aggregate document output.
- Compute evaluation metrics (CER/WER) when ground-truth transcription is available.

## Sample Output
Curated high-confidence examples are provided in `sample_outputs/good_lines.txt`.

Example lines:
- En la villa de Muguruza y Baca.
- Ante mi, escribano publico del numero.
- En testimonio de verdad lo firme.
- Dada en esta villa, a veinte y dos dias.

## Challenges
- Historical spelling variation.
- Degraded manuscript quality.
- Line segmentation inconsistencies.

## Future Work
- Integrate Kraken baseline-aware segmentation more robustly.
- Fine-tune OCR on additional historical Spanish datasets.
- Replace/add a multilingual text-correction LLM (e.g., BLOOMZ) for post-correction benchmarking.
- Extend and standardize CER/WER evaluation across all processed pages.
- Add layout-aware VLM region detection for marginalia exclusion.

## Repository Layout
```
renaissance-ocr/
|-- README.md
|-- notebook.ipynb
|-- notebook.pdf
|-- sample_outputs/
|   `-- good_lines.txt
`-- requirements.txt
```

## Note on `notebook.pdf`
`notebook.pdf` is included as a placeholder file in this commit. Export the final notebook manually from Jupyter:
`File -> Export as PDF`.
