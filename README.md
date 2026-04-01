# Assignment 3 — Part 2: Prompting Large Language Models

**Course:** Analysing Data — Week 7  
**Program:** Master in Digital Humanities, University of Groningen  

---

## Overview

This project uses the [Ollama](https://ollama.com/) Python library to classify song lyrics by genre using an open-source Large Language Model (`llama3.2:3b`). Two prompting strategies are implemented and compared:

- **Zero-shot prompting** — the model classifies lyrics with no examples, only a genre list
- **Few-shot prompting** — the model is given one example per genre before classifying

Performance is evaluated using **Precision**, **Recall**, and **F1 Score** per genre.

---

## Dataset

The dataset (`genreLyrics_train.csv` and `genreLyrics_test.csv`) contains song lyrics labeled with one of 11 genres:

`Country`, `Electronic`, `Folk`, `Hip-Hop`, `Indie`, `Jazz`, `Metal`, `Other`, `Pop`, `R&B`, `Rock`

The files are tab-separated (TSV) with multiline lyrics fields. A custom parser is used to load them correctly.

> **Note:** The dataset files are included in the `data/` folder of this repository.

---

## Repository Structure

```
A3/Part2/
├── data/
│   ├── genreLyrics_train.csv   # Training set (not included)
│   └── genreLyrics_test.csv    # Test set (not included)
├── notebook/
│   └── Part2.ipynb             # Main notebook
├── results/
│   ├── classification_results.png   # F1 bar chart + Precision/Recall scatter
│   ├── metrics_per_genre.csv        # Per-genre P/R/F1 for both strategies
│   └── predictions.csv             # All predictions vs. true labels
└── README.md
```

---

## Requirements

See `requirements.txt`. Install with:

```bash
pip install -r requirements.txt
```

You also need **Ollama** installed and running locally:

1. Download from [ollama.com](https://ollama.com)
2. Pull the model: `ollama pull llama3.2:3b`
3. Start the server: `ollama serve`

---

## How to Run

1. Clone the repository and navigate to the `Part2` folder
2. Install dependencies: `pip install -r requirements.txt`
3. Make sure Ollama is running with `llama3.2:3b` available
4. Open `notebook/Part2.ipynb` and run all cells in order

> **Hardware note:** The notebook was developed on a machine with 8 GB RAM using `llama3.2:3b`. Running the full classification (110 songs × 2 strategies) takes approximately 30–40 minutes on this hardware.

---

## Results Summary

| Strategy | Weighted F1 |
|----------|-------------|
| Zero-shot | 0.18 |
| Few-shot | 0.19 |

Both strategies showed low overall performance, with `Hip-Hop` as the best-performing genre (F1 = 0.56 few-shot) and `Jazz`, `R&B`, `Electronic`, and `Folk` scoring F1 = 0.00 in at least one condition. See `results/` for full metrics and visualizations.

---

## Notes

- Lyrics are truncated to the first 500 characters per song to reduce inference time
- A stratified sample of 10 songs per genre (110 total) is used from the test set
- The `clean_prediction()` function handles edge cases where the model returns punctuation alongside the genre name
