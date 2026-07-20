# OCR-Based Question Paper Topic Classification

## Overview

This project extracts text from scanned PDF question papers using Optical Character Recognition (OCR) and automatically classifies each question into its corresponding subject topic using Sentence Transformers and semantic similarity.

The pipeline is designed to work with scanned examination papers where the text is not machine-readable. It performs OCR, cleans the extracted text, detects the subject, extracts individual questions, generates embeddings, and predicts the most relevant topic for each question.

---

# Project Workflow

```
Scanned PDF
      │
      ▼
OCR Extraction (PyMuPDF + Tesseract)
      │
      ▼
Text Cleaning
      │
      ▼
Subject Detection
      │
      ▼
Question Extraction
      │
      ▼
Topic Selection
      │
      ▼
Sentence Embedding Generation
      │
      ▼
Cosine Similarity
      │
      ▼
Topic Classification
      │
      ▼
Classification Results
```

---

# Tech Stack

* Python
* PyMuPDF (fitz)
* Tesseract OCR
* Pillow
* Sentence Transformers
* Scikit-learn
* Regular Expressions (Regex)

---

# Pipeline Steps

## Step 1 – OCR Text Extraction

### Objective

Extract text from scanned PDF pages.

### Implementation

* Open the PDF using **PyMuPDF**.
* Render each page at high resolution (4× scaling).
* Convert the rendered page into an image.
* Perform OCR using Tesseract.
* If OCR quality is poor, retry using a different Page Segmentation Mode (PSM).
* Combine extracted text from all pages.

### Output

Raw OCR text from the complete PDF.

---

## Step 2 – Text Cleaning

### Objective

Improve OCR output quality before processing.

### Operations Performed

* Normalize quotation marks.
* Remove extra spaces.
* Remove unnecessary blank lines.
* Remove page numbers.
* Remove exam set identifiers.
* Standardize formatting.

### Output

Cleaned text ready for analysis.

---

## Step 3 – Subject Detection

### Objective

Automatically determine whether the question paper belongs to:

* Mathematics
* English

### Method

Keyword-based scoring.

Example keywords:

**English**

* Reading Comprehension
* Grammar
* Literature
* Notice
* Letter Writing

**Mathematics**

* Quadratic
* Probability
* Triangle
* Trigonometry
* Coordinate Geometry

The subject with the higher keyword score is selected.

### Output

```
Detected Subject: Math
```

or

```
Detected Subject: English
```

---

## Step 4 – Question Extraction

### Objective

Extract each question separately.

### Method

Regex-based parsing to identify:

* Question number
* Question text
* Marks (when available)

Example output:

```python
{
    "question_number": "3.",
    "question": "Solve the quadratic equation...",
    "marks": "4 Marks"
}
```

---

## Step 5 – Load Sentence Transformer

### Objective

Load a pre-trained embedding model for semantic similarity.

### Model Used

```
all-MiniLM-L6-v2
```

This model converts text into dense vector embeddings suitable for semantic comparison.

---

## Step 6 – Topic Selection

### Objective

Choose the predefined topic list based on the detected subject.

### English Topics

* Reading Comprehension
* Grammar
* Literature
* Poetry
* Notice Writing
* Letter Writing
* Article Writing
* Report Writing
* Character Sketch
* Long Answer
* Short Answer

### Mathematics Topics

* Real Numbers
* Polynomials
* Quadratic Equations
* Triangles
* Coordinate Geometry
* Circles
* Trigonometry
* Statistics
* Probability
* Surface Area and Volume
* Case Study
* Assertion Reason

---

## Step 7 – Generate Topic Embeddings

### Objective

Generate vector embeddings for every predefined topic.

### Process

The Sentence Transformer converts each topic into a semantic embedding.

These embeddings are generated only once and reused for all questions.

---

## Step 8 – Compute Cosine Similarity

### Objective

Compare every extracted question with all predefined topics.

### Process

* Generate embeddings for each question.
* Compute cosine similarity between:

  * Question embedding
  * Topic embeddings

The similarity score represents how closely a question matches a topic.

---

## Step 9 – Topic Classification

### Objective

Assign the most relevant topic to every extracted question.

### Process

* Sort similarity scores.
* Select the topic with the highest similarity.
* Store:

  * Predicted topic
  * Confidence score
  * Top 3 predicted topics

Example output:

```
Question: 5

Predicted Topic:
Quadratic Equations

Confidence:
0.892

Top 3 Predictions

Quadratic Equations → 0.892
Polynomials → 0.741
Real Numbers → 0.632
```

---

# Output

The pipeline generates classification results for every extracted question, including:

* Question Number
* Question Text
* Predicted Topic
* Confidence Score
* Top 3 Matching Topics

---

# Project Structure

```
project/
│
├── notebooks/
│   └── OCR_Classification.ipynb
│
├── sample_pdfs/
│   ├── Math.pdf
│   └── English.pdf
│
├── outputs/
│   └── classification_results.csv
│
├── requirements.txt
│
└── README.md
```

---

# Future Improvements

* Fine-tune a transformer model on labeled examination datasets.
* Support additional subjects such as Science and Social Studies.
* Improve OCR preprocessing using image enhancement techniques.
* Export results in JSON, CSV, and Excel formats.
* Build a web interface using Streamlit or FastAPI.
* Incorporate confidence thresholding to flag low-confidence predictions for manual review.

---

# Author

**Jaiwin Chaudhari**

AI / Machine Learning | NLP | OCR | Semantic Search | Sentence Transformers
