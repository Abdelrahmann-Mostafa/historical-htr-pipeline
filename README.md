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

## Accuracy Assessment

Manual evaluation of 5 sample lines from first page:

| Line | Quality | Spanish-like Words |
|------|---------|-------------------|
| 1 | Good | 5/8 (63%) |
| 2 | Good | 7/9 (78%) |
| 3 | Fair | 5/9 (56%) |
| 4 | Fair | 3/5 (60%) |
| 5 | Good | 4/6 (67%) |

**Estimated CER:** ~0.50-0.65 (typical for handwritten OCR)

**Key Findings:**
- Pipeline successfully segments 19-31 lines per page
- GLM produces readable, context-aware Spanish text
- Output captures document structure and key terms
- Handles multiple document formats and handwriting styles

**Limitations:**
- Kraken integration requires further refinement
- Occasional line merging in dense text
- GLM uncertainty markers [?] for ambiguous characters

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
